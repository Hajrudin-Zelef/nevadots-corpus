---
id: collect-261121-veille-ia/kimi/the-uschina-open-weight-race-muse-spark-open-beam-kimi-k3-and-glm-5-3
title: "The US–China Open-Weight Race: Muse Spark Open, Beam, Kimi K3 and GLM-5.3"
domain: kimi
role: reference
task: article
actors: ["Moonshot AI"]
dates: ["2026-10-15"]
keywords: ["muse", "open-weight", "open", "kimi", "glm-5", "spark", "model", "beam"]
source: https://www.faradaymachines.com/us-china-open-weight-race/
source_anchor: ""
source_lines: [1, 193]
section: "Kimi"
---
# The US–China Open-Weight Race: Muse Spark Open, Beam, Kimi K3 and GLM-5.3

Through 2026, the top open-weight models have mostly come from Chinese labs. In October, Meta released Muse Spark Open and Reflection AI shipped Beam under Apache 2.0, putting two US labs back in the race. Here is how they compare with Kimi K3, GLM-5.3, DeepSeek V4.1 Flash, MiMo-V2.6 and MiniMax’s M3 family, what each needs to run, and what a model’s country of origin does and doesn’t change for a Canadian buyer.

## Key takeaways

- Meta released the weights of Muse Spark Open on October 15, 2026: an 816B-parameter model with 42B active that Meta calls “the most capable US open-weight model to date”, priced at $1.25 per million input tokens and $4.25 per million output tokens on OpenRouter.
- On Meta’s own runs, Muse Spark Open leads the MCP-Atlas tool-use benchmark at 89.2, ahead of GLM-5.3 (84.2) and Kimi K3 (82.3), but trails both on Terminal-Bench 2.1 coding, at 76.4 against Kimi K3’s 85.0 and GLM-5.3’s 83.9.
- On the independent Artificial Analysis Intelligence Index v4.3, the top open-weight scores still belong to Chinese labs: MiMo-V2.6-Pro at 46, GLM-5.3 at 45 and Kimi K3 at 44. Muse Spark Open has no confirmed score yet.
- Apple measured Muse Spark Open at 24.99 tokens per second on two linked M5 Ultra Mac Studios and 36.73 on four, one request at a time at an 8,192-token context; the 512 GB Mac Studio the two-unit setup needs isn’t priced yet.
- A model’s origin decides whose law covers the API provider, not what happens on your own hardware: self-hosted weights remove the provider-jurisdiction question for any model, while its license and usage policy still apply.

## The US Is Back in the Open-Weight Race

Through the summer, the open-weight leaderboard was a Chinese story. Moonshot’s Kimi K3 arrived in July as the largest open-weight model ever released, Z.ai published GLM-5.3’s weights in August, and Xiaomi’s MiMo-V2.6-Pro, released in September, now holds the top open-weight score on Artificial Analysis’s index.

October brought two US replies. Reflection AI announced Beam 1.0 on October 5, a 501B-parameter model with 23B active under Apache 2.0, with weights reported on October 16. Meta followed on October 15 with Muse Spark Open, an 816B model with 42B active, a 1-million-token context and text, image, video and audio input. It runs on SGLang, vLLM and Apple’s MLX, and is available through Amazon SageMaker AI, Google Cloud Model Garden, Microsoft Foundry and seven API providers, including OpenRouter.

Neither release ends the Chinese lead outright. Reflection itself says Kimi K3 stays ahead on raw capability, and Meta’s own benchmark table shows Muse winning on tool use and losing on coding. What changed is that a buyer who has been told to avoid Chinese-built models now has a large, current open model from a US lab to consider.

## Ten Open-Weight Models Side by Side

Memory is what the weights need at 4-bit quantization, before working memory for long prompts. A mixture-of-experts model must hold all its parameters in memory, even though it computes with only the active ones for each token.

