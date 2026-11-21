---
id: collect-261121-veille-ia/claude-anthropic/auto-mode-becomes-the-default-in-claude-code-sparkone
title: "Auto mode becomes the default in Claude Code — SparkOne"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-08-14"]
keywords: ["claude", "mode", "percent", "auto", "default", "code", "classifier", "tool"]
source: https://sparkone.nl/en/blog/claude-code-auto-mode-default/
source_anchor: ""
source_lines: [1, 103]
section: "Claude_Anthropic"
---
Back in June I wrote here that I had dropped Plan mode and moved everything into auto mode. That was a choice at the time. I had switched it on, I knew what it did, and if it went wrong that was on me.

Since 14 August it is no longer a choice but a starting point. New sessions on Pro, Max and Team open in auto mode. That is a different conversation from my own preference, because a default also applies to everyone who never thought about it.

Anthropic shipped a blog post, an engineering write-up and a video alongside the change. The video is the clearest explanation I have seen of it, so here it is in full.

## What changes, and for whom

From 14 August 2026, new sessions on Pro, Max and Team run in auto mode by default. If you had already set a different default yourself, you get a one-time prompt asking whether you want to switch. If you pinned a default, nothing changes for you. Shift+Tab takes you back to manual approval.

The classifier costs a few extra tokens per tool call. That overhead is no longer billed on Pro, Max and Team, effective 7 August.

On Claude Enterprise, the Claude API, Claude Platform on AWS, Amazon Bedrock, Google Cloud’s Agent Platform and Microsoft Foundry, auto mode stays opt-in for now, giving admins time to review the change. Anthropic says it plans to make it the default there within a month. Enterprise admins who want it today can set it through managed settings.

## Why Anthropic is doing this

The whole argument rests on one number: 97 percent of permission prompts in Claude Code get approved. Often fast enough that it looks more like clicking through than reviewing.

What gives that number meaning is the contrast sitting next to it. When Claude presents a plan, users reject 39 percent of them. For individual permission requests, the rejection rate is 3 percent. So people do read, just not the same dialog a hundred times a day.

It shows up in what people configure, too. As of June 2026, 49.5 percent of active CLI users had manually created a Bash allow rule: 5 percent allow any shell command outright, another 43 percent have interpreter rules like `Bash(python:*)` that amount to the same thing in practice. That share grows by roughly 5 percentage points every 5 weeks. On top of that, 62 percent have used bypassPermissions or clicked “don’t ask again”, and 25 percent of interactive sessions start in bypass mode.

Put plainly, the safety measure was being switched off at scale because it got in the way. Auto mode is the attempt to put back something people will actually leave on.

## How the judgement works

The easy misreading is that Claude now approves its own actions. That is not what happens, and the distinction is the heart of the design. Claude reviewing Claude is writing, reviewing and merging your own pull request.

Instead, every action passes through a separate classifier. It sees your messages and Claude’s tool calls. What it does not see: Claude’s reasoning, Claude’s replies to you, or tool output. That is deliberate. If the classifier reads Claude’s own justification, Claude can talk it into a bad call. And since tool output is where hostile content enters, stripping it is also the primary prompt-injection defence at that layer.

The check runs in two stages. Stage one is a fast yes-or-no with no reasoning, tuned to err on the side of blocking. Only what it flags goes to stage two, which re-reads the same input with reasoning. On real traffic, stage one produces 8.5 percent false positives; the full pipeline brings that down to 0.4 percent.

Not everything reaches the classifier. Your own `deny`, `ask` and `allow` rules run first. Then a tier check sorts by risk:

| Tier | What goes through it | What happens | 
|---|---|---|
| 1 | Reads, search, code navigation, todos, plan transitions | Allowed straight away | 
| 2 | File writes and edits inside your project directory | Allowed straight away, since version control makes them reviewable | 
| 3 | Shell commands, web fetches, external tools, subagent spawns, filesystem work outside your project | Sent to the classifier | 

One exception deserves its own paragraph. The moment you enter auto mode, allow rules that amount to arbitrary code execution are set aside: blanket shell access, interpreter rules like `python:*` or `node:*`, package manager run commands. Otherwise the most dangerous command would be the one that skips the classifier. Your settings file is not modified, and the rules apply again as soon as you switch modes. Narrow rules, such as allowing a formatter, carry over untouched.

Approval is not the end of it either. A server-side probe scans every tool result for hidden instructions before it enters Claude’s context, and attaches a warning when something looks off. So an attack has to get past that probe and past a classifier that independently re-checks the next action against what you asked for.

When the classifier blocks something, Claude usually finds a safer route on its own, or asks you directly. After three blocks in a row, or twenty across a session, Claude Code falls back to manual approvals.

## The numbers, including the awkward one

