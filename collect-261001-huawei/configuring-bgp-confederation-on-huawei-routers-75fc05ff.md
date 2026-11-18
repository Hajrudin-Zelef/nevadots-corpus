# Configuring BGP Confederation on Huawei Routers

(Source archive : https://lyfeytech.co.ke/configuring-bgp-confederation-on-huawei-routers/ — capture 20260113001347)

[ ![Lyfey Technologies](https://lyfeytech.co.ke/wp-content/uploads/2024/01/lyfey.png) ](https://lyfeytech.co.ke/)

[ ![Lyfey Technologies](https://lyfeytech.co.ke/wp-content/uploads/2024/01/lyfey.png) ](https://lyfeytech.co.ke/)

 

  * [Blog](https://lyfeytech.co.ke/blog-2/)
  * [About Us](https://lyfeytech.co.ke/about-us/)
  * [Privacy Policy](https://lyfeytech.co.ke/privacy-policy/)
  * [News](https://lyfeytech.co.ke/news/)



[Home](https://lyfeytech.co.ke "Go to Lyfey Technologies.") __[Blog](https://lyfeytech.co.ke/blog-2/ "Go to Blog.") __[Networking](https://lyfeytech.co.ke/category/networking/ "Go to the Networking category archives.") __[Huawei](https://lyfeytech.co.ke/category/networking/huawei/ "Go to the Huawei category archives.") __Configuring BGP Confederation on Huawei Routers

#  Configuring BGP Confederation on Huawei Routers 

# Configuring BGP Confederation on Huawei Routers

  * __March 27, 2024
  * Posted by: Lyfey Technologies
  * Categories: Huawei, Networking



[__393 Comments](https://lyfeytech.co.ke/configuring-bgp-confederation-on-huawei-routers/#comments)

![](https://lyfeytech.co.ke/wp-content/uploads/2024/03/BGP_Confed.png)

BGP has different mechanisms for controlling loops in the network. These loop control mechanisms are explained in this article in detail. I[BGP Loop control mechanisms](https://lyfeytech.co.ke/ibgp-routing-loop-control-mechanisms/ "BGP Loop control mechanisms").

This article demonstrates how to set up and configure BGP confederation on Huawei routers.

Topology Diagram

![](https://lyfeytech.co.ke/wp-content/uploads/2024/03/image-128.png)

**Configuration Steps:**

Step 1: Configure system names and Interface IPs on all routers.
    
    
    ****************************R1
    sys
    sysname R1
    #
    interface GigabitEthernet0/0/0
     ip address 40.40.40.2 255.255.255.254
    #
    interface GigabitEthernet0/0/1
     ip address 40.40.40.1 255.255.255.254
    #
    interface LoopBack0
     ip address 1.1.1.1 255.255.255.255
    #
    
    
    ****************************R2
    sys
    sysname R2
    #
    interface GigabitEthernet0/0/0
     ip address 40.40.40.3 255.255.255.254
    #
    interface LoopBack0
     ip address 2.2.2.2 255.255.255.255
    #
    
    
    ****************************R3
    sys
    sysname R3
    #
    interface GigabitEthernet0/0/0
     ip address 40.40.40.0 255.255.255.254
    #
    interface GigabitEthernet0/0/1
     ip address 10.10.10.2 255.255.255.254
    #
    interface GigabitEthernet0/0/2
     ip address 10.10.10.4 255.255.255.254
    #
    interface GigabitEthernet0/0/3
     ip address 30.30.30.0 255.255.255.254
    #
    interface Ethernet0/0/0
     ip address 20.20.20.0 255.255.255.254
    #
    interface LoopBack0
     ip address 3.3.3.3 255.255.255.255
    #
    
    
    ****************************R4
    sys
    sysname R4
    #
    interface GigabitEthernet0/0/0
     ip address 10.10.10.5 255.255.255.254
    #
    interface GigabitEthernet0/0/1
     ip address 10.10.10.0 255.255.255.254
    #
    interface LoopBack0
     ip address 4.4.4.4 255.255.255.255
    #
    
    
    ****************************R5
    sys
    sysname R5
    #
    interface GigabitEthernet0/0/0
     ip address 10.10.10.3 255.255.255.254
    #
    interface GigabitEthernet0/0/1
     ip address 10.10.10.1 255.255.255.254
    #
    interface LoopBack0
     ip address 5.5.5.5 255.255.255.255
    #
    
    
    ****************************R6
    sys
    sysname R6
    #
    interface Ethernet0/0/0
     ip address 20.20.20.1 255.255.255.254
    #
    interface GigabitEthernet0/0/0
     ip address 20.20.20.2 255.255.255.254
    #
    interface LoopBack0
     ip address 6.6.6.6 255.255.255.255
    #
    
    
    ****************************R7
    sys
    sysname R7
    #
    interface GigabitEthernet0/0/0
     ip address 30.30.30.1 255.255.255.254
    #
    interface LoopBack0
     ip address 7.7.7.7 255.255.255.255
    #
    
    
    ****************************R8
    sys
    sysname R8
    #
    interface GigabitEthernet0/0/0
     ip address 20.20.20.3 255.255.255.254
    #
    interface LoopBack0
     ip address 8.8.8.8 255.255.255.255
    #

Step 2: Configure the BGP Confederation in AS 400.
    
    
    ********************************R3
    #
    bgp 300
     router-id 3.3.3.3
     confederation id 400
     confederation peer-as 100 200

     peer 20.20.20.1 as-number 200
     peer 30.30.30.1 as-number 100
     peer 40.40.40.1 as-number 500
     #
     ipv4-family unicast
      undo synchronization

      peer 20.20.20.1 enable
      peer 20.20.20.1 next-hop-local
      peer 30.30.30.1 enable
      peer 30.30.30.1 next-hop-local

    #
    
    
    **********************************R7
    #
    bgp 100
     router-id 7.7.7.7
     confederation id 400
     confederation peer-as 300
     peer 30.30.30.0 as-number 300
     #
     ipv4-family unicast
      undo synchronization
      peer 30.30.30.0 enable
    #
    
    
    #
    ******************************R6
    bgp 200
     router-id 6.6.6.6
     confederation id 400
     confederation peer-as 300
     peer 20.20.20.0 as-number 300

     #
     ipv4-family unicast
      undo synchronization
      peer 20.20.20.0 enable

    #

Step 3: Configure IBGP peering in AS 300.
    
    
    ******************************R3
    #
    bgp 300
     router-id 3.3.3.3
     confederation id 400
     confederation peer-as 100 200
     peer 10.10.10.3 as-number 300
     peer 10.10.10.5 as-number 300

     #
     ipv4-family unicast
      undo synchronization
      peer 10.10.10.3 enable
      peer 10.10.10.3 next-hop-local
      peer 10.10.10.5 enable
      peer 10.10.10.5 next-hop-local

    #
    
    
    ***************************R4
    #
    bgp 300
     router-id 4.4.4.4
     confederation id 400
     peer 10.10.10.1 as-number 300
     peer 10.10.10.4 as-number 300
     #
     ipv4-family unicast
      undo synchronization
      peer 10.10.10.1 enable
      peer 10.10.10.4 enable
    #
    
    
    ***************************R5
    #
    bgp 300
     router-id 5.5.5.5
     confederation id 400
     peer 10.10.10.0 as-number 300
     peer 10.10.10.2 as-number 300
     #
     ipv4-family unicast
      undo synchronization
      peer 10.10.10.0 enable
      peer 10.10.10.2 enable
    #

Step 4: Configure EBGP between AS 400 and AS 500
    
    
    ****************************R3
    bgp 300
     peer 40.40.40.1 as-number 500
     #
     ipv4-family unicast

      peer 40.40.40.1 enable
    #
    
    
    ***************************R1
    #
    bgp 500
     router-id 1.1.1.1
     peer 40.40.40.0 as-number 400

     #
     ipv4-family unicast
      undo synchronization
      peer 40.40.40.0 enable

    #

Step 5 : Configure IBGP in AS 500
    
    
    ***************************R1
    #
    bgp 500

     peer 40.40.40.3 as-number 500
     #
     ipv4-family unicast

      peer 40.40.40.3 enable
    #
    
    
    ***************************R2
    #
    bgp 500
     router-id 2.2.2.2
     peer 40.40.40.2 as-number 500
     #
     ipv4-family unicast
      undo synchronization
      peer 40.40.40.2 enable
    #
    commit

Step 5: Verify BGP peering status.

![](https://lyfeytech.co.ke/wp-content/uploads/2024/03/image-129-1024x463.png) ![](https://lyfeytech.co.ke/wp-content/uploads/2024/03/image-130.png)

Step 6: Verify the route 100.100.100.100/32 from R2 on R3 and R6

![](https://lyfeytech.co.ke/wp-content/uploads/2024/03/image-132-1024x442.png) ![](https://lyfeytech.co.ke/wp-content/uploads/2024/03/image-131-1024x374.png)

Thank You for reading our article. Please check more posts on our blog and leave your comments below.

**Related Posts**

  * [Step by step guide on how to implement different networking protocols on Juniper MX routers](https://lyfeytech.co.ke/step-by-step-guide-on-how-to-implement-different-networking-protocols-on-juniper-mx-routers/)
  * [L2 EVPN Implementation on Huawei Routers.](https://lyfeytech.co.ke/l2-evpn-implementation-on-huawei-routers/)
  * [VRRP Monitoring of the Uplink Interface status on Huawei routers.](https://lyfeytech.co.ke/vrrp-monitoring-of-the-uplink-interface-status-on-huawei-routers/)
  * [Association between VRRP and BFD Implementation on Huawei routers.](https://lyfeytech.co.ke/association-between-vrrp-and-bfd-implementation-on-huawei-routers/)
  * [Association between VRRP and STP Implementation on Huawei routers.](https://lyfeytech.co.ke/association-between-vrrp-and-stp-implementation-on-huawei-routers/)



  
  


[BGP](https://lyfeytech.co.ke/tag/bgp/) [Huawei](https://lyfeytech.co.ke/tag/huawei/) [IGP](https://lyfeytech.co.ke/tag/igp/)

### Leave a Reply [Cancel reply](/configuring-bgp-confederation-on-huawei-routers/#respond)

You must be [logged in](https://lyfeytech.co.ke/wp-login.php?redirect_to=https%3A%2F%2Flyfeytech.co.ke%2Fconfiguring-bgp-confederation-on-huawei-routers%2F) to post a comment.

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
