I'm not quite certain how to achieve the equivalent of ip directed broadcast with a FortiGate. In this case a FortiGate 60E with FortiOS 5.6.7. So I started to dig a little.
QUESTION:
Can anyone confirm that, on a FortiGate, set broadcast-forward enable on the egress interface does actually forward a directed broadcast packet to the given subnet as broadcast (as in: DstMAC ff:ff:ff:ff:ff:ff) out of that interface? Suitable firewall policies assumed to be in place, of course.
EDIT: That part of the question is answered: No, set broadcast-forward enable on the egress interface does not have this
desired effect.
Sideline Question: Is there another way to achieve this on a FortiGate?
EDIT 2020-07-21: Yes, it is possible. See "ADDON-2" below. It is based on Lukas' answer (see below).
Please note: I am perfectly familiar with ip directed-broacast <someACL> on Cisco routing gear, and I've successfully deployed WoL support many times with that.
Also note: I'm also not trying to make something like a broadcast-helper or WoL relay work on a FortiGate interface facing the WoL Magic Packet sending host. That host knows the remote subnet's directed broadcast address and sends to it.
I keep finding hints (such as next door on serverfault) that set broadcast-forward enable were to add support to have directed broadcasts forwarded as broadcasts in the attached subnet.
The documentation (or its equivalent for FortiOS 5.6) quoted with that has this to say:
ARP: by default, ARP broadcasts and ARP reply packets are flooded/forwarded on all ports or VLANs belonging to the same forwarding domain, without the need of firewall policies between the ports. This default behavior is necessary to allow the population of the FDB and allow further firewall policy lookup (see section Transparent mode Firewall processing for more details). This option is configurable at the interface settings level with the parameter arpforward (enabled by default).
Non-ARP: To forward non-ARP broadcasts, the following CLI command is used:
config system interface
edit "port2"
set broadcast-forward enable
next
end
BUT this quote is from the Networking in Transparent Mode section of the documentation (see --> Packet Forwarding --> Broadcast, Multicast, Unicast Forwarding), and we're not running transparent mode, here.
AND I do get the impression that set broadcast-forward enable is more an ingress thing than something for egress. The "best answer" in this thread on  the Fortinet community kind of confirms this gut feeling.
I have also read the FortiNet KB article, which is also being quoted and referenced elsewhere, but ... static ARP entries? Really? We have dozens of clients at that site! (Well, I could still add a static ARP entry for the directed broadcast address with ff:ff:ff:ff:ff:ff, but that seems somewhat wrong.)
Thanks for your answers, comments and pointers.
[ADDON-1]:
As suggested in zac67's answer, I tried with a multicast address, multicast policy, plus a narrow unicast policy (allowing source to directed-broadcast). None had the desired effect.
Also: set broadcast-forward enable on the egress interface has no effect. Packets get dropped upon ingress because of an ip forwarding check failure.
id=20085 trace_id=35 func=fw_local_in_handler line=402 msg="iprope_in_check() check failed on policy 0, drop"
Interestingly this happens despite the fact that the firewall does have a entry in the routing table mapping 192.168.10.255/32 to the correct egress interface.
RemoteFirewall # diag ip route list | grep 192.168.10
tab=255 vf=0 scope=253 type=3 proto=2 prio=0 0.0.0.0/0.0.0.0/0->192.168.10.0/32 pref=192.168.10.1 gwy=0.0.0.0 dev=8(internal1)
tab=255 vf=0 scope=254 type=2 proto=2 prio=0 0.0.0.0/0.0.0.0/0->192.168.10.1/32 pref=192.168.10.1 gwy=0.0.0.0 dev=8(internal1)
tab=255 vf=0 scope=253 type=3 proto=2 prio=0 0.0.0.0/0.0.0.0/0->192.168.10.255/32 pref=192.168.10.1 gwy=0.0.0.0 dev=8(internal1)
tab=254 vf=0 scope=253 type=1 proto=2 prio=0 0.0.0.0/0.0.0.0/0->192.168.10.0/24 pref=192.168.10.1 gwy=0.0.0.0 dev=8(internal1)
It is only with set broadcast-forward enable on the ingress interface (sic! Just don't get me started on the implications of this!) of the last hop Fortigate that I see a change in behaviour.
Please note: My tests were done with ICMP. Near the WoL sender, I only have access to systems that can send ICMP, not udp/9. Should be of no relevance, here.
filters=[host 192.168.115.1]
3.881184 WAN1 in 192.168.115.1 -> 192.168.10.255: icmp: echo request
3.881469 internal1 out 192.168.115.1 -> 192.168.10.255: icmp: echo request
3.882015 internal1 in 192.168.10.3 -> 192.168.115.1: icmp: echo reply
3.882016 internal1 in 192.168.10.2 -> 192.168.115.1: icmp: echo reply
3.882033 internal1 in 192.168.10.4 -> 192.168.115.1: icmp: echo reply
4.896689 WAN1 in 192.168.115.1 -> 192.168.10.255: icmp: echo request
4.896771 internal1 out 192.168.115.1 -> 192.168.10.255: icmp: echo request
4.897309 internal1 in 192.168.10.2 -> 192.168.115.1: icmp: echo reply
4.897325 internal1 in 192.168.10.3 -> 192.168.115.1: icmp: echo reply
4.899076 internal1 in 192.168.10.4 -> 192.168.115.1: icmp: echo reply
    
So at least, something is happening. But these packets are (at layer 2) not real broadcasts, but they're being sent to DstMac 00:00:00:00:00:00 (where I'd expect ff:ff:ff:ff:ff:ff)
filters=[host 192.168.115.1]
4.492989 WAN1 in 192.168.115.1 -> 192.168.10.255: icmp: echo request
0x0000   0000 0000 0001 0000 0000 0000 0800 4500        ..............E.
0x0010   0054 2614 0000 4001 5544 c0a8 7301 c0a8        .T&...@.UD..s...
0x0020   0aff 0800 8984 5700 0000 13a2 c25c 0000        ......W......\..
0x0030   0000 3a7c 0700 0000 0000 0000 0000 0000        ..:|............
0x0040   0000 0000 0000 0000 0000 0000 0000 0000        ................
0x0050   0000 0000 0000 0000 0000 0000 0000 0000        ................
0x0060   0000                                           ..
4.493277 internal1 out 192.168.115.1 -> 192.168.10.255: icmp: echo request
0x0000   0000 0000 0000 0009 0f09 0b01 0800 4500        ..............E.
0x0010   0054 2614 0000 3f01 5644 c0a8 7301 c0a8        .T&...?.VD..s...
0x0020   0aff 0800 8984 5700 0000 13a2 c25c 0000        ......W......\..
0x0030   0000 3a7c 0700 0000 0000 0000 0000 0000        ..:|............
0x0040   0000 0000 0000 0000 0000 0000 0000 0000        ................
0x0050   0000 0000 0000 0000 0000 0000 0000 0000        ................
0x0060   0000                                           ..
This behaviour is seen with or without any of the multicast config bits in place, and with or without the narrow unicast firewall policy.
Adding set broadcast-forward enable to the egress interface does not change the DstMAC address being used in the egress packet. Still, some systems on the local subnet seem to react to DstMAC 00:00:00:00:00:00 and send their ping replies. these of course are out-of-state to the firewall and get dropped - no harm in that.
I'll have the server team try WoL with the given configuration - if that won't work, we'll try setting a static ARP entry mapping 192.168.10.255 to ff:ff:ff:ff:ff:ff
[/ADDON-1]:
[ADDON-2 2020-07-21]:
Yes, it took a while for the Systems Managament people to get back to the topic and eventually find some time to send some WoL Magic Packets down the WAN.
Testing was done on a Fortigate 100E with FortiOS 6.0.8.
See Lukas' answer below for a config example. No form of broadcast-forward enable was needed. I am aware that zac67's answer says the same, but includes broadcast-forward enable.
This is what the directed broadcast looked like when it left the FG100 into the given LAN/Subnet. You'll note the proper broadcast destination address (ffff.ffff.ffff).
87.273480 port7 -- 192.168.35.9.54525 -> 192.168.37.255.7: udp 102
0x0000   ffff ffff ffff 0009 0f09 2012 0800 4500        ..............E.
0x0010   0082 185b 0000 7e11 59b7 c0a8 2309 c0a8        ...[..~.Y...#...
0x0020   25ff d4fd 0007 006e 5494 ffff ffff ffff        %......nT.......
0x0030   f8b4 6a25 9dd7 f8b4 6a25 9dd7 f8b4 6a25        ..j%....j%....j%
0x0040   9dd7 f8b4 6a25 9dd7 f8b4 6a25 9dd7 f8b4        ....j%....j%....
0x0050   6a25 9dd7 f8b4 6a25 9dd7 f8b4 6a25 9dd7        j%....j%....j%..
0x0060   f8b4 6a25 9dd7 f8b4 6a25 9dd7 f8b4 6a25        ..j%....j%....j%
0x0070   9dd7 f8b4 6a25 9dd7 f8b4 6a25 9dd7 f8b4        ....j%....j%....
0x0080   6a25 9dd7 f8b4 6a25 9dd7 f8b4 6a25 9dd7        j%....j%....j%..
Hint: the FG100E showed similar behaviour as the FG60E from earlier tests.
With diag sniffer packet any <someFilterString>, the destination MAC was shown as 0000.0000.0000, but diag sniffer packet port7 <someFilterString> showed ffff.ffff.ffff. That's not quite what one would expect, and extends troubleshooting unnecessarily.
[/ADDON-2]:
index=8 ifname=internal1 255.255.255.255 ff:ff:ff:ff:ff:ff state=00000040 ...exec ping 192.168.1.255), the packets are leaving the box with DstMAc addressff:ff:ff:ff:ff:ff.