| Model | Origin | Developer | Total / active params | Context | License | API price (in / out per 1M) | AA Index v4.3 | Memory to self-host | 
|---|---|---|---|---|---|---|---|---|
| Muse Spark Open | US | Meta | 816B / 42B | 1M | Custom (Muse Model License) | $1.25 / $4.25 (OpenRouter) | — | ~530 GB weights (NVFP4); Apple runs it on 2 × 512 GB or 4 × 256 GB Mac Studios | 
| Beam 1.0 | US | Reflection AI | 501B / 23B | — | Apache 2.0 (vendor-stated) | — | — | ~275 GB (our estimate) | 
| Kimi K3 | China | Moonshot AI | 2.8T / 104B | 1M | Custom (Kimi K3 License) | $3.00 / $15.00 (Moonshot) | 44 (max) | ~1.5 TB | 
| GLM-5.3 | China | Z.ai | ~753B / ~40B | 1M | Custom (glm-5.3) | $1.40 / $4.40 (Z.ai list) | 45 | ~410 GB | 
| GLM-5.3-Flash | China | Z.ai | 320B / 18B | 1M | MIT | — | 42 | ~180 GB | 
| DeepSeek V4.1 Flash | China | DeepSeek | 552B / 16B (decode) | 1M | MIT | $0.30 / $1.20 peak (DeepSeek) | — | ~400–445 GB | 
| MiMo-V2.6-Pro | China | Xiaomi | 1.02T / 42B | 1M | MIT | — | 46 | ~560 GB | 
| MiMo-V2.6-Flash | China | Xiaomi | 309B / 15B | — | MIT | — | 38 | ~170 GB | 
| MiniMax M3 | China | MiniMax | ~428B / ~23B | Up to 1M (512K guaranteed) | Custom (minimax-community) | $0.60 / $2.40 (MiniMax) | 29 | ~240 GB | 
| MiniMax M3.1 Flash | China | MiniMax | 428B / ~23B | 1M | Custom MiniMax Community License; weights since October 23 | $0.15 / $0.60 (MiniMax) | 42 at launch (now listed 43) | ~240 GB | 

As of October 19, 2026 (the MiniMax M3.1 Flash row updated October 23), from vendor announcements, model cards, Artificial Analysis and OpenRouter. “—” means no confirmed figure. AA Index is Artificial Analysis Intelligence Index v4.3.x, not comparable with v4.1; an unconfirmed report of 39 for Muse Spark Open is omitted, and MiniMax M3.1 Flash’s 42 is a first listing. M3.1 Flash’s parameter count is per MarkTechPost. Memory is roughly 0.55 bytes per parameter at 4-bit (Beam’s figure is our calculation). DeepSeek V4.1 Flash is the exception: its mostly FP8 weights and a separate Engram memory of about 196B parameters put its 4-bit builds at about 400–445 GB, against about 510 GB as released. Muse Spark Open’s model card gives a floor of about 700 GB of GPU memory for NVFP4 and 2 TB for BF16 on data-center GPUs. GLM-5.3’s price is aggregator-reported; DeepSeek’s off-peak rate is $0.15 / $0.60.

## Where Each Model Leads

Meta’s launch benchmarks put Muse Spark Open, Kimi K3 and GLM-5.3 on the same tests. These are Meta’s runs, not independent ones. Moonshot’s own Terminal-Bench 2.1 score for Kimi K3 is higher than Meta measured, and Reflection has published its own numbers for Beam.

| Model | MCP-Atlas, tool use (Meta’s run) | Terminal-Bench 2.1, coding (Meta’s run) | Terminal-Bench 2.1 (developer-reported) | 
|---|---|---|---|
| Muse Spark Open | 89.2 | 76.4 | 76.4 (Meta) | 
| GLM-5.3 | 84.2 | 83.9 | — | 
| Kimi K3 | 82.3 | 85.0 | 88.3 (Moonshot) | 
| Beam 1.0 | — | — | 80.1 (Reflection) | 

All figures are vendor-reported. Meta did not include Beam in its runs; Reflection’s numbers come from its own launch, which also reports 80.9 on SWE-bench Verified. MiniMax reports 66.0 on Terminal-Bench 2.1 for MiniMax M3. Scores from different harnesses aren’t strictly comparable.

Read together with the independent index, the field splits by job rather than by country:

