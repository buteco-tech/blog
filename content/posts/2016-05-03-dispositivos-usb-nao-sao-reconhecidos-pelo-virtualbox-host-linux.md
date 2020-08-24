---
title: Dispositivos USB não são reconhecidos pelo VirtualBox (host Linux)
summary: |
  O VirtualBox é uma das mais bem conceituadas opções para emulação de sistemas operacionais em máquinas virtuais, com suporte a x86 e AMD64/Intel64. Atualmente o VirtualBox possui versões para hosts Windows, Linux, OS X, Solaris e FreeBSD.
  Porém no Linux, em alguns casos, o reconhecimento de dispositivos USB, Webcam e leitores SD Card do host no sistema guest não funciona. Esse problema é geralmente causado por falta de permissão no seu usuário no momento da execução do VirtualBox.
date: 2016-05-03 22:36:32
authors:
  - alexandrevicenzi
slug: dispositivos-usb-nao-sao-reconhecidos-pelo-virtualbox-host-linux
images:
  - /images/wp-content/uploads/2016/04/virtualbox.png
categories:
  - Tutoriais
tags:
  - linux
  - virtualbox
  - virtualização
  - vm
---

{{< figure src="/images/wp-content/uploads/2016/04/virtualbox.png" alt="VBox" width="150" >}}

O [VirtualBox](https://www.virtualbox.org/) é uma das mais bem conceituadas opções para emulação de sistemas operacionais em máquinas virtuais, com suporte a x86 e AMD64/Intel64. Atualmente o VirtualBox possui versões para hosts Windows, Linux, OS X, Solaris e FreeBSD.

Porém no Linux, em alguns casos o reconhecimento de dispositivos USB, Webcam e leitores SD Card do host no sistema guest não funciona. Esse problema é geralmente causado por falta de permissão no seu usuário no momento da execução do VirtualBox.

### VirtualBox Extension Pack

Primeiro vamos instalar o pacote de extensão que adiciona suporte a USB 2.0 e 3.0, VirtualBoxRDP e PXE boot. Faça o [download do VirtualBox Extension Pack](https://www.virtualbox.org/wiki/Downloads) para a versão do seu VirtualBox.

No VirtualBox acesse o menu Arquivo > Preferências > Extensões. Na tela de extensões clique no botão de acrescentar e escolha o arquivo que você acabou de baixar.

{{< figure src="/images/wp-content/uploads/2016/04/vbox-pref-extensoes-a.png" alt="VBox" >}}

Aguarde a instalação.

### Adicionando seu usuário ao grupo vboxusers

Primeiro vamos descobrir se seu usuário pertence ao grupo _vboxusers_.

Para isto execute o comando abaixo no terminal:

`$ groups SEU_USUARIO`

O resultado deverá ser algo similar ao abaixo:

`SEU_USUARIO : users vboxusers`

Se nesta linha incluir o _vboxusers_ já está tudo certo.

Caso não, vamos adicionar seu usuário ao grupo com o comando:

`$ sudo adduser SEU_USUARIO vboxusers`

ou

`$ usermod SEU_USUARIO -a -G vboxusers`

Após isto, faça logoff ou reinicie o computador.

Agora deverá ser possível reconhecer dispositivos conectados ao Linux no sistema hospedeiro.

{{< figure src="/images/wp-content/uploads/2016/04/vbox-usb-a.png" alt="VBox" >}}

Espero que este tutorial ajude você, assim como me ajudou.
