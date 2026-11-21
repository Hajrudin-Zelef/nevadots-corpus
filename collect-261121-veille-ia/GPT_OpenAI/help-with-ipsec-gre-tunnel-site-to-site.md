---
id: collect-261121-veille-ia/reseau-networking/help-with-ipsec-gre-tunnel-site-to-site
title: "Help with IPsec/GRE tunnel site to site"
domain: reseau-networking
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["interface", "bridge1", "site", "ipsec", "bridge", "name", "list", "tunnels"]
source: https://forum.mikrotik.com/t/help-with-ipsec-gre-tunnel-site-to-site/263759
source_anchor: ""
source_lines: [1, 451]
section: "GPT_OpenAI"
---
Hello, all!

Some time ago I tried setting up a VPN connecting a pair of LAN. I am a beginner,

still learning. After taking some advice from this forum, I was able to set up an

IPsec-GRE connection from site to site, closely following the MikroTik documentation

on this issue 1.

Now I want to add a third site/LAN. I have tried repeating the same

configuration seen in 1 for the third router, but it does not work.

My set-up is:

```
Site A (dynamic public IP) <- IPsec/GRE -> Site B (static public IP)
and
Site C (dynamic public IP) <- IPsec/GRE -> Site B (static public IP)
```

Hopefully, traffic from site A to C will be able to flow through their

mutual connection with B.

I have been able to create two IPsec tunnels, corroborated by dynamic policies

and installed SA appearing on all three routers. It is possible to ping the

private IP of both ends of the IPsec tunnels. But I am struggling with GRE: it

only works when one of the two IPsec tunnels is disabled. So, only one GRE can be

running at a time.

Does anyone know what configuration/set up could be missing for the GRE tunnel

to work?

Once I get both independent tunnels working, I guess that I will be able to route traffic from A to C, right?

I have seen other posts achieving multiple site to site, like 2; there, it had been

proposed to create a pool for assigning IPs to IPsec tunnels. However, at the moment,

for three sites does not seem worth?

Any help or hints appreciated,

Here goes the partially obfuscated config:

### Site A (client/initiator in IPsec/GRE connection)

