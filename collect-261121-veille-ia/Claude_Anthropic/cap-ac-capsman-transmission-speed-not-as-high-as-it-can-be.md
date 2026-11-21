---
id: collect-261121-veille-ia/claude-anthropic/cap-ac-capsman-transmission-speed-not-as-high-as-it-can-be
title: "cAP AC + CAPsMAN - transmission speed not as high as it can be"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["mbps", "name", "datapath", "ghz-channel", "caps-man", "channel", "rates", "local-rates"]
source: https://forum.mikrotik.com/t/cap-ac-capsman-transmission-speed-not-as-high-as-it-can-be/162303
source_anchor: ""
source_lines: [1, 372]
section: "Claude_Anthropic"
---
Hey.

i have this question, is it possible to do something to bump up the transmission speed to the maximum possible (866Mbps)?

My Macbook Pro practically always sets the connection at 390Mbps, sometimes higher values fall in. From what I’ve noticed, there’s no difference if I take the measurement at my desk (the cAP hangs behind a wall about 1m away) or under the AP itself. As for interference, it’s clean here, I live in the countryside, so it’s not a major problem. I paste below the configuration of my CAPsMAN.

I attached a screen from registration table + MacOS WiFi config




```
# nov/16/2022 11:00:52 by RouterOS 7.6
# software id = 23YS-AQUL
#
# model = RB760iGS
# serial number = D4500DDD9071
/caps-man channel
add band=2ghz-g/n control-channel-width=20mhz extension-channel=XX frequency=\
    2412,2437,2462 name=2.4Ghz-channel
add band=5ghz-onlyac control-channel-width=20mhz extension-channel=Ceee \
    frequency=5180,5260 name=5Ghz-channel skip-dfs-channels=no
/caps-man datapath
add bridge=bridge-vlan10-mgmt name=vlan10-datapath
add bridge=bridge-vlan20-lan name=vlan20-datapath vlan-id=20 vlan-mode=\
    use-tag
add bridge=bridge-vlan30-guest name=vlan30-datapath vlan-id=30 vlan-mode=\
    use-tag
add bridge=bridge-vlan40-devices name=vlan40-datapath vlan-id=40 vlan-mode=\
    use-tag
add bridge=bridge-vlan50-iot name=vlan50-datapath vlan-id=50 vlan-mode=\
    use-tag
/caps-man rates
add basic=11Mbps,6Mbps,9Mbps,12Mbps,18Mbps,24Mbps,36Mbps,48Mbps,54Mbps \
    ht-basic-mcs="" name=local-rates supported=\
    11Mbps,6Mbps,9Mbps,12Mbps,18Mbps,24Mbps,36Mbps,48Mbps,54Mbps
/caps-man security
add authentication-types=wpa2-psk eap-radius-accounting=no encryption=aes-ccm \
    name=temp-lan-cfg
add authentication-types=wpa2-psk encryption=aes-ccm name=guest-networrk-cfg
add authentication-types=wpa2-psk encryption=aes-ccm name=devices-network-cfg
add authentication-types=wpa2-psk encryption=aes-ccm name=iot-network-cfg
/caps-man configuration
add channel=2.4Ghz-channel country=poland datapath=vlan20-datapath \
    datapath.local-forwarding=no distance=dynamic hide-ssid=no installation=\
    any keepalive-frames=enabled mode=ap multicast-helper=full name=\
    2.4Ghz-main_network rates=local-rates security=temp-lan-cfg ssid=Z93-LAN
add channel=5Ghz-channel country=poland datapath=vlan20-datapath \
    datapath.local-forwarding=no distance=dynamic hide-ssid=no installation=\
    any keepalive-frames=enabled mode=ap multicast-helper=full name=\
    5Ghz-main_network rates=local-rates security=temp-lan-cfg ssid=Z93-LAN5
add channel=2.4Ghz-channel country=poland datapath=vlan30-datapath \
    installation=any name=2.4Ghz-guest_network rates=local-rates security=\
    guest-networrk-cfg ssid=Z93-GUEST
add channel=2.4Ghz-channel country=poland datapath=vlan40-datapath \
    installation=any name=2.4Ghz-devices_network rates=local-rates security=\
    devices-network-cfg ssid=Z93-DEVICES
add channel=2.4Ghz-channel country=poland datapath=vlan50-datapath hide-ssid=\
    no installation=any name=2.4Ghz-iot_network rates=local-rates security=\
    iot-network-cfg ssid=Z93-IOT
add channel=5Ghz-channel country=poland datapath=vlan40-datapath distance=\
    dynamic installation=any keepalive-frames=enabled name=\
    5GHz-devices_network rates=local-rates security=devices-network-cfg ssid=\
    Z93-DEVICES5
/caps-man access-list
add action=accept allow-signal-out-of-range=10s disabled=no signal-range=\
    -80..120 ssid-regexp=""
add action=accept allow-signal-out-of-range=10s comment="ESP - Korytarz" \
    disabled=no mac-address=8C:AA:B5:7B:F7:7B ssid-regexp=""
add action=accept allow-signal-out-of-range=10s comment="Watomierz ZMAI" \
    disabled=no mac-address=98:F4:AB:C2:1F:9E ssid-regexp=""
add action=accept allow-signal-out-of-range=10s comment=Pralka disabled=no \
    mac-address=A4:CF:12:77:88:90 ssid-regexp=""
add action=reject allow-signal-out-of-range=10s disabled=no signal-range=\
    -120..-81 ssid-regexp=""
/caps-man manager
set enabled=yes
/caps-man provisioning
add action=create-dynamic-enabled hw-supported-modes=gn master-configuration=\
    2.4Ghz-main_network name-format=prefix-identity name-prefix=2.4Ghz- \
    slave-configurations="2.4Ghz-main_network,2.4Ghz-guest_network,2.4Ghz-devi\
    ces_network,2.4Ghz-iot_network"
add action=create-dynamic-enabled hw-supported-modes=ac master-configuration=\
    5Ghz-main_network name-format=prefix-identity name-prefix=5Ghz- \
    slave-configurations=5GHz-devices_network
```

 
            
           
          
            
            
              The radio interface rate will go UP only if needed (and will obviously be upwards limited by radio channel conditions).

