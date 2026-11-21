---
id: collect-261121-veille-ia/cursor-ide/cursor-s-security-incident-explained-a-leaked-vendor-key-a-poisoned-extension-and-5-000-users-at-ris
title: "Cursor's Security Incident, Explained: A Leaked Vendor Key, a Poisoned Extension, and 5,000 Users at Risk"
domain: cursor-ide
role: reference
task: article
actors: ["Anysphere"]
dates: []
keywords: ["cursor", "extension", "code", "attacker", "exposed", "users", "marketplace", "credentials"]
source: https://dev.to/tin_thnhbnh_6d6c9102d/cursors-security-incident-explained-a-leaked-vendor-key-a-poisoned-extension-and-5000-users-47h4
source_anchor: ""
source_lines: [1, 54]
section: "Cursor_IDE"
---
On November 13, Cursor published the full investigation into a security incident it first disclosed on October 29: a malicious, unauthorized copy of Anthropic's Claude Code extension had appeared in the Cursor VS Code extension marketplace. The post is worth reading in full, because the attack chain is a case study in how a single leaked vendor key can cascade through a supply chain. Here is what actually happened, what was and wasn't affected, and what to do if you were in the exposure window.

**1. It started at Fireworks.** An attacker obtained an access key that Cursor had issued to Fireworks, an inference vendor. The key was exposed by Fireworks, and the attacker used it to copy data from a read-only storage bucket Cursor used to share data with the vendor.

That bucket held limited coding agent traces, mostly from 2025, from sessions that were not in Privacy Mode — data Cursor was permitted to use for model training. A small number of those traces contained credentials that had not been redacted. Cursor says less than 0.01% of customers had API credentials exposed.

**2. Stolen credentials opened more doors.** Starting with those credentials, the attacker reached select SaaS tools used by Cursor. From there they accessed archived, non-production Cursor source code repositories (some containing business contact information), limited customer support data and email conversations, product usage metrics and contact details, summaries of some coding agent conversations, and some credentials users had sent to the Cursor support team.

**3. The attacker poisoned the extension marketplace.** On October 29, using a credential stolen from the exposed internal data, the attacker placed malicious, unauthorized copies of Anthropic's Claude Code extension directly on the cache used to serve the Cursor VS Code extension marketplace.

Two points Cursor stresses: Anthropic did not publish this copy, and this was not a vulnerability in Anthropic's product. The marketplace source, Open VSX, never had the malware. The poisoned build lived only in Cursor's serving cache.

At most 5,000 users who requested specific cached versions of the extension on October 29 potentially received the malicious build. It was available for approximately 11 hours before Cursor removed it. Cursor took the extension marketplace offline and worked with the cloud provider hosting the attacker's server to take that down too.

**4. Notification.** On October 30, Cursor published a security notice on its blog and on X, with indicators of compromise, remediation steps, and scripts to check your system, and began notifying affected users and team admins through in-app notices and email. Team admins whose credentials were exposed received a list of the specific keys to rotate, and Cursor asked the third-party services that issued those credentials to revoke them.

**5. Containment.** Cursor says the last attacker activity it has seen was on November 1, and it was blocked. The incident is believed contained, with monitoring continuing.

The list of what stayed safe matters as much as the breach itself:

- Your prompts and code, if you use Privacy Mode. For users not in Privacy Mode, coding agent traces were exposed for fewer than 0.05% of users — each has been notified directly.
- Any data or live traffic you are sending to Cursor right now. The exposed traces were limited to storage used for historical offline analysis.
- The production systems and infrastructure that run Cursor.
- The Cursor app and its official updates, and the code shipped to clients and cloud services. Cursor ships from a separate code hosting system the attacker never accessed, and no release signing keys were exposed.
- Other products, including Cloud Agents, Grok Bot, and all other SpaceX SI products.
- Any other extension in the marketplace — Cursor validated that all others were authentic, and reviewed and rebuilt the marketplace store.
- Active inference requests, which were not exposed. As a precaution, Cursor moved all inference away from the third-party training and inference vendor.
- Any plugins downloaded from the Cursor Marketplace, and customer code repositories.

- Moved all inference traffic off Fireworks, deactivated the key issued to it, and cut off its access to all shared data sets.
- Rotated every exposed Cursor credential found, highest risk first, and as a precaution rotated all credentials in use across its internal systems, even where there was no evidence of exposure.
- Validated all other extensions in the marketplace and rebuilt the store.
- Expanded network access restrictions across service providers, so compromised credentials can't be used outside their intended location.
- Added secret scanning and protections across all code repositories, and expanded scanning to other internal data sources.
- Redeployed all production infrastructure and services as a precaution, reviewing every internal service before restoring it.
- Engaged external forensics experts, and contacted everyone affected.

If you installed or updated to the tampered versions — **Anthropic.claude-code 2.1.312 or 2.1.313** — from the Cursor VS Code extension marketplace during the exposure window, follow the steps in Cursor's security notice. Uninstalling the extension alone may not be enough; the notice includes indicators of compromise and scripts to check your system.

If you received no notification from Cursor, your data was not in the exposed traces.

Three things stand out from this incident:

**Vendor keys are blast-radius decisions.** The breach did not start with Cursor's own infrastructure. It started with a key issued to a third party, for a read-only bucket, that turned out to contain unredacted credentials in training traces. Every API key you hand to a vendor is a trust boundary — scope it, monitor it, and assume the data behind it can leak.

**The cache is part of the supply chain.** The malicious extension never touched Open VSX, the marketplace source. It was injected into the cache that serves the marketplace. If you run any distribution layer — a CDN, a mirror, a cache — that layer is as much a target as the origin, and it needs the same integrity checks.

**Privacy Mode is not just a preference.** The single clearest dividing line in this incident was whether sessions were in Privacy Mode. Traces from Privacy Mode sessions were not in the exposed data at all. For tools that train on your usage by default, the privacy toggle is a security control, not a setting to leave for later.

Cursor apologized to those impacted and said it will update the post if anything important changes. For a tool that sits between your code and the models that read it, that follow-through is the part worth watching.

*Source: Cursor, "An update on our recent security incident," November 13, 2026 — https://cursor.com/blog/security-incident-update*

## Top comments (0)
