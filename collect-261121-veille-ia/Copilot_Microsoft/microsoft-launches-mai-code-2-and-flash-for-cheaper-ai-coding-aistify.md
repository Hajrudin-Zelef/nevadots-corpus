---
id: collect-261121-veille-ia/copilot-microsoft/microsoft-launches-mai-code-2-and-flash-for-cheaper-ai-coding-aistify
title: "Microsoft Launches MAI Code 2 and Flash for Cheaper AI Coding | AIstify"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["code", "flash", "microsoft", "models", "coding", "tokens", "cost", "rates"]
source: https://aistify.com/microsoft-mai-code-2-flash-copilot-pricing/
source_anchor: ""
source_lines: [1, 39]
section: "Copilot_Microsoft"
---
### Key Notes

- MAI Code 2 targets complex software work and interfaces, while Flash focuses on routine fixes and code explanations.
- Microsoft's headline benchmark cost comparisons use introductory prices rather than the rates scheduled for May 2027.
- GitHub Copilot lists both models as available, with Code 2 Flash also offered in a limited-access Microsoft Foundry preview.

Microsoft has released MAI Code 2 and MAI Code 2 Flash in GitHub Copilot, offering developers two new coding models at introductory prices that will double next May. The larger model targets demanding software work and interface creation, while Flash handles routine changes and code explanations.

Microsoft announced the models on November 17 at Ignite. Its pitch centers on making everyday engineering cheaper, with benchmark comparisons that place Code 2 close to selected OpenAI and Anthropic models at a lower cost per task.

The accompanying model card specifies a 1.05-million-token context window for both models, with up to 922,000 input tokens and 128,000 output tokens. Both accept text and images and return text, allowing developers to supply screenshots and interface mockups alongside code.

### Coding Benchmarks With a Pricing Caveat

Microsoft says Code 2 achieves 98% of GPT-6.1 Sol’s performance at 40% of its cost, and 96% of Claude Opus 5.5’s performance at 5% of its cost. Those figures are averages across four coding evaluations: DeepSWE 1.1, TerminalBench 2.1, SWE-Atlas and FrontierCode 1.1.

These are relative benchmark results, not a claim that Code 2 correctly completes 98% of all programming tasks. Microsoft used maximum reasoning effort and included its 50% promotional price in the comparison. Three evaluations ran on Microsoft’s internal platform using the Copilot CLI without subagents; FrontierCode results and costs came from Artificial Analysis.

Code 2 is intended for debugging, changes spanning multiple files and interface development. Flash prioritizes explanations, targeted edits and routine fixes. The distinction matters for teams choosing a default model: cheaper tokens can reduce the cost of frequent small tasks, but additional attempts and human corrections can change the economics.

### Introductory Rates End in May

GitHub’s pricing documentation lists MAI Code 2 at $0.50 per million input tokens and $2.50 per million output tokens. Flash costs $0.05 for input and $0.25 for output, making its base token rates one-tenth of Code 2’s.

Cached input costs $0.025 per million tokens for Code 2 and $0.005 for Flash. These are Copilot’s published model-usage rates, rather than the price of a Copilot subscription.

Starting May 17, 2027, Code 2 rises to $1 for input, $0.05 for cached input and $5 for output per million tokens. Flash rises to $0.10, $0.01 and $0.50, respectively. All three rates double for each model.

For teams planning sustained use, the useful comparison is the cost of accepted code after retries and review, calculated at the rates that will apply beyond the promotion.

### Where the Models Are Available

Both models are available in GitHub Copilot. Code 2 Flash is also in a limited-access preview through Microsoft Foundry, with additional distribution through services including OpenRouter and Vercel planned.

The launch arrives alongside OpenAI’s GPT-6.2 release, although Microsoft’s headline comparison uses the earlier GPT-6.1 Sol. It should not be read as a direct comparison with OpenAI’s newest pair.

Microsoft’s model card also identifies limits: long coding sessions can drift from instructions or end before work has been fully verified, and higher reasoning effort does not always improve results. The models give developers another price-performance option, while leaving code review and testing central to deciding whether their output is ready to merge.

Disclaimer: AIstify is an independent media brand owned and operated by NuvexMedia LLC, publishing news, research, and insights on artificial intelligence, emerging technologies, automation, and related industries. NuvexMedia LLC invests in and collaborates with companies across the AI, technology, software, and digital innovation sectors. These relationships do not influence AIstify’s editorial coverage, and the publication maintains full editorial independence to provide accurate, timely, and objective information. © 2026 NuvexMedia LLC. All rights reserved. This content is for informational purposes only and should not be considered legal, tax, investment, financial, or other professional advice.
