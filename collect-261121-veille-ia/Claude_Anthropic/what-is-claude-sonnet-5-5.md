---
id: collect-261121-veille-ia/claude-anthropic/what-is-claude-sonnet-5-5
title: "What Is Claude Sonnet 5.5?"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-09-28"]
keywords: ["claude", "sonnet", "model", "opus", "output", "september", "costs", "thinking"]
source: https://apidog.com/blog/what-is-claude-sonnet-5-5/
source_anchor: ""
source_lines: [1, 143]
section: "Claude_Anthropic"
---
Claude Sonnet 5.5 is Anthropic’s mid-tier model, released on September 28, 2026, with the API id `claude-sonnet-5-5`. It costs $2 per million input tokens and $10 per million output tokens, the same as Sonnet 5. In its launch post, Anthropic says it “runs 30%+ faster” than Sonnet 5 and, in Anthropic’s testing, “costs up to 30% less per task than its predecessor.” It’s the second model in the Claude 5.5 family: a cheaper complement to Opus 5.5 ($4/$20) and a step above Haiku 4.5.

Below: specs, the lineup, changes from Sonnet 5, benchmarks, where to run it, and who should switch, with links to deeper guides like the pricing breakdown. To call the model as you read, Apidog can send the Messages request, keep your key in an environment variable, and show the response and token usage together.

## Claude Sonnet 5.5 specs at a glance

| Spec | Claude Sonnet 5.5 | 
|---|---|
| Release date | September 28, 2026 | 
| Model id (Claude API, Google Cloud, Microsoft Foundry, Claude Platform on AWS) | `claude-sonnet-5-5` | 
| Model id (Amazon Bedrock) | `anthropic.claude-sonnet-5-5` | 
| Context window | 1M tokens | 
| Max output | 128K; 300K on the Batch API with the `output-300k-2026-03-24` beta | 
| Thinking | Adaptive, on by default | 
| Default effort | `high` on the Claude Platform;`medium` in Claude Code and the Claude apps | 
| Knowledge cutoff | June 2026 | 
| Retirement | Not sooner than September 28, 2027 | 
| Price per MTok | $2 input, $10 output, $0.20 cache read, $2.50 (5-minute) or $4 (1-hour) cache write; Batch $1 / $5 | 

Sources: the Sonnet 5.5 model overview and Claude pricing page.

There’s no long-context premium: a 900K-token request costs the same per token as a 9K one. Fast mode isn’t available; it’s Opus-only. US-only inference (`inference_geo: "us"`) costs 1.1x. Sonnet 5’s planned September 1 rise to $3/$15 was cancelled, so $2/$10 is the standard Sonnet rate.

## Where Sonnet 5.5 sits in the Claude lineup

| Model | Input / output per MTok | Context / max output | On Claude Free? | Role | 
|---|---|---|---|---|
| Claude Fable 5.1 | $10 / $50 | 1M / 128K | No | Demanding reasoning, long-horizon agentic work | 
| Claude Opus 5.5 | $4 / $20 | 1M / 128K | No | Complex, open-ended work | 
| Claude Sonnet 5.5 | $2 / $10 | 1M / 128K | Yes (chat) | Well-scoped everyday work | 
| Claude Haiku 4.5 | $1 / $5 | 200K / 64K | Yes | Lowest-cost tier | 

Anthropic pitches Sonnet 5.5 for well-scoped everyday tasks, bug fixes, and polished docs, slides and spreadsheets, and says Opus 5.5 “remains clearly stronger at complex, open-ended work.” Haiku 5.5 is due “in the coming weeks.” For the tier above, read what Claude Opus 5.5 is.

## What’s new compared with Sonnet 5

Price and tokenizer are unchanged. The API contract isn’t. The migration guide lists five breaking changes:

1. **`thinking: {"type": "disabled"}` is gone.** Use`{"type": "between_tools"}` , accepted only at`low` ,`medium` or`high` effort.
2. **Forced tool use is removed.**`tool_choice` of`any` or`tool` fails. Send`auto` with`strict: true` tools and say in the prompt when to use them.
3. **Thinking blocks are bound to the conversation.** For accounts created on or after 2026-08-31, replaying a block after editing earlier history fails. Keep conversations append-only.
4. **`computer_20251124` is rejected** on the Claude API and Google Cloud. Use`computer_toolset_20260801` .
5. **Advisor pairings are restricted** to Opus 5, Opus 5.5, Sonnet 5.5, Fable 5, Fable 5.1, Mythos 5 or Mythos 5.1.

