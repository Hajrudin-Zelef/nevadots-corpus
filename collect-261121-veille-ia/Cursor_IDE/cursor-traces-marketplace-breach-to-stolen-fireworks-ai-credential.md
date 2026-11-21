---
id: collect-261121-veille-ia/cursor-ide/cursor-traces-marketplace-breach-to-stolen-fireworks-ai-credential
title: "Cursor traces marketplace breach to stolen Fireworks AI credential"
domain: cursor-ide
role: reference
task: article
actors: ["Anysphere"]
dates: ["2026-10-29"]
keywords: ["cursor", "extension", "marketplace", "code", "malicious", "stolen", "fireworks", "anthropics"]
source: https://gcn.com/cursor-traces-marketplace-breach-stolen-fireworks/23390/
source_anchor: ""
source_lines: [1, 49]
section: "Cursor_IDE"
---
Cursor has confirmed that a stolen Fireworks AI access key set off a chain of intrusions that let attackers plant a malicious copy of Anthropic’s Claude Code extension inside its marketplace on October 29, exposing up to 5,000 developers to an attempted SSH backdoor.

The AI coding company published a detailed incident update explaining how the intrusion unfolded after weeks of investigation.

## What happened in the marketplace

“On October 29, using a stolen credential from the exposed data from our internal system, the attacker placed malicious, unauthorized copies of Anthropic’s Claude Code extension directly on the cache used to serve the Cursor VS Code extension marketplace,” Cursor explained.

“Between about 2:27 am PT and 1:35 pm PT today, a maliciously modified version of Anthropic’s Claude Code extension was made available in the Cursor VS Code extension Marketplace,” Cursor said.

“The extension runs malicious code that attempts to install a persistent SSH backdoor.”

The attackers distributed it under Anthropic’s legitimate Claude Code extension ID (anthropic.claude-code), rather than as a separate fake extension.

Cursor later widened the warning to include version 2.1.312 for Windows and macOS, in addition to the originally named version 2.1.313.

## How attackers got in

“An attacker obtained an access key we had issued to Fireworks,” Cursor said in a Friday update. The stolen key, which Cursor had issued to Fireworks and which Fireworks exposed, provided read-only access to a shared storage bucket. Using credentials found in those records, the attacker reached selected internal services used by Cursor.

On October 29, 2026, the attacker used a stolen credential from the exposed data of its internal system to place trojanized versions of Anthropic’s Claude Code extension directly on the cache used to serve Cursor’s extension marketplace.

Independent researchers who analyzed the payload found it carried real network callbacks. Researchers at Phorion and OpenSourceMalware found that the attackers inserted a malicious JavaScript loader at the beginning of the extension’s extension.js file, which adds an attacker-controlled SSH key and attempts to enable SSH access and establish a reverse tunnel.

Cursor said errors in the malicious code and missing administrator rights limited the remote login attempt on most Windows and macOS hosts, since enabling SSH required elevated rights that the extension normally lacked. That flaw in the malware’s own code appears to have limited real-world damage even though the extension was live for about 11 hours.

## Scope and who was not affected

Cursor was careful to draw a boundary around the incident. The attack did not compromise Anthropic’s legitimate extension or Open VSX, the extension marketplace Cursor uses.

“At most 5,000 users who requested specific cached versions of the extension on October 29 potentially received the malicious build,” Cursor said.

Microsoft also weighed in on its own platform’s exposure. In a statement from Microsoft Threat Intelligence, the company confirmed that the malicious packages found on Cursor marketplace were never published nor distributed from the Microsoft Visual Studio Marketplace extension and the npm anthropic-ai/claude-code, which were not affected by this incident. Microsoft also said Anthropic confirmed to Microsoft that their systems were not compromised and they did not develop the malicious package posted in Cursor marketplace.

That means developers who got Claude Code through those official channels were not at risk. Cursor, built on top of Microsoft’s VS Code, maintains its own separate extension marketplace and cache, which is what the attacker actually compromised rather than Anthropic’s own release pipeline.

## Cursor’s response and remediation

The company described an immediate containment effort once the tampering was detected. “We took it down, took the extension marketplace offline, and worked with the cloud service provider hosting the attacker’s server to take it down.”

Cursor says it moved all inference traffic off Fireworks, deactivated the key issued to it, and cut its access to all shared datasets, and the company has also rotated exposed credentials, rebuilt its extension marketplace, and redeployed its production infrastructure as a precaution.

For anyone who installed the compromised builds, Cursor warned that a simple uninstall will not fully clean a machine. Customers who installed or updated to the affected Claude Code extension during the attack are advised to follow Cursor’s remediation guidance.

Sophos X-Ops recommends that organizations identify hosts running Cursor and check if the affected version was installed during the exposure window, and since uninstalling the extension leaves the backdoor in place, organizations need to remove the SSH key, persistence mechanisms, and tunnel processes from the affected host, and rotate accessible credentials.

The episode underscores how a single compromised vendor credential, in this case tied to an AI infrastructure provider, can ripple into developer tooling trusted by thousands of engineers.

It also arrives as package registries and extension marketplaces across the industry face a wave of credential-theft attacks targeting the software supply chain. That trend has pushed companies to scrutinize who can publish updates to widely used developer tools, including similar scrutiny already applied to GitHub’s workflow tool and a 16-year-old default in major code libraries.
