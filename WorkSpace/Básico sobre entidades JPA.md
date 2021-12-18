# Básico sobre entidades JPA

Last change in segunda-feira, 22 de fevereiro de 2021

### Introdução

Neste post vamos ter uma visão básica sobre entidades JPA. Veremos apenas as anotações mais utilizadas, para o mapeamento de uma simples tabela.

Para demonstração, vamos realizar o mapeamento de uma tabela do banco de dados, que armazena dados sobre os pets de uma veterinária.

![1](https://storage.googleapis.com/gasparbarancelli-blog/posts/39/1.png)

Figure 1. Diagrama da tabela PET

### Entidade

As entidades JPA são POJOS (Plain Old Java Objects), que são objetos com design simplificado, ou seja, sem nada de especial. Essas entidades representam uma tabela do banco de dados, e cada instância desse objeto representa uma linha da tabela.

Adicionado a anotação `@Entity` numa classe, deixamos o JPA ciente de que essa classe é a representação de uma tabela do banco de dados.

```java
import javax.persistence.*;

@Entity
public class Pet {

}
```

### Alterar nome da tabela

Quando o nome da tabela do banco de dados difere do nome da classe, devemos utilizar a anotação `@Table` para definir o valor correto.

```java
import javax.persistence.*;

@Entity
@Table(name = "PET")
public class Pet {

}
```

### Definindo a chave primária

Toda entidade JPA obrigatoriamente deve ter um identificador definido, que é sua chave primaria.

Assim como no banco de dados, o JPA permite definir uma chave única ou composta, mas para simplificar, neste post veremos a forma de implementação de chave única utilizando a anotação `@Id`. Como os identificadores únicos podem ser gerador de diversas maneiras, precisamos deixar o JPA ciente qual implementação é utilizada, para isso devemos adicionar a anotação `@GeneratedValue`.

```java
@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)
private Long codigo;
```

### Coluna

Para especificar a relação de uma coluna do banco com um campo da classe, utilizaremos a anotação `@Column`. Por meio desta anotação, podemos especificar o nome da coluna, se ela aceita valores nulos e também qual o seu tamanho máximo.

```java
@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)
@Column(name = "ID")
private Long codigo;

@Column(name = "NOME", nullable = false, length = 100)
private String nome;

@Column(name = "PESO")
private Double peso;

@Column(name = "NASCIMENTO", nullable = false)
private LocalDate dataNascimento;

@Column(name = "DATA_MODIFICACAO", nullable = false)
private LocalDate dataModificacao;
```

### Gravar objetos do tipo Enum

Em muitos dos casos precisamos armazenar no banco de dados objetos do tipo `Enum`. Por padrão esses objetos são armazenados numa coluna numérica, e seus valores são atribuídos de forma ordinal, seguindo a ordem de declaração dos valores do `Enum`. No entanto, podemos alterar o comportamento do JPA, fazendo com que ele armazene no banco de dados o nome dos valores do `Enum`, para isso vamos fazer uso da anotação `@Enumerated`.

```java
@Enumerated(EnumType.STRING)
@Column(name = "TIPO", nullable = false, length = 4)
private Tipo tipo;
public enum Tipo {

    CAO,
    GATO

}
```

### Campos não persistentes

O JPA entende que todos os campos da nossa classe são colunas do banco de dados, mas muitas vezes essa não deveria ser uma verdade, pois utilizamos campos para aplicarmos alguma lógica na aplicação. Para deixarmos o JPA ciente de que ele não deve persistir um determinado campo no banco de dados podemos utilizar a anotação `@Transient`.

```java
@Transient
private Integer idade;
```

### Versionamento

Imagine o seguinte cenário, onde dois usuários do sistema estão editando o cadastro de um Pet exatamente no mesmo momento. E neste cenários pode acontecer que o primeiro usuário tenha alterado a data de nascimento do Pet, enquanto o outro usuário alterou o nome do Pet. Então quando um dos usuários persistir a informação o outro na sequência vai sobrescrever o valor alterado para o valor antigo.

O ideal nesses casos é utilizar uma estratégia de versionamento dos registros. Onde o JPA irá verificar a versão do registro e só persistirá o objeto quando o código da versão for a mesma que está armazenada no banco de dados.

Voltando ao cenário anterior, agora utilizando uma estratégia de versionamento.

Assim que os dois usuários forem obter as informações do Pet do banco de dados, eles vão receber o código de versionamento do registro, esse código é um número sequencial, e para contextualizar digamos que o valor desse versionamento seja 1.

 Agora quando o primeiro usuário for persistir no banco de dados o JPA vai verificar se o código de versionamento do objeto é o mesmo que está no banco de dados, que em nosso caso ainda é 1, pois o objeto alterado ainda não foi persistido, sendo assim o JPA entende que o objeto pode ser alterado e persiste o mesmo no banco de dados, e também incrementa o valor da versão do registro para 2. 

Agora quando o usuário 2 for alterar o objeto no banco de dados, o JPA sabe que a versão do objeto alterado pelo usuário é 1 mas no banco é 2, então o JPA lança uma exceção para o usuário informando que existe um registro mais recente.

Para adicionar todo esse controle, basta o seguinte campo e anotação em nossa entidade.

```java
@Version
@Column(name = "VERSAO", nullable = false)
private Long versao;
```

### Conclusão

Neste post nos criamos uma classe que representa uma tabela em nosso banco de dados, e aprendemos apenas algumas das anotações mais utilizadas da especificação JPA.

Abaixo segue a implementação completa da nossa classe.

```java
import javax.persistence.*;
import java.time.LocalDate;

@Entity
@Table(name = "PET")
public class Pet {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "ID")
    private Long codigo;

    @Column(name = "NOME", nullable = false, length = 100)
    private String nome;

    @Column(name = "PESO")
    private Double peso;

    @Column(name = "NASCIMENTO", nullable = false)
    private LocalDate dataNascimento;

    @Transient
    private Integer idade;

    @Enumerated(EnumType.STRING)
    @Column(name = "TIPO", nullable = false, length = 4)
    private Tipo tipo;

    @Version
    @Column(name = "VERSAO", nullable = false)
    private Long versao;

}
```

#### // Share this post

![facebook sharing button](https://platform-cdn.sharethis.com/img/facebook.svg) Share

![twitter sharing button](https://platform-cdn.sharethis.com/img/twitter.svg) Tweet

![linkedin sharing button](https://platform-cdn.sharethis.com/img/linkedin.svg) Share

![whatsapp sharing button](https://platform-cdn.sharethis.com/img/whatsapp.svg) Share

![wechat sharing button](https://platform-cdn.sharethis.com/img/wechat.svg) Share

![email sharing button](https://platform-cdn.sharethis.com/img/email.svg) Email

// Related Posts[Multithreading com Java - Iniciando com Threads](https://gasparbarancelli.com/post/multithreading-com-java-iniciando-com-threads?lang=pt)[Removendo acentuações e sinais gráficos de uma String](https://gasparbarancelli.com/post/removendo-acentuacoes-e-sinais-graficos-de-uma-string?lang=pt)[Construindo imagem Docker em projetos Java](https://gasparbarancelli.com/post/construindo-imagem-docker-em-projetos-java?lang=pt)[Spring Boot com certificado SSL gerado pelo Let`s Encrypt e utilizando Nginx como servidor Web](https://gasparbarancelli.com/post/spring-boot-com-certificado-ssl-gerado-pelo-lets-encrypt-e-utilizando-nginx-como-servidor-web?lang=pt)[Melhore a usabilidade dos NullPointerExceptions gerados pela JVM](https://gasparbarancelli.com/post/melhore-a-usabilidade-dos-null-pointer-exceptions-gerados-pela-JVM?lang=pt)