The problem with your configuration is use of CAPsMAN forwarding (datapath.local-forwarding=no). It adds two bottlenecks (as compared to local-forwarding=yes): one is traffic encapsulation into tunnel between CAP and CAPsMAN which causes traffic volume overhead (full-size ethernet frames have to be fragmented, also smaller frames get additional header overhead) and the other is CPU-bound encryption/decryption of the said tunnel. Both are known to reduce wireless throughput when using CAPsMAN forwarding, the magnitude depends on both cAP and CAPsMAN performance (and hEX S is not exactly speed monster).

The reduced maximum data rate (due to explained woes) mean that radio interface doesn’t have to go above observed rates.

Alternative to CAPsMAN forwarding is local forwarding, which in turn requires proper configuration of bridge on cAP device and the trunk connection between cAP and main router. This config has to be done manually on cAP itself, CAPsMAN doesn’t touch that part of CAP devices. The benefit is that it avoids all the overhead described above and usually allow for maximum possible wireless speeds.

             
            
           
          
            
            
              I always advocate for local forwarding… it makes it that much easier to change your wireless to another vendor, once reality sets in.

But as for connection speed… that’s a negotiated thing between client and AP. And remember you are using WIFi5 V1. No MU-MIMO or any of the other enhancements of the 2016 update to the wifi spec.

             
            
           
          
            
            
              
But as for connection speed… that’s a negotiated thing between client and AP


Indeed defined between client and AP, both directions are independent.

This MCS05=520Mbps  and MCS07=650Mbps used here, with 80MHz dual stream, normally means (apart from the fact that there might be no need to rise the interface rate, or that the interface rate is young and still rising) , that the finer encoding with 256-QAM did not succeed in this connection, and the speed fell back to 64-QAM.

See MCSINDEX.COM


The mechanism is described in the MT wiki (Wifi troubleshooting). The not-acked-transmission will be retried “hw-retries” times before it is counted as a failure, what will initiate a MCS rate step-down.

A successful transmission will initiate a MCS rate step-up. (Step-up can be MCS increase or going from 1S to 2S, or from 0.8µs to SGI 0.4µs.) Doing so, will converge the interface rate around it’s usable max rate.

Wall’s, reflections and interference distort transmissions, making high QAM transmissions too blurred to be decodeable.

Experience has shown that the MCS rate step-up in MT devices is slow. With a new connection, you can easily see the slow increments in the “registration table”. (Try to understand what you see, As from time to time the basic rate is show there!) Other brands are faster in step-up, and may even allow you to select the rate adjustment algoritm. (eg Draytek).

Interface rate adjustment should not depend on CAPsMAN or not, unless the CAPsMAN data link (CAPWAN tunnel) is only slowly feeding the AP. [But the CCQ would be 100% in that case, as all transmissions succeed]



PS:see this for the Intel rate adaptation algorithm (RAA) : https://remy.grunblatt.org/pdfs/Simulation_and_Performance_Evaluation_of_the_Intel_Rate_Adaptation_Algorithm.pdf

             
            
           
          
            
            
              OK,

thanks for the comprehensive answer  I think it was the last answer that most described what I was thinking of.

