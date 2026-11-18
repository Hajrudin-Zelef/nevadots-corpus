FortiGuard Services, Updates, Anycast, FortiManager, IoT Detection, Proxy & Air-Gap
Practical reference for FortiOS Administration / NSE4 / NSE7
FortiGuard provides cloud-based security intelligence and update services for FortiGate.
Common FortiGuard services include:
- Antivirus (AV)
- Intrusion Prevention System (IPS)
- Application Control
- AntiSpam
- Web Filtering
- Web Application Firewall (WAF)
- Botnet / threat intelligence
- IoT device identification
- Security rating / reputation services
- Firmware and security database updates
For normal cloud-based FortiGuard operation:
FortiGate
   |
   | HTTPS / DNS / FortiGuard protocols
   v
FortiGuard Distribution Network (FDN)
The FortiGate generally needs Internet connectivity to:
- Validate its FortiGuard license.
- Reach FortiGuard services.
- Download security databases.
- Receive updated security intelligence.
AV and IPS packages are cryptographically signed by Fortinet.
During an update, FortiGate validates the package signature before accepting it.
| Level | Behavior | 
|---|---|
| level-0 | Accept unsigned package | 
| level-1 | Warn and request confirmation | 
| level-2 | Reject unsigned package | 
If no level is configured, the effective behavior is generally Level 1.
⚠️ Security levels are preconfigured on the BIOS on supported platforms.
FortiGuard Package
       |
       v
Signature Validation
       |
       +---- Valid ----> Accept
       |
       +---- Invalid --> Security-level dependent behavior
FortiGuard automatic update behavior can operate at intervals determined by the FortiGuard update mechanism and contract state.
Typical automatic update intervals can range from approximately:
10 minutes
      |
      v
...
      |
      v
60 minutes
config system autoupdate schedule
    set status enable
    set frequency automatic
end
The update interval can be influenced by the percentage of remaining valid contract time.
Conceptually:
More remaining contract
        |
        v
Shorter update interval
        |
        v
More frequent updates
Example from the training scenario:
~70% contract remaining
        -> ~10 minute interval
~50% contract remaining
        -> ~20 minute interval
Exam Tip: Do not assume that every FortiGate always uses exactly the same update interval. Hardware/platform, service state, FortiOS version and FortiGuard policy can affect behavior.
On supported hardware/platforms, FortiGate can maintain a persistent secure connection to FortiGuard when the feature is available.
Conceptually:
FortiGate
   |
   | Persistent HTTPS connection
   v
FortiGuard
   |
   | New update notification
   v
fds_notify
   |
   | Download request
   v
FortiGuard Update Server
The fds_notify daemon waits for update notifications and then initiates the update download.
This feature availability is platform/version dependent.
The Improve IPS Quality option allows FortiGate to send attack-related information to FortiGuard.
Purpose:
Attack detected
      |
      v
FortiGate
      |
      | Security telemetry
      v
FortiGuard
      |
      v
Threat intelligence / signature improvement
The objective is to help FortiGuard improve IPS signatures as attacks evolve.
PUP/PUA detection allows FortiGate Antivirus functionality to identify:
- Potentially Unwanted Applications
- Grayware
- Other potentially unwanted software
This can provide an additional detection layer beyond conventional malware signatures.
FortiGate can use a proxy/tunnel for certain FortiGuard update operations.
Example:
config system autoupdate tunneling
    set address 1.2.3.4
    set port 1344
    set username test
    set password 123
    set status enable
end
Proxy tunneling is supported only for specific services, including:
- Registration
- Antivirus updates
- IPS updates
For FortiGate VMs, proxy tunneling can additionally be used for license validation.
A critical distinction:
AV / IPS Update
       |
       +---- Proxy tunneling supported
Web Filtering / Spam Rating
       |
       +---- UDP-based communication
Web filtering and spam-filtering services may use UDP communication such as:
UDP/53
UDP/8888
UDP traffic cannot simply be redirected through a traditional proxy.
Therefore, even if FortiOS supports HTTPS-based FortiGuard communication for some services, do not assume all FortiGuard traffic can traverse the same proxy.
For environments without direct Internet access, FortiManager can provide local update and rating services.
Architecture:
                 Internet
                    |
                    v
              FortiManager
             Local Services
              /          \
             /            \
       Update              Rating
       AV/IPS              Web/Spam
          \                  /
           \                /
              FortiGate
