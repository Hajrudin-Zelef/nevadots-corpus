---
id: collect-261121-veille-ia/grok-xai/grok-4-6-features-benchmarks-pricing-comparisons
title: "Grok 4.6: Features, Benchmarks, Pricing & Comparisons"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: []
keywords: ["grok", "spacexai", "reasoning", "model", "pricing", "tokens", "training", "tasks"]
source: https://www.datacamp.com/blog/grok-4-6
source_anchor: ""
source_lines: [1, 209]
section: "Grok_xAI"
---
## What Is Grok 4.6?

Grok 4.6 is SpaceXAI's proprietary reasoning model. The API and Cursor do not expose the same context limit, and the higher long-context rate begins well before the API limit. The table puts those constraints beside input types, tools, and the knowledge cutoff.

| **Spec** | **Grok 4.6** | 
| Context window | 500,000 tokens (API); 256,000 tokens inside Cursor | 
| Input / output | Text and images in; text out | 
| Reasoning effort | Low, medium, high (default), xhigh | 
| Knowledge cutoff | February 1, 2026 | 
| Tools | Function calling, web search, X search, code execution, collections search (RAG), remote MCP | 
| Standard pricing | $2 / million input tokens, $0.50 cached, $6 / million output | 
| Long-context pricing | $4 / $1 / $12 once a prompt reaches 200,000 tokens, applied to the whole request | 

 
SpaceXAI still doesn't publish a parameter count for either model. The 1.5 trillion figure in some reports did not come from SpaceXAI, so it is not a confirmed spec.

## What's New in Grok 4.6?

SpaceXAI says this isn't a new model trained from scratch. Instead, it gave Grok 4.5 more training, with extra attention on AI agent tasks. The reported changes fall into longer agent runs, visual builds, and post-training.

### Longer agent tasks

SpaceXAI says Grok 4.6 stays on track during long tasks with several steps, such as research, working across a codebase, and building an application. The model reportedly checks its work more often instead of doing everything in one pass.

That matches a wider change in model testing. A good first answer matters less if the model forgets earlier decisions after several tool calls. GitHub's internal testing also found that Grok 4.6 kept reasoning and using tools over longer tasks.

The API has features tied to that claim. Grok 4.6 can stream reasoning summaries, which Grok 4.5 does not expose, and xAI recommends context compaction with a sticky `prompt_cache_key` for long agent loops. Compaction shortens earlier history into one item, but it still uses tokens, and the request must fit the context window before compaction runs.

### Visual and interactive web development

SpaceXAI and Cursor both report better first attempts on visual projects. In one outside test, Grok 4.6 rebuilt an old iPod Mini product page as a 3D site with a working color wheel and scroll animation. The demo worked, but one test cannot prove the wider claim.

### How Grok 4.6 was post-trained

If you only care about benchmarks and pricing, skip this subsection. It explains SpaceXAI's account of where the gain came from.

SpaceXAI ran more training than it did for Grok 4.5. It used AI-made reasoning examples and engineering examples, removed poor examples with model-based checks, and then used reinforcement learning for coding, office tasks, web development, kernel tuning, and computer design. SpaceXAI also says Grok 4.5 created new training examples across different reasoning settings and subjects. The company attributes the gain to more training and stricter data selection, not a new architecture.

Outside teams cannot check those details, and SpaceXAI has not shared enough information to repeat the training. This is the company's account of what changed, not a result checked by someone else.

## Grok 4.6 Benchmarks: Official and Independent Results

SpaceXAI's launch table shows where scores rose, but its competitor scores are "the best of self-reported or publicly available results." One team did not run every model under the same setup. Reasoning effort is included below because it affects the score.

| Benchmark | Grok 4.6 (high) | Grok 4.5 (high) | GPT-5.6 Sol (max) | Claude Fable 5 (max) | 
|---|---|---|---|---|
| AA Intelligence Index | 61 | 56 | 61 | 62 | 
| GDPVal-AA v2 (Elo) | 1,753 | 1,526 | 1,728 | 1,741 | 
| DeepSWE 1.1 | 65.9% | 54.0% | 73.0% | 70.0% | 
| Terminal-Bench 3.0 | 26.0% | 15.7% | 34.6% | 34.1% | 
| APEX-Agents | 57.5% | 47.1% | 56.7% | 59.2% | 
| CursorBench v3.2 | 69.9% | 66.7% | 67.2% | 70.5% | 

 
Grok 4.6 improves over Grok 4.5 throughout the table. Sol's largest gaps appear on DeepSWE and Terminal-Bench, while Fable leads 3 rows. Even SpaceXAI's own table does not point to one winner.

