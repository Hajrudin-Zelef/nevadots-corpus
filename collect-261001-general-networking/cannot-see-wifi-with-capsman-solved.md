Hi there,

I’m new to the CapsMan setup. Firstly I strugled to get a youtube video info on setting CAPsMAN up using the latest Winbox on WiFi 6 equipment as the CAPsMAN menu moved.

Router - RB5009UPr+S+

Indoor Wifi - cAP ax Dual Band - cAPGi-5HaxD2HaxD

Outdoor Wifi  - NetMetal ax - L23UGSR-5HaxD2HaxD-NM

I’m using the latest Winbox and all units are upgraded to OS 7.17.2

I found this video from MikroTik “CAPsMAN evolved - central AP management for WiFi6” and setup everything accordingly using the cAP ax first to test the CAPsMAN as the router does not have built in wifi. Once I sorted the CAPsMAN out to work on the cAP ax, I will then setup CAPsMAN on my Router to remotely change everything on my wifi units:

https://youtu.be/37aff6d14Xk?si=48JSMSq1q6ON52-f

When CAPsMAN is not activated I see my WiFi and can connect and everything works 100%, as soon as I activate CAPsMAN on the devices, I no longer see the WiFi and my phone automatically disconnect as the WiFi is gone.

Where did I miss something. What I would like to achive is to have 1 SSID for all my WiFi units and for both 2G and 5G network and CAPsMAN sort out the best signal connection at the place where I am.

Below are my settings:


             
            
           
          
            
            
              Can you please supply the config:

```
/export file=anynameyoulike
```

Remove serial and any other private info, post between code tags by using the </> button.

Did you create provision rules (like mentioned in the documentation):

https://help.mikrotik.com/docs/spaces/ROS/pages/224559120/WiFi#WiFi-CAPsMAN-CAPsimpleconfigurationexample:

Sure Max Tx Power set to 0 is correct?

Are we looking at the cAP AX screenshots? Because CAPsMAN can’t manage its own interfaces:

CAPsMAN cannot manage it’s own wifi interfaces using configuration.manager=capsman, it is enough to just set the same configuration profile on local interfaces manually as you would with provisioning rules, and the end result will be the same as if they were CAPs. That being said, it is also possible to provision local interfaces via /interface/wifi/radio menu, it should be noted that to regain control of local interfaces after provisioning, you will need to disable the matching provisioning rules and press “provision” again, which will return local interfaces to an unconfigured state.


No need to install additional packages on the RB5009, CAPsMAN is available.

             
            
           
          
            
            
              Hi yes the screenshots are from the cAP AX. as mentioned I first just wanted to test if the capsman work by joining the 2.4 & 5GHz using a single SSID.

The reason the TX Max power, etc are 0 is because I left everything as "default" settings. I'm not to advanced adjusting all those settings.

This is the script from the cAP AX


\

# 2025-02-19 09:44:55 by RouterOS 7.17.2

# software id = WWE0-FFJ0


# model = cAPGi-5HaxD2HaxD

# serial number = *******

/interface bridge

add name=bridge1

/interface wifi channel

add band=2ghz-ax disabled=no frequency=2412,2432,2472 name=2Ghz width=20mhz

add band=5ghz-ax disabled=no frequency=5180-5850 name=5Ghz width=20/40/80mhz

/interface wifi datapath

add bridge=bridge1 disabled=no name=datapath1

/interface wifi security

add authentication-types=wpa2-psk,wpa3-psk disabled=no name=secKoekWiFi

/interface wifi configuration

add channel=2Ghz country="South Africa" datapath=datapath1 disabled=no 

manager=capsman mode=ap name=cfg_2GHz security=secKoekWiFi ssid=

KoekiesWiFi station-roaming=no tx-power=0

add channel=5Ghz country="South Africa" datapath=datapath1 disabled=no 

manager=capsman mode=ap name=cfg_5G security=secKoekWiFi ssid=KoekiesWiFi

/interface wifi

# managed by CAPsMAN **:**:**:**:**:**%bridge1

set [ find default-name=wifi1 ] configuration=cfg_5G configuration.mode=ap 

.tx-power=0 disabled=no

# managed by CAPsMAN **:**:**:**:**:**%bridge1

set [ find default-name=wifi2 ] configuration=cfg_2GHz configuration.mode=ap 

disabled=no

/interface bridge port

add bridge=bridge1 interface=ether1

add bridge=bridge1 interface=ether2

add bridge=bridge1 interface=wifi1

add bridge=bridge1 interface=wifi2

/interface wifi cap

set caps-man-addresses=127.0.0.1 discovery-interfaces=bridge1 enabled=yes

/interface wifi capsman

set interfaces=bridge1 package-path="" require-peer-certificate=no 

upgrade-policy=none

/interface wifi provisioning

