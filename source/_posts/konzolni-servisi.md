---
title: Konzolni servisi
date: 2020-05-29 22:07:00
updated: 2020-05-29 22:07:00
categories:
  - Tutorials
tags:
  - konzola
  - terminal
  - servisi
---

# Konzolni servisi

![terminal TAB+TAB](/fragments/images/termina-tab-tsb.png)

##  Klijenti

Lista korisnih konzolnih (terminalnih) servisa dostupnih preko HTTP, HTTPS ili drugog mreznog protokola.

{%  codeblock lang:bash  "command line interface" %}
$ command
{%  endcodeblock  %}

<!--more--> Potreban je barem jedan od klijenata instalisan na racunaru:

- aria2 [link](https://aria2.github.io/)
- bitsadmin [link](https://docs.microsoft.com/windows/win32/bits/)
- curl  [link](https://curl.haxx.se/)
- httpie  [link](https://httpie.org/)
- httrack [link](https://www.httrack.com/)
- powershell  [link](https://microsoft.com/powershell/)
- rclone  [link](https://rclone.org/)
- wget  [link](https://www.gnu.org/software/wget/)
- wget2 [link](https://gitlab.com/gnuwget/wget2)

{%  note info %}
Servisi su sortirani prema upotrebi / radi boljeg razumjevanja i upotrebe nazivi i komande na engleskom ().
{%  endnote %}

##  IP address

### InLine

```bash
curl l2.io/ip
```
```bash
curl echoip.de
```
```bash
curl ifconfig.me
```
```bash
curl ipecho.net/plain
```
```bash
curl -L ident.me #API http://api.ident.me/
```
```bash
curl -L canihazip.com/s
```
```bash
curl -L tnx.nl/ip
```
```bash
curl bot.whatismyipaddress.com
```
```bash
curl whatismyip.akamai.com
```
```bash
curl curlmyip.net
```
```bash
curl api.ipify.org
```
```bash
curl ipv4bot.whatismyipaddress.com
```
```bash
curl ipcalf.com
```

### New Line

```bash
curl ipcalf.com
```
```bash
curl ipaddr.site
```
```bash
curl ifconfig.pro
```
```bash
curl curlmyip.net
```
```bash
curl ipinfo.io/ip
```
```bash
curl checkip.amazonaws.com
```
```bash
curl smart-ip.net/myip
```
```bash
curl ip-api.com/line?fields=query
```
```bash
curl ifconfig.io/ip
```

###   DNS (IPv4 & IPv6)

```bash
dig @1.1.1.1 whoami.cloudflare ch txt +short
```
```bash
dig @2606:4700:4700::1111 whoami.cloudflare ch txt -6 +short
```
```bash
dig @ns1.google.com o-o.myaddr.l.google.com TXT -6 +short
```
```bash
dig @ns1.google.com o-o.myaddr.l.google.com TXT -4 +short
```
```bash
dig resolver.dnscrypt.info TXT +short
```
```bash
curl https://dnsjson.com/resolver.dnscrypt.info/TXT.json
```

### JSON


```bash
curl httpbin.org/ip
```
```bash
curl wtfismyip.com/json
```
```bash
curl -L iphorse.com/json
```
```bash
curl geoplugin.net/json.gp
```
```bash
curl https://ipapi.co/json
```
```bash
curl -L jsonip.com
```
```bash
curl gd.geobytes.com/GetCityDetails
```
```bash
curl ip.jsontest.com
```

##  Geolocation

```bash
curl ipinfo.io/8.8.8.8 #ili
curl ipinfo.io/8.8.8.8/loc
```
```bash
curl ifconfig.co/country #ili
curl ifconfig.co/city #ili
curl ifconfig.co/country-iso #ili
http ifconfig.co/json
```

##  Text Sharing

```bash
echo "Ovo zelim da podijelim sa svijetom" | curl -F 'f:1=<-' ix.io
```
```bash
echo "Ovo zelim da podijelim sa svijetom!" | curl -F file=@- 0x0.st
```
```bash
echo "Ovo zelim da podijelim sa svijetom!" | curl -F 'clbin=<-' https://clbin.com
```
```bash
echo "Ovo zelim da podijelim sa svijetom!" | nc termbin.com 9999
```
```bash
echo "Ovo zelim da podijelim sa svijetom!" | curl -F 'sprunge=<-' sprunge.us
```

##  URL shortener

```bash
curl --upload-file <fajl> transfer.sh/<naziv-fajla>
```
```bash
curl --upload-file <fajl> filepush.co/upload/<naziv-fajla>
```
```bash
curl -F file=@<fajl> https://ttm.sh
```

##  Browser

```bash
ssh brow.sh
```

##   Monitoring

```bash
curl ping.gg
```

##  Weather

```bash
curl wttr.in #ili
curl wttr.in/Gracanica,BA
```
```bash
finger oslo@graph.no
```

##  Map

`telnet mapscii.me`

##	Documentation

```bash
curl cheat.sh
```

##	Dictionaries and translators

```bash
curl 'dict.org/d:command line'
```

##	Generators

```bash
curl -H 'Accept: text/plain' foaas.com/cool/:from   # fuck off kao servis
```
```bash
curl pseudorandom.name  #  generira pseudo random ime
```

##	Entertainment and Games

```bash
nc towel.blinkenlights.nl 23   # StarWars u terminalu
```

```bash
ssh chat.shazow.net   # chat preko SSH (shazow/ssh-chat)
```

```bash
curl parrot.live  # animirani party papagaj (hugomd/parrot.live)
```

```bash
curl node-web-console.glitch.me   # emoji utrka (source)
```

###	Telnet/SSH:

`ssh sshtron.zachlatta.com` — snake, awsd tipke
`ssh netris.rocketnine.space` — multiplayer tetris
`ssh nethack@alt.org` — fun roguelike game
`ssh play@anonymine-demo.oskog97.com -p 2222` — pass: play
`ssh twenex@sdf.org` — checkers i sl.
`telnet freechess.org` — šah
`telnet milek7.gq` — pong, break out, tetris
`telnet aardmud.org` — MUD
`telnet mud.darkerrealms.org 2000` — MUD
`telnet telehack.com` — dosta zanimljivih stvari
