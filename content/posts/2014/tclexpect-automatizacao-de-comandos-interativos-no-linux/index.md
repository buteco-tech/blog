---
title: "Tcl/Expect: Automatização de comandos interativos no Linux"
date: 2014-10-07 00:23:49
authors:
  - marcossouza
slug: tclexpect-automatizacao-de-comandos-interativos-no-linux
categories:
  - desenvolvimento
  - tutoriais
tags:
  - linux
  - tcl
  - tcl-expect
  - ruby
  - automacao
---

Neste artigo iremos abordar a automatização de comandos com o *Tcl/expect*.

[Tcl](https://www.tcl.tk/) é, segundo a Wikipédia, uma linguagem de script para rápida prototipação.
Tcl significa *Tool Command Language*, e foi criada no ano de 1988 por John Ousterhout.
Para mais informações sobre Tcl, sua origem ou outros dados, você pode verificar a [Wikipedia](https://en.wikipedia.org/wiki/Tcl).

Seu aprendizado é bem simples, sendo que, ao meu ver, é uma linguagem parecida com Bash e Python.
Uma exemplo de Hello World com ela pode ser feito com:

```ruby
puts "Hello World!"
```

Para executar comandos Tcl, será necessário instalar o interpretador Tcl.
Atualmente todas as maiores distribuições Linux contém o Tcl em seus repositórios.
Existem muitos exemplos de como usar Tcl.
Você pode ver alguns deles no [Wikibooks](https://en.wikibooks.org/wiki/Tcl_Programming/Examples).

[Expect](https://www.tcl.tk/man/expect5.31/expect.1.html) é uma extensão da linguagem Tcl para automatizar interações com programas em modo texto.
Este foi escrito por Don Libes em 1990, e inicialmente foi escrito para ser executado em sistemas UNIX e para poder controlar programas interativos como o ftp, ssh, telnet e vários outros.
Como o Tcl, o interpretador expect também precisa ser instalado. Este também está nos repositórios das distribuições Linux.

Sua facilidade de controlar programas interativos se deve ao fato do Expect poder executar um programa e manter uma espécie de "handler" desse programa.
Este handler se chama `spawn_id`. Com este `spawn_id` o programador consegue enviar comandos e também esperar por padrões de saída do programa.
Como primeiro exemplo será mostrado um telnet, onde interativamente precisamos informar usuário e senha, e neste caso será automatizado pelo Expect:

```tcl
# Assume $remote_server, $my_user_id, $my_password, and $my_command were read in earlier
# in the script.
# Open a telnet session to a remote server, and wait for a username prompt.
spawn telnet $remote_server
expect "username:"
# Send the username, and then wait for a password prompt.
send "$my_user_id\r"
expect "password:"
# Send the password, and then wait for a shell prompt.
send "$my_password\r"
expect "%"
# Send the prebuilt command, and then wait for another shell prompt.
send "$my_command\r"
expect "%"
# Capture the results of the command into a variable. This can be displayed, or written to disk.
set results $expect_out(buffer)
# Exit the telnet session, and wait for a special end-of-file character.
send "exit\r"
expect eof
```

A variável global `timeout` diz respeito a quanto tempo cada chamada do comando Expect vai esperar pelo retorno especificado.
Colocando este como `-1` remove a restrição de tempo do comando `expect`.
Já o comando `send` envia comandos ao processo, como se fosse o próprio usuário digitando no terminal.

Como você pode ver, com esta combinação de comandos um programador pode controlar qualquer programa de terminal por um script *expect*.
Ao buscar no google por exemplos em *Expect*, um dos seus usos mais comuns é para fazer uma conexão SSH.

Um outro comando interessante do *Expect* é o `interact`.
Este comando basicamente entrega ao usuário o programa executado pelo expect.
Um exemplo deste pode ser mostrado com uma conexão SSH, onde temos um usuário pré-configurado e ao fazer o login o prompt do SSH é mostrado ao usuário para que este possa executar comando no host remoto.

```tcl
proc Login {username server password} {
    spawn /usr/bin/ssh $username@$server
    expect {
        -re "Are you sure you want to continue connecting (yes/no)?" {
            exp_send "yes\r"
            exp_continue
            #continue to match statements within this expect {}
        }

        -nocase "password: " {
            exp_send "$password\r"
            interact
        }
    }
}
```

Nó código acima podemos ver uma função de Tcl/Expect para criar uma conexão SSH.
No Tcl os parâmetros não tem tipos, são tratados como strings.
Após enviar a senha para o usuário especificado na conexão, a função executa o comando `interact`.
Após este comando, o usuário pode digitar normalmente no terminal dentro da sessão de SSH remota.

Espero ter abordado o *Tcl/Expect* com a simplicidade que este de fato tem, e ajudado a facilitarem suas vidas com esta linguagem :)

No próximo artigo pretendo escrever sobre *pexpect*, que é a integração do Python com *Expect*, deixando este trabalho ainda mais simples!

Até mais!
