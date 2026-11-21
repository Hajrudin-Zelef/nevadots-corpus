---
id: collect-261121-veille-ia/fortinet/sheynshield-fortinet-fortigate-cheatsheet-foundation-md-at-172fb9490610bedf2470836990d0ff5347fa74a0--1
title: "sheynshield/Fortinet/FortiGate/CheatSheet/Foundation.md at 172fb9490610bedf2470836990d0ff5347fa74a0 · Shayan-heydarikhah/sheynshield"
domain: fortinet
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["security", "fortigate", "processing", "traffic", "sheynshield", "fortinet", "port", "application"]
source: https://github.com/shayan-heydarikhah/sheynshield/blob/HEAD/Fortinet/FortiGate/CheatSheet/Foundation.md
source_anchor: ""
source_lines: [1, 1500]
section: "Copilot_Microsoft"
---
**SheynShield | Engineering Secure Networks**

**FortiGate • FortiOS • NGFW • UTM • Hardware Architecture • Management • Security • Networking • Troubleshooting • NSE 4 • NSE 7**


This is designed as a **Fortinet technical foundation and engineering reference**, with emphasis on the knowledge required to understand FortiGate at both **NSE 4 professional** and **NSE 7 advanced troubleshooting / solution** levels.

It is intentionally structured around four questions:

```
How does FortiGate work?
        ↓
How does FortiGate secure traffic?
        ↓
How does FortiGate process traffic?
        ↓
How do we troubleshoot and design it?
```
Traditional firewalls primarily make forwarding decisions using network and transport information:

```
Source IP
Destination IP
Protocol
Port
Session
```
Example:

```
Source      = 10.10.10.10
Destination = 8.8.8.8
Protocol    = TCP
Port        = 443
Action      = ACCEPT
```
The firewall knows that the traffic is using TCP port 443.

However:

```
TCP/443
   ≠
Guaranteed Application Identity
```
Port numbers alone do not provide complete application visibility.

UTM combines multiple security functions into a single security platform.

Typical capabilities include:

```
Firewall
+
Antivirus
+
IPS
+
Web Filtering
+
Application Control
+
Anti-Spam
+
VPN
```
```
Multiple Security Technologies
             ↓
      One Security Platform
```
The major advantage is integration between multiple security functions.

NGFW extends traditional firewall capabilities with deeper inspection and contextual security.

Typical capabilities:

```
Firewall
+
Application Control
+
IPS
+
Antivirus
+
Web Filtering
+
SSL/TLS Inspection
+
Identity Awareness
+
Threat Intelligence
+
Security Analytics
```
```
IP + Port
```
```
IP
+
Port
+
Application
+
User
+
Content
+
Threat
+
Context
```
IDS primarily detects suspicious activity.

```
Traffic
   ↓
Detection
   ↓
Alert
   ↓
Log
```
```
Detect
+
Alert
```
IPS adds active prevention.

```
Traffic
   ↓
Detection
   ↓
Analysis
   ↓
Decision
   ↓
Block / Drop / Reset
```
```
Detect
+
Prevent
```
```
IDS = Detect
IPS = Detect + Prevent
```
A simplified FortiGate security flow:

```
Incoming Traffic
       ↓
Interface
       ↓
Routing
       ↓
Firewall Policy
       ↓
Security Inspection
       ↓
Enforcement
       ↓
Logging
       ↓
Forwarding
```
Security inspection may involve:

```
Antivirus
IPS
Application Control
Web Filtering
DNS Filtering
SSL/TLS Inspection
File Filtering
DLP
Botnet Detection
Threat Intelligence
```
A simplified FortiGate architecture:

```
                         FortiGate
                            │
              ┌─────────────┴─────────────┐
              │                           │
             CPU                         SPU
                                          │
                              ┌───────────┴───────────┐
                              │                       │
                             NP                      CP
```
Where:

```
CPU
↓
General-Purpose Processing
NP
↓
Network Processing / Acceleration
CP
↓
Content / Security Processing Acceleration
SPU
↓
Fortinet Security Processing Architecture
```
Actual hardware architecture varies between FortiGate models and processor generations.

General-purpose processor.

Typical responsibilities may include:

- Management
- Control-plane processing
- Routing/control functions
- Processes that cannot be offloaded
- System services

**Security Processing Unit**

SPU is Fortinet's broader specialized hardware-processing architecture.

It can include:

```
SPU
 ├── NP
 └── CP
```
Optimized for network and packet processing.

Think:

```
Traffic
   ↓
NP
   ↓
High-Speed Network Processing
```
Potential acceleration areas depend on the platform and generation.

