---
id: collect-261121-veille-ia/gpt-openai/microsoft-unveils-seven-mai-models-putting-code-2-at-98-of-gpt-6-1-sols-performance-the-edge
title: "Microsoft Unveils Seven MAI Models, Putting Code 2 at 98% of GPT-6.1 Sol’s Performance - The Edge"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["code", "microsoft", "models", "coding", "model", "gpt-6", "performance", "flash"]
source: https://edgeconsultancykw.com/microsoft-mai-seven-models-code-2-cyber-2-flash-ignite-2026/
source_anchor: ""
source_lines: [1, 103]
section: "GPT_OpenAI"
---
**The Takeaway:** Microsoft released a family of seven new models from its MAI unit at its Ignite conference, pitching them on price as much as on capability. It says its new flagship coding model delivers 98% of a leading OpenAI model’s performance at 40% of the cost, on tests Microsoft ran or chose and at a promotional price. The coding and security comparisons are Microsoft’s own; the firmer facts are the prices it printed for its new image and voice models.

Microsoft (Nasdaq: MSFT) chief executive Satya Nadella opened the company’s Ignite conference on Tuesday by singling out two model lines: Code 2 for coding and Cyber 2 Flash for security.

Microsoft AI, the unit that builds the company’s MAI models, set out the rest of the San Francisco launch in its own posts the same day, calling it “a family of seven new world class models”. The family is aimed at customers who want to run AI agents around the clock, and it also covers image editing, live speech, medical transcription and a model that sends each task to a larger or a smaller model.

## Code 2 is built for routine enterprise coding, not frontier research

Code 2 is meant for the coding work customers do every day, including taking code to production, deep debugging and changes that span many files, Microsoft said. Both new coding models are “explicitly not yet tuned for harder problems, like fundamental science research and very long horizon agentic work”.

Microsoft averaged four widely used benchmarks, chosen to reflect the everyday tasks it sees in GitHub Copilot, its coding assistant. On that average, Code 2 delivers 98% of the performance of OpenAI’s GPT-6.1 Sol at 40% of the cost, measured at a promotional price, the company says.

Against Anthropic’s Claude Opus 5.5, on the same tests and again at the promotional price, Microsoft puts Code 2 at 96% of the performance for 5% of the cost.

The smaller Code 2 Flash is built for high-volume work such as code explanations, targeted edits and routine fixes. On its own benchmark runs and at a promotional price, Microsoft says it is comparable to OpenAI’s GPT-6 Luna at 65% of the cost, and to Anthropic’s Claude Haiku 5.5 at 20%.

Those cost comparisons carry conditions that Microsoft spelled out in its notes. Every model ran at maximum reasoning effort, and three of the four coding benchmarks ran on MSBench, Microsoft’s internal evaluation platform, through the GitHub Copilot command-line tool.

Scores and costs for the fourth benchmark come from the benchmarking site Artificial Analysis. Both Code 2 models are costed at their 50% promotional prices, and the posts print no list price for either one. “Performance and cost comparisons vary by workload, model configuration, reasoning effort, and pricing assumptions,” Microsoft said.

In Microsoft’s own blind side-by-side evaluations of front-end work such as interfaces, games and data visualizations, judged by 258 annotators, Code 2’s results were preferred over OpenAI’s GPT-6 Sol 64% of the time, the company said.

Code 2 and Code 2 Flash are available in GitHub Copilot, and Code 2 Flash is in limited-access preview in Microsoft Foundry, the company’s platform for building AI applications, according to the product post. Nadella’s edited keynote remarks said both models were generally available on GitHub Copilot and “also available in Foundry”. The product post adds that a release on Open Router, Vercel, Hermes, Open Code and Kilo is coming “soon”, without saying which of the two models.

Microsoft has also tuned a version of Code 2 Flash, MAI Work Flash, to serve products across Microsoft 365 and Fabric IQ. In Word, the company says, that model performs similarly to GPT-6.1 Sol on five internal benchmarks while using about 42% fewer input tokens, the units of text a model reads or writes and is billed by, per call in production.

