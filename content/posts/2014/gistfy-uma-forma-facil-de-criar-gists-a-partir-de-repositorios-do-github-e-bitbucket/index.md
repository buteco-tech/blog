---
title: Crie Gists a partir de repositórios do GitHub e Bitbucket
tagline: Conheça o Gistfy
date: 2014-09-24 15:17:18
authors:
  - alexandrevicenzi
slug: gistfy-uma-forma-facil-de-criar-gists-a-partir-de-repositorios-do-github-e-bitbucket
categories:
  - ferramentas
tags:
  - gist
  - gistfy
  - github
  - node-js
---

Estes dias tive um problema, eu necessitava adicionar um trecho de código do GitHub a uma página, mas este código estava em um repositório do GitHub e não em um Gist.
Particularmente eu não gosto muito de criar Gists, e como o GitHub não oferece uma forma de criar um Gist a partir de um repositório eu resolvi criar a minha própria ferramenta.

A partir desta necessidade eu criei o [Gistfy](https://gistfy-app.herokuapp.com), um meio de compartilhar código do GitHub e Bitbucket em páginas, como um Gist. A ideia e o projeto são simples, mas cumpre o que promete.

Na documentação você irá encontrar exemplos de [como usar](https://gistfy-app.herokuapp.com/usage.html) e os [parâmetros aceitos](https://gistfy-app.herokuapp.com/api.html).

Como você pode observar no exemplo abaixo o snippet é semelhante a um Gist do GitHub:

<script type="text/javascript" src="https://gistfy-app.herokuapp.com/github/isagalaev/highlight.js/test/detect/python/default.txt?lang=python"></script>

Para usar basta adicionar esta linha a sua página (substitua os dados pelos seus):

```html
<script type="text/javascript"
        src="https://gistfy-app.herokuapp.com/github/isagalaev/highlight.js/test/detect/python/default.txt?lang=python"></script>
```

Você pode criar o seu próprio servidor no Heroku, confira o [repositório do projeto](https://github.com/alexandrevicenzi/gistfy).

Não esqueça de deixar seu comentário.
