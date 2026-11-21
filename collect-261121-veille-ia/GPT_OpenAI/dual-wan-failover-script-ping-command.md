---
id: collect-261121-veille-ia/reseau-networking/dual-wan-failover-script-ping-command
title: "Dual WAN Failover Script Ping Command"
domain: reseau-networking
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["gateway", "script", "find", "ping", "clear", "dhcp", "route", "connection"]
source: https://forum.mikrotik.com/t/dual-wan-failover-script-ping-command/150516?page=2
source_anchor: ""
source_lines: [1, 336]
section: "GPT_OpenAI"
---
No worries, the OP is happy with your solution, albeit the wrong choice,  just kidding.

             
            
           
          
            
            
              Okay I will bite, perhaps there is a better way to do what I wish.

Here is my dhcp script…

:if ($bound=1) do={

:local iface $interface

:local gw [ /ip dhcp-client get [ find interface=$“iface” ] gateway ]

/ip route set [ find comment=“PrimaryRecursive” gateway!=$gw ] gateway=$gw

/ip route set [ find comment=“SecondaryRecursive” gateway!=$gw ] gateway=$gw

/tool e-mail send to=“llamaworks@accesswave.ca” subject=([/system identity

get name]) body=" This is your new gateway IP: $gw";

:local sub3 ([/system clock get time])

/tool fetch “https://api.telegram.org/bot-----------:B---------------------------------T/sendMessage?chat_id=-1111111111&text=At+$sub3+BellFibre+Changed+WANIP”

:log info “Telegram notification sent  VlanBell IP Changed”

}

The idea here is that every time the WANIP is bounded, it will find the gateway now allocated and then place it in my routes…

This addresses the inability for the router to populate the routes otherwise, be it a WANIP change, a router reboot etc…

             
            
           
          
            
            
              how to make 3 ip gateway in one comment ? 2 IP pppoe and 1 ip static

Thanks

             
            
           
          
            
            
              The WAN fail over technique works properly if I clear connection tracking. Otherwise, the network appears to timeout. I tested with a long ping session to a remote host and a VPN session. Disabling the interface will automatically clear connection tracking and makes the fail over occur right away. So, does this mean scripting is still needed?

Note, I’m testing by disabling the remote hosts to trigger a failure mode.

             
            
           
          
            
            
              Clear connection-tracking is needed because remote address unreachable do not cause the clear of connection-tracking.

What access method you use?

For ppp user just put one script on on down /on up

For dhcp client like the same

For other metods can be finded a solution.

For example on ppp profile or dhcp client triggered only if ppp/dhcp connection go up/down:

```
/ip fire conn
:foreach idc in=[find where timeout>60] do={
 remove [find where .id=$idc]
}
```

If the LAN is NATted or other gateway are used, for example on ppp profile

```
:global newIP [:tostr $"local-address"]
/ip fire conn
:foreach idc in=[find where timeout>60 and (!(reply-dst-address~$newIP))] do={
 remove [find where .id=$idc]
}
```

             
            
           
          
            
            
              
Thank you **anav** and **rextended** for your examples and help on this subject. I'm testing in a lab using two simple MikroTik units. So, my connection method in the real world, which I am simulating here, is probably going to be an ISP bridge device of some type. Fiber to copper convertor or the like.

The R1 unit represents a router with failover. The ISP Simulator unit represents a device providing two WAN links (simulating two Internet providers in the building). On the ISP unit, I have two firewall rules that disable Host1 (8888) and Host2 (9999) to simulate network failure. R1 is checking ping access to these hosts. It works well, except that I must clear connection-tracking (on R1 which discovers the downed hosts).

If this is a requirement, I guess the question becomes, *how do I know when to fire a script to do this*? The WAN links are still up, the ether interfaces are still up. Is there a way to detect the Hosts are down and fire a script? The route command (*/ip route add check-gateway=ping distance=1 gateway=8.8.8.8*) does not have a script or event action. Likewise, for when they are back up. I realize I could program a Raspberry Pi in the rack to check for me, but was wondering if this can be done all inside the MikroTik.

**Router with Failover**

