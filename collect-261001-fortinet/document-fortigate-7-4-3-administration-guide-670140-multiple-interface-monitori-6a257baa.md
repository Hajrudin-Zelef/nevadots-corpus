Multiple interface monitoring for IPsec
Multiple interface monitoring for IPsec
IPsec can monitor multiple interfaces per tunnel, and activate a backup link only when all of the primary links are down. This can be useful if you have multiple WAN links and want to optimize your WAN link selection and performance while limiting the use of more expensive and bandwidth intensive interfaces, like 5G or LTE.
In cases where multiple primary overlays are deployed and the backup overlay is on an LTE connection, avoiding IPsec keep alive messages, BGP hellos, and SD-WAN health checks on the backup connection is required when the primary overlays are working. The backup overlay can monitor all of the primary overlays, and is not activated until the number of unhealthy primary overlays equals or surpasses the predefined threshold.
config vpn ipsec phase1-interface
    edit <phase-1 name>                                                   
        set monitor <overlay> <overlay> ... <overlay>
        set monitor-min <integer>
    next
end
                                            | monitor  | The IPsec interfaces to monitor. | 
| monitor-min  | The minimum number of monitored interfaces that must become degraded before this interface is activated (0 = all interfaces, default = 0). | 
In this example, four primary overlays are configured, T1 - T4, on fixed broadband connections and one backup overlay, T5, is configured on an LTE connection.
The backup overlay stays down as long as the primary overlays are working normally. When all four of the primary overlays go down, the backup overlay is activated and used to forward traffic. If any of the primary overlays recover, then the backup overlay goes down.
SD-WAN can also be configured to steer traffic.
To configure the overlays:
- 
                                                    Configure the VPN remote gateways: config vpn ipsec phase1-interface
    edit "T1"                                                             
        set interface "dmz"
        set ike-version 2
        set peertype any
        set net-device disable
        set proposal aes128-sha256
        set remote-gw 172.16.208.2
        set psksecret **********
    next
    edit "T2"                                                            
        set interface "agg1"
        set ike-version 2
        set peertype any
        set net-device disable
        set proposal aes128-sha256
        set remote-gw 172.16.203.2
        set psksecret **********
    next
    edit "T3"                                                            
        set interface "vlan100"
        set ike-version 2
        set peertype any
        set net-device disable
        set proposal aes128-sha256
        set remote-gw 172.16.206.2
        set psksecret **********
    next
    edit "T4"                                                         
        set interface "port15"
        set ike-version 2
        set peertype any
        set net-device disable
        set proposal aes128-sha256
        set remote-gw 172.16.209.2
        set psksecret **********
    next
    edit "T5"                                                        
        set interface "vlan200"
        set ike-version 2
        set peertype any
        set monitor "T1" "T2" "T3" "T4"
        set monitor-min 4
        set net-device disable
        set proposal aes128-sha256
        set remote-gw 172.16.210.2
        set psksecret **********
    next
end
- 
                                                    Configure the VPN tunnels: config vpn ipsec phase2-interface
    edit "T1_P2"
        set phase1name "T1"
        set proposal aes256-sha256
        set auto-negotiate enable
    next
    edit "T2_P2"
        set phase1name "T2"
        set proposal aes256-sha256
        set auto-negotiate enable
    next
    edit "T3_P2"
        set phase1name "T3"
        set proposal aes256-sha256
        set auto-negotiate enable
    next
    edit "T4_P2"
        set phase1name "T4"
        set proposal aes256-sha256
        set auto-negotiate enable
    next
    edit "T5_P2"
        set phase1name "T5"
        set proposal aes256-sha256
        set auto-negotiate enable
    next
end
- 
                                                    Configure the interfaces: config system interface
    edit "T1"
        set vdom "root"
        set ip 100.1.1.1 255.255.255.255
        set allowaccess ping
        set type tunnel
        set remote-ip 100.1.1.2 255.255.255.0
        set snmp-index 113
        set interface "dmz"
    next
    edit "T2"
        set vdom "root"
        set ip 100.1.2.1 255.255.255.255
        set allowaccess ping
        set type tunnel
        set remote-ip 100.1.2.2 255.255.255.0
        set snmp-index 114
        set interface "agg1"
    next
    edit "T3"
        set vdom "root"
        set ip 100.1.3.1 255.255.255.255
        set allowaccess ping
        set type tunnel
        set remote-ip 100.1.3.2 255.255.255.0
        set snmp-index 115
        set interface "vlan100"
    next
    edit "T4"
        set vdom "root"
        set ip 100.1.4.1 255.255.255.255
        set allowaccess ping
        set type tunnel
        set remote-ip 100.1.4.2 255.255.255.0
        set snmp-index 65
        set interface "port15"
    next
    edit "T5"
        set vdom "root"
        set ip 100.1.5.1 255.255.255.255
        set allowaccess ping
        set type tunnel
        set remote-ip 100.1.5.2 255.255.255.0
        set snmp-index 117
        set interface "vlan200"
    next
