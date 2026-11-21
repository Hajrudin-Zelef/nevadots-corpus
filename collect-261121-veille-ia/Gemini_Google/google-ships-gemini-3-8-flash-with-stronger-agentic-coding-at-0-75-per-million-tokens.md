---
id: collect-261121-veille-ia/gemini-google/google-ships-gemini-3-8-flash-with-stronger-agentic-coding-at-0-75-per-million-tokens
title: "Google ships Gemini 3.8 Flash with stronger agentic coding at $0.75 per million tokens"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["flash", "gemini", "model", "cyber", "models", "google", "million", "tokens"]
source: https://gcn.com/google-ships-gemini-flash-stronger-agentic/21390/
source_anchor: ""
source_lines: [1, 47]
section: "Gemini_Google"
---
Google shipped Gemini 3.8 Flash on September 2, 2026, pricing the model at $0.75 per million input tokens and $3.75 per million output tokens, the same introductory rate the company used for Gemini 3.7 Flash just three weeks earlier.

The launch, announced just three weeks after the previous Flash release, marks Google’s third Flash release in six weeks , continuing a release cadence that has made the Flash line resemble a rolling delivery train rather than a series of discrete product events.

## A workhorse built for agentic loops

Google calls Gemini 3.8 Flash “our most intelligent Flash model,” built for long-horizon software engineering, autonomous agents, and multi-step enterprise reasoning. The model delivers substantial gains from 3.7 Flash, often approaching the performance of higher-cost frontier models; on DeepSWE v1.1 it outperforms most larger frontier models in autonomously solving complex engineering problems end to end, at a fraction of the cost, and exhibits the dependability required for critical enterprise autonomy across specialized knowledge domains.

The Flash line bridges the gap between deep-reasoning Pro models and high-throughput Flash-Lite models while delivering high token efficiency and multi-step multimodal processing.

Performance gains stem from a core design choice: 3.8 Flash works harder. On complex tasks the model performs additional reasoning steps and calls tools iteratively, which can increase token usage, particularly at higher effort levels.

Developers have control over the thinking budget through low, medium, and high thinking levels to balance latency, token usage, and output quality based on the task.

Developers prioritizing compute efficiency can lower the effort setting or continue using 3.7 Flash, which remains fully supported.

The model arrives as developers are already navigating shifting usage policies across Google’s AI suite. Gemini Notebook moved to compute-based limits on September 2, the same day as the 3.8 Flash launch.

## Benchmark gains and pricing window

On Google’s announcement table, 3.8 Flash scored 73.7% on DeepSWE v1.1 against 65.3% for 3.7 Flash, 89.4% on Terminal-bench 2.1 against 85.8%, and 59.0% on OSWorld-2.0 against 50.6%.

The model also achieves 54.9% on HLE-Verified, demonstrating its ability to handle multi-step reasoning across STEM, humanities, and professional fields.

The introductory price of $0.75/$3.75 per million input/output tokens expires on December 31, 2026, after which it will rise to $1.50/$7.50.

The pricing matters to developers building high-volume pipelines, particularly those evaluating how AI inference costs fit into broader infrastructure budgets, a concern that now sits alongside context cap changes that have already caught some coding tool users off guard.

## Gemini 3.8 Flash Cyber and the Fairwind Program

Google has launched the Fairwind Program to provide governments, critical infrastructure organizations, and software maintainers with access to Gemini 3.8 Flash Cyber, a version of the model tuned for software vulnerability hunting and remediation.

The Chrome Security team found that 3.8 Flash Cyber produced 2.6 times more correct patches to vulnerabilities in Chrome than the best commercial models that are much larger.

Wiz found that Gemini 3.8 Flash Cyber achieves +7.5, 9.7% higher recall on their internal penetration testing benchmark for a 2.3, 5.2x lower cost compared to other leading frontier models.

Google’s Cloud Vulnerability Research team leveraged the 3.8 Flash Cyber model to find a critical foundational vulnerability in less than 2 hours, a discovery that would ordinarily take months.

On CyberGym, the standard industry benchmark for finding vulnerabilities, Gemini 3.8 Flash Cyber demonstrates frontier-level performance in autonomous vulnerability discovery, surpassing both 3.5 Flash Cyber and significantly larger frontier models. Access to the Cyber variant is limited to applicants approved through the Fairwind Program, while standard 3.8 Flash is open to everyone via the Gemini API and Google AI Studio.

## Where developers can access it

Gemini 3.8 Flash is available for developers through Google Antigravity, the Gemini API in Google AI Studio and Android Studio, and Stitch. Enterprises can use Gemini Enterprise. Consumers with Google AI Pro and Ultra subscriptions can access the model via the Gemini app, AI Mode in Google Search, and Gemini in Google Sheets.

Google released Gemini 3.8 Flash on September 2, 2026, alongside a restricted cybersecurity variant, Gemini 3.8 Flash Cyber. By Google’s own count it is the third Flash release in six weeks, and the headline item is the thing that did not move: the price.

For enterprises already on 3.7 Flash, the migration path carries the same API surface and pricing, with the model ID as the only required change according to Google’s official announcement.
