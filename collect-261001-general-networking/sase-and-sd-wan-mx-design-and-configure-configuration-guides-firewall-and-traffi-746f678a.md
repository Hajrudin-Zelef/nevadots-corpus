MX Firewall Settings
Click 日本語 for Japanese
This article provides an overview of what traditional and next-generation firewalls are, in addition to the configuration and capabilities of the MX Security & SD-WAN Appliance.
The firewall settings page in the Meraki Dashboard is accessible via Security & SD-WAN > Configure > Firewall. On this page you can configure Layer 3 and Layer 7 outbound firewall rules, publicly available WAN appliance services, port forwarding, 1:1 NAT mappings, and 1:Many NAT mappings.
For information regarding firewall rules for communication between Meraki devices and the Meraki cloud, please refer to the article on Firewall Rules for Cloud Connectivity.
Learn more with these free online training courses on the Meraki Learning Hub:
Comparing Next Generation Firewalls to Traditional Firewalls
When selecting a firewall for your network you will encounter a myriad of potential options. Some will be next-generation, some will be traditional. Let's take a look at the key differences below.
Features
Firewall selection typically begins with a required feature set. Traditional firewalls have a limited feature set while next-generation firewalls have an extended feature set. More details on the difference in features available are provided below.
Performance
Enabling additional features such as detailed client tracking, intrusion detection/prevention, and stateful inspection will impact performance. These features require additional resource utilization (memory and CPU) to perform the underlying packet scanning that enables these features. Comparing the performance of a next-generation firewall to a traditional firewall is not typically possible given the default feature set enabled on each.
Management
A traditional firewall is managed via a CLI or GUI that is hosted on the device. Next-generation firewalls typically have Cloud or Controller management capabilities.
What is a traditional firewall?
A traditional firewall will provide basic access control list (ACL) capabilities at Layers 3 & 4 in addition to Layer 3 routing and flow state tracking capabilities. These features are typically enabled by default, though they may not be configured out-of-the-box. Additional configuration capabilities are fairly limited, as many of the foundational features that enable Software Defined Routing (SD-Routing) are not available on traditional firewalls.
Standard Feature List
A standard firewall's default feature set will include functionality such as:
- Stateful or stateless flow tracking
- Standard firewalling capabilities such as:
    
  - Access Control Lists (ACLs) based on Layer 3 & Layer 4 information
- Network Address Translation (NAT) and Port Address Translation (PAT)
Extended Feature List
- Traffic Shaping
    
  - Rate limiting / policing
- Quality of Service
    
  - DSCP & CoS Honoring / Marking
- Virtual Private Network (VPN)
What is a Next Generation Firewall?
A next-generation firewall (NGFW) is a network security device that provides capabilities beyond a traditional, stateful firewall. While a traditional firewall typically provides stateful inspection of incoming and outgoing network traffic, a next-generation firewall includes additional features like application awareness and control, integrated intrusion prevention, and cloud-delivered threat intelligence. NGFW appliances will also include Software Defined Networking (SDN) capabilities which traditional firewalls cannot achieve without orchestration and use of additional integrations.
The standard feature set of a NGFW is a 'generation ahead' of a traditional firewall. These standard features are the foundation upon which Software Defined Networking (SDN) and threat protection implementations are built.
Standard Feature List
A next-generation firewall's default feature set will include functionality such as:
- Stateful or stateless flow tracking
- Standard firewalling capabilities such as:
    
  - Access Control Lists (ACLs) based on Layer 3 & Layer 4 information
- Network Address Translation (NAT) and Port Address Translation (PAT)
- Deep packet inspection (DPI)
- URL Filtering
- Traffic Shaping
    
  - Rate limiting / policing
- Quality of Service (QoS)
    
  - DSCP & CoS Honoring / Marking
