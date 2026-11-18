In this lesson we will focus Multi Area OSPF Configuration on Packet Tracer. We will work with **OSPF Standard Area** and **OSPF Backbone Area** together for our OSPF configuration example. In other words, we will do a **Multi Area OSPF Packet Tracer Configuration** with Backbone Area and OSPF Standard Areas. In other lesson, we will also give example for the other **OSPF Area Types** and their configuration examples on **Packet Tracer**.

You can **DOWNLOAD** the **Cisco Packet Tracer** example with **.pkt** format **at the end of this lesson**.

As you know from the previous lessons, **OSPF** **(Open Shortest Path First)** has **6** different **areas**. These OSPF Area Types are given below:


**You can check the below lessons for the configuration of different OSPF Area Types:**

In this OSPF Configuration lesson, we will focus on **Backbone Area** and **Standard (Normal) Area**. Beside this, we will see the configuration of **virtual-link** on OSPF. What is **virtual-link in OSPF?** It is the link that used to connect the normal areas to the backbone area, if they are not connected directly to the backbone area. Remember, in OSPF, there was a rule. All areas must be connected to the backbone area, **Area 0**. If they are not, they can temporarily connect to the backbone via **virtual-links**.

*OSPF Backbone Area and Standard Area with Accepted LSAs*


**You can also check Basic OSPF Confiuration**

**What is Packet Tracer?** | **Download Packet Tracer** 

**Packet Tracer Labs Coure** | **Download Packet Tracer Labs**


**Backbone Area** is also a Normal Area but it is **Area 0 OSPF**.

Normal Areas accept the **Summary LSAs** from other Areas **(Type 3 and Type 4 LSAs)**. They accept also the **External LSAs (Type 5 LSAs)**. Type 1 and Type 2 LSA are already accepted inside area.



Table of Contents

We talked about theorical too much. This post aims to show you the configuration of this areas and as you know, doing the configuration is the most effective way of learning network protocols.

Our topology will be like below fort he first example.

Firstly let’s configure the IP Addresses on all routers:

__Router1__

```
Router1>
```
**enable**
Router1# **configure terminal**
Router1(config)# **interface GigabitEthernet0/0**
Router1(config-if)# **ip address 10.1.0.1 255.255.255.0**
Router1(config-if)# **no shutdown**
Router1(config-if)# **exit**
Router1(config)# **interface GigabitEthernet0/1**
Router1(config-if)# **ip address 10.2.0.1 255.255.255.0**
Router1(config-if)# **no shutdown**
Router1(config-if)# **end**
Router1# **copy running-config startup-config**

__Router2__

```
Router2>
```
**enable**
Router2# **configure terminal**
Router2(config)# **interface GigabitEthernet0/0**
Router2(config-if)# **ip address 10.1.0.2 255.255.255.0**
Router2(config-if)# **no shutdown**
Router2(config-if)# **exit**
Router2(config)# **interface GigabitEthernet0/1**
Router2(config-if)# **ip address 10.3.0.1 255.255.255.0**
Router2(config-if)# **no shutdown**
Router2(config-if)# **end**
Router2# **copy running-config startup-config**

__Router3__

```
Router3>
```
**enable**
Router3# **configure terminal**
Router3(config)# **interface GigabitEthernet0/0**
Router3(config-if)# **ip address 10.2.0.2 255.255.255.0**
Router3(config-if)# **no shutdown**
Router3(config-if)# **exit**
Router3# **copy running-config startup-config**

__Router4__

```
Router4>
```
**enable**
Router4# **configure terminal**
Router4(config)# **interface GigabitEthernet0/0**
Router4(config-if)# **ip address 10.4.0.1 255.255.255.0**
Router4(config-if)# **no shutdown**
Router4(config-if)# **exit**
Router4(config)# **interface GigabitEthernet0/1**
Router4(config-if)# **ip address 10.3.0.2 255.255.255.0**
Router4(config-if)# **no shutdown**
Router4(config-if)# **end**
Router4# **copy running-config startup-config**

__Router5__

