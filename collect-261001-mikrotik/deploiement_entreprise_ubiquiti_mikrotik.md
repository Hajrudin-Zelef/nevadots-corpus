# Déploiement entreprise — Ubiquiti & MikroTik (référence complète)

> Référence de déploiement réseau d'entreprise pour **Ubiquiti**
> (UniFi + EdgeMAX/EdgeRouter) et **MikroTik** (RouterOS) : architecture,
> plan d'adressage, configurations complètes, WAN, VPN, WiFi, QoS,
> supervision, interopérabilité, recette et mise en production.
>
> **Conventions** : `UBNT>` = EdgeOS (EdgeRouter, CLI style Vyatta),
> `MT>` = RouterOS (CLI `/...`). UniFi est piloté par **contrôleur**
> (pas de CLI significatif) : les étapes sont décrites en parcours
> d'interface. Versions visées : **EdgeOS 2.x**, **RouterOS v7.x**,
> **UniFi OS / Network 8.x-9.x**.

---

## 1. Positionnement des gammes

| Besoin | Ubiquiti | MikroTik |
|---|---|---|
| Routeur principal / edge | EdgeRouter (EdgeOS CLI) | CCR / RB5009 (RouterOS) |
| Switching managé | UniFi Switch (contrôleur) | CRS (RouterOS, SwitchOS) |
| WiFi entreprise | UniFi AP (contrôleur) | cAP / CAPsMAN (RouterOS) |
| Site simple tout-en-un | UniFi Gateway (UXG/UDM) | hAP ax / RB4011 |
| VPN multi-sites | EdgeRouter / UniFi | RouterOS (WireGuard natif) |

- **UniFi** = écosystème contrôleur : adoption, provisionnement central,
  superbe pour le WiFi et les sites simples. Le CLI y est marginal.
- **EdgeRouter** = vrai routeur CLI (EdgeOS, base Vyatta) : OSPF/BGP,
  firewall zones, QoS — le « Cisco du pauvre » en CLI.
- **MikroTik** = tout en CLI RouterOS (`/ip firewall`, `/routing`…) :
  le plus flexible et le moins cher, scripting puissant, courbe
  d'apprentissage raide.

---

## 2. Architecture de référence

```
                    ┌──────────────┐      ┌──────────────┐
                    │   FAI-1      │      │   FAI-2      │
                    └──────┬───────┘      └──────┬───────┘
                           │                    │
              ┌────────────┴────────────────────┴────────────┐
              │  EDGE : EdgeRouter-12 / CCR2116 (VRRP .253)  │
              │  NAT, firewall, BGP ou failover, VPN         │
              └────────────┬────────────────────┬────────────┘
                           │ trunk              │
              ┌────────────┴────────┐  ┌────────┴───────────┐
              │ SWITCH CORE/ACCÈS   │  │  WiFi              │
              │ UniFi Switch Pro 48 │  │  UniFi AP / cAP    │
              │ ou CRS518           │  │  (CAPsMAN ou       │
              │ VLANs 10/20/30/99   │  │   contrôleur UniFi)│
              └─────────────────────┘  └────────────────────┘
```

## Plan d'adressage (commun aux deux vendors)

| Usage | Plage |
|---|---|
| Loopbacks / router-id | 10.255.255.0/24 |
| WAN1 / WAN2 | bloc FAI |
| VLAN 10 DATA | 192.168.10.0/24, GW .254 |
| VLAN 20 VOIX | 192.168.20.0/24, GW .254 |
| VLAN 30 INVITES | 192.168.30.0/24, GW .254 |
| VLAN 40 SERVEURS | 192.168.40.0/24, GW .254 |
| VLAN 99 MGMT | 192.168.99.0/24, GW .254 |
| VPN WireGuard | 10.8.0.0/24 |

---

## 3. Socle management

### 3.1 EdgeRouter — socle (EdgeOS)

```shell
UBNT> configure
UBNT# set system host-name ER-EDGE-01
UBNT# set system domain-name entreprise.lan
UBNT# set system login user admin authentication plaintext-password AdminFort2026
UBNT# set system login user admin level admin
UBNT# set service ssh port 22
UBNT# set service ssh disable-password-authentication
UBNT# set system ntp server 192.168.99.10
UBNT# set system ntp server 192.168.99.11
UBNT# set system syslog host 192.168.99.50 facility all level info
UBNT# set system time-zone UTC
UBNT# set snmp community NOC authorization ro
UBNT# set snmp contact noc@entreprise.lan
UBNT# set snmp location DATACENTER-RangA12
UBNT# commit
UBNT# save
UBNT# exit
UBNT> show version
```

### 3.2 MikroTik — socle (RouterOS v7)

