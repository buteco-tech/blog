---
title: Enviando emails com o Python
date: 2014-12-28 06:00:49
authors:
  - alexandrevicenzi
slug: /enviando-emails-com-o-python/
categories:
  - Desenvolvimento
  - Tutoriais
tags:
  - email
  - python
  - smtplib
  - smtp
images:
  - /images/wp-content/uploads/2014/12/python-logo.png
---

{{< figure src="/images/wp-content/uploads/2014/12/python-logo.png" alt="Python" width="150" >}}

Hoje iremos enviar emails com Python. O exemplo mostrado usará o módulo [smtplib](https://docs.python.org/3.8/library/smtplib.html).

Vale ressaltar que o exemplo é indicado para envio de email mais simples, para enviar email com formatações especiais e anexos é recomendado o uso do módulo [email](https://docs.python.org/3.8/library/email.html) que foi abordado no artigo [Formatando e adicionando anexos a e-mails no Python](/formatando-e-adicionando-anexos-a-e-mails-no-python/).

O módulo _smtplib_ define um cliente SMTP que pode ser usado para enviar emails tanto via SMTP quanto ESMTP. A _smtplib_ segue os padrões da RFC 821 (SMTP), RFC 1869 (ESMTP), RFC 2554 (Autenticação SMTP) e RFC 2487 (SMTP Seguro via TLS).

Como este módulo já está incluso nas bibliotecas do Python você não precisará instalar nenhuma biblioteca adicional.

Agora que já demos uma boa introdução sobre o módulo _smtplib_ vamos ao que interessa.

Inicialmente vamos importar o módulo:

```py
import smtplib
```

Vamos criar a instância do SMTP de acordo com a forma de autenticação:

**TLS**

```py
smtp = smtplib.SMTP('localhost', 587)
smtp.starttls()
```

**SSL**

```py
smtp = smtplib.SMTP_SSL('localhost', 465)
```

**Sem autenticação**

```py
smtp = smtplib.SMTP('localhost', 25)
```

Se escolhemos TLS ou SSL devemos fazer a autenticação:

```py
smtp.login('usuário', 'senha')
```

Caso seja sem autenticação devemos nos identificar enviando o comando EHLO ou HELO:

```py
# EHLO
smtp.ehlo()

# HELO
smtp.helo()

# De forma genérica. Tenta EHLO primeiro.
smtp.ehlo_or_helo_if_needed()
```

Não há necessidade de chamar os métodos `ehlo` ou `helo` quando se utiliza SSL ou TLS, pois o método login faz a chamada desses métodos caso seja necessário.

Enviando um email:

```py
msg = '''From: Seu Nome <seuemail@seudominio.com.br>
To: outroemail@seudominio.com.br
Subject: Mensagem do Buteco

Email de teste do Buteco'''

smtp.sendmail('seuemail@seudominio.com.br', ['outroemail@seudominio.com.br'], msg)
```

Note que o segundo parâmetro do método `sendmail` deve ser uma lista. Mesmo que o destinatário seja apenas um.

Finalizando a sessão SMTP:

```py
smtp.quit()
```

Agora que já sabemos quais partes usar, vamos colocar tudo em prática em um exemplo. No exemplo vou utilizar o Gmail como servidor SMTP, mas você pode utilizar outro de sua preferência.

Abaixo você pode verificar como enviar usando TLS:

```py
import smtplib

smtp = smtplib.SMTP('smtp.gmail.com', 587)
smtp.starttls()

smtp.login('seuemail@gmail.com', 'suasenha')

de = 'seuemail@gmail.com'
para = ['seuemail@gmail.com']
msg = """From: %s
To: %s
Subject: Mensagem do Buteco

Email de teste do Buteco.""" % (de, ', '.join(para))

smtp.sendmail(de, para, msg)

smtp.quit()
```

Já neste outro exemplo você pode verificar como enviar via SSL:

```py
import smtplib

smtp = smtplib.SMTP_SSL('smtp.gmail.com', 465)

smtp.login('seuemail@gmail.com', 'suasenha')

de = 'seuemail@gmail.com'
para = ['seuemail@gmail.com']
msg = """From: %s
To: %s
Subject: Mensagem do Buteco

Email de teste do Buteco.""" % (de, ', '.join(para))

smtp.sendmail(de, para, msg)

smtp.quit()
```

Confira abaixo uma lista dos servidores de email mais comuns e suas configurações.

| Nome | Servidor | Autenticação | Porta |
|:--------------|:-------------------:|:------------:|:-----:|
| Gmail         |   smtp.gmail.com    |     SSL      |  465  |
| Gmail         |   smtp.gmail.com    |   StartTLS   |  587  |
| Hotmail       |    smtp.live.com    |     SSL      |  465  |
| Mail.com      |    smtp.mail.com    |     SSL      |  465  |
| Outlook.com   |    smtp.live.com    |   StartTLS   |  587  |
| Office365.com | smtp.office365.com  |   StartTLS   |  587  |
| Yahoo Mail    | smtp.mail.yahoo.com |     SSL      |  465  |

Espero que você tenha gostado desta publicação.

Continue acompanhando que faremos uma continuação falando sobre o módulo _email_.
