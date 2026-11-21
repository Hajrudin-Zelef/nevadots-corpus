---
id: collect-261121-veille-ia/claude-anthropic/what-is-claude-haiku-5-5
title: "What Is Claude Haiku 5.5?"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-10-07"]
keywords: ["haiku", "claude", "tokens", "output", "model", "input", "prompts", "effort"]
source: https://apidog.com/blog/what-is-claude-haiku-5-5/
source_anchor: ""
source_lines: [1, 136]
section: "Claude_Anthropic"
---
Claude Haiku 5.5 is Anthropic’s smallest and fastest current model, released on October 7, 2026, with the API id `claude-haiku-5-5`. It costs $0.10 per million input tokens and $0.50 per million output tokens for prompts up to 100K tokens, and $0.50/$2.50 for prompts over 100K. In its launch post, Anthropic says it costs “around 75% less to run” than Haiku 4.5 on average, and calls it “our fastest model to date.” It’s also the first Haiku with an effort setting, and it brings a 1M-token context window.

Below: specs, the two-tier price, changes from Haiku 4.5, benchmarks, where to run it, and who should switch, with links to deeper guides like the Haiku 5.5 pricing breakdown. To call the model as you read, Apidog can send the Messages request, keep your key in an environment variable, and show the response next to its token usage.

## Claude Haiku 5.5 specs at a glance

| Spec | Claude Haiku 5.5 | 
|---|---|
| Release date | October 7, 2026 | 
| Model id (Claude API, Google Cloud, Microsoft Foundry, Claude Platform on AWS) | `claude-haiku-5-5` (fixed id, no date suffix) | 
| Model id (Amazon Bedrock) | `anthropic.claude-haiku-5-5` | 
| Context window | 1M tokens (Haiku 4.5: 200K) | 
| Max output | 128K (Haiku 4.5: 64K); 300K on the Batch API with the `output-300k-2026-03-24` beta header | 
| Thinking | Adaptive only | 
| Effort levels | `low` ,`medium` (default),`high` ,`xhigh` ,`max` | 
| Knowledge cutoff | June 2026 | 
| Price per MTok, prompts up to 100K tokens | $0.10 input, $0.50 output | 
| Price per MTok, prompts over 100K tokens | $0.50 input, $2.50 output | 
| Retirement | Not sooner than October 7, 2027 | 

## Where Haiku 5.5 sits in the Claude lineup

| Model | Input / output per MTok | Default effort | Latency | 
|---|---|---|---|
| Claude Fable 5.1 | $10 / $50 | high | Slower | 
| Claude Opus 5.5 | $4 / $20 | medium | Moderate | 
| Claude Sonnet 5.5 | $2 / $10 | high | Fast | 
| Claude Haiku 5.5 | $0.10 / $0.50 for prompts up to 100K tokens | medium | Fastest | 

All four share a 1M context window, 128K max output and a June 2026 cutoff. Anthropic built Haiku 5.5 “for high-volume, latency-sensitive tasks such as classification, extraction, and routing,” plus summaries, live support, browser use and subagent work under Opus 5.5 or Sonnet 5.5.

The speed claim has a footnote: fastest “at each model’s standard speed,” but slower than Opus in Fast Mode. Anthropic publishes no tokens-per-second figure.

## Claude Haiku 5.5 pricing

The price depends on prompt length. A prompt over 100,000 tokens pays the higher rate.

Source: Claude API pricing.

The “around 75% less” figure is an average: 90% cheaper than Haiku 4.5 for prompts up to 100K tokens, 50% cheaper above, with 90% of Haiku 4.5 requests under the threshold. It already nets out the new tokenizer, which produces about 30% more tokens for the same text.

US-only inference (`inference_geo`) costs 1.1x on every token category. The minimum cacheable prompt drops to 512 tokens, from 4,096 on Haiku 4.5.

Under 100K tokens, the list price matches GPT-6 Luna exactly ($0.10 input, $0.50 output, $0.01 cache read, $0.125 cache write). The thresholds differ: Luna’s surcharge starts above 272K input tokens. The Haiku 5.5 vs GPT-6 Luna comparison works through that gap, and the pricing guide has worked cost examples.

## What changed from Haiku 4.5

The context window grows fivefold, max output doubles, and you get effort control for the first time. The migration guide warns that code written for Haiku 4.5 can break. Five changes return a 400 error:

1. **Manual thinking budgets.**`thinking: {"type": "enabled", "budget_tokens": N}` fails. Use`{"type": "adaptive"}` with`output_config.effort` .
2. **Sampling parameters.** Remove`temperature` ,`top_p` and`top_k` . Non-default`temperature` or`top_p` values fail, as does any`top_k` or sending both`temperature` and`top_p` .
3. **Assistant prefill.** Rejected, even with thinking turned off.
4. **Old computer use tool.** On the Claude API and Google Cloud, use`computer_toolset_20260801` ;`computer_20250124` fails.
5. **Edited history.** Changing`system` ,`tools` or earlier messages before a returned thinking block fails. Keep conversations append-only.

Other changes won’t throw an error but will change your results:

- **Thinking display.** Thinking blocks come back empty by default, with only a signature. Set`"display": "summarized"` to read them.
- **Token counts.** About 30% more tokens for the same text;`max_tokens` also covers thinking.
- **Forced tool use.**`tool_choice` of`any` or a named tool still works, but the response has no thinking block.
- **Refusals.** Safety classifiers can return`stop_reason: "refusal"` , with no server-side fallback.
- **Priority Tier.** Not supported on Haiku 5.5.

