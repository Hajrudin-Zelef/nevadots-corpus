---
id: collect-261121-veille-ia/mistral/mistral-large-4-preview-pricing-benchmarks-open-weights
title: "Mistral Large 4 Preview: Pricing, Benchmarks, Open Weights"
domain: mistral
role: reference
task: article
actors: ["Mistral AI"]
dates: ["2026-10-06"]
keywords: ["mistral", "model", "preview", "independent", "october", "open", "weights", "price"]
source: https://dev.to/akaranjkar08/mistral-large-4-preview-pricing-benchmarks-open-weights-jb9
source_anchor: ""
source_lines: [1, 98]
section: "Mistral"
---
**Mistral Large 4 lists at $1.36 per million input tokens, its own docs page shows $0.68, and the independent Terminal-Bench 4 score is 22.73%.** The model entered public preview on 6 October 2026 with open weights promised by the end of October (per aggregated coverage from Connic, Kingy and BenchLM, which this post treats as verify-at-use). It is a mixture-of-experts model with about 1.05 trillion total and 49 to 52 billion active parameters, nicknamed "Le Chonk" in some coverage.

Short answer: Mistral Large 4 is a credible mid-priced agent model, not a price leader. At either price it costs 5.8 to 11.5 times more than Claude Haiku 5.5 on a standard run, and less than GPT-6.1 Sol or Gemini 3.5 Flash. The case for it is the open weights, if they arrive with a usable licence, not the preview API. Treat the vendor benchmark numbers as marketing until a second source reproduces them.

Public preview API access started on 6 October 2026. Mistral promises open weights by the end of October but has not named the licence, and the licence decides whether you can host the model commercially, so the promise carries less information than it sounds. Total size is about 1.05 trillion parameters with 49 to 52 billion active per token, which means roughly 5% of the network runs for any given token. That ratio is why a trillion-parameter model can be priced like a mid-size one.

Context length is reported two ways. Mistral's model card says 1 million tokens. Artificial Analysis lists the preview at 524,000. Until the documentation and the independent listing agree, plan for the lower number if you send long prompts.

Two sets of prices are live, and the difference is exactly a factor of two.

| Source | Input per million | Output per million | Cached input | 
|---|---|---|---|

| List price in coverage | $1.36 | $4.18 | $0.14 |

| Prices shown in current docs | **$0.68** | **$2.09** | **$0.07** |

| Batch API | 50% off the applicable rate |

One outlet says the docs price reflects a 50% launch discount that lasts two weeks. Mistral has not confirmed that. If it is true and runs from the 6 October preview, it would end around 20 October, and a budget built on $0.68 would double overnight. The safe approach is to budget on the list price and treat the docs price as upside. Re-read the live pricing page on the day you sign off, because this is exactly the type of number that moves.

The benchmarks come from three kinds of source, and they should not be averaged together.

| Source type | Benchmark | Result | 
|---|---|---|

| Vendor-reported | AutomationBench | 59.9% |

| Vendor-reported | Cybench | 93.0% |

| Independent, Vals | Finance Agent v2 | **54.68% at $1.20 per test** |

| Independent, Vals | Terminal-Bench 4 | **22.73% at $7.90 per test** |

| Independent, Artificial Analysis | Intelligence Index | **38, at $1.13 cost per task** |

| Aggregator, BenchLM | Overall | 53.7 out of 100, rank 69 of 214, from only 3 benchmark rows |

Vendor numbers are real measurements on benchmarks the vendor chose. A 93.0% on Cybench is a security-capture-the-flag result, and it is the vendor's own run. The independent numbers are lower and more varied: 54.68% on a finance agent test and 22.73% on a hard terminal-task suite. Neither is bad for a model priced like this. Neither is the frontier.

The BenchLM rank deserves a warning. Rank 69 of 214 rests on three benchmark rows, and an aggregate computed on three rows moves a lot when one more row arrives. Do not quote it as a ranking.

Cost per test is where the independent data becomes useful. Divide the cost by the success rate and you get cost per solved task, assuming the test cost covers every attempt:


```
Finance Agent v2:  $1.20 / 0.5468 = $2.19 per solved task
Terminal-Bench 4:  $7.90 / 0.2273 = $34.75 per solved task
```
The gap between those two numbers is the lesson. On finance-style agent tasks the model is cheap per success. On long terminal tasks it burns eight dollars per attempt and fails three attempts in four, so each success costs $34.75. A price per million tokens tells you nothing about that. Task difficulty drives the real bill, and the Agent Run Cost Simulator models exactly this by letting you set a step count and a retry rate alongside the token price.

