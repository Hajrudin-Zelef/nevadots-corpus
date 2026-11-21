---
id: collect-261121-veille-ia/muse/muse-spark-open-metas-open-model-pricing-techpilot-ai
title: "Muse Spark Open: Meta’s Open Model & Pricing | TechPilot.ai"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: ["2026-10-15"]
keywords: ["muse", "spark", "open", "model", "meta", "free", "weights", "context"]
source: https://techpilot.ai/tools/muse-spark-open/
source_anchor: ""
source_lines: [1, 162]
section: "Muse"
---
Muse Spark Open gives developers Meta’s Muse Spark as downloadable weights, with a 1M-token context and strong coding and agent skills.

## The AI news that actually mattered this week.

One short email every Friday: the biggest AI stories, what they mean for you, and the tools worth trying. A 3-minute read, no hype.

Free. Unsubscribe anytime.

 **You're in.**
Watch for the brief on Friday. Free. Unsubscribe anytime.

## Summary

## Pros and cons

### What we like

-  **Open weights** Download, self-host and fine-tune Meta's Muse Spark.
-  **1M-token context** Handles large codebases and long agent sessions.
-  **Low API price** $1.25 input and $4.25 output per 1M tokens on hosted providers.
-  **Many providers** OpenRouter, Together, Fireworks, Baseten, Modal, Nebius and Groq.
-  **Multimodal** Reads text and images, with tool calling and structured output.

### What could be better

-  **Huge hardware needs** Self-hosting takes two Mac Studios or big GPU servers.
-  **Limited commercial terms** The Muse Model License isn't fully permissive.
-  **Groq is enterprise-only** Limited preview with a 131K context.
-  **Full evaluation pending** Meta's complete report isn't out yet.

## Key features

- Open weights: BF16 and NVFP4 weights on Hugging Face and major clouds.
- Agentic coding: Built for long, multi-step coding and tool-use tasks.
- 1M-token context: Fit whole repositories or long documents in one prompt.
- Image input: Reason over screenshots, diagrams and photos.
- Runs on Apple silicon: Optimized for MLX on two M5 Ultra Mac Studios.
- Fine-tuning: Customize it with PyTorch TorchTitan.

## Screenshots

## Full review

Independent and reader-supported. Some links may earn us a commission; it never changes our scores. Editorial policy

Meta Muse Spark Open is the free open-weight version of Muse Spark: 816B parameters and a 1M-token context. See API pricing, hardware and how to run it.

On October 15, 2026, Meta did something many people didn’t expect: it released the weights of Muse Spark, the model behind its Muse app. If you searched for “Meta Muse Spark,” this page covers what Muse Spark Open is, whether it’s really free, what it costs through hosted APIs (prices checked on October 18, 2026), what hardware you need to run it yourself, and how it compares with the rest of Meta’s Muse family.

## What is Muse Spark Open?

Muse Spark Open is the open-weight version of Muse Spark, the model Meta Superintelligence Labs released in April 2026 to power the Meta Muse app and Meta AI. In its announcement, Meta calls it “the most capable US open-weight model to date.” The key specs:

- **Size:** 816 billion total parameters, with 42 billion active per token (a Mixture-of-Experts model).
- **Context window:** 1 million tokens.
- **Input and output:** natively multimodal. It takes text and images and writes text.
- **Built for:** coding and agent work, such as long multi-step tasks, complex software engineering and reasoning. On Groq it always reasons before answering, with adjustable reasoning effort.
- **Licence:** the Muse Model License, which Meta describes as having “limited commercial terms.”

Meta says it tested the model running entirely on two Apple Mac Studios, using Muse Code to ship real bug fixes and features to Ice Cubes, an open-source Mastodon app, and to drive DaVinci Resolve end to end, picking the best moments from footage and cropping them vertically.

## Is Muse Spark Open free?

The weights are free to download from Hugging Face, Amazon SageMaker AI, Google Cloud Model Garden and Microsoft Foundry Models. Meta says the Muse Model License “allow[s] many developers to use the model for free,” but it’s not a fully permissive open-source licence like Apache-2.0 or MIT. If you plan to build a big commercial product on it, read the licence terms first.

Running it isn’t free, though. An 816B model needs serious hardware, so most people will either pay per token through a hosted API or rent GPUs.

## Muse Spark Open Pricing (2026)

Hosted Muse Spark Open is billed per token. We checked these prices on October 18, 2026 on OpenRouter and the Together AI pricing page:

| Where | Input (per 1M tokens) | Output (per 1M tokens) | Cached input | Context | 
|---|---|---|---|---|
| **OpenRouter** | $1.25 | $4.25 | $0.15 | 1M tokens | 
| **Together AI** | $1.25 | $4.25 | $0.15 | 1M tokens | 
| **Groq** | Not published | Not published | – | 131K tokens | 
| **Self-hosted** | Free weights | Your hardware cost | – | 1M tokens | 

Together AI’s pricing page also listed a limited-time free promotion for the model, valid until October 18, 2026, so expect the standard rate from now on. Meta also names Baseten, Fireworks AI, Modal and Nebius as launch partners.

