---
id: collect-261121-veille-ia/copilot-microsoft/problem-with-basic-capsman-configuration
title: "Problem with basic CAPsMAN configuration"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["capsman", "device", "internet", "reach", "cap1", "dont", "cant", "wireless"]
source: https://forum.mikrotik.com/t/problem-with-basic-capsman-configuration/94821
source_anchor: ""
source_lines: [1, 292]
section: "Copilot_Microsoft"
---
Hello,

I have a issue with CAPsMAN. I will try explain it in simple terms. When I don’t use CAPsMAN, so I’m using simple two AP, I can connect to these AP and reach the internet.

But when I create CAPsMAN I can’t reach the internet with CAP1. CAP1 is nicely connected to the CAP manager. If I change some AP settings on the manager the config on the CAP1 is nicely mirrored. This is just fine. But I can connect only to CAPsMAN AP to reach internet. If I connect to CAP1 AP the device don’t receive any DHCP parameters and obviously I can’t reach internet. In addition I can’t see registration in the Wireless tables.

But when I disable CAPs mode on the AP the device is working well. In other words, I can use only the CAPsMAN AP to reach internet.

I think that the problem is very simple because I don’t have any complex config on the devices. Even though I can’t figure out why this is not working.

The CAPsMAN device is wAP and the CAP device is hAP. Both devices are configured in the same manner:

Eth1 and WLAN1 is in Bridge mode (without STP, STP = none)

RouterOS 6.33.5

Firmware 3.29



Can anybody help me with this issue or any suggestions? I am novice with the CAPsMAN but I think this is a very basic scenario.

Thank you very much for any help.

             
            
           
          
            
            
              It may help if you publish an export of your route capsman config and wireless config please.

With capsman we do not manually bridge wlan to Ethernet. Capsman automatically will do this if required.

Capsman can either local forward data or tunnel it to the controller.

It sounds to me you may not be linking the data path correctly and you create the ap but it is not being attached to the bridge ?

Check /interface wireless cap settings on both ap’s and with attention to bridge settings and also data path setting on controller.

             
            
           
          
            
            
              Hi, thank you for your replay.

I created the datapath which is pointing to bridge1. Here I post the configs of wireless interfaces and the CAPsMAN of the both AP.

I realized one important thing. Previously I didn’t enable the CAP option on the wireless interface on the CAPsMAN device. This was the reason why I was able to reach internet on the CAPsMAN device. I went to internet straight out. But now when I enabled the CAP on the both APs I can’t reach the internet from the both device. I configured the datapath but still not working. The end device (tablet, laptop) din’t get any DHCP parameters.

             
            
           
          
            
            
              To deliver your configuration to APs you shoud make at least one provision rule, i.e.:

```
/caps-man provisioning
add action=create-dynamic-enabled master-configuration=Config1 name-format=identity
```

             
            
           
          
            
            
              Thank you for your suggestion. It seems that it helped. But I still have a issue with the CAP on the CAPsMAN device. It doesn’t get the SSID information. The status is:

– managed by CAPsMAN

– channel: 2427/20-Ce/gn(30dBm), SSID: , CAPsMAN forwarding

CAP configuration is the same as in the previous post. Discovery interface Bridge1. But if the CAP is on the same device which is the CAPsMAN configured what is the discovery interface? I’ve already read about this and the suggestion is don’t use discovery interface and instead of this use CAPsMAN Addresses: 127.0.0.1 but still nothing.

CAP1 is correct. When I turn off the CAP1 device my tablet / laptop loss internet connection which is obvious because it don’t switch to other AP.

What is needed to configure on the CAPsMAN device to have functional CAP on that? Thank you much.

In log I see this, so some kind of connection is realized but the AP didn’t get SSID.

             
            
           
          
            
            
              What if You set discovery interface to Eth1?

This is my working configuration for RB2011UiAS-2HnD with CAPsMAN and CAP enabled (interface *LAN* is a bridge containing all Ethernet ports, excluding Internet):

```
/interface wireless cap
set discovery-interfaces=LAN enabled=yes interfaces=wlan1
/caps-man manager
set enabled=yes
/caps-man channel
add band=2ghz-b/g/n name=2g-b/g/n
/caps-man datapath
add bridge=LAN client-to-client-forwarding=yes name=datapath-lan
/caps-man security
add authentication-types=wpa2-psk encryption=aes-ccm,tkip group-encryption=\
aes-ccm name=wpa2-psk passphrase=YourWirelessKey
/caps-man configuration
add channel=2g-b/g/n country=latvia datapath=datapath-lan mode=ap name=\
master-cfg security=wpa2-psk ssid=YourWirelessSsid
/caps-man provisioning
add action=create-dynamic-enabled master-configuration=master-cfg\
name-format=identity
```

             
            
           
          
            
            
              I set the discovery interface to Eth1 but still nothing (even I tried every possible interface not just Eth1). I also tried your configuration so I copied the code to my routerboard. The result is the same. When I set the configuration back I issuing another problem. Neither CAP or CAPsMAN's CAP don't get SSID.

