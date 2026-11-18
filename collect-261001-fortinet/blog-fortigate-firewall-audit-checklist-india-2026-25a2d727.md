FortiGate Firewall Audit Checklist: 10 Misconfigurations Indian Enterprises Must Fix in 2026
According to Gartner, 99% of firewall breaches are caused by misconfiguration, not firewall flaws — a prediction that has proven accurate year after year. For the 700,000+ FortiGate appliances deployed globally, this statistic is not theoretical — it is an operational emergency. In April 2026, Fortinet disclosed that attackers were retaining read-only access to patched FortiGate devices via symbolic links planted in the SSL VPN language folder, surviving even firmware upgrades. Earlier, the “Belsen Group” leaked 15,000 FortiGate configurations on the dark web — exposing VPN credentials, firewall rules, and private keys from devices compromised through known vulnerabilities that were never patched. If your FortiGate has not been audited in the last 90 days, assume it is at risk.
CERT-In now mandates annual cybersecurity audits for regulated entities effective July 2026. SEBI’s CSCRF framework requires quarterly firewall reviews for stock brokers and market infrastructure institutions from April 2026. This is no longer optional — firewall audits are a compliance requirement across Indian enterprise.
Why FortiGate Firewall Audits Matter More Than Ever
Firewalls are the single most critical control in your network security architecture. A properly configured FortiGate blocks lateral movement, segments trust zones, inspects encrypted traffic, and enforces zero-trust policies. A misconfigured one is worse than no firewall at all — it gives a false sense of security while leaving critical gaps that attackers exploit in minutes.
The threat landscape for FortiGate devices has intensified dramatically since 2023. Multiple critical vulnerabilities — several with CVSS scores above 9.0 — have been actively exploited in the wild. Sophisticated threat actors, including state-sponsored groups, are specifically targeting FortiGate appliances because they sit at the network perimeter and often have VPN services exposed to the internet.
But patching alone is not enough. The April 2026 symlink persistence technique proved that attackers can survive firmware upgrades if the underlying configuration allows it. A comprehensive audit goes beyond patch status — it examines every policy, every admin account, every exposed service, and every logging configuration to ensure the device is hardened according to industry benchmarks.
Ogma’s Firewall Compliance Audit service was built for exactly this challenge. Our NSE7-certified engineers assess your FortiGate against CIS benchmarks, CERT-In requirements, and real-world attack patterns — not just checkbox compliance, but operational security posture.
5 Recent FortiGate CVEs Every CISO Should Know
These are not theoretical vulnerabilities. Every one of these has been actively exploited in the wild, with confirmed breaches in enterprises across Asia, Europe, and North America. If your FortiGate was internet-facing during any of these disclosure windows and you did not patch within days, your device may have been compromised.
| CVE | Type | CVSS | Disclosed | Impact | 
|---|---|---|---|---|
| CVE-2023-27997 | Heap-based buffer overflow (SSL VPN) | 9.8 | June 2023 | Remote code execution via crafted SSL VPN requests. Pre-authentication — no credentials needed. | 
| CVE-2024-21762 | Out-of-bounds write (SSL VPN) | 9.6 | Feb 2024 | RCE via specially crafted HTTP requests to SSL VPN. Exploited by Chinese state-sponsored actors; CISA added to KEV catalogue. | 
| CVE-2024-47575 | Missing authentication (FortiManager) | 9.8 | Oct 2024 | Known as “FortiJump”. Unauthenticated attackers execute arbitrary code on FortiManager, potentially compromising all managed FortiGates. | 
| CVE-2024-55591 | Authentication bypass via websocket | 9.6 | Jan 2026 | Attackers gain super-admin access via crafted websocket requests to Node.js module. Exploited by initial access brokers selling access to ransomware groups. | 
| Symlink Persistence | Post-exploitation persistence technique | N/A | Apr 2026 | Attackers create symbolic links in SSL VPN language folders linking to the root filesystem. Survives firmware upgrades — attackers retain read-only access to configs even after patching. | 
Belsen Group Data Leak (January 2026): A threat actor published configuration files from 15,000 FortiGate devices on the dark web — including plaintext VPN credentials, firewall policies, and private certificates. These were harvested from devices compromised through CVE-2022-40684 (authentication bypass) that were never remediated. If your FortiGate was running FortiOS 7.0.x–7.2.x in late 2022 without patching, your configuration may be in that dump.
10 Critical FortiGate Misconfigurations and How to Fix Them
These are the ten most common and dangerous misconfigurations we find during FortiGate audits across Indian enterprises. Each one is a confirmed attack vector — not a theoretical risk.
1. Default Admin Account with No Password
CRITICAL
      FortiGate ships with a default admin account and no password. If this account is not secured immediately after deployment, anyone who can reach the management interface has full super-admin access. We find this in ~15% of audits, especially on branch office devices.
    
