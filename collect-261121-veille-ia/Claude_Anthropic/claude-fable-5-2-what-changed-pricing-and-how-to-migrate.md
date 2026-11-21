---
id: collect-261121-veille-ia/claude-anthropic/claude-fable-5-2-what-changed-pricing-and-how-to-migrate
title: "Claude Fable 5.2: What Changed, Pricing and How to Migrate"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-11-10"]
keywords: ["claude", "fable", "code", "prefix", "account", "mtok", "check", "every"]
source: https://www.developersdigest.tech/blog/claude-fable-5-2-whats-new-migration-2026
source_anchor: ""
source_lines: [1, 200]
section: "Claude_Anthropic"
---
TL;DR

Claude Fable 5.2 costs $8/$40 per MTok, down from $10/$50, but enforces the prefix check on every account. The four breaking changes and how to migrate.

**Last updated:** November 10, 2026 (fast mode on Fable 5.2 in Claude Code, and Claude Code 2.1.322's fast mode on Vertex). First published November 9, 2026

Anthropic shipped Claude Fable 5.2 (`claude-fable-5-2`) on November 9, 2026, at $8 per million input tokens and $40 per million output tokens, 20 percent below Claude Fable 5.1's $10 and $50, with a 1M token context window and 128k max output tokens. The catch is in the request handling, not the price: four changes from Fable 5.1 can break working code, and the biggest one, the prefix check on every account, can fail code that never failed before.

Everything below comes from the November 9 release notes, What's new in Claude Fable 5.2 and the migration guide, read on launch day. The code was type-checked against `anthropic` 2.7.0 and `@anthropic-ai/sdk` 1.7.0, both published the same day. I did not have an API key in this environment, so none of it was run against the live API.

## The release in one table#

|  | Claude Fable 5.2 | Claude Fable 5.1 | 
|---|---|---|
| Model ID | `claude-fable-5-2` | `claude-fable-5-1` | 
| Input / output per MTok | $8 / $40 | $10 / $50 | 
| Cache reads per MTok | $0.20 | 0.025x base input, same multiplier | 
| Thinking | Adaptive, always on | Adaptive, always on | 
| Default effort | `high` | `high` | 
| Prefix check | Enforced on every account | Enforced on accounts created on or after Aug 31, 2026 | 

Fable 5.2 has a 1M token context window (default and maximum), 128k max output tokens and fast mode on the Claude API (`speed: "fast"`, $16 / $80 per MTok, no beta header). Claude Mythos 5.2 (`claude-mythos-5-2`) has the same capabilities and prices but is only available to organizations verified through Anthropic's programs, such as the Cyber Verification Program. Both models require 30-day data retention and are not available under zero data retention unless Anthropic expressly authorizes it. Batch processing is $4 / $20 per MTok, 5-minute cache writes are $10 and 1-hour cache writes are $16.

Anthropic's own guidance on when to use it is worth quoting: "For most workloads, start with the latest Claude Opus model ... and move to Claude Fable 5.2 when your evals on a Claude Opus model at higher effort still don't meet your requirements." If you are choosing between models rather than upgrading, our Fable 5 effort levels guide covers the effort trade-off that decision rests on.

## The four breaking changes#

### 1. The prefix check now applies to every account#

When you send back a thinking block, the API checks that nothing before it has changed since the model produced it: the `system` prompt, the `tools` or an earlier message. On Fable 5.1 a failed check returned a 400 only on accounts created on or after August 31, 2026. On Fable 5.2 and Mythos 5.2 it returns a 400 "whatever the account's age", with a message containing `The block is bound to a different conversation`.

The docs spell out the trap: "code on Claude Fable 5.1 that edits its history can run without errors on an older account before the switch and fail after it."

You are not affected if Claude Code, claude.ai, Claude Managed Agents or the Claude Agent SDK builds your requests, or if your own code keeps `system` and `tools` fixed for the whole conversation, only appends to `messages`, and sends each assistant turn back exactly as the API returned it. Otherwise, test first. The migration guide's recommended test is to run a session with the `thinking-binding-controls-2026-08-01` beta header and `prefix_mismatch_behavior: "drop_block"`, log `input_transformations`, and fix every `prefix_binding_mismatch` entry. In production, leave the setting at its default, `"error"`, if a mismatch can only mean a bug, and treat `"drop_block"` as a stopgap, because dropping blocks can raise output tokens.

Two newer features exist to help you avoid edits: compaction on demand (beta, `compact-2026-09-04`) and mid-conversation tool changes (beta, `inline-tools-2026-09-15` on the Claude API), which add or remove tools in a `role: "system"` message instead of editing `tools`.

### 2. Only Fable 5.2 and Mythos 5.2 read Fable 5.2 thinking blocks#

Moving a conversation onto Fable 5.2 keeps its reasoning: on the Claude API and Claude Models on Google Cloud (Google managed), Fable 5.2 reads thinking blocks from Opus 5.5, Sonnet 5.5, Haiku 5.5, Fable 5.1, Mythos 5.1 and earlier Opus, Sonnet and Haiku models. Moving the other way loses it. Every model except Mythos 5.2 drops Fable 5.2's blocks, including Opus 5.5, Fable 5.1, and Fable 5.2's own fallback models, Claude Opus 4.8 and Claude Opus 5. The request still succeeds and dropped blocks are not billed, but a router that bounces long agent sessions between Fable 5.2 and a cheaper model will quietly lose the expensive model's reasoning on every bounce.

Fable 5.2 thinking blocks are also bound to the account that produced them, or an account linked to it. Another account's blocks are dropped with `reason: "organization_binding_mismatch"`.

### 3. `computer_20251124` returns a 400 outside Bedrock#

Fable 5.1 accepted both the `computer_toolset_20260801` toolset and, with a beta header, the older `computer_20251124` tool. Fable 5.2 and Mythos 5.2 return a 400 `invalid_request_error` for the older tool "on every platform except Amazon Bedrock". Move to the toolset before you switch models.

### 4. A Fable 5.2 executor needs a Fable 5.2 or Mythos 5.2 advisor#

With the advisor tool (beta), the advisor must be at least as capable as the executor. A Fable 5.2 executor with any other advisor, Fable 5.1 included, gets a 400. Change the advisor's `model` to `claude-fable-5-2` or `claude-mythos-5-2` in the same change, and do it in new conversations: changing the advisor's `model` is an edit to `tools`, which fails the prefix check above. The reverse is easy: every executor model accepts Fable 5.2 as its advisor, and a Fable 5.2 advisor returns its advice encrypted.

## Rules that are not new, but bite if you skip Fable 5.1#

If you are coming from Fable 5.1, Mythos 5.1 or Opus 5.5, these are unchanged. From Opus 5 or older, both are breaking:

- **Thinking is always on.**`thinking: {"type": "disabled"}` and`{"type": "enabled", "budget_tokens": N}` both return a 400. Omit`thinking` or send`{"type": "adaptive"}` , and use effort.
- **No forced tool use.**`tool_choice` of`{"type": "any"}` or`{"type": "tool", ...}` returns a 400. For schema-valid JSON, keep`tool_choice: {"type": "auto"}` with`strict: true` , or use JSON outputs. In organizations with customer-managed encryption keys, structured outputs are not available on Fable 5.2 at all.

Coming from Opus 5.5, also note that Fable 5.2's default effort is `high` where Opus 5.5's is `medium`, so an unchanged request runs one level higher. Set `effort` explicitly.

## How to switch#

Change the model ID, set effort explicitly, and turn on the audit while you test:

```
import anthropic
client = anthropic.Anthropic()
# 1. A plain Fable 5.2 request. Set effort explicitly: the default is "high".
reply = client.messages.create(
    model="claude-fable-5-2",
    max_tokens=16000,
    output_config={"effort": "high"},
    messages=[{"role": "user", "content": "Summarize the tradeoffs of event sourcing."}],
)
for block in reply.content:
    if block.type == "text":
        print(block.text)
# 2. Fast mode on the Claude API: no beta header, $16/$80 per MTok.
client.messages.create(
    model="claude-fable-5-2",
    max_tokens=4000,
    speed="fast",
    messages=[{"role": "user", "content": "One-line status update, please."}],
)
# 3. Find history edits before they become 400s: drop failing blocks and log them.
audited = client.beta.messages.create(
    model="claude-fable-5-2",
    max_tokens=16000,
    betas=["thinking-binding-controls-2026-08-01"],
    thinking={"type": "adaptive", "block_binding": {"prefix_mismatch_behavior": "drop_block"}},
    messages=[{"role": "user", "content": "Continue the refactor plan."}],
)
for t in audited.input_transformations or []:
    if t.type == "thinking_dropped":
        print(t.path, t.reason)
```
Pyright in strict mode reports 0 errors against `anthropic` 2.7.0, and misspelling `"drop_block"` as `"drop_blocks"` fails the check. The same three calls in TypeScript (`client.messages.create`, `speed: "fast"`, `client.beta.messages.create` with `betas` and `thinking.block_binding`) pass `tsc --strict` against `@anthropic-ai/sdk` 1.7.0. In the SDK types, `reason` on a `thinking_dropped` entry is one of `model_binding_mismatch`, `prefix_binding_mismatch`, `organization_binding_mismatch` or `end_user_binding_mismatch`. After a deliberate model switch, `model_binding_mismatch` entries are expected; `prefix_binding_mismatch` entries are the edits to fix.

Two practical notes from the migration guide. Thinking tokens count toward `max_tokens`, so a small `max_tokens` can stop with `stop_reason: "max_tokens"` after the thinking block and before any text. And if code reads `content[0].text`, select blocks by `type` instead, because thinking comes first.

For fast mode elsewhere: it uses the same `speed: "fast"` on Claude Models on Google Cloud (both Anthropic and Google managed), while on Microsoft Foundry it is a separate `claude-fable-5-2-fast` deployment with `speed` left out. Remove `speed: "fast"` in Claude Managed Agents. If you are weighing fast mode at all, our Claude Code fast mode breakdown covers when the 2x price pays off.

## Behavior to re-check in your prompts#

From the "Behavior differences" section, the items most likely to show up in an existing agent:

- **"Answer directly" lines stop working.** Fable 5.1 usually follows a line like "answer directly without deliberating". In Anthropic's testing, Fable 5.2 still thought at`medium` effort and above, and at`high` it "thinks before almost every reply." Remove those lines and lower effort for easy turns.
- **Turns can end early.** On long multistep tasks the model sometimes stops with a list of what remains or an offer to continue, more often at`low` and`medium` .
- **It may review itself with subagents.** At`xhigh` and`max` in an agentic coding product, Anthropic saw Fable 5.2 start subagents to review its own work, which Fable 5.1 did not do. "Subagents can account for a large part of what a session costs."
- **Code ownership in the prompt matters.** Without it, the model tended to add to the existing structure and report nearby problems instead of fixing them.

Refusals work as before: HTTP 200 with `stop_reason: "refusal"` and `stop_details`. The permitted fallback targets for Fable 5.2 are Opus 4.8 and Opus 5, which, per change 2, cannot read its thinking blocks. If you already handle refusals, our Fable 5 refusal handling guide still applies.

## In Claude Code#

Claude Code 2.1.321 adds `claude-fable-5-2` as "now the default Fable model". Saved Fable 5.1 picks move to `fable`: on Pro and Max plans at once, and on Team and Enterprise plans once your organization allows it. To stay on the older model, run `/model claude-fable-5-1`. Because Claude Code builds its own requests, the prefix check change does not affect it. The same release also fixes fast mode retrying for minutes when fast serving is rate-limited: it now tries standard speed on the same model first. How the previous default switch played out is in our Claude Code Fable 5.1 default post.

Fable 5.2 is also one of the four models that Claude Code's experimental Code Mode supports, along with Opus 5.5, Sonnet 5.5 and Haiku 5.5.

### Fast mode on Fable 5.2 in Claude Code#

Per the Claude Code fast mode docs, fast mode on Fable 5.2 "requires Claude Code v2.1.321 or later" and costs $16/$80 per MTok, against $8/$40 for fast mode on Opus 5.5. On subscription plans it is "available via usage credits only and not included in the subscription rate limits." Two Fable-specific details:

- **It does not carry over from Opus.** Switching to Fable 5.2 turns fast mode off "unless you turned it on while using Fable 5.2". Run`/fast` on Fable 5.2 to turn it on; unless you set per-session opt-in, it then lasts across sessions.
- **The setting alone does nothing on Fable 5.2.**`"fastMode": true` in user settings "has no effect until you run`/fast` while using Fable 5.2."

Claude Code 2.1.322 (November 10) adds "fast mode on Vertex in the global, us and eu regions, falling back to standard speed if Vertex refuses it". The changelog does not name the models, and the fast mode docs had no Vertex section yet when we checked, so confirm with `/fast` and the `↯` icon next to the prompt that a Vertex session is actually running fast.

## Who should move now#

- **You use an agent framework:** check its version first. Pydantic AI 2.67.0 added`claude-fable-5-2` and lists it among the models that bind thinking blocks to the conversation prefix.
- **You use Claude Code, Managed Agents or the Agent SDK:** nothing to change beyond re-checking cost, since the price per token drops.
- **You build `messages` yourself and never edit history:** switch, set effort explicitly, re-run evals.
- **You edit history, route between models, or use the advisor tool or `computer_20251124`:** run the`drop_block` audit first, fix the edits, then switch. If you skip this on an older account, Fable 5.2 can turn a working integration into a stream of 400s.

For the previous release's API details, see our Fable 5.1 and Mythos 5.1 API guide.

## FAQ#

### How much does Claude Fable 5.2 cost?#

$8 per million input tokens and $40 per million output tokens, down from $10 and $50 for Claude Fable 5.1. Cache reads are $0.20 per MTok, 5-minute cache writes $10 and 1-hour cache writes $16. Batch is $4/$20, and fast mode on the Claude API is $16/$80.

### What breaks when I switch from Fable 5.1 to Fable 5.2?#

Four things: the prefix check returns a 400 on every account if your code edits `system`, `tools` or an earlier message and sends back a later thinking block; only Fable 5.2 and Mythos 5.2 read Fable 5.2 thinking blocks; the `computer_20251124` tool returns a 400 outside Amazon Bedrock; and a Fable 5.2 executor needs a Fable 5.2 or Mythos 5.2 advisor.

### Does Claude Code use Fable 5.2 automatically?#

Claude Code 2.1.321 makes `claude-fable-5-2` the default Fable model and moves saved Fable 5.1 picks to `fable`: at once on Pro and Max plans, and on Team and Enterprise plans once the organization allows it. Run `/model claude-fable-5-1` to switch back.

### Can I turn thinking off on Claude Fable 5.2?#

No. Adaptive thinking is always on, and `thinking: {"type": "disabled"}` or `{"type": "enabled", "budget_tokens": N}` returns a 400 error. Use the effort parameter instead; the default is `high`.

### Is fast mode available for Claude Fable 5.2?#

Yes, in Messages API requests on the Claude API with `speed: "fast"` and no beta header, at $16 per MTok input and $80 output. It is also on Claude Models on Google Cloud, and on Microsoft Foundry as a separate `claude-fable-5-2-fast` deployment.

## Sources#

- Claude Platform release notes, November 9, 2026
- What's new in Claude Fable 5.2
- Claude Fable 5.2 and Claude Mythos 5.2 migration guide
- Preserved thinking: the prefix check
- Prompting Claude Fable 5.2
- Claude Code CHANGELOG, 2.1.321
- anthropic on PyPI and @anthropic-ai/sdk on npm - the versions type-checked here

## Continue Reading#

- Claude Fable 5.1 and Mythos 5.1 API Guide - the release this one replaces
- Claude Code Fable 5.1 Default Model - how the last default switch worked in Claude Code
- Fable 5 Effort Levels Explained - the setting that now controls Fable 5.2's thinking
- Claude PCB Design: Fable 5.2 Built Three Working Prototypes - Anthropic's electronics design research with Fable 5.2
- AI API Pricing Comparison - per-token input and output prices side by side
- Where to Access AI Models (2026) - official APIs, clouds and routers compared

## Get the next comparison like this in your inbox

One email a week on Anthropic and the rest of the AI dev stack. Free.

Read next on Claude Code

### Claude Fable 5.1 and Mythos 5.1: The Developer Migration Guide

Claude Fable 5.1 keeps $10/$50 pricing but cuts cache reads to $0.25/MTok. The three breaking API changes, new betas, and when Opus 5.5 is enough.

16 min read
### Claude Fable 5.1 in Claude Code: How to Switch, Pin, and Pay Less

Claude Code v2.1.257 points /model fable at Fable 5.1: 1M context, $10/$50 per MTok, $0.25 cache reads. Setup, gotchas, and when Opus 5.5 wins.

11 min read
### Claude Effort Levels Explained: Fable 5, Opus 5.5, Haiku 5.5

Claude effort levels explained: what low, medium, high, xhigh and max change on Fable 5, Opus 5.5 and Haiku 5.5, each default, and what a level costs.

14 min read
Technical content at the intersection of AI and development. Building with AI agents, Claude Code, and modern dev tools - then showing you exactly how it works.
