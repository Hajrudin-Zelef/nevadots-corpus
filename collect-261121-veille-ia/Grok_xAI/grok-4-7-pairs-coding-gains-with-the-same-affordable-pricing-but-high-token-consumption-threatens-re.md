---
id: collect-261121-veille-ia/grok-xai/grok-4-7-pairs-coding-gains-with-the-same-affordable-pricing-but-high-token-consumption-threatens-re
title: "Grok 4.7 pairs coding gains with the same affordable pricing — but high token consumption threatens real-world ROI"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: []
keywords: ["grok", "token", "fast", "flash", "openai", "coding", "cursor", "input"]
source: https://venturebeat.com/technology/grok-4-7-pairs-coding-gains-with-the-same-affordable-pricing-but-high-token-consumption-threatens-real-world-roi
source_anchor: ""
source_lines: [1, 100]
section: "Grok_xAI"
---
SpaceXAI earlier today released Grok 4.7, its latest model for coding and professional knowledge work, with improved performance on benchmarks (especially coding, or Terminal Bench), a longer reinforcement-learning run and a new safeguard stack aimed at making the system more reliable on tasks that can stretch across hours.

The most consequential part of the release for developers may be what did not change: price. Grok 4.7 starts at $2/$6 USD per 1 million input/output tokens, the same rates as Grok 4.6, released only a little more than a month prior.

But for engineering teams deciding which model to route agentic workloads through, the more important number may not be the token rate at all. It is the cost of getting a task successfully finished after reasoning tokens, tool calls and long-running agent loops are counted. New independent testing of Grok 4.7 makes that distinction particularly important.

SpaceXAI is also offering a fast variant at an increased token processing speed for 2X the price ($4/$12), though it has not published a tokens-per-second figure and independent throughput measurements were not yet available at publication time.

## **VentureBeat Frontier Model Per 1M Token API Cost Comparison Table Snapshot - Late 2026**

| **Model** | **Input ($/1M)** | **Output ($/1M)** | **Total ($/1M)** | **Source** | 
| Muse Spark 1.2 / 1.3 Contributor | $0.10 | $0.20 | $0.30 | Meta | 
| MiMo-V2.5 Flash | $0.10 | $0.30 | $0.40 | Xiaomi | 
| DeepSeek-V4.1-Flash — off-peak | $0.15 | $0.60 | $0.75 | DeepSeek | 
| GPT-5.6 Luna | $0.20 | $1.20 | $1.40 | OpenAI | 
| MiniMax-M3 | $0.30 | $1.20 | $1.50 | MiniMax | 
| LongCat-2.0 — limited-time promo | $0.30 | $1.20 | $1.50 | LongCat | 
| DeepSeek-V4.1-Flash — peak hours | $0.30 | $1.20 | $1.50 | DeepSeek | 
| MiMo-V2.5 | $0.40 | $2.00 | $2.40 | Xiaomi | 
| DeepSeek-V4-Pro — off-peak | $0.66 | $1.98 | $2.64 | DeepSeek | 
| LongCat-2.0 — standard | $0.75 | $2.95 | $3.70 | LongCat | 
| MiMo-V2.5 Pro (≤256K) | $1.00 | $3.00 | $4.00 | Xiaomi | 
| Gemini 3.7 Flash — through Dec. 31, 2026 | $0.75 | $3.75 | $4.50 |  | 
| Gemini 3.8 Flash — through Dec. 31, 2026 | $0.75 | $3.75 | $4.50 |  | 
| DeepSeek-V4-Pro — peak hours | $1.32 | $3.96 | $5.28 | DeepSeek | 
| Muse Spark 1.1 / 1.2 / 1.3 | $1.25 | $4.25 | $5.50 | Meta | 
| GLM-5.3 | $1.40 | $4.40 | $5.80 | Z.AI | 
| **Grok 4.7 — ≤200K prompt tokens** | **$2.00** | **$6.00** | **$8.00** | **xAI** | 
| MiMo-V2.5 Pro (>256K) | $2.00 | $6.00 | $8.00 | Xiaomi | 
| Qwen3.8-Max | $2.00 | $6.00 | $8.00 | QwenCloud | 
| Gemini 3.7 Flash — starting Jan. 1, 2027 | $1.50 | $7.50 | $9.00 |  | 
| Gemini 3.8 Flash — starting Jan. 1, 2027 | $1.50 | $7.50 | $9.00 |  | 
| GPT-5.6 Terra | $2.00 | $12.00 | $14.00 | OpenAI | 
| **Grok 4.7 — >200K prompt tokens** | **$4.00** | **$12.00** | **$16.00** | **xAI** | 
| Grok 4.7 Fast (Cursor) — ≤256K input | $4.00 | $12.00 | $16.00 | Cursor | 
| GPT-5.4 | $2.50 | $15.00 | $17.50 | OpenAI | 
| Kimi K3 | $3.00 | $15.00 | $18.00 | Moonshot AI | 
| **Grok 4.7 Fast (Cursor) — >256K input, up to 500K** | **$6.00** | **$18.00** | **$24.00** | **Cursor** | 
| Claude Opus 5 | $5.00 | $25.00 | $30.00 | Anthropic | 
| Sakana Fugu Ultra (≤272K) | $5.00 | $30.00 | $35.00 | Sakana AI | 
| GPT-5.6 Sol — Standard mode | $5.00 | $30.00 | $35.00 | OpenAI | 
| Claude Fable 5 / Claude Mythos 5 | $10.00 | $50.00 | $60.00 | Anthropic | 
| Claude Fable 5.1 / Claude Mythos 5.1 | $10.00 | $50.00 | $60.00 | Anthropic | 
| GPT-6 Astra — Standard mode | $10.00 | $50.00 | $60.00 | OpenAI | 
| GPT-5.6 Sol — Fast mode | $10.00 | $60.00 | $70.00 | OpenAI | 
| GPT-6 Astra — Fast mode | $20.00 | $100.00 | $120.00 | OpenAI | 

