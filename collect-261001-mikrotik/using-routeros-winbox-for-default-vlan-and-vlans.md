I’ve got a CRS328-24P-4S+RM that needs configuration. I need to use RouterOS, for the NTP client and SNMPv3 support, otherwise I would use SwitchOS. RouterOS has got me confused with the bridging/VLANS configuration. Right now I have a single VLAN (5) for guest wifi, and use the untagged vlan for everything else. That may change later, but not now. I need port 1, 9, 17, SPF+3 and SPF4+ to allow BOTH untagged and vlan 5. It should not delete the packets or assign a vlan to them. All other ports should just be the default untagged and be left that way. The WiFi access points are NOT Mikrotik and use a single ethernet port for both untagged/default and VLAN 5.

I want to do this with WinBox for configuration. Everything I can find either does not allow untagged/default packets. I also don’t want to lose any hardware acceleration or slow down the speed because of this configuration.

Please help!

             
            
           
          
            
            
              Read through this tutorial. You should get idea how to configure things properly and on CRS3xx that kind of setup is fully HW offloaded.

Just an advice: don’t use untagged "V"LAN … go for all-tagged setup and use access ports for end devices without VLAN support. For 3rd party APs use hybrid ports and tag the untagged part on the CRS port.

             
            
           
          
            
            
              Thank you for the response. However, it doesn’t cover what I asked. Like I said, I **can not** switch to all VLANs at this time. I already saw and read the forum post you linked to, but it does not show how to do this through winbox, nor does it allow untagged.

             
            
           
          
            
            
              I won’t argue about you not being able to switch to all-tagged … it is possible to (ab)use VLANs for different tasks and in those cases VLANs are entirely internal to single switch/router, for external devices connected to such switch such VLANs don’t exist.

When configuring hybrid (tagged/untagged) use, you can mostly think of untagged as yet another VLAN, but it does come with different configuring commands. And this is the main reason I recomend the all-tagged approach where one deals with untagged only in port configuration, the rest (SNMP, …) is then all done uniformly on VLANs.

And last: in Mikrotik world GUI and CLI map almost 1:1 between each other. If e.g. there’s CLI command */interface bridge port set [ find interface=ether2 ] pvid=13* then in winbox you follow interface → bridge, select tab port, select ether2 and set pvid to 13. Or another example: */interface bridge vlan add bridge=bridge1 vlan-ids=10 tagged=ether1 untagged=ether2* … in winbox follow interface → bridge , select tab vlan, add new entry where you select bridge1 as bridge name, set vlan-ids to 10 and add appropriate interfaces in tagged and untagged selections…

             
            
           
          
            
            
              You can do a hybrid port on bridge vlan filtering method but that assumes the ‘specialized’ device your talking to on that port (such as a VOIP phone) is able to get the tagged info and pass the untagged data onto the connected device such as a PC.

             
            
           
          
            
            
              I still don’t understand how to do this. Most of the switch right now needs to be untagged.

Until a number of changes are made, some outside my control, we can’t go pure vlans. If I have ether1 and ether9 that needs to pass both vlan5 and untagged, and the rest of the switch untagged.

The switch already comes with a default bridge with all ports assigned.

I figure I have to do this:

-Bridge, add new BridgeVLAN5

-Interface List, VLAN, make VLAN5 and set the interface to BridgeVLAN5

-Bridge, VLANs, add new bridge VLAN with Bridge = BridgeVLAN5, VLAN ID5, tagged=BridgeVLAN5, untagged= Default Bridge

-Bridge, Ports, ether1 and set the bridge to BridgeVLAN5

-Bridge, Ports, ether9 and set the bridge to BridgeVLAN5

Doing all that gets VLAN5 working. However untagged on these ports don’t work with this configuration. With Mikrotik, do I have to maybe manually define VLAN1 and assign everything to it?

             
            
           
          
            
            
              Here’s example of setup with hybrid ports towards other devices but tagged-only internally:

```
/interface bridge
add name=bridge vlan-filtering=yes
/interface bridge port
add bridge=bridge interface=ether1 pvid=10  #untagged gets tagged on ingress
add bridge=bridge interface=ether9 pvid=10
/interface bridge vlan
add bridge=bridge tagged=bridge untagged=ether1,ether9 vlan-ids=10 #VID gets untagged on egress, remains tagged internally
add bridge=bridge tagged=bridge,ether1,ether9 vlan-ids=5 #tagged on all member interfaces including internally
/interface vlan 
# create interfaces to be used for interaction with individual VLANs
add interface=bridge name=vlan5 vlan-id=5
add interface=bridge name=vlan10 vlan-id=10
#below is example use of VLAN interface
/ip address
add interface=vlan5 address=192.168.5.1/24
```

