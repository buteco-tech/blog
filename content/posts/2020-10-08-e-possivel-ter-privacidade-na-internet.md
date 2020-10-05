---
title: "É possível ter privacidade na internet?"
summary: "Sim, é possível ter privacidade na internet, mas é um tanto quanto difícil de obter, depende muito do tipo de privacidade que você está buscando. Neste artigo vamos analisar alguns conceitos sobre privacidade e internet, além de explicar como você pode proteger sua privacidade ao navegar na internet e evitar a vigilância e rastreamento constante."
description: "Analisamos e explicamos como proteger sua privacidade ao navegar na internet e evitar a vigilância e rastreamento constante."
date: 2020-10-08 08:00:00
slug: e-possivel-ter-privacidade-na-internet
authors:
  - alexandrevicenzi
categories:
  - privacidade
tags:
  - internet
  - anonimato
  - google
  - facebook
  - firefox
  - dns
  - vpn
  - tor
images:
  - /images/posts/privacidade-na-internet.jpg
---

{{<figure src="/images/posts/privacidade-na-internet.jpg" width="100%" alt="Privacidade" caption="Photo by [Lianhao Qu](https://unsplash.com/@lianhao) on [Unsplash](https://unsplash.com)">}}

Sim, é possível ter privacidade na internet, mas é um tanto quanto difícil de obter, depende muito do tipo de privacidade que você está buscando. Neste artigo vamos analisar alguns conceitos sobre privacidade e internet, além de explicar como você pode proteger sua privacidade ao navegar na internet e evitar a vigilância e rastreamento constante.

Começando com o básico, a privacidade dos seus dados nas redes sociais, uma coisa é fato, suas fotos só estão na internet e redes sociais porque você, ou algum conhecido seu, as colocou lá. O Google, Facebook, Instagram e demais não possuem uma câmera vigiando pessoas 24h por dia. Em teoria as suas fotos só irão parar nesses serviços se você enviar explicitamente. Existem vários meios de colocar as fotos nesses locais, porém é algo que você tem um grande controle sobre o que postar ou não.

Quando nos referimos a dados pessoais, como CPF e endereço, é um pouco diferente, pois você é geralmente obrigado a inserir esses dados devido a leis ou outras regras, principalmente em serviços onde requer pagamento e/ou entrega de produtos. Neste quesito é torcer para que a empresa ou site faça bom uso dos dados, não compartilhe com terceiros, e que não seja hackeada.

A LGPD ([Lei n.º 13.709/2018][L13709]) propões algumas mudanças em como as empresas tratam dados pessoais. A tendência é melhorar e ter uma maior segurança, assim como aconteceu na Europa com a [GDPR][gdpr], mas ainda assim, não excluir a obrigatoriedade de informação desses dados.

O que você não possui um certo, ou nenhum, controle sobre na internet, é o rastreamento que acontece. No mundo atual, tudo é rastreado, todo clique que você faz em uma página, todo site visitado, todo like e dislike nas redes sociais, toda compra no cartão de crédito, tudo pode ser, ou é, monitorado, por diversas empresas, serviços e até o governo. Esse rastreamento é invasivo, que fere a privacidade das pessoas e gera muitas discussões no mundo todo, e é o que tem feito alguns países adotarem legislações mais rigorosas em relação à privacidade.

O único meio de não ser rastreado é parando de usar a internet, mesmo que você pare de usar redes sociais, elas ainda podem lhe rastrear, pois serviços como Google e Facebook oferecem métodos de rastreios diferenciados. Isso pode parecer um tanto utópico ou paranoico, como viver sem usar a internet? A resposta é, não é possível, dependemos da internet para muitas coisas no nosso dia a dia. Existem meios de diminuir o rastreamento, ou de enganar quem está lhe rastreando com alguns passos simples que podem ser feitos por qualquer pessoa e que vamos abordar no decorrer deste artigo.

O meio mais simples de diminuir o rastreamento (das redes sociais, Google, e demais) é mudar o navegador que você está usando, lembre-se que o Chrome pertence à Google. Minha recomendação para quem busca mais privacidade sem muito esforço é o [Firefox][firefox] ou o [Brave][brave], pois ambos possuem um recurso que bloqueia rastreadores no site que você está visitando. Você pode conferir mais detalhes sobre este recurso do Firefox [aqui][firefox-privacidade]. Se você optar pelo Firefox, a extensão [Facebook Container][facebook-container] permite bloquear ainda mais o rastreamento do Facebook.

Outra opção para reduzir o rastreamento, mas agora do seu [ISP][isp] (seu provedor de internet), é mudar o seu [DNS][dns] padrão. O DNS é responsável por resolver os nomes de domínios, como [buteco.tech][buteco], para um IP, o que possibilita o acesso ao site de forma fácil. Muitos provedores de internet rastreiam essas resoluções de nomes, e para se livrar deste tipo de rastreamento basta alterar o seu DNS para o [1.1.1.1][cf-dns] do Cloudflare ou [OpenDNS][opendns], que são serviços de DNS que prometem uma maior privacidade. Alterar o DNS é simples, basta ir nas configurações de rede do seu computador ou se preferir, altere diretamente nas configurações do seu roteador, sendo assim, todos os dispositivos da sua rede utilizarão o novo DNS sem configuração extra.

