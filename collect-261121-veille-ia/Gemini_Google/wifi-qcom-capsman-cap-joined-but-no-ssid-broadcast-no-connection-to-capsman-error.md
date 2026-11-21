---
id: collect-261121-veille-ia/gemini-google/wifi-qcom-capsman-cap-joined-but-no-ssid-broadcast-no-connection-to-capsman-error
title: "Wifi-qcom CAPsMAN - CAP joined but no SSID broadcast (no connection to CAPsMAN error)"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["capsman", "interface", "wifi", "configuration", "ssid", "connection", "disabled", "name"]
source: https://forum.mikrotik.com/t/wifi-qcom-capsman-cap-joined-but-no-ssid-broadcast-no-connection-to-capsman-error/268325
source_anchor: ""
source_lines: [1, 480]
section: "Gemini_Google"
---
Hello Guys,

I've been trying to set up my managed wifi network for a week now, but I'm struggling even with the first CAP. It seems like a simple configuration, but I'm stuck.

My Setup:

- CAPsMAN: hAP ax2
- CAP: hAP ax lite
- Firmware:All devices are running RouterOS 7.21.2 with the wifi-qcom package installed.

The Issue:

The CAP appears to be under CAPsMAN control, but it doesn't seem to broadcast the SSID. On the floor where the ax lite is located, the signal is non-existent, as if the radio were off.

Strange symptom: On the CAP side, the interface shows as "Bound", but on the CAPsMAN side, the dynamic interface shows a comment: "no connection to CAPsMAN"

I lack the experience to spot the error here. Any help would be greatly appreciated.

---**1. CAP Side - Interface Status:**

The MBI flags are present, which is concerning.

````
Flags: M - MASTER; B - BOUND; I - INACTIVE
#   NAME    CONFIGURATION.MODE
;;; managed by CAPsMAN 04:F4:1C:34:81:DD%bridgeLocal
0 MBI wifi1   ap
```**2. CAP Configuration:**
```/interface wifi
set [ find default-name=wifi1 ] configuration.manager=capsman .mode=ap disabled=no
/interface wifi cap
set certificate=request discovery-interfaces=bridgeLocal enabled=yes slaves-datapath=*1
```**3. CAP Logs (after toggle):**
```2026-02-04 09:57:07 caps,info selected CAPsMAN ax2@04:F4:1C:34:81:DD%*7
2026-02-04 09:57:07 caps,info connected to ax2@04:F4:1C:34:81:DD%*7
**1. Interface Status on Manager:**
Note the "no connection" comment.
[netadm@ax2] /interface/wifi> print
Flags: M - MASTER; B - BOUND; I - INACTIVE, R - RUNNING
#      NAME         CONFIGURATION.MODE   CONFIGURATION.SSID
;;; no connection to CAPsMAN
0 MBI  cap-wifi1                         wMw
**2. CAPsMAN Configuration:**
/interface wifi capsman
set ca-certificate=auto certificate=auto enabled=yes interfaces=bridge require-peer-certificate=no upgrade-policy=none
/interface wifi configuration
add country=Hungary datapath=dp1 disabled=no manager=capsman name=conf1 security=sec1 ssid=wMw steering=st1
/interface wifi provisioning
add action=create-dynamic-enabled disabled=no master-configuration=conf1
**3. CAPsMAN Logs:**
I see a lot of "no suitable CAPsMAN" messages.
2026-02-04 09:57:07 caps,info axl@48:A9:8A:A8:59:8C%*9 joined
2026-02-04 09:57:17 caps,debug no suitable CAPsMAN
2026-02-04 09:57:30 caps,debug no suitable CAPsMAN
Thank you in advance for your help!
````

             
            
           
          
            
            
              This worries me:

Can you reset the CAP to CAPS Mode:

```
/system reset capsmode
```

And please provide `/interface wifi export` for both CAP and CAPsMAN and post between Preformatted text tags by using the </> button.

             
            
           
          
            
            
              Hi,

I have performed the full system reset on the CAP as requested (/system reset-configuration capsmode=yes). Unfortunately, the situation remains exactly the same:

On CAPsMAN side: I still see the "No connection to CAPsMAN" error on the dynamic interface.

On CAP side: The interface says "managed by CAPsMAN 04:F4:1C:34:81:DD%bridgeLocal", but remains Inactive.

Here are the fresh, clean exports from both devices:

