---
id: collect-261121-veille-ia/claude-anthropic/claude-haiku-5-5-pricing-benchmarks-tested-computingforgeeks
title: "Claude Haiku 5.5: Pricing, Benchmarks, Tested | ComputingForGeeks"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["haiku", "claude", "tokens", "model", "output", "anthropic", "pricing", "benchmarks"]
source: https://computingforgeeks.com/claude-haiku-5-5-released-features-benchmarks/
source_anchor: ""
source_lines: [1, 230]
section: "Claude_Anthropic"
---
Claude Haiku 5.5 costs $0.10 per million input tokens and $0.50 per million output, a tenth of what Haiku 4.5 charges, and on the benchmarks Anthropic published it beats GPT-6 Luna almost everywhere. The headline “75% cheaper” figure undersells the per-token cut and oversells it at the same time, because the new tokenizer inflates every prompt and a second price tier starts at 100K tokens.

This guide covers the Claude Haiku 5.5 specs, pricing, official benchmarks and the API changes that break Haiku 4.5 code. Then come our own numbers: a tokenizer count across six file types, a graded coding test at every effort level, streaming latency, and the exact 400 errors the API returns. Every test ran against the live Claude API in October 2026, the day after launch.

## What is new in Claude Haiku 5.5

Anthropic released Haiku 5.5 on October 7, 2026, nine days after Claude Sonnet 5.5. It completes the Claude 5.5 family and replaces Haiku 4.5 as the small, fast tier. The announcement calls it “the cheapest, fastest, and most capable small model we’ve ever released”. The model card facts, from the platform docs and the Bedrock model card:

| Spec | Claude Haiku 5.5 | Claude Haiku 4.5 | 
|---|---|---|
| API model ID | `claude-haiku-5-5` (no date suffix, no alias) | `claude-haiku-4-5-20251001` | 
| Bedrock ID | `anthropic.claude-haiku-5-5` , profiles`global.` ,`us.` ,`eu.` ,`au.` ,`jp.` | `anthropic.claude-haiku-4-5-20251001-v1:0` , called through a`us.` or`global.` profile | 
| Context window | 1M tokens | 200K tokens | 
| Max output | 128K (300K on Batches with the `output-300k-2026-03-24` beta) | 64K | 
| Input / output | Text and images in, text out | Text and images in, text out | 
| Knowledge cutoff | June 2026 | February 2025 | 
| Thinking | Adaptive, on by default | Manual `budget_tokens` | 
| Effort levels | low, medium, high, xhigh, max (default medium) | Not supported | 
| Tokenizer | New, same family as Sonnet 5.5 and Opus 5.5 | Older tokenizer | 
| Retirement | Not sooner than October 7, 2027 | Not sooner than October 15, 2026 | 

Three changes matter most in practice. The context window grew five times, which puts a 1M-token window on the cheapest Claude model for the first time. Haiku now thinks by default through adaptive thinking, and it is the first Haiku that takes the effort parameter. Finally, the sampling controls are gone: `temperature`, `top_p` and `top_k` return a 400, the same as on the larger 5.5 models.

## Claude Haiku 5.5 pricing

Long prompts are where the pricing gets complicated. Haiku 5.5 is the one current Claude model with a long-context price tier. Every other model since Claude 4.6 bills the full 1M window at its standard rate. Haiku 5.5 charges five times its base rate once the prompt passes 100K tokens, and the higher rate then applies to every token in that request, output and cache included:

| Per 1M tokens | Haiku 5.5, prompt up to 100K | Haiku 5.5, prompt over 100K | Haiku 4.5 | Sonnet 5.5 | 
|---|---|---|---|---|
| Input | $0.10 | $0.50 | $1.00 | $2.00 | 
| Output | $0.50 | $2.50 | $5.00 | $10.00 | 
| Cache write, 5 minutes | $0.125 | $0.625 | $1.25 | $2.50 | 
| Cache write, 1 hour | $0.20 | $1.00 | $2.00 | $4.00 | 
| Cache read | $0.01 | $0.05 | $0.10 | $0.10 | 
| Batch input / output | $0.05 / $0.25 | $0.25 / $1.25 | $0.50 / $2.50 | $1.00 / $5.00 | 

Priority Tier is not available for Haiku 5.5 on the Claude API. US-only inference through `inference_geo` adds the usual 1.1x multiplier. The Sonnet 5.5 cache-read price in that table is new too: Anthropic cut it from $0.20 to $0.10 in the same announcement.

### Is Claude Haiku 5.5 75% or 90% cheaper?

