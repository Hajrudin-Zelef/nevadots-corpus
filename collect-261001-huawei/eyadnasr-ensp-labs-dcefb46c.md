# EyadNasr/eNSP_Labs

Repo GitHub : https://github.com/eyadnasr/ensp_labs

Description : Various network configuration labs on Huawei eNSP emulator.

Contenu : README + 45 fichiers de configuration d'equipements exportes depuis eNSP
(labs MPLS InterAS L3VPN Hub&Spokes options A/B, MPLS L3VPN Intranet).
Les fichiers binaires du repo (topologies .topo/.efz, captures .pcapng, vrpcfg.zip) ne sont pas inclus.

## Fichier : Hub&Spokes MPLS InterAS L3VPN  - BGP - same AS - Option A/Configs exported/Hub-CE7.cfg

```
[V200R003C00]
#
 sysname Hub_CE7
 ftp server enable
#
 snmp-agent local-engineid 800007DB03000000000000
 snmp-agent 
#
 clock timezone China-Standard-Time minus 08:00:00
#
portal local-server load portalpage.zip
#
 drop illegal-mac alarm
#
 lldp enable 
#
 set cpu-usage threshold 80 restore 75
#
aaa 
 authentication-scheme default
 authorization-scheme default
 accounting-scheme default
 domain default 
 domain default_admin 
 local-user admin password cipher %$%$HaEUO,PreYFOx)26I<"JK$&y%$%$
 local-user admin service-type http
 local-user test123 password cipher %$%$qHI;'~2a9LwxJuLq;ER&KW%V%$%$
 local-user test123 privilege level 15
 local-user test123 ftp-directory flash:
 local-user test123 service-type telnet ssh ftp
#
firewall zone Local
 priority 15
#
interface GigabitEthernet0/0/0
 ip address 192.168.200.17 255.255.255.0 
#
interface GigabitEthernet0/0/1
 ip address 10.0.67.7 255.255.255.0 
#
interface GigabitEthernet0/0/2
 ip address 10.0.76.7 255.255.255.0 
#
interface NULL0
#
interface LoopBack0
 ip address 7.7.7.7 255.255.255.255 
#
bgp 100
 router-id 7.7.7.7
 peer 6.6.6.7 as-number 312 
 peer 6.6.6.7 ebgp-max-hop 2 
 peer 6.6.6.7 connect-interface LoopBack0
 peer 6.6.6.8 as-number 312 
 peer 6.6.6.8 ebgp-max-hop 2 
 peer 6.6.6.8 connect-interface LoopBack0
 #
 ipv4-family unicast
  undo synchronization
  peer 6.6.6.7 enable
  peer 6.6.6.7 route-policy no-adv-to-VPN_In export
  peer 6.6.6.8 enable
  peer 6.6.6.8 advertise-ext-community
#
route-policy no-adv-to-VPN_In deny node 10 
 if-match ip-prefix match-any 
#
 sftp server enable 
 stelnet server enable 
#
ip ip-prefix match-any index 10 permit 0.0.0.0 0
#
ip route-static 6.6.6.7 255.255.255.255 10.0.76.6
ip route-static 6.6.6.8 255.255.255.255 10.0.67.6
#
user-interface con 0
 authentication-mode password
user-interface vty 0 4
 authentication-mode aaa
 protocol inbound all
user-interface vty 16 20
#
wlan ac
#
return
```

## Fichier : Hub&Spokes MPLS InterAS L3VPN  - BGP - same AS - Option A/Configs exported/Hub-PE26.cfg

```
[V200R003C00]
#
 sysname Hub-PE26
 ftp server enable
#
 snmp-agent local-engineid 800007DB03000000000000
 snmp-agent 
#
 clock timezone China-Standard-Time minus 08:00:00
#
portal local-server load portalpage.zip
#
 drop illegal-mac alarm
#
 lldp enable 
#
 set cpu-usage threshold 80 restore 75
#
ip vpn-instance site1
 ipv4-family
  route-distinguisher 300:1
  vpn-target 300:1 export-extcommunity
  vpn-target 100:1 import-extcommunity
#
ip vpn-instance site2
 ipv4-family
  route-distinguisher 300:2
  vpn-target 300:1 export-extcommunity
  vpn-target 200:1 import-extcommunity
#
mpls lsr-id 26.26.26.26
mpls
#
mpls ldp
#
#
aaa 
 authentication-scheme default
 authorization-scheme default
 accounting-scheme default
 domain default 
 domain default_admin 
 local-user admin password cipher %$%$c)];PQVkENs{=j'Rz%.UKEwV%$%$
 local-user admin service-type http
 local-user test123 password cipher %$%$S*~-;gQC(HnTEsU"UwEMKWgO%$%$
 local-user test123 privilege level 15
 local-user test123 ftp-directory flash:
 local-user test123 service-type telnet ssh ftp
#
firewall zone Local
 priority 15
#
interface GigabitEthernet0/0/0
#
interface GigabitEthernet0/0/0.1
 dot1q termination vid 1
 ip binding vpn-instance site1
 ip address 10.0.69.9 255.255.255.0 
 arp broadcast enable
#
interface GigabitEthernet0/0/0.2
 dot1q termination vid 2
 ip binding vpn-instance site2
 ip address 10.1.69.9 255.255.255.0 
 arp broadcast enable
#
interface GigabitEthernet0/0/1
 ip address 192.168.200.9 255.255.255.0 
#
interface GigabitEthernet0/0/2
 ip address 10.0.26.9 255.255.255.0 
 mpls
 mpls ldp
#
interface NULL0
#
interface LoopBack0
 ip address 26.26.26.26 255.255.255.255 
#
interface LoopBack1
 ip binding vpn-instance site1
 ip address 26.26.26.27 255.255.255.255 
#
interface LoopBack2
 ip binding vpn-instance site2
 ip address 26.26.26.28 255.255.255.255 
#
bgp 123
 router-id 26.26.26.26
 peer 3.3.3.3 as-number 123 
 peer 3.3.3.3 connect-interface LoopBack0
 peer 4.4.4.4 as-number 123 
 peer 4.4.4.4 connect-interface LoopBack0
 #
 ipv4-family unicast
  undo synchronization
  peer 3.3.3.3 enable
  peer 4.4.4.4 enable
 # 
 ipv4-family vpnv4
  policy vpn-target
  peer 3.3.3.3 enable
  peer 3.3.3.3 route-policy no-site1 export
  peer 4.4.4.4 enable
  peer 4.4.4.4 route-policy no-site2 export
 #
 ipv4-family vpn-instance site1 
  router-id 26.26.26.27
  peer 66.66.66.67 as-number 312 
  peer 66.66.66.67 ebgp-max-hop 2 
  peer 66.66.66.67 connect-interface LoopBack1
 #
 ipv4-family vpn-instance site2 
  router-id 26.26.26.28
  peer 66.66.66.68 as-number 312 
  peer 66.66.66.68 ebgp-max-hop 2 
  peer 66.66.66.68 connect-interface LoopBack2
#
ospf 1 
 area 0.0.0.0 
  network 10.0.26.9 0.0.0.0 
  network 26.26.26.26 0.0.0.0 
#
route-policy no-site2 deny node 10 
 if-match extcommunity-filter 200:1 
#
route-policy no-site2 permit node 20 
#
route-policy no-site1 deny node 10 
 if-match extcommunity-filter 100:1 
#
route-policy no-site1 permit node 20 
#
 sftp server enable 
 stelnet server enable 
#
ip extcommunity-filter basic 100:1 permit rt 100:1 
ip extcommunity-filter basic 200:1 permit rt 200:1 
#
ip route-static vpn-instance site1 66.66.66.67 255.255.255.255 10.0.69.66
ip route-static vpn-instance site2 66.66.66.68 255.255.255.255 10.1.69.66
#
user-interface con 0
 authentication-mode password
user-interface vty 0 4
 authentication-mode aaa
 protocol inbound all
user-interface vty 16 20
#
wlan ac
#
return
```

## Fichier : Hub&Spokes MPLS InterAS L3VPN  - BGP - same AS - Option A/Configs exported/Hub-PE6.cfg

```
[V200R003C00]
#
 sysname Hub-PE6
 ftp server enable
#
 board add 0/4 1GEC 
#
 snmp-agent local-engineid 800007DB03000000000000
 snmp-agent 
#
 clock timezone China-Standard-Time minus 08:00:00
#
portal local-server load portalpage.zip
#
 drop illegal-mac alarm
#
 lldp enable 
#
 set cpu-usage threshold 80 restore 75
#
ip vpn-instance VPN_In
 ipv4-family
  route-distinguisher 400:3
  vpn-target 100:1 200:1 import-extcommunity
#
ip vpn-instance VPN_Out
 ipv4-family
  route-distinguisher 300:3
  vpn-target 300:1 export-extcommunity
#
mpls lsr-id 6.6.6.6
mpls
#
mpls ldp
#
#
aaa 
 authentication-scheme default
 authorization-scheme default
 accounting-scheme default
 domain default 
 domain default_admin 
 local-user admin password cipher %$%$ktRQ*_`W;/ttaKRMV|7=K6U2%$%$
 local-user admin service-type http
 local-user test123 password cipher %$%$g*in8tS58'pue~;rf'eUKW@-%$%$
 local-user test123 privilege level 15
 local-user test123 ftp-directory flash:
 local-user test123 service-type telnet ssh ftp
#
firewall zone Local
 priority 15
#
interface GigabitEthernet0/0/0
 ip address 192.168.200.5 255.255.255.0 
#
interface GigabitEthernet0/0/1
 ip binding vpn-instance VPN_Out
 ip address 10.0.67.6 255.255.255.0 
#
interface GigabitEthernet0/0/2
 ip binding vpn-instance VPN_In
 ip address 10.0.76.6 255.255.255.0 
#
interface GigabitEthernet4/0/0
 ip address 10.0.16.6 255.255.255.0 
 mpls
 mpls ldp
#
interface NULL0
#
interface LoopBack0
 ip address 6.6.6.6 255.255.255.255 
#
interface LoopBack1
 ip binding vpn-instance VPN_In
 ip address 6.6.6.7 255.255.255.255 
#
interface LoopBack2
 ip binding vpn-instance VPN_Out
 ip address 6.6.6.8 255.255.255.255 
#
bgp 312
 router-id 6.6.6.6
 peer 66.66.66.66 as-number 312 
 peer 66.66.66.66 connect-interface LoopBack0
 #
 ipv4-family unicast
  undo synchronization
  peer 66.66.66.66 enable
 # 
 ipv4-family vpnv4
  policy vpn-target
  peer 66.66.66.66 enable
 #
 ipv4-family vpn-instance VPN_In 
  router-id 6.6.6.7
  peer 7.7.7.7 as-number 100 
  peer 7.7.7.7 ebgp-max-hop 2 
  peer 7.7.7.7 connect-interface LoopBack1
  peer 7.7.7.7 substitute-as
  peer 7.7.7.7 advertise-ext-community
 #
 ipv4-family vpn-instance VPN_Out 
  router-id 6.6.6.8
  peer 7.7.7.7 as-number 100 
  peer 7.7.7.7 ebgp-max-hop 2 
  peer 7.7.7.7 connect-interface LoopBack2
  peer 7.7.7.7 allow-as-loop 2
#
ospf 1 router-id 6.6.6.6 
 area 0.0.0.0 
  network 6.6.6.6 0.0.0.0 
  network 10.0.16.6 0.0.0.0 
#
 sftp server enable 
 stelnet server enable 
#
ip route-static vpn-instance VPN_In 7.7.7.7 255.255.255.255 10.0.76.7
ip route-static vpn-instance VPN_Out 7.7.7.7 255.255.255.255 10.0.67.7
#
user-interface con 0
 authentication-mode password
user-interface vty 0 4
 authentication-mode aaa
 protocol inbound all
user-interface vty 16 20
#
wlan ac
#
return
```

## Fichier : Hub&Spokes MPLS InterAS L3VPN  - BGP - same AS - Option A/Configs exported/LSW_MGMT.cfg

```
#
sysname LSW_MGMT
#
cluster enable
ntdp enable
ndp enable
#
lldp enable
#
drop illegal-mac alarm
#
diffserv domain default
#
drop-profile default
#
aaa 
 authentication-scheme default
 authorization-scheme default
 accounting-scheme default
 domain default 
 domain default_admin 
 local-user admin password simple admin
 local-user admin service-type http
#
interface Vlanif1
#
interface MEth0/0/1
#
interface GigabitEthernet0/0/1
#
interface GigabitEthernet0/0/2
#
interface GigabitEthernet0/0/3
#
interface GigabitEthernet0/0/4
#
interface GigabitEthernet0/0/5
#
interface GigabitEthernet0/0/6
#
interface GigabitEthernet0/0/7
#
interface GigabitEthernet0/0/8
#
interface GigabitEthernet0/0/9
#
interface GigabitEthernet0/0/10
#
interface GigabitEthernet0/0/11
#
interface GigabitEthernet0/0/12
#
interface GigabitEthernet0/0/13
#
interface GigabitEthernet0/0/14
#
interface GigabitEthernet0/0/15
#
interface GigabitEthernet0/0/16
#
interface GigabitEthernet0/0/17
#
interface GigabitEthernet0/0/18
#
interface GigabitEthernet0/0/19
#
interface GigabitEthernet0/0/20
#
interface GigabitEthernet0/0/21
#
interface GigabitEthernet0/0/22
#
interface GigabitEthernet0/0/23
#
interface GigabitEthernet0/0/24
#
interface NULL0
#
user-interface con 0
user-interface vty 0 4
#
return 
```

## Fichier : Hub&Spokes MPLS InterAS L3VPN  - BGP - same AS - Option A/Configs exported/P10.cfg

```
[V200R003C00]
#
 sysname P10
 ftp server enable
