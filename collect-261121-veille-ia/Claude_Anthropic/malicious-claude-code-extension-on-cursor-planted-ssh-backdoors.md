---
id: collect-261121-veille-ia/claude-anthropic/malicious-claude-code-extension-on-cursor-planted-ssh-backdoors
title: "Malicious Claude Code extension on Cursor planted SSH backdoors"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["code", "extension", "malicious", "cursor", "malware", "claude", "access", "tunnel"]
source: https://www.bleepingcomputer.com/news/security/malicious-claude-code-extension-on-cursor-planted-ssh-backdoors/
source_anchor: ""
source_lines: [1, 65]
section: "Claude_Anthropic"
---
A malicious version of Anthropic's Claude Code VS Code extension was distributed through Cursor's extension marketplace for approximately 11 hours on October 29, exposing up to 5,000 developers to malware designed to install persistent SSH backdoors.

The extension used the version number 2.1.313 and was distributed between 2:27 a.m. and 1:35 p.m. Pacific Time.

Cursor is an AI-powered code editor built from Microsoft’s open-source Visual Studio Code (VS Code), with support for VS Code extensions. Anthropic’s Claude Code extension brings its AI coding assistant directly into compatible editors, helping developers write and debug code using natural-language instructions.

Cursor says Anthropic did not develop the malicious extension but has not explained how attackers distributed the compromised package under Anthropic's name.

"Between about 2:27 am PT and 1:35 pm PT today, a maliciously modified version of Anthropic’s Claude Code extension was made available in the Cursor VS Code extension Marketplace," warned Cursor.

"The extension runs malicious code that attempts to install a persistent SSH backdoor."

Researchers at Phorion and OpenSourceMalware found that the attackers inserted a malicious JavaScript loader at the beginning of the extension's extension.js file, leaving the legitimate extension code otherwise unchanged.

The malicious code executes when the extension loads, without requiring user interaction with Claude Code.

When installed, the malicious extension performs the following actions on compromised systems:

- Collects system and user information, including Git email and public IP address.
- Adds an attacker-controlled SSH key and attempts to enable SSH access and establish a reverse tunnel.
- Installs persistence through a macOS LaunchAgent or Windows scheduled task/Startup script, retrying the tunnel roughly every minute.
- Sends host details and backdoor status to the attacker’s server at the IP address 134.209.82[.]225

However, according to Phorion, coding errors in the malware's SSH setup routines likely prevented attackers from gaining remote access to many affected Windows and macOS systems.

The mistakes prevented the malware from enabling SSH on machines where it was not already running, leaving the reverse tunnel unable to provide access to the compromised device.

On macOS, the malware attempts to check whether Remote Login is enabled using the systemsetup -getremotelogin command, but runs it without elevated privileges. Instead of returning the SSH service's status, the command produces an administrator-access error.

Because the malware only attempts to enable SSH when the output contains "Off," it skips the activation step entirely.

On Windows, the malware attempts to install and start the OpenSSH server through PowerShell, but it tries to do so via /bin/sh, a Unix shell that isn't available on standard Windows installations. Due to this, the SSH setup fails.

In both cases, the malware can still install its SSH keys, establish persistence, and attempt to create the reverse tunnel. However, if the SSH server is not already running, the tunnel cannot provide access to the compromised machine.

Even if the SSH backdoor fails, the malware still sends reconnaissance data to the attacker's server, including the victim's username, hostname, public IP address, Git email address, and names of environment variables associated with sensitive credentials.

However, the script does not directly steal credentials, suggesting the attackers intended to retrieve sensitive data manually after gaining access through the SSH tunnel.

Cursor removed the malicious extension and temporarily shut down its entire marketplace to audit other extensions and rotate all access keys before making it available again.

The company also worked with the hosting provider to take the attacker's server offline, disrupting both the reverse SSH connections and the transmission of reconnaissance data.

Following an internal investigation, Cursor says it found no evidence that any other extension versions or the official distribution were affected.

Those who have downloaded the package during the exposure window should check for the presence of an 'svc-telemetry' SSH key on their machines, and follow Cursor’s remediation instructions if impact is confirmed.

For impacted machines, it is recommended to isolate them, rotate accessible secrets, invalidate sessions, and rebuild the affected environment from safe backups.

While initial reports suggested that npm, Open VSX, and Microsoft's Visual Studio Code Marketplace were also impacted, it was later confirmed that Cursor was the only source distributing the malicious extension.

However, many questions remain unanswered about the supply chain attack, including how the threat actors published a malicious extension under Anthropic's name on Cursor's marketplace and whether Cursor or Anthropic's systems were compromised during the incident.

BleepingComputer contacted Anthropic and Cursor with questions about the attack and will update this story if we receive a response.

Adaptive Security's AI agents analyze email intent, context, and behavior to uncover phishing, business email compromise, and invoice fraud.

See what's getting through with a 14-day read-only scan of Microsoft 365 or Google Workspace, with no MX changes.

Start your free email security scan.
## Post a Comment Community Rules

## You need to login in order to post a comment

Not a member yet? Register Now
