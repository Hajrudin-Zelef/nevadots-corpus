---
id: collect-261121-veille-ia/cursor-ide/the-secrets-were-in-the-training-data-what-the-cursor-breach-actually-teaches
title: "The Secrets Were in the Training Data: What the Cursor Breach Actually Teaches"
domain: cursor-ide
role: reference
task: article
actors: ["Anysphere"]
dates: ["2026-11-13"]
keywords: ["cursor", "training", "credentials", "marketplace", "data", "fireworks", "exposed", "secrets"]
source: https://dev.to/damiano_builds/the-secrets-were-in-the-training-data-what-the-cursor-breach-actually-teaches-363a
source_anchor: ""
source_lines: [1, 35]
section: "Cursor_IDE"
---
A malicious copy of a popular coding assistant reached developers this month not through a phishing email or a compromised build pipeline, but through the ordinary update path of a marketplace. Cursor's November 13 incident report explains the full chain, and the weakest link in it is not the one most teams audit: telemetry collected for model training, holding live credentials nobody remembered were there.

The short version: an access key Cursor had issued to Fireworks AI was exposed at Fireworks. The attacker used it to copy a read-only storage bucket of coding-agent traces, found unredacted credentials inside those traces, pivoted into Cursor's SaaS tools, and on October 29 placed trojanized copies of Anthropic's Claude Code extension on the cache serving Cursor's marketplace. At most 5,000 users potentially received the malicious build during roughly eleven hours of exposure. Every link in that chain deserves attention, because most of them exist in other companies too.

Three primary documents anchor this story. Cursor's incident update describes the attack path and its containment. Fireworks AI's security bulletin describes its own October incidents, links the activity to a financially motivated group, and says it is working with Palo Alto Networks Unit 42. Notion's advisory confirms it was hit through the same vendor: chat transcripts from its Insights feature, mostly unattributable, affecting fewer than 1% of workspaces. Secondary reporting from BleepingComputer and The Hacker News adds that the trojanized extension attempted to install persistent SSH backdoors, and that coding errors likely limited its success on Windows and macOS systems where SSH was not already enabled.

Attribution discipline matters here as always. Cursor says the last attacker activity it observed was on November 1 and was blocked. Fireworks links its incident to criminals selling unauthorized model access, a practice Google Threat Intelligence Group calls LLMJacking. Uber told California regulators that a service provider's compromised credentials exposed driver information; Uber did not name the provider, and TechCrunch reported it was Fireworks. These are stated confidence levels, not courtroom findings.

The pivotal detail is where the attacker found working credentials: inside historical agent traces, mostly from 2025, from sessions not in Privacy Mode, shared with a vendor for model training. Cursor is explicit that fewer than 0.01% of customers had API credentials exposed this way, and that it was permitted to use those sessions for training.

Permission is not hygiene. Any pipeline that moves production-adjacent data to a training or analytics environment will, over time, accumulate secrets: API keys pasted into a chat to "quickly test something", tokens in configuration snippets, credentials someone once sent to a support agent. Notion's advisory quietly confirms how common this is: during its investigation it found exposed third-party API keys inside affected user messages, reported them to the issuing vendors, and revoked exposed Notion keys on November 11.

The practical rule writes itself. Treat every trace, transcript, log line, and support thread as a place secrets can survive for years, and redact before the data leaves, not before it is used. Users can help by never pasting credentials into agent sessions, but the obligation sits with whoever ships the pipeline: a secret that reaches training storage is a secret that will eventually be read by someone it was not meant for.

The marketplace part of the attack rewards precision. Anthropic did not publish the malicious copy, and this was not a vulnerability in Anthropic's product. Open VSX, the marketplace source Cursor uses, never hosted the malware. The attacker placed the tampered builds, versions 2.1.312 and 2.1.313, directly on the cache that served the marketplace, using a credential stolen during the data pivot.

This is a distribution lesson disguised as a marketplace story. Verifying that your upstream repository is clean tells you nothing about every layer between that repository and the developer's machine: caches, mirrors, CDNs, and any credential that can write to them. Cursor's response, reviewing and rebuilding the entire marketplace store and validating every other extension, is the correct shape of remediation, and an admission of how much standing trust a cache quietly accumulates.

For anyone who requested the affected versions on October 29, Cursor's remediation notice applies, with one line worth repeating verbatim in spirit: uninstalling the extension alone may not be enough.

The intrusion started with a single access key issued to a vendor. That key reached a shared bucket; data in that bucket reached more credentials; those reached SaaS tools, then the distribution path. None of the later steps required breaking encryption or exploiting a novel vulnerability. They required one leaked key and a series of places where that key, or what it unlocked, was trusted further than intended.

The fixes Cursor describes map directly onto the failure: deactivate the vendor key, move inference off the vendor, rotate every credential including ones with no evidence of exposure, and expand network access restrictions so a compromised credential stops working outside its intended location. That last one is the structural insight. A stolen credential should be a dead credential, and that is a property you build before the breach, through scoping, network restrictions, and rotation you have actually rehearsed.

Fireworks ties its incidents to LLMJacking, and the November 2 report from Google Threat Intelligence Group and Mandiant explains the economics. Stolen cloud and model credentials are resold through illicit routing gateways called LLM Mixers, which aggregate hijacked access and sell it below market rates. Mandiant observed fraudulent charges exceeding $100,000 in a single day and cumulative charges near $1 million at some organizations before access was revoked. One service, Jet Night, offered unlimited frontier-model access for about $60 a month, a price that only works if someone else pays the bill.

BleepingComputer, citing Wiz, frames October as a coordinated wave against AI developer platforms, with Railway reporting roughly $740,000 in fraudulent charges and OpenCode, Northflank, Lightning AI, and Warp among those affected. AI credentials have become immediately monetizable loot, more liquid than data and faster to monetize than ransomware negotiations. Every company issuing or holding model API keys should assume active, specialized demand for them, because the market already exists.

Cursor's percentages, less than 0.05% of users with traces exposed and less than 0.01% with credentials exposed, are the company's own counts from its own investigation. Whether the SSH backdoor succeeded on any specific machine is not publicly established, and the observed coding errors suggest partial failure, not total failure. The financial-motive attribution for the Cursor phase comes via Fireworks' assessment of its own incident. And the full set of companies touched through Fireworks may not be public yet: Notion and, per TechCrunch, Uber are confirmed or reported, and Fireworks says its investigation is ongoing.

None of these gaps soften the operational lessons. Training pipelines must redact before storage. Distribution must be verified at every layer, not just the source. Vendor keys must be scoped so their theft is boring. And secrets belong in secret managers, never in chat sessions, support tickets, or traces, because the next attacker already knows that is where people keep leaving them.

*Sources: Cursor, "An update on our recent security incident" (Nov 13, 2026); Fireworks AI security bulletin, "Update on October 2026 security incident" (updated Nov 9, 2026); Notion security advisory (Nov 12, 2026); BleepingComputer (Nov 14, 2026); The Hacker News (Nov 15, 2026); Google Cloud / Mandiant GTIG, "The Rise of LLMJacking" (Nov 2, 2026).*

## Top comments (0)
