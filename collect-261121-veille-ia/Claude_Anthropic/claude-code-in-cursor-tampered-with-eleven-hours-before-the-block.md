---
id: collect-261121-veille-ia/claude-anthropic/claude-code-in-cursor-tampered-with-eleven-hours-before-the-block
title: "Claude Code in Cursor tampered with: eleven hours before the block"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-11-01"]
keywords: ["cursor", "marketplace", "code", "extension", "stepsecurity", "tampered", "hours", "claude"]
source: https://senn-tech.com/en/blog/claude-code-cursor-erweiterung-gesperrt
source_anchor: ""
source_lines: [1, 127]
section: "Claude_Anthropic"
---
# Claude Code in Cursor tampered with: eleven hours before the block

Anyone who runs Claude Code in Cursor does not get the extension from Anthropic. It comes from the IDE
vendor's own marketplace, and on 29 October 2026 that marketplace was the way in. Cursor itself reports an attack
on its extension system: `anthropic.claude-code`, versions 2.1.312 and 2.1.313, was available between 09:27 and
20:35 UTC with a payload that writes a foreign key into the SSH authorisation file and tries to open a tunnel to
the outside. Microsoft, StepSecurity and Phorion analysed the file; Sophos builds on Phorion. We cross-read the
sources because the case calls two common assumptions into question: that a marketplace review works like a gate, and that
an extension you have removed is gone.

**What is a tampered IDE extension?** A tampered IDE extension is modified code that an attacker places in a
marketplace under the name of a genuine publisher. It runs with the rights of the logged-in user. In the case
of Claude Code in Cursor it planted a foreign key in `authorized_keys`. That entry applies to every SSH login and
survives uninstalling the extension and the marketplace being locked down.

More supply chain incidents are collected in the topic hub Security and data protection. A related case is LiteLLM 1.107.0, where tampered packages were distributed through PyPI and Docker Hub. Who owns Cursor today is explained in SpaceX bought Cursor.

## The timeline, as far as the sources go

The reports date from 29 October to 1 November 2026 (Cursor 29 Oct, updated 30 Oct; Phorion and StepSecurity with later revisions); we read them against the sources on 5 November. They give one published time window and a few findings of differing precision. The table names the measuring source for each. All figures come from the vendors.

| Event | Time (unit: UTC) | Published | Source | 
|---|---|---|---|
| Malicious package available in the Cursor marketplace | 29 Oct 2026, 09:27 to 20:35 | 29 Oct 2026 (Cursor), update 30 Oct 2026 | Cursor notice (2:27am to 1:35pm Pacific, "about"); repeated by Microsoft (citing Cursor), Sophos, StepSecurity and BankInfoSecurity | 
| Analysis of payload, keys and persistence | 30 Oct 2026 | 30 Oct 2026 | Microsoft Security Blog, Phorion (source analysis), StepSecurity | 
| Estimate of affected users | up to 5,000 | 30 Oct 2026 | Cursor's estimate, quoted by Sophos, BankInfoSecurity (there as a social media post by Cursor) and StepSecurity; the Cursor notice itself gives no figure | 
| Update to the Cursor notice (IOCs, versions) | 30 Oct 2026, 18:30 (11:30am PT) | 30 Oct 2026 | Timestamp in the Cursor notice | 

Pacific time was still on daylight saving on 29 October (UTC minus 7 hours), which gives the UTC times. The open window is therefore eleven hours and eight minutes. It follows from two clock times that Cursor gives as "about"; nobody measured it. BankInfoSecurity writes of eleven hours.

Three points weigh more than the hour count:

