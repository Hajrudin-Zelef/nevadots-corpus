---
id: collect-261121-veille-ia/gemini-google/deploying-meraki-vmx-in-a-transit-vpc-with-aws-cloud-wan-tunnel-less-connect
title: "Deploying Meraki vMX in a Transit VPC with AWS Cloud WAN Tunnel-Less Connect"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["network", "cloud", "connect", "tunnel-less", "core", "global", "transit", "meraki"]
source: https://documentation.meraki.com/SASE_and_SD-WAN/MX/Design_and_Configure/Deployment_Guides/Deploying_Meraki_vMX_in_a_Transit_VPC_with_AWS_Cloud_WAN_Tunnel-Less_Connect
source_anchor: ""
source_lines: [1, 294]
section: "Gemini_Google"
---
## Overview

This document goes over the step-by-step configuration and provides a reference architecture for deploying the Meraki vMXs with AWS Cloud WAN. It helps organizations to extend their Meraki SD-WAN fabric to the application running on AWS.

## Why AWS Cloud WAN

Customers are increasingly moving towards multi-region deployments in the cloud and require a more dynamic, secure and a reliable way to connect from branch sites to cloud workloads across different regions. While this can be done using VPC peering across different regions and using static routes, this can quickly become cumbersome and difficult to manage.

AWS Cloud WAN provides a central dashboard for making connections between your branch offices, data centers, and Amazon Virtual Private Clouds (VPCs) in just a few clicks. With Cloud WAN, you use network policies to automate network management and security tasks in one location. Cloud WAN generates a complete view of your on-premises and AWS networks to help you monitor network health, security, and performance.

Meraki vMX integrates with AWS Cloud WAN to allow admins to define a multi-region, segmented, dynamically routed global network with intent-driven policies. This allows organizations to scale across different regions without worrying about managing the complexity of peering across different regions.

## Tunnel-Less Connect

Tunnel-less connect provides a simple and high-performance way to build global SD-WANs using the AWS Global network as a middle-mile transport network. Tunnel-less connect allows SD-WAN appliances to peer natively with Cloud WAN using BGP without any sort of tunneling technology like GRE or IPsec. The key benefits of this are:

- 
    **Cloud-enabled SD-WAN:** Simplify branch connectivity between on premises resources and the AWS cloud. With tunnel-less connect, these SD-WAN appliances can be onboarded into AWS with less overhead and higher throughput.
- 
    **AWS Global Network as a middle-mile for inter-office connectivity:** AWS Cloud WAN can provide high throughput and low latency connectivity for geographically disperse sites, using the AWS cloud as a backbone network.
- 
    **Improved throughput performance:** Without GRE or IPsec adding overhead and reducing overall effective throughput, tunnel-less connect provides much higher performance and simplicity, with up to 100Gbps of bandwidth per attachment.

## Cloud WAN Components

The main components of Cloud WAN with Tunnel-less connect are:

- 
    **Global network:** Private network that acts as the high-level container for your network objects. Global networks can contain AWS Transit Gateways and Cloud WAN core networks.
- 
    **Cloud WAN Core Network:** Part of your global network managed by AWS. Includes regional connection points and attachments, such as VPNs, VPCs, and Transit Gateway Connects. Your core network operates in the regions that are defined in your core network policy document.
- 
    **Core Network Edge (CNE):** Regional connection points managed by AWS in each Region, as defined in the core network policy. Under the hood, CNEs are AWS Transit Gateways, and they inherit many of the same properties.
- 
    **Attachments:** Connections or resources that you want to add to your core network. Supported attachments include VPCs, VPNs, Transit Gateway route table attachments and Connect attachments.
- 
    **Core network policy:** Single document applied to your core network that captures your intent and deploys it for you. The policy uses a declarative language defining segments, AWS region routing and attachment to segment mappings.
- 
    **Network segment:** Segments are dedicated routing domains, similar to globally consistent Virtual Routing and Forwarding (VRF) tables. By default, only attachments within the same segment can communicate. Segment actions can be defined to share routes across segments in the core network policy.
- 
    **Connect Attachment (Tunnel-less):** Peering point for your SD-WAN appliances that functions in a tunnel-less manner and uses native BGP to dynamically exchange routing and reachability information between SD-WAN appliances in the VPC and the CNE.
