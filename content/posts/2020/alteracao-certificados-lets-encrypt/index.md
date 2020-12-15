---
title: "Let's Encrypt usará novos certificados em 2021"
summary: "A partir de 1 de janeiro de 2021 o Let's Encrypt usará novos certificados, entenda o que muda para você."
description: "A partir de 1 de janeiro de 2021 o Let's Encrypt usará novos certificados, entenda o que muda para você."
tagline: "Entenda o que muda para você"
date: 2020-12-17 08:00:00
slug: alteracao-certificados-lets-encrypt
authors:
  - alexandrevicenzi
categories:
  - noticias
tags:
  - lets-encrypt
  - seguranca
  - privacidade
  - tls
  - ssl
resources:
  - name: thumbnail
    src: le-logo.png
  - name: thumbnail-wide
    src: le-logo-wide.png
  - name: hierarchy-pre-sept-2020
    src: hierarchy-pre-sept-2020.png
  - name: hierarchy-post-sept-2020
    src: hierarchy-post-sept-2020.png
---

{{< figure src="thumbnail-wide" width="600" >}}

Nos últimos cinco anos, o [Let's Encrypt][LE] disponibilizou certificados SSL/TLS válidos de forma gratuita, emitindo mais de [um bilhão de certificados][1-bi] e assegurando mais de 200 milhões de sites.

A partir de 1 de janeiro de 2021 será usado um novo certificado *root* para emitir os certificados SSL/TLS dos sites e como esses são renovados a cada 90 dias, essa alteração será propagada rapidamente.

Na prática, não muda muita coisa para quem utiliza Let's Encrypt, o que muda é que os certificados a partir de janeiro serão gerados com um novo *root*.

O problema principal é para as pessoas que acessa sites com certificados emitido pelo Let's Encrypt, com a alteração, sistemas que não foram atualizados desde 2016 podem não reconhecer o novo certificado *root* e considerá-lo inválido.

Celulares com Android 7.1.1 ou mais antigo poderão ter problemas, pois esses não recebem atualizações há anos. Atualmente 66.2% dos usuários usam versões superiores à 7.1 e 33.8% usam versões anteriores. O problema é facilmente corrigido se usando um navegador atualizado, como o [Firefox Mobile][firefox].

## Detalhes técnicos

O Let's Encrypt atualmente usa certificados *top-level* 4096 bit RSA. O ISRG Root X1 é válido até 2035. Durante os anos foram emitidos certificados intermediários, X1 e X2 em 2015 e X3 e X4 em 2016, todos com validade de 5 anos. O LE Authority X3 é usado para emitir os certificados dos sites e esse irá expirar em setembro de 2021.

{{< figure src="hierarchy-pre-sept-2020" caption="Hierarquia até antes de setembro de 2020" width="800" >}}

Em setembro foi [criado o ISRG Root X2][new-cert], usando ECDSA. A maior vantangem de ECDSA é que ele transmite menos dados durante o *handshake* SSL/TLS por possuir uma chave menor, mas garantindo a mesma segurança que as anteriores.

{{< figure src="hierarchy-post-sept-2020" caption="Hierarquia a partir de setembro de 2020" width="800" >}}

O ISRG Root X2 é *cross-signed*, isso quer dizer que o ele foi assinado com múltiplas chaves, uma delas sendo a mesma ISRG Root X1. Como muitos dos dispositivos já possuem o X1 em sua base de certificados válidos, eles continuarão a reconhecer os novos certificados emitidos pelo *root* X2.

O ISRG Root X1 foi *cross-signed* com o IdenTrust DST Root CA X3, um certificado *root* controlado por outra emissora, e isso permitiu que o X1 fosse usado desde o início, pois a grande maioria dos sistemas e navegadores já reconhecia o DST X3.

**Referências**

- [Let's Encrypt's New Root and Intermediate Certificates][new-cert]
- [Standing on Our Own Two Feet][own-feet]

[LE]: https://letsencrypt.org/
[1-bi]: https://letsencrypt.org/2020/02/27/one-billion-certs.html
[new-cert]: https://letsencrypt.org/2020/09/17/new-root-and-intermediates.html
[own-feet]: https://letsencrypt.org/2020/11/06/own-two-feet.html
[firefox]: https://www.mozilla.org/en-US/firefox/mobile/