1. 
Two versions are affected, 2.1.312 and 2.1.313, for Windows and macOS, in Cursor's marketplace. Cursor states that the versions were not developed by Anthropic and that no other extensions there are affected. The version numbers are easy to misread. According to StepSecurity, Anthropic never published 2.1.313 through its official channels and went straight to 2.1.314. StepSecurity downloaded the 2.1.312 on Open VSX and verified it as clean; it is a different artifact from the 2.1.312 in Cursor's marketplace. Microsoft confirms that neither the Visual Studio Marketplace nor the npm package `@anthropic-ai/claude-code` distributed the malicious packages. Anthropic
told Microsoft that its own systems were not compromised.
2. 
The malicious code is in the package itself; nothing is fetched from foreign repositories. Phorion read the `extension.js` : the loader runs as soon as Node loads the file, before the extension's normal initialisation.
StepSecurity describes the file as a clean 2.1.312 bundle with a loader of roughly 7 KB in front. In our assessment, a check that
only inspects the code at the first user click never sees this phase.
3. 
Cleanup has two parts. What stays is an Ed25519 key with the comment `svc-telemetry` in`authorized_keys` and persistence through a LaunchAgent (macOS) or a scheduled task with a Startup script as fallback (Windows).
Cursor states explicitly that removing the extension may not be sufficient. Per Cursor, the step that enables the SSH service generally fails,
because it needs administrator rights and the malicious code contains errors. Microsoft relays this as a statement by Cursor, Sophos
names coding errors. Per Cursor, the key and persistence are written anyway. That the key is only usable when
an SSH service runs on the machine follows from how SSH works. No source puts it this way for this case; it is
our own conclusion.

## What the numbers do not yet tell us

The scale is an estimate, and it comes from Cursor. Sophos writes that Cursor estimated up to 5,000 affected users; BankInfoSecurity and StepSecurity also attribute the figure to Cursor, StepSecurity as its first statement. The Cursor notice itself gives no figure (retrieved 5 November 2026); according to BankInfoSecurity it comes from a social media post by Cursor. No report gives a counted number of installs. A reader who needs a number for their own report estimates how many systems may have received the affected versions and labels it as an estimate.

It is still open how the attackers got into the marketplace's delivery system. Cursor writes that the versions were "made available" there; the notice does not name the mechanism. BankInfoSecurity quotes Anthropic as saying that an attacker used access to Cursor's marketplace to publish a tampered extension and that Anthropic was not involved in building, publishing or hosting it. StepSecurity quotes Felix Rieseberg of Anthropic to the same effect. The reports thus show only that the control failed. Where the failure sat remains open, as does attribution to an actor. The reach in an individual case is open too: whether an environment pulled the package is in its endpoint, network and installation telemetry. A vendor report does not replace that search.

## The three chains that broke here

The extension alone was not it. The decisive part is the chain, and every link is common enough in a small company's IT.

**Chain one: the name is right, the origin is not.** Installation happens from the development environment's
marketplace. The modified versions carried the real publisher's name, so checking the publisher finds nothing
unusual. The version scheme deceived as well, because 2.1.312 reused a genuine number. The giveaway was that
2.1.313 never existed officially. That can be checked against the vendor's release channel, not against the
marketplace.

**Chain two: the entry point sits in the distributor.** According to Anthropic (quoted by BankInfoSecurity and StepSecurity) the vendor stayed untouched; the
attack hit the channel. Installing from a third-party channel means vetting the channel, because the origin of
the code is not proven by it.

**Chain three: rights and place of persistence.** The change sits where a developer has the most rights: in the
SSH path. That puts every machine this person logs in to at risk, production servers included. The severity
depends on the target this path opens. How access rights and the developer workstation are set up in operation is
described on our page on IT infrastructure and operations.

## Indicators named by Cursor and the analyses

The details come from Cursor's notice and the analyses; we did not examine any file ourselves.

- Attacker server: `134.209.82.225` , ports 22 and 8099. Cursor reports the server is shut down.
- Key comment `svc-telemetry` in`authorized_keys` ; hashes of the VSIX packages and of`extension.js` are in
Cursor's notice.
- Persistence according to Cursor: on macOS the file `~/Library/LaunchAgents/com.apple.sysmond.plist` , on Windows
the scheduled task`SysMon` with the fallback script`sysmon.cmd` in the Startup folder.
- Linux: Cursor's list names a cron entry with `@reboot` , the file`~/.config/.smt` and files`/tmp/.stk_*` .
Cursor lists only macOS and Windows as distributed packages, but the Linux items are in its IOC list, and
Phorion also describes a Linux path in the code.
- According to Kevin Beaumont (Mastodon post, quoted by BankInfoSecurity), the replacement versions Anthropic published, which update automatically, do not remove the backdoor; BankInfoSecurity names version 2.1.314.

## Four measures, ranked by benefit