Both numbers come from Anthropic, and they measure different things. The per-token price is 90% lower than Haiku 4.5 for prompts up to 100K tokens and 50% lower above that. The 75% figure is a blended cost to run: footnote 2 of the announcement weights the two tiers by real traffic (90% of Haiku 4.5 requests were under 100K) and subtracts the extra tokens the new tokenizer produces.

Your own number depends on prompt size. Two worked examples at list price, using the 1.3x token inflation Anthropic documents. They assume low effort or thinking off. At the default medium effort, thinking tokens bill as output and push the output count higher:

| Request | Haiku 4.5 cost | Haiku 5.5 cost | Saving | 
|---|---|---|---|
| 20K-token prompt, 1K output (Haiku 4.5 counts) | $0.0250 | $0.0033 (26K in, 1.3K out) | 87% | 
| 150K-token prompt, 2K output (Haiku 4.5 counts) | $0.1600 | $0.1040 (195K in, 2.6K out) | 35% | 

One trap follows from that math. The 100K boundary is counted in Haiku 5.5 tokens. A prompt that measured 80K tokens on Haiku 4.5 comes out near 104K on Haiku 5.5, which puts it in the expensive tier. If a pipeline sends large documents, count them again with the new model before you budget.

## How many more tokens the Haiku 5.5 tokenizer uses

The docs say the same text counts as about 30% more tokens than on Haiku 4.5. We sent six samples of up to 20,000 characters to the `count_tokens` endpoint for both models. The samples were real files from our own repository: article prose, a Python tool, a PHP theme file, a Bash hook, an Authelia YAML config and a JSON price list.

| Sample (20,000 chars) | Haiku 4.5 tokens | Haiku 5.5 tokens | Ratio | Input cost vs Haiku 4.5 | 
|---|---|---|---|---|
| English prose | 5,935 | 8,087 | 1.36x | 13.6% (86% cheaper) | 
| Python (12,147 chars, whole file) | 3,757 | 4,829 | 1.29x | 12.9% (87% cheaper) | 
| PHP | 5,983 | 7,752 | 1.30x | 13.0% (87% cheaper) | 
| Bash script | 7,114 | 9,579 | 1.35x | 13.5% (87% cheaper) | 
| YAML config | 5,339 | 7,280 | 1.36x | 13.6% (86% cheaper) | 
| JSON | 8,980 | 9,945 | 1.11x | 11.1% (89% cheaper) | 

English prose and YAML took the biggest hit at 36%. JSON barely moved. The coding test below confirms the pattern from a different angle: the same eight prompts measured 1,023 input tokens on Haiku 4.5 and 1,421 on Haiku 5.5, a 1.39x ratio. Under 100K tokens, the per-character input cost still lands 86% to 89% below Haiku 4.5, so the tokenizer eats only a few points of the 90% cut.

## Claude Haiku 5.5 benchmarks

Anthropic compares Haiku 5.5 with Haiku 4.5, the small OpenAI model GPT-6 Luna, and Sonnet 5.5 as a ceiling. Haiku 5.5 numbers are at max effort. The announcement table, plus the coding rows from the 144-page system card:

| Benchmark | Haiku 5.5 | Haiku 4.5 | GPT-6 Luna | Sonnet 5.5 | 
|---|---|---|---|---|
| GDPval-AA v2.1 (Elo) | 1620 | 735 | 1437 | 1840 | 
| AA-Briefcase v1.1 (Elo) | 1578 | 614 | 1336 | 1824 | 
| OSWorld 2.1, offline subset | 72.4% | 15.7% | 48.9% | 83.9% | 
| Humanity’s Last Exam, no tools | 45.9% | 10.2% | n/a | 56.9% | 
| Humanity’s Last Exam, with tools | 57.4% | 18.7% | n/a | 64.5% | 
| Terminal-Bench 4.0 | 39.2% | 0.0% | 16.4% | 70.6% | 
| FrontierCode 1.1 Main | 46.4% | n/a | 42.4% | 52.1% (xhigh) | 
| Chartography, no tools | 46.4% | 6.4% | 29.1% | 61.6% | 
| SWE-Bench Pro | 64.8% | n/a | n/a | 81.3% | 
| SWE-bench Multilingual | 83.7% | 67.4% | n/a | 90.3% | 

On every row where both models have a score, Haiku 5.5 beats GPT-6 Luna, and on the two knowledge-work Elo ratings it closes about 80% of the gap between Haiku 4.5 and Sonnet 5.5 at a twentieth of the Sonnet input price. Terminal-Bench is the gap that stays wide: 39.2% against 70.6% for Sonnet 5.5. Agentic shell work is still Sonnet territory.

