# Example SD-WAN configurations using ADVPN 2.0

## Example SD-WAN configurations using ADVPN 2.0

The configuration example illustrates the edge discovery and path management processes for a typical hub and spoke topology. This example focuses on SD-WAN configuration for steering traffic and establishing shortcuts in the direction from Spoke 1 to Spoke 2.

|  | Currently, ADVPN 2.0 only supports IPv4. | 

In this example, BGP per overlay was used for dynamic routing to distribute the LAN routes behind each spoke to the other spoke. However, this was a design choice. You can also use BGP on loopback for this example.


Spokes 1 and 2 have the following VPN overlays between themselves and the hub:

| VPN Overlays | IP address on Spoke 1 | IP address on Spoke 2 | 
|---|---|---|
| H1_T11 | 172.31.80.1/32 | 172.31.80.2/32 | 
| H1_T22 | 172.31.81.1/32 | 172.31.81.2/32 | 
| H1_T33 | 172.31.82.1/32 | 172.31.82.2/32 | 

SD-WAN Rules/Services defined on Spoke 1:

|  | SD-WAN Rule/Service 1 | SD-WAN Rule/Service 2 | SD-WAN Rule/Service 3 | 
|---|---|---|---|
|    | H1_T11 | H1_T22 | H1_T33 | 
|  | H1_T22 | H1_T11 | H1_T11 | 
|  | H1_T33 | H1_T33 | H1_T22 | 
| Strategy for choosing outgoing interfaces | Lowest cost (SLA) | Lowest cost (SLA) | Best quality, link cost factor: packet loss | 

Throughout this example, transport group 1 is used for VPN overlays over Internet links while transport group 2 is used for the VPN overlay over an MPLS link.

In this example, user traffic is initiated behind Spoke 1 and destined to Spoke 2. Because of this, Spoke 1 is considered the local spoke, and Spoke 2 is considered the remote spoke.

This section includes:

- 
                                                    SD-WAN configuration and health check status on Spoke 1:
- 
                                                    SD-WAN configuration and health check status on Spoke 2:

```
config system sdwan
    set status enable
    config zone
        edit "virtual-wan-link"
        next
        edit "overlay"
            
```
**set advpn-select enable
            set advpn-health-check "HUB"**
        next
    end
    config members
        edit 1
            set interface "H1_T11"
            set zone "overlay"
            **set transport-group 1** 
        next
        edit 2
            set interface "H1_T22"
            set zone "overlay"
            **set transport-group 1** 
        next
        edit 3
            set interface "H1_T33"
            set zone "overlay"
            **set transport-group 2** 
        next
    end
    config health-check
        edit "HUB"
            set server "172.31.100.100"
            set members 1 2 3
            config sla
                edit 1
                    set link-cost-factor latency
                    set latency-threshold 100
                next
            end
        next
    end
    config service
        edit 1
            set name "1"
            **set mode sla
            set shortcut-priority enable**
            set dst "spoke-2_LAN-1" "Tunnel_IPs"
            set src "spoke-1_LAN-1" "Tunnel_IPs"
            config sla
                edit "HUB"
                    set id 1
                next
            end
            **set priority-members 1 2 3**
        next
        edit 2
            set name "2"
            **set mode sla
            set shortcut-priority enable**
            set dst "spoke-2_LAN-2" "Tunnel_IPs"
            set src "spoke-1_LAN-1" "Tunnel_IPs"
            config sla
                edit "HUB"
                    set id 1
                next
            end
            **set priority-members 2 1 3**
        next
        edit 3
            set name "3"
            **set mode priority**
            set dst "spoke-2_LAN-3" "Tunnel_IPs"
            set src "spoke-1_LAN-1" "Tunnel_IPs"
            set health-check "HUB"
            **set link-cost-factor packet-loss
            set priority-members 3 1 2**
        next
    end
end
# diagnose sys sdwan health-check
Health Check(HUB):
Seq(1 H1_T11): state(alive), packet-loss(0.000%) latency(0.231), jitter(0.029), mos(4.404), bandwidth-up(999999), bandwidth-dw(999997), bandwidth-bi(1999996) **sla_map=0x1**
Seq(2 H1_T22): state(alive), packet-loss(0.000%) latency(0.193), jitter(0.010), mos(4.404), bandwidth-up(999994), bandwidth-dw(999997), bandwidth-bi(1999991) **sla_map=0x1**
Seq(3 H1_T33): state(alive), packet-loss(0.000%) latency(0.144), jitter(0.007), mos(4.404), bandwidth-up(999999), bandwidth-dw(999997), bandwidth-bi(1999996) **sla_map=0x1**

```
config system sdwan
    set status enable
    config zone
        edit "virtual-wan-link"
        next
        edit "overlay"
            
```
**set advpn-select enable
            set advpn-health-check "HUB"**
        next
    end
    config members
        edit 1
            set interface "H1_T11"
            set zone "overlay"
            **set cost 100
            set transport-group 1** 
        next
        edit 2
            set interface "H1_T22"
            set zone "overlay"
            **set transport-group 1** 
        next
        edit 3
            set interface "H1_T33"
            set zone "overlay"
            **set transport-group 2** 
        next
    end
    config health-check
        edit "HUB"
            set server "172.31.100.100"
            set members 3 1 2
            config sla
                edit 1
                    set link-cost-factor latency
                    set latency-threshold 100
                next
            end
        next
    end
end
# diagnose sys sdwan health-check
Health Check(HUB):
Seq(3 H1_T33): state(alive), packet-loss(0.000%) latency(0.124), jitter(0.009), mos(4.404), bandwidth-up(999999), bandwidth-dw(999998), bandwidth-bi(1999997) **sla_map=0x1**
Seq(1 H1_T11): state(alive), packet-loss(0.000%) latency(0.216), jitter(0.043), mos(4.404), bandwidth-up(999999), bandwidth-dw(999998), bandwidth-bi(1999997) **sla_map=0x1**
Seq(2 H1_T22): state(alive), packet-loss(0.000%) latency(0.184), jitter(0.012), mos(4.404), bandwidth-up(999994), bandwidth-dw(999998), bandwidth-bi(1999992) **sla_map=0x1**

