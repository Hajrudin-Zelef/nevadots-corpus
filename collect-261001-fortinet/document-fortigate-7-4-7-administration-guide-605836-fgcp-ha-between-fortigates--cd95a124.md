FGCP HA between FortiGates of the same model with different AC and DC PSUs
To improve power redundancy, FGCP HA clusters can support forming HA between units of the same model but with different AC PSU and DC PSU power supplies. This enables redundancy in a situation where power is completely lost on the AC grid, but traffic can fail over to a cluster member running on an independent DC grid.
The cluster members must be the same model with the same firmware installed, and must have the same hardware configuration other than the PSU.
In the following examples, there is an FGCP cluster with AC and DC PSU members: a FortiGate 1800F-DC (primary) and FortiGate 1800F (secondary).
Basic configuration
To configure the FGCP cluster in the GUI:
- 
                                                    On the primary FortiGate (FG-1800F-DC), go to System > HA.
- 
                                                    Configure the following settings: Mode Active-Passive Device priority 128 Group ID 0 Group name Example_cluster Password Enter a password. Session pickup Enable this setting. Monitor interfaces Click the + to add port5 and port6. Heartbeat interfaces Click the + to add ha1 and ha2.
- 
                                                    Click OK.
- 
                                                    On the secondary FortiGate (FG-1800F), go to System > HA.
- 
                                                    Configure the following settings: Mode Active-Passive Device priority 127 Group ID 0 Group name Example_cluster Password Enter a password. Session pickup Enable this setting. Monitor interfaces Click the + to add port5 and port6. Heartbeat interfaces Click the + to add ha1 and ha2.
- 
                                                    Click OK.
- 
                                                    Verify that the cluster status is Synchronized.
To configure the FGCP cluster in the CLI:
- 
                                                    Configure the primary FortiGate (FG-1800F-DC): config system ha
    set group-name "Example_cluster"
    set mode a-p
    set password **********
    set hbdev "ha2" 0 "ha1" 0 
    set session-pickup enable
    set override disable
    set monitor "port5" "port6" 
end
- 
                                                    Configure the secondary FortiGate (FG-1800F): config system ha
    set group-name "Example_cluster"
    set mode a-p
    set password **********
    set hbdev "ha2" 0 "ha1" 0 
    set session-pickup enable
    set override disable
    set priority 127
    set monitor "port5" "port6" 
end
- 
                                                    Verify the cluster status on the primary FortiGate: # get system ha status  
HA Health Status: OK
Model: FortiGate-1800F
Mode: HA A-P
Group Name: Example_cluster
Group ID: 0
Debug: 0
Cluster Uptime: 0 days 0:56:11
Cluster state change time: 2023-05-29 19:11:14
Primary selected using:
    <2023/05/29 19:11:14> vcluster-1: FG180FTK*******1 is selected as the primary because its uptime is larger than peer member FG180FTK*******2.
    <2023/05/29 18:59:45> vcluster-1: FG180FTK*******2 is selected as the primary because its uptime is larger than peer member FG180FTK*******1.
    <2023/05/29 18:59:45> vcluster-1: FG180FTK*******1 is selected as the primary because its override priority is larger than peer member FG180FTK*******2.
ses_pickup: enable, ses_pickup_delay=disable
override: disable
Configuration Status:
    FG180FTK*******1(updated 4 seconds ago): in-sync
    FG180FTK*******1 chksum dump: 95 4e 92 c3 39 75 8e 0e db 83 8d b7 b2 b1 9f 04 
    FG180FTK*******2(updated 5 seconds ago): in-sync
    FG180FTK*******2 chksum dump: 95 4e 92 c3 39 75 8e 0e db 83 8d b7 b2 b1 9f 04 
System Usage stats:
    FG180FTK*******1(updated 4 seconds ago):
        sessions=4, npu-sessions=0, average-cpu-user/nice/system/idle=0%/0%/0%/99%, memory=22%
    FG180FTK*******2(updated 5 seconds ago):
        sessions=0, npu-sessions=0, average-cpu-user/nice/system/idle=0%/0%/0%/99%, memory=22%