### Independent benchmark results

Artificial Analysis ran its own Intelligence Index across six models in this comparison. Its results are close to SpaceXAI's numbers, but not the same.

Grok 4.6 versus five frontier rivals. Image by Author.

The chart places Grok level with Sol on intelligence, but much lower on blended price. Artificial Analysis calculates that price with a 7:2:1 mix of cached input, uncached input, and output. Cost per completed index task shows the same gap: $0.84 for Grok, $1.23 for Sol, and $2.34 for Opus 5.

Time to first answer token rose from 14.62 seconds on Grok 4.5 to 40.44 seconds, while cost per task increased from $0.36 to $0.84. Grok 4.6 used about 20% more output tokens on the same tests, so an unchanged list price does not mean an unchanged task cost. Cost per finished task gives a better view than API price cards alone because it includes some of the extra reasoning used to finish the work.

Compared with GPT-5.6 Sol and Claude Sonnet 5, Grok 4.6 sends its first answer token sooner. Sol at max effort takes 213.52 seconds, and Sonnet 5 takes 184.48, compared with Grok's 40.44. Grok only looks slow beside lower-latency models such as Gemini 3.7 Flash.

### How reliable are these benchmark comparisons?

They are useful for finding relative strengths, but not a reliable cross-model ranking. Reasoning effort changes score and response time: GPT-5.6 Sol scored 57 at high effort and 61 at max. Benchmark names can also refer to different versions, so xAI's Terminal-Bench 3.0 and Artificial Analysis's Terminal-Bench 2.1 are not comparable.

## What Changed From Grok 4.5?

For Grok 4.5 users, the decision is not whether 4.6 is "better" in the abstract. It is whether the higher score offsets a different latency and token-use profile. The table puts those changes on the same baseline.

| **Measure** | **Grok 4.5 (high)** | **Grok 4.6 (high)** | 
| AA Intelligence Index | 56 | 61 | 
| Cost per index task | $0.36 | $0.84 | 
| Time to first answer token | 14.62s | 40.44s | 
| Cached input price | $0.30 / M | $0.50 / M  | 
| Output tokens used to run the index | 60M | 72M | 
| Base input / output price | $2 / $6 | $2 / $6 (unchanged) | 

 
However, "same price" only describes the base list rate. One more change matters for existing Grok 4.5 users: cached input rose 67%, from $0.30 to $0.50 per million tokens. That higher cache rate adds to the gap between the price card and the cost of a finished task.

## Grok 4.6 vs the Competition

Now that we have settled the comparison with its own predecessor, let’s see how Grok 4.6 compares with other state-of-the-art models from OpenAI and Anthropic.

### Grok 4.6 vs GPT-5.6 Sol

The index tie hides a clear split. Sol leads Grok by 7.1 points on DeepSWE and 8.6 points on Terminal-Bench, making terminal-heavy coding its strongest case. Grok scores higher on the other three task rows.

OpenAI's own tests extend that terminal result to web browsing and computer use. They remain vendor-selected evidence rather than a controlled comparison, but point in the same direction: Sol's strongest case is work that depends on a terminal, a browser, or repeated tool calls.

OpenAI also previewed an Ultrafast mode for Sol that it says runs up to 14 times faster. It has no published price yet, so it does not belong in the cost comparison.

Grok 4.6 starts at $2 input and $6 output per million tokens. Sol's standard rate is $5 input and $30 output, five times Grok's output price. Above 272,000 tokens, Sol doubles the input price and raises output to $45.

That does not make every Grok task five times cheaper. Reasoning-token use, cache hits, and the number of turns still change the final bill. Grok's price advantage is clearest when token use is predictable. For terminal-heavy work, Sol's higher benchmark scores may matter more than the list-price gap.

