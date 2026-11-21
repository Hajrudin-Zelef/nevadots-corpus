---
id: collect-261121-veille-ia/mistral/mistral-large-4-review-pricing-benchmarks-and-real-cost
title: "Mistral Large 4 Review: Pricing, Benchmarks and Real Cost"
domain: mistral
role: reference
task: article
actors: ["Mistral AI"]
dates: ["2026-10-06"]
keywords: ["large", "preview", "mistral", "model", "pricing", "sonnet", "cost", "input"]
source: https://www.truefoundry.com/blog/mistral-large-4-review-pricing
source_anchor: ""
source_lines: [1, 133]
section: "Mistral"
---
# Mistral Large 4 Review: Pricing, Benchmarks and Real Cost Per Task

### Built for Speed: ~10ms Latency, Even Under Load

Blazingly fast way to build, track and deploy your models!

- Handles 350+ RPS on just 1 vCPU — no tuning needed
- Production-ready with full enterprise support

## **What Mistral Large 4 is**

The Mistral Large 4, dubbed “Le Chonk,” is the newest flagship from Mistral. It is a sparse mixture-of-experts model — roughly 1.05 trillion total parameters with only about 49–52 billion active per token [VERIFY: Mistral’s announcement and docs card disagree on 49B vs 52B active] — paired with a 1.6B-parameter vision encoder, so it takes text and images in and returns text. Tool calling, structured outputs, document Q&A, batching, and built-in tools make this technology clearly fall within the “agent + pipeline” realm rather than purely conversational.

Context is the essential skill. Mistral’s model card lists a 1-million-token context window and a maximum output of 262,144 tokens, though some third-party trackers report 512K [VERIFY against Mistral’s model card before quoting]. Anyway, it’s a model for long contexts, designed for loads of paperwork and agent-based activities.

It shipped as an API public preview on October 6, 2026. The important thing about the word “preview” is that the discount factor applies to it but the weights have not yet been released.

## **Mistral Large 4 pricing, in full**

Here is the API rate card next to Claude Sonnet 5.5 and Haiku 5.5 for context — a frontier competitor and a budget baseline.

During the preview, Mistral Large 4 costs **$0.68 per million input tokens, $0.07 per million cached input tokens, and $2.09 per million output tokens**. List pricing — what you should assume for budgeting — is **$1.36 / $0.14 / $4.18**, exactly double on input and output. Batch processing takes another 50% off, and cached input is discounted up to 90% for repeated prompts [VERIFY API rates against console.mistral.ai; preview discount has no published end date].

At preview rates, Large 4 is meaningfully cheaper than Sonnet 5.5 — a third of the input price and a fifth of the output price. At list rates, the output gap narrows to roughly 2.4× and input to 1.5×. That swing is the single most important fact in this review: **the model’s cost position depends entirely on whether you are modeling the preview price or the real one.**

## **Preview pricing is a trap if you budget off it**

Preview discounts are a customer-acquisition tool. They get a model into your pipeline at a price that may not survive general availability, and Mistral has published no end date for this one. If you benchmark Large 4 against Sonnet 5.5 today at $0.68 input and ship it into production, your unit economics can change under you the day the preview ends and the price doubles.

The defensive move is to model both prices before you commit, and to put the model behind a layer where switching is a config change rather than a code change. That is the entire argument for routing model access through a gateway: the day preview pricing ends, you re-run the cost comparison and, if the math no longer works, you re-point traffic — without touching application code.

## **Real cost per task**

As with any model, the rate card is not the bill. What you pay depends on the token shape of each task. Here are four representative jobs priced on Large 4 at preview rates, against Sonnet 5.5 and Haiku 5.5.

- **Ticket triage** (600 in / 40 out): about**$0.49 per 1,000** on Large 4 preview, versus $1.60 on Sonnet and $0.08 on Haiku.
- **Support reply** (1,800 in / 350 out): about**$1.96 per 1,000** .
- **RAG answer** (12,000 in / 500 out): about**$9.21 per 1,000** .
- **Long-document analysis** (120,000 in / 1,500 out): about**$84.74 per 1,000** .

Large 4 preview sits between Haiku and Sonnet on every task — cheaper than Sonnet, well above Haiku. At list price, those Large 4 numbers roughly double, which pushes it much closer to Sonnet territory while Sonnet scores higher on general-intelligence benchmarks. That is the crux of the value question: at preview price it is a genuine bargain; at list price you are paying nearly Sonnet money for a mid-pack intelligence score, unless your workload happens to be one of its strengths.

## **Benchmarks: a security specialist, not a generalist leader**

Large 4’s benchmark profile is lopsided in an interesting way.

On the Artificial Analysis Intelligence Index it scored 38 in preview — mid-pack, well behind the top closed-source models. But on security it is genuinely strong: tied for first on the AA Cyber Index (alongside GLM 5.3 Flash) and in the top five globally on CyberGym. Coding and agentic results are respectable but not leading [VERIFY: benchmark figures are from launch-week trackers citing single sources; confirm against the original evaluations].

The practical read: if your workload is security tooling, threat analysis, or code-security review, Large 4 is worth a serious look. If you need a general reasoning leader, the intelligence index says you are paying for a specialty you may not use.

## **Where teams get Mistral Large 4 wrong**

**They lock in on preview pricing.** The most common mistake is building unit economics on the $0.68 input rate. Model the $1.36 list rate; treat the preview as a bonus, not a baseline.