Read the table with three caveats. All Haiku 5.5 scores were run at max effort, and the system card shows how far that is from the default: GDPval-AA drops from 1620 to 1277 at medium effort, though medium uses about a tenth of the output tokens. Artificial Analysis measured Terminal-Bench 4.0 at 33% in its own harness, against 39.2% in the Anthropic run, and puts Haiku 5.5 at 43 on its Intelligence Index, ahead of Gemini 3.8 Flash at 41. Anthropic did not publish SWE-bench Verified, GPQA Diamond, AIME or tau-bench for this model, so any article quoting those as Anthropic figures is guessing.

## We graded Haiku 5.5 and Haiku 4.5 on eight coding tasks

Vendor tables run at max effort. Most production calls will not. So we wrote eight small Python tasks with tricky edge cases and graded the answers with hidden asserts in a subprocess. The tasks: an ISO 8601 duration parser, half-open interval merging, an O(1) LRU cache, a topological sort with lexical tie-breaks, SemVer 2.0.0 ordering, strict Roman numerals, POSIX shell word splitting without `shlex`, and a cron day-of-week expander. Each model got the same system prompt asking for one code block and nothing else, with `max_tokens` at 32,000. Costs are at list price.

| Model and effort | Passed | Output tokens | Wall time (sum) | Cost for all 8 | 
|---|---|---|---|---|
| Haiku 4.5 | 4/8 | 6,580 | 54.3 s | $0.0339 | 
| Haiku 5.5, low | 8/8 | 6,444 | 31.2 s | $0.0034 | 
| Haiku 5.5, medium (default) | 8/8 | 8,360 | 39.1 s | $0.0043 | 
| Haiku 5.5, high | 8/8 | 10,830 | 47.4 s | $0.0056 | 
| Haiku 5.5, max | 7/8 | 132,499 | 538.4 s | $0.0664 | 

At low effort, Haiku 5.5 solved every task, finished faster than Haiku 4.5 and cost a tenth as much. Low, medium and high all scored 8/8. Max effort scored worse, ran more than eleven times longer than high and cost twice as much as Haiku 4.5. On the cron task it spent all 32,000 output tokens thinking and never wrote any code. Single tasks at max took about 100 seconds or more, and one used 26,746 tokens to sort version strings.

The Haiku 4.5 failures were real bugs, not formatting problems. It accepted `P1DT` as a valid duration. It returned the wrong words for `a '' b`, dropping the empty quoted argument. It rejected the cron range `5-7` as reversed, even though the prompt said 7 means Sunday. The Roman numeral answer was the strangest: Haiku 4.5 wrote `import anthropic` and a function that sends the numeral to `claude-3-5-sonnet-20241022` and asks that model to validate it. We ran that prompt three times and got the same approach every time.

This is a small test, eight tasks with one run each, so do not read it as a benchmark. The SemVer task was run a second time for every model after we found a bug in our own expected list; the table uses that run. The test does show that the generation gap is real at the cheap end, and that effort above high does not help small, well-specified coding tasks.

## How fast Haiku 5.5 streams

Anthropic calls Haiku 5.5 its fastest model at standard speed but publishes no tokens-per-second figure. We streamed a 600-word explanation of cgroups v2 three times per configuration and took the median:

| Configuration | Time to first text | Total time | Output tokens | Output speed | 
|---|---|---|---|---|
| Haiku 4.5 | 0.56 s | 10.2 s | 850 | 85 tokens/s | 
| Haiku 5.5, low, thinking off | 0.59 s | 6.4 s | 1,031 | 183 tokens/s | 
| Haiku 5.5, medium (default) | 2.66 s | 8.4 s | 1,534 (includes thinking) | Not comparable | 

With thinking off, Haiku 5.5 starts as fast as Haiku 4.5 and writes text about twice as fast. At the default medium effort it thought for about two seconds before the first visible word, then still finished ahead of Haiku 4.5. For chat interfaces and autocomplete, where the first word matters most, set effort to low and turn thinking off. Thinking tokens are generated before the first text token, so the medium row has no fair tokens-per-second figure.

## Call Claude Haiku 5.5 from the API

The request body is the familiar Messages API with one addition, `output_config.effort`. Leave out `temperature` and the other sampling parameters. Export your key first:

`export ANTHROPIC_API_KEY="your-api-key-here"`
A low-effort call with curl, piped through `jq` to keep the answer and the token counts:

```
curl -s https://api.anthropic.com/v1/messages \
  -H "x-api-key: $ANTHROPIC_API_KEY" \
  -H "anthropic-version: 2023-06-01" \
  -H "content-type: application/json" \
  -d '{"model": "claude-haiku-5-5", "max_tokens": 4096, "output_config": {"effort": "low"}, "messages": [{"role": "user", "content": "Give me a one-line systemd unit description for a backup job."}]}' \
  | jq '{model, stop_reason, text: .content[-1].text, usage: {input_tokens: .usage.input_tokens, output_tokens: .usage.output_tokens}}'
```
The response from our run:

```
{
  "model": "claude-haiku-5-5",
  "stop_reason": "end_turn",
  "text": "Description=Nightly backup of /home and /etc to off-site storage",
  "usage": {
    "input_tokens": 27,
    "output_tokens": 24
  }
}
```
The jq filter reads the last content block because a thinking block can come first. For the same reason, the Python SDK version selects blocks by type. Save this as `sdk_example.py`:

```
import anthropic
client = anthropic.Anthropic()
response = client.messages.create(
    model="claude-haiku-5-5",
    max_tokens=4096,
    output_config={"effort": "low"},
    messages=[{"role": "user", "content": "Explain what a Kubernetes PodDisruptionBudget does in two sentences."}],
)
for block in response.content:
    if block.type == "text":
        print(block.text)
print(response.usage.input_tokens, response.usage.output_tokens, response.stop_reason)
```
We ran it with `uv`, which pulled anthropic 1.12.1 into a throwaway environment. `pip install anthropic` followed by `python3 sdk_example.py` works the same:

`uv run --with anthropic python sdk_example.py`
Two sentences, then the usage line:

```
A PodDisruptionBudget (PDB) limits how many pods of an application can be voluntarily taken down at once, such as during node drains, cluster upgrades, or autoscaling events. It ensures a minimum number or percentage of pods remain available, so that planned maintenance doesn't cause service outages.
36 101 end_turn
```
In Claude Code, Haiku 5.5 is the default `haiku` model on the Anthropic API from version 2.1.293. Start a session with `claude --model claude-haiku-5-5` or switch inside one with `/model claude-haiku-5-5`. On Bedrock, Google Cloud, Foundry and Claude Platform on AWS, the `haiku` alias still resolves to Haiku 4.5. Pin it with `ANTHROPIC_DEFAULT_HAIKU_MODEL`: on Bedrock the value is an inference profile such as `us.anthropic.claude-haiku-5-5` or `global.anthropic.claude-haiku-5-5`, while Google Cloud and Foundry take the plain `claude-haiku-5-5`. Claude Code does not let you turn thinking off for this model. The built-in Explore subagent runs on your main model, not Haiku. To push exploration onto the cheap model, define a custom agent with `model: haiku`, as covered in our Claude Code subagents guide.

## Haiku 4.5 code that breaks on Haiku 5.5

Swapping the model ID is not enough. We sent common Haiku 4.5 request shapes to Haiku 5.5, and five of them now fail with HTTP 400:

Each error, and what to send instead.

### `temperature` is deprecated for this model.

Any `temperature` other than 1 fails. The same request with `temperature: 0.2` still returns 200 on Haiku 4.5. `top_k` fails at any value, and `top_p` is accepted only at 0.99. Sending `temperature` and `top_p` together fails even at those values. The fix is to delete all three from the request. Current Python SDK releases have already dropped these arguments, so there you get a `TypeError` in the client before any request leaves. If you used a low temperature for repeatable output, use structured outputs or a tool schema to pin the format.

### “thinking.type.enabled” is not supported for this model

The full message is: Use “thinking.type.adaptive” and “output_config.effort” to control thinking behavior. Manual thinking with `budget_tokens` is gone. Remove the `thinking` object, or send `{"type": "adaptive"}`, and set `output_config.effort` instead.

### This model does not support assistant message prefill

A request that ends with an assistant turn is rejected even with thinking off. Prefilling `{` to force JSON was a common Haiku trick. Use structured outputs on the Claude API. Bedrock does not support structured outputs, so define a tool with the JSON schema there.

### output_config.effort ‘max’ is not supported when thinking is disabled on this model

Thinking can be switched off with `{"type": "disabled"}`, but only at effort high or below. Our low-effort, thinking-off call returned 200 in 1.1 seconds with zero thinking tokens. At xhigh or max, leave thinking on.

