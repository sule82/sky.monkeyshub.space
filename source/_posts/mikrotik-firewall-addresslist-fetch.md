---
title: Fetch remote & Update firewall filter
date: 2020-05-04 23:07:00
updated: 2020-05-04 23:07:00
categories:
  - Tutorials
tags:
  - mikrotik
  - script
  - routeros
---

# Update Mikrotik firewall liste adresa

## Uvod

Naravno, možemo i ručno obrisati access listu, pronaći novu i zaljepiti, ali gdje je tu fun <span role="img" aria-label="zvijezda">☀</span>

U ovom primjeru koristimo 3 malware list providera da preuzmem friške liste i python-om (dobrim starim) napravim jednu, uploadujem i hostam na serveru:

<!--more-->

## Refresh liste i upload na server

{% codeblock lang:python python-requests %}
#!/usr/bin/python3
#coding: utf-8
import re
import requests

sources = (
'https://feodotracker.abuse.ch/downloads/ipblocklist.txt',
'https://malc0de.com/bl/IP_Blacklist.txt',
'https://www.malwaredomainlist.com/hostslist/ip.txt',
)
destination =[/sftp:host/]'[/monkeyshub.space/][/mikrotik/malware.rsc]'

ips = [
'192.168.x.y', # (rucno)
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
f.write('remove [find list=malware]\n')
for ip in sorted(set(ips)):
f.write('add list=malware list={}\n'.format(ip))
{% endcodeblock %}

## RouterOS dio

Opcije:

1. ručno = 2-4 komande,
2. dodati skriptu,
3. skripta + scheduler,
4. skripta + scheduler + cron.

### Ručno

#### 1. Preuzimanje = fetch liste

{% codeblock tool>fetch %}
/tool fetch url=https://[server/mikrotik/]malware.rsc
{% endcodeblock %}

#### 2. Disable log rule

_Kako bi izbjegli spam log:_

{% codeblock system>logging %}
:foreach rule in=[/system logging find] do={
:if ([:find [/system logging get $rule topics] "info" -1] > -1) do={
/system logging disable \$rule
{% endcodeblock %}

#### 3. Import liste

{% codeblock console>import %}
import malware.rsc
{% endcodeblock %}

#### 4. Enable log rule

{% codeblock system>logging %}
:foreach rule in=[/system logging find] do={
:if ([:find [/system logging get $rule topics] "info" -1] > -1) do={
/system logging enable \$rule
{% endcodeblock %}

## Firewall filter

Naravno, lista je beskorisna ako nemamo firewall filter koji će je koristiti:

{% codeblock ip>firewall>filter %}
/ip firewall filter
add action=drop comment="malware lista" chain=forward disabled=no dst-address-list=malware log=yes log-prefix="malware-OUT:"
{% endcodeblock %}

{%  note info %}
update uskoro!
{%  endnote %}