```
Router5>
```
**enable**
Router5# **configure terminal**
Router5(config)# **interface GigabitEthernet0/0**
Router5(config-if)# **ip address 10.4.0.2 255.255.255.0**
Router5(config-if)# **no shutdown**
Router5(config-if)# **exit**
Router5(config)# **interface GigabitEthernet0/1**
Router5(config-if)# **ip address 10.5.0.1 255.255.255.0**
Router5(config-if)# **no shutdown**
Router5(config-if)# **end**
Router5# **copy running-config startup-config**

__Router6__

```
Router6>
```
**enable**
Router6# **configure terminal**
Router6(config)# **interface GigabitEthernet0/1**
Router6(config-if)# **ip address 10.5.0.2 255.255.255.0**
Router6(config-if)# **no shutdown**
Router6(config-if)# **end**
Router6# **copy running-config startup-config**

After this basic interface configurations, now, we will focus on the main part of our **Multi Area OSPF Packet Tracer Configuraiton example.** Here, we will configure **OSPF (Open Shortest Path First)** on all routers. Here, OSPF process number will be 1 and the Routerx’s router ID will be x.x.x.x. Beside this, all the connected areas will be configured.

```
Router1>
```
**enable**
Router1# **configure terminal**
Router1(config)# **router ospf 1**
Router1(config-router)# **router-id 1.1.1.1**
Router1(config-router)# **network 10.1.0.0 0.0.0.255 area 0**
Router1(config-router)# **network 10.2.0.0 0.0.0.255 area 1**
Router1(config-router)# **end**
Router1# **copy running-config startup-config**

```
Router2>
```
**enable**
Router2# **configure terminal**
Router2(config)# **router ospf 1**
Router2(config-router)# **router-id 2.2.2.2**
Router2(config-router)# **network 10.1.0.0 0.0.0.255 area 0**
Router2(config-router)# **network 10.3.0.0 0.0.0.255 area 2**
Router2(config-router)# **end**
Router2# **copy running-config startup-config**

```
Router3>
```
**enable**
Router3# **configure terminal**
Router3(config)# **router ospf 1**
Router3(config-router)# **router-id 3.3.3.3**
Router3(config-router)# **network 10.2.0.0 0.0.0.255 area 1**
Router3(config-router)# **end**
Router3# **copy running-config startup-config**

```
Router4>
```
**enable**
Router4# **configure terminal**
Router4(config)# **router ospf 1**
Router4(config-router)# **router-id 4.4.4.4**
Router4(config-router)# **network 10.3.0.0 0.0.0.255 area 2**
Router4(config-router)# **network 10.4.0.0 0.0.0.255 area 2**
Router4(config-router)# **end**
Router4# **copy running-config startup-config**

```
Router5>
```
**enable**
Router5# **configure terminal**
Router5(config)# **router ospf 1**
Router5(config-router)# **router-id 5.5.5.5**
Router5(config-router)# **network 10.4.0.0 0.0.0.255 area 2**
Router5(config-router)# **network 10.5.0.0 0.0.0.255 area 3**
Router5(config-router)# **end**
Router5# **copy running-config startup-config**

```
Router6>
```
**enable**
Router6# **configure terminal**
Router6(config)# **router ospf 1**
Router6(config-router)# **router-id 6.6.6.6**
Router6(config-router)# **network 10.5.0.0 0.0.0.255 area 3**
Router6(config-router)# **end**
Router6# **copy running-config startup-config**

After doing this configuration we will see all the network on **Topology Table** except **Area 3**. Area 3 is not directly connected to the Backbone Area,Area 0. So in the routing table of the routers there will be no route to this Area 3.

To overcome this issue, on **Router 2** and **Router 5**, we will configure the **virtual-link**. After this configuration, we will see all the routes in all the routers’ Topology Tables.


In this step, we will configureOSPF Virtual-Link between Router 2 and Router 5. After this configuration, we will see all the routes in all the routers’ Topology Tables.

```
Router2>
```
**enable**
Router2# **configure terminal**
Router2(config)# **router ospf 1**
Router2(config-router)# **area 2 virtual-link 5.5.5.5**
Router2(config-router)# **end**
Router2# **copy running-config startup-config**

```
Router5>
```
**enable**
Router5# **configure terminal**
Router5(config)# **router ospf 1**
Router5(config-router)# **area 2 virtual-link 2.2.2.2**
Router5(config-router)# **end**
Router5# **copy running-config startup-config**

