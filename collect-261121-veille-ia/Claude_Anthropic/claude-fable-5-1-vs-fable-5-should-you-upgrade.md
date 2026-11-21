---
id: collect-261121-veille-ia/claude-anthropic/claude-fable-5-1-vs-fable-5-should-you-upgrade
title: "Claude Fable 5.1 vs Fable 5: Should You Upgrade?"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["fable", "same", "mtok", "claude", "tokens", "agentic", "anthropic", "upgrade"]
source: https://codersera.com/blog/claude-fable-5-1-vs-fable-5-2026/
source_anchor: ""
source_lines: [1, 173]
section: "Claude_Anthropic"
---
**Quick answer.**Switch if you use prompt caching. Claude Fable 5.1 costs the same $10/$50 as Fable 5 but charges $0.25 per million cached input tokens instead of $1.00, cutting real bills 25–45%, and it beats Fable 5 on every agentic benchmark. It is not a drop-in swap: forced

`tool_choice` now returns a 400.
Anthropic released **Claude Fable 5.1** on 1 September 2026, three months after Claude Fable 5. The headline price didn't move, which makes the upgrade look cosmetic at a glance. It isn't. The interesting number is buried in the prompt-caching row, and there's one breaking API change that will stop some agent code dead.

Here's what actually changed, what it costs, and whether it's worth your migration time.

## What actually changed between Claude Fable 5 and Fable 5.1?

The most striking thing about this comparison is how many rows are identical. That similarity is the finding — Fable 5.1 is a same-price, same-shape drop-in, with the movement concentrated in exactly three places: cache reads, knowledge cutoff, and forced tool use.

| Spec | Claude Fable 5 | Claude Fable 5.1 | 
|---|---|---|
| Model ID | `claude-fable-5` | `claude-fable-5-1` | 
| Released | 9 June 2026 | 1 September 2026 | 
| Input price | $10 / MTok | **$10 / MTok (same)** | 
| Output price | $50 / MTok | **$50 / MTok (same)** | 
| 5-minute cache write | $12.50 / MTok | **$12.50 / MTok (same)** | 
| 1-hour cache write | $20 / MTok | **$20 / MTok (same)** | 
| **Cache read** | **$1.00 / MTok** | **$0.25 / MTok (4× cheaper)** | 
| Batch API | $5 / $25 per MTok | $5 / $25 per MTok (same) | 
| Context window | 1M tokens | 1M tokens (same) | 
| Max output | 128K tokens | 128K tokens (same) | 
| Tokenizer | Opus 4.7 generation | Same tokenizer | 
| Thinking | Adaptive, always on | Adaptive, always on (same) | 
| Default effort | `high` | `high` (same) | 
| Min. cacheable prompt | 512 tokens | 512 tokens (same) | 
| **Knowledge cutoff** | Jan 2026 | **Jun 2026** | 
| **Forced `tool_choice`** | Supported | **Returns 400 error** | 
| Lifecycle status | Active (legacy) | Active (latest) | 

Because the tokenizer is unchanged, your existing token counts and cost models carry over directly — no re-baselining needed, which is not something you could say when moving from an older Claude generation.

## What does Anthropic say is better in Fable 5.1?

Anthropic names six areas, and notes the gap widens at higher `effort` levels: long-session agentic coding (multi-file features, large refactors, hours-long debugging), knowledge work producing documents, spreadsheets and slides from a blank page, multistep research and search, vision on dense charts and PDF-nested tables, reasoning across the full 1M-token window, and computer use.

Multilingual performance is explicitly called out as *on par* with Fable 5, not better. That's a useful signal: this is an agentic-work release, not a broad across-the-board lift.

## How do Fable 5.1 and Fable 5 score on the same benchmarks?

Anthropic's launch materials publish same-harness numbers for both versions. These are vendor-reported figures, so treat them as directional rather than independent, but they're measured on identical benchmarks:

| Benchmark | Fable 5 | Fable 5.1 | Delta | 
|---|---|---|---|
| Terminal-Bench-Science 0.1 (agentic research) | 24.7% | 52.6% | +27.9 pts | 
| Terminal-Bench 4.0 (agentic coding) | 42.0% | 55.8% | +13.8 pts | 
| OSWorld 2.0 (computer use, partial) | 36.1% | 41.7% | +5.6 pts | 
| CursorBench 3.2.0 (agentic coding) | 70.5% | 73.4% | +2.9 pts | 
| Humanity's Last Exam (with tools) | 63.8% | 65.0% | +1.2 pts | 

Read the spread, not the average. On **agentic** tasks the jump is large — Terminal-Bench-Science more than doubles. On **static reasoning** the jump is small: Humanity's Last Exam moves 1.2 points, which is close to noise. Fable 5.1 without tools scores 60.9% on that benchmark, below Fable 5's 63.8% with tools.

The practical translation: if your workload is a single-shot reasoning call, expect a marginal difference and let the caching economics decide. If it's a long agent loop with tools, the capability gain is real and substantial. Anthropic notably did not publish a SWE-bench Verified figure for this release.

## How much does the 4× cheaper cache actually save?

This is where most of the money is, so it's worth understanding the mechanics before the arithmetic — the discount only applies if your workload actually hits the cache.

### How prompt caching billing works

- **Cache writes** cost more than normal input: 1.25× base for the 5-minute TTL ($12.50/MTok) and 2× base for the 1-hour TTL ($20/MTok).**These are identical on both versions** — the discount does not touch writes.
- **Cache reads** (hits and refreshes) cost 0.1× base input on Fable 5 — and on every other Claude model — but**0.025× on Fable 5.1** . That's $1.00 versus $0.25 per million tokens. This is not a promotional discount but a separate pricing class: per Anthropic's pricing footnote, only Fable 5.1 and Mythos 5.1 use the 0.025× multiplier, and "all other models use the standard 0.1x multiplier".
- **Minimum cacheable prefix is 512 tokens** on both. Below that, nothing caches and*no error is returned* — a silent failure worth checking for.
- **TTL is measured from the start of the request** , not the end. If a response takes four minutes to stream, the next request has roughly one minute to land on a 5-minute cache.
- Reusing a cache refreshes its TTL at no extra charge beyond the read.

### The arithmetic on a realistic session

Take a coding agent with a **200,000-token cached prefix** (system prompt, tool definitions, repository context) running a **50-turn session** on the 5-minute TTL — one write, 49 reads:

| Line item | Claude Fable 5 | Claude Fable 5.1 | 
|---|---|---|
| 1 cache write (200K × $12.50/M) | $2.50 | $2.50 | 
| 49 cache reads (9.8M tokens) | $9.80 | $2.45 | 
| **Prefix cost for the session** | **$12.30** | **$4.95** | 

That's a **60% cut on the cached portion** of the bill. The saving isn't the full 75% because the write is unchanged and unavoidable; the longer the session runs, the more the write amortises and the closer you get to the full 4× reduction on reads.

Anthropic's own estimate for the end-to-end bill is **about 25% less than Fable 5 for typical workloads, rising to roughly 45% for highly agentic work** — lower than the 60% above because real requests also pay for uncached input and for output tokens, neither of which got cheaper.

### Does this apply to you?

Only if you re-read a cached prefix repeatedly. Concretely, you benefit if you run multi-turn agent loops over a big fixed context, serve many requests against one large system prompt or document, or keep long-lived chat sessions warm. You gain nothing if your requests are one-shot with unique prompts every time, your prefix is under 512 tokens, or your traffic is sparse enough that caches expire between calls. In those cases Fable 5.1 costs you exactly what Fable 5 did, and the decision rests purely on capability. For background on how Fable-family billing behaves in practice, see our guide to Claude Fable 5 usage and credits.

## Is the missing tool_choice parameter real?

Yes — and this is the change most likely to break running code. Third-party model listings that show Fable 5.1 without a `tool_choice` parameter are reflecting a genuine removal, not a cataloguing gap.

On Fable 5.1, `tool_choice` set to `{"type": "any"}` or `{"type": "tool", "name": "..."}` returns a 400 `invalid_request_error`:

`tool_choice: type "tool" and "any" are not supported for this model.`
`{"type": "auto"}` (the default) and `{"type": "none"}` are unchanged. The same validation applies to the token-counting endpoint, so a pre-flight token count with a forced tool choice fails too.

The rationale is coherent: thinking is always on for this model, and a forced tool call would skip it — the model would write its working-out into the tool arguments instead, lowering argument quality. Anthropic's recommended replacements:

- **For schema-valid JSON:** keep`tool_choice: {"type": "auto"}` and set`strict: true` , or move the schema to structured outputs.
- **To make the model reach for a tool instead of replying in text:** say so in the prompt ("Use the`get_weather` tool to answer"). Anthropic states Fable 5.1 follows explicit tool instructions reliably.

## What other behaviour changes should you expect?

Two further breaking changes affect anyone who builds the `messages` array by hand. First, **thinking blocks are now bound to the model that produced them** — Fable 5.1 can read earlier models' thinking, but no earlier model can read Fable 5.1's. A router that falls back mid-conversation silently drops those blocks (they aren't billed). Second, **editing earlier turns invalidates thinking blocks**: rebuilding the `system` prompt or `tools` array mid-conversation, or injecting and later removing a per-turn reminder, triggers a 400. This check is enforced for accounts created on or after 31 August 2026. Claude Code, claude.ai, and the Agent SDK handle the prefix for you.

There are also un-flagged default shifts, each with a documented prompting fix. Fable 5.1 **batches parallel tool calls less aggressively**, sometimes issuing one call per turn where Fable 5 issued several — no quality loss, but more round trips. It writes **fewer progress updates** between tool calls, **answers from memory more often at `low` effort** instead of searching, uses **less formatting in chat**, and more often **rewrites a whole file** rather than making a targeted edit.

On the additive side: per-message `effort` changes mid-conversation without invalidating the cache, turn-scoped system messages that clear at the next user turn, a `display: "updates"` option surfacing progress updates as readable text, and content provenance — Fable 5.1 output carries Anthropic's statistical text watermark, which adds no tokens or hidden characters.

## Is Claude Fable 5 deprecated, and how long will it be served?

Fable 5 is **not deprecated**. Its lifecycle status is "Active (legacy)" — still fully available on the Claude API, Amazon Bedrock, Google Cloud, Microsoft Foundry, and Claude Platform on AWS. Anthropic's retirement commitment is **not sooner than 9 June 2027**, a year out from its release. Fable 5.1's own commitment runs to no sooner than 1 September 2027.

So there is no forced-migration deadline. But there's also no reason to pay $1.00 per million cached tokens when the same money buys a better model at $0.25.

## How do you migrate from Fable 5 to Fable 5.1?

For most integrations this is a one-line change plus a short audit:

1. **Change the model ID** from`claude-fable-5` to`claude-fable-5-1` .
2. **Remove every forced `tool_choice`** of type`any` or`tool` . Move schema enforcement to strict tool use or structured outputs.
3. **Keep conversation history append-only.** If your code assembles`messages` itself, move per-turn reminders to turn-scoped system messages and`system` /`tools` edits to mid-conversation changes.
4. **Re-tune `effort`.** The default is`high` on both, but the capability gap widens at higher effort — and per-message effort is now available.
5. **Add a batching instruction** to agent loops if you see one tool call per turn where you used to get several.
6. **Re-run your evals.** Refusal handling, fallback, fallback credit, and token counts all carry over unchanged.

Fallback targets for Fable 5.1 are Claude Opus 4.8 and Claude Opus 5. If you're weighing the tier rather than the version, our comparison of Claude Fable 5.1 vs Opus 5 covers that decision, and the complete Claude Fable 5.1 guide goes deeper on the model itself. For the original release, see our Claude Fable 5 launch guide.

## Should you switch from Fable 5 to Fable 5.1?

A decision rule rather than a sales pitch:

- **Switch now** if you run long agent loops with prompt caching. You get a materially better model and roughly 25–45% off the bill. This is the clearest case in the lineup.
- **Switch soon** if you do document, spreadsheet, slide, research, or computer-use work. The benchmark gains concentrate here.
- **Switch when convenient** if your workload is one-shot reasoning with no caching. You'll pay the same and gain about a point on reasoning benchmarks — plus five months of fresher knowledge (June 2026 versus January 2026), which may matter more than the benchmark delta.
- **Test before switching** if you depend on forced tool use, edit conversation history in place, rely on parallel tool batching, or surface inter-tool narration in your UI. All four need work, none is hard.
- **Stay on Fable 5** only if you're mid-release-freeze. You have until at least June 2027, but you're paying 4× for cache reads until you move.

The honest summary: this is a rare upgrade where the pricing change and the capability change point the same direction. The cheaper cache alone justifies migrating agentic workloads, and the benchmark gains come free on top. The `tool_choice` removal is the only real friction, and it has a documented replacement.

## FAQ

### What's new in Claude Fable 5.1?

Claude Fable 5.1, released 1 September 2026, keeps Fable 5's $10/$50 pricing, 1M context and 128K output, but cuts cache reads 4× to $0.25/MTok. It improves long-session agentic coding, document and spreadsheet work, research, vision and computer use, and moves the knowledge cutoff from January to June 2026. Forced tool use is removed.

### Is Fable 5.1 more expensive than Fable 5?

No — it is cheaper. Base input ($10/MTok), output ($50/MTok), both cache write rates and the Batch API rates are identical on both models. The only price that changed is the cache read, which fell from $1.00 to $0.25 per million tokens. Anthropic estimates about 25% lower end-to-end cost on typical workloads.

### Should I upgrade from Fable 5 to 5.1?

For most users, yes. If you use prompt caching in agent loops the case is overwhelming — better benchmarks and a 25–45% lower bill. If you run one-shot reasoning without caching, the gain is roughly a point on reasoning benchmarks plus a fresher knowledge cutoff, so upgrade at your convenience. Budget time to remove forced `tool_choice` calls.

### Is Claude Fable 5 deprecated?

No. Claude Fable 5's status is "Active (legacy)" — still available on the Claude API, Amazon Bedrock, Google Cloud, Microsoft Foundry and Claude Platform on AWS. Anthropic commits to retiring it no sooner than 9 June 2027. Anthropic recommends migrating for improved performance, but there is no forced deadline and no functionality has been withdrawn.

### How much does prompt caching cost on Fable 5.1?

Cache writes cost $12.50/MTok for the 5-minute TTL and $20/MTok for the 1-hour TTL — unchanged from Fable 5. Cache reads cost $0.25/MTok, which is 0.025× the base input price, against the 0.1× multiplier every other Claude model uses. The minimum cacheable prefix is 512 tokens; shorter prefixes silently don't cache.

### Do I need to change my code?

Usually just the model ID string, from `claude-fable-5` to `claude-fable-5-1`. Beyond that, remove any `tool_choice` of type `any` or `tool` — those now return a 400 error. If your code edits conversation history in place or rebuilds the `system` or `tools` arrays mid-conversation, that needs fixing too, since it invalidates thinking blocks.

### Does Fable 5.1 support forced tool use?

No. `tool_choice` of `{"type": "any"}` or `{"type": "tool", "name": "..."}` returns a 400 `invalid_request_error`, and the same validation applies to token counting. `auto` and `none` still work. Because thinking is always on, a forced call would skip it. Use `strict: true` or structured outputs for schema validity instead.

### Did Fable 5.1 improve benchmark scores over Fable 5?

On agentic tasks, substantially: Terminal-Bench-Science 0.1 rose from 24.7% to 52.6% and Terminal-Bench 4.0 from 42.0% to 55.8%. On static reasoning the gain is small — Humanity's Last Exam with tools moved from 63.8% to 65.0%. CursorBench 3.2.0 rose 70.5% to 73.4% and OSWorld 2.0 from 36.1% to 41.7%. These are vendor-reported figures.

*Prices, benchmarks and lifecycle dates verified 3 September 2026 against Anthropic's* *pricing page**, the* *What's new in Claude Fable 5.1* *release notes, the* *Fable 5.1* *and* *Fable 5* *model pages, the* *prompt caching documentation**, and Anthropic's* *Fable 5.1 announcement**. Model pricing moves; re-check before committing to a budget.*
