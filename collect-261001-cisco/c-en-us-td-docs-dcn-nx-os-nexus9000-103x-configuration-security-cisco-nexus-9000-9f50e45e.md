About ACLs
An ACL is an ordered set of rules that you can use to filter traffic. Each rule specifies a set of conditions that a packet must satisfy to match the rule. When the device determines that an ACL applies to a packet, it tests the packet against the conditions of all rules. The first matching rule determines whether the packet is permitted or denied. If there is no match, the device applies the applicable implicit rule. The device continues processing packets that are permitted and drops packets that are denied.
You can use ACLs to protect networks and specific hosts from unnecessary or unwanted traffic. For example, you could use ACLs to disallow HTTP traffic from a high-security network to the Internet. You could also use ACLs to allow HTTP traffic but only to specific sites, using the IP address of the site to identify it in an IP ACL.
ACL Types and Applications
The device supports the following types of ACLs for security traffic filtering:
- IPv4 ACLs
- The device applies IPv4 ACLs only to IPv4 traffic.
- IPv6 ACLs
- The device applies IPv6 ACLs only to IPv6 traffic.
- MAC ACLs
- The device applies MAC ACLs only to non-IP traffic.
IP and MAC ACLs have the following types of applications:
- Port ACL
- Filters Layer 2 traffic
- MAC ACL with UDF-based match
- Filters MAC ACLs with UDF-based match
- Router ACL
- Filters Layer 3 traffic
- VLAN ACL
- Filters VLAN traffic
- VTY ACL
- Filters virtual teletype (VTY) traffic
This table summarizes the applications for security ACLs.
| Table 1. Security ACL                                     		Applications |  |  | 
|---|---|---|
| Application | Supported Interfaces | Types of ACLs Supported | 
|---|---|---|
| Port ACL |  When a port ACL is applied to a trunk port, the ACL filters traffic on all VLANs on the trunk port. |  | 
| Router ACL |  |  | 
| VLAN ACL |  |  | 
| VTY ACL |  |  | 
| Note | You must enable VLAN interfaces globally before you can configure a VLAN interface. | 
| Note | MAC ACLs are supported on Layer 3 interfaces only if you enable MAC packet classification. | 
Order of ACL Application
When the device processes a packet, it determines the forwarding path of the packet. The path determines which ACLs that the device applies to the traffic. The device applies the ACLs in the following order:
-  
                                 		  
                                 Port ACL
-  
                                 		  
                                 Ingress VACL
-  
                                 		  
                                 Ingress router ACL
-  
                                 		  
                                 Ingress VTY ACL
-  
                                 		  
                                 Egress VTY ACL
-  
                                 		  
                                 Egress router ACL
-  
                                 		  
                                 Egress VACL