We have completed our **Multi Area OSPF Packet Tracer Configuration**. Now, let’s verify the **Routing Tables, Topology Databases (OSPF Databases)** and **OSPF Neighbourhip** on all OSPF routers.

```
Router1#
```
**show ip ospf neighbor** 
Neighbor ID     Pri   State           Dead Time   Address         Interface
2.2.2.2           1   FULL/DR         00:00:38    10.1.0.2        GigabitEthernet0/0
3.3.3.3           1   FULL/DR         00:00:37    10.2.0.2        GigabitEthernet0/1

```
Router1#
```
**show ip ospf database** 
            OSPF Router with ID (1.1.1.1) (Process ID 1)
                Router Link States (Area 0)
Link ID         ADV Router      Age         Seq#       Checksum Link count
1.1.1.1         1.1.1.1         223         0x8000000b 0x0079af 1
5.5.5.5         5.5.5.5         802         0x80000005 0x009874 1
2.2.2.2         2.2.2.2         228         0x8000000b 0x00e604 2
                Net Link States (Area 0)
Link ID         ADV Router      Age         Seq#       Checksum
10.1.0.2        2.2.2.2         228         0x80000005 0x001b1b
                Summary Net Link States (Area 0)
Link ID         ADV Router      Age         Seq#       Checksum
10.2.0.0        1.1.1.1         1009        0x80000006 0x00d078
10.3.0.0        2.2.2.2         909         0x8000000a 0x009ea1
10.4.0.0        2.2.2.2         884         0x8000000b 0x009aa2
10.4.0.0        5.5.5.5         812         0x8000000e 0x0030fe
10.3.0.0        5.5.5.5         802         0x8000000f 0x0044e9
10.5.0.0        5.5.5.5         482         0x80000010 0x00200c
                Router Link States (Area 1)
Link ID         ADV Router      Age         Seq#       Checksum Link count
1.1.1.1         1.1.1.1         224         0x8000000a 0x008f98 1
3.3.3.3         3.3.3.3         229         0x80000009 0x000415 1
                Net Link States (Area 1)
Link ID         ADV Router      Age         Seq#       Checksum
10.2.0.2        3.3.3.3         229         0x80000005 0x00f67e
                Summary Net Link States (Area 1)
Link ID         ADV Router      Age         Seq#       Checksum
10.1.0.0        1.1.1.1         1008        0x80000013 0x00c27a
10.3.0.0        1.1.1.1         902         0x80000014 0x00b286
10.4.0.0        1.1.1.1         879         0x80000015 0x00ae87
10.5.0.0        1.1.1.1         477         0x80000016 0x00aa88

```
Router1#
```
**show ip route** 
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
       * - candidate default, U - per-user static route, o - ODR
       P - periodic downloaded static route
Gateway of last resort is not set
     10.0.0.0/8 is variably subnetted, 7 subnets, 2 masks
C       10.1.0.0/24 is directly connected, GigabitEthernet0/0
L       10.1.0.1/32 is directly connected, GigabitEthernet0/0
C       10.2.0.0/24 is directly connected, GigabitEthernet0/1
L       10.2.0.1/32 is directly connected, GigabitEthernet0/1
O IA    10.3.0.0/24 [110/2] via 10.1.0.2, 02:04:54, GigabitEthernet0/0
O IA    10.4.0.0/24 [110/3] via 10.1.0.2, 02:04:54, GigabitEthernet0/0
O IA    10.5.0.0/24 [110/4] via 10.1.0.2, 02:04:54, GigabitEthernet0/0

```
Router2#
```
**show ip ospf neighbor** 
Neighbor ID     Pri   State           Dead Time   Address         Interface
1.1.1.1           1   FULL/BDR        00:00:39    10.1.0.1        GigabitEthernet0/0
5.5.5.5           0   FULL/  -        00:00:39    10.4.0.2        OSPF_VL0
3.3.3.3           1   FULL/DR         00:00:39    10.3.0.2        GigabitEthernet0/1

```
Router2#
```
**show ip ospf database**
            OSPF Router with ID (2.2.2.2) (Process ID 1)
                Router Link States (Area 0)
