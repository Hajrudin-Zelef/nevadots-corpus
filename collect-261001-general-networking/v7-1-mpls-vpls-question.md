Hi everybody!

Over the last few days, I have tried to create an MPLS / VPLS tunnel between two mikrotik routers on v7.1. I tried every option I know, but was unsuccessful. (We are currently using MPLS / VPLS on v6, so I have minimal experience with it.)

Both routers have v7.1. OSPF and MPLS seem to work, loopback IPs are reachable, the VPLS tunnel is created, but no data goes through it. The MTU is also set up well everywhere, I double checked it.

The documentation seems pretty incomplete for v7.1, so if anyone has an idea to make it work, please feel free to share it with me.

             
            
           
          
            
            
              Post your config.  I have been able to successfully pass vpls traffic on my test bench with 7.1 between a few rb1100ahx4’s I use for testing.

             
            
           
          
            
            
              OSPF and MPLS configured, but VPLS is not UP

MK-06

/mpls interface

add disabled=no interface=all mpls-mtu=9000

/mpls ldp

add afi=ip,ipv6 disabled=no lsr-id=10.255.255.6 transport-addresses=10.255.255.6 vrf=main

/mpls ldp interface

add disabled=no interface=ether1 transport-addresses=“”

add disabled=no interface=ether4 transport-addresses=“”

/interface vpls

add arp=enabled bridge=vpls-cliente disabled=no mac-address=02:81:A6:BC:EF:77 

mtu=1500 name=vpls1-cliente peer=10.255.255.7 pw-control-word=default 

pw-type=vpls vpls-id=2010:1



MK-07

/mpls interface

add disabled=no interface=all mpls-mtu=9000

/mpls ldp

add afi=ip,ipv6 disabled=no lsr-id=10.255.255.7 transport-addresses=10.255.255.7 vrf=main

/mpls ldp interface

add disabled=no interface=ether3 transport-addresses=“”

add disabled=no interface=ether5 transport-addresses=“”

/interface vpls

add arp=enabled bridge=vpls-cliente disabled=no mac-address=02:75:A3:36:BC:2B mtu=1500 name=vpls1-cliente peer=10.255.255.6 

pw-control-word=default pw-type=vpls vpls-id=2010:1

             
            
           
          
            
            
              What kind of router? Physical or say HyperV?  HyperV requires an extra setting.

             
            
           
          
            
            
              
Unset these transport-addresses entirely rather than having them set to an empty string.

             
            
           
          
            
            
              
It’s already removed, I didn’t even specify, export that showed
            

 
            
           
          
            
            
              I don’t know if Eve-NG supports mac-spoofing protection, but under hyperv I had problems with that setting until I disabled it,

             
            
           
          
            
            
              
for each /mpls ldp interface add “accept-dynamic-neighbors=yes” and set the transport address to your respective loopback ips.

```
/mpls ldp interface add accept-dynamic-neighbors=yes afi=ip disabled=no hello-interval=5s hold-time=10s interface="ether1 - v1590" transport-addresses=15.0.2.254
```

is how I have mine set and working.

             
            
           
          
            
            
              
Yes it is a winbox issue. Do this from the command line:

```
/mpls ldp interface
set 0 !transport-addresses
set 1 !transport-addresses
```

That should clear the empty string.

             
            
           
          
            
            
              
These are the last configs of the two RBs used for testing. They are connected directly with ethernet.

