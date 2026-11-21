---
id: collect-261121-veille-ia/deepseek/glm-5-3-flash-benchmark-vs-gemini-deepseek-gpt-5-6
title: "GLM-5.3-Flash Benchmark vs Gemini, DeepSeek & GPT-5.6"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: ["2026-09-09"]
keywords: ["glm-5", "flash", "benchmark", "terminal-bench", "leads", "gpt-5", "model", "gemini"]
source: https://www.edenai.co/post/glm-5-3-flash-benchmark-vs-gemini-deepseek-gpt-5-6
source_anchor: ""
source_lines: [1, 273]
section: "DeepSeek"
---
Summarize this article with:

## GLM-5.3-Flash Benchmark Results in Full

The **glm 5.3 flash benchmark** results are strongest in agentic workflows and vision-heavy tasks rather than across every category. In Z.ai's own benchmark suite, GLM-5.3-Flash leads the comparison set on Toolathlon Verified, GDPval-AA v2, OfficeQA Pro, and Chartography with Tools. It does not lead Terminal-Bench 2.1, DeepSWE v1.1, or several pure vision benchmarks.

*Bold = best result in each row. All figures are Z.ai-reported.*

For a broader independent reference, Artificial Analysis gives GLM-5.3-Flash an **Intelligence Index score of 57**. That sits below the separate GLM-5.3 at 60 and below frontier reference points such as Claude Opus 5 at 63, Claude Fable 5 at 62, GPT-5.6 Sol at 61, and Grok 4.6 at 61.

### Where the gains came from

The largest generation-over-generation improvements appear in coding and automation.

On **DeepSWE v1.1**, Z.ai reports GLM-5.3-Flash at **63.4**, up from **46.2 for GLM-5.2**. That is a 17.2-point increase. For developers, DeepSWE is relevant because it evaluates software-engineering work rather than isolated code completion. The model has to operate on broader engineering tasks where repository context, code changes, and task completion matter together.

On **AutomationBench v1.0.6**, Z.ai reports a jump from **26.2 to 48.8**, an increase of 22.6 points. This is especially relevant for agentic production workloads because it focuses on multi-step automation rather than a single prompt-response interaction.

The broader table shows the same pattern. GLM-5.3-Flash reaches **78.4 on Toolathlon Verified** and **1773 Elo on GDPval-AA v2**, both the highest Z.ai-reported results in this comparison set. But it does not dominate every agent benchmark. Gemini 3.7 Flash leads AutomationBench at 52.3, while GPT-5.6 Terra leads Agents' Last Exam at 28.0.

Vision results are similarly mixed. GLM-5.3-Flash leads **OfficeQA Pro at 62.4** and **Chartography with Tools at 78.0**, but Gemini 3.7 Flash leads BabyVision, MVbench, and MMVU.

### Why Terminal-Bench 2.1 and 3.0 scores aren't comparable

GLM-5.3-Flash scores **84.3 on Terminal-Bench 2.1**, according to Z.ai. GPT-5.6 Terra leads the same reported comparison at 87.4, followed by Gemini 3.7 Flash at 85.8 and Opus 4.8 at 85.0.

Those numbers should only be compared with other **Terminal-Bench 2.1** results.

A Terminal-Bench 3.0 score is not an updated number on the same scale. It comes from a different benchmark version with different tasks and evaluation conditions. Putting a 2.1 result beside a 3.0 result and ranking the models numerically creates a comparison the underlying data does not support.

For this article, **Terminal-Bench 2.1 and Terminal-Bench 3.0 are treated as separate benchmarks, not interchangeable scoreboards**.

## What is GLM-5.3-Flash?

For six days, **GLM-5.3-Flash** was not GLM-5.3-Flash at all. Starting **20 August 2026**, the model appeared anonymously as **“Ox Alpha”** across OpenRouter, OpenCode, Cline, and Nous Research’s portal. Access was free, but the developer behind the model was undisclosed. On **26 August 2026**, Z.ai, also known as Zhipu AI, revealed Ox Alpha as GLM-5.3-Flash and officially released the model.

The distinction from **GLM-5.3** matters. GLM-5.3-Flash is a separate, smaller model rather than a faster serving configuration of the 743B-parameter GLM-5.3 released on 14 August.

GLM-5.3-Flash uses a **Mixture-of-Experts architecture with 320B total parameters and 18B active parameters per inference**. It supports a **1,048,576-token context window** and accepts **text, images, and video** as input, while generating text as output.

