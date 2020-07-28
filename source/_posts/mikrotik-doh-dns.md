---
title: DoH DNS proxy na Mikrotik-u
date: 2020-07-20 23:07:00
updated: 2020-07-20 23:07:00
categories:
  - Tutorials
tags:
  - mikrotik
  - script
  - routeros
---

# DNS over HTTPS RouterOS konfiguracija

Sa verzijom **v6.47** dolazi dugo čekana DNS over HTTPS (DoH) integracija u Mikrotik ruterima. DoH inače koristi HTTPS protokol pri slanju i primanju DNS zahtjeva za bolju data integraciju i eliminira "man in the middle attacks" (MITM). Nije kompatibilan sa FWD statičkim unosima.

<!--more-->

Minimalno tri informacije su nam potrebne za konfiguraciju:

- root CA certifikat DOH servera
- hostname DoH servera
- jedan DNS server

### Kako do CA certifikata

Do certifikata možemo doći na više načina, ja koristim Firefox: {%  label info@Page Info > Security > View Certificate  %}, gdje možete direktno skinuti, snimiti certifikat koji uploadujete u ruter preko ftp ili winbox / webfig: _file>upload_.

![certifikat](https://static.monkeyshub.space/fragments/sky/dns12.png)

Ako koristite openssl do certifikata možete doći ovako:

{% codeblock lang:bash openssl %}
openssl s_client -showcerts -servername server -connect server:443 > cacert.pem
{% endcodeblock %}

### Import root CA certifikata u ruter:

{%  note  danger %}
**Uvijek** koristite web stranicu izdavača certifikata da `fetch` ?skinete root CA certifikat, u ovom primjeru to je [Let's Encrypt](https://letsencrypt.org/certs/)
{%  endnote %}

{% codeblock tools>fetch https://wiki.mikrotik.com/wiki/Manual:Tools/Fetch Manual:Tools/Fetch %}
/tool fetch url="https://letsencrypt.org/certs/lets-encrypt-x3-cross-signed.pem"
/certificate import file-name=lets-encrypt-x3-cross-signed.pem
{% endcodeblock %}

{%  label info@OpenDNS kao primjer  %}

### Konfiguaracija DoH servera:

{% codeblock ip>dns https://wiki.mikrotik.com/wiki/Manual:IP/DNS Manual:IP/DNS %}
/ip dns set use-doh-server=https://doh.opendns.com/dns-query verify-doh-cert=yes
{% endcodeblock %}

I potrebno je imati barem jedan regularan DNS podešen za `resolve` samog DoH hostname-a (osnove DNS-a ❤). Ako već niste imali podešen dinamički ili statički dns možete koristiti IP od istog provajdera:

#### Kako postaviti dinamički dns server

{% codeblock ip>dns https://wiki.mikrotik.com/wiki/Manual:IP/DNS Manual:IP/DNS %}
/ip dns set servers=208.67.222.222
{% endcodeblock %}

#### Kako postaviti statički dns server

{% codeblock ip>dns>static https://wiki.mikrotik.com/wiki/Manual:IP/DNS Manual:IP/DNS %}
/ip dns static add address=208.67.222.222 name=opendns.com disabled=no
{% endcodeblock %}

### Debug

#### Ispis grešaka u log

{%  codeblock system>logging %}
/system logging
add topics=dns prefix=DNS404 action=memory
{%  endcodeblock  %}

#### Praćenje loga

{%  codeblock log> %}
/log
print follow where topics~"dns"

# ~ dodajte i error u topics za praćenje samo grešaka

{%  endcodeblock  %}

#### Riješenja za par grešaka

`connection error: SSL: handshake failed`
{%  note warning %}
_certifikat ili nije importovan ili je neispravan_
{%  endnote  %}
`connection error: resolving error`
{%  note warning %}
_DoH server nije upisan valjano_ -> **https://url.**
{%  endnote  %}

#### Torch

Pokrenite Torch na WAN-u i gledajte da **nema** `UDP` i `TCP` konekcija na **port 53**.

### Dodatak

DNS serveri se mogu miksati od različih provajdera. Listu besplatnih javnih DNS servera možete naći u članku:

<p>
{% post_link odlicni-besplatni-dns-serveri Lista besplatnih DNS serevera %} <span role="img" aria-label="link">✈</span>
</p>

`/ip dns cache flush` za čišćenje postojećeg keša ⚡. Potrebno je i par minuta pričekati kako bi nove DNS postavke prodisale, i naravno vrijeme na ruteru treba da je usklađeno:
`/system ntp client set enabled=yes server-dns-names=time.cloudflare.com`.

#### Cloudflare + Cleanbrowsing:

{%  codeblock ip>dns>export>verbose %}
/ip dns set
allow-remote-requests=yes cache-max-ttl=2d cache-size=8192KiB \
 max-concurrent-queries=100 max-concurrent-tcp-sessions=20 max-udp-packet-size=4096 \
 query-server-timeout=2s query-total-timeout=10s servers="185.228.168.168" use-doh-server=\
 https://doh.cleanbrowsing.org/doh/security-filter/ verify-doh-cert=yes
/ip dns static
add address=104.16.248.249 disabled=no name=cloudflare-dns.com ttl=1d type=A
{%  endcodeblock  %}

#### Cloudflare Solo

{%  codeblock ip>dns>export>verbose %}
/ip dns
set allow-remote-requests=yes cache-max-ttl=1d cache-size=8192KiB max-concurrent-queries=100 max-concurrent-tcp-sessions=20 \
 max-udp-packet-size=4096 query-server-timeout=3s query-total-timeout=13s servers="" use-doh-server=\
 https://cloudflare-dns.com/dns-query verify-doh-cert=yes
/ip dns static
add address=104.16.248.249 disabled=no name=cloudflare-dns.com ttl=1d type=A
add address=104.16.249.249 disabled=no name=cloudflare-dns.com ttl=1d type=A
{%  endcodeblock  %}

#### Provjera konekcije na Cloudflare

```
https://1.1.1.1/help
```

Dokaz da {% label success@solucija sa Cloudflare radi! %}
![Dokaz da ova gore solucija radi!](https://static.monkeyshub.space/fragments/sky/cetirijedinicedoh.png)

#### DNS leak test

```
www.dnsleaktest.com
```

#### CA certifikati ekstraktovani iz Mozilla

```
curl https://curl.haxx.se/cacacert.pem > cacert.pem
```