1. **Muse Spark Open** — Tool use. 89.2 on MCP-Atlas in Meta’s runs, five points ahead of GLM-5.3, with text, image, video and audio input.
2. **Kimi K3** — Coding. 85.0 on Terminal-Bench 2.1 in Meta’s run and 88.3 per Moonshot, plus first place on LMArena’s Frontend Code Arena (1,679) at launch.
3. **GLM-5.3** — Balance. Second on both of Meta’s benchmarks and 45 on Artificial Analysis v4.3.
4. **MiMo-V2.6-Pro** — Independent score. The top open-weight model on Artificial Analysis v4.3.2 at 46, at about $0.13 per index task, under MIT.
5. **DeepSeek V4.1 Flash** — Value. Vals.ai ranks it the top open-weight model on its Vals Index (57.86%, 15th of 56 overall), and its API costs $0.30 / $1.20 at peak.
6. **MiniMax M3.1 Flash** — Price. $0.15 / $0.60 per million tokens and about $0.22 per index task at a launch score of 42 (now listed at 43); its weights shipped on October 23 under MiniMax’s custom community license.
7. **Beam 1.0** — License. Apache 2.0, the only US model here under a standard permissive license; Reflection reports 80.1 on Terminal-Bench 2.1 with 23B active parameters.

The honest summary: the US entries are competitive, not dominant. Muse Spark Open wins on tool use in Meta’s own runs, and Chinese labs hold the top three confirmed open-weight scores on the one index that tests every model the same way.

## Muse Spark Open on Two Mac Studios

Meta calls a Mac Studio running Muse Spark Open the “best way to run a powerful model on your own desktop.” Apple’s MLX research measured decode speed, the rate at which the model writes its answer, on linked M5 Ultra Mac Studios at an 8,192-token context, one request at a time.

The 512 GB Mac Studio is due in late October and has no price yet; the $30,000–$40,000 figure for a pair is a press estimate, not an Apple price. The four-unit route uses the 256 GB model, which already costs over $10,000, so four of them start above $40,000.

At about 25 tokens per second, a 1,000-token answer takes roughly 40 seconds to write, and a 10,000-token output takes close to seven minutes. That is workable for one person. Apple’s figures don’t cover several simultaneous users, the time to read a long prompt, or contexts far beyond 8,192 tokens.

On token cost alone, the numbers are sobering. Running flat out around the clock at 24.99 tokens per second, the pair would write about 790 million tokens a year, which costs about $3,350 at OpenRouter’s $4.25 output rate, before input tokens. The case for two Mac Studios is control over where data goes, not cheaper tokens.

This is editorial analysis, not a Faraday offering. Faraday Machines installs open-weight models on Mac Studio hardware in Toronto, one machine per plan; its largest standard plan, Scale, is a single M5 Ultra with 512 GB. At 4-bit, MiniMax M3 (~240 GB) fits on Scale, a tighter fit. DeepSeek V4.1 Flash fits on Scale as a 1- to 3-bit build of about 250–350 GB, which trades some quality, most noticeably on long or precise work; at 4-bit (about 400–445 GB) it needs a larger setup. Muse Spark Open’s two- or four-machine M5 Ultra setup is larger than any single Faraday plan.

## What Origin Changes for a Canadian Buyer

Model origin and API host are separate questions. An open-weight model can be served by anyone who downloads the weights: Muse Spark Open, for example, is already on seven API providers and three cloud platforms. The jurisdiction that matters is the one over whoever processes your prompts.

| API from a US provider compared with API from a Chinese lab and self-hosted weights |  |  |  | 
|---|---|---|---|
| Consideration | API from a US provider | API from a Chinese lab | Self-hosted weights, any origin | 
|---|---|---|---|
| Who processes prompts | The provider, on its servers | The lab, on its servers | You, on hardware you control | 
| Jurisdiction over data | US law applies to the provider | Chinese law applies to the lab | No outside provider in the loop | 
| License and usage policy | Provider terms plus the model license | The lab’s API terms plus the model license | The model license still applies in full | 
| Pricing | Per token, set by the provider | Per token, set by the lab | Hardware and power, no per-token charge | 
| Model choice | Whatever the provider serves | The lab’s own models | Limited by memory: ~170 GB to ~1.5 TB at 4-bit for the models above | 

