WireGuard S2S VPN w/o split tunnel 
        
        
        
    
    
    Hi!
I'm trying to create my first WireGaurd based S2S VPN between RB4011 and WAP AC LTE6 Kit.
I succeeded to enable the VPN and I see that traffic between site A & B LANs is going thourgh the tunnel, but I want to force WAP AC reach Internet via the tunnel and here I fail
I prepared some drawing, where WAP AC LTE6 Kit is R2.It does ping LAN in site A it also uses local Internet to reach e.g. 8.8.8.8When I enable default route via tunnel - everything goes down (pings fail on WAP AC LTE6 Kit, WireGuard interface counters do not increase)
When route is disabled - pings to LAN on the left go via tunnel and internet connectivity via LTE interface...
Somehow I cannot get rid of default via lte1 - it's dynamic.
What is wrong with this config?
Below RB4011 dump
      [admin@RB4011.home] > export
    
      # jul/07/2023 14:33:41 by RouterOS 7.4.1
    
      # software id = MQZK-Y2R6
    
      #
    
      # model = RB4011iGS+
    
      # serial number = ...
    
      /interface bridge
    
      add admin-mac=48:8F:5A:2D:09:91 auto-mac=no comment=defconf name=bridge
    
      /interface wireguard
    
      add listen-port=13231 mtu=1420 name=WG_RA_VPN
    
      add listen-port=13232 mtu=1420 name=WG_S2S_VPN
    
      /interface vlan
    
      add interface=bridge name=vlan10_Outside vlan-id=10
    
      add interface=bridge name=vlan11_Inside vlan-id=11
    
      add interface=bridge name=vlan12_Untrusted vlan-id=12
    
      add interface=ether1 mtu=1492 name=vlan82 vlan-id=82
    
      /interface pppoe-client
    
      add add-default-route=yes disabled=no interface=vlan82 name=pppoe-out1 service-name=[ISP] user=[username]
    
      /interface list
    
      add comment=defconf name=WAN
    
      add comment=defconf name=LAN
    
      /interface lte apn
    
      set [ find default=yes ] ip-type=ipv4 use-network-apn=no use-peer-dns=no
    
      ...
    
      /ip ipsec mode-config
    
      add name=NordVPN responder=no src-address-list=local use-responder-dns=no
    
      /ip ipsec policy group
    
      add name=NordVPN
    
      /ip ipsec profile
    
      add dh-group=modp2048 enc-algorithm=aes-192 hash-algorithm=sha256 name=NordVPN
    
      add dh-group=modp2048 enc-algorithm=aes-192 name=NordVPN_Light
    
      add dh-group=ecp256,ecp384,ecp521,modp8192,modp6144,modp4096,modp3072,modp2048 enc-algorithm=blowfish hash-algorithm=sha512 name=NordVPN_Secure
    
      /ip ipsec peer
    
      add address=pl134.nordvpn.com exchange-mode=ike2 name=NordVPN profile=NordVPN_Light
    
      add address=pl152.nordvpn.com exchange-mode=ike2 name=NordVPN_LowLoad profile=NordVPN_Light
    
      add address=pl211.nordvpn.com exchange-mode=ike2 name=NordVPN_Close profile=NordVPN_Light
    
      /ip ipsec proposal
    
      add auth-algorithms=sha256 enc-algorithms=aes-128-cbc name=NordVPN pfs-group=none
    
      add enc-algorithms=aes-128-cbc name=NordVPN_Light pfs-group=none
    
      add auth-algorithms=sha512,sha256,sha1 enc-algorithms=aes-256-cbc,aes-256-ctr,aes-256-gcm,camellia-256,aes-192-cbc,aes-192-ctr,aes-192-gcm,camellia-192,aes-128-cbc,aes-128-ctr,aes-128-gcm,camellia-128,blowfish,twofish name=NordVPN_Secure pfs-group=none
    
      /ip pool
    
      add name=dhcp ranges=192.168.1.21-192.168.1.254
    
      /ip dhcp-server
    
      add address-pool=dhcp interface=bridge name=defconf
    
      ...
    
      /interface bridge port
    
      add bridge=bridge comment=defconf ingress-filtering=no interface=ether2
    
      add bridge=bridge comment=defconf ingress-filtering=no interface=ether3
    
      add bridge=bridge comment=defconf ingress-filtering=no interface=ether4
    
      add bridge=bridge comment=defconf ingress-filtering=no interface=ether5
    
      add bridge=bridge comment=defconf ingress-filtering=no interface=ether6
    
      add bridge=bridge comment=defconf ingress-filtering=no interface=ether7
    
      add bridge=bridge comment=defconf ingress-filtering=no interface=ether8
    
      add bridge=bridge comment=defconf ingress-filtering=no interface=ether9
    
      add bridge=bridge comment=defconf ingress-filtering=no interface=ether10
    
      add bridge=bridge comment=defconf ingress-filtering=no interface=sfp-sfpplus1
    
      /ip neighbor discovery-settings
    
      set discover-interface-list=LAN
    
      ...
    
      /interface bridge vlan
    
      add bridge=bridge tagged=ether2 vlan-ids=10,11,12
    
      /interface list member
    
      add comment=defconf interface=bridge list=LAN
    
      add comment=defconf interface=ether1 list=WAN
    
      add interface=pppoe-out1 list=WAN
    
      /interface ovpn-server server
    
      set auth=sha1,md5
    
      /interface wireguard peers
    
      add allowed-address=192.168.13.2/32 interface=WG_RA_VPN public-key=...
    
      add allowed-address=0.0.0.0/0 endpoint-address=10.120.83.102 endpoint-port=13232 interface=WG_S2S_VPN public-key=...
    
      /ip address
    
      add address=192.168.1.1/24 comment=defconf interface=ether2 network=192.168.1.0
    
      add address=192.168.10.1/24 comment=defconf interface=vlan10_Outside network=192.168.10.0
    
      add address=192.168.11.1/24 comment=defconf interface=vlan11_Inside network=192.168.11.0
    
      add address=192.168.12.1/24 comment=defconf interface=vlan12_Untrusted network=192.168.12.0
    
      add address=192.168.13.1/24 interface=WG_RA_VPN network=192.168.13.0
    
      add address=192.168.14.1/24 interface=WG_S2S_VPN network=192.168.14.0
    
      /ip dhcp-client
    
      add comment=defconf disabled=yes interface=ether1 use-peer-dns=no
    
      /ip dhcp-server lease
    
      add address=192.168.1.20 mac-address=...
    
      add address=192.168.1.22 mac-address=...
    
      add address=192.168.1.29 mac-address=... server=defconf
    
      /ip dhcp-server network
    
      add address=192.168.1.0/24 comment=defconf dns-server=192.168.1.1 domain=home gateway=192.168.1.1 netmask=24 ntp-server=40.119.148.38
    
      add address=192.168.10.0/24 dns-server=192.168.10.1 domain=home gateway=192.168.10.1 netmask=24 ntp-server=40.119.148.38
    
      add address=192.168.11.0/24 dns-server=192.168.11.1 domain=home gateway=192.168.11.1 netmask=24 ntp-server=40.119.148.38
    
      add address=192.168.12.0/24 dns-server=192.168.12.1 domain=home gateway=192.168.12.1 netmask=24 ntp-server=40.119.148.38
    
      /ip dns
    
      set allow-remote-requests=yes servers=8.8.8.8
    
      ...
    
      /ip firewall address-list
    
      add address=192.168.1.0/24 list=local
    
      /ip firewall filter
    
      add action=accept chain=input comment="WireGuard RA VPN SRV" dst-port=13231 protocol=udp
    
      add action=accept chain=input comment="WireGuard S2S VPN" dst-port=13232 protocol=udp
    
      ...
    
      add action=accept chain=input comment="defconf: accept established,related,untracked" connection-state=established,related,untracked
    
      add action=drop chain=input comment="defconf: drop invalid" connection-state=invalid
    
      add action=accept chain=input comment="defconf: accept ICMP" protocol=icmp
    
      add action=accept chain=forward protocol=icmp
    
      add action=accept chain=input comment="defconf: accept to local loopback (for CAPsMAN)" dst-address=127.0.0.1
    
      add action=drop chain=input comment="defconf: drop all not coming from LAN" in-interface-list=!LAN
    
      add action=accept chain=forward comment="defconf: accept in ipsec policy" ipsec-policy=in,ipsec
    
      add action=accept chain=forward comment="defconf: accept out ipsec policy" ipsec-policy=out,ipsec
    
      add action=fasttrack-connection chain=forward comment="defconf: fasttrack" connection-state=established,related hw-offload=yes src-address=!192.168.1.0/24
    
      add action=accept chain=forward comment="defconf: accept established,related, untracked" connection-state=established,related,untracked
    
      add action=drop chain=forward comment="defconf: drop invalid" connection-state=invalid
    
      add action=drop chain=forward comment="defconf: drop all from WAN not DSTNATed" connection-nat-state=!dstnat connection-state=new in-interface-list=WAN
    
      /ip firewall mangle
    
      add action=change-mss chain=forward new-mss=1380 out-interface=WG_RA_VPN passthrough=yes protocol=tcp tcp-flags=syn tcp-mss=1381-65535
    
      /ip firewall nat
    
      add action=masquerade chain=srcnat comment="defconf: masquerade" ipsec-policy=out,none out-interface-list=WAN
    
      ...
    
      /ip ipsec identity
    
      add auth-method=eap certificate=NordVPN_CA disabled=yes eap-methods=eap-mschapv2 generate-policy=port-strict mode-config=NordVPN peer=NordVPN policy-template-group=NordVPN username=...
    
      add auth-method=eap certificate=NordVPN_CA disabled=yes eap-methods=eap-mschapv2 generate-policy=port-strict mode-config=NordVPN peer=NordVPN_Close policy-template-group=NordVPN username=...
    
      add auth-method=eap certificate=NordVPN_CA disabled=yes eap-methods=eap-mschapv2 generate-policy=port-strict mode-config=NordVPN peer=NordVPN_LowLoad policy-template-group=NordVPN username=...
    
      /ip ipsec policy
    
      add dst-address=0.0.0.0/0 group=NordVPN proposal=NordVPN_Light src-address=0.0.0.0/0 template=yes
    
      /ip route
    
      add disabled=no dst-address=192.168.21.0/24 gateway=192.168.14.2 routing-table=main suppress-hw-offload=no
    
      ...
    
      /system identity
    
      set name=RB4011.home
    
      /system resource irq rps
    
      set sfp-sfpplus1 disabled=no
    
      ...
    
      /tool graphing interface
    
      add interface=pppoe-out1
    
      /tool mac-server
    
      set allowed-interface-list=LAN
    
      /tool mac-server mac-winbox
    
      set allowed-interface-list=LAN
    
