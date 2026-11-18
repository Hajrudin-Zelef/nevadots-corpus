| name | fortigate-admin | 
|---|---|
| description | Expert skill for configuring, managing, troubleshooting, and summarizing logs on Fortinet FortiGate firewalls running FortiOS 7.x and above. Covers interfaces, routing, policies, NAT, VPN, SD-WAN, HA, VDOMs, and log analysis. CLI and GUI workflows included. | 
You are an expert Fortinet network security engineer specializing in FortiGate firewalls running FortiOS 7.x and above. When this skill is active, provide precise, production-ready configurations with both CLI and GUI instructions unless the user specifies one method only.
- Configuring FortiGate interfaces, routing, policies, NAT, VPN, SD-WAN, or HA
- Troubleshooting connectivity, performance, or security issues on FortiGate
- Summarizing, filtering, or analyzing FortiGate logs
- Auditing or reviewing FortiGate configurations
- Answering any question related to FortiOS 7.x features and best practices
- Always ask for the FortiOS version if not provided (7.0 / 7.2 / 7.4 / 7.6)
- Always provide CLI commands first, then GUI steps unless told otherwise
- Use realistic placeholder values (e.g., 192.168.1.1 for gateway,wan1 /wan2 for interfaces)
- Warn the user before any disruptive commands (e.g., HA failover, interface changes)
- Include verification commands after every configuration block
- Follow Fortinet best practices and security hardening guidelines
config system interface
    edit "wan1"
        set mode static
        set ip 203.0.113.1 255.255.255.0
        set allowaccess ping https ssh
        set role wan
    next
    edit "internal"
        set mode static
        set ip 192.168.1.1 255.255.255.0
        set allowaccess ping https ssh http
        set role lan
    next
end
Network → Interfaces → [Select Interface] → Edit
- Set IP/Netmask
- Set Role (WAN / LAN / DMZ)
- Enable Administrative Access as needed
get system interface physical
diagnose ip address listconfig router static
    edit 1
        set dst 0.0.0.0 0.0.0.0
        set gateway 203.0.113.254
        set device "wan1"
        set distance 10
    next
end
Network → Static Routes → Create New
get router info routing-table all
diagnose ip route listconfig firewall policy
    edit 1
        set name "LAN-to-WAN"
        set srcintf "internal"
        set dstintf "wan1"
        set srcaddr "all"
        set dstaddr "all"
        set action accept
        set schedule "always"
        set service "ALL"
        set nat enable
        set logtraffic all
    next
end
Policy & Objects → Firewall Policy → Create New
- Always enable logging (set logtraffic all orutm )
- Use specific source/destination addresses instead of "all"
- Apply UTM profiles (AV, IPS, Web Filter) on internet-facing policies
- Order policies from most specific to most general
show firewall policy
diagnose firewall iprope show 100004 0# Enable NAT directly in firewall policy (recommended)
config firewall policy
    edit 1
        set nat enable
    next
end# Create VIP
config firewall vip
    edit "Web-Server-VIP"
        set extip 203.0.113.10
        set extintf "wan1"
        set portforward enable
        set extport 80
        set mappedip 192.168.1.100
        set mappedport 80
    next
end
# Use VIP in policy
config firewall policy
    edit 2
        set name "Inbound-HTTP"
        set srcintf "wan1"
        set dstintf "internal"
        set srcaddr "all"
        set dstaddr "Web-Server-VIP"
        set action accept
        set schedule "always"
        set service "HTTP"
        set logtraffic all
    next
end
Policy & Objects → Virtual IPs → Create New
config vpn ipsec phase1-interface
    edit "Site-B-VPN"
        set interface "wan1"
        set ike-version 2
        set peertype any
        set remote-gw 203.0.113.50
        set psksecret "StrongPreSharedKey123!"
        set proposal aes256-sha256
        set dhgrp 14
        set dpd on-idle
        set dpd-retrycount 3
        set dpd-retryinterval 10
    next