- Software Defined WAN (SD-WAN) and SD-Routing decision capabilities
- Client and host tracking (Layer 3 - Layer 7)
Extended Feature List
Below are a few features which we would typically expect to find
- Stateful packet inspection
- Virtual Private Network (VPN)
- Intrusion Detection and Prevention (IPS)
- Malware Protection
Outbound Rules
Outbound rules are Layer 3 firewall ACLs that permit or deny traffic between local VLANs or from the LAN to the Internet. Rules can match protocol, source IP address or subnet, source port, destination IP address, subnet, FQDN, domain name, and destination port.
These rules do not apply to Auto VPN traffic. To configure firewall rules between VPN peers, use the Site-to-site VPN Firewall Settings.
To add a rule, click Add a rule and configure the following fields:
- Policy: Select whether matching traffic is allowed or denied.
- Rule description: Add an optional description or comment for the rule.
- Protocol: Select TCP, UDP, ICMPv4/ICMPv6, or Any.
- Source: Enter an IP address, CIDR subnet, comma-separated list, or Any. Source IPs and subnets must match subnets configured on the MX Addressing & VLANs page.
- Destination: Enter an IP address, CIDR subnet, FQDN, domain name, comma-separated list, or Any.
- Src Port / Dst Port: Enter a port or port range. Multiple individual ports can be entered as a comma-separated list.
Considerations and Limitations
- Outbound connections are allowed by default. Add a default deny rule if the deployment requires explicit allow-list behavior.
- Firewall rules only apply to traffic passing through the MX. They do not apply to traffic sourced from or destined to the MX itself, such as LDAP binds or inbound Client VPN connections.
- MX VLAN interface IPs and MX WAN IPs are not evaluated as source or destination matches in allow or deny rules.
- Rules are flow-based. Existing flows may continue until they time out or the MX is rebooted. Rules apply immediately to new outbound Internet flows.
- In a hub-and-spoke VPN topology where the spoke uses the Hub as its default route, spoke traffic is evaluated against the Hub’s outbound Layer 3 firewall rules.
    
  - For information on Hub-Spoke topology please refer to Configuring Hub-and-spoke VPN Connections on the MX Security Appliance.
