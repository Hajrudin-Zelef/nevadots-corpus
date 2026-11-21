---
id: collect-261121-veille-ia/claude-anthropic/claude-opus-5-5-specs-price-and-place-in-the-field
title: "Claude Opus 5.5: Specs, Price, and Place in the Field"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-09-30"]
keywords: ["opus", "claude", "page", "output", "model", "price", "tokens", "fable"]
source: https://dev.to/modelflare_dev/claude-opus-55-specs-price-and-place-in-the-field-3jal
source_anchor: ""
source_lines: [1, 155]
section: "Claude_Anthropic"
---
A source-backed account of Claude Opus 5.5: published specs, official token prices, how to read the benchmark table against Fable 5.1, Opus 5, and GPT-6 Astra, and the Modelflare groups and user prices returned on 30 September 2026.

Claude Opus 5.5 was released on 22 September 2026. The public contract is on the model page and in the what's-new note: API id `claude-opus-5-5`, a 1M-token context window, 128K max output, adaptive thinking that stays on, default effort `medium`, and a list price of $4 per million input tokens and $20 per million output tokens. Anthropic's pages do not publish a parameter count or a training-compute figure. This article stays with the published spec, the published price card, and the published benchmark table.

**Checked conclusion:** on Anthropic's list price, Opus 5.5 is the current Opus tier. Per token it is cheaper than Opus 5, and much cheaper than Fable 5.1. On the coding and knowledge-work rows Anthropic printed, its scores lead Fable 5.1, Opus 5, and, on most of those rows, GPT-6 Astra. Two printed rows go the other way: Astra leads Terminal-Bench-Science 0.1 and Zapier's AutomationBench. Modelflare's pricing API, read on 30 September 2026, includes `claude-opus-5-5` at the official $4 / $20 unit prices. That row enables `claude-award` 0.25, `claude-stable` 0.315, and `claude-premium` 0.5. A key for this tier sends `claude-opus-5-5`. Live rates are on the pricing page.

Official behavior below is cited from the Opus 5.5 model page, what's new, and the launch page. The earlier Fable 5.1 review is the place for that model's migration notes.

| Item | Claude Opus 5.5 | 
|---|---|
| Model id, Claude API | `claude-opus-5-5` | 
| Amazon Bedrock id | `anthropic.claude-opus-5-5` | 
| Google Cloud and Microsoft Foundry id | `claude-opus-5-5` | 
| Release date | 22 September 2026 | 
| Context window | 1M tokens, both the default and the maximum | 
| Max output | 128K tokens | 
| Thinking | Adaptive, always on | 
| Default effort | `medium` | 
| Latency class on the model page | Moderate | 
| Reliable knowledge cutoff | June 2026 | 
| Parameter count | Not on the public model page | 

"Reliable knowledge cutoff" is Anthropic's name for the date through which the model's stored knowledge is most extensive. It is separate from the context window. A file you attach is still in the request.

Fast mode is a speed switch on the same id, documented as a research preview on the Claude API only. The request sets `speed: "fast"` with the `fast-mode-2026-02-01` beta header. Anthropic prices that mode at $8 / $40 per million tokens and describes output as up to 2.5× faster. The what's-new page says this fast mode is absent on Amazon Bedrock, Claude Platform on AWS, Google Cloud, and Microsoft Foundry.

The same page lists the effort ladder as the control for thinking depth: `low`, `medium`, `high`, `xhigh`, and `max`. A manual thinking budget is rejected. The minimum cacheable prompt is 512 tokens.

Figures are US dollars per million tokens. Opus 5.5's cache, batch, and fast-mode rows come from the what's-new pricing section. Opus 5's cache-write cell is the single "cache writes" number on the launch comparison table, which does not split the 5-minute and 1-hour tiers. Fable 5.1's cache read is the official figure already used in this blog's Fable 5.1 review. Sonnet 5 and Haiku 4.5 input and output prices are the comparison row on the Opus 5.5 model page. An em dash means this article did not take that cell from a primary price row.

| Price per 1M tokens | Opus 5.5 | Opus 5 | Fable 5.1 | Sonnet 5 | Haiku 4.5 | 
|---|---|---|---|---|---|
| Input | $4 | $5 | $10 | $2 | $1 | 
| Output | $20 | $25 | $50 | $10 | $5 | 
| Cache read | $0.20 | $0.50 | $0.25 | — | — | 
| Cache write | $5 for 5 minutes; $8 for 1 hour | $6.25 | — | — | — | 
| Batch input / output | $2 / $10 | — | — | — | — | 
| Fast mode input / output | $8 / $40 | — | — | — | — | 

