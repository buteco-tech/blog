---
title: Criando gráficos com matplot
date: 2015-10-07 23:40:15
authors:
  - willianbriotto
slug: criando-graficos-com-matplot
images:
  - /images/wp-content/uploads/2015/10/logoMatPlotLib.png
categories:
  - data-science
  - tutoriais
tags:
  - matplot
  - python
  - scipy
  - numpy
---

{{<figure src="/images/wp-content/uploads/2015/10/logoMatPlotLib.png">}}

Criado por [John Hunter (1968-2012)](https://en.wikipedia.org/wiki/John_D._Hunter), [matplot](https://matplotlib.org/) é usada para criação de *plots*, histogramas, espectrogramas, gráficos (*charts*) simples e 3D e etc. Para ter uma ideia da diversidade dessa ferramenta, você pode visitar a [galeria de exemplos](https://matplotlib.org/gallery/index.html).

Neste artigo, vou fazer uma demonstração bem simples, onde apresentarei como criar um gráfico de barras (*barchart*) em Python.
<!--more-->
Primeiramente, você deve fazer a instalação de algumas bibliotecas.

```bash
pip3 install matplotlib scipy numpy
```

Estando agora com as bibliotecas necessárias instaladas, podemos dar partida ao nosso código de exemplo:

```python
import numpy as np
import matplotlib.pyplot as plt

data = ((30, 40, 80), ('r', 'g', '#00FF33'), (2013, 2014, 2015))
xPositions = np.arange(len(data[0]))
barWidth = 0.50  # Largura da barra

_ax = plt.axes()  # Cria axes

# bar(left, height, width=0.8, bottom=None, hold=None, **kwargs)
_chartBars = plt.bar(xPositions, data[0], barWidth, color=data[1],
                     yerr=5, align='center')  # Gera barras

for bars in _chartBars:
    # text(x, y, s, fontdict=None, withdash=False, **kwargs)
    _ax.text(bars.get_x() + (bars.get_width() / 2.0), bars.get_height() + 5,
             bars.get_height(), ha='center')  # Label acima das barras

_ax.set_xticks(xPositions)
_ax.set_xticklabels(data[2])

plt.xlabel('Years')
plt.ylabel('Rate')
plt.grid(True)
plt.legend(_chartBars, data[2])

plt.show()
```

Agora vamos a explicação. Primeiramente devemos fazer o *import* do* numpy *e* matplot.*

```python
import numpy as np
import matplotlib.pyplot as plt
```

Depois disso, devemos declarar algumas variáveis que utilizaremos durante o *script*.

```python
data = ((30, 40, 80), ('r', 'g', '#00FF33'), (2013, 2014, 2015))
xPositions = np.arange(len(data[0]))
barWidth = 0.50
```

O data é uma matriz que contém os dados para nosso gráfico de barras, veja significado de cada posição: data[0] – valores usados para definir a altura de cada barra; data[1] – será usado para as cores das barras. Observe a variação de formatos de cores declaradas. data[2] – para os nomeadores de referência para as barras (*labels*) e legenda.

Agora, devemos criar os objetos necessários para manipulação do gráfico e também de nossos *Axes*:

```python
_ax = plt.axes()

_chartBars = plt.bar(xPositions, data[0], barWidth, color=data[1], yerr=5, align='center')  # Gera barras
```

Para melhorarmos os aspecto visual, podemos colocar os valores de cada barra, acima delas, fazendo da seguinte forma:

```python
for bars in _chartBars:
    _ax.text(bars.get_x() + (bars.get_width() / 2.0), bars.get_height() + 5, bars.get_height(), ha='center')  # Label acima das barras
```

Com o código abaixo pode-se adicionar também informações de ano referente ao valor, abaixo das barras:

```python
_ax.set_xticks(xPositions)
_ax.set_xticklabels(data[2])
```

O código abaixo adiciona labels nos eixos x e y e também uma grid como brackground, para facilitar a “divisão visual” dos itens:

```python
plt.xlabel('Years')
plt.ylabel('Rate')
plt.grid(True)
```

Por fim, para termos um barchart com qualidade intuitiva bacana, adcione um legend e então teremos um gráfico bem autoexplicativo:

```python
plt.legend(_chartBars, data[2])
```

No final de tudo, somente renderizamos o gráfico:

```python
plt.show()
```

Segue abaixo uma imagem do gráfico gerado script mostrado acima:

{{<figure src="/images/wp-content/uploads/2015/10/Screenshot-from-2015-10-07-23-31-36.png">}}

Espero que tenham gostado deste exemplo, até a próxima.