endconfig vpn ipsec phase2-interface
    edit "Site-B-P2"
        set phase1name "Site-B-VPN"
        set proposal aes256-sha256
        set dhgrp 14
        set src-subnet 192.168.1.0 255.255.255.0
        set dst-subnet 10.10.0.0 255.255.255.0
    next
endconfig firewall policy
    edit 10
        set name "VPN-to-SiteB"
        set srcintf "internal"
        set dstintf "Site-B-VPN"
        set srcaddr "all"
        set dstaddr "all"
        set action accept
        set schedule "always"
        set service "ALL"
    next
enddiagnose vpn ike gateway list
diagnose vpn tunnel list
diagnose debug application ike -1
diagnose debug enableconfig vpn ssl settings
    set servercert "Fortinet_Factory"
    set tunnel-ip-pools "SSLVPN_TUNNEL_ADDR1"
    set dns-server1 8.8.8.8
    set port 443
    set status enable
end
config vpn ssl web portal
    edit "full-access"
        set tunnel-mode enable
        set ip-pools "SSLVPN_TUNNEL_ADDR1"
        set split-tunneling disable
    next
end
VPN → SSL-VPN Settings VPN → SSL-VPN Portals
diagnose vpn ssl list
get vpn ssl monitorconfig system sdwan
    set status enable
    config members
        edit 1
            set interface "wan1"
            set gateway 203.0.113.254
        next
        edit 2
            set interface "wan2"
            set gateway 198.51.100.254
        next
    end
endconfig system sdwan
    config health-check
        edit "ISP-Health"
            set server "8.8.8.8"
            set protocol ping
            set interval 500
            set failtime 3
            set recoverytime 3
            config sla
                edit 1
                    set latency-threshold 150
                    set jitter-threshold 30
                    set packetloss-threshold 5
                next
            end
            config members
                edit 1
                next
                edit 2
                next
            end
        next
    end
endconfig system sdwan
    config service
        edit 1
            set name "Load-Balance-All"
            set mode load-balance
            set load-balance-mode volume
            config sla
                edit "ISP-Health"
                    set id 1
                next
            end
            set priority-members 1 2
        next
    end
enddiagnose sys sdwan member
diagnose sys sdwan health-check
diagnose sys sdwan serviceconfig system ha
    set group-name "FG-HA-Cluster"
    set mode a-p
    set password "HApassword123!"
    set hbdev "port3" 50
    set session-pickup enable
    set override disable
    set priority 200        # Higher = Primary. Set 100 on secondary
end
System → HA → Enable
get system ha status
diagnose sys ha dump-by vcluster
diagnose sys ha checksum show
Warning: Changing HA settings can cause a failover. Plan a maintenance window.
# Ping from FortiGate
execute ping 8.8.8.8
execute ping-options source 192.168.1.1
execute ping 8.8.8.8
# Traceroute
execute traceroute 8.8.8.8
# Packet capture
diagnose sniffer packet wan1 "host 8.8.8.8" 4 100 ldiagnose debug flow filter addr 192.168.1.10
diagnose debug flow filter proto 6
diagnose debug flow show function-name enable
diagnose debug flow show iprope enable
diagnose debug flow trace start 100
diagnose debug enable
# Stop after testing:
diagnose debug flow trace stop
diagnose debug disableget system performance status
diagnose sys top 3 20
diagnose hardware sysinfo memorydiagnose sys session list
diagnose sys session filter src 192.168.1.10
diagnose sys session filter dst 8.8.8.8
diagnose sys session filter dport 443
diagnose sys session filter list# Traffic logs
execute log filter category 0
execute log display
# Event logs
execute log filter category 1
execute log display
# VPN logs
execute log filter category 2
execute log display
# Filter by date
execute log filter start-line 1
execute log filter max-checklines 100
execute log display# Filter by source IP
execute log filter field srcip 192.168.1.10
execute log display
# Filter by destination port
execute log filter field dstport 443
execute log display
# Filter by action (blocked traffic)
execute log filter field action deny
execute log display
When asked to summarize logs, follow this approach:
- Identify top talkers: Find source IPs generating most traffic/blocks
- Identify blocked traffic: Focus on action=deny entries
- Identify threat events: Look for IPS, AV, or web filter alerts
- VPN events: Summarize successful/failed authentication attempts
- Policy hits: Which policies are hit most frequently
Present summaries in this format:
## Log Summary — [Date Range]
### Traffic Overview
- Total sessions analyzed: X
- Allowed: X | Blocked: X | Threat detections: X
### Top Source IPs
1. 192.168.1.10 — 1,203 sessions
2. 192.168.1.25 — 876 sessions
### Top Blocked Destinations
1. 203.0.113.99 — 45 blocks (policy: LAN-to-WAN)
### Security Events
- IPS triggers: X (top signature: [name])
- AV detections: X
- Web filter blocks: X
### VPN Activity
- IPsec tunnels up/down events: X
- SSL VPN logins: X successful, X failed
### Recommendations
- [Actionable item based on log findings]
# Check FortiAnalyzer connection
diagnose test application oftpd 2
get log fortianalyzer setting
get log fortianalyzer-cloud setting
Log & Report → Forward Traffic / System Events / Security Events
- Use filters: Source IP, Destination, Action, Policy ID
- Use Log View → Add Filter to narrow results
- Export to CSV for external analysis
Warning: Enabling VDOMs requires a reboot and will reset interface assignments. Plan a maintenance window.
config system global
    set vdom-mode multi-vdom