```shell
MT> /system identity set name=MT-EDGE-01
MT> /user add name=admin group=full password=AdminFort2026
MT> /user disable admin
MT> /ip service disable telnet,ftp,www,api
MT> /ip service set ssh port=22
MT> /ip service set winbox address=192.168.99.0/24
MT> /system ntp client set enabled=yes
MT> /system ntp client servers add address=192.168.99.10
MT> /system ntp client servers add address=192.168.99.11
MT> /system clock set time-zone-name=UTC
MT> /system logging action add name=SIEM target=remote remote=192.168.99.50
MT> /system logging add topics=info action=SIEM
MT> /snmp set enabled=yes contact="noc@entreprise.lan" location="DATACENTER-RangA12"
MT> /snmp community set name=public addresses=192.168.99.50/32 security=authorized read-access=yes
MT> /export file=socle-avant-prod
MT> /system backup save name=socle-avant-prod
```

> **Premier réflexe MikroTik** : `/export` (config lisible) + `/system
> backup save` (binaire). Les deux, à chaque étape qui marche.

---

## 4. Ubiquiti UniFi — déploiement via contrôleur

### 4.1 Prise en main
1. Installer le contrôleur : **UniFi OS** (Cloud Key / UDM / UXG) ou
   self-hosted (Docker/serveur).
2. Adopter les équipements : Devices > **Adopt** (ils doivent être en
   usine et joignables en L2/L3 ; sinon adoption manuelle par SSH :
   `set-inform http://<controleur>:8080/inform`).
3. **Settings > System** : backup auto du contrôleur, mises à jour
   planifiées (jamais en heures ouvrées).

### 4.2 Réseaux (VLANs)
4. **Settings > Networks > Create New** :
   - `DATA` : VLAN 10, `192.168.10.1/24`, DHCP `192.168.10.100–200`.
   - `VOIX` : VLAN 20, `192.168.20.1/24`.
   - `INVITES` : VLAN 30, `192.168.30.1/24` (+ **Guest isolation** /
     portail captif si besoin : Settings > Hotspot).
   - `SERVEURS` : VLAN 40, `192.168.40.1/24`, DHCP avec réservations.
   - `MGMT` : VLAN 99, `192.168.99.1/24`.
5. **Devices > Switch > Ports** : profils de ports — `All` (trunk) sur
   les uplinks, `DATA` (access) sur les ports utilisateurs, `VOIX`
   en voice VLAN sur les ports téléphonie.

### 4.3 WiFi
6. **Settings > WiFi > Create New** :
   - `CORP` : WPA2/3 Enterprise (RADIUS vers NPS/FreeRADIUS) ou PSK
     fort, VLAN 10, bandes 5 GHz優先.
   - `INVITES` : portail captif + VLAN 30 + **client isolation** +
     limite de débit.
7. **Radio management** : laissez l'optimisation auto (canaux, puissance)
   sauf site dense → plan manuel (1/6/11 en 2.4 GHz).

### 4.4 Firewall (UniFi Gateway)
8. **Settings > Security > Firewall Rules** :
   - LAN IN : autoriser DATA→SERVEURS (ports utiles), **refuser
     INVITES→RFC1918** (règle explicite, placée avant l'autorisation
     Internet).
   - GUEST IN : règles du portail invité (auto).
   - WAN IN : **rien** sauf VPN (WireGuard/OpenVPN) et port-forwards
     nécessaires.
9. **Port Forwarding** : WAN 443 → serveur DMZ (avec règle WAN IN
   associée auto).
10. **Traffic & Firewall Rules > Ad Blocking / GeoIP filtering** :
    activer selon politique.

### 4.5 VPN (UniFi)
11. **Settings > VPN** :
    - **WireGuard** : serveur, pairs (télétravailleurs), AllowedIPs.
    - **OpenVPN** : serveur road-warrior (fichiers `.ovpn` exportables).
    - **Site-to-Site IPsec** : vers les agences (PSK ou certificats),
      statut dans **Insights > VPN**.
12. **Teleport / Identity** (selon licence) : accès distant simplifié.

### 4.6 Supervision UniFi
- **Insights** : clients, débits, DPI (applications), alertes.
- **Alerts** : configurer les notifications (gateway down, rogue AP).
- Logs : **System Logs** filtrables par équipement.

---

## 5. EdgeRouter — déploiement complet en CLI

### 5.1 Interfaces, VLANs, bonding

