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

![info](/fragments/images/backup-1.png)

Skripta je namjenjena za dnevni, ili po izboru sedmični, mjesečni itd, backup rutera na e-mail. Također možete postaviti redovnu provjeru i automatski update ili dojavu o dostupnoj novoj verziji mailom.

- opcije: samo backup, provjera verzije, update & backup
- update kanal po izboru
- instalacija samo patch na postojeću verziju (ako ograničite update na xx.xx.patch)
- osnovne informacije o ruteru
- sigurnosni triger: update neće biti pokrenut ako nije uspjelo slanje backup-a na mail
- firmware upgrade (kraljevski završetak 🐵)

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

```javascript

# Skripta: BackUP (ne mjenjajte naziv skripte!)
# Minimalna RouterOS verzija za korištenje ove skripte je v6.43.7
#
#
################################################ OVAJ DIO UREDITE PO SVOJIM POTREBAMA ###
## Notifikacijski e-mail (postavite e-mail-a i na ruteru: Tools -> Email)
:local emailAddress "sky@monkeyshub.space";
## Modovi, moguće varijante: backup, osupdate, osnotify.
# backup 	- 	samo backup (defaultna opcija).
# osupdate 	- 	instalira novi RouterOS ako je dostupan.
# osnotify 	- 	Šalje samo email notifikaciju (bez backup-a) ako je novi RouterOS dostupan.
#	!'forceBackup` postavite na `true` ako želite backup svaki put kada je skripta pokrenuta
:local scriptMode "backup";
:local forceBackup false;
## Zaštita, ako želite
:local backupPassword ""
:local sensetiveDataInConfig false;
## MT update kanal.
:local updateChannel "stable";
## Instalacija samo patch-a na postojeću verziju.
## !samo ako je scriptMode podešen na "osupdate"
## Update će biti instalisan samo ako je patch brojčaba vrijednost verzije veća od trenutne
:local installOnlyPatchUpdates	false;

############!!!! NE RADI IZMJENE ISPOD OVE LINIJE, OSIM AKO ZNAŠ ŠTA RADIŠ 😬 !!!! ######
##--------------------------------------------------------------------------------------##

:local SMP "AutoBackUP:"
:log info "\r\n$SMP script \"Mikrotik RouterOS automatski backup & update\" je u toku.";
:log info "$SMP Script Mode: $scriptMode, forceBackup: $forceBackup";
:if ([:len $emailAddress] = 0 or [:len [/tool e-mail get address]] = 0 or [:len [/tool e-mail get from]] = 0) do={
	:log error ("$SMP Email konfiguracija NIJE OK, provjeri u Tools -> Email. Skripta se zaustavlja.");
	:error "$SMP Greska u postavci!";
}
if ([:len [/system identity get name]] = 0 or [/system identity get name] = "MikroTik") do={
	:log warning ("$SMP Promjeni identitet rutera, idi na (System -> Identity), pazite da bude sto kraci i Vama znacajan.");
};

:global buGlobalFuncGetOsVerNum do={
	:local osVer $paramOsVer;
	:local osVerNum;
	:local osVerMicroPart;
	:local zro 0;
	:local tmp;
	:local isBetaPos [:tonum [:find $osVer "beta" 0]];
	:if ($isBetaPos > 1) do={
		:set osVer ([:pick $osVer 0 $isBetaPos] . "." . [:pick $osVer ($isBetaPos + 4) [:len $osVer]]);
	}
	:local dotPos1 [:find $osVer "." 0];
	:if ($dotPos1 > 0) do={
		:set osVerNum  [:pick $osVer 0 $dotPos1];
		:local dotPos2 [:find $osVer "." $dotPos1];
		:if ([:len $dotPos2] = 0) 	do={:set tmp [:pick $osVer ($dotPos1+1) [:len $osVer]];}
		:if ($dotPos2 > 0) 			do={:set tmp [:pick $osVer ($dotPos1+1) $dotPos2];}
		:if ([:len $tmp] = 1) 	do={:set osVerNum "$osVerNum$zro$tmp";}
		:if ([:len $tmp] = 2) 	do={:set osVerNum "$osVerNum$tmp";}
		:if ($dotPos2 > 0) do={
			:set tmp [:pick $osVer ($dotPos2+1) [:len $osVer]];
			:if ([:len $tmp] = 1) do={:set osVerNum "$osVerNum$zro$tmp";}
			:if ([:len $tmp] = 2) do={:set osVerNum "$osVerNum$tmp";}
		} else={
			:set osVerNum "$osVerNum$zro$zro";
		}
	} else={
		:set osVerNum "$osVer$zro$zro$zro$zro";
	}
	:return $osVerNum;
}

