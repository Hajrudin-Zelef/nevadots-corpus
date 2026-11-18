FortiOS 7.2.x | NSE4 / NSE7 Practical Reference
SheynShield | Engineering Secure Networks
A practical covering Administrator Security, NTP/PTP, Management Ports, Session Processing, Auxiliary Sessions, NPU Offloading, Link Monitor, Configuration Save Modes, TPM and Workspace Transactions.
On a factory-default FortiGate, the initial admin account behavior depends on the FortiOS/model generation.
Never leave the factory/default administrator credential unchanged.
Immediately after initial access:
- Change the administrator password.
- Create named administrator accounts.
- Avoid daily administration with the shared admin account.
- Restrict management access to trusted interfaces.
- Prefer HTTPS/SSH over insecure management protocols.
- Enable MFA where supported.
FortiOS password policy can control whether an administrator is allowed to reuse the previous password.
config system password-policy
    set status enable
    set reuse-password disable
end
| Setting | Result | 
|---|---|
| reuse-password enable | Password reuse is allowed | 
| reuse-password disable | New password must differ from old password | 
| min-change-characters > 0 | Requires a minimum number of characters to differ | 
| min-change-characters +reuse-password | min-change-characters takes precedence | 
Example:
config system password-policy
    set status enable
    set minimum-length 12
    set min-upper-case-letter 1
    set min-lower-case-letter 1
    set min-number 1
    set min-non-alphanumeric 1
    set reuse-password disable
end
Fortinet documents reuse-password as the control determining whether administrators may reuse the same password; min-change-characters overrides it when both are enabled.
Accurate system time is not optional on a production FortiGate.
Incorrect time can affect:
- Logs
- Event correlation
- Scheduled policies
- Certificates
- SSL-dependent features
- Authentication
- Security investigations
- Troubleshooting
- SIEM correlation
First-day rule: Configure the timezone and a reliable time source during initial deployment.
Fortinet specifically notes that accurate FortiOS system time is required for features such as scheduling, logging and SSL-dependent functionality.
execute date 2026-08-29
execute time 12:30:00
Check:
execute date
execute time
Example for Tehran:
config system global
    set timezone 41
end
FortiOS timezone 41 corresponds to GMT+03:30 Tehran.
config system global
    set timezone 41
    set dst disable
end
dst controls daylight-saving-time behavior.
Important: Do not blindly enable DST because an old configuration example contains set dst enable. Timezone/DST behavior should match the actual jurisdiction and current FortiOS/timezone rules.
FortiOS exposes dst as the CLI control for daylight-saving time.
FortiGate can synchronize its clock using:
- FortiGuard NTP
- Custom NTP servers
- NTP authentication
- NTP server mode
Basic configuration:
config system ntp
    set ntpsync enable
    set type custom
    set syncinterval 60
    config ntpserver
        edit 1
            set server ntp.example.com
            set ntpv3 disable
        next
    end
endset ntpv3 enable
means:
NTPv3
while:
set ntpv3 disable
means:
NTPv4
FortiOS documents ntpv3 as the switch for NTPv3; when disabled, NTPv4 is used.
FortiOS supports NTP authentication using:
- MD5
- SHA1
Example:
config system ntp
    set ntpsync enable
    set type custom
    config ntpserver
        edit 1
            set server ntp.example.com
            set authentication enable
            set key-id 123
            set key "SECRET"
            set ntpv3 enable
        next
    end
end
| NTP Version | Authentication | 
|---|---|
| NTPv3 | MD5 | 
| NTPv4 | SHA1 | 
Fortinet's CLI reference documents MD5/SHA1 authentication and the ntpv3 behavior.
Security note: NTP authentication is useful for trusted/internal NTP infrastructure. Do not expose unnecessary NTP services to the Internet.
config system ntp
    set syncinterval 1
end
The value is in minutes.
FortiOS supports an NTP synchronization interval from 1 to 1440 minutes.
FortiGate can also provide NTP service to downstream devices.
config system ntp
    set server-mode enable
    set interface "LAN"
end
Conceptually:
                    Internet / Internal NTP
                              |
                              v
                       +-------------+
                       |  FortiGate  |
                       | NTP Client  |
                       +-------------+
                              |
                       NTP Server Mode
                              |
             +----------------+----------------+
             |                |                |
             v                v                v
           PC-01            SW-01            Server
