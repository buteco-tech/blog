---
title: Formatando e adicionando anexos a e-mails no Python
date: 2015-01-15 06:00:19
authors:
  - alexandrevicenzi
slug: formatando-e-adicionando-anexos-a-e-mails-no-python
images:
  - /images/wp-content/uploads/2014/12/python-logo.png
categories:
  - Desenvolvimento
  - Tutoriais
tags:
  - email
  - python
  - smtplib
  - smtp
  - mime
---

{{< figure src="/images/wp-content/uploads/2014/12/python-logo.png" alt="Python" width="150" >}}

Há algumas semanas, nós mostramos [como enviar e-mail pelo Python](/enviando-emails-com-o-python) usando o módulo [smtplib](https://docs.python.org/3.8/library/smtplib.html). Hoje iremos dar continuidade ao envio de e-mail, desta vez focaremos em sua formatação e adição de arquivos anexos.

Para fazer este serviço iremos utilizar o módulo [email](https://docs.python.org/3.8/library/email.html). Este módulo é uma biblioteca para manipulação de mensagens de email e outros documentos do tipo MIME. Como este módulo já está incluso nas bibliotecas do Python você não precisará instalar nenhuma biblioteca adicional.

## Submódulos

Antes de iniciar, vamos ver os submódulos do módulo email que iremos utilizar:

`from email import encoders`

Útil para quando criamos um payload que não seja um já implementado. Neste caso necessitamos codificar a mensagem.

`from email.mime.base import MIMEBase`

Classe base para quando desejamos enviar um arquivo não suportado pelas classes já disponíveis. Um exemplo é o envio de aquivos de vídeos.

`from email.mime.multipart import MIMEMultipart`

Usado para criar uma mensagem MIME multpart.

`from email.mime.audio import MIMEAudio`

Usado para criar objetos MIME para a maior parte de aquivos de áudio.

`from email.mime.image import MIMEImage`

Usado para criar objetos MIME para a maior parte de aquivos de imagem.

`from email.mime.text import MIMEText`

Usado para criar objetos MIME para a maior parte de aquivos de texto.

## Usando os submódulos

Agora vamos verificar como criar uma mensagem com cada um desses tipos:

### MIMEBase

```py
with open('arquivo.zip', 'rb') as f:
  mime = MIMEBase('application', 'zip')
  mime.set_payload(f.read())

encoders.encode_base64(mime)
```

### MIMEMultipart

```py
msg = MIMEMultipart()
msg.attach(arquivo_mime_1)
msg.attach(arquivo_mime_2)
```

arquivo_mime deve ser outro subtipo MIME, por exemplo MIMEAudio.

### MIMEAudio

```py
with open('audio.ogg', 'rb') as f:
  mime = MIMEAudio(f.read(), _subtype='ogg')
```

### MIMEImage

```py
with open('imagem.png', 'rb') as f:
  mime = MIMEImage(f.read(), _subtype='png')
```

### MIMEText

```py
with open('pagina.html') as f:
  mime = MIMEText(f.read(), _subtype='html')
```

Adicionando os dados de envio e recebimento no cabeçalho da mensagem:

```py
msg = MIMEText('Exemplo.', 'plain')
msg['From'] = 'seuemail@seudominio.com'
msg['To'] = ', '.join(['outroemail@seudominio.com'])
msg['Cc'] = ', '.join(['emailemcopia@seudominio.com'])
msg['Bcc'] = ', '.join(['emailocultol@seudominio.com'])
msg['Reply-To'] = ', '.join(['seuemail@seudominio.com'])
msg['Subject'] = 'Assunto'
```

* O *From* indica quem está enviando mensagem.
* O *To* indica quem irá receber.
* O *Cc* indica quem receberá uma cópia deste email.
* O *Bcc* também indica quem receberá uma cópia do email, mas as demais pessoas não irão ver o email dele na lista dos enviados.
* O *Reply-To* indica para quem será respondido o email.
* O *Subject* é o assunto do email.

Para enviar os objetos MIME via SMTP devemos gerar as mensagens no formato _raw_, para isto utilizamos o método _as_string_.

## Exemplos

Agora que já possuímos uma noção básica do funcionamento do módulo email, vamos criar um exemplo completo utilizando tudo o que foi visto.

O exemplo abaixo exemplifica o envio de um email HTML:

```py
import smtplib
from email.mime.text import MIMEText

de = 'seumail@gmail.com'
para = ['outroemail@gmail.com']

msg = MIMEText('Exemplo de email HTML do &lt;b&gt;Buteco Open Source&lt;b/&gt;.', 'html', 'utf-8')
msg['From'] = de
msg['To'] = ', '.join(para)
msg['Subject'] = 'Buteco Open Source'

raw = msg.as_string()

smtp = smtplib.SMTP_SSL('smtp.gmail.com', 465)
smtp.login('seumail@gmail.com', 'suasenha')
smtp.sendmail(de, para, raw)
smtp.quit()
```

O exemplo abaixo exemplifica o envio de um email HTML com anexo:

```py
import mimetypes
import os
import smtplib

from email import encoders
from email.mime.audio import MIMEAudio
from email.mime.base import MIMEBase
from email.mime.image import MIMEImage
from email.mime.multipart import MIMEMultipart
from email.mime.text import MIMEText

def adiciona_anexo(msg, filename):
    if not os.path.isfile(filename):
        return

    ctype, encoding = mimetypes.guess_type(filename)

    if ctype is None or encoding is not None:
        ctype = 'application/octet-stream'

    maintype, subtype = ctype.split('/', 1)

    if maintype == 'text':
        with open(filename) as f:
            mime = MIMEText(f.read(), _subtype=subtype)
    elif maintype == 'image':
        with open(filename, 'rb') as f:
            mime = MIMEImage(f.read(), _subtype=subtype)
    elif maintype == 'audio':
        with open(filename, 'rb') as f:
            mime = MIMEAudio(f.read(), _subtype=subtype)
    else:
        with open(filename, 'rb') as f:
            mime = MIMEBase(maintype, subtype)
            mime.set_payload(f.read())

        encoders.encode_base64(mime)

    mime.add_header('Content-Disposition', 'attachment', filename=filename)
    msg.attach(mime)

de = 'seumail@gmail.com'
para = ['outroemail@gmail.com']

msg = MIMEMultipart()
msg['From'] = de
msg['To'] = ', '.join(para)
msg['Subject'] = 'Buteco Open Source'

# Corpo da mensagem
msg.attach(MIMEText('Exemplo de email HTML com anexo do <b>Buteco</b>.', 'html', 'utf-8'))

# Arquivos anexos.
adiciona_anexo(msg, 'texto.txt')
adiciona_anexo(msg, 'imagem.jpg')

raw = msg.as_string()

smtp = smtplib.SMTP_SSL('smtp.gmail.com', 465)
smtp.login('seumail@gmail.com', 'suasenha')
smtp.sendmail(de, para, raw)
smtp.quit()
```

Comparando este tutoria ao anterior, a montagem e o envio de um email ficam bem mais simplificados com esta biblioteca para manipulação de email, não é?

Espero que você tenha gostado. Não esqueça de assinar nosso feed.

_Parte deste código foi obtido na [documentação do módulo email](https://docs.python.org/3.8/library/email.examples.html)._
