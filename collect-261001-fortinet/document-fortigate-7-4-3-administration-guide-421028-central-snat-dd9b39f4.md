Central SNAT
Central SNAT
The central SNAT table enables you to define and control (with more granularity) the address translation performed by FortiGate. With the NAT table, you can define the rules for the source address or address group, and which IP pool the destination address uses.
FortiGate reads the NAT rules from the top down until it hits a matching rule for the incoming address. This enables you to create multiple NAT policies that dictate which IP pool is used based on source address, destination address, and source port. NAT policies can be rearranged within the policy list. NAT policies are applied to network traffic after a security policy.
The central SNAT table allows you to create, edit, delete, and clone central SNAT entries.
Central SNAT notes
- The central NAT feature is not enabled by default.
- If central NAT is enabled, the NAT option under IPv4 policies is skipped and SNAT must be done via central-snat-map . The firewall policy list and dialog boxes have messages and redirection links to show this information.
- If NGFW mode is policy-based, then it is assumed that central NAT (specifically SNAT) is enabled implicitly.
Sample configuration
To enable central SNAT from the GUI:
- 
                                                    In System > Settings, under System Operations Settings, enable Central SNAT.
- 
                                                    Click Apply.
To enable or disable central SNAT using the CLI:
config system settings
   set central-nat {enable | disable}
end
                                            When central NAT is enabled, Policy & Objects displays the Central SNAT section.
The Central SNAT policy has many options:
| Field | Description | 
|---|---|
| Type | Specify whether you are performing SNAT on IPv4 or IPv6. This option only appears when IPv6 is enabled under Feature Visibility. | 
| Incoming Interface | Specify one or more interfaces for the ingress traffic. | 
| Outgoing Interface | Specify one or more interfaces for the egress traffic. | 
| Source Address | Specify the address or address group of the source. | 
| Destination Address | Specify the address or address group of the destination. | 
| NAT | Enable or disable to perform NAT. When disabled, no source address translation will occur. | 
| IP Pool Configuration | Use outgoing interface address:  Use Dynamic IP Pool:  | 
| Protocol | Choose from any, TCP, UDP, SCTP, or specify the protocol number to match. For example, for ICMP, click specify with the protocol number 1. | 
| Explicit port mapping | Enable in order to match this NAT policy only when the following ports are a match:  Explicit port mapping cannot apply to some protocols which do not use ports, such as ICMP. When enabling a NAT policy which uses Explicit port mapping, always consider that ICMP traffic will not match this policy. When using IP Pools, only the Overload type IP Pool allows Explicit port mapping. When Explicit port mapping is applied, you must define an original source port range and a translated sort port range. The source port will map one to one with the translated port. Refer to Dynamic SNAT to understand how each IP Pool type works. | 
| Comments | Enter comments for this NAT policy. | 
| Enable this policy | Enable or disable this policy. | 
To configure central SNAT using the CLI:
config firewall central-snat-map
    edit <policyID number>
        set status {enable | disable}
        set orig-addr <valid address object preconfigured on the FortiGate>
        set srcintf <name of interface on the FortiGate>
        set dst-addr <valid address object preconfigured on the FortiGate>
        set dstintf <name of interface on the FortiGate>
        set protocol <integer for protocol number>
        set dst-port <integer for destination port or port range>
        set orig-port <integer for original port number>
        set nat-port <integer for translated port number>
        set comments <string>
    next
end
                                            |  | Setting the destination port for traffic matching is available when the protocols are TCP, UDP, or SCTP. | 
Example one
Apply SNAT to all traffic from port2 to port3.
To configure from the CLI:
config firewall central-snat-map
    edit 1
        set srcintf "port3"
        set dstintf "port2"
        set orig-addr "all"
        set dst-addr "all"
    next
end
                                            Example two
Apply an IP Pool to all traffic from port3 to port2 that are TCP. NAT all other traffic using the outgoing interface IP.
To configure from the CLI:
config firewall ippool
    edit "Overload-IPPOOL"
        set startip 192.168.2.201
        set endip 192.168.2.202
    next