However, as far as CAPsMAN is concerned - local forwarding does not work for me, the moment I turn off “Discovery Interfaces” I get the error “CAP did not find suitable CAPsMAN” over and over again.

MGMT connection is via VLAN (it is added to the interface, it gets an address from DHCP), on CAP I have a bridge done (wlan1+wlan2+ether1), CAP sees HEX, firewall on HEX allows UDP/5246,5247, unfortunately it does not work. The moment I turn on “Discovery Interfaces” - everything returns to normal (of course, then I have to add a bridge in the datapath configuration on the capsman side). Maybe continuing with the topic of speed optimization you will have some idea what I am doing wrong.

             
            
           
          
            
            
              When toggling between local- and capsman-forwarding you don’t change discovery interface … that one is necessary for provisioning. You only change setting in datapath part on capsman. Also make sure bridge, mentioned in capsman datapath setup exists on clients and is configured with necessary VLANs.

             
            
           
          
            
            
              Then maybe I misunderstood something, because reading various information (including MUMs) everywhere using the Local forwarding option (in datapath on CAPSMAN), on CAP discovery interfaces was empty, and capsman address was given.

An interesting fact is that if I leave discovery interfaces set, master interfaces still have CAPsMAN forwarding, while slave interfaces display local forwarding (regardless of how it is set in datapath). For testing at this point on both sides, I disabled vlan filtering on the bridges.

             
            
           
          
            
            
              Re. discovery: CAP device can either auto-discover CAPsMAN if it’s present in same L2 network and it uses discovery interface for that. Or it can communicate via IP to CAPsMAN in any routed network but needs CAPsMAN’s address (the usual routing rules apply).

Re. local/remote forwarding: check CAPsMAN config that all applicable datapaths have local-forwarding enabled explicitly, I don’t remember which is default setting.

I don’t think you can see actual setting on CAP under wireless. You should see all wireless interfaces as bridge ports on CAP client though - use *print* command, export won’t show that info.

             
            
           
          
            
            
              OK, in my case it can be L2 option (but I don’t understand why IP connection doesn’t work - maybe this will be next lesson for me )

And about local forwarding - that was my missclick in Configurations tab (i missed that in one configuration “Local forwarding” option was not hidden, and not checked, so it overrides my datapath config) - now all works brilliant. But VLAN Filtering is not enabled yet.

             
            
           
          
            
            
              
Make sure you enable safe mode before enabling VLAN-filtering. If things go wrong, they go really bad on L2.

             
            
           
          
            
            
              There are several options for placing access points on the network.

Schematically, I displayed them in this figure.

In option #3, the “local forwarding” mode didn’t work for me anywhere.

An example of option #3 is a building with several floors.

             
            
           
          
            
            
              You can seriously have a cap connect to caps-man across layer 3. I had the wap at my in-laws controlled by the router at my house.

I did this over direct port forwarding.

Over VPN

And over xvlan

Point being you have to make sure you allow the traffic from the caps to the caps-manager.

Once that’s done… Caps can connect and get a config with nothing more than caps-man enabled after reset (if you are in the same IP scope). If you need to hit a server elsewhere… You tell the device to go find the caps-manager.

             
            
           
          
            
            
              With local forwarding it’s vital that whole L2 infrastructure and settings on all involved devices can support the needs. In case #3 by @BrateloSlava it could happen that not all switches are configured with all necessary VLANs. CAPsMAN forwarding largely skips the proper LAN infrastructure setup problems as it tends to work if CAP can connect CAPsMAN. It also allows some more fancy stuff (like filtering traffic between wireless clients in a single point - CAPsMAN bridge) that is otherwise hard to achieve.

But as explained, performance wise it can be a hog …

And no, multiple floor physical layout doesn’t explain why local forwarding doesn’t work …

             
            
           
          
            
            
              
Yup, as I said, usual routing rules apply (and firewall as well, I thought that’s obvious).

             
            
           
          
            
            
              @**zett93** Show me, please, full text configuration of your AP. Without serial numbers and etc. With bridge and VLANs info.

And a question along the way. When you enable “local forwarding” mode for access points, do wireless clients not get IP addresses from DHCP server?

@**mkx** Most likely I do not understand or do not know something. Consider scheme #3. Imagine a simple network with no VLAN, the “primary” router is a DHCP and CAPsMAN server. The router has only one bridge configured. Switches are connected through the LAN ports of the router. Some switches can be cascaded. Access points are connected to the switch ports. All switches are also configured with a single bridge. We get one “big” L2 network.

Activate the “local forwarding” mode. Wireless clients receive IP addresses and start moving between access points. As soon as the client connects to an access point that is connected to a different switch, not the same one as the previous one, the connection is lost. This does not happen when using “capsman forwarding”.

If necessary, I will make a simple text configuration of the devices of such a network. To clarify configuration errors.

