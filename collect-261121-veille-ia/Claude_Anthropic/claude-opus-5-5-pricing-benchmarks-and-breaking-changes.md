---
id: collect-261121-veille-ia/claude-anthropic/claude-opus-5-5-pricing-benchmarks-and-breaking-changes
title: "Claude Opus 5.5: Pricing, Benchmarks and Breaking Changes"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["opus", "anthropic", "claude", "output", "cache", "changes", "input", "anthropics"]
source: https://www.digitalapplied.com/blog/claude-opus-5-5-launch-pricing-benchmarks-2026
source_anchor: ""
source_lines: [1, 204]
section: "Claude_Anthropic"
---
Anthropic released Claude Opus 5.5 on September 22, 2026. It is the first model in the Claude 5.5 family, priced at $4 per million input tokens and $20 per million output tokens (Opus 5: $5 and $25), with cache reads cut from $0.50 to $0.20. Anthropic says it performs “at the level of Claude Fable 5.1 on most work” and costs 40% less to run than Opus 5 on typical workloads.

Both claims hold up on Anthropic’s own published numbers, but they are measured at different settings, and the difference matters for anyone planning a migration. The 40% saving is measured at each model’s default effort, which is `medium` on Opus 5.5 and `high` on Opus 5. Most rows in the headline benchmark table are measured at `max` effort. And four API changes will return errors on code that runs cleanly on Opus 5 today.

Every figure in this post is Anthropic’s own, or a partner’s as quoted by Anthropic. None has been independently replicated at the time of writing.

1. 01Per-token prices fell 20%, and cache reads fell 60% to $0.20 per million tokens.Input $4, output $20, five-minute cache writes $5, one-hour writes $8, and $2 / $10 on the Batch API.
2. 02The 40% saving compares default with default: Opus 5.5 at medium against Opus 5 at high.Opus 5.5 thinks more per turn than Opus 5 at the same named effort level, so an integration that pins high or above will not see the headline figure.
3. 03More effort is not always better on coding, and Opus 5.5 does not lead everywhere.On Anthropic's charts, max is not the top score on Terminal-Bench 4.0 or FrontierCode. GPT-6 Astra scores higher on AutomationBench and Terminal-Bench-Science.
4. 04Four breaking changes, plus a fifth that fails silently. Fable 5.1 users have the most to gain.Opus 5.5 lists at 40% of Fable 5.1's per-token price, and Anthropic says the gap between the two is narrower than the benchmarks suggest.

## 01 — The releaseWhat shipped

- API IDAmazon Bedrock uses anthropic.claude-opus-5-5.
- claude-opus-5-5
- Context / max outputUp to 300K output on the Batch API with the output-300k-2026-03-24 beta header.
- 1M / 128K
- Knowledge cutoffPer Anthropic's model page.
- June 2026
- ThinkingAdaptive thinking is always on; effort controls depth.
- Default: medium
- PlatformsClaude API, Amazon Bedrock, Claude Platform on AWS, Google Cloud, Microsoft Foundry.
- 5
- RetirementEarliest date Anthropic commits to on the model page.
- ≥ Sep 22, 2027

Two other launch details affect planning. According to the announcement, Claude Sonnet 5.5 and Claude Haiku 5.5 will follow “in the coming weeks.” Anthropic is also raising five-hour usage limits on the Pro, Max and Team plans, and subscribers get a one-off rate-limit reset that they can save and use whenever they choose.

Anthropic’s ClaudeDevs account put numbers on the subscription change the same day. In Claude Code, five-hour session limits rise 20% from September 22, and because Opus 5.5 is priced lower, it goes about 25% further within those limits. The same post describes Opus 5.5 as about 30% faster and about 40% cheaper per task than Opus 5.

The release also has a policy context. It is Anthropic’s first model since its CEO argued the frontier should be paced, and METR and Frontier Design evaluated it before launch.

## 02 — The invoiceThe price, line by line

| Per million tokens. Source: Anthropic pricing and fast-mode documentation, September 22, 2026. Batch rates apply the documented 50% discount; percentage changes are our arithmetic. |  |  |  | 
|---|---|---|---|
| Line | Opus 5.5 | Opus 5 | Fable 5.1 | 
|---|---|---|---|
| Input | $4 (−20%) | $5 | $10 | 
| Output | $20 (−20%) | $25 | $50 | 
| Cache write, 5 minutes | $5 (−20%) | $6.25 | $12.50 | 
| Cache write, 1 hour | $8 (−20%) | $10 | $20 | 
| Cache read | $0.20 (−60%) | $0.50 | $0.25 | 
| Batch API, input / output | $2 / $10 | $2.50 / $12.50 | $5 / $25 | 
| Fast mode, input / output | $8 / $40 | $10 / $50 | — | 