This is particularly useful in:
- Closed networks
- Restricted networks
- OT environments
- Data centers
- Air-gapped or partially isolated environments
Example:
config system central-management
    set type fortimanager
    set fmg 192.168.254.200
    config server-list
        edit 1
            set server-type update
            set server-address 192.168.254.200
        next
        edit 2
            set server-type rating
            set server-address 192.168.254.200
        next
    end
    set fmg-update-port 443
    set include-default-servers enable
end
| Server Type | Typical Purpose | 
|---|---|
| update | AV / IPS and update-related services | 
| rating | Web filtering / AntiSpam rating | 
set include-default-servers enable
This allows FortiGate to retain default FortiGuard servers as fallback when configured FortiManager connectivity is unavailable.
Older FortiOS/FortiManager implementations may use different ports or mechanisms. Always verify the target release.
FortiGuard can use Anycast to improve global connectivity and reduce latency.
                    FortiGuard
                 Global Anycast IP
                         |
          +--------------+--------------+
          |              |              |
        Region A       Region B       Region C
          |              |              |
       FortiGate       FortiGate       FortiGate
Instead of manually selecting a distant FortiGuard server, DNS/Anycast routing can direct the FortiGate toward an appropriate service location.
globalupdate.fortinet.net
globalguardservice.fortinet.net
update.fortiguard.net
service.fortiguard.net
securewf.fortiguard.net
usupdate.fortinet.net
usguardservice.fortinet.net
Non-Anycast examples:
usupdate.fortiguard.net
usservice.fortiguard.net
ussecurewf.fortiguard.net
euupdate.fortinet.net
euguardservice.fortinet.net
config system fortiguard
    set update-server-location automatic
end
Possible location behavior can include:
automatic
usa
eu
automatic is generally associated with selecting an appropriate FortiGuard location, commonly using Anycast where supported.
Example:
config system fortiguard
    set protocol https
    set port 443
end
Depending on the FortiGuard service and FortiOS release, other ports/protocols may be involved.
Commonly encountered:
HTTPS  443
DNS    53
UDP    8888
Do not test FortiGuard connectivity by checking only TCP/443.
A complete troubleshooting process should consider:
DNS
 |
 +--> FortiGuard hostname resolution
 |
Network
 |
 +--> Routing
 |
Firewall
 |
 +--> Required ports
 |
TLS
 |
 +--> Certificate / trust
 |
FortiGuard
 |
 +--> Service/license availability
FortiGate can cache certain FortiGuard results locally.
The cache can store web filtering results locally for a configurable period.
Concept:
Client
  |
  v
FortiGate
  |
  +---- Cache hit ----> Local decision
  |
  +---- Cache miss ---> FortiGuard
Example:
config system fortiguard
    set webfilter-cache enable
    set webfilter-cache-ttl 3600
    set webfilter-timeout 15
end
config system fortiguard
    set antispam-cache enable
    set antispam-cache-ttl 1800
    set antispam-cache-mpercent 2
    set antispam-timeout 7
end
Cache values and supported options are release dependent.
FortiGuard services can have force-off controls.
Example:
config system fortiguard
    set antispam-force-off disable
    set webfilter-force-off disable
end
Conceptually:
force-off enable
      |
      v
Service disabled
force-off disable
      |
      v
Service available
Example configuration:
config system fortiguard
    set outbreak-prevention-force-off disable
    set outbreak-prevention-cache enable
    set outbreak-prevention-cache-ttl 300
    set outbreak-prevention-cache-mpercent 2
    set outbreak-prevention-timeout 7
end
The cache helps reduce repeated external lookups.
Example:
config system fortiguard
    set sdns-server-ip 208.91.112.220
    set sdns-server-port 53
end
SDNS-related communication can be important for FortiGuard DNS/security services.
FortiGate can periodically send encrypted security statistics to FortiGuard.
Potential telemetry includes statistics related to:
- Antivirus
- IPS
- Botnet IP lists
- Application Control
The information can include device-related metadata such as:
- FortiGate IP address
- Serial number
- Country
The purpose is to improve Fortinet threat intelligence and security services.
The notes specify:
60 minutes
config system global
    set fds-statistics enable
    set fds-statistics-period 60
