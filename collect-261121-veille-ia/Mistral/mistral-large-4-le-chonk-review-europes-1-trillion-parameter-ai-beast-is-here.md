---
id: collect-261121-veille-ia/mistral/mistral-large-4-le-chonk-review-europes-1-trillion-parameter-ai-beast-is-here
title: "Mistral Large 4 (“Le Chonk”) Review: Europe’s 1-Trillion-Parameter AI Beast Is Here"
domain: mistral
role: reference
task: article
actors: ["Mistral AI"]
dates: ["2026-10-06"]
keywords: ["mistral", "large", "model", "chonk", "trillion-parameter", "european", "models", "just"]
source: https://ai.octodance.com/mistral-large-4-le-chonk-review-europes-1-trillion-parameter-ai-beast-is-here/
source_anchor: ""
source_lines: [1, 163]
section: "Mistral"
---
Europe just answered back. On October 6, 2026, Mistral AI launched the public preview of **Mistral Large 4** — unofficially nicknamed **“Le Chonk”** — a 1-trillion-parameter behemoth that’s being called the most capable open-weight AI model ever built. And yes, it’s already making waves in the benchmarks, the developer community, and the AI sovereignty conversation worldwide.

If you’ve been following Mistral’s trajectory — from the scrappy Paris startup that dropped open weights when everyone else was locking them behind APIs, to building a genuine challenger to OpenAI and Google — this moment feels like a coming-of-age. Mistral Large 4 isn’t just big. It’s strategically, philosophically, and technically important. Let me break it all down.

## What Is Mistral Large 4?

Mistral Large 4 (ML4) is a natively multimodal **Mixture-of-Experts (MoE)** model with **1.05 trillion total parameters** and 49–52 billion active parameters per token. To put that in perspective: the MoE architecture means the model doesn’t activate all its parameters at once — only the most relevant “experts” fire for any given input. This keeps latency and compute costs from going completely off the rails even at this scale.

It accepts both **text and image inputs**, powered by a 1.6 billion parameter vision encoder. The context window is a generous **1 million tokens**, with up to 262,144 completion tokens — meaning you can feed it entire codebases, lengthy legal documents, or hours of meeting transcripts without hitting a wall.

The model was trained from scratch on **3,800 NVIDIA Grace Blackwell GPUs** in Mistral’s own European data centers — no AWS, no Azure, no GCP. This is a deliberate choice, and we’ll get to why that matters in a moment.

## The Benchmarks: How Does “Le Chonk” Actually Perform?

Benchmarks are always a grain-of-salt situation, but the ML4 numbers are genuinely impressive. Here’s a summary of the key scores:

| Benchmark | ML4 Score | Context | 
|---|---|---|
| DeepSWE v1.1 (Coding) | 61.7% | Top open-weight model | 
| CyBench (Cybersecurity) | 93% | Far ahead of closed models | 
| Lakera B3 AI Security | 93.3% attack resistance | Beats GPT-6 and Claude Opus 5.5 | 
| FinWorkBench (Finance) | 67% | Tied with DeepSeek V4 Pro | 
| AutomationBench (Agents) | 59.9 | Near GLM-5.3 level | 
| Dense200 Visual Grounding | 42% | Surpasses GPT-6 Astra | 
| DIOR-RSVG (Satellite Imagery) | 73% | Best in class | 

The cybersecurity numbers deserve special attention. In a field where many frontier closed models (including Claude Opus 5.5 and GPT-6 Astra) score *near zero* on tasks like vulnerability reproduction and patching because of safety refusals, ML4 actually performs the task — scoring 82% on a real-world patching test. Mistral is very intentional about this: open weights + self-deployment means organizations can run security work under their own policies without relying on a provider’s guardrails.

On human blind evaluation by Surge AI, ML4 preview ranked **second among five leading models** (score: 3.74), behind only Claude Opus 5. That’s remarkable for an open-weight model — it means in side-by-side comparisons with human raters, ML4 is in the same conversation as the best proprietary models on earth.

## The “AI Sovereignty” Angle — Why This Actually Matters

Mistral keeps using the phrase “AI sovereignty” and it’s not just marketing. When you’re a European bank, a government department, or a healthcare provider, you have real constraints on where your data goes and whose laws govern your AI infrastructure. US-based AI providers operate under US jurisdiction — including potential subpoenas, export controls, and policy changes that could affect your access overnight.

ML4 is served on European infrastructure, under European law, operated end-to-end by Mistral — not rented from a hyperscaler. The model weights will be released openly by end of October, meaning any organization can run ML4 entirely on-premises. For a hospital in Germany, a defense contractor in France, or a law firm in Singapore that wants EU-compliant AI, this is the pitch.

