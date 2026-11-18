Virtual VLAN switch
Virtual VLAN switch
The hardware switch ports on FortiGate models that support virtual VLAN switches can be used as a layer 2 switch. Virtual VLAN switch mode allows 802.1Q VLANs to be assigned to ports, and the configuration of one interface as a trunk port.
The following FortiGate series are supported in FortiOS 7.4: 40F, 60F, 70F, 80F, 90G, 100F, 120G, 140E, 200F, 300E, 400E, 400F, 600F, 900G, 1000F, 1100E, 1800F, 2600F, 3000F, 3200F, 3500F, 3700F, 4200F, 4400F, and 4800F. FortiWiFi 60F models are not supported.
The virtual-switch-vlan option must be enabled in the CLI to configure VLAN switch mode from the GUI or CLI.
To enable VLAN switches:
config system global
    set virtual-switch-vlan enable
end
                                            After this setting is enabled, any previously configured hardware switches will appear in the Network > Interfaces page under VLAN Switch.
To enable VLAN switch mode in the GUI:
- 
                                                    Go to System > Settings.
- 
                                                    In the View Settings section, enable VLAN switch mode.
- 
                                                    Click Apply.
Basic configurations
Hardware switch ports can be configured as either a VLAN switch port or a trunk port. The available interfaces and allowable VLAN IDs that can be used depend on the FortiGate model. It is recommended to remove ports from the default VLAN switch before you begin configurations.
To create a new VLAN and assign ports in the GUI:
- 
                                                    Go to Network > Interfaces and click Create New > Interface.
- 
                                                    Enter a name and configure the following: 
  - 
                                                            Set the Type to VLAN Switch.
  - 
                                                            Enter a VLAN ID.
  - 
                                                            Click the + and add the Interface Members.
  - 
                                                            Configure the Address and Administrative Access settings as needed.
- 
                                                            
- 
                                                    Click OK.
To create a new VLAN and assign ports in the CLI:
- 
                                                    Configure the VLAN: config system virtual-switch
    edit "VLAN10"
        set physical-switch "sw0"
        set vlan 10
        config port
            edit "internal1"
            next
            edit "internal2"
            next
        end
    next
end
- 
                                                    Configure the VLAN switch interface addressing: config system interface
    edit "VLAN10"
        set vdom "root"
        set ip 192.168.10.99 255.255.255.0
        set allowaccess ping https ssh snmp http fgfm
        set type hard-switch
    next
end
To designate an interface as a trunk port:
config system interface
    edit internal5
        set trunk enable   
    next
end
                                            Example 1: HA using a VLAN switch
In this example, two FortiGates in an HA cluster are connected to two ISP routers. Instead of connecting to external L2 switches, each FortiGate connects to each ISP router on the same hardware switch port on the same VLAN. A trunk port connects the two FortiGates to deliver the 802.1Q tagged traffic to the other. A full mesh between the FortiGate cluster and the ISP routers is achieved where no single point of failure will cause traffic disruptions.
This example assumes that the HA settings are already configured. The interface and VLAN switch settings are identical between cluster members and synchronized. See HA using a hardware switch to replace a physical switch for a similar example that does not use a VLAN switch.
To configure the VLAN switches:
- 
                                                    Configure the ISP interfaces with the corresponding VLAN IDs: config system virtual-switch
    edit "ISP1"
        set physical-switch "sw0"
        set vlan 2951
        config port
            edit "port1"
            next
        end
    next
    edit "ISP2"
        set physical-switch "sw0"
        set vlan 2952
        config port
            edit "port2"
            next
        end
    next
end
- 
                                                    Configure the VLAN switch interface addressing: config system interface
    edit "ISP1"
        set vdom "root"
        set ip 192.168.10.99 255.255.255.0
        set allowaccess ping 
        set type hard-switch
    next
    edit "ISP2"
        set vdom "root"
        set ip 192.168.20.99 255.255.255.0
        set allowaccess ping 
        set type hard-switch
    next
