---
id: collect-261121-veille-ia/mistral/mistral-large-4-vs-mistral-large-3-pricing-reasoning-1m-context-and-retirement-date
title: "Mistral Large 4 vs Mistral Large 3: Pricing, Reasoning, 1M Context and Retirement Date"
domain: mistral
role: reference
task: article
actors: ["Mistral AI"]
dates: ["2025-12-02"]
keywords: ["mistral", "large", "reasoning", "model", "context", "october", "license", "retirement"]
source: https://www.datastudios.org/post/mistral-large-4-vs-mistral-large-3-comparison
source_anchor: ""
source_lines: [1, 585]
section: "Mistral"
---
Mistral Large 4 vs Mistral Large 3: Complete Comparison and Report on Pricing, Reasoning Modes, Context Window, and Migration Deadline

Nov 7

15 min read

Mistral Large 4 and Mistral Large 3 are the two most recent flagship models from Mistral AI, released eleven months apart. Mistral Large 3 (API name mistral-large-2512) arrived on December 2, 2025 as a 675-billion-parameter sparse mixture-of-experts model under the Apache 2.0 license, with a 256k-token context window and no dedicated reasoning mode. Mistral Large 4 (API names mistral-large-4 and mistral-large-4-0, version 26.10) entered public preview on October 6, 2026 and had its open weights published on Hugging Face on October 29, 2026. It is a 1.05-trillion-parameter mixture-of-experts model with a 1M-token context window, three reasoning levels and a new license, the Mistral Ecosystem Open-Weight License.

The choice between them now has a deadline attached. Mistral's model documentation lists Mistral Large 3 as deprecated from October 31, 2026, with retirement from the Mistral API on April 30, 2027, and names Mistral Large 4 as the replacement. On price, Large 4 costs $0.70 per 1M input tokens and $2.10 per 1M output tokens, which is 40% more than Large 3 on both lines ($0.50 and $1.50). Large 4 adds a cached-input rate of $0.07, a context window four times longer and reasoning that cannot be switched off. Large 3 keeps two practical advantages. Artificial Analysis measures its time to first answer token at 2.52 seconds against 33.86 seconds for Large 4 at high reasoning, and its Apache 2.0 license carries no restriction on model-as-a-service businesses.

The direct verdict follows from those facts. Teams starting a new integration on Mistral's API should build on Large 4, because Large 3 has a fixed end date and Large 4 is the only one of the two that handles inputs above 256k tokens or exposes a reasoning control. Teams running latency-sensitive chat, or hosting the weights to sell inference to third parties, have reasons to keep Large 3 in service until the retirement date, or to self-host the Apache 2.0 weights beyond it. Prices in this comparison are as listed on Mistral's official pricing and model pages on November 7, 2026.

··········


MISTRAL LARGE 4 AND MISTRAL LARGE 3 RELEASE TIMELINE AND MODEL IDENTITY.

Mistral Large 3 was announced as part of the Mistral 3 family, alongside the smaller Ministral 3 models. Mistral described it as its first mixture-of-experts model since the Mixtral series, trained from scratch on 3,000 NVIDIA H200 GPUs, and released both a base and an instruction-tuned checkpoint under Apache 2.0. At launch the company reported that the model debuted at number two among open-source non-reasoning models on LMArena and sixth among open-source models overall.

Mistral Large 4, nicknamed "Le Chonk" by Mistral, followed a two-stage release. The October 6 preview made the model available through Mistral Studio and the API, with a launch price discount of 50% for two weeks recorded in Mistral's changelog. The open-weight release later in the month added about three more weeks of reinforcement learning, which Mistral says improved coding, defensive cybersecurity and reasoning efficiency, and replaced the preview's "None" reasoning setting with a "Low" level. Mistral says Large 4 was trained from scratch on 3,800 NVIDIA Grace Blackwell GPUs in its European data centers.

........

Item

Mistral Large 4

Mistral Large 3

API model name

mistral-large-4, mistral-large-4-0

mistral-large-2512

Version

26.10

25.12

First release