```shell
UBNT> configure
! --- WAN ---
UBNT# set interfaces ethernet eth0 address dhcp
UBNT# set interfaces ethernet eth0 description WAN1-FAI-FIBRE
UBNT# set interfaces ethernet eth1 address 198.51.100.50/29
UBNT# set interfaces ethernet eth1 description WAN2-FAI-SECOURS
! --- LAN trunk vers switch ---
UBNT# set interfaces ethernet eth2 description TRUNK-SWITCH
! --- VLANs (VIF) ---
UBNT# set interfaces ethernet eth2 vif 10 address 192.168.10.254/24
UBNT# set interfaces ethernet eth2 vif 10 description VLAN10-DATA
UBNT# set interfaces ethernet eth2 vif 20 address 192.168.20.254/24
UBNT# set interfaces ethernet eth2 vif 20 description VLAN20-VOIX
UBNT# set interfaces ethernet eth2 vif 30 address 192.168.30.254/24
UBNT# set interfaces ethernet eth2 vif 30 description VLAN30-INVITES
UBNT# set interfaces ethernet eth2 vif 40 address 192.168.40.254/24
UBNT# set interfaces ethernet eth2 vif 40 description VLAN40-SERVEURS
UBNT# set interfaces ethernet eth2 vif 99 address 192.168.99.254/24
UBNT# set interfaces ethernet eth2 vif 99 description VLAN99-MGMT
! --- Bonding LACP (si 2 uplinks physiques) ---
UBNT# set interfaces bonding bond0 mode 802.3ad
UBNT# set interfaces bonding bond0 member interface eth3
UBNT# set interfaces bonding bond0 member interface eth4
UBNT# set interfaces bonding bond0 vif 10 address 192.168.10.253/24
UBNT# commit
UBNT# save
```

### 5.2 Firewall policies (zones)

```shell
UBNT> configure
! --- Politique LAN vers WAN ---
UBNT# set firewall name LAN-WAN default-action accept
UBNT# set firewall name LAN-WAN description "LAN vers Internet"
! --- Politique INVITES : Internet seul ---
UBNT# set firewall name GUEST-WAN default-action drop
UBNT# set firewall name GUEST-WAN rule 10 action accept
UBNT# set firewall name GUEST-WAN rule 10 description "DNS"
UBNT# set firewall name GUEST-WAN rule 10 destination port 53
UBNT# set firewall name GUEST-WAN rule 10 protocol udp
UBNT# set firewall name GUEST-WAN rule 20 action accept
UBNT# set firewall name GUEST-WAN rule 20 description "Web"
UBNT# set firewall name GUEST-WAN rule 20 destination port 80,443
UBNT# set firewall name GUEST-WAN rule 20 protocol tcp
UBNT# set firewall name GUEST-WAN rule 30 action drop
UBNT# set firewall name GUEST-WAN rule 30 description "Bloque RFC1918"
UBNT# set firewall name GUEST-WAN rule 30 destination address 10.0.0.0/8
UBNT# set firewall name GUEST-WAN rule 31 action drop
UBNT# set firewall name GUEST-WAN rule 31 destination address 172.16.0.0/12
UBNT# set firewall name GUEST-WAN rule 32 action drop
UBNT# set firewall name GUEST-WAN rule 32 destination address 192.168.0.0/16
! --- WAN vers LAN : tout bloqué par défaut ---
UBNT# set firewall name WAN-LAN default-action drop
UBNT# set firewall name WAN-LAN rule 10 action accept
UBNT# set firewall name WAN-LAN rule 10 state established enable
UBNT# set firewall name WAN-LAN rule 10 state related enable
UBNT# set firewall name WAN-LAN rule 20 action drop
UBNT# set firewall name WAN-LAN rule 20 state invalid enable
! --- Application aux interfaces ---
UBNT# set interfaces ethernet eth2 vif 30 firewall in name GUEST-WAN
UBNT# set interfaces ethernet eth2 vif 30 firewall local name WAN-LAN
UBNT# set interfaces ethernet eth0 firewall in name WAN-LAN
UBNT# set interfaces ethernet eth0 firewall local name WAN-LOCAL
UBNT# commit
UBNT# save
UBNT> show firewall
```

### 5.3 NAT

```shell
UBNT> configure
! --- Masquerade (PAT) ---
UBNT# set service nat rule 5000 description "PAT LAN vers WAN1"
UBNT# set service nat rule 5000 outbound-interface eth0
UBNT# set service nat rule 5000 source address 192.168.0.0/16
UBNT# set service nat rule 5000 type masquerade
! --- Port-forward ---
UBNT# set service nat rule 10 description "HTTPS vers DMZ"
UBNT# set service nat rule 10 inbound-interface eth0
UBNT# set service nat rule 10 destination port 443
UBNT# set service nat rule 10 protocol tcp
UBNT# set service nat rule 10 inside-address address 192.168.40.10
UBNT# set service nat rule 10 inside-address port 443
UBNT# set service nat rule 10 type destination
! --- Règle firewall associée (WAN-LAN) ---
UBNT# set firewall name WAN-LAN rule 30 action accept
UBNT# set firewall name WAN-LAN rule 30 destination address 192.168.40.10
UBNT# set firewall name WAN-LAN rule 30 destination port 443
UBNT# set firewall name WAN-LAN rule 30 protocol tcp
UBNT# commit
UBNT# save
UBNT> show nat rules
UBNT> show nat translations
```