### Empty response with stop_reason max_tokens

This one returns 200, which makes it worse. Thinking tokens count against `max_tokens`. We asked Haiku 5.5 a counting puzzle at the default medium effort with `max_tokens` set to 16,000. Both runs spent all 16,000 output tokens on thinking and returned a single thinking block with no text. A repeat at 3,000 confirmed the stop reason: `max_tokens`. A simple question got no thinking block at all, so adaptive thinking does scale with the task, but a hard prompt can burn the whole budget. Leave generous headroom, lower the effort, or check for a missing text block before you parse.

By default the `thinking` field comes back as an empty string with a signature. Add `"display": "summarized"` to the thinking object if you want to log a readable summary. The rest of the migration list, from the official migration guide:

- Computer use on the Claude API and Google Cloud accepts only `computer_toolset_20260801` . The`computer_20250124` tool returns 400. Bedrock lists`computer_20251124` instead.
- A new browser tool, `browser_toolset_20260801` , is available on the Claude API and Google Cloud. Haiku 4.5 does not support it.
- Thinking blocks are bound to the account that produced them. A block replayed from another account is silently dropped. The conversation must also stay append-only when you send thinking blocks back: on accounts created from August 31, 2026, editing an earlier turn and replaying the block returns 400. Older accounts get that error only with `thinking.block_binding.prefix_mismatch_behavior` set.
- Handle `stop_reason: "refusal"` . Haiku 5.5 has no server-side fallback model, and a retry usually refuses again.

## Where Claude Haiku 5.5 is available

It shipped everywhere on day one:

- **Claude apps:** Free, Pro, Max, Team and Enterprise plans, on web, iOS and Android.
- **Claude API and Claude Platform on AWS:**`claude-haiku-5-5` .
- **Amazon Bedrock:**`anthropic.claude-haiku-5-5` , with global, US, EU, Australia and Japan cross-region profiles, plus GovCloud (US).
- **Google Cloud Vertex AI and Microsoft Foundry:**`claude-haiku-5-5` .
- **GitHub Copilot:** generally available for Pro, Pro+, Max, Business and Enterprise, across VS Code, JetBrains, Xcode, the Copilot CLI and github.com. GitHub says it matched Sonnet 5 on many coding tasks in early testing with fewer tokens and steps.
- **Claude Code:** version 2.1.293 and later, as the default`haiku` model. Our Claude Code cheat sheet lists the model and effort commands.

Max and Team subscribers also got monthly API credits in the same announcement: $100 on Max 5x, $200 on Max 20x and up to $500 pooled on Team. Team credits are $20 per Standard seat and $100 per Premium seat. The credits do not roll over. Max subscribers claim them by linking a Console organization under Settings, Billing; Team owners do it under Organization settings, Billing.

## What the system card says about safety

The system card does not use an ASL label. It says Haiku 5.5 does not cross the CB-2 or Autonomy-2 thresholds and treats it as meeting CB-1 and Autonomy-1, with matching safeguards. A few findings matter for anyone building on it:

- **Prompt injection is far harder.** In the Gray Swan coding scenarios the attack success rate fell from 58.4% on Haiku 4.5 to 0.08%, and to zero with the Anthropic probes on.
- **It over-refuses.** In the automated behavioral audit it over-refused more than any other model tested, although it refused benign single-turn requests less often than Haiku 4.5. The cyber classifiers are stricter than those on Haiku 4.5 and still block penetration testing, so security tooling may need the Cyber Verification Program.
- **It hallucinates about as much as Haiku 4.5.** Artificial Analysis measured a 40% hallucination rate on AA-Omniscience.
- **One regression.** When a test leaked the expected answer, Haiku 5.5 used it without telling the user 17% of the time, against 2% for Haiku 4.5. Graders and eval harnesses should keep answers out of the context.

## Should you move from Haiku 4.5 now?

Haiku 4.5 is still listed as active, with retirement not before October 15, 2026. That date is a floor, not a shutdown: Anthropic gives at least 60 days of notice before retiring a model, and no deprecation notice came with this launch. You have time, but there is little reason to wait. For prompts under 100K tokens, Haiku 5.5 cost us 86% to 89% less per character and solved twice as many coding tasks. Run it at low or medium effort, strip the sampling parameters and prefills, and count your largest prompts again with the new tokenizer before trusting the bill. Keep Sonnet 5.5 or Opus 5.5 for long agentic shell work, where Haiku still trails badly. For coding agents, our guide to cutting Claude Code token usage shows where a cheap model fits in the loop.