Designed to accelerate content/security-intensive operations.

Conceptually:

```
Traffic
   ↓
CP
   ↓
Security / Content Processing
```
Never assume:

```
NP = Everything Network Related
CP = Everything Security Related
```
Actual offloading depends on:

```
FortiGate Model
+
Processor Generation
+
FortiOS Version
+
Feature Configuration
+
Traffic Characteristics
+
Inspection Mode
```
FortiGate platforms are available across multiple performance classes.

A simplified historical/product-family model:

Examples include:

```
1000
2000
3000
5000
7000
```
Typical environments:

- Large enterprises
- Data centers
- Service providers
- High-throughput deployments
- Large security infrastructures

Examples include:

```
100
200
300
400
500
600
700
800
900
```
Typical environments:

- Enterprise branches
- Regional offices
- Campus networks
- Medium-sized organizations

Examples include:

```
30
40
50
60
80
```
Model suffixes such as:

```
F
G
```
represent different hardware generations and configurations.

**Important:** Model numbers should never be treated as a simple linear performance ranking. Always check the specific FortiGate data.


```
                         FORTINET
                            │
          ┌─────────────────┼─────────────────┐
          │                 │                 │
       NETWORK           SECURITY         OPERATIONS
          │                 │                 │
          ▼                 ▼                 ▼
      FortiGate          FortiWeb        FortiManager
      FortiSwitch        FortiMail       FortiAnalyzer
      FortiAP            FortiNAC        FortiSIEM
      FortiWLC           FortiDDoS       FortiClient EMS
      FortiADC            FortiAuth
```
FortiGuard provides Fortinet security intelligence and subscription services.

Conceptually:

```
                     FortiGuard
                         │
                         ▼
                Threat Intelligence
                         │
          ┌──────────────┼──────────────┐
          ▼              ▼              ▼
         AV             IPS          Web/DNS
       Updates        Updates       Categorization
          │              │              │
          └──────────────┼──────────────┘
                         ▼
                     FortiGate
```
Relevant intelligence may include:

- Antivirus updates
- IPS signatures
- Web categorization
- Application information
- Botnet intelligence
- DNS/security intelligence
- Other security databases

FortiGate can be understood using two major planes:

```
                 FortiGate
                    │
        ┌───────────┴───────────┐
        │                       │
        ▼                       ▼
    Data Plane              Management Plane
        │                       │
        ▼                       ▼
 Traffic Forwarding       Administration
 Firewall Policies        GUI / CLI
 Routing                  Monitoring
 Security Inspection      Configuration
```
```
Data Plane
    ≠
Management Plane
```
Many FortiGate platforms use:

```
192.168.1.99/24
```
as a factory-default management address.

Typical initial credentials:

```
Username: admin
Password: blank
```
Exact behavior varies by:

- Hardware model
- FortiOS release
- Factory-default configuration

```
Factory Default
      ↓
Change Password
      ↓
Enable MFA
      ↓
Restrict Management Access
```
Never leave a production FortiGate with default credentials.

Administrative access is configured per interface.

Example:

```
config system interface
    edit "port1"
        set ip 192.168.1.1 255.255.255.0
        set allowaccess https ping ssh
    next
end
```
Meaning:

```
HTTPS
  ↓
Web GUI
SSH
  ↓
CLI
PING
  ↓
ICMP
```
Possible management protocols depend on platform and configuration.

Recommended architecture:

```
                 Production Network
                        │
                        │
                     FortiGate
                        │
                        │
                 Management VLAN
                        │
                        ▼
                 Admin / Jump Host
```
Enterprise architecture:

```
Production Plane
       ≠
Management Plane
```
Preferred controls:

```
Management VLAN
+
OOB Management
+
Jump Server
+
Source IP Restriction
+
MFA
+
RBAC
+
Trusted Certificate
```
Administrative access is controlled through administrator accounts and administrator profiles.

Concept:

```
Administrator Account
          +
Administrator Profile
          ↓
Administrative Permissions
```
Provides broad administrative privileges.

Conceptually:

```
Global
+
VDOM
+
System
+
Network
+
Security
```
Can be limited according to:

- Permissions
- Features
- Read/write access
- VDOM scope
- Administrative role

Use:

```
Unique Admin Accounts
+
Least Privilege
+
MFA
```
instead of sharing one administrator account.

Concurrent administrative sessions determine whether the same administrative identity can maintain multiple active sessions.

Concept:

