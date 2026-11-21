---
id: collect-261121-veille-ia/copilot-microsoft/microsoft-releases-coding-ai-models-mai-code-2-and-mai-code-2-flash-in-github-copilot-says-they-near
title: "Microsoft releases coding AI models 'MAI Code 2' and 'MAI Code 2 Flash' in GitHub Copilot, says they near top models' scores at lower cost in its own evaluation"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["code", "microsoft", "models", "cost", "opus", "coding", "flash", "copilot"]
source: https://ai-oriens.com/en/news/microsoft-mai-code-2-launch/
source_anchor: ""
source_lines: [1, 55]
section: "Copilot_Microsoft"
---
## Two models for everyday coding

Microsoft's AI division, Microsoft AI, announced on its official blog on Nov. 17 U.S. time (Nov. 18 Japan time) the programming AI model "MAI Code 2" and the smaller "MAI Code 2 Flash." It positions both for the coding work that enterprise developers do every day and says they deliver comparable performance to rival models at lower cost. The models are available in the "GitHub Copilot" coding assistant.

MAI Code 2 is strong at taking code to production, deep debugging, multi-file implementation, reasoning across an entire repository and building user interfaces (UI), the company said. MAI Code 2 Flash is aimed at high-volume work such as code explanations, targeted edits, routine fixes and rapid iterations. The company explicitly states the models are "not yet tuned for harder problems," such as fundamental science research and very long horizon agentic work.

The MAI Code 2 name had been reported as spotted in a developer preview before the official announcement. In a roundup of announcements timed to its annual "Microsoft Ignite 2026" event on the same day, Microsoft also listed the image-editing model "MAI Image 2.7," "Dragon Copilot Transcribe" built on the "MAI Transcribe 2" transcription model, "Decision 1.1," which routes requests between models, and its in-house AI chip "Maia 200."

## Average score close to Sol and Opus at lower cost per task, Microsoft says

According to Microsoft, averaged over four benchmarks (DeepSWE 1.1, Terminal Bench 2.1, SWE-Atlas and FrontierCode v1.1), MAI Code 2 delivered 98% of the performance of OpenAI's "GPT-6.1 Sol" at 40% of the cost. Against Anthropic's "Claude Opus 5.5," it delivered 96% of the performance at 5% of the cost, the company said. Microsoft describes the four as benchmarks targeting the everyday enterprise coding tasks it sees in GitHub Copilot.

According to the chart on the blog, the average pass rate was 63.2% for MAI Code 2, 64.1% for GPT-6.1 Sol and 65.3% for Claude Opus 5.5. The average cost per task was $0.309 for MAI Code 2, $0.761 for GPT-6.1 Sol and $6.306 for Claude Opus 5.5. MAI Code 2 had the lowest average score of the three models.

Microsoft set all of the conditions. Every model ran at maximum reasoning effort, and DeepSWE, Terminal Bench and SWE Atlas were measured on Microsoft's internal evaluation platform using the command-line version of GitHub Copilot without subagents that split up the work. FrontierCode scores and costs were taken from the evaluation site Artificial Analysis, the company said. MAI Code 2's cost was calculated at a 50% promotional price, and Microsoft has not disclosed a comparison at the regular price or when the promotion ends.

To test UI-building ability, Microsoft had the models build games, data visualizations, 3D scenes and other output, and asked people to compare MAI Code 2 and OpenAI's "GPT-6 Sol" results without knowing which model made them. The share of 258 reviewers who preferred MAI Code 2 on both aesthetics and functionality is 64% on both the blog and the same-day roundup. Pages saved around 4 a.m. Japan time on Nov. 18 showed 67% on the blog and 68% in the roundup, but both have since been changed to 64%.

## Trails Sol and Opus on SWE Atlas

Benchmark by benchmark, MAI Code 2 did not rank first among the three models on any of the four. On FrontierCode v1.1 it scored 52.0%, above GPT-6.1 Sol (49.4%) but below Claude Opus 5.5 (56.0%). On DeepSWE 1.1 and Terminal-Bench 2.1, it was within 1.4 points of GPT-6.1 Sol.

The widest gap was on SWE Atlas, where MAI Code 2 scored 43.1% against 47.4% for GPT-6.1 Sol and 57.5% for Claude Opus 5.5. MAI Code 2 had the lowest cost per task on all four benchmarks; on DeepSWE 1.1 it cost $0.618 against $10.717 for Claude Opus 5.5.

