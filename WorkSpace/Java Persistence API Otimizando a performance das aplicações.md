# Java Persistence API: Otimizando a performance das aplicações

## Veja neste artigo como podemos melhorar a performance das aplicações Java que utilizam a JPA (Java Persistence API) para acesso a dados.

- [Artigos](https://www.devmedia.com.br/artigos/)[Java](https://www.devmedia.com.br/artigos/java)Java Persistence API: Otimizando a performance das aplicações

O desempenho da [JPA (Java Persistence API)](http://www.devmedia.com.br/java-persistence-api-jpa-primeiros-passos/30511) é afetada diretamente pelo [desempenho do driver JDBC](http://www.devmedia.com.br/java-jdbc-melhorando-o-desempenho-de-aplicacoes-java/31976) que a API utiliza, por isso as considerações de desempenho feitas ao JDBC se aplicam a JPA também. Além disso, a JPA tem considerações adicionais de desempenho.

A JPA alcança um bom desempenho alterando o *bytecode* de classes de entidade (*Entity Class*). Isso é realizado de forma transparente no ambiente **Java EE**. Em um ambiente **Java SE** é muito importante nos certificarmos que o processamento do *bytecode* está configurado corretamente. Caso não esteja corretamente configurado, o desempenho do aplicativo *JPA* será imprevisível, onde campos que deveriam ser carregados mais tardiamente (*[lazy load](http://www.devmedia.com.br/lazy-e-eager-loading-com-hibernate/29554)*) podem ser carregados de forma antecipada, dados salvos na base de dados poderiam ser redundantes, informações que deveriam estar no cache da JPA podem precisar ser buscados novamente na base de dados, entre outros problemas.

Na JPA não existe uma forma definida dos *bytecodes* serem processados. Normalmente, isto é feito como parte da compilação, ou seja, após as classes de entidades serem compiladas e antes que elas sejam carregadas em arquivos *JAR* ou executados pela *[JVM](http://www.devmedia.com.br/entenda-como-funciona-a-java-virtual-machine-jvm/27624)*, eles são transmitidos através de um pós-processador específico de implementação que "melhora" o *bytecode*, produzindo um arquivo de classe alterada com as otimizações desejadas. Algumas implementações da *JPA* fornecem algumas formas de melhorar o *bytecode* dinamicamente quando as classes são carregadas na *JVM*. Essa forma dinâmica requer um agente executando na *JVM* que é notificado quando as classes são carregadas. O agente intervém sobre o carregamento da classe e altera os *bytes* antes que eles sejam usados para definir a classe. O agente é especificado na linha de comando da aplicação. Por exemplo, no EclipseLink podemos incluir o argumento "-javaagent:path_to/eclipselink.jar" para ativar o agente.

No restante do artigo veremos diversas formas de otimizar nossa aplicação que utiliza a API JPA.

### Gerenciando Transações

A JPA pode ser utilizada tanto por aplicações *Java SE* quanto por *Java EE*. A plataforma em uso afeta a forma como as transações *JPA* são gerenciadas.

Na plataforma *Java EE*, as transações *JPA* são parte da implementação [*Java Transaction API (JTA)*](http://www.devmedia.com.br/java-transaction-api-jta-na-plataforma-java-ee-7/31820) do servidor de aplicação. Com isso temos duas opções de como a transação é gerenciada: o servidor de aplicação pode cuidar da transação (usando *Container Managed Transactions* ou *CMT*), ou a transação pode ser explicitamente programada na aplicação (usando *User Managed Transactions* ou *UMT*).

Não existem diferenças significativas na performance entre *CMT* e *UMT* se eles forem utilizados de forma equivalente. Contudo, nem sempre é possível usá-los de forma equivalente.

Em particular, transações *UMT* podem ter um escopo maior ou menor que *CMT*, na qual pode acarretar um impacto significativo na performance.

Segue na **Listagem 1** um exemplo de como podemos utilizar CMT.

**Listagem 1.** Utilizando um escopo de transação CMT.



```
@Stateless
public class Calculadora {
         @PersistenceContext(unitName="Calculadora")
         EntityManager em;

         @TransactionAttribute(REQUIRED)
         public void calcula() {
                   Parameters p = em.find(...);
                   ...executa diversos calculos custosos...
                   em.persist(respostaCalculos);
         }
}
```



O escopo de transação aqui (usando CMT) é o método inteiro. Se o método requer leituras repetíveis para a informação que está sendo persistida, então a informação na tabela será bloqueada durante o cálculo custoso que está sendo processado no método.

Se utilizarmos *User Managed Transactions*, teríamos mais flexibilidade conforme o código da **Listagem 2.**

**Listagem 2.** Utilizando um escopo de transação *UMT*.



```
@Stateless
public class Calculadora {
         @PersistenceContext(unitName="Calculadora")
         EntityManager em;

         public void calcula() {
                   UserTransaction ut = ... pesquisa o UT no servidor de aplicacao...;
                   ut.begin();
                   Parameters p = em.find(...);
                   ut.commit();
                   ... executa diversos calculos custosos...
                   ut.begin();
                   em.persist(respostaCalculos);
                   ut.commit();
         }
}
```



Dividindo a transação, que pode ser realizado mais facilmente apenas com *UMT*, podemos limitar o efeito da transação sobre a aplicação.

Para resolver este problema utilizando transações *CMT* poderíamos dividir o trabalho em três métodos diferentes, cada um deles teria um atributo de transação diferente. Em geral, utilizar *UMT* é mais conveniente nesses casos.

Da mesma forma, um [*servlet*](http://www.devmedia.com.br/java-servlets-introduzindo-o-webservlet/30247) usando transações *UMT* poderia estabelecer um limite de transação que abrangeria várias chamadas para um *EJB*. Porém, se utilizássemos um *CMT* para conseguir o mesmo efeito significaria adicionarmos na interface *EJB* um novo meta-método que chama esses outros métodos dentro da mesma transação.

Em aplicações *Java SE*, o *EntityManager* é responsável por fornecer o objeto de transação, mas a aplicação é responsável por demarcar os limites da transação neste objeto. Segue na **Listagem 3** um exemplo.

**Listagem 3.** Demarcando transações utilizando um objeto do *EntityManager*.



```
public void processa() {
         for (int i = estoqueInicial; i < estoqueTotal; i++) {

                   EntityManager em = emf.createEntityManager();
                   EntityTransaction txn = em.getTransaction();
                   txn.begin();

                   while (!dataAtual.after(dataFinal)) {
                            PrecoEstoque pe = criaEstoqueRandomico(dataAtual);

                            if (pe != null) {
                                      em.persist(pe);
                                      for (int j = 0; j < 5; j++) {
                                               PrecoItemEstoqueImpl ipe =
                                               criaItemRandomico(pe.getSimbolo(), pe.getDate());
                                               em.persist(ipe);
                                      }
                            }

                   }

                   txn.commit();
                   em.close();
         }
}
```



### Transações XA

Entidades *JPA* podem frequentemente ser envolvidas em uma transação *XA*, ou seja, uma transação que usa mais do que um recurso de banco de dados, ou um banco de dados e outro recurso de transação como um recurso *JMS*.

Confirmar (*commit*) transações com diferentes recursos transacionais é uma operação bastante custosa. O algoritmo que executa isso implementa o *eXtended Architecture*, ou o *XA*, que é a arquitetura padrão para confirmação de informações. Esta operação é muito inteligente e muito complexa, que exige múltiplas trocas entre os recursos envolvidos na transação. A maioria dos servidores de aplicação [*Java EE*](http://www.devmedia.com.br/curso/curso-de-java-ee-construa-uma-aplicacao-completa-java-ee/403) permitirão otimizações internas nesses tipos de situações. Essas otimizações são conhecidas como *Last Agent Optimization (LAO)*, *Logging Last Resource (LLR)*, *Last Resource Commit Optimization (LRCO)*, *Last Resource Gambit (LRG)*, entre outros.

### Otimizando Escritas JPA

Quando utilizamos *JDBC* verificarmos duas técnicas de performance críticas que é o reuso dos *Prepared Statement* e realizar atualizações em lotes. Isso também é possível de ser realizado com *JPA*, porém a forma como podemos fazer isso depende da implementação da *JPA* em uso. Não existe nenhuma chamada para a *API JPA* para que possamos fazer isso. Para aplicações *Java SE*, essas otimizações requerem apenas configurarmos uma propriedade no arquivo *persistence.xml*.

Por exemplo, no *EclipseLink*, podemos reutilizar o *statement* adicionando a seguinte propriedade no arquivo *persistence.xml*:



```
<property name="eclipselink.jdbc.cache-statements" value="true" />
```



Com isso o reuso de *statement* já está habilitado no *EclipseLink*. Vale ressaltar que se o driver *JDBC* permite o *pool* de *statement*, é preferível habilitarmos o *cache* no *driver* e deixar a propriedade desabilitada nas configurações da *JPA*.

O *Statement* em lotes no *EclipseLink* se dá adicionando as propriedades abaixo:



```
<property name="eclipselink.jdbc.batch-writing" value="JDBC" />
  <property name="eclipselink.jdbc.batch-writing.size" value="10000" />
```



Os drivers *JDBC* não podem automaticamente implementar o *Statement* em lotes, por isso esta propriedade é bem útil de ser configurada em todos os casos. O tamanho do lote pode ser controlado de duas formas: primeiro, o tamanho da propriedade pode ser configurada, como realizado no exemplo acima, e segundo a aplicação pode periodicamente chamar o método *flush()* do *EntityManager*, que fará com que todas as declarações em lote sejam executadas imediatamente.

### Otimizando Leituras JPA

Otimizar quando e como *JPA* lê as informações da base de dados é muito mais complicado do que parece. Isso porque a *JPA* coloca as informações em cache esperando que este dado possa ser utilizado no futuro. Apesar dessa abordagem ser interessante para o desempenho, o [SQL gerado](http://www.devmedia.com.br/linguagem-sql-extraindo-informacao-de-qualidade-com-sql/31396) não é o ideal.

A recuperação dos dados é otimizada para atender às necessidades do cache JPA, ao invés de ser otimizado para qualquer requisição particular que está em andamento.

A *JPA* lê as informações da base de dados através de três formas: quando o método *find()* do *Entity Manager* é chamado, quando uma consulta *JPA* é executada, e quando o código navega para uma nova entidade por meio da relação de uma entidade existente.

O caso mais simples é utilizar o método *find()*, pois apenas uma única linha é envolvida, e essa linha única é lida da base de dados. A única coisa que pode ser controlada aqui é o quanto de informação será retornada. A *JPA* pode retornar apenas alguns campos na linha, a linha inteira, ou pode buscar antecipadamente outras entidades que estão relacionadas com a linha que está sendo recuperada.

Essas otimizações aplicam-se a consultas também. Existem dois caminhos possíveis: ler menos informação possível porque não serão necessárias, ou ler mais informações porque esses dados serão necessários no futuro. Esses casos serão analisados nas próximas sessões.

### Lendo menos Dados

Para ler menos dados, devemos especificar que o campo em questão deve ser carregado mais tardiamente o que também é conhecido como *lazy load*. Quando uma entidade é retornada, os campos com a anotação *lazy* serão excluídos do *SQL* usado para carregar a informação. Se o método *getter* de um campos é executado, significa que a *JPA* deverá ir até o banco de dados para recuperar essa informação. Essa anotação não precisa ser utilizada nas colunas simples de tipos básicos, mas devemos considerar fortemente a sua utilização se a entidade contém objetos *BLOB*.

Segue na **Listagem 4** um exemplo de como utilizar a anotação *LAZY* para um *BLOB*

**Listagem 4**. Utilizando *LAZY* para um campo *BLOB*.



```
@Lob
@Column(name = "IMAGEM")
@Basic(fetch = FetchType.LAZY)
private byte[] imagem;
```



Essa informação binária é bastante grande e não deveria ser carregada até que seja necessária a sua utilização. Com isso temos um *SQL* mais rápido e menos memória será utilizada, o que consequentemente causará menos *GC*.

Por outro lado, talvez alguns dados devem ser pré-carregados. Por exemplo, quando uma entidade possui outras entidades que devem ser buscadas junto para completar uma informação que será exibida ou manipulada. Nesse caso, poderíamos apenas utilizar a anotação *EAGER* conforme mostra o exemploda **Listagem 5.**

**Listagem 5**. Utilizando EAGER para buscar dados antecipadamente.



```
@OneToMany(mappedBy="estoque", fetch=FetchType.EAGER)
private Collection<EstoqueImpl> estoques;
```



Por padrão, entidades relacionadas são buscadas antecipadamente se o tipo do relacionamento é *@OneToOne* ou *@ManyToOne*. Para otimizar isso deveríamos utilizar *FetchType.LAZY*. É interessante sabermos que o *SQL* gerado através de relacionamentos com *EAGER* não resulta em um simples *JOIN*. Nesse caso, a implementação da *JPA* realizará uma busca pelo objeto primário e então um ou mais comandos *SQL* são utilizados para buscar objetos relacionados. O método *find()* não oferece um controle sobre isso, sendo assim teríamos que usar uma consulta e programar um *JOIN* nessa consulta conforme veremos na próxima seção.

### Usando JOIN nas Consultas

A *JPQL* não permite especificarmos campos de um objeto para serem retornados. Segue abaixo um exemplo de uma consulta usando *JPQL*:



```
Query q = em.createQuery("SELECT e FROM EstoqueImpl e");
```



Essa consulta terá o equivalente abaixo em *SQL*:



```
SELECT <lista de campos não-LAZY> FROM EstoqueImpl
```



Para retornar menos campos no *SQL* gerado não temos muitas opções a não ser marcar os campos com *LAZY*. No entanto, se esses campos estiverem marcados com *LAZY* não temos nenhuma forma de retorna-los na consulta.

Se existem relacionamentos entre as entidades, as entidades podem ter um *JOIN* explicito na consulta *JPQL*, assim serão retornadas as entidades iniciais e as relacionadas de uma única vez. Segue na **Listagem 6** um exemplo de como podemos realizar esse *JOIN* utilizando *JPQL*:

**Listagem 6**. Utilizando *JOIN* utilizando *JPQL*.



```
Query q = em.createQuery("SELECT e FROM EstoqueImpl s JOIN FETCH e.itensEstoque");
```



O SQL equivalente é similar ao da **Listagem 7.**

**Listagem 7**. *SQL* equivalente a consulta *JPA* acima.



```
SELECT t1.<campos>, t0.<campos> FROM EstoqueImpl t0, ItensEstoqueImpl t1
WHERE t0.id = t1.id
```



Diferentes fornecedores de *JPA* podem retornar diferentes resultados.

O *JOIN FETCH* é válido para relacionamentos entre entidades, mesmo que eles estejam anotados com *EAGER* ou *LAZY*.

Se o *JOIN* é realizado em um relacionamento *LAZY*, as entidades anotadas com *LAZY* que satisfazem a pesquisa são retornadas da base de dados, e se essas entidades são utilizadas mais tarde, nenhuma pesquisa adicional ao banco de dados é necessária.

Quando todos os dados retornados por uma consulta usando *JOIN FETCH* é utilizado, então o *JOIN FETCH* na maioria das vezes oferecerá uma grande melhoria na performance.

### Batching e Consultas

Consultas *JPA* são gerenciadas como uma consulta *JDBC* produzindo um conjunto de resultados. A implementação *JPA* tem a opção de retornar todos os resultados de uma vez, obter os resultados um de cada vez conforme a aplicação itera sobre os resultados da consulta, ou obter alguns resultados de cada vez.

Não existe uma forma padrão de controlar isto, mas os fornecedores de *JPA* possuem mecanismos proprietários para configurar o tamanho da busca. No *EclipseLink*, podemos configurar conforme abaixo:



```
q.setHint("eclipselink.JDBC_FETCH_SIZE", "100000");
```



O Hibernate oferece uma anotação customizada chamada *@BatchSize*.

Se uma grande quantidade de informação está sendo processada, o código pode ter de percorrer a lista retornada pela consulta. Isso tem uma relação natural com a forma como os dados podem ser exibidos para o usuário em uma página da web onde um sub-conjunto de informações é exibido (100 linhas), juntamente com links "Próximo" e "Anterior" na página para navegar através dos dados.

Isto pode ser realizado através da criação de um intervalo na consulta conforme a **Listagem 9.**

**Listagem 9**. Filtrando dados a serem exibidos.



```
Query q = em.createNamedQuery("selectAll");
query.setFirstResult(101);
query.setMaxResults(100);
List<? implements StockPrice> = q.getResultList();
```



Esta consulta retorna uma lista para ser exibida na segunda página da aplicação web, do 101 até o 200. Com isso recuperamos apenas o intervalo de dados necessários que será mais eficiente do que a recuperação de 200 linhas e rejeitando os primeiros 100 deles.

Podemos verificar que estamos usando uma *named query* através do método createNamedQuery() ao invés de uma consulta direta usando createQuery().

Na maioria das implementações *JPA*, os *named queries* são mais rápidos, pois a *JPA* quase sempre utilizará um Prepared Statement com parâmetros *bind*, utilizando o *pool* de *Statement*.

### JPA Caching

Um dos casos mais comuns de uso relacionados com o desempenho de *Java* é fornecer uma camada intermediária que armazena os dados dos recursos de banco de dados. A camada *Java* executa um número de funções úteis arquiteturalmente. Para uma melhor performance é interessante fazermos cache na camada *Java*, o que fornecerá uma resposta mais rápida aos clientes.

A *JPA* tem dois tipos de *cache*. Cada instância de um *Entity Manager* tem seu próprio *cache*, ou seja, localmente ele faz um *cache* dos dados que são retornados durante a transação e também os dados que são escritos durante a transação. Um programa pode ter muitas instâncias de *Entity Manager*, cada um executando uma transação diferente, e cada um com seu próprio *cache* local.

Quando um *Entity Manager* confirma (*commit*) uma transação, todas informações no *cache* local podem ser mesclados em um *cache* global. O cache global é compartilhado entre todos os *Entity Managers* na aplicação. O *cache* global também é conhecido como "*Level 2 (L2) cache*" ou *cache* de segundo nível. O cache na *Entity Manager* é conhecido como "*Level 1 (L1) cache*" ou *cache* de primeiro nível.

O *cache L1* está habilitado em todas as implementações *JPA*. Porém, a *cache L2* apesar de ser fornecida na maioria das implementações *JPA*, não está habilitada na maioria por padrão.

A *cache* da *JPA* opera apenas nas entidades acessadas pelas suas chaves primárias, ou seja, itens retornados através de chamadas ao método *find()*, ou através de itens retornados por uma entidade relacionada (*EAGER*).

uando o *Entity Manager* tenta localizar um objeto, quer através de sua chave primária ou um mapeamento de relacionamentos, ele pode fazer uma pesquisa primeiro na *cache L2* e retornar o(s) objeto(s) se forem encontrados lá, poupando assim uma ida até o banco de dados.

Os itens retornados através de uma consulta não são colocadas na *cache L2*. Algumas implementações *JPA* possuem um mecanismo específico de acordo com o fornecedor para o armazenamento dos resultados de uma consulta em *cache*, mas esses resultados apenas são reusados se a mesma consulta for executada novamente.

Algumas implementações da *JPA* têm um mecanismo específico do fornecedor para armazenar em *cache* os resultados de uma consulta, mas esses resultados somente são reutilizados se a mesma consulta é executada novamente.

Mesmo que a implementação da JPA suporte o *cache* de uma consulta, as entidades não são armazenadas na *L2* e não podem ser retornadas em uma chamada subsequente do método *find()*.

**Bibliografia**

[1] G. Arun, Java EE 7 Essentials. O’Reilly, 2013.

[2] Oaks, S. Java Performance: The Definitive Guide. O'Reilly, 2014.

Tecnologias:

- Hibernate
- [Java](https://www.devmedia.com.br/java/)
- JPA
- [SQL](https://www.devmedia.com.br/sql/)



