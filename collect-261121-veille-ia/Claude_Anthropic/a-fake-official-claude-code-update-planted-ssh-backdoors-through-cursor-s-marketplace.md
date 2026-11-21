---
id: collect-261121-veille-ia/claude-anthropic/a-fake-official-claude-code-update-planted-ssh-backdoors-through-cursor-s-marketplace
title: "A fake 'official' Claude Code update planted SSH backdoors through Cursor's marketplace"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-10-29"]
keywords: ["cursor", "extension", "marketplace", "code", "attacker", "claude", "official", "through"]
source: https://dev.to/suraj_nandan/a-fake-official-claude-code-update-planted-ssh-backdoors-through-cursors-marketplace-2o19
source_anchor: ""
source_lines: [1, 104]
section: "Claude_Anthropic"
---
For about 11 hours on 29 October, developers using Cursor with the Claude Code extension may have received an update that looked completely normal. It had the same extension ID, the same publisher and the next version number.

It quietly tried to add an attacker's SSH key to their machine.

Cursor says malicious versions of the `anthropic.claude-code` extension were available in **Cursor's own extension marketplace** from about **02:27 to 13:35 PT on 29 October 2026** (09:27–20:35 UTC). Most reporting focused on version **2.1.313**. Cursor's security notice, updated on 30 October, lists **2.1.312 and 2.1.313** as affected, on Windows and macOS.

Key facts so far:

- 
**Anthropic did not build it.** Cursor says so directly, and its investigation found no sign that the official distribution was affected.
- 
**It was only on Cursor's marketplace.** Early reports also named Open VSX, Microsoft's Visual Studio Marketplace and npm. Later reporting corrected that: Cursor was the only distributor.
- 
**Up to about 5,000 users** may have been exposed, according to figures from Cursor cited by The Hacker News, BleepingComputer and Sophos.
- Cursor pulled the version, **shut down its whole marketplace temporarily** , audited access, rotated keys, rebuilt the marketplace and worked with the hosting provider to take the attacker's server offline. The clean release is 2.1.314.

What isn't known yet is **how a tampered package got published under Anthropic's extension ID**. Cursor hasn't said whether a publishing credential, the marketplace itself or something else was compromised.

Cursor is built on VS Code, but forks like it can't use Microsoft's Visual Studio Marketplace, so they rely on separate registries. Extensions are trusted mostly by their **ID and publisher name**. If the registry serves a malicious build under the right ID, the editor has little reason to doubt it, especially when auto-update is on.

VS Code-style extensions also have no sandbox. They run in an extension host with the same file and network access as your user account.

Researchers at Phorion and OpenSourceMalware, cited by BleepingComputer, found a **66-line JavaScript loader prepended to `extension.js`**. The rest of the extension was unchanged, so Claude Code kept working. The loader ran as soon as the extension loaded, without any interaction from the user.

A simplified view, based on Cursor's notice and the published analyses:


```
Editor auto-updates the extension (looks official)
  └─ extension.js loads → malicious loader runs first
       ├─ recon → attacker server
       │    hostname, username, OS, home + working dirs, git email, public IP,
       │    and the NAMES (not values) of secret-looking env vars
       ├─ append SSH key labelled "svc-telemetry" → ~/.ssh/authorized_keys
       ├─ try to enable the local SSH service      (usually failed: needs admin)
       ├─ open a reverse SSH tunnel → 134.209.82.225, retrying about every 60s
       └─ persistence: LaunchAgent (macOS) / scheduled task or Startup script (Windows)
```
A **reverse tunnel** is the dangerous part. Instead of the attacker connecting *in* (which firewalls and NAT usually block), the victim's machine connects *out* to the attacker and exposes its SSH port through that connection. Outbound connections are rarely blocked on developer laptops.

BleepingComputer reports that coding errors stopped most backdoors from working. On macOS, the status check `systemsetup -getremotelogin` needs elevated privileges, so it returned an error and the "enable SSH" step was skipped. On Windows, the OpenSSH setup was routed through `/bin/sh`, which standard Windows doesn't have.

But **"mostly failed" isn't the same as "safe"**. The recon data was still sent, the SSH key could still be written and persistence could still be installed. Cursor warns that the key and persistence may remain on affected machines even though the attacker's server is offline.

- 
**Possibly affected:** Cursor users on Windows or macOS who had the Claude Code extension and received 2.1.312 or 2.1.313 during the window. Auto-update makes that more likely.
- 
**Not affected, per current reporting:** the Claude Code CLI from npm, and the extension installed from Microsoft's marketplace or Open VSX.

