---
id: collect-261121-veille-ia/mistral/mistral-releases-mistral-large-4-le-chonk
title: "Mistral releases Mistral Large 4 ('le Chonk')"
domain: mistral
role: reference
task: article
actors: ["Mistral AI"]
dates: ["2026-10-06"]
keywords: ["mistral", "large", "model", "days", "docs", "preview", "weights", "october"]
source: https://postcutoff.com/e/2026-10-06-mistral-large-4/
source_anchor: ""
source_lines: [1, 115]
section: "Mistral"
---
The takeaway

On Oct 6, 2026 Mistral AI released Mistral Large 4, a granular mixture-of-experts model with ~1.05T total and 49B active parameters, a 1M-token context window and vision, in public preview on Mistral Studio; open weights are promised by the end of October 2026.

## Status

- Claim
- Confirmed
- Our reporting
- High confidence
- Importance
- Major (4 of 5)
- Last verified
- 29 October 2026

## Your AI and this story

- GPT-6.2 Astra128 days after its cutoff
- Claude Opus 5.598 days after its cutoff
- Gemini 4 Hydrogen189 days after its cutoff
- Grok 4.7128 days after its cutoff

None of these four assistants can know about it. The closest, Claude Opus 5.5, stops 98 days before it.

## Key facts

- Architecture: granular MoE, 49B active / 1.05T total parameters, 1.6B vision encoder (docs)
- Context window: 1M tokens (docs); API id mistral-large-4-0 / mistral-large-4
- Trained on 3,800 NVIDIA Grace Blackwell GPUs in Mistralâs European datacenters (blog; Techmeme/The Deep View said ~4,000)
- Availability: public preview via Mistral Studio from Oct 6; weights to be released by end of October 2026
- Pricing differs between sources: docs list $0.68 input / $0.07 cached / $2.09 output per 1M tokens; the blog lists $1.36 / $4.18 (docs figure may be a preview price - unverified)
- Mistral-reported benchmarks: Cybench 93%, DeepSWE v1.1 61.7%, Terminal Bench 4.0 28.3%, AutomationBench 59.9%, Dense 200 42% (vs GPT-6-Astra 41%); top 5 on the AA Cyber Index
- Hacker News: the blog thread reached ~1,666 points and ~947 comments by Oct 7 (627 points for the docs page on launch day). Commenters noted a 50% launch discount to compete with DeepSeek V4.1 Flash, a likely source of the two price figures (unconfirmed)
- Reach: Mistralâs launch post on X had ~4.04M views and 37k likes by Oct 7; NVIDIA AIâs post on the training run (~4,000 Grace Blackwell superchips) ~250k views

## Show 11 more

