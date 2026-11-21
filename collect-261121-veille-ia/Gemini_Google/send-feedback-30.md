---
id: collect-261121-veille-ia/gemini-google/send-feedback-30
title: "IPsec VPN wizard hub-and-spoke ADVPN support"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["tunnel", "spoke", "click", "configuration", "advpn", "wizard", "interface", "next"]
source: https://docs.fortinet.com/document/fortigate/8.0.0/administration-guide/853412/ipsec-vpn-wizard-hub-and-spoke-advpn-support
source_anchor: ""
source_lines: [1, 191]
section: "Gemini_Google"
---
# IPsec VPN wizard hub-and-spoke ADVPN support

# IPsec VPN wizard hub-and-spoke ADVPN support

Auto Discovery VPN (ADVPN) enables spokes in a hub-and-spoke topology to dynamically establish direct tunnels (shortcuts) with each other, bypassing the hub for spoke-to-spoke traffic. This reduces latency and avoids bottlenecking traffic through a central hub.

When using the IPsec VPN wizard to create a hub-and-spoke VPN, multiple local interfaces can be selected. At the end of the wizard, changes can be reviewed, real-time updates can be made to the local address group and tunnel interface, and easy configuration keys can be copied for configuring the spokes. These keys pre-populate spoke settings such as the hub tunnel IP, simplifying deployment and reducing the risk of misconfiguration.

This example shows the configuration of a hub with two spokes.


## Configuration Steps

###### To configure the hub:

1. 
                                                    Go to *VPN > VPN Wizard* .
2. 
                                                    Enter the tunnel name *ADVPN* .
3. 
                                                    Select the Template *Hub and Spoke with ADVPN* .
4. 
                                                    Click *Begin* .
5. 
                                                    On the first page, select the Role *Hub* .
6. 
                                                    Click *Next* .
7. 
                                                    In the *VPN tunnel* configuration section:Setting Value Authentication method Pre-shared Key. For added security, use a digital signature Pre-shared key <key> IKE Version 2
8. 
                                                    Click *Next* .
9. 
                                                    In the *Hub* configuration section:Setting Value Incoming interface that binds to tunnel port3 Create and add interface to zone Disable. Enable to have the interface added to a zone Hub tunnel IP/netmask 10.10.1.1/24 Local interface port1 Local subnets that can access VPN 10.0.1.0/24 Local AS 65400
10. 
                                                    Click *Next* .
11. 
                                                    In the *Spoke* configuration section:Setting Value Configure spoke tunnel IP Individually Spoke tunnel IP 10.10.1.2 10.10.1.3
12. 
                                                    Click *Next* .
13. 
                                                    *Review Settings* :Confirm that the settings look correct, then click *Submit* .
14. 
                                                    In the VPN Tunnels list page, click the new ADVPN tunnel to view its settings.
15. 
                                                    Under Hub & spoke topology, see the 2 Spokes and their Easy Config Key. These can be used to configure the spoke FortiGates.

###### To configure the spokes:

1. 
                                                    Go to *VPN > IPsec Wizard* .
2. 
                                                    Enter the tunnel name *ADVPN-S1*
3. 
                                                    Select the Template *Hub and Spoke with ADVPN*
4. 
                                                    Click *Begin* .
5. 
                                                    On the first page, select the Role *Spoke*
6. 
                                                    Using the Easy Config Key for Spoke 1 (10.10.1.2), paste the key to Easy configuration key.
7. 
                                                    Click *Next* .
8. 
                                                    In the *VPN tunnel* configuration section:Setting Value Authentication method Pre-shared Key. For added security, use a digital signature Pre-shared key <key> IKE Version 2
9. 
                                                    Click Next
10. 
                                                    In the *Spoke* configuration section:Setting Value Incoming interface that binds to tunnel port2 Create and add interface to zone Disable. Enable to have the interface added to a zone Spoke tunnel IP 10.10.1.2 (Pre-configured by the Key) Local interface port3 Local subnets that can access VPN 10.0.1.0/24 Local AS 65400
