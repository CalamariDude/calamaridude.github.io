---
title: "Oblivion Writeup"
date: 2022-08-08T10:48:17-05:00
draft: false
tags: networking
---

## from HackArmour

## Setup
came with executable (exfiltration)

hexedit and binwalk showed arm instruction set and a few words but nothing of use

running it produces :

	Инструмент эксфильтрации            
	Работающий...

	отправленная информация (1 байт)...[OK]

	...(48 times TOTAL)

	отправленная информация (1 байт)...[OK]
	АдИос

which translates to:

	exfiltration tool

	working....

	information sent (1 Byte)..[OK]

	...(48 times Total)

	information sent (1 Byte)..[OK]

	adios


## Lets get going

I pcap'd networking to see what packets were being sent with

`tcpdump -w savetofile.pcap`

opened the file in wireshark - important, my computer sends a lot of packets for other applications (spotify, arp etc), I closed those, but the output still wasn't clean, I had to guess that the 48 "information sent" outputs corresponded to 48 DNS queries in wireshark

example:

`48    6.940687    10.0.0.8    8.8.8.8    DNS    70    Standard query 0x0054 A amazon.com`

I tried looking through to see what was different, turns out the "transaction id" was different in each DNS query, in the above example, its 0x0054. I took all 48 DNS queries and looked at the transaction id and wrote them in order here:

`47 72 65 65 74 69 6e 67 73 21 20 54 68 65 20 74 6f 6b 65 6e 20 66 6f 72 20 74 68 69 73 20 67 61 6d 65 20 69 73 20 4b 47 46 44 49 44 51 53 51 54`

convert this to hex gets you: 

Greetings! The token for this game is `KGFDIDQSQT`
Which was the flag

# Tools

[Wireshark](https://www.wireshark.org/)