#
 snmp-agent local-engineid 800007DB03000000000000
 snmp-agent 
#
 clock timezone China-Standard-Time minus 08:00:00
#
portal local-server load portalpage.zip
#
 drop illegal-mac alarm
#
 lldp enable 
#
 set cpu-usage threshold 80 restore 75
#
mpls lsr-id 10.10.10.10
mpls
#
mpls ldp
#
#
aaa 
 authentication-scheme default
 authorization-scheme default
 accounting-scheme default
 domain default 
 domain default_admin 
 local-user admin password cipher %$%$Ty[pB\'{y'+m=q@3CZ,GKI7j%$%$
 local-user admin service-type http
 local-user test123 password cipher %$%$vr4{QNwSpXg'e-Bfg_,(KW&z%$%$
 local-user test123 privilege level 15
 local-user test123 ftp-directory flash:
 local-user test123 service-type telnet ssh ftp
#
firewall zone Local
 priority 15
#
interface GigabitEthernet0/0/0
 ip address 192.168.200.6 255.255.255.0 
#
interface GigabitEthernet0/0/1
 ip address 10.0.105.10 255.255.255.0 
 mpls
 mpls ldp
#
interface GigabitEthernet0/0/2
 ip address 10.0.108.10 255.255.255.0 
 mpls
 mpls ldp
#
interface NULL0
#
interface LoopBack0
 ip address 10.10.10.10 255.255.255.255 
#
ospf 1 router-id 10.10.10.10 
 area 0.0.0.0 
  network 10.0.105.10 0.0.0.0 
  network 10.0.108.10 0.0.0.0 
  network 10.10.10.10 0.0.0.0 
#
 sftp server enable 
 stelnet server enable 
#
user-interface con 0
 authentication-mode password
user-interface vty 0 4
 authentication-mode aaa
 protocol inbound all
user-interface vty 16 20
#
wlan ac
#
return
```

## Fichier : Hub&Spokes MPLS InterAS L3VPN  - BGP - same AS - Option A/Configs exported/P15.cfg

```
[V200R003C00]
#
 sysname P15
 ftp server enable
#
 snmp-agent local-engineid 800007DB03000000000000
 snmp-agent 
#
 clock timezone China-Standard-Time minus 08:00:00
#
portal local-server load portalpage.zip
#
 drop illegal-mac alarm
#
 lldp enable 
#
 set cpu-usage threshold 80 restore 75
#
mpls lsr-id 15.15.15.15
mpls
#
mpls ldp
#
#
aaa 
 authentication-scheme default
 authorization-scheme default
 accounting-scheme default
 domain default 
 domain default_admin 
 local-user admin password cipher %$%$^=2N(y3M-7rnXSF)N7W*K.Ts%$%$
 local-user admin service-type http
 local-user test123 password cipher %$%$"LVMJ/b$mV;9Q"AYXVK;KW+J%$%$
 local-user test123 privilege level 15
 local-user test123 ftp-directory flash:
 local-user test123 service-type telnet ssh ftp
#
firewall zone Local
 priority 15
#
interface GigabitEthernet0/0/0
 ip address 192.168.200.15 255.255.255.0 
#
interface GigabitEthernet0/0/1
 ip address 10.0.115.15 255.255.255.0 
 mpls
 mpls ldp
#
interface GigabitEthernet0/0/2
 ip address 10.0.153.15 255.255.255.0 
 mpls
 mpls ldp
#
interface NULL0
#
interface LoopBack0
 ip address 15.15.15.15 255.255.255.255 
#
ospf 1 router-id 15.15.15.15 
 area 0.0.0.0 
  network 10.0.115.15 0.0.0.0 
  network 10.0.153.15 0.0.0.0 
  network 15.15.15.15 0.0.0.0 
#
 sftp server enable 
 stelnet server enable 
#
user-interface con 0
 authentication-mode password
user-interface vty 0 4
 authentication-mode aaa
 protocol inbound all
user-interface vty 16 20
#
wlan ac
#
return
```

## Fichier : Hub&Spokes MPLS InterAS L3VPN  - BGP - same AS - Option A/Configs exported/P16.cfg

```
[V200R003C00]
#
 sysname P16
 ftp server enable
#
 snmp-agent local-engineid 800007DB03000000000000
 snmp-agent 
#
 clock timezone China-Standard-Time minus 08:00:00
#
portal local-server load portalpage.zip
#
 drop illegal-mac alarm
#
 lldp enable 
#
 set cpu-usage threshold 80 restore 75
#
mpls lsr-id 16.16.16.16
mpls
#
mpls ldp
#
#
aaa 
 authentication-scheme default
 authorization-scheme default
 accounting-scheme default
 domain default 
 domain default_admin 
 local-user admin password cipher %$%$}y4wK8}@n(~'o*09J9u&K:nZ%$%$
 local-user admin service-type http
 local-user test123 password cipher %$%$lE+-G%Vc'0vk;OS9AZ;GKWSE%$%$
 local-user test123 privilege level 15
 local-user test123 ftp-directory flash:
 local-user test123 service-type telnet ssh ftp
#
firewall zone Local
 priority 15
#
interface GigabitEthernet0/0/0
 ip address 192.168.200.2 255.255.255.0 
#
interface GigabitEthernet0/0/1
 ip address 10.0.16.16 255.255.255.0 
 mpls
 mpls ldp
#
interface GigabitEthernet0/0/2
 ip address 10.0.166.16 255.255.255.0 
 mpls
 mpls ldp
#
interface NULL0
#
interface LoopBack0
 ip address 16.16.16.16 255.255.255.255 
#
ospf 1 router-id 16.16.16.16 
 area 0.0.0.0 
  network 10.0.16.16 0.0.0.0 
  network 10.0.166.16 0.0.0.0 
  network 16.16.16.16 0.0.0.0 
#
 sftp server enable 
 stelnet server enable 
#
user-interface con 0
 authentication-mode password
user-interface vty 0 4
 authentication-mode aaa
 protocol inbound all
user-interface vty 16 20
#
wlan ac
#
return
```

## Fichier : Hub&Spokes MPLS InterAS L3VPN  - BGP - same AS - Option A/Configs exported/P5.cfg

```
[V200R003C00]
#
 sysname P5
 ftp server enable
#
 board add 0/3 1GEC 
 board add 0/4 1GEC 
#
 snmp-agent local-engineid 800007DB03000000000000
 snmp-agent 
#
 clock timezone China-Standard-Time minus 08:00:00
#
portal local-server load portalpage.zip
#
 drop illegal-mac alarm
#
 lldp enable 
#
 set cpu-usage threshold 80 restore 75
#
mpls lsr-id 5.5.5.5
mpls
#
mpls ldp
#
#
aaa 
 authentication-scheme default
 authorization-scheme default
 accounting-scheme default
 domain default 
 domain default_admin 
 local-user admin password cipher %$%$JTOW#Wouq"2S5LTA{y^)KMtk%$%$
 local-user admin service-type http
 local-user test123 password cipher %$%$/odQ9*N#EXnnD"1h`{&3KWT)%$%$
 local-user test123 privilege level 15
 local-user test123 ftp-directory flash:
 local-user test123 service-type telnet ssh ftp
#
firewall zone Local
 priority 15
#
interface GigabitEthernet0/0/0
 ip address 10.0.59.5 255.255.255.0 
 ospf cost 2
 mpls
 mpls ldp
#
interface GigabitEthernet0/0/1
 ip address 10.0.35.5 255.255.255.0 
 mpls
 mpls ldp
#
interface GigabitEthernet0/0/2
 ip address 10.0.45.5 255.255.255.0 
 mpls
 mpls ldp
#
interface GigabitEthernet3/0/0
 ip address 192.168.200.8 255.255.255.0 
#
interface GigabitEthernet4/0/0
 ip address 10.0.105.5 255.255.255.0 
 mpls
 mpls ldp
#
interface NULL0
#
interface LoopBack0
 ip address 5.5.5.5 255.255.255.255 
#
ospf 1 router-id 5.5.5.5 
 area 0.0.0.0 
  network 5.5.5.5 0.0.0.0 
  network 10.0.35.5 0.0.0.0 
  network 10.0.45.5 0.0.0.0 
  network 10.0.59.5 0.0.0.0 
  network 10.0.105.5 0.0.0.0 
#
 sftp server enable 
 stelnet server enable 
#
user-interface con 0
 authentication-mode password
user-interface vty 0 4
 authentication-mode aaa
 protocol inbound all
user-interface vty 16 20
#
wlan ac
#
return
```

## Fichier : Hub&Spokes MPLS InterAS L3VPN  - BGP - same AS - Option A/Configs exported/P8.cfg

```
[V200R003C00]
#
 sysname P8
 ftp server enable
#
 board add 0/4 1GEC 
#
 snmp-agent local-engineid 800007DB03000000000000
 snmp-agent 
#
 clock timezone China-Standard-Time minus 08:00:00
#
portal local-server load portalpage.zip
#
 drop illegal-mac alarm
#
 lldp enable 
#
 set cpu-usage threshold 80 restore 75
#
mpls lsr-id 8.8.8.8
mpls
#
mpls ldp
#
#
aaa 
 authentication-scheme default
 authorization-scheme default
 accounting-scheme default
 domain default 
 domain default_admin 
 local-user admin password cipher %$%$K8m.Nt84DZ}e#<0`8bmE3Uw}%$%$
 local-user admin service-type http
 local-user test123 password cipher %$%$Ydz@6j~Kr"$d#C8j5(f,KWEa%$%$
 local-user test123 privilege level 15
 local-user test123 ftp-directory flash:
 local-user test123 service-type telnet ssh ftp
#
firewall zone Local
 priority 15
#
interface GigabitEthernet0/0/0
 ip address 192.168.200.7 255.255.255.0 
#
interface GigabitEthernet0/0/1
 ip address 10.0.89.8 255.255.255.0 
 mpls
 mpls ldp
#
interface GigabitEthernet0/0/2
 ip address 10.0.108.8 255.255.255.0 
 ospf cost 2
 mpls
 mpls ldp
#
interface GigabitEthernet4/0/0
 ip address 10.0.26.8 255.255.255.0 
 mpls
 mpls ldp
#
interface NULL0
#
interface LoopBack0
 ip address 8.8.8.8 255.255.255.255 
#
ospf 1 router-id 8.8.8.8 
 area 0.0.0.0 
  network 8.8.8.8 0.0.0.0 
  network 10.0.26.8 0.0.0.0 
  network 10.0.89.8 0.0.0.0 
  network 10.0.108.8 0.0.0.0 
#
 sftp server enable 
 stelnet server enable 
#
user-interface con 0
 authentication-mode password
user-interface vty 0 4
 authentication-mode aaa
 protocol inbound all
user-interface vty 16 20
#
wlan ac
#
return
```

## Fichier : Hub&Spokes MPLS InterAS L3VPN  - BGP - same AS - Option A/Configs exported/P9.cfg

```
[V200R003C00]
#
 sysname P9
 ftp server enable
#
 snmp-agent local-engineid 800007DB03000000000000
 snmp-agent 
#
 clock timezone China-Standard-Time minus 08:00:00
#
portal local-server load portalpage.zip
#
 drop illegal-mac alarm
#
 lldp enable 
#
 set cpu-usage threshold 80 restore 75
#
mpls lsr-id 9.9.9.9
mpls
#
mpls ldp
#
#
aaa 
 authentication-scheme default
 authorization-scheme default
 accounting-scheme default
 domain default 
 domain default_admin 
 local-user admin password cipher %$%$Mv[k6a;02*31,-%3{dMPKH.m%$%$
 local-user admin service-type http
 local-user test123 password cipher %$%$<vry--aLRX5tEU2*Mjm*KWq,%$%$
 local-user test123 privilege level 15
 local-user test123 ftp-directory flash:
 local-user test123 service-type telnet ssh ftp
#
firewall zone Local
 priority 15
#
interface GigabitEthernet0/0/0
 ip address 10.0.59.9 255.255.255.0 
 mpls
 mpls ldp
#
interface GigabitEthernet0/0/1
 ip address 10.0.89.9 255.255.255.0 
 mpls
 mpls ldp
#
interface GigabitEthernet0/0/2
 ip address 192.168.200.4 255.255.255.0 
#
interface NULL0
#
interface LoopBack0
 ip address 9.9.9.9 255.255.255.255 
#
ospf 1 router-id 9.9.9.9 
 area 0.0.0.0 
  network 9.9.9.9 0.0.0.0 
  network 10.0.59.9 0.0.0.0 
  network 10.0.89.9 0.0.0.0 
#
 sftp server enable 
 stelnet server enable 
#
user-interface con 0
 authentication-mode password
user-interface vty 0 4
 authentication-mode aaa
 protocol inbound all
user-interface vty 16 20
#
wlan ac
#
return
```

## Fichier : Hub&Spokes MPLS InterAS L3VPN  - BGP - same AS - Option A/Configs exported/PE66.cfg

```
[V200R003C00]
#
 sysname PE66
 ftp server enable
#
 snmp-agent local-engineid 800007DB03000000000000
 snmp-agent 
#
 clock timezone China-Standard-Time minus 08:00:00
#
portal local-server load portalpage.zip
#
 drop illegal-mac alarm
#
 lldp enable 
#
 set cpu-usage threshold 80 restore 75
#
ip vpn-instance site1
 ipv4-family
  route-distinguisher 400:1
  vpn-target 100:1 export-extcommunity
  vpn-target 300:1 import-extcommunity
#
ip vpn-instance site2
 ipv4-family
  route-distinguisher 400:2
  vpn-target 200:1 export-extcommunity
  vpn-target 300:1 import-extcommunity
#
mpls lsr-id 66.66.66.66
mpls
#
mpls ldp
#
#
aaa 
 authentication-scheme default
 authorization-scheme default
 accounting-scheme default
 domain default 
 domain default_admin 
 local-user admin password cipher %$%$d3+R.%}wxD6,V}0TMzE-K=*7%$%$
 local-user admin service-type http
 local-user test123 password cipher %$%$#<Jn*:7sJ/Sm%iF;pbw/KWB<%$%$
 local-user test123 privilege level 15
 local-user test123 ftp-directory flash:
 local-user test123 service-type telnet ssh ftp
