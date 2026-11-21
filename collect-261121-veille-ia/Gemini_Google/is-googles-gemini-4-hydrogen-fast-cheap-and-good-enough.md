---
id: collect-261121-veille-ia/unifi-ubiquiti/is-googles-gemini-4-hydrogen-fast-cheap-and-good-enough
title: "Is Google’s Gemini 4 Hydrogen Fast, Cheap and Good Enough?"
domain: unifi-ubiquiti
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["gemini", "hydrogen", "google", "model", "tokens", "default", "output", "high"]
source: https://medium.com/@amitsrivastava115/is-googles-gemini-4-hydrogen-fast-cheap-and-good-enough-1adacfa22cbc
source_anchor: ""
source_lines: [1, 431]
section: "Gemini_Google"
---
Is Gemini 4 Hydrogen the fast model that finally makes Argon optional?

Two days after launch, Google’s mid-tier Gemini 4 model is already being discussed as more than a Flash replacement. On the Artificial Analysis Intelligence Index it scores 51 at its default high thinking level, one point above Gemini 4 Argon, and 54 at xhigh. It costs $1.50 per million input tokens and $7.50 per million output tokens, flat across a unified 1,048,576-token window. The question is whether those figures mean Hydrogen can stand in for Argon on everyday coding and agentic work, or whether the gaps in terminal tasks, computer use, and unpublished classic exams matter more than the headline index score.

This is a sourced readout as of November 14, 2026. Every table is labeled with its source and date.

## What Google actually announced

Google introduced Gemini 4 Hydrogen on November 12, 2026, as a model for everyday coding and agentic workflows. Gemini 4 Carbon and Gemini 4 Argon remain the frontier models for deep, long-horizon reasoning. The DeepMind model card says Hydrogen “is adapted from Gemini 4 Carbon.” Carbon and Argon default to medium thinking and cap at high. Hydrogen defaults to high and adds xhigh. The model ID is `gemini-4-hydrogen`.

It is rolling out across Google AI Studio, the Gemini API, Gemini for business, the Gemini app, Google Search, Gemini in Sheets for eligible subscriptions, and Google Antigravity for paid Google AI Pro and Ultra subscribers. Free Gemini app users get it through auto model selection. In Antigravity it is now the default for Managed Agents.

Google also launched Orchestrator in Antigravity, which can switch between Carbon and Hydrogen mid-session. On Terminal-Bench 4.0, Google says this setup reached near-Carbon task success rates while, on average, using 24% less quota and finishing 19% faster. That claim is Google’s, not an independent replication. Google says Hydrogen at its high default finishes tasks 2.5 times faster than Gemini 3.8 Flash, more than triples the Terminal-Bench 4.1 score (57% versus 18.2%), and cuts fabricated claims by more than 5x.

## Core specifications

Table: Core specifications. Sources: DeepMind model card, Nov. 12, 2026; Google AI Studio and Cloud developer guides.

| Spec | Value |

| — — | — — |

| Model name | Gemini 4 Hydrogen |

| API model ID | `gemini-4-hydrogen` |

| Lineage | Adapted from Gemini 4 Carbon |

| Status | GA; short-term release with a minimum 45-day retirement notice |

| Knowledge cutoff | March 2026; some domains may be less current |

| Context window | Unified 1,048,576 tokens (input, thinking, and output share one pool) |

| Default synchronous output | Up to 65,536 tokens per hop |

| Long decode | Up to 1,048,576 tokens minus input, via background execution or `continuation_token` on `v1beta1` |

| Thinking levels | `low`, `medium`, `high` (default), `xhigh`; `minimal` is rejected |

| Input modalities | Text, images, audio, video, PDF, Microsoft Office, and OpenDocument files |

| Output modality | Text |

| Tool use | Function calling, structured output, code execution, Search and Maps grounding |

| Caching | Implicit caching on prefixes of 4,096 tokens or more; no explicit `cachedContents` |

| Agentic video understanding | Generally available, off by default |

| Regions (Agent Studio) | Global, `us`, or `eu` |

