---
title: "GPS Parte 1: Entendendo o seu funcionamento"
summary: |
  O GPS é um sistema de navegação global desenvolvido pelo Departamento de Defesa dos Estados Unidos da América. O seu intuito inicial era ser o principal sistema de navegação das forças armadas americanas. A concepção do sistema GPS permite que um usuário, em qualquer local da superfície terrestre, ou próxima a ela, tenha à sua disposição, no mínimo, quatro satélites para serem rastreados.
date: 2015-12-28 20:55:51
authors:
  - alexandrevicenzi
slug: gps-parte-1-entendendo-o-seu-funcionamento
images:
  - /images/wp-content/uploads/2015/12/map_pin.png
categories:
  - tutoriais
  - desenvolvimento
tags:
  - geolocalizacao
  - glonass
  - gnss
  - gps
  - nmea
---

{{< figure src="/images/wp-content/uploads/2015/12/map_pin.png" alt="Map" width="200" >}}

Você já parou pra pensar em como um GPS funciona? Bem, se você ainda não pesquisou sobre o assunto, este post abordará um pouco sobre o que é este dispositivo, que está presente em nosso dia a dia.

Essa série de artigos será divida em três partes. A primeira parte abordará os conceitos básicos. A segunda abordará como se interpreta os dados obtidos do GPS. E por fim será criada uma aplicação Web que indica no mapa onde você está.

## GNSS

_Global Navigation Satellite Systems_ (GNSS) é o termo genérico mais comum para sistemas de navegação por satélite que fornece cobertura global de posicionamento em espaço 3D. Atualmente existem dois sistemas GNSS em operação: o _Global Positioning System_ (GPS) e o _Globalnaya Navigazionnaya Sputnikovaya Sistema_ (GLONASS). Além de mais dois em fase de implantação.

## GPS

O GPS é um sistema de navegação global desenvolvido pelo Departamento de Defesa dos Estados Unidos da América. O seu intuito inicial era ser o principal sistema de navegação das forças armadas americanas. A concepção do sistema GPS permite que um usuário, em qualquer local da superfície terrestre, ou próxima a ela, tenha à sua disposição, no mínimo, quatro satélites para serem rastreados.

O GPS funciona utilizando o processo de trilateração, comumente chamado de triangulação. O processo de trilateração determina a posição de um objeto medindo a distância entre outros objetos que já se sabe a posição, no caso, pontos de referência. Este processo permite calcular a posição dos objetos em espaço 2D e 3D.

## Formato NMEA

A US National Marine Electronics Association (NMEA) propôs em 1983 o formato NMEA 0183, originalmente definido como interface dos dispositivos eletrônicos marítimos, acabou se tornando o padrão industrial dos receptores GNSS. Uma sentença NMEA, consiste em uma cadeia de caracteres no formato ASCII 8-bit e contém no máximo 82 caracteres.

Toda sentença inicia com $. Os próximos dois caracteres indicam o _talker_ e os próximos três indicam o tipo da sentença. Os _talkers_ mais comuns são GP, indicando GPS, e GL, indicando GLONASS. Os demais campos são separados por vírgula seguidos de um asterisco e um checksum opcional na maioria das mensagens. Por fim a mensagem termina com _carriage return_ (CR) e _line feed_.

Abaixo é possível observar a sentença GGA, responsável por conter informação de localização e precisão.

`$GPGGA,123519,4807.038,N,01131.000,E,1,08,0.9,545.4,M,46.9,M,,*47`

Para entender melhor, vamos explicar o que é cada parte:

| Parte | Descrição |
|-------------|------------------------------------------------------|
| GP          | Talker (GPS)                                         |
| GGA         | Nome da sentença                                     |
| 123519      | Hora da Fix (12:35:19 UTC)                           |
| 4807.038,N  | Latitude 48 deg 07.038' N                            |
| 01131.000,E | Longitude 11 deg 31.000' E                           |
| 1           | Qualidade da Fix                                     |
| 08          | Número de satélites visíveis                         |
| 0.9         | Posição horizontal                                   |
| 545.4,M     | Altitude, em metros, acima do nível do mar           |
| 46.9,M      | Nível médio do mar                                   |
| (vazio)     | Tempo em segundos desde a última atualização do DGPS |
| (vazio)     | DGPS ID                                              |
| \*47        | Checksum                                             |

Bem, por hoje é só pessoal. No próximo artigo irei abordar como implementamos um decodificador do formato NMEA em Python.

_Este artigo é uma adaptação da monografia BUSTRACKER: Sistema de rastreamento para transporte coletivo de Alexandre Vicenzi e o texto na íntegra pode ser encontrado [aqui](https://bu.furb.br/consulta/portalConsulta/recuperaMfnCompleto.php?menu=rapida&CdMFN=363720)._
