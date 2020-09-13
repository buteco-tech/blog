---
title: Recuperando dados perdidos do git-stash
summary: Confira como recuperar dados perdidos do git-stash
date: 2015-10-28 21:29:14
authors:
  - guilhermevanz
slug: recuperando-dados-perdidos-do-git-stash
images:
  - /images/wp-content/uploads/2015/10/t8cnk.jpg
categories:
  - tutoriais
  - ferramentas
tags:
  - git
  - git-stash
  - github
---

{{< figure src="/images/wp-content/uploads/2015/10/t8cnk.jpg" alt="K8s" width="400" >}}

Olá senhores e senhoras! =]

Em meu primeiro post no Buteco vou demonstrar como recuperar dados perdidos do git-stash. Talvez salvando muitas horas de trabalho que poderiam ser perdidas acidentalmente. Na realidade o método demonstrado neste artigo pode ser utilizado para recuperar qualquer objeto git perdido. Antes de mais nada, enquanto você estiver implementando alguma grande _feature_, quebre-a em pequenos pedaços e faça _commits_ regularmente. Não é uma boa ideia permanecer muito tempo sem "comitar". Tenha cuidado.

Vamos simular o cenário para mostrar o que você pode fazer quando perder dados do _stash_ do git. Em um repositório para demonstração existe apenas um arquivo, `main.c`. Ele será utilizado para demonstrar o problema e a solução. Então, nosso repositório está assim agora:

{{< figure src="/images/wp-content/uploads/2015/10/missing_data_from_stash_01.jpeg" alt="git" >}}

e existe apenas um _commit_:

{{< figure src="/images/wp-content/uploads/2015/10/missing_data_from_stash_02.jpeg" alt="git" >}}

A primeira versão do arquivo é:

{{< figure src="/images/wp-content/uploads/2015/10/missing_data_from_stash_03.jpeg" alt="git" >}}

Vamos começar a codificar alguma coisa. Para este exemplo, não é necessário uma grande mudança, é apenas alguma coisa para colocar no git stash. Para isso, será adicionado apenas uma nova linha. O git-diff após a alteração é:

{{< figure src="/images/wp-content/uploads/2015/10/missing_data_from_stash_04.jpeg" alt="git" >}}

Agora, suponha que você tenha que realizar o _pull_ de novas alterações de um repositório remoto e não quer realizar o _commit_ de suas alterações por que elas não estão terminadas. Assim, você decide joga-las para o _stash_, baixar as alterações do repositório remoto e aplicar seu código novamente. Por isso, você executa o seguinte comando para mover suas alterações para o git stash:

`git stash`

Analisando o _stash_ será possível ver que o comando funcionou como o esperado:

{{< figure src="/images/wp-content/uploads/2015/10/missing_data_from_stash_06.jpeg" alt="git" >}}

Neste momento, o código está em um local seguro e o _branch master_ está limpo e o _pull_ das mudanças pode ser realizado. Depois de ter atualizado o _branch_ local com as alterações do _branch_ remoto, é a hora de aplicar o código no _master_ novamente. Vamos supor que acidentalmente você executa:

`git stash drop`

e depois é possível verifica, com o comando `git stash list`, que o _stash_ limpou, e que o código foi removido e não foi aplicado no _branch master_. OMG! Quem poderá nos ajudar? Como você irá ver em breve, o git não apagou o objeto que contem as alterações. Ele apenas removeu a referência para ele. Para provar isso, pode ser usado o git-fsck. Este comando, verifica a conectividade e validade dos objeto na base de dados. Veja que no começo do nosso repositório foi executado o comando git-fsck e a saída foi:

{{< figure src="/images/wp-content/uploads/2015/10/missing_data_from_stash_07.jpeg" alt="git" >}}

Basicamente, git-fsck mostrou os objetos que são inalcançáveis ( flag `–unreachable` ). Como pode ser visto, ele não mostrou nenhum objeto. Depois que os elementos do _stash_ foram _dropados_  o git-fsck mostrou:

{{< figure src="/images/wp-content/uploads/2015/10/missing_data_from_stash_08.jpeg" alt="git" >}}

Na última execução pode ser visto que existem três objeto inalcançáveis. Mas qual deles é o código perdido? Temos que procurar por ele, para isso podemos usar o comando git-show para visualizar o que é cada objeto.

{{< figure src="/images/wp-content/uploads/2015/10/missing_data_from_stash_09.jpeg" alt="git" >}}

Ai está! O ID 95ccbd927ad4cd413ee2a28014c81454f4ede82c é a alteração que estávamos procurando e temos que recuperá-lo! Uma possível solução é fazer o _checkout_ do ID em um novo _branch_ ou aplicar o _commit_ diretamente. Neste exemplo será utilizado o git-stash para aplicar o _commit_ no _branch master_ novamente.

`git stash apply 95ccbd927ad4cd413ee2a28014c81454f4ede82c`

É importante relembrar, git executa seu _garbage collector_ (gc) periodicamente. Após a execução do gc não será mais possível ver os objetos inalcançáveis.

**Referência**

* [git-fsck](https://git-scm.com/docs/git-fsck)
* [git-stash](https://git-scm.com/docs/git-stash)
