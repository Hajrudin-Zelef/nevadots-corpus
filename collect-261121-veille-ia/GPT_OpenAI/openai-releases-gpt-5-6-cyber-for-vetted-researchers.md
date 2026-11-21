---
id: collect-261121-veille-ia/gpt-openai/openai-releases-gpt-5-6-cyber-for-vetted-researchers
title: "OpenAI Releases GPT-5.6-Cyber for Vetted Researchers"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: ["2026-08-10"]
keywords: ["gpt-5", "cyber", "model", "openai", "daybreak", "security", "standard", "cybersecurity"]
source: https://blog.evolvedigital.ai/openai-launches-gpt-5-6-cyber-the-first-offense-grade-ai-model-built-for-security-professionals/
source_anchor: ""
source_lines: [1, 41]
section: "GPT_OpenAI"
---
OpenAI released GPT-5.6-Cyber on August 10, 2026, marking the first time the company has shipped a model purpose-trained for offensive cybersecurity research. The model is available exclusively through the Daybreak Red program, a tightly controlled access tier designed for vetted security professionals and authorized red-team operators. The launch signals a meaningful shift in how frontier AI labs approach dual-use capabilities, moving from general-purpose guardrail removal toward domain-specific models built with security practitioners as the primary audience.

## What Was Announced

GPT-5.6-Cyber is built on top of GPT-5.6 Sol, OpenAI’s current frontier model, and has been fine-tuned specifically for cybersecurity workflows. The model is trained to find zero-day vulnerabilities, develop exploit chains, and assist with red-team operations, tasks that standard production models decline or handle poorly because of safety restrictions. GPT-5.6-Cyber is designed to reduce those refusals for authorized practitioners working within approved-use constraints.

Access to the model is exclusively through the Daybreak Red program. Applicants, both individuals and organizations, must pass identity verification, meet account security requirements, complete legal attestations, and receive OpenAI’s direct approval before access is granted. Initial launch partners include Accenture, IBM, CrowdStrike, Cloudflare, and Palo Alto Networks, all participants in OpenAI’s Daybreak Cyber Partner Program.

OpenAI has not published pricing for GPT-5.6-Cyber. The company’s rate card shows blank values for the Cyber tier, and all access currently runs through the Daybreak Red application process rather than a standard API endpoint with a published model ID. Beginning September 1, 2026, hardware security keys will be mandatory for all Daybreak Red accounts.

Separately, the Daybreak Blue tier, which removes guardrails from standard GPT-5.6 Sol, remains available for defenders who need broader uplift without the specialized offensive tooling of the Cyber model. OpenAI describes Blue as the recommended starting point for most security teams.

## Technical Details

On OpenAI’s internal Advanced Cybersecurity Completion Rate evaluation, GPT-5.6-Cyber achieves a 95.0% completion rate on advanced security prompts. The standard GPT-5.6 Sol model scores 1.5% on the same benchmark. OpenAI notes that this metric measures how often the model responds, not the accuracy or correctness of the output, a distinction the company highlighted to contextualize the numbers.

GPT-5.6-Cyber outperforms its predecessor GPT-5.5-Cyber, which achieved a 57.3% completion rate on the same evaluation. The new model performs well on the ExploitGym benchmark for exploit development but scores lower than standard Sol on vulnerability report writing and shows worse token efficiency on ExploitBench under standard 300-turn settings. OpenAI describes these tradeoffs as expected given the model’s specialization.

Real-world results have been demonstrated through the Daybreak program. Researchers using GPT-5.6-Cyber discovered two previously unknown, chained vulnerabilities in V8, the JavaScript engine at the core of Google Chrome. Google has patched both issues, which are assigned CVE-2026-15903. Additional research using the model uncovered more than 400 privilege-escalation vulnerabilities across mobile operating systems, databases, and kernel subsystems. OpenAI has classified GPT-5.6-Cyber as “High” for cybersecurity capability, the second-highest tier in its internal risk framework, below the “Critical” designation assigned to the still-unreleased Astra model.

## Industry Impact and Reactions

The launch of GPT-5.6-Cyber is notable because it is OpenAI’s clearest acknowledgment yet that frontier AI models have genuine offensive utility in cybersecurity, and that the company intends to channel that utility toward vetted defenders rather than attempt to suppress it entirely. The Daybreak Red program represents a controlled distribution model rather than a blanket restriction, and the partnership structure with firms like CrowdStrike and Palo Alto Networks integrates GPT-5.6-Cyber directly into established security toolchains.

The CVE discoveries have drawn attention from the broader security research community. Finding two chained zero-days in V8 and a portfolio of over 400 privilege-escalation bugs using a single model in a structured research engagement is a concrete demonstration of capability that goes beyond benchmark numbers. Security researchers have noted that the volume and speed of vulnerability discovery enabled by the model changes the economics of offensive security research in ways that will require defensive teams to adapt.

The mandatory hardware security key requirement starting September 1 reflects the sensitivity of the access tier. OpenAI’s decision to enforce strong authentication at the account level, rather than relying solely on legal attestations and application screening, positions Daybreak Red as a regulated access program comparable in rigor to certain government and defense contractor tooling agreements.

## What Comes Next

OpenAI has indicated that the Daybreak program will expand access to additional vetted partners through the remainder of 2026. The company has not announced a timeline for making GPT-5.6-Cyber available through a public API endpoint or for publishing pricing. The September 1 hardware key mandate is the next firm date in the program’s rollout calendar.

The still-unreleased Astra model, which OpenAI rates as “Critical” for cybersecurity capability, remains on an undisclosed timeline. Astra’s existence and its placement above GPT-5.6-Cyber on the risk scale suggests OpenAI is already managing a more capable model internally and developing a corresponding access framework before any release. How OpenAI structures that program, and whether the Daybreak Red model scales to Astra-level capability, will be among the more consequential AI safety and access decisions of the coming months.

## Conclusion

GPT-5.6-Cyber is a significant step in the maturation of AI-assisted security research. By building a model specifically for offensive workflows and distributing it through a tightly controlled partner program, OpenAI is making a deliberate bet that purpose-built access controls are more effective than capability suppression. The real-world vulnerability discoveries already produced by the model validate the core premise, and the framework it establishes will likely shape how other frontier AI labs approach dual-use security tooling in the months ahead.

*Stay updated on the latest AI news at Evolve Digital.*

## Leave a Reply
