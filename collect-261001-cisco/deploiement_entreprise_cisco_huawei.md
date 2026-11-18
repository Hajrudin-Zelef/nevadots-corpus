# Déploiement entreprise — Cisco & Huawei (référence complète)

> Référence de déploiement réseau d'entreprise couvrant **tous les
> aspects** : architecture, adressage, configuration complète par
> couche (core / distribution / accès), WAN, haute disponibilité,
> sécurité, QoS, supervision, interopérabilité Cisco ↔ Huawei,
> recette et mise en production.
>
> **Conventions** : `CISCO>` = IOS/IOS XE, `HUAWEI>` = VRP (Comware-like
> des switchs S / routeurs AR, syntaxe vue dans vos Command Reference).
> Les exemples forment un tout cohérent : même plan d'adressage des
> deux côtés pour faciliter la comparaison et l'interopérabilité.

---

## 1. Architecture de référence

```
                    ┌──────────────┐      ┌──────────────┐
                    │   FAI-1      │      │   FAI-2      │
                    │ AS 100       │      │ AS 200       │
                    └──────┬───────┘      └──────┬───────┘
                           │ eBGP               │ eBGP
                    ┌──────┴────────────────────┴──────┐
                    │        EDGE (routeurs)           │
                    │   AS 65001 — BGP + NAT + IPsec   │
                    └──────┬────────────────────┬──────┘
                           │                    │
                    ┌──────┴──────┐      ┌───────┴──────┐
                    │  FIREWALL   │      │  DMZ         │
                    │  (USG/pair) │      │  serveurs    │
                    └──────┬──────┘      │  publics     │
                           │            └──────────────┘
              ┌────────────┴────────────┐
              │      CORE (L3)          │  2 switchs, VRRP/HSRP,
              │  10.0.0.0/30 transit   │  OSPF area 0
              └────┬──────────────┬─────┘
                   │ Eth-Trunk/LACP      │ Eth-Trunk/LACP
        ┌──────────┴────────┐  ┌─────────┴─────────┐
        │ DISTRIBUTION-BAT-A │  │ DISTRIBUTION-BAT-B │  routage inter-VLAN,
        │  OSPF area 0       │  │  OSPF area 0        │  STP, DHCP snooping
        └────┬──────────┬───┘  └────┬──────────┬───┘
             │          │           │          │
        ┌────┴───┐ ┌────┴───┐  ┌────┴───┐ ┌────┴───┐
        │ ACCESS │ │ ACCESS │  │ ACCESS │ │  WiFi  │  port-security,
        │  Sw-1  │ │  Sw-2  │  │  Sw-3  │ │  (AP)  │  802.1X, PoE
        └────────┘ └────────┘  └────────┘ └────────┘
```

- **Core** : commutation L3 rapide, aucune ACL lourde, redondance
  maximale. Jamais de postes utilisateurs en direct.
- **Distribution** : frontière L2/L3 (passerelles VLAN), politiques
  (ACL, QoS), agrégation des switchs d'accès.
- **Accès** : ports utilisateurs, PoE, sécurité de port.
- **Edge/WAN** : BGP vers 2 FAIs, NAT, VPN IPsec/DMVPN vers les sites.
- **DMZ** : serveurs publics derrière le firewall, jamais routés
  directement vers le LAN.

---

## 2. Plan d'adressage et conventions

| Usage | Plage | Exemple |
|---|---|---|
| Loopbacks (router-id, management) | 10.255.255.0/24 | CORE-1 = .1, CORE-2 = .2 |
| Transits point-à-point | 10.0.0.0/24 en /30 | CORE-1↔EDGE-1 = 10.0.0.0/30 |
| Management (OOB ou VLAN 99) | 192.168.99.0/24 | — |
| VLAN 10 DATA | 192.168.10.0/24 | GW .254 (HSRP/VRRP) |
| VLAN 20 VOIX | 192.168.20.0/24 | GW .254 |
| VLAN 30 INVITES | 192.168.30.0/24 | GW .254 |
| VLAN 40 SERVEURS | 192.168.40.0/24 | GW .254 |
| VLAN 99 MGMT | 192.168.99.0/24 | GW .254 |
| WAN publics | bloc FAI | — |

**Conventions de nommage** : `CORE-01`, `DIST-A-01`, `ACC-A-01-03`
(bâtiment A, local 01, switch 03). Descriptions systématiques sur les
interfaces (`description UPLINK_DIST-A-01`).

---

## 3. Socle management (les deux vendors)

À déployer **en premier** sur chaque équipement : sans NTP/SNMP/syslog/
AAA homogènes, pas de supervision ni d'audit possibles.

### 3.1 Cisco — socle