The AI coding platform Cursor, which was recently acquired by SpaceXAI, also offers a Fast tier for Grok 4.7 at twice the standard token price — $4/$12 per 1 million input/output tokens — and makes Fast the default for Pro and higher plans.

Cursor does not publish a tokens-per-second figure or claim an exact 2× throughput increase. For inputs above 256K tokens, Cursor charges $4/$12 for standard Grok 4.7 and $6/$18 for Fast, up to the model’s 500K maximum context window.

The model is available now in Cursor, Grok Build and the Grok API, as well as through third-party coding harnesses, model routers and cloud platforms, according to SpaceXAI. GitHub is separately rolling Grok 4.7 out gradually across Copilot Pro, Pro+, Max, Business and Enterprise, with support spanning VS Code, Visual Studio, Copilot CLI, GitHub’s cloud agent, JetBrains, Xcode and Eclipse.

That combination — higher claimed capability without a base token-price increase — is the core of SpaceXAI’s pitch. In its launch materials, the company describes Grok 4.7 as its most capable model yet for coding and knowledge work, saying it can “work longer on difficult tasks” and verify its work more carefully.

## **A larger model trained for work that takes hours**

SpaceXAI says Grok 4.7 uses a new, larger base model than Grok 4.6 and underwent a longer reinforcement-learning run using a harder distribution of tasks weighted toward problems that take many hours to complete. The company also says it improved long-context management and trained the model to work even better with SpaceXAI's recently released, agentic AI focused Grok Bot harness.

That emphasis is important for enterprises evaluating models as agents rather than chat interfaces. A model that can stay on task through a lengthy coding session, assemble a document, navigate a terminal or complete a multistep business workflow has different operational requirements from one optimized primarily for isolated prompts.

Grok 4.7 notches some sizable gains over its predecessor on common third-party benchmarks, but it still lags the latest models from U.S. rivals OpenAI, Anthropic, and even Google.

