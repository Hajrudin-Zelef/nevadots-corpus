---
id: collect-261121-veille-ia/actu-business/wireguard-site-to-site-routing-with-a-central-server
title: "Wireguard site-to-site routing with a central server"
domain: actu-business
role: reference
task: article
actors: []
dates: []
keywords: ["comment", "address", "list", "internet", "chain", "action", "icmp", "rfc6890"]
source: https://forum.mikrotik.com/t/wireguard-site-to-site-routing-with-a-central-server/170969
source_anchor: ""
source_lines: [1, 615]
section: "Actu_Business"
---
Beginner’s mistake? I’m new here and the post seemed long enough.

Here are the full scripts:



Central server:

```
/interface ethernet
set [ find default-name=ether1 ] advertise=\
    10M-half,10M-full,100M-half,100M-full,1000M-half,1000M-full
set [ find default-name=ether2 ] advertise=\
    10M-half,10M-full,100M-half,100M-full,1000M-half,1000M-full
/interface wireguard
add listen-port=13231 mtu=1420 name=wireguard1
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/routing bgp template
set default disabled=no output.network=bgp-networks
/routing ospf instance
add disabled=no name=default-v2
/routing ospf area
add disabled=yes instance=default-v2 name=backbone-v2
/snmp community
set [ find default=yes ] addresses=0.0.0.0/0
/interface wireguard peers
add allowed-address=\
    192.168.224.2/32,192.168.8.0/23,192.168.16.0/22,192.168.96.0/22 \
    interface=wireguard1 persistent-keepalive=5s \
    public-key="..."
add allowed-address=192.168.224.16/32 \
    interface=wireguard1 persistent-keepalive=5s public-key=\
    "..."
add allowed-address=192.168.224.97/32 interface=\
    wireguard1 public-key="..."
add allowed-address=192.168.224.3/32,192.168.188.0/24 \
    interface=wireguard1 persistent-keepalive=1m \
    public-key="..."
add allowed-address=192.168.224.4/32,192.168.240.0/24 \
    interface=wireguard1 persistent-keepalive=1m \
    public-key="..."
add allowed-address=192.168.224.5/32,192.168.184.0/24 \
    interface=wireguard1 persistent-keepalive=5s public-key=\
    "..."
/ip address
add address=[PUBLIC_IP]/24 interface=ether1 network=89.221.222.0
add address=192.168.224.1/24 interface=wireguard1 network=192.168.224.0
/ip dns
set servers=46.28.108.2,31.31.72.3
/ip firewall address-list
add address=0.0.0.0/8 comment=RFC6890 list=not_in_internet
add address=172.16.0.0/12 comment=RFC6890 list=not_in_internet
add address=192.168.0.0/16 comment=RFC6890 list=not_in_internet
add address=10.0.0.0/8 comment=RFC6890 list=not_in_internet
add address=169.254.0.0/16 comment=RFC6890 list=not_in_internet
add address=127.0.0.0/8 comment=RFC6890 list=not_in_internet
add address=224.0.0.0/4 comment=Multicast list=not_in_internet
add address=198.18.0.0/15 comment=RFC6890 list=not_in_internet
add address=192.0.0.0/24 comment=RFC6890 list=not_in_internet
add address=192.0.2.0/24 comment=RFC6890 list=not_in_internet
add address=198.51.100.0/24 comment=RFC6890 list=not_in_internet
add address=203.0.113.0/24 comment=RFC6890 list=not_in_internet
add address=100.64.0.0/10 comment=RFC6890 list=not_in_internet
add address=240.0.0.0/4 comment=RFC6890 list=not_in_internet
add address=192.88.99.0/24 comment=RFC3068 list=not_in_internet
/ip firewall filter
add action=accept chain=input comment="accept established,related" \
    connection-state=established,related
add action=drop chain=input comment="drop invalid" connection-state=invalid
add action=jump chain=input comment="own chain for ICMP" jump-target=icmp \
    protocol=icmp
add action=accept chain=input comment="WinBox from WireGuard" dst-port=8291 \
    in-interface=wireguard1 protocol=tcp src-address=192.168.16.0/22
add action=accept chain=input comment="Bandwith test server" dst-port=2000 \
    in-interface=wireguard1 protocol=tcp
add action=accept chain=input comment="WireGuard server" dst-port=13231 \
    protocol=udp
add action=accept chain=input comment="WireGuard traffic" src-address=\
    192.168.224.0/24
add action=drop chain=input
add action=accept chain=icmp comment="echo reply" icmp-options=0:0 protocol=\
    icmp
add action=accept chain=icmp comment="net unreachable" icmp-options=3:0 \
    protocol=icmp
add action=accept chain=icmp comment="host unreachable" icmp-options=3:1 \
    protocol=icmp
add action=accept chain=icmp comment=\
    "host unreachable fragmentation required" icmp-options=3:4 protocol=icmp
add action=accept chain=icmp comment="allow echo request" icmp-options=8:0 \
    protocol=icmp
add action=accept chain=icmp comment="allow parameter bad" icmp-options=12:0 \
    protocol=icmp
add action=accept chain=icmp comment="allow time exceed" icmp-options=11:0 \
    protocol=icmp
add action=drop chain=icmp comment="deny the rest" log=yes log-prefix="\?"
/ip route
add disabled=no dst-address=0.0.0.0/0 gateway=89.221.222.1
add disabled=no distance=1 dst-address=192.168.16.0/22 gateway=wireguard1 \
    pref-src="" routing-table=main scope=30 suppress-hw-offload=no \
    target-scope=10
add disabled=no distance=1 dst-address=192.168.188.0/24 gateway=wireguard1 \
    pref-src="" routing-table=main scope=30 suppress-hw-offload=no \
    target-scope=10
add disabled=no distance=1 dst-address=192.168.96.0/22 gateway=wireguard1 \
    pref-src="" routing-table=main scope=30 suppress-hw-offload=no \
    target-scope=10
add disabled=no distance=1 dst-address=192.168.8.1/23 gateway=wireguard1 \
    routing-table=main suppress-hw-offload=no
add disabled=no dst-address=192.168.184.0/24 gateway=wireguard1 \
    routing-table=main suppress-hw-offload=no
/ip service
set telnet disabled=yes
set ftp disabled=yes
set www disabled=yes
set ssh disabled=yes
set api disabled=yes
set api-ssl disabled=yes
/system clock
set time-zone-name=Europe/Prague
/system ntp client
set enabled=yes
/system ntp client servers
add address=time.google.com
add address=tik.cesnet.cz
add address=tak.cesnet.cz
```