```
/interface bridge
add name=bridge1
/interface gre
# These are the IPs of both ends of the IPsec tunnel.
add local-address=10.200.2.2 name=gre1 remote-address=10.200.2.1
/interface list
add name=LAN
add name=WAN
/ip ipsec mode-config
add name=ike2-gre responder=no
/ip ipsec policy group
add name=ike2-gre
/ip ipsec profile
add dh-group=modp2048,modp1536,modp1024 enc-algorithm=aes-256,aes-192,aes-128 \
    hash-algorithm=sha256 name=ike2-gre
/ip ipsec peer
add address=DNS-XXX exchange-mode=ike2 name=peer-server profile=\
    ike2-gre
/ip ipsec proposal
add auth-algorithms=sha512,sha256 enc-algorithms=\
    aes-256-cbc,aes-256-ctr,aes-256-gcm,aes-192-ctr,aes-192-gcm,aes-128-gcm \
    lifetime=2h name=ike2-gre pfs-group=none
/ip pool
add name=dhcp1 ranges=192.168.7.100-192.168.7.254
/ip dhcp-server
add address-pool=dhcp1 interface=bridge1 lease-time=1h name=dhcp
/port
set 0 name=serial0
/interface bridge port
add bridge=bridge1 hw=no interface=ether2
add bridge=bridge1 hw=no interface=ether3
add bridge=bridge1 hw=no interface=ether4
add bridge=bridge1 hw=no interface=ether5
add bridge=bridge1 hw=no interface=ether6
add bridge=bridge1 hw=no interface=ether7
add bridge=bridge1 hw=no interface=ether8
add bridge=bridge1 hw=no interface=ether9
add bridge=bridge1 hw=no interface=ether10
add bridge=bridge1 hw=no interface=ether11
add bridge=bridge1 hw=no interface=ether12
add bridge=bridge1 hw=no interface=ether13
add bridge=bridge1 hw=no interface=ether14
add bridge=bridge1 hw=no interface=ether16
/ip neighbor discovery-settings
set discover-interface-list=none
/ip settings
set tcp-syncookies=yes
/interface list member
add interface=ether1 list=WAN
add interface=bridge1 list=LAN
/ip address
add address=192.168.7.1/24 interface=bridge1 network=192.168.7.0
add address=10.100.2.2/30 interface=gre1 network=10.100.2.0
/ip cloud
set ddns-enabled=yes ddns-update-interval=30m
/ip dhcp-client
add interface=ether1
/ip dhcp-server network
add address=192.168.7.0/24 dns-server=1.1.1.1,8.8.8.8 gateway=192.168.7.1
/ip dns
set allow-remote-requests=yes servers=8.8.8.8,1.1.1.1
/ip firewall address-list
add list=ddos-attacker
add list=ddos-target
/ip firewall filter
add action=accept chain=input comment=\
    "defconf: accept established,related,untracked" connection-state=\
    established,related,untracked
add action=drop chain=input comment="defconf: drop invalid" connection-state=\
    invalid
add action=accept chain=input comment="defconf: accept ICMP" protocol=icmp
add action=accept chain=input comment=\
    "defconf: accept to local loopback (for CAPsMAN)" dst-address=127.0.0.1
add action=accept chain=input comment=\
    "Accept UDP of IPsec ports" dst-port=1701,500,4500 \
    in-interface-list=WAN protocol=udp
add action=accept chain=input comment="Accept protocol IPsec" protocol=\
    ipsec-esp
add action=drop chain=input comment="defconf: drop all not coming from LAN" \
    disabled=yes in-interface-list=!LAN
add action=jump chain=forward connection-state=new in-interface-list=WAN \
    jump-target=detect-ddos
add action=accept chain=forward comment="defconf: accept incoming IPsec" \
    ipsec-policy=in,ipsec
add action=accept chain=forward comment="defconf: accept outgoing IPsec" \
    ipsec-policy=out,ipsec
add action=fasttrack-connection chain=forward comment="defconf: fasttrack" \
    connection-state=established,related hw-offload=yes
add action=accept chain=forward comment=\
    "defconf: accept established,related,untracked" connection-state=\
    established,related,untracked
add action=drop chain=forward comment="defconf: drop invalid" \
    connection-state=invalid
add action=drop chain=forward comment=\
    "defconf: drop all from WAN not DSTNATed" connection-nat-state=!dstnat \
    connection-state=new in-interface-list=WAN
add action=return chain=detect-ddos dst-limit=64,64,src-and-dst-addresses/10s
add action=add-dst-to-address-list address-list=ddos-target \
    address-list-timeout=10m chain=detect-ddos
add action=add-src-to-address-list address-list=ddos-attacker \
    address-list-timeout=10m chain=detect-ddos
/ip firewall nat
add action=masquerade chain=srcnat comment="defconf: masquerade" \
    ipsec-policy=out,none out-interface-list=WAN
/ip firewall raw
add action=drop chain=prerouting dst-address-list=ddos-target \
    src-address-list=ddos-attacker
/ip ipsec identity
add auth-method=digital-signature certificate=site_A.cert \
    generate-policy=port-strict mode-config=ike2-gre peer=peer-server \
    policy-template-group=ike2-gre remote-certificate=\
    CA.ike2.cert
/ip ipsec policy
add dst-address=10.200.2.1/32 group=ike2-gre proposal=proposal-ike2 \
    src-address=10.200.2.2/32 template=yes
/ip route
add disabled=no distance=1 dst-address=192.168.5.0/24 gateway=10.100.2.1 \
    routing-table=main scope=30 suppress-hw-offload=no target-scope=10
/system identity
set name=site-A
/system ntp client
set enabled=yes
/system ntp client servers
add address=0.....pool.ntp.org
add address=1.....pool.ntp.org
add address=3.....pool.ntp.org
add address=4.....pool.ntp.org
add address=2.....pool.ntp.org
```

### Site B (server/responder in IPsec/GRE connection)

