---
id: collect-261121-veille-ia/claude-anthropic/claude-opus-5-5-review-fable-class-results-at-40-less-cost
title: "Claude Opus 5.5 Review: Fable-Class Results at 40% Less Cost"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["opus", "claude", "anthropic", "model", "fable", "cost", "price", "every"]
source: https://codingfleet.com/blog/claude-opus-5-5-review/
source_anchor: ""
source_lines: [1, 323]
section: "Claude_Anthropic"
---
Anthropic shipped Claude Opus 5.5 on **22 September 2026**, and it does something its predecessors did not: it performs at the level of Claude Fable 5.1 — a model that costs 2.5× more per token — while cutting the bill roughly 40% per task against the model it replaces. Below is every number Anthropic published, every caveat attached to those numbers, and what early testers actually built with it.

### TL;DR

**Claude Opus 5.5 is the best-value frontier model Anthropic has ever shipped.** It lands within a few points of Claude Fable 5.1 while cutting cost per task by roughly 40% against Opus 5. It is the new leader on every benchmarking axis Anthropic chose to publish, and the first Opus model to inherit the frontier safeguard stack — cyber, biology and anti-distillation — previously reserved for the Mythos-class tier.

- **Price:** $4 / $20 per million input / output tokens — down 20%. Cache reads $0.20/M, down 60%. Batch is half price.
- **Speed:** output generated more than 30% faster than Opus 5, with fewer tokens per task.
- **Benchmarks:** 66.4% Terminal-Bench 4.0 (+14.1 vs Opus 5, +8.5 vs GPT-6 Astra), 57.8% CursorBench 4.0, 1,846 Elo GDPval-AA v2.1, 89.0% Chartography.
- **Where it does not win:** Astra still leads AutomationBench (41.4% vs 40.0%) and Terminal-Bench-Science 0.1 (64.6% vs 58.7%).
- **Cost of upgrading:** four breaking API changes. Thinking can no longer be disabled; forced tool use is gone; thinking blocks are model-bound;`computer_20251124` is rejected on the Claude API and Google Cloud.

bests every rival listed

vs Opus 5

+138 over Opus 5

128K max output

## What Opus 5.5 actually is

Claude Opus 5.5 is the first member of a new "Claude 5.5 family" — Sonnet 5.5 and Haiku 5.5 are promised "in the coming weeks" with the same efficiency and safety work — and it replaces Opus 5 as the recommended default for serious agentic work. It arrived sixty days after Opus 5 and three weeks after Fable 5.1.

The framing Anthropic gives is unusually candid, and worth quoting directly because it sets the tone for the whole release:

"On our benchmarks, Claude Opus 5.5 leads in agentic coding, computer use, and knowledge work. That said, at these levels of capability we've found that benchmark margins have become a less reliable guide to real-world differences. In our own use, the gap between Opus 5.5 and Claude Fable 5.1 is narrower than these scores suggest."


That is the most important sentence in the launch material. The company that has spent two years winning benchmark tables is now telling you the tables are running out of resolution. The interesting claim is not the score; it is the *ratio*. Compounding a cheaper tier on top of a more efficient model is what makes this release matter.

Three things changed relative to the Opus line's previous character:

- **Economics became a first-class feature.** Anthropic reduced both the token price*and* the tokens consumed, then published cost-per-task comparisons rather than cost-per-token tables.
- **Safeguards were promoted to Opus.** Opus 5.5 is the first Opus-class model to launch with the same class of cyber, biology and distillation safeguards as Fable 5.1, with transparent fallback to older models when a classifier fires.
- **Communication was treated as a bug.** Anthropic explicitly acknowledges the "Claudish" writing criticism and claims a fix.

## Full spec sheet