- Guillaume Lample (X thread, ~197k and ~487k views on Oct 7; the second tweet, on the 3,800-GPU BruyÃ¨res-le-ChÃ¢tel cluster, had ~1.29M views by Oct 8; NVIDIA AI Infraâs training-run post ~286k): the preview is a checkpoint of an RL run âstill in flightâ that âshows no sign of saturationâ; a final version will ship with the weights before the end of October; trained on 3,800 Grace Blackwell GPUs in Mistralâs BruyÃ¨res-le-ChÃ¢tel cluster, funded by its Series B, with Series C and D clusters âcoming online soonâ
- Arthur Mensch: âTrained and served on our own compute, and RL shows no sign of saturationâ; Mistral researcher Albert Q. Jiang said âan even chonkier boi is munching on trillions of tokensâ, meaning a larger model is in training
- Launch post: âWorking with cybersecurity partners privatelyâ and deployable from Europe via Mistral Cloud; Hugging Face placeholder repo mistralai/Mistral-Large-4.0-1T05-A52B (linked by Julien Chaumond)
- Early tests: Simon Willison found only reasoning ânoneâ or âhighâ settings and called its pelican SVG the best from any Mistral model; Arena added it to Agent Arena and Code Arena (scores pending). HN users asked whether it was OpenRouterâs stealth model âspace-bunny-alphaâ (unconfirmed)
- Artificial Analysis (Oct 6â7): Intelligence Index 38, âcomparable to GPT-6 Luna (max, 38) and DeepSeek V4.1 Flash (max, 39)â, âthe most intelligent model from outside the US and Chinaâ; Cyber Index 50 (level with GLM-5.3-Flash); GDP.pdf 19% (+18 pts vs Mistral Large 3); AA lists a 512k context in its test setup; $1.36/$4.18 per 1M standard, 50% off ($0.68/$2.09) for the first two weeks, which resolves the pricing discrepancy; ~$1.13 per task at standard price
- VentureBeat: custom Mistral license; open weights dated Oct 27; 160+ languages; other Mistral benchmarks Harvey Legal Agent 15%, FinWorkBench 67%, DIOR-RSVG 73%; Arthur Mensch joked about the âle chaton fatâ meme (âItâs actually le gros chatonâ)
- Hugging Face âUpcoming releaseâ page for mistralai/Mistral-Large-4.0-1T05-A52B (linked by HF co-founder and CTO Julien Chaumond; read 7 Oct): âExpected release October 31, 2026â (VentureBeat said Oct 27); â49 billion active parameters per token (52 billion including embeddings and output layers)â, which explains âA52Bâ in the name; 579 people waiting
- Same week setback: Berlin-based search engine Ecosia (used by Germanyâs federal environment ministry and the UK NHS) told POLITICO (CEO Christian Kroll interview, published Oct 6â8) it is dropping Mistral, its AI supplier only since May 2026 (when it left OpenAI), for open-weight models including Qwen, GLM and Kimi served by the German platform Melious on EU servers. Kroll: Mistral is âa year behindâ, âWe were simply too large a customer for Mistralâ, and the switch âroughly cut our costs in half while improving qualityâ (ppc.land, Cybernews, Trending Topics). Lample, asked at an Oct 5 press conference, said: âI would encourage them to test our new model as soon as possibleâ (per ppc.land)
- Tooling: Simon Willisonâs llm-mistral 0.16 (Oct 6) adds support for Mistral reasoning models such as Large 4
- Weights released Oct 29, 2026 (see Mistral releases the weights of an improved Mistral Large 4 (âLe Chonkâ), a 1T-parameter MoE, under a modified MIT licence): a further-trained checkpoint (Terminal-Bench 4.0 41% vs 28% in preview, Mistral-reported) as mistralai/Mistral-Large-4-1T-A52B (FP8, NVFP4) under the Mistral Ecosystem Open-Weight licence (modified MIT; model-as-a-service firms need a commercial licence); API price cut to $0.70/$2.10 per 1M tokens
- MistralâNvidia industrial-engineering collaboration (Oct 21, 2026): Mistral says Large 4 was trained on 3,800 NVIDIA Grace Blackwell GPUs in Mistral-operated European data centers, repeats that weights are planned for the end of October, and announces public physics-model benchmarks, open datasets and agentic CAE/EDA benchmarks (no names yet) plus a frontier base model with the Nemotron Coalition; Mistral targets 1 GW of compute by 2030

## What happened

Mistral AI launched Mistral Large 4, nicknamed âle Chonkâ, on October 6, 2026. It is a granular mixture-of-experts model with about 1.05 trillion total and 49 billion active parameters, a 1.6B vision encoder and a 1M-token context window. It launched as a public preview on Mistral Studio, and Mistral promises open weights by the end of October. Mistral pitches it for general agentic work and highlights its cybersecurity scores (Cybench 93%, top 5 on the Artificial Analysis Cyber Index). All benchmark numbers are self-reported.

The license for the weights and the final price are not yet confirmed: the docs and the blog list different prices.

Reactions on X (views at 11:31 UTC on 7 Oct): CEO Arthur Mensch wrote âTrained and served on our own compute, and RL shows no sign of saturationâ (~111k). Quoting Lampleâs tweet about the BruyÃ¨res-le-ChÃ¢tel cluster, which shows a photo of the Breuillet â BruyÃ¨res-le-ChÃ¢tel railway station, he added âWeâre building AGI from this train stationâ (~130k). Peter Gostev (Arena) contrasted â4,000 GPUsâ with Astraâs â100,000â (~162k). Both figures come from earlier claims, not new disclosures: NVIDIA said ânearly 4,000â and Mistral 3,800, while Astraâs âmore than 100,000 GPUsâ is in OpenAI releases GPT-6 Astra, its first GPT-6 model. Memes also travelled: synthwavedd joked that the one benchmark ML4 leads is âabout regulationâ (~156k), and stupidtechtakes posted âMistral using AI ðâ (~53k).

