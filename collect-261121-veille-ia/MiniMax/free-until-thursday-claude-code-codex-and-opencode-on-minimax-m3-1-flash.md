---
id: collect-261121-veille-ia/minimax/free-until-thursday-claude-code-codex-and-opencode-on-minimax-m3-1-flash
title: "Free Until Thursday: Claude Code, Codex and OpenCode on MiniMax M3.1 Flash"
domain: minimax
role: reference
task: article
actors: ["MiniMax"]
dates: ["2026-10-19"]
keywords: ["flash", "free", "minimax", "claude", "ollama", "until", "code", "input"]
source: https://medium.com/@r3dtuxedo/free-until-thursday-claude-code-codex-and-opencode-on-minimax-m3-1-flash-41292135b0da
source_anchor: ""
source_lines: [1, 172]
section: "MiniMax"
---
# Free Until Thursday: Claude Code, Codex and OpenCode on MiniMax M3.1 Flash

The upgrade to my 95% cheaper Claude Code setup: 1M context, image input, half the M2.7 price, and three copy-paste setups you can try before the free window closes

*TL;DR*

- **MiniMax M3.1 Flash** came out on 19 Oct 2026. It is**free on Ollama Cloud until Thu 22 Oct, 23:59 UTC** (that is**Fri 23 Oct, 01:59 Berlin time** ).
- OpenRouter’s `:free` variant ends earlier:**22 Oct, 12:00 UTC (14:00 Berlin)** .
- After the free period, the list price is **$0.15 input / $0.60 output / $0.015 cached input per 1M tokens** . That is half of what I paid for MiniMax-M2.7.
- One command gets Claude Code onto it: `ollama launch claude --model minimax-m3.1-flash:cloud` . Codex and OpenCode setups are below.
- Use it for mechanical worker tasks. Keep a frontier model for planning and review.

Earlier this year I wrote Cut Your Claude Code Bill by Up to 95%. It pointed Claude Code at MiniMax-M2.7 through MiniMax’s Anthropic-compatible endpoint, using two shell functions, `claudecode` and `claudeminimax`. It also covered what breaks and how to pair it with oh-my-claudecode.

That setup still works as an idea, but the model under it just got replaced. MiniMax released M3.1 Flash on 19 October, and Ollama is serving it for free for about 72 hours. If you have ever wanted to test a cheap backend on real work without paying for the experiment, this is the week to do it.

This article covers what changed, how to wire up three agents, and what still breaks.

## What changed from M2.7 to M3.1 Flash

- **Input / output per 1M tokens:** M2.7: $0.30 / $1.20 → M3.1 Flash: $0.15 / $0.60
- **Cached input per 1M tokens:** M2.7: n/a in my setup → M3.1 Flash: $0.015
- **Context window:** M2.7: 204,800 tokens → M3.1 Flash: 1,000,000 (MiniMax API); 976K listed on Ollama
- **Image input:** M2.7: No (text and tool calls only) → M3.1 Flash: Yes (images and video on MiniMax’s API; image input on Ollama)
- **Thinking:** M2.7: Always on → M3.1 Flash: Always on, depth tunable from`low` to`max` (default`max` )
- **Size:** M2.7: ~230B total, ~10B active → M3.1 Flash: 428B parameters (per Ollama); active count not published
- **Free option right now:** M2.7: none → M3.1 Flash: Ollama Cloud until 22 Oct 23:59 UTC; OpenRouter`:free` until 22 Oct 12:00 UTC

Sources: MiniMax’s launch post, pricing and Anthropic API docs; the Ollama model page and pricing page.

The headline claims come from MiniMax and are repeated on Ollama’s page: “nearly three times faster” inference and “usage costs reduced by 70%” versus the previous generation. MiniMax says output can reach 200+ tokens per second, with a median above 170. OpenRouter’s public stats for the free endpoint showed a median of about 76 tokens per second over the last three days. A free launch endpoint under heavy load is not a fair speed test, but don’t plan around 200.

Against Anthropic’s current list prices (pricing docs), M3.1 Flash output is 94% cheaper than Sonnet 5.5 ($10) and 97% cheaper than Opus 5.5 ($20). Tokenizers differ between vendors, and a model that thinks longer uses more tokens, so treat this as a price per token, not a price per task.

## Benchmarks, with a warning

MiniMax claims 73.7% on DeepSWE v1.1, 61.7 on OSWorld V2, 29.6 on Agents’ Last Exam, 76.2 on BankerToolBench and 82% on CyberGym. Those are MiniMax’s own numbers.