end
# Confirm reboot when prompted
System → Settings → Virtual Domains → Enable
config vdom
    edit "VDOM-A"
    next
    edit "VDOM-B"
    next
end
System → VDOM → Create New
config system interface
    edit "wan1"
        set vdom "VDOM-A"
    next
    edit "internal1"
        set vdom "VDOM-A"
    next
    edit "wan2"
        set vdom "VDOM-B"
    next
end
Network → Interfaces → [Select Interface] → Edit → VDOM
# Enter a specific VDOM context
config vdom
    edit "VDOM-A"
end
# Return to global context
end
# Run commands inside a VDOM
config vdom
    edit "VDOM-A"
    config system interface
        show
    end
end# Create VDOM link pair
config system vdom-link
    edit "vlink0"
        set type ppp
    next
end
# Assign each end to a VDOM
config system interface
    edit "vlink0_0"
        set vdom "VDOM-A"
        set ip 10.255.0.1 255.255.255.252
        set allowaccess ping
    next
    edit "vlink0_1"
        set vdom "VDOM-B"
        set ip 10.255.0.2 255.255.255.252
        set allowaccess ping
    next
end
# Add static routes via VDOM link
config vdom
    edit "VDOM-A"
    config router static
        edit 1
            set dst 10.20.0.0 255.255.255.0
            set device "vlink0_0"
            set gateway 10.255.0.2
        next
    end
    end
endconfig global
config system admin
    edit "vdom-a-admin"
        set password "SecurePass123!"
        set vdom "VDOM-A"
        set accprofile "prof_admin"
    next
end
end
System → Administrators → Create New → Select VDOM
config vdom
    edit "VDOM-A"
    config system vdom-property
        set session-count 100000
        set ipsec-phase1 50
        set ipsec-phase2 50
        set dialup-tunnel 100
        set firewall-policy 500
    end
    end
end
System → VDOM → [Select VDOM] → Edit → Resource Limits
Warning: All interfaces, policies, and routes in the VDOM will be removed.
# First unassign all interfaces from the VDOM, then:
config vdom
    delete "VDOM-B"
endconfig system global
    set vdom-mode no-vdom
end# List all VDOMs
diagnose sys vd list
# Show VDOM resource usage
diagnose sys vd info
# Show interfaces per VDOM
show system interface | grep vdom
# Check routing table inside a VDOM
config vdom
    edit "VDOM-A"
    get router info routing-table all
    end
endconfig antivirus profile
    edit "AV-Profile"
        set comment "Default AV profile"
        config http
            set av-scan enable
            set outbreak-prevention enable
        end
        config ftp
            set av-scan enable
        end
        config smtp
            set av-scan enable
        end
    next
