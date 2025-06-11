J'ai remplacé ma BBox Fibre par une meilleure offre de la part de RED-By-SFR. J'ai donc maintenant, pour mon acces internet à la maison, cette offre à 22€/mois de RED, qui s'appuie sur l'infrastructure de SFR, y compris la box.

Précédemment, avec mon abonnement BBox de Bouygues Telecom, je payais 34€ par mois, donc là j'économise 10€ par mois (en juin 2025). Voici quelques différences entre les deux box :
- La box SFR possède le terminal optique intégré à la box, tandis que celle de Bouygues avait un boitier convertisseur optique-ethernet séparé. J'ai du acheté un rallonge optique ("jarretiere") SC-APC pour pouvoir positionner la box où je voulais.
- La box SFR possède un seul port téléphonique, celle de Bouygues en a deux

Quelques aventages de cette nouvelle box :
- possibilité d'avoir un deuxième réseau Wifi, le "guest", avec un mot de passe différent. Il n'y a pas de possibilité de mettre de la QOS sur ce deuxième réseau, donc l'interêt est limité.
- ipv6 natif par défaut, en LAN ou sur internet. Je n'ai jamais réussi à faire fonctionner l'ipV6 sur la Bbox.

Quelques gros points noirs a relever avec la box SFR :
- l'interface de gestion web, accessible via http://192.168.1.1/ est catastrophique : lente, pas en https, accessible uniquement par son adresse IP.
- pas de control parental : impossible de désactive le wifi pour certains équipements à des heures précises.
- impossible, par défaut, de faire de la redirection de port pour qu'une machine du LAN soit accessible d'internet en ipv4. Cella provient de la mutualisation des adresses ipv4 sur le réseau SFR. Il y'a un message d'avertissement qui apparait dans la console web : "Les configurations IPv4 utilisateur peuvent ne pas fonctionner correctement en raison de l'architecture de routage IPv6 WAN actuelle.". Ce warning n'est pas clair, aucune info, aucune option pour chnager "l'architecture de routage IPv6". Il faut contacter le support technique, via l'appli RED et demander un demander un rollback en IPv4 full stack

La box ne propose pas d'API standard pour accéder aux informations. Il y'a bien une "simulation" d'une vieille API issue des Neuf-box (https://lafibre.info/sfr-les-news/nb6vac-et-api/) mais elle n'est pas complete. Pour avoir accès à plus d'information sur la box, il faut passer par l'interface web, et donc faire un bon gros scrapping des pages.

Voici un script python qui permet d'obtenir quelques informations de la box : https://gist.github.com/psa-jforestier/1f8e99b6be687c26d5e6473633ba62ab#file-rbox_status-py

```
$ python3 rbox_status2.py -u admin -p password
>>> Login
login_time: 5.49749755859375

>>> BoxInfo
routeSwVersion: 3ENT010201R00C
routerUptime: 2d 6h 27m 27s
routerOpticalInterface: Up|-26.0dBm|4.4dBm|NOS
lanPortStatus: 1|1000|Full|1|100|Full|0|0|---|0|0|---
wifi0Ssid: XXX
wifi1Ssid: XXX
voiceStatus: Up
voiceExtension: +331XXXXXXXX
tvStatus: 1

>>> BoxInfoExtra
routerName: GR140CG
wifi0GuestStatus: 1
wifi1GuestStatus: 1

>>> WifiGuest
wifiGuestSsid: jejebox_guest

>>> RouterDeviceInfo
routerName: GR140CG
routeSwVersion: 3ENT010201R00C
routerUptime: 2d 6h 27m 37s
linkStatus: 5
routerOpticalInterface: Up|-26.0dBm|4.3dBm|NOS

>>> WANStatistics
wanRx: 1188794954
wanTx: 569177370

>>> LANDevices
>>>>>> LanDevice1
name: JXXX
ipv4: 192.168.1.78
ipv6:
mac: B8:XXX
intf: eth0.0
deviceStatus: 1

>>> LANStatistics
>>>>>> Interface1
intf: eth0
rx: 25086797
tx: 204887641

>>>>>> Interface2
intf: eth1
rx: 21255638
tx: 70557527

>>>>>> Interface3
intf: eth2
rx: 0
tx: 0

>>>>>> Interface4
intf: eth3
rx: 0
tx: 0


>>> WifiDevices
>>>>>> WifiDevice1
name: JXXXX
ipv4: 192.168.1.103
ipv6: XXXXX
mac: 50:XXXX
intf: wl0
deviceStatus: 1

>>>>>> WifiDevice2
name: Google-Home-Mini
ipv4: 192.168.1.48
ipv6: 2a02:XXXX
mac: 44:XXXX
intf: wl0
deviceStatus: 1
(...)

>>> WifiStatistics
>>>>>> Interface1
intf: wl0[WiFi 2.4GHz]
rx: 860785135
tx: 2607918849

>>>>>> Interface2
intf: wl1[WiFi 5GHz]
rx: 4248568195
tx: 612480837


>>> OtherInformation
nbDeviceUp: 10
timeToComplete: 28.67517924308777
```

