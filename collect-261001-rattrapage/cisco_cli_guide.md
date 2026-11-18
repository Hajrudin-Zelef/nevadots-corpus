# Cisco — Guide CLI complet (IOS / IOS XE)

> Synthèse rédigée à partir de la documentation officielle Cisco
> (Command Reference IOS/IOS XE). Couvre l'essentiel du CLI : accès,
> switching, routage, NAT, VPN, QoS, HA, sécurité, supervision et
> maintenance. Focus IOS/IOS XE (routeurs ISR/ASR, switchs Catalyst) ;
> les différences IOS XR / NX-OS sont signalées en fin de document.
>
> **À jour 2025-2026** : les exemples visent **IOS XE 17.x** (17.9 / 17.12 /
> 17.15, versions recommandées actuelles). IOS 15.x est en fin de vie :
> certaines commandes legacy (crypto map IKEv1, PAgP, RIPv1) y existent
> encore mais sont dépréciées. Algorithmes faibles (DES, 3DES, MD5,
> DH groupes 1/2/5) bannis des exemples — voir §13.
> Vérifiez les variantes selon votre version exacte (`show version`).

---

## 1. Gammes et concepts

- **IOS / IOS XE** : routeurs ISR/ASR, switchs Catalyst. CLI historique,
  le plus répandu.
- **IOS XE 17.x** (2025) : ISR 4000, Catalyst 8000 (edge), Catalyst
  9000 (9300/9400/9500/9600, switching). C'est la cible des exemples
  de ce guide.
- **IOS XR** : routeurs opérateurs (ASR 9000, NCS). Syntaxe proche mais
  avec `commit` obligatoire après chaque changement.
- **NX-OS** : switchs datacenter Nexus. Syntaxe proche d'IOS avec
  particularités (`feature` à activer : `feature ospf`, `feature bgp`…).
- Ce guide couvre **IOS/IOS XE**, la base commune (~90 % du CLI).
- Modes : `User EXEC` (`Router>`) → `Privileged EXEC` (`Router#`) →
  `Global config` (`Router(config)#`) → sous-modes
  (`config-if`, `config-router`, `config-line`…).
- `show` = afficher, `no` = annuler (équivalent du `undo` Huawei).
- La config active est en RAM : **`copy running-config startup-config`**
  (ou `write memory`) pour sauvegarder, sinon tout est perdu au reboot.

---

## 2. Premier accès et configuration de base

- Console : 9600 bauds, 8N1. Pas de mot de passe par défaut sur la
  plupart des équipements récents (configuration initiale via le
  dialogue `setup`).
- SSH/HTTPS : à configurer (voir §12).

```shell
Router> enable
Router# configure terminal
Router(config)# hostname R-SIEGE
R-SIEGE(config)# no ip domain-lookup
R-SIEGE(config)# ip domain-name entreprise.lan
R-SIEGE(config)# service password-encryption
R-SIEGE(config)# banner motd # Acces reserve #
R-SIEGE(config)# end
R-SIEGE# show version
R-SIEGE# show running-config
```

---

## 3. Commandes fondamentales

```shell
enable / disable              # monter / descendre de niveau
configure terminal            # passer en configuration (conf t)
end                           # retour en mode privilégié (Ctrl+Z)
exit                          # remonte d'un niveau
show running-config           # config active
show startup-config           # config sauvegardée
copy running-config startup-config   # sauvegarder (write memory)
show ?                        # aide contextuelle partout
do show ip interface brief    # exécuter un show depuis le mode config
```

- Complétion avec `Tab`, rappel avec flèches haut/bas.
- `| include`, `| exclude`, `| begin`, `| section` pour filtrer les
  sorties : `show running-config | include interface`.
- `terminal length 0` : désactive la pagination (utile en script).

---

## 4. Interfaces