In this scenario, PC1 connected to Spoke 1 initiates an ICMP ping destined for PC1 connected to Spoke 2. Therefore, this user traffic matches SD-WAN rule 1 and triggers shortcut path selection and establishment.

The Path Manager of Spoke 1 calculates the best shortcut path by comparing transport group, link quality (for SLA mode), link cost, and member configuration order between Spoke 1 and Spoke 2.

For an SLA mode service, the following algorithm is used to consider endpoints of the best shortcut path:

1. 
                                                    Overlays with the same transport group
2. 
                                                    In-SLA overlays
3. 
                                                    Lowest link-cost overlays
4. 
                                                    Member configuration order as a final tiebreaker

Based on this algorithm, the Path Manager on Spoke 1 selects Spoke 1 H1_T11 because it is first in the priority-members order for SD-WAN rule 1, it has the lowest link cost, and it is within SLA. Likewise, the Path Manager on Spoke 1 selects Spoke 2 H1_T22 since it has the lowest link cost compared to Spoke 2 H1_T11 (which has a cost of 100), it is within SLA, and has the same transport group as Spoke 1 H1_T11. Therefore, the Path Manager of Spoke 1 calculates the best shortcut path as Spoke 1 H1_T11 to Spoke 2 H1_T22.

The Path Manager will advise IKE to establish the best shortcut and add it to SD-WAN rule 1 as follows:

```
Branch1_FGT# diagnose sys sdwan service4
 Service(1): Address Mode(IPV4) flags=0x4200 use-shortcut-sla use-shortcut
  Tie break: cfg
  Shortcut priority: 1
   Gen(11), TOS(0x0/0x0), Protocol(0): src(1->65535):dst(1->65535), Mode(sla), sla-compare-order
   Member sub interface(4):
     2: seq_num(1), interface(H1_T11):
        1: H1_T11_0(71)
   Members(4):
     
```
**1: Seq_num(1 H1_T11_0 overlay), alive, sla(0x1), gid(0), cfg_order(0), local cost(0), selected**     2: Seq_num(1 H1_T11 overlay), alive, sla(0x1), gid(0), cfg_order(0), local cost(0), selected
     3: Seq_num(2 H1_T22 overlay), alive, sla(0x1), gid(0), cfg_order(1), local cost(0), selected
     4: Seq_num(3 H1_T33 overlay), alive, sla(0x1), gid(0), cfg_order(2), local cost(0), selected
   Src address(2):
         172.31.0.0-172.31.255.255
         10.0.3.0-10.0.3.255
   Dst address(2):
         172.31.0.0-172.31.255.255
         **10.0.4.0-10.0.4.255** ...
                                            Since shortcut-priority is enabled, we observe that the shortcut is formed over the selected overlay path and prioritized over the parent overlay.

From the diagnostic command on Spoke 1, we observe the selected shortcut path in **bold**. (Note that the remote IP matches Spoke 2 H1_T22 in the corresponding table above.)

Branch1_FGT# diagnose sys sdwan advpn-session
 Session head(Branch2_FGT-0-overlay:1)
 **(1) Service ID(1), last access(7809088), remote health check info(3) Selected path: local(H1_T11, port1) gw: 172.31.3.1 remote IP: 172.31.3.105(172.31.81.2)** 
 Remote information:
 1: latency: 0.176267 jitter: 0.005733 pktloss: 0.000000 mos: 4.404302 sla: 0x1 cost: 0 transport_group: 1 bandwidth up: 999994 down: 999997 bidirection: 1999991
 ipv4: 172.31.3.105(172.31.81.2) ipv6 2000:172:31:3::105(::)
 2: latency: 0.119133 jitter: 0.004800 pktloss: 0.000000 mos: 4.404331 sla: 0x1 cost: 0 transport_group: 2 bandwidth up: 999999 down: 999997 bidirection: 1999996
 ipv4: 172.31.4.101(172.31.82.2) ipv6 1410:4b02::f088:93ee:7f00:0(c010:4b02::788a:93ee:7f00:0)
 3: latency: 0.182400 jitter: 0.008800 pktloss: 0.000000 mos: 4.404295 sla: 0x1 cost: 100 transport_group: 1 bandwidth up: 999999 down: 999997 bidirection: 1999996
 ipv4: 172.31.3.101(172.31.80.2) ipv6 2000:172:31:3::101(d88a:93ee:7f00:0:d88a:93ee:7f00:0)

From the diagnostic command on Spoke 2, we observe the selected shortcut in **bold**:

Branch2_FGT# diagnose sys sdwan health-check
 Health Check(HUB):
 Seq(3 H1_T33): state(alive), packet-loss(0.000%) latency(0.122), jitter(0.004), mos(4.404), bandwidth-up(999999), bandwidth-dw(999997), bandwidth-bi(1999996) sla_map=0x1
 Seq(1 H1_T11): state(alive), packet-loss(0.000%) latency(0.186), jitter(0.011), mos(4.404), bandwidth-up(999999), bandwidth-dw(999997), bandwidth-bi(1999996) sla_map=0x1
 Seq(2 H1_T22): state(alive), packet-loss(0.000%) latency(0.180), jitter(0.005), mos(4.404), bandwidth-up(999994), bandwidth-dw(999997), bandwidth-bi(1999991) sla_map=0x1
 **Seq(2 H1_T22_0): state(alive), packet-loss(0.000%) latency(0.265), jitter(0.011), mos(4.404), bandwidth-up(999999), bandwidth-dw(999999), bandwidth-bi(1999998) sla_map=0x1**

In this scenario, PC1 connected to Spoke 1 initiates an ICMP ping destined for PC2 connected to Spoke 2. Therefore, this user traffic matches SD-WAN rule 2, and traffic will go through shortcut H1_T11_0 of Spoke 1 previously established in Scenario 1 above.

The local spoke sends a shortcut-query to the remote spoke to trigger a shortcut after ADVPN 2.0 path management makes a path decision with updated remote spoke WAN link information, which is received periodically (5 seconds) on the established shortcuts.

For an SLA mode service, the following algorithm is followed for considering endpoints of the best shortcut path:

1. 
                                                    Overlays with the same transport group
2. 
                                                    In-SLA overlays
3. 
                                                    Lowest link-cost overlays
4. 
                                                    Member configuration order as a final tiebreaker

