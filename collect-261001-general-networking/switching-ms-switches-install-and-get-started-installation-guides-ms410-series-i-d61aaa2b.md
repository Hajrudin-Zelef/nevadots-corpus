MS410 Series Installation Guide
About this Guide
This guide provides instruction on how to install and configure your MS410 series switch. This guide also provides mounting instructions and limited troubleshooting procedures. For more switch installation guides, refer to the switch installation guides section on our documentation website.
Product Overview
Models
| Model number | Description | 
|---|---|
| MS410-16 | Layer-3 16-port 1Gbe SFP and 2-port 10Gbe SFP+ aggregation switch with 1 management interface and hot-swappable power supplies / fans | 
| MS410-32 | Layer-3 32-port 1Gbe SFP and 4-port 10Gbe SFP+ aggregation switch with 1 management interface and hot-swappable power supplies / fans | 
Physical Specifications
|  | MS410-16 | MS410-32 | 
| 1GbE SFP | 16 | 32 | 
| 10GbE SFP+ uplink | 2 | 4 | 
| 40G QSFP+ Stacking ports | 2 | 2 | 
| Dedicated Mgmt Interface | 1 | 1 | 
| Hot Swap Power Supply | Yes, Dual | Yes, Dual | 
| Hot Swap Fans | Yes, 2x | Yes, 2x | 
| Power Input | 100 - 240 VAC, 47-63 Hz | 100 - 240 VAC, 47-63 Hz | 
| Power Consumption | 50-85W | 50-85W | 
| Operating Temperature | 32°F - 104 °F 0°C - 40 °C | 32°F - 104 °F 0°C - 40 °C | 
| Storage and Transportation Temperature | -4°F - 158°F -20°C - 70°C | -4°F - 158°F -20°C - 70°C | 
| Humidity | 5% to 95% | 5% to 95% | 
| Mounting | 1U Rack Mount | 1U Rack Mount | 
Product View and Physical Features
MS410-16 Series front panel
MS410-32 Series front panel
In addition, there is a RESTORE button available on the front panel.
Insert a paper clip if a restore is required.
- A brief, momentary press: To delete a downloaded configuration and reboot.
- Press and hold for more than 10 sec: To force the unit into a full factory restore.
Ports and Status Indicators
The MS uses LEDs to inform the user of the device's status. When the device powers on, the main LED will be amber. Additional functions are described below, from left to right.
| Item | Function | LED Status | Meaning | 
| 1 | Restore | N/A | Restore button to clear switch IP and local configuration settings | 
| 2 | Fan | Orange | One or more of the system fans has malfunctioned or is missing | 
| 3 | Power (PSU) | Orange | One of the system PSUs (power supplies) has malfunctioned or is missing | 
| 2 | Status | Orange | Switch is unable to or has not yet connected to the Meraki cloud | 
|  |  | Flashing green | Firmware upgrade in process | 
|  |  | White | Switch is fully operational and connected to the Meraki cloud | 
|  |  | Rainbow | Switch is booting, searching for uplink to Meraki Cloud | 
|  |  | Off | Switch does not have power | 
| 3 | Switch Port LEDs | Off | No client connected | 
|  |  | Solid orange | 1 Gbps on SFP+ | 
|  |  | Solid green | 1 Gbps on SFP/10 Gbps on SFP+ | 
Factory Reset Button
If the button is pressed and held for at least 10 seconds and then released, the switch will reboot and be restored to its original factory settings by deleting all configuration information stored on the unit.
Insert a paper clip if a restore is required.
- A brief, momentary press: To delete a downloaded configuration and reboot.
- Press and hold for more than 10 sec: To force the unit into a full factory restore.
Back Panel
| Power input | Power cords may be ordered separately. | 
| Function | LED Status | Meaning | 
| Fan | Orange | One or more of the system fans has malfunctioned or is missing | 
| Power (PSU) | Orange | One of the system PSUs (power supplies) has malfunctioned or is missing | 
| Management Port | Green | Connected, used for easy access to the local status page | 
Equipment is to be used only in a restricted access location and installed/operated only by trained service personnel.
Package contents
In addition to the MS switch, the following are provided (mounting kit provided with 1U models only):
- US 12-24 mounting screws and cage nuts, 5 of each
- INTL M5 mounting screws and cage nuts, 5 of each
- INTL M6 mounting screws and cage nuts, 5 of each
- Mounting washers
The MS410 series will ship with all fans and a single power supply included, additional accessories including spare fans and power supplies can be purchased separately.
Safety and Warnings
These operations are to be taken with respect to all local laws. Please take the following into consideration for safe operation:
- Power off the unit before you begin. Read the installation instructions before connecting the system to the power source.
- Before you work on any equipment, be aware of the hazards involved with electrical circuitry and be familiar with standard practices for preventing accidents.
- Read the mounting instructions carefully before beginning installation. Failure to use the correct hardware or to follow the correct procedures could result in a hazardous situation to people and damage to the system.
- This product relies on the building’s installation for short-circuit (overcurrent) protection. Ensure that the protective device is rated not greater than: 15 A, 125 Vac, or 10A, 240 Vac.
- Please only power the device with the provided power cables to ensure regulatory compliance.
Pre-install Preparation
You should complete the following steps before going on-site to perform an installation.
Configure your Dashboard Network
The following is a brief overview only of the steps required to add a switch to your network. For detailed instructions about creating, configuring and managing Meraki networks, refer to the online documentation (documentation.meraki.com).
- Login to http://dashboard.meraki.com. If this is your ﬁrst time, create a new account.
- Find the network to which you plan to add your switches or create a new network.
- Add your switches to your network. You will need your Meraki order number (found on your invoice) or the serial number of each switch, which looks like Qxxx-xxxx-xxxx, and is found on the bottom of the unit. You will also need your Enterprise license key, which you should have received via email.
- Go to the map / ﬂoor plan view and place each switch on the map by clicking and dragging it to the location where you plan to mount it.
Check and Set Firmware
To ensure your switch performs optimally immediately following installation, it is recommended that you facilitate a ﬁrmware upgrade prior to mounting your switch.
- Attach your switch to power and a wired Internet connection.
- The switch will turn on and the power LED will glow solid orange.
- If the unit requires an upgrade, the power LED will begin blinking white until the upgrade is complete, at which point the LED will turn solid white. You should allow at least a few minutes for the ﬁrmware upgrade to complete, depending on the speed of your internet connection.
Check and Configure Upstream Firewall Settings
If a ﬁrewall is in place, it must allow outgoing connections on particular ports to particular IP addresses. The most current list of outbound ports and IP addresses for your particular organization can be found on the firewall configuration page in your dashboard.
Assigning an IP Address
All switches must be assigned routable IP addresses. These IP addresses can be dynamically assigned via DHCP or statically assigned.
Dynamic Assignment
When using DHCP, the DHCP server should be configured to assign a static IP address for each MAC address belonging to a Meraki switch. Other features of the network, such as 802.1X authentication, may rely on the property that the switches have static IP addresses.
Static Assignment
Static IPs are assigned using the local web server on each switch. The following procedure describes how to set the static IP:
- Using a client machine (e.g., a laptop), connect to the switch over a wired connection.
- Using a web browser on the client machine, access the switch’s built-in web server by browsing to http://my.meraki.com. Alternatively, browse to http://1.1.1.100
- Click on the “Uplink Conﬁguration” tab. Log in. The default login is the serial number (e.g. Qxxx-xxxx-xxxx), with no password (e.g., Q2DD-551C-ZYW3).
- Configure the static IP address, net mask, gateway IP address and DNS servers that this switch will use on its management connection.
- If necessary, reconnect the switch to the LAN.
Static IP via DHCP Reservations
Instead of associating to each Meraki switch individually to configure static IP addresses, an administrator can assign static IP addresses on the upstream DHCP server. Through “DHCP reservations,” IP addresses are “reserved” for the MAC addresses of the Meraki switches. Please consult the documentation for the DHCP server to conﬁgure DHCP reservations.
Installation Instructions
Note: Each switch comes with a graphical instruction pamphlet within the box. This pamphlet contains detailed step by step guides and images to assist in the physical install of the switch.
1. Install the mounting cage nuts in the rack being used for the switch.
2. Attach the switch face plate to the cage nuts on the rack.
3. Insert the power supply unit into the back of the switch. After it has been securely installed, you can connect power to the power supply unit.
Mounting hardware
The mounting hardware includes integrated mounting ears for standard 1U racks. When installing the device, make sure that there is sufficient space between the rear of the rack and other obstacles to ensure adequate airflow.
Bringing your Stack Online
The steps below explain how to prepare a group of switches for physical stacking, how to stack them together, and how to configure the stack in Dashboard.
- Add the switches into a Dashboard network. This can be a new Dashboard network for these switches, or an existing network with other switches. Do not configure the stack in Dashboard yet.
- Connect each switch with individual uplinks to bring them both online and ensure they can check in with the Meraki Dashboard.
- Download the latest firmware build using the Firmware Upgrade Manager under Organization > Monitor > Firmware Upgrades, if they are not already set for this. This helps ensure each switch is running the same firmware build.
- With all switches powered off and links disconnected, connect the switches together via stacking cables in a ring topology (as shown in the following image). To create a full ring, start by connecting switch 1/stack port 1 to switch 2/stack port 2, then switch 2/stack port 1 to switch 3/stack port 2 and so forth, with the bottom switch connecting to the top switch to complete the ring.
- Connect one uplink for the entire switch stack.
- Power on all the switches, then waits several minutes for them to download the latest firmware and updates from Dashboard. The switches may reboot during this process.
    
  - The power LEDs on the front of each switch will blink during this process.
  - Once the switches are done downloading and installing the firmware, their power LEDs will stay solid white or green.