The list-price move from Opus 5 is 20% on fresh input and output, from $5 / $25 to $4 / $20. Cache reads, which dominate long agent loops, move from $0.50 to $0.20. That is 60% off the Opus 5 cache-read price, and it also sits under Fable 5.1's $0.25 cache read. Anthropic's separate workload claim is that a typical run at default settings costs about 40% less than Opus 5, because the model also uses fewer tokens. That 40% describes their harness. A real bill still follows effort, cache hit rate, and tool rounds.

Batch processing is half the new list price. Fast mode is double the new list price, on the Claude API only.

`GET https://modelflare.dev/api/pricing` on 30 September 2026 includes `claude-opus-5-5`. The same response also includes `claude-opus-5`, `claude-fable-5-1`, `claude-sonnet-5`, `claude-sonnet-5-5`, `claude-haiku-4-5`, `gpt-6-astra`, and `gpt-5.6-sol`. It does not include a Haiku 5.5 id.

Unit prices below are that response's token prices before a group ratio. For the Claude rows they match the official list. Astra and Sol are tiered: the second pair applies when input context is above 272,000 tokens, and the long-context output price is the tier's own output price. The example column multiplies Claude rows by `claude-stable` 0.315 and OpenAI rows by `openai-stable` 0.08. Claude ratios on this read are `claude-premium` 0.5, `claude-stable` 0.315, and `claude-award` 0.25. OpenAI ratios are `openai-premium` 0.13, `openai-stable` 0.08, and `openai-award` 0.03. Opus 5.5 enables only the three Claude groups above. `claude-tmp-cheap` is a public Claude group, and this model row does not include it.

| Model | Catalog id | Unit input / output | User price on the stable group | 
|---|---|---|---|
| Opus 5.5 | `claude-opus-5-5` | $4 / $20 | $1.26 / $6.30 | 
| Opus 5 | `claude-opus-5` | $5 / $25 | $1.575 / $7.875 | 
| Fable 5.1 | `claude-fable-5-1` | $10 / $50 | $3.15 / $15.75 | 
| Sonnet 5 | `claude-sonnet-5` | $2 / $10 | $0.63 / $3.15 | 
| GPT-6 Astra | `gpt-6-astra` | $10 / $50, then $20 / $75 | $0.80 / $4.00, then $1.60 / $6.00 | 
| GPT-5.6 Sol | `gpt-5.6-sol` | $5 / $30, then $10 / $45 | $0.40 / $2.40, then $0.80 / $3.60 | 

Opus 5.5 on `claude-stable` is $4 × 0.315 and $20 × 0.315. Use the official table to compare labs. Use the catalog table to read a Modelflare bill you can send today.

Haiku 4.5 is in the same catalog at a $1 / $5 unit price. On `claude-stable` that is $0.315 / $1.575. The model-page comparison still gives Haiku a 200K context window and 64K max output. Haiku 5.5 is not in the 30 September response. Haiku 4.5 remains the low end of the Claude line that is actually listed.

Cache ratios on the catalog rows are a fraction of the input unit price: 0.05 for Opus 5.5, so the catalog cache read is $0.20 and the catalog cache write is $5. That write matches the official 5-minute tier. The official 1-hour write is $8 and is not a separate catalog price. Opus 5 and Sonnet 5 use 0.1, and Fable 5.1 uses 0.025. Settle cache, long context, and priority tiers from the pricing page and from the finished usage record.

| Behavior | Opus 5.5 | Opus 5 | Fable 5.1 | 
|---|---|---|---|
| API id | `claude-opus-5-5` | `claude-opus-5` | `claude-fable-5-1` | 
| Context / max output | 1M / 128K | 1M / 128K | 1M / 128K | 
| Thinking | Always on. `disabled` and a manual budget return 400 | On by default. `disabled` is accepted only at effort`high` or below | Always on | 
| Default effort when the field is omitted | `medium` | `high` | `high` | 
| Reliable knowledge cutoff | June 2026 | May 2026 | June 2026 | 
| List input / output | $4 / $20 | $5 / $25 | $10 / $50 | 
| Cache read | $0.20 | $0.50 | $0.25 | 
| Forced tool choice | `any` and`tool` return 400 | Listed as a breaking change away from Opus 5 | Same 400 as Opus 5.5 | 
| Cybersecurity safeguard | Hands the task to Opus 4.8 | The launch note does not put this fallback on Opus 5 | Fable-class safeguards | 
| Biology and frontier-LLM safeguard | Hands the task to Opus 5 | The launch note pairs this fallback with Opus 5.5 | Same safeguard class | 

Opus 5.5 reads thinking blocks from Opus 5 and from earlier Opus, Sonnet, and Haiku models. It does not read thinking blocks from Fable or Mythos. On the Claude API, Fable 5.1 and Mythos 5.1 read Opus 5.5 thinking blocks, and the what's-new page says no other current model does. A block the target model cannot read is dropped before the model sees it, and a dropped block is not billed.

