This is a simple script for a simple VLAN configuration for WAN/LAN/DMZ. Ports are normally acting as hybrid ports, where LAN traffic is untagged and DMZ and WAN traffic is tagged, which is a good match for running VMs that might need to be reachable from the outside, which might even have public addresses. Port assignment is managed solely through “Interface List” and the membership of ports in each list. The first port “ether1” is assigned to WAN, that would be your internet access port. DHCP clients and servers can be assigned to the VLAN interfaces WAN, LAN and DMZ as required.

The same script can be run on all devices further downstream, and will carry the same VLANs if connected through hybrid ports. Please note that CRS1xx/CRS2xx devices, and certain others prefer a different method of configuration to achieve full wire speed.

```
/interface bridge
add ingress-filtering=yes name=bridge vlan-filtering=yes
/interface vlan
add comment="1 LAN" interface=bridge name=lan vlan-id=1
add comment="10 WAN" interface=bridge name=wan vlan-id=10
add comment="20 DMZ" interface=bridge name=dmz vlan-id=20
/interface list
add name=interfaces-all
add name=interfaces-lan
add name=interfaces-wan
add name=interfaces-dmz
/interface bridge port
add bridge=bridge interface=interfaces-all
add bridge=bridge frame-types=admit-only-untagged-and-priority-tagged interface=interfaces-lan
add bridge=bridge frame-types=admit-only-untagged-and-priority-tagged interface=interfaces-wan pvid=10
add bridge=bridge frame-types=admit-only-untagged-and-priority-tagged interface=interfaces-dmz pvid=20
/interface bridge vlan
add bridge=bridge comment="1 LAN" untagged=interfaces-lan,interfaces-all vlan-ids=1
add bridge=bridge comment="10 WAN" tagged=interfaces-all untagged=interfaces-wan vlan-ids=10
add bridge=bridge comment="20 DMZ" tagged=interfaces-all untagged=interfaces-dmz vlan-ids=20
/interface list member
add interface=ether1 list=interfaces-wan
add interface=ether2 list=interfaces-all
add interface=ether3 list=interfaces-all
add interface=ether4 list=interfaces-all
add interface=ether5 list=interfaces-all
add interface=ether6 list=interfaces-all
add interface=ether7 list=interfaces-all
add interface=ether8 list=interfaces-all
add interface=ether9 list=interfaces-all
```

             
            
           
          
            
            
              I believe I will give this a shot in my lab.

             
            
           
          
            
            
              
It’s mostly a template, so adding VLANs like CCTV, IOT, GUEST etc. is easy. But I found that if you don’t start with a VLAN config outright, and one that is maintainable, you run into a dead end where your installation relies on certain things, and changing those means changing everything.

I also tested the configuration with various hypervisors, including KVM, Bhyve and Hyper-V, and it is very simple to selectively give your VMs access to certain or all VLANs.

             
            
           
          
            
            
              A good thing, is avoid using vlanid 1 when using vlan…

             
            
           
          
            
            
              
VLAN ID 1 is obviously used on purpose, vs. 10/20/30 for this example, and unless you have some input on what the pitfalls might be, it’s not really a useful comment.

The benefit of consistently using VLAN ID 1 is that it is the default untagged network for Mikrotik devices. Even with gross misconfiguration, you ever won’t lose connectivity.

             
            
           
          
            
            
              
So basically leave it in due to expected incompetence while also leaving security holes in a vlan setup.   Bad advice.

             
            
           
          
            
            
              
Unless you can pinpoint where the security issue actually is, this falls under FUD. Using 10/20/30 has no functional difference, and VLAN 1 on Mikrotik devices has well-defined behavior. The main point for this particular configuration is that it is transparent.

             
            
           
          
            
            
              
You’re right about well-defined behaviour. The problem is that it’s not apparent, default VLAN 1 config is not shown in exported config nor in most GUI screens (apart from those showing running config or statuses). Which IMO means implicit configuration … and personally I don’t like implicit configuration. In a way it’s transparent … in the sense it’s not visible.

Of course there are different ways of seeing something as “transparent” and your way seems to be different than mine, @anav’s and a few other experienced forum users’ … it doesn’t mean yours is wrong.

             
            
           
          
            
            
              
The goal was to have a configuration that is operationally identical to your traditional, VLAN-less configuration of 1 dedicated WAN port, and the rest being LAN ports on a bridge (which also happens to be the defconf that devices will ship with). With this configuration, you can seamlessly switch to VLANs without changing anything else, which is the first step in actually increasing security by implementing isolated VLANs.

I think this is mostly gut-feeling, since I still haven’t gotten a clear answer on where the security issue is supposed to lie.

             
            
           
          
            
            
              Just for illustration: two problems with your template:

/interface bridge

add ingress-filtering=no name=bridge vlan-filtering=yes

/interface vlan

add comment=“1 LAN” interface=bridge name=lan vlan-id=1


Implicit configuration has bridge *CPU-facing port* set with pvid=1. Which makes bridge untagged member of VLAN 1 … and yet you create VLAN interface for that VLAN. VLAN interfaces work with tagged frames on the “anchor side”, making your interface a NoOp in the best case.

And a security feature: setting property *ingress-filtering* to yes is IMO a very important thing on anything but access port because it prevents connected device from injecting frames into VLANs which are not configured on that particular port. And that includes *CPU-facing bridge port* to which the setting in quoted part only applies. BTW, setting this property to no is default setting.