Main router on Site A:

```
/interface bridge
add name=allinall-bridge vlan-filtering=yes
/interface ethernet
set [ find default-name=sfp-sfpplus1 ] disabled=yes
set [ find default-name=sfp-sfpplus2 ] disabled=yes
set [ find default-name=sfp-sfpplus3 ] disabled=yes
set [ find default-name=sfp-sfpplus4 ] disabled=yes
/interface wireguard
add listen-port=13231 mtu=1420 name=wireguard1
/interface vlan
add interface=allinall-bridge name=unfriendly-vlan vlan-id=99
add interface=allinall-bridge name=backbone-vlan vlan-id=9
add interface=allinall-bridge name=friendly-vlan vlan-id=19
add interface=allinall-bridge name=handling-vlan vlan-id=4
/caps-man configuration
add country="czech republic" datapath.bridge=allinall-bridge \
    .client-to-client-forwarding=no .local-forwarding=yes .vlan-id=99 \
    .vlan-mode=use-tag hide-ssid=no mode=ap name=unfriendly-wlan \
    security.authentication-types=wpa2-psk ssid=Wifi1
add country="czech republic" datapath.bridge=allinall-bridge \
    .client-to-client-forwarding=yes .local-forwarding=yes .vlan-id=19 \
    .vlan-mode=use-tag hide-ssid=yes name=friendly-wlan \
    security.authentication-types=wpa2-psk ssid=Wifi2
/caps-man datapath
add bridge=allinall-bridge client-to-client-forwarding=no local-forwarding=no \
    name=unfriendly-datapath vlan-id=99 vlan-mode=use-tag
/interface list
add comment="all unfriendly interfaces" name=unfriendly-interface-list
/interface lte apn
set [ find default=yes ] ip-type=ipv4 use-network-apn=no
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/ip pool
add name=unfriendly-pool ranges=192.168.98.1-192.168.99.254
add name=backbone-pool ranges=192.168.9.1-192.168.9.254
add name=friendly-pool ranges=192.168.18.1-192.168.19.254
add name=handling-pool ranges=192.168.4.1-192.168.4.13
/ip dhcp-server
add address-pool=unfriendly-pool interface=unfriendly-vlan lease-time=10m name=\
    unfriendly-dhcp
add address-pool=backbone-pool allow-dual-stack-queue=no insert-queue-before=\
    bottom interface=backbone-vlan lease-time=10m name=backbone-dhcp
add address-pool=friendly-pool interface=friendly-vlan lease-time=10m name=\
    friendly-dhcp
add address-pool=handling-pool interface=handling-vlan lease-time=10m name=\
    handling-dhcp
/port
set 0 name=serial0
/routing bgp template
set default disabled=no output.network=bgp-networks
/routing ospf instance
add disabled=no name=default-v2
/routing ospf area
add disabled=yes instance=default-v2 name=backbone-v2
/caps-man manager
set enabled=yes
/caps-man manager interface
add interface=backbone-vlan
/caps-man provisioning
add action=create-dynamic-enabled master-configuration=friendly-wlan \
    name-format=identity slave-configurations=unfriendly-wlan
/interface bridge port
add bridge=allinall-bridge interface=ether3 pvid=9
add bridge=allinall-bridge interface=ether4 pvid=9
add bridge=allinall-bridge interface=ether5 pvid=9
add bridge=allinall-bridge interface=ether6 pvid=9
add bridge=allinall-bridge interface=ether7 pvid=9
add bridge=allinall-bridge interface=ether8 pvid=9
add bridge=allinall-bridge interface=ether9 pvid=19
add bridge=allinall-bridge interface=ether10 pvid=19
add bridge=allinall-bridge interface=ether11 pvid=19
add bridge=allinall-bridge interface=ether12 pvid=19
add bridge=allinall-bridge interface=ether13 pvid=19
add bridge=allinall-bridge interface=ether14 pvid=19
add bridge=allinall-bridge interface=ether15 pvid=19
add bridge=allinall-bridge interface=ether16 pvid=19
add bridge=allinall-bridge interface=ether17 pvid=99
add bridge=allinall-bridge interface=ether18 pvid=99
add bridge=allinall-bridge interface=ether19 pvid=99
add bridge=allinall-bridge interface=ether20 pvid=99
add bridge=allinall-bridge interface=ether21 pvid=99
add bridge=allinall-bridge interface=ether22 pvid=99
add bridge=allinall-bridge interface=ether23 pvid=99
add bridge=allinall-bridge interface=ether24 pvid=99
add bridge=allinall-bridge interface=ether1 pvid=4
/ip settings
set max-neighbor-entries=8192
/ipv6 settings
set disable-ipv6=yes
/interface bridge vlan
add bridge=allinall-bridge tagged=allinall-bridge untagged=\
    ether3,ether4,ether5,ether6,ether7,ether8 vlan-ids=9
add bridge=allinall-bridge tagged=\
    allinall-bridge,ether3,ether4,ether5,ether6,ether7,ether8 untagged=\
    ether9,ether10,ether11,ether12,ether13,ether14,ether15,ether16 vlan-ids=\
    19
add bridge=allinall-bridge tagged=\
    allinall-bridge,ether3,ether4,ether5,ether6,ether7,ether8 untagged=\
    ether17,ether18,ether19,ether20,ether21,ether22,ether23,ether24 vlan-ids=\
    99
add bridge=allinall-bridge tagged=allinall-bridge untagged=ether1 vlan-ids=4
/interface ethernet switch rule
add comment="unfriendly packets must are firewalled" ports=\
    ether3,ether4,ether5,ether6,ether7,ether8 redirect-to-cpu=yes switch=\
    switch1 vlan-header=present vlan-id=99
/interface ovpn-server server
set auth=sha1,md5
/interface wireguard peers
add allowed-address=192.168.224.0/24,192.168.188.0/24,192.168.184.0/24 \
    endpoint-address=[PUBLIC_IP] endpoint-port=13231 interface=wireguard1 \
    persistent-keepalive=5s public-key="..."
/ip address
add address=192.168.96.1/22 interface=unfriendly-vlan network=192.168.96.0
add address=192.168.8.1/23 interface=backbone-vlan network=192.168.8.0
add address=192.168.16.1/22 interface=friendly-vlan network=192.168.16.0
add address=192.168.4.14/28 interface=handling-vlan network=192.168.4.0
add address=192.168.224.2/24 interface=wireguard1 network=192.168.224.0
/ip cloud
set update-time=no
/ip dhcp-client
add interface=ether2
/ip dhcp-server config
set store-leases-disk=15m
/ip dhcp-server network
add address=192.168.4.0/28 dns-server=192.168.4.14 gateway=192.168.4.14 \
    ntp-server=216.239.35.4,195.113.144.201
add address=192.168.8.0/23 dns-server=192.168.8.1 gateway=192.168.8.1 \
    ntp-server=216.239.35.4,195.113.144.201
add address=192.168.16.0/22 dns-server=192.168.16.1 gateway=192.168.16.1 \
    ntp-server=216.239.35.4,195.113.144.201
add address=192.168.96.0/22 dns-server=192.168.96.1 gateway=192.168.96.1 \
    ntp-server=216.239.35.4,195.113.144.201
/ip dns
set allow-remote-requests=yes servers=\
    192.168.4.14,192.168.8.1,192.168.16.1,192.168.96.1
/ip firewall address-list
add address=0.0.0.0/8 comment=RFC6890 list=not_in_internet
add address=172.16.0.0/12 comment=RFC6890 list=not_in_internet
add address=192.168.0.0/16 comment=RFC6890 list=not_in_internet
add address=10.0.0.0/8 comment=RFC6890 list=not_in_internet
add address=169.254.0.0/16 comment=RFC6890 list=not_in_internet
add address=127.0.0.0/8 comment=RFC6890 list=not_in_internet
add address=224.0.0.0/4 comment=Multicast list=not_in_internet
add address=198.18.0.0/15 comment=RFC6890 list=not_in_internet
add address=192.0.0.0/24 comment=RFC6890 list=not_in_internet
add address=192.0.2.0/24 comment=RFC6890 list=not_in_internet
add address=198.51.100.0/24 comment=RFC6890 list=not_in_internet
add address=203.0.113.0/24 comment=RFC6890 list=not_in_internet
add address=100.64.0.0/10 comment=RFC6890 list=not_in_internet
add address=240.0.0.0/4 comment=RFC6890 list=not_in_internet
add address=192.88.99.0/24 comment=RFC3068 list=not_in_internet
/ip firewall filter
add action=fasttrack-connection chain=forward connection-state=\
    established,related hw-offload=yes
add action=accept chain=forward comment="disable forward chain [TEMPORARY]"
add action=accept chain=forward comment="accept established,related" \
    connection-state=established,related
add action=drop chain=forward comment="drop invalid" connection-state=invalid
add action=drop chain=forward comment="incoming not NATed" \
    connection-nat-state=!dstnat connection-state=new in-interface=ether2 \
    log=yes log-prefix=!nat
add action=drop chain=forward comment="incoming without public IP" \
    in-interface=ether2 log=yes log-prefix=!public src-address-list=\
    not_in_internet
add action=jump chain=forward comment="separate chain for ICMP" jump-target=\
    icmp protocol=icmp
add action=accept chain=forward comment="everyone has internet access" \
    in-interface=!ether2 out-interface=ether2 src-address=192.168.0.0/16
add action=accept chain=forward comment="everyone uses HTTP on pokladnice" \
    dst-address=192.168.16.61 dst-port=80 out-interface=friendly-vlan \
    protocol=tcp
add action=accept chain=forward comment="allow SyncThing" dst-port=22000 \
    out-interface=friendly-vlan protocol=tcp
add action=accept chain=forward comment="friendly talk to friendly" \
    dst-address=192.168.16.0/22 in-interface=friendly-vlan out-interface=\
    friendly-vlan src-address=192.168.16.0/22
add action=accept chain=forward comment="friendly talk to unfriendly" \
    dst-address=192.168.96.0/22 in-interface=friendly-vlan out-interface=\
    unfriendly-vlan src-address=192.168.16.0/22
add action=accept chain=forward comment="manage APs" dst-address=\
    192.168.8.0/23 in-interface=handling-vlan out-interface=backbone-vlan \
    src-address=192.168.4.0/28
add action=accept chain=forward comment="wireguard to friendly" dst-address=\
    192.168.16.0/22 in-interface=wireguard1 out-interface=friendly-vlan \
    src-address=192.168.224.0/24
add action=accept chain=forward comment="wireguard to unfriendly" disabled=yes \
    dst-address=192.168.96.0/22 in-interface=wireguard1 out-interface=\
    unfriendly-vlan src-address=192.168.224.0/24
add action=accept chain=forward comment="TEMPORARY: slatina to friendly" \
    dst-address=192.168.16.0/22 in-interface=wireguard1 out-interface=\
    friendly-vlan src-address=192.168.188.0/24
add action=accept chain=forward comment="TEMPORARY: slatina to unfriendly" \
    dst-address=192.168.96.0/22 in-interface=wireguard1 out-interface=\
    unfriendly-vlan src-address=192.168.188.0/24
add action=accept chain=forward comment="friendly to wireguard" in-interface=\
    friendly-vlan out-interface=wireguard1 src-address=192.168.16.0/22
add action=drop chain=forward comment=\
    "skip log: devices not responding to DHCP" src-address=169.254.0.0/16
add action=drop chain=forward comment="deny the rest" log=yes log-prefix="\?"
add action=accept chain=input
add action=accept chain=input comment="accept established,related" \
    connection-state=established,related
add action=drop chain=input comment="drop invalid" connection-state=invalid
add action=drop chain=input comment="incoming from internet" \
    connection-state=new in-interface=ether2
add action=drop chain=input comment="incoming without public IP" \
    in-interface=ether2 log=yes log-prefix=!public src-address-list=\
    not_in_internet
add action=accept chain=input comment="wide open for management" \
    in-interface=handling-vlan log=yes log-prefix=mgmt
add action=jump chain=input comment="separate chain for ICMP" jump-target=\
    icmp protocol=icmp
add action=accept chain=input comment="DNS server" dst-port=53 protocol=udp \
    src-address=192.168.0.0/16
add action=accept chain=input comment="DHCP server incl. broadcast" dst-port=\
    67 protocol=udp
add action=accept chain=input comment="infrastructure Mikrotik" dst-port=5678 \
    in-interface=backbone-vlan protocol=udp src-address=192.168.8.0/23 \
    src-port=5678
add action=accept chain=input comment="bandwith test server" dst-port=2000 \
    protocol=tcp src-port=""
add action=accept chain=input comment="configure via WiFi [TEMPORARY]" \
    src-address=192.168.16.0/22
add action=drop chain=input comment=\
    "skip log: HIKvision discovery is not used" dst-address=255.255.255.255 \
    dst-port=7989 in-interface=unfriendly-vlan protocol=udp src-address=\
    192.168.96.96
add action=drop chain=input comment=\
    "skip log: devices not responding to DHCP" src-address=169.254.0.0/16
add action=drop chain=input comment="deny the rest" log=yes log-prefix="\?"
add action=accept chain=icmp comment="echo reply" icmp-options=0:0 protocol=\
    icmp
add action=accept chain=icmp comment="net unreachable" icmp-options=3:0 \
    protocol=icmp
add action=accept chain=icmp comment="host unreachable" icmp-options=3:1 \
    protocol=icmp
add action=accept chain=icmp comment=\
    "host unreachable fragmentation required" icmp-options=3:4 protocol=icmp
add action=accept chain=icmp comment="allow echo request" icmp-options=8:0 \
    protocol=icmp
add action=accept chain=icmp comment="allow parameter bad" icmp-options=12:0 \
    protocol=icmp
add action=accept chain=icmp comment="allow time exceed" icmp-options=11:0 \
    protocol=icmp
add action=drop chain=icmp comment="deny the rest" log=yes log-prefix="\?"
/ip firewall nat
add action=masquerade chain=srcnat out-interface=ether2
/ip route
add comment=utechov disabled=no dst-address=192.168.184.0/24 gateway=\
    wireguard1 routing-table=main suppress-hw-offload=no
add comment=slatina disabled=no dst-address=192.168.188.0/24 gateway=\
    wireguard1 routing-table=main suppress-hw-offload=no
/ip service
set telnet disabled=yes
set ftp disabled=yes
set www disabled=yes
set ssh disabled=yes
set api disabled=yes
set api-ssl disabled=yes
/routing bfd configuration
add disabled=no
/system logging
add action=email disabled=yes topics=info,firewall
add action=email topics=error
add action=email topics=critical
add action=email topics=warning
/system note
set show-at-login=no
/system ntp client
set enabled=yes
/system ntp client servers
add address=time.google.com
add address=tik.cesnet.cz
add address=tak.cesnet.cz
/system routerboard settings
set boot-os=router-os
/tool sniffer
set filter-interface=allinall-bridge filter-operator-between-entries=and \
    memory-scroll=no
```