```shell
CISCO> enable
CISCO# configure terminal
CISCO(config)# hostname CORE-01
CISCO(config)# no ip domain-lookup
CISCO(config)# ip domain-name entreprise.lan
CISCO(config)# service password-encryption
CISCO(config)# service timestamps log datetime msec
CISCO(config)# service timestamps debug datetime msec
! --- NTP ---
CISCO(config)# ntp server 192.168.99.10 prefer
CISCO(config)# ntp server 192.168.99.11
CISCO(config)# clock timezone UTC 0
! --- SSH ---
CISCO(config)# username admin privilege 15 secret AdminFort2026
CISCO(config)# crypto key generate rsa modulus 2048
CISCO(config)# ip ssh version 2
CISCO(config)# line vty 0 15
CISCO(config-line)# login local
CISCO(config-line)# transport input ssh
CISCO(config-line)# exec-timeout 10 0
CISCO(config-line)# exit
! --- Syslog ---
CISCO(config)# logging host 192.168.99.50
CISCO(config)# logging trap informational
CISCO(config)# logging source-interface Loopback 0
CISCO(config)# logging buffered 32768
! --- SNMPv3 ---
CISCO(config)# snmp-server group NOC v3 priv
CISCO(config)# snmp-server user zabbix NOC v3 auth sha AuthPass priv aes 256 PrivPass
CISCO(config)# snmp-server host 192.168.99.50 version 3 priv zabbix
CISCO(config)# snmp-server location DATACENTER-RangA12
CISCO(config)# snmp-server contact noc@entreprise.lan
! --- AAA TACACS+ ---
CISCO(config)# aaa new-model
CISCO(config)# tacacs-server host 192.168.99.60 key CleTACACS
CISCO(config)# aaa authentication login default group tacacs+ local
CISCO(config)# aaa authorization exec default group tacacs+ local
CISCO(config)# aaa accounting exec default start-stop group tacacs+
CISCO(config)# end
CISCO# copy running-config startup-config
```

### 3.2 Huawei — socle

```shell
HUAWEI> system-view
HUAWEI] sysname CORE-01
HUAWEI] undo telnet server enable
HUAWEI] ssh server enable
HUAWEI] dsa local-key-pair create
! --- utilisateur admin ---
HUAWEI] aaa
HUAWEI-aaa] local-user admin password irreversible-cipher AdminFort2026
HUAWEI-aaa] local-user admin privilege level 15
HUAWEI-aaa] local-user admin service-type ssh terminal
HUAWEI-aaa] quit
HUAWEI] user-interface vty 0 14
HUAWEI-ui-vty] authentication-mode aaa
HUAWEI-ui-vty] protocol inbound ssh
HUAWEI-ui-vty] idle-timeout 10 0
HUAWEI-ui-vty] quit
! --- NTP ---
HUAWEI] ntp-service unicast-server 192.168.99.10 preference
HUAWEI] ntp-service unicast-server 192.168.99.11
HUAWEI] clock timezone UTC add 00:00:00
! --- Syslog ---
HUAWEI] info-center loghost 192.168.99.50
HUAWEI] info-center source default channel loghost log level informational
HUAWEI] info-center loghost source LoopBack 0
! --- SNMPv3 ---
HUAWEI] snmp-agent
HUAWEI] snmp-agent sys-info location DATACENTER-RangA12
HUAWEI] snmp-agent sys-info contact noc@entreprise.lan
HUAWEI] snmp-agent group v3 NOC privacy
HUAWEI] snmp-agent local-user zabbix group NOC
HUAWEI] snmp-agent local-user zabbix auth-mode sha AuthPass
HUAWEI] snmp-agent local-user zabbix priv-mode aes256 PrivPass
HUAWEI] snmp-agent target-host trap address udp-domain 192.168.99.50 params securityname zabbix v3 privacy
! --- HWTACACS ---
HUAWEI] hwtacacs-server template TAC
HUAWEI-hwtacacs] hwtacacs-server authentication 192.168.99.60
HUAWEI-hwtacacs] hwtacacs-server authorization 192.168.99.60
HUAWEI-hwtacacs] hwtacacs-server accounting 192.168.99.60
HUAWEI-hwtacacs] hwtacacs-server shared-key cipher CleTACACS
HUAWEI-hwtacacs] quit
HUAWEI] aaa
HUAWEI-aaa] authentication-scheme default
HUAWEI-aaa-authen] authentication-mode hwtacacs local
HUAWEI-aaa-authen] quit
HUAWEI-aaa] authorization-scheme default
HUAWEI-aaa-author] authorization-mode hwtacacs local
HUAWEI-aaa-author] quit
HUAWEI-aaa] accounting-scheme default
HUAWEI-aaa-accounting] accounting-mode hwtacacs
HUAWEI-aaa-accounting] quit
HUAWEI-aaa] quit
HUAWEI] domain default
HUAWEI-domain] hwtacacs-server TAC
HUAWEI-domain] quit
HUAWEI] quit
HUAWEI> save
```

---

## 4. Cisco — déploiement par couche

### 4.1 Core (Catalyst 9500 × 2)

