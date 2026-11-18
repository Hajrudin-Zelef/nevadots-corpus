Hi all,

I recently decided to explore the idea of segmenting my home network into multiple VLANs. I am by no means a Mikrotik expert and my networking knowledge was mostly confined to cloud provider abstractions. Due to this lack of experience it was a frustrating and time-consuming experience.

Instead of just dumping my config here and asking the community if it's correct, I decided I'd rather write this up as a guide so that other people can also benefit from my learnings. I would of course appreciate any recommendations or corrections, and I'll update the guide to reflect these.

 **Disclaimer**: I am not a networking or Mikrotik expert, and this guide is based on my personal experience and learning.

This guide is opinionated and does not claim to be error-free. Please consult the official Mikrotik documentation to verify any commands or configurations that you apply.


# 1. Motivation

There are many reasons why you might want to segment your network with VLANs, but my specific goals were:

1. Control IoT device communication, including the ability to prevent IoT devices from initiating communication with security sensitive devices on my network (such as my laptop).
2. Allow guest devices to access the internet and nothing else (no communication with other devices on the network, including other guest devices).
3. Prepare for various other things I'd like to implement, such as secure access to my home network with a reverse tunnel.
4. Learn more about networks and generally just play around with RouterOS for fun.

There are almost certainly other approaches to achieve these goals, but I wanted to do it with VLANs since this felt like the most flexible and future-proof solution.

# 2. Overview

## 2.1. VLANs

The end result of this guide is a network with the following VLANs:

- **VLAN 10** : Owner Devices  - This VLAN is for my personal devices, such as my laptop, TV and phone.
- **VLAN 20** : IoT Devices  - This VLAN is for my IoT devices, such as a Raspberry Pi (physically connected via ethernet) and Victron Cerbo GX (
connected via Wi-Fi).
- **VLAN 30** : Guest Devices  - This VLAN is for guest devices, such as visitors' phones that connect to Wi-Fi.
- **VLAN 99** : Management  - This VLAN is for management of the router and access points.

Even though this is likely a common setup, you can adjust the VLANs to suit your needs. For example, you might want to create a separate VLAN TVs or place TVs in the IoT VLAN (I decided against this since I wanted to simplify casting from my phone to the TV).

## 2.2. Wi-Fi SSIDs

To assign devices to the correct VLANs, the following Wi-Fi SSIDs will be created:

- **OwnerWifi** : This SSID will be used for my personal devices and will be assigned to VLAN 10.
- **IoTWifi** : This SSID will be used for IoT devices and will be assigned to VLAN 20.
- **GuestWifi** : This SSID will be used for guest devices and will be assigned to VLAN 30.  - This SSID will have client isolation enabled, so guest devices will not be able to communicate with each other.

 Instinctively you might want to create a single SSID for all devices and use MAC address filtering to assign devices to the correct VLANs, but this is generally not recommended.


## 2.3. Hardware

Even though many of the concepts in this guide applies to RouterOS in general, there are some things that are specific to the hardware I am using.

- Router: **Mikrotik RB5009UPr+S+IN**  - The RB5009**UG** +S+IN should work exactly the same - I chose the UPr since I wanted multiple PoE outputs.
- Wireless Access Points: **Mikrotik RBcAPGi-5acD2nD** (x2)  - I already had two of these, so I made do with them.
  - These cAP ac APs unfortunately don't support CAPsMAN based VLAN provisioning at the time of writing, which caused a lot of frustration, but the guide explains how make VLANs work with them.
  - If you use cAP ax APs, you'll have less configuration to do on the APs themselves (more on this later).
- Optical Network Terminal: Huawei EchoLife HG8240H
  - This is the ONT provided by my ISP, it facilitates the connection to the fiber network via GPON and PPPoE.
  - If you use something other than PPPoE to connect to the internet you will need to adjust the configuration accordingly.
  - Configuration changes on the ONT was not necessary in my case, and generally shouldn't be required.
- Computer:
  - Any computer with internet access and an Ethernet port should work.
- Cabling:
  - You will need an Ethernet cable to connect your computer to the router and access points.

## 2.4. Software

I used the latest stable version of RouterOS at the time of writing, but this guide should work with any 7.x version.

See the official Mikrotik download page for the latest versions: https://mikrotik.com/download

- Router:
  - RouterOS: **7.19.2** (`routeros-7.19.2-arm64.npk` in my case)
- Wireless Access Points:
  - RouterOS: **7.19.2** (`routeros-7.19.2-arm.npk` in my case)
  - Wifi QCom AC (`wifi-qcom-ac-7.19.2-arm.npk` in my case)    - It's important to use this package instead of the older `wireless` package if you are using cAP ac APs.