end
config firewall central-snat-map
    edit 1
        set srcintf "port3"
        set dstintf "port2"
        set orig-addr "all"
        set dst-addr "all"
        set protocol 6
        set nat-ippool "Overload-IPPOOL"
    next
    edit 2
        set srcintf "port3"
        set dstintf "port2"
        set orig-addr "all"
        set dst-addr "all"
    next
end
                                            To collect session table output from the CLI:
diagnose sys session list
The TCP session (protocol 6) is NAT'd with Overload-IPPOOL to 192.168.2.201:
session info: proto=6 proto_state=05 duration=14 expire=0 timeout=3600 flags=00000000 socktype=0 sockport=0 av_idx=0 use=3
origin-shaper=
reply-shaper=
per_ip_shaper=
class_id=0 ha_id=0 policy_dir=0 tunnel=/ vlan_cos=0/255
state=may_dirty 
statistic(bytes/packets/allow_err): org=860/7/1 reply=555/8/1 tuples=2
tx speed(Bps/kbps): 60/0 rx speed(Bps/kbps): 38/0
orgin->sink: org pre->post, reply pre->post dev=9->6/6->9 gwy=192.168.2.1/192.168.0.10
hook=post dir=org act=snat 192.168.0.10:49531->23.57.57.114:443(192.168.2.201:61776)
hook=pre dir=reply act=dnat 23.57.57.114:443->192.168.2.201:61776(192.168.0.10:49531)
pos/(before,after) 0/(0,0), 0/(0,0)
dst_mac=04:d5:90:5f:a2:2a
misc=0 policy_id=2 auth_info=0 chk_client_info=0 vd=0
serial=00011065 tos=ff/ff app_list=0 app=0 url_cat=0
sdwan_mbr_seq=0 sdwan_service_id=0
rpdb_link_id=00000000 rpdb_svc_id=0 ngfwid=n/a
npu_state=0x040000
A UDP session (protocol 17) is NAT'd to the outgoing interface IP address 192.168.2.86:
session info: proto=17 proto_state=01 duration=16 expire=163 timeout=0 flags=00000000 socktype=0 sockport=0 av_idx=0 use=3
origin-shaper=
reply-shaper=
per_ip_shaper=
class_id=0 ha_id=0 policy_dir=0 tunnel=/ helper=dns-udp vlan_cos=0/255
state=may_dirty 
statistic(bytes/packets/allow_err): org=59/1/1 reply=187/1/1 tuples=2
tx speed(Bps/kbps): 3/0 rx speed(Bps/kbps): 11/0
orgin->sink: org pre->post, reply pre->post dev=9->6/6->9 gwy=192.168.2.1/192.168.0.10
hook=post dir=org act=snat 192.168.0.10:52177->4.2.2.1:53(192.168.2.86:61770)
hook=pre dir=reply act=dnat 4.2.2.1:53->192.168.2.86:61770(192.168.0.10:52177)
dst_mac=04:d5:90:5f:a2:2a
misc=0 policy_id=2 auth_info=0 chk_client_info=0 vd=0
serial=00011061 tos=ff/ff app_list=0 app=0 url_cat=0
sdwan_mbr_seq=0 sdwan_service_id=0
rpdb_link_id=00000000 rpdb_svc_id=0 ngfwid=n/a
npu_state=0x040000
Example three
Apply an IP Pool to all traffic from port3 to port2 that have a specific original port range, mapping the ports to the same NAT'd port range. Nat all other traffic using the outgoing interface IP.
To configure from the CLI:
config firewall central-snat-map
    edit 1
        set srcintf "port3"
        set dstintf "port2"
        set orig-addr "all"
        set dst-addr "all"
        set orig-port 50000-65535
        set nat-ippool "Overload-IPPOOL"
        set nat-port 50000-65535
    next
    edit 2
        set srcintf "port3"
        set dstintf "port2"
        set orig-addr "all"
        set dst-addr "all"
    next
end
                                            To collect session table output from the CLI:
diagnose sys session list
Traffic with original port in the range between 50000-65535 will be NAT'd with the Overload type IP Pool. The mapped port is in the same port range:
session info: proto=17 proto_state=01 duration=3 expire=176 timeout=0 flags=00000000 socktype=0 sockport=0 av_idx=0 use=3
origin-shaper=
reply-shaper=
per_ip_shaper=
class_id=0 ha_id=0 policy_dir=0 tunnel=/ helper=dns-udp vlan_cos=0/255
state=may_dirty 
statistic(bytes/packets/allow_err): org=71/1/1 reply=123/1/1 tuples=2
tx speed(Bps/kbps): 23/0 rx speed(Bps/kbps): 40/0
orgin->sink: org pre->post, reply pre->post dev=9->6/6->9 gwy=192.168.2.1/192.168.0.10
hook=post dir=org act=snat 192.168.0.10:52540->4.2.2.1:53(192.168.2.201:52540)
hook=pre dir=reply act=dnat 4.2.2.1:53->192.168.2.201:52540(192.168.0.10:52540)
dst_mac=04:d5:90:5f:a2:2a
misc=0 policy_id=2 auth_info=0 chk_client_info=0 vd=0
serial=00011399 tos=ff/ff app_list=0 app=0 url_cat=0
sdwan_mbr_seq=0 sdwan_service_id=0
rpdb_link_id=00000000 rpdb_svc_id=0 ngfwid=n/a
npu_state=0x040000
Traffic with original port outside the range of 50000-65535 will be NAT'd to the outgoing interface IP:
session info: proto=6 proto_state=01 duration=3 expire=3597 timeout=3600 flags=00000000 socktype=0 sockport=0 av_idx=0 use=3
origin-shaper=
reply-shaper=
per_ip_shaper=
class_id=0 ha_id=0 policy_dir=0 tunnel=/ vlan_cos=0/255
state=may_dirty 
statistic(bytes/packets/allow_err): org=2262/10/1 reply=2526/11/1 tuples=2
tx speed(Bps/kbps): 741/5 rx speed(Bps/kbps): 828/6
orgin->sink: org pre->post, reply pre->post dev=9->6/6->9 gwy=192.168.2.1/192.168.0.10
hook=post dir=org act=snat 192.168.0.10:49805->142.250.68.66:443(192.168.2.86:62214)
hook=pre dir=reply act=dnat 142.250.68.66:443->192.168.2.86:62214(192.168.0.10:49805)
pos/(before,after) 0/(0,0), 0/(0,0)
dst_mac=04:d5:90:5f:a2:2a
misc=0 policy_id=2 auth_info=0 chk_client_info=0 vd=0
serial=0001139a tos=ff/ff app_list=0 app=0 url_cat=0
sdwan_mbr_seq=0 sdwan_service_id=0
rpdb_link_id=00000000 rpdb_svc_id=0 ngfwid=n/a
npu_state=0x040000
Protocols which do not use ports, such as ICMP, will be NAT'd to the outgoing interface IP:
session info: proto=1 proto_state=00 duration=7 expire=59 timeout=0 flags=00000000 socktype=0 sockport=0 av_idx=0 use=3
origin-shaper=
reply-shaper=
per_ip_shaper=
class_id=0 ha_id=0 policy_dir=0 tunnel=/ vlan_cos=0/255
state=may_dirty 
statistic(bytes/packets/allow_err): org=480/8/1 reply=480/8/1 tuples=2
tx speed(Bps/kbps): 66/0 rx speed(Bps/kbps): 66/0
orgin->sink: org pre->post, reply pre->post dev=9->6/6->9 gwy=192.168.2.1/192.168.0.10
hook=post dir=org act=snat 192.168.0.10:1->4.2.2.1:8(192.168.2.86:62209)
hook=pre dir=reply act=dnat 4.2.2.1:62209->192.168.2.86:0(192.168.0.10:1)
dst_mac=04:d5:90:5f:a2:2a
misc=0 policy_id=2 auth_info=0 chk_client_info=0 vd=0
serial=0001138b tos=ff/ff app_list=0 app=0 url_cat=0
sdwan_mbr_seq=0 sdwan_service_id=0
rpdb_link_id=00000000 rpdb_svc_id=0 ngfwid=n/a
npu_state=0x040000
Example four
In the following example, two central SNAT rules will be created:
- 
                                                    Rule 3 will have a destination port set and IP pool test-ippool4-3 applied.
