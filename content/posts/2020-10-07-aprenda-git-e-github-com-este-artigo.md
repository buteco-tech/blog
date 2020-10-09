---
title: "Tutorial de Git e GitHub"
summary: "Se você é ou pretende se tornar um desenvolvedor, já deve ter ouvido falar do Git e GitHub, um sistema do controle de versão e um repositório online para os nossos projetos. Agora é a hora de conhecer mais sobre essas ferramentas e aprender a usá-las."
tagline: "Iniciando com Git e GitHub"
date: 2020-10-07 08:00:00
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

## Configuração do ambiente REVER REVER

É necessário ter o Git instalado no seu computador e uma conta no GitHub criada.

Ambos os passos são bem rápidos.

Você pode baixar o Git [aqui](https://git-scm.com/downloads), é só selecionar o seu sistema operacional e seguir o setup, no caso do Linux, costuma-se instalar o Git via um gerenciador de pacotes, nesse caso, é só você ir até [essa página](https://git-scm.com/download/linux), e executar no seu terminal o comando específico da sua distribuição. E o GitHub você cria uma conta [aqui](https://github.com/).

É necessário configurar o seu email e nome, que será exibido em seus commits, para isso é só digitar no terminal o seguinte.

- **git config --global user.name "Seu nome que será exibido"**

- **git config --global user.email "seu-email@email.com"**

Agora iremos criar uma chave de SSH para provar ao GitHub que nós somos os donos da nossa conta no GitHub, isso é feito por meio de uma chave SSh, digite então esse comando no seu terminal (o email que você inserir tem que ser o mesmo que você usa para logar na sua conta do GitHub):

- **ssh-keygen -t rsa -b 4096 -C "seuemailgithub@email.com"**

Irá pedir um nome do arquivo para salvar a chave, digite algo como "githubkey". Depois será pedido uma "passphrase", digite uma senha e confirme ela em seguida. Pronto, você gerou uma chave SSH. Se você navegar até o local onde a chave foi criada, que é o local onde você estava executando o terminal, você verá dois arquivos, "githubkey" e "githubkey.pub", este coma extensão "pub", que significa público, ou seja, chave pública, é a que você irá informar ao **GitHub**. O arquivo sem o ".pub", é a sua chave privada, é o que você não irá informar aos outros e manter ele seguro.

Você irá informar essa chave pública ao **GitHub**, e então todas as vezes que você quiser se conectar ao GitHub, ou subir o seu código ao **GitHub** por exemplo, você usará a sua chave private, para mostrar ao **GitHub** que foi você quem gerou essa chave pública. Agora o que você precisa fazer é abrir o arquivo da chave pública e copiar todo o conteúdo dele.

Agora vá até [essa página](https://github.com/settings/keys), aqui você pode ver as chaves SSH associadas a sua conta. Clique então no botão **"New SSH Key"**. O título você pode escolher um nome, e em "key", você informará o conteúdo que você copiou anteriormente do seu arquivo da chave pública, cuide para que todo o arquivo seja copiado, dês de "ssh-rsa..." até o seu email.

## Workflow

Geralmente, o workflow de quando trabalhamos com **Git** e **GitHub**, é realizar um **fork** de um determinado repositório no **GitHub** de outra pessoa, dessa forma vamos estar com uma cópia desse reposítorio no nosso perfil, depois, fazer o **clone** do **fork** para a nossa máquina local, realizar as alterações, subir as alterações feitas localmente, ao seu repositório **fork** no GitHub, e realizar um **Pull Request** do seu repsitório para o repositório principal.

Caso você nunca tenha viso algum desses nomes antes, não se assuste, pois vamos ver tudo isso neste artigo.

### "Fork"

Vamos começar pegando um repositório base no GitHub e realizar o **fork** deste. Vá até (essa página)[https://github.com/LeoDalcegio/aprenda-git-e-github] e clique no canto superior direito no botão **Fork**, isso fará uma cópia do repositório para o seu perfil no GitHub.

Agora navegue até os repositórios do seu perfil, você verá que existe um ali chamado **"aprenda-git-e-github"**, clique nele. Agora você não está no repositório principal, você está no seu repositório próprio, as alterações feitas ali, não terão impacto no repositório principal, a menos que você faça um **Pull Request**, e é isso que vamos fazer daqui a pouco.

### "git clone"

Vamos agora clonar esse seu repositório para a sua máquina local,

TERMINAR ISSO AQUI E USAR SSH FALAR DA PASTA .git DAÍ, NO WINDOWS TALVEZ n SEJA POSSIVEL VISUALIZAR ELA

---

### "git status" e "git add"

Vamos criar um novo arquivo para comitar ele depois, então, crie um arquivo de texto com o seu nome por exemplo.

Voltando pro terminal, digite **"git status"**. Você verá essa mensagem:

{{< figure src="/images/posts/git-status.png" alt="git status" >}}

Essas são as alterações pendentes, nós precisamos adicionar elas à área de **"staging"**, as alterações que estão no **"staging"** são as que serão **"commitadas"** futuramente.

Então, digite **"git add ."**, para adicionar tudo. Se você quisesse adicionar apenas um arquivo, daí seria **"git add nome-do-arquivo"**

O **"git add"** não afeta diretamente o repositório, pois ele ainda não "salvou" as alterações, apenas às moveu para a área de **"staging"**, quando fazermos um **"git commit"** é que as alterações serão salvas.

Agora sim, podemos commitar as alterações, então vamos lá, **"git commit -m 'Primeiro commit'"**, o -m é para informar uma mensagem que será gravada junto ao commit.

O seu terminal ficará parecido com o abaixo.

{{< figure src="/images/posts/git-commit.png" alt="git commit" >}}

Agora é hora de subir as alterações feitas no seu repositório local, ao repositório remoto, nesse caso, o do GitHub.

---

### "git push"

Quando estamos com alguma alteração commitada localmente e queremos subir elas para o repositório remoto, precisamos fazer um **"git push"**, então digite **"git push"** no seu terminal e recarregue a página do seu repositório no **"GitHub"**, você verá que as alterações feitas localmente, já estão lá.

### Abrindo uma "Pull request"

Se você for até o seu repositório, você vai ver uma...

Pronto, agora vá até a página das **pull requests** do repositório principal, a sua vai estar lá. Agora é só esperar ela ser aceita por algum mantedor, que a sua alteração estará lá.

---

### O fim, se existe, está muito distante ainda

Se você chegou até aqui, então aprendeu bastante coisa, viu sobre como fazer um **fork** de um repositório no **"GitHub"**, clonar ele para a sua máquina local com **git clone**, adicionar as suas alterações a ele com **git add**, commitar elas com **git commit**, subir as suas alterações ao repositório remoto com **git push**, abrir uma **pull request** com as alterações.

Mas ainda não acabou por aí, o próximo passo, é você estudar sobre **branches** e como elas funcionam e são usadas.

E ai, você já usava Git antes? O que achou? Deixe um comentário abaixo.