```shell
R-SIEGE(config)# interface GigabitEthernet 0/0
R-SIEGE(config-if)# description LIEN_FAI
R-SIEGE(config-if)# ip address 202.10.10.10 255.255.255.252
R-SIEGE(config-if)# no shutdown
R-SIEGE(config-if)# duplex full
R-SIEGE(config-if)# speed 1000
R-SIEGE(config-if)# exit

# Sous-interface 802.1Q (router-on-a-stick)
R-SIEGE(config)# interface GigabitEthernet 0/1.10
R-SIEGE(config-subif)# encapsulation dot1Q 10
R-SIEGE(config-subif)# ip address 192.168.10.1 255.255.255.0
R-SIEGE(config-subif)# exit

# Loopback (toujours UP, idéale pour router-id / management)
R-SIEGE(config)# interface Loopback 0
R-SIEGE(config-if)# ip address 10.255.255.1 255.255.255.255

# Vérifications
R-SIEGE# show ip interface brief
R-SIEGE# show interfaces GigabitEthernet 0/0
R-SIEGE# show interfaces status        # switchs
R-SIEGE# show interfaces description
```

### EtherChannel (agrégat)

```shell
R-SIEGE(config)# interface range GigabitEthernet 0/2 - 3
R-SIEGE(config-if-range)# channel-group 1 mode active    # LACP
R-SIEGE(config-if-range)# exit
R-SIEGE(config)# interface Port-channel 1
R-SIEGE(config-if)# ip address 10.0.0.1 255.255.255.252
R-SIEGE# show etherchannel summary
```

- Modes : `active`/`passive` (LACP), `desirable`/`auto` (PAgP), `on`
  (statique).

---

## 5. Switching : VLAN, trunk, STP

```shell
# VLANs
SW(config)# vlan 10
SW(config-vlan)# name COMPTA
SW(config-vlan)# exit
SW(config)# vlan 20
SW(config-vlan)# name INVITES
SW(config-vlan)# exit

# Port d'accès
SW(config)# interface GigabitEthernet 1/0/5
SW(config-if)# switchport mode access
SW(config-if)# switchport access vlan 10
SW(config-if)# spanning-tree portfast       # edge port
SW(config-if)# exit

# Trunk 802.1Q
SW(config)# interface GigabitEthernet 1/0/24
SW(config-if)# switchport mode trunk
SW(config-if)# switchport trunk allowed vlan 10,20
SW(config-if)# switchport trunk native vlan 99
SW(config-if)# exit

# Spanning Tree
SW(config)# spanning-tree mode rapid-pvst
SW(config)# spanning-tree vlan 10 priority 4096   # root primaire

# Vérifications
SW# show vlan brief
SW# show interfaces trunk
SW# show spanning-tree vlan 10
SW# show mac address-table
```

- `switchport mode dynamic desirable/auto` : négociation DTP (à éviter,
  figez en access/trunk).
- VTP : `vtp mode transparent` recommandé (évite les effacements de
  VLAN accidentels en `server`/`client`).

---

## 6. Adressage, DHCP, DNS, NTP

```shell
# DHCP serveur
R(config)# ip dhcp excluded-address 192.168.1.1 192.168.1.10
R(config)# ip dhcp pool LAN
R(dhcp-config)# network 192.168.1.0 255.255.255.0
R(dhcp-config)# default-router 192.168.1.1
R(dhcp-config)# dns-server 8.8.8.8 1.1.1.1
R(dhcp-config)# lease 3
R(dhcp-config)# exit
R# show ip dhcp binding
R# show ip dhcp pool

# DHCP client (interface WAN)
R(config)# interface GigabitEthernet 0/0
R(config-if)# ip address dhcp

# DNS / NTP
R(config)# ip name-server 8.8.8.8
R(config)# ntp server 192.168.1.100
R# show ntp status
R# ping www.google.com     # teste aussi la résolution DNS
```

---

## 7. Routage statique et par politique

