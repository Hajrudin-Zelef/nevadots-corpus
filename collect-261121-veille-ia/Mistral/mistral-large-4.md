---
id: collect-261121-veille-ia/mistral/mistral-large-4
title: "Mistral Large 4"
domain: mistral
role: reference
task: article
actors: ["Mistral AI"]
dates: ["2025-12-02"]
keywords: ["mistral", "large", "model", "preview", "weights", "october", "simon", "text"]
source: https://www.dsebastien.net/mistral-large-4/
source_anchor: ""
source_lines: [1, 117]
section: "Mistral"
---
*Canonical version:* *Mistral Large 4**.*

Mistral Large 4 (ML4, or "le Chonk" as Mistral AI calls it in its own announcement) is Mistral's new flagship: a 1 trillion parameter, natively multimodal mixture-of-experts model. The public preview opened on 6 October 2026 through the Mistral API and Mistral Studio. The open weights followed at the end of October 2026, with a version that had three more weeks of reinforcement learning than the preview (see **#Open weights release**).

Why should you care? Because the previous flagship, Mistral Large 3, scored 9 on the Artificial Analysis index. ML4 scores 38. That jump is why Simon Willison opened his post with "Mistral are back in the game."

## Key facts

|  | Mistral Large 3 | **Mistral Large 4** | 
|---|---|---|
| Released | 2 December 2025 (GA) | 6 October 2026 (public preview), weights end of October 2026 | 
| Total parameters | 675B | **1.05T** , plus a 1.6B vision encoder | 
| Active per token | 41B | **49B** (52B counting embeddings and output layers) | 
| **Context** | 256k | **1M** | 
| Input / output | text, image / text | text, image / text, reasoning | 
| Training hardware | 3,000 NVIDIA H200 | 3,800 NVIDIA Grace Blackwell, in Mistral's own European datacenters | 
| API price (per M tokens) | $0.50 in / $1.50 out | $0.70 in / $0.07 cached / $2.10 out at release (preview sale price: $0.68 / $0.07 / $2.09) | 
| Weights license | **Apache 2.0** | Mistral Ecosystem Open-Weight License (modified MIT; separate license for model-as-a-service) | 

The API names are `mistral-large-4` and `mistral-large-4-0`. During the preview, reasoning had exactly two levels through the API: `none` and `high`. The released model has `low`, `medium` and `high` (the default).

About that active parameter count: Simon writes 49B, while Mistral's announcement and docs say 52B. The Hugging Face page settles it. 49B parameters are active per token, and 52B once you add the embeddings and output layers.

ML4 is the first model on the roadmap funded by Mistral's €3 billion Series D, and Mistral says it will be the base for a new generation of specialized Mistral models.

## What Mistral claims

These are Mistral's own numbers. Read them as marketing until independent evals catch up:

- **Agentic coding** : 61.7% on DeepSWE v1.1, 59.4% on SWE-Atlas-QnA, 28.3% on Terminal-Bench 4. Its Coding Agent Index of 49.8% puts it ahead of DeepSeek V4 Pro 0813 and Qwen3.8 Max
- **Blind human eval on coding** (Surge AI, 1 to 5 scale): 3.74, second of five models, ahead of Kimi K3 (3.59) and GLM-5.3 (3.60), behind Claude Opus 5 (4.22)
- **Agentic workflows** : 59.9% on AutomationBench, 657 business workflows across Gmail, Google Sheets, Slack and Salesforce
- **Vision** : 42% on Dense 200 against 41% for GPT-6 Astra. Visual grounding is the one area where Mistral claims to beat closed frontier models
- **Cybersecurity** : top five on the Artificial Analysis Cyber Index, 93% on Cybench, and 82% on a reproduce-then-patch test (the best score of any model)
- **Safety** : it resists 93.3% of attacks on Lakera's B3 benchmark for indirect prompt injection, and refuses malicious cyber prompts more often than the other open models Mistral tested
- **Languages** : more than 160 in the training data, including every official EU language