If the packet is bridged within the ingress VLAN, the device does not apply router ACLs.
About Rules
Rules are what you create, modify, and remove when you configure how an ACL filters network traffic. Rules appear in the running configuration. When you apply an ACL to an interface or change a rule within an ACL that is already applied to an interface, the supervisor module creates ACL entries from the rules in the running configuration and sends those ACL entries to the applicable I/O module. Depending upon how you configure the ACL, there may be more ACL entries than rules, especially if you implement policy-based ACLs by using object groups when you configure rules.
You can create rules in access-list configuration mode by using the permit or deny command. The device allows traffic that matches the criteria in a permit rule and blocks traffic that matches the criteria in a deny rule. You have many options for configuring the criteria that traffic must meet in order to match the rule.
This section describes some of the options that you can use when you configure a rule.
Protocols for IP ACLs and MAC ACLs
IPv4, IPv6, and MAC ACLs allow you to identify traffic by protocol. For your convenience, you can specify some protocols by name. For example, in an IPv4 or IPv6 ACL, you can specify ICMP by name.
You can specify any protocol by number. In MAC ACLs, you can specify protocols by the EtherType number of the protocol, which is a hexadecimal number. For example, you can use 0x0800 to specify IP traffic in a MAC ACL rule.
In IPv4 and IPv6 ACLs, you can specify protocols by the integer that represents the Internet protocol number.
Source and Destination
In each rule, you specify the source and the destination of the traffic that matches the rule. You can specify both the source and destination as a specific host, a network or group of hosts, or any host. How you specify the source and destination depends on whether you are configuring IPv4 ACLs, IPv6 ACLs, or MAC ACLs.
Implicit Rules for IP and MAC ACLs
IP and MAC ACLs have implicit rules, which means that although these rules do not appear in the running configuration, the device applies them to traffic when no other rules in an ACL match. When you configure the device to maintain per-rule statistics for an ACL, the device does not maintain statistics for implicit rules.
All IPv4 ACLs include the following implicit rule:
deny ip any any
This implicit rule ensures that the device denies unmatched IP traffic.
All IPv6 ACLs include the following implicit rule:
deny ipv6 any any
This implicit rule ensures that the device denies unmatched IPv6 traffic.
| Note |  | 
All MAC ACLs include the following implicit rule:
deny any any protocol This implicit rule ensures that the device denies the unmatched traffic, regardless of the protocol specified in the Layer 2 header of the traffic.
Additional Filtering Options
You can identify traffic by using additional options. These options differ by ACL type. The following list includes most but not all additional filtering options:
-  
                                    		  
                                    IPv4 ACLs support the following additional filtering options: 
  -  
                                          				
                                          Layer 4 protocol
  -  
                                          				
                                          TCP and UDP ports
  -  
                                          				
                                          ICMP types and codes
  -  
                                          				
                                          IGMP types
  -  
                                          				
                                          Precedence level
  -  
                                          				
                                          Differentiated Services Code Point (DSCP) value
  -  
                                          				
                                          TCP packets with the ACK, FIN, PSH, RST, SYN, or URG bit set
  -  
                                          				
                                          Established TCP connections
  -  
                                          				
                                          Packet length
-  
                                          				
                                          
-  
                                    		  
                                    IPv6 ACLs support the following additional filtering options: 
  -  
                                          				
                                          Layer 4 protocol
  -  
                                          				
                                          Encapsulating Security Payload
  -  
                                          				
                                          Payload Compression Protocol
  -  
                                          				
                                          Stream Control Transmission Protocol (SCTP)
  -  
                                          				
                                          SCTP, TCP, and UDP ports
  -  
                                          				
                                          ICMP types and codes
  -  
                                          				
                                          DSCP value
  -  
                                          				
                                          TCP packets with the ACK, FIN, PSH, RST, SYN, or URG bit set
  -  
                                          				
                                          Established TCP connections
  -  
                                          				
                                          Packet length
-  
                                          				
                                          
-  
                                    		  
                                    MAC ACLs support the following additional filtering options: 
  -  
                                          				
                                          Layer 3 protocol (Ethertype)
  -  
                                          				
                                          VLAN ID
  -  
                                          				
                                          Class of Service (CoS)
-  
                                          				
                                          
- 
                                    				
                                    Beginning Cisco NX-OS Release 9.2(4), IPv4 ACLs and IPv6 in Cisco Nexus 9500 platform switches with N9K-X96136YC-R, N9K-X9636C-R, and N9K-X9636C-RX line cards and N9K-C9504-FM-R fabric module support the following additional filtering options: 
  - 
                                          						
                                          TCP packets with the ACK, FIN, PSH, RST, SYN, or URG bit set
  - 
                                          						
                                          Established TCP connections
- 
                                          						
                                          
| Note |  | 
Sequence Numbers
The device supports sequence numbers for rules. Every rule that you enter receives a sequence number, either assigned by you or assigned automatically by the device. Sequence numbers simplify the following ACL tasks:
- Adding new rules between existing rules
- 
                                    			 
                                    By specifying the sequence number, you specify where in the ACL a new rule should be positioned. For example, if you need to insert a rule between rules numbered 100 and 110, you could assign a sequence number of 105 to the new rule.
- Removing a rule
- 
                                    			 
                                    Without using a sequence number, removing a rule requires that you enter the whole rule, as follows: switch(config-acl)# no permit tcp 10.0.0.0/8 any
