---
id: collect-261121-veille-ia/claude-anthropic/example-vlan-configuration-in-nat-mode
title: "Example VLAN configuration in NAT mode"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["vlan", "interface", "fortigate", "external", "unit", "configuration", "example", "internal"]
source: https://www.fortinetguru.com/2016/12/example-vlan-configuration-in-nat-mode/
source_anchor: ""
source_lines: [1, 463]
section: "Claude_Anthropic"
---
**E****xa****m****p****l****e VLAN configuration in NAT mode**

In this example two different internal VLAN networks share one interface on the FortiGate unit, and share the connection to the Internet. This example shows that two networks can have separate traffic streams while sharing a single interface. This configuration could apply to two departments in a single company, or to different companies.

There are two different internal network VLANs in this example. VLAN_100 is on the 10.1.1.0/255.255.255.0 subnet, and VLAN_200 is on the 10.1.2.0/255.255.255.0 subnet. These VLANs are connected to the VLAN switch, such as a Cisco 2950 Catalyst switch.

The FortiGate internal interface connects to the VLAN switch through an 802.1Q trunk. The internal interface has an IP address of 192.168.110.126 and is configured with two VLAN subinterfaces (VLAN_100 and VLAN_200). The external interface has an IP address of 172.16.21.2 and connects to the Internet. The external interface has no VLAN subinterfaces.


**Fo****r****t****i****G****a****t****e unit with VLANs in NAT mode**

When the VLAN switch receives packets from VLAN_100 and VLAN_200, it applies VLAN ID tags and forwards the packets of each VLAN both to local ports and to the FortiGate unit across the trunk link. The FortiGate unit has policies that allow traffic to flow between the VLANs, and from the VLANs to the external network.

This section describes how to configure a FortiGate unit and a Cisco Catalyst 2950 switch for this example network topology. The Cisco configuration commands used in this section are IOS commands.

It is assumed that both the FortiGate unit and the Cisco 2950 switch are installed and connected and that basic configuration has been completed. On the switch, you will need to be able to access the CLI to enter commands.

Refer to the manual for your FortiGate model as well as the manual for the switch you select for more information.

It is also assumed that no VDOMs are enabled.


**G****e****n****e****r****a****l configuration steps**

The following steps provide an overview of configuring and testing the hardware used in this example. For best results in this configuration, follow the procedures in the order given. Also, note that if you perform any additional actions between procedures, your configuration may have different results.

**1****.** Configure the FortiGate unit

- Configure the external interface
- Add two VLAN subinterfaces to the internal network interface
- Add firewall addresses and address ranges for the internal and external networks
- Add security policies to allow:
- the VLAN networks to access each other
- the VLAN networks to access the external network.

**2****.** Configure the VLAN switch


**C****on****f****i****gu****r****e the FortiGate unit**

Configuring the FortiGate unit includes:


**C****on****f****i****gu****r****e the external interface**

The FortiGate unit’s external interface will provide access to the Internet for all internal networks, including the two VLANs.


**T****o configure the external interface – web-based manager**

**1****.** Go to **S****ys****t****e****m > Network > Interface**.

**2****.** Select **E****d****i****t** for the external interface.

**3****.** Enter the following information and select **O****K**:

**A****dd****r****ess****i****n****g mode**                     Manual

**I****P****/****N****e****t****w****o****r****k Mask**                       172.16.21.2/255.255.255.0


**T****o configure the external interface – CLI**

config system interface edit external

set mode static

set ip 172.16.21.2 255.255.255.0

end


**A****d****d VLAN subinterfaces**

This step creates the VLANs on the FortiGate unit internal physical interface. The IP address of the internal interface does not matter to us, as long as it does not overlap with the subnets of the VLAN subinterfaces we are configuring on it.

The rest of this example shows how to configure the VLAN behavior on the FortiGate unit, configure the switches to direct VLAN traffic the same as the FortiGate unit, and test that the configuration is correct.

Adding VLAN subinterfaces can be completed through the web-based manager, or the CLI.


**T****o add VLAN subinterfaces – web-based manager**

**1****.** Go to **S****ys****t****e****m > Network > Interface**.

**2****.** Select **C****r****ea****t****e New**.

**3****.** Enter the following information and select **O****K**:

**N****a****m****e**                                           VLAN_100

**I****n****t****e****r****f****ac****e**                                     internal

**V****L****A****N ID**                                      100

**A****dd****r****ess****i****n****g mode**                     Manual

**I****P****/****N****e****t****w****o****r****k Mask**                       10.1.1.1/255.255.255.0