```shell
! --- VLANs L3 (SVI) ---
CISCO(config)# vlan 10
CISCO(config-vlan)# name DATA
CISCO(config-vlan)# exit
CISCO(config)# vlan 20
CISCO(config-vlan)# name VOIX
CISCO(config-vlan)# exit
CISCO(config)# vlan 40
CISCO(config-vlan)# name SERVEURS
CISCO(config-vlan)# exit
CISCO(config)# vlan 99
CISCO(config-vlan)# name MGMT
CISCO(config-vlan)# exit
!
! --- SVI passerelles + HSRP (CORE-01 actif) ---
CISCO(config)# interface Vlan 10
CISCO(config-if)# ip address 192.168.10.2 255.255.255.0
CISCO(config-if)# standby 10 ip 192.168.10.254
CISCO(config-if)# standby 10 priority 110
CISCO(config-if)# standby 10 preempt
CISCO(config-if)# no shutdown
CISCO(config-if)# exit
! (répéter pour Vlan 20/40/99 — CORE-02 en priority 100, preempt)
!
! --- Uplinks L3 vers distribution (point-à-point, pas de trunk au core) ---
CISCO(config)# interface range TenGigabitEthernet 1/0/1 - 2
CISCO(config-if-range)# no switchport
CISCO(config-if-range)# ip address 10.0.0.1 255.255.255.252
CISCO(config-if-range)# no shutdown
!
! --- OSPF backbone ---
CISCO(config)# router ospf 1
CISCO(config-router)# router-id 10.255.255.1
CISCO(config-router)# passive-interface default
CISCO(config-router)# no passive-interface TenGigabitEthernet 1/0/1
CISCO(config-router)# no passive-interface TenGigabitEthernet 1/0/2
CISCO(config-router)# network 10.0.0.0 0.0.0.255 area 0
CISCO(config-router)# network 192.168.0.0 0.0.255.255 area 0
CISCO(config-router)# exit
!
! --- Loopback ---
CISCO(config)# interface Loopback 0
CISCO(config-if)# ip address 10.255.255.1 255.255.255.255
!
CISCO# show ip ospf neighbor
CISCO# show standby brief
```

### 4.2 Distribution (Catalyst 9300)

```shell
! --- Trunks vers core (LACP) ---
CISCO(config)# interface range TenGigabitEthernet 1/1/1 - 2
CISCO(config-if-range)# switchport mode trunk
CISCO(config-if-range)# switchport trunk allowed vlan 10,20,30,40,99
CISCO(config-if-range)# channel-group 10 mode active
CISCO(config-if-range)# exit
CISCO(config)# interface Port-channel 10
CISCO(config-if)# switchport mode trunk
CISCO(config-if)# switchport trunk allowed vlan 10,20,30,40,99
!
! --- STP : distribution = root secondaire ---
CISCO(config)# spanning-tree mode rapid-pvst
CISCO(config)# spanning-tree vlan 10,20,30,40,99 priority 8192
! (core en 4096 = root primaire)
!
! --- Routage inter-VLAN ici OU au core (choisir UNE frontière L3) ---
! Option B : SVI en distribution (recommandé si core = pur L3 sans SVI)
! (même syntaxe SVI + HSRP que §4.1)
!
! --- DHCP snooping + DAI ---
CISCO(config)# ip dhcp snooping
CISCO(config)# ip dhcp snooping vlan 10,20,30,40
CISCO(config)# interface Port-channel 10
CISCO(config-if)# ip dhcp snooping trust
CISCO(config-if)# exit
CISCO(config)# ip arp inspection vlan 10,20,30,40
CISCO(config)# interface Port-channel 10
CISCO(config-if)# ip arp inspection trust
!
! --- OSPF ---
CISCO(config)# router ospf 1
CISCO(config-router)# router-id 10.255.255.11
CISCO(config-router)# network 10.0.0.0 0.0.0.255 area 0
CISCO(config-router)# network 192.168.0.0 0.0.255.255 area 0
CISCO(config-router)# passive-interface default
CISCO(config-router)# no passive-interface Port-channel 10
```

### 4.3 Accès (Catalyst 9200/9300, PoE)

```shell
! --- Template de port utilisateur ---
CISCO(config)# interface range GigabitEthernet 1/0/1 - 44
CISCO(config-if-range)# switchport mode access
CISCO(config-if-range)# switchport access vlan 10
CISCO(config-if-range)# spanning-tree portfast
CISCO(config-if-range)# spanning-tree bpduguard enable
CISCO(config-if-range)# switchport port-security
CISCO(config-if-range)# switchport port-security maximum 3
CISCO(config-if-range)# switchport port-security violation restrict
CISCO(config-if-range)# switchport port-security mac-address sticky
CISCO(config-if-range)# switchport port-security aging time 60
CISCO(config-if-range)# ip verify source              ! IP Source Guard
CISCO(config-if-range)# exit
!
! --- Ports voix (téléphones IP) ---
CISCO(config)# interface range GigabitEthernet 1/0/1 - 24
CISCO(config-if-range)# switchport voice vlan 20
CISCO(config-if-range)# mls qos trust dscp
!
! --- Uplink vers distribution ---
CISCO(config)# interface range GigabitEthernet 1/0/47 - 48
CISCO(config-if-range)# switchport mode trunk
CISCO(config-if-range)# switchport trunk allowed vlan 10,20,30,99
CISCO(config-if-range)# channel-group 1 mode active
CISCO(config-if-range)# ip dhcp snooping trust
CISCO(config-if-range)# ip arp inspection trust
!
! --- 802.1X (optionnel, niveau avancé) ---
CISCO(config)# aaa new-model
CISCO(config)# radius-server host 192.168.99.61 key CleRADIUS
CISCO(config)# aaa authentication dot1x default group radius
CISCO(config)# aaa authorization network default group radius
CISCO(config)# dot1x system-auth-control
CISCO(config)# interface range GigabitEthernet 1/0/1 - 44
CISCO(config-if-range)# authentication port-control auto
CISCO(config-if-range)# dot1x pae authenticator
```

---

## 5. Huawei — déploiement par couche (miroir)

Même architecture, même adressage. Notez les équivalences :
`standby` → `vrrp vrid`, `channel-group` → `eth-trunk`,
`spanning-tree portfast` → `stp edged-port`.

### 5.1 Core (CloudEngine S6730 × 2, ou S5735)