- Configuration software:
  - Windows
    - WinBox is required for this guide since we will use Layer 2 (MAC) based communication for configuration.
    - If you only have Linux available, you can consider using `wine` or virtualization (such as VirtualBox) to run Winbox.
  - WinBox: **3.42**    - The latest version at the time of writing, but any recent version should work.

# 3. Initial Setup

This guide assumes that you are starting with a fresh RouterOS installation without any defaults on both the router and access points.

First we will reset the router and access points.

 This will erase all existing configurations on the router and access points. Make sure to back up any existing configurations you want to retain before proceeding.


1. Connect the router to your computer using an Ethernet cable.
  1. Use **ethernet port 8** , since this port will be a dedicated port for configuration in this guide.
2. Open WinBox and connect to the router using the MAC address.
3. Reset the router to factory defaults```
 /system reset-configuration no-defaults=yes skip-backup=yes  
```
Your router will reboot, and you will need to reconnect to it using WinBox.
4. Configure your username and password
  1. It's best practice to use a non-default username and a strong password.

Repeat the same steps for each of your access points, but connect the access points to your computer using **ethernet port 2**, since this port will be a dedicated port for configuration of the access points.

The next step is to install the required packages on the router and access points.

Since the devices do not have any internet at this point, we will perform a manual upgrade using WinBox.

Please follow this guide to install the packages as specified in the "Software" section above.

 It's highly recommended to also update the RouterBoard firmware after upgrading RouterOS. See the link above.

 If you are using cAP ac APs, make sure to uninstall the older `wireless` package if it's installed.


# 4. Router Configuration

Now that we have a clean RouterOS installation on the router and access points, we can start configuring the router.

We will use the terminal in WinBox to configure the router, but you can also use the GUI if you prefer.

## 4.1. Basic Configuration

```
/system 
# Set the identity
identity set name=router
# Setup initial timezone and time
clock set time-zone-name=Africa/Johannesburg
clock set date="jun/28/2025"
clock set time="15:00:00"
# Use Cloudflare NTP servers for accurate timekeeping
ntp client set enabled=yes servers=time.cloudflare.com
```

 Customization

- Set the `time-zone-name` to your local timezone. You can find a list of timezones
here.
- Set the `date` and`time` to the current date and time (anything close is fine).
- It's not necessary to use an NTP server, and you can use your ISP's NTP server if you prefer.


## 4.2. Ethernet interfaces

We will configure the Ethernet interfaces to have descriptive names and comments for easy identification. This is not strictly necessary, but it will make it easier to follow the configuration and understand what each port is used for.

```
/interface ethernet
set [ find default-name=ether1 ] name=ether1-wan        comment="ISP PPPoE WAN"
set [ find default-name=ether2 ] name=ether2-ap1        comment="Access Point 1"
set [ find default-name=ether3 ] name=ether3-ap2        comment="Access Point 2"
set [ find default-name=ether4 ] name=ether4-laptop     comment="Personal laptop"
set [ find default-name=ether5 ] name=ether5-rpi        comment="Raspberry PI"
set [ find default-name=ether8 ] name=ether8-management comment="Dedicated unbridged port for management access"
set [ find default-name=sfp-sfpplus1 ] disabled=yes
```

 Customization

- Adjust the names and comments to match your setup.
- If you have more or fewer Ethernet ports, adjust the configuration accordingly.
- If you have an SFP+ port, you can keep it enabled it if you plan to use it.


## 4.3. VLANs

Create a bridge and enable VLAN filtering on it.

```
/interface bridge add name=bridge-lan vlan-filtering=yes comment="LAN bridge"
```

Create a VLAN interface for each VLAN we want to use. This allows us to use IP (Layer 3) services such as on each VLAN.

```
/interface vlan
add interface=bridge-lan name=vlan10-owner      vlan-id=10 comment="Owner VLAN"
add interface=bridge-lan name=vlan20-iot        vlan-id=20 comment="IoT VLAN"
add interface=bridge-lan name=vlan30-guest      vlan-id=30 comment="Guest VLAN"
add interface=bridge-lan name=vlan99-management vlan-id=99 comment="Management VLAN"
```

Next add the required bridge ports to the bridge.