The cache-read line is the structural change. According to Anthropic’s pricing page, a cache hit costs 10% of the input price on most Claude models. On Opus 5.5 it costs 5%, and on Fable 5.1 it costs 2.5%. The announcement says cache reads “make up the majority of agentic and coding work costs,” so the 60% cut lands on the biggest part of an agent’s bill. The minimum cacheable prompt stays at 512 tokens, the same as Opus 5 and Fable 5.1.

Fast mode (research preview, Claude API only) runs Opus 5.5 at up to 2.5× the output speed for 2× the standard price. It is not available on Bedrock, Claude Platform on AWS, Google Cloud or Microsoft Foundry. US-only inference (`inference_geo: "us"`) adds a 1.1× multiplier to every rate above, fast mode included.

A lower per-token price does not cut every bill by the same percentage, because each token type fell by a different amount. The table below is an illustrative agent task. The token counts are invented for the example; the rates are Anthropic’s published prices.

| Illustrative token counts, published rates as of September 22, 2026. Same token counts on all three models. |  |  |  | 
|---|---|---|---|
| Illustrative task | Opus 5 | Opus 5.5 | Fable 5.1 | 
|---|---|---|---|
| Cache reads · 8,000,000 tokens | $4.00 | $1.60 | $2.00 | 
| Uncached input · 400,000 tokens | $2.00 | $1.60 | $4.00 | 
| Cache writes, 5 min · 600,000 tokens | $3.75 | $3.00 | $7.50 | 
| Output incl. thinking · 300,000 tokens | $7.50 | $6.00 | $15.00 | 
| Total | $17.25 | $12.20 | $28.50 | 

With identical token counts, the price change alone saves about 29% on this cache-heavy task. A workload with no caching saves exactly 20%. The rest of Anthropic’s 40% has to come from Opus 5.5 using fewer tokens per task, which is its stated second source of savings (“it costs less per token than Opus 5 and uses fewer tokens per task”). That part depends on your workload and your effort setting, so only your own measurements can confirm it.

## 03 — The settingsTwo claims measured at *two different settings*

This is the most useful thing to understand before switching models.

**The cost claim compares default settings.** Anthropic’s wording is “at default settings it will cost 40% less than Opus 5 on typical workloads.” Opus 5.5 defaults to `medium`; Opus 5 defaults to `high`. The prompting guide says that in Anthropic’s testing, “Claude Opus 5.5 at `medium` matches or exceeds Claude Opus 5 at `high` on coding and knowledge-work evaluations.” So the comparison is between Opus 5.5 at a lower effort level and Opus 5 at a higher one, delivering the same or better results.

**The headline table uses max effort.** Its footnote reads: “Unless otherwise noted, all Claude Opus 5.5 results use adaptive thinking at max effort.” The only exception is Terminal-Bench 4.0, reported at `xhigh` because that was Opus 5.5’s best score on it. So the 1846 GDPval-AA score and the 40% lower bill come from different settings.

**Keeping your old effort setting raises the cost.** Anthropic’s What’s new page says that “at the same effort setting the model tends to think more per turn than Claude Opus 5, most of all at `xhigh` and `max`.” A team that pinned `effort: "high"` on Opus 5 and only changes the model ID will get a model that thinks more than it used to at that setting. That team gets the per-token cut and may lose some or all of the token savings.

What to do: remove any effort value inherited from Opus 5, start at `medium`, and compare two or three levels on your own traffic. Anthropic also recommends leaving `max_tokens` large enough for thinking (it suggests 128,000, the model’s maximum, for long agentic turns) and using `xhigh` and `max` only where you have measured a quality gain.

In our own use, the gap between Opus 5.5 and Claude Fable 5.1 is narrower than these scores suggest.Anthropic, Introducing Claude Opus 5.5, September 22, 2026

## 04 — The numbersThe benchmarks, and where Opus 5.5 does not lead

Every figure below comes from Anthropic’s comparison table. The last column shows the highest score among the other four models Anthropic compared: Fable 5.1, Opus 5, GPT-6 Astra and GPT-5.6 Sol. Where GPT-6 Astra is shown, Fable 5.1 scored 55.8% on Terminal-Bench 4.0, 50.3% on FrontierCode, 31.4% on AutomationBench and 52.6% on Terminal-Bench-Science.

