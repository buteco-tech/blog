---
title: "Tutorial de Git e GitHub"
summary: "Se você é ou pretende se tornar um desenvolvedor, já deve ter ouvido falar do Git e GitHub, um sistema do controle de versão e um repositório online para os nossos projetos. Agora é a hora de conhecer mais sobre essas ferramentas e aprender a usá-las."
tagline: "Iniciando com Git e GitHub"
date: 2020-10-10 08:00:00
slug: tutorial-git-e-github
authors:
  - leonardodalcegio
categories:
  - desenvolvimento
  - tutoriais
tags:
  - git
  - github
resources:
  - name: git-clone-btn
    src: git-clone-btn.png
  - name: git-clone
    src: git-clone.png
  - name: git-commit
    src: git-commit.png
  - name: thumbnail
    src: git-logo.png
  - name: git-status
    src: git-status.png
  - name: pull-request-btn
    src: pull-request-btn.png
  - name: ssh-add
    src: ssh-add.png
  - name: repositorio-github-criado
    src: repositorio-github-criado.png
  - name: git-init
    src: git-init.png
---

{{<figure src="thumbnail" alt="git">}}

Se você é ou pretende se tornar um desenvolvedor, já deve ter ouvido falar do [Git](https://git-scm.com/) e [GitHub](https://github.com/), um sistema do controle de versão e um repositório online para os nossos projetos. Agora é a hora de conhecer mais sobre essas ferramentas e aprender a usá-las.

## O que é Git?

Git é um sistema de código aberto e gratuito para controle de versão, criado em 2005 por [Linus Torvalds](https://pt.wikipedia.org/wiki/Linus_Torvalds) quando estava desenvolvendo o [Kernel do Linux](https://github.com/torvalds/linux). **Controle de versão**, é um sistema que permite gravar alterações em arquivos ao longo do tempo, portanto, podemos visualizar versões específicas desses arquivos posteriormente.

Na maioria dos projetos, geralmente, acontece de termos muitos desenvolvedores trabalhando na mesma base de códigos, então um sistema de controle de versão como o Git é necessário para que não haja conflito entre o código de cada desenvolvedor.

## O que é GitHub?

Se você vai utilizar Git, provavelmente vai querer armazenar o seu repositório em algum lugar, existem duas formas de se fazer isso, offline, no seu próprio computador, ou online, em alguma plataforma como [GitHub](https://github.com/), [BitBucket](https://bitbucket.org/) ou [GitLab](https://gitlab.com/explore). É exatamente para isso que o GitHub serve, para armazenar seu repositório nele, é o "Google Drive dos códigos".

## Configuração do ambiente

É necessário possuir o Git instalado no seu computador e uma conta no GitHub.

Ambos os passos são bem rápidos, você pode baixar o Git [aqui](https://git-scm.com/downloads), é só selecionar o seu sistema operacional e seguir o guia de instalação, no caso do Linux, costuma-se instalar o Git via um gerenciador de pacotes, nesse caso, você pode ir até [essa página](https://git-scm.com/download/linux), e executar no seu terminal o comando específico da sua distribuição. Já para o GitHub, você deve cria uma conta [aqui](https://github.com/).

Depois de ter baixado o Git, é necessário configurar o seu nome e e-mail que serão exibidos em seus _commits_, para isso é só digitar no terminal os comandos abaixo (caso você esteja no Windows, não se esqueça de utilizar o **git bash**, ao invés do **cmd**, pois o **git bash** simula um ambiente Unix e fica melhor de se trabalhar dessa forma):

- `git config --global user.name "Buteco Tecnológico"`

- `git config --global user.email "git@buteco.tech"`

Agora iremos criar uma chave de SSH, com ela conseguimos provar ao GitHub que nós somos os donos da nossa conta. Você talvez até já tenha uma chave SSH no seu computador, podemos checar se já existe uma entrando no seu diretório **.ssh** e listando o conteúdo:

- `cd ~/.ssh`

- `ls`

  Caso o comando `ls` retorne algo com a extensão **.pub**, você não precisa executar o comando `ssh-keygen`. Caso contrário, digite esse comando no seu terminal:

- `ssh-keygen -t rsa -b 4096 -C "git@buteco.tech"`

O comando acima irá pedir um nome do arquivo para salvar a chave, não escreva nada e aperte enter (será gerada uma com o nome **id_rsa**). Depois, informe uma senha (_passphrase_) e confirme ela. Pronto, você gerou uma chave SSH.

Vamos agora adicionar a sua chave ao **ssh-agent**, um programa que fará a autenticação da sua máquina local, com o servidor remoto, que nesse caso seria o GitHub, execute então o comando [ssh-agent](https://pt.wikipedia.org/wiki/Ssh-agent), você deverá receber um _Agent pid_:

- `eval $(ssh-agent -s)`

Agora iremos executar o comando `ssh-add`, onde o **id_rsa** é o nome da sua chave SSH, depois de digitar o comando, será requisitado a _passphrase_ que você informou quando criou a chave.

- `ssh-add id_rsa`

Você deverá receber a mensagem _Identity added_ : id_rsa (git@buteco.tech).

## Adicionando a chave SSH à sua conta no GitHub

Se você navegar até o local onde a chave foi criada (diretório do seu usuário, pasta **.ssh**), você verá dois arquivos, **id_rsa** e **id_rsa.pub**. O com a extensão **.pub** é a chave **pública**, ou seja, a chave que você irá informar ao GitHub. O arquivo sem o **.pub** é a sua chave **privada**.

Digite o seguinte comando no terminal para exibir o conteúdo da chave gerada, depois copie todo o conteúdo para a sua área de transferência (lembre-se de copiar desde "ssh-rsa" até o seu e-mail):

- `cat ~/.ssh/id_rsa.pub`

Acesse a página de [SSH and GPG keys no GitHub](https://github.com/settings/keys) para poder associar novas chaves SSH a sua conta. Clique então no botão **_New SSH Key_**. O título você pode escolher um, e em _key_, você informará o conteúdo que copiou anteriormente do seu arquivo da chave pública.

Depois de informado a sua chave **pública** ao GitHub, a sua configuração está feita.
{{<figure src="ssh-add" alt="ssh add" >}}

---

## Fluxo de trabalho 1

Geralmente, o fluxo de trabalho de quando trabalhamos com Git e GitHub, é realizar um _fork_ de um determinado repositório no GitHub de outra pessoa, dessa forma, vamos estar com uma cópia deste no nosso perfil, depois, fazer o _clone_ do _fork_ para a nossa máquina local, realizar as alterações, subir as alterações feitas localmente, ao seu repositório _fork_ no GitHub, e realizar um _pull request_ do seu _fork_ para o repositório principal.

Caso você nunca tenha viso algum desses nomes antes, não se assuste, iremos ver tudo isso neste artigo. Existe outra maneira de se trabalhar com o Git, que veremos mais adiante, que é criando primeiramente o repositório local no seu computador, para depois subir ele ao GitHub.

---

### _Fork_

Vamos começar realizando o _fork_ de um repositório base no GitHub. Vá até [essa página](https://github.com/buteco-tech/artigo-tutorial-git) e clique no botão **_Fork_** no canto superior direito, isso fará uma cópia do repositório para o seu perfil no GitHub.

Agora navegue até os repositórios do seu perfil, você verá que existe um ali chamado **"artigo-tutorial-git"**, clique nele. Agora você não está no repositório principal, você está no seu, as alterações feitas ali, não terão impacto no principal, a menos que você faça um _Pull Request_, que é o que iremos fazer daqui a pouco.

### "git clone"

Vamos agora clonar esse seu repositório para a sua máquina local, para que você possa realizar alterações nele. Na página do seu repositório no GitHub, existe um botão chamado **_Code_**, clique nele, selecione a opção **_SSH_**, e depois clique no botão de copiar.

{{<figure src="git-clone-btn" alt="ssh add" >}}

Agora abra o seu terminal no local que deseja copiar o repositório, e execute `git clone`, seguido do endereço que você copiou. Será pedido para que você informe a sua _passphrase_ de quando criou a sua chave SSH. Depois digite `cd artigo-tutorial-git`, para navegar com o seu terminal até a pasta do repositório clonado. Você ficará com o terminal assim:

{{<figure src="git-clone" alt="git clone" >}}

Perceba que existe ali, uma pasta chamada **".git"**, é nela que o git guarda todas as informações do seu repositório, todas as alterações, o endereço do repositório remoto, tudo o que é relacionado ao controle de versão do git é guardado dentro dela. No Windows talvez você não consiga visualizar esta pasta, pois ela é um arquivo oculto, então é só seguir [esse tutorial](https://support.microsoft.com/pt-br/help/14201/windows-show-hidden-files), que já vai ser possível visualizar ela.

### "git status" e "git add"

Vamos criar um novo arquivo para que possamos adicionar ele depois, então, crie um arquivo de texto com o seu nome por exemplo.

Voltando para o terminal, digite `git status`. Você verá essa mensagem:

{{<figure src="git-status" alt="git status" >}}

Essas _Untracked files_ são as alterações pendentes, nós precisamos adicionar elas à área de _staging_, as alterações que estão no _staging_ são as que serão _commitadas_ futuramente.

Então, digite `git add .`, para adicionar tudo. Se você quisesse adicionar apenas um arquivo, o comando seria `git add nome-do-arquivo`.

O `git add` não afeta diretamente o repositório, pois ele ainda não "salvou" as alterações, apenas às moveu para a área de _staging_, quando fazermos um `git commit` é que as alterações serão salvas.

Agora sim, podemos _commitar_ as alterações, então vamos lá, `git commit -m 'Meu primeiro commit'`, o -m é para informar uma mensagem que será gravada junto ao _commit_.

Se você executar o comando `git status`, o seu terminal deverá estar parecido com o abaixo.

{{<figure src="git-commit" alt="git commit" >}}

Agora é hora de subir as alterações feitas no seu repositório local, ao repositório remoto, nesse caso, o do GitHub.

---

### "git push"

Quando estamos com alguma alteração _commitada_ localmente e queremos subir elas para o repositório remoto, precisamos fazer um `git push`, então digite `git push` no seu terminal e recarregue a página do seu repositório no GitHub, você verá que as alterações feitas localmente, já estão lá, mas atenção, quando você fez o _push_, as alterações foram para o seu _fork_, não para o repositório principal.

### Abrindo uma _Pull request_

Se você for até o seu repositório agora, depois de ter feito um _push_ nele, você verá um botão escrito **_Pull request_**. Daí é só clicar nele.

{{<figure src="pull-request-btn" alt="pull request" >}}

Irá abrir uma página mostrando os arquivos alterado em um botão **_Create pull request_**, clique nele, agora informe o título e um comentário para essa _pull request_ e clique naquele mesmo botão **_Create pull request_**, pronto, está criada. Se você for até a [página](https://github.com/buteco-tech/artigo-tutorial-git/pulls) das _pull requests_ do repositório principal, a sua vai estar lá. Agora, para a sua alteração entrar de fato no repositório principal, a sua _pull request_ tem que ser aceita por algum mantedor do repositório e deve ser feito o _merge_ com as alterações.

### O que é _pull request_?

Desde o começo desse artigo, tudo girou em torno deste momento, em que você cria a sua _pull request_, mas afinal, o que é uma _pull request_?

Para entendermos o que é uma _pull request_ precisamos entender o que é um _pull_. Quando existe alguma alteração no repositório remoto, e você quer baixar essa alteração para o seu repositório local, ou seja, o seu repositório local está atrasado em relação ao remoto, você digita `git pull` no terminal (olha só, mais um comando novo). Uma _pull request_ é uma sugestão de uma alteração, ou seja, você está pedindo para que aquele repositório que você enviou a _pull request_, faça um _pull_ com as suas alterações.

---

## Fluxo de trabalho 2

Como já dito anteriormente, agora vamos ver uma outra maneira de se utilizar o Git e GitHub. Vamos começar com o comando `git init` para criar o nosso repositório localmente, depois iremos conectar ele ao GitHub e enviar os nossos _commits_.

### "git init"

Crie uma pasta em qualquer lugar do seu computador e navegue até ela com o seu terminal, digite então dentro dela, o comando `git init`. Você verá que foi criado uma pasta **".git"**, que "trackeia" as alterações e commits do repositório.

Realize agora alguma alteração. Crie um arquivo de texto, por exemplo. Depois, execute o comando `git add` que foi falado anteriormente e `git commit -m`, agora você já está craque nisso. Lembre-se, é `git add nome_do_arquivo` (ou `git add .`), e `git commit -m 'mensagem do seu commit'`.

{{<figure src="git-init" alt="git init" >}}

Feito isso, é hora de subirmos essas alterações para o repositório remoto (que ainda não foi criado). Para isso, visite a página do [GitHub](https://github.com/), e com a sua conta logada, clique no canto superior esquerdo, no botão **_New_**. Irá abrir uma página para você informar algumas informações sobre o repositório. Digite um nome, uma descrição, e clique em **_Create repository_**.

### Vinculando o repositório local, ao remoto

Veja que existem ali três opções:

- **_…or create a new repository on the command line_**

- **_…or push an existing repository from the command line_**

- **_…or import code from another repository_**

{{<figure src="repositorio-github-criado" alt="Repositório no GitHub" >}}

A primeira opção, é para criar um repositório local, vincular ele ao GitHub, e fazer um _push_ das alterações. A segunda é para apenas conectar um repositório local já existente, e fazer um _push_ dele. E a terceira, é para caso possuamos algum repositório que esteja utilizando um sistema de versionamento de código diferente do git, possamos importar o código dele ao GitHub e começar a utilizar o git com ele.

Nós já temos o nosso repositório git criado, queremos apenas subir ele ao GitHub, portanto, vamos realizar os passos da segunda opção.

Com o seu terminal aberto na pasta do seu repositório local, execute então os seguintes comandos:

- `git remote add origin git@github.com:LeoDalcegio/git-init-exemplo.git`(altere o endereço para aquele que está aparecendo ali para você)

- `git branch -M main` (para criar a _branch_ de nome "main")

- `git push -u origin main` (para enviar as alterações _commitadas_, ao repositório remoto)

Pronto, se você for até a página do GitHub do seu repositório, as alterações _commitadas_ estarão lá.

### O fim, se existe, está muito distante ainda

Se você chegou até aqui, então aprendeu bastante coisa, viu sobre como fazer um _fork_ de um repositório no GitHub, clonar ele para a sua máquina local com `git clone`, adicionar as suas alterações a ele com `git add`, _commitar_ elas com `git commit`, subir as suas alterações ao repositório remoto com `git push`, abrir uma `pull request` com as alterações.

Mas não acabou por aí, o próximo passo é estudar sobre _branches_, entendendo como elas funcionam e são usadas. E claro, contribuir com software livre.

E ai, você já usou Git ou Github antes? O que achou? Deixe um comentário abaixo.
