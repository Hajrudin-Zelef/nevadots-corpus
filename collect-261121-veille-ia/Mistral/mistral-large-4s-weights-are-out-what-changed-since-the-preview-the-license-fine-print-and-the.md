---
id: collect-261121-veille-ia/mistral/mistral-large-4s-weights-are-out-what-changed-since-the-preview-the-license-fine-print-and-the
title: "Mistral Large 4’s Weights Are Out: What Changed Since the Preview, the License Fine Print, and the…"
domain: mistral
role: reference
task: article
actors: ["Mistral AI"]
dates: ["2026-10-29"]
keywords: ["mistral", "large", "model", "preview", "license", "about", "weights", "changed"]
source: https://medium.com/@ascendrixtech/mistral-large-4s-weights-are-out-what-changed-since-the-preview-the-license-fine-print-and-the-2616f39134f8
source_anchor: ""
source_lines: [1, 120]
section: "Mistral"
---
# Mistral Large 4’s Weights Are Out: What Changed Since the Preview, the License Fine Print, and the Hardware You’ll Need

Three weeks after its public preview, **Mistral Large 4**, nicknamed **“Le Chonk,”** is now an **open-weight model.** On **October 29, 2026**, Mistral published the full weights on **Hugging Face**, making it the **largest open-weight model any US or European lab has released.**

The released version isn’t just the preview with a download button. It’s **noticeably stronger**, it’s **about half the price** through Mistral’s API, and its license has some fine print every business should read.

Here’s what changed, what you can and can’t do with it, and what it takes to actually run a trillion-parameter model.

### What was released

- **Model:**`mistralai/Mistral-Large-4-1T-A52B` on Hugging Face
- **Size:** about**1.05 trillion parameters** in a mixture-of-experts design, with roughly**52 billion active per token** , according to the released model’s name and specs. The preview was described with about 49 billion active.
- **Context window:****1 million tokens**
- **Inputs:****text and images** (with a vision encoder of about 1.6 billion parameters), output in text
- **Languages:****160+**
- **Training:** from scratch on**3,800 NVIDIA Grace Blackwell GPUs** in Mistral’s European data centres
- **Two official versions:**
- **FP8:** about**1.06 TB**
- **NVFP4:** about**610 GB** , a 4-bit format for NVIDIA’s newest Blackwell GPUs

### What changed since the preview

**1. Better at agentic coding.** Mistral says **three more weeks of reinforcement learning**, guided by customer feedback, lifted the model’s score on **Terminal-Bench 4.0**, a benchmark for agents completing real tasks in a terminal, from **28.3% to about 41%.** Independent replication is still pending, so treat that number with some caution.

**2. More control over thinking.** Early testers, which Mistral describes as governments and enterprises, wanted **more control over how long the model “thinks”** before answering. Balancing speed, cost, and quality is crucial in production, so this is a practical improvement.

**3. Roughly half the price.** On the day the weights shipped, Mistral **cut its API price roughly in half**, to about **$0.70 per million input tokens and $2.10 per million output tokens**, down from $1.36 and $4.18 during the preview.

**4. A strong open model, not a frontier leader.** Mistral calls Large 4 the **best open model for coding and cyber defence**, and reports top results among open-weight models on **FrontierCode 2.0 and Terminal-Bench 4.0.** But independent coverage notes that the coding lead over **GLM-5.3** is about **one point** on a single index. It’s a strong open model, but it **doesn’t dethrone the leading closed models.**

### The license: read this before you build a business on it

Mistral Large 4 uses the new **Mistral Ecosystem Open-Weight License**, described as a **modified MIT license.** In practice:

**You can:**

- **Download** the weights without approval
- **Use it commercially** inside your own products
- **Fine-tune** and modify it
- **Run it on your own infrastructure** , including on-premises

**You need a separate agreement with Mistral to:**

- **Offer it as a hosted model service** to third parties, for example selling API access to Large 4 itself

So a company using Large 4 inside its own app, chatbot, or internal tools is fine. A company planning to become a **Large 4 API provider** needs to talk to Mistral first.

It’s also **open-weight, not fully open-source.** The trained model is available, but the **training data isn’t**, and the hosting restriction goes beyond a standard MIT license.