- Layer 3 firewall rules on this page are stateful. Layer 7 firewall rules are stateful starting in MX 26.1.
- In NAT/Routed mode, inbound connections are denied by default except ICMP traffic to the appliance. Use port forwarding or NAT policies to allow inbound traffic.
- To determine the priority of Layer 3 vs Layer 7 rules, please refer to Layer 3 and 7 Firewall Processing Order.
Layer 7 firewall rules are stateful starting in MX 26.1.
Template Firewall Rules
Additional options are available when configuring firewall rules on a configuration template. For details, see the Firewall rules for templates section of the Configuration Templates page.
Cellular Failover Rules
These cellular firewall rules are evaluated separately from, and after, the existing L3 outbound firewall rule table when the appliance has failed over to a cellular modem uplink. For example, an Allow Any Any rule in the L3 outbound firewall rules allows the packet past that ruleset, but the packet is still subsequently evaluated against the cellular firewall rules. If a packet is denied by the L3 outbound firewall rule table, the cellular firewall rule table is not checked. This can be useful for restricting cellular traffic to business-critical uses and helping prevent unnecessary cellular overages.
Note: As the cellular failover rules are appended, if a deny any any is applied to the general Layer 3 firewall rules, cellular failover rules will never trigger.
Note: Prior to MX version 26.x, the default Inbound Cellular Failover Rule was to allow all. In MX version 26.x and above, the default Inbound Cellular Failover Rule is to deny all.
FQDN Support
Fully qualified domain names can be configured in the Destination field.
Note: At this time, cellular firewall rules do not support the use of FQDNs.
FQDN is not supported as a source or on inbound firewall rules.
FQDN-based L3 firewall rules are implemented based on snooping DNS traffic. When a client device attempts to access a web resource, the MX will track the DNS requests and response to learn the IP of the web resource returned to the client device.
There are several important considerations for utilizing and testing this configuration:
- The MX must see a client DNS request and the server's response to learn the proper IP mapping. The DNS request does not need to be from a specific client, the MX will map the FQDN based on the DNS response, regardless of which client made the DNS request. The communication between the client and DNS server can be an inter-VLAN, VPN, or NATed traffic flow, but cannot be intra-VLAN (this DNS traffic is not snooped).
- In some cases, a client device may already have IP information about the web resource it is attempting to access. This could be due to the client having cached a previous DNS response, or a local statically configured DNS entry on the device. The MX may not be able to properly block or allow communications to the web resource in these cases if the client devices do not generate a DNS request for the MX to inspect. Clients caching DNS query responses for longer than the TTL reported in the response must send new queries in order to maintain the IP mapping for that domain.
- FQDN rules imply a wildcard when no subdomain is used by prepending "*" to the domain.tld. This wildcard is not shown on the Dashboard but is visible in syslog messages if syslog is configured for a network. For example, a rule to permit "yahoo.com" would permit any subdomain under yahoo.com such as mail.yahoo.com. Permitting "mail.yahoo.com" in the rule would only permit mail.yahoo.com and not the TLD or other subdomain of yahoo.com.
MX will not be able to snoop TCP-based or encrypted DNS traffic.
An example configuration is included below:
To ensure successful operation, DNS traffic must be allowed by the MX's Layer 3 firewalls. Blocking DNS will result in the MX being unable to learn hostname and IP address mappings and, subsequently, prevent it from blocking or allowing traffic as expected.
Behavior when a Canonical Name (CNAME) record point to a different Top Level Domain (TLD)
If your lookup uses a CNAME that resolves to a different Top Level Domain, FQDN rules will not be applied to that traffic. (Example: Lookup for "update.microsoft.com" resolves to "update.microsoft.com.akadns.net")
WAN Appliance Services
- ICMP Ping: Use this setting to allow the WAN appliance to reply to inbound ICMP ping requests coming from the specified address(es). Supported values for the remote IP address field include None, Any, or a specific IP range (using CIDR notation). You can also enter multiple IP ranges separated by commas. To add specific IP addresses rather than ranges, use the format X.X.X.X/32. This setting has never applied to cellular. To allow ICMP to cellular uplink on MX version 26.x and above, create an Inbound Cellular Failover Rule allowing ICMP.
- Web (local status & configuration): Use this setting to allow or disable access to the local management page (wired.meraki.com) via the WAN IP of the WAN appliance. Supported values for the remote IPs field are the same as for ICMP Ping.
- SNMP: Use this setting to allow SNMP polling of the appliance from the WAN. Supported values for the remote IPs field are the same as for ICMP Ping.
The WAN appliance services configuration section is removed for WAN appliances operating in Passthrough or VPN Concentrator mode as these are configured in the inbound firewall.
Layer 7 Firewall Rules
Using Meraki's unique Layer 7 traffic analysis technology, it is possible to create firewall rules to block specific web-based services, websites, or types of websites without specifying IP addresses or port ranges. This can be particularly useful when applications or websites use more than one IP address, or when their IP addresses or port ranges are subject to change.
It is possible to block applications by category (e.g. 'All video & music sites') or for a specific type of application within a category (e.g. only iTunes within the 'Video & music' category). The figure below illustrates a set of Layer 7 firewall rules that includes both blocking entire categories and blocking specific applications within a category:
It is also possible to block traffic based on HTTP hostname, destination port, remote IP range, and destination IP/port combinations.
The remote IPs cannot be blocked inbound for L2TP VPN or AnyConnect VPN.
Geo-IP Based Firewall Rules
The Layer 7 firewall can block traffic based on the destination country of outbound traffic and the source country of return traffic. Geo-IP classification is provided by MaxMind.
To configure a Geo-IP firewall rule, create a Layer 7 firewall rule and select Countries... from the Application drop-down. You can block traffic to selected countries, or block traffic to all countries except the countries you specify.
Considerations and Limitations
- Geo-IP rules supports IPv4 traffic only. IPv6 traffic is not evaluated against Geo-IP firewall rules.
- Geo-IP rules are supported only on physical MX appliances with an Advanced Security license. They are not supported on Z3 devices or vMX appliances.
- Geo-IP rules do not support exceptions for specific IP ranges within a blocked country. For more granular policy control, use an organization-wide Group Policy, which supports both Layer 3 and Layer 7 firewall rules in a single ruleset.
- Geo-IP rules also apply to internally routed traffic. If a subnet configured on Security & SD-WAN > Configure > Addressing & VLANs geolocates to a blocked country, the MX will drop traffic sourced from that subnet.
Forwarding rules
Use this area to configure port forwarding rules and 1:1 NAT mappings as desired.
Port forwarding
Use this option to forward traffic destined for the WAN IP of the MX on a specific port to any IP address within a local subnet or VLAN. Click Add a port forwarding rule to create a new port forward. You need to provide the following:
- 
    Description: A description of the rule.
