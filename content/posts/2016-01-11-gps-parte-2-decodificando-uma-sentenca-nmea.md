---
title: "GPS Parte 2: Decodificando uma sentença NMEA"
date: 2016-01-11 21:38:15
authors:
  - alexandrevicenzi
slug: gps-parte-2-decodificando-uma-sentenca-nmea
images:
  - /images/wp-content/uploads/2015/12/map_pin.png
categories:
  - Tutoriais
  - Desenvolvimento
tags:
  - geolocalização
  - gps
  - nmea
  - python
---

{{< figure src="/images/wp-content/uploads/2015/12/map_pin.png" alt="Map" width="200" >}}

Na [primeira parte sobre GPS (Entendendo o seu funcionamento)](/gps-parte-1-entendendo-o-seu-funcionamento) vimos como este sistema de rastreamento por satélite funciona. Também aproveitamos para explicar o que é o protocolo NMEA, utilizado pelos sistemas de navegação global.

Uma sentença NMEA é formada por caracteres passíveis de impressão e **CR** (carriage return) e **LF** (line feed). Toda sentença inicia com `$` e termina com &lt;CR&gt; &lt;LF&gt;. Existem três tipos básicos de sentenças: _talker sentences_, _proprietary sentences_ e _query sentences_.

As _talker sentences_ são as sentenças genéricas de comunicação do protocolo, já as _proprietary sentences_ são sentenças proprietárias dos fabricantes e as _query sentences_ são sentenças utilizadas para requisitar informações a partir de um receptor.

Neste artigo iremos ver como implementar um decodificar de sentenças do protocolo NMEA 0183 versão 2.3.