For a more detailed comparison, you can consult our Grok 4.6 vs GPT-5.6 Sol guide.

### Grok 4.6 vs. Claude Sonnet 5

Sonnet 5 and Grok 4.6 both start at $2 per million input tokens, which makes their prices easy to compare. Sonnet 5 has a 1 million token context window by default, twice Grok's 500,000, with no higher rate for long prompts. Output costs $10 per million against Grok's $6.

Two days before Grok 4.6 launched, Anthropic made the $2/$10 pricing permanent and dropped a planned increase to $3/$15. This matters when comparing against an older price sheet, including an older one of ours.

On the Artificial Analysis index, Sonnet 5 scores 55 at max effort, six points below Grok 4.6. It used 300 million output tokens for the tests against Grok's 72 million, putting its cost per task at $1.72. Its tokenizer produces roughly 30% more tokens than Sonnet 4.6 for the same text. That makes a price comparison between those two Sonnet versions harder. Sonnet 5 isn't Anthropic's top model, either.

### Grok 4.6 vs Claude Opus 5 and Fable 5

Opus 5, at $5 input and $25 output, tops the index at 63. Fable 5, at $10 input and $50 output, scores 62. Fable's index score also needs a note. Artificial Analysis recorded safety fallbacks on some harder prompts, so the score reflects the model as people can use it, not an unrestricted version that Anthropic does not offer.

On Artificial Analysis's AA-Briefcase benchmark for long agent tasks, Grok 4.6 finished in about 53 turns and used 0.5 billion input tokens. Opus 5 took about 103 turns and 2 billion tokens. This does not show that Grok is the stronger model overall. It shows that Grok used fewer steps and fewer input tokens on this test set, while Claude led on the other tests covered above.

## Grok 4.6 Pricing

The 200,000-token prompt threshold is the part I would watch: it moves every token in the request to the higher tier. The table also includes priority and tool charges.

| **Item** | **Rate** | 
| Input tokens, under 200K prompt | $2.00 / million | 
| Cached input, under 200K prompt | $0.50 / million | 
| Output tokens, under 200K prompt | $6.00 / million | 
| Input / cached / output (over 200K prompt) | $4.00 / $1.00 / $12.00 | 
| Fast or priority processing | 2x standard rate | 
| Web search, X search, code execution | $5 per 1,000 calls | 

 
Tool charges are separate from token charges. A request that searches the web and then runs code can incur both tool fees before the final text is billed. There is no separate `grok-4.6-fast` model name. The direct API has a priority option billed at 2x when the response confirms priority use. Cursor shows a separate "Grok 4.6 (Fast)" choice at the same 2x rate.


Grok 4.6 Fast selected inside Cursor. Image by Author.

### How Can I Access Grok 4.6?

Grok 4.6 is available first-party via: 

- the SpaceXAI API
- Cursor
- Grok Build
- Third-party gateways (OpenRouter, Vercel, Cloudflare, Google Cloud Vertex AI)
- Other coding tools (GitHub Copilot, VS Code, JetBrains, Xcode, and Eclipse)

The rollout covers Pro, Pro+, Max, Business, and Enterprise plans. Business and Enterprise admins must turn the model on because it is off by default. Grok 4.6 is not supported on SpaceXAI's Batch API, so there is no discounted batch option.

One enterprise detail sits outside model availability. Responses are stateful by default, and SpaceXAI says the history is stored for 30 days. Under Zero Data Retention, teams cannot use stored response chains or deferred completions; xAI points to encrypted reasoning replay and WebSocket Responses instead. Each response includes a header showing whether ZDR was active.

## Grok 4.6’s Launch Week: Models, Copilot, and Cursor

What stood out to me that week was how quickly model launches and distribution deals clustered around Grok 4.6.

Grok 4.6's launch week, beyond benchmarks. Image by Author.

Grok Bot set the tone before the model launch. The early-beta cloud agent was bundled with SuperGrok Heavy at $300 a month or Cursor Ultra at $200 a month rather than sold on its own, tying access to premium plans. If you want to learn how to use the tool, I recommend reading our Grok Bot tutorial. Grok 4.6 then carried the same agent focus into the API, Cursor, and Grok Build.