As you can see, everything is configured on single bridge (which acts as a smart switch). VLAN 5 is tagged on wires connected to ether1 and ether9 while untagged frames outside these ports are converted to tagged VLAN 10 inside CRS.

Not shown in the example, but the principle is same for untagged-only ports (they are only listed as untagged members of appropriate VLAN) and for tagged-only (trunk) ports (which don’t have pvid set and are always listed as tagged members of appropriate VLANs).

There are further per-port settings: *frame-types* and *ingress-filtering* which deal with VLAN-related port security.

             
            
           
          
            
            
              Thank you! So with Mikrotik, you can’t use the default VLAN when using VLANs? You have to assign packets into a VLAN on ingress and remove the VLAN id on egress? So I need to have a VLAN, like 10 in your example to do the conversions when mixing.

I went through the commands, changing it to the below, however, I’m having issues.

```
/interface bridge
  add name=DefaultBridge vlan-filtering=yes
/interface bridge port
add bridge=DefaultBridge interface=ether1 pvid=10  
      #untagged gets tagged on ingress
add bridge=DefaultBridge interface=ether9 pvid=10
      #untagged gets tagged on ingress
add bridge=DefaultBridge interface=ether23 pvid=10
/interface bridge vlan
add bridge=DefaultBridge tagged=DefaultBridge untagged=ether1,ether9,ether23 vlan-ids=10
      #VID gets untagged on egress, remains tagged internally
add bridge=DefaultBridge tagged=DefaultBridge,ether1,ether9,ether23 vlan-ids=5
      #tagged on all member interfaces including internally
/interface vlan 
add interface=DefaultBridge name=GuestWiFIvLAN5 vlan-id=5
add interface=DefaultBridge name=UntaggedToVLAN10 vlan-id=10
      # create interfaces to be used for interaction with individual VLANs
#below is example use of VLAN interface
/ip address
add interface=GuestWiFIvLAN5 address=172.16.10.200/24
```

My laptop is on another switch using the default VLAN on that port. That switch is connected to port 23 of the Mikrotik, with both untagged and VLAN5 for the non-mikrotik switch. No VLAN routing is setup. My laptops can ping 172.16.10.200 on vlan 5. I can also ping the ip of the Mikrotik switch on the default vlan. I shouldn’t be able to do this, right?

The WAP plugged into the Mikrotik on ether1 isn’t happy. Most devices are connecting, dropping and connecting constantly on the SSID assigned to the Guest WiFi (vlan5). The other SSID assigned to the default VLAN, well nothing can connect to it. However, the management utility and the management IP address that is assigned to the WAP over the default VLAN work. Moving it back to the existing switch makes everything happy.

I’m going to dig out a POE power injector just to make sure its not a power issue from Mikrotik;s POE support. I’m also going port based vlan to assign 10 and 5 to ports 5 and 10 for testing with my laptop and the WAPs. I just can’t do that today.

             
            
           
          
            
            
              I didn’t write you can’t use “default VLANs” on Mikrotik, I just wrote that for me things are easier if everything is tagged internally. And yes, to do it like that you need a VLAN dedicated for that. On the upside, using this technique you can “partition” your switch to two (or more) parts, all of them untagged externally but separated from each other … and you don’t need multiple bridges for that (remember, only single bridge can be HW offloaded).

CRS is a switch with routing capabilities … so it might route traffic between VLANs if configuration allows.