However, if the same rule had a sequence number of 101, removing the rule requires only the following command: switch(config-acl)# no 101
- Moving a rule
- 
                                    			 
                                    With sequence numbers, if you need to move a rule to a different position within an ACL, you can add a second instance of the rule using the sequence number that positions it correctly, and then you can remove the original instance of the rule. This action allows you to move the rule without disrupting traffic.
If you enter a rule without a sequence number, the device adds the rule to the end of the ACL and assigns a sequence number that is 10 greater than the sequence number of the preceding rule to the rule. For example, if the last rule in an ACL has a sequence number of 225 and you add a rule without a sequence number, the device assigns the sequence number 235 to the new rule.
In addition, Cisco NX-OS allows you to reassign sequence numbers to rules in an ACL. Resequencing is useful when an ACL has rules numbered contiguously, such as 100 and 101, and you need to insert one or more rules between those rules.
Logical Operators and Logical Operation Units
IP ACL rules for TCP and UDP traffic can use logical operators to filter traffic based on port numbers. Cisco NX-OS supports logical operators in only the ingress direction.
The device stores operator-operand couples in registers called logical operator units (LOUs). The LOU usage for each type of operator is as follows:
- eq
- Is never stored in an LOU
- gt
- Uses 1 LOU
- lt
- Uses 1 LOU
- neq
- Uses 1 LOU
- range
- Uses 1 LOU
| Note | For range operators, LOU threshold configuration is used to control how the port range is expanded when configuring an ACL entry. If you want to use the LOU operator when the number of the ACL rules exceed the configured threshold value, run the following command: hardware access-list lou resource threshold <x>, wherein <x> denotes the number of ACL rules to be used before the LOU threshold is reached. The range value for <x> is 1 to 50, and the default value for LOU threshold is 5. | 
ACL Logging
The ACL logging feature monitors ACL flows and logs statistics.
A flow is defined by the source interface, protocol, source IP address, source port, destination IP address, and destination port values. The statistics maintained for a flow include the number of forwarded packets (for each flow that matches the permit conditions of the ACL entry) and dropped packets (for each flow that matches the deny conditions of the ACL entry).
Time Ranges
You can use time ranges to control when an ACL rule is in effect. For example, if the device determines that a particular ACL applies to traffic arriving on an interface, and a rule in the ACL uses a time range that is not in effect, the device does not compare the traffic to that rule. The device evaluates time ranges based on its clock.
When you apply an ACL that uses time ranges, the device updates the affected I/O module whenever a time range referenced in the ACL starts or ends. Updates that are initiated by time ranges occur on a best-effort priority. If the device is especially busy when a time range causes an update, the device may delay the update by up to a few seconds.
IPv4, IPv6, and MAC ACLs support time ranges. When the device applies an ACL to traffic, the rules in effect are as follows:
-  
                                 		  
                                 All rules without a time range specified
-  
                                 		  
                                 Rules with a time range that includes the second when the device applies the ACL to traffic
The device supports named, reusable time ranges, which allows you to configure a time range once and specify it by name when you configure many ACL rules. Time range names have a maximum length of 64 alphanumeric characters.
A time range contains one or more rules. The two types of rules are as follows:
- Absolute
-  
                                 			 
                                 A rule with a specific start date and time, specific end date and time, both, or neither. The following items describe how the presence or absence of a start or end date and time affect whether an absolute time range rule is active: 
  -  
                                       				  
                                       Start and end date and time both specified—The time range rule is active when the current time is later than the start date and time and earlier than the end date and time.
  -  
                                       				  
                                       Start date and time specified with no end date and time—The time range rule is active when the current time is later than the start date and time.
  -  
                                       				  
                                       No start date and time with end date and time specified—The time range rule is active when the current time is earlier than the end date and time.
  -  
                                       				  
                                       No start or end date and time specified—The time range rule is always active.
 For example, you could prepare your network to allow access to a new subnet by specifying a time range that allows access beginning at midnight of the day that you plan to place the subnet online. You can use that time range in ACL rules that apply to the subnet. After the start time and date have passed, the device automatically begins applying the rules that use this time range when it applies the ACLs that contain the rules.
