Cisco Meraki’s dashboard for managing your entire end-to-end network infrastructure in one platform (from anywhere) is the solution you’ve been seeking. Meraki creates a variety of incredible products for companies of all sizes. Whether you’re with a global enterprise looking for a solution that can manage a massive network or a small business that needs a scalable option, Meraki devices are ideal.
Cisco Meraki’s configuration process is user-friendly but can be intimidating for new users. So, should you do it yourself? Or should you call in a professional? We’ll review, step-by-step, how to proceed with your dashboard configuration. Then you can decide whether you’re going to DIY the task or let Stratus Information Services take care of the heavy lifting.
What Is Cisco Meraki Used For?
Before we get into the Cisco Meraki configuration guide, let’s briefly discuss what Cisco Meraki products are used for. As a tech professional with a busy schedule, you recognize that the decision to invest in Meraki products comes with substantial benefits. These are top-of-the-line devices that can transform the way your company handles:
- 
IT issues
- 
Network security
- 
Network visibility
- 
Mobile device management
Cisco Meraki offers several main device types, including MX security appliances, MS switches, and MR wireless access points. Meraki networks can include multiple device types, allowing for flexible and scalable network deployments.
Essentially, you can think of Meraki products as the unification of your network management system. They can control thousands of mobile and desktop devices through one ultra-convenient, secure dashboard. Meraki networks act as logical containers for multiple devices, making management and permission settings more streamlined and efficient.
Cisco Meraki provides a centralized cloud management platform for all Meraki devices and services. Its solutions enhance network security through cloud-managed firewalls and access points.
Can I Configure Cisco Meraki Products By Myself?
After receiving their shiny new hardware, most people start wondering how to configure their Cisco Meraki products. A common question that comes up is whether or not people can fly solo without expert help.
The short answer is: Yes, you can, but you don’t have to.
For IT professionals, the process might be a breeze. For those that feel less equipped to take this project on, start by following our six-step guide, and if you get stuck, reach out to us here at Stratus Information Systems. Our team is on standby, ready to jump in and help you untangle the cords.
Setting Up the Meraki Cloud
Setting up the Meraki cloud is the foundation for managing your Cisco Meraki devices from anywhere in the world. The Meraki cloud leverages a centralized, web-based Meraki dashboard, giving users the power to configure, monitor, and manage all their Meraki devices through a single dashboard account. Once you’ve created your Meraki dashboard account and added your devices, you’ll have access to a comprehensive suite of configuration settings and network details, all in one place.
The Meraki dashboard is designed for simplicity and efficiency. From this interface, users can easily configure device settings, monitor network health, and access advanced features like mobile device management and endpoint management through the systems manager network. Whether you’re managing a single site or a global network, the Meraki cloud ensures you can access and control your network infrastructure securely and remotely. With features like real-time alerts, device inventory, and detailed reporting, the Meraki dashboard empowers users to manage their networks proactively and efficiently—no matter where they are.
6 Steps for Setting Up Your Cisco Meraki Dashboard
As Cisco itself boasts, “There is a tremendous amount of flexibility with the initial setup for a Meraki deployment.” This is good news because “you can configure everything before you even have your devices, thanks to the Meraki cloud.” For multi-site deployments, Configuration Templates can be used to push consistent settings across networks.
Before you begin working your way through the setup, here are other topics you should thoroughly understand first:
- 
Upstream Firewall Rules for Cloud Connectivity
- 
Meraki Cloud Architecture
- 
Meraki Dashboard Organizational Structure
- 
Combined Dashboard Networks
- 
Systems Manager FAQ
- 
MS Warm Sparew)
- 
Cisco Meraki Licensing Guidelines and Limitations
- 
Meraki documentation: Refer to official Meraki documentation for authoritative configuration steps and best practices.
Here are the six steps to set up your Cisco Meraki dashboard correctly.
1. Gather Your Information for Pre-Setup
This is what you’ll need on hand for setting up your dashboard account, network, and devices:
- 
An order number for your Meraki purchase OR the serial #s of your Meraki devices
- 
A plan for how you will group your devices (as well as what you are going to do with them)
- 
An internet uplink for your devices 
Note: The only prerequisite to set up a Meraki device is an uplink connection on the device itself.
- 
A valid firewall with configured rules
When deploying Cisco Meraki devices, follow the recommended deployment hierarchy: install the Router (MX) first, then the Switch (MS), and finally the Access Point (MR).
When creating a network within your organization, be sure to select the appropriate device types—MX for routers, MS for switches, and MR for access points—to match your deployment needs.
Physical installation requires mounting the hardware and connecting it to the internet.
To create a dashboard account, register at dashboard.meraki.com.
2. Create a Meraki Dashboard Account
If you don’t already have an account, it’s time to create one. Here’s how:
- 
Navigate to Meraki’s Dashboard Login.
- 
Click “Create an Account.”
- 
You can sign in with your Cisco SSO or create a free account to access the Meraki dashboard.
- 
Choose your Meraki dashboard and organization region. This can’t be changed, so make sure it’s correct!
- 
Enter in your:
- 
Email address for login and communication
- 
Full name that will be displayed on your account
- 
Password that’s 8+ characters long and a combination of upper- and lower-case letters, numbers, and special characters
- 
Company or organization name
- 
Address for default network locations and maps (optional)
- 
Click “Create account.”
- 
Verify your account by checking your email and following the confirmation link.
Once you’re in and your dashboard is set up, you’ll have access to immense visibility and a wealth of features. This intuitive, interactive web interface lets you see reports that keep you updated on your devices’ health, network usage, client information, and more.
Looking for an overview of your hardware and network features? How about the next steps in the setup process? Fire up your Meraki dashboard to access this information and much more.
3. Create a Network
A device network should be created for each physical location. In Cisco Meraki, a network is a logical container for multiple devices, allowing for streamlined management and configuration. Organizations are collections of networks, and user roles with specific permissions are required to manage them. To manage organizations, users need the Meraki Organization (View) permission, and to manage networks, they need the Meraki Networks (View and Manage) permission.
- 
Log into your Meraki dashboard account.
- 
Click “Register Meraki devices” and “Next.”
- 
Input the following information: 
  - 
