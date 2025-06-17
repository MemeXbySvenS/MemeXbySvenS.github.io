---
layout: single
title: Polar - Operando nodos LND via terminal
excerpt: "Polar es un aliado muy poderoso como ya vimos en el post anterior, en esta ocasión vamos a profundizar en la operación de los nodos por medio del terminal, para así tener un control total de las posibilidades que nos ofrecen las diferentes implementaciones."
date: 2025-06-17
figurePlacement: H
classes: wide
header:
  teaser: /assets/images/mbs-writeup-Polar_Terminal/carap_t.png
  teaser_home_page: true
  icon: /assets/images/mbs-writeup-Polar/icon.png
categories:
  - polar
  - lightning
tags:
  - lightning
  - bitcoin
  - nodos
  - LND
  - regtest
  - pow
  - terminal
---

# COMANDOS LND

![Terminal Polar - LND](../assets/images/mbs-writeup-Polar_Terminal/carap_t.png)


## COMANDOS BÁSICOS DE INFORMACIÓN

- getinfo

Muestra información general del nodo.

    lncli getinfo

![lncli getinfo](../assets/images/mbs-writeup-Polar_Terminal/1p_t.png)

---

- walletbalance

Balance de la wallet on-chain.

    lncli walletbalance

![lncli walletbalance](../assets/images/mbs-writeup-Polar_Terminal/2p_t.png)

---

- channelbalance

Balance total en canales Lightning.

    lncli channelbalance

![lncli channelbalance](../assets/images/mbs-writeup-Polar_Terminal/3p_t.png)

---

- listpeers

Ver conexiones activas con otros nodos.

    lncli listpeers

![lncli listpeers](../assets/images/mbs-writeup-Polar_Terminal/4p_t.png)

---

## CONEXIÓN CON OTROS NODOS

- connect

Conectar con otro nodo.

    lncli connect <pubkey>@<ip>:<port>
### Ejemplo:
    lncli connect 03abc...def@127.0.0.1:9735

![lncli connect 1](../assets/images/mbs-writeup-Polar_Terminal/5p_t.png)

![lncli connect 2](../assets/images/mbs-writeup-Polar_Terminal/6p_t.png)

---

- disconnect

Desconectarse de un peer.

    lncli disconnect <pubkey>

![lncli disconnect 1](../assets/images/mbs-writeup-Polar_Terminal/7p_t.png)

![lncli disconnect 2](../assets/images/mbs-writeup-Polar_Terminal/8p_t.png)

---

## TRANSACCIONES ON-CHAIN (Bitcoin)

- newaddress

Generar una nueva dirección para recibir BTC.

    lncli newaddress p2wkh

![lncli newaddress p2wkh](../assets/images/mbs-writeup-Polar_Terminal/9p_t.png)

---

- sendcoins

Enviar BTC on-chain.

    lncli sendcoins <direccion> 100000

![Optengo dirección](../assets/images/mbs-writeup-Polar_Terminal/10p_t.png)

![lncli sendcoins](../assets/images/mbs-writeup-Polar_Terminal/11p_t.png)

---

## CANALES LIGHTNING

- openchannel

Abrir canal con otro nodo.

    lncli openchannel --node_key=<pubkey> --local_amt=100000

![lncli openchannel](../assets/images/mbs-writeup-Polar_Terminal/12p_t.png)

---

- listchannels

Lista canales abiertos.

    lncli listchannels

![lncli listchannels](../assets/images/mbs-writeup-Polar_Terminal/13p_t.png)

---

- pendingchannels

Muestra canales pendientes de abrir/cerrar.

    lncli pendingchannels

![lncli pendingchannels](../assets/images/mbs-writeup-Polar_Terminal/14p_t.png)

---

- closechannel

Cerrar un canal (force o cooperativo).

    lncli closechannel --funding_txid=<txid> --output_index=<0-1>

![lncli closechannel](../assets/images/mbs-writeup-Polar_Terminal/15p_t.png)

---

- closedchannels

Lista de canales cerrados.

    lncli closedchannels

![lncli closedchannels](../assets/images/mbs-writeup-Polar_Terminal/16p_t.png)

---

## FACTURAS E INGRESOS

- addinvoice

Crear factura Lightning para cobrar.

    lncli addinvoice --amt=1000 --memo="prueba de pago"

![lncli addinvoice](../assets/images/mbs-writeup-Polar_Terminal/17p_t.png)

---

- listinvoices

Listar facturas generadas.

    lncli listinvoices

![lncli listinvoices](../assets/images/mbs-writeup-Polar_Terminal/18p_t.png)

---

## PAGOS ENVIADOS

- payinvoice

Pagar una factura recibida.

    lncli payinvoice <bolt11>

![lncli payinvoice](../assets/images/mbs-writeup-Polar_Terminal/19p_t.png)

---

- listpayments

Listar pagos realizados.

    lncli listpayments

![lncli listpayments](../assets/images/mbs-writeup-Polar_Terminal/20p_t.png)

---

## MACAROONS Y SEGURIDAD

- bakemacaroon

Crear un nuevo macaroon con permisos limitados.

    lncli bakemacaroon info:read

![lncli bakemacaroon](../assets/images/mbs-writeup-Polar_Terminal/21p_t.png)

---

## ADMINISTRACIÓN Y MANTENIMIENTO

- debuglevel

Ver los subsistemas disponibles

    lncli debuglevel --show

![lncli debuglevel --show](../assets/images/mbs-writeup-Polar_Terminal/22p_t.png)

Especificar niveles para subsistemas concretos

    lncli debuglevel --level=PEER=debug,HSWC=debug,FNDG=debug

![lncli debuglevel --level](../assets/images/mbs-writeup-Polar_Terminal/23p_t.png)

El archivo de los logs se encuentra en este caso en:

    ~/.lnd/logs/bitcoin/regtest/lnd.log

Y para verlos en tiempo real lo hacemos de la siguiente manera:

    tail -f ~/.lnd/logs/bitcoin/regtest/lnd.log

![tail -f](../assets/images/mbs-writeup-Polar_Terminal/24p_t.png)

---

- stop

Detener el nodo LND.

    lncli stop

---

- describegraph

Ver el grafo completo de la red Lightning.

    lncli describegraph

![lncli describegraph](../assets/images/mbs-writeup-Polar_Terminal/25p_t.png)

---

## UTILIDADES

- getchaninfo

Obtener info de un canal público por ID.

    lncli getchaninfo <channel_id>

![lncli listchannels](../assets/images/mbs-writeup-Polar_Terminal/26p_t.png)

![lncli getchaninfo](../assets/images/mbs-writeup-Polar_Terminal/27p_t.png)

---

- decodepayreq

Ver el contenido de una factura BOLT11.

    lncli decodepayreq <bolt11>

![lncli decodepayreq](../assets/images/mbs-writeup-Polar_Terminal/28p_t.png)

---

- feereport

Informe sobre fees que ganas en tus canales.

    lncli feereport

![lncli feereport](../assets/images/mbs-writeup-Polar_Terminal/29p_t.png)

---

## EXTRAS

Si quieres ver todos los comandos disponibles:

    lncli help

![lncli help](../assets/images/mbs-writeup-Polar_Terminal/30p_t.png)

---

Y para ver la ayuda específica de un comando:

    lncli <comando> --help

![lncli comando --help](../assets/images/mbs-writeup-Polar_Terminal/31p_t.png)

