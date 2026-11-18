# Inter-VDOM routing configuration example: Internet access

This example shows how to configure a FortiGate unit to use inter-VDOM routing to route outgoing traffic from individual VDOMs to a root VDOM with Internet access. See Inter-VDOM routing for more information.

Two departments of a company, Accounting and Sales, are connected to one FortiGate. The company uses a single ISP to connect to the Internet. This is an example of the Internet access configuration. See Topologies for details.


This example assumes that the interfaces of the FortiGate have already been configured with the IP addresses depicted in the preceding diagram.

## General steps for this example

This example includes the following general steps. We recommend following the steps in the order below:

This example demonstrates how to configure these steps first using the GUI and then, at the end of the section, using the CLI. See Configuration with the CLI for details.

Create the Accounting and Sales VDOMs.

###### To enable VDOMs in the GUI:

1. 
                                                    Go to *System > Settings* .
2. 
                                                    In the *System Operation Settings* section, enable*Virtual Domains* .
3. 
                                                    Click *OK* .

|  | On FortiGate 90 series models and lower, VDOMs can only be enabled using the CLI. | 

###### To create the Sales and Accounting VDOMs in the GUI:

1. 
                                                    In the *Global* VDOM, go to*System > VDOM* .
2. 
                                                    Click *Create New* .
3. 
                                                    In the *Virtual Domain* field, enter*Sales* .
4. 
                                                    If required, set the *NGFW Mode* . If the*NGFW Mode* is*Profile-based* ,*Central SNAT* can be enabled.
5. 
                                                    Click *OK* to create the VDOM.
6. 
                                                    Repeat the above steps for *Accounting* .

This example uses three interfaces on the FortiGate unit: port2 (AccountingLocal), port3 (SalesLocal), and port1 (WAN). Port2 and port3 interfaces each have a department's network connected. Port1 is for all traffic to and from the Internet and uses DHCP to configure its IP address, which is common with many ISPs.

###### To assign interfaces to VDOMs in the GUI:

1. 
                                                    In the *Global* VDOM, go to*Network > Interfaces* .
2. 
                                                    Select *port2* and click*Edit* .
3. 
                                                    From the *Virtual domain* list, select*Accounting* .
4. 
                                                    Click *OK* .
5. 
                                                    Repeat the preceding steps to assign *port3* to the*Sales* VDOM.
6. 
                                                    Repeat the preceding steps to assign *port1* to the*root* VDOM.

To complete the connection between each VDOM and the management VDOM, add the two VDOM links. One pair is the Accounting - management link and the other is the Sales - management link. Each side of these links will be assigned IP addresses since they will be handy in configuring inter-VDOM routing in the next step.

###### To configure the Accounting and management VDOM link in the GUI:

1. 
                                                    In the *Global* VDOM, go to*Network > Interfaces* .
2. 
                                                    Select *Create New > VDOM Link* .
3. 
                                                    Enter the following information: **Name**AccountVlnk **Interface 0****Virtual Domain**Accounting **IP/Netmask**11.11.11.2/255.255.255.252 **Administrative Access**HTTPS, PING, SSH **Comment**Accounting side of the VDOM link **Interface 1****Virtual Domain**root **IP/Netmask**11.11.11.1/255.255.255.252 **Administrative Access**HTTPS, PING, SSH **Comment**Management side of the VDOM link
4. 
                                                    Click *OK* .

###### To configure the Sales and management VDOM link in the GUI:

1. 
                                                    In the *Global* VDOM, go to*Network > Interfaces* .
2. 
                                                    Select *Create New > VDOM link* .
3. 
                                                    Enter the following information: **Name**SalesVlnk **Interface 0****Virtual Domain**Sales **IP/Netmask**12.12.12.2/255.255.255.252 **Administrative Access**HTTPS, PING, SSH **Comment**Accounting side of the VDOM link **Interface 1****Virtual Domain**root **IP/Netmask**12.12.12.1/255.255.255.252 **Administrative Access**HTTPS, PING, SSH **Comment**Management side of the VDOM link
