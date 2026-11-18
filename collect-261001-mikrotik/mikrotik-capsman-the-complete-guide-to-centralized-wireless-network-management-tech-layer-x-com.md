Managing multiple wireless access points individually wastes time and creates configuration inconsistencies. MikroTik CAPsMAN (Controlled Access Point System Manager) solves this problem by providing centralized wireless management without additional licensing costs.

## 1. Understanding MikroTik CAPsMAN Architecture

### Core Components of CAPsMAN

CAPsMAN consists of two primary components:

- **CAPsMAN Controller (Manager):** The central router that stores wireless configurations and manages all connected access points. Any RouterOS device with sufficient resources can function as a controller.
- **CAP (Controlled Access Point):** Wireless devices that receive their configuration from the controller. CAPs operate as lightweight access points without local wireless configuration.

### How CAP Discovery Works

CAPs locate their controller using two discovery methods:

| Discovery Method | Protocol | Use Case | Requirements | 
|---|---|---|---|
| Layer 2 (MAC) | Broadcast/Multicast | Same broadcast domain | CAP and Controller on same LAN | 
| Layer 3 (IP) | UDP port 5246/5247 | Across routed networks | IP connectivity to controller | 

### Control and Data Plane Separation

CAPsMAN separates wireless management into two planes:

- **Control Plane:** Handles CAP provisioning, configuration distribution, and management communication. Uses DTLS encryption.
- **Data Plane:** Handles client traffic. Can operate in local forwarding or manager forwarding mode.

### CAPsMAN vs. Standalone AP Management

| Aspect | CAPsMAN | Standalone APs | 
|---|---|---|
| Configuration Location | Centralized on controller | Individual on each AP | 
| New AP Deployment | Automatic provisioning | Manual configuration | 
| Policy Changes | Single point update | Update each AP individually | 
| Client Roaming | Controller-assisted | Client-driven only | 
| Monitoring | Centralized dashboard | Access each AP separately | 
| Failure Impact | Controller failure affects management | Single AP failure isolated | 

## 2. Key Benefits of CAPsMAN for Enterprise Networks

### Simplified Wireless Configuration Management

- Configure all access points from one location
- Push changes to dozens of APs simultaneously
- Maintain consistent settings across the entire network
- Reduce human error from repetitive configuration

### Automatic Provisioning of New Access Points

- New CAPs receive configuration automatically upon connection
- Zero-touch deployment reduces installation time
- Provisioning rules match CAPs to appropriate configurations
- Replacement APs inherit settings without manual intervention

### Centralized Security and Policy Enforcement

- Apply uniform security settings across all APs
- Manage RADIUS integration from controller
- Implement access lists centrally
- Audit security configurations from single interface

### Seamless Client Roaming

- Controller maintains client session information
- Layer 2 roaming between CAPs on same datapath
- Reduced client disconnection during movement
- PMK caching for faster 802.1X reauthentication

### Cost-Effective Scaling

- No licensing fees for controller functionality
- Any RouterOS device can serve as controller
- Add APs without additional software costs
- Scale from 5 to 100+ APs on same platform

## 3. Prerequisites and Planning

### Hardware Requirements

#### CAPsMAN Controller Recommendations

| Deployment Size | Number of CAPs | Recommended Controller | Minimum RAM | 
|---|---|---|---|
| Small | 1-10 | hAP ac², RB750Gr3 | 256 MB | 
| Medium | 11-30 | RB4011, CCR1009 | 512 MB | 
| Large | 31-100+ | CCR1036, CCR2004 | 1 GB+ | 

#### Compatible CAP Devices

- **cAP Series:** cAP ac, cAP XL ac (ceiling mount)
- **wAP Series:** wAP ac, wAP 60G (outdoor)
- **hAP Series:** hAP ac², hAP ac³ (multi-purpose)
- **Audience:** Audience, Audience LTE (tri-band)
- **Any RouterOS device** with wireless interface

### Software Requirements

- **RouterOS Version:** 6.22 or higher (recommend 6.49+ or 7.x)
- **Required Package:** wireless (included in default installation)
- **Version Matching:** Controller and CAP should run same RouterOS version
- **License:** No additional license required

### Network Design Considerations

#### VLAN Planning

- **Management VLAN:** Isolate CAPsMAN control traffic
- **Corporate VLAN:** Internal employee wireless access
- **Guest VLAN:** Isolated internet-only access
- **IoT VLAN:** Segmented device networks

#### IP Addressing Scheme Example

Management Network:  10.0.1.0/24   (Controller: 10.0.1.1, CAPs: DHCP)
Corporate WLAN:      10.0.10.0/24  (VLAN 10)
Guest WLAN:          10.0.20.0/24  (VLAN 20)
IoT WLAN:            10.0.30.0/24  (VLAN 30)

#### Firewall Considerations

Allow these ports for CAPsMAN communication:

- **UDP 5246:** CAPsMAN control (CAPWAP)
- **UDP 5247:** CAPsMAN data (when using manager forwarding)
- **Layer 2:** MAC-based discovery uses broadcast

