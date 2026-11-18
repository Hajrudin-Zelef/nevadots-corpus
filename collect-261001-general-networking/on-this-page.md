# Augmenting VPN security with ZTNA tags

# Augmenting VPN security with ZTNA tags

FortiGate's integration of ZTNA tags into the VPN infrastructure offers a powerful solution to enhance VPN security. ZTNA tags are a feature exclusively offered with the licensed FortiClient versions (FortiClient EMS). ZTNA tags are objects that are assigned to the FortiClient endpoints in real-time. ZTNA tags are used in the firewall policies on the FortiGate to allow or deny access to the VPN and network resources based on the organization's security compliance regulations. These compliance regulations are enforced in real-time, thereby safeguarding the organization against constantly evolving security threats.

The following table compares features of the free VPN-only standalone FortiClient versus a licensed FortiClient managed by EMS for security compliance.

| Feature | Free VPN-only standalone FortiClient | Licensed FortiClient | 
|---|---|---|
| Basic VPN connection | Yes | Yes | 
| Managed remote access profiles | No | Yes | 
| Compliance using ZTNA tags:  | No | Yes | 

For more detailed information, see Feature comparison of FortiClient standalone and licensed versions in the FortiClient Administration Guide.

ZTNA tags (formerly FortiClient EMS tags in FortiOS 6.4 and earlier) are tags synchronized from FortiClient EMS as dynamic address objects on the FortiGate. FortiClient EMS uses zero-trust tagging rules to automatically tag managed endpoints based on various attributes detected by the FortiClient. When the FortiGate establishes a connection with the FortiClient EMS server through the EMS Fabric connector, it pulls zero-trust tags containing device IP and MAC addresses and converts them to read-only dynamic address objects. It also establishes a persistent WebSocket connection to monitor for changes in zero-trust tags, which keeps the device information current. These zero-trust tags can then be used in SSL VPN firewall rules to perform security posture checks to restrict or allow access to network resources, enabling role-based access control.

The FortiGate needs to be connected to FortiClient EMS in order to retrieve the ZTNA tags so they can be used in firewall policies. This is done by configuring a FortiClient EMS Security Fabric connector on the FortiGate to connect to FortiClient EMS. See Configuring FortiClient EMS for more information.

You can create, edit, and delete zero-trust tagging rules for endpoints. You can also view and manage the tags used to dynamically group endpoints.

The following process occurs when using zero-trust tagging rules with EMS and FortiClient:

1. 
                                                    EMS sends zero-trust tagging rules to endpoints through telemetry communication.
2. 
                                                    FortiClient checks endpoints using the provided rules and sends the results to EMS.
3. 
                                                    EMS receives the results from FortiClient.
4. 
                                                    EMS dynamically groups endpoints together using the tag configured for each rule. The dynamic endpoint groups can be viewed on the *Zero Trust Tags > Zero Trust Tag Monitor* page. See Zero Trust Tag Monitor in the FortiClient EMS Administration Guide for more information.

In this topic, two zero-trust tagging rule sets are created:

| ZTNA tag | ZTNA tagging rule | 
|---|---|
| AD-Joined | Apply if a remote user has OS version Windows 8.1 or Windows 10 and is a part of the AD group, FORTI-ARBUTUS.LOCAL/IT/IT. | 
| Vulnerable | Apply if critical vulnerabilities are detected on a remote user. | 

These tags will be applied in two scenario examples (see Scenario 1 and Scenario 2). For more information about zero-trust tagging rule settings, see Adding a Zero Trust tagging rule set and Zero Trust tagging rule types in the FortiClient EMS Administration Guide.

###### To create a zero-trust tagging rule set in FortiClient EMS:

1. 
                                                    Go to *Zero Trust Tags > Zero Trust Tagging Rules* , and click*Add* .