Name to be used to identify the network
  - 
Network type (select the appropriate device types for your deployment, such as MX for routers, MS for switches, and MR for access points)
  - 
Devices (optional)
- 
- 
Click “Create network.”
After creating a specific network, you can view and configure it from the network page at the network level, where you can manage devices, monitor metrics, and apply configurations.
4. Create a Systems Management Network
There’s a slightly different process for creating a systems manager network for endpoint management:
- 
Log into your Meraki dashboard account.
- 
Click “Set up Systems Manager.”
- 
Click “Next.”
- 
Name your network. Or, if there is a “Network type” drop-down menu, click “EMM (Systems Manager).”
- 
Review and click “Create network.”
Once this step is complete, you’re able to configure the following endpoint management features:
- 
Profiles
- 
Payloads
- 
Applications
- 
Etc.
5. Add Devices
To claim a new device, such as an MX security appliance or MS Series switch, log into your Meraki dashboard, click the ‘Organization’ button, and choose ‘Inventory’. Enter the serial number of the new device in the inventory list and click the blue “Claim” button. Once claimed, the new device will appear in your inventory list and can be added to a specific network. Devices must be added to a network to download their configuration in Cisco Meraki.
Step-by-step:
- 
Log into your Meraki dashboard.
- 
Click ‘Organization’ and select ‘Inventory’.
- 
Enter the serial number of the new device (such as an MX appliance, MS Series switch, or other devices) next to the blue “Claim” button.
- 
Click “Claim.”
- 
Click the checkbox next to any devices you want to be added to the network.
- 
Click “Add to.”
- 
Under “Existing network,” choose the network you created earlier.
- 
Click “Add to existing.”
Note: The MX security appliance is set to send a DHCP request on its internet port by default. Devices require DHCP to pull an IP address and connect to the Meraki cloud.
Switch ports and other devices, such as access points and switches, can also be managed and configured from the dashboard. MS Series switches support advanced features for port management, including storm control, adaptive policies, and secure authentication.
For more information on configuring specific devices, check out these resources:
- 
How to Add and Configure a Cisco Meraki Access Point
- 
This Is How to Configure a Meraki Firewall
- 
How to Connect a Meraki Switch Locally
6. Add Licenses
If you ordered your Meraki device license separately, you might need to manually add it:
- 
Log into your Meraki dashboard.
- 
Click “Organization.”
- 
Click “Configure.”
- 
Click “License info.”
- 
Click “Add another license.”
- 
Click “License more devices” for the “Operation.”
- 
Enter the “License key.”
- 
Click “Add license.”
- 
Review the details for accuracy.
- 
Click “Add license.”
Congrats! Your Meraki network and devices are now ready for configuration.
Managing Multiple Devices
Managing multiple devices across your organization doesn’t have to be complicated. The Meraki dashboard streamlines the process, allowing users to add and oversee multiple devices from a single dashboard account. With intuitive tools for device network and hardware network management, you can organize your Meraki devices by location, function, or department, ensuring every device is accounted for and configured correctly.
The systems manager network feature further enhances your ability to manage multiple devices, providing centralized control over device policies, security settings, and application deployments. The Meraki dashboard also supports advanced configuration options such as IP assignment and static IP setup, making it easy to tailor your network to your organization’s needs. Features like spanning tree help prevent network loops and ensure optimal performance across all connected devices. With these capabilities, users can efficiently configure, monitor, and manage multiple devices, keeping their network secure, scalable, and easy to maintain.
3 Common Configuration Issues and How to Solve Them
Whether you’re an IT veteran or not, there is always room for potential technical difficulties in the Meraki configuration process. Here are three potential configuration issues you may run into and some tips on how to solve them.
If you are unable to access the Meraki dashboard during initial setup, try connecting an ethernet cable directly from your computer to the MX appliance. This allows you to access the Meraki offline setup page, where the following page (login page) will appear for initial configuration.
Note: Once devices are online, they will automatically download the latest firmware, which is indicated by a flashing LED. To verify device connectivity, look for a solid green light on Access Points or a solid white light on MX/MS devices.
1. Misconfigured DNS
Because Meraki devices rely on DNS to repair dashboard URLs, if your device is reporting issues with its DNS configuration, it’s likely not receiving responses to DNS requests.
Try these things to address this issue:
- 
Verify that the device’s static IP has been working, and make sure that the address is still valid.
- 
Check for a typo or otherwise incorrect value when assigning the static IP.
- 
Ensure the correct VLAN is used for DHCP.
Lastly, you can try switching to DHCP. You would have received this error message only if the device found another working IP address. Once you switch the IP assignment to DHCP instead of static, the device will use the correct address, and the error will go away eventually.
2. Conflicting Uplink IP Address
If you’re getting an alert about a conflicting uplink IP address, another device in the network is using the same IP address as your Meraki hardware.
If this problem persists, try the following tips:
- 
Make sure your devices all have unique IP addresses.
- 
Check by opening the client’s page on your dashboard and finding the matching IP addresses.
3. Poorly Configured IP Assignment
Here are some quick troubleshooting tips to address this issue:
- 
Verify that the device’s static IP has been working, and make sure that the address is still valid.
- 
Check for a typo or otherwise incorrect value when assigning the static IP.
- 
Ensure the correct VLAN is used for DHCP.
Best Practices for Configuration
To maximize the performance and security of your Cisco Meraki deployment, it’s essential to follow best practices for configuration. Start by creating a dedicated Meraki dashboard account and promptly adding all your Meraki devices to the dashboard. This ensures centralized visibility and control from the outset. When configuring your devices, assign static IP addresses where appropriate to maintain consistent connectivity and simplify troubleshooting.
Implement spanning tree protocols to prevent network loops and maintain a resilient network topology. Always keep your Meraki devices updated with the latest firmware and software releases—this not only unlocks new features but also ensures your network benefits from the latest security enhancements. Regularly monitor your network and devices through the Meraki dashboard, using built-in analytics and alerts to proactively address issues before they impact your users. By adhering to these best practices, you’ll ensure your Meraki network is robust, secure, and ready to scale as your organization grows.
Get Expert Guidance
If you find setting up your Cisco Meraki Dashboard to be outside your comfort level, above or below your paygrade, or beyond the time you have to spare, no worries.
Stratus Information Systems can give you the expert guidance you need to work through it yourself or do it completely for you. Check out our options for Cisco Meraki configurations and make sure you get in touch if we can help.