-  
                                       				  
                                       
- Periodic
-  
                                 			 
                                 A rule that is active one or more times per week. For example, you could use a periodic time range to allow access to a lab subnet only during work hours on weekdays. The device automatically applies ACL rules that use this time range only when the range is active and when it applies the ACLs that contain the rules.
| Note | The order of rules in a time range does not affect how a device evaluates whether a time range is active. Cisco NX-OS includes sequence numbers in time ranges to make editing the time range easier. | 
Time ranges also allow you to include remarks, which you can use to insert comments into a time range. Remarks have a maximum length of 100 alphanumeric characters.
The device determines whether a time range is active as follows:
-  
                                 		  
                                 The time range contains one or more absolute rules—The time range is active if the current time is within one or more absolute rules.
-  
                                 		  
                                 The time range contains one or more periodic rules—The time range is active if the current time is within one or more periodic rules.
-  
                                 		  
                                 The time range contains both absolute and periodic rules—The time range is active if the current time is within one or more absolute rules and within one or more periodic rules.
When a time range contains both absolute and periodic rules, the periodic rules can only be active when at least one absolute rule is active.
Policy-Based ACLs
The device supports policy-based ACLs (PBACLs), which allow you to apply access control policies across object groups. An object group is a group of IP addresses or a group of TCP or UDP ports. When you create a rule, you specify the object groups rather than specifying IP addresses or ports.
Using object groups when you configure IPv4 or IPv6 ACLs can help reduce the complexity of updating ACLs when you need to add or remove addresses or ports from the source or destination of rules. For example, if three rules reference the same IP address group object, you can add an IP address to the object instead of changing all three rules.
PBACLs do not reduce the resources required by an ACL when you apply it to an interface. When you apply a PBACL or update a PBACL that is already applied, the device expands each rule that refers to object groups into one ACL entry per object within the group. If a rule specifies the source and destination both with object groups, the number of ACL entries created on the I/O module when you apply the PBACL is equal to the number of objects in the source group multiplied by the number of objects in the destination group.
The following object group types apply to port, router, policy-based routing (PBR), and VLAN ACLs:
- IPv4 Address Object Groups
-  
                                 			 
                                 Can be used with IPv4 ACL rules to specify source or destination addresses. When you use the permit or deny command to configure a rule, the addrgroup keyword allows you to specify an object group for the source or destination.
- IPv6 Address Object Groups
-  
                                 			 
                                 Can be used with IPv6 ACL rules to specify source or destination addresses. When you use the permit or deny command to configure a rule, the addrgroup keyword allows you to specify an object group for the source or destination.
- Protocol Port Object Groups
-  
                                 			 
                                 Can be used with IPv4 and IPv6 TCP and UDP rules to specify source or destination ports. When you use the permit or deny command to configure a rule, the portgroup keyword allows you to specify an object group for the source or destination.