HBDEV stats:
    FG180FTK*******1(updated 4 seconds ago):
        ha2: physical/10000full, up, rx-bytes/packets/dropped/errors=18367581/33512/0/0, tx=9563450/16609/0/0
        ha1: physical/10000full, up, rx-bytes/packets/dropped/errors=11543018/22166/0/0, tx=12359673/22151/0/0
    FG180FTK*******2(updated 5 seconds ago):
        ha2: physical/10000full, up, rx-bytes/packets/dropped/errors=19133123/35087/0/0, tx=10685583/18475/0/0
        ha1: physical/10000full, up, rx-bytes/packets/dropped/errors=17011332/25876/0/0, tx=11919050/24991/0/0
MONDEV stats:
    FG180FTK*******1(updated 4 seconds ago):
        port5: physical/1000full, up, rx-bytes/packets/dropped/errors=988220/13742/0/0, tx=106998000/73260/0/0
        port6: physical/1000full, up, rx-bytes/packets/dropped/errors=107084264/73624/0/0, tx=953158/13611/0/0
    FG180FTK*******2(updated 5 seconds ago):
        port5: physical/1000full, up, rx-bytes/packets/dropped/errors=38194/128/0/0, tx=0/0/0/0
        port6: physical/1000full, up, rx-bytes/packets/dropped/errors=99019/448/0/0, tx=0/0/0/0
Primary     : FortiGate-1800F , FG180FTK*******1, HA cluster index = 1
Secondary   : FortiGate-1800F , FG180FTK*******2, HA cluster index = 0
number of vcluster: 1
vcluster 1: work 169.254.0.2
Primary: FG180FTK*******1, HA operating index = 0
Secondary: FG180FTK*******2, HA operating index = 1
- 
                                                    Verify the cluster status on the secondary FortiGate: # get system ha status  
HA Health Status: OK
Model: FortiGate-1800F
Mode: HA A-P
Group Name: Example_cluster
Group ID: 0
Debug: 0
Cluster Uptime: 0 days 0:56:53
Cluster state change time: 2023-05-29 19:11:14
Primary selected using:
    <2023/05/29 19:11:14> vcluster-1: FG180FTK*******1 is selected as the primary because its uptime is larger than peer member FG180FTK*******2.
    <2023/05/29 18:59:45> vcluster-1: FG180FTK*******2 is selected as the primary because its uptime is larger than peer member FG180FTK*******1.
    <2023/05/29 18:55:03> vcluster-1: FG180FTK*******2 is selected as the primary because it's the only member in the cluster.
    <2023/05/29 18:54:57> vcluster-1: FG180FTK*******2 is selected as the primary because SET_AS_SECONDARY flag is set on peer member FG180FTK*******1.
ses_pickup: enable, ses_pickup_delay=disable
override: disable
...
Secondary   : FortiGate-1800F , FG180FTK*******2, HA cluster index = 0
Primary     : FortiGate-1800F , FG180FTK*******1, HA cluster index = 1
number of vcluster: 1
vcluster 1: standby 169.254.0.2
Secondary: FG180FTK*******2, HA operating index = 1
Primary: FG180FTK*******1, HA operating index = 0
Testing synchronization in the cluster
Based on the preceding example, the interface and firewall policy configurations are changed on the primary FortiGate. These configuration changes and sessions are synchronized to the secondary FortiGate. If the switch interface connected to the primary's port5 is down (port2), this triggers the monitor interface to be down, and the PC1 traffic will fail over to the secondary FortiGate.
To test configuration synchronization in the FGCP cluster:
- 
                                                    Modify configurations on the primary FortiGate (FG-1800F-DC). 
  - 
                                                            Edit the interface settings: config system interface
    edit "port5"
        set ip 10.1.100.1 255.255.255.0
        set allowaccess ping https ssh http telnet
        set alias "To_Client_PC"
        config ipv6
            set ip6-address 2000:10:1:100::1/64
            set ip6-allowaccess ping https ssh http
        end
    next
    edit "port6"
        set ip 172.16.200.1 255.255.255.0
        set allowaccess ping https ssh http fgfm
        set alias "To_Server"
        config ipv6
            set ip6-address 2000:172:16:200::1/64
            set ip6-allowaccess ping https ssh http
        end
    next
