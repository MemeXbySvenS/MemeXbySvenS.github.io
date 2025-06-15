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
  - regtest
  - pow
---

![Polar](../assets/images/mbs-writeup-Polar/carapolar.png)

## Descarga e instalación

Como en todo lo que concierne a bitcoin, **don't trust, verify**, asique con esta expresión tan cierta lo primero que tenemos que hacer es ir a la fuente de la solución que queremos probar o utilizar, en este caso debemos ir directamente al repositorio de [github](https://github.com/jamaljsr/polar.git).

![Portada Github](../assets/images/mbs-writeup-Polar/1Polar.png)

Una vez aquí, nos dirigimos al apartado **dependencias** en el que nos explica que requisitos necesitamos para la instalación.

Como puedes observar, necesitarás instalar Docker para poder crear las diferentes redes.

Dependiendo de cual sea tu sistema operativo, tendras que instalar para Mac y Windows  [Docker Desktop](https://www.docker.com/products/docker-desktop) o si utilizas Linux como es mi caso y como vamos a seguir este post [Docker Server](https://docs.docker.com/engine/install/#server).

Si lo instalas como yo en Linux, sigue estos pasos:

Nos dirigimos al enlace y se nos abrirá la pagina principal de DockerDocs.

![Portada Dockerdocs](../assets/images/mbs-writeup-Polar/2Polar.png)

Elegimos de entre la lista de distribuciones la nuestra, en mi caso voy a seleccionar Ubuntu por que la distribución que utilizo es Pop!_OS que esta basada en ella.

Después de leernos las advertencias y comprobar que cumplimos los requisitos nos dirigimos en esa misma pagina un poco mas abajo en donde dice **Métodos de Instalación** y hacemos clic a [Docker's APT repository](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository), y añadimos desde apt con la clave oficial GPG, la secuencia de comandos seria la siguiente:

    sudo apt-get update

    sudo apt-get install ca-certificates curl

    sudo install -m 0755 -d /etc/apt/keyrings

    sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc

    sudo chmod a+r /etc/apt/keyrings/docker.asc

![Terminal instalación Docker 1](../assets/images/mbs-writeup-Polar/3Polar.png)

Añadimos el repositorio de Docker:

    echo \

    "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \

    $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \

    sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

    sudo apt-get update

![Terminal instalación Docker 2](../assets/images/mbs-writeup-Polar/4Polar.png)

Instalamos los paquetes de Docker:

    sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin.

![Terminal instalación Docker 3](../assets/images/mbs-writeup-Polar/5Polar.png)

Luego es recomendable, para que Polar pueda ejecutar comandos sudo, añadir nuestro usuario al grupo docker:

    sudo usermod -aG docker $USER

![Terminal agregar usuario](../assets/images/mbs-writeup-Polar/9Polar.png)

Recuerda que si añades tu usuario al grupo docker, tienes que reiniciar para aplicar el cambio.

Para finalizar y verificar que la instalación se ha realizado correctamente utilizamos el siguiente comando, que descarga una imagen de prueba y la ejecuta en un contenedor:

     sudo docker run hello-world

Y el mensaje en pantalla si todo ha salido bien es Hello from Docker!

![Terminal comprobar Docker 1](../assets/images/mbs-writeup-Polar/6Polar.png)

Ahora aunque la instalación haya salido bien, vamos a cerciorarnos de que esta corriendo:

    sudo systemctl status docker

![Terminal comprobar Docker 2](../assets/images/mbs-writeup-Polar/7Polar.png)

Si no estuviera activo, lo activaríamos con el siguiente comando:

    sudo systemctl start docker

Y si queremos que inicie automáticamente al arrancar, lo haríamos de la siguiente manera:

    sudo systemctl enable docker

Una vez que tenemos Docker instalado y funcionando, pasamos a instalar Polar y para ello volvemos a repositorio de [github](https://github.com/jamaljsr/polar.git) a la parte de **Descarga** y elegimos nuestro sistema operativo, y como Linux es un ecosistema muy rico en recursos, nos ofrece tres maneras diferentes de descargarnos los paquetes, yo voy a elegir en esta ocasión .deb, para lo cual creo una carpeta Polar y desde ahí hago la instalación:

    sudo dpkg -i polar-linux-amd64-v3.2.0.deb

![Terminal instalación Polar](../assets/images/mbs-writeup-Polar/8Polar.png)

Y con esto ya tendríamos Polar instalado y preparado para empezar a hacer redes, nodos y transacciones, nos metemos en harina!

## Interface

Empezamos haciendo clic a Crea una red Lightning.

![¡Empecemos!](../assets/images/mbs-writeup-Polar/10Polar.png)

Le damos un nombre, una descripción y elegimos los nodos con las diferentes implementaciones que ofrece más aparte un nodo Bitcoin Core así como Taproot Assets o los nodos en modo terminal que queremos implementar y le hacemos clic a Crear red.

![Crear red](../assets/images/mbs-writeup-Polar/11Polar.png)

Al ser la primera vez que creamos una red, tarda un poco en que docker cree las imágenes,hacemos clic en Inicio y es el momento de ir a por la bebida y hacer palomitas...

![Iniciar red](../assets/images/mbs-writeup-Polar/12Polar.png)

Después de que haya creado las imágenes de las diferentes implementaciones nos aparecerá un grafo con los nodos y las conexiones que podemos hacer entre ello, así como si nos posicionamos encima con el cursor, en la ventana de la derecha nos mostrará información de cada uno ademas de poder hacer diferentes acciones como veremos ahora mismo.

![Pantalla principal](../assets/images/mbs-writeup-Polar/13Polar.png)

Si hacemos clic con el cursor encima de la conexión nos dará información de que tipo de conexión es. Para cambiar las conexiones entre los diferentes nodos Polar a divido el tipo de conexión según su posición, las conexiones que están encima y abajo del nodo son para los backend y las conexiones que están a la derecha y a la izquierda son para canales.

![Cambiar backend](../assets/images/mbs-writeup-Polar/14Polar.png)

Volviendo a la ventana de la derecha, tenemos la opción de ver las direcciones IP como los puertos para las diferentes conexiones.

![Conexiones](../assets/images/mbs-writeup-Polar/15Polar.png)

Y en la pestaña de **Comportamiento** es donde podemos en el caso del nodo de Bitcoin Core minar bloques para que podamos tener liquidez para la red.

![Minar en backend](../assets/images/mbs-writeup-Polar/16Polar.png)

 O en el caso de los nodos lightning depositar fondos, abrir canales o hacer pagos.

![Comportamiento en Nodo LD](../assets/images/mbs-writeup-Polar/17Polar.png)

En la parte superior de la ventana tenemos las opciones de **Minado rápido** o  configurar la red para que mine automáticamente a diferentes velocidades dependiendo del realismo que le queramos dar a la simulación.

También nos ofrece información de si esta iniciada la red, como la altura de bloque minado.

![Minar automaticamente](../assets/images/mbs-writeup-Polar/18Polar.png)


## Minado de bloques

Como se describió antes, para minar bloques, tenemos que dirigirnos a la pestaña comportamiento con el nodo backend de Bitcoin Core seleccionado.

Seleccionamos la cantidad de bloques que queremos minar y clicamos en minar.

![Minando](../assets/images/mbs-writeup-Polar/19Polar.png)

## Depositar fondos

Después de haber minado unos bloques, y para empezar a utilizar la red lo que debemos hacer es proveer de liquidez a los diferentes nodos lightning , y esto lo conseguimos posicionándonos en los diferentes nodos lightning y dirigiéndonos a la pestaña **comportamiento** de la ventana de la derecha, haciendo clic en **depositar** después de haber elegido una cantidad si con la que viene por defecto, que es de 1000000 de satoshis, no es suficiente.

![Depositar fondos](../assets/images/mbs-writeup-Polar/20Polar.png)

Si después nos dirigimos al nodo backend de Bitcoin Core, podemos observar que ha restado del total la cantidad que hemos depositado.

![Monto del Backend](../assets/images/mbs-writeup-Polar/21Polar.png)

Y si vamos al nodo lightning podemos ver que la liquidez ahí está.

![Monto del nodo](../assets/images/mbs-writeup-Polar/22Polar.png)

## Creación de canales

Una vez que toda la red tiene liquidez, es momento de empezar a abrir canales, y esto lo podemos conseguir de dos maneras...

La primera es dirigiéndonos a la ventana derecha y en **comportamiento** elegir abrir canal **entrante** o **saliente** dependiendo de las necesidades y escenarios que queramos probar.

Con esta manera de hacerlo vamos a crear un canal entrante de Bob a Alice.

![Inicio de apertura de canal](../assets/images/mbs-writeup-Polar/23Polar.png)

Elegimos a Alice en la ventana emergente.

![Elección de Alice](../assets/images/mbs-writeup-Polar/24Polar.png)

La capacidad del canal y luego hay dos checkbox...

El primero en el que dice: Deposita suficientes asientos a Alice para financiar el canal, lo que quiere decir en realidad si esta marcado es que Polar le daría automáticamente a Alice suficientes fondos para que pueda abrir el canal.

El segundo checkbox haría el canal privado.

![Apertura de canal](../assets/images/mbs-writeup-Polar/25Polar.png)

Como podemos ver al crear el canal, automáticamente Polar a depositado en la cuenta de Alice los 10000000 satoshis.

![Deposito automático](../assets/images/mbs-writeup-Polar/26Polar.png)

La segunda manera de abrir canales es directamente en los nodos en su parte derecha e izquierda, utilizando el ratón para ello y arrastrando hacia el nodo donde queramos abrir el canal.

Para este ejemplo vamos a abrir un canal de Dave a Carol.

![Apertura manual](../assets/images/mbs-writeup-Polar/27Polar.png)

![Configura el canal](../assets/images/mbs-writeup-Polar/28Polar.png)

## Pagos y creación de facturas

Para realizar pagos o crear una factura nos debemos dirigir a la pestaña **Comportamiento** del nodo desde el que queramos hacer la operación.

Para hacer una factura, clicamos en su botón y en la ventana emergente elegimos el monto que quieres reclamar y hacemos clic en **Crear factura**.

![Crear factura](../assets/images/mbs-writeup-Polar/29Polar.png)

Nos devolverá otra ventana emergente en el que nos informa de que se ha realizado con éxito y una dirección para que la contraparte te realice el pago, copiar y cerrar.

![Éxito creando factura](../assets/images/mbs-writeup-Polar/30Polar.png)

Ahora con esa dirección copiada debemos ir al nodo que queramos realice el pago, pestaña **Comportamiento** y **Pagar factura**.

![Bob paga factura](../assets/images/mbs-writeup-Polar/31Polar.png)

En la ventana emergente, pegamos la dirección en el cuadro reservado para ello.

![Pega direccion](../assets/images/mbs-writeup-Polar/32Polar.png)

Y nos informa de que el pago se ha realizado correctamente.

![Pago realizado](../assets/images/mbs-writeup-Polar/33Polar.png)

## Cierre de canal

Como en la creacion de canales, para cerrarlos tambien tenemos dos maneras de operar.

La primera seria al hacer clic sobre el enlace del canal y en la información abajo del todo nos indica un botón **Cerrar canal**.

![Cierre de canal 1](../assets/images/mbs-writeup-Polar/34Polar.png)

Y la otra seria hacer clic también sobre el enlace del canal, pero con clic derecho y directamente nos daría la opción.

![Cierre de canal 2](../assets/images/mbs-writeup-Polar/35Polar.png)

![Cierre](../assets/images/mbs-writeup-Polar/36Polar.png)

![Cierre confirmado](../assets/images/mbs-writeup-Polar/37Polar.png)

### Operar mediante Terminal

Las implementaciones pueden ser operadas directamente por terminal, si... ya se que no es tan bonito...pero solo por la potencia que tiene merece la pena que le eches un ojo.

Si te diriges en la pestaña de **Comportamiento** y bajas un poco te encontraras un boton en el que te invita a **Ejecutar** el terminal.

![Ejecutar terminal](../assets/images/mbs-writeup-Polar/38Polar.png)

Cada implementación tiene sus propios comandos, pudiendo compartir algunos.

[Bitcoin-cli](https://chainquery.com/bitcoin-cli)

[c-lightning-cli](https://docs.corelightning.org/reference/lightning-cli)

[LND-cli](https://docs.lightning.engineering/lightning-network-tools/lightning-terminal/command-line-interface)

[Eclair-cli](https://github.com/ACINQ/eclair/blob/master/eclair-core/eclair-cli)


Hoy en día hay recursos de sobra para poder saber los comandos básicos para moverte medianamente bien en la terminal como preguntar a Chat-gpt o utilizar el comando help dependiendo de la implementación, por ejemplo en c-lightning el comando de ayuda seria:

    lightning-cli help

![Comando lightning-cli help](../assets/images/mbs-writeup-Polar/39Polar.png)

![Salida comando help](../assets/images/mbs-writeup-Polar/40Polar.png)

La salida ofrece todos los comandos que utiliza, de hecho daría para un post única y exclusivamente dedicado a operar con la terminal, te animo a probar.

## Conclusiones

Polar es una herramienta muy completa y potente para operar los nodos de las diferentes implementaciones que ofrece y hacer todas las pruebas que imagines con la seguridad de hacerlo en una red controlada por ti, con fondos ilimitados manejados por ti y en la que si desarrollas una solución lightning , poderla testear introduciéndola en una red.

Es imposible cubrir todas las posibilidades que ofrece en un solo post, así que no descarto repetir centrándome en temas específicos como el de la terminal o Taproot Assets, que este ni he entrado.

Espero que el post te haya gustado y resultado de ayuda... y recuerda... la mejor manera de verificar algo... es hacerlo tu mismo!

¡Un saludo!

¡Nos vemos en el siguiente!

SvenS