| Note | Policy-based routing (PBR) ACLs do not support deny access control entries (ACEs) or deny commands to configure a rule. | 
Kernel Stack ACL
The Kernel Stack ACL is a common CLI infrastructure to configure ACLs for management of inband and outband components.
The Kernel Stack ACL uses NX-OS ACL CLI to secure management applications on management and front panel ports. Configuring a single ACL must be able to secure all management applications on NX-OS.
Kernel Stack ACL is the component that fixes the manual intervention of the user and automatically programs iptable entries when the ACL is applied to mgmt0 interface.
The following is an example for configuring Kernel Stack ACL:
swtich# conf t
Enter configuration commands, one per line. End with CNTL/Z.
switch(config)# ip access-list kacl1
switch(config-acl)# statistics per-entry
switch(config-acl)# 10 deny tcp any any eq 443
switch(config-acl)# 20 permit ip any any
switch(config-acl)# end
switch#
switch(config-if)# interface mgmt0
switch(config-if)#   ip access-group acl1 in
switch(config-if)#   ipv6 traffic-filter acl6 in
switch(config-if)#
switch# sh ip access-lists kacl1
IP access list kacl1
statistics per-entry
10 deny tcp any any eq 443 [match=136]
20 permit ip any any [match=44952]
switch(config)#
The following is the Kernel Stack filtering for iptables entries based on the configuration:
bash-4.4# ip netns exec management iptables -L -n -v --line-numbers
Chain INPUT (policy ACCEPT 0 packets, 0 bytes)
num pkts bytes target prot opt in out source destination
1 9 576 DROP tcp -- * * 0.0.0.0/0 0.0.0.0/0 tcp dpt:443
2 0 0 ACCEPT all -- * * 0.0.0.0/0 0.0.0.0/0
3 0 0 DROP all -- * * 0.0.0.0/0 0.0.0.0/0
Chain FORWARD (policy ACCEPT 0 packets, 0 bytes)
num pkts bytes target prot opt in out source destination
Chain OUTPUT (policy ACCEPT 0 packets, 0 bytes)
num pkts bytes target prot opt in out source destination
bash-4.4#
The following are the limitations for the Kernel Stack ACL support:
- 
                                 
                                 This feature is supported only on mgmt0 interface and not on other inband interfaces.
- 
                                 
                                 Five tuples (protocol, source-ip, destination-ip, source-port, and destination-port) of the ACL entry are programmed in the iptables. Rest of the options provided in the ACL entry are not programmed in the iptables and throws a warning syslog in such instances. For example, "WARNING: Some ACL options are not supported in kstack. Only partial rule will be installed".
- 
                                 
                                 If the device user has host bash access, then the user can manually update the iptables. This update could potentially corrupt the iptable rules for which they are programmed.
- 
                                 
                                 The verified maximum number of ACEs is 100 for IPv4 traffic and an additional 100 for IPv6 traffic. Throughput may be impacted if more than this scale is applied.
Statistics and ACLs
The device can maintain global statistics for each rule that you configure in IPv4, IPv6, and MAC ACLs. If an ACL is applied to multiple interfaces, the maintained rule statistics are the sum of packet matches (hits) on all the interfaces on which that ACL is applied.
| Note | The device does not support interface-level ACL statistics. | 
For each ACL that you configure, you can specify whether the device maintains statistics for that ACL, which allows you to turn ACL statistics on or off as needed to monitor traffic filtered by an ACL or to help troubleshoot the configuration of an ACL.
The device does not maintain statistics for implicit rules in an ACL. For example, the device does not maintain a count of packets that match the implicit deny ip any any rule at the end of all IPv4 ACLs. If you want to maintain statistics for implicit rules, you must explicitly configure the ACL with rules that are identical to the implicit rules.
Atomic ACL Updates
By default, when a supervisor module of a Cisco Nexus 9000 Series device updates an I/O module with changes to an ACL, it performs an atomic ACL update. An atomic update does not disrupt traffic that the updated ACL applies to; however, an atomic update requires that an I/O module that receives an ACL update has enough available resources to store each updated ACL entry in addition to all pre-existing entries in the affected ACL. After the update occurs, the additional resources used for the update are freed. If the I/O module lacks the required resources, the device generates an error message and the ACL update to the I/O module fails.
If an I/O module lacks the resources required for an atomic update, you can disable atomic updates by using the no hardware access-list update atomic command; however, during the brief time required for the device to remove the preexisting ACL and implement the updated ACL, traffic that the ACL applies to is dropped by default.
If you want to permit all traffic that an ACL applies to while it receives a nonatomic update, use the hardware access-list update default-result permit command.
This example shows how to disable atomic updates to ACLs:
switch# config t 
switch(config)# no hardware access-list update atomic
This example shows how to permit affected traffic during a nonatomic ACL update:
switch# config t 
switch(config)# hardware access-list update default-result permit
This example shows how to revert to the atomic update method:
switch# config t 
switch(config)# no hardware access-list update default-result permit 
switch(config)# hardware access-list update atomic
Session Manager Support for IP ACLs
Session Manager supports the configuration of IP and MAC ACLs. This feature allows you to verify ACL configuration and confirm that the resources required by the configuration are available prior to committing them to the running configuration.
ACL TCAM Regions
You can change the size of the ACL ternary content addressable memory (TCAM) regions in the hardware.
On Cisco Nexus 9300 and 9500 Series switches, the egress TCAM size is 1K, divided into four 256 entries. On other Cisco Nexus 9300 and 9500 Series switches, the ingress TCAM size is 4K, divided into eight 256 slices and four 512 slices. A slice is the unit of allocation. A slice can be allocated to one region only. For example, a 512-size slice cannot be used to configure two features of size 256 each. Similarly, a 256-size slice cannot be used to configure two features of size 128 each. The IPv4 TCAM regions are single wide. The IPv6, QoS, MAC, CoPP, and system TCAM regions are double wide and consume double the physical TCAM entries. For example, a logical region size of 256 entries actually consumes 512 physical TCAM entries.
You can create IPv6, port ACLs, VLAN ACLs, and router ACLs, and you can match IPv6 and MAC addresses for QoS. However, Cisco NX-OS cannot support all of them simultaneously. You must remove or reduce the size of the existing TCAM regions (TCAM carving) to enable the IPv6, MAC, or other desired TCAM regions. For every TCAM region configuration command, the system evaluates if the new change can be fit in the TCAM. If not, it reports an error, and the command is rejected. You must remove or reduce the size of existing TCAM regions to make room for new requirements.
On Cisco Nexus 9200 Series switches, the egress TCAM size is 2K, and the ingress TCAM size is 4K. The concepts of TCAM slices and single- and double-wide regions do not apply to these switches. For example, the ing-ifacl region can host IPv4, IPv6, or MAC type entries. IPv4 and MAC types occupy one TCAM entry whereas IPv6 types occupy two TCAM entries.
For N9K-X9636C-RX, when PACL uses external TCAM region, the internal TCAM needs to take 2K for ifacl and the ingress RACL-IPv4 can take upto 2044. Additional four entries are required when egress PACL external TCAM region is used.
ACL TCAM region sizes have the following guidelines and limitations:
- 
                                 				
                                 To enable RACL or PACL on existing TCAM regions, you must carve the TCAM region beyond 12, 288.
