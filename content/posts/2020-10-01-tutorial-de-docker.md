---
title: "Tutorial de Docker"
summary: "Você sabe o que é Docker e como usá-lo? Neste tutorial explicamos um pouco sobre o Docker, como instalá-lo no Linux, Windows e Mac, além de exemplificar alguns comandos com exemplos práticos."
tagline: "Conhecendo o Docker com exemplos práticos."
date: 2020-10-01 08:00:00
categories:
  - Desenvolvimento
tags:
  - docker
  - devops
  - containers
  - linux
slug: tutorial-de-docker
authors:
  - jaswdr
images:
- /images/posts/docker-logo.png
---

{{< figure src="/images/posts/docker-logo.png" alt="Docker" >}}

O Docker é um conjunto de ferrametas criada para facilitar o uso de contêineres, algo que não é tão novo no mundo Linux uma vez que projetos como o LXC já existem a muitos anos, porém o seu uso é complexo e a curva de aprendizado é bastante grande. Foi para resolver este e outros problemas que o Docker nasceu. Inicialmente criado por uma empresa independente, teve tanto sucesso que a própria empresa mudou seu nome posteriormente para Docker Inc..

Mas por que você deveria se importar com [contêineres](https://butecotecnologico.com.br/de-que-sao-feitos-containers-linux/)? Existe uma série de motivos que fazem os contêineres serem interessantes e uma melhor alternativa que as máquinas virtuais. O principal deles é que contêineres usam bem menos recursos da maquina hospedeira, na prática cada contêiner é apenas um processo isolado, diferente de toda a sobrecarga que existe quando se usa uma máquina virtual, nesta onde se tem todo um sistema operacional convidado que consome recursos independente da sua aplicação, apenas para manter este sistema operacional funcionando. Isso permite que você execute muito mais aplicações e contêineres no mesmo equipamento físico.

Outra questão importante que nem sempre é considerado quando se usa máquinas virtuais, é que contêineres são efêmeros, ou seja, um contêiner não deve armazenar dados importantes em seu `filesystem`.

Aprenda neste tutorial como usar e aproveitar o máximo do Docker no seu dia a dia como desenvolvedor de software.

## Instalação

A forma de instalação do docker depende do sistema operacional que você está usando. No caso do Linux os contêineres são nativos e devido a isso você pode instalar a última versão com o simples comando abaixo:

```bash
$ curl -fsSL https://get.docker.io | bash
```
O comando acima irá fazer o download de um script que fará a instalação do docker na maioria das distribuições Linux, e executa a instalação usando o `bash`.

Para fazer a instalação em outros sistemas operacionais, clique no link abaixo para fazer a transferência do instalador:

- [Windows](https://download.docker.com/win/stable/Docker%20Desktop%20Installer.exe)
- [Mac](https://download.docker.com/mac/stable/Docker.dmg)

A instalação no Windows e Mac são bem simples, basta seguir o passo a passo ao executador o instalador. Perceba que o Docker Desktop funciona de forma diferente, como o docker é nativo apenas de Linux e se você quiser rodar contêineres Linux em outro sistema operacional, o Docker Desktop irá criar uma máquina virtual para executar os contêineres. Sendo que no Windows é usado por padrão o HyperV e no Mac o VirtualBox.

Algo importante de se notar é que existem contêineres nativos para Windows, porém saiba que imagens de contêineres Linux não são compatíveis com imagens de contêineres Windows, contudo usando contêineres Windows no sistema operacional Windows você terá a mesma experiência e desempenho semelhantes a contêineres Linux sendo executados em alguma distribuição.

> A partir daqui tudo que será mostrado estará sendo executado em ambiente Linux, mais especificamente usando uma máquina virtual com CentOS, mesmo assim os comandos irão funcionar em qualquer outro sistema operacional que use contêineres Linux.

## Começando

Uma vez instalado o docker você pode abrir o terminal e executar o comando `docker` que irá listar todas as opções disponíveis.

```bash
$ docker
Usage:	docker [OPTIONS] COMMAND

A self-sufficient runtime for containers

Options:
      --config string      Location of client config files (default "/Users/schwede/.docker")
  -c, --context string     Name of the context to use to connect to the daemon (overrides
                           DOCKER_HOST env var and default context set with "docker context use")
  -D, --debug              Enable debug mode
  -H, --host list          Daemon socket(s) to connect to
  -l, --log-level string   Set the logging level ("debug"|"info"|"warn"|"error"|"fatal")
                           (default "info")
      --tls                Use TLS; implied by --tlsverify
      --tlscacert string   Trust certs signed only by this CA (default
                           "/Users/schwede/.docker/ca.pem")
      --tlscert string     Path to TLS certificate file (default "/Users/schwede/.docker/cert.pem")
      --tlskey string      Path to TLS key file (default "/Users/schwede/.docker/key.pem")
      --tlsverify          Use TLS and verify the remote
  -v, --version            Print version information and quit

Management Commands:
  builder     Manage builds
  config      Manage Docker configs
  container   Manage containers
  context     Manage contexts
  image       Manage images
  network     Manage networks
  node        Manage Swarm nodes
  plugin      Manage plugins
  secret      Manage Docker secrets
  service     Manage services
  stack       Manage Docker stacks
  swarm       Manage Swarm
  system      Manage Docker
  trust       Manage trust on Docker images
  volume      Manage volumes

Commands:
  attach      Attach local standard input, output, and error streams to a running container
  build       Build an image from a Dockerfile
  commit      Create a new image from a container's changes
  cp          Copy files/folders between a container and the local filesystem
  create      Create a new container
  diff        Inspect changes to files or directories on a container's filesystem
  events      Get real time events from the server
  exec        Run a command in a running container
  export      Export a container's filesystem as a tar archive
  history     Show the history of an image
  images      List images
  import      Import the contents from a tarball to create a filesystem image
  info        Display system-wide information
  inspect     Return low-level information on Docker objects
  kill        Kill one or more running containers
  load        Load an image from a tar archive or STDIN
  login       Log in to a Docker registry
  logout      Log out from a Docker registry
  logs        Fetch the logs of a container
  pause       Pause all processes within one or more containers
  port        List port mappings or a specific mapping for the container
  ps          List containers
  pull        Pull an image or a repository from a registry
  push        Push an image or a repository to a registry
  rename      Rename a container
  restart     Restart one or more containers
  rm          Remove one or more containers
  rmi         Remove one or more images
  run         Run a command in a new container
  save        Save one or more images to a tar archive (streamed to STDOUT by default)
  search      Search the Docker Hub for images
  start       Start one or more stopped containers
  stats       Display a live stream of container(s) resource usage statistics
  stop        Stop one or more running containers
  tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
  top         Display the running processes of a container
  unpause     Unpause all processes within one or more containers
  update      Update configuration of one or more containers
  version     Show the Docker version information
  wait        Block until one or more containers stop, then print their exit codes

Run 'docker COMMAND --help' for more information on a command.
```

Primeiro de tudo, não se assuste com a quantidade de comandos disponíveis, muitos deles são específicos para realizar certas tarefas, porém vamos por partes. Vamos fazer o clássico `hello-world` que é feito em toda linguagem de programação, porém no docker.

```bash
$ docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
0e03bdcc26d7: Pull complete
Digest: sha256:4cf9c47f86df71d48364001ede3a4fcd85ae80ce02ebad74156906caff5378bc
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

O comando acima executa o subcomando `run` que basicamente cria e executa um novo contêiner. O único parâmetro que realmente é obrigatório para executar um contêiner é o nome da imagem do contêiner. Uma imagem de contêiner é um pacote executável leve, standalone, que incluí tudo necessário para executar a aplicação, como código, runtime, ferramentas de runtime, bibliotecas e configurações.

Na saída do programa podemos ver que o docker tentou buscar localmente a imagem `hello-world`, porém como nesse caso não existia ele buscou do repositório padrão, que é o [Docker Hub](https://hub.docker.com/). Logo após iniciou o download de cada camada da imagem, cada camada é uma mudança na imagem, como o `hello-world` é simples, possuí apenas uma única camada.

Por fim, após o download do imagem é criado uma instância da imagem que é o contêinere em si, podendo ver então o texto do `Hello from Docker!`

## Conceitos importantes

Além do que vimos acima, existem diversos outros conceitos importantes que você deve saber antes de começar a usar o docker, ou até mesmo para usar todo o potêncial do mesmo.

### Volumes

O primeiro conceito importante é o de *volumes*. Volumes são usados para armazenar dados que não são efêmeros, isto é, dados que não são apagados quando o contêinere é excluído. Usando o cliente docker você faz o *bind* de volumes, diretórios ou arquivos locais da sua máquina para o contêiner, dessa forma qualquer alteração feita localmente é refletida dentro do contêiner e vice versa.

Para entender melhor vamos a um exemplo prático, execute em qualquer diretório o comando abaixo para criar um arquivo.

```bash
$ touch meuarquivo.txt
```

Agora execute o comando usando o docker abaixo para criar um contêiner do Ubuntu e fazer o bind do diretório atual para o diretório `/exemplo` dentro do contêiner.

```bash
$ docker run -it -v $PWD:/exemplo ubuntu
```
A princípio nada de mais parece ter mudado, mas vá para o diretório `/exemplo` usando o `cd`.

```bash
$ cd exemplo/
$ ls
meuarquivo.txt
```
Perceba que temos o nosso arquivo `meuarquivo.txt` listado, vamos escrever algo nele e sair do contêiner.

```bash
$ echo 'hello volume!' > meuarquivo.txt
$ exit
```
Saindo do contêiner podemos ver que nosso arquivo continua existindo localmente e agora com o conteúdo que escrevemos nele.

```bash
$ ls
meuarquivo.txt
$ cat meuarquivo.txt
hello volume!
```

Pronto! agora se quisermos executar outro contêiner o arquivo continuará tendo o conteúdo que escrevemos.

```bash
$ docker run -it -v $PWD:/exemplo ubuntu
root@484eea8e9505:/# cat /exemplo/meuarquivo.txt
hello volume!
```
Usando volumes você pode por exemplo executar o seu código local em um contêiner da linguagem que você usa, enquanto edita no seu editor de texto ou IDE localmente, tudo de maneira fácil e prática.

### Vincular Portas

Outro conceito importante de entender é como o docker vincula portas locais com as portas do contêiner. Muitas vezes você executa algum serviço que executa numa porta, como servidores WEB ou até mesmo bancos de dados. Para ter acesso a esses serviços localmente você vincular as portas da sua máquina a portas do contêiner. Para entender melhor vamos novamente a um exemplo prático.

Digamos que você queira rodar o servidor Nginx, para isso basta executar o comando abaixo:

```bash
$ docker run -d -p 8080:80 nginx
```

Perceba o uso da flag `-p` passando o valor `8080:80`, isso significa que está sendo feito um vinculo, ou *bind*, da porta 8080 local com a porta 80 do contêiner, fazendo com que requisições que sejam feitos a porta 8080 local sejam redirecionadas a porta 80 do contêiner, é importante ressaltar pode-se ter apenas um vínculo por porta, não sendo possível ter 2 ou mais contêineres vinculados a mesma porta. Experimente acessar o endereço http://localhost:8080/ no seu navegador para ver a página padrão do Nginx.

## Exemplos

Se você ainda não se convenceu do potêncial do docker, veja abaixo alguns exemplos mais comuns e veja por você mesmo a praticidade dessa ferramenta que revolucionou o mundo do DevOps.

## Banco de dados (PostgreSQL)

Execute uma nova instância do PostgreSQL localmente na porta 5432.

```bash
$ docker run -d -e POSTGRES_PASSWORD=secret -p 5432:5432 postgres
```

Execute o comando acima e tente conectar com algum cliente do PostgreSQL na porta 5432 da sua máquina com o usuário `postgres` e senha `secret`.

## Nginx

Execute o servidor Nginx passando o diretório atual como *root*.

```bash
$ docker run -d -v $PWD:/usr/share/nginx/html -p 8080:80 nginx
```

Execute o comando acime e tente acessar o endereço `http://localhost:8080` para ver suas páginas estáticas que você tem localmente na sua máquina.

### Redis

Execute um servidor do Redis localmente na sua máquina na porta 6379.

```bash
$ docker run -d -p 6379:6379 redis
```

### Python

Teste seu código Python em diferentes versões de forma rápida e fácil.

Arquivo `main.py` de teste.

```bash
import sys
print("Python version")
print (sys.version)
print("Version info.")
print (sys.version_info)
```

Python 3.6

```bash
$ docker run --rm -v $PWD/main.py:/main.py python:3.6 python /main.py
Python version
3.6.11 (default, Jul 22 2020, 13:26:52)
[GCC 8.3.0]
Version info.
sys.version_info(major=3, minor=6, micro=11, releaselevel='final', serial=0)
```

Python 3.8

```bash
$ docker run --rm -v $PWD/main.py:/main.py python:3.8 python /main.py
Python version
3.8.5 (default, Sep 10 2020, 16:47:10)
[GCC 8.3.0]
Version info.
sys.version_info(major=3, minor=8, micro=5, releaselevel='final', serial=0)
```

## Ubuntu

Execute uma sessão interativa do Ubuntu localmente.

```bash
$ docker run -it ubuntu bash
root@f948917eca40:/# apt-get update > /dev/null
root@f948917eca40:/# apt-get install -y curl > /dev/null
debconf: delaying package configuration, since apt-utils is not installed
root@f948917eca40:/# curl https://example.org
<!doctype html>
<html>
<head>
    <title>Example Domain</title>
...
```

# Conclusão

O docker é uma alternativa perfeita para ajudar no desenvolvimento de software, com ele você consegue rapidamente criar seu ambiente de desenvolvimento, testar seu código em diferentes versões, criar servidores de banco de dados, servidores WEB e muito mais, tudo de forma rápida, isolada e confiável.

E ai gostou do docker? Quer ver mais conteúdo sobre, tem alguma sugestão? Comente abaixo e nos vemos na próxima.
