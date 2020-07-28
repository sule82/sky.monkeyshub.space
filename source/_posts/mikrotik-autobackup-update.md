---
title: Auto Backup & Update sa Mikrotik ruterom
date: 2020-05-09 23:07:00
updated: 2020-05-09 23:07:00
categories:
  - Tutorials
tags:
  - mikrotik
  - script
  - routeros
---

# Namjena

Skripta je namjenjena za dnevni, ili po izboru sedmični, mjesečni itd, backup rutera na e-mail. Također možete postaviti redovnu provjeru i automatski update ili dojavu o dostupnoj novoj verziji mailom.

- opcije: samo backup, provjera verzije, update & backup
- update kanal po izboru
- instalacija samo patch na postojeću verziju (ako ograničite update na xx.xx.patch)
- osnovne informacije o ruteru
- sigurnosni triger: update neće biti pokrenut ako nije uspjelo slanje backup-a na mail
- firmware upgrade (kraljevski 🐵)

<!--more-->

# Opcije skripte

**Backup samo** - Kreira sistem i backup konfiguracije te šalje na mail u atachmentu. Koristi mail za skladištenje backup fajlova.

**Backup & dojave o novim RouterOS izdanjima** - Uključuje gore navedeno, također provjerava nova izdanja RouterOS firmware i šalje informacije na mail.

**Backup & automatski RouterOS upgrade** - Gore dvoje navedeno, i ako je novi firmware dostupan, sskripta pokreće upgrade proces. Na kraju dobijate dva mail-a. Prvi sa sistem backup i prethodnom RouterOS verzijom, drugi šalje po završetku upgrade (prikačen backup sa update sistemom).

# Kako se koristi

## 1. Konfiguracija parametara

Skriptu kopirajte i po želji podesite parametre na vrhu. U slučaju da naletite na poteškoće, kontaktirajte me, slobodno.

{% note warning %}
Naziv skripte treba biti "BackUP"
Ne zaboravite unjeti ispravnu e-mail adresu i obratite pažnju na `scriptMode` varijable.
{% endnote %}

```
# Skripta: BackUP                                    ++++++++#
###                   _                 _       _           +#
#+      _____ ___ ___| |_ ___ _ _ ___  | |_ _ _| |_         +#
#+     |     | . |   | '_| -_| | |_ -| |   | | | . |        +#
#+     |_|_|_|___|_|_|_,_|___|_  |___|o|_|_|___|___|        +#
#+                           |___|                          +#
###                                                        ###
#---INFORMACIJE---Mikrotik RouterOS automatic backup & update#
@
# Version: 2.0.3_ba
# Created: 07/08/2018
# Author:  Alexander Tebiev
# Modified & Translated to Bosnian: Adnan Sulejmanovic
# Mod & Translate by: Adnan Sulejmanovic
# I never translate, or use in new ones, script functions, calls to action, variables and such
# in other language exept english, that's stupid and lame for multiple reasons, also in most
# cases avoiding č, ć, ž, š, đ symbols.
#
# Napomena!
# Minimalna RouterOS verzija za korištenje ove skripte je v6.43.7
#
# Info za početnike:
# linije koje na početku imaju tarabu (#) ne koriste se u skripti, isključene su.
#
#----------OVAJ DIO UREDITE PO SVOJIM POTREBAMA------------------
## Notifikacijski e-mail
## (Obavezno uredite - postavite e-mail na ruteru: Tools -> Email)
:local emailAddress "sky@monkeyshub.space";

## Script mode, moguće varijante: backup, osupdate, osnotify.
# backup 	- 	Samo backup (defaultna opcija, ako ne postavite drugačije)
#
# osupdate 	- 	Instalira novi RouterOS ako je dostupan.
#     !uključite `forceBackup` ako želite backup svaki put kada je skripta pokrenuta
#
# osnotify 	- 	Šalje samo email notifikaciju (bez backup-a) ako je novi RouterOS dostupan.
#		 	!uključite 'forceBackup` ako želite backup svaki put kada je skripta pokrenuta
:local scriptMode "backup";

## Dodatni parametar ako ste postavili `scriptMode` na `osupdate` ili `osnotify`
# `true` za backup bez obzira koja varijanta skripte je pokrenuta, `false` je defaultno.
:local forceBackup false;

## Backup enkripcija, neće biti zaštićen enkripcijom ako ne unesete i podesite šifru.
:local backupPassword ""

## Ako slijedeće postavite na `true`, šifra će biti unutar izvezene konfiguracije.
:local sensetiveDataInConfig false;

## Update kanal. Izaberite preferirani od mnogih koje Mikrotik nudi:
## stable, long-term, testing, development
:local updateChannel "stable";

## Instalacija samo patch-a na postojeću verziju.
## scriptMode mora biti podešen na "osupdate"
## Update će biti instalisan samo ako su MAJOR i MINOR vezijski brojevi isti kao kod
## trenutno instalisane verzije RouterOS. npr: v6.44.1 => major.minor.PATCH
:local installOnlyPatchUpdates	false;