What does that mean in practice? A coding-agent call with 20,000 input tokens and 2,000 output tokens costs about $0.034, so roughly 30 calls per $1 (our math from the listed rates). Cached input brings repeated context down to $0.15 per million tokens. That’s the same price OpenRouter lists today for the closed Muse Spark 1.3, and far below Muse Wonder.

## How to run Muse Spark Open

### Through an API (easiest)

Create an account on OpenRouter or Together AI, add credit and call the model with any OpenAI-compatible client. On OpenRouter the model ID is `meta/muse-spark-open`; on Groq it’s `meta-models/muse-spark-open` once your organization has access.

```
from openai import OpenAI
# Needs: pip install openai, and an OpenRouter API key
client = OpenAI(base_url="https://openrouter.ai/api/v1", api_key="sk-or-your-key")
reply = client.chat.completions.create(
    model="meta/muse-spark-open",
    messages=[{"role": "user", "content": "Find the bug in this function: ..."}],
)
print(reply.choices[0].message.content)
```
### On your own hardware

- **On a Mac:** Meta says it’s the first model in its class to run on a desktop, using two Apple Mac Studios with M5 Ultra chips (up to 512 GB of unified memory each) and the latest MLX framework. MLX pulls the weights straight from Hugging Face.
- **On GPU servers:** it runs on SGLang, vLLM, TensorRT-LLM, Hugging Face Transformers, llama.cpp and Unsloth, with optimizations for AMD, Apple, Intel and NVIDIA chips. Hugging Face has BF16 and smaller NVFP4 versions of the weights.
- **Fine-tuning:** Meta points to PyTorch’s TorchTitan for further tuning on your own data.

If you don’t have a terabyte of memory, a smaller open model is more practical. Our guide to the best local AI models covers options that fit on one GPU or a laptop.

## Muse Spark Open vs Muse Spark vs Muse Wonder

| Model | Weights | API price per 1M tokens (input / output) | Context | 
|---|---|---|---|
| **Muse Spark Open** | Open (Muse Model License) | $1.25 / $4.25 on hosted providers | 1M tokens | 
| **Muse Spark 1.3** | Closed | $1.25 / $4.25 | 1M tokens | 
| **Muse Wonder** | Closed | $4 / $20 | 1M tokens | 

Muse Wonder is Meta’s bigger, more capable model, and it costs about 3 times more for input and almost 5 times more for output. See our Muse Wonder page for its plans. Muse Spark Open is the one to pick if you want Meta’s model on your own servers, want to fine-tune it, or want the cheaper per-token price with a choice of providers.

## Limitations

- **Huge hardware needs:** even the desktop setup Meta shows takes two top-end Mac Studios.
- **Not fully open source:** the Muse Model License has limited commercial terms.
- **Context varies by provider:** Groq serves it with 131K tokens of context, not 1M.
- **Full evaluation still to come:** Meta says detailed safety results are in the model card, with a full evaluation report “available soon.”

## Verdict

Muse Spark Open is a big deal for open models: a 1M-context, multimodal coding and agent model from Meta that you can download, fine-tune and host anywhere. For most developers the practical way in is a hosted API at $1.25 and $4.25 per million tokens, the same price as the closed Muse Spark. Self-hosting only makes sense if you have the hardware and have checked the licence fits your use.

## TechPilot.ai’s Verdict on Muse Spark Open

Muse Spark Open gives developers Meta’s Muse Spark as downloadable weights, with a 1M-token context and strong coding and agent skills.

Use a hosted API at $1.25/$4.25 per million tokens to start. Self-host only if you have the hardware and the Muse Model License fits your product.

## Muse Spark Open FAQ

## Is Muse Spark Open free?

The weights are free to download from Hugging Face and other platforms under the Muse Model License, which Meta says lets many developers use it for free, with limited commercial terms. Using it through a hosted API costs $1.25 per million input tokens and $4.25 per million output tokens.

•

## How much does Muse Spark Open cost?

As of October 18, 2026, OpenRouter and Together AI charge $1.25 per 1M input tokens, $4.25 per 1M output tokens and $0.15 per 1M cached input tokens. Groq offers it as an enterprise-only limited preview without a public price.

•

## What is Meta Muse Spark?

Muse Spark is the AI model from Meta Superintelligence Labs that powers the Muse app and Meta AI. It launched in April 2026. Muse Spark Open, released on October 15, 2026, is its open-weight version with 816B total parameters and a 1M-token context.

•

## Can I run Muse Spark Open locally?

Yes, with a lot of hardware. Meta shows it running on two Apple Mac Studios with M5 Ultra chips using MLX. On servers it runs on SGLang, vLLM, TensorRT-LLM, Transformers, llama.cpp and Unsloth.

•

## Is Muse Spark Open open source?

It’s open-weight, not fully open source. You can download and run the weights, but under the Muse Model License, which has limited commercial terms, rather than a permissive licence like Apache-2.0 or MIT.