config system admin
  edit "admin"
    set password <strong-password>
  next
end
    2. HTTP/Telnet Management on WAN Interface
CRITICAL
Enabling HTTP or Telnet admin access on the WAN interface exposes credentials in cleartext. Attackers passively sniff traffic or brute-force the login. Management access should be HTTPS-only on internal interfaces with trusted host restrictions.
config system interface
  edit "wan1"
    set allowaccess ping https ssh
  next
end
    3. No Trusted Host Restriction for Admin Accounts
HIGH
Without trusted host configuration, admin accounts can log in from any source IP — including the internet. Trusted hosts restrict management access to specific subnets, making brute-force and credential-stuffing attacks from external IPs impossible.
config system admin
  edit "admin"
    set trusthost1 10.10.10.0 255.255.255.0
    set trusthost2 192.168.1.0 255.255.255.0
  next
end
    4. No Two-Factor Authentication for Admin Logins
HIGH
Single-factor admin authentication means a stolen or guessed password gives complete control of the firewall. FortiToken (hardware or mobile) adds a second factor that prevents credential-based attacks even if the password is compromised.
config system admin
  edit "admin"
    set two-factor fortitoken
    set fortitoken <serial-number>
  next
end
    5. SSL VPN Unpatched or Unnecessarily Exposed
CRITICAL
SSL VPN has been the attack surface for CVE-2023-27997, CVE-2024-21762, and the symlink persistence technique. If you are not actively using SSL VPN, disable it entirely. If you need it, ensure FortiOS is current and restrict portal access to specific source IPs or use ZTNA as a replacement.
# Disable SSL VPN if not needed:
config vpn ssl settings
  set status disable
end
# Or restrict source IPs:
config firewall address
  edit "SSLVPN-Allowed-IPs"
    set subnet 203.0.113.0 255.255.255.0
  next
end
    6. Overly Permissive Any/Any/Any Rules
HIGH
Policies with source=any, destination=any, service=any bypass the entire purpose of a firewall. These are often created during initial deployment as “temporary” rules and never removed. They allow unrestricted lateral movement and data exfiltration.
# Identify overly permissive rules:
diagnose firewall policy list | grep -i "all"
# Replace with specific src/dst/service policies
# Enable UTM inspection on every allow rule
    7. Unused, Shadow, and Redundant Policies
MEDIUM
Over time, firewall policies accumulate. Unused policies (zero hit counts) create confusion, shadow policies (never matched because a broader rule above catches the traffic first) mask intent, and redundant policies slow down processing. A clean policy table is essential for security and performance.
# Check policy hit counts:
diagnose firewall policy list
# Delete policies with zero hits after review:
config firewall policy
  delete <policy-id>
end
    8. FortiGuard Subscriptions Expired
HIGH
When FortiGuard subscriptions expire, IPS signatures stop updating, AV definitions go stale, web filtering categories freeze, and application control loses visibility into new apps. Your FortiGate effectively becomes a stateful packet filter — blind to modern threats. We find expired subscriptions in ~30% of audits.
# Check subscription status:
diagnose autoupdate status
diagnose autoupdate versions
# Verify FortiGuard connectivity:
exec ping guard.fortinet.com
exec telnet guard.fortinet.com 443
    9. Logging Disabled or Memory-Only
MEDIUM
Memory-only logging means logs are lost on reboot — exactly when you need them most (during an incident). CERT-In requires 180 days of log retention. Without syslog/FortiAnalyzer forwarding and disk logging, incident response and forensics are impossible, and you fail every compliance audit.
config log disk setting
  set status enable
  set maximum-log-age 180
end
config log fortianalyzer setting
  set status enable
  set server <FortiAnalyzer-IP>
end
    10. Firmware Not Updated or Running EOL FortiOS
CRITICAL
Running end-of-life FortiOS versions (6.0, 6.2, 6.4) means you receive no security patches — period. Known CVEs remain permanently exploitable. We routinely find devices running FortiOS 6.0.x in production, sometimes with internet-facing SSL VPN. The upgrade path exists; the risk of not upgrading is existential.
# Check current firmware:
get system status
# Upgrade path (always use Fortinet's tool):
# https://docs.fortinet.com/upgrade-tool
# FortiOS 6.4 → 7.0 → 7.2 → 7.4 → 7.6
    CIS FortiGate Benchmark — Top 10 Controls