Link ID         ADV Router      Age         Seq#       Checksum Link count
2.2.2.2         2.2.2.2         349         0x8000000b 0x00e604 2
5.5.5.5         5.5.5.5         922         0x80000005 0x009874 1
1.1.1.1         1.1.1.1         344         0x8000000b 0x0079af 1
                Net Link States (Area 0)
Link ID         ADV Router      Age         Seq#       Checksum
10.1.0.2        2.2.2.2         349         0x80000005 0x001b1b
                Summary Net Link States (Area 0)
Link ID         ADV Router      Age         Seq#       Checksum
10.3.0.0        2.2.2.2         1030        0x8000000a 0x009ea1
10.4.0.0        2.2.2.2         1005        0x8000000b 0x009aa2
10.2.0.0        1.1.1.1         1131        0x80000006 0x00d078
10.4.0.0        5.5.5.5         933         0x8000000e 0x0030fe
10.3.0.0        5.5.5.5         922         0x8000000f 0x0044e9
10.5.0.0        5.5.5.5         603         0x80000010 0x00200c
                Router Link States (Area 2)
Link ID         ADV Router      Age         Seq#       Checksum Link count
2.2.2.2         2.2.2.2         344         0x8000000a 0x0057c6 1
3.3.3.3         3.3.3.3         349         0x8000000d 0x001fc4 2
5.5.5.5         5.5.5.5         349         0x8000000a 0x00946e 1
                Net Link States (Area 2)
Link ID         ADV Router      Age         Seq#       Checksum
10.4.0.2        5.5.5.5         941         0x80000005 0x001739
10.3.0.2        3.3.3.3         349         0x80000006 0x003b8b
                Summary Net Link States (Area 2)
Link ID         ADV Router      Age         Seq#       Checksum
10.1.0.0        2.2.2.2         1039        0x8000000b 0x00b48c
10.2.0.0        2.2.2.2         1039        0x8000000c 0x00b08d
10.5.0.0        5.5.5.5         603         0x80000013 0x001a0f
10.1.0.0        5.5.5.5         335         0x80000014 0x005ccd

```
Router2#
```
**show ip route**
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
       * - candidate default, U - per-user static route, o - ODR
       P - periodic downloaded static route
Gateway of last resort is not set
     10.0.0.0/8 is variably subnetted, 7 subnets, 2 masks
C       10.1.0.0/24 is directly connected, GigabitEthernet0/0
L       10.1.0.2/32 is directly connected, GigabitEthernet0/0
O IA    10.2.0.0/24 [110/2] via 10.1.0.1, 02:06:09, GigabitEthernet0/0
C       10.3.0.0/24 is directly connected, GigabitEthernet0/1
L       10.3.0.1/32 is directly connected, GigabitEthernet0/1
O       10.4.0.0/24 [110/2] via 10.3.0.2, 02:15:59, GigabitEthernet0/1
O IA    10.5.0.0/24 [110/3] via 10.3.0.2, 02:10:17, GigabitEthernet0/1

```
Router3#
```
**show ip ospf neighbor** 
Neighbor ID     Pri   State           Dead Time   Address         Interface
1.1.1.1           1   FULL/BDR        00:00:34    10.2.0.1        GigabitEthernet0/1

```
Router3#
```
**show ip ospf database**
            OSPF Router with ID (3.3.3.3) (Process ID 1)
                Router Link States (Area 1)
Link ID         ADV Router      Age         Seq#       Checksum Link count
3.3.3.3         3.3.3.3         394         0x80000009 0x000415 1
1.1.1.1         1.1.1.1         389         0x8000000a 0x008f98 1
                Net Link States (Area 1)
Link ID         ADV Router      Age         Seq#       Checksum
10.2.0.2        3.3.3.3         394         0x80000005 0x00f67e
                Summary Net Link States (Area 1)
Link ID         ADV Router      Age         Seq#       Checksum
10.1.0.0        1.1.1.1         1174        0x80000013 0x00c27a
10.3.0.0        1.1.1.1         1067        0x80000014 0x00b286
10.4.0.0        1.1.1.1         1045        0x80000015 0x00ae87
10.5.0.0        1.1.1.1         642         0x80000016 0x00aa88