Based on this algorithm, the Path Manager on Spoke 1 selects Spoke 1 H1_T22 because it is the first in the priority-members order for SD-WAN rule 2, it has the lowest link cost, and it is within SLA. Likewise, the Path Manager on Spoke 1 selects Spoke 2 H1_T22 since it has the lowest link cost compared to Spoke 2 H1_T11 (which has a cost of 100), it is within SLA, and has the same transport group as Spoke 1 H1_T11. Therefore, the Path Manager of Spoke 1 calculates the best shortcut path as Spoke 1 H1_T22 to Spoke 2 H1_T22.

The Path Manager will advise IKE to establish the best shortcut and add it to SD-WAN rule 2 as follows:

```
Branch1_FGT# diagnose sys sdwan service4
...
Service(2): Address Mode(IPV4) flags=0x4200 use-shortcut-sla use-shortcut
 Tie break: cfg
 Shortcut priority: 1
  Gen(12), TOS(0x0/0x0), Protocol(0): src(1->65535):dst(1->65535), Mode(sla), sla-compare-order
  Member sub interface(5):
    3: seq_num(2), interface(H1_T22):
       1: H1_T22_0(72)
    4: seq_num(1), interface(H1_T11):
       1: H1_T11_0(71)
  Members(5):
    
```
**1: Seq_num(2 H1_T22_0 overlay), alive, sla(0x1), gid(0), cfg_order(0), local cost(0), selected**
    2: Seq_num(1 H1_T11_0 overlay), alive, sla(0x1), gid(0), cfg_order(1), local cost(0), selected, last_used=2023-12-05 14:34:07
    3: Seq_num(2 H1_T22 overlay), alive, sla(0x1), gid(0), cfg_order(0), local cost(0), selected
    4: Seq_num(1 H1_T11 overlay), alive, sla(0x1), gid(0), cfg_order(1), local cost(0), selected
    5: Seq_num(3 H1_T33 overlay), alive, sla(0x1), gid(0), cfg_order(2), local cost(0), selected
  Src address(2):
        172.31.0.0-172.31.255.255
        10.0.3.0-10.0.3.255
  Dst address(2):
        172.31.0.0-172.31.255.255
        **10.0.40.0-10.0.40.255**
...
                                            The newly selected shortcut is prioritized over the previously selected shortcut as seen in the **bolded** output above.

From the diagnostic command on Spoke 1, we observe the selected shortcut path in **bold**. (Note that the remote IP matches Spoke 2 H1_T22 in the corresponding table above.)

Branch1_FGT# diagnose sys sdwan advpn-session
Session head(Branch2_FGT-0-overlay:2)
(1) Service ID(1), last access(8024725), remote health check info(3)
Selected path: local(H1_T11, port1) gw: 172.31.3.1  remote IP: 172.31.3.105(172.31.81.2)
Remote information:
1: latency: 0.118267 jitter: 0.004633 pktloss: 0.000000 mos: 4.404331 sla: 0x1 cost: 0 transport_group: 2 bandwidth up: 999999 down: 999997 bidirection: 1999996
ipv4: 172.31.4.101(172.31.82.2) ipv6 180:adfb::d88a:93ee:7f00:0(d88a:93ee:7f00:0:d88a:93ee:7f00:0)
2: latency: 0.176067 jitter: 0.006567 pktloss: 0.000000 mos: 4.404301 sla: 0x1 cost: 0 transport_group: 1 bandwidth up: 999994 down: 999997 bidirection: 1999991
ipv4: 172.31.3.105(172.31.81.2) ipv6 2000:172:31:3::105(::)
3: latency: 0.170333 jitter: 0.008133 pktloss: 0.000000 mos: 4.404302 sla: 0x1 cost: 100 transport_group: 1 bandwidth up: 999999 down: 999997 bidirection: 1999996
ipv4: 172.31.3.101(172.31.80.2) ipv6 2000:172:31:3::101(c010:4b02::788a:93ee:7f00:0)
**(1) Service ID(2), last access(8024725), remote health check info(3)
Selected path: local(H1_T22, port2) gw: 172.31.3.5  remote IP: 172.31.3.105(172.31.81.2)** 
Remote information:
1: latency: 0.118267 jitter: 0.004633 pktloss: 0.000000 mos: 4.404331 sla: 0x1 cost: 0 transport_group: 2 bandwidth up: 999999 down: 999997 bidirection: 1999996
ipv4: 172.31.4.101(172.31.82.2) ipv6 180:adfb::d88a:93ee:7f00:0(d88a:93ee:7f00:0:d88a:93ee:7f00:0)
2: latency: 0.176067 jitter: 0.006567 pktloss: 0.000000 mos: 4.404301 sla: 0x1 cost: 0 transport_group: 1 bandwidth up: 999994 down: 999997 bidirection: 1999991
ipv4: 172.31.3.105(172.31.81.2) ipv6 2000:172:31:3::105(::)
3: latency: 0.170333 jitter: 0.008133 pktloss: 0.000000 mos: 4.404302 sla: 0x1 cost: 100 transport_group: 1 bandwidth up: 999999 down: 999997 bidirection: 1999996
ipv4: 172.31.3.101(172.31.80.2) ipv6 2000:172:31:3::101(c010:4b02::788a:93ee:7f00:0)
...

From the diagnostic command on Spoke 2, we observe the selected shortcut in **bold**:

Branch2_FGT# diagnose sys sdwan health-check
Health Check(HUB):
Seq(3 H1_T33): state(alive), packet-loss(0.000%) latency(0.118), jitter(0.005), mos(4.404), bandwidth-up(999999), bandwidth-dw(999998), bandwidth-bi(1999997) sla_map=0x1
Seq(1 H1_T11): state(alive), packet-loss(0.000%) latency(0.171), jitter(0.005), mos(4.404), bandwidth-up(999999), bandwidth-dw(999998), bandwidth-bi(1999997) sla_map=0x1
Seq(2 H1_T22): state(alive), packet-loss(0.000%) latency(0.175), jitter(0.006), mos(4.404), bandwidth-up(999994), bandwidth-dw(999998), bandwidth-bi(1999992) sla_map=0x1
Seq(2 H1_T22_0): state(alive), packet-loss(0.000%) latency(0.240), jitter(0.009), mos(4.404), bandwidth-up(1000000), bandwidth-dw(1000000), bandwidth-bi(2000000) sla_map=0x1
**Seq(2 H1_T22_1): state(alive), packet-loss(0.000%) latency(0.259), jitter(0.019), mos(4.404), bandwidth-up(1000000), bandwidth-dw(1000000), bandwidth-bi(2000000) sla_map=0x1**