- 
    **VPC Attachments:** VPC attachments act as transport attachments and carry data-plane traffic between SD-WAN appliances and the CNE.
- 
    **Transport VPCs:** These are dedicated VPCs for hosting your SD-WAN appliances, in this case Cisco Meraki virtual MXs (vMX). These VPCs hold only the necessary resources to provide connectivity to your vMXs, and do not host any additional workloads or services. One Transport VPC is recommended for every geographic area where you have SD-WAN services that need connectivity to your AWS resources. It is also recommended to host your vMXs in pairs, in separate Availability Zones (AZs) for maximum availability.
- 
    **Workload VPCs:** Workload VPCs host any of the compute applications in your AWS environment. In practice, it is common to have multiple types of workload VPCs, such as Production, Development and Testing, but in this guide we will just deploy Production VPCs (the same steps can be repeated for any number of additional workload types). For high availability of workloads, it is common to have dedicated Workload VPCs in every region where you need AWS services.

## Solution Architecture

In this guide we will guide you through the steps to set up an environment with a core network interconnecting two separate AWS regions, each with their own Transit and Workload VPCs. These Transit and Workload VPCs will be assigned to two separate segments, SD-WAN and Production, and we will enable segment sharing between these for end-to-end connectivity.

The sample topology is as below:

For every region in your deployment, you will want to have a dedicated SD-WAN or Transit VPC to host your vMXs. In this guide, we will deploy these VPCs in the us-east-1 and us-west-1 regions. In each of these VPCs, you will want to deploy a pair of subnets, each in a separate Availability Zone (AZ) within the region for maximum availability. Each of these subnets will host one vMX only, so /28 addressing is fine for these (5 addresses of every subnet are reserved for AWS usage). All these regional SD-WAN VPCs will be associated with the SD-WAN segment in Cloud WAN.

You can deploy any number of workload VPCs in each of these regions. If your environment has multiple workload environments (Prod, Dev, Test), it is recommended to have a dedicated Cloud WAN segment for each. In this case, we will only be using a single Production segment for all the workload VPCs.

A single Cloud WAN core network will be provisioned, with edge locations in us-east-1 and us-west-1. Each of these edge locations will have a Core Network Edge (CNE) associated with it, and the core network edge will have VPC attachments to each of the SD-WAN and Production VPCs in its own region. Additionally, over the SD-WAN VPC attachment, a Connect attachment will be deployed for each of the subnets and AZs in the VPC (two per VPC). These Connect attachments will be provisioned in Tunnel-less Connect or no encapsulation mode, and these will be the endpoints that will peer via EBGP with your vMXs in the VPC.

The Meraki SD-WAN fabric will have a single ASN associated with it, while the Core Network will deploy one ASN for every CNE deployed (two in this case, due to operating in two regions). The BGP ASNs will be assigned as:

We will also enable bidirectional segment sharing between the two segments (SD-WAN and Production), which will allow them to advertise all their prefixes to each other.

By default, Meraki AutoVPN hubs form tunnels to each other. For large scale deployments, it is often recommended to disable hub-to hub-tunnels in the Meraki SD-WAN AutoVPN to achieve routing that is as deterministic as possible. This is because:

1. In AutoVPN, hubs automatically share prefixes they learn with each other
2. Hubs will perform path pre-pending for any spoke prefixes they learn as they advertise them to their EBGP peers, where the primary hub for a spoke prefix will announce its AS_PATH once, the secondary will announce its AS_PATH twice and so forth
3. However, for prefixes a hub learns from a different hub, no path pre-pending is done
4. This means that EBGP peers learning of a prefix belonging to a different hub from the ones they're peered to directly may interchangeably use a any of the peers they're learning this prefix from using ECMP and introducing asymmetric routing in the network, which is often undesirable

Additionally, if you do not disable hub to hub tunnels, traffic traversing two vMX hubs in different AWS regions (e.g. so that a spoke in Region 1 communicates with a spoke in Region 2) might not use the AWS Cloud WAN backbone and use the public Internet instead, which may partially defeat the purpose of using Cloud WAN. For more information on Meraki's BGP implementation see this documentation article.

