# O que é Spring MVC?

[![img](https://secure.gravatar.com/avatar/98def809dd1ff0d88623373ddc725475?s=40&d=mm&r=g)](https://blog.algaworks.com/author/alexandre-afonso/)[Escrito por **Alexandre Afonso**](https://blog.algaworks.com/author/alexandre-afonso/)em 12/01/2017

![img](https://algaworks-blog.s3.amazonaws.com/wp-content/uploads/Spring-MVC.png)

Gostaria de facilidade e flexibilidade para trabalhar com requisições web? O **Spring MVC** é a ferramenta que dá isso para você!

Hoje é difícil conceber uma aplicação sem a parte web, concorda? Além das numerosas aplicações web, a maioria dos aplicativos móveis (para Android e iOS, por exemplo) precisam de uma APIs RESTful para consumir.

Por isso, conhecer um framework que ajuda nesse trabalho **é vantajoso para qualquer programador**.

Essa é a ideia de agora: explicar como o Spring MVC funciona. Continue lendo para **aprender sobre**:

- O que é Spring MVC?
- Criação de controladores web
- Novas anotações de mapeamento
- Recebendo dados de um formulário
- Enviando dados para a página
- Como configurar o `ViewResolver`
- Como usar o Spring MVC para criar uma API RESTful

Vamos lá?



## O que é Spring MVC?

<iframe loading="lazy" src="https://www.youtube.com/embed/8duNNsRhD9k?autohide=1" frameborder="0" allowfullscreen="allowfullscreen" name="fitvid0" id="_dytid_2968" style="box-sizing: border-box; -webkit-font-smoothing: antialiased; max-width: 100%; border: none; position: absolute; top: 0px; left: 0px; width: 750px; height: 421.875px;"></iframe>

O Spring MVC é um framework que ajuda no desenvolvimento de aplicações web. Com ele nós conseguimos construir **aplicações web robustas e flexíveis**.

Ele já tem todas as funcionalidades que precisamos para (1) atender as requisições HTTP, (2) delegar responsabilidades de processamento de dados para outros componentes e (3) preparar a resposta que precisa ser dada. É uma excelente implementação do **padrão MVC**.

MVC é acrônimo de Model, View e Controller, e é bacana entender o papel de cada um deles dentro do sistema. Esse entendimento vai te ajudar a trabalhar com Spring MVC de forma a construir aplicações mais organizadas e de fácil manutenção.

Vamos começar com a imagem abaixo que representa o fluxo de uma requisição do ponto de vista do Spring MVC:

[![Fluxo do Spring MVC](https://algaworks-blog.s3.amazonaws.com/wp-content/uploads/Fluxo-do-Spring-MVC.png)](https://algaworks-blog.s3.amazonaws.com/wp-content/uploads/Fluxo-do-Spring-MVC.png)

Detalhadamente, os passos mostrados na imagem acima, são:

\1. Acessamos uma URL no browser que envia a requisição HTTP para o servidor que roda a aplicação web com Spring MVC. Perceba que quem recebe a requisição é o controlador do framework, o Spring MVC.

\2. O controlador do framework irá procurar qual classe é responsável por tratar essa requisição, entregando a ela os dados enviados pelo browser. Essa classe faz o papel do controller.

\3. O controller passa os dados para o model, que por sua vez executa todas as regras de negócio, como cálculos, validações e acesso ao banco de dados.

\4. O resultado das operações realizadas pelo model é retornado ao controller.

\5. O controller retorna o nome da view, junto com os dados que ela precisa para renderizar a página.

\6. O Framework encontra a view que processa os dados, transformando o resultado em um HTML.

\7. Finalmente, o HTML é retornado ao browser do usuário.

Na prática, o *controller* é a classe Java com os métodos que tratam essas requisições. Portanto, tem acesso a toda informação relacionada a ela como parâmetros da URL, dados submetidos através de um formulário, cabeçalhos HTTP, etc.

Veja um exemplo simples:

```
@Controller``public` `class` `HomeController {` ` ``@RequestMapping``(``"/"``)`` ``public` `String index() {``  ``return` `"index.jsp"``;`` ``}``}
```

O método `index()` atende a requisição para `/` (que é a raiz da aplicação) e devolve a página `index.jsp` na resposta. Fique tranquilo, ainda veremos mais detalhes.

O *model* seria algo como as famosas classes de serviços, repositórios e as entidades de banco de dados. Um pequeno exemplo poderia ser:

```
@Autowired``private` `Funcionarios funcionarios; ``// <<< Nosso repositório.`` ` `@RequestMapping``(method = RequestMethod.GET)``public` `ModelAndView listar() {`` ``ModelAndView modelAndView = ``new` `ModelAndView(``"funcionario-lista.jsp"``);`` ``modelAndView.addObject(``"funcionarios"``, funcionarios.findAll());``  ` ` ``return` `modelAndView;``}
```

Não se assuste. Olhe o código acima com calma e no decorrer da nossa conversa isso ficará mais claro. Por ora, veja nosso *model* entrando em ação nas linhas 2 e 7.

Por fim temos nossa *view*. Essa dispensa exemplos aqui, pois, são nossas páginas JSP que irão tornar-se em HTML para chegar aos usuários.

## Criação de controladores web

Agora nós vamos estudar as opções que temos para criar e configurar nossos controladores. Contudo, vamos precisar de uma aplicação já configurada com o [Spring Framework](https://projects.spring.io/spring-framework/). Você pode escolher dentre duas opções na hora de configurar.

A primeira opção é utilizar a configuração de um projeto web comum. Um exemplo disso será o [código fonte que irei disponibilizar para você](https://github.com/algaworks/artigo-spring-mvc).

Você vai observar o pacote `com.algaworks.mvc`. Nele tem as classes de configuração que criei. Em especial, veja as classes `SpringWebInitializer` e `WebConfig`, pois, elas representam o mínimo para o Spring MVC funcionar.

Não deixe de olhar também o `pom.xml`. De qualquer forma vou deixar aqui o mínimo de dependências para trabalhar com Spring MVC:

```
<``dependency``>`` ``<``groupId``>org.springframework</``groupId``>`` ``<``artifactId``>spring-webmvc</``artifactId``>`` ``<``version``>4.3.4.RELEASE</``version``>``</``dependency``>` `<``dependency``>`` ``<``groupId``>javax.servlet</``groupId``>`` ``<``artifactId``>javax.servlet-api</``artifactId``>`` ``<``version``>3.1.0</``version``>`` ``<``scope``>provided</``scope``>``</``dependency``>` `<``dependency``>`` ``<``groupId``>javax.servlet</``groupId``>`` ``<``artifactId``>jstl</``artifactId``>`` ``<``version``>1.2</``version``>``</``dependency``>
```

A segunda (e melhor opção na minha opinião) é criar uma aplicação com o Spring Boot. Você pode aprender isso em detalhes através do [e-book gratuito de Spring da AlgaWorks](http://cafe.algaworks.com/livro-spring-boot/). Clique abaixo para fazer o download dele:

[![FN013-CTA-Lead-Magnet--Img02](https://algaworks-blog.s3.amazonaws.com/wp-content/uploads/FN013-CTA-Lead-Magnet-Img02.png)](http://cafe.algaworks.com/livro-spring-boot/)

Com o projeto configurado, podemos continuar. ;)

A primeira coisa é criar uma classe comum e anotá-la com `@Controller`. Essa anotação fará com que o Spring reconheça a classe como um controlador.

Depois disso criamos nossas ações. Elas são métodos Java comuns que, através de anotações, serão configuradas para responder as requisições web.

```
@Controller``@RequestMapping``(``"/funcionarios"``)``public` `class` `FuncionariosController {`` ` ` ``@Autowired`` ``private` `Funcionarios funcionarios;`` ` ` ``@RequestMapping``(method = RequestMethod.GET)`` ``public` `ModelAndView listar() {``  ``ModelAndView modelAndView = ``new` `ModelAndView(``"funcionario-lista.jsp"``);``  ``modelAndView.addObject(``"funcionarios"``, funcionarios.findAll());``  ` `  ``return` `modelAndView;`` ``}``}
```

Vamos começar a analisar o código acima linha a linha, mais especialmente a ação `listar()`, para entendermos tudo isso melhor:

Na primeira linha temos, como já falamos, a anotação que torna nossa classe um controlador. A segunda linha, com a anotação `@RequestMapping`, configura qual o *path* inicial para todas as ações do nosso controlador.

Já nas linhas 5 e 6 vemos nossa propriedade `funcionarios`, referente ao nosso repositório, anotada com `@Autowired` e, portanto, será injetada pelo Spring Framework. Nós temos aqui, no blog, [um artigo sobre injeção de dependências](https://blog.algaworks.com/injecao-de-dependencias-com-spring/) para quem quiser saber mais sobre o assunto. Essa parte não é diretamente relacionada ao Spring MVC.

Pulando para a linha 8, vemos novamente a anotação `@RequestMapping` que está especificando como será a requisição que a ação `listar()` vai atender.

```
@RequestMapping``(method = RequestMethod.GET)
```

A primeira coisa a se observar é que a propriedade `method` da nossa anotação está especificando um método HTTP que é o GET, ou seja, para a ação atender a requisição, a mesma deve ser do tipo GET.

A propriedade `method` pode ser omitida. Se assim fosse, a ação passaria a responder a todos os métodos HTTP (GET, POST, PUT, DELETE, etc.).

Veja também que a anotação não tem um *path* configurado. Isso quer dizer que o método `listar()` é a ação padrão do nosso controlador e vai responder para uma requisição do tipo GET com o *path* `/funcionarios`. Para configurar um, poderíamos utilizar as propriedades `value` ou `path` da anotação:

```
@RequestMapping``(method = RequestMethod.GET, path = ``"/lista"``)</pre>` `Agora o mais importante: nossa ação que começa na linha ``9``:``<pre ``class``=``"lang:java decode:true"` `title=``"Ação lista"``>``public` `ModelAndView listar() {`` ``ModelAndView modelAndView = ``new` `ModelAndView(``"funcionario-lista.jsp"``);`` ``modelAndView.addObject(``"funcionarios"``, funcionarios.findAll());``  ` ` ``return` `modelAndView;``}
```

Ela começa instanciando a classe `ModelAndView`. Essa classe foi utilizada para especificar a *view* que será renderizada para o usuário final e para informarmos quais os dados ela utilizará para isso.

Repare que no construtor temos a nossa página `funcionario-lista.jsp` e o método `addObject` é utilizado para adicionarmos uma lista de objetos que serão exibidos por ela.

Por fim, o objeto do tipo é `ModelAndView` retornado.

Você pode implementar essa ação de outra maneira se quiser. Veja uma alternativa:

```
public` `String lista(Model model) {`` ``model.addAttribute(``"funcionarios"``, funcionarios.findAll());``  ` ` ``return` `"funcionario-lista.jsp"``;``}
```

Neste caso, recebemos a interface `Model` como parâmetro (ela é passada pelo Spring MVC) e utilizamos o método `addAttribute` para adicionar nossa lista de objetos. A página foi, simplesmente, colocada como retorno.

## Recebendo dados de um formulário

Vou te mostrar agora como fazer com que os dados de um formulário cheguem até a ação do nosso controlador. Preparei um formulário, para cadastro de funcionários, e a ação que receberá essa submissão.

Como temos uma classe importante envolvida, que é a classe `Funcionario`, então quero mostrá-la pra você. São os dados dela que serão submetidos pelo formulário e recebidos pela ação:

```
public` `class` `Funcionario {` ` ``private` `Long id;`` ` ` ``private` `String nome;`` ` ` ``private` `String cpf;` ` ``//getters e setter omitidos``}
```

Agora tenho aqui o formulário construído em duas versões. Uma com as tags do Spring:

```
<``s:url` `value``=``"/funcionarios/salvar"` `var``=``"acao"` `/>` `<``sf:form` `method``=``"post"` `modelAttribute``=``"funcionario"` `action``=``"${acao}"``>`` ` `<``div``>``  ``<``label` `for``=``"nome"``>Nome:</``label``>``  ``<``sf:input` `path``=``"nome"` `/>`` ``</``div``>` ` ` `<``div``>``  ``<``label` `for``=``"cpf"``>CPF:</``label``>``  ``<``sf:input` `path``=``"cpf"` `/>`` ``</``div``>` ` ``<``button` `type``=``"submit"``>Salvar</``button``>``</``sf:form``>
```

E outra, para quem preferir, em HTML puro:

```
<``form` `id``=``"funcionario"` `action``=``"/app/funcionarios/salvar"` `method``=``"post"``>`` ` `<``div``>``  ``<``label` `for``=``"nome"``>Nome:</``label``>``  ``<``input` `id``=``"nome"` `name``=``"nome"` `type``=``"text"``/>`` ``</``div``>` ` ` `<``div``>``  ``<``label` `for``=``"cpf"``>CPF:</``label``>``  ``<``input` `id``=``"cpf"` `name``=``"cpf"` `type``=``"text"``/>`` ``</``div``>` ` ``<``button` `type``=``"submit"``>Salvar</``button``>``</``form``>
```

A vantagem do formulário com as tags Spring é que ele já irá exibir as informações do funcionário caso elas já venham preenchidas. Isso é útil no caso de edição de registros.

Já a ação para capturar os dados, referentes a submissão desse formulário, fica assim:

```
@Controller``@RequestMapping``(``"/funcionarios"``)``public` `class` `FuncionariosController {`` ` ` ``@Autowired`` ``private` `FuncionarioService funcionarioService;`` ` ` ``@RequestMapping``(method = RequestMethod.POST, path = ``"/salvar"``)`` ``public` `RedirectView salvar(``    ``Funcionario funcionario, ``    ``RedirectAttributes redirectAttributes) {``  ``funcionarioService.salvar(funcionario);``  ` `  ``redirectAttributes.addFlashAttribute(``        ``"mensagem"``, ``"Cadastro feito com sucesso!"``);``  ` `  ``return` `new` `RedirectView(``"/funcionarios"``, ``true``);`` ``}``}
```

Da forma como fizemos com a ação `listar()`, deixa eu mostrar agora a ação `salvar()`.

Nossa ação está recebendo dois parâmetros sendo que o primeiro – do tipo `Funcionario` – será preenchido com os dados que virão do formulário e o segundo – do tipo `RedirectAttributes` – utilizaremos para incluir o atributo com a mensagem que será exibida após o redirecionamento.

Com nossa instância do tipo `Funcionario` preenchida podemos manipulá-la como quisermos. No caso, simplesmente, salvamos ela:

```
funcionarioService.salvar(funcionario);
```

Como já dei a entender, não será renderizada uma página após essa ação, será feito um redirecionamento. Para isso, foi utilizado um objeto do tipo `RedirectView`.

```
new` `RedirectView(``"/funcionarios"``, ``true``);
```

O primeiro argumento do construtor desse objeto é a URL, ou *path* de destino e o segundo é somente para quando utilizamos um *path*. Passando `true`, a classe entende que é para incluir o contexto de nossa aplicação ao *path* informado. O resultado seria algo como: http://localhost:8080/app/funcionarios.

Acima mostrei o formulário e a ação que recebe os dados, mas qual ação criar para imprimir o formulário para o usuário? Pode usar essa daqui:

```
@RequestMapping``(method = RequestMethod.GET, path = ``"/novo"``)``public` `ModelAndView novo() {`` ``ModelAndView modelAndView = ``new` `ModelAndView(``"funcionario-formulario.jsp"``);`` ``modelAndView.addObject(``"funcionario"``, ``new` `Funcionario());``  ` ` ``return` `modelAndView;``}
```

E, se o seu formulário for em HTML puro, pode trocar por essa:

```
@RequestMapping``(method = RequestMethod.GET, path = ``"/novo"``)``public` `String novo() {  `` ``return` `"funcionario-formulario.jsp"``;``}
```

## Enviando dados para a página

Como já sabemos receber dados de um formulário, vamos entender como enviar dados para as nossas páginas.

Na verdade, a gente já fez isso quando utilizamos os métodos `addAttribute` da interface `Model`, `addObject` da classe `ModelAndView` e até no método `addFlashAttribute` da classe `RedirectAttributes`. Mas agora quero fazer isso formalmente simulando a função de edição.

Aqui o formulário construído com as tags do Spring MVC é importante, pois, já vai mostrar os dados do objeto que a gente passar para ser utilizado na nossa *view*.

Quanto ao formulário, ele teve uma leve mudança para incluir também o campo *hidden* que irá armazenar o identificador do funcionário (veja linha 8):

```
<``s:url` `value``=``"/funcionarios/salvar"` `var``=``"acao"` `/>` `<``sf:form` `method``=``"post"` `modelAttribute``=``"funcionario"` `action``=``"${acao}"``>`` ``<``c:if` `test``=``"${not empty funcionario.id}"``>``  ` `<``div``>``   ``<``label``>Código:</``label``>``   ` `${funcionario.id}` `   ``<``sf:hidden` `path``=``"id"` `/>``  ``</``div``>` ` ``</``c:if``>``  ` ` ` `<``div``>``  ``<``label` `for``=``"nome"``>Nome:</``label``>``  ``<``sf:input` `path``=``"nome"` `/>`` ``</``div``>` ` ` `<``div``>``  ``<``label` `for``=``"cpf"``>CPF:</``label``>``  ``<``sf:input` `path``=``"cpf"` `/>`` ``</``div``>` ` ``<``button` `type``=``"submit"``>Salvar</``button``>``</``sf:form``>
```

Repare que a ação que o formulário acima vai chamar, ao ser submetido, continua sendo a mesma que apresentei pra você antes, ou seja, a ação `salvar()`. Isso não vai mudar.

Agora vou mostrar a ação `edicao()` que vai ser chamada para abrir esse formulário. Ela é praticamente igual a ação `novo()` que já vimos também. Olhe só:

```
@RequestMapping``(method = RequestMethod.GET, path = ``"/edicao"``)``public` `ModelAndView edicao(``@RequestParam` `Long id) {`` ``ModelAndView modelAndView = ``new` `ModelAndView(``"funcionario-formulario.jsp"``);`` ``modelAndView.addObject(``"funcionario"``, funcionarios.findOne(id));``  ` ` ``return` `modelAndView;``}
```

Basta invocarmos essa ação passando o identificador do funcionário que queremos editar para que ele faça a pesquisa do mesmo e devolva para nossa *view* preparar o formulário que será enviado ao *browser*.

Um exemplo de URL para invocação seria: http://localhost:8080/app/funcionarios/edicao?id=1.

A única coisa de novo que a nova ação tem é a anotação `@RequestParam`. Como podemos ver, ela nos ajuda a ter acesso aos parâmetros da requisição de uma maneira mais simples.

Uma observação aqui é que, se o parâmetro que vem pela URL for diferente do parâmetro da ação, então é preciso especificá-lo:

```
public` `ModelAndView edicao(``@RequestParam``(``"id"``) Long funcionario) { ... }
```

Outra forma para anotar nosso método que deixaria a URL da página mais elegante, seria essa aqui:

```
@RequestMapping``(method = RequestMethod.GET, path = ``"/{id}/edicao"``)``public` `ModelAndView edicao(``@PathVariable` `Long id) { ... }
```

Não coloquei a implementação do método acima, pois, ela não se altera. O que muda é a URL para invocar a ação com esse novo mapeamento: http://localhost:8080/app/funcionarios/1/edicao. Quanto a anotação `@PathVariable`, veremos ela ainda mais uma vez.

Tem mais uma questão que envolve esse tópico aqui como um todo que é o seguinte:

O Spring MVC é um framework *action-based* e isso é pelo fato da própria ação enviar os dados para serem utilizados na página. Como fizemos aqui, enviando o objeto `Funcionario` para ser editado.

A ação recebe a requisição e ela mesma já trata de disponibilizar a informação (que seriam os objetos Java) que vai dinamizar as páginas que serão exibidas para o usuário final.

## Novas anotações de mapeamento

A partir de versão 4.3.X do Spring Framework já temos disponíveis as seguintes anotações:

- @GetMapping
- @PostMapping
- @PutMapping
- @DeleteMapping
- @PatchMapping

Podemos utilizá-las como alternativa a anotação @RequestMapping. Ao invés de utilizar assim:

```
@RequestMapping``(method = RequestMethod.GET, path = ``"/novo"``)
```

Usamos assim:

```
@GetMapping``(``"/novo"``)
```

É melhor porque além de não termos de configurar a propriedade `method`, melhora também a semântica dos nossos controladores.

Veja como nosso controlador fica melhor com essas anotações:

```
@Controller``@RequestMapping``(``"/funcionarios"``)``public` `class` `FuncionariosController {`` ` ` ``...`` ` ` ``@GetMapping`` ``public` `ModelAndView listar() { ... }`` ` ` ``@GetMapping``(``"/novo"``)`` ``public` `ModelAndView novo() { ... }`` ` ` ``@GetMapping``(``"/edicao"``)`` ``public` `ModelAndView edicao(``@RequestParam` `Long id) { ... }`` ` ` ``@PostMapping``(``"/salvar"``)`` ``public` `RedirectView salvar(Funcionario funcionario, ``                ``RedirectAttributes redirectAttributes) { ... }``}
```

## Como configurar o `ViewResolver`

`ViewResolver `é uma interface que ajuda o Spring MVC a encontrar as páginas que são retornadas por nossas ações. Para o código-fonte de exemplo que estou utilizando aqui (e irei disponibilizar para você) eu utilizei uma configuração diferente da padrão.

A resolução padrão das páginas é feita em conjunto com a URL da requisição. No caso de, por exemplo, utilizarmos a URL http://localhost:8080/app/funcionarios/novo para invocar nossa ação `novo()`:

```
@GetMapping``(``"/novo"``)``public` `String novo() {  `` ``return` `"funcionario-formulario.jsp"``;``}
```

Então, o Spring MVC iria procurar pela página `funcionario-formulario.jsp` em um diretório da nossa aplicação web chamada “funcionarios”.

A configuração que utilizei é um pouco diferente. Foi estipulado a pasta “/WEB-INF/paginas/” para ser o diretório raiz de nossas páginas.

Se quiséssemos um subdiretório com o nome “funcionarios”, teríamos que fazer o seguinte:

```
@GetMapping``(``"/novo"``)``public` `String novo() {  `` ``return` `"funcionarios/funcionario-formulario.jsp"``;``}
```

Assim o Spring MVC vai procurar nosso arquivo dentro da pasta “/WEB-INF/paginas/funcionarios/”.

A configuração foi feita através do Java. Veja como ficou:

```
@Configuration``public` `class` `ViewResolverConfig {``  ` ` ``@Bean`` ``public` `ViewResolver internalResourceViewResolver(){``  ``InternalResourceViewResolver viewResolver = ``new` `InternalResourceViewResolver();``    ` `  ``viewResolver.setViewClass(JstlView.``class``);``  ``viewResolver.setPrefix(``"/WEB-INF/paginas/"``);``  ``viewResolver.setViewNames(``new` `String[] {``"*.jsp"``});``    ` `  ``return` `viewResolver;`` ``}``}
```

## Como usar o Spring MVC para criar uma API RESTful

Quero terminar falando um pouco sobre os recursos que temos no Spring MVC para criarmos APIs RESTful.

Mas não quero aqui falar sobre arquitetura e nem de boas práticas de APIs RESTful. Vou, simplesmente, mostrar o caminho de como fazer isso, beleza?

Imagine que você precise retornar todos os funcionários da sua base no formado JSON. Olhe só como ficaria:

```
@Controller``@RequestMapping``(``"/funcionarios"``)``public` `class` `FuncionariosController {` ` ``...`` ` ` ``@ResponseBody`` ``@GetMapping``(``"/todos"``)`` ``public` `List<Funcionario> todos() {``  ``return` `funcionarios.findAll();`` ``}``}
```

Veja que já devolvemos a lista de funcionários diretamente. Isso é porque estamos utilizando a anotação `@ResponseBody`. Ela diz ao Spring MVC para jogar o retorno do método na resposta.

E antes de fazer uma requisição para nossa nova ação, que vai retornar a resposta em JSON, não podemos esquecer de adicionar a seguinte dependência em nosso `pom.xml`:

```
<``dependency``>`` ``<``groupId``>com.fasterxml.jackson.core</``groupId``>`` ``<``artifactId``>jackson-databind</``artifactId``>`` ``<``version``>2.8.5</``version``>``</``dependency``>
```

Aqueles que optaram por uma [configuração com o Spring Boot](http://cafe.algaworks.com/livreto-spring-mvc/), não precisaram adicionar a dependência acima, pois, ele já faz isso por você.

Agora sim podemos fazer uma requisição para http://localhost:8080/app/funcionarios/todos. Teríamos um JSON de retorno com os funcionários parecido com este:

```
[`` ``{``  ``"id"``: 1,``  ``"nome"``: ``"João"``,``  ``"cpf"``: ``"11111111111"`` ``},`` ``{``  ``"id"``: 2,``  ``"nome"``: ``"Maria"``,``  ``"cpf"``: ``"11111111112"`` ``},`` ``{``  ``"id"``: 3,``  ``"nome"``: ``"Mateus"``,``  ``"cpf"``: ``"11111111113"`` ``}``]
```

Melhor ainda se criarmos um novo controlador e, ao invés de anotá-lo com `@Controller`, usarmos `@RestController`. Veja como ficaria:

```
@RestController``@RequestMapping``(``"/api/funcionarios"``)``public` `class` `FuncionariosResource {`` ` ` ``@Autowired`` ``private` `Funcionarios funcionarios;`` ` ` ``@GetMapping`` ``public` `List<Funcionario> todos() {``  ``return` `funcionarios.findAll();`` ``}``}
```

A URL para invocar a ação `todos()`, nesse novo controlador, fica assim agora: http://localhost:8080/app/api/funcionarios.

Repare que, como estamos utilizando a anotação `@RestController`, nosso método não precisou da anotação `@ResponseBody`. Para o caso da construção de APIs, essa última opção com `@RestController`, fica mais elegante. :)

Vamos a um último exemplo para utilizarmos, mais uma vez, a anotação `@PathVariable`. Essa é uma anotação que serve para pegarmos informações do *path* da nossa ação

```
@RestController``@RequestMapping``(``"/api/funcionarios"``)``public` `class` `FuncionariosResource {`` ` ` ``@Autowired`` ``private` `Funcionarios funcionarios;``  ` ` ``@GetMapping``(``"/{id}"``)`` ``public` `Funcionario buscar(``@PathVariable` `Long id) {``  ``return` `funcionarios.findOne(id);`` ``}` `}
```

O que vai acontecer agora com nossa ação `buscar()` é que, quando fizermos uma requisição para http://localhost:8080/app/api/funcionarios/1 o parâmetro `id`, especificado na anotação `@GetMapping`, será passado para nossa ação.

Veja o resultado dessa última requisição:

```
{``"id"``:1,``"nome"``:``"João"``,``"cpf"``:``"11111111111"``}
```

## Conclusão: Spring MVC é um framework web cheio de recursos

Espero ter passado a você um boa impressão do Spring MVC, pois, é um excelente framework web, cheio de recursos.

Vimos aqui a estrutura de um controlador e como configurar nossas ações. Depois mostrei as novas anotações de mapeamento para as ações.

Ensinei como customizar a configuração do `ViewController`.

Expliquei como receber dados de um formulário e como enviar objetos para serem utilizados em nossas páginas JSP.

Por último, vimos como utilizar o Spring MVC para construir uma API RESTful.

Espero que tenha gostado. ;)

Caso você esteja começando com Spring MVC e queira dar mais um passo, [baixe o e-book de Spring](http://cafe.algaworks.com/livro-spring-boot/) da AlgaWorks. Basta clicar abaixo agora:

[![FN013-CTA-Lead-Magnet--Img02](https://algaworks-blog.s3.amazonaws.com/wp-content/uploads/FN013-CTA-Lead-Magnet-Img02.png)](http://cafe.algaworks.com/livro-spring-boot/)

Um abraço pra você até uma próxima!

**PS**: você pode baixar o código-fonte de exemplo em nosso GitHub: [http://github.com/algaworks/artigo-spring-mvc](https://github.com/algaworks/artigo-spring-mvc).