```shell
HUAWEI> system-view
HUAWEI] sysname CORE-01-HW
! --- VLANs ---
HUAWEI] vlan batch 10 20 40 99
HUAWEI] vlan 10
HUAWEI-vlan10] description DATA
HUAWEI-vlan10] quit
! --- Vlanif + VRRP (CORE-01 = master) ---
HUAWEI] interface Vlanif 10
HUAWEI-Vlanif10] ip address 192.168.10.2 24
HUAWEI-Vlanif10] vrrp vrid 10 virtual-ip 192.168.10.254
HUAWEI-Vlanif10] vrrp vrid 10 priority 120
HUAWEI-Vlanif10] vrrp vrid 10 preempt-mode timer delay 20
HUAWEI-Vlanif10] quit
! (répéter Vlanif 20/40/99 — CORE-02 en priority 100)
!
! --- Uplinks L3 ---
HUAWEI] interface 10GE 1/0/1
HUAWEI-10GE] undo portswitch
HUAWEI-10GE] ip address 10.0.0.1 30
HUAWEI-10GE] undo shutdown
HUAWEI-10GE] quit
!
! --- OSPF ---
HUAWEI] ospf 1 router-id 10.255.255.1
HUAWEI-ospf] area 0
HUAWEI-ospf-area] network 10.0.0.0 0.0.0.255
HUAWEI-ospf-area] network 192.168.0.0 0.0.255.255
HUAWEI-ospf-area] quit
HUAWEI-ospf] silent-interface Vlanif 10
HUAWEI-ospf] silent-interface Vlanif 20
HUAWEI-ospf] silent-interface Vlanif 40
HUAWEI-ospf] silent-interface Vlanif 99
HUAWEI-ospf] quit
!
HUAWEI] interface LoopBack 0
HUAWEI-LoopBack] ip address 10.255.255.1 32
HUAWEI-LoopBack] quit
HUAWEI] quit
HUAWEI> display ospf peer
HUAWEI> display vrrp
```

### 5.2 Distribution (S5735)

```shell
HUAWEI> system-view
! --- Eth-Trunk LACP vers core ---
HUAWEI] interface Eth-Trunk 10
HUAWEI-Eth-Trunk10] mode lacp
HUAWEI-Eth-Trunk10] trunkport 10GE 1/0/1 to 1/0/2
HUAWEI-Eth-Trunk10] port link-type trunk
HUAWEI-Eth-Trunk10] port trunk allow-pass vlan 10 20 30 40 99
HUAWEI-Eth-Trunk10] quit
!
! --- STP : root secondaire ---
HUAWEI] stp mode rstp
HUAWEI] stp priority 8192
! (core en 4096)
HUAWEI] stp instance 0 priority 8192
!
! --- DHCP snooping + anti-attaque ARP ---
HUAWEI] dhcp snooping enable
HUAWEI] vlan 10
HUAWEI-vlan10] dhcp snooping enable
HUAWEI-vlan10] quit
! (répéter vlan 20/30/40)
HUAWEI] interface Eth-Trunk 10
HUAWEI-Eth-Trunk10] dhcp snooping trusted
HUAWEI-Eth-Trunk10] arp anti-attack check user-bind enable
HUAWEI-Eth-Trunk10] quit
!
! --- OSPF (idem §5.1, router-id 10.255.255.11) ---
HUAWEI] ospf 1 router-id 10.255.255.11
HUAWEI-ospf] area 0
HUAWEI-ospf-area] network 10.0.0.0 0.0.0.255
HUAWEI-ospf-area] network 192.168.0.0 0.0.255.255
HUAWEI-ospf-area] quit
HUAWEI-ospf] silent-interface all
HUAWEI-ospf] undo silent-interface Eth-Trunk 10
HUAWEI-ospf] quit
```

### 5.3 Accès (S5735-L, PoE)

```shell
HUAWEI> system-view
! --- Ports utilisateurs ---
HUAWEI] port-group USER-PORTS
HUAWEI-port-group] group-member GigabitEthernet 0/0/1 to 0/0/44
HUAWEI-port-group] port link-type access
HUAWEI-port-group] port default vlan 10
HUAWEI-port-group] stp edged-port enable
HUAWEI-port-group] port-security enable
HUAWEI-port-group] port-security max-mac-num 3
HUAWEI-port-group] port-security protect-action restrict
HUAWEI-port-group] port-security aging-time 60
HUAWEI-port-group] quit
!
! --- Ports voix ---
HUAWEI] port-group VOICE-PORTS
HUAWEI-port-group] group-member GigabitEthernet 0/0/1 to 0/0/24
HUAWEI-port-group] voice-vlan 20 enable
HUAWEI-port-group] trust dscp
HUAWEI-port-group] quit
!
! --- Uplink ---
HUAWEI] interface Eth-Trunk 1
HUAWEI-Eth-Trunk1] mode lacp
HUAWEI-Eth-Trunk1] trunkport GigabitEthernet 0/0/47 to 0/0/48
HUAWEI-Eth-Trunk1] port link-type trunk
HUAWEI-Eth-Trunk1] port trunk allow-pass vlan 10 20 30 99
HUAWEI-Eth-Trunk1] dhcp snooping trusted
HUAWEI-Eth-Trunk1] quit
!
! --- 802.1X (optionnel) ---
HUAWEI] dot1x enable
HUAWEI] radius-server template RAD
HUAWEI-radius] radius-server shared-key cipher CleRADIUS
HUAWEI-radius] radius-server authentication 192.168.99.61 1812
HUAWEI-radius] radius-server accounting 192.168.99.61 1813
HUAWEI-radius] quit
HUAWEI] aaa
HUAWEI-aaa] authentication-scheme default
HUAWEI-aaa-authen] authentication-mode radius
HUAWEI-aaa] quit
HUAWEI] interface GigabitEthernet 0/0/1
HUAWEI-GE] dot1x enable
```