### 5.4 DHCP + DNS

```shell
UBNT> configure
UBNT# set service dhcp-server disabled false
UBNT# set service dhcp-server shared-network-name DATA subnet 192.168.10.0/24 default-router 192.168.10.254
UBNT# set service dhcp-server shared-network-name DATA subnet 192.168.10.0/24 dns-server 192.168.10.254
UBNT# set service dhcp-server shared-network-name DATA subnet 192.168.10.0/24 range 0 start 192.168.10.100
UBNT# set service dhcp-server shared-network-name DATA subnet 192.168.10.0/24 range 0 stop 192.168.10.200
UBNT# set service dhcp-server shared-network-name DATA subnet 192.168.10.0/24 static-mapping SRV-WEB ip-address 192.168.40.10
UBNT# set service dhcp-server shared-network-name DATA subnet 192.168.10.0/24 static-mapping SRV-WEB mac-address AA:BB:CC:DD:EE:FF
UBNT# set service dns forwarding listen-on eth2.10
UBNT# set service dns forwarding listen-on eth2.20
UBNT# set service dns forwarding name-server 9.9.9.9
UBNT# set service dns forwarding name-server 1.1.1.1
UBNT# commit
UBNT# save
UBNT> show dhcp leases
```

### 5.5 Routage : OSPF + failover WAN

```shell
UBNT> configure
! --- OSPF interne ---
UBNT# set protocols ospf area 0 network 192.168.0.0/16
UBNT# set protocols ospf area 0 network 10.0.0.0/8
UBNT# set protocols ospf parameters router-id 10.255.255.1
UBNT# set protocols ospf passive-interface eth2.10
UBNT# set protocols ospf passive-interface eth2.20
! --- Failover WAN (load-balance avec health-check) ---
UBNT# set load-balance group WAN-FO interface eth0
UBNT# set load-balance group WAN-FO interface eth1
UBNT# set load-balance group WAN-FO interface eth0 route-test type ping target 9.9.9.9
UBNT# set load-balance group WAN-FO interface eth1 route-test type ping target 1.1.1.1
UBNT# set protocols static table 1 route 0.0.0.0/0 next-hop <GW-WAN1>
UBNT# set protocols static table 2 route 0.0.0.0/0 next-hop <GW-WAN2>
UBNT# set firewall modify WAN-RULE rule 10 action modify
UBNT# set firewall modify WAN-RULE rule 10 modify table main
UBNT# set firewall modify WAN-RULE rule 10 source address 192.168.0.0/16
UBNT# set interfaces ethernet eth2 vif 10 firewall in modify WAN-RULE
UBNT# commit
UBNT# save
UBNT> show ip ospf neighbor
UBNT> show load-balance status
```

### 5.6 VPN : IPsec site-à-site + WireGuard

```shell
UBNT> configure
! --- IPsec IKEv2 vers agence ---
UBNT# set vpn ipsec ipsec-interfaces interface eth0
UBNT# set vpn ipsec nat-traversal enable
UBNT# set vpn ipsec ike-group IKE-AGENCE proposal 1 encryption aes256
UBNT# set vpn ipsec ike-group IKE-AGENCE proposal 1 hash sha256
UBNT# set vpn ipsec ike-group IKE-AGENCE proposal 1 dh-group 14
UBNT# set vpn ipsec esp-group ESP-AGENCE proposal 1 encryption aes256
UBNT# set vpn ipsec esp-group ESP-AGENCE proposal 1 hash sha256
UBNT# set vpn ipsec site-to-site peer 198.51.100.2 authentication mode pre-shared-secret
UBNT# set vpn ipsec site-to-site peer 198.51.100.2 authentication pre-shared-secret CleIPsec
UBNT# set vpn ipsec site-to-site peer 198.51.100.2 ike-group IKE-AGENCE
UBNT# set vpn ipsec site-to-site peer 198.51.100.2 tunnel 1 esp-group ESP-AGENCE
UBNT# set vpn ipsec site-to-site peer 198.51.100.2 tunnel 1 local prefix 192.168.0.0/16
UBNT# set vpn ipsec site-to-site peer 198.51.100.2 tunnel 1 remote prefix 192.168.100.0/24
UBNT# commit
UBNT# save
UBNT> show vpn ipsec sa
```

> WireGuard sur EdgeOS : via `set interfaces wireguard wg0 …`
> (EdgeOS 2.x) — même logique que RouterOS §6.5.

### 5.7 QoS (traffic-policy shaper)