- 
                                 				
                                 On Cisco Nexus 9300 Series switches, the X9536PQ, X9564PX, and X9564TX line cards are used to enforce the QoS classification policies applied on 40G ports. It has 768 TCAM entries available for carving in 256-entry granularity. These region names are prefixed with "ns-".
- 
                                 				
                                 For the X9536PQ, X9564PX, and X9564TX line cards, only the IPv6 TCAM regions consume double-wide entries. The rest of the TCAM regions consume single-wide entries.
- 
                                 				
                                 When a VACL region is configured, it is configured with the same size in both the ingress and egress directions. If the region size cannot fit in either direction, the configuration is rejected.
- 
                                 				
                                 				
                                 On Cisco Nexus 9200 Series switches, the ing-sup region occupies a minimum size of 512 entries, and the egr-sup region occupies a minimum size of 256 entries. These regions cannot be configured to lesser values. Any region size can be carved with a value only in multiples of 256 entries (with the exception of the span region, which can be carved only in multiples of 512 entries).
- 
                                 				
                                 RACL v6, CoPP, and multicast have default TCAM sizes and these TCAM sizes must be non-zero on the following Cisco Nexus 9504 and Cisco Nexus 9508 line cards to avoid line card failure during reload: 
  - 
                                       						
                                       N9K-X96136YC-R
  - 
                                       						
                                       N9K-X9636C-RX
  - 
                                       						
                                       N9K-X9636Q-R
  - 
                                       						
                                       N9K-X9636C-R
- 
                                       						
                                       
- 
                                 				
                                 When the egress RACL is beyond 4K, the TCAM carving configuration has to be ingress RACL (RACL) + egress RACL (e-racl) summing to 20480. See the following TCAM carving example: hardware access-list tcam region ifacl 0