```
# feb/09/2022 14:25:54 by RouterOS 7.2rc3
# model = 1100AHx2
/interface bridge
add admin-mac=32:3E:39:11:B6:18 auto-mac=no name=loopback protocol-mode=none
/interface ethernet
set [ find default-name=ether10 ] l2mtu=9498 name=ether10_RB2011
set [ find default-name=ether1 ] name=ether12
set [ find default-name=ether2 ] l2mtu=9000 name=ether13_mgmt
/interface vpls
add arp=enabled cisco-static-id=10 disabled=no mac-address=02:A8:6C:37:29:04 mtu=1500 name=vpls1 peer=10.50.50.101 pw-control-word=disabled pw-l2mtu=1508 pw-type=raw-ethernet
/interface lte apn
set [ find default=yes ] ip-type=ipv4
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/port
set 0 name=serial0
set 1 name=serial1
/routing ospf instance
add name=default-v2 redistribute=connected,static router-id=10.50.50.100
/routing ospf area
add instance=default-v2 name=backbone-v2
/ip neighbor discovery-settings
set discover-interface-list=all
/ip settings
set max-neighbor-entries=8192
/ipv6 settings
set max-neighbor-entries=8192
/interface ovpn-server server
set auth=sha1,md5
/ip address
add address=10.50.50.100 interface=loopback network=10.50.50.100
add address=10.254.1.21/30 interface=ether10_RB2011 network=10.254.1.20
add address=10.254.1.202/30 interface=vpls1 network=10.254.1.200
/ip dhcp-client
add disabled=yes interface=ether13_mgmt
/ip dns
set servers=8.8.8.8
/ip service
set www-ssl certificate=root-cert disabled=no
/mpls interface
add disabled=no interface=all mpls-mtu=1600
/mpls ldp
add afi=ip disabled=no lsr-id=10.50.50.100 transport-addresses=10.50.50.100 vrf=main
/mpls ldp interface
add accept-dynamic-neighbors=yes disabled=no hello-interval=5s hold-time=15s interface=ether10_RB2011
/routing ospf interface-template
add area=backbone-v2 comment=2011 cost=10 interfaces=ether10_RB2011 networks=10.254.1.20/30 type=ptp
add area=backbone-v2 cost=10 interfaces=loopback networks=10.50.50.100/32
/system clock
set time-zone-name=Europe/Budapest
/system identity
set name="Test RB1100AHx2 v7"
/system ntp client
set enabled=yes
/system ntp client servers
add address=pool.ntp.org
/system package update
set channel=testing
/system routerboard settings
set auto-upgrade=yes
/tool romon
set enabled=yes
#
#
#
#-----------------
#
#
#
# feb/09/2022 14:23:25 by RouterOS 7.2rc3
# model = 2011UiAS
/interface bridge
add name=loopback protocol-mode=none
/interface ethernet
set [ find default-name=ether1 ] l2mtu=4074 name=ether1_RB1100
/interface vpls
add arp=enabled cisco-static-id=10 disabled=no mac-address=02:17:D7:9F:72:3E mtu=1500 name=vpls1 peer=10.50.50.100 pw-control-word=disabled pw-l2mtu=1508 pw-type=raw-ethernet
/interface lte apn
set [ find default=yes ] ip-type=ipv4
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/port
set 0 name=serial0
/routing ospf instance
add name=default-v2 redistribute=connected,static router-id=10.50.50.101
/routing ospf area
add instance=default-v2 name=backbone-v2
/ip settings
set max-neighbor-entries=8192
/ipv6 settings
set disable-ipv6=yes max-neighbor-entries=8192
/interface ovpn-server server
set auth=sha1,md5
/ip address
add address=10.254.1.22/30 interface=ether1_RB1100 network=10.254.1.20
add address=10.50.50.101 interface=loopback network=10.50.50.101
/ip dns
set servers=8.8.8.8
/mpls interface
add disabled=no interface=all mpls-mtu=1600
/mpls ldp
add afi=ip disabled=no lsr-id=10.50.50.101 transport-addresses=10.50.50.101 vrf=main
/mpls ldp interface
add accept-dynamic-neighbors=yes disabled=no hello-interval=5s hold-time=15s interface=ether1_RB1100
/routing ospf interface-template
add area=backbone-v2 auth-id=1 auth-key="" cost=10 interfaces=ether1_RB1100 networks=10.254.1.20/30 priority=1 type=ptp
add area=backbone-v2 interfaces=loopback networks=10.50.50.101/32 passive
/system clock
set time-zone-name=Europe/Budapest
/system identity
set name="Test RB2011 v7"
/system ntp client
set enabled=yes
/system ntp client servers
add address=162.159.200.1
/system package update
set channel=testing
/system routerboard settings
set auto-upgrade=yes
/tool romon
set enabled=yes
```

The problem is the same: VPLS tunnel shows “R”, but they are not really working, there is no rx data, only tx.

I have already tested with three RBs, with the same result.

If anybody has a worcing config, please share with me, beacause I slowly rip my hair out  (just kidding…I’m bald  )


             
            
           
          
            
            
              Some things to consider.  Max phy MTU on the rb2011 is limited much lower than you appear to have set on the 1100.

RB2011 series 	ether1-ether5:4074; ether6-ether10:2028; sfp1:4074

RB1100AH 	ether1-ether10:9498; ether11:9500, ether12-ether13:9116

https://wiki.mikrotik.com/wiki/Manual:Maximum_Transmission_Unit_on_RouterBoards



I cant remember if this is necessary or not but set your mpls interface input=yes

/mpls interface set 0 input=yes

You also have your mpls MTU set to 1600 and your interface MTU appears to be default 1500.

I have not tested with as new of a version as you are using though, but it *should* still be working.

             
            
           
          
            
            
              
The MTUs are all ok, but that “/mpls interface set 0 input=yes” saved my day. I set it on both routers and it worked like charm. Thank you very much!

             
            
           
          
            
            
              I was missing the input=yes in my config. Adding it improved things - instead of the VPLS tunnel not doing anything and then the devices freezing, I get exactly one ping through and then the devices freeze and have to be power cycled.

             
            
           
          
            
            
              
I have the same problem with 7.1.3 - VPLS is still not working correctly. Actually, sometimes it starts working after I disable/enable the VPSL in config (I can see packets sent/receive), just then after a few seconds my router gets frozen, and the only method to restart it is to power cycle the router.

In my case, I’m trying to establish VPLS between bridges on RB3011 UiAS and hAP AC2. It used to work perfectly with RouterOS v6, just I’d like to migrate to v7 due to WireGuard.

             
            
           
          
            
            
              Unfortunately, the VPLS does not work with an MTU greater than 1500. When I set the PW L2MTU higher, it does nothing. L2MTU is not displayed at all 

             
            
           
          
            
            
              
Yes, I can confirm that issue.

```
v7.1.3 and 7.2rc4:
ping 172.16.70.1 do-not-fragment size=1501
  SEQ HOST                                     SIZE TTL TIME       STATUS                                                
    0 172.16.70.1                                                  timeout                                               
    1 172.16.70.1                                                  timeout                                               
    2 172.16.70.1                                                  timeout                                               
    sent=3 received=0 packet-loss=100% 
v6.48.6:
ping 172.16.70.1 do-not-fragment size=1501
  SEQ HOST                                     SIZE TTL TIME       STATUS                                                
    0 172.16.70.1                              1501  64 1ms594us  
    1 172.16.70.1                              1501  64 1ms601us  
    2 172.16.70.1                              1501  64 1ms634us  
    sent=3 received=3 packet-loss=0% min-rtt=1ms594us avg-rtt=1ms609us max-rtt=1ms634us
```

Mikrotik when it will be fixed?