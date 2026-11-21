---
id: collect-261121-veille-ia/copilot-microsoft/microsoft-mai-code-2-and-code-2-flash-in-github-copilot-plans-benchmarks-pricing
title: "Microsoft MAI Code 2 and Code 2 Flash in GitHub Copilot: plans, benchmarks, pricing"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["code", "copilot", "microsoft", "flash", "github", "model", "models", "both"]
source: https://www.datastudios.org/post/microsoft-mai-code-2-github-copilot-code-2-flash-free-plan
source_anchor: ""
source_lines: [1, 237]
section: "Copilot_Microsoft"
---
Microsoft launches MAI Code 2 in GitHub Copilot, with Code 2 Flash on Free

4 days ago

8 min read

Updated: 3 days ago

Microsoft released MAI Code 2 and MAI Code 2 Flash on November 17, 2026, the opening day of Ignite 2026, and made both coding models selectable in GitHub Copilot on the same day. MAI Code 2 is the larger model, aimed at deep debugging, multi-file implementation, refactoring and repository-wide reasoning, while Code 2 Flash is a lightweight model for code explanations, targeted edits, routine fixes and fast iteration. Both come from Microsoft AI, the company's in-house model group, and both are described as built for agentic coding workflows.

Availability splits by plan. According to the GitHub changelog, MAI Code 2 is offered on Copilot Pro, Pro+, Max, Business and Enterprise, and MAI Code 2 Flash is offered on every plan, including Copilot Free and Copilot Student. Both models are billed under Copilot's usage-based billing, and the model picker exposes them in Visual Studio Code, Visual Studio, JetBrains IDEs, Xcode, Eclipse, the Copilot CLI, the Copilot cloud agent, the GitHub Copilot app, github.com and GitHub Mobile on iOS and Android.

Microsoft's headline claim is about price-performance. Averaged over four coding benchmarks (DeepSWE 1.1, Terminal Bench 2.1, SWE-Atlas and FrontierCode v1.1), the company says MAI Code 2 delivers 98% of GPT-6.1 Sol's performance at 40% of its cost and 96% of Claude Opus 5.5's performance at 5% of its cost. A footnote in the announcement states that the MAI costs in those comparisons are calculated at a 50% promotional price, and the per-token prices for the two new models were not yet listed on GitHub's official Copilot pricing page when Data Studios checked it on November 17, 2026.

··········


MAI CODE 2 AND CODE 2 FLASH: WHAT MICROSOFT RELEASED.

The two models share a purpose and divide the work by size. Microsoft positions MAI Code 2 for what it calls everyday enterprise coding: taking code to production, debugging across files, implementing features that touch several modules, reasoning over a whole repository and building user interfaces. The announcement says the model produces changes that maintainers "would merge", which is a vendor claim about review quality rather than a published metric.

MAI Code 2 Flash targets high-volume, low-latency work. Microsoft lists code explanations, targeted edits, routine fixes and rapid iterations as its intended tasks, which is the kind of traffic that dominates inline chat and quick agent steps inside an IDE. The Flash data card, published alongside the launch, gives more technical detail than the announcement itself. It describes Code 2 Flash as a 12-billion-parameter model built on Microsoft's internal MAI 12B base model, trained to transfer software engineering skills from a larger teacher model into a smaller one suited to high-volume developer use.

Microsoft is also explicit about the limits of both models. The announcement and the Ignite keynote state that MAI Code 2 and Code 2 Flash are not yet tuned for fundamental science research or very long-horizon agentic work. Teams that run multi-hour autonomous coding sessions can keep using the other models in Copilot's model picker for that work.

The table below summarizes what Microsoft has and has not published for each model as of launch day.

Explanations, targeted edits, routine fixes, fast iteration

Parameters

Not disclosed

12B (data card)

Context window

Not disclosed

Not disclosed

GitHub Copilot

Pro, Pro+, Max, Business, Enterprise

All plans, including Free and Student

Microsoft Foundry

Not announced