```
Router3#
```
**show ip route**
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
       * - candidate default, U - per-user static route, o - ODR
       P - periodic downloaded static route
Gateway of last resort is not set
     10.0.0.0/8 is variably subnetted, 6 subnets, 2 masks
O IA    10.1.0.0/24 [110/2] via 10.2.0.1, 02:47:08, GigabitEthernet0/1
C       10.2.0.0/24 is directly connected, GigabitEthernet0/1
L       10.2.0.2/32 is directly connected, GigabitEthernet0/1
O IA    10.3.0.0/24 [110/3] via 10.2.0.1, 02:47:08, GigabitEthernet0/1
O IA    10.4.0.0/24 [110/4] via 10.2.0.1, 02:17:52, GigabitEthernet0/1
O IA    10.5.0.0/24 [110/5] via 10.2.0.1, 02:11:08, GigabitEthernet0/1
```
Router4#
```
**show ip ospf neighbor** 
Neighbor ID     Pri   State           Dead Time   Address         Interface
5.5.5.5           1   FULL/DR         00:00:36    10.4.0.2        GigabitEthernet0/0
2.2.2.2           1   FULL/BDR        00:00:38    10.3.0.1        GigabitEthernet0/1
```
Router4#
```
**show ip ospf database**
            OSPF Router with ID (3.3.3.3) (Process ID 1)
                Router Link States (Area 2)
Link ID         ADV Router      Age         Seq#       Checksum Link count
3.3.3.3         3.3.3.3         429         0x8000000d 0x001fc4 2
5.5.5.5         5.5.5.5         429         0x8000000a 0x00946e 1
2.2.2.2         2.2.2.2         425         0x8000000a 0x0057c6 1
                Net Link States (Area 2)
Link ID         ADV Router      Age         Seq#       Checksum
10.3.0.2        3.3.3.3         429         0x80000006 0x003b8b
10.4.0.2        5.5.5.5         1021        0x80000005 0x001739
                Summary Net Link States (Area 2)
Link ID         ADV Router      Age         Seq#       Checksum
10.1.0.0        2.2.2.2         1119        0x8000000b 0x00b48c
10.2.0.0        2.2.2.2         1119        0x8000000c 0x00b08d
10.5.0.0        5.5.5.5         684         0x80000013 0x001a0f
10.1.0.0        5.5.5.5         415         0x80000014 0x005ccd
                Router Link States (Area 3)
Link ID         ADV Router      Age         Seq#       Checksum Link count
3.3.3.3         3.3.3.3         1113        0x80000007 0x00ac9a 0
                Net Link States (Area 3)
Link ID         ADV Router      Age         Seq#       Checksum
10.4.0.1        3.3.3.3         764         0x80000006 0x003b1d

```
Router4#
```
**show ip route**
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
       * - candidate default, U - per-user static route, o - ODR
       P - periodic downloaded static route
Gateway of last resort is not set
     10.0.0.0/8 is variably subnetted, 7 subnets, 2 masks
O IA    10.1.0.0/24 [110/2] via 10.3.0.1, 02:46:14, GigabitEthernet0/1
O IA    10.2.0.0/24 [110/3] via 10.3.0.1, 02:46:14, GigabitEthernet0/1
C       10.3.0.0/24 is directly connected, GigabitEthernet0/1
L       10.3.0.2/32 is directly connected, GigabitEthernet0/1
C       10.4.0.0/24 is directly connected, GigabitEthernet0/0
L       10.4.0.1/32 is directly connected, GigabitEthernet0/0
O IA    10.5.0.0/24 [110/2] via 10.4.0.2, 02:11:46, GigabitEthernet0/0

```
Router5#
```
**show ip ospf neighbor** 
Neighbor ID     Pri   State           Dead Time   Address         Interface
6.6.6.6           1   FULL/DR         00:00:31    10.5.0.2        GigabitEthernet0/1
2.2.2.2           0   FULL/  -        00:00:31    10.3.0.1        OSPF_VL0
3.3.3.3           1   FULL/BDR        00:00:31    10.4.0.1        GigabitEthernet0/0