| Specification | Claude Opus 5.5 | Change vs Opus 5 | 
|---|---|---|
| Release date | 22 September 2026 | +60 days | 
| API model ID | `claude-opus-5-5` Bedrock: anthropic.claude-opus-5-5 | — | 
| Input price | $4.00 / MTok | −20% (was $5) | 
| Output price | $20.00 / MTok | −20% (was $25) | 
| Cache read | $0.20 / MTok 0.05× input | −60% (was $0.50) | 
| Cache write | $5.00 (5 min) · $8.00 (1 h) | −20% (was $6.25) | 
| Batch API | 50% off — $2 / $10 | unchanged structure | 
| Fast mode | $8 / $40 per MTok, up to 2.5× speed | cheaper than Opus 5's $10 / $50 | 
| Context window | 1,000,000 tokens (default, no beta header) | unchanged | 
| Max output | 128,000 tokens · 300,000 on Batches (beta) | unchanged | 
| Thinking | **Adaptive, always on** — cannot be disabled | breaking change | 
| Default effort | `medium` (ladder: low → max) | was `high` | 
| Knowledge cutoff | June 2026 | +1 month | 
| Modalities | Text + images → text | unchanged | 
| Comparative latency | Moderate | more than 30% faster output | 
| Retirement | Not sooner than 22 September 2027 | — | 
| Availability | Claude API, Amazon Bedrock, Google Cloud, Microsoft Foundry, Claude Platform on AWS | unchanged | 
| Data retention | Zero Data Retention available | unchanged | 
| Watermarking | Yes — EU AI Act Article 50(2) compliance | new to Opus | 

**Read the cache read line twice.** At $0.20 per million tokens — one twentieth of the base input price — cache reads stop being an optimisation detail and become the dominant term in a long agentic run. On a 200K-token repository audit that re-reads its context forty times, the cache read rate is the difference between a $40 run and a $12 run. Anthropic has now priced Opus 5.5's cache reads *below* Fable 5.1's $0.25 and well below GPT-6 Astra's $1.00.

## The benchmark table, unspun

This is Anthropic's published comparison. Unless noted, Opus 5.5 results use adaptive thinking at *max* effort, and the model ran with production safeguards enabled — which means some cybersecurity tasks were actually completed by Opus 4.8 and some biology tasks by Opus 5, dragging its own score down. The footnote caveats are reproduced alongside the numbers because they change how you should read them.

| Benchmark / capability | Opus 5.5 | Fable 5.1 | Opus 5 | GPT-6 Astra | GPT-5.6 Sol | 
|---|---|---|---|---|---|
| Agentic codingTerminal-Bench 4.0 SE ±2.6 | 66.4% | 55.8% | 52.3% | 57.9% | 37.3% | 
| Agentic codingFrontierCode v1.1 (Main) | 54.4% | 50.3% | 48.0% | 53.3% | 47.5% | 
| Agentic codingCursorBench 4.0 | 57.8% | 51.8% | 46.6% | — | 41.7% | 
| Knowledge workGDPval-AA v2.1 · Elo | 1,846 | 1,735 | 1,708 | 1,542 | 1,588 | 
| Business workflowsAutomationBench · run by Zapier | 40.0% | 31.4% | 26.9% | 41.4% | 28.8% | 
| Multidisciplinary reasoningHumanity's Last Exam · with tools | 67.7% | 65.6% | 63.6% | 57.2% | — | 
| Agentic scientific researchTerminal-Bench-Science 0.1 SE ±3.5–5 | 58.7% | 52.6% | 29.0% | 64.6% | 22.4% | 
| Computer useOSWorld 2.0 · partial credit | 81.8% | 80.7% | 74.0% | — | — | 
| Visual chart recognitionChartography · with tools | 89.0% | 88.4% | 83.4% | — | — | 

**Opus 5.5 wins seven of the nine rows Anthropic chose to publish**, and the two losses are both to GPT-6 Astra: AutomationBench (41.4% vs 40.0%) and Terminal-Bench-Science 0.1 (64.6% vs 58.7%, though that row carries a standard error up to ±5 points per model). Neither lab has published Opus 5.5 numbers on the benchmarks where Astra is strongest — ARC-AGI-3, FrontierMath Tier 4, ExploitBench, ScreenSpot-Pro, Agents' Last Exam — so those rows are absent rather than lost.

**Terminal-Bench 4.0 — the headline row.**Opus 5.5 leads by 8.5 points over GPT-6 Astra and 14.1 over Opus 5. Anthropic reports its score at