```
/interface bridge
# The Wi-Fi access points ports are trunk ports which will carry traffic for mutliple VLANs.
port add bridge=bridge-lan frame-types=admit-only-vlan-tagged interface=ether2-ap1 comment="AP1 Trunk Port"
port add bridge=bridge-lan frame-types=admit-only-vlan-tagged interface=ether3-ap2 comment="AP2 Trunk Port"
# The Ethernet ports for personal devices and IoT devices are access ports which will carry traffic for a single VLAN.
port add bridge=bridge-lan frame-types=admit-only-untagged-and-priority-tagged interface=ether4-laptop pvid=10 comment="Laptop Access Port (VLAN 10)"
port add bridge=bridge-lan frame-types=admit-only-untagged-and-priority-tagged interface=ether5-rpi    pvid=20 comment="Raspberry PI Access Port (VLAN 30)"
```

 Do not add the management port (`ether8-management`) to the bridge, this port is used for unbridged management access only.


Next we configure the bridge VLAN table to restrict which VLANs are allowed on each port.

```
vlan add bridge=bridge-lan tagged=bridge-lan,ether2-ap1,ether3-ap2 untagged=ether4-laptop vlan-ids=10 comment="Owner VLAN with access port for laptop"
vlan add bridge=bridge-lan tagged=bridge-lan,ether2-ap1,ether3-ap2 untagged=ether5-rpi    vlan-ids=20 comment="IoT VLAN with access port for Raspberry PI"
vlan add bridge=bridge-lan tagged=bridge-lan,ether2-ap1,ether3-ap2                        vlan-ids=30 comment="Guest VLAN with access only via Wi-FI AP trunks"
vlan add bridge=bridge-lan tagged=bridge-lan,ether2-ap1,ether3-ap2                        vlan-ids=99 comment="Management VLAN"
```

## 4.4. Basic IP Services

Next we will configure various IP services on the router, such as DHCP and DNS.

### 4.4.1. DNS

I decided to use Cloudflare and Google DNS servers for my home network.

```
/ip dns set allow-remote-requests=yes servers=1.1.1.1,8.8.8.8
```

 Customization

- You can use any DNS servers you prefer, such as your ISP's DNS servers or a local DNS server.


### 4.4.2. IP Addresses

Add IP addresses to the VLAN interfaces we created earlier.

 IP capacity planning is a complex topic which I will not provide guidance on here.


For my home network I decided to use a `/24` subnet for each VLAN, which provides sufficient capacity for my needs. I also decided to use `10.<VLAN_ID>.0.0/24` as the network for each VLAN, which makes it easy to identify the VLAN based on the IP address.

```
/ip address
add address=10.10.0.1/24 comment="Owner VLAN addresses"      interface=vlan10-owner      network=10.10.0.0
add address=10.20.0.1/24 comment="IoT VLAN addresses"        interface=vlan20-iot        network=10.20.0.0
add address=10.30.0.1/24 comment="Guest VLAN addresses"      interface=vlan30-guest      network=10.30.0.0
add address=10.99.0.1/24 comment="Management VLAN addresses" interface=vlan99-management network=10.99.0.0
```

 Customization

- Adjust the IP addresses and networks to suit your needs.


### 4.4.3. DHCP Server

Each VLAN will have its own DHCP server to assign IP addresses to devices on the network.

I decided to only use `10.<VLAN_ID>.0.100-10.10.0.199` as the DHCP range for each VLAN, which leaves room for static IP addresses in the `10.<VLAN_ID>.0.2-10.<VLAN_ID>.0.99` and `10.<VLAN_ID>.0.200 10.<VLAN_ID>.0.254` ranges, which suits my needs.

Create the IP pools for each DHCP server to use.

```
/ip pool
add name=pool-owner ranges=10.10.0.100-10.10.0.199
add name=pool-iot ranges=10.20.0.100-10.20.0.199
add name=pool-guest ranges=10.30.0.100-10.30.0.199
add name=pool-management ranges=10.99.0.100-10.99.0.199
```

 Customization

- Adjust the IP pools to suit your needs, but I recommend reserving a range for static IP addresses.


Create each DHCP server and add a DHCP network for each.

```
/ip dhcp-server
add address-pool=pool-owner      interface=vlan10-owner      lease-time=1d name=dhcp-owner
add address-pool=pool-iot        interface=vlan20-iot        lease-time=1d name=dhcp-iot
add address-pool=pool-guest      interface=vlan30-guest      lease-time=1h name=dhcp-guest
add address-pool=pool-management interface=vlan99-management lease-time=1d name=dhcp-management
network add address=10.10.0.0/24 dns-server=10.10.0.1 gateway=10.10.0.1 comment="Owner DHCP Network"
network add address=10.20.0.0/24 dns-server=10.20.0.1 gateway=10.20.0.1 comment="IoT DHCP Network"
network add address=10.30.0.0/24 dns-server=10.30.0.1 gateway=10.30.0.1 comment="Guest DHCP Network"
network add address=10.99.0.0/24 dns-server=10.99.0.1 gateway=10.99.0.1 comment="Management DHCP Network"
```

 Customization