October 6, 2026 (public preview)

December 2, 2025

Open weights

October 29, 2026 (Hugging Face)

December 2, 2025

Status on Mistral API

Generally available

Deprecated October 31, 2026

Retirement on Mistral API

None announced

April 30, 2027

License

Mistral Ecosystem Open-Weight License

Apache 2.0

Training hardware (vendor)

3,800 NVIDIA Grace Blackwell GPUs

3,000 NVIDIA H200 GPUs

........

The deprecation entry for Large 3 sits in the "Deprecated and retired models" table of Mistral's model overview, together with Mistral Medium 3.5, which shares the same October 31 and April 30 dates and the same recommended replacement. Mistral's own model page for Large 3 now opens with the line "This model is deprecated. Use Mistral Large 4 for new integrations."

··········

MISTRAL LARGE 4 VS LARGE 3 API PRICING AND RATE STRUCTURE.

Mistral prices both models per million tokens, with input and output billed separately. The current pricing page in Mistral's documentation lists Large 4 at $0.70 input, $0.07 cached input and $2.10 output. Large 3 no longer appears in that list, but Mistral's public pricing page states that "Mistral Large costs $0.5 /M tokens in and $1.5 /M tokens out," the same figures published in the Mistral 3 announcement for mistral-large-2512, and Artificial Analysis lists the same $0.50 and $1.50 for Large 3 on Mistral's API.

The batch rule comes from Mistral's pricing page, which says batch processing "reduces the price by 50%" and that cached input tokens cut input cost by "up to 90%." Both model pages list batching among supported capabilities. The cached rate of $0.07 for Large 4 is exactly 10% of its input price. For Large 3, Artificial Analysis reports a 90% cache discount, but Mistral's current pricing table does not print a cached figure for the model, so this comparison does not compute one.

The headline percentages understate the cost gap for many workloads. Large 4 always produces reasoning before its answer, and Mistral's documentation lists "reasoning" as an output modality, so those tokens add to output volume at $2.10 per million. Large 3 has no reasoning stage. The cost section below shows how much that changes the monthly bill under stated assumptions.

··········

CONTEXT WINDOW, OUTPUT LIMITS AND REASONING CONTROLS IN MISTRAL LARGE 4 AND LARGE 3.

The largest functional change between the two models is input length. Large 4 accepts 1M tokens according to Mistral's model page and its Hugging Face model card, against 256k for Large 3. Neither Mistral model page lists a maximum output length. Amazon Bedrock's model card for Large 3 lists a 32K-token output maximum on that platform, and no equivalent figure has been published for Large 4 on Mistral's API.

........

Specification

Mistral Large 4

Mistral Large 3

Context window

1M tokens

256k tokens

Max output tokens

Not published

Not published by Mistral (32K on Amazon Bedrock)

Input modalities

Text, image

Text, image

Output

Text, reasoning

Text

Reasoning levels

low, medium, high (default high)

None (non-reasoning model)

Reasoning off switch

Not available ("none" runs as high)

Not applicable

Recommended sampling

Temperature 1.0, top_p 0.95

Temperature below 0.1 for production

Images per request

100 (Artificial Analysis, preview)

8 (Artificial Analysis, previous Mistral models)

........

Large 4's model card describes the three levels as high for complex or agentic tasks, medium for everyday use and low for quick chat, with high as the default. The Hugging Face launch blog adds that reasoning cannot be disabled and that passing "none" still runs the model at high. That makes the effort setting the main cost and latency control on Large 4, and Artificial Analysis data show what it buys. Large 4 at high scores 26 on its Intelligence Index v5.0.1 at a cost of $2.06 per index task, while Large 4 at medium scores 23 at $1.41 per task.

Large 3 works differently. Its model card says it is not a dedicated reasoning model and that dedicated reasoning models can outperform it on strict reasoning tasks. Mistral recommends a temperature below 0.1 for production use of Large 3, while the Large 4 card recommends 1.0 with top_p 0.95 at every reasoning level, so prompts and sampling settings tuned for Large 3 need to be revisited during a migration.

