# JPA – O QUE É? PARA QUE SERVE? COMO IMPLEMENTAR UM SISTEMA SIMPLES?

30 de junho de 2015

![img](https://secure.gravatar.com/avatar/96b0cf816922260d5cefaf274a072e27?s=112&d=mm&r=g)[Danilo Filitto](https://dfilitto.com.br/author/danilo-filitto/)

Share This!



## JPA – O que é? Para que serve? Como implementar um sistema simples?

O post **JPA – O que é? Para que serve? Como implementar um sistema simples?** tem como objetivo descrever as principais vantagens de se utilizar a tecnologia JPA no desenvolvimento de softwares comerciais utilizando a linguagem de programação java e demonstrar como implementar um sistema de perguntas simples utilizando essa tecnologia.

### 

### Introdução



Ao desenvolver softwares comerciais, boa parte do tempo investido na criação é gasto com a codificação de queries SQL para manipular um banco de dados, inserindo, alterando, excluindo e localizando dados nas tabelas.
Outro grande problema é a mudança de paradigma ao desenvolver os sistemas que utilizam banco de dados, pois a maioria das linguagens de programação utiliza o paradigma de programação orientada a objeto, em que os dados são representados por meio de classes e atributos, podendo utilizar herança, composição para relacionar atributos, polimorfismo, enumerações, entre outros. Enquanto que os banco de dados utilizam o paradigma voltado para o relacionamento entre entidades, o qual representa os dados no banco utilizando tabelas e colunas, que possuem chave primária (PK) e podem ser relacionadas por meio da criação de chaves estrangeiras (FK) com outras tabelas.

Isto faz com que o programador tenha que pensar de duas maneiras diferentes para fazer um único sistema.

Pensando nisso, varias ferramentas vem sendo desenvolvidas para auxiliar nesta tarefa. Essas ferramentas são conhecidas como ferramentas de mapeamento objeto-relacional (ORM).

No caso da linguagem de programação java, pode-se utilizar o JPA implementado em vários FrameWorks como EclipseLink, Hibernet e TopLink.

### 

### Mas o que é JPA e para que serve?

### 

Java Persistence API – JPA é uma coleção de classes e métodos voltados para armazenar persistentemente as vastas quantidades de dados em um banco de dados.  Com base no JPA vários FrameWorks são desenvolvidos (como o EclipseLink, Hibernet e TopLink) com o objetivo de proporcionar uma interação com um banco de dados relacional, evitando com que o desenvolvedor gaste tempo com o desenvolvimento de códigos voltados para a manipulação dos dados presentes no banco de dados.

[![Funcionamento da JPA](https://i1.wp.com/www.dfilitto.com.br/wp-content/uploads/2015/06/jpa_provider.png?resize=320%2C163)](https://i1.wp.com/3.bp.blogspot.com/-v5On0ev3JEk/VZKcFjYy7sI/AAAAAAAALZI/q78KRkSZXJ4/s1600/jpa_provider.png)

Exemplo de utilização do JPA

Em outras palavras o JPA proporciona meios de armazenar os dados presentes nos objetos implementados no sistema desenvolvido dentro das entidades no banco de dados. A imagem a seguir mostra a arquitetura de nível de classe JPA. Ele exibe as classes e interfaces de JPA.

[![JPA - Java Percistence API](https://i1.wp.com/www.dfilitto.com.br/wp-content/uploads/2015/06/jpa_class_level_architecture.png?resize=320%2C189)](https://i0.wp.com/1.bp.blogspot.com/-dMb8BKi9m08/VZKf2HA7_yI/AAAAAAAALZU/0Buymq0L4zg/s1600/jpa_class_level_architecture.png)

Arquitetura da API JPA

### Como implementar um sistema utilizando a tecnologia JPA?

Pesquisando em vários canais no YouTube, encontrei um canal que explica detalhadamente como trabalhar com o JPA utilizando o FrameWork EclipseLink com o banco de dados JavaDB.

#### 01 – Introdução ao JPA – Parte 1

https://www.youtube.com/watch?v=1amGLhkd_FE

#### 02 – Introdução ao JPA – Parte 2

https://www.youtube.com/watch?v=JyNEf2t3sY4

#### 03 – Introdução ao JPA – Parte 3

https://www.youtube.com/watch?v=GQwC1zjSi34

#### 04 – Introdução ao JPA – Parte 4

https://www.youtube.com/watch?v=OSDfD0IVjSw

#### 05 – Introdução ao JPA – Parte 5

https://www.youtube.com/watch?v=l6zzfB9_tWk

#### Fontes:

- [Uma introdução prática ao JPA com Hibernate](http://www.caelum.com.br/apostila-java-web/uma-introducao-pratica-ao-jpa-com-hibernate/)
- [JPA – Introduction](http://www.tutorialspoint.com/jpa/jpa_introduction.htm)
- [Ricardo Antonello](https://www.youtube.com/channel/UCvfuJbZ-4DCNKAEeiETl5ow)