add action=create-dynamic-enabled disabled=no

/ip address

add address=192.168.11.41/24 interface=bridge1 network=192.168.11.0

/ip dhcp-client

add interface=bridge1

/system clock

set time-zone-name=Africa/Johannesburg

/system identity

set name="WiFi Kantoor"

/system note

set show-at-login=no

             
            
           
          
            
            
              Setting tx-power to 0 is not default, you need to remove it, and you should set CAPsMAN address to 127.0.0.1, the reply by erlinden is correct regarding CAPsMAN managing it’s local interfaces. Aside from that, check what frequency is selected by CAPs, it might be that upper frequency is selected on 5GHz interface that your wireless client might not support, sometimes that’s the case if SSID cannot be seen.

             
            
           
          
            
            
              Thanx I see, I by mistake clicked the TX MAX then it add the 0, I have deleted it.

If you look at my first post in the bottom left corner I have set the CAPsMAN Address to 127.0.0.1

I have also expanded the 5GHz range to the default value 2300-7300 but still I do not see the WiFi SSID on any of my devices or laptops. When I disable CAPsMAN I see the SSID

Below is a new script after the changes made

# 2025-02-19 12:13:39 by RouterOS 7.17.2

# software id = WWE0-FFJ0


# model = cAPGi-5HaxD2HaxD

# serial number = ******

/interface bridge

add name=bridge1

/interface wifi channel

add band=2ghz-ax disabled=no frequency=2412,2432,2472 name=2Ghz width=20mhz

add band=5ghz-ax disabled=no frequency=2300-7300 name=5Ghz width=20/40/80mhz

/interface wifi security

add authentication-types=wpa2-psk,wpa3-psk disabled=no name=secKoekWiFi

/interface wifi configuration

add channel=2Ghz country="South Africa" disabled=no manager=capsman mode=ap 

name=cfg_2GHz security=secKoekWiFi ssid=KoekiesWiFi station-roaming=no

add channel=5Ghz country="South Africa" disabled=no manager=capsman mode=ap 

name=cfg_5G security=secKoekWiFi ssid=KoekiesWiFi

/interface wifi

# managed by CAPsMAN 78:9A:18:A2:F5:9E%bridge1

set [ find default-name=wifi1 ] configuration=cfg_5G configuration.mode=ap 

disabled=no

# managed by CAPsMAN 78:9A:18:A2:F5:9E%bridge1, traffic processing on CAP

set [ find default-name=wifi2 ] configuration=cfg_2GHz configuration.mode=ap 

disabled=no

/interface bridge port

add bridge=bridge1 interface=ether1

add bridge=bridge1 interface=ether2

add bridge=bridge1 interface=wifi1

add bridge=bridge1 interface=wifi2

/interface wifi cap

set caps-man-addresses=127.0.0.1 discovery-interfaces=bridge1 enabled=yes

/interface wifi capsman

set interfaces=bridge1 package-path="" require-peer-certificate=no 

upgrade-policy=none

/interface wifi provisioning

add action=create-dynamic-enabled disabled=no

/ip address

add address=192.168.11.41/24 interface=bridge1 network=192.168.11.0

/ip dhcp-client

add interface=bridge1

/system clock

set time-zone-name=Africa/Johannesburg

/system identity

set name="WiFi Kantoor"

/system note

set show-at-login=no

             
            
           
          
            
            
              I meant to say “shouldn’t”, or rather you must not set it to 127.0.0.1. See the text quoted earlier.

             
            
           
          
            
            
              Ok I have made some changes:

1.>> Setup my router (RB5009UPr+S+) to handle the CAPsMAN setting

2.>> Setup the cAP AX to retrieve the CAPsMAN settings from the router

Somewhere I'm making a mistake using the Winbox user interface to setup CAPsMAN as the cAP AX is receiving some CAPsMAN config but not everything. Below are the scripts for the router and cAP AX

**Router Script:**

# 2025-02-19 13:36:40 by RouterOS 7.17.2

# software id = FC9Z-1KWF


# model = RB5009UPr+S+

# serial number = *****

/interface bridge

add name=bridge1

/interface ethernet

set [ find default-name=ether1 ] comment=WAN name="ether1[internet]"

/interface wifi channel

add band=2ghz-ax disabled=no frequency=2412,2432,2472 name="2.4Ghz AX" width=

20mhz

add band=5ghz-ax disabled=no frequency=2300-7300 name="5Ghz AX" width=

20/40/80mhz

/interface wifi datapath

add bridge=bridge1 disabled=no name=datapath1

/interface wifi security

add authentication-types=wpa2-psk,wpa3-psk disabled=no name=secKoekWiFi

/interface wifi configuration

add channel="2.4Ghz AX" country="South Africa" datapath=datapath1 disabled=no 

manager=capsman mode=ap name=cfg_WiFi2GHz_AX security=secKoekWiFi ssid=

