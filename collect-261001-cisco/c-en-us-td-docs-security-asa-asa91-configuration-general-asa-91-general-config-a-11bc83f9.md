Configuring Objects 
    
    
 
    
 
  This chapter describes how to configure reusable named objects and groups for use in your configuration, and it includes the following sections:
Information About Objects
Objects are reusable components for use in your configuration. They can be defined and used in ASA configurations in the place of inline IP addresses, services, names, and so on. Objects make it easy to maintain your configurations because you can modify an object in one place and have it be reflected in all other places that are referencing it. Without objects you would have to modify the parameters for every feature when required, instead of just once. For example, if a network object defines an IP address and subnet mask, and you want to change the address, you only need to change it in the object definition, not in every feature that refers to that IP address.
Licensing Requirements for Objects
Guidelines and Limitations
Supported in single and multiple context mode.
Supported in routed and transparent firewall mode.
- Supports IPv6.
- The ASA does not support IPv6 nested network object groups, so you cannot group an object with IPv6 entries under another IPv6 object group.
- You can mix IPv4 and IPv6 entries in a network object group; you cannot use a mixed object group for NAT.
Additional Guidelines and Limitations
- Object must have unique names. While you might want to create a network object group named “Engineering” and a service object group named “Engineering,” you need to add an identifier (or “tag”) to the end of at least one object group name to make it unique. For example, you can use the names “Engineering_admins” and “Engineering_hosts” to make the object group names unique and to aid in identification.
- Objects and object groups share the same name space.
- You cannot remove an object or make an object empty if it is used in a command.
Configuring Objects
Configuring Network Objects and Groups
This section describes how to configure network objects and groups, and it includes the following topics:
Configuring a Network Object
A network object can contain a host, a network IP address, or a range of IP addresses, a fully qualified domain name (FQDN). You can also enable NAT rules on the object (excepting FQDN objects). (See Chapter 4, “Configuring Network Object NAT,” in the firewall configuration guide for more information.)
Detailed Steps
|  |  |  | 
|---|---|---|
| Step 1 |  | Creates a new network object. The obj_name is a text string up to 64 characters in length and can be any combination of letters, digits, and the following characters: The prompt changes to network object configuration mode. | 
| Step 2 | { host ip_addr \| subnet net_addr net_mask \| range ip_addr_1 ip_addr_2 \| fqdn fully_qualified_domain_name } | Assigns the IP address or FQDN to the named object. Note You cannot configure NAT for an FQDN object. | 
| Step 3 |  | Adds a description to the object. | 
Examples
To create a network object, enter the following commands:
Configuring a Network Object Group
Network object groups can contain multiple network objects as well as inline networks. Network object groups can support a mix of both IPv4 and IPv6 addresses.
Restrictions
You cannot use a mixed IPv4 and IPv6 object group for NAT, or object groups that include FQDN objects.
Detailed Steps
|  |  |  | 
|---|---|---|
| Step 1 |  | Adds a network group. The grp_id is a text string up to 64 characters in length and can be any combination of letters, digits, and the following characters: The prompt changes to protocol configuration mode. | 
| Step 2 |  | (Optional) Adds a description. The description can be up to 200 characters. | 
| Step 3 | Add one or more of the following group members: |  | 
|  |  | Adds an object to the network object group. | 
|  |  | Adds a host or network inline, either IPv4 or IPv6. | 
|  |  | Adds an existing object group under this object group. The nested group must be of the same type. | 
Example
To create a network group that includes the IP addresses of three administrators, enter the following commands:
Create network object groups for privileged users from various departments by entering the following commands:
You then nest all three groups together as follows:
Configuring Service Objects and Service Groups
Service objects and groups identify protocols and ports. This section describes how to configure service objects, service groups, TCP and UDP port service groups, protocol groups, and ICMP groups, and it includes the following topics:
Configuring a Service Object
The service object can contain a protocol, ICMP, ICMPv6, TCP or UDP port or port ranges.
Detailed Steps
|  |  |  | 
|---|---|---|
| Step 1 |  | Creates a new service object. The obj_name is a text string up to 64 characters in length and can be any combination of letters, digits, and the following characters: The prompt changes to service object configuration mode. | 
| Step 2 |  | Adds a service definition to the object. The protocol argument specifies an IP protocol name or number. If you specify the icmp , icmp6 , tcp , or udp protocols, you can specify additional properties; for other protocols, there are no additional properties. For icmp and icmp6 (for ICMP version 6), you can optionally include the ICMP type, such as “echo” or the type number. If you specify a type, you can optionally include an ICMP code, between 1 and 255. For TCP and UDP, you can optionally specify the source and/or destination ports, between 0 and 65535. For a list of supported names, see the CLI help. The operator can be: | 
Example
To create a service object, enter the following commands:
Configuring a Service Group
A service object group includes a mix of protocols, if desired, including optional source and destination ports for TCP or UDP.
Detailed Steps
|  |  |  | 
|---|---|---|
| Step 1 |  | Adds a service group. The grp_id is a text string up to 64 characters in length and can be any combination of letters, digits, and the following characters: The prompt changes to service configuration mode. | 
| Step 2 | Add one or more of the following group members: |  | 
|  |  | Identifies the protocol name or number, between 0 and 255. | 
|  |  | You can specify the source and/or destination ports, between 0 and 65535, for the TCP and UDP protocols; tcp-udp matches both protocols. For a list of supported names, see the CLI help. The operator can be: | 
|  |  | Specifies that the service type is for ICMP or ICMPv6 connections. You can optionally specify the ICMP type by name or number, between 0 and 255. The optional icmp_code specifies an ICMP code, between 1 and 255. | 
|  |  | Specifies a service object name, created with the object service command. | 
|  |  | Adds an existing object group under this object group. The nested group must be of the same type. | 
| Step 3 |  | (Optional) Adds a description. The description can be up to 200 characters. | 
Examples
The following example shows how to add both TCP and UDP services to a service object group:
The following example shows how to add multiple service objects to a service object group:
Configuring a TCP or UDP Port Service Group
A TCP or UDP service group includes a group of ports for a specific protocol (TCP, UDP, or TCP-UDP).
|  |  |  | 
|---|---|---|
| Step 1 |  | Adds a service group. The object keyword adds an additional object to the service object group. The grp_id is a text string up to 64 characters in length and can be any combination of letters, digits, and the following characters: Specifies the protocol for the services (ports) you want to add with either the tcp , udp , or tcp-udp keywords. Enter the tcp-udp keyword if your service uses both TCP and UDP with the same port number, for example, DNS (port53). The prompt changes to service configuration mode. | 
| Step 2 | Add one or more of the following group members: |  | 
|  |  | Defines the ports in the group. Enter the command for each port or range of ports. For a list of permitted keywords and well-known port assignments, see the “Protocols and Applications” section. | 
|  |  | Adds an existing object group under this object group. The nested group must be of the same type. | 
| Step 3 |  | (Optional) Adds a description. The description can be up to 200 characters. | 
Example
To create service groups that include DNS (TCP/UDP), LDAP (TCP), and RADIUS (UDP), enter the following commands:
Configuring an ICMP Group
Detailed Steps
|  |  |  | 
|---|---|---|
| Step 1 |  | Adds an ICMP type object group. The grp_id is a text string up to 64 characters in length and can be any combination of letters, digits, and the following characters: The prompt changes to ICMP type configuration mode. | 
| Step 2 | Add one or more of the following group members: |  | 
|  |  | Defines the ICMP types in the group. Enter the command for each type. For a list of ICMP types, see the“ICMP Types” section. | 
|  |  | Adds an existing object group under this object group. The nested group must be of the same type. | 
| Step 3 |  | (Optional) Adds a description. The description can be up to 200 characters. | 
Example
Create an ICMP type group that includes echo-reply and echo (for controlling ping) by entering the following commands:
Configuring a Protocol Group
Detailed Steps
|  |  |  | 
|---|---|---|
| Step 1 |  | Adds a protocol group. The obj_grp_id is a text string up to 64 characters in length and can be any combination of letters, digits, and the following characters: The prompt changes to protocol configuration mode. | 
| Step 2 | Add one or more of the following group members: |  | 
|  |  | Defines the protocols in the group. Enter the command for each protocol. The protocol is the numeric identifier of the specified IP protocol (1 to 254) or a keyword identifier (for example, icmp , tcp , or udp ). To include all IP protocols, use the keyword ip . For a list of protocols that you can specify, see the “Protocols and Applications” section. | 
|  |  | Adds an existing object group under this object group. The nested group must be of the same type. | 
| Step 3 |  | (Optional) Adds a description. The description can be up to 200 characters. | 
Example
To create a protocol group for TCP, UDP, and ICMP, enter the following commands:
Configuring Local User Groups
You can create local user groups for use in features that support the identity firewall (IDFW) by including the group in an extended ACL, which in turn can be used in an access rule, for example.
The ASA sends an LDAP query to the Active Directory server for user groups globally defined in the Active Directory domain controller. The ASA imports these groups for identity-based rules. However, the ASA might have localized network resources that are not defined globally that require local user groups with localized security policies. Local user groups can contain nested groups and user groups that are imported from Active Directory. The ASA consolidates local and Active Directory groups.
A user can belong to local user groups and user groups imported from Active Directory.
Prerequisites
See “Configuring the Identity Firewall,” to enable IDFW.
Detailed Steps
|  |  |  | 
|---|---|---|
| Step 1 | object-group user user _ group _name hostname(config)# object-group user users1 | Defines object groups that you can use to control access with the Identity Firewall. | 
| Step 2 | Add one or more of the following group members: |  | 
|  | user domain_NetBIOS_name \ user_name hostname(config-user-object-group)# user SAMPLE\users1 | Specifies the user to add to the access rule. The user_name can contain any character including [a-z], [A-Z], [0-9], [!@#$%^&()-_{}. ]. If domain_NetBIOS_name \ user _ name contains a space, you must enclose the domain name and user name in quotation marks. The user _ name can be part of the LOCAL domain or a user imported by the ASA from Active Directory domain. If the domain_NetBIOS_name is associated with a AAA server, the user _ name must be the Active Directory sAMAccountName, which is unique, instead of the common name (cn), which might not be unique. The domain_NetBIOS_name can be LOCAL or the actual domain name as specified in user - identity domain domain_NetBIOS_name aaa - server aaa _ serve r_ group _ tag command. | 
|  |  | Adds an existing object group under this object group. The nested group must be of the same type. | 
| Step 3 |  | (Optional) Adds a description. The description can be up to 200 characters. | 
Configuring Security Group Object Groups
You can create security group object groups for use in features that support Cisco TrustSec by including the group in an extended ACL, which in turn can be used in an access rule, for example.
When integrated with Cisco TrustSec, the ASA downloads security group information from the ISE. The ISE acts as an identity repository, by providing Cisco TrustSec tag to user identity mapping and Cisco TrustSec tag to server resource mapping. You provision and manage security group ACLs centrally on the ISE.
However, the ASA might have localized network resources that are not defined globally that require local security groups with localized security policies. Local security groups can contain nested security groups that are downloaded from the ISE. The ASA consolidates local and central security groups.
To create local security groups on the ASA, you create a local security object group. A local security object group can contain one or more nested security object groups or Security IDs or security group names. User can also create a new Security ID or security group name that does not exist on the ASA.
You can use the security object groups you create on the ASA to control access to network resources. You can use the security object group as part of an access group or service policy.
Prerequisites
See “Configuring the ASA to Integrate with Cisco TrustSec,” to enable TrustSec.
Detailed Steps
|  |  |  | 
|---|---|---|
| Step 1 | object-group security objgrp_name ciscoasa(config)# object-group security mktg-sg | Creates a security group object. Where objgrp_name is the name for the group entered as a 32-byte case sensitive string. The objgrp_name can contain any character including [a-z], [A-Z], [0-9], [!@#$%^&()-_{}. ]. | 
| Step 2 | Add one or more of the following group members: |  | 
|  | security-group { tag sgt# \| name sg_name } ciscoasa(config)# security-group name mktg | Specifies the type of security group object as either an inline tag or a named object.  An SGT is assigned to a device through IEEE 802.1X authentication, web authentication, or MAC authentication bypass (MAB) by the ISE. Security group names are created on the ISE and provide user-friendly names for security groups. The security group table maps SGTs to security group names. | 
|  |  | Adds an existing object group under this object group. The nested group must be of the same type. | 
| Step 3 |  | (Optional) Adds a description. The description can be up to 200 characters. | 
Examples
The following example shows how to configure a security group object:
Configuring Regular Expressions
Creating a Regular Expression
A regular expression matches text strings either literally as an exact string, or by using metacharacters so that you can match multiple variants of a text string. You can use a regular expression to match the content of certain application traffic; for example, you can match a URL string inside an HTTP packet.
Guidelines
Use Ctrl+V to escape all of the special characters in the CLI, such as question mark (?) or a tab. For example, type d[Ctrl+V]?g to enter d?g in the configuration.
See the regex command in the command reference for performance impact information when matching a regular expression to packets.
Note As an optimization, the ASA searches on the deobfuscated URL. Deobfuscation compresses multiple forward slashes (/) into a single slash. For strings that commonly use double slashes, like “http://”, be sure to search for “http:/” instead.
Table 17-1 lists the metacharacters that have special meanings.
|  |  |  | 
|---|---|---|
| . | Dot | Matches any single character. For example, d.g matches dog, dag, dtg, and any word that contains those characters, such as doggonnit. | 
| ( exp ) | Subexpression | A subexpression segregates characters from surrounding characters, so that you can use other metacharacters on the subexpression. For example, d(o\|a)g matches dog and dag, but do\|ag matches do and ag. A subexpression can also be used with repeat quantifiers to differentiate the characters meant for repetition. For example, ab(xy){3}z matches abxyxyxyz. | 
| \| | Alternation | Matches either expression it separates. For example, dog\|cat matches dog or cat. | 
| ? | Question mark | A quantifier that indicates that there are 0 or 1 of the previous expression. For example, lo?se matches lse or lose. Note You must enter Ctrl+V and then the question mark or else the help function is invoked. | 
| * | Asterisk | A quantifier that indicates that there are 0, 1 or any number of the previous expression. For example, lo*se matches lse, lose, loose, and so on. | 
| + | Plus | A quantifier that indicates that there is at least 1 of the previous expression. For example, lo+se matches lose and loose, but not lse. | 
| { x } or { x ,} | Minimum repeat quantifier | Repeat at least x times. For example, ab(xy){2,}z matches abxyxyz, abxyxyxyz, and so on. | 
| [ abc ] | Character class | Matches any character in the brackets. For example, [abc] matches a, b, or c. | 
| [^ abc ] | Negated character class | Matches a single character that is not contained within the brackets. For example, [^abc] matches any character other than a, b, or c. [^A-Z] matches any single character that is not an uppercase letter. | 
| [ a - c ] | Character range class | Matches any character in the range. [a-z] matches any lowercase letter. You can mix characters and ranges: [abcq-z] matches a, b, c, q, r, s, t, u, v, w, x, y, z, and so does [ a-cq-z] . The dash (-) character is literal only if it is the last or the first character within the brackets: [abc-] or [-abc] . | 
| “” | Quotation marks | Preserves trailing or leading spaces in the string. For example, “ test” preserves the leading space when it looks for a match. | 
| ^ | Caret | Specifies the beginning of a line. | 
| \ | Escape character | When used with a metacharacter, matches a literal character. For example, \[ matches the left square bracket. | 
| char | Character | When character is not a metacharacter, matches the literal character. | 
| \r | Carriage return | Matches a carriage return 0x0d. | 
| \n | Newline | Matches a new line 0x0a. | 
| \t | Tab | Matches a tab 0x09. | 
| \f | Formfeed | Matches a form feed 0x0c. | 
| \x NN | Escaped hexadecimal number | Matches an ASCII character using hexadecimal (exactly two digits). | 
| \ NNN | Escaped octal number | Matches an ASCII character as octal (exactly three digits). For example, the character 040 represents a space. | 
Detailed Steps
Step 1 To test a regular expression to make sure it matches what you think it will match, enter the following command:
Where the input_text argument is a string you want to match using the regular expression, up to 201 characters in length.
The regular_expression argument can be up to 100 characters in length.
Use Ctrl+V to escape all of the special characters in the CLI. For example, to enter a tab in the input text in the test regex command, you must enter test regex “test[Ctrl+V Tab]” “test\t” .
If the regular expression matches the input text, you see the following message:
If the regular expression does not match the input text, you see the following message:
Step 2 To add a regular expression after you tested it, enter the following command:
Where the name argument can be up to 40 characters in length.
The regular_expression argument can be up to 100 characters in length.
Examples
The following example creates two regular expressions for use in an inspection policy map:
Creating a Regular Expression Class Map
A regular expression class map identifies one or more regular expressions. You can use a regular expression class map to match the content of certain traffic; for example, you can match URL strings inside HTTP packets.
Prerequisites
Create one or more regular expressions according to the “Creating a Regular Expression” section.
Detailed Steps
Step 1 Create a class map by entering the following command:
Where class_map_name is a string up to 40 characters in length. The name “class-default” is reserved. All types of class maps use the same name space, so you cannot reuse a name already used by another type of class map.
The match-any keyword specifies that the traffic matches the class map if it matches at least one of the regular expressions.
The CLI enters class-map configuration mode.
Step 2 (Optional) Add a description to the class map by entering the following command:
Step 3 Identify the regular expressions you want to include by entering the following command for each regular expression:
Examples
The following example creates two regular expressions, and adds them to a regular expression class map. Traffic matches the class map if it includes the string “example.com” or “example2.com.”
Configuring Time Ranges
Create a reusable component that defines starting and ending times that can be applied to various security features. Once you have defined a time range, you can select the time range and apply it to different options that require scheduling.
The time range feature lets you define a time range that you can attach to traffic rules, or an action. For example, you can attach an ACL to a time range to restrict access to the ASA.
A time range consists of a start time, an end time, and optional recurring entries.
Guidelines
- Multiple periodic entries are allowed per time range. If a time range has both absolute and periodic values specified, then the periodic values are evaluated only after the absolute start time is reached, and they are not further evaluated after the absolute end time is reached.
- Creating a time range does not restrict access to the device. This procedure defines the time range only.
Detailed Steps
|  |  |  | 
|---|---|---|
| Step 1 |  | Identifies the time-range name. | 
| Step 2 | Do one of the following: |  | 
|  |  | Specifies a recurring time range. You can specify the following values for days-of-the-week : The time is in the format hh : mm . For example, 8:00 is 8:00 a.m. and 20:00 is 8:00 p.m. | 
|  |  | Specifies an absolute time range. The time is in the format hh : mm . For example, 8:00 is 8:00 a.m. and 20:00 is 8:00 p.m. The date is in the format day month year ; for example, 1 january 2006 . | 
Examples
The following is an example of an absolute time range beginning at 8:00 a.m. on January 1, 2006. Because no end time and date are specified, the time range is in effect indefinitely.
The following is an example of a weekly periodic time range from 8:00 a.m. to 6:00 p.m on weekdays:
Monitoring Objects
To monitor objects and groups, enter the following commands:
|  |  | 
|---|---|
|  | Displays the access list entries that are expanded out into individual entries without their object groupings. | 
|  | Displays all current object groups. | 
|  | Displays the current object groups by their group ID. | 
|  | Displays the current object groups by their group type. | 
Feature History for Objects
Table 17-2 lists each feature change and the platform release in which it was implemented.
|  |  |  | 
|---|---|---|
| Object groups | 7.0(1) | Object groups simplify ACL creation and maintenance. We introduced or modified the following commands: object-group protocol , object-group network , object-group service , object-group icmp_type . | 
| Regular expressions and policy maps | 7.2(1) | Regular expressions and policy maps were introduced to be used under inspection policy maps. The following commands were introduced: class-map type regex , regex , match regex . | 
| Objects | 8.3(1) | Object support was introduced. We introduced or modified the following commands: object-network , object-service , object-group network , object-group service , network object , access-list extended , access-list webtype , access-list remark . | 
| User Object Groups for Identity Firewall | 8.4(2) | User object groups for identity firewall were introduced. We introduced the following commands: object-network user , user . | 
| Mixed IPv4 and IPv6 network object groups | 9.0(1) | Previously, network object groups could only contain all IPv4 addresses or all IPv6 addresses. Now network object groups can support a mix of both IPv4 and IPv6 addresses. Note You cannot use a mixed object group for NAT. We modified the following commands: object-group network . | 
| Security Group Object Groups for Cisco TrustSec | 8.4(2) | Security group object groups for TrustSec were introduced. We introduced the following commands: object-network security , security . | 
| Extended ACLand object enhancement to filter ICMP traffic by ICMP code | 9.0(1) | ICMP traffic can now be permitted/denied based on ICMP code. We introduced or modified the following commands: access-list extended , service-object , service . |