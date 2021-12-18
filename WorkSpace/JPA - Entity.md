# JPA - Entity

## Entity

Uma entity é um objeto leve de domínio persistente utilizado para representar uma tabela da base de dados, sendo que cada instância da entity corresponde a uma linha da tabela. A entity é baseada em um simples [POJO](http://www.universidadejava.com.br/java/java-pojo/), ou seja, uma classe Java comum, com anotações para fornecer informações mais especifica para o gerenciador das entidades - [EntityManager](http://www.universidadejava.com.br/javaee/jpa-entitymanager/).

Exemplo de entity que representa uma tabela de PRODUTO:

```
package br.universidadejava.jpa.exemplo;

import java.io.Serializable;
import java.util.Date;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.Temporal;
import javax.persistence.TemporalType;

@Entity
public class Produto implements Serializable {
  private static final long serialVersionUID = 4185059514364687794L;

  @Id
  @GeneratedValue(strategy = GenerationType.AUTO)
  private Long id;
  private String descricao;
  @Temporal(TemporalType.DATE)
  private Date dataValidade;
  private Double peso;

  public Date getDataValidade() {
    return dataValidade;
  }
  public void setDataValidade(Date dataValidade) {
    this.dataValidade = dataValidade;
  }
  public String getDescricao() {
    return descricao;
  }
  public void setDescricao(String descricao) {
    this.descricao = descricao;
  }
  public Long getId() {
    return id;
  }
  public void setId(Long id) {
    this.id = id;
  }
  public Double getPeso() {
    return peso;
  }
  public void setPeso(Double peso) {
    this.peso = peso;
  }
}
```

Neste exemplo utilizamos a anotação **javax.persistence.Entity** para definir está classe como uma entidade do banco de dados. Utilizamos a anotação **javax.persistence.Id** para definir que o atributo **Long id** é chave primaria da tabela PRODUTO, também definimos que o atributo **Long id** tem seu valor gerado automaticamente através da anotação **javax.persistence.GeneratedValue**. Quando temos atributos que representam datas ou tempos precisamos adicionar a anotação **javax.persistence.Temporal** para definir que o atributo **Date dataValidade** é um campo **Date** no banco de dados.

As anotações do JPA seguem um padrão para fazer o mapeamento objeto / relacional, a anotação @Entity usa o nome da classe Java para associar com a tabela do banco de dados, por padrão todos os atributos da classe representam colunas da tabela e se seus nomes são iguais o mapeamento é feito automaticamente. Dessa forma quando você solicitar ao JPA para salvar, consultar, alterar, excluir uma entity e outros, ele irá criar o script SQL para executar a operação no banco de dados.

> O nome das tabelas no banco de dados não são case sensitive quando o banco de dados é instalado no Windows, no caso do Linux já são case sensitive.

## Anotações para Entity

As classes Entity podem ser melhor especificadas quando adicionamos algumas anotações, estas anotações podem informar ao JPA que você por exemplo não segue o padrão de nome de tabela igual ao nome da classe Java, que sua tabela no banco tem um relacionamento de Um-Para-Muitos com outra tabela, que a tabela utiliza um gerador de ID do tipo [SEQUENCE](http://www.universidadejava.com.br/javaee/jpa-sequence/) para definir o número da chave primaria e outras informações.

Obrigatoriamente toda entity do JPA precisa ter pelo menos as anotações **javax.persistence.Entity** que informa que é uma tabela do banco de dados e **javax.persistence.Id** que informa qual o atributo é chave primaria da tabela.

Segue algumas anotações básicas utilizadas no mapeamento do JPA:

### javax.persistence.Entity

Usado para definir que uma classe é uma Entity, por padrão quando o nome da Entity é igual ao nome da tabela o mapeamento é feito automaticamente pelo JPA.

| Propriedade | Descrição                                                    |
| :---------- | :----------------------------------------------------------- |
| name        | Informa o nome da Entity, por padrão o nome da entity é nome da classe. Este nome é utilizado para referenciar a entity na consulta. |

Exemplo:

Criar uma Entity que está mapeada para a tabela PRODUTO.

```
@Entity
public class Produto {
  ...
}
```

### javax.persistence.Table

Define o nome da tabela no banco de dados.

| Propriedade       | Descrição                                                |
| :---------------- | :------------------------------------------------------- |
| catalog           | O catalogo da tabela.                                    |
| name              | O nome da tabela.                                        |
| schema            | O esquema da tabela.                                     |
| uniqueConstraints | Regras / Restrições que podem ser adicionadas na tabela. |

Exemplo:

Criar uma Entity que está mapeada para a tabela PRJ_PDT que armazena os dados de um produto. Se criarmos uma classe chamada Prj_Pdt para mapear esta tabela, não estaremos seguindo as boas praticas de nomeação de classes e por padrão a anotação @Entity irá fazer o mapeamento através do nome da classe. Para especificar manualmente qual a tabela deve ser mapeada com a Entity, podemos usar a anotação @Table e especificar a propriedade name.

```
@Entity
@Table(name="PRJ_PDT")
public class Produto {
  ...
}
```

### javax.persistence.Id

Informa o atributo da Entity que representa a chave primaria.

Exemplo:

Criar um atributo para mapear a coluna ID da tabela PRODUTO que representa a chave primaria desta tabela. O tipo do atributo pode ser definido de acordo com o tipo da coluna da tabela.

```
@Entity
public class Produto {

  @Id
  private Long id;

  ...
}
```

### javax.persistence.Column

Informa as configurações de coluna da tabela, por padrão quando o nome do atributo da Entity é igual ao nome da coluna da tabela, o relacionamento é feito automaticamente pelo JPA.

| Propriedade      | Descrição                                                    |
| :--------------- | :----------------------------------------------------------- |
| columnDefinition | Definição do tipo da coluna.                                 |
| insertable       | Informa se a tabela deve ser incluída no SQL de insert, por padrão é true. |
| length           | Tamanho da coluna, por padrão é 255.                         |
| name             | Nome da tabela que contém está coluna, se não for informado a coluna assume o nome da tabela da entity. |
| nullable         | Informa se o valor pode ser null.                            |
| precision        | Precisão da coluna decimal.                                  |
| scale            | Número de casas decimais, usado somente em coluna com número decimal. |
| table            | Nome da tabela que contém está coluna, se não for informado assume o nome da tabela da entity. |
| unique           | Informa se a coluna é chave única.                           |
| updatable        | Informa se a coluna deve ser incluída no SQL de update, por padrão é true. |

Exemplo:

Criar um atributo para representar a coluna DATA_VALIDADE da tabela PRODUTO, sendo que o tipo desta coluna no banco de dados representa uma data. Se criarmos um atributo com o nome data_validade não estaremos seguindo as boas práticas de nomes de atributos e por padrão os nomes dos atributos representam os nomes das colunas da tabela. Para especificar manualmente qual o nome da coluna, podemos usar a anotação @Column e especificar a propriedade name.

```
@Entity
public class Produto {

  @Column(name="DATA_VALIDADE")
  @Temporal(value=TemporalType.DATE)
  private Date dataValidade;

}
```

### javax.persistence.SequenceGenerator

Utilizado para representar uma sequência numérica gerada através do banco de dados.

| Propriedade    | Descrição                                                    |
| :------------- | :----------------------------------------------------------- |
| name           | Nome único para o gerador que pode ser referenciado por uma ou mais classes que pode ser utilizado para gerar valores de chave primaria. |
| allocationSize | A quantidade que será incrementada na sequence, o padrão é 50. |
| initialValue   | Valor inicial da sequence.                                   |
| sequenceName   | Nome da sequence do banco de dados.                          |

Exemplo:

Criar uma Entity para representar a tabela PRODUTO e no banco de dados temos criado uma [SEQUENCE](http://www.universidadejava.com.br/javaee/jpa-sequence/) chamada SEQ_PRODUTO que deve ser usada para geração do ID da tabela.

```
@Entity
@SequenceGenerator(name="SEQ_PROD", sequenceName="SEQ_PRODUTO", initialValue=1, allocationSize=1)
public class Produto {
  ...
}
```

### javax.persistence.GeneratedValue

Define a estratégia para criar o ID, pode ser tipo AUTO (incrementa automaticamente 1, 2, 3 em sequência) ou utilizando uma SEQUENCE.

| Propriedade | Descrição                                                    |
| :---------- | :----------------------------------------------------------- |
| generator   | Nome do gerador da chave primaria que é especificado na anotação @SequenceGenerator ou @TableGenerator. |
| strategy    | Estratégia de geração de chave primaria que o serviço de persistência precisa usar para gerar a chave primaria. Seu valor pode ser obtido através da enum javax.persistence.GenerationType, os valores podem ser AUTO, IDENTITY, SEQUENCE ou TABLE. |

Exemplo:

Criar uma Entity para representar a tabela PRODUTO e usar o AUTO_INCREMENT do banco de dados para gerar o valor da coluna ID que representa chave primaria da tabela.

```
@Entity
public class Produto {

  @Id
  @GeneratedValue(strategy=GenerationType.AUTO)
  private Long id;

  ...
}
```

Criar uma Entity para representar a tabela PRODUTO e no banco de dados temos criado uma SEQUENCE chamada SEQ_PRODUTO que deve ser usada para gerar o valor da coluna ID que representa a chave primaria da tabela.

```
@Entity
@SequenceGenerator(name="SEQ_PROD", sequenceName="SEQ_PRODUTO", initialValue=1, allocationSize=1)
public class Produto {

  @Id
  @GeneratedValue(strategy=GenerationType.SEQUENCE, generator="SEQ_PROD")
  private Long id;

  ...
}
```

### javax.persistence.Temporal

Utilizado para representar campos de Data e Hora, nesta anotação podemos definir o tipo de dado DATE, TIME e TIMESTAMP.

| Propriedade | Descrição                                                    |
| :---------- | :----------------------------------------------------------- |
| value       | O tipo usado para mapear java.util.Date e java.util.Calendar. Seu valor pode ser obtido através da enum javax.persistence.TemporalType, os valores podem ser DATE, TIME e TIMESTAMP. |

Exemplo:

Criar um atributo para representar a coluna DATA_VALIDADE da tabela PRODUTO, sendo que o tipo desta coluna no banco de dados representa uma data.

```
@Entity
public class Produto {

  @Column(name="DATA_VALIDADE")
  @Temporal(value=TemporalType.DATE)
  private Date dataValidade;

}
```

Criar um atributo para representar a coluna ENTRADA da tabela VALET, sendo que o tipo desta coluna no banco de dados representa uma data e hora.

```
@Entity
public class Valet {

  @Temporal(value=TemporalType.TIMESTAMP)
  private Date entrada;

}
```

### javax.persistence.Transient

Informa que o atributo não representa uma coluna da tabela.

Criar um atributo para representar a idade do cliente e não deve ser persistido no banco de dados.

```
@Entity
public class Cliente {

  @Transient
  private Integer idade;

}
```

### Exemplo de Entity

Vamos criar uma Entity para representar a tabela USUARIO a seguir:

```
CREATE TABLE Usuario (
  id NUMBER(10) NOT NULL PRIMARY KEY,
  nome VARCHAR2(100) NOT NULL,
  dataNasc DATE NOT NULL,
  email VARCHAR2(150) NOT NULL,
  ativo NUMBER(1) NOT NULL,
  comentário VARCHAR2(200)
);
```

Criando a Entity para representar a tabela USUARIO:

```
package br.universidadejava.jpa.exemplo;

import java.io.Serializable;
import java.util.Date;
import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.Temporal;
import javax.persistence.TemporalType;

@Entity
public class Usuario implements Serializable {
  private static final long serialVersionUID = -8762515448728066246L;

  @Id
  @GeneratedValue(strategy = GenerationType.AUTO)
  private Long id;
  @Column(nullable = false)
  private String nome;
  @Temporal(TemporalType.DATE)
  @Column(name="dataNasc", nullable = false)
  private Date dataNascimento;
  @Column(nullable = false)
  private String email;
  @Column(nullable = false)
  private Boolean ativo;
  private String comentario;

  public Boolean getAtivo() {
    return ativo;
  }
  public void setAtivo(Boolean ativo) {
    this.ativo = ativo;
  }
  public String getComentario() {
    return comentario;
  }
  public void setComentario(String comentario) {
    this.comentario = comentario;
  }
  public Date getDataNascimento() {
    return dataNascimento;
  }
  public void setDataNascimento(Date dataNascimento) {
    this.dataNascimento = dataNascimento;
  }
  public String getEmail() {
    return email;
  }
  public void setEmail(String email) {
    this.email = email;
  }
  public Long getId() {
    return id;
  }
  public void setId(Long id) {
    this.id = id;
  }
  public String getNome() {
    return nome;
  }
  public void setNome(String nome) {
    this.nome = nome;
  }
}
```

Utilizamos a anotação **@Entity** para informar que a classe Usuario é uma Entity do banco de dados, definimos que a propriedade **Long id** será o ID (chave primaria) da tabela através da anotação **@Id** e também informamos que seu valor será gerado automaticamente com a anotação **@GeneratedValue**. Através da anotação **@Column** conseguimos informar quais os atributos não podem ser **null** e que o atributo **Date dataNascimento** está mapeado para a coluna **dataNasc** da tabela USUARIO.

### Conteúdos relacionados

- [Configurando o EntityManager do JPA](http://www.universidadejava.com.br/jee/jpa-entitymanager/)
- [Criando uma entidade com uma chave primaria](http://www.universidadejava.com.br/jee/jpa-chavecomposta/)
- [Exemplo de CRUD com JPA](http://www.universidadejava.com.br/jee/jpa-exemplo-crud/)
- [Criando consultas com JPA](http://www.universidadejava.com.br/jee/jpa-query/)