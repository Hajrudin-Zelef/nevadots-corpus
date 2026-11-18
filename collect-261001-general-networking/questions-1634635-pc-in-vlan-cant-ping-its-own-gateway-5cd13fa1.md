My VLAN's can't ping their own gateway and beyond.

1 - For VLANs to be able to ping their own gateway, do I have to enable the Parent VLAN and assign an IP to it in the OPNsense firewall ?

2 - Do I have to enable DHCP for devices in the VLAN, I know sounds foolish but just checking as all videos I found regarding this on OPNSense show as DHCP enabled which I'm not using.

All rules are allowed on all interfaces, and I'm not using any physical switch, this is a VMware Workstation setup, I have a Windows VM and OPNsense VM.

**Parent VLAN Interface**

**Server VLAN**

**Server Ping Failure**

**tcpdump on em3 shows the following**

```
root@firewallwm:~ # tcpdump -e -n -i em3
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on em3, link-type EN10MB (Ethernet), capture size 262144 bytes
18:26:52.651787 00:0c:29:ae:a2:10 > ff:ff:ff:ff:ff:ff, ethertype ARP (0x0806), length 60: Request who-has 192.168.28.35 tell 192.168.28.47, length 46
18:26:53.316297 00:0c:29:ae:a2:10 > ff:ff:ff:ff:ff:ff, ethertype ARP (0x0806), length 60: Request who-has 192.168.28.35 tell 192.168.28.47, length 46
18:26:54.316412 00:0c:29:ae:a2:10 > ff:ff:ff:ff:ff:ff, ethertype ARP (0x0806), length 60: Request who-has 192.168.28.35 tell 192.168.28.47, length 46
```
I checked non device has the MAC address **00:0c:29:ae:a2:10**.

**Firewall Settings**

**Windows VM**