```
#
# software = RouterOS 6.47.10
# model    = RouterBOARD 952Ui-5ac2nD (hAP AC Lite)
###########################################################
# A router with WAN failover to two uplinks on 
# ether2 and ether3.
###########################################################
/system identity set name=R1
# The two ISP WAN connections
/ip dhcp-client
add add-default-route=no disabled=no interface=ether2 use-peer-dns=no comment="ISP1"
add add-default-route=no disabled=no interface=ether3 use-peer-dns=no comment="ISP2"
# Route failover by checking two hosts
/ip route
add check-gateway=ping distance=1 gateway=8.8.8.8                comment=Host1
add check-gateway=ping distance=2 gateway=9.9.9.9                comment=Host2
add distance=1 dst-address=8.8.8.8/32 gateway=10.3.30.1 scope=10 comment=ISP1_check
add distance=2 dst-address=9.9.9.9/32 gateway=10.3.30.1 scope=10 comment=ISP1_check
add distance=3 gateway=10.4.40.1                                 comment=ISP2
###########################################################
# Example LAN environment
###########################################################
/interface bridge add name=BR1 protocol-mode=none vlan-filtering=yes
/interface vlan add interface=BR1 name=VLAN1 vlan-id=44
/interface bridge port add bridge=BR1 interface=ether5 pvid=44
/interface bridge vlan add bridge=BR1 tagged=BR1 vlan-ids=44
/ip address add address=10.44.40.1/24 interface=VLAN1 network=10.44.40.0
/ip pool add name=POOL1 ranges=10.44.40.2-10.44.40.254
/ip dhcp-server add address-pool=POOL1 disabled=no interface=VLAN1 name=DHCP1
/ip dhcp-server network add address=10.44.40.0/24 dns-server=10.44.40.1 gateway=10.44.40.1
/interface list
add name=WAN
add name=VLAN
add name=BASE
/interface list member
add interface=ether2 list=WAN
add interface=ether3 list=WAN
add interface=VLAN1  list=VLAN
/ip dns set allow-remote-requests=yes servers=9.9.9.9,8.8.8.8
/ip firewall filter
add action=accept chain=input connection-state=established,related comment="Allow Estab & Related"
add action=accept chain=input in-interface-list=VLAN comment="Allow VLANs"
add action=drop chain=input comment=Drop
add action=accept chain=forward connection-state=established,related comment="Allow Estab & Related"
add action=accept chain=forward connection-state=new in-interface-list=VLAN out-interface-list=WAN comment="Allow VLANs"
add action=drop chain=forward comment=Drop
/ip firewall nat add action=masquerade chain=srcnat out-interface-list=WAN comment="Default masquerade"
```

**ISP Simulator**

```
#
# software = RouterOS 6.47.10
# model    = RouterBOARD 750UP r2 (hEX PoE Lite)
###########################################################
# Test router representing two ISP providers.
# ISP1 on ether3 and ISP2 on ether4
# See the two firewall rules that simulate network down.
###########################################################
/system identity set name=ISP_Provider
/interface ethernet
set [ find default-name=ether3 ] comment=ISP1
set [ find default-name=ether4 ] comment=ISP2
###########################################################
# Example environment provided by this Test ISP
###########################################################
/interface bridge
add fast-forward=no name=BR1 protocol-mode=none vlan-filtering=yes
/interface vlan
add interface=BR1 name=VLAN_ISP1 vlan-id=30
add interface=BR1 name=VLAN_ISP2 vlan-id=40
/interface list
add name=WAN
add name=VLAN
add name=BASE
/ip pool
add name=POOL1 ranges=10.3.30.2-10.3.30.254
add name=POOL2 ranges=10.4.40.2-10.4.40.254
/ip dhcp-server
add address-pool=POOL1 disabled=no interface=VLAN_ISP1 name=DHCP1
add address-pool=POOL2 disabled=no interface=VLAN_ISP2 name=DHCP2
/interface bridge port
add bridge=BR1 interface=ether3 pvid=30
add bridge=BR1 interface=ether4 pvid=40
/interface bridge vlan
add bridge=BR1 tagged=BR1 vlan-ids=30
add bridge=BR1 tagged=BR1 vlan-ids=40
/interface list member
add interface=ether1 list=WAN
add interface=VLAN_ISP1 list=VLAN
add interface=VLAN_ISP2 list=VLAN
/ip address
add address=10.3.30.1/24 interface=VLAN_ISP1 network=10.3.30.0
add address=10.4.40.1/24 interface=VLAN_ISP2 network=10.4.40.0
/ip dns set allow-remote-requests=yes
/ip dhcp-client add dhcp-options=hostname disabled=no interface=ether1
/ip dhcp-server network
add address=10.3.30.0/24 dns-server=10.3.30.1 gateway=10.3.30.1
add address=10.4.40.0/24 dns-server=10.4.40.1 gateway=10.4.40.1
/ip firewall filter
add action=accept chain=input connection-state=established,related comment="Allow Estab & Related"
add action=accept chain=input in-interface-list=VLAN comment="Allow VLANs"
add action=drop chain=input comment=Drop
###########################################################
# These two rules test causing failover to occur
###########################################################
add action=drop chain=forward disabled=yes dst-address=8.8.8.8 src-address=10.3.30.0/24 comment="Enable to test Host1 Failure"
add action=drop chain=forward disabled=yes dst-address=9.9.9.9 src-address=10.3.30.0/24 comment="Enable to test Host2 Failure"
add action=accept chain=forward connection-state=established,related comment="Allow Estab & Related"
add action=accept chain=forward connection-state=new in-interface-list=VLAN out-interface-list=WAN comment="Allow VLANs"
add action=drop chain=forward comment=Drop
/ip firewall nat add action=masquerade chain=srcnat comment="Default masquerade" out-interface-list=WAN
```

             
            
           
          
            
            
              
how do I know when to fire a script to do this?