Main router on site B:

```
# 2023-11-08 21:11:31 by RouterOS 7.11.2
# software id = **ELIDED**
#
# model = RBD25GR-5HPacQD2HPnD
# serial number = **ELIDED**
/interface bridge
add admin-mac= **ELIDED** auto-mac=no comment=defconf name=bridge
/interface lte
# SIM not inserted
set [ find default-name=lte1 ] allow-roaming=no band=""
/interface wireless
# managed by CAPsMAN
# channel: 2412/20-Ce/gn(26dBm), SSID: Wifi3, local forwarding
set [ find default-name=wlan1 ] disabled=no ssid=MikroTik
# managed by CAPsMAN
# channel: 5180/20-Ceee/ac(12dBm), SSID: Wifi4, local forwarding
set [ find default-name=wlan2 ] disabled=no ssid=MikroTik
/interface wireguard
add listen-port=13231 mtu=1420 name=wireguard1
/caps-man security
add authentication-types=wpa2-psk comment=defconf disable-pmkid=yes \
    encryption=aes-ccm group-encryption=aes-ccm name=capSec
/caps-man configuration
add channel.band=2ghz-b/g/n .control-channel-width=20mhz .extension-channel=\
    XX comment=defconf datapath.client-to-client-forwarding=yes \
    .local-forwarding=yes distance=indoors installation=indoor name=cfg-2ghz \
    security=capSec ssid=Wifi3
add channel.band=5ghz-a/n/ac .control-channel-width=20mhz .extension-channel=\
    XXXX comment=defconf datapath.client-to-client-forwarding=yes \
    .local-forwarding=yes distance=indoors installation=indoor name=\
    cfg-5ghz-ac security=capSec ssid=Wifi4
add channel.band=5ghz-a/n .control-channel-width=20mhz .extension-channel=XX \
    comment=defconf datapath.client-to-client-forwarding=yes \
    .local-forwarding=yes distance=indoors installation=indoor name=\
    cfg-5ghz-an security=capSec ssid=Wifi4
/interface list
add comment=defconf name=WAN
add comment=defconf name=LAN
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
add authentication-types=wpa2-psk comment=defconf disable-pmkid=yes mode=\
    dynamic-keys name=wpsSync supplicant-identity=MikroTik
/interface wireless
set [ find default-name=wlan3 ] band=5ghz-a/n/ac channel-width=20/40mhz-XX \
    disabled=no hide-ssid=yes mode=ap-bridge security-profile=wpsSync ssid=\
    Backhaul
/ip pool
add name=default-dhcp ranges=192.168.184.20-192.168.184.200
/ip dhcp-server
add address-pool=default-dhcp interface=bridge lease-time=1m name=defconf
/caps-man manager
set enabled=yes
/caps-man manager interface
set [ find default=yes ] forbid=yes
add comment=defconf disabled=no interface=bridge
/caps-man provisioning
add action=create-dynamic-enabled comment=defconf hw-supported-modes=gn \
    master-configuration=cfg-2ghz name-format=prefix name-prefix=2ghz
add action=create-dynamic-enabled comment=defconf hw-supported-modes=ac \
    master-configuration=cfg-5ghz-ac name-format=prefix name-prefix=5ghz-ac
add action=create-dynamic-enabled comment=defconf hw-supported-modes=an \
    master-configuration=cfg-5ghz-an name-format=prefix name-prefix=5ghz-an
/interface bridge port
add bridge=bridge ingress-filtering=no interface=wlan3
/ip neighbor discovery-settings
set discover-interface-list=LAN
/interface list member
add comment=defconf interface=bridge list=LAN
add comment=defconf interface=ether1 list=WAN
add comment=defconf interface=lte1 list=WAN
add interface=ether2 list=LAN
add interface=wireguard1 list=LAN
/interface wireguard peers
add allowed-address=192.168.224.0/24,192.168.16.0/22,192.168.188.0/24 \
    endpoint-address=[PUBLIC_IP] endpoint-port=13231 interface=wireguard1 \
    persistent-keepalive=5s public-key="..."
/interface wireless cap
# 
set bridge=bridge caps-man-addresses=127.0.0.1 enabled=yes interfaces=\
    wlan1,wlan2
/ip address
add address=192.168.184.1/24 comment=defconf interface=bridge network=\
    192.168.184.0
add address=192.168.224.5/24 interface=wireguard1 network=192.168.224.0
/ip dhcp-client
add interface=ether1
add add-default-route=no interface=ether2
/ip dhcp-server network
add address=192.168.184.0/24 comment=defconf dns-server=192.168.184.1 \
    gateway=192.168.184.1
/ip dns
set allow-remote-requests=yes
/ip dns static
add address=192.168.184.1 comment=defconf name=centrala.utechov
add address=192.168.184.2 name=garaz.utechov
add address=192.168.184.3 name=chodba.utechov
/ip firewall filter
add action=accept chain=input comment="Accept established,related,untracked" \
    connection-state=established,related,untracked
add action=drop chain=input comment="Drop invalid" connection-state=invalid
add action=accept chain=input comment="Simply allow all ICMP" protocol=icmp
add action=accept chain=input comment="Local loopback for CAPsMAN" \
    dst-address=127.0.0.1
add action=accept chain=input comment="WinBox from LAN" dst-port=8291 \
    in-interface-list=LAN protocol=tcp
add action=accept chain=input comment="Bandwith test" dst-port=2000 protocol=\
    tcp
add action=drop chain=input comment="Drop everything not listed above"
add action=fasttrack-connection chain=forward comment="defconf: fasttrack" \
    connection-state=established,related hw-offload=yes
add action=accept chain=forward comment=\
    "defconf: accept established,related, untracked" connection-state=\
    established,related,untracked
add action=drop chain=forward comment="defconf: drop invalid" \
    connection-state=invalid
add action=drop chain=forward comment=\
    "defconf: drop all from WAN not DSTNATed" connection-nat-state=!dstnat \
    connection-state=new in-interface-list=WAN
/ip firewall nat
add action=masquerade chain=srcnat comment="defconf: masquerade" \
    ipsec-policy=out,none out-interface-list=WAN
/ip route
add disabled=no distance=1 dst-address=192.168.16.0/22 \
    gateway=wireguard1 pref-src="" routing-table=main scope=30 \
    suppress-hw-offload=no target-scope=10
add disabled=no dst-address=192.168.188.0/24 gateway=\
    wireguard1 routing-table=main suppress-hw-offload=no
/ip service
set telnet disabled=yes
set ftp disabled=yes
set www disabled=yes
set ssh disabled=yes
set api disabled=yes
set api-ssl disabled=yes
/ipv6 firewall address-list
add address=::/128 comment="defconf: unspecified address" list=bad_ipv6
add address=::1/128 comment="defconf: lo" list=bad_ipv6
add address=fec0::/10 comment="defconf: site-local" list=bad_ipv6
add address=::ffff:0.0.0.0/96 comment="defconf: ipv4-mapped" list=bad_ipv6
add address=::/96 comment="defconf: ipv4 compat" list=bad_ipv6
add address=100::/64 comment="defconf: discard only " list=bad_ipv6
add address=2001:db8::/32 comment="defconf: documentation" list=bad_ipv6
add address=2001:10::/28 comment="defconf: ORCHID" list=bad_ipv6
add address=3ffe::/16 comment="defconf: 6bone" list=bad_ipv6
/ipv6 firewall filter
add action=accept chain=input comment=\
    "defconf: accept established,related,untracked" connection-state=\
    established,related,untracked
add action=drop chain=input comment="defconf: drop invalid" connection-state=\
    invalid
add action=accept chain=input comment="defconf: accept ICMPv6" protocol=\
    icmpv6
add action=accept chain=input comment="defconf: accept UDP traceroute" port=\
    33434-33534 protocol=udp
add action=accept chain=input comment=\
    "defconf: accept DHCPv6-Client prefix delegation." dst-port=546 protocol=\
    udp src-address=fe80::/10
add action=accept chain=input comment="defconf: accept IKE" dst-port=500,4500 \
    protocol=udp
add action=accept chain=input comment="defconf: accept ipsec AH" protocol=\
    ipsec-ah
add action=accept chain=input comment="defconf: accept ipsec ESP" protocol=\
    ipsec-esp
add action=accept chain=input comment=\
    "defconf: accept all that matches ipsec policy" ipsec-policy=in,ipsec
add action=drop chain=input comment=\
    "defconf: drop everything else not coming from LAN" in-interface-list=\
    !LAN
add action=accept chain=forward comment=\
    "defconf: accept established,related,untracked" connection-state=\
    established,related,untracked
add action=drop chain=forward comment="defconf: drop invalid" \
    connection-state=invalid
add action=drop chain=forward comment=\
    "defconf: drop packets with bad src ipv6" src-address-list=bad_ipv6
add action=drop chain=forward comment=\
    "defconf: drop packets with bad dst ipv6" dst-address-list=bad_ipv6
add action=drop chain=forward comment="defconf: rfc4890 drop hop-limit=1" \
    hop-limit=equal:1 protocol=icmpv6
add action=accept chain=forward comment="defconf: accept ICMPv6" protocol=\
    icmpv6
add action=accept chain=forward comment="defconf: accept HIP" protocol=139
add action=accept chain=forward comment="defconf: accept IKE" dst-port=\
    500,4500 protocol=udp
add action=accept chain=forward comment="defconf: accept ipsec AH" protocol=\
    ipsec-ah
add action=accept chain=forward comment="defconf: accept ipsec ESP" protocol=\
    ipsec-esp
add action=accept chain=forward comment=\
    "defconf: accept all that matches ipsec policy" ipsec-policy=in,ipsec
add action=drop chain=forward comment=\
    "defconf: drop everything else not coming from LAN" in-interface-list=\
    !LAN
/system clock
set time-zone-name=Europe/Prague
/system note
set show-at-login=no
/tool mac-server
set allowed-interface-list=none
```