**A****d****m****i****n****i****s****t****r****a****t****i****v****e Access**             HTTPS, PING, TELNET

**4****.** Select **C****r****ea****t****e New**.

**5****.** Enter the following information and select **O****K**:

**N****a****m****e**                                           VLAN_200

**I****n****t****e****r****f****ac****e**                                     internal

**V****L****A****N ID**                                      200

**A****dd****r****ess****i****n****g mode**                     Manual

**I****P****/****N****e****t****w****o****r****k Mask**                       10.1.2.1/255.255.255.0

**A****d****m****i****n****i****s****t****r****a****t****i****v****e Access**             HTTPS, PING, TELNET


**T****o add VLAN subinterfaces – CLI**

config system interface edit VLAN_100

set vdom root

set interface internal set type vlan

set vlanid 100 set mode static

set ip 10.1.1.1 255.255.255.0

set allowaccess https ping telnet next

edit VLAN_200

set vdom root

set interface internal

end

set type vlan set vlanid 200 set mode static

set ip 10.1.2.1 255.255.255.0

set allowaccess https ping telnet



**A****d****d the firewall addresses**

You need to define the addresses of the VLAN subnets for use in security policies. The FortiGate unit provides one default address, “all”, that you can use when a security policy applies to all addresses as a source or destination of a packet. However, using “all” is less secure and should be avoided when possible.

In this example, the “_Net” part of the address name indicates a range of addresses instead of a unique address. When choosing firewall address names, use informative and unique names.


**T****o add the firewall addresses – web-based manager**

**1****.** Go to **F****i****r****e****w****a****l****l Objects > Address > Addresses**.

**2****.** Select **C****r****ea****t****e New**.

**3****.** Enter the following information and select **O****K**:

**N****a****m****e**                                          VLAN_100_Net

**T****y****p****e**                                            Subnet

**S****ubn****e****t / IP Range**                     10.1.1.0/255.255.255.0

**4****.** Select **C****r****ea****t****e New**.

**5****.** Enter the following information and select **O****K**:

**N****a****m****e**                                          VLAN_200_Net

**T****y****p****e**                                            Subnet

**S****ubn****e****t / IP Range**                     10.1.2.0/255.255.255.0


**T****o add the firewall addresses – CLI**

config firewall address edit VLAN_100_Net

set type ipmask

set subnet 10.1.1.0 255.255.255.0 next

edit VLAN_200_Net set type ipmask

set subnet 10.1.2.0 255.255.255.0

end


**A****d****d the security policies**

Once you have assigned addresses to the VLANs, you need to configure security policies for them to allow valid packets to pass from one VLAN to another and to the Internet.

You can customize the Security Policy display by including some or all columns, and customize the column order onscreen. Due to this feature, security policy screenshots may not appear the same as on your screen.

If you do not want to allow all services on a VLAN, you can create a security policy for each service you want to allow. This example allows all services.


**T****o add the security policies – web-based manager**

**1****.** Go to **P****o****li****c****y & Objects > Policy > IPv4** or **P****o****li****c****y & Objects > Policy > IPv6** and select **C****r****ea****t****e New**.

**2****.** Leave the **P****o****li****c****y Type** as **F****i****r****e****w****a****l****l** and the **P****o****li****c****y Subtype** as **A****dd****r****ess**.

**3****.** Enter the following information and select **O****K**:

**I****n****c****o****m****i****n****g Interface**                   VLAN_100

**S****ou****r****c****e Address**                        VLAN_100_Net

**O****u****t****go****i****n****g Interface**                   VLAN_200

**D****es****t****i****n****a****t****i****o****n Address**                 VLAN_200_Net

**S****c****h****e****du****le**                                    Always

**S****e****r****v****i****c****e**                                       ALL

**A****c****t****i****o****n**                                         ACCEPT

**E****n****a****b****l****e NAT**                                Enable

**4****.** Select **C****r****ea****t****e New**.

**5****.** Leave the **P****o****li****c****y Type** as **F****i****r****e****w****a****l****l** and the **P****o****li****c****y Subtype** as **A****dd****r****ess**.

**6****.** Enter the following information and select **O****K**:

**I****n****c****o****m****i****n****g Interface**                   VLAN_200

**S****ou****r****c****e Address**                        VLAN_200_Net

**O****u****t****go****i****n****g Interface**                   VLAN_100

**D****es****t****i****n****a****t****i****o****n Address**                 VLAN_100_Net

**S****c****h****e****du****l****e**                                    Always

**S****e****r****v****i****c****e**                                       ALL

**A****c****t****i****o****n**                                         ACCEPT

**E****n****a****b****l****e NAT**                                Enable

**7****.** Select **C****r****ea****t****e New**.

**8****.** Leave the **P****o****li****c****y Type** as **F****i****r****e****w****a****l****l** and the **P****o****li****c****y Subtype** as **A****dd****r****ess**.

**9****.** Enter the following information and select **O****K**:

**I****n****c****o****m****i****n****g Interface**                   VLAN_100

**S****ou****r****c****e Address**                        VLAN_100_Net

**O****u****t****go****i****n****g Interface**                   external

**D****es****t****i****n****a****t****i****o****n Address**                 all

**S****c****h****e****du****l****e**                                    Always

**S****e****r****v****i****c****e**                                       ALL

**A****c****t****i****o****n**                                         ACCEPT

**E****n****a****b****l****e NAT**                                Enable

**10****.** Select **C****r****ea****t****e New**.

**11****.** Verify the **P****o****li****c****y Type** is **F****i****r****e****w****a****l****l** and the **P****o****li****c****y Subtype** is **A****dd****r****ess**.

**12****.** Enter the following information and select **O****K**:

**I****n****c****o****m****i****n****g Interface**                   VLAN_200

**S****ou****r****c****e Address**                        VLAN_200_Net

**O****u****t****go****i****n****g Interface**                   external

**D****es****t****i****n****a****t****i****o****n Address**                 all

**S****c****h****e****du****l****e**                                    Always

**S****e****r****v****i****c****e**                                       ALL

**A****c****t****i****o****n**                                         ACCEPT

**E****n****a****b****l****e NAT**                                Enable


**T****o add the security policies – CLI**

config firewall policy or Config firewall policy6 edit 1

set srcintf VLAN_100

set srcaddr VLAN_100_Net set dstintf VLAN_200

set dstaddr VLAN_200_Net set schedule always

set service ALL set action accept set nat enable

set status enable next

edit 2

set srcintf VLAN_200

set srcaddr VLAN_200_Net set dstintf VLAN_100

set dstaddr VLAN_100_Net set schedule always

set service ALL set action accept set nat enable

set status enable next

edit 3

set srcintf VLAN_100

set srcaddr VLAN_100_Net set dstintf external

set dstaddr all

set schedule always set service ALL

set action accept set nat enable

set status enable next

edit 4

set srcintf VLAN_200

set srcaddr VLAN_200_Net set dstintf external

set dstaddr all

set schedule always set service ALL

set action accept set nat enable

set status enable

end


**C****on****f****i****gu****r****e the VLAN switch**

On the Cisco Catalyst 2950 Catalyst VLAN switch, you need to define VLANs 100 and 200 in the VLAN database, and then add a configuration file to define the VLAN subinterfaces and the 802.1Q trunk interface.

One method to configure a Cisco switch is to connect over a serial connection to the console port on the switch, and enter the commands at the CLI. Another method is to designate one interface on the switch as the management interface and use a web browser to connect to the switch’s graphical interface. For details on connecting and configuring your Cisco switch, refer to the installation and configuration manuals for the switch.

The switch used in this example is a Cisco Catalyst 2950 switch. The commands used are IOS commands. Refer to the switch manual for help with these commands.


**T****o configure the VLAN subinterfaces and the trunk interfaces**

Add this file to the Cisco switch:

!

interface FastEthernet0/3 switchport access vlan 100

!

interface FastEthernet0/9 switchport access vlan 200

!

interface FastEthernet0/24

switchport trunk encapsulation dot1q switchport mode trunk

!

The switch has the configuration:

**P****o****r****t 0/3**                         VLAN ID 100

**P****o****r****t 0/9**                         VLAN ID 200

**P****o****r****t 0/24**                       802.1Q trunk

To complete the setup, configure devices on VLAN_100 and VLAN_200 with default gateways. The default gateway for VLAN_100 is the FortiGate VLAN_100 subin- terface. The default gateway for VLAN_200 is the FortiGate VLAN_200 subinterface.


**T****es****t the configuration**

Use diagnostic commands, such as tracert, to test traffic routed through the FortiGate unit and the Cisco switch.


**T****es****t****i****n****g traffic from VLAN_100 to VLAN_200**

In this example, a route is traced between the two internal networks. The route target is a host on VLAN_200. Access a command prompt on a Windows computer on the VLAN_100 network, and enter the following command:


C:\>tracert 10.1.2.2

Tracing route to 10.1.2.2 over a maximum of 30 hops:

1 <10 ms <10 ms <10 ms 10.1.1.1

2 <10 ms <10 ms <10 ms 10.1.2.2

Trace complete.


**T****es****t****i****n****g traffic from VLAN_200 to the external network**

In this example, a route is traced from an internal network to the external network. The route target is the external network interface of the FortiGate-800 unit.

From VLAN_200, access a command prompt and enter this command:

C:\>tracert 172.16.21.2

Tracing route to 172.16.21.2 over a maximum of 30 hops:

1 <10 ms <10 ms <10 ms 10.1.2.1

2 <10 ms <10 ms <10 ms 172.16.21.2

Trace complete.

ja
Sir, i have a Question below:

1. do we need to add any routing in the Fortiget for each vlan? if yes, what will be the routing.

2. how to allow Internet in one PC only in vlan 200 (IP 10.1.2.10)

MikePost author
If you are creating the VLANs via the FortiGate then they are connected routes.

KC
Hello Mike,

I currently have a Fortinet 80C that is configured with 192.168.25.1 on port 2 (LAN) with no VDOM configured. I now need to make this a VLAN and add two other VLAN’s going to a layer 2 switch (Router on a stick). I created two of the VLAN’s but I’m unable to change 192.168.25.1 to a VLAN on the Fortinet. Can you tell me how to accomplish this. Thank you in advance for any assistance you can offer.

MikePost author
You are going to have to remove the IP from the physical interface (which uses only native tagging) and manually recreate using the create interface process. Schedule a maintenance window.

– Leave all of your existing policies alone (hopefully your address objects and VIPs etc aren’t tied to the interface specifically)

– Set physical interface the 192.168.25.1 address is currently assigned to so that it shows 0.0.0.0/0.0.0.0

– Create a new interface like you did the others, set it to VLAN xyz or whatever you like, name it accordingly, tag it accordingly, and place that IP address there

– Switch the source and destination interfaces of your previous policies accordingly so that they all transfer over 🙂

artur
hello ,

i dont fully understand why in your example the internal intrafce has an ip ? if you configuring the internal interface as a trunk port so its need to be with no address on it like 0.0.0.0/0 , and then create vlans assign the valns to the internal ports and give to the vlans you created the IP addresses.

please correct me ig i worng ?

thank you
