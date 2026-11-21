---
id: collect-261121-veille-ia/qwen/qwen4-max-is-here-alibaba-s-2-4t-agent-powerhouse-tested
title: "Qwen4-Max Is Here: Alibaba's 2.4T Agent Powerhouse Tested"
domain: qwen
role: reference
task: article
actors: ["Alibaba"]
dates: []
keywords: ["qwen4-max", "alibaba", "qwen", "qwen3", "november", "model", "gated", "tokens"]
source: https://www.intelligentliving.co/qwen4-max-alibaba-agent-tested/
source_anchor: ""
source_lines: [1, 134]
section: "Qwen"
---
Alibaba flipped the switch on the Qwen 4 era on November 16, 2026. Qwen4-Max, the first general release model in the Qwen 4 family, is a 2.4-trillion-parameter mixture-of-experts system built for coding, professional work, and long-running agents, with native vision, a one-million-token context window, and reasoning turned on by default. This explainer unpacks what it actually is, what Alibaba claims, what independent testing shows so far, what it costs, and what remains unreleased.

Qwen4-Max, model ID qwen4-max, went generally available on November 16, 2026, in Hangzhou, five days after a preview version appeared on Alibaba’s Token Plan. It is the flagship of the four Qwen 4 tiers Alibaba named at its Apsara conference in September, and the only Qwen 4 model you can use today.

The headline numbers carry over from the prior generation in size but not in behavior. Like the September update to Qwen3.8 Max, the backbone totals 2.4 trillion parameters, but Qwen4-Max activates only 71 billion per token, about 25% fewer than the roughly 95 billion active in Qwen3.8-Max. On top sits a 328-billion-parameter n-gram embedding layer, a giant phrase memory that adds capacity without much per-token compute.

| Spec | Qwen4-Max | 
|---|---|
| Status | Generally available November 16, 2026, as qwen4-max; preview ID qwen4-max-preview retires November 25 | 
| Parameters | 2.4T backbone, 71B active per token, plus 328B n-gram embedding parameters | 
| Architecture | Sparse mixture-of-experts with Gated DeltaNet and Qwen Sparse Attention layers, gated residual connections, Muon-trained | 
| Context window | 1,000,000 tokens; max input 991,808 tokens, or 983,616 with thinking on | 
| Output limit | 131,072 tokens without thinking; 262,144 tokens for thinking plus answer combined | 
| Inputs | Text, images, video, and PDFs | 
| Thinking | On by default at high effort, with low, high, and xhigh levels | 
| Tools | Function calling, web search, code interpreter, MCP tools, structured output | 
| API compatibility | OpenAI Chat Completions and Responses, Anthropic Messages, DashScope | 
| Open weights | Not released; API only | 

The architecture follows the direction Alibaba previewed in August with Qwen3.8-Flash-Next, which the Qwen team called an early preview of Qwen 4: hybrid Gated DeltaNet linear attention for memory, sparse attention for retrieval, parallel gated residual pathways, and a large n-gram lookup table. Alibaba reports its in-house base-model tests show 5 to 13% lower bits per byte than the Qwen3.8-Max base model, where lower is better. That is a vendor result on a vendor test, not an independent score.

Every score in this section is vendor-reported, taken from the chart Alibaba published with the launch. Alibaba compared Qwen4-Max against Qwen3.8-Max-0902, Claude Opus 5.5, GPT-6.1 Sol, GPT-6 Astra, and Kimi K3.1 on 16 benchmarks, plus its own Work360 test. Alibaba notes that several rows reuse third-party leaderboards, and it ran JobBench itself on an updated dataset. Do not read these as independent verification.

On that chart, Qwen4-Max leads its predecessor across the shared set and takes four outright tops. The table below carries the full numbers; the pattern is what matters. The biggest jumps are long-horizon agents and clinical reasoning, while terminal replication and visual chart work trail the leaders by wide margins.