The most quoted study: 1,053 paid professional testers in a purpose-built test environment. Partway through each session, one permission prompt was swapped for a clearly dangerous command. Testers caught it 13.6 percent of the time (143 of 1,053); auto mode caught 89 percent (937 of 1,053). Head to head, auto mode blocked 800 commands a human approved, while humans blocked 6 that auto mode let through.

The pattern inside that study tells me more than the headline. Early in a session, testers blocked roughly 17 percent. After fifty or more prior prompts, that had dropped to roughly 5 percent. The classifier’s block rate stayed flat no matter how long the session ran. Attention erodes. A model does not.

In real usage, across sessions flagged for review in May and June 2026: at production-level severity (7 or higher on their scale), 6.3 percent of manually approved sessions contained harm the user had not asked for, against 2.4 percent of auto mode sessions.

On prompt injection, they commissioned Trajectory Labs to run 72 scenarios ten times each. Of those 720 attempts, none succeeded against Fable 5, Opus 5 or Sonnet 5 in auto mode. Against GPT-5.6 Sol in Codex’s Auto-review mode, 5.83 percent succeeded.

Then the number that does not flatter anyone, which Anthropic publishes themselves. On a set of 52 real cases where the model overstepped, the full pipeline lets 17 percent through. Their own analysis of those misses is sharp: the classifier usually does see the danger. What it gets wrong is whether your consent actually covers the blast radius. “Clean up that PR” was said; force-pushing was not. An earlier approval of something similar is not a pattern. They add that prompt-engineering around it has not produced a fix worth its cost.

Anthropic also write the most honest sentence you can put in a post like this: if you are coming from `--dangerously-skip-permissions`, this is a substantial improvement. If you were genuinely reviewing every command by hand, you are giving something up.

## The caveat

Three things bother me, and none of them make auto mode a bad idea.

**The default is the real change, not the feature.** Auto mode has existed for months and you could turn it on. What happens now is that the judgement about “should this run” moves from you to a model you do not control, for everyone who does nothing. That is exactly the group least likely to configure it. The numbers say the shift is better on average, and I believe them. But better on average is not the same as better in your case.

**The trust boundary starts very narrow.** By default the classifier treats only your working directory and your git remotes as internal. Everything else is external, including your own organisation’s infrastructure. That is the safe direction to fail in, but it means an unconfigured auto mode either blocks too much or teaches you to wave blocks away. The second one is the same habit as approving 97 percent of prompts, moved up a layer.

**More is being read than before.** The probe that scans tool results runs server-side. Files you read and pages you fetch pass a check at Anthropic before the model sees them. That is precisely why the injection defence works, and it is a real widening of what leaves your machine. For a public repo it hardly matters. For work involving personal data it is the question you ask up front, not afterwards.

What I do myself: leave auto mode on, and fill in the `environment` description as soon as I am working in a repo that touches anything outside my own directories. For production infrastructure I sit and watch. That is also what Anthropic recommend, and I see no reason to be cleverer than the people who counted the misses.

## Frequently asked questions

### How do I turn auto mode off again?

Shift+Tab cycles modes during a session. To change it permanently, set `permissions.defaultMode` in `~/.claude/settings.json`. A pinned default is not overwritten by this change.

### Am I paying for the classifier?

Not on Pro, Max and Team, as of 7 August 2026. On the other platforms the overhead is still billed, with the stated plan to drop that when auto mode becomes the default there too.

### What if the classifier blocks something I want?

Claude usually finds another route, or asks you directly. If it keeps getting stuck, the session falls back to manual approvals after three blocks in a row or twenty in total. The structural fix is the `environment` description and `allow` exceptions, not switching the mode off.

### Does auto mode cover subagents?

Yes. The classifier runs on both ends of a handoff: when work is delegated out, and when results come back. On return it reviews the subagent’s full action history, and if that flags a concern it prepends a warning rather than discarding the results.

### Is auto mode safer than approving things yourself?

By every measure Anthropic publish, yes, compared with how people actually approve in practice. Compared with someone who genuinely reads every command, not necessarily. They say themselves it is not a replacement for your own review on high-stakes infrastructure.

## Sources

- Anthropic, Auto mode is now the default in Claude Code for Pro, Max, and Team plans, 7 August 2026
- Anthropic Engineering, How we built Claude Code auto mode: a safer way to skip permissions, 25 March 2026
- Anthropic, video How auto mode works with Claude Code
- Claude Code Docs, Choose a permission mode
- @ClaudeDevs on X, announcement of 10 August 2026

Figures taken from the Anthropic publications listed above and checked on 10 August 2026. Per the documentation the classifier now runs on Claude Sonnet 5 by default; the March engineering post still names Sonnet 4.6, so that changed in between. All percentages come from research Anthropic ran or commissioned; I have not been able to reproduce them independently.