```shell
R(config)# ip route 0.0.0.0 0.0.0.0 202.10.10.9
R(config)# ip route 192.168.2.0 255.255.255.0 10.0.0.2 10   # distance admin
R(config)# ipv6 route ::/0 2001:db8::1
R# show ip route
R# show ip route 8.8.8.8        # quelle route pour cette destination ?
R# show ip cef 8.8.8.8          # forwarding exact (CEF)

# PBR (Policy-Based Routing)
R(config)# access-list 100 permit ip 192.168.1.0 0.0.0.255 any
R(config)# route-map PBR1 permit 10
R(config-route-map)# match ip address 100
R(config-route-map)# set ip next-hop 10.0.0.254
R(config-route-map)# exit
R(config)# interface GigabitEthernet 0/1
R(config-if)# ip policy route-map PBR1
```

---

## 8. OSPF

```shell
R(config)# router ospf 1
R(config-router)# router-id 10.255.255.1
R(config-router)# network 192.168.1.0 0.0.0.255 area 0
R(config-router)# network 10.0.0.0 0.0.0.3 area 0
R(config-router)# passive-interface default
R(config-router)# no passive-interface GigabitEthernet 0/2
R(config-router)# exit

# Authentification MD5
R(config)# interface GigabitEthernet 0/2
R(config-if)# ip ospf authentication message-digest
R(config-if)# ip ospf message-digest-key 1 md5 CleOSPF
R(config-if)# ip ospf cost 10
R(config-if)# ip ospf priority 100

R# show ip ospf neighbor
R# show ip ospf database
R# show ip ospf interface brief
R# show ip route ospf
```

- États : Down → Init → 2-Way → ExStart → Exchange → Loading → **Full**.
- Reste en ExStart/Exchange = MTU mismatch ou duplicate router-id.

---

## 9. EIGRP et BGP

```shell
# EIGRP (named mode moderne)
R(config)# router eigrp LAN
R(config-router)# address-family ipv4 unicast autonomous-system 100
R(config-router-af)# network 192.168.1.0 0.0.0.255
R(config-router-af)# eigrp router-id 10.255.255.1
R(config-router-af)# exit
R# show ip eigrp neighbors
R# show ip eigrp topology

# BGP
R(config)# router bgp 65001
R(config-router)# bgp router-id 10.255.255.1
R(config-router)# neighbor 10.0.0.2 remote-as 65002
R(config-router)# neighbor 10.0.0.2 description FAI
R(config-router)# network 192.168.1.0 mask 255.255.255.0
R(config-router)# exit
R# show ip bgp summary
R# show ip bgp neighbors 10.0.0.2
R# show ip bgp
```

- BGP : `show ip bgp summary` — l'état doit être un nombre (préfixes),
  pas `Active`/`Idle` (problème de session).
- iBGP : full-mesh ou route-reflector ; eBGP multihop si plusieurs sauts :
  `neighbor x ebgp-multihop 2`.

---

## 10. ACL (listes de contrôle d'accès)

```shell
# Étendue numérotée
R(config)# access-list 100 permit tcp 192.168.1.0 0.0.0.255 any eq 80
R(config)# access-list 100 permit tcp 192.168.1.0 0.0.0.255 any eq 443
R(config)# access-list 100 deny ip any any log

# Nommée (recommandée : modifiable ligne par ligne)
R(config)# ip access-list extended FILTRE-WAN
R(config-ext-nacl)# permit tcp any host 192.168.50.10 eq 443
R(config-ext-nacl)# deny ip any any log
R(config-ext-nacl)# exit
R(config)# interface GigabitEthernet 0/0
R(config-if)# ip access-group FILTRE-WAN in

R# show access-lists
R# show ip access-lists FILTRE-WAN
```

- Le `deny ip any any` implicite existe toujours en fin d'ACL.
- `in`/`out` = sens par rapport au routeur ; testez dans le bon sens.
- `log` en fin de deny aide énormément au diagnostic.

---

## 11. NAT