Thinking can be disabled on the API at `high` effort or below; at `xhigh` and `max` that returns a 400. Haiku 4.5 isn’t deprecated: it’s still listed as active. The Haiku 5.5 vs Haiku 4.5 guide has before/after JSON for each fix.

## Claude Haiku 5.5 benchmarks

Haiku 5.5 scores are at max effort, mostly averaged over five trials. Artificial Analysis ran GDPval-AA and AA-Briefcase, Cognition ran FrontierCode, and Anthropic graded Chartography (a Surge AI benchmark) with its own implementation.

Read three cells with care. FrontierCode puts Haiku at max against Sonnet 5.5 at xhigh; at max, Sonnet scored 46.2%, below Haiku’s 46.4%. Anthropic ran Luna’s OSWorld score through OpenAI’s API, while Luna’s Terminal-Bench score comes from the public leaderboard (Codex CLI). And at the default `medium` effort, Haiku 5.5 scored 1277 on GDPval-AA and 1372 on AA-Briefcase.

Anthropic says Sonnet 5.5 and Opus 5.5 “remain better choices for complex agentic coding tasks.” No independent leaderboard had a Haiku 5.5 result as of October 8, 2026; Cursor reports 48.4% on its own CursorBench at max. The benchmarks deep dive has per-effort scores and costs.

## Where you can use Claude Haiku 5.5

| Surface | Access | Notes | 
|---|---|---|
| Claude API | `claude-haiku-5-5` | Pay per token | 
| Amazon Bedrock | `anthropic.claude-haiku-5-5` | Billed through AWS Marketplace | 
| Google Cloud, Microsoft Foundry, Claude Platform on AWS | `claude-haiku-5-5` | Same id as the Claude API | 
| Claude.ai (web, iOS, Android) | Model picker | Free, Pro, Max, Team and Enterprise users can select it | 
| Claude Code | `/model claude-haiku-5-5` | v2.1.293 or later; paid plans | 
| Vercel AI Gateway | `anthropic/claude-haiku-5.5` | From $0.10 / $0.50 | 
| Cursor | `claude-haiku-5-5` | Other Models pool | 

As of October 8, 2026, it wasn’t on OpenRouter or announced for GitHub Copilot.

Chat access on the Free plan isn’t an API key, and the API is pay as you go beyond the small free credit Anthropic gives new users for testing. Max and Team plans now include monthly API credits ($100 on Max 5x, $200 on Max 20x, up to $500 pooled on Team), which don’t cover Claude Code. The free access guide lists what is and isn’t free.

In Claude Code, the `haiku` alias means Haiku 5.5 only on the Anthropic API; elsewhere it still means Haiku 4.5. See Haiku 5.5 in Claude Code for subagent setups.

## Who should switch to Haiku 5.5

- **You run Haiku 4.5:** switch, after fixing the five breaking changes. Haiku 5.5 beats Haiku 4.5 on every launch-table row where both have a score, at a lower list price.
- **You run Sonnet or Opus for simple, repeated jobs:** test Haiku 5.5 on classification, extraction, summaries and subagent work. Keep the larger model for complex agentic coding.
- **Your prompts often exceed 100K tokens:** price both tiers before you commit. Above the threshold, input costs 5x more per token.
- **You do security work:** Haiku 5.5’s cyber safeguards still block penetration testing.

In Anthropic’s launch post, Asana reports over 30% lower latency on task completions, and Box saw a score 11 points above Haiku 4.5 at about half the latency. These are customer claims, not independent tests.

The system card notes it over-refused more than any other model in Anthropic’s automated audit, so test refusal rates on your own prompts.

## Send your first Haiku 5.5 request

```
curl https://api.anthropic.com/v1/messages \
  -H "x-api-key: $ANTHROPIC_API_KEY" \
  -H "anthropic-version: 2023-06-01" \
  -H "content-type: application/json" \
  -d '{
    "model": "claude-haiku-5-5",
    "max_tokens": 4000,
    "thinking": {"type": "adaptive", "display": "summarized"},
    "output_config": {"effort": "low"},
    "messages": [
      {"role": "user", "content": "Classify this support ticket as billing, bug, or feature request: The export button returns a 500 error."}
    ]
  }'
```
Leave out sampling parameters and prefill. Read content blocks by `type`, since the response can start with a thinking block.

In Apidog, import the curl command as a new request and store the key in an `ANTHROPIC_API_KEY` environment variable. Add assertions that `stop_reason` isn’t `refusal` or `max_tokens`. Then duplicate the request at `medium` and compare `usage` to see what each effort level costs. The Haiku 5.5 API guide covers caching, batch and streaming, and the Anthropic API key guide helps if you need a key.

## FAQ

**When was Claude Haiku 5.5 released?** October 7, 2026, nine days after Sonnet 5.5.

**Is Claude Haiku 5.5 free?** In the Claude.ai chat app, Free users can select it. The API is paid, apart from small test credits for new users. See every free route.

**What is the Claude Haiku 5.5 context window?** 1M tokens, with 128K max output on the Messages API.

**Is Haiku 4.5 being retired?** Not yet. It’s still listed as active, with retirement not sooner than October 15, 2026.

## Next step

Pick one high-volume job you run today, like ticket triage or document summaries. Send it to `claude-haiku-5-5` at `low` and `medium`, check the output, and compare `usage` against your current model. Download Apidog to keep those requests and assertions in one project, then use the pricing guide to turn token counts into a monthly bill.