Google's Gemini 3.7 Flash launch and OpenAI's GPT-5.6 Sol Ultrafast preview made the week more crowded. At the same time, the Copilot rollout and Cursor joining SpaceX widened Grok's distribution. Cursor traced the process back to its SpaceXAI partnership earlier that year and framed the move around access to SpaceX's GPUs rather than model scores. Its post says that access will help it build models at a lower cost.

My read of that week is that SpaceXAI wants Grok inside the tools people already use. Grok 4.5 was jointly trained with Cursor, and Grok 4.6 moved into Copilot almost immediately. That makes partner distribution part of the release, not an afterthought.

## When Should You Use Grok 4.6?

Grok 4.6 fits work where API price and long agent tasks matter more than first-token latency. It is a weaker fit when Sol's higher terminal scores or Sonnet 5's larger context window match the task more closely.

Grok 4.6 fits when:

- **API price is a primary constraint** . It undercuts Sol, Opus 5, and Fable 5 on list price and cost per task
- **Work runs as long, multi-step agent tasks** , where its turn efficiency (fewer turns and input tokens than Opus 5 on AA-Briefcase) pays off
- **The task is knowledge work or legal reasoning** , where it leads the benchmark table
- **First-token latency doesn't matter** . A slow start is noise over a long run, but the whole experience in interactive chat

There might be better alternatives available when:

- The work is terminal-heavy coding → GPT-5.6 Sol (higher DeepSWE and Terminal-Bench scores)
- The task needs a large context window → Claude Sonnet 5 (1M context, no long-prompt surcharge)
- You want the lowest latency for interactive use → Gemini 3.7 Flash
- You're buying purely on top-end intelligence → Opus 5 (63) or Fable 5 (62) top the index

## Final Thoughts on Grok 4.6

Grok 4.6 lands in an awkward middle. It moves up five index points and stays below Sol and Opus 5 on listed prices, yet starts more slowly and costs more per measured task than Grok 4.5. "Same price, better model" misses half the release.

The part I still want to see is a multi-hour run where the codebase, tools, and instructions keep changing. Cursor and GitHub Copilot now provide that setting. If correction and failure rates stay low there, the agent-training claim has evidence beyond a fixed test; if not, the five-point gain remains just that.

For readers building against the xAI API, our Grok 4 API tutorial covers the Python client and request flow. And to see Grok 4.6 driving SpaceXAI's coding agent against Claude Opus 5, read Grok Build vs. Claude Code.

Most readers still know the lab behind Grok as xAI. SpaceXAI released Grok 4.6 on August 12, 2026, just over a month after Grok 4.5, with a focus on coding, visual web work, and agent tasks that run for longer.

The launch data does not support one simple ranking. Grok leads some of SpaceXAI's chosen tests, while GPT-5.6 Sol and Claude Fable 5 lead others. Artificial Analysis places Grok beside Sol at a lower blended price, but the upgrade also starts answering later and costs more per measured task than Grok 4.5.

This article follows that trade through the model's specs, pricing, access, and direct comparisons with Sol and Claude. Our Grok 4.5 coverage focused on low token use. Here, the question is whether a higher score changes the actual bill once cached input, reasoning tokens, and extra output are counted.

To get started with the model, make sure to read our Grok 4.6 API tutorial. We also compared it against other flagship models; read our verdict in our Grok 4.6 vs Claude Opus 5 and Grok 4.6 vs GPT-5.6 Sol guides.

## TL;DR: Grok 4.6

My short read is that the five-point gain matters less on its own than the launch page suggests. Price behavior and task type decide whether it changes the model choice.

At

`high`effort, Grok 4.6 scores 61 on the Artificial Analysis Intelligence Index, numerically equal to Sol at`max`and two points behind Opus 5 at`max`.
Base input and output rates stay at $2 and $6 per million tokens, but cached input, first-token delay, and measured task cost all rise from Grok 4.5.

In AA-Briefcase, Grok used fewer turns and input tokens than Opus 5; Sol leads the terminal tests, while Sonnet 5 offers twice the context without a long-prompt surcharge.

The comparisons below keep provider claims separate from Artificial Analysis results. That matters because a model can change position when the effort level or test setup changes.
