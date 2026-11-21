---
id: collect-261121-veille-ia/mistral/mistral-releases-mistral-large-4-open-weights-with-a-license-catch
title: "Mistral releases Mistral Large 4 open weights, with a license catch"
domain: mistral
role: reference
task: article
actors: ["Mistral AI"]
dates: []
keywords: ["mistral", "percent", "model", "large", "license", "weights", "index", "october"]
source: https://www.deai.org/news/mistral-large-4-open-weights
source_anchor: ""
source_lines: [1, 65]
section: "Mistral"
---
Mistral AI released the weights of Mistral Large 4 on October 29, making its 1-trillion-parameter flagship model available for anyone to download and self-host. The number that matters for builders sits in the license: running it as a commercial hosted API requires a separate agreement with Mistral. API pricing starts at $0.70 per million input tokens.

## Key facts

- Mistral Large 4 has about 1 trillion total parameters with 49-52 billion active per token, a 1M-token context window, and natively multimodal text-plus-image input, per the Hugging Face release post.
- The license is a modified MIT that permits download, fine-tuning, local runs, and commercial products, but requires businesses operating a model-as-a-service offering to obtain a separate commercial license from Mistral.
- Mistral claims the top open-weight score on Cognition's FrontierCode 2.0 at 19.7 percent, and 41 percent on Terminal-Bench 4.0, up 13 points from the preview. These are vendor self-reports, not independent grades.
- On the independent Artificial Analysis Intelligence Index, the model scores 26 points, behind DeepSeek V4.1 Flash and GLM-5.3 at 32 each. Hugging Face's own post calls it the fifth-best open-weights model on that index as of October 29.
- Pricing is $0.70 per million input tokens, $0.07 cached, and $2.10 per million output tokens through Mistral's API.
- Mistral claims a 96.6 percent refusal rate on malicious cyber requests while scoring 51 percent on the Artificial Analysis Cyber Index, though its own chart places DeepSeek V4.1 Flash at 57 percent and MiMo-V2.6-Pro at 56 percent above it.

## What happened

Mistral published the weights of Mistral Large 4, nicknamed "Le Chonk," to Hugging Face on October 29, three weeks after the model entered public preview on October 6 and exactly on the schedule the company set when it promised an end-of-October release. Two official quantizations are up: an FP8 checkpoint and an NVFP4 variant, with GGUF builds from Unsloth and the llama.cpp team for smaller setups. Self-hosting the full model is not casual: per SGLang's documentation, the standard version runs on eight Nvidia B300 GPUs, and the compressed variant on four GB300s.

The architecture, per Hugging Face's technical post, is a 72-layer Mixture-of-Experts decoder with 128 experts, of which four are routed per token plus one shared expert that always runs, for five active experts. Attention combines Multi-head Latent Attention with a DeepSeek Sparse Attention indexer that selects up to 2,048 key positions per query. A 1.6-billion-parameter Pixtral encoder handles images.

Between preview and release, Mistral ran three more weeks of reinforcement learning. The company says this lifted Terminal-Bench 4.0 from 28 to 41 percent and put the model at 48 percent on the Artificial Analysis Coding Agent Index, effectively tied with GLM-5.3 while using 47 percent fewer tokens per task. On Cognition's FrontierCode 2.0, a private benchmark that asks whether a repository maintainer would merge the agent's change, Mistral claims the open-weight lead at 19.7 percent against 17.8 percent for GLM-5.3. For comparison, The Decoder reports Claude Opus 5.5 scores 46 percent on the same benchmark. All of these are Mistral-reported numbers pending independent replication.

The security story carries the same framing. Mistral says the model scores 51 percent on the Artificial Analysis Cyber Index, which tests whether a model can find a vulnerability, reproduce it, and patch it without breaking existing functionality, and that closed frontier models often refuse such work outright. The claimed 96.6 percent refusal rate on malicious cyber prompts from JailbreakBench, StrongREJECT, and AgentHarm is Mistral's own measurement. The company also published endorsements from CrowdStrike, Cloudflare, Cisco, Fortinet, Elastic, Booz Allen Hamilton, and the EU's ENISA, each describing evaluations of the model. Those are partner statements, not benchmark results.

Then there is the license. The Mistral Ecosystem Open-Weight License is described by Mistral as a modified MIT: free to use, modify, fine-tune, and commercialize, with one condition. Businesses that operate a model-as-a-service offering must obtain a separate commercial license before using the model. Downloads, local deployments, and products built on top of the model stay open.

## Why it matters