**They assume one big model for everything.** Large 4’s intelligence score is mid-pack. Routing easy, high-volume work to a cheaper model (Haiku 5.5, Mistral Small) and reserving Large 4 for its strengths beats sending everything to the flagship.

**They wait on open weights without a plan.** Weights are expected around end of October 2026, but under a custom license, not Apache 2.0. Read the license before assuming self-hosting is free for commercial use.

**They compare on sticker price, not task cost.** A model’s input/output ratio changes the ranking. Large 4’s output is 3× its input; generation-heavy workloads cost more than the input rate suggests.

## **How to run Mistral Large 4 through TrueFoundry**

The recurring theme — model both prices, keep switching cheap, measure real cost — is exactly what a gateway is for. In the TrueFoundry AI Gateway, Mistral is a provider you add alongside the others, and every call is metered the same way.

### **Add Mistral in a couple of steps**

From the dashboard, go to AI Gateway > Models and select Mistral AI.

Add your Mistral account with its API key, name it, and set collaborators so the right teams get access.

Pick Large 4 (or any Mistral model) from the list, and you can call it immediately over an OpenAI-compatible API — from the Playground or your own application.

### **Why this matters for a preview-priced model**

Because Large 4 reaches your app through the same OpenAI-compatible endpoint as every other model, three things become config, not code:

- **Switching providers** when preview pricing ends — re-point the virtual model, no redeploy.
- **Fallback** to another model if Mistral’s preview endpoint is rate-limited or degraded.
- **Cost tracking** across providers, so you can see Large 4’s real spend next to Claude and GPT in one dashboard, attributed by model, team, and request. Public cost uses TrueFoundry’s open-source pricing catalog; private cost lets you enter Mistral’s preview or negotiated rates directly, so the tracked number matches your actual bill.

## **A worked example: routing to Large 4 for security review**

A security team runs an automated code-security review: 40,000 PR-diff tokens in, 1,200 tokens of findings out, 2,000 reviews a month.

On Large 4 at preview rates, that is about **(40,000 × $0.68 + 1,200 × $2.09) per million**, roughly **$0.0298 per review** — about **$60/month**. At list rates it is roughly **$0.0595 per review**, or **$119/month**. On Sonnet 5.5 the same job is about **$0.092 per review**, or **$184/month**.

For this workload — where Large 4’s security strength is the point — preview pricing makes it a clear win, and even at list it undercuts Sonnet. Route the security reviews to Large 4, keep general chat on a cheaper model, and let the gateway’s metrics confirm the blended cost. If the preview ends and the economics shift, you change one routing rule.

## **Gotchas worth knowing**

- **Preview rates are temporary.** Budget at list ($1.36 / $4.18). Treat the discount as upside.
- **Specs are still settling.** Active-parameter count and context window differ across Mistral’s own announcement, its docs card, and third-party trackers. Confirm against the official model card before quoting hard numbers.
- **Open weights come with a custom license.** Not Apache 2.0. Self-hosting for commercial use may carry terms; read them.
- **It’s a specialist.** Strong on security, mid-pack on general intelligence. Match it to the work.
- **Output is 3× input.** Generation-heavy tasks cost more than the input rate implies; watch output tokens.

## **Related reading**

## **Conclusion**

Mistral Large 4 is a sharp, specialized model at an attractive preview price — a genuine security standout wrapped in a long-context, multimodal, tool-calling package. The two things to keep your eye on are the preview discount, which doubles when it ends, and the mid-pack general-intelligence score, which means it earns its place on specific workloads rather than as a default.

The way to use it well is the way you use any new model: put it behind a gateway, send it the work it is good at, and measure real cost per task against the alternatives. Then preview-to-list pricing, open-weight timing, and even a better model next quarter all become routing decisions instead of rewrites.

TrueFoundry AI Gateway delivers ~3–4 ms latency, handles 350+ RPS on 1 vCPU, scales horizontally with ease, and is production-ready, while LiteLLM suffers from high latency, struggles beyond moderate RPS, lacks built-in scaling, and is best for light or prototype workloads.

## Recent Blogs

## Frequently asked questions

### How much does Mistral Large 4 cost?

During the API public preview, $0.68 per million input tokens and $2.09 per million output tokens, with cached input at $0.07. List pricing is $1.36 input and $4.18 output. Batch processing is 50% off and cached input is discounted up to 90%.

### Is Mistral Large 4 cheaper than Claude Sonnet 5.5?

At preview rates, yes — roughly a third of the input price and a fifth of the output price. At list rates the gap narrows to about 1.5× on input and 2.4× on output, so the advantage depends heavily on whether the preview discount is still active.

### Is Mistral Large 4 open source?

Not yet. Mistral has committed to releasing open weights around the end of October 2026, but under a custom Mistral license rather than the Apache 2.0 license used for Mistral Large 3.

### What is Mistral Large 4 good at?

Security. It is tied for first on the Artificial Analysis Cyber Index and ranks in the top five on CyberGym. On general-intelligence benchmarks it is mid-pack, so it is best matched to security and specialized workloads rather than used as a general reasoning leader.

### How do I track Mistral Large 4 cost next to other models?

Route it through the TrueFoundry AI Gateway. It meters every call and attributes cost by model, team, user, and request, so Mistral spend appears in the same dashboard as Claude, OpenAI, and Gemini — with public or private (your preview-rate) pricing.

### Can I run Mistral Large 4 in my own cloud?

Through the gateway, yes — the data plane runs in your VPC or on-prem, so prompts and usage stay in your environment. Once Mistral’s open weights ship, you can also self-host the model itself, subject to its license.