FortiGate relays NTP requests to its configured upstream NTP server when server mode is enabled.
PTP is designed for environments requiring much tighter time synchronization than ordinary NTP.
Typical use cases:
- Telecom
- Industrial systems
- Financial systems
- Time-sensitive applications
- Specialized data-center environments
FortiOS supports PTP configuration including:
- Interface
- Multicast/hybrid mode
- End-to-End delay
- Peer-to-Peer delay
Example:
config system ptp
    set status enable
    set interface "port1"
    set delay-mechanism E2E
end
The delay is measured across the complete path between the master and slave.
Simplified concept:
Master                         Slave
  |                              |
  | -------- Sync -------------->|
  |                              |
  | <------ Delay_Req -----------|
  |                              |
  | -------- Delay_Resp -------->|
The slave uses timestamps to estimate path delay.
Each link/path segment measures its own propagation delay through peer-delay exchanges.
Master ---- Switch ---- Switch ---- Slave
           P2P delay    P2P delay
E2E = End-to-End path delay
P2P = Peer-to-Peer link delay
Check system time:
execute date
execute time
Check NTP:
diagnose sys ntp status
Test UDP/123 reachability:
execute telnet 192.168.20.200 123
Note: TCP telnet to UDP/123 is not a definitive NTP test. It only proves whether something is listening/reachable using the tested transport. For real NTP validation, use FortiGate NTP diagnostics and packet capture.
PTP debugging:
diagnose debug application ptpd -1
On a Windows NTP server, verify:
netstat -nao
Check the Windows Time service:
services.msc
    Windows Time
Restart:
net stop w32time
net start w32time
Typical policy location:
Computer Configuration
 └── Administrative Templates
     └── System
         └── Windows Time Service
             ├── Global Configuration Settings
             └── Time Providers
Verify:
- Windows NTP Client enabled
- NTP Server configured
- Appropriate AnnounceFlags
- Windows Time service running
Example:
config system global
    set admin-port 80
    set admin-sport 443
    set admin-https-redirect enable
    set admin-ssh-port 2142
    set admin-telnet-port 2323
end
Prefer:
HTTPS
SSH
Avoid:
HTTP
TELNET
unless there is a specific operational requirement.
set admin-https-redirect enable
Concept:
HTTP :80
   |
   | redirect
   v
HTTPS :443
config system global
    set default-service-source-port 20-30
end
This affects the source-port range used for system-generated TCP/UDP service traffic.
Do not confuse this setting with the destination service port of firewall policies.
FortiOS controls administrator idle timeout through:
config system global
    set admintimeout 10
end
For administrative environments:
5–10 minutes
is generally preferable to leaving long idle sessions active.
FortiOS defines admintimeout as the number of minutes before an idle administrator session times out.
Example:
config system global
    set admin-lockout-threshold 2
    set admin-lockout-duration 60
end
Meaning:
2 failed attempts
        |
        v
Account locked
        |
        v
60 seconds
FortiOS supports up to 10 failed attempts, while the lockout duration is configured in seconds.
GUI:
Log & Report
    >
System Events
    >
General System Events
Useful for identifying:
- Failed logins
- Successful logins
- Lockouts
- Administrative activity
- Authentication anomalies
Auxiliary Session allows FortiGate to create additional session state when traffic associated with an existing connection uses a different incoming or outgoing interface/path.
It is particularly relevant to:
- ECMP
- Asymmetric routing
- Multiple WAN links
- Load balancing
- Policy-based routing
- SD-WAN
- ADVPN
- Complex multi-path topologies
Fortinet describes auxiliary sessions as a mechanism for handling changes in incoming, outgoing, or return interfaces in environments such as ECMP and load balancing.
| FortiOS | Auxiliary Session | 
|---|---|
| 6.0 and earlier | Not supported | 
| 6.2.0–6.2.2 | Permanently enabled | 
| 6.2.3+ | Disabled by default; can be enabled | 
config system settings
    set auxiliary-session enable
end
Disable:
config system settings
    set auxiliary-session disable
end
FortiOS 7.2 CLI documents the default as disabled.
IPv4:
config system settings
    set asymroute enable
end
IPv6:
config system settings
    set asymroute6 enable
end
ICMP-specific controls also exist:
set asymroute-icmp enable
set asymroute6-icmp enable
FortiOS documents these as separate controls for IPv4, IPv6 and ICMP asymmetric routing.
These are related but not identical.
Asymmetric Routing
        |
        +---- Traffic uses different paths
        |
        v
