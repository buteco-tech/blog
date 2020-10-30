---
title: Primeiros passos com o ESP8266
summary: |
  Para a brincadeira de hoje, iremos necessitar de pelo menos um ESP8266, fique a vontade na escolha do seu módulo, eu irei utilizar o ESP-01 e o ESP-12. Além disso será necessário um módulo UART de 3.3v, um computador com uma porta USB disponível e algum programa para conversar serialmente, no meu caso irei utilizar a própria IDE do Arduino.
  O ESP permite que você conecte ele de duas formas, a primeira e mais comum é o modo de programação, onde é possível fazer upload de arquivos ou enviar comandos, tudo via serial. A outra forma é a de atualização de firmware (ou Flash). Esta por sua vez permite que você sobrescreva o firmware que vem no módulo. Dependendo o fabricante, as vezes nem é disponibilizado um firmware inicial, e você deve fazer o upload antes de iniciar o uso, mas isto é assunto para o próximo artigo.
date: 2015-04-30 08:53:10
authors:
  - alexandrevicenzi
slug: primeiros-passos-com-o-esp8266
images:
  - /images/wp-content/uploads/2015/03/esp8266.jpg
categories:
  - embarcados
  - tutoriais
tags:
  - diy
  - arduino
  - esp-01
  - esp-12
  - esp8266
  - iot
---


{{< figure src="/images/wp-content/uploads/2015/03/esp8266.jpg" alt="ESP8266" width="250" >}}

Dando continuidade ao artigo [sobre o ESP8266](/conheca-o-esp8266-um-modulo-wifi-por-menos-de-5-dolares), hoje irei mostrar os primeiros passos com este módulo. O primeiro passo e o mais importante é colocar ele pra funcionar. :D

## Material necessário

Para a brincadeira de hoje, iremos necessitar de pelo menos um ESP8266, fique a vontade na escolha do seu módulo, eu irei utilizar o ESP-01 e o ESP-12. Além disso será necessário um módulo UART de 3.3v, um computador com uma porta USB disponível e algum programa para conversar serialmente, no meu caso irei utilizar a própria IDE do Arduino.

## ESP-01 e ESP-12

Abaixo você pode conferir os meus ESPs montados.

{{< figure src="/images/wp-content/uploads/2015/04/esp-12-uart.jpg" alt="ESP-12" height="400" >}}

{{< figure src="/images/wp-content/uploads/2015/04/esp-01-uart.jpg" alt="ESP-01" width="400" >}}

## Módulo UART

No meu caso o módulo usa o chip CP2102 e possui alimentação de 5 e 3.3v.

{{< figure src="/images/wp-content/uploads/2015/04/cp2102-usb-ttl-high-speed-stc-download-hdd-flash-line-3.jpg" alt="UART USB TTL CP2102" width="250" >}}

## IDE Arduino