For a Canadian business, a US-hosted API raises the same CLOUD Act question as any US cloud service, covered in our Canadian AI sovereignty guide. A Chinese lab’s own API puts your prompts under Chinese jurisdiction instead. Running the weights on your own hardware removes the provider from the picture, whether the model came from Meta, Moonshot or Xiaomi.

What self-hosting doesn’t remove is the license. Muse Spark Open’s usage policy bans, among other things, operating critical infrastructure and the unauthorized practice of regulated professions such as finance, law and medicine, and those terms travel with the weights. Your own customers or procurement rules may also care where a model was trained. That is a policy question, and the answer differs by buyer.

## Who Is Using Chinese Open Models

The best public usage data comes from OpenRouter, an API marketplace, so it covers one aggregator’s traffic, not direct API use or self-hosting. The figures also use different definitions, so they can’t be strung into one trend line.

| Figure | What it measures | Period | Source | 
|---|---|---|---|
| About one-third | Open-weight models’ share of all OpenRouter token volume | 13 months to November 2025 | OpenRouter and a16z, “State of AI” (December 4, 2025) | 
| ~1.2% to peaks near 30%; average ~13% | Chinese open-weight models’ share of weekly OpenRouter tokens | Late 2024 through 2025 | OpenRouter and a16z, “State of AI” (December 4, 2025) | 
| 15–20% programming; over 50% roleplay and creative | What open models were used for, from a classifier run on a 0.25% opt-in sample | 13 months to November 2025 | OpenRouter and a16z, “State of AI” (December 4, 2025) | 
| About 60%, mostly Chinese models | Open-weight models’ share of OpenRouter’s US token consumption | August 2026 | The New Stack | 

The 2025 study covered more than 100 trillion tokens of all OpenRouter traffic. Its 13% figure is a weekly share for Chinese open models alone, averaged over the study period; the August 2026 figure is one month’s share for all open-weight models, limited to US traffic. Both point the same way, but the jump from 13% to 60% compares different things. OpenRouter’s COO described the pattern to The New Stack:

                            Chinese open models are “disproportionately heavy in agentic flows run by U.S. firms.”
                            — OpenRouter’s COO, quoted by The New Stack (2026)
                        

That is the market Muse Spark Open and Beam are entering: US companies already use Chinese open models heavily for agent work, which is where Muse Spark Open’s tool-use lead would matter most, if it holds up in independent tests.

## License Terms Differ More Than Origin

License terms don’t follow the flag. Among the US releases, Beam is Apache 2.0 while Muse Spark Open uses a custom license that requires products to display “Built with Muse Spark Open”. Among the Chinese ones, DeepSeek V4.1 Flash, MiMo-V2.6 and GLM-5.3-Flash are MIT, while Kimi K3, GLM-5.3 and MiniMax M3 use custom licenses. Most custom conditions target very large model-as-a-service providers and leave internal use by ordinary businesses free. Some reach smaller companies too: Muse Spark Open’s attribution rule and usage policy apply at any size, and MiniMax M3’s license requires prior written authorization from any organization above $20 million in yearly revenue. Our open-weight AI license tracker sets out the terms model by model. It isn’t legal advice; check the license text before you deploy.

## Frequently Asked Questions

## ### What is Muse Spark Open?


                                Muse Spark Open is Meta’s open-weight AI model, released on October 15, 2026. It is a mixture-of-experts model with 816 billion total and 42 billion active parameters, a 1-million-token context and text, image, video and audio input. The weights are on Hugging Face under Meta’s custom Muse Model License, and OpenRouter lists it at $1.25 per million input tokens and $4.25 per million output tokens.

## ### Is Muse Spark Open better than Kimi K3?


                                It depends on the job: on Meta’s own benchmark runs, Muse Spark Open leads on tool use and Kimi K3 leads on coding. Meta measured Muse at 89.2 on MCP-Atlas against Kimi K3’s 82.3, but at 76.4 on Terminal-Bench 2.1 against Kimi K3’s 85.0; Moonshot reports 88.3 for Kimi K3. These are vendor-run numbers, and Artificial Analysis hasn’t confirmed a score for Muse Spark Open.

