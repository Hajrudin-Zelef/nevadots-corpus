---
id: collect-261121-veille-ia/gemini-google/send-feedback-6
title: "ZTNA IP MAC based access control example"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["click", "select", "access", "rule", "ztna", "forticlient", "tags", "endpoint"]
source: https://docs.fortinet.com/document/fortigate/7.4.1/administration-guide/477578/ztna-ip-mac-based-access-control-example
source_anchor: ""
source_lines: [1, 337]
section: "Gemini_Google"
---
# ZTNA IP MAC based access control example

# ZTNA IP MAC based access control example

In this example, firewall policies are configured that use ZTNA tags to control access between on-net devices and an internal web server. This mode does not require the use of the access proxy, and only uses ZTNA tags for access control. Traffic is passed when the FortiClient endpoint meets two conditions.

1. 
                                                    It is tagged with the *Domain-Users* ZTNA tag, identifying the device as logged on to the Domain.
2. 
                                                    It has the *High* importance classification tag indicating the device is High importance and low risk.

 Traffic is denied when the FortiClient endpoint is tagged with *Malicious-File-Detected*.


This example assumes that the FortiGate EMS fabric connector is already successfully connected.

|  | To configure ZTNA in the GUI, go to *System > Feature Visibility* and enable*Zero Trust Network Access* . | 

###### To configure Zero Trust tagging rules on the FortiClient EMS:

1. 
                                                    Log in to the FortiClient EMS.
2. 
                                                    Go to *Zero Trust Tags > Zero Trust Tagging Rules* , and click*Add* .
3. 
                                                    In the *Name* field, enter*Malicious-File-Detected* .
4. 
                                                    In the *Tag Endpoint As* dropdown list, select*Malicious-File-Detected* .
5. 
                                                    Click *Add Rule* then configure the rule:
  1. 
                                                            For *OS* , select*Windows* .
  2. 
                                                            From the *Rule Type* dropdown list, select*File* and click  the + button.
  3. 
                                                            Enter a file name, such as *C:\virus.txt* .
  4. 
                                                            Click *Save* .
6. 
                                                            
7. 
                                                    Click *Save* .
8. 
                                                    Click *Add* again to add another rule.
9. 
                                                    In the *Name* field, enter*Domain-Users* .
10. 
                                                    In the *Tag Endpoint As* dropdown list, enter*Domain-Users* and press`Enter` .
11. 
                                                    Click *Add Rule* , then configure the rule:
  1. 
                                                            For *OS* , select*Windows* .
  2. 
                                                            From the *Rule Type* dropdown list, select*User in AD Group* .
  3. 
                                                            For *AD Group* , select the*Domain-Users* AD group.
  4. 
                                                            Click *Save* .
12. 
                                                            

###### To configure a classification tag on the FortiClient EMS:

1. 
                                                    Go to *Endpoint > All Endpoints* .
2. 
                                                    Select the WIN10-01 computer that will be granted access. This computer should be already registered to FortiClient EMS.
3. 
                                                    In the *Summary* tab, under*Classification Tags* , click*Add* and then set to High Importance.
4. 
                                                    Go to *Administration > Fabric Devices* .
5. 
                                                    Select the connecting FortiGate, then click *Edit* .
6. 
                                                    Under *Tag Types Being Shared* , add*Classification Tags* .
7. 
                                                    Click *Save* .

###### To configure a firewall policy with IP/MAC based access control to deny traffic in the GUI:

1. 
                                                    Go to *Policy & Objects > Firewall Policy* and click*Create New* .
2. 
                                                    Set *Name* to*block-internal-malicious-access* .
3. 
                                                    Set *Type* to*Standard* .
4. 
                                                    Set *Incoming Interface* to*port1* .
5. 
                                                    Set *Outgoing Interface* to*port2* .
6. 
                                                    Set *Source* to*all* .
7. 
                                                    Set *IP/MAC Based Access Control* to the*Malicious-File-Detected* tag.
8. 
                                                    Set *Destination* to the address of the Web server. If no address is created, create a new address object for 10.88.0.3/32.
9. 
                                                    Set *Service* to*ALL* .
10. 
                                                    Set *Action* to*DENY* .