| **Benchmark** | **Grok 4.6** | **Grok 4.7** | **Improvement** | 
| Terminal-Bench 4.0 | 20.3% | 38.0% | +17.7 percentage points | 
| EEBench | 53.0% | 64.0% | +11.0 points | 
| HealthBench Professional | 48.5% | 56.7% | +8.2 points | 
| CursorBench 4.0 | 40.4% | 46.3% | +5.9 points | 
| DeepSWE v1.1 | 65.2% | 71.0% | +5.8 points | 
| Harvey Legal Agent Benchmark | 15.8% | 19.6% | +3.8 points | 
| AA Briefcase v1.1 | 1,546 | 1,657 | +111 Elo | 
| GDPval | 1,605 | 1,695 | +90 Elo | 

And, in the words of developer and writer Dan McAteer's post on X, is "horrendous" on Terminal-Bench, the terminal use benchmark on which it saw the largest improvement overall over its predecessor.

Third-party AI model evaluation firm Artificial Analysis measures Grok 4.7 at its "xHigh" reasoning effort setting at roughly 26% on the benchmark, versus 59.6% for OpenAI’s GPT-6 Astra xHigh and about 49% for Anthropic’s Claude Opus 5 at max effort.

On CursorBench 4.0, Grok 4.7 at xHigh effort scores 46.3%, up from 40.4% for Grok 4.6 High. GPT-5.6 Sol Max scores 41.7% in SpaceXAI’s comparison, while Fable 5.1 Max reaches 51.8%. On DeepSWE v1.1, Grok 4.7 reaches 71.0%, compared with 65.2% for Grok 4.6.

Still, others were impressed with the release. Popular AI news-focused X user @haider1 called the CursorBench numbers “absurd actually,” pointing to Grok 4.7 xHigh at 46.3% for about $6.01 per task, versus 46.8% at $7.05 for Fable 5.1 Medium and 41.7% at $8.23 for GPT-5.6 Sol Max in the leaderboard shown in his post. He also highlighted the improvement over Grok 4.6 at roughly comparable cost

## **Cheap tokens do not necessarily mean the cheapest task**

Artificial Analysis testing now adds an important qualification to the sticker-price story. Grok 4.7 xHigh used roughly **81,000 output tokens per Intelligence Index task, versus 36,000 for Grok 4.6 High** and 27,000 for GPT-6 Astra Max. 

Artificial Analysis says that represents **125% more output-token use than Grok 4.6 and 196% more than GPT-6 Astra.** 

Artificial Analysis has also now populated a dollar-denominated cost-per-task result: about $3.74 per Intelligence Index task for Grok 4.7 xHigh and $2.73 for Grok 4.7 High. Its methodology incorporates input, cache, reasoning and answer-token costs rather than simply comparing API list prices. For context, the same comparison currently puts GPT-5.6 Sol Max at about $1.99 per task, despite its substantially higher $4/$20 input/output list pricing.

That reverses the intuitive reading of the API price sheet. **A model charging less per token can still be more expensive on a finished workload** if it needs substantially more reasoning tokens to get there. And the difference becomes material quickly at enterprise scale. 

At the published $2/$6 rates, a workload consuming 10 billion input and 2 billion output tokens each month would produce about $32,000 in monthly model charges, or $384,000 annually. At 50 billion input and 10 billion output tokens, the same rates imply about $160,000 a month, or $1.92 million a year, before platform-specific fees. Those are illustrative volume calculations, not estimates of a typical Grok deployment, but they show why token efficiency compounds into an infrastructure-budget issue at production scale.

For a lead engineer, the Monday-morning takeaway is therefore straightforward: benchmark Grok 4.7 on representative internal jobs and track cost per successful completion, token consumption, latency, retries and required human intervention alongside the API rate.

## Safety gets a new stack

SpaceXAI also says Grok 4.7 introduces an entirely new safeguard stack, reporting a 62.4% result on LatchBio’s biosafety benchmark and saying the model allowed only 3.3% of risky dual-use prompts through its HackerBench v0.3 evaluation. Those are company-reported safety results and should be treated as such pending broader independent testing.

For engineering teams evaluating Grok 4.7 this quarter, the practical test is therefore not whether its $2/$6 API rate undercuts competitors on paper. It is whether the model completes the organization’s actual coding and knowledge-work jobs with fewer dollars, retries and engineer interventions. Grok 4.7’s benchmark gains give teams a reason to run that evaluation; production traces will determine whether its low token price translates into a lower bill for finished work.