In the CAPsMAN I see Remote CAP (both APs) state run etc. But the CAP don't get the SSID.

Here is the configuration of CAPsMAN device:

**/caps-man interface**


add disabled=no l2mtu=1600 mac-address=E4:8D:8C:CE:E4:4F master-interface=none name=cap1 radio-mac=

E4:8D:8C:CE:E4:4F


add disabled=no l2mtu=1600 mac-address=E4:8D:8C:C7:DC:2B master-interface=none name=cap2 radio-mac=

E4:8D:8C:C7:DC:2B

/caps-man datapath

add bridge=bridge1 name=datapath1

/caps-man configuration

add country="czech republic" datapath=datapath1 mode=ap name=cfg ssid=abcd

/caps-man manager

set enabled=yes

/caps-man provisioning

add action=create-dynamic-enabled master-configuration=cfg name-format=identity

**/interface bridge**

add name=bridge1 protocol-mode=none

/interface bridge port

add bridge=bridge1 interface=wlan1-local

add bridge=bridge1 interface=ether1-gateway

**/interface wireless**

# managed by CAPsMAN

# channel: 2412/20-Ce/gn(30dBm), SSID: , CAPsMAN forwarding

set [ find default-name=wlan1 ] band=2ghz-b/g/n channel-width=20/40mhz-Ce distance=indoors frequency=auto 

mode=ap-bridge name=wlan1-local ssid="" wireless-protocol=802.11

/interface wireless security-profiles

set [ find default=yes ] supplicant-identity=MikroTik

/interface wireless cap

set caps-man-addresses=127.0.0.1 discovery-interfaces=bridge1 enabled=yes interfaces=wlan1-local

Here is the config of CAP device:

**/interface wireless**

# managed by CAPsMAN

# channel: 2422/20-Ce/gn(30dBm), SSID: , CAPsMAN forwarding

set [ find default-name=wlan1 ] band=2ghz-b/g/n channel-width=20/40mhz-Ce 

distance=indoors frequency=auto mode=ap-bridge ssid="" wireless-protocol=

802.11

/interface wireless security-profiles

set [ find default=yes ] supplicant-identity=MikroTik

/interface wireless cap

set bridge=bridge1 discovery-interfaces=bridge1 enabled=yes interfaces=wlan1

**/interface bridge**

add name=bridge1 protocol-mode=none

/interface bridge port

add bridge=bridge1 interface=ether2

add bridge=bridge1 interface=wlan1

add bridge=bridge1 interface=ether1

add bridge=bridge1 interface=ether3

add bridge=bridge1 interface=ether4

add bridge=bridge1 interface=ether5

             
            
           
          
            
            
              Sorry, I have no ideas. Only difference from me I see is, You added wlan1 to bridge1 in /interface bridge and also in /capsman datapath. I have only second one.

             
            
           
          
            
            
              So I tried this method:

1. Reset configuration to defaults
2. Make everything from the begin
3. In this case Bridge interface doesn’t contain WLAN interface

In this topic I get a suggestion that when I use CAPsMAN, it automatically add WLAN interface (in this case CAP interface) to the bridge.

Current config on the CAPsMAN device:

```
/interface wireless
# managed by CAPsMAN
# channel: 2412/20-Ce/gn(20dBm), SSID: abcd, CAPsMAN forwarding
set [ find default-name=wlan1 ] ssid=MikroTik
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/interface wireless cap
set caps-man-addresses=127.0.0.1 enabled=yes interfaces=wlan1
/interface bridge
add name=bridge1 protocol-mode=none
/interface bridge port
add bridge=bridge1 interface=ether1
/caps-man datapath
add bridge=bridge1 name=datapath1
/caps-man configuration
add country="czech republic" datapath=datapath1 mode=ap name=abcd-cfg ssid=abcd
/caps-man manager
set enabled=yes
/caps-man provisioning
add action=create-dynamic-enabled master-configuration=abcd-cfg name-format=identity
```

Current config on the CAP device:

```
/interface wireless
# managed by CAPsMAN
# channel: 2447/20-Ce/gn(20dBm), SSID: abcd, CAPsMAN forwarding
set [ find default-name=wlan1 ] ssid=MikroTik
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/interface wireless cap
set bridge=bridge1 discovery-interfaces=bridge1 enabled=yes interfaces=wlan1
/interface bridge
add name=bridge1 protocol-mode=none
/interface bridge port
add bridge=bridge1 interface=ether1
add bridge=bridge1 interface=ether2
add bridge=bridge1 interface=ether3
add bridge=bridge1 interface=ether4
add bridge=bridge1 interface=ether5
```

It seems that it works fine. Thank everyone for your help and recommendations.