Se você quiser ir além, você pode fazer uso de [DNS over HTTPS][dns-over-https]. Essa é uma funcionalidade recente, de 2018, com objetivo de aumentar a segurança e privacidade, e também mitigar [ataques man-in-the-middle][man-in-the-middle], que utiliza resoluções de nomes encriptadas com HTTPS. O Cloudflare e o OpenDNS suportam DNS over HTTPS, porém o ideal é possuir um roteador com suporte para configurá-lo facilmente. Se você usa [OpenWrt][openwrt] como firmware do seu roteador, talvez você queria conferir [esta documentação][dns-over-https-owrt], demais roteadores basta olhar o manual e ver se essa opção existe e como pode ser configurada.

Algumas marcas e modelos de roteadores possuem backdoors (brechas de segurança propositais) ou falhas de segurança (devido a hardware e software antigo) e utilizar o [OpenWrt][openwrt] pode ser uma saída contra esses problemas. O [OpenWrt][openwrt] é um firmware de código aberto disponível para vários modelos e marcas de roteadores e pode ser facilmente instalado e removido, você pode conferir alguns modelos suportados [nessa lista][openwrt-list].

Por último, mas não menos importante que as demais, você pode fazer o uso de uma [VPN][vpn]. O principal uso de uma VPN hoje em dia é para burlar restrições geográficas, pois você pode fingir estar em outro país, acessar a Netflx US estando no Brasil, por exemplo.

VPN é muito mais que burlar restrições, certas VPNs garantem um acesso privado, quase anônimo, o que ajuda a dificultar o rastreamento e descobrir o local de origem. Outro ponto a mencionar é que, VPNs confiáveis, irão criptografar todo o tráfego que passar pela VPN, dificultando ou impossibilitando o acesso aos dados por pessoas não autorizadas ([ataques man-in-the-middle][man-in-the-middle], por exemplo). Em VPNs confiáveis, você tem uma chave de acesso única, só quem possuir essa chave de acesso conseguirá descriptografar o conteúdo que estiver sendo trafegado.

Na questão VPN, cabe a você fazer o dever de casa e identificar qual a mais segura. As dicas que posso dar é, jamais use uma VPN gratuita e evite VPNs em certas jurisdições, como, por exemplo, os Estados Unidos ou países aliados. Tais países possuem leis relacionadas a privacidade que podem fazer com que o serviço de VPN entregue seus dados pessoais ao governo. Existe um comparativo muito bom criado pelo *That One Privacy Guy* que pode ser encontrado [aqui][vpns-comp].

Ainda no quesito VPN, talvez você possa utilizar o [Tor][tor]. O Tor não é uma VPN, mas sim uma rede anônima. Na rede Tor, o seu tráfego é redirecionado para vários servidores pelo mundo, chamado de [overlay network][o-n], como ocorrem muitos redirecionamentos, fica quase impossível rastrear o servidor de origem. Uma desvantagem das redes Tor é justamente o fato desse redirecionamento frequente, o que acaba causando latência e atrasos. O Tor, para mim, é a última das opções, porém, em alguns países o Tor pode significar a única opção.

Sim, parece loucura, mas algumas das dicas dadas são fáceis e que você pode começar a usar hoje mesmo. Cabe a você decidir do que você deseja abrir mão em troca de comodidade. Eu uso Firefox e DNS 1.1.1.1 do dia a dia, e quando preciso uso VPN. Eu nem mencionei as assistentes virtuais como Alexa, Siri, Cortana ou Google Assistent, mas você pode ligar os pontos e decidir se vale ou não a pena usar esses serviços, afinal, alguns deles estão lhe escutando 24/7/365.

Sua privacidade importa, mesmo que você não tenha nada a esconder. Muitas empresas utilizam suas informações para fazer dinheiro e isto não é certo. Até as leis mudarem em favor dos consumidores, todo meio de fortificar a nossa privacidade e diminuir o rastreamento é bem-vindo.

Você se preocupa com sua privacidade? Quais ferramentas você utiliza para se proteger? Deixe um comentário.

[L13709]: http://www.planalto.gov.br/ccivil_03/_ato2015-2018/2018/lei/L13709.htm
[gdpr]: https://pt.wikipedia.org/wiki/Regulamento_Geral_sobre_a_Prote%C3%A7%C3%A3o_de_Dados
[isp]: https://pt.wikipedia.org/wiki/Fornecedor_de_acesso_%C3%A0_internet
[dns]: https://pt.wikipedia.org/wiki/Sistema_de_Nomes_de_Dom%C3%ADnio
[cf-dns]: https://1.1.1.1/dns/
[opendns]: https://www.opendns.com/
[tor]: https://www.torproject.org/pt-BR/
[o-n]: https://pt.wikipedia.org/wiki/Rede_sobreposta
[vpns-comp]: https://thatoneprivacysite.net/#detailed-vpn-comparison
[vpn]: https://pt.wikipedia.org/wiki/Rede_privada_virtual
[openwrt]: https://openwrt.org/start?id=pt-br/start
[openwrt-list]: https://openwrt.org/toh/start
[dns-over-https-owrt]: https://openwrt.org/docs/guide-user/services/dns/doh_dnsmasq_https-dns-proxy
[dns-over-https]: https://pt.wikipedia.org/wiki/DNS_sobre_HTTPS
[man-in-the-middle]: https://pt.wikipedia.org/wiki/Ataque_man-in-the-middle
[brave]: https://brave.com/
[firefox]: https://www.mozilla.org/pt-BR/firefox/new/
[firefox-privacidade]: https://support.mozilla.org/pt-BR/kb/protecao-aprimorada-contra-rastreamento-firefox-desktop
[facebook-container]: https://addons.mozilla.org/pt-BR/firefox/addon/facebook-container/
[buteco]: https://buteco.tech
