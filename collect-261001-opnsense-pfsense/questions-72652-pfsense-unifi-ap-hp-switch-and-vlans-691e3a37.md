I have a Unifi AP with a single MYNET SSID connected to my HP 2520 switch on port 23. I have a pfSense LAN interface connected on port 1. AP clients get DHCP (192.168.1.0/24) from pfSense and can access Internet, etc. I have static leases for all my "known" devices but also a DHCP pool so new devices can get on to get configured.
I have OpenVPN (client) configured on pfSense and policy route ports 443, etc over it. Some websites drop VPN traffic though, so thought I'd setup a "vpn bypass" SSID On the AP and give it a completely different subnet. I could tell pfSense to just route the entire network out the WAN interface instead.
I added vlan 200 on the AP and gave it NOVPN for an SSID. On pfSense, I followed some tutorials and setup vlan 200 on the LAN interface. I have the vlan interface enabled, added the firewall "pass" rule for the it. Then, setup DHCP for a completely different subnet (192.168.2.0/24). I don't have static leases configured for it yet, if it matters.
When a client leaves MYNET and connects to NOVPN, the client doesn't get an IP. tcpdump shows the DHCP request come in from the client, and pfSense replies, but that's all I see going on. On the client, the connection times out/fails. On pfSense, I see a DHCP reply going to the client's static lease for MYNET. The reply is originating from x.x.1.1 (pfSense LAN interface). I was expecting it to come from x.x.2.1 which is the vlan 200 address for pfSense.
11:44:16.625014 IP 192.168.1.1.67 > 192.168.1.66.68: BOOTP/DHCP, Reply, length 300
I think this might be happening because I don't have any VLANs on my switch maybe? I tried setting up vlan 200 on my switch but got lost with tagged/untagged/GVRP, etc. Do I need to trunk/tag the AP and pfSense ports somehow since they will be carrying multiple VLAN traffic? I tried a few things from this HP link but couldn't quite figure out what was needed to make this work. Any thoughts?
Current HP ports assignments:
===========================- TELNET - MANAGER MODE -============================
               Switch Configuration - VLAN - VLAN Port Assignment
  Port   DEFAULT_VLAN  no_vpn_vlan    |  Port   DEFAULT_VLAN  no_vpn_vlan
  ---- + ------------  ------------   |  ---- + ------------  ------------
  1    | No            Untagged       |  13   | No            Untagged
  2    | No            Untagged       |  14   | No            Untagged
  3    | No            Untagged       |  15   | No            Untagged
  4    | No            Untagged       |  16   | No            Untagged
  5    | No            Untagged       |  17   | No            Untagged
  6    | No            Untagged       |  18   | No            Untagged
  7    | No            Untagged       |  19   | No            Untagged
  8    | No            Untagged       |  20   | No            Untagged
  9    | No            Untagged       |  21   | No            Untagged
  10   | No            Untagged       |  22   | No            Untagged
  11   | No            Untagged       |  23   | No            Untagged
  12   | No            Untagged       |  24   | No            Untagged
 Actions->   Cancel     Edit     Save     Help