end
FortiGuard can use submitted malware statistics to identify active threats.
Conceptually:
New malware activity
        |
        v
FortiGate telemetry
        |
        v
FortiGuard analysis
        |
        v
Signature activity evaluation
        |
        +---- Active ----> Active AV database
        |
        +---- Inactive --> Extended AV database
If activity for an inactive threat reappears, its signature can be promoted back into the active database.
FortiGate and FortiGuard establish a secure TLS connection before exchanging protected data.
Conceptually:
FortiGate                         FortiGuard
   |                                  |
   |---- TLS ClientHello ------------>|
   |                                  |
   |<--- ServerHello + Certificate ---|
   |                                  |
   |---- Certificate / Key Info ----->|
   |                                  |
   |<--- Certificate Verify ----------|
   |<--- Finished --------------------|
   |                                  |
   |---- Finished ------------------->|
   |                                  |
   |<==== Encrypted Communication ===>|
The certificates must belong to a trusted Fortinet certificate chain.
During TLS validation, FortiGate evaluates the server identity and certificate status.
Important failure conditions include:
Certificate CN/SAN mismatch
        |
        v
DNS-resolved domain != certificate identity
        |
        v
TLS handshake aborted
Other abort conditions can include:
- Invalid OCSP status
- Revoked issuer CA
- Untrusted certificate chain
FortiGuard connectivity can use certificate status validation such as OCSP.
Conceptually:
FortiGate
   |
   | ClientHello
   | + OCSP status request
   v
FortiGuard
   |
   | Certificate + OCSP status
   v
FortiGate
   |
   +---- Valid ----> Continue TLS
   |
   +---- Invalid --> Abort
Exact certificate-validation behavior can vary by FortiOS release and service.
Example:
config system fortiguard
    set fortiguard-anycast enable
    set fortiguard-anycast-source fortinet
end
Depending on the FortiOS release, alternative source options may be available.
A useful conceptual distinction:
| Feature | Direct FortiGuard | Local FortiManager | 
|---|---|---|
| Anycast | Available | Local architecture | 
| Internet dependency | Usually yes | Can reduce direct dependency | 
| AV/IPS updates | Yes | Yes | 
| Rating services | Yes | Yes | 
| Local control | Lower | Higher | 
| Closed environment | Limited | Useful | 
| Centralized update distribution | Limited | Strong | 
For large environments:
FortiGuard
     |
     v
FortiManager
     |
     +---- FGT-01
     +---- FGT-02
     +---- FGT-03
     +---- FGT-04
diagnose sys service-communication
Useful for inspecting FortiGuard/service communication status.
diagnose debug application updated -1
diagnose debug enable
Use this when troubleshooting update behavior.
Remember to stop debugging after troubleshooting:
diagnose debug disable
Check:
System
  >
FortiGuard
  >
License
Also inspect:
Log & Report
  >
System Events
  >
General System Events
Cloud/service communication logging can also help identify:
- License problems
- FortiGuard connectivity issues
- Update failures
- Authentication problems
FortiGuard Labs provides online lookup resources.
Use it to determine:
- URL category
- URL rating
- Classification
It can also be used to request reevaluation of incorrectly categorized URLs.
Useful for researching:
- Viruses
- Botnet C&C
- IPS signatures
- Vulnerabilities
- Mobile malware
FortiGuard Threat Encyclopedia
Search application signatures and classifications.
FortiGuard Application Control
FortiGuard IoT Detection is a subscription-based service that can help identify devices not completely identified by the local device database.
Architecture:
Unknown Device
      |
      v
FortiGate Device Detection
      |
      v
Local CIDB
      |
      +---- Known ----> Device identified
      |
      +---- Unknown --> FortiGuard
                            |
                            v
                       Device analysis
                            |
                            v
                       Result returned
The FortiGate should:
- Be registered with FortiCare.
- Have an appropriate IoT Detection license.
- Be connected to a supported FortiGuard Anycast service.
- Have device detection enabled on the relevant interface.
Device
  |
  | DHCP / MAC / HTTP / traffic characteristics
  v
FortiGate
  |
  | Device information
  v
FortiGuard Collection Server
  |
  v
