iMaster NCE-Campus can identify common applications using the built-in application signature database. When predefined applications cannot meet your requirements, you can customize a new application according to its characteristics.
Currently, the following types of customized applications are supported: SA, Domain Name, and Advance Rule. Table 1 lists the methods of identifying customized applications of each type.
| Table 1 Methods of identifying a customized application |  | 
|---|---|
| Customized Application Type | Application Identification Method | 
|---|---|
| SA | Rules can be created through 3-tuple information, keywords, or both 3-tuple information and keywords. The 3-tuple information consists of the server address, protocol type, and port number. The keyword is the feature of the data packet or data flow of the corresponding application. The keyword uniquely identifies the application. If you create a customized application rule based on the 3-tuple information, the rule must contain at least one IP address or port number. | 
| Domain Name | FPI through association with the DNS function: Rules are created by specifying DNS domain names. If users access applications using domain names, you can configure DNS domain name rules to identify these application packets. For devices running V300, this function applies only to UDP packets, DNS packets cannot be truncated (Truncated cannot be set to 1), and a maximum of 10 IP addresses can be resolved in a packet. | 
|  | FPI based on advanced ACL rules: Rules are created based on 5-tuple and DSCP information. The 5-tuple information includes the protocol type, source IP address, destination IP address, source port number, and destination port number of a packet. If you know the 5-tuple and DSCP information of service packets, you can configure rules based on 5-tuple and DSCP information to identify these packets. When you use this method to configure a customized application rule, at least one of the source IP address, destination IP address, source port, destination port, and DSCP information must be configured. | 
| Advance Rule |  | 
For different models of ARs, refer to Table 2 to view the application specifications of the SA type and refer to Table 4 and Table 5 to view application specifications of the FPI types (Domain Name and Advance Rule).
For different models of firewall gateways, refer to Table 3 to view the application specifications of the SA type and refer to Table 6 to view the application specifications of the Advance Rule type.
The following tables describe the maximum specifications supported by the controller for applications of different types, and list device models whose application specifications are lower than the maximum specifications supported by the controller. If application specifications of a device model exceed those supported by the controller, the specifications supported by the controller take effect.
| Table 2 SA application specifications |  |  |  | 
|---|---|---|---|
| Device Model | Maximum Number of Customized Applications | Maximum Number of Rules for a Single Application | Maximum Number of Rules | 
|---|---|---|---|
| Device models are not distinguished. | 256 | 8 | 512 | 
| Table 3 SA specifications (firewall gateway) |  |  |  | 
|---|---|---|---|
| Device Type | Maximum Number of Customized Applications | Maximum Number of Rules for a Single Application | Maximum Number of Rules | 
|---|---|---|---|
| USG6510F-DK, USG6510F-DL, USG6000F-E06-DL, USG6510F-DPL, USG6530F-D, USG6000F-E12-D, USG6530F-DL, USG6560F-D, USG6530F-DPL, USG6510F-D, USG6000F-E06-D | 128 | 8 | 256 | 
| Other models | 256 | 8 | 512 | 
| Table 4 Specifications for applications of the Domain Name type |  |  |  | 
|---|---|---|---|
| Device Model | Maximum Number of Customized Applications | Maximum Number of Rules for a Single Application | Maximum Number of Rules | 
|---|---|---|---|
| AR611, AR611-S, AR611W, AR611W-S, AR611W-LTE4CN, AR611W-LTE6EA | 128 | 128 | 128 | 
| AR617, AR617-LTE4EA, AR617VW, AR617VW-LTE4, AR617VW-LTE4EA |  |  |  | 
| AR631I-LTE4EA, AR631I-LTE4CN |  |  |  | 
| AR651C and AR651F-Lite |  |  |  | 
| Other models | 256 |  | 256 | 
| Table 5 Application specifications of the Advance Rule type (AR) |  |  |  | 
|---|---|---|---|
| Device Model | Maximum Number of Customized Applications | Maximum Number of Rules for a Single Application | Maximum Number of Rules | 
|---|---|---|---|
| AR611, AR611-LTE4EA, AR611-S AR611W, AR611W-S, AR611W-LTE4CN, AR611W-LTE6EA | 128 | 8 | 256 | 
| AR617, AR617-LTE4EA, AR617VW, AR617VW-LTE4, AR617VW-LTE4EA |  |  |  | 
| AR631I-LTE4EA, AR631I-LTE4CN |  |  |  | 
| AR651C and AR651F-Lite |  |  |  | 
| AR6280/AR6300 series | Common application: 256 Extended application: 1792 | Common application: 8 Extended application: 1024 | Common application: 1024 Extended application: 15360 | 
| Other models | 256 | 8 | 1024 | 
| Table 6 Application specifications of the Advance Rule type (firewall gateway) |  |  |  | 
|---|---|---|---|
| Device Type | Maximum Number of Customized Applications | Maximum Number of Rules for a Single Application | Maximum Number of Rules | 
|---|---|---|---|
| USG6510F-DK, USG6510F-DL, USG6000F-E06-DL, USG6510F-DPL, USG6530F-D, USG6000F-E12-D, USG6530F-DL, USG6560F-D, USG6530F-DPL, USG6510F-D, USG6000F-E06-D | 128 | 8 | 256 | 
| USG6520F-K, USG6525F, USG6555F, USG6560F-K, USG6565F, USG6585F, USG6590F-K, USG6585F-B | 256 | 8 | 512 | 
| USG6600F series, USG6700F series | 256 | 8 | 1024 | 
Table 7 lists the specifications of customized applications supported by iMaster NCE-Campus.
| Table 7 Specifications of customized applications |  |  |  | 
|---|---|---|---|
| Application Type | Maximum Number of Customized Applications | Maximum Number of Rules for a Single Application | Maximum Number of Rules | 
|---|---|---|---|
| SA | 256 | 8 | 512 | 
| Domain Name | 256 (total number of applications of the Domain Name and Advance Rule types) | 128 | 256 | 
| Advance Rule (for common applications) |  | 8 | 1024 | 
| Advance Rule (for extended applications) | 1792 | 1024 | 15360 | 
Configuring extended configurations at sites where AR6280 or AR6300 series devices are deployed has the following restrictions:
The AR6700V-L supports this function starting from V600R023C10.
Customized applications of the Domain Name type are supported by devices running V600 since V600R023C00, and supported by AR6700V-L devices since V600R023C10.
Customized applications of the Advance Rule type are supported by devices running V600 since V600R022C00 and are supported by AR6700V-L devices since V600R023C10.
After a customized application is created, iMaster NCE-Campus delivers the customized application rule to its managed devices. If multiple customized applications are created, the corresponding rules are delivered based on their configuration sequence. That is, the rule of a customized application that is configured first is delivered first to devices. If an application packet matches rules of multiple customized applications, the rule of the customized application that is delivered first takes effect. Once devices find the matching application for traffic, they stop matching the traffic with other applications.
For example, application traffic traffic1 matches rules of both the customized applications APP1 and APP2. If the rule of APP1 is delivered to a device earlier than that of APP2, the device preferentially matches traffic1 with APP1 and does not continue to match the traffic with APP2.
| Table 8 Follow-up procedure of customized applications |  |  | 
|---|---|---|
| Function | Operation Scenario and Constraint | Procedure | 
|---|---|---|
| Viewing details about a customized application | You can view detailed information about a customized application. |  | 
| Modifying a customized application | You cannot modify Application Name of a customized application that has been referenced by an application group. |  | 
| Cloning a customized application | You can quickly create a customized application by simply modifying an existing customized application. |  | 
| Deleting a customized application | You cannot delete a customized application that has been referenced by an application group. |  | 
| Table 9 Parameters on the Customized Application page |  |  |  |  |  | 
|---|---|---|---|---|---|
| Parameter |  |  |  | Description | Data Plan Required or Not | 
|---|---|---|---|---|---|
| Name |  |  |  | Name of a customized application. The value can contain only digits, letters, and underscores (_). The name of a customized application of the SA type can contain 1 to 29 characters, and that of a customized application of a non-SA type can contain 1 to 28 characters. | Y | 
| Description |  |  |  | Description of a customized application's function. | - | 
| Application Group |  |  |  | Application group to which the application is to be added. It must be an existing application group. You can also add an application to an application group after customizing it. | Y | 
| Application scope |  |  |  | The value can be Common or Extended. Extended is applicable to the scenario where the specifications of applications of the Advance Rule type need to be expanded. Currently, only AR6280 and AR6300 series devices support this function. | Y | 
| Application Type |  |  |  | You can select an application type to define a rule for identifying application packets. The application types include SA, Domain Name, and Advance Rule. The configurable rule parameters vary according to the application type. If Application scope is set to Extended, you can only create customized applications of the Advance Rule type. | Y | 
| Rule | Name |  |  | Name of a rule defined for identifying application packets. A rule contains the protocol number and port number used by an application, and other basic attributes. | Y | 
|  | Description |  |  | Description of a rule. | - | 
|  | When Application Type is set to SA | IP/Port |  | IP address and port number of application packets matching the rule. The system does not distinguish source and destination IP addresses of SA application packets, as well as source and destination port numbers. If an IP address and a port number are specified together in a rule, the relationship between IP and Port is And, that is, application packets only with the specified IP address and port number match this rule. For details about the value ranges of the IP address and port number, see the prompt message on the web UI.  NOTE:  When configuring a rule for V600 devices, you need to set this parameter to the destination IP address and destination port number of application packets. Otherwise, application packets cannot be matched. | Y | 
|  |  | Protocol |  | Transport layer protocol of a customized application rule. The options include All, TCP, and UDP. | Y | 
|  |  | Signature | Signature | Signature information. Data packets of some applications contain the same character string, which is regarded as a signature. | - | 
|  |  |  | Context | You can select the packet- or flow-based mode for signature identification: In the packet-based mode, the system checks every packet of applications. In flow-based mode, the system only checks the first packet in the application data flow and does not check the subsequent packets if the system detects that the subsequent packets belong to the same data flow based on the 5-tuple information. | - | 
|  |  |  | Direction | Direction of packets to be identified. You can configure a rule to identify the signatures only in request or response packets, or in both of them. | - | 
|  |  |  | Plain-text String | Signature string, which is case-sensitive. | - | 
|  | When Application Type is set to Domain Name | Domain Name |  | Domain name of a customized application. | Y | 
|  | When Application Type is set to Advance Rule | Source IP/Source Port |  | Source IP address and port number of application packets matching the rule. If the source IP address and source port number are specified together in a rule, the relationship between Source IP and Source Port is And, that is, application packets only with the specified source IP address and source port number match this rule. For details about the value ranges of the IP address and port number, see the prompt message on the web UI. | Y | 
|  |  | Destination IP/Destination Port |  | Destination IP address and destination port number of application packets matching the rule. If the destination IP address and destination port number are specified together in a rule, the relationship between Destination IP and Destination Port is And, that is, application packets only with the specified destination IP address and destination port number match this rule. In most cases, the IP address of an application server is a fixed public IP address. This allows the system to identify application packets based on the destination IP address. | Y | 
|  |  | DSCP |  | You can set the packet priority by specifying a DSCP value. The options include Default (which is 0), CS (low priority), AF (medium priority), EF (high priority), and User Defined. V600 devices do not support this function. | Y | 
|  |  | Protocol |  | Transport layer protocol of a customized application rule. The options include All, TCP, and UDP. | Y |