Inter-VDOM routing configuration example: Partial-mesh VDOMs
This example shows how to configure a FortiGate unit to use inter-VDOM routing to route traffic between an internal network and FTP server that are each behind separate VDOMs. See Inter-VDOM routing for more information.
The following example shows how to configure per-VDOM settings, such as operation mode, routing, and firewall policies, in a network that includes the following VDOMs:
- 
                                                    VDOM-A: allows the internal network to access the Internet.
- 
                                                    VDOM-B: allows external connections to an FTP server.
- 
                                                    root: the management VDOM.
You can use VDOMs in either NAT or transparent mode on the same FortiGate. By default, VDOMs operate in NAT mode. In this example, both VDOM-A and VDOM-B use NAT mode. An inter-VDOM link is created and inter-VDOM routes configured to allow users on the internal network to access the FTP server.
This is an example of the partial-mesh VDOMs configuration since only VDOM-A is connected to VDOM-B but neither of those VDOMs are connected to the root VDOM. See Topologies for details.
This example assumes that the interfaces of the FortiGate have already been configured with the IP addresses depicted in the preceding diagram.
General steps for this example
This configuration requires the following general steps:
This example demonstrates how to configure these steps first using the GUI and then, at the end of the section, using the CLI. See Configuration with the CLI for details.
Enable Multi-VDOM mode and create the VDOMs
Multi-VDOM mode can be enabled in the GUI or CLI. Enabling it does not require a reboot, but does log you out of the device. The current configuration is assigned to the root VDOM.
|  | On FortiGate 90 series models and lower, VDOMs can only be enabled using the CLI. | 
To enable multi-VDOM mode in the GUI:
- 
                                                    On the FortiGate, go to System > Settings.
- 
                                                    In the System Operation Settings section, enable Virtual Domains.
- 
                                                    Click OK.
To create the VDOMs in the GUI:
- 
                                                    In the Global VDOM, go to System > VDOM. Click Create New.
- 
                                                    In the Virtual Domain field, enter VDOM-A.
- 
                                                    If required, set the NGFW Mode. If the NGFW Mode is Profile-based, Central SNAT can be enabled.
- 
                                                    Click OK to create the VDOM.
- 
                                                    Repeat the above steps for VDOM-B.
Assign interfaces to VDOMs
This example uses three interfaces on the FortiGate unit: port1 (internal network), port2 (FTP server), wan1 (WAN link for VDOM-A), and wan2 (WAN link for VDOM-B). The port1 and port2 interfaces are connected to the internal network and FTP server, respectively. The wan1 and wan2 interfaces are static assigned with IP addresses and default gateways provided by the ISPs for those WAN links.
To assign interfaces to VDOMs in the GUI:
- 
                                                    In the Global VDOM, go to Network > Interfaces.
- 
                                                    Select port1 and click Edit.
- 
                                                    From the Virtual domain list, select VDOM-A.
- 
                                                    Click OK.
- 
                                                    Repeat the preceding steps to assign port2 to VDOM-B.
- 
                                                    Repeat the preceding steps to assign wan1 to VDOM-A.
- 
                                                    Repeat the preceding steps to assign wan2 to VDOM-B.
Configure VDOM-A
VDOM-A allows connections from devices on the internal network to the Internet. WAN1 and port1 are assigned to this VDOM.
The per-VDOM configuration for VDOM-A includes the following:
- 
                                                    A firewall address for the internal network
- 
                                                    A static route to the ISP gateway
- 
                                                    A firewall policy allowing the internal network to access the Internet
All procedures in this section require you to connect to VDOM-A, either using a global or per-VDOM administrator account.
To add the firewall addresses in the GUI:
- 
                                                    Go to Policy & Objects > Addresses and select Address.
- 
                                                    Click Create new.
- 
                                                    Enter the following information: Name internal-network Type Subnet IP/Netmask 192.168.10.0/255.255.255.0 Interface port1
- 
                                                    Click OK.