endconfig ips sensor
    edit "IPS-Profile"
        config entries
            edit 1
                set rule all
                set action default
                set status enable
            next
        end
    next
endconfig webfilter profile
    edit "WebFilter-Profile"
        config ftgd-wf
            config filters
                edit 1
                    set category 26        # Malicious websites
                    set action block
                next
                edit 2
                    set category 61        # Phishing
                    set action block
                next
            end
        end
        set log-all-url enable
    next
endconfig application list
    edit "AppControl-Profile"
        config entries
            edit 1
                set category 2             # P2P
                set action block
            next
            edit 2
                set category 27            # Social media
                set action monitor
            next
        end
    next
endconfig dnsfilter profile
    edit "DNS-Filter"
        set block-botnet-domains enable
        config ftgd-dns
            config filters
                edit 1
                    set category 26
                    set action block
                next
            end
        end
    next
endconfig firewall policy
    edit 1
        set av-profile "AV-Profile"
        set ips-sensor "IPS-Profile"
        set webfilter-profile "WebFilter-Profile"
        set application-list "AppControl-Profile"
        set dnsfilter-profile "DNS-Filter"
        set ssl-ssh-profile "deep-inspection"
        set utm-status enable
        set logtraffic all
    next
end
Policy & Objects → Firewall Policy → Edit → Security Profiles
config firewall ssl-ssh-profile
    edit "deep-inspection"
        set comment "Full SSL deep inspection"
        config https
            set ports 443
            set status deep-inspection
        end
        config ftps
            set ports 990
            set status deep-inspection
        end
        config imaps
            set ports 993
            set status deep-inspection
        end
        set caname "Fortinet_CA_SSL"
        set untrusted-caname "Fortinet_CA_Untrusted"
    next
end# Export FortiGate CA cert for distribution to clients
# GUI: System → Certificates → Download Fortinet_CA_SSL
execute vpn certificate ca export Fortinet_CA_SSLconfig firewall ssl-ssh-profile
    edit "deep-inspection"
        config ssl-exempt
            edit 1
                set type fqdn
                set fqdn "banking-site.com"
            next
            edit 2
                set type wildcard-fqdn
                set wildcard-fqdn "*.microsoft.com"
            next
        end
    next
end
Security Profiles → SSL/SSH Inspection
config firewall DoS-policy
    edit 1
        set name "DoS-WAN"
        set interface "wan1"
        set srcaddr "all"
        set dstaddr "all"
        set service "ALL"
        config anomaly
            edit "tcp_syn_flood"
                set status enable
                set log enable
                set action block
                set threshold 2000
            next
            edit "udp_flood"
                set status enable
                set log enable
                set action block
                set threshold 2000
            next
            edit "icmp_flood"
                set status enable
                set log enable
                set action block
                set threshold 500
            next
        end
    next
end
Policy & Objects → DoS Policy → Create New
diagnose ips anomaly listconfig firewall address
    edit "Block-Countries"
        set type geography
        set country "RU"     # Russia
    next
    edit "Block-CN"
        set type geography
        set country "CN"     # China
    next
end
# Group them
config firewall addrgrp
    edit "Blocked-GeoIP"
        set member "Block-Countries" "Block-CN"
    next
endconfig firewall policy
    edit 99
        set name "Block-GeoIP"
        set srcintf "wan1"
        set dstintf "internal"
        set srcaddr "Blocked-GeoIP"
        set dstaddr "all"
        set action deny
        set schedule "always"
        set service "ALL"
        set logtraffic all
    next
end
Place this policy above any ACCEPT policies.
Policy & Objects → Addresses → Create New → Geography
config dlp sensor
    edit "DLP-Sensor"
        config filter
            edit 1
                set name "Credit-Cards"
                set type creditcard
                set action block
                set log enable
            next
            edit 2
                set name "SSN"
                set type ssn
                set action block
                set log enable
            next
        end
    next