The unified window matters in production. Gemini 3.8 Flash separated a 1 million token input budget from a 64K output budget. On Hydrogen, a long prompt plus a high thinking level can shrink the room left for the answer, and thinking tokens are billed as output. Longer outputs need `background=true` or a continuation loop, on `v1beta1` rather than `v1`.

## Pricing

Paid-tier Gemini API pricing is flat across the full 1 million token window, with no published long-context surcharge. Free-tier tokens are free of charge, with rate limits. Paid-tier usage is not used to improve Google products; free-tier usage is.

Table: Gemini Developer API pricing for `gemini-4-hydrogen`. Source: Google AI Studio pricing page, retrieved Nov. 14, 2026.

| Feature | Free tier | Paid tier |

| — — | — — | — — |

| Input | Free of charge | $1.50 per 1M tokens |

| Output, including thinking tokens | Free of charge | $7.50 per 1M tokens |

| Context caching | Free of charge | $0.075 per 1M tokens, implicit only |

| Grounding with Google Search | Not available | 5,000 free requests per month, shared across Gemini 3 and newer models, then $14 per 1,000 requests, with a default daily cap of 1,500 requests per project |

| Grounding with Google Maps | Not available | 5,000 free requests per month, shared the same way, then $14 per 1,000 queries |

| Used to improve Google products | Yes | No |

The paid-tier page also states that the Batch API is offered at a 50% cost reduction, matching the Flex/Batch table on Google Cloud at $0.75/$3.75.

Table: Google Cloud Agent Platform token prices for Gemini 4 Hydrogen, USD per 1 million tokens. Non-global endpoints are 10% higher. Cache-write prices effective January 12, 2027: $1.875 standard global, $3.375 Priority global, $0.9375 Flex/Batch global. Source: Google Cloud pricing, retrieved Nov. 14, 2026.

| Service class | Input | Output (response and reasoning) | Cache read |

| — — | — — | — — | — — |

| Standard, global | $1.50 | $7.50 | $0.075 |

| Standard, non-global | $1.65 | $8.25 | $0.0825 |

| Priority, global | $2.70 | $13.50 | $0.135 |

| Priority, non-global | $2.97 | $14.85 | $0.1485 |

| Flex/Batch, global | $0.75 | $3.75 | $0.0375 |

| Flex/Batch, non-global | $0.825 | $4.125 | $0.04125 |

Table: Standard global list prices, Gemini 4 family, USD per 1 million tokens. Source: Google AI Studio and Cloud pricing, retrieved Nov. 14, 2026.

| Model | Input through Nov. 30, 2026 | Output through Nov. 30, 2026 | Input from Dec. 1, 2026 | Output from Dec. 1, 2026 |

| — — | — — | — — | — — | — — |

| Gemini 4 Hydrogen | $1.50 | $7.50 | $1.50 (no published change) | $7.50 (no published change) |

| Gemini 4 Argon | $2.00 | $10.00 | $4.00 | $20.00 |

| Gemini 4 Carbon | $2.00 | $10.00 | $4.00 | $20.00 |

At today’s promotional rates, Hydrogen is already 25% cheaper than Argon or Carbon on a raw token basis. From December 1, 2026, unless Google changes the lists, it will be 62.5% cheaper ($1.50/$7.50 versus $4/$20).

Table: Other published billing and quota notes. Sources: Google Cloud pricing and AI Studio rate-limits page, retrieved Nov. 14, 2026.

| Item | Published figure |

| — — | — — |

| Grounding with your data | $2.50 per 1,000 prompts |

| Computer-use tool calls | Billed as ordinary tokens for the model in use |

| Completions longer than 32K tokens | Retrievable usage can be charged even if the request later fails on a timeout, effective Nov. 13, 2026 |

| Batch enqueued tokens | 3,000,000 for Hydrogen; 5,000,000 for Carbon and Argon |

| Spend-based cap, rolling 10-minute window | $10 at Tier 1; $200 at Tiers 2 and 3 |

| Priority inference rate limits | Default to 0.3 times the standard limit for each model and tier |

| RPM, input TPM, and RPD | Official docs point to live limits in AI Studio; no fixed public Hydrogen table is listed |

## Four thinking levels, and what they cost in practice

