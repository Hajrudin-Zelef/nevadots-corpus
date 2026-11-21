---
id: collect-261121-veille-ia/copilot-microsoft/microsoft-mai-seven-new-models-prices-and-benchmark-caveats
title: "Microsoft MAI Seven New Models: Prices and Benchmark Caveats"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-11-16"]
keywords: ["models", "microsoft", "flash", "seven", "benchmark", "gpt-6", "price", "code"]
source: https://braindetox.kr/en/posts/microsoft_ignite_2026_mai_seven_models.html
source_anchor: ""
source_lines: [1, 158]
section: "Copilot_Microsoft"
---
# Microsoft MAI Seven New Models: Prices and Benchmark Caveats

## The seven models Microsoft AI announced at Ignite 2026: pricing and benchmark conditions, how to get them, and the values still missing

## The seven models at a glance

Microsoft AI cites rising token spend and says it is releasing models at a price point that lets you run agents 24/7. The source puts Maia 200 in the same list as the seven models. It describes Maia 200 as Microsoftâs second-generation AI accelerator, now in production running its largest models, with 1.7x more tokens per watt. The comparison baseline is not stated, so this article stays with the seven models.

| Model | Area | Headline claim in the source | Access path (per the source) | 
|---|---|---|---|
| MAI Code 2 | Coding | 98% of GPT-6.1 Sol performance at 40% of the price, averaged over four benchmarks | GitHub Copilot | 
| MAI Code 2 Flash | High-volume everyday coding | Comparable to GPT-6 Luna at 65% of the price and Claude Haiku 5.5 at 20% | GitHub Copilot; limited-access preview in Microsoft Foundry | 
| MAI Cyber 2 Flash | Security | CyberGym-E2E 92.3% vs 91.5% (GPT-6 Astra), under 2% of the cost per task | Microsoft security products only | 
| MAI Image 2.7 and Flash | Image generation and editing | MAI Image 2.7 is #1 on Artificial Analysis image editing (no ranking value for Flash in the source) | Public preview in Foundry and some partners | 
| MAI Realtime 1 | Live speech | Said to outperform GPT Realtime 2.1; from $0.01 per audio minute | MAI Labs; register for the Foundry limited-access preview | 
| MAI Transcribe 2 | Multilingual transcription | Multilingual error rates halved; 10 million patient encounters a month | Inside Dragon Copilot | 
| Decision 1.1 | Model routing | 50x faster and 79x cheaper than GPT-6.1 Sol | â | 

Comparison model names follow the sourceâs spelling. The comparison targets and metrics differ from row to row, so the rows should not be read against each other. The sections below go through the conditions under which each figure holds.

## Two coding models: what is left to the earlier article

Code 2 and Code 2 Flash already have their own article. Pricing and benchmark commentary stays in the MAI Code 2 and Flash article; only the conditions this announcement adds are listed here.

- Microsoft AI says that, averaged over DeepSWE 1.1, Terminal Bench 2.1, SWE-Atlas and FrontierCode v1.1, Code 2 gives 98% of GPT-6.1 Solâs performance at 40% of the price and 96% of Claude Opus 5.5âs at 5% of the price.
- For Code 2 Flash it says performance is comparable to GPT-6 Luna and Claude Haiku 5.5 at 65% and 20% of the price, respectively, across three benchmarks (DeepSWE 1.1, Terminal Bench 2.1 and SWE-Atlas).
- The announcement states that these models prioritize everyday coding work and are explicitly not yet tuned for harder problems, like fundamental science research and very long horizon agentic work.

The third item matters for any decision. The benchmark average targets everyday enterprise coding tasks, and the source gives no basis for assuming the ratios carry over to long autonomous work or research-style tasks.

## MAI Cyber 2 Flash: benchmark conditions to read before the score

The security model carries the longest list of conditions in the announcement. The footnotes help a decision more than the score does.

