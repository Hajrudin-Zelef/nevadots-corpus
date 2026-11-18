Information About 802.1x Port-Based Authentication
The 802.1x standard defines a client-server-based access control and authentication protocol that prevents unauthorized clients from connecting to a LAN through publicly accessible ports unless they are properly authenticated. The authentication server authenticates each client connected to a switch port before making available any services offered by the switch or the LAN.
| Note | TACACS is not supported with 802.1x authentication. | 
Until the client is authenticated, 802.1x access control allows only Extensible Authentication Protocol over LAN (EAPOL), Cisco Discovery Protocol (CDP), and Spanning Tree Protocol (STP) traffic through the port to which the client is connected. After authentication is successful, normal traffic can pass through the port.
The table shown below lists the maximum number of each client session supported on Catalyst 3850 and Catalyst 3650 switches:
| Client session | Maximum sessions supported | 
|---|---|
| Maximum dot1x or MAB client sessions | 2000 | 
| Maximum web-based authentication sessions | 2000 | 
| Maximum dot1x sessions with critical-auth VLAN enabled and server re-initialized | 2000 | 
| Maximum MAB sessions with various session features applied | 2000 | 
| Maximum dot1x sessions with service templates or session features applied | 2000 | 
Port-Based Authentication Process
To configure IEEE 802.1X port-based authentication, you must enable authentication, authorization, and accounting (AAA) and specify the authentication method list. A method list describes the sequence and authentication method to be queried to authenticate a user.
The AAA process begins with authentication. When 802.1x port-based authentication is enabled and the client supports 802.1x-compliant client software, these events occur:
-  
                                 		  
                                 If the client identity is valid and the 802.1x authentication succeeds, the switch grants the client access to the network.
-  
                                 		  
                                 If 802.1x authentication times out while waiting for an EAPOL message exchange and MAC authentication bypass is enabled, the switch can use the client MAC address for authorization. If the client MAC address is valid and the authorization succeeds, the switch grants the client access to the network. If the client MAC address is invalid and the authorization fails, the switch assigns the client to a guest VLAN that provides limited services if a guest VLAN is configured.
-  
                                 		  
                                 If the switch gets an invalid identity from an 802.1x-capable client and a restricted VLAN is specified, the switch can assign the client to a restricted VLAN that provides limited services.
-  
                                 		  
                                 If the RADIUS authentication server is unavailable (down) and inaccessible authentication bypass is enabled, the switch grants the client access to the network by putting the port in the critical-authentication state in the RADIUS-configured or the user-specified access VLAN. 
 Note
 Inaccessible authentication bypass is also referred to as critical authentication or the AAA fail policy. 
If Multi Domain Authentication (MDA) is enabled on a port, this flow can be used with some exceptions that are applicable to voice authorization.
The switch re-authenticates a client when one of these situations occurs:
-  
                                 		  
                                 Periodic re-authentication is enabled, and the re-authentication timer expires. You can configure the re-authentication timer to use a switch-specific value or to be based on values from the RADIUS server. After 802.1x authentication using a RADIUS server is configured, the switch uses timers based on the Session-Timeout RADIUS attribute (Attribute[27]) and the Termination-Action RADIUS attribute (Attribute [29]). The Session-Timeout RADIUS attribute (Attribute[27]) specifies the time after which re-authentication occurs. The range is 1 to 65535 seconds. The Termination-Action RADIUS attribute (Attribute [29]) specifies the action to take during re-authentication. The actions are Initialize and ReAuthenticate. When the Initialize action is set (the attribute value is DEFAULT), the 802.1x session ends, and connectivity is lost during re-authentication. When the ReAuthenticate action is set (the attribute value is RADIUS-Request), the session is not affected during re-authentication.
-  
                                 		  
                                 You manually re-authenticate the client by entering the dot1x re-authenticate interface interface-id privileged EXEC command.
Port-Based Authentication Initiation and Message Exchange
During 802.1x authentication, the switch or the client can initiate authentication. If you enable authentication on a port by using the authentication port-control auto interface configuration command, the switch initiates authentication when the link state changes from down to up or periodically as long as the port remains up and unauthenticated. The switch sends an EAP-request/identity frame to the client to request its identity. Upon receipt of the frame, the client responds with an EAP-response/identity frame.
However, if during bootup, the client does not receive an EAP-request/identity frame from the switch, the client can initiate authentication by sending an EAPOL-start frame, which prompts the switch to request the client’s identity.
| Note | If 802.1x authentication is not enabled or supported on the network access device, any EAPOL frames from the client are dropped. If the client does not receive an EAP-request/identity frame after three attempts to start authentication, the client sends frames as if the port is in the authorized state. A port in the authorized state effectively means that the client has been successfully authenticated. | 
When the client supplies its identity, the switch begins its role as the intermediary, passing EAP frames between the client and the authentication server until authentication succeeds or fails. If the authentication succeeds, the switch port becomes authorized. If the authentication fails, authentication can be retried, the port might be assigned to a VLAN that provides limited services, or network access is not granted.
The specific exchange of EAP frames depends on the authentication method being used.
If 802.1x authentication times out while waiting for an EAPOL message exchange and MAC authentication bypass is enabled, the switch can authorize the client when the switch detects an Ethernet packet from the client. The switch uses the MAC address of the client as its identity and includes this information in the RADIUS-access/request frame that is sent to the RADIUS server. After the server sends the switch the RADIUS-access/accept frame (authorization is successful), the port becomes authorized. If authorization fails and a guest VLAN is specified, the switch assigns the port to the guest VLAN. If the switch detects an EAPOL packet while waiting for an Ethernet packet, the switch stops the MAC authentication bypass process and starts 802.1x authentication.
Authentication Manager for Port-Based Authentication
Port-Based Authentication Methods
| Table 1. 802.1x Features |  |  |  |  | 
|---|---|---|---|---|
| Authentication method | Mode |  |  |  | 
|---|---|---|---|---|
|  | Single host | Multiple host | MDA | Multiple Authentication | 
| 802.1x | VLAN assignment Per-user ACL Filter-ID attribute Downloadable ACL Redirect URL | VLAN assignment | VLAN assignment Per-user ACL Filter-Id attribute Downloadable ACL Redirect URL | VLAN assignment Per-user ACL Filter-Id attribute Downloadable ACL Redirect URL | 
| MAC authentication bypass | VLAN assignment Per-user ACL Filter-ID attribute Downloadable ACL Redirect URL | VLAN assignment | VLAN assignment Per-user ACL Filter-Id attribute Downloadable ACL Redirect URL | VLAN assignment Per-user ACL Filter-Id attribute Downloadable ACL Redirect URL | 
| Standalone web authentication | Proxy ACL, Filter-Id attribute, downloadable ACL |  |  |  | 
| NAC Layer 2 IP validation | Filter-Id attribute Downloadable ACL Redirect URL | Filter-Id attribute Downloadable ACL Redirect URL | Filter-Id attribute Downloadable ACL Redirect URL | Filter-Id attribute Downloadable ACL Redirect URL | 
| Web authentication as fallback method | Proxy ACL Filter-Id attribute Downloadable ACL | Proxy ACL Filter-Id attribute Downloadable ACL | Proxy ACL Filter-Id attribute Downloadable ACL | Proxy ACL Filter-Id attribute Downloadable ACL | 
Per-User ACLs and Filter-Ids
| Note | Using role-based ACLs as Filter-Id is not recommended. | 
More than one host can be authenticated on MDA-enabled and multiauth ports. The ACL policy applied for one host does not effect the traffic of another host. If only one host is authenticated on a multi-host port, and the other hosts gain network access without authentication, the ACL policy for the first host can be applied to the other connected hosts by specifying any in the source address.
Port-Based Authentication Manager CLI Commands
The authentication-manager interface-configuration commands control all the authentication methods, such as 802.1x, MAC authentication bypass, and web authentication. The authentication manager commands determine the priority and order of authentication methods applied to a connected host.
The authentication manager commands control generic authentication features, such as host-mode, violation mode, and the authentication timer. Generic authentication commands include the authentication host-mode , authentication violation , and authentication timer interface configuration commands.
802.1x-specific commands begin with the dot1x keyword. For example, the authentication port-control auto interface configuration command enables authentication on an interface.
To disable dot1x on a switch, remove the configuration globally by using the no dot1x system-auth-control command, and also remove it from all configured interfaces.
| Note | If 802.1x authentication is globally disabled, other authentication methods are still enabled on that port, such as web authentication. To re-enable dot1x on the switch, you must configure both the dot1x global and interface configurations. Incomplete configurations can cause high CPU utilization. | 
The authentication manager commands provide the same functionality as earlier 802.1x commands.
When filtering out verbose system messages generated by the authentication manager, the filtered content typically relates to authentication success. You can also filter verbose messages for 802.1x authentication and MAB authentication. There is a separate command for each authentication method:
-  
                                       			 
                                       The no authentication logging verbose global configuration command filters verbose messages from the authentication manager.