| Benchmark, vendor-reported | What it tests | Qwen4-Max | Qwen3.8-Max | Top in Alibaba chart | 
|---|---|---|---|---|
| DeepSWE v1.1 | Agentic coding | 75.7 | 69.3 | Qwen4-Max, 75.7 | 
| CodeArena Elo | Web development | 1750 | 1676 | Claude Opus 5.5, 1816 | 
| Terminal-Bench 4.0 | Agentic terminal coding | 48.5 | 38.9 | Kimi K3.1, 67.2 | 
| ProgramBench | Black-box software replication | 31.5 | 28.0 | Claude Opus 5.5, 65.0 | 
| AA-Briefcase v1.1 Elo | Agentic knowledge work | 1686 | 1605 | Claude Opus 5.5, 1801 | 
| Agents’ Last Exam v1.1 | Long-horizon tasks | 46.7 | 27.9 | Qwen4-Max, 46.7 | 
| JobBench | Professional job tasks | 68.5 | 65.3 | Claude Opus 5.5, 75.4 | 
| APEX-Agents v1.1 | Professional services tasks | 72.1 | 63.3 | Claude Opus 5.5, 73.5 | 
| HLAB-Vals-1.1 | Legal agent work | 21.3 | 15.4 | Qwen4-Max, 21.3 | 
| AutomationBench-AA | Agentic SaaS workflows | 72.5 | 56.2 | Kimi K3.1, 73.3 | 
| BioMysteryBench | Bioinformatics research | 85.0 | 84.1 | Qwen4-Max, 85.0 | 
| HealthBench Professional | Clinical reasoning | 60.0 | 39.6 | GPT-6 Astra, 66.0 | 
| Humanity’s Last Exam | Multidisciplinary reasoning | 51.7 | 43.9 | Claude Opus 5.5, 59.8 | 
| Chartography | Visual chart understanding | 54.2 | 42.0 | GPT-6 Astra, 71.1 | 
| Video-Index | Video understanding | 68.1 | 63.2 | GPT-6 Astra, 76.7 | 

Alibaba also introduced Work360, its own benchmark built on 26 domains, 182 subdomains, and 721 specialized fields of professional work. Qwen4-Max scored 52.69, up from 47.74 for Qwen3.8-Max. Alibaba says the new model uses about 35% fewer tokens at high reasoning effort and 24% fewer at xhigh effort, cutting average cost per Work360 task from $4.43 to $1.67, a 62% reduction. There is no public Work360 leaderboard, and Alibaba designed and ran the test, so treat the efficiency claim as a vendor claim until others reproduce it.

Artificial Analysis independently tested Qwen4-Max on its Intelligence Index v5.0.1, a standardized run across nine evaluations including AA-Briefcase, AutomationBench-AA, Terminal-Bench 4.0, FrontierCode-AA, AA-Omniscience, GDP.pdf, HLE-Diamond, CritPt hard subset, and Terminal-Bench-Science. This is the number to watch for cross-lab comparison, because every model runs the same harness.

Qwen4-Max scores 43 on the Intelligence Index, 9 points ahead of Qwen3.8-Max and 3 points behind Kimi K3.1 Pro. That is the middle of the frontier pack, not the top of it. The token story underneath is mixed: about 105,000 output tokens per task in that run, 39% fewer than the roughly 171,000 for Qwen3.8-Max, yet still far above the median model. Cost per Intelligence Index task is $3.27, 67% less than the $10.02 for Qwen3.8-Max and below the $3.68 for Kimi K3.1 Pro.

| Artificial Analysis, independent | Qwen4-Max | Context | 
|---|---|---|
| Intelligence Index | 43 | Plus 9 vs Qwen3.8-Max, minus 3 vs Kimi K3.1 Pro | 
| Cost per Index task | $3.27 | 67% less than Qwen3.8-Max at $10.02 | 
| Output tokens per task | About 105k | 39% fewer than Qwen3.8-Max | 
| Total Index output | 127M tokens | Very verbose vs 65.1M median | 
| Time per task | 31.3 minutes | Slow vs 12.3 minute median; 56 tok/s, first answer at 146.14s | 
| List pricing | $1.50 in, $4.50 out per 1M | Blended $0.80; 95% cache discount | 
| Context measured | 984k tokens | Text, image, and video in; text out; reasoning model | 

The breakout below comes from the same independent source, via its model page and standardized benchmark feed. Higher is better on all rows except cost and time.

| Individual benchmark, Artificial Analysis independent | Score | 
|---|---|
| AutomationBench-AA, agentic SaaS workflows | 72.6% | 
| GDPval-AA, agentic real-world work tasks | 65.4% | 
| APEX-Agents-AA, professional services agent tasks | 59.1% | 
| EnterpriseOps-Gym-AA, agentic business operations | 48.5% | 
| Terminal-Bench 4.0, agentic terminal coding | 48.5% | 
| AA-AnalystAgent, spreadsheet and document analysis | 46.3% | 
| HLE-Diamond, reasoning and knowledge | 44.9% | 
| CritPt v2 hard subset, physics reasoning | 43.0% | 
| FrontierCode-AA 2.0, open-source repo engineering | 26.5% | 
| GDP.pdf, professional document reasoning all-pass | 26.2% | 
| Terminal-Bench-Science 0.1, agentic science workflows | 23.3% | 
| Harvey LAB-AA, legal agentic work | 9.4% | 