You can request for this to be disabled for your organization through Meraki support. If you do end up disabling this functionality, steps 12 and 13 of this guide describe the process to allow cross-regional communications when hubs don't form tunnels to each other.

## Deployment Steps

### Step 1) Prepare AWS environment and deploy SD-WAN VPC in your desired regions.

1. Log into your AWS Console and navigate to the __VPC console__

2. Choose your desired base region, for example us-west-1 in our case.

3. Create the required VPC resources for the SD-WAN VPC and the Production VPC as mentioned on the AWS VPC getting started __guide__. Optionally, add tags to your resources for future reference and automation.

4. Deploy the necessary subnets. It is recommended to deploy at least two subnets in the SD-WAN, each in a separate AZ for your vMXs to be highly available. In this case we will be deploying two subnets in two dedicated AZs.

5. Make sure that the VPC route tables allow access to the Internet through an __Internet Gateway__ or __NAT Gateway__.

6. Repeat steps 1-5 for each other region where you will have Transit VPCs. In the case of the guide, us-west-1 and us-east-1.

### Step 2) Deploy vMXs in Dashboard and AWS

1. 
    Deploy two vMXs in each of the VPCs created in Step 1. Make sure to reference the VPCs and subnets created before when deploying.
2. 
    You can deploy vMXs following either the __manual step by step deployment guide__ , or the automated deployment using the__Cloud Integrations functionality.__ This second option requires API Access Keys to AWS with the proper permissions, so if this is not feasible use the manual option.
3. 
    Make sure to choose the appropriate vMX size for your deployment using our __Meraki MX sizing guide__ .
4. 
    Once your vMXs are deployed, navigate to your Meraki Dashboard and to each of the four vMX networks.
5. 
    In each network, navigate to Security & SD-WAN → Addressing and VLANs. Set the mode of operation to Passthrough or VPN concentrator.

1. 
    In each network, navigate to Security & SD-WAN → Site-to-site VPN. Set the VPN mode to Hub.

### Step 3) Set up AWS Cloud WAN

1. 
    Navigate to Cloud WAN in your AWS console

1. 
    Click on Create global network and give your global network a name, and then click Next

1. 
    Specify a name for the core network.

1. 
    Scroll down to core network policy settings, and specify a range of ASNs for your core network. These are used to provision CNEs in your regions, with each CNE requiring a dedicated ASN. Specify the edge locations where you want to deploy these CNEs (us-east-1 and us-west-1 in this case), and create an SDWAN segment to start.

1. 
    Click create global network.

1. 
    Navigate to Core network - attachments and click Create attachment.

1. 
    Deploy an attachment to your first Transit VPCs. Set the attachment type to VPC.

1. 
    Select appliance mode support, and reference your VPC’s ID, as well as the two subnets you deployed in each of the transit VPCs. Optionally, add tags for future reference.

1. 
    Repeat steps 6-8 for your second Transit VPC.

### Step 4) Edit your core network policy

1. Navigate to Core network → Policy versions

2. Select the latest executed policy and click on Edit

3. Create an Inside CIDR blocks and provision a block of addresses. This block of addresses will be used to provision BGP peering points for your connect attachments at each CNE. Each connect attachment peer will use up 2 IP addresses.

4. Edit the edge location for one of the regions

1. 
    Allocate an ASN for this edge location from the pool assigned to the core network, and an Inside CIDR block for it from the large block you configured above. This CIDR block is used to provision BGP peers in Connect attachments for this edge location, so consider future growth (each connect attachment uses at least 2 of these addresses).

6. Edit the other edge location and provision an ASN and Inside CIDR block for it.

7. Select Segments, and choose the Create segment option.

8. Provision a new segment for the Production workloads. If you will have other types of workloads (Production, Development, Testing) make sure to add segments for those.

9. Navigate to attachment policies and choose to Create a new attachment policy.

10. Assign rule number 100, specify a policy to assign any attachment with the tag/value pair segment/SDWAN to the SDWAN segment.

11. Repeat this process for the Production segment

12. Select Create policy

13. From the policy versions screen, select your newly created policy (likely the one labeled LATEST), and choose View or apply change set, and then choose Apply change set.