end
  - 
                                                            Edit the firewall policy settings: config firewall policy
    edit 1
        set name "to_server_policy"
        set srcintf "port5"
        set dstintf "port6"
        set action accept
        set srcaddr "all"
        set dstaddr "all"
        set schedule "always"
        set service "ALL"
        set logtraffic-start enable
    next
end
- 
                                                            
- 
                                                    On the secondary FortiGate (FG-1800F), verify that the settings were synchronized. 
  - 
                                                            Verify the interface settings: show system interface
    config system interface
        ...
        edit "port5"
            set vdom "root"
            set ip 10.1.100.1 255.255.255.0
            set allowaccess ping https ssh http telnet
            set type physical
            set alias "To_Client_PC"
            set snmp-index 9
            config ipv6
                set ip6-address 2000:10:1:100::1/64
                set ip6-allowaccess ping https ssh http
            end
        next
        edit "port6"
            set vdom "root"
            set ip 172.16.200.1 255.255.255.0
            set allowaccess ping https ssh http fgfm
            set type physical
            set alias "To_Server"
            set snmp-index 10
            config ipv6
                set ip6-address 2000:172:16:200::1/64
                set ip6-allowaccess ping https ssh http
            end
        next
    end
  - 
                                                            Verify the firewall policy settings: show firewall policy
    config firewall policy
            edit 1
            set name "to_server_policy"
            set uuid 82a05e78-fe90-51ed-eb16-ee7bdea60de0
            set srcintf "port5"
            set dstintf "port6"
            set action accept
            set srcaddr "all"
            set dstaddr "all"
            set schedule "always"
            set service "ALL"
            set logtraffic-start enable
        next
    end
  - 
                                                            Verify the HA checksum: # diagnose sys ha checksum show is_manage_primary()=0, is_root_primary()=0 debugzone global: 4e 15 af c3 c6 87 32 f5 69 5c b7 33 b1 8b 27 12 root: 4a 52 e4 f1 6a 2b eb 7d 84 7d f1 48 50 93 fe d9 all: 95 4e 92 c3 39 75 8e 0e db 83 8d b7 b2 b1 9f 04 checksum global: 4e 15 af c3 c6 87 32 f5 69 5c b7 33 b1 8b 27 12 root: 4a 52 e4 f1 6a 2b eb 7d 84 7d f1 48 50 93 fe d9 all: 95 4e 92 c3 39 75 8e 0e db 83 8d b7 b2 b1 9f 04
- 
                                                            
To test session synchronization in the FGCP cluster:
- 
                                                    On PC1, verify the IP address and gateway: root@pc1:~# ifconfig eth1
eth1      Link encap:Ethernet  HWaddr 00:0c:29:a0:60:d6 
          inet addr:10.1.100.11  Bcast:10.1.100.255  Mask:255.255.255.0					 
          ...
root@pc1:~# route -n
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
0.0.0.0         10.1.100.1      0.0.0.0         UG    0      0        0 eth1
10.1.100.0      0.0.0.0         255.255.255.0   U     0      0        0 eth1
10.6.30.0       0.0.0.0         255.255.255.0   U     0      0        0 eth0
169.254.0.0     0.0.0.0         255.255.0.0     U     1000   0        0 eth0
- 
                                                    Using Wget, initiate a large file download with HTTP that will maintain a long session: root@pc1:~# wget http://172.16.200.55/big100MB.html   --keep-session-cookies --limit-rate=128k --progress=dot -S -r --delete-after
--2023-05-29 14:55:33--  http://172.16.200.55/big100MB.html
Connecting to 172.16.200.55:80... connected.
HTTP request sent, awaiting response... 
  HTTP/1.1 200 OK
  Date: Mon, 29 May 2023 21:55:41 GMT
  Server: Apache/2.4.18 (Ubuntu)
  Last-Modified: Thu, 01 Dec 2016 00:17:35 GMT
  ETag: "6126784-5428dbf967ad3"
  Accept-Ranges: bytes
  Content-Length: 101869444
  Vary: Accept-Encoding
  Keep-Alive: timeout=5, max=100
  Connection: Keep-Alive
  Content-Type: text/html