### Pre-Deployment Checklist

1. Verify RouterOS version compatibility
2. Document VLAN assignments and IP scheme
3. Perform wireless site survey
4. Calculate AP density requirements
5. Plan channel allocation (avoid overlap)
6. Document SSID and security requirements
7. Identify RADIUS server details (if using 802.1X)
8. Establish naming convention for CAPs

## 4. Step-by-Step CAPsMAN Controller Configuration

### Step 1: Enable CAPsMAN Manager

Enable the CAPsMAN controller functionality on your router:

#### CLI Configuration

/caps-man manager
set enabled=yes

#### Winbox Configuration

1. Navigate to CAPsMAN → Manager
2. Check “Enabled”
3. Click OK

### Step 2: Create CAPsMAN Channels

Define radio frequency settings for your wireless networks.

#### 2.4GHz Channel Configuration

```
/caps-man channel
add name=channel-2ghz \
    frequency=2412 \
    band=2ghz-g/n \
    control-channel-width=20mhz \
    extension-channel=disabled \
    tx-power=20
```
#### 5GHz Channel Configuration

```
/caps-man channel
add name=channel-5ghz \
    frequency=5180 \
    band=5ghz-n/ac \
    control-channel-width=20mhz \
    extension-channel=eeCe \
    tx-power=23
```
#### Channel Parameters Explained

| Parameter | Description | Recommended Value | 
|---|---|---|
| frequency | Center frequency in MHz | 2412/2437/2462 (2.4G), 5180/5220/5745 (5G) | 
| band | Wireless standard | 2ghz-g/n or 5ghz-n/ac | 
| control-channel-width | Primary channel width | 20mhz (2.4G), 20mhz (5G for compatibility) | 
| extension-channel | Channel bonding direction | disabled (2.4G), eeCe for 80MHz (5G) | 
| tx-power | Transmit power in dBm | 17-20 (indoor), 23-27 (outdoor) | 

### Step 3: Configure CAPsMAN Datapaths

Datapaths define how client traffic flows through the network.

#### Local Forwarding Datapath (Recommended)

Client traffic bridges locally at the CAP. Controller handles only management.

```
/caps-man datapath
add name=datapath-corporate \
    bridge=bridge-corporate \
    local-forwarding=yes \
    client-to-client-forwarding=yes \
    vlan-mode=use-tag \
    vlan-id=10
add name=datapath-guest \
    bridge=bridge-guest \
    local-forwarding=yes \
    client-to-client-forwarding=no \
    vlan-mode=use-tag \
    vlan-id=20
```
#### Manager Forwarding Datapath

All client traffic tunnels to the controller. Use when central inspection is required.

```
/caps-man datapath
add name=datapath-centralized \
    bridge=bridge-main \
    local-forwarding=no \
    client-to-client-forwarding=yes
```
#### Datapath Parameters Explained

| Parameter | Description | 
|---|---|
| bridge | Bridge interface for client traffic on controller (manager forwarding) or reference for CAP (local forwarding) | 
| local-forwarding | yes = traffic bridges at CAP, no = traffic tunnels to controller | 
| client-to-client-forwarding | Allow wireless clients to communicate directly | 
| vlan-mode | use-tag = apply VLAN tag to traffic | 
| vlan-id | VLAN tag for this datapath | 

### Step 4: Create Security Configurations

#### WPA2-PSK Security Profile

```
/caps-man security
add name=security-corporate \
    authentication-types=wpa2-psk \
    encryption=aes-ccm \
    passphrase="YourSecurePassword123!" \
    group-key-update=1h
```
#### WPA2-Enterprise Security Profile

```
/caps-man security
add name=security-enterprise \
    authentication-types=wpa2-eap \
    encryption=aes-ccm \
    eap-methods=passthrough \
    eap-radius-accounting=yes
```
#### WPA3 Security Profile (RouterOS 7+)

```
/caps-man security
add name=security-wpa3 \
    authentication-types=wpa3-psk \
    encryption=aes-ccm \
    passphrase="YourSecurePassword123!" \
    group-key-update=1h
```
### Step 5: Create RADIUS Configuration (Optional)

Required for WPA2-Enterprise deployments:

```
/radius
add service=wireless \
    address=10.0.1.100 \
    secret="RadiusSharedSecret" \
    authentication-port=1812 \
    accounting-port=1813 \
    timeout=3s
/caps-man aaa
set interim-update=5m \
    called-format=mac \
    mac-mode=as-username-and-password
```
### Step 6: Build CAPsMAN Configurations

Configurations combine channels, datapaths, and security profiles into complete wireless settings.

#### Corporate 5GHz Configuration

```
/caps-man configuration
add name=cfg-corporate-5ghz \
    ssid="Corporate-WiFi" \
    mode=ap \
    country="united states" \
    channel=channel-5ghz \
    datapath=datapath-corporate \
    security=security-corporate \
    hw-retries=7 \
    disconnect-timeout=5s \
    max-sta-count=50
```
#### Corporate 2.4GHz Configuration