2. 
                                                    Create the AD-Joined tagging rule set: 
  1. 
                                                            In the *Name* field, enter*AD-Joined* .
  2. 
                                                            In the *Tag Endpoint As* dropdown list, enter*AD-Joined* and press`Enter` .EMS uses this tag to dynamically group together endpoints that satisfy the rule, as well as any other rules that are configured to use this tag.
  3. 
                                                            Toggle *Enabled* on to enable the rule.
  4. 
                                                            Configure the user in AD group rule: 
    1. 
                                                                    Click *Add Rule* .
    2. 
                                                                    Set *OS* to*Windows* .
    3. 
                                                                    Set the *Rule Type* to*User in AD Group* .
    4. 
                                                                    Set the *AD Group* to*FORTI-ARBUTUS.LOCAL/IT/IT* .
    5. 
                                                                    Click *Save* .
  5. 
                                                                    
  6. 
                                                            Configure the OS rule: 
    1. 
                                                                    Click *Add Rule* .
    2. 
                                                                    Set *OS* to*Windows* .
    3. 
                                                                    Set the *Rule Type* to*OS Version* and select*Windows 8.1* .
    4. 
                                                                    Click the *+* button and select*Windows 10* .
    5. 
                                                                    Click *Save* .
  7. 
                                                                    
  8. 
                                                            By default, an endpoint must satisfy all configured rules to be eligible for the rule set. You may want to apply the tag to endpoints that satisfy some, but not all, of the configured rules. In this example, you need to modify the rule set logic to apply the same tag to endpoints that fulfill one of the following criteria: 
    - 
                                                                    Running Windows 8.1 or 10
    - 
                                                                    Is part of an AD group called FORTI-ARBUTUS.LOCAL/IT/IT
 With the default rule set logic, an endpoint would be eligible for the rule set if it is running Windows 8.1 or 10 and is part of an AD group called IT. To modify the rule set logic, do the following: 
    1. 
                                                                    Click *Edit Logic* .
    2. 
                                                                    Clicking *Edit Logic* assigns numerical values to each configured rule. You can use*and* and*or* to define the rule logic. You cannot use*not* when defining the rule logic. You can also use parentheses to group rules.In the *Rule Logic* field, enter*1 and (2 or 3)* to indicate that endpoints that satisfy that they are part of the AD IT group (rule 1) and Windows 8.1  (rule 2) or Windows 10 (rule 3) satisfy the rule set.
  9. 
                                                                    
  10. 
                                                            Click *Save* .
3. 
                                                            
4. 
                                                    Create the Vulnerable tagging rule set: 
  1. 
                                                            Click *Add* .
  2. 
                                                            In the *Name* field, enter*Vulnerable* .
  3. 
                                                            In the *Tag Endpoint As* dropdown list, enter*Vulnerable* and press`Enter` .
  4. 
                                                            Toggle *Enabled* on to enable the rule.
  5. 
                                                            Configure the vulnerable devices rule: 
    1. 
                                                                    Click *Add Rule* .
    2. 
                                                                    Set *OS* to*Windows* .
    3. 
                                                                    Set the *Rule Type* to*Vulnerable Devices* .
    4. 
                                                                    Set the *Security Level* to*Critical* .
    5. 
                                                                    Click *Save* .
  6. 
                                                                    
  7. 
                                                            Click *Save* .
5. 
                                                            

|  | For more information about editing, deleting, and importing ZTNA rules, see Zero Trust Tagging Rules in the FortiClient EMS Administration Guide. | 

After FortiClient software installation is complete on an endpoint, you can connect FortiClient to FortiClient EMS. Depending on the way the FortiClient installation is performed, you can either manually or automatically connect to FortiClient EMS, see Connecting FortiClient Telemetry after installation in the FortiClient Administration Guide for more details.

Once FortiClient connects to the FortiClient EMS, the *Status* shows up as *Connected* in the *Zero Trust Telemetry* tab.


