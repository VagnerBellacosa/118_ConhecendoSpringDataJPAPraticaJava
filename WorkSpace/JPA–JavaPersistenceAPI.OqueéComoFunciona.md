![img](https://www.tidicas.com.br/wp-content/themes/twentyten/images/headers/path.jpg)

- https://www.tidicas.com.br/?page_id=2)

[← Eclipse Instalação e Configuração Inicial](https://www.tidicas.com.br/?p=1635)

[JPA – Anotação @Transient o que é ? →](https://www.tidicas.com.br/?p=1933)

# JPA – Java Persistence API. O que é ? Como Funciona ?

Publicado em [13 de dezembro de 2020](https://www.tidicas.com.br/?p=1864) por [Evaldo Junior](https://www.tidicas.com.br/?author=1)

Neste tutorial irei explicar o que é a JPA (Java Persistence API), e mostrar como ela funciona.

![Eclipse](http://www.tidicas.com.br/wp-content/uploads/2020/12/jpa_01.png)

**O que é a JPA** – É uma especificação, e como uma especificação, ela preocupa-se com a persistência, o que significa vagamente qualquer mecanismo pelo qual os objetos Java sobrevivam ao processo do aplicativo que os criou. Nem todos os objetos Java precisam ser persistidos, mas a maioria dos aplicativos persiste os principais objetos de negócios.

Abaixo segue um diagrama sobre ORM – Object Relational Mapping (Mapeamento Objeto Relacional)
![Eclipse](http://www.tidicas.com.br/wp-content/uploads/2020/12/jpa_diagrama_01.png)

Esse diagrama mostra o fluxo de funcionamento da API JPA, de onde a ela é inicialmente chamada, e até onde ela vai.
Como o primeiro framework Java para ORM foi o Hibernate, no início eram utilizados arquivos XML, os conhecidos hbm.xml, depois veio o XDoclet, antecessor das Annotations. Era utilizado com ejb’s e o Hibernate.
A JPA, é uma especificação Java para acessar, persistir e gerenciar dados entre objetos Java e um banco de dados relacional. O JPA foi definido como parte da especificação EJB 3.0 como um substituto para a especificação EJB 2 CMP Entity Beans. A JPA veio da necessidade de simplificar a complexidade do EJB para persistir dados.
A JPA agora é considerada a abordagem padrão da indústria para ORM – Object to Relational Mapping.
Mas independente do framework ORM utilizado, seja o Hibernate, o EclipseLink, o TopLink, o OpenJPA, etc, isso não importa, a aplicação será portável para qualquer banco de dados que possua driver JDBC. E não será preciso reescrever o código-fonte, pois ele será o mesmo para todos os bancos de dados.

**Como funciona a JPA** – Ele funciona através de qualquer framework ORM (Mapeamento Objeto Relacional) baseado na especificação JPA.
Pode ser o framework Hibernate, EclipseLink, TopLink, OpenJpa, etc.

Abaixo segue um diagrama bem detalhado sobre ORM – Object Relational Mapping (Mapeamento Objeto Relacional)
![Eclipse](http://www.tidicas.com.br/wp-content/uploads/2020/12/jpa_diagrama_02.png)

Nesse diagrama podemos ver o mundo exterior ao sistema onde os usuários podem acessar a aplicação de qualquer dispositivo, seja um computador, um tablet, um smartphone ou um notebook, eles vão através da aplicação chamar a camada ORM, onde e localiza a interface JPA e o framework utilizado seja o Hibernate, o EclipseLink, o TopLink, o OpenJPA, etc. E nesse diagrama podemos ver detalhadamente a divisão entre a interface JPA e o framework ORM utilizado.

Agora vou citar alguns objetos da API JPA:
**EntityManagerFactory** – Interface usada para interagir com a fábrica do gerenciador de entidades para a unidade de persistência.
Quando o aplicativo terminar de usar a fábrica do gerenciador de entidades e/ou no encerramento do aplicativo, o aplicativo deve fechar a fábrica do gerenciador de entidades. Depois que um EntityManagerFactory é fechado, todos os seus gerenciadores de entidade são considerados no estado fechado.

**EntityManager** – Interface usada para interagir com o contexto de persistência.
Uma instância de EntityManager está associada a um contexto de persistência. Um contexto de persistência é um conjunto de instâncias de entidade em que, para qualquer identidade de entidade persistente, existe uma instância de entidade única. Dentro do contexto de persistência, as instâncias de entidade e seu ciclo de vida são gerenciados. A API EntityManager é usada para criar e remover instâncias de entidade persistentes, para localizar entidades por sua chave primária e para consultar entidades.
O conjunto de entidades que podem ser gerenciadas por uma determinada instância EntityManager é definido por uma unidade de persistência. Uma unidade de persistência define o conjunto de todas as classes relacionadas ou agrupadas pelo aplicativo e que devem ser colocadas em seu mapeamento em um único banco de dados.

Agora vou citar algumas anotações da API JPA:
**@Entity** – Serve para a API JPA saber que a classe anotada com @Entity corresponde a uma tabela da base de dados. Uma entidade corresponde a uma tabela.
**@Table** – É usada em conjunto com a anotação @Entity, serve para a API JPA saber o nome da tabela, em que a classe anotada com @Table, tem um atributo “name”, que contém o nome da tabela.

Abaixo segue um exemplo prático da utilização das anotações @Entity e @Table.

Abaixo a ilustração da classe Blog, como podem ver ela possui as anotações @Entity e @Table. E na anotação @Table, o atributo name, está preenchido com o nome da tabela “blog”.
![Eclipse](http://www.tidicas.com.br/wp-content/uploads/2020/12/blog.png)

Abaixo a ilustração da classe Categoria, como podem ver ela possui as anotações @Entity e @Table. E na anotação @Table, o atributo “name”, está preenchido com o nome da tabela “categoria”.

![Eclipse](http://www.tidicas.com.br/wp-content/uploads/2020/12/categoria.png)

Segue um exemplo de aplicação Java utilizando JPA.

[Baixar Código-Fonte via GitHub](https://github.com/jrxxjr/jpa)

No vídeo abaixo, irei mostrar este tutorial.



Para ver o vídeo no YouTube [Clique Aqui](https://youtu.be/FK7MAlcnqrQ)

Por favor, deixe seu like se gostar da dica.

Fonte: https://www.oracle.com/java/technologies/persistence-jsp.html

Esta entrada foi publicada em [Java SE](https://www.tidicas.com.br/?cat=8). Adicione o [link permanente](https://www.tidicas.com.br/?p=1864)aos seus favoritos.

[← Eclipse Instalação e Configuração Inicial](https://www.tidicas.com.br/?p=1635)

[JPA – Anotação @Transient o que é ? →](https://www.tidicas.com.br/?p=1933)

-  

- ### Tópicos recentes

  - [Java – Convenções de Nomenclatura](https://www.tidicas.com.br/?p=2106)
  - [JPA – Relacionamentos OneToOne, ManyToOne, OneToMany, ManyToMany](https://www.tidicas.com.br/?p=2058)
  - [JPA – Comparativo entre Hibernate e EclipseLink](https://www.tidicas.com.br/?p=2028)
  - [JPA – Consultas utilizar JPQL ou Criteria ?](https://www.tidicas.com.br/?p=2005)
  - [JPA – Conexão com 2 Bancos de Dados Diferentes](https://www.tidicas.com.br/?p=1980)

  ### 