:global buGlobalFuncCreateBackups do={
	:log info ("$SMP globalna funkcija \"buGlobalFuncCreateBackups\" je ispaljena.");
	:local backupFileSys "$backupName.backup";
	:local backupFileConfig "$backupName.rsc";
	:local backupNames {$backupFileSys;$backupFileConfig};
	:if ([:len $backupPassword] = 0) do={
		/system backup save dont-encrypt=yes name=$backupName;
	} else={
		/system backup save password=$backupPassword name=$backupName;
	}
	:log info ("$SMP sistemski backup je kreiran. $backupFileSys");
	:if ($sensetiveDataInConfig = true) do={
		/export compact file=$backupName;
	} else={
		/export compact hide-sensitive file=$backupName;
	}
	:log info ("$SMP konfiguracijski fajl je kreiran i izvezen. $backupFileConfig");
	:delay 7s;
	:return $backupNames;
}

:global buGlobalVarUpdateStep;
:local dateTime ([:pick [/system clock get date] 7 11] . [:pick [/system clock get date] 0 3] . [:pick [/system clock get date] 4 6] . "-" . [:pick [/system clock get time] 0 2] . [:pick [/system clock get time] 3 5] . [:pick [/system clock get time] 6 8]);
:local deviceOsVerInst          [/system package update get installed-version];
:local deviceOsVerInstNum       [$buGlobalFuncGetOsVerNum paramOsVer=$deviceOsVerInst];
:local deviceOsVerAvail         "";
:local deviceOsVerAvailNum      0;
:local deviceRbModel            [/system routerboard get model];
:local deviceRbSerialNumber     [/system routerboard get serial-number];
:local deviceRbCurrentFw        [/system routerboard get current-firmware];
:local deviceRbUpgradeFw        [/system routerboard get upgrade-firmware];
:local deviceIdentityName       [/system identity get name];
:local deviceIdentityNameShort  [:pick $deviceIdentityName 0 18]
:local deviceUpdateChannel      [/system package update get channel];
:local isOsUpdateAvailable  false;
:local isOsNeedsToBeUpdated false;
:local isSendEmailRequired	true;
:local mailSubject    "$SMP ruter - $deviceIdentityNameShort.";
:local mailBody       "";
:local mailBodyDeviceInfo	"\r\nPozdrav, \r\n\r\nRuter info: \r\nNaziv:  $deviceIdentityName \r\nModel:  $deviceRbModel  \r\nSerijski broj:  $deviceRbSerialNumber \r\nRouterOS verzija: $deviceOsVerInst  ($[/system package update get channel]) $[/system resource get build-time] \r\nTrenutni firmware: $deviceRbCurrentFw  \r\nRuter uptime: $[/system resource get uptime]";  :local mailBodyCopyright  "\r\n\r\nRouterOS $deviceIdentityName automatski backUP \r\nhttps://sky.monkeyshub.space";
:local changelogUrl ("Izmjene sa novom verzijom;  RouterOS changelog: https://mikrotik.com/download/changelogs/"  . $updateChannel  . "-release-tree");
:local backupName "$deviceIdentityName.$deviceRbModel.$dateTime";
:local backupNameBeforeUpd	"backup_staraverzija_$backupName";
:local backupNameAfterUpd	"backup_novaverzija_$backupName";
:local backupNameFinal		$backupName;
:local mailAttachments		[:toarray ""];
:local updateStep $buGlobalVarUpdateStep;
:do {/system script environment remove buGlobalVarUpdateStep;} on-error={}
:if ([:len $updateStep] = 0) do={
	:set updateStep 1;
}

