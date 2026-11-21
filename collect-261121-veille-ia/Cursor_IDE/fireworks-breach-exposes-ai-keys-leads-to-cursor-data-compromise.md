---
id: collect-261121-veille-ia/cursor-ide/fireworks-breach-exposes-ai-keys-leads-to-cursor-data-compromise
title: "Fireworks Breach Exposes AI Keys, Leads to Cursor Data Compromise"
domain: cursor-ide
role: reference
task: article
actors: ["Anysphere"]
dates: []
keywords: ["cursor", "fireworks", "extension", "credentials", "access", "marketplace", "data", "attacker"]
source: https://osintsights.com/fireworks-breach-exposes-ai-keys-leads-to-cursor-data-compromise
source_anchor: ""
source_lines: [1, 38]
section: "Cursor_IDE"
---
"An attacker obtained an access key we had issued to Fireworks," Cursor said, laying out the chain that led to a secondary compromise: a trojanized build of Anthropic's official Claude Code extension appearing on Cursor's extension marketplace.

## How the Fireworks AI key seeded a broader breach

Cursor's account ties the incident to a single point of failure: an access key it had provided to a vendor called Fireworks. That key was "exposed by Fireworks," Cursor said, and an attacker used it to copy data from a read-only storage bucket Cursor used to share data with the vendor. The exposed bucket held limited coding agent traces from sessions that had not been placed in Cursor's Privacy Mode; most of those sessions date back to 2025 and had been permitted for model training.

Among the traces Cursor identified were credentials that "had not been edited out," which resulted in the exposure of API credentials for less than 0.01% of customers. Using those credentials as an entry point, the attacker moved to reach select software-as-a-service (SaaS) tools Cursor used and accessed a range of materials: archived non-production Cursor source code repositories (including repositories containing business contact information); limited customer support data and email conversations; product usage metrics and contact details; summaries of some coding agent conversations; and some credentials users had sent to the Cursor support team.

## The trojanized Claude Code extension on Cursor Marketplace

The breach escalated into a supply-chain-style incident on October 29, 2026, when the attacker used a stolen credential from Cursor's internal system to place trojanized versions of Anthropic's Claude Code extension onto the cache that serves Cursor's extension marketplace. Cursor estimated that "at most 5,000 users who requested specific cached versions of the extension on October 29 potentially received the malicious build." The malicious build was available for approximately 11 hours before Cursor removed it, took the extension marketplace offline, and worked with the cloud service provider hosting the attacker's server to take that server down.

Cursor notified users who installed versions 2.1.312 or 2.1.313 from the marketplace during the exposure window and assisted in revoking exposed credentials. The trojanized extension attempted to install a persistent SSH backdoor on affected systems, according to Cursor's disclosure.

### Audit-ready is a season. It shouldn't be.

Evidence in spreadsheets, controls drifting between audits, frameworks multiplying on flat headcount. Nubivance runs continuous compliance on Rapid7 Cyber GRC - SOC 2, HIPAA, ISO 27001, PCI, CMMC.

End the scramble
## Containment steps Cursor reported taking

Cursor described a sequence of reactive measures: deactivating the vendor key issued to Fireworks, revoking and rotating credentials, hunting for evidence of deeper access, and containing the incident after identifying further access. Cursor said it temporarily disabled internal and product services, which disrupted access to some Cursor features, and moved all inference traffic off Fireworks, deactivated the key issued to it, and cut off Fireworks' access to all datasets Cursor had shared.

Cursor emphasized that it had not detected additional malicious activity "since November 1, 2026." The company also said there is no evidence the incident affected users who had enabled Privacy Mode, customer code repositories, Cursor's production environment, the Cursor app and its official updates, its other apps, or any other extension or plugin downloaded from Cursor Marketplace.

## Uber, Notion, and the third‑party fallout

The Fireworks key exposure did not only touch Cursor. Uber confirmed that unknown threat actors stole thousands of drivers' license records after obtaining internal credentials from a service provider. In a filing with the U.S. state of California, Uber said that on October 9, 2026, a service provider notified Uber of a compromise of the provider's internal credentials and that by October 13, an investigation had determined the unauthorized party used those credentials to access Uber data connected to the service provider. The provider's name was not disclosed by Uber; TechCrunch reported the provider was Fireworks AI. Uber has not disclosed how many users were impacted by the breach.

Separately, online productivity startup Notion confirmed its data was stolen in the Fireworks AI hack, according to the reporting included in the disclosure.

## What this means for technologists, enterprise security teams, and end users

- Technologists and security teams: The incident shows how a single exposed vendor key and residual credentials in shared artifacts can be leveraged to pivot into internal systems and distribution channels. Cursor reported revoking keys, rotating credentials, and hunting for deeper access while also disabling services to contain impact.
- Enterprise procurement and legal teams: Organizations that rely on external AI vendors and shared storage should note that Cursor moved inference traffic off the affected vendor and cut off its access to shared datasets; contracts and access controls tied to third-party keys and caches will be central to post-incident reviews.
- End users and developers: Cursor's assessment states there is no evidence the incident affected users who had enabled Privacy Mode, which allows opting out of data use for training, and it identified the small fraction of exposed API credentials (less than 0.01% of customers). Users who installed the specified extension versions were notified and offered help revoking exposed credentials.

Cursor's narrative traces a short, dangerous chain: an access key issued to a vendor was exposed, credentials in shared traces were abused to reach SaaS tools and internal caches, and a stolen credential was used to seed a malicious extension into an extension marketplace. The disclosure lists concrete containment actions and affected artifact versions, and it documents collateral impact on at least two other companies. Whether further disclosures will follow rests on ongoing investigations and on whether other providers find similar exposures in their shared buckets and caches.
