---
id: collect-261121-veille-ia/gpt-openai/gpt-6-astra-pricing-the-cache-break-even-verified
title: "GPT-6 Astra Pricing: The Cache Break-Even, Verified"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["astra", "gpt-6", "pricing", "openai", "model", "more", "gpt-5", "cache"]
source: https://www.truefoundry.com/blog/gpt-6-astra
source_anchor: ""
source_lines: [1, 164]
section: "GPT_OpenAI"
---
# GPT-6 Astra Pricing: Where Caching Still Pays at $10 Input

### Built for Speed: ~10ms Latency, Even Under Load

Blazingly fast way to build, track and deploy your models!

- Handles 350+ RPS on just 1 vCPU — no tuning needed
- Production-ready with full enterprise support

## **What OpenAI shipped**

GPT-6 Astra landed on 3 September 2026. The API string is gpt-6-astra — no dated snapshot, no separate alias; the model page lists gpt-6-astra as both the snapshot and the alias.

The specifications, from OpenAI’s model documentation:

Three things in the fine print matter more than the headline.

First, **reasoning.effort no longer accepts none**. GPT-5.6 Sol did. If you have a cheap fast path pinned at none, it will not port.

Second, **temperature, top_p and logprobs are gone**. The changelog states Astra “does not support custom temperature or top_p values or log probabilities.” Anything in your stack that sets sampling parameters needs auditing before you switch.

Third, **tool calling requires the Responses API**. If you call tools through Chat Completions today, that is a migration, not a model swap.

[SCREENSHOT: OpenAI Platform — the gpt-6-astra model page showing context window, max output and per-token pricing]

## **What changed versus GPT-5.6 Sol**

The context window and output limit did not move. The price did.

Sources: GPT-6 Astra model docs, GPT-5.6 Sol model docs, September 2026. Note that OpenAI describes GPT-5.6 Sol’s rates as promotional pricing available at least through 21 November 2026, so the ratio may not hold past that date.

Every rate moved by exactly the same factor. That turns out to matter for the caching question below.

Batch and Flex remain at 50% of Standard. Fast mode, which OpenAI says delivers up to 2x the speed of Standard processing, is priced at 2x the applicable rates — so Fast mode Astra is $20 input and $100 output.

## **The benchmarks, read honestly**

OpenAI publishes a large head-to-head table in the launch post. Here is the part worth reproducing, and then the part most coverage skips.

Source: GPT-6 Astra launch post, September 2026. Scores are the maximum at any reasoning effort.

**Where Astra clearly leads.** Long-horizon agentic work. Terminal-Bench 4.0 is up more than twenty points over Sol. Terminal-Bench Science nearly triples. AutomationBench more than doubles. Long-context retrieval at 512K-1M goes from 73.8% to 96.3%. These are the workloads the model was built for and the gap is not subtle.

**Where the gap narrows to noise.** Single-shot knowledge and reasoning. GPQA Diamond moves 94.6% to 96.0%. DeepSWE moves 72.7% to 74.1%. If your traffic is questions with answers rather than tasks with steps, you are paying 2.5x for roughly a point.

**Where Astra does not win.** On the two third-party composite indices OpenAI chose to include, Astra is not first. Artificial Analysis Intelligence Index puts Claude Fable 5.1 at 65.7 against Astra’s 61.2. The Coding Agent Index puts Claude Opus 5 at 68.1 against Astra’s 67.0. OpenAI published both numbers, which is to its credit, and they are the honest counterweight to the agentic sweep above.

**One thing that complicates the price story.** OpenAI’s per-task cost estimates sometimes run *lower* than Sol despite the 2.5x rates, because Astra finishes in fewer tokens. The launch post claims roughly 9% lower estimated API cost per task on Terminal-Bench 4.0 versus Sol, about 27% lower on Terminal-Bench Science at a lower-cost setting, and about 43% lower on BenchCAD. These are OpenAI’s own estimates in specific configurations, not independent reproductions, and they will not generalise to your traffic. But they do mean “2.5x more expensive” is the wrong summary for agentic work. Measure tokens per completed task, not price per token.

## **What GPT-6 pricing means for caching**

TrueFoundry ran the cache break-even maths on GPT-5.6 when cache writes first became billable. The same question applies here, and the answer is more interesting than it looks.

Astra’s cache multipliers are identical to GPT-5.6’s. The model docs state cache writes are billed at 1.25x the uncached input rate, and the published cached-input rate of $1.00 against a $10.00 input rate is exactly 0.10x. So:

- Input: P
- Cache write: 1.25 x P
- Cache read: 0.10 x P

Let w be the share of requests against a shared prefix that are cache writes rather than reads. Caching breaks even when:

1.25w + 0.10(1 - w) = 1

1.15w = 0.90

w = 78.3%

**The break-even is 78.3%, unchanged from GPT-5.6.** P cancels out, which is why the 2.5x price increase moves nothing. Caching stops paying only when more than roughly 78% of your requests against a given prefix are writes — that is, when your shared context survives fewer than about 1.3 requests on average before it changes or expires. Almost no real agent workload lives there.

What the price increase does change is the size of the prize. Take a 32K-token shared prefix reused across a 25-request session — one write, twenty-four reads:

The percentage is identical. The dollars are 2.5x larger. **Caching was always worth doing; on Astra, not doing it costs two and a half times as much.**

The failure mode is unchanged too: context that churns fast. If your agent rebuilds its system prompt or tool schema every few turns, you pay $12.50 per million to write a cache you barely read. That is now a $12.50 mistake instead of a $5.00 one.

## **The 272K cliff nobody mentions**

The advertised GPT-6 context window is 1,050,000 tokens. The billing does not scale smoothly across it.

From the model docs: “Prompts with more than 272K input tokens are priced at 2x input and cache rates and 1.5x output for the full request.”

Read that carefully. It is **for the full request**, not for the overflow above 272K. Crossing the line does not cost you double on the extra tokens; it costs you double on all of them.

A 0.7% increase in prompt size produces a 101% increase in the input bill. Cached reads double too, from $1.00 to $2.00 per million, and output goes from $50.00 to $75.00. GPT-5.6 Sol has the same rule, so this is not new — but at Astra’s rates the step is 2.5x taller.

The practical consequence: if your prompts sit anywhere near 272K, the single highest-leverage cost optimisation available to you is getting them under it. Trimming 5K tokens off a 275K prompt is worth more than any routing change you could make.

[SCREENSHOT: OpenAI Platform — usage dashboard showing input, cached input and output token spend by model]

## **Where teams hit trouble**

Four failure modes come up repeatedly in the first weeks after a frontier launch, and Astra has all four.

**Everything gets routed to the new model.** The most expensive thing a team does after a launch is point the default at it. At $10 input and $50 output, a classification call that a cheap model answers correctly now costs 2.5x what it did last month for no quality gain. The benchmark spread above is an argument for a mixed fleet, not a migration.

**The migration is not a string swap.** No none reasoning effort, no temperature, no top_p, no logprobs, and tool calling that requires the Responses API. Code that worked against gpt-5.6-sol can fail against gpt-6-astra in ways that are not obvious until production.

**Misalignment monitoring can stop your request.** OpenAI runs asynchronous checks on agent work in supported Responses API requests. The launch post is explicit about the consequence: in ChatGPT or Codex you may be asked to review an action, but “in the API, the task will stop.” OpenAI acknowledges these checks “can sometimes interrupt legitimate work.” That is a new reliability characteristic to design around, not a footnote.

**Nobody knows what it actually costs until the invoice arrives.** Per-token price tells you very little when the model uses a different number of tokens per task. Without per-request, per-team cost attribution, “is Astra cheaper or more expensive for us” is unanswerable.

## **Where TrueFoundry fits**

A more expensive frontier model does not weaken the case for a gateway. It strengthens it, because every routing decision is now worth 2.5x more.

**Route by complexity, not by habit.** The AI Gateway classifies incoming requests and sends each to an appropriate tier, so hard agentic work reaches Astra and extraction, classification and summarisation do not. In TrueFoundry’s own Auto Routing benchmark across 550 prompts, tiering cut cost by **69%** while retaining **98%** of baseline quality, with mean latency falling from 7.6s to 4.0s. On production-shaped traffic the reduction reached 80%. Those percentages applied to $10/$50 rates are worth considerably more than they were at $4/$20.

**Normalise caching across providers.** Astra’s cache semantics are OpenAI’s. Anthropic’s are different, and Bedrock’s differ again. Provider-agnostic prompt caching at the gateway means you write the prefix once and the 78.3% rule above stays true regardless of which model the router picks.

**Measure cost per task, not cost per token.** The whole Astra pricing argument turns on token efficiency, and you cannot resolve it from a price list. Per-request cost, token counts and latency broken down by team and model is how you find out whether Astra’s fewer-tokens-per-task claim holds on your workload.