11. 
                                                    Enable *Log Violation Traffic* .
12. 
                                                    Configuring the remaining settings as needed.
13. 
                                                    Click *OK* .

###### To configure a firewall policy with IP/MAC based access control to allow access in the GUI:

1. 
                                                    Go to *Policy & Objects > Firewall Policy* and click*Create New* .
2. 
                                                    Set *Name* to*allow-internal-access* .
3. 
                                                    Set *Type* to*Standard* .
4. 
                                                    Set *Incoming Interface* to*port1* .
5. 
                                                    Set *Outgoing Interface* to*port2* .
6. 
                                                    Set *Source* to*all* .
7. 
                                                    Set *IP/MAC Based Access Control* to the*Domain-Users* ZTNA IP tag.
8. 
                                                    Set *Logical And With Secondary Tags* to*Specify* . This option allows for a second group of tags to be used with a logical And operator.
9. 
                                                    Set *Secondary Tags* as the*High* Class IP tag.
10. 
                                                    Set *Destination* to the address of the Web server.
11. 
                                                    Set *Service* to*ALL* .
12. 
                                                    Set *Action* to*ACCEPT* .
13. 
                                                    Enable *Log Allowed Traffic* and set it to*All Sessions* .
14. 
                                                    Configuring the remaining settings as needed.
15. 
                                                    Click *OK* .

###### To configure firewall policies with IP/MAC based access control to block and allow access in the CLI:

```
config firewall policy
    edit 10
        set name "block-internal-malicious-access"
        set srcintf "port1"
        set dstintf "port2"
        set ztna-status enable
        set srcaddr "all"
        set dstaddr "Webserver"
        set ztna-ems-tag "EMS1_ZTNA_Malicious-File-Detected"
        set schedule "always"
        set service "ALL"
        set logtraffic all
    next
    edit 12
        set name "allow-internal-access"
        set srcintf "port1"
        set dstintf "port2"
        set action accept
        set ztna-status enable
        set srcaddr "all"
        set dstaddr "Webserver"
        set ztna-ems-tag "EMS1_ZTNA_Domain-Users"
        set ztna-ems-tag-secondary "EMS1_CLASS_High"
        set schedule "always"
        set service "ALL"
        set logtraffic all
    next
end
```
                                            |  | When multiple tags are selected with `set ztna-ems-tag <tags>` , matching occurs using a logical OR operator. Therefore any single tag that match will return true. The `set ztna-tags-match-logic {and \| or}` option cannot be used to change the logical operator. This option is applied by wad to tags selected for ZTNA proxy-policy. The `set ztna-ems-tag-secondary <tags>` option by default allows a second group of tags to be specified. This group and the primary group are joined by the logical and operator. | 

## Testing the access to the web server from the on-net client endpoint

###### Access allowed:

1. 
                                                    On the WIN10-01 PC, open FortiClient.
2. 
                                                    On the *Zero Trust Telemetry* tab, make sure that you are connected to the EMS server.
3. 
                                                    Open a browser and enter the address of the server.
4. 
                                                    The FortiGate matches your security posture by verifying your ZTNA tags and matching the corresponding `allow-internal-access` firewall policy, and you are allowed access to the web server.

###### Access denied:

1. 
                                                    On the WIN10-01 PC, trigger the Zero Trust Tagging Rule by creating the file in C:\virus.txt.
2. 
                                                    Open a browser and enter the address of the server.
3. 
                                                    FortiGate checks your security posture. Because EMS has tagged the PC with the *Malicious-File-Detected* tag, it matches the*block-internal-malicious-access* firewall policy.
4. 
                                                    You are denied access to the web server.

### Logs and debugs

###### Access allowed:

```
# diagnose endpoint record list
Record #1:
                IP Address = 
```
**10.0.1.2**
                MAC Address = 02:09:0f:00:01:02
                MAC list = 
                VDOM = root (0)
                EMS serial number: FCTEMS8822001975
                EMS tenant id: 00000000000000000000000000000000
                Client cert SN: 2B8D4FF0E71FE7E064288FE1B4F87E25232092D0
                Public IP address: 34.23.223.220
                Quarantined: no
                **Online status: online
                Registration status: registered
                On-net status: on-net
                Gateway Interface: port1**
                FortiClient version: 7.2.0
                ...
                Number of Routes: (1)
                        Gateway Route #0:
                                - IP:10.0.1.2, MAC: 02:09:0f:00:01:02, VPN: no
                                - Interface:port1, VFID:0, SN: FGVM02TM22013111