:if ($updateStep = 1) do={
	:log info ("$SMP kreiram backup... provjeravam verziju... provjeri e-mail za par sekundi.");
	if ($scriptMode = "osupdate" or $scriptMode = "osnotify") do={
		log info ("$SMP trenutno instalisana RouterOS verzija je: $deviceOsVerInst -> provjeravam koje su nam dostupne...");
		/system package update set channel=$updateChannel;
		/system package update check-for-updates;
		:delay 7s;
		:set deviceOsVerAvail [/system package update get latest-version];
		:if ([:len $deviceOsVerAvail] = 0) do={
			:log warning ("$SMP imamo problema pri dohvatanju informacija o dostupnim RouterOS sa servera.");
			:set mailSubject	($mailSubject . "Nismo dohvatili nove RouterOS podatke!")
			:set mailBody 		($mailBody . "Trenutno smo u problemu. \r\nNemamo stabilnu konekciju sa Mikrotik serverom! \r\nPregledaj log za vise informacija.")
		} else={
			:set deviceOsVerAvailNum [$buGlobalFuncGetOsVerNum paramOsVer=$deviceOsVerAvail];
			:if ($deviceOsVerAvailNum > $deviceOsVerInstNum) do={
				:set isOsUpdateAvailable true;
				:log info ("$SMP nova verzija RouterOS-a je dostupna: $deviceOsVerAvail");
			} else={
				:set isSendEmailRequired false;
				:log info ("$SMP sistem nam je update-ovan, imamo najnoviji OS, fw ok... ma svaka cast sefe.");
				:set mailSubject ($mailSubject . " nema novih OS update-a.");
				:set mailBody 	 ($mailBody . "Sistem na ruteru je up to date, sa najnovijom RouterOS verzijom.");
			}
		};
	} else={
		:set scriptMode "backup";
	};

	if ($forceBackup = true) do={
		:set isSendEmailRequired true;
	}
	if ($isOsUpdateAvailable = true and $isSendEmailRequired = true) do={
		if ($scriptMode = "osnotify") do={
			:set mailSubject 	($mailSubject . " Dostupna je nova verzija RouterOS-a! v.$deviceOsVerAvail.")
			:set mailBody 		($mailBody . "Nova RouterOS verzija dostupna za instalaciju je: v.$deviceOsVerAvail ($updateChannel) \r\n$changelogUrl")
		}
		if ($scriptMode = "osupdate") do={
			:set isOsNeedsToBeUpdated true;
			:if ($installOnlyPatchUpdates = true) do={
				:if ([:pick $deviceOsVerInstNum 0 ([:len $deviceOsVerInstNum]-2)] = [:pick $deviceOsVerAvailNum 0 ([:len $deviceOsVerAvailNum]-2)]) do={
					:log info ("$SMP Novi patch na verziju RouterOS firmware-a je dostupan.");
				} else={
					:log info ("$SMP Nova verzija RouterOS firmware-a je dostupna. Potrebno je rucno pokrenuti instalaciju.");
					:set mailSubject 	($mailSubject . " Novi RouterOS: v.$deviceOsVerAvail zahtjeva rucnu instalaciju.");
					:set mailBody 		($mailBody . "Nova verzija RouterOS firmware: v.$deviceOsVerAvail ($updateChannel). \r\nYPotrebna rucna instalacija, vise informacija potrazite u log. \r\n$changelogUrl");
					:set isOsNeedsToBeUpdated false;
				}
			}
			if ($isOsNeedsToBeUpdated = true) do={
				:log info ("$SMP nova RouterOS verzija ce biti instalisana! v.$deviceOsVerInst -> v.$deviceOsVerAvail");
				:set mailSubject	($mailSubject . " ⚡ Nova RouterOS verzija ce biti uskoro instalisana! v.$deviceOsVerInst -> v.$deviceOsVerAvail.");
				:set mailBody ($mailBody . "Tvoj Mikrotik ruter ce dobti updat sa novom RouterOS verzijom, preko v.$deviceOsVerInst instalira se v.$deviceOsVerAvail (update kanal: $updateChannel) \r\nFinalni report sa detaljnim informacijama stize nakon zavrsenog procesa. \r\nAko u roku od 5 minuta ne stigne novi mail onda je vjerovatno nesto poslo u pogresnom smjeru. (Provjeri log na uredjaju)");
				#!!
			}

		}
	}
	:log info ("$SMP provjeravam da li skripta treba backup.");
	if ($forceBackup = true or $scriptMode = "backup" or $isOsNeedsToBeUpdated = true) do={
		:log info ("$SMP da, kreiram potrebni backup.");
		if ($isOsNeedsToBeUpdated = true) do={
			:set backupNameFinal $backupNameBeforeUpd;
		};
		if ($scriptMode != "backup") do={
			:set mailBody ($mailBody . "\r\n\r\n");
		};
		:set mailSubject	($mailSubject . " Backup je kreiran.");
		:set mailBody		($mailBody . "Kreiran je backup sistema te se nalazi u attachment-u ovog mail-a.");
		:set mailAttachments [$buGlobalFuncCreateBackups backupName=$backupNameFinal backupPassword=$backupPassword sensetiveDataInConfig=$sensetiveDataInConfig];
	} else={
		:log info ("$SMP ne, nema potrebe za backup.");
	}
	:set mailBody ($mailBody . $mailBodyDeviceInfo . $mailBodyCopyright);
}
:if ($updateStep = 2) do={
	:log info ("$SMP routerboard fw upgrade u toku...");
	if ($deviceRbCurrentFw != $deviceRbUpgradeFw) do={
		:set isSendEmailRequired false;
		:delay 13s;
		:log info "$SMP radimo upgrade routerboard firmware-a sa v.$deviceRbCurrentFw na v.$deviceRbUpgradeFw";
		/system routerboard upgrade;
		:delay 7s;
		:log info "$SMP routerboard upgrade uspjesno zavrsena, idem se osvjeziti, reboot... cao!";
		/system schedule add name=BKPUPD-FINAL-REPORT-ON-NEXT-BOOT on-event=":delay 7s; /system scheduler remove BKPUPD-FINAL-REPORT-ON-NEXT-BOOT; :global buGlobalVarUpdateStep 3; :delay 13s; /system script run BackUP;" start-time=startup interval=0;
		/system reboot;
	} else={
		:log info "$SMP hmmm, cini mi se da je routerboard vec na novoj verziji, zaobilazimo ovaj korak.";
		:set updateStep 3;
	};
}