```
/caps-man configuration
add name=cfg-corporate-2ghz \
    ssid="Corporate-WiFi" \
    mode=ap \
    country="united states" \
    channel=channel-2ghz \
    datapath=datapath-corporate \
    security=security-corporate \
    hw-retries=7 \
    max-sta-count=30
```
#### Guest Network Configuration

```
/caps-man configuration
add name=cfg-guest-5ghz \
    ssid="Guest-WiFi" \
    mode=ap \
    country="united states" \
    channel=channel-5ghz \
    datapath=datapath-guest \
    security=security-guest \
    hide-ssid=no \
    max-sta-count=100
```
### Step 7: Create Provisioning Rules

Provisioning rules assign configurations to CAPs automatically.

#### Basic Provisioning (All CAPs Same Config)

```
/caps-man provisioning
add action=create-dynamic-enabled \
    master-configuration=cfg-corporate-5ghz \
    slave-configurations=cfg-corporate-2ghz \
    name-format=identity \
    comment="Default provisioning for all CAPs"
```
#### Location-Based Provisioning

```
# Floor 1 CAPs (identified by identity prefix)
/caps-man provisioning
add action=create-dynamic-enabled \
    identity-regexp="^CAP-F1-.*" \
    master-configuration=cfg-corporate-5ghz-f1 \
    slave-configurations=cfg-corporate-2ghz-f1 \
    name-format=identity \
    comment="Floor 1 CAPs"
# Floor 2 CAPs
/caps-man provisioning
add action=create-dynamic-enabled \
    identity-regexp="^CAP-F2-.*" \
    master-configuration=cfg-corporate-5ghz-f2 \
    slave-configurations=cfg-corporate-2ghz-f2 \
    name-format=identity \
    comment="Floor 2 CAPs"
```
#### Radio-Specific Provisioning

```
# 5GHz radios only
/caps-man provisioning
add action=create-dynamic-enabled \
    hw-supported-modes=ac \
    master-configuration=cfg-corporate-5ghz \
    name-format=identity \
    comment="5GHz capable radios"
# 2.4GHz radios only
/caps-man provisioning
add action=create-dynamic-enabled \
    hw-supported-modes=gn \
    master-configuration=cfg-corporate-2ghz \
    name-format=identity \
    comment="2.4GHz radios"
```
#### Provisioning Parameters Reference

| Parameter | Description | 
|---|---|
| action | create-dynamic-enabled (auto-enable), create-enabled (static), none | 
| master-configuration | Primary radio configuration (typically 5GHz) | 
| slave-configurations | Secondary radio configuration (typically 2.4GHz) | 
| identity-regexp | Match CAP by system identity | 
| radio-mac | Match CAP by specific MAC address | 
| hw-supported-modes | Match by radio capabilities (ac, gn, an) | 
| name-format | How to name created interfaces (identity, cap, prefix) | 

## 5. CAP Device Setup and Deployment

### Preparing CAP Devices

#### Step 1: Reset CAP to Default

/system reset-configuration no-defaults=yes skip-backup=yes

#### Step 2: Set System Identity

/system identity set name="CAP-F1-01"

#### Step 3: Configure Basic IP Connectivity

# Create bridge for management
/interface bridge
add name=bridge-mgmt
/interface bridge port
add bridge=bridge-mgmt interface=ether1
# Get IP via DHCP
/ip dhcp-client
add interface=bridge-mgmt disabled=no

#### Step 4: Upgrade Firmware

/system package update
set channel=stable
check-for-updates
download
# Reboot after download
/system reboot

### Configure CAP Mode

#### Layer 2 Discovery (Same Subnet)

```
/interface wireless cap
set enabled=yes \
    interfaces=wlan1,wlan2 \
    discovery-interfaces=bridge-mgmt \
    bridge=bridge-mgmt
```
#### Layer 3 Discovery (Routed Network)

```
/interface wireless cap
set enabled=yes \
    interfaces=wlan1,wlan2 \
    caps-man-addresses=10.0.1.1 \
    bridge=bridge-mgmt
```
#### CAP Configuration Parameters

| Parameter | Description | 
|---|---|
| enabled | Enable CAP functionality | 
| interfaces | Wireless interfaces to be controlled by CAPsMAN | 
| discovery-interfaces | Interfaces for Layer 2 discovery broadcasts | 
| caps-man-addresses | Controller IP addresses for Layer 3 discovery | 
| bridge | Bridge for local forwarding traffic | 
| lock-to-caps-man | Bind CAP to specific controller | 

### Certificate-Based CAP Authentication

#### Generate CA Certificate on Controller

/certificate
add name=capsman-ca common-name=capsman-ca key-usage=key-cert-sign,crl-sign
sign capsman-ca

#### Generate Controller Certificate

/certificate
add name=capsman-controller common-name=capsman-controller
sign capsman-controller ca=capsman-ca

#### Generate CAP Certificate

/certificate
add name=cap-01 common-name=cap-01
sign cap-01 ca=capsman-ca

#### Enable Certificate Requirement on Controller

```
/caps-man manager
set require-peer-certificate=yes \
    certificate=capsman-controller \
    ca-certificate=capsman-ca
```
#### Configure CAP with Certificate