**online records: 1;** offline records: 0; quarantined records: 0; out-of-sync records: 0
# diagnose wad dev query-by uid 9A016B5A6E914B42AD4168C066EB04CA FCTEMS8822001975 00000000000000000000000000000000  
Attr of type=0, length=83, value(ascii)=9A016B5A6E914B42AD4168C066EB04CA
Attr of type=4, length=0, value(ascii)=
Attr of type=6, length=1, value(ascii)=true
Attr of type=5, length=40, value(ascii)=2B8D4FF0E71FE7E064288FE1B4F87E25232092D0
Attr of type=3, length=66, value(ascii)=**ZTNA_Domain-Users**_FCTEMS882200197500000000000000000000000000000000
Attr of type=3, length=68, value(ascii)=ZTNA_Remote-Allowed_FCTEMS882200197500000000000000000000000000000000
Attr of type=3, length=83, value(ascii)=ZTNA_Group-Membership-Domain-Users_FCTEMS882200197500000000000000000000000000000000
Attr of type=3, length=59, value(ascii)=**CLASS_High_FCTEMS882200197500000000000000000000000000000000**
Attr of type=3, length=61, value(ascii)=CLASS_Remote_FCTEMS882200197500000000000000000000000000000000
Attr of type=3, length=76, value(ascii)=ZTNA_all_registered_clients_FCTEMS882200197500000000000000000000000000000000
Response termination due to no more data

# diagnose firewall dynamic list
List all dynamic addresses:
IP dynamic addresses in VDOM root(vfid: 0):
...
CMDB name: EMS1_CLASS_High
TAG name: High
**EMS1_CLASS_High**: ID(134)
        RANGE(10.0.1.0-10.0.0.255)
        **ADDR(10.0.1.2)**
Total IP dynamic range blocks: 1.
Total IP dynamic addresses: 0. 
...
CMDB name: EMS1_ZTNA_Domain-Users
TAG name: Domain-Users
**EMS1_ZTNA_Domain-Users**: ID(186)
        RANGE(10.0.1.0-10.0.0.255)
        **ADDR(10.0.1.2)**
Total IP dynamic range blocks: 1.
Total IP dynamic addresses: 0.

# diagnose test application fcnacd 7
Entry #1:
 - UID: 9A016B5A6E914B42AD4168C066EB04CA
 - EMS Fabric ID: FCTEMS8822001975:00000000000000000000000000000000
 - Sys upd time: 2023-05-11 01:39:29.5936762
 - Tag upd time: 2023-05-11 06:24:59.1435977
lls_idx_mask = 0x00000001
#ID:0
UID:     9A016B5A6E914B42AD4168C066EB04CA
State:   sysinfo:1, tag:1, tagsz:1, out-of-sync:0
Owner:   
Cert SN: 2B8D4FF0E71FE7E064288FE1B4F87E25232092D0
online:  Yes
**Route IP:10.0.1.2**
vfid:    0
has more:No
Tags:
idx:0, ttdl:1   **name:Domain-Users**
idx:1, ttdl:1   name:Remote-Allowed
idx:2, ttdl:1   name:Group-Membership-Domain-Users
idx:3, ttdl:2   **name:High**
idx:5, ttdl:2   name:Remote
idx:6, ttdl:1   name:all_registered_clients

# execute log filter field srcip 10.0.1.2
# execute log display
35: date=2023-05-10 time=23:22:14 eventtime=1683786134265076528 tz="-0700" logid="0000000013" type="traffic" subtype="forward" level="notice" vd="root" **srcip=10.0.1.2** srcport=14358 srcintf="port1" srcintfrole="undefined" **dstip=10.88.0.3 dstport=9443** dstintf="port2" dstintfrole="dmz" srcuuid="b458a65a-f759-51ea-d7df-ef2e750026d1" dstuuid="592dfb72-0775-51ec-aa79-94bd9894388c" srccountry="Reserved" dstcountry="Reserved" sessionid=177080 proto=6 action="server-rst" **policyid=12** policytype="policy" poluuid="aae1d38a-efc2-51ed-e820-ff7964c9bdeb" **policyname="allow-internal-access"** service="tcp/9443" trandisp="noop" duration=130 sentbyte=2821 rcvdbyte=310602 sentpkt=31 rcvdpkt=222 appcat="unscanned" sentdelta=0 rcvddelta=40

