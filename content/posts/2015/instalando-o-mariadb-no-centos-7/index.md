---
title: Instalando o MariaDB no CentOS 7
summary: |
  O MariaDB é um servidor de banco de dados que oferece as mesmas funcionalidades do MySQL. Na verdade ele é um fork do MySQL, feito após a sua compra pela Oracle. O MariaDB é desenvolvido pela comunidade de software livre e por alguns dos autores originais do MySQL.
date: 2015-02-11 19:30:29
authors:
  - alexandrevicenzi
slug: instalando-o-mariadb-no-centos-7
images:
  - /images/wp-content/uploads/2015/02/Mariadb-seal-shaded-browntext.png
categories:
  - tutoriais
  - distros
tags:
  - centos
  - linux
  - mariadb
  - mysql
  - sql
  - wordpress
---

{{< figure src="/images/wp-content/uploads/2015/02/Mariadb-seal-shaded-browntext.png" alt="MariaDB" width="250" >}}

O [MariaDB](https://mariadb.org/pt-br/) é um servidor de banco de dados que oferece as mesmas funcionalidades do MySQL. Na verdade ele é um fork do MySQL, feito após a sua compra pela Oracle. O [MariaDB](https://mariadb.org/pt-br/) é desenvolvido pela comunidade de software livre e por alguns dos autores originais do MySQL.

No meu caso utilizarei o [CentOS 7](https://www.centos.org/), mas este artigo pode ser usado como base para a instalação em outras distribuições.

## Instalação

Para instalar o MariaDB no Centos 7 execute o comando:

`sudo yum install mariadb-server mariadb`

Ao final da instalação deverá aparecer a mensagem _Completed!_.

## Configuração

Após instalado, devemos configurar alguns itens.

O primeiro é iniciar o serviço do MariaDB. Para iniciá-lo execute o comando:

`sudo systemctl start mariadb`

Após iniciado, vamos configurar a senha do _root_. Para isso, execute o comando:

`sudo mysql_secure_installation`

Note que este passo solicita a configuração de outros itens, então iremos por partes.

Inicialmente será solicitado a senha do _root_, conforme abaixo:

```
NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
SERVERS IN PRODUCTION USE! PLEASE READ EACH STEP CAREFULLY!

In order to log into MariaDB to secure it, we'll need the current
password for the root user. If you've just installed MariaDB, and
you haven't set the root password yet, the password will be blank,
so you should just press enter here.

Enter current password for root (enter for none):
```

Apenas aperte Enter, pois não existe uma senha definida.

Após isso, será solicitado se você deseja definir uma nova senha do _root_, conforme abaixo:

```
Setting the root password ensures that nobody can log into the MariaDB
root user without the proper authorisation.

Set root password? [Y/n]
```

Escolha **Y** e defina uma nova senha.

Se tudo ocorrer bem, você deverá ver algo semelhante a:

```
Password updated successfully!
Reloading privilege tables..
```

Ainda serão feitas outras perguntas sobre acesso a base, apenas escolha entre Y/N de acordo com a sua necessidade.

Após configurado, vamos colocá-lo para iniciar automaticamente no boot. Para isso, execute o comando:

`systemctl enable mariadb.service`

## Teste

Se tudo estiver certo, agora você já pode se conectar no MySQL. Para isso, execute o comando:

`mysql -u root -p`

Para fazer um pequeno teste no nosso BD, vamos criar um usuário e uma base para o [Wordpress](https://br.wordpress.org/).

Para criar uma base execute o seguinte comando:

`CREATE DATABASE wordpress;`

Para criar um usuário execute o seguinte comando:

`CREATE USER 'usuario'@'localhost' IDENTIFIED BY 'senha';`

Para atribuir permissões de acesso a base ao usuário execute o seguinte comando:

`GRANT ALL PRIVILEGES ON wordpress . * TO 'usuario'@'localhost'; FLUSH PRIVILEGES;`

Pronto!

## Conclusão

A instalação do MariaDB em si é bem simples. Espero que tenha gostado.

Até a próxima.