# Export CA certificate from controller
/certificate export-certificate capsman-ca
# Import on CAP (after copying file)
/certificate import file-name=capsman-ca.crt
# Configure CAP with certificate
/interface wireless cap
set certificate=cap-01

### Zero-Touch Provisioning with DHCP

Configure DHCP server to provide CAPsMAN controller address:

# On DHCP Server
/ip dhcp-server option
add name=capsman-option code=138 value="'10.0.1.1'"
/ip dhcp-server network
set [find] dhcp-option=capsman-option

CAPs will automatically discover the controller using DHCP option 138.

## 6. Advanced CAPsMAN Features and Optimization

### Access List Configuration

Control which clients can connect based on MAC address or signal strength.

#### MAC-Based Access Control

```
# Allow specific devices
/caps-man access-list
add mac-address=AA:BB:CC:DD:EE:FF \
    action=accept \
    comment="Executive laptop"
# Block specific devices
add mac-address=11:22:33:44:55:66 \
    action=reject \
    comment="Blocked device"
# Default policy (allow all others)
add action=accept \
    comment="Default allow"
```
#### Signal Strength-Based Access Control

```
# Reject weak signal clients (force roaming)
/caps-man access-list
add signal-range=-75..0 \
    action=accept \
    comment="Accept clients with good signal"
add signal-range=-120..-76 \
    action=reject \
    comment="Reject weak signal clients"
```
#### Time-Based Access Control

```
/caps-man access-list
add interface=cfg-guest \
    time=8h-18h,mon,tue,wed,thu,fri \
    action=accept \
    comment="Guest access during business hours only"
add interface=cfg-guest \
    action=reject \
    comment="Deny guest access outside hours"
```
### Dynamic Channel and Power Management

#### Automatic Channel Selection

```
/caps-man channel
add name=channel-5ghz-auto \
    band=5ghz-n/ac \
    control-channel-width=20mhz \
    extension-channel=eeCe \
    reselect-interval=1h \
    skip-dfs-channels=yes
```
Note: Leave frequency empty for automatic selection.

#### Per-CAP Channel Assignment

```
/caps-man provisioning
add identity-regexp="^CAP-F1-01" \
    master-configuration=cfg-corp-ch36 \
    comment="Channel 36 for CAP-F1-01"
add identity-regexp="^CAP-F1-02" \
    master-configuration=cfg-corp-ch44 \
    comment="Channel 44 for CAP-F1-02"
```
### Load Balancing Between Access Points

#### Limit Clients Per Radio

/caps-man configuration
set cfg-corporate-5ghz max-sta-count=40
/caps-man configuration
set cfg-corporate-2ghz max-sta-count=25

#### Band Steering (2.4GHz to 5GHz)

# Lower tx-power on 2.4GHz to encourage 5GHz
/caps-man channel
set channel-2ghz tx-power=14
# Higher tx-power on 5GHz
/caps-man channel
set channel-5ghz tx-power=20

### Multiple SSID Configuration

#### Corporate + Guest + IoT Setup

```
# Datapaths with different VLANs
/caps-man datapath
add name=dp-corporate vlan-id=10 vlan-mode=use-tag local-forwarding=yes
add name=dp-guest vlan-id=20 vlan-mode=use-tag local-forwarding=yes client-to-client-forwarding=no
add name=dp-iot vlan-id=30 vlan-mode=use-tag local-forwarding=yes
# Configurations for each SSID
/caps-man configuration
add name=cfg-corporate ssid="CORP-SECURE" datapath=dp-corporate security=sec-wpa2-ent
add name=cfg-guest ssid="GUEST" datapath=dp-guest security=sec-wpa2-psk
add name=cfg-iot ssid="IoT-Devices" datapath=dp-iot security=sec-iot hide-ssid=yes
# Provisioning with all SSIDs
/caps-man provisioning
add master-configuration=cfg-corporate \
    slave-configurations=cfg-guest,cfg-iot \
    action=create-dynamic-enabled
```
### Hotspot Integration

```
# Create datapath for hotspot
/caps-man datapath
add name=dp-hotspot \
    local-forwarding=no \
    bridge=bridge-hotspot
# Configure hotspot on controller
/ip hotspot
add name=hotspot-guest \
    interface=bridge-hotspot \
    address-pool=hotspot-pool \
    profile=hsprof-guest
# Guest configuration with hotspot datapath
/caps-man configuration
add name=cfg-hotspot \
    ssid="Free-WiFi" \
    datapath=dp-hotspot \
    security.authentication-types=""
```
## 7. Monitoring and Troubleshooting

### Monitoring Tools and Commands

#### View Registered CAPs

/caps-man remote-cap print

Output shows:

- CAP identity and MAC address
- State (running, disabled)
- Version
- Radio details

#### View Active Interfaces

/caps-man interface print

#### View Connected Clients

/caps-man registration-table print

Output shows:

- Client MAC address
- Interface name
- Signal strength (rx/tx)
- Data rates
- Uptime
- Packets/bytes transferred

