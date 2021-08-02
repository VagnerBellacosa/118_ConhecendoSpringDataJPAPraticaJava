**Docker - Criando uma imagem com Dockerfile**

------

| ![img](http://www.macoratti.net/mac3_1.jpg) | Neste artigo vou apresentar os conceitos básicos relativos ao Docker mostrando como criar uma imagem usando o Dockerfile. | ![img](http://www.macoratti.net/19/01/intro_docker12.png) |
| ------------------------------------------- | ------------------------------------------------------------ | --------------------------------------------------------- |
|                                             |                                                              |                                                           |

Se você não conhece o Docker sugiro que acompanhe a série de artigos : [Docker uma introdução básica](http://www.macoratti.net/19/01/intro_docker1.htm)

Já sabemos que podemos criar contêineres a partir de imagens prontas que podemos baixar do Docker registry.

Mas e se quisermos criar nossas próprias imagens ? Como podemos fazer ?

| ![img](http://www.macoratti.net/19/01/linux1.png) | [![img](http://www.macoratti.net/19/01/banner_docker1.jpg)](http://www.macoratti.net/curso_docker1.htm) |
| ------------------------------------------------- | ------------------------------------------------------------ |
|                                                   |                                                              |

Para criar uma imagem podemos usar um arquivo Dockerfile.

Um Dockerfile é um arquivo texto que descreve as etapas que o Docker precisa para preparar uma imagem, incluindo a instalação de pacotes, criação de diretórios e definição de variáveis de ambiente entre outras coisas.

Vamos propor um exemplo bem simples: Você deseja criar uma imagem baseada na imagem do debian onde vai instalar o nginx e vai exibir uma página html.

**Nota1**: O Nginx é um servidor HTTP e proxy reverso, bem como um servidor para proxy de email IMAP/POP3, escrito por Igor Sysoev desde 2005.

**Nota2**: O Debin é uma Distribuição Linux.

Neste artigo eu vou usar o Docker no Linux e para isso eu usei o VirtualBox da Oracle no Windows 10 Pro e crie uma maáquina virtual Linux onde instalei o Ubuntu 18.04.

Para saber instalar o Docker e como criar um ambiente Linux no Windows 10 com o Ubuntu 18.04 veja este artigo: [Instalando o Docker no Linux](http://www.macoratti.net/19/01/dock_lnxinst1.htm)

**Nota:** *Você também pode usar o Docker for Windows.(usando contêineres Linux)*

**Criando sua primeira imagem usando o Dockerfile**

No ambiente Linux abra um terminal de comandos e crie uma pasta chamada **demo1** que será nossa pasta de trabalho.

A seguir abra o seu editor de códigos preferido, eu vou usar o Visual Studio Code digitando: **code .**

![img](http://www.macoratti.net/19/02/dock_imgfile11.png)

Com o Visual Studio Code aberto na pasta do projeto crie um arquivo chamado DockerFile *(sem extensão)* com o seguinte conteúdo :

![img](http://www.macoratti.net/19/02/dock_imgfile14.png)

Abaixo temos o código usado no aquivo Dockerfile:

```
# define a imagem base FROM debian:latest``# define o mantenedor da imagem LABEL maintainer="Macoratti"``# Atualiza a imagem com os pacotes RUN apt-get update && apt-get upgrade -y``# Instala o NGINX para testar RUN apt-get install nginx -y``# Expoe a porta 80 EXPOSE 80``# Comando para iniciar o NGINX no Container CMD ["nginx", "-g", "daemon off;"]
```

Observe que definimos comandos no arquivo Dockerfile que serão processados pelo Docker.

Vejamos:

FROM - O primeiro argumento do Dockerfile deve ser sempre o FROM, seguido da imagem e versão que será utilizada. Caso não seja informada a versão, o Docker vai procurar a mais atual do seu repositório oficial.

LABEL: coloca um metadado para o container;

RUN: executa os comandos dentro do container;

EXPOSE: expõe a porta informada do container;

CMD: Informa o comando que será executado após a criação do container, e também pode ser usado para definir os parâmetros que serão usados no comando ENTRYPOINT.

Além dos comandos usados temos também os comandos :

ENTRYPOINT - Define o aplicativo padrão usado toda vez que um contêiner é criado a partir da imagem. Se usado em conjunto com o CMD, você pode remover o aplicativo e apenas definir os argumentos no CMD.

ENV - Define/modifica as variáveis de ambiente dentro dos Containers criados a partir da imagem.

COPY - Copia arquivos do seu ambiente para o contâiner.Ex: COPY origem destino

**Processando o arquivo Dockerfile**

Muito bem já temos o arquivo Dockerfile que descreve as etapas que serão executadas para criar a imagem.

Para processar o arquivo Dockerfile usamos o comando : **docker build -t <imagem> .**

Vamos supor que quero criar a imagem com o nome : macoratti/nginx:1.0

Para fazer isso usamos o comando **build** e informamos o nome da imagem, a tag e um ponto(.). O comando fica assim:

```
docker build -t macoratti/nginx:1.0 .
```

onde:

**docker build**      -> O comando
**-t**               -> Parâmetro usado para informar que a imagem pertence ao meu usuário
**macoratti/nginx:1.0** -> O nome da imagem e a tag atribuída à imagem
**.**                -> significa o diretório atual (*pois dei o build dentro da pasta do Dockerfile*)

**Nota**: *O build não trabalha com o caminho do arquivo, apenas com o seu diretório, então é preciso informar o caminho do diretório ou, no nosso caso, como estamos no diretório onde se localiza o Dockerfile, apenas um ponto(.) para identificar que o Dockerfile está no diretório atual.*

*O comando docker build constrói uma imagem a partir de um Dockerfile e de um contexto. O contexto do build é o conjunto de arquivos na localização especificada PATH ou URL. O PATH é o diretório no seu sistema de arquivos local e a URL é a localização do repositório GIT.*

Executando o comando temos o processamento do Dockerfile:

 ![img](http://www.macoratti.net/19/02/dock_imgfile15.png)

E ao final teremos a imagem macoratti/nginx:1.0 criada:

![img](http://www.macoratti.net/19/02/dock_imgfile16.png)

Pronto !

Acabamos de criar nossa imagem customizada a partir da imagem do debian que contém o nginx instalado.

Para conferir digite o comando : **docker images**

![img](http://www.macoratti.net/19/02/dock_imgfile17.png)

Para testar a imagem vamos criar um contêiner:

**docker container run --name teste -p 80:80 macoratti/nginx:1.0**

![img](http://www.macoratti.net/19/02/dock_imgfile18.png)

Agora basta abrir um navegador e acessar [http://localhost](http://localhost/) para visualizar a página padrão do nginx sendo exibida:

![img](http://www.macoratti.net/19/02/dock_imgfile19.png)

Vemos assim nosso contêiner em execução a partir da nossa imagem criada usando o Dockerfile.

Para distribuir a sua imagem basta fazer o login na sua conta criada no Docker Hub usando o comando :
 **docker login**

**Roteiro para criar a conta no Docker Hub**1- Acesse a página do Docker Hub e clique na opção Create Account 2- Insira o seu usuário, um endereço de email e uma senha (*concorde com os termos de serviço, etc.)* 3- Ao receber o email de verificação clique no link para entrar na página de login e faça o login 4- Na página de Welcome você verá duas opções : Create a Repository e Create an Organization 5- Clique em Create a Repository e preencha o formulário e informe o nome do repositorio e a sua descrição 6- Marque o repositório como público para outros possam visualizar a sua imagem *(Existe opção para repositório privado)* 7- Clique no botão Create 8- Pronto. Você já pode fazer o login no Docker e enviar imagens para o seu repositório.

Lembrando que antes de enviar a imagem você deve '*taguear'* a imagem usando o comando : **docker tag**

**docker tag <imagem_id> <sua_conta>/<nome_image>:tag**

A seguir envie a sua imagem para repositório via comando : 

**docker push <nome_da_sua_conta>/sua_imagem:tag**