#
firewall zone Local
 priority 15
#
interface GigabitEthernet0/0/0
#
interface GigabitEthernet0/0/0.1
 dot1q termination vid 1
 ip binding vpn-instance site1
 ip address 10.0.69.66 255.255.255.0 
 arp broadcast enable
#
interface GigabitEthernet0/0/0.2
 dot1q termination vid 2
 ip binding vpn-instance site2
 ip address 10.1.69.66 255.255.255.0 
 arp broadcast enable
#
interface GigabitEthernet0/0/1
 ip address 192.168.200.10 255.255.255.0 
#
interface GigabitEthernet0/0/2
 ip address 10.0.166.66 255.255.255.0 
 mpls
 mpls ldp
#
interface NULL0
#
interface LoopBack0
 ip address 66.66.66.66 255.255.255.255 
#
interface LoopBack1
 ip binding vpn-instance site1
 ip address 66.66.66.67 255.255.255.255 
#
interface LoopBack2
 ip binding vpn-instance site2
 ip address 66.66.66.68 255.255.255.255 
#
bgp 312
 router-id 66.66.66.66
 peer 6.6.6.6 as-number 312 
 peer 6.6.6.6 connect-interface LoopBack0
 #
 ipv4-family unicast
  undo synchronization
  peer 6.6.6.6 enable
 # 
 ipv4-family vpnv4
  policy vpn-target
  peer 6.6.6.6 enable
  peer 6.6.6.6 allow-as-loop 2
 #
 ipv4-family vpn-instance site1 
  router-id 66.66.66.67
  peer 26.26.26.27 as-number 123 
  peer 26.26.26.27 ebgp-max-hop 2 
  peer 26.26.26.27 connect-interface LoopBack1
  peer 26.26.26.27 substitute-as
  peer 26.26.26.27 advertise-ext-community
 #
 ipv4-family vpn-instance site2 
  router-id 66.66.66.68
  peer 26.26.26.28 as-number 123 
  peer 26.26.26.28 ebgp-max-hop 2 
  peer 26.26.26.28 connect-interface LoopBack2
  peer 26.26.26.28 substitute-as
  peer 26.26.26.28 advertise-ext-community
#
ospf 1 router-id 66.66.66.66 
 area 0.0.0.0 
  network 10.0.166.66 0.0.0.0 
  network 66.66.66.66 0.0.0.0 
#
 sftp server enable 
 stelnet server enable 
#
ip route-static vpn-instance site1 26.26.26.27 255.255.255.255 10.0.69.9
ip route-static vpn-instance site2 26.26.26.28 255.255.255.255 10.1.69.9
#
user-interface con 0
 authentication-mode password
user-interface vty 0 4
 authentication-mode aaa
 protocol inbound all
user-interface vty 16 20
#
wlan ac
#
return
```

## Fichier : Hub&Spokes MPLS InterAS L3VPN  - BGP - same AS - Option A/Configs exported/Spoke-CE1.cfg

```
[V200R003C00]
#
 sysname Spoke_CE1
 ftp server enable
#
 snmp-agent local-engineid 800007DB03000000000000
 snmp-agent 
#
 clock timezone China-Standard-Time minus 08:00:00
#
portal local-server load portalpage.zip
#
 drop illegal-mac alarm
#
 lldp enable 
#
 set cpu-usage threshold 80 restore 75
#
aaa 
 authentication-scheme default
 authorization-scheme default
 accounting-scheme default
 domain default 
 domain default_admin 
 local-user admin password cipher %$%$ClHXK<c%F;hR#J@W=n"2KkZ(%$%$
 local-user admin service-type http
 local-user test123 password cipher %$%$0(C=<\w]lG=%|Q"YSTu=KV$>%$%$
 local-user test123 privilege level 15
 local-user test123 ftp-directory flash:
 local-user test123 service-type telnet ssh ftp
#
firewall zone Local
 priority 15
#
interface GigabitEthernet0/0/0
 ip address 10.0.13.1 255.255.255.0 
#
interface GigabitEthernet0/0/1
 ip address 192.168.200.13 255.255.255.0 
#
interface GigabitEthernet0/0/2
#
interface NULL0
#
interface LoopBack0
 ip address 1.1.1.1 255.255.255.255 
#
interface LoopBack1
 ip address 192.168.1.1 255.255.255.0 
#
bgp 100
 router-id 1.1.1.1
 peer 11.11.11.12 as-number 321 
 peer 11.11.11.12 ebgp-max-hop 2 
 peer 11.11.11.12 connect-interface LoopBack0
 #
 ipv4-family unicast
  undo synchronization
  network 10.0.13.0 255.255.255.0 
  network 192.168.1.0 
  peer 11.11.11.12 enable
#
 sftp server enable 
 stelnet server enable 
#
ip route-static 11.11.11.12 255.255.255.255 10.0.13.11
#
user-interface con 0
 authentication-mode password
user-interface vty 0 4
 authentication-mode aaa
 protocol inbound all
user-interface vty 16 20
#
wlan ac
#
return
```

## Fichier : Hub&Spokes MPLS InterAS L3VPN  - BGP - same AS - Option A/Configs exported/Spoke-CE2.cfg

```
[V200R003C00]
#
 sysname Spoke_CE2
 ftp server enable
#
 snmp-agent local-engineid 800007DB03000000000000
 snmp-agent 
#
 clock timezone China-Standard-Time minus 08:00:00
#
portal local-server load portalpage.zip
#
 drop illegal-mac alarm
#
 lldp enable 
#
 set cpu-usage threshold 80 restore 75