```
Router5#
```
**show ip ospf database**
            OSPF Router with ID (5.5.5.5) (Process ID 1)
                Router Link States (Area 0)
Link ID         ADV Router      Age         Seq#       Checksum Link count
5.5.5.5         5.5.5.5         1059        0x80000005 0x009874 1
2.2.2.2         2.2.2.2         487         0x8000000b 0x00e604 2
1.1.1.1         1.1.1.1         481         0x8000000b 0x0079af 1
                Net Link States (Area 0)
Link ID         ADV Router      Age         Seq#       Checksum
10.1.0.2        2.2.2.2         487         0x80000005 0x001b1b
                Summary Net Link States (Area 0)
Link ID         ADV Router      Age         Seq#       Checksum
10.4.0.0        5.5.5.5         1069        0x8000000e 0x0030fe
10.3.0.0        5.5.5.5         1059        0x8000000f 0x0044e9
10.5.0.0        5.5.5.5         739         0x80000010 0x00200c
10.2.0.0        1.1.1.1         1268        0x80000006 0x00d078
10.3.0.0        2.2.2.2         1168        0x8000000a 0x009ea1
10.4.0.0        2.2.2.2         1142        0x8000000b 0x009aa2
                Router Link States (Area 2)
Link ID         ADV Router      Age         Seq#       Checksum Link count
5.5.5.5         5.5.5.5         485         0x8000000a 0x00946e 1
3.3.3.3         3.3.3.3         486         0x8000000d 0x001fc4 2
2.2.2.2         2.2.2.2         481         0x8000000a 0x0057c6 1
                Net Link States (Area 2)
Link ID         ADV Router      Age         Seq#       Checksum
10.4.0.2        5.5.5.5         1076        0x80000005 0x001739
10.3.0.2        3.3.3.3         486         0x80000006 0x003b8b
                Summary Net Link States (Area 2)
Link ID         ADV Router      Age         Seq#       Checksum
10.5.0.0        5.5.5.5         739         0x80000013 0x001a0f
10.1.0.0        2.2.2.2         1175        0x8000000b 0x00b48c
10.2.0.0        2.2.2.2         1175        0x8000000c 0x00b08d
10.1.0.0        5.5.5.5         471         0x80000014 0x005ccd
                Router Link States (Area 3)
Link ID         ADV Router      Age         Seq#       Checksum Link count
5.5.5.5         5.5.5.5         486         0x8000000c 0x009768 1
6.6.6.6         6.6.6.6         485         0x80000008 0x005e9d 1
                Net Link States (Area 3)
Link ID         ADV Router      Age         Seq#       Checksum
10.5.0.2        6.6.6.6         679         0x80000005 0x00330c
                Summary Net Link States (Area 3)
Link ID         ADV Router      Age         Seq#       Checksum
10.1.0.0        5.5.5.5         1040        0x80000010 0x0064c9

```
Router5#
```
**show ip route**
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
       * - candidate default, U - per-user static route, o - ODR
       P - periodic downloaded static route
Gateway of last resort is not set
     10.0.0.0/8 is variably subnetted, 7 subnets, 2 masks
O       10.1.0.0/24 [110/3] via 10.4.0.1, 01:38:19, GigabitEthernet0/0
O IA    10.2.0.0/24 [110/4] via 10.4.0.1, 01:38:09, GigabitEthernet0/0
O       10.3.0.0/24 [110/2] via 10.4.0.1, 02:08:15, GigabitEthernet0/0
C       10.4.0.0/24 is directly connected, GigabitEthernet0/0
L       10.4.0.2/32 is directly connected, GigabitEthernet0/0
C       10.5.0.0/24 is directly connected, GigabitEthernet0/1
L       10.5.0.1/32 is directly connected, GigabitEthernet0/1

```
Router6#
```
**show ip ospf neighbor** 
Neighbor ID     Pri   State           Dead Time   Address         Interface
5.5.5.5           1   FULL/BDR        00:00:37    10.5.0.1        GigabitEthernet0/1
Router6#**show ip ospf database**
            OSPF Router with ID (6.6.6.6) (Process ID 1)
                Router Link States (Area 3)
