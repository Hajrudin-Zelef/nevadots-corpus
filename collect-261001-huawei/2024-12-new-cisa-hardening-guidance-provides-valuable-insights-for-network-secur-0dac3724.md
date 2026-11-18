New CISA Hardening Guidance Provides Valuable Insights for Network Security Engineers
Recent guidance from CISA and the FBI highlights best practices to monitor and harden network infrastructure. The guidance, published in response to high-profile attacks on telecom infrastructure, is applicable to a wider audience. This blog unpacks important points and explains how Tenable products can help with compliance scans.
In November, the Cybersecurity and Infrastructure Security Agency (CISA) and Federal Bureau of Investigation (FBI) released a joint statement concerning an investigation into cyberattacks on commercial telecommunications infrastructure. The ongoing investigation centers on threat actors believed to be affiliated with the People’s Republic of China (PRC) government. In response to the cyberattacks, U.S. and international government agencies, including CISA and the FBI, authored joint guidance to help network defenders improve network visibility and security. This guidance highlights the importance of monitoring and alerting, but also provides specific ways to strengthen cybersecurity with increased configuration management and strong identity hygiene.
What’s this all about?
The U.S. government has been monitoring PRC-sponsored groups such as Volt Typhoon and Salt Typhoon because it suspects they may be preparing for a large-scale disruption of U.S. critical infrastructure. A press release from mobile telecom provider T-Mobile highlights the activity that it has identified, the controls that it had in place to help prevent a greater threat, as well as how it is collaborating with the authorities’ investigation. According to U.S. government officials, at least eight telecommunications companies have been targeted so far but there may be more.
The new guidance can help prevent these attacks, whose main goal is to reportedly carry out cyber espionage activities on behalf of the Chinese government by, among other things, stealing customer call-records data. The guidelines pair well with recommendations in Center for Internet Security (CIS) Benchmarks for specific network devices. CIS Benchmarks are written and maintained by industry professionals with the goal of simplifying the implementation of security controls to help mitigate risk. By using CIS Benchmarks, network and security engineers can identify and harden configurations, and establish a more secure posture as suggested by the guidance.
We’ll be taking a closer look at the specific sections in the recent guidance and highlight CIS Benchmark recommendations that align with these objectives.
Strengthening visibility
This section highlights monitoring and alerting best practices. It breaks these guidelines into two sets of tasks: one for network engineers and another one for network defenders. However, the common goal is to help them find and trigger alerts on misconfigurations, changes and user account activity. One key recommendation is to use an independent and centralized log-storage environment, and if possible, a security information and event management (SIEM) solution built specifically to analyze the logs to produce alerts.
Alerting should be focused on configuration changes; configurations that don’t meet specific criteria; and open ports or enabled services. In addition, devices that accept traffic from outside of the network (external facing) should be reviewed to ensure that only necessary services are accessible to and from the internet.
Examples of centralized logging criteria can be found in CIS Benchmarks for Cisco, Fortinet, Juniper Networks and Palo Alto Networks devices:
- Cisco: “Ensure Syslog Logging is configured”
- Fortigate: “Centralized Logging and Reporting”
- Juniper: “Ensure logging data is monitored”
- Palo Alto: “Syslog logging should be configured”
This section also focuses on monitoring user- and service-account logins to ensure that anomalous login activity is detected and prevented. Unused accounts should be disabled whenever possible. Some examples of this criteria can be found in CIS Benchmarks for Check Point Software and Palo Alto Networks devices:
Hardening systems and devices
This section aims to help reduce risk by limiting access to the network and network devices; ensuring that communication is encrypted and secure; and providing more direct guidance with regards to Cisco-based devices. This section includes recommendations regarding access control and network segmentation, provides specific protocol guidelines (such as using only SNMPv3 when SNMP is necessary), and details what is considered to be “strong” encryption.
First, network segmentation helps to limit movement across the network and to make it easier to inspect inbound and outbound traffic. It also helps to maintain a DMZ to contain the services that must face externally (towards the internet) and prevent direct access to backend resources and networks. Segmentation also involves creating and using VLANs, and the recommendation is that these VLANs should be used to group together devices of a similar nature, which is common in most networks. In addition to segmenting the network, the authoring agencies also recommend adopting Transport Layer Security-everywhere using strong algorithms. These guidelines can help keep threat actors out of corporate networks, as well as ensure that these actors are limited in what they can do and/or see if they manage to penetrate the outermost defenses.
Another component of segmentation is initializing a default-deny access-control list (ACL), which can be done at the firewall level. This is important for all traffic types, but especially so when isolating management traffic for network devices. Most physical network devices, such as routers and switches, have dedicated ports for management traffic that can be attached to a physically segmented network in order to limit administrative access. Further controls on lateral movement are also recommended for the management network, and it is advisable to not manage devices directly from the internet. Some examples of segmentation and ACL firewall configurations can be found in CIS Benchmarks for Cisco, Juniper Networks, and Palo Alto Networks products.
The guidance further identifies numerous insecure protocols and services and notes that they should be disabled. These include FTP, TFTP, SSHv1, HTTP, and SNMP v1/v2. Additionally, any network protocols or services in use should require authentication when available, including routing protocols. Meanwhile, you should use SNMP Version 3 with encryption and authentication. Having centralized authentication, authorization, and accounting (AAA) logging is emphasized here, in addition to prior mentions of syslog configuration. Examples of identifying and disabling protocols can be found in several CIS Benchmarks for Cisco, Fortinet, Juniper Networks, and Palo Alto Networks products:
This section highlights specific criteria for Cisco devices. Disabling the Smart Install and Guest Shell features is recommended, as is disabling Telnet in favor of SSH. Specific commands are also provided to disable HTTP-only access so that device management is performed over HTTPS instead. If UI access is not necessary, the secure service should also be disabled. The specific password type recommended is type-8 when possible, and type-6 encryption for securing the Terminal Access Controller Access-Control System + (TACACS+) key. The document also links to the hardening guide for Cisco IOS XE and a guide for securing NX-OS devices.
Secure by design
The secure-by-design concept helps introduce the security conversation earlier in the development lifecycle. This approach helps ensure that security considerations are addressed at the beginning of the product lifecycle. Customers should make sure that products they plan to buy adhere to this principle. CISA has more information on its “Secure by Design” site. Tenable has committed to a secure-by-design approach, as can be seen in a recent initiative reported on here and here.
How Tenable can help
This overview is meant to help give network and security engineers a summary of the best practices, as well as provide insight on how CIS Benchmarks cover many of the guidance’s topics. Still, engineers should read the guidance to ensure they fully understand the material and how it relates to their own networks. It’s equally important to map out the network and understand what devices exist and where they are placed. However, this is only a first step in securing the network.
Tenable has several products, such as Tenable Vulnerability Management, Tenable Security Center, and Nessus that support auditing a wide array of devices and operating systems using CIS Benchmarks. These products could help with maintaining control over risk factors that threat actors often attempt to exploit. Tenable audits are written to test for the criteria of each automated recommendation in CIS Benchmarks. After an evaluation is run against the target, a result is provided as well as remediation text from the CIS Benchmark so that engineers can remediate and harden the device or operating system.
Tenable provides audit files for the following CIS Benchmarks to help organizations assess device configurations:
- CIS Check Point Firewall Benchmark v1.1.0 – Level 1, Level 2
- CIS Cisco ASA 9.x Firewall Benchmark v1.1.0 – Level 1, Level 2
- CIS Cisco Firewall v8.x Benchmark v4.2.0 – Level 1
- CIS Cisco IOS XE 16.x Benchmark v2.1.0 – Level 1, Level 2
- CIS Cisco IOS XE 17.x Benchmark v2.1.1 – Level 1, Level 2
- CIS Cisco IOS XR 7.x v1.0.0 – Level 1, Level 2
- CIS Cisco NX-OS Benchmark v1.1.0 – Level 1, Level 2
- CIS Fortigate 7.0.x Benchmark v1.3.0 – Level 1, Level 2
- CIS Juniper OS Benchmark v2.1.0 – Level 1, Level 2
- CIS Palo Alto Firewall 10 Benchmark v1.2.0 – Level 1, Level 2
- CIS Palo Alto Firewall 11 Benchmark v1.1.0 – Level 1, Level 2
These CIS Benchmarks align with the intent of the CISA hardening guidance. The example below highlights the CIS Cisco IOS XE 17.x v2.1.1 CIS Benchmark, and how it relates to the CISA hardening guidance:
Section 1.1 – Authentication, Authorization and Accounting (AAA) configuration
- Strengthening visibility as AAA logging supports user account login monitoring, and tracking changes
- Hardening systems and devices by providing identity management and policy enforcement
Section 1.2 – Access Rules for device administration
- Hardening systems and devices by restricting device management, and ensuring sessions are limited
Section 1.3 – Banner Rules to communicate legal rights to users
- Strengthening visibility by informing users they are subject to monitoring, and the event logs can support prosecution
Section 1.4 – Password Rules to enforce secure credentials and password lifecycle
- Hardening systems and devices by ensuring strong passwords are utilized, and passwords are securely stored
Section 1.5 – SNMP Rules provides guidance for secure configuration parameters
- Hardening systems and devices by ensuring SNMP is disabled, or is configured with secure parameters
Section 2.1 – Global Service Rules to reduce attack surface and disable unnecessary services
- Hardening systems and devices by disabling unnecessary, unused, exploitable, or plaintext services and protocols
Section 2.2 – Logging Rules configures log collection and forwarding
- Strengthening visibility by collecting event logs, and forwarding to a central log collection source
- Hardening systems and devices by forwarding logs to a central log collection source
Section 2.3 – NTP Rules ensures system time is provided by a single, consistent source
- Strengthening visibility by ensuring a consistent time source for event logs
- Hardening systems and devices by requiring that NTP is authenticated
Section 2.4 – Lookback Rules for configuring device initiated connections to supporting services such as AAA, SYSLOG, or NTP
- Hardening systems and devices by ensuring that traffic is initiated from a specific source, which can be used to set ACLs/filtering
Section 3.1 – Routing Rules to disable unneeded services
- Hardening systems and devices by disabling unneeded services such as source routing
Section 3.2 – Border Router Filtering defines filtering between internal and external networks
- Hardening systems and devices by implementing a strategy to control inbound and egress traffic
Section 3.3 – Neighbor Authentication configures routing protocol authentication
- Hardening systems and devices by requiring routing protocols are authenticated
Learn more
- Tenable Nessus
- Tenable Security Center
- Tenable Vulnerability Management
- Tenable Research audits
- Who is CIS?
- “Enhanced Visibility and Hardening Guidance for Communications Infrastructure” joint publication from various U.S. and international government agencies