- Adjust the lease times to suit your needs. Typically guest devices will have shorter lease times, while permanent devices will have longer lease times.


### 4.5 PPPoE Client

Now we will configure the PPPoE client to connect to the internet.

```
/interface pppoe-client
add add-default-route=yes allow=pap,chap comment="ISP PPPoE" interface=ether1-wan max-mru=1480 max-mtu=1480 mrru=1600 name=pppoe-out user=your_user@isp.com password=your_password
```

 Customization

- The `allow` ,`max-mru` ,`max-mtu` and`mrru` values are what my ISP requires, adjust these values to suit your ISP's requirements.
- Specify your ISP's PPPoE username and password in the `user` and`password` fields.
- If you do not use a PPPoE connection, you can skip this step and configure your internet connection accordingly.


## 4.6. Wi-Fi

Now we will configure the Wi-Fi, including the special configuration considerations for the cAP ac APs.

The cAP ac APs do not support dynamic VLAN assignment based on datapath when using the`wifi-qcom-ac` package.

 If you are using cAP ax APs, please see this official example configuration and adapt the steps below accordingly.


### 4.6.1. Datapath

Create a single generic datapath.

```
/interface wifi datapath add bridge=bridge-lan disabled=no name=dp-ac-generic comment="Generic datapath for cAP ac APs"
```

### 4.6.2. Band steering

I decided to enable band steering on the cAP ac APs to encourage dual-band capable devices to connect to the 5GHz band, which has less interference and higher speeds.

```
/interface wifi steering add disabled=no name=steering-main rrm=yes wnm=yes
```

 Customization

- Band steering is optional, do your own research to determine if it's suitable for your environment.


### 4.6.3. Security Profiles

Create security profiles for each SSID we will create. These profiles are provisioned to the access points using CAPsMAN.

```
/interface wifi security
add authentication-types=wpa2-psk,wpa3-psk disabled=no ft=yes name=sec-owner comment="Owner WiFi security"
add authentication-types=wpa2-psk,wpa3-psk disabled=no ft=no  name=sec-iot   comment="IoT WiFi security"
add authentication-types=wpa2-psk,wpa3-psk disabled=no ft=yes name=sec-guest comment="Guest WiFi security"
```

 Customization

- I decided to use WPA2/WPA3 PSK for all SSIDs. You can use WPA3 only if you prefer, but this will prevent older devices from connecting.
- Fast Transition (FT) is optional. I enabled FT for the owner and guest SSIDs, while I disabled it for the IoT SSID since these devices are not expected to roam between access points.


### 4.6.4. Configuration Profiles

Create configuration profiles for each SSID we want to create. These profiles are provisioned to the access points using CAPsMAN.

```
/interface wifi configuration
add country="South Africa" datapath=dp-ac-generic disabled=no name=config-owner-wifi security=sec-owner ssid=OwnerWifi steering=steering-main comment="Config for Owner WiFi SSID"
add country="South Africa" datapath=dp-ac-generic disabled=no name=config-iot-wifi   security=sec-iot   ssid=IoTWifi                          comment="Config for IoT WiFi SSID"
add country="South Africa" datapath=dp-ac-generic disabled=no name=config-guest-wifi security=sec-guest ssid=GuestWifi steering=steering-main comment="Config for Guest WiFi SSID"
```

 Customization

- Adjust the `country` field to your country name.
- Adjust the `ssid` field to your desired SSID names.
- The
 `steering` field is optional, I enabled it for the owner and guest SSIDs to encourage dual-band capable devices to connect to the 5GHz band.


### 4.6.5. CAPsMAN

Enable and configure CAPsMAN to manage the access points.

 The `action` is not set to `create-dynamic-enabled` (instead we use `create-enabled`) since we are somewhat manually managing the CAPs due to the "wifi-qcom-ac" package not supporting automatic VLAN provisioning.


```
/interface wifi
capsman set enabled=yes ca-certificate=auto certificate=auto interfaces=vlan99-management
provisioning add action=create-enabled disabled=no master-configuration=config-owner-wifi slave-configurations=config-iot-wifi,config-guest-wifi comment="Provision config to all APs"
```

## 4.7. Security

 Security is a complex topic and this section will not cover all aspects of securing your home network. See the Securing your router and Firewall documentation for more information.


### 4.7.1. Interface lists

Create interface lists to group interfaces based on their purpose. This will make it easier to manage firewall rules and other configurations.