#### Monitor Real-Time Client Activity

/caps-man registration-table print interval=1

#### View Client Statistics

/caps-man registration-table print stats

### SNMP Monitoring

#### Enable SNMP on Controller

/snmp
set enabled=yes contact="admin@company.com" location="DataCenter"
/snmp community
set public addresses=10.0.1.0/24 read-access=yes

#### Key SNMP OIDs for CAPsMAN

| OID | Description | 
|---|---|
| .1.3.6.1.4.1.14988.1.1.1.2 | Wireless registration table | 
| .1.3.6.1.4.1.14988.1.1.1.3 | Wireless interface statistics | 
| .1.3.6.1.2.1.2.2 | Interface table (standard MIB-II) | 

### Common Issues and Solutions

#### Issue: CAP Not Connecting to Controller

Troubleshooting steps:

1. Verify network connectivity:
# On CAP /ping 10.0.1.1
2. Check CAP configuration:
/interface wireless cap print
3. Verify CAPsMAN is enabled:
# On Controller /caps-man manager print
4. Check firewall rules:
# Allow CAPsMAN traffic /ip firewall filter add chain=input protocol=udp dst-port=5246-5247 action=accept
5. Check RouterOS version match

#### Issue: Clients Cannot Connect

Troubleshooting steps:

1. Verify interface is running:
/caps-man interface print where disabled=no
2. Check security configuration:
/caps-man security print
3. Verify RADIUS connectivity (if using):
/radius print /radius monitor 0
4. Check access lists:
/caps-man access-list print

#### Issue: Poor Roaming Performance

Solutions:

- Enable access list with signal threshold:
/caps-man access-list add signal-range=-70..0 action=accept add signal-range=-120..-71 action=reject
- Use same datapath for all APs
- Enable local forwarding to reduce latency
- Verify overlapping coverage between APs

#### Issue: Intermittent Disconnections

Solutions:

- Check for channel interference
- Increase disconnect timeout:
/caps-man configuration set cfg-corporate disconnect-timeout=10s
- Verify CAP-to-controller link stability
- Check for power issues on CAPs

### Logging and Debugging

#### Enable CAPsMAN Debug Logging

/system logging
add topics=caps,debug action=memory
add topics=wireless,debug action=memory

#### View Logs

/log print where topics~"caps"

#### Enable Remote Logging

/system logging action
add name=remote-syslog target=remote remote=10.0.1.200
/system logging
add topics=caps action=remote-syslog

## 8. Security Best Practices

### Securing the Management Plane

#### Restrict CAPsMAN Access

/ip firewall filter
# Allow CAPsMAN from management network only
add chain=input src-address=10.0.1.0/24 protocol=udp dst-port=5246-5247 action=accept comment="CAPsMAN from mgmt"
add chain=input protocol=udp dst-port=5246-5247 action=drop comment="Block other CAPsMAN"

#### Management VLAN Isolation

# Create management VLAN
/interface vlan
add name=vlan-mgmt vlan-id=99 interface=bridge
# Place CAPs on management VLAN
/interface bridge vlan
add bridge=bridge tagged=bridge,ether1,ether2 vlan-ids=99

#### Disable Unnecessary Services on CAPs

/ip service
set telnet disabled=yes
set ftp disabled=yes
set www disabled=yes
set api disabled=yes
set api-ssl disabled=yes
set winbox address=10.0.1.0/24
set ssh address=10.0.1.0/24

### Wireless Security Recommendations

#### Use WPA3 Where Possible (RouterOS 7+)

```
/caps-man security
add name=sec-wpa3 \
    authentication-types=wpa3-psk \
    encryption=aes-ccm \
    passphrase="ComplexPassword123!"
```
#### Enable Protected Management Frames

/caps-man configuration
set cfg-corporate security.pmf=required

#### Implement 802.1X with RADIUS

```
# RADIUS server configuration
/radius
add address=10.0.1.100 secret="RadiusSecret123" service=wireless
# Security profile for EAP
/caps-man security
add name=sec-enterprise \
    authentication-types=wpa2-eap \
    encryption=aes-ccm \
    eap-methods=passthrough
# AAA settings
/caps-man aaa
set interim-update=5m \
    called-format=mac:ssid \
    mac-mode=as-username
```
#### Disable Legacy Protocols

# Use only modern standards
/caps-man channel
set channel-5ghz band=5ghz-onlyac
/caps-man configuration
set cfg-corporate supported-rates-b="" basic-rates-b=""

### Security Audit Checklist

1. Verify WPA2/WPA3 encryption enabled
2. Confirm PMF is enabled (required or preferred)
3. Validate RADIUS server connectivity
4. Review access list entries
5. Check firewall rules for CAPsMAN traffic
6. Verify certificate-based CAP authentication
7. Audit guest network isolation
8. Review password complexity for PSK networks
9. Confirm management access restrictions
10. Validate VLAN assignments

### Regular Maintenance Tasks

- **Weekly:** Review logs for anomalies
- **Monthly:** Backup configurations
- **Quarterly:** Review and rotate passwords
- **Semi-annually:** Update firmware
- **Annually:** Full security audit