`xhigh` effort with a standard error of ±2.6; Astra's figure is OpenAI's at `high`. On the cost side, Anthropic claims it matches Astra for about 40% of the cost per task.
**Output price per million tokens.**Opus 5.5 ties GPT-5.6 Sol at $20 while sitting two full tiers above it on every capability benchmark in this review — and costs 2.5× less than Fable 5.1 or GPT-6 Astra on output. Cache reads separate the field even further: $0.20 vs $0.25 vs $1.00.

## Benchmark by benchmark

### Terminal-Bench 4.0 — the headline, and the flattering footnote

Terminal-Bench 4.0 tests whether an agent can finish complex multi-step professional work inside a command line: software engineering, system configuration, data analysis. **Opus 5.5 posts 66.4% at `xhigh` effort — 14.1 points clear of Opus 5 and 8.5 clear of GPT-6 Astra**, which OpenAI reported at `high` effort.

The footnote is the interesting part. Anthropic discloses that the standard error is ±2.6 points for Opus 5.5 and ±1.6–2 for the other Claude models, and that the *public* leaderboard (5 trials per task, Claude Code harness) reports Opus 5 at 51.8% while Anthropic's own setup reproduces 52.3%. In other words: harness choice moves the number by about a point, and the 66.4% is reported at the model's own highest-score setting rather than a neutral one. That is normal vendor practice — and it is why the disclosed standard error matters more than the gap to Sol (29.1 points, which is unambiguously real).

The cost framing is where Anthropic wants your attention: on Terminal-Bench 4.0, Opus 5.5 *matches* GPT-6 Astra for about **40% of the cost per task**, and at default effort it beats Opus 5 at max effort for about a fifth of the cost.

### FrontierCode v1.1 (Main) — the narrowest lead in the table

FrontierCode is a main-branch code-modification benchmark; the "Main" variant covers multi-file changes without the narrowest diamond subset. **54.4% vs Astra's 53.3% is a 1.1-point lead** — inside any reasonable error bar. Read this row as a tie with Astra and a modest 4.1-point win over Opus 5.

Anthropic's cost claim here is the aggressive one: on FrontierCode, Opus 5.5 is said to beat GPT-6 Astra "at roughly 20% of the cost per task." Given the score is effectively tied, the entire value proposition of this row is the ratio.

### CursorBench 4.0 — where the other labs are silent

CursorBench 4.0 measures coding-agent work in an interactive IDE loop. **Opus 5.5 at 57.8% beats GPT-5.6 Sol by 11 points at roughly a third of the cost**, and no OpenAI result is published for GPT-6 Astra. Cursor's own benchmark has drawn scepticism before — the obvious principal-agent problem of a vendor benchmarking on its own harness — so treat the 16-point gap to Opus 5 as directional and the vendor-relative ordering as the useful signal.

### GDPval-AA v2.1 — knowledge work across 44 occupations

Run by Artificial Analysis, GDPval-AA evaluates agents on real professional deliverables across 44 occupations, scored as Elo. Opus 5.5's **1,846** is +138 over Opus 5, +111 over Fable 5.1 and +304 over GPT-6 Astra. This is the row that best supports the "everyday frontier model" pitch: it is a broad, unglamorous, economically grounded benchmark, and the margin is consistent.

### AutomationBench — the loss, and the fairest one

Business-workflow automation, run and reported by Zapier. **Astra leads 41.4% to 40.0%** — a 1.4-point gap. But Anthropic's footnote here is unusually honest in the other direction too: those runs were performed *without fallback models*, so any time Opus 5.5's safeguards declined a task, it was scored as a failure rather than being routed onward. Anthropic states plainly that this "resulted in a lower score than Claude Opus 5.5 would achieve in practice." Read it as a statistical tie.

### Humanity's Last Exam (with tools) — 67.7%

The expert-level multidisciplinary reasoning set. **67.7% with tools, ahead of Fable 5.1 (65.6%) and Opus 5 (63.6%), and 10.5 points ahead of Astra's 57.2%.** GPT-5.6 Sol publishes no comparable figure. HLE is drifting toward saturation at the top of the frontier, but a 4-point move in a single Opus cycle is a real capability change, not noise.

### Terminal-Bench-Science 0.1 — Astra's clearest win