:if ($updateStep = 3) do={
	:log info ("$SMP kreiram i saljem finalni izvjestaj...");
	:log info "BackUP: RouterOS i routerboard upgrade procesi su zavrseni uspjesno. nova RouterOS verzija: v.$deviceOsVerInst, routerboard firmware: v.$deviceRbCurrentFw.";
	:log info "$SMP finalni email sa izvjestajem i backup fajlovima saljem za tacno jedan minut.";
	:delay 1m;
	:set mailSubject	($mailSubject . " RouterOS Upgrade je zavrsen, nova verzija je: v.$deviceOsVerInst!");
	:set mailBody 	  	"RouterOS i routerboard upgrade procesi su uspjesno zavrseni. \r\nNova instalisana RouterOS verzija je: v.$deviceOsVerInst, routerboard firmware: v.$deviceRbCurrentFw. \r\n$changelogUrl \r\n\r\nBackup fajlovi sistema se nalaze u aatachmentu ovog mail-a. \r\n\r\n$mailBodyDeviceInfo \r\n\r\n\r\n$mailBodyCopyright";
	:set mailAttachments [$buGlobalFuncCreateBackups backupName=$backupNameAfterUpd backupPassword=$backupPassword sensetiveDataInConfig=$sensetiveDataInConfig];
}

:do {/system script environment remove buGlobalFuncGetOsVerNum;} on-error={}
:do {/system script environment remove buGlobalFuncCreateBackups;} on-error={}