```
/interface bridge
add name=ike2-gre-loopback-A
add name=ike2-gre-loopback-C
/interface ethernet
set [ find default-name=ether1 ] disable-running-check=no
set [ find default-name=ether2 ] disable-running-check=no
/interface gre
add local-address=10.200.2.1 name=gre-A remote-address=10.200.2.2
add local-address=10.200.1.1 name=gre-C remote-address=10.200.1.2
/interface wireguard
add listen-port=13231 mtu=1420 name=wireguard1
/interface list
add name=LAN
add name=WAN
/ip ipsec mode-config
add address=10.200.1.2 name=ike-C split-include=10.200.1.1/32 \
    system-dns=no
add address=10.200.2.2 name=ike-A split-include=10.200.2.1/32 \
    system-dns=no
/ip ipsec policy group
add name=ike2-gre
/ip ipsec profile
add dh-group=modp2048,modp1536,modp1024 enc-algorithm=aes-256,aes-192,aes-128 \
    hash-algorithm=sha256 name=ike2-gre
/ip ipsec peer
add exchange-mode=ike2 name=ike2-server passive=yes profile=ike2-gre \
    send-initial-contact=no
/ip ipsec proposal
add auth-algorithms=sha512,sha256 enc-algorithms=\
    aes-256-cbc,aes-256-ctr,aes-256-gcm,aes-192-ctr,aes-192-gcm,aes-128-gcm \
    lifetime=2h name=ike2-gre pfs-group=none
/ip neighbor discovery-settings
set discover-interface-list=none
/ip settings
set tcp-syncookies=yes
/interface list member
add interface=ether1 list=WAN
add interface=ether2 list=LAN
add interface=wireguard1 list=LAN
/ip address
add address=XXX interface=ether1 network=XXX
add address=192.168.5.1/24 interface=ether2 network=192.168.5.0
add address=192.168.105.1/24 interface=wireguard1 network=192.168.105.0
add address=10.200.1.1 comment=site-C interface=ike2-gre-loopback-C \
    network=10.200.1.1
add address=10.100.2.1/30 interface=gre-site-A network=10.100.2.0
add address=10.200.2.1 comment=site-A interface=ike2-gre-loopback-A \
    network=10.200.2.1
add address=10.100.1.1/30 interface=gre-site-C network=10.100.1.0
/ip cloud
set ddns-enabled=yes ddns-update-interval=30m
/ip dns
set allow-remote-requests=yes servers=8.8.8.8,1.1.1.1
/ip firewall address-list
add list=ddos-attacker
add list=ddos-target
/ip firewall filter
add action=accept chain=input comment=\
    "defconf: accept established,related,untracked" connection-state=\
    established,related,untracked
add action=drop chain=input comment="defconf: drop invalid" connection-state=\
    invalid
add action=accept chain=input comment="defconf: accept ICMP" protocol=icmp
add action=accept chain=input comment=\
    "defconf: accept to local loopback (for CAPsMAN)" dst-address=127.0.0.1
add action=accept chain=input comment=\
    "Accept UDP of IPsec ports" dst-port=1701,500,4500 \
    in-interface-list=WAN protocol=udp
add action=accept chain=input comment="Accept protocol IPsec" protocol=\
    ipsec-esp
add action=drop chain=input comment="defconf: drop all not coming from LAN" \
    disabled=yes in-interface-list=!LAN
add action=jump chain=forward connection-state=new in-interface-list=WAN \
    jump-target=detect-ddos
add action=accept chain=forward comment="defconf: accept incoming IPsec" \
    ipsec-policy=in,ipsec
add action=accept chain=forward comment="defconf: accept outgoing IPsec" \
    ipsec-policy=out,ipsec
add action=fasttrack-connection chain=forward comment="defconf: fasttrack" \
    connection-state=established,related hw-offload=yes
add action=accept chain=forward comment=\
    "defconf: accept established,related,untracked" connection-state=\
    established,related,untracked
add action=drop chain=forward comment="defconf: drop invalid" \
    connection-state=invalid
add action=drop chain=forward comment=\
    "defconf: drop all from WAN not DSTNATed" connection-nat-state=!dstnat \
    connection-state=new in-interface-list=WAN
add action=return chain=detect-ddos dst-limit=64,64,src-and-dst-addresses/10s
add action=add-dst-to-address-list address-list=ddos-target \
    address-list-timeout=10m chain=detect-ddos
add action=add-src-to-address-list address-list=ddos-attacker \
    address-list-timeout=10m chain=detect-ddos
/ip firewall nat
add action=masquerade chain=srcnat comment="defconf: masquerade" \
    ipsec-policy=out,none out-interface-list=WAN
/ip firewall raw
add action=drop chain=prerouting dst-address-list=ddos-target \
    src-address-list=ddos-attacker
/ip ipsec identity
add auth-method=digital-signature certificate=CA.ike2.cert comment=site-C \
    generate-policy=port-strict match-by=certificate mode-config=\
    ike-gre-site-C peer=ike2-server policy-template-group=ike2-gre \
    remote-certificate=site_C.cert
add auth-method=digital-signature certificate=CA.ike2.tacf comment=site-A \
    generate-policy=port-strict match-by=certificate mode-config=\
    ike-gre-site-A peer=ike2-server policy-template-group=ike2-gre \
    remote-certificate=site_A.cert
/ip ipsec policy
add dst-address=10.200.0.0/16 group=ike2-gre proposal=ike2-gre src-address=\
    10.200.0.0/16 template=yes
/ip route
add gateway=XXX
# Route to site C
add disabled=no distance=1 dst-address=192.168.6.0/24 gateway=10.100.1.2 \
    routing-table=main scope=30 suppress-hw-offload=no target-scope=10
# Route to site A
add disabled=no distance=1 dst-address=192.168.7.0/24 gateway=10.100.2.2 \
    routing-table=main scope=30 suppress-hw-offload=no target-scope=10
/system identity
set name=site-B
/system ntp client
set enabled=yes
/system ntp client servers
add address=0.....pool.ntp.org
add address=1.....pool.ntp.org
add address=3.....pool.ntp.org
add address=4.....pool.ntp.org
add address=2.....pool.ntp.org
```