## ### Can Muse Spark Open run on a Mac Studio?


                                Yes, on two or four linked M5 Ultra Mac Studios, not on one. Apple measured 24.99 tokens per second on two units with 512 GB each and 36.73 on four 256 GB units, one request at a time at an 8,192-token context. The 512 GB model isn’t priced yet; press estimates put a pair at $30,000 to $40,000.

## ### Does it matter whether an open-weight model comes from China or the US?


                                It matters most when you use the model through an API, because the provider’s home jurisdiction then covers your prompts. If you run the weights on hardware you control, no outside provider handles your data, whatever the model’s origin. The license still applies: Meta, Moonshot, Z.ai and MiniMax use custom licenses for their flagships, while DeepSeek V4.1 Flash and MiMo-V2.6 are MIT and Reflection’s Beam is Apache 2.0.

## ### How much do US companies use Chinese open-weight models?


                                Heavily, at least on OpenRouter: The New Stack reports that open-weight models were about 60% of OpenRouter’s US token consumption in August 2026, most of it from Chinese models. That covers only traffic through OpenRouter. An earlier OpenRouter and a16z study of over 100 trillion tokens found Chinese open-weight models averaged about 13% of weekly share in the 13 months to November 2025.

## References

**[1]** Meta. (2026). Muse Spark Open announcement. October 15, 2026. Available at: research.meta.ai

**[2]** Meta. (2026). Muse-Spark-Open-816B-A42B model card. Available at: huggingface.co

**[3]** TechCrunch. (2026). “Meta launches an open-weight model that runs on Macs.” October 15, 2026. Available at: techcrunch.com

**[4]** VentureBeat. (2026). Meta unveils Muse Spark Open, the new top US open-weights model. Available at: venturebeat.com

**[5]** The New Stack. (2026). Meta Muse Spark Open. Available at: thenewstack.io

**[6]** Apple Machine Learning Research. (2026). MLX distributed inference measurements on M5 Ultra Mac Studio. Available at: machinelearning.apple.com

**[7]** MacRumors. (2026). Mac Studio with M5 Ultra and 512GB of memory due in October. August 25, 2026. Available at: macrumors.com

**[8]** Reflection AI. (2026). Introducing Beam. Available at: reflection.ai

**[9]** TechCrunch. (2026). Reflection debuts Beam, an open-weight AI model to rival Chinese models at lower compute cost. October 5, 2026. Available at: techcrunch.com

**[10]** Muse Model License analysis: trendingtopics.eu; beri.net

**[11]** Kimi K3: VentureBeat; Artificial Analysis; Tom’s Hardware

**[12]** GLM-5.3 and GLM-5.3-Flash: Digital Applied; Artificial Analysis (GLM-5.3); Artificial Analysis (GLM-5.3-Flash)

**[13]** DeepSeek V4.1 Flash: VentureBeat; Vals Index via Flowtivity

**[14]** MiMo-V2.6: DeepLearning.AI, The Batch; Artificial Analysis (MiMo-V2.6-Flash)

**[15]** MiniMax M3: MiniMax; license summary at minimax-ai.chat

**[16]** MiniMax M3.1 Flash: MiniMax; MarkTechPost; Artificial Analysis

**[17]** Artificial Analysis. (2026). “Artificial Analysis Intelligence Index v4.3.” September 7, 2026. Available at: artificialanalysis.ai

**[18]** OpenRouter and a16z. (2025). “State of AI.” December 4, 2025. Available at: openrouter.ai

**[19]** The New Stack. (2026). OpenRouter US region routing and open-weight usage. Available at: thenewstack.io

**[20]** modelfit.io. (2026). “DeepSeek V4.1 Flash Mac memory requirements.” Available at: modelfit.io; DeepSeek model repository: huggingface.co/deepseek-ai/DeepSeek-V4.1-Flash

**[21]** Unsloth. (2026). “DeepSeek-V4.1-Flash-1017-GGUF.” Available at: huggingface.co

## Talk to Us About Running Open Models Yourself

Faraday Machines installs open-weight models on Mac Studio hardware in Toronto, so your prompts stay on equipment you control. If you’re weighing which open model fits your work and your memory budget, we’re happy to talk it through.

Talk to Us
