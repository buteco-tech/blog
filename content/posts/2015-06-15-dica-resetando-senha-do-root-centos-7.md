---
title: Resetando a senha do root no Centos 7
summary: |
  Aprenda a resetar a senha do root em sistemas da Red Hat.
date: 2015-06-15 23:13:07
authors:
  - marcooliveira
slug: dica-resetando-senha-do-root-centos-7
categories:
  - Tutoriais
  - Distros
tags:
  - linux
  - centos
images:
  - /images/wp-content/uploads/2015/06/password-error.jpg
---

{{< figure src="/images/wp-content/uploads/2015/06/password-error.jpg" alt="PWD Error" >}}

Dica rápida para este processo, trivial do cotidiano de um administrador que pega ambientes dos quais não existem documentações, ou daqueles que como eu se esquecem da senha do root de sua máquina virtualizada... XD

No grub entre no modo de edição da parametrização do boot do kernel que você utiliza. Pressionando "E".

O modo de edição do grub permite navegar utilizando as teclas de setas, e a hotkey **CTRL + E** para ir ao final da linha. Procure a linha onde existe a configuração **linux16** e no final dela adicione "**rd.break enforcing=0"**. Conforme a imagem a seguir:

{{< figure src="/images/wp-content/uploads/2015/06/reset2.png" alt="PWD Reset" >}}

Pressione **CTRL + X** para bootar no kernel modificado.

Na sequência ele deve iniciar o sistema operacional no modo de quebra, para tentar resolver algum problema que tenha ocorrido no sistema antes dele inicializar ( **rd.break** ), e desabilitar o **SELINUX** temporariamente pois não precisamos para fazer esta alteração.

Para melhor exemplificar confira a abaixo imagem com todos os comandos necessários executar para resetar a senha:

{{< figure src="/images/wp-content/uploads/2015/06/reset4.png" alt="PWD Reset" >}}

No modo de quebra, montamos o disco do nosso sistema no modo de escrita.

`# mount -o remount,rw /sysroot`

Utilizamos o **chroot** para alternar as pasta raíz do sistema, para que possamos utilizar dos programas de onde o sistema operacional se encontra instalado no disco.

`# chroot /sysroot`

A próxima etapa é alterar a senha com o comando **passwd**.

`# passwd`

Na sequência remontamos a raiz do sistema no modo de leitura.

`# mount -o remount,ro /`

Por fim saímos do **chroot ** e do modo **rd.break** executando duas vezes o comando **exit**. O sistema em sequência deve carregar o ambiente agora com a nova senha de root.

**Referências**

* [Terminal Menu Editing During Boot](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/system_administrators_guide/ch-working_with_the_grub_2_boot_loader#sec-Terminal_Menu_Editing_During_Boot) - Documentação oficial da Red Hat