-  
                                       			 
                                       The no dot1x logging verbose global configuration command filters 802.1x authentication verbose messages.
-  
                                       			 
                                       The no mab logging verbose global configuration command filters MAC authentication bypass (MAB) verbose messages
| Table 2. Authentication Manager                                           		  Commands and Earlier 802.1x Commands |  |  | 
|---|---|---|
| The authentication manager commands in Cisco IOS Release 12.2(50)SE or later | The equivalent 802.1x commands in Cisco IOS Release 12.2(46)SE and earlier | Description | 
|---|---|---|
| authentication control-direction {both \| in} | dot1x control-direction {both \| in} | Enable 802.1x authentication with the wake-on-LAN (WoL) feature, and configure the port control as unidirectional or bidirectional. | 
| authentication event | dot1x auth-fail vlan dot1x critical (interface configuration) dot1x guest-vlan6 | Enable the restricted VLAN on a port. Enable the inaccessible-authentication-bypass feature. Specify an active VLAN as an 802.1x guest VLAN. | 
| authentication fallback fallback-profile | dot1x fallback fallback-profile | Configure a port to use web authentication as a fallback method for clients that do not support 802.1x authentication. | 
| authentication host-mode [multi-auth \| multi-domain \| multi-host \| single-host] | dot1x host-mode {single-host \| multi-host \| multi-domain} | Allow a single host (client) or multiple hosts on an 802.1x-authorized port. | 
| authentication order | mab | Provides the flexibility to define the order of authentication methods to be used. | 
| authentication periodic | dot1x reauthentication | Enable periodic re-authentication of the client. | 
| authentication port-control {auto \| force-authorized \| force-un authorized} | dot1x port-control {auto \| force-authorized \| force-unauthorized} | Enable manual control of the authorization state of the port. | 
| authentication timer | dot1x timeout | Set the 802.1x timers. | 
| authentication violation {protect \| restrict \| shutdown} | dot1x violation-mode {shutdown \| restrict \| protect} | Configure the violation modes that occur when a new device connects to a port or when a new device connects to a port after the maximum number of devices are connected to that port. | 
Ports in Authorized and Unauthorized States
During 802.1x authentication, depending on the switch port state, the switch can grant a client access to the network. The port starts in the unauthorized state. While in this state, the port that is not configured as a voice VLAN port disallows all ingress and egress traffic except for 802.1x authentication, CDP, and STP packets. When a client is successfully authenticated, the port changes to the authorized state, allowing all traffic for the client to flow normally. If the port is configured as a voice VLAN port, the port allows VoIP traffic and 802.1x protocol packets before the client is successfully authenticated.
| Note | CDP bypass is not supported and may cause a port to go into err-disabled state. | 
If a client that does not support 802.1x authentication connects to an unauthorized 802.1x port, the switch requests the client’s identity. In this situation, the client does not respond to the request, the port remains in the unauthorized state, and the client is not granted access to the network.
In contrast, when an 802.1x-enabled client connects to a port that is not running the 802.1x standard, the client initiates the authentication process by sending the EAPOL-start frame. When no response is received, the client sends the request for a fixed number of times. Because no response is received, the client begins sending frames as if the port is in the authorized state.
You control the port authorization state by using the authentication port-control interface configuration command and these keywords:
-  
                                 		  
                                 force-authorized —disables 802.1x authentication and causes the port to change to the authorized state without any authentication exchange required. The port sends and receives normal traffic without 802.1x-based authentication of the client. This is the default setting.
-  
                                 		  
                                 force-unauthorized —causes the port to remain in the unauthorized state, ignoring all attempts by the client to authenticate. The switch cannot provide authentication services to the client through the port.
-  
                                 		  
                                 auto —enables 802.1x authentication and causes the port to begin in the unauthorized state, allowing only EAPOL frames to be sent and received through the port. The authentication process begins when the link state of the port changes from down to up or when an EAPOL-start frame is received. The switch requests the identity of the client and begins relaying authentication messages between the client and the authentication server. Each client attempting to access the network is uniquely identified by the switch by using the client MAC address.
If the client is successfully authenticated (receives an Accept frame from the authentication server), the port state changes to authorized, and all frames from the authenticated client are allowed through the port. If the authentication fails, the port remains in the unauthorized state, but authentication can be retried. If the authentication server cannot be reached, the switch can resend the request. If no response is received from the server after the specified number of attempts, authentication fails, and network access is not granted.
When a client logs off, it sends an EAPOL-logoff message, causing the switch port to change to the unauthorized state.
If the link state of a port changes from up to down, or if an EAPOL-logoff frame is received, the port returns to the unauthorized state.
Port-Based Authentication and Switch Stacks
If a switch is added to or removed from a switch stack, 802.1x authentication is not affected as long as the IP connectivity between the RADIUS server and the stack remains intact. This statement also applies if the stack's active switch is removed from the switch stack. Note that if the active switch fails, a stack member becomes the new active switch of the stack by using the election process, and the 802.1x authentication process continues as usual.
If IP connectivity to the RADIUS server is interrupted because the switch that was connected to the server is removed or fails, these events occur:
-  
                                 		  
                                 Ports that are already authenticated and that do not have periodic re-authentication enabled remain in the authenticated state. Communication with the RADIUS server is not required.
-  
                                 		  
                                 Ports that are already authenticated and that have periodic re-authentication enabled (with the dot1x re-authentication global configuration command) fail the authentication process when the re-authentication occurs. Ports return to the unauthenticated state during the re-authentication process. Communication with the RADIUS server is required. For an ongoing authentication, the authentication fails immediately because there is no server connectivity.
If the switch that failed comes up and rejoins the switch stack, the authentications might or might not fail depending on the boot-up time and whether the connectivity to the RADIUS server is re-established by the time the authentication is attempted.
To avoid loss of connectivity to the RADIUS server, you should ensure that there is a redundant connection to it. For example, you can have a redundant connection to the stack's active switch and another to a stack member, and if the active switch fails, the switch stack still has connectivity to the RADIUS server.
802.1x Host Mode
You can configure an 802.1x port for single-host or for multiple-hosts mode. In single-host mode, only one client can be connected to the 802.1x-enabled switch port. The switch detects the client by sending an EAPOL frame when the port link state changes to the up state. If a client leaves or is replaced with another client, the switch changes the port link state to down, and the port returns to the unauthorized state.
In multiple-hosts mode, you can attach multiple hosts to a single 802.1x-enabled port. In this mode, only one of the attached clients must be authorized for all clients to be granted network access. If the port becomes unauthorized (re-authentication fails or an EAPOL-logoff message is received), the switch denies network access to all of the attached clients.
The switch supports multidomain authentication (MDA), which allows both a data device and a voice device, such as an IP Phone (Cisco or non-Cisco), to connect to the same switch port.
802.1x Multiple Authentication Mode
Multiple-authentication (multiauth) mode allows multiple authenticated clients on the data VLAN and voice VLAN. Each host is individually authenticated. There is no limit to the number of data or voice device that can be authenticated on a multiauthport.
| Note | When a port is in multiple-authentication mode, the authentication-failed VLAN features do not activate. | 
You can assign a RADIUS-server-supplied VLAN in multi-auth mode, under the following conditions:
- 
                                 				
                                 The host is the first host authorized on the port, and the RADIUS server supplies VLAN information
- 
                                 				
                                 Subsequent hosts are authorized with a VLAN that matches the operational VLAN.
- 
                                 				
                                 A host is authorized on the port with no VLAN assignment, and subsequent hosts either have no VLAN assignment, or their VLAN information matches the operational VLAN.
- 
                                 				
                                 The first host authorized on the port has a group VLAN assignment, and subsequent hosts either have no VLAN assignment, or their group VLAN matches the group VLAN on the port. Subsequent hosts must use the same VLAN from the VLAN group as the first host. If a VLAN list is used, all hosts are subject to the conditions specified in the VLAN list.
- 
                                 				
                                 After a VLAN is assigned to a host on the port, subsequent hosts must have matching VLAN information or be denied access to the port.
- 
                                 				
                                 You cannot configure a guest VLAN or an auth-fail VLAN in multi-auth mode.
- 
                                 				
                                 The behavior of the critical-auth VLAN is not changed for multi-auth mode. When a host tries to authenticate and the server is not reachable, all authorized hosts are reinitialized in the configured VLAN.