Auxiliary Session
        |
        +---- Helps FortiGate maintain session handling
              when interfaces/paths change
asymroute controls acceptance/handling of asymmetric routing.
auxiliary-session controls creation/use of auxiliary sessions for changing traffic paths.
             ISP-1
               |
               v
Client ---- FortiGate ---- Server
               ^
               |
             ISP-2
Suppose:
Forward path  = ISP-1
Return path   = ISP-2
The original session was created through one interface, but return traffic arrives through another.
Without auxiliary sessions, FortiGate may need to modify/refresh the session state.
With auxiliary sessions:
Main Session
      |
      +---- Auxiliary Session
                   |
                   v
             Alternate Path
This can allow traffic to continue while preserving appropriate session handling.
A simplified FortiGate packet-processing model:
             Incoming Packet
                    |
                    v
                  CPU
                    |
             Session Lookup
                    |
             +------+------+
             |             |
           New           Existing
             |             |
             v             v
       Create Session   Session Check
             |             |
             v             v
       Policy Lookup    NPU Eligibility
             |             |
             v             v
        Routing/PBR       NPU
                           |
                           v
                    Fast Forwarding
The CPU is responsible for control-plane/session-establishment work such as:
- Initial packet processing
- Session lookup
- New session creation
- Policy decisions
- Routing decisions
- Session-state management
- Control-plane functions
On supported FortiGate platforms, the NPU can offload eligible data-plane processing.
Typical benefit:
CPU
 |
 | session establishment
 v
NPU
 |
 | high-speed forwarding
 v
Output Interface
NPU offloading does not mean the CPU disappears from the session.
The CPU generally performs the initial/session-control work, while the NPU can handle subsequent eligible packet forwarding.
When traffic changes interface/path:
Existing Session
      |
      v
Interface Changed
      |
      v
Session becomes DIRTY
      |
      v
Session/interface update
      |
      v
CPU processing
This can become expensive in environments with:
- ECMP
- Interface flapping
- Multiple WAN paths
- Frequent path changes
Existing Session
      |
      v
Path/interface changed
      |
      v
Auxiliary Session
      |
      v
NPU can continue eligible forwarding
This can reduce the need to repeatedly modify the original session.
A session may appear as:
state = dirty
A dirty session indicates that the session state requires additional processing/update.
Typical triggers include:
- Path change
- Incoming interface change
- Return interface change
- Routing changes
- ECMP behavior
- Asymmetric traffic
Example session output:
npu info:
flag=0x91/0x81
offload=8/8
Both directions show offload.
Another example:
npu info:
flag=0x91/0x00
offload=8/0
Only one direction is offloaded.
offload=x/y
x = one direction
y = reverse direction
Always interpret the exact output according to the FortiOS/platform version.
Client
  |
port1
  |
FortiGate
  |
port3
  |
Server
Return:
Server
  |
port3
  |
FortiGate
  |
port1
  |
Client
Return traffic can use the original session/path.
FortiGate evaluates the applicable route/PBR/SD-WAN decision.
If the best path is:
port1
traffic uses port1.
If the best path is:
port2
traffic may use port2.
When both PBR and SD-WAN-related routing decisions are involved, the exact decision process depends on the configuration and FortiOS version.
Do not memorize "PBR always wins" without checking the specific FortiOS routing decision order.
Original:
Session:
port1 ---> FortiGate ---> port3
Return arrives:
port4 ---> FortiGate
The existing session may become:
DIRTY
FortiGate updates the session/interface state.
High traffic or repeated path changes can increase CPU processing.
FortiGate can create:
Original Session
       +
Auxiliary Session
allowing traffic to continue using the alternate path.
Original:
port1 ---> FortiGate ---> port3
New incoming packet:
port2 ---> FortiGate
Existing Session
      |
      v
DIRTY
      |
      v
Session update
Existing Session
      |
      v
Auxiliary Session
      |
      v
Continue forwarding
Original:
Client
  |
port1
  |
FortiGate
  |
port3
  |
Server
A new route appears:
Server -> port4
If the original route remains valid, an established session may continue using the original path rather than immediately moving to the newly preferred route.
A route/interface change can trigger session handling that refreshes the path/interface information.
Auxiliary sessions should not be enabled blindly.
Fortinet specifically warns that routing symmetry should be considered in topologies such as:
- SD-WAN Hub-and-Spoke
- ADVPN
- Other environments where return traffic is expected to be symmetric
Symmetric traffic expected?
        |
        +---- YES ---> Prefer disabling auxiliary-session
        |
        +---- NO ----> Evaluate auxiliary-session
