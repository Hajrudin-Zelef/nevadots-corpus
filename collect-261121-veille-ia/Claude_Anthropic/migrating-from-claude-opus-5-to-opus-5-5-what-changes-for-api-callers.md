---
id: collect-261121-veille-ia/claude-anthropic/migrating-from-claude-opus-5-to-opus-5-5-what-changes-for-api-callers
title: "Migrating from Claude Opus 5 to Opus 5.5: What Changes for API Callers"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-09-22"]
keywords: ["opus", "million", "tokens", "model", "page", "same", "claude", "token"]
source: https://dev.to/hassann/migrating-from-claude-opus-5-to-opus-55-what-changes-for-api-callers-3kl7
source_anchor: ""
source_lines: [1, 120]
section: "Claude_Anthropic"
---
Anthropic shipped Claude Opus 5.5 on September 22, 2026. For anyone calling the Claude API, the upgrade is one string: `claude-opus-5` becomes `claude-opus-5-5`.

What sits behind that string is 20% cheaper per token in both directions, caps a single response at 128,000 output tokens, is claimed to produce output 30% faster, and is built to stay on one task for 18 hours or more. None of that is free of consequences for code you tuned against Opus 5.

This is the second migration hop on this model line and it reads nothing like the first. Opus 4.8 to Opus 5 was a correctness migration: defaults moved underneath working code, and one previously valid request combination started returning a hard 400. This hop is a budget and operating envelope migration. The work is in re-measuring numbers you already measured once, because every one of them moved. Opus 5.5 also landed the same day as two new OpenAI models, which is its own story.

| Line item | Opus 5 | Opus 5.5 | Change | 
|---|---|---|---|
| Model id | `claude-opus-5` | `claude-opus-5-5` | one string | 
| Input, per million tokens | $5.00 | $4.00 | 20% less | 
| Output, per million tokens | $25.00 | $20.00 | 20% less | 
| Cache write, per million | check vendor page | $5.00 | unknown | 
| Cache read, per million | check vendor page | $0.20 | unknown | 
| Fast mode, per million | check vendor page | $8.00 in, $40.00 out | unknown | 
| Max output tokens | check vendor page | 128,000 | unknown | 
| Context window | check vendor page | 1,000,000 | unknown | 

The blank cells are deliberate. Anthropic published the Opus 5.5 side on its model page and we are not going to reconstruct the Opus 5 side from memory, because a wrong number in a cost model is worse than a missing one. Pull those five rows off Anthropic's pricing page before you build a savings estimate on them.

Swap the string and you pay 20% less per token in both directions. That part is arithmetic: $5 to $4 on input, $25 to $20 on output, the same cut on both sides.

Anthropic also says Opus 5.5 costs 40% less to run than Opus 5. Those two numbers are not in conflict, but they are not the same claim either. A 20% per-token cut cannot produce a 40% bill reduction on its own. The other half has to come from the model using fewer tokens to finish the same job, and token efficiency is workload-specific in a way that a price sheet is not.

So treat 40% as a reason to re-measure rather than a number to drop into a forecast. Replay a representative slice of last week's real traffic against both model ids with identical prompts and settings, compare the `usage` blocks rather than your impression of them, and multiply the actual token counts by the new rates. Your answer will land somewhere between 20% and 40%, and where it lands tells you something useful about how much of your prompt was never needed.

Cache reads on Opus 5.5 cost $0.20 per million tokens against $4.00 for fresh input, a 95% discount. Cache writes cost $5.00 per million, a $1.00 premium over the $4.00 you would have paid to send those tokens uncached.

That premium pays for itself on the first hit, with change. Writing a prefix costs $1.00 per million more than not caching it. Each later read of that prefix saves $3.80 per million. You break even at roughly a quarter of one re-read, which in practice means: if the prefix is ever reused even once, caching it was correct.

Here is the shape on a 200,000 token system prompt, which is a normal size once you paste in an OpenAPI spec bundle:

| Pattern | Cost for 10 calls | 
|---|---|
| Fresh input every call, $4.00/M | $8.00 | 
| Write once at $5.00/M, then 9 reads at $0.20/M | $1.36 | 

Same prompts, same model, 83% less. The request that gets you the second row marks the prefix explicitly:


```
{
  "model": "claude-opus-5-5",
  "max_tokens": 4096,
  "system": [
    {
      "type": "text",
      "text": "<your 200k-token spec bundle>",
      "cache_control": {"type": "ephemeral"}
    }
  ],
  "messages": [
    {"role": "user", "content": "Which endpoints changed between v2 and v3?"}
  ]
}
```
Then read the response to confirm you got a hit, because a cache you think you have and do not is the most expensive kind:


```
"usage": {
  "input_tokens": 41,
  "cache_creation_input_tokens": 204800,
  "cache_read_input_tokens": 0,
  "output_tokens": 612
}
```
On the first call, `cache_creation_input_tokens` carries the load. On every call after it, that number should drop to zero and `cache_read_input_tokens` should pick it up. If it does not, something upstream is mutating your prefix, usually a timestamp, a request id, or a reordered tool list. Saving one call in Apidog and firing it twice is the fastest way to see which of the two fields moves. The full arithmetic, including when a 1M context is worth caching at all, is in the Opus 5.5 caching cost math.