---

## 6. WAN entreprise — BGP multi-homing

### 6.1 Cisco (routeurs ISR 4000 / ASR)

```shell
CISCO(config)# ip prefix-list PL-LOCAL seq 5 permit 192.168.0.0/16
CISCO(config)# ip prefix-list PL-LOCAL seq 10 permit 203.0.113.0/24
CISCO(config)# ip prefix-list PL-FULL seq 5 deny 0.0.0.0/0
CISCO(config)# ip prefix-list PL-FULL seq 10 permit 0.0.0.0/0 le 24
!
CISCO(config)# route-map RM-OUT-FAI1 permit 10
CISCO(config-route-map)# match ip address prefix-list PL-LOCAL
CISCO(config-route-map)# exit
CISCO(config)# route-map RM-IN-FAI1 permit 10
CISCO(config-route-map)# match ip address prefix-list PL-FULL
CISCO(config-route-map)# set local-preference 200
CISCO(config-route-map)# exit
!
CISCO(config)# router bgp 65001
CISCO(config-router)# bgp router-id 10.255.255.254
CISCO(config-router)# neighbor 203.0.113.9 remote-as 100
CISCO(config-router)# neighbor 203.0.113.9 description FAI-1
CISCO(config-router)# neighbor 203.0.113.9 password MD5BGP
CISCO(config-router)# neighbor 203.0.113.9 route-map RM-IN-FAI1 in
CISCO(config-router)# neighbor 203.0.113.9 route-map RM-OUT-FAI1 out
CISCO(config-router)# neighbor 203.0.113.9 maximum-prefix 1000000
CISCO(config-router)# network 192.168.0.0 mask 255.255.0.0
CISCO(config-router)# network 203.0.113.0 mask 255.255.255.0
! (répéter vers FAI-2 avec local-preference 100 = backup)
CISCO# show ip bgp summary
CISCO# show ip bgp neighbors 203.0.113.9 advertised-routes
```

### 6.2 Huawei (routeurs AR / NetEngine)

```shell
HUAWEI> system-view
HUAWEI] ip ip-prefix PL-LOCAL index 10 permit 192.168.0.0 16 greater-equal 16 less-equal 24
HUAWEI] ip ip-prefix PL-LOCAL index 20 permit 203.0.113.0 24
HUAWEI] ip ip-prefix PL-FULL index 10 deny 0.0.0.0 0
HUAWEI] ip ip-prefix PL-FULL index 20 permit 0.0.0.0 0 greater-equal 1 less-equal 24
!
HUAWEI] route-policy RM-OUT-FAI1 permit node 10
HUAWEI-route-policy] if-match ip-prefix PL-LOCAL
HUAWEI-route-policy] quit
HUAWEI] route-policy RM-IN-FAI1 permit node 10
HUAWEI-route-policy] if-match ip-prefix PL-FULL
HUAWEI-route-policy] apply local-preference 200
HUAWEI-route-policy] quit
!
HUAWEI] bgp 65001
HUAWEI-bgp] router-id 10.255.255.254
HUAWEI-bgp] peer 203.0.113.9 as-number 100
HUAWEI-bgp] peer 203.0.113.9 description FAI-1
HUAWEI-bgp] peer 203.0.113.9 password cipher MD5BGP
HUAWEI-bgp] peer 203.0.113.9 ip-prefix PL-FULL import
HUAWEI-bgp] peer 203.0.113.9 route-policy RM-OUT-FAI1 export
HUAWEI-bgp-af] network 192.168.0.0 255.255.0.0
HUAWEI-bgp-af] network 203.0.113.0 255.255.255.0
HUAWEI-bgp-af] quit
HUAWEI-bgp] quit
HUAWEI] quit
HUAWEI> display bgp peer
HUAWEI> display bgp routing-table
```

> **Règle d'or BGP entreprise** : ne jamais accepter ni annoncer sans
> filtre. `maximum-prefix` (Cisco) / `peer … route-limit` (Huawei)
> protègent contre les fuites de tables complètes.

---

## 7. VPN inter-sites

### 7.1 Cisco — DMVPN (hub & spoke, recommandé multi-sites)

