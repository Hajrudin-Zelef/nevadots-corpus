---
id: collect-261121-veille-ia/copilot-microsoft/mai-code-2-web-observer
title: "MAI Code 2 - Web Observer"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["code", "copilot", "github", "models", "model", "flash", "input", "promotional"]
source: https://web-observer.com/tag/mai-code-2/
source_anchor: ""
source_lines: [1, 91]
section: "Copilot_Microsoft"
---
**Takeaway:** On November 17, 2026, Microsoft introduced MAI Code 2 and MAI Code 2 Flash, and GitHub added both models to GitHub Copilot. Code 2 is the stronger general-purpose coding model; Flash is the lower-cost option for everyday edits. Both are billed by usage. GitHub’s pricing docs list promotional rates through May 17, 2027, then higher standard rates. Eligible Copilot subscribers must be on annual plans. Flash is available on Copilot Free and Student as well as paid plans; Code 2 starts at Copilot Pro.

This is a model addition inside Copilot, not a redesign of Copilot itself. The plan limits, surfaces, and token prices below come from GitHub’s November 17 changelog and from GitHub’s models-and-pricing documentation. Microsoft’s launch post supplies the performance and cost-comparison claims, which use the models’ promotional prices and Microsoft’s own evaluation setup. That is a different story from our earlier coverage of Copilot cloud sessions and MCP connectors.

## What the two models are for

GitHub describes MAI Code 2 as a general-purpose coding model aimed at getting from a request to a result with focused responses and less back-and-forth. It flags Code 2 for deeper work: debugging, multi-file implementation, refactoring, and repository-wide reasoning. MAI Code 2 Flash is the lightweight, cost-efficient model for everyday workflows — code explanations, targeted edits, routine fixes, and rapid iteration. Both, GitHub says, are designed for agentic coding workflows, so you can pick the model that fits the task.

Microsoft’s post is more explicit about what the models are not. The company says they were built for everyday enterprise coding and “are explicitly not yet tuned for harder problems, like fundamental science research and very long horizon agentic work.” That caveat is Microsoft’s, not an independent assessment, but it is part of the official launch text.

## Where you can use them in Copilot

According to the GitHub changelog:

- **MAI Code 2:** Copilot Pro, Pro+, Max, Business, and Enterprise.
- **MAI Code 2 Flash:** Copilot Free, Student, Pro, Pro+, Max, Business, and Enterprise.

Both models are available to eligible GitHub Copilot subscribers on annual plans. You can select them in the model picker in Visual Studio Code, Visual Studio, Copilot CLI, GitHub Copilot cloud agent, the GitHub Copilot app, github.com, GitHub Mobile on iOS and Android, JetBrains, Xcode, and Eclipse. Copilot Enterprise and Business administrators can manage access through the model policy in Copilot settings. Under default model enablement, GitHub says new models are turned on automatically unless an administrator has turned off the global default or disabled the model.

Microsoft also says Code 2 Flash is in limited-access preview in Microsoft Foundry, and that it will later release the models on Open Router, Vercel, Hermes, Open Code, and Kilo. Those hosts are Microsoft’s stated roadmap; the November 17 GitHub changelog is about Copilot availability, not those other marketplaces, and Microsoft did not give dates for them.

## Official Copilot prices, including the promo window

GitHub says both models are billed under usage-based billing and points to its models-and-pricing documentation. Those docs list the following rates per 1 million tokens. Until May 17, 2027, the figures are promotional; starting May 17, 2027, the standard rates apply:

- **MAI Code 2 (promotional, through May 17, 2027):** $0.50 input, $0.025 cached input, $2.50 output.
- **MAI Code 2 (starting May 17, 2027):** $1.00 input, $0.05 cached input, $5.00 output.
- **MAI Code 2 Flash (promotional, through May 17, 2027):** $0.05 input, $0.005 cached input, $0.25 output.
- **MAI Code 2 Flash (starting May 17, 2027):** $0.10 input, $0.01 cached input, $0.50 output.

GitHub bills that usage in AI credits. The promotional window and the later standard rates are in GitHub’s footnotes for the two models; we are not treating the promo figures as permanent list prices.

## Microsoft’s performance claims, and the caveats attached to them

Microsoft says that, averaged over four benchmarks it ties to everyday enterprise coding in GitHub Copilot — DeepSWE 1.1, Terminal Bench 2.1, SWE-Atlas, and FrontierCode v1.1 — MAI Code 2 delivers 98% of the performance of GPT 6.1 Sol at 40% of the cost, and 96% of Claude Opus 5.5’s performance at 5% of the cost. It also says reviewers in blind frontend evaluations preferred MAI Code 2’s results over GPT-6 Sol 64% of the time, across user interfaces, games, data visualizations, 3D scenes, and SVG graphics, based on 258 annotators. For Flash, Microsoft says the model offers comparable performance to GPT-6 Luna and Claude Haiku 5.5 at 65% and 20% of the cost, across three coding benchmarks for routine work.

