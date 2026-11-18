- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
07-13-2009 07:50 PM - edited 03-06-2019 06:45 AM
Why would someone run multiple OSPF processes on the same router?
What are the implications?
Are there two SEPARATE databases that are established, one for each process?
Are these processes totally isolated?
What about the RIB? Are there effectively separate routing tables?
lets say a route is learned through both OSPF processes, what happens then....?
Another quick question:
router ospf 499
router-id 10.199.254.38
log-adjacency-changes
auto-cost reference-bandwidth 1000000
nsf
area 1 authentication message-digest
redistribute static metric-type 1 subnets route-map into-ospf
network 10.195.48.40 0.0.0.7 area 1
network 10.195.48.48 0.0.0.15 area 1
network 10.199.254.38 0.0.0.0 area 1
network 99.207.255.8 0.0.0.0 area 1
!
router ospf 599
log-adjacency-changes
auto-cost reference-bandwidth 1000000
nsf
area 0 authentication message-digest
area 2 authentication message-digest
redistribute static
passive-interface default
no passive-interface GigabitEthernet4/1
no passive-interface Vlan98
no passive-interface Vlan99
network 10.195.48.8 0.0.0.3 area 2
network 10.195.48.32 0.0.0.7 area 2
network 10.195.48.248 0.0.0.7 area 0
network 10.195.49.0 0.0.0.127 area 2
network 10.195.50.0 0.0.0.127 area 2
network 10.195.64.0 0.0.15.255 area 2
network 10.195.80.0 0.0.0.255 area 2
<b>interface GigabitEthernet4/1
ip address 10.195.48.53 255.255.255.240</b>
Notice the IP address of this interface. OSPF 499 is running OSPF on this interface (see the network statement), yet the interface is un-passived out in OSPF599....why is that? Typos?
Thanks
Solved! Go to Solution.
- Labels:
- 
						
							
		
			Other Switching