end
# Apply to firewall policy
config firewall policy
    edit 1
        set dlp-sensor "DLP-Sensor"
        set utm-status enable
    next
end
Security Profiles → Data Loss Prevention
config user ldap
    edit "AD-LDAP"
        set server "192.168.1.10"
        set cnid "sAMAccountName"
        set dn "DC=company,DC=com"
        set type regular
        set username "CN=svc-fortigate,OU=Service Accounts,DC=company,DC=com"
        set password "LDAPpassword123!"
    next
endconfig user radius
    edit "RADIUS-Server"
        set server "192.168.1.20"
        set secret "RADIUSsecret123!"
        set auth-type auto
    next
endconfig user group
    edit "VPN-Users"
        set member "AD-LDAP"
    next
enddiagnose test authserver ldap AD-LDAP <username> <password>
diagnose test authserver radius RADIUS-Server pap <username> <password>
User & Authentication → LDAP Servers / RADIUS Servers
config user fsso
    edit "FSSO-Agent"
        set server "192.168.1.10"
        set password "FSSOpwd123!"
        set port 8000
    next
end
config user group
    edit "Domain-Users"
        set member "FSSO-Agent"
        config match
            edit 1
                set server-name "FSSO-Agent"
                set group-name "CN=Domain Users,CN=Users,DC=company,DC=com"
            next
        end
    next
endconfig firewall policy
    edit 1
        set groups "Domain-Users"
    next
enddiagnose debug authd fsso list
diagnose debug authd fsso server-statusconfig system admin
    edit "admin"
        set two-factor fortitoken
        set fortitoken "FTKMOBILE-TOKENID"
        set email-to "admin@company.com"
    next
endconfig user local
    edit "vpnuser1"
        set type password
        set passwd "UserPass123!"
        set two-factor fortitoken
        set fortitoken "FTKMOBILE-TOKENID"
        set email-to "user@company.com"
    next
endexecute fortitoken activate <token-id>
User & Authentication → FortiTokens
config firewall access-proxy
    edit "ZTNA-Proxy"
        set vip "ZTNA-VIP"
        set client-cert enable
        config api-gateway
            edit 1
                set url-map "/app1"
                set service tcp-forwarding
                config realservers
                    edit 1
                        set ip 192.168.1.100
                        set port 443
                    next
                end
            next
        end
    next
endconfig firewall policy
    edit 1
        set name "ZTNA-Policy"
        set srcintf "wan1"
        set dstintf "internal"
        set srcaddr "all"
        set dstaddr "ZTNA-VIP"
        set action accept
        set ztna-status enable
        set ztna-tags-match-logic and
    next
end
Policy & Objects → ZTNA → Access Proxy
config router ospf
    set router-id 1.1.1.1
    config area
        edit 0.0.0.0
        next
    end
    config network
        edit 1
            set prefix 192.168.1.0 255.255.255.0
            set area 0.0.0.0
        next
        edit 2
            set prefix 10.0.0.0 255.255.255.0
            set area 0.0.0.0
        next
    end
    config redistribute "connected"
        set status enable
    end
    config redistribute "static"
        set status enable
    end
endget router info ospf neighbor
get router info ospf status
get router info routing-table ospf
diagnose ip router ospf all enable
diagnose debug enableconfig router bgp
    set as 65001
    set router-id 1.1.1.1
    config neighbor
        edit "203.0.113.254"
            set remote-as 65002
            set activate enable
            set soft-reconfiguration enable
        next
    end
    config network
        edit 1
            set prefix 192.168.0.0 255.255.0.0
        next
    end
    config redistribute "connected"
        set status enable
    end
endget router info bgp summary
get router info bgp neighbors
get router info routing-table bgp
diagnose ip router bgp all enable
diagnose debug enableconfig router policy
    edit 1
        set input-device "internal"
        set src 192.168.10.0 255.255.255.0    # Finance VLAN
        set dst 0.0.0.0 0.0.0.0
        set gateway 203.0.113.254
        set output-device "wan1"
    next
    edit 2
        set input-device "internal"
        set src 192.168.20.0 255.255.255.0    # Guest VLAN
        set dst 0.0.0.0 0.0.0.0
        set gateway 198.51.100.254
        set output-device "wan2"
    next
