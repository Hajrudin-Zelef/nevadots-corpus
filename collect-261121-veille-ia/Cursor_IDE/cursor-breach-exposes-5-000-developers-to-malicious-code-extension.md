---
id: collect-261121-veille-ia/cursor-ide/cursor-breach-exposes-5-000-developers-to-malicious-code-extension
title: "Cursor Breach Exposes 5,000 Developers to Malicious Code Extension"
domain: cursor-ide
role: reference
task: article
actors: ["Anysphere"]
dates: []
keywords: ["cursor", "code", "extension", "exposed", "attackers", "data", "malicious", "credentials"]
source: https://osintsights.com/cursor-breach-exposes-5000-developers-to-malicious-code-extension
source_anchor: ""
source_lines: [1, 50]
section: "Cursor_IDE"
---
"On October 29, using a stolen credential from the exposed data from our internal system, the attacker placed malicious, unauthorized copies of Anthropic's Claude Code extension directly on the cache used to serve the Cursor VS Code extension marketplace," Cursor explained.

## How a Fireworks AI key opened the door

Cursor confirmed the incident began when attackers obtained an access key stored on Fireworks AI, a third‑party provider Cursor used for AI model training and inference. The exposed key let the attackers copy data from a read‑only storage bucket that Cursor used to share information with Fireworks. That bucket contained historical AI coding‑agent traces — mostly from 2025 — collected from users who had not enabled Privacy Mode and whose data could be used for model training.

Some traces contained unredacted API credentials. Cursor says fewer than 0.05% of users had coding‑agent traces exposed, and fewer than 0.01% of customers had API credentials compromised. Using those exposed credentials, attackers accessed additional Cursor systems and stole customer data.

## What the attackers published to the extension marketplace

With a credential obtained from the exposed internal data, the attackers placed malicious copies of Anthropic's Claude Code extension on Cursor's VS Code extension marketplace cache on October 29. The unauthorized builds were labeled as versions 2.1.312 and 2.1.313 and were available for approximately 11 hours before Cursor removed them.

As described by security researchers and summarized by Cursor, the attackers modified the legitimate Claude Code extension to execute malicious JavaScript that attempted to install persistent SSH backdoors on developers' machines. The malware collected system information, installed an attacker‑controlled SSH key, attempted to enable SSH access, and configured persistence mechanisms intended to maintain remote access. Security researchers found coding errors that likely prevented the backdoor from working on many Windows and macOS systems where SSH was not already enabled.

Cursor estimates up to 5,000 users who requested the affected extension versions during that exposure window may have received the malicious builds. Cursor advises any customers who installed or updated to those affected Claude Code versions during the attack to follow the company's remediation guidance.

### Audit-ready is a season. It shouldn't be.

Evidence in spreadsheets, controls drifting between audits, frameworks multiplying on flat headcount. Nubivance runs continuous compliance on Rapid7 Cyber GRC - SOC 2, HIPAA, ISO 27001, PCI, CMMC.

End the scramble
## Data and systems the attackers accessed and stole

Cursor says the attackers stole data from several of its software‑as‑a‑service tools after using exposed credentials. The company lists the following categories of compromised information:

- Archived, non‑production Cursor source code repositories, some containing business contact information.
- Limited customer support information and email conversations.
- Product usage metrics and customer contact details.
- Summaries of some AI coding‑agent conversations.
- Credentials that customers had previously shared with Cursor's support team.

Cursor also reports that its production infrastructure, official application releases, customer code repositories, and code and prompts belonging to users with Privacy Mode enabled were not affected.

## Cursor's detection and remediation steps

Cursor says its monitoring systems detected the unauthorized activity, prompting it to revoke the Fireworks access key, rotate exposed credentials, and investigate additional unauthorized access. As the inquiry continued, Cursor temporarily turned off some internal and product services as a precaution.

Operational steps Cursor reports taking include moving all inference traffic away from Fireworks, revoking and rotating exposed credentials, rebuilding its extension marketplace, and redeploying production infrastructure. Despite these actions, the attackers had already used a credential to publish the malicious extension builds during the window of exposure.

## What this means for developers, Cursor customers, and AI infrastructure providers

Developers: Up to 5,000 developers may have installed malicious builds of Claude Code during the roughly 11‑hour exposure window; those developers are specifically advised to follow Cursor's remediation guidance because the attacker code attempted to establish persistent SSH access.

Cursor customers: A limited set of customer support emails, contact details, product usage metrics, and some previously shared credentials were stolen. Fewer than 0.01% of customers had API credentials compromised, per Cursor's estimate; affected customers will need to verify rotated credentials and any exposed support exchanges.

AI infrastructure providers (third‑party vendors like Fireworks AI): The incident originated from a key stored on Fireworks AI and underscores the chain‑reaction risk when third‑party access keys are exposed. Cursor has moved inference away from Fireworks and cited multiple Fireworks breaches in October as part of the broader pattern.

This breach is part of a wider set of incidents cybersecurity firm Wiz labeled "Black October," attacks that targeted development infrastructure to steal authentication credentials used for AI model access. Wiz and reporting cited multiple affected companies, including Railway, Northflank, Lightning AI, OpenCode, and Warp. At Railway, attackers reportedly used stolen customer authentication secrets to generate about $740,000 in fraudulent AI charges. Fireworks AI disclosed multiple October incidents that also affected other customers; Notion, for example, disclosed that AI chat transcripts from a small number of workspaces were exposed.

The immediate facts are clear: a third‑party key copied data that included unredacted credentials, attackers used those credentials to publish malicious extension builds to a marketplace cache for roughly 11 hours, and Cursor has rotated keys, rebuilt its marketplace, and redeployed infrastructure while advising impacted users to follow remediation steps. The incident leaves a narrow but consequential trail — proof that access keys and historical agent traces can be a vector for wider compromise — and Cursor's next public steps will be watched closely by developers and customers who verify whether their own environments were touched.