The Center for Internet Security (CIS) publishes vendor-specific security benchmarks — consensus-based configuration guidelines developed by cybersecurity practitioners, government agencies, and technology vendors. The CIS FortiGate Benchmark is the gold standard for FortiOS hardening and is referenced by auditors worldwide.
Current versions: CIS FortiGate 7.0.x Benchmark v1.2.0 and CIS FortiGate 7.4.x Benchmark v1.0.0. Each contains 50+ individual controls across categories including management plane, data plane, logging, authentication, and network configuration.
- Disable HTTP and Telnet on all interfaces — Management access must use HTTPS and SSH only (CIS 1.1)
- Configure trusted hosts for all admin accounts — Restrict management access to known admin subnets (CIS 1.2)
- Set idle timeout to 5 minutes or less — Prevent session hijacking from unattended terminals (CIS 1.3)
- Enable two-factor authentication for all admins — FortiToken or RADIUS with MFA (CIS 1.4)
- Encrypt admin password storage using strong hashing — Use set admin-password-hash sha256 (CIS 1.5)
- Configure NTP with authentication — Accurate timestamps are essential for log correlation and forensics (CIS 2.1)
- Enable logging to external syslog or FortiAnalyzer — Memory-only logging is insufficient for incident response (CIS 3.1)
- Enable DNS filtering on all outbound policies — Block C2 callbacks and malicious domain resolution (CIS 4.1)
- Disable unused interfaces and services — Reduce attack surface by shutting down ports not in use (CIS 5.1)
- Configure firmware auto-update notifications — Ensure visibility into available security patches (CIS 6.1)
Ogma’s Firewall Audit benchmarks your FortiGate against the full CIS profile — not just the top 10. Our report maps each finding to the specific CIS control ID, severity level, and provides exact CLI remediation commands. Book a Firewall Compliance Audit.
India Compliance Landscape for Firewall Audits
Indian enterprises face an increasingly stringent compliance environment where firewall audits are no longer a best practice — they are a legal requirement. Here is how each regulatory framework applies to your FortiGate:
CERT-In (Indian Computer Emergency Response Team)
- 6-hour incident reporting to CERT-In for any cybersecurity incident
- 180-day log retention within Indian jurisdiction
- Annual cybersecurity audit by a CERT-In format auditors (mandatory from July 2026)
- Audit must cover firewall configurations, access controls, and vulnerability management
RBI IS Audit Framework (Banking & NBFCs)
- Annual IS audit mandated for all scheduled commercial banks and NBFCs
- Firewall rule review is a specific audit checklist item
- Must verify no any/any rules, proper segmentation, and logging
- Board-level reporting of audit findings required
SEBI CSCRF (Capital Markets)
- Effective April 2026 for stock brokers, depository participants, and MIIs
- Quarterly firewall rule review mandated
- Network segmentation between trading and back-office systems required
- Audit trail of all firewall policy changes
DPDPA 2023 & IT Act Section 43A
- Digital Personal Data Protection Act 2023 — significant data fiduciaries must implement “reasonable security safeguards”
- IT Act Section 43A — bodies corporate handling sensitive personal data must maintain “reasonable security practices”
- A misconfigured firewall that leads to a data breach exposes the organisation to penalties under both frameworks
- Regular firewall audits serve as evidence of due diligence
How Ogma’s FortiGate Audit Works
Ogma is an authorised Fortinet partner with NSE7-certified engineers, and our Firewall Compliance Audit is backed by the full depth of our security operations platform — 1,000+ vulnerability assessments, 256 breach-and-attack simulations, and 4.1 million+ threat intelligence indicators.
Configuration Export
Secure export of FortiGate configuration via read-only admin or direct console access. No changes made to the device.
CIS Benchmark Analysis
Automated parsing against every CIS FortiGate Benchmark control. Each setting scored as Pass, Fail, or Not Applicable.
Policy Optimisation Review
Identify unused, shadow, and overly permissive policies. Analyse hit counts, rule ordering, and service exposure.
Vulnerability Correlation
Cross-reference FortiOS version and enabled services against known CVEs and CISA KEV catalogue. Flag any unpatched critical vulnerabilities.
Compliance Mapping
Map findings to CERT-In, RBI, SEBI CSCRF, PCI DSS, and ISO 27001 requirements. Generate compliance-ready documentation.
Remediation Report & Hardening
Detailed report with severity-ranked findings, exact CLI remediation commands, and optional hands-on hardening by our NSE7 engineers.
Contact our team to schedule a FortiGate audit or request portal access for automated compliance scanning. You can also reach us directly at hello@ogma.in.
Get Your Free FortiGate Security Audit
Ogma’s NSE7 engineers will audit your FortiGate against CIS benchmarks and Indian compliance requirements — no cost, no obligation.
Book Your Free Audit
Frequently Asked Questions
Stay ahead of cyber threats
One short email a week — curated Indian cybersecurity news, Fortinet releases, DPDPA updates. No fluff.