#### Backup Configuration Script

```
/system scheduler
add name=weekly-backup interval=7d on-event={
    /system backup save name=([/system identity get name] . "-" . [:pick [/system clock get date] 0 10])
    /export file=([/system identity get name] . "-config-" . [:pick [/system clock get date] 0 10])
}
```
## 9. High Availability and Redundancy

### Dual CAPsMAN Controller Setup

CAPs can be configured with primary and backup controller addresses.

#### Configure CAP with Multiple Controllers

```
/interface wireless cap
set enabled=yes \
    interfaces=wlan1,wlan2 \
    caps-man-addresses=10.0.1.1,10.0.1.2 \
    bridge=bridge-mgmt
```
#### Controller Failover Behavior

- CAP attempts connection to first address
- If primary fails, CAP connects to secondary
- CAP periodically checks primary availability
- Manual intervention required to sync configurations between controllers

#### Configuration Synchronization

MikroTik does not provide automatic configuration sync between controllers. Options:

1. **Manual Sync:** Export/import configurations periodically
2. **Scripted Sync:** Create script to copy configuration elements
3. **Netinstall:** Restore identical configuration to both

#### Export Configuration for Sync

# On Primary Controller
/caps-man channel export file=capsman-channels
/caps-man datapath export file=capsman-datapaths
/caps-man security export file=capsman-security
/caps-man configuration export file=capsman-configs
/caps-man provisioning export file=capsman-provisioning

### Network Redundancy Considerations

#### VRRP for Gateway Redundancy

# On Primary Router
/interface vrrp
add name=vrrp-mgmt interface=vlan-mgmt vrid=10 priority=200
/ip address
add address=10.0.1.1/24 interface=vrrp-mgmt
# On Secondary Router
/interface vrrp
add name=vrrp-mgmt interface=vlan-mgmt vrid=10 priority=100
/ip address
add address=10.0.1.1/24 interface=vrrp-mgmt

#### Controller Failure Impact Analysis

| Forwarding Mode | Controller Failure Impact | 
|---|---|
| Local Forwarding | Existing clients continue working. No new clients can connect. No roaming. | 
| Manager Forwarding | All traffic stops. Complete network outage for wireless clients. | 

**Recommendation:** Use local forwarding for production networks to minimize controller failure impact.

### Backup and Recovery Procedures

#### Complete Configuration Backup

# Binary backup (includes passwords)
/system backup save name=capsman-controller-full
# Text export (readable, excludes passwords)
/export file=capsman-controller-export

#### Restore Configuration

# Binary restore
/system backup load name=capsman-controller-full
# Text import
/import file=capsman-controller-export.rsc

#### Documentation Requirements

Maintain documentation for:

- Controller IP addresses and credentials
- VLAN assignments and IP schemes
- RADIUS server details
- Certificate information
- Provisioning rule logic
- Channel planning chart
- CAP inventory with locations

## 10. Real-World Deployment Scenarios

### Scenario 1: Small Office (5-10 APs)

#### Network Topology

```
Internet
    |
[ISP Router]
    |
[MikroTik RB4011 - CAPsMAN Controller]
    |
[Managed Switch]
    |
+---+---+---+---+
|   |   |   |   |
CAP CAP CAP CAP CAP
```
#### Key Configuration

- **Controller:** RB4011 (512MB RAM)
- **CAPs:** 5x cAP ac
- **SSIDs:** Corporate (WPA2-PSK), Guest (isolated VLAN)
- **Forwarding:** Local forwarding

#### Configuration Summary

# Complete Small Office Setup
# Channels
/caps-man channel
add name=ch-5ghz band=5ghz-n/ac control-channel-width=20mhz extension-channel=eeCe
add name=ch-2ghz band=2ghz-g/n control-channel-width=20mhz
# Datapaths
/caps-man datapath
add name=dp-corp bridge=bridge local-forwarding=yes vlan-id=10 vlan-mode=use-tag
add name=dp-guest bridge=bridge local-forwarding=yes vlan-id=20 vlan-mode=use-tag client-to-client-forwarding=no
# Security
/caps-man security
add name=sec-corp authentication-types=wpa2-psk encryption=aes-ccm passphrase="CorpSecure2024!"
add name=sec-guest authentication-types=wpa2-psk encryption=aes-ccm passphrase="GuestAccess2024"
# Configurations
/caps-man configuration
add name=cfg-corp-5g ssid="CorpNet" channel=ch-5ghz datapath=dp-corp security=sec-corp country="united states"
add name=cfg-corp-2g ssid="CorpNet" channel=ch-2ghz datapath=dp-corp security=sec-corp country="united states"
add name=cfg-guest-5g ssid="GuestNet" channel=ch-5ghz datapath=dp-guest security=sec-guest country="united states"
# Provisioning
/caps-man provisioning
add master-configuration=cfg-corp-5g slave-configurations=cfg-corp-2g,cfg-guest-5g action=create-dynamic-enabled
# Enable Manager
/caps-man manager set enabled=yes

