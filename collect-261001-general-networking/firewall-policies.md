## Firewall policies

The firewall policy is the axis around which most of the other features of the FortiGate firewall revolve. A large portion of the settings in the firewall at some point will end up relating to or being associated with the firewall policies and the traffic that they govern. Any traffic going through a FortiGate unit has to be associated with a policy. These policies are essentially discrete compartmentalized sets of instructions that control the traffic flow going through the firewall. These instructions control where the traffic goes, how it’s processed, if it’s processed and even whether or not it’s allowed to pass through the FortiGate.

When the firewall receives a connection packet, it analyzes the packet’s source address, destination address, and service (by port number). It also registers the incoming interface, the outgoing interface it will need to use and the time of day. Using this information the FortiGate firewall attempts to locate a security policy that matches the packet. If it finds a policy that matches the parameters it then looks at the action for that policy. If it is **ACCEPT** the traffic is allowed to proceed to the next step. If the Action is **DENY** or a match cannot be found the traffic is not allowed to proceed.

The 2 basic actions at the initial connection are either **ACCEPT** or **DENY**:

- If the **Action** is**ACCEPT** , thee policy action permits communication sessions. There may be other packet processing instructions, such as requiring authentication to use the policy or restrictions on th source and destination of the traffic.
- If the **Action** is**DENY** , the policy action blocks communication sessions, and you can optionally log the denied traffic. If no security policy matches the traffic, the packets are dropped. A**DENY** security policy is needed when it is required to log the denied traffic, also called “violation traffic”.

There are two other Actions that can be associated with the policy:

- **LEARN** – This is a specialized variation on the**ACCEPT** That is set up to allow traffic but to keep traffic logs so that the administrator can go through them to learn what kind of traffic has to be dealt with. l**IPsec** – This is an ACCEPT action that is specifically for IPsec VPNs.

There can also be a number of instructions associated with a FortiGate firewall in addition to the **ACCEPT** or

**DENY** actions, some of which are optional. Instructions on how to process the traffic can also include such things as:

- Logging Traffic l Authentication l Network Address Translation or Port Address Translation l Use Virtual IPs or IP Pools l Caching l Whether the source of the traffic is based on address, user, device or a combination l Whether to treat as regular traffic or IPsec traffic l What certificates to use l Security profiles to apply l Proxy Options l Traffic Shaping

### Firewall policy parameters

As mentioned before, for traffic to flow through the FortiGate firewall there must be a policy that matches its parameters:

#### Incoming interface(s)

This is the interface or interfaces that the traffic is first connection to the FortiGate unit by. The exception being traffic that the FortiGate generates itself. This is not limited to the physical Ethernet ports found on the device. The incoming interface can also be a logical or virtual interface such as a VPN tunnel, a Virtual WAN link or a wireless interface.

#### Outgoing interface(s)

After the firewall has processed the traffic it needs to leave a port to get to its destination and this will be the interface or interfaces that the traffic leaves by. This interface, like the **Incoming Interface** is not limited to only physical interfaces.

#### Source address(es)

The addresses that a policy can receive traffic from can be wide open or tightly controlled. For a public web server that the world at large should be able to access, the best choice will be “all”. If the destination is a private web server that only the branch offices of a company should be able to access or a list of internal computers that are the only ones allowed to access an external resource then a group of preconfigured addresses is the better strategy.

Additional parameters under the Source Address, though they are not mandatory are:

l **Source User(s)**

This parameter is based on a user identity that can be from a number of authentication authorities. It will be an account or group that has been set up in advance that can be selected from the drop down menu. The exception to this is the feature that allows the importing of LDAP Users. When the feature is used, a small wizard window will appear to guide the user through the setup. The caveat is that the LDAP server object in the **User and Device > Authentication > LDAP Servers** section has to be already configured to allow the use of this import feature. l **Source Device Type**

