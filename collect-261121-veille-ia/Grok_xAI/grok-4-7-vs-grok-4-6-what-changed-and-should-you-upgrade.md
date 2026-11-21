---
id: collect-261121-veille-ia/grok-xai/grok-4-7-vs-grok-4-6-what-changed-and-should-you-upgrade
title: "Grok 4.7 vs Grok 4.6: What Changed and Should You Upgrade?"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: ["2026-09-21"]
keywords: ["grok", "model", "price", "tokens", "base", "september", "context", "larger"]
source: https://codersera.com/blog/grok-4-7-vs-grok-4-6-2026/
source_anchor: ""
source_lines: [1, 252]
section: "Grok_xAI"
---
**Quick answer.**Grok 4.7 (released 21 September 2026) keeps Grok 4.6's $2/$6 per million token price and 500K context, but runs on a new, larger base model. It beats 4.6 on all seven benchmarks in xAI's table, most on Terminal-Bench 4.0 (+17.3 points). The catch: about 2.25x more output tokens per task, so jobs can cost roughly twice as much.

SpaceXAI (xAI's name since the SpaceX merger) shipped Grok 4.7 on 21 September 2026, forty days after Grok 4.6 and a month after Elon Musk's first target date. The per-token price list stayed the same. What you are really buying with the upgrade is a larger model that keeps working on long, hard tasks. You pay for that in tokens, and on current measurements in wall-clock time as well.

This page covers only the differences between the two versions: specs, price, benchmark deltas, the real cost per task, speed, where each version is available, what the API migration involves, and whether you should switch. For the full Grok 4.7 guide covering features, tools, safety and the competitive picture, read our Grok 4.7 complete guide. For the Grok 4.6 baseline, see our Grok 4.6 launch guide.

## What changed from Grok 4.6 to Grok 4.7?

xAI's launch post lists four changes:

- **A new, larger base model.** Grok 4.6 was a post-training release on the existing foundation. Grok 4.7 is not. xAI has not published a parameter count. The "2.1T parameters" figure you will see comes from Musk's July posts and press coverage, and the launch materials do not confirm it.
- **A longer reinforcement-learning run on harder, multi-hour tasks.** The training is weighted toward work that takes a long time to finish.
- **Better self-verification and context management.** Cursor's model page says 4.7 "stays with difficult, long-running work and checks its own results more carefully than Grok 4.6."
- **Native understanding of the Grok Bot harness** , xAI's persistent "AI teammate" product that launched in beta on 11 August.

The API surface is almost unchanged. The context window, price list, modalities, reasoning-effort levels, rate limits and regions all match 4.6. The knowledge cutoff moves to May 2026. There is one behaviour change in the Responses API, covered in the migration section below.

## How do Grok 4.7 and Grok 4.6 specs compare?

| Spec | Grok 4.6 | Grok 4.7 | 
|---|---|---|
| Released | 12 August 2026 | 21 September 2026 | 
| API model ID | `grok-4.6` | `grok-4.7` (no aliases) | 
| Base model | Same foundation as 4.5, new post-training | New, larger base model | 
| Context window | 500,000 tokens | 500,000 tokens | 
| Modalities | Text + image in, text out | Text + image in, text out | 
| Knowledge cutoff | Not listed on docs.x.ai | May 2026 | 
| Reasoning effort | low / medium / high (default) / xhigh | low / medium / high (default) / xhigh | 
| Rate limits (base) | 150 RPS, 50M tokens/min | 150 RPS, 50M tokens/min | 
| Regions | us-east-1, us-west-2, us-central-1 | us-east-1, us-west-2, us-central-1 | 
| Batch API | Not supported | Not supported | 
| Responses API reasoning | Returned when requested | Always returns `reasoning.encrypted_content` | 
| Fast variant | Priority Processing ( `service_tier: "priority"` , 2x) | Grok 4.7 Fast, Cursor and Grok Build only (2x); Priority Processing still available on the API | 
| Open weights | No | No | 

Sources: docs.x.ai grok-4.7, docs.x.ai grok-4.6 and the xAI release notes, checked 22 September 2026. When 4.6 launched, the docs listed only two regions. us-central-1 now appears on both model pages.

## Did the price change from Grok 4.6 to Grok 4.7?

Not the per-token price. Both models sit on identical rows of the xAI price list:

| Rate (per 1M tokens) | Grok 4.6 | Grok 4.7 | 
|---|---|---|
| Input, prompt < 200K | $2.00 | $2.00 | 
| Cached input, prompt < 200K | $0.50 | $0.50 | 
| Output, prompt < 200K | $6.00 | $6.00 | 
| Input / cached / output, prompt ≥ 200K | $4.00 / $1.00 / $12.00 | $4.00 / $1.00 / $12.00 | 
| Fast / priority tier | 2x all rates (Priority Processing) | 2x all rates ($4 / $1 / $12 below 200K) | 
| Artificial Analysis blended price | $1.35 | $1.35 | 

The 200K threshold works the same way on both. Once a prompt reaches 200K tokens, xAI bills *every* token in that request at the higher rate, not only the tokens above the line. Server-side tools are also unchanged: web search and code execution cost $5 per 1,000 calls. Our Grok 4.6 pricing breakdown covers the cache economics and has worked examples, and all of it still applies to 4.7 token for token.

Some gateways are running launch deals. Vercel's AI Gateway took 40% off Grok 4.7 for the launch week starting 21 September, according to CellCog. OpenRouter showed a rate below list in search snippets, but we could not confirm it on the page, so treat it as unverified. GitHub Copilot bills Grok 4.7 at provider list price under usage-based billing.

## How much better is Grok 4.7 on benchmarks?

xAI's launch table compares 4.7 directly with 4.6 on seven benchmarks. Every row improves. These are **vendor-reported** numbers:

| Benchmark (xAI-reported) | Grok 4.6 | Grok 4.7 | Change | 
|---|---|---|---|
| Terminal-Bench 4.0 | 20.3% | 37.6% | +17.3 pts | 
| EEBench (electrical engineering) | 53.0% | 64.0% | +11.0 pts | 
| HealthBench Professional | 48.5% | 56.7% | +8.2 pts | 
| CursorBench 4.0 | 40.4% | 46.3% | +5.9 pts | 
| DeepSWE v1.1 | 65.2% | 71.0%* | +5.8 pts | 
| Harvey Legal Agent Benchmark | 15.8% | 19.6% | +3.8 pts | 
| AA-Briefcase v1.1 (Elo) | 1,546 | 1,657 | +111 | 

* xAI footnotes the DeepSWE score as "high effort." Some launch coverage reports Terminal-Bench as 38.0% and +17.7 points. The figure in xAI's own table is 37.6%.

The coding and terminal work that xAI trained for shows the largest gains. Terminal-Bench 4.0 nearly doubles. Legal agent work improves the least and stays low in absolute terms: 19.6% still means the model fails most Harvey tasks.

### Watch the benchmark versions

Do not compare these 4.6 numbers with the ones in our August coverage. The 4.6 launch post reported CursorBench *v3.2* (69.9%) and Terminal-Bench *v3.0* (26%). The 4.7 table uses CursorBench *4.0* and Terminal-Bench *4.0*, both harder, and re-scores 4.6 on them. Each row in the table above uses the same version for both models. Our Grok 4.6 benchmarks explainer describes the same version trap.

### What independent testing says

Artificial Analysis ran both models through its own harness. Its results are more modest than xAI's:

| Artificial Analysis metric | Grok 4.6 (high) | Grok 4.7 (xhigh) | 
|---|---|---|
| Intelligence Index v4.3.2 | 44 | 46 (+2) | 
| Index rank | #28 of 212 | #21 of 212 | 
| Coding Agent Index (with Grok Build, xhigh) | 47 | 56 (+9) | 
| GDPval-AA (Elo) | 1,605 | 1,695 (+90) | 
| AA-Omniscience hallucination rate | 34% | 29% | 
| AA-Omniscience index | 30 | 32 | 

Artificial Analysis reports the Coding Agent Index as "+9 over Grok 4.6 (xhigh)" and GDPval-AA as "+90 over Grok 4.6 (high)." We derived the 4.6 values from those deltas.

A +2 index gain for a new base model is small, and that is the main complaint in launch-week coverage. The +9 on the Coding Agent Index is the better signal for developers, because that index measures what 4.7 was trained for: long agentic coding runs inside a harness. Artificial Analysis now ranks Grok 4.7 fourth among native coding-agent harnesses, behind Claude Fable 5.1, GPT-6 Astra and Claude Opus 5.

A note on the numbers: our August articles quoted Grok 4.6 at 61 on the Intelligence Index. That score was on an earlier version of the index. On the current v4.3.2, Grok 4.6 (high) scores 44. Only compare scores taken on the same index version.

## Does Grok 4.7 cost more per task?

Yes, roughly twice as much. The per-token price did not move, but the number of tokens per task more than doubled.

| Artificial Analysis measurement | Grok 4.6 (high) | Grok 4.7 (xhigh) | Ratio | 
|---|---|---|---|
| Output tokens per Index task | ~36K | ~81K | ~2.25x | 
| Output-token cost per task at $6/1M | ~$0.22 | ~$0.49 | ~2.25x | 
| Total output tokens to run the Index | 94M | 240M | ~2.55x | 
| Total cost to run the Index | $2,351.83 | $4,967.35 | ~2.1x | 

Sources: AA Grok 4.7 page, AA Grok 4.6 page, AA launch analysis. We calculated the per-task cost row from AA's token counts and the $6/1M list price.

The comparison has one important caveat. Artificial Analysis headlines Grok 4.7 at **xhigh** effort and Grok 4.6 at **high**. Part of the 2.25x token increase comes from the higher effort setting, not only from the new model. xAI's API default is still `high` on both models. If you swap the model ID and change nothing else, your cost increase will probably be smaller than 2x. It will still be above zero, because the model was trained to keep working on long tasks and to check its own output. Cursor's docs also say 4.7 has "more separated effort levels," which means `xhigh` spends noticeably more than `high`.

In practice: budget for up to 2x on agent workloads, then measure your own cost per completed task at `high` before you commit. When the higher token count buys a task that 4.6 would have abandoned, the cost per *successful* task can drop. Before its release, 4.7 was reportedly held back because early RL runs pushed the model to give up on tasks too soon.

## Is Grok 4.7 faster or slower than Grok 4.6?

On output speed it is faster; on time to first token it is considerably slower. These figures were re-read from Artificial Analysis on 5 October 2026, and an earlier version of this page had both rows the wrong way round:

| Speed metric (Artificial Analysis, live) | Grok 4.6 (high) | Grok 4.7 (xhigh) | 
|---|---|---|
| Output speed | 72.9 tok/s | 83.3 tok/s | 
| Time to first token | 27.8s | 43.2s | 
| Time per Index task | Not published | ~7.1 minutes | 

The first-token wait is the regression, and it belongs to 4.7, not 4.6: about 43 seconds against 4.6's 28. Raw throughput actually improves by roughly 14% (83.3 against 72.9 tok/s), but that is not enough to offset 2.25x more output plus the longer wait before anything appears, so end-to-end a task still takes materially longer — AA measures 19.0s median end-to-end for 4.7 against 34.7s for 4.6 on its own prompt set, while a full Index task runs about 7.1 minutes. AA's launch article gives a different figure of about 188 tok/s on long prompts, and DataCamp measured about 53 tok/s. These are live endpoint numbers and they will change as xAI adds capacity. The reliable conclusion is that 4.7 is verbose and not fast.

xAI's answer for speed is **Grok 4.7 Fast**: the same model on faster infrastructure, with twice the output speed at twice the price ($4 / $1 / $12 per million tokens). It is *not* available on the public xAI API. It runs only in Cursor and Grok Build, and Grok Build's free tier does not include it. On the API, the closest option is Priority Processing (`service_tier: "priority"`, billed at 2x), which gives your requests higher scheduling priority. It does not run them on different hardware.

## Where is Grok 4.7 available compared with Grok 4.6?

| Channel | Grok 4.6 | Grok 4.7 (as of 22 Sept 2026) | 
|---|---|---|
| xAI API | Yes | Yes, day one | 
| Cursor | Yes | Yes; Fast is the default speed tier on Pro and above | 
| Grok Build | Yes | Default model; free tier gets standard only | 
| GitHub Copilot | Yes (from 14 Aug) | Gradual rollout to Pro, Pro+, Max, Business, Enterprise | 
| OpenRouter / Vercel / Cloudflare | Yes | Yes ( `x-ai/grok-4.7` on OpenRouter) | 
| Amazon Bedrock | GA 19 Aug | Not confirmed yet | 
| Google Model Garden | 21 Aug | Not confirmed yet | 
| Microsoft Foundry | 26 Aug | Not confirmed yet | 

If you use Grok 4.6 through one of the major clouds, you cannot upgrade there yet. Grok 4.6 took one to two weeks after launch to reach Bedrock, Model Garden and Foundry, so expect a similar gap for 4.7. That estimate comes from 4.6's timeline, not from any announced date.

Cursor users should check one setting in particular. Grok 4.7 Fast is the default speed tier on Pro and higher plans, so moving to 4.7 in Cursor can double your per-token rate on top of the higher token count. Cursor also uses a 256K standard window. Requests between 256K and 500K bill at 2x on standard and 3x on Fast, according to Cursor's model docs. Grok 4.6 is still in the Cursor model pool, and xAI has published no deprecation date for `grok-4.6` on the API.

## How do you migrate from Grok 4.6 to Grok 4.7?

For most code, you only change the model string. xAI's Grok 4.7 developer page lists one behavioural difference that matters.

### Chat Completions: change the model ID

```
from openai import OpenAI
client = OpenAI(api_key=XAI_API_KEY, base_url="https://api.x.ai/v1")
resp = client.chat.completions.create(
    model="grok-4.7",           # was "grok-4.6"
    reasoning_effort="high",    # default; try this before xhigh
    messages=[{"role": "user", "content": "Refactor this module and run the tests."}],
)
print(resp.choices[0].message.content)
```
xAI states that Chat Completions is not affected by the reasoning change. Parameters, tool definitions and structured-output schemas carry over as they are.

### Responses API: pass encrypted reasoning back

Grok 4.7 always returns `reasoning.encrypted_content` in the Responses API, even when your `include` list does not ask for it. In multi-turn conversations, xAI's instruction is to send those reasoning items back **unchanged** in the next request so the model keeps its context. If your code rebuilds the conversation from text only, or filters output items by type, test it: dropped reasoning items will not throw an error, but you will quietly lose context in long agent loops.

```
history = [{"role": "user", "content": "Plan the migration."}]
r1 = client.responses.create(model="grok-4.7", input=history)
# Append ALL output items (including reasoning) untouched, then the next turn
history += r1.output
history.append({"role": "user", "content": "Now implement step 1."})
r2 = client.responses.create(model="grok-4.7", input=history)
```
### Migration checklist

- Swap `grok-4.6` for`grok-4.7` everywhere, including fallbacks and eval configs.
- Keep `reasoning_effort` at`high` to start. Only move to`xhigh` for tasks where you can measure a better result.
- Raise output-token budgets and per-task timeouts. With about 2.25x the output and lower throughput, some tasks will hit limits that 4.6 never reached.
- Recheck your caching: set `prompt_cache_key` (Responses) or the`x-grok-conv-id` header (Chat Completions), as xAI's 4.7 docs recommend.
- Keep prompts under 200K tokens. The long-context price cliff is unchanged, and a more verbose model makes it easier to cross in a growing agent transcript.
- Run the old and new model side by side on your own eval set and compare *cost per successful task* , not cost per token.

If you are also considering other providers, our Claude Opus 5.5 vs GPT-6 Astra vs Grok 4.7 comparison covers the frontier models released the same week.

## Why was Grok 4.7 delayed, and when is Grok 4.8 coming?

Grok 4.7 missed four public deadlines. Per CellCog's timeline:

| Date (2026) | What Musk said | 
|---|---|
| 24 July | "Grok 4.7 in 4 weeks" (about 21 August) | 
| 12 August | "Ready in 3 to 4 weeks" | 
| 1 September | "Comes out in 10 days" | 
| 11 September | "Needs a few more days to cook" | 
| 13 September | Names Grok 4.8, a "2.5T" model on a new C++ training stack | 
| 21 September | Grok 4.7 ships (16:17 UTC), 31 days after the first target | 

The Grok 4.8 announcement on 13 September led Startup Fortune to report that 4.7 had been shelved. That was wrong, since 4.7 shipped a week later. The same report gave a cause for the delay: RL penalised response length too heavily, and the model learned to abandon tasks early. That fits with the verbosity of the model that eventually shipped. xAI has not confirmed the explanation.

**Grok 4.8 release date:** there is none. As of 22 September 2026, xAI has announced no date, pricing or API plan for Grok 4.8. Musk said pre-training would finish the week of 14 September, followed by RL, and described 4.8 as "a noticeable improvement" over 4.7. He placed 4.7 at roughly Claude Opus 5.0 level and said 4.9 would "probably" reach Astra/Fable class. Estimates of an October release are extrapolations from those comments, and given that 4.7 slipped by a month, you should not plan a roadmap around them. The 2.5T parameter figure for 4.8 is also Musk's claim and has not been published as a spec.

## Should you upgrade from Grok 4.6 to Grok 4.7?

| Your workload | Recommendation | Why | 
|---|---|---|
| Long-running coding agents (Cursor, Grok Build, CI agents) | Upgrade | +9 on AA Coding Agent Index, +17 pts Terminal-Bench, less task abandonment | 
| Terminal / DevOps automation | Upgrade | Terminal-Bench 4.0 is the largest gain (20.3% to 37.6%) | 
| Engineering, health or legal knowledge work | Upgrade and test | EEBench +11, HealthBench +8.2, GDPval-AA +90; legal scores are still low | 
| High-volume, short-answer tasks (classification, extraction, chat) | Stay on 4.6, or drop to `grok-4.3` | Same price, more tokens, slower output; little to gain on short tasks | 
| Latency-sensitive interactive UI | Test first | Slower first token (~43s vs ~28s) despite higher throughput, and longer answers — the wait before the first character is what a user feels | 
| Fixed monthly token budget | Upgrade carefully | Plan for up to ~2x spend per agent task; cap effort at `high` | 
| Access through Bedrock, Model Garden or Foundry | Wait | 4.7 is not confirmed on the clouds yet | 
| Waiting for Grok 4.8 | Don't wait | No date exists; migrating to 4.7 is a one-line change | 

The practical rule: make Grok 4.7 your default for agentic and coding work, and keep 4.6 or a cheaper model for everything else. If you run coding agents across tools, our Grok Build vs Claude Code vs Codex CLI comparison and the AI coding agents guide show where Grok fits in a multi-model setup.

## FAQ

### What is the difference between Grok 4.7 and Grok 4.6?

Grok 4.7 runs on a new, larger base model with a longer RL run on multi-hour tasks. Grok 4.6 was a post-training update of the earlier foundation. Price ($2/$6), context (500K), modalities and reasoning-effort levels are the same. Grok 4.7 scores higher on all seven benchmarks in xAI's table, uses about 2.25x more output tokens per task, and has a May 2026 knowledge cutoff.

### Is Grok 4.7 more expensive than Grok 4.6?

The per-token price is identical: $2.00 input, $0.50 cached and $6.00 output per million tokens below 200K, doubling across the whole request at 200K and above. Cost per task is higher. Artificial Analysis spent $4,967 running its Intelligence Index on Grok 4.7 at xhigh, against $2,352 for Grok 4.6 at high, about 2.1x.

### How do I upgrade from Grok 4.6 to Grok 4.7 in the API?

Change the model ID from `grok-4.6` to `grok-4.7`. Chat Completions needs nothing else. In the Responses API, Grok 4.7 always returns `reasoning.encrypted_content`, and you should pass those reasoning items back unchanged in the next turn to keep context. Raise output-token limits and timeouts as well.

### Can I use Grok 4.7 Fast through the xAI API?

No. Grok 4.7 Fast, the same model on faster infrastructure at twice the token price, is available only in Cursor and Grok Build, and not on Grok Build's free tier. On the API, Priority Processing (`service_tier: "priority"`) is the nearest equivalent, also billed at 2x.

### When will Grok 4.8 be released?

There is no official date. Musk announced Grok 4.8 on 13 September 2026 as a 2.5T-parameter model trained on a new C++ stack, with RL to follow pre-training. xAI has published no release date, price or API details. Grok 4.7 arrived a month after its first target, so treat any October estimate as speculation.

### Is Grok 4.6 being deprecated?

Not yet. As of 22 September 2026, `grok-4.6` is still on xAI's model list with no deprecation notice, and it remains in Cursor's model pool. GitHub has announced a separate mid-October deprecation of selected Copilot models without naming Grok 4.6 in the 4.7 changelog, so check your provider's notices.

## Sources

- SpaceXAI: Introducing Grok 4.7 (benchmark table, Fast variant)
- docs.x.ai model list and pricing, grok-4.7, grok-4.6, Grok 4.7 developer notes, release notes, pricing
- Artificial Analysis: Benchmarking Grok 4.7, Grok 4.7 and Grok 4.6 model pages
- Cursor docs: Grok 4.7
- GitHub changelog: Grok 4.7 in Copilot
- CellCog: Grok 4.7 release timeline, Grok 4.8 release date

If your team is building agents or products on Grok, Claude or GPT and needs more engineering capacity, Codersera can help you hire vetted remote developers who already work with these models.