```shell
R(config)# interface GigabitEthernet 0/0
R(config-if)# ip nat outside
R(config-if)# exit
R(config)# interface GigabitEthernet 0/1
R(config-if)# ip nat inside
R(config-if)# exit

# PAT (overload) : tout le LAN derrière l'IP WAN
R(config)# access-list 1 permit 192.168.1.0 0.0.0.255
R(config)# ip nat inside source list 1 interface GigabitEthernet 0/0 overload

# Pool de PAT
R(config)# ip nat pool PUBLIC 202.10.10.10 202.10.10.20 netmask 255.255.255.0
R(config)# ip nat inside source list 1 pool PUBLIC overload

# NAT statique (publication de serveur)
R(config)# ip nat inside source static tcp 192.168.50.10 80 202.10.10.10 8080 extendable
R(config)# ip nat inside source static 192.168.50.11 202.10.10.11   # 1:1

R# show ip nat translations
R# show ip nat statistics
R# clear ip nat translation *
```

- `inside`/`outside` inversés = NAT qui ne traduit rien (erreur n°1).
- Pour les protocoles à ports dynamiques : `ip nat service` / ALG
  (`show ip nat translations verbose`).

---

## 12. AAA, utilisateurs, SSH

```shell
# Utilisateurs locaux
R(config)# username admin privilege 15 secret MotDePasseFort
R(config)# username operateur privilege 7 secret AutreMotDePasse

# Lignes console / VTY
R(config)# line console 0
R(config-line)# password ConPass
R(config-line)# login
R(config-line)# logging synchronous
R(config-line)# exit
R(config)# line vty 0 4
R(config-line)# login local
R(config-line)# transport input ssh
R(config-line)# exec-timeout 10 0
R(config-line)# exit

# SSH
R(config)# crypto key generate rsa modulus 2048
R(config)# ip ssh version 2
R(config)# ip ssh time-out 60
R# show ip ssh
R# show users
R# show ssh
```

- `transport input ssh` seul = telnet désactivé (bien).
- AAA centralisé : `aaa new-model`, `tacacs-server host`, `radius-server
  host` (voir §20 pour l'exemple TACACS+).

---

## 13. VPN IPsec site-à-site (IKEv1)

```shell
# Phase 1
R(config)# crypto isakmp policy 10
R(config-isakmp)# encryption aes 256
R(config-isakmp)# hash sha256
R(config-isakmp)# authentication pre-share
R(config-isakmp)# group 14
R(config-isakmp)# lifetime 86400
R(config-isakmp)# exit
R(config)# crypto isakmp key ClePartagee address 202.20.20.2

# Phase 2
R(config)# crypto ipsec transform-set TS1 esp-aes 256 esp-sha256-hmac
R(cfg-crypto-trans)# mode tunnel
R(cfg-crypto-trans)# exit
R(config)# access-list 100 permit ip 192.168.1.0 0.0.0.255 192.168.2.0 0.0.0.255
R(config)# crypto map CMAP 10 ipsec-isakmp
R(config-crypto-map)# set peer 202.20.20.2
R(config-crypto-map)# set transform-set TS1
R(config-crypto-map)# set pfs group14
R(config-crypto-map)# match address 100
R(config-crypto-map)# exit
R(config)# interface GigabitEthernet 0/0
R(config-if)# crypto map CMAP

R# show crypto isakmp sa
R# show crypto ipsec sa
R# show crypto map
```

- ACL **miroir** des deux côtés (erreur n°1 de la phase 2).
- UDP 500/4500 + ESP autorisés dans les ACL d'entrée.
- `debug crypto isakmp` / `debug crypto ipsec` (avec parcimonie !).

> **Note crypto 2025** : IKEv1 + crypto maps sont **legacy**.
> Bannissez DES, 3DES, MD5, SHA-1 et les groupes DH 1/2/5 (cassés ou
> faibles). Minimum : AES-128/256, SHA-256, DH groupe 14 (2048 bits) ;
> recommandé : AES-GCM, DH groupes 19/20 (ECP). Préférez **IKEv2**
> (ci-dessous) pour tout nouveau déploiement.

### IKEv2 (moderne, recommandé)

```shell
R(config)# crypto ikev2 proposal PROP1
R(config-ikev2-proposal)# encryption aes-gcm-256
R(config-ikev2-proposal)# prf sha256
R(config-ikev2-proposal)# group 14
R(config-ikev2-proposal)# exit
R(config)# crypto ikev2 keyring KR1
R(config-ikev2-keyring)# peer PEER1
R(config-ikev2-keyring-peer)# address 202.20.20.2
R(config-ikev2-keyring-peer)# pre-shared-key local CleA
R(config-ikev2-keyring-peer)# pre-shared-key remote CleA
R(config-ikev2-keyring-peer)# exit
```

---

## 14. GRE et DMVPN (aperçu)

```shell
# GRE simple
R(config)# interface Tunnel 0
R(config-if)# ip address 172.16.0.1 255.255.255.252
R(config-if)# tunnel source GigabitEthernet 0/0
R(config-if)# tunnel destination 202.20.20.2
R(config-if)# tunnel mode gre ip

# GRE sur IPsec : appliquez la crypto map sur l'interface physique
# avec une ACL qui matche le trafic GRE (permit gre host A host B)
```

- DMVPN : hub `nhrp`, spokes dynamiques + IPsec + EIGRP/OSPF par-dessus ;
  idéal pour les réseaux d'agences (config en 3 phases : mGRE, NHRP,
  IPsec). Dites-moi si vous voulez le guide DMVPN détaillé.