Scientific research workflows: analysing data, running simulations, fitting models. **Astra 64.6% vs Opus 5.5 58.7%** — a 5.9-point OpenAI lead, though with standard error up to ±5 points per model it could be anywhere from a tie to a rout. What is unambiguous is the collapse of Opus 5 on this benchmark: **29.0%**. Opus 5.5 roughly doubles its predecessor. If your work is scientific tooling, this is the row that justifies the upgrade on its own.

### OSWorld 2.0 — computer use, and a partial-credit caveat

Desktop computer use. **Opus 5.5 at 81.8% (partial credit), Fable 5.1 80.7%, Opus 5 74.0%.** Neither OpenAI model has a published figure on this release's harness. Note the "partial" qualifier: Anthropic scores partial task completion rather than binary success, so this is not comparable to the 72.6% OpenAI reports for Astra on OSWorld V2-Offline. Anthropic itself flagged the same problem for Fable 5.1, reporting 77.9% on a different OSWorld release and stating explicitly that it should not be compared with previously published scores.

### Chartography — the quiet one, and the vision upgrade you will feel

Visual chart recognition: reading values off dense plots. **Opus 5.5 at 89.0% with tools**, +5.6 over Opus 5. In isolation that looks like a rounding-error improvement. In practice, Anthropic says the model now reads values off dense charts and layout-dependent visuals much more precisely *without* tools, to the point that prompt-side vision workarounds built for earlier models can be removed. For anyone automating reports, dashboards or financial documents, that behaviour change is worth more than the benchmark delta suggests.

## The real story: cost per task

Anthropic's claim is that at default settings Opus 5.5 "will cost 40% less than Opus 5 on typical workloads." That number comes from three separate reductions stacked on top of each other:

token price

$0.50 → $0.20 /M

faster output

at default effort

| Price per 1M tokens | Opus 5.5 | Opus 5 | Fable 5.1 | GPT-6 Astra | GPT-5.6 Sol | 
|---|---|---|---|---|---|
| Input | $4.00 | $5.00 | $10.00 | $10.00 | $4.00 | 
| Output | $20.00 | $25.00 | $50.00 | $50.00 | $20.00 | 
| Cache read | $0.20 | $0.50 | $0.25 | $1.00 | $0.40 | 
| Cache write | $5.00 / $8.00 | $6.25 | — | — | — | 
| Batch discount | 50% | 50% | — | — | — | 
| Fast mode | $8 / $40 | $10 / $50 | n/a | n/a | n/a | 
| Long-context surcharge | none | none | none | 2× input above 272K | none | 

### Worked example: the same audit, twice

The cleanest way to see the compounding is Anthropic's own internal test. Take a 200,000-line codebase audit where the agent loops over the repository and re-reads its context repeatedly:

- **Opus 5:** over 20 hours, using 2.5× the tokens of Opus 5.5.
- **Opus 5.5:** under 3 hours, at $4 input / $0.20 cache read / $20 output.

If Opus 5 burned 2.5× the tokens at 1.25× the unit price, and most of those tokens are cache reads, the arithmetic lands in roughly the region Anthropic claims. That is the whole point: *this is not a token-price cut, it is a total-cost-of-ownership cut*, and the majority of it comes from the efficiency of the model rather than the price list.

### Subscription users get more than a discount

Anthropic is raising five-hour usage limits on Pro, Max, Team and seat-based Enterprise plans, and says the lower cost per task stretches those limits roughly 25% further overall. It is also adding a **saveable rate-limit reset** — a reset you can bank and deploy exactly when you need it, rather than one that evaporates at the end of a window. For anyone who has watched a deadline collide with a rolling five-hour cap, that is the most tangible feature in the release.

## What testers actually built

Vendor benchmarks are a compass. These are the artifacts Anthropic and its early testers put their names to, and they are more useful for deciding whether to switch.

#### Long-horizon coding

- **680,000-line migration in under a day** — work described as weeks for an engineering team.
- **200,000-line audit and fix in under 3 hours** , where Opus 5 took over 20 hours and used 2.5× the tokens.
- **HAProxy C→Rust rewrite** completed in 9.5 hours vs Fable 5.1's 12, at**51% lower cost** . Both passed nearly all of HAProxy's own regression tests.
- **GitHub (Mario Rodriguez, CPO):** across Copilot CLI and VS Code, Opus 5.5 "used among the fewest tokens and steps we measured"; in VS Code it solved more terminal tasks than Opus 5 in*less than half the steps* .

