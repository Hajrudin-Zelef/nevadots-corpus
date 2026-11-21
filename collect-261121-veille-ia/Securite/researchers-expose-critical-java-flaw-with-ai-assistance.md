---
id: collect-261121-veille-ia/securite/researchers-expose-critical-java-flaw-with-ai-assistance
title: "Researchers Expose Critical Java Flaw with AI Assistance"
domain: securite
role: reference
task: article
actors: []
dates: []
keywords: ["calif", "java", "exploit", "execution", "researchers", "flaw", "code", "critical"]
source: https://osintsights.com/researchers-expose-critical-java-flaw-with-ai-assistance
source_anchor: ""
source_lines: [1, 36]
section: "Securite"
---
"the era of 'the centaur hacker, part human, part AI,'" Calif Research wrote — and on Tuesday that hybrid proved startlingly effective.

## CVE-2026-114938 (Java4Shell): a memory-corruption bug that can lead to remote code execution

Security researchers published technical details on a critical Java vulnerability tracked as CVE-2026-114938 and nicknamed Java4Shell. Calif Research rated the flaw 9.2 out of 10 on the CVSS scale. The defect is a memory-corruption bug in OpenJDK’s Base64 decoder: faulty code writes data where it does not belong, which can crash a program or, at worst, allow an attacker to run commands on the server without any username or password.

## Where the flaw lives: OpenJDK’s AVX-512 speedup and a specific input string

The bug is located in hand-written processor instructions OpenJDK uses to accelerate Base64 decoding. That optimization runs only on chips supporting AVX-512; ARM64 systems are unaffected. Calif reported the decoder fails on a precise input — sixty-four 'A' characters followed by a single '=' padding character. The decoder then miscalculates a length and writes 64 bytes into a buffer that can be as small as 50 bytes, overwriting adjacent memory. Although the program may reject the bad input afterward, the surrounding memory is already corrupted, which typically crashes the program unless an attacker can turn the corruption into code execution through heap grooming.

### Your scanner finds 4,000 vulns. Which 12 matter?

Nubivance is a Rapid7 Registered Partner delivering vulnerability management as a service - scanning, risk-based prioritization, and remediation follow-through across IT and OT.

Fix the backlog
## Demonstrated impacts and affected products

Calif reported unauthenticated remote code execution (RCE) in widely used products and demonstrated a working compromise of Keycloak, an open-source login system. The group said unauthenticated execution was found in Keycloak, Jira, Confluence, Jenkins and PingFederate. Authenticated execution — requiring a valid account — was shown in Kafka, Solr and Elasticsearch. Researcher samczsun additionally showed Java programs can be compromised simply by connecting to a malicious server. Calif emphasized there is no single universal exploit and that each target requires tailored exploit steps; the group said it is withholding product-specific exploit details so users can patch first.

## How AI contributed: rapid discovery and exploit generation

Calif credited AI coding tools for doing much of the work. The group said researcher Quang Luong found the bug in about an hour by auditing OpenJDK with OpenAI’s Codex, spending under $200 in computing tokens. Calif added that AI agents produced most of the group’s exploit chains nearly on their own and framed the discovery as the second major AI-assisted find after php2shell. The researchers described the moment as “looking for copper and found gold,” and called the period the era of the “centaur hacker.”

## Oracle, CISA, and what operators must weigh

OpenJDK published an advisory on Tuesday and Oracle shipped patched Java builds that same afternoon as part of its November update, which covered 652 flaws across Oracle’s product line. As of the report’s publication, Calif had not observed exploitation in the wild and none of the CVEs tied to this issue had been added to CISA’s Known Exploited Vulnerability list; nevertheless, CISA published a notice calling for a patch late Tuesday.

Security practitioners warned that the combination of a widely deployed runtime, demonstrated exploitability in popular applications, and accelerated exploit development using AI raises the likelihood of real-world attacks. “We’re talking about a vulnerability in one of the most widely used software runtimes in the world, with initial exploit development taking just a few hours,” Gal Elbaz, co-founder and CTO of Oligo Security, told CyberScoop. Elbaz added that while not every Java application will be vulnerable — attackers must still account for hardware, runtime configurations, and application behavior — the technical barriers to exploitation are surmountable and organizations should treat the issue as a serious exposure while patches are deployed.

## How security teams, CISA, and affected enterprises are responding

- Security teams: Patch urgency is high — Oracle shipped patched builds the same day the flaw went public, and Calif is withholding target-specific exploit details to allow time for upgrades.
- CISA and regulators: CISA issued a notice calling for a patch; as of publication the vulnerability was not yet on CISA’s Known Exploited Vulnerability list.
- Affected enterprises and operators: Organizations running OpenJDK-based services (Calif noted OpenJDK runs at about 90% of Fortune 500 companies) must verify their hardware (AVX-512 presence matters), their use of Base64 decoding paths, and apply the supplied Oracle/OpenJDK patches or controls while upgrades are rolled out.

Calif’s disclosure closes a short loop: rapid AI-assisted discovery, public technical details, same-day vendor patches, and a CISA notice — all within a narrow calendar window. The central unresolved question is whether attackers, aided by the same AI tools, will quickly convert public proofs into widespread exploitation before organizations complete hardware-aware patching and mitigations.