This parameter is for narrowing down the traffic sending devices to those that the FortiGate is familiar with. Again the contents of this parameter need to be a preconfigured object and these are defined at **User and Device > Custom Devices & Groups.** This parameter can limit the devices that can connect to this policy to those specific MAC addresses that are already known by the FortiGate and are approved for the policy.

#### Destination address(es)

In the same way that the source address may need to be limited, the destination address can be used as a traffic filter. When the traffic is destined for internal resources the specific address of the resource can be defined to better protect the other resources on the network. One of the specialized destination address options is to use a Virtual IP address. The destination address doesn’t need to be internal you can define policies that are only for connecting to specific addresses on the Internet.

#### Internet service(s)

In this context, and Internet service is a combination of one or more addresses and one or more services associated with a service found on the Internet such as an update service for software.

#### Schedule

The time frame that is applied to the policy. This can be something as simple as a time range that the sessions are allowed to start such as between 8:00 am and 5:00 pm. Something more complex like business hours that include a break for lunch and time of the session’s initiation may need a schedule group because it will require multiple time ranges to make up the schedule.

#### Service

The service or service chosen here represent the TCP/IP suite port numbers that will most commonly be used to transport the named protocols or group of protocols. This will be a little different than Application Control which looks more closely at the packets to determine the actual protocol used to create them.

Without all six (possibly 8) of these things matching, the traffic will be declined. Each traffic flow requires a policy and the direction is important as well. Just because packets can go from point A to point B on port X does not mean that the traffic can flow from point B to point A on port X. A policy must be configured for each direction.

When designing a policy there is often reference to the traffic flow, but most communication is a two way connection so trying to determine the direction of the flow can be somewhat confusing. If traffic is HTTP web traffic the user sends a request to the web site, but most of the traffic flow will be coming from the web site to the user. Is the traffic flow considered to be from the user to the web site, the web site to the user or in both directions? For the purposes of determining the direction for a policy the important factor is the direction of the initiating communication. The user is sending a request to the web site so this is the initial communication and the web site is just responding to it so the traffic will be from the users network to the Internet.

A case where either side can initiate the communication like between two internal interfaces on the FortiGate unit would be a more likely situation to require a policy for each direction.

### Application groups for NGFW policies

In addition to parameters like schedule and service, NGFW policies can filter by application or application category.

To use the feature first create an application group in **Security Profiles > Custom Signatures**.

From the editing page for the **New Application Group**, choose a group type of **Application** and select individual applications for membership in the group.

Alternatively, select **Category** and add one or more application categories as group members.

Whichever type of **Application Group** you choose, the available **Members** will be displayed in the selection pane that slides out from the right of the window.

Once the **Application Group** is created, you can apply it to a policy in the **Application** field, by clicking on the **+** in the field and selecting members from the options under the **Group** tab at the top of the pane that slides out from the right of the window.

#### CLI

**To create or edit an application group:**

config application group edit <group_name> set comments set type {application | category} set application <Application ID number> set category <category ID number> end

**To add an application group to a policy:**

config firewall policy

edit 1 set app-group “test” “test1”

end

#### Application ID number

In the CLI, you add applications to a group by using the application ID number. To see the list of application ID numbers, run the following command when type is set to application: set application ? <enter>

The start of the list looks like: set application

ID Select Application ID

38614 1kxun

29025 1und1.Mail

- 2ch
- 2ch_Post

16284 3PC

16616 4shared

35760 4shared_File.Download

34742 4shared_File.Upload

- 5ch
- 5ch_Post

38923 8tracks

17045 9PFS

16554 126.Mail

23345 360.Safeguard.Update

35963 360.Yunpan

35967 360.Yunpan_File.Download

35966 360.Yunpan_File.Upload

42324 360.Yunpan_Login

16413 A.N

31529 ABC

…

Only the first 20 have been listed here.

#### Category ID number

The ID numbers for the categories in the CLI are found in the same manner as the applications. When the type is set to category, run the command: set category ? <enter>

This list is shorter.

set category

ID Select Category ID