In this scenario, PC1 connected to Spoke 1 initiates an ICMP ping destined for PC3 connected to Spoke 2. Therefore, this user traffic matches SD-WAN rule 3, and traffic will go through shortcut H1_T11_0 of Spoke 1 previously established in Scenario 1 above.

The local spoke sends a shortcut-query to the remote spoke to trigger a shortcut after ADVPN 2.0 path management makes a path decision with updated remote spoke WAN link information, which is received periodically (5 seconds) on the established shortcuts.

For a best quality mode service, the following algorithm is followed for considering endpoints of the best shortcut path:

1. 
                                                    Overlays with the same transport group
2. 
                                                    Best quality overlays (link cost factor of packet loss, in this scenario)
3. 
                                                    Member configuration order as a final tiebreaker

Based on this algorithm, the Path Manager on Spoke 1 selects Spoke 1 H1_T33 because it is the first in the priority-members order for SD-WAN rule 3, and it has the best quality link. Likewise, the Path Manager on Spoke 1 selects Spoke 2 H1_T33 since it has the same transport group as Spoke 1 H1_T33. Therefore, the Path Manager of Spoke 1 calculates the best shortcut path as Spoke 1 H1_T33 to Spoke 2 H1_T33.

The Path Manager will advise IKE to establish the best shortcut and add it to SD-WAN rule 3 as follows:

```
Branch1_FGT# diagnose sys sdwan service4
...
Service(3): Address Mode(IPV4) flags=0x4200 use-shortcut-sla use-shortcut
 Tie break: cfg
 Shortcut priority: 3
  Gen(13), TOS(0x0/0x0), Protocol(0): src(1->65535):dst(1->65535), Mode(priority), link-cost-factor(packet-loss), link-cost-threshold(10), heath-check(HUB)
  Member sub interface(6):
    4: seq_num(3), interface(H1_T33):
       1: H1_T33_0(73)
    5: seq_num(1), interface(H1_T11):
       1: H1_T11_0(71)
    6: seq_num(2), interface(H1_T22):
       1: H1_T22_0(72)
  Members(6):
    
```
**1: Seq_num(3 H1_T33_0 overlay), alive, packet loss: 0.000%, selected**
    2: Seq_num(1 H1_T11_0 overlay), alive, packet loss: 0.000%, selected, last_used=2023-12-05 14:38:02
    3: Seq_num(2 H1_T22_0 overlay), alive, packet loss: 0.000%, selected
    4: Seq_num(3 H1_T33 overlay), alive, packet loss: 0.000%, selected
    5: Seq_num(1 H1_T11 overlay), alive, packet loss: 0.000%, selected
    6: Seq_num(2 H1_T22 overlay), alive, packet loss: 0.000%, selected
  Src address(2):
        172.31.0.0-172.31.255.255
        10.0.3.0-10.0.3.255
  Dst address(2):
        172.31.0.0-172.31.255.255
        **10.0.41.0-10.0.41.255**
                                            From the diagnostic command on Spoke 1, we observe the selected shortcut path in **bold**. (Note that the remote IP matches Spoke 2 H1_T33 in the corresponding table above.)

Branch1_FGT# diagnose sys sdwan advpn-session
Session head(Branch2_FGT-0-overlay:3)
**(1) Service ID(3), last access(8047297), remote health check info(3)
Selected path: local(H1_T33, port3) gw: 172.31.4.1  remote IP: 172.31.4.101(172.31.82.2)** 
Remote information:
1: latency: 0.116600 jitter: 0.004600 pktloss: 0.000000 mos: 4.404332 sla: 0x1 cost: 0 transport_group: 2 bandwidth up: 999999 down: 999998 bidirection: 1999997
ipv4: 172.31.4.101(172.31.82.2) ipv6 180:adfb::d88a:93ee:7f00:0(d88a:93ee:7f00:0:d88a:93ee:7f00:0)
2: latency: 0.174767 jitter: 0.005533 pktloss: 0.000000 mos: 4.404303 sla: 0x1 cost: 0 transport_group: 1 bandwidth up: 999994 down: 999998 bidirection: 1999992
ipv4: 172.31.3.105(172.31.81.2) ipv6 2000:172:31:3::105(c010:4b02::788a:93ee:7f00:0)
3: latency: 0.172900 jitter: 0.005167 pktloss: 0.000000 mos: 4.404304 sla: 0x1 cost: 100 transport_group: 1 bandwidth up: 999999 down: 999998 bidirection: 1999997
ipv4: 172.31.3.101(172.31.80.2) ipv6 2000:172:31:3::101(::)

From the diagnostic command on Spoke 2, we observe the selected shortcut in **bold**:

Branch2_FGT# diagnose sys sdwan  health-check
Health Check(HUB):
Seq(3 H1_T33): state(alive), packet-loss(0.000%) latency(0.116), jitter(0.005), mos(4.404), bandwidth-up(999999), bandwidth-dw(999998), bandwidth-bi(1999997) sla_map=0x1
**Seq(3 H1_T33_0): state(alive), packet-loss(0.000%) latency(0.113), jitter(0.005), mos(4.404), bandwidth-up(1000000), bandwidth-dw(1000000), bandwidth-bi(2000000) sla_map=0x1**
Seq(1 H1_T11): state(alive), packet-loss(0.000%) latency(0.171), jitter(0.004), mos(4.404), bandwidth-up(999999), bandwidth-dw(999998), bandwidth-bi(1999997) sla_map=0x1
Seq(2 H1_T22): state(alive), packet-loss(0.000%) latency(0.174), jitter(0.008), mos(4.404), bandwidth-up(999994), bandwidth-dw(999998), bandwidth-bi(1999992) sla_map=0x1
Seq(2 H1_T22_0): state(alive), packet-loss(0.000%) latency(0.239), jitter(0.007), mos(4.404), bandwidth-up(999999), bandwidth-dw(999999), bandwidth-bi(1999998) sla_map=0x1
Seq(2 H1_T22_1): state(alive), packet-loss(0.000%) latency(0.260), jitter(0.014), mos(4.404), bandwidth-up(1000000), bandwidth-dw(1000000), bandwidth-bi(2000000) sla_map=0x1

