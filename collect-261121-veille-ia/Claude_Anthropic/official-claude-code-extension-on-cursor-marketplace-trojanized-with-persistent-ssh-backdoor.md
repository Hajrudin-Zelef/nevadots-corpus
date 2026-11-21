---
id: collect-261121-veille-ia/claude-anthropic/official-claude-code-extension-on-cursor-marketplace-trojanized-with-persistent-ssh-backdoor
title: "Official Claude Code Extension on Cursor Marketplace Trojanized with Persistent SSH Backdoor"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-10-30"]
keywords: ["extension", "code", "cursor", "marketplace", "version", "claude", "anthropic", "malicious"]
source: https://www.thehackernews.com/2026/10/official-claude-code-vs-code-extension.html?m=1&hl=ja_JP
source_anchor: ""
source_lines: [1, 72]
section: "Claude_Anthropic"
---
Cybersecurity researchers have disclosed that a trojanized version of Anthropic's official Claude Code extension was distributed through the Cursor Marketplace as a legitimate update, attempting to install a persistent SSH backdoor on affected systems.

The malicious update, version 2.1.313, was distributed under the extension's legitimate publisher identity, prompting Cursor to temporarily shut down its marketplace to remove the compromised package. The exact method attackers used to introduce the unauthorized update remains unclear.

"We identified a security incident affecting a modified Claude Code VS Code extension accessed through our Cursor IDE extension marketplace, impacting at most 5,000 users," Cursor said. "This malicious version of the extension was not developed by Anthropic. It does not impact other extensions downloaded from the cursor[.]com/marketplace, Grok Bot, Grok, or our other SpaceXSI product.

However, Cursor's statement that the malicious version "was not developed by Anthropic" misses the point. The attackers distributed it as version 2.1.313 under Anthropic's legitimate Claude Code extension ID (anthropic.claude-code), rather than as a separate fake extension.

"The package identifies itself as anthropic.claude-code version 2.1.313, but its main JavaScript bundle contains a malicious 66-line loader inserted before the legitimate extension code," Jenn Gile, co-founder of OpenSourceMalware, said in a statement shared with The Hacker News.

The malicious update was distributed only through Cursor's marketplace, with no evidence that it reached Open VSX or Microsoft's official VS Code Marketplace. Users who installed Claude Code through either of those marketplaces are not known to be affected by this incident.

As of October 30, 2026, the malicious version has been removed. The latest version published by Anthropic on Open VSX and Microsoft VS Code Marketplace is 2.1.314.

The extension is designed to activate after VS Code startup and when its webview opens, then proceeds to add an attacker-controlled key to the user's "~/.ssh/authorized_keys" file. The key is labeled "svc-telemetry" in an attempt to make it appear harmless.

Subsequently, it attempts to make the victim reachable through SSH across Windows, Linux, and macOS, establishes a reverse tunnel to "134.209.82[.]225," installs platform-specific persistence in the form of a scheduled task, crontab, and LaunchAgent, and transmits host reconnaissance data to "134.209.82[.]225:8099/beacon" on port 8099 over an unencrypted HTTP request.

"Turning on the local SSH service requires admin rights, which the malicious extension does not have on most macOS and Windows hosts," the SpaceXAI-owned company said in its own analysis of the incident.

"On those hosts the step fails and the attacker could not have logged in through the tunnel. The extension still adds the attacker's SSH key, installs persistence, and sends host details to the attacker. Follow the remediation steps below regardless."

The collected data includes -

- Public IP address from api.ipify[.]org
- Hostname and local username
- Operating system and CPU architecture
- Home directory and current working directory
- Global Git email from git config --global user.email
- Names of up to 40 environment variables containing KEY, TOKEN, SECRET, PASS, AWS, or GCP

"Environment-variable values are not placed directly in the initial beacon," security researcher Paul McCarty said. "Their names can still reveal which cloud providers, CI systems, and credential types are configured. More importantly, successful SSH access would allow the operator to inspect the account directly."

No other follow-on payloads have been observed. That said, the establishment of a reverse tunnel provides a pathway to the victim's SSH service, allowing the operator to issue commands or transfer files.

"It was pulled very quickly, but anyone who has their Claude Code instance set to auto-update could have consumed the malware," Gile said.

Cursor has revealed that the incident has affected at most 5,000 users who installed the malicious version between about 2:27 a.m. and 1:35 p.m. PT on October 29, 2026. The company also said that it will be reaching out to impacted users directly.

Users who have installed the compromised extension during the window are advised to isolate the hosts, remove the unauthorized SSH key and persistence mechanisms, and rotate credentials and sessions.

The disclosure comes a little over a week after an unknown attacker replaced the release binaries of five already-published versions of OVHcloud's official CLI ("ovh/ovhcloud-cli") with a secret-stealing malware after uploading them through a GitHub account of a legitimate OVH contributor ("HassenBF") hijacked in March 2026 as part of the GlassWorm operation.

### Update

Anthropic has disclosed that the incident did not involve the compromise of its systems, adding that a malicious actor used their access to Cursor's marketplace to publish a tampered version that remained live for close to 12 hours.

"The versions on our official channels including VS Code and OpenVSX are not affected and this only impacts Cursor users," David Dworken, a security engineer at Anthropic, said in an X post.

Felix Rieseberg, an Anthropic engineering manager, said that "we're working with Cursor on this incident" and reiterated it did not involve a compromise of Anthropic's systems or employee laptops.

In response to the incident, Cursor said it rotated its keys, rebuilt the marketplace, and worked with the cloud provider to take the server offline so that the reverse shell is no longer functional. It also stressed that no other extensions were maliciously modified as part of the attack.

It's worth mentioning that Cursor has revised its security notice to identify both anthropic.claude-code versions 2.1.312 and 2.1.313, distributed through the Cursor Marketplace for Windows and macOS, as affected. The advisory previously listed only version 2.1.313.

However, several questions surrounding the supply chain attack remain unanswered, including how the threat actor managed to publish a malicious extension under Anthropic's name on Cursor's marketplace and what level of access they were able to obtain.

Software supply chain security company Socket, which published its own analysis of the incident, said it found no evidence that other registries were impacted as a result of the incident.

"Our review of registry records and its own ingestion logs has found no evidence that Claude Code version 2.1.313 became publicly available through Open VSX during the period examined," Socket said. "The version is also absent from the npm package's current version and publication-time records."

Microsoft has also confirmed that the malicious version was never published or distributed from the Microsoft Visual Studio Marketplace extension or through npm via the "anthropic-ai/claude-code" package.

"The injected loader runs when the extension module loads and attempts to add an attacker-controlled Secure Shell (SSH) key, enable the local SSH service, establish reverse SSH forwarding, configure persistence, and report host details," Microsoft said.

"Successful remote access depends on host privileges, SSH configuration, and the attacker's server configuration; however, the loader may still write keys and persistence artifacts even when tunnel access fails."

The Windows maker is urging organizations or developers to identify systems that have installed or automatically updated to version 2.1.312 or 2.1.313 from Cursor's extension marketplace, preserve evidence, isolate them, remove confirmed malicious artifacts, review endpoint telemetry, and rotate credentials or revoke sessions.

"Removing or updating the extension does not remove artifacts written outside its installation directory. Remove confirmed unauthorized SSH keys, tunnel identities, scripts, and startup entries separately," the Microsoft Security Research team said.

*(An earlier version of the story stated that the extension on the Open VSX registry was compromised. The incident has since been traced back to a compromise of the Cursor VS Code Marketplace.)*
