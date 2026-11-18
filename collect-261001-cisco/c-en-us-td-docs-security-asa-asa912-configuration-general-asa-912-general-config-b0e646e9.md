About Logging
System logging is a method of collecting messages from devices to a server running a syslog daemon. Logging to a central syslog server helps in aggregation of logs and alerts. Cisco devices can send their log messages to a UNIX-style syslog service. A syslog service accepts messages and stores them in files, or prints them according to a simple configuration file. This form of logging provides protected long-term storage for logs. Logs are useful both in routine troubleshooting and in incident handling.
The ASA system logs provide you with information for monitoring and troubleshooting the ASA. With the logging feature, you can do the following:
-  
                              		  
                              Specify which syslog messages should be logged.
-  
                              		  
                              Disable or change the severity level of a syslog message.
-  
                              		  
                              Specify one or more locations where syslog messages should be sent, including: 
  -  
                                       				
                                       An internal buffer
  -  
                                       				
                                       One or more syslog servers
  -  
                                       				
                                       ASDM
  -  
                                       				
                                       An SNMP management station
  -  
                                       				
                                       Specified e-mail addresses
  - 
                                       				
                                       Console
  -  
                                       				
                                       Telnet and SSH sessions.
-  
                                       				
                                       
-  
                              		  
                              Configure and manage syslog messages in groups, such as by severity level or class of message.
-  
                              		  
                              Specify whether or not a rate-limit is applied to syslog generation.
-  
                              		  
                              Specify what happens to the contents of the internal log buffer when it becomes full: overwrite the buffer, send the buffer contents to an FTP server, or save the contents to internal flash memory.
-  
                              		  
                              Filter syslog messages by locations, severity level, class, or a custom message list.
Logging in Multiple Context Mode
Each security context includes its own logging configuration and generates its own messages. If you log in to the system or admin context, and then change to another context, messages you view in your session are only those messages that are related to the current context.
Syslog messages that are generated in the system execution space, including failover messages, are viewed in the admin context along with messages generated in the admin context. You cannot configure logging or view any logging information in the system execution space.
You can configure the ASA to include the context name with each message, which helps you differentiate context messages that are sent to a single syslog server. This feature also helps you to determine which messages are from the admin context and which are from the system; messages that originate in the system execution space use a device ID of system, and messages that originate in the admin context use the name of the admin context as the device ID.
Syslog Message Analysis
The following are some examples of the type of information you can obtain from a review of various syslog messages:
-  
                                 		  
                                 Connections that are allowed by ASA security policies. These messages help you spot holes that remain open in your security policies.
-  
                                 		  
                                 Connections that are denied by ASA security policies. These messages show what types of activity are being directed toward your secured inside network.
- 
                                 				
                                 Using the ACE deny rate logging feature shows attacks that are occurring on your ASA.
-  
                                 		  
                                 IDS activity messages can show attacks that have occurred.
-  
                                 		  
                                 User authentication and command usage provide an audit trail of security policy changes.
-  
                                 		  
                                 Bandwidth usage messages show each connection that was built and torn down as well as the duration and traffic volume used.
-  
                                 		  
                                 Protocol usage messages show the protocols and port numbers used for each connection.
-  
                                 		  
                                 Address translation audit trail messages record NAT or PAT connections being built or torn down, which are useful if you receive a report of malicious activity coming from inside your network to the outside world.
Syslog Message Format
Syslog messages begin with a percent sign (%) and are structured as follows:
%ASA Level Message_number: Message_text
Field descriptions are as follows:
| ASA | The syslog message facility code for messages that are generated by the ASA. This value is always ASA. | 
| Level | 1 through 7. The level reflects the severity of the condition described by the syslog message—the lower the number, the more severe the condition. | 
| Message_number | A unique six-digit number that identifies the syslog message. | 
| Message_text | A text string that describes the condition. This portion of the syslog message sometimes includes IP addresses, port numbers, or usernames. | 
Severity Levels
The following table lists the syslog message severity levels. You can assign custom colors to each of the severity levels to make it easier to distinguish them in the ASDM log viewers. To configure syslog message color settings, either choose the Tools > Preferences > Syslog tab or, in the log viewer itself, click Color Settings on the toolbar.
| Table 1. Syslog Message Severity Levels |  |  | 
|---|---|---|
| Level Number | Severity Level | Description | 
|---|---|---|
| 0 | emergencies | System is unusable. | 
| 1 | alert | Immediate action is needed. | 
| 2 | critical | Critical conditions. | 
| 3 | error | Error conditions. | 
| 4 | warning | Warning conditions. | 
| 5 | notification | Normal but significant conditions. | 
| 6 | informational | Informational messages only. | 
| 7 | debugging | Debugging messages only. Log at this level only temporarily, when debugging issues. This log level can potentially generate so many messages that system performance can be affected. | 
| Note | ASA does not generate syslog messages with a severity level of zero (emergencies). | 
Syslog Message Filtering
You can filter generated syslog messages so that only certain syslog messages are sent to a particular output destination. For example, you could configure the ASA to send all syslog messages to one output destination and to send a subset of those syslog messages to a different output destination.
Specifically, you can direct syslog messages to an output destination according to the following criteria:
-  
                                 		  
                                 Syslog message ID number
-  
                                 		  
                                 Syslog message severity level
-  
                                 		  
                                 Syslog message class (equivalent to a functional area)
You customize these criteria by creating a message list that you can specify when you set the output destination. Alternatively, you can configure the ASA to send a particular message class to each type of output destination independently of the message list.
Syslog Message Classes
You can use syslog message classes in two ways:
-  
                                    			 
                                    Specify an output location for an entire category of syslog messages. Use the logging class command.