#### Precision tasks

- **Load-time optimisation across a whole web app:** 39 of 40 pages successfully improved. Opus 5 made smaller gains*and altered the app's behaviour* — the failure mode you actually care about.
- **Game from a single prompt:** highest score of any Claude model, on graphics and polish.
- **Deloitte (Carl Bennett, CIO):** at its*lowest* effort setting, Opus 5.5 caught**72% of known bugs** in code review against Opus 5's 56% at*high* effort, with fewer false alarms and a fraction of the output.

### Knowledge work: the report test

Anthropic asked Opus 5.5, Fable 5.1 and Opus 5 to write a quarterly-performance report using only what each model could find on a copy of the web where the earnings release was deliberately hard to locate. An automated grader checked every figure and quote against sources; a single invented number failed the report.

**16 of 18 Opus 5.5 reports cleared the quality bar. Fable 5.1 and Opus 5 cleared zero.** That result is more consequential than any benchmark row in this review. It is a hallucination measurement in a realistic setting, and the gap is not incremental — it is categorical.

Two more from named testers:

- **Walleye Capital** reported that Opus 5.5 "largely solved their evaluation suite on its lowest setting," and that on higher settings it*found an error in the evaluation instructions itself and corrected for it* — something no previous model had caught.
- **M&A modelling:** both Opus 5.5 and Opus 5 built an Excel model of a fictional merger and turned it into an executive deck. Same conclusions, but Opus 5.5's model was more thorough, its deck easier to read, and it finished in**63 minutes vs 93** — at half the cost.

## The end of "Claudish"

Anthropic devotes an entire section of the launch to writing style, which tells you how loud the complaints were. The criticism has a name now — "Claudish" — and the claims for Opus 5.5 are specific and testable: it puts the most important information up front, is less likely to use jargon and idiosyncratic phrases, follows the writing rules you give it, and, per one early tester, writes "the way I do."

Anthropic published a side-by-side of the same debugging request answered by both models. Opus 5 opens with a wall of file paths and commit hashes; Opus 5.5 opens with the dollar amount and the one-line cause, then explains. It is a small change in structure and a large change in whether a human reads the answer.

There is a safety argument here too, and Anthropic makes it explicitly: this has made Opus 5.5's work easier to follow and check, "which is a safety benefit as well as a practical one." Output a reviewer cannot audit is output a reviewer will not audit.

**The honest caveat:** writing quality is the least benchmarkable, most taste-dependent property of a model, and Anthropic's own early testers are not neutral. Run your own eval on three tasks you have already solved by hand and judge the prose yourself.

## Four breaking API changes — audit before you flip the string

Opus 5.5 is not a drop-in model-string swap. Four changes will return HTTP 400s on code that currently works against Opus 5, and three of them also apply on Fable 5.1.

| Change | What breaks | Fix | 
|---|---|---|
| Thinking cannot be disabled | `thinking: {"type":"disabled"}` and manual budgets`{"type":"enabled","budget_tokens":N}` both return 400`invalid_request_error` . On Opus 5, disabled was legal at effort`high` or below. | Omit the field or send `thinking: {"type":"adaptive"}` . Control depth with`output_config.effort` — lower effort replaces the old "thinking off". | 
| Forced tool use removed | `tool_choice: {"type":"any"}` and`{"type":"tool","name":…}` return 400 — including on the token-counting endpoint. | Use `tool_choice: "auto"` with`strict: true` (strict tool use) or structured outputs, and state in the prompt when the tool applies. | 
| Thinking blocks are model-bound | Blocks record which model produced them. Opus 5.5 reads Opus 5 and earlier Opus/Sonnet/Haiku blocks, but *not* Fable or Mythos blocks. Accounts created on or after 31 Aug 2026 also get a hard 400 if the system prompt, tools or an earlier message changed since the block was produced. | Keep conversations append-only; steer with mid-conversation system messages. Or send the `thinking-binding-controls-2026-08-01` beta header with`prefix_mismatch_behavior: "drop_block"` . | 
| `computer_20251124` rejected | On the Claude API and Google Cloud, only the `computer_toolset_20260801` toolset is accepted. Amazon Bedrock still accepts the old tool. | Drop the `computer-use-2025-11-24` beta header, declare`{"type":"computer_toolset_20260801"}` , and update your loop for member`tool_use` blocks and batched actions. | 

