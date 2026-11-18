Huawei M-Lag and OSPF problem 
        
        
        
    
    
    How you all doing,
I have 2 spines connected in Active-backup M-Lag. The spines are connected to a Palo-Alto Firewall with 2 links: internal and external. The traffic goes from the campus network to the spine, and from the spine to the Firewall internal link. Then the firewall should return the traffic through the external link back to the spine.
The spine is connected to the Firewall with 2 different OSPF processes and 2 different VRFs.
The problem is that the OSPF is always going Full state on one spine, and is Init or ExStart on the other spine. The traffic drops because the firewall takes traffic from one spine and returns it to the other, where the OSPF is never up.
Any tips for why the OSPF is never in Full state on both spines or even any change in the M-lag configurations that would help.
Thanks in advance.
Section des commentaires
Are you sure this is a true spine-leaf setup? Spines shouldn’t be connected to each other (meaning no MLAG).
Can you share a simple drawing of the setup?
Sorry for the late reply but i forgot to add that the Spines are used as a border
MLAG is only layer 2. MLAG takes two switches and presents them as a single Layer 2 device.
The two switches are always two separate routers, though. You'll want non-MLAG links to the spines. The FWs should see two different routers.
So each spine must have different IP or could both spines have the same IP in the OSPF as in the FW should see Two different IPs for each spine or could the FW see both spines with the same IP
They can have the same IP but only as a first hop/next hop. Not as a routing peering session.
You can't peer with a shared IP. If you were to establish a BGP peering session or OSPF neighbor, it has to be with the individual device on a unique IP.
Spines shouldn’t have links between them. Firewalls should connect to a “boarder leaf” where you can do all your L2.
Sorry for the late reply but i forgot to add that the Spines are used as a border