```shell
! --- HUB ---
CISCO(config)# interface Tunnel 0
CISCO(config-if)# ip address 172.16.0.1 255.255.255.0
CISCO(config-if)# ip nhrp network-id 1
CISCO(config-if)# ip nhrp authentication CleNHRP
CISCO(config-if)# tunnel source GigabitEthernet 0/0
CISCO(config-if)# tunnel mode gre multipoint
CISCO(config-if)# tunnel protection ipsec profile PROF-IPSEC
CISCO(config-if)# exit
CISCO(config)# router eigrp DMVPN
CISCO(config-router)# address-family ipv4 unicast autonomous-system 200
CISCO(config-router-af)# network 172.16.0.0 0.0.0.255
CISCO(config-router-af)# network 192.168.0.0 0.0.255.255
! --- SPOKE (site distant) ---
CISCO(config)# interface Tunnel 0
CISCO(config-if)# ip address 172.16.0.11 255.255.255.0
CISCO(config-if)# ip nhrp network-id 1
CISCO(config-if)# ip nhrp authentication CleNHRP
CISCO(config-if)# ip nhrp nhs 172.16.0.1
CISCO(config-if)# ip nhrp map 172.16.0.1 203.0.113.10
CISCO(config-if)# ip nhrp map multicast 203.0.113.10
CISCO(config-if)# tunnel source GigabitEthernet 0/0
CISCO(config-if)# tunnel mode gre multipoint
CISCO(config-if)# tunnel protection ipsec profile PROF-IPSEC
```

### 7.2 Huawei — IPsec site-à-site (IKEv2)

```shell
HUAWEI> system-view
HUAWEI] ike proposal 10
HUAWEI-ike-proposal] encryption-algorithm aes-256
HUAWEI-ike-proposal] dh group14
HUAWEI-ike-proposal] authentication-algorithm sha2-256
HUAWEI-ike-proposal] integrity-algorithm hmac-sha2-256
HUAWEI-ike-proposal] quit
HUAWEI] ike peer PEER-SITEB
HUAWEI-ike-peer] ike-proposal 10
HUAWEI-ike-peer] remote-address 198.51.100.2
HUAWEI-ike-peer] pre-shared-key cipher CleIPsec
HUAWEI-ike-peer] quit
HUAWEI] ipsec proposal PROP1
HUAWEI-ipsec-proposal] esp authentication-algorithm sha2-256
HUAWEI-ipsec-proposal] esp encryption-algorithm aes-256
HUAWEI-ipsec-proposal] quit
HUAWEI] acl number 3001
HUAWEI-acl] rule permit ip source 192.168.0.0 0.0.255.255 destination 192.168.100.0 0.0.0.255
HUAWEI-acl] quit
HUAWEI] ipsec policy MAP1 10 isakmp
HUAWEI-ipsec-policy] security acl 3001
HUAWEI-ipsec-policy] ike-peer PEER-SITEB
HUAWEI-ipsec-policy] proposal PROP1
HUAWEI-ipsec-policy] quit
HUAWEI] interface GigabitEthernet 0/0/0
HUAWEI-GE] ipsec policy MAP1
HUAWEI-GE] quit
HUAWEI] quit
HUAWEI> display ike sa
HUAWEI> display ipsec sa
```

---

## 8. Haute disponibilité

| Fonction | Cisco | Huawei |
|---|---|---|
| Passerelle redondante | HSRP (`standby`) | VRRP (`vrrp vrid`) |
| Load-sharing passerelle | GLBP | VRRP + load-balance / Eth-Trunk |
| Stacking | StackWise (Catalyst) | CSS / iStack (S) |
| Firewall | HSRP / FHRP | HRP (USG, voir guide USG) |

- **Interop passerelle** : utilisez **VRRP des deux côtés** si le
  segment est mixte (HSRP est propriétaire Cisco).
- **Stacking** : simplifie la gestion (1 seul plan de contrôle) mais
  crée un domaine de panne unique — en core, préférez 2 châssis
  indépendants + HSRP/VRRP.

---

## 9. QoS entreprise

### 9.1 Cisco (MQC)

```shell
! --- Classification et marquage à l'accès ---
CISCO(config)# class-map match-any VOIP
CISCO(config-cmap)# match dscp ef
CISCO(config-cmap)# exit
CISCO(config)# class-map match-any SIGNAL
CISCO(config-cmap)# match dscp cs3
CISCO(config-cmap)# exit
CISCO(config)# class-map match-any CRITIQUE
CISCO(config-cmap)# match dscp af31 af32 af33
CISCO(config-cmap)# exit
!
CISCO(config)# policy-map QOS-CORE-OUT
CISCO(config-pmap)# class VOIP
CISCO(config-pmap-c)# priority percent 30
CISCO(config-pmap-c)# exit
CISCO(config-pmap)# class SIGNAL
CISCO(config-pmap-c)# bandwidth percent 5
CISCO(config-pmap-c)# exit
CISCO(config-pmap)# class CRITIQUE
CISCO(config-pmap-c)# bandwidth percent 30
CISCO(config-pmap-c)# random-detect dscp-based
CISCO(config-pmap-c)# exit
CISCO(config-pmap)# class class-default
CISCO(config-pmap-c)# fair-queue
CISCO(config-pmap-c)# random-detect
CISCO(config-pmap-c)# exit
CISCO(config-pmap)# exit
CISCO(config)# interface range TenGigabitEthernet 1/0/1 - 2
CISCO(config-if-range)# service-policy output QOS-CORE-OUT
!
CISCO# show policy-map interface TenGigabitEthernet 1/0/1
```

### 9.2 Huawei (MQC : classifier / behavior / policy)

