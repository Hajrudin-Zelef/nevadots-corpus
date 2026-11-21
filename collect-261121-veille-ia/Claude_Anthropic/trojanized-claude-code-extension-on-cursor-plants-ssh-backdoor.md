---
id: collect-261121-veille-ia/claude-anthropic/trojanized-claude-code-extension-on-cursor-plants-ssh-backdoor
title: "Trojanized Claude Code Extension on Cursor Plants SSH Backdoor"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-11-15"]
keywords: ["cursor", "extension", "code", "claude", "marketplace", "version", "october", "svc-telemetry"]
source: https://blog.ostorlab.co/claude-code-cursor-extension-ssh-backdoor.html
source_anchor: ""
source_lines: [1, 190]
section: "Claude_Anthropic"
---
## TL;DR

- Tampered Claude Code extension copies 2.1.312 and 2.1.313 were served through Cursor's marketplace for about 11 hours on October 29, 2026. Cursor states at most 5,000 users installed them.
- The build carries a 66-line loader that adds an SSH key to `authorized_keys` , tries to enable SSH, opens a reverse tunnel and installs persistence.
- Cursor's November 13 report traces the breach to a Fireworks AI key and says the last attacker activity was blocked on November 1. No CVE is assigned, and it is not in CISA KEV as of November 15, 2026.
- If you installed or updated Claude Code from Cursor's marketplace on October 29, run the `svc-telemetry` check and follow the removal steps. Uninstalling the extension alone is not enough.

## Am I affected?

All of these must hold:

- You use the Cursor IDE, not VS Code, and you installed or updated the Claude Code extension from Cursor's extension marketplace.
- The install or update happened between about 2:27 a.m. and 1:35 p.m. Pacific Time on October 29, 2026, when the malicious version was available.
- The extension came from Cursor's marketplace and the installed version is 2.1.312 or 2.1.313. Cursor installs extensions automatically when updates are enabled, so the version matters more than a memory of choosing to install. The same version numbers from Anthropic's official channels or Open VSX are not affected.

You are not affected through any of these channels, which OpenSourceMalware confirmed clean:

- The `@anthropic-ai/claude-code` npm package.
- The Claude Code extension on Open VSX.
- The Claude Code extension on the Microsoft Visual Studio Code Marketplace.

Version 2.1.313 was never an official Anthropic release. Version 2.1.312 is an official release, but the copy Cursor served from its cache on October 29 was tampered with. Anthropic's published releases jump from 2.1.312 on October 27, 2026 to 2.1.314 on October 30, 2026, so the malicious 2.1.313 number existed only in Cursor's marketplace.

Check whether the payload ran on your machine. The loader's most reliable artifact is the SSH key it appends. On macOS and Linux:

```
grep -qs svc-telemetry ~/.ssh/authorized_keys && echo "svc-telemetry key found" || echo "No key found; machine not impacted"
```
On Windows, run in PowerShell:

```
$files = "$HOME\.ssh\authorized_keys", "C:\ProgramData\ssh\administrators_authorized_keys" | Where-Object { Test-Path $_ }
if ($files -and (Select-String svc-telemetry $files)) { "svc-telemetry key found" } else { "No key found; machine not impacted" }
```
These checks come from Cursor's security notice. A missing key does not prove the extension never ran; it proves the backdoor's key step did not complete. The indicators in "How do I check if I was compromised?" cover the rest.

## How do I fix it?

The malicious extension is gone from the marketplace, so the fix is removal and verification.

1. In Cursor, uninstall the Claude Code extension and restart the editor. If you still need Claude Code in Cursor, reinstall from the rebuilt marketplace and confirm the version is 2.1.314 or later.
2. Run the `svc-telemetry` check above. If it finds the key, continue with "What should I do if the check finds the key?" before anything else.
3. Run Cursor's remediation script for your platform: `remediate-macos.sh` as the affected user on macOS, or`remediate-windows.ps1` from an administrator PowerShell on Windows. The scripts remove the extension, the persistence mechanisms, the tunnel processes and the attacker's key, and exit non-zero if anything remains.
4. Re-run the `svc-telemetry` check until it reports no key.

Anthropic published version 2.1.314 to Open VSX and the Microsoft VS Code Marketplace on October 30, 2026, so extensions from those two marketplaces update to a clean build normally.

## What does the malicious code do?

The only modified file is `extension.js`. OpenSourceMalware found a 66-line immediately invoked function inserted before the legitimate extension bundle, so the malicious code runs when the extension module loads: on VS Code startup and when the extension's webview opens. No package lifecycle script is involved; loading the module is enough.

The loader runs the same chain on macOS, Linux and Windows:

1. **Reconnaissance.** It collects the hostname, local username, operating system, CPU architecture, home directory, current working directory, the global Git email from`git config --global user.email` , and the names of up to 40 environment variables containing the strings KEY, TOKEN, SECRET, PASS, AWS or GCP. Values are not sent; the names still reveal which cloud providers and credential types the machine has.
2. **Authorized-key installation.** It creates`~/.ssh` with mode 0700, appends an Ed25519 public key with the comment`svc-telemetry` to`~/.ssh/authorized_keys` , and sets the file to mode 0600.
3. **SSH service enablement.** It tries to turn on Remote Login on macOS with`sudo -n systemsetup -setremotelogin on` , start`sshd` on Linux through`systemctl` , and install and start OpenSSH Server on Windows through PowerShell.
4. **Reverse SSH tunnel.** It decodes an embedded OpenSSH private key into a temporary file named`.stk_<identifier>` , then repeatedly connects as`tunnel@134.209.82[.]225` with the forwarding request`-N -R '*:0:localhost:22'` , asking the attacker's server to allocate a port that forwards to the victim's SSH service. Host-key verification is disabled, and the connection retries every 60 seconds, preferring`autossh` when available.
5. **Persistence.** On macOS it writes`~/.config/.smt` and`~/.config/.smtk` plus a LaunchAgent named`com.apple.sysmond.plist` with`RunAtLoad` and`KeepAlive` . On Linux it writes the same two files under`~/.config` and adds the tunnel script to the user's crontab with an`@reboot` entry. On Windows it creates a scheduled task named`SysMon` that tries to run as SYSTEM, falling back to a Startup-folder script.
6. **Beacon.** After 1.5 seconds it looks up the public IP with`curl ifconfig.me` and`curl https://api.ipify.org` and POSTs the collected details as JSON to`http://134.209.82[.]225:8099/beacon` over unencrypted HTTP, with a second POST to`http://127.0.0.1:8099/beacon` . The response is discarded.

Nearly every exception is suppressed, so failures leave no visible sign and the legitimate extension keeps loading. The injected stage downloads nothing further; OpenSourceMalware found no second-stage downloader, only the IP lookups, the beacon and the interactive tunnel.

## How did the trojanized update reach users?

The tampered builds were served through the Cursor VS Code extension marketplace under the Claude Code extension's legitimate identity, `anthropic.claude-code`, as versions 2.1.312 and 2.1.313. Because the copies carried the real publisher identity, users and auto-update received them as ordinary updates rather than a new extension. The tampered versions were available for about 11 hours, from about 2:27 a.m. to 1:35 p.m. Pacific Time, between two official Anthropic releases.

Cursor's incident report of November 13, 2026 traces the breach to an access key Cursor had issued to Fireworks AI. Fireworks exposed the key, and the attacker used it to copy data from a read-only bucket that held historical coding-agent traces, mostly from 2025, from sessions not in Privacy Mode. Fewer than 0.01% of customers had API credentials in those traces, and fewer than 0.05% of users had traces exposed. With those credentials the attacker reached select SaaS tools and obtained a credential that let it place the tampered extension copies in the marketplace cache.

Cursor states it removed the copies, rebuilt the marketplace, rotated credentials, moved inference off Fireworks and blocked the last attacker activity on November 1. It says the incident is contained and that its production systems, the Cursor app and the other extensions were not affected. The attacker server at 134.209.82[.]225 no longer functions, so the reverse tunnel and beacon endpoints are offline.

## How does the attack flow?

One sentence: the attacker minted a trojanized 2.1.313 that Cursor's marketplace served as a normal update, and the prepended loader turned every editor restart into a persistent SSH route back to the attacker's server.

2.1.313"] --> B["Cursor marketplace

serves it as an update"] B --> C["VS Code loads

extension.js"] C --> D["Prepended 66-line

loader runs first"] D --> E["svc-telemetry key

added to authorized_keys"] D --> F["Reverse SSH tunnel

to 134.209.82[.]225"] D --> G["Persistence: LaunchAgent,

cron, scheduled task"] D --> H["Beacon with host

details to :8099"]

## What can an attacker do?

The chain's goal is persistent remote access over SSH. Its full reach has limits, and Cursor's own analysis states them plainly:

- On most macOS and Windows hosts the SSH-enablement step failed. Turning on the local SSH service requires admin rights, and Cursor states that outside "highly unusual circumstances where Cursor was started with administrator privileges", errors in the malicious code made this step fail. Without the local SSH service listening, the tunnel had no port 22 to reach, so the operator could not have logged in through it on those hosts.
- The key installation, persistence and beacon still land on those hosts, and the machine details still reach the attacker. Cursor tells affected users to follow the remediation steps regardless.
- On hosts where the SSH service did start, an operator holding the matching private key could connect through the reverse tunnel and run commands as the victim's user account, which is how OpenSourceMalware describes the tunnel's purpose.
- The beacon carries host details and environment-variable names, not values. OpenSourceMalware notes that successful SSH access would let the operator inspect the account directly, so the beacon understates rather than overstates the access.

No follow-on payloads were observed by either analysis.

## What should I do if the check finds the key?

Treat the machine as compromised and respond in this order, combining Cursor's and OpenSourceMalware's guidance:

1. Disconnect the machine from the network and isolate it. Block `134.209.82[.]225` on ports 22 and 8099 at your controls.
2. Preserve forensic evidence before cleanup: the dropped files, the `authorized_keys` line, the LaunchAgent or scheduled task, and any process telemetry. OpenSourceMalware advises collecting evidence first because the observable files may not represent the full compromise.
3. Remove the extension, the persistence mechanisms and any running `ssh` or`autossh` tunnel processes, using Cursor's remediation scripts or by hand from the indicators below.
4. Rotate credentials and sessions accessible from that account, from a known-clean system: SSH keys, Git hosts, package registries, cloud consoles, password stores and browser sessions.
5. Assess whether to rebuild. OpenSourceMalware writes that rebuilding the host is safer than assuming that removing the known artifacts restores trust, because the tunnel created an interactive path into the account.

## How do I check if I was compromised?

Beyond the `svc-telemetry` key check, search endpoints, network logs and extension-install telemetry for the published indicators. All of them come from Cursor's security notice and OpenSourceMalware's analysis.

Files and persistence to search for:

- `~/.ssh/authorized_keys` containing the comment`svc-telemetry`
- `~/.config/.smt` and`~/.config/.smtk` (macOS, Linux), or`%USERPROFILE%\.config\smt.cmd` and`smtk` (Windows)
- `$TMPDIR` or`%TEMP%` files matching`.stk_<10 hex>` or`.nc<PID>.js`
- macOS LaunchAgent `~/Library/LaunchAgents/com.apple.sysmond.plist` with label`com.apple.sysmond`
- Windows scheduled task named `SysMon` , or`sysmon.cmd` in the Startup folder
- An extension folder named `anthropic.claude-code-2.1.313-darwin-arm64` or`anthropic.claude-code-2.1.313-win32-x64` whose`extension.js` contains`134.209.82.225` and`svc-telemetry` These folder names come from the 2.1.313 build analyzed on October 30; a tampered 2.1.312 copy sits in a 2.1.312 folder, so the`svc-telemetry` key check is the authoritative test for both versions.

Network indicators:

- Any connection to `134.209.82[.]225` , especially port 22 and port 8099
- HTTP POSTs to `/beacon` , including loopback attempts to`127.0.0.1:8099`
- Lookups to `ifconfig.me` and`api.ipify.org` are legitimate services; treat them as context, not proof

Process command lines to hunt:

- `ssh -N -R '*:0:localhost:22'` or`autossh -M 0`
- SSH options `StrictHostKeyChecking=no` with`UserKnownHostsFile=/dev/null`
- `schtasks /create /tn SysMon` ,`systemsetup -setremotelogin on` , or`Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0`

Package hashes for endpoint scans: the modified VSIX files hash to `432ccdee3801f99e52b8fe31f69ea9870118e96ea7ef9ecabed1479621b5ab62` (darwin-arm64) and `3de1722901165769893bef57c72594e9a54459faa23f3091b136b79966edd081` (win32-x64), and the malicious `extension.js` to `ecb58cb29078d9646ad1dcdbe51526f2d9cfea06be5fb2ab183e8ecc3ab6bf15`.

A hit on any single indicator is a lead, not proof of the full chain. An attempt whose steps all failed can still have left the beacon and the key.

## Timeline

- 2026-10-27: Anthropic publishes Claude Code extension 2.1.312, the last official release before the incident.
- 2026-10-29: Between about 2:27 a.m. and 1:35 p.m. Pacific Time, tampered 2.1.312 and 2.1.313 copies are available in the Cursor extension marketplace. Cursor later states at most 5,000 users were affected.
- 2026-10-29: Cursor identifies the malicious version, removes it, blocks distribution and shuts the VS Code marketplace down as a precaution, then rebuilds it after rotating keys and auditing access.
- 2026-10-29: Cursor works with the hosting provider to take the attacker server at 134.209.82[.]225 offline.
- 2026-10-30: Anthropic publishes Claude Code extension 2.1.314 to Open VSX and the Microsoft VS Code Marketplace.
- 2026-10-30: OpenSourceMalware publishes its technical analysis and indicators of compromise; Cursor's security notice with check and remediation scripts is live.
- 2026-11-01: The last attacker activity is observed and blocked, per Cursor's incident report.
- 2026-11-13: Cursor publishes its incident report, tracing the breach to the Fireworks AI key.