To be able to give you further assistance, post actual configuration of CRS (run */export hide-sensitive* and post results inside [__code] [/code] environment). When doing inter-vlan ping test, have your test setup (CRS and the two devices) separated from the rest of network to be sure the problem isn’t elsewhere.

             
            
           
          
            
            
              ```
# jul/21/2020 10:19:13 by RouterOS 6.47.1
# software id = 75MP-QGGQ
#
# model = CRS328-24P-4S+
# serial number = D41D0B79CC86
/interface bridge
add admin-mac=C4:AD:34:F6:87:FD auto-mac=no name=DefaultBridge vlan-filtering=\
    yes
/interface vlan
add interface=DefaultBridge name=GuestWiFiVLAN5 vlan-id=5
add interface=DefaultBridge name=UntaggedToVLAN10 vlan-id=10
/interface list
add name=WAN
add name=LAN
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/ip hotspot profile
set [ find default=yes ] html-directory=flash/hotspot
/interface bridge port
add bridge=DefaultBridge interface=ether1
add bridge=DefaultBridge interface=ether2
add bridge=DefaultBridge interface=ether3
add bridge=DefaultBridge interface=ether4
add bridge=DefaultBridge interface=ether5
add bridge=DefaultBridge interface=ether6
add bridge=DefaultBridge interface=ether7
add bridge=DefaultBridge interface=ether8
add bridge=DefaultBridge interface=ether9
add bridge=DefaultBridge interface=ether10
add bridge=DefaultBridge interface=ether11
add bridge=DefaultBridge interface=ether12
add bridge=DefaultBridge interface=ether13
add bridge=DefaultBridge interface=ether14
add bridge=DefaultBridge interface=ether15
add bridge=DefaultBridge interface=ether16
add bridge=DefaultBridge interface=ether17
add bridge=DefaultBridge interface=ether18
add bridge=DefaultBridge interface=ether19
add bridge=DefaultBridge interface=ether20
add bridge=DefaultBridge interface=ether21
add bridge=DefaultBridge interface=ether22
add bridge=DefaultBridge interface=ether23
add bridge=DefaultBridge interface=ether24
add bridge=DefaultBridge interface=sfp-sfpplus1
add bridge=DefaultBridge interface=sfp-sfpplus2
add bridge=DefaultBridge interface=sfp-sfpplus3
add bridge=DefaultBridge interface=sfp-sfpplus4
/interface bridge vlan
add bridge=DefaultBridge tagged=DefaultBridge untagged=ether1,ether9,ether23 \
    vlan-ids=10
add bridge=DefaultBridge tagged=DefaultBridge,ether1,ether9,ether23 vlan-ids=5
/interface list member
add interface=ether1 list=LAN
add interface=ether2 list=LAN
add interface=ether3 list=LAN
add interface=ether4 list=LAN
add interface=ether5 list=LAN
add interface=ether6 list=LAN
add interface=ether7 list=LAN
add interface=ether8 list=LAN
add interface=ether9 list=LAN
add interface=ether10 list=LAN
add interface=ether11 list=LAN
add interface=ether12 list=LAN
add interface=ether13 list=LAN
add interface=ether14 list=LAN
add interface=ether15 list=LAN
add interface=ether16 list=LAN
add interface=ether17 list=LAN
add interface=ether18 list=LAN
add interface=ether19 list=LAN
add interface=ether20 list=LAN
add interface=ether21 list=LAN
add interface=ether22 list=LAN
add interface=ether23 list=LAN
add interface=ether24 list=LAN
add interface=sfp-sfpplus1 list=LAN
add interface=sfp-sfpplus2 list=LAN
add interface=sfp-sfpplus3 list=LAN
add interface=sfp-sfpplus4 list=LAN
/ip address
add address=172.16.10.200 interface=GuestWiFiVLAN5 network=255.255.255.0
/ip dhcp-client
add disabled=no interface=DefaultBridge
/system clock
set time-zone-name=America/New_York
/system identity
set name=CRS328-1
/system ntp client
set enabled=yes primary-ntp=172.16.1.90 secondary-ntp=172.16.1.91
/system package update
set channel=testing
/system routerboard settings
set boot-os=router-os
/system swos
set identity=CRS328-1 static-ip-address=172.16.1.11
```

I really just want VLAN 5 and default on selected ports. Internal to switch VLANs are going to get messy, especially when we can switch to all VLANs

             
            
           
          
            
            
              Your setup is mostly fine, but probably not what you want. As it’s got IP addresses in both VLANs, it will route between the subnets if it gets opportunity. You wrote that you want to use device as switch, therefore you should set IP address only on management interface (which currently it should be either DefaultBridge or UntaggedToVLAN10 depending on which concept you’re going to go with). And DefaultBridge *interface* doesn’t have to be member of VLAN with which it doesn’t interact (where it doesn’t have IP address set).

You can take the opportunity to prepare for the all-tagged setup by pursuing the “internally tagged” setup you already started. Just configure DHCP client on UntaggedToVLAN10 interface and it should be fine.

BTW, when manually setting IP address, using CIDR netmask is mandatory, so your setup should be */ip address add address=172.16.10.200/24 interface=GuestWiFiVLAN5* (setting network is optional, in your case it’s actually wrong, it should be network address not netmask)

             
            
           
          
            
            
              So how would I do this without VLAN 10 and using just the default VLAN?

             
            
           
          
            
            
              So I change things up to this:

Ignore VLANS 6,99 and 100