-  
                                    			 
                                    Create a message list that specifies the message class. Use the logging list command.
The syslog message class provides a method of categorizing syslog messages by type, equivalent to a feature or function of the device. For example, the rip class denotes RIP routing.
All syslog messages in a particular class share the same initial three digits in their syslog message ID numbers. For example, all syslog message IDs that begin with the digits 611 are associated with the vpnc (VPN client) class. Syslog messages associated with the VPN client feature range from 611101 to 611323.
In addition, most of the ISAKMP syslog messages have a common set of prepended objects to help identify the tunnel. These objects precede the descriptive text of a syslog message when available. If the object is not known at the time that the syslog message is generated, the specific heading = value combination does not appear.
The objects are prefixed as follows:
Group = groupname, Username = user, IP = IP_address
Where the group is the tunnel-group, the username is the username from the local database or AAA server, and the IP address is the public IP address of the remote access client or Layer 2 peer.
The following table lists the message classes and the range of message IDs in each class.
| Table 2. Syslog Message Classes and Associated Message ID Numbers |  |  | 
|---|---|---|
| Class | Definition | Syslog Message ID Numbers | 
|---|---|---|
| auth | User Authentication | 109, 113 | 
| — | Access Lists | 106 | 
| — | Application Firewall | 415 | 
| bridge | Transparent Firewall | 110, 220 | 
| ca | PKI Certification Authority | 717 | 
| citrix | Citrix Client | 723 | 
| — | Clustering | 747 | 
| — | Card Management | 323 | 
| config | Command Interface | 111, 112, 208, 308 | 
| csd | Secure Desktop | 724 | 
| cts | Cisco TrustSec | 776 | 
| dap | Dynamic Access Policies | 734 | 
| eap, eapoudp | EAP or EAPoUDP for Network Admission Control | 333, 334 | 
| eigrp | EIGRP Routing | 336 | 
|  | E-mail Proxy | 719 | 
| — | Environment Monitoring | 735 | 
| ha | Failover | 101, 102, 103, 104, 105, 210, 311, 709 | 
| — | Identity-based Firewall | 746 | 
| ids | Intrusion Detection System | 400, 733 | 
| — | IKEv2 Toolkit | 750, 751, 752 | 
| ip | IP Stack | 209, 215, 313, 317, 408 | 
| ipaa | IP Address Assignment | 735 | 
| ips | Intrusion Protection System | 400, 401, 420 | 
| — | IPv6 | 325 | 
| — | Botnet traffic filtering. | 338 | 
| — | Licensing | 444 | 
| mdm-proxy | MDM Proxy | 802 | 
| nac | Network Admission Control | 731, 732 | 
| nacpolicy | NAC Policy | 731 | 
| nacsettings | NAC Settings to apply NAC Policy | 732 | 
| — | Network Access Point | 713 | 
| np | Network Processor | 319 | 
| — | NP SSL | 725 | 
| ospf | OSPF Routing | 318, 409, 503, 613 | 
| — | Password Encryption | 742 | 
| — | Phone Proxy | 337 | 
| rip | RIP Routing | 107, 312 | 
| rm | Resource Manager | 321 | 
| — | Smart Call Home | 120 | 
| session | User Session | 106, 108, 201, 202, 204, 302, 303, 304, 305, 314, 405, 406, 407, 500, 502, 607, 608, 609, 616, 620, 703, 710 | 
| snmp | SNMP | 212 | 
| — | ScanSafe | 775 | 
| ssl | SSL Stack | 725 | 
| svc | SSL VPN Client | 722 | 
| sys | System | 199, 211, 214, 216, 306, 307, 315, 414, 604, 605, 606, 610, 612, 614, 615,701, 711, 741 | 
| — | Threat Detection | 733 | 
| tre | Transactional Rule Engine | 780 | 
| — | UC-IME | 339 | 
| tag-switching | Service Tag Switching | 779 | 
| vm | VLAN Mapping | 730 | 
| vpdn | PPTP and L2TP Sessions | 213, 403, 603 | 
| vpn | IKE and IPsec | 316, 320, 402, 404, 501, 602, 702, 713, 714, 715 | 
| vpnc | VPN Client | 611 | 
| vpnfo | VPN Failover | 720 | 
| vpnlb | VPN Load Balancing | 718 | 
| — | VXLAN | 778 | 
| webfo | WebVPN Failover | 721 | 
| webvpn | WebVPN and AnyConnect Client | 716 | 
| — | NAT and PAT | 305 | 
Custom Message Lists
- 
                                    		  
                                    Severity level
- 
                                    		  
                                    Message IDs
- 
                                    		  
                                    Ranges of syslog message IDs
- 
                                    		  
                                    Message class.
For example, you can use message lists to do the following:
-  
                                 		  
                                 Select syslog messages with the severity levels of 1 and 2 and send them to one or more e-mail addresses.
-  
                                 		  
                                 Select all syslog messages associated with a message class (such as ha) and save them to the internal buffer.
A message list can include multiple criteria for selecting messages. However, you must add each message selection criterion with a new command entry. It is possible to create a message list that includes overlapping message selection criteria. If two criteria in a message list select the same message, the message is logged only once.
Clustering
Syslog messages are an invaluable tool for accounting, monitoring, and troubleshooting in a clustering environment. Each ASA unit in the cluster (up to eight units are allowed) generates syslog messages independently; certain logging commands then enable you to control header fields, which include a time stamp and device ID. The syslog server uses the device ID to identify the syslog generator. You can use the logging device-id command to generate syslog messages with identical or different device IDs to make messages appear to come from the same or different units in the cluster.