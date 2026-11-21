---
id: collect-261121-veille-ia/copilot-microsoft/microsoft-launches-mai-code-2-and-code-2-flash-coding-models
title: "Microsoft launches MAI Code 2 and Code 2 Flash coding models"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["code", "microsoft", "flash", "models", "coding", "cost", "performance", "benchmarks"]
source: https://glonce.com/microsoft-launches-mai-code-2-and-code-2/
source_anchor: ""
source_lines: [1, 41]
section: "Copilot_Microsoft"
---
Microsoft's MAI team has announced two coding models, MAI Code 2 and MAI Code 2 Flash, built for everyday enterprise coding tasks. The pitch is leading performance at a fraction of the cost of competing models. Every figure in the announcement is Microsoft's own.

MAI Code 2 is described as being at the frontier of price-performance for common enterprise coding tasks. Microsoft says it excels at taking code to production, deep debugging, multi-file implementation, repo-wide reasoning and UI work, and that it produces changes code maintainers would merge. MAI Code 2 Flash is aimed at high-volume work: code explanations, targeted edits, routine fixes and rapid iterations, with quality at a much lower cost. Microsoft is explicit about the limits. The models are not yet tuned for harder problems such as fundamental science research and very long-horizon agentic work.

The headline numbers come from an average over four benchmarks that target the everyday enterprise tasks Microsoft sees in GitHub Copilot: DeepSWE 1.1, Terminal Bench 2.1, SWE-Atlas and FrontierCode v1.1. On that average, MAI Code 2 gives 98% of the performance of GPT 6.1 Sol at 40% of the cost, and 96% of Claude Opus 5.5's performance at 5% of the cost. MAI Code 2 is also tuned for dashboards, visualizations and visual coding. In blind frontend app evaluations, reviewers preferred its results over GPT-6 Sol 64% of the time. For MAI Code 2 Flash, Microsoft claims comparable performance to GPT-6 Luna and Claude Haiku 5.5 at 65% and 20% of their cost respectively, across three coding benchmarks of day-to-day, high-volume routine tasks.

Microsoft also says MAI Code 2 is becoming the workhorse behind its own products. From Code 2 Flash it further optimized a single model, MAI Work Flash, to serve products across M365 and Fabric IQ. The claims by product: in Excel, the model is on par with GPT 6 Luna for accuracy and token efficiency on key internal customer-scenario benchmarks. In PowerPoint, it performs on par with GPT 6 Luna in both quality and token efficiency on key production benchmark scenarios. In Word, it performs similarly to GPT 6.1 sol (Microsoft describes its own model as significantly smaller), on 5 key internal benchmarks, using about 42% fewer input tokens per call than GPT6.1 Sol in production. In Teams, it beats GPT 6 Luna at tracking decisions, filling knowledge gaps and adapting custom instructions, using up to 28% fewer tokens on instruction following and tool calling. In Fabric IQ, offline evaluation runs show the MAI model outperforming GPT-5.6-sol, GPT-6-astra and Opus-5.5 at DAX query generation, the core capability behind "Chat with your Data" for Power BI semantic models.

On availability, MAI Code 2 and Code 2 Flash are available in GitHub Copilot, and Code 2 Flash is in limited-access preview in Microsoft Foundry. Microsoft says it will soon release the models on Open Router, Vercel, Hermes, Open Code and Kilo, with no date given. A Custom Models page covers customizing MAI Code 2. The announcement closes with a caution: results reflect evaluations on specific coding tasks and configurations, and performance and cost comparisons vary by workload, model configuration, reasoning effort and pricing assumptions.

## Key facts

- Averaged over four benchmarks (DeepSWE 1.1, Terminal Bench 2.1, SWE-Atlas, FrontierCode v1.1), MAI Code 2 reaches 98% of GPT 6.1 Sol's performance at 40% of the cost, and 96% of Claude Opus 5.5's at 5% of the cost.
- MAI Code 2 Flash is claimed to match GPT-6 Luna and Claude Haiku 5.5 at 65% and 20% of their cost, across three routine-coding benchmarks.
- In blind frontend app evaluations, reviewers preferred MAI Code 2's results over GPT-6 Sol 64% of the time.
- Both models are available in GitHub Copilot; Code 2 Flash is in limited-access preview in Microsoft Foundry; releases on Open Router, Vercel, Hermes, Open Code and Kilo are promised 'soon'.
- MAI Work Flash, a further-optimized version of Code 2 Flash, serves M365 and Fabric IQ products; in Word it uses about 42% fewer input tokens per call than GPT6.1 Sol in production.

## Why it matters

Microsoft is putting its own first-party coding models into GitHub Copilot and arguing they match the leading GPT and Claude models on everyday work at a small share of the cost. The sharpest claim is against Claude Opus 5.5: 96% of its performance at 5% of the cost. The announcement also ties the models to Microsoft's own products, with MAI Work Flash serving M365 and Fabric IQ.

## Who it affects

Developers and enterprises using GitHub Copilot, where both models are available now. Teams that run high-volume routine coding (explanations, targeted edits, routine fixes) are the target of Code 2 Flash. Users of Excel, PowerPoint, Word, Teams and Power BI's "Chat with your Data" are affected indirectly, since MAI Work Flash is said to serve those products. Developers on Open Router, Vercel, Hermes, Open Code and Kilo are promised access later.

## How to use it

MAI Code 2 and Code 2 Flash are available in GitHub Copilot. Code 2 Flash is also in limited-access preview in Microsoft Foundry. Releases on Open Router, Vercel, Hermes, Open Code and Kilo are planned with no date. Microsoft points to its Custom Models page for customizing MAI Code 2. No absolute prices (per token or otherwise) are given, only percentages of competitors' cost.

## How solid is it

All figures are Microsoft's own, and no independent verification is mentioned. No raw benchmark scores are given for any model, only relative percentages. The basis for the cost percentages (pricing assumptions, reasoning effort, token counts) is not specified, and the footnotes referenced for the main claims have no text in the source. The comparison model is called both 'GPT 6.1 Sol' and 'GPT-6 Sol' without clarification of whether they are the same model. Microsoft itself says comparisons vary by workload, configuration, reasoning effort and pricing assumptions.

## Risks and caveats

Microsoft says the models are explicitly not yet tuned for fundamental science research or very long-horizon agentic work, so the claims cover everyday coding only. Many product comparisons rest on internal benchmarks and offline evaluation runs, and several are 'on par' claims rather than wins. The 28% token saving in Teams is a maximum ('up to'), and the 42% input-token saving in Word is approximate. The source does not say which model sizes, architecture or training data underlie the models. Availability is partial: Code 2 Flash is a limited-access preview in Foundry, and the third-party releases are only promised.

“MAI Code 2 is at the frontier of price-performance for common enterprise coding tasks”

— Microsoft, MAI Code 2 announcement
