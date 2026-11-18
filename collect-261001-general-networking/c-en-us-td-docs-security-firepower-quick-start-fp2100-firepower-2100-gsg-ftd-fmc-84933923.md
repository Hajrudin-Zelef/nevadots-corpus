Before You Start
Deploy and perform initial configuration of the Firewall Management Center. See the getting started guide for your model.
| Note | Version 7.4 is the final release for the Firepower 2100. | 
Is This Chapter for You?
To see all available applications and managers, see Which Application and Manager is Right for You?. This chapter applies to the Firewall Threat Defense with the Firewall Management Center.
This chapter explains how to manage the Firewall Threat Defense with a Firewall Management Center located on your management network. For remote branch deployment, where the Firewall Management Center resides at a central headquarters, see Firewall Threat Defense Deployment with a Remote Firewall Management Center.
About the Firewall
The hardware can run either Firewall Threat Defense software or ASA software. Switching between Firewall Threat Defense and ASA requires you to reimage the device. You should also reimage if you need a different software version than is currently installed. See Cisco Secure Firewall ASA and Secure Firewall Threat Defense Reimage Guide.
The firewall runs an underlying operating system called the Secure Firewall eXtensible Operating System (FXOS). The firewall does not support the FXOS Secure Firewall Chassis Manager; only a limited CLI is supported for troubleshooting purposes. See the Cisco FXOS Troubleshooting Guide for the Firewall Threat Defense for more information.
Privacy Collection Statement—The firewall does not require or actively collect personally identifiable information. However, you can use personally identifiable information in the configuration, for example for usernames. In this case, an administrator might be able to see this information when working with the configuration or when using SNMP.
See the following tasks to deploy the Firewall Threat Defense with the Firewall Management Center.
|  | Pre-Configuration | Install the firewall. See the hardware installation guide. | 
|  | Pre-Configuration | Review the Network Deployment. | 
|  | Pre-Configuration | Cable the Device. | 
|  | Pre-Configuration | Power on the Device. | 
|  | CLI | (Optional) Check the Software and Install a New Version. | 
|  | CLI or Firewall Device Manager | Complete the Firewall Threat Defense Initial Configuration | 
|  | Firewall Management Center | Log Into the Firewall Management Center. | 
|  | Cisco Commerce Workspace | Obtain Licenses for the Firewall Management Center: Buy feature licenses. | 
|  | Smart Software Manager | Obtain Licenses for the Firewall Management Center: Generate a license token for the Firewall Management Center. | 
|  | Firewall Management Center | Obtain Licenses for the Firewall Management Center: Register the Firewall Management Center with the Smart Licensing server. | 
|  | Firewall Management Center | Register the Firewall Threat Defense with the Firewall Management Center. | 
|  | Firewall Management Center | Configure a Basic Security Policy. | 
The Firewall Management Center communicates with the Firewall Threat Defense on the Management interface.
The dedicated Management interface is a special interface with its own network settings:
By default, the Management 1/1 interface is enabled and configured as a DHCP client. If your network does not include a DHCP server, you can set the Management interface to use a static IP address during initial setup at the console port.
Both the Firewall Threat Defenseand the Firewall Management Center require internet access from their management interfaces for licensing and updates.
| Note | The management connection is a secure, TLS-1.3-encrypted communication channel between itself and the device. You do not need to run this traffic over an additional encrypted tunnel such as Site-to-Site VPN for security purposes. If the VPN goes down, for example, you will lose your management connection, so we recommend a simple management path. | 
You can configure other interfaces after you connect the Firewall Threat Defense to the Firewall Management Center.
The following figure shows a typical network deployment for the firewall where the Firewall Threat Defense, Firewall Management Center, and management computer connect to the management network.
The management network has a path to the internet for licensing and updates.
The following figure shows a typical network deployment for the firewall where:
Inside acts as the internet gateway for Management and for the Firewall Management Center.
Management 1/1 connects to an inside interface through a Layer 2 switch.
The Firewall Management Center and management computer connect to the switch.
This direct connection is allowed because the Management interface has separate routing from the other interfaces on the Firewall Threat Defense.
To cable one of the above scenarios on the Firepower 2100, see the following steps.
| Note | Other topologies can be used, and your deployment will vary depending on your basic logical network connectivity, ports, addressing, and configuration requirements. | 
| Step 1 | Install the chassis. See the hardware installation guide. | 
| Step 2 | Cable for a separate management network: | 
| Step 3 | Cable for an edge deployment: | 
| Note | For version 6.5 and earlier, the Management 1/1 default IP address is 192.168.45.45. | 
The power switch is located to the left of power supply module 1 on the rear of the chassis. It is a toggle switch that controls power to the system. If the power switch is in standby position, only the 3.3-V standby power is enabled from the power supply module and the 12-V main power is OFF. When the switch is in the ON position, the 12-V main power is turned on and the system boots.
| Note | The first time you boot up the Firewall Threat Defense, initialization can take approximately 15 to 30 minutes. | 
It's important that you provide reliable power for your device (for example, using an uninterruptable power supply (UPS)). Loss of power without first shutting down can cause serious file system damage. There are many processes running in the background all the time, and losing power does not allow the graceful shutdown of your system.
| Step 1 | Attach the power cord to the device and connect it to an electrical outlet. | 
| Step 2 | Press the power switch on the back of the device. | 
| Step 3 | Check the PWR LED on the front of the device; if it is solid green, the device is powered on. | 
| Step 4 | Check the SYS LED on the front of the device; after it is solid green, the system has passed power-on diagnostics. | 
| Note | Before you move the power switch to the OFF position, use the shutdown commands so that the system can perform a graceful                                                       shutdown. This may take several minutes to complete. After the graceful shutdown is complete, the console displays It is safe to power off now . The front panel blue locator beacon LED lights up indicating the system is ready to be powered off. You can now move the                                                       switch to the OFF position. The front panel PWR LED flashes momentarily and turns off. Do not remove the power until the PWR                                                       LED is completely off. See the FXOS Configuration Guide for more information on using the shutdown commands. | 
To check the software version and, if necessary, install a different version, perform these steps. We recommend that you install your target version before you configure the firewall. Alternatively, you can perform an upgrade after you are up and running, but upgrading, which preserves your configuration, may take longer than using this procedure.
What Version Should I Run?
Cisco recommends running a Gold Star release indicated by a gold star next to the release number on the software download page. You can also refer to the release strategy described in https://www.cisco.com/c/en/us/products/collateral/security/firewalls/bulletin-c25-743178.html; for example, this bulletin describes short-term release numbering (with the latest features), long-term release numbering (maintenance releases and patches for a longer period of time), or extra long-term release numbering (maintenance releases and patches for the longest period of time, for government certification).
| Step 1 | Connect to the CLI. See Access the Firewall Threat Defense and FXOS CLI for more information. This procedure shows using the console port, but you can use SSH instead. Log in with the admin user and the default password, Admin123. You connect to the FXOS CLI. The first time you log in, you are prompted to change the password. This password is also used for the Firewall Threat Defense login for SSH. Example:  firepower login: admin Password: Admin123 Successful login attempts for user 'admin' : 1  [...]  Hello admin. You must change your password. Enter new password: ******** Confirm new password: ******** Your password was updated successfully.  [...]  firepower#    | 
| Step 2 | At the FXOS CLI, show the running version. scope ssa show app-instance Example:  Firepower# scope ssa Firepower /ssa # show app-instance  Application Name     Slot ID    Admin State     Operational State    Running Version Startup Version Cluster Oper State -------------------- ---------- --------------- -------------------- --------------- --------------- ------------------ ftd                  1          Enabled         Online               7.6.0.65        7.6.0.65        Not Applicable   | 
| Step 3 | If you want to install a new version, perform these steps. | 
| Note | If the password was already changed, and you do not know it, you must perform a factory reset to reset the password to the default. See the FXOS troubleshooting guide for the factory reset procedure. | 
You can complete the Firewall Threat Defense initial configuration using the CLI or Firewall Device Manager.
When you use the Firewall Device Manager for initial setup, the following interfaces are preconfigured in addition to the Management interface and manager access settings. Note that other settings, such as the DHCP server on inside, access control policy, or security zones, are not configured.
Ethernet 1/1—"outside", IP address from DHCP, IPv6 autoconfiguration
Ethernet 1/2— "inside", 192.168.95.1/24
Default route—Obtained through DHCP on the outside interface
If you perform additional interface-specific configuration within Firewall Device Manager before registering with the Firewall Management Center, then that configuration is preserved.
When you use the CLI, only the Management interface and manager access settings are retained (for example, the default inside interface configuration is not retained).
| Step 1 | Log in to the Firewall Device Manager. | 
| Step 2 | Use the setup wizard when you first log into the Firewall Device Manager to complete the initial configuration. You can optionally skip the setup wizard by clicking Skip device setup at the bottom of the page. After you complete the setup wizard, in addition to the default configuraton for the inside interface (Ethernet1/2), you will have configuration for an outside (Ethernet1/1) interface that will be maintained when you switch to Firewall Management Center management. | 
| Step 3 | (Might be required) Configure a static IP address for the Management interface. See the Management interface on . If you want to configure a static IP address, for example for an edge deployment where there is not DHCP server on the network yet, be sure to also set the default gateway to be a unique gateway instead of the data interfaces. If you use DHCP, you do not need to configure anything. | 
| Step 4 | If you want to configure additional interfaces, including an interface other than outside or inside, choose Device, and then click the link in the Interfaces summary. See Configure the Firewall in the Firewall Device Manager for more information about configuring interfaces in the Firewall Device Manager. Other Firewall Device Manager configuration will not be retained when you register the device to the Firewall Management Center. | 
| Step 5 | Choose , and click Proceed to set up the Firewall Management Center management. | 
| Step 6 | Configure the Management Center/Security Cloud Control Details. | 
| Step 7 | Configure the Connectivity Configuration. | 
| Step 8 | Click Connect. The Registration Status dialog box shows the current status of the switch to the Firewall Management Center. After the Saving Management Center/CDO Registration Settings step, go to the Firewall Management Center, and add the firewall. If you want to cancel the switch to the Firewall Management Center, click Cancel Registration. Otherwise, do not close the Firewall Device Manager browser window until after the Saving Management Center/CDO Registration Settings step. If you do, the process will be paused, and will only resume when you reconnect to the Firewall Device Manager. If you remain connected to the Firewall Device Manager after the Saving Management Center/CDO Registration Settings step, you will eventually see the Successful Connection with Management Center/CDO dialog box, after which you will be disconnected from the Firewall Device Manager. | 
Set the Management IP address, gateway, and other basic networking settings using the setup wizard. The dedicated Management interface is a special interface with its own network settings. In 6.7 and later: If you do not want to use the Management interface for the manager access, you can use the CLI to configure a data interface instead. You will also configure the Firewall Management Center communication settings. When you perform initial setup using the Firewall Device Manager (7.1 and later), all interface configuration completed in the Firewall Device Manager is retained when you switch to the Firewall Management Center for management, in addition to the Management interface and manager access interface settings. Note that other default configuration settings, such as the access control policy, are not retained.
| Step 1 | Connect to the Firewall Threat Defense CLI, either from the console port or using SSH to the Management interface, which obtains an IP address from a DHCP server by default. If you intend to change the network settings, we recommend using the console port so you do not get disconnected. The console port connects to the FXOS CLI. The SSH session connects directly to the Firewall Threat Defense CLI. | 
| Step 2 | Log in with the username admin and the password Admin123. At the console port, you connect to the FXOS CLI. The first time you log in to FXOS, you are prompted to change the password. This password is also used for the Firewall Threat Defense login for SSH. Example:  firepower login: admin Password: Admin123 Successful login attempts for user 'admin' : 1  [...]  Hello admin. You must change your password. Enter new password: ******** Confirm new password: ******** Your password was updated successfully.  [...]  firepower#    | 
| Step 3 | If you connected to FXOS on the console port, connect to the Firewall Threat Defense CLI. connect ftd Example:  firepower# connect ftd >   | 
| Step 4 | The first time you log in to the Firewall Threat Defense, you are prompted to accept the End User License Agreement (EULA) and, if using an SSH connection, to change the admin password. You are then presented with the CLI setup script. Defaults or previously entered values appear in brackets. To accept previously entered values, press Enter. See the following guidelines:  Example:  You must accept the EULA to continue. Press <ENTER> to display the EULA: End User License Agreement [...]  Please enter 'YES' or press <ENTER> to AGREE to the EULA:   System initialization in progress.  Please stand by. You must change the password for 'admin' to continue. Enter new password: ******** Confirm new password: ******** You must configure the network to continue. Configure at least one of IPv4 or IPv6 unless managing via data interfaces. Do you want to configure IPv4? (y/n) [y]: Do you want to configure IPv6? (y/n) [y]:n Configure IPv4 via DHCP or manually? (dhcp/manual) [manual]: Enter an IPv4 address for the management interface [192.168.45.45]: 10.10.10.15 Enter an IPv4 netmask for the management interface [255.255.255.0]: 255.255.255.192 Enter the IPv4 default gateway for the management interface [data-interfaces]: 10.10.10.1 Enter a fully qualified hostname for this system [firepower]: ftd-1.cisco.com Enter a comma-separated list of DNS servers or 'none' [208.67.222.222,208.67.220.220,2620:119:35::35]: Enter a comma-separated list of search domains or 'none' []:cisco.com If your networking information has changed, you will need to reconnect. Disabling IPv6 configuration: management0 Setting DNS servers: 208.67.222.222,208.67.220.220,2620:119:35::35 Setting DNS domains:cisco.com Setting hostname as ftd-1.cisco.com Setting static IPv4: 10.10.10.15 netmask: 255.255.255.192 gateway: 10.10.10.1 on management0 Updating routing tables, please wait... All configurations applied to the system. Took 3 Seconds. Saving a copy of running network configuration to local disk. For HTTP Proxy configuration, run 'configure network http-proxy'  Manage the device locally? (yes/no) [yes]: no DHCP server is already disabled DHCP Server Disabled Configure firewall mode? (routed/transparent) [routed]: Configuring firewall mode ...   Device is in OffBox mode - disabling/removing port 443 from iptables. Update policy deployment information     - add device configuration     - add network discovery     - add system policy  You can register the sensor to a Firepower Management Center and use the Firepower Management Center to manage it. Note that registering the sensor to a Firepower Management Center disables on-sensor Firepower Services management capabilities.  When registering the sensor to a Firepower Management Center, a unique alphanumeric registration key is always required.  In most cases, to register a sensor to a Firepower Management Center, you must provide the hostname or the IP address along with the registration key. 'configure manager add [hostname \| ip address ] [registration key ]'  However, if the sensor and the Firepower Management Center are separated by a NAT device, you must enter a unique NAT ID, along with the unique registration key. 'configure manager add DONTRESOLVE [registration key ] [ NAT ID ]'  Later, using the web interface on the Firepower Management Center, you must use the same registration key and, if necessary, the same NAT ID when you add this sensor to the Firepower Management Center. >  | 
| Step 5 | Identify the Firewall Management Center that will manage this Firewall Threat Defense. configure manager add {hostname \| IPv4_address \| IPv6_address \| DONTRESOLVE} reg_key [nat_id]  Example:  > configure manager add MC.example.com 123456 Manager successfully configured.If the Firewall Management Center is behind a NAT device, enter a unique NAT ID along with the registration key, and specify DONTRESOLVE instead of the hostname, for example: Example:  > configure manager add DONTRESOLVE regk3y78 natid90 Manager successfully configured.If the Firewall Threat Defense is behind a NAT device, enter a unique NAT ID along with the Firewall Management Center IP address or hostname, for example: Example:  > configure manager add 10.70.45.5 regk3y78 natid56 Manager successfully configured. | 
| Note | If the password was already changed, and you do not know it, you must reimage the device to reset the password to the default. See the FXOS troubleshooting guide for the reimage procedure. | 
| Note | You cannot repeat the CLI setup wizard unless you clear the configuration; for example, by reimaging. However, all of these settings can be changed later at the CLI using configure network commands. See Cisco Secure Firewall Threat Defense Command Reference. | 
Register your firewall to the Firewall Management Center.
Use the Firewall Management Center to configure and monitor the Firewall Threat Defense.
| Step 1 | Using a supported browser, enter the following URL. https://fmc_ip_address | 
| Step 2 | Enter your username and password. | 
| Step 3 | Click Log In. | 
All licenses are supplied to the Firewall Threat Defense by the Firewall Management Center. You can purchase the following licenses:
Essentials—Required
IPS
Malware Defense
URL Filtering
Cisco Secure Client
For a more detailed overview on Cisco Licensing, go to cisco.com/go/licensingguide
When you bought your device from Cisco or a reseller, your licenses should have been linked to your Smart Software License account. If you don't have an account on the Smart Software Manager, click the link to set up a new account.
If you have not already done so, register the Firewall Management Center with the Smart Software Manager. Registering requires you to generate a registration token in the Smart Software Manager. See the Cisco Secure Firewall Management Center Administration Guide for detailed instructions.
| Step 1 | Make sure your Smart Licensing account contains the available licenses you need. If you need to add licenses yourself, go to Cisco Commerce Workspace and use the Search All field. Choose Products & Services from the results. Search for the following license PIDs:  | 
| Step 2 | If you have not already done so, register the Firewall Management Center with the Smart Licensing server. Registering requires you to generate a registration token in the Smart Software Manager. See the Cisco Secure Firewall Management Center Administration Guide for detailed instructions. | 
| Note | If a PID is not found, you can add the PID manually to your order. | 
Register the Firewall Threat Defense to the Firewall Management Center manually using the device IP address or hostname.
| Step 1 | In the Firewall Management Center, choose . | 
| Step 2 | From the Add drop-down list, choose Add Device. Set the following parameters:  | 
| Step 3 | Click Register, and confirm a successful registration. If the registration succeeds, the device is added to the list. If it fails, you will see an error message. If the Firewall Threat Defense fails to register, check the following items:  For more troubleshooting information, see https://cisco.com/go/fmc-reg-error. | 
This section describes how to configure a basic security policy with the following settings:
Inside and outside interfaces—Assign a static IP address to the inside interface, and use DHCP for the outside interface.
DHCP server—Use a DHCP server on the inside interface for clients.
Default route—Add a default route through the outside interface.
NAT—Use interface PAT on the outside interface.
Access control—Allow traffic from inside to outside.
To configure a basic security policy, complete the following tasks.
When you use the Firewall Device Manager for initial setup, the following interfaces are preconfigured:
If you performed additional interface-specific configuration within Firewall Device Manager before registering with the Firewall Management Center, then that configuration is preserved.
In any case, you need to perform additional interface configuration after you register the device. Enable the Firewall Threat Defense interfaces, assign them to security zones, and set the IP addresses. .
The following example configures a routed mode inside interface with a static address and a routed mode outside interface using DHCP.
| Step 1 | Choose , and click the Edit () for the firewall. | 
| Step 2 | Click Interfaces. | 
| Step 3 | Click Edit () for the interface that you want to use for inside. The General tab appears. | 
| Step 4 | Click the Edit () for the interface that you want to use for outside. The General tab appears. | 
| Step 5 | Click Save. | 
Enable the DHCP server if you want clients to use DHCP to obtain IP addresses from the Firewall Threat Defense.
| Step 1 | Choose , and click Edit () for the device. | 
| Step 2 | Choose . | 
| Step 3 | On the Server page, click Add, and configure the following options:  | 
| Step 4 | Click OK. | 
| Step 5 | Click Save. | 
The default route normally points to the upstream router reachable from the outside interface. If you use DHCP for the outside interface, your device might have already received a default route. If you need to manually add the route, complete this procedure. If you received a default route from the DHCP server, it will show in the IPv4 Routes or IPv6 Routes table on the page.
| Step 1 | Choose , and click Edit () for the device. | 
| Step 2 | Choose . | 
| Step 3 | Click Add Route, and set the following:  | 
| Step 4 | Click OK. The route is added to the static route table. | 
| Step 5 | Click Save. | 
A typical NAT rule converts internal addresses to a port on the outside interface IP address. This type of NAT rule is called interface Port Address Translation (PAT).
| Step 1 | Choose , and click . | 
| Step 2 | Name the policy, select the device(s) that you want to use the policy, and click Save. The policy is added the Firewall Management Center. You still have to add rules to the policy. | 
| Step 3 | Click Add Rule. The Add NAT Rule dialog box appears. | 
| Step 4 | Configure the basic rule options:  | 
| Step 5 | On the Interface Objects page, add the outside zone from the Available Interface Objects area to the Destination Interface Objects area. | 
| Step 6 | On the Translation page, configure the following options:  | 
| Step 7 | Click Save to add the rule. The rule is saved to the Rules table. | 
| Step 8 | Click Save on the NAT page to save your changes. | 
If you created a basic Block all traffic access control policy when you registered the Firewall Threat Defense, then you need to add rules to the policy to allow traffic through the device. The following procedure adds a rule to allow traffic from the inside zone to the outside zone. If you have other zones, be sure to add rules allowing traffic to the appropriate networks.
| Step 1 | Choose , and click Edit () for the access control policy assigned to the Firewall Threat Defense. | 
| Step 2 | Click Add Rule, and set the following parameters:  Leave the other settings as is. | 
| Step 3 | Click Apply. The rule is added to the Rules table. | 
| Step 4 | Click Save. | 
Deploy the configuration changes to the Firewall Threat Defense; none of your changes are active on the device until you deploy them.
| Step 1 | Click Deploy in the upper right. | 
| Step 2 | For a quick deployment, check specific devices and then click Deploy, or click Deploy All to deploy to all devices. Otherwise, for additional deployment options, click Advanced Deploy. | 
| Step 3 | Ensure that the deployment succeeds. Click the icon to the right of the Deploy button in the menu bar to see status for deployments. | 
Use the command-line interface (CLI) to set up the system and do basic system troubleshooting. You cannot configure policies through a CLI session. You can access the CLI by connecting to the console port.
You can also access the FXOS CLI for troubleshooting purposes.
| Note | You can alternatively SSH to the Management interface of the Firewall Threat Defense device. Unlike a console session, the SSH session defaults to the Firewall Threat Defense CLI, from which you can connect to the FXOS CLI using the connect fxos command. You can later connect to the address on a data interface if you open the interface for SSH connections. SSH access to data interfaces is disabled by default. This procedure describes console port access, which defaults to the FXOS CLI. | 
| Step 1 | To log into the CLI, connect your management computer to the console port. The Firepower 2100 ships with a DB-9 to RJ-45 serial cable, so you may need a third party DB-9-to-USB serial cable to make the connection. Be sure to install any necessary USB serial drivers for your operating system. The console port defaults to the FXOS CLI. Use the following serial settings:  You connect to the FXOS CLI. Log in to the CLI using the admin username and the password you set at initial setup (the default is Admin123). Example:  firepower login: admin Password: Last login: Thu May 16 14:01:03 UTC 2019 on ttyS0 Successful login attempts for user 'admin' : 1  firepower#    | 
| Step 2 | Access the Firewall Threat Defense CLI. connect ftd Example:  firepower# connect ftd >  After logging in, for information on the commands available in the CLI, enter help or ? . For usage information, see Cisco Secure Firewall Threat Defense Command Reference. | 
| Step 3 | To exit the Firewall Threat Defense CLI, enter the exit or logout command. This command returns you to the FXOS CLI prompt. For information on the commands available in the FXOS CLI, enter ? . Example:  > exit firepower#   | 
It's important that you shut down your system properly. Simply unplugging the power or pressing the power switch can cause serious file system damage. Remember that there are many processes running in the background all the time, and unplugging or shutting off the power does not allow the graceful shutdown of your firewall system.
You can power off the device using the Firewall Management Center device management page, or you can use the FXOS CLI.
It's important that you shut down your system properly. Simply unplugging the power or pressing the power switch can cause serious file system damage. Remember that there are many processes running in the background all the time, and unplugging or shutting off the power does not allow the graceful shutdown of your firewall.
You can shut down your system properly using the Firewall Management Center.
| Step 1 | Choose . | 
| Step 2 | Next to the device that you want to restart, click Edit (). | 
| Step 3 | Click the Device tab. | 
| Step 4 | Click Shut Down Device () in the System section. | 
| Step 5 | When prompted, confirm that you want to shut down the device. | 
| Step 6 | If you have a console connection to the firewall, monitor the system prompts as the firewall shuts down. You will see the following prompt:  System is stopped. It is safe to power off now.  Do you want to reboot instead? [y/N] If you do not have a console connection, wait approximately 3 minutes to ensure the system has shut down. | 
| Step 7 | You can now turn off the power switch and unplug the power to physically remove power from the chassis if necessary. | 
You can use the FXOS CLI to safely shut down the system and power off the device. You access the CLI by connecting to the console port; see Access the Firewall Threat Defense and FXOS CLI.
| Step 1 | In the FXOS CLI, connect to local-mgmt: firepower # connect local-mgmt | 
| Step 2 | Issue the shutdown command: firepower(local-mgmt) # shutdown Example: firepower(local-mgmt)# shutdown  This command will shutdown the system.  Continue? Please enter 'YES' or 'NO': yes INIT: Stopping Cisco Threat Defense......ok | 
| Step 3 | Monitor the system prompts as the firewall shuts down. You will see the following prompt:  System is stopped. It is safe to power off now. Do you want to reboot instead? [y/N]  | 
| Step 4 | You can now turn off the power switch and unplug the power to physically remove power from the chassis if necessary. | 
To continue configuring your Firewall Threat Defense, see the documents available for your software version at Navigating the Cisco Secure Firewall Threat Defense Documentation.
For information related to using the Firewall Management Center, see the Cisco Secure Firewall Management Center Device Configuration Guide.