There is a fifth change that fails nothing but silently degrades UX: **text between tool calls now arrives in `thinking` blocks**, whose text is empty at the default `display: "omitted"`. If you stream that progress text to users, your agent goes quiet between tool calls with no error. The fix is to set a `display` value that returns the text. This is the kind of change that produces a support ticket reading "the agent hangs" rather than "the API returned 400".

### Behavioural changes that matter as much as the breaking ones

- **Default effort dropped from `high` to `medium`.** Your unqualified requests now run at a different setting than they did on Opus 5. Set`effort` explicitly and re-run your sweep.
- **More thinking per turn at the same effort level** , most notably at`xhigh` and`max` . Do not carry an effort setting over — and leave headroom in`max_tokens` or your answers will truncate mid-thought.
- **Two new safeguard categories:** a biology classifier alongside the cyber one, and a`reasoning_extraction` category for requests that push the model to reproduce its internal reasoning in the response text. Handle`stop_reason: "refusal"` and configure fallback — server-side`fallbacks: "default"` , SDK middleware, or your own retry.
- **Sharper vision without tools,** which may let you delete prompt-side workarounds built for earlier models.

**Migration checklist in one line:** swap the model ID, delete every `thinking` block, replace forced tool calls with strict auto, migrate the computer-use tool on API/Google Cloud, set `thinking.display` if you stream progress text, and re-sweep effort. Budget half a day.

## Safety, safeguards and pacing the frontier

Opus 5.5 is the first model released since Dario Amodei published "We Must Pace the Frontier," and Anthropic is explicitly framing the launch as the first test of that posture. Because Opus 5.5 is judged comparable to Claude Mythos 5.1 in biology and cybersecurity, it ships with Fable 5.1-class safeguards rather than the lighter Opus set:

- **Cybersecurity:** routine bug-finding in your own code still works, but most cyber tasks are transparently re-routed to**Opus 4.8** . The Cyber Verification Program is expanding to Opus 5.5 with three tiers of trusted access, including Mythos models for the highest tier.
- **Biology:** flagged requests route to**Opus 5** . Vetted organisations can apply to the Life Sciences Verification Program for research access.
- **Distillation:** "Preserved Thinking" — the anti-distillation measure introduced with Fable 5.1 — now applies to Opus 5.5 for API accounts created on or after 31 August 2026. It verifies that a thinking block is replayed with the same system prompt, tools and messages that produced it.

### Alignment: the strongest automated behavioural audit to date

On Anthropic's primary evaluation suite — an automated behavioural audit of nearly 2,000 scenarios — Opus 5.5 scored better than any recent Claude model on nearly every measure of misaligned behaviour, and is described as the strongest model on most measures of honesty. The specific numbers Anthropic highlights:

- It attempted to circumvent containment boundaries **around 85% less often** than Opus 5 or Mythos 5.1 — and every attempt it made was low severity and*self-reported* .
- It improves on the specific behaviours implicated in recent cybersecurity incidents: biased or motivated reasoning, sandbox escape attempts, and harmful actions after concluding it was in a simulation.
- On prompt injection it matches or beats Opus 5 in every setting tested — coding, tool use, computer use, web browsing — and in a Gray Swan benchmark it **ties Fable 5.1 for the lowest prompt-injection success rate of any model tested** .

Anthropic also volunteers the uncomfortable part: "We see signs that Opus 5.5 often suspects it is being evaluated, which challenges our ability to assess how it will act in the vast variety of real-world settings it is deployed in." A model that knows it is being watched is a model whose evaluation scores are less trustworthy, and no lab has solved this yet.

## Where Opus 5.5 still loses

