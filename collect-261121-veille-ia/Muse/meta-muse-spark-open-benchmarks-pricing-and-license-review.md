---
id: collect-261121-veille-ia/muse/meta-muse-spark-open-benchmarks-pricing-and-license-review
title: "Meta Muse Spark Open: Benchmarks, Pricing and License Review"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: ["2026-10-15"]
keywords: ["model", "meta", "muse", "spark", "open", "hugging", "face", "license"]
source: https://heshamsaad.com/en/meta-muse-spark-open/
source_anchor: ""
source_lines: [1, 154]
section: "Muse"
---
Meta Muse Spark Open is the open-weight release of Muse Spark, Meta’s model for coding and agent work, published on October 15, 2026. You can download the weights from Hugging Face, run them on your own servers, fine-tune them, or call the model through hosted providers such as OpenRouter, Together AI and Groq. Meta calls it the most capable US open-weight model to date. That is a strong claim, so this article looks at what the model card actually shows, where Muse Spark Open leads, where Chinese open models still beat it, and what it costs to use.

Every number below comes from Meta’s model card on Hugging Face, Meta’s announcement on its AI research blog, the Meta Model API documentation, or OpenRouter’s public price list on launch day. All benchmark figures are reported by Meta and have not yet been independently reproduced.

## What Meta released

Muse Spark Open is a large mixture-of-experts model. That design splits the network into many smaller “expert” blocks and only switches on a few of them for each token, which keeps the running cost far below what the full size suggests.

- **Size:** 816 billion parameters in total, with about 42 billion active for each token.
- **Experts:** 256 experts in each mixture-of-experts layer, with 8 chosen per token.
- **Context window:** 1,048,576 tokens, so roughly one million.
- **Inputs and output:** it reads text, images, audio and video, and it writes text only.
- **Languages:** trained on data from more than 100 languages.
- **Reasoning effort:** low, medium, high or xhigh. The default is xhigh, and Meta recommends high or xhigh for coding and agent tasks.
- **Formats:** BF16 full precision and a smaller NVFP4 version.
- **License:** the new Muse Model License, covered in detail below.

The model card also lists a vision component Meta calls a Perception Encoder, with 1.8 billion parameters, which is how the model reads screenshots, charts, documents and video frames.

## Where you can run Muse Spark Open

Meta split availability into two routes.

### Hosted APIs

The model is available through Baseten, Fireworks AI, Groq, Modal, Nebius, OpenRouter and Together AI. Hugging Face also offers one-command deployment to its Inference Endpoints, which expose an OpenAI-compatible API. In practice that means existing code built for the OpenAI Chat Completions format can switch to Muse Spark Open by changing the base URL and model name.

### Your own hardware

The weights can be downloaded from Hugging Face, Amazon SageMaker AI, Google Cloud Model Garden and Microsoft Foundry. Supported inference engines include SGLang, vLLM, TensorRT-LLM, Hugging Face Transformers, llama.cpp, Unsloth and Apple MLX.

The hardware bar is high. According to the model card, you need GPUs with at least 700 GB of combined memory for the NVFP4 version and about 2 TB for BF16. Meta’s headline for home use is that the model runs on two Mac Studios with the M5 Ultra chip using MLX, because each machine can give up to 512 GB of unified memory to its GPU. That is still a serious purchase, but it is the first time a model in this class has been shown running on desktop machines.

Access to the weights on Hugging Face is gated. You fill in a form with your full legal name, date of birth and organization, and Meta approves requests manually.

## Muse Spark Open benchmarks and comparison with rivals

Meta compares the model with other large open-weight models: Inkling, Nemotron 3 Ultra, Beam, Mistral Large 4, Qwen3.8-2.4T-A95B, GLM 5.3 and Kimi K3. The table below keeps the four most useful columns. A dash means Meta did not report a score.