Multi-auth Per User VLAN assignment
The Multi-auth Per User VLAN assignment feature allows you to create multiple operational access VLANs based on VLANs assigned to the clients on the port that has a single configured access VLAN. The port configured as an access port where the traffic for all the VLANs associated with data domain is not dot1q tagged, and these VLANs are treated as native VLANs.
The number of hosts per multi-auth port is 8, however there can be more hosts.
The following scenarios are associated with the multi-auth Per User VLAN assignments:
Scenario one
When a hub is connected to an access port, and the port is configured with an access VLAN (V0).
The host (H1) is assigned to VLAN (V1) through the hub. The operational VLAN of the port is changed to V1. This behaviour is similar on a single-host or multi-domain-auth port.
When a second host (H2) is connected and gets assigned to VLAN ( V2), the port will have two operational VLANs (V1 and V2). If H1 and H2 sends untagged ingress traffic, H1 traffic is mapped to VLAN (V1) and H2 traffic to VLAN (V2), all egress traffic going out of the port on VLAN (V1) and VLAN (V2) are untagged.
If both the hosts, H1 and H2 are logged out or the sessions are removed due to some reason then VLAN (V1) and VLAN (V2) are removed from the port, and the configured VLAN (V0) is restored on the port.
Scenario two
When a hub is connected to an access port, and the port is configured with an access VLAN (V0). The host (H1) is assigned to VLAN (V1) through the hub. The operational VLAN of the port is changed to V1.
When a second host (H2) is connected and gets authorized without explicit vlan policy, H2 is expected to use the configured VLAN (V0) that is restored on the port. A ll egress traffic going out of two operational VLANs, VLAN (V0) and VLAN (V1) are untagged.
If host (H2 ) is logged out or the session is removed due to some reason then the configured VLAN (V0) is removed from the port, and VLAN (V1) becomes the only operational VLAN on the port.
Scenario three
When a hub is connected to an access port in open mode, and the port is configured with an access VLAN (V0) .
The host (H1) is assigned to VLAN (V1) through the hub. The operational VLAN of the port is changed to V1. When a second host (H2) is connected and remains unauthorized, it still has access to operational VLAN (V1) due to open mode.
If host H1 is logged out or the session is removed due to some reason, VLAN (V1) is removed from the port and host (H2) gets assigned to VLAN (V0).
| Note | The combination of Open mode and VLAN assignment has an adverse affect on host (H2) because it has an IP address in the subnet that corresponds to VLAN (V1). | 
Limitation in Multi-auth Per User VLAN assignment
In the Multi-auth Per User VLAN assignment feature, egress traffic from multiple vlans are untagged on a port where the hosts receive traffic that is not meant for them. This can be a problem with broadcast and multicast traffic.
- 
                                       		  
                                       IPv4 ARPs: Hosts receive ARP packets from other subnets. This is a problem if two subnets in different Virtual Routing and Forwarding (VRF) tables with overlapping IP address range are active on the port. The host ARP cache may get invalid entries.
- 
                                       				
                                       IPv6 control packets: In IPv6 deployments, Router Advertisements (RA) are processed by hosts that are not supposed to receive them. When a host from one VLAN receives RA from a different VLAN, the host assign incorrect IPv6 address to itself. Such a host is unable to get access to the network. The workaround is to enable the IPv6 first hop security so that the broadcast ICMPv6 packets are converted to unicast and sent out from multi-auth enabled ports.. The packet is replicated for each client in multi-auth port belonging to the VLAN and the destination MAC is set to an individual client. Ports having one VLAN, ICMPv6 packets broadcast normally.
- 
                                       		  
                                       IP multicast: Multicast traffic destined to a multicast group gets replicated for different VLANs if the hosts on those VLANs join the multicast group. When two hosts in different VLANs join a multicast group (on the same mutli-auth port), two copies of each multicast packet are sent out from that port.
MAC Move
When a MAC address is authenticated on one switch port, that address is not allowed on another authentication manager-enabled port of the switch. If the switch detects that same MAC address on another authentication manager-enabled port, the address is not allowed.
There are situations where a MAC address might need to move from one port to another on the same switch. For example, when there is another device (for example a hub or an IP phone) between an authenticated host and a switch port, you might want to disconnect the host from the device and connect it directly to another port on the same switch.
You can globally enable MAC move so the device is reauthenticated on the new port. When a host moves to a second port, the session on the first port is deleted, and the host is reauthenticated on the new port. MAC move is supported on all host modes. (The authenticated host can move to any port on the switch, no matter which host mode is enabled on the that port.) When a MAC address moves from one port to another, the switch terminates the authenticated session on the original port and initiates a new authentication sequence on the new port. The MAC move feature applies to both voice and data hosts.
| Note | In open authentication mode, a MAC address is immediately moved from the original port to the new port, with no requirement for authorization on the new port. | 
MAC Replace
The MAC replace feature can be configured to address the violation that occurs when a host attempts to connect to a port where another host was previously authenticated.
| Note | This feature does not apply to ports in multi-auth mode, because violations are not triggered in that mode. It does not apply to ports in multiple host mode, because in that mode, only the first host requires authentication. | 
If you configure the authentication violation interface configuration command with the replace keyword, the authentication process on a port in multi-domain mode is:
- 
                                 		  
                                 A new MAC address is received on a port with an existing authenticated MAC address.
- 
                                 		  
                                 The authentication manager replaces the MAC address of the current data host on the port with the new MAC address.
- 
                                 		  
                                 The authentication manager initiates the authentication process for the new MAC address.
- 
                                 		  
                                 If the authentication manager determines that the new host is a voice host, the original voice host is removed.
If a port is in open authentication mode, any new MAC address is immediately added to the MAC address table.
802.1x Accounting
The 802.1x standard defines how users are authorized and authenticated for network access but does not keep track of network usage. 802.1x accounting is disabled by default. You can enable 802.1x accounting to monitor this activity on 802.1x-enabled ports:
- 
                                 User successfully authenticates.
- 
                                 User logs off.
- 
                                 Link-down occurs.
- 
                                 Re-authentication successfully occurs.
- 
                                 Re-authentication fails.
The switch does not log 802.1x accounting information. Instead, it sends this information to the RADIUS server, which must be configured to log accounting messages.
802.1x Accounting Attribute-Value Pairs
The information sent to the RADIUS server is represented in the form of Attribute-Value (AV) pairs. These AV pairs provide data for different applications. (For example, a billing application might require information that is in the Acct-Input-Octets or the Acct-Output-Octets attributes of a RADIUS packet.)
AV pairs are automatically sent by a switch that is configured for 802.1x accounting. Three types of RADIUS accounting packets are sent by a switch:
-  
                                    			 
                                    START–sent when a new user session starts
-  
                                    			 
                                    INTERIM–sent during an existing session for updates
-  
                                    			 
                                    STOP–sent when a session terminates
| Note | To view debug logs for RADIUS and AAA, use the show platform software trace message smd command. For more information, see the Tracing Commands section in Command Reference Guide, Cisco IOS XE Denali 16.1.1. | 
This table lists the AV pairs and when they are sent are sent by the switch.
| Table 3. Accounting AV Pairs |  |  |  |  | 
|---|---|---|---|---|
| Attribute Number | AV Pair Name | START | INTERIM | STOP | 
|---|---|---|---|---|
| Attribute[1] | User-Name | Always | Always | Always | 
| Attribute[4] | NAS-IP-Address | Always | Always | Always | 
| Attribute[5] | NAS-Port | Always | Always | Always | 
| Attribute[8] | Framed-IP-Address | Never | Sometimes3 | Sometimes | 
| Attribute[30] | Called-Station-ID | Always | Always | Always | 
| Attribute[31] | Calling-Station-ID | Always | Always | Always | 
| Attribute[40] | Acct-Status-Type | Always | Always | Always | 
| Attribute[41] | Acct-Delay-Time | Always | Always | Always | 
| Attribute[42] | Acct-Input-Octets | Never | Always | Always | 
| Attribute[43] | Acct-Output-Octets | Never | Always | Always | 
| Attribute[47] | Acct-Input-Packets | Never | Always | Always | 
| Attribute[48] | Acct-Output-Packets | Never | Always | Always | 
| Attribute[44] | Acct-Session-ID | Always | Always | Always | 
| Attribute[45] | Acct-Authentic | Always | Always | Always | 
| Attribute[46] | Acct-Session-Time | Never | Always | Always | 
| Attribute[49] | Acct-Terminate-Cause | Never | Never | Always | 
| Attribute[61] | NAS-Port-Type | Always | Always | Always | 
802.1x Readiness Check
The 802.1x readiness check monitors 802.1x activity on all the switch ports and displays information about the devices connected to the ports that support 802.1x. You can use this feature to determine if the devices connected to the switch ports are 802.1x-capable. You use an alternate authentication such as MAC authentication bypass or web authentication for the devices that do not support 802.1x functionality.
This feature only works if the supplicant on the client supports a query with the NOTIFY EAP notification packet. The client must respond within the 802.1x timeout value.
Switch-to-RADIUS-Server Communication
RADIUS security servers are identified by their hostname or IP address, hostname and specific UDP port numbers, or IP address and specific UDP port numbers. The combination of the IP address and UDP port number creates a unique identifier, which enables RADIUS requests to be sent to multiple UDP ports on a server at the same IP address. If two different host entries on the same RADIUS server are configured for the same service—for example, authentication—the second host entry configured acts as the fail-over backup to the first one. The RADIUS host entries are tried in the order that they were configured.
802.1x Authentication with VLAN Assignment
The switch supports 802.1x authentication with VLAN assignment. After successful 802.1x authentication of a port, the RADIUS server sends the VLAN assignment to configure the switch port. The RADIUS server database maintains the username-to-VLAN mappings, assigning the VLAN based on the username of the client connected to the switch port. You can use this feature to limit network access for certain users.
Voice device authentication is supported with multidomain host mode in Cisco IOS Release 12.2(37)SE. In Cisco IOS Release 12.2(40)SE and later, when a voice device is authorized and the RADIUS server returned an authorized VLAN, the voice VLAN on the port is configured to send and receive packets on the assigned voice VLAN. Voice VLAN assignment behaves the same as data VLAN assignment on multidomain authentication (MDA)-enabled ports.
When configured on the switch and the RADIUS server, 802.1x authentication with VLAN assignment has these characteristics:
- 
                                 		  
                                 If no VLAN is supplied by the RADIUS server or if 802.1x authentication is disabled, the port is configured in its access VLAN after successful authentication. Recall that an access VLAN is a VLAN assigned to an access port. All packets sent from or received on this port belong to this VLAN.