##-------------------------------------------------------------------------------------##
#    !!!! NE MJENJAJTE NIŠTA ISPOD OVE LINIJE, OSIM AKO ZNATE ŠTA RADITE 😬 !!!!      #
##-------------------------------------------------------------------------------------##

#Prefiks poruke
:local SMP "AutoBackUP:"

:log info "\r\n$SMP script \"Mikrotik RouterOS automatski backup & update\" je u toku.";
:log info "$SMP Script Mode: $scriptMode, forceBackup: $forceBackup";

#Provjera konfiguracije e-maila
:if ([:len $emailAddress] = 0 or [:len [/tool e-mail get address]] = 0 or [:len [/tool e-mail get from]] = 0) do={
	:log error ("$SMP Email konfiguracija NIJE OK, provjeri u Tools -> Email. Skripta se zaustavlja.");
	:error "$SMP Greska u postavci!";
}

#Provjera da li je ispravan indetifikacijski naziv rutera ok
if ([:len [/system identity get name]] = 0 or [/system identity get name] = "MikroTik") do={
	:log warning ("$SMP Promjeni identitet rutera, idi na (System -> Identity), pazite da bude sto kraci i Vama znacajan.");
};

######################################################################!GLOBALNI DIO!-1###
# Funkcija konvertuje standardne mikrotik build verzije u broj.
# Mogući argumenti: paramOsVer
# :put [$buGlobalFuncGetOsVerNum paramOsVer=[/system routerboard get current-RouterOS]];
:global buGlobalFuncGetOsVerNum do={
	:local osVer $paramOsVer;
	:local osVerNum;
	:local osVerMicroPart;
	:local zro 0;
	:local tmp;

	# Mijenja `beta` sa tačkom
	:local isBetaPos [:tonum [:find $osVer "beta" 0]];
	:if ($isBetaPos > 1) do={
		:set osVer ([:pick $osVer 0 $isBetaPos] . "." . [:pick $osVer ($isBetaPos + 4) [:len $osVer]]);
	}

	:local dotPos1 [:find $osVer "." 0];

	:if ($dotPos1 > 0) do={
		# AA
		:set osVerNum  [:pick $osVer 0 $dotPos1];
		:local dotPos2 [:find $osVer "." $dotPos1];
```

Konfigurisanu skriptu ubacite u ruter:

```
/system script add source="/vasa-pocetna kategorija skripte \
add nesto name="nesto" da-radi-nesto="no" verbose 🐈
🕖 🕗 🕘 🕙 🕚
"
```

## Email postavke

`/tools e-mail`

Postavite email server parametre. Ako nemate mail server, koristite [smtp2go.com](https://smtp2go.com "smtp2go.com") servis, dopušteno slanje 1000 besplatnih mailova mjesečno, isprobao - dobro funkcioniše.

![smtp2go](/https://static.monkeyshub.space/fragments/sky/backup-2.png)

Provjerite postavke, pošaljite test mail iz terminala sljedećom komandom, izmjenite to, subject & body unutar "" po želji:

```
/tool e-mail send to="tvojMail@tvojDomen.com" subject="backup & update test" body="Ah gle, radi ovo!";
```

Test:

![banana test](/https://static.monkeyshub.space/fragments/sky/backup-4.png)

Ako je sve ok, stići će vam mail:

![banana test ok](/https://static.monkeyshub.space/fragments/sky/backup-5.png)

## Kako kreirati sheduled zadatak

Otvorite Sheduler:

```
System -> Scheduler [Add]
```

Unesite željene opcije:

```
Name: `Backup & Update`
Start Time: `01:01:00`
Interval: `1d 00:00:00`
On Event: `/system script run BackUP;`
```

# Testirajte skriptu

Kada sve odradite, testirajte kako bi se uvjerili da je sve dobro podešeno.

![update](https://static.monkeyshub.space/https://static.monkeyshub.space/fragments/skys/sky/backup-1.png)

Otvorite New Terminal i Log prozorčić u WinBox-u, i pokrenite skriptu manuelno komandom:

```rsc
system script run BackUP;
```

Vidjeti ćete radni proces skripte u log-u. Ako skripta završi bez greške, provjerite email, tu je svježa nova poruka sa backup fajlovima sa vašeg MikroTik rutera.

Primjer notifikacije:
![notifikacija](https://static.monkeyshub.space/https://static.monkeyshub.space/fragments/skys/sky/backup-6.png)

Primjer manuelnog pokretanja sa update opcijom:
![update](https://static.monkeyshub.space/https://static.monkeyshub.space/fragments/skys/sky/backup-7.png)

Primjer email-a pri završetku update-a:
![update](https://static.monkeyshub.space/https://static.monkeyshub.space/fragments/skys/sky/backup-8.png)

Provjera RouterOS firmware upgrade-a:
![fw upgrade](https://static.monkeyshub.space/https://static.monkeyshub.space/fragments/skys/sky/backup-9.png)

{% note info %}
_slike gore su uzete pri testiranju i doradi skripte_
{% endnote %}

Naravno, ako zatreba pomoć, ili ako želite na naš jezik komentiranu skriptu tu sam.