## FAQ

### Was the npm package @anthropic-ai/claude-code malicious?

No. OpenSourceMalware checked the npm package, the Open VSX extension and the Microsoft VS Code Marketplace extension and found all three clean. Only the cached 2.1.312 and 2.1.313 copies in Cursor's marketplace carried the loader. Version 2.1.313 never existed in Anthropic's official releases, and the 2.1.312 copy from official channels is clean.

### Could attackers actually log in through the backdoor?

On most hosts, no. Cursor states that turning on the local SSH service requires admin rights and that errors in the malicious code made the step fail unless Cursor itself ran with administrator privileges. Without a listening SSH service, the reverse tunnel had nothing to forward to. The key, persistence and beacon were still installed, and Cursor tells affected users to remediate regardless.

### How did the attacker get the marketplace credential?

Cursor's incident report says an access key Cursor issued to Fireworks AI was exposed by Fireworks. The attacker used it to read a bucket of historical coding-agent traces, found unredacted API credentials in fewer than 0.01% of customers' traces, and used those credentials to reach select SaaS tools. A credential obtained there allowed the marketplace cache to be poisoned.

### Did anyone get the malicious version automatically?

Yes, that is the likely path for most of the affected users. OpenSourceMalware's Jenn Gile noted that anyone with Claude Code set to auto-update could have consumed the malware, because marketplaces deliver updates under the publisher's identity. Cursor is reaching out to impacted users directly.

### Is the attacker still reachable?

No. Cursor states the attacker server no longer functions: the company worked with the cloud provider hosting it to take it offline, so the reverse SSH tunnel and the host-detail beacon POST are not operational. The key and persistence on an affected machine remain until removed.

### Do I need to rebuild an affected machine?

Assess it. OpenSourceMalware writes that rebuilding is safer than assuming that removing the known artifacts restores trust, because the tunnel provided an operator-controlled route to the SSH service and follow-on activity would not appear in the static artifact. At minimum, remove the key and persistence, rotate reachable credentials and review authentication, SSH and process logs.

### Who found and disclosed this?

Dean Hardcastle spotted the malicious update as it happened and alerted OpenSourceMalware, whose researchers Paul McCarty and Jenn Gile published the analysis on October 30, 2026. Cursor published its own security notice with indicators, a check and remediation scripts. The Hacker News reported the incident on October 30, 2026, correcting an early version of its story that had named Open VSX as the compromised registry.

## References

- Cursor incident report, "An update on our recent security incident", November 13, 2026, with the Fireworks AI key, the trace scope, the containment date and what was not affected: https://cursor.com/blog/security-incident-update (checked November 15, 2026)
- Cursor security notice, "Security notice", on the malicious version of Anthropic.claude-code (2.1.313) in the Cursor Extension Marketplace, October 29, 2026, with the affected window, the 5,000-user figure, the failure analysis of the SSH-enablement step, the check, the remediation scripts and the indicators: https://cursor.com/blog/claude-code-extension-security-notice (checked October 30, 2026)
- OpenSourceMalware analysis, "Trojanized Claude Code Extension Plants an SSH Backdoor", October 30, 2026, with the 66-line loader breakdown, the beacon, tunnel and persistence mechanics, and the hashes: https://opensourcemalware.com/blog/trojanized-claude-code-extension-plants-persistent-ssh-backdoor (checked October 30, 2026)
- Cursor's statement on the incident, posted October 29, 2026: https://x.com/cursor_ai/status/2116074289053175808 (checked October 30, 2026)
- Anthropic's Claude Code releases, where 2.1.314 was published on October 30, 2026 and 2.1.313 does not exist: https://github.com/anthropics/claude-code/releases (checked October 30, 2026)
- The Hacker News, "Official Claude Code Extension on Cursor Marketplace Trojanized with Persistent SSH Backdoor", October 30, 2026, reporting that the method used to introduce the update remains unclear: https://thehackernews.com/2026/10/official-claude-code-vs-code-extension.html (checked October 30, 2026)
- The @anthropic-ai/claude-code npm package page, which shows 12,837,062 downloads in the week of October 22 to 28, 2026, for the scale of the clean package the extension shares a name with: https://www.npmjs.com/package/@anthropic-ai/claude-code (checked October 30, 2026)