Table: Official thinking-level guidance. Source: Google AI Studio “What’s new in Gemini 4 Hydrogen,” retrieved Nov. 14, 2026.

| Thinking level | Token use, per Google | Official “when to use” guidance |

| — — | — — | — — |

| `low` | Fewest tokens, lowest latency | Fast chat and high-throughput processing; still capable on agentic tasks |

| `medium` | Lower token use, faster responses | Interactive coding, live UI prototyping, responsive chat, latency-sensitive tool calls |

| `high` (default) | Balanced intelligence and speed | Recommended default for most complex coding, agentic, tool, and multimodal work |

| `xhigh` | Most tokens, deepest reasoning | Hard multi-step problems where `high` falls short, if you can trade latency and tokens |

Table: Artificial Analysis measurements of Gemini 4 Hydrogen by thinking level, Intelligence Index v5.0.1. Source: Artificial Analysis, retrieved Nov. 14, 2026.

| Metric | low | medium | high (default) | xhigh |

| — — | — — | — — | — — | — — |

| Intelligence Index | 27 | 41 | 51 | 54 |

| Cost per Index task | $0.29 | $0.80 | $1.87 | $3.16 |

| Time per Index task (minutes) | 1.4 | 3.8 | 8.3 | 14.5 |

| Output speed (tokens/s) | 173 | 174 | 171 | 178 |

| Time to first answer token (seconds) | 5.58 | 11.81 | 17.37 | 44.79 |

| AA-Briefcase v1.1 (Elo) | 947 | 1296 | 1497 | 1568 |

| AutomationBench-AA | 60.5% | 72.0% | 74.4% | 76.1% |

| Terminal-Bench 4.0 | 6.6% | 29.3% | 54.0% | 53.5% |

| FrontierCode-AA 2.0 | 9.3% | 31.5% | 36.3% | 39.0% |

| AA-Omniscience v1.1 (index) | 36 | 42 | 44 | 46 |

| GDP.pdf | 28.2% | 32.8% | 30.0% | 27.6% |

| HLE-Diamond | 32.2% | 47.7% | 54.9% | 58.8% |

| CritPt v2, hard subset | 48.0% | 61.0% | 61.5% | 66.0% |

| Terminal-Bench-Science 0.1 | 2.4% | 15.2% | 41.4% | 49.5% |

Output speed is almost flat, around 170 to 178 tokens per second, so extra time at high and xhigh is mostly more thinking tokens, not a slower decoder. Time to first answer token jumps from 17.37 seconds at high to 44.79 seconds at xhigh. The index gain from high to xhigh is only 3 points, while cost per task rises from $1.87 to $3.16, and Terminal-Bench 4.0 does not improve. For most production traffic, high is the rational default.

## Official benchmarks versus models in its class

The model card compares Hydrogen with Claude Sonnet 5.5 and GPT-6.1 Terra, not with Carbon, Argon, GPT-6 Astra, or Claude Opus 5.5. Methodology notes say Hydrogen scores are pass@1 except where noted, run at the highest thinking settings unless stated otherwise, and that Terminal-Bench 4.1 is cheating-adjusted. HLE-Diamond in this table is taken from Artificial Analysis.

Table: Official Gemini 4 Hydrogen results as of November 2026. Source: DeepMind model card and evals methodology page.

| Category | Benchmark | Notes | Gemini 4 Hydrogen | Claude Sonnet 5.5 | GPT-6.1 Terra |

| — — | — — | — — | — — | — — | — — |

| Agentic coding | SWE-sweep | Subtask-weighted reward | 13.5% | 3.1% | 5.8% |

| Agentic coding | Terminal-Bench 4.1 | Cheating-adjusted | 57.0% | 63.9% | Not in this table |

| Knowledge work | Agents’ Last Exam v1.1 | Averaged rubric score, 122 tasks, 5-hour window | 71.6% | 60.4% | 69.8% |

| Knowledge work | AutomationBench-AA | From Artificial Analysis | 76.1% | 71.8% | 60.2% |

| Knowledge work | Vending-Bench 2 | Bank balance, USD, Andon Labs | $14,549 | $11,165 | $9,107 |

| Computer use | OSWorld-2.1 | Offline 82-task subset, max of 3 runs | 77.6% | 83.9% | 74.2% |