FortiGuard Query Server
  |
  v
Device identification
  |
  v
FortiGate Device Inventory
View results from:
Dashboard
  >
Users & Devices
  >
Device Inventory
For troubleshooting/testing:
diagnose cid sigs disable
This disables the local device signature database so FortiGate can query FortiGuard for device identification.
diagnose debug application iotd -1
diagnose debug enable
diagnose user device list
FortiAP can collect information about connected devices and provide it to FortiGate for device identification.
Potential data sources include:
DHCP
MAC address
HTTP
Device behavior
Traffic characteristics
Architecture:
Client Device
      |
      v
    FortiAP
      |
      v
  FortiGate
      |
      v
 FortiGuard IoT
      |
      v
Device intelligence
      |
      v
  FortiGate
Example:
config wireless-controller setting
    edit ap-1
        set device-weight 1
        set device-holdoff 5
        set device-idle 1440
    end
end
The notes describe weighting of different device-identification sources.
Conceptually:
FortiGuard intelligence
        +
DHCP information
        +
Behavioral analysis
        |
        v
Confidence score
        |
        v
Device classification
Exact scoring and supported ranges are version/platform dependent. Verify the CLI reference for the target FortiOS release.
Controls how long FortiGate waits before sending device information toward FortiGuard.
Example:
device-holdoff = 5 minutes
Controls how long inactive device information remains in the table.
Example:
device-idle = 1440 minutes
FortiGate can use an explicit proxy for certain FortiCloud/FortiGuard communication.
Concept:
FortiGate
    |
    | Proxy authentication
    v
Explicit Proxy
    |
    v
Internet
    |
    v
FortiCloud / FortiGuard
⚠️ Not every FortiGuard service supports these proxy settings. Web Filter service traffic, in particular, may follow a different communication mechanism.
config firewall proxy-policy
    edit 1
        set proxy explicit-web
        set dstintf port1
        set srcaddr all
        set dstaddr all
        set service webproxy
        set action accept
        set schedule always
        set logtraffic all
        set users guest1
    next
end
config user local
    edit guest1
        set type password
        set passwd <PASSWORD>
    next
end
config authentication scheme
    edit local-basic
        set method basic
        set user-database local-user-db
    next
end
config authentication rule
    edit local-basic-rule
        set srcaddr all
        set ip-based disable
        set active-auth-method local-basic
    next
end
config firewall policy
    edit 1
        set srcintf port3
        set dstintf port1
        set srcaddr all
        set dstaddr all
        set action accept
        set schedule always
        set service dns
        set fsso disable
        set nat enable
    end
end
On the FortiGate using the proxy:
config system fortiguard
    set proxy-server-ip 192.168.254.251
    set proxy-server-port 8080
    set proxy-username guest1
    set proxy-password <PASSWORD>
end
Test FortiCloud/FortiGuard-related communication as appropriate for the target FortiOS release.
🔐 Never place real production credentials inside shared documentation or GitHub repositories.
FortiOS firmware images include Fortinet Internet Service Database (ISDB) information.
Depending on the image/update state, the built-in ISDB can be:
Partial / lightweight ISDB
        |
        v
Core Fortinet services
Full ISDB
        |
        v
Large database with current service objects
The lightweight database helps policies and policy routes referencing Fortinet services continue functioning after a firmware upgrade before the full database is retrieved.
diagnose firewall internet-service list
This displays ISDB entries.
config firewall policy
    edit 1
        set srcintf port1
        set dstintf port3
        set srcaddr all
        set dstaddr all
        set internet-service enable
        set internet-service-id 1245187 1245326 1245324 1245325
        set action accept
        set schedule always
        set logtraffic all
        set fsso disable
    end
end
ISDB IDs are release/database dependent. Do not assume an ID has the same meaning across FortiOS versions.
After a firmware upgrade/reboot, FortiGate can perform an automatic ISDB update after startup.
Conceptually:
Firmware Upgrade
       |
       v
FortiGate Reboot
       |
       v
Initial built-in ISDB
       |
       v
Automatic update
       |
       v
Current ISDB
The notes specify an approximate startup update delay of:
~5 minutes
Check:
diagnose autoupdate versions | grep internet -a 6
Air-gapped environments may not permit direct Internet access.
Common examples:
- OT networks
- Industrial environments
- Critical infrastructure
- Isolated data centers
Architecture:
Connected Environment
        |
        | Download license/package
        v