In this scenario, we place remote Spoke 2 H1_T22 out-of-SLA and observe that this link quality change is sensed by the local spoke through regular WAN link information updates on shortcuts. Because service 1 and 2 are the only rules that have new best shortcut paths when Spoke 2 H1_T22 is out-of-SLA, the local Spoke 1 will directly send shortcut-queries to the remote Spoke 2 to trigger shortcuts for service 1 and 2 after ADVPN 2.0 path management makes path decisions with updated remote spoke WAN link information.

For an SLA mode service, the following algorithm is followed for considering endpoints of the best shortcut path:

1. 
                                                    Overlays with the same transport group
2. 
                                                    In-SLA overlays
3. 
                                                    Lowest link-cost overlays
4. 
                                                    Member configuration order as a final tiebreaker

Based on this algorithm, the Path Manager on Spoke 1 still selects these Spoke 1 interfaces:

- 
                                                    SD-WAN Rule 1: H1_T11
- 
                                                    SD-WAN Rule 2: H1_T22

These are the first in the priority-members order for SD-WAN rules 1 and 2, respectively.

Based on the updated WAN link information, the Path Manager on Spoke 1 selects these Spoke 2 interfaces because they are the only remaining in-SLA VPN overlays over Internet links (transport group 1):

- 
                                                    SD-WAN Rule 1: H1_T11
- 
                                                    SD-WAN Rule 2: H1_T11

Therefore, the Path Manager of Spoke 1 calculates the best shortcut paths as follows:

- 
                                                    SD-WAN Rule 1: Spoke 1 H1_T11 to Spoke 2 H1_T11
- 
                                                    SD-WAN Rule 2: Spoke 1 H1_T22 to Spoke 2 H1_T11

The Path Manager will advise IKE to establish the best shortcuts and add them to SD-WAN rules 1 and 2 as follows:

- 
                                                    For SD-WAN Rule 1, H1_T11_1 is the new best shortcut.
- 
                                                    For SD-WAN Rule 2, H1_T22_1 is the new best shortcut.

```
# diagnose sys sdwan service4
Service(1): Address Mode(IPV4) flags=0x4200 use-shortcut-sla use-shortcut
 Tie break: cfg
 Shortcut priority: 1
  Gen(17), TOS(0x0/0x0), Protocol(0): src(1->65535):dst(1->65535), Mode(sla), sla-compare-order
  Member sub interface(8):
    6: seq_num(1), interface(H1_T11):
       1: H1_T11_0(74)
       2: H1_T11_1(75)
    7: seq_num(2), interface(H1_T22):
       1: H1_T22_0(72)
       2: H1_T22_1(76)
    8: seq_num(3), interface(H1_T33):
       1: H1_T33_0(73)
  Members(8):
    1: Seq_num(1 H1_T11_0 overlay), alive, sla(0x1), gid(0), cfg_order(0), local cost(0), selected
    
```
**2: Seq_num(1 H1_T11_1 overlay), alive, sla(0x1), gid(0), cfg_order(0), local cost(0), selected**                 
    3: Seq_num(2 H1_T22_0 overlay), alive, sla(0x1), gid(0), cfg_order(1), local cost(0), selected
    **4: Seq_num(2 H1_T22_1 overlay), alive, sla(0x1), gid(0), cfg_order(1), local cost(0), selected**        
    5: Seq_num(3 H1_T33_0 overlay), alive, sla(0x1), gid(0), cfg_order(2), local cost(0), selected
    6: Seq_num(1 H1_T11 overlay), alive, sla(0x1), gid(0), cfg_order(0), local cost(0), selected
    7: Seq_num(2 H1_T22 overlay), alive, sla(0x1), gid(0), cfg_order(1), local cost(0), selected
    8: Seq_num(3 H1_T33 overlay), alive, sla(0x1), gid(0), cfg_order(2), local cost(0), selected
  Src address(2):
        172.31.0.0-172.31.255.255
        10.0.3.0-10.0.3.255
  Dst address(2):
        172.31.0.0-172.31.255.255
        10.0.4.0-10.0.4.255
Service(2): Address Mode(IPV4) flags=0x4200 use-shortcut-sla use-shortcut
 Tie break: cfg
 Shortcut priority: 1
  Gen(17), TOS(0x0/0x0), Protocol(0): src(1->65535):dst(1->65535), Mode(sla), sla-compare-order
  Member sub interface(8):
    6: seq_num(2), interface(H1_T22):
       1: H1_T22_0(72)
       2: H1_T22_1(76)
    7: seq_num(1), interface(H1_T11):
       1: H1_T11_0(74)
       2: H1_T11_1(75)
    8: seq_num(3), interface(H1_T33):
       1: H1_T33_0(73)
  Members(8):
    1: Seq_num(2 H1_T22_0 overlay), alive, sla(0x1), gid(0), cfg_order(0), local cost(0), selected
    **2: Seq_num(2 H1_T22_1 overlay), alive, sla(0x1), gid(0), cfg_order(0), local cost(0), selected
    3: Seq_num(1 H1_T11_1 overlay), alive, sla(0x1), gid(0), cfg_order(1), local cost(0), selected**
    4: Seq_num(1 H1_T11_0 overlay), alive, sla(0x1), gid(0), cfg_order(1), local cost(0), selected
    5: Seq_num(3 H1_T33_0 overlay), alive, sla(0x1), gid(0), cfg_order(2), local cost(0), selected
    6: Seq_num(2 H1_T22 overlay), alive, sla(0x1), gid(0), cfg_order(0), local cost(0), selected
    7: Seq_num(1 H1_T11 overlay), alive, sla(0x1), gid(0), cfg_order(1), local cost(0), selected
    8: Seq_num(3 H1_T33 overlay), alive, sla(0x1), gid(0), cfg_order(2), local cost(0), selected
  Src address(2):
        172.31.0.0-172.31.255.255
        10.0.3.0-10.0.3.255
  Dst address(2):
        172.31.0.0-172.31.255.255
        10.0.40.0-10.0.40.255