The image figures come from Artificial Analysis's October 6 article on the Large 4 preview, which reported that Mistral's API accepted 100 images per request for Large 4, up from 8 for previous Mistral models. Mistral has not repeated those limits on its own model pages.

··········

MISTRAL LARGE 4 AND LARGE 3 ACCESS BY PLATFORM AND DEPLOYMENT OPTION.

Both models can be used through Mistral's API, downloaded as open weights, or reached through third-party clouds, but the list of partners differs. Mistral launched Large 3 on a wide set of platforms on day one. For Large 4, Mistral names Microsoft Foundry as the first cloud provider and NVIDIA-hosted endpoints, with other partners still to be announced.

........

Access route

Mistral Large 4

Mistral Large 3

Mistral API and Mistral Studio

Yes, GA

Yes, deprecated, retires April 30, 2027

Hugging Face weights

Yes (FP8 and NVFP4)

Yes (FP8, BF16, NVFP4)

Amazon Bedrock

Not announced by Mistral

Yes (mistral.mistral-large-3-675b-instruct)

Microsoft Foundry

Yes (first cloud named by Mistral)

Yes (since December 2025)

NVIDIA endpoints and NIM

Yes

NIM listed as "coming soon" at launch

OpenRouter

Yes

Yes

Vibe Code (chat.mistral.ai/code)

Yes

Not stated

Fine-tuning service

Mistral Forge

Not stated

........

On Amazon Bedrock, Large 3 is listed as Active, with a launch date of December 2, 2025, an end-of-life date "no sooner than" December 2, 2026 and a legacy period of at least six months. It is available in-Region in 13 AWS Regions, including us-east-1, us-west-2 and eu-west-2, with Standard, Priority and Flex service tiers. Mistral's deprecation applies to its own API, so Bedrock and other providers follow their own lifecycle schedules.

Mistral's consumer product, renamed from Le Chat to Vibe on August 12, 2026, does not state on its help pages which model powers each mode. The only explicit consumer surface Mistral names for Large 4 is Vibe Code.

··········

LICENSE TERMS: APACHE 2.0 FOR LARGE 3, A MODIFIED MIT LICENSE FOR LARGE 4.

The license change affects anyone who self-hosts. Large 3 is released under Apache 2.0, a standard permissive license with no restriction on how the model is offered. Large 4 uses the Mistral Ecosystem Open-Weight License, which Mistral describes as a modified MIT license that allows use, modification, fine-tuning and commercialization, on the condition that businesses operating a model-as-a-service offering must obtain a separate commercial license from Mistral before using the model commercially. Mistral's announcement mentions no revenue or user thresholds for that condition.

Both are open-weight releases, which means the trained weights are downloadable. Neither release includes the training data or the full training code, so neither is open source in the full sense of the term.

For a company using either model inside its own products, both licenses allow commercial use. For an inference provider that sells API access to the model, Large 3 can be served without asking Mistral, while Large 4 requires a commercial agreement. After April 30, 2027, the Apache 2.0 weights of Large 3 also remain the only way to keep running that model, since the Mistral API will no longer serve it.

··········

MISTRAL LARGE 4 VS LARGE 3 ARCHITECTURE AND SELF-HOSTING REQUIREMENTS.

Both models use what Mistral calls a granular mixture-of-experts design, but Large 4 is larger and adds new attention machinery for long inputs. Mistral's figures differ slightly between pages for both models, so the table records each source.

........

Architecture item

Mistral Large 4

Mistral Large 3

Total parameters

1.05T (Mistral docs), about 1T (model card)

675B (Mistral docs), 673B language model (model card)

Active parameters

52B (docs), 49B per token excluding embeddings and output layers (model card)

41B (docs), 39B language model (model card)

Vision encoder

1.6B parameters

2.5B parameters

Experts

128 routed, 4 active, plus 1 shared

Not published in text

Layers

72

Not published in text

Attention

Multi-head Latent Attention with a sparse attention indexer (up to 2,048 keys per query)

Not published in text

Official checkpoints