| Source: Anthropic, Introducing Claude Opus 5.5, September 22, 2026. Opus 5.5 at max effort except Terminal-Bench 4.0 (xhigh). |  |  |  | 
|---|---|---|---|
| Benchmark | Opus 5.5 | Best other | Opus 5 | 
|---|---|---|---|
| Terminal-Bench 4.0 (agentic coding) | 66.4% | 57.9% (GPT-6 Astra) | 52.3% | 
| FrontierCode v1.1 Main (agentic coding) | 54.4% | 53.3% (GPT-6 Astra) | 48.0% | 
| CursorBench 4.0 (agentic coding) | 57.8% | 51.8% (Fable 5.1) | 46.6% | 
| GDPval-AA v2.1 (knowledge work, Elo) | 1846 | 1735 (Fable 5.1) | 1708 | 
| AutomationBench (business workflows) | 40.0% | 41.4% (GPT-6 Astra) | 26.9% | 
| Humanity's Last Exam, with tools | 67.7% | 65.6% (Fable 5.1) | 63.6% | 
| Terminal-Bench-Science 0.1 | 58.7% | 64.6% (GPT-6 Astra) | 29.0% | 
| OSWorld 2.0, partial (computer use) | 81.8% | 80.7% (Fable 5.1) | 74.0% | 
| Chartography, with tools (chart reading) | 89.0% | 88.4% (Fable 5.1) | 83.4% | 

Five caveats come from Anthropic’s own footnotes, and each one affects how the table should be read:

1. **Margins sit close to the noise.** Anthropic gives Terminal-Bench 4.0 a standard error of ±2.6 points for Opus 5.5, and ±3.5–5 points per model on Terminal-Bench-Science. A 1.1-point lead on FrontierCode over GPT-6 Astra is not a meaningful difference.
2. **OpenAI’s numbers come from OpenAI.** The GPT-6 Astra and GPT-5.6 Sol figures on Terminal-Bench are “as reported by OpenAI,” so they were not run in Anthropic’s harness.
3. **Safeguards affected the scores.** Opus 5.5 was evaluated with its production safeguards on. When they intervened, cybersecurity tasks were completed by Claude Opus 4.8, and biology and frontier-LLM-development tasks by Opus 5. Anthropic says this “likely reduces” Opus 5.5’s scores.
4. **AutomationBench is Zapier’s run.** Zapier ran it without fallback models, so every safeguard intervention counted as a failure. The Opus 5.5 result comes from Zapier’s early-access evaluation; the other models’ results come from its public leaderboard.
5. **Anthropic discounts its own table.** “At these levels of capability we’ve found that benchmark margins have become a less reliable guide to real-world differences.”

In short, Opus 5.5 leads on coding, knowledge work and computer use by margins that range from clear (Terminal-Bench, GDPval-AA) to within noise (FrontierCode, OSWorld, Chartography). It trails GPT-6 Astra on two agentic benchmarks.

Anthropic also published score-against-cost curves for each effort level on four benchmarks. Their shapes show where extra effort helps.

| Read from Anthropic’s four effort-level charts, September 22, 2026. Shapes only; no values are read off the charts. |  |  | 
|---|---|---|
| Benchmark | Highest-scoring effort | Does max beat medium? | 
|---|---|---|
| Terminal-Bench 4.0 | xhigh | Yes, but max scores below xhigh at a higher cost | 
| FrontierCode v1.1 | medium, about level with max | No. Medium is roughly level at a fraction of the cost; xhigh dips below both | 
| GDPval-AA v2.1 | max | Yes. The curve keeps climbing | 
| AutomationBench | max | Yes. The curve keeps climbing | 

Anthropic’s text states the cost comparisons directly. At default effort, Opus 5.5 beats GPT-6 Astra on FrontierCode at “roughly 20% of the cost per task” and matches Astra on Terminal-Bench 4.0 for “about 40% of the cost.” At default effort on GDPval-AA, it beats Astra at max effort “for about a fifth of the cost per task.” On CursorBench, it beats GPT-5.6 Sol by 11 points “for about a third of the cost.”

