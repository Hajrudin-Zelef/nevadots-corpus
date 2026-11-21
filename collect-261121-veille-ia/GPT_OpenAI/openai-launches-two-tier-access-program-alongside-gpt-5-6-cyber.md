---
id: collect-261121-veille-ia/gpt-openai/openai-launches-two-tier-access-program-alongside-gpt-5-6-cyber
title: "OpenAI Launches Two-Tier Access Program Alongside GPT 5.6 Cyber"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["gpt5", "cyber", "openai", "daybreak", "tasks", "access", "models", "vulnerability"]
source: https://www.infosecurity-magazine.com/news/openai-daybreak-blue-red-gpt-cyber/
source_anchor: ""
source_lines: [1, 43]
section: "GPT_OpenAI"
---
OpenAI is launching GPT‑5.6‑Cyber, its newest large langue model (LLM) purpose-trained for cybersecurity tasks, based on its latest frontier AI model, GPT‑5.6 Sol.

The AI company also tweaked its Daybreak program to introduce two new tiers, Daybreak Blue – for defensive cyber tasks – and Daybreak Red – for more advanced defensive tasks as well as offensive cyber tasks.

From now on, members of Daybreak Blue will be granted access to frontier general-purpose models, including GPT‑5.6 Sol, with what OpenAI called “system-level safeguards” to perform authorized defensive security work.

This includes vulnerability discovery, secure code review, malware analysis, incident response and patch validation.

Meanwhile, members of Daybreak Red will also have access to purpose-trained cybersecurity models like GPT‑5.5‑Cyber and GPT‑5.6‑Cyber for more advanced cybersecurity tasks, such as vulnerability research, exploit validation and security testing.

## 
	**GPT‑5.6 Sol with Daybreak Blue, GPT‑5.6‑Cyber with Daybreak Red**

In a blog announcing GPT‑5.6‑Cyber and the two-tier system, published on August 10, OpenAI said GPT‑5.6 Sol “delivers state-of-the-art performance on cybersecurity tasks.”

Nevertheless, the AI company has deployed system-level safeguards to “screen cybersecurity-related requests to prevent misuse,” which could “also block legitimate defensive work.”

While general users of GPT‑5.6 Sol will have these guardrails turned on, Daybreak Blue access removes them. OpenAI noted that even without these added guardrails, GPT‑5.6 Sol will still refuse to comply to some “highly dual-use cybersecurity prompts (e.g. pentesting production systems).”

This is where GPT‑5.6‑Cyber, only available through Daybreak Red, comes into play.

## 
	**GPT‑5.6‑Cyber Outperforms All OpenAI Models At Security Tasks**

OpenAI claimed this new cyber-focused AI model further reduces refusals and improves performance on certain security tasks, completing 95% of a set of sensitive requests, involving exploit-chain development, authentication bypass, privilege escalation and other advanced cybersecurity scenarios.

General-access GPT‑5.6 Sol completed just 1.5% of the same tasks and 2.0% when used with Daybreak Blue access. GPT‑5.5‑Cyber, OpenAI’s previous cyber-focused model, completed only 57.3% of the same requests.

OpenAI also compared GPT‑5.6‑Cyber with general-access GPT‑5.6 Sol, Daybreak Blue-access GPT‑5.6 Sol and GPT‑5.5‑Cyber on cybersecurity-specific AI benchmarks – like ExploitGym and ExploitBench – and a series of tasks, such as zero-day vulnerability discovery evaluation, vulnerability reporting. It outperformed all the other OpenAI models at all tasks.

The AI company also said it used GPT‑5.6‑Cyber to find CVE-2026-15903, a high-severity vulnerability in V8, Chrome’s JavaScript engine.

“Our researchers validated the findings and reported them to Google through coordinated vulnerability disclosure. Google fixed the vulnerability,” said the OpenAI blog.

Commenting on the launch, Alex Goller, principal solution architect for EMEA at Illumio, described the introduction of the two-tier system as “a good first step for OpenAI to mitigate two issues.”

“Daybreak Red restricts models from doing more harm than they should and limits people's exposure to knowing what the models do, while Daybreak Blue fixes the gap that left Hugging Face's responders unable to use frontier models during their incident,” he said.

However, he highlighted that AI model guardrails were never the control plane for defense.

“These are agents operating inside your environment and the controls that matter follow zero trust principles. There must be visibility into what the agent is doing and what it can reach, and then segmentation to contain the blast radius when something goes wrong,” he explained, praising OpenAI's own guidance recommending sandboxing and scoped authorization.

“However, that enforcement lives in your infrastructure, not in the model,” he added.