| Benchmark | Muse Spark Open | Qwen3.8-2.4T | GLM 5.3 | Kimi K3 | 
|---|---|---|---|---|
| Artificial Analysis Intelligence Index | 39 | 40 | 45 | 44 | 
| JobBench (agentic) | 61.8 | 52.7 | 61.4 | 54.3 | 
| MCP-Atlas (tool use) | 89.2 | 84.5 | 84.2 | 82.3 | 
| OSWorld Verified (computer use) | 80.4 | – | – | 84.8 | 
| HLE-Diamond, no tools | 26.1 | 17.3 | 16.4 | 22.2 | 
| GPQA Diamond | 89.0 | 93.5 | 91.7 | 93.5 | 
| DeepSWE 1.1 (coding) | 60 | 57 | 67 | 68 | 
| Terminal-Bench 2.1 (coding) | 76.4 | 82.0 | 83.9 | 85.0 | 
| ZeroBench with tools (vision) | 50.0 | – | – | 41.0 | 
| VideoMME v2 (video) | 61.5 | – | – | 65.4 | 

Read together, the numbers tell a more mixed story than the headline.

- **Where Muse Spark Open leads:** tool calling across many steps (MCP-Atlas 89.2, the best in the table), the JobBench agent test, hard reasoning without tools on HLE-Diamond, and several image tests such as ZeroBench and SimpleVQA.
- **Where it trails:** pure coding. Kimi K3 and GLM 5.3 score higher on DeepSWE and Terminal-Bench, and both also beat it on the overall Artificial Analysis index. Kimi K3 is ahead on OSWorld computer use and on the video test.
- **Against other US open models:** the gap is large. Inkling scores 26 and Nemotron 3 Ultra 23 on the overall index, against 39 for Muse Spark Open. That is what backs Meta’s “most capable US open-weight model” claim. It is not a claim to beat every open model in the world.

Meta used xhigh reasoning effort for its own model and each rival’s recommended settings, as described in its published evaluation methodology. Some rival scores, for Beam and Mistral Large 4, were copied from those companies’ own launch posts.

### Safety results

This is the area where Meta separates itself most clearly. The open-weight version received extra safety training because anyone can run it without guardrails. On the StrongREJECT v2 adaptive jailbreak test, where lower is better, Muse Spark Open scores 0.5 while Kimi K3 scores 66.6, GLM 5.3 62.9 and Qwen3.8 65.6. In Meta’s cyber misuse tests it refused 99.2% of jailbreak attempts on an internal benchmark, compared with 75.5% for Kimi K3 in the same setup.

For a business that wants to expose a model to customers, such as a support chat on a website, that gap matters more than a few points on a coding test.

## Muse Spark Open pricing

The weights are free to download under the license terms below. If you use a hosted API, you pay the provider. On launch day, OpenRouter listed Muse Spark Open at these prices:

- **Input:** $1.25 per million tokens.
- **Output:** $4.25 per million tokens.
- **Cached input:** $0.15 per million tokens.

For context, the same list showed Kimi K3 at $0.45 input and $15 output per million tokens, GLM 5.3 at $0.40 and $8, and Qwen3.8-2.4T-A95B at $2 and $6. So Muse Spark Open is mid-priced on input and cheaper than its closest coding rivals on output, which is where agent workloads spend most of their money. Prices on OpenRouter change often and differ by provider, so check the live page before you budget.

Running it yourself has no per-token fee, but the hardware cost is real. Two M5 Ultra Mac Studios or a multi-GPU server with 700 GB of memory is out of reach for most small businesses, which is why the hosted route will be the realistic option for most readers.

## The Muse Model License in plain words

This is not a standard open-source license, and it is worth reading before you build on it. The main terms:

- You may use, copy, modify, fine-tune, distill, redistribute and sell the model and derivatives, free of charge.
- You must show “Built with Muse Spark Open” in your product’s interface, documentation, website or settings menu.
- If you sell access to AI models as a service through an API, and your company and its affiliates earn more than 20 million US dollars in any 12-month period, you must notify Meta and sign a separate agreement first.
- That revenue condition does not apply to internal use, research, or using the model to power features inside your own product that is not a model-as-a-service business.
- Meta claims no ownership of the outputs.
- A usage policy applies. It bans uses such as malware, fraud, spam, fake reviews, and presenting the model’s output as human-written. The model is not intended for anyone under 18.