---

## 15. QoS (MQC : class-map / policy-map)

```shell
R(config)# class-map match-any VOIP
R(config-cmap)# match dscp ef
R(config-cmap)# exit
R(config)# class-map match-any CRITIQUE
R(config-cmap)# match dscp af31
R(config-cmap)# match access-group 110
R(config-cmap)# exit
R(config)# policy-map QOS-WAN
R(config-pmap)# class VOIP
R(config-pmap-c)# priority percent 30
R(config-pmap-c)# exit
R(config-pmap)# class CRITIQUE
R(config-pmap-c)# bandwidth percent 40
R(config-pmap-c)# exit
R(config-pmap)# class class-default
R(config-pmap-c)# fair-queue
R(config-pmap-c)# random-detect
R(config-pmap-c)# exit
R(config-pmap)# exit
R(config)# interface GigabitEthernet 0/0
R(config-if)# service-policy output QOS-WAN

R# show policy-map interface GigabitEthernet 0/0
```

- `priority` = LLQ (temps réel), `bandwidth` = CBWFQ (garanti).
- Marquez au plus près de la source (`set dscp`), contrôlez en sortie.

---

## 16. Haute disponibilité : HSRP / VRRP / GLBP

```shell
# HSRP (Cisco propriétaire)
R(config)# interface GigabitEthernet 0/1
R(config-if)# standby 1 ip 192.168.1.254
R(config-if)# standby 1 priority 110
R(config-if)# standby 1 preempt
R(config-if)# standby 1 track GigabitEthernet 0/0 20

# VRRP (standard)
R(config-if)# vrrp 1 ip 192.168.1.254
R(config-if)# vrrp 1 priority 110
R(config-if)# vrrp 1 preempt

# GLBP (répartition de charge entre passerelles)
R(config-if)# glbp 1 ip 192.168.1.254
R(config-if)# glbp 1 priority 110
R(config-if)# glbp 1 preempt
R(config-if)# glbp 1 load-balancing round-robin

R# show standby brief
R# show vrrp brief
R# show glbp brief
```

---

## 17. Sécurité switching

```shell
# Port-security
SW(config)# interface GigabitEthernet 1/0/5
SW(config-if)# switchport port-security
SW(config-if)# switchport port-security maximum 2
SW(config-if)# switchport port-security violation restrict
SW(config-if)# switchport port-security mac-address sticky
SW# show port-security interface GigabitEthernet 1/0/5

# DHCP snooping + DAI + IP Source Guard (anti-spoofing)
SW(config)# ip dhcp snooping
SW(config)# ip dhcp snooping vlan 10
SW(config)# interface GigabitEthernet 1/0/24
SW(config-if)# ip dhcp snooping trust        # uplink uniquement !
SW(config)# ip arp inspection vlan 10
SW(config)# ip verify source vlan dhcp-snooping

# Storm control
SW(config)# interface GigabitEthernet 1/0/5
SW(config-if)# storm-control broadcast level 5.00
SW(config-if)# storm-control action trap
```