And WAP AC LTE6
      [admin@WAP_AC_LTE6] > export
    
      # 2023-07-07 14:39:19 by RouterOS 7.10.1
    
      # software id = V6I3-6BQW
    
      #
    
      # model = RBwAPGR-5HacD2HnD
    
      # serial number = ...
    
      /interface bridge
    
      add admin-mac=DC:2C:6E:D9:5F:00 auto-mac=no comment=defconf name=bridge
    
      /interface lte
    
      set [ find default-name=lte1 ] allow-roaming=no band=1,3,7,20 network-mode=lte
    
      /interface wireguard
    
      add listen-port=13232 mtu=1420 name=WG_S2S_VPN
    
      /interface list
    
      add comment=defconf name=WAN
    
      add comment=defconf name=LAN
    
      ...
    
      /ip pool
    
      add name=default-dhcp ranges=192.168.21.10-192.168.21.254
    
      /ip dhcp-server
    
      add address-pool=default-dhcp interface=bridge lease-time=10m name=defconf
    
      /interface bridge port
    
      add bridge=bridge comment=defconf interface=ether1
    
      add bridge=bridge comment=defconf interface=ether2
    
      add bridge=bridge interface=wlan_5Ghz
    
      add bridge=bridge interface=wlan_2,4GHz
    
      /ip neighbor discovery-settings
    
      set discover-interface-list=LAN
    
      /interface list member
    
      add comment=defconf interface=bridge list=LAN
    
      add comment=defconf interface=lte1 list=WAN
    
      /interface wireguard peers
    
      add allowed-address=0.0.0.0/0 endpoint-address=70.18.138.1 endpoint-port=13232 interface=WG_S2S_VPN public-key=...
    
      /ip address
    
      add address=192.168.21.1/24 comment=defconf interface=bridge network=192.168.21.0
    
      add address=192.168.14.2/24 interface=WG_S2S_VPN network=192.168.14.0
    
      /ip dhcp-client
    
      # DHCP client can not run on slave or passthrough interface!
    
      add interface=wlan_2,4GHz
    
      # DHCP client can not run on slave or passthrough interface!
    
      add interface=wlan_5Ghz
    
      /ip dhcp-server network
    
      add address=192.168.21.0/24 comment=defconf dns-server=192.168.21.1 gateway=192.168.21.1
    
      /ip dns
    
      set allow-remote-requests=yes
    
      /ip dns static
    
      add address=192.168.21.1 comment=defconf name=router.lan
    
      /ip firewall filter
    
      add action=accept chain=input comment="defconf: accept established,related,untracked" connection-state=established,related,untracked
    
      add action=drop chain=input comment="defconf: drop invalid" connection-state=invalid
    
      add action=accept chain=input comment="defconf: accept ICMP" protocol=icmp
    
      add action=accept chain=input comment="defconf: accept to local loopback (for CAPsMAN)" dst-address=127.0.0.1
    
      add action=drop chain=input comment="defconf: drop all not coming from LAN" in-interface-list=!LAN
    
      add action=accept chain=forward comment="defconf: accept in ipsec policy" ipsec-policy=in,ipsec
    
      add action=accept chain=forward comment="defconf: accept out ipsec policy" ipsec-policy=out,ipsec
    
      add action=fasttrack-connection chain=forward comment="defconf: fasttrack" connection-state=established,related hw-offload=yes
    
      add action=accept chain=forward comment="defconf: accept established,related, untracked" connection-state=established,related,untracked
    
      add action=drop chain=forward comment="defconf: drop invalid" connection-state=invalid
    
      add action=drop chain=forward comment="defconf: drop all from WAN not DSTNATed" connection-nat-state=!dstnat connection-state=new in-interface-list=WAN
    
      add action=accept chain=input dst-port=13232 protocol=udp
    
      /ip firewall nat
    
      add action=masquerade chain=srcnat comment="defconf: masquerade" ipsec-policy=out,none out-interface-list=WAN
    
      /ip route
    
      add disabled=no distance=1 dst-address=192.168.10.0/24 gateway=192.168.14.1 pref-src="" routing-table=main scope=30 suppress-hw-offload=no target-scope=10
    
      add disabled=yes distance=1 dst-address=0.0.0.0/0 gateway=192.168.14.1 pref-src="" routing-table=main scope=30 suppress-hw-offload=no target-scope=10
    
      ...
    
      /system identity
    
      set name=WAP_AC_LTE6
    
      ...
    
      /tool mac-server
    
      set allowed-interface-list=LAN
    
      /tool mac-server mac-winbox
    
      set allowed-interface-list=LAN
    
