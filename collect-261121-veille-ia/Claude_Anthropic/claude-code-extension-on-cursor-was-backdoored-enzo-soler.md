---
id: collect-261121-veille-ia/claude-anthropic/claude-code-extension-on-cursor-was-backdoored-enzo-soler
title: "Claude Code extension on Cursor was backdoored | Enzo Soler"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["cursor", "extension", "code", "marketplace", "anthropic", "claude", "says", "access"]
source: https://enzosoler.com/news/claude-code-cursor-extension-backdoor/
source_anchor: ""
source_lines: [1, 44]
section: "Claude_Anthropic"
---
Malicious versions 2.1.312 and 2.1.313 of Anthropic's Claude Code extension went out through Cursor's extension marketplace on October 29 and planted an SSH key and a persistent tunnel on Macs and Windows PCs. Cursor has pulled them and published a check and cleanup scripts. Cursor's November 13 report traces the access to a key exposed by its vendor Fireworks.

**Update, November 14:** Cursor has now explained the access. Its November 13 incident report says an attacker used a key exposed by its vendor Fireworks, then credentials found in stolen data, and placed the tampered copies in the cache behind Cursor’s VS Code extension marketplace. Cursor confirms at most 5,000 users could have received them. The same breach exposed coding agent traces and support credentials; see what Cursor admins should rotate.

**Update, November 6:** A week on, we found no formal security advisory or customer notice from Anthropic about the tampered extension. Its public response is still the statements from its staff summarized below, which say only Cursor users were affected. Cursor’s security notice remains the place for the check and cleanup steps.

**Update, October 31:** Anthropic has now responded. Felix Rieseberg of Anthropic said this was not a compromise of Anthropic’s systems or employee laptops, and that “a malicious actor used access to Cursor’s marketplace to publish a tampered extension” that Anthropic did not build, publish or host, according to StepSecurity and BankInfoSecurity. Anthropic’s David Dworken said 2.1.313 was never published to Open VSX, the Visual Studio Marketplace or any other official host, so only Cursor users were hit. A clean release, 2.1.314, went to Open VSX on October 30 at 01:07 UTC, and StepSecurity found none of the known indicators in it. Security researcher Kevin Beaumont says the clean version does not remove a backdoor already installed, so anyone who received 2.1.313 still needs the check and cleanup below. How the attacker got that marketplace access is still unexplained.

If you use Claude Code inside Cursor, check your machine today. On October 29, between about 2:27 a.m. and 1:35 p.m. Pacific, Cursor’s extension marketplace served a tampered copy of Anthropic’s Claude Code extension. Cursor’s security notice says versions 2.1.312 and 2.1.313 for macOS and Windows were “not developed by Anthropic” and tried to install a persistent SSH backdoor. Cursor extensions update on their own by default, so you may have received it without clicking anything.

## What happened

According to Ars Technica, the bad build went out under Anthropic’s own extension listing on the Cursor marketplace, which is why it replaced the real extension in place. Anthropic says it did not publish that build (see the update above). Nobody has said how the attacker got that access. Ars reports Cursor put the exposure at “at most 5,000 users.”

Confirmed by Cursor, the malicious code:

- **Sent your machine’s details to the attacker:** hostname, username, OS, home folder path, environment variable names, working folders, your Git email and your public IP.
- **Added the attacker’s SSH key** to your`authorized_keys` file, labeled`svc-telemetry` .
- **Installed persistence:** a LaunchAgent on macOS, a scheduled task on Windows, and background scripts that retried a reverse SSH tunnel about every 60 seconds.

Two things limit the damage. Turning on the SSH service needs admin rights, so on most Macs and PCs that step failed and the attacker could not log in through the tunnel. And Cursor worked with the hosting provider to take the attacker’s server offline. Phorion’s technical write-up found the macOS code had a bug that skipped the SSH step entirely for standard users. Both say the key, the persistence and the data upload still happened.

Cursor says it removed the versions, shut down and rebuilt its marketplace, rotated its own keys and found no other tampered extensions. Phorion says the payload also has a Linux path, and Cursor’s indicator list includes Linux file paths, though its notice names only macOS and Windows.

## What to do now

**1. Run the check.** On a Mac or Linux machine, in Terminal:

`grep -qs svc-telemetry ~/.ssh/authorized_keys && echo "svc-telemetry key found" || echo "No key found; machine not impacted"`
Windows users have a PowerShell version in Cursor’s notice.

**2. If the key is there, run Cursor’s cleanup script.** The notice links `remediate-macos.sh` (run as your normal user, not root) and `remediate-windows.ps1` (run as administrator). Cursor warns that uninstalling the extension may not be enough, because the payload leaves standalone scripts running.

**3. Rotate credentials from a different, clean machine.** That means GitHub and Git hosting tokens, cloud keys, API keys and any session the affected laptop could reach. The attacker only took environment variable names, but Cursor says that does not rule out later access through the backdoor.

**4. If you run a team, search your fleet.** Cursor’s notice lists file hashes, the attacker IP and the key fingerprint for your endpoint tools.

## Turn off auto-update for agent extensions

An AI coding extension runs with your full user permissions: your source code, your SSH folder, your tokens. This attack needed no exploit. It needed control of one trusted publisher account and an auto-update setting most people never change. If you run agent extensions in Cursor or VS Code at work, turn off auto-update for them and move to new versions on purpose, a day after release. The same logic applies to the tools these plans sell, like Claude Ultra’s longer agent sessions and Claude Duet: the more an agent can do on your laptop, the more a poisoned update can do. Stolen keys turn into real bills: Google reports LLMjacking charges reaching $1 million at some companies. For how scoped agent credentials limit that blast radius, see 1Password for agents. Cursor users weighing the bundled plans can read our note on Cursor’s X subscription. Cloud dev machines carry the same risk: Nvidia rotated the SSH keys on Brev instances after a 16-day breach.

Related, November 13: another route to developers’ machines, fake Zoom and Teams calls that ask you to paste a command, per the FBI.

Related, November 16: Anthropic shortens its vulnerability disclosure deadline to 30 days for high- and critical-severity bugs.