...
                                            From the diagnostic command on Spoke 1, we observe the newly selected shortcut paths in **bold**. (Note that the remote IP 172.31.80.2 matches Spoke 2 H1_T11, which is the VPN overlay over the Internet link with cost 100 in the corresponding table above.)

# diagnose sys sdwan advpn-session
Session head(Branch2_FGT-0-overlay:3)
**(1) Service ID(1), last access(8293060), remote health check info(3)
Selected path: local(H1_T11, port1) gw: 172.31.3.1  remote IP: 172.31.3.101(172.31.80.2)**
Remote information:
1: latency: 0.119500 jitter: 0.006067 pktloss: 0.000000 mos: 4.404329 sla: 0x1 cost: 0 transport_group: 2 bandwidth up: 999999 down: 999997 bidirection: 1999996
ipv4: 172.31.4.101(172.31.82.2) ipv6 180:adfb::d88a:93ee:7f00:0(d88a:93ee:7f00:0:d88a:93ee:7f00:0)
2: latency: 250.170761 jitter: 0.011500 pktloss: 0.000000 mos: 3.992655 sla: 0x0 cost: 0 transport_group: 1 bandwidth up: 999994 down: 999997 bidirection: 1999991
ipv4: 172.31.3.105(172.31.81.2) ipv6 2000:172:31:3::105(c010:4b02::788a:93ee:7f00:0)
3: latency: 0.182200 jitter: 0.012000 pktloss: 0.000000 mos: 4.404292 sla: 0x1 cost: 100 transport_group: 1 bandwidth up: 999999 down: 999997 bidirection: 1999996
ipv4: 172.31.3.101(172.31.80.2) ipv6 2000:172:31:3::101(::)
**(1) Service ID(2), last access(8293060), remote health check info(3)
Selected path: local(H1_T22, port2) gw: 172.31.3.5  remote IP: 172.31.3.101(172.31.80.2)**
Remote information:
1: latency: 0.119500 jitter: 0.006067 pktloss: 0.000000 mos: 4.404329 sla: 0x1 cost: 0 transport_group: 2 bandwidth up: 999999 down: 999997 bidirection: 1999996
ipv4: 172.31.4.101(172.31.82.2) ipv6 180:adfb::d88a:93ee:7f00:0(d88a:93ee:7f00:0:d88a:93ee:7f00:0)
2: latency: 250.170761 jitter: 0.011500 pktloss: 0.000000 mos: 3.992655 sla: 0x0 cost: 0 transport_group: 1 bandwidth up: 999994 down: 999997 bidirection: 1999991
ipv4: 172.31.3.105(172.31.81.2) ipv6 2000:172:31:3::105(c010:4b02::788a:93ee:7f00:0)
3: latency: 0.182200 jitter: 0.012000 pktloss: 0.000000 mos: 4.404292 sla: 0x1 cost: 100 transport_group: 1 bandwidth up: 999999 down: 999997 bidirection: 1999996
ipv4: 172.31.3.101(172.31.80.2) ipv6 2000:172:31:3::101(::)

From the diagnostic command on Spoke 2, we observe the selected shortcuts in **bold**:

Branch2_FGT# diagnose sys sdwan  health-check
Health Check(HUB):
Seq(3 H1_T33): state(alive), packet-loss(0.000%) latency(0.120), jitter(0.007), mos(4.404), bandwidth-up(999999), bandwidth-dw(999997), bandwidth-bi(1999996) sla_map=0x1
Seq(3 H1_T33_0): state(alive), packet-loss(0.000%) latency(0.128), jitter(0.003), mos(4.404), bandwidth-up(1000000), bandwidth-dw(1000000), bandwidth-bi(2000000) sla_map=0x1
Seq(1 H1_T11): state(alive), packet-loss(0.000%) latency(0.180), jitter(0.008), mos(4.404), bandwidth-up(999999), bandwidth-dw(999997), bandwidth-bi(1999996) sla_map=0x1
**Seq(1 H1_T11_0): state(alive), packet-loss(0.000%) latency(0.259), jitter(0.023), mos(4.404), bandwidth-up(1000000), bandwidth-dw(1000000), bandwidth-bi(2000000) sla_map=0x1
Seq(1 H1_T11_1): state(alive), packet-loss(0.000%) latency(0.257), jitter(0.014), mos(4.404), bandwidth-up(1000000), bandwidth-dw(1000000), bandwidth-bi(2000000) sla_map=0x1
Seq(2 H1_T22): state(alive), packet-loss(0.000%) latency(250.169), jitter(0.009), mos(3.993), bandwidth-up(999994), bandwidth-dw(999997), bandwidth-bi(1999991) sla_map=0x0**
Seq(2 H1_T22_1): state(alive), packet-loss(0.000%) latency(0.245), jitter(0.013), mos(4.404), bandwidth-up(1000000), bandwidth-dw(1000000), bandwidth-bi(2000000) sla_map=0x1
Seq(2 H1_T22_0): state(alive), packet-loss(0.000%) latency(0.223), jitter(0.005), mos(4.404), bandwidth-up(1000000), bandwidth-dw(1000000), bandwidth-bi(2000000) sla_map=0x1

This example relies on the same Network Topology used above, except Spoke 1 has a single SD-WAN rule/service that uses the load balancing strategy with SLA targets. For details, see Load balancing strategy with SLA targets.

This section shows the SD-WAN configuration, selected IPsec configuration, and health check status on Spoke 1: and Spoke 2::

```
config system sdwan
    set status enable
    config zone
        edit "virtual-wan-link"
        next
        edit "overlay"
            
```
**set advpn-select enable 
            set advpn-health-check "HUB"**
        next
    end
    config members
        edit 1
            set interface "H1_T11"
            set zone "overlay"
            **set transport-group 1** 
        next
        edit 2
            set interface "H1_T22"
            set zone "overlay"
            **set transport-group 1** 
        next
        edit 3
            set interface "H1_T33"
            set zone "overlay"
            **set transport-group 2** 
        next
    end
    config health-check
        edit "HUB"
            set server "172.31.100.100"
            set members 1 2 3
            config sla
                edit 1
                    set link-cost-factor latency
                    set latency-threshold 100
                next
            end
        next
    end
    config service
        edit 1
            set name "1"
            **set load-balance enable            
            set mode sla**
            set dst "CORP_LAN"
            set src "CORP_LAN"
            config sla
                edit "HUB"
                    set id 1
                next
            end
            **set priority-members 1 2 3**
        next
    end
