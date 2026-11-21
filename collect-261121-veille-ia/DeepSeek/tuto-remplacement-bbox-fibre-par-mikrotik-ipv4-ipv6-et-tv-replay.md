---
id: collect-261121-veille-ia/mikrotik/tuto-remplacement-bbox-fibre-par-mikrotik-ipv4-ipv6-et-tv-replay
title: "[TUTO] Remplacement BBox Fibre par Mikrotik (IPv4, IPv6 et TV/Replay)"
domain: mikrotik
role: reference
task: article
actors: ["DeepSeek"]
dates: []
keywords: ["bbox", "npassword", "mikrotik", "gpon", "configuration", "adresse", "tuto", "interface"]
source: https://lafibre.info/remplacer-bbox/tuto-remplacement-bbox-fibre-par-mikrotik-ipv4-ipv6-et-tvreplay/
source_anchor: ""
source_lines: [1, 217]
section: "DeepSeek"
---
Bonjour à tous, 

Je vous propose un tuto pour remplacer la Box Internet de Bouygues. Je remercie toutes les personnes (en particulier mirtouf et gartox) qui ont postées leurs configurations sur ce forum, sans eux, cela m'aurait pris beaucoup de temps. 

La configuration ci-dessous est une base (très) simplifiée de ma configuration actuelle qui doit permettre le bon fonctionnement d'un routeur Mikrotik sur l'infra Bouygues. Il est possible qu'il y ait des erreurs dû à cette simplification. Pour la bonne compréhension de ce tutoriel, il faut de nombreuses notions de bases (et avancées) en réseau, informatique et Linux (pour la partie TV).

**EDIT 26/09/2024 : Mise à jour RouterOS 7.16 - Attention, RouterOS a changé la manière de traiter dont les priorités pcp/dscp. Lorsque les paquets arrivent sur une interface d'un switch/router (avec chip  switch), les priorités sont réinitialisées. Il faut donc modifier le paramètre Trust L2 et Trust L3 (Ignore à Keep) afin que les paquets marqués avec un pcp/dscp <> 0 ne soient réinitialisées (pcp = 0 + dscp =0)** `/interface/ethernet/switch/qos/port/set trust-l2=keep trust-l3=keep INTERFACENAME`IntroductionConfiguration initiale**Matériel**
Box opérateur : Sagemcom F@st 5688b

SFP : Sagemcom CS50001 sfp_eole_gpon

BoxTV : technicolor Boyugtel 4K UZW4020BYT4

**Offre**
B&You 1G/700M

Configuration finale (sans TV)**Matériel**
Mikrotik CCR2116-12G-4S+

SFP : GPON Huawei MA5671A - Firmware FS.COM (version 6BA1896SPLQA42)

**Je conseille vivement pour les débutants de prendre directement le GPON fs.com https://www.fs.com/products/133619.html**
Comment flasher le firmware (ou utiliser le SFP original fs.com : 

https://hack-gpon.org/ont-huawei-ma5671a-fs-mod/Configuration finale (avec TV)**Matériel**
Idem configuration sans TV

1 serveur avec igmpproxy

Mikrotik CRS310-1G-5S-4S+IN

Matériel requis supplémentaire
Afin de faciliter le paramétrage du module SFP GPON  : 1x convertisseur de media (par exemple TPLink TP-MC22L)

Afin de forcer la version 2 d'IGMP : un second serveur avec igmpproxy (dans mon cas, j'ai utilisé 1 conteneur LXC avec Debian 12 sous Proxmox).

I - Récupérer les informations sur la BoxSur l'étiquette arrière de la BBox
L'adresse MAC : 48:29:FF:FF:FF:FF

L'IMEI : 123456789012345

Dans l'interface de la BBox
Le SN du SFP commençant par SMB : SMBA0000X000

Sur l'étiquette arrière de la BBox TV
L'adresse MAC : D0 5A FF FF FF FF

II - Mettre à jour le SFP GPON
- Installer le SFP dans la cage du TP-Link

- Relier un RJ45 entre 1 PC et le TP-Link

- Configurer l'interface du PC reliée au TP-Link avec une IP dans la plage d'adresse 192.168.1.1 à 192.168.1.254 et différent de 192.168.1.10