Accounts created on or after 31 August 2026 get a stricter check: if the system prompt, the tool list, or an earlier message changed after an Opus 5.5 thinking block was produced, replaying that block returns 400. The migration guide's path is an append-only history, or a mid-conversation system message when instructions have to change. The launch page also says Opus 5.5 ships with the preserved-thinking control introduced on Fable 5.1, with EU AI Act watermarking, and with zero data retention, in line with earlier Opus models.

The table is the one printed on the launch page. Anthropic says that, unless a footnote says otherwise, Opus 5.5 ran with adaptive thinking at max effort. Terminal-Bench 4.0 is the footnote: Opus 5.5 at `xhigh`, GPT-6 Astra at `high`, each as that model's highest reported score. Astra and Sol numbers are quoted as reported by OpenAI. Production safeguards were on. When they fired, cybersecurity tasks were completed by Opus 4.8, and biology plus frontier LLM-development tasks were completed by Opus 5. Anthropic says that pulls Opus 5.5's scores down.

| Benchmark | Opus 5.5 | Fable 5.1 | Opus 5 | GPT-6 Astra | GPT-5.6 Sol | 
|---|---|---|---|---|---|
| Terminal-Bench 4.0 | 66.4% | 55.8% | 52.3% | 57.9% | 37.3% | 
| FrontierCode v1.1 (Main) | 54.4% | 50.3% | 48.0% | 53.3% | 47.5% | 
| CursorBench 4.0 | 57.8% | 51.8% | 46.6% | — | 41.7% | 
| GDPval-AA v2.1 | 1846 | 1735 | 1708 | 1542 | 1588 | 
| AutomationBench | 40.0% | 31.4% | 26.9% | 41.4% | 28.8% | 
| Humanity's Last Exam, with tools | 67.7% | 65.6% | 63.6% | 57.2% | — | 
| Terminal-Bench-Science 0.1 | 58.7% | 52.6% | 29.0% | 64.6% | 22.4% | 
| OSWorld 2.0, partial | 81.8% | 80.7% | 74.0% | — | — | 
| Chartography, with tools | 89.0% | 88.4% | 83.4% | — | — | 

Put the gaps next to the error bars they published. Terminal-Bench 4.0 is ±2.6 points for Opus 5.5 and ±1.6 to ±2 points for the other Claude models. The gap from 66.4% to Astra's 57.9%, and to Fable's 55.8%, is wider than those bars. Terminal-Bench-Science is ±3.5 to ±5 points per model. Astra's 64.6% against Opus 5.5's 58.7% sits near the width of those bars, with Astra ahead in the printed point estimate.

AutomationBench was run by Zapier. Safeguard interventions counted as failures because the run had no fallback model. Anthropic says the practical score would be higher. The printed 40.0% against Astra's 41.4% is a close published result, with Astra ahead on the point estimate. GDPval-AA v2.1 is an Elo on real professional tasks across 44 occupations: 1846 for Opus 5.5, 1735 for Fable 5.1, 1708 for Opus 5, 1588 for Sol, 1542 for Astra.

The same launch page prints a second pair of scores on the cost charts, taken at default effort `medium`: FrontierCode 54.6% and CursorBench 52.5%. The comparison table prints 54.4% and 57.8%. Keep them as two Anthropic figures at two effort settings. The chart caption also says that, at medium effort, FrontierCode beats Astra's top score of 53.3% at about a fifth of the cost per task, and that Terminal-Bench matches Astra at about 40% of the cost. Those are cost-per-task claims about Anthropic's harness.

The sentence under the table is the one to keep: in Anthropic's own use, the gap between Opus 5.5 and Fable 5.1 is narrower than the scores, and benchmark margins at this level are a weaker guide to a real task. Customer quotes on the launch page are selected testimonials. They can suggest what to replay. They do not belong in the score table.

Three checkable shifts landed on the same day.

The work Anthropic describes as Fable 5.1 level for most tasks now has an Opus price card. List price is $4 / $20 against Fable 5.1 at $10 / $50. Cache reads are $0.20 against Fable's $0.25 and Opus 5's $0.50. Where a team's own replay agrees, Fable 5.1 stays the model for the tasks that still fail, and Opus 5.5 is the model that should run the loop. Where the replay disagrees, the table already shows the published exceptions: science-agent tasks and Zapier's business workflows, where Astra leads the printed numbers.

The default moved with the price. Claude Code's model configuration, updated with this release, sets Opus 5.5 as the default for Pro, Max, Team, Enterprise, and the Anthropic API, and also for Claude Platform on AWS, Amazon Bedrock, and Google Cloud's Agent Platform. A default inside the agent product moves more traffic than a new row in a picker.

