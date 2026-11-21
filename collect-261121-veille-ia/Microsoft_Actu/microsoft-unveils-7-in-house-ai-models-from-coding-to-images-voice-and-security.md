---
id: collect-261121-veille-ia/microsoft-actu/microsoft-unveils-7-in-house-ai-models-from-coding-to-images-voice-and-security
title: "Microsoft Unveils 7 In-House AI Models, From Coding to Images, Voice and Security"
domain: microsoft-actu
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-11-17"]
keywords: ["code", "flash", "microsoft", "standard", "pricing", "models", "cost", "input"]
source: https://xenospectrum.com/en/microsoft-mai-code-2-pricing/
source_anchor: ""
source_lines: [1, 60]
section: "Microsoft_Actu"
---
On November 17, 2026 (US time), at Ignite 2026, Microsoft announced two coding-focused AI models, "MAI Code 2" and "MAI Code 2 Flash," and made them available in GitHub Copilot the same day. Of the seven models announced at Ignite, MAI Code 2 is said to deliver comparable performance to Claude Opus 5.5 at 5% of the cost. However, that comparison uses a limited-time discounted price. Across the four benchmarks Microsoft published, if we hold each model's token consumption and Claude's unit price fixed, and apply the standard pricing that takes effect on May 17, 2027 to MAI Code 2 alone, the cost ratio comes to about 10%. What matters is how much of the gap in per-token pricing actually carries through to the cost of real work.

## Rereading "5% of Opus 5.5" at standard pricing

According to Microsoft's Code 2 announcement, averaged across four benchmarks with reasoning set to maximum, MAI Code 2 scored 96% of Claude Opus 5.5 and 98% of GPT-6.1 Sol. Its costs were 5% and about 40% of theirs, respectively. Three of the benchmarks, DeepSWE 1.1, Terminal Bench 2.1 and SWE-Atlas, were measured by Microsoft on MSBench using GitHub Copilot CLI, without calling subagents. For the fourth, FrontierCode v1.1, Microsoft used scores and costs measured by Artificial Analysis. The 96% and 98% figures are relative values calculated against each comparison model's score across these four benchmarks.

The cost comparison at announcement used a limited-time rate that halves MAI's per-token prices. According to GitHub Copilot's pricing table, per-token prices for MAI Code 2 and MAI Code 2 Flash switch to standard pricing on May 17, 2027. Prices are in US dollars per million tokens, as follows.

| Model | Pricing tier | Input | Cached input | Output | 
|---|---|---|---|---|
| MAI Code 2 | Limited-time | 0.50 | 0.025 | 2.50 | 
| MAI Code 2 | Standard | 1.00 | 0.05 | 5.00 | 
| MAI Code 2 Flash | Limited-time | 0.05 | 0.005 | 0.25 | 
| MAI Code 2 Flash | Standard | 0.10 | 0.01 | 0.50 | 

Input, cached input and output prices all double when the switch to standard pricing occurs. Assuming each model's token consumption during testing and the comparison models' unit prices stay the same, putting only MAI Code 2 back on standard pricing gives a cost ratio of about 10% against Claude Opus 5.5. That is the published 5% divided by 0.5, the ratio of the limited-time price to the standard price. The same calculation raises the cost ratio against GPT-6.1 Sol from about 40% to about 80%. Both calculations change only the prices, leaving the number of tokens each model consumed in testing unchanged.

The pricing table also allows cost ratios under other conditions. The standard price of Claude Opus 5.5 in GitHub Copilot is $4 per million input tokens and $20 per million output tokens. If caching is not used and both models consume the same number of input and output tokens, MAI Code 2 at standard pricing costs 25% of Opus, because both its input and output prices are one-quarter as high. In real work, however, the number of tokens spent on reasoning and the share of past input that can be reused from cache differ by model. Price changes to the comparison models would also change the cost ratio.

## Which tasks suit Code 2 and Flash?

Microsoft designed MAI Code 2 for implementation and debugging that span multiple files. MAI Code 2 Flash, by contrast, suits repetitive work such as partial code fixes and code explanations. For Flash's evaluation, Microsoft used the three benchmarks it measured itself out of the four used for Code 2. According to the evaluation results, Flash achieved scores close to GPT-6 Luna and Claude Haiku 5.5 at 65% and 20% of their costs, respectively, at the limited-time price.

Both models use a mixture-of-experts (MoE) architecture, which activates only the necessary parts at inference time. MAI Code 2 Flash has 12 billion active parameters, up from 5 billion in the earlier MAI-Code-1.1-Flash. When training the new Flash from scratch, Microsoft also used distillation, learning from the outputs of a larger in-house model. According to the Code 2 series model card, both models accept text and image input and support a context length of 1.05 million tokens. Because the amount of compute allocated to reasoning is configurable, the same model's reasoning settings can be changed to suit the task.

MAI Code 2 is available in GitHub Copilot's model picker on the individual Pro, Pro+ and Max plans and the organizational Business and Enterprise plans. MAI Code 2 Flash can also be selected on Free and Student. On Business and Enterprise, however, availability depends on the model policy set by administrators. The older MAI-Code-1.1-Flash is scheduled to be retired from GitHub Copilot on December 18, 2026. GitHub has announced both the availability of the new models and the planned retirement of the old model.

GitHub Copilot also offers "Copilot Fusion," which combines multiple models to process tasks, and MAI Code 2 has now been added to its Standard tier. High incorporates GPT-6.1 Terra and Max incorporates Claude Sonnet 5.5. Details are in GitHub's Fusion update. Meanwhile, in Microsoft Foundry's availability notice, the model for which access terms were given this time is MAI Code 2 Flash, offered as a limited preview. A team that has tried both models in Copilot and wants to build Flash into its own app via Foundry will need to join the preview.