- 
                                 		  
                                 If 802.1x authentication is enabled but the VLAN information from the RADIUS server is not valid, authorization fails and configured VLAN remains in use. This prevents ports from appearing unexpectedly in an inappropriate VLAN because of a configuration error. Configuration errors could include specifying a VLAN for a routed port, a malformed VLAN ID, a nonexistent or internal (routed port) VLAN ID, an RSPAN VLAN, a shut down or suspended VLAN. In the case of a multidomain host port, configuration errors can also be due to an attempted assignment of a data VLAN that matches the configured or assigned voice VLAN ID (or the reverse).
- 
                                 		  
                                 If 802.1x authentication is enabled and all information from the RADIUS server is valid, the authorized device is placed in the specified VLAN after authentication.
- 
                                 		  
                                 If the multiple-hosts mode is enabled on an 802.1x port, all hosts are placed in the same VLAN (specified by the RADIUS server) as the first authenticated host.
- 
                                 		  
                                 Enabling port security does not impact the RADIUS server-assigned VLAN behavior.
- 
                                 		  
                                 If 802.1x authentication is disabled on the port, it is returned to the configured access VLAN and configured voice VLAN.
- 
                                 				
                                 If an 802.1x port is authenticated and put in the RADIUS server-assigned VLAN, any change to the port access VLAN configuration does not take effect. In the case of a multidomain host, the same applies to voice devices when the port is fully authorized with these exceptions: 
  - 
                                       						
                                       If the VLAN configuration change of one device results in matching the other device configured or assigned VLAN, then authorization of all devices on the port is terminated and multidomain host mode is disabled until a valid configuration is restored where data and voice device configured VLANs no longer match.
  - 
                                       						
                                       If a voice device is authorized and is using a downloaded voice VLAN, the removal of the voice VLAN configuration, or modifying the configuration value to dot1p or untagged results in voice device un-authorization and the disablement of multi-domain host mode.
- 
                                       						
                                       
When the port is in the force authorized, force unauthorized, unauthorized, or shutdown state, it is put into the configured access VLAN.
To configure VLAN assignment you need to perform these tasks:
- 
                                 				
                                 Enable AAA authorization by using the network keyword to allow interface configuration from the RADIUS server.
- 
                                 				
                                 Enable 802.1x authentication. (The VLAN assignment feature is automatically enabled when you configure 802.1x authentication on an access port).
- 
                                 				
                                 Assign vendor-specific tunnel attributes in the RADIUS server. The RADIUS server must return these attributes to the switch: 
  - 
                                       						
                                       [64] Tunnel-Type = VLAN
  - 
                                       						
                                       [65] Tunnel-Medium-Type = 802
  - 
                                       						
                                       [81] Tunnel-Private-Group-ID = VLAN name or VLAN ID
  - 
                                       						
                                       [83] Tunnel-Preference
 Attribute [64] must contain the value VLAN (type 13). Attribute [65] must contain the value 802 (type 6). Attribute [81] specifies the VLAN name or VLAN ID assigned to the IEEE 802.1x-authenticated user.
- 
                                       						
                                       
802.1x Authentication with Per-User ACLs
You can enable per-user access control lists (ACLs) to provide different levels of network access and service to an 802.1x-authenticated user. When the RADIUS server authenticates a user connected to an 802.1x port, it retrieves the ACL attributes based on the user identity and sends them to the switch. The switch applies the attributes to the 802.1x port for the duration of the user session. The switch removes the per-user ACL configuration when the session is over, if authentication fails, or if a link-down condition occurs. The switch does not save RADIUS-specified ACLs in the running configuration. When the port is unauthorized, the switch removes the ACL from the port.
You can configure router ACLs and input port ACLs on the same switch. However, a port ACL takes precedence over a router ACL. If you apply input port ACL to an interface that belongs to a VLAN, the port ACL takes precedence over an input router ACL applied to the VLAN interface. Incoming packets received on the port, to which a port ACL is applied, are filtered by the port ACL. Incoming routed packets received on other ports are filtered by the router ACL. Outgoing routed packets are filtered by the router ACL. To avoid configuration conflicts, you should carefully plan the user profiles stored on the RADIUS server.
RADIUS supports per-user attributes, including vendor-specific attributes. These vendor-specific attributes (VSAs) are in octet-string format and are passed to the switch during the authentication process. The VSAs used for per-user ACLs are inacl#<n> for the ingress direction and outacl#<n> for the egress direction. MAC ACLs are supported only in the ingress direction. The switch supports VSAs only in the ingress direction. It does not support port ACLs in the egress direction on Layer 2 ports.
Use only the extended ACL syntax style to define the per-user configuration stored on the RADIUS server. When the definitions are passed from the RADIUS server, they are created by using the extended naming convention. However, if you use the Filter-Id attribute, it can point to a standard ACL.
You can use the Filter-Id attribute to specify an inbound or outbound ACL that is already configured on the switch. The attribute contains the ACL number followed by .in for ingress filtering or .out for egress filtering. If the RADIUS server does not allow the .in or .out syntax, the access list is applied to the outbound ACL by default. The user is marked unauthorized if the Filter-Id sent from the RADIUS server is not configured on the device. Because of limited support of Cisco IOS access lists on the switch, the Filter-Id attribute is supported only for IP ACLs numbered in the range of 1 to 199 (IP standard ACLs) and 1300 to 2699 (IP extended ACLs).
The maximum size of the per-user ACL is 4000 ASCII characters but is limited by the maximum size of RADIUS-server per-user ACLs.
You must meet the following prerequisites to configure per-user ACLs:
-  
                                 		  
                                 Enable AAA authentication.
-  
                                 		  
                                 Enable AAA authorization by using the network keyword to allow interface configuration from the RADIUS server.
-  
                                 		  
                                 Enable 802.1x authentication.
-  
                                 		  
                                 Configure the user profile and VSAs on the RADIUS server.
-  
                                 		  
                                 Configure the 802.1x port for single-host mode. 
 Note
 Per-user ACLs are supported only in single-host mode. 
802.1x Authentication with Downloadable ACLs and Redirect URLs
You can download ACLs and redirect URLs from a RADIUS server to the switch during 802.1x authentication or MAC authentication bypass of the host. You can also download ACLs during web authentication.
| Note | A downloadable ACL is also referred to as a dACL. | 
If more than one host is authenticated and the host is in single-host, MDA, or multiple-authentication mode, the switch changes the source address of the ACL to the host IP address.
You can apply the ACLs and redirect URLs to all the devices connected to the 802.1x-enabled port.
If no ACLs are downloaded during 802.1x authentication, the switch applies the static default ACL on the port to the host. On a voice VLAN port configured in multi-auth or MDA mode, the switch applies the ACL only to the phone as part of the authorization policies.
| Note | The limit for dACL with stacking is 64 ACEs per dACL per port. The limit without stacking is the number of available TCAM entries which varies based on the other ACL features that are active. | 
For a URL redirect ACL:
- 
                                 				
                                 Packets that match a permit access control entry (ACE) rule are sent to the CPU for forwarding to the AAA server.
- 
                                 				
                                 Packets that match a deny ACE rule are forwarded through the switch.