I priced a standard agent run of 60,000 input tokens and 8,000 output tokens with no caching, then multiplied by 1,000 runs. The run is deliberately under Haiku 5.5's 100,000-token tier boundary. It is a plausible shape, not a measured workload, so replace it with yours.

| Model | Input / output per million | Per run | Per 1,000 runs | 
|---|---|---|---|

| Claude Haiku 5.5 (7 Oct 2026, up to 100K prompt) | $0.10 / $0.50 | $0.010 | **$10.00** |

| Mistral Large 4, docs price | $0.68 / $2.09 | $0.0575 | $57.52 |

| Mistral Large 4, list price | $1.36 / $4.18 | $0.1150 | $115.04 |

| Gemini 3.5 Flash (19 May 2026) | $1.50 / $9.00 | $0.162 | $162.00 |

| GPT-6.1 Sol | $2.00 / $10.00 | $0.200 | $200.00 |

Haiku 5.5 is the headline. Anthropic released it on 7 October with an average 75% price cut against Haiku 4.5, and at $10 per 1,000 runs it undercuts even the discounted Mistral price by 5.8 times. Above 100,000 tokens of prompt Haiku jumps to $0.50 and $2.50, so long-context workloads narrow that gap. A newer Gemini 3.8 Flash is reported at $0.75 and $3.75 as an introductory price through 31 December 2026, which would put the same run at $75, but sources disagree on those numbers, so treat them as unconfirmed.

Price is not quality. Haiku 5.5 is a lightweight model, and I have not seen an independent score that puts it near Mistral Large 4 on hard agent tasks. If your pipeline fails on Haiku, the right comparison is cost per solved task, as above, not the table. For a side-by-side that also handles cached input, use the AI Model Cost Calculator.

One more caution on the cost numbers: the independent per-test costs include whatever prompt and tool setup Vals and Artificial Analysis used, which will not match your harness. Use them to compare shapes, such as finance tasks being cheap per success and terminal tasks being expensive, and not as a forecast of your invoice.

The open-weights promise matters for one group: teams that must host the model themselves. Do the weights arithmetic before getting excited. About 1.05 trillion parameters stored at 8 bits per weight is around 1.05 terabytes, and at 4 bits it is about 525 gigabytes, before any key-value cache or serving overhead. That is a multi-node deployment, not a workstation job, even though only 49 to 52 billion parameters are active per token. All parameters must still sit in memory.

So the practical decision tree is short:

- **Need the cheapest capable model today:** Haiku 5.5, then test whether it passes your tasks.
- **Need a mid-priced API model with a non-US vendor:** Mistral Large 4 at the list price, with a re-check on 20 October.
- **Need self-hosting or sovereignty:** wait for the weights and the licence text, and price the GPUs before you commit.
- **Running agents in production either way:** log token counts per step first, because routing between a cheap and a mid-priced model saves more than choosing one.

Routing is where the money is. Send the easy steps to the cheapest model and the hard ones to the stronger model, and cap retries. Whether more agents help at all is a separate question, which we covered in the multi-agent tax. The AI Agent Ops Bundle includes a routing spec and per-step cost logging, and the Agent Prompt Vault gives you 50 production prompts that run identically across models, so a model swap tests the model and not your wording.

Whatever you pick, test before you trust a headline. Take 30 real tasks from your own logs, run each model on all of them with the same prompt, and record pass or fail with a script, not by eye. Thirty tasks will not give you a precise rate, but they separate a model that passes 80% from one that passes 30%, and that difference swamps every price in the tables above. Log tokens in, tokens out and the number of retries for every run, because a model that passes more often but needs three times the output tokens can still lose on cost per solved task. Run the set twice, once on the docs price and once on the list price, so you can see how a change in the discount alters your budget.

The licence deserves the same scrutiny as the price. Open weights can mean a permissive licence that allows commercial use, or a research licence with a revenue threshold, or something in between. Mistral has not named the licence yet, so any plan that assumes commercial self-hosting is a plan built on a missing document. Write down the three questions you need answered when the text appears: whether commercial use is allowed, whether there is a user or revenue cap, and whether fine-tuned derivatives must carry the same terms.

List price coverage shows $1.36 per million input tokens and $4.18 output, with cached input at $0.14. The current docs show $0.68, $2.09 and $0.07. Batch is 50% off.

One outlet reports a 50% launch discount for two weeks from the preview. Mistral has not confirmed it. Budget on the list price.

Mistral promised open weights by the end of October 2026 but has not named the licence. Check the licence text before you plan a commercial deployment.

No. On a 60,000 input and 8,000 output run, Haiku 5.5 costs $0.010 against $0.0575 at the docs price and $0.115 at the list price for Mistral Large 4.

*Originally published at wowhow.cloud*

## Top comments (0)