## Smaller Flash beats Luna and Haiku on three tasks

For MAI Code 2 Flash, Microsoft said that across three benchmarks (DeepSWE 1.1, Terminal Bench 2.1 and SWE-Atlas), it offers comparable performance to OpenAI's "GPT-6 Luna" and Anthropic's "Claude Haiku 5.5" at 65% and 20% of the cost, respectively. The model page says it costs one fifth as much as Claude Haiku 5.5 with 12% higher performance.

According to the chart on the blog, the average pass rate was 61.0% for MAI Code 2 Flash, 55.1% for GPT-6 Luna and 53.9% for Claude Haiku 5.5. The average cost per task was $0.052, $0.08 and $0.319, respectively. MAI Code 2 Flash scored highest of the three models on all three benchmarks. Conditions were the same as for the larger model, and Flash's cost was also calculated at a 50% promotional price.

## Available in GitHub Copilot, no prices given

MAI Code 2 and MAI Code 2 Flash are available in GitHub Copilot. MAI Code 2 Flash is also offered as a limited-access preview in "Microsoft Foundry," the company's enterprise AI development platform. Microsoft said it will soon release it on Open Router, Vercel, Hermes, Open Code and Kilo.

Neither the blog nor the model page lists per-million-token prices. Microsoft has not disclosed the original price behind the "50% promotional price" used in the cost comparisons, or how long the promotion runs. ORIENS could not confirm which GitHub Copilot plans include the models.

## Derived model for Office and Fabric

Microsoft said "MAI Work Flash," a model further tuned from MAI Code 2 Flash, will serve products across Microsoft 365 and Fabric IQ. In the company's internal evaluations, it matched OpenAI's "GPT 6 Luna" in results and token efficiency on Excel and PowerPoint tasks. In Word, it performed close to "GPT 6.1 Sol" while using about 42% fewer input tokens per call in production, the company said. In Teams, it used up to 28% fewer tokens on instruction following and tool calling.

Among customer cases, AT&T built a custom MAI model for 5G network operations analytics that outperformed GPT-6.1 Sol at one fiftieth of the cost, according to Microsoft. Woven by Toyota tuned MAI Code 2 Flash on its history of fixes under "MISRA," a coding standard for automotive software. In 44% of cases where fixes from the frontier models it runs in production needed human rework, the tuned model produced ready-to-merge fixes at less than one tenth of the cost, it said.

## "MAI Cyber 2 Flash" for vulnerability scanning

On the same day, Microsoft also announced "MAI Cyber 2 Flash," a model that searches for software vulnerabilities. It succeeds October's "MAI-Cyber-1.1-Flash" and has a 1 million-token context window, four times that of the previous version.

In the company's evaluation, it scored 92.3% on CyberGym-E2E, a benchmark for discovering real-world vulnerabilities, matching OpenAI's "GPT-6 Astra" (91.5%) with half the reasoning budget. Its cost per task was less than 2% of GPT-6 Astra's, the company said. Its price per million tokens is $0.10 for input, $0.01 for cached input and $0.50 for output, which Microsoft lists alongside $10, $1 and $50 for GPT-6 Astra (Azure OpenAI) used in the comparison. Microsoft said it excluded 92 tasks that overlapped with its training data and reported results on the remaining 813 tasks. The model page, however, shows a CyberGym-E2E score of 83.2%, which differs from the blog's figure.

MAI Cyber 2 Flash will be offered only in Microsoft security products. In the vulnerability-scanning system "Codename MDASH," it will automatically replace the existing model for eligible customers starting in mid-December. Microsoft said it plans to share comparisons with OpenAI's cyber-defense model "GPT-6 Cyber" in the coming weeks.

## "MAI Realtime 1" for voice conversations

Microsoft also announced "MAI Realtime 1," its first model that listens to speech and answers with speech. Pricing starts at $0.01 per audio minute, with reasoning billed separately at $1 per million input tokens and $5 per million output tokens. In the company's evaluation, it outperformed OpenAI's "GPT Realtime 2.1" on quality, speed and price.

It is compatible with OpenAI's Realtime API, making it easier to migrate existing voice applications, the company said. Microsoft plans to bring it to the customer-service product "Dynamics 365 Contact Center" and the healthcare product "Dragon Copilot," and Agoda and Reservations.ai are testing it. It can be tried on the company's trial site "MAI Labs," and customers can register for a limited-access preview in Microsoft Foundry; preview customers will not be billed until public preview begins.
