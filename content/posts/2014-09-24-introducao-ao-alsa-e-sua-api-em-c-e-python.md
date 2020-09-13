---
title: Introdução ao ALSA e sua API em C e Python
date: 2014-09-24 23:48:00
authors:
  - alexandrevicenzi
  - marcossouza
slug: introducao-ao-alsa-e-sua-api-em-c-e-python
categories:
  - desenvolvimento
tags:
  - alsa
  - c
  - cpp
  - python
  - linux
images:
  - /images/posts/alsa-mixer.png
---

O [ALSA][alsa] (Advanced Linux Sound Architeture) é um framework para *kernel space* e *user space* para manipular dispositivos de áudio.
No *kernel space* o [ALSA][alsa] é a API padrão para a criação dos *devices drivers* de dispositivos de áudio, e no *user space* o [ALSA][alsa] existe como uma API para permitir a configuração dos dispositivos de áudio que tenham seu *device driver* implementado com o framework [ALSA][alsa].
Um exemplo de um programa que utilize a API *user space* do [ALSA][alsa] é o *alsamixer*.

Abaixo uma imagem do `alsamixer` executando em uma máquina desktop:

{{<figure src="/images/posts/alsa-mixer.png" alt="Alsa Mixed">}}

Na imagem podemos ver algumas configurações da placa de áudio e podemos interagir com esta.

Para os controles básicos de áudio, tal como nível de volume e configuração de microfone, é necessário utilizar a [API mixer-controls][alsa-simple-mixer], a documentação completapode ser encontrada [aqui][alsa-lib-doc].

Para ilustrar o funcionamento básico do [ALSA][alsa] seguem dois exemplos de códigos que permitem alterar o volume de de áudio de uma máquina com Linux.
Um dos exemplos será na linguagem C e o outro será em Python.
O primeiro exemplo é na linguagem C, que basicamente diz quais os valores limite do volume da placa de áudio e também permite ao usuário trocar o nível do volume.

```c
#include <stdio.h>
#include <alsa/asoundlib.h>

int main(int argc, char* argv[])
{
    long vol_min = 0, vol_max = 0, vol = 0;
    snd_mixer_t *handle;
    snd_mixer_elem_t *elem;
    snd_mixer_selem_id_t *sid;
    snd_mixer_selem_channel_id_t channel = SND_MIXER_SCHN_FRONT_LEFT;

    snd_mixer_open(&handle, 0);
    snd_mixer_attach(handle, "default");
    snd_mixer_selem_register(handle, NULL, NULL);
    snd_mixer_load(handle);

    snd_mixer_selem_id_alloca(&sid);
    snd_mixer_selem_id_set_index(sid, 0);

    // pegar volume minimo e máximo
    // Master aqui se refere ao volume de saida
    // geralmente esse é o nome da configuração de volume da máquina
    snd_mixer_selem_id_set_name(sid, "Master");
    elem = snd_mixer_find_selem(handle, sid);

    snd_mixer_selem_get_playback_volume_range(elem, &vol_min, &vol_max);

    printf("Volume mínimo: %ld e máximo: %ld\n", vol_min, vol_max);

    if (argc == 1) {
        fprintf(stderr, "Voce precisa passar um valor %ld e %ld como volume\n", vol_min, vol_max);
        return 1;
    }

    snd_mixer_selem_set_playback_volume_all(elem, atol(argv[1]));

    snd_mixer_selem_get_playback_volume(elem, channel, &vol);
    printf("Volume atual: %ld\n", vol);

    return 0;
}
```

Para compilar este programar basta executar:

```bash
g++ volume.c -o volume -lasound
```

Onde o arquivo salvo tem o nome `volume.c`.

Executando em minha máquina, tenho a seguinte saída:

```bash
[marcos@localhost ~]$ ./volume 40000
Volume mínimo: 0 e máximo: 65536
Volume atual: 40000
```

Abaixo temos um exemplo da utilização do [ALSA][alsa] em Python.
Será necessário instalar a biblioteca [pyalsaaudio][pyalsaaudio].

```python
import alsaaudio

print("ALSA Audio")
print("----------")
print("Placas disponíveis:")

# Lista as placas disponíveis
for x in alsaaudio.cards():
    print(x)

print("Controles disponíveis:")

# Lista os controles disponíveis
for x in alsaaudio.mixers():
    print(x)

# Seleciona a placa de som com ID 0 - Master
mixer = alsaaudio.Mixer(control="Master", id=0, cardindex=0)

# Lista algumas informações do controle Master
print("O volume de playback do controle Master é: %s" % mixer.getvolume(alsaaudio.PCM_PLAYBACK))
print("O volume do captura controle Master é: %s" % mixer.getvolume(alsaaudio.PCM_CAPTURE))
print("Controle Master está mudo." if mixer.getmute() == 1 else "Controle Master não está mudo.")

mixer.setvolume(100)

print("O volumedo do controle Master foi alterado para 100.")

mixer = alsaaudio.Mixer(control="Mic", id=0, cardindex=0)

# Lista algumas informações do controle Mic
print("O volume de playback do controle Mic é: %s" % mixer.getvolume(alsaaudio.PCM_PLAYBACK))
print("O volume do captura controle Mic é: %s" % mixer.getvolume(alsaaudio.PCM_CAPTURE))
print("Controle Mic está mudo." if mixer.getmute() == 1 else "Controle Mic não está mudo.")

mixer.setvolume(100)

print("O volumedo do controle Mic foi alterado para 100.")

# Seleciona a placa de som com ID 0 - Captura
mixer = alsaaudio.Mixer(control="Capture", id=0, cardindex=0)

mixer.setrec(False)
print("Captura desabilitada no controle Capture")

mixer.setrec(True)
print("Captura habilitada no controle Capture")
```

No exemplo em Python, podemos observar alguns usos do framework [ALSA][alsa], como por exemplo, pegar e setar configurações de placas e níveis de áudio.

Se você tiver algum problema na execução desses códigos, ou tem alguma dúvida, deixe um comentário.

[alsa]: https://www.alsa-project.org/wiki/Main_Page
[alsa-lib-doc]: https://www.alsa-project.org/alsa-doc/alsa-lib/
[alsa-simple-mixer]: https://www.alsa-project.org/alsa-doc/alsa-lib/group___simple_mixer.html
[pyalsaaudio]: http://larsimmisch.github.io/pyalsaaudio/