The correct setting depends on the actual topology and traffic behavior.
diagnose sys session list
Useful information includes:
state
dev
npu_state
npu info
offload
vlan
qid
diagnose sys session list
Search for:
state=dirty
or:
state = dirty
DIRTY
  |
  +-- Path/interface changed
  +-- Session state requires update
  +-- Possible CPU processing increase
Look for:
npu info:
offload=8/8
or:
offload=8/0
8/8 = both directions offloaded
8/0 = one direction offloaded
Example:
reflect info 0:
dev=37->38/38->37
npu_state=0x000400
npu info:
flag=0x91/0x00
offload=8/0
...
total reflect session num: 1
The exact fields are platform/version dependent.
Use the output to correlate:
Session
   |
   +-- Interface
   +-- NPU state
   +-- Offload state
   +-- Auxiliary/reflect information
                 ISP-1
              192.168.254.2
                   |
                 port1
                   |
             +-----------+
             |  FGT-1    |
             +-----------+
                   |
                 port3
                   |
             192.168.20.0/24
                   |
                Server
                 ISP-2
              192.168.200.1
                   |
                 port2
                   |
             +-----------+
             |  FGT-1    |
             +-----------+
Primary:
0.0.0.0/0
    via 192.168.254.2
    distance 10
Backup:
0.0.0.0/0
    via 12.12.12.2
    distance 250
Concept:
Incoming Interface:
    WAN
Destination:
    192.168.20.0/24
Outgoing Interface:
    port3
Gateway:
    12.12.12.2
Example:
config system link-monitor
    edit "WAN1"
        set addr-mode ipv4
        set srcintf "port1"
        set server-config default
        set server-type static
        set server 192.168.254.2
        set protocol ping
        set gateway-ip 192.168.254.2
        set source-ip 0.0.0.0
        set interval 500
        set probe-timeout 500
        set failtime 5
        set recoverytime 5
        set probe-count 30
        set ha-priority 1
        set update-cascade-interface enable
        set update-static-route enable
        set update-policy-route enable
        set status enable
    next
    edit "WAN2"
        set addr-mode ipv4
        set srcintf "port2"
        set server-config default
        set server-type static
        set server 192.168.200.1
        set protocol ping
        set gateway-ip 192.168.200.1
        set source-ip 0.0.0.0
        set interval 500
        set probe-timeout 500
        set failtime 5
        set recoverytime 5
        set probe-count 30
        set ha-priority 1
        set update-cascade-interface enable
        set update-static-route enable
        set update-policy-route enable
        set status enable
    next
end
| Parameter | Meaning | 
|---|---|
| srcintf | Interface used for the probe | 
| server | Health-check destination | 
| protocol | Probe protocol | 
| gateway-ip | Gateway associated with monitor | 
| interval | Probe interval | 
| probe-timeout | Probe timeout | 
| failtime | Failure threshold | 
| recoverytime | Recovery threshold | 
| probe-count | Probe count | 
| update-static-route | Update static route state | 
| update-policy-route | Update policy route state | 
| update-cascade-interface | Cascade interface state changes | 
For a routing/failover lab:
Firewall policies
    |
    +-- WAN access
    +-- Internal access
NAT should be enabled/disabled according to the actual traffic design.
Do not blindly use NAT disabled in production. The correct setting depends on whether the traffic requires source NAT.
On both FortiGates:
config system settings
    set auxiliary-session enable
    set asymroute enable
end
For multi-FortiGate designs:
Keep participating FortiGate devices on compatible and preferably identical FortiOS releases when reproducing/operating a feature-sensitive design.
Example conceptual Netwatch logic:
Netwatch
 |
 +-- UP
 |    |
 |    +-- Enable primary route
 |    +-- Disable backup route
 |
 +-- DOWN
      |
      +-- Disable primary route
      +-- Enable backup route
Example:
Host:
192.168.20.200
Route logic:
Primary:
0.0.0.0/0 -> 192.168.254.2
Backup:
0.0.0.0/0 -> 192.168.200.1
Masquerade:
IP
 └── Firewall
      └── NAT
           └── masquerade
FortiGate provides configuration save behavior through:
config system global
    set cfg-save automatic
end
Available modes include:
automatic
manual
revert
Fortinet documents these configuration-save modes under config system global.
config system global
    set cfg-save automatic