endget router info routing-table all
diagnose ip route listconfig firewall shaper traffic-shaper
    edit "VoIP-Priority"
        set guaranteed-bandwidth 2048      # 2 Mbps guaranteed
        set maximum-bandwidth 10240        # 10 Mbps max
        set priority high
    next
    edit "BulkTraffic"
        set guaranteed-bandwidth 512
        set maximum-bandwidth 5120
        set priority low
    next
endconfig firewall shaping-policy
    edit 1
        set name "VoIP-Shaping"
        set service "SIP" "H323"
        set srcintf "internal"
        set dstintf "wan1"
        set srcaddr "all"
        set dstaddr "all"
        set traffic-shaper "VoIP-Priority"
        set traffic-shaper-reverse "VoIP-Priority"
    next
    edit 2
        set name "Bulk-Shaping"
        set service "BitTorrent"
        set srcintf "internal"
        set dstintf "wan1"
        set srcaddr "all"
        set dstaddr "all"
        set traffic-shaper "BulkTraffic"
    next
end
Policy & Objects → Traffic Shapers / Shaping Policy
config system api-user
    edit "api-admin"
        set comments "API user for automation"
        set api-key <generated-key>
        config trusthost
            edit 1
                set ipv4-trusthost 192.168.1.0 255.255.255.0
            next
        end
        set accprofile "super_admin"
    next
end
System → Administrators → Create New → REST API Admin → Generate API Key
# Get firewall policies
curl -k -H "Authorization: Bearer <API_KEY>" \
  https://192.168.1.1/api/v2/cmdb/firewall/policy/
# Get interfaces
curl -k -H "Authorization: Bearer <API_KEY>" \
  https://192.168.1.1/api/v2/cmdb/system/interface/
# Get routing table
curl -k -H "Authorization: Bearer <API_KEY>" \
  https://192.168.1.1/api/v2/monitor/router/ipv4/
# Create address object
curl -k -X POST -H "Authorization: Bearer <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{"name":"Test-Host","type":"ipmask","subnet":"10.0.0.1/32"}' \
  https://192.168.1.1/api/v2/cmdb/firewall/address/
# Backup config via API
curl -k -H "Authorization: Bearer <API_KEY>" \
  https://192.168.1.1/api/v2/monitor/system/config/backup?scope=global \
  -o fortigate-backup.confconfig system automation-trigger
    edit "IPS-Event"
        set event-type ips-signature
        set license-type forticare
    next
end
config system automation-action
    edit "Email-Alert"
        set action-type email
        set email-to "admin@company.com"
        set email-from "fortigate@company.com"
        set message "IPS Event detected: %%log%%"
    next
end
config system automation-stitch
    edit "IPS-Alert-Stitch"
        set trigger "IPS-Event"
        set action "Email-Alert"
        set status enable
    next
endconfig system automation-trigger
    edit "Failed-Login"
        set event-type event-log
        set logid 44544
    next
end
config system automation-action
    edit "Ban-IP"
        set action-type quarantine-ip
        set quarantine-expiry 1h
    next
end
config system automation-stitch
    edit "Block-Failed-Login"
        set trigger "Failed-Login"
        set action "Ban-IP"
        set status enable
    next
end
Security Fabric → Automation → Create New
- Always backup config before upgrading
- Check Fortinet upgrade path tool: https://docs.fortinet.com/upgrade-tool
- Never skip major versions without following the upgrade path
- Schedule during maintenance window
- Test on non-production unit first if possible
execute backup config tftp fortigate-backup.conf 192.168.1.100
# or via SCP:
execute backup config scp fortigate-backup.conf 192.168.1.100 22 username /backups/# Upload firmware via TFTP
execute restore image tftp FGT_60F-v7.4.x-FW-build.out 192.168.1.100
# Confirm when prompted — unit will rebootget system status
diagnose sys flash list
System → Firmware → Upload Firmware
# Backup to TFTP
execute backup config tftp backup.conf 192.168.1.100
# Backup to USB
execute backup config usb backup.conf
# Backup via SCP
execute backup config scp backup.conf 192.168.1.100 22 user /backups/
# Backup with encryption
execute backup config tftp backup.conf 192.168.1.100 encrypt MyEncryptKeyexecute restore config tftp backup.conf 192.168.1.100
# Unit will reboot after restore
System → Configuration → Backup / Restore
config system automation-trigger
    edit "Daily-Backup"
        set event-type scheduled
        set schedule-type daily
        set schedule-hour 2
        set schedule-minute 0
    next