- 
                                 				
                                 Packets that match neither the permit ACE rule or deny ACE rule are processed by the next dACL, and if there is no dACL, the packets hit the implicit-deny ACL and are dropped.
Cisco Secure ACS and Attribute-Value Pairs for the Redirect URL
The switch uses these cisco-av-pair VSAs:
- 
                                    		  
                                    url-redirect is the HTTP or HTTPS URL.
- 
                                    		  
                                    url-redirect-acl is the switch ACL name or number.
The switch uses the CiscoSecure-defined-ACL attribute value pair to intercept an HTTP or HTTPS request from the end point. The switch then forwards the client web browser to the specified redirect address. The url-redirect AV pair on the Cisco Secure ACS contains the URL to which the web browser is redirected. The url-redirect-acl attribute value pair contains the name or number of an ACL that specifies the HTTP or HTTPS traffic to redirect.
| Note |  | 
If a redirect URL is configured for a client on the authentication server, a default port ACL on the connected client switch port must also be configured.
When security ACL/dACL and punt/redirect ACLs are applied together to the session, the url-redirect-acl has the higher priority.
For more information about using redirect ACLs, refer the document here.
Cisco Secure ACS and Attribute-Value Pairs for Downloadable ACLs
You can set the CiscoSecure-Defined-ACL Attribute-Value (AV) pair on the Cisco Secure ACS with the RADIUS cisco-av-pair vendor-specific attributes (VSAs). This pair specifies the names of the downloadable ACLs on the Cisco Secure ACS with the #ACL#-IP-name-number attribute.
- 
                                    The name is the ACL name.
- 
                                    The number is the version number (for example, 3f783768).
If a downloadable ACL is configured for a client on the authentication server, a default port ACL on the connected client switch port must also be configured.
If the default ACL is configured on the switch and the Cisco Secure ACS sends a host-access-policy to the switch, it applies the policy to traffic from the host connected to a switch port. If the policy does not apply, the switch applies the default ACL. If the Cisco Secure ACS sends the switch a downloadable ACL, this ACL takes precedence over the default ACL that is configured on the switch port. However, if the switch receives an host access policy from the Cisco Secure ACS but the default ACL is not configured, the authorization failure is declared.
VLAN ID-Based MAC Authentication
You can use VLAN ID-based MAC authentication if you wish to authenticate hosts based on a static VLAN ID instead of a downloadable VLAN. When you have a static VLAN policy configured on your switch, VLAN information is sent to an IAS (Microsoft) RADIUS server along with the MAC address of each host for authentication. The VLAN ID configured on the connected port is used for MAC authentication. By using VLAN ID-based MAC authentication with an IAS server, you can have a fixed number of VLANs in the network.
The feature also limits the number of VLANs monitored and handled by STP. The network can be managed as a fixed VLAN.
802.1x Authentication with Guest VLAN
You can configure a guest VLAN for each 802.1x port on the switch to provide limited services to clients, such as downloading the 802.1x client. These clients might be upgrading their system for 802.1x authentication, and some hosts, such as Windows 98 systems, might not be IEEE 802.1x-capable.
When you enable a guest VLAN on an 802.1x port, the switch assigns clients to a guest VLAN when the switch does not receive a response to its EAP request/identity frame or when EAPOL packets are not sent by the client.
The switch maintains the EAPOL packet history. If an EAPOL packet is detected on the interface during the lifetime of the link, the switch determines that the device connected to that interface is an IEEE 802.1x-capable supplicant, and the interface does not change to the guest VLAN state. EAPOL history is cleared if the interface link status goes down. If no EAPOL packet is detected on the interface, the interface changes to the guest VLAN state.
If the switch is trying to authorize an 802.1x-capable voice device and the AAA server is unavailable, the authorization attempt fails, but the detection of the EAPOL packet is saved in the EAPOL history. When the AAA server becomes available, the switch authorizes the voice device. However, the switch no longer allows other devices access to the guest VLAN. To prevent this situation, use one of these command sequences:
- 
                                 		  
                                 Enter the authentication event no-response action authorize vlan vlan-id interface configuration command to allow access to the guest VLAN.
- 
                                 		  
                                 Enter the shutdown interface configuration command followed by the no shutdown interface configuration command to restart the port.
If devices send EAPOL packets to the switch during the lifetime of the link, the switch no longer allows clients that fail authentication access to the guest VLAN.
| Note | If an EAPOL packet is detected after the interface has changed to the guest VLAN, the interface reverts to an unauthorized state, and 802.1x authentication restarts. | 
Any number of 802.1x-incapable clients are allowed access when the switch port is moved to the guest VLAN. If an 802.1x-capable client joins the same port on which the guest VLAN is configured, the port is put into the unauthorized state in the user-configured access VLAN, and authentication is restarted.
Guest VLANs are supported on 802.1x ports in single host, multiple host, multi-auth and multi-domain modes.
You can configure any active VLAN except an RSPAN VLAN, a private VLAN, or a voice VLAN as an 802.1x guest VLAN. The guest VLAN feature is not supported on internal VLANs (routed ports) or trunk ports; it is supported only on access ports.
The switch supports MAC authentication bypass. When MAC authentication bypass is enabled on an 802.1x port, the switch can authorize clients based on the client MAC address when IEEE 802.1x authentication times out while waiting for an EAPOL message exchange. After detecting a client on an 802.1x port, the switch waits for an Ethernet packet from the client. The switch sends the authentication server a RADIUS-access/request frame with a username and password based on the MAC address. If authorization succeeds, the switch grants the client access to the network. If authorization fails, the switch assigns the port to the guest VLAN if one is specified.
802.1x Authentication with Restricted VLAN
You can configure a restricted VLAN (also referred to as an authentication failed VLAN) for each IEEE 802.1x port on a switch stack or a switch to provide limited services to clients that cannot access the guest VLAN. These clients are 802.1x-compliant and cannot access another VLAN because they fail the authentication process. A restricted VLAN allows users without valid credentials in an authentication server (typically, visitors to an enterprise) to access a limited set of services. The administrator can control the services available to the restricted VLAN.
| Note | You can configure a VLAN to be both the guest VLAN and the restricted VLAN if you want to provide the same services to both types of users. | 
Without this feature, the client attempts and fails authentication indefinitely, and the switch port remains in the spanning-tree blocking state. With this feature, you can configure the switch port to be in the restricted VLAN after a specified number of authentication attempts (the default value is 3 attempts).
The authenticator counts the failed authentication attempts for the client. When this count exceeds the configured maximum number of authentication attempts, the port moves to the restricted VLAN. The failed attempt count increments when the RADIUS server replies with either an EAP failure or an empty response without an EAP packet. When the port moves into the restricted VLAN, the failed attempt counter resets.
Users who fail authentication remain in the restricted VLAN until the next re-authentication attempt. A port in the restricted VLAN tries to re-authenticate at configured intervals (the default is 60 seconds). If re-authentication fails, the port remains in the restricted VLAN. If re-authentication is successful, the port moves either to the configured VLAN or to a VLAN sent by the RADIUS server. You can disable re-authentication. If you do this, the only way to restart the authentication process is for the port to receive a link down or EAP logoff event. We recommend that you keep re-authentication enabled if a client might connect through a hub. When a client disconnects from the hub, the port might not receive the link down or EAP logoff event.
After a port moves to the restricted VLAN, a simulated EAP success message is sent to the client. This prevents clients from indefinitely attempting authentication. Some clients (for example, devices running Windows XP) cannot implement DHCP without EAP success.
Restricted VLANs are supported on 802.1x ports in all host modes and on Layer 2 ports.
You can configure any active VLAN except an RSPAN VLAN, a primary private VLAN, or a voice VLAN as an 802.1x restricted VLAN. The restricted VLAN feature is not supported on internal VLANs (routed ports) or trunk ports; it is supported only on access ports.
Other security port features such as dynamic ARP Inspection, DHCP snooping, and IP source guard can be configured independently on a restricted VLAN.
802.1x Authentication with Inaccessible Authentication Bypass
Use the inaccessible authentication bypass feature, also referred to as critical authentication or the AAA fail policy, when the switch cannot reach the configured RADIUS servers and new hosts cannot be authenticated. You can configure the switch to connect those hosts to critical ports.
When a new host tries to connect to the critical port, that host is moved to a user-specified access VLAN, the critical VLAN. The administrator gives limited authentication to the hosts.
When the switch tries to authenticate a host connected to a critical port, the switch checks the status of the configured RADIUS server. If a server is available, the switch can authenticate the host. However, if all the RADIUS servers are unavailable, the switch grants network access to the host and puts the port in the critical-authentication state, which is a special case of the authentication state.
| Note | If critical authentication is configured on interface, then vlan used for critical authorization (critical vlan) should be active on the switch. If the critical vlan is inactive (or) down, critical authentication session will keep trying to enable inactive vlan and fail repeatedly. This can lead to large amount of memory holding. | 
Inaccessible Authentication Bypass Support on Multiple-Authentication Ports
When a port is configured on any host mode and the AAA server is unavailable, the port is then configured to multi-host mode and moved to the critical VLAN. To support this inaccessible bypass on multiple-authentication (multiauth) ports, use the authentication event server dead action reinitialize vlan vlan-id command. When a new host tries to connect to the critical port, that port is reinitialized and all the connected hosts are moved to the user-specified access VLAN.
This command is supported on all host modes.
Inaccessible Authentication Bypass Authentication Results
The behavior of the inaccessible authentication bypass feature depends on the authorization state of the port:
- 
                                    If the port is unauthorized when a host connected to a critical port tries to authenticate and all servers are unavailable, the switch puts the port in the critical-authentication state in the RADIUS-configured or user-specified access VLAN.
