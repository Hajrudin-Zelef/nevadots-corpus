---
id: collect-261121-veille-ia/claude-anthropic/anthropic-vs-code-extension-on-cursor-backdoored-for-nearly-12-hours-rocketnews
title: "Anthropic VS code extension on Cursor backdoored for nearly 12 hours - RocketNews"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["code", "cursor", "extension", "anthropic", "version", "hours", "backdoored", "access"]
source: https://rocketnews.com/2026/11/anthropic-vs-code-extension-on-cursor-backdoored-for-nearly-12-hours-6/
source_anchor: ""
source_lines: [1, 9]
section: "Claude_Anthropic"
---
A compromised version of Anthropic’s official Claude extension for VS Code was distributed through Cursor, an AI-assisted development tool platform, exposing potentially 5,000 developer machines to malicious code. The incident occurred when version 2.1.313 was posted to Anthropic’s verified account on Cursor’s marketplace before being replaced with version 2.1.314 several hours later.

The backdoored extension was designed to install a persistent SSH backdoor using an attacker-controlled SSH key, enabling reverse SSH tunneling and gaining access to local system resources. Because Cursor extensions operate with IDE-level permissions, the malware could potentially access local files, source code, developer tools, and extract variable names—which security researchers noted would allow operators to identify stored cloud credentials and API tokens for follow-up targeted attacks. The malicious code was engineered to function across Windows, macOS, and Linux systems, though implementation bugs limited effectiveness on some platforms.

Cursor worked with its cloud infrastructure provider to sever connections between infected devices and the attacker’s server. The company removed the malicious version from its marketplace, blocked further distribution, and temporarily shut down the VSCode Marketplace to rotate security keys and audit access controls before confirming no other extensions were compromised. Developers who installed or updated the extension between 5:27 am ET and 4:35 pm on Thursday should assess their exposure.

Neither Anthropic nor Cursor has publicly disclosed how attackers gained access to the official account, and Anthropic has not issued a customer warning about the incident despite the potential scope of compromise. Security researchers have published indicators of compromise including IP addresses and cryptographic hashes to help affected users identify infections.

Article summary produced by Claude AI
