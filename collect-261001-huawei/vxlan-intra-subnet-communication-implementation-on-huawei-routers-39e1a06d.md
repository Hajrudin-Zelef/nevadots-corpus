# VXLAN (intra-subnet communication) Implementation on Huawei switches.

(Source archive : https://lyfeytech.co.ke/vxlan-intra-subnet-communication-implementation-on-huawei-routers/ — capture 20260113002542)

[ ![Lyfey Technologies](https://lyfeytech.co.ke/wp-content/uploads/2024/01/lyfey.png) ](https://lyfeytech.co.ke/)

[ ![Lyfey Technologies](https://lyfeytech.co.ke/wp-content/uploads/2024/01/lyfey.png) ](https://lyfeytech.co.ke/)

 

  * [Blog](https://lyfeytech.co.ke/blog-2/)
  * [About Us](https://lyfeytech.co.ke/about-us/)
  * [Privacy Policy](https://lyfeytech.co.ke/privacy-policy/)
  * [News](https://lyfeytech.co.ke/news/)



[Home](https://lyfeytech.co.ke "Go to Lyfey Technologies.") __[Blog](https://lyfeytech.co.ke/blog-2/ "Go to Blog.") __[Networking](https://lyfeytech.co.ke/category/networking/ "Go to the Networking category archives.") __[Huawei](https://lyfeytech.co.ke/category/networking/huawei/ "Go to the Huawei category archives.") __VXLAN (intra-subnet communication) Implementation on Huawei switches.

#  VXLAN (intra-subnet communication) Implementation on Huawei switches. 

# VXLAN (intra-subnet communication) Implementation on Huawei switches.

  * __September 15, 2024
  * Posted by: James Majani
  * Categories: Huawei, Networking



[__399 Comments](https://lyfeytech.co.ke/vxlan-intra-subnet-communication-implementation-on-huawei-routers/#comments)

![](https://lyfeytech.co.ke/wp-content/uploads/2024/09/Screenshot-2024-09-15-211015.png)

Virtual extensible Local-Area Network (VXLAN) is a network virtualization technology standard. It caters to the limitations of the VLAN, enhancing scalability, performance, security, and network virtualization through isolated segmentation in cloud environments. VXLANs overcome VLAN scaling limitations in the following ways:

1) You can theoretically create as many as 16 million VXLANs in an administrative domain, compared to a maximum of 4094 traditional VLANs. In this way, VXLANs provide network segmentation at the scale required by cloud and service providers to support very large numbers of tenants.  
2) VXLANs enable you to create network segments that stretch between data centers. Traditional VLAN-based network segmentation creates broadcast domains, but as soon as a packet containing VLAN tags hits a router, all of that VLAN information is removed. This means VLANs only travel as far as your underlying Layer 2 network can reach. This is a problem for some use cases, such as virtual machine (VM) migration, which typically prefers not to cross Layer 3 boundaries. By contrast, VXLAN network segmentation encapsulates the original packet inside a UDP packet. This allows a VXLAN network segment to extend as far as the physical Layer 3 routed network can reach, provided that all switches and routers in the path support VXLAN, without the applications running on the virtual overlay network being required to cross any Layer 3 boundaries. As far as the servers connected to the network are concerned, they are part of the same Layer 2 network, even though the underlying UDP packets may have transited one or more routers.  
3) The ability to provide Layer 2 segmentation over the top of an underlying Layer 3 network, combined with the high number of supported network segments, allows servers to be part of the same VXLAN even if they are remote from one another while enabling network administrators to keep Layer 2 networks small. Having smaller Layer 2 networks helps avoid MAC table overflow on switches.

**Networking Description**.

Configure a VXLAN tunnel between the two switches to enable the three PCs on the same network segment to communicate with each other. 

**Step 1: Basic configurations.**
    
    
    *******************************************CE1
    sysname CE1
    #
    #
    interface GE1/0/0
     undo portswitch
     undo shutdown
     ip address 10.1.0.1 255.255.255.252
    #
    interface LoopBack0
     description VTEP
     ip address 10.0.0.1 255.255.255.255
    #
    interface LoopBack1
     description ROUTER-ID
     ip address 1.1.1.1 255.255.255.255
    
    *******************************************CE2
    sysname CE2
    #
    interface GE1/0/0
     undo portswitch
     undo shutdown
     ip address 10.1.0.2 255.255.255.252
    #
    interface LoopBack0
     description VTEP
     ip address 10.0.0.2 255.255.255.255
    #
    interface LoopBack1
     description ROUTER-ID
     ip address 2.2.2.2 255.255.255.255

**Step 2: Configure OSPF between the two switches.**
    
    
    *******************************************CE1
    ospf 1 router-id 1.1.1.1
     area 0.0.0.0
      network 10.0.0.1 0.0.0.0
      network 10.1.0.0 0.0.0.3
    
    *******************************************CE2
    ospf 1 router-id 2.2.2.2
     area 0.0.0.0
      network 10.0.0.2 0.0.0.0
      network 10.1.0.0 0.0.0.3

**Step 3: Configure BD, VNI, VTEPs and VAPs .**
    
    
    *******************************************CE1
    bridge-domain 10
     vxlan vni 100
    #
    interface Nve1
     source 10.0.0.1
     vni 100 head-end peer-list 10.0.0.2
    #
    interface GE1/0/1
     undo shutdown
    #
    interface GE1/0/1.1 mode l2
     encapsulation untag
     bridge-domain 10
    #
    interface GE1/0/2
     undo shutdown
    #
    interface GE1/0/2.1 mode l2
     encapsulation untag
     bridge-domain 10
    
    *******************************************CE2
    bridge-domain 10
     vxlan vni 100
    #
    interface Nve1
     source 10.0.0.2
     vni 100 head-end peer-list 10.0.0.1
    #
    interface GE1/0/1
     undo shutdown
    #
    interface GE1/0/1.1 mode l2
     encapsulation untag
     bridge-domain 10
    

> In static mode, there is no control plane. During VXLAN tunnel establishment, you need to manually specify the IP addresses of the local VTEP and remote VTEP as the VXLAN tunnel's source and destination IP addresses, respectively. The static mode has poor flexibility and requires a lot of manual configuration, it is not applicable to large-scale networking scenarios.

**Step 4: Results confirmation.**

![](https://lyfeytech.co.ke/wp-content/uploads/2024/09/image-4.png) ![](https://lyfeytech.co.ke/wp-content/uploads/2024/09/image-5.png) ![](https://lyfeytech.co.ke/wp-content/uploads/2024/09/image-6.png) ![](https://lyfeytech.co.ke/wp-content/uploads/2024/09/image-7.png)

  
  


[Huawei](https://lyfeytech.co.ke/tag/huawei/) [IGP](https://lyfeytech.co.ke/tag/igp/) [OSPF](https://lyfeytech.co.ke/tag/ospf/) [Troubleshooting](https://lyfeytech.co.ke/tag/troubleshooting/) [VXLAN](https://lyfeytech.co.ke/tag/vxlan/)

### Leave a Reply [Cancel reply](/vxlan-intra-subnet-communication-implementation-on-huawei-routers/#respond)

You must be [logged in](https://lyfeytech.co.ke/wp-login.php?redirect_to=https%3A%2F%2Flyfeytech.co.ke%2Fvxlan-intra-subnet-communication-implementation-on-huawei-routers%2F) to post a comment.

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