1. 
Check whether the key is there. On every machine where Claude Code ran in Cursor: `grep -qs svc-telemetry ~/.ssh/authorized_keys` on macOS (this is the command in Cursor's notice; on Linux it is the same check, and carrying it over is our step). On Windows, Cursor gives a PowerShell snippet that checks`$HOME\.ssh\authorized_keys` and`C:\ProgramData\ssh\administrators_authorized_keys` for the key comment; the note on the second path (Windows OpenSSH usually keeps keys for administrator accounts there) is our own addition. Cursor provides
remediation scripts for macOS and Windows that remove the extension, persistence, tunnel processes and key, and
exit with an error status if anything remains. On Linux, also look for the cron entry with`@reboot` ,`~/.config/.smt` and`/tmp/.stk_*` .
2. 
Enforce allowed extensions. Microsoft recommends in its report to restrict extensions to approved publishers and to configure an extension installation cooldown; for both settings Microsoft points to Cursor's hardening guidance, which we have not read. The publisher restriction alone would not have stopped this case, because the release went out under the real publisher's name. A cooldown longer than eleven hours would, in our assessment, have caught the window. It acts before the infection.
3. 
Watch the outbound traffic of developer workspaces. This payload's beacon goes out as an HTTP POST to port 8099, plus an attempt to 127.0.0.1:8099. Monitoring that covers only model calls and tokens shows none of it.
4. 
Rotate credentials and sessions from a trusted system. Cursor says so explicitly: the limited collection of environment variables (only the names, per Cursor) does not rule out later credential access through the backdoor. Whatever sessions and keys were on an affected machine are no longer clean after such a finding.

## What is done and what is not

Cursor removed the versions from the marketplace, also shut the marketplace down completely, rotated keys and rebuilt it; per Cursor, the hosting provider took the attackers' server offline. With that the reverse tunnel and the reporting channel no longer work, as Cursor writes. Not done are the planted key and the persistence on machines that pulled the package in the window. Until that check has run on a machine, it counts as not remediated.

Revision of 5 November 2026: we re-read the statements of Cursor, Microsoft and Sophos against their pages, corrected the attribution of the failed SSH activation and removed the section on our own search, because we cannot show a measurement file for it. In addition: the Cursor notice is dated 29 Oct (update 30 Oct), the 5,000 figure comes from a social media post by Cursor according to BankInfoSecurity, and the Microsoft pointer to the hardening guidance is now marked as a pointer.

## Further sources

- Cursor: Security notice, malicious versions of Anthropic.claude-code, Cursor, updated 30 Oct 2026
- Inside a malicious Claude Code-branded extension and its persistent SSH backdoor, Microsoft Security Blog, 30 Oct 2026 (curl gets HTTP 403, opens in a browser)
- Official Claude Code Extension Compromise: Security Advisory, Phorion
- Claude Code extension trojanized, StepSecurity
- Malicious Claude Code extension distributed through Cursor marketplace, Sophos (no publication date)
- Cursor confirms Anthropic Claude Code extension compromised, BankInfoSecurity, 30 Oct 2026
- T1098.004 SSH Authorized Keys, MITRE ATT&CK (technique reference)
- LiteLLM 1.107.0: supply chain attack checked at our own gateway, a related case from a different angle

## How do I recognise the official Claude Code extension in the Cursor marketplace?+

Not by the publisher alone: the modified versions carried the real publisher's name. You can check the version number against Anthropic's release channel. Anthropic never published version 2.1.313 there, and 2.1.312 was followed by 2.1.314 (per StepSecurity, citing Anthropic).

## Is uninstalling the extension enough?+

No. Cursor writes that removing the extension may not be sufficient, because the payload drops its own persistence scripts. According to Kevin Beaumont, the automatic update to 2.1.314 does not remove the backdoor either. Check authorized_keys, the LaunchAgents folder on macOS and the scheduled tasks on Windows.

## Does this affect companies that only use Claude Code on the command line?+

According to Microsoft, neither the Visual Studio Marketplace nor the npm package @anthropic-ai/claude-code distributed the malicious packages, and Anthropic told Microsoft its own systems were not compromised. Per Cursor, the affected installs are versions 2.1.312 and 2.1.313 from the Cursor marketplace on macOS and Windows within the stated window.
