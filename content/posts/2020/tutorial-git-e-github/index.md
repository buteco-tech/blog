---
title: "Tutorial de Git e GitHub"
summary: "Se você é ou pretende se tornar um desenvolvedor, já deve ter ouvido falar do Git e GitHub, um sistema do controle de versão e um repositório online para os nossos projetos. Agora é a hora de conhecer mais sobre essas ferramentas e aprender a usá-las."
tagline: "Entenda mais sobre essas ferramentas"
date: 2020-11-18 08:00:00
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

Você já deve ter ouvido falar do [Git](https://git-scm.com/) e [GitHub](https://github.com/), um sistema do controle de versão e um repositório online para os nossos projetos. Agora é a hora de conhecer mais sobre essas ferramentas e aprender a usá-las.

## O que é Git?

Git é um sistema de código aberto e gratuito para controle de versão, criado em 2005 por [Linus Torvalds](https://pt.wikipedia.org/wiki/Linus_Torvalds) enquanto desenvolvia o [Kernel do Linux](https://github.com/torvalds/linux). **Controle de versão** é um sistema que permite gravar alterações em arquivos ao longo do tempo, portanto, podemos visualizar versões específicas desses arquivos posteriormente.

Geralmente, acontece de termos muitos desenvolvedores trabalhando numa mesma base de códigos, então um sistema de controle de versão como o Git é necessário para diminuir conflitos entre o código de cada desenvolvedor.

## O que é GitHub?

Se você vai utilizar Git, é necessário armazenar seu repositório em algum lugar. Existem duas formas de se fazer isso, offline, no seu próprio computador, ou online, em alguma plataforma como [GitHub](https://github.com/), [BitBucket](https://bitbucket.org/) ou [GitLab](https://gitlab.com/explore).

É exatamente para isso que o GitHub serve, para armazenar seus repositórios, ele é o "Google Drive dos códigos".

## Configuração do ambiente

Primeiramente, é necessário possuir o Git instalado no seu computador e uma conta no GitHub. Você pode baixar o Git no [site oficial](https://git-scm.com/downloads), basta selecionar o seu sistema operacional e seguir o guia de instalação.

No Linux, recomenda-se instalar o Git via gerenciador de pacotes, nesse caso, você pode ir até [essa página](https://git-scm.com/download/linux) e executar no seu terminal o comando específico da sua distribuição.

Depois de instalar o Git, é necessário configurar o seu nome e e-mail que serão exibidos em seus _commits_, para isso é só digitar no terminal os comandos abaixo:

```bash
git config --global user.name "Seu Nome"
git config --global user.email "seu-email@buteco.tech"
```

> Observação: Caso você esteja no Windows, não se esqueça de utilizar o **git bash**, pois ele simula um ambiente Unix facilitando a forma de se trabalha com Git.

Agora iremos criar uma chave de SSH, com ela provaremos ao GitHub que nós somos os donos da nossa conta. Caso já tenha uma chave SSH, podemos verificar seu diretório **.ssh** listando o conteúdo:

```bash
cd ~/.ssh
ls
```

Caso o comando `ls` retorne algo com a extensão **.pub**, você não precisa executar o comando `ssh-keygen`. Caso contrário, digite esse comando no seu terminal:

```bash
ssh-keygen -t rsa -b 4096 -C "seu-email@buteco.tech"
```

O comando acima irá pedir um nome do arquivo para salvar a chave, não escreva nada e aperte enter (será gerada uma com o nome **id_rsa**). Depois, informe uma senha (_passphrase_) e confirme ela. Pronto, você gerou uma chave SSH.

Vamos agora adicionar a chave ao [ssh-agent](https://pt.wikipedia.org/wiki/Ssh-agent), um programa que fará a autenticação da sua máquina local, com o servidor remoto, que nesse caso seria o GitHub, execute então o comando `ssh-agent`:

```bash
eval $(ssh-agent -s)
```

A mensagem `Agent pid 1234` será exibida.

Executar o comando `ssh-add`, onde o **id_rsa** é o nome da sua chave SSH, informe a _passphrase_ utilizada no comando `ssh-keygen`:

```bash
ssh-add ~/.ssh/id_rsa
```

A mensagem `Identity added : id_rsa (seu-email@buteco.tech)` será exibida.

## Adicionando a chave SSH à sua conta no GitHub

Primeiramente, você deve [criar uma conta](https://github.com/join) no GitHub.

No diretório `~/.ssh` existem dois arquivos, **id_rsa** e **id_rsa.pub**. O com a extensão **.pub** é a chave pública, ou seja, a chave que você irá informar ao GitHub. O arquivo sem o **.pub** é a sua chave privada.

Digite o seguinte comando no terminal para exibir o conteúdo da chave gerada:

```bash
cat ~/.ssh/id_rsa.pub
```

Copie todo o conteúdo para a sua área de transferência (lembre-se de copiar desde "ssh-rsa" até o seu e-mail).

Acesse a página de [SSH and GPG keys no GitHub](https://github.com/settings/keys) para associar a nova chaves SSH a sua conta. Clique então no botão **_New SSH Key_**.

O título você pode escolher um (ex: chave seu-email@buteco.tech), e em _Key_, você informará o conteúdo que copiou anteriormente.

Clique em _Add SSH Key_ para salvar. Pronto, sua configuração está feita.

{{<figure src="ssh-add" alt="ssh add">}}

## Fluxo de trabalho 1

Geralmente, o fluxo de trabalho com Git e GitHub é realizando um _fork_ de um determinado repositório de outra pessoa. Dessa forma, vamos criar uma cópia deste repositório no nosso perfil. Após fazer o _clone_ do _fork_ para a nossa máquina local, podemos realizar alterações, e publicar as alterações feitas em seu repositório _fork_ no GitHub. Por fim, poremos criar um _pull request_ do seu _fork_ para o repositório principal (_upstream_).

### Fork

Vamos começar realizando o _fork_ de um repositório base no GitHub. Veja nosso repositório [artigo-tutorial-git](https://github.com/buteco-tech/artigo-tutorial-git) e clique no botão **_Fork_** no canto superior direito. Ao clicar, será feita uma cópia do repositório do Buteco em seu perfil no GitHub.

Agora em seus repositórios você tera uma cópia de **artigo-tutorial-git**. Lembre-se, você não está no repositório principal, você está no seu, as alterações feitas ali, não terão impacto no principal.

Você deve criar um _Pull Request_ para enviar suas alterações, que é o que iremos ver daqui a pouco.

### Git Clone

Vamos clonar o repositório para a sua máquina local para que você possa realizar alterações. Na página do seu repositório no GitHub, existe um botão chamado **_Code_**, clique nele, selecione a opção **_SSH_**, e depois clique no botão de copiar.

{{<figure src="git-clone-btn" alt="ssh add">}}

Agora abra o seu terminal no local que deseja baixar o repositório, e execute (use o endereço que você copiou):

```bash
git clone git@github.com:seu-usuario/artigo-tutorial-git.git
```

Será pedido para que você informe a sua _passphrase_ de quando criou a sua chave SSH. Após, o clone do repositório você terá uma nova pasta chamada `artigo-tutorial-git`.

Entre na nova pasta com o comando:

```bash
cd artigo-tutorial-git
```

Você deverá ter algo similar a imagem abaixo:

{{<figure src="git-clone" alt="git clone">}}

Perceba que existe ali, uma pasta chamada **.git**, é nela que o Git guarda todas as informações do seu repositório, todas as alterações, o endereço do repositório remoto e tudo o que é relacionado ao controle de versão.

> No Windows talvez você não consiga visualizar esta pasta pois ela é um arquivo oculto, então é só seguir [esse tutorial](https://support.microsoft.com/pt-br/help/14201/windows-show-hidden-files) para alterar as configurações.

### Git Status e Git Add

Vamos criar um novo arquivo para que possamos adicionar ele depois, então, crie um arquivo de texto qualquer dentro do diretório do repositório.

```bash
echo "Tutorial de Git e GitHub" > arquivo.txt
```

Voltando para o terminal, digite `git status`. Você verá essa mensagem:

{{<figure src="git-status" alt="git status">}}

_Untracked files_ são as alterações pendentes, nós precisamos adicionar elas à área de _staging_. As alterações que estão no _staging_ são as que serão _commitadas_ futuramente.

Para adicionar apenas um arquivo execute o comando:

```bash
git add nome-do-arquivo
```

Ou para adicionar todos os arquivos execute o comando:

```bash
git add .
```

O `git add` não afeta diretamente o repositório, pois ele ainda não "salvou" as alterações, apenas às moveu para a área de _staging_, quando executamos `git commit` essas as alterações serão salvas.

Agora sim, podemos confirmar as alterações, para isso execute o comando:

```bash
git commit -m "Meu primeiro commit"
```

O `-m` é para informar uma mensagem que será gravada junto ao _commit_.

Se você executar o comando `git status` novamente, o seu terminal deverá estar parecido com o abaixo.

{{<figure src="git-commit" alt="git commit">}}

Agora é hora de subir as alterações feitas no seu repositório local para o seu repositório remoto, nesse caso, o do GitHub.

### Git Push

Quando estamos com alguma alteração _commitada_ localmente e queremos subir elas para o repositório remoto, precisamos publicá-las, para isso execute o comando:

```bash
git push
```

Após, visite a página do seu repositório no GitHub. Você percebera que as alterações feitas localmente, já estão lá, mas atenção, quando você fez o _push_, as alterações foram para o seu _fork_, não para o repositório principal.

### Criando um Pull request

No seu repositório, após feito um _push_, você verá um botão escrito **_Pull request_**. Clique nele.

{{<figure src="pull-request-btn" alt="pull request">}}

Isso irá abrir uma página mostrando os arquivos alterados e um botão **_Create pull request_**. Clique nele, informe o título e um comentário para esse _pull request_ e clique no botão **_Create pull request_**.

Pronto, seu PR foi criado com sucesso.

Se você for até a página de [pull requests do repositório principal](https://github.com/buteco-tech/artigo-tutorial-git/pulls), o seu PR estara lá. Agora, para a sua alteração entrar de fato no repositório principal, o se _pull request_ deve ser aceito por algum mantedor do repositório. Esse processo de aceitar um _pull request_ é chamado de _merge_.

### O que é Pull Request?

Desde o começo desse artigo, tudo girou em torno deste momento, em que você cria o seu _pull request_, mas afinal, o que é um _pull request_?

Para entendermos o que é um _pull request_ precisamos entender o que é um _pull_. Quando existe alguma alteração no repositório remoto, e você quer baixar essa alteração para o seu repositório local, ou seja, o seu repositório local está atrasado em relação ao remoto, você digita `git pull` no terminal (olha só, mais um comando novo). Um _pull request_ é uma sugestão de uma alteração, ou seja, você está pedindo para que aquele repositório que você enviou o _pull request_ faça um _pull_ com as suas alterações.

## Fluxo de trabalho 2

Como dito anteriormente, vamos ver outra maneira de se utilizar o Git e GitHub.

Desta vez, vamos começar com o comando `git init` para criar o nosso repositório localmente, depois iremos conectar ele ao GitHub e enviar os nossos _commits_.

### Git Init

Crie uma pasta em qualquer lugar do seu computador e navegue até ela com o seu terminal. Digite então dentro dela, o comando:

```bash
git init
```

Você perceberá que foi criado uma pasta **.git**, que mapeia as alterações e _commits_ do repositório como mencionado anteriormente.

Realize agora alguma alteração. Crie um arquivo de texto, por exemplo. Depois, execute o comando `git add` que foi falado anteriormente e `git commit -m`, agora você já está craque nisso.

{{<figure src="git-init" alt="git init">}}

Feito isso, é hora de subirmos essas alterações para o repositório remoto (que ainda não foi criado).

Precisamos [criar um repositório no GitHub](https://github.com/new). Nesta página informe algumas informações sobre o repositório, como o nome, descrição, e clique em **_Create repository_**.

Pronto, um novo repositório foi criado.

### Vinculando o repositório local, ao remoto

Veja que existem ali três opções:

- **_…or create a new repository on the command line_**

- **_…or push an existing repository from the command line_**

- **_…or import code from another repository_**

{{<figure src="repositorio-github-criado" alt="Repositório no GitHub">}}

A primeira opção, é para criar um repositório local, vincular ele ao GitHub, e fazer um _push_ das alterações. A segunda é para apenas conectar um repositório local já existente, e fazer um _push_ dele. E a terceira, é para importar o código de algum repositório que esteja utilizando um sistema de versionamento de código diferente do Git.

Nós já temos o nosso repositório criado, queremos apenas subir ele ao GitHub, portanto, vamos realizar os passos da segunda opção.

Com o seu terminal aberto na pasta do seu repositório local, execute então os seguintes comandos:

```bash
git remote add origin git@github.com:seu-nome/git-init-exemplo.git
```

Esse comando define para onde enviaremos nossos _commits_, nesse caso o GitHub.

```bash
git branch -M main
```

Esse comando cria uma _branch_ com o nome `main`, padrão do GitHub.

```bash
git push -u origin main
```

Esse comando envia as alterações _commitadas_ ao seu repositório remoto no GitHub.

Pronto, se você for até a página do seu repositório no GitHub as alterações já estarão lá.

### O fim, se existe, está muito distante ainda

Se você chegou até aqui, então aprendeu bastante coisa.

Você aprendeu como fazer um _fork_ de um repositório no GitHub, como _clonar_ ele para a sua máquina local com `git clone`, adicionar as suas alterações a ele com `git add`, _commitar_ elas com `git commit`, subir as suas alterações ao repositório remoto com `git push` e como abrir um `pull request` com as alterações.

Mas não acabou por aí, o próximo passo é estudar sobre _branches_, entendendo como elas funcionam e são usadas. E claro, contribuir com software livre.

E ai, você já usou Git ou Github antes? O que achou? Deixe um comentário abaixo.
