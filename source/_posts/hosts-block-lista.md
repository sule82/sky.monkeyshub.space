---
title: Blokiranje adresa preko hosts fajla
date: 2020-07-27 12:01:00
updated: 2020-07-27 12:01:00
categories:
  - Tutorials
tags:
  - mikrotik
  - hosts
  - script
---

# blokiranje internet sadržaja preko hosts + Mikrotik address-list

{% code lang:bash update_hosts.sh %}
#!/bin/bash
#Block List hosts Generator
	read -r -p "Jesi li spreman za update hosts liste? [y/N] " response
	if [[ "$response" =~ ^([yY][eE][sS]|[yY])$ ]]
	then
echo "Ciscenje & preuzimanje hosts listi..."
mkdir -p ~/Projects/scripts/BLT/hosts
cd ~/Projects/scripts/BLT/hosts
rm -f hosts.* *.final *.hosts novihosts.txt
sleep 1 #*...
{% endcode %}
<!-- more -->
## Šta je hosts fajl?

Hosts fajl fajl operativnog sistema koji mapira hostnames u IP adrese. Za razliku od udaljenih DNS servera ovdje korisnik / admin lokalne mašine ima potpuno kontrolu. Čisti je tekstualni fajl i u najjednostavnijoj verziji izgleda ovako:

{% code etc/hosts %}
127.0.0.1  localhost loopback
::1        localhost
{% endcode %}

Ovaj primjer sadrži samo loopback adrese sistema i njegove hostnames. IP adresa može sadržavati višestruke hostnames i hostname može biti mapiran i do IPv4 i IPv6 IP adresa. Finija, proširena verzija, sa opcijama koju preporučujem, i koja će se koristi kao početak svakog generiranog hosts fajla bi bila:

{% code etc/hosts %}
127.0.0.1	localhost localhost.localdomain localhost4 localhost4.localdomain4
::1 ip6-localhost ip6-loopback localhost6 localhost6.localdomain6
# 127.0.1.1	UNESI-SVOJ-HOSTNAME-OVDJE-I-UKLONI-#
# Poželjne linije za IPv6 sposoban hosts na LiGNUx:
# fe00::0 ip6-localnet
# ff00::0 ip6-mcastprefix
# ff02::1 ip6-allnodes
# ff02::2 ip6-allrouters
{% endcode %}

Da bi mapiranje funkcionisalo mašina (OS) treba da bude podešena da po defaultu čita hosts fajl. I da, obavezno {% label success@(preporučuje se toplo) %} treba ukloniti IPv6 konekciju sa mrežnog interfejsa {% label info@ako je ne koristimo %}, inače poželjno je generirati hostnames mapu posebno za IPv4 i za IPv6 IP adrese.

## Gdje se nalazi?

| Operativni sistem | verzija | lokacija |
| :---------------- | :------ | -------: |
| Unix, Unix-like, POSIX |   | /etc/hosts |
| Microsoft Windows | 95, 98, ME | %WinDir%\hosts |
| Microsoft Windows | NT, 2000, XP, 2003, Vista, 2008, 7, 2012, 8, 10 | %SystemRoot%\System32\drivers\etc\hosts |
| Apple Macintosh | Mac OS X 10.2 i noviji | /etc/hosts (link, /private/etc/hosts) |
| Android |   | /etc/hosts (link, /system/etc/hosts) |
| iOS | iOS 2.0 i noviji | /etc/hosts (link, /private/etc/hosts) |

## Šira primjena

U svojoj funkciji {% label warning@resolving host names %}, host fajle se može koristiti za definisanje bilo kojeg hostname ili domain name za korištenje u lokalnom sistemu.

### Preusmjeravanje lokalnih domena

Neki web servisi, intranet developeri i administratori definišu lokalno definisane domene u LAN-u za razne potrebe.

### Blokiranje internet resursa

**Unosi u hosts fajlu se mogu koristiti za blokiranje online reklama, ili/i domena (poznatog) zlonamjernog sadržaja i servera koji sadrže spyware, adware i dr malware.**
Ovo postižemo unosom zahtjeva za preusmjerenje do drugih adresa koje ne postoje ili do sigurnih destinacija kao što je lokalna mašina.

{% note info %}
Dalje u ovom članku ćemo se pozabaviti upravo ovom proširenom funkcijom hosts fajla.
{% endnote %}

#### hosts primjer za blokiranje internet resursa

Primjer hosts fajla koji {% label info@pokušava %} preusmjeriti / blokirati google tracking i google ads (unosi preuzeti od adguard & energized), {% label warning@napomena: sa ovom listom nije moguće napraviti 100% g-ad-block filter %}, te za google treba specificirati po regionima kako bi "odustao" od plasiranja reklama:

{% code etc/hosts %}
127.0.0.1	localhost localhost.localdomain localhost4 localhost4.localdomain4
::1 ip6-localhost ip6-loopback localhost6 localhost6.localdomain6

0.0.0.0 ads.google.com
0.0.0.0 www.ads.google.com
0.0.0.0 adservice.google.com
0.0.0.0 adservice.google.ba
0.0.0.0 adwords.l.google.com
0.0.0.0 id.google.com
0.0.0.0 google-analytics.com
0.0.0.0 analytics.corp.google.com
0.0.0.0 ssl.google-analytics.com
0.0.0.0 ssl-google-analytics.l.google.com
0.0.0.0 googleads.g.doubleclick.com
0.0.0.0 googleads.g.doubleclick.net
0.0.0.0 googleads2.g.doubleclick.net
0.0.0.0 googleads4.g.doubleclick.net
0.0.0.0 googleadservices.com
0.0.0.0 gstaticadssl.l.google.com
0.0.0.0 pagead-tpc.l.google.com
0.0.0.0 pagead.l.google.com
0.0.0.0 googleadservices.com
0.0.0.0 partner.googleadservices.com
0.0.0.0 partnerad.l.google.com
0.0.0.0 partnerssl.googleadservices.com
0.0.0.0 pagead2.googleadservices.com
0.0.0.0 pagead2.googlesyndication.com
0.0.0.0 pagead46.googlesyndication.com
0.0.0.0 ogs.google.com
0.0.0.0 s0-2mdn-net.l.google.com
0.0.0.0 tpc.googlesyndication.com
0.0.0.0 tpcnc.googlesyndication.com
0.0.0.0 video-ad-stats.googlesyndication.com
0.0.0.0 www.google-analytics.com
0.0.0.0 www.googleadservices.com
0.0.0.0 www.googletagmanager.com
0.0.0.0 www.googletagservices.com
0.0.0.0 www-google-analytics.l.google.com
0.0.0.0 www-googletagmanager.l.google.com
0.0.0.0 10.afs.googleadservices.com
0.0.0.0 18.afs.googleadservices.com
0.0.0.0 2.afs.googleadservices.com
0.0.0.0 4.afs.googleadservices.com
0.0.0.0 ad-creatives-public.commondatastorage.googleapis.com
0.0.0.0 ampcid.google.com
0.0.0.0 cdn.googletoolservices.com

{% endcode %}

Gotove hosts fajlove, friško generirane lagano možete pronaći pretragom na bilo kojem pretraživaču, te možete također generirati iz izvora koje preferirate:

## Generisanje hosts fajla iz više izvora

Pregled skripte koja skuplja domene sa više listi koje možete pronaći kod raznih provajdera, i ukratko ovo su koraci koje pravi kako bi došli do ažuriranog hosts fajla:

1. `wget` (preuzima) zadane liste
2. `sort` `uniq` `pcgrep` sortira i sređuje podatke prema zadanim parametrima
3. `cp` pravi backup trenutnog hosts
4. `cp` kopira generirani hosts na mijesto glavnog hosts fajla

Skripta je **samo** za unix / linux operativne sisteme, i potrebno je da već imate instalisane slijedeće pakete:

`wget pcre (or pcre2 or pcregrep) perl p7zip p7zip-plugins (or p7zip-full)`