- On CyberGym-E2E, MAI Cyber 2 Flash scores 92.3% and GPT-6 Astra 91.5%, at less than 2% of the cost per task, Microsoft AI says. The footnote states it used half the reasoning budget.
- On WebExploitBench, the footnote gives 40.4% against 38.4% for GPT-5.6 Cyber at the same reasoning budget.
- Cost per task is computed on CyberGym-E2E from all input, cached input and output tokens at list price: $0.10 / $0.01 / $0.50 per 1M tokens for MAI Cyber 2 Flash and $10 / $1 / $50 for GPT-6 Astra (Azure OpenAI Global). The source says this reflects offline evaluation cost, not customer spend.
- The context window is one million tokens, four times larger than MAI Cyber 1.1 Flash.
- Microsoft AI says it is priced 86% lower on output tokens than MAI Cyber 1.1 Flash.

The benchmark integrity footnote stands out. Microsoft AI says that while reviewing its CyberGym-E2E results it found that 92 of the benchmarkâs 905 tasks are based on the same open source projects and vulnerability reports as some of its training data. It says the benchmarkâs reference solutions were never used in training, and that, to be conservative, it excluded these 92 tasks for every model and reports results on the remaining 813 tasks. Three conditions apply when reading the figure.

- The 92.3% figure is on 813 tasks, not on all 905.
- There is no comparison with GPT-6 Cyber yet. The source says GPT-6 Cyber is currently available only to select customers, that evaluating it on agentic, tool-using benchmarks requires additional safeguards still being put in place, and that Microsoft AI plans to share comparative results in the coming weeks.
- Insight Enterprisesâ customer-reported 20% token-efficiency advantage for MAI Cyber models is measured against GPT-5.4, GPT-5.3 Codex and GPT-5.4 Mini, not the GPT-6 family in this announcement.

The access path is Microsoft security products only. The source says the model will automatically replace the existing model in the MAI profile of Codename MDASH for eligible customers starting mid-December. It also says MAI Cyber accounts for almost 40% of scans in MDASH and that 90% of scans that started with MAI continued to use it. The period for that 90% is not given.

## MAI Image 2.7 and Flash: the ranking date and the price

An image-model ranking is a value fixed to a date. The source footnote gives November 16, 2026.

- Microsoft AI says MAI Image 2.7 ranks #1 on AA-Image-Editing v2.1 with 1,215 Elo across 5,478 votes, ahead of GPT Image 2.6 Sunburst (max) by 4 Elo.
- The rankings rest on Artificial Analysisâs editing-action evaluations. For text-to-image generation the model is ranked #4, which the announcement describes as making Microsoft AI the second-best lab.
- In public preview on Foundry, MAI Image 2.7 costs about $35 per 1,000 images and Flash about $10.
- Both models support transparent backgrounds, and MAI Image 2.7 is said to preserve visual consistency across successive edits.

The price comparison is worded two ways within the announcement. The main post says one-fifth of the price of GPT Image 2.6 Sunburst (max), while the individual post says one-fifth of the cost. The competitorâs per-image price is not in the source, so a reader comparing directly needs both price lists at the same resolution and quality setting.

The access path is public preview through Microsoft Foundry and partners including fal, Genspark, OpenRouter and Vercel. Canvas in MAI Labs is an interactive workspace powered by Flash, where you start from a prompt or sketch and select part of an image to replace, move or restyle. Microsoft AI also says Adobe, Figma Weave, Minute (Sports Illustrated), Wirestock and Manus are working with MAI Image. It adds that MAI Image became the default model family powering Bing Image Creator after Build in June, with usage growing at about 15% compound weekly growth (a reported 14.7%).

## MAI Realtime 1: what $0.01 per minute covers

MAI Realtime 1 is Microsoft AIâs first live speech-to-speech model. The $0.01 per audio minute in the title and summary covers only part of the cost.

- The footnote says base speech-to-speech usage is $0.01 per minute and reasoning is billed separately at $1 per million input tokens and $5 per million output tokens.
- The comparison target is GPT Realtime 2.1, using public benchmarks including Big Bench Audio and Ï-Voice, alongside Dynamics 365 Contact Center evaluations.
- The model is compatible with the OpenAI Realtime API, which the source says makes it straightforward to test and migrate existing voice applications.
- Microsoft AI says companies including Agoda and Reservations.ai are putting it to the test, and that it is coming to Dynamics 365 Contact Center and Dragon Copilot.

