---
title: "Street Fighter X Mega Man no Linux"
summary: "Jogo gratuito criado pela Capcom e Zong Hui em 2012 para comemorar os 25 anos da franquia, curta a nostalgia e relembre sua infância."
tagline: "Curta a nostalgia e relembre sua infância"
date: 2020-12-08 08:00:00
categories:
  - jogos
tags:
  - linux
  - wine
  - capcom
  - street-fighter
  - mega-man
slug: street-fighter-x-mega-man-linux
authors:
  - alexandrevicenzi
resources:
- name: thumbnail
  src: sfxmm.jpeg
---

{{<figure src="thumbnail" alt="Street Fighter X Mega Man">}}

Em uma colaboração entre a [Capcom][capcom-br] e seus fãs para o aniversário de 25 anos de Street Fighter e Mega Man, Zong Hui desenvolveu Street Fighter X Mega Man, um *crossover* entre os personagens de ambos os jogos.

O jogo foi disponibilizado de forma gratuita inicialmente em 2012 e atualizado em 2013. Para quem quiser matar a saudade e relembrar algumas memórias, é possível baixar o jogo no site da Capcom.

{{<button url="http://megaman.capcom.com/sfxmm/" target="_blank" text="Baixar jogo" location="center" color="danger" >}}

O jogo foi desenvolvido para Windows, mas é possível executar no Linux facilmente através do [Wine][wine] ou [Play on Linux][pol].

Instale os pacotes necessários no Wine:

```bash
WINEARCH=win32 winetricks directmusic dsound d3dx9
```

Execute o jogo:

```bash
WINEARCH=win32 wine SFXMMv2_US.exe
```

Confira uma prévia do jogo:

{{<youtube id="bOgogzz8o5w">}}

_Todas as informações, como som, vídeo, texto, música, fotos, logotipos e ilustrações neste jogo são de propriedade ou controlados pela Capcom e protegidos por leis de direitos autorais e tratados internacionais. Venda, modificação, adaptação, tradução e replicação são estritamente proibidos._

[capcom-br]: http://www.capcom-unity.com.br/
[wine]: https://www.winehq.org/
[pol]: https://www.playonlinux.com/en
