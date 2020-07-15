---
title: Mikrotik supout.rif
date: 2020-05-04 20:25:00
updated: 2020-05-04 20:25:00
categories:
  - Research
tags:
  - mikrotik
  - script
  - routeros
---

# Mikrotik 'support' fajl
<!--more-->
## Šta je supout.rif fajl

Koristi se za uklanjanje pogrešaka MikroTik RouterOS i brže riješavanje problema upućenih podršci.
Sadrži svu konfiguraciju rutera, logove i sl podatke, ne sadrži šifre i druge osjetljive podatke. Na mikrotik.com u izborniku sa ljeve strane (nakon logovanja u svoj account) imate supout.rif viewer, otprilike ovakav rezultat dobijete nakon uploada fajla:

{% codeblock, lang:python,  output sa mt offical %}
uptime: 1h24m54s
version: 6.45.2 (stable)
build-time: Jul/17/2019 10:04:19
factory-software: 6.42.1
free-memory: 204.1MiB
total-memory: 256.0MiB
cpu: ARMv7
cpu-count: 4
cpu-frequency: 716MHz
cpu-load: 6%
free-hdd-space: 2272.0KiB
total-hdd-space: 15.3MiB
write-sect-since-reboot: 599
write-sect-total: 3244
bad-blocks: 0%
architecture-name: arm
board-name: SXTsq 5 ac
platform: MikroTik

CLOCK
time: 05:13:17
date: may/04/2020
time-zone-autodetect: yes
time-zone-name: Europe/Sarajevo
gmt-offset: +02:00
dst-active: yes

SYSTEM
Linux 3.3.5 #1 SMP Wed Jul 17 09:19:10 UTC 2019 armv7l unknown

CPU
Processor	: ARMv7 Processor rev 5 (v7l)
processor	: 0
BogoMIPS	: 1430.32

processor	: 1
BogoMIPS	: 1430.32

processor	: 2
BogoMIPS	: 1430.32

processor	: 3
BogoMIPS	: 1430.32

Features	: swp half thumb fastmult vfp edsp neon vfpv3 tls vfpv4 idiva idivt
CPU implementer	: 0x41
CPU architecture: 7
CPU variant	: 0x0
CPU part	: 0xc07
CPU revision	: 5

Hardware	: Qualcomm (Flattened Device Tree)
Revision	: 0000
Serial		: 0000000000000000
{%  endcodeblock  %}

## Kako izgleda?

Supout.rif fajl se sastoji od kodiranih blokova / sekcija (👓 ⬇), njih 62, dole imate primjer, a vodimo da "oficijelni" alat dekodira resurse i periferije (max 3 sekcije).

{% codeblock, lang:python,  base64  kodirana  sekcija  %}

--BEGIN ROUTEROS SUPOUT SECTION
pB3clNGA4xZhUF1jaCEE+dS8/wm7xGon6dt5qJ8g5krHJeiR4h2naWZHkNuwS3d56Z/13BUBUULP
oMz3MzOz38xu0zbF55gFv4/9BWvIob0TI/g4QYcNdtAY2kZVW7ypZ8YbyK0QB6CZODUkBWDsWWVg
lrCexfuXTF+UVOQCtUYwgIDRHYBA37O48OSKVn6QFbkKuJNzVnSHRg84Ouog2Z04nsfgB6qiU9wS
d2oklFuZSWx4hP+kd1LjGO+RignAGOeGjYkClsQqpCn4UIerrcNs7YFypGHji+OoQc3dYpZFMHeu
BUvjOGnVbnR/gnVm5kQ5iSced/S9s6PzbRkfkvXYzkeC70l42TOrCWGEOd+Vo2e0UfeiWa6wT69E
1hZ5UCTXzYj/yXdiXHbXzefbc77jfq69Wa6hhZN1pIR3hXrpzqu/tgZenrN60y9VD1L6DTgC+dJo
NOycxu7ajyNXWX7VevFE5RqUPtk5J1zmsANx9lxpAOK2Cm7/8PbiPCDwAZFCqBsvls1HN454CmjI
TRDash/OmzAre7gIcG2TGHdrVINzYYnpdnM5+hEGOXn4AFcGZsU4SFiGWtRG2Uoj9aturWQtIMa6
85ezIhTbmqXxOK9POUgeoZn+qNxD/3LcZdWYOR+hR+PH2cUde45OgSJVYlGeCOCsuMJBUXHPFPzr
ifImcpj2gDRDO6r2hzRW4C53EDKwgzwbw0VSm84zxJnXD4jCuCYng3eAcdG1gf4f98PoEaxRkjrm
bk/xpvQK4x8e4IYNyOn1CZ82z6vTC468LhILNXGvCYdZOT0jf6gUfh31rfVknv/q89/2ftx0f/1i
d79XbcXa/VDfb+rbIMUoeh63nfHYhGEhky45bmQ2CqcQcfOYu/jEVW9P/q6K0PvVSwInsQS0lxpk
EuAISFehiCiNS1OrV4BAqgQ8GqMpBIrC9fDvBeDPHwVL3wRN1fBm1+jg0oSs0FMCrMrwy6fAE2mK
VC==
--END ROUTEROS SUPOUT SECTION

--BEGIN ROUTEROS SUPOUT SECTION
oVWYsRHaAgHnjXuAAAgJAgB=  ///...itd
{%  endcodeblock  %}

## Kako doći do fajla?

Spout.rif putem winbox-a ili webfig-a generišemo klikom na "Make Supout.rif", nakon par sekundi je spreman za download u "Files" pregledniku.

![rif](/fragments/images/rif.png)

U konzoli - cli komanda je:

```javascript
/system sup-output name=supout.rif
```
## Koje to podatke krije?

2015 godine Kirils Solovjovs je u par linija napravio python scripticu koja dekodira kompletan supout.rif uz pomoć `tribit`-a i `base64.b64decode`.

```python
 ("Usage: "+sys.argv[0]+" <supout.rif> [output_folder]")
 # u akciji:
 $ python desupout.py supout.rif [naziv-foldera]
```

Nakon dekripcije dobijemo folder sa:

![folder](/fragments/images/folder.png)

**output u 06_resource**:

```javascript
uptime: 1h24m54s
version: 6.45.2 (stable)
build-time: Jul/17/2019 10:04:19
factory-software: 6.42.1
free-memory: 204.1MiB
total-memory: 256.0MiB
cpu: ARMv7
cpu-count: 4
cpu-frequency: 716MHz
cpu-load: 6%
free-hdd-space: 2272.0KiB
total-hdd-space: 15.3MiB
write-sect-since-reboot: 599
write-sect-total: 3244
bad-blocks: 0%
architecture-name: arm
board-name: SXTsq 5 ac
platform: MikroTik
/// itd...
```
OK, imate info o SVIM parametrima, i trebalo bi da služi za debug i tuning/reparaciju ne debakl uređaja ♨📟♨.

Vjerujem da većini full info routerboard-a i nije toliko zanimljivo. Ako je uređaj pod garancijom bug/problem će otkloniti distributer zamjenom ili servisom. Lično, koristio sam često, naročito u otklanjanju problema sa boot-om, memorijom, napajanjem, PoE interfejsima...

{%  note warning  %}
Volim routerOS i poštujem Mikrotik dečke, i njihove odluke. Tako da... pišite na mail ako ne uspijete pronaći skriptu.
{%  endnote %}