end
- 
                                                    Designate port15 as the trunk port: config system interface
    edit port15
        set trunk enable   
    next
end
- 
                                                    Configure firewall policies to allow outgoing traffic on the ISP1 and ISP2 interfaces: config firewall policy
    edit 1
        set srcintf "port11"
        set dstintf "ISP1"
        set srcaddr "all"
        set dstaddr "all"
        set action accept
        set schedule "always"
        set service "ALL"
        set nat enable
    next
    edit 2
        set srcintf "port11"
        set dstintf "ISP2"
        set srcaddr "all"
        set dstaddr "all"
        set action accept
        set schedule "always"
        set service "ALL"
        set nat enable
    next
end
Example 2: LAN extension
In this example, two hardware switch ports are assigned VLAN10, and two ports are assigned VLAN20 on FortiGate B. The wan2 interface is designated as the trunk port, and is connected to the upstream FortiGate A. The corresponding VLAN subinterfaces VLAN10 and VLAN20 on the upstream FortiGate allow further access to other networks.
|  | The available interfaces and VLAN IDs varies between FortiGate models. The FortiGate B in this example is a 60F model. | 
To configure FortiGate B:
- 
                                                    Configure the VLAN interfaces: config system virtual-switch
    edit "VLAN10"
        set physical-switch "sw0"
        set vlan 10
        config port
            edit "internal1"
            next
            edit "internal2"
            next
        end
    next
    edit "VLAN20"
        set physical-switch "sw0"
        set vlan 20
        config port
            edit "internal3"
            next
            edit "internal4"
            next
        end
    next
end
- 
                                                    Configure the VLAN switch interface addressing: config system interface
    edit "VLAN10"
        set vdom "root"
        set ip 192.168.10.99 255.255.255.0
        set allowaccess ping https ssh snmp http fgfm
        set type hard-switch
    next
    edit "VLAN20"
        set vdom "root"
        set ip 192.168.20.99 255.255.255.0
        set allowaccess ping https ssh snmp http fgfm
        set type hard-switch
    next
end
- 
                                                    Designate wan2 as the trunk port: config system interface
    edit wan2
        set trunk enable   
    next
end
To configure FortiGate A:
- 
                                                    Configure the VLAN subinterfaces: config system interface
    edit "VLAN10"
        set ip 192.168.10.98 255.255.255.0
        set allowaccess ping https ssh
        set role lan
        set interface "dmz"
        set vlanid 10
    next
    edit "VLAN20"
        set ip 192.168.20.98 255.255.255.0
        set allowaccess ping https ssh
        set role lan
        set interface "dmz"
        set vlanid 20
    next
end
- 
                                                    Configure the DHCP server on VLAN10: config system dhcp server
    edit 0
        set dns-service default
        set default-gateway 192.168.10.98
        set netmask 255.255.255.0
        set interface "VLAN10 "
        config ip-range
            edit 1
                set start-ip 192.168.10.100
                set end-ip 192.168.10.254
            next
        end
        set timezone-option default
    next
end
- 
                                                    Configure firewall policies that allow traffic from the VLAN10 and VLAN20 interfaces to the internet: config firewall policy
    edit 0
        set name "VLAN10-out"
        set srcintf "VLAN10"
        set dstintf "wan1"
        set srcaddr "all"
        set dstaddr "all"
        set action accept
        set schedule "always"
        set service "ALL"
        set nat enable
    next
    edit 0
        set name "VLAN20-out"
        set srcintf "VLAN20"
        set dstintf "wan1"
        set srcaddr "all"
        set dstaddr "all"
        set action accept
        set schedule "always"
        set service "ALL"
        set nat enable
    next
end
To test the connection:
- 
                                                    Connect a PC to internal1 on FortiGate B.
- 
                                                    Verify that it receives an IP address from FortiGate A's DHCP server.
- 
                                                    From the PC, ping FortiGate B on 192.168.10.99.
- 
                                                    Ping FortiGate A on 192.168.10.98.
- 
                                                    Connect to the internet. Traffic is allowed by the VLAN10-out policy.