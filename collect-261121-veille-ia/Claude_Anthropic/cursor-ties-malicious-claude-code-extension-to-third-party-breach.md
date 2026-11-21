---
id: collect-261121-veille-ia/claude-anthropic/cursor-ties-malicious-claude-code-extension-to-third-party-breach
title: "Cursor Ties Malicious Claude Code Extension to Third-Party Breach"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["cursor", "malicious", "extension", "code", "marketplace", "claude", "third-party", "breach"]
source: https://Www.Darkreading.com/application-security/cursor-malicious-claude-code-extension-third-party-breach
source_anchor: ""
source_lines: [1, 35]
section: "Claude_Anthropic"
---
# Cursor Traces Malicious Claude Code Extensions to Third-Party Breach

A compromised key issued to AI infrastructure provider Fireworks was used by threat actors to access Cursor data and credentials.

Cursor has traced the origins of a security incident involving a malicious version of Anthropic's Claude Code extension in its marketplace to a breach at AI infrastructure provider Fireworks.

As many as 5,000 users may have downloaded the tampered extension during the roughly 11 hours it was live on Oct. 29. It was a reminder of the risk organizations face when they rely on third-party marketplaces for software applications and extensions. It also illustrates how one security lapse can lead to a cascade of subsequent breaches and supply chain attacks.

When Cursor first disclosed the incident, the company said attackers had placed malicious versions of Anthropic's Claude Code extension in a cache serving its Visual Studio (VS) Code extension marketplace. Users who requested either of two specific cached versions of the extension (2.1.312 and 2.1.313) during the exposure window may have downloaded the malicious builds, the company said. The malicious extensions attempted to collect information about infected systems and establish persistent remote access, including by installing an attacker-controlled SSH key.

Cursor quickly removed the malicious extensions, temporarily shut down its marketplace, and audited the store for signs of further tampering. Cursor also rotated its keys, rebuilt the marketplace, and worked with the cloud provider hosting the attacker's server to take it offline. The company's initial advisory, however, did not explain how the attacker might have gained access to its environment and planted the malicious extension in its extensions marketplace.

## The Third-Party Breach(es) That Triggered It All

In an update on Nov. 13, Cursor filled in that blank, with a detailed description of what its investigation had unearthed in the days following the Oct. 29 incident.

The intrusion, according to Cursor, began when an attacker obtained an access key the company had issued to Fireworks, an AI startup based in San Mateo, Calif. Cursor said "the key was exposed by Fireworks," and the attacker used it to copy data from a read-only storage bucket containing historical coding-agent session records belonging to Cursor users. Most of the records were from 2025 and involved sessions that were not in Privacy Mode and that Cursor was permitted to use for model training purposes.

Some of the stored session records contained credentials that had not been properly redacted. The attacker used those credentials to access what Cursor described as select SaaS tools for accessing non-production source code repositories, customer support data, emails, usage metrics, and other data.

On Oct. 29, the attacker subsequently used one of the compromised credentials to tamper with the cache that Cursor uses to deliver extensions through its VS Code marketplace. The attacker inserted unauthorized copies of Anthropic's Claude Code extension in the marketplace where they remained available to users for about 11 hours.

"Anthropic did not publish this copy, and this was not a vulnerability in Anthropic's product," Cursor stressed in its update. "The marketplace source, Open VSX, never had the malware. At most 5,000 users who requested specific cached versions of the extension on October 29 potentially received the malicious build."

Cursor estimated that API credentials belonging to fewer than 0.01% of its customers were exposed in the incident. Historical coding-agent session records, or traces, belonging to fewer than 0.05% of users were accessed, the company said.

However, the supply chain attack goes back even further than Fireworks, which itself was a victim of a third-party breach. In a Nov. 9 update, Fireworks said a cloud provider hosting the AI company's R&D systems "experienced a security incident on Oct. 23." The attackers used the access to the R&D systems to obtain sensitive data, including API keys and access tokens.

According to Fireworks, the available evidence linked the breach of its environment to a financially motivated cybercriminal group seeking to monetize unauthorized access to large language models. It's a practice known as LLMjacking, which Google and others have described as a rapidly emerging threat for organizations across multiple sectors in North America, Asia-Pacific, and other regions.

## Cursor's Major Security Overhaul

Following the incident, Cursor said it has implemented a broad security overhaul to reduce the risk of similar compromises. Immediate steps the company has taken include cutting off Firework's access to all its shared data sets, revoking and replacing potentially exposed credentials, and rotating internal credentials. The company has also tightened restrictions on how its systems and third-party service providers can communicate with one another and expanded automated scanning for secrets across its code repositories and internal data.

The Cursor incident is another indication of how the rapid adoption of AI-powered development tools and increasingly interconnected software ecosystems has exacerbated software supply chain risks. Recently, for instance, researchers uncovered more than 550 exposed secrets in Microsoft VS Code and Open VSX extension packages, including over 100 publishing tokens that could have allowed attackers to distribute malicious updates to legitimate extensions. Similarly, earlier this year researchers identified dozens of malicious GlassWorm VS Code extensions seeded in the Open VSX marketplace to steal developer credentials that attackers could use to spread poisoned software packages.