```shell
UBNT> configure
UBNT# set traffic-policy shaper WAN-OUT bandwidth 1000mbit
UBNT# set traffic-policy shaper WAN-OUT class 10 bandwidth 30%
UBNT# set traffic-policy shaper WAN-OUT class 10 priority 7
UBNT# set traffic-policy shaper WAN-OUT class 10 match VOIP ip dscp ef
UBNT# set traffic-policy shaper WAN-OUT class 20 bandwidth 40%
UBNT# set traffic-policy shaper WAN-OUT class 20 match CRIT ip dscp af31
UBNT# set traffic-policy shaper WAN-OUT default bandwidth 30%
UBNT# set traffic-policy shaper WAN-OUT default queue-type fair-queue
UBNT# set interfaces ethernet eth0 traffic-policy out WAN-OUT
UBNT# commit
UBNT# save
```

### 5.8 VRRP (HA EdgeRouter)

```shell
UBNT> configure
UBNT# set interfaces ethernet eth2 vif 10 vrrp vrrp-group 10 virtual-address 192.168.10.254/24
UBNT# set interfaces ethernet eth2 vif 10 vrrp vrrp-group 10 priority 200
UBNT# set interfaces ethernet eth2 vif 10 vrrp vrrp-group 10 preempt true
UBNT# set interfaces ethernet eth2 vif 10 vrrp vrrp-group 10 sync-group LAN-HA
UBNT# commit
UBNT# save
UBNT> show vrrp
```

---

## 6. MikroTik RouterOS — déploiement complet en CLI

### 6.1 Interfaces : bridge, VLANs, bonding

```shell
MT> /interface bridge add name=bridge-trunk vlan-filtering=yes comment="Trunk vers switch"
MT> /interface bridge port add bridge=bridge-trunk interface=ether2 comment="Uplink switch"
MT> /interface bridge vlan add bridge=bridge-trunk vlan-ids=10 tagged=bridge-trunk,ether2
MT> /interface bridge vlan add bridge=bridge-trunk vlan-ids=20 tagged=bridge-trunk,ether2
MT> /interface bridge vlan add bridge=bridge-trunk vlan-ids=30 tagged=bridge-trunk,ether2
MT> /interface bridge vlan add bridge=bridge-trunk vlan-ids=40 tagged=bridge-trunk,ether2
MT> /interface bridge vlan add bridge=bridge-trunk vlan-ids=99 tagged=bridge-trunk,ether2
MT> /interface vlan add name=vlan10-data vlan-id=10 interface=bridge-trunk
MT> /interface vlan add name=vlan20-voix vlan-id=20 interface=bridge-trunk
MT> /interface vlan add name=vlan30-invites vlan-id=30 interface=bridge-trunk
MT> /interface vlan add name=vlan40-srv vlan-id=40 interface=bridge-trunk
MT> /interface vlan add name=vlan99-mgmt vlan-id=99 interface=bridge-trunk
MT> /ip address add address=192.168.10.254/24 interface=vlan10-data
MT> /ip address add address=192.168.20.254/24 interface=vlan20-voix
MT> /ip address add address=192.168.30.254/24 interface=vlan30-invites
MT> /ip address add address=192.168.40.254/24 interface=vlan40-srv
MT> /ip address add address=192.168.99.254/24 interface=vlan99-mgmt
MT> /ip address add address=10.255.255.1/32 interface=lo comment="Loopback"
MT> /interface bonding add name=bond-uplink slaves=ether3,ether4 mode=802.3ad comment="LACP"
```

> **VLAN filtering sur bridge** (RouterOS v7) : la méthode moderne et
> performante. L'ancienne méthode « un bridge par VLAN » est dépréciée.

### 6.2 Firewall filter (pare-feu)

```shell
MT> /ip firewall filter add chain=input action=accept connection-state=established,related comment="Retours"
MT> /ip firewall filter add chain=input action=drop connection-state=invalid
MT> /ip firewall filter add chain=input action=accept protocol=icmp comment="Ping diag"
MT> /ip firewall filter add chain=input action=accept src-address=192.168.99.0/24 comment="MGMT"
MT> /ip firewall filter add chain=input action=accept in-interface=vlan10-data dst-port=53 protocol=udp comment="DNS LAN"
MT> /ip firewall filter add chain=input action=drop comment="Tout le reste en input"
MT>
MT> /ip firewall filter add chain=forward action=accept connection-state=established,related
MT> /ip firewall filter add chain=forward action=drop connection-state=invalid
MT> /ip firewall filter add chain=forward action=accept in-interface=vlan10-data out-interface=ether1-WAN comment="DATA vers Internet"
MT> /ip firewall filter add chain=forward action=accept in-interface=vlan30-invites out-interface=ether1-WAN dst-port=80,443 protocol=tcp comment="Invites web seul"
MT> /ip firewall filter add chain=forward action=drop in-interface=vlan30-invites dst-address=192.168.0.0/16 comment="Invites isoles du LAN"
MT> /ip firewall filter add chain=forward action=accept in-interface=vlan10-data out-interface=vlan40-srv comment="DATA vers serveurs"
MT> /ip firewall filter add chain=forward action=drop comment="Reste du forward"
MT> /ip firewall filter print
```