Preuzmi skriptu: [update_hosts](https://static.monkeyshub.space/soul/block/update_hosts.txt), promjeni putanje sa stvarnim, na svojoj mašini i preimenuj u `.sh`.
U ovoj skripti kao primjer izvori su hostani na monkeyshub.space i skripta se pokreće iz i sređuje u `~/Projects/scripts/`.

Također, napravi folder `/BLT/parsing` (ili po volji, samo ne zaboravi promijeniti putanju u skripti) i tu ubaci helpere:

- [hostpatterns.dat](https://static.monkeyshub.space/soul/block/BLT/parsing/hostpatterns.dat)
- [novihosts-template.txt](https://static.monkeyshub.space/soul/block/BLT/parsing/novihosts-template.txt)
- [tld-filter.dat](https://static.monkeyshub.space/soul/block/BLT/parsing/tld-filter.dat)

Skripta po završetku pravi novi folder `/BLT/hosts` u koji kopira backup i novi generirani hosts fajl: `hostsbackup.txt` i `novihosts.txt`.

## Domain names u IP adrese

**Korisno za IP firewall rules = iptables, odnosno i za mikrotik firewall address-list**

Primjer dodavanja u iptables:
{% code lang:bash iptables>add %}
iptables -A INPUT -s 172.217.19.110 -j DROP
service iptables save
{% endcode %}
Primjer brisanja iz iptables:
{% code lang:bash iptables>delete %}
iptables -D INPUT -s 172.217.19.110 -j DROP
service iptables save
{% endcode %}

Koristite jedan od dns klijenata (`dig`,`whois`... `dns.google.com`... ) kako bi došli do određenih IP adresa, vama bitnih za blokiranje web stranica, a gotove liste potražite kod omiljenog provajdera, npr feodotracker:

[feodotracker ipblocklist.txt](https://feodotracker.abuse.ch/downloads/ipblocklist.txt)

ja lično koristim ovu:

[iplist.txt](https://static.monkeyshub.space/soul/block/iplist.txt)

## Generisanje liste IP adresa u Mikrotik address-list

### Update liste

Skripta koja održava `droplist.rsc` = briše stare, dodaje nove linije IP adresa koristeći željenu ip listu u droplist listu adresa:

{% code lang:bash fedoracker https://feodotracker.abuse.ch/downloads/ipblocklist.txt ipblocklist.txt %}

#! /bin/bash
WORK_DIR=$(cd $(dirname $0); pwd);

if [ ! -d "$WORK_DIR/tmp" ];then
  mkdir $WORK_DIR/tmp
fi

curl -s https://feodotracker.abuse.ch/downloads/ipblocklist.txt && \
cat > $WORK_DIR/droplist.rsc << EOF
/log info "Importuj IPv4 CIDR list..."
/ip firewall address-list remove [/ip firewall address-list find list=droplist]
/ip firewall address-list
EOF
cat $WORK_DIR/droplist.txt | awk '{ printf(":do {add address=%s
list=droplist} on-error={}\n",$0) }' >> $WORK_DIR/droplist.rsc && \
cat >> $WORK_DIR/droplist.rsc << EOF

#   za IPv6 listu:
#   :if ([:len [/system package find where name="ipv6" and disabled=no]] > 0) do={
#   /log info "Importuj IPv6 CIDR list..."
#   /ipv6 firewall address-list remove [/ipv6 firewall address-list find list=droplist]
#   /ipv6 firewall address-list
#   EOF
#   cat $WORK_DIR/iplevel6.txt | awk '{ printf(":do {add address=%s
list=droplist} on-error={}\n",$0) }' >>
#   $WORK_DIR/droplist.rsc && \
#   echo "}" >> $WORK_DIR/droplist.rsc
EOF

{% endcode %}

### Update firewall address-list

Izgled generirane `droplist.rsc`:

{% code lang:bash droplist.rsc %}
/log info "Import cn IPv4 CIDR list..."
/ip firewall address-list remove [/ip firewall address-list find list=droplist]
/ip firewall address-list
:do {add address=93.189.41.213 list=droplist} on-error={}
:do {add address=185.164.32.204 list=droplist} on-error={}
:do {add address=78.189.111.208 list=droplist} on-error={}
:do {add address=212.156.133.218 list=droplist} on-error={}
#...
{% endcode %}

U mikrotik ruteru, od `droplist.rsc` napravimo skriptu i pokrenemo:

{% code lang:bash system/script> remove && add && run %}
/system script \
add name=updateDROP source="/log info "Import IPv4 CIDR list..."
/ip firewall address-list remove [/ip firewall address-list find list=droplist]
/ip firewall address-list
:do {add address=93.189.41.213 list=droplist} on-error={}
:do {add address=185.164.32.204 list=droplist} on-error={}
:do {add address=78.189.111.208 list=droplist} on-error={}
:do {add address=212.156.133.218 list=droplist} on-error={}
..."
run updateDROP
{% endcode %}

ili opcija iz firewall-a, uklonimo stare i dodamo nove adrese u listu preko `/ip firewall`:

{% code lang:bash /ip>firewall>address-list %}
/ip firewall address-list \
remove [/ip firewall address-list find list=droplist]
# zatim kopiramo i zaljepimo linije linije [ENTER] razmaknute:
:do {add address=93.189.41.213 list=droplist} on-error={}
:do {add address=185.164.32.204 list=droplist} on-error={}
:do {add address=78.189.111.208 list=droplist} on-error={}
...
{% endcode %}

ili `/import`:
{% code lang:bash /import>file-name %}
/import file-name=droplist.rsc from-line=2
{% endcode %}

Za prve dvije opcije potrebno je srediti, obrisati sve linije iz iplist.txt koje ne sadrže ip adresu. Kod `/import` možemo odrediti početnu liniju, ali opet treba izbrisati ako slučajno ima komentara na dnu fajla.

`/tool fetch` je koristan ako smo uploadovali droplist.rsc na neki server.

{% label success@Ufff %} - Sretno `blokanje` && Ćao.