When the failover is active… and when go back online the main line, and the failover is not active

```
:global something
:if ([:len [/ip route find where comment="ISP2" and active=yes]] > 0) do={
    :if ($something != true) do={
        /ip fire conn
        :foreach idc in=[find where timeout>60] do={ remove [find where .id=$idc] }
        :set something true
    }
} else={
    :if ($something != false) do={
        /ip fire conn
        :foreach idc in=[find where timeout>60] do={ remove [find where .id=$idc] }
        :set something false
    }
}
```

             
            
           
          
            
            
              rextended,

Oh my goodness. This is awesome! It works excellent. You should make a separate post about WAN Failover and update the link in your signature to point to that new dedicated topic. It takes a good while to write up topics, so no pressure. Just a grateful user.

Note, I changed the timeout to 0, vs 60. This is because ping sessions timeout at 10s by default. I suppose people could tune the value to their application behavior.

             
            
           
          
            
            
              NO, leave it to 60 seconds, or at least 20 seconds (check gateway 10 ping off, 10 ping on)

I do not write the things without reason.

If just one connection on connection tracking  is already closed for timeout (or other reasons) during the execution of the clean,

the script stop with error because when try the connection is already closed, and do not finish his works.

             
            
           
          
            
            
              
Okay, if that is the case, would it be possible to close connections in a sub function? Perhaps store in memory (whatever MikroTik scripting calls it) a list of all connections, then loop over that list closing them down? If the connection is no longer present, move on to the next item in the list? I need > 9 seconds of closing capability.

             
            
           
          
            
            
              
???

I do not insist further, I have already written you the script that does the right job,

based on the real traffic of equipment in production and not only theoretically simulated.

             
            
           
          
            
            
              
Its okay, I can sort it out. If you have a ping session, not stop (ping 8.8.8.8 -t), when the change over occurs it will hang because it is on a 9s timeout.

             
            
           
          
            
            
              pcunite, I do not understand what you are discussing regarding clearing connections.

Is this something I should be worried about on my setup???

Typically my issue is not failover perse but when the primary come back online,  the router was not able to route the traffic properly if the gateway had changed.

THus my script is to ensure that the new gateway is used for routing.

Where does your connection revelation belong in this??

             
            
           
          
            
            
              
Well, I don’t know. It comes down to how ISP1 fails and the applications you are using. After the failover to the different ISP, if your application times out on its own, you’ll be fine. However, if you have long running applications (VPN or a never ending ping session), those packet flows will not automatically change over if the interface is still up. At least in my testing they did not. So, I just go ahead and clear out any and all connections. This provides the same effect had you disabled the interface.

I think most people must test their failover by unplugging a cable or disabling an interface. In my case, I’m testing hosts that go down, and that is why I want the change over to occur. For all intents, the first ISP still works, in some ways. However, I want to fail over because the second ISP gives me access to those hosts. Changing the route out from under all the other connections leaves them in an unstable state. So, I close them out. Then the applications reconnect on their own.
