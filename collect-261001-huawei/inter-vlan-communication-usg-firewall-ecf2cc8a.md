# Inter-VLAN communication on USG firewall

(Source archive : http://labnario.com/inter-vlan-communication-usg-firewall/ — capture 20260616033907)

__

[__](https://www.facebook.com/Huawei-From-Scratch-376204559133899/ "Facebook")[__](https://twitter.com/labnario "Twitter")[__](https://foursquare.com/mo3aser "Foursquare")

Tuesday , June 16 2026

__

[__](https://www.facebook.com/Huawei-From-Scratch-376204559133899/ "Facebook")[__](https://twitter.com/labnario "Twitter")[__](https://foursquare.com/mo3aser "Foursquare")

##  [ ![Labnario](http://labnario.com/wp-content/uploads/2017/06/logo_labnario.png)**Labnario Huawei From Scratch** ](http://labnario.com/ "Labnario")

  * [HOME](http://labnario.com/)
  * [HUAWEI CHEAT SHEETS](http://labnario.com/huawei-cheat-sheets/)
  * [ABOUT AUTHOR](http://labnario.com/about/)



[Home](http://labnario.com) / [Security](http://labnario.com/category/security/) / Inter-VLAN communication on USG firewall

# Inter-VLAN communication on USG firewall

__3 Oct, 2015 __[Security](http://labnario.com/category/security/) __[Leave a comment](http://labnario.com/inter-vlan-communication-usg-firewall/#respond)

It's time to check a firewall available in [eNSP](http://support.huawei.com/enterprise/softdownload?pid=9017384&idAbsPath=fixnode01|7919710|9856717|7923123|9858914|9017384&lang=en "Huawei eNSP simulator") simulator. Today a simple task, just configuring inter-VLAN communication on **Huawei USG5500**.

Let's look at the following topology:

[![](http://labnario.com/wp-content/uploads/2017/06/Inter-VLAN-communication-topology-300x200.png)](http://labnario.com/wp-content/uploads/2017/06/Inter-VLAN-communication-topology.png)

 

Configuration roadmap:

  1. Configure L2 communication on the switch.
  2. As the switch is L2, configure subinterfaces on the firewall and enable L3 communication between different **VLANs**.
  3. Set IP addresses and gateways for all PCs.
  4. Create 2 security zones and configure interzone packet-filterfing to control traffic between VLANs.
  5. Security requirements for the network:


  * PC1 in VLAN100 and PC2 in VLAN200 can communicate each other.
  * PC3 in VLAN300 can access PC1 and PC2. PC1 and PC2 cannot access PC3.



L2 communication on the switch:
    
    
    #
    interface Ethernet0/0/1
     port link-type access
     port default vlan 100
    #
    interface Ethernet0/0/2
     port link-type access
     port default vlan 200
    #
    interface Ethernet0/0/3
     port link-type access
     port default vlan 300
    #
    interface GigabitEthernet0/0/1
     port link-type trunk
     port trunk allow-pass vlan 100 200 300

L3 communication on the _firewall_ :
    
    
    #
    **interface GigabitEthernet0/0/1.100**
     vlan-type dot1q 100
     alias GigabitEthernet0/0/1.100
     ip address 10.1.1.2 255.255.255.0
    #
    **interface GigabitEthernet0/0/1.200**
     vlan-type dot1q 200
     alias GigabitEthernet0/0/1.200
     ip address 172.16.1.2 255.255.255.0
    #
    **interface GigabitEthernet0/0/1.300**
     vlan-type dot1q 300
     alias GigabitEthernet0/0/1.300
     ip address 192.168.1.2 255.255.255.0
    #
    

Create two _security zones_ and add subinterfaces to them based on the above requirements:
    
    
    [Firewall]**firewall zone name a**
    [Firewall-zone-a]set priority 70
    [Firewall-zone-a]add interface GigabitEthernet0/0/1.100
    [Firewall-zone-a] add interface GigabitEthernet0/0/1.200
    [Firewall-zone-a]
    
    [Firewall]**firewall zone name b**
    [Firewall-zone-b]set priority 80
    [Firewall-zone-b] add interface GigabitEthernet0/0/1.300
    [Firewall-zone-b]

A security zone is a security concept, based on which most security policies are implemented. A security zone is a set of the networks connected by interfaces. Users on these networks have the same security attributes. USG has 4 security zones by default, which cannot be deleted and their security level cannot be changed. You can create security zones and specify their security levels according to actual networking requirements. The security level ranges from 1 to 100. The larger the value, the higher the security level.

Configure _interzone packet filtering_ based on the above requirements:
    
    
    [Firewall]**policy interzone b a inbound**
    [Firewall-policy-interzone-b-a-inbound]policy 1
    [Firewall-policy-interzone-b-a-inbound-1]  action deny
    [Firewall-policy-interzone-b-a-inbound-1]  policy source 10.1.1.0 0.0.0.255
    [Firewall-policy-interzone-b-a-inbound-1]  policy source 172.16.1.0 0.0.0.255
    [Firewall-policy-interzone-b-a-inbound-1]  policy destination 192.168.1.0 0.0.0.255
    
    [Firewall]**policy interzone b a outbound**
    [Firewall-policy-interzone-b-a-outbound]policy 1
    [Firewall-policy-interzone-b-a-outbound-1]  action permit
    [Firewall-policy-interzone-b-a-outbound-1]  policy source 192.168.1.0 0.0.0.255

A security interzone is to describe the transmission channel of the traffic, which is the unique path between two zones.

Data flows in the interzone are directional:

  * **Outbound** - indicates that data is transmitted from a higher security zone to a lower security zone.
  * **Inbound** - indicates that data is transmitted from a lower security zone to a higher security zone



Verification:
    
    
    PC1>**ping 172.16.1.1**
    
    Ping 172.16.1.1: 32 data bytes, Press Ctrl_C to break
    From 172.16.1.1: bytes=32 seq=1 ttl=127 time=62 ms
    From 172.16.1.1: bytes=32 seq=2 ttl=127 time=78 ms
    From 172.16.1.1: bytes=32 seq=3 ttl=127 time=47 ms
    From 172.16.1.1: bytes=32 seq=4 ttl=127 time=63 ms
    From 172.16.1.1: bytes=32 seq=5 ttl=127 time=62 ms
    
    --- 172.16.1.1 ping statistics ---
      5 packet(s) transmitted
      5 packet(s) received
      0.00% packet loss
      round-trip min/avg/max = 47/62/78 ms
    
    PC1>**ping 192.168.1.1**
    
    Ping 192.168.1.1: 32 data bytes, Press Ctrl_C to break
    Request timeout!
    Request timeout!
    Request timeout!
    Request timeout!
    Request timeout!
    
    --- 192.168.1.1 ping statistics ---
      5 packet(s) transmitted
      0 packet(s) received
      100.00% packet loss
    
    PC1>
    
    
    PC2>**ping 10.1.1.1**
    
    Ping 10.1.1.1: 32 data bytes, Press Ctrl_C to break
    From 10.1.1.1: bytes=32 seq=1 ttl=127 time=63 ms
    From 10.1.1.1: bytes=32 seq=2 ttl=127 time=62 ms
    From 10.1.1.1: bytes=32 seq=3 ttl=127 time=78 ms
    From 10.1.1.1: bytes=32 seq=4 ttl=127 time=63 ms
    From 10.1.1.1: bytes=32 seq=5 ttl=127 time=63 ms
    
    --- 10.1.1.1 ping statistics ---
      5 packet(s) transmitted
      5 packet(s) received
      0.00% packet loss
      round-trip min/avg/max = 62/65/78 ms
    
    PC2>**ping 192.168.1.1**
    
    Ping 192.168.1.1: 32 data bytes, Press Ctrl_C to break
    Request timeout!
    Request timeout!
    Request timeout!
    Request timeout!
    Request timeout!
    
    --- 192.168.1.1 ping statistics ---
      5 packet(s) transmitted
      0 packet(s) received
      100.00% packet loss
    
    PC2>
    
    
    PC3>**ping 10.1.1.1**
    
    Ping 10.1.1.1: 32 data bytes, Press Ctrl_C to break
    From 10.1.1.1: bytes=32 seq=1 ttl=127 time=78 ms
    From 10.1.1.1: bytes=32 seq=2 ttl=127 time=62 ms
    From 10.1.1.1: bytes=32 seq=3 ttl=127 time=46 ms
    From 10.1.1.1: bytes=32 seq=4 ttl=127 time=62 ms
    From 10.1.1.1: bytes=32 seq=5 ttl=127 time=78 ms
    
    --- 10.1.1.1 ping statistics ---
      5 packet(s) transmitted
      5 packet(s) received
      0.00% packet loss
      round-trip min/avg/max = 46/65/78 ms
    
    PC3>**ping 172.16.1.1**
    
    Ping 172.16.1.1: 32 data bytes, Press Ctrl_C to break
    From 172.16.1.1: bytes=32 seq=1 ttl=127 time=63 ms
    From 172.16.1.1: bytes=32 seq=2 ttl=127 time=47 ms
    From 172.16.1.1: bytes=32 seq=3 ttl=127 time=63 ms
    From 172.16.1.1: bytes=32 seq=4 ttl=127 time=62 ms
    From 172.16.1.1: bytes=32 seq=5 ttl=127 time=47 ms
    
    --- 172.16.1.1 ping statistics ---
      5 packet(s) transmitted
      5 packet(s) received
      0.00% packet loss
      round-trip min/avg/max = 47/56/63 ms
    
    PC3>

As you can see from the outputs, PC1 and PC2 can communicate each other but cannot access PC3. PC3 can access PC1 and PC2 which completes our task.

Share

  * [__ Facebook](http://www.facebook.com/sharer.php?u=http://labnario.com/?p=925)
  * [__ Twitter](https://twitter.com/intent/tweet?text=Inter-VLAN+communication+on+USG+firewall&url=http://labnario.com/?p=925)
  * [__ LinkedIn](http://www.linkedin.com/shareArticle?mini=true&url=http://labnario.com/?p=925&title=Inter-VLAN+communication+on+USG+firewall)



Tags [Huawei CLI](http://labnario.com/tag/huawei-cli/) [Huawei firewall](http://labnario.com/tag/huawei-firewall/) [VLAN](http://labnario.com/tag/vlan/)

[Previous Huawei eNSP - news](http://labnario.com/huawei-ensp-news/)

[Next basic concept of RIP](http://labnario.com/basic-concept-rip/)

### Leave a Reply [Cancel reply](/inter-vlan-communication-usg-firewall/#respond)

Your email address will not be published. Required fields are marked *

Comment *

Name *

Email *

Website

Save my name, email, and website in this browser for the next time I comment.

Δ

[__](https://www.facebook.com/Huawei-From-Scratch-376204559133899/ "Facebook")[__](https://twitter.com/labnario "Twitter")[__](https://foursquare.com/mo3aser "Foursquare")

__

#### Categories

  * [Basic Configuration](http://labnario.com/category/basic-configuration/)
  * [Cheat Sheets](http://labnario.com/category/cheat-sheets/)
  * [Command Line](http://labnario.com/category/command-line/)
  * [Ethernet](http://labnario.com/category/ethernet/)
  * [FAQ](http://labnario.com/category/faq/)
  * [General](http://labnario.com/category/general/)
  * [How To](http://labnario.com/category/how-to/)
  * [IP Routing](http://labnario.com/category/ip-routing/)
  * [IP Services](http://labnario.com/category/ip-services/)
  * [Multicast](http://labnario.com/category/multicast/)
  * [QoS](http://labnario.com/category/qos/)
  * [Reliability](http://labnario.com/category/reliability/)
  * [Security](http://labnario.com/category/security/)
  * [System Management](http://labnario.com/category/system-management/)
  * [Uncategorized](http://labnario.com/category/uncategorized/)
  * [Video](http://labnario.com/category/video/)
  * [VPN](http://labnario.com/category/vpn/)
  * [WAN](http://labnario.com/category/wan/)



#### Recent Posts

  * [HCIA Datacom Course Summer Sale](http://labnario.com/hcia-datacom-course-summer-sale/)
  * [What's new in the New Year?](http://labnario.com/whats-new-in-the-new-year/)
  * [New Year Sale](http://labnario.com/new-year-sale/)
  * [Huawei AR router USB-based deployment (ZTP)](http://labnario.com/huawei-ar-router-usb-based-deployment-ztp/)
  * [Feel invited to the Huawei HCIA course](http://labnario.com/feel-invited-to-the-huawei-hcia-course/)



Design By: [ThemeTen](themeten.com)