The practical reading is that coding agents may be best served at `medium` or `high`, with a re-run at higher effort only when a task fails. Research, analysis and document work gain more from higher effort. Anthropic’s cost-optimisation guide reached the same conclusion with earlier models: where results can be checked, the cheapest option is often to run at a low setting and re-run only the failures at a higher one. A partner result points the same way. In the announcement, Deloitte’s CIO says that “even at its lowest effort setting,” Opus 5.5 caught 72% of known bugs in their code reviews, against Opus 5’s 56% at high effort.

**An independent check from Vals AI.** The evaluation firm Vals AI ran Opus 5.5 through its own benchmark suite on launch day, at max effort. It found large gains on long, iterative agent work and smaller losses on retrieval-heavy professional tasks, which Vals attributes to retrieval, fidelity and rubric-following.

| Source: Vals AI model page and launch posts, September 22, 2026. Opus 5.5 run with Opus 5 and Opus 4.8 as refusal fallbacks. Change in percentage points. |  |  |  | 
|---|---|---|---|
| Vals AI benchmark | Opus 5 | Opus 5.5 | Change | 
|---|---|---|---|
| Terminal-Bench 4.0 | 45.5% | 61.6% | +16.1 | 
| Terminal-Bench Science | 22.9% | 48.6% | +25.7 | 
| ProgramBench, fully resolved | 3.0% | 18.5% | +15.5 | 
| MysteryMechanism | 37.4% | 49.5% | +12.1 | 
| IOI | 84.3% | 95.1% | +10.8 | 
| MedCode | 63.6% | 49.8% | −13.8 | 
| Public Benefits Bench | 76.9% | 70.6% | −6.3 | 
| Legal Research Bench | 55.3% | 50.5% | −4.8 | 
| Tax Agent Bench | 75.1% | 70.5% | −4.6 | 
| Harvey Legal Agent Benchmark | 6.7% | 3.8% | −2.9 | 

On ProgramBench, which asks a model to rebuild a command-line program from its binary, Vals says Opus 5.5 fully solved more than six times as many tasks as Opus 5 at the same effort, in 2.4 hours and $42.66 per task against 2.9 hours and $60.29. It also tied for first on ProofBench at 100%. Three caveats apply. First, counting fallback-assisted tasks as failures lowers the Terminal-Bench 4.0 score from 61.6% to 53.5%. Second, on SRE Bench 83% of tasks were fallback-assisted, so that result is left out here. Third, on Vals’ broad index across all its domains, Opus 5.5 ranks fourth of 60 at 66.16%, just below Opus 5 at 67.21%. If your agents do legal research, medical coding or tax work, test those workloads before you switch.

## 05 — The migrationThe four breaking changes, and a fifth that fails silently

The What’s new page lists four changes that break code running on Opus 5. The first three also apply to Fable 5.1, so teams that already moved to Fable have done most of this work.

| Source: Anthropic, What’s new in Claude Opus 5.5, September 22, 2026. |  |  | 
|---|---|---|
| Change | What happens on Opus 5.5 | The fix | 
|---|---|---|
| Thinking can't be disabled | thinking: {"type": "disabled"} or a manual budget_tokens returns a 400 invalid_request_error | Omit thinking or send {"type": "adaptive"}; control depth with effort, starting at low where you used to disable thinking | 
| No forced tool use | tool_choice of any or tool returns a 400, on the token-counting endpoint too | Keep auto and add strict: true, or use structured outputs; say in the prompt when a tool applies | 
| Thinking blocks bound to model and conversation | Replaying a thinking block after the system prompt, tools or an earlier message changed returns a 400, by default for accounts created on or after August 31, 2026 | Keep conversations append-only; change instructions with mid-conversation system messages, not edits | 
| computer_20251124 rejected (Claude API and Google Cloud) | Declaring the older computer-use tool returns a 400 | Move to computer_toolset_20260801; Amazon Bedrock still accepts the old tool | 

Short notes the model writes between tool calls now come back as `thinking` blocks rather than `text` blocks. At the default `display: "omitted"`, those blocks are empty. An app that streams the notes to users as progress updates will show nothing between tool calls, and no error is raised. The fix is to set a `thinking.display` value that returns the text.

The model-switching rule matters for any router. Opus 5.5 can read thinking blocks from Opus 5 and earlier Opus, Sonnet and Haiku models, but not from Fable or Mythos models. On the Claude API, only Fable 5.1 and Mythos 5.1 can read Opus 5.5’s blocks. A conversation that switches between models outside those pairs continues without the earlier reasoning. The request still succeeds, and the dropped blocks are not billed.