## Cyber 2 Flash matches GPT-6 Astra on a test Microsoft costed at list prices

On CyberGym-E2E, a benchmark of real-world vulnerability discovery, Cyber 2 Flash completed 92.3% of tasks against 91.5% for OpenAI’s GPT-6 Astra, Microsoft said. It used half the reasoning budget, at less than 2% of the cost per task, with both models costed at list prices.

Microsoft costed that test at the list prices in the table below and says the result reflects offline evaluation cost, not customer spend. On those prices, every Cyber 2 Flash token costs 1% of the matching GPT-6 Astra token, for input, cached input and output alike.

The company also disclosed that 92 of the benchmark’s 905 tasks are built on the same open-source projects and vulnerability reports as some of its training data. It excluded those tasks for every model and reports results on the remaining 813, adding that the benchmark’s reference solutions were never used in training.

On WebExploitBench, a second security test, Cyber 2 Flash scored 40.4% against 38.4% for OpenAI’s GPT-5.6 Cyber at the same reasoning budget. Microsoft did not compare it with GPT-6 Cyber, which it said is available only to select customers, and plans to share those results “in the coming weeks”.

The new model can take in up to one million tokens at a time, four times as much as MAI Cyber 1.1 Flash. Microsoft says it is also priced 86% lower on output tokens than that model.

MAI Cyber models already handle almost 40% of scans in MDASH, Microsoft’s vulnerability-scanning service, and 90% of scans that began with an MAI model stayed with it, the company said. On 27 October, Microsoft AI had put the MAI share at almost 50% of all MDASH scans; the new post does not explain the difference.

Cyber 2 Flash is “coming exclusively to Microsoft security products”, Microsoft said. In MDASH it will automatically replace the existing model in the MAI profile for eligible customers from mid-December, and Nadella said Microsoft is integrating Cyber 2 with Perception, its agentic security system, which he described as now generally available.

**MAI models against rivals, as Microsoft reports them (in %)**

| Model | Rival | Result | Cost | 
|---|---|---|---|
| Code 2 | GPT-6.1 Sol | 98 | 40 | 
| Code 2 | Claude Opus 5.5 | 96 | 5 | 
| Code 2 Flash | GPT-6 Luna | Comparable | 65 | 
| Code 2 Flash | Claude Haiku 5.5 | Comparable | 20 | 
| Cyber 2 Flash | GPT-6 Astra | 92.3 vs 91.5 | Under 2 | 

*Result is the MAI model’s performance as a share of the rival’s; for Cyber 2 Flash it is the two models’ task-success rates on CyberGym-E2E. Cost is the MAI model’s cost as a share of the rival’s. Code 2 is averaged over DeepSWE 1.1, Terminal Bench 2.1, SWE-Atlas and FrontierCode v1.1, Code 2 Flash over the first three, and both are costed at 50% promotional prices. Every row is Microsoft’s own run or selection.*

## Image and voice models arrive with printed prices

Image 2.7 ranks first on the image-editing leaderboard of Artificial Analysis as of 16 November, Microsoft said. It scores 1,215 Elo points, a rating built from head-to-head votes, across 5,478 votes.

That puts it 4 points ahead of OpenAI’s GPT Image 2.6 Sunburst (max), at what Microsoft says is one-fifth of the cost. When Microsoft released Image 2.7 in preview on 27 October, for editing only, it placed second on the Artificial Analysis image-editing leaderboard.

In public preview on Microsoft Foundry, Image 2.7 costs about $35 per 1,000 images. The faster Image 2.7 Flash costs about $10, less than a third as much.

MAI Image models are also now the default behind Bing Image Creator, where Microsoft reports usage growing at a compound weekly rate of 14.7% since June.

Realtime 1, Microsoft’s first speech-to-speech model, starts at $0.01 per audio minute. Reasoning is billed separately, at $1 per million input tokens and $5 per million output tokens.