---

## 18. Supervision : syslog, SNMP, NetFlow

```shell
# Syslog
R(config)# logging host 192.168.1.100
R(config)# logging trap informational
R(config)# logging source-interface Loopback 0
R# show logging

# SNMP
R(config)# snmp-server community PublicRO RO
R(config)# snmp-server community PriveRW RW
R(config)# snmp-server host 192.168.1.100 version 2c PublicRO
R(config)# snmp-server enable traps
R(config)# snmp-server location SIEGE-Baie1
R(config)# snmp-server contact admin@entreprise.lan

# SNMPv3 (recommandé : chiffré et authentifié, remplace les community)
R(config)# snmp-server group ADMIN v3 priv
R(config)# snmp-server user operateur ADMIN v3 auth sha AuthPass priv aes 256 PrivPass
R(config)# snmp-server host 192.168.1.100 version 3 priv operateur

# NetFlow (trafic)
R(config)# interface GigabitEthernet 0/0
R(config-if)# ip flow ingress
R(config-if)# ip flow egress
R(config-if)# exit
R(config)# ip flow-export version 9
R(config)# ip flow-export destination 192.168.1.100 9996
R# show ip flow interface
R# show ip cache flow
```

---

## 19. Diagnostic

```shell
R# show version                    # modèle, IOS, uptime
R# show inventory                   # matériel / S/N
R# show processes cpu               # CPU (trié)
R# show processes cpu history       # graphique 72h
R# show memory                      # mémoire
R# show interfaces status
R# show ip interface brief
R# show interfaces Gi0/0 | include errors
R# show controllers Gi0/0           # niveau physique
R# ping 8.8.8.8 source Loopback 0
R# ping 8.8.8.8 size 1400 df-bit    # test MTU
R# traceroute 8.8.8.8
R# show ip route 8.8.8.8
R# show arp
R# show mac address-table address aabb.ccdd.eeff
R# show cdp neighbors detail        # voisins Cisco directs
R# show lldp neighbors              # standard LLDP
R# debug ip packet                  # ATTENTION : dangereux, toujours avec ACL !
R# debug ip ospf hello
R# undebug all                      # arrêter tous les debugs
R# show tech-support                # dump complet (pour le TAC)
```

- **Règle d'or du debug** : `access-list 199 permit ip host X any` puis
  `debug ip packet 199` — jamais de debug sans filtre en production.
- `service timestamps debug datetime msec` : horodate les debugs/logs.

---

## 20. AAA centralisé (TACACS+/RADIUS)

```shell
R(config)# aaa new-model
R(config)# tacacs-server host 192.168.1.50 key CleTACACS
R(config)# aaa authentication login default group tacacs+ local
R(config)# aaa authorization exec default group tacacs+ local
R(config)# aaa accounting exec default start-stop group tacacs+
R# test aaa group tacacs+ admin Cisco123 legacy   # test d'authentification
R# show aaa servers
```

- `group tacacs+ local` = fallback local si le serveur ne répond pas.
- RADIUS : `radius-server host 192.168.1.50 key CleRADIUS`.

---

## 21. Fichiers, upgrade IOS, mots de passe