Port/Trunk settings (all ports are set the same as #1):
  Port    Type      Enabled      Mode      Flow Ctrl  Group  Type
  ----  --------- + -------  ------------  ---------  -----  -----
  1     1000T     | Yes      Auto          Disable
  ...
UPDATE: adding switch configs below:
running config
switch-2520G# show run
Running configuration:
; J9299A Configuration Editor; Created on release #J.14.54
hostname "switch-2520G" 
vlan 1 
   name "DEFAULT_VLAN" 
   no untagged 1-24 
   no ip address 
   exit 
vlan 200 
   name "no_vpn_vlan" 
   untagged 1-24 
   ip address 192.168.1.10 255.255.255.0 
   exit 
auto-tftp 192.168.100.120 "/tftp"
banner motd "HP SWITCH
"
include-credentials
password manager user-name "x..." sha1 "x..."
no telnet-server
ip authorized-managers 192.168.1.220 255.255.255.255 access manager
ip authorized-managers 172.22.200.220 255.255.255.0 access manager
ip authorized-managers 192.168.1.220 255.255.255.255 access manager
ip ssh public-key operator "ssh-rsa ..."
ip ssh public-key operator "ssh-rsa ..."
snmp-server community "x..." operator
snmpv3 engineid "xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx"
aaa authentication ssh login public-key
no tftp server
IP info
switch-2520G# show ip
 Internet (IP) Service
  Default Gateway :                
  Default TTL     : 64   
  Arp Age         : 20  
  Domain Suffix   :                               
  DNS server      :                                         
  VLAN                 | IP Config  IP Address      Subnet Mask     Proxy ARP
  -------------------- + ---------- --------------- --------------- ---------
  DEFAULT_VLAN         | Disabled 
  no_vpn_vlan          | Manual     192.168.1.1      255.255.255.0   No
 
VLAN info
switch-2520G# show vlan
 Status and Counters - VLAN Information
  Maximum VLANs to support : 256                  
  Primary VLAN : DEFAULT_VLAN
  Management VLAN :             
  VLAN ID Name                 Status       Voice Jumbo
  ------- -------------------- ------------ ----- -----
  1       DEFAULT_VLAN         Port-based   No    No   
  200     no_vpn_vlan          Port-based   No    No   
 
VLAN 1 (DEFAULT_VLAN)
switch-2520G# show vlan 1
 Status and Counters - VLAN Information - VLAN 1
  VLAN ID : 1      
  Name : DEFAULT_VLAN        
  Status : Port-based  Voice : No 
  Jumbo : No 
  Port Information Mode     Unknown VLAN Status    
  ---------------- -------- ------------ ----------
 
  Overridden Port VLAN configuration
  Port Mode        
  ---- ------------
 
VLAN 200 (no_vpn_vlan)
switch-2520G# show vlan 200
 Status and Counters - VLAN Information - VLAN 200
  VLAN ID : 200    
  Name : no_vpn_vlan         
  Status : Port-based  Voice : No 
  Jumbo : No 
  Port Information Mode     Unknown VLAN Status    
  ---------------- -------- ------------ ----------
  1                Untagged Learn        Up        
  2                Untagged Learn        Down      
  3                Untagged Learn        Down      
  4                Untagged Learn        Down      
  5                Untagged Learn        Down      
  6                Untagged Learn        Down      
  7                Untagged Learn        Down      
  8                Untagged Learn        Down      
  9                Untagged Learn        Down      
  10               Untagged Learn        Down      
  11               Untagged Learn        Up        
  12               Untagged Learn        Down      
  13               Untagged Learn        Up        
  14               Untagged Learn        Down      
  15               Untagged Learn        Down      
  16               Untagged Learn        Up        
  17               Untagged Learn        Down      
  18               Untagged Learn        Down      
  19               Untagged Learn        Down      
  20               Untagged Learn        Down      
  21               Untagged Learn        Up        
  22               Untagged Learn        Down      
  23               Untagged Learn        Up        
  24               Untagged Learn        Down      
VLAN port assignment
  Port   DEFAULT_VLAN  no_vpn_vlan    |  Port   DEFAULT_VLAN  no_vpn_vlan
  ---- + ------------  ------------   |  ---- + ------------  ------------
  1    | No            Untagged       |  13   | No            Untagged
  2    | No            Untagged       |  14   | No            Untagged
  3    | No            Untagged       |  15   | No            Untagged
  4    | No            Untagged       |  16   | No            Untagged
  5    | No            Untagged       |  17   | No            Untagged
  6    | No            Untagged       |  18   | No            Untagged
  7    | No            Untagged       |  19   | No            Untagged
  8    | No            Untagged       |  20   | No            Untagged
  9    | No            Untagged       |  21   | No            Untagged
  10   | No            Untagged       |  22   | No            Untagged
  11   | No            Untagged       |  23   | No            Untagged
  12   | No            Untagged       |  24   | No            Untagged
UPDATE-2/SOLUTION (as provided by Zac67):
Change IP addresses for both VLANS: Switch Configuration - Internet (IP) Service
  Default Gateway :                
  Default TTL     : 64   
  Arp Age         : 20  
          VLAN           IP Config     IP Address       Subnet Mask
  -------------------- + ----------  ---------------  ---------------
  DEFAULT_VLAN         | Manual      192.168.1.1      255.255.255.0
  no_vpn_vlan          | Manual      192.168.2.1      255.255.255.0
Correct VLAN tags:
               Switch Configuration - VLAN - VLAN Port Assignment
  Port   DEFAULT_VLAN  no_vpn_vlan    |  Port   DEFAULT_VLAN  no_vpn_vlan
  ---- + ------------  ------------   |  ---- + ------------  ------------
  1    | Untagged      Tagged         |  13   | Untagged      No
  2    | Untagged      No             |  14   | Untagged      No
  3    | Untagged      No             |  15   | Untagged      No
  4    | Untagged      No             |  16   | Untagged      No
  5    | Untagged      No             |  17   | Untagged      No
  6    | Untagged      No             |  18   | Untagged      No
  7    | Untagged      No             |  19   | Untagged      No
  8    | Untagged      No             |  20   | Untagged      No
  9    | Untagged      No             |  21   | Untagged      No
  10   | Untagged      No             |  22   | Untagged      No
  11   | Untagged      No             |  23   | Untagged      Tagged
  12   | Untagged      No             |  24   | Untagged      No
192./162/.1.66above a typo or deliberate? Note that 192.162.0.0/22 is a Russian network and not private.{}).