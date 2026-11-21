---
id: collect-261121-veille-ia/claude-anthropic/stolen-fireworks-ai-key-led-to-trojanized-claude-code-extension-cyberpeople
title: "Stolen Fireworks AI Key Led to Trojanized Claude Code Extension | CyberPeople"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["cursor", "extension", "fireworks", "code", "trojanized", "claude", "attacker", "access"]
source: https://cyberpeople.tech/blog/stolen-fireworks-ai-key-trojanized-claude-code-cursor
source_anchor: ""
source_lines: [1, 81]
section: "Claude_Anthropic"
---
A Fireworks AI breach exposed a Cursor access key, letting attackers publish a trojanized Claude Code extension that reached up to 5,000 developers.

When one company is breached and another company's customers get hurt, that is a textbook supply-chain attack. That is exactly what happened in mid-November 2026: an incident at Fireworks AI, an AI compute provider, spilled into a breach of Cursor's infrastructure and the publication of a trojanized copy of the Claude Code extension. Here is what happened, how big the impact was, and what every development team should learn from it.

Cursor is a popular AI code editor that embeds a coding assistant directly into the development environment and runs its own extension marketplace. On October 29, 2026, the company reported that it had found a "malicious, unauthorized copy" of Anthropic's official Claude Code extension in that marketplace.

On November 14, Cursor published an update naming the root cause: the compromise of an access key the company had issued to a third-party vendor — Fireworks AI. Fireworks AI is an AI compute provider that Cursor used for inference and model training.

"An attacker obtained an access key we had issued to Fireworks. The key was exposed by Fireworks, and the attacker used it to copy data from a read-only storage bucket we used to share data with them," Cursor said.

In other words: Cursor itself was not breached first — its vendor was. But a key issued "outward" proved enough to build an entire chain of follow-on intrusions.

## The attack chain: from a stolen key to a trojanized extension

The sequence of events Cursor reconstructed looks like this:

1. **Fireworks AI compromise.** The attacker breached Fireworks AI's environment and obtained an access key Cursor had issued to that vendor.
2. **Copying data from a read-only bucket.** Using that key, the attacker copied data from a storage bucket Cursor used to share data with Fireworks. The bucket contained limited coding-agent traces, mostly from 2025, from sessions that were not in Privacy Mode, which Cursor was permitted to use for model training.
3. **Exposure of unredacted credentials.** A small number of those traces contained credentials that had not been redacted. Cursor estimates API credentials were exposed for less than 0.01% of customers.
4. **Further escalation.** Starting with the stolen credentials, the attacker reached select Cursor SaaS tools, and from there accessed select archived (non-production) source code repositories, some customer support data, product usage metrics, contact details, summaries of some coding-agent conversations, and some credentials users had sent to the support team.
5. **Publishing the trojanized extension.** On October 29, using a stolen credential, the attacker placed malicious copies of the Claude Code extension directly on the cache that serves the Cursor extension marketplace.

The key point: Anthropic did not publish this copy, and this was not a vulnerability in Anthropic's product. The upstream marketplace (Open VSX) never contained the malware at all — the malicious build was placed straight into the cache that serves extension delivery in Cursor.

## The scale: how many users were affected

According to Cursor, at most 5,000 users who requested specific cached versions on October 29 could potentially have received the malicious build. It was available for roughly 11 hours before being removed.

The versions in question are **Anthropic.claude-code 2.1.312** and **2.1.313**.

According to researchers (including Morphisec), trojanized version 2.1.313 installed a persistent SSH backdoor on the developer's machine. Outwardly it was a "real" extension that behaved like ordinary Claude Code — the only difference was what it did in the background after launch. For a developer, such a backdoor means the attacker gains remote access to a machine that often holds keys, tokens, and repository access.

At the same time, Cursor stresses that the vast majority of customers were not affected. The following were not exposed:

- prompts and code from users working in Privacy Mode;
- data and live traffic being sent to Cursor right now;
- the production systems and infrastructure that run Cursor;
- the Cursor app itself and its official updates, along with release signing keys;
- other extensions in the marketplace;
- active inference requests and customers' code repositories.

For users not using Privacy Mode, coding-agent traces were exposed for fewer than 0.05% of users — and Cursor says it notified each of them directly.

## Why this matters right now

This incident is not an isolated case but a symptom of a broader trend: AI development tools have become a new target for supply-chain attacks. When a developer trusts an AI assistant with access to code, repositories, and credentials, compromising a single extension or a single vendor hands the attacker keys to many environments at once.

Several specific aspects of this case deserve attention:

- **A third party as the weak link.** The attack did not start at Cursor but at Fireworks AI. A key issued "outward" proved enough to read shared storage. The reminder: your organization's security equals the security of your vendors.
- **Unredacted secrets in traces.** Credentials that had not been redacted ended up in model-training data. The lesson: "raw" coding-agent logs can contain secrets and must be scanned and cleaned before storage.
- **Trust in the extension marketplace.** The malicious build never went through the upstream Open VSX — it was dropped directly into the delivery cache. To the user, the extension looked legitimate.

## How Cursor responded

After detecting the unauthorized activity, Cursor:

- deactivated the key issued to Fireworks and fully moved inference traffic off that vendor;
- began revoking and rotating credentials — the riskiest first, then, out of caution, all credentials across internal systems;
- removed the malicious extension, took the marketplace offline, and worked with the cloud provider hosting the attacker's server to take it down;
- validated the authenticity of all other marketplace extensions and rebuilt the marketplace;
- expanded network access restrictions, added secret scanning across repositories, and extended scanning to other internal data sources;
- redeployed production infrastructure and engaged external forensics experts.

On October 30, Cursor published a security notice with indicators of compromise and system-check scripts, and began notifying affected users and team admins through in-app notices and email. The last attacker activity was observed on November 1 — and it was blocked. The company believes the incident is contained.

## What developers and teams should do

If you use Cursor or any AI coding assistant, a few simple but effective steps are worth taking:

1. **Check the extension version.** If you installed or updated the Anthropic.claude-code extension through the Cursor marketplace on October 29 and you are on version 2.1.312 or 2.1.313, follow the steps in Cursor's security notice. Simply uninstalling the extension may not be enough, because the backdoor can remain on the system.
2. **Rotate credentials.** If you received a list of keys to rotate from Cursor, do it immediately. In general, it is wise to regularly rotate the API keys and tokens used by AI tools.
3. **Enable Privacy Mode** when working with confidential code: it was non-Privacy-Mode session data that ended up in the exposed bucket.
4. **Scan for secrets in traces and logs.** Make sure coding-agent logs contain no passwords, tokens, or keys. If your team already has such tools, point them at all internal data stores.
5. **Control third-party access.** Review which keys and permissions are issued to vendors and whether they are all still needed. A key issued to a vendor is an entry point worth monitoring.

## Conclusion

The Fireworks AI breach and the trojanized Claude Code extension are another reminder that, in the era of AI development, the supply chain has grown longer — and therefore more fragile. The attack started at a third-party compute provider and hit users of a popular AI editor.

For a security team, this means one thing: trust in AI tools must be backed by verification — of versions, secrets, access rights, and vendors. For a developer, it means that even an "official" extension deserves scrutiny before it gets access to your code.