### 6.3 NAT

```shell
MT> /ip firewall nat add chain=srcnat action=masquerade out-interface=ether1-WAN comment="PAT"
MT> /ip firewall nat add chain=dstnat action=dst-nat to-addresses=192.168.40.10 to-ports=443 protocol=tcp dst-port=443 in-interface=ether1-WAN comment="HTTPS vers DMZ"
MT> /ip firewall nat print
MT> /ip firewall connection print where src-address~"192.168.10."
```

### 6.4 Address-lists (l'équivalent des aliases)

```shell
MT> /ip firewall address-list add list=RFC1918 address=10.0.0.0/8
MT> /ip firewall address-list add list=RFC1918 address=172.16.0.0/12
MT> /ip firewall address-list add list=RFC1918 address=192.168.0.0/16
MT> /ip firewall address-list add list=BOGONS address=0.0.0.0/8
MT> /ip firewall address-list add list=BOGONS address=169.254.0.0/16
MT> /ip firewall filter add chain=forward action=drop in-interface=vlan30-invites dst-address-list=RFC1918
```

### 6.5 DHCP + DNS

```shell
MT> /ip pool add name=pool-data ranges=192.168.10.100-192.168.10.200
MT> /ip pool add name=pool-invites ranges=192.168.30.100-192.168.30.200
MT> /ip dhcp-server add name=dhcp-data interface=vlan10-data address-pool=pool-data
MT> /ip dhcp-server network add address=192.168.10.0/24 gateway=192.168.10.254 dns-server=192.168.10.254
MT> /ip dhcp-server add name=dhcp-invites interface=vlan30-invites address-pool=pool-invites
MT> /ip dhcp-server network add address=192.168.30.0/24 gateway=192.168.30.254 dns-server=192.168.30.254
MT> /ip dhcp-server lease add address=192.168.40.10 mac-address=AA:BB:CC:DD:EE:FF comment="SRV-WEB"
MT> /ip dhcp-server enable dhcp-data,dhcp-invites
MT> /ip dns set servers=9.9.9.9,1.1.1.1 allow-remote-requests=yes
MT> /ip dhcp-server lease print
```

### 6.6 Routage : statique, OSPF, BGP (v7)

```shell
MT> /ip route add dst-address=0.0.0.0/0 gateway=203.0.113.9 comment="Defaut FAI-1"
! --- Failover WAN par routage récursif (méthode éprouvée) ---
MT> /ip route add dst-address=9.9.9.9/32 gateway=203.0.113.9 scope=10 comment="Check FAI-1"
MT> /ip route add dst-address=0.0.0.0/0 gateway=9.9.9.9 check-gateway=ping distance=1 comment="Defaut via check"
MT> /ip route add dst-address=1.1.1.1/32 gateway=198.51.100.1 scope=10 comment="Check FAI-2"
MT> /ip route add dst-address=0.0.0.0/0 gateway=1.1.1.1 check-gateway=ping distance=2 comment="Secours"
! --- OSPF v7 ---
MT> /routing ospf instance add name=ospf1 router-id=10.255.255.1
MT> /routing ospf area add name=backbone area-id=0.0.0.0 instance=ospf1
MT> /routing ospf interface-template add networks=192.168.0.0/16 area=backbone
MT> /routing ospf interface-template add networks=10.0.0.0/8 area=backbone
MT> /routing ospf neighbor print
! --- BGP v7 ---
MT> /routing bgp connection add name=FAI1 remote.address=203.0.113.9 remote.as=100 local.address=203.0.113.10 local.as=65001
MT> /routing bgp template set default as=65001 router-id=10.255.255.1
MT> /routing filter rule add chain=bgp-out rule="if (dst in 192.168.0.0/16) {accept} else {reject}"
MT> /routing bgp connection print
```

### 6.7 VPN : WireGuard (natif v7) + IPsec

```shell
MT> /interface wireguard add name=wg0 listen-port=51820 private-key="<cle-privee>"
MT> /ip address add address=10.8.0.1/24 interface=wg0
MT> /interface wireguard peers add interface=wg0 public-key="<cle-publique-client>" allowed-address=10.8.0.2/32 persistent-keepalive=25s comment="Teletravailleur-1"
MT> /ip firewall filter add chain=input action=accept protocol=udp dst-port=51820 comment="WireGuard"
MT> /ip firewall filter add chain=forward action=accept in-interface=wg0 out-interface=vlan10-data
MT> /interface wireguard print
MT> /interface wireguard peers print
! --- IPsec site-à-site (IKEv2) ---
MT> /ip ipsec profile add name=prof-agence hash-algorithm=sha256 enc-algorithm=aes-256 dh-group=modp2048
MT> /ip ipsec peer add name=peer-agence address=198.51.100.2/32 profile=prof-agence auth-method=pre-shared-key secret="CleIPsec"
MT> /ip ipsec proposal add name=prop-agence auth-algorithms=sha256 enc-algorithms=aes-256-cbc
MT> /ip ipsec policy add src-address=192.168.0.0/16 dst-address=192.168.100.0/24 peer=peer-agence proposal=prop-agence tunnel=yes
MT> /ip ipsec active-peers print
```

