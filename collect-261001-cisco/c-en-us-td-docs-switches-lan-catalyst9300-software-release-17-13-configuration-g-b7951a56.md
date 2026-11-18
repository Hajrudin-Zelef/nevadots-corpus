Prerequisites for Port Security
If you try to set the maximum value to a number less than the number of secure addresses already configured on an interface, the command is rejected.
The maximum number of secure MAC addresses that you can configure on a switch is set by the maximum number of available MAC addresses allowed in the system. This number is the total of available MAC addresses, including those used for other Layer 2 functions and any other secure MAC addresses configured on interfaces.
Port Security is not supported on EtherChanel interfaces.
Port Security is not supported on private VLAN ports.
We recommend that you do not enable port security on an 802.1X authenticator interface.
When port-security is disabled on a port, the 802.1X sessions on the port get removed, because the aging timer and inactivity type is still configured. To ensure that the 802.1X sessions are not removed, when disabling port-security, disable the aging timer and inactivity type by removing the following commands:
switchport port-security aging time 1
switchport port-security aging type inactivity
If the inactivity timer is required, see the section "Enabling and Configuring Port Security Aging".
You can use the port security feature to restrict input to an interface by limiting and identifying MAC addresses of the stations allowed to access the port. When you assign secure MAC addresses to a secure port, the port does not forward packets with source addresses outside the group of defined addresses. If you limit the number of secure MAC addresses to one and assign a single secure MAC address, the workstation attached to that port is assured the full bandwidth of the port.
If a port is configured as a secure port and the maximum number of secure MAC addresses is reached, when the MAC address of a station attempting to access the port is different from any of the identified secure MAC addresses, a security violation occurs. Also, if a station with a secure MAC address configured or learned on one secure port attempts to access another secure port, a violation is flagged.
The switch cannot physically detect the link state for devices connected through a Cisco IP phone. When a port disconnects from the switch, the system immediately removes the dynamic secure MAC addresses, even if the port-security aging inactivity is configured to retain those addresses.
The switch supports these types of secure MAC addresses:
Static secure MAC addresses—These are manually configured by using the switchport port-security mac-address mac-address interface configuration command, stored in the address table, and added to the switch running configuration.
Dynamic secure MAC addresses—These are dynamically configured, stored only in the address table, and removed when the switch restarts.
Sticky secure MAC addresses—These can be dynamically learned or manually configured, stored in the address table, and added to the running configuration. If these addresses are saved in the configuration file, when the switch restarts, the interface does not need to dynamically reconfigure them.
The following table shows the default settings for the MAC address table.
| Table 1.  Default Settings for the MAC Address |  | 
|---|---|
| Feature | Default Setting | 
|---|---|
| Aging time | 300 seconds | 
| Dynamic addresses | Automatically learned | 
| Static addresses | None configured | 
With multiple MAC addresses supported on all ports, you can connect any port on the device to other network devices. The device provides dynamic addressing by learning the source address of packets it receives on each port and adding the address and its associated port number to the address table. As devices are added or removed from the network, the device updates the address table, adding new dynamic addresses and aging out those that are not in use.
The aging interval is globally configured. However, the device maintains an address table for each VLAN, and STP can accelerate the aging interval on a per-VLAN basis.
The device sends packets between any combination of ports, based on the destination address of the received packet. Using the MAC address table, the device forwards the packet only to the port associated with the destination address. If the destination address is on the port that sent the packet, the packet is filtered and not forwarded. The device always uses the store-and-forward method: complete packets are stored and checked for errors before transmission.
You can configure an interface to convert the dynamic MAC addresses to sticky secure MAC addresses and to add them to the running configuration by enabling sticky learning. The interface converts all the dynamic secure MAC addresses, including those that were dynamically learned before sticky learning was enabled, to sticky secure MAC addresses. All sticky secure MAC addresses are added to the running configuration.
The sticky secure MAC addresses do not automatically become part of the configuration file, which is the startup configuration used each time the switch restarts. If you save the sticky secure MAC addresses in the configuration file, when the switch restarts, the interface does not need to relearn these addresses. If you do not save the sticky secure addresses, they are lost.
If sticky learning is disabled, the sticky secure MAC addresses are converted to dynamic secure addresses and are removed from the running configuration.
It is a security violation when one of these situations occurs:
The maximum number of secure MAC addresses have been added to the address table, and a station whose MAC address is not in the address table attempts to access the interface.
An address learned or configured on one secure interface is seen on another secure interface in the same VLAN.
Running diagnostic tests with port security enabled.
You can configure the interface for one of three violation modes, based on the action to be taken if a violation occurs:
protect—when the number of secure MAC addresses reaches the maximum limit allowed on the port, packets with unknown source addresses are dropped until you remove a sufficient number of secure MAC addresses to drop below the maximum value or increase the number of maximum allowable addresses. You are not notified that a security violation has occurred.
| Note | We do not recommend configuring the protect violation mode on a trunk port. The protect mode disables learning when any VLAN reaches its maximum limit, even if the port has not reached its maximum limit. | 
restrict—when the number of secure MAC addresses reaches the maximum limit allowed on the port, packets with unknown source addresses are dropped until you remove a sufficient number of secure MAC addresses to drop below the maximum value or increase the number of maximum allowable addresses. In this mode, you are notified that a security violation has occurred. An SNMP trap is sent, a syslog message is logged, and the violation counter increments.
shutdown—a port security violation causes the interface to become error-disabled and to shut down immediately, and the port LED turns off. When a secure port is in the error-disabled state, you can bring it out of this state by entering the errdisable recovery cause psecure-violation global configuration command, or you can manually re-enable it by entering the shutdown and no shut down interface configuration commands. This is the default mode.
shutdown vlan—Use to set the security violation mode per-VLAN. In this mode, the VLAN is error disabled instead of the entire port when a violation occurs
This table shows the violation mode and the actions taken when you configure an interface for port security.
| Table 2. Security Violation Mode Actions |  |  |  |  |  |  | 
|---|---|---|---|---|---|---|
| Violation Mode | Traffic is forwarded 1 | Sends SNMP trap | Sends syslog message | Displays error message 2 | Violation counter increments | Shuts down port | 
|---|---|---|---|---|---|---|
| protect | No | No | No | No | No | No | 
| restrict | No | Yes | Yes | No | Yes | No | 
| shutdown | No | No | No | No | Yes | Yes | 
| shutdown vlan | No | No | Yes | No | Yes | No 3 | 
You can use port security aging to set the aging time for all secure addresses on a port. Two types of aging are supported per port:
Absolute—The secure addresses on the port are deleted after the specified aging time.
Inactivity—The secure addresses on the port are deleted only if the secure addresses are inactive for the specified aging time.
When a switch joins a stack, the new switch will get the configured secure addresses. All dynamic secure addresses are downloaded by the new stack member from the other stack members.
When a switch (either the active switch or a stack member) leaves the stack, the remaining stack members are notified, and the secure MAC addresses configured or learned by that switch are deleted from the secure MAC address table.
| Table 3. Default Port Security Configuration |  | 
|---|---|
| Feature | Default Setting | 
|---|---|
| Port security | Disabled on a port. | 
| Sticky address learning | Disabled. | 
| Maximum number of secure MAC addresses per port | One address | 
| Violation mode | Shutdown. The port shuts down when the maximum number of secure MAC addresses is exceeded. | 
| Port security aging | Disabled. Aging time is 0. Static aging is disabled. Type is absolute. | 
The following guidelines are applicable during port security configuration:
Port security can only be configured on static access ports or trunk ports. A secure port cannot be a dynamic access port.
A secure port cannot be a destination port for Switched Port Analyzer (SPAN).
Voice VLAN is only supported on access ports and not on trunk ports, even though the configuration is allowed.
When you enable port security on an interface that is also configured with a voice VLAN, set the maximum allowed secure addresses on the port to two. When the port is connected to a Cisco IP phone, the IP phone requires one MAC address. The Cisco IP phone address is learned on the voice VLAN, but is not learned on the access VLAN. If you connect a single PC to the Cisco IP phone, no additional MAC addresses are required. If you connect more than one PC to the Cisco IP phone, you must configure enough secure addresses to allow one for each PC and one for the phone.
When a trunk port configured with port security and assigned to an access VLAN for data traffic and to a voice VLAN for voice traffic, entering the switchport voice and switchport priority extend interface configuration commands has no effect.
When a connected device uses the same MAC address to request an IP address for the access VLAN and then an IP address for the voice VLAN, only the access VLAN is assigned an IP address.
When you enter a maximum secure address value for an interface, and the new value is greater than the previous value, the new value overwrites the previously configured value. If the new value is less than the previous value and the number of configured secure addresses on the interface exceeds the new value, the command is rejected.
The switch does not support port security aging of sticky secure MAC addresses.
This table summarizes port security compatibility with other port-based features.
| Table 4. Port Security Compatibility with Other Switch Features |  | 
|---|---|
| Type of Port or Feature on Port | Compatible with Port Security | 
|---|---|
| DTP 4 port 5 | No | 
| Trunk port | Yes | 
| Dynamic-access port 6 | No | 
| Routed port | No | 
| SPAN source port | Yes | 
| SPAN destination port | No | 
| EtherChannel | No | 
| Tunneling port | Yes | 
| Protected port | Yes | 
| IEEE 802.1x port | Yes | 
| Voice VLAN port 7 | Yes | 
| IP source guard | Yes | 
| Dynamic Address Resolution Protocol (ARP) inspection | Yes | 
| Flex Links | Yes | 
A device in a network allows traffic like SNMP, HTTP, HTTPS, Telnet, Secure Shell SSH and Netconf through any port with any IP address. Traffic flow from various interfaces to the local devices might decrease the security strength in a network.
Management traffic control feature allows traffic to enter through a user-defined physical interface and restricts traffic to any other interfaces that is not defined by the user. When the feature is enabled a single IP address is assigned on the device to receive traffic. The user can configure the feature by defining an interface under the management traffic control feature. When the network protocol and the IP address is set according to the user’s preference, traffic flow is allowed only through the defined interface.
The feature is supported on:
Layer 2 physical interface.
Layer 3 physical interface.
Layer 2 port channel.
Layer 3 port channel.
App-hosting interface.
For example, in the following figure Switch1 and Switch2 are devices that are in a network. Management traffic control feature is enabled on Switch1 with the interface Tengig 1/0/1 and destination IP address 30.30.30.30. Traffic is allowed through the interface with the enabled protocols SSH, Telnet, SNMP, HTTP, HTTPS, Netconf, SCP to destination IP address 30.30.30.30. Traffic passing through interface Tengig 1/0/1 to IP address 20.20.20.20 is dropped. Management traffic control feature enables only one destination IP address to be configured. If interface TenGig 1/0/2 is not defined by the management traffic control feature for any of the devices, traffic will not be allowed to Switch1 but can still pass through to its configured destination in the network.
| Note |  | 
| Table 5. Supported protocols and the respective port numbers |  |  | 
|---|---|---|
| Protocol | Keyword | Port Number | 
|---|---|---|
| HTTPS | TCP | 443 | 
| TELNET | TCP | 23 | 
| SSH | TCP | 22 | 
| NETCONF-SSH | TCP | 830 | 
| SNMP | UDP | 161 | 
| HTTP | TCP | 80 | 
This task restricts input to an interface by limiting and identifying MAC addresses of the stations allowed to access the port:
|  | Command or Action | Purpose | 
|---|---|---|
| Step 1 | enable Example:  Device> enable | Enables privileged EXEC mode.  | 
| Step 2 | configure terminal Example:  Device# configure terminal | Enters global configuration mode. | 
| Step 3 | interface interface-id Example:  Device(config)# interface gigabitethernet 1/0/1   | Specifies the interface to be configured, and enter interface configuration mode. | 
| Step 4 | switchport mode {access \| trunk} Example:  Device(config-if)# switchport mode access   | Sets the interface switchport mode as access or trunk; an interface in the default mode (dynamic auto) cannot be configured as a secure port. | 
| Step 5 | switchport voice vlan vlan-id Example:  Device(config-if)# switchport voice vlan 22   | Enables voice VLAN on a port. vlan-id —Specifies the VLAN to be used for voice traffic. | 
| Step 6 | switchport port-security Example:  Device(config-if)# switchport port-security   | Enables port security on the interface. | 
| Step 7 | switchport port-security [maximum value [vlan {vlan-list \| {access \| voice}}]] Example:  Device(config-if)# switchport port-security maximum 20   | (Optional) Sets the maximum number of secure MAC addresses for the interface. The maximum number of secure MAC addresses that you can configure on a switch or switch stack is set by the maximum number of available MAC addresses allowed in the system. This number is the total of available MAC addresses, including those used for other Layer 2 functions and any other secure MAC addresses configured on interfaces. (Optional) vlan —sets a per-VLAN maximum value Enter one of these options after you enter the vlan keyword:  | 
| Step 8 | switchport port-security violation {protect \| restrict \| shutdown \| shutdown vlan} Example:  Device(config-if)# switchport port-security violation restrict   | (Optional) Sets the violation mode, the action to be taken when a security violation is detected, as one of these:  | 
| Step 9 | switchport port-security [mac-address mac-address [vlan {vlan-id \| {access \| voice}}] Example:  DEvice(config-if)# switchport port-security mac-address 00:A0:C7:12:C9:25 vlan 3 voice   | (Optional) Enters a secure MAC address for the interface. You can use this command to enter the maximum number of secure MAC addresses. If you configure fewer secure MAC addresses than the maximum, the remaining MAC addresses are dynamically learned. (Optional) vlan —sets a per-VLAN maximum value. Enter one of these options after you enter the vlan keyword:  | 
| Step 10 | switchport port-security mac-address sticky Example:  Device(config-if)# switchport port-security mac-address sticky  | (Optional) Enables sticky learning on the interface. | 
| Step 11 | switchport port-security mac-address sticky [mac-address \| vlan {vlan-id \| {access \| voice}}] Example:  Device(config-if)# switchport port-security mac-address sticky 00:A0:C7:12:C9:25 vlan voice   | (Optional) Enters a sticky secure MAC address, repeating the command as many times as necessary. If you configure fewer secure MAC addresses than the maximum, the remaining MAC addresses are dynamically learned, are converted to sticky secure MAC addresses, and are added to the running configuration. (Optional) vlan —sets a per-VLAN maximum value. Enter one of these options after you enter the vlan keyword:  | 
| Step 12 | end Example:  Device(config-if)# end | Exits interface configuration mode and returns to privileged EXEC mode. | 
| Step 13 | show port-security Example:  Device# show port-security   | Displays information about the port-security settings. | 
| Note | Under certain conditions, when port security is enabled on the member ports in a switch stack, the DHCP and ARP packets would be dropped. As a workaround, shutdown the interface and then configure the no shutdown command. | 
| Note | The voice keyword is available only if a voice VLAN is configured on a port and if that port is not the access VLAN. If an interface is configured for voice VLAN, configure a maximum of two secure MAC addresses. | 
| Note | If you enable sticky learning after you enter this command, the secure addresses that were dynamically learned are converted to sticky secure MAC addresses and are added to the running configuration. | 
| Note | If you do not enable sticky learning before this command is entered, an error message appears, and you cannot enter a sticky secure MAC address. | 
Use this feature to remove and add devices on a secure port without manually deleting the existing secure MAC addresses and to still limit the number of secure addresses on a port. You can enable or disable the aging of secure addresses on a per-port basis.
|  | Command or Action | Purpose | 
|---|---|---|
| Step 1 | enable Example:  Device> enable | Enables privileged EXEC mode.  | 
| Step 2 | configure terminal Example:  Device# configure terminal | Enters global configuration mode. | 
| Step 3 | interface interface-id Example:  Device(config)# interface gigabitethernet1/0/1   | Specifies the interface to be configured, and enter interface configuration mode. | 
| Step 4 | switchport port-security aging {static \| time time \| type {absolute \| inactivity}} Example:  Device(config-if)# switchport port-security aging time 120   | Enables or disable static aging for the secure port, or set the aging time or type. Enter static to enable aging for statically configured secure addresses on this port. For time , specifies the aging time for this port. The valid range is from 0 to 1440 minutes. For type , select one of these keywords:  | 
| Step 5 | end Example:  Device(config-if)# end | Exits interface configuration mode and returns to privileged EXEC mode. | 
| Step 6 | show port-security [interface interface-id] [address] Example:  Device# show port-security interface gigabitethernet1/0/1  | Displays information about the port-security settings on the specified interface. | 
| Note | The switch does not support port security aging of sticky secure addresses. | 
Follow these steps to configure the dynamic address table aging time:
|  | Command or Action | Purpose | 
|---|---|---|
| Step 1 | enable Example:  Device> enable | Enables privileged EXEC mode.  | 
| Step 2 | configure terminal Example:  Device# configure terminal | Enters global configuration mode. | 
| Step 3 | mac address-table aging-time [0 \| 10-1000000] [routed-mac \| vlan vlan-id] Example:  Device(config)# mac address-table  aging-time 500 vlan 2   | Sets the length of time that a dynamic entry remains in the MAC address table after the entry is used or updated. The range is 10 to 1000000 seconds. The default is 300. You can also enter 0, which disables aging. Static address entries are never aged or removed from the table. vlan-id— Valid IDs are 1 to 4094. | 
| Step 4 | end Example:  Device(config)# end | Exits global configuration mode and returns to privileged EXEC mode. | 
This table displays port security information.
| Table 6. Commands for Displaying Port Security Status and Configuration |  | 
|---|---|
| Command | Purpose | 
|---|---|
| show port-security [interface interface-id] | Displays port security settings for the device or for the specified interface, including the maximum allowed number of secure MAC addresses for each interface, the number of secure MAC addresses on the interface, the number of security violations that have occurred, and the violation mode. | 
| show port-security [interface interface-id] address | Displays all secure MAC addresses configured on all device interfaces or on a specified interface with aging information for each address. | 
| show port-security interface interface-id vlan | Displays the number of secure MAC addresses configured per VLAN on the specified interface. | 
|  | Command or Action | Purpose | 
|---|---|---|
| Step 1 | enable Example:  Device> enable  | Enables privileged EXEC mode.  | 
| Step 2 | configure terminal Example:  Device# configure terminal  | Enters global configuration mode. | 
| Step 3 | mgmt-traffic control ipv4 Example:  Device# mgmt-traffic control ipv4 Device(config-mtc-ipv4)# | Enables the management traffic control feature. | 
| Step 4 | interface interface-id Example:  Device(config-mtc-ipv4)# interface gigabitethernet1/0/1 Device(config-mtc-ipv4)# | Defines the interface through which traffic is allowed. | 
| Step 5 | protocol{[ telnet http https netconf scp snmp ssh]} Example:  Device(config-mtc-ipv4)# protocol telnet http https netconf scp snmp ssh Device(config-mtc-ipv4)# | Enables the specified network protocols in the interface. | 
| Step 6 | address ip-address Example:  Device(config-mtc-ipv4)# address 30.30.30.30 Device(config-mtc-ipv4)# | Specifies the destination address of traffic. | 
| Step 7 | end Example:  Device(config-mtc-ipv4)# end  | Exits management traffic control configuration mode and returns to privileged EXEC mode. | 
This example shows how to enable port security on a port and to set the maximum number of secure addresses to 50. The violation mode is the default, no static secure MAC addresses are configured, and sticky learning is enabled.
Device> enable
Device# configure terminal
Device(config)# interface gigabitethernet1/0/1
Device(config-if)# switchport mode access
Device(config-if)# switchport port-security
Device(config-if)# switchport port-security maximum 50
Device(config-if)# switchport port-security mac-address sticky
Device(config-if)# end
This example shows how to configure a static secure MAC address on VLAN 3 on a port:
Device> enable
Device# configure terminal
Device(config)# interface gigabitethernet1/0/2
Device(config-if)# switchport mode trunk
Device(config-if)# switchport port-security
Device(config-if)# switchport port-security mac-address 0000.0200.0004 vlan 3
Device(config-if)# end
This example shows how to enable sticky port security on a port, to manually configure MAC addresses for data VLAN and voice VLAN, and to set the total maximum number of secure addresses to 20 (10 for data VLAN and 10 for voice VLAN).
Device> enable
Device# configure terminal
Device(config)# interface tengigabitethernet1/0/1
Device(config-if)# switchport access vlan 21
Device(config-if)# switchport mode access
Device(config-if)# switchport voice vlan 22
Device(config-if)# switchport port-security
Device(config-if)# switchport port-security maximum 20
Device(config-if)# switchport port-security violation restrict
Device(config-if)# switchport port-security mac-address sticky
Device(config-if)# switchport port-security mac-address sticky 0000.0000.0002
Device(config-if)# switchport port-security mac-address 0000.0000.0003
Device(config-if)# switchport port-security mac-address sticky 0000.0000.0001 vlan voice
Device(config-if)# switchport port-security mac-address 0000.0000.0004 vlan voice
Device(config-if)# switchport port-security maximum 10 vlan access
Device(config-if)# switchport port-security maximum 10 vlan voice
Device(config-if)# end
This table provides release and related information for features explained in this module.
These features are available on all releases subsequent to the one they were introduced in, unless noted otherwise.
| Release | Feature | Feature Information | 
|---|---|---|
| Cisco IOS XE Everest 16.5.1a | Port Security | The Port Security feature restricts input to an interface by limiting and identifying MAC addresses of the stations allowed to access the port. | 
| Cisco IOS XE Everest 16.5.1a | Port Security MAC Aging | When devices are added or removed from a network, the device updates the address table, adding new dynamic addresses and aging out those that are not in use. | 
| Cisco IOS XE 17.13.1 | Management Traffic Protocol | Management traffic control feature allows traffic to enter through a user-defined physical interface and restricts traffic to any other interfaces that is not defined by the user. When the network protocol and the IP address is set according to the user’s preference, traffic flow is allowed only through the defined interface. | 
Use Cisco Feature Navigator to find information about platform and software image support. To access Cisco Feature Navigator, go to https://cfnng.cisco.com.