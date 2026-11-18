    Cheatsheet Huawei/Cisco Commands                                                                                                                                            Copyright @ https://www.netcamp.ch by Philip Schaller




                                      Basic                                                                          Routing                                                                             Switching

           Huawei VRP 5                                   Cisco IOS                         Huawei VRP 5                                      Cisco IOS                         Huawei VRP 5                                       Cisco IOS
system-view                               configure terminal                     display ip interface brief                   show ip interface brief                display vlan                                show vlan
reboot                                    reload                                 display ipv6 interface brief                 show ipv6 interface brief              display mac-address                         show mac address-table
save                                      copy running-config startup-config     display ip routing-table                     show ip route                          display stp                                 show spanning-tree
display logbuffer                         show logging                           display ipv6 routing-table                   show ipv6 route                        display stp region-configuration            show spanning-tree mst configuration
display version                           show version                           display arp                                  show arp                               display stp interface X                     show spanning-tree mst interface X
display diagnostic-information            show tech-support                      display ospf brief                           show ip ospf summary                   display stp bridge root                     show spanning-tree root
display current-configuration             show running-config                    display ospf peer                            show ip ospf neighbor                  display interface description               show interface status
display saved-configuration               show startup-config                    display ospf interface                       show ip ospf interface                 display port vlan                           show interfaces trunk
display cpu-usage                         show processes cpu                     reset ospf                                   clear ip opsf process                  display poe power                           show power inline
display memory-usage                      show processes memory                  display acl all                              show ip access-lists                   poe enable (interface view)                 power inline auto (interface view)
display temperature all                   show environment temperature           ip route-static X                            ip route X                             stp mode mstp                               spanning-tree mode mst
display history-command                   show history                           reset arp dynamic                            clear arp dynamic                      stp mode rstp                               spanning-tree mode rapid-pvst
display ntp-service status                show ntp status                        display route-policy                         show route-map                         stp edged-port enable                       spanning-tree portfast
reset counters                            clear counters                         reset ip routing-table statistics protocol   clear route *                          stp edged-port enable                       spanning-tree portfast trunk
reset saved-configuration                 write erase                            display bgp peer                             show ip bgp neighbors                  port negotiation disable                    switchport nonegotiate
debugging / undo debugging                debug / no debug                       reset bgp all                                clear ip bgp                           undo portswitch                             no switchport
tracert                                   traceroute                             display ip routing-table protocol bgp        show ip route bgp                      stp bpdu-protection                         spanning-tree bpduguard enable
display lldp neighbor brief               show lldp neighbors                    display vrrp brief                           show vrrp brief                        stp bpdu filter enable                      spanning-tree bpdufilter enable
display cdp neighbor brief                show cdp neighbors                     display vrrp statistics                      show vrrp statistics                   stp loop-protection                         spanning-tree guard loop
display eth-trunk                         show etherchannel summary              display isis peer                            show isis neighbors                    stp root-protection                         spanning-tree guard root
schedule reboot delay X                   reload in X                            display pim neighbor                         show ip pim neighbor                   dhcp snooping trusted                       ip dhcp snooping trust
display schedule reboot                   show reload                            display pim interface                        show ip pim interface                  port link-type access                       switchport mode access
kill user-interface vty                   clear line vty                         display igmp interface                       show ip igmp interface                 port default vlan X                         switchport access vlan X
return                                    end                                    display nqa history                          show ip sla history                    port link-type trunk                        switchport mode trunk
quit                                      exit                                   display nqa results                          show ip sla statistics                 port trunk allow-pass vlan all              switchport trunk allowed vlan all
undo http server enable                   no ip http server                      ip vpn-instance X                            ip vrf X                               port trunk pvid vlan X                      switchport trunk native vlan X
undo http secure-server enable            no ip http secure-server               ip binding vpn-instance X                    ip vrf forwarding X                    port link-type dot1q-tunnel                 switchport mode dot1q-tunnel
lldp enable                               lldp run                               ip route-static vpn-instance X               ip route vrf X                         voice-vlan X enable (interface view)        switchport voice vlan X (interface view)
info-center loghost x.x.x.x               logging x.x.x.x                        display ip routing-table vpn-instance X      show ip route vrf X                    virtual-cable-test (interface view)         test cable-diagnostics tdr interface X
header login information «X»              banner login «X»                       display ip ip-prefix                         show ip prefix-list detail             display counters error                      show interface counters errors
header shell information «X»              banner exec «X»                        acl name X basic                             ip access-list standard X              display port-mirroring                      show monitor session all
rsa local-key-pair create                 crypto key generate rsa                acl name X advance                           ip access-list extended X              storm-control action error-down             storm-control action shutdown
dns resolve                               ip domain-lookup                       step X (within ACL)                          ip access-list resequence X            display storm-control                       show storm-control
dns domain X                              ip domain-name X                       traffic classifier X                         class-map X                            vlan batch X to Y                           vlan X-Y
dns server X                              ip name-server X                       traffic policy X                             policy-map X                           igmp-snooping enable                        ip igmp snooping
sysname X                                 hostname X                             traffic-policy X inbound                     service-policy input X                 dot1x                                       dot1x system-auth-control
ntp-service unicast-server X              ntp server X                           ipv6 nd ra halt (interface view)             ipv6 nd suppress-ra (interface view)
ntp-service refclock-master X             ntp master X                           multicast routing-enable                     ip multicast-routing
scp server enable                         ip scp server enable
snmp-agent community read cipher X        snmp-server community X RO
snmp-agent community write cipher X       snmp-server community X RW
display snmp-agent statistics             show snmp
local-user X password cipher X (in AAA)   username X password X
aaa                                       aaa new-model
user-interface vty 0 4                    line vty 0 14
protocol inbound ssh (VTY view)           transport input ssh (VTY view)
dhcp enable                               service dhcp
dhcp relay server-ip X (interface view)   ip helper-address X (interface view)
undo shutdown (interface view)            no shutdown (interface view)
display bfd session all verbose           show bfd session detail
startup system-software X.cc              boot system flash:X.bin
auto speed (interface view)
auto duplex (interface view)
                                          speed auto (interface view)
                                          duplex auto (interface view)                                                                                               You can find more Cheatsheets, Templates, Courses,
jumbo enable X (interface view)
dldp enable
                                          system mtu jumbo X
                                          udld enable                                                                                                                Guides, eBooks, Exam Quizzes, etc. for your continuing
                                                                                                                                                                     education on our website. Scan the QR code to access
                                                                                                                                                                     them.




                                                                                                                                                                                                                                                Version 2.0 (03.01.2026)
