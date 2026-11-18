For those running VLANS on their switches 
        
    Please would someone running successful VLANS on their switches be kind enough to export their config here (Without sensitive data of course) in order for me to wrap my head around VLAN filtering in bridges.
No matter what config I try, I just cannot grasp the concept.
In the past I've used bridging interfaces to VLAN interfaces but I know it's not the right way and CPU intensive.
Specifically looking for those with VLAN filtering in the the bridge.
Thanks in advance.
Section des commentaires
The thing is that not all VLAN configuration methods are suitable for all devices. This is not clear in tutorials, including the recent one by Mikrotik
What is also not clear, is that if you abstract your bridges differently than the physical chips, you can get weird routing and bottlenecks. Also, if you have multiple chips, each one should be treated like a separate switch/bridge. Mixing ports will result in CPU load, unless you bridge them with an ethernet cable and configure them accordingly as separate switches. RSTP is also not supported by some chips, forcing offloading to switch off.
You need to check the VLAN switching support page and follow the one suitable for your device and chipsets.
https://help.mikrotik.com/docs/display/ROS/Basic+VLAN+switching#BasicVLANswitching-Otherdeviceswithabuilt-inswitchchip
https://help.mikrotik.com/docs/display/ROS/Bridging+and+Switching#BridgingandSwitching-BridgeHardwareOffloading
https://help.mikrotik.com/docs/display/ROS/Switch+Chip+Features
Edit: That might be more relevant for routers, but it's worth noting for any device that uses hardware acceleration
Thanks for this, something to also consider.
are you sure about that? i think that was the case years ago, but in recent years i don't think i've ever seen a device that supports switching but can't offload with spanning-tree running.
Here's a stripped-down configuration from my CRS328 that includes only a couple of relevant interfaces:
/interface bridgeadd name=interfaceSwitch vlan-filtering=yes/interface bridge portadd bridge=interfaceSwitch interface=ether1add bridge=interfaceSwitch interface=ether2 pvid=172/interface bridge vlanadd bridge=interfaceSwitch comment=vlanData tagged=ether1 untagged=ether2 vlan-ids=172/ip addressadd address=192.0.2.1/25 interface=interfaceSwitch
I've got ether1 as a trunk carrying the default VLAN (1, native) and the data VLAN (172, tagged); ether2 is an access interface with the PVID and native VLAN set to the data VLAN (172). The switch has L3 reachability on the default VLAN via 192.0.2.1 but is only running VLAN 172 with L2 switching.
If we want L3 switching and want to apply an IP address to the data VLAN, interfaceSwitch needs to be added to the data VLAN's tagged interface list. Then we can add a VLAN interface and apply an address to it.
/interface vlanadd interface=interfaceSwitch name=interfaceVlan172 vlan-id=172/ip addressadd address=192.0.2.129/25 interface=interfaceVlan172
Clear as mud, but I hope that helps.
Edit: Couldn't get the formatting to take.
It's a start, much appreciated.
192.0.2.129 is just a placeholder here? For documentation reasons?
All bridge vlan filtering does is apply the following logic: "Is the vlan tag on this packet in my /interface/bridge/vlan list? If yes, forward. If no, drop"
The port PVID (port vlan id) setting kinda does the inverse and says "if I see an untagged packet coming in to this port, add this vlan tag"
The /interface/bridge/vlan list then allows mutliple vlan tags to ingress or egress through specified ports. Untagged ports get the tag for packets from that vlan stripped, so they pair well with a port with a matching PVID.
Once you get the hang of it, it's not too terrible. I'll be the first to admit it can be a steep learning curve though.
Fucking thank you. I work with Cisco and HP devices and the way Mikrotik does VLANs did not compute. Out of all the documentation and explanations I’ve found, yours is the simplest and clearest.
Yeah def a weird learning curve. I struggled a lot at first, but once you get the hang of it, it's as cantanko describes here.
It really took a day with my switch, a console cable (port not on bridge) and continiously nuking the config with MikroTik's wiki open (CRS328 switching/bridging/l3 section) to fully wrap my head around it. Now that I did that - it's so damn simple. You just gotta trust in the bridge lol
What makes it extra complicated is the hardware chips and limitations that have to be respected in your configuration and bridge abstractions. If you violate chip boundaries, you will get weird routing and bottlenecks
thank you!
this is so simple,it should be stickied on mikrotik wiki...you forget !one! port in config and the day is gone,searching forums and wikis...
MikroTik actually just released some pro informative videos on the subject on their own YouTube channel!
It is hard to wrap around, but let's do a walk through on how I do mine. Let's make some assumptions:
- You'll be using these as hybrid ports (PVIDs for the ports because PCs, but VLANs tagged for things like Voice.)
- We're doing a Router on a Stick configuration.
- One port is a dedicated management port for the OH SHIT reasons.
- Three VLANs: LAN, Voice, Management (we'll use IDs 10, 50, 99)
- Trunk port is ether1, and we'll pretend it is a 48port switch. The OH SHIT port is #48. Port 2 is going to have an AP, like hAP ac2 attached.
Name things as you wish, I tend to use a structured name scheme such as: <Itemtype><Desc/VLAN><Attachment> For example, the first line below, Int for Interfaces, the VLANID, and which physical interface it is attached to.
From here, you just add ports to the Interface List for the PVID, and they will add to the bridge and be assigned that PVID as untagged, then add whichever tags you need in the Bridge/VLAN section.
I like the organized thinking here. I'll apply to the rest of my life as well!
Config below.
Connected network devices: Apeiron - RB5009, Permeant - CSS610.
4 VLAN's exist on the network but currently only one gets tagged on this switch.
# 2023-07-28 14:28:27 by RouterOS 7.10
# model = CRS309-1G-8S+
/interface bridge
add admin-mac=xxxxxxxxxx auto-mac=no dhcp-snooping=yes \
ingress-filtering=no name=bridge vlan-filtering=yes
/interface ethernet
set [ find default-name=sfp-sfpplus1 ] name=sfp1-Trunk-Permeant
set [ find default-name=sfp-sfpplus2 ] name=sfp2
set [ find default-name=sfp-sfpplus3 ] name=sfp3-NUC
set [ find default-name=sfp-sfpplus4 ] name=sfp4
set [ find default-name=sfp-sfpplus5 ] name=sfp5
set [ find default-name=sfp-sfpplus6 ] name=sfp6-NAS-TS-664
set [ find default-name=sfp-sfpplus7 ] name=sfp7
set [ find default-name=sfp-sfpplus8 ] name=sfp8-Trunk-Apeiron
/interface vlan
add interface=bridge name=Guest-VL-140 vlan-id=140
add interface=bridge name=Misc-VL-130 vlan-id=130
add interface=bridge name=Things-VL-120 vlan-id=120
add interface=bridge name=Users-VL-110 vlan-id=110
/interface lte apn
set [ find default=yes ] ip-type=ipv4 use-network-apn=no
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/port
set 0 name=serial0
/interface bridge port
add bridge=bridge comment=defconf ingress-filtering=no interface=ether1
add bridge=bridge comment=defconf ingress-filtering=no interface=\
sfp1-Trunk-Permeant trusted=yes
add bridge=bridge comment=defconf ingress-filtering=no interface=sfp2
add bridge=bridge comment=defconf ingress-filtering=no interface=sfp3-NUC \
pvid=110
add bridge=bridge comment=defconf ingress-filtering=no interface=sfp4 pvid=\
110
add bridge=bridge comment=defconf ingress-filtering=no interface=sfp5 pvid=\
110
add bridge=bridge comment=defconf ingress-filtering=no interface=\
sfp6-NAS-TS-664 pvid=110
add bridge=bridge comment=defconf ingress-filtering=no interface=sfp7 pvid=\
110
add bridge=bridge comment=defconf ingress-filtering=no interface=\
sfp8-Trunk-Apeiron trusted=yes
add bridge=bridge interface=Users-VL-110 pvid=110
add bridge=bridge interface=Things-VL-120 pvid=120
add bridge=bridge interface=Misc-VL-130 pvid=130
add bridge=bridge interface=Guest-VL-140 pvid=140
/ip settings
set max-neighbor-entries=8192
/ipv6 settings
set disable-ipv6=yes
/interface bridge vlan
add bridge=bridge tagged=bridge,sfp1-Trunk-Permeant,sfp8-Trunk-Apeiron \
untagged=Users-VL-110,sfp3-NUC,sfp4,sfp5,sfp6-NAS-TS-664,sfp7 vlan-ids=\
110
add bridge=bridge tagged=bridge,sfp8-Trunk-Apeiron,sfp1-Trunk-Permeant \
untagged=Things-VL-120 vlan-ids=120
add bridge=bridge tagged=bridge,sfp1-Trunk-Permeant,sfp8-Trunk-Apeiron \
untagged=Misc-VL-130 vlan-ids=130
add bridge=bridge tagged=bridge,sfp8-Trunk-Apeiron,sfp1-Trunk-Permeant \
untagged=Guest-VL-140 vlan-ids=140
/interface ovpn-server server
set auth=sha1,md5
/ip dhcp-client
add comment="Akasha IP" interface=bridge
/ip dns
set servers=192.168.x.x
/ip service
set telnet disabled=yes
set ftp disabled=yes
set www address=192.168.x.0/24,192.168.x.0/24,10.x.x.0/24
set ssh address=192.168.x.0/24,192.168.x.0/24,10.x.x.0/24
set api disabled=yes
set winbox address=192.168.x.0/24,192.168.x.0/24,10.x.x.0/24
set api-ssl disabled=yes
/routing bfd configuration
add disabled=no
/system clock
set time-zone-name=Europe/London
/system identity
set name=Akasha
/system leds
set 0 disabled=yes
set 1 disabled=yes
set 4 disabled=yes
set 5 disabled=yes
set 10 disabled=yes
set 11 disabled=yes
set 14 disabled=yes
set 15 disabled=yes
/system leds settings
set all-leds-off=immediate
/system note
set show-at-login=no
/system ntp client
set enabled=yes
/system ntp client servers
add address=0.pool.ntp.org
add address=2.pool.ntp.org
/system routerboard settings
set boot-os=router-os
/system swos
set static-ip-address=192.168.x.x
Thank you!
This article Using RouterOS to VLAN your network - MikroTik helped me with this. It has config examples to look through.
Great thanks
It's still a work in progress as I continue changing things but this has been working great.
Edit: /system identitiy and below is not required just part of my export. Currently IP settings are missing as I'm directly wired to the switch, but will be added when I'm finished.
Depends on the switch model.
CRS1x/2x you have to do it via the Switch menu if you want HW offloaded vlans.
CRS3x you can do it bridge>vlans.
CRS1x/2x : https://help.mikrotik.com/docs/pages/viewpage.action?pageId=103841836#CRS1xx/2xxseriesswitchesexamples-VLAN
CRS3x and most RB: https://www.youtube.com/watch?v=YI0gPoCQDmM
Also: BUY a WOOBM. This is your anti lockout if you have a USB port on your switch.
My quick summary of VLANs configuration on Mikrotik:
/interface/bridge/port defines ingress behavior. The frame-types parameter determines if frames are tagged on ingress. The PVID parameter specifies the VLAN id added.
/interface/bridge/vlan defines egress behavior. Each bridge has a single vlan table. The table contains entries (rows) for each VID. Each VID can only appear once in a table. The entry (row) for a VID will contain a list of member ports that tagged and member ports that are untagged.
The /interface/bridge/* stuff configures L2 stuff, switch-level processing.
/interface/vlan exists to add layer 3 functionality on a vlan. For example IP addressing, DHCP, and IP routing on top of VLANs.
Oh god THANK YOU! You finally made sense.
VLANs in general are not that hard. Why does Mikrotik have to make to insanely unnecesarely artificially complicated?
This of a vlan another ethernet cable going inside the ethernet cable.