- 
                                    If the port is already authorized and reauthentication occurs, the switch puts the critical port in the critical-authentication state in the current VLAN, which might be the one previously assigned by the RADIUS server.
- 
                                    If the RADIUS server becomes unavailable during an authentication exchange, the current exchange times out, and the switch puts the critical port in the critical-authentication state during the next authentication attempt.
You can configure the critical port to reinitialize hosts and move them out of the critical VLAN when the RADIUS server is again available. When this is configured, all critical ports in the critical-authentication state are automatically re-authenticated.
Inaccessible Authentication Bypass Feature Interactions
Inaccessible authentication bypass interacts with these features:
-  
                                    		  
                                    Guest VLAN—Inaccessible authentication bypass is compatible with guest VLAN. When a guest VLAN is enabled on 8021.x port, the features interact as follows: 
  -  
                                          				
                                          If at least one RADIUS server is available, the switch assigns a client to a guest VLAN when the switch does not receive a response to its EAP request/identity frame or when EAPOL packets are not sent by the client.
  -  
                                          				
                                          If all the RADIUS servers are not available and the client is connected to a critical port, the switch authenticates the client and puts the critical port in the critical-authentication state in the RADIUS-configured or user-specified access VLAN.
  -  
                                          				
                                          If all the RADIUS servers are not available and the client is not connected to a critical port, the switch might not assign clients to the guest VLAN if one is configured.
  -  
                                          				
                                          If all the RADIUS servers are not available and if a client is connected to a critical port and was previously assigned to a guest VLAN, the switch keeps the port in the guest VLAN.
-  
                                          				
                                          
-  
                                    		  
                                    Restricted VLAN—If the port is already authorized in a restricted VLAN and the RADIUS servers are unavailable, the switch puts the critical port in the critical-authentication state in the restricted VLAN.
-  
                                    		  
                                    802.1x accounting—Accounting is not affected if the RADIUS servers are unavailable.
- 
                                    				
                                    Private VLAN—You can configure inaccessible authentication bypass on a private VLAN host port. The access VLAN must be a secondary private VLAN.
-  
                                    		  
                                    Voice VLAN—Inaccessible authentication bypass is compatible with voice VLAN, but the RADIUS-configured or user-specified access VLAN and the voice VLAN must be different.
-  
                                    		  
                                    Remote Switched Port Analyzer (RSPAN)—Do not configure an RSPAN VLAN as the RADIUS-configured or user-specified access VLAN for inaccessible authentication bypass.
In a switch stack:
- 
                                    				
                                    The stack's active switch checks the status of the RADIUS servers by sending keepalive packets. When the status of a RADIUS server changes, the stack's active switch sends the information to the stack members. The stack members can then check the status of RADIUS servers when re-authenticating critical ports.
- 
                                    				
                                    If the new active switch is elected, the link between the switch stack and RADIUS server might change, and the new stack immediately sends keepalive packets to update the status of the RADIUS servers. If the server status changes from dead to alive, the switch re-authenticates all switch ports in the critical-authentication state.
When a member is added to the stack, the stack's active switch sends the member the server status.
802.1x Critical Voice VLAN
When an IP phone connected to a port is authenticated by the Cisco Identity Services Engine (ISE), the phone is put into the voice domain. If the ISE is not reachable, the switch cannot determine if the device is a voice device. If the server is unavailable, the phone cannot access the voice network and therefore cannot operate.
For data traffic, you can configure inaccessible authentication bypass, or critical authentication, to allow traffic to pass through on the native VLAN when the server is not available. If the RADIUS authentication server is unavailable (down) and inaccessible authentication bypass is enabled, the switch grants the client access to the network and puts the port in the critical-authentication state in the RADIUS-configured or the user-specified access VLAN. When the switch cannot reach the configured RADIUS servers and new hosts cannot be authenticated, the switch connects those hosts to critical ports. A new host trying to connect to the critical port is moved to a user-specified access VLAN, the critical VLAN, and granted limited authentication.
| Note | Dynamic assignment of critical voice VLAN is not supported with nested service templates. It causes the device to switch between VLANs continuously in a loop. | 
You can enter the authentication event server dead action authorize voice interface configuration command to configure the critical voice VLAN feature. When the ISE does not respond, the port goes into critical authentication mode. When traffic coming from the host is tagged with the voice VLAN, the connected device (the phone) is put in the configured voice VLAN for the port. The IP phones learn the voice VLAN identification through Cisco Discovery Protocol (Cisco devices) or through LLDP or DHCP.
You can configure the voice VLAN for a port by entering the switchport voice vlan vlan-id interface configuration command.
This feature is supported in multidomain and multi-auth host modes. Although you can enter the command when the switch in single-host or multi-host mode, the command has no effect unless the device changes to multidomain or multi-auth host mode.
802.1x User Distribution
You can configure 802.1x user distribution to load-balance users with the same group name across multiple different VLANs.
The VLANs are either supplied by the RADIUS server or configured through the switch CLI under a VLAN group name.
- 
                                 Configure the RADIUS server to send more than one VLAN name for a user. The multiple VLAN names can be sent as part of the response to the user. The 802.1x user distribution tracks all the users in a particular VLAN and achieves load balancing by moving the authorized user to the least populated VLAN.
- 
                                 Configure the RADIUS server to send a VLAN group name for a user. The VLAN group name can be sent as part of the response to the user. You can search for the selected VLAN group name among the VLAN group names that you configured by using the switch CLI. If the VLAN group name is found, the corresponding VLANs under this VLAN group name are searched to find the least populated VLAN. Load balancing is achieved by moving the corresponding authorized user to that VLAN. 
 Note
 The RADIUS server can send the VLAN information in any combination of VLAN-IDs, VLAN names, or VLAN groups. 
802.1x User Distribution Configuration Guidelines
- 
                                    Confirm that at least one VLAN is mapped to the VLAN group.
- 
                                    You can map more than one VLAN to a VLAN group.
- 
                                    You can modify the VLAN group by adding or deleting a VLAN.
- 
                                    When you clear an existing VLAN from the VLAN group name, none of the authenticated ports in the VLAN are cleared, but the mappings are removed from the existing VLAN group.
- 
                                    If you clear the last VLAN from the VLAN group name, the VLAN group is cleared.
- 
                                    You can clear a VLAN group even when the active VLANs are mapped to the group. When you clear a VLAN group, none of the ports or users that are in the authenticated state in any VLAN within the group are cleared, but the VLAN mappings to the VLAN group are cleared.
IEEE 802.1x Authentication with Voice VLAN Ports
A voice VLAN port is a special access port associated with two VLAN identifiers:
- 
                                 		  
                                 VVID to carry voice traffic to and from the IP phone. The VVID is used to configure the IP phone connected to the port.
- 
                                 		  
                                 PVID to carry the data traffic to and from the workstation connected to the switch through the IP phone. The PVID is the native VLAN of the port.