4. 
                                                    Click *OK* .

A default static route can be configured on each VDOM to provide Internet access. In other words, this static route would provide inter-VDOM routing between each department VDOM and the root VDOM.

For this static route, these settings are used:

- 
                                                    Default Gateway: IP address of the management side of the VDOM link 
  - 
                                                            Accounting VDOM: 11.11.11.1
  - 
                                                            Sales VDOM: 12.12.12.1
- 
                                                            
- 
                                                    Interface: Interface on the department VDOM side of the VDOM link 
  - 
                                                            Accounting VDOM: AccountVlnk0
  - 
                                                            Sales VDOM: SalesVlnk0
- 
                                                            
- 
                                                    IP address: 0.0.0.0/0.0.0.0 (default)

###### To configure the default static route to the Internet in the Accounting VDOM:

1. 
                                                    In the *Accounting* VDOM, go to*Network > Static Routes* .
2. 
                                                    Click on *Create New* and select the version you need.
3. 
                                                    Enter the following information: **Destination**Subnet **IP address**0.0.0.0/0.0.0.0 **Gateway**11.11.11.1 **Interface**AccountVlink0 **Administrative Distance**10
4. 
                                                    Click *OK* .

###### To configure the default static route to the Internet in the Sales VDOM:

1. 
                                                    In the *Sales* VDOM, go to*Network > Static Routes* .
2. 
                                                    Click on *Create New* and select the version you need.
3. 
                                                    Enter the following information: **Destination**Subnet **IP address**0.0.0.0/0.0.0.0 **Gateway**12.12.12.1 **Interface**SalesVlink0 **Administrative Distance**10
4. 
                                                    Click *OK* .

With the VDOMs, physical interfaces, VDOM links, and static routes configured, the firewall must now be configured to allow the proper traffic. Firewalls are configured per-VDOM, and firewall objects and routes must be created for each VDOM separately.

###### To configure the firewall policies from AccountingLocal to Internet in the GUI:

1. 
                                                    In the *Accounting* VDOM, go to*Policy & Objects > Firewall Policy* .
2. 
                                                    Click *Create New* .
3. 
                                                    Enter the following information: **Name**Account-Local-to-Management **Incoming Interface**port2 **Outgoing Interface**AccountVlnk0 **Source**All **Destination**All **Schedule**always **Service**ALL **Action**ACCEPT **NAT**enabled
4. 
                                                    Click *OK* .
5. 
                                                    In the *root* VDOM, go to*Policy & Objects > Firewall Policy* .
6. 
                                                    Click *Create New* .
7. 
                                                    Enter the following information: **Name**Account-VDOM-to-Internet **Incoming Interface**AccountVlnk1 **Outgoing Interface**port1 **Source**All **Destination**All **Schedule**always **Service**ALL **Action**ACCEPT **NAT**enabled
8. 
                                                    Click *OK* .

###### To configure the firewall policies from SalesLocal to Internet in the GUI:

1. 
                                                    In the *Sales* VDOM, go to*Policy & Objects > Firewall Policy* .
2. 
                                                    Click *Create New* .
3. 
                                                    Enter the following information: **Name**Sales-Local-to-Management **Incoming Interface**port3 **Outgoing Interface**SalesVlnk0 **Source**All **Destination**All **Schedule**always **Service**ALL **Action**ACCEPT **NAT**enabled
4. 
                                                    Click *OK* .
5. 
                                                    In the *root* VDOM, go to*Policy & Objects > Firewall Policy* .
6. 
                                                    Click *Create New* .
7. 
                                                    Enter the following information: **Name**Sales-VDOM-to-Internet **Incoming Interface**SalesVlnk1 **Outgoing Interface**port1 **Source**All **Destination**All **Schedule**always **Service**ALL **Action**ACCEPT **NAT**enabled
8. 
                                                    Click *OK* .

When the inter-VDOM routing has been configured, test the configuration to confirm proper operation. Testing connectivity ensures that physical networking connections, FortiGate unit interface configurations, and firewall policies are properly configured.

