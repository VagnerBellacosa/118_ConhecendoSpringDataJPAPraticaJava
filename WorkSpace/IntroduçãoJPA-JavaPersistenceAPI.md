# Introdução à JPA - Java Persistence API

## Veja neste artigo uma introdução à JPA. Veremos o que é JPA, como se deu a sua evolução até a sua versão atual, quais são as suas principais características que foram incluídas e quais os benefícios de utilizar JPA em aplicações corporativas.

[Artigos](https://www.devmedia.com.br/artigos/)[Java](https://www.devmedia.com.br/artigos/java)Introdução à JPA - Java Persistence API

JPA é um framework leve, baseado em [POJOS (Plain Old Java Objects)](https://www.devmedia.com.br/diferenca-entre-os-patterns-po-pojo-bo-dto-e-vo/28162) para persistir objetos Java. A **Java Persistence API**, diferente do que muitos imaginam, não é apenas um framework para [Mapeamento Objeto-Relacional (ORM - Object-Relational Mapping)](https://www.devmedia.com.br/tecnicas-de-mapeamento-objeto-relacional-revista-sql-magazine-40/6980), ela também oferece diversas funcionalidades essenciais em qualquer aplicação corporativa.

Atualmente temos que praticamente todas as aplicações de grande porte **utilizam JPA para persistir objetos Java**. JPA provê diversas funcionalidades para os programadores, como será mais detalhadamente visto nas próximas seções. Inicialmente será visto a história por trás da JPA, a qual passou por algumas versões até chegar na sua versão atual.

<iframe width="560" height="315" src="https://www.youtube.com/embed/VAxT1YD77DY" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="" style="outline: none; -webkit-tap-highlight-color: transparent; max-width: 100%; margin: auto; height: 455.062px; width: 809px;"></iframe>

Curso: [O que é JPA?](https://www.devmedia.com.br/curso/java-persistence-api/2136)

### História da Especificação

Após diversos anos de reclamações sobre a complexidade na construção de aplicações com Java, a especificação [Java EE](https://www.devmedia.com.br/guia/java-enterprise-edition-java-ee/34474) 5 teve como principal objetivo a facilidade para desenvolver aplicações JEE 5. O EJB 3 foi o grande percursor para essa mudança fazendo os Enterprise JavaBeans mais fáceis e mais produtivos de usar.

No caso dos Session Beans e Message-Driven Beans, a solução para questões de usabilidade foram alcançadas simplesmente removendo alguns dos mais onerosos requisitos de implementação e permitindo que os componentes sejam como Plain Java Objects ou POJOS.

Já os Entity Beans eram um problema muito mais sério. A solução foi começar do zero. Deixou-se os Entity Beans sozinhos e introduziu-se um novo modelo de persistência. A versão atual da JPA nasceu através das necessidades dos profissionais da área e das soluções proprietárias que já existiam para [resolver os problemas com persistência.](https://www.devmedia.com.br/jpa-2-0-persistencia-a-toda-prova/17437) Com a ajuda dos desenvolvedores e de profissionais experientes que criaram outras ferramentas de persistência, chegou a uma versão muito melhor que é a que os desenvolvedores Java conhecem atualmente.

Dessa forma os líderes das soluções de mapeamento objetos-relacionais deram um passo adiante e padronizaram também os seus produtos. Hibernate e TopLink foram os primeiros a firmar com os fornecedores EJB.

O resultado final da especificação EJB finalizou com três documentos separados, sendo que o terceiro era o Java Persistence API. Essa especificação descrevia o modelo de persistência em ambos os ambientes [Java SE](https://www.devmedia.com.br/java-se-7-plataforma-java-magazine-83/18015) e Java EE.

### JPA 2.0

No momento em que a primeira versão do JPA foi iniciada, outros modelos de persistência ORM já haviam evoluído. Mesmo assim muitas características foram adicionadas nesta versão e outras foram deixadas para uma próxima versão.

A versão JPA 2.0 incluiu um grande número de características que não estavam na primeira versão, especialmente as mais requisitadas pelos usuários, entre elas a capacidade adicional de mapeamento, expansões para a [Java Persistence Query Language (JPQL)](https://www.devmedia.com.br/realizando-consultas-com-jpa-e-jpql/33362), a API Criteria para criação de consultas dinâmicas, entre outras características.

Curso: [JPQL: Escrevendo consultas na JPA](https://www.devmedia.com.br/curso/curso-jpql/2108)

Entre as principais inclusões na JPA destacam-se:

- **POJOS Persistentes**: Talvez o aspecto mais importante da JPA seja o fato que os objetos são POJOs (Plain Old Java Object ou Velho e Simples Objeto Java), significando que os objetos possuem design simples que não dependem da herança de interfaces ou classes de frameworks externos. Qualquer objeto com um construtor default pode ser feito persistente sem nenhuma alteração numa linha de código. Mapeamento Objeto-Relacional com JPA é inteiramente dirigido a metadados. Isto pode ser feito através de anotações no código ou através de um XML definido externamente.
- **Consultas em Objetos**: As consultas podem ser realizadas através da Java Persistence Query Language (JPQL), uma linguagem de consulta que é derivada do EJB QL e transformada depois para SQL. As consultas usam um esquema abstraído que é baseado no modelo de entidade como oposto às colunas na qual a entidade é armazenada.
- **Configurações simples**: Existe um grande número de características de persistência que a especificação oferece, todas são configuráveis através de anotações, XML ou uma combinação das duas. Anotações são simples de usar, convenientes para escrever e fácil de ler. Além disso, JPA oferece diversos valores defaults, portanto para já sair usando JPA é simples, bastando algumas anotações.
- **Integração e Teste**: Atualmente as aplicações normalmente rodam num Servidor de aplicação, sendo um padrão do mercado hoje. Testes em servidores de aplicação são um grande desafio e normalmente impraticáveis. Efetuar teste de unidade e teste caixa branca em servidores de aplicação não é uma tarefa tão trivial. Porém, isto é resolvido com uma API que trabalha fora do servidor de aplicação. Isto permite que a JPA possa ser utilizada sem a existência de um servidor de aplicação. Dessa forma, testes unitários podem ser executados mais facilmente.

#### Conclusão

Neste artigo vimos uma **introdução sobre JPA**. Destacou-se [o que é JPA](https://www.devmedia.com.br/curso/java-persistence-api/2136), como JPA evoluiu ao longo do tempo e o que influenciou as características presentes na sua versão atual, considerada uma grande evolução na plataforma Java. Além disso, tivemos a oportunidade de verificar quais são as principais inclusões na atual versão da JPA e como eles podem impulsionar o desenvolvimento de aplicações corporativas que exigem uma manutenção mais simples, fácil e rápida além de confiabilidade e testes de unidades, indispensável em qualquer aplicação corporativa hoje.

Conforme solicitado pelos usuários, esse primeiro artigo introdutório foi feito pensando nas características básica da JPA, enfatizando como se deu seu desenvolvimento e quais foram as suas principais características disponibilizadas nas últimas versões. Peço que continuem enviando seus e-mails, mais artigos sobre JPA estarão disponíveis em breve.

#### Links Úteis

- [Gerenciando mudanças com o Oracle RAT](https://www.devmedia.com.br/gerenciando-mudancas-com-o-oracle-rat/39296):
  Este artigo apresenta como o Oracle Real Application Testing pode ser utilizado para prever e mensurar riscos e benefícios para uma modificação seja ela de hardware, software ou configuração.
- [PHP e Bootstrap: cadastro completo com autenticação via e-mail](https://www.devmedia.com.br/exemplo/php-e-bootstrap-cadastro-completo-com-autenticacao-via-e-mail/46):
  Aprenda neste exemplo como criar um sistema de cadastro com autenticação por e-mail. Faremos isso usando apenas PHP em conjunto com o banco de dados MySQL, além do Bootstrap para estilizar as páginas da aplicação.
- [AngularJS e Bootstrap: Desenvolva front-ends modernos e responsivos](https://www.devmedia.com.br/angularjs-e-bootstrap-desenvolva-front-ends-modernos-e-responsivos/39295):
  Esse artigo busca retratar através de exemplos práticos uma forma comum de integração entre os frameworks Bootstrap e AngularJS, ambos famosos por abraçar diferentes âmbitos do desenvolvimento web.

#### Saiba mais sobre Java ;)

- [Guias Java](https://www.devmedia.com.br/guias/java):
  Encontre aqui os Guias de estudo que vão ajudar você a aprofundar seu conhecimento na linguagem Java. Desde o básico ao mais avançado. Escolha o seu!
- [Carreira Programador Java](https://www.devmedia.com.br/guia/carreira-programador-java/37809):
  Nesse Guia de Referência você encontrará o conteúdo que precisa para iniciar seus estudos sobre a tecnologia Java, base para o desenvolvimento de aplicações desktop, web e mobile/embarcadas.
- [Linguagem de Programação Java](https://www.devmedia.com.br/guia/linguagem-java/38169):
  Neste Guia de Referência você encontrará todo o conteúdo que precisa para começar a programar com a linguagem Java, a sua caixa de ferramentas base para criar aplicações com Java.

##### Bibliografia

- [The Java Persistence API - A Simpler Programming Model for Entity Persistence](http://www.oracle.com/technetwork/articles/javaee/jpa-137156.html)
- [Understanding JPA](http://www.javaworld.com/javaworld/jw-01-2008/jw-01-jpa1.html)
- Pro JPA 2 Mastering the Java Persistence API, Apress, 2009.

Tecnologias:

- [Java](https://www.devmedia.com.br/java/)
- JPA