The independent numbers are more mixed. Artificial Analysis (as shown on OpenRouter) gives it 42.7 on its Intelligence Index, rank 21 of 351 and just below Claude Haiku 5.5. It also scores 7.2% on AA-Omniscience’s non-hallucination rate, which ranks 256th of 280. When M3.1 Flash doesn’t know something, it usually makes up an answer instead of saying so. VentureBeat’s write-up makes the same point. Keep that in mind when you read the advice at the end.

## Three setups

There are two routes. **Ollama** is free until the deadline and needs a free Ollama account. The **MiniMax API** is the paid route that keeps working afterwards. Never paste a real key into a config file you commit. Keep keys in your environment or a secrets manager.

## 1. Claude Code

**Option A:** **ollama launch**

`ollama launch claude --model minimax-m3.1-flash:cloud`
Anything after `--` is passed through to Claude Code, and `--yes` skips the prompts in scripts:

`ollama launch claude --model minimax-m3.1-flash:cloud --yes -- -p "summarize this repo"`
**Option B: environment variables.** Through a signed-in local Ollama:

```
ANTHROPIC_AUTH_TOKEN=ollama \
ANTHROPIC_API_KEY="" \
ANTHROPIC_BASE_URL=http://localhost:11434 \
claude --model minimax-m3.1-flash:cloud
```
Or straight to Ollama’s cloud, with no local install. Direct cloud calls use the hosted name without `:cloud`:

```
ANTHROPIC_BASE_URL=https://ollama.com \
ANTHROPIC_AUTH_TOKEN="$OLLAMA_API_KEY" \
ANTHROPIC_API_KEY="" \
claude --model minimax-m3.1-flash
```
Ollama notes that Claude settings files can override shell variables. Run `/status` inside Claude Code to check which endpoint it is actually using.

**Option C: MiniMax direct, the updated** **claudeminimax****.** This is the function from the first article, updated with the model name and variables from MiniMax's Claude Code guide. The `[1m]` suffix and the 512K auto-compact window are MiniMax's recommendations.

```
# ~/.zshrc: reads MINIMAX_API_KEY from your env or secrets manager
function claudeminimax(){
  export ANTHROPIC_BASE_URL=https://api.minimax.io/anthropic
  export ANTHROPIC_AUTH_TOKEN=$MINIMAX_API_KEY
  export ANTHROPIC_API_KEY=""
  export ANTHROPIC_MODEL="MiniMax-M3.1-Flash[1m]"
  export ANTHROPIC_DEFAULT_OPUS_MODEL="MiniMax-M3.1-Flash[1m]"
  export ANTHROPIC_DEFAULT_SONNET_MODEL="MiniMax-M3.1-Flash[1m]"
  export ANTHROPIC_DEFAULT_HAIKU_MODEL="MiniMax-M3.1-Flash[1m]"
  export CLAUDE_CODE_AUTO_COMPACT_WINDOW=524288
  exec claude "$@"
}
```
You should update the old function anyway. MiniMax’s Anthropic SDK page now lists only `MiniMax-M3.1-Flash` and `MiniMax-M3` as supported on that endpoint. Because the function ends in `exec`, the variables die with that session, so a plain `claude` in a new shell is back on Anthropic. MiniMax's docs also offer a setup wizard (`npx -y mmx-cli@latest agent setup`) that writes `~/.claude/settings.json` for you. Remember that environment variables override that file.

## 2. Codex CLI

Ollama’s Codex docs cover a launcher, too:

`ollama launch codex          # pick minimax-m3.1-flash:cloud in the model picker`
Or the manual route through Codex’s built-in Ollama provider:

`codex --oss -m minimax-m3.1-flash:cloud`
One caveat: Ollama’s model page lists Claude Code, OpenCode, Hermes and OpenClaw as apps for this model, but not Codex. Codex should work through the same launcher, but this pairing is less tested, so start with a small task.

For the paid route, this is MiniMax’s own Codex config. I swapped the inline `experimental_bearer_token` for Codex's documented `env_key`, so the key stays out of the file:

```
# ~/.codex/config.toml
model = "MiniMax-M3.1-Flash"
model_provider = "minimax"
model_context_window = 524288
```
```
[model_providers.minimax]
name = "MiniMax"
base_url = "https://api.minimax.io/v1"
env_key = "MINIMAX_API_KEY"
wire_api = "responses"
```
MiniMax’s docs also include an optional model catalog that adds reasoning levels to `/model`. Don't add an `"none"` effort level: M3.1 Flash always thinks, and the API returns a 400 error.

## 3. OpenCode

Ollama’s model page gives the one-liner:

`ollama launch opencode --model minimax-m3.1-flash:cloud`
It starts OpenCode with an inline config and leaves `~/.config/opencode/opencode.json` untouched (Ollama OpenCode docs). For MiniMax direct, use `/connect` (or `opencode auth login`), pick MiniMax, and select `MiniMax-M3.1-Flash` in `/models`. If your version doesn't list the model yet, MiniMax's OpenCode page shows the model block to add:

```
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "minimax-coding-plan": {
      "models": {
        "MiniMax-M3.1-Flash": {
          "name": "MiniMax-M3.1-Flash",
          "limit": { "context": 524288 }
        }
      }
    }
  }
}
```
(`minimax-coding-plan` is the provider for MiniMax's subscription key. Pay-as-you-go keys go through OpenCode's regular MiniMax provider.)

## What still breaks, and the caveats

**The free tier is narrow.** Ollama’s Free plan allows one concurrent request. Requests beyond that are queued, and rejected when the queue is full. Claude Code fanning out to parallel subagents will feel slow. Pro allows 3 concurrent requests and Max allows 10. I couldn’t confirm whether a Free account needs purchased credits to use this model during the free window. Ollama’s pricing page says Free covers “starter models” and credits unlock the rest, so check your account. OpenRouter’s `:free` endpoint is rate-limited as well.

**No Anthropic prompt caching semantics.** Ollama’s Anthropic compatibility page lists `cache_control` prompt caching as unsupported, together with `tool_choice`, token counting, PDF document blocks and batches. Hosted web search is not fully supported either. Ollama and MiniMax do list a cached-input price ($0.015), but the caching is done their way, not Claude Code's.

**Thinking is always on, and defaults to maximum.** MiniMax’s API rejects `thinking: disabled` with a 400 error. With effort defaulting to `max`, the model can burn many thinking tokens. One r/MiniMax_AI user reported that M3.1 Flash hit their plan limits faster than M2.7. For simple work, try a lower effort level. I haven't measured how much that saves.

**Guard your secrets.** A r/ollama thread from today reports the model reading `.env` files without being asked to. That's one user's report, but adding deny rules is cheap. Claude Code supports them in `settings.json`:

`{ "permissions": { "deny": ["Read(./.env)", "Read(./.env.*)"] } }`
**Data handling.** Ollama’s launch email says M3.1 Flash is “hosted in the US with zero data retention”. Its pricing page says models run primarily in the US and may be routed to Europe for capacity. Another r/ollama thread asks which inference provider actually serves a model whose weights aren’t public yet. I haven’t seen an answer to that.

**After the deadline.** At 23:59 UTC on 22 Oct, Ollama starts charging $0.15 / $0.60 per 1M tokens from your usage credits. Nothing in your config needs to change. On OpenRouter, the `:free` variant is retired after 12:00 UTC and you have to switch to `minimax/minimax-m3.1-flash`. To keep the same price without Ollama, use the MiniMax-direct configs above. To go back to Claude, open a new shell and run plain `claude` (or `codex`/`opencode` with your usual profile).

## How I’d actually use it

Don’t move your whole workflow over. Treat M3.1 Flash as a cheap worker:

- **Give it:** test scaffolding, mechanical refactors, renames across files, migrations with a clear spec, lint fixes, log triage, first drafts of docs, screenshots of UI bugs (it accepts images now).
- **Keep on a frontier model:** planning, architecture decisions, security-sensitive changes, and the final review of anything M3.1 Flash wrote.

The hallucination score is the reason for that split. A cheap model that is confidently wrong is fine when tests or a reviewer catch it. It is not fine when it is the one deciding what’s true.

Two habits make the split work across tools. First, put your project rules in **AGENTS.md**. Codex reads it, and Claude Code can read `AGENTS.md` as its project instructions, so every agent and every backend starts from the same rules. Second, hand work over with a written spec and acceptance checks instead of a long chat, so a cheap worker gets a small, clear context. I wrote about that orchestration pattern in Stop Burning Tokens: How Agent Orchestration and Knowledge Graphs Cut AI Coding Costs by 70%. The original MiniMax setup is in Cut Your Claude Code Bill by Up to 95%.

## Try it before Thursday night

The free window is short, so plan a real test while it’s open. Pick three tasks from your backlog that you would normally give a mid-tier model. Run them through `ollama launch claude --model minimax-m3.1-flash:cloud` (or Codex, or OpenCode), and have your usual frontier model review the diffs. You'll learn more from that than from any benchmark table, including the ones above.

**Deadline again: Ollama Cloud free until 22 Oct 2026, 23:59 UTC (23 Oct, 01:59 Berlin). OpenRouter** **:free** **until 22 Oct, 12:00 UTC (14:00 Berlin).**

*Prices and limits as published on 21 Oct 2026. Check the linked pages before you rely on them.*