- P2P
- VoIP
- Video/Audio
- Proxy
- Access
- Game

12 General.Interest

15 Network.Service

17 Update

- Backup
- Media
- Client
- Industrial
- Collaboration
- Business
- IT
- Mobile

### What is not expressly allowed is denied

One of the fundamental ideas that can be found in just about any firewall is the rule than anything that is not expressly allowed is by default denied. This is the foundation for any strategy of protecting your network. Right out of the box, once you have your FortiGate device connected into your network and hooked up with your ISP, your network is protected. Nothing is getting out or in so it is not very convenient, but you don’t have to worry that between the time you hooked it up and the point that you got all of the policies in place that someone could have gotten in and done something to your resources. The reason that this needs to be kept in mind when designing policies is because you cannot assume that any traffic will be allowed just because it makes sense to do so. If you want any kind of traffic to make it past the FortiGate firewall you need to create a policy that will allow that traffic. To maintain the protection of the network should also make sure that the any policy you create allows only the traffic you intend to go only to where you specifically want it to go and when you want it to go there.

**Example**

You have a web server on your network that is meant to provide a collaborative work environment web site for your employees and a partner company for a project over the course of the next 3 months.

It is theoretically possible to allow connections into your network to any device on that network for any service and at any time. The problem with this is that we might not want just anybody looking at those resources. Sadly, no matter how much it is wished otherwise, not everybody on the Internet can be trusted. Which means we now have to be very specific in our instructions as to what traffic to allow into the network. Each step that we take towards being more specific as to what we allow means that there is that much more that is not allowed and the level of protection of a resources is directly proportional to the amount of traffic that is not allowed. If somebody can’t get at it they can’t damage or steal it.

Limiting where the traffic is allowed to go to means that other computers on your network besides the web-server are protected.

- Limiting where the traffic is allowed to come from means that, if feasible, you can limit the systems that can access the web server to just employees or the partner company computers.
- Limiting the services to just web traffic means that a malicious person, even if they were connection from a computer at the partner organization could only use the features of web traffic to do anything malicious.
- Limiting the policy to the time span of the project would mean that even if the IT department forgot to remove the policy after the end of the project than no computer from the other company could be used to do anything malicious through the policy that allowed the traffic.

This is just a very basic example but it shows the underlying principles of how the idea that anything not expressly allowed is by default denied can be used to effectively protect your network.

### Policy order

Another important factor in how firewall policies work is the concept of precedence of order or if you prefer a more recognizable term, “first come, first served”.

It is highly likely that even after only a relatively small number of policies have been created that there will be some that overlap or are subsets of the parameters that the policies use to determine which policy should be matched against the incoming traffic. When this happens there has to be a method to determine which policy should be applied to the packet. The method which is used by most firewalls it based on the order of the sequence of the policies.

If all of the policies were placed in a sequential list the process to match up the packet would start at the top of the list and work its way down. It would compare information about the packet, specifically these points of information:

1. The interface the packet connected to the FortiGate firewall
2. The source of the packet. This can include variations of the address, user credentials or device
3. The destination of the packet. This can include address or Internet service
4. The interface the packet would need to use to get to the destination address based on the routing table
5. The service or port the packet is destined for
6. The time that the packet connected to the FortiGate

As soon as the a policy is reached that matches all of the applicable parameters, the instructions of that policy are applied and the search for any other matching policies is stopped. All subsequent policies are disregarded. Only 1 policy is applied to the packet.

If there is no matching policy among the policies that have been configured for traffic the packet finally drops down to what is always the last policy. It is an implicit policy. One of a few that are referred to by the term “policy0”. This policy denies everything.

The implicit policy is made up of the following settings:

l Incoming Interface: any l Source Address: any l Outgoing Interface: any l Destination Address: any l Action: DENY

The only setting that is editable in the implicit policy is the logging of violation traffic.