*Why am I describing such a “simple” network scheme (several switches and no VLAN) - it’s easier for me to assemble it from the equipment that I have at home.*

             
            
           
          
            
            
              
This sounds to me as trouble with switches not updating their ARP tables properly. Or some funky configuration somewhere in L2 network, can’t tell without some advanced troubleshooting. And I’ve seen this bug happen even with switches by most renowned brand. I can imagine even some “security features” on switches to cause such behaviour.

It’s not a bug of local-forwarding but in such case capsman forwarding is a (welcome) workaround. If this was a general problem with wireless clients roaming, then roaming in wireless network without CAPsMAN would not work at all … while in reality it does (and until CAPsMAN starts to speak 802.11 r/k/v it works equally well).

             
            
           
          
            
            
              @**mkx**

I assembled a “small and simple network” at home according to scheme #3. Before that, everything worked for me according to scheme #1. The access points were directly connected to the router, and the rest of the devices worked through the CRS326 switch.

hAP ac3 (172.22.99.254) as router, DHCP and CAPsMAN server.

hEX (172.22.99.253) as switch #1

cAP ac (172.22.99.252) as AP #1

RB951G (172.22.99.251) as switch #2

wAP R ac (172.22.99.250)  as AP #2


Access points are currently configured to connect to CAPsMAN via L3. The “local forwarding” mode does not work with this network scheme. Switching access points to search for the CAPsMAN server via L2 does not change anything and the “local forwarding” mode also remains inoperative.

172.22.99.253_19-11-2022_v7.rsc (2.21 KB)

172.22.99.254_19-11-2022_v7.rsc (9.64 KB)

172.22.99.250_19-11-2022_v7.rsc (4 KB)

172.22.99.251_19-11-2022_v7.rsc (2.32 KB)

172.22.99.252_19-11-2022_v7.rsc (2.98 KB)

             
            
           
          
            
            
              @BrateloSlava

1. CAPsMAN forwarding
On a CAP, the wlan interfaces are not in the bridge. Otherwise loop messages on the capsman controller and RSTP triggering disabling the ports. Since the same MAC comes from different interfaces.
2. Local forwarding
 On the CAP, the wlan interfaces are in the bridge.
 You need to look in the logs sometimes false RSTP triggering happens and you have to disable it on the bridge of the capsman manager
For experiments it is better to use provisioning add action=create-enabled instead of action=create-dynamic-enabled

PS If you want we can contact you, I will help you tweak some wifi settings.

PSS When the access points are powered from the CRS328 then it is more convenient to configure the capsman on it.

             
            
           
          
            
            
              @**Ca6ko**

As I wrote earlier, in the case when all access points are connected to one router or one switch, there are no problems.

The problem arises when there is a cascade of switches. Moreover, this chain of switches has branchings. And somewhere in these chains, access points are connected.

A wireless network client roams between points on such a network. From the point of view of the CAPsMAN server - the MAC address of the device of such a client appears in “completely unexpected” places on the network. It is possible, that I am explaining my point of view in terms that are not entirely correct, but as long as the client is near one point and talking through the messenger, there are no problems. Moved to another access point - the connection was interrupted. Rather, there is a reconnection in the messenger.

In the case when the forwarding mode via CAPsMAN is used, this situation is not observed.

That’s why today I put together the “simplest network” at home, in which there is a problem when using “local forwarding”. A router to which access points are connected via separate switches.

In this scheme, there are no difficulties, the “simplest” bridges, a single address space, no VLAN.

I gave the configuration of all devices. RTSP on bridges I turned on and off. I tried adding wireless interfaces to bridges before connecting to CAPsMAN. No difference. In the “local forwarding” mode, wireless interfaces are dynamically added with the necessary parameters to the bridges on the access points. When using CAPsMAN forwarding, these local wireless interfaces on access points are automatically disabled from local bridges.

P.S. Thank you for your willingness to help with a personal consultation. But for me it is important to understand my mistakes myself. Perhaps there is some step-by-step instruction for configuring CAPsMAN in a situation where access points are “scattered” over the network. At work, I very often have to make networks “work fine”, that others have designed and installed.

PSS. About action=create-enabled instead of action=create-dynamic-enabled. In my current configuration, access points are generally described statically, with binding to MAC addresses. Therefore, new elements in the “CAP interfaces” section are not created at all.

             
            
           
          
            
            
              @BrateloSlava: I’ve had a (quick) look at config files … nothing really hits me as wrong, the only thing which might interfere are arp settings on DHCP server. There have been some questions on this forum about what exactly “add-arp=yes” option does and it seems to have some security implications … which can kick in when a device needs to move between bridge ports. I’d set that option to default (which is “add-arp=no”) and see if it helps.