Those comparisons are Microsoft’s. The footnotes matter. All of the cited coding-benchmark runs used maximum reasoning effort. DeepSWE 1.1, TerminalBench 2.1, and SWE Atlas were run on MSBench, Microsoft’s internal evaluation platform, with the GitHub Copilot CLI and without subagents. FrontierCode 1.1 scores and costs come from Artificial Analysis. Microsoft also says the MAI Code 2 and Code 2 Flash cost figures in those comparisons use each model’s 50% promotional price. The company adds that results depend on the workload, model configuration, reasoning effort, and pricing assumptions. Until someone else reproduces the same setup, treat the percentages as Microsoft’s launch claims, not as third-party audited rankings.

Microsoft also says a further-optimized model, MAI Work Flash, derived from Code 2 Flash, is being used across Microsoft 365 and Fabric IQ, with internal comparisons for Excel, PowerPoint, Word, Teams, and DAX query generation in Fabric IQ. That is a statement about Microsoft’s own products. The GitHub changelog does not present MAI Work Flash as a model you can pick in Copilot.

## What the official pages do not say

Neither GitHub nor Microsoft publishes context-window sizes, parameter counts, or per-plan included-credit allotments specifically for these two models in the launch pages we used. Microsoft does not date the planned Open Router, Vercel, Hermes, Open Code, or Kilo availability, and “limited-access preview” in Microsoft Foundry is not the same as general availability. The launch materials also do not say whether monthly Copilot plans can use the models; GitHub’s changelog limits availability to eligible subscribers on annual plans.

## Key facts

| Item | What Microsoft and GitHub say | 
|---|---|
| Announcement | November 17, 2026, in GitHub Copilot; Microsoft launch post the same day. | 
| MAI Code 2 | General-purpose model for deeper debugging, multi-file work, refactoring, and repo-wide reasoning. Copilot Pro, Pro+, Max, Business, and Enterprise, on annual plans. | 
| MAI Code 2 Flash | Lower-cost model for explanations, targeted edits, routine fixes, and rapid iteration. Adds Copilot Free and Student to the paid plans above, also on annual plans. | 
| Billing | Usage-based, converted to GitHub AI credits. | 
| Promo prices (per 1M tokens, through May 17, 2027) | Code 2: $0.50 / $0.025 / $2.50 (input / cached input / output). Flash: $0.05 / $0.005 / $0.25. | 
| Standard prices (from May 17, 2027) | Code 2: $1.00 / $0.05 / $5.00. Flash: $0.10 / $0.01 / $0.50. | 
| Microsoft cost comparisons | Use the 50% promotional price and Microsoft’s evaluation setup, including MSBench runs without subagents. | 
| Stated limits of the models | Microsoft: not yet tuned for fundamental science research or very long-horizon agentic work. | 
| Outside Copilot | Flash in limited-access preview in Microsoft Foundry; Microsoft says other hosts are coming, with no dates given. | 

## FAQ

### What are MAI Code 2 and MAI Code 2 Flash?

They are Microsoft AI coding models added to GitHub Copilot on November 17, 2026. Code 2 is the stronger general-purpose model for more complex work. Flash is the lighter, cheaper model for everyday explanations, edits, and fixes.

### Which GitHub Copilot plans include MAI Code 2?

GitHub says MAI Code 2 is available on Copilot Pro, Pro+, Max, Business, and Enterprise. MAI Code 2 Flash is also available on Copilot Free and Student. In both cases, GitHub limits access to eligible subscribers on annual plans.

### How much do MAI Code 2 and Code 2 Flash cost in Copilot?

They are billed by token usage. Through May 17, 2027, GitHub’s promotional prices are $0.50 / $0.025 / $2.50 per 1 million tokens for Code 2 and $0.05 / $0.005 / $0.25 for Flash (input, cached input, and output). After that date, the listed standard rates are twice those promotional figures.

### Where can I select the new models?

GitHub says you can pick them in the model picker in Visual Studio Code, Visual Studio, Copilot CLI, GitHub Copilot cloud agent, the Copilot app, github.com, GitHub Mobile, JetBrains, Xcode, and Eclipse. Business and Enterprise admins can allow or block the models in Copilot model policy settings.

### Are Microsoft’s speed and cost comparisons independently verified?

No. The 98% / 40% and 96% / 5% figures, the 64% frontend preference, and the Flash comparisons to GPT-6 Luna and Claude Haiku 5.5 come from Microsoft’s launch post. The cost side of those comparisons uses the 50% promotional price, and several benchmark runs used Microsoft’s MSBench platform.

### Can I use MAI Code 2 on a monthly Copilot plan or on Copilot Free?

GitHub’s changelog says both models are for eligible subscribers on annual plans. Copilot Free is listed only for MAI Code 2 Flash, not for MAI Code 2. The launch pages do not say that monthly plans are included.

### Is MAI Code 2 Flash available outside GitHub Copilot?

Microsoft says Code 2 Flash is in limited-access preview in Microsoft Foundry and that it will also release the models on Open Router, Vercel, Hermes, Open Code, and Kilo. It does not give a date for those other hosts.

### Are these models meant for long, open-ended research work?

Microsoft says they are built for everyday enterprise coding and are not yet tuned for fundamental science research or very long-horizon agentic work. That is the company’s own positioning, not a third-party evaluation.
