My network diagram is as follows:
The printer on the 2-network responds to pings from the 3-wifi-network for sometime and then just drops out for some reason i can’t figure out.
I have policies on the fortigate firewall to traffic from 3.0 to 2.0 and also reverse with all ports open. I have a server on 2.250 which responds to pings perfectly but not this printer. The printer is on a static ip of 2.16.
I have also configured policy routing on the fortigate to force to and from traffic with source and destination addresses 2.0 and 3.0 and vice versa to force the traffic between those two.
My policy config and routers as follows:
config firewall policy
    edit 3
        set uuid 2e5c19ea-8776-51e5-f4a0-26880cb5c37b
        set srcintf "internal1"
        set dstintf "internal2"
        set srcaddr "all"
        set dstaddr "all"
        set action accept
        set schedule "always"
        set service "ALL"
        set logtraffic disable
    next
end
config firewall policy
    edit 4
        set uuid 2e75fa22-8776-51e5-a324-5bb10126bbdd
        set srcintf "internal2"
        set dstintf "internal1"
        set srcaddr "all"
        set dstaddr "all"
        set action accept
        set schedule "always"
        set service "ALL"
        set logtraffic disable
    next
end
And policy routing to force traffic:
config router policy
    edit 2
        set input-device "internal2"
        set src "192.168.3.0/255.255.255.0"
        set dst "192.168.2.0/255.255.255.0"
        set output-device "internal1"
    next
    edit 1
        set input-device "internal1"
        set src "192.168.2.0/255.255.255.0"
        set dst "192.168.3.0/255.255.255.0"
        set output-device "internal2"
    next
end
and the interfaces :
config system interface
    edit "internal1"
        set vdom "root"
        set ip 192.168.2.1 255.255.255.0
        set allowaccess ping https ssh
        set vlanforward enable
        set type physical
        set alias "LAN"
        set snmp-index 4
    next
end
config system interface
    edit "internal2"
        set vdom "root"
        set ip 192.168.3.1 255.255.255.0
        set allowaccess ping https ssh
        set vlanforward enable
        set type physical
        set alias "WIFI"
        set snmp-index 5
    next
end
Any help please.