The weights are available on Hugging Face under an **MIT license**, so teams can use the hosted API or deploy the model independently where infrastructure allows.

**Key specs:**

- **Developer:** Z.ai (Zhipu AI)
- **Release date:** 26 August 2026
- **Architecture:** 320B MoE, 18B active parameters
- **Context:** 1,048,576 tokens
- **Input:** text, image, video
- **Output:** text
- **License:** MIT, open weights
- **API price:** $0.15/M input tokens and $0.50/M output tokens
- **Prompt-cache discount:** 83%
- **Launch promotion:** 50% discount through 9 September 2026

The positioning is straightforward: **GLM-5.3-Flash is a frontier-adjacent multimodal model priced at roughly one-tenth the cost of its larger GLM-5.3 sibling.**

## GLM-5.3-Flash vs GLM-5.3: Not the Same Model

Most coverage of **GLM-5.3-Flash** conflates it with the larger **GLM-5.3**. They are not two serving modes of the same model. They are separate releases with different parameter counts, modalities, access conditions, and pricing.

Z.ai released **GLM-5.3** on 14 August 2026 and **GLM-5.3-Flash** on 26 August. GLM-5.3 has **743B total parameters and 40B active parameters**, while Flash has **320B total and 18B active**. Both use a Mixture-of-Experts design, but GLM-5.3 keeps the same MoE base as GLM-5.2. Its gains come from post-training rather than a new architecture.

The biggest practical difference is modality. GLM-5.3 is **text-only**. GLM-5.3-Flash accepts **text, images, and video**, which makes it the relevant option for multimodal agent and document workflows.

## Is "Flash" Actually Fast? What the Throughput Numbers Show

**GLM-5.3-Flash is cheap, but it is not especially fast by current budget-model standards.** Artificial Analysis independently measures roughly **50 tokens per second**, below the **~67 tok/s median** for comparable models, with a **1.47-second time to first token**.

These are Artificial Analysis measurements, not Z.ai-reported figures, and throughput can vary by host, hardware, batching, region, and load. Still, the result makes one point clear: **“Flash” describes the price tier better than the latency profile**.

That is consistent with the architecture. GLM-5.3-Flash has **320B total parameters but only 18B active per inference**, which can reduce compute cost without automatically increasing wall-clock speed. Serving infrastructure still has a major impact on throughput.

The pricing reinforces that interpretation: **$0.15 per 1M input tokens and $0.50 per 1M output tokens**, with Artificial Analysis estimating **$0.09 per Intelligence Index task**.

At ~50 tok/s, the model is suitable for background agents, coding loops, document processing, extraction, and batch workflows. A 500-token answer takes roughly **10 seconds to generate** after the first token. It is less attractive for autocomplete, low-latency chat, and interactive coding tools where responsiveness is part of the user experience.

**Bottom line: GLM-5.3-Flash is a budget “Flash” model, not a latency-first one.**

## GLM-5.3-Flash vs Gemini 3.7 Flash: The Multimodal Matchup

**Gemini 3.7 Flash is ahead on most measured tasks, but GLM-5.3-Flash is dramatically cheaper and wins several document-oriented vision benchmarks.** For production teams, the decision comes down to whether Gemini's extra accuracy is worth a price gap that reaches 15x on output tokens.

*Benchmark figures are Z.ai-reported.*

### Gemini leads coding and agentic benchmarks

Gemini 3.7 Flash wins all three directly comparable coding and agentic rows here. On **Terminal-Bench 2.1**, it scores 85.8 versus 84.3 for GLM-5.3-Flash. On **DeepSWE v1.1**, the gap is 65.3 to 63.4. AutomationBench is slightly wider at 52.3 versus 48.8.

The differences are real, but not large. Gemini is ahead by roughly one to four points across these tests, while costing **5x more on input and 7.5x more on output today**. From January 2027, those gaps rise to **10x and 15x**. For high-volume agent loops, that price difference can matter more than a small benchmark lead.

### Vision depends on what kind of images you process

The multimodal results split more clearly by workload. Gemini is stronger on natural-image and video-oriented tests. It leads BabyVision by a large margin, 70.9 to 53.4, and also wins MVbench and MMVU. GLM-5.3-Flash performs better on structured visual reasoning. It scores **78.0 on Chartography with Tools versus 65.0 for Gemini**, while CharXiv Reasoning is essentially tied at 89.4 versus 88.7.

