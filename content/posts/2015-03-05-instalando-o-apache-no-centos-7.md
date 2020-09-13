---
title: Instalando o Apache Server no CentOS 7
summary: |
  O Apache é um dos mais bem sucedido servidor web livre, representando cerca de 47% dos servidores ativos no mundo.
date: 2015-03-05 00:20:38
authors:
  - alexandrevicenzi
slug: instalando-o-apache-no-centos-7
images:
  - /images/wp-content/uploads/2015/03/Apache-Logo.png
categories:
  - tutoriais
  - distros
tags:
  - apache
  - centos
  - httpd
  - php
  - wordpress
---

{{< figure src="/images/wp-content/uploads/2015/03/Apache-Logo.png" alt="K8s" width="200" >}}

O [Apache HTTP Server](http://httpd.apache.org/docs/) é um dos mais bem-sucedido servidor web livre, representando cerca de 47% dos servidores ativos no mundo.

O Apache é um dos projetos que é mantido pela [Apache Software Foundation](https://www.apache.org/).

## Instalação

Para instalar o Apache execute o comando:

`yum install httpd`

Ao término da instalação deverá ser exibida uma mensagem de concluído.

Para iniciar o serviço do Apache execute o comando:

`systemctl start httpd.service`

Para adicionar o Apache na inicialização do sistema execute o comando:

`systemctl enable httpd.service`

## Teste

Após iniciado basta digitar no seu browser [http://localhost](http://localhost) e você deverá ver uma página com a seguinte mensagem:

`Testing 123.. page`

## Pacotes adicionais

Para utilizar com o Worpress, é necessário instalar alguns pacotes adicionais.

Execute o comando a seguir para instalar os pacotes necessários para a hospedagem do Worpress no seu servidor:

`yum install php php-mysql php-gd`

Após isto reinicie o Apache com o comando:

`systemctl restart httpd.service`

## Conclusão

A instalação do Apache em si é bem simples. Espero que tenha gostado.

Até a próxima.