Accepted Solutions
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
07-16-2009 03:51 AM
Joe
You are pretty close. Let me try to clarify one of the things that you say:"it is not the fact that the network statement under OSPF encompasses the whole range of subnets that determines which LSAs are created and sent, but the actual interfaces that are configured on each router."
It is not just the network statement and it is not just the interfaces on the router that are configured. It is the combination of the network statement and the configured interfaces that determine what is advertised by OSPF.
To explain in a bit more detail:
When the OSPF process starts it looks at its configured network statements (and the address ranges defined by the address and the mask) and it looks at every interface on the router (that is in up/up state) and if an interface falls into the range defined by a network statement then that interface is included into OSPF. Then OSPF looks at the subnet defined on that interface (including its mask) and advertises that subnet.
So to clarify a couple of points:
- the network statement does not tell OSPF what to advertise but tells OSPF what interfaces to process.
- the network statement does not tell OSPF to summarize (there are separate commands to control summarization).
- OSPF will determine what to advertise based on the configured subnets on the interfaces that it includes in its processing.
HTH
Rick
Rick
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
07-13-2009 09:42 PM
Why would someone run multiple OSPF processes on the same router? --you can do it but to able to achieve routing between them you have to use the redistribution.
Yes they have their own separate databases.
What about the RIB? Are there effectively separate routing tables? -yes
lets say a route is learned through both OSPF processes, what happens then....?
it will only happen when you redistribute.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
07-14-2009 03:21 AM
Joe
I would answer a couple of things a bit differently from Manish. There are several reasons why you might choose to run separate OSPF processes. Perhaps your router connects to customer A and you want to run OSPF with customer A so that you can support them. And perhaps your router also connects to customer B. And you want to run OSPF with customer B. But you do not want customer A to see customer B routes. The easy solution for this is to run an OSPF process on the interface connecting to customer A and to run a separate OSPF process on the interface connecting to customer B. In this case your router knows routes of both customers but will not advertise routes from customer B to customer A.
Or perhaps there is a situation where your router will learn certain routes in OSPF but you do not want to advertise some of those routes to other parts of your network. The easy way to accomplish this is to run separate OSPF processes, to redistribute routes between processes, and to filter the redistribution to allow only some routes to be redistributed and advertised by the other process.
The implications of running separate OSPF processes include these:
- an interface can be active in only 1 interface. So each OSPF process will have a unique set of neighbors.
- each OSPF process will learn its own prefixes and maintain those prefixes in its own database.
- each OSPF process will advertise only prefixes from its own data base. The only way to have one process advertise prefixes from the other process is to redistribute.
Yes the separate OSPF processes have separate OSPF data bases and the separate OSPF processes do not share any data with the other OSPF process unless you redistribute.
No there are not separate RIBs. There is a single routing table for the router which will contain routes from both OSPF processes. But each OSPF process will advertise only the routes that are contained in its own database.
If you run 2 OSPF processes and if each process learns the same prefix then each OSPF process will attempt to insert its route into the routing table. If both prefixes have the same metric them both routes will show up in the routing table (and the router will load share traffic to that destination). Note that since both processes can not run on the same interface, the 2 routes that it learns will have different next hop addresses - it is not possible for both OSPF processes to learn the same prefix with the same next hop.
OSPF process 599 may have a no passive-interface command, but if there is not a network statement that matches the address then it will not cause the interface to be included in the process. Having the no passive-interface command under OSPF 599 does no harm, but it does no good either. Perhaps at one time process 599 did include that interface and then it was moved to process 499 but someone forgot to remove the no passive-interface. Note that if both processes did have network statements that match the interface address then the interface can be active in one process but not the other.
HTH
Rick
Rick
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
07-14-2009 04:41 AM
Rick:
Thank you VERY much for that very informative and extremey helpful answer.
Much of what you said is precisely what I expected, but I couldnt find some good documentation on the subject to verify my thoughts. I know plenty must exist, but I couldnt find anyhing on the Internet and it was getting late....
I think I have all the information I need, but I MAY come back for a bit more.
Thanks again, really appreciate it....
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
07-14-2009 05:26 AM
Rick:
I have another question. That was fast, huh?
Let's please go back to that router with the OSPF 599 process and ignore 499 for a moment.
There are actually 2 routers running the same OSPF process and "advertising" the same summarized addresses.
The reason is that each router has about 120 /29 vlans in the SAME network ranges that you see under the OSPF process, but one router hosts odd-numbered vlans while the other hosts even-numbered vlans.
Example:
10.195.80.241/29 - VLAN 537 - ODD
10.195.80.249/29 - VLAN 410 - EVEN
Router 1:
router ospf 599
log-adjacency-changes
auto-cost reference-bandwidth 1000000
nsf
area 0 authentication message-digest
area 2 authentication message-digest
redistribute static
passive-interface default
no passive-interface GigabitEthernet4/1
no passive-interface Vlan98
no passive-interface Vlan99
network 10.195.48.8 0.0.0.3 area 2
network 10.195.48.32 0.0.0.7 area 2
network 10.195.48.248 0.0.0.7 area 0
network 10.195.49.0 0.0.0.127 area 2
network 10.195.50.0 0.0.0.127 area 2
network 10.195.64.0 0.0.15.255 area 2
network 10.195.80.0 0.0.0.255 area 2 <---- SAME network range
ROUTER 2:
router ospf 599
log-adjacency-changes
auto-cost reference-bandwidth 1000000
nsf
area 0 authentication message-digest
area 2 authentication message-digest
redistribute static
passive-interface default
no passive-interface GigabitEthernet4/1
no passive-interface Vlan98
no passive-interface Vlan99
network 10.195.48.0 0.0.0.3 area 2
network 10.195.48.32 0.0.0.7 area 2
network 10.195.48.248 0.0.0.7 area 0
network 10.195.49.0 0.0.0.127 area 2
network 10.195.50.0 0.0.0.127 area 2
network 10.195.64.0 0.0.15.255 area 2
network 10.195.80.0 0.0.0.255 area 2 <---- SAME network range
There is NO HSRP between the two.
There is also a routed connection between the two routers (vlan 99). See config.
These routers are also uplinked to 2 other core routers, which we could probably leave out of the dicussion for now.
OK, so the Question:
If I were in router 1 and did a "sh ip ro" for one of the /29 subnets hanging off of router 2, what would I see? Would it be a directly connected route, since it falls within the range of the supernet under the OSPF 599 process of router 1? Or would it see a specific route pointing to router 2 for the specific subnet?
Does the answer to this question have to do with whether the routers are creating and sending out specific LSAs for each /29 subnet or if they are creating and sending out LSAs for the summarized (supernet) addresses only?
If the routers are sending specific LSAs for each /29 subnet, then router 1's routing table should point to router 2 as the next-hop for that subnet hanging off of router 2. However, if only an LSA is generated for each supernet, then router 1 will see the route to the /29 hanging off of r2 as a "directly connected" route."
My guess is that the routers are indeed creating specific LSAs for EACH /29 subnet because there is no summary route configured under the OSPF 599 process. If there were, then there would be only ONE LSA for the entire summarized address range.
Is the above analysis and assumptions correct?
Unfortunately, I do NOT have access to the routers, or I would have done all the discovery myself. We have a situation where a remote engineer has access and me and my engineer have to walk him through some routing issues.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
07-14-2009 07:27 AM
Joe
There are some things about the topology of the network and what is going on that I do not yet understand very well. But I believe that enough is clear to answer your main question. If we need to go deeper then you may need to clarify some things.
Each router should see individual routes for each subnet. For the VLAN/subnets to which it is connected they will appear as connected routes. For the subnets for which it is not directly connected the route should be intra area routes and should have the other router as the next hop. This would be true even if the routers were configured to generate summary routes - and they are not configured to generate summaries.
The important concept here is that the VLAN/subnets are all in area 2. And within an area all OSPF routers will see all of the detail for all the prefixes within the area.
Let me also clarify that just because some subnet falls into a range used in a network statement on a router, it does not mean that the subnet would show as connected. The only "connected" routes are the ones to which your router is actually physically connected.
Let me also clarify that just because the OSPF configuration has a range in the network statement, that does not mean that OSPF will generate a summary for the range. The range in the network statement is only to simplify the process of determining which interfaces get included within the routing process. If you want summary routes in OSPF then you must specifically configure it to generate summary routes.
HTH
Rick
Rick
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
07-15-2009 08:25 PM
Rick:
I understand much of what you have said...
So, just to summarize, each router will create and send an LSA for EACH routed interface configured on it that participtes in OSPF. Correct?
So, each of these routers has about 125 SVIs (/29 subnets) configured on them, so each router should generate an LSA for each of those /29 subnets. Correct?
So, this is why, router 1 will point to router 2 as the next hop for a subnet configured on router 2, and NOT view it as a directly connected route - because it is not the fact that the network statement under OSPF encompasses the whole range of subnets that determines which LSAs are created and sent, but the actual interfaces that are configured on each router. Correct?
Thanks and sorry for taking so long to get ack to you. Im getting slammmed with 14 hour days this new client.
Thanks
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
07-16-2009 03:51 AM
Joe
You are pretty close. Let me try to clarify one of the things that you say:"it is not the fact that the network statement under OSPF encompasses the whole range of subnets that determines which LSAs are created and sent, but the actual interfaces that are configured on each router."
It is not just the network statement and it is not just the interfaces on the router that are configured. It is the combination of the network statement and the configured interfaces that determine what is advertised by OSPF.
To explain in a bit more detail:
When the OSPF process starts it looks at its configured network statements (and the address ranges defined by the address and the mask) and it looks at every interface on the router (that is in up/up state) and if an interface falls into the range defined by a network statement then that interface is included into OSPF. Then OSPF looks at the subnet defined on that interface (including its mask) and advertises that subnet.
So to clarify a couple of points:
- the network statement does not tell OSPF what to advertise but tells OSPF what interfaces to process.
- the network statement does not tell OSPF to summarize (there are separate commands to control summarization).
- OSPF will determine what to advertise based on the configured subnets on the interfaces that it includes in its processing.
HTH
Rick
Rick
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
07-16-2009 09:28 AM
Thanks, Rick..excellent exlanation...got it.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
11-05-2018 10:47 PM
Hi I have an scenario using two ABR routers where there two OSPF Process ID using Area 0? How can connect the Process ID that is part of the Aggregation ring? Since I'm facing some issues when the link goes down in the AGG.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
11-06-2018 12:46 PM
I have looked at the diagram that you posted but am still confused about what is going on. I see ABR1 and ABR2 And a router between them. What is that middle router, is it in both OSPF processes?
I see a dashed blue line that seems to indicate what is the area 0 for OSPF10 It shows a direct connection between the ABRs and connection through the core nodes. And I see a dashed green line but am not sure about it and what it means. It would seem to indicate the area 0 for OSPF 100. But both lines run through the agg routers and there does not seem to be any direct connection for these routers in OSPF 100. Can you provide clarification?
HTH
Rick
Rick
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
11-13-2018 09:55 AM
Hi the router between RR participate in OSPF10, is not part of OSPF100. The OSPF100 participate between AGG and RR nodes. I understand the best solution is create a link between RR and be part of OSPF 100, sometimes is not posible in the real scenario. Some issues are present if there a link donw in the AGG nodes, to reduce this I redistribute the OSPF into BGP. I'm using MP BGP, but when I show the routes The AGG nodes learn by OSPF the Lo0 of RR, but in the RR the Lo0 is learned via BGP (If I generate a link down between AGG ABR04 and ABR2 RR inline)
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
09-21-2013 05:01 PM
Richard:
You wrote (I changed the hyphens to a, b, c):
The implications of running separate OSPF processes include these:
a) an interface can be active in only 1 interface. So each OSPF process will have a unique set of neighbors.
b) each OSPF process will learn its own prefixes and maintain those prefixes in its own database.
c) each OSPF process will advertise only prefixes from its own data base. The only way to have one process advertise prefixes from the other process is to redistribute.
In a), did you mean "An interface can be active in only one process."? Or, "A process can be active on only one interface."? I think you may have duplicated the word interface unwittingly. Please help me underestand what you meant to say. Thanks.
Great explanation, nonetheless.
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
09-21-2013 08:46 PM
Kerry
Thank you for your attention to detail in reading an old post. You are absolutely correct that I did unwittingly duplicate the word interface. What I intended to say is that an interface can be active in only 1 process. And to expand that thought I would say that while several OSPF processes may have network statements that include a particular interface that the interface can be active in only one process at a time.
HTH
Rick
Rick
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
11-27-2013 07:54 AM
What about area-0 when there are two processes declared? Can you have two different area-0 in the same router (one per process)?