## CAPsMAN (hAP ax2) /interface/wifi export:

```
/interface wifi security
add authentication-types=wpa2-psk,wpa3-psk disabled=no name=sec1
/interface wifi steering
add name=st1 neighbor-group=dynamic
/interface wifi
set [ find default-name=wifi1 ] configuration=conf1 configuration.manager=local .mode=ap disabled=no
set [ find default-name=wifi2 ] configuration=conf1 configuration.manager=local .mode=ap disabled=no
/interface wifi cap
set certificate=request discovery-interfaces=bridge enabled=yes
/interface wifi capsman
set ca-certificate=auto certificate=auto enabled=yes interfaces=all require-peer-certificate=no upgrade-policy=none
/interface wifi configuration
add country=Hungary datapath=dp1 disabled=no manager=capsman name=conf1 security=sec1 ssid=wMw steering=st1
/interface wifi datapath
add bridge=bridge disabled=no name=dp1
/interface wifi provisioning
add action=create-dynamic-enabled disabled=no master-configuration=conf1
```

## CAP (hAP ax lite) / interface/wifi export:

```
/interface wifi
managed by CAPsMAN 04:F4:1C:34:81:DD%bridgeLocal
set [ find default-name=wifi1 ] configuration.manager=capsman datapath=capdp
/interface wifi cap
set discovery-interfaces=bridgeLocal enabled=yes slaves-datapath=capdp
/interface wifi datapath
```

add bridge=bridgeLocal comment=defconf disabled=no name=capdp

Thank you for your continued help!

             
            
           
          
            
            
              The only thing I notice is that you set interfaces to all:

```
/interface wifi capsman
set ca-certificate=auto certificate=auto enabled=yes interfaces=all require-peer-certificate=no upgrade-policy=none
```

Better change the interfaces to the bridge on the CAPsMAN.

In regards to the "no connection". Is this the radio of the CAP or is it the radio on the CAPsMAN? You can figure that out by checking the MAC Address.

If that doesn't provide information, I would reset the wifi settings on the CAPsMAN and reconfigure it (this can be done by executing the config commands).

             
            
           
          
            
            
              Hi erlinden,

I have changed the CAPsMAN interfaces back to bridge as suggested. Regarding the previous interfaces=all setting, I only used it as a "brute-force" test to eliminate any potential interface-level blocking while troubleshooting; now it's back to the standard bridge configuration.

Regarding the "no connection" error: It appears on the interface belonging to the CAP (hAP ax lite). Its radio MAC is 48:A9:8A:A8:59:90. The local radios of the CAPsMAN (ax2) are working fine in local mode.

Even after changing the interface to bridge, the CAP still shows "Inactive" on the manager side with the same error. Here is the detailed status of the CAP interface from the CAPsMAN side:

```
Flags: M - master; D - dynamic; B - bound; X - disabled, I - inactive, R - running
0 MDBI ;;; no connection to CAPsMAN
cap="MikroTik@48:A9:8A:A8:59:8C%*9" name="cap-wifi1" mac-address=48:A9:8A:A8:59:90 arp-timeout=auto
radio-mac=48:A9:8A:A8:59:90 configuration=conf1
configuration.ssid="wMw" .country=Hungary .manager=capsman
security.authentication-types=wpa2-psk,wpa3-psk
datapath.bridge=bridge
steering.neighbor-group=dynamic
```

I am now considering your suggestion to reset the wifi configuration on the CAPsMAN side and re-apply the settings.

Before I do that, is there any specific log topic I should enable to see why the manager is rejecting the connection after provisioning the interface?

Regards

             
            
           
          
            
            
              
Not AFAIK.

Make sure to do an export with show sensitive:

```
/interface wifi
export show-sensitive
```

But befor doing so...can you delete the dynamic interface?

Is the interface removed when you disable CAP on the CAPS?

Can you do an `/interface export` on the CAPsMAN and share it?

             
            
           
          
            
            
              Hi erlinden,

Here are the results of the tests you requested:

Manual removal on CAPsMAN: I could not manually delete the dynamic interface while the CAP was active.

Interface removal via CAP: When I disable CAP mode on the CAP device, the dynamic interface disappears from the CAPsMAN list immediately.

Full interface export from CAPsMAN (Passphrases obfuscated):


