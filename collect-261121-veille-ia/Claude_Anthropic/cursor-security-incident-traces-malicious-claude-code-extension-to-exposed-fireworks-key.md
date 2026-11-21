---
id: collect-261121-veille-ia/claude-anthropic/cursor-security-incident-traces-malicious-claude-code-extension-to-exposed-fireworks-key
title: "Cursor Security Incident Traces Malicious Claude Code Extension to Exposed Fireworks Key"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["cursor", "exposed", "fireworks", "extension", "code", "traces", "marketplace", "security"]
source: https://securityonline.info/cursor-security-incident-fireworks-claude-code/
source_anchor: ""
source_lines: [1, 33]
section: "Claude_Anthropic"
---
## At a Glance

| **Malware Family** | Tampered Anthropic.claude-code extension (versions 2.1.312 and 2.1.313 containing a persistent reverse SSH backdoor and telemetry script) | 
| **Threat Actor** | Unattributed operator (confirmed abuse of an access key exposed at Fireworks AI; suspected link to broader October LLMJacking activity) | 
| **Target / Victims** | Cursor users (fewer than 0.05% had coding agent traces exposed; fewer than 0.01% had API credentials exposed; at most 5,000 users received the tampered extension) | 
| **Delivery Vector** | Direct injection of malicious packages into the Cursor VS Code extension marketplace cache using a stolen internal credential | 
| **Key Capabilities** | Shared storage bucket copying, SaaS lateral movement, marketplace cache poisoning, SSH authorized_keys backdooring, scheduled task and LaunchAgent persistence | 

## TL;DR

On November 13, 2026, AI code editor maker Cursor published findings from a two-week investigation into its October 29 marketplace compromise. The Cursor security incident began when an attacker used an access key exposed at third-party inference vendor Fireworks to copy historical coding agent traces. Using unredacted credentials found in those logs, the Fireworks Cursor security incident escalated into internal SaaS access and planted malicious Claude Code extensions in the marketplace cache.

## Delivery and Attribution

The intrusion originated outside Cursor’s primary network when an attacker obtained a vendor access key that Cursor had issued to Fireworks. Using that exposed key, the intruder copied data from a read-only storage bucket used to share model training traces from 2025 sessions that were not running in Privacy Mode. According to the Cursor security incident update, “A small number of those traces contained credentials that had not been redacted; less than 0.01% of customers had API credentials exposed.”

Overall, historical trace exposure affected fewer than 0.05 percent of users. While confirmed attribution does not name the threat group behind the Cursor security incident, confirmed technical evidence proves the attacker used a stolen internal credential on October 29 to place modified copies of Anthropic.claude-code versions 2.1.312 and 2.1.313 directly into the Cursor marketplace distribution cache. Meanwhile, suspected attribution from external reporting by Aviatrix and Fireworks bulletins links the initial vendor compromise to financially motivated cloud credential thieves. As Cursor noted, “Anthropic did not publish this copy, and this was not a vulnerability in Anthropic’s product. The marketplace source, Open VSX, never had the malware.” At most 5,000 developers received the cached package during the 11-hour window.

## Infection Chain

Starting with the unredacted keys harvested from the shared trace bucket, the attacker pivoted into select SaaS tools used by Cursor. From those platforms, the intruder accessed archived non-production Cursor source code repositories, customer support emails, usage metrics, conversation summaries, and credentials that users had sent to support staff. Next, the attacker extracted an internal credential from that exposed data to write the trojanized Claude Code packages directly into the marketplace cache. When developers installed or updated the cached extension on October 29, embedded JavaScript executed at startup to plant an unauthorized SSH public key and register background persistence tasks on Windows and macOS hosts.

## Command-and-Control and Data-Exfiltration Behavior

Beyond copying the read-only Fireworks bucket and internal SaaS records, the tampered extension builds attempted to establish a persistent reverse SSH tunnel and send host details to a remote command server. However, Cursor worked with the cloud hosting provider to take the attacker’s command server offline on October 29. Cursor also moved all live inference traffic away from Fireworks, revoked the vendor key, rotated all internal credentials, and blocked the last observed attacker activity on November 1. Importantly, live traffic, Privacy Mode prompts, customer code repositories, and official Cursor release signing keys remained untouched.

## Defense and Detection Guidance

Cursor urges developers who installed Anthropic.claude-code 2.1.312 or 2.1.313 during the October 29 window to run Cursor’s remediation scripts rather than relying on a simple extension uninstall. As the advisory warns, “Uninstalling the extension alone may not be enough.” Security teams and admins who received direct breach notifications must rotate any API keys exposed in non-Privacy Mode sessions or support tickets, enable Privacy Mode for sensitive coding workflows, and audit developer workstations for unauthorized SSH keys and persistence tasks.

#### Support Our Threat Intelligence

Find our threat intelligence and malware analysis helpful? Support our work today and unlock a **100% ad-free** reading experience!