Caso você ainda não possua um aplicativo para comunicar com o ESP, eu sugiro a IDE do Arduino, que pode ser encontrar [aqui](https://www.arduino.cc/en/main/Software).

## Modos de Operação

O ESP permite que você conecte ele de duas formas, a primeira e mais comum é o modo de programação, onde é possível fazer upload de arquivos ou enviar comandos, tudo via serial. A outra forma é a de atualização de firmware (ou Flash). Esta por sua vez permite que você sobrescreva o firmware que vem no módulo. Dependendo o fabricante, as vezes nem é disponibilizado um firmware inicial, e você deve fazer o upload antes de iniciar o uso, mas isto é assunto para o próximo artigo.

Por padrão, nem sempre ocorre, os módulos ESP saem de fabrica com o firmware [AT](https://www.electrodragon.com/w/Category:ESP8266). Este firmware consiste em uma série de comandos básicos para o uso do módulo em si, mas não se compara ao [NodeMCU](https://www.nodemcu.com/index_en.html), um firmware que permite que você programe o dispositivo em LUA, transformando ele em um mini Arduino com WiFi integrado. No caso do AT, é necessário o uso de um outro dispositivo para controlar o ESP, como é o caso dos shields Ethernet do Arduino. Mas com o NodeMCU, o ESP se torna auto gerenciável, bastando apenas fazer upload de um script LUA com o código a ser executado.

Confira abaixo como conectar o módulo UART no ESP para usá-lo nos modos de programação a atualização de firmware.

## Conexão

Lembre-se que estas conexões mudam de acordo com o ESP e o módulo UART escolhido, então não nos comprometemos com os passos a seguir, faça por sua conta e risco. Lembre-se também de usar a voltagem correta.

A conexão básica funciona assim:

| ESP    | UART |
|:------:|:----:|
| RX     | TX   |
| TX     | RX   |
| VCC    | VCC  |
| CH\_PD | VCC  |
| GND    | GND  |

Abaixo você pode observar as conexões específicas, pois além destes pinos, outros são necessários para o funcionamento.

## ESP-01

Abaixo você pode observar os pinos do ESP-01.

{{< figure src="/images/wp-content/uploads/2015/04/Esp8266pinout1.png" alt="ESP-01 Pins" width="400" >}}

## Programação

Não necessita pinos adicionais.

## Flash

Para entrar no modo de atualização de firmware, você deve conectar o GND ao GPIO0. Confira imagem abaixo:

{{< figure src="/images/wp-content/uploads/2015/04/esp01-uart-flash.png" alt="ESP-01 UART Flash" width="400" >}}

## ESP-12

Abaixo você pode observar os pinos do ESP-12.

{{< figure src="/images/wp-content/uploads/2015/04/esp_12_pin_map.png" alt="ESP-12 Pins" width="400" >}}

## Programação

No modo de programação ainda é necessário conectar o pino GND ao GPIO15. Confira imagem abaixo:

{{< figure src="/images/wp-content/uploads/2015/04/ESP-12-UART-Prog.png" alt="ESP-12 Pins" height="400" >}}

## Flash

Para entrar no modo de atualização de firmware, você deve conectar o GND ao GPIO0. Confira imagem abaixo:

{{< figure src="/images/wp-content/uploads/2015/04/ESP-12-UART-Flash.png" alt="ESP-12 Pins to Flash" height="400" >}}

## Comunicação Serial

Agora que já temos o nosso ESP pronto para ser ligado ao PC, vamos ao que interessa, enviar comandos básicos com o firmware AT.

Após abrir a IDE do Arduino vá em *Tools > Port* e escolha a porta USB onde está o UART. Depois disto, vá em *Tools > Serial Monitor*. Você deverá ver uma tela como esta:

{{< figure src="/images/wp-content/uploads/2015/04/serial-monitor.png" alt="Arduino Serial Monitor" >}}

Agora vamos configurar a comunicação. Por padrão a velocidade é *9200 baud* e o fim de linha é *Both NL &amp; CR*, como mostrado acima, mas caso você tenha problemas, tente mudar estes valores.

Depois de configurar vamos aos comandos básicos:

| Comando      | Valores                                | Descrição                      | Exemplo                         |
|--------------|----------------------------------------|--------------------------------|---------------------------------|
| AT           | N/A                                    | Verifica se o ESP está OK      | AT                              |
| AT+RST       | N/A                                    | Reinicia o ESP                 | AT+RST                          |
| AT+CWMODE    | 1 = Station<br>2 = AP<br>3 = Ambos     | Define o modo de operação WiFI | AT+CWMODE=3                     |
| AT+CWLAP     | N/A                                    | Lista as redes disponíveis     | AT+CWLAP                        |
| AT+CWJAP     | SSID<br>Senha<br>Canal<br>Criptografia | Conecta a uma rede             | AT+CWJAP="seu SSID","sua senha" |
| AT+CWQAP     | N/A                                    | Desconecta da rede             | AT+CWQAP                        |
| AT+CIPSTATUS | N/A                                    | Retorna o status do WiFi       | AT+CIPSTATUS                    |
| AT+CWJAP?    | N/A                                    | Verifica qual rede conectada   | AT+CWJAP?                       |
| AT+CIFSR     | N/A                                    | Retorna o IP do ESP na rede    | AT+CIFSR                        |

O comando mais básico a ser executado é o *AT*. Ao executar um comando você tem duas possíveis respostas:

* *OK* - O seu comando foi executado com sucesso
* *ERROR* - Erro no comando. Ou ele não existe, ou está com parâmetro errado

Se você executou o AT e não teve nenhum dos dois retornos, talvez você esteja em uma destas situações:

* Você fez a conexão dos pinos errada
* O seu ESP pode estar operando em outra velocidade
* O seu ESP não possui firmware, ou veio com outro que não é o AT

Se o seu retorno for algum texto diferente, pode ser outro firmware. Se o texto for caracteres estranhos, provavelmente é a velocidade de comunicação muito elevada.

Espero que este artigo possa ajudar você no mais básico processo de comunicação. Nas próximas publicações mostrarei como alterar o firmware e fazer algumas brincadeiras com LEDs.

Até a próxima.
