---
title: "#013 - Projeto de sistema embarcado com o Yocto"
description: "O que é o projeto Yocto e como é o desenvolvimento de uma distribuição Linux para sistemas embarcados? Otávio Salvador e Daiane Angolini, autores do livro Embedded Linux Development with Yocto Project, explicam isso e muito mais nesse episódio."
summary: "O que é o projeto Yocto e como é o desenvolvimento de uma distribuição Linux para sistemas embarcados? Otávio Salvador e Daiane Angolini, autores do livro Embedded Linux Development with Yocto Project, explicam isso e muito mais nesse episódio."
date: 2021-04-01 08:00:00
episode: "013"
authors:
  - alexandrevicenzi
slug: projeto-de-sistema-embarcado-com-o-yocto
categories:
  - Podcast
tags:
  - yocto
  - sistema-embarcado
  - linux
  - iot
resources:
- name: thumbnail
  src: thumbnail.png
---

O que é o projeto [Yocto][yocto] e como é o desenvolvimento de uma distribuição Linux para sistemas embarcados? Otávio Salvador e Daiane Angolini, autores do livro [Embedded Linux Development with Yocto Project][livro-otavio-daiane], explicam isso e muito mais nesse episódio.


Conversamos sobre a história do projeto e seus principais componentes (BitBake e Poky). Comparamos com o [Buildroot][buildroot] e [Linux From Scratch][lfs], e explicamos quando faz sentido usar um projeto ou outro.

Explicamos como se cria uma distribuição com Yocto, falando de receitas, camadas, configurações, complexidade das ferramentas, estratégia de atualização, segurança, robustez e muito mais.

E claro, ao se falar de Yocto, não podemos deixar de abordar temas relacionas a Linux e outros aspectos do mundo de sistemas embarcados, como hardware, limitações e restrições.

Por fim, Otávio e Daiane deixam algumas dicas importantes para quem quiser começar se aventurar e criar uma distribuição.

{{< podcast-episodio id="56Jg7eEM8UHIq0Pp4jj0UR" >}}

Livros mencionados:

* [Embedded Linux Development with Yocto Project][livro-otavio-daiane]
* [Heading for the Yocto Project][heading-for-the-yocto-project]
* [Mastering Embedded Linux Programming][mastering-embedded-linux-programming]
* [Yocto Project Reference Manual][yocto-manual]

Lista de email e demais comunidades do projeto Yocto podem ser encontradas [aqui][yocto-comunidade].

Escute em uma variedade de locais:

{{< podcast-distribuicao download="https://s3.castbox.fm/37/99/bb/557cf7499ea3eb78007811ce94.mp3" >}}

Ou assista a gravação na íntegra:

{{< youtube id="J70_sdYvgkY" >}}

### Participantes

#### Alexandre Vicenzi

Engenheiro de Software e bacharel em Ciência da Computação. Contribui com software de código aberto há quase 10 anos, além de ser co-fundador do Buteco Tecnológico.

{{< social www="https://www.alexandrevicenzi.com/" linkedin="alexandrevicenzi" twitter="alxvicenzi" >}}

#### Daiane Angolini

Daiane Angolini é Engenheira de Computação formada pela Unicamp. Desde 2006 trabalha com Sistemas Embarcados, tendo trabalhado em empresas nacionais e multinacionais. Atualmente trabalha na Foundries.io onde tem a oportunidade de interagir ativamente com vários projetos Open Source com foco em Linux Embarcado, o que inclui Yocto Project, Linux Kernel, U-Boot, entre outros.

{{< social linkedin="angolini" twitter="angolini" >}}

#### Marcos Paulo de Souza

Engenheiro de Software na SUSE. Bacharel em Ciência da Computação pela Fundação Universidade Regional de Blumenau e contribuidor de projetos livres e de código aberto. Trabalhando atualmente na SUSE como Enterprise Storage Developer.

{{< social www="http://mpdesouza.com/" linkedin="marcospsouza" twitter="omarcossouza" >}}

#### Otavio Salvador

Otávio Salvador é empresário, escritor, fundador e CTO na O.S. Systems Development Lab. Formado em Administração e Ciência da Computação pela Universidade Católica de Pelotas (UCPel), tem como principal área de atuação o Linux embarcado, onde trabalha com consultoria e desenvolvimento desde 2002. Iniciou sua trajetória junto à comunidade Open Source como desenvolvedor Debian em 1998, onde tornou-se Release Manager do Debian Installer em 2011. 

Hoje, integrante do Conselho Consultivo do Yocto Project, Otávio também contribui e participa ativamente de diversos projetos como U-Boot, Linux Kernel, OpenEmbedded e Rust, além de acompanhar de perto todos os projetos da O.S. Systems, em especial os dois grandes lançamentos da empresa, o ShellHub e o UpdateHub.

{{< social www="https://www.ossystems.com.br/" email="contato@ossystems.com.br" linkedin="otavio" twitter="otaviosalvador" >}}

[livro-otavio-daiane]: https://www.amazon.com.br/gp/product/B00LO2GFM0/ref=dbs_a_def_rwt_hsch_vapi_tkin_p1_i0
[heading-for-the-yocto-project]: https://github.com/CollaborativeWritersHub/heading-for-the-yocto-project
[mastering-embedded-linux-programming]: https://www.amazon.com.br/Mastering-Embedded-Linux-Programming-English-ebook/dp/B00YSILBYO/ref=tmm_kin_swatch_0?_encoding=UTF8&qid=&sr=
[yocto]: https://www.yoctoproject.org/
[yocto-manual]: https://docs.yoctoproject.org/ref-manual/index.html#yocto-project-reference-manual
[yocto-comunidade]: https://www.yoctoproject.org/community/
[buildroot]: https://buildroot.org/
[lfs]: http://www.linuxfromscratch.org/