These are defensive checks taken from Cursor's notice and the analyses above.


```
# macOS / Linux: look for the attacker's key label
grep -n "svc-telemetry" ~/.ssh/authorized_keys
# macOS: look for unfamiliar LaunchAgents added around 29 Oct
ls -lt ~/Library/LaunchAgents | head
```
```
# Windows: per-user and admin authorized_keys files
Select-String -Path "$env:USERPROFILE\.ssh\authorized_keys" -Pattern "svc-telemetry"
Select-String -Path "C:\ProgramData\ssh\administrators_authorized_keys" -Pattern "svc-telemetry"
```
Also check scheduled tasks and the Startup folder on Windows, and look in network logs for connections to `134.209.82.225`.

If you find anything:

1. **Isolate the machine.**
2. 
**Don't stop at uninstalling the extension.** Cursor and Sophos both say that leaves the backdoor in place. Remove the key, the persistence entries and any tunnel processes. Cursor publishes remediation scripts (`remediate-macos.sh` , run as the affected user, and`remediate-windows.ps1` , run elevated). Rebuilding the host may be the safer option.
3. 
**Rotate every credential reachable from that machine** , from a different, trusted device. Only env var*names* were sent, but a working SSH backdoor would have exposed everything.

**AI coding tools sit where the secrets are.** They live inside repositories, next to `.env` files, cloud CLIs and SSH configs. That makes them an obvious target, and collecting the *names* of secret-like environment variables reads like reconnaissance for a follow-up.

**Auto-update is a supply chain.** Most developers would never install an unknown extension, but they accept updates from known ones without a second look. This incident used exactly that trust.

**Forks split the trust model.** Every VS Code fork with its own registry has its own publishing pipeline, its own credentials and its own chance to get this wrong.

- On sensitive machines, turn off automatic extension updates (in VS Code, `"extensions.autoUpdate": false` ) or delay them, and review changelogs before updating.
- Use an extension allowlist where your editor supports one. VS Code has an `extensions.allowed` policy; check whether your fork does too.
- Alert on changes to `authorized_keys` and on new LaunchAgents, scheduled tasks or Startup entries. That's a cheap rule for any endpoint tool.
- Keep long-lived secrets out of shell environments on developer laptops, and prefer short-lived, scoped credentials.
- For registry operators: tighter publisher verification, signed packages, and alerts on unusual version publishing.

*This section is analysis, not reporting.*

The big open question is how the package got published under Anthropic's ID. If a publisher credential was stolen, expect pressure for mandatory signing and stronger publisher verification across extension registries. If the marketplace itself was breached, every fork that runs its own registry will face a security review. Either way, attacks on AI developer tools are clearly a trend. In the same month, an OpenCode Console breach may have exposed users' stored AI provider keys.

Nothing about this update looked wrong: right name, right publisher, next version number. That's the point. In 2026, the editor extension you trust most may be the easiest way onto your laptop, and checking where your tools come from is part of the job.

**Your turn:** do you let your editor auto-update extensions? Would this change that for you?

- Cursor, "Security notice: malicious version of Anthropic.claude-code identified in the Cursor Extension Marketplace" (published 29 Oct 2026, updated 30 Oct): https://cursor.com/blog/claude-code-extension-security-notice
- The Hacker News, "Official Claude Code Extension on Cursor Marketplace Trojanized with Persistent SSH Backdoor" (30 Oct 2026): https://thehackernews.com/2026/10/official-claude-code-vs-code-extension.html
- BleepingComputer, "Malicious Claude Code extension on Cursor planted SSH backdoors" (30 Oct 2026): https://www.bleepingcomputer.com/news/security/malicious-claude-code-extension-on-cursor-planted-ssh-backdoors/
- Sophos, "Malicious Claude Code extension distributed through Cursor marketplace": https://www.sophos.com/en-us/blog/malicious-claude-code-extension-distributed-through-cursor-marketplace
- The Hacker News, "OpenCode Console Breach May Have Exposed OpenAI, Anthropic, and Google API Keys" (19 Oct 2026): https://thehackernews.com/2026/10/opencode-console-breach-may-have.html

Part of **The Wire**, a daily brief on the tech and security news that matters to developers. Details are as reported on 30–31 October 2026 and may change as the investigation continues.

☕ **Enjoyed this post? You can support my writing on Ko-fi.**

## Top comments (0)