The cyber angle is the interesting one. On that 82% test, Claude Opus 5.5 and GPT-6 Astra score near zero because they refuse the task. Mistral's argument: defenders often have to prove a flaw is real before they can patch it, and a model you run yourself, under your own policies, won't refuse halfway through an incident. Until the weights ship, Mistral is red-teaming ML4 with security firms, vetted partners and state authorities, who get a version with "reduced moderation and expanded cyber capabilities".

## Simon Willison's take

Simon's post is short, and the title does half the work. "Le chonk" is Mistral's own unofficial nickname for the model; he simply kept it. His points:

- **Mistral are back in the game.** ML4 scores 38 on Artificial Analysis, "just behind DeepSeek 4.1 Flash, which is a 552B model". Mistral Large 3 scored 9 and, in his words, "drew this terrible pelican"
- **The pelicans got better.** His usual test (an SVG of a pelican riding a bicycle) looked better at`high` reasoning, which oddly used fewer output tokens (2,717) than`none` (3,275). On Hacker News he added that the setting "didn't seem to make any real difference" beyond a tiny bit of thinking trace, and that it's the best pelican he's seen from any Mistral model
- **Six months behind, and that's good news.** "It's certainly not a Fable-class model, but it's great to see Mistral put out a model that's back to being maybe about 6 months behind the frontier." Fable being Anthropic's Claude Fable 5.1

That last sentence is the fairest one-line summary of ML4. Nobody serious claims ML4 beats the frontier. What changed is that Europe has a model in the race again.

## Open weights release

Mistral kept its promise. At the end of October 2026 it published the weights on Hugging Face (`mistralai/Mistral-Large-4-1T-A52B`, FP8) with the improved version of the model: about three more weeks of reinforcement learning on top of the preview, mostly for coding and cybersecurity. An NVFP4 build for NVIDIA Blackwell GPUs is also available (`mistralai/Mistral-Large-4-1T-A52B-NVFP4`).

What changed against the preview, per Mistral:

- **Terminal-Bench 4** : 41%, up from 28.3%, with 30% fewer output tokens
- **Artificial Analysis Coding Agent Index** : 48%, effectively tied with GLM-5.3 while using 47% fewer tokens per task
- **Cognition FrontierCode 2.0** : 19.7%, the best score in Mistral's comparison. Mistral now calls it the "best open-weight model for coding"
- **Surge human preference on code** : second only to Claude Opus 5.5
- **BrowseComp** : 84.5%.**FinWorkBench** : 73.3% (67.4% in preview), second among open models after Kimi K3
- **Safety** : 97.1% attack resistance on Lakera's B3 (93.3% in preview), and a 96.6% refusal rate on malicious cyber requests, "the highest we measure among open models"

The Hugging Face page also lists 26 on the Artificial Analysis Intelligence Index v5. Don't compare it with the 38 above: different index version.

**The license.** It's not Apache 2.0 like Large 3. The Mistral Ecosystem Open-Weight License is a modified MIT license: you can use, modify, fine-tune and sell products built on the model, and internal use is fine. But a business that runs a model-as-a-service offering needs a separate commercial license from Mistral. On Hacker News, that started the usual debate: several commenters said "open weight" is misleading and "weights available" would be more honest, and compared it to the HashiCorp and Elasticsearch license changes. Mistral itself says "open weight", not open source, and the OSI definition would also require training code and data details.

**Where to run it.** Mistral Studio and API, OpenRouter, Mistral Vibe, NVIDIA endpoints (also a NIM container), and Microsoft Foundry, the first cloud provider to offer it, with private Azure deployments. Mistral's line on sovereignty: "Sovereignty does not always require self-hosting." One HN commenter pointed out a 10% surcharge for EU-only inference.

**Self-hosting is not for your laptop.** Mistral's examples use 8 GPUs with tensor parallelism in vLLM or SGLang, plus optional speculative decoding with a built-in DSpark draft head (see AI Speculative Decoding). On HN, someone with 4× H200 (540 GB) learned that 4-bit weights are about 610 GB, and the smallest Unsloth quant still needs about 256 GB of VRAM. A lighter Large 4 that runs on a laptop was reported in the French press, but Mistral hasn't announced it.