Capability indexes from the same independent run add texture: Finance and Accounting 54.1, Strategy and Ops 56.9, Legal 49.6, Healthcare and Medical 48.2, Engineering 50.1, and Economics 51.3. The pattern matches the vendor chart. Agentic office and business work looks stronger than legal and medical depth, and the model trades speed for deliberation, which is why time per task ranks near the bottom of its price tier even as cost per task looks competitive.

Vals.ai independently tested Qwen4-Max on Vals Index v2.2 and its Finance Index, at xhigh reasoning effort, temperature 1.0, top_p 0.95, and top_k 20. Vals builds its scores from real professional tasks rather than exam-style questions, so it reads as a complement to Artificial Analysis, not a repeat.

Qwen4-Max posts 63.20% plus or minus 1.01 on the Vals Index, the highest-scoring Chinese model in that run and up from 59.05% for Qwen3.8-Max-0902. It also scores 61.99% on the Vals Finance Index. Cost per Vals test is $9.33 with a 64-minute, 39-second latency in the Vals harness—a different workload from the Artificial Analysis test, so the two dollar figures are not directly comparable.

Read the 43 and the 63.20% as answers to different questions. Artificial Analysis runs a broad general-intelligence gauntlet: nine evaluations spanning agentic knowledge work, terminal coding, repo engineering, science workflows, document reasoning, physics, and hallucination control, folded into a single index. Vals builds its scores around real-world finance, legal, and coding workflows with heavy tool use and filing-style outputs. A model can be a strong finance agent and a mid-pack generalist at the same time, and that is what the two numbers show.

For a buyer, the split translates cleanly. Qwen4-Max wins on office-style agent work: AutomationBench-AA at 72.6%, APEX-Agents-AA at 59.1%, EnterpriseOps-Gym-AA at 48.5%, GDPval-AA at 65.4%. It loses on knowledge depth and hard science: GDP.pdf at 26.2%, CritPt hard physics at 43.0% against much stronger frontier numbers, FrontierCode repo engineering at 26.5%, legal agent work at 9.4% on Harvey LAB-AA, and Terminal-Bench-Science at 23.3%. On the Vals side it confirms the same shape: finance and app-building highs against legal retrieval and tax middles. The vendor chart showing a sixteen-benchmark sweep over its predecessor is a comparison against Qwen3.8-Max, not against the frontier, and the independent 43 is the number that puts it 3 points behind Kimi K3.1 Pro and in the middle of the pack overall. Treat Alibaba’s chart as proof of progress and the 43 as proof of position.

| Vals.ai benchmark, independent | Score | Rank and note | 
|---|---|---|
| Vals Index | 63.20% | Top Chinese model, ahead of Kimi K3.1 Pro at 62.52% | 
| Vals Finance Index | 61.99% | Strong finance showing | 
| Finance Agent v2.1 | 66.68% | About 51 tool calls per question vs 39 typical; leads with conclusion, calculation, and filing cite | 
| Vibe Code Bench v1.1 | 86.66% | 21 of 50 apps pass every test vs 16 of 50 for Qwen3.8-Max | 
| Terminal-Bench 4.0 | 48.48% | Up from 38.9% for Qwen3.8-Max; gains in ML and ops, still weak on hardware tasks | 
| Tax Agent Bench | 40.35% | Regression from 49.7% for Qwen3.8-Max; answers 40% shorter with 30% fewer tool calls | 
| Vals Legal Index | 70.71% | Solid but mid-pack legally | 
| Legal Research Bench | 41.35% | Weaker legal retrieval result | 
| EMB v1.1 | 70.83% | Mid-pack | 
| HLAB-Vals-1.1 | 21.25% | Strong agent-legal crossover | 

The finance spike plus the tax dip is the most useful detail for buyers. On Finance Agent work Qwen4-Max ranks near the top of the Vals table, first on qualitative analysis at 84.1% and second on adjustments at 71.0%. On Tax Agent work it slides from fourth to the middle of the pack. Same model, same harness family, opposite moves. That split is exactly why single-index summaries mislead, and why teams should test their own professional tasks before switching.

Qwen4-Max lists at $1.50 per million input tokens and $4.50 per million output tokens, including thinking, on QwenCloud’s international Singapore list, 25% below the $2.00 and $6.00 list for Qwen3.8-Max. Cached input reads at $0.075 per million, 5% of the input rate, with 5-minute cache writes at $1.875 and 1-hour writes at $3.00. Pay-as-you-go calls in Singapore include 1 million free tokens. Beijing and the global regions share one price list while Singapore has its own, so check your region before budgeting.

