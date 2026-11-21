---
id: collect-261121-veille-ia/claude-anthropic/claude-code-1m-context-window-when-it-helps-what-it-costs
title: "Claude Code 1M context window: when it helps, what it costs"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-10-22"]
keywords: ["context", "claude", "code", "opus", "tokens", "window", "costs", "cache"]
source: https://shuriken.sh/guides/claude-code-1m-context-window
source_anchor: ""
source_lines: [1, 106]
section: "Claude_Anthropic"
---
## Contents

Claude Code's 1M context window is the default on Opus 4.7 and later, Sonnet 5 and later, Haiku 5.5 and the Fable models (as of 22 October 2026). Every request re-reads the whole conversation, though, so a session at 800K tokens costs far more per turn than one at 50K. Use the room for big refactors and long logs, and `/clear` between tasks.

## Which models and plans get the 1M context window#

Anthropic's model configuration docs list it per model (read 22 October 2026):

| Model in Claude Code | 1M context | 
|---|---|
| Opus 5.5, Opus 5, Opus 4.7 | On by default, no suffix needed | 
| Sonnet 5.5, Sonnet 5 | On by default | 
| Haiku 5.5 | On by default | 
| Fable 5.1, Fable 5 | On by default | 
| Opus 4.6 | Through `opus[1m]` or`claude-opus-4-6[1m]` . Included on Max, Team and Enterprise; needs usage credits on Pro | 
| Sonnet 4.6 | Through `claude-sonnet-4-6[1m]` . Needs usage credits on every subscription plan | 

An API key gets the full 1M on every model in the table. For Opus 4.6 and Sonnet 4.6 it became generally available on 13 March 2026; the newer models ship with it built in.

`CLAUDE_CODE_DISABLE_1M_CONTEXT=1` turns it off. The `[1m]` variants then leave the model picker, and Claude Code treats the 1M models as 200K models.

## What a 1M context costs per turn#

The price per token doesn't change with size. Anthropic's docs: "the 1M context window uses standard model pricing with no premium for tokens beyond 200K", except on Haiku 5.5, which costs more above 100K tokens.

Each request just gets bigger. Claude Code sends the whole conversation every time, and one turn is often several requests, one per batch of tool calls. Read some files, run the tests, reply: three requests, each with the full context. Prompt caching makes the repeated part cheap, but not free.

Here's one request at different sizes on Opus 5.5, at API list prices from Anthropic's caching page (cache read $0.20 per million tokens, one-hour cache write $8 per million; the one-hour cache is what Claude Code gives your main conversation on a subscription; with an API key it's five minutes, $5 per million). Our arithmetic, not a measurement:

| Context size | Warm cache (read) | Cold cache (written again) | 
|---|---|---|
| 50K tokens | $0.01 | $0.40 | 
| 200K tokens | $0.04 | $1.60 | 
| 500K tokens | $0.10 | $4.00 | 
| 900K tokens | $0.18 | $7.20 | 

Warm, 900K costs $0.18 a request against $0.01 at 50K: 18 times as much before a single new token. Cold is worse. After a break longer than the cache lifetime, the first message back writes the whole context again, at 40 times the warm price on Opus 5.5, or $7.20 for one request at 900K (when the cache goes cold is covered in prompt caching in Claude Code).

On a Pro or Max plan you don't see dollars, but the tokens still count against your limits. Anthropic's cost guide puts it this way: "a one-line question in a session that has been open all day still draws usage for the whole conversation."

## Does a bigger context make Claude worse?#

It can. Anthropic's best practices open with it: "LLM performance degrades as context fills. When the context window is getting full, Claude may start 'forgetting' earlier instructions or making more mistakes."

So 1M is room to work, not a target. A session that has wandered across three tasks at 600K is worse on cost and on focus than three clean sessions.

## When the 1M context helps#

- **One large change across many files** , where Claude needs the call sites, the types and the tests in view at once.
- **A big log, trace or data file** you need read in full once. Often a subagent is better: it reads the file in its own context and hands back a summary.
- **A long run on one task** , where compacting halfway would throw away detail you still need.
- **Reading an unfamiliar codebase** before a plan, when the plan depends on how many places do the same thing.

## When /clear is better#

- **You're starting something unrelated.** Old context rides along on every message and distracts Claude.
- **You've corrected Claude twice on the same thing.** Anthropic's advice:`/clear` and start again with a better prompt.

Our guide to /compact vs /clear has the full decision table.

## Steer when it compacts#

On the 1M models Claude Code compacts on its own at **about 967K tokens** by default. If you'd rather keep sessions smaller without giving up the headroom, set an earlier point: `/autocompact 500k` (Claude Code v2.1.221 or later) saves it, for the current model only since v2.1.288, and `--autocompact 500k` sets it for one session. It accepts 100K to 1M.

If Claude Code stops with `Prompt is too long` near 200K tokens on one of these models, it may not recognise your model ID and assume a 200K window. Our guide to that error has the fix.

## How Shuriken shows it#

Shuriken doesn't change what a token costs; your agents run on your own plans and keys. It keeps the size in view. The context meter over each chat's composer opens a pop-up with:

- what fills the window, by category, much like `/context` (system prompt, MCP tools, memory files, skills, messages);
- how far the chat is from auto-compact, as a percentage and the tokens left;
- the prompt cache, shown as Warm with the time left, Going cold, or Cold ("The next message reads all â¦ again at full price");
- a **Compact** button with an optional "Keepâ¦" box (the placeholder suggests "the API design").

In a big session the cache line is the one that decides what your next message costs.

For more on where the tokens go, see why Claude Code uses so many tokens. If you'd like to see each chat's context at a glance, download Shuriken.

Try the free tool: Claude limit reset tracker

## Sources

1. Model configuration: extended context and auto-compact thresholds (Claude Code docs) (accessed 22 October 2026)
2. 1M context is now generally available for Opus 4.6 and Sonnet 4.6 (Claude blog, 13 March 2026) (accessed 22 October 2026)
3. Prompt caching: pricing (Claude API docs) (accessed 22 October 2026)
4. Best practices for Claude Code (Claude Code docs) (accessed 22 October 2026)
5. Manage costs effectively: why usage climbs in a long session (Claude Code docs) (accessed 22 October 2026)

## Frequently asked questions

## Is the 1M context window on by default in Claude Code?

Yes, on current models. As of 22 October 2026, Opus 4.7 and later, Sonnet 5 and later, Haiku 5.5 and the Fable models run with 1M by default. Opus 4.6 and Sonnet 4.6 need their [1m] variant.

## Does the 1M context cost more per token?

Not on the Anthropic API: there is no long-context premium, except on Haiku 5.5 above 100K tokens. But every request re-reads the whole context, so a big session costs more per turn.

## How do I turn the 1M context off?

Set CLAUDE_CODE_DISABLE_1M_CONTEXT=1. Claude Code then treats the 1M models as 200K models and compacts at 200K. To keep 1M but compact earlier, use /autocompact, for example /autocompact 500k.

## Does a bigger context use my subscription limits faster?

Yes. Every request carries the whole conversation, and Anthropic's cost guide says "a one-line question in a session that has been open all day still draws usage for the whole conversation." Cached re-reads cost less, but they still count.