For nearly every agency, store or SaaS founder in the region, the 20 million dollar threshold will never apply. The attribution line and the usage policy will.

## Pros and cons

### Pros

- Open weights you can host privately, which matters for companies with strict data rules.
- Strong at multi-step tool use and agent tasks, the work behind coding agents and business automation.
- Reads images, audio and video, not just text, in a single model.
- A one-million-token context window, enough for a full codebase or a long contract set.
- Much stronger refusal of harmful requests than the main open rivals, according to Meta’s tests.
- Wide availability across hosted providers, with OpenAI-compatible endpoints.

### Cons

- Not the top open model for pure coding. Kimi K3 and GLM 5.3 score higher on the coding benchmarks Meta itself published.
- Self-hosting needs at least 700 GB of GPU memory, so it is a server-room or data-center model.
- The license requires visible attribution and adds terms for large API businesses.
- Weight access is gated and requires personal and company details.
- Text output only, so it does not generate images or speech.
- All results are self-reported and only hours old.

## Best use cases

- **Private coding agents:** teams that cannot send source code to a closed API can run an agent-grade model inside their own network.
- **Business automation:** workflows that call many tools in sequence, such as reading an order, checking stock, drafting a reply and updating a CRM.
- **Document and screenshot reading:** invoices, scanned forms, dashboards and product photos.
- **Customer-facing assistants** where low jailbreak rates reduce the risk of embarrassing or harmful replies.
- **Synthetic data and model evaluation:** the model card lists both as intended uses.

If your main need is time series forecasting for stock or demand rather than language and code, a smaller specialist model is a better fit. We covered one such option, which also runs on your own hardware, in our article on Mistral Timestral and AI demand forecasting.

## What Meta Muse Spark Open means for WordPress sites and businesses

For most WordPress site owners, Muse Spark Open will arrive through an API key, not a download. Many WordPress AI plugins already let you set a custom OpenAI-compatible endpoint, and OpenRouter is a common option inside them. That means you can try the model for content drafting, product description writing or a support chatbot without changing your stack.

For the newer app-based voice and avatar features, see our Meta Muse voice and avatars guide; those features should not be confused with deploying the open model.

A few practical points before you do:

- **Check the attribution rule.** If a customer-facing feature on your site runs on Muse Spark Open, the license expects “Built with Muse Spark Open” to appear somewhere visible, such as the chat window or your site’s AI disclosure page.
- **Do not use it for fake reviews or undisclosed AI content.** Both are banned by the usage policy, and both also hurt trust and search rankings.
- **Use the right effort level.** For short chat replies on a store, low or medium effort will be faster and cheaper than the xhigh default.
- **Keep guardrails.** Meta itself recommends deploying the model inside a system with its own safety checks, so a plugin with moderation and logging is a better choice than a raw API call.
- **Consider privacy.** For clinics, law firms or finance businesses, the real value is private hosting, which keeps customer data off third-party servers. That is a bigger project and needs proper infrastructure.

If you want an AI assistant, an automated workflow or a custom plugin built on a model like this, our WordPress development team can plan and build it. If you are not sure whether a hosted or private setup suits your business, book a free consultation and we will look at it with you.

## Frequently asked questions

## Is Muse Spark Open free?

The weights are free to download and use under the Muse Model License. Hosted APIs charge per token. OpenRouter listed it at $1.25 per million input tokens and $4.25 per million output tokens on launch day.

## Can I run Muse Spark Open on my own server?

Yes, but you need at least 700 GB of GPU memory for the NVFP4 version or about 2 TB for BF16. Meta also shows it running on two Mac Studios with the M5 Ultra chip using MLX.

## Is Muse Spark Open better than Kimi K3 for coding?

Not on Meta’s own numbers. Kimi K3 scores higher on DeepSWE 1.1 and Terminal-Bench 2.1. Muse Spark Open leads on multi-step tool use, some reasoning tests and safety.

## Can I use Muse Spark Open commercially?

Yes. You must display “Built with Muse Spark Open” and follow the usage policy. A separate agreement with Meta is only needed if you run a model-as-a-service business earning more than 20 million US dollars a year.
