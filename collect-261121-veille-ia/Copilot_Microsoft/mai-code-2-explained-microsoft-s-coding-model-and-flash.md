---
id: collect-261121-veille-ia/copilot-microsoft/mai-code-2-explained-microsoft-s-coding-model-and-flash
title: "MAI Code 2 Explained: Microsoft's Coding Model and Flash"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-11-18"]
keywords: ["code", "microsoft", "flash", "model", "coding", "work", "github", "copilot"]
source: https://www.layer3labs.io/guides/mai-code-2-explained
source_anchor: ""
source_lines: [1, 116]
section: "Copilot_Microsoft"
---
# What Is MAI Code 2? Microsoft's Coding Model and Code 2 Flash

A breakdown of Microsoft's coding models, reported benchmark conditions, partner implementations, and current GitHub Copilot availability.

MAI Code 2 is Microsoft AI's enterprise coding model and Code 2 Flash is its cheaper high-volume variant, announced November 17, 2026 and available in GitHub Copilot.

Microsoft offers a limited-access preview of Code 2 Flash in Microsoft Foundry, but has not published a dollar price or context window size on its product pages as of November 18, 2026.

For a broader overview of the model family, read our guide to Microsoft MAI models.

## What MAI Code 2 Is and Where You Can Use It

MAI Code 2 is an enterprise coding model from Microsoft AI designed to carry software projects into production. Microsoft announced the model alongside Code 2 Flash on November 17, 2026. Microsoft says both models are in GitHub Copilot.

Code 2 Flash is also available through a limited-access preview in Microsoft Foundry. Microsoft has not published standard commercial dollar pricing or context window specifications for either model on its announcement or model pages as of November 18, 2026.

The model family addresses two distinct operational workloads. MAI Code 2 handles multi-file implementation, repository-wide reasoning, deep debugging, and user interface creation. Code 2 Flash handles high-volume tasks such as targeted code edits, rapid fixes, and code explanations.

- Maker: Microsoft AI.
- Announcement date: November 17, 2026.
- Current availability: Live inside GitHub Copilot for immediate developer use.
- Preview access: Limited-access preview for Code 2 Flash hosted in Microsoft Foundry.
- Primary use cases: Production code generation, deep debugging, repository analysis, and UI engineering.
- Stated non-targets: Microsoft explicitly notes neither model is tuned for fundamental science research or very long-horizon agentic work.

## MAI Code 2 vs Code 2 Flash

Microsoft separates MAI Code 2 and Code 2 Flash by their target workloads. MAI Code 2 is built for everyday enterprise coding, multi-file implementation, repo-wide reasoning, deep debugging, and UI work. Code 2 Flash targets high-volume routine coding work including code explanations, targeted edits, routine fixes, and rapid iteration.

| Model | Job Microsoft Targets | Where It Is Available | What It Is Not Tuned For | 
|---|---|---|---|
| MAI Code 2 | Taking code to production, deep debugging, multi-file implementation, repo-wide reasoning, and UI work | GitHub Copilot | Fundamental science research and very long-horizon agentic work | 
| Code 2 Flash | High-volume routine work, code explanations, targeted edits, routine fixes, and rapid iteration | GitHub Copilot and limited-access preview in Microsoft Foundry | Fundamental science research and very long-horizon agentic work | 

Choosing between the two depends on whether an engineering task requires broad repository reasoning or rapid, targeted code assistance. Code 2 Flash is meant for high-volume routine coding work where rapid iteration and lower cost are priorities. Both systems share common boundaries, as Microsoft notes neither model is tuned for fundamental science research or very long-horizon agentic work.

## Microsoft's Reported Benchmarks and Their Conditions

Microsoft reports its benchmark achievements using relative performance percentages rather than publishing complete academic evaluation tables. According to Microsoft AI, MAI Code 2 reaches 98 percent of GPT 6.1 Sol performance at 40 percent of its cost, and 96 percent of Claude Opus 5.5 performance at 5 percent of its cost. Microsoft notes these cost figures reflect a 50 percent promotional price.

The benchmark comparisons rely on specific testing configurations that developers must keep in mind. Microsoft states that these performance metrics represent an average across four specific benchmarks: DeepSWE 1.1, Terminal Bench 2.1, SWE-Atlas, and FrontierCode v1.1. Microsoft ran all tests at maximum reasoning effort and explicitly notes that results vary.

For user interface and visual tasks, Microsoft conducted a blind frontend evaluation involving 258 human annotators. In that evaluation, annotators preferred MAI Code 2 over GPT-6 Sol 64 percent of the time. External model comparisons involve models from OpenAI and Anthropic.

| Comparison Model | Reported Metric | Reported Value | Condition | 
|---|---|---|---|
| GPT 6.1 Sol | Relative performance | 98 percent | Averaged across 4 benchmarks at max reasoning effort | 
| GPT 6.1 Sol | Relative cost | 40 percent | Shown at a 50 percent promotional price | 
| Claude Opus 5.5 | Relative performance | 96 percent | Averaged across 4 benchmarks at max reasoning effort | 
| Claude Opus 5.5 | Relative cost | 5 percent | Shown at a 50 percent promotional price | 
| GPT-6 Sol | Blind frontend preference | 64 percent win rate | Evaluated by 258 annotators | 
| GPT-6 Luna and Claude Haiku 5.5 | Routine coding performance | Comparable performance | Reported in announcement on three routine-coding benchmarks | 
| Claude Haiku 5.5 | Code 2 Flash cost relative to Claude Haiku 5.5 | About one-fifth (20 percent) | Reported in announcement at 50 percent promotional price | 
| Claude Haiku 5.5 | Code 2 Flash performance relative to Claude Haiku 5.5 | 12 percent higher performance | Reported on the MAI Code 2 model page | 