KoekiesWiFi

add channel="5Ghz AX" country="South Africa" datapath=datapath1 disabled=no 

mode=ap name=cfg_WiFi5GHz_AX security=secKoekWiFi security.connect-group=

"" ssid=KoekiesWiFi

/ip pool

add name=dhcp_pool0 ranges=192.168.11.100-192.168.11.199

/ip dhcp-server

add address-pool=dhcp_pool0 interface=bridge1 lease-time=8h name=dhcp1

/interface bridge port

add bridge=bridge1 interface=ether2

add bridge=bridge1 interface=ether3

add bridge=bridge1 interface=ether4

add bridge=bridge1 interface=ether5

add bridge=bridge1 interface=ether6

add bridge=bridge1 interface=ether7

add bridge=bridge1 interface=ether8

add bridge=bridge1 interface=sfp-sfpplus1

/ip firewall connection tracking

set udp-timeout=10s

/ip neighbor discovery-settings

set discover-interface-list=!dynamic

/interface ovpn-server server

add mac-address=**:**:**:**:**:** name=ovpn-server1

/interface wifi capsman

set ca-certificate=auto certificate=auto enabled=yes interfaces=all 

package-path="" require-peer-certificate=no upgrade-policy=none

/interface wifi provisioning

add action=create-dynamic-enabled disabled=no

/ip address

add address=192.168.11.1/24 comment="LAN IP" interface=bridge1 network=

192.168.11.0

/ip dhcp-client

add comment="Internet WAN" interface="ether1[internet]"

/ip dhcp-server network

add address=192.168.11.0/24 gateway=192.168.11.1

/ip firewall filter

add action=fasttrack-connection chain=forward connection-state=

established,related hw-offload=yes

add action=accept chain=forward connection-state=established,related

add action=drop chain=forward connection-nat-state="" connection-state=

invalid

add action=drop chain=forward connection-mark="" connection-nat-state=!dstnat 

connection-state=new in-interface="ether1[internet]"

/ip firewall nat

add action=masquerade chain=srcnat comment="Internet Connection Rule" 

out-interface="ether1[internet]"

/ip ipsec profile

set [ find default=yes ] dpd-interval=2m dpd-maximum-failures=5

/ip service

set telnet disabled=yes

set ftp disabled=yes

set www disabled=yes

set api disabled=yes

set api-ssl disabled=yes

/system clock

set time-zone-name=Africa/Johannesburg

/system identity

set name="MikroTik Router"

/system note

set show-at-login=no

**cAP AX Script:**

# 2025-02-19 13:37:26 by RouterOS 7.17.2

# software id = WWE0-FFJ0


# model = cAPGi-5HaxD2HaxD

# serial number = ******

/interface bridge

add name=bridge1

/interface wifi

# managed by CAPsMAN 192.168.11.1, traffic processing on CAP

set [ find default-name=wifi1 ] configuration.manager=capsman .mode=ap 

disabled=no

# managed by CAPsMAN 192.168.11.1, traffic processing on CAP

set [ find default-name=wifi2 ] configuration.manager=capsman .mode=ap 

disabled=no

/interface bridge port

add bridge=bridge1 interface=ether1

add bridge=bridge1 interface=ether2

add bridge=bridge1 interface=wifi1

add bridge=bridge1 interface=wifi2

/interface wifi cap

set caps-man-addresses=192.168.11.1 enabled=yes

/interface wifi capsman

set enabled=yes interfaces=bridge1 package-path="" require-peer-certificate=

no upgrade-policy=none

/interface wifi provisioning

add action=create-dynamic-enabled disabled=no

/ip address

add address=192.168.11.41/24 interface=bridge1 network=192.168.11.0

/ip dhcp-client

add interface=bridge1

/system clock

set time-zone-name=Africa/Johannesburg

/system identity

set name="WiFi Kantoor"

/system note

set show-at-login=no

Belwo is the screenshots from the Router and cAP AX:

**Router:**


**cAP AX:**
             
            
           
          
            
            
              manager=capsman needs to removed from configuration you are passing via CAPsMAN to CAP, and you need to two separate provisioning rules, it would be best to add one rule for each, using supported-bands as a matcher.

             
            
           
          
            
            
              ```
/interface wifi provisioning
add action=create-dynamic-enabled disabled=no
```

What should be provisioned? You are missing the configuration (“master-configuration”) here.

```
/interface wifi capsman
set enabled=yes interfaces=bridge1 package-path="" require-peer-certificate=\
no upgrade-policy=none
```

You should disable CAPsMAN on the cAP AX, as it has no purpose.

             
            
           
          
            
            
              Thank you, I was setting the provision but as you mentioned no “master” was selected (I did not know that this was required - BIG mistake!!). I selected the master and now everythig is working.