---
layout: post
title: Como fazer um leitor compatível com libnfc no precinho.
---

Como fuçar na libnfc se o kit do Proxmark 3 custa $ 418,00 ? E os leitores do AliExpress custam com frete e impostos quase R$ 200,00 e só chegam depois de 3 meses ? A resposta é **ARDUINO !!!**

Pra montar esse leitor, precisamos de duas coisas:

 - Arduino UNO

![unor3]({{ site.baseurl }}/images/arduino/unor3.jpg)

- PN532 (ElecHouse)

![pn532]({{ site.baseurl }}/images/arduino/PN532-1.jpg)


*É fácil encontrar o PN532 no Mercado Livre por volta de R$ 55,00. Já o Arduino UNO, mais ainda.*

### O Hardware

A primeira coisa a se fazer é colocar o PN532 em modo HSU (High Speed Uart), colocando os switches na posição 0, e soldar os headers nos pinos: GND, VCC, TXD e RXD. Nesse caso eu fiz um shield usando uma placa de circuito ilhada.

A segunda é preparar o arduino. E pra isso você vai precisar retirar o arduino do arduino ????? Com **CUIDADO** retire o chip do arduino e tenha cuidado pra não amassar os pinos.

- Ligue o TXD do PN532 ao TX do Arduino.
- Ligue o RXD do PN532 ao RX do Arduino.

Ligue o Arduino no cabo USB e pronto, Bob's your uncle !!!

### O Software

Agora pra instalar os drivers do pn532 no computador vamos precisar de uma distro Linux. Kali Linux é bacana pois ele já vem com algumas ferramentas instaladas, como o Mifare Classic Offline Cracker (MFOC) e o MiFare Classic Universal toolKit (MFCUK). Mas em qualquer "ubuntão" funciona muito bem. Use os comandos abaixo pra instalar e configurar a libnfc no PN532 no Uno. ~~pfvr, desligue o arduino do pc antes rodar os comandos abaixo~~

```bash
apt-get install autoconf libtool libusb-dev libpcsclite-dev build-essential
git clone https://github.com/danfragoso/libnfc
cd libnfc
autoreconf -vis
./configure --with-drivers=pn532_uart --sysconfdir=/etc --prefix=/usr
make clean all
sudo make install
sudo mkdir /etc/nfc
sudo cp contrib/libnfc/pn532_uart2unousb.conf /etc/nfc/libnfc.conf
```
Ligue o leitor ao computador.

```bash
sudo nfc-list
```
Se você fez tudo certo o output deve ser parecido com esse.
```bash

nfc-list uses libnfc 1.7.1
NFC device: pn532_uart:/dev/ttyUSB0 opened

```



The easiest way to make your first post is to edit this one. Go into /_posts/ and update the Hello World markdown file. For more instructions head over to the [Jekyll Now repository](https://github.com/barryclark/jekyll-now) on GitHub.
