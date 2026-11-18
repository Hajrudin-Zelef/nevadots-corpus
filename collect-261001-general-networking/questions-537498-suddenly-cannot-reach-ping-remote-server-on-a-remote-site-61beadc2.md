We have 2 sites linked together with VPN tunnel (Fortigate 60C devices). On each site I have ESXi server with a couple of VMs. Normally, everything works fine.
Site 1 (S1) subnet is 192.168.254.0/24, with Machine A1, A2 on ESXi1
 
Site 2 (S2) subnet is 192.168.253.0/24, with Machine B1, B2 on ESXi2
All ping between those machines works normally through VPN tunnel.
Suddently, S1-A1 cannot ping S2-B1 anymore, but S2-B1 still ping S1-A1.
All pings (using IP addresses) accross all machines (VMs and ESXi) works except from S1-A1 -> S2-B1.
Traceroute results were:
 
S1-A1 -> S2-B1  -> through Internet (?????)
S1-A1 -> S2-B2  -> through VPN Tunnel
S2-B2 -> S1-A1  -> through VPN Tunnel
S1-A1 -> S2-ESXi2 -> through VPN Tunnel
Machine A1 is a Windows 2003 R2 - SP2. There is 5 IP addresses binded on the NIC. I tried to disable and enable the NIC but the network management stopped responding. Only a reboot fixed the problem.
route print did not change.  Gateway is the same and no specific route to reach B2.
arp -a did not show anything related to 192.168.253.0/24.
I don't understand why S1-A1 -> S2-ESXi2 worked but but not S1-A1 -> S2-B1 because B2 (192.168.253.18) is running on ESXi2 (192.168.253.23).
Registry entry of the Network Interface
Windows Registry Editor Version 5.00
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\Interfaces\{0E114693-5FC8-4AA4-AB98-14CE43E24DE5}]
"UseZeroBroadcast"=dword:00000000
"EnableDeadGWDetect"=dword:00000001
"EnableDHCP"=dword:00000000
"IPAddress"=hex(7):31,00,39,00,32,00,2e,00,31,00,36,00,38,00,2e,00,32,00,35,00,\
  34,00,2e,00,31,00,35,00,00,00,31,00,39,00,32,00,2e,00,31,00,36,00,38,00,2e,\
  00,32,00,35,00,34,00,2e,00,31,00,32,00,00,00,31,00,39,00,32,00,2e,00,31,00,\
  36,00,38,00,2e,00,32,00,35,00,34,00,2e,00,31,00,33,00,00,00,31,00,39,00,32,\
  00,2e,00,31,00,36,00,38,00,2e,00,32,00,35,00,34,00,2e,00,31,00,35,00,31,00,\
  00,00,31,00,39,00,32,00,2e,00,31,00,36,00,38,00,2e,00,32,00,35,00,34,00,2e,\
  00,34,00,30,00,00,00,00,00
  which is 192.168.254.15 192.168.254.12 192.168.254.13 192.168.254.151 192.168.254.40
"SubnetMask"=hex(7):32,00,35,00,35,00,2e,00,32,00,35,00,35,00,2e,00,32,00,35,\
  00,35,00,2e,00,30,00,00,00,32,00,35,00,35,00,2e,00,32,00,35,00,35,00,2e,00,\
  32,00,35,00,35,00,2e,00,30,00,00,00,32,00,35,00,35,00,2e,00,32,00,35,00,35,\
  00,2e,00,32,00,35,00,35,00,2e,00,30,00,00,00,32,00,35,00,35,00,2e,00,32,00,\
  35,00,35,00,2e,00,32,00,35,00,35,00,2e,00,30,00,00,00,32,00,35,00,35,00,2e,\
  00,32,00,35,00,35,00,2e,00,32,00,35,00,35,00,2e,00,30,00,00,00,00,00
  which is 255.255.255.0 255.255.255.0  255.255.255.0  255.255.255.0 255.255.255.0
"DefaultGateway"=hex(7):31,00,39,00,32,00,2e,00,31,00,36,00,38,00,2e,00,32,00,\
  35,00,34,00,2e,00,32,00,35,00,34,00,00,00,00,00
  which is 192.168.254.254
