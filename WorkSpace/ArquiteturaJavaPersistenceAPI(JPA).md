# Arquitetura Java Persistence API (JPA)

A persistência de dados é a possibilidade de manter os dados entre as execuções do aplicativo. A persistência é vital para os aplicativos corporativos por causa do acesso necessário aos bancos de dados relacionais. Os aplicativos que forem desenvolvidos para este ambiente devem gerenciar a persistência por si só ou usar soluções de terceiros para manipular as atualizações e recuperações do banco de dados com persistência. O Java™ Persistence API (JPA) fornece um mecanismo para gerenciar a persistência e mapeamento relacional de objeto e funções para as especificações de EJB.

A especificação JPA define o mapeamento relacional de objetos internamente, em vez de depender das implementações de mapeamento específicas do fornecedor. A JPA está baseada no modelo de programação Java que se aplica aos ambientes Java EE, mas a JPA pode funcionar em um ambiente Java SE para teste das funções do aplicativo.

A JPA representa uma simplificação do modelo de programação de persistência. A especificação JPA define explicitamente o mapeamento relacional de objetos, em vez de depender das implementações de mapeamento específicas do fornecedor. A JPA padroniza a importante tarefa de mapeamento relacional de objetos utilizando anotações ou o XML para mapear objetos para uma ou mais tabelas de um banco de dados. Para simplificar ainda mais o modelo de programação de persistência:

- A API EntityManager pode persistir, atualizar, recuperar ou remover objetos de um banco de dados
- A API EntityManager e os metadados de mapeamento de objeto relacional manipulam a maioria das operações do banco de dados sem precisar gravar o código JDBC ou SQL para manter a persistência.
- A JPA fornece uma linguagem de consulta, estendendo a linguagem de consulta EJB independente (também conhecida como JPQL), que pode ser utilizada para recuperar objetos sem a gravação de consultas SQL específicas do banco de dados com o qual se está trabalhando.

A JPA foi projetada para operar dentro e fora de um contêiner Java Enterprise Edition (Java EE). Ao executar a JPA em um contêiner, os aplicativos podem usar o contêiner para gerenciar o contexto de persistência. Se não houver contêiner para gerenciar a JPA, o aplicativo deverá manipular o gerenciamento do contexto de persistência sozinho. Os aplicativos projetados para persistência gerenciada por contêiner não exigem muita implementação de código para tratar a persistência, mas esses aplicativos não podem ser utilizados fora de um contêiner. Aplicativos que gerenciam sua própria persistência podem funcionar em um ambiente de contêiner ou ambiente Java SE.



Os contêineres Java EE que suportam o modelo de programação EJB 3.x devem suportar uma implementação JPA, também chamada provedor de persistência. Um provedor de persistência JPA usa os seguintes elementos para permitir o fácil gerenciamento de persistência em um ambiente EJB 3.x:

- Unidade de persistência

  Consiste de metadados declarativos que descrevem o relacionamento dos objetos da classe de entidade para um banco de dados relacional. O EntityManagerFactory utiliza esses dados para criar um contexto de persistência que possa ser acessado por meio do EntityManager.

- EntityManagerFactory

  Usado para criar um EntityManager para interações com o banco de dados. Os contêineres do servidor de aplicativos normalmente fornecem essa função, mas o EntityManagerFactory será necessário se você estiver utilizando a persistência gerenciada pelo aplicativo JPA. Uma instância do EntityManagerFactory representa um Contexto de Persistência.

- Contexto de persistência

  Define o conjunto de instâncias ativas que o aplicativo está manipulando atualmente. O contexto de persistência pode ser criado manualmente ou por meio de injeção.

- EntityManager

  O gerenciador de recursos que mantém a coleção ativa de objetos de entidade que estão sendo usados pelo aplicativo. O EntityManager manipula a interação do banco de dados e os metadados para mapeamentos de objetos relacionais. Uma instância do EntityManager representa um Contexto de Persistência. Um aplicativo em um contêiner pode obter o EntityManager por meio de introdução no aplicativo ou consultando-o no espaço de nomes de componentes Java. Se o aplicativo gerenciar sua persistência, o EntityManager será obtido do EntityManagerFactory.**Atenção:** A injeção do EntityManager é suportada apenas para os seguintes artefatos:Beans de sessão EJB 3.xBeans acionados por mensagens EJB 3.xServlets, Filtros de Servlet e ListenersIdentificadores de tag JSP que implementam as interfaces javax.servlet. jsp.tagext.Tag e javax.servlet.jsp.tagext.SimpleTagBeans gerenciados do JavaServer Faces (JSF)a classe principal do aplicativo cliente.

