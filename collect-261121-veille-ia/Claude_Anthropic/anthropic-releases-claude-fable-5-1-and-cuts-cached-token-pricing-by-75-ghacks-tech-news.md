---
id: collect-261121-veille-ia/claude-anthropic/anthropic-releases-claude-fable-5-1-and-cuts-cached-token-pricing-by-75-ghacks-tech-news
title: "Anthropic Releases Claude Fable 5.1 and Cuts Cached Token Pricing by 75% - gHacks Tech News"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-09-03"]
keywords: ["fable", "anthropic", "model", "claude", "company", "pricing", "mythos", "tasks"]
source: https://www.ghacks.net/2026/09/03/anthropic-releases-claude-fable-5-1-and-cuts-cached-token-pricing-by-75/
source_anchor: ""
source_lines: [1, 60]
section: "Claude_Anthropic"
---
Anthropic has launched Claude Fable 5.1, its latest flagship model, which is now available through the API, cloud platforms, and the Claude desktop app. The company also introduced Mythos 5.1, a version of the same model set aside for approved cybersecurity and life-sciences organizations.

Both models are aimed at tasks that take hours to complete, not just sec onds. While Anthropic kept Fable 5.1's standard API rates unchanged, it cut the price of cache reads by 75%. According to the company, this could lower effective costs by up to 45% for applications that rely heavily on agents.

Fable 5.1 is widely available and includes safeguards. Mythos 5.1 has fewer restrictions but is only accessible to a limited group.

## Claude Fable 5.1 and Mythos 5.1 Versions and Benchmark Results

Fable 5.1 is the main version and includes safeguards that block certain high-risk tasks in cybersecurity and scientific research. Mythos 5.1 removes many of these restrictions, but only organizations in Anthropic's trusted-access program can use it.

The update is aimed at customers who use AI for larger assignments, not just single prompts or code requests. Anthropic says Fable 5.1 is better at handling software projects, reviewing code across entire applications, troubleshooting issues in external libraries, and managing complex scientific tasks.

It can also help design experiments, model outcomes, and interpret detailed tables and diagrams. The model is built to get more work done with fewer tokens, which matters for companies running AI agents over long periods.

Anthropic reported gains on several evaluations. Fable 5.1 scored 55.8% on Terminal-Bench 4.0, a benchmark for command-line coding work, compared with 42% for Fable 5. Mythos 5.1 scored 60.9% under its more permissive safety settings.

On Terminal-Bench-Science 0.1, which measures agentic scientific research, Anthropic reported 52.6% for Fable 5.1, more than double Fable 5's 24.7%. The company also reported gains in business-workflow automation, knowledge work, and browser-agent tasks.

Tthese results are Anthropic's own and should not be treated as independent proof that the model beats every competing system, though they align with the company's claim that Fable 5.1 is better suited to work requiring a model to track a problem, use tools, and revisit earlier steps.

Some early customers described that kind of use. Millennium said Fable 5.1 found the cause of a rare software crash in an outside vendor's library after the issue had gone unexplained for four to five years. Ramp said it ran the model for 38 hours on a machine-learning task, during which it revisited a previous conclusion, launched six experiments, and produced findings and recommendations.

## Claude Fable 5.1 Pricing, Data Retention, and Safety Changes

Anthropic did not cut Fable 5.1's standard API rates, which remain $10 per million input tokens and $50 per million output tokens, the same as Fable 5. Instead, the company cut the price of cache reads by 75%. Reading cached input now costs $0.25 per million tokens, down from $1 per million.

Cache writes remain $12.50 per million tokens for a five-minute cache, and this pricing change is important for agentic workloads, where agents often resend the same information to the model, such as code repositories, system instructions, technical documentation, tool specs, and records of previous work.

Anthropic says cached material can make up half or more of token use in some long-running tasks. The company expects the new pricing to cut the effective cost of running Fable 5.1 by about 25% for typical workloads and up to 45% for applications that rely heavily on agents.much as 45% for heavily agentic applications.

Fable remains expensive compared with Anthropic's Opus and Sonnet models and many competitors, but the company is betting businesses will judge it on the cost of completing a difficult assignment rather than the release also brings changes to how Anthropic handles data retention.

Starting this fall, the company plans to offer Enterprise Frontier Safeguards, which will let customers keep monitoring data on their own infrastructure while still using Anthropic's misuse-detection systems.

This marks a shift from Anthropic's earlier stance that data retention was needed for advanced models due to cybersecurity risks. With the new setup, customers get more control over their data and logs, while Anthropic's safety controls stay in place. controls remain active.

Anthropic says it has improved the classifiers that decide whether to block a model request, which should mean fewer false positives and less disruption for legitimate cybersecurity and biology work.

This became more important after Anthropic and the UK AI Security Institute found that earlier Claude models, when tested under very permissive cybersecurity settings, had taken unauthorized actions against real systems. Anthropic paused external cyber evaluations for a time and added more containment and monitoring before resuming them.

## Claude Fable 5.1 Watermarking and Availability

Fable 5.1 and Mythos 5.1 are the first new Claude models to include invisible watermarks in text and file outputs. Anthropic had said it would add watermarking to models released after August 2, 2026, in line with the EU AI Act.

The watermarks influence word choice when there are several plausible options, so they stay detectable even after copying, pasting, or light editing, but remain invisible to readers.

Anthropic says the watermark does not affect the quality or content of a response and does not include any information about the user, organization, or conversation.

The detection API is available to regulators, law enforcement, media, fact-checkers, independent researchers, and other eligible groups. A positive result shows that Claude wrote or processed the content, but if there is no watermark, it does not prove a human wrote it.

Claude Fable 5.1 is now available through Anthropic's API, cloud platforms, and the Claude desktop app. Mythos 5.1 is only available to organizations in Anthropic's trusted-access program for approved cybersecurity and life-sciences use.

The Enterprise Frontier Safeguards data retention option is expected this fall, but Anthropic has not announced a specific date. The lower cache read pricing is already in effect, while standard input and output rates have not changed.

Peppesaid on
EU contribution to the most advanced technology in the world.

Watermarks requirements.

What a joke these bureaucrats are.