```
2026-02-05 12:34:13 by RouterOS 7.21.2
software id = CKNL-JXW2
model = C52iG-5HaxD2HaxD
serial number = HJR0ARECNZ1
/interface bridge
add admin-mac=04:F4:1C:34:81:DD auto-mac=no comment=defconf name=bridge
/interface ethernet switch
set 0 cpu-flow-control=yes
/interface list
add comment=defconf name=WAN
add comment=defconf name=LAN
/interface wifi datapath
add bridge=bridge disabled=no name=dp1
/interface wifi security
add authentication-types=wpa2-psk,wpa3-psk disabled=no name=sec1 passphrase=
***
/interface wifi steering
add name=st1 neighbor-group=dynamic
/interface wifi configuration
add country=Hungary datapath=dp1 disabled=no manager=capsman name=conf1 
security=sec1 ssid=wMw steering=st1
/interface wifi
set [ find default-name=wifi1 ] configuration=conf1 configuration.manager=
local .mode=ap disabled=no
set [ find default-name=wifi2 ] configuration=conf1 configuration.manager=
local .mode=ap disabled=no
/interface bridge port
add bridge=bridge comment=defconf interface=ether2
add bridge=bridge comment=defconf interface=ether3
add bridge=bridge comment=defconf interface=ether4
add bridge=bridge comment=defconf interface=ether5
/interface list member
add comment=defconf interface=bridge list=LAN
add comment=defconf interface=ether1 list=WAN
/interface wifi cap
set certificate=request discovery-interfaces=bridge
/interface wifi capsman
set ca-certificate=auto certificate=auto enabled=yes interfaces=bridge 
require-peer-certificate=no upgrade-policy=none
/interface wifi provisioning
add action=create-dynamic-enabled disabled=no master-configuration=conf1
```

I hope this helps to identify any bridge or tagging issues.******

             
            
           
          
            
            
              Doesn't make sense as everything is set correct (as far as I can see).

Please remove your passphrase as it is of no use for this problem.

             
            
           
          
            
            
              Hi,

I have edited the post and replaced the requested information with '***'. I turned to this forum specifically to overcome this issue with the help of more experienced users. I have truly examined everything that I could think of. I am looking forward to any advice from the pros.

Thank you.

             
            
           
          
            
            
              Just to be sure, what RouterOS and firmware version are you running on the CAPS and CAPsMAN? Make sure to have all versions equal.

Before resetting CAPsMAN, please give this a try on the **CAP**:

```
# reset the wifi interface
/interface wifi reset wifi1
# this could give an error when the datapath is still available
/interface wifi datapath
add bridge=bridgeLocal comment=defconf disabled=no name=capdp
# proceed even if the above statement gives an error
/interface wifi
set [ find default-name=wifi1 ] configuration.manager=capsman datapath=capdp
/interface wifi cap
set discovery-interfaces=bridgeLocal enabled=yes slaves-datapath=capdp
```

             
            
           
          
            
            
              Hi,

I finally found the misconfiguration! Although I don't 100% understand why it affected the remote CAP this way (as I thought it would only cause issues for the CAPsMAN's local radios), changing this one setting fixed everything.

The culprit: In my /interface wifi configuration, I had the manager set strictly to capsman: /interface wifi configuration add ... manager=capsman

The fix: I changed the manager mode to capsman-or-local. After this, cap disable and enable again,  and the connection established immediately on both sides.

CAPsMAN (ax2) status now:

```
Flags: M - MASTER; D - DYNAMIC; B - BOUND; R - RUNNING
NAME       CONFIGURATION.MODE  CONFIGURATION.SSID
;;; operated by CAP 48:A9:8A:A8:59:8C%bridge, traffic processing on CAP
0 MDB  cap-wifi1                      wMw
1 M BR wifi1      ap                  wMw
2 M BR wifi2      ap                  wMw
```

CAP (ax lite) status now:

```
Flags: M - MASTER; B - BOUND; R - RUNNING
NAME
;;; managed by CAPsMAN 04:F4:1C:34:81:DD%bridgeLocal, traffic processing on CAP
;;; mode: AP, SSID: wMw, channel: 2472/ax/eC
0 MBR wifi1
```

It seems that when using manager=capsman, the system might be too restrictive for certain CAP-side operations, even if provisioning seems to succeed at first. Changing it to capsman-or-local solved the "no connection" loop.

But anyway one could tell precise explanation, please let us know.

I hope this helps someone else struggling with the same issue on RouterOS 7.21.2!