GLM-5.3-Flash also scores **62.4 on OfficeQA Pro**, ahead of Claude Opus 4.8 and DeepSeek-V4-Vision-Exp. Z.ai did not publish a Gemini 3.7 Flash score on that benchmark, so it should not be presented as a direct win over Gemini.

In practice, **Gemini is the safer choice for photographs, video, and broader visual understanding. GLM-5.3-Flash is more attractive for charts, invoices, forms, and scanned documents.**

### The deployment trade-off is just as important

Both models provide roughly a 1M-token context window, but Gemini supports more input types, including audio and PDF.

GLM-5.3-Flash has the stronger portability story. Its weights are available under an **MIT license**, so teams can self-host, fine-tune, or change hosting providers. Gemini 3.7 Flash is available as a managed Google API.

**Verdict: choose Gemini 3.7 Flash for stronger coding, video, audio, and natural-image performance. Choose GLM-5.3-Flash for document-heavy multimodal pipelines, much lower token costs, and workloads where open weights matter.**

## GLM-5.3-Flash vs DeepSeek V4-Flash: The Price Floor

**DeepSeek sets the price floor in this tier, and it does not charge extra for vision. The case for GLM-5.3-Flash here is capability, not integration convenience.**

Input pricing is effectively a tie. Output is not. GLM-5.3-Flash costs **$0.22 more per 1M output tokens**, or roughly **1.8x DeepSeek's output rate**.

That matters for agentic workloads, which can generate large amounts of output across repeated tool calls. At **100M output tokens**, the difference is: `100 × $0.22 = $22`

At **1B output tokens**, it becomes: `1,000 × $0.22 = $220`

There is also no hidden vision premium on the DeepSeek side. **DeepSeek V4-Flash-Vision-Exp uses the same $0.14 / $0.28 pricing**, with each image billed as up to 384 input tokens. Teams do not need to add a separately priced vision model just to process still images.

The reason to pay more for GLM-5.3-Flash is benchmark performance.

In Z.ai's own comparison, GLM-5.3-Flash leads DeepSeek on **11 of 13 reported benchmarks**. The gaps are particularly visible on agentic and structured-vision tasks:

- **AutomationBench:** 48.8 vs 38.8
- **GDPval-AA v2:** 1773 vs 1675
- **CharXiv Reasoning:** 89.4 vs 80.4
- **Chartography:** 78.0 vs 64.3
- **BabyVision:** 53.4 vs 35.1

DeepSeek leads only **NL2Repo, 57.7 to 56.3**, and **Agents' Last Exam, 27.3 to 26.3** in this Z.ai-selected benchmark set. All figures are **Z.ai-reported**, so they should be validated independently before production selection.

One capability difference is binary rather than incremental: **GLM-5.3-Flash accepts video input**, while DeepSeek's vision variant handles still images only, including JPEG, PNG, GIF, and WebP.

**Verdict: choose DeepSeek V4-Flash for high-volume text and image workloads where token cost dominates; choose GLM-5.3-Flash when video input or stronger agentic and chart-reasoning performance is worth paying 1.8x more for output.**

## GLM-5.3-Flash vs GPT-5.6 Luna: Open Weights vs Managed

**GPT-5.6 Luna is the closest US-hosted price peer to GLM-5.3-Flash, and the trade-off is unusually clean: GLM is cheaper and scores higher on independent evaluation, while Luna is much faster.**

The input-price difference is small. Output is where the economics diverge. Luna costs **2.4x more per generated token**, and agent loops are often output-heavy because planning, tool use, retries, and intermediate steps all generate tokens.

Artificial Analysis independently scores **GLM-5.3-Flash at 57** on its Intelligence Index. GPT-5.6 Luna reaches **51 to 52 at maximum reasoning effort**, then falls to 47 at high, 39 at medium, and 34 at low. That matters for cost comparisons because Luna reaches its best score while generating more tokens at **$1.20 per million output tokens**.

Luna's strongest advantage is speed. Artificial Analysis measures roughly **130 tok/s for Luna versus ~50 tok/s for GLM-5.3-Flash**. For interactive assistants and user-facing streaming, that difference is immediately visible. Luna also narrowly leads Terminal-Bench 2.1, **84.7 to 84.3**.