To add a default route in the GUI:
- 
                                                    Go to Network > Static Routes and create a new route.
- 
                                                    Enter the following information: Destination Subnet IP address 0.0.0.0/0.0.0.0 Gateway 172.20.201.254 Interface wan1 Administrative Distance 10
- 
                                                    Click OK.
To add the firewall policy in the GUI:
- 
                                                    Go to Policy & Objects > Firewall Policy.
- 
                                                    Click Create New.
- 
                                                    Enter the following information: Name VDOM-A-Internet Incoming Interface port1 Outgoing Interface wan1 Source internal-network Destination all Schedule always Service ALL Action ACCEPT NAT enabled
- 
                                                    Click OK.
Configure VDOM-B
VDOM-B allows external connections to reach an internal FTP server. WAN2 and port2 are assigned to this VDOM.
The per-VDOM configuration for VDOM-B includes the following:
- 
                                                    A firewall address for the FTP server
- 
                                                    A virtual IP address for the FTP server
- 
                                                    A static route to the ISP gateway
- 
                                                    A firewall policy allowing external traffic to reach the FTP server
The procedures described above require you to connect to VDOM-B, either using a global or per-VDOM administrator account.
To add the firewall addresses in the GUI:
- 
                                                    Go to Policy & Objects > Addresses and select Address.
- 
                                                    Click Create new.
- 
                                                    Enter the following information: Name FTP-server Type Subnet IP/Netmask 192.168.20.10/255.255.255.255 Interface port2
- 
                                                    Click OK.
To add the virtual IP address in the GUI:
- 
                                                    Go to Policy & Objects > Virtual IPs and navigate to the Virtual IP tab.
- 
                                                    Click Create new.
- 
                                                    Enter the following information: Name FTP-server-VIP Interface wan2 External IP address/range 172.20.10.2 Map To 192.168.20.10
- 
                                                    Click OK.
To add a default route in the GUI:
- 
                                                    Go to Network > Static Routes and create a new route.
- 
                                                    Enter the following information: Destination Subnet IP address 0.0.0.0/0.0.0.0 Gateway 172.20.201.254 Interface wan2 Administrative Distance 10
- 
                                                    Click OK.
To add the firewall policy in the GUI:
- 
                                                    Go to Policy & Objects > Firewall Policy.
- 
                                                    Click Create New.
- 
                                                    Enter the following information: Name Access-server Incoming Interface wan2 Outgoing Interface port2 Source all Destination FTP-server-VIP Schedule always Service FTP Action ACCEPT NAT enabled
- 
                                                    Click OK.
Configure the VDOM link
The VDOM link allows connections from VDOM-A to VDOM-B. The VDOM link interface configured in this step will be used for inter-VDOM routing.
This step requires you to connect to the global VDOM using a global administrator account.
To add the VDOM link in the GUI:
- 
                                                    In the Global VDOM, go to Network > Interfaces.
- 
                                                    Create New > VDOM link.
- 
                                                    Enter the following information: Name VDOM-link Interface 0 Virtual Domain VDOM-A IP/Netmask 11.11.11.1/255.255.255.252 Interface 1 Virtual Domain VDOM-B IP/Netmask 11.11.11.2/255.255.255.252
- 
                                                    Click OK.
Configure inter-VDOM routing
Inter-VDOM routing allows users on the internal network to route traffic to the FTP server through the FortiGate.
The configuration of inter-VDOM routing includes the following:
- 
                                                    Firewall addresses for the FTP server on VDOM-A and for the internal network on VDOM-B
- 
                                                    Inter-VDOM routing using static routes for the FTP server on VDOM-A and for the internal network on VDOM-B
- 
                                                    Policies allowing traffic using the VDOM link
The procedures described above require you to connect to both VDOM-A and VDOM-B, either using a global or per-VDOM administrator account.
To add the firewall address on VDOM-A in the GUI:
- 
                                                    In the VDOM-A VDOM, go to Policy & Objects > Addresses and select Address.
