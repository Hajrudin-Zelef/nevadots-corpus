---
id: collect-261121-veille-ia/claude-anthropic/cursor-claude-code-extension-backdoor-iocs-and-triage-alpha-cyber
title: "Cursor Claude Code Extension Backdoor: IOCs and Triage | Alpha Cyber"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-10-29"]
keywords: ["cursor", "extension", "microsoft", "code", "persistence", "config", "claude", "tunnel"]
source: https://alpha-cyber.com/cursor-claude-code-extension-ssh-backdoor/
source_anchor: ""
source_lines: [1, 99]
section: "Claude_Anthropic"
---
**Summary finding.**Two tampered builds of the Claude Code extension, versions 2.1.312 and 2.1.313, were served by the Cursor extension marketplace on 29 October 2026. Cursor and Microsoft Threat Intelligence both describe the same loader: it adds an attacker SSH key, tries to open a reverse SSH tunnel to

**134.209.82[.]225**, and installs startup persistence. Cursor reports the tunnel usually failed for lack of admin rights, but the key and the persistence do not need them, and removing the extension removes neither. If anyone in your business used Cursor that day, search for the

`svc-telemetry` key and the persistence files listed below before deciding a machine is clean.
## What happened

Cursor reports that between about 2:27am and 1:35pm Pacific time on 29 October, "a maliciously modified version of Anthropicâs Claude Code extension was made available in the Cursor VS Code Extension Marketplace" (Cursor security notice). The affected builds are `anthropic.claude-code` 2.1.312 and 2.1.313 for macOS and Windows. Cursor removed them, says it audited access, rotated its keys and rebuilt the marketplace.

Both Cursor and Microsoft relay that Anthropic did not build these versions; Microsoft writes that Anthropic confirmed its systems were not compromised. Microsoft also reports that the builds never reached the Visual Studio Marketplace or the npm package `@anthropic-ai/claude-code`. Cursor puts it more cautiously: it has "no evidence" that other versions or the official distribution are affected.

This report is an independent analysis of those two public disclosures plus passive lookups on the published indicators. A.C.R. did not obtain or run the sample.

## What the loader does

Cursor and Microsoft agree on the behaviour. When the extension module loads, injected JavaScript runs before the real extension code. It collects host details (hostname, user, OS, home directory, Git email) and the names, not the values, of environment variables that look like secrets. It then appends an Ed25519 public key labelled `svc-telemetry` to the user's `authorized_keys`, tries to enable the local SSH service, and starts `ssh` or `autossh` with `-N -R '*:0:localhost:22'`, which asks the attacker's server to forward a port back to the workstation's own SSH service.

| Platform | Persistence | Files to look for | 
|---|---|---|
| macOS | User LaunchAgent, RunAtLoad and KeepAlive | `~/Library/LaunchAgents/com.apple.sysmond.plist` ,`~/.config/.smt` ,`~/.config/.smtk` | 
| Linux | User crontab `@reboot` entry | `~/.config/.smt` ,`~/.config/.smtk` | 
| Windows | Scheduled task `SysMon` requesting SYSTEM, Startup-folder fallback | `%USERPROFILE%\.config\smt.cmd` ,`%USERPROFILE%\.config\smtk` ,`Startup\sysmon.cmd` | 
| All | Temporary tunnel identity | `.stk_` followed by ten hex characters | 

Table: persistence artefacts as listed by Microsoft and Cursor. The names imitate monitoring components; Microsoft notes that "names alone are not sufficient to distinguish malicious objects from legitimate monitoring software", so inspect what each one runs.

## Why most installs still count

Cursor reports that enabling the SSH service "requires admin rights, which the malicious extension does not have on most macOS and Windows hosts", so on most machines nobody could log in through the tunnel. Cursor also says it worked with the hosting provider to take the attacker server offline.

Neither point makes a machine clean. Microsoft writes that "the loader may still write keys and persistence artifacts even when tunnel access fails", and both publishers say removing or updating the extension leaves those artefacts in place. We assess with moderate confidence that every machine that loaded either build should be treated as holding an attacker key and a retry loop until checked, because the parts that need no admin rights are exactly the parts that persist. A later SSH server change or an admin session would turn a dormant key into working access.

On secrets: Microsoft says collecting variable names "does not demonstrate theft" of the values, and Cursor adds that it "does not rule out later credential access over the backdoor". On a machine where the tunnel did work, rotate what that user could reach.

## Infrastructure

The only network indicator is 134.209.82[.]225, used for the SSH tunnel and an HTTP beacon on port 8099. Cursor and Microsoft list it independently of each other's analysis, so it is confirmed. Team Cymru places it in DigitalOcean (AS14061, 134.209.80.0/20), a rented cloud host, so block the address, not the range. GreyNoise reports the address scanning as recently as 3 November, after the reported takedown; that is one sensor network, and a cloud address may have been reassigned, so treat it as a lead, not as proof the operator is still active.

