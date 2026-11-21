---
id: collect-261121-veille-ia/claude-anthropic/opus-5-5-in-claude-code-limits-effort-cost
title: "Opus 5.5 in Claude Code: Limits, Effort, Cost"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-09-22"]
keywords: ["opus", "claude", "default", "effort", "model", "code", "cache", "limits"]
source: https://www.buildthisnow.com/blog/guide/mechanics/claude-opus-5-5-in-claude-code
source_anchor: ""
source_lines: [1, 160]
section: "Claude_Anthropic"
---
# Opus 5.5 in Claude Code: Limits, Effort, Cost

Claude Opus 5.5 is now the default Claude Code model. Here is how the 1M context, $4/$20 pricing, higher 5-hour usage limits, and the new medium effort default actually work, and what to re-check after updating.

**Opus 5.5 is now the model you get when you open Claude Code and change nothing.** Claude Code 2.1.280 (September 22, 2026) made it the default Opus, with a 1M-token context window and API pricing of $4 in and $20 out per million tokens. It also runs at `medium` effort by default, not `high`, and your old saved effort level no longer carries over to it. That last detail is the one most people miss.

This post covers what changed in 2.1.280, how the price and limit changes actually stack, how effort works now that it is stored per model, and the failure modes worth checking after you update.

Release 2.1.280 shipped alongside the model launch. The parts that touch Opus 5.5:

| Change | What it means for you | 
|---|---|
| Opus 5.5 ( `claude-opus-5-5` ) becomes the default Opus | The `opus` alias and the`default` setting now resolve to Opus 5.5 on the Anthropic API | 
| Pro and Team Standard start on Opus instead of Sonnet | Lower-tier plans now open sessions on the bigger model, which draws down limits differently | 
| 1M context on Opus 5.5 | No `[1m]` suffix and no usage credits needed on the Anthropic API | 
| Old saved effort no longer applies to new models | Opus 5.5 starts at its own default ( `medium` ) until you pick a level | 
| Opus 5.5 requires 2.1.280+ | Older builds list it as disabled | 

Per the Claude Code model configuration docs, the `default` model setting now resolves to Opus 5.5 for Pro, Max, Team, Enterprise, and the Anthropic API, and also on Claude Platform on AWS, Amazon Bedrock, and Google Cloud's Agent Platform. Microsoft Foundry is the exception and still defaults to Sonnet 4.5.

One thing did not change: `/model` still switches the session to another model and your choice persists as before. If you had explicitly pinned an older model, you stay on it. Nothing moves you to Opus 5.5 on its own.

The `best` alias is also worth knowing. It picks Fable where your account has it and falls back to `opus` otherwise. So on an account without Fable, `best` is now Opus 5.5 too.

Here are the verified API numbers from Anthropic's Opus 5.5 docs:

| Per million tokens | Opus 5 | Opus 5.5 | Change | 
|---|---|---|---|
| Input | $5 | $4 | 20% lower | 
| Output | $25 | $20 | 20% lower | 
| Cache read | $0.50 | $0.20 | 60% lower | 
| 5-minute cache write | $6.25 | $5 | 20% lower | 

On Opus 5.5, 1-hour cache writes are $8, batch processing is half price ($2 in, $10 out), and fast mode on the Claude API is $8 in and $40 out.

Anthropic's headline claim is that Opus 5.5 costs "40% less than Opus 5 on typical workloads" at default settings. That number is bigger than the 20% list-price cut, and the gap is the interesting part. It comes from three places:

1. **Lower per-token price.** The straight 20% on input and output.
2. **Much cheaper cache reads.** Agentic coding is dominated by re-reading the same context. Every turn in a Claude Code session resends the system prompt, your CLAUDE.md, tool definitions, and the conversation so far, and most of that is served from cache. Cache reads falling 60% hits the biggest line on a coding bill hardest.
3. **A lower default effort.** "Default settings" means`medium` . Opus 5 ran at`high` by default. Less thinking per turn means fewer output tokens, and output is the expensive side.

To see why the cache line matters, take a simple illustrative session: 10M cache-read tokens, 500K fresh input, 200K output.

|  | Opus 5 | Opus 5.5 | 
|---|---|---|
| 10M cache reads | $5.00 | $2.00 | 
| 500K input | $2.50 | $2.00 | 
| 200K output | $5.00 | $4.00 | 
| **Total** | **$12.50** | **$8.00** | 

That is 36% cheaper before effort even enters the picture. The mix in your real sessions will differ, so treat this as arithmetic, not a benchmark. The point is that long Claude Code sessions are cache-heavy, and cache is where Opus 5.5 got cheapest.

The catch: the 40% figure assumes `medium`. Anthropic's docs also say Opus 5.5 "tends to think more per turn than Claude Opus 5" at the same effort setting, "most of all at `xhigh` and `max`." If you push Opus 5.5 to `xhigh` for everything, you spend some or all of that saving back on thinking tokens.

