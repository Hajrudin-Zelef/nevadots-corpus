---
id: collect-261121-veille-ia/claude-anthropic/claude-code-2-1-323-context-fix-for-third-party-models-2026
title: "Claude Code 2.1.323 Context Fix for Third-Party Models 2026"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-11-14"]
keywords: ["claude", "code", "model", "context", "session", "default", "third-party", "models"]
source: https://aiaffer.com/2026/11/claude-code-2-1-323-context-compression-fix-2026/
source_anchor: ""
source_lines: [1, 134]
section: "Claude_Anthropic"
---
A quiet default change in Claude Code 2.1.323 can turn a long coding session into a wall of failures. The change touches only one group of users: people who route Claude Code to a model through a third-party endpoint. Everyone on a standard Anthropic model can ignore this article entirely. Everyone else should read the two fixes below before the next long session, because the failure arrives silently and the session does not recover on its own.

**Verification note:** I checked the 2.1.323 changelog wording against the official release notes as mirrored on 2026-11-14, and I queried the npm registry the same day: the `latest` tag had already moved to 2.1.325, while `stable` still sat at 2.1.311. The change described here landed in 2.1.323 and stays in effect in later versions. The one claim I could not re-verify first-hand is the state of Anthropic’s model-configuration documentation: the source material I worked from reports that the docs still listed the old 200K default as of 2026-11-13 US Eastern time, contradicting the changelog. Treat that docs discrepancy as reported, not as independently confirmed. **Data cutoff: 2026-11-14.**

**Quick answers:**

**What broke in Claude Code 2.1.323?**
The assumed context window for a model ID that Claude Code does not recognize rose from 200,000 tokens to 1,000,000 tokens. Automatic compaction keys off that assumed window, so it now waits far too long on models whose real window is smaller.

**Who is affected?**
Anyone who points Claude Code at a third-party model through `ANTHROPIC_BASE_URL`, a gateway, or a custom model name that is not on the built-in list. That includes unmapped gateway aliases.

**What is the fastest fix?**
Set `CLAUDE_CODE_DISABLE_1M_CONTEXT=1` before the session fills up. That restores the old 200K assumption.

**Does `/autocompact` work instead?**
Yes. Running `/autocompact 200k` inside a session lowers the compaction point to 200,000 tokens. The documented range is 100K to 1M.

## What Changed in Claude Code 2.1.323

Claude Code 2.1.323 shipped on 2026-11-12 with 294 changes, headlined by an experimental feature called Code Mode, in which Claude writes JavaScript to call its tools. Buried in the same release, under “Changed”, sits the line that matters here: the context window assumed for a model ID that Claude Code does not recognize changed from 200K to 1M. The release notes add the remedy in the same breath: use `/autocompact` to compact earlier, or set `CLAUDE_CODE_DISABLE_1M_CONTEXT=1` to keep 200K.

The change makes sense from Anthropic’s side. The current Claude models — Sonnet 5 and later, Haiku 5.5, Opus 4.7 and later, and the Fable line — all run with a 1M-token window by default, and Claude Code compacts those sessions at about 967,000 tokens. A default assumption of 1M matches the models most people actually run. The problem is the word “unrecognized”. Claude Code keeps a built-in list of model IDs it knows. Anything outside that list — a third-party model, a gateway alias, a custom name — now gets the 1M assumption whether the real model supports it or not.

Note the release channel split, because it decides who already has the change. On 2026-11-12 the npm `latest` tag moved to 2.1.323 while `stable` stayed on 2.1.311. When I checked the registry on 2026-11-14, `latest` had already advanced to 2.1.325 and `stable` was still 2.1.311. Anyone who installs with the default `npm install -g @anthropic-ai/claude-code` gets the new default; anyone pinned to the stable channel does not — yet. If the goal is a calm rollout across a team, pinning to stable buys time, but the fix below is cheaper than a version freeze.

For background on what Claude Code is and how it works as a terminal agent, see my Claude Code CLI review.

## Why the Session Fails Instead of Compacting

Automatic compaction is the safety valve that keeps a long session alive. As the conversation approaches the context limit, Claude Code summarizes the history into a compact form and keeps going. The trigger point is computed from the assumed window, not the real one. With the old 200K assumption, a session on a 200K third-party model compacted at the right moment. With the new 1M assumption, Claude Code believes it has roughly five times more room than the model actually offers, so it keeps stuffing messages, tool results, and file reads into a window that is already full.