- 
                                                    Click Create new.
- 
                                                    Enter the following information: Name FTP-server Type Subnet IP/Netmask 192.168.20.10/32 Interface VDOM-link2 Static route configuration enabled
- 
                                                    Click OK.
To add the static route on VDOM-A in the GUI:
- 
                                                    Connect to VDOM-A.
- 
                                                    Go to Network > Static Routes and create a new route.
- 
                                                    Enter the following information: Destination Named Address Named Address FTP-server Gateway 11.11.11.2 Interface VDOM-link0
- 
                                                    Click OK.
To add the firewall address on VDOM-B in the GUI:
- 
                                                    In the VDOM-B VDOM, go to Policy & Objects > Addresses and select Address.
- 
                                                    Click Create new.
- 
                                                    Enter the following information: Name internal-network Type Subnet IP/Netmask 192.168.10.0/24 Interface VDOM-link1 Static route configuration enabled
- 
                                                    Click OK.
To add the static route on VDOM-B in the GUI:
- 
                                                    In the VDOM-B VDOM, go to Network > Static Routes and create a new route.
- 
                                                    Enter the following information: Destination Named Address Named Address internal-network Gateway 11.11.11.1 Interface VDOM-link1
- 
                                                    Click OK.
Configure firewall policies using the VDOM link
Firewall policies using the VDOM link allows users on the internal network to access the FTP server through the FortiGate.
Configuring policies allowing traffic using the VDOM link require you to connect to both VDOM-A and VDOM-B, respectively, either using a global or per-VDOM administrator account.
To add the firewall policy on VDOM-A in the GUI:
- 
                                                    In the VDOM-A VDOM, go to Policy & Objects > Firewall Policy.
- 
                                                    Click Create New.
- 
                                                    Enter the following information: Name Access-FTP-server Incoming Interface port1 Outgoing Interface VDOM-link0 Source internal-network Destination FTP-server Schedule always Service FTP Action ACCEPT NAT disabled
- 
                                                    Click OK.
To add the firewall policy on VDOM-B in the GUI:
- 
                                                    In the VDOM-B VDOM, go to Policy & Objects > Firewall Policy.
- 
                                                    Click Create New.
- 
                                                    Enter the following information: Name Internal-server-access Incoming Interface VDOM-link1 Outgoing Interface port2 Source internal-network Destination FTP-server Schedule always Service FTP Action ACCEPT NAT disabled
- 
                                                    Click OK.
Configuration with the CLI
The example can also be configured in the CLI.
To configure the two VDOMs:
- 
                                                    Enable multi-VDOM mode: config system global
    set vdom-mode multi-vdom
endYou will be logged out of the device when VDOM mode is enabled.
- 
                                                    Create the VDOMs: config vdom
    edit VDOM-A
    next
    edit VDOM-B
    next
end
- 
                                                    Assign interfaces to the VDOMs: config global
    config system interface
        edit port1
            set vdom VDOM-A
        next
        edit port2
            set vdom VDOM-B
        next
        edit wan1
            set vdom VDOM-A
        next
        edit wan2
            set vdom VDOM-B
        next
    end
end
- 
                                                    Add the firewall addresses to VDOM-A: config vdom
    edit VDOM-A
        config firewall address
            edit internal-network
                set associated-interface port1
                set subnet 192.168.10.0 255.255.255.0
            next
        end
    next
end
- 
                                                    Add a default route to VDOM-A: config vdom
    edit VDOM-A
        config router static
            edit 0
                set gateway 172.20.201.254
                set device wan1
            next
        end
    next
end
- 
                                                    Add the firewall policy to VDOM-A: config vdom
    edit VDOM-A
        config firewall policy
            edit 1
                set name "VDOM-A-Internet"
                set srcintf "port1"
                set dstintf "wan1"
                set srcaddr "internal-network"
                set dstaddr "all"
                set action accept
                set schedule "always"
                set service "ALL"
                set nat enable
            next
        end
    next
