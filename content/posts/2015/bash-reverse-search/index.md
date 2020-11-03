---
type: wordpress
title: "Bash: Reverse Search"
summary: |
  Este post tenta mostrar como funciona o recurso Reverse-Search do Bash.
date: 2015-12-16 21:22:36
authors:
  - marcossouza
slug: bash-reverse-search
images:
  - /images/wp-content/uploads/2015/12/bash.png
categories:
  - tutoriais
  - ferramentas
tags:
  - bash
  - console
  - linux
  - shell
---

No Bash, é comum reexecutarmos comandos o tempo inteiro, especialmente quando precisamos compilar um programa ou mover um arquivo. O que se faz geralmente é ir apertando a tecla "para cima" e encontrar o comando desejado. Essa tarefa pode ser um pouco custosa, uma vez que o comando pode ter sido executado há horas e então, ficar voltando enumeros comandos não deve ser muito interessante.

<!--more-->

Dessa forma, o Bash tem uma função chamada de <em>Reverse-Search</em>. Ela é basicamente uma procura no seu histórico de comandos, na ordem inversa da executada. Para usar este recurso, basta abrir o Bash e apertar <code>Ctrl-R</code>. A imagem abaixo mostra este recurso sendo utilizado:
<a href="/images/wp-content/uploads/2015/12/Screenshot-from-2015-12-15-00-26-41.png"><img class="alignnone size-full wp-image-4288" src="/images/wp-content/uploads/2015/12/Screenshot-from-2015-12-15-00-26-41.png" alt="Screenshot from 2015-12-15 00-26-41" width="729" height="122" /></a>

Depois de iniciado o <em>Reverse-Search</em>, basta digitar o que deseja procurar. Na imagem abaixo, eu digitei "rm", então a busca me trouxe um comando cat, que eu executei em um arquivo do /sys. Ele mostrou este comando por causa do caminho do cat, que continha a string "drm":
<a href="/images/wp-content/uploads/2015/12/Screenshot-from-2015-12-15-00-31-39.png"><img class="alignnone size-full wp-image-4289" src="/images/wp-content/uploads/2015/12/Screenshot-from-2015-12-15-00-31-39.png" alt="Screenshot from 2015-12-15 00-31-39" width="566" height="23" /></a>

Se a sua busca não foi exatamente como você queria, basta apagar e reescrevê-la, ou então pressionar <code>Ctrl-R</code> repetidamente para que o <em>Reverse-Search</em> continue buscando por comandos mais antigos com a mesma string que você digitou (a busca é dos comandos mais novos para os mais antigos). O campo de busca do <em>Reverse-Search</em> é como o Bash, você pode digitar e apagar o que escreveu, que ele busca conforme o for digitando.

Quando você encontrar o comando desejado, basta pressionar <code>Enter</code> que o comando é executado, ou então apertar no <code>Tab</code> que o comando cai no bash para que você possa editá-lo e então executá-lo.

Para sair do <em>Reverse-Search</em> basta digitar <code>Ctrl-R</code>.

Espero que tenham gostado desse post sobre o Bash! Até a próxima!