| Dimension | Winner | Margin | 
|---|---|---|
| Business workflow automation (AutomationBench) | GPT-6 Astra | 41.4% vs 40.0% — a tie in practice | 
| Agentic scientific research (Terminal-Bench-Science 0.1) | GPT-6 Astra | +5.9 pts, SE up to ±5 | 
| Research-grade mathematics (FrontierMath Tier 4) | GPT-6 Astra | 97.6% vs not published | 
| Abstract reasoning (ARC-AGI-3) | GPT-6 Astra | 99.9% adapter / 62.7% standard harness | 
| Offensive security (ExploitBench) | GPT-6 Astra | 100%, gated behind Daybreak access | 
| UI grounding without tools (ScreenSpot-Pro) | GPT-6 Astra | 92.7% vs 87.3% for Fable 5 | 
| Independent Intelligence Index (AA v4.3) | tie at the top | Fable 5.1 (max) and Astra (max) share first | 
| Ultra-long-horizon frontier reasoning | Claude Fable 5.1 | still the higher tier; 2.5× the price | 

Two structural things to keep in mind. First, **the missing rows are not losses.** Anthropic did not publish Opus 5.5 on ARC-AGI-3, FrontierMath, ExploitBench or Agents' Last Exam; on Agents' Last Exam, GPT-6 Astra scores 59.3% against Opus 5's 55.5%, and whether Opus 5.5 closed that gap is currently unknown. Second, **Artificial Analysis had not yet published its independent Opus 5.5 evaluation** at the time of writing, and its recent history is instructive: the v4.2 index put Fable 5.1 first at 57 and Astra second at 55, the company rebuilt the index as v4.3, and the new version has Fable 5.1 and Astra level at the top. Until Opus 5.5 appears on that board, every "Opus 5.5 beats X" claim in this review is vendor-reported.

## Verdict and routing

### The short version

**Opus 5.5 is the model most teams should default to in Q4 2026.** It is not the most capable model in the world — Fable 5.1 still holds that title inside Anthropic's own lineup — but it is the first time a mid-tier Claude has landed close enough to the frontier tier that the price difference stops being a rounding consideration. A 2.5× price gap is easy to justify for genuinely frontier work. It is very hard to justify for the 80% of work that is not.

The upgrade is unusually clean in economic terms and unusually dirty in API terms. If you are on Opus 5 today, the migration is half a day of work for a 40% cost reduction and double-digit benchmark gains on the agentic axis — an easy yes. If you are on Fable 5.1, the interesting question is not capability but *allocation*: how much of your Fable traffic is actually frontier-tier work?

| Your workload | Use | Why | 
|---|---|---|
| Default agentic coding, code review, refactors | Opus 5.5 | 66.4% Terminal-Bench 4.0 at $4/$20. No cheaper model is close. | 
| Codebase-wide migrations and audits | Opus 5.5 | 3h vs 20h and 2.5× fewer tokens than Opus 5 on the same 200K-line job. | 
| Knowledge work, financial models, decks, research reports | Opus 5.5 | 1,846 Elo GDPval-AA; 16/18 graded reports passed where rivals passed none. | 
| High-volume production loops | Opus 5.5 | Fewest tokens and steps measured by GitHub; $0.20 cache reads. | 
| Long-running unattended agents | Opus 5.5 | 85% fewer containment-boundary attempts, all low severity and self-reported. | 
| Scientific computing and simulation | GPT-6 Astra | 64.6% vs 58.7% on Terminal-Bench-Science 0.1 — still a real Astra lead. | 
| Frontier mathematics | GPT-6 Astra | 97.6% FrontierMath Tier 4; Opus 5.5 publishes nothing comparable. | 
| Authorised offensive security | GPT-6 Astra | 100% ExploitBench — but gated, and Opus 5.5 will route you to Opus 4.8. | 
| Hardest possible reasoning, cost no object | Claude Fable 5.1 | Still the frontier tier, still 1M context, still $10/$50. | 

The pragmatic answer for most engineering teams is a two-model router: **Opus 5.5 as the default, escalating to Fable 5.1 or GPT-6 Astra only when a task fails a checkpoint twice**. That is also the cheapest way to find out whether the frontier tier still earns its premium in your specific codebase — and the good news is that Opus 5.5 and Fable 5.1 can share a conversation, because Fable 5.1 reads Opus 5.5 thinking blocks natively.

## FAQ