| Multimodal | Multimodal Knowledge Agent (MKA) | Climb WSR accuracy | 53.4% | 52.0% | 43.8% |

| Multimodal | Chartography | Without tools, Surge protocol | 66.6% | 61.6% | 63.2% |

| Science and math | Terminal-Bench-Science 0.1 | Average reward, 5 attempts | 51.4% | 59.9% | 58.3% |

| Reasoning | HLE-Diamond | From Artificial Analysis | 58.8% | 53.3% | 47.3% |

Hydrogen leads this trio on SWE-sweep, Agents’ Last Exam v1.1, AutomationBench-AA, Vending-Bench 2, MKA, Chartography, and HLE-Diamond. Sonnet 5.5 leads on Terminal-Bench 4.1 (63.9% versus 57.0%), OSWorld-2.1 (83.9% versus 77.6%), and Terminal-Bench-Science 0.1 (59.9% versus 51.4%). The announcement also cites #5 on Agent Arena, #1 on the Vals Tax Agent Bench at 52.3%, and a tie for #1 on the Vals Legal Research Bench at 55.8%, as of the November 12–13 post.

## How it compares with Carbon and Argon

Google did not publish one official table that lines all three Gemini 4 models up on the same harness for every exam. The table below mixes official Hydrogen, Carbon, and Argon figures with Artificial Analysis Index scores that Google itself quotes. Do not treat “n/p” cells as zeros.

Table: Gemini 4 family, selected published scores. Sources: Google Hydrogen blog and model card, Nov. 12–13, 2026; Google Carbon and Argon announcement tables; Artificial Analysis, checked Nov. 14, 2026. “n/p” means no published number was found for that exact setting.

| Benchmark | Hydrogen | Carbon | Argon |

| — — | — — | — — | — — |

| AA Intelligence Index, Google-cited | 51 high / 54 xhigh | 59 (Google); 60 high on live AA, Nov. 14 | 50 at high |

| Vals Index v2.2 | n/p | 70.4% | 69.2% |

| SWE-sweep | 13.5% | 9.6% | 4.3% |

| FrontierSWE 2 | n/p | 61.2% | 55.0% |

| Terminal-Bench 4.1, cheating-adjusted | 57.0% | n/p | n/p |

| Terminal-Bench 4.0 (AA) | 54.0% high / 53.5% xhigh | 65.2% | 57.1% at high |

| Agents’ Last Exam v1.1 | 71.6% | 77.4% | 69.0% |

| AutomationBench-AA | 76.1% xhigh / 74.4% high | n/p | 77.5% high (AA) |

| AutomationBench (Zapier), Carbon/Argon cards | n/p | 53.9% | 51.3% |

| Vending-Bench 2, bank balance | $14,549 | $16,484 | $13,718 |

| OSWorld-2.1, offline subset | 77.6% | 81.5% | 74.7% |

| MKA, Climb WSR accuracy | 53.4% | n/p | n/p |

| Chartography | 66.6% | 72.5% | 71.6% |

| Blueprint-Bench 2, similarity | n/p | 58.6% | 54.4% |

| GraphWalks, 256k to 1M, BFS, F1 | n/p | 96.2% | 84.2% |

| Terminal-Bench-Science 0.1, Google card | 51.4% | n/p | n/p |

| Terminal-Bench-Science 0.1 (AA) | 41.4% high / 49.5% xhigh | 64.8% | 52.9% at high |

| RiemannBench | n/p | 80.0% | 76.0% |

| HLE-Diamond (AA) | 58.8% xhigh / 54.9% high | n/p in the Hydrogen card | 56.7% high (AA) |

| CWE-bench | n/p | 71.0% | 68.0% |

| ExploitGym | n/p | 54.8% | 42.0% |

| SEC-bench Pro | n/p | 88.2% | 83.4% |