### 6.8 VRRP (HA)

```shell
MT> /interface vrrp add name=vrrp-data interface=vlan10-data vrid=10 priority=200 preemption-mode=yes
MT> /ip address add address=192.168.10.254/24 interface=vrrp-data
MT> /interface vrrp print
! (second nœud : priority=100, même vrid, sync via /system backup + export)
```

### 6.9 QoS : simple queues + queue tree (PCQ)

```shell
! --- Simple : limiter un client ---
MT> /queue simple add name=clientA target=192.168.10.50/32 max-limit=50M/50M
! --- Queue tree : prioriser la VoIP (avec mangle) ---
MT> /ip firewall mangle add chain=forward action=mark-packet new-packet-mark=voip dscp=46 comment="Marque VoIP"
MT> /queue tree add name=q-voip parent=global packet-mark=voip priority=1 max-limit=30M
MT> /queue tree add name=q-defaut parent=global priority=8 max-limit=1000M
MT> /queue tree print
! --- PCQ : équité entre N utilisateurs ---
MT> /queue type add name=pcq-down kind=pcq pcq-classifier=dst-address pcq-rate=20M
MT> /queue simple add name=partage target=192.168.30.0/24 queue=pcq-down/pcq-down max-limit=100M/100M
```

### 6.10 WiFi : CAPsMAN (contrôleur intégré)

```shell
MT> /interface wifi capsman set enabled=yes
MT> /interface wifi provisioning add master-configuration=cfg-corp slave-configurations=cfg-invites
MT> /interface wifi configuration add name=cfg-corp ssid=CORP security.authentication-types=wpa2-psk,wpa3-psk security.passphrase="MotDePasseFort" datapath.vlan-id=10
MT> /interface wifi configuration add name=cfg-invites ssid=INVITES security.authentication-types=wpa2-psk security.passphrase="Invite2026" datapath.vlan-id=30
MT> /interface wifi capsman remote-cap add address=192.168.99.0/24
MT> /interface wifi registration-table print
```

### 6.11 Outils de diagnostic MikroTik

```shell
MT> /ping 8.8.8.8 count=5
MT> /tool traceroute 8.8.8.8
MT> /tool torch interface=ether1-WAN
MT> /tool sniffer quick interface=vlan10-data ip-address=192.168.10.50
MT> /tool profile
MT> /interface print stats
MT> /ip firewall connection print count-only
MT> /log print where topics~"firewall"
MT> /system resource print
MT> /system health print
```

### 6.12 Scripts & automatisation

```shell
MT> /system script add name=backup-quotidien source="/system backup save name=auto-backup; /export file=auto-export"
MT> /system scheduler add name=sched-backup on-event=backup-quotidien start-time=03:00:00 interval=1d
MT> /tool netwatch add host=9.9.9.9 interval=30s up-script="/log info \"FAI-1 OK\"" down-script="/log warning \"FAI-1 DOWN\""
MT> /system script print
```

---

## 7. WiFi entreprise — comparatif

| Aspect | UniFi (contrôleur) | MikroTik CAPsMAN |
|---|---|---|
| Gestion | GUI centralisée, adoption auto | CLI `/interface wifi` |
| SSID/VLAN | Profils réseau en 2 clics | `datapath.vlan-id` |
| Portail invité | Hotspot intégré + vouchers | Hotspot `/ip hotspot` |
| Roaming | 802.11r/k/v auto | 802.11r à configurer |
| Idéal pour | Simplicité, sites multi-AP | Budget, contrôle fin |

---

## 8. Sécurité : durcissement