```shell
R# dir flash:
R# show flash:
R# copy tftp: flash:                  # récupère l'image (nom demandé)
R# verify flash:c2900-universalk9-mz.SPA.157-3.M8.bin
R(config)# boot system flash:c2900-universalk9-mz.SPA.157-3.M8.bin
R(config)# config-register 0x2102
R# show boot
R# reload

# Sauvegarde de config sur TFTP
R# copy running-config tftp:
R# copy startup-config tftp:

# Mot de passe perdu (routeur) :
# 1. Redémarrer, Ctrl+Break -> rommon>
# 2. rommon> confreg 0x2142
# 3. rommon> reset  (démarre sans charger la startup-config)
# 4. Router# copy startup-config running-config
# 5. Changer les mots de passe, puis :
#    (config)# config-register 0x2102
#    # copy running-config startup-config + reload

# Licences (Smart Licensing, obligatoire depuis IOS XE 17.x)
R# show license status
R# show license summary
R(config)# license smart transport smart
# Plus de fichiers PAK : l'équipement s'enregistre sur le Smart Account
# (direct, via satellite on-prem, ou réservation hors-ligne).
```

- `0x2142` = ignore la startup-config ; `0x2102` = valeur normale.
- Sur switch Catalyst : mode `switch:` au boot, `flash_init`,
  `rename flash:config.text flash:config.old`, `boot`.

---

## 22. IPv6 (bases)

```shell
R(config)# ipv6 unicast-routing
R(config)# interface GigabitEthernet 0/1
R(config-if)# ipv6 address 2001:db8:1::1/64
R(config-if)# ipv6 enable
R(config)# ipv6 route ::/0 2001:db8:ff::1
R(config)# ipv6 access-list FILTRE6
R(config-ipv6-acl)# permit tcp any any eq 80
R(config-ipv6-acl)# deny ipv6 any any log
R# show ipv6 interface brief
R# show ipv6 route
R# ping ipv6 2001:db8::1
```

- OSPFv3 : `router ospfv3 1`, `address-family ipv6 unicast`.

---

## 23. Exemple complet : routeur de site

```shell
hostname R-AGENCE
no ip domain-lookup
ip domain-name agence.lan
service password-encryption
!
username admin privilege 15 secret AdminFort
!
interface GigabitEthernet 0/0
 description WAN_FAI
 ip address dhcp
 ip nat outside
 ip access-group FILTRE-WAN in
 no shutdown
!
interface GigabitEthernet 0/1
 description LAN
 ip address 192.168.10.1 255.255.255.0
 ip nat inside
 ip policy route-map PBR1
 no shutdown
!
ip dhcp excluded-address 192.168.10.1 192.168.10.10
ip dhcp pool LAN
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 8.8.8.8 1.1.1.1
!
ip access-list extended FILTRE-WAN
 permit tcp any host 202.10.10.10 eq 443
 deny ip any any log
!
access-list 1 permit 192.168.10.0 0.0.0.255
ip nat inside source list 1 interface GigabitEthernet 0/0 overload
!
ip route 0.0.0.0 0.0.0.0 GigabitEthernet 0/0 dhcp
!
router ospf 1
 router-id 10.255.10.1
 network 192.168.10.0 0.0.0.255 area 0
 passive-interface default
 no passive-interface Tunnel 0
!
line vty 0 4
 login local
 transport input ssh
!
crypto key generate rsa modulus 2048
ip ssh version 2
!
ntp server 192.168.1.100
logging host 192.168.1.100
snmp-server community PublicRO RO
!
end
copy running-config startup-config
```

---

## 24. Différences IOS XR / NX-OS (repères)

- **IOS XR** : après chaque modif, `commit` (ou `commit confirmed`) ;
  `show configuration` = changements en attente ; `show running-config`
  existe aussi. Interfaces nommées `GigabitEthernet0/0/0/0`.
- **NX-OS** : `feature ospf` / `feature bgp` / `feature hsrp` à activer
  avant usage ; `show running-config` identique ; VRF par défaut
  `management` ; `copy running-config startup-config` identique.
- Les concepts (VLAN, OSPF, BGP, NAT, IPsec) sont les mêmes, seule la
  syntaxe varie à la marge.

---

*Fin du guide. Pour aller plus loin : les Command Reference officiels
Cisco (IOS Master Command List) détaillent chaque commande avec ses
options exactes par version.*