:if ($isSendEmailRequired = true) do={
	:log info "$SMP saljem email poruku, trebati ce mi oko pola minute...";
	:do {/tool e-mail send to=$emailAddress subject=$mailSubject body=$mailBody file=$mailAttachments;} on-error={
		:delay 7s;
		:log error "$SMP neuspjesno slanje email poruke ($[/tool e-mail get last-status]). pokusati cu ponovo za cca 5 minuta."
		:delay 5m;
		:do {/tool e-mail send to=$emailAddress subject=$mailSubject body=$mailBody file=$mailAttachments;} on-error={
			:delay 5s;
			:log error "$SMP neuspjesno slanje email poruke ($[/tool e-mail get last-status]) po drugi put."
			if ($isOsNeedsToBeUpdated = true) do={
				:set isOsNeedsToBeUpdated false;
				:log warning "$SMP skripta nece pokrenuti update proces zbog nemogucnosti slanja backup mail-a."
			}
		}
	}

	:delay 30s;
	:if ([:len $mailAttachments] > 0 and [/tool e-mail get last-status] = "succeeded") do={
		:log info "$SMP cistim fajl sistem."
		/file remove $mailAttachments;
		:delay 2s;
	}
}

if ($isOsNeedsToBeUpdated = true) do={
	/system schedule add name=BKPUPD-UPGRADE-ON-NEXT-BOOT on-event=":delay 7s; /system scheduler remove BKPUPD-UPGRADE-ON-NEXT-BOOT; :global buGlobalVarUpdateStep 2; :delay 13s; /system script run BackUP;" start-time=startup interval=0;
   :log info "$SMP sve je spremno za instalaciju novog RouterOS-a, idem se reboot-ati za koji tren!"
	/system package update install;
}

:log info "$SMP script \"Mikrotik RouterOS automatski backup & update\" je zavrsio svoje zadatke, uspjesno naravno.\r\n";

```

Konfigurisanu skriptu ubacite u ruter **System -> Scripts [Add]** u `source` polje.

## Email postavke

**Tools -> Email**

Postavite email server parametre. Ako nemate mail server, koristite [smtp2go.com](https://smtp2go.com "smtp2go.com") servis, dopušteno slanje 1000 besplatnih mailova mjesečno, isprobao - dobro funkcioniše.

![smtp2go](/fragments/images/backup-2.png)

Provjerite postavke, pošaljite test mail iz terminala sljedećom komandom, izmjenite to, subject & body unutar "" po želji:

```javascript
/tool e-mail send to="tvojMail@tvojDomen.com" subject="backup & update test" body="Ah gle, radi ovo!";
```

Test:
![banana test](/fragments/images/backup-4.png)
Ako je sve ok, stići će vam mail:
![banana test ok](/fragments/images/backup-5.png)

## Kako kreirati sheduled zadatak

Otvorite Sheduler:
```command
System -> Scheduler [Add]
```
Unesite željene opcije:
```command
Name: `Backup & Update`
Start Time: `01:01:00`
Interval: `1d 00:00:00`
On Event: `/system script run BackUP;`
```

#  Testirajte skriptu

Kada sve odradite, testirajte kako bi se uvjerili da je sve dobro podešeno.

Otvorite New Terminal i Log prozorčić u WinBox-u, i pokrenite skriptu manuelno komandom:
```command
system script run BackUP;
```
Vidjeti će te radni proces skripte u log-u. Ako skripta završi bez greške, provjerite email, tu je svježa nova poruka sa backup fajlovima sa vašeg MikroTik rutera 🎉.

Primjer notifikacije:
![notifikacija](/fragments/images/backup-6.png)

Primjer manuelnog pokretanja sa update opcijom:
![update](/fragments/images/backup-7.png)

Primjer email-a pri završetku update-a:
![update](/fragments/images/backup-8.png)

Provjera RouterOS firmware upgrade-a:
![fw upgrade](/fragments/images/backup-7.png)

_~slike gore su uzete pri testiranju i doradi skripte~_

Naravno, ako zatreba pomoć, ili ako želite na naš jezik komentiranu skriptu tu sam.