- Objetos de entidade

  Uma classe Java simples que representa uma linha em uma tabela de banco de dados em sua forma mais simples. Os objetos de entidade podem ser classes concretas ou abstratas. Elas mantêm os estados utilizando propriedades ou campos.

## API de Persistência Java e Contextos de Transação Local

O WebSphere fornece contextos de transação global e local a componentes gerenciados. Uma transação é sempre ativa se é um contexto de transação global (GTC) ou um contexto de transação local (LTC) com encadeamentos que verificam um componente gerenciado. Enquanto essa transação ativa não tem nenhum efeito em contextos de persistência gerenciada por aplicativo (ou seja, EntityManagers JPA adquiridos por meio de EntityManagerFactories injetados em um aplicativo via @PersistenceUnit), ela influencia contextos de persistência gerenciada por contêiner (ou seja, EntityManagers JPA injetados por meio de @PersistenceContext).

Os métodos da API JPA lançam uma TransactionRequiredException quando chamados de fora dos limites de um GTC e ainda lançam a exceção como esperado. No entanto, enquanto o LTC permanece ativo (até que um novo GTC seja iniciado ou o fim da chamada de serviço de componente seja atingido) o contexto de persistência de transações gerenciadas por contêiner (CMT) também permanece ativo. Permanecer ativo significa que as entidades buscadas (por localizar ou consulta) por um EntityManager CMT dentro dos limites de um LTC ainda são gerenciadas por esse contexto de persistência em vez de se tornarem imediatamente separadas. Esse comportamento pode surpreender alguns como uma diferença inesperada de comportamento em comparação com alguns guias de programação JPA. No entanto, uma vez que um GTC começa, o contexto de persistência mantido ativo pelo LTC é eliminado e entidades que são gerenciadas por esse contexto de persistência tornam-se separadas.

- **[JPA for WebSphere Application Server](https://www.ibm.com/docs/pt-br/SSEQTP_8.5.5/com.ibm.websphere.nd.multiplatform.doc/ae/cejb_jpanote.html)**
  O Java Persistence API (JPA) 2.0 for WebSphere Application Server é construído no projeto de software livre Apache OpenJPA 2.x.
- **[Comando wsjpaversion](https://www.ibm.com/docs/pt-br/SSEQTP_8.5.5/com.ibm.websphere.nd.multiplatform.doc/ae/rejb_wsjpaversion.html)**
  Use essa ferramenta de linha de comandos para descobrir informações sobre a versão instalada do Java Persistence API (JPA) for WebSphere Application Server.
- **[Propriedades wsjpa](https://www.ibm.com/docs/pt-br/SSEQTP_8.5.5/com.ibm.websphere.nd.multiplatform.doc/ae/cdat_wsjpaprop.html)**
  As propriedades de extensão da Java Persistence API (JPA) para WebSphere Application Server podem ser especificadas com o prefixo openjpa ou wsjpa. Esse tópico apresenta as propriedades wsjpa.

## Tarefas relacionadas

- [Visão geral da tarefa: IBM Optim pureQuery Runtime](https://www.ibm.com/docs/pt-br/SSEQTP_8.5.5/com.ibm.websphere.nd.multiplatform.doc/ae/tejb_jpapdq.html)

## Informações relacionadas

- [Transações Local e Global](https://www.ibm.com/docs/pt-br/SSEQTP_8.5.5/com.ibm.websphere.nd.multiplatform.doc/ae/cjta_glocons.html)
- [Local transaction containment](https://www.ibm.com/docs/pt-br/SSEQTP_8.5.5/com.ibm.websphere.nd.multiplatform.doc/ae/cjta_loctran.html)