- Se connecter en SSH sur l'IP 192.168.1.10 avec les identifiants

User : ONTUSER 

Password : 7sp!lwUBz1

Sourceshttps://lafibre.info/remplacer-bbox/routeur-sfp-pour-remplacer-bbox-fibre-ont/msg922029/#msg922029/https://lafibre.info/remplacer-bbox/retro-ingenierie/msg626687/#msg626687https://lafibre.info/remplacer-bbox/informations-de-connexion-ftth/msg603592/#msg603592Corriger le script onu.sh
Commenter les 4 lignes suivantes dans le fichier  /etc/init.d/onu.sh

```
#       nPassword=""                                                                                
#       nPassword=`fw_printenv nPassword 2>&- | cut -f2 -d=`                  
#       if [ -z "$nPassword" ]; then                                              
          config_get nPassword "ploam" nPassword                                  
#       fi
```
Remplacement des informations```
# Remplacer le numéro de série par celui de la BBox
set_serial_number SMBS0000X0000  
# Changer le mot de passe Ploam par le code IMEI en ajoutant 00000 devant l'IMEI 123456789012345 -> 00000123456789012345
uci set gpon.ploam.nPassword="0x00 0x00 0x01 0x23 0x45 0x67 0x89 0x01 0x23 0x45"
fw_setenv nPassword 0x00 0x00 0x01 0x23 0x45 0x67 0x89 0x01 0x23 0x45
# Remplacer l'adresse MAC par l'adresse MAC de la BBox
uci set network.lct.macaddr="48:29:FF:FF:FF:FF"
uci set network.host.macaddr="48:29:FF:FF:FF:FF"
# Valider les modifications
uci commit
# Redémarrer l'ONT
reboot
```
Vérifier les informations
Se reconnecter au SFP

```
#Vérifier le Ploam Password, quoi doit correspondre à l'IMEI avec 5 zeros devant
gtop b
```
Configurer le Mikrotik**Note importante : Les configurations ci-dessous supposent qu'il n'y a aucune règle firewall. Il faut les adapter ensuite selon les besoins de chacun.**IPv4 (WAN)
Important :

- Ajout de l'option DHCP VendorID dans le client DHCP

- Création d'un VLAN 100 sur l'interface du SFP pour le client DHCP

```
/interface ethernet
set [ find default-name=sfp-sfpplus2 ] auto-negotiation=no name=LNK_GPON_BYT speed=2.5G-baseX
/interface vlan
add interface=LNK_GPON_BYT name=BouyguesNet vlan-id=100
/ip dhcp-client option
add code=60 name=vendorid_bbox value="'BYGTELIAD'"
/ip dhcp-client
add dhcp-options=hostname,clientid,vendorid_bbox interface=BouyguesNet
/ip firewall nat
add action=masquerade chain=srcnat comment="Accept masquerade on WAN interface" out-interface-list=WAN
/interface list
add comment="Interface for WAN - Interface that needs Masquerade" name=WAN
/interface list member
add interface=BouyguesNet list=WAN
```
IPv6 (WAN)
Important :

- Modification de la MAC (=MAC de la BBox) de l'interface Ethernet du SFP

- Création d'un VLAN 100 sur l'interface du SFP (déjà réalisé dans IPv4) : BouyguesNet

```
# Activer IPv6 avec RA + Forward
/ipv6 settings
set accept-redirects=no accept-router-advertisements=yes disable-ipv6=no forward=yes max-neighbor-entries=4096
# Demander le préfixe à Bouygues
/ipv6 dhcp-client
add add-default-route=yes default-route-distance=1 dhcp-options="" dhcp-options="" disabled=no interface=BouyguesNet pool-name=ipv6-pool pool-prefix-length=64 prefix-hint=::/0 request=prefix \
    use-interface-duid=yes use-peer-dns=no
```
IPv4 (LAN)```
# Changer le nom de l'interface LAN
/interface ethernet
set [ find default-name=sfp-sfpplus4 ] auto-negotiation=no name=LAN
#Créer une adresse IP serveur dhcp pour la partie LAN
/ip address add address=192.168.100.1/24 interface=LAN disabled=no
#Créer un pool d'adresse pour le serveur dhcp
/ip pool add name=LAN-pool1 ranges=192.168.100.100-192.168.100.250
#Créer le serveur dhcp pour la partie LAN
/ip dhcp-server add interface=LAN name=LAN address-pool=LAN-pool1
#Créer la configuration du serveur dhcp pour la partie LAN
/ip dhcp-server network add address=192.168.100.0/24 interface=LAN name=LAN gateway=192.168.100.1 dns-server=192.168.100.1
#Activer l'écoute dns sur le mikrotik
/ip dns set allow-remote-requests=yes
```
IPv6 (LAN)```
# Désactiver le nd pour toutes les interfaces et autoriser que celle dont vous avez besoin
/ipv6 nd
set [ find default=yes ] advertise-dns=no disabled=yes hop-limit=64
add interface=LAN
# Ajouter une adresse IPv6 à votre LAN (l'adresse sera générée automatiquement pour l'interface et sera distribuée avec SLAAC)
/ipv6 address
add address=::1 from-pool=ipv6-pool interface=LAN
```
TV+Replay
Important

