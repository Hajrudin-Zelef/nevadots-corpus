---
id: collect-261121-veille-ia/claude-anthropic/malicious-claude-code-extension-in-cursor-check-your-machine
title: "Malicious Claude Code Extension in Cursor: Check Your Machine"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["cursor", "extension", "code", "attacker", "says", "anthropic", "claude", "machine"]
source: https://dev.to/devtalk94/malicious-claude-code-extension-in-cursor-check-your-machine-1b05
source_anchor: ""
source_lines: [1, 83]
section: "Claude_Anthropic"
---
If you use Cursor and had the Claude Code extension installed on 29 October 2026, there is a real chance you ran a backdoored copy of it. Headlines have described this as "Anthropic hacked", and that is not what the vendor reports say. This post separates what has been confirmed from what has been claimed, and gives you a short, practical checklist.

The short version: a modified build of the extension sat in **Cursor's** extension marketplace for about 11 hours. It tried to give an attacker SSH access to the developer's machine. Cursor says Anthropic did not publish it and that its product was not vulnerable.

These points come from Cursor's own notices, with Sophos and other security vendors repeating them:

- 
**Affected versions:**`Anthropic.claude-code` 2.1.312 and 2.1.313, on Windows and macOS, from the Cursor VS Code extension marketplace. Cursor's first notice named 2.1.313; it was updated on 30 October to include 2.1.312.
- 
**Window:** about 09:27 to 20:35 UTC on 29 October 2026 (Cursor gives it as roughly 2:27 am to 1:35 pm Pacific).
- 
**Exposure:** Cursor estimates up to 5,000 users. It describes these as users who requested the specific cached versions that day.
- 
**Who built it:** Cursor states that the malicious versions "were not developed by Anthropic."

Based on Cursor's incident update (13 November), no. Cursor says an attacker used a credential taken from Cursor's own internal data to place unauthorized copies of the extension on the cache that serves its marketplace. It says Anthropic did not publish this copy, this was not a vulnerability in Anthropic's product, and the upstream source, Open VSX, never had the malware.

The same update describes how the attacker got that credential: an access key Cursor had issued to a third-party inference provider, Fireworks, which Cursor says was exposed by Fireworks. The attacker used it to copy data from a read-only storage bucket, and some stored traces contained unredacted credentials. I have only Cursor's account of this chain. Treat it as the vendor's statement, not independent confirmation.

I could not open the Doublepulsar article that prompted this post, so I can't say how it frames the incident. If you have read it, compare its claims against Cursor's notices linked at the end.

According to Cursor, it:

1. Collects host and user details (hostname, username, OS, architecture, home directory, environment variable *names* , working directories, Git email and public IP) and sends them to an attacker's server.
2. Tries to add an attacker-controlled SSH key, enable SSH and open a reverse tunnel. Cursor says this would usually fail without administrator rights.
3. Tries to persist through a LaunchAgent on macOS or a scheduled task on Windows, retrying about every 60 seconds.

Note the word "tries". Sophos, citing an analysis by Phorion, says coding errors made SSH enablement fail on most macOS and Windows hosts. That limits the damage on many machines, but it is not a reason to skip the checks. Environment variable names alone can tell an attacker what cloud providers and services you use.

Cursor says the attacker's server has been taken offline. That means blocking the IP is of limited use now, but its logs and your own are still useful for working out whether a machine connected.

1. 
**Did you have the extension in Cursor on 29 October?** Look at your installed extensions and the version. If you only use Claude Code in VS Code from Microsoft's marketplace, or in a terminal, these notices do not cover you.
2. 
**Look for the planted SSH key.** Cursor's notice names the key comment`svc-telemetry` . On macOS or Linux:`grep -n 'svc-telemetry' ~/.ssh/authorized_keys` No output (or "No such file") means that key is not there. A matching line means the key was added.
3. 
**Look at running processes and persistence.** Cursor provides platform-specific remediation scripts (`remediate-macos.sh` and`remediate-windows.ps1` ) and the full list of indicators. Use those rather than a checklist copied from a blog, including this one, because indicators were updated after the first notice.

On Windows, Cursor gives an equivalent PowerShell check for the user's and ProgramData's `authorized_keys` files. Get it from the notice itself.

Cursor's advice, in order of priority:

- 
**Isolate the machine** if there are signs the code ran or a backdoor was installed.
- 
**Do not rely on uninstalling the extension.** Cursor warns that uninstalling may not be enough because standalone persistence scripts can keep running. Remove the unauthorized key, the persistence entries and the tunnel processes. Consider rebuilding the host.
- 
**Rotate credentials and sessions** that were reachable from that machine, doing it from a trusted system. Start with SSH keys, cloud provider keys, API tokens and anything stored in environment variables or`.env` files.

If you have no sign of the key and no sign of the persistence entries, a precautionary rotation of the most sensitive credentials is still cheap compared with the alternative.

These are my recommendations, not findings from the incident:

- 
**Extensions run with your privileges.** They can read your environment, your SSH directory and your project secrets. Treat an editor extension update like a dependency update, not like a theme change.
- 
**Auto-update is a trade-off.** It gets you security fixes quickly, but it also delivers a bad build quickly. For machines that hold production credentials, consider managing extension versions deliberately.
- 
**Keep production secrets off developer laptops.** Short-lived credentials, a secrets manager and separate deploy keys limit what a compromised editor can reach. Our SSH deploy article uses a dedicated deploy key for that reason.
- 
**Watch your**`authorized_keys`**.** A new key you didn't add is a high-signal alert on any machine, and it is simple to monitor.
- 
**Supply-chain risk follows the tooling.** A malicious build under a trusted vendor's name is exactly the case where checking the publisher alone is not enough.

- Uninstalling the extension and stopping there.
- Rotating credentials from the possibly compromised machine.
- Assuming you are safe because you use the Microsoft marketplace. The notices concern the Cursor marketplace only; check where your copy came from.
- Searching only for one version number. Cursor extended the affected versions after its first notice.
- Relying on the attacker IP alone, since the server was taken down.

The notices I read describe the Cursor marketplace copy only. Cursor says there is no evidence that other versions of the extension or the official distribution are affected. I did not test any of this.

Cursor describes an attempt to add an SSH key and open a reverse tunnel, which would give an attacker remote access if it worked. I haven't seen the term "remote access trojan" in the vendor notices I read, so I use their wording.

Follow Cursor's current notice. One third-party advisory I read mentioned 2.1.314 as the fixed version, but I couldn't verify that, and updating alone does not remove anything already planted.

- Cursor: security notice, malicious Anthropic.claude-code versions in the Cursor marketplace
- Cursor: an update on our recent security incident
- Sophos: malicious Claude Code extension distributed through Cursor marketplace

*Originally published on DEV Talk.*

## Top comments (0)