###### Access denied:

# diagnose wad dev query-by uid 9A016B5A6E914B42AD4168C066EB04CA FCTEMS8822001975 00000000000000000000000000000000
Attr of type=0, length=83, value(ascii)=9A016B5A6E914B42AD4168C066EB04CA
Attr of type=4, length=0, value(ascii)=
Attr of type=6, length=1, value(ascii)=true
Attr of type=5, length=40, value(ascii)=2B8D4FF0E71FE7E064288FE1B4F87E25232092D0
Attr of type=3, length=66, value(ascii)=ZTNA_Domain-Users_FCTEMS882200197500000000000000000000000000000000
Attr of type=3, length=68, value(ascii)=ZTNA_Remote-Allowed_FCTEMS882200197500000000000000000000000000000000
Attr of type=3, length=83, value(ascii)=ZTNA_Group-Membership-Domain-Users_FCTEMS882200197500000000000000000000000000000000
Attr of type=3, length=59, value(ascii)=CLASS_High_FCTEMS882200197500000000000000000000000000000000
Attr of type=3, length=77, value(ascii)=**ZTNA_Malicious-File-Detected**_FCTEMS882200197500000000000000000000000000000000
Attr of type=3, length=61, value(ascii)=CLASS_Remote_FCTEMS882200197500000000000000000000000000000000
Attr of type=3, length=76, value(ascii)=ZTNA_all_registered_clients_FCTEMS882200197500000000000000000000000000000000

# diagnose firewall dynamic list
 List all dynamic addresses:
 ...
CMDB name: EMS1_ZTNA_Malicious-File-Detected
TAG name: **Malicious-File-Detected**
EMS1_ZTNA_Malicious-File-Detected: ID(205)
        RANGE(10.0.1.0-10.0.0.255)
        **ADDR(10.0.1.2)**
Total IP dynamic range blocks: 1.
Total IP dynamic addresses: 0.

# diagnose test application fcnacd 7
Entry #1:
...
State:   sysinfo:1, tag:1, tagsz:1, out-of-sync:0
Owner:   
Cert SN: 2B8D4FF0E71FE7E064288FE1B4F87E25232092D0
online:  Yes
Route IP:**10.0.1.2**
vfid:    0
has more:No
Tags:
idx:0, ttdl:1   name:Domain-Users
idx:1, ttdl:1   name:Remote-Allowed
idx:2, ttdl:1   name:Group-Membership-Domain-Users
idx:3, ttdl:2   name:High
idx:4, ttdl:1   name:**Malicious-File-Detected**
idx:5, ttdl:2   name:Remote
idx:6, ttdl:1   name:all_registered_clients

# execute log filter field srcip 10.0.1.2
# execute log display 
 
1: date=2023-05-10 time=23:37:02 eventtime=1683787022146761572 tz="-0700" logid="0000000013" type="traffic" subtype="forward" level="notice" vd="root" **srcip=10.0.1.2** srcport=14609 srcintf="port1" srcintfrole="undefined" **dstip=10.88.0.3 dstport=9443** dstintf="port2" dstintfrole="dmz" srcuuid="b458a65a-f759-51ea-d7df-ef2e750026d1" dstuuid="592dfb72-0775-51ec-aa79-94bd9894388c" srccountry="Reserved" dstcountry="Reserved" sessionid=177409 proto=6 **action="deny" policyid=10** policytype="policy" poluuid="92938512-ef9a-51ed-6a39-bafb9147e9aa" **policyname="block-internal-malicious-access"** service="tcp/9443" trandisp="noop" duration=0 sentbyte=0 rcvdbyte=0 sentpkt=0 rcvdpkt=0 appcat="unscanned" crscore=30 craction=131072 crlevel="high"