Opus 5.5 will return up to 128,000 output tokens in one response. At $20 per million that is $2.56 for a single maxed-out call, which is worth knowing before someone sets `max_tokens` to the ceiling in a retry loop.

The bigger issue is everything between the model and your code:

- 
**HTTP read timeouts.** A response that long takes minutes, not seconds. Default client timeouts in most SDKs and reverse proxies are shorter than that.
- 
**Proxy and gateway buffers.** A gateway that buffers the full body before forwarding holds megabytes per in-flight request, and some will just cut the connection.
- 
**Non-streaming calls.** Without streaming you wait the whole time with nothing to show and nothing to checkpoint.
- 
**Truncated JSON.** Hitting`max_tokens` mid-object produces invalid JSON, not a partial object you can salvage. Check`stop_reason` before you parse.

Raising `max_tokens` because the ceiling went up is the easy half. Getting your timeout chain, your buffers and your parser to agree with the new ceiling is the half that causes the incident.

Anthropic states Opus 5.5 produces output 30% faster than Opus 5. Output speed is one term in the latency you actually serve. Time to first token, retries, and queueing sit alongside it, and a model that streams faster can still feel slower if it thinks longer before the first token arrives.

So measure it. Fire a fixed set of representative prompts at both model ids from the same network position, and record p50 and p95 separately for time to first token and for total time. Average latency hides the case that pages you.

If you keep API test scenarios in Apidog, this is a small job: clone one saved request, change the model id, run both on a schedule, and read the response time series rather than a stopwatch. The point is to enter the migration with a before number, because after you cut over there is no way to get one.

Fast mode on Opus 5.5 costs $8.00 per million input and $40.00 per million output. Set next to the standard $4.00 and $20.00, the pricing rule is easy to hold in your head: fast mode is a straight 2x on both sides.

That makes the decision arithmetic rather than judgement: if latency on a given route is worth doubling that route's token bill, use it there, and otherwise do not. The usual split is fast mode on interactive paths and standard rates on batch and background work.

Anthropic's published numbers for Opus 5.5 are strong: Terminal-Bench 4.0 at 66.4%, FrontierCode v1.1 at 54.4%, CursorBench 4.0 at 57.8%, GDPval-AA v2.1 at 1846 Elo, AutomationBench at 40.0%, Humanity's Last Exam at 67.7% with tools, OSWorld 2.0 at 81.8%, and Chartography at 89.0%. Artificial Analysis, a third party rather than either vendor, places it first of 212 models on its index with a score of 58.

Two more vendor claims matter operationally rather than competitively. Opus 5.5 is built to stay on a single task for 18 hours or more, which breaks a lot of ordinary assumptions about timeouts, retries and resumability. And Anthropic reports 85% fewer successful boundary-circumvention attempts, which is a testable property of your own endpoints rather than a property you inherit.

None of these tell you whether Opus 5.5 is better on your prompts. Run your own eval set across both ids before cutting production over, for the same reason you would not ship a database upgrade on the vendor's benchmark numbers.

1. Change `claude-opus-5` to`claude-opus-5-5` in one service, not all of them.
2. Read Anthropic's migration guide for this hop before assuming the parameter surface is unchanged. The 4.8 to 5 jump moved defaults quietly, so verify rather than infer.
3. Replay a slice of real traffic against both ids and diff the `usage` blocks.
4. Recalculate your monthly spend at $4 and $20, and separately at $5 cache writes and $0.20 cache reads.
5. Confirm cache hits in `cache_read_input_tokens` rather than trusting that the prefix is stable.
6. Raise your HTTP read timeouts and check gateway buffers before anyone sets `max_tokens` near 128,000.
7. Record p50 and p95 latency on both ids before you cut over.
8. Decide fast mode route by route, at 2x list.
9. Re-run your own eval set. Vendor benchmarks are the reason to test, not the test.

Opus 5.5 is a cheaper, faster, longer-running version of a model you already call. The migration risk is not that the string change fails. It is that you carry over a cost model, a timeout, and a `max_tokens` value that were all sized for a different model.

**Is `claude-opus-5-5` a drop-in replacement for `claude-opus-5`?** Treat it as one only after checking Anthropic's migration guide for this hop. The id change is a one-line edit, but the previous hop on this line moved defaults without changing request shape, so verification is cheap insurance.

**How much cheaper is Opus 5.5?** 20% less per token in both directions, $5 to $4 on input and $25 to $20 on output. Anthropic separately states a 40% reduction in cost to run, which implies token efficiency gains on top of the rate cut. Measure your own workload to see where between the two you land.

**What does prompt caching cost on Opus 5.5?** Writes are $5.00 per million tokens and reads are $0.20 per million, against $4.00 for uncached input. One reuse of a cached prefix more than pays for the write premium.

**Does the 128,000 token output limit change anything in my client?** Usually yes. Check your HTTP read timeouts, your gateway buffering, and whether you stream. Also check `stop_reason` before parsing, because hitting the cap mid-object produces invalid JSON rather than a truncated object.

## Top comments (0)
