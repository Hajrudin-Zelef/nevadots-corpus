# BGP INTER-AS Option B Configuration and troubleshooting on Huawei routers

(Source archive : https://lyfeytech.co.ke/bgp-inter-as-option-b-configuration-and-troubleshooting-on-huawei-routers/ — capture 20260113001850)

[ ![Lyfey Technologies](https://lyfeytech.co.ke/wp-content/uploads/2024/01/lyfey.png) ](https://lyfeytech.co.ke/)

[ ![Lyfey Technologies](https://lyfeytech.co.ke/wp-content/uploads/2024/01/lyfey.png) ](https://lyfeytech.co.ke/)

 

  * [Blog](https://lyfeytech.co.ke/blog-2/)
  * [About Us](https://lyfeytech.co.ke/about-us/)
  * [Privacy Policy](https://lyfeytech.co.ke/privacy-policy/)
  * [News](https://lyfeytech.co.ke/news/)



[Home](https://lyfeytech.co.ke "Go to Lyfey Technologies.") __[Blog](https://lyfeytech.co.ke/blog-2/ "Go to Blog.") __[Networking](https://lyfeytech.co.ke/category/networking/ "Go to the Networking category archives.") __[Huawei](https://lyfeytech.co.ke/category/networking/huawei/ "Go to the Huawei category archives.") __BGP INTER-AS Option B Configuration and troubleshooting on Huawei routers

#  BGP INTER-AS Option B Configuration and troubleshooting on Huawei routers 

# BGP INTER-AS Option B Configuration and troubleshooting on Huawei routers

  * __February 15, 2024
  * Posted by: Lyfey Technologies
  * Categories: Huawei, Networking



[__405 Comments](https://lyfeytech.co.ke/bgp-inter-as-option-b-configuration-and-troubleshooting-on-huawei-routers/#comments)

![](https://lyfeytech.co.ke/wp-content/uploads/2024/02/BGP_OPTION_B.png)

Last time we looked at how to configure Inter-AS BGP MPLS Option-A. You can check out the article from this link: [INTER-AS BGP MPLS OPTION A](https://lyfeytech.co.ke/inter-as-bgp-mpls-ip-vpn-option-a-setup-and-configuration-on-huawei-routers/ "INTER-AS BGP MPLS OPTION A"). In this article, we will simulate how to configure INTER-AS BGP MPLS option B on Huawei routers.

**Quick facts about Inter-AS Option B:**

  1. It's more scalable and easy to configure compared to option A.
  2. Route Targets (RTs) are globally significant.
  3. MPLS runs between the ASBRs.
  4. We configure VPNV4 peering between ASBRs.
  5. NO VRF configurations on the ASBRs, VRFs are only configured on the PEs.
  6. ASBRs accepts all the VPNv4 routing information without the VPN target filtering. You need to disable VPN target filtering



**Topology Setup**

![](https://lyfeytech.co.ke/wp-content/uploads/2024/02/image-22-1024x527.png)

**Configuration Steps:**

**Step 1: Basic Configuration**

Configure IP addresses on interfaces on each router, Configure IGP(ISIS for our case), MPLS, and LDP on all interfaces within the AS. Refer to this article for IS-IS Configurations, [Configuring ISIS on Huawei](https://lyfeytech.co.ke/isis-protocol-configuration-on-huawei-routers/ "Configuring ISIS on Huawei"). Below is sample IS-IS and interface configuration for PE01:
    
    
    *******************************PE01
    sys
    isis 20
     is-level level-2
     cost-style wide
     network-entity 49.0000.0020.0200.2002.00
     is-name PE01
    #
    interface GigabitEthernet0/0/1
     ip address 10.251.250.3 255.255.255.254
     isis enable 20
     isis circuit-type p2p
     isis cost 100
     mpls
     mpls ldp
    #

Let's verify our IGP and MPLS sessions are established by checking their status on the P router in each AS using the commad **display isis peer**

![](https://lyfeytech.co.ke/wp-content/uploads/2024/02/image-23.png)We have two IS-IS peers on P01 router. ![](https://lyfeytech.co.ke/wp-content/uploads/2024/02/image-24.png)We have two IS-IS peers on P02 router

**Step 2: BGP Configurations for backbone: Configure IBGP VPNV4 peering between PEs and ASBRs. Configure EBGP VPNV4 between ASBRs. Note: You must disable VPN target filtering on both ASBRs.**
    
    
    *******************************PE01**********************************
    bgp 200
     router-id 4.4.4.4
     peer 4.4.4.4 as-number 200
     peer 4.4.4.4 connect-interface LoopBack0
     #
     ipv4-family unicast
      undo synchronization
      undo peer 4.4.4.4 enable
     #
     ipv4-family vpnv4
      undo policy vpn-target
      peer 4.4.4.4 enable
     #
    
    
    ********************************ASBR01********************************
    bgp 200
     peer 2.2.2.2 as-number 200
     peer 172.16.16.2 as-number 100
     #
     ipv4-family unicast
      undo synchronization
      undo peer 2.2.2.2 enable
      undo peer 172.16.16.2 enable
     #
     ipv4-family vpnv4
      undo policy vpn-target
      peer 2.2.2.2 enable
      peer 172.16.16.2 enable
    #
    
    
    *******************************ASBR02*********************************
    #
    bgp 100
     peer 7.7.7.7 as-number 100
     peer 7.7.7.7 connect-interface LoopBack0
     peer 172.16.16.3 as-number 200
     #
     ipv4-family unicast
      undo synchronization
      undo peer 7.7.7.7 enable
      undo peer 172.16.16.3 enable
     #
     ipv4-family vpnv4
      undo policy vpn-target
      peer 7.7.7.7 enable
      peer 172.16.16.3 enable
    #
    
    
    ******************************PE02*****************************************
    #
    bgp 100
     peer 5.5.5.5 as-number 100
     peer 5.5.5.5 connect-interface LoopBack0
     #
     ipv4-family unicast
      undo synchronization
      peer 5.5.5.5 enable
     #
     ipv4-family vpnv4
      undo policy vpn-target
      peer 5.5.5.5 enable
     #
     ipv4-family vpn-instance VRF1
      peer 20.20.20.3 as-number 65000
    #

**Step 3: Configure VPN instance on PEs and bind the interfaces connecting to the customer to the VPN instance. Pay attention to route targets.**
    
    
    *************************************PE01************************
    ip vpn-instance VRF1
     ipv4-family
      route-distinguisher 200:1
      vpn-target 200:1 export-extcommunity
      vpn-target 200:1 import-extcommunity
    #
    interface GigabitEthernet0/0/0
     ip binding vpn-instance VRF1
     ip address 10.10.10.2 255.255.255.254
    #
    
    
    ***********************************PE02****************************
    ip vpn-instance VRF1
     ipv4-family
      route-distinguisher 65000:1
      vpn-target 200:1 export-extcommunity
      vpn-target 200:1 import-extcommunity
    #
    interface GigabitEthernet0/0/1
     ip binding vpn-instance VRF1
     ip address 20.20.20.2 255.255.255.254
    #

**Step 4: Configure PE-CE routing. BGP is recommended for PE-CE routing protocol.**
    
    
    ************************************PE01*******************************
    bgp 200
     ipv4-family vpn-instance VRF1
      network 50.50.50.50 255.255.255.255
     #
    
    
    ***********************************Customer01***************************
    bgp 65500
     router-id 1.1.1.1
     peer 10.10.10.2 as-number 200
     #
     ipv4-family unicast
      undo synchronization
    
  import-route direct
      peer 10.10.10.2 enable
    #
    
    
    ***************************PE02
     ipv4-family vpn-instance VRF1
      peer 20.20.20.3 as-number 65000
    #
    
    
    ***************************Customer2
    bgp 65000
     peer 20.20.20.2 as-number 100
     #
     ipv4-family unicast
      undo synchronization

      import-route direct
      peer 20.20.20.2 enable
    #

**Step 5: Verification.**

![](https://lyfeytech.co.ke/wp-content/uploads/2024/02/image-25.png)ASBR has a peering to PE01 and ASBR02. ![](https://lyfeytech.co.ke/wp-content/uploads/2024/02/image-26-1024x411.png)PE02 has a peering to ASB02 and Customer 2 ![](https://lyfeytech.co.ke/wp-content/uploads/2024/02/image-27-1024x362.png)PE01 is learning routes from PE02 ![](https://lyfeytech.co.ke/wp-content/uploads/2024/02/image-28.png)Customer 01 is learning routes from Customer 2. ![](https://lyfeytech.co.ke/wp-content/uploads/2024/02/image-29.png)Customer 01 can communicate with customer 02.

  
  


[BGP](https://lyfeytech.co.ke/tag/bgp/) [Huawei](https://lyfeytech.co.ke/tag/huawei/) [ISP](https://lyfeytech.co.ke/tag/isp/) [MPLS](https://lyfeytech.co.ke/tag/mpls/)

####  405 Comments 

  * [Implementing Inter-AS BGP MPLS VPN Option C on Huawei Routers. - Lyfey Technologies](https://lyfeytech.co.ke/implementing-inter-as-bgp-mpls-vpn-option-c-on-huawei-routers/)

[ March 5, 2024 at 8:02 pm ](https://lyfeytech.co.ke/bgp-inter-as-option-b-configuration-and-troubleshooting-on-huawei-routers/#comment-20) [Log in to Reply](https://lyfeytech.co.ke/wp-login.php?redirect_to=https%3A%2F%2Flyfeytech.co.ke%2Fbgp-inter-as-option-b-configuration-and-troubleshooting-on-huawei-routers%2F)

[…] Inter-AS BGP MPLS VPN Option B implementation on Huawei routers […]




### Leave a Reply [Cancel reply](/bgp-inter-as-option-b-configuration-and-troubleshooting-on-huawei-routers/#respond)

You must be [logged in](https://lyfeytech.co.ke/wp-login.php?redirect_to=https%3A%2F%2Flyfeytech.co.ke%2Fbgp-inter-as-option-b-configuration-and-troubleshooting-on-huawei-routers%2F) to post a comment.

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