### Checklist commune
- [ ] Services inutiles désactivés (telnet/ftp/www/api ouverts au monde).
- [ ] Admin par comptes nominatifs, mots de passe forts, SSH par clés.
- [ ] Firewall input : **drop par défaut**, n'ouvrir que MGMT + services.
- [ ] WinBox : restreint au MGMT (`/ip service set winbox address=…`),
      jamais exposé sur WAN (vecteur d'attaque historique).
- [ ] Mises à jour : RouterOS **stable** (pas de testing en prod),
      EdgeOS : vérifier les CVE.
- [ ] Backups chiffrés + export, hors-site, test de restore.

### MikroTik — protections spécifiques
```shell
MT> /ip firewall filter add chain=input action=add-src-to-address-list address-list=port-scanners address-list-timeout=2w in-interface=ether1-WAN psd=21,3s,3,1 comment="Detecte scan"
MT> /ip firewall filter add chain=input action=drop src-address-list=port-scanners
MT> /ip ssh set strong-crypto=yes
MT> /ip neighbor discovery-settings set discover-interface-list=MGMT
```

---

## 9. Supervision

| Outil | Ubiquiti | MikroTik |
|---|---|---|
| SNMP | UniFi : limité ; EdgeRouter : complet | SNMP complet (v1/2c/3) |
| Syslog | EdgeOS/UniFi → SIEM | `/system logging` → SIEM |
| Natif | UniFi Insights / DPI | **The Dude** (cartographie, sondes) |
| NetFlow | EdgeRouter : `set system flow-accounting` | Traffic Flow `/ip traffic-flow` → collecteur |

```shell
! EdgeRouter — flow accounting
UBNT> configure
UBNT# set system flow-accounting interface eth0
UBNT# set system flow-accounting netflow server 192.168.99.70 port 9996
UBNT# set system flow-accounting netflow version 9
UBNT# commit; save
```

```shell
! MikroTik — Traffic Flow
MT> /ip traffic-flow set enabled=yes interfaces=ether1-WAN
MT> /ip traffic-flow target add dst-address=192.168.99.70 port=9996 version=9
```

---

## 10. Interopérabilité Ubiquiti ↔ MikroTik

| Protocole | État | Notes |
|---|---|---|
| Trunk 802.1Q | ✅ | PVID/native cohérents des 2 côtés |
| LACP 802.3ad | ✅ | `mode 802.3ad` ↔ `mode=802.3ad` |
| RSTP | ✅ | Bridge MikroTik : `stp=yes`, même priority root |
| OSPF | ✅ | Mêmes timers, MTU identique |
| BGP | ✅ | eBGP standard |
| VRRP | ✅ | Même VRID ; préemption cohérente |
| WireGuard | ✅ | Clés/AllowedIPs miroir, keepalive 25 s |
| IPsec IKEv2 | ✅ | Proposals identiques (AES-256/SHA-256/DH14) |
| DHCP | ⚠️ | Un seul serveur par VLAN (sinon conflits) |

---

## 11. Checklist de mise en production

### Avant le jour J
- [ ] Configs exportées : EdgeRouter `show configuration` ; MikroTik
      `/export` + `/system backup save` — hors-site.
- [ ] Plan de rollback écrit (Netinstall MikroTik / reset EdgeRouter
      en dernier recours).
- [ ] Accès console/SSH testés sur chaque équipement.
- [ ] Firmware : version **stable** validée (pas de release candidate).
- [ ] Étiquetage des câbles et ports.

### Recette
- [ ] Chaque VLAN : DHCP, passerelle, Internet, isolation conformes.
- [ ] NAT : `show nat translations` / `/ip firewall connection` OK.
- [ ] Failover WAN : débrancher FAI-1 → bascule, rebrancher → retour.
- [ ] VRRP : extinction du master → VIP sur le backup < 10 s.
- [ ] VPN : WireGuard handshake récent, IPsec SA établies, trafic OK.
- [ ] WiFi : roaming entre AP, débit, portail invité.
- [ ] QoS : appel VoIP clair pendant saturation du lien.
- [ ] Firewall : scan nmap depuis WAN → bloqué + loggé.
- [ ] Supervision : SNMP/syslog/NetFlow reçus par les collecteurs.
- [ ] NTP synchronisé partout.

### Ordre de déploiement
1. Edge/routeurs → 2. Switchs/VLANs → 3. WiFi → 4. Firewall/NAT →
   5. VPN → 6. QoS → 7. Supervision. Valider chaque couche avant la suivante.

---

## 12. Maintenance

- **MikroTik** : rester sur la branche **stable**, lire le changelog
  (les upgrades RouterOS sont rapides mais parfois cassants sur les
  configs exotiques) ; Netinstall en plan B ultime.
- **EdgeRouter** : upgrades moins fréquents, sauvegarder `config.boot`
  avant (`cp /config/config.boot /config/config.boot.bak`).
- **UniFi** : contrôleur backupé avant chaque upgrade ; ne pas upgrader
  contrôleur + firmwares AP le même jour en prod.
- **Trimestriel** : comptes inactifs, règles firewall obsolètes,
  certificats, capacité (ports, CPU), test de restore backup.

---

*Fin de la référence. Compagnons : [Déploiement Cisco & Huawei](deploiement_entreprise_cisco_huawei.md)
(architecture 3-tier détaillée), [Guide CLI OPNsense](opnsense_cli_guide.md),
[Guide web OPNsense](opnsense_gui_guide.md), [Scénarios OPNsense](opnsense_deploiements.md).*
