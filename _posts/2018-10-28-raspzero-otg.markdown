---
layout: post
title:  "Raspberry Pi Zero OTG Problems"
date:   2018-10-28 16:47:27 +0200
categories: raspberry pi
---
TL;DR Connettersi al Raspberrypi Zero via OTG non è sempre una cosa facile, soprattutto quando ci si vuole connettere usando mDNS ovvero utilizzando l'inidrizzo del tipo *raspberrypi.local*.   


Premetto che ho cambiato l'hostname del mio raspberry in `raspzerow` quindi tutti i seguenti comandi vanno modificati per il corretto hostname.


Problemi riscontrati finora e possibili soluzioni:

### Problema 1
Ne sono affetti sia Linux che Windows in quanto la causa è il Raspberry.       
Capita di non riuscire a connettersi al Raspberry utilizzando l'indirizzo `raspzerow.local`. Una delle possibili cause è che `avahi-daemon` nel Raspberry si blocchi e quindi l'indirizzo `raspzerow.local` non venga risolto. Se avete un altro accesso al Rasp (tipo wifi), connettetevi e lanciate il comando
`sudo service avahi-daemon restart`. Riavviate per sicurezza. Se questa era la causa dovreste essere in grado di pingare e quindi connettervi.

### Problema 2
Problema in Windows.   
A volte *Bonjour Service*, reponsabile della risoluzione degli indirizzi mDNS, risolve correttamente gli inidrizzi mDNS, ma non è possibile pingarli o connettersi (ritorna l'errore *Unknow Host*).

Si può utilizzare `dns-sd` per trovare dispositivi che espongono servizi o risalire all'indirizzo ip dall'indirizzo locale (se questo è stato risolto).

Ad esempio per trovare servizi:   
```
>dns-sd -B _ssh._tcp

Browsing for _ssh._tcp
```

Per trovare l'indirizzo ipv6 a partire da quello locale:
```
>dns-sd -G v6 raspzerow.local

Timestamp     A/R Flags if Hostname          Address                                      TTL
22:16:19.287  Add     2 24 raspzerow.local.  FE80:0000:0000:0000:0243:B36F:E207:30D7%ethernet_32780 120
```


Per trovare l'indirizzo ipv4 a partire da quello locale:
```
>dns-sd -G v4 raspzerow.local

Timestamp     A/R Flags if Hostname          Address                                      TTL
22:17:01.125  Add     2 24 raspzerow.local.  169.254.50.63                                120
```

A questo punto connettetevi utilizzando l'ip.

### Problema 3
Problema in Linux.

A volte Linux non riesce a connettersi alla Lan creata dal Raspberry connesso in OTG. Non so la causa, ma so la soluzione. 

Andate sulle proprietà della connessione creata dal Raspberry, `ipv4->only-local-links` e `ipv6->only-local-links`