On SWE-sweep, Hydrogen’s 13.5% is ahead of Carbon’s 9.6% and Argon’s 4.3%. On Agents’ Last Exam v1.1, OSWorld-2.1, and Vending-Bench 2, Hydrogen sits between Argon and Carbon. On terminal work, the replacement claim is weaker. Artificial Analysis has Argon (high) at 57.1% on Terminal-Bench 4.0 and Hydrogen at 54.0% high / 53.5% xhigh. On Terminal-Bench-Science 0.1, AA has Argon (high) at 52.9% and Hydrogen at 41.4% high / 49.5% xhigh, still short of Argon at xhigh. Carbon is well ahead on both (65.2% and 64.8%). Chartography favors the frontier models (Carbon 72.5%, Argon 71.6%, Hydrogen 66.6%). Hydrogen at xhigh is 54 on the Index, Argon at high is 50, and Carbon is 59 in Google’s citation (60 on the live AA board as of November 14). If your workload looks like Terminal-Bench, Argon is not obsolete. If it looks like SWE-sweep, knowledge work, or cost-sensitive agent farms, Hydrogen is already the more interesting Gemini 4 model.

## Coding, reasoning, multimodal, and unpublished exams

SWE-sweep is not SWE-bench Verified. As of November 14, 2026, this article found no published Hydrogen score on SWE-bench Verified or classic SWE-bench, or on GPQA or GPQA Diamond, ARC-AGI or ARC-AGI-2, RiemannBench, GraphWalks, or Vals Index v2.2. Do not infer those scores from Carbon or Argon. Carbon scored 96.2% and Argon 84.2% on GraphWalks (256k to 1M, BFS, F1).

The official 58.8% HLE-Diamond figure is the AA xhigh score, not the high-default score (54.9%). Hydrogen only overtakes Argon high (56.7%) on that exam at xhigh. CritPt v2 is a clearer win (61.5% high, 66.0% xhigh, versus 52.5% for Argon high). FrontierCode-AA 2.0 is also a Hydrogen win: 36.3% at high and 39.0% at xhigh, against 25.7% for Argon at high. Contemporaneous coverage of the AA board reported 64% for Sonnet 5.5 max and 56% for GPT-6.1 Sol max on Terminal-Bench 4.0. GDP.pdf peaks at medium thinking (32.8%), then falls at high (30.0%) and xhigh (27.6%).

## Independent leaderboards

Artificial Analysis is the third-party source Google chose to quote. The Index is v5.0.1, built from nine exams: AA-Briefcase v1.1, AutomationBench-AA, Terminal-Bench 4.0, FrontierCode-AA 2.0, AA-Omniscience v1.1, GDP.pdf, HLE-Diamond, CritPt v2 (hard subset), and Terminal-Bench-Science 0.1.

Table: Artificial Analysis Intelligence Index and cost/speed, selected models, retrieved November 14, 2026. Source: https://artificialanalysis.ai/leaderboards/models

| Model and setting | Intelligence Index | Cost per task | Time per task (minutes) | First answer token (seconds) |

| — — | — — | — — | — — | — — |

| Claude Fable 5.2 (max) | 65 | $26.54 | 45.3 | 270.17 |

| Claude Fable 5.2 (high) | 62 | $9.08 | 16.2 | 27.63 |

| Gemini 4 Carbon (high) | 60 | $5.48 | 30.9 | 70.53 |

| Claude Opus 5.5 (max) | 59 | $10.04 | 25.4 | 396.07 |

| Gemini 4 Carbon (medium) | 58 | $4.68 | 27.9 | 22.18 |

| Claude Opus 5.5 (xhigh) | 57 | $6.05 | 15.7 | 88.46 |

| GPT-6 Astra (max) | 57 | $6.24 | 24.1 | 253.50 |

| GPT-6.1 Sol (max) | 55 | $1.27 | 23.3 | 201.02 |

| Claude Sonnet 5.5 (max) | 55 | $10.02 | 33.8 | 272.39 |

| Gemini 4 Hydrogen (xhigh) | 54 | $3.16 | 14.5 | 44.79 |

| Gemini 4 Hydrogen (high) | 51 | $1.87 | 8.3 | 17.37 |

| Gemini 4 Argon (high) | 50 | $4.97 | 29.2 | 44.99 |

| GPT-6.1 Sol (high) | 50 | $0.59 | 7.9 | 43.12 |

| Grok 4.7 (xhigh) | 35 | $7.77 | 26.7 | 63.03 |