end
- 
                                                    Check the IPsec tunnel summary: # get vpn ipsec tunnel summary 'T2' 172.16.203.2:0 selectors(total,up): 1/1 rx(pkt,err): 0/0 tx(pkt,err): 0/4 'T3' 172.16.206.2:0 selectors(total,up): 1/1 rx(pkt,err): 0/0 tx(pkt,err): 0/4 'T4' 172.16.209.2:0 selectors(total,up): 1/1 rx(pkt,err): 0/0 tx(pkt,err): 0/4 'T5' 172.16.210.2:0 selectors(total,up): 1/0 rx(pkt,err): 0/0 tx(pkt,err): 0/4 'T1' 172.16.208.2:0 selectors(total,up): 1/1 rx(pkt,err): 0/0 tx(pkt,err): 0/4 The backup overlay, T5, is down.
To configure steering traffic with SD-WAN:
- 
                                                    Configure the SD-WAN: config system sdwan
    set status enable
    config zone
        edit "virtual-wan-link"
        next
    end
    config members
        edit 1
            set interface "T1"
        next
        edit 2
            set interface "T2"
        next
        edit 3
            set interface "T3"
        next
        edit 4
            set interface "T4"
        next
        edit 5
            set interface "T5"
        next
    end
    config service
        edit 1
            set name "1"
            set load-balance enable 
            set dst "all"
            set src "172.16.205.0"
            set priority-members 1 2 3 4 5
        next
    end
end
- 
                                                    Configure a static route: config router static
    edit 5
        set dst 8.0.0.0 255.0.0.0
        set distance 1
        set sdwan-zone "virtual-wan-link"
    next
end
- 
                                                    Check the routing table: # get router info routing-table static
Routing table for VRF=0
S       8.0.0.0/8 [1/0] via T2 tunnel 172.16.203.2, [1/0]
                  [1/0] via T3 tunnel 172.16.206.2, [1/0]
                  [1/0] via T1 tunnel 172.16.208.2, [1/0]
                  [1/0] via T4 tunnel 172.16.209.2, [1/0]
Check the results:
- 
                                                    When both the T1 and T2 connections are down, T5 stays down as well, and traffic is load-balanced on T3 and T4 by the SD-WAN configuration: # get vpn ipsec tunnel summary 'T2' 172.16.203.2:0 selectors(total,up): 1/0 rx(pkt,err): 0/0 tx(pkt,err): 0/0 'T3' 172.16.206.2:0 selectors(total,up): 1/1 rx(pkt,err): 0/0 tx(pkt,err): 0/0 'T4' 172.16.209.2:0 selectors(total,up): 1/1 rx(pkt,err): 0/0 tx(pkt,err): 0/4 'T5' 172.16.210.2:0 selectors(total,up): 1/0 rx(pkt,err): 0/0 tx(pkt,err): 0/4 'T1' 172.16.208.2:0 selectors(total,up): 1/0 rx(pkt,err): 0/0 tx(pkt,err): 0/0 # get router info routing-table static
Routing table for VRF=0
S       8.0.0.0/8 [1/0] via T3 tunnel 172.16.206.2, [1/0]
                  [1/0] via T4 tunnel 172.16.209.2, [1/0]Traffic is load-balanced between the remaining tunnels: # diagnose sniffer packet any 'host 8.8.8.8' 4 interfaces=[any] filters=[host 8.8.8.8] 3.027055 port5 in 172.16.205.100 -> 8.8.8.8: icmp: echo request 3.027154 T4 out 172.16.205.100 -> 8.8.8.8: icmp: echo request 3.031434 T4 in 8.8.8.8 -> 172.16.205.100: icmp: echo reply 3.031485 port5 out 8.8.8.8 -> 172.16.205.100: icmp: echo reply 3.612818 port5 in 172.16.205.100 -> 8.8.8.8: icmp: echo request 3.612902 T3 out 172.16.205.100 -> 8.8.8.8: icmp: echo request 3.617107 T3 in 8.8.8.8 -> 172.16.205.100: icmp: echo reply 3.617159 port5 out 8.8.8.8 -> 172.16.205.100: icmp: echo reply 4.168845 port5 in 172.16.205.100 -> 8.8.8.8: icmp: echo request 4.168907 T4 out 172.16.205.100 -> 8.8.8.8: icmp: echo request 4.173150 T4 in 8.8.8.8 -> 172.16.205.100: icmp: echo reply 4.173174 port5 out 8.8.8.8 -> 172.16.205.100: icmp: echo reply 4.710907 port5 in 172.16.205.100 -> 8.8.8.8: icmp: echo request 4.710991 T3 out 172.16.205.100 -> 8.8.8.8: icmp: echo request 4.715933 T3 in 8.8.8.8 -> 172.16.205.100: icmp: echo reply 4.715958 port5 out 8.8.8.8 -> 172.16.205.100: icmp: echo reply
