---
id: collect-261121-veille-ia/dev-langages/ca-26-045-cursor-marketplace-claude-code-extension-supply-chain-compromise
title: "CA-26-045: Cursor Marketplace Claude Code Extension Supply Chain Compromise"
domain: dev-langages
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-10-29"]
keywords: ["extension", "cursor", "code", "claude", "marketplace", "persistence", "anthropic", "public"]
source: https://www.deepwatch.com/labs/ca-26-045-cursor-marketplace-claude-code-extension-supply-chain-compromise/
source_anchor: ""
source_lines: [1, 93]
section: "Claude_Anthropic"
---
*Supply Chain | Malicious Extension | SSH Backdoor | Developer Tooling | Claude Code*

**Source Material:** Cursor security notice, Microsoft Security Research technical analysis **| Technology:** Cursor VS Code Extension Marketplace; Anthropic Claude Code extension **| Targeting:** Opportunistic developer workstations using Cursor

#### Executive Summary

On October 29, 2026, malicious modified builds of the Anthropic Claude Code extension, versions 2.1.312 and 2.1.313, were distributed to macOS and Windows users through the Cursor VS Code Extension Marketplace between approximately 2:27 a.m. and 1:35 p.m. Pacific time. The injected code ran when the extension module loaded; no Claude prompt or user interaction was required.

The payload collected host and user metadata, appended an attacker-controlled SSH public key, attempted to enable the local SSH service, created a reverse SSH tunnel, and installed platform-specific persistence that retried about every 60 seconds. The reported collection included environment-variable names, but public analysis does not establish that the corresponding secret values were transmitted by the initial beacon.

Cursor removed the malicious builds, blocked distribution, temporarily shut down and rebuilt its marketplace, rotated marketplace keys, audited other extensions, and coordinated takedown of the attacker server. Microsoft independently reported that Microsoft Visual Studio Marketplace and the npm package @anthropic-ai/claude-code were not affected; Anthropic stated that its systems were not compromised and it did not build or publish the malicious packages.

Customers that installed or updated the extension from Cursor during the exposure window should treat the endpoint as potentially compromised. Identify and isolate affected systems, preserve evidence, remove the unauthorized SSH key and persistence, review follow-on activity, and rotate accessible credentials and sessions from a known-clean device. Updating or uninstalling the extension alone is insufficient because persistence files were written outside the extension directory.

#### Threat Overview and Strategic Impact

This was a software supply-chain compromise of a trusted development-tool channel, not a Claude Code vulnerability. Cursor confirmed that only modified Cursor-marketplace builds of Anthropic.claude-code 2.1.312 and 2.1.313 for macOS and Windows were affected. Because legitimate builds existed in official channels, a version match alone is not proof of compromise; correlate installation source and time, hashes, and payload indicators.

The injected JavaScript ran during module loading. It collected host and user metadata, configured Git email, public IP, and up to 40 environment-variable names matching secret-related terms; public analysis does not show that the corresponding values were sent by the initial beacon. It appended an SSH key labeled svc-telemetry, attempted to enable SSH and create reverse forwarding to 134[.]209[.]82[.]225, and configured macOS or Windows persistence.

Remote access depended on local privileges, SSH configuration, and attacker infrastructure. The server is offline, but keys and persistence may remain. Because developer workstations often hold repository, cloud, signing, and CI/CD access, organizations should investigate confirmed installations as potential initial-access events and review downstream activity. No public source reviewed attributes the incident to a named threat actor.

#### Security Hardening and Recommendations

Identify devices that installed or updated Anthropic.claude-code 2.1.312 or 2.1.313 from Cursor during the exposure window. Before cleanup, preserve the VSIX, extension.js, SSH configuration, persistence files, and relevant endpoint and network telemetry. Isolate hosts with evidence of payload execution.

After reviewing Cursor’s remediation scripts, remove and verify the absence of the unauthorized key, tunnel processes, retry scripts, LaunchAgent, scheduled task, and Startup entry. Install a current clean release, at minimum 2.1.314, from a verified official channel; uninstalling the extension alone does not remove persistence. Rotate accessible credentials and revoke sessions from a known-clean device, and rebuild systems when trust cannot be restored.

Restrict extensions to approved publishers and registries, use an installation cooldown for new releases, monitor extension changes, limit workstation privileges, and alert on unexpected SSH tunneling from code editors.

#### Detection Strategy

As an MDR provider, Deepwatch monitors customer environments for behavioral indicators and post-exploitation artifacts associated with this supply-chain compromise. Because developer extension inventories are maintained locally on endpoints and may not be fully exposed in standard log sources, detection focuses on ingesting and correlating process execution, endpoint persistence, network traffic, and authentication events.