Offline Transfer
        |
        v
Air-Gapped FortiGate
FortiGuard packages such as AV and IPS may need to be manually transferred.
Licensing can also be manually transferred on supported hardware appliances.
According to the supplied training notes, manual licensing for air-gapped environments is supported for supported FortiGate hardware appliances running FortiOS 7.2.0 or later, but not FortiGate VM appliances. Verify current platform support before deployment.
Example:
execute restore manual-license ftp a.lic 192.168.20.200
The exact transfer method and syntax should be checked against the target FortiOS release.
Example:
execute restore ips tftp nids-720-19.261.pkg 192.168.20.200
Then debug update processing:
diagnose debug application updated -1
diagnose debug enable
After troubleshooting:
diagnose debug disable
A FortiGate in a restricted environment might use:
config system fortiguard
    set protocol https
    set port 443
    set load-balance-servers 1
    set auto-join-forticloud disable
    set update-server-location usa
end
The exact configuration depends on the architecture and whether any local update infrastructure exists.
The following options may appear in config system fortiguard depending on FortiOS version:
set update-ffdb enable
set update-uwdb enable
set update-extdb enable
set update-build-proxy enable
Used for FortiGuard/FortiOS database update functionality.
Unified Weight Database.
Can contribute to threat weighting and security intelligence.
Provides extended security database information.
Can be relevant when using an explicit web proxy for supported update workflows.
Version Note: Do not blindly copy all options into production. CLI availability and behavior are FortiOS-release dependent.
Example:
config system fortiguard
    set webfilter-force-off disable
    set webfilter-cache enable
    set webfilter-cache-ttl 3600
    set webfilter-timeout 15
end
Concept:
URL Request
    |
    v
FortiGate
    |
    +---- Cached result ----> Apply rating
    |
    +---- No cache ---------> FortiGuard
                                  |
                                  v
                              Rating result
A consolidated example:
config system fortiguard
    set protocol https
    set port 443
    set load-balance-servers 1
    set update-server-location automatic
    set update-ffdb enable
    set update-uwdb enable
    set update-extdb enable
    set update-build-proxy enable
    set antispam-force-off disable
    set antispam-cache enable
    set antispam-cache-ttl 1800
    set antispam-cache-mpercent 2
    set antispam-timeout 7
    set outbreak-prevention-force-off disable
    set outbreak-prevention-cache enable
    set outbreak-prevention-cache-ttl 300
    set outbreak-prevention-cache-mpercent 2
    set outbreak-prevention-timeout 7
    set webfilter-force-off disable
    set webfilter-cache enable
    set webfilter-cache-ttl 3600
    set webfilter-timeout 15
    set sdns-server-ip 208.91.112.220
    set sdns-server-port 53
    set source-ip 0.0.0.0
end
Important: Treat this as a reference template, not a universal copy/paste configuration.
                    FortiGuard Problem
                           |
                           v
                    Is license valid?
                      /          \
                    NO            YES
                    |              |
              Fix licensing        v
                              DNS resolution?
                              /          \
                            NO            YES
                            |              |
                       Fix DNS             v
                                    Routing working?
                                      /       \
                                    NO         YES
                                    |           |
                               Fix routing      v
                                         Required ports?
                                          /          \
                                        NO            YES
                                        |              |
                                   Allow traffic       v
                                                TLS validation?
                                                 /        \
                                               NO          YES
                                               |             |
                                      Check certificates     v
                                                        Service issue?
                                                         /       \
                                                       YES       NO
                                                       |          |
                                              Debug service      Check
                                              communication     FortiGuard