The rate card is only half the bill, and this is where cheap tokens meet expensive tasks. At $1.50 in and $4.50 out, Qwen4-Max looks inexpensive per million tokens. At roughly 105,000 output tokens per Intelligence Index task, most of them reasoning billed at the output rate, the per-task bill lands at $3.27 on Artificial Analysis and $9.33 on the heavier Vals harness. A 1M-context model that thinks out loud is costly per task almost by definition, because long contexts get re-read and long reasoning gets billed. Alibaba’s real saving is relative: fewer tokens per job than Qwen3.8-Max, so $3.27 instead of $10.02 on the same workload. Judge it on cost per task for your workload, not on the per-token sticker.

One budgeting catch carries over from the Qwen 3.8 generation. Thinking is billed as output and defaults to high effort with a 131,072-token budget, so the effort setting is the first cost control to learn. Alibaba also adds Token Plan subscriptions, Individual and Team editions with monthly Credits for use inside coding and agent tools, alongside QwenWork, Qoder, and QwenStudio apps. If you used the preview, move off qwen4-max-preview before its window ends at 24:00 Beijing time on November 25.

Qwen4-Max is API only. There are no weights to download, no model card with a local checkpoint, and no community quantization to run at home. Alibaba’s launch post says smaller Qwen 4 models will be open-sourced, with no names, sizes, licenses, or dates.

Access runs through QwenCloud pay-as-you-go under qwen4-max, through Token Plan Credits inside supported coding tools such as Claude Code, Codex, and OpenCode, or inside QwenWork, Qoder, and QwenStudio. The API speaks OpenAI Chat Completions and Responses plus Anthropic Messages formats, so an existing client connects with a base URL, a key, and the model string. Before swapping the model string, note these behavior changes:

- Thinking defaults to on at high effort, so set the effort level deliberately rather than leaving it untouched.
- The 262,144-token output limit caps thinking and the final answer combined, so budget for both rather than the answer alone.
- Multi-turn calls retain reasoning by default, so preserve assistant messages and reasoning when returning tool results.
- Caching requires explicit breakpoints on shared prefixes such as system prompts and tool definitions.
- Reasoning can return as a readable summary plus an encrypted copy that restores context on later turns.

If you need a self-hosted Qwen flagship today, that remains Qwen3.8-Max, whose 2.4T open checkpoint arrived in August. Qwen4-Max is the stronger and cheaper API model on Alibaba’s numbers. Qwen3.8-Max is the one you can download. For local context on what the prior open 27B can do, see our Nanbeige vs Qwen 27B agent test.

Nothing here is released, so treat this section as expectations, not specs. Alibaba has said only that smaller Qwen 4 models will be open-sourced. It has not named Plus, Flash, or a 27B, and it has not given dates, parameter counts, context lengths, licenses, or prices for any of them.

The hope in the community is straightforward: a 27B or 31B class mid-size open model for single-GPU workstations, plus smaller 9B and 4B models people can run locally on laptops or modest cards. That hope is grounded in what came before. Qwen3.8-27B on Hugging Face was a very impressive release, a 27.8-billion-parameter dense vision-language model under Apache 2.0 that traded blows with far larger closed models on coding and computer-use tasks and ran at home in 4-bit form on a 24 GB card. Expectations for its Qwen 4 successor are high precisely because that 27B reset what a local model could do.

Do not list specs for models that are not out. There is no confirmed Qwen 4 27B size, no confirmed 31B alternative, no confirmed 9B or 4B, and no confirmed license or hardware target. The Qwen 3.8 pattern, where the flagship API landed first and open weights followed about ten days later, is precedent, not a promise. Until Alibaba publishes a model card, the only Qwen 4 model that exists is Max, behind the API.

Qwen4-Max is a real step up from Qwen3.8-Max on both vendor and independent numbers, with the clearest gains in agentic coding, long-horizon tasks, and finance-flavored office work, at a lower per-token price and with fewer tokens per task than its predecessor. Independent testing also draws the boundaries honestly: middle of the frontier on the Artificial Analysis index at 43, top Chinese model on the Vals Index at 63.20%, slow and verbose per task, weaker on hardware terminal work, legal retrieval, and tax detail than its finance highs suggest.

If you run agents or professional coding workflows through an API, it deserves a head-to-head against your current model:

- Run a fixed set of 20 to 50 prompts from your own product on your current model and save outputs plus token counts.
- Run the same set on qwen4-max at high effort and compare quality next to tokens used.
- Weight cost per task alongside accuracy, since Alibaba’s cost claim rests on using fewer tokens per job.

If you run models at home, wait. The open Qwen 4 family people want, led by a 27B-class successor to a great local release, is still only a sentence in a launch post.