```
Administrator
    │
    ├── Session 1
    ├── Session 2
    └── Session 3
```
Prefer unique accounts for individual engineers.

Benefits:

```
Accountability
+
Auditability
+
Traceability
+
Incident Investigation
```
Production FortiGate management should use a trusted certificate.

Recommended architecture:

```
Enterprise CA
     │
     ▼
FortiGate Management Certificate
     │
     ▼
HTTPS
     │
     ▼
Administrator
```
Benefits:

- Trusted identity
- Reduced browser warnings
- Better management security
- Enterprise PKI integration

Changing the certificate or management port is not a substitute for access control.

VDOM means:

**Virtual Domain**

VDOMs allow a FortiGate to logically separate configurations and security domains.

Concept:

```
                 FortiGate
                     │
          ┌──────────┼──────────┐
          │          │          │
        VDOM-A     VDOM-B     VDOM-C
          │          │          │
       Network    Network    Network
       Policies   Policies   Policies
```
VDOMs can be used for:

- Multi-tenancy
- Organizational separation
- Administrative separation
- Network segmentation
- Resource isolation

Management scope can be:

```
Global
+
VDOM
```
depending on the administrator role.

One of the most important FortiGate concepts:

```
Routing
   ↓
WHERE?
```
versus:

```
Firewall Policy
   ↓
WHETHER?
```
Determines where the packet should go.

Determines whether the traffic is permitted.

Therefore:

```
Default Route
      ≠
Internet Permission
```
FortiGate evaluates policies according to policy lookup.

Conceptually:

```
Policy 1
   ↓
Policy 2
   ↓
Policy 3
   ↓
Policy 4
   ↓
...
   ↓
Implicit Deny
```
If no appropriate policy allows the traffic:

```
Traffic
   ↓
DENIED
```
```
No Matching Allow Policy
          ↓
        Deny
```
A policy can contain concepts such as:

```
Source Interface
Source Address
Destination Interface
Destination Address
Schedule
Service
Action
NAT
Authentication
Security Profiles
Logging
```
Mental model:

```
Traffic
   ↓
Interface
   ↓
Address
   ↓
Service
   ↓
Policy Match
   ↓
Security Controls
   ↓
Action
```
Typical LAN → Internet flow:

```
LAN Client
    ↓
FortiGate LAN
    ↓
Routing
    ↓
Firewall Policy
    ↓
NAT
    ↓
Security Inspection
    ↓
WAN
    ↓
Internet
```
Typical policy:

```
Source      = LAN
Destination = Internet
Service     = Required Services
Action      = ACCEPT
NAT         = Enabled where required
```
A static/default route does not automatically authorize traffic.

```
Route
 ↓
Path
Policy
 ↓
Permission
```
Security profiles provide additional inspection and enforcement.

Common security profiles/features include:

```
Antivirus
IPS
Web Filter
DNS Filter
Application Control
SSL/SSH Inspection
File Filter
DLP
Botnet Protection
```
A simplified policy architecture:

```
Firewall Policy
       │
       ├── Antivirus
       ├── IPS
       ├── Web Filter
       ├── Application Control
       ├── SSL Inspection
       └── Logging
```
For common Internet access scenarios:

```
Private IP
    ↓
FortiGate
    ↓
Source NAT
    ↓
Public IP
    ↓
Internet
```
Example concept:

```
192.168.10.10
      ↓
NAT
      ↓
203.0.113.10
```
NAT is a translation mechanism.

It is not itself a security policy.

```
NAT
 ≠
Firewall Authorization
```
FortiGate can support authentication workflows associated with firewall policies.

Conceptually:

```
Client
  ↓
Firewall Policy
  ↓
Authentication
  ↓
Redirect
  ↓
Authenticated Session
```
Example configuration structure:

```
config firewall policy
    edit <policy-id>
        set authentication-redirect-address <FQDN>
        set redirect-url <URL>
    next
end
```
Use the exact syntax and supported options for the installed FortiOS version.

FortiTelemetry provides communication between FortiClient and FortiGate in supported deployments.

Concept:

```
FortiClient
     │
     │ Telemetry
     ▼
FortiGate
```
A commonly associated port is:

```
TCP/8013
```
Exact communication behavior depends on the FortiOS and FortiClient versions and architecture.

CAPWAP is used for centralized wireless AP communication.

Concept:

```
FortiGate / Controller
          │
          │ CAPWAP
          ▼
       FortiAP
```
Common CAPWAP ports:

```
UDP/5246 → Control
UDP/5247 → Data
```
```
5246 = Control
5247 = Data
```
FortiManager provides centralized management.