The source describes five capabilities: natural conversation (it manages interruptions, handles quick acknowledgments and knows when to wait for someone to finish speaking), adaptive intelligence (when a task requires deeper thinking, it taps into a more powerful reasoning engine), robust understanding (it recognizes requests even with background noise or other people talking), real-world action (it connects to enterprise systems and tools) and adaptable personas (tone, style and behavior adjust through prompting). This is Microsoft AIâs own description and not an independently measured value.

The access paths are MAI Labs and registration for the Foundry limited-access preview. The source says preview customers wonât be billed until public preview begins. At this stage, then, $0.01 per minute is a published price list and not a figure on an actual invoice. To estimate a voice agentâs monthly cost, the reasoning tokens used need a separate estimate for your own scenario on top of base usage.

## MAI Transcribe 2 and Decision 1.1: two narrowly stated figures

These two models have no individual announcement post and appear only in the main announcement and the keynote transcript, so each value comes with a narrow stated scope.

- Microsoft AI says Dragon Copilot Transcribe, built on MAI Transcribe 2, powers 100% of doctorâpatient conversation transcriptions in Dragon Copilot, covers 10 million encounters a month, halves error rates in multilingual workflows, and is in the hands of a third of US physicians.
- The source does not say which metric the error rate uses or what it is halved against.
- Decision 1.1 is described as providing model routing integrated into applications so that bigger, pricier models are used only when really needed.
- The source says that on the Artificial Analysis Decision Index Microsoft is ahead of Jev, Google and OpenAI while faster and cheaper, that 4,000 customers use it, and that it is 50x faster and 79x cheaper than GPT-6.1 Sol. The conditions behind those multipliers are not given.

The structure of Decision-family models and how to choose among them were covered in the Decision model comparison article. For routing approaches in general, see the article on Cloudflareâs decision model. Here only the new model figures are covered.

## MAI Licensed Weights and the custom-model cases

MAI Licensed Weights, announced alongside the seven models, is a delivery method and not a model. The idea is to bring a frontier MAI model into your own Azure environment, customize it, and decide yourself who can access it and when it is upgraded.

- Microsoft AI says your data and adaptations remain yours, with Microsoft only using them to train its models with your express agreement.
- AT&T used MAI models to analyze 5G networks; for standard tasks MAI outperformed GPT-6.1 Sol at 1/50th of the cost, and for more complex tasks it achieved frontier-level performance at a significantly lower cost. The footnote says this was a customized MAI Code 2 Flash and Code 2 evaluated on 5G network operations analytics.
- Woven by Toyota used MAI models to produce ready-to-merge fixes in 44% of cases where its existing frontier models required human intervention, at less than one-tenth of the cost. The footnote says this was MAI Code 2 Flash fine-tuned on historical MISRA coding-standard fixes, compared with frontier models used in production.
- Alaska Airlines improved aircraft maintenance report classification accuracy from 81.2% to 91.1% following tuning. The footnote adds that qualified employees retain responsibility for final decisions.

Availability is coming soon through Frontier Company engagements, and the contact is the Microsoft account team. The footnote says all three results are customer-specific evaluations that vary by workload, model configuration and methodology. They are not a basis for expecting another organization to get the same ratios.

## Who is affected and how

| Reader | What changes now | Condition stated in the source | 
|---|---|---|
| Developers (coding assistants) | Code 2 and Flash become options in Copilot | Everyday coding focus; long agentic work is not a tuning target | 
| Security operations teams | MDASH customers may see the existing model replaced automatically from mid-December | Eligible customers; Microsoft security products only | 
| Image and design producers | An editing-focused model is available via Foundry public preview and some partners | Ranking as of November 16, 2026; preview stage | 
| Voice and contact-center developers | OpenAI Realtime API compatibility is said to ease test migration | Limited-access preview; no billing before public preview | 
| Healthcare organizations and Dragon Copilot users | Transcription in Dragon Copilot runs on MAI Transcribe 2 | Metric and baseline not stated in the source | 
| Companies wanting their own model | MAI Licensed Weights is coming soon | Frontier Company engagements; no price given | 

