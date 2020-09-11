---
title: "Virsh: Criando e gerenciando VMs pelo terminal"
summary: |
  Conheça o virsh, ferramenta baseada na libvirt para gerenciar máquinas virtuais no Linux.
date: 2015-08-25 23:44:58
authors:
  - marcossouza
slug: virsh-criando-e-gerenciando-vms-pelo-terminal
images:
  - /images/wp-content/uploads/2015/06/libvirtLogo.png
categories:
  - Tutoriais
  - Ferramentas
tags:
  - linux
  - kvm
  - qemu
  - terminal
  - virsh
  - virt-manager
  - virtualização
---

{{< figure src="/images/wp-content/uploads/2015/06/libvirtLogo.png" alt="libvirt" >}}

O [virsh][virsh] é um utilitário criado para gerenciar máquinas virtuais de tecnologias como KVM, Xen, VMware ESX, QEMU entre outras. Esse suporte se deve ao fato do [virsh][virsh] ser construído utilizando a [libvirt][libvirt] como base.

De uma forma simples, a [libvirt][libvirt] é uma API/daemon criada pela Red Hat para gerenciar máquinas virtuais. Assim como o [virsh][virsh], outras ferramentas são baseadas na libvirt para gerenciar VMs, como [virt-manager][virt-manager], OpenStack e oVirt. Apesar de ser escrita em C, existem bindings para outras linguagens, como Python, Perl, Ruby, Java e PHP.

Para instalar os pacotes necessários para este tutorial em uma distro Red Hat-like (Fedora, CentOS), execute o seguinte comando:

`sudo yum install qemu-img libvirt-client virt-viewer virt-install`

Para iniciar, vamos criar um HD para nossa VM. Na verdade, o HD é um arquivo dentro do sistemas de arquivos da sua distro, onde a VM irá utilizar como se fosse um HD propriamente dito.

`qemu-img create -f qcow2 guest.qcow2 8192M`

O [qemu-img][qemu-img] é um utilitário de imagens de disco do QEMU. Com ele é possível aumentar e diminuir o tamanho das imagens de disco, converter entre formatos, e muito mais. No comando acima, estamos criando uma nova imagem de disco, com o formato qcow2 (que é o padrão do QEMU, mas podem ser utilizados outros formatos como VMDK, VHDX, RAW, VDI e outros), com 8G de espaço e com o nome de guest.qcow2.

O comando a seguir cria uma nova máquina virtual utilizando o o disco previamente criado com uma imagem live do Fedora:

`virt-install -r 1024 -f guest.qcow2 -n Fedora_Guest --cdrom /isos/Fedora-Live-LXDE-x86_64.iso --virt-type kvm --network bridge=virbr0 --vnc`


**-r**: memória RAM em megabytes

**-f**: disco onde será instalado o sistema

**-n**: nome da máquina virtual

**--cdrom**: local para uma ISO de uma distro da sua escolha

**--network**: tipo de rede que você deseja habilitar para a VM (para desabilitar a rede basta informar none)

**--vnc**: inicia um VNC server para esta VM (utilizado posteriormente)

O parâmetro **virt-type** merece um destaque, pois ele informa o tipo de virtualização que será utilizado na máquina. Na minha máquina em questão, meu processador tem suporte a KVM (Kernel Virtual Machine), que possibilita uma maior desempenho quando executando uma VM. Para verificar se o seu processador tem essa tecnologia, execute:

`virsh capabilities`

Se no output deste comando houver uma tag como abaixo, o seu processador tem esta tecnologia. Caso não tenha, utilize qemu como parâmetro do virt-type (`--virt-type qemu`).

```xml
<domain type='kvm'>
    <emulator>/usr/bin/qemu-kvm</emulator>
    <machine canonical='pc-i42.3' maxCpus='255'>pc</machine>
    <machine maxCpus='255'>pc-1.3</machine>
    <machine maxCpus='255'>pc-0.12</machine>
    <machine maxCpus='255'>pc-q35-1.6</machine>
```

Após executar este comando, vemos algo do tipo:

```bash
[marcos@xfiles ~]$ virt-install -r 1024 -f guest.qcow2 -n Fedora_Guest --cdrom /isos/Fedora-Live-LXDE-x86_64.iso --virt-type kvm --network bridge=virbr0 --vnc
WARNING  Graphics requested but DISPLAY is not set. Not running virt-viewer.
WARNING  No console to launch for the guest, defaulting to --wait -1

Starting install...
Criando o domínio...                                     |    0 B  00:00:00
Domain installation still in progress. Waiting for installation to complete.
```

Se você executar estes comandos no console de uma máquina com GUI, o virt-viewer irá aparecer mostrando a execução da VM, como o Virtual Box ou virt-manager por exemplo, mas caso você esteja acessando uma máquina via ssh, nenhuma janela será mostrada e o shell ficará preso até você configurar o SO que foi iniciado.

Para poder visualizar a VM via VNC na rede, basta executar o comando abaixo:

`virt-viewer -c qemu+ssh://usuario@ip/system Fedora_Guest`

Para utilizar o VNC na mesma máquina, basta colocar `-c qemu:///sytem`.

Com este comando, será solicitado o usuário e senha do máquina que você está conectando, e após este será exibido o virt-viewer com a imagem da VM sendo executada:

{{<figure src="/images/wp-content/uploads/2015/06/Captura-de-tela-de-2015-06-15-21-23-27.png" alt="virt-viewer">}}

Após instalar a VM, o processo é todo feito como um gerenciador de VMs qualquer, com a vantagem de poder acessar as VMs por VNC.

Com o [virsh][virsh] é possível gerenciar essas VMs, com os seguintes comandos:

- `virsh list`: Lista todos as VMs criadas e seu estado de funcionamento
- `virsh shutdown`: Envia comando de desligamento para a VM
- `virsh destroy`: Faz um force shutdown na VM
- `virsh start`: Liga a VM

Com o [virsh][virsh] é possível ainda manipular discos nas VMs, adicionando e removendo discos conforme a necessidade, alterar parâmetros de rede, criando e removendo interfaces e vários outros.

O [virsh][virsh] é uma ótima ferramenta para gerenciar suas máquinas virtuais. Se você deseja uma ferramenta com interface gráfica não esquece de ver nosso [artigo sobre o virt-manager](/conhecendo-o-virt-manager), que é basicamente um front-end para o [virsh][virsh] .

Espero que tenham gostado desta pequena explicação sobre o [virsh][virsh] . Até a próxima!

**Referências**

- [virsh Manual](https://linux.die.net/man/1/virsh)
- [virt-install Manual](https://linux.die.net/man/1/virt-install)
- [virt-viewer Manual](https://linux.die.net/man/1/virt-viewer)

[virsh]: https://libvirt.org/manpages/virsh.html
[libvirt]: https://libvirt.org/
[virt-manager]: https://virt-manager.org/
[qemu-img]: https://linux.die.net/man/1/qemu-img