Link ID         ADV Router      Age         Seq#       Checksum Link count
6.6.6.6         6.6.6.6         520         0x80000008 0x005e9d 1
5.5.5.5         5.5.5.5         522         0x8000000c 0x009768 1
                Net Link States (Area 3)
Link ID         ADV Router      Age         Seq#       Checksum
10.5.0.2        6.6.6.6         714         0x80000005 0x00330c
                Summary Net Link States (Area 3)
Link ID         ADV Router      Age         Seq#       Checksum
10.1.0.0        5.5.5.5         1076        0x80000010 0x0064c9

```
Router6#
```
**show ip route** 
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
       * - candidate default, U - per-user static route, o - ODR
       P - periodic downloaded static route
Gateway of last resort is not set
     10.0.0.0/8 is variably subnetted, 3 subnets, 2 masks
O IA    10.1.0.0/24 [110/4] via 10.5.0.1, 02:12:20, GigabitEthernet0/1
C       10.5.0.0/24 is directly connected, GigabitEthernet0/1
L       10.5.0.2/32 is directly connected, GigabitEthernet0/1

You can also used **“show ip route ospf”** command for displaying only OSPF routes like below:

```
Router1#
```
**show ip route ospf** 
     10.0.0.0/8 is variably subnetted, 7 subnets, 2 masks
O IA    10.3.0.0 [110/2] via 10.1.0.2, 02:14:52, GigabitEthernet0/0
O IA    10.4.0.0 [110/3] via 10.1.0.2, 02:14:52, GigabitEthernet0/0
O IA    10.5.0.0 [110/4] via 10.1.0.2, 02:14:52, GigabitEthernet0/0

Lastly, let’s check the virtual-links.

```
Router2#
```
**show ip ospf virtual-links** 
Virtual Link OSPF_VL0 to router 5.5.5.5 is up
  Run as demand circuit
  Transit area 2, via interface GigabitEthernet0/1, Cost of using 2
  Transmit Delay is 1 sec, State POINT_TO_POINT,
  Timer intervals configured, Hello 10, Dead 40, Wait 40, Retransmit 5
    Hello due in 00:00:09
    Adjacency State FULL
    Index 1/2, retransmission queue length 0, number of retransmission 0
        First 0x0(0)/0x0(0) Next 0x0(0)/0x0(0)
        Last retransmission scan length is 0, maximum is 0
        Last retransmission scan time is 0 msec, maximum is 0 msec

```
Router5#
```
**show ip ospf virtual-links** 
Virtual Link OSPF_VL0 to router 2.2.2.2 is up
  Run as demand circuit
  Transit area 2, via interface GigabitEthernet0/0, Cost of using 2
  Transmit Delay is 1 sec, State POINT_TO_POINT,
  Timer intervals configured, Hello 10, Dead 40, Wait 40, Retransmit 5
    Hello due in 00:00:00
    Adjacency State FULL
    Index 1/2, retransmission queue length 0, number of retransmission 0
        First 0x0(0)/0x0(0) Next 0x0(0)/0x0(0)
        Last retransmission scan length is 0, maximum is 0
        Last retransmission scan time is 0 msec, maximum is 0 msec

In this post, we have talked about Standard OSPF Areas and Backbone Areas, beside Virtual-Links. We focused on configuration mostly. I hope this post will be helpful for you.

In the next post we will focus on other OSPF Area Types. Keep on, IPcisco.com ;)

You can **DOWNLOAD** the **Packet Tracer** example with **.pkt** format  **HERE**.




**You can download “Packet Tracer” in Tools section.**

Gokhan Kosem is a Network Engineer, Instructor and the Founder of IPCisco.com with 15+ years of experience in Cisco, Nokia, Huawei, Juniper, Linux, Service Provider Networks, Routing and Switching technologies.

He has worked on the backbone networks of major service providers and network vendors including Nortel, Alcatel-Lucent (Nokia) and has extensive hands-on experience with Cisco, Huawei, Juniper and Nokia networking technologies.

He has trained thousands of networking students worldwide through IPCisco.com, Udemy, books, labs, quizzes, and educational content across multiple social media platforms.

IPCisco.com | Best Route to Your Dreams

## Leave a Reply