Microsoft will also use "MAI Work Flash," a tuned version of Flash, in Microsoft 365 and Fabric IQ. According to the company, it performed on par with GPT-6.1 Sol on five internal Word benchmarks. In actual Word operation, it also used about 42% fewer input tokens per model call than GPT-6.1 Sol. These results are published in the Microsoft AI announcement.

## Image generation and voice conversation use different billing units

The image generation model "MAI Image 2.7" had its image editing capabilities released early on October 27. At Ignite, Microsoft newly announced MAI Image 2.7 Flash, and both models' image generation and editing features are offered as a public preview in Microsoft Foundry. They also support images with transparent backgrounds, and in MAI Labs' Canvas, Flash handles edits to selected regions. According to Microsoft's announcement, the estimated cost in the Foundry public preview is about $35 per 1,000 images for MAI Image 2.7 and about $10 for Flash.

In Artificial Analysis's image editing evaluation, humans compare images edited from text instructions one pair at a time, and rankings are estimated from those preferences. As of November 18, MAI Image 2.7's Elo score was 1,215 and GPT Image 2.6 Sunburst (max)'s was 1,211. The gap in estimates is just 4 points, and the 95% confidence intervals, 1,201 to 1,229 and 1,198 to 1,224 respectively, overlap. It is an option for trying image editing at lower cost, but this narrow margin alone makes it hard to judge which model is better for your own editing instructions.

The voice conversation model "MAI Realtime 1" also handles interruptions when a user starts speaking while the AI is responding. It is compatible with the OpenAI Realtime API and can be tried in MAI Labs. Foundry is accepting registrations for a limited preview. According to Microsoft's announcement, pricing is $0.01 per minute of audio, with additional inference processing billed at $1 per million input tokens and $5 per million output tokens. Participants in the limited preview will not be charged until the public preview begins.

Meanwhile, MAI Transcribe 2, announced on September 3, identifies multiple speakers and can transcribe even when the language switches mid-conversation. It is now built into "Dragon Copilot," which supports the creation of medical documentation, and handles all transcription of doctor-patient conversations. According to Microsoft's explanation at Ignite, Dragon Copilot is used in 10 million patient encounters a month, and MAI Transcribe 2 halved the error rate in multilingual processing.

## Security product integration and a decision model for routing

"MAI Cyber 2 Flash" will be built into dedicated security products aimed at defense. In "Codename MDASH," for eligible customers, it is scheduled to replace the existing model in the MAI profile from mid-December. According to Microsoft's announcement, of the 905 CyberGym-E2E items used in offline evaluation, the company excluded for all models 92 items based on the same public projects and vulnerability reports as the training data. On the remaining 813 items, the task success rate was 92.3% for MAI Cyber 2 Flash and 91.5% for GPT-6 Astra. Microsoft says that the reasoning budget assigned to Cyber 2 Flash in this evaluation was half that of the comparison model, and that the cost per task, converting consumed tokens at list price, was under 2% of GPT-6 Astra's.

Separately, "Microsoft-Decision-1.1," which entered public preview in October, became generally available on November 17 on Microsoft Foundry, OpenRouter and Vercel. It is based on Gemma 4 26B A4B and also supports image input. The model chooses from options supplied by an app, such as classification categories or processing priorities, and returns a confidence level for its decision. Microsoft uses it to route user requests to either Copilot or Cowork. In Cowork, it also decides how much reasoning compute to allocate depending on the work.

App developers can configure whether to hold a response or route processing to a higher-performance model depending on the confidence level the model returns. For example, in a setup where only high-confidence requests are handled by a cheaper model, the number of calls to the higher-tier model varies with the hold and routing conditions. The way costs are estimated also differs from processing every input with a single model.

## What changes when you use model weights on Azure?

Microsoft plans to offer "MAI Licensed Weights" to eligible companies soon through the Frontier Company program. This will let customers run models in their own Azure environments and further train them on their own data. Customers will also control access to the models and when they are updated. Rights to customer data and tuning results belong to the customer, and Microsoft needs the customer's explicit consent to use them to train its own models. The MAI Licensed Weights announcement does not disclose pricing, eligible models or a specific release date.

In examples of per-company model tuning published by Microsoft, Woven by Toyota further trained Flash using past fixes it had made to comply with MISRA rules for embedded software. In cases where the company's previous production model required manual correction, the tuned model generated fix proposals that could be adopted as is in 44% of them. Microsoft reports that costs were also less than one-tenth of before. At Alaska Airlines, the accuracy of classifying aircraft maintenance reports rose from 81.2% to 91.1%. Final decisions remain with qualified employees.

On AT&T, Microsoft says that in analyzing standard 5G network operations, a tuned MAI model outperformed GPT-6.1 Sol at one-fiftieth of the cost. However, the three companies each tuned models for different tasks. The metrics also differ: the share of fix proposals adoptable as is, report classification accuracy and network operations analysis performance, so the results cannot be compared directly.

On the Microsoft-OpenAI relationship, even after the April 2026 contract revision, Azure remains OpenAI's primary cloud, and Microsoft's license to OpenAI's intellectual property is a non-exclusive agreement running through 2032. In the Ignite keynote, Microsoft also described bringing GPT-6.2 to Foundry. On Azure, tuning Microsoft's own models to your business and choosing external models coexist. Furthermore, the company's inference chip "Maia 200" was already in production use in Copilot and elsewhere as of October.

When companies compare models, setting the same tasks and pass criteria lets them measure how differences in unit price affect actual cost of work. Costs include not only usage fees calculated at standard pricing but also the time spent on retries and human review. If you manage models yourself on Azure, you also need to account for the burden of further training and updates. To assess lasting cost reductions, it is important to compare not just call counts and per-token prices but the total cost of obtaining usable results.
