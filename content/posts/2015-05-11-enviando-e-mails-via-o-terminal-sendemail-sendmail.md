---
title: Enviando e-mails pelo terminal com sendEmail
tagline: sendEmail != sendmail
summary: |
  Tutorial sobre como utilizar o sendEmail pelo terminal para enviar e-mails
date: 2015-05-11 22:54:26
authors:
  - marcossouza
slug: enviando-emails-pelo-terminal-com-sendemail
aliases:
  - enviando-e-mails-via-o-terminal-sendemail-sendmail
categories:
  - Tutoriais
tags:
  - linux
  - terminal
  - email
  - smtp
  - gmail
images:
  - /images/posts/sendEmail-script.png
---

Está dica é um script em Perl maroto que conheço e uso a muito tempo para envio de e-mails, ele é muito prático, chamado [sendEmail][sendemail], que não é o nosso tão conhecido servidor de e-mail [sendmail][sendmail].

O [sendEmail][sendemail] é um aplicativo de linha de comando que funciona como um cliente de e-mail SMTP. É a maneira ideal de enviar e-mails através do terminal.

{{< figure src="/images/posts/sendEmail-script.png" alt="sendEmail" >}}

Você pode instalar no Debian/Ubuntu com o comando `apt install sendemail`, no Fedora/CentOS com o comando `yum install sendemail` ou [baixar diretamente no site do projeto][sendemail] caso esteja usando alguma outra distribuição, BSD, Windows ou OSX.

E antes de mais nada, SIM!, existem milhares de formas de enviar e-mail! Use a que você achar melhor!

Um exemplo de uso com o Gmail:

```bash
sendEmail -f meu-email@gmail.com \
  -t destino@gmail.com \
  -s smtp.gmail.com:587 \
  -u "Teste via terminal" \
  -m "Email enviado com o sendEmail!" \
  -a /arquivo.txt \
  -xu meu-email@gmail.com \
  -xp 'senha' \
  -o tls=yes
```

Explicando cada parâmetro:

**-f** define o remetente do e-mail

**-t** define o destinatário, caso haja mais de um separar por virgula (ex: `joao@gmail.com,maria@gmail.com`)

**-s** define o endereço do servidor SMTP

**-u** define o assunto do e-mail

**-u** define o texto do e-mail

**-a** permite anexar um arquivo

**-xu** define o usuário de envio no servidor SMTP

**-xp** define a senha do usuário no servidor SMTP

**-o** permite definer outras opções, no caso do Gmail se deve utilizar TLS (`tls=yes`)

Ao executar o script será enviado o e-mail caso tudo esteja correto (domínio, porta, usuário, senha), ou caso haja erro, é retornado a resposta do servidor de e-mail.

Um possível erro de exemplo:

`ERROR No TLS support! SendEmail can't load required libraries. (try installing Net::SSLeay and IO::Socket::SSL)`

Esta mensagem quer dizer que falta instalar os módulos `Net::SSLeay` e `IO::Socket::SSL`, procure no seu sistema como instalar, existem várias maneiras via cpan, ou via gerenciador de pacotes.

**Referências**

* [Projeto sendEmail][sendemail]
* [Enviando e-mails pelo terminal - Viva o Linux][vivaolinux]

[sendmail]: https://linux.die.net/man/8/sendmail.sendmail
[sendemail]: http://caspian.dotconf.net/menu/Software/SendEmail/
[vivaolinux]: https://www.vivaolinux.com.br/artigo/Enviando-emails-pelo-terminal/