```
/interface list
add name=management comment="Management interface list"
member add list=management interface=ether8-management comment="Dedicated management port"
member add list=management interface=vlan99-management comment="Management VLAN"
add name=vlan comment="All VLANs excluding management VLAN"
member add list=vlan interface=vlan10-owner    comment="Owner VLAN"
member add list=vlan interface=vlan20-iot      comment="IoT VLAN"
member add list=vlan interface=vlan30-guest    comment="Guest VLAN"
add name=wan comment="WAN interface list"
member add list=wan interface=pppoe-out comment="PPPoE WAN interface"
```

### 4.7.2. Address lists

I wanted my laptop and phone to be able to access the router via HTTP, and I assigned static IP addresses to these devices. I thus have an address list for them which is used in the firewall rules.

```
/ip firewall address-list
add address=10.10.0.2 list=owner-webfig comment="Owner laptop"
add address=10.10.0.3 list=owner-webfig comment="Owner phone"
```

### 4.7.3. Disable unused services and tools

Disable any unused services and tools to reduce the attack surface of the router.

```
/tool 
# Disable the bandwidth test server
bandwidth-server set enabled=no
# Restrict Layer 2 (MAC) access to the management interface list
mac-server mac-winbox set allowed-interface-list=management
mac-server set allowed-interface-list=management
/ip
# Only allow neighbor discovery from the management interface list
neighbor discovery-settings set discover-interface-list=management
# Disable unused services
service disable telnet,ftp,api,api-ssl
# Disable DynDNS
cloud set ddns-enabled=auto update-time=no
# Use stronger encryption for SSH
ssh set strong-crypto=yes
```

 Customization

- The configuration above is opinionated and may not suit your needs, so please review it carefully.
- I decided to only allow Layer 2 (MAC) access to the management interface list. If you want to allow Layer 2 access via WinBox to other interfaces, you can adjust the `allowed-interface-list` accordingly.


### 4.7.4. Firewall

The firewall configuration below is based on various sources and research, and is by no means comprehensive or a definitive reference for securing your home network. Again, please consult the official Mikrotik documentation.

```
/ip firewall
# Configure NAT for internet access
nat add chain=srcnat out-interface=pppoe-out action=masquerade comment="NAT for internet access"
/ip firewall filter
# Traffic into the router
add chain=input   action=accept connection-state=established,related,untracked                                       comment="Allow established connections"
add chain=input   action=drop   connection-state=invalid                                                             comment="Drop invalid packets"
add chain=input   action=accept protocol=icmp                                                                        comment="Allow ICMP"
add chain=input   action=accept dst-address=127.0.0.1                                                                comment="Allow local loopback for CAPsMAN"
add chain=input   action=accept in-interface-list=!wan dst-port=53 protocol=udp                                      comment="Allow LAN DNS queries"
add chain=input   action=accept in-interface-list=!wan dst-port=53 protocol=tcp                                      comment="Allow LAN DNS queries"
add chain=input   action=accept in-interface-list=management                                                         comment="Allow full access to the management interfaces"
add chain=input   action=accept in-interface=vlan10-owner src-address-list=owner-webfig dst-port=80,443 protocol=tcp comment="Allow restricted webfig access to the router"
add chain=input   action=drop                                                                                        comment="Drop all other inputs"
# Traffic forwarded through the router
# Basics
add chain=forward action=fasttrack-connection connection-state=established,related hw-offload=yes comment="Fasttrack established connections"
add chain=forward action=accept               connection-state=established,related,untracked      comment="Allow established connections"
add chain=forward action=drop                 connection-state=invalid                            comment="Drop invalid packets"
# Inter-VLAN communication
add chain=forward action=accept connection-state=new in-interface=vlan10-owner out-interface=vlan20-iot        comment="Allow Owner -> IoT traffic"
add chain=forward action=accept connection-state=new in-interface=vlan10-owner out-interface=vlan99-management comment="Allow Owner -> Management traffic"
# Internet access
add chain=forward action=accept connection-state=new in-interface-list=management out-interface-list=wan comment="Allow internet access for management VLAN"
add chain=forward action=accept connection-state=new in-interface-list=vlan       out-interface-list=wan comment="Allow internet access for all other VLANs"
# Port forwarding
add chain=forward action=accept connection-nat-state=dstnat comment=“Allow port forwarding”
# Drop all other forwarded traffic
add chain=forward action=drop comment="Drop all other forwarded traffic"
```

 Customization

- The firewall configuration above is opinionated and may not suit your needs, so please review it carefully and adjust it to suit your needs.
- You may for example not plan to use port forwarding, in which case you can remove the "Allow port forwarding" filter.


*The next post will continue the guide at "5. Wi-Fi Access Point Configuration".*