Para implementar o decodificador iremos utilizar Python sem nenhuma biblioteca adicional. O código desenvolvido é apenas para ilustrar como é feito este tipo de processo, se você procura algo para utilizar em seu sistema eu recomendo a biblioteca [pynmea2](https://github.com/Knio/pynmea2).

## Decodificando uma sentença NMEA

Para decodificar uma sentença primeiro é necessário entender o seu funcionamento. No caso da sentença **GGA** (Global Positioning System Fix Data) podemos observar a descrição dos campos abaixo:

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

Se você deseja conhecer as demais sentenças e seus campos eu recomento ler a [especificação do protocolo NMEA 0183](http://www.tronico.fi/OH6NT/docs/NMEA0183.pdf) (em inglês).

Bem, para entender melhor, vamos primeiro ver as principais partes do código, no fim será exibido o código por completo com alguns exemplos de uso.

## Classe Sentence

A classe `Sentence` é a base de todas as sentenças NMEA. Toda sentença deve estender esta classe, como podemos observar na classe `GGLSentence`.

```py
class Sentence(object):
    sentence_name = 'Unknown'
    sentence_description = 'Unknown Sentence'
    fields = ()

    def __init__(self):
        self._fields_count = len(self.fields)
        self._last = self._fields_count - 1

    def parse(self, data):
        data = str(data)
        sentence, checksum = data.split('*')
        raw_fields = sentence.split(',')

        if len(raw_fields) != self._fields_count:
            raise ParseException('Field count mismatch. Expected %d fields, but found %d.' % (self._fields_count, len(raw_fields)))

        for index, field in enumerate(self.fields):
            field_name, _, field_type = field
            try:
                value = raw_fields[index]
                value = value.strip()

                if value:
                    setattr(self, field_name, field_type(value))
                else:
                    setattr(self, field_name, None)
            except:
                raise ParseException('Can\'t parse value into field "%s": %s' % (field_name, value))

        return self

    @property
    def is_valid(self):
        return False

    def __repr__(self):
        return '%sSentence(is_valid=%s)' % (self.sentence_name, self.is_valid)
```

Toda sentença deve possuir um nome (`sentence_name`), uma descrição (`sentence_description`) e seus respectivos campos (`fields`). Além disto, as classes devem implementar um método validador (`is_valid`) para verificar se a sentença recebida é valida.

Podemos observar que existe um método para decodificar (`parse`) a sentença. Como as sentenças devem seguir o mesmo padrão, o método é genérico para todas as classes derivadas de `Sentence`.

Inicialmente é extraído da sentença o seu checksum caso exista. Após é verificado se a quantidade de campos recebidos na sentença corresponde a quantidade de campos registrados. Por fim, é convertido o valor do campo recebido para um tipo Python compatível.

Como sabemos para que tipo devemos converter determinado campo? É isso que você verá na classe `GLLSentence`.

## Estendendo a classe Sentence

Na classe `GLLSentence` sobrescrevemos as propriedades necessárias para o funcionamento correto do _parser_.

```py
class GLLSentence(Sentence, LatLonMixin):
    sentence_name = 'GLL'
    sentence_description = 'Geographic Position – Latitude/Longitude'
    fields = (
        ('latitude', 'Latitude', str),
        ('ns_indicator', 'N/S Indicator', str.upper),
        ('longitude', 'Longitude', str),
        ('ew_indicator', 'E/W Indicator', str.upper),
        ('utc_time', 'UTC Time', UTCTimeParser),
        ('status', 'Status', str.upper),
        # ('mode', 'Mode', str), NMEA V 3.00
    )

    @property
    def is_valid(self):
        return self.status == 'A'
```

Como podemos observar, sobrescrevemos os atributos `sentence_name` e `sentence_description` com o nome e a descrição da sentença.

O atributo `fields` foi sobrescrito por uma tupla de tuplas que corresponde ao seguinte: o primeiro valor da tupla é o nome do campo, deve sem um nome de atributo válido, pois ele será atribuído a classe em tempo de execução; o segundo campo é uma descrição para o campo, pode ser qualquer texto; o terceiro campo é uma função/classe que será utilizada para converter o valor. Neste caso deve-se lembrar que a função/classe deve possuir apenas um parâmetro e do tipo `str`. Isto porque a função/classe é invocada com o valor recebido no campo.

Se observarmos, é possível verificar que o campo `latitude` será convertido para `str`, já o campo `ns_indicator` será convertido para `str`, porém maiúscula. O campo `utc_time` usa a classe `UTCTimeParser` para converter para o tipo `datetime.date`.

Por fim, implementamos a validação da sentença. No caso da sentença GLL, ela é valida se o `status` for igual a `A`. Uma validação não implementada é a do checksum. O checksum serve para verificar se o conteúdo recebido foi o mesmo que o enviado. Como o intuito é apenas exemplificar o funcionamento, podemos ignorar este item.

## Classe NMEAParser

Para finalizar, vamos verificar como a classe `NMEAParser` identifica qual a sentença e sua respectiva classe para conversão.

```py
class NMEAParser(object):
    parsers = {
        GGASentence.sentence_name: GGASentence,
        GLLSentence.sentence_name: GLLSentence,
        RMCSentence.sentence_name: RMCSentence,
        VTGSentence.sentence_name: VTGSentence,
    }

    def _get_parser(self, name):
        parser = NMEAParser.parsers.get(name.upper())

        if parser:
            return parser()

        return None

    def parse(self, data):
        if not data:
            raise ParseException('Can\'t parse empty data.')

        if data[0] == u'$':
            data = data[1:]

        if data[0:2] == u'GP':
            data = data[2:]

        sentence = data[0:3]
        parser = self._get_parser(sentence)

        if not parser:
            raise ParseException('Can\'t find parser for sentence: %s' % sentence)

        return parser.parse(data[4:])
```

A classe `NMEAParser` possui um atributo (`parsers`) responsável por armazenar o nome da sentença e a classe de conversão. Para identificar qual a classe correta extraímos no método `parse` o nome da sentença recebida. Se não for possível identificar a sentença ou se recebermos uma sentença não suportada é gerada uma exceção.

Abaixo é possível observar o código completo da aplicação com alguns exemplos de uso.

```py
class NMEAParser(object):
    parsers = {
        GGASentence.sentence_name: GGASentence,
        GLLSentence.sentence_name: GLLSentence,
        RMCSentence.sentence_name: RMCSentence,
        VTGSentence.sentence_name: VTGSentence,
    }

    def _get_parser(self, name):
        parser = NMEAParser.parsers.get(name.upper())

        if parser:
            return parser()

        return None

    def parse(self, data):
        if not data:
            raise ParseException('Can\'t parse empty data.')

        if data[0] == u'$':
            data = data[1:]

        if data[0:2] == u'GP':
            data = data[2:]

        sentence = data[0:3]
        parser = self._get_parser(sentence)

        if not parser:
            raise ParseException('Can\'t find parser for sentence: %s' % sentence)

        return parser.parse(data[4:])
```

Você pode conferir o código fonte completo [aqui](https://github.com/ButecoOpenSource/exemplos/blob/master/nmea/nmea.py).

Espero que você tenha gostado deste artigo. Na parte final desta série de artigos, iremos implementar uma pequena aplicação que captura a posição do GPS e informa em uma página Web.

Até a próxima.

_Este artigo é uma adaptação da monografia BUSTRACKER: Sistema de rastreamento para transporte coletivo de Alexandre Vicenzi e o texto na íntegra pode ser encontrado [aqui](https://bu.furb.br/consulta/portalConsulta/recuperaMfnCompleto.php?menu=rapida&CdMFN=363720)._