Microsoft says Realtime 1 beats OpenAI’s GPT Realtime 2.1 on quality, speed and price in public benchmarks and its own product evaluations. It is offered in MAI Labs and in a limited-access Foundry preview, and preview customers will not be billed until a public preview begins.

Decision 1.1, which routes each request so that bigger, pricier models are used only when needed, is now generally available and has been used by more than 4,000 customers, Microsoft said. Its transcription model, MAI Transcribe 2, powers Dragon Copilot Transcribe, part of its Dragon Copilot healthcare product, across 10 million patient encounters a month.

**List prices Microsoft printed for the new models and its benchmark rival (in $)**

| Model | Unit | Price | 
|---|---|---|
| Cyber 2 Flash | Million input tokens | 0.10 | 
| Cyber 2 Flash | Million output tokens | 0.50 | 
| GPT-6 Astra | Million input tokens | 10 | 
| GPT-6 Astra | Million output tokens | 50 | 
| Image 2.7 | 1,000 images | 35 | 
| Image 2.7 Flash | 1,000 images | 10 | 
| Realtime 1 | Audio minute | 0.01 | 

*GPT-6 Astra is shown at its Azure OpenAI Global list price as Microsoft’s notes give it; cached input is $0.01 per million tokens for Cyber 2 Flash and $1 for GPT-6 Astra. Image prices are approximate as printed. Realtime 1 reasoning is billed separately. Cyber 2 Flash will be offered only inside Microsoft security products.*

## Licensed weights, and Microsoft’s spending on capacity

The launch also brings MAI Licensed Weights, which will let organizations deploy and customize Microsoft’s models in their own Azure environments. Customers’ data and adaptations remain theirs and train Microsoft’s models only with their express agreement, the company said; the offer is “coming soon” through its Frontier Company engagements.

The customer results Microsoft cites come from models adapted for particular tasks. AT&T’s customized model outperformed GPT-6.1 Sol on standard 5G network analysis at one-fiftieth of the cost, it said. In an Alaska Airlines evaluation, a tuned Code 2 Flash raised accuracy in classifying aircraft maintenance reports to 91.1% from 81.2%.

The MAI line sits beside rivals’ models on Microsoft’s own platform. Nadella said OpenAI’s GPT 6.2 is coming to Foundry, as are all of Meta’s latest models.

The price pitch also comes from a company spending heavily on capacity. In the quarter to 30 September, Microsoft’s additions to property and equipment reached $39.4 billion, against $19.4 billion a year earlier, its earnings release shows. Research and development cost $9.0 billion.

Microsoft AI’s keynote transcript says chief information and finance officers tell it that “token spend is going through the roof”, and adds that “at frontier prices, the math just isn’t sustainable”.

In The Edge’s reading, the firmest news is the pricing customers can check for themselves: the image and voice prices are posted, while the coding and security gains rest on tests Microsoft ran or chose, and Code 2’s cost edge is measured at a promotional price.

**What to watch:** From mid-December, Cyber 2 Flash replaces the existing model in the MAI profile of MDASH for eligible customers, the only date Microsoft has set for the new security model to reach customers.

*Method: model details, benchmark results, availability and prices are from Microsoft AI’s posts of 17 November 2026 (the Ignite overview, the posts on MAI Code 2 and Code 2 Flash, MAI Cyber 2 Flash, MAI Image 2.7, MAI Realtime 1 and MAI Licensed Weights, and the MAI keynote transcript), Satya Nadella’s edited keynote remarks on the Official Microsoft Blog of the same date, and Microsoft AI’s posts of 27 October 2026 on MAI Cyber 1.1 Flash and MAI Image 2.7; spending figures are from Microsoft’s earnings release for the first quarter of fiscal year 2027, dated 28 October 2026; every benchmark result is Microsoft’s own, and the leaderboard placings are Artificial Analysis rankings as Microsoft reports them; price ratios not attributed to Microsoft are The Edge’s calculations.*

*Sources: Microsoft, The Edge.*