The IP phone uses the VVID for its voice traffic, regardless of the authorization state of the port. This allows the phone to work independently of IEEE 802.1x authentication.
In single-host mode, only the IP phone is allowed on the voice VLAN. In multiple-hosts mode, additional clients can send traffic on the voice VLAN after a supplicant is authenticated on the PVID. When multiple-hosts mode is enabled, the supplicant authentication affects both the PVID and the VVID.
A voice VLAN port becomes active when there is a link, and the device MAC address appears after the first CDP message from the IP phone. Cisco IP phones do not relay CDP messages from other devices. As a result, if several IP phones are connected in series, the switch recognizes only the one directly connected to it. When IEEE 802.1x authentication is enabled on a voice VLAN port, the switch drops packets from unrecognized IP phones more than one hop away.
When IEEE 802.1x authentication is enabled on a switch port, you can configure an access port VLAN that is also a voice VLAN.
When IP phones are connected to an 802.1x-enabled switch port that is in single host mode, the switch grants the phones network access without authenticating them. We recommend that you use multidomain authentication (MDA) on the port to authenticate both a data device and a voice device, such as an IP phone
| Note | If you enable IEEE 802.1x authentication on an access port on which a voice VLAN is configured and to which a Cisco IP Phone is connected, the Cisco IP phone loses connectivity to the switch for up to 30 seconds. | 
IEEE 802.1x Authentication with Port Security
In general, Cisco does not recommend enabling port security when IEEE 802.1x is enabled. Since IEEE 802.1x enforces a single MAC address per port (or per VLAN when MDA is configured for IP telephony), port security is redundant and in some cases may interfere with expected IEEE 802.1x operations.
IEEE 802.1x Authentication with Wake-on-LAN
The IEEE 802.1x authentication with wake-on-LAN (WoL) feature allows dormant PCs to be powered when the switch receives a specific Ethernet frame, known as the magic packet. You can use this feature in environments where administrators need to connect to systems that have been powered down.
When a host that uses WoL is attached through an IEEE 802.1x port and the host powers off, the IEEE 802.1x port becomes unauthorized. The port can only receive and send EAPOL packets, and WoL magic packets cannot reach the host. When the PC is powered off, it is not authorized, and the switch port is not opened.
When the switch uses IEEE 802.1x authentication with WoL, the switch forwards traffic to unauthorized IEEE 802.1x ports, including magic packets. While the port is unauthorized, the switch continues to block ingress traffic other than EAPOL packets. The host can receive packets but cannot send packets to other devices in the network.
| Note | If PortFast is not enabled on the port, the port is forced to the bidirectional state. | 
When you configure a port as unidirectional by using the authentication control-direction in interface configuration command, the port changes to the spanning-tree forwarding state. The port can send packets to the host but cannot receive packets from the host.
When you configure a port as bidirectional by using the authentication control-direction both interface configuration command, the port is access-controlled in both directions. The port does not receive packets from or send packets to the host.
IEEE 802.1x Authentication with MAC Authentication Bypass
You can configure the switch to authorize clients based on the client MAC address by using the MAC authentication bypass feature. For example, you can enable this feature on IEEE 802.1x ports connected to devices such as printers.
If IEEE 802.1x authentication times out while waiting for an EAPOL response from the client, the switch tries to authorize the client by using MAC authentication bypass.
When the MAC authentication bypass feature is enabled on an IEEE 802.1x port, the switch uses the MAC address as the client identity. The authentication server has a database of client MAC addresses that are allowed network access. After detecting a client on an IEEE 802.1x port, the switch waits for an Ethernet packet from the client. The switch sends the authentication server a RADIUS-access/request frame with a username and password based on the MAC address. If authorization succeeds, the switch grants the client access to the network. If authorization fails, the switch assigns the port to the guest VLAN if one is configured. This process works for most client devices; however, it does not work for clients that use an alternate MAC address format. You can configure how MAB authentication is performed for clients with MAC addresses that deviate from the standard format or where the RADIUS configuration requires the user name and password to differ.
If an EAPOL packet is detected on the interface during the lifetime of the link, the switch determines that the device connected to that interface is an 802.1x-capable supplicant and uses 802.1x authentication (not MAC authentication bypass) to authorize the interface. EAPOL history is cleared if the interface link status goes down.
If the switch already authorized a port by using MAC authentication bypass and detects an IEEE 802.1x supplicant, the switch does not unauthorize the client connected to the port. When re-authentication occurs, the switch uses the authentication or re-authentication methods configured on the port, if the previous session ended because the Termination-Action RADIUS attribute value is DEFAULT.
Clients that were authorized with MAC authentication bypass can be re-authenticated. The re-authentication process is the same as that for clients that were authenticated with IEEE 802.1x. During re-authentication, the port remains in the previously assigned VLAN. If re-authentication is successful, the switch keeps the port in the same VLAN. If re-authentication fails, the switch assigns the port to the guest VLAN, if one is configured.
If re-authentication is based on the Session-Timeout RADIUS attribute (Attribute[27]) and the Termination-Action RADIUS attribute (Attribute [29]) and if the Termination-Action RADIUS attribute (Attribute [29]) action is Initialize (the attribute value is DEFAULT), the MAC authentication bypass session ends, and connectivity is lost during re-authentication. If MAC authentication bypass is enabled and the IEEE 802.1x authentication times out, the switch uses the MAC authentication bypass feature to initiate re-authorization. For more information about these AV pairs, see RFC 3580, “IEEE 802.1X Remote Authentication Dial In User Service (RADIUS) Usage Guidelines.”
MAC authentication bypass interacts with the features:
-  
                                 		  
                                 IEEE 802.1x authentication—You can enable MAC authentication bypass only if 802.1x authentication is enabled on the port .
-  
                                 		  
                                 Guest VLAN—If a client has an invalid MAC address identity, the switch assigns the client to a guest VLAN if one is configured.
-  
                                 		  
                                 Restricted VLAN—This feature is not supported when the client connected to an IEEE 802.lx port is authenticated with MAC authentication bypass.
-  
                                 		  
                                 Port security
-  
                                 		  
                                 Voice VLAN
- 
                                 				
                                 Private VLAN—You can assign a client to a private VLAN.
- 
                                 				
                                 Network Edge Access Topology (NEAT)—MAB and NEAT are mutually exclusive. You cannot enable MAB when NEAT is enabled on an interface, and you should not enable NEAT when MAB is enabled on an interface.
Cisco IOS Release 12.2(55)SE and later supports filtering of verbose MAB system messages
Network Admission Control Layer 2 IEEE 802.1x Validation
The switch supports the Network Admission Control (NAC) Layer 2 IEEE 802.1x validation, which checks the antivirus condition or posture of endpoint systems or clients before granting the devices network access. With NAC Layer 2 IEEE 802.1x validation, you can do these tasks:
-  
                                 		  
                                 Download the Session-Timeout RADIUS attribute (Attribute[27]) and the Termination-Action RADIUS attribute (Attribute[29]) from the authentication server.
-  
                                 		  
                                 Set the number of seconds between re-authentication attempts as the value of the Session-Timeout RADIUS attribute (Attribute[27]) and get an access policy against the client from the RADIUS server.
-  
                                 		  
                                 Set the action to be taken when the switch tries to re-authenticate the client by using the Termination-Action RADIUS attribute (Attribute[29]). If the value is the DEFAULT or is not set, the session ends. If the value is RADIUS-Request, the re-authentication process starts.
- 
                                 		  
                                 Set the list of VLAN number or name or VLAN group name as the value of the Tunnel Group Private ID (Attribute[81]) and the preference for the VLAN number or name or VLAN group name as the value of the Tunnel Preference (Attribute[83]). If you do not configure the Tunnel Preference, the first Tunnel Group Private ID (Attribute[81]) attribute is picked up from the list.
- 
                                 		  
                                 View the NAC posture token, which shows the posture of the client, by using the show authentication privileged EXEC command.
-  
                                 		  
                                 Configure secondary private VLANs as guest VLANs.
Configuring NAC Layer 2 IEEE 802.1x validation is similar to configuring IEEE 802.1x port-based authentication except that you must configure a posture token on the RADIUS server.
Flexible Authentication Ordering
- 
                                    		  
                                    dot1X—IEEE 802.1X authentication is a Layer 2 authentication method.
- 
                                    		  
                                    mab—MAC-Authentication Bypass is a Layer 2 authentication method.
- 
                                    		  
                                    webauth—Web authentication is a Layer 3 authentication method.
Using this feature, you can control which ports use which authentication methods, and you can control the failover sequencing of methods on those ports. For example, MAC authentication bypass and 802.1x can be the primary or secondary authentication methods, and web authentication can be the fallback method if either or both of those authentication attempts fail.
- 
                                    		  
                                    multi-auth—Multiauthentication allows one authentication on a voice VLAN and multiple authentications on the data VLAN.
- 
                                    		  
                                    multi-domain—Multidomain authentication allows two authentications: one on the voice VLAN and one on the data VLAN.
Open1x Authentication
Open1x authentication allows a device access to a port before that device is authenticated. When open authentication is configured, a new host can pass traffic according to the access control list (ACL) defined on the port. After the host is authenticated, the policies configured on the RADIUS server are applied to that host.
You can configure open authentication with these scenarios:
- 
                                 		  
                                 Single-host mode with open authentication–Only one user is allowed network access before and after authentication.
- 
                                 		  
                                 MDA mode with open authentication–Only one user in the voice domain and one user in the data domain are allowed.
- 
                                 		  
                                 Multiple-hosts mode with open authentication–Any host can access the network.
- 
                                 		  
                                 Multiple-authentication mode with open authentication–Similar to MDA, except multiple hosts can be authenticated. 
 Note
 If open authentication is configured, it takes precedence over other authentication controls. This means that if you use the authentication open interface configuration command, the port will grant access to the host irrespective of the authentication port-control interface configuration command. 