- Navigate to Switch > Monitor > Switch stacks.
- Configure the switch stack in Dashboard. If Dashboard has already detected the correct stack under Detected potential stacks, click Provision this stack to automatically configure the stack.
Otherwise, to configure the stack manually:
- Navigate to Switch > Monitor > Switch stacks.
- Click add one / Add a stack:
- Select the checkboxes of the switches you would like to stack, name the stack, and then click Create.:
- The configuration is complete and the stack should be up and running.
Basic Troubleshooting
The following steps can be used for troubleshooting basic connectivity issues with your switch.
- 
    Reset the switch
- Factory reset the switch by holding the factory reset button for 5 seconds
- Try switching cables, or testing your cable on another device
If your switch still does not connect, the following link may be useful, depending on your issue: Troubleshooting an MS Switch
Reference https://documentation.meraki.com/MS for additional information and troubleshooting tips.
If you are still experiencing hardware issues, please open a case via Meraki dashboard, click the ? icon in the top right of the menu bar and select Support Center. Select the relevant Product or Platform tile, then follow the prompts to enter your case details and submit. For full steps, see Ways to Contact Meraki Support.
Warranty
MS Warranty coverage periods are as follows:
|  | Time Period | Comments | 
| MS410 | Lifetime |  | 
| MS Accessories | 1 Year | The following are considered accessories: SFP Modules, twinax/SFP+ cables, stacking cables, all mounting kits and stands, antennas, interface modules, additional power cords, PoE injectors | 
Note: The above table is a general guideline for warranty terms and is not final. Warranty terms are subject to printed warranty information on the relevant online Meraki data sheets.
If your Cisco Meraki device fails and the problem cannot be resolved by troubleshooting, contact support to address the issue. Once support determines that the device is in a failed state, they can process an RMA and send out a replacement device free of charge. In most circumstances, the RMA will include a pre-paid shipping label so the faulty equipment can be returned.
In order to initiate a hardware replacement for non-functioning hardware that is under warranty, you must have access to the original packaging the hardware was shipped in. The original hardware packaging includes device serial number and order information, and may be required for return shipping.
Meraki MS410 devices have been tested and found to comply with the limits for a Class A digital device, pursuant to part 15 of the FCC rules. A digital device that is marketed for use in a residential environment notwithstanding use in commercial, business and industrial environments.
Additional warranty information can be found on: https://meraki.cisco.com/support#process:warranty
Support and Additional Information
If issues are encountered with device installation or additional help is required, contact Meraki Support by logging in to dashboard.meraki.com and opening a case by visiting the Get Help section.
- The equipment is intended for industrial or other commercial activities.
- The equipment is used in areas without exposure to harmful and dangerous production factors, unless otherwise specified in the operational documentation and/or on the equipment labeling.
- The equipment is not for domestic use. The equipment is intended for operation without the constant presence of maintenance personnel.
- The equipment is subject to installation and maintenance by specialists with the appropriate qualifications, sufficient specialized knowledge, and skills.
- Rules and conditions for the sale of equipment are determined by the terms of contracts concluded by Cisco or authorized Cisco partners with equipment buyers.
- Disposal of a technical device at the end of its service life should be carried out in accordance with the requirements of all state regulations and laws.
- Do not throw in the device with household waste. The technical equipment is subject to storage and disposal in accordance with the organization's disposal procedure.
- The equipment should be stored in its original packaging in a room protected from atmospheric precipitation. The permissible temperature and humidity ranges during storage are specified in the Operation (Installation) Manual.
- Transportation of equipment should be carried out in the original packaging in covered vehicles by any means of transport. The temperature and humidity during transportation must comply with the permissible established ranges of temperature and humidity during storage (in the off state) specified in the Operation Manual (Installation).
For additional information on Meraki hardware and for other installation guides, please refer to documentation.meraki.com.