# Tutorial definitivo: Tudo o que você precisa para começar bem com JPA

[![img](https://secure.gravatar.com/avatar/98def809dd1ff0d88623373ddc725475?s=40&d=mm&r=g)](https://blog.algaworks.com/author/alexandre-afonso/)[Escrito por **Alexandre Afonso**](https://blog.algaworks.com/author/alexandre-afonso/)em 02/07/2019

![img](https://algaworks-blog.s3.amazonaws.com/wp-content/uploads/AR190613A-Tutorial-JPA-840x440-Tiny.png)

Você ouve falar de JPA em vários lugares e quer entender detalhes de como ele funciona?

É grande o número de aplicações Java que utilizam JPA, por isso seria bem desconfortável não dominar esse assunto tão essencial para quem deseja programar com Java para o mundo corporativo.

Caso esse seja o seu caso, que bom que está aqui, pois é o melhor lugar para começar a aprender sobre o assunto.

## Antes trabalhoso, agora bem mais fácil

Criar uma aplicação Java sem um framework que ajude a lidar com o banco de dados é bem trabalhoso.

É necessário usar a JDBC diretamente e escrever bastante SQL.

Com isso em mente, alguns bons anos atrás, desenvolvedores começaram a criar algo que agilizasse esse trabalho.

Nascia então o Hibernate. Um framework objeto relacional que simplificava a interação entre a aplicação e o banco de dados.

O Hibernate é um framework objeto relacional porque ajuda a representar tabelas de um banco de dados relacional através de classes.

A vantagem dessa estratégia é a de automatizar as tarefas com banco de dados de forma que é possível simplificar o código da aplicação.

Ele consegue gerar, em tempo de execução, o SQL necessário para interagir com o banco de dados.

Para cada banco de dados (MySQL, PostgreSQL, Oracle, etc), ele tem um dialeto diferente que pode ser configurado de acordo com a necessidade da aplicação.

Isso significa também que podemos trocar o banco de dados utilizado sem ter que alterar o código-fonte. Podemos fazer somente com configuração.

Ganhamos em produtividade!

A ideia de **mapeamento objeto relacional** do Hibernate deu tão certo que ele serviu como a principal inspiração para criação de uma especificação Java para persistência.

Nascia a especificação JPA. Também conhecida como *Java Persistence API*.

## O que é JPA?

JPA (ou *Java Persistence API*) é uma especificação oficial que descreve como deve ser o comportamento dos frameworks de persistência Java que desejarem implementá-la.

Ser uma especificação significa que a JPA não possui código que possa ser executado.

Por analogia, você pode pensar na especificação JPA como uma interface que possui algumas assinaturas, mas que precisa que alguém a implemente.

Apesar de não ter nada executável, a especificação possui algumas classes, interfaces e anotações que ajudam o desenvolvedor a abstrair o código.

São artefatos do pacote *javax.persistence* que ajudam a manter o código independente das implementações da especificação.

Assim não precisamos importar classes de terceiros em nosso código.

Implementação é quem dá vida para a especificação. É o código que podemos executar, que chamamos de framework.

Enfim, para persistir dados com JPA, é preciso escolher uma implementação que é quem, de fato, vai fazer todo o trabalho.

## Implementações da especificação JPA

A implementação é algo que pode ser executado em nossa aplicação.

Qualquer pessoa ou equipe pode escrever sua própria implementação da especificação JPA.

Dentre as mais famosas temos o **OpenJPA** da Apache, o **Hibernate** da Red Hat e o **EclipseLink** da Eclipse Foundation.

As duas mais importantes são o Hibernate, por ainda ser a mais utilizada, e o EclipseLink, por ser a implementação de referência.

Ser a referência quer dizer que ela será a primeira a validar as mudanças e novas ideias que surgirem na especificação.

É um papel importante para mostrar para a comunidade que a especificação é possível e viável.

A grande ideia da especificação JPA é que a aplicação possa trocar de implementação sem que precise de mudanças no código. Apenas um pouco de configuração.

Uma mesma aplicação que poderia estar rodando no GlassFish com EclipseLink poderia também funcionar no WildFly com Hibernate.

## Mapeamento Objeto Relacional

**Mapeamento Objeto Relacional** é a representação de uma tabela de um banco de dados relacional através de classes Java.

É também conhecido como **ORM** ou *Object Relational Mapping*.

Repare como isso começa a ser possível:

| Banco de dados | Linguagem Orientada a Objetos |
| :------------- | :---------------------------- |
| Tabela         | Classe                        |
| Coluna         | Atributo                      |
| Registro       | Objeto                        |

Enquanto que no banco de dados temos tabelas, colunas e registros, em uma linguagem orientada a objetos, como o Java, temos o equivalente com classes, atributos e objetos.

Essa equivalência já é boa parte do caminho, mas não o suficiente para automatizar todo o trabalho.

Para completar, temos as anotações que adicionarão metadados às classes e permitirão os frameworks ORM, como Hibernate ou EclipseLink, entrarem em ação.

Algumas muito usadas são:

- `@Entity`
- `@Table`
- `@Id`
- `@Column`

## Por que utilizar JPA?

Escrever SQL para consultas, inserções e atualizações, por mais que não seja complexo, é uma tarefa repetitiva e chata.

A primeira vantagem percebida é que já ganhamos isso pronto. O SQL para as operações é gerado em tempo de execução.

Uma segunda coisa que logo reparamos é na simplificação do código-fonte da aplicação. Como muita coisa é automatizada, bem menos código é escrito para as mesmas funções.

Em aplicações desenvolvidas sem ORM, trocar de banco de dados pode ser bem complexo, pois exigiria verificação e correção de todo o SQL da aplicação.

Com JPA, se houver a necessidade de alterar o sistema de banco de dados, basta trocar o dialeto que um novo e compatível SQL é gerado em tempo de execução.

Interessante notar também que o uso do JPA permite que a implementação da especificação seja alterada sem impactos no fonte da aplicação.

Isso tudo permite ter uma manutenabilidade excelente em nossa aplicação, facilitando o dia a dia do desenvolvedor.

## Primeiros passos com JPA

Crie um novo projeto com o Maven e adicione as seguintes a dependências:

```
<``dependency``>``  ``<``groupId``>org.hibernate</``groupId``>``  ``<``artifactId``>hibernate-core</``artifactId``>``  ``<``version``>5.4.2.Final</``version``>``</``dependency``>` `<``dependency``>``  ``<``groupId``>mysql</``groupId``>``  ``<``artifactId``>mysql-connector-java</``artifactId``>``  ``<``version``>8.0.16</``version``>``</``dependency``>
```

A primeira é a dependência do Hibernate. Ele será a implementação usada no exemplo de agora.

Somente a dependência acima já adiciona tanto as classes do Hibernate quanto as anotações do pacote *javax.persistence*, de que precisaremos.

A segunda é a dependência que contém o driver do MySQL. Apesar de não usarmos o JDBC diretamente, o Hibernate vai. Por isso precisamos do driver.

Dessa forma já estamos prontos para começar a escrever nossa pequena aplicação de exemplo.

Vamos simular um pequeno cadastro de clientes.

## Configurações de persistência

A primeira coisa que precisamos é informar os dados de conexão com o banco de dados.

Fazemos isso no arquivo *persistence.xml*. Ele deve ficar dentro da pasta *META-INF*. Para isso, crie-o em *src/main/resources/META-INF*.

Ele é o arquivo em que fazemos as configurações gerais de persistência.

Vamos começar com a estrutura básica dele:

```
<?``xml` `version``=``"1.0"` `encoding``=``"UTF-8"``?>``<``persistence` `xmlns``=``"http://xmlns.jcp.org/xml/ns/persistence"``  ``xmlns:xsi``=``"http://www.w3.org/2001/XMLSchema-instance"``  ``xsi:schemaLocation``=``"http://xmlns.jcp.org/xml/ns/persistence http://xmlns.jcp.org/xml/ns/persistence/persistence_2_2.xsd"``  ``version``=``"2.2"``>``</``persistence``>
```

Agora vamos configurar a unidade persistência que chamaremos de “Clientes-PU”.

A tag da unidade de persistência deve ficar dentro da tag *persistence* mostrada acima.

Dentro dela ficam as propriedades como os dados de conexão e também o dialeto do banco que iremos utilizar:

```
<``persistence-unit` `name``=``"Clientes-PU"``> `` ``<``properties``>``  ``<``property` `name``=``"javax.persistence.jdbc.url"` `value``=``"jdbc:mysql://localhost/cadastrocliente"` `/>``  ``<``property` `name``=``"javax.persistence.jdbc.user"` `value``=``"root"` `/>``  ``<``property` `name``=``"javax.persistence.jdbc.password"` `value``=``"123"` `/>``  ``<``property` `name``=``"javax.persistence.jdbc.driver"` `value``=``"com.mysql.cj.jdbc.Driver"` `/>` `  ``<``property` `name``=``"hibernate.dialect"` `value``=``"org.hibernate.dialect.MySQL8Dialect"` `/>`` ``</``properties``>``</``persistence-unit``>
```

Não esqueça de ajustar os dados acima (url, usuário e senha) de acordo com o seu ambiente.

O que temos já é suficiente, mas muitas outras coisas podem ser configuradas nesse arquivo.

Para comodidade na execução dos exemplos do artigo, é legal também adicionarmos mais essas duas propriedades:

```
<``property` `name``=``"hibernate.show_sql"` `value``=``"true"` `/>``<``property` `name``=``"hibernate.format_sql"` `value``=``"true"` `/>
```

A primeira controla se o Hibernate deve exibir o SQL executado no console. A segunda é para controlar a formatação.

Em nosso caso, as duas propriedades ficaram habilitadas (repare o valor igual a *true*).

## Fazendo o mapeamento

A tabela cliente, que deve estar no banco de dados, pode ser criada como abaixo:

```
create` `table` `cliente (``  ``id ``bigint` `not` `null` `auto_increment,``  ``nome ``varchar``(60) ``not` `null``,``  ``primary` `key` `(id)``) engine=InnoDB;
```

Para já termos registros para consulta, deve ser executado também esse SQL para inserção:

```
insert` `into` `cliente (id, nome) ``values` `(1, ``'Autopeças Estrada'``);
```

Agora vamos criar uma classe que representará a tabela. Seus atributos representam as colunas:

```
public` `class` `Cliente {` ` ``private` `Integer id;`` ``private` `String nome;` ` ``// getters e setters omitidos` `}
```

Vamos adicionar o mapeamento usando as anotações do JPA:

```
@Entity``public` `class` `Cliente {`` ` ` ``@Id`` ``private` `Integer id;`` ` ` ``private` `String nome;` ` ``// getters e setters omitidos` `}
```

Esse é o mínimo necessário para que o framework funcione.

Repare que o framework vai assumir o nome da classe como sendo o nome para a tabela e o nome de cada atributo como as colunas.

Caso você precise ou queira que o nome da classe ou atributos sejam diferentes, então pode customizar com as anotações `@Table` e `@Column`:

```
@Entity``@Table``(name = ``"cliente"``)``public` `class` `Cliente {`` ` ` ``@Id`` ``@Column``(name = ``"id"``)`` ``private` `Integer id;`` ` ` ``@Column``(name = ``"nome"``)`` ``private` `String nome;` ` ``// getters e setters omitidos` `}
```

## Criando o EntityManager

Todas as operações de busca ou de alteração vão nascer com a instância da classe `EntityManager`.

Mas antes de termos acesso ao `EntityManager`, precisamos da fábrica:

```
EntityManagerFactory entityManagerFactory ``    ``= Persistence.createEntityManagerFactory(``"Clientes-PU"``);
```

Repare que o parâmetro do método `createEntityManagerFactory` é o mesmo que está no *persistence.xml*, na tag *persistence-unit*, atributo *name*.

Esse é o momento da implementação JPA (Hibernate, EclipseLink, etc) se inicializar baseada nas configurações que deixamos no *persistence.xml*.

Uma vez que a fábrica é criada, a ideia é que você guarde essa instância durante todo o tempo de vida da sua aplicação.

A partir dela, temos acesso à instância de `EntityManager`:

```
EntityManager entityManager = entityManagerFactory.createEntityManager();
```

Com essa instância, já podemos fazer operações de pesquisa e alteração no banco de dados.

Não existe uma regra para qual o momento ou quanto tempo essa instância deve ser mantida.

Como linhas gerais, você usa uma instância de `EntityManager` por *thread* ou por requisição (no caso de aplicações web).

Quando terminar de usá-la, você chama o método `close`:

```
entityManager.close();
```

Inclusive, a fábrica também possui um método `close`, que deve ser chamado quando a aplicação se encerra.

```
entityManagerFactory.close();
```

Nosso exemplo será usado um simples método `main`, pois é o suficiente para colocarmos em prática o que está sendo aprendido.

O resultado parcial é esse:

```
public` `class` `CadastroCliente {`` ``public` `static` `void` `main(String... args) {``  ``EntityManagerFactory entityManagerFactory ``    ``= Persistence.createEntityManagerFactory(``"Clientes-PU"``);``  ``EntityManager entityManager = entityManagerFactory.createEntityManager();` `  ``// Código CRUD aqui` `  ``entityManager.close();``  ``entityManagerFactory.close();`` ``}``}
```

Como só temos um método `main`, ao final dele, estamos fechando tanto o `EntityManager` quanto o `EntityManagerFactory`.

## Buscando registros

Usando JPA, para buscar um registro da base de dados, só precisamos de uma linha. Isso claro, sem contar com a criação da instância de `EntityManager`.

```
public` `static` `void` `main(String... args) {`` ``EntityManagerFactory entityManagerFactory ``    ``= Persistence.createEntityManagerFactory(``"Clientes-PU"``);`` ``EntityManager entityManager = entityManagerFactory.createEntityManager();` ` ``Cliente cliente = entityManager.find(Cliente.``class``, ``1``);`` ``System.out.println(cliente.getNome());` ` ``entityManager.close();`` ``entityManagerFactory.close();``}
```

Execute o código acima e verá que será impresso o nome do cliente de código 1 no seu console.

## Abrindo e fechando a transação

Diferente da busca e pesquisa de registros, qualquer operação de alteração do banco (*insert*, *update*, *delete*) irá precisar de uma transação.

Precisamos iniciar a transação, executar as operações necessárias e confirmar a mesma através do método `commit`.

Uma transação serve para garantirmos que operações que precisam acontecer juntas só sejam confirmadas se todas obtiverem sucesso.

No caso de uma operação der errado, todas as outras são desfeitas. Isso garante a integridade do nosso banco de dados.

Transação nada mais é que o período de tempo que usamos para alterar informações no banco de dados.

Nós mesmos vamos delimitar esse período de tempo através da chamada dos métodos `begin` e `commit`.

O código para quando vamos fazer alguma operação de alteração com o banco é esse:

```
public` `static` `void` `main(String... args) {`` ``EntityManagerFactory entityManagerFactory ``    ``= Persistence.createEntityManagerFactory(``"Clientes-PU"``);`` ``EntityManager entityManager = entityManagerFactory.createEntityManager();` ` ``entityManager.getTransaction().begin();`` ``// Código de alteração aqui`` ``entityManager.getTransaction().commit();` ` ``entityManager.close();`` ``entityManagerFactory.close();``}
```

## Inserindo registros

Agora que temos a transação, podemos chamar o método `persist` sem que erros sejam lançados.

Vamos salvar nosso primeiro registro:

```
public` `static` `void` `main(String... args) {`` ``EntityManagerFactory entityManagerFactory ``    ``= Persistence.createEntityManagerFactory(``"Clientes-PU"``);`` ``EntityManager entityManager = entityManagerFactory.createEntityManager();` ` ``Cliente cliente = ``new` `Cliente();`` ``cliente.setId(``2``);`` ``cliente.setNome(``"Armazém Feliz"``);``  ` ` ``entityManager.getTransaction().begin();`` ``entityManager.persist(cliente);`` ``entityManager.getTransaction().commit();` ` ``entityManager.close();`` ``entityManagerFactory.close();``}
```

Depois da chamada do método `commit`, o registro deve ser persistido na base para eventuais consultas.

## Deixando o MySQL gerar a chave primária

Em nosso mapeamento ainda não temos uma estratégia de geração da nossa chave primária.

Na prática foi preciso informar o identificador (equivalente a chave primária da tabela) manualmente.

Repare que o método `setId` foi chamado explicitamente no exemplo anterior.

Acontece que, no banco de dados, nossa tabela está com a opção de auto incremento habilitada.

Isso pode ser notado pelo DDL de criação da tabela que foi passado mais no início do artigo.

Para tirarmos proveito disso, basta usar a estratégia chamada *identity* em nosso identificador.

Fica assim:

```
@Entity``@Table``(name = ``"cliente"``)``public` `class` `Cliente {`` ` ` ``@Id`` ``@Column``(name = ``"id"``)`` ``@GeneratedValue``(strategy = GenerationType.IDENTITY)`` ``private` `Integer id;`` ` ` ``@Column``(name = ``"nome"``)`` ``private` `String nome;` ` ``// getters e setters omitidos``}
```

Com essa estratégia configurada, não precisamos mais informar o identificador, já que está sendo usado o recurso de *auto_increment* em nossa tabela do MySQL.

Com essa pequena alteração no mapeamento, o exemplo de inserção fica assim:

```
public` `static` `void` `main(String... args) {`` ``EntityManagerFactory entityManagerFactory ``    ``= Persistence.createEntityManagerFactory(``"Clientes-PU"``);`` ``EntityManager entityManager = entityManagerFactory.createEntityManager();` ` ``Cliente cliente = ``new` `Cliente();`` ``cliente.setNome(``"Armazém Estrela"``);``  ` ` ``entityManager.getTransaction().begin();`` ``entityManager.persist(cliente);`` ``entityManager.getTransaction().commit();` ` ``entityManager.close();`` ``entityManagerFactory.close();``}
```

Repare que agora não informamos o identificador do cliente.

Importante destacar também que o exemplo anterior, que chama o método `setId` explicitamente para inserir um novo registro, não funciona mais.

Isso porque com a estratégia *identity* na classe `Cliente`, não podemos informar o identificador para inserções.

## Removendo registros

Para remover, é tão simples quanto as operações que já fizemos até aqui. Basta uma chamada para o método `remove`.

Vamos remover o registro de identificador número 2. Ficará como abaixo:

```
public` `static` `void` `main(String... args) {`` ``EntityManagerFactory entityManagerFactory ``    ``= Persistence.createEntityManagerFactory(``"Clientes-PU"``);`` ``entityManager entityManager = entityManagerFactory.createEntityManager();``  ` ` ``Cliente cliente = entityManager.find(Cliente.``class``, ``2``);` ` ``entityManager.getTransaction().begin();`` ``entityManager.remove(cliente);`` ``entityManager.getTransaction().commit();` ` ``entityManager.close();`` ``entityManagerFactory.close();``}
```

Depois de executado o código acima, o registro de identificador 2 não deve existir mais na base de dados.

Repare que a instância de cliente que foi passada para remoção foi buscada através do próprio `EntityManager`, e isso é um detalhe importante.

O `EntityManager` só remove objetos que ele está gerenciando. Acontece que, quando usamos o método `find`, o retorno passa ser gerenciado automaticamente.

Em outras palavras, nossa instância de cliente foi removida porque, logo após o `find`, ela se tornou gerenciada pelo `EntityManager`.

Para entender isso melhor é preciso compreender mais a fundo sobre o `EntityManager`.

## Entendendo o funcionamento do EntityManager

Em uma tradução podemos chamar a classe `EntityManager` de gerente de entidades, e é isso que ela faz.

Esse gerenciamento é feito para que a interação com o banco de dados seja a mais otimizada possível.

Um exemplo disso é o caso de quando buscamos um registro duas vezes com o método `find` na mesma instância de `EntityManager`.

Supondo que buscamos o registro de identificador 1, ao tentar buscar pela segunda vez, o `EntityManager` não iria no banco, mas resgataria o mesmo da memória.

Podemos chamar isso também de **cache de primeiro nível**.

Outra coisa que ele faz é postergar as alterações de banco de dados para o mais tardar possível.

Quer dizer que uma operação de inserção ou atualização não é levada ao banco logo no momento em que o método `persist` (ou método `merge`, que veremos mais a frente) é chamado.

Isso porque podem existir momentos em que, por exemplo, uma entidade foi passada para ser inserida ou atualizada, mas, dentro da mesma execução, removida antes da confirmação (`commit`).

Por postergar as operações, o `EntityManager` economizaria uma operação com o banco de dados com uma entidade que foi passada para inserção e depois removida.

Não existe um momento exato para que o `EntityManager` jogue as operações para o banco de dados.

O que é certo é que, se ainda houver algo pendente no momento da chamada do método `commit`, tudo vai ser executado e a confirmação será feita.

Existe também a possibilidade de adiantar a operação com o banco de dados através do método `flush`.

Supondo que foi realizada uma operação de inserção com o método `persist`, se logo após for chamado o método `flush`, então mesmo que o `EntityManager` estivesse guardando a inserção para depois, a tentativa seria feita.

O objetivo do `flush` é sincronizar as alterações, ainda na memória do `EntityManager`, com a base de dados.

Note que, mesmo chamando o `flush`, ainda é necessário o `commit`.

Na verdade, o `commit` que, logo antes de confirmar as operações, chama o `flush`.

O nível de gerenciamento do `EntityManager` para com as entidades não para por aí.

Ele consegue reconhecer até mesmo as alterações nos atributos das entidades para que, quando for o momento, ele jogue o comando de atualização (*update*) para o banco de dados.

Isso acontece com objetos que foram resgatados com o método `find` e também com objetos pré-existentes que passam a ser gerenciados após a chamada do método `merge`.

O método `merge` serve para incluir um objeto, construído pela nossa aplicação, no gerenciamento do `EntityManager`.

## Atualizando registros

Atualizar registros utilizando o `EntityManager` é algo natural. Basta simplesmente alterar os atributos de uma entidade que já está como gerenciada.

O `EntityManager` consegue reconhecer essa alteração e quando for o momento (no máximo, até a chamada do método `commit`) ele executa o comando na base.

Primeiro veja a atualização de um objeto que já nasce gerenciado, ou seja, foi retornado pelo método `find`:

```
public` `static` `void` `main(String... args) {`` ``EntityManagerFactory entityManagerFactory ``    ``= Persistence.createEntityManagerFactory(``"Clientes-PU"``);`` ``EntityManager entityManager = entityManagerFactory.createEntityManager();``  ` ` ``Cliente cliente = entityManager.find(Cliente.``class``, ``1``);` ` ``entityManager.getTransaction().begin();`` ``cliente.setNome(``"Autopeças Rodovia"``);`` ``entityManager.getTransaction().commit();` ` ``entityManager.close();`` ``entityManagerFactory.close();``}
```

Repare que a única coisa feita foi a alteração do atributo nome através do método `setNome` da classe `Cliente`.

Basta isso para que o `EntityManager` saiba que precisa executar o comando de atualização no banco de dados.

Agora a outra forma é utilizando o método `merge`:

```
public` `static` `void` `main(String... args) {`` ``EntityManagerFactory entityManagerFactory = Persistence.createEntityManagerFactory(``"Clientes-PU"``);`` ``EntityManager entityManager = entityManagerFactory.createEntityManager();``  ` ` ``Cliente cliente = ``new` `Cliente();`` ``cliente.setId(``1``);`` ``cliente.setNome(``"Autopeças Estrada"``);` ` ``entityManager.getTransaction().begin();`` ``entityManager.merge(cliente);`` ``entityManager.getTransaction().commit();` ` ``entityManager.close();`` ``entityManagerFactory.close();``}
```

Note que o cliente de identificador 1 deve existir na base de dados.

Logo que chamamos o método `merge`, ele faz uma exata cópia do objeto `cliente`.

A cópia passa a ser gerenciada e o `EntityManager` reconhece que precisa executar a atualização.

## Inserindo registros com o merge

No primeiro exemplo de inserção foi usado o `persist` para esse trabalho.

Mas podemos fazer isso com o `merge` também.

```
public` `static` `void` `main(String... args) {`` ``EntityManagerFactory entityManagerFactory ``    ``= Persistence.createEntityManagerFactory(``"Clientes-PU"``);`` ``EntityManager entityManager = entityManagerFactory.createEntityManager();``  ` ` ``Cliente cliente = ``new` `Cliente();`` ``cliente.setNome(``"Eletrônica Almeida"``);` ` ``entityManager.getTransaction().begin();`` ``entityManager.merge(cliente);`` ``entityManager.getTransaction().commit();` ` ``entityManager.close();`` ``entityManagerFactory.close();``}
```

O `EntityManager` simplesmente reconhece que tem um objeto gerenciado sem identificador e faz a inserção.

## Diferença entre o persist e o merge

O método `persist` serve para entidades novas, que acabaram de ser criadas e que ainda não existem na base de dados.

Quando passamos uma nova entidade para ele, o mesmo a torna uma entidade gerenciada e que será inserida na base.

O objetivo do método `merge` é tornar um objeto novo como gerenciado.

Mas não é a exata instância passada para ele que será gerenciada.

Ele pega a instância dada a ele e faz uma cópia. Essa sim é a instância gerenciada.

Para ter acesso a ela, basta pegar o retorno do `merge`. Assim:

```
cliente = entityManager.merge(cliente);
```

Uma vez que a instância está como gerenciada, qualquer alteração na mesma é enviada para a base de dados no momento do `flush` e confirmada no `commit` da transação.

## Impedindo a sincronização com o banco

Esse comportamento do `EntityManager` de reconhecer que uma entidade precisa ser inserida ou atualizada na base de dados facilita as coisas.

Mas pode ser que esse comportamento não seja desejado.

Para evitar isso você pode remover a entidade do gerenciamento do `EntityManager`. Isso é feito com o método `detach`:

```
public` `static` `void` `main(String... args) {`` ``EntityManagerFactory entityManagerFactory ``    ``= Persistence.createEntityManagerFactory(``"Clientes-PU"``);`` ``EntityManager entityManager = entityManagerFactory.createEntityManager();``  ` ` ``Cliente cliente = entityManager.find(Cliente.``class``, ``2``);`` ``entityManager.detach(cliente);` ` ``entityManager.getTransaction().begin();`` ``cliente.setNome(``"Autopeças Rodovia"``);`` ``entityManager.getTransaction().commit();`` ` ` ``entityManager.close();`` ``entityManagerFactory.close();``}
```

Repare o código acima onde o objeto `cliente` não será sincronizado com a base de dados.

## Estados de uma entidade

Uma entidade pode assumir alguns estados com relação ao `EntityManager`. Os estados podem ser:

- Novo (*new* ou *transient*)
- Gerenciado (*managed*)
- Removido (*removed*)
- Desanexado (*detached*)

O estado “novo” é o mais natural. É simplesmente quando construímos um objeto qualquer usando o operador `new`.

Para estar no estado “gerenciado”, podemos chamar os métodos `persist`, `merge` ou buscar a entidade usando o `EntityManager`.

O estado “removido” é alcançado quando chamamos o método `remove`.

Por último, uma entidade fica no estado “desanexado” quando é passada para o método `detach`.

Importante notar que entidades desanexadas podem voltar a ser gerenciadas com a chamada do método `merge`.

Veja como fica no diagrama abaixo.

[![Diagrama de estados](https://algaworks-blog.s3.amazonaws.com/wp-content/uploads/Diagrama-de-estados.png)](https://algaworks-blog.s3.amazonaws.com/wp-content/uploads/Diagrama-de-estados.png)

Repare os comentários no código abaixo:

```
public` `static` `void` `main(String... args) {`` ``EntityManagerFactory entityManagerFactory ``    ``= Persistence.createEntityManagerFactory(``"Clientes-PU"``);`` ``EntityManager entityManager = entityManagerFactory.createEntityManager();``  ` ` ``// Estado novo`` ``Cliente cliente = ``new` `Cliente();`` ``cliente.setNome(``"Construtora Silva"``);` ` ``entityManager.getTransaction().begin();` ` ``// Estado gerenciado`` ``entityManager.persist(cliente);` ` ``// Estado desanexado (nenhuma operação será feita)`` ``entityManager.detach(cliente);` ` ``// Volta ao estado gerenciado `` ``cliente = entityManager.merge(cliente);` ` ``// Estado removido (será removido da base de dados)`` ``entityManager.remove(cliente);` ` ``entityManager.getTransaction().commit();` ` ``entityManager.close();`` ``entityManagerFactory.close();``}
```

## Quando chamar *begin* e *commit*

É uma boa prática deixar todas as operações de alteração de banco de dados (inserção, atualização e remoção) nos limites dos métodos `begin` e `commit`.

Acontece que, como o `EntityManager` busca postergar o envio dos comandos para a base de dados, podemos ter uma versão de interação com o banco menos intuitiva, mas que funciona.

Isso seria o mais recomendado:

```
Cliente cliente = ``new` `Cliente();``cliente.setNome(``"Computer Nova Informática"``);` `entityManager.getTransaction().begin();``entityManager.persist(cliente);``entityManager.getTransaction().commit();
```

Mas assim também funciona:

```
Cliente cliente = ``new` `Cliente();``cliente.setNome(``"Rei dos Tecidos"``);``entityManager.persist(cliente);` `entityManager.getTransaction().begin();``entityManager.getTransaction().commit();
```

No momento da chamada do método `persist`, o `EntityManager` não tenta enviar para o banco.

Depois a transação é aberta e, ao chamar o `commit`, as alterações pendentes são enviadas para a base dentro da transação.

É uma situação que não ocorre muito, mas é interessante conhecer.

## Conclusão

Apesar de ser um artigo para quem está começando, agora tem muita coisa que você sabe que muitos outros programadores (até mesmo alguns mais experientes) não sabem, principalmente os detalhes do funcionamento do `EntityManager`.

Sua parte agora é executar os exemplos do artigo e ir além, executando alguns códigos inventados por você.

E se você tem interesse em aprofundar mais em JPA, conheça o [nosso curso online avançado de JPA e Hibernate](https://cafe.algaworks.com/jpa-lista-espera/?utm_source=blog-aw&utm_medium=artigo&utm_campaign=blog-posts&utm_content=artigo-tutorial-jpa).

Um abraço e até a próxima!

**PS**: você pode baixar o código-fonte dos exemplos em nosso GitHub: [http://github.com/algaworks/artigo-tutorial-jpa](https://github.com/algaworks/artigo-tutorial-jpa).