Long context is a different story. Luna advertises a **1M-token context window**, but scores only **41.3% on MRCR long-context recall**, versus 89.6% for Terra and 91.5% for Sol. The nominal context window is large, but its ability to retrieve information reliably across that window is much weaker.

OpenAI's **30 July 2026 repricing** is what made Luna competitive on price. Luna dropped 80%, from $1/$6 to $0.20/$1.20. Terra fell 20% to $2/$12, while Sol remained at $5/$30. That leaves a **25x output-price spread** between Luna and Sol.

GLM-5.3-Flash offers a structural alternative. Its **MIT-licensed weights** can be self-hosted or moved between inference providers. That does not guarantee lower total cost, but it turns vendor pricing and data-location decisions into infrastructure choices the team controls.

**Verdict: choose GPT-5.6 Luna when latency dominates and your working context stays short; choose GLM-5.3-Flash for higher measured intelligence at 2.4x lower output cost, video input, long-context workloads, and teams that need the option to self-host or switch providers.**

## Does It Really Approach Claude Opus 4.8?

Yes. On Artificial Analysis's current independent Intelligence Index, GLM-5.3-Flash and Claude Opus 4.8 are tied at 57. The more important question is why Z.ai chose a model Anthropic has already deprecated as its headline comparison.

Artificial Analysis currently scores GLM-5.3-Flash at 57 and Claude Opus 4.8 at 57. Older launch coverage may cite 61.4 for Opus 4.8, but Artificial Analysis has since revised the index composition. Opus 4.8 now ranks #16 of 187 and is flagged as deprecated in favor of Claude Opus 5.

On Z.ai's own benchmark set, GLM-5.3-Flash wins nine of fifteen head-to-head comparisons.

*Benchmark figures above are Z.ai-reported.*

The pattern is clearer than a simple "almost as good." Opus 4.8 is stronger at writing and reasoning about code, while GLM-5.3-Flash is stronger across several agentic, document, and multimodal tasks.

Price makes the trade unusually asymmetric. Opus 4.8 costs $5 per 1M input tokens and $25 per 1M output tokens, versus $0.15 and $0.50 for GLM-5.3-Flash. That is roughly 33x more on input and 50x more on output. Output speed is much closer: about 58.6 tok/s for Opus 4.8 versus ~50 tok/s for GLM-5.3-Flash.

The caveat is benchmark selection. Z.ai chose both the benchmarks and the opponent. Opus 4.8 was frontier-class when it launched, but the current Artificial Analysis frontier band is higher: GLM-5.3 at 60, GPT-5.6 Sol and Grok 4.6 at 61, Claude Fable 5 at 62, and Claude Opus 5 at 63.

So both conclusions can be true. GLM-5.3-Flash genuinely matches or beats Claude Opus 4.8 across much of this evaluation at roughly one-fiftieth the output cost. But it is not a current frontier model, and Z.ai's choice of a deprecated comparison point makes the headline look stronger than a comparison against today's leaders would.

## Cost per Task, Not Cost per Token

**Per-token prices are easy to compare, but they are not what production teams actually pay for.** Models differ in output verbosity, and agentic workloads are especially output-heavy because planning, tool calls, retries, and intermediate reasoning all generate tokens.

A better comparison is **cost per completed task**.

For the normalized example below, assume each agent task uses:

- **10,000 input tokens**
- **3,000 output tokens**
- **1,000 tasks total**
- **50% of GLM-5.3-Flash input is served from cache**
- No caching is applied to the other models

These are modeling assumptions, not measured averages. Replace them with your own production traces.

The base formula is:

`Cost per 1,000 tasks = 1,000 × [(input tokens ÷ 1M × input price) + (output tokens ÷ 1M × output price)]`

For GLM-5.3-Flash, the 83% cache discount means cached input is billed at 17% of the normal input rate:

`Cached GLM input price = $0.15 × 17% = $0.0255 / 1M`

So its input calculation becomes:

`5M uncached × $0.15/M + 5M cached × $0.0255/M`

Under this workload, DeepSeek remains the cheapest at **$2.24 per 1,000 tasks**, with GLM-5.3-Flash close behind at **$2.38** once caching is included.

The larger differences come from output pricing. Across 1,000 tasks at 3,000 output tokens each, the workload generates **3M output tokens**. Every additional **$1 per 1M output tokens adds $3 to the final bill**.

That is why Luna at $1.20 output costs more than twice as much per task as GLM-5.3-Flash, even though their input prices differ by only five cents. It is also why GLM-5.3 becomes expensive quickly at **$4.40 per 1M output tokens**.