end
config vpn ipsec phase1-interface
    edit "H1_T11"
        ...
        set idle-timeout enable
        **set shared-idle-timeout enable**
        set idle-timeoutinterval 5
        ...
    next
end
config vpn ipsec phase1-interface
    edit "H1_T22"
        ...
        set idle-timeout enable
        **set shared-idle-timeout enable**
        set idle-timeoutinterval 5                            
        ...
    next
end
config vpn ipsec phase1-interface
    edit "H1_T33"
        ...
        set idle-timeout enable
        **set shared-idle-timeout enable**
        set idle-timeoutinterval 5
        ...
    next
end # diagnose sys sdwan health-check 
Health Check(HUB):
Seq(1 H1_T11): state(alive), packet-loss(0.000%) latency(0.223), jitter(0.018), mos(4.404), bandwidth-up(999999), bandwidth-dw(999998), bandwidth-bi(1999997) **sla_map=0x1**
Seq(2 H1_T22): state(alive), packet-loss(0.000%) latency(0.191), jitter(0.009), mos(4.404), bandwidth-up(999993), bandwidth-dw(999998), bandwidth-bi(1999991) **sla_map=0x1**
Seq(3 H1_T33): state(alive), packet-loss(0.000%) latency(0.139), jitter(0.007), mos(4.404), bandwidth-up(999999), bandwidth-dw(999998), bandwidth-bi(1999997) **sla_map=0x1**

```
config system sdwan
    set status enable
    config zone
        edit "virtual-wan-link"
        next
        edit "overlay"
            
```
**set advpn-select enable 
            set advpn-health-check "HUB"**
        next
    end
    config members
        edit 1
            set interface "H1_T11"
            set zone "overlay"
            **set transport-group 1** 
        next
        edit 2
            set interface "H1_T22"
            set zone "overlay"
            **set transport-group 1** 
        next
        edit 3
            set interface "H1_T33"
            set zone "overlay"
            **set transport-group 2** 
        next
    end
    config health-check
        edit "HUB"
            set server "172.31.100.100"
            set members 3 1 2
            config sla
                edit 1
                    set link-cost-factor latency
                    set latency-threshold 100
                next
            end
        next
    end
end
config vpn ipsec phase1-interface
    edit "H1_T11"
        ...
        set idle-timeout enable
        **set shared-idle-timeout enable**
        set idle-timeoutinterval 5
        ...
    next
end
config vpn ipsec phase1-interface
    edit "H1_T22"
        ...
        set idle-timeout enable
        **set shared-idle-timeout enable**
        set idle-timeoutinterval 5                            
        ...
    next
end
config vpn ipsec phase1-interface
    edit "H1_T33"
        ...
        set idle-timeout enable
        **set shared-idle-timeout enable**
        set idle-timeoutinterval 5
        ...
    next
end
# diagnose sys sdwan health-check 
Health Check(HUB):
Seq(3 H1_T33): state(alive), packet-loss(0.000%) latency(0.148), jitter(0.021), mos(4.404), bandwidth-up(999999), bandwidth-dw(999998), bandwidth-bi(1999997) **sla_map=0x1**
Seq(1 H1_T11): state(alive), packet-loss(0.000%) latency(0.183), jitter(0.010), mos(4.404), bandwidth-up(999999), bandwidth-dw(999998), bandwidth-bi(1999997) **sla_map=0x1**
Seq(2 H1_T22): state(alive), packet-loss(0.000%) latency(0.163), jitter(0.005), mos(4.404), bandwidth-up(999994), bandwidth-dw(999998), bandwidth-bi(1999992) **sla_map=0x1**

In this scenario, PC1 connected to Spoke 1 initiates an ICMP ping destined for PC1 connected to Spoke 2. Therefore, this user traffic matches SD-WAN rule 1 and triggers shortcut path selection and establishment.

On Spoke 1, in the IKE debug (diagnose debug application ike -1), debug messages indicate that multiple direct shortcut-query packets are being sent to Spoke 2:

ike :VWL_ADVPN_MSG_T_TRIGGER
ike V=root:0 looking up shortcut by addr 172.31.80.2, resp-name:H1_T11, name H1_T22, peer-addr 172.31.3.101:0
ike V=root:0:H1_T22: send shortcut-query
...
ike :VWL_ADVPN_MSG_T_TRIGGER
ike V=root:0 looking up shortcut by addr 172.31.81.2, resp-name:H1_T22, name H1_T22, peer-addr 172.31.3.105:0
ike V=root:0:H1_T22: send shortcut-query
...
ike :VWL_ADVPN_MSG_T_TRIGGER
ike V=root:0 looking up shortcut by addr 172.31.82.2, resp-name:H1_T33, name H1_T33, peer-addr 172.31.4.101:0
ike V=root:0:H1_T33: send shortcut-query
...

From the diagnostic command on Spoke 1, observe that multiple shortcuts are triggered in **bold** based on the ADVPN 2.0 path management calculation where in-SLA overlays within the same transport group were selected.

```
Branch1_FGT# diagnose system sdwan service4
Service(1): Address Mode(IPV4) flags=0x24200 use-shortcut-sla use-shortcut
 Tie break: cfg
 Shortcut priority: 3
  Gen(69), TOS(0x0/0x0), Protocol(0): src(1->65535):dst(1->65535), Mode(sla  hash-mode=round-robin)
  Member sub interface(8):
    1: seq_num(1), interface(H1_T11):
       1: H1_T11_0(103)
       2: H1_T11_1(104)
    2: seq_num(2), interface(H1_T22):
       1: H1_T22_0(105)
       2: H1_T22_1(106)
    3: seq_num(3), interface(H1_T33):
       1: H1_T33_0(100)
  Members(8):
    1: Seq_num(1 H1_T11 overlay), alive, sla(0x1), gid(2), num of pass(1), selected
    2: Seq_num(2 H1_T22 overlay), alive, sla(0x1), gid(2), num of pass(1), selected
    3: Seq_num(3 H1_T33 overlay), alive, sla(0x1), gid(2), num of pass(1), selected
    
```
**4: Seq_num(3 H1_T33_0 overlay), alive, sla(0x1), gid(2), num of pass(1), selected
    5: Seq_num(1 H1_T11_0 overlay), alive, sla(0x1), gid(2), num of pass(1), selected
    6: Seq_num(1 H1_T11_1 overlay), alive, sla(0x1), gid(2), num of pass(1), selected
    7: Seq_num(2 H1_T22_0 overlay), alive, sla(0x1), gid(2), num of pass(1), selected
    8: Seq_num(2 H1_T22_1 overlay), alive, sla(0x1), gid(2), num of pass(1), selected**
  Src address(1):
        10.0.0.0-10.255.255.255
  Dst address(1):
        10.0.0.0-10.255.255.255
                                            From the diagnostic command on Spoke 2, observe the shortcuts in **bold**:

Branch2_FGT# diagnose sys sdwan  health-check
 Health Check(HUB):
 Seq(3 H1_T33): state(alive), packet-loss(0.000%) latency(0.120), jitter(0.007), mos(4.404), bandwidth-up(999999), bandwidth-dw(999997), bandwidth-bi(1999996) sla_map=0x1
 Seq(3 H1_T33_0): state(alive), packet-loss(0.000%) latency(0.128), jitter(0.003), mos(4.404), bandwidth-up(1000000), bandwidth-dw(1000000), bandwidth-bi(2000000) sla_map=0x1
 Seq(1 H1_T11): state(alive), packet-loss(0.000%) latency(0.180), jitter(0.008), mos(4.404), bandwidth-up(999999), bandwidth-dw(999997), bandwidth-bi(1999996) sla_map=0x1
 **Seq(1 H1_T11_0): state(alive), packet-loss(0.000%) latency(0.259), jitter(0.023), mos(4.404), bandwidth-up(1000000), bandwidth-dw(1000000), bandwidth-bi(2000000) sla_map=0x1 
 Seq(1 H1_T11_1): state(alive), packet-loss(0.000%) latency(0.257), jitter(0.014), mos(4.404), bandwidth-up(1000000), bandwidth-dw(1000000), bandwidth-bi(2000000) sla_map=0x1 
 Seq(2 H1_T22): state(alive), packet-loss(0.000%) latency(250.169), jitter(0.009), mos(3.993), bandwidth-up(999994), bandwidth-dw(999997), bandwidth-bi(1999991) sla_map=0x0** 
 Seq(2 H1_T22_1): state(alive), packet-loss(0.000%) latency(0.245), jitter(0.013), mos(4.404), bandwidth-up(1000000), bandwidth-dw(1000000), bandwidth-bi(2000000) sla_map=0x1
 Seq(2 H1_T22_0): state(alive), packet-loss(0.000%) latency(0.223), jitter(0.005), mos(4.404), bandwidth-up(1000000), bandwidth-dw(1000000), bandwidth-bi(2000000) sla_map=0x1 

At this point, PC1 connected to Spoke 1 initiated multiple ICMP pings destined for PC1 connected to Spoke 2. The packet capture diagnostic command on Spoke 1 demonstrates that these ICMP pings have been load balanced over all shortcuts:

Branch1_FGT# diagnose sniffer packet any 'host 10.0.4.2' 4
interfaces=[any]
filters=[host 10.0.4.2]
3.481994 port4 in 10.0.3.2 -> 10.0.4.2: icmp: echo request
3.482103 **H1_T11_1** out 10.0.3.2 -> 10.0.4.2: icmp: echo request
3.482799 **H1_T11_1** in 10.0.4.2 -> 10.0.3.2: icmp: echo reply
3.482928 port4 out 10.0.4.2 -> 10.0.3.2: icmp: echo reply
4.614480 port4 in 10.0.3.2 -> 10.0.4.2: icmp: echo request
4.614580 **H1_T33_0** out 10.0.3.2 -> 10.0.4.2: icmp: echo request
4.615122 **H1_T33_0** in 10.0.4.2 -> 10.0.3.2: icmp: echo reply
4.615152 port4 out 10.0.4.2 -> 10.0.3.2: icmp: echo reply
5.286394 port4 in 10.0.3.2 -> 10.0.4.2: icmp: echo request
5.286497 **H1_T22_0** out 10.0.3.2 -> 10.0.4.2: icmp: echo request
5.287129 **H1_T22_0** in 10.0.4.2 -> 10.0.3.2: icmp: echo reply
5.287155 port4 out 10.0.4.2 -> 10.0.3.2: icmp: echo reply
6.079759 port4 in 10.0.3.2 -> 10.0.4.2: icmp: echo request
6.079883 **H1_T22_1** out 10.0.3.2 -> 10.0.4.2: icmp: echo request
6.080496 **H1_T22_1** in 10.0.4.2 -> 10.0.3.2: icmp: echo reply
6.080537 port4 out 10.0.4.2 -> 10.0.3.2: icmp: echo reply
7.983357 port4 in 10.0.3.2 -> 10.0.4.2: icmp: echo request
7.983447 **H1_T11_0** out 10.0.3.2 -> 10.0.4.2: icmp: echo request
7.984078 **H1_T11_0** in 10.0.4.2 -> 10.0.3.2: icmp: echo reply
7.984120 port4 out 10.0.4.2 -> 10.0.3.2: icmp: echo reply

Without user traffic traversing the shortcut during the idle interval time, from the diagnostic command on Spoke 1, observe that all shortcuts have been removed:

```
Branch1_FGT# diagnose system sdwan service4
Service(1): Address Mode(IPV4) flags=0x24200 use-shortcut-sla use-shortcut
 Tie break: cfg
 Shortcut priority: 3
  Gen(16), TOS(0x0/0x0), Protocol(0): src(1->65535):dst(1->65535), Mode(sla  hash-mode=round-robin)
  Members(3):
    1: Seq_num(1 H1_T11 overlay), alive, sla(0x1), gid(2), num of pass(1), selected
    2: Seq_num(2 H1_T22 overlay), alive, sla(0x1), gid(2), num of pass(1), selected
    3: Seq_num(3 H1_T33 overlay), alive, sla(0x1), gid(2), num of pass(1), selected
  Src address(1):
        10.0.0.0-10.255.255.255
  Dst address(1):
        10.0.0.0-10.255.255.255
```