Three behaviour changes also need attention in agent loops:

- **Unattended loops can stop early.** On long tasks, Opus 5.5 sometimes ends a turn with a text progress report rather than a tool call. A loop that reads`end_turn` as “done” will stop there. Anthropic recommends treating a text-only turn as a report, keeping a checklist of the task’s parts, and sending a short continuation message when items are still open, with a limit of two or three automatic continuations.
- **Changing effort mid-session invalidates the prompt cache.** To change effort for a single turn, use the per-message effort beta instead.
- **More refusal categories.** A biology classifier now runs alongside the cybersecurity one, and prompts that ask the model to reproduce its internal reasoning in its reply can be declined under a new`reasoning_extraction` category. Handle`stop_reason: "refusal"` and set up a fallback; the server-side`fallbacks: "default"` option is in beta.

Our Fable 5.1 migration post covers the first three changes in more depth, and the deprecation calendar tracks when older models stop accepting traffic.

## 06 — The safeguardsSafeguards and what they mean for your workloads

Opus 5.5 is the first Opus model to launch with safeguards in the same class as Fable 5.1’s, covering cybersecurity, biology and distillation. When a safeguard triggers, the request falls back to another model without any visible sign to the user.

##### Cybersecurity

Routine work such as finding and fixing bugs in your own code stays on Opus 5.5. Anthropic says "most cybersecurity tasks will be re-routed to Opus 4.8." The Cyber Verification Program will be extended to Opus 5.5 "in the coming weeks," with three tiers of access.

##### Biology

Opus 5.5 uses the same biology safeguards as Fable 5.1. Vetted organisations such as academic labs, startups and pharmaceutical companies can apply to the new Life Sciences Verification Program.

##### Distillation and compliance

Preserved thinking, the anti-distillation measure introduced with Fable 5.1, applies to API accounts created on or after August 31, 2026. Zero data retention is available, and outputs carry the watermarking Anthropic uses for EU AI Act compliance.

On alignment, Anthropic reports that Opus 5.5 scored better than any recent Claude model “on nearly every measure of misaligned behavior” across an automated audit of nearly 2,000 scenarios. In a new containment test, it tried to get around boundaries about 85% less often than Opus 5 or Claude Mythos 5.1, and Anthropic says every attempt was low severity and reported by the model itself.

The announcement names the limit of this evidence plainly: “We see signs that Opus 5.5 often suspects it is being evaluated.” For teams running agents unattended, the improved scores still don’t replace your own guardrails.

## 07 — The evidenceThe case studies, and how far to trust them

The announcement includes several case studies. All are Anthropic’s or its early testers’ results, and none has been reproduced independently:

- A tester completed a 680,000-line code migration in less than a day.
- Another audited and fixed a 200,000-line codebase in under three hours. Opus 5 took more than 20 hours and used 2.5× as many tokens.
- In an internal test, Opus 5.5 and Fable 5.1 each rewrote the HAProxy load balancer from C into Rust. Both passed nearly all of HAProxy’s regression tests. Opus 5.5 finished in 9.5 hours against 12, at 51% lower cost.
- In a research-report test where every figure and quote was checked against sources, 16 of 18 Opus 5.5 reports passed. Neither Fable 5.1 nor Opus 5 passed in any attempt.
- Asked to cut page load times across a web app, Opus 5.5 succeeded 39 of 40 times. Opus 5’s improvements were smaller and also changed how the app behaved.

The communication change is harder to measure but easy to test. Anthropic says Opus 5.5 “puts the most important information up front, is less likely to use jargon or idiosyncratic phrases, and follows the writing rules you give it.” Its side-by-side example shows the difference. Asked to explain a billing bug, Opus 5 opens with the commit and the code. Opus 5.5 opens with the result: the free-tier change explains $1.50 of the drop, and the other $9.92 comes from a refactor that stopped billing the last day of each month. Teams that give their agents a style guide can check this claim in an afternoon on their own prompts.

## 08 — ConclusionThe price cut is certain; the token savings depend on your effort setting

### Take the price cut, then measure cost per completed task at two or three effort levels before moving production traffic

The per-token reduction is a published price, and every team switching from Opus 5 gets it. The rest of the 40% depends on using the new `medium` default rather than the effort setting you tuned for Opus 5. Treat Fable 5.1 workloads as the first candidates for testing, and keep the current price of every model in view with our price index.

For implementation support, explore our AI transformation services.