The safeguard boundary moved down onto Opus. Earlier Opus models were where traffic went when a Fable classifier would not answer. Opus 5.5 launches with a similar set: cybersecurity tasks are completed by Opus 4.8, and biology plus frontier LLM-development tasks are completed by Opus 5. A log line that records the requested id `claude-opus-5-5` can sit on a completion produced by a different model. Refusals return HTTP 200 with `stop_reason: "refusal"`.

Sonnet 5.5 is already in the 30 September catalog at the same $2 / $10 unit price as Sonnet 5. Haiku 5.5 is not in that response. The cheaper listed Claude cards are Sonnet 5.5, Sonnet 5 at $2 / $10, and Haiku 4.5 at $1 / $5. Haiku 4.5 remains on a 200K window. Sonnet 5.5's groups are in the Sonnet 5.5 review.

This is a position you can audit from documents: list price, the Claude Code default, which model answers when a safeguard fires, and which rows of the vendor table Astra still leads. It is not a market-share claim.

A client that only replaces the model string still hits four breaks documented for code that was running on Opus 5. The first three also apply to Fable 5.1. The shape below is the one the migration note tells you to send. The comments mark the variants that return `invalid_request_error`.


```
model: claude-opus-5-5
thinking: {"type": "adaptive"}
tool_choice: {"type": "auto"}
output_config.effort: medium
# 400: thinking type disabled, or enabled with budget_tokens
# 400: tool_choice any, or tool_choice tool
# 400 on the Claude API and Google Cloud: tool type computer_20251124
```
Depth replaces the old on/off thinking switch. Omit `thinking`, or send `adaptive`. Set effort explicitly. `tool_choice` `auto` and `none` remain valid, including on the token-counting endpoint. For schema-valid JSON, the migration note points to strict tool use or structured outputs, plus a prompt line that says when the tool applies.

On the Claude API and Google Cloud, replace `computer_20251124` with the `computer_toolset_20260801` toolset and drop the old computer-use beta header. Amazon Bedrock still accepts `computer_20251124` on Opus 5.5. Browser-use integrations that already use the toolset need no change for this break.

For accounts created on or after 31 August 2026, a replay that edits the prefix in front of an Opus 5.5 thinking block is the fourth 400. Keep the conversation append-only.

Omitting effort now runs `medium`. The same omission on Opus 5 ran `high`. At a matched effort, Opus 5.5 tends to think more per turn than Opus 5, especially at `xhigh` and `max`. An effort value copied from an Opus 5 config is a new cost point. Leave room in `max_tokens` for the thinking blocks.

Text written between tool calls comes back inside thinking blocks. At the default display value `omitted`, those blocks are empty. A UI that used to stream that text goes quiet, and the request still succeeds. Set a display value when the product needs the progress lines.

A biology classifier runs beside the cybersecurity classifier. A prompt that pushes the model to copy its reasoning into the visible answer can be declined under `reasoning_extraction`. Chart and screenshot reading is sharper before any vision tool is added, according to the prompting guide, so older prompts that used to compensate for weak chart reading are worth replaying on their own.

Replay Opus 5.5 on the jobs the launch page is specific about: a multi-file change, a migration with the repository's own tests, a research memo graded against its sources, and a computer-use flow you can run twice. Record the requested model, the model that actually answered, effort, input tokens, cache-read tokens, output tokens, wall time, retries, and human edits. Anthropic's "about 40% less" and "about a fifth of the cost" are claims about their harness. They do not replace that sheet.

Keep Fable 5.1, or an explicit fallback, on tasks the replay still loses. Plan for Opus 4.8 on traffic the cybersecurity safeguard will hand off, and for Opus 5 on biology and frontier-model-development traffic the safeguard will hand off. Keep Sonnet 5.5 or Sonnet 5 where the task fits a $2 / $10 card and does not need the Opus loop.

On Modelflare, send `claude-opus-5-5` for this tier. The 30 September row enables `claude-premium` at 0.5 when a failed call is expensive, `claude-stable` at 0.315 for ordinary production, and `claude-award` at 0.25 when the call can be retried. `claude-tmp-cheap` is not enabled on this model. On `claude-stable` the user price is $1.26 per million input tokens and $6.30 per million output tokens.

- Launch page, 22 September 2026: https://www.anthropic.com/claude-opus-5-5
- Model page, specs and the current comparison row: https://platform.claude.com/docs/en/models/opus-5-5/overview
- What's new, breaking changes and the price card: https://platform.claude.com/docs/en/models/opus-5-5/whats-new-opus-5-5
- Claude Code model configuration, default model: https://code.claude.com/docs/en/model-config
- Modelflare pricing API, read 30 September 2026: https://modelflare.dev/api/pricing
- Fable 5.1 review on this blog: Fable 5.1 review

*Originally published at modelflare.dev.*

*Disclosure: I'm part of the Modelflare team, which builds the AI API gateway mentioned in this article.*

## Top comments (0)
