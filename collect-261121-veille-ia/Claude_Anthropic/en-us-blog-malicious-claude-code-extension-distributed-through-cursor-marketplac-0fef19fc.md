---
id: collect-261121-veille-ia/claude-anthropic/en-us-blog-malicious-claude-code-extension-distributed-through-cursor-marketplac-0fef19fc
title: "En us blog malicious claude code extension distributed through cursor marketplac"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["cursor", "extension", "malicious", "claude", "code", "sophos", "blog", "distributed"]
source: ""
source_anchor: ""
source_lines: [1, 7]
section: "Claude_Anthropic"
---
On October 29, 2026, Cursor reported that a malicious version of Anthropic’s Claude Code extension (2.1.313) was available in the Cursor extension marketplace between approximately 09:27 and 20:35 UTC. Cursor identified impacted macOS and Windows packages and estimates that up to 5,000 users were affected.
Analysis by Phorion revealed that the extension adds an attacker-controlled SSH key to authorized_keys, attempts to open a reverse SSH tunnel to 134.209.82[.]225, establishes persistence, and sends host details to the attacker. Due to coding errors, SSH enablement fails on most macOS and Windows hosts.
Recommended actions
Sophos X-Ops recommends that organizations identify hosts running Cursor and check if version 2.1.313 was installed during the exposure window. Uninstalling the extension leaves the backdoor in place, so organizations need to remove the SSH key, persistence mechanisms, and tunnel processes from the affected host, and rotate accessible credentials.
Sophos countermeasures
SophosLabs continues to monitor the threat landscape for activity related to this threat and will deliver detections and protections as available.
In addition, Sophos is searching customer telemetry for reported indicators and using Sophos AI Defense to identify internal devices that have Cursor installed.