After FortiClient telemetry connects to EMS, FortiClient endpoints receive the ZTNA tags if they satisfy any of the required ZTNA rules configured on the FortiClient EMS.

After the FortiGate is connected and authorized to and by FortiClient EMS, the ZTNA tags that were created in the zero-trust tagging rules are retrieved by the FortiGate. To view the tags on the FortiGate, go to *Policy & Objects > ZTNA* and select the *ZTNA Tags* tab.


If the tags are not visible on the FortiGate, ensure that the FortiClient EMS is configured to share tagging information with the FortiGate. See Configuring EMS to share tagging information with multiple FortiGates in the FortiClient EMS Administration Guide for more details.

To view the ZTNA tags assigned to the FortiClient endpoints by FortiClient EMS, click the user avatar and locate the *Zero Trust Tags* section. The following FortiClient endpoint is assigned  the *AD-Joined* tag.


Ensure that the *Show Zero Trust Tag on FortiClient GUI* is enabled on FortiClient EMS (*Endpoint Profiles > System Settings* in the profile's *Advanced* view) so the tags are visible in FortiClient. See System Settings in the FortiClient EMS Administration Guide for more details.

ZTNA tags can also be monitored on FortiClient EMS from the endpoint's details in the *Endpoints* pane.

###### To view ZTNA tag information in the endpoint details:

1. 
                                                    Go to *Endpoints* , and select*All Endpoints* , a domain, or workgroup. The list of endpoints for the selected domain or workgroup displays.
2. 
                                                    Click an endpoint to display details about it in the content pane.
3. 
                                                    In the *Summary* pane, you can see the*Zero Trust Tags* associated with the endpoint. For example, this user has the*AD-Joined* and*all_registered_clients* tags.For detailed descriptions of the options in the *Endpoints* content pane, see Viewing the Endpoints pane in the FortiClient EMS Administration Guide.

ZTNA tags can be used to augment VPN security using the following methods:

- 
                                                    Restrict an endpoint to connect to the VPN tunnel based on the ZTNA tag (see Scenario 1).
- 
                                                    Control access to network resources by allowing or denying traffic passing through the FortiGate using the *IP/MAC Based Access Control* field in the firewall policy (also known as ZTNA IP MAC based access control, see Scenario 2).

Both methods are demonstrated in the following example.


In this example, Off-net-Client is the FortiClient endpoint connected to and managed by FortiClient EMS. The telemetry traffic passes through the FortiGate using a virtual IP. The two ZTNA rule tagging sets configured previously (AD-Joined and Vulnerable) are applied.

Enterprise Core is the FortiGate that acts as the SSL VPN server. To configure SSL VPN, refer to SSL VPN and SSL VPN security best practices. Critical Assets are network resources that the off-net user tries to access after connecting to the VPN. SSL VPN is used in this example, but a similar configuration also applies to dialup IPsec VPN where the FortiGate acts as a dialup server.

FortiClient endpoint profiles can be configured to allow or block an endpoint from connecting to a VPN tunnel based on its applied zero-trust tag. This feature is only available for Windows endpoints.

In this scenario, the endpoint profile is configured to prohibit the Off-net-Client (with a Windows OS) from connecting to the VPN if the endpoint has a Vulnerable ZTNA tag. The Vulnerable tag was configured previously (see Creating ZTNA tags and ZTNA rules in FortiClient EMS).

###### To configure the remote access profile in FortiClient EMS:

1. 
                                                    Go to *Endpoint Profiles > Remote Access* , and edit an existing profile or add a new one.
2. 
                                                    In the *General* section, enable*Enable Secure Remote Access* .
3. 
                                                    In the *VPN Tunnels* section, edit an existing VPN tunnel or add a new one.
4. 
                                                    Configure the following under *Advanced Settings* :
  1. 
                                                            For the *Tag* field, select*Prohibit* from the first dropdown.
  2. 
                                                            Select the *Vulnerable* tag from the second dropdown.
  3. 
                                                            Enable *Customize Host Check Fail Warning* .
  4. 
                                                            Enter a message to display to users when their connection to the VPN tunnel is prohibited due to critical vulnerabilities on their device.
  5. 
                                                            Configure the other VPN tunnel settings as needed.
  6. 
                                                            Click *Save* .
5. 
                                                            
6. 
                                                    Configure the other remote access profile settings as needed.
7. 
                                                    Click *Save* .After the next communication between FortiClient EMS and FortiClient, endpoints with this profile applied are unable to connect to this VPN tunnel if they have critical vulnerabilities.

###### To verify the configuration using a vulnerable endpoint:

1. 
                                                    On the Off-net-Client endpoint, open FortiClient.
2. 
                                                    Click *Vulnerability Scan* , then click*Scan Now* to initiate a manual scan.Vulnerability scans can also be scheduled. See Vulnerability Scan in the FortiClient EMS Administration Guide for more details.
3. 
                                                    Wait a few minutes for the scan to complete.
4. 
                                                    Click the user avatar and locate the *Zero Trust Tags* section.FortiClient discovered the vulnerability and added a *Vulnerable* ZTNA tag in addition to the existing*AD-Joined* tag.
5. 
                                                    Click *Remote Access* to try to connect to the VPN:
  1. 
                                                            Enter the *Username* and*Password* .
  2. 
                                                            Click *Connect* .Based on the remote access profile configuration, the endpoint's access is denied due to the assigned *Vulnerable* ZTNA tag. The message configured in the remote access profile appears as a notification above the FortiTray icon (in the Windows system tray).See FortiTray in the FortiClient Administration Guide for more details about this icon.
6. 
                                                            

ZTNA tags are used in firewall policies to control access to network resources with the *IP/MAC Based Access Control* field. 

In this scenario, if the Off-net Client is tagged with a Vulnerable tag, then it is not allowed to access Finance Server 1 (10.100.77.200). If the Off-net Client is tagged with an AD-Joined tag and no Vulnerable tag, then it is allowed to access Finance Server 1. Two firewall policies are configured as follows.

- 
                                                    Deny Vulnerable Endpoints: use IP/MAC based access control with the Vulnerable ZTNA IP tag to deny access.
- 
                                                    SSL VPN to DMZ: use IP/MAC based access control with the AD-Joined ZTNA IP tag to allow access.

These polices use a source address and group that have already be configured for SSL VPN users and authentication. See User groups for more information.

###### To configure the Deny Vulnerable Endpoints policy:

1. 
                                                    Go to *Policy & Objects > Firewall Policy* and click*Create New* .
2. 
                                                    Configure the following settings: Name Deny Vulnerable Endpoints Type Standard Incoming Interface ssl.root Outgoing Interface port2 Source SSLVPN_TUNNEL_ADDR1, AD-Joined VPN Users IP/MAC Based Access Control Vulnerable Destination DMZ Subnet Schedule always Service ALL Action DENY Log Violation Traffic Enable this setting. Enable this policy Enable this setting.
3. 
                                                    Click *OK* .

###### To verify the configuration using an off-net client with a Vulnerable tag:

|  | This verification assumes that a vulnerability scan was performed, the endpoint has a critical vulnerability, and the Vulnerable zero-trust tag was added. | 

1. 
                                                    On the endpoint, open FortiClient and click *Remote Access* to connect to the VPN:
  1. 
                                                            Enter the *Username* and*Password* .
  2. 
                                                            Click *Connect* .
2. 
                                                            
3. 
                                                    Once the FortiClient endpoint is connected to the VPN, try to access Finance Server 1 using the web server. The connection times out because the traffic is denied by the firewall policy.
4. 
                                                    Verify the forward traffic log: 
  1. 
                                                            In the GUI, go to *Log & Report > Forward Traffic* .
  2. 
                                                            In the CLI, enter the following: # execute log filter category 0 # execute log filter field policyname "Deny Vulnerable Endpoints" # execute log display date=2023-10-24 time=17:04:19 eventtime=1698192258985043569 tz="-0700" logid="0000000013" **type="traffic" subtype="forward"** level="notice" vd="root" srcip=10.212.134.200 srcport=53801**srcintf="ssl.root"** srcintfrole="undefined" dstip=10.100.77.200**dstport=80 dstintf="port2" dstintfrole="dmz"** srcuuid="697b0036-37db-51ee-162f-5fed6735b06e" dstuuid="2e024fe8-57d2-51ee-73a7-63e15a078456" srccountry="Reserved" dstcountry="Reserved" sessionid=25809**proto=6 action="deny"** policyid=3 policytype="policy" poluuid="c715492c-72aa-51ee-481d-09eef1adf713"**policyname="Deny Vulnerable Endpoints"** user="markgilbert"**service="HTTP"** trandisp="noop" duration=0 sentbyte=0 rcvdbyte=0 sentpkt=0 rcvdpkt=0 appcat="unscanned" crscore=30 craction=131072 crlevel="high"
5. 
                                                            

###### To configure the SSL VPN to DMZ policy:

1. 
                                                    Go to *Policy & Objects > Firewall Policy* and click*Create New* .
2. 
                                                    Configure the following settings: Name SSL VPN to DMZ Type Standard Incoming Interface ssl.root Outgoing Interface port2 Source SSLVPN_TUNNEL_ADDR1, AD-Joined VPN Users IP/MAC Based Access Control AD-Joined Destination DMZ Subnet Schedule always Service ALL Action ACCEPT Log Allowed Traffic Enable this setting and select *All Sessions* .Enable this policy Enable this setting.
3. 
                                                    Configure the other settings as needed.
4. 
                                                    Click *OK* .

###### To verify the configuration using an off-net client with an AD-Joined tag:

1. 
                                                    On the endpoint, open FortiClient, click *Remote Access* to  connect to the VPN:
  1. 
                                                            Enter the *Username* and*Password* .
  2. 
                                                            Click *Connect* .
2. 
                                                            
3. 
                                                    Once the FortiClient endpoint is connected to the VPN, try to access Finance Server 1 using the web server. The traffic is allowed by the firewall policy, and the server is accessible.
4. 
                                                    Verify the forward traffic log: 
  1. 
                                                            In the GUI, go to *Log & Report > Forward Traffic* .
  2. 
                                                            In the CLI, enter the following: # execute log filter category 0 # execute log filter field policyname "SSL VPN to DMZ" # execute log display date=2023-10-24 time=14:07:05 eventtime=1698181625479969117 tz="-0700" logid="0000000020" type="traffic" **subtype="forward"** level="notice" vd="root" srcip=10.212.134.200 srcport=51841**srcintf="ssl.root"** srcintfrole="undefined"**dstip=10.100.77.200 dstport=80 dstintf="port2" dstintfrole="dmz"** srcuuid="697b0036-37db-51ee-162f-5fed6735b06e" dstuuid="2e024fe8-57d2-51ee-73a7-63e15a078456" srccountry="Reserved" dstcountry="Reserved" sessionid=6656**proto=6 action="accept"** policyid=5 policytype="policy" poluuid="8393d776-72b0-51ee-b955-68588258f38d"**policyname="SSL VPN to DMZ"** user="markgilbert" group="AD-Joined VPN Users"**service="HTTP"** trandisp="noop" duration=155 sentbyte=1196 rcvdbyte=1084 sentpkt=9 rcvdpkt=7 appcat="unscanned" sentdelta=1196 rcvddelta=1084 dstdevtype="Computer" dstosname="Debian" masterdstmac="02:09:0f:00:01:04" dstmac="02:09:0f:00:01:04" dstserver=0
5.