end
- 
                                                    Add the firewall addresses to VDOM-B: config vdom
    edit VDOM-B
        config firewall address
            edit FTP-server
                set associated-interface port2
                set subnet 192.168.20.10 255.255.255.255
            next
        end
    next
end
- 
                                                    Add the virtual IP address to VDOM-B: config vdom
    edit VDOM-B
        config firewall vip 
            edit FTP-server-VIP
                set extip 172.20.10.2
                set extintf wan2
                set mappedip 192.168.20.10
            next
        end 
    next
end
- 
                                                    Add a default route to VDOM-B: config vdom
    edit VDOM-B
        config router static
            edit 0
                set gateway 172.20.10.254
                set device wan2
            next
        end
    next
end
- 
                                                    Add the firewall policy to VDOM-B: config vdom
    edit VDOM-B
        config firewall policy
            edit 1
                set name "Access-server"
                set srcintf "wan2"
                set dstintf "port2"
                set srcaddr "all"
                set dstaddr "FTP-server-VIP"
                set action accept
                set schedule "always"
                set service "FTP"
                set nat enable
            next
        end
    next
end
To configure the VDOM link:
- 
                                                    Configure the VDOM link: config global
    config system vdom-link
        edit "VDOM-link"
        next
    end    
    config system interface
        edit VDOM-link0
            set vdom VDOM-A
            set ip 11.11.11.1 255.255.255.252
            set allowaccess https ping ssh
            set description "VDOM-A side of the VDOM link"
        next
        edit VDOM-link1
            set vdom VDOM-B
            set ip 11.11.11.2 255.255.255.252
            set allowaccess https ping ssh
            set description "VDOM-A side of the VDOM link"
        next
    end
end
- 
                                                    Configure the firewall addresses on VDOM-A: config vdom
    edit VDOM-A
        config firewall address
            edit "FTP-server"
                set associated-interface "VDOM-link0"
                set allow-routing enable
                set subnet 192.168.20.10 255.255.255.255
            next
        end
    next
end
- 
                                                    Add the firewall policy to VDOM-B: config vdom
    edit VDOM-B
        config firewall policy
            edit 1
                set name "Access-server"
                set srcintf "wan2"
                set dstintf "port2"
                set srcaddr "all"
                set dstaddr "FTP-server-VIP"
                set action accept
                set schedule "always"
                set service "FTP"
                set nat enable
            next
        end
    next
end
- 
                                                    Add the static route on VDOM-A: config vdom
    edit VDOM-A
        config router static 
            edit 0
                set device VDOM-link0
                set dstaddr FTP-server
                set gateway 11.11.11.2 
            next
        end
    next
end
- 
                                                    Configure the firewall addresses on VDOM-B: config vdom
    edit VDOM-B
        config firewall address
            edit internal-network
                set associated-interface VDOM-link1
                set allow-routing enable
                set subnet 192.168.10.0 255.255.255.0
            next
        end
    next
end
- 
                                                    Add the static route on VDOM-B: config vdom
    edit VDOM-B
        config router static 
            edit 0
                set device VDOM-link1
                set dstaddr internal-network
                set gateway 11.11.11.1
            next
        end
    next
end
- 
                                                    Add the security policy on VDOM-A: config vdom
    edit VDOM-A
        config firewall policy 
            edit 0
                set name Access-FTP-server
                set srcintf port1
                set dstintf VDOM-link0
                set srcaddr internal-network
                set dstaddr FTP-server
                set action accept
                set schedule always
                set service FTP
            next
        end
    next
end
- 
                                                    Add the firewall policy on VDOM-B: config vdom
    edit VDOM-B
        config firewall policy 
            edit 0
                set name Internal-server-access
                set srcintf VDOM-link1
                set dstintf port2
                set srcaddr internal-network
                set dstaddr FTP-server
                set action accept
                set schedule always
                set service FTP
            next
        end
    next
end