It was also trained on data spanning **160+ languages**, including every official EU language — Arabic, Japanese, Chinese, Hindi, and many more. This isn’t an English-first model that tolerates other languages. It genuinely performs across linguistic contexts.

## Key Use Cases and Strengths

### 🔧 Coding & Software Engineering

ML4 places **first among open-weight models** on the Coding Agent Index, with a combined score of 49.8%. It outperforms DeepSeek V4 Pro 0813 and Qwen3.8 Max on repository-level coding tasks. If you’re building an AI coding assistant or a dev-tooling pipeline and you want to self-host your model, ML4 is the strongest option available once weights drop.

Terminal-bench performance (28.3% on Terminal-Bench 4.0) also shows it can handle the kind of multi-turn, file-system-aware coding sessions that tools like Claude Code and Codex specialize in. For open-weight, this is genuinely new territory.

### 🔒 Cybersecurity

This might be ML4’s most differentiated vertical. Mistral worked with cybersecurity leaders, vetted partners, and state authorities during the preview period to red-team the model. The result: ML4 can assist with log inspection, code auditing, vulnerability reproduction, and patch development — capabilities where closed-model providers often refuse entirely.

For red teams, pentesters, and security researchers, having a frontier-class model that will actually help with legitimate offensive security work — and can be run locally — is a game changer. Expect enterprise security tooling to adopt ML4 quickly once weights are released.

### 📊 Finance, Legal, and Enterprise

ML4 scores 67% on FinWorkBench (finance tasks) and 15% task-pass rate on Harvey’s Legal Agent Benchmark, where it outperforms both Kimi K3 and GPT-6 Astra. These aren’t flashy consumer metrics — they’re the grind of document analysis, clause extraction, financial modeling, and regulatory compliance that enterprises actually pay for.

Mistral trained ML4 with direct input from enterprises in finance, logistics, manufacturing, pharmaceuticals, and the public sector — which explains why these industry-specific benchmarks are strong, not just the general-purpose LLM leaderboards.

### 👁️ Multimodal & Visual Intelligence

ML4’s vision capabilities go beyond the typical “describe this image” use case. Its **73% score on DIOR-RSVG** (satellite and aerial imagery analysis) means it can interpret complex geospatial imagery — useful in logistics, agriculture, defense, and urban planning contexts. The 1.6B vision encoder also handles charts, complex documents, screenshots, and product photos with strong accuracy.

## Pricing: Surprisingly Competitive

The launch preview pricing is:

- **Input:** $0.68 per million tokens (launch sale; list price $1.36)
- **Output:** $2.09 per million tokens (list price $4.18)
- **Cached input:** $0.07 per million tokens
- **Batch discount:** 50% off
- **EU regional deployment:** 1.1x pricing
- **Priority tier:** 1.75x list pricing

Compared to Claude Opus 5.5 (roughly $15/$75 per million tokens input/output) or GPT-6 Astra pricing in a similar tier, ML4’s pricing is dramatically more accessible for high-volume workloads — especially if you batch. And when the weights drop end of October, self-hosting eliminates per-token costs entirely for organizations with the infrastructure to run it.

## How to Access Mistral Large 4 Right Now

ML4 is available in **public preview** via Mistral’s API. Here’s how to get started:

### 1. Via Mistral Studio (Console)

Head to console.mistral.ai and create an account or log in. Navigate to the Playground and select **mistral-large-4** from the model dropdown. You’ll get access to the chat interface and can immediately start testing it with your own prompts — no waitlist.

### 2. Via API

```
import os
from mistralai import Mistral
client = Mistral(api_key=os.environ["MISTRAL_API_KEY"])
response = client.chat.complete(
    model="mistral-large-4",
    messages=[
        {
            "role": "user",
            "content": "Analyze the security vulnerabilities in this code snippet and suggest patches."
        }
    ],
    reasoning_effort="medium"  # New parameter: "low", "medium", "high"
)
print(response.choices[0].message.content)
```
The new `reasoning_effort` parameter is worth noting — it lets you dial up or down the model’s chain-of-thought reasoning, trading latency for quality depending on your use case. For quick classification tasks, keep it low. For complex agentic workflows or security analysis, max it out.

### 3. Via OpenRouter

ML4 is also available at OpenRouter as `mistralai/mistral-large-4-0`, which means you can drop it into any OpenAI-compatible client with a one-line model ID swap. Perfect if you’re already using LiteLLM, LangChain, or any other framework.

## Pros and Cons

### ✅ Pros