On subscriptions you do not pay per token. You spend a five-hour allowance, plus a weekly cap. Two things moved on September 22.

**The five-hour limit went up.** Anthropic announced it is "increasing five-hour usage limits on Pro, Max, and Team plans," and the launch post extends that to seat-based Enterprise. Anthropic's own posts did not give a percentage. Press coverage (for example MakeUseOf) reports it as a 20% increase. Weekly limits were not part of the announcement.

**Each subscriber got a banked reset.** You receive one rate limit reset that you can save and trigger whenever you choose. Notebookcheck reports that the app shows an expiry of October 22, which the announcement does not mention. Save it for the one long session where hitting the wall would actually hurt, like a big refactor or a migration you do not want to split across two days.

So how much further does a window go? Anthropic does not publish how many tokens a Pro or Max window holds, so any exact number you see is a guess. What you can reason about is direction:

- **The bucket is bigger** (about 20% per the reported figure).
- **Each task costs less** if usage tracks compute. Anthropic says Opus 5.5 "requires less compute to serve than Opus 5, and its pricing reflects that." Cheaper tasks mean more tasks per window.
- **Pro and Team Standard now start on Opus.** If you were on Pro and happy on Sonnet, your default session now runs a bigger model. A bigger model can draw the window down faster per task, so check whether the bigger bucket covers the heavier default for your workload.
- **Effort decides a lot of it.** A session at`medium` and a session at`xhigh` can burn the same window at very different speeds.

The honest summary: if you were on Max using Opus 5 at `high`, you should get noticeably more work out of a window at Opus 5.5 `medium`. If you were on Pro using Sonnet, you now get a smarter default, and whether it lasts as long depends on the tasks you run.

For the general mechanics of the five-hour and weekly caps, see Claude Code usage limits in 2026.

This is the change that surprises people most, because it is silent.

Effort in Claude Code is now stored **per model**. The 2.1.280 changelog says an effort level saved before `/effort` became per-model "no longer applies to newly released models such as Opus 5.5." New models "start at their own default until a level is picked."

Here is the mechanism in order:

1. You had set `/effort high` months ago. That was saved as a global preference.
2. You update to 2.1.280. The default model becomes Opus 5.5.
3. Claude Code looks for a saved effort for `claude-opus-5-5` . There is none, because the old global value is not applied to new models.
4. Opus 5.5 runs at its own default: `medium` .

So you may have been on `high` for months and now be on `medium` without seeing any message. For many tasks that is fine. Anthropic says `medium` on Opus 5.5 "matches or exceeds" Opus 5 at `high` on its coding evaluations. But if you have a workflow tuned around a specific level, check it.

Supported levels on Opus 5.5 are `low`, `medium`, `high`, `xhigh`, and `max`. The ways to set them, from the docs:

To pin a level for Opus 5.5 specifically, put it under `modelSettings` in your settings file:

The top-level `effortLevel` and the per-model entry are separate. The per-model entry is what reliably applies to Opus 5.5.

Two precedence details matter:

- **The environment variable wins.**`CLAUDE_CODE_EFFORT_LEVEL` overrides saved settings. If you exported it in your shell profile long ago, Opus 5.5 is not at`medium` . It is at whatever that variable says, and`/effort` changes will not seem to stick.
- **`ultracode` is not an effort level.** The`/effort` menu offers it, but it is a Claude Code setting that sends`xhigh` to the model and also has Claude orchestrate dynamic workflows for substantive tasks. That burns more usage than plain`xhigh` .

A reasonable default split: `medium` for everyday edits and questions, `high` for multi-file changes and debugging, `xhigh` or `max` only for the hard problems where one more pass saves you an hour.

Opus 5.5 runs with a 1M-token window. On the Anthropic API, the docs say Opus 4.7 and later run with the 1M window on every plan, including Pro, with no `[1m]` suffix and no usage credits. So you do not need to pick a special variant to get it.

Two practical notes:

- **Auto-compact moves later.** The docs list the default auto-compact threshold at roughly 967K tokens on a 1M window. Sessions run much longer before Claude Code summarizes history.
- **Big context is not free context.** Every turn resends the whole conversation. Cheaper cache reads soften this a lot, but a 700K-token session still costs more per turn than a 70K one, on the API and against your plan limits. Clearing or compacting between unrelated tasks is still the biggest single lever.

If you want the 200K behavior back, `CLAUDE_CODE_DISABLE_1M_CONTEXT=1` caps native 1M models at 200K. For how the long window behaves over a real session, see 1M context in practice.

Put together, the defaults now look like this for most accounts:

| Setting | Before 2.1.280 (typical) | After 2.1.280 | 
|---|---|---|
| Default model on Max | Opus 5 | Opus 5.5 | 
| Default model on Pro / Team Standard | Sonnet | Opus 5.5 | 
| Default effort on Opus | `high` | `medium` | 
| Your saved global effort | Applied | Not applied to Opus 5.5 | 
| Context | 1M | 1M | 

What to do after updating:

1. **Run `claude --version`** and confirm 2.1.280 or newer.
2. **Run `/model`** and check what you are actually on. If you had pinned Opus 5 or Sonnet, you are still there.
3. **Run `/effort`** and set the level you want for Opus 5.5. Or add it under`modelSettings` .
4. **Check your shell** for`CLAUDE_CODE_EFFORT_LEVEL` and`ANTHROPIC_DEFAULT_OPUS_MODEL` . Either one can override what you think you configured.
5. **Spend your banked reset deliberately** before it expires.

Anthropic also says Opus 5.5 generates output more than 30% faster than Opus 5. In an interactive session that shortens the wait between turns at the same effort level. If speed was your reason for dropping effort on Opus 5, try going back up a notch. If you want even more speed on the API, fast mode is available for Opus 5.5 on the Claude API only, at double the price.

**"Opus 5.5 (disabled)" in the model picker.** Your Claude Code build is older than 2.1.280. Some package managers lag behind the upstream release and report you as up to date when you are not. Check `claude --version`, and if your channel is behind, update through one that has the current release.

**Effort changes do not stick.** Almost always `CLAUDE_CODE_EFFORT_LEVEL` in your environment. It overrides saved settings. Unset it or change it.

**Quality feels lower than last week.** You are probably on `medium` where you used to be on `high`. Not a regression in the model. Set `/effort high` for the session and compare.

**Usage burns faster on Pro than it used to.** Your default moved from Sonnet to Opus. If Sonnet was enough for your work, switch back with `/model sonnet`. Your choice persists.

**Switching models mid-session loses reasoning.** Anthropic's docs explain that thinking blocks are tied to the model that produced them. Opus 5.5 can read thinking from Opus 5 and earlier Opus, Sonnet, and Haiku. But when a conversation moves from Opus 5.5 to any model other than Fable 5.1 or Mythos 5.1, the turns after the switch run without Opus 5.5's earlier reasoning. The request still succeeds, and the dropped blocks are not billed. You just lose that context. This matters for `opusplan` (Opus plans, Sonnet executes) and for manual `/model` hops. Keep the model steady within a task where you can.

**Your own API scripts break.** If you call Opus 5.5 directly through the SDK (headless scripts, CI jobs, custom agents), four API changes return errors that Opus 5 accepted:

| What you send | Result on Opus 5.5 | Fix | 
|---|---|---|
| `thinking: { type: "disabled" }` | 400 | Omit `thinking` or use`adaptive` , and lower effort instead | 
| `thinking: { type: "enabled", budget_tokens: N }` | 400 | Same, effort controls depth now | 
| `tool_choice` of`any` or a named tool | 400 | Use `auto` with strict tool use, or say in the prompt when the tool applies | 
| `computer_20251124` on the Claude API or Google Cloud | 400 | Move to `computer_toolset_20260801` | 

There is also a silent one: short notes the model writes between tool calls now come back as `thinking` blocks instead of `text`. At the default display setting their text is empty, so a UI that streams progress updates goes quiet with no error. The migration guide covers setting `thinking.display`. Inside Claude Code you can run `/claude-api migrate` to have the bundled Claude API skill apply the model ID swap and parameter changes across a codebase and hand you a checklist.

**Editing history breaks replays on newer accounts.** For API accounts created on or after August 31, 2026, the API checks whether anything before an Opus 5.5 thinking block (system prompt, tools, earlier messages) changed since the block was produced. If it did, replaying the block returns a 400. Keep conversations append-only and change instructions with mid-conversation system messages instead of editing earlier turns.

Yes, from Claude Code 2.1.280. The `default` model setting resolves to Opus 5.5 on Pro, Max, Team, Enterprise, and the Anthropic API. Pro and Team Standard moved from Sonnet to Opus. On Microsoft Foundry the default is still Sonnet 4.5.

$4 per million input tokens and $20 per million output tokens on the API. Cache reads are $0.20, 5-minute cache writes $5, 1-hour cache writes $8. Batch is half price. Fast mode is $8 and $40 on the Claude API.

`medium` by default. Your old saved effort level does not apply to it, so set one with `/effort` or under `modelSettings` if you want something else.

Five-hour limits went up on Pro, Max, Team, and seat-based Enterprise plans, reported at about 20%. Subscribers also got one banked reset to use when they choose. Weekly limits were not part of the announcement.

You are on a Claude Code build older than 2.1.280. Update, then check with `claude --version`. For choosing between models beyond the default, see Claude Code model selection.

*Posted by @speedy_devv*