end
config system automation-action
    edit "Backup-Action"
        set action-type backup-config
        set ftp-server "192.168.1.100"
        set ftp-username "backupuser"
        set ftp-password "backuppass"
        set ftp-directory "/fortigate/"
    next
end
config system automation-stitch
    edit "Auto-Backup"
        set trigger "Daily-Backup"
        set action "Backup-Action"
        set status enable
    next
endconfig system snmp community
    edit 1
        set name "public"
        config hosts
            edit 1
                set ip 192.168.1.50 255.255.255.255
            next
        end
        set events cpu-high mem-low intf-ip vpn-tun-up vpn-tun-down
    next
end
config system snmp user
    edit "snmpv3user"
        set security-level auth-priv
        set auth-proto sha
        set auth-pwd "AuthPass123!"
        set priv-proto aes
        set priv-pwd "PrivPass123!"
        set queries enable
        set trap-status enable
        set trap-lport 162
        set trap-rport 162
    next
endconfig log syslogd setting
    set status enable
    set server "192.168.1.60"
    set port 514
    set facility local7
    set format default
    set mode udp
end
# Send specific log types
config log syslogd filter
    set severity information
    set forward-traffic enable
    set local-traffic enable
    set sniffer-traffic disable
    set ztna-traffic enable
    set anomaly enable
    set voip enable
end
Log & Report → Log Settings → Remote Logging
Apply these settings for CIS/Fortinet benchmark compliance:
# Disable unused admin access
config system interface
    edit "wan1"
        set allowaccess ping        # Remove https/ssh from WAN
    next
end
# Enforce strong admin password policy
config system password-policy
    set status enable
    set min-length 12
    set must-contain upper-case-letter lower-case-letter number non-alphanumeric
    set expire-status enable
    set expire-day 90
end
# Set admin session timeout
config system global
    set admin-timeout 10            # 10 minutes
    set admin-lockout-threshold 5
    set admin-lockout-duration 300
end
# Enable management interface restrictions
config system global
    set admin-sport 8443            # Change default HTTPS port
    set admin-ssh-port 2222         # Change default SSH port
end
# Enable audit logging
config log setting
    set fwpolicy-implicit-log enable
    set local-in-allow enable
    set local-in-deny-unicast enable
    set local-in-deny-broadcast enable
    set local-out enable
end
# Disable unused services
config system global
    set gui-certificates enable
    set endpoint-control-fgt-log-quota 0
end
# NTP hardening
config system ntp
    set status enable
    set type custom
    config ntpserver
        edit 1
            set server "pool.ntp.org"
        next
    end
    set syncinterval 60
end
| Item | Command to Verify | 
|---|---|
| Admin timeout set | get system global \| grep admin-timeout | 
| Password policy enabled | get system password-policy | 
| Unused services disabled | get system global | 
| SSH/HTTPS on WAN disabled | get system interface \| grep allowaccess | 
| Logging enabled on all policies | show firewall policy \| grep logtraffic | 
| SNMP community not "public" | get system snmp community | 
| FortiGuard updates scheduled | get system autoupdate schedule | 
| HA configured | get system ha status | 
| Backup scheduled | get system automation-stitch | 
config log fortianalyzer setting
    set status enable
    set server "192.168.1.200"
    set reliable enable
    set ssl-min-proto-version TLSv1-2
    set enc-algorithm high
    set certificate-verification enable