The source states no direct change for investors or general users. The one change that reaches general users is that MAI Image became the default model family behind Bing Image Creator.

## Adoption criteria and what you can do now

- If the comparison target is a model you already use, re-measure quality and cost on your own task samples in the same way rather than trusting the announced ratios. The footnotes themselves say comparisons vary by workload and methodology.
- For the security model, read the score together with whether it is on 813 tasks and how the reasoning budgets differed. Revisit it when the GPT-6 Cyber comparison is published.
- If you are an MDASH customer, ask your Microsoft account team before the mid-December automatic replacement whether you can plan your own evaluation. The source does not give the replacement procedure.
- When choosing an image model, put the Foundry public preview price (about $35 and about $10 per 1,000 images) next to your current modelâs per-image price under the same conditions.
- For a voice agent, estimate monthly cost by separating the base per-minute rate from the reasoning token rates, and check when billing starts during the preview.
- If a custom model is the goal, the guidance is to contact the Microsoft account team until Licensed Weights pricing and schedule are published. The case ratios are examples, not expectations.

## What could not be confirmed

- The source gives no value for Decision 1.1âs access path and price, or for the conditions behind 50x and 79x.
- The source gives no value for MAI Transcribe 2âs error-rate metric and baseline, or its standalone price.
- The source gives no value for what Maia 200âs 1.7x tokens per watt is measured against.
- The source gives no value for the competitorâs per-image price and the resolution and quality settings behind the Image 2.7 comparison.
- The source gives no value for the MAI Licensed Weights price and exact availability date.
- The source gives no value for independent reproduction outside Microsoft AI. Every figure in this article is one Microsoft AI states.

## References

- Seven new models announcement â Microsoft AI, November 17, 2026. The list of seven and each headline figure.
- Introducing MAI Cyber 2 Flash â Microsoft AI. Benchmark footnotes and the integrity note.
- Introducing MAI Image 2.7 â Microsoft AI. Ranking date and pricing.
- Introducing MAI Realtime 1 â Microsoft AI. Pricing footnote and access paths.
- Introducing MAI Licensed Weights â Microsoft AI. Customer cases and footnotes.
- Ignite 2026 MAI keynote transcript â Microsoft AI. Remarks on Decision 1.1 and Transcribe 2.
- GeekNews item â the entry that prompted this article.

## Frequently asked questions

### Which seven models did Microsoft AI announce?

The source lists MAI Cyber 2 Flash, MAI Code 2, MAI Code 2 Flash, MAI Image 2.7, MAI Realtime 1, MAI Transcribe 2 (the base of Dragon Copilot Transcribe) and Decision 1.1. The eighth line in the same list, Maia 200, is an AI accelerator and not a model.

### Under what conditions is the MAI Cyber 2 Flash CyberGym-E2E score reported?

Microsoft AI reported 92.3% against 91.5% for GPT-6 Astra. It says 92 of the benchmarkâs 905 tasks, based on the same open source projects and vulnerability reports as some of its training data, were excluded for every model, so the figure covers the remaining 813 tasks, and that it used half the reasoning budget.

### Is $0.01 per audio minute the whole cost of MAI Realtime 1?

The source footnote says base speech-to-speech usage is $0.01 per minute and that reasoning is billed separately at $1 per million input tokens and $5 per million output tokens. The $0.01 figure therefore covers base usage only.

### Can MAI Licensed Weights be used today?

Microsoft AI says it is coming soon through its Frontier Company engagements and asks readers to contact their Microsoft account team. The source gives no price or schedule.

### Are these numbers independently verified?

The source footnotes say results reflect specific model configurations and evaluations, and that performance, speed and cost comparisons vary by workload and methodology. Most figures are Microsoft AIâs own, so re-measuring on your own workload is the sensible test.