Length: 101869444 (97M) [text/html]
Saving to: '172.16.200.55/big100MB.html'
     0K .......... .......... .......... .......... ..........  0%  199K 8m18s
    50K .......... .......... .......... .......... ..........  0%  100K 12m26s
   100K .......... .......... .......... .......... ..........  0%  200K 11m3s
   150K .......... .......... .......... .......... ..........  0%  100K 12m25s
   200K .......... .......... .......... .......... ..........  0%  100K 13m14s
   250K .......... .......... .......... .......... ..........  0%  200K 12m24s
- 
                                                    On the primary FortiGate (FG-1800F-DC), check the session information: # diagnose sys session filter dport 80 # diagnose sys session list session info: proto=6 proto_state=01 duration=5 expire=3594 timeout=3600 flags=00000000 socktype=0 sockport=0 av_idx=0 use=4 origin-shaper= reply-shaper= per_ip_shaper= class_id=0 ha_id=0 policy_dir=0 tunnel=/ vlan_cos=0/255 state=may_dirty npu synced log-start statistic(bytes/packets/allow_err): org=112/2/1 reply=60/1/1 tuples=2 tx speed(Bps/kbps): 0/0 rx speed(Bps/kbps): 0/0 orgin->sink: org pre->post, reply pre->post dev=13->14/14->13 gwy=0.0.0.0/0.0.0.0 hook=pre dir=org act=noop 10.1.100.11:54752->172.16.200.55:80(0.0.0.0:0) hook=post dir=reply act=noop 172.16.200.55:80->10.1.100.11:54752(0.0.0.0:0) pos/(before,after) 0/(0,0), 0/(0,0) misc=0 policy_id=1 pol_uuid_idx=15767 auth_info=0 chk_client_info=0 vd=0 serial=00000d80 tos=ff/ff app_list=0 app=0 url_cat=0 rpdb_link_id=00000000 ngfwid=n/a npu_state=0x4000c00 ofld-O ofld-R npu info: flag=0x81/0x81, offload=9/9, ips_offload=0/0, epid=133/132, ipid=132/133, vlan=0x0000/0x0000 vlifid=132/133, vtag_in=0x0000/0x0000 in_npu=1/1, out_npu=1/1, fwd_en=0/0, qid=12/12 total session: 1
- 
                                                    On the secondary FortiGate (FG-1800F), check that the session is synchronized: # diagnose sys session filter dport 80 # diagnose sys session list session info: proto=6 proto_state=01 duration=47 expire=3552 timeout=3600 flags=00000000 socktype=0 sockport=0 av_idx=0 use=3 origin-shaper= reply-shaper= per_ip_shaper= class_id=0 ha_id=0 policy_dir=0 tunnel=/ vlan_cos=0/255 state=dirty may_dirty npu syn_ses statistic(bytes/packets/allow_err): org=0/0/0 reply=0/0/0 tuples=2 tx speed(Bps/kbps): 0/0 rx speed(Bps/kbps): 0/0 orgin->sink: org pre->post, reply pre->post dev=13->14/14->13 gwy=0.0.0.0/0.0.0.0 hook=pre dir=org act=noop 10.1.100.11:54752->172.16.200.55:80(0.0.0.0:0) hook=post dir=reply act=noop 172.16.200.55:80->10.1.100.11:54752(0.0.0.0:0) pos/(before,after) 0/(0,0), 0/(0,0) misc=0 policy_id=1 pol_uuid_idx=0 auth_info=0 chk_client_info=0 vd=0 serial=00000d80 tos=ff/ff app_list=0 app=0 url_cat=0 rpdb_link_id=00000000 ngfwid=n/a npu_state=0x4000000 npu info: flag=0x00/0x00, offload=0/0, ips_offload=0/0, epid=0/0, ipid=0/0, vlan=0x0000/0x0000 vlifid=0/0, vtag_in=0x0000/0x0000 in_npu=0/0, out_npu=0/0, fwd_en=0/0, qid=0/0 no_ofld_reason: total session: 1