Against Argon, Hydrogen at high is already 51 versus 50, at less than 40% of the cost per task ($1.87 versus $4.97) and in 8.3 minutes versus 29.2. Against Carbon, the gap remains: Carbon high is 60 at $5.48 and 30.9 minutes, while Hydrogen xhigh is 54 at $3.16 and 14.5 minutes. Hydrogen xhigh sits just behind Sonnet 5.5 max and GPT-6.1 Sol max, both at 55. Fable 5.2 max still leads at 65, at $26.54 per task. Opus 5.5 max is 59, and GPT-6 Astra max is 57. Sol max is 55 at $1.27 per task, cheaper than Hydrogen xhigh, though much slower to first token (201.02 seconds versus 44.79). Grok 4.7 xhigh, at 35 and $7.77 per task, is not in this race on the AA Index. At xhigh, Hydrogen ranked #78 of 79 models for verbosity, generating 304 million output tokens across the Index run against a median of 77.3 million. On AA-Omniscience v1.1, Hydrogen (xhigh) has an 8% hallucination rate, versus 4% for Carbon (medium) and Argon (high).

LMArena numbers, as reported on the Artificial Analysis Hydrogen page and checked in November 2026, are early.

Table: LMArena scores for Gemini 4 Hydrogen as reported by Artificial Analysis, checked November 2026.

| Arena | Elo | Rank |

| — — | — — | — — |

| Overall | 1490 | 15 |

| Coding | 1518 | 43 |

| Math | 1495 | 21 |

| Creative writing | 1492 | 6 |

| Instruction following | 1503 | 6 |

| Hard prompts | 1514 | 15 |

| Longer query | 1514 | 8 |

| Multi-turn | 1476 | 50 |

| Expert | 1542 | 6 |

The creative-writing, expert, and instruction-following ranks (all 6th) are the surprise, because Google did not pitch Hydrogen as a writing model. The coding rank (43rd) and the multi-turn rank (50th) are the caution. A two-day-old arena score can move.

## Safety, limitations, and early reviews

The full Safety and Security Report is described as “coming soon” on the November 12 model card. The figures in the next table are Google’s internal evaluations. They have not been independently reproduced in the sources reviewed here.

Table: Selected Google blog safety claims for Gemini 4 Hydrogen versus the prior stack. Source: Google blog, November 12–13, 2026.

| Claim | Official figure |

| — — | — — |

| Reduction in agentic overreach | Up to 81% |

| Reduction in under-service | Up to 79% |

| Cut in misaligned actions and task shortcutting (tool-use and coding evals) | 12x |

| Lower chance of bypassing permission boundaries on impossible or blocked tasks | Up to 2x |

| Internal agent honesty evaluation | 94% honesty rate |

| Chain-of-thought legibility | 99.9%, with zero deceptive reasoning edits across more than 200,000 audited turns |

| Gray Swan indirect prompt injection resilience | More than 2.3x |

| HealthBench Pro false refusals | Under 0.4% |

The model card also publishes a comparison with Gemini 3.8 Flash from automated development evaluations, with a warning that the evaluation set was updated, so the figures are not comparable with older Gemini cards.

Table: Automated safety evaluations, Gemini 4 Hydrogen versus Gemini 3.8 Flash. Source: DeepMind model card, November 12, 2026.

| Evaluation | Direction that is better | Change versus 3.8 Flash |

| — — | — — | — — |

| Text-to-text safety | Higher | +0.80 pp |

| Multilingual safety | Higher | +1.10 pp |

| Image-to-text safety | Higher | +7.00 pp |

| Refusal tone (neutral, not preachy) | Higher | -0.65 pp |

| Unjustified refusals | Lower | +5.57 pp |

The card says multimodal content safety improved clearly, text and multilingual safety stayed high within confidence intervals, refusal tone was slightly worse, and unjustified refusals rose, especially on multimodal prompts. Under the Frontier Safety Framework (April 2026), Google concluded Hydrogen did not show a material capability increase versus Argon. Targeted CBRN and cyber tests were still run. The card says Hydrogen does not reach those critical capability levels, but it does reach the alert thresholds in both domains. Known limitations include hallucinations, occasional slowness or timeouts, and higher token use at higher thinking levels. The knowledge cutoff is March 2026. The short-term GA lifecycle, with a 45-day minimum retirement notice, matters if you pin a production agent to this model ID.

