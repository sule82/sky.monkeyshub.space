---
title: Fetch remote & update firewall malware filter
date: 2020-05-04 23:07:00
updated: 2020-05-04 23:07:00
categories:
  - Tutorials
tags:
  - mikrotik
  - script
  - routeros
---

# Update malware liste

## Uvod

Naravno, možemo i ručno obrisati access listu, pronaći novu i zaljepiti, ali gdje je tu fun ☀🐮☀

Ovdje koristim 3 providera da preuzmem friške liste i python-om (dobrim starim) napravim jednu, uploadujem i hostam na serveru:

<!--more-->

```python
#!/usr/bin/python3
# -*- coding: utf-8 -*-

import re
import requests

sources = (
    'https://feodotracker.abuse.ch/downloads/ipblocklist.txt',
    'https://malc0de.com/bl/IP_Blacklist.txt',
    'https://www.malwaredomainlist.com/hostslist/ip.txt',
)
destination = ***/sftp:host=sftp.sd6.gpaas.net/***/shou.monkeyshub.space/***/mikrotik/malware.rsc'

ips = [
    '192.168.x.y',      # (custom)
]

ip_re = re.compile(r'(?<![0-9])(?:(?:[0-1]?[0-9]{1,2}|2[0-4][0-9]|25[0-5])[.](?:[0-1]?[0-9]{1,2}|2[0-4][0-9]|25[0-5])[.](?:[0-1]?[0-9]{1,2}|2[0-4][0-9]|25[0-5])[.](?:[0-1]?[0-9]{1,2}|2[0-4][0-9]|25[0-5]))(?![0-9])')

for source in sources:
    try:
        r = requests.get(sofeodotrackerurce)
        for line in r.text.splitlines():
            m = ip_re.match(line)
            if m:
                ips.append(m.group(0))
    except:
        pass

with open(destination, 'w') as f:
    f.write('/ip firewall address-list\n')
    f.write('remove [find list=Malware]\n')
    for ip in sorted(set(ips)):
        f.write('add list=Malware address={}\n'.format(ip))
```

# Download liste na routerOS

Na ruteru brisemo staru i dohvatamo novu listu (opcije: 1' ručno / 2-4 komande, 2' dodati skriptu, 3' skripta + scheduler, 4' skripta + scheduler + cron).

```bash
#Dohvati listu
/tool fetch url=https://shou.monkeyshub.space/mikrotik/malware.rsc

# Stopiraj info loganje - izbjegni log spam ->
:foreach rule in=[/system logging find] do={
    :if ([:find [/system logging get $rule topics] "info" -1] > -1) do={
        /system logging disable $rule
    }
}

# Uvezi listu
import malware.rsc

# Pokreni ponovo info loganje
:foreach rule in=[/system logging find] do={
    :if ([:find [/system logging get $rule topics] "info" -1] > -1) do={
        /system logging enable $rule
    }
}
```

# Firewall filter

Naravno, vrijedi 0 ako nemamo firewall filter koji će je koristiti:

```firewall
;;; Log and drop malware traffic
      chain=forward action=drop dst-address-list=Malware log=yes
      log-prefix="OUT MALWARE:"
```
Kod mene je našao mjesto poslje drop blocked IP liste a prije bypass allowed IP liste. Nisam dovoljno testirao, čini mi se da radi brže uz fasttrack, te ova mala zaštita je dovoljna za klijenta, ISP ih već reže uveliko.

[To Do]:

  - [x] napravit api sa json bazom.

Na ovaj način možete praviti i ostale liste, ili čak produžiti ovu sa ad blockerima i sl.

{%  note info %}
Na ovaj način možete praviti i ostale liste, ili čak produžiti ovu sa ad blockerima i sl:
Za host filtriranje na svim uređajima (💻 ☎ 📟 📺) potražite Energized ⚫ *dobra polazna tačka*.
{%  endnote %}