#
aaa 
 authentication-scheme default
 authorization-scheme default
 accounting-scheme default
 domain default 
 domain default_admin 
 local-user admin password cipher %$%$5RqT"Qx8g.cd[`$s[a#)K}Vq%$%$
 local-user admin service-type http
 local-user test123 password cipher %$%$:hqx(Y59$515cH~u"6ODKW\3%$%$
 local-user test123 privilege level 15
 local-user test123 ftp-directory flash:
 local-user test123 service-type telnet ssh ftp
#
firewall zone Local
 priority 15
#
interface GigabitEthernet0/0/0
 ip address 10.0.24.2 255.255.255.0 
#
interface GigabitEthernet0/0/1
 ip address 192.168.200.12 255.255.255.0 
#
interface GigabitEthernet0/0/2
 ip address 10.0.222.2 255.255.255.0 
#
interface NULL0
#
interface LoopBack0
 ip address 2.2.2.2 255.255.255.255 
#
interface LoopBack1
 ip address 172.16.0.1 255.255.255.0 
#
bgp 100
 router-id 2.2.2.2
 peer 4.4.4.5 as-number 123 
 peer 4.4.4.5 ebgp-max-hop 2 
 peer 4.4.4.5 connect-interface LoopBack0
 peer 22.22.22.22 as-number 100 
 peer 22.22.22.22 connect-interface LoopBack0
 #
 ipv4-family unicast
  undo synchronization
  network 10.0.24.0 255.255.255.0 
  network 172.16.0.0 255.255.255.0 
  peer 4.4.4.5 enable
  peer 22.22.22.22 enable
  peer 22.22.22.22 next-hop-local 
#
 sftp server enable 
 stelnet server enable 
#
ip as-path-filter noLoop deny 123 123 123 123
#
ip route-static 4.4.4.5 255.255.255.255 10.0.24.4
ip route-static 22.22.22.22 255.255.255.255 10.0.222.22
#
user-interface con 0
 authentication-mode password
user-interface vty 0 4
 authentication-mode aaa
 protocol inbound all
user-interface vty 16 20
#
wlan ac
#
return
```

## Fichier : Hub&Spokes MPLS InterAS L3VPN  - BGP - same AS - Option A/Configs exported/Spoke-CE22.cfg

```
[V200R003C00]
#
 sysname Spoke-CE22
 ftp server enable
#
 snmp-agent local-engineid 800007DB03000000000000
 snmp-agent 
#
 clock timezone China-Standard-Time minus 08:00:00
#
portal local-server load portalpage.zip
#
 drop illegal-mac alarm
#
 lldp enable 
#
 set cpu-usage threshold 80 restore 75
#
aaa 
 authentication-scheme default
 authorization-scheme default
 accounting-scheme default
 domain default 
 domain default_admin 
 local-user admin password cipher %$%$3&&Y8NU#V@$u`z9&SSpSKv-6%$%$
 local-user admin service-type http
 local-user test123 password cipher %$%$LJPQ.P#_I=VRF,-&,:t8KV)\%$%$
 local-user test123 privilege level 15
 local-user test123 ftp-directory flash:
 local-user test123 service-type telnet ssh ftp
#
firewall zone Local
 priority 15
#
interface GigabitEthernet0/0/0
 ip address 192.168.200.3 255.255.255.0 
#
interface GigabitEthernet0/0/1
 ip address 10.0.224.22 255.255.255.0 
#
interface GigabitEthernet0/0/2
 ip address 10.0.222.22 255.255.255.0 
#
interface NULL0
#
interface LoopBack0
 ip address 22.22.22.22 255.255.255.255 
#
bgp 100
 router-id 22.22.22.22
 peer 2.2.2.2 as-number 100 
 peer 2.2.2.2 connect-interface LoopBack0
 peer 4.4.4.6 as-number 123 
 peer 4.4.4.6 ebgp-max-hop 2 
 peer 4.4.4.6 connect-interface LoopBack0
 #
 ipv4-family unicast
  undo synchronization
  network 10.0.224.0 255.255.255.0 
  peer 2.2.2.2 enable
  peer 2.2.2.2 next-hop-local 
  peer 4.4.4.6 enable
#
 sftp server enable 
 stelnet server enable 
#
ip as-path-filter noLoop deny 123 123 123 123
#
ip route-static 2.2.2.2 255.255.255.255 10.0.222.2
ip route-static 4.4.4.6 255.255.255.255 10.0.224.4
#
user-interface con 0
 authentication-mode password
user-interface vty 0 4
 authentication-mode aaa
 protocol inbound all
user-interface vty 16 20
#
wlan ac
#
return
```

## Fichier : Hub&Spokes MPLS InterAS L3VPN  - BGP - same AS - Option A/Configs exported/Spoke-PE11.cfg

```
[V200R003C00]
#
 sysname Spoke-PE11
 ftp server enable
#
 snmp-agent local-engineid 800007DB03000000000000
 snmp-agent 
#
 clock timezone China-Standard-Time minus 08:00:00
#
portal local-server load portalpage.zip
#
 drop illegal-mac alarm
#
 lldp enable 
#
 set cpu-usage threshold 80 restore 75
#
ip vpn-instance VPN
 ipv4-family
  route-distinguisher 100:1
  vpn-target 100:1 export-extcommunity
  vpn-target 100:1 import-extcommunity
#
mpls lsr-id 11.11.11.11
mpls
#
mpls ldp
#
#
aaa 
 authentication-scheme default
 authorization-scheme default
 accounting-scheme default
 domain default 
 domain default_admin 
 local-user admin password cipher %$%$'V*6+/AYJ$WH/UX*6}59K)IS%$%$
 local-user admin service-type http
 local-user test123 password cipher %$%$`@d")$0<}"R~o^0Y;X7EKVKr%$%$
 local-user test123 privilege level 15
 local-user test123 ftp-directory flash:
 local-user test123 service-type telnet ssh ftp
#
firewall zone Local
 priority 15
#
interface GigabitEthernet0/0/0
 ip binding vpn-instance VPN
 ip address 10.0.13.11 255.255.255.0 
#
interface GigabitEthernet0/0/1
 ip address 10.0.115.11 255.255.255.0 
 mpls
 mpls ldp
#
interface GigabitEthernet0/0/2
 ip address 192.168.200.14 255.255.255.0 
#
interface NULL0
#
interface LoopBack0
 ip address 11.11.11.11 255.255.255.255 
#
interface LoopBack1
 ip binding vpn-instance VPN
 ip address 11.11.11.12 255.255.255.255 
#
bgp 321
 router-id 11.11.11.11
 peer 33.33.33.33 as-number 321 
 peer 33.33.33.33 connect-interface LoopBack0
 #
 ipv4-family unicast
  undo synchronization
  peer 33.33.33.33 enable
 # 
 ipv4-family vpnv4
  policy vpn-target
  peer 33.33.33.33 enable
 #
 ipv4-family vpn-instance VPN 
  peer 1.1.1.1 as-number 100 
  peer 1.1.1.1 ebgp-max-hop 2 
  peer 1.1.1.1 connect-interface LoopBack1
  peer 1.1.1.1 substitute-as
#
ospf 1 router-id 11.11.11.11 
 area 0.0.0.0 
  network 10.0.115.11 0.0.0.0 
  network 11.11.11.11 0.0.0.0 
#
 sftp server enable 
 stelnet server enable 
#
ip route-static vpn-instance VPN 1.1.1.1 255.255.255.255 10.0.13.1
#
user-interface con 0
 authentication-mode password
user-interface vty 0 4
 authentication-mode aaa
 protocol inbound all
user-interface vty 16 20
#
wlan ac
#
return
```

## Fichier : Hub&Spokes MPLS InterAS L3VPN  - BGP - same AS - Option A/Configs exported/Spoke-PE3.cfg

```
[V200R003C00]
#
 sysname Spoke-PE3
 ftp server enable
#
 snmp-agent local-engineid 800007DB03000000000000
 snmp-agent 
#
 clock timezone China-Standard-Time minus 08:00:00
#
portal local-server load portalpage.zip
#
 drop illegal-mac alarm
#
 lldp enable 
#
 set cpu-usage threshold 80 restore 75
#
ip vpn-instance VPN
 ipv4-family
  route-distinguisher 100:3
  vpn-target 100:1 export-extcommunity
  vpn-target 300:1 import-extcommunity
#
mpls lsr-id 3.3.3.3
mpls
#
mpls ldp
#
#
aaa 
 authentication-scheme default
 authorization-scheme default
 accounting-scheme default
 domain default 
 domain default_admin 
 local-user admin password cipher %$%$,N3U=TDp~@1b#_6*pj<RK~Lv%$%$
 local-user admin service-type http
 local-user test123 password cipher %$%$jnoH8(8ZOQt*ESXv[P;"KWM2%$%$
 local-user test123 privilege level 15
 local-user test123 ftp-directory flash:
 local-user test123 service-type telnet ssh ftp
#
firewall zone Local
 priority 15
#
interface GigabitEthernet0/0/0
 ip address 10.0.35.3 255.255.255.0 
 mpls
 mpls ldp
#
interface GigabitEthernet0/0/1
#
interface GigabitEthernet0/0/1.1
 dot1q termination vid 1
 ip binding vpn-instance VPN
 ip address 10.0.133.3 255.255.255.0 
 arp broadcast enable
#
interface GigabitEthernet0/0/2
 ip address 192.168.200.11 255.255.255.0 
#
interface NULL0
#
interface LoopBack0
 ip address 3.3.3.3 255.255.255.255 
#
interface LoopBack1
 ip binding vpn-instance VPN
 ip address 3.3.3.4 255.255.255.255 
#
bgp 123
 router-id 3.3.3.3
 peer 26.26.26.26 as-number 123 
 peer 26.26.26.26 connect-interface LoopBack0
 #
 ipv4-family unicast
  undo synchronization
  peer 26.26.26.26 enable
 # 
 ipv4-family vpnv4
  policy vpn-target
  peer 26.26.26.26 enable
 #
 ipv4-family vpn-instance VPN 
  router-id 3.3.3.4
  peer 33.33.33.34 as-number 321 
  peer 33.33.33.34 ebgp-max-hop 2 
  peer 33.33.33.34 connect-interface LoopBack1
#
ospf 1 router-id 3.3.3.3 
 area 0.0.0.0 
  network 3.3.3.3 0.0.0.0 
  network 10.0.35.3 0.0.0.0 
#
 sftp server enable 
 stelnet server enable 
#
ip route-static vpn-instance VPN 33.33.33.34 255.255.255.255 10.0.133.33
#
user-interface con 0
 authentication-mode password
user-interface vty 0 4
 authentication-mode aaa
 protocol inbound all
user-interface vty 16 20
#
wlan ac
#
return
```

## Fichier : Hub&Spokes MPLS InterAS L3VPN  - BGP - same AS - Option A/Configs exported/Spoke-PE33.cfg

```
[V200R003C00]
#
 sysname Spoke-PE33
 ftp server enable
#
 snmp-agent local-engineid 800007DB03000000000000
 snmp-agent 
#
 clock timezone China-Standard-Time minus 08:00:00
#
portal local-server load portalpage.zip
#
 drop illegal-mac alarm
#
 lldp enable 
#
 set cpu-usage threshold 80 restore 75
#
ip vpn-instance VPN
 ipv4-family
  route-distinguisher 100:2
  vpn-target 100:1 export-extcommunity
  vpn-target 100:1 import-extcommunity
#
mpls lsr-id 33.33.33.33
mpls
#
mpls ldp
#
#
aaa 
 authentication-scheme default
 authorization-scheme default
 accounting-scheme default
 domain default 
 domain default_admin 
 local-user admin password cipher %$%$I|y:@+K^~19'og!Seg%%K12T%$%$
 local-user admin service-type http
 local-user test123 password cipher %$%$3tQ|1V3"SBrb=_6&.sLYKW6I%$%$
 local-user test123 privilege level 15
 local-user test123 ftp-directory flash:
 local-user test123 service-type telnet ssh ftp
#
firewall zone Local
 priority 15
#
interface GigabitEthernet0/0/0
 ip address 192.168.200.16 255.255.255.0 
#
interface GigabitEthernet0/0/1
#
interface GigabitEthernet0/0/1.1
 dot1q termination vid 1
 ip binding vpn-instance VPN
 ip address 10.0.133.33 255.255.255.0 
 arp broadcast enable
#
interface GigabitEthernet0/0/2
 ip address 10.0.153.33 255.255.255.0 
 mpls
 mpls ldp
#
interface NULL0
#
interface LoopBack0
 ip address 33.33.33.33 255.255.255.255 
#
interface LoopBack1
 ip binding vpn-instance VPN
 ip address 33.33.33.34 255.255.255.255 
#
bgp 321
 router-id 33.33.33.33
 peer 11.11.11.11 as-number 321 
 peer 11.11.11.11 connect-interface LoopBack0
 #
 ipv4-family unicast
  undo synchronization
  peer 11.11.11.11 enable
 # 
 ipv4-family vpnv4
  policy vpn-target
  peer 11.11.11.11 enable
 #
 ipv4-family vpn-instance VPN 
  router-id 33.33.33.34
  peer 3.3.3.4 as-number 123 
  peer 3.3.3.4 ebgp-max-hop 2 
  peer 3.3.3.4 connect-interface LoopBack1
#
ospf 1 router-id 33.33.33.33 
 area 0.0.0.0 
  network 10.0.153.33 0.0.0.0 
  network 33.33.33.33 0.0.0.0 
#
 sftp server enable 
 stelnet server enable 
#
ip route-static vpn-instance VPN 3.3.3.4 255.255.255.255 10.0.133.3
#
user-interface con 0
 authentication-mode password
user-interface vty 0 4
 authentication-mode aaa
 protocol inbound all
user-interface vty 16 20
#
wlan ac
#
return
```

## Fichier : Hub&Spokes MPLS InterAS L3VPN  - BGP - same AS - Option A/Configs exported/Spoke-PE4.cfg

```
[V200R003C00]
#
 sysname Spoke-PE4
 ftp server enable
#
 board add 0/4 1GEC 
#
 snmp-agent local-engineid 800007DB03000000000000
 snmp-agent 
#
 clock timezone China-Standard-Time minus 08:00:00
#
portal local-server load portalpage.zip
#
 drop illegal-mac alarm
#
 lldp enable 
#
 set cpu-usage threshold 80 restore 75
#
ip vpn-instance VPN
 ipv4-family
  route-distinguisher 200:1
  vpn-target 200:1 export-extcommunity
  vpn-target 300:1 import-extcommunity
#
mpls lsr-id 4.4.4.4
mpls
#
mpls ldp
#
#
aaa 
 authentication-scheme default
 authorization-scheme default
 accounting-scheme default
 domain default 
 domain default_admin 
 local-user admin password cipher %$%$*It-1y2tz%AWS9=@c/r3KCI^%$%$
 local-user admin service-type http
 local-user test123 password cipher %$%$zomG8/}4T*@t]t-&|JRRKWYy%$%$
 local-user test123 privilege level 15
 local-user test123 ftp-directory flash:
 local-user test123 service-type telnet ssh ftp
#
firewall zone Local
 priority 15
#
interface GigabitEthernet0/0/0
 ip address 10.0.45.4 255.255.255.0 
 mpls
 mpls ldp
#
interface GigabitEthernet0/0/1
 ip binding vpn-instance VPN
 ip address 10.0.24.4 255.255.255.0 
#
interface GigabitEthernet0/0/2
 ip address 192.168.200.1 255.255.255.0 
#
interface GigabitEthernet4/0/0
 ip binding vpn-instance VPN
 ip address 10.0.224.4 255.255.255.0 
#
interface NULL0
#
interface LoopBack0
 ip address 4.4.4.4 255.255.255.255 
#
interface LoopBack1
 ip binding vpn-instance VPN
 ip address 4.4.4.5 255.255.255.255 
#
interface LoopBack2
 ip binding vpn-instance VPN
 ip address 4.4.4.6 255.255.255.255 
#
bgp 123
 router-id 4.4.4.4
 peer 26.26.26.26 as-number 123 
 peer 26.26.26.26 connect-interface LoopBack0
 #
 ipv4-family unicast
  undo synchronization
  peer 26.26.26.26 enable
 # 
 ipv4-family vpnv4
  policy vpn-target
  peer 26.26.26.26 enable
 #
 ipv4-family vpn-instance VPN 
  router-id 4.4.4.5
  peer 2.2.2.2 as-number 100 
  peer 2.2.2.2 ebgp-max-hop 2 
  peer 2.2.2.2 connect-interface LoopBack1
  peer 2.2.2.2 substitute-as
  peer 2.2.2.2 soo 200:1
  peer 22.22.22.22 as-number 100 
  peer 22.22.22.22 ebgp-max-hop 2 
  peer 22.22.22.22 connect-interface LoopBack2
  peer 22.22.22.22 substitute-as
  peer 22.22.22.22 soo 200:1
#
ospf 1 router-id 4.4.4.4 
 area 0.0.0.0 
  network 4.4.4.4 0.0.0.0 
  network 10.0.45.4 0.0.0.0 
#
 sftp server enable 
 stelnet server enable 
#
ip route-static vpn-instance VPN 2.2.2.2 255.255.255.255 10.0.24.2
ip route-static vpn-instance VPN 22.22.22.22 255.255.255.255 10.0.224.22
#
user-interface con 0
 authentication-mode password
user-interface vty 0 4
 authentication-mode aaa
 protocol inbound all
user-interface vty 16 20
#
wlan ac
#
return
```

## Fichier : Hub&Spokes MPLS InterAS L3VPN  - BGP - same AS - Option B/Configs exported/Hub-CE7.cfg

```
#
sysname Hub_CE7
#
aaa 
 authentication-scheme default
 authorization-scheme default
 accounting-scheme default
 domain default 
 domain default_admin 
 local-user admin password cipher <@$^RJZwhW+/Y@:Y>Lw(xa^#
 local-user admin service-type http
#
firewall zone Local
 priority 16
#
interface Ethernet0/0/0
#
interface Ethernet0/0/1
#
interface Serial0/0/0
 link-protocol ppp
#
interface Serial0/0/1
 link-protocol ppp
#
interface Serial0/0/2
 link-protocol ppp
#
interface Serial0/0/3
 link-protocol ppp
#
interface GigabitEthernet0/0/0
#
interface GigabitEthernet0/0/1
 ip address 10.0.67.7 255.255.255.0 
#
interface GigabitEthernet0/0/2
 ip address 10.0.76.7 255.255.255.0 
#
interface GigabitEthernet0/0/3
#
wlan
#
interface NULL0
#
interface LoopBack0
 ip address 7.7.7.7 255.255.255.255 
#
bgp 100
 router-id 7.7.7.7
 peer 6.6.6.7 as-number 312 
 peer 6.6.6.7 ebgp-max-hop 2 
 peer 6.6.6.7 connect-interface LoopBack0
 peer 6.6.6.8 as-number 312 
 peer 6.6.6.8 ebgp-max-hop 2 
 peer 6.6.6.8 connect-interface LoopBack0
 #
 ipv4-family unicast
  undo synchronization
  peer 6.6.6.7 enable
  peer 6.6.6.7 route-policy no-adv-to-VPN_In export
  peer 6.6.6.8 enable
  peer 6.6.6.8 advertise-ext-community
#
route-policy no-adv-to-VPN_In deny node 10 
 if-match ip-prefix match-any 
#
ip ip-prefix match-any index 10 permit 0.0.0.0 0
#
ip route-static 6.6.6.7 255.255.255.255 10.0.76.6
ip route-static 6.6.6.8 255.255.255.255 10.0.67.6
#
user-interface con 0
user-interface vty 0 4
user-interface vty 16 20
#
return 
```

## Fichier : Hub&Spokes MPLS InterAS L3VPN  - BGP - same AS - Option B/Configs exported/Hub-PE26.cfg

```
#
sysname Hub-PE26
#
mpls lsr-id 26.26.26.26
mpls
#
mpls ldp
#
#
aaa 
 authentication-scheme default
 authorization-scheme default
 accounting-scheme default
 domain default 
 domain default_admin 
 local-user admin password cipher gD/VGZfKq7ZypQCee$t3e0%#
 local-user admin service-type http
#
firewall zone Local
 priority 16
#
interface Ethernet0/0/0
#
interface Ethernet0/0/1
#
interface Serial0/0/0
 link-protocol ppp
#
interface Serial0/0/1
 link-protocol ppp
#
interface Serial0/0/2
 link-protocol ppp
#
interface Serial0/0/3
 link-protocol ppp
#
interface GigabitEthernet0/0/0
#
interface GigabitEthernet0/0/1
 ip address 10.0.69.9 255.255.255.0 
 mpls
 mpls ldp
#
interface GigabitEthernet0/0/2
#
interface GigabitEthernet0/0/3
 ip address 10.0.26.9 255.255.255.0 
 mpls
 mpls ldp
#
wlan
#
interface NULL0
#
interface LoopBack0
 ip address 26.26.26.26 255.255.255.255 
#
interface LoopBack1
#
interface LoopBack2
#
bgp 123
 router-id 26.26.26.26
 peer 3.3.3.3 as-number 123 
 peer 3.3.3.3 connect-interface LoopBack0
 peer 4.4.4.4 as-number 123 
 peer 4.4.4.4 connect-interface LoopBack0
 peer 66.66.66.66 as-number 312 
 peer 66.66.66.66 ebgp-max-hop 2 
 peer 66.66.66.66 connect-interface LoopBack0
 #
 ipv4-family unicast
  undo synchronization
  peer 3.3.3.3 enable
  peer 4.4.4.4 enable
  peer 66.66.66.66 enable
 # 
 ipv4-family vpnv4
  undo policy vpn-target
  apply-label per-nexthop
  peer 3.3.3.3 enable
  peer 3.3.3.3 route-policy no-site1 export
  peer 4.4.4.4 enable
  peer 4.4.4.4 route-policy no-site2 export
  peer 66.66.66.66 enable
  peer 66.66.66.66 allow-as-loop 3
#
ospf 1 
 area 0.0.0.0 
  network 10.0.26.9 0.0.0.0 
  network 26.26.26.26 0.0.0.0 
#
route-policy no-site2 deny node 10 
 if-match extcommunity-filter 200:1 
#
route-policy no-site2 permit node 20 
#
route-policy no-site1 deny node 10 
 if-match extcommunity-filter 100:1 
#
route-policy no-site1 permit node 20 
#
ip extcommunity-filter basic 100:1 permit rt 100:1 
ip extcommunity-filter basic 200:1 permit rt 200:1 
#
ip route-static 66.66.66.66 255.255.255.255 10.0.69.66
#
user-interface con 0
user-interface vty 0 4
user-interface vty 16 20
#
return 
```

## Fichier : Hub&Spokes MPLS InterAS L3VPN  - BGP - same AS - Option B/Configs exported/Hub-PE6.cfg

```
#
sysname Hub-PE6
#
ip vpn-instance VPN_In
 ipv4-family
  route-distinguisher 400:3
  vpn-target 100:1 200:1 import-extcommunity
#
ip vpn-instance VPN_Out
 ipv4-family
  route-distinguisher 300:3
  vpn-target 300:1 export-extcommunity
#
mpls lsr-id 6.6.6.6
mpls
#
mpls ldp
#
#
aaa 
 authentication-scheme default
 authorization-scheme default
 accounting-scheme default
 domain default 
 domain default_admin 
 local-user admin password cipher L@mz+H[^kV]@l3D+mKgUIS]#
 local-user admin service-type http
#
firewall zone Local
 priority 16
#
interface Ethernet0/0/0
#
interface Ethernet0/0/1
#
interface Serial0/0/0
 link-protocol ppp
#
interface Serial0/0/1
 link-protocol ppp
#
interface Serial0/0/2
 link-protocol ppp
#
interface Serial0/0/3
 link-protocol ppp
#
interface GigabitEthernet0/0/0
#
interface GigabitEthernet0/0/1
 ip binding vpn-instance VPN_Out
 ip address 10.0.67.6 255.255.255.0 
#
interface GigabitEthernet0/0/2
 ip binding vpn-instance VPN_In
 ip address 10.0.76.6 255.255.255.0 
#
interface GigabitEthernet0/0/3
 ip address 10.0.16.6 255.255.255.0 
 mpls
 mpls ldp
#
wlan
#
interface NULL0
#
interface LoopBack0
 ip address 6.6.6.6 255.255.255.255 
#
interface LoopBack1
 ip binding vpn-instance VPN_In
 ip address 6.6.6.7 255.255.255.255 
#
interface LoopBack2
 ip binding vpn-instance VPN_Out
 ip address 6.6.6.8 255.255.255.255 
#
bgp 312
 router-id 6.6.6.6
 peer 66.66.66.66 as-number 312 
 peer 66.66.66.66 connect-interface LoopBack0
 #
 ipv4-family unicast
  undo synchronization
  peer 66.66.66.66 enable
 # 
 ipv4-family vpnv4
  policy vpn-target
  peer 66.66.66.66 enable
 #
 ipv4-family vpn-instance VPN_In 
  router-id 6.6.6.7
  peer 7.7.7.7 as-number 100 
  peer 7.7.7.7 ebgp-max-hop 2 
  peer 7.7.7.7 connect-interface LoopBack1
  peer 7.7.7.7 substitute-as
  peer 7.7.7.7 advertise-ext-community
 #
 ipv4-family vpn-instance VPN_Out 
  router-id 6.6.6.8
  peer 7.7.7.7 as-number 100 
  peer 7.7.7.7 ebgp-max-hop 2 
  peer 7.7.7.7 connect-interface LoopBack2
  peer 7.7.7.7 allow-as-loop 2
#
ospf 1 router-id 6.6.6.6 
 area 0.0.0.0 
  network 6.6.6.6 0.0.0.0 
  network 10.0.16.6 0.0.0.0 
#
ip route-static vpn-instance VPN_In 7.7.7.7 255.255.255.255 10.0.76.7
ip route-static vpn-instance VPN_Out 7.7.7.7 255.255.255.255 10.0.67.7
#
user-interface con 0
user-interface vty 0 4
user-interface vty 16 20
#
return 
```

## Fichier : Hub&Spokes MPLS InterAS L3VPN  - BGP - same AS - Option B/Configs exported/P10.cfg

```
#
sysname P10
#
mpls lsr-id 10.10.10.10
mpls
#
mpls ldp
#
#
aaa 
 authentication-scheme default
 authorization-scheme default
 accounting-scheme default
 domain default 
 domain default_admin 
 local-user admin password cipher OOCM4m($F4ajUn1vMEIBNUw#
 local-user admin service-type http
#
firewall zone Local
 priority 16
#
interface Ethernet0/0/0
#
interface Ethernet0/0/1
#
interface Serial0/0/0
 link-protocol ppp
#
interface Serial0/0/1
 link-protocol ppp
#
interface Serial0/0/2
 link-protocol ppp
#
interface Serial0/0/3
 link-protocol ppp
#
interface GigabitEthernet0/0/0
#
interface GigabitEthernet0/0/1
#
interface GigabitEthernet0/0/2
 ip address 10.0.108.10 255.255.255.0 
 mpls
 mpls ldp
#
interface GigabitEthernet0/0/3
 ip address 10.0.105.10 255.255.255.0 
 mpls
 mpls ldp
#
wlan
#
interface NULL0
#
interface LoopBack0
 ip address 10.10.10.10 255.255.255.255 
#
ospf 1 router-id 10.10.10.10 
 area 0.0.0.0 
  network 10.0.108.10 0.0.0.0 
  network 10.0.105.10 0.0.0.0 
  network 10.10.10.10 0.0.0.0 
#
user-interface con 0
user-interface vty 0 4
user-interface vty 16 20
#
return 
```

## Fichier : Hub&Spokes MPLS InterAS L3VPN  - BGP - same AS - Option B/Configs exported/P15.cfg

```
#
sysname P15
#
mpls lsr-id 15.15.15.15
mpls
#
mpls ldp
#
#
aaa 
 authentication-scheme default
 authorization-scheme default
 accounting-scheme default
 domain default 
 domain default_admin 
 local-user admin password cipher OOCM4m($F4ajUn1vMEIBNUw#
 local-user admin service-type http
#
firewall zone Local
 priority 16
#
interface Ethernet0/0/0
#
interface Ethernet0/0/1
#
interface Serial0/0/0
 link-protocol ppp
#
interface Serial0/0/1
 link-protocol ppp
#
interface Serial0/0/2
 link-protocol ppp
#
interface Serial0/0/3
 link-protocol ppp
#
interface GigabitEthernet0/0/0
#
interface GigabitEthernet0/0/1
 ip address 10.0.115.15 255.255.255.0 
 mpls
 mpls ldp
#
interface GigabitEthernet0/0/2
 ip address 10.0.153.15 255.255.255.0 
 mpls
 mpls ldp
#
interface GigabitEthernet0/0/3
#
wlan
#
interface NULL0
#
interface LoopBack0
 ip address 15.15.15.15 255.255.255.255 
#
ospf 1 router-id 15.15.15.15 
 area 0.0.0.0 
  network 15.15.15.15 0.0.0.0 
  network 10.0.115.15 0.0.0.0 
  network 10.0.153.15 0.0.0.0 
#
user-interface con 0
user-interface vty 0 4
user-interface vty 16 20
#
return 
```

## Fichier : Hub&Spokes MPLS InterAS L3VPN  - BGP - same AS - Option B/Configs exported/P16.cfg

```
#
sysname P16
#
mpls lsr-id 16.16.16.16
mpls
#
mpls ldp
#
#
aaa 
 authentication-scheme default
 authorization-scheme default
 accounting-scheme default
 domain default 
 domain default_admin 
 local-user admin password cipher %~f8LuqYKY=H)H2[EInB8r9#
 local-user admin service-type http
#
firewall zone Local
 priority 16
#
interface Ethernet0/0/0
#
interface Ethernet0/0/1
#
interface Serial0/0/0
 link-protocol ppp
#
interface Serial0/0/1
 link-protocol ppp
#
interface Serial0/0/2
 link-protocol ppp
#
interface Serial0/0/3
 link-protocol ppp
#
interface GigabitEthernet0/0/0
#
interface GigabitEthernet0/0/1
#
interface GigabitEthernet0/0/2
 ip address 10.0.166.16 255.255.255.0 
 mpls
 mpls ldp
#
interface GigabitEthernet0/0/3
 ip address 10.0.16.16 255.255.255.0 
 mpls
 mpls ldp
#
wlan
#
interface NULL0
#
interface LoopBack0
 ip address 16.16.16.16 255.255.255.255 
#
ospf 1 router-id 16.16.16.16 
 area 0.0.0.0 
  network 16.16.16.16 0.0.0.0 
  network 10.0.166.16 0.0.0.0 
  network 10.0.16.16 0.0.0.0 
#
user-interface con 0
user-interface vty 0 4
user-interface vty 16 20
#
return 
```

## Fichier : Hub&Spokes MPLS InterAS L3VPN  - BGP - same AS - Option B/Configs exported/P5.cfg

```
#
sysname P5
#
mpls lsr-id 5.5.5.5
mpls
#
mpls ldp
#
#
aaa 
 authentication-scheme default
 authorization-scheme default
 accounting-scheme default
 domain default 
 domain default_admin 
 local-user admin password cipher EX/TQ-xLj@3@9_G-B0Y2&KE#
 local-user admin service-type http
#
firewall zone Local
 priority 16
#
interface Ethernet0/0/0
#
interface Ethernet0/0/1
#
interface Serial0/0/0
 link-protocol ppp
#
interface Serial0/0/1
 link-protocol ppp
#
interface Serial0/0/2
 link-protocol ppp
#
interface Serial0/0/3
 link-protocol ppp
#
interface GigabitEthernet0/0/0
 ip address 10.0.59.5 255.255.255.0 
 ospf cost 2
 mpls
 mpls ldp
#
interface GigabitEthernet0/0/1
 ip address 10.0.35.5 255.255.255.0 
 mpls
 mpls ldp
#
interface GigabitEthernet0/0/2
 ip address 10.0.45.5 255.255.255.0 
 mpls
 mpls ldp
#
interface GigabitEthernet0/0/3
 ip address 10.0.105.5 255.255.255.0 
 mpls
 mpls ldp
#
wlan
#
interface NULL0
#
interface LoopBack0
 ip address 5.5.5.5 255.255.255.255 
#
ospf 1 router-id 5.5.5.5 
 area 0.0.0.0 
  network 10.0.35.5 0.0.0.0 
  network 10.0.45.5 0.0.0.0 
  network 5.5.5.5 0.0.0.0 
  network 10.0.59.5 0.0.0.0 
  network 10.0.105.5 0.0.0.0 
#
user-interface con 0
user-interface vty 0 4
user-interface vty 16 20
#
return 
```

## Fichier : Hub&Spokes MPLS InterAS L3VPN  - BGP - same AS - Option B/Configs exported/P8.cfg

```
#
sysname P8
#
mpls lsr-id 8.8.8.8
mpls
#
mpls ldp
#
#
aaa 
 authentication-scheme default
 authorization-scheme default
 accounting-scheme default
 domain default 
 domain default_admin 
 local-user admin password cipher V.yY.=Sf9Ipe}@HMNPn@>aa#
 local-user admin service-type http
#
firewall zone Local
 priority 16
#
interface Ethernet0/0/0
#
interface Ethernet0/0/1
#
interface Serial0/0/0
 link-protocol ppp
#
interface Serial0/0/1
 link-protocol ppp
#
interface Serial0/0/2
 link-protocol ppp
#
interface Serial0/0/3
 link-protocol ppp
#
interface GigabitEthernet0/0/0
#
interface GigabitEthernet0/0/1
 ip address 10.0.89.8 255.255.255.0 
 mpls
 mpls ldp
#
interface GigabitEthernet0/0/2
 ip address 10.0.108.8 255.255.255.0 
 ospf cost 2
 mpls
 mpls ldp
#
interface GigabitEthernet0/0/3
 ip address 10.0.26.8 255.255.255.0 
 mpls
 mpls ldp
#
wlan
#
interface NULL0
#
interface LoopBack0
 ip address 8.8.8.8 255.255.255.255 
#
ospf 1 router-id 8.8.8.8 
 area 0.0.0.0 
  network 8.8.8.8 0.0.0.0 
  network 10.0.89.8 0.0.0.0 
  network 10.0.108.8 0.0.0.0 
  network 10.0.26.8 0.0.0.0 
#
user-interface con 0
user-interface vty 0 4
user-interface vty 16 20
#
return 
```

## Fichier : Hub&Spokes MPLS InterAS L3VPN  - BGP - same AS - Option B/Configs exported/P9.cfg

```
#
sysname P9
#
mpls lsr-id 9.9.9.9
mpls
#
mpls ldp
#
#
aaa 
 authentication-scheme default
 authorization-scheme default
 accounting-scheme default
 domain default 
 domain default_admin 
 local-user admin password cipher OOCM4m($F4ajUn1vMEIBNUw#
 local-user admin service-type http
#
firewall zone Local
 priority 16
#
interface Ethernet0/0/0
#
interface Ethernet0/0/1
#
interface Serial0/0/0
 link-protocol ppp
#
interface Serial0/0/1
 link-protocol ppp
#
interface Serial0/0/2
 link-protocol ppp
#
interface Serial0/0/3
 link-protocol ppp
#
interface GigabitEthernet0/0/0
 ip address 10.0.59.9 255.255.255.0 
 mpls
 mpls ldp
#
interface GigabitEthernet0/0/1
 ip address 10.0.89.9 255.255.255.0 
 mpls
 mpls ldp
#
interface GigabitEthernet0/0/2
#
interface GigabitEthernet0/0/3
#
wlan
#
interface NULL0
#
interface LoopBack0
 ip address 9.9.9.9 255.255.255.255 
#
ospf 1 router-id 9.9.9.9 
 area 0.0.0.0 
  network 10.0.89.9 0.0.0.0 
  network 10.0.59.9 0.0.0.0 
  network 9.9.9.9 0.0.0.0 
#
user-interface con 0
user-interface vty 0 4
user-interface vty 16 20
#
return 
```

## Fichier : Hub&Spokes MPLS InterAS L3VPN  - BGP - same AS - Option B/Configs exported/PE66.cfg

```
#
sysname PE66
#
mpls lsr-id 66.66.66.66
mpls
#
mpls ldp
#
#
aaa 
 authentication-scheme default
 authorization-scheme default
 accounting-scheme default
 domain default 
 domain default_admin 
 local-user admin password cipher >^R6#_pU[U939O4.`(ZGs0'#
 local-user admin service-type http
#
firewall zone Local
 priority 16
#
interface Ethernet0/0/0
#
interface Ethernet0/0/1
#
interface Serial0/0/0
 link-protocol ppp
#
interface Serial0/0/1
 link-protocol ppp
#
interface Serial0/0/2
 link-protocol ppp
#
interface Serial0/0/3
 link-protocol ppp
#
interface GigabitEthernet0/0/0
#
interface GigabitEthernet0/0/1
 ip address 10.0.69.66 255.255.255.0 
 mpls
 mpls ldp
#
interface GigabitEthernet0/0/2
#
interface GigabitEthernet0/0/3
 ip address 10.0.166.66 255.255.255.0 
 mpls
 mpls ldp
#
wlan
#
interface NULL0
#
interface LoopBack0
 ip address 66.66.66.66 255.255.255.255 
#
interface LoopBack1
#
interface LoopBack2
#
bgp 312
 router-id 66.66.66.66
 peer 6.6.6.6 as-number 312 
 peer 6.6.6.6 connect-interface LoopBack0
 peer 26.26.26.26 as-number 123 
 peer 26.26.26.26 ebgp-max-hop 2 
 peer 26.26.26.26 connect-interface LoopBack0
 #
 ipv4-family unicast
  undo synchronization
  peer 6.6.6.6 enable
  peer 26.26.26.26 enable
 # 
 ipv4-family vpnv4
  undo policy vpn-target
  apply-label per-nexthop
  peer 6.6.6.6 enable
  peer 6.6.6.6 allow-as-loop 2
  peer 26.26.26.26 enable
#
ospf 1 router-id 66.66.66.66 
 area 0.0.0.0 
  network 66.66.66.66 0.0.0.0 
  network 10.0.166.66 0.0.0.0 
#
ip route-static 26.26.26.26 255.255.255.255 10.0.69.9
#
user-interface con 0
user-interface vty 0 4
user-interface vty 16 20
#
return 
```

## Fichier : Hub&Spokes MPLS InterAS L3VPN  - BGP - same AS - Option B/Configs exported/Spoke-CE1.cfg

```
#
sysname Spoke_CE1
#
aaa 
 authentication-scheme default
 authorization-scheme default
 accounting-scheme default
 domain default 
 domain default_admin 
 local-user admin password cipher _HKiN-Koq!ZypQCee$t3:ae#
 local-user admin service-type http
#
firewall zone Local
 priority 16
#
interface Ethernet0/0/0
#
interface Ethernet0/0/1
#
interface Serial0/0/0
 link-protocol ppp
#
interface Serial0/0/1
 link-protocol ppp
#
interface Serial0/0/2
 link-protocol ppp
#
interface Serial0/0/3
 link-protocol ppp
#
interface GigabitEthernet0/0/0
 ip address 10.0.13.1 255.255.255.0 
#
interface GigabitEthernet0/0/1
#
interface GigabitEthernet0/0/2
#
interface GigabitEthernet0/0/3
#
wlan
#
interface NULL0
#
interface LoopBack0
 ip address 1.1.1.1 255.255.255.255 
#
interface LoopBack1
 ip address 192.168.1.1 255.255.255.0 
#
bgp 100
 router-id 1.1.1.1
 peer 11.11.11.12 as-number 321 
 peer 11.11.11.12 ebgp-max-hop 2 
 peer 11.11.11.12 connect-interface LoopBack0
 #
 ipv4-family unicast
  undo synchronization
  network 10.0.13.0 255.255.255.0 
  network 192.168.1.0 
  peer 11.11.11.12 enable
#
ip route-static 11.11.11.12 255.255.255.255 10.0.13.11
#
user-interface con 0
user-interface vty 0 4
user-interface vty 16 20
#
return 
```

## Fichier : Hub&Spokes MPLS InterAS L3VPN  - BGP - same AS - Option B/Configs exported/Spoke-CE2.cfg

```
#
sysname Spoke_CE2
#
aaa 
 authentication-scheme default
 authorization-scheme default
 accounting-scheme default
 domain default 
 domain default_admin 
 local-user admin password cipher yAXdS2kGKWZypQCee$t3L[%#
 local-user admin service-type http
#
firewall zone Local
 priority 16
#
interface Ethernet0/0/0
#
interface Ethernet0/0/1
#
interface Serial0/0/0
 link-protocol ppp
#
interface Serial0/0/1
 link-protocol ppp
#
interface Serial0/0/2
 link-protocol ppp
#
interface Serial0/0/3
 link-protocol ppp
#
interface GigabitEthernet0/0/0
 ip address 10.0.24.2 255.255.255.0 
#
interface GigabitEthernet0/0/1
#
interface GigabitEthernet0/0/2
 ip address 10.0.222.2 255.255.255.0 
#
interface GigabitEthernet0/0/3
#
wlan
#
interface NULL0
#
interface LoopBack0
 ip address 2.2.2.2 255.255.255.255 
#
interface LoopBack1
 ip address 172.16.0.1 255.255.255.0 
#
bgp 100
 router-id 2.2.2.2
 peer 4.4.4.5 as-number 123 
 peer 4.4.4.5 ebgp-max-hop 2 
 peer 4.4.4.5 connect-interface LoopBack0
 peer 22.22.22.22 as-number 100 
 peer 22.22.22.22 connect-interface LoopBack0
 #
 ipv4-family unicast
  undo synchronization
  network 10.0.24.0 255.255.255.0 
  network 172.16.0.0 255.255.255.0 
  peer 4.4.4.5 enable
  peer 22.22.22.22 enable
  peer 22.22.22.22 next-hop-local 
#
ip as-path-filter noLoop deny 123 123 123 123
#
ip route-static 4.4.4.5 255.255.255.255 10.0.24.4
ip route-static 22.22.22.22 255.255.255.255 10.0.222.22
#
user-interface con 0
user-interface vty 0 4
user-interface vty 16 20
#
return 
```

## Fichier : Hub&Spokes MPLS InterAS L3VPN  - BGP - same AS - Option B/Configs exported/Spoke-CE22.cfg

```
#
sysname Spoke-CE22
#
aaa 
 authentication-scheme default
 authorization-scheme default
 accounting-scheme default
 domain default 
 domain default_admin 
 local-user admin password cipher ~=3;Hj'/0F+/Y@:Y>Lw(T[##
 local-user admin service-type http
#
firewall zone Local
 priority 16
#
interface Ethernet0/0/0
#
interface Ethernet0/0/1
#
interface Serial0/0/0
 link-protocol ppp
#
interface Serial0/0/1
 link-protocol ppp
#
interface Serial0/0/2
 link-protocol ppp
#
interface Serial0/0/3
 link-protocol ppp
#
interface GigabitEthernet0/0/0
#
interface GigabitEthernet0/0/1
#
interface GigabitEthernet0/0/2
 ip address 10.0.222.22 255.255.255.0 
#
interface GigabitEthernet0/0/3
 ip address 10.0.224.22 255.255.255.0 
#
wlan
#
interface NULL0
#
interface LoopBack0
 ip address 22.22.22.22 255.255.255.255 
#
bgp 100
 router-id 22.22.22.22
 peer 2.2.2.2 as-number 100 
 peer 2.2.2.2 connect-interface LoopBack0
 peer 4.4.4.6 as-number 123 
 peer 4.4.4.6 ebgp-max-hop 2 
 peer 4.4.4.6 connect-interface LoopBack0
 #
 ipv4-family unicast
  undo synchronization
  network 10.0.224.0 255.255.255.0 
  peer 2.2.2.2 enable
  peer 2.2.2.2 next-hop-local 
  peer 4.4.4.6 enable
#
ip as-path-filter noLoop deny 123 123 123 123
#
ip route-static 2.2.2.2 255.255.255.255 10.0.222.2
ip route-static 4.4.4.6 255.255.255.255 10.0.224.4
#
user-interface con 0
user-interface vty 0 4
user-interface vty 16 20
#
return 
```

## Fichier : Hub&Spokes MPLS InterAS L3VPN  - BGP - same AS - Option B/Configs exported/Spoke-PE11.cfg

```
#
sysname Spoke-PE11
#
ip vpn-instance VPN
 ipv4-family
  route-distinguisher 100:1
  vpn-target 100:1 export-extcommunity
  vpn-target 300:1 import-extcommunity
#
mpls lsr-id 11.11.11.11
mpls
#
mpls ldp
#
#
aaa 
 authentication-scheme default
 authorization-scheme default
 accounting-scheme default
 domain default 
 domain default_admin 
 local-user admin password cipher @)1]-"U`o(:z9:%F`[a=y/R#
 local-user admin service-type http
#
firewall zone Local
 priority 16
#
interface Ethernet0/0/0
#
interface Ethernet0/0/1
#
interface Serial0/0/0
 link-protocol ppp
#
interface Serial0/0/1
 link-protocol ppp
#
interface Serial0/0/2
 link-protocol ppp
#
interface Serial0/0/3
 link-protocol ppp
#
interface GigabitEthernet0/0/0
 ip binding vpn-instance VPN
 ip address 10.0.13.11 255.255.255.0 
#
interface GigabitEthernet0/0/1
 ip address 10.0.115.11 255.255.255.0 
 mpls
 mpls ldp
#
interface GigabitEthernet0/0/2
#
interface GigabitEthernet0/0/3
#
wlan
#
interface NULL0
#
interface LoopBack0
 ip address 11.11.11.11 255.255.255.255 
#
interface LoopBack1
 ip binding vpn-instance VPN
 ip address 11.11.11.12 255.255.255.255 
#
bgp 321
 router-id 11.11.11.11
 peer 33.33.33.33 as-number 321 
 peer 33.33.33.33 connect-interface LoopBack0
 #
 ipv4-family unicast
  undo synchronization
  peer 33.33.33.33 enable
 # 
 ipv4-family vpnv4
  policy vpn-target
  peer 33.33.33.33 enable
 #
 ipv4-family vpn-instance VPN 
  peer 1.1.1.1 as-number 100 
  peer 1.1.1.1 ebgp-max-hop 2 
  peer 1.1.1.1 connect-interface LoopBack1
  peer 1.1.1.1 substitute-as
#
ospf 1 router-id 11.11.11.11 
 area 0.0.0.0 
  network 11.11.11.11 0.0.0.0 
  network 10.0.115.11 0.0.0.0 
#
ip route-static vpn-instance VPN 1.1.1.1 255.255.255.255 10.0.13.1
#
user-interface con 0
user-interface vty 0 4
user-interface vty 16 20
#
return 
```

## Fichier : Hub&Spokes MPLS InterAS L3VPN  - BGP - same AS - Option B/Configs exported/Spoke-PE3.cfg

```
#
sysname Spoke-PE3
#
mpls lsr-id 3.3.3.3
mpls
#
mpls ldp
#
#
aaa 
 authentication-scheme default
 authorization-scheme default
 accounting-scheme default
 domain default 
 domain default_admin 
 local-user admin password cipher [d!V4]iv8O@X,k6.E\Z,G/S#
 local-user admin service-type http
#
firewall zone Local
 priority 16
#
interface Ethernet0/0/0
#
interface Ethernet0/0/1
#
interface Serial0/0/0
 link-protocol ppp
#
interface Serial0/0/1
 link-protocol ppp
#
interface Serial0/0/2
 link-protocol ppp
#
interface Serial0/0/3
 link-protocol ppp
#
interface GigabitEthernet0/0/0
 ip address 10.0.35.3 255.255.255.0 
 mpls
 mpls ldp
#
interface GigabitEthernet0/0/1
 ip address 10.0.133.3 255.255.255.0 
 mpls
 mpls ldp
#
interface GigabitEthernet0/0/2
#
interface GigabitEthernet0/0/3
#
wlan
#
interface NULL0
#
interface LoopBack0
 ip address 3.3.3.3 255.255.255.255 
#
interface LoopBack1
#
bgp 123
 router-id 3.3.3.3
 peer 26.26.26.26 as-number 123 
 peer 26.26.26.26 connect-interface LoopBack0
 peer 33.33.33.33 as-number 321 
 peer 33.33.33.33 ebgp-max-hop 2 
 peer 33.33.33.33 connect-interface LoopBack0
 #
 ipv4-family unicast
  undo synchronization
  peer 26.26.26.26 enable
  peer 33.33.33.33 enable
 # 
 ipv4-family vpnv4
  undo policy vpn-target
  apply-label per-nexthop
  peer 26.26.26.26 enable
  peer 33.33.33.33 enable
#
ospf 1 router-id 3.3.3.3 
 area 0.0.0.0 
  network 3.3.3.3 0.0.0.0 
  network 10.0.35.3 0.0.0.0 
#
ip route-static 33.33.33.33 255.255.255.255 10.0.133.33
#
user-interface con 0
user-interface vty 0 4
user-interface vty 16 20
#
return 
```

## Fichier : Hub&Spokes MPLS InterAS L3VPN  - BGP - same AS - Option B/Configs exported/Spoke-PE33.cfg

```
#
sysname Spoke-PE33
#
mpls lsr-id 33.33.33.33
mpls
#
mpls ldp
#
#
aaa 
 authentication-scheme default
 authorization-scheme default
 accounting-scheme default
 domain default 
 domain default_admin 
 local-user admin password cipher [d!V4]iv8ObL^B&WSBiQ5/[#
 local-user admin service-type http
#
firewall zone Local
 priority 16
#
interface Ethernet0/0/0
#
interface Ethernet0/0/1
#
interface Serial0/0/0
 link-protocol ppp
#
interface Serial0/0/1
 link-protocol ppp
#
interface Serial0/0/2
 link-protocol ppp
#
interface Serial0/0/3
 link-protocol ppp
#
interface GigabitEthernet0/0/0
#
interface GigabitEthernet0/0/1
 ip address 10.0.133.33 255.255.255.0 
 mpls
 mpls ldp
#
interface GigabitEthernet0/0/2
 ip address 10.0.153.33 255.255.255.0 
 mpls
 mpls ldp
#
interface GigabitEthernet0/0/3
#
wlan
#
interface NULL0
#
interface LoopBack0
 ip address 33.33.33.33 255.255.255.255 
#
interface LoopBack1
#
bgp 321
 router-id 33.33.33.33
 peer 3.3.3.3 as-number 123 
 peer 3.3.3.3 ebgp-max-hop 2 
 peer 3.3.3.3 connect-interface LoopBack0
 peer 11.11.11.11 as-number 321 
 peer 11.11.11.11 connect-interface LoopBack0
 #
 ipv4-family unicast
  undo synchronization
  peer 3.3.3.3 enable
  peer 11.11.11.11 enable
 # 
 ipv4-family vpnv4
  undo policy vpn-target
  apply-label per-nexthop
  peer 3.3.3.3 enable
  peer 11.11.11.11 enable
#
ospf 1 router-id 33.33.33.33 
 area 0.0.0.0 
  network 33.33.33.33 0.0.0.0 
  network 10.0.153.33 0.0.0.0 
#
ip route-static 3.3.3.3 255.255.255.255 10.0.133.3
#
user-interface con 0
user-interface vty 0 4
user-interface vty 16 20
#
return 
```

## Fichier : Hub&Spokes MPLS InterAS L3VPN  - BGP - same AS - Option B/Configs exported/Spoke-PE4.cfg

```
#
sysname Spoke-PE4
#
ip vpn-instance VPN
 ipv4-family
  route-distinguisher 200:1
  vpn-target 200:1 export-extcommunity
  vpn-target 300:1 import-extcommunity
#
mpls lsr-id 4.4.4.4
mpls
#
mpls ldp
#
#
aaa 
 authentication-scheme default
 authorization-scheme default
 accounting-scheme default
 domain default 
 domain default_admin 
 local-user admin password cipher uj~Q%10=#N3IF$':[285rbX#
 local-user admin service-type http
#
firewall zone Local
 priority 16
#
interface Ethernet0/0/0
#
interface Ethernet0/0/1
#
interface Serial0/0/0
 link-protocol ppp
#
interface Serial0/0/1
 link-protocol ppp
#
interface Serial0/0/2
 link-protocol ppp
#
interface Serial0/0/3
 link-protocol ppp
#
interface GigabitEthernet0/0/0
 ip address 10.0.45.4 255.255.255.0 
 mpls
 mpls ldp
#
interface GigabitEthernet0/0/1
 ip binding vpn-instance VPN
 ip address 10.0.24.4 255.255.255.0 
#
interface GigabitEthernet0/0/2
#
interface GigabitEthernet0/0/3
 ip binding vpn-instance VPN
 ip address 10.0.224.4 255.255.255.0 
#
wlan
#
interface NULL0
#
interface LoopBack0
 ip address 4.4.4.4 255.255.255.255 
#
interface LoopBack1
 ip binding vpn-instance VPN
 ip address 4.4.4.5 255.255.255.255 
#
interface LoopBack2
 ip binding vpn-instance VPN
 ip address 4.4.4.6 255.255.255.255 
#
bgp 123
 router-id 4.4.4.4
 peer 26.26.26.26 as-number 123 
 peer 26.26.26.26 connect-interface LoopBack0
 #
 ipv4-family unicast
  undo synchronization
  peer 26.26.26.26 enable
 # 
 ipv4-family vpnv4
  policy vpn-target
  peer 26.26.26.26 enable
 #
 ipv4-family vpn-instance VPN 
  router-id 4.4.4.5
  peer 2.2.2.2 as-number 100 
  peer 2.2.2.2 ebgp-max-hop 2 
  peer 2.2.2.2 connect-interface LoopBack1
  peer 2.2.2.2 substitute-as
  peer 2.2.2.2 soo 200:1
  peer 22.22.22.22 as-number 100 
  peer 22.22.22.22 ebgp-max-hop 2 
  peer 22.22.22.22 connect-interface LoopBack2
  peer 22.22.22.22 substitute-as
  peer 22.22.22.22 soo 200:1
#
ospf 1 router-id 4.4.4.4 
 area 0.0.0.0 
  network 4.4.4.4 0.0.0.0 
  network 10.0.45.4 0.0.0.0 
#
ip route-static vpn-instance VPN 2.2.2.2 255.255.255.255 10.0.24.2
ip route-static vpn-instance VPN 22.22.22.22 255.255.255.255 10.0.224.22
#
user-interface con 0
user-interface vty 0 4
user-interface vty 16 20
#
return 
```

## Fichier : MPLS L3VPN with BGP - Intranet/Exported Config/CE1.cfg

```
#
sysname CE1
#
aaa 
 authentication-scheme default
 authorization-scheme default
 accounting-scheme default
 domain default 
 domain default_admin 
 local-user admin password cipher OOCM4m($F4ajUn1vMEIBNUw#
 local-user admin service-type http
#
firewall zone Local
 priority 16
#
interface Ethernet0/0/0
#
interface Ethernet0/0/1
#
interface Serial0/0/0
 link-protocol ppp
#
interface Serial0/0/1
 link-protocol ppp
#
interface Serial0/0/2
 link-protocol ppp
#
interface Serial0/0/3
 link-protocol ppp
#
interface GigabitEthernet0/0/0
 ip address 192.168.100.1 255.255.255.0 
#
interface GigabitEthernet0/0/1
#
interface GigabitEthernet0/0/2
#
interface GigabitEthernet0/0/3
#
wlan
#
interface NULL0
#
interface LoopBack0
 ip address 10.0.1.1 255.255.255.255 
#
interface LoopBack1
 ip address 192.168.1.1 255.255.255.0 
 ospf network-type broadcast
#
ospf 1 router-id 10.0.1.1 
 area 0.0.0.0 
  network 192.168.1.0 0.0.0.255 
  network 192.168.100.1 0.0.0.0 
#
user-interface con 0
user-interface vty 0 4
user-interface vty 16 20
#
return 
```

## Fichier : MPLS L3VPN with BGP - Intranet/Exported Config/CE2.cfg

```
#
sysname CE2
#
aaa 
 authentication-scheme default
 authorization-scheme default
 accounting-scheme default
 domain default 
 domain default_admin 
 local-user admin password cipher OOCM4m($F4ajUn1vMEIBNUw#
 local-user admin service-type http
#
firewall zone Local
 priority 16
#
interface Ethernet0/0/0
#
interface Ethernet0/0/1
#
interface Serial0/0/0
 link-protocol ppp
#
interface Serial0/0/1
 link-protocol ppp
#
interface Serial0/0/2
 link-protocol ppp
#
interface Serial0/0/3
 link-protocol ppp
#
interface GigabitEthernet0/0/0
 ip address 192.168.100.1 255.255.255.0 
#
interface GigabitEthernet0/0/1
#
interface GigabitEthernet0/0/2
#
interface GigabitEthernet0/0/3
#
wlan
#
interface NULL0
#
interface LoopBack0
 ip address 10.0.2.2 255.255.255.255 
#
interface LoopBack1
 ip address 192.168.1.2 255.255.255.0 
#
bgp 100
 router-id 10.0.2.2
 peer 1.1.1.1 as-number 123 
 peer 1.1.1.1 ebgp-max-hop 2 
 peer 1.1.1.1 connect-interface LoopBack0
 #
 ipv4-family unicast
  undo synchronization
  network 192.168.1.0 
  peer 1.1.1.1 enable
#
ip route-static 1.1.1.1 255.255.255.255 192.168.100.2
#
user-interface con 0
user-interface vty 0 4
user-interface vty 16 20
#
return 
```

## Fichier : MPLS L3VPN with BGP - Intranet/Exported Config/CE3.cfg

```
#
sysname CE3
#
aaa 
 authentication-scheme default
 authorization-scheme default
 accounting-scheme default
 domain default 
 domain default_admin 
 local-user admin password cipher OOCM4m($F4ajUn1vMEIBNUw#
 local-user admin service-type http
#
firewall zone Local
 priority 16
#
interface Ethernet0/0/0
#
interface Ethernet0/0/1
#
interface Serial0/0/0
 link-protocol ppp
#
interface Serial0/0/1
 link-protocol ppp
#
interface Serial0/0/2
 link-protocol ppp
#
interface Serial0/0/3
 link-protocol ppp
#
interface GigabitEthernet0/0/0
 ip address 192.168.200.1 255.255.255.0 
#
interface GigabitEthernet0/0/1
#
interface GigabitEthernet0/0/2
#
interface GigabitEthernet0/0/3
#
wlan
#
interface NULL0
#
interface LoopBack0
 ip address 10.0.3.3 255.255.255.255 
#
interface LoopBack1
 ip address 192.168.2.3 255.255.255.0 
 ospf network-type broadcast
#
ospf 1 
 area 0.0.0.0 
  network 192.168.200.1 0.0.0.0 
  network 192.168.2.0 0.0.0.255 
#
user-interface con 0
user-interface vty 0 4
user-interface vty 16 20
#
return 
```

## Fichier : MPLS L3VPN with BGP - Intranet/Exported Config/CE4.cfg

```
#
sysname CE4
#
aaa 
 authentication-scheme default
 authorization-scheme default
 accounting-scheme default
 domain default 
 domain default_admin 
 local-user admin password cipher OOCM4m($F4ajUn1vMEIBNUw#
 local-user admin service-type http
#
firewall zone Local
 priority 16
#
interface Ethernet0/0/0
#
interface Ethernet0/0/1
#
interface Serial0/0/0
 link-protocol ppp
#
interface Serial0/0/1
 link-protocol ppp
#
interface Serial0/0/2
 link-protocol ppp
#
interface Serial0/0/3
 link-protocol ppp
#
interface GigabitEthernet0/0/0
 ip address 192.168.200.1 255.255.255.0 
#
interface GigabitEthernet0/0/1
#
interface GigabitEthernet0/0/2
#
interface GigabitEthernet0/0/3
#
wlan
#
interface NULL0
#
interface LoopBack0
 ip address 10.0.4.4 255.255.255.255 
#
interface LoopBack1
 ip address 192.168.2.4 255.255.255.0 
#
bgp 200
 router-id 10.0.4.4
 peer 3.3.3.3 as-number 123 
 peer 3.3.3.3 ebgp-max-hop 2 
 peer 3.3.3.3 connect-interface LoopBack0
 #
 ipv4-family unicast
  undo synchronization
  network 192.168.2.0 
  peer 3.3.3.3 enable
#
ip route-static 3.3.3.3 255.255.255.255 192.168.200.2
#
user-interface con 0
user-interface vty 0 4
user-interface vty 16 20
#
return 
```

## Fichier : MPLS L3VPN with BGP - Intranet/Exported Config/CE5.cfg

```
#
sysname CE5
#
aaa 
 authentication-scheme default
 authorization-scheme default
 accounting-scheme default
 domain default 
 domain default_admin 
 local-user admin password cipher OOCM4m($F4ajUn1vMEIBNUw#
 local-user admin service-type http
#
firewall zone Local
 priority 16
#
interface Ethernet0/0/0
#
interface Ethernet0/0/1
#
interface Serial0/0/0
 link-protocol ppp
#
interface Serial0/0/1
 link-protocol ppp
#
interface Serial0/0/2
 link-protocol ppp
#
interface Serial0/0/3
 link-protocol ppp
#
interface GigabitEthernet0/0/0
 ip address 192.168.150.5 255.255.255.0 
#
interface GigabitEthernet0/0/1
#
interface GigabitEthernet0/0/2
#
interface GigabitEthernet0/0/3
#
wlan
#
interface NULL0
#
interface LoopBack0
 ip address 10.0.5.5 255.255.255.255 
#
interface LoopBack1
 ip address 192.168.3.5 255.255.255.0 
 ospf network-type broadcast
#
ospf 1 router-id 10.0.5.5 
 area 0.0.0.0 
  network 192.168.3.5 0.0.0.0 
  network 192.168.150.5 0.0.0.0 
#
user-interface con 0
user-interface vty 0 4
user-interface vty 16 20
#
return 
```

## Fichier : MPLS L3VPN with BGP - Intranet/Exported Config/CE6.cfg

```
#
sysname CE6
#
aaa 
 authentication-scheme default
 authorization-scheme default
 accounting-scheme default
 domain default 
 domain default_admin 
 local-user admin password cipher OOCM4m($F4ajUn1vMEIBNUw#
 local-user admin service-type http
#
firewall zone Local
 priority 16
#
interface Ethernet0/0/0
#
interface Ethernet0/0/1
#
interface Serial0/0/0
 link-protocol ppp
#
interface Serial0/0/1
 link-protocol ppp
#
interface Serial0/0/2
 link-protocol ppp
#
interface Serial0/0/3
 link-protocol ppp
#
interface GigabitEthernet0/0/0
 ip address 192.168.150.5 255.255.255.0 
#
interface GigabitEthernet0/0/1
#
interface GigabitEthernet0/0/2
#
interface GigabitEthernet0/0/3
#
wlan
#
interface NULL0
#
interface LoopBack0
 ip address 10.0.6.6 255.255.255.255 
#
interface LoopBack1
 ip address 192.168.3.6 255.255.255.0 
#
bgp 300
 router-id 10.0.6.6
 peer 10.0.4.4 as-number 123 
 peer 10.0.4.4 ebgp-max-hop 2 
 peer 10.0.4.4 connect-interface LoopBack0
 #
 ipv4-family unicast
  undo synchronization
  peer 10.0.4.4 enable
#
ip route-static 10.0.4.4 255.255.255.255 192.168.150.4
#
user-interface con 0
user-interface vty 0 4
user-interface vty 16 20
#
return 
```

## Fichier : MPLS L3VPN with BGP - Intranet/Exported Config/P3.cfg

```
#
sysname P3
#
mpls lsr-id 2.2.2.2
mpls
#
mpls ldp
#
#
aaa 
 authentication-scheme default
 authorization-scheme default
 accounting-scheme default
 domain default 
 domain default_admin 
 local-user admin password cipher $9*a,nZr1~=H)H2[EInBX]2#
 local-user admin service-type http
#
firewall zone Local
 priority 16
#
interface Ethernet0/0/0
#
interface Ethernet0/0/1
#
interface Serial0/0/0
 link-protocol ppp
#
interface Serial0/0/1
 link-protocol ppp
#
interface Serial0/0/2
 link-protocol ppp
#
interface Serial0/0/3
 link-protocol ppp
#
interface GigabitEthernet0/0/0
 ip address 10.0.12.2 255.255.255.0 
 mpls
 mpls ldp
#
interface GigabitEthernet0/0/1
 ip address 10.0.23.2 255.255.255.0 
 mpls
 mpls ldp
#
interface GigabitEthernet0/0/2
 ip address 10.0.34.2 255.255.255.0 
 ospf enable 1 area 0.0.0.0
 mpls
 mpls ldp
#
interface GigabitEthernet0/0/3
#
wlan
#
interface NULL0
#
interface LoopBack0
 ip address 2.2.2.2 255.255.255.255 
#
ospf 1 router-id 2.2.2.2 
 area 0.0.0.0 
  network 2.2.2.2 0.0.0.0 
  network 10.0.12.2 0.0.0.0 
  network 10.0.23.2 0.0.0.0 
#
user-interface con 0
user-interface vty 0 4
user-interface vty 16 20
#
return 
```

## Fichier : MPLS L3VPN with BGP - Intranet/Exported Config/PE1.cfg

```
#
sysname PE1
#
ip vpn-instance VPNX
 ipv4-family
  route-distinguisher 100:1
  vpn-target 100:123 export-extcommunity
  vpn-target 100:321 100:513 import-extcommunity
#
ip vpn-instance VPNY
 ipv4-family
  route-distinguisher 200:1
  vpn-target 200:432 export-extcommunity
  vpn-target 200:234 200:513 import-extcommunity
#
mpls lsr-id 1.1.1.1
mpls
#
mpls ldp
#
#
aaa 
 authentication-scheme default
 authorization-scheme default
 accounting-scheme default
 domain default 
 domain default_admin 
 local-user admin password cipher 2Aaz:U,_7+=H)H2[EInBz#r#
 local-user admin service-type http
#
firewall zone Local
 priority 16
#
interface Ethernet0/0/0
#
interface Ethernet0/0/1
#
interface Serial0/0/0
 link-protocol ppp
#
interface Serial0/0/1
 link-protocol ppp
#
interface Serial0/0/2
 link-protocol ppp
#
interface Serial0/0/3
 link-protocol ppp
#
interface GigabitEthernet0/0/0
 ip address 10.0.12.1 255.255.255.0 
 mpls
 mpls ldp
#
interface GigabitEthernet0/0/1
 ip binding vpn-instance VPNX
 ip address 192.168.100.2 255.255.255.0 
#
interface GigabitEthernet0/0/2
 ip binding vpn-instance VPNY
 ip address 192.168.100.2 255.255.255.0 
#
interface GigabitEthernet0/0/3
#
wlan
#
interface NULL0
#
interface LoopBack0
 ip binding vpn-instance VPNY
 ip address 1.1.1.1 255.255.255.255 
#
interface LoopBack1
 ip address 1.1.1.1 255.255.255.255 
#
bgp 123
 router-id 1.1.1.1
 peer 3.3.3.3 as-number 123 
 peer 3.3.3.3 connect-interface LoopBack1
 peer 10.0.4.4 as-number 123 
 peer 10.0.4.4 connect-interface LoopBack1
 #
 ipv4-family unicast
  undo synchronization
  peer 3.3.3.3 enable
  peer 10.0.4.4 enable
 # 
 ipv4-family vpnv4
  policy vpn-target
  peer 3.3.3.3 enable
  peer 10.0.4.4 enable
 #
 ipv4-family vpn-instance VPNX 
  import-route ospf 2
 #
 ipv4-family vpn-instance VPNY 
  router-id 1.1.1.1
  import-route direct
  peer 10.0.2.2 as-number 100 
  peer 10.0.2.2 ebgp-max-hop 2 
  peer 10.0.2.2 connect-interface LoopBack0
#
ospf 1 router-id 1.1.1.1 
 area 0.0.0.0 
  network 1.1.1.1 0.0.0.0 
  network 10.0.12.1 0.0.0.0 
#
ospf 2 router-id 1.1.1.1 vpn-instance VPNX
 import-route bgp
 area 0.0.0.0 
  network 192.168.100.2 0.0.0.0 
#
ip route-static vpn-instance VPNY 10.0.2.2 255.255.255.255 192.168.100.1
#
user-interface con 0
user-interface vty 0 4
user-interface vty 16 20
#
return 
```

## Fichier : MPLS L3VPN with BGP - Intranet/Exported Config/PE2.cfg

```
#
sysname PE2
#
ip vpn-instance VPNX
 ipv4-family
  route-distinguisher 100:1
  vpn-target 100:321 export-extcommunity
  vpn-target 100:123 100:513 import-extcommunity
#
ip vpn-instance VPNY
 ipv4-family
  route-distinguisher 200:1
  vpn-target 200:234 export-extcommunity
  vpn-target 200:432 200:513 import-extcommunity
#
mpls lsr-id 3.3.3.3
mpls
#
mpls ldp
#
#
aaa 
 authentication-scheme default
 authorization-scheme default
 accounting-scheme default
 domain default 
 domain default_admin 
 local-user admin password cipher diV)I+U,,MECB7Ie7'/)<#s#
 local-user admin service-type http
#
firewall zone Local
 priority 16
#
interface Ethernet0/0/0
#
interface Ethernet0/0/1
#
interface Serial0/0/0
 link-protocol ppp
#
interface Serial0/0/1
 link-protocol ppp
#
interface Serial0/0/2
 link-protocol ppp
#
interface Serial0/0/3
 link-protocol ppp
#
interface GigabitEthernet0/0/0
 ip address 10.0.23.3 255.255.255.0 
 mpls
 mpls ldp
#
interface GigabitEthernet0/0/1
 ip binding vpn-instance VPNX
 ip address 192.168.200.2 255.255.255.0 
#
interface GigabitEthernet0/0/2
 ip binding vpn-instance VPNY
 ip address 192.168.200.2 255.255.255.0 
#
interface GigabitEthernet0/0/3
#
wlan
#
interface NULL0
#
interface LoopBack0
 ip binding vpn-instance VPNY
 ip address 3.3.3.3 255.255.255.255 
#
interface LoopBack1
 ip address 3.3.3.3 255.255.255.255 
#
bgp 123
 router-id 3.3.3.3
 peer 1.1.1.1 as-number 123 
 peer 1.1.1.1 connect-interface LoopBack1
 peer 10.0.4.4 as-number 123 
 peer 10.0.4.4 connect-interface LoopBack1
 #
 ipv4-family unicast
  undo synchronization
  peer 1.1.1.1 enable
  peer 10.0.4.4 enable
 # 
 ipv4-family vpnv4
  policy vpn-target
  peer 1.1.1.1 enable
  peer 10.0.4.4 enable
 #
 ipv4-family vpn-instance VPNX 
  import-route ospf 2
 #
 ipv4-family vpn-instance VPNY 
  router-id 3.3.3.3
  import-route direct
  peer 10.0.4.4 as-number 200 
  peer 10.0.4.4 ebgp-max-hop 2 
  peer 10.0.4.4 connect-interface LoopBack0
#
ospf 1 router-id 3.3.3.3 
 area 0.0.0.0 
  network 3.3.3.3 0.0.0.0 
  network 10.0.23.3 0.0.0.0 
#
ospf 2 router-id 3.3.3.3 vpn-instance VPNX
 import-route bgp
 area 0.0.0.0 
  network 192.168.200.2 0.0.0.0 
#
ip route-static vpn-instance VPNY 10.0.4.4 255.255.255.255 192.168.200.1
#
user-interface con 0
user-interface vty 0 4
user-interface vty 16 20
#
return 
```

## Fichier : MPLS L3VPN with BGP - Intranet/Exported Config/PE4.cfg

```
#
sysname PE4
#
ip vpn-instance VPNX
 ipv4-family
  route-distinguisher 300:1
  vpn-target 100:513 export-extcommunity
  vpn-target 100:321 100:123 import-extcommunity
#
ip vpn-instance VPNY
 ipv4-family
  route-distinguisher 200:1
  vpn-target 200:513 export-extcommunity
  vpn-target 200:234 200:432 import-extcommunity
#
mpls lsr-id 10.0.4.4
mpls
#
mpls ldp
#
#
aaa 
 authentication-scheme default
 authorization-scheme default
 accounting-scheme default
 domain default 
 domain default_admin 
 local-user admin password cipher OOCM4m($F4ajUn1vMEIBNUw#
 local-user admin service-type http
#
firewall zone Local
 priority 16
#
interface Ethernet0/0/0
#
interface Ethernet0/0/1
#
interface Serial0/0/0
 link-protocol ppp
#
interface Serial0/0/1
 link-protocol ppp
#
interface Serial0/0/2
 link-protocol ppp
#
interface Serial0/0/3
 link-protocol ppp
#
interface GigabitEthernet0/0/0
 ip address 10.0.34.4 255.255.255.0 
 ospf enable 1 area 0.0.0.0
 mpls
 mpls ldp
#
interface GigabitEthernet0/0/1
 ip binding vpn-instance VPNX
 ip address 192.168.150.4 255.255.255.0 
#
interface GigabitEthernet0/0/2
 ip binding vpn-instance VPNY
 ip address 192.168.150.4 255.255.255.0 
#
interface GigabitEthernet0/0/3
#
wlan
#
interface NULL0
#
interface LoopBack0
 ip address 10.0.4.4 255.255.255.255 
 ospf enable 1 area 0.0.0.0
#
interface LoopBack1
 ip binding vpn-instance VPNY
 ip address 10.0.4.4 255.255.255.255 
#
bgp 123
 router-id 10.0.4.4
 peer 1.1.1.1 as-number 123 
 peer 1.1.1.1 connect-interface LoopBack0
 peer 3.3.3.3 as-number 123 
 peer 3.3.3.3 connect-interface LoopBack0
 #
 ipv4-family unicast
  undo synchronization
  peer 1.1.1.1 enable
  peer 3.3.3.3 enable
 # 
 ipv4-family vpnv4
  policy vpn-target
  peer 1.1.1.1 enable
  peer 3.3.3.3 enable
 #
 ipv4-family vpn-instance VPNX 
  import-route ospf 2
 #
 ipv4-family vpn-instance VPNY 
  router-id 10.0.4.4
  import-route direct
  peer 10.0.6.6 as-number 300 
  peer 10.0.6.6 ebgp-max-hop 2 
  peer 10.0.6.6 connect-interface LoopBack1
#
ospf 1 router-id 10.0.4.4 
 area 0.0.0.0 
#
ospf 2 router-id 10.0.4.4 vpn-instance VPNX
 import-route bgp
 area 0.0.0.0 
  network 192.168.150.4 0.0.0.0 
#
ip route-static vpn-instance VPNY 10.0.6.6 255.255.255.255 192.168.150.5
#
user-interface con 0
user-interface vty 0 4
user-interface vty 16 20
#
return 
```

## Fichier : README.md

```
# eNSP_Labs
Various network configuration labs on Huawei eNSP emulator

- **Hub&Spokes MPLS InterAS L3VPN  - BGP - same AS - Option A**

![image](https://github.com/EyadNasr/eNSP_Labs/assets/62260537/291567a6-150b-45b7-ab91-7f2b99e45987)

- **Hub&Spokes MPLS InterAS L3VPN  - BGP - same AS - Option B**

![image](https://github.com/EyadNasr/eNSP_Labs/assets/62260537/cf1bc9d3-f558-4404-a79c-738032130e87)

- **Intranet MPLS L3VPN with OSPF & BGP**

![image](https://github.com/EyadNasr/eNSP_Labs/assets/62260537/53a5ecb2-f2fe-4d61-98f6-41fbd7dd8258)
```