- Uplink: Listen on the public IP of Internet 1, Internet 2, or both.
- Protocol: TCP or UDP.
- Public port: Destination port of the traffic that is arriving on the WAN.
- LAN IP: Local IP address to which traffic will be forwarded.
- Local port: Destination port of the forwarded traffic that will be sent from the MX to the specified host on the LAN. If you simply wish to forward the traffic without translating the port, this should be the same as the Public port.
- Allowed remote IPs: Remote IP addresses or ranges that are permitted to access the internal resource via this port forwarding rule.
You can also create a port forwarding rule to forward a range of ports. However, the range configured in the Public port field must be the same length as the range configured in the Local port field. The public ports will be forwarded to their corresponding local ports within the range. For instance, if you forward TCP 223-225 to TCP 628-630, port 223 would be translated to 628, port 224 would be translated to 629, and port 225 would be translated to 630.
1:1 NAT
Use this option to map an IP address on the WAN side of the MX (other than the WAN IP of the MX itself) to a local IP address on your network. Click Add a 1:1 NAT mapping to create a new mapping. You need to provide the following:
- Name: A descriptive name for the rule
- Public IP: The IP address that will be used to access the internal resource from the WAN.
- LAN IP: The IP address of the server or device that hosts the internal resource that you wish to make available on the WAN.
- Uplink: The physical WAN interface on which the traffic will arrive.
- Allowed inbound connections: The ports this mapping will provide access on, and the remote IPs that will be allowed access to the resource. To enable an inbound connection, click Allow more connections and enter the following information:
    
  - Protocol: Choose from TCP, UDP, ICMP ping, or any.
  - Ports: Enter the port or port range that will be forwarded to the host on the LAN. You can specify multiple ports or ranges separated by commas.
  - Remote IPs: Enter the range of WAN IP addresses that are allowed to make inbound connections on the specified port or port range. You can specify multiple WAN IP ranges separated by commas.
You can move a configured rule up or down in the list using the + icon. Click the X to remove it entirely.
Creating a 1:1 NAT rule does not automatically allow inbound traffic to the public IP listed in the NAT mapping. By default all inbound connections are denied. You will have to configure Allowed inbound connections as described above to allow the inbound traffic.
1:Many NAT
1:Many NAT, also known as Port Address Translation (PAT), is more flexible than 1:1 NAT. It allows you to specify one public IP that has multiple forwarding rules for different ports and LAN IPs. To add a 1:Many NAT listener IP, click Add 1:Many IP.
- Public IP: The IP address that will be used to access the internal resource from the WAN.
- Uplink: The physical WAN interface on which the traffic will arrive.
A 1:Many NAT entry will be created with one associated forwarding rule. To add additional rules, click Add a port forwarding rule under the existing rule or rules for a particular 1:Many entry.
- 
    Description: A description of the rule.
- Protocol: TCP or UDP.
- Public port: Destination port of the traffic that is arriving on the WAN.
- LAN IP: Local IP address to which traffic will be forwarded.
- Local port: Destination port of the forwarded traffic that will be sent from the MX to the specified host on the LAN. If you simply wish to forward the traffic without translating the port, this should be the same as the Public port.
- Allowed remote IPs: Remote IP addresses or ranges that are permitted to access the internal resource via this port forwarding rule.
Bonjour Forwarding
Use this feature to allow Bonjour to work between VLANs. Click Add a Bonjour forwarding rule to create a new forwarding rule.
- Description: Specify a name for the rule.
- Service VLANs: Select one or more VLANs where network services are running. Bonjour requests from the Client VLANs will be forwarded to these VLANs.
- Client VLANs:  Select one or more VLANs from which client Bonjour requests can originate. Requests on these VLANs will be forwarded to the Service VLANs. The list of services that can be forwarded include:
    
  - All services
  - AirPlay
  - Printers
  - AFP (Apple file sharing)
  - Scanners
  - iChat