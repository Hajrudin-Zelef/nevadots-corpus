# Basic IP MPLS VPN configuration on Huawei Routers

(Source archive : https://lyfeytech.co.ke/basic-ip-mpls-vpn-configuration-on-huawei-routers/ — capture 20260313233820)

[ ![Lyfey Technologies](https://lyfeytech.co.ke/wp-content/uploads/2024/01/lyfey.png) ](https://lyfeytech.co.ke/)

[ ![Lyfey Technologies](https://lyfeytech.co.ke/wp-content/uploads/2024/01/lyfey.png) ](https://lyfeytech.co.ke/)

 

  * [Blog](https://lyfeytech.co.ke/blog-2/)
  * [About Us](https://lyfeytech.co.ke/about-us/)
  * [Privacy Policy](https://lyfeytech.co.ke/privacy-policy/)
  * [News](https://lyfeytech.co.ke/news/)



[Home](https://lyfeytech.co.ke "Go to Lyfey Technologies.") __[Blog](https://lyfeytech.co.ke/blog-2/ "Go to Blog.") __[Networking](https://lyfeytech.co.ke/category/networking/ "Go to the Networking category archives.") __[Huawei](https://lyfeytech.co.ke/category/networking/huawei/ "Go to the Huawei category archives.") __Basic IP MPLS VPN configuration on Huawei Routers

#  Basic IP MPLS VPN configuration on Huawei Routers 

# Basic IP MPLS VPN configuration on Huawei Routers

  * __March 29, 2024
  * Posted by: Lyfey Technologies
  * Categories: Huawei, Networking



[__401 Comments](https://lyfeytech.co.ke/basic-ip-mpls-vpn-configuration-on-huawei-routers/#comments)

![](https://lyfeytech.co.ke/wp-content/uploads/2024/03/IP_MPLS_VPN.png)

Configuration Steps

Step 1: Configure system names, interface IPs and enable MPLS on all routers in the MPLS backbone
    
    
    **********************************PE01
    sys
    sysname PE01
    #
    mpls lsr-id 1.1.1.1
    mpls
    #
    mpls ldp
    #
    interface GigabitEthernet0/0/2
     ip address 10.10.10.0 255.255.255.254
     mpls
     mpls ldp
    #
    interface LoopBack0
     ip address 1.1.1.1 255.255.255.255
    #
    
    
    **********************************P01
    sys
    sysname P01
    #
    mpls lsr-id 2.2.2.2
    mpls
    #
    mpls ldp
    #
    interface GigabitEthernet0/0/0
     ip address 10.10.10.1 255.255.255.254
     mpls
     mpls ldp
    #
    interface GigabitEthernet0/0/1
     ip address 10.10.10.2 255.255.255.254
     mpls
     mpls ldp
    #
    interface LoopBack0
     ip address 2.2.2.2 255.255.255.255
    #
    
    
    **********************************PE02
    sys
    sysname PE02
    #
    mpls lsr-id 3.3.3.3
    mpls
    #
    mpls ldp
    #
    interface GigabitEthernet0/0/0
     ip address 10.10.10.3 255.255.255.254
     mpls
     mpls ldp
    #
    interface LoopBack0
     ip address 3.3.3.3 255.255.255.255
    #

Step 2: Configure OSPF in the MPLS backbone and verify OSPF adjacency status.
    
    
    ******************************PE01
    #
    ospf 10 router-id 1.1.1.1
     area 0.0.0.0
      network 1.1.1.1 0.0.0.0
      network 10.10.10.0 0.0.0.1
    #
    
    
    ******************************P01
    #
    ospf 10 router-id 2.2.2.2
     area 0.0.0.0
      network 2.2.2.2 0.0.0.0
      network 10.10.10.0 0.0.0.1
      network 10.10.10.2 0.0.0.1
    #
    
    
    ******************************PE02
    #
    ospf 10 router-id 3.3.3.3
     area 0.0.0.0
      network 3.3.3.3 0.0.0.0
      network 10.10.10.2 0.0.0.1
    #

![](https://lyfeytech.co.ke/wp-content/uploads/2024/03/image-142.png)The adjacency status is established on P01 to PE01 and PE02. ![](https://lyfeytech.co.ke/wp-content/uploads/2024/03/image-143-1024x590.png)P01 has learned routes from PE01 and PE02.

Step 3: Configure MP-BGP between PE01 and PE02 and verify status.
    
    
    *****************************PE01
    #
    bgp 100
     router-id 1.1.1.1
     peer 3.3.3.3 as-number 100
     peer 3.3.3.3 connect-interface LoopBack0
     #
     ipv4-family unicast
      undo synchronization
      undo peer 3.3.3.3 enable
     #
     ipv4-family vpnv4
      policy vpn-target
      peer 3.3.3.3 enable
    #
    
    
    *****************************PE02
    #
    bgp 100
     router-id 3.3.3.3
     peer 1.1.1.1 as-number 100
     peer 1.1.1.1 connect-interface LoopBack0
     #
     ipv4-family unicast
      undo synchronization
      undo peer 1.1.1.1 enable
     #
    
     ipv4-family vpnv4
      policy vpn-target
      peer 1.1.1.1 enable
    #

![](https://lyfeytech.co.ke/wp-content/uploads/2024/03/image-144.png)BGP peering established between PE01 and PE02

Step 4: Configure VPN instances on PE01 and PE02 and bind customer facing interfaces to the VPN instances. Assign IP addresses to customer-facing interfaces.
    
    
    ********************************PE01
    ip vpn-instance CE01_CE03_VPN
     ipv4-family
      route-distinguisher 100:1
      vpn-target 100:1 export-extcommunity
      vpn-target 100:1 import-extcommunity
    #
    ip vpn-instance CE02_CE04_VPN
     ipv4-family
      route-distinguisher 100:2
      vpn-target 100:2 export-extcommunity
      vpn-target 100:2 import-extcommunity
    #
    interface GigabitEthernet0/0/0
     ip binding vpn-instance CE01_CE03_VPN
     ip address 10.10.10.4 255.255.255.254
    #
    interface GigabitEthernet0/0/1
     ip binding vpn-instance CE02_CE04_VPN
     ip address 10.10.10.6 255.255.255.254
    #
    
    
    ********************************PE02
    ip vpn-instance CE01_CE03_VPN
     ipv4-family
      route-distinguisher 100:1
      vpn-target 100:1 export-extcommunity
      vpn-target 100:1 import-extcommunity
    #
    ip vpn-instance CE02_CE04_VPN
     ipv4-family
      route-distinguisher 100:2
      vpn-target 100:2 export-extcommunity
      vpn-target 100:2 import-extcommunity
    #
    interface GigabitEthernet0/0/1
     ip binding vpn-instance CE01_CE03_VPN
     ip address 10.10.10.10 255.255.255.254
    #
    interface GigabitEthernet0/0/2
     ip binding vpn-instance CE02_CE04_VPN
     ip address 10.10.10.8 255.255.255.254
    #

Step 5: Configure BGP peering to CEs on PE01 and PE02
    
    
    *****************************PE01
    bgp 100
     ipv4-family vpn-instance CE01_CE03_VPN
      peer 10.10.10.5 as-number 200
     #
     ipv4-family vpn-instance CE02_CE04_VPN
      peer 10.10.10.7 as-number 300
    #
    
    
    *****************************PE02
    bgp 100
     ipv4-family vpn-instance CE01_CE03_VPN
      peer 10.10.10.11 as-number 300
     #
     ipv4-family vpn-instance CE02_CE04_VPN
      peer 10.10.10.9 as-number 400
    #

Step 7: Configure CEs
    
    
    ******************************CE01
    sys
    sysname CE01
    #
    interface GigabitEthernet0/0/0
     ip address 10.10.10.5 255.255.255.254
    #
    #
    interface LoopBack0
     ip address 6.6.6.6 255.255.255.255
    #
    bgp 200
     peer 10.10.10.4 as-number 100
     #
     ipv4-family unicast
      undo synchronization
      network 6.6.6.6 255.255.255.255
      peer 10.10.10.4 enable
    #
    commit
    
    
    ******************************CE02
    sys
    sysname CE02
    #
    interface GigabitEthernet0/0/0
     ip address 10.10.10.7 255.255.255.254
    #
    #
    interface LoopBack0
     ip address 7.7.7.7 255.255.255.255
    #
    bgp 300
     peer 10.10.10.6 as-number 100
     #
     ipv4-family unicast
      undo synchronization
      network 7.7.7.7 255.255.255.255
      peer 10.10.10.6 enable
    #
    commit
    
    
    ******************************CE03
    sys
    sysname CE03
    #
    interface GigabitEthernet0/0/0
     ip address 10.10.10.11 255.255.255.254
    #
    #
    interface LoopBack0
     ip address 4.4.4.4 255.255.255.255
    #
    bgp 400
     peer 10.10.10.10 as-number 100
     #
     ipv4-family unicast
      undo synchronization
      network 4.4.4.4 255.255.255.255
      peer 10.10.10.10 enable
    #
    commit
    
    
    ******************************CE04
    sys
    sysname CE04
    #
    interface GigabitEthernet0/0/0
     ip address 10.10.10.9 255.255.255.254
    #
    #
    interface LoopBack0
     ip address 5.5.5.5 255.255.255.255
    #
    bgp 400
     peer 10.10.10.8 as-number 100
     #
     ipv4-family unicast
      undo synchronization
      network 5.5.5.5 255.255.255.255
      peer 10.10.10.8 enable
    #
    commit

Step 8: Verify BGP peering between CEs and PEs is up and routes are propagated between CEs in the same VPN Instance.

![](https://lyfeytech.co.ke/wp-content/uploads/2024/03/image-145-1024x514.png) ![](https://lyfeytech.co.ke/wp-content/uploads/2024/03/image-146-1024x465.png) ![](https://lyfeytech.co.ke/wp-content/uploads/2024/03/image-147.png)

Thank You for reading our blogs, please your comments in the comments section and check out other related posts on our blog.

**Related Posts**

  * [Step by step guide on how to implement different networking protocols on Juniper MX routers](https://lyfeytech.co.ke/step-by-step-guide-on-how-to-implement-different-networking-protocols-on-juniper-mx-routers/)
  * [L2 EVPN Implementation on Huawei Routers.](https://lyfeytech.co.ke/l2-evpn-implementation-on-huawei-routers/)
  * [VRRP Monitoring of the Uplink Interface status on Huawei routers.](https://lyfeytech.co.ke/vrrp-monitoring-of-the-uplink-interface-status-on-huawei-routers/)
  * [Association between VRRP and BFD Implementation on Huawei routers.](https://lyfeytech.co.ke/association-between-vrrp-and-bfd-implementation-on-huawei-routers/)
  * [Association between VRRP and STP Implementation on Huawei routers.](https://lyfeytech.co.ke/association-between-vrrp-and-stp-implementation-on-huawei-routers/)



  
  


[BGP](https://lyfeytech.co.ke/tag/bgp/) [Huawei](https://lyfeytech.co.ke/tag/huawei/) [OSPF](https://lyfeytech.co.ke/tag/ospf/) [Troubleshooting](https://lyfeytech.co.ke/tag/troubleshooting/) [VPN](https://lyfeytech.co.ke/tag/vpn/)

### Leave a Reply [Cancel reply](/basic-ip-mpls-vpn-configuration-on-huawei-routers/#respond)

You must be [logged in](https://lyfeytech.co.ke/wp-login.php?redirect_to=https%3A%2F%2Flyfeytech.co.ke%2Fbasic-ip-mpls-vpn-configuration-on-huawei-routers%2F) to post a comment.

__

##### Recent Comments

  * [Crypto Predictions](https://cryptopredic.com) on [Implementing OSPF LSA filtering & Inter-area route filtering on Huawei Routers](https://lyfeytech.co.ke/implementing-ospf-lsa-filtering-inter-area-route-filtering-on-huawei-routers/#comment-1948)
  * [Lyfey Technologies](https://lyfeytech.co.ke) on [Implementation of Pseudo wire on Huawei Routers.](https://lyfeytech.co.ke/implementation-of-pseudo-wire-on-huawei-routers/#comment-45)
  * Imran on [Implementing HoVPN IPRAN Architecture on Huawei Routers](https://lyfeytech.co.ke/implementing-hovpn-ipran-architecture-on-huawei-routers/#comment-44)
  * Imran on [Implementing Aggregate VLAN on Huawei Switches](https://lyfeytech.co.ke/implementing-aggregate-vlan-on-huawei-switches/#comment-43)
  * [Majani James](https://lyfeytech.co.ke/) on [Implementing Aggregate VLAN on Huawei Switches](https://lyfeytech.co.ke/implementing-aggregate-vlan-on-huawei-switches/#comment-42)



##### Archives

  * [September 2025](https://lyfeytech.co.ke/2025/09/)
  * [February 2025](https://lyfeytech.co.ke/2025/02/)
  * [January 2025](https://lyfeytech.co.ke/2025/01/)
  * [December 2024](https://lyfeytech.co.ke/2024/12/)
  * [November 2024](https://lyfeytech.co.ke/2024/11/)
  * [October 2024](https://lyfeytech.co.ke/2024/10/)
  * [September 2024](https://lyfeytech.co.ke/2024/09/)
  * [August 2024](https://lyfeytech.co.ke/2024/08/)
  * [July 2024](https://lyfeytech.co.ke/2024/07/)
  * [June 2024](https://lyfeytech.co.ke/2024/06/)
  * [May 2024](https://lyfeytech.co.ke/2024/05/)
  * [April 2024](https://lyfeytech.co.ke/2024/04/)
  * [March 2024](https://lyfeytech.co.ke/2024/03/)
  * [February 2024](https://lyfeytech.co.ke/2024/02/)
  * [January 2024](https://lyfeytech.co.ke/2024/01/)



##### Categories

  * [Cisco](https://lyfeytech.co.ke/category/networking/cisco/)
  * [Firewalls](https://lyfeytech.co.ke/category/networking/firewall/)
  * [Huawei](https://lyfeytech.co.ke/category/networking/huawei/)
  * [Juniper](https://lyfeytech.co.ke/category/networking/juniper-networking/)
  * [Networking](https://lyfeytech.co.ke/category/networking/)
  * [Nokia](https://lyfeytech.co.ke/category/networking/nokia/)



##### Meta

  * [Log in](https://lyfeytech.co.ke/wp-login.php)
  * [Entries feed](https://lyfeytech.co.ke/feed/)
  * [Comments feed](https://lyfeytech.co.ke/comments/feed/)
  * [WordPress.org](https://wordpress.org/)



  * [ __](https://www.linkedin.com/in/martin-indeche-668402115/)
  * [ __](https://twitter.com/stylemix_themes/)
  * [ __](https://www.youtube.com/@LyfeyTechnologies)



#### Contact info

Phone: +254 739106372  
Email: [[email protected]](/cdn-cgi/l/email-protection)

#### Quick links

  * [Blog](https://lyfeytech.co.ke/blog-2/)
  * [About Us](https://lyfeytech.co.ke/about-us/)
  * [Privacy Policy](https://lyfeytech.co.ke/privacy-policy/)
  * [News](https://lyfeytech.co.ke/news/)



#### Our Service

  * [Company overview](https://lyfeytech.co.ke/company-overview/)
  * [Careers](https://lyfeytech.co.ke/company-overview/careers/)
  * [Company history](https://lyfeytech.co.ke/company-overview/company-history/)
  * [Our approach](https://lyfeytech.co.ke/company-overview/our-approach/)
  * [Partners](https://lyfeytech.co.ke/company-overview/our-partners/)
  * [Our team grid](https://lyfeytech.co.ke/company-overview/our-team-grid/)



© 2026 Lyfey Technologies - Networking Training & Consultancy 

This website uses cookies and asks your personal data to enhance your browsing experience.

Ok, I agree [Privacy Policy](https://lyfeytech.co.ke/privacy-policy/)

[Privacy Policy](https://lyfeytech.co.ke/wpautoterms/privacy-policy/)
