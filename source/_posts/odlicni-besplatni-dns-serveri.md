---
title: Javni DNS serveri
date: 2020-07-19 01:03:00
updated: 2020-05-19 01:03:00
categories:
  - Research
tags:
  - dns
  - opensource
---

# Lista Javnih DNS servera

_Lista najboljih potpuno besplatnih DNS servera, plus upute kako ih koristiti._

{% note success %}
Zadnja izmjena: **Juli 2020**
Serveri: | **Quad9**| **OpenDNS** | **Cloudflare** | **CleanBrowsing** | **AdGuard DNS** | ...
{% endnote %}

 <!-- more -->

{% label info@Popis dodatnih besplatnih DNS servera, kao i korisni linkovi se nalaze pri dnu stranice. %}

**Šta je DNS server**?

[DNS serveri](https://en.wikipedia.org/wiki/Name_server) prevode nama prisan naziv domene stranice koji upisujemo u browser, _npr: google.com_, u [javnu IP adresu](https://www.lifewire.com/what-is-a-public-ip-address-2625974) koja je zapravo potrebna kako bi uređaj koji koristite za surfanje komunicirao sa stranicom kojoj pristupate.

Vaš [ISP](https://www.lifewire.com/internet-service-provider-isp-2625924) automatski određuje DNS servere ali niste obavezni da koristite iste, postoji mnogo razloga zašto bi koristili neke druge {%  label info@imate problema sa trenutnim, želite bolju performansu, ne želite tragove vaših aktivnosti, želite blokirati određene stranice %}, {% label success@privatnost i brzina %} su ono što ponajprije uočite promjenom.

Primarni DNS serveri (_preferirani_) i sekundarni DNS serveri (_alternativni_) se mogu kombinovati od više provajdera.

## Google

![dns.google](https://static.monkeyshub.space/fragments/sky/dns1.png)

[Google Public DNS](https://developers.google.com/speed/public-dns/) nudi: brže surfanje, poboljšanu sigurnost, i precizne rezultate bez preusmjeravanja.

- IPv4:
- Primarni DNS: `8.8.8.8`
- Sekundarni DNS: `8.8.4.4`
- IPv6:
- Primarni DNS: `2001:4860:4860::8888`
- Sekundarni DNS: `2001:4860:4860::8844`

Ono što omugućava brzinu korištenjem google DNS su njihovi [široko rasprostranjeni data serveri](https://developers.google.com/speed/public-dns/docs/performance#geography).

{% note success %}
**DoT**

```
kdig -d +noall +answer @dns.google example.com \
+tls-ca +tls-hostname=dns.google # +tls-sni=dns.google
```

{% endnote %}

{% note success %}
**DNS-over-TLS**
`dns.google`
{% endnote %}

{% note success %}
**DNS-over-HTTPS**
`https://dns.google/dns-query`
`https://dns.google/resolve`
{% endnote %}

## Quad9

![quad9.net](https://static.monkeyshub.space/fragments/sky/dns2.png)

[Quad9](https://www.quad9.net/) ima besplatne DNS servere koji vas štite od cyber napada trenutnim i automatskim blokiranjem pristupa nesigurnim web stranicama [bez skladištenja vaših privatnih informacija](https://www.quad9.net/policy/).

- IPv4:
- Primarni DNS: `9.9.9.9`
- Sekundarni DNS: `149.112.112.112`
- IPv6:
- Primarni DNS: `2620:fe::fe`
- Sekundarni DNS: `2620:fe::9`

Quad9 blokira domene koje su zaražene ili sadrže malware. Također, ima i [nesiguran IPv4 javni DNS server](https://www.quad9.net/faq/#Is_there_a_service_that_Quad9_offers_that_does_not_have_the_blocklist_or_other_security) na `9.9.9.10 | 2620:fe::10`.

{% note success %}
**DNSCrypt**
`https://www.quad9.net/quad9-resolvers.toml`
{% endnote %}

{% note success %}
**DNS-over-TLS**
`dns.quad9.net`
{% endnote %}

{% note success %}
**DNS-over-HTTPS**
`https://dns.quad9.net/dns-query`
{% endnote %}

## OpenDNS

![opendns.com](https://static.monkeyshub.space/fragments/sky/dns3.png)

[OpenDNS](https://www.opendns.com/), Cisco, sa 100% pouzdanosti i up-time, te preko 90 miliona korisnika nudi dva seta besplatnih javnih DNS servera:

### OpenDNS Home

{% btn https://signup.opendns.com/homefree/, OpenDNS Home, fa fa-home fa-fw, Home %}

- IPv4:
- Primarni DNS: `208.67.222.222`
- Sekundarni DNS: `208.67.220.220`
- IPv6:
- Primarni DNS: `2620:119:35::35`
- Sekundarni DNS: `2620:119:53::53`

### OpenDNS FamilyShield

{% btn https://www.opendns.com/setupguide/#familyshield, OpenDNS FamilyShield, fa fa-shield-alt fa-fw, Shield %}

- IPv6
- Primarni DNS: `208.67.222.123`
- Sekundarni DNS: `208.67.220.123`

{% note success %}
**DNSCrypt**
`https://www.opendns.com/about/innovations/dnscrypt/#download`
{% endnote %}

{% note info %}
**Tehnologija**
[OpenDNS Innovations](https://www.opendns.com/about/innovations/)
{% endnote %}

## Cloudflare

![1.1.1.1](https://static.monkeyshub.space/fragments/sky/dns4.png)

[Cloudflare](https://cloudflare.com) je izgradio [1.1.1.1](https://1.1.1.1/) sa ciljem da bude:

{% cq %}
fastest DNS service in the world
{% endcq %}

i nikada ne pohranjuje tvoju IP adresu, ne prodaje tvoje podatke niti ih koristi kao mete reklama.

- IPv4:
- Primarni DNS: `1.1.1.1`
- Sekundarni DNS: `1.0.0.1`
- IPv6:
- Primarni DNS: `2606:4700:4700::1111`
- Sekundarni DNS: `2606:4700:4700::1001`

{% note success %}
**DNS-over-Twitter**
[@1111Resolver](https://twitter.com/1111Resolver)
{% endnote %}

{% note success %}
**DNS-over-TLS**
`kdig -d @1.1.1.1 +tls-ca +tls-host=cloudflare-dns.com example.com`
{% endnote %}

{% note success %}
**DNS-over-HTTPS**
`https://cloudflare-dns.com/dns-query`
{% endnote %}

## CleanBrowsing

![cleanbrowsing.org](https://static.monkeyshub.space/fragments/sky/dns5.png)

CleanBrowsing ima [3 besplatna javna DNS servera](https://cleanbrowsing.org/filters):

### Family Filter

Blokira pristup svim pornografskim i eksplicitnim stranicama. Blokira također i proksi i VPN domene koje se koriste za zaobilaženje filtera. Stranice miješanih sadržaja (kao što je Reddit) su također blokirane. Google, Bing i YouTube su postavljene na siguran način. Malware i Phishing domene su blokirane.

- IPv4:
- Primarni DNS: `185.228.168.168`
- Sekundarni DNS: `185.228.169.168`
- IPv6:
- Primarni DNS: `2a0d:2a00:1::`
- Sekundarni DNS: `2a0d:2a00:2::`

### Adult Filter

Blokira pristup svim pornografskim i eksplicitnim stranicama. Ne blokira proksi i VPN domene, niti stranice miješanih sadržaja. Google, Bing i YouTube su postavljene na siguran način. Malware i Phishing domene su blokirane.

- IPv4:
- Primarni DNS: `185.228.168.10`
- Sekundarni DNS: `185.228.169.11`
- IPv6:
- Primarni DNS: `2a0d:2a00:1::1`
- Sekundarni DNS: `2a0d:2a00:2::1`

### Security Filter

Blokira Spam, Malware, Phishing i zlonamjeran sadržaj. Ne blokira sadržaj za odrasle. Update baze se vrši svaki sat vremena i smatra se jednom među najboljim.

- IPv4:
- Primarni DNS: `185.228.168.9`
- Sekundarni DNS: `185.228.169.9`
- IPv6:
- Primarni DNS: `2a0d:2a00:1::2`
- Sekundarni DNS: `2a0d:2a00:2::2`

Security filter {% label info@trenutno koristim %}, u kombinaciji sa cloudflare i {% btn https://dnscrypt.info/, DNSCrypt, fa fa-microchip fa-fw, DNS crypt %}.

<br />

{% note success %}
**DNSCrypt**
Family Filter
`sdns://AQMAAAAAAAAAFDE4NS4yMjguMTY4LjE2ODo4NDQzILysMvrVQ2kXHwgy1gdQJ8MgjO7w6OmflBjcd2Bl1I8pEWNsZWFuYnJvd3Npbmcub3Jn`
Adult Filter
`sdns://AQMAAAAAAAAAEzE4NS4yMjguMTY4LjEwOjg0NDMgvKwy-tVDaRcfCDLWB1AnwyCM7vDo6Z-UGNx3YGXUjykRY2xlYW5icm93c2luZy5vcmc`
{% endnote %}

{% note success %}
**DNS-over-HTTPS**
`https://doh.cleanbrowsing.org/doh/security-filter/`
{% endnote %}

{% note success %}
**DNS-over-TLS**
`security-filter-dns.cleanbrowsing.org`
{% endnote %}

## Alternate DNS

![alternate-dns.com](https://static.monkeyshub.space/fragments/sky/dns9.png)

[Alternate DNS](https://alternate-dns.com/) je besplatni DNS servis koji blokira reklame prije nego dođu to vaše mreže.

- IPv4:
- Primarni DNS: `23.253.163.53`
- Sekundarni DNS: `98.101.242.72`
- IPv6:
- Primarni DNS: `2001:4801:7825:103:be76:4eff:fe10:2e49`
- Sekundarni DNS: `2001:4800:780e:510:a8cf:392e:ff04:8982`

Najavljuju i DoH server.

## Verisign

![verisign.com](https://static.monkeyshub.space/fragments/sky/dns6.png)

[Verisign DNS servisi](https://www.verisign.com/en_US/security-services/public-dns/index.xhtml) se vrte oko stabilnosti i sigurnosti sa 100% up-time, također nude i privatnost, kako kažu:

{% cq %}
will not sell your public DNS data to third parties nor redirect your queries to serve you any ads
{% endcq %}

- IPv4
- Primarni DNS: `64.6.64.6`
- Sekundarni DNS: `64.6.65.6`
- IPv6
- Primarni DNS: `2620:74:1b::1:1`
- Sekundarni DNS: `2620:74:1c::2:2`

## AdGuard DNS

![adguard.com](https://static.monkeyshub.space/fragments/sky/dns10.png)

[AdGuard DNS](https://adguard.com/en/adguard-dns/overview.html) blokira video oglase i oglase u aplikacijama, preglednicima, igrama i na bilo kojoj web stranici i ne pohranjuje zapise DNS upita. Nudi tri opcije:

### Default

- IPv4:
- Primarni DNS: `176.103.130.130`
- Sekundarni DNS: `176.103.130.131`
- IPv6:
- Primarni DNS: `2a00:5a60::ad1:0ff`
- Sekundarni DNS: `2a00:5a60::ad2:0ff`

### Family protection

Pored reklama blokira i stranice sa sadržajem za odrasle.

- IPv4:
- Primarni DNS: `176.103.130.132`
- Sekundarni DNS: `176.103.130.134`
- IPv6:
- Primarni DNS: `2a00:5a60::bad1:0ff`
- Sekundarni DNS: `2a00:5a60::bad2:0ff`

### Non-filtering

Omogućuju sigurnu i stabilnu konekciju ali ne filtriraju sadržaj.

- IPv4:
- Primarni DNS: `176.103.130.136`
- Sekundarni DNS: `176.103.130.137`
- IPv6:
- Primarni DNS: `2a00:5a60::01:ff`
- Sekundarni DNS: `2a00:5a60::02:ff`

{% note success %}
**DNSCrypt**
Default
`sdns://AQIAAAAAAAAAFDE3Ni4xMDMuMTMwLjEzMDo1NDQzINErR_JS3PLCu_iZEIbq95zkSV2LFsigxDIuUso_OQhzIjIuZG5zY3J5cHQuZGVmYXVsdC5uczEuYWRndWFyZC5jb20`
Family protection
`sdns://AQIAAAAAAAAAFDE3Ni4xMDMuMTMwLjEzMjo1NDQzILgxXdexS27jIKRw3C7Wsao5jMnlhvhdRUXWuMm1AFq6ITIuZG5zY3J5cHQuZmFtaWx5Lm5zMS5hZGd1YXJkLmNvbQ`
Non-filtering
`sdns://AQcAAAAAAAAAFDE3Ni4xMDMuMTMwLjEzNjo1NDQzILXoRNa4Oj4-EmjraB--pw3jxfpo29aIFB2_LsBmstr6JTIuZG5zY3J5cHQudW5maWx0ZXJlZC5uczEuYWRndWFyZC5jb20`
{% endnote %}

{% note success %}
**DNS-over-TLS**
Default
`dns.adguard.com`
Family protection
`dns-family.adguard.com`
Non-filtering
`dns-unfiltered.adguard.com`
{% endnote %}

{% note success %}
**DNS-over-HTTPS**
Default:
`https://dns.adguard.com/dns-query`
Family protection:
`https://dns-family.adguard.com/dns-query`
Non-filtering:
`https://dns-unfiltered.adguard.com/dns-query`
{% endnote %}

## Dodatni DNS serveri

**Tablica sa DNS serverima koji, pored gore navedenih, su vrijedni spomena.**

| Provajder                                               |   Primarni DNS | Sekundarni DNS |
| ------------------------------------------------------- | -------------: | -------------: |
| [DNS.WATCH](https://dns.watch)                          |   84.200.69.80 |   84.200.70.40 |
| [Comodo Secure DNS](https://www.comodo.com/secure-dns/) |     8.26.56.26 |    8.20.247.20 |
| [FreeDNS](https://freedns.zone/en/)                     |     45.33.97.5 |   37.235.1.177 |
| [OpenNic](https://www.opennic.org/)                     | 192.71.245.208 |  94.247.43.254 |
| [SafeDNS](https://www.safedns.com)                      |   195.46.39.39 |   195.46.39.40 |

{% label info@OpenNIC ima nekoliko DNS servera. Posjetite njihovu web stranicu. %}

#### Korisni linkovi

<div class="text-centar">
{% btn https://www.dnsperf.com/, &nbsp;DNSpref, fa fa-cogs fa-fw fa-lg, Status DNS servera %}{% btn https://speedtest.net, &nbsp;Speedtest, fa fa-cogs fa-fw fa-lg, Brzina interneta %}{% btn https://www.dnsleaktest.com, &nbsp;DNSleak, fa fa-cogs fa-fw fa-lg, Curenje IP podataka %}{%btn https://dnscrypt.info/, &nbsp;DNSCrypt, fa fa-cogs fa-fw fa-lg, DNS enkripcija \$}
</div>