A sixth change fails nothing but alters the response shape: text between tool calls now arrives as progress-update `thinking` blocks, empty under the default `display: "omitted"`, so your UI can go quiet mid-task. Set `display: "updates"` (beta header `thinking-display-updates-2026-08-18`) or `"summarized"` to see them.

Also new:

- **Recalibrated effort.** Don’t copy Sonnet 5 settings. Start at`high` (or`medium` for well-specified agentic coding) and run a fresh sweep.
- **Cyber safeguards.** It’s the first Sonnet with them: higher-risk cyber tasks “will visibly fall back to Sonnet 5” in Anthropic’s apps; on the API, fallback is opt-in (`fallbacks: "default"` , beta).
- **Account-bound thinking.** Thinking can’t move between accounts, which matters if you switch accounts mid-session in Claude Code.
- **Per-message effort.** Changing top-level effort between requests invalidates the prompt cache; the beta per-message effort change avoids that.
- **Lower cache minimum.** The smallest cacheable prompt is 512 tokens, down from 1,024.

The Sonnet 5.5 vs Sonnet 5 guide has the exact errors and before/after JSON for each fix.

## Claude Sonnet 5.5 benchmarks

The launch table compares Sonnet 5.5 with Sonnet 5, Opus 5.5 and GPT-6 Sol. Sonnet 5.5 ran at max effort unless a row says otherwise.

| Benchmark | Sonnet 5.5 | Sonnet 5 | Opus 5.5 | GPT-6 Sol | 
|---|---|---|---|---|
| Terminal-Bench 4.0 | 70.6% | 10.3% | 66.4% (xhigh) | n/r | 
| FrontierCode 1.1 Main (max) | 46.2% | 42.4% | 54.4% | 49.3% | 
| FrontierCode 1.1 Main (xhigh) | 52.1% | 42.7% | 51.4% | 48.4% | 
| CursorBench 4.0 | 55.5% | 34.1% | 57.8% | n/r | 
| GDPval-AA v2.1 (Elo) | 1844 | 1449 | 1846 | 1487 | 
| Humanity’s Last Exam (tools) | 64.5% | 54.9% | 67.7% | n/r | 
| OSWorld 2.1 (partial) | 80.1% | 57.0% | 81.8% | n/r | 

n/r: not reported. The xhigh FrontierCode row is from the launch page’s per-effort chart.

Anthropic ran Terminal-Bench, Humanity’s Last Exam and OSWorld itself. Cognition ran FrontierCode, where Sonnet 5.5 scored lower at max: it more often fanned out review subagents, which in cases Cognition examined caused a timeout or out-of-scope edits. Cursor ran CursorBench; Artificial Analysis ran GDPval-AA. Read the Terminal-Bench lead over Opus with care: Artificial Analysis measured Sonnet 5.5 at 63.6% in its own run, and the system card says fallback touched 10% of Opus trials versus 1.5% of Sonnet’s.

Sonnet 5.5 scores 56 on the Artificial Analysis Intelligence Index, #3 of 216, behind two Opus 5.5 endpoints (Sonnet 5 scored 38). The catch is verbosity: the index cost $8,977 to run at max effort versus $1,176 at high. The benchmarks deep dive has the per-effort tables.

## Where you can use Claude Sonnet 5.5

| Surface | Access | Notes | 
|---|---|---|
| Claude API | `claude-sonnet-5-5` | Prepaid credits | 
| Amazon Bedrock | `anthropic.claude-sonnet-5-5` | Global cross-Region inference only (commercial Regions); no strict tools | 
| Google Cloud, Microsoft Foundry, Claude Platform on AWS | `claude-sonnet-5-5` | Same id as the Claude API | 
| Claude.ai (web, iOS, Android) | Chat | Anyone, including Free | 
| Claude Code | `claude --model claude-sonnet-5-5` | Paid plans, v2.1.284+; default stays Opus 5.5 | 
| GitHub Copilot | VS Code, JetBrains, Copilot CLI and more | Pro, Pro+, Max, Business, Enterprise | 
| Cursor | `claude-sonnet-5-5` | Other Models pool (Pro and above) | 
| OpenRouter, Vercel AI Gateway | `anthropic/claude-sonnet-5.5` | $2 / $10; no free variant | 