## Partner Implementations and Custom Results

Microsoft's model page highlights customized deployments of Code 2 Flash. These enterprise metrics represent custom or fine-tuned versions of the model rather than standard out-of-the-box base performance. Organizations reviewing these numbers must account for the specialized training involved in each pilot.

AT&T used a custom Code 2 Flash model for 5G network analysis. Microsoft reports that AT&T's custom model beat GPT-6.1 Sol on 5G network analysis at 50x lower cost.

Woven by Toyota used a fine-tuned Code 2 Flash model to produce ready-to-merge MISRA fixes. Microsoft states this specialized implementation operated at under one-tenth the cost.

- AT&T custom deployment: Custom Code 2 Flash beat GPT-6.1 Sol on 5G network analysis at 50x lower cost.
- Woven by Toyota fine-tuning: Fine-tuned Code 2 Flash produced ready-to-merge MISRA code fixes in 44 percent of cases.
- Woven by Toyota cost efficiency: Woven by Toyota's fine-tuned model operated at under one-tenth the cost.
- Customization scope: Both results reflect partner fine-tuning and domain adaptation, not standard off-the-shelf model behavior.

## Where MAI Code 2 Is Available Now and Next

Microsoft says both models are in GitHub Copilot. The Microsoft AI model page features a direct link to try the system within the Copilot interface.

Code 2 Flash is also available in a limited-access preview in Microsoft Foundry. Beyond its first-party channels, Microsoft plans to expand model distribution to third-party developer platforms. Microsoft says it will soon release the models on OpenRouter, Vercel, Hermes, Open Code, and Kilo.

Microsoft has not published specific release dates or regional availability schedules for these third-party platforms. To see how these releases fit into Microsoft's broader foundation model catalog, browse the guide to Microsoft MAI models. For current seat subscription details, review the GitHub Copilot pricing guide.

- First-party availability: Available in GitHub Copilot.
- Enterprise preview: Limited-access preview for Code 2 Flash inside Microsoft Foundry.
- Planned distribution: Upcoming releases announced for OpenRouter, Vercel, Hermes, Open Code, and Kilo.
- Release timeline: Microsoft has published no rollout dates or regional restrictions for third-party platforms.

## What Microsoft Has Not Published

Several technical and commercial details are not published on Microsoft's announcement or model pages. As of November 18, 2026, Microsoft has not published a dollar price, a context window size, detailed benchmark tables, availability dates or regions, or specific Copilot plan and region details on its announcement or model pages.

The lack of published dollar pricing forces software teams to rely entirely on relative percentage claims. While Microsoft states MAI Code 2 operates at 40 percent of GPT 6.1 Sol cost and 5 percent of Claude Opus 5.5 cost at promotional rates, dollar prices remain undisclosed.

Technical teams should check the Microsoft AI news listing directly for future updates on API specifications. Until Microsoft publishes concrete context windows and dollar prices, teams should treat all efficiency numbers as vendor-reported percentages. General pricing across live commercial models is documented in our AI model pricing index.

- Dollar pricing: Not published on Microsoft's product pages as of November 18, 2026.
- Context window size: Not published on Microsoft's product pages as of November 18, 2026.
- Detailed benchmark tables: Not published on Microsoft's product pages as of November 18, 2026.
- Availability dates and regions: Not published on Microsoft's product pages as of November 18, 2026.
- Copilot plan and region tiers: Not published on Microsoft's product pages as of November 18, 2026.

## Should You Try MAI Code 2

If you already use GitHub Copilot, you should try MAI Code 2 inside your existing development workflow for enterprise coding tasks.

When evaluating the commercial upside, treat Microsoft's cost and performance percentages as vendor-reported claims calculated at a 50 percent promotional price. MAI Code 2 is not suitable for software teams that require published dollar prices or confirmed context window limits before committing engineering time. Microsoft also explicitly states that neither model is tuned for fundamental science research or very long-horizon agentic work.

This recommendation would change if Microsoft publishes definitive dollar prices, confirms context window limits, or clarifies Copilot plan and region details. Independent evaluations from third-party testing organizations would also provide clearer validation. For context on Microsoft's broader model development, see our explainer on MAI-Base-2. Verify your current seat options in GitHub Copilot and test MAI Code 2 on a non-critical feature branch.

## Frequently Asked Questions

- Microsoft says MAI Code 2 and Code 2 Flash are available in GitHub Copilot as of its November 17, 2026 announcement. It states no Copilot plan or region details. Code 2 Flash is also in a limited-access preview in Microsoft Foundry.
- MAI Code 2 targets complex enterprise coding while Code 2 Flash targets high-volume routine development. MAI Code 2 handles multi-file implementation, deep debugging, UI development, and repository-wide reasoning. Code 2 Flash focuses on rapid code edits, quick fixes, code explanations, and rapid iteration.
- No, Microsoft has not published a dollar price or context window size for MAI Code 2 or Code 2 Flash on its product pages as of November 18, 2026. Microsoft reports costs only as relative percentages of competing models based on a 50 percent promotional price.
- Microsoft ran the benchmarks at maximum reasoning effort across four named evaluations: DeepSWE 1.1, Terminal Bench 2.1, SWE-Atlas, and FrontierCode v1.1. Microsoft stated that individual results vary, and reported that MAI Code 2 achieved 98 percent of GPT 6.1 Sol performance and 96 percent of Claude Opus 5.5 performance across those four tests.
- Microsoft announced plans to release the models soon on third-party platforms including OpenRouter, Vercel, Hermes, Open Code, and Kilo, but has not published specific launch dates.

### Plan Your Enterprise AI Model Architecture

Evaluate how new coding foundation models, developer assistants, and private fine-tuning strategies fit your engineering workflow. Book a consultation with Layer3Labs.

Book a Consultation