Limited-access preview

Per-token price on GitHub

Not yet listed

Not yet listed

........

··········

GITHUB COPILOT PLANS AND SURFACES FOR MAI CODE 2.

For individual developers, the plan determines which of the two models appears in the picker. A Copilot Free or Student account sees Code 2 Flash only, so Flash is the model from this release that developers can use without a paid subscription. Pro, Pro+ and Max subscribers see both models. In organizations, Business and Enterprise seats get both, subject to the administrator's model policy. The changelog adds that subscribers on eligible annual plans also have access to both models.

Administrators control access through the model policy in Copilot settings. Under GitHub's default model enablement, new models are switched on automatically unless an administrator has disabled the global default or turned off the specific model. Organizations with an approved-model list should check that policy now, because MAI Code 2 and Code 2 Flash will appear for users wherever automatic enablement is still active.

........

Copilot plan

MAI Code 2

MAI Code 2 Flash

Copilot Free

No

Yes

Copilot Student

No

Yes

Copilot Pro

Yes

Yes

Copilot Pro+

Yes

Yes

Copilot Max

Yes

Yes

Copilot Business

Yes

Yes

Copilot Enterprise

Yes

Yes

........

Billing follows Copilot's usage-based model. Each model has its own per-token rates, the token cost of every request is converted into GitHub AI Credits at 1 credit = $0.01, and usage beyond a plan's included allowance is charged at those rates. Because the official GitHub price list did not yet show rates for MAI Code 2 or Code 2 Flash on November 17, developers on metered plans cannot yet calculate the credit cost of a session with either model from published figures. The plan-level allowances that apply are covered in our comparison of GitHub Copilot Pro and Pro+.

MAI Code 2 also reaches users who never open the model picker. A separate GitHub changelog entry dated November 17 adds MAI-Code-2 to Copilot Fusion Standard, the effort level of GitHub's automatic routing system that selects and coordinates models per task. Fusion became generally available on all plans last month, as described in our report on GitHub Copilot Fusion and automatic model routing.

··········

[[ADPLUS_300x250_1]]

MAI CODE 2 BENCHMARK CLAIMS AND HOW TO READ THEM.

Microsoft frames every result relative to a competitor, which makes the methodology footnotes essential. All models in the comparison ran at maximum reasoning effort. DeepSWE 1.1, Terminal Bench 2.1 and SWE-Atlas were run on MSBench, Microsoft's internal evaluation platform, using the GitHub Copilot CLI without subagents, while FrontierCode v1.1 scores and costs were taken from Artificial Analysis. The MAI costs reflect a 50% promotional price, and Microsoft adds that results vary with workload, configuration, reasoning effort and pricing assumptions.

The relative cost figures describe the cost of running the benchmarks, which combines per-token price with the number of tokens each model consumed. GitHub's official price list shows why the two cannot be separated from the outside. GPT-6 Luna and Claude Haiku 5.5 carry the same default Copilot rates of $0.10 per 1M input tokens and $0.50 per 1M output tokens, yet Microsoft reports Code 2 Flash at 65% of Luna's cost and 20% of Haiku's. That gap can only come from different token consumption on the benchmark tasks, so the percentages cannot be converted into an implied per-token price for the MAI models.

For visual work, Microsoft says reviewers preferred MAI Code 2 over GPT-6 Sol in blind front-end app evaluations covering interfaces, games, data visualizations, 3D scenes and SVG graphics. The model announcement gives the preference rate as 67% across 258 annotators, while Microsoft AI's Ignite overview page gives 68%. Neither page identifies the prompts or the scoring rubric, so the result is a vendor evaluation without independent replication at launch.

··········

MAI CODE 2 VERSUS GITHUB COPILOT MODEL PRICES ON THE OFFICIAL LIST.