- 
                                                    Rule 5 will have IP pool test-ippool4-1 applied but will not set the destination port.
Example traffic will then be passed to see how the rule is matched.
To test central SNAT rule destination port support:
- 
                                                    Configure central SNAT rule 3 with the destination port range specified: config firewall ippool
    edit "test-ippool4-3"
        set startip 172.16.200.150
        set endip 172.16.200.150
    next
end
config firewall central-snat-map
    edit 3
        set srcintf "port24"
        set dstintf "port17"
        set orig-addr "all"
        set dst-addr "all"
        set protocol 6
        set nat-ippool "test-ippool4-3"
        set dst-port 80-443
    next
end
- 
                                                    Configure central SNAT rule 5: config firewall ippool
    edit "test-ippool4-1"
        set startip 172.16.200.151
        set endip 172.16.200.151
    next
end
config firewall central-snat-map
    edit 5
        set srcintf "port24"
        set dstintf "port17"
        set orig-addr "all"
        set dst-addr "all"
        set nat-ippool "test-ippool4-1"
    next
end
- 
                                                    Send HTTP traffic to pass through the FortiGate that is expected to match central SNAT rule 3. IP pool test-ippool4-3 will perform source NAT.
- 
                                                    Check the session to review for expected behavior: # diagnose sys session list session info: proto=6 proto_state=01 duration=2 expire=3599 timeout=3600 flags=00000000 socktype=0 sockport=0 av_idx=0 use=3 origin-shaper= reply-shaper= per_ip_shaper= class_id=0 ha_id=0 policy_dir=0 tunnel=/ vlan_cos=0/255 state=log may_dirty f00 statistic(bytes/packets/allow_err): org=1800/31/1 reply=77304/60/1 tuples=2 tx speed(Bps/kbps): 602/4 rx speed(Bps/kbps): 25854/206 orgin->sink: org pre->post, reply pre->post dev=24->17/17->24 gwy=172.16.200.55/10.1.100.42 hook=post dir=org act=snat 10.1.100.42:46731->172.16.200.55:80(172.16.200.150:46731) hook=pre dir=reply act=dnat 172.16.200.55:80->172.16.200.150:46731(10.1.100.42:46731) pos/(before,after) 0/(0,0), 0/(0,0) misc=0 policy_id=99 pol_uuid_idx=15864 auth_info=0 chk_client_info=0 vd=0 serial=00003c37 tos=ff/ff app_list=0 app=0 url_cat=0 rpdb_link_id=00000000 ngfwid=n/a npu_state=0x4000001 no_offload no_ofld_reason: disabled-by-policy total session 1
- 
                                                    Send PING traffic to pass through the FortiGate that is expected to match central SNAT rule 5. IP pool test-ippool4-1 will perform source NAT.
- 
                                                    Check the session to review for expected behavior: # diagnose sys session list session info: proto=1 proto_state=00 duration=2 expire=59 timeout=0 flags=00000000 socktype=0 sockport=0 av_idx=0 use=3 origin-shaper= reply-shaper= per_ip_shaper= class_id=0 ha_id=0 policy_dir=0 tunnel=/ vlan_cos=0/255 state=log may_dirty f00 statistic(bytes/packets/allow_err): org=252/3/1 reply=252/3/1 tuples=2 tx speed(Bps/kbps): 99/0 rx speed(Bps/kbps): 99/0 orgin->sink: org pre->post, reply pre->post dev=24->17/17->24 gwy=172.16.200.55/10.1.100.42 hook=post dir=org act=snat 10.1.100.42:36732->172.16.200.55:8(172.16.200.151:36732) hook=pre dir=reply act=dnat 172.16.200.55:36732->172.16.200.151:0(10.1.100.42:36732) misc=0 policy_id=99 pol_uuid_idx=15864 auth_info=0 chk_client_info=0 vd=0 serial=00003f62 tos=ff/ff app_list=0 app=0 url_cat=0 rpdb_link_id=00000000 ngfwid=n/a npu_state=0x4000001 no_offload no_ofld_reason: disabled-by-policy total session 1