## Why it matters

It is the first trillion-parameter open-weight model from a European lab, and it is trained on European compute. It competes directly with Chinese open-weight frontier models.

## Sources

21 sources from 13 sites. Numbers match the chips in the text.

21 sources: 8 primary, 6 press, 7 reactions

### Primary

1. Mistral: Mistral and NVIDIA bring AI to industrial engineering (Oct 21, 2026)mistral.ai, official
2. Simon Willison: llm-mistral 0.16simonwillison.net, code
3. Artificial Analysis: Mistral Large 4 puts France back on top outside the US and Chinaartificialanalysis.ai, docs
4. Mistral AI: Mistral Large 4 âLe Chonkâmistral.ai, official
5. Mistral docs: Mistral Large 4 model carddocs.mistral.ai, docs
6. Mistral AI on X: Meet Mistral Large 4, aka Le Chonk (~4.0M views)x.com, official
7. Guillaume Lample on X: ML4 launch threadx.com, official
8. Hugging Face: Mistral-Large-4.0-1T05-A52B (upcoming release page)huggingface.co, docs

### Press

1. POLITICO Europe: Ecosia drops Mistral for open-source models, including Chinese onespolitico.eu, press
2. PPC Land: Ecosia drops Mistral AI for open models, citing a 1-year quality gapppc.land, press
3. Ars Technica: Mistral says âLe Chonkâ can challenge the best AI modelsarstechnica.com, press
4. VentureBeat: Mistral debuts Large 4 âLe Chonkâ, a 1-trillion-parameter model with open weights plannedventurebeat.com, press
5. The Deep View: Why Mistralâs 1T model is a hedge against lock-inthedeepview.com, press
6. TechCrunch: Mistralâs new 1T model aims to leapfrog closed and open rivalstechcrunch.com, press

### Reactions

1. Simon Willison: Mistral Large 4 (HN thread notes)simonwillison.net, discussion
2. Hacker News discussion (~1,666 pts)news.ycombinator.com, discussion
3. Simon Willison: Introducing Mistral Large 4: Le chonksimonwillison.net, discussion
4. Arthur Mensch on X: âTrained and served on our own compute, and RL shows no sign of saturationâ (~111k views)x.com, discussion
5. Arthur Mensch on X: âWeâre building AGI from this train stationâ (~130k views)x.com, discussion
6. Peter Gostev on X: 4,000 GPUs vs Astraâs 100,000 (~162k views)x.com, discussion
7. Julien Chaumond (Hugging Face) on X: link to the weights pagex.com, discussion

### Also cited in the text

- synthwaveddx.com
- stupidtechtakesx.com

## Changes

- Weights released; pointer to the new entry Mistral releases the weights of an improved Mistral Large 4 (âLe Chonkâ), a 1T-parameter MoE, under a modified MIT licence
- Added the Oct 21 MistralâNvidia post (3,800 GB-series GPUs, weights end of October, CAE/EDA benchmarks)
- Added Ecosia dropping Mistral for open Chinese models (POLITICO via ppc.land), Ars Technica and llm-mistral 0.16 links
- Added X posts (Mensch x2, Peter Gostev, Julien Chaumond, meme reach) and the Hugging Face upcoming-release page (ETA Oct 31, 52B active incl. embeddings)
- Linked the video âMeet Mistral Large 4, aka Le Chonkâ
- Added Artificial Analysis scores (Index 38, Cyber 50) and pricing clarification, VentureBeat details, The Deep View and Simon Willison links

## Show all 8 changes

- Added X reach and first-hand posts (Mistral, Lample, Mensch, Jiang), HN thread details and early tests
- Filed