And I could go on and on, I’m pretty sure there are other problems in your template. But I’m not interested in disecting it any further.

             
            
           
          
            
            
              
Yes, I tested whether an explicit VLAN interface would make any functional difference, and since it does not, I decided to have that explicitly added for configuring addresses and/or DHCP clients, instead of configuring it directly on the bridge. Since it is a “NoOp”, to which I mostly agree, besides being a more explicit way to immediately see to which VLAN an address was bound, since it says “lan” instead of “bridge”, I decided to have that “NoOp”. Can you point out a problem with that? To me it is convenience without any drawbacks.

Who and how is anyone injecting frames with foreign VLAN IDs causing a problem? Besides the fact that you can in fact just turn ingress-filtering on. Which I did in my setup, after verifying the configuration works, so that I somewhat agree with, and the template could be changed to reflect that security precaution. Although I am still puzzled what the security implication might be.

Yes, you can go on and on, without writing anything in particular, since it seemingly relies on gut-feelings, and/or “I wouldn’t do it like that/I’ve never done it like that”.

             
            
           
          
            
            
              
Oh, and I forgot, the NoOp VLAN interface gives you a traffic monitor that only includes LAN traffic, while the bridge shows all bridge traffic:


             
            
           
          
            
            
              
… if that traffic hits CPU-facing bridge port (either due to being CPU communicating with devices on same VLAN or if it’s broadcast traffic). Most of traffic between devices, connected to bridge, does not.

Another thing: if device supports bridge L2 offload to switch chip (seems your drvice doesn’t), most of happening will be hidden from bridge (the switch-like entity), including port and VLAN stats.

             
            
           
          
            
            
              
True, and in my setup, there is plenty of traffic that will never even see that particular router, but it is what it is. I still have a counter of what part of the total traffic of the bridge is non-DMZ/WAN-related. Traffic on a different leg of the network that just gets L2 switched is only ever recorded directly on the interface.

Again, the LAN VLAN interface is a NoOp, but there is also no reason against it. On the other side, you can just do:

```
/ip dhcp-server
add add-arp=yes address-pool=client-pool interface=lan lease-script=update-dhcp-dns lease-time=52w1d name=dhcp-lan-server
```

And that remains the same, no matter if there is a bridge called “lan” or a VLAN interface called “lan”. And without any context, it’s still clear what it is supposed to do. That’s also the basic idea behind putting interfaces into an interface-list and then setting tagged/untagged based on the list. It is far less convoluted than having to fight the brain teaser that is manually setting up tagged/untagged on

```
/interface bridge vlan
```

.

             
            
           
          
            
            
              Your setup probably works fine for you and I’m glad for it. But the problem is when it gets published as a general template for newcomers to grab and blindly apply. Because generally it has a few problems and those will bite a few of those users. And that’s the reason for it getting quite some negative feedback. Don’t take it personally, it’s just that everybody has different experiences and some will spot weaknesses and problems in the code where others won’t. So if you want to make the template as universal and safe as possible, listen to what people say and consider their suggestions to improve your code. As it is now it hardly surpasses some ignorant youtube guides (e.g. on how to “secure” MT router). And this is my subjective opinion, backed by a pinch of experience and knowledge on the matter.

             
            
           
          
            
            
              I’m not taking this negatively or even personally. I want to fix the real issues, but besides the ingress-filtering I haven’t seen any. I’ll be changing that issue accordingly. The rest remains “I wouldn’t do that” without any explanation. The NoOp-VLAN-interface has no issues - it is a matter of choice. Using VLAN ID 1 has no implications, at least none that anyone could point out, and it is again a convenience thing.

             
            
           
          
            
            
              As I already explained, VLAN ID 1 is used in implicit configuration which makes it non-obvious and even non-transparent. And that makes it insecure.

Having the NoOp VLAN interface again makes things a bit muddy, users who don’t understand how bridge and L2HW offload works might jump into wrong conclusions.

Having *ingress-filtering* disabled (default setting, you did it explicitly on a particular bridge port for some random reason you didn’t explain), together with default *frame-types=admit-all* setting, makes setup insecure, I explained it in one of my previous posts.

I’ll say it one more time before I leave this thread: if you consider other people’s objections as irrelevant based on the fact that you didn’t experience anything similar yet, then you’re making your code irrelevant (in best case) or you’re leading less knowkedgeable users into problems by providing substandard solution. And I’m not trying to make you change the code you’re running on your own device, I’m pretty sure you’ll be able to get yourself out of any trouble yourself. I’m trying to persuade you to make published code as bulletproof as possible (because in networking it’s not about ease of configuration, it’s about security).

             
            
           
          
            
            
              
Again no explanation besides, “I feel like that is insecure”?

It feels “muddy”? Talk about objective security analysis.

I assume export of the configuration and switching some stuff around made me eventually show the configuration that way. As you pointed out, the default value would be disabled, so wouldn’t even appear in an export. As I wrote, I fixed that in the template, especially since that is how I run it as well.

You never mentioned

```
frame-types=admit-all
```

, but if you’d care to elaborate?

I consider it irrelevant because you and others fail to point out any concrete consequence, not because of a lack of experience. Your arguments are “non-transparent” and “muddy”. This setup is already a great improvement over the VLAN tutorial articles posted by Mikrotik, since they all manually manage each interface’s untagged/tagged configuration, which is honestly a disgrace seeing ROS’ capabilities in consolidating configurations.

             
            
           
          
            
            
              
In practical testing, I found out that the “NoOp” VLAN interface is necessary for WiFi CAP to operate correctly.