end
Changes are automatically saved.
Concept:
CLI/GUI Change
      |
      v
RAM
      |
      v
Flash
config system global
    set cfg-save manual
end
Changes are applied but must be explicitly saved.
execute cfg saveconfig system global
    set cfg-save revert
    set cfg-revert-timeout 600
end
Concept:
Change configuration
       |
       v
Temporary state
       |
       +---- Commit/save ---> Keep changes
       |
       +---- Timeout/reboot -> Revert
600 means 600 seconds.
Save:
execute cfg save
Reload/reboot:
execute cfg reloadSAVE   = write configuration
RELOAD = reboot/reload device
Fortinet documents execute cfg save and execute cfg reload in configuration save mode.
On supported FortiGate hardware, TPM can protect sensitive cryptographic material.
TPM can help protect:
- Passwords
- Keys
- Sensitive configuration data
Fortinet states that TPM is disabled by default on supported FortiGate hardware.
Enable:
config system global
    set private-data-encryption enable
end
FortiGate then prompts for a:
32 hexadecimal digit
master/private data encryption key
Simplified:
32-hex-digit
Master Encryption Password
          |
          v
       AES-128-CBC
          |
          v
Sensitive Data
          +
          |
          v
        TPM
          |
          v
     RSA-2048
          |
          v
Primary Key
Fortinet describes the master-encryption-password as protecting sensitive data using AES-128-CBC and the TPM-generated 2048-bit primary key as protecting that master password using RSA-2048.
Critical exam point:
TPM != Full Disk Encryption
Fortinet explicitly notes that the TPM module does not encrypt the disk drive.
TPM-protected configuration introduces an important dependency.
Backup
  |
  v
TPM-protected encryption information
  |
  v
Restore
Restore behavior depends on:
TPM state
+
Master Encryption Password
| Restore Condition | Result | 
|---|---|
| TPM disabled | Protected configuration cannot be restored | 
| TPM enabled + wrong master key | Cannot restore protected configuration | 
| TPM enabled + matching master key | Configuration can be restored | 
For an HA cluster:
FGT-A
  |
  +-- Master Encryption Key
  |
  v
FGT-B
HA members must use the same master encryption key for the cluster to form correctly and synchronize protected configuration.
diagnose hardware test infodiagnose hardware deviceinfo tpmdiagnose tpm
Examples include:
- Alert email credentials
- BGP/routing-related credentials
- External resource credentials
- FortiGuard proxy password
- FortiToken seeds
- HA password
- IPsec PSK
- Link Monitor server password
- Local certificate private keys
- LDAP/RADIUS/FSSO credentials
- Modem/PPPoE credentials
- NTP credentials
- SDN connector credentials
- SNMP credentials
- Wireless security credentials
Private Data Encryption
        |
        +-- Passwords
        +-- PSKs
        +-- Private Keys
        +-- Authentication Secrets
        +-- Service Credentials
FortiGate replacement messages can customize user-facing messages generated by security features.
Common groups/categories include:
UTM
 ├── admin
 ├── alertmail
 ├── custom-message
 ├── fortiguard-wf
 ├── ftp
 ├── http
 ├── icap
 ├── mail
 ├── nac-quar
 ├── spam
 ├── sslvpn
 ├── traffic-quota
 ├── utm
 └── webproxy
AUTH
 ├── auth
 └── webproxy
Enable GUI replacement-message groups:
config system settings
    set gui-replacement-message-groups enable
endconfig emailfilter profile
    edit "newmsgs"
        set replacemsg-group "newutm"
    next
end
Firewall policy:
config firewall policy
    edit 1
        set replacemsg-override-group "newauth"
        set inspection-mode proxy
        set emailfilter-profile "newmsgs"
    next
endFirewall Policy
       |
       +-- Proxy inspection
       |
       +-- Email Filter Profile
       |
       +-- Replacement Message
FortiGate GUI provides a CLI/script interface for deploying configuration commands.
Typical workflow:
GUI
 |
 v
CLI / Script
 |
 v
Paste commands
 |
 v
Execute
A CLI script comment begins with:
#
Example:
# Configure administrator timeout
config system global
    set admintimeout 10
end
Everything after # on that line is treated as a comment.
Workspace/transaction mode is useful when configuration changes need controlled deployment.
Concept:
Administrator A
       |
       v