## The verdict, as of November 14, 2026

Is Gemini 4 Hydrogen the fast model that finally makes Argon optional? Sometimes, and only for some jobs.

It is a genuine step-change from Gemini 3.8 Flash: much higher Terminal-Bench 4.1, much lower fabricated-claim rate in Google’s accounting, a unified 1 million token window with long decode, and list pricing that stays at $1.50/$7.50 while Argon and Carbon are scheduled to move to $4/$20 on December 1. At its high default, it is slightly ahead of Argon on the Artificial Analysis Intelligence Index and far ahead on cost and task time. On SWE-sweep, it leads the Gemini 4 family in the published tables.

It does not erase Argon on terminal and science-agent work, it does not catch Carbon, and it does not catch Claude Fable 5.2 or, at the top thinking settings, Claude Opus 5.5 or GPT-6 Astra. Classic public exams such as GPQA, ARC-AGI, SWE-bench Verified, and GraphWalks had no published Hydrogen numbers at the time of writing.

The setup that fits the evidence is Hydrogen at high for everyday execution, xhigh only when a hard reasoning or science task needs it, Carbon (or a top-tier external model) for planning and for work that looks like Terminal-Bench-Science or a security audit, and a real eval on your own repo before you delete Argon from the router.

Sources

Google, Introducing Gemini 4 Hydrogen, Nov. 12, 2026, updated Nov. 13, 2026: https://blog.google/innovation-and-ai/models-and-research/gemini-models/introducing-gemini-4-hydrogen/

Google, Gemini 4 Hydrogen for developers, Nov. 12, 2026: https://blog.google/innovation-and-ai/technology/developers-tools/build-with-gemini-4-hydrogen/

Google DeepMind, Gemini 4 Hydrogen model card, Nov. 12, 2026: https://deepmind.google/models/model-cards/gemini-4-hydrogen/

Google DeepMind, Gemini 4 Hydrogen evaluations methodology, Nov. 2026: https://deepmind.google/models/evals-methodology/gemini-4-hydrogen

Google AI Studio, What’s new in Gemini 4 Hydrogen: https://aistudio.google.com/docs/whats-new-gemini-4-hydrogen

Google AI Studio, Gemini Developer API pricing, retrieved Nov. 14, 2026: https://aistudio.google.com/docs/pricing

Google AI Studio, Rate limits, retrieved Nov. 14, 2026: https://aistudio.google.com/docs/rate-limits

Google Cloud, Developer’s guide to Gemini 4 Hydrogen, last updated Nov. 12, 2026 UTC: https://docs.cloud.google.com/gemini-enterprise-agent-platform/models/guides/gemini-4-hydrogen

Google Cloud, Agent Platform pricing, retrieved Nov. 14, 2026: https://cloud.google.com/model-garden/pricing

Artificial Analysis, Gemini 4 Hydrogen model page, Hydrogen versus Argon comparison, models leaderboard, and AA-Omniscience v1.1, all retrieved Nov. 14, 2026: https://artificialanalysis.ai/models/gemini-4-hydrogen , https://artificialanalysis.ai/models/comparisons/gemini-4-hydrogen-vs-gemini-4-argon , https://artificialanalysis.ai/leaderboards/models , https://artificialanalysis.ai/evaluations/omniscience

OpenRouter, Gemini 4 Hydrogen, released Nov. 12, 2026: https://openrouter.ai/google/gemini-4-hydrogen

The Decoder, Nov. 12, 2026: https://the-decoder.com/googles-gemini-4-hydrogen-outscores-the-bigger-argon-just-four-weeks-after-its-launch/

heise online, 2026: https://www.heise.de/en/news/Gemini-4-Hydrogen-Google-s-efficient-alternative-to-Argon-and-Carbon-11523905.html

SmartScope, verified Nov. 13, 2026: https://smartscope.blog/en/blog/gemini-4-hydrogen-vs-argon-vs-carbon-differences-2026/

Follow on X — @amitmuflis