FP8, NVFP4

FP8, BF16, NVFP4

Example deployment

8x H200 with FP8 (Hugging Face recipe); NVFP4 on Blackwell

FP8 on one node of 8x H200 or B200; NVFP4 on one node of H100 or A100

........

The deployment examples point to a real hardware difference. Mistral's Large 3 card says the NVFP4 checkpoint runs on a single node of H100 or A100 GPUs, which many companies already own. The Large 4 NVFP4 checkpoint targets NVIDIA Blackwell GPUs, and the published FP8 recipes use an 8x H200 node. The Hugging Face blog's vLLM example for Large 4 also caps the served context at 131,072 tokens, well below the 1M-token maximum, which suggests that running the full context window requires more memory than a single standard node provides. Mistral has not published official memory requirements for Large 4.

··········

[[ADPLUS_300x250_1]]

BENCHMARK METHODOLOGY AND WHY FEW LARGE 4 VS LARGE 3 SCORES ARE DIRECTLY COMPARABLE.

Mistral has not published a benchmark table placing Large 4 next to Large 3. The Large 4 announcements compare it with other open-weight and closed models (GLM-5.3, Kimi K3, DeepSeek V4.1 Flash, Claude Opus 5.5, GPT-6 Astra), and the Large 3 announcement presented its scores mostly as chart images without numbers in the text. The Large 3 model card states one figure in text, a GPQA Diamond score of 67.17, and Mistral has published no GPQA Diamond score for Large 4.

The benchmarks themselves have also changed. Many of the evaluations Mistral cites for Large 4, such as Terminal-Bench 4.0, FrontierCode 2.0 and AutomationBench, did not exist or were not used when Large 3 was released. Artificial Analysis now runs version 5.0.1 of its Intelligence Index, and its Large 3 page shows no score on that version, so the index cannot be used to rank the two models against each other.

The comparison that follows therefore separates three kinds of evidence: measurements Artificial Analysis took on both models with the same method; scores Mistral published for Large 4 only; and statements without figures.

··········

INDEPENDENT MEASUREMENTS OF MISTRAL LARGE 4 AND LARGE 3 BY ARTIFICIAL ANALYSIS.

Artificial Analysis is the only named evaluation organization whose current pages cover both models. Its figures for speed and latency were measured on Mistral's own API.

........

Measurement (Artificial Analysis)

Mistral Large 4 (high)

Mistral Large 3

Output speed

97 tokens per second

76 tokens per second

Time to first answer token

33.86 s

2.52 s

Intelligence Index v5.0.1

26 (rank 11 of 26 on the page)

No score on v5.0.1

Intelligence Index v5.0.1, medium reasoning

23

Not applicable

Cost per Intelligence Index task

$2.06 (high), $1.41 (medium)

Not published

Output tokens to run the index

107M (median of comparable open-weight models 99.3M)

Not published

GDP.pdf document and image reasoning

19% (preview, October 6 article)

18 points lower, per the same article

........

The speed figures show two sides of the same design. Once Large 4 starts writing, it generates tokens about 28% faster than Large 3 (97 against 76 tokens per second). The time to first answer token, however, is more than 13 times longer, because Large 4 spends its first seconds on reasoning tokens before the visible answer begins. For a chat interface or a voice assistant, 33.86 seconds before the first word of the answer is a long wait, and the low reasoning level is the setting Mistral recommends for quick chat. Artificial Analysis has not published a latency figure for Large 4 at low.

The GDP.pdf result is the one quality measurement in which Artificial Analysis states a direct gap. In its October 6 article on the preview, it reported a 19% score for Large 4 and called it an 18-point improvement over Large 3. That measurement predates the final open-weight checkpoint. The same article reported preview figures of 38 on the Intelligence Index and different list prices, which no longer match Artificial Analysis's current model page or Mistral's pricing page, so this report uses the current pages for the index and prices.

··········

MISTRAL LARGE 4 BENCHMARKS WITH NO LARGE 3 EQUIVALENT.