| Purpose | Command | 
|---|---|
| FortiGuard/service communication | diagnose sys service-communication | 
| Autoupdate debugging | diagnose debug application updated -1 | 
| Enable debug | diagnose debug enable | 
| Disable debug | diagnose debug disable | 
| ISDB entries | diagnose firewall internet-service list | 
| Update versions | diagnose autoupdate versions | 
| IoT daemon debug | diagnose debug application iotd -1 | 
| Device list | diagnose user device list | 
| Disable local CIDB signatures | diagnose cid sigs disable | 
- Default route exists.
- DNS works.
- FortiGate can reach the Internet.
- Required FortiGuard ports are allowed.
- Upstream firewall is not blocking FortiGuard.
- Proxy settings are correct if used.
- FortiGate is registered.
- FortiGuard contract is valid.
- Required subscription exists.
- System time is correct.
- Certificate chain is trusted.
- DNS resolves to the expected service.
- No TLS interception is breaking validation.
- Required TLS versions are supported.
- Automatic update is enabled.
- FortiGuard location is appropriate.
- Update server is reachable.
- AV/IPS package signature validates.
- Update debug has no errors.
- FortiGate can reach FortiManager.
- Update server is configured.
- Rating server is configured.
- Correct port is reachable.
- Fallback behavior is understood.
FortiGuard
   |
   +-- AV Updates
   |
   +-- IPS Updates
   |
   +-- Web Rating
   |
   +-- AntiSpam
   |
   +-- Application Intelligence
   |
   +-- IoT Intelligence
   |
   +-- Threat Intelligence
   |
   +-- Security Databases
   |
   +-- Cloud Services
| Topic | Remember | 
|---|---|
| AV/IPS packages | Digitally signed by Fortinet CA | 
| Anycast | Helps reach an appropriate FortiGuard service location | 
| Web/Spam | Can use communication mechanisms that are not proxy-friendly | 
| FortiManager | Can act as a local FortiGuard update/rating source | 
| Air-Gap | Requires offline/manual update/licensing workflows | 
| IoT Detection | Uses FortiGuard intelligence when local identification is insufficient | 
| ISDB | Built-in database allows FortiGate policies to continue functioning during update transition | 
| Malware statistics | Used to improve Fortinet threat intelligence | 
| TLS | Certificate/trust validation is part of secure communication | 
| Cache | Reduces repeated FortiGuard lookups | 
| Diagnostics | diagnose sys service-communication is a key starting point | 
# FortiGuard configuration
config system fortiguard
    set protocol https
    set port 443
    set update-server-location automatic
end
# Automatic updates
config system autoupdate schedule
    set status enable
    set frequency automatic
end
# Update tunneling
config system autoupdate tunneling
    set address <PROXY-IP>
    set port <PORT>
    set username <USERNAME>
    set password <PASSWORD>
    set status enable
end
# FortiGuard diagnostics
diagnose sys service-communication
# Update debug
diagnose debug application updated -1
diagnose debug enable
# Stop debugging
diagnose debug disable
# IoT diagnostics
diagnose debug application iotd -1
diagnose debug enable
diagnose user device list
# ISDB
diagnose firewall internet-service list
diagnose autoupdate versions | grep internet -a 6
                           ┌─────────────────────┐
                           │    FortiGuard Labs   │
                           │ Threat Intelligence  │
                           └──────────┬──────────┘
                                      |
                              FortiGuard Network
                                      |
                 ┌────────────────────┼────────────────────┐
                 |                    |                    |
              Anycast              Regional           FortiManager
                 |                 Servers              Local FDN
                 |                    |                    |
                 └────────────────────┼────────────────────┘
                                      |
                                  FortiGate
                                      |
       ┌──────────────┬───────────────┼───────────────┬──────────────┐
       |              |               |               |              |
      AV             IPS          Web Filter       AntiSpam         IoT
       |              |               |               |              |
       └──────────────┴───────────────┼───────────────┴──────────────┘
                                      |
                               Security Decision
FortiGuard is not simply an “update server”; it is a distributed security-intelligence ecosystem that provides signatures, ratings, threat intelligence, device intelligence, and security databases through multiple communication models such as Anycast, direct connectivity, proxy-based access, and FortiManager-local services.
FortiGuard · FortiGate FortiGuard · FortiOS FortiGuard · FortiGuard Anycast · FortiGuard troubleshooting · FortiGuard update · FortiManager FortiGuard · FortiGate IoT Detection · FortiGate ISDB · FortiGate air gap · FortiGate offline licensing · FortiGuard proxy · FortiGuard AV IPS update · FortiGuard Labs · NSE4 · NSE7 · FortiGate CLI · Fortinet Security
- 
YouTube — SheynShield 
  - Fortinet NSE content
  - FortiGate troubleshooting
  - Network Security Engineering