**Keep one guardrail path across models.** Gateway-level guardrails run on the request and response path whichever model answers, so a routing change does not quietly change your safety posture.

## **How to run GPT-6 Astra through the gateway**

The short version, as of September 2026:

1. **Add OpenAI as a provider and register gpt-6-astra.** The gateway exposes it through the same OpenAI-compatible endpoint as everything else, so application code does not change when you route elsewhere later.
2. **Audit your request parameters first.** Strip temperature, top_p and logprobs. Replace any reasoning.effort: none with low. Move tool-calling traffic to the Responses API.
3. **Set reasoning effort deliberately per route.** low for routine calls, high for everyday agent work, xhigh or max for the hard cases. This is the largest single cost lever on the model, because effort drives output tokens and output is $50 per million.
4. **Put a hard ceiling under 272K input tokens.** Truncate, summarise or chunk before the request rather than after the invoice.
5. **Route a slice, do not migrate.** Send real traffic to both Astra and your incumbent, compare cost per completed task rather than cost per token, and expand only where Astra wins.

## **Related reading**

## **Conclusion**

GPT-6 Astra is a genuine step forward on the work it was built for. Terminal-Bench 4.0 up twenty points, long-context retrieval up twenty-two, AutomationBench more than doubled — those are not incremental. On the third-party composite indices OpenAI itself published, it is not the leader, and on single-shot knowledge questions the delta over GPT-5.6 Sol is close to a rounding error.

The pricing tells the same split story. Every rate went up 2.5x, which sounds like a straightforward increase until you notice that OpenAI’s own per-task estimates come out lower than Sol on several agentic benchmarks because Astra finishes in fewer tokens. Both things can be true. Which one applies to you is an empirical question about your traffic, and the only way to answer it is to run both and measure cost per completed task.

Two things do not require measurement. Caching still pays at exactly the same 78.3% break-even it did on GPT-5.6, so if you cache today, keep caching. And the 272K billing cliff is a step function that will quietly double an input bill for a prompt that grew by a few thousand tokens.

So it comes down to what you want to own. If you want one model and a simple bill, point everything at Astra and accept that you are overpaying for the easy half of your traffic. If you would rather send hard work to the frontier and everything else somewhere cheaper, that needs a routing layer — and at $10 input and $50 output, the arithmetic for building one just got two and a half times more persuasive.

TrueFoundry AI Gateway delivers ~3–4 ms latency, handles 350+ RPS on 1 vCPU, scales horizontally with ease, and is production-ready, while LiteLLM suffers from high latency, struggles beyond moderate RPS, lacks built-in scaling, and is best for light or prototype workloads.

## Recent Blogs

## Frequently asked questions

### What is GPT-6 and when was it released?

GPT-6 Astra is OpenAI’s frontier model, released on 3 September 2026 and callable in the API as gpt-6-astra. It is available through the OpenAI API, Microsoft Azure and AWS Bedrock, and to ChatGPT Plus, Pro, Business and Enterprise users.

### How much does GPT-6 cost?

As of September 2026, per 1M tokens: $10.00 input, $1.00 cached input, $12.50 cache writes, $50.00 output. Batch and Flex are 50% of those rates; Fast mode is 2x. Requests over 272K input tokens are billed at 2x input and cache rates and 1.5x output for the entire request.

### What is the GPT-6 context window?

1,050,000 tokens, with a 128,000-token maximum output. Both are identical to GPT-5.6 Sol. The practical limit is commercial rather than technical: past 272K input tokens the whole request is billed at double the input rate.

### Is GPT-6 worth it versus GPT-5?

It depends entirely on task shape. On long-horizon agentic work the gains are large — Terminal-Bench 4.0 goes from 37.3% to 57.9%. On single-shot knowledge questions they are marginal, and you are paying 2.5x per token for them. Route accordingly rather than switching wholesale.

### Does prompt caching still pay off at GPT-6 prices?

Yes, and by the same margin. Cache writes are 1.25x input and reads are 0.10x, exactly as on GPT-5.6, so the break-even sits at a 78.3% write share either way. Higher list prices raise the absolute savings, not the threshold.

### Does an AI gateway add meaningful latency?

TrueFoundry’s AI Gateway adds roughly 3-4 ms and sustains 350+ RPS on 1 vCPU, which is immaterial next to multi-second generation times.
