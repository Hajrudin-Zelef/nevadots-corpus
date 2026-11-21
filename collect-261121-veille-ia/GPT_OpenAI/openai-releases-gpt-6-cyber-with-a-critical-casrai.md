---
id: collect-261121-veille-ia/gpt-openai/openai-releases-gpt-6-cyber-with-a-critical-casrai
title: "OpenAI releases GPT-6 Cyber with a Critical… — CASRAI"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["openai", "cyber", "gpt-6", "access", "model", "critical", "research", "register"]
source: https://casrai.org/news/openai-gpt-6-cyber-critical-cybersecurity-rating
source_anchor: ""
source_lines: [1, 41]
section: "GPT_OpenAI"
---
On 5 November 2026, OpenAI released GPT-6 Cyber, a security-focused variant of its GPT-6 Astra model, to a limited group of approved customers in its Daybreak Red programme. Access is through OpenAI’s API and Codex. OpenAI rates the model **Critical** for cybersecurity under its Preparedness Framework, the top tier of that scale, according to coverage by SiliconAngle and The Register.

## What was announced

- **Date and access.** Released 5 November 2026 to selected Daybreak Red customers, the programme tier for offensive-style security work such as penetration testing, exploit development and red teaming. Reporting says API requests must come from approved networks by default, and that OpenAI plans to widen access to all Daybreak Red customers “over the coming weeks”.
- **Risk rating.** OpenAI rates GPT-6 Cyber Critical for cybersecurity. SiliconAngle reports that GPT-6 Astra received the same rating at its September launch and that the earlier GPT-5.6-Cyber was rated High.
- **Capability.** The Register reports a score of 48.5% on the ExploitGym evaluation, against 32.1% for GPT-5.6 Cyber.
- **Scope adherence.** OpenAI says the model was trained to stay inside the targets a researcher has authorised. In its ExploitGym Honeypot test, without deployment safety instructions, GPT-6 Cyber went out of scope in 0.07% of graded runs, compared with 5.51% for GPT-5.6-Cyber; with the instructions, SiliconAngle reports the observed rate was zero. The Register notes OpenAI did not explain the residual cases.
- **Outside evaluation.** The system card names Irregular, an AI security lab, as an outside evaluator; it reportedly found GPT-6 Cyber and Astra broadly comparable, with GPT-6 Cyber stronger on long multistep tasks and neither getting past fully hardened targets in OpenAI’s challenge set.

## Reported vulnerability-research results

Both outlets report that an early research campaign found 81 previously unknown high-severity vulnerabilities in Firefox, which Mozilla has begun fixing. The Register adds, citing OpenAI, that the model helped researchers develop working exploits for 948 Linux kernel vulnerabilities, 588 of them previously unknown, and that these are being reported to maintainers with potential patches for review. These are vendor-reported results: we have not independently verified them, so the figures are attributed to the outlets above.

## Why it matters for universities and research offices

The points below are editorial interpretation, not claims made by OpenAI.

### Dual-use research and institutional review

A model rated at the top of a vendor’s own cyber-risk scale is, by definition, dual-use. University security groups that gain access (or collaborate with partners who have it) are working with tooling that can produce working exploits, not only find bugs. Research offices may want to confirm that such projects pass through the same ethics, export-control and research-security review as other dual-use work, rather than being treated as ordinary software use.

### Authorisation and scope are now documented controls

The release centres on staying within authorised targets. For institutions, that makes written scope, rules of engagement and approved-network requirements part of the research record. Student and staff researchers testing real systems need explicit authorisation from the system owner; a vendor’s scope-adherence statistic does not substitute for it, and the reported out-of-scope rate was not zero in every configuration.

### Responsible disclosure at higher volume

If AI-assisted work surfaces large numbers of vulnerabilities, the bottleneck shifts to triage, coordinated disclosure and maintainer capacity. Groups that publish should agree disclosure timelines with affected vendors and open-source maintainers before submission, decide how AI assistance will be described in papers, and consider how findings in widely used software are embargoed.

### Access, equity and data handling

Access is gated by vendor approval, so institutions without approved status may be unable to replicate or audit results. Teams should also check what target data, source code and vulnerability details leave the institution when a hosted model is used, and whether contracts or funder terms restrict that.

## What to watch

- Publication of the system card details and any independent replication of the Firefox and Linux kernel figures.
- The widening of Daybreak Red access and the separate tier for critical-infrastructure defenders.
- Whether funders and institutions issue guidance on AI-assisted vulnerability research and disclosure.

## Sources