A logical best practice that comes from the knowledge of how this process works is to make sure that the more specific or specialized a policy is, the closer to the beginning of the sequence it should be. The more general a policy is the higher the likelihood that it could include in its range of parameters a more specifically targeted policy. The more specific a policy is, the higher the probability that there is a requirement for treating that traffic in a specific way.

**Example**

For security reasons there is no FTP traffic allowed out of a specific subnet so there is a policy that states that any traffic coming from that subnet is denied if the service is FTP, so the following policy was created:

**Policy #1**

| **Source Interface** | Internal1 | 
| **Source Address** | 192.168.1.0/24 | 
| **Source User(s)** | <left at default setting> | 
| **Source Device Type** | <left at default setting> | 
| **Outgoing****Interface** | WAN1 | 
| **Destination Address** | 0.0.0.0/0.0.0.0 | 
| **Service** | FTP | 
| **Schedule** | always | 
| **Action** | deny | 

Now as these things usually go it turns out that there has to be an exception to the rule. There is one very secure computer on the subnet that is allowed to use FTP and once the content has been checked it can them be distributed to the other computer on the subnet. So a second firewall policy is created.

**Policy #2**

| **Source Interface** | Internal1 | 
| **Source Address** | 192.168.1.38/32 | 
| **Source User(s)** | <left at default setting> | 
| **Source Device Type** | <left at default setting> | 
| **Outgoing****Interface** | WAN1 | 
| **Destination Address** | 0.0.0.0/0.0.0.0 | 
| **Service** | FTP | 
| **Schedule** | always | 
| **Action** | Allow | 

By default, a policy that has just been created will be placed last in the sequence so that it is less likely to interfere with existing policies before it can be moved to its intended position. If you look at Policy #2 you will notice that it is essentially the same as Policy #1 exempt for the Source Address and the Action. You will also notice that the Source Address of the Policy #2 is a subset of the Source address in policy #1. This means that if nothing further is done, Policy #2 will never see any traffic because the traffic will always be matched by Policy #1 and processed before it has a chance to reach the second policy in the sequence. For both policies to work as intended Policy #2 needs to be moved to before Policy #1 in the sequence.

### Policy identification

There are two ways to identify a policy. The most obvious is the policy name and this is easily read by humans, but with a little effort it is possible to have a policy without a name, therefore every policy has an ID number.

When looking at the policy listing it can appear as if the policies are identified by the sequence number in the far left column. The problem is that this number changes as the position of the policy in the sequence changes. The column that correctly identifies the policy, and the value sticks with the policy is the “ID” column. This column is not shown by default in the listing but can be added to the displayed columns by right clicking on the column heading bar and selecting it from the list of possible columns.

When looking in the configuration file the sequence is based upon the order of the policies as they are in the file just as they are in the list in the GUI. However, if you need to edit the policy in the CLI you must use the ID number.

### UUID support

Universally Unique Identifier (UUID) attributes have been added to policies to improve functionality when working with FortiManager or FortiAnalyzer units. If required, the UUID can be set manually through the CLI.

CLI Syntax:

config firewall {policy/policy6/policy46/policy64} edit 1 set uuid <example uuid: 8289ef80-f879-51e2-20dd-fa62c5c51f44> next

end

### NTurbo support CAPWAP traffic

NTurbo is used for IPSec+IPS case. The IPSec SA info is passed to NTurbo as part of VTAG for control packet and will be used for the xmit.

If the packets need to go through IPSec interface, the traffic will be always offloaded to NTurbo. But for the case that SA has not been installed to NP6 because of hardware limitation or SA offload disable, the packets will be sent out through raw socket by IPS instead of NTurbo, since the software encryption is needed in this case.

**CLI :**

Previously, NTurbo could only be enabled or disabled globally. The setting of np-acceleration has been added to the firewall policy context instead of just the global context.

CLI command in the firewall policy to enable/disable NTurbo acceleration.

config firewall policy edit 1 set np-accelation [enable|disable] end

When IPS is enabled for VPN IPsec traffic, the data can be accelerated by NTurbo.