### The memory math: can you run it?

Here’s where “free to download” meets reality. A rough estimate of memory needed for the **weights alone:**

```
params = 1.05e12          # total parameters
fp8_gb = params * 1.0 / 1e9      # 1 byte per parameter
nvfp4_gb = params * 0.58 / 1e9   # ~4 bits plus scaling data
print(f"FP8 weights:   ~{fp8_gb:,.0f} GB")
print(f"NVFP4 weights: ~{nvfp4_gb:,.0f} GB")
```
That roughly matches the official checkpoint sizes: about **1.06 TB for FP8** and **610 GB for NVFP4.**

And that’s **just the weights.** Real serving also needs memory for the **KV cache**, which grows with context length and the number of users. Using anywhere near the **1 million-token context** adds a lot more.

Mistral’s own serving examples use:

- **8 GPUs for FP8**
- **4 to 8 GPUs for NVFP4** , on NVIDIA**B200, B300, GB200, or GB300** Blackwell GPUs

In other words, Large 4 is a **data-centre model.** It won’t run on a laptop, a gaming PC, or a single GPU server. Even though it’s mixture-of-experts and only about 52 billion parameters are active per token, **all 1 trillion must be stored in GPU memory**, because any expert may be needed for the next token.

A typical multi-GPU deployment with an inference server like vLLM looks something like this (check the model card for the exact recommended settings):

```
vllm serve mistralai/Mistral-Large-4-1T-A52B \
  --tensor-parallel-size 8 \
  --tokenizer-mode mistral
```
### If you can’t self-host, the API is the practical option

For most teams, the easiest way to use Large 4 is through a hosted service:

- **Mistral’s API** (as`mistral-large-4` ), at the new lower price
- **OpenRouter**
- **Microsoft Foundry**
- **Mistral’s Vibe** coding agent, for some users

Hugging Face also offered **demo Spaces** with a small free allowance during the first week after release.

### Who should care about the open weights?

**Self-hosting makes sense if you:**

- Need **data to stay on your own infrastructure** , for example in government, banking, defence, or healthcare
- Want **European data residency** and independence from US cloud providers
- Plan to **fine-tune** a very large model for a specialised domain, such as security or a specific language
- Already have access to **multi-GPU Blackwell servers**

**The API is probably better if you:**

- Want to **test the model** before committing to hardware
- Have **moderate usage** , where per-token pricing beats owning GPUs
- Don’t have specific data-control requirements

### The bigger picture

The open-weight frontier has grown fast in 2026. Chinese labs have released several trillion-parameter-class open models, including **Meituan’s LongCat-2.0** in June, trained on more than 50,000 domestic Chinese accelerators. Large 4 is Europe’s answer: a trillion-parameter open model, trained in Europe, with **Mistral’s own data centres and a growing NVIDIA partnership** behind it.

For developers, the trend is clear: **open models keep getting bigger and better**, but running the largest ones is still a serious infrastructure decision. The practical skill is knowing **which model fits which job**, from small local models like Flint for everyday coding to trillion-parameter models like Large 4 for the hardest, most sensitive workloads.

### Practical recommendation

1. **Test it through the API first.** Run 20 to 50 of your real tasks and compare quality, speed, and cost with what you use now, at the new lower price.
2. **Read the license** if you plan any commercial use, especially anything that looks like offering the model as a service.
3. **Only consider self-hosting** if you have clear data-control needs and access to multi-GPU Blackwell hardware. Do the memory math, including the KV cache, before buying anything.
4. **Wait for independent benchmarks** before relying on the Terminal-Bench improvement.

Mistral Large 4 is a milestone for open AI in Europe. Just remember that “open” means you’re *allowed* to run it, not that it’s *easy* to run it.

*Sources: Mistral’s October 29, 2026 announcement as reported by The Next Web, Euronews, Fortune and ZDNet; Hugging Face’s “Welcome Le Chonk” blog and model card; and coverage from TUN, Apple Scoop, CellCog, Online Tech Tips, and DEV Community, plus The Decoder and TechRadar on Meituan’s LongCat-2.0.*