The easiest way to test connectivity is to use the `ping` and `traceroute` commands on hosts in the Accounting and Sales networks, respectively, to confirm the connectivity of different routes on the network. Test connectivity with hosts connected to port2 (AccountingLocal) in the Accounting VDOM to the internet and hosts connected to port3 (SalesLocal) in the Sales VDOM to the internet.

The example can also be configured in the CLI.

###### To configure inter-VDOM routing in the CLI:

1. 
                                                    Enable multi-VDOM mode: ```
config system global
    set vdom-mode multi-vdom
end
```
You will be logged out of the device when VDOM mode is enabled.
2. 
                                                    Create the Sales and Accounting VDOMs: ```
config vdom
    edit Accounting
    next
    edit Sales
    next
end
```
3. 
                                                    Assign interfaces to the VDOMs: ```
config global
    config system interface
        edit port2
            set vdom Accounting
        next
        edit port3
            set vdom Sales
        next
        edit port1
            set vdom root
        next
    end
end
```
4. 
                                                    Configure the Accounting and management VDOM link: ```
config global
    config system vdom-link
        edit AccountVlnk
        next
    end
    config system interface
        edit AccountVlnk0
            set vdom Accounting
            set ip 11.11.11.2 255.255.255.252
            set allowaccess https ping ssh
            set description "Accounting side of the VDOM link"
        next
        edit AccountVlnk1
            set vdom root
            set ip 11.11.11.1 255.255.255.252
            set allowaccess https ping ssh
            set description "Management side of the VDOM link"
        next
    end
end
```
5. 
                                                    Configure the Sales and management VDOM link: ```
config global
    config system vdom-link
        edit SalesVlnk
        next
    end
    config system interface
        edit SalesVlnk0
            set vdom Sales
            set ip 12.12.12.2 255.255.255.252
            set allowaccess https ping ssh
            set description "Sales side of the VDOM link"
        next
        edit SalesVlnk1
            set vdom root
            set ip 12.12.12.1 255.255.255.252
            set allowaccess https ping ssh
            set description "Management side of the VDOM link"
        next
    end
end
```
6. 
                                                    Configure the default static route to the Internet in the Accounting VDOM: ```
config vdom
    edit Accounting
    config router static
        edit 1
            set gateway 11.11.11.1
            set device "AccountVlnk0"
        next
    end
end
```
7. 
                                                    Configure the default statis route to the Internet in the Sales VDOM: ```
config vdom
    edit Sales
    config router static
        edit 1
            set gateway 12.12.12.1
            set device "SalesVlnk0"
        next
    end
end
```
8. 
                                                    Configure the firewall policies from AccountingLocal to the Internet: ```
config vdom
    edit Accounting
        config firewall policy
            edit 1
                set name "Accounting-Local-to-Management"
                set srcintf port2
                set dstintf AccountVlnk0
                set srcaddr all
                set dstaddr all
                set action accept
                set schedule always
                set service ALL
                set nat enable
            next
        end
    next
    edit root
        config firewall policy
            edit 2
                set name "Accounting-VDOM-to-Internet"
                set srcintf AccountVlnk1
                set dstintf port1
                set srcaddr all
                set dstaddr all
                set action accept
                set schedule always
                set service ALL
                set nat enable
            next
        end
    next
end
```
9. 
                                                    Configure the firewall policies from SalesLocal to the Internet: ```
config vdom
    edit Sales
        config firewall policy
            edit 3
                set name "Sales-local-to-Management"
                set srcintf port3
                set dstintf SalesVlnk0
                set srcaddr all
                set dstaddr all
                set action accept
                set schedule always
                set service ALL
                set nat enable
            next
        end
    next
    edit root
        config firewall policy
            edit 4
                set name "Sales-VDOM-to-Internet"
                set srcintf SalesVlnk1
                set dstintf port1
                set srcaddr all
                set dstaddr all
                set action accept
                set schedule always
                set service ALL
                set nat enable
            next
        end
    next
end
```