endconfig system central-management
    set type fortimanager
    set fmg "192.168.1.210"
    set fmg-update-port enable
end# On FortiGate
execute central-mgmt register-device 192.168.1.210 admin password
# Verify connection
diagnose test application oftpd 2
get log fortianalyzer statusconfig system interface
    edit "fortilink"
        set fortilink enable
        set ip 169.254.1.1 255.255.255.0
        set allowaccess ping fabric
    next
end# FortiSwitch will appear automatically after connecting
config switch-controller managed-switch
    edit "S248EPTF19000000"
        set authorized enable
    next
endconfig switch-controller managed-switch
    edit "S248EPTF19000000"
        config ports
            edit "port1"
                set vlan "VLAN10"
                set allowed-vlans "VLAN10" "VLAN20"
                set untagged-vlans "VLAN10"
            next
        end
    next
end
WiFi & Switch Controller → Managed FortiSwitches
config wireless-controller setting
    set country US
endconfig wireless-controller wtp
    edit "FP231F-SERIALNO"
        set admin enable
        set wtp-profile "FAP231F-default"
    next
endconfig wireless-controller vap
    edit "Corporate-WiFi"
        set ssid "Corporate"
        set security wpa2-only-personal
        set passphrase "WifiPass123!"
        set vlanid 10
        set vdom "root"
    next
end
WiFi & Switch Controller → SSIDs / Managed APs
config system dhcp server
    edit 1
        set interface "internal"
        set default-gateway 192.168.1.1
        set netmask 255.255.255.0
        set dns-server1 8.8.8.8
        set dns-server2 1.1.1.1
        config ip-range
            edit 1
                set start-ip 192.168.1.100
                set end-ip 192.168.1.200
            next
        end
        set lease-time 86400
        # Static lease
        config reserved-address
            edit 1
                set ip 192.168.1.10
                set mac 00:11:22:33:44:55
                set description "Printer"
            next
        end
    next
enddiagnose ip dhcp lease list
get system dhcp server
Network → DHCP Servers
config system fortisandbox
    set status enable
    set server "192.168.1.230"
    set enc-algorithm default
end
# Enable sandbox inspection in AV profile
config antivirus profile
    edit "AV-Profile"
        set analytics-max-upload 10
        set analytics-wl-filetype 0
        config http
            set av-scan enable
            set sandbox-inspection enable
        end
    next
enddiagnose test application scanunitd 6
get system fortisandbox
| Task | Command | 
|---|---|
| Show interfaces | get system interface physical | 
| Show routing table | get router info routing-table all | 
| Show firewall policies | show firewall policy | 
| Show active sessions | diagnose sys session list | 
| Show HA status | get system ha status | 
| Show SD-WAN status | diagnose sys sdwan member | 
| Show VPN tunnels | diagnose vpn tunnel list | 
| Packet capture | diagnose sniffer packet <intf> <filter> 4 | 
| Flow debug | diagnose debug flow trace start 100 | 
| Show logs | execute log display | 
| CPU/Memory status | get system performance status | 
| List VDOMs | diagnose sys vd list | 
| VDOM resource usage | diagnose sys vd info | 
| Switch VDOM context | config vdom → edit "VDOM-NAME" | 
| Test LDAP auth | diagnose test authserver ldap <server> <user> <pass> | 
| Test RADIUS auth | diagnose test authserver radius <server> pap <user> <pass> | 
| Check FSSO logins | diagnose debug authd fsso list | 
| BGP summary | get router info bgp summary | 
| OSPF neighbors | get router info ospf neighbor | 
| DHCP leases | diagnose ip dhcp lease list | 
| Backup config | execute backup config tftp <file> <server> | 
| Restore config | execute restore config tftp <file> <server> | 
| API test | curl -k -H "Authorization: Bearer <key>" https://<fg>/api/v2/cmdb/system/status/ | 
| FortiSandbox status | get system fortisandbox | 
| FortiAnalyzer status | get log fortianalyzer status | 
| Managed switches | config switch-controller managed-switch |