The scores below come from Mistral's announcements and the Large 4 model card, all for the final open-weight model unless marked as preview. None has a published Large 3 counterpart. Settings were not disclosed for most of them beyond the default high reasoning level.

........

Benchmark (source)

Mistral Large 4 score

Notes

Terminal-Bench 4.0 (Mistral, self-reported)

41%

28% in the preview, with 30% fewer output tokens in the final model

Artificial Analysis Coding Agent Index (cited by Mistral)

48%

Tied with GLM-5.3, with 47% fewer tokens per task

FrontierCode 2.0, Cognition (private)

19.7%

Highest on Mistral's chart

BrowseComp

84.5%

Kimi K3 at 91.2% on the same chart

FinWorkBench (Finch)

73.3%

67.4% in the preview

Artificial Analysis Cyber Index (cited by Mistral)

Mistral also reported that, in Surge's blind human evaluation of code, testers preferred Large 4 over GLM-5.3, Kimi K3 and DeepSeek V4.1 Flash, with only Claude Opus 5.5 ranked higher among the models evaluated. The Large 4 model card adds a CWE-bench resolve rate of 46.3 and a Terminal-Bench Science score of 4.3.

These numbers describe where Large 4 sits among current frontier and open-weight models. They do not measure how much better it is than Large 3, because Mistral did not run, or did not publish, the same tests on its previous flagship.

··········

OUTPUT TOKENS AND MONTHLY COST OF MISTRAL LARGE 4 VS LARGE 3 ON THREE WORKLOADS.

The cost examples below use Mistral's list prices from November 7, 2026 and were computed in Python. Because Mistral does not publish how many reasoning tokens Large 4 produces per request, the first two workloads show Large 4 at several reasoning-token levels instead of a single estimate.

Workload A is a customer-support assistant handling 10,000 requests a day for 30 days, with 3,000 input tokens and 400 answer tokens per request. Workload B is a batch job summarizing 2,000 documents of 200,000 tokens each into 1,000-token summaries, billed at the 50% batch rate. Workload C is a single request over a 600,000-token contract archive or codebase with a 4,000-token answer, which exceeds Large 3's 256k context window.

........

Workload and assumption

Mistral Large 4

Mistral Large 3

A: no reasoning tokens counted

$882.00 a month

$630.00 a month

A: 500 reasoning tokens per request

$1,197.00 a month

Not applicable

A: 1,500 reasoning tokens per request

$1,827.00 a month

Not applicable

A: 2,000 of 3,000 input tokens cached, 500 reasoning tokens

$819.00 a month

No official cached price

B: batch, no reasoning tokens counted

$142.10

$101.50

B: batch, 2,000 reasoning tokens per document

$146.30

Not applicable

C: one 600,000-token request, 4,000-token answer

$0.43

Not possible (256k limit)

........

The arithmetic shows where the 40% list-price gap grows or shrinks. In the support workload, output is a large share of the bill, so every 500 reasoning tokens per request adds $315 a month to Large 4, and at 1,500 reasoning tokens the monthly cost is 2.9 times Large 3's. Caching moves the result the other way: if two-thirds of each prompt is a shared system prompt and knowledge-base prefix billed at $0.07, Large 4 with 500 reasoning tokens costs $819, which is 30% more than Large 3 at full input price. In the long-document batch job, input dominates the cost, so reasoning tokens barely move the total, and Large 4 costs about 40% to 44% more than Large 3. The third workload has no Large 3 price because the model cannot accept the input in one request.

··········


SPEED, LATENCY AND WHEN THE FASTER FIRST TOKEN OF LARGE 3 COUNTS.

The latency gap is the most practical reason to keep Large 3 running until its retirement. Artificial Analysis records 2.52 seconds to the first answer token for Large 3 on Mistral's API, which suits interactive chat, autocomplete and voice, and 33.86 seconds for Large 4 at its default high reasoning. Large 4's 97 tokens per second output speed means long answers finish faster once they start, and Artificial Analysis records an average of 14.1 minutes per Intelligence Index task for Large 4 at high, a figure that reflects multi-step agentic tasks.