"DefaultGatewayMetric"=hex(7):30,00,00,00,00,00
"NameServer"="192.168.254.254"
"Domain"=""
"RegistrationEnabled"=dword:00000001
"RegisterAdapterName"=dword:00000000
"TCPAllowedPorts"=hex(7):30,00,00,00,00,00
"UDPAllowedPorts"=hex(7):30,00,00,00,00,00
"RawIPAllowedProtocols"=hex(7):30,00,00,00,00,00
"NTEContextList"=hex(7):00,00
"DhcpClassIdBin"=hex:
"DhcpServer"="255.255.255.255"
"Lease"=dword:00000e10
"LeaseObtainedTime"=dword:51185713
"T1"=dword:51185e1b
"T2"=dword:51186361
"LeaseTerminatesTime"=dword:51186523
"IPAutoconfigurationAddress"="0.0.0.0"
"IPAutoconfigurationMask"="255.255.0.0"
"IPAutoconfigurationSeed"=dword:00000000
"AddressType"=dword:00000000
I exclude the Fortigates as part of the problem since just needed to reboot A1.
2013-09-19 : Issue again. Seems to occur everytime the VPNs drops between the Fortigates.
HOCHELAGA_2 # get router info routing-table all
Codes: K - kernel, C - connected, S - static, R - RIP, B - BGP
       O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
       * - candidate default
S*      0.0.0.0/0 [10/0] via 64.15.130.49, wan1
C       10.10.10.0/24 is directly connected, dmz
C       10.100.254.1/32 is directly connected, fat
C       10.100.254.2/32 is directly connected, fat
C       64.15.130.48/28 is directly connected, wan1
                        is directly connected, wan1
                        is directly connected, wan1
                        is directly connected, wan1
                        is directly connected, wan1
                        is directly connected, wan1
S       192.168.200.0/24 [10/0] via 10.100.254.2, fat
C       192.168.250.0/24 is directly connected, internal
S       192.168.252.0/24 [10/0] is directly connected, hoch st-bruno
S       192.168.253.0/24 [10/0] is directly connected, HOCH-KAN
C       192.168.254.0/24 is directly connected, internal
                         is directly connected, internal
                         is directly connected, internal
