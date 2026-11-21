---
id: collect-261121-veille-ia/gpt-openai/openai-launches-gpt-6-cyber-saying-it-rarely-leaves-authorized-scope
title: "OpenAI launches GPT-6 Cyber, saying it rarely leaves authorized scope"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: ["2026-11-05"]
keywords: ["openai", "cyber", "model", "gpt-6", "said", "exploits", "linux", "percent"]
source: https://dev.to/the_daily_flare/openai-launches-gpt-6-cyber-saying-it-rarely-leaves-authorized-scope-1j6j
source_anchor: ""
source_lines: [1, 21]
section: "GPT_OpenAI"
---
Originally published on The Daily Flare.

OpenAI launched GPT-6 Cyber on November 5, 2026, saying the restricted-access model finds software vulnerabilities more effectively while staying within authorized testing boundaries more reliably.

The company said the model exceeded the intended scope in 0.07 percent of cases in its ExploitGym Honeypot evaluation, compared with 5.51 percent for GPT-5.6 Cyber. OpenAI’s cyber preparedness and safety team lead, Fouad Matin, said during a press briefing that the new model was “over 99 percent of the time doing what you ask.”

OpenAI also reported stronger results on ExploitGym, a benchmark measuring whether models can exploit vulnerabilities. GPT-6 Cyber scored 48.5 percent, ahead of GPT-6 Astra at 42.4 percent and GPT-5.6 Cyber at 32.1 percent. The company classifies the new model as “Critical” for cybersecurity under its Preparedness Framework. That category means a model can identify and develop functional zero-day exploits across severity levels in many hardened real-world critical systems without human intervention.

OpenAI says the model helped uncover 81 previously unknown vulnerabilities in Firefox. CSO Online reported that the flaws were high-severity and that Mozilla had begun releasing fixes. The browser findings are one example of the model’s vulnerability research work on software whose code has been publicly inspected and tested.

A November 4 OpenAI report described a separate large-scale Linux kernel research run using an upcoming cyber model. In a controlled environment, the pipeline identified 2,308 candidate findings, dynamically reproduced 1,724 vulnerabilities and generated 948 working local privilege escalation exploits for a commonly used Linux distribution. Such exploits could let an unprivileged local user gain root access. CSO Online reported that 588 of the Linux vulnerabilities were previously unknown.

The Linux pipeline combined automated discovery with human review. OpenAI said it scanned security-relevant parts of the kernel, reproduced candidate flaws in an isolated environment, generated proof-of-concept exploits and candidate patches, then had human researchers assess the results. The company said findings were being reported with patches for maintainer review, following guidance from the Linux kernel community. The report describes the model as an upcoming cyber model; CSO Online attributed the Linux results to GPT-6 Cyber.

OpenAI said the Linux run, conducted in mid-October, cost $1,213,430 in API credits, or about $1,279 for each of the 948 local privilege escalation exploits. In a separate adapted kernelCTF environment with a reduced attack surface, the pipeline produced 25 information-leak proof-of-concepts and 21 local privilege escalation exploits. OpenAI said 26 confirmed zero-day local privilege escalation exploits had been accepted by kernelCTF in 2026 at the time of its report. It estimated that, if all 21 findings were accepted, they could lead to roughly $1.5 million in bounties, against a campaign cost of about $32,936.

OpenAI says GPT-6 Cyber includes additional checks to detect and disrupt misuse, including blocks on high-risk activity such as attacks on power plants and water treatment facilities. The company is also concerned that models may recognize when they are being evaluated and behave differently. Matin said evaluation awareness is “super top of mind,” adding that OpenAI is prioritizing tests that put models against real codebases and objectives to make evaluations more like production use.

Access is limited to selected participants in OpenAI’s Daybreak Red program, which serves organizations conducting authorized defensive cybersecurity work. The company plans to expand access to all Daybreak Red customers in the coming weeks.

## Top comments (0)