### Learning mode for policies

The learning mode feature is a quick and easy method for setting a policy to allow everything but to log it all so that it can later be used to determine what restrictions and protections should be applied. The objective is to monitor the traffic not act upon it while in Learning mode.

Once the **Learn** action is enabled, functions produce hard coded profiles that will be enabled on the policy. The following profiles are set up:

- AntiVirus (av-profile) l Web Filter ( webfilter-profile) l Anti Spam( spamfilter-profile ) l Data Leak Prevention (dlp-sensor ) l Intrusion Prevention (ips-sensor ) l Application Control (application-list ) l Proxy Options (profile-protocol-options)

Profiles that are not being used are:

- DNS Filter (Does not have a Flow mode) l Web Application Firewall(Does not have a Flow mode) l CASI(Almost all signatures in CASI require SSL deep inspection. Without SSL inspection, turning on CASI serves little purpose)

The ability to allow policies to be set to a learning mode is enabled on a per VDOM basis.

config system settings set gui-policy-learning [enable | disable] end

Once the feature is enabled on the VDOM, Learn is an available **Action** option when editing a policy.

Once the Learning policy has been running for a sufficient time to collect needed information a report can be looked at by going to **Log & Report > Learning Report**.

The Report can be either a **Full Report** or a **Report Summary** The time frame of the report can be **5 minutes**, **1 hour**, or **24 hours**.

The Learning Report includes: **Deployment Methodology** l Test Details l Start time l End time l Model


Policy modes

- Firmware
- Policy List

**Executive Summary** l Total Attacks Detected l Top Application Category l Top Web Category l Top Web Domain l Top Host by Bandwidth l Host with Highest Session Count **Security and Threat Prevention** l High Risk Applications l Application Vulnerability Exploits l Malware, botnets and Spyware/Adware l At-Risk Devices and Hosts **User Productivity** l Application Usage l Top Application Categories l Top Social Media Applications l Top Video/Audio Streaming Applications l Top Peer to Peer Applications l Top Gaming Applications

- Web Usage l Top Web Categories l Top Web Applications l Top Web Domains

## Policy modes

You can operate your FortiGate or individual VDOMs in **Next Generation Firewall (NGFW) Policy Mode**.

You can enable NGFW policy mode by going to **System > Settings**, setting the **Inspection mode** to **Flowbased** and setting the NGFW mode to **Policy-based**. When selecting **NGFW policy-based** mode you also select the SSL/SSH Inspection mode that is applied to all policies

**Flow-based** inspection with profile-based **NGFW mode** is the default in FortiOS 5.6.

Or use the following CLI command: config system settings

Policy modes

set inspection-mode flow

set ngfw-mode {profile-based | policy-based}

end

### NGFW policy mode and NAT

If your FortiGate is operating in NAT mode, rather than enabling source NAT in individual NGFW policies you go to **Policy & Objects > Central SNAT** and add source NAT policies that apply to all matching traffic. In many cases you may only need one SNAT policy for each interface pair. For example, if you allow users on the internal network (connected to port1) to browse the Internet (connected to port2) you can add a port1 to port2 Central SNAT policy similar to the following:

### Application control in NGFW policy mode

You configure **Application Control** simply by adding individual applications to security policies. You can set the action to accept or deny to allow or block the applications.

Policy modes

### Web filtering in NGFW mode

You configure **Web Filter** by adding URL categories to security policies. You can set the action to accept or deny to allow or block the applications.


### Other NGFW policy mode options

You can also combine both application control and web filtering in the same NGFW policy mode policy. Also if the policy accepts applications or URL categories you can also apply Antivirus, DNS Filtering, and IPS profiles in NGFW mode policies as well a logging and policy learning mode.

TJ
Hi,

In security profiles > custom signatures, I only see options to create a new IPS signature or a new application signature. I am using NGFW policy-based mode.

I no longer see a way to ready the applications or categories, which existed before changing to policy-based mode.