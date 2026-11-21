---
id: collect-261121-veille-ia/claude-anthropic/claude-sonnet-5-5-vs-opus-5-5-when-half-the-price-really-saves
title: "Claude Sonnet 5.5 vs Opus 5.5: When Half the Price Really Saves"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-09-28"]
keywords: ["sonnet", "opus", "claude", "medium", "work", "effort", "half", "price"]
source: https://blog.laozhang.ai/en/posts/claude-sonnet-5-5-vs-opus-5-5
source_anchor: ""
source_lines: [1, 161]
section: "Claude_Anthropic"
---
## On this page

**Run Claude Sonnet 5.5 at low or medium effort for well-scoped, checkable work: bug fixes, test-covered changes, documents, and high-volume tool loops. Keep Claude Opus 5.5 at its default `medium` effort for open-ended work that needs sustained judgment.** If a task only goes well once you push Sonnet 5.5 to `xhigh` or `max`, move that task to Opus 5.5 instead. At that point Sonnet's half-price token rate no longer buys you a cheaper result.

Sonnet 5.5 launched on September 28, 2026, six days after Opus 5.5. Per million tokens it costs $2 input and $10 output, against $4 and $20 for Opus 5.5. Cache reads cost $0.10 on Sonnet 5.5 and $0.20 on Opus 5.5 (Anthropic pricing), so every rate is now exactly half. How many tokens each model spends at a given effort level decides whether "half the price" turns into a smaller bill. Anthropic's launch charts plot cost per task by effort level, and Artificial Analysis measures both models at all five effort levels on its Intelligence Index v5.0, the nine-evaluation version that replaced v4.3.2 on October 27–28, 2026. Neither tells you what your own tasks will cost, so the sections below show how to work that out.

## Which model to start with, by kind of work

| Your work | Start with | Effort | Switch when | 
|---|---|---|---|
| Clear spec, and tests or a checker can confirm the result (bug fixes, small features, refactors with tests) | Sonnet 5.5 | `medium` ;`low` for simple edits | Failures keep recurring after one retry at `high` : send those tasks to Opus 5.5 | 
| High-volume agent loops, PR review on every commit, bulk document or spreadsheet work | Sonnet 5.5 | `low` or`medium` | Misses start costing more than the tokens you saved | 
| Chat and other latency-sensitive replies | Sonnet 5.5 | `low` or`medium` | Rarely; this is where Sonnet's speed matters most | 
| Ambiguous specs, architecture calls, long multi-step work where a wrong early decision is expensive | Opus 5.5 | `medium` (its default on the API and in Claude Code) | Opus still misses at `high` or`xhigh` : see Claude Fable 5.2 vs Opus 5.5: When Fable High Beats Opus Max | 
| Hard code review where a missed bug is the costly failure | Opus 5.5 | `medium` or higher | Stays on Opus unless your own misses show otherwise | 
| You don't know yet | Opus 5.5 | `medium` | Measure Sonnet 5.5 at `low` and`medium` against it, as shown at the end | 

Most of these rows follow Anthropic's own positioning. Its launch post calls Sonnet 5.5 "strongest at well-scoped everyday tasks, fixing bugs, and creating polished documents, slides, and spreadsheets," while "Opus 5.5 remains clearly stronger at complex, open-ended work requiring sustained judgment" (Introducing Claude Sonnet 5.5). The effort starting points come from Anthropic's Sonnet 5.5 prompting guide. For agentic coding it recommends `medium` for well-specified tasks and `high` for harder or longer ones. Its models overview still says "if you're unsure which model to use, start with Claude Opus 5.5 for most workloads" (models overview). That explains the last row.

A smaller option now exists. Anthropic released Claude Haiku 5.5, aimed at high-volume and cost-sensitive work, on October 7, 2026, at one-twentieth of Sonnet 5.5's input and output prices; Claude Haiku 5.5 vs Sonnet 5.5: When the 20x Price Gap Pays Off covers which work to move down.

## Why "half the price" and "costs more per task" are both true

The price sheet on its own looks simple:

| Claude API, per million tokens | Sonnet 5.5 | Opus 5.5 | 
|---|---|---|
| Input | $2 | $4 | 
| Output | $10 | $20 | 
| Cache write, 5 minutes | $2.50 | $5 | 
| Cache write, 1 hour | $4 | $8 | 
| Cache read | $0.10 | $0.20 | 
| Batch input / output | $1 / $5 | $2 / $10 | 
| Fast mode | Not offered | $8 / $40 | 

Sonnet 5.5 costs exactly half on every line. Cache reads were the exception at launch, when both models charged $0.20. On October 7, 2026, Anthropic cut Sonnet 5.5's cache read to $0.10, so both models now pay 0.05x their input price for cache hits, while most models pay 0.1x (Claude Sonnet 5.5 Cache Read Price Cut to $0.10: What You Save). Apart from that cut, Sonnet 5.5 keeps Sonnet 5's input, output and cache-write rates (Sonnet 5's cache read stays at $0.20), so the other rates in Claude Sonnet 5 Price Increase Canceled: Current Rates and Billing Math still apply. Both models have a 1M-token context window and a 128K-token output limit, and a full 1M context is billed at the standard rates.

A per-task bill multiplies these prices by the tokens a model actually uses: thinking, output, extra turns, and the context it re-reads on each turn. Three things can erase Sonnet's price advantage.

**Effort changes how much work Sonnet 5.5 does.** Anthropic's launch post draws the line itself. Sonnet 5.5 "complements Opus 5.5 best when running at lower effort settings, where it costs less per task. At higher settings, it can perform comparably at a similar cost." The prompting guide explains why the top settings get expensive. At `xhigh` and `max`, Sonnet 5.5 "can start its own rounds of review and verification, sometimes with subagents," and makes related fixes it noticed along the way. The benchmark table even carries a footnote for this: on FrontierCode, Sonnet 5.5 scored lower at `max` (46.2%) than at `xhigh` (52.1%). Anthropic says the reason was that at `max` it ran Claude Code's code-review skill more often, which led to timeouts and edits outside the task's scope.

**Third-party measurements show the advantage running out at `xhigh` and `max`.** Artificial Analysis runs both models at all five effort levels on its Intelligence Index v5.0. At `max`, Sonnet 5.5 scored 54 at $10.02 per index task and Opus 5.5 scored 58 at $10.04, so the model with half the token price cost the same per task, scored 4 points lower, and took longer (1,326 seconds per task against 1,133). At `max`, Sonnet 5.5 produced about 285,000 output tokens per index task against 173,000 for Opus 5.5, roughly 1.65 times as many. Output tokens are only part of the gap, though. More turns also mean more input and more cache reads. These figures are from Artificial Analysis's release comparison of the two models as of October 28, 2026:

| Effort | Sonnet 5.5 (score · cost · time per task) | Opus 5.5 (score · cost · time per task) | Sonnet's cost as a share of Opus's | 
|---|---|---|---|
| `low` | 25 · $0.51 · 121 s | 35 · $0.98 · 147 s | 52% | 
| `medium` | 30 · $0.78 · 178 s | 48 · $2.45 · 337 s | 32% | 
| `high` | 41 · $1.64 · 332 s | 52 · $3.46 · 464 s | 47% | 
| `xhigh` | 50 · $4.09 · 691 s | 57 · $6.05 · 779 s | 68% | 
| `max` | 54 · $10.02 · 1,326 s | 58 · $10.04 · 1,133 s | 100% | 

At the same effort, Sonnet 5.5 is never the more expensive model, but Opus 5.5 scores higher at every level. "Costs more per task" becomes true once Sonnet needs a higher effort than Opus to get the same result: Sonnet at `xhigh` (50, $4.09) costs more than Opus at `high` (52, $3.46) and scores lower. Across effort levels the trade-off gets sharper. Sonnet at `high` (41, $1.64) scores 6 points above Opus at `low` (35, $0.98) for about 1.7 times the cost. Opus at `medium` (48, $2.45) scores 7 points above Sonnet at `high` for about 1.5 times the cost, in the same time per task (337 seconds against 332). Opus at `low` beats Sonnet at `medium` by 5 points (35 vs. 30) for about 1.26 times the cost. From `xhigh` up, Opus wins outright: Opus at `high` (52, $3.46, 464 s) outscores Sonnet at `xhigh` (50, $4.09, 691 s) for less money and less time, and Opus at `xhigh` (57, $6.05) beats Sonnet at `max` (54, $10.02).

**The API defaults don't match.** On the Claude API, Sonnet 5.5 defaults to `high` effort and Opus 5.5 defaults to `medium`. If you swap the model ID and leave effort unset, you are comparing Sonnet one level higher than Opus. In Claude Code, both models start at `medium`, so the mismatch only affects raw API calls. On the API, set effort explicitly on both sides before you compare anything.

The word "Max" can mean two things here. The **Claude Max plan** is a subscription tier. **`max` effort** is the highest effort level on the API and in Claude Code. The Artificial Analysis numbers above refer to the effort level.

## Break-even: how much more work Sonnet 5.5 can do and still cost less

At identical token counts, Sonnet 5.5 costs exactly half as much, cache reads included since October 7, 2026. That gives a simple way to see how much extra work it can do before the bill stops favoring it.

Suppose Sonnet 5.5 needs `k` times as many tokens as Opus 5.5 for the same task, from more turns, more thinking, or more re-reading. Every rate is half, so the rule doesn't depend on how your bill splits between input, output, and cache reads:

Before the cut, when cache reads cost $0.20 on both models, cache-heavy work narrowed that margin: the break-even fell from 2.0x with no caching to about 1.33x when cache reads were half the Opus bill, and 1.14x at 75%. That curve no longer applies.

*The chart shows the break-even curve at the old $0.20 Sonnet 5.5 cache-read price; at the $0.10 price in effect as of October 28, 2026, the multiple is 2.0x at every cache-read share.*

Three illustrative token profiles, priced per turn at list rates (inputs are examples, not measured traffic):

Anthropic says cache reads "make up the majority of agentic and coding work costs" (in its Opus 5.5 announcement). At the old shared $0.20 rate that cut Sonnet's room in agent loops to about 1.33x; at $0.10 an agent loop gets the same 2x margin as an uncached chat call. Whether Sonnet 5.5 stays under 2x depends on effort. On Artificial Analysis's index tasks it cost 32% to 68% of Opus 5.5 per task from `low` to `xhigh`, comfortably inside the margin. At `max` the two cost the same per task, which puts Sonnet right on the 2x line.

The same ratio holds for batch jobs, because both models get the same 50% batch discount. Fast mode changes the comparison in the other direction. At $8 / $40 it doubles Opus 5.5's standard input and output rates, so an Opus fast-mode bill is being compared with Sonnet at a quarter of the price on those lines.

## What the benchmarks settle, and what they don't

Anthropic's launch table puts the two models close on most rows. The table leaves out one thing that matters: most scores are at the highest effort each model was run at, not at the settings you would use day to day.

| Benchmark (Anthropic-reported) | Sonnet 5.5 | Opus 5.5 | Effort note | 
|---|---|---|---|
| Terminal-Bench 4.0 | 70.6% | 66.4% | Opus 5.5 at `xhigh` , its highest score | 
| FrontierCode 1.1 (Main) | 52.1% at `xhigh` (46.2% at`max` ) | 54.4% | Sonnet scored lower at `max` than at`xhigh` | 
| CursorBench 4.0 | 55.5% | 57.8% | High-effort results | 
| GDPval-AA v2.1 | 1844 | 1846 | Run by Artificial Analysis on a pre-release deployment; part of its v4.3.2 index, not v5.0 | 
| Humanity's Last Exam, with tools | 64.5% | 67.7% | High-effort results | 
| OSWorld 2.1 (partial) | 80.1% | 81.8% | High-effort results | 

The "beats Opus at coding" headline is the Terminal-Bench 4.0 row, where Sonnet 5.5 leads by about four points. On the other coding rows Opus 5.5 is ahead by two or three. At its default `medium` effort, Anthropic reports Opus 5.5 at 54.6% on FrontierCode and 52.5% on CursorBench. Don't set that 52.5% against Sonnet's 55.5% as if the two were run the same way. One is Opus at medium, the other is Sonnet at a high setting.

Independent coding results so far are small but consistent. CodeRabbit tested 13 hard pull-request cases, each with one verified bug. Sonnet 5.5 with thinking on caught 6. Opus 5.5 caught 8 at standard settings and 10 at max, both from CodeRabbit's earlier September run on the same cases. Opus also had higher actionable precision (66.7% at standard vs 41.2% for Sonnet 5.5). Sonnet 5.5's review calls cost about $0.46–$0.47 per review at list price, and CodeRabbit concluded that each model fits a different role. Opus 5.5 is for high-risk changes where a missed bug is expensive. Sonnet 5.5 is for the review pass on every pull request. CodeRabbit also ran one side-by-side Claude Code build. Sonnet 5.5 finished in 29 minutes 27 seconds and Opus 5.5 in 44 minutes 50 seconds, with results "close to identical" and Opus "a little higher in fidelity." CodeRabbit calls it "one run, not a benchmark," and 13 cases show a direction rather than proof.

## In Claude Code and the Claude apps

On a Pro or Max subscription, you don't pay per token, so the practical trade-offs are speed and quality, plus how quickly you use up your plan's limits. Anthropic lists Sonnet 5.5's latency as "Fast" and Opus 5.5's as "Moderate." Sonnet 5.5 is Anthropic's fastest Sonnet so far, generating output more than 30% faster than Sonnet 5.

In Claude Code, Sonnet 5.5 and Opus 5.5 both start at `medium` effort. You can change the model and the effort level separately (Claude Code model configuration):

A few things to know before you rely on it:

- **Plan access.** Claude's pricing page lists Sonnet as available on Free, while Opus needs Pro ($20/month, or $17/month billed annually) or Max (from $100/month). The table doesn't name model versions, so check that Free shows Sonnet 5.5 in your model picker. For choosing between the paid plans, see Claude Pro vs Max in 2026: Pricing, Claude Code Limits, and When Max Is Worth It. Opus 5.5 also raised five-hour usage limits on Pro, Max, Team, and seat-based Enterprise plans. Claude Opus 5.5 Pricing and Limit Reset: What Each One Changes covers that change.
- **Changing effort is cheap; changing models is not.** On these two models, with an API key or a Claude subscription, a new effort level keeps the prompt cache and applies right away. That makes`/effort high` for one hard step a low-cost move. It doesn't hold on Amazon Bedrock, Google Cloud, or a Claude apps gateway. Switching models is different. Each model has its own cache, so after`/model` the next request re-reads the whole conversation with no cache hits. Claude Code asks you to confirm while the cache is still warm (prompt caching in Claude Code).
- **Switching mid-session also drops the previous model's reasoning.** On the API, Sonnet 5.5 can't read Opus 5.5's thinking blocks, and no other model reads Sonnet 5.5's. Blocks a model can't read are silently dropped. Claude Code's documentation doesn't spell out what`/model` does with earlier thinking, but the API rule suggests the new model continues without it. To hand a plan from Opus to Sonnet, have Opus write the plan into a file or a normal message first. Plain text carries over, and thinking blocks don't.
- **`opusplan` is the built-in split.** This alias uses`opus` in plan mode and`sonnet` for execution. Each plan-mode toggle is a model switch, so it starts a fresh cache every time. Check what the aliases point to on your provider. On the Anthropic API,`sonnet` means Sonnet 5.5, but on Amazon Bedrock and Google Cloud it resolves to Sonnet 4.5, and on Claude Platform on AWS to Sonnet 4.6. On Microsoft Foundry,`opus` is Opus 4.6 and`sonnet` is Sonnet 4.5. On those providers, pin the full model ID, or set`ANTHROPIC_DEFAULT_SONNET_MODEL` to your provider's Sonnet 5.5 ID.
- **Safety fallbacks differ.** In the Claude apps, Sonnet 5.5 is the first Sonnet with cyber fallbacks. A narrow set of flagged offensive-security requests and some frontier-AI-development requests are re-run on Sonnet 5, and the chat stays on Sonnet 5 until you switch back. Opus 5.5's cyber fallbacks go to Opus 4.8. Routine secure-coding work, such as scanning your own source code for vulnerabilities, stays on Sonnet 5.5 according to Anthropic's help page. The classifiers also read memory, connector content, search results, and files, so a switch can come from content you didn't type.
- **Fast mode is Opus-only.** Opus 5.5 fast mode is available in Claude Code and on the Claude Platform, and Anthropic claims "up to 2.5x speed." Sonnet 5.5 has no fast mode. It is fast by default.

## Swapping model IDs in an API integration: what breaks

Both models reject request shapes that worked on Sonnet 5 and Opus 5. Moving between them has its own traps too. Check this list before you flip a model ID in production:

| Behavior | Sonnet 5.5 ( `claude-sonnet-5-5` ) | Opus 5.5 ( `claude-opus-5-5` ) | 
|---|---|---|
| API default effort | `high` | `medium` | 
| `thinking: {"type": "disabled"}` | 400 error; send `{"type": "between_tools"}` to turn off up-front thinking | 400 error; thinking is always on, so lower `effort` instead | 
| `between_tools` limits | Only at `low` ,`medium` ,`high` ; 400 at`xhigh` /`max` ; no per-message effort changes | Not available | 
| Manual `budget_tokens` | 400 error | 400 error | 
| `tool_choice``any` or`tool` | 400 error; use `auto` with`strict: true` or structured outputs | Same | 
| Non-default `temperature` ,`top_p` ,`top_k` | 400 error | 400 error (Opus 4.7 and later) | 
| Text between tool calls | Arrives in `thinking` blocks, empty at default`display: "omitted"` | Same | 
| `computer_20251124` tool | Rejected on Claude API and Google Cloud; use `computer_toolset_20260801` (Bedrock still accepts it) | Same | 
| Fast mode ( `speed: "fast"` ) | Not offered | Claude API, Google Cloud and Microsoft Foundry | 
| Server-side fallback ( `fallbacks: "default"` , beta) | Retries `cyber` and`frontier_llm` refusals on Sonnet 5 | Retries on the model Anthropic recommends for the category | 
| Thinking blocks after a switch | Can't read Opus 5.5 blocks; no other model reads Sonnet 5.5 blocks | Can't read Sonnet 5.5 blocks | 
| Editing history before a thinking block | 400 by default on accounts created on or after August 31, 2026, unless you opt into `drop_block` | Same | 

Sources: What's new in Claude Sonnet 5.5, What's new in Claude Opus 5.5, and the Opus 5.5 migration guide.

Two failures don't show up as errors. A streaming UI "goes quiet between tool calls, with no error" until you set `thinking.display` on either model. On Sonnet 5.5 you can also use `between_tools` to get the text back. And a router that sends one conversation to Opus 5.5 for some turns and to Sonnet 5.5 for others still gets successful requests. Each model simply works without the other's reasoning, and the dropped blocks aren't billed. If you route, route per task or per conversation, not per turn. On the API, keep top-level effort fixed within a cached conversation as well, since changing it invalidates the prompt cache. The per-message effort beta changes effort for a single turn while keeping the cache.

## Using both: plan with Opus, build with Sonnet, escalate failures

"Opus plans, Sonnet implements" is a split some developers already use, and it can pay off. Only one version of it has measured support, though.

- **Escalate failures.** This has the strongest evidence. When a task can be checked automatically, run it cheap first and re-run only the failures on a stronger setting. Anthropic measured this within Opus 5.5 on a SWE-bench Pro subset. Running everything at`low` and re-running failures at`high` passed about 97% of tasks for about $0.17 each. Running everything at`high` passed 95.3% for $0.29 (Optimizing for cost and intelligence). Sending Sonnet 5.5's failures on to Opus 5.5 follows the same logic, but it hasn't been measured. It needs a reliable failure signal, and every failed first attempt adds latency.
- **Hand off a written plan.** Opus 5.5 writes the plan into a file, and Sonnet 5.5 implements it in a new session. This avoids the dropped-thinking problem, because the plan travels as text.
- **`opusplan` in Claude Code.** This is the same split, built in: Opus in plan mode, Sonnet for execution. It pays a full cache re-read at each plan-mode toggle, and Anthropic hasn't published cost data for it with the 5.5 models. Pin Sonnet 5.5 explicitly outside the Anthropic API, as described above.
- **Advisor tool (API beta).** A Sonnet 5.5 executor can consult Opus 5.5 as its advisor, and the advice comes back encrypted. Opus 4.8, Opus 4.7, and Sonnet 5 advisors return a 400 with a Sonnet 5.5 executor. Anthropic's cost guide says advisors pay off when the advisor is priced "well above the executor" and actually gets consulted. Opus 5.5 is only twice Sonnet 5.5's price, and low-effort executors can stop asking. The guide's advice: "first price the advisor's model alone at low effort; that is the baseline to beat." Anthropic hasn't published a Sonnet 5.5 + Opus 5.5 advisor measurement.

## Check it on your own traffic

Launch charts and benchmark indexes can't tell you what your tasks cost at the settings you actually run. An afternoon on your own tasks gives a more reliable answer:

1. Pull 20–50 real tasks, weighted like production. Include the hardest tenth, because the cost of the tasks the cheaper model fails is what decides the bill. Anthropic's cost guide makes the same point about pricing the tail.
2. Write an outcome check for each task: tests pass, ticket closed, expected rows returned.
3. Run four arms in separate sessions: Sonnet 5.5 at `low` and`medium` , and Opus 5.5 at`low` and`medium` . Set effort explicitly. Add Sonnet 5.5 at`high` if`medium` fails too often.
4. Price every request from its `usage` block. Divide total spend, failed attempts included, by the number of tasks that passed.

Read the result against the 2x break-even. If Sonnet 5.5 at `medium` passes about as many tasks as Opus 5.5 at `medium`, it wins as long as its token use stays under twice Opus's. If it needs `high` to match, compare the per-task cost closely. If it only matches Opus at `xhigh` or above, the savings are gone: on Artificial Analysis's index, Sonnet at `xhigh` costs $4.09 per task against $2.45 for Opus at `medium`. In that case Opus 5.5 at `medium` is the simpler default. Anthropic's measurements are also "directional, not guarantees," and both models are days old, so re-run the check when prices or defaults change.

## Sonnet 5.5 vs Opus 5.5: frequently asked questions

**Is Claude Sonnet 5.5 cheaper than Opus 5.5?**
Per token, yes: half the price on every rate, including cache reads, which have cost $0.10 per million on Sonnet 5.5 against $0.20 on Opus 5.5 since October 7, 2026. Per completed task, only while Sonnet 5.5 needs less than twice the tokens Opus does. On Artificial Analysis's Intelligence Index v5.0 as of October 28, 2026, it cost 32% to 68% of Opus 5.5 per task from `low` to `xhigh`, and the same as Opus at `max`, where it scored 4 points lower.

**Does Sonnet 5.5 really beat Opus 5.5 at coding?**
On one benchmark. Anthropic reports Sonnet 5.5 at 70.6% on Terminal-Bench 4.0 against Opus 5.5's 66.4% at `xhigh`. Opus 5.5 leads on FrontierCode, CursorBench, and CodeRabbit's hard code-review cases. Anthropic itself says Opus stays stronger on open-ended work that needs sustained judgment.

**Is Opus 5.5 cheaper than Opus 5?**
Yes. Opus 5.5 is $4 / $20 per million input and output tokens, against $5 / $25 for Opus 5, and cache reads dropped 60% to $0.20. Anthropic says Opus 5.5 at default settings, where effort is now `medium` instead of Opus 5's `high`, costs 40% less than Opus 5 on typical workloads.

**Which should I pick for my Claude Code default?**
If most of your sessions are well-defined changes with tests, set Sonnet 5.5 as the default. Switch to Opus 5.5 with `/model` for design work and messy debugging, ideally at the start of a new task, because the switch re-reads the conversation without cache hits. `opusplan` automates the plan-then-build version. If most sessions are exploratory or span many files with unclear requirements, keep Opus 5.5 as the default. If you are also comparing vendors, Claude Opus 5.5 vs GPT-6 Sol: When the 2x Price Premium Pays Off covers that decision.