```
# jul/21/2020 17:23:45 by RouterOS 6.47.1
# software id = 75MP-QGGQ
#
# model = CRS328-24P-4S+
# serial number = D41D0B79CC86
/interface bridge
add admin-mac=C4:AD:34:F6:87:FD auto-mac=no name=DefaultBridge vlan-filtering=\
    yes
/interface vlan
add interface=DefaultBridge name=EnterpriseWiFiVLAN6 vlan-id=6
add interface=DefaultBridge name=GuestWiFiVLAN5 vlan-id=5
add interface=DefaultBridge name=InternalSwitchVLAN99 vlan-id=99
add interface=DefaultBridge name=ManagementVLAN100 vlan-id=100
/interface list
add name=WAN
add name=LAN
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/ip hotspot profile
set [ find default=yes ] html-directory=flash/hotspot
/interface bridge port
add bridge=DefaultBridge interface=ether1
add bridge=DefaultBridge interface=ether2
add bridge=DefaultBridge interface=ether3
add bridge=DefaultBridge interface=ether4
add bridge=DefaultBridge interface=ether5
add bridge=DefaultBridge interface=ether6
add bridge=DefaultBridge interface=ether7
add bridge=DefaultBridge interface=ether8
add bridge=DefaultBridge interface=ether9
add bridge=DefaultBridge interface=ether10
add bridge=DefaultBridge interface=ether11
add bridge=DefaultBridge interface=ether12
add bridge=DefaultBridge interface=ether13
add bridge=DefaultBridge interface=ether14
add bridge=DefaultBridge interface=ether15
add bridge=DefaultBridge interface=ether16
add bridge=DefaultBridge interface=ether17
add bridge=DefaultBridge interface=ether18
add bridge=DefaultBridge interface=ether19
add bridge=DefaultBridge interface=ether20
add bridge=DefaultBridge interface=ether21
add bridge=DefaultBridge interface=ether22
add bridge=DefaultBridge interface=ether23
add bridge=DefaultBridge interface=ether24
add bridge=DefaultBridge interface=sfp-sfpplus1
add bridge=DefaultBridge interface=sfp-sfpplus2
add bridge=DefaultBridge interface=sfp-sfpplus3
add bridge=DefaultBridge interface=sfp-sfpplus4
/interface bridge vlan
add bridge=DefaultBridge tagged=GuestWiFiVLAN5,ether1,ether9,ether17 vlan-ids=5
add bridge=DefaultBridge tagged=EnterpriseWiFiVLAN6 vlan-ids=6
add bridge=DefaultBridge tagged=InternalSwitchVLAN100 vlan-ids=99
add bridge=DefaultBridge tagged=ManagementVLAN100 vlan-ids=100
/interface list member
add interface=ether1 list=LAN
add interface=ether2 list=LAN
add interface=ether3 list=LAN
add interface=ether4 list=LAN
add interface=ether5 list=LAN
add interface=ether6 list=LAN
add interface=ether7 list=LAN
add interface=ether8 list=LAN
add interface=ether9 list=LAN
add interface=ether10 list=LAN
add interface=ether11 list=LAN
add interface=ether12 list=LAN
add interface=ether13 list=LAN
add interface=ether14 list=LAN
add interface=ether15 list=LAN
add interface=ether16 list=LAN
add interface=ether17 list=LAN
add interface=ether18 list=LAN
add interface=ether19 list=LAN
add interface=ether20 list=LAN
add interface=ether21 list=LAN
add interface=ether22 list=LAN
add interface=ether23 list=LAN
add interface=ether24 list=LAN
add interface=sfp-sfpplus1 list=LAN
add interface=sfp-sfpplus2 list=LAN
add interface=sfp-sfpplus3 list=LAN
add interface=sfp-sfpplus4 list=LAN
/ip dhcp-client
add disabled=no interface=DefaultBridge
/system clock
set time-zone-name=America/New_York
/system identity
set name=CRS328-1
/system ntp client
set enabled=yes
/system ntp server
set manycast=no
/system routerboard settings
set boot-os=router-os
/system swos
set identity=CRS328-1 static-ip-address=172.16.1.11
```

Everything is on the default vlan, and ports 1,9 and 17 also have vlan 5 accessible. The default vlan and vlan5 work. However, the default vlan can reach vlan5. vlan5 does not appear to be able to reach the default VLAN.

             
            
           
          
            
            
              My bad, the config is right for vlan 5 and default vlan. The test router I plugged the switch into had the vlans routed together.

             
            
           
          
            
            
              While designing half breed labeled/untagged use, you can for the most part consider untagged one more VLAN, however it accompanies distinctive arranging orders. Also, this is the primary explanation I recomend the all-labeled methodology where one arrangements with untagged just in port setup, the rest SNMP, is then totally done consistently on VLANs.