VirusTotal flags the address (16 engines) and the modified `extension.js` (37 engines). Its verdicts and the OTX pulses repeat the two disclosures, so they are not counted as further confirmation. VirusTotal had no record of the four packaged VSIX hashes Cursor lists; those stay single-source and are listed in the notes, not the confirmed table.

## Detection

Two rules accompany this report. Both were written from the published descriptions and syntax-checked (YARA with `yarac`, Sigma with pySigma); neither has been run against the sample or recorded events.

The YARA rule matches a file that contains the C2 address plus two of the loader's strings (`svc-telemetry`, `*:0:localhost:22`, `com.apple.sysmond`, `.stk_`, `.config/.smt`). Run it over extension folders, not whole disks. The Sigma rule flags `ssh`, `autossh` or `ssh.exe` started with a remote forward to local port 22 and host-key checking disabled. Administrators who publish a workstation through a jump host can trigger it.

Microsoft publishes Defender advanced hunting queries for the address, the hash and the file names in its analysis, and Cursor publishes check and remediation scripts in its notice.

## What is not known

- Who tampered with the extension. No source names an actor, and Microsoft states its technique mapping does not attribute.
- How the tampered build entered the marketplace. Cursor describes its response but has not published the entry point.
- How many machines installed it, and on how many the tunnel opened.

## Confidence and limitations

High confidence in the behaviour, the C2 address and the `extension.js` hash: each rests on two independent publishers. Moderate confidence in the exposure window and the takedown, which only Cursor can see. Everything here comes from published analysis and passive lookups; no sample was handled.

## Sources

- Cursor: security notice, malicious anthropic.claude-code 2.1.312 and 2.1.313 (29 October, updated 30 October 2026)
- Microsoft Threat Intelligence: Inside a malicious Claude Code-branded extension and its persistent SSH backdoor (30 October 2026)
- Cursor: enterprise security hardening

## Recommendations

1. **Find who used Cursor on 29 October** and whether the extension auto-updated to 2.1.312 or 2.1.313.
2. **Check for the key** : search`~/.ssh/authorized_keys` (and on Windows`administrators_authorized_keys` ) for`svc-telemetry` .
3. **Preserve, then remove** the key, the LaunchAgent, crontab entry or`SysMon` task, the`.smt` /`.smtk` files and any running tunnel process. Uninstalling the extension does not do this.
4. **Rotate credentials** reachable from any machine where the tunnel may have worked, from a clean device.
5. **Block 134.209.82[.]225** and search proxy, firewall and EDR logs for it back to 29 October.
6. **Set a publisher allowlist and an install cooldown** in Cursor for business fleets, so a short-lived malicious release is not installed the hour it appears.

## Detection

Hunting query (Elastic KQL; adapt the field names and baseline against your own data):

## MITRE ATT&CK mapping

T1098.004, T1572, T1053.003 and T1053.005 are Microsoft's mapping. T1195.002, T1082, T1543.001 and T1036.005 are A.C.R.'s, from the behaviour both sources describe. Mapped against MITRE ATT&CK v19.2.

| Tactic | Technique ID | Technique name | Observable | 
|---|---|---|---|
| Initial Access | T1195.002 | Supply Chain Compromise: Compromise Software Supply Chain | Tampered build served through the Cursor marketplace (A.C.R. mapping) | 
| Discovery | T1082 | System Information Discovery | Hostname, user, OS and Git email collected on load | 
| Persistence | T1098.004 | Account Manipulation: SSH Authorized Keys | svc-telemetry key appended (Microsoft) | 
| Persistence | T1053.003 | Scheduled Task/Job: Cron | Linux @reboot entry (Microsoft) | 
| Persistence | T1053.005 | Scheduled Task/Job: Scheduled Task | Windows SysMon task (Microsoft) | 
| Persistence | T1543.001 | Create or Modify System Process: Launch Agent | com.apple.sysmond LaunchAgent (A.C.R. mapping) | 
| Defense Evasion | T1036.005 | Masquerading: Match Legitimate Resource Name or Location | Apple-like and SysMon names (A.C.R. mapping) | 
| Command and Control | T1572 | Protocol Tunneling | Reverse SSH forward to local port 22 (Microsoft) | 

## Indicators of compromise

| Type | Indicator | Note | 
|---|---|---|
| ipv4 | 134.209.82[.]225 | SSH tunnel and HTTP beacon destination (Cursor, Microsoft). DigitalOcean; reported offline since 29 Oct | 
| url | hxxp://134.209.82[.]225:8099/beacon | Host-detail beacon, HTTP POST (Cursor, Microsoft) | 
| sha256 | ecb58cb29078d9646ad1dcdbe51526f2d9cfea06be5fb2ab183e8ecc3ab6bf15 | Modified extension.js in 2.1.312 and 2.1.313 (Cursor, Microsoft) |
