---
title: "Aprenda Git e GitHub com este artigo"
summary: "Se você é ou pretende se tornar um desenvolvedor, já deve ter ouvido falar do Git e GitHub, um sistema do controle de versão e um repositório online para os nossos projetos. Agora é a hora de conhecer mais sobre essas ferramentas e aprender a usá-las."
tagline: "Tutorial para iniciantes em Git e GitHub"
date: 2020-06-16 08:00:00
slug: aprenda-git-e-github-com-este-artigo
authors:
  - leonardodalcegio
categories:
  - desenvolvimento
  - tutoriais
tags:
  - git
  - github
images:
- /images/posts/lxc-logo.png
---

{{< figure src="/images/posts/lxc-logo.png" alt="LXC" >}}

Se você é ou pretende se tornar um desenvolvedor, já deve ter ouvido falar do Git e GitHub, um sistema do controle de versão e um repositório online para os nossos projetos. Agora é a hora de conhecer mais sobre essas ferramentas e aprender a usá-las.

## O que é Git?

Git é um sistema de código aberto e gratuito para controle de versão, criado em 2005 por [Linus Torvalds](https://pt.wikipedia.org/wiki/Linus_Torvalds) quando estava desenvolvendo o Kernel do Linux. Controle de versão, é um sistema que permite gravar alterações em arquivos ao longo do tempo, portanto, podemos visualizar versões específicas desses arquivos posteriormente.

Na maioria dos projetos, geralmente, acontece de termos muitos desenvolvedores trabalhando na mesma base de códigos, então um sistema de controle de versão como o Git é necessário para que não haja conflito entre o código de cada desenvolvedor.

## O que é GitHub?

Se você vai utilizar git, provavelmente vai querer armazenar o seu repositório em algum lugar, existem duas formas de se fazer isso, offline, no seu próprio computador, e online, em alguma plataforma como GitHub, BitBucket ou GitLab.

E é exatamente para isso que o GitHub serve, para armazenar seu repositório nele, é o Google Drive dos códigos.

Existem muitas outras funcionalidades dele, como as "Pull Requests" por exemplo, mas isso é um assunto para outro dia. 

## Configuração do ambiente

É necessário ter o Git instalado no seu computador e uma conta no GitHub criada.

Ambos os passos são bem rápidos e você pode seguir esse tutorial aqui.

## Como criar um repositório Git?

Agora chegou a hora de criar o seu repositório Git localmente, para depois subir ele ao GitHub.

Então, crie uma pasta em qualquer lugar do seu computador, que será o local do repositório, dentro dela ficará os arquivos que você quer versionar.

Agora, navegue até ela com o seu terminal, ou clique com o botão direito sobre ela e selecione "Git Bash Here" (irá abrir o terminal do git dentro dela).

## "git init"

Digite o comando "git init" no terminal, ele irá criar uma pasta chamada ".git", caso você não consiga ver essa pasta, é por que você não consegue ver arquivos ocultos, daí é só seguir [esse tutorial](https://support.microsoft.com/pt-br/help/14201/windows-show-hidden-files) que já vai ser possível visualizar.

É dentro dessa pasta ".git" que ficarão os arquivos que [comitaremos](https://pt.wikipedia.org/wiki/Commit) no nosso repositório.

-imagem

### "git status" e "git add"

Precisamos criar um novo arquivo para poder comitar ele, então, crie um arquivo de texto qualquer.

A sua pasta ficará parecida com isso.

COLOCAR IMAGEM COM O .git E O Novo arquivo de texto.txt

Voltando pro terminal, digite git status. Você verá essa mensagem:

-- mostrar imagem

Essas são as alterações pendentes, elas ainda não estão dentro da pasta ".git", para colocarmos elas lá, precisamos comitar as alterações, mas antes de comitar, precisamos definir o que é que queremos comitar, para isso existe o comando "git add".

Nesse caso, digite "git add .", para comitar tudo, se você quisesse comitar apenas um arquivo, daí seria "git add nome-do-arquivo"

Agora, ao digitar "git commit", estaremos colocando as alterações para dentro da pasta ".git".

Então vamos lá, "git commit -m 'Primeiro commit'", o -m é para informar uma mensagem que será gravada junto ao comit.

Agora que você está com o seu repositório criado, é hora de subir ele ao GitHub.

### Subindo um repositório no GitHub

Se você entrar na página inicial do [GitHub](https://github.com/), vai ver no canto esquerdo da tela, um botão chamado "new", clique nele, irá abrir [essa página](https://github.com/new).

-- colocar imagem da pagina (tem no meu notion)

Digite o nome do seu repositório, uma descrição para ele e defina se outras pessoas vão poder visualizar ele, definindo se ele é público ou privado, as outras opções vamos ignorar por enquanto.

Agora é só clicar em "Create repository".

Você verá essa página:

E pronto, o seu repositório já está criado, mas ainda é necessário subir para ele as alterações que foram feitas na sua máquina local.

### Conectando repositório local com o remoto

Execute o primeiro comando da imagem anterior, "", para "conectar" o seu repositório local, ao repositório do GitHub.

Agora execute o segundo comando, "git push", é para enviar as informações commitadas no repositório local, ao repositório remoto. 

Caso você tenha acabado de instalar o Git no seu computador, talvez seja necessário configurar o seu email e nome, que será exibido em seus commits.

Para isso é só digitar no terminal o seguinte.

git config --global user.name "Seu nome que será exibido".

git config --global user.email "seu-email@email.com".

Agora, para não deixar batido, como que seria se você ou outra pessoa quisesse baixar esse repositório futuramente?

Simples, é só abrir o terminal do git em qualquer lugar do seu computador, e digitar "git clone url-do-seu-repositorio".git.

Dali em diante você vai poder ir fazendo os seus "git add", "git commit", "git push" e muitos outros comandos.

### O fim, se existe, está muito distante ainda

Se você chegou até aqui, então aprendeu bastante coisa, viu sobre como criar um repositório git local, adicionar as suas alterações com git add, commitar elas com git commit, conectar o seu repositório local com o remoto com git remote add, subir as suas alterações com git push.

Mas ainda não acabou por aí, o próximo passo, é você estudar sobre fork, pull request e como eles funcionam e são usados, para poder contribuir com outros repositórios e com o Open Source.

E ai, você já usava Git antes? O que achou? Deixe um comentário abaixo. 
