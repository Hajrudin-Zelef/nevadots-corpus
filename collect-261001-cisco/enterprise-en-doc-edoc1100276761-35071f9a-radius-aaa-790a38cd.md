Enterprise
AAA can be implemented using multiple protocols. RADIUS is most frequently used in actual scenarios.
RADIUS is a protocol that uses the client/server model in distributed mode and protects a network from unauthorized access. It is often used on networks that require high security and control remote user access. It defines the UDP-based RADIUS packet format and transmission mechanism, and specifies destination UDP ports 1812 and 1813 as the default authentication and accounting ports respectively.
At the very beginning, RADIUS was only the AAA protocol used for dial-up users. As the user access mode diversifies, such as Ethernet access, RADIUS can also be applied to these access modes. RADIUS provides the access service through authentication and authorization and records the network resource usage of users through accounting.
RADIUS has the following characteristics:
Client/Server model
Secure message exchange mechanism
Fine scalability
RADIUS client
RADIUS clients run on the NAS to transmit user information to a specified RADIUS server and process requests (for example, permit or reject user access requests) based on the responses from the server. RADIUS clients can locate at any node on a network.
As a RADIUS client, a device supports:
standard RADIUS protocol and its extensions, including RFC 2865 and RFC 2866
RADIUS server status detection
retransmission of Accounting-Request(Stop) packets in the local buffer
active/standby and load balancing functions between RADIUS servers
RADIUS server
RADIUS servers typically run on central computers and workstations to maintain user authentication and network service access information. The servers receive connection requests from users, authenticate the users, and send all required information (such as permitting or rejecting authentication requests) to the clients. A RADIUS server generally needs to maintain three databases, as shown in Figure 1-6.
Authentication messages between a RADIUS server and RADIUS clients are exchanged using a shared key. The shared key is a character string that is transmitted in out-of-band mode, is known to both clients and the server, and does not need to be transmitted independently on the network.
A RADIUS packet has a 16-octet Authenticator field that contains the digital signature data of the whole packet. The signature data is calculated using the MD5 algorithm and shared key. The RADIUS packet receiver needs to verify whether the signature is correct and discards the packet if the signature is incorrect.
This mechanism improves security of message exchange between RADIUS clients and the RADIUS server. In addition, user passwords contained in RADIUS packets are encrypted using shared keys before the packets are transmitted to prevent the user passwords from being stolen during transmission on an insecure network.
A RADIUS packet consists of a packet header and a certain number of attributes. The protocol implementation remains unchanged even if new attributes are added to a RADIUS packet.
RADIUS is based on the UDP protocol. Figure 1-7 shows the RADIUS packet format.
Attribute: This field is variable in length. RADIUS attributes carry the specific authentication, authorization, accounting information and configuration details for the request and reply packets. The Attribute field may contain multiple attributes, each of which consists of Type, Length, and Value. For details, see RADIUS Attributes.
RADIUS defines 16 types of packets. Table 1-5 describes types of the authentication packets, Table 1-6 describes types of the accounting packets. For RADIUS CoA/DM packets, see RADIUS CoA/DM.
| Packet Name | Description | 
|---|---|
| Access-Request | Access-Request packets are sent from a client to a RADIUS server and is the first packet transmitted in a RADIUS packet exchange process. This packet conveys information (such as the user name and password) used to determine whether a user is allowed access to a specific NAS and any special services requested for that user. | 
| Access-Accept | After a RADIUS server receives an Access-Request packet, it must send an Access-Accept packet if all attribute values in the Access-Request packet are acceptable (authentication success). The user is allowed access to requested services only after the RADIUS client receives this packet. | 
| Access-Reject | After a RADIUS server receives an Access-Request packet, it must send an Access-Reject packet if any of the attribute values are not acceptable (authentication failure). | 
| Access-Challenge | During an EAP relay authentication, when a RADIUS server receives an Access-Request packet carrying the user name from a client, it generates a random MD5 challenge and sends the MD5 challenge to the client through an Access-Challenge packet. The client encrypts the user password using the MD5 challenge, and then sends the encrypted password in an Access-Request packet to the RADIUS server. The RADIUS server compares the encrypted password received from the client with the locally encrypted password. If they are the same, the server determines the user is valid. | 
| Packet Name | Description | 
|---|---|
| Accounting-Request(Start) | If a RADIUS client uses RADIUS accounting, the client sends this packet to a RADIUS server before accessing network resources. | 
| Accounting-Response(Start) | The RADIUS server must send an Accounting-Response(Start) packet after the server successfully receives and records an Accounting-Request(Start) packet. | 
| Accounting-Request(Interim-update) | You can configure the real-time accounting function on a RADIUS client to prevent the RADIUS server from continuing user accounting if it fails to receive the Accounting-Request(Stop) packet. The client then periodically sends Accounting-Request(Interim-update) packets to the server, reducing accounting deviation. | 
| Accounting-Response(Interim-update) | The RADIUS server must send an Accounting-Response(Interim-update) packet after the server successfully receives and records an Accounting-Request(Interim-update) packet. | 
| Accounting-Request(Stop) | When a user goes offline proactively or is forcibly disconnected by the NAS, the RADIUS client sends this packet carrying the network resource usage information (including the online duration and number of incoming/outgoing bytes) to the RADIUS server, requesting the server to stop accounting. | 
| Accounting-Response(Stop) | The RADIUS server must send an Accounting-Response(Stop) packet after receiving an Accounting-Request(Stop) packet. | 
A device that functions as a RADIUS client collects user information, including the user name and password, and sends the information to the RADIUS server. The RADIUS server then authenticates users according to the information, after which it performs authorization and accounting for the users. Figure 1-8 shows the information exchange process between a user, a RADIUS client, and a RADIUS server.
The RADIUS server verifies the user identity:
When a user is authenticated, a device sends an Access-Request packet to the RADIUS server. To ensure that the device can receive a response packet from the server even if a network fault or delay occurs, a retransmission upon timeout mechanism is used. The retransmission times and retransmission interval are controlled using timers.
As shown in Figure 1-9, 802.1X authentication and client-initiated authentication are used as an example. After receiving an EAP packet (EAP-Response/Identity) containing the user name of the client, the device encapsulates the packet into a RADIUS Access-Request packet and sends the packet to the RADIUS server. The retransmission timer is enabled at the same time. The retransmission timer is composed of the retransmission interval and retransmission times. If the device does not receive any response packet from the RADIUS server when the retransmission interval expires, it sends a RADIUS Access-Request packet again.
The device does not definitely mark the status of the server that does not respond as Down. The device marks the server status as Down only if the corresponding conditions are met.
For the RADIUS server status introduction and conditions for a device to mark the server status as Down, see RADIUS Server Status Detection.
RADIUS packet retransmission discussed here applies only to a single server. If multiple servers are configured in a RADIUS server template, the overall retransmission period depends on the retransmission interval, retransmission times, RADIUS server status, number of servers, and algorithm for selecting the servers.
| Command | Description | 
|---|---|
| radius-server retransmit retry-times | Specifies the retransmission times. The default value is 5. | 
| radius-server timeout time-value | Specifies the retransmission interval. The default value is 2 seconds. | 
In addition, the algorithm for selecting a RADIUS server can be set to the single user-based or packet-based algorithm. If the algorithm for selecting a RADIUS server is set to the single user-based algorithm, authentication server information is saved in the authentication phase, and the device preferentially sends an accounting request to the accounting server in the accounting phase when the authentication server is also the accounting server. If the algorithm for selecting a RADIUS server is set to the packet-based algorithm, authentication server information is not saved in the authentication phase, and the accounting server is reselected in the accounting phase, which may result in that authentication and accounting for a user is not performed on the same server.
The primary and secondary roles are determined by the weights configured for the RADIUS authentication servers or RADIUS accounting servers. The server with the largest weight is the primary server. If the weight values are the same, the earliest configured server is the primary server. As shown in Figure 1-10, the device preferentially sends an authentication or accounting packet to the primary server among all servers in Up status. If the primary server does not respond, the device then sends the packet to the secondary server.
If this algorithm is used and a device sends an authentication or accounting packet to a server, the device selects a server based on the weights configured for the RADIUS authentication servers or RADIUS accounting servers. As shown in Figure 1-11, RADIUS server1 is in Up status and its weight is 80, and RADIUS server2 is also in Up status and its weight is 20. The possibility for the device to send the packet to RADIUS server1 is 80% [80/(80 + 20)], and that for RADIUS server2 is 20% [20/(80 + 20)].
Regardless of which algorithm is used, if all the servers in Up status do not respond to a packet sent by a device, the device retransmits the packet to a server among the servers whose status is originally marked as Down (to which the device has not sent any authentication or accounting packets) based on the server weight. If the device does not receive any response in the current authentication mode, the backup authentication mode is used, for example, local authentication mode. The backup authentication mode needs to be already configured in the authentication scheme. Otherwise, the authentication process ends.
Availability and maintainability of a RADIUS server are the prerequisites of user access authentication. If a device cannot communicate with the RADIUS server, the server cannot perform authentication or authorization for users. To resolve this issue, the device supports the user escape function upon transition of the RADIUS server status to Down. To be specific, if the RADIUS server goes Down, users cannot be authorized by the server but still have certain network access rights.
The user escape function upon transition of the RADIUS server status to Down can be enabled only after the device marks the RADIUS server status as Down. If the RADIUS server status is not marked as Down and the device cannot communicate with the RADIUS server, users cannot be authorized by the server and the escape function is also unavailable. As a result, users have no network access rights. Therefore, the device must be capable of detecting the RADIUS server status in a timely manner. If the device detects that the RADIUS server status transitions to Down, users can obtain escape rights; if the device detects that the RADIUS server status reverts to Up, escape rights are removed from the users and the users are reauthenticated.
The mechanism of checking the RADIUS server status by using accounting packets is the same as that of checking the RADIUS server status by using request packets.
After the automatic detection function is enabled, automatic detection is classified into the following conditions depending on differences of the RADIUS server status.
| Server Status | Whether the RADIUS Server Is Available | Reason for the Status | 
|---|---|---|
| Up | The RADIUS server is available. |  | 
| Down | The RADIUS server is unavailable. | The conditions for marking the RADIUS server status as Down are met. | 
| Force-up | When no RADIUS server is available, the device establishes a connection with a RADIUS server in Force-up state. | The timer specified by dead-time expires. | 
The RADIUS server status is initially marked as Up. After a RADIUS Access-Request packet is received and the conditions for marking the RADIUS server status as Down are met, the RADIUS server status transitions to Down. The RADIUS Access-Request packet that triggers the server status transition can be sent during user authentication or constructed by the administrator. For example, the RADIUS Access-Request packet can be a test packet sent when the test-aaa command is run or detection packet sent during automatic detection.
Whether the status of a RADIUS server can be marked as Down depends on the following factors:
The device marks the RADIUS server status as Down during the RADIUS server status detection.
After the system starts, the RADIUS server status detection timer runs. If the device does not receive any packet from the RADIUS server after sending the first RADIUS Access-Request packet to the server and the condition that the number of times the device does not receive any packet from the server (n) is greater than or equal to the maximum number of consecutive unacknowledged packets (dead-count) is met in a detection interval, a communication interruption is recorded. If the device still does not receive any packet from the RADIUS server, the device marks the RADIUS server status as Down when recording the communication interruption for the same times as the detection interval cycles.
If the device does not record any communication interruption in a detection interval, all the previous communication interruption records are cleared.
The device marks the status of a RADIUS server as Down if no response is received from the server for a long period of time.
If the user access frequency is low, the device receives only a few RADIUS Access-Request packets from users, conditions for marking the RADIUS server status as Down during the RADIUS server status detection cannot be met, and the interval for sending two consecutive unacknowledged RADIUS Access-Request packets is greater than the value of max-unresponsive-interval, the device marks the RADIUS server status as Down. This mechanism ensures that users can obtain escape authorization.
If multiple servers are configured in the RADIUS server template, the overall status detection time is related to the number of servers and the server selection algorithm. If a user terminal uses the client software for authentication and the timeout period of the terminal client software is less than the summary of all the status detection time, the terminal client software may dial up repeatedly and cannot access the network. If the user escape function is configured, the summary of all the status detection time must be less than the timeout period of the terminal client software to ensure that escape rights can be added to the users.
The following table lists the related commands.
| Command | Description | 
|---|---|
| radius-server { dead-interval dead-interval \| dead-count dead-count \| detect-cycle detect-cycle } | Configures conditions for marking the RADIUS server status as Down during the RADIUS server status detection.  | 
| radius-server max-unresponsive-interval interval | Configures the longest unresponsive interval of the RADIUS server. The default value is 300 seconds. If the interval for sending two consecutive RADIUS Access-Request packets is greater than the value of max-unresponsive-interval, the device marks the RADIUS server status as Down. | 
| radius-server dead-time dead-time | Configures the duration for which the RADIUS server status remains Down. dead-time: Specifies the duration for which the RADIUS server status remains Down after the server status is marked as Down. After the duration expires, the device marks the server status as Force-up. The default value is 5 minutes. | 
After the RADIUS server status is marked as Down, you can configure the automatic detection function to test the RADIUS server reachability.
The automatic detection function needs to be manually enabled. The automatic server status detection function can be enabled only if the user name and password for automatic detection are configured in the RADIUS server template view on the device rather than on the RADIUS server. Authentication success is not mandatory. If the device can receive the authentication failure response packet, the RADIUS server is properly working.
| Server Status | Whether Automatic Detection Is Supported | Time When an Automatic Detection Packet Is Sent | Condition for Switching the Server Status | 
|---|---|---|---|
| Down | Automatic detection is supported by default. | An automatic detection packet is sent after the automatic detection period expires. | If the device receives a response packet from the RADIUS server within the timeout period for detection packets, the device marks the RADIUS server status as Up; otherwise, the RADIUS server status remains Down. | 
| Up | Automatic detection can be enabled using the radius-server detect-server up-server interval command. | An automatic detection packet is sent after the automatic detection period expires. | If the conditions for marking the RADIUS server status as Down are met, the device marks the RADIUS server status as Down; otherwise, the RADIUS server status remains Up. | 
| Force-up | Automatic detection is supported by default. | An automatic detection packet is sent immediately. | If the device receives a packet from the RADIUS server within the timeout period, the device marks the RADIUS server status as Up; otherwise, the device marks the RADIUS server status as Down. | 
On a large-scale network, you are advised not to enable automatic detection for RADIUS servers in Up state. This is because if automatic detection is enabled on multiple NAS devices, the RADIUS server periodically receives a large number of detection packets when processing RADIUS Access-Request packets source from users, which may deteriorate processing performance of the RADIUS server.
After the radius-server testuser command is configured, the dead-time timer configured using the radius-server dead-time command does not take effect.
The following table lists commands related to automatic detection.
| Command | Description | 
|---|---|
| radius-server testuser username user-name password cipher password | Enables the automatic detection function.  | 
| radius-server detect-server interval interval | Specifies the automatic detection interval for RADIUS servers in Down status. The default value is 60 seconds. | 
| radius-server detect-server up-server interval interval | Enables the automatic detection function for the RADIUS server in Up status and configures the automatic detection interval. The default value is 0 seconds; that is, the device does not automatically detect RADIUS servers in Up status. | 
| radius-server detect-server timeout time-value | Specifies the timeout period for automatic detection packets. The default value is 3 seconds. | 
After the device marks the RADIUS server status as Down, you can configure the escape function to make users obtain escape authorization. After the device detects that the RADIUS server status reverts to Up, you can configure the reauthentication function to make users obtain authorization from the server through reauthentication, as shown in Figure 1-13.
For 802.1X authenticated users and MAC address authenticated users, after the RADIUS server status reverts to Up, users exist from escape authorization and are reauthenticated. For Portal authenticated users, after the RADIUS server status reverts to Up, users obtain pre-connection authorization and can be redirected to the Portal server for authentication only if the users attempt to access network resources.
The following table lists the commands for configuring the escape rights upon transition of the RADIUS server status to Down and configuring the reauthentication function, respectively.
| Command | Description | 
|---|---|
| authentication event authen-server-down action authorize { vlan vlan-id \| service-scheme service-scheme-name \| ucl-group ucl-group-name } [ response-fail ] | Configures the escape function upon transition of the RADIUS server status to Down. | 
| authentication event authen-server-up action re-authen | Configures the reauthentication function for users in escape status when the RADIUS server status reverts to Up. | 
Table 1-7 describes types of the CoA/DM packets.
| Packet Name | Description | 
|---|---|
| CoA-Request | When an administrator needs to modify the rights of an online user (for example, prohibit the user from accessing a website), the RADIUS server sends this packet to the RADIUS client, requesting the client to modify the user rights. | 
| CoA-ACK | If the RADIUS client successfully modifies the user rights, it returns this packet to the RADIUS server. | 
| CoA-NAK | If the RADIUS client fails to modify the user rights, it returns this packet to the RADIUS server. | 
| DM-Request | When an administrator needs to disconnect a user, the RADIUS server sends this packet to the RADIUS client, requesting the client to disconnect the user. | 
| DM-ACK | If the RADIUS client has disconnected the user, it returns this packet to the RADIUS server. | 
| DM-NAK | If the RADIUS client fails to disconnect the user, it returns this packet to the RADIUS server. | 
CoA allows the administrator to change the rights of an online user or perform reauthentication for the user through RADIUS after the user passes authentication. Figure 1-14 shows the CoA interaction process.
When a user needs to be disconnected, the RADIUS server sends a DM packet to the device. Figure 1-15 shows the DM interaction process.
The device returns a DM-ACK or DM-NAK packet as follows:
Different from the process in which authorization is performed for an online user or a user proactively goes offline, the server sends a request packet and the device sends a response packet in the CoA/DM process. If CoA/DM succeeds, the device returns an ACK packet. Otherwise, the device returns a NAK packet.
Each service provided by the NAS to a user constitutes a session, with the beginning of the session defined as the point where service is first provided and the end of the session defined as the point where service is ended.
The match methods are as follows:
any method
The device performs a match check between an attribute and user information on the device. The priority for identifying the RADIUS attributes used by the users is as follows: Acct-Session-ID (44) > Calling-Station-Id (31) > Framed-IP-Address (8). The device searches for the attributes in the request packet based on the priority, and performs a match check between the first found attribute and user information on the device. If the attribute is successfully matched, the device responds with an ACK packet; otherwise, the device responds with a NAK packet.
all method
The device performs a match check between all attributes and user information on the device. The device identifies the following RADIUS attributes used by the users: Acct-Session-ID (44), Calling-Station-Id (31), Framed-IP-Address (8), and User-Name (1). The device performs a match check between all the preceding attributes in the Request packet and user information on the device. If all the preceding attributes are successfully matched, the device responds with an ACK packet; otherwise, the device responds with a NAK packet.
When the CoA-Request or DM-Request packet from the RADIUS server fails to match user information on the device, the device describes the failure cause using the error code in the CoA-NAK or DM-NAK packet. For the error code description, see Table 1-8 and Table 1-9.
| Name | Value | Description | 
|---|---|---|
| RD_DM_ERRCODE_MISSING_ATTRIBUTE | 402 | The request packet lacks key attributes, so that the integrity check of the RADIUS attributes fails. | 
| RD_DM_ERRCODE_INVALID_REQUEST | 404 | Parsing the attributes in the request packet fails. | 
| RD_DM_ERRCODE_INVALID_ATTRIBUTE_VALUE | 407 | The request packet contains attributes that are not supported by the device or do not exist, so that the attribute check fails. Contents of the authorization check include VLAN, ACL, CAR, number of the ACL used for redirection, and whether Huawei RADIUS extended attributes RD_hw_URL_Flag and RD_hw_Portal_URL can be authorized to the interface-based authenticated user. Errors that may occur are as follows:  | 
| RD_DM_ERRCODE_SESSION_CONTEXT_NOT_FOUND | 503 | The session request fails. The cause includes:  | 
| RD_DM_ERRCODE_RESOURCES_UNAVAILABLE | 506 | This error code is used for other authorization failures. | 
RFC2865, RFC2866, and RFC3576 define standard RADIUS attributes that are supported by all mainstream vendors. For details, see Table 1-10.
| Attribute No. | Attribute Name | Attribute Type | Description | 
|---|---|---|---|
| 1 | User-Name | string | User name for authentication. The user name format can be user name@domain name or user name.  NOTE:  If this attribute carries Chinese characters, it cannot be delivered using the radius-attribute set attribute-name attribute-value command. | 
| 2 | User-Password | string | User password for authentication, which is only valid for the Password Authentication Protocol (PAP). | 
| 3 | CHAP-Password | string | User password for authentication, which is only valid for the Challenge Handshake Authentication Protocol (CHAP). | 
| 4 | NAS-IP-Address | ipaddr | Internet Protocol (IP) address of the NAS carried in authentication request packets. By default, the attribute value is the source IP address of the authentication request packets sent by the NAS. You can change the attribute value to the specified IP address on the NAS or the IP address of the AP using the radius-attribute nas-ip { ip-address \| ap-info } command. | 
| 5 | NAS-Port | integer | Physical port number of the network access server that is authenticating the user, which is in either of the following formats:  | 
| 6 | Service-Type | integer | Service type of the user to be authenticated:  | 
| 7 | Framed-Protocol | integer | Encapsulation protocol of Frame services:  | 
| 8 | Framed-IP-Address | ipaddr | User IP address. | 
| 9 | Framed-IP-Netmask | ipaddr | User IP address mask. This field must be used with the Framed-IP-Address field. | 
| 11 | Filter-Id | string | UCL group name, user group name, or IPv4 Access Control List (ACL) ID.  NOTE:   | 
| 12 | Framed-MTU | integer | MTU value of the data link between the NAS and server. For example, in 802.1X Extensible Authentication Protocol (EAP) authentication, the NAS specifies the maximum length of the EAP packet in this attribute. An EAP packet larger than the link MTU may be lost.  NOTE:  This attribute is carried only in EAP relay mode. | 
| 14 | Login-IP-Host | ipaddr | Management user IP address:  | 
| 15 | Login-Service | integer | Service to use to connect the user to the login host:   NOTE:  An attribute can contain multiple service types. | 
| 18 | Reply-Message | string | This attribute determines whether a user is authenticated:  | 
| 19 | Callback-Number | string | Information sent from the authentication server and to be displayed to a user, such as a mobile number. | 
| 22 | Framed-Route | string | Routing information provided by the RADIUS server to users, in format Destination/Mask NextHop Metric, for example, 192.168.1.0/24 192.168.1.1 1. If the NextHop value is 0.0.0.0, the user IP address is used as the next hop address. The device can obtain only one Metric value. If the attribute delivered by the RADIUS server contains multiple Metric values, the device obtains only the first one.  NOTE:  Currently, the Framed-Route attribute cannot provide routing information for PPPoE users on the device. | 
| 24 | State | string | If the RADIUS server sends a RADIUS Access-Challenge packet carrying this attribute to a device, the subsequent RADIUS Access-Request packets sent from the device must carry this attribute with the same value. This attribute is sent by the server to the client in an Access-Accept packet that also includes a Termination-Action attribute with the value of RADIUS-Request. If the NAS performs the termination action by sending a new Access-Request packet upon the termination of the current session, it must include the State attribute unchanged in that Access-Request packet. | 
| 25 | Class | string | If the RADIUS server sends a RADIUS Access-Accept packet carrying the Class attribute to the NAS, the subsequent RADIUS Accounting-Request packets sent from the NAS must carry the Class attribute with the same value. | 
| 26 | Vendor-Specific | string | Vendor-specific attribute. For details, see Table 1-11. A packet can carry one or more private attributes. Each private attribute contains one or more sub-attributes. | 
| 27 | Session-Timeout | integer | In the Access-Accept packet, this attribute indicates the maximum number of seconds a user should be allowed to remain connected. In the Access-Challenge packet, this attribute indicates the duration for which EAP authentication users are reauthenticated. When the value of this attribute is 0:   NOTE:  This attribute is only valid for 802.1X, MAC address, Portal, and PPPoE authentication users. When the RADIUS server delivers only this attribute, the value of attribute 29 Termination-Action is set to 0 (users are forced offline) by default. | 
| 28 | Idle-Timeout | integer | Maximum number of consecutive seconds of idle connection the user is allowed before termination of the session or prompt.  NOTE:   | 
| 29 | Termination-Action | integer | What action the NAS should take when the specified service is completed:   NOTE:  This attribute is valid only for 802.1X and MAC address authentication users. When the authentication point is deployed on a VLANIF interface, MAC address authenticated users do not support the authorization of Termination-Action=1. When the RADIUS server delivers only this attribute, the value of attribute 27 Session-Timeout is set to 3600s (for 802.1X authentication users) or 1800s (for MAC address authentication users) by default. | 
| 30 | Called-Station-Id | string | Number of the NAS:   NOTE:  If this attribute carries Chinese characters, it cannot be delivered using the radius-attribute set attribute-name attribute-value command. | 
| 31 | Calling-Station-Id | string | Identification number of the client. Generally, it is the MAC address of the client.  NOTE:  If this attribute carries Chinese characters, it cannot be delivered using the radius-attribute set attribute-name attribute-value command. | 
| 32 | NAS-Identifier | string | String identifying the NAS device originating the Access-Request. By default, the attribute value is the host name of the NAS device. You can change the attribute value to the VLAN ID of the user or the MAC address of the AP using the radius-server nas-identifier-format { hostname \| vlan-id \| ap-info } command.  NOTE:  If this attribute carries Chinese characters, it cannot be delivered using the radius-attribute set attribute-name attribute-value command. | 
| 40 | Acct-Status-Type | integer | Accounting-Request type:  | 
| 41 | Acct-Delay-Time | integer | Number of seconds the client has been trying to send the accounting packet (excluding the network transmission time). | 
| 42 | Acct-Input-Octets | integer | Number of bytes in upstream traffic, corresponding to the lower 32 bits in the data structure for storing the upstream traffic. Contents of this attribute and the RADIUS attribute 52 (Acct-Input-Gigawords) compose the upstream traffic. The traffic unit must be the same as that of the RADIUS server and can be Byte, KByte, MByte, and GByte. To set the traffic unit for each RADIUS server, run the radius-server traffic-unit command. By default, the unit is Byte. | 
| 43 | Acct-Output-Octets | integer | Number of bytes in downstream traffic, corresponding to the lower 32 bits in the data structure for storing the downstream traffic. Contents of this attribute and the RADIUS attribute 53 (Acct-Output-Gigawords) compose the downstream traffic. The traffic unit must be the same as that of the RADIUS server and can be Byte, KByte, MByte, and GByte. To set the traffic unit for each RADIUS server, run the radius-server traffic-unit command. By default, the unit is Byte. | 
| 44 | Acct-Session-Id | string | Accounting session ID. The Accounting-Start, Interim-Accounting, and Accounting-Stop packets of the same accounting session must have the same session ID. The format of this attribute is: Host name (7 bits) + Slot ID (2 bits) + Subcard number (1 bit) + Port number (2 bits) + Outer VLAN ID (4 bits) + Inner VLAN ID (5 bits) + Central Processing Unit (CPU) Tick (6 bits) + User ID prefix (2 bits) + User ID (5 bits). | 
| 45 | Acct-Authentic | integer | User authentication mode:  | 
| 46 | Acct-Session-Time | integer | How long (in seconds) the user has received service.  NOTE:  If the administrator modifies the system time after the user goes online, the online time calculated by the device may be incorrect. | 
| 47 | Acct-Input-Packets | integer | Number of incoming packets. | 
| 48 | Acct-Output-Packets | integer | Number of outgoing packets. | 
| 49 | Acct-Terminate-Cause | integer | Cause of a terminated session:  | 
| 50 | Acct-Multi-Session-Id | string | Accounting ID, which is used in multi-link session scenarios. | 
| 52 | Acct-Input-Gigawords | integer | Number of times the number of bytes in upstream traffic is greater than 4 GB (2^32), corresponding to the higher 32 bits in the data structure for storing the upstream traffic. Contents of this attribute and the RADIUS attribute 42 (Acct-Input-Octets) compose the upstream traffic. The traffic unit must be the same as that of the RADIUS server and can be Byte, KByte, MByte, and GByte. To set the traffic unit for each RADIUS server, run the radius-server traffic-unit command. By default, the unit is Byte. | 
| 53 | Acct-Output-Gigawords | integer | Number of times the number of bytes in downstream traffic is greater than 4 GB (2^32), corresponding to the higher 32 bits in the data structure for storing the downstream traffic. Contents of this attribute and the RADIUS attribute 43 (Acct-Output-Octets) compose the downstream traffic. The traffic unit must be the same as that of the RADIUS server and can be Byte, KByte, MByte, and GByte. To set the traffic unit for each RADIUS server, run the radius-server traffic-unit command. By default, the unit is Byte. | 
| 55 | Event-Timestamp | integer | Time when an Accounting-Request packet is generated, represented by is the number of seconds elapsed since 00:00:00 of January 1, 1970. | 
| 56 | Egress-VLANID | integer | VLAN with a specified ID that an interface is added to in tagged or untagged mode. The VLAN ID is converted into a 6-digit hexadecimal number. If the number contains fewer than 6 digits, the number is prefixed with zeros. Then, the 6-digit hexadecimal number is prefixed with 0x31 in tagged mode or with 0x32 in untagged mode, which generates an 8-digit hexadecimal number. Finally, the 8-digit hexadecimal number is converted into a decimal number as the value configured on the RADIUS server. For example, the value used to authorize VLAN 48 to which an interface is added in untagged mode is 838860848.  NOTE:  The Egress-VLANID can be authorized only after the authentication mode multi-share command is configured in an authentication profile. | 
| 58 | Egress-VLAN-Name | string | VLAN with a specified name or description that an interface is added to in tagged or untagged mode. The VLAN name is prefixed with 1 in tagged mode or 2 in untagged mode. For example, the value used to authorize VLAN 48 to which an interface is added in untagged mode is 2vlan48. | 
| 60 | CHAP-Challenge | string | Challenge field in CHAP authentication. This field is generated by the NAS for Message Digest algorithm 5 (MD5) calculation. | 
| 61 | NAS-Port-Type | integer | NAS port type.  | 
| 64 | Tunnel-Type | integer | Protocol type of the tunnel. The value is fixed as 13, indicating VLAN. | 
| 65 | Tunnel-Medium-Type | integer | Medium type used on the tunnel. The value is fixed as 6, indicating Ethernet. | 
| 66 | Tunnel-Client-Endpoint | string | Tunnel client address. | 
| 67 | Tunnel-Server-Endpoint | string | Tunnel server address. | 
| 75 | Password-Retry | integer | Number of password retries. | 
| 79 | EAP-Message | string | Encapsulates Extended Access Protocol (EAP) packets so that RADIUS supports EAP authentication. When an EAP packet is longer than 253 bytes, the packet is encapsulated into multiple attributes. A RADIUS packet can carry multiple EAP-Message attributes. | 
| 80 | Message-Authenticator | string | Authenticates and verifies authentication packets to prevent spoofing packets. This attribute is used only when RADIUS supports EAP authentication. | 
| 81 | Tunnel-Private-Group-ID | string | Tunnel private group ID, which is used to deliver user VLANs.  NOTE:  If users are authorized with VLANs based on VLAN pools in the Tunnel-Private-Group-ID attribute, both the Tunnel-Type and Tunnel-Medium-Type attributes must be configured, and their values must be 13 and 6 respectively. The VLAN pool is configured as the service VLAN for wireless users. If the authorized VLAN is in the VLAN pool, the VLAN can be switched when an IP address fails to be obtained. If the authorized VLAN is not in the VLAN pool, the VLAN cannot be switched. Authorization can be performed based on the VLAN ID, VLAN description, VLAN name, and VLAN pool, which are listed in descending order of priority. Both wired and wireless users support authorization based on the VLAN pool. In V200R013C00 and later versions, wireless users support authorization based on the VLAN pool. When the Tunnel-Private-Group-ID attribute is used to authorize a VLAN pool, the HW-VoiceVlan attribute cannot be used to authorize a voice VLAN. Otherwise, the two attributes do not take effect. For details, see Licensing Requirements and Limitations for NAC Unified Mode. | 
| 82 | Tunnel-Assignment-Id | string | ID allocated to a tunnel. | 
| 83 | Tunnel-Preference | string | Tunnel priority. | 
| 85 | Acct-Interim-Interval | integer | Interim accounting interval, in seconds. It is recommended that the interval be at least 600 seconds. The value ranges from 60 to 3932100. | 
| 87 | NAS-Port-Id | string | Port of the NAS that is authenticating the user. The NAS-Port-Id attribute has the following formats:   NOTE:  If this attribute carries Chinese characters, it cannot be delivered using the radius-attribute set attribute-name attribute-value command. | 
| 88 | Framed-Pool | string | Address pool, which is only included in the Access-Accept packet. It is used as authorization information in Efficient VPN. | 
| 89 | Chargeable-User-Identity | string | Charging ID delivered by the server. To configure a device to support this attribute, run the radius-server support chargeable-user-identity [ not-reject ] command. | 
| 90 | Tunnel-Client-Auth-Id | string | Client tunnel ID used for authentication during tunnel setup. | 
| 91 | Tunnel-Server-Auth-Id | string | Server tunnel ID used for authentication during tunnel setup. | 
| 95 | NAS-IPv6-Address | ipaddr | IPv6 address carried in the authentication request packet sent by the NAS. Both the NAS-IPv6-Address and NAS-IP-Address fields can be included in a packet. | 
| 96 | Framed-Interface-Id | string | IPv6 interface identifier to be configured for the user. | 
| 97 | Framed-IPv6-Prefix | ipaddr | IPv6 prefix to be configured for the user. | 
| 98 | Login-IPv6-Host | ipaddr | User IPv6 address carried in the RADIUS authentication request of an IPv6 administrator. | 
| 101 | Error-Cause | integer | Cause of the authorization failure:  | 
| 121 | Input-Peak-Rate | integer | Uplink peak rate. | 
| 122 | Input-Average-Rate | integer | Average uplink rate. | 
| 123 | Input-Basic-Rate | integer | Uplink basic rate. | 
| 124 | Output-Peak-Rate | integer | Downlink peak rate. | 
| 125 | Output-Average-Rate | integer | Average downlink rate. | 
| 126 | Output-Basic-Rate | integer | Downlink basic rate. | 
| 135 | Ascend-Client-Primary-Dns | ipaddr | DNS server address 1. | 
| 136 | Ascend-Client-Secondary-Dns | ipaddr | DNS server address 2. | 
| 168 | Framed-IPv6-Address | ipaddr | IPv6 address of the user. | 
| 192 | Remote-IP-Host | ipaddr | Remote IP host. | 
| 195 | HW-SecurityStr | string | Security information of users in EAP relay authentication. | 
RADIUS is a fully extensible protocol. The No. 26 attribute (Vendor-Specific) defined in RFC2865 can be used to extend RADIUS for implementing functions not supported by standard RADIUS attributes. Table 1-11 describes Huawei proprietary RADIUS attributes.
Extended RADIUS attributes contain the vendor ID of the device. The vendor ID of Huawei is 2011.
| Attribute No. | Attribute Name | Attribute Type | Description | 
|---|---|---|---|
| 26-1 | HW-Input-Peak-Information-Rate | integer | Peak information rate (PIR) at which the user accesses the NAS, which is the maximum rate of traffic that can pass through an interface. The value is a 4-byte integer, in bit/s. The minimum value is 64. The HW-Input-Peak-Information-Rate must be higher than or equal to the HW-Input-Committed-Information-Rate. The default HW-Input-Peak-Information-Rate is equal to the HW-Input-Committed-Information-Rate. | 
| 26-2 | HW-Input-Committed-Information-Rate | integer | Committed information rate (CIR) at which the user accesses the NAS, which is the allowed average rate of traffic that can pass through an interface. The value is a 4-byte integer, in bit/s. The minimum value is 64.  NOTE:  This attribute must be specified when the rate of packets sent from the user to the NAS is limited. | 
| 26-3 | HW-Input-Committed-Burst-Size | integer | Committed burst size (CBS) at which the user accesses the NAS, which is the average volume of burst traffic that can pass through an interface. The value is a 4-byte integer, in bits. The minimum value is 80000. | 
| 26-4 | HW-Output-Peak-Information-Rate | integer | Peak information rate at which the NAS connects to the user. The value is a 4-byte integer, in bit/s. The minimum value is 64. The HW-Output-Peak-Information-Rate must be higher than or equal to the HW-Output-Committed-Information-Rate. The default HW-Output-Peak-Information-Rate is equal to the HW-Output-Committed-Information-Rate. | 
| 26-5 | HW-Output-Committed-Information-Rate | integer | Committed information rate at which the NAS connects to the user. The value is a 4-byte integer, in bit/s. The minimum value is 64.  NOTE:  This attribute must be specified when the rate of packets sent from the NAS to the user is limited. | 
| 26-6 | HW-Output-Committed-Burst-Size | integer | Committed burst size at which the NAS connects to the user. The value is a 4-byte integer, in bits. The minimum value is 80000. | 
| 26-15 | HW-Remanent-Volume | integer | Remaining traffic. The unit is KB. | 
| 26-17 | HW-Subscriber-QoS-Profile | string | Name of the QoS profile.  NOTE:    | 
| 26-18 | HW-UserName-Access-Limit | integer | Maximum number of users who are allowed to access the network using the same user name. The limit is indicated by a particular numeric value as follows:   NOTE:  This attribute can be carried only in Access-Accept packets. | 
| 26-26 | HW-Connect-ID | integer | Index of a user connection. | 
| 26-28 | HW-FTP-Directory | string | Initial directory of an FTP user. | 
| 26-29 | HW-Exec-Privilege | integer | Privilege level of an administrator (such as a Telnet user). The value ranges from 0 to 15. The priority that is greater than or equal to 16 is ineffective. | 
| 26-31 | HW-Qos-Data | string | Name of the QoS profile. The maximum length of the name is 31 bytes. The RADIUS server uses this field to deliver the QoS profile for traffic policing. The QoS profile must exist on the device and traffic policing has been configured using the car (QoS profile view) command.  NOTE:  If the server delivers both the uplink or downlink bandwidth limit (equivalent to the RADIUS attribute HW-Input-Committed-Information-Rate or HW-Output-Committed-Information-Rate) and the RADIUS attribute HW-Qos-Data for user authorization, only the uplink or downlink bandwidth limit take effect. | 
| 26-33 | HW-VoiceVlan | integer | Voice VLAN authorization flag. The value 1 indicates that the authorized VLAN is the voice VLAN. This attribute is used with VLAN authorization attributes.  NOTE:  After the authentication mode multi-share command is configured in the authentication profile, voice VLAN authorization is not supported. When the Tunnel-Private-Group-ID attribute is used to authorize a VLAN pool, the HW-VoiceVlan attribute cannot be used to authorize a voice VLAN. Otherwise, the two attributes do not take effect. | 
| 26-35 | HW-ProxyRdsPkt | integer | Whether a RADIUS server is a proxy server:  | 
| 26-59 | HW-NAS-Startup-Time-Stamp | integer | NAS start time, represented by the number of seconds elapsed since 00:00:00 of January 1, 1970. | 
| 26-60 | HW-IP-Host-Address | string | User IP address and MAC address carried in authentication and accounting packets, in the format A.B.C.D hh:hh:hh:hh:hh:hh. The IP address and MAC address are separated by a space. If the user's IP address is detected to be invalid during authentication, the IP address is set to 255.255.255.255. | 
| 26-61 | HW-Up-Priority | integer | 802.1p priority of upstream packets. | 
| 26-62 | HW-Down-Priority | integer | 802.1p priority of downstream packets. | 
| 26-75 | HW-Primary-WINS | ipaddr | Primary WINS server address delivered by the RADIUS server after a user is successfully authenticated. | 
| 26-76 | HW-Second-WINS | ipaddr | Secondary WINS server address delivered by the RADIUS server after a user is successfully authenticated. | 
| 26-77 | HW-Input-Peak-Burst-Size | integer | Upstream peak rate, in bit/s. The minimum value is 10000. | 
| 26-78 | HW-Output-Peak-Burst-Size | integer | Downstream peak rate, in bit/s. The minimum value is 10000. | 
| 26-82 | HW-Data-Filter | string | Used by the RADIUS server to deliver IPv4 or IPv6 ACL rules to users. ACL rules can be delivered in two modes: delivering ACL rules through DACL groups and delivering ACL rules directly. There are old and new attribute formats for ACL rules. Compared with the old attribute format, the new attribute format shortens the length of an ACL rule. Using a DACL group to deliver ACL rules saves more ACL resources than delivering ACL rules directly. The users in the same DACL group share the ACL resources in the group, whereas each user occupies ACL resources when ACL rules are directly delivered.  NOTE:   Directly delivering ACL rules in new attribute format (fields in square brackets are optional) The attribute format is as follows: $number permit/deny [ protocol ] [ direction ip-address [ port ] ] [ direction ip-address [ port ] ]. The fields are described as follows:  The following examples are the attribute values entered on the server: $1 permit dst 10.0.239.192/26 $2 permit udp src any 8080 $3 permit icmp echo dst 10.1.1.1/24 $5 deny Directly delivering ACL rules in old attribute format The attribute format is acl number key1 key-value1... keyN key-valueN permit/deny. The fields are described as follows: All keywords are case-insensitive. All keywords are separated from keyword values using spaces. The location of keywords is not fixed. The keywords permit and deny can be placed after number or the whole command line.  The following examples are the attribute values entered on the server: acl 10005 deny acl 10006 tcp-dstport 5080 permit acl 10007 dest-ip 10.11.11.2 dest-ipmask 32 permit acl 10008 dest-ip 10.11.11.3 dest-ipmask 32 udp-dstport 5070 permit acl 10009 dest-ip 11.11.11.2 dest-ipmask 32 udp-dstport 5070 udp-dstport-end 5080 deny Delivering ACL rules using DACL groups The format of ACL rules in a DACL group can be the new or old format. The new format is recommended. When the device is connected to a Cisco ISE server, an ACL rule starts with the number sign (#). The following examples are the attribute values entered on the server: $1 dacl-group-name example $2 permit dst 10.0.239.192/26 $3 permit udp src any 8080 $4 deny | 
| 26-94 | HW-VPN-Instance | string | VPN instance name delivered by the RADIUS server after a user is successfully authenticated. It specifies the VPN to which the user belongs. When the RADIUS server delivers this attribute, it also needs to deliver the standard RADIUS attribute 8 (Framed-IP-Address). | 
| 26-135 | HW-Client-Primary-DNS | ipaddr | Primary DNS address delivered by the RADIUS server after a user is successfully authenticated. | 
| 26-136 | HW-Client-Secondary-DNS | ipaddr | Secondary DNS address delivered by the RADIUS server after a user is successfully authenticated. | 
| 26-138 | HW-Domain-Name | string | Name of the domain used for user authentication. This attribute can be the domain name contained in a user name or the name of a forcible domain. | 
| 26-141 | HW-AP-Information | string | AP's MAC address used for STA authentication, in H-H-H format. H is a 4-digit hexadecimal number. | 
| 26-142 | HW-User-Information | string | User security check information delivered by the RADIUS server to an Extensible Authentication Protocol over LAN (EAPoL) user to notify the user of items that require security checks. | 
| 26-146 | HW-User-Policy | string | Service scheme name. A service scheme contains user authorization information and policies. | 
| 26-153 | HW-Access-Type | integer | User access type carried in the Access-Request or Accounting-Request packets that the device sends to the RADIUS server:  | 
| 26-155 | HW-URL-Flag | integer | Whether a Uniform Resource Locator (URL) needs to be forcibly pushed when it is used together with another attribute, for example, HW-Portal-URL:  | 
| 26-156 | HW-Portal-URL | string | Forcibly pushed URL. The maximum length is 247 bytes. If information delivered by the RADIUS server matches the configured URL template, the URL configured in the template is used. Otherwise, the character string delivered by the RADIUS server is used. | 
| 26-157 | HW-Terminal-Type | string | Terminal type of a user. To configure this attribute, run the device-type device-name command. | 
| 26-158 | HW-DHCP-Option | hex | DHCP Option, encapsulated in Type-Length-Value (TLV) format. A packet may contain multiple HW-DHCP-Option attributes to carry Option information. Only Option 82 can be delivered. | 
| 26-159 | HW-HTTP-UA | string | User-Agent information in Hypertext Transfer Protocol (HTTP) packets. | 
| 26-160 | HW-UCL-Group | integer | Index of a UCL group.  NOTE:  This attribute cannot be authorized together with ACLs. Otherwise, only the authorized ACLs take effect. | 
| 26-161 | HW-Forwarding-VLAN | string | Delivers the Internet Service Provider (ISP) VLAN for user packet forwarding.  NOTE:  This attribute is supported only by the following: X series cards. | 
| 26-162 | HW-Forwarding-Interface | string | Outbound interface for forwarding user packets.  NOTE:  This attribute is supported only by the following: X series cards. | 
| 26-163 | HW-LLDP | string | LLDP information. A packet can contain multiple HW-LLDP-Info attributes to carry different options. The meanings of different options are as follows:  | 
| 26-164 | HW-CDP | string | CDP information. A packet can contain multiple HW-CDP attributes to carry different options. The meanings of different options are as follows:  | 
| 26-166 | HW-Acct-ipv6-Input-Octets | integer | Number of upstream bytes in an IPv6 flow. The unit can be byte, kilobyte, megabyte, or gigabyte. | 
| 26-167 | HW-Acct-ipv6-Output-Octets | integer | Number of downstream bytes in an IPv6 flow. The unit can be byte, kilobyte, megabyte, or gigabyte. | 
| 26-168 | HW-Acct-ipv6-Input-Packets | integer | Number of upstream packets in an IPv6 flow. | 
| 26-169 | HW-Acct-ipv6-Output-Packets | integer | Number of downstream packets in an IPv6 flow. | 
| 26-170 | HW-Acct-ipv6-Input-Gigawords | integer | This attribute specifies the number of times that more than 4 GB upstream packets are carried in an IPv6 flow. This attribute is usually used with the HW-Acct-ipv6-Input-Octets attribute. | 
| 26-171 | HW-Acct-ipv6-Output-Gigawords | integer | This attribute specifies the number of times that more than 4 GB downstream packets are carried in an IPv6 flow. This attribute is usually used with the HW-Acct-ipv6-Output-Octets attribute. | 
| 26-173 | HW-Redirect-ACL | string | Redirect IPv4 ACL. Redirection is performed for only the users matching the ACL rules. The ACL number or ACL name can be delivered. The ACL name must start with a character.  NOTE:  The value range of acl-number is from 3000 to 3999 for wired users and from 3000 to 3031 for wireless users. After the authentication mode multi-share command is configured in the authentication profile, redirect ACL authorization is not supported. | 
| 26-174 | HW-Tariff-Ipv6-Input-Octets | string | Number of upstream bytes in the IPv6 traffic at the specified tariff level sent to the accounting server. This field is included in the accounting packets. The unit is Byte, KByte, MByte, or GByte. | 
| 26-175 | HW-Tariff-Ipv6-Output-Octets | string | Number of downstream bytes in the IPv6 traffic at the specified tariff level sent to the accounting server. This field is included in the accounting packets. The unit is Byte, KByte, MByte, or GByte. | 
| 26-176 | HW-Tariff-Ipv6-Input-Gigawords | string | How many times is the number of upstream bytes in the IPv6 traffic at the specified tariff level larger than 4 GB. This field and the HW-Tariff-Ipv6-Input-Octets field specify the number of upstream bytes in the IPv6 traffic at the specified tariff level. | 
| 26-177 | HW-Tariff-Ipv6-Output-Gigawords | string | How many times is the number of downstream bytes in the IPv6 traffic at the specified tariff level larger than 4 GB. This field and the HW-Tariff-Ipv6-Input-Octets field specify the number of downstream bytes in the IPv6 traffic at the specified tariff level. | 
| 26-178 | HW-IPv6-Redirect-ACL | string | Redirection IPv6 ACL. Redirection is performed for only the users matching the ACL rules. The ACL number or ACL name can be delivered. The ACL name must start with a character.  NOTE:   | 
| 26-201 | HW-User-Extend-Info | string | Extended user information. This attribute is contained in authentication and accounting request packets. A packet can contain multiple HW-User-Extend-Info attributes. The following describes extended user information:  This attribute applies only to MAC address authentication and Portal authentication. | 
| 26-202 | HW-MUD-URL | string | Identity information of iConnect terminals. The information is carried in an authentication request packet. The RADIUS server determines whether a terminal is an iConnect terminal based on the HW-MUD-URL attribute. If the terminal is an iConnect terminal, the RADIUS server queries the corresponding authorization policy based on the user account and encapsulates the authorization policy into an authentication response packet. For an iConnect terminal, you are advised to configure a redirection-related RADIUS attribute (such as HW-Redirect-ACL or HW-Portal-URL). In this way, the iConnect terminal will be redirected to a URL to download an EAP-TLS certificate after being authenticated successfully. After the certificate is downloaded, EAP-TLS authentication is triggered for the terminal.  NOTE:  The HW-MUD-URL attribute can be used only in wireless scenarios. The HW-MUD-URL attribute is supported only on the X series cards. The RADIUS server must be iMaster NCE-Campus. If a client sends an EAPoL-Start packet to trigger authentication, iConnect-URL is not carried during RADIUS authentication of an iConnect terminal. | 
| 26-203 | HW-VIP-Level-ID | integer | User priority. The value is 0 or 1. A larger value indicates a higher priority. The default value is 0. | 
| 26-204 | HW-SAC-Profile | string | SAC profile name.  NOTE:  This attribute is supported only by the S12700E. | 
| 26-237 | HW-Web-Authen-Info | string | Information sent from the portal server via the device (which transparently transmits the information) to the RADIUS server. For example, a user selects the authentication-free option and time information for next login, based on which the RADIUS server saves the MAC address of the user for a period of time. Upon the next login of the user, the login page is not displayed. Instead, MAC address authentication is preferentially used. This attribute can be used for transparent transmission in complex modes such as EAP. | 
| 26-238 | HW-Ext-Specific | string | User extended attributes:   NOTE:  During RADIUS CoA dynamic authorization, when the value of the user-command field is 1, 2, or 3, other authorization attributes are not supported. The user-dscp-in and user-dscp-out attributes cannot be authorized to wireless users in direct forwarding mode. This attribute applies only to NAC users. Pay attention to the following points if the value of the user-command field in the RADIUS attribute HW-Ext-Specific(26-238) carried in a CoA packet sent by the RADIUS server is 2 or 3:  | 
| 26-239 | HW-User-Access-Info | string | User context profile information.  NOTE:  The attribute can be authorized only after the access-context profile enable command is run. | 
| 26-240 | HW-Access-Device-Info | string | The authentication and accounting request packets carry the IP addresses, MAC addresses, and port numbers of access switches in policy association. The format is ip=A.B.C.D;mac=XXXX-XXXX-XXXX;slot=XX;subslot=XXX;port=XXX;vlanid=XXXX; interfaceName=port. | 
| 26-244 | HW-Reachable-Detect | string | Server reachability detection information. Authentication packets carrying this attribute are server detection packets. | 
| 26-247 | HW-Tariff-Input-Octets | string | Number of upstream bytes at the specified tariff level sent to the accounting server. This field is included in the accounting packets. The unit can be byte, kilobyte, megabyte, or gigabyte. The format is Tariff level:Number of upstream bytes. An accounting packet can contain the traffic of at most 8 tariff levels. | 
| 26-248 | HW-Tariff-Output-Octets | string | Number of downstream bytes at the specified tariff level sent to the accounting server. This field is included in the accounting packets. The unit can be byte, kilobyte, megabyte, or gigabyte. The format is Tariff level:Number of downstream bytes. An accounting packet can contain the traffic of at most 8 tariff levels. | 
| 26-249 | HW-Tariff-Input-Gigawords | string | Number of times larger the number of upstream bytes at the specified tariff level is than 4G. This field and the HW-Tariff-Input-Octets field specify the number of upstream bytes at the specified tariff level. | 
| 26-250 | HW-Tariff-Output-Gigawords | string | Number of times larger the number of downstream bytes at the specified tariff level is than 4G. This field and the HW-Tariff-Output-Octets field specify the number of downstream bytes at the specified tariff level. | 
| 26-251 | HW-IPv6-Filter-ID | string | Number of a user IPv6 ACL. The value range is 2000 to 3999 (for wired users) or 2000 to 3031 (for wireless users).  NOTE:    | 
| 26-253 | HW-Framed-IPv6-Address | ipaddr | IPv6 address to be configured for the user. | 
| 26-254 | HW-Version | string | Software version of the device.  NOTE:  If this attribute carries Chinese characters, it cannot be delivered using the radius-attribute set attribute-name attribute-value command. | 
| 26-255 | HW-Product-ID | string | NAS product name. | 
| Attribute No. | Attribute Name | Attribute Type | Description | 
|---|---|---|---|
| MICROSOFT-16 | MS-MPPE-Send-Key | string | This attribute indicates the MPPE sending key. | 
| MICROSOFT-17 | MS-MPPE-Recv-Key | string | This attribute indicates the MPPE receiving key. | 
| CISCO-1 | Cisco-avpair | string | This attribute indicates the voice VLAN. | 
| DSLFORUM-1 | Agent-Circuit-Id | string | Circuit ID of the access device of the online user. | 
| DSLFORUM-2 | Agent-Remote-Id | string | Unique ID of the circuit associated with the online user. | 
The following table lists only the attributes supported by the device.
| Attribute No. | Access-Request | Access-Accept | Access-Reject | Access-Challenge | 
|---|---|---|---|---|
| User-Name(1) | 0-1 | 0-1 | 0 | 0 | 
| User-Password(2) | 0-1 | 0 | 0 | 0 | 
| CHAP-Password(3) | 0-1 | 0 | 0 | 0 | 
| NAS-IP-Address(4) | 0-1 | 0 | 0 | 0 | 
| NAS-Port(5) | 0-1 | 0 | 0 | 0 | 
| Service-Type(6) | 0-1 | 0-1 | 0 | 0 | 
| Framed-Protocol(7) | 0-1 | 0-1 | 0 | 0 | 
| Framed-IP-Address(8) | 0-1 | 0-1 | 0 | 0 | 
| Framed-IP-Netmask(9) | 0-1 | 0-1 | 0 | 0 | 
| Filter-Id(11) | 0 | 0+ | 0 | 0 | 
| Framed-MTU(12) | 0-1 | 0-1 | 0 | 0 | 
| Login-IP-Host(14) | 0+ | 0+ | 0 | 0 | 
| Login-Service(15) | 0 | 0-1 | 0 | 0 | 
| Reply-Message(18) | 0 | 0+ | 0+ | 0+ | 
| Callback-Number(19) | 0-1 | 0-1 | 0 | 0 | 
| Framed-Route(22) | 0 | 0+ | 0 | 0 | 
| State(24) | 0-1 | 0-1 | 0 | 0-1 | 
| Class(25) | 0 | 0+ | 0 | 0 | 
| Vendor-Specific(26) | 0+ | 0+ | 0 | 0+ | 
| Session-Timeout(27) | 0 | 0-1 | 0 | 0-1 | 
| Idle-Timeout(28) | 0 | 0-1 | 0 | 0-1 | 
| Termination-Action(29) | 0 | 0-1 | 0 | 0 | 
| Called-Station-Id(30) | 0-1 | 0 | 0 | 0 | 
| Calling-Station-Id(31) | 0-1 | 0 | 0 | 0 | 
| NAS-Identifier(32) | 0-1 | 0 | 0 | 0 | 
| Login-LAT-Service(34) | 0-1 | 0-1 | 0 | 0 | 
| Login-LAT-Node(35) | 0-1 | 0-1 | 0 | 0 | 
| Login-LAT-Group(36) | 0-1 | 0-1 | 0 | 0 | 
| Framed-AppleTalk-Link(37) | 0 | 0-1 | 0 | 0 | 
| Framed-AppleTalk-Network(38) | 0 | 0+ | 0 | 0 | 
| Framed-AppleTalk-Zone(39) | 0 | 0-1 | 0 | 0 | 
| Acct-Session-id(44) | 1 | 0 | 0 | 0 | 
| CHAP-Challenge(60) | 0-1 | 0 | 0 | 0 | 
| NAS-Port-Type(61) | 0-1 | 0 | 0 | 0 | 
| Login-LAT-Port(63) | 0-1 | 0-1 | 0 | 0 | 
| Tunnel-Type(64) | 0 | 0-1 | 0 | 0 | 
| Tunnel-Medium-Type(65) | 0 | 0-1 | 0 | 0 | 
| Tunnel-Client-Endpoint(66) | 0-1 | 0-1 | 0 | 0 | 
| Tunnel-Server-Endpoint(67) | 0-1 | 0-1 | 0 | 0 | 
| Password-Retry(75) | 0 | 0-1 | 0 | 0 | 
| EAP-Message(79) | 0-1 | 0-1 | 0-1 | 0-1 | 
| Message-Authenticator(80) | 0-1 | 0-1 | 0-1 | 0-1 | 
| Tunnel-Private-Group-ID(81) | 0 | 0-1 | 0-1 | 0 | 
| Tunnel-Assignment-Id(82) | 0 | 0-1 | 0 | 0 | 
| Tunnel-Preference(83) | 0 | 0-1 | 0 | 0 | 
| Acct-Interim-Interval(85) | 0 | 0-1 | 0 | 0 | 
| NAS-Port-Id(87) | 0-1 | 0 | 0 | 0 | 
| Framed-Pool(88) | 0 | 0-1 | 0 | 0 | 
| Chargeable-User-Identity(89) | 0-1 | 0-1 | 0 | 0 | 
| Tunnel-Client-Auth-Id(90) | 0 | 0+ | 0 | 0 | 
| Tunnel-Server-Auth-Id(91) | 0 | 0+ | 0 | 0 | 
| NAS-IPv6-Address(95) | 0-1 | 0 | 0 | 0 | 
| Framed-Interface-Id(96) | 0+ | 0 | 0 | 0 | 
| Framed-IPv6-Prefix(97) | 0+ | 0 | 0 | 0 | 
| Login-IPv6-Host(98) | 0-1 | 0 | 0 | 0 | 
| Input-Peak-Rate(121) | 0 | 0-1 | 0 | 0 | 
| Input-Average-Rate(122) | 0 | 0-1 | 0 | 0 | 
| Input-Basic-Rate(123) | 0 | 0-1 | 0 | 0 | 
| Output-Peak-Rate(124) | 0 | 0-1 | 0 | 0 | 
| Output-Average-Rate(125) | 0 | 0-1 | 0 | 0 | 
| Output-Basic-Rate(126) | 0 | 0-1 | 0 | 0 | 
| Ascend-Client-Primary-Dns(135) | 0 | 0-1 | 0 | 0 | 
| Ascend-Client-Secondary-Dns(136) | 0 | 0-1 | 0 | 0 | 
| Remote-IP-Host(192) | 0-1 | 0 | 0 | 0 | 
| HW-SecurityStr(195) | 0-1 | 0 | 0 | 0 | 
| HW-Input-Peak-Information-Rate(26-1) | 0 | 0-1 | 0 | 0 | 
| HW-Input-Committed-Information-Rate(26-2) | 0 | 0-1 | 0 | 0 | 
| HW-Input-Committed-Burst-Size(26-3) | 0 | 0-1 | 0 | 0 | 
| HW-Output-Peak-Information-Rate(26-4) | 0 | 0-1 | 0 | 0 | 
| HW-Output-Committed-Information-Rate(26-5) | 0 | 0-1 | 0 | 0 | 
| HW-Output-Committed-Burst-Size(26-6) | 0 | 0-1 | 0 | 0 | 
| HW-Remanent-Volume(26-15) | 0 | 0-1 | 0 | 0 | 
| HW-Subscriber-QoS-Profile(26-17) | 0 | 0-1 | 0 | 0 | 
| HW-UserName-Access-Limit(26-18) | 0 | 0-1 | 0 | 0 | 
| HW-Connect-ID(26-26) | 1 | 0 | 0 | 0 | 
| Ftp-directory(26-28) | 0 | 0-1 | 0 | 0 | 
| HW-Exec-Privilege(26-29) | 0 | 0-1 | 0 | 0 | 
| HW-Qos-Data(26-31) | 0 | 0-1 | 0 | 0 | 
| HW-VoiceVlan(26-33) | 0 | 0-1 | 0 | 0 | 
| HW-ProxyRdsPkt(26-35) | 0 | 0-1 | 0 | 0 | 
| HW-NAS-Startup-Time-Stamp(26-59) | 1 | 0 | 0 | 0 | 
| HW-IP-Host-Address(26-60) | 1 | 0 | 0 | 0 | 
| HW-Up-Priority(26-61) | 0 | 0-1 | 0 | 0 | 
| HW-Down-Priority(26-62) | 0 | 0-1 | 0 | 0 | 
| HW-Primary-WINS(26-75) | 0 | 0-1 | 0 | 0 | 
| HW-Second-WINS(26-76) | 0 | 0-1 | 0 | 0 | 
| HW-Input-Peak-Burst-Size(26-77) | 0 | 0-1 | 0 | 0 | 
| HW-Output-Peak-Burst-Size(26-78) | 0 | 0-1 | 0 | 0 | 
| HW-Data-Filter(26-82) | 0 | 0-1 | 0-1 | 0 | 
| HW-VPN-Instance(26-94) | 0 | 0-1 | 0 | 0 | 
| HW-Client-Primary-DNS(26-135) | 0 | 0-1 | 0 | 0 | 
| HW-Client-Secondary-DNS(26-136) | 0 | 0-1 | 0 | 0 | 
| HW-Domain-Name(26-138) | 0-1 | 0 | 0 | 0 | 
| HW-AP-Information(26-141) | 0-1 | 0 | 0 | 0 | 
| HW-User-Information(26-142) | 0 | 0-1 | 0 | 0 | 
| HW-User-Policy(26-146) | 0 | 0-1 | 0 | 0 | 
| HW-Access-Type(26-153) | 1 | 0-1 | 0 | 0 | 
| HW-URL-Flag(26-155) | 0 | 0-1 | 0 | 0 | 
| HW-Portal-URL(26-156) | 0 | 0-1 | 0 | 0 | 
| HW-Terminal-Type(26-157) | 0-1 | 0 | 0 | 0 | 
| HW-DHCP-Option(26-158) | 0+ | 0 | 0 | 0 | 
| HW-UCL-Group(26-160) | 0 | 0-1 | 0 | 0 | 
| HW-Forwarding-VLAN(26-161) | 0 | 0-1 | 0 | 0 | 
| HW-Forwarding-Interface(26-162) | 0 | 0-1 | 0 | 0 | 
| HW-LLDP(26-163) | 0-1 | 0 | 0 | 0 | 
| HW-CDP(26-164) | 0-1 | 0 | 0 | 0 | 
| HW-Redirect-ACL(26-173) | 0 | 0-1 | 0 | 0 | 
| HW-IPv6-Redirect-ACL(26-178) | 0 | 0-1 | 0 | 0 | 
| HW-User-Extend-Info(26-201) | 0-1 | 0 | 0 | 0 | 
| HW-MUD-URL(26-202) | 0-1 | 0 | 0 | 0 | 
| HW-VIP-Level-ID(26-203) | 0 | 0-1 | 0 | 0 | 
| HW-SAC-Profile(26-204) | 0 | 0-1 | 0 | 0 | 
| HW-Web-Authen-Info(26-237) | 0-1 | 0 | 0 | 0 | 
| HW-Ext-Specific(26-238) | 0 | 0-1 | 0 | 0 | 
| HW-User-Access-Info(26-239) | 0-1 | 0 | 0 | 0 | 
| HW-Access-Device-Info(26-240) | 0-1 | 0 | 0 | 0 | 
| HW-Reachable-Detect(26-244) | 0-1 | 0 | 0 | 0 | 
| HW-IPv6-Filter-ID(26-251) | 0 | 0-1 | 0 | 0 | 
| HW-Framed-IPv6-Address(26-253) | 0-1 | 0 | 0 | 0 | 
| HW-Version(26-254) | 1 | 0 | 0 | 0 | 
| HW-Product-ID(26-255) | 1 | 0 | 0 | 0 | 
| MS-MPPE-Send-Key(MICROSOFT-16) | 0 | 0-1 | 0 | 0 | 
| MS-MPPE-Recv-Key(MICROSOFT-17) | 0 | 0-1 | 0 | 0 | 
| Cisco-avpair(CISCO-1) | 0 | 0-1 | 0 | 0 | 
| Agent-Circuit-Id(DSLFORUM-1) | 0-1 | 0 | 0 | 0 | 
| Agent-Remote-Id(DSLFORUM-2) | 0-1 | 0 | 0 | 0 | 
| Attribute No. | Accounting-Request (Start) | Accounting-Request (Interim-Update) | Accounting-Request (Stop) | Accounting-Response (start) | Accounting-Response(Interim-Update) | Accounting-Response (Stop) | 
|---|---|---|---|---|---|---|
| User-Name(1) | 0-1 | 0-1 | 0-1 | 0 | 0 | 0 | 
| NAS-IP-Address(4) | 0-1 | 0-1 | 0-1 | 0 | 0 | 0 | 
| NAS-Port(5) | 0-1 | 0-1 | 0-1 | 0 | 0 | 0 | 
| Service-Type(6) | 0-1 | 0-1 | 0-1 | 0 | 0 | 0 | 
| Framed-Protocol(7) | 0-1 | 0-1 | 0-1 | 0 | 0 | 0 | 
| Framed-IP-Address(8) | 0-1 | 0-1 | 0-1 | 0-1 | 0-1 | 0-1 | 
| Class(25) | 0+ | 0+ | 0+ | 0 | 0 | 0 | 
| Vendor-Specific(26) | 0+ | 0+ | 0+ | 0 | 0 | 0 | 
| Session-Timeout(27) | 0-1 | 0-1 | 0-1 | 0-1 | 0-1 | 0 | 
| Called-Station-Id(30)  NOTE:  For users who access the network through PPP authentication, this attribute is optional. If the authentication request packet does not carry this attribute, then neither does the accounting request packet. | 0-1 | 0-1 | 0-1 | 0 | 0 | 0 | 
| Calling-Station-Id(31) | 0-1 | 0-1 | 0-1 | 0 | 0 | 0 | 
| NAS-Identifier(32) | 0-1 | 0-1 | 0-1 | 0 | 0 | 0 | 
| Acct-Status-Type(40) | 1 | 1 | 1 | 0 | 0 | 0 | 
| Acct-Delay-Time(41) | 0-1 | 0-1 | 0-1 | 0 | 0 | 0 | 
| Acct-Input-Octets(42) | 0-1 | 0-1 | 0-1 | 0 | 0 | 0 | 
| Acct-Output-Octets(43) | 0-1 | 0-1 | 0-1 | 0 | 0 | 0 | 
| Acct-Session-Id(44) | 1 | 1 | 1 | 0 | 0 | 0 | 
| Acct-Authentic(45) | 0-1 | 0-1 | 0-1 | 0 | 0 | 0 | 
| Acct-Session-Time(46) | 0-1 | 0-1 | 0-1 | 0 | 0 | 0 | 
| Acct-Input-Packets(47) | 0 | 0 | 0-1 | 0 | 0 | 0 | 
| Acct-Output-Packets(48) | 0 | 0-1 | 0-1 | 0 | 0 | 0 | 
| Acct-Terminate-Cause(49) | 0 | 0 | 1 | 0 | 0 | 0 | 
| Acct-Multi-Session-Id(50) | 0-1 | 0-1 | 0-1 | 0 | 0 | 0 | 
| Acct-Input-Gigawords(52) | 0 | 0-1 | 0-1 | 0 | 0 | 0 | 
| Acct-Output-Gigawords(53) | 0 | 0-1 | 0-1 | 0 | 0 | 0 | 
| Event-Timestamp(55) | 1 | 1 | 1 | 0 | 0 | 0 | 
| NAS-Port-Type(61) | 1 | 1 | 1 | 0 | 0 | 0 | 
| Tunnel-Type(64) | 0-1 | 0-1 | 0-1 | 0 | 0 | 0 | 
| Tunnel-Medium-Type(65) | 0-1 | 0-1 | 0-1 | 0 | 0 | 0 | 
| Tunnel-Client-Endpoint(66) | 0-1 | 0-1 | 0-1 | 0 | 0 | 0 | 
| Tunnel-Server-Endpoint(67) | 0-1 | 0-1 | 0-1 | 0 | 0 | 0 | 
| Tunnel-Assignment-Id(82) | 0-1 | 0-1 | 0-1 | 0 | 0 | 0 | 
| NAS-Port-Id(87) | 1 | 1 | 1 | 0 | 0 | 0 | 
| Chargeable-User-Identity(89) | 0-1 | 0-1 | 0-1 | 0 | 0 | 0 | 
| Tunnel-Client-Auth-Id(90) | 0+ | 0+ | 0+ | 0 | 0 | 0 | 
| Tunnel-Server-Auth-Id(91) | 0+ | 0+ | 0+ | 0 | 0 | 0 | 
| NAS-IPv6-Address(95) | 0-1 | 0-1 | 0-1 | 0 | 0 | 0 | 
| HW-Input-Committed-Information-Rate(26-2) | 0-1 | 0-1 | 0-1 | 0 | 0 | 0 | 
| HW-Output-Committed-Information-Rate(26-5) | 0-1 | 0-1 | 0-1 | 0 | 0 | 0 | 
| HW-Connect-ID(26-26) | 1 | 1 | 1 | 0 | 0 | 0 | 
| HW-IP-Host-Address(26-60) | 1 | 1 | 1 | 0 | 0 | 0 | 
| HW-Domain-Name(26-138) | 1 | 1 | 1 | 0 | 0 | 0 | 
| HW-AP-Information(26-141) | 0-1 | 0-1 | 0-1 | 0 | 0 | 0 | 
| HW-User-Information(26-142) | 0 | 0 | 0 | 0-1 | 0-1 | 0 | 
| HW-Access-Type(26-153) | 0-1 | 0-1 | 0-1 | 0 | 0 | 0 | 
| HW-Terminal-Type(26-157) | 0-1 | 0-1 | 0-1 | 0 | 0 | 0 | 
| HW-DHCP-Option(26-158) | 0+ | 0+ | 0+ | 0 | 0 | 0 | 
| HW-HTTP-UA(26-159) | 0-1 | 0-1 | 0-1 | 0 | 0 | 0 | 
| HW-LLDP(26-163) | 0-1 | 0-1 | 0-1 | 0 | 0 | 0 | 
| HW-CDP(26-164) | 0-1 | 0-1 | 0-1 | 0 | 0 | 0 | 
| HW-User-Extend-Info(26-201) | 0-1 | 0-1 | 0-1 | 0 | 0 | 0 | 
| HW-Access-Device-Info(26-240) | 0-1 | 0-1 | 0-1 | 0 | 0 | 0 | 
| HW-Reachable-Detect(26-244) | 0-1 | 0-1 | 0-1 | 0 | 0 | 0 | 
| HW-Tariff-Input-Octets(26-247) | 0 | 0-1 | 0-1 | 0 | 0 | 0 | 
| HW-Tariff-Output-Octets(26-248) | 0 | 0-1 | 0-1 | 0 | 0 | 0 | 
| HW-Tariff-Input-Gigawords(26-249) | 0 | 0-1 | 0-1 | 0 | 0 | 0 | 
| HW-Tariff-Output-Gigawords(26-250) | 0 | 0-1 | 0-1 | 0 | 0 | 0 | 
| HW-Framed-IPv6-Address(26-253) | 0-1 | 0-1 | 0-1 | 0 | 0 | 0 | 
| Agent-Circuit-Id(DSLFORUM-1) | 0-1 | 0-1 | 0-1 | 0 | 0 | 0 | 
| Agent-Remote-Id(DSLFORUM-2) | 0-1 | 0-1 | 0-1 | 0 | 0 | 0 | 
| Attribute No. | CoA REQUEST | CoA ACK | CoA NAK | DM REQUEST | DM ACK | DM NAK | 
|---|---|---|---|---|---|---|
| User-Name(1) | 0-1 | 0-1 | 0-1 | 0-1 | 0-1 | 0-1 | 
| NAS-IP-Address(4) | 0-1 | 0-1 | 0 | 0 | 0 | 0 | 
| NAS-Port(5) | 0-1 | 0 | 0 | 0 | 0 | 0 | 
| Framed-IP-Address(8) | 0-1 | 0-1 | 0-1 | 0-1 | 0-1 | 0-1 | 
| Filter-Id(11) | 0-1 | 0 | 0 | 0 | 0 | 0 | 
| Vendor-Specific(26) | 0+ | 0 | 0 | 0+ | 0 | 0 | 
| Session-Timeout(27) | 0-1 | 0 | 0 | 0 | 0 | 0 | 
| Idle-Timeout(28) | 0-1 | 0 | 0 | 0 | 0 | 0 | 
| Termination-Action(29) | 0-1 | 0 | 0 | 0 | 0 | 0 | 
| Calling-Station-Id(31) | 0-1 | 0-1 | 0-1 | 0-1 | 0-1 | 0-1 | 
| Acct-Session-Id(44) | 1 | 1 | 1 | 1 | 1 | 1 | 
| Tunnel-Type(64) | 0-1 | 0 | 0 | 0 | 0 | 0 | 
| Tunnel-Medium-Type(65) | 0-1 | 0 | 0 | 0 | 0 | 0 | 
| Message-Authenticator(80) | 0-1 | 0 | 0 | 0 | 0 | 0 | 
| Tunnel-Private-Group-ID(81) | 0-1 | 0 | 0 | 0 | 0 | 0 | 
| Acct-Interim-Interval(85) | 0-1 | 0 | 0 | 0 | 0 | 0 | 
| NAS-Port-Id(87) | 0-1 | 0 | 0 | 0 | 0 | 0 | 
| Error-Cause(101) | 0 | 0 | 0+ | 0 | 0 | 0+ | 
| HW-Input-Peak-Information-Rate(26-1) | 0-1 | 0 | 0 | 0 | 0 | 0 | 
| HW-Input-Committed-Information-Rate(26-2) | 0-1 | 0 | 0 | 0 | 0 | 0 | 
| HW-Output-Peak-Information-Rate(26-4) | 0-1 | 0 | 0 | 0 | 0 | 0 | 
| HW-Output-Committed-Information-Rate(26-5) | 0-1 | 0 | 0 | 0 | 0 | 0 | 
| HW-Output-Committed-Burst-Size(26-6) | 0-1 | 0 | 0 | 0 | 0 | 0 | 
| HW-Subscriber-QoS-Profile(26-17) | 0-1 | 0 | 0 | 0 | 0 | 0 | 
| HW-Qos-Data(26-31) | 0-1 | 0 | 0 | 0 | 0 | 0 | 
| HW-Up-Priority(26-61) | 0-1 | 0 | 0 | 0 | 0 | 0 | 
| HW-Down-Priority(26-62) | 0-1 | 0 | 0 | 0 | 0 | 0 | 
| HW-Input-Peak-Burst-Size(26-77) | 0-1 | 0 | 0 | 0 | 0 | 0 | 
| HW-Output-Peak-Burst-Size(26-78) | 0-1 | 0 | 0 | 0 | 0 | 0 | 
| HW-Data-Filter(26-82) | 0-1 | 0 | 0 | 0 | 0 | 0 | 
| HW-User-Policy(26-146) | 0-1 | 0 | 0 | 0 | 0 | 0 | 
| HW-URL-Flag(26-155) | 0-1 | 0 | 0 | 0 | 0 | 0 | 
| HW-Portal-URL(26-156) | 0-1 | 0 | 0 | 0 | 0 | 0 | 
| HW-UCL-Group (26-160) | 0-1 | 0 | 0 | 0 | 0 | 0 | 
| HW-Forwarding-VLAN(26-161) | 0-1 | 0 | 0 | 0 | 0 | 0 | 
| HW-Forwarding-Interface(26-162) | 0-1 | 0 | 0 | 0 | 0 | 0 | 
| HW-Redirect-ACL(26-173) | 0-1 | 0 | 0 | 0 | 0 | 0 | 
| HW-IPv6-Redirect-ACL(26-178) | 0-1 | 0 | 0 | 0 | 0 | 0 | 
| HW-VIP-Level-ID(26-203) | 0-1 | 0 | 0 | 0 | 0 | 0 | 
| HW-SAC-Profile(26-204) | 0-1 | 0 | 0 | 0 | 0 | 0 | 
| HW-Ext-Specific(26-238) | 0-1 | 0 | 0 | 0 | 0 | 0 | 
| HW-IPv6-Filter-ID(26-251) | 0-1 | 0 | 0 | 0 | 0 | 0 | 
| Cisco-avpair(CISCO-1) | 0-1 | 0 | 0 | 0 | 0 | 0 | 
| Agent-Circuit-Id(DSLFORUM-1) | 0-1 | 0 | 0 | 0 | 0 | 0 | 
| Agent-Remote-Id(DSLFORUM-2) | 0-1 | 0 | 0 | 0 | 0 | 0 | 
Dynamic VLAN:
If dynamic VLAN delivery is configured on the server, authorization information includes the delivered VLAN attribute. After the device receives the delivered VLAN attribute, it changes the VLAN of the user to the delivered VLAN.
The delivered VLAN does not change or affect the interface configuration. The delivered VLAN, however, takes precedence over the VLAN configured on the interface. That is, the delivered VLAN takes effect after the authentication succeeds, and the configured VLAN takes effect after the user goes offline.
To ensure that the RADIUS server delivers VLAN information correctly, all the three RADIUS attributes must be used. In addition, the Tunnel-Type and Tunnel-Medium-Type attributes must be set to the specified values.
RADIUS attribute dictionary defines Huawei proprietary private RADIUS attributes (including attribute number, attribute name, and attribute type) and Huawei vendor ID. When a Huawei device connects to a RADIUS server, the RADIUS server can correctly identify and process Huawei-defined RADIUS attributes after loading the attribute dictionary file. Different products of the same vendor may use the same attribute number to represent different attribute values. Therefore, private RADIUS attributes cannot be loaded on the same RADIUS server.
The following example describes how to install the freeRADIUS server of the Linux SUSE 12.
| Configuration | Step | Description | 
|---|---|---|
| Configure a local server. | Obtain the root permission. | Obtain the root permission on the Linux server where the RADIUS server is installed. | 
| Replace the RADIUS attribute dictionary. | Open the directory where the RADIUS attribute dictionary is saved. | Open the directory /usr/share/freeradius on the RADIUS server. | 
|  | Replace dictionary attributes. | Replace the original dictionary.huawei file with the RADIUS attribute dictionary. You are advised to back up the original file and name the backup file, for example, dictionary.huawei.bak. | 
| - | Verify the configuration. | After the replacement, restart the RADIUS server and verify that the RADIUS private attributes take effect based on onsite services and that the replacement is successful. | 
RADIUS_Attribute.txt
Different vendors support different collections of RADIUS attributes and each vendor may have their private attributes. As a result, RADIUS attributes of different vendors may be incompatible and RADIUS attributes sent between devices from different vendors fail to be parsed. To resolve this issue, the RADIUS attribute disablement and translation functions are often used in interconnection and replacement scenarios.
The RADIUS server may have RADIUS attributes with the same attribute IDs and names as but different encapsulation formats or contents from those on the device. In this case, you can configure the RADIUS attribute disablement function to disable such attributes. The device then does not parse these attributes after receiving them from the RADIUS server, and does not encapsulate these attributes into RADIUS packets to be sent to the server.
Currently, RADIUS attributes supported by Huawei(with the attribute names and IDs supported by Huawei) in a sent or received packet can be disabled on a device.
RADIUS attribute translation is used for achieve compatibility between RADIUS attributes defined by different vendors. For example, a Huawei device delivers the priority of an administrator using the Huawei proprietary attribute Exec-Privilege (26-29), whereas another vendor's NAS and the RADIUS server deliver this priority using the Login-service (15) attribute. In a scenario where the Huawei device and another vendor's NAS share one RADIUS server, users want the Huawei device to be compatible with the Login-service (15) attribute. After RADIUS attribute translation is configured on the Huawei device, the device automatically processes the Login-service (15) attribute in a received RADIUS authentication response packet as the Exec-Privilege (26-29) attribute.
The device supports translation between RADIUS attributes supported and unsupported by Huawei. Table 1-16 describes the translation modes.
The device can translate a RADIUS attribute of another vendor only if the length of the Type field in the attribute is 1 octet.
The device can translate the RADIUS attribute only when the type of the source RADIUS attribute is the same as that of the destination RADIUS attribute. For example, the types of NAS-Identifier and NAS-Port-Id attributes are string, and they can be translated into each other. The types of NAS-Identifier and NAS-Port attributes are string and integer respectively, they cannot be translated into each other.
| Whether the Source RADIUS Attribute Is Supported by Huawei | Whether the Destination RADIUS Attribute Is Supported by Huawei | Supported Translation Direction | Configuration Command (RADIUS Server Template View) | 
|---|---|---|---|
| Supported | Supported | Transmit and receive directions | radius-attribute translate src-attribute-name dest-attribute-name { receive \| send \| access-accept \| access-request \| account-request \| account-response } * | 
| Supported | Not supported | Transmit direction | radius-attribute translate extend src-attribute-name vendor-specific dest-vendor-id dest-sub-id { access-request \| account-request } * | 
| Not supported | Supported | Receive direction | radius-attribute translate extend vendor-specific src-vendor-id src-sub-id dest-attribute-name { access-accept \| account-response } * |