- **Telemetry & Behavioral Detection:** Deepwatch actively monitors ingested EDR, network, and system logs to identify execution leads and command-and-control (C2) activity:
  - **Network & C2 Traffic:** Monitoring for outbound connections to the identified IP over TCP 22 or 8099, as well as HTTP POST requests to /beacon.*(Note: General queries to external IP lookup services such as ifconfig.me or api.ipify.org occur routinely and are not treated as sole indicators of compromise).*
  - **Persistence Mechanisms:** Monitoring for file creation, script execution, or scheduled persistence, including .smt, .smtk, .stk_<10 hex>, macOS LaunchAgent modifications (com.apple.sysmond.plist), Windows Scheduled Tasks (SysMon), and Startup entries (sysmon.cmd).
  - **Unauthorized Remote Access:** Detecting reverse SSH tunnel execution (ssh, autossh), Remote Login enablement (systemsetup -setremotelogin), OpenSSH server installations, or addition of the published SSH key fingerprint.
- **Customer Collaboration & Incident Escalation:**  - **Local Inventory Auditing:** Customers should complement MDR telemetry monitoring by inspecting local developer workstations for Anthropic.claude-code versions 2.1.312 or 2.1.313 installed or updated during the exposure window, as central log ingestion may not reflect quiet software extension installs.
  - **Investigation & Response:** When telemetry triggers an investigative lead, Deepwatch analysts investigate downstream activity, such as code-editor child processes, SSH logins, lateral movement, repository access, and cloud or CI/CD credential usage, and collaborate with your team to execute response procedures.

#### How Deepwatch Protects Our Customers

Deepwatch experts correlate software inventory, endpoint, process, file, network, and identity telemetry within available telemetry. Deepwatch detections can surface suspicious processes, SSH-key modification, reverse tunneling, scheduled-task or LaunchAgent creation, and connections to the reported infrastructure. Customers should confirm data-source coverage and retention for the exposure window.

#### Relevant Detections

Please visit the Guardian Platform to access the relevant detections for this activity and ask NEXA Detection Advisor for a breakdown of which detections are enabled in your environment.

#### Threat Hunting Leads

- Inventory Cursor extensions and identify Anthropic.claude-code 2.1.312 or 2.1.313 installed or updated during the exposure window
- Search extension.js and network telemetry for published indicators (see Technical Artifacts)
- Inspect user and administrator authorized_keys files for the svc-telemetry key and published fingerprint
- Hunt for .smt, .smtk, .stk_<10 hex>, com.apple.sysmond.plist, SysMon, and sysmon.cmd persistence artifacts
- Search process telemetry for reverse SSH forwarding, autossh retry loops, schtasks creation, systemsetup -setremotelogin, and OpenSSH Server installation
- Review SSH authentication, child-process, repository, cloud, and CI/CD activity for follow-on access

#### Technical Artifacts

Please visit the Guardian Platform to access available technical artifacts for this report.

#### Threat Object Mapping

#### **Intrusion Set:**

- Unattributed

#### **Attack Pattern (MITRE ATT&CK/MITRE ATLAS):**

| **Tactic** | **Technique** | **Technique ID** | **Associated Threat Activity** | 
| **Initial Access** | Compromise Software Supply Chain | T1195.002 | Trojanized extension builds were delivered through a trusted marketplace channel. | 
| **Persistence** | SSH Authorized Keys | T1098.004 | Appended an attacker-controlled public key labeled svc-telemetry to authorized_keys. | 
| **Persistence** | Event Triggered Execution: Launch Agent | T1543.001 | Installed macOS LaunchAgent (com.apple.sysmond.plist) to maintain execution across reboots. | 
| **Persistence** | Scheduled Task/Job: Scheduled Task | T1053.005 | Created Windows scheduled task (SysMon) and Startup entry (sysmon.cmd) for persistent retry loops. | 
| **Discovery** | System Information Discovery | T1082 | Injected payload collected host details, username, IP, Git configuration, and environment variable names. | 
| **Command and Control** | Protocol Tunneling | T1572 | Reverse SSH forwarding exposed the workstation’s local SSH service through attacker infrastructure. | 

#### **Vulnerabilities:**

- N/A. This event involved maliciously modified marketplace packages rather than exploitation of a disclosed software vulnerability.

#### **Malware/Tool:**

- Trojanized Anthropic.claude-code extension; built-in ssh or autossh; PowerShell; schtasks; macOS launchctl and systemsetup.

#### Additional Sources

- Cursor security notice
- Microsoft Security Research technical analysis
- Anthropic Claude Code v2.1.314 release notes

Share