Multidomain Authentication
The switch supports multidomain authentication (MDA), which allows both a data device and voice device, such as an IP phone (Cisco or non-Cisco), to authenticate on the same switch port. The port is divided into a data domain and a voice domain.
| Note | For all host modes, the line protocol stays up before authorization when port-based authentication is configured. | 
MDA does not enforce the order of device authentication. However, for best results, we recommend that a voice device is authenticated before a data device on an MDA-enabled port.
Follow these guidelines for configuring MDA:
-  
                                 		  
                                 You must configure a switch port for MDA.
-  
                                 		  
                                 You must configure the voice VLAN for the IP phone when the host mode is set to multidomain.
-  
                                 		  
                                 Voice VLAN assignment on an MDA-enabled port is supported Cisco IOS Release 12.2(40)SE and later.
-  
                                 		  
                                 To authorize a voice device, the AAA server must be configured to send a Cisco Attribute-Value (AV) pair attribute with a value of device-traffic-class=voice. Without this value, the switch treats the voice device as a data device.
-  
                                 		  
                                 The guest VLAN and restricted VLAN features only apply to the data devices on an MDA-enabled port. The switch treats a voice device that fails authorization as a data device.
-  
                                 		  
                                 If more than one device attempts authorization on either the voice or the data domain of a port, it is error disabled.
-  
                                 		  
                                 Until a device is authorized, the port drops its traffic. Non-Cisco IP phones or voice devices are allowed into both the data and voice VLANs. The data VLAN allows the voice device to contact a DHCP server to obtain an IP address and acquire the voice VLAN information. After the voice device starts sending on the voice VLAN, its access to the data VLAN is blocked.
-  
                                 		  
                                 A voice device MAC address that is binding on the data VLAN is not counted towards the port security MAC address limit.
-  
                                 		  
                                 MDA can use MAC authentication bypass as a fallback mechanism to allow the switch port to connect to devices that do not support IEEE 802.1x authentication.
-  
                                 		  
                                 When a data or a voice device is detected on a port, its MAC address is blocked until authorization succeeds. If the authorization fails, the MAC address remains blocked for 5 minutes.
-  
                                 		  
                                 If more than five devices are detected on the data VLAN or more than one voice device is detected on the voice VLAN while a port is unauthorized, the port is error disabled.
-  
                                 		  
                                 When a port host mode is changed from single- or multihost to multidomain mode, an authorized data device remains authorized on the port. However, a Cisco IP phone that has been allowed on the port voice VLAN is automatically removed and must be reauthenticated on that port.
-  
                                 		  
                                 Active fallback mechanisms such as guest VLAN and restricted VLAN remain configured after a port changes from single- or multihost mode to multidomain mode.
-  
                                 		  
                                 Switching a port host mode from multidomain to single- or multihost mode removes all authorized devices from the port.
-  
                                 		  
                                 If a data domain is authorized first and placed in the guest VLAN, non-IEEE 802.1x-capable voice devices need to tag their packets on the voice VLAN to trigger authentication.
-  
                                 		  
                                 We do not recommend per-user ACLs with an MDA-enabled port. An authorized device with a per-user ACL policy might impact traffic on both the voice and data VLANs of the port. If used, only one device on the port should enforce per-user ACLs.
802.1x Supplicant and Authenticator Switches with Network Edge Access Topology (NEAT)
The Network Edge Access Topology (NEAT) feature extends identity to areas outside the wiring closet (such as conference rooms). This allows any type of device to authenticate on the port.
-  
                                 		  
                                 802.1x switch supplicant: You can configure a switch to act as a supplicant to another switch by using the 802.1x supplicant feature. This configuration is helpful in a scenario, where, for example, a switch is outside a wiring closet and is connected to an upstream switch through a trunk port. A switch configured with the 802.1x switch supplicant feature authenticates with the upstream switch for secure connectivity. Once the supplicant switch authenticates successfully the port mode changes from access to trunk in an authenticator switch. In a supplicant switch you must manually configure trunk when enabling CISP.
-  
                                 		  
                                 If the access VLAN is configured on the authenticator switch, it becomes the native VLAN for the trunk port after successful authentication.
In the default state, when you connect a supplicant switch to an authenticator switch that has BPDU guard enabled, the authenticator port could be error-disabled if it receives a Spanning Tree Protocol (STP) bridge protocol data unit (BPDU) packets before the supplicant switch has authenticated. Beginning with Cisco IOS Release 15.0(1)SE, you can control traffic exiting the supplicant port during the authentication period. Entering the dot1x supplicant controlled transient global configuration command temporarily blocks the supplicant port during authentication to ensure that the authenticator port does not shut down before authentication completes. If authentication fails, the supplicant port opens. Entering the no dot1x supplicant controlled transient global configuration command opens the supplicant port during the authentication period. This is the default behavior.
We strongly recommend using the dot1x supplicant controlled transientcommand on a supplicant switch when BPDU guard is enabled on the authenticator switch port with the spanning-tree bpduguard enable interface configuration command.
| Note | If you globally enable BPDU guard on the authenticator switch by using the spanning-tree portfast bpduguard default global configuration command, entering the dot1x supplicant controlled transient command does not prevent the BPDU violation. | 
You can enable MDA or multiauth mode on the authenticator switch interface that connects to one more supplicant switches. Multihost mode is not supported on the authenticator switch interface.
When you reboot an authenticator switch with single-host mode enabled on the interface, the interface may move to err-disabled state before authentication. To recover from err-disabled state, flap the authenticator port to activate the interface again and initiate authentication.
Use the dot1x supplicant force-multicast global configuration command on the supplicant switch for Network Edge Access Topology (NEAT) to work in all host modes.
-  
                                 		  
                                 Host Authorization: Ensures that only traffic from authorized hosts (connecting to the switch with supplicant) is allowed on the network. The switches use Client Information Signalling Protocol (CISP) to send the MAC addresses connecting to the supplicant switch to the authenticator switch.
-  
                                 		  
                                 Auto enablement: Automatically enables trunk configuration on the authenticator switch, allowing user traffic from multiple VLANs coming from supplicant switches. Configure the cisco-av-pair as device-traffic-class=switch at the ISE. (You can configure this under the group or the user settings.)
| 1 | Workstations (clients) | 2 | Supplicant switch (outside wiring closet) | 
| 3 | Authenticator switch | 4 | Cisco ISE | 
| 5 | Trunk port |  |  | 
| Note | The switchport nonegotiate command is not supported on supplicant and authenticator switches with NEAT. This command should not be configured at the supplicant side of the topology. If configured on the authenticator side, the internal macros will automatically remove this command from the port. | 
Voice Aware 802.1x Security
| Note | To use voice aware IEEE 802.1x authentication, the switch must be running the LAN base image. | 
You use the voice aware 802.1x security feature to configure the switch to disable only the VLAN on which a security violation occurs, whether it is a data or voice VLAN. In previous releases, when an attempt to authenticate the data client caused a security violation, the entire port shut down, resulting in a complete loss of connectivity.
You can use this feature in IP phone deployments where a PC is connected to the IP phone. A security violation found on the data VLAN results in the shutdown of only the data VLAN. The traffic on the voice VLAN flows through the switch without interruption.
Common Session ID
Authentication manager uses a single session ID (referred to as a common session ID) for a client no matter which authentication method is used. This ID is used for all reporting purposes, such as the show commands and MIBs. The session ID appears with all per-session syslog messages.
The session ID includes:
- 
                                 The IP address of the Network Access Device (NAD)
- 
                                 A monotonically increasing unique 32 bit integer
- 
                                 The session start time stamp (a 32 bit integer)
This example shows how the session ID appears in the output of the show authentication command. The session ID in this example is 160000050000000B288508E5:
Device# show authentication sessions
Interface  MAC Address     Method   Domain   Status         Session ID
Fa4/0/4    0000.0000.0203  mab      DATA     Authz Success  160000050000000B288508E5
This is an example of how the session ID appears in the syslog output. The session ID in this example is also160000050000000B288508E5:
1w0d: %AUTHMGR-5-START: Starting 'mab' for client (0000.0000.0203) on Interface Fa4/0/4
AuditSessionID 160000050000000B288508E5
1w0d: %MAB-5-SUCCESS: Authentication successful for client (0000.0000.0203) on Interface
Fa4/0/4 AuditSessionID 160000050000000B288508E5
1w0d: %AUTHMGR-7-RESULT: Authentication result 'success' from 'mab' for client
(0000.0000.0203) on Interface Fa4/0/4 AuditSessionID 160000050000000B288508E5
The session ID is used by the NAD, the AAA server, and other report-analyzing applications to identify the client. The ID appears automatically. No configuration is required.