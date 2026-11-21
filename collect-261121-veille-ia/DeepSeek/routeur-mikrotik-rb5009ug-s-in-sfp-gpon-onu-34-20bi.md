---
id: collect-261121-veille-ia/deepseek/routeur-mikrotik-rb5009ug-s-in-sfp-gpon-onu-34-20bi
title: "Routeur MikroTik RB5009UG+S+IN & SFP GPON-ONU-34-20BI"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: []
keywords: ["routeur", "find", "default-name", "disabled", "rb5009ug", "part", "mikrotik", "livebox"]
source: https://lafibre.info/remplacer-livebox/routeur-mikrotik-rb5009ugsin-pour-remplacer-livebox/
source_anchor: ""
source_lines: [1, 263]
section: "DeepSeek"
---
Bonjour, 

Le routeur MikroTik RB5009UG+S+IN peut servir à remplacer le livebox, je propose de créer un thread dédié.

Ce routeur n'est pas aussi avancé que son grand frère le CCR2004, mais est largement plus accessibles pour nos bourses. 

Il est également plus évolué que le HeX S. 

La complexité du CCR2004 fait que la configuration proposée dans le thread original 

https://lafibre.info/remplacer-livebox/guide-de-connexion-fibre-directement-sur-un-routeur-voire-meme-en-2gbps/
 est loin de coller directement à ce routeur, elle m'a à la fois aidé, mais par la suite découragé la première fois que j'ai essayé de configurer mon RB5009UG. 

Je vais ici détailler ce que j'ai fait pour arriver à faire marcher l'ipv4, et on verra où cela nous mènera. 

J'ai comme décrit sur la page du CCR2004, acheté un ONU acheté chez FS.com. 

Réf. de l'ONU : SFP GPON-ONU-34-20BI (

https://www.fs.com/fr/products/183843.html?attribute=46672&id=3208708
)

**PART 1 : Quelques notes et configuration basique**
On se prépare psychologiquement, et on sauvegarde quelque part les élements suivant que l'on trouve notés sur l'arrière de sa livebox, dans les menus admin, ou dans la doc reçu avec la livebox.

- Serial number : SMBSXXXXXXXX

- Hardware version : SMBSSXXXXXXX

- Login/password pour la connexion.

On branche le routeur, cable rj45 sur le port ether8. 

On accède à sa console d'admin à partir de son navigateur, à partir de l'adresse : 192.168.88.1

On change le mot de passe, on met à jour son routeur et on effectue les modifications suivantes : 

On débranche/rebranche son cable réseau.

**PART 2 : Configuration de l'ONU**
1/ sur le GPON après un ssh sur 192.168.1.10

[Conditions : 

- Faut avoir la fibre connectée sur le module 

- Si vous êtes sur votre réseau en 192.168.1.255 , alors vérifier bien que vous n'avez rien en .10]

`ssh -o KexAlgorithms=diffie-hellman-group14-sha1 -oHostKeyAlgorithms=+ssh-dss  192.168.1.10 -l ONTUSER`
Mot de passe 

`7sp!lwUBz1`
2/ on rentre le numéro de série précédement noté 

`set_serial_number SMBSXXXXXXXX`
3/ on rentre le vendor_ID 

`sfp_i2c -i 7 -s “ SMBS”`
4/ pour ma part, j'ai dû mettre le hardware version de l'ONT. 

```
cp /etc/mibs/data_1g_8q.ini /etc/mibs/data_1g_8q.ini.ORG
vi /etc/mibs/data_1g_8q.ini
```
rempalacer la ligne ci-dessous avec le hardware version de l'ONT =>

# ONT-G

```
256 0 HWTC 0000000000000 00000000 2 0 0 0 0 #0
```
PAR

```
256 0 SMBS SMBSSXXXXXXX\0 00000000 2 0 0 0 0 #0
```
5/ un petit reboot

`reboot`
6/ On se reconnecte, puis on tape cette commande dont le résultat devrait faire apparaitre 

**"curr_state=5 previous_state=4"**`onu ploamsg`**PART 3 : Configuration IPv4**
Je désactive les interfaces que je ne compte pas utiliser.

De ether2 à ether7. 

Pour le moment on est branché à ether8, on utilisera ether8.

L'idéal étant d'utiliser ether1 qui est le port en 2,5Gb/s

```
/interface ethernet
set [ find default-name=ether2 ] disabled=yes
set [ find default-name=ether3 ] disabled=yes
set [ find default-name=ether4 ] disabled=yes
set [ find default-name=ether5 ] disabled=yes
set [ find default-name=ether6 ] disabled=yes
set [ find default-name=ether7 ] disabled=yes
```
On met l'adresse mac de sa livebox (adaptez bien cette ligne... n'allez pas la recopier)

```
/interface bridge
add admin-mac=20:XX:XX:XX:XX:XX auto-mac=no name=bridge
```
VLAN 832 pour Internet

```
/interface vlan
add comment="Internet ONT" interface=sfp-sfpplus1 loop-protect-disable-time=0s \
    loop-protect-send-interval=1s name=vlan832-internet vlan-id=832
```
Set des options du client DHCP, adaptez les 140 caractères YYYYYYYY, comme suivant :

1- allez sur 

https://jsfiddle.net/kgersen/3mnsc6wy/
, entrez le login/pass et copiez la sortie simplement.

2- dans votre éditeur préféré : ajouter 0x au début, et supprimez les deux points ":" partout.

```
/ip dhcp-client option
add code=60 name=vendor-class-identifier value=0x736167656d
add code=77 name=userclass value="0x2b46535644534c5f6c697665626f782e496e7465726e65742e736f66746174686f6d652e4c697665626f7833"
add code=90 name=authsend  value=0xYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYY
```
```
/ip dhcp-server
add address-pool=dhcp interface=bridge lease-time=1w name=LAN
```
```
/interface bridge filter
add action=set-priority chain=output dst-port=67 ip-protocol=udp log=yes \
    log-prefix="Set CoS6 on DHCP request" mac-protocol=ip new-priority=6 \
    out-interface=vlan832-internet passthrough=yes
```
```
/ip dhcp-client
add dhcp-options=hostname,clientid,authsend,userclass,vendor-class-identifier \
    interface=bridge
```
```
/ip dhcp-server network
add address=192.168.1.0/24 comment=defconf dns-server=1.1.1.1,1.0.0.1 \
    gateway=192.168.1.1 netmask=24
```
Règles firewall, et je suis totalement dépassé ! 

```
/ip firewall address-list
add address=192.168.1.0/24 list=support
add address=192.168.42.0/24 list=support
add address=192.168.255.0/24 list=support
##############################
# Puis quelques classes qui ne devraient pas nécessairement pousser des datagrames sur l'interface WAN
##############################
add address=0.0.0.0/8 comment="Self-Identification [RFC 3330]" list=bogons
add address=10.0.0.0/8 comment="Private[RFC 1918] - CLASS A" disabled=yes list=bogons
add address=127.0.0.0/16 comment="Loopback [RFC 3330]" list=bogons
add address=169.254.0.0/16 comment="Link Local [RFC 3330]" list=bogons
add address=172.16.0.0/12 comment="Private[RFC 1918] - CLASS B" disabled=yes list=bogons
add address=192.168.0.0/16 comment="Private[RFC 1918] - CLASS C" disabled=yes list=bogons
add address=192.0.2.0/24 comment="Reserved - IANA - TestNet1" list=bogons
add address=192.88.99.0/24 comment="6to4 Relay Anycast [RFC 3068]" list=bogons
add address=198.18.0.0/15 comment="NIDB Testing" list=bogons
add address=198.51.100.0/24 comment="Reserved - IANA - TestNet2" list=bogons
add address=203.0.113.0/24 comment="Reserved - IANA - TestNet3" list=bogons
#add address=224.0.0.0/4 comment="MC, Class D, IANA" disabled=yes list=bogons
/ip firewall filter
add action=add-src-to-address-list address-list=Syn_Flooder address-list-timeout=30m chain=input comment="Add Syn Flood IP to the list" connection-limit=30,32 protocol=tcp tcp-flags=syn
add action=drop chain=input comment="Drop to syn flood list" src-address-list=Syn_Flooder
add action=add-src-to-address-list address-list=Port_Scanner address-list-timeout=1w chain=input comment="Port Scanner Detect" protocol=tcp psd=21,3s,3,1
add action=drop chain=input comment="Drop to port scan list" src-address-list=Port_Scanner
add action=jump chain=input comment="Jump for icmp input flow" jump-target=ICMP protocol=icmp
add action=drop chain=input comment="Block all access to the winbox - except to support list # DO NOT ENABLE THIS RULE BEFORE ADD YOUR SUBNET IN THE SUPPORT ADDRESS LIST" dst-port=8291 protocol=tcp src-address-list=!support
add action=jump chain=forward comment="Jump for icmp forward flow" jump-target=ICMP protocol=icmp
add action=drop chain=forward comment="Drop to bogon list" dst-address-list=bogons
add action=add-src-to-address-list address-list=spammers address-list-timeout=3h chain=forward comment="Add Spammers to the list for 3 hours" connection-limit=30,32 dst-port=25,587 limit=30/1m,0:packet protocol=tcp
add action=drop chain=forward comment="Avoid spammers action" dst-port=25,587 protocol=tcp src-address-list=spammers
add action=accept chain=input comment="Accept DNS - UDP" port=53 protocol=udp
add action=accept chain=input comment="Accept DNS - TCP" port=53 protocol=tcp
add action=accept chain=input comment="Accept to established connections" connection-state=established
add action=accept chain=input comment="Accept to related connections" connection-state=related
add action=accept chain=input comment="Full access to SUPPORT address list" src-address-list=support
add action=drop chain=input comment="Drop anything else! # DO NOT ENABLE THIS RULE BEFORE YOU MAKE SURE ABOUT ALL ACCEPT RULES YOU NEED"
add action=accept chain=ICMP comment="Echo request - Avoiding Ping Flood" icmp-options=8:0 limit=1,5:packet protocol=icmp
add action=accept chain=ICMP comment="Echo reply" icmp-options=0:0 protocol=icmp
add action=accept chain=ICMP comment="Time Exceeded" icmp-options=11:0 protocol=icmp
add action=accept chain=ICMP comment="Destination unreachable" icmp-options=3:0-1 protocol=icmp
add action=accept chain=ICMP comment=PMTUD icmp-options=3:4 protocol=icmp
add action=drop chain=ICMP comment="Drop to the other ICMPs" protocol=icmp
add action=jump chain=output comment="Jump for icmp output" jump-target=ICMP protocol=icmp
```
Si on expose le port ssh:

```
add action=drop chain=input comment="drop ssh brute forcers" dst-port=22 protocol=tcp src-address-list=ssh_blacklist
add action=add-src-to-address-list address-list=ssh_blacklist address-list-timeout=1w3d chain=input connection-state=new dst-port=22 protocol=tcp src-address-list=ssh_stage3
add action=add-src-to-address-list address-list=ssh_stage3 address-list-timeout=1m chain=input connection-state=new dst-port=22 protocol=tcp src-address-list=ssh_stage2
add action=add-src-to-address-list address-list=ssh_stage2 address-list-timeout=1m chain=input connection-state=new dst-port=22 protocol=tcp src-address-list=ssh_stage1
add action=add-src-to-address-list address-list=ssh_stage1 address-list-timeout=1m chain=input connection-state=new dst-port=22 protocol=tcp
add action=drop chain=forward comment="drop ssh brute downstream" dst-port=22 protocol=tcp src-address-list=ssh_blacklist
```
```
/ip firewall nat
add action=masquerade chain=srcnat out-interface=bridge to-addresses=0.0.0.0
```
```
/ip service
set telnet disabled=yes
set ftp disabled=yes
```
Avec ça vous avez internet. 

**PART 4 : Configuration IPv6***Je ne le ferai pas, si quelqu'un est motivé pour s'en charger, je mettrai à jour ici cette partie.* **PART 5 : Autours du routeur***** dhcp statique :**
Voici un exemple, à adapter selon votre IP/adresse mac. 

```
add address=192.168.1.100 comment="cam 1" mac-address=XX:XX:XX:XX:XX:XX
add address=192.168.1.101 comment="cam 2" mac-address=XX:XX:XX:XX:XX:XX
```
*** création d'un DNS NAME**```
/ip cloud 
set ddns-enabled=yes
print
```
On devrait avoir ceci : 

```
 ddns-enabled: yes
 ddns-update-interval: none
          update-time: yes
       public-address: 159.148.147.196
  public-address-ipv6: 2a02:610:7501:1000::2
             dns-name: 529c0491d41c.sn.mynetname.net.   <<<<<<<<<<<<<<< voici ce qui nous interesse.
               status: updated
```
Maintenant il faut créer une liste d'adresses qui ne contiendra que votre DNS Name.

Cette adresse sera utilisée pour le port forward.

```
add address=529c0491d41c.sn.mynetname.net list=public_address
```
*** port forwarding avec loopback**
Exemple avec port 443.

Au lieu de filtrer avec l'IP externe directement, qui risque de changer, on utilise la liste "public_address" qui ne contient qu'une seule adresse : votre ip externe. 

Pour être honnête, je n'ai pas encore tester le changement d'ip. 

```
/ip/firewall/nat
add action=dst-nat chain=dstnat comment=https dst-address-list=public_address \
    dst-port=443 protocol=tcp src-address-list="" to-addresses=192.168.1.100 \
    to-ports=443
```
CHANTIER EN COURS.

N'hésitez pas à apporter des corrections et améliorations. 

J'espère que ça simplifiera grandement la vie à ceux qui n'ont pas le temps de faire un master réseau pour changer leur livebox.