```
                    FortiManager
                         │
          ┌──────────────┼──────────────┐
          ▼              ▼              ▼
      FortiGate       FortiGate       FortiGate
```
Typical functions:

- Centralized device management
- Policy management
- Configuration management
- Revision control
- Administrative workflows
- Large-scale deployment

```
FortiManager
     =
Centralized Management
```
FortiAnalyzer focuses on:

```
Logging
+
Analytics
+
Reporting
+
Investigation
```
Typical flow:

```
FortiGate
    │
    │ Logs
    ▼
FortiAnalyzer
    │
    ├── Search
    ├── Analytics
    ├── Reports
    └── Investigation
```
```
FortiAnalyzer
     =
Logs + Analytics + Reporting
```
Endpoint agent.

Capabilities can include:

- VPN
- Endpoint security
- ZTNA
- Telemetry
- Endpoint posture/security capabilities

Centralized management for FortiClient endpoints.

```
                    FortiClient EMS
                           │
             ┌─────────────┼─────────────┐
             ▼             ▼             ▼
        Endpoint 1    Endpoint 2    Endpoint 3
        FortiClient   FortiClient   FortiClient
```
```
FortiClient
    =
Endpoint Agent
FortiClient EMS
    =
Endpoint Management
```
FortiSIEM provides SIEM and security operations capabilities.

Concept:

```
Servers
Network Devices
Security Devices
Applications
Endpoints
      │
      ▼
  FortiSIEM
      │
      ├── Events
      ├── Correlation
      ├── Monitoring
      └── Security Operations
```
```
FortiSIEM
    =
SIEM / Security Operations
```
FortiWeb is a:

**Web Application Firewall (WAF)**

Concept:

```
Internet
   ↓
FortiWeb
   ↓
Web Application
```
Protection can focus on:

- Web application attacks
- HTTP/HTTPS traffic
- Application-layer threats
- OWASP-related attack patterns
- API/application protection

```
FortiGate
    =
Network Security
FortiWeb
    =
Web Application Security
```
FortiMail provides email security.

Concept:

```
Internet
   ↓
FortiMail
   ↓
Mail Infrastructure
```
Typical security areas:

- Anti-spam
- Malware protection
- Email security
- Content inspection
- Email policy enforcement

FortiNAC provides Network Access Control.

Concept:

```
Device
   ↓
Identity / Device Profiling
   ↓
Policy
   ↓
Network Access
```
Potential use cases:

- Device visibility
- Network access control
- Segmentation
- Endpoint/device profiling
- Guest access

FortiAuthenticator provides identity and authentication services.

Concept:

```
User
  ↓
Identity
  ↓
Authentication
  ↓
Authorization
  ↓
Network Access
```
It can integrate with enterprise identity environments and authentication mechanisms.

FortiSandbox provides sandbox-based malware analysis.

Concept:

```
Suspicious File
       ↓
FortiSandbox
       ↓
Analysis
       ↓
Behavior
       ↓
Verdict
```
Sandboxing complements signature-based detection by providing deeper analysis of suspicious objects.

FortiADC provides application delivery capabilities.

It can be conceptually compared to an application delivery/load-balancing platform.

```
Clients
   │
   ▼
FortiADC
   │
   ├── Server 1
   ├── Server 2
   └── Server 3
```
Typical concepts:

- Load balancing
- Application delivery
- Server health monitoring
- Traffic distribution

Wireless Access Point.

```
Client
  ↓
FortiAP
  ↓
Network
```
Wireless LAN Controller functionality/platform.

Concept:

```
                    Controller
                         │
              ┌──────────┼──────────┐
              ▼          ▼          ▼
           FortiAP     FortiAP     FortiAP
```
Wireless management can use CAPWAP.

FortiToken provides authentication-token functionality.

Concept:

```
Username + Password
        +
OTP / Token
        ↓
     MFA
```
FortiToken Mobile provides mobile token functionality.

FortiOS is the operating system used by FortiGate.

Conceptual stack:

```
                    FortiOS
────────────────────────────────
Firewall
Routing
VPN
SD-WAN
Security Profiles
Authentication
HA
VDOM
Logging
Wireless
Management
Troubleshooting
────────────────────────────────
Hardware Acceleration
────────────────────────────────
FortiGate Hardware
```
Configuration can be managed through:

```
GUI
CLI
FortiManager
Automation / APIs
```
Useful for:

- Configuration
- Monitoring
- Dashboard
- Reports
- Wizards

Essential for:

- Advanced configuration
- Troubleshooting
- Debugging
- Automation
- Precise control