Transaction
       |
       +---- Validate
       |
       +---- Commit
       |
       +---- Abort
Instead of immediately exposing every change to the rest of the system, configuration changes are held in a transaction until committed.
Fortinet documents Workspace Mode as a change-control mechanism where changes are made in a local CLI process and become available to other processes after commit.
execute config-transaction start
Or specify a timeout:
execute config-transaction start 5
The timeout is specified in minutes.
FortiOS supports a transaction timeout from 1 to 60 minutes, with a default of 5 minutes.
execute config-transaction commit
Concept:
Transaction
    |
    v
Commit
    |
    v
Configuration becomes active globally
execute config-transaction abort
Concept:
Transaction
    |
    v
Abort
    |
    v
Discard transaction changes
Example:
config transaction id=1 will expire in 30 seconds
config transaction id=1 will expire in 20 seconds
config transaction id=1 will expire in 10 seconds
config transaction id=1 has expired
If the administrator does not commit the transaction before expiration, the transaction expires.
You may encounter:
Can not config the object since either the object or the referenced objects
are being configured by other transactions.
Command fail.
Return code 14
Meaning:
Another active transaction
        |
        v
Object/reference locked
        |
        v
Your configuration cannot modify it
diagnose sys config-transaction show txn-meta
Useful for:
Active transaction information
Concurrent transaction/session information
diagnose sys config-transaction show txn-info
Useful for:
Transaction changes
Modification information
diagnose sys config-transaction show txn-entity
Shows affected entities.
diagnose sys config-transaction show txn-lock
Shows locked objects.
Useful when another administrator is modifying configuration.
diagnose sys config-transaction show txn-cli-commands
Shows CLI commands associated with the transaction.
diagnose sys config-transaction show mctx
Useful for examining memory/context information related to the transaction.
If memory resources become exhausted, configuration transaction operations may be affected.
Some configuration areas cannot be changed through a workspace transaction.
Examples include:
config system console
config system resource-limits
config system elbc
config system global
set split-port
set vdom-admin
set management-vdom
set wireless-mode
set internal-switch-mode
end
config system settings
set opmode
end
config system npu
config system np6
config system wireless
set mode
end
config system vdom-property
config system storage
Fortinet documents these restrictions for Workspace Mode.
admin-lockout-threshold
        |
        v
Failed login count
admin-lockout-duration
        |
        v
Lockout time
reuse-password enable
        |
        v
Password reuse allowed
reuse-password disable
        |
        v
New password required
min-change-characters > 0
        |
        v
Overrides reuse-password
NTP
 |
 +-- Normal network time synchronization
PTP
 |
 +-- Precision/time-sensitive environments
ntpv3 enable
    =
NTPv3
ntpv3 disable
    =
NTPv4
E2E = End-to-End
P2P = Peer-to-Peer
FortiOS <= 6.0
    -> Not supported
6.2.0 - 6.2.2
    -> Permanently enabled
6.2.3+
    -> Disabled by default
ECMP
SD-WAN
Multiple paths
Asymmetric traffic
       |
       v
Auxiliary Session
       |
       v
Additional session handling
config system settings
    set asymroute enable
endFirst packet / session setup
            |
            v
           CPU
            |
            v
       Session created
            |
            v
       NPU eligibility
            |
            v
           NPU
            |
            v
    High-speed forwarding
Path/interface change
        |
        v
Session update required
        |
        v
DIRTY
automatic
    |
    +-- Automatically save
manual
    |
    +-- execute cfg save
revert
    |
    +-- Save point + timeout
start
  |
  v
modify
  |
  +---- abort ---> discard
  |
  +---- commit --> apply
Private Data Encryption
          |
          v
Master Encryption Password
          |
          +---- AES-128-CBC
          |
          v
Sensitive Data
TPM
 |
 +---- RSA-2048 Primary Key
 |
 v