Thank you.

             
            
           
          
            
            
              Huh, that's a funny one. IMO it's intended for configuring local wifi interfaces but it seems that it interferes with capsman ...

CAP interfaces, run by CAPsMAN, are "managed by local" on CAPsMAN device. And actual local interfaces should be managed by "local" anyway. Surely, the same setting on CAP device should be set to "capsman".

             
            
           
          
            
            
              
Exactly on which device did you change this mode ?

If on capsman controller which also has local radios, it should be local. Not even capsman-or-local.

If on APs, it should be capsman (unless you also foresee some local config in case communication to capsman controller becomes unavailable, it's a fallback option).

If this was on controller and this resulted in all remote caps to not behave as they should, then it might be worthwhile to file a bug report.

             
            
           
          
            
            
              I agree, it's definitely a 'funny' one. Logically, you are right: the CAP device should be 'capsman' and the local should be 'local'.

             
            
           
          
            
            
              Hi,

I changed it on the **CAPsMAN controller. That is why I couldn’t understand exactly the situation and would be happy for an explanation, how it could work.**

To clarify: the 'manager=capsman' setting was part of the configuration profile that the controller (hapac2) was provisioning to the remote CAPs (hapax lite). As soon as I updated that profile on the controller to 'capsman-or-local', and remote CAP connected.

Before this change, the remote CAPs would see the CAPsMAN, attempt to bind, but then immediately drop with a 'no connection to CAPsMAN' message, even though the configuration seemed to be pushed successfully.

             
            
           
          
            
            
              But still, on the hapAC2, it should be only "local".

As holvoeth explained, the setting for capsman-or-local is intended on the CAPS devices, NOT on the CAPSMAN device, as a sort of failover/fallback configuration in case of connection problems with the CAPSMAN.

What should be happening right now should be that the hapAC2 (the CAPSMAN) looks for a CAPSMAN on the LAN, cannot find it (as there is none) and resorts to use the local.

Setting it as local should avoid this initial search for CAPSMAN (though probably it has no particular side effect).

             
            
           
          
            
            
              To clarify the topology and address the recent comments: In my setup, the ax2 is the sole CAPsMAN controller. Both the ax lite and the hAP ac2 are configured strictly as CAP devices.

Following the logic mentioned by holvoeth and jaclaz, setting the manager to 'capsman' on these CAP devices should be the 'correct' way. However, in practice with ROS 7.21.2, that setting caused a continuous connection loop—the CAPs would bind and then immediately drop.

I just finished integrating the hAP ac2 (after upgrading it to 7.21.2 and installing the wifi-qcom-ac package). Just like with the ax lite, using 'capsman-or-local' in the configuration profile was the only way to achieve a stable connection. Once the wifi interfaces were added to the local bridge on the ac2, provisioning completed successfully.

Here is the result from the hAP ac2 acting as a CAP:

```
Flags: M - MASTER; B - BOUND; R - RUNNING
Columns: NAME
NAME
;;; managed by CAPsMAN 04:F4:1C:34:81:DD%bridgeLocal, traffic processing on CAP
;;; mode: AP, SSID: wMw, channel: 2417/n/Ce
0 MBR wifi1
```

Whether the CAPs are 'resorting to local' or not, this is the only configuration that results in a stable MBR state. It appears that in this ROS version, the 'pure capsman' manager setting interferes with the provisioning handshake for remote CAPs.

             
            
           
          
            
            
              Maybe it is a bug specific on 7.21.2?

But it is more probable that there is *something else* escaping us.

Can you post your latest exports?

In your original ones I noticed (maybe relevant, maybe not) that you had the setting in two (actually three) places (not specifically this one, but I remember people having issues when the same setting was in more than one  places).

In:

/interface wifi

set [ find default-name=wifi1 ] configuration=conf1 configuration.**manager=local** .mode=ap disabled=no

set [ find default-name=wifi2 ] configuration=conf1 configuration.**manager=local** .mode=ap disabled=no

And in:

/interface wifi configuration

add country=Hungary datapath=dp1 disabled=no **manager=capsman** name=conf1 security=sec1 ssid=wMw steering=st1

             
            
           
          
            
            
              
May be it is time to load 7.19.6 or some similar trusted version to test this hypothesis?

             
            
           
          
            
            
              
I don't remember exactly where, but the developer wrote that in the configuration settings, you don't need to set the manager field; leave it blank by default.