### Site C (client/initiator in IPsec/GRE connection)

```
/interface bridge
add name=bridge1
/interface gre
# These are the IPs of both ends of the IPsec tunnel.
add local-address=10.200.1.2 name=gre1 remote-address=10.200.1.1
/interface list
add name=LAN
add name=WAN
/ip ipsec mode-config
add name=ike2-gre responder=no use-responder-dns=yes
/ip ipsec policy group
add name=ike2-gre
/ip ipsec profile
add dh-group=modp2048,modp1536,modp1024 enc-algorithm=aes-256,aes-192,aes-128 \
    hash-algorithm=sha256 name=ike2-gre
/ip ipsec peer
add address=DNS-XXX exchange-mode=ike2 name=peer-server profile=\
    ike2-gre
/ip ipsec proposal
add auth-algorithms=sha512,sha256 enc-algorithms=\
    aes-256-cbc,aes-256-ctr,aes-256-gcm,aes-192-ctr,aes-192-gcm,aes-128-gcm \
    lifetime=2h name=ike2-gre pfs-group=none
/ip pool
add name=dhcp1 ranges=192.168.6.100-192.168.6.254
/ip dhcp-server
add address-pool=dhcp1 interface=bridge1 lease-time=1h name=dhcp
/port
set 0 name=serial0
/interface bridge port
add bridge=bridge1 hw=no interface=ether2
add bridge=bridge1 hw=no interface=ether3
add bridge=bridge1 hw=no interface=ether4
add bridge=bridge1 hw=no interface=ether5
add bridge=bridge1 hw=no interface=ether6
add bridge=bridge1 hw=no interface=ether7
add bridge=bridge1 hw=no interface=ether8
add bridge=bridge1 hw=no interface=ether9
add bridge=bridge1 hw=no interface=ether10
add bridge=bridge1 hw=no interface=ether11
add bridge=bridge1 hw=no interface=ether12
add bridge=bridge1 hw=no interface=ether13
add bridge=bridge1 hw=no interface=ether14
add bridge=bridge1 hw=no interface=ether16
/ip neighbor discovery-settings
set discover-interface-list=none
/ip settings
set tcp-syncookies=yes
/interface list member
add interface=ether1 list=WAN
add interface=bridge1 list=LAN
/ip address
add address=192.168.6.1/24 interface=bridge1 network=192.168.6.0
add address=10.100.1.2/30 interface=gre-datacenter network=10.100.1.0
/ip cloud
set ddns-enabled=yes ddns-update-interval=30m
/ip dhcp-client
add interface=ether1
/ip dhcp-server network
add address=192.168.6.0/24 dns-server=1.1.1.1,8.8.8.8 gateway=192.168.6.1
/ip dns
set allow-remote-requests=yes servers=8.8.8.8,1.1.1.1
/ip firewall address-list
add list=ddos-attacker
add list=ddos-target
/ip firewall filter
add action=accept chain=input comment=\
    "defconf: accept established,related,untracked" connection-state=\
    established,related,untracked
add action=drop chain=input comment="defconf: drop invalid" connection-state=\
    invalid
add action=accept chain=input comment="defconf: accept ICMP" protocol=icmp
add action=accept chain=input comment=\
    "defconf: accept to local loopback (for CAPsMAN)" dst-address=127.0.0.1
add action=accept chain=input comment=\
    "Accept UDP of IPsec ports" dst-port=1701,500,4500 \
    in-interface-list=WAN protocol=udp
add action=accept chain=input comment="Accept protocol IPsec" protocol=\
    ipsec-esp
add action=drop chain=input comment="defconf: drop all not coming from LAN" \
    disabled=yes in-interface-list=!LAN
add action=jump chain=forward connection-state=new in-interface-list=WAN \
    jump-target=detect-ddos
add action=accept chain=forward comment="defconf: accept incoming IPsec" \
    ipsec-policy=in,ipsec
add action=accept chain=forward comment="defconf: accept outgoing IPsec" \
    ipsec-policy=out,ipsec
add action=fasttrack-connection chain=forward comment="defconf: fasttrack" \
    connection-state=established,related hw-offload=yes
add action=accept chain=forward comment=\
    "defconf: accept established,related,untracked" connection-state=\
    established,related,untracked
add action=drop chain=forward comment="defconf: drop invalid" \
    connection-state=invalid
add action=drop chain=forward comment=\
    "defconf: drop all from WAN not DSTNATed" connection-nat-state=!dstnat \
    connection-state=new in-interface-list=WAN
add action=return chain=detect-ddos dst-limit=64,64,src-and-dst-addresses/10s
add action=add-dst-to-address-list address-list=ddos-target \
    address-list-timeout=10m chain=detect-ddos
add action=add-src-to-address-list address-list=ddos-attacker \
    address-list-timeout=10m chain=detect-ddos
/ip firewall nat
add action=masquerade chain=srcnat comment="defconf: masquerade" \
    ipsec-policy=out,none out-interface-list=WAN
/ip firewall raw
add action=drop chain=prerouting dst-address-list=ddos-target \
    src-address-list=ddos-attacker
/ip ipsec identity
add auth-method=digital-signature certificate=site_C.cert \
    generate-policy=port-strict mode-config=ike2-gre peer=peer-server \
    policy-template-group=ike2-gre remote-certificate=\
    CA.ike2.cert
/ip ipsec policy
add dst-address=10.200.1.1/32 group=ike2-gre proposal=ike2-gre src-address=\
    10.200.1.2/32 template=yes
/ip route
add disabled=no distance=1 dst-address=192.168.5.0/24 gateway=10.100.1.1 \
    routing-table=main scope=30 suppress-hw-offload=no target-scope=10
/system identity
set name=site-C
/system ntp client
set enabled=yes
/system ntp client servers
add address=0.....pool.ntp.org
add address=1.....pool.ntp.org
add address=3.....pool.ntp.org
add address=4.....pool.ntp.org
add address=2.....pool.ntp.org
```