- 
                                                    When all of the primary overlays are down, T5 is activated and used for traffic # get vpn ipsec tunnel summary 'T2' 172.16.203.2:0 selectors(total,up): 1/0 rx(pkt,err): 0/0 tx(pkt,err): 0/0 'T3' 172.16.206.2:0 selectors(total,up): 1/0 rx(pkt,err): 0/0 tx(pkt,err): 0/0 'T4' 172.16.209.2:0 selectors(total,up): 1/0 rx(pkt,err): 0/0 tx(pkt,err): 0/0 'T5' 172.16.210.2:0 selectors(total,up): 1/1 rx(pkt,err): 0/0 tx(pkt,err): 0/4 'T1' 172.16.208.2:0 selectors(total,up): 1/0 rx(pkt,err): 0/0 tx(pkt,err): 0/0 # get router info routing-table static Routing table for VRF=0 S 8.0.0.0/8 [1/0] via T5 tunnel 172.16.210.2, [1/0] Traffic is using the backup overlay, T5: # diagnose sniffer packet any 'host 8.8.8.8' 4 interfaces=[any] filters=[host 8.8.8.8] 1.907944 port5 in 172.16.205.100 -> 8.8.8.8: icmp: echo request 1.908045 T5 out 172.16.205.100 -> 8.8.8.8: icmp: echo request 1.912283 T5 in 8.8.8.8 -> 172.16.205.100: icmp: echo reply 1.912351 port5 out 8.8.8.8 -> 172.16.205.100: icmp: echo reply 2.665921 port5 in 172.16.205.100 -> 8.8.8.8: icmp: echo request 2.665999 T5 out 172.16.205.100 -> 8.8.8.8: icmp: echo request 2.670209 T5 in 8.8.8.8 -> 172.16.205.100: icmp: echo reply 2.670235 port5 out 8.8.8.8 -> 172.16.205.100: icmp: echo reply 5.269997 port5 in 172.16.205.100 -> 8.8.8.8: icmp: echo request 5.270090 T5 out 172.16.205.100 -> 8.8.8.8: icmp: echo request 5.274275 T5 in 8.8.8.8 -> 172.16.205.100: icmp: echo reply 5.274300 port5 out 8.8.8.8 -> 172.16.205.100: icmp: echo reply 5.781848 port5 in 172.16.205.100 -> 8.8.8.8: icmp: echo request 5.781920 T5 out 172.16.205.100 -> 8.8.8.8: icmp: echo request 5.786334 T5 in 8.8.8.8 -> 172.16.205.100: icmp: echo reply 5.786363 port5 out 8.8.8.8 -> 172.16.205.100: icmp: echo reply
- 
                                                    If T4 recovers, T5 is deactivated and traffic switches to T4: # get vpn ipsec tunnel summary 'T2' 172.16.203.2:0 selectors(total,up): 2/0 rx(pkt,err): 0/0 tx(pkt,err): 0/0 'T3' 172.16.206.2:0 selectors(total,up): 2/0 rx(pkt,err): 0/0 tx(pkt,err): 0/0 'T4' 172.16.209.2:0 selectors(total,up): 2/2 rx(pkt,err): 0/0 tx(pkt,err): 0/0 'T5' 172.16.210.2:0 selectors(total,up): 2/0 rx(pkt,err): 0/0 tx(pkt,err): 0/0 'T1' 172.16.208.2:0 selectors(total,up): 2/0 rx(pkt,err): 0/0 tx(pkt,err): 0/0 # get router info routing-table static Routing table for VRF=0 S 8.0.0.0/8 [1/0] via T4 tunnel 172.16.209.2, [1/0] The primary overlay T4 has recovered, and the backup overlay is down again: # diagnose sniffer packet any 'host 8.8.8.8' 4 interfaces=[any] filters=[host 8.8.8.8] 4.555685 port5 in 172.16.205.100 -> 8.8.8.8: icmp: echo request 4.555790 T4 out 172.16.205.100 -> 8.8.8.8: icmp: echo request 4.560428 T4 in 8.8.8.8 -> 172.16.205.100: icmp: echo reply 4.560478 port5 out 8.8.8.8 -> 172.16.205.100: icmp: echo reply 5.163223 port5 in 172.16.205.100 -> 8.8.8.8: icmp: echo request 5.163332 T4 out 172.16.205.100 -> 8.8.8.8: icmp: echo request 5.167590 T4 in 8.8.8.8 -> 172.16.205.100: icmp: echo reply 5.167620 port5 out 8.8.8.8 -> 172.16.205.100: icmp: echo reply 5.650089 port5 in 172.16.205.100 -> 8.8.8.8: icmp: echo request 5.650194 T4 out 172.16.205.100 -> 8.8.8.8: icmp: echo request 5.654352 T4 in 8.8.8.8 -> 172.16.205.100: icmp: echo reply 5.654387 port5 out 8.8.8.8 -> 172.16.205.100: icmp: echo reply 6.102181 port5 in 172.16.205.100 -> 8.8.8.8: icmp: echo request 6.102263 T4 out 172.16.205.100 -> 8.8.8.8: icmp: echo request 6.106411 T4 in 8.8.8.8 -> 172.16.205.100: icmp: echo reply 6.106445 port5 out 8.8.8.8 -> 172.16.205.100: icmp: echo reply