The license line is the story for anyone deciding where to run models. Open weights with a hosted-API restriction are not new; what is new is a frontier-scale model drawing that line explicitly. If you fine-tune Le Chonk for your own product, the license does not touch you. If you were planning to become someone's inference provider for it, you now owe Mistral a negotiation first. That distinction reshapes the reseller layer of the open-weights ecosystem, and it applies to exactly the kind of high-traffic, price-sensitive serving that a 52-billion-active-parameter model at $0.70/$2.10 per million tokens invites.

The independent numbers matter just as much. On Artificial Analysis' Intelligence Index, the model's 26 points trail DeepSeek V4.1 Flash and GLM-5.3 at 32, which places a ceiling on the "best open model" framing. The cyber ranking tells a similar story: Mistral's own chart shows two open models above its 51 percent. DeAI covered the preview release and the Artificial Analysis index gap in recent weeks, and this release narrows no gaps on the general index; it adds a strong coding specialist and a serious defensive-security option to the open pool.

The clock is also ticking on the coding lead. Moonshot AI has promised Kimi K3.1 Pro weights by November 4, and per The Decoder that model already tops Terminal-Bench 4.0 at a claimed 67 percent in pre-release testing. Mistral's FrontierCode lead may last about a week.

## Background

Mistral previewed Large 4 on October 6 with the unusual step of naming a release window, a bet that paid off on schedule. The model was trained from scratch on European infrastructure, which Mistral frames as a sovereignty argument, and the release is aimed at organizations, including governments and cybersecurity vendors, that want weights they can audit and deploy inside their own perimeter. TechCrunch reports CEO Arthur Mensch's framing that AI should "be a tool for empowerment, not dominance," with Mistral claiming frontier-adjacent performance "at a fraction of the compute and capital committed by closed labs." Those are the company's claims, not ours.

The release lands in a market where the open-weights frontier is currently Chinese-led on general benchmarks. Our coverage of DeepSeek's V4.1 Flash update and Moonshot's pending K3.1 Pro release sketches the competitive picture Mistral is entering: strong on coding and security specialization, behind on the broad index, and distinct in Europe as the only lab at this scale with a European training and deployment story. For a primer on what open weights do and do not give you, see what are open-weight models.

## What's next

Moonshot AI's Kimi K3.1 Pro weights are due by November 4, which will re-rank the open-weight coding benchmarks within the week. Mistral says a full technical report for Large 4 is coming, and that the model will serve as the foundation for specialized downstream Mistral models. The license is the document to watch: if Mistral's separate commercial-API agreements surface with published terms, the hosted-serving market for Le Chonk will price itself quickly; if they do not, potential resellers should read the actual license file rather than the announcement before building on it.

Questions

- Can I use Mistral Large 4 weights commercially for free?
- For most uses, yes. Mistral's Ecosystem Open-Weight License, a modified MIT license, allows download, modification, fine-tuning, and building commercial products. The one condition: operating the model as a hosted model-as-a-service offering requires a separate commercial license from Mistral AI.
- How large is Mistral Large 4 and what hardware does it need?
- Mistral Large 4 has about 1 trillion total parameters with roughly 49-52 billion active per token in a Mixture-of-Experts layout, and a 1M-token context window. Per SGLang's documentation, the standard FP8 version runs on eight Nvidia B300 GPUs; a compressed NVFP4 variant runs on four GB300 GPUs.
- Is Mistral Large 4 the best open-weight model?
- It depends on the benchmark. Mistral claims the top open-weight spot on Cognition's FrontierCode 2.0 coding benchmark at 19.7 percent. On Artificial Analysis' Intelligence Index, independent testing scores it 26 points, behind DeepSeek V4.1 Flash and GLM-5.3 at 32, and fifth among open-weights models.

Sources

1. Mistral Large 4 (Le Chonk) is now open weight — Mistral AI
2. Welcome Le Chonk: Mistral Large 4 on Hugging Face — Hugging Face
3. Mistral Ecosystem Open-Weight License (model repo LICENSE) — Mistral AI via Hugging Face
4. Paris-based Mistral releases 'Le Chonk,' a strong coding model that still trails China overall — The Decoder
5. Mistral releases weights for its trillion-parameter AI model, targeting cybersecurity — TechCrunch

About DeAI

DeAI is an independent publication covering open-weight AI models, private inference, and decentralized infrastructure — the tools for running AI you actually control. We test providers on price, privacy, and refusal behavior and publish the numbers, not the vibes. DeAI is powered by Morpheus (mor.org), a decentralized inference marketplace, and covers it on the same terms as every other provider.

Powered by Morpheus and StrandCMS

Morpheus is a decentralized inference marketplace, covered on the same terms as every other provider — we rank it wherever the data lands. StrandCMS is the open-source, agent-first framework this site is built on.