Protects Master Encryption Password
| Goal | Command | 
|---|---|
| Current date | execute date | 
| Current time | execute time | 
| NTP status | diagnose sys ntp status | 
| PTP debug | diagnose debug application ptpd -1 | 
| Session table | diagnose sys session list | 
| TPM hardware info | diagnose hardware deviceinfo tpm | 
| TPM diagnostics | diagnose tpm | 
| Hardware test | diagnose hardware test info | 
| Transaction metadata | diagnose sys config-transaction show txn-meta | 
| Transaction info | diagnose sys config-transaction show txn-info | 
| Transaction entities | diagnose sys config-transaction show txn-entity | 
| Transaction locks | diagnose sys config-transaction show txn-lock | 
| Transaction CLI commands | diagnose sys config-transaction show txn-cli-commands | 
| Transaction memory context | diagnose sys config-transaction show mctx | 
- Change factory/default admin credential
- Create named administrator accounts
- Enable appropriate password policy
- Disable unnecessary password reuse
- Configure administrator idle timeout
- Configure login lockout
- Restrict management interfaces
- Prefer HTTPS/SSH
- Disable unnecessary Telnet/HTTP exposure
- Configure correct timezone
- Configure NTP
- Verify NTP synchronization
- Configure logging destination
- Verify certificate/time-dependent features
- Identify whether routing is symmetric
- Identify ECMP
- Identify SD-WAN
- Identify PBR
- Identify ADVPN
- Identify multiple ISP paths
- Determine whether return traffic can change interface
-  Evaluate auxiliary-session
-  Evaluate asymroute
- Verify NPU offload
-  Investigate dirty sessions
- Compare CPU/NPU behavior under load
- Use Workspace Mode for sensitive changes
- Understand transaction timeout
- Commit deliberately
- Abort failed changes
- Check transaction locks
-  Use cfg-save revert for risky remote changes
- Set a reasonable revert timeout
- Keep configuration backups
- Verify TPM support
- Understand Private Data Encryption
- Protect the master encryption key
- Document HA key requirements
- Test backup/restore procedures
- Understand TPM restore dependencies
- Remember: TPM is not disk encryption
                    FORTIGATE SYSTEM BASICS
                             |
       +---------------------+----------------------+
       |                     |                      |
    SECURITY                TIME                 SESSION
       |                     |                      |
 Password Policy            NTP                 Auxiliary
 Lockout                    PTP                 Asymroute
 Admin Timeout              TZ                  ECMP
 Management Ports           Logs                SD-WAN
       |                     |                      |
       +---------------------+----------------------+
                             |
                         PROCESSING
                             |
                     +-------+-------+
                     |               |
                    CPU             NPU
                     |               |
              Session Setup      Fast Path
              Policy/Routing     Offloading
                             |
                             v
                      CONFIGURATION
                             |
                +------------+------------+
                |                         |
          Save Modes                 Transactions
          automatic                 start/commit
          manual                    abort/timeout
          revert                    locks
                             |
                             v
                            TPM
                             |
                    Private Data Encryption
Auxiliary Session and Asymmetric Routing are the same thing.
❌ Wrong.
asymroute          = asymmetric routing behavior
auxiliary-session  = additional session handling
NPU handles the entire session from the first packet.
❌ Oversimplified.
CPU -> session establishment/control
NPU -> eligible data-plane forwarding
TPM encrypts the FortiGate disk.
❌ Wrong.
TPM protects cryptographic material.
TPM != Full Disk Encryption
NTPv3 is used when ntpv3 disable is configured.
❌ Wrong.
ntpv3 enable  = NTPv3
ntpv3 disable = NTPv4
Workspace changes are immediately committed globally.
❌ Wrong.
start
  |
modify
  |
commit
Changes become available globally after commit.
execute cfg save means reboot.
❌ Wrong.
execute cfg save   = save configuration
execute cfg reload = reload/reboot
Auxiliary sessions should always be enabled in SD-WAN.
❌ Wrong.
Routing symmetry must be considered. Fortinet specifically recommends evaluating the effect on symmetric return traffic in SD-WAN hub/spoke and ADVPN-type topologies.
FortiGate  · FortiOS 7.2  · FortiGate NSE4 · FortiGate NSE7 · Fortinet NSE4 notes · Fortinet NSE7 notes · FortiGate auxiliary session · FortiGate asymmetric routing · FortiGate NPU offloading · FortiGate NTP · FortiGate PTP · FortiGate TPM · FortiGate workspace mode · FortiGate configuration transaction · FortiGate troubleshooting commands · FortiGate hardening · FortiGate session troubleshooting · FortiOS CLI 
The real skill is not memorizing CLI commands.
Understand the relationship between:
Routing → Session State → CPU → NPU → Auxiliary Session → Configuration State
Once this chain is clear, troubleshooting complex FortiGate behavior becomes much easier.
SheynShield | Engineering Secure Networks
- 
YouTube — SheynShield 
  - Fortinet NSE content
  - FortiGate troubleshooting
  - Network Security Engineering