### Scenario 2: Multi-Floor Enterprise (20-50 APs)

#### Network Topology

```
         [Core Switch]
              |
    +---------+---------+
    |         |         |
[Floor 1] [Floor 2] [Floor 3]
 Switch    Switch    Switch
    |         |         |
  CAPs      CAPs      CAPs
(10 APs)  (15 APs)  (10 APs)
              |
       [CCR1036 - Controller]
```
#### Key Configuration

- **Controller:** CCR1036 (4GB RAM)
- **CAPs:** 35x cAP ac/cAP XL ac
- **SSIDs:** Corporate (802.1X), Guest (Hotspot), IoT
- **VLANs:** Mgmt (99), Corp (10), Guest (20), IoT (30)
- **RADIUS:** Microsoft NPS for 802.1X

#### Channel Planning Strategy

Floor 1: Channels 36, 44, 149, 157 (5GHz)
Floor 2: Channels 40, 48, 153, 161 (5GHz)
Floor 3: Channels 36, 44, 149, 157 (5GHz)
2.4GHz: Channels 1, 6, 11 (rotate per AP)

#### Provisioning by Floor

/caps-man provisioning
add identity-regexp="^F1-.*" master-configuration=cfg-corp-f1-5g slave-configurations=cfg-corp-f1-2g,cfg-guest-5g,cfg-iot action=create-dynamic-enabled
add identity-regexp="^F2-.*" master-configuration=cfg-corp-f2-5g slave-configurations=cfg-corp-f2-2g,cfg-guest-5g,cfg-iot action=create-dynamic-enabled
add identity-regexp="^F3-.*" master-configuration=cfg-corp-f3-5g slave-configurations=cfg-corp-f3-2g,cfg-guest-5g,cfg-iot action=create-dynamic-enabled

### Scenario 3: Multi-Site with Centralized Controller

#### Network Topology

```
[Headquarters]              [Branch 1]              [Branch 2]
     |                          |                       |
[Controller]----[VPN/MPLS]----[Router]----[VPN/MPLS]----[Router]
     |                          |                       |
   CAPs                       CAPs                    CAPs
(20 APs)                    (5 APs)                 (5 APs)
```
#### Key Configuration

- **WAN Consideration:** Use Layer 3 discovery over VPN
- **Forwarding:** Local forwarding mandatory (avoids WAN traffic for data)
- **Bandwidth:** Management traffic minimal (~1 Kbps per CAP)

#### Branch CAP Configuration

```
# Branch CAP settings
/interface wireless cap
set enabled=yes \
    interfaces=wlan1,wlan2 \
    caps-man-addresses=10.100.1.1 \
    bridge=bridge-local
# Ensure local bridge exists for forwarding
/interface bridge
add name=bridge-local
/interface bridge port
add bridge=bridge-local interface=ether1
```
#### Site-Specific Configurations

# Create site-specific datapaths
/caps-man datapath
add name=dp-hq-corp bridge=bridge local-forwarding=yes vlan-id=10
add name=dp-branch1-corp bridge=bridge local-forwarding=yes vlan-id=10
add name=dp-branch2-corp bridge=bridge local-forwarding=yes vlan-id=10
# Provisioning by site identity
/caps-man provisioning
add identity-regexp="^HQ-.*" master-configuration=cfg-hq-5g action=create-dynamic-enabled
add identity-regexp="^BR1-.*" master-configuration=cfg-branch1-5g action=create-dynamic-enabled
add identity-regexp="^BR2-.*" master-configuration=cfg-branch2-5g action=create-dynamic-enabled

### Scenario 4: Hotel/Hospitality Environment

#### Requirements

- High client density per AP
- Guest captive portal with terms acceptance
- Bandwidth limits per client
- Client isolation for security

#### Key Configuration

```
# High-density channel configuration
/caps-man channel
add name=ch-5ghz-dense \
    band=5ghz-n/ac \
    control-channel-width=20mhz \
    extension-channel=Ce \
    tx-power=17
# Datapath with client isolation
/caps-man datapath
add name=dp-guest \
    local-forwarding=no \
    bridge=bridge-hotspot \
    client-to-client-forwarding=no
# Configuration with rate limiting
/caps-man configuration
add name=cfg-guest \
    ssid="Hotel-WiFi" \
    channel=ch-5ghz-dense \
    datapath=dp-guest \
    security.authentication-types="" \
    rate.rx-rate=10M \
    rate.tx-rate=5M \
    max-sta-count=100
# Hotspot setup on controller
/ip pool
add name=hotspot-pool ranges=10.0.20.10-10.0.20.250
/ip dhcp-server
add name=dhcp-hotspot interface=bridge-hotspot address-pool=hotspot-pool
/ip hotspot
add name=hs-guest interface=bridge-hotspot address-pool=hotspot-pool
/ip hotspot profile
set default login-by=http-chap,https \
    html-directory=flash/hotspot \
    rate-limit="5M/10M"
```
## 11. CAPsMAN vs. Alternative Solutions

### Feature Comparison Table

| Feature | MikroTik CAPsMAN | Ubiquiti UniFi | Cisco WLC | Aruba Central | 
|---|---|---|---|---|
| License Cost | Free | Free | Per-AP licensing | Subscription | 
| Controller Hardware | Any RouterOS device | Dedicated/Cloud | Dedicated appliance | Cloud-only | 
| Max APs (typical) | 100-150 | 50-2000 (by model) | 500-6000 | 10000+ | 
| Configuration Complexity | Medium-High | Low | High | Medium | 
| CLI Access | Full | Limited | Full | Limited | 
| RADIUS Support | Yes | Yes | Yes | Yes | 
| WPA3 | Yes (ROS7) | Yes | Yes | Yes | 
| Seamless Roaming | Layer 2 | Layer 2/3 | Layer 2/3 | Layer 2/3 | 
| API/Automation | Yes (API) | Yes (API) | Yes (API) | Yes (API) | 
| WiFi 6/6E Support | Limited | Yes | Yes | Yes | 

### When to Choose CAPsMAN

CAPsMAN is the right choice when:

- Budget is a primary concern
- Existing MikroTik infrastructure is in place
- Team has MikroTik expertise
- Full CLI control is required
- Deployment size is under 100 APs
- Integration with MikroTik routing features is needed

### CAPsMAN Limitations

Consider alternatives when:

- Deployments exceed 100-150 APs
- Advanced RF optimization is required
- Dedicated support contracts are mandatory
- WiFi 6E is a requirement (limited MikroTik support currently)
- GUI simplicity is a priority for operations team
- AI/ML-based wireless optimization is desired

### Cost Comparison (50 AP Deployment)

| Solution | Controller Cost | AP Cost (×50) | Annual License | 5-Year TCO | 
|---|---|---|---|---|
| MikroTik CAPsMAN | $300 (RB4011) | $3,500 ($70/AP) | $0 | ~$3,800 | 
| Ubiquiti UniFi | $200 (Cloud Key) | $7,500 ($150/AP) | $0 | ~$7,700 | 
| Cisco (Meraki) | Cloud-hosted | $15,000 ($300/AP) | $7,500 | ~$52,500 | 
| Aruba Central | Cloud-hosted | $12,500 ($250/AP) | $5,000 | ~$37,500 | 

*Note: Prices are approximate and vary by region and reseller.*

## 12. Future of MikroTik CAPsMAN

### RouterOS 7 CAPsMAN Improvements

RouterOS 7 introduces several enhancements:

- **WPA3 Support:** Full WPA3-Personal and WPA3-Enterprise
- **Improved 802.11ax:** WiFi 6 support on compatible hardware
- **Enhanced PMF:** Better protected management frames implementation
- **Performance Improvements:** Optimized control plane processing
- **New Wireless Package:** Rewritten wireless driver architecture

### WiFi 6 and WiFi 6E Considerations

Current MikroTik WiFi 6 capable devices:

- Audience (WiFi 6 tri-band)
- hAP ax² (WiFi 6)
- hAP ax³ (WiFi 6)

WiFi 6E (6GHz) support: Limited availability. Check MikroTik announcements for updates.

### Recommended RouterOS Version

| Deployment Type | Recommended Version | 
|---|---|
| Production (stability focus) | RouterOS 6.49.x (long-term) | 
| New deployments | RouterOS 7.x stable | 
| WiFi 6 required | RouterOS 7.x stable | 
| WPA3 required | RouterOS 7.x stable | 

### Community Resources

- **MikroTik Forum:** forum.mikrotik.com
- **MikroTik Wiki:** wiki.mikrotik.com
- **Reddit r/mikrotik:** Community discussions and troubleshooting
- **MikroTik Training:** Official MTCNA, MTCWE courses

## 13. Conclusion

MikroTik CAPsMAN provides enterprise-grade centralized wireless management without licensing costs. The platform suits organizations with existing MikroTik infrastructure and teams comfortable with RouterOS configuration.

### Key Takeaways

- CAPsMAN eliminates repetitive AP configuration through centralized management
- Local forwarding mode minimizes controller dependency for data traffic
- Provisioning rules enable zero-touch AP deployment
- Certificate-based authentication secures CAP-to-controller communication
- Access lists and signal thresholds improve roaming behavior
- Regular backups and documentation are essential for disaster recovery

### Recommended Next Steps

1. Build a lab environment with one controller and 2-3 CAPs
2. Test basic provisioning with single SSID
3. Add VLANs and multiple SSIDs
4. Implement RADIUS authentication
5. Practice troubleshooting common issues
6. Document your production configuration
7. Deploy in phases, starting with non-critical areas

### Quick Reference Commands

# Enable CAPsMAN
/caps-man manager set enabled=yes
# View connected CAPs
/caps-man remote-cap print
# View connected clients
/caps-man registration-table print
# View provisioned interfaces
/caps-man interface print
# Enable CAP mode
/interface wireless cap set enabled=yes interfaces=wlan1,wlan2
# Debug logging
/system logging add topics=caps,wireless action=memory

## Additional Resources

### Official Documentation

### Related Topics

Check our list of MikroTik guides