**Reception.** The HN thread passed 1,700 points. The praise: cheap ($0.70 / $2.10, about 20× cheaper than Anthropic's API for one user), "good enough" with a well-built context (skills, AGENTS.md, MCP), and EU hosting for compliance. The criticism: slow when thinking, worse agent reliability than six-month-old US models for some users, and vendor benchmarks nobody trusts. Several commenters said Qwen3.8-Flash-Next or DeepSeek V4.1 Flash did better on their own tasks. One commenter also asked the right question about the 96.6% cyber refusal rate: without the false-refusal rate on benign requests, it says little.

Mistral also previews a cybersecurity variant (penetration testing, vulnerability assessment, red-teaming) for vetted enterprise partners like Orange Cyberdefense, restricted to systems they own or are authorized to test.

## Reception

The Hacker News thread passed 2,000 points, and the top comments fall into two camps:

- **Relief.** People are glad to see Mistral "still in the game even after some big stumbles with Large 3", and hope it becomes "the European go-to". Others liked the vision and cyber numbers enough to call it a possible daily driver
- **Skepticism.** One commenter compared the mood to "people cheering for the last kid crossing the finish line": US labs keep pushing the frontier, Chinese labs keep getting closer than expected, and Mistral moves slower

Both camps care about the Sovereign AI part. ML4 is trained in the EU and served in the EU, and Mistral runs a European deployment end to end, under European law and (in its words) independently of other digital service providers.

## What I take from it

Written at the preview. My view after the weights release is at the end.

- **The sovereignty argument finally has a strong model behind it.** Until now, "use a European model" meant accepting a big quality gap. Six months behind the frontier at $0.68 / $2.09 per million tokens is a trade a lot of European companies will take, especially once they can self-host
- **The weights matter more than the preview.** Yet another API from a European provider is nice. A 1T model you can run on your own infrastructure, for security work that US models refuse, is a different offer. Check the license when it lands: Large 3 was Apache 2.0, and Mistral hasn't said anything about ML4 yet
- **Most of the benchmarks are Mistral's.** The Artificial Analysis score of 38 is the independent number, and it puts ML4 just behind a DeepSeek model half its size
- **Expect the model to move.** Mistral says the reinforcement learning run behind the preview is still going and shows "no signs of saturation". What you test today may not be what ships at the end of the month

After the weights release:

- **The license is the real news.** Large 3 was Apache 2.0. Large 4 is free for almost everyone, except the one business that competes with Mistral: selling inference. I understand why (a 1T model is expensive to train and anyone with GPUs could resell it), but it's a step back, and it makes "open weight" a weaker promise than it was a year ago
- **Sovereignty still means renting, for most people.** You need 8 datacenter GPUs to run it. For a European company, the realistic options are Mistral's own EU-only API (10% more) or a private deployment on Azure. Self-hosting is for those who already own a cluster
- **The coding gains are real, on Mistral's numbers.** Terminal-Bench 4 going from 28% to 41% in three weeks of RL is a big jump. I'd still wait for independent agentic evals before switching any coding work to it

## References

- Simon Willison, "Introducing Mistral Large 4: Le chonk" (6 October 2026): https://simonwillison.net/2026/Oct/6/le-chonk/
- Mistral announcement: https://mistral.ai/news/mistral-large-4/
- Model card (Mistral docs): https://docs.mistral.ai/models/mistral-large-4-0
- Hugging Face model page (weights): https://huggingface.co/mistralai/Mistral-Large-4-1T-A52B
- Artificial Analysis model page: https://artificialanalysis.ai/models/mistral-large-4
- Hacker News discussion: https://news.ycombinator.com/item?id=49977979
- Mistral, "We're releasing the weights of Mistral Large 4": https://mistral.ai/news/mistral-large-4-oss/
- Hugging Face, NVFP4 weights: https://huggingface.co/mistralai/Mistral-Large-4-1T-A52B-NVFP4
- Hacker News discussion of the weights release: https://news.ycombinator.com/item?id=50289418

## Related

## About Sébastien

## Ready to get to the next level?

**Found this valuable?** Share it with someone who needs it.