HOCHELAGA_2 # diagnose ip route list
tab=254 vf=0 scope=253 type=1 proto=2 prio=0 0.0.0.0/0.0.0.0/0->10.100.254.2/32 pref=10.100.254.1 gwy=0.0.0.0 dev=11(fat)
tab=254 vf=0 scope=253 type=1 proto=2 prio=0 0.0.0.0/0.0.0.0/0->64.15.130.48/28 pref=64.15.130.56 gwy=0.0.0.0 dev=3(wan1)
tab=254 vf=0 scope=253 type=1 proto=2 prio=0 0.0.0.0/0.0.0.0/0->169.254.0.64/26 pref=169.254.0.66 gwy=0.0.0.0 dev=16(havdlink1)
tab=254 vf=0 scope=0 type=1 proto=11 prio=0 0.0.0.0/0.0.0.0/0->192.168.200.0/24 pref=0.0.0.0 gwy=10.100.254.2 dev=11(fat)
tab=254 vf=0 scope=253 type=1 proto=2 prio=0 0.0.0.0/0.0.0.0/0->192.168.250.0/24 pref=192.168.250.254 gwy=0.0.0.0 dev=5(internal)
tab=254 vf=0 scope=253 type=1 proto=2 prio=0 0.0.0.0/0.0.0.0/0->10.10.10.0/24 pref=10.10.10.1 gwy=0.0.0.0 dev=4(dmz)
tab=254 vf=0 scope=0 type=1 proto=11 prio=0 0.0.0.0/0.0.0.0/0->192.168.252.0/24 pref=0.0.0.0 gwy=0.0.0.0 dev=9(hoch st-bruno)
tab=254 vf=0 scope=0 type=1 proto=11 prio=0 0.0.0.0/0.0.0.0/0->192.168.253.0/24 pref=0.0.0.0 gwy=0.0.0.0 dev=10(HOCH-KAN)
tab=254 vf=0 scope=253 type=1 proto=2 prio=0 0.0.0.0/0.0.0.0/0->192.168.254.0/24 pref=192.168.254.254 gwy=0.0.0.0 dev=5(internal)
tab=254 vf=0 scope=0 type=1 proto=11 prio=0 0.0.0.0/0.0.0.0/0->0.0.0.0/0 pref=0.0.0.0 gwy=64.15.130.49 dev=3(wan1)
tab=255 vf=0 scope=253 type=3 proto=2 prio=0 0.0.0.0/0.0.0.0/0->64.15.130.63/32 pref=64.15.130.56 gwy=0.0.0.0 dev=3(wan1)
tab=255 vf=0 scope=253 type=3 proto=2 prio=0 0.0.0.0/0.0.0.0/0->127.255.255.255/32 pref=127.0.0.1 gwy=0.0.0.0 dev=7(root)
tab=255 vf=0 scope=254 type=2 proto=2 prio=0 0.0.0.0/0.0.0.0/0->10.10.10.1/32 pref=10.10.10.1 gwy=0.0.0.0 dev=4(dmz)
tab=255 vf=0 scope=253 type=3 proto=2 prio=0 0.0.0.0/0.0.0.0/0->10.10.10.0/32 pref=10.10.10.1 gwy=0.0.0.0 dev=4(dmz)
tab=255 vf=0 scope=254 type=2 proto=2 prio=0 0.0.0.0/0.0.0.0/0->10.100.254.1/32 pref=10.100.254.1 gwy=0.0.0.0 dev=11(fat)
tab=255 vf=0 scope=254 type=2 proto=2 prio=0 0.0.0.0/0.0.0.0/0->64.15.130.59/32 pref=64.15.130.59 gwy=0.0.0.0 dev=2(wan2)
tab=255 vf=0 scope=254 type=2 proto=2 prio=0 0.0.0.0/0.0.0.0/0->192.168.254.2/32 pref=192.168.254.254 gwy=0.0.0.0 dev=5(internal)
tab=255 vf=0 scope=254 type=2 proto=2 prio=0 0.0.0.0/0.0.0.0/0->64.15.130.58/32 pref=64.15.130.56 gwy=0.0.0.0 dev=3(wan1)
tab=255 vf=0 scope=254 type=2 proto=2 prio=0 0.0.0.0/0.0.0.0/0->169.254.0.66/32 pref=169.254.0.66 gwy=0.0.0.0 dev=16(havdlink1)
tab=255 vf=0 scope=253 type=3 proto=2 prio=0 0.0.0.0/0.0.0.0/0->192.168.250.0/32 pref=192.168.250.254 gwy=0.0.0.0 dev=5(internal)
tab=255 vf=0 scope=254 type=2 proto=2 prio=0 0.0.0.0/0.0.0.0/0->192.168.254.1/32 pref=192.168.254.254 gwy=0.0.0.0 dev=5(internal)
tab=255 vf=0 scope=254 type=2 proto=2 prio=0 0.0.0.0/0.0.0.0/0->64.15.130.57/32 pref=64.15.130.56 gwy=0.0.0.0 dev=3(wan1)
tab=255 vf=0 scope=253 type=3 proto=2 prio=0 0.0.0.0/0.0.0.0/0->192.168.254.0/32 pref=192.168.254.254 gwy=0.0.0.0 dev=5(internal)
tab=255 vf=0 scope=254 type=2 proto=2 prio=0 0.0.0.0/0.0.0.0/0->64.15.130.56/32 pref=64.15.130.56 gwy=0.0.0.0 dev=3(wan1)
tab=255 vf=0 scope=253 type=3 proto=2 prio=0 0.0.0.0/0.0.0.0/0->169.254.0.64/32 pref=169.254.0.66 gwy=0.0.0.0 dev=16(havdlink1)
tab=255 vf=0 scope=254 type=2 proto=2 prio=0 0.0.0.0/0.0.0.0/0->64.15.130.54/32 pref=64.15.130.56 gwy=0.0.0.0 dev=3(wan1)
tab=255 vf=0 scope=253 type=3 proto=2 prio=0 0.0.0.0/0.0.0.0/0->169.254.0.127/32 pref=169.254.0.66 gwy=0.0.0.0 dev=16(havdlink1)
tab=255 vf=0 scope=254 type=2 proto=2 prio=0 0.0.0.0/0.0.0.0/0->64.15.130.53/32 pref=64.15.130.56 gwy=0.0.0.0 dev=3(wan1)
tab=255 vf=0 scope=254 type=2 proto=2 prio=0 0.0.0.0/0.0.0.0/0->64.15.130.52/32 pref=64.15.130.56 gwy=0.0.0.0 dev=3(wan1)
tab=255 vf=0 scope=253 type=3 proto=2 prio=0 0.0.0.0/0.0.0.0/0->10.10.10.255/32 pref=10.10.10.1 gwy=0.0.0.0 dev=4(dmz)
tab=255 vf=0 scope=253 type=3 proto=2 prio=0 0.0.0.0/0.0.0.0/0->127.0.0.0/32 pref=127.0.0.1 gwy=0.0.0.0 dev=7(root)
tab=255 vf=0 scope=254 type=2 proto=2 prio=0 0.0.0.0/0.0.0.0/0->192.168.254.254/32 pref=192.168.254.254 gwy=0.0.0.0 dev=5(internal)
tab=255 vf=0 scope=253 type=3 proto=2 prio=0 0.0.0.0/0.0.0.0/0->192.168.250.255/32 pref=192.168.250.254 gwy=0.0.0.0 dev=5(internal)
tab=255 vf=0 scope=254 type=2 proto=2 prio=0 0.0.0.0/0.0.0.0/0->127.0.0.1/32 pref=127.0.0.1 gwy=0.0.0.0 dev=7(root)
tab=255 vf=0 scope=253 type=3 proto=2 prio=0 0.0.0.0/0.0.0.0/0->64.15.130.48/32 pref=64.15.130.56 gwy=0.0.0.0 dev=3(wan1)
tab=255 vf=0 scope=254 type=2 proto=2 prio=0 0.0.0.0/0.0.0.0/0->192.168.250.254/32 pref=192.168.250.254 gwy=0.0.0.0 dev=5(internal)
tab=255 vf=0 scope=253 type=3 proto=2 prio=0 0.0.0.0/0.0.0.0/0->192.168.254.255/32 pref=192.168.254.254 gwy=0.0.0.0 dev=5(internal)
tab=255 vf=0 scope=254 type=2 proto=2 prio=0 0.0.0.0/0.0.0.0/0->127.0.0.0/8 pref=127.0.0.1 gwy=0.0.0.0 dev=7(root)
PING succesfull on a server
diagnose sniffer packet any "host 192.168.253.23" 4
23.232067 internal in 192.168.254.15 -> 192.168.253.23: icmp: echo request
23.232329 HOCH-KAN out 192.168.254.15 -> 192.168.253.23: icmp: echo request
23.248800 HOCH-KAN in 192.168.253.23 -> 192.168.254.15: icmp: echo reply
23.248932 internal out 192.168.253.23 -> 192.168.254.15: icmp: echo reply
PING failed on a server
diagnose sniffer packet any "host 192.168.253.18" 4
8.212249 internal in 192.168.254.15 -> 192.168.253.18: icmp: echo request
8.212479 wan1 out 64.15.130.56 -> 192.168.253.18: icmp: echo request
10.508155 internal in 192.168.254.15.1113 -> 192.168.253.18.139: syn 1271941747
10.508436 wan1 out 64.15.130.56.42334 -> 192.168.253.18.139: syn 1271941747
11.706287 internal in 192.168.254.15.1112 -> 192.168.253.18.445: syn 341420858
11.706540 wan1 out 64.15.130.56.42332 -> 192.168.253.18.445: syn 341420858
Why the route taken is different for the server on the same network ? I don't use any RIP, OSPF, BGP routing. No policy routing. Juste a static route between VPNs. Nothing is showing a dynamic route for 192.168.253.23 and the Fortigate decide to route it into the wan1 interface instead.
Anything I could check next time it happens ?
Thank in advance
And sorry if is not fully clear, french is my mother language
S.