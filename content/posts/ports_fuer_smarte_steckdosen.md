---
title: "Firewall Ports für diverse smarte Steckdosen"
date: 2024-09-27T23:05:03+00:00
# weight: 1
# aliases: ["/first"]
tags: ["Firewall","OPNSense","Smarte Steckdosen","Hama","Amazon Echo","Smartlife","Tuya","Smarthome"]
author: "Christian Weber"
# author: ["Me", "You"] # multiple authors
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: true
description: "Smarte Geräte im Heimnetzwerk benötigen bestimmte geöffnete Ports an einer FIrewall, damit die Geräte kommunizieren können. Hier findest du eine kleine Auswahl."
canonicalURL: "https://www.christian-weber.net/posts/ports_fuer_smarte_steckdosen/"
disableHLJS: true # to disable highlightjs
disableShare: false
disableHLJS: false
hideSummary: false
searchHidden: true
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
cover:
    image: "<image path/url>" # image path/url
    alt: "<alt text>" # alt text
    caption: "<text>" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: true # only hide on current single page
---

### Wie man kurzerhand das halbe Haus lahmlegt

Vor etwa vier Wochen habe ich mir aus Komponenten, die ich noch auf Lager hatte, eine kleine Firewall in einem 1U 19" Gehäuse gebaut. Hintergrund ist, dass ich Kunden übernommen habe, die alte Sophos Firewalls im Einsatz haben. Bei einem Kunden ist sogar der bisherige IT-Verantwortliche verstorben und hat mangels Dokumentation diverse Logins mit ins Grab genommen. Da im nächsten Jahr sowieso ein Wechsel in der gesamten IT ansteht, wollte ich die Gelegenheit nutzen und mich mit OPNsense weiter in das Thema Firewalls einarbeiten. Ich habe schon früher mit pfSense gearbeitet und viel früher in meinen Anfängen mit IPCop.

Nachdem ich also meine Firewall gebaut und OPNsense installiert hatte, ging es an die Arbeit. Die Firewall wurde zwischen Router und Netzwerk gehängt und zunächst so konfiguriert, dass alles offen war. Danach beschäftigte ich mich zunächst mit weiteren kleinen Plugins wie AdGuard, ClamAV und CrowdSec.

Es folgten diverse Firewall-Regeln, im Prinzip erst mal alles, was man zum Surfen, Mails schreiben und versenden sowie SSH so braucht. Die Firewall lief gut, war ein paar Stunden in Betrieb und dann merkte ich so langsam, dass da noch ein Berg Arbeit auf mich wartete: Praktisch kein smartes Gerät im Haus wollte noch irgendetwas für mich tun. Licht an- und ausschalten oder Radio hören über unsere Echo-Geräte war nicht mehr möglich, unser Haus war praktisch lahm gelegt.

### Die ewige Suche nach den richtigen Ports

Die Ports für die Echos waren schnell gefunden und freigeschaltet, aber vor allem unsere Steckdosen bereiteten mir Kopfzerbrechen. Ich habe (leider) einige Steckdosen von Gosund, diese schalten sowieso nur, wenn sie wollen - und die passenden Ports für unsere Modelle zu finden, war nicht einfach. Bei einigen anderen Herstellern musste ich es per Mail versuchen. Positiv hervorzuheben ist hier Hama, die innerhalb weniger Stunden sehr ausführlich auf meine Anfrage geantwortet haben. Bei anderen warte ich bis heute.

### Die Liste mit allen Ports

Natürlich ist diese Liste nicht vollständig, aber sie deckt 90% der von uns verwendeten Geräte ab. Für den einen oder anderen können die Ports hilfreich sein, wenn man danach sucht.

| Hersteller                                                             | Port        | Protokoll |
| ---------------------------------------------------------------------- | ----------- | --------- |
| KASA                                                                   | 9999        | TCP       |
| Tapo (bei uns im speziellen Kameras, für Steckosen ggf. nicht passend) | 554         | TCP       |
| Gosund                                                                 | 51526       | TCP       |
| Gosund                                                                 | 44077       | TCP       |
| Smartlife/Tuya                                                         | 8883        | TCP       |
| Smartlife/Tuya                                                         | 6667-6669   | TCP       |
| Smartlife/Tuya                                                         | 6681        | TCP       |
| Smartlife/Tuya                                                         | 6608        | TCP       |
| Smartlife/Tuya                                                         | 6682        | TCP       |
| Hama                                                                   | 8001        | TCP       |
| Hama                                                                   | 67-68       | TCP       |
| Hama                                                                   | 10443       | TCP       |
| Hama                                                                   | 2442        | TCP       |
| Hama                                                                   | 1443        | TCP       |
| Hama                                                                   | 8886        | TCP       |
| Hama                                                                   | 8883        | TCP       |
| Amazon Echo                                                            | 3478 (STUN) | UDP       |
| Amazon Echo                                                            | 49152-65535 | UDP       |
| Amazon Echo                                                            | 5353        | UDP       |
| Amazon Echo                                                            | 4070        | TCP + UDP |

#### Weitere Ports

Wir hatten auch das Problem, dass das Versenden von WhatsApp Nachrichten sehr lange dauerte. Das liegt daran, dass WhatsApp auf Port 443 als Fallback zurückgreift, nachdem es vorher versucht hat über Port 5222 zu kommunizieren. Dazu habe ich Aussagen in verschiedene Richtungen gefunden, die einen sagen es geht generell nur über Port 443, die anderen sagen WhatsApp versucht es erst ein paar mal über 5222. Wie auch immer, seit der Port offen ist, kommen die Nachrichten direkt bei uns an. 
Daher hier noch eine kleine Liste mit "Zusatzdiensten":

| Anwendung                                         | Port        | Protokoll |
| ------------------------------------------------- | ----------- | --------- |
| WhatsApp                                          | 5222        | TCP       |
| Microsoft Teams                                   | 3478-3481   | UDP       |
| Discord                                           | 50000-65535 | UDP       |
| Push Notifications Smartphones (ggf. nur Android) | 5229-5230   | TCP       |
| SSH                                               | 22          | TCP       |

##### Schlussworte

Vielleicht kann ich dem einen oder anderen mit der Liste helfen. Von einigen Herstellern warte ich heute noch auf eine Antwort bzgl. der Ports, werde sie aber wohl nie bekommen, da sie irgendwo im tiefsten China sitzen. Natürlich könnte man mit Wireshark und Co. alles selbst recherchieren, aber ich bin mit den Steckdosen von Smartlife nicht nur wegen ihrer kompakten Abmessungen sehr zufrieden und werfe deshalb die letzten Kandidaten von Gosund und Co. nach und nach aus dem Haus.