In Claude Code, the `sonnet` alias maps to Sonnet 5.5 only on the Anthropic API; on Bedrock, Google Cloud and Foundry it still resolves to Sonnet 4.5, so pin the full id. See Sonnet 5.5 in Claude Code for plans and effort. GitHub’s Copilot changelog says the rollout is gradual.

Anyone can chat with Sonnet 5.5 on the Claude Free plan, but a chat plan includes no API key, and API usage is prepaid. See how to use Sonnet 5.5 for free and whether a free Sonnet 5.5 API exists.

## Who should switch to Sonnet 5.5

- **You run Sonnet 5:** switch. Price and tokenizer match, and Sonnet 5.5 beats Sonnet 5 on every row of the launch table. Budget time for the breaking changes and an effort sweep.
- **You run Opus 5.5 for everything:** route well-scoped work (bug fixes, docs, simple code reviews, subagent tasks) to Sonnet 5.5 at half the list price, and keep Opus for open-ended problems. The Sonnet 5.5 vs Opus 5.5 comparison breaks this down by workload.
- **You plan to run at high effort or above:** test Opus 5.5 at lower effort too. On Terminal-Bench 4.0, Sonnet 5.5 at high scores 43.0% for $1.94 per attempt; Opus 5.5 at medium scores 57.6% for $2.94. Sonnet’s 70.6% at max costs $12.54 per attempt. Many Hacker News commenters read the charts this way: Sonnet’s niche is low or medium effort, or subagent work under an Opus orchestrator.
- **Your work is security-heavy:** Anthropic warns of increased refusals “even on benign cybersecurity-related tasks.”

## What early customers report

Customer claims from Anthropic’s launch page, not independent tests:

- **Slack:** it beat Sonnet 5 on almost all offline Slackbot evals with no prompt changes and about 14% fewer output tokens.
- **Base44:** across 118 app builds, it matched Opus 5 quality with 3.6 iterations per build versus 7.7 for Opus 5.
- **Balyasny Asset Management:** on 2,441 finance tasks, it used about 121K tokens per answer versus 497K for Sonnet 5.

## Send your first Sonnet 5.5 request

```
curl https://api.anthropic.com/v1/messages \
  -H "x-api-key: $ANTHROPIC_API_KEY" \
  -H "anthropic-version: 2023-06-01" \
  -H "content-type: application/json" \
  -d '{
    "model": "claude-sonnet-5-5",
    "max_tokens": 16000,
    "output_config": {"effort": "medium"},
    "messages": [
      {"role": "user", "content": "List three risks of retrying a POST request without an idempotency key."}
    ]
  }'
```
Thinking is on by default, so skip the `thinking` field. Leave out `temperature`, `top_p` and `top_k`: non-default values return a 400. Refusals come back as HTTP 200 with `stop_reason: "refusal"` and a `stop_details` category, so check the body, not only the status.

In Apidog, paste the curl command into a new request, move the key into an `ANTHROPIC_API_KEY` environment variable, and assert that `stop_reason` isn’t `refusal` or `max_tokens`. Duplicate it at `low` and `high` effort to compare `usage`. The Sonnet 5.5 API guide covers streaming, tools and caching; our Anthropic API key guide helps if you need a key.

## FAQ

**When was Claude Sonnet 5.5 released?** September 28, 2026, six days after Opus 5.5.

**What is the Claude Sonnet 5.5 context window?** 1M tokens, with 128K max output (300K on the Batch API with a beta header). Long prompts cost the same per token as short ones.

**Is Claude Sonnet 5.5 free?** In the Claude.ai chat app, yes, including on the Free plan. The API is prepaid and Claude Code needs a paid plan. See every free route.

**What is the Claude Sonnet 5.5 knowledge cutoff?** June 2026.

**Is Sonnet 5.5 the default model in Claude Code?** No. The default stays Opus 5.5 on every paid plan. Switch with `/model` or `claude --model claude-sonnet-5-5`.

## Next step

Take one workload you run on Sonnet 5 or Opus 5.5 today. Send it to `claude-sonnet-5-5` at `low`, `medium` and `high`, compare quality and `usage`, and keep the cheapest setting that passes your checks. Download Apidog to keep those requests and assertions in one project, then use the pricing breakdown to turn token counts into a monthly bill.