```shell
HUAWEI> system-view
HUAWEI] traffic classifier VOIP
HUAWEI-classifier] if-match dscp ef
HUAWEI-classifier] quit
HUAWEI] traffic classifier CRITIQUE
HUAWEI-classifier] if-match dscp af31
HUAWEI-classifier] if-match dscp af32
HUAWEI-classifier] quit
HUAWEI] traffic behavior B-VOIP
HUAWEI-behavior] queue af bandwidth pct 30
HUAWEI-behavior] quit
HUAWEI] traffic behavior B-CRITIQUE
HUAWEI-behavior] queue af bandwidth pct 30
HUAWEI-behavior] quit
HUAWEI] traffic policy QOS-CORE-OUT
HUAWEI-policy] classifier VOIP behavior B-VOIP
HUAWEI-policy] classifier CRITIQUE behavior B-CRITIQUE
HUAWEI-policy] quit
HUAWEI] interface 10GE 1/0/1
HUAWEI-10GE] traffic-policy QOS-CORE-OUT outbound
HUAWEI-10GE] quit
HUAWEI] quit
HUAWEI> display traffic policy applied-record
```

> **Modèle DiffServ commun** : EF = voix (LLQ/priority), CS3 = signalisation,
> AF31 = données critiques, Default = best effort. Marquez au plus près
> de la source (`trust dscp` sur les ports voix), appliquez en sortie
> des uplinks.

---

## 10. Sécurité : durcissement entreprise

### 10.1 ACL d'infrastructure (Cisco + Huawei)

```shell
! --- Cisco : protéger le plan de contrôle ---
CISCO(config)# ip access-list extended ACL-INFRA
CISCO(config-ext-nacl)# permit tcp 192.168.99.0 0.0.0.255 any eq 22
CISCO(config-ext-nacl)# permit udp 192.168.99.0 0.0.0.255 any eq snmp
CISCO(config-ext-nacl)# permit tcp 192.168.99.0 0.0.0.255 any eq 443
CISCO(config-ext-nacl)# deny ip any any log
CISCO(config-ext-nacl)# exit
CISCO(config)# line vty 0 15
CISCO(config-line)# access-class ACL-INFRA in
```

```shell
! --- Huawei : ACL + appel sur VTY ---
HUAWEI> system-view
HUAWEI] acl number 3000
HUAWEI-acl] rule permit tcp source 192.168.99.0 0.0.0.255 destination-port eq 22
HUAWEI-acl] rule permit udp source 192.168.99.0 0.0.0.255 destination-port eq snmp
HUAWEI-acl] rule deny ip
HUAWEI-acl] quit
HUAWEI] user-interface vty 0 14
HUAWEI-ui-vty] acl 3000 inbound
HUAWEI-ui-vty] quit
HUAWEI] quit
HUAWEI> save
```

### 10.2 Protections anti-spoofing / anti-attaque

| Menace | Cisco | Huawei |
|---|---|---|
| Usurpation IP source | `ip verify source` (+ `ip verify unicast source reachable-via rx`) | `arp anti-attack check user-bind` / uRPF `ip urpf` |
| DHCP rogue | `ip dhcp snooping` | `dhcp snooping enable` |
| ARP spoofing | `ip arp inspection vlan` (DAI) | `arp anti-attack` / DAI équivalent |
| Storm broadcast | `storm-control broadcast level` | `storm suppression` / `broadcast-suppression` |
| BPDU rogue | `spanning-tree bpduguard` | `stp bpdu-protection` |

### 10.3 Checklist hardening
- [ ] Telnet désactivé partout, SSH v2 + clés.
- [ ] Comptes nominatifs, 2FA sur les accès VPN/admin, TACACS+/RADIUS.
- [ ] VTY/console filtrés par ACL (réseau MGMT uniquement).
- [ ] SNMP v3 authPriv ; communities v2 supprimées.
- [ ] DHCP snooping + DAI + IP Source Guard sur tous les VLANs users.
- [ ] Port-security + BPDU guard sur tous les ports d'accès.
- [ ] Storm-control sur les ports d'accès.
- [ ] NTP authentifié si possible ; logs vers SIEM redondant.
- [ ] Firmware : version validée, `show version` documentée, procédure
      de rollback testée.

---

## 11. Interopérabilité Cisco ↔ Huawei (réseaux mixtes)

| Protocole | Interopérable ? | Points d'attention |
|---|---|---|
| Trunk 802.1Q | ✅ | **Native VLAN identique** des 2 côtés ; `allowed vlan` miroir |
| LACP | ✅ | `mode active` (Cisco) ↔ `mode lacp` (Huawei), les deux en actif |
| RSTP | ✅ | Timers par défaut identiques ; vérifier `display stp brief` |
| MSTP | ✅ | **Region name + revision + mapping VLAN→instance IDENTIQUES** |
| OSPF | ✅ | Mêmes timers hello/dead, même auth MD5, MTU identique |
| BGP | ✅ | eBGP standard ; MD5 compatible |
| VRRP | ✅ | Même VRID, même VIP, priorités cohérentes |
| HSRP | ❌ | Propriétaire Cisco → utilisez VRRP en mixte |
| DHCP snooping | ⚠️ | Chaque vendor gère sa base ; configurer le trust de chaque côté |
| QoS DSCP | ✅ | Le marquage DSCP est standard ; `trust dscp` des deux côtés |

**Exemple : trunk Cisco ↔ Huawei**
```shell
! Cisco
CISCO(config-if)# switchport mode trunk
CISCO(config-if)# switchport trunk allowed vlan 10,20,30
CISCO(config-if)# switchport trunk native vlan 99
```
```shell
! Huawei
HUAWEI-Eth-Trunk] port link-type trunk
HUAWEI-Eth-Trunk] port trunk allow-pass vlan 10 20 30
HUAWEI-Eth-Trunk] port trunk pvid vlan 99
```
> Le `native vlan` Cisco = le `pvid` Huawei. **Mismatch = VLAN hopping
> potentiel et trafic perdu** : vérifiez toujours les deux côtés.