hardware access-list tcam region ipv6-ifacl 0
hardware access-list tcam region mac-ifacl 0
hardware access-list tcam region racl 0
hardware access-list tcam region ipv6-racl 0
hardware access-list tcam region span 0
hardware access-list tcam region redirect_v4 0
hardware access-list tcam region redirect_v6 0
hardware access-list tcam region e-racl 20480
- 
                                 				
                                 You can partially use IPv6 RACL with IPv6 IFCAL. This is applicable to Cisco Nexus N9K-C9508 and N9K-C9504 with N9K-X96136YC-R, N9K-X9636C-R, N9K-X9636Q-R, and N9K-X9636C-RX line cards.
- 
                                 				
                                 The N9K-X9636C-R and N9K-X9636Q-R line cards support a maximum TCAM region size of 12K. If you configure a greater number, the TCAM region is set to 12K.
- 
                                 				
                                 The N9K-X96136YC-R and N9K-X9636C-R line cards support egress RACL of 2K.
- 
                                 				
                                 The N9K-X9636C-RX line card supports a TCAM region size beyond 12K. If you configure the RACL IPv4 TCAM region to 100K, the TCAM region is set to 12K for the N9K-X9636C-R and N9K-X9636Q-R line cards and to 100K for the N9K-X9636C-RX line card, provided you have set all of the other TCAM regions and made space for the N9K-X9636C-R and N9K-X9636Q-R line cards to accommodate 12K.
- 
                                 				
                                 Beginning with Cisco NX-OS Release 10.2(2)F, The N9K-X9636C-R and N9K-X9636Q-R line cards support a maximum TCAM region size of 20K. If you configure a greater number, the TCAM region is re-set to 20K.
- 
                                 				
                                 In addition to the internal TCAM, an external TCAM of 128K is available on the N9K-X9636C-RX line card.