### Step 5) Edit the route table for the SD-WAN VPC and verify that the vMX Security Group has the appropriate permissions

1. 
    Navigate to the SD-WAN VPC in each of your regions and update the route table so that RFC1918 prefixes (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16) point to the newly created core network attachment.

1. 
    Navigate to EC2 in each of your regions, and select Security Groups

1. 
    Create a security group for your Transit VPC vMXs, and for the Inbound rules allow TCP port 179 from the Inbound CIDR you created for Cloud WAN in the previous section (allows BGP peering). For the Outbound rules allow all traffic to 0.0.0.0/0. This ensures your vMX is able to communicate with the Meraki Cloud.

1. 
    Navigate to your instances in each region and apply the new security group to all transit vMX instances.

### Step 6) Deploy Tunnel-less Connect Attachment

1. 
    In your AWS Console, navigate to Core network → Attachments, and select Create attachment
2. 
    Choose Connect attachment as the attachment type, and choose one of the two regions with Transit VPCs

1. 
    Specify Tunnel-less (No Encapsulation) as the Connect protocol, and select the VPC attachment for the Transit VPC as the Transport Attachment ID

1. 
    Make sure to add a Tag at the bottom with Key **SDWAN** and Value**cloudwan-segment** . This tag makes it so the attachment is associated with the SDWAN segment of Cloud WAN.

1. 
    After deployment, verify that your Connect attachment is associated with the SDWAN segment by clicking on it and clicking details.

1. 
    Navigate to your Meraki Dashboard, and select the network of your first vMX in this transit VPC
2. 
    Navigate to Security & SD-WAN → Appliance Status → Uplink, and make note of the private IP address assigned to the vMX uplink

1. 
    Navigate to Security & SD-WAN → Routing, and turn on BGP for this vMX, and specify a private ASN for your AutoVPN mesh (this is shared across the entire mesh) but leave the peers blank for now (don’t save yet)

1. 
    Back in your AWS Console in the Cloud WAN attachments list, select the Connect attachment you just created and choose Connect peers → Create Connect peer

1. 
    For the first Connect peer, specify the IP address of the first vMX in the VPC that you wrote down in item 5, as well as the ASN you specified in 6, and select the subnet this vMX belongs to
2. 
    Wait a few seconds for the Connect peer to show up in your connect peers list, and look for the IP addresses assigned to each of the two BGP interfaces

1. 
    Go back to your Meraki Dashboard where you were configuring your vMX BGP settings, and add the IP address of each of the two BGP interfaces for the Connect peer, and set the EBGP multihop count to 2

1. 
    Repeat steps 7-10 for the other vMX in the VPC (create a second connect peer in the same Connect attachment for it)
2. 
    Repeat steps 1-11 for the Transit VPC in the other region

### Step 7) Connect branches to transit vMXs

1. 
    Navigate back to your Meraki Dashboard
2. 
    For every group of branches that needs to connect to your Transit VPCs, navigate to Site-to-site VPN, specify them as Spokes, and specify the two Transit vMXs you have in their respective region as priority 1 and 2

1. 
    Repeat these steps for all regions where you have Transit VPCs, making sure to group the appropriate branches to it (for example, branches in the West Coast use us-west-1, and branches in the East Coast use us-east-1)

### Step 8) Adding a Workload VPC to the Production Segment

1. 
    Navigate to your AWS Console → Cloud WAN → Core network → Attachments, and for every workload VPC in your environment that needs connectivity to your branches, create a new VPC attachment in the appropriate region, allowing the needed subnets and the tag/value pair segment/Production to make sure the attachment is routed to the proper segment

1. 
    Navigate to your VPC Dashboard for each of these Workload VPCs and update the route tables your instances are using to have a default route to the VPC attachment you created for this VPC. You may choose to do more specific routes in your environment, but we’re using a default route for simplicity in this example.

1. 
    Navigate to your EC2 Dashboard, and make sure to update the Security Group of the instances your branches need reachability to, to allow the necessary protocols and IP addresses inbound. In our case, we’re just allowing ICMPv4 from the RFC1918 addresses for testing connectivity, and SSH from our own public IP. This will likely vary for your environment.

### Step 9) Implement segment sharing between Production and SDWAN

