---
title: Criando um git server local
summary: |
  Mini tutorial de como criar um servidor git local.
date: 2015-09-15 00:35:22
authors:
  - marcossouza
slug: criando-um-git-server-local
images:
  - /images/wp-content/uploads/2014/11/Git-Icon-1788C.png
categories:
  - tutoriais
  - ferramentas
tags:
  - git
  - ssh
  - linux
---

{{< figure src="/images/wp-content/uploads/2014/11/Git-Icon-1788C.png" alt="Git" width="250" >}}

Algumas vezes queremos ter um ponto comum em uma rede local onde podemos enviar nossos *commits*, especialmente quando estamos trabalhando em equipe. Pois bem, a solução é simples.

Para tal feito, basta ter uma máquina Linux com `sshd` rodando e `git` instalado. Feito isto, vá até um local onde queira criar o repositório, podendo ser em um `$HOME` de um usuário, como por exemplo:

```bash
mkdir /home/fulano/projeto.git
cd /home/fulano/projeto.git
```

Ao executar o famoso `git init`, é preciso passar um parâmetro adicional:

`git init --bare`

Com este comando o git cria um repositório "nú". Ao executar o `git init`, este repositório se torna um *working directory*. Esse tipo de repositório tem um diretório `.git` onde uma cópia dos arquivos da pasta do projeto e todas as configurações são guardadas.

Já nos "repositórios nús", os arquivos de configuração são colocados na própria pasta raiz do projeto ao invés da pasta `.git`, e com isso não existe uma *working tree*, com uma cópia dos seus arquivos lá dentro.

Para poder acessar os arquivos do seu novo repositório, basta executar o comando:

`git clone fulano@ip-do-git-server:/home/fulano/projeto`

O `git` utiliza o protocolo SSH para fazer as transferências de *commits*, então basta você informar a senha do usuário do SSH e tudo funcionará.

Ao fazer `git push` você também precisa informar a senha do usuário do SSH. Mas para evitar, basta executar um `ssh-copy-id`, assim, não será necessário informar a senha em cada *push*.

Espero que este artigo tenha sido útil para você! Dúvidas, críticas ou sugestões? Deixe um comentário.

Até a próxima!

**Referências**

- [Git init manual](https://www.kernel.org/pub/software/scm/git/docs/git-init.html)
- [ssh-copy-id manual](https://linux.die.net/man/1/ssh-copy-id)