GLM-5.3-Flash is temporarily cheaper still. Z.ai is offering a **50% launch discount through 9 September 2026**. 

Artificial Analysis provides a useful independent cross-check. It measures GLM-5.3-Flash at **$0.09 per Intelligence Index task**. That is a different workload and methodology, so it cannot be compared directly with the synthetic 1,000-task calculation above. It does support the same broader finding: **GLM-5.3-Flash looks stronger when cost is normalized by completed work rather than headline token price alone.**

For production, the metric to track is not dollars per million tokens. It is **dollars per successful task after output length, caching, retries, and routing are included.**

## When to Use GLM-5.3-Flash, and When Not To

### Use GLM-5.3-Flash when

- **You are running high-volume agent loops** where output cost matters more than absolute latency. At**$0.50 per 1M output tokens** , it stays well below GPT-5.6 Luna, Gemini 3.7 Flash, and the larger GLM-5.3.
- **You process invoices, forms, charts, scanned documents, or mixed office files.** Z.ai reports**62.4 on OfficeQA Pro** and**78.0 on Chartography with Tools** , both strong results for structured visual workloads.
- **Your pipeline needs text, image, and video in one model.** GLM-5.3-Flash supports all three input types, which makes it a better fit than text-only models and image-only vision variants when video is part of the request path.
- **You need a long-context model for document-heavy agents.** Its**1,048,576-token context window** gives you room for large repositories, long sessions, and multi-document workflows.
- **You need open weights for self-hosting or provider portability.** GLM-5.3-Flash is**MIT-licensed with weights on Hugging Face** , so you are not locked into a single managed API.
- **You have repeated system prompts or shared context.** The**83% cache discount** can materially reduce input cost when large portions of the prompt are reused.

### Don't use GLM-5.3-Flash when

- **You are building autocomplete, live coding suggestions, or latency-sensitive chat.** Artificial Analysis measures roughly**50 tok/s** , below the ~67 tok/s median for comparable models and far behind GPT-5.6 Luna at roughly 130 tok/s.
- **You need the highest broad reasoning performance available.** GLM-5.3-Flash scores**57 on Artificial Analysis's independent Intelligence Index** , below the current frontier band of**60 to 63** .
- **A few benchmark points could materially affect production quality and you have not run your own evals.** Most of the detailed coding, agentic, and vision results in this article are**Z.ai-reported** , with Z.ai selecting the benchmark set.
- **Your business case only works at launch pricing.** The**50% promotional discount expires 9 September 2026** . Production budgets should use the standard**$0.15 input / $0.50 output** pricing unless the promotion is explicitly extended.

## Access GLM-5.3-Flash and 500+ Models Through One API

This benchmark shows why locking an application to one model is risky. The best option changes with **modality mix, output volume, and throughput requirements**, while pricing can move within weeks.

OpenAI repriced GPT-5.6 Luna on **30 July 2026**. Z.ai launched GLM-5.3-Flash on **26 August**, with its 50% promotional pricing scheduled to expire on **9 September**. A routing layer lets you react to those changes without rebuilding the application around a new provider API.

With **Eden AI**, GLM-5.3-Flash is available through the same unified chat completions endpoint used for other supported models.

```
import requests
url = "https://api.edenai.run/v3/chat/completions"
headers = {
    "Authorization": "Bearer <your-api-key>",
    "Content-Type": "application/json"
}
payload = {
    "model": "zai/glm-5.3-flash",
    "messages": [
        {
            "role": "system",
            "content": "You are a technical assistant. Return concise Markdown."
        },
        {
            "role": "user",
            "content": "Analyze this workload and return the main technical risks."
        }
    ],
    "stream": False
}
response = requests.post(url, headers=headers, json=payload)
response.raise_for_status()
data = response.json()
print(data)
```

Switching to another **Eden AI-supported model** in this comparison is a one-line change:

`payload["model"] = "<another-supported-model-id>"`

The rest of the application can keep the same endpoint, authentication, and request structure.

For production workloads, Eden AI can also add **fallback routing**, so requests can move to another configured model if the primary route fails. Usage and cost are tracked through the same platform across providers, which makes it easier to compare actual production economics instead of maintaining separate billing logic for each API.

That matters in this price tier because the cheapest or fastest option today may not remain so after the next pricing update.