To test failover in the FGCP cluster:
- 
                                                    On the switch connected to port5 of the primary FortiGate, change port2's status to be down: config switch physical-port 
    edit port2
        set status down 
    next
end
- 
                                                    Check the HA status on the primary FortiGate (FG-1800F-DC), which now becomes the secondary device: # get system ha status  
HA Health Status: 
    WARNING: FG180FTK*******1 has mondev down; 
Model: FortiGate-1800F
Mode: HA A-P
Group Name: Example_cluster
Group ID: 0
Debug: 0
Cluster Uptime: 0 days 1:16:13
Cluster state change time: 2023-05-29 20:08:56
Primary selected using:
    <2023/05/29 20:08:56> vcluster-1: FG180FTK*******2 is selected as the primary because the value 0 of link-failure + pingsvr-failure is less than peer member FG180FTK*******1.
    <2023/05/29 19:11:14> vcluster-1: FG180FTK*******1 is selected as the primary because its uptime is larger than peer member FG180FTK*******2.
    <2023/05/29 18:59:45> vcluster-1: FG180FTK*******2 is selected as the primary because its uptime is larger than peer member FG180FTK*******1.
    <2023/05/29 18:59:45> vcluster-1: FG180FTK*******1 is selected as the primary because its override priority is larger than peer member FG180FTK*******2.
ses_pickup: enable, ses_pickup_delay=disable
override: disable
...
Secondary   : FortiGate-1800F , FG180FTK*******1, HA cluster index = 1
Primary     : FortiGate-1800F , FG180FTK*******2, HA cluster index = 0
number of vcluster: 1
vcluster 1: standby 169.254.0.1
Secondary: FG180FTK*******1, HA operating index = 1
Primary: FG180FTK*******2, HA operating index = 0
- 
                                                    Check the HA status on the new primary FortiGate (FG-1800F): # get system ha status   
HA Health Status: 
    WARNING: FG180FTK*******1 has mondev down; 
Model: FortiGate-1800F
Mode: HA A-P
Group Name: Example_cluster
Group ID: 0
Debug: 0
Cluster Uptime: 0 days 1:19:9
Cluster state change time: 2023-05-29 20:08:56
Primary selected using:
    <2023/05/29 20:08:56> vcluster-1: FG180FTK*******2 is selected as the primary because the value 0 of link-failure + pingsvr-failure is less than peer member FG180FTK*******1.
    <2023/05/29 19:11:14> vcluster-1: FG180FTK*******1 is selected as the primary because its uptime is larger than peer member FG180FTK*******2.
    <2023/05/29 18:59:45> vcluster-1: FG180FTK*******2 is selected as the primary because its uptime is larger than peer member FG180FTK*******1.
    <2023/05/29 18:55:03> vcluster-1: FG180FTK*******2 is selected as the primary because it's the only member in the cluster.
ses_pickup: enable, ses_pickup_delay=disable
override: disable
...
Primary     : FortiGate-1800F , FG180FTK*******2, HA cluster index = 0
Secondary   : FortiGate-1800F , FG180FTK*******1, HA cluster index = 1
number of vcluster: 1
vcluster 1: work 169.254.0.1
Primary: FG180FTK*******2, HA operating index = 0
Secondary: FG180FTK*******1, HA operating index = 1
- 
                                                    On PC1, verify that the HTTP traffic remains uninterrupted: ... 74700K .......... .......... .......... .......... .......... 75% 100K 3m13s 74750K .......... .......... .......... .......... .......... 75% 200K 3m13s 74800K .......... .......... .......... .......... .......... 75% 100K 3m12s 74850K .......... .......... .......... .......... .......... 75% 200K 3m12s 74900K .......... .......... .......... .......... .......... 75% 100K 3m12s 74950K .......... .......... .......... .......... .......... 75% 100K 3m11s 75000K .......... .......... .......... .......... .......... 75% 200K 3m11s 75050K .......... .......... .......... .......... .......... 75% 100K 3m10s 75100K .......... .......... .......... .......... .......... 75% 200K 3m10s 75150K .......... .......... .......... .......... .......... 75% 100K 3m10s