- **Open weights (end of October):** Self-host it, fine-tune it, run it fully air-gapped
- **Genuinely frontier-class performance:** #2 in human evals, competitive with Claude Opus 5
- **Cybersecurity-capable:** Performs offensive security tasks that closed models refuse
- **Massive context window:** 1 million tokens with 256K output
- **Multilingual:** 160+ languages, every EU official language
- **Affordable API pricing:** $0.68 input / $2.09 output (launch pricing) — 10–20x cheaper than top closed models
- **EU-sovereign infrastructure:** Real option for regulated European enterprises
- **MoE efficiency:** Trillion parameters, but only ~50B active per token keeps it practical

### ❌ Cons

- **Still in preview:** Weights aren’t public yet — you’re dependent on Mistral’s API until end of October
- **Self-hosting requirements:** Running a trillion-parameter model is not for the faint-hearted — you’ll need serious GPU infrastructure
- **Not #1 overall:** Claude Opus 5 still leads in human preference evals
- **Context caching still pricey:** At $0.07/M cached tokens, repeated large-context queries add up
- **Le Chonk nickname is fun, but finding it in search is awkward:** “Mistral Large 4” + “Le Chonk” = confusing for new users

## Mistral Large 4 vs. The Competition

Where does ML4 actually fit in the broader landscape? Let’s be direct:

- **vs. Claude Opus 5:** Claude still wins in human preference evals (38.5% of head-to-heads go to Claude). But Claude is proprietary, expensive, and you can’t self-host it. If sovereignty or cost matters, ML4 wins.
- **vs. GPT-6 Astra:** ML4 outperforms GPT-6 on several specialized benchmarks (cybersecurity, satellite imagery, legal). OpenAI’s pricing and terms remain restrictive for many enterprise use cases.
- **vs. DeepSeek V4 Pro:** ML4 is competitive in coding and finance. DeepSeek offers lower pricing on some tiers, but Mistral’s EU-sovereign infrastructure is a differentiator for certain regulated industries.
- **vs. Qwen 3.8 Max:** ML4 leads on the Coding Agent Index. Qwen has its own sovereignty story (Chinese infrastructure), which creates a different set of regulatory concerns depending on your geography.

The honest positioning: ML4 is **the best open-weight model in the world right now**, and in some narrow but important verticals (cybersecurity, satellite imagery analysis), it beats the closed models too. For anyone who needs a frontier-class model they can actually control, own, and customize — it has no real competition yet.

## What Happens When the Weights Drop?

Mistral says weights release at the end of October 2026, after additional red-teaming with cybersecurity partners and state authorities. Once they’re out:

- The Hugging Face community will have fine-tuned versions within days
- Quantized versions (GGUF, GPTQ, AWQ) for consumer and semi-pro hardware will appear on Ollama and LM Studio
- Enterprise deployments on Kubernetes/Ray Serve clusters will proliferate
- Expect a wave of domain-specific fine-tunes: medical, legal, finance, security

In raw compute terms, running a full FP16 1T-parameter model requires hundreds of high-end GPUs. But quantized MoE models — where only 50B parameters are active at any time — are much more tractable. Expect community-optimized versions for 8-GPU workstations within a few weeks of weight release. Think of it like running Mixtral 8x22B but smarter and bigger.

## My Take: Is This Worth Your Attention?

Absolutely yes — and not just for the benchmarks. The Mistral Large 4 launch is significant for a few reasons that go beyond the numbers:

**1. Open weights at this scale changes the game.** Before ML4, if you wanted frontier-class performance, you were locked into API dependency with OpenAI, Anthropic, or Google. Now you have an option that you can actually own. This creates competitive pressure on closed providers to either lower prices or release more capable open weights themselves.

**2. European AI is real now.** Mistral went from a small French startup in 2023 to training trillion-parameter models on their own European GPU clusters in 2026. That’s not a story about government subsidies and protectionism — it’s a genuine technical and commercial achievement.

**3. The cybersecurity vertical is being unlocked.** This is probably the most underappreciated angle of ML4. Security research, red-teaming, and incident response are legitimate use cases that closed models have been squeamish about for years. ML4 filling that gap — with open weights you can verify and audit — is going to generate real enterprise adoption.

If you’re a developer, start experimenting via the API now. If you’re planning infrastructure, keep October in your calendar for the weights release. And if you’re still paying Anthropic or OpenAI rates for high-volume workloads where absolute best-in-class quality isn’t required — ML4 at batch pricing is going to save you a lot of money.

*Tia Li writes about AI tools, emerging models, and the tech shaping how we build and work. Have a question about ML4 or want to see a specific benchmark explored in more depth? Drop it in the comments.*