GitHub's official Copilot price list gives a reference point for where the new models are likely to sit, even though their own rows are missing. The previous Microsoft coding model on the list, MAI-Code-1.1-Flash, is priced at $0.20 per 1M input tokens, $0.02 cached and $1.20 output. The table below shows the official rates of the models Microsoft names in its comparisons, as listed on the GitHub Docs page on November 17, 2026, in USD per 1M tokens at the default context tier.

........

Model on GitHub Copilot

Input

Cached input

Cache write

Output

MAI-Code-1.1-Flash

$0.20

$0.02

Not listed

$1.20

GPT-6 Luna

$0.10

$0.01

$0.125

$0.50

Claude Haiku 5.5

$0.10

$0.01

$0.125

$0.50

GPT-6.1 Sol

$2.00

$0.10

$2.50

$10.00

Claude Opus 5.5

$4.00

$0.20

$5.00

$20.00

MAI Code 2 / Code 2 Flash

Not yet listed

Not yet listed

Not yet listed

Not yet listed

........

GPT-6.1 Sol and GPT-6 Luna switch to higher long-context rates above 272K input tokens, and Claude Haiku 5.5 does so above 100K, so the defaults shown apply to ordinary IDE requests. Once GitHub publishes the MAI rows, developers will be able to check Microsoft's relative-cost claims against their own usage logs, because Copilot reports token consumption per request.

··········


MAI CODE 2 FLASH TRAINING DATA AND MICROSOFT'S WIDER MAI ROLLOUT.

The Code 2 Flash data card lists crawled web data from July 2010 to March 2026, more than 10 trillion text tokens and more than 1 billion images used for vision-language and software engineering tasks. Post-training synthetic data was generated through November 2026, partly with third-party models identified as GPT-5.6 Sol and GPT-6 Astra and partly with internal Microsoft models. English dominates, with German, Spanish, French, Italian, Portuguese, Simplified Chinese, Russian, Hindi, Japanese, Korean, Arabic and Hungarian also included, and the card says coverage and quality vary by language. The model was placed on the EU market on the release date.

Outside GitHub, Code 2 Flash is in limited-access preview in Microsoft Foundry, and Microsoft says both models are coming to OpenRouter, Vercel, Hermes, OpenCode and Kilo, without dates. Microsoft also derived a model called MAI Work Flash from Code 2 Flash and uses it inside Microsoft 365 and Fabric IQ. The company says MAI Work Flash performs similarly to GPT-6.1 Sol in Word while using about 42% fewer input tokens per call in production, and that it uses up to 28% fewer tokens on instruction following and tool calling in Teams.

Custom versions are part of the pitch. Through its custom models program and the new MAI Licensed Weights offer, Microsoft lets selected customers tune MAI models on their own data and run them in their own Azure environment. AT&T says its custom Code 2 Flash outperforms GPT-6.1 Sol on standard 5G network analysis at 50 times lower cost, and Woven by Toyota says a Flash model fine-tuned on its MISRA fix history produced ready-to-merge fixes in 44% of cases where frontier models needed human rework, at under one-tenth of the cost. Both figures are customer statements published by Microsoft.

··········

MAI CODE 2 IN GITHUB COPILOT: CONFIRMED FACTS AND OPEN QUESTIONS.

The confirmed part of the launch is concrete. MAI Code 2 and MAI Code 2 Flash have been available in GitHub Copilot since November 17, 2026, Code 2 Flash is the option open to Free and Student accounts, MAI Code 2 requires a paid individual plan or an organization seat, both are billed through usage-based AI Credits, and MAI-Code-2 is already part of Copilot Fusion Standard. The Flash data card fixes its size at 12 billion parameters and documents its training data and languages.

The open questions concern cost and scope. GitHub has not yet published per-token rates for either model, Microsoft's relative-cost figures rest on a 50% promotional price whose end date is not stated, and the context window of both models is undisclosed. The benchmark results come from Microsoft's own evaluation platform, apart from the FrontierCode figures sourced from Artificial Analysis. Teams evaluating MAI Code 2 for daily coding work have enough to start testing in Copilot today, and they should wait for the official price rows before projecting monthly credit spend.
