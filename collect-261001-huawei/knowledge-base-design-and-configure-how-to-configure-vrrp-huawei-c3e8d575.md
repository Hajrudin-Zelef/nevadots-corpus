Poland
GRANDMETRIC Sp. z o.o.
ul. Metalowa 5, 60-118 Poznań, Poland
NIP 7792433527
+48 61 271 04 43
info@grandmetric.com
UK
Grandmetric LTD
Office 584b
182-184 High Street North
London
E6 2JA
+44 20 3321 5276
info@grandmetric.com
US Region
Grandmetric LLC
Lewes DE 19958
16192 Coastal Hwy USA
EIN: 98-1615498
+1 302 691 94 10 
info@grandmetric.com
Technology: Routing
Area: NHRP
Vendor: Huawei
Software: eNSP
Platform: Routing Platforms
The Virtual Router Redundancy Protocol (VRRP) groups multiple routing devices into a virtual router and uses the virtual gateway device’s IP address as the default gateway address. When the gateway fails, VRRP selects a new gateway to transmit service traffic to ensure reliable communication.
To configure VRRP, use the following command:
<HuaweiI> system-view
[Huawei] interface GigabitEtherent 0/0/1
[Huawei- interface GigabitEtherent 0/0/1]undo portswitch
[Huawei-interface GigabitEtherent 0/0/1]vrrp vrid 10 virtual-ip 10.1.1.1
[Huawei-interface GigabitEtherent 0/0/1]vrrp vrid 10 priority 120 
[Huawei-interface GigabitEtherent 0/0/1]vrrp vrid 10 preempt-mode timer delay 20 
[Huawei- interface GigabitEtherent 0/0/1] quit