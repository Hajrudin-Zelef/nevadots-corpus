I don't believe that there is anything simpler than show interfaces | <some regex> unfortunately.
Edit:
From the comments below, @Santino pointed out a more concise RegEx:
show ip interface | include line protocol|access list
My testing so far indicates that this gives the same results as my longer RegEx below.
I usually use the following to find where ACLs are applied:
show ip interface | include is up|is administratively|is down|Outgoing|Inbound
This gives you every interface, no matter the state, then what the Outbound and Inbound ACL's are.  For example:
LAB-4510-A#show ip interface | include is up|is administratively|is down|Outgoing|Inbound 
Vlan1 is administratively down, line protocol is down
Vlan110 is up, line protocol is up
  Outgoing access list is not set
  Inbound  access list is VENDOR->INTERNET
Vlan140 is administratively down, line protocol is down
  Outgoing access list is not set
  Inbound  access list is not set
Vlan150 is down, line protocol is down
  Outgoing access list is not set
  Inbound  access list is VENDOR->INTERNET
Vlan210 is up, line protocol is up
  Outgoing access list is not set
  Inbound  access list is not set
FastEthernet1 is administratively down, line protocol is down
  Outgoing access list is not set
  Inbound  access list is not set
GigabitEthernet1/1 is up, line protocol is up
  Outgoing access list is not set
  Inbound  access list is not set
GigabitEthernet1/2 is down, line protocol is down
  Inbound  access list is not set
  Outgoing access list is not set
And so on for every interface.
This command works on both Cisco switches and routers.  See sample output from a 7200 Router below:
LAB-7204-A#show ip interface | include is up|is administratively|is down|Outgoing|Inbound
GigabitEthernet0/1 is up, line protocol is up
  Outgoing access list is not set
  Inbound  access list is not set
FastEthernet0/2 is administratively down, line protocol is down
GigabitEthernet0/2 is up, line protocol is up
  Outgoing access list is not set
  Inbound  access list is not set
GigabitEthernet0/3 is administratively down, line protocol is down
SSLVPN-VIF0 is up, line protocol is up
  Outgoing access list is not set
  Inbound  access list is not set
Loopback0 is up, line protocol is up
  Outgoing access list is not set
  Inbound  access list is not set
Loopback1 is up, line protocol is up
  Outgoing access list is not set
  Inbound  access list is not set