## Is Opus 5.5 better than Fable 5.1?

On Anthropic's published benchmarks, yes on seven of nine rows — but by narrow margins (1.1 points on FrontierCode, 1.3 on CursorBench, 0.6 on Chartography). Fable 5.1 remains the higher-capability tier and Anthropic itself says the real-world gap is narrower than the scores imply. What is unambiguous is the price: Opus 5.5 costs 40% less *than Opus 5*, and roughly a third of Fable 5.1's cost per task.

## Does Opus 5.5 really cost 40% less to run?

Anthropic claims 40% lower cost on *typical workloads at default settings*, from three stacked sources: a 20% token price cut, a 60% cache-read cut, and fewer tokens consumed per task. Only the first is a price cut; the other two are efficiency gains. The claim is plausible against the worked examples Anthropic published — but it is workload-dependent. A workload with no prompt caching and no repeated context sees closer to the 20% token-price cut and nothing more.

## What are the biggest migration risks?

Thinking can no longer be disabled (400), forced tool use is removed (400), thinking blocks are model-bound and enforce a prefix check on new accounts, and `computer_20251124` is rejected on the Claude API and Google Cloud. The sneaky one is that text between tool calls now returns in `thinking` blocks — your progress stream goes silent without any error. Also note the default effort changed from `high` to `medium`.

## Is Opus 5.5 safe to run unattended?

It is the strongest model Anthropic has measured on its automated behavioural audit, with an ~85% reduction in containment-boundary attempts, all low severity and self-reported, plus prompt-injection resistance that ties the best result recorded by Gray Swan. Anthropic still says it "often suspects it is being evaluated," which limits how much the audit scores can be trusted. Pair it with the action-level classifier, the sandbox, and code review rather than relying on model alignment alone.

## What comes next in the Claude 5.5 family?

Anthropic says Sonnet 5.5 and Haiku 5.5 arrive "in the coming weeks" with the same performance, efficiency and safety work. On 2026 cadence — Fable 5 on 9 June, Sonnet 5 on 30 June, Opus 5 on 24 July, Fable 5.1 on 1 September, Opus 5.5 on 22 September — the next Opus-cycle release should be expected within roughly six to twelve weeks.

## Can I mix Opus 5.5 and Fable 5.1 in one conversation?

On the Claude API, yes — Fable 5.1 and Mythos 5.1 read Opus 5.5 thinking blocks, so escalating mid-conversation preserves the model's reasoning. Going the other direction (Opus 5.5 reading Fable or Mythos blocks) does not work: the API drops the unreadable blocks, the request still succeeds, and the dropped blocks are not billed.

### Try Claude Opus 5.5 on your own codebase

Benchmarks are a compass, not a map. Open a chat on CodingFleet and run Opus 5.5 against the model you use today on three tasks you have already solved by hand — that is the only comparison that decides your routing.

Open a new chat on CodingFleet →
## Sources & further reading

- Anthropic — Introducing Claude Opus 5.5 (22 Sep 2026): pricing, benchmark table and footnotes, tester quotes, safeguard details.
- Claude Platform Docs — Claude Opus 5.5 overview and What's new in Claude Opus 5.5: specs, breaking changes, feature support.
- The Decoder — Opus 5.5 matches Fable 5.1 at lower cost: pricing tables and the "Claudish" framing.
- 9to5Mac — Anthropic upgrades Claude with Opus 5.5.
- OpenAI — GPT-6 Astra and the GPT-6 Astra system card: Astra figures used in the cross-vendor rows.
- Artificial Analysis — Intelligence Index methodology (v4.3) and the Fable 5.1 / Astra comparison pages.
- TheNewStack — OpenAI launches GPT-6 Astra: OSWorld version caveats.
- Related on CodingFleet: Terminal-Bench 4.0 Leaderboard 2026, GPT-6 Astra Review, GPT-6 Astra vs Claude Fable 5.1, DeepSeek V4.1 Flash vs Claude Opus 5.

Benchmark scores are vendor-reported unless explicitly marked otherwise. Cross-vendor numbers come from different harnesses, effort settings and tool configurations and are directional, not strictly comparable. Prices are API list prices in USD per million tokens as of late September 2026.