Section des commentaires
You want help with wireguard, but you did not share anything about the specific thing where you need help.
Would you mind sharing:
interface/wireguard/peers/print proplist=allowed-address,interface
Sure thing!
Attached 2 files with configs stripped from non VPN stuff.
Be aware that RB4011 has also:
WireGuard RA VPN interface - that works fine
Few NordVPN configs, that currently are disabled
If I understand correctly, you connect to R1 via the R1 public IP (70.18.138.1) which would try to reach this address due it has no more specific route than the default (0.0.0.0/0) on R2.
So if you want to maintain the wireguard tunnel running and reacht the R1 public interface it would require to add
ip route add dst-address=70.18.138.1 gateway=lte1
u/willyhun, u/adamxp12, u/Whitehawk29 - thanks for your help.
After adding specific route to R1, VPN started to work quite fine, yet not perfect.
Currently most websites work, but not all of them.
For instance speedtest.net takes forever to load.
I thought this might be MTU issue, so I started to sense on both sides what is the biggest I might use.
Pings to the other side of the tunnel allowed me to use regular MTU 1420, but some webstites still took forever to load.
I lowered MTU to 1300 temporarily - this didn't solve the problem.
I think it's not BW problem as I get satisfacory speedtest results
Attached GIFs compare behaviour with and without VPN.
When VPN is on it use MTU 1300 set on both sides and 8.8.8.8 as DNS
Here Speedtest with VPN
Here Speedtest without VPN
Few websites with VPN
You have the solution in your R1 firewall config already for something else (in the mangle hook), why don't you try to apply it on R2 (of course, modified for the actual device).
mangle you say... This is something I don't get
I created a rule like this on both ends:
chain=forward action=change-mss new-mss=1420 passthrough=yes tcp-flags=syn protocol=tcp out-interface=WG_S2S_VPN tcp-mss=1421-65535
No luck
Works! It works! :)
After editing mangle rule on both ends to:
chain=forward action=change-mss new-mss=1380 passthrough=yes tcp-flags=syn protocol=tcp out-interface=WG_S2S_VPN tcp-mss=1381-65535 log=no log-prefix=""
Everything started to work correctly
Thanks for your patience! :)
Do I understand it correctly, that for VPN vonnection that MTU is 1420 I lower payload packet size to 1380 and look between 1381-65535 size with this command?
Magle is something I don't feel too much :)
As someone else mentioned. Adding a default route that goes over the VPN will break the VPN connection as the external IP your connecting with will also be routed.
If your VPN IP is static your easy solution is a static route over the LTE for just that IP. otherwise you will have to do some more complicated route filtering