The failure mode is what makes this change nasty. The model provider rejects the oversized request, and the next turn fails too, because nothing has shrunk the context in the meantime. The release notes describe it plainly: once full, sessions on other vendors’ models with a smaller window can fail every turn until `/clear`. The session is not corrupted in any permanent sense — `/clear` starts a fresh conversation and everything works again — but all of the context that made the session useful is gone. A debugging session that has accumulated an hour of findings, file reads, and failed attempts evaporates in one command.

There is a second, subtler cost. Compaction is not free of quality loss: a summary written at the very edge of a full window is written by a model that is already struggling with degraded context. Anthropic’s own session-management guidance describes context rot — model performance degrading as context grows — and the worst moment to summarize is the moment the window is fullest. The env-var fix does more than prevent failures; it brings compaction back to a point where the summary still has room to be good.

## Who This Hits — and Who It Does Not

The affected group is narrow but real. It includes anyone who sets `ANTHROPIC_BASE_URL` to point Claude Code at another provider, anyone who runs Claude Code through a gateway that presents its own model names, and anyone whose organization maps Claude model names to internal aliases without registering those aliases with Claude Code. If the model name in the session is not on the built-in list, the 1M assumption applies.

The unaffected group is much larger. Anyone running a standard Anthropic model — Sonnet, Opus, or Haiku through the normal API or subscription — sees no change at all, because those model IDs are recognized and their real windows really are 1M (or 200K for older generations without the extended-context variant, where the correct assumption already applies). The same holds for the recognized models on Amazon Bedrock, Google Cloud, and Microsoft Foundry.

One edge case deserves attention: a model whose real window is smaller than 200K. The env-var fix restores the 200K assumption, which is still too large for such a model, so the session can still overfill before compaction fires. For that setup, only the second fix — an explicit `/autocompact` value matched to the real window — offers real protection. Check the provider’s documented window for the exact model in use before choosing between the two fixes.

Cost is another reason to care about the compaction point. Long sessions that run past 100K tokens can cross into higher pricing tiers on some models — Haiku 5.5, for example, charges a higher rate for prompts over 100,000 tokens. A compaction window set deliberately low keeps a session in the cheaper tier. My guide to cutting Claude Code mobile debugging costs covers the same cost logic from a different angle.

## Fix 1: Set CLAUDE_CODE_DISABLE_1M_CONTEXT=1

This is the fix the release notes recommend first, and it is the one to apply before upgrading a fleet of machines. The variable tells Claude Code to keep the old behavior: assume 200K for unrecognized model IDs. Set it before the session fills up — once the context is already over the real limit, the variable cannot shrink what is already there.

For a single session, prefix the launch command:

```
CLAUDE_CODE_DISABLE_1M_CONTEXT=1 claude
```
For a permanent fix on a machine that always routes through a third-party endpoint, add the export to the shell profile:

```
# ~/.bashrc or ~/.zshrc — restore the 200K assumption for unrecognized models
export CLAUDE_CODE_DISABLE_1M_CONTEXT=1
```
After editing the profile, reload it or open a fresh terminal before starting Claude Code:

```
source ~/.bashrc   # or: source ~/.zshrc
```
A quick sanity check confirms the setting is visible in the environment:

```
echo $CLAUDE_CODE_DISABLE_1M_CONTEXT
# expected output: 1
```
The variable is a blunt instrument by design. It applies to every unrecognized model on the machine, which is exactly right when every session on that machine goes through the same gateway. If the machine mixes third-party sessions with sessions on native 1M models, the variable still only changes the assumption for unrecognized IDs — recognized native models keep their real 1M window. That makes the env var safe to set globally on a gateway machine.

One limitation: the variable does nothing for a model whose real window sits below 200K. It restores the old assumption, not a correct one. If the gateway serves a 128K model, the session still overfills. That case needs Fix 2.

## Fix 2: Lower the Compaction Point with /autocompact

The second fix is finer-grained. Inside a Claude Code session, the `/autocompact` command sets the auto-compact window — how full the context gets before the automatic summary runs. The value saves to the user settings under the current model, so it persists across sessions for that model, and it applies immediately to the running session.

```
/autocompact 200k
```
The command accepts a plain token count (`200000`), a suffixed value (`200k`, `1M`), or a bare number from 100 to 1000 meaning thousands (`200` also sets 200,000). The accepted range is 100K to 1M tokens. Running `/autocompact` with no argument opens a dialog showing the current value, and `/autocompact auto` returns to the window Claude Code tunes for the model. The command needs version 2.1.221 or later, so any 2.1.323 install supports it.

Two more ways to set the same value exist for scripted setups. The `--autocompact` launch flag sets it for one session without touching saved settings:

```
claude --autocompact 200k
```
And the `CLAUDE_CODE_AUTO_COMPACT_WINDOW` environment variable overrides everything, but it takes a plain integer only — `200000`, not `200k`. A suffixed value there reads as a bare number and clamps to the 100K minimum, so a careless `export CLAUDE_CODE_AUTO_COMPACT_WINDOW=200k` would silently compact at 100K instead of 200K.

```
# Plain integer only — no "k" suffix in this variable
export CLAUDE_CODE_AUTO_COMPACT_WINDOW=200000
```
Which fix to choose? The env var from Fix 1 is the right default for a gateway machine: one line, no per-model fiddling, and it restores the behavior every third-party session had before 2.1.323. The `/autocompact` route is the right choice when the real window is known and differs from 200K — set the value a little below the real window to leave headroom. A smaller context window also means smaller prompts per turn, which is the same lever my AGENTS.md token-saving test pulls from a different direction.

## Check the Installed Version Before Anything Else

Both fixes assume the installed version actually carries the change. A machine on the stable channel may still run 2.1.311, where the old 200K default is already in force and neither fix is needed. Check first, then act:

```
claude --version
npm list -g @anthropic-ai/claude-code
```
The first command prints the CLI version; the second shows the installed npm package and its location. If the output shows 2.1.323 or later, the 1M assumption is active for unrecognized models. If it shows 2.1.311 or earlier, the machine is still on the old default, and the fixes above are worth applying now as preparation rather than as repair — the stable channel will catch up eventually.

Version numbers move fast in this project: between the source material’s check on 2026-11-12 and my registry query on 2026-11-14, `latest` advanced from 2.1.323 to 2.1.325. Treat any version-specific claim in this article as true for 2.1.323 and later unless a newer changelog says otherwise.

## Model Window vs. Effective Behavior

The table below summarizes how the assumed window, the real window, and the two fixes interact. “Assumed window” is the number Claude Code uses to decide when to compact; “real window” is what the provider actually accepts.

| Setup | Real window | Assumed window (2.1.323+) | What happens | 
|---|---|---|---|
| Native 1M model (Sonnet 5, Opus 4.7+, Fable 5.2) | 1M | 1M | Auto-compact near ~967K; sessions keep running | 
| Recognized 200K model | 200K | 200K | Auto-compact at the 200K boundary | 
| Unrecognized third-party model, 200K real window | 200K | 1M | No compaction until 1M; every turn fails once the real window fills | 
| Same setup, with `CLAUDE_CODE_DISABLE_1M_CONTEXT=1` | 200K | 200K | Old behavior restored | 
| Same setup, with `/autocompact 200k` | 200K | 1M assumed, compaction at 200K | Compaction fires at 200K | 
| Unrecognized model with a real window under 200K | Below 200K | 1M (or 200K with the env var) | Still overfills; only a matched `/autocompact` value protects it | 

The ~967K compaction point for native 1M models comes from Anthropic’s documentation as reported by multiple independent write-ups checked on 2026-11-14. The failure behavior in the third row is the release notes’ own wording, confirmed against the 2.1.323 changelog text.

## What I Could Not Verify

Honesty about the gaps matters more than a tidy ending, so here they are. I did not run the failing scenario myself: no live session was driven into a full third-party window to watch the per-turn failures happen. The failure description comes from the release notes, not from my own terminal. I did not re-open Anthropic’s model-configuration documentation page, so the claim that it still listed the old 200K default as of 2026-11-13 US Eastern time rests on the source material alone — the docs and the changelog disagreed at that moment, and I cannot say which one has been corrected since. I also did not test every gateway alias pattern; the “unrecognized” category covers any model ID outside the built-in list, and a gateway that maps its aliases onto recognized Claude model IDs may behave differently from one that passes custom names through.

What I did verify: the version exists (npm registry, 2026-11-14: `latest` 2.1.325, `stable` 2.1.311), the changelog text about the 200K-to-1M change and both remedies matches the official release notes as mirrored in multiple independent sources, and the `/autocompact` syntax and 100K–1M range match Anthropic’s documentation as quoted by those same sources. The environment variable name and the launch flag are confirmed the same way. If any of those details change in a later release, the version check in the previous section is the fastest way to find out.

The practical takeaway is short. If Claude Code talks to a third-party model, set `CLAUDE_CODE_DISABLE_1M_CONTEXT=1` before the next long session, or set `/autocompact` to a value just under the model’s real window. Either fix takes less than a minute. Recovering a session that has already filled up takes a `/clear` and everything in it.

*Full disclosure: This article contains no affiliate links. I earn nothing if you install Claude Code or change any setting described here. The version and changelog details were checked against the npm registry and the official release notes on 2026-11-14.*