---

## 12. Supervision et télémétrie

- **SNMPv3** → Zabbix/Centreon : CPU, mémoire, température, statut
  ports, compteurs d'erreurs, voisins OSPF/BGP, sessions.
- **Syslog** → SIEM (Graylog/ELK) : auth, changements de config
  (`logging`/`info-center`), alertes STP, flaps OSPF.
- **NetFlow v9 / sFlow** (Cisco) / **NetStream** (Huawei) → collecteur :
  qui parle à qui, top talkers, détection d'anomalies.
  ```shell
  ! Cisco — Flexible NetFlow
  CISCO(config)# flow record REC1
  CISCO(config-flow-record)# match ipv4 source address
  CISCO(config-flow-record)# match ipv4 destination address
  CISCO(config-flow-record)# collect counter bytes long
  CISCO(config)# flow exporter EXP1
  CISCO(config-flow-exporter)# destination 192.168.99.70
  CISCO(config)# flow monitor MON1
  CISCO(config-flow-monitor)# record REC1
  CISCO(config-flow-monitor)# exporter EXP1
  CISCO(config)# interface TenGigabitEthernet 1/0/1
  CISCO(config-if)# ip flow monitor MON1 input
  ```
  ```shell
  ! Huawei — NetStream
  HUAWEI> system-view
  HUAWEI] ip netstream
  HUAWEI] ip netstream export source LoopBack 0
  HUAWEI] ip netstream export host 192.168.99.70 9996
  HUAWEI] interface 10GE 1/0/1
  HUAWEI-10GE] ip netstream inbound
  HUAWEI-10GE] ip netstream outbound
  ```
- **IP SLA** (Cisco) / **NQA** (Huawei) : sondes de latence/perte vers
  les sites critiques + tracking pour HSRP/VRRP.

---

## 13. Checklist de mise en production

### Avant le jour J
- [ ] Configs relues par un pair (second œil obligatoire).
- [ ] `copy run start` / `save` sur **chaque** équipement.
- [ ] Backups `config.text`/`vrpcfg.zip` exportés hors-site.
- [ ] Plan de rollback écrit (qui fait quoi si ça casse, dans quel ordre).
- [ ] Fenêtre de maintenance communiquée aux utilisateurs.
- [ ] Console/accès OOB testés sur chaque équipement (en cas de perte
      réseau, on administre en console).
- [ ] Étiquetage physique : chaque câble, chaque port uplink.

### Recette (tests à cocher)
- [ ] Chaque VLAN : DHCP OK, passerelle OK, Internet OK.
- [ ] Inter-VLAN : autorisé/refusé **conformément à la politique**
      (tester les deux sens, avec `ping` ET le vrai protocole).
- [ ] HSRP/VRRP : extinction du master → bascule < 10 s, retour
      préemptif OK.
- [ ] OSPF : `show ip ospf neighbor` / `display ospf peer` = Full
      partout ; coupure d'un lien → reconvergence < 5 s.
- [ ] BGP : sessions Established, `show ip bgp` = vos préfixes
      uniquement en annonce.
- [ ] VPN : tunnels UP, trafic chiffré, test de coupure/reconnexion.
- [ ] DHCP snooping : brancher un serveur DHCP pirate → bloqué,
      alerte log.
- [ ] Port-security : 4e MAC → violation `restrict`, log.
- [ ] QoS : saturation du lien + appel VoIP → voix claire
      (test réel, pas théorique).
- [ ] NTP : `show ntp status` / `display ntp-service status` =
      synchronisé sur tous.
- [ ] SNMP : le superviseur voit tous les équipements.
- [ ] Syslog : un `login` test apparaît dans le SIEM.
- [ ] 802.1X (si déployé) : machine non autorisée → VLAN invité ou
      rejet.

### Le jour J (ordre)
1. Core → 2. Distribution → 3. Accès → 4. Edge/WAN → 5. Firewall/DMZ.
2. Valider chaque couche avant de passer à la suivante.
3. Garder l'ancien réseau en parallèle si possible (rollback = rebrancher).

---

## 14. Maintenance et évolutions

- **Firmwares** : 1 version de retard sur la dernière (sauf faille
  critique), jamais de `.0` en production, procédure de rollback
  testée en lab.
- **Config management** : versionner les configs (Git), tout changement
  = ticket + fenêtre + backup avant + vérification après.
- **Revues trimestrielles** : règles firewall/ACL obsolètes, comptes
  inactifs, certificats < 90 jours, capacités (CPU, ports libres).
- **Exercices** : failover HA et restore backup **chronométrés** 2×/an.
  Un plan non testé est un vœu pieux.
- **Documentation vivante** : schéma à jour, plan d'adressage à jour,
  mots de passe dans le coffre (jamais dans la doc).

---

*Fin de la référence. Documents compagnons : [Guide CLI Cisco](cisco_cli_guide.md),
[Guide CLI Huawei USG](huawei_usg_guide.md),
[Troubleshooting Huawei USG](huawei_usg_troubleshooting.md),
références officielles Huawei (tars EDOC) et Cisco (Command Reference IOS).*
