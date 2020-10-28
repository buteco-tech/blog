---
title: "Tutorial de Git e GitHub"
summary: "Se você é ou pretende se tornar um desenvolvedor, já deve ter ouvido falar do Git e GitHub, um sistema do controle de versão e um repositório online para os nossos projetos. Agora é a hora de conhecer mais sobre essas ferramentas e aprender a usá-las."
tagline: "Iniciando com Git e GitHub"
date: 2020-10-28 08:00:00
slug: tutorial-git-e-github
authors:
  - leonardodalcegio
categories:
  - desenvolvimento
  - tutoriais
tags:
  - git
  - github
images:
  - /images/posts/git-logo.png
---

{{< figure src="/images/posts/git-logo.png" alt="git" >}}

Se você é ou pretende se tornar um desenvolvedor, já deve ter ouvido falar do [Git](https://git-scm.com/) e [GitHub](https://github.com/), um sistema do controle de versão e um repositório online para os nossos projetos. Agora é a hora de conhecer mais sobre essas ferramentas e aprender a usá-las.

## O que é Git?

Git é um sistema de código aberto e gratuito para controle de versão, criado em 2005 por [Linus Torvalds](https://pt.wikipedia.org/wiki/Linus_Torvalds) quando estava desenvolvendo o [Kernel do Linux](https://github.com/torvalds/linux). Controle de versão, é um sistema que permite gravar alterações em arquivos ao longo do tempo, portanto, podemos visualizar versões específicas desses arquivos posteriormente.

Na maioria dos projetos, geralmente, acontece de termos muitos desenvolvedores trabalhando na mesma base de códigos, então um sistema de controle de versão como o Git é necessário para que não haja conflito entre o código de cada desenvolvedor.

## O que é GitHub?

Se você vai utilizar Git, provavelmente vai querer armazenar o seu repositório em algum lugar, existem duas formas de se fazer isso, offline, no seu próprio computador, e online, em alguma plataforma como [GitHub](https://github.com/), [BitBucket](https://bitbucket.org/) ou [GitLab](https://gitlab.com/explore). É exatamente para isso que o GitHub serve, para armazenar seu repositório nele, é o Google Drive dos códigos.

## Configuração do ambiente

É necessário ter o Git instalado no seu computador e uma conta no GitHub criada.

Ambos os passos são bem rápidos, você pode baixar o Git [aqui](https://git-scm.com/downloads), é só selecionar o seu sistema operacional e seguir o setup, no caso do Linux, costuma-se instalar o Git via um gerenciador de pacotes, nesse caso, você pode ir até [essa página](https://git-scm.com/download/linux), e executar no seu terminal o comando específico da sua distribuição. E o GitHub você cria uma conta [aqui](https://github.com/).

Depois de ter baixado o Git, é necessário configurar o seu nome e email que serão exibidos em seus commits, para isso é só digitar no terminal o seguinte (caso você esteja no Windows, não se esqueça de utilizar o **git bash**, ao invés do **cmd**, pois o **git bash** simula um ambiente **Unix** e fica melhor de se trabalhar dessa forma):

- **git config --global user.name "Seu nome que será exibido"**

- **git config --global user.email "seu-email@email.com"**

Agora iremos criar uma chave de SSH, com ela conseguimos provar ao GitHub que nós somos os donos da nossa conta, você talvez até já tenha uma chave SSH no seu computador, podemos checar se já existe uma entrando no seu diretório **".ssh"** e listando o conteúdo:

- **cd ~/.ssh**

- **ls**

  Caso esses o comando **"ls"** retorne algo com a extensão ".pub", você não precisa executar o comando **"ssh-keygen"**. Caso contrário, digite esse comando no seu terminal, o email que você inserir tem que ser o mesmo que você usa para logar na sua conta do GitHub:

- **ssh-keygen -t rsa -b 4096 -C "seuemailgithub@email.com"**

Irá pedir um nome do arquivo para salvar a chave, não escreva nada e aperte **enter** (será gerada uma com o nome "id_rsa"). Depois, informe uma **"passphrase"**, digite uma senha e confirme ela. Pronto, você gerou uma chave SSH. Se você navegar até o local onde a chave foi criada, você verá dois arquivos, "id_rsa" e "id_rsa.pub", este com a extensão "pub", que significa público, ou seja, chave pública, é a chave que você irá informar ao **GitHub**. O arquivo sem o ".pub", é a sua chave privada, é o que você não irá informar aos outros e manter ele seguro. Digite o seguinte comando no terminal para exibir o conteúdo da chave gerada:

- **cat ~/.ssh/id_rsa.pub**

Para copiar todo o conteúdo desta chave, no caso do Windows, você pode digitar o seguinte comando:

- **clip < ~/.ssh/id_rsa.pub**

E caso você esteja em um Linux, você pode baixar um programa de apenas 16kb chamado **"xclip"**

- **sudo apt-get install xclip** (para baixar o xclip, caso você não tenha o apt-get, pode usar outro instalador para instalar o xclip)

- **xclip -sel clip < ~/.ssh/id_rsa.pub** (para copiar todo o conteúdo)

Você irá informar essa chave pública ao **GitHub**, e então todas as vezes que você quiser subir o seu código ao **GitHub** por exemplo, você usará a sua chave privada, para mostrar ao **GitHub** que foi você quem gerou essa chave pública.

Agora vá até [essa página](https://github.com/settings/keys), aqui você pode ver as chaves SSH associadas a sua conta. Clique então no botão **"New SSH Key"**. O título você pode escolher um, e em "key", você informará o conteúdo que você copiou anteriormente do seu arquivo da chave pública.

Depois de informado a sua chave **pública** ao **GitHub**, vamos agora adicionar a sua chave **privada** ao **ssh-agent**, um programa que fará a autenticação da sua máquina local, com o servidor remoto, que nesse caso seria o **GitHub**, execute então o comando [ssh-agent](https://pt.wikipedia.org/wiki/Ssh-agent), você deverá receber um "Agent pid":

- **eval \$(ssh-agent -s)**

Agora iremos executar o comando ssh-add, onde o "id_rsa" é o nome da nossa chave SSH, depois de digitar o comando, será requisitado a sua "passphrase" que você informou quando criou a chave.

- **ssh-add id_rsa**

Você deverá receber a mensagem **"Identity added: id_rsa (seuemailgithub@email.com)"**. Pronto, a sua configuração está feita.

{{< figure src="/images/posts/ssh-add.png" alt="ssh add" >}}

## Workflow

Geralmente, o workflow de quando trabalhamos com **Git** e **GitHub**, é realizar um **fork** de um determinado repositório no **GitHub** de outra pessoa, dessa forma vamos estar com uma cópia desse reposítorio no nosso perfil, depois, fazer o **clone** do **fork** para a nossa máquina local, realizar as alterações, subir as alterações feitas localmente, ao seu repositório **fork** no **GitHub**, e realizar um **pull request** do seu repsitório para o repositório principal.

Caso você nunca tenha viso algum desses nomes antes, não se assuste, pois vamos ver tudo isso neste artigo.

---

### "Fork"

Vamos começar pegando um repositório base no **GitHub** e realizar o **fork** deste. Vá até [essa página](https://github.com/buteco-tech/artigo-tutorial-git) e clique no botão **Fork** no canto superior direito, isso fará uma cópia do repositório para o seu perfil no **GitHub**.

Agora navegue até os repositórios do seu perfil, você verá que existe um ali chamado **"artigo-tutorial-git"**, clique nele. Agora você não está no repositório principal, você está no seu repositório próprio, as alterações feitas ali, não terão impacto no repositório principal, a menos que você faça um **Pull Request**, que é o que iremos fazer daqui a pouco.

### "git clone"

Vamos agora clonar esse seu repositório para a sua máquina local, para que você possa realizar alterações nele Na página do seu repositório no GitHub, existe um botão chamado **"Code"**, clique nele, selecione a opção **SSH**, e clique no botão de copiar.

{{< figure src="/images/posts/git-clone-btn.png" alt="ssh add" >}}

Agora abra o seu terminal no local que deseja copiar o repositório, e digite no **"git clone"**, seguido do comando que você copiou. Será pedido para que você informe a sua "passphrase" de quando criou a sua chave SSH. Depois digite **"cd artigo-tutorial-git"**, para navegar com o seu terminal até a pasta do repositório clonado. Você ficará com o terminal assim:

{{< figure src="/images/posts/git-clone.png" alt="git clone" >}}

Perceba que existe ali, uma pasta chamada **".git"**, é nela que o git guarda todas as informações do seu repositório, todas as alterações, o endereço do repositório remoto, tudo o que é relacionado ao controle de versão do git é guardado dentro dela. No Windows talvez você não consiga visualizar esta pasta, pois ela é um arquivo oculto, então é só seguir [esse tutorial](https://support.microsoft.com/pt-br/help/14201/windows-show-hidden-files), que já vai ser possível visualizar ela.

### "git status" e "git add"

Vamos criar um novo arquivo para que possamos comitar ele depois, então, crie um arquivo de texto com o seu nome por exemplo.

Voltando pro terminal , digite **"git status"**. Você verá essa mensagem:

{{< figure src="/images/posts/git-status.png" alt="git status" >}}

Essas "Untracked files" são as alterações pendentes, nós precisamos adicionar elas à área de **"staging"**, as alterações que estão no **"staging"** são as que serão **"commitadas"** futuramente.

Então, digite **"git add ."**, para adicionar tudo. Se você quisesse adicionar apenas um arquivo por exemplo, daí seria **"git add nome-do-arquivo"**

O **"git add"** não afeta diretamente o repositório, pois ele ainda não "salvou" as alterações, apenas às moveu para a área de **"staging"**, quando fazermos um **"git commit"** é que as alterações serão salvas.

Agora sim, podemos commitar as alterações, então vamos lá, **"git commit -m 'Meu primeiro commit'"**, o -m é para informar uma mensagem que será gravada junto ao commit.

Se você rodar um **"git status"**, o seu terminal deverá estar parecido com o abaixo.

{{< figure src="/images/posts/git-commit.png" alt="git commit" >}}

Agora é hora de subir as alterações feitas no seu repositório local, ao repositório remoto, nesse caso, o do GitHub.

---

### "git push"

Quando estamos com alguma alteração commitada localmente e queremos subir elas para o repositório remoto, precisamos fazer um **"git push"**, então digite **"git push"** no seu terminal e recarregue a página do seu repositório no **"GitHub"**, você verá que as alterações feitas localmente, já estão lá, mas atenção, quando você fez o **push**, as alterações foram para o seu repositório **fork**, não para o repositório principal.

### Abrindo uma "Pull request"

Se você for até o seu repositório agora, depois de ter feito um **push** nele, você vai ver um botão escrito **"Pull request"**. Daí só clicar nele.

{{< figure src="/images/posts/pull-request-btn.png" alt="pull request" >}}

Irá abrir uma página mostrando os arquivos alterado em um botão **"Create pull request"**, clique nele, agora informe o título e um comentário para essa **pull request** e clique naquele mesmo botão **"Create pull request"**, e pronto, está criada. Se você for até a página das **pull requests** do repositório principal, a sua vai estar lá. Agora para a sua alteração entrar de fato no repositório principal, a sua **pull request** tem que ser aceita por algum mantedor do repositório.

### O que é "pull request"?

Desde o começo desse artigo, tudo girou em torno deste momento, em que você cria a sua **pull request**, mas afinal, o que é uma **pull request**?

Para entendermos o que é uma **pull request** precisamos entender o que é um **pull**. Quando existe alguma alteração no repositório remoto, e você quer baixar essa alteração para o seu repositório local, ou seja, o seu repositório local está atrasado em relação ao repositório remoto, você digita **"git pull"** no terminal (olha só, mais um comando novo). Uma **pull request** é uma sugestão de uma alteração, ou seja, você está pedindo para que aquele repositório que você enviou a **pull request**, faça um **"pull"** com as suas alterações.

---

### O fim, se existe, está muito distante ainda

Se você chegou até aqui, então aprendeu bastante coisa, viu sobre como fazer um **fork** de um repositório no **GitHub**, clonar ele para a sua máquina local com **git clone**, adicionar as suas alterações a ele com **git add**, commitar elas com **git commit**, subir as suas alterações ao repositório remoto com **git push**, abrir uma **pull request** com as alterações.

Mas ainda não acabou por aí, o próximo passo, é você estudar sobre **branches** e como elas funcionam e são usadas. E claro, contribuir com muito código **open source**.

E ai, você já usava Git antes? O que achou? Deixe um comentário abaixo.