1. By default, segments do not share routes with each other. You can confirm this by navigating to Core network → Routes and entering a given segment and region and hit search. You will see that only that own segment’s routes are populated.
2. To implement segment sharing, navigate to Core network → Policy versions and edit your latest active policy.
3. 3. Navigate to Segment actions and under Sharing click Create
4. 4. Select the SDWAN segment, and choose Allow selected (or all if you want to share with all segments), and pick the desired segments from the list. By default, segment sharing is bidirectional, so sharing SDWAN to Production also shares Production to SDWAN. Click Create sharing.
5. Select create policy.
6. Select your new policy (should say LATEST) and click View or apply change set and click Apply change set in the next screen
7. After your policy finishes executing, navigate back to Core network → Routes and input a segment and region and click search. You should now see the other segment’s routes in this routing table as well.
8. Navigate to your Meraki Dashboard, and choose one of the branches connected to your Transit vMXs. Go to Security & SD-WAN → Route table, and you should see the routes from the Production segment in your route table. You can do the same verification from your Transit vMXs.

### Step 10) Testing connectivity for the first region

1. 
    If you wish to test connectivity to any of your vMXs, you need to add an inbound ICMP rule for RFC 1918 prefixes in the security group associated with them. You can navigate to your EC2 instances, pick a vMX and select the Security tab. Then click on the security group to edit it.
2. 
    Navigate to your Meraki Dashboard, select a branch network and go to Security & SD-WAN → Appliance status → Tools, and initiate pings to the private IPs of your transit VPC vMXs
3. 
    From the same page you can add a few pings toward the EC2 instances within your Workload VPCs in the same region, and any cross-region workloads

### Step 11) Testing redundancy

1. 
    You can test redundancy for your branches by bringing down the primary vMX in their corresponding Transit VPC. You can do this by navigating to EC2 in the AWS Console and choosing the primary vMX. Then from the Instance state choose Stop instance. This process can take up to 5 minutes to complete, so you will want to wait. You can also Force stop the instance, by selecting the instance state again.
2. 
    After the vMX goes down, you can attempt a new ping from the branches to your instances and vMXs. BGP may take some time to converge (up to the hold timer set to 240s by default). After convergence, your workloads should respond again, and the primary vMX should be down.

### Step 12) Allowing branch-to-branch connectivity across regions

1. 
    If you have disabled hub-to-hub connectivity as mentioned in the solution architecture section of this document, and you wish to allow branches connecting to different transit VPCs to use Cloud WAN as transit to communicate with each other, you need to do some additional manual work. This is because since the entire Meraki AutoVPN mesh shares a single ASN, vMXs in opposite regions will reject routes from the other region’s branches, as the originating ASN is its own.

Another potential approach to allow cross-regional spoke connectivity with hub-to-hub tunnels disabled would be to simply add the hubs of the opposing region as additional hubs for the spokes in the local region. However, two side effects of this will be:

1. Each hub vMX will advertise a larger amount of prefixes to Cloud WAN, and you could end up exceeding the 1,000 route limit per connect peer in this way
2. Cross-regional spoke communications will bypass the Cloud WAN backbone and use the public Internet instead, which doesn't reap the same performance benefits

2. This means you need to inject a manual route from Cloud WAN into each region to be able to reach the opposite region.

1. 
    Navigate to Core network → Policy versions, and edit your latest active policy.
2. 
    Navigate to Segment actions, and under Routes select Create
3. 
    Specify the SDWAN segment and add a summary route or multiple summary routes for the branch prefixes in one of the regions, pointing to that region’s VPC attachment. For example, in this case to reach the 172.25.0.0/16 prefix associated with the West Branches, we would specify the next hop attachment as the us-west-1 VPC attachment, and for the 172.26.0.0/16 prefix associated with the East Branches, we would specify the next hop attachment as the us-east-1 VPC attachment.
4. 
    Click create segment route and create policy, and activate the policy.

### Step 13) Testing Cross-Regional connectivity

1. 
    Navigate to your Meraki Dashboard, and you should now see the other regions’ MX routes in the branch MX route tables.
2. 
    Start a ping from a branch in one region and ping a branch in the opposite region. This should now succeed.
