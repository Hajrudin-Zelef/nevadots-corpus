---
id: collect-261121-veille-ia/claude-anthropic/claude-fable-5-2-is-here-do-you-really-need-it-for-vibe-coding
title: "Claude Fable 5.2 is here. Do you really need it for vibe coding?"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-11-09"]
keywords: ["opus", "fable", "claude", "sonnet", "anthropic", "model", "price", "cursor"]
source: http://dev.to/kondasviktor/claude-fable-52-is-here-do-you-really-need-it-for-vibe-coding-29gj
source_anchor: ""
source_lines: [1, 116]
section: "Claude_Anthropic"
---
**Disclosure:** No sponsorship or affiliate links in this article. Benchmarks cited are lab-reported (Anthropic / Cursor). Prices are list prices in USD as of 13 Nov 2026.


Confession first: **I haven't used Claude Fable 5.2 yet.** I'm not against it. I just haven't worked on any project that Opus 5.5 couldn't handle.

Most of what I build is landing pages, small SaaS features, Apify Actors and bug fixes. That work doesn't need the biggest brain in the room. So this is a practical guide: what Fable 5.2 is, what it costs, what Claude Pro actually shows this week, and where it fits next to the models I already use.

- 
**Do you need Fable 5.2 for vibe coding?** Probably not. I haven't needed it yet.
- 
**Price:****$8 / $40** per million tokens — 2× Opus 5.5, 4× Sonnet 5.5, and a permanent**20%** cut from Fable 5.1 (not 50%).
- 
**My split:** Sonnet 5.5 builds, tests and deploys. Opus 5.5 plans and handles auth and payments. Fable only when Opus gets stuck.
- 
**Claude Pro:** Fable normally runs on usage credits. Anthropic says it's included up to 50% of your weekly limit**until 16 Nov** . That's a usage share, not a half-price sale.

From Anthropic's launch post and model docs:

- Released **9 Nov 2026** . Model ID`claude-fable-5-2` .
- 
**$8 / $40** (was $10 / $50 on 5.1). Optional fast mode**$16 / $80** .
- Up to **30% faster** output than Fable 5.1.
- 
**1M** context /**128K** max output (same as Opus 5.5 and Sonnet 5.5).
- Available in the Claude apps, Claude Code, Claude Platform, AWS, Google Cloud, Microsoft Foundry, and Cursor's model list.
- Anthropic reports Fable 5.2 ahead of Opus 5.5 on most of its own benches. On FrontierCode 2.0: **49.1%** (Xhigh) vs Opus**45.5%** (Max). At High effort Fable scored**45.3%** — about level with Opus at Max — for roughly**half the cost per task** (Anthropic's numbers).

OpenRouter and Vercel AI Gateway list the same price. Vercel keeps Fable prompts/completions **30 days** for Anthropic safety monitoring. Fable 5.2 is not on OpenRouter's EU in-region routing list right now.

| Benchmark | Fable 5.2 | Fable 5.1 | Opus 5.5 | vs Opus | 
|---|---|---|---|---|
| FrontierCode 2.0 | **49.1%** | 32.4% | 45.5% | +3.6 | 
| FrontierSWE v2 | **69.9%** | 56.3% | 62.3% | +7.6 | 
| OSWorld 2.1 | **89.3%** | 83.5% | 87.2% | +2.1 | 
| Humanity's Last Exam | **74.2%** | 65.6% | 67.7% | +6.5 | 

The jump from 5.1 is large. The lead over Opus is smaller: a few points for twice the token price.

| Model | Price in / out | CursorBench 4.0 best | 
|---|---|---|
| Claude Fable 5.2 | $8 / $40 | 60.0% · $32.79 (Max) | 
| Claude Opus 5.5 | $4 / $20 | 57.8% · $13.43 (Max) | 
| Claude Sonnet 5.5 | **$2 / $10** | 55.5% · $7.05 (Max) | 
| Grok 4.7 (Cursor) | **$2 / $6** | 46.3% · $6.01 (Xhigh) | 
| Gemini 4 Carbon | $2 / $10 (intro to 30 Nov) | **60.4%** · $9.22 (High) | 

**Takeaway:** Sonnet stays within about **5 points** of Fable on CursorBench at under a quarter of the cost per task. The best model and the best model *for you* aren't the same thing.

Anthropic itself says start most coding on **Opus 5.5** and move up to Fable when Opus at `high` still falls short (or for long unattended runs). See their model guide.

My default is **Sonnet 5.5** on a $20 plan — more building before you hit the limit:

| Model | Job | Price | 
|---|---|---|
| Sonnet 5.5 | Build, test, deploy | $2 / $10 | 
| Opus 5.5 | Plan, auth, payments | $4 / $20 | 
| Fable 5.2 | When Opus is stuck | $8 / $40 | 
| Grok 4.7 (Cursor) | Everyday Cursor agent | $2 / $6 | 

Sonnet is half Opus's token price and close on most benches. Opus pulls away on hard multi-file work (SWE-bench Pro: Opus **89.9%** vs Sonnet **81.3%** on Anthropic's table). That's why I use Opus for thinking and Sonnet for doing.

1. Plan with **Opus 5.5 at High** — written plan, tests, definition of done.
2. Hand the plan over as a **document** (switching models drops hidden reasoning).
3. Build, test, deploy with **Sonnet 5.5** .
4. Auth and payments go back to Opus.
5. Escalate to Fable only when Opus at High is stuck.

Claude Code shortcuts (v2.1.321+): `/model sonnet|opus|fable`, `/effort`, `/model opusplan`. Run `claude update` first for the Fable 5.2 alias.

For everyday features they're close enough that I pick whichever my plan's usage favours:

- Same **$2** input; Grok output is**$6** vs Sonnet**$10** .
- CursorBench at High: Sonnet **47.8%** , Grok**43.9%** . At Medium, Grok leads**41.6% vs 39.2%** .
- Not close on Terminal-Bench 4.0 (Anthropic **70.6%** for Sonnet vs Cursor**37.6%** for Grok) or context (1M vs 256K).

Neither one is a Fable problem.

Checked against my Pro account and Anthropic's help center (launch offers, Fable on your plan).

| Plan | Fable normally | Launch offer | 
|---|---|---|
| Pro / Team standard | Usage credits | Included until **16 Nov** up to 50% of weekly limit + one Fable reset | 
| Max / Ultra / Team premium | Up to 50% of weekly limits | One to three Fable resets (into 2027) | 
| Free | Not available | Not included | 

Fine print that matters:

- Pro offer ends **16 Nov, 11:59 pm PT** (Tue 17 Nov, 08:59 Budapest). Unused allowance doesn't carry over.
- A Fable reset gives back weekly usage spent on Fable **and** refills your whole**5-hour** window (including Sonnet/Opus). Claim in Settings → Usage; the button isn't in Claude Code yet.
- If you're on Pro and not using Fable, treat **Reset for free** as a**5-hour refill** — claim before it expires.

**Is Fable half price this week?** No. Permanent API cut is **20%**. The “50%” is the share of weekly limit Pro can spend on Fable until 16 Nov.

Chunky session maths: **400K fresh input + 100K output**, no cache:

| Model | Session cost | 
|---|---|
| Grok 4.7 (beyond Cursor pool) | **$1.40** | 
| Sonnet 5.5 | **$1.80** | 
| Opus 5.5 | **$3.60** | 
| Fable 5.2 | **$7.20** | 
| Fable 5.2 fast | **$14.40** | 

Per token, Fable is **4× Sonnet**. Per finished hard task, it can be cheaper than maxing Opus — pay for it when the task is hard, not by default.

**Worth it for vibe coding?** Usually no — Sonnet + Opus cover the loop. Use Fable for long multi-file migrations, big refactors, or agent runs where Opus at High still falls short.

**Available on Claude Pro?** Yes, normally via usage credits. Launch offer includes Fable up to 50% of weekly limit until 16 Nov. Max/Ultra include Fable up to 50% as standard.

**Better than Opus on benches?** Anthropic says yes on most of its table; still start everyday coding on Opus.

**How do I use it in Claude Code?** `claude update` to v2.1.321+, then `/model fable` or `claude --model fable`. `/effort` defaults to Medium.

Sources: Anthropic Fable / Mythos 5.2. Related notes on Vibe Coder's Life.

## Top comments (0)