Developers moving latency-sensitive features to Large 4 have one lever, the reasoning level. Mistral's guidance pairs low with quick chat, and the preview's removal of the "None" option means a sub-three-second first answer on Large 4 has not been demonstrated in any published measurement. Testing low against Large 3 on real prompts before the April 30, 2027 retirement is the only way to know whether the newer model meets an existing latency budget.

··········

SAFETY RESULTS, CYBER RESTRICTIONS AND DATA HANDLING FOR MISTRAL LARGE 4 AND LARGE 3.

Mistral has put cybersecurity at the center of the Large 4 release, in both directions. The model scores 51% on the Artificial Analysis Cyber Index, according to Mistral, and Mistral reports a 96.6% average refusal rate on malicious cyber requests and 97.1% resistance on the Lakera B3 agent security benchmark. A specialized cybersecurity variant is in preview for vetted enterprise partners such as Orange Cyberdefense under contractual safeguards, and Mistral says it is still adjusting guardrails based on live traffic. Mistral published no comparable safety evaluations for Large 3 in its announcement or model card.

On data handling, Mistral's regulatory documentation for Large 4 says user inputs and outputs from its products (Vibe and Mistral AI Studio) can be used for training, with the option to opt out. The Large 4 preview announcement also lists a European deployment that Mistral operates end to end under European law, alongside other regions. Large 3's open Apache 2.0 weights, like Large 4's, allow fully private deployment where no data reaches Mistral.

··········

WHAT MISTRAL HAS NOT PUBLISHED ABOUT LARGE 4 VS LARGE 3.

Several figures that buyers would need for a complete comparison are missing from Mistral's pages. There is no benchmark table with both models, no maximum output length for either model on Mistral's API, no official cached-input price for Large 3, no published rate limits for Large 4, no reasoning-token averages per effort level, and no official memory requirement for self-hosting Large 4 at its full 1M-token context. Artificial Analysis has no Intelligence Index v5.0.1 score for Large 3 and no latency figure for Large 4 at low or medium reasoning. Mistral's help pages do not state which model powers the Vibe assistant.

The active-parameter count also appears in two forms: 52B in Mistral's documentation and 49B per token in the model card, which reaches 52B once embeddings and output layers are included.

··········

MISTRAL LARGE 4 OR LARGE 3: WHICH TO CHOOSE BY WORKLOAD.

The deprecation turns the decision into a question of timing for most API users, and of licensing and hardware for those who self-host.

........

Workload or situation

Recommended model

Reason

New integration on Mistral's API

Mistral Large 4

Large 3 retires from the API on April 30, 2027

Inputs above 256k tokens

Mistral Large 4

1M-token context; Large 3 stops at 256k

Agentic coding and multi-step tasks

Mistral Large 4

Reasoning levels and published agentic scores; none published for Large 3

Real-time chat or voice with strict latency

Mistral Large 3 until retirement, then test Large 4 at low

2.52 s against 33.86 s to first answer token at Large 4's default

High-volume support with short answers

Mistral Large 3 for cost, Large 4 with caching

Large 4 costs 40% more per token before reasoning tokens

Long-document batch summarization under 256k

Either; Large 4 costs about 40% more

Input dominates cost; reasoning adds little

Reselling inference to third parties

Mistral Large 3 (Apache 2.0)

Large 4 requires a commercial license for model-as-a-service

Self-hosting on H100 or A100 hardware

Mistral Large 3

Its NVFP4 checkpoint runs on one H100 or A100 node; Large 4's targets Blackwell

Use on Amazon Bedrock today

Mistral Large 3

Listed as Active on Bedrock; Large 4 not announced there

The facts support a split plan. API users should move new and long-context workloads to Large 4 now, budget for the 40% higher token price plus reasoning output, and use caching to recover part of the gap, while keeping latency-critical chat on Large 3 until they have measured Large 4 at low reasoning against it. Self-hosters with Hopper or Ampere hardware, and anyone who sells inference to others, have a durable reason to stay on the Apache 2.0 Large 3 weights after April 30, 2027.