11. 
                                                    Click Next
12. 
                                                    In the *Hub* configuration section:Setting Value Hub tunnel IP/netmask 10.10.1.1 255.255.255.0 (Pre-configured by the Key) Hub public IP address 203.0.113.249 (Pre-configured by the Key)
13. 
                                                    Review settings. Confirm that the settings look correct, then click *Submit* .
14. 
                                                    Follow the same steps to configure the second spoke. Use the Easy Config Key for 10.10.1.3

## Tunnel Verification

1. 
                                                    On the hub FortiGate, go to *Dashboard > Network Monitor > VPN* .The tunnels to the spokes are established. Each Spoke will have a tunnel to the Hub.
2. 
                                                    On a spoke, go to *Dashboard > Network Monitor > VPN* .The tunnel to the hub is established. If another Spoke is up, a spoke to spoke shortcut is also established.

## CLI configurations

###### Hub:

```
config vpn ipsec phase1-interface
    edit "ADVPN"
        set type dynamic
        set interface "port3"
        set ike-version 2
        set peertype any
        set net-device disable
        set proposal aes128-sha256 aes256-sha256 aes128gcm-prfsha256 aes256gcm-prfsha384 chacha20poly1305-prfsha256
        set add-route disable
        set dpd on-idle
        set dhgrp 20 21
        set wizard-type hub-fortigate-auto-discovery
        set auto-discovery-sender enable
        set psksecret ENC \<key\>
    next
end
```
```
config vpn ipsec phase2-interface
    edit "ADVPN"
        set phase1name "ADVPN"
        set proposal aes128-sha256 aes256-sha256 aes128gcm aes256gcm chacha20poly1305
        set dhgrp 20 21
    next
end
```
```
config firewall policy
    edit 21
        set name "vpn_ADVPN_spoke2hub"
        set srcintf "ADVPN"
        set dstintf "port1"
        set action accept
        set srcaddr "all"
        set dstaddr "ADVPN_local"
        set schedule "always"
        set service "ALL"
    next
    edit 22
        set name "vpn_ADVPN_spoke2spoke"
        set srcintf "ADVPN"
        set dstintf "ADVPN"
        set action accept
        set srcaddr "all"
        set dstaddr "all"
        set schedule "always"
        set service "ALL"
    next
end
```
                                            ###### Spoke:

```
config vpn ipsec phase1-interface
    edit "ADVPN-S1"
        set interface "port2"
        set ike-version 2
        set peertype any
        set net-device enable
        set proposal aes128-sha256 aes256-sha256 aes128gcm-prfsha256 aes256gcm-prfsha384 chacha20poly1305-prfsha256
        set add-route disable
        set dpd on-idle
        set dhgrp 20 21
        set wizard-type spoke-fortigate-auto-discovery
        set auto-discovery-receiver enable
        set transport auto
        set remote-gw 203.0.113.249
        set psksecret ENC \<key\>
    next
end
config vpn ipsec phase2-interface
    edit "ADVPN-S1"
        set phase1name "ADVPN-S1"
        set proposal aes128-sha256 aes256-sha256 aes128gcm aes256gcm chacha20poly1305
        set dhgrp 20 21
    next
end
```
```
config firewall policy
    edit 5
        set name "vpn_ADVPN-S1_remote"
        set srcintf "ADVPN-S1"
        set dstintf "port3"
        set action accept
        set srcaddr "all"
        set dstaddr "ADVPN-S1_local"
        set schedule "always"
        set service "ALL"
    next
    edit 6
        set name "vpn_ADVPN-S1_local"
        set srcintf "port3"
        set dstintf "ADVPN-S1"
        set action accept
        set srcaddr "all"
        set dstaddr "all"
        set schedule "always"
        set service "ALL"
    next
end
```
