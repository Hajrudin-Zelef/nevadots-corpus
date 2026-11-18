The default configuration on most enterprise switches will work out-of-box as vendors tend to use a default switch port config of "trunk all, with native vlan 1". It is important to note that if connecting a Meraki MS switch to another vendor's switch, the other end of the link must be identically configured. If this is not the case, the link may not operate as expected due to VLAN or Native VLAN mismatch. Common misconfigurations include:
switchport mode
trunk encapsulation type (must be dot1q)
native VLAN mismatch
allowed VLAN mismatch
In the following scenario, we have a Cisco Meraki access switch uplinked to an other (non-Meraki) switch. The network administrator has configured the Cisco Meraki uplink port as trunk mode, native VLAN 1, allowed VLANs 1,10,20,30, and the non-Meraki switch to the left as its default configuration of trunk mode, native VLAN 1, allowed VLANs 1.
In this example, the PC user will not be able to reach the server on the left-hand side as the traffic being sent by the Meraki switch with a VLAN ID of 20 will not be accepted by its peer since it has not been configured to allow such traffic.
Cisco Meraki switch configuration example(assuming connection is on port 2)
Recommended Configurations
The following configuration examples outline how a non-Meraki peer switch's trunk link should be configured, to best operate with the example above.
Note: Dell PowerConnect switches have LLDP disabled and Transparency enabled by default. In order to make use of the topology feature in Dashboard, LLDP must be enabled and Transparency must be disabled. As the topology updates are not real-time, a delay should be expected.
Since Cisco Meraki VLAN tagging operates on the 802.1q tagging standard, any standard-compliant switch can be configured to operate in tandem with an MS switch. The best practice/examples outlined above should be used as a reference. For vendor-specific recommendations, refer to your switch vendor's documentation for 802.1q tagging and trunking.