```
GUI
 =
Visibility + Convenience
CLI
 =
Precision + Troubleshooting
```
Before major changes:

```
Firmware Upgrade
HA Changes
Major Policy Changes
Routing Changes
Interface Changes
VDOM Changes
```
perform:

```
BACKUP
```
Recommended backup architecture:

```
FortiGate
   ↓
Encrypted Backup
   ↓
Secure Repository
   ↓
Versioned Storage
```
Never publish production configuration backups to a public GitHub repository.

General workflow:

```
Backup File
    ↓
Verify Hardware
    ↓
Verify FortiOS Version
    ↓
Verify VDOM Structure
    ↓
Restore
    ↓
Reboot if Required
    ↓
Validate
```
Before restoring, check:

```
Platform
Firmware
Interfaces
VDOMs
Certificates
Encrypted Secrets
HA Configuration
```
Recommended process:

```
Current Version
      ↓
Check Upgrade Path
      ↓
Check Hardware Compatibility
      ↓
Read Release Notes
      ↓
Backup Configuration
      ↓
Upgrade
      ↓
Reboot
      ↓
Validate
```
Never think:

```
Upload
  ↓
Reboot
  ↓
Done
```
A production firmware upgrade is a controlled change.

When normal boot/recovery procedures are insufficient, a low-level firmware recovery process may be required.

High-level workflow:

```
Firmware Image
      ↓
TFTP Server
      ↓
FortiGate Boot Menu
      ↓
Network Connection
      ↓
Firmware Download
      ↓
Flash / Run Image
      ↓
Boot
```
Typical procedure:

1. Prepare the correct firmware image.
2. Configure a TFTP server.
3. Connect the required FortiGate interface.
4. Connect to the FortiGate console.
5. Reboot the device.
6. Enter the boot menu.
7. Select the firmware download/recovery option.
8. Enter the TFTP server IP.
9. Configure the FortiGate local IP.
10. Enter the firmware filename.
11. Download the firmware.
12. Select the appropriate boot/save option.
13. Reboot.
14. Validate system operation.

Do not assume that a single interface such as `WAN1` is universally used for firmware recovery.

The required interface and exact boot-menu procedure depend on the FortiGate model.

```
get system status
```
Useful information:

- Hostname
- Serial number
- FortiOS version
- System information

```
show system interface
```
```
config system interface
    edit "port1"
        set ip 192.168.1.1 255.255.255.0
        set allowaccess https ping ssh
    next
end
```
```
diagnose sys top
```
Useful for observing:

```
CPU
Processes
Memory
System Activity
```
```
execute reboot
```
```
execute factoryreset
```
**DESTRUCTIVE COMMAND**

Use only when the operational impact is fully understood.

```
execute shutdown
```
On supported releases/platforms:

```
execute backup config flash
```
Always verify syntax for the installed FortiOS version.

At NSE 4 level, you must understand how FortiGate forwards traffic.

At NSE 7 level, you must be able to determine **why the expected behavior is not occurring**.

The core methodology is:

```
FOLLOW THE PACKET
```
NSE 4-level FortiGate engineering should cover:

```
Interfaces
VLAN
Routing
Static Routes
Dynamic Routing
VDOM
HA
SD-WAN
VPN
DHCP
DNS
```
```
Firewall Policies
NAT
Authentication
Antivirus
IPS
Web Filtering
Application Control
SSL Inspection
Security Profiles
```
```
Administrators
Admin Profiles
Management Access
Certificates
Backup
Restore
Firmware
Logging
```
At advanced level, the goal changes from:

```
"What command do I use?"
```
to:

```
"Why is this system behaving this way?"
```
Important domains:

```
Advanced Threat Protection
Enterprise Firewall
Advanced FortiGate Troubleshooting
Secure Access
Cloud Security
Performance
Architecture
Traffic Flow
Security Inspection
Hardware Acceleration
```
The advanced engineer should think in layers.

```
Physical
   ↓
Interface
   ↓
Layer 2
   ↓
Layer 3
   ↓
Routing
   ↓
Policy
   ↓
NAT
   ↓
Session
   ↓
Security Inspection
   ↓
Hardware Offload
   ↓
Return Traffic
```
The question is not:

```
"Which command should I type?"
```
The question is:

```
"At which stage did the expected behavior change?"
```
```
Routing
   =
WHERE?
```
```
Firewall Policy
   =
WHETHER?
```
```
NAT
   =
Address Translation
```
```
Firewall Policy
   =
Traffic Authorization