The following table summarizes the regions that need to be configured for a given feature to work. The region sizes should be selected based on the scale requirements of a given feature.
| Table 2. Features per ACL                                     		TCAM Region |  | 
|---|---|
| Feature Name | Region Name | 
|---|---|
| Port ACL | ifacl: For IPv4 port ACLs ifacl-udf: For UDFs on IPv4 port ACLs ing-ifacl: For ingress IPv4, IPv6, and MAC port ACLs ing-ifacl: For ingress IPv4, IPv6, MAC port ACLs, and MAC port ACLs with UDF ipv6-ifacl: For IPv6 port ACLs mac-ifacl: For MAC port ACLs | 
| Port QoS (QoS classification policy applied on Layer 2 ports or port channels) | qos, qos-lite, rp-qos, rp-qos-lite, ns-qos, e-qos, or e-qos-lite: For classifying IPv4 packets ing-l2-qos: For classifying ingress Layer 2 packets ipv6-qos, rp-ipv6-qos, ns-ipv6-qos, or e-ipv6-qos: For classifying IPv6 packets mac-qos, rp-mac-qos, ns-mac-qos, or e-mac-qos: For classifying non-IP packets | 
| VACL | vacl: For IPv4 packets ipv6-vacl: For IPv6 packets mac-vacl: For non-IP packets | 
| VLAN QoS (QoS classification policy applied on a VLAN) | vqos or ns-vqos: For classifying IPv4 packets ipv6-vqos or ns-ipv6-vqos: For classifying IPv6 packets ing-l3-vlan-qos: For classifying ingress Layer 3, VLAN, and SVI QoS packets mac-vqos or ns-mac-vqos: For classifying non-IP packets | 
| RACL | egr-racl: For egress IPv4 and IPv6 RACLs e-racl: For egress IPv4 RACLs e-ipv6-racl: For egress IPv6 RACLs ing-racl: For ingress IPv4 and IPv6 RACLs racl: For IPv4 RACLs racl-lite: For IPv4 RACLs racl-udf: For UDFs on IPv4 RACLs ipv6-racl: For IPv6 RACLs | 
| Layer 3 QoS (QoS classification policy applied on Layer 3 ports or port channels) | l3qos, l3qos-lite, or ns-l3qos: For classifying IPv4 packets ipv6-l3qos or ns-ipv6-l3qos: For classifying IPv6 packets | 
| VLAN source or VLAN filter SPAN (for Cisco Nexus 9500 or 9300 Series switches) Rx SPAN on 40G ports (for Cisco Nexus 9300 Series switches only) | span | 
| SPAN filters | ifacl: For filtering IPv4 traffic on Layer 2 (switch port) source interfaces. ifacl-udf: For UDFs on IPv4 port ACLs ipv6-ifacl: For filtering IPv6 traffic on Layer 2 (switch port) source interfaces. mac-ifacl: For filtering Layer 2 traffic on Layer 2 (switch port) source interfaces. racl-udf: For UDFs on IPv4 RACLs vacl: For filtering IPv4 traffic on VLAN sources. ipv6-vacl: For filtering IPv6 traffic on VLAN sources. mac-vacl: For filtering Layer 2 traffic on VLAN sources. racl: For filtering IPv4 traffic on Layer 3 interfaces. ipv6-racl: For filtering IPv6 traffic on Layer 3 interfaces. ing-l2-span-filter: For filtering ingress Layer 2 SPAN traffic ing-l3-span-filter: For filtering ingress Layer 3 and VLAN SPAN traffic | 
| SVI counters | svi | 
| BFD, DHCP relay, or DHCPv6 relay | redirect | 
| CoPP | copp | 
| System-managed ACLs | system | 
| vPC convergence | vpc-convergence | 
| Fabric extender (FEX) | fex-ifacl, fex-ipv6-ifacl, fex-ipv6-qos, fex-mac-ifacl, fex-mac-qos, fex-qos, fex-qos-lite | 
| Dynamic ARP inspection (DAI) | arp-ether | 
| IP source guard (IPSG) | ipsg | 
| Multicast PIM Bidir | mcast_bidir | 
| Static MPLS | mpls | 
| Network address translation (NAT) | nat | 
| NetFlow | ing-netflow | 
| OpenFlow | openflow | 
| sFlow | sflow | 
| Supervisor modules | egr-sup: Egress supervisor ing-sup: Ingress supervisor | 
| Policy-Based Routing (PBR) | ing-racl: For matching ingress L3 traffic for PBR. | 
| Layer 2 Intelligent Traffic Director (ITD) | vacl: Programs L2 redirect ACLs at the VLAN level. | 
| Layer 3 Intelligent Traffic Director (ITD) | ing-racl: Programs L3 redirect ACLs for ITD. | 
| Enhanced Policy-Based Redirect at L2 (ePBR) | ing-ifacl: Programs L2 redirect ACLs for ePBR L2. | 
| Enhanced Policy-Based Redirect at L3 (ePBR) | ing-racl: Programs L3 redirect ACLs for ePBR L3. | 
| Note |  | 
| Note |  | 
| Note |  | 
| Note |  | 
| Note |  | 
| Note |  | 
| Note |  | 
| Note |  | 
| Note |  | 
Maximum Label Sizes Supported for ACL Types
Cisco NX-OS switches support the following label sizes for the corresponding ACL types:
| Table 3. ACL Types and Maximum Label Sizes |  |  |  | 
|---|---|---|---|
| ACL Types | Direction | Label | Label Type | 
|---|---|---|---|
| RACL/PBR/VACL/ L3-VLAN QoS/L3-VLAN SPAN ACL | Ingress | 62 | BD | 
| PACL/L2 QoS/L2 SPAN ACL | Ingress |  | IF | 
| RACL/VACL/L3-VLAN QoS | Egress | 254 | BD | 
| L2 QoS | Egress | 31 | IF | 
The label size can be increased to 62 when you enter the hardware access-list tcam label ing-ifacl 6 command and reload the switch.
Beginning with Cisco NX-OS Release 9.3(6), the hardware access-list tcam label ing-ifacl 6 command is introduced and is applicable only for Cisco Nexus 9300-FX platform switches.
Beginning with Cisco NX-OS Release 10.1(2), the hardware access-list tcam label ing-ifacl 6 command is also supported on Cisco Nexus 9300-FX2 platform switches.