- Disposer d'un service proxy IGMP sur le routeur

- Forcer la version 2 d'IGMP pour les requêtes envoyées sur le VLAN 100

- Changer la priorité (pcp/802.1p) à 5 pour les requêtes IGMP envoyées sur le VLAN 100

- Forcer les serveurs DNS de la BBox TV

NB : Pour le Replay, je n'ai eu aucune configuration spécifique à réaliser (on parle d'une redirection des ports 20000-30000)

```
/routing igmp-proxy
set query-interval=1m quick-leave=yes
/routing igmp-proxy interface
add alternative-subnets=89.86.96.0/24,89.86.97.0/24,176.165.8.0/24,193.251.97.0/24 interface=BouyguesNet upstream=yes
add interface=LAN
#Ajouter l'option DNS pour les serveurs
/ip/dhcp-server/option
add code=6 name=dns-bboxtv value="'194.158.122.10''194.158.122.15'"
#Créer un bail dhcp spécifique pour la BBox TV
/ip dhcp-server lease
add address=192.168.100.10 comment="BBox TV" dhcp-option=dns-bboxtv mac-address=D0:5A:xx:xx:xx:xx server=LAN
/ip firewall mangle
add action=set-priority chain=output new-priority=5 out-interface=BouyguesNet passthrough=no protocol=igmp
```
Forcer IGMPv2 (workaround)
- Relier le port WAN du routeur à un switch

- Relier la première interface du second igmpproxy au LAN (pour l'installation  bon fonctionnement d'IGMP Proxy)

- Relier la seconde interface du second igmpproxy au même switch que SFP GPON

- Le switch doit être correctement configuré pour autoriser le VLAN100 sur les 2 ports, ne pas activer l'IGMP Snooping sur le VLAN100

**Installation du conteneur LXC sous proxmox**
Voici une très rapide explication de ce que j'ai fait.

*Se connecter en ssh au serveur proxmox et créer le conteneur*```
#Télécharger le script de création du conteneur LXC
wget https://github.com/tteck/Proxmox/raw/main/ct/debian.sh
#Modifier les droits pour l'exécution
chmod +x ./debian.sh
```
*Exécuter le script et configurer le conteneur avec l'interface connectée au LAN**Créer une seconde interface eth1 pour le conteneur lxc via CLI ou WebUI**Se connecter au conteneur lxc*```
#Lancer la console du conteneur
lxc-console XXX 
#Saisir les identifiants renseignés lors de l'installation du contenur
#Installer iptables + iptables-persistent + tcpdump + igmpproxy
apt-get update;apt-get install iptables iptables-persistent tcpdump igmpproxy
#Bloquer le traffic par défaut pour les chain INPUT, OUTPUT, FORWARD - parce qu'on a besoin de rien d'autre
iptables -P INPUT DROP
iptables -P OUTPUT DROP
iptables -P FORWARD DROP
#Autoriser 12 paquets IGMP par heure vers l'adresse 224.0.0.1/32 et l'interface de sortie eth1
iptables -A OUTPUT -d 224.0.0.1/32 -o eth1 -p igmp -m limit --limit 12/hour -j ACCEPT
#Sauvegarder les règles iptables
netfilter-persistent save
#Quitter la console avec ctrl-a puis q
```
Bon courage à tous.
