---
id: collect-261121-veille-ia/claude-anthropic/the-claude-code-extension-backdoor-in-cursor-what-happened-and-the-one-check-to-run
title: "The Claude Code extension backdoor in Cursor: what happened, and the one check to run"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["extension", "cursor", "code", "cursors", "marketplace", "claude", "happened", "check"]
source: https://medium.com/@circuitbay/the-claude-code-extension-backdoor-in-cursor-what-happened-and-the-one-check-to-run-87b10ac804a0
source_anchor: ""
source_lines: [1, 52]
section: "Claude_Anthropic"
---
# The Claude Code extension backdoor in Cursor: what happened, and the one check to run

On October 29 a tampered copy of the Claude Code extension sat in Cursor’s extension marketplace for about eleven hours. Cursor says at most 5,000 of its users received it. The extension looked like the real one in every way that mattered to a user, and it auto-updated into place. This is what happened, what it did on a developer’s machine, why the damage was smaller than it could have been, and the one check worth running today.

Cursor’s security notice says a maliciously modified version of Anthropic’s Claude Code extension was available in the Cursor VS Code Extension Marketplace between about 2:27am and 1:35pm Pacific time on October 29, which is 09:27 to 20:35 UTC. It names versions 2.1.312 and 2.1.313 for macOS and Windows. [1]

StepSecurity’s analysis describes version 2.1.313, published to the Open-VSX registry and mirrored into Cursor’s marketplace, reaching up to 5,000 Cursor users in that window. It found the build identical to the official extension except for a small loader embedded in its extension.js file, about 7 KB larger than the previous release. A clean 2.1.314 was published at 01:07 UTC on October 30. [2]

The two accounts differ on one detail: StepSecurity checked the 2.1.312 build on Open-VSX and found it clean, while Cursor lists 2.1.312 among the bad versions in its own marketplace. If you use Cursor and had either version, treat yourself as in scope.

The first headlines said Anthropic had been hacked. Anthropic disputes that. Its Felix Rieseberg said this was not a compromise of Anthropic’s systems or employee laptops [2], and Anthropic’s statement, quoted by DoublePulsar, reads: “A malicious actor used access to Cursor’s marketplace to publish a tampered extension targeting Cursor’s users, we were not involved with building, publishing, or hosting this.” [3]

Cursor’s notice says it removed the version on detection, then took the whole marketplace offline, audited access, found no other tampered extensions, rotated its keys and rebuilt the marketplace before bringing it back. [1] The security firm Phorion was the first to notice the malicious activity on endpoints. [3][4]

- It collected host and user details: hostname, username, operating system, home directory, working directories, Git email, public IP, and the names of environment variables. Only the names, not the values, so the operator learned which tokens and cloud keys existed on the machine without reading them. [1][4]
- It appended an attacker’s SSH public key, labelled svc-telemetry, to ~/.ssh/authorized_keys. [1][2]
- It tried to turn on the local SSH service and open a reverse SSH tunnel to a server at 134.209.82.225, so the operator could reach the machine from outside. [2][4]
- It installed persistence so the tunnel would come back after a reboot: a LaunchAgent named com.apple.sysmond on macOS, a scheduled task called SysMon with a Startup-folder fallback on Windows, and a cron entry that runs at every reboot on Linux. Background scripts retried the tunnel about every 60 seconds. [1][2][4]
- Phorion found a bug in the Linux crontab step that can wipe a user’s existing cron jobs when it runs again. [4]

Turning on the SSH service needs administrator rights. Cursor says that, outside the unusual case where Cursor itself was started with administrator privileges, errors in the malicious code made that step fail, so the attacker could not log in through the tunnel. Phorion describes the same failure on macOS. [1][4]

Cursor also worked with the hosting provider to take the attacker’s server offline, so the tunnel and the reporting endpoint no longer function. [1]

That is good news, not an all-clear. On most affected machines the attacker’s key and the persistence scripts were still written to disk, and the environment-variable inventory was still sent while the server was up. Cursor’s own words: the limited collection “does not rule out later credential access over the backdoor”. [1]

On macOS or Linux:

grep svc-telemetry ~/.ssh/authorized_keys

If it prints nothing, the key is not there. If it prints a line, the machine was hit. Cursor’s notice has the equivalent PowerShell check for Windows, which also looks in the administrators’ authorized_keys file under ProgramData. [1]

Uninstalling the extension does not undo this. The loader drops standalone persistence that keeps running after the extension is gone. [1][2]

StepSecurity’s order of operations, condensed: [2]

1. Keep evidence, then remove the persistence: the com.apple.sysmond LaunchAgent, the SysMon task or the sysmon.cmd file in the Windows Startup folder, any cron line that runs at reboot, ~/.config/.smt, .smtk and smt.cmd, and the .stk_ key files in your temp folder. Cursor publishes remediate-macos.sh and remediate-windows.ps1 that do this and exit non-zero if anything remains. [1]
2. Remove the bad version and install 2.1.314 or later.
3. Rotate everything reachable from that machine, from a different, clean machine: GitHub and GitLab tokens, CI tokens, cloud keys, SSH keys and package-registry tokens. The beacon told the operator which of those existed.
4. If there is any sign the tunnel actually connected, rebuild the machine rather than clean it.

- A mirror copies whatever it is given. One publish upstream reached every downstream consumer of the registry within the same window. [2]
- Auto-update turned a marketplace problem into a fleet problem in hours. Most people never saw a prompt.
- An extension runs as you. Whatever your user can read, it can read: SSH keys, tokens in environment files, cloud credentials.
- The damage was limited by one ordinary habit: not running the editor as an administrator. That is the cheapest control in this story, and it worked.

1. Cursor, security notice on the Claude Code extension, 2026–10–29, updated 2026–10–30 — cursor.com/blog/claude-code-extension-security-notice
2. Rohan Prabhu, StepSecurity, “Trojanized Claude Code Extension Delivered Persistent SSH Backdoor to Developer Machines”, 2026–10–30 — stepsecurity.io/blog/claude-code-extension-trojanized
3. Kevin Beaumont, DoublePulsar, “Anthropic hacked via Cursor to deliver remote access trojan to Claude developer laptops worldwide”, 2026–10–30 (updated with Anthropic’s statement) — doublepulsar.com
4. Phorion, “Official Claude Code Extension Compromise: Security Advisory”, October 2026 — phorion.io/blog/offical-claude-code-vsx-compromise/

For a team, the practical list is short. Keep an inventory of the extensions on developer machines. Pin versions, or delay updates, for extensions that see your code and credentials. Alert on changes to authorized_keys. And keep long-lived tokens off laptops where a short-lived one would do.
