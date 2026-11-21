---
id: collect-261121-veille-ia/ia-agents/haiku-5-5-is-priced-to-be-your-subagent-model-here-s-how-to-wire-it-in
title: "Haiku 5.5 is priced to be your subagent model. Here's how to wire it in"
domain: ia-agents
role: reference
task: article
actors: []
dates: []
keywords: ["haiku", "model", "subagent", "claude", "code", "about", "sonnet", "agent"]
source: https://tokenise.rosvetic.com/posts/haiku-5-5-subagent-model/
source_anchor: ""
source_lines: [1, 58]
section: "IA_Agents"
---
Anthropic released Claude Haiku 5.5 on 7 October, and the pitch is the job you probably already have for a small model: the narrow, high-volume work your main agent delegates. Input costs $0.10 per million tokens and output $0.50, against $1 and $5 for Haiku 4.5. Claude Code 2.1.293 shipped the same day with it as the default Haiku, per the changelog .

Anthropic’s announcement is explicit about the role. It positions Haiku 5.5 as a subagent next to Opus 5.5 and Sonnet 5.5, good at things like compaction, summarisation and subagent work. It also says Sonnet 5.5 and Opus 5.5 remain the better choice for complex agentic coding. The numbers back that up: on Terminal-Bench 4.0, Anthropic reports 39.2% for Haiku 5.5 and 70.6% for Sonnet 5.5.

So this isn’t a model to put in the driver’s seat. It’s a model to put behind a delegation boundary.

## What the price really is

The headline drop is smaller once you account for the tokeniser. Haiku 5.5 uses the same tokeniser as Claude 4.7 and later, which Anthropic’s docs say produces about 30% more tokens for the same text than Haiku 4.5. Worked through, that makes effective input about $0.13 against $1, and output about $0.65 against $5. Still a large cut, just not a literal tenfold one. Anthropic puts the average at roughly 75% lower.

There’s also a cliff. Prompts over 100,000 tokens are billed at $0.50 input and $2.50 output, five times the base rate. Sonnet 5.5 charges the same $2 and $10 across its whole window, so a subagent that swallows a big context is still cheaper on Haiku, but not by the margin you budgeted. Keep delegated tasks small and you stay in the cheap tier.

## Wiring it into Claude Code

A subagent picks its model from the `model` field in its frontmatter, which accepts an alias (`haiku`) or a full ID such as `claude-haiku-5-5`, per the subagent docs
. A log-triage agent is a reasonable candidate:

```
---
name: log-triager
description: Reads test and CI logs and reports the first real failure
model: haiku
---
Read the log you're given. Report the first failing test or error,
the file and line if present, and the three lines of context around it.
Do not suggest fixes.
```
To move every subagent that doesn’t pick its own model, set `CLAUDE_CODE_SUBAGENT_MODEL` in settings:

```
{
  "env": {
    "CLAUDE_CODE_SUBAGENT_MODEL": "haiku"
  }
}
```
Two catches from the docs. Frontmatter and per-invocation models still win over that variable, so it’s a default, not an override. And the built-in Explore agent ignores it on its own; to move Explore, define your own subagent named `Explore` with `model: haiku`, or add `CLAUDE_CODE_SUBAGENT_MODEL_FORCE=1` (v2.1.257 or later). Run `/tasks` to confirm what a running subagent actually uses.

## Where it’ll bite you

Anthropic’s prompting guide is candid about two failure modes. At low and medium effort, Haiku 5.5 sometimes reports a code change as done without running a check. And in long agent prompts at low effort, it sometimes stops early and hands the task back. Medium is the default, and the guide says to start there for agentic coding.

If you let it edit code, put the verification requirement in the subagent’s prompt rather than hoping. Something like:

```
Before reporting a change as done, run the project's tests, type-checker
or build against it. If no check can run here, say which one you skipped
and why.
```
That is a shortened version of Anthropic’s own suggested paragraph. For read-only roles like triage and summaries, the problem mostly doesn’t arise, which is another reason to keep Haiku there.

## If you call the API directly

Moving from Haiku 4.5 isn’t a model-ID swap. According to the migration guide
, `budget_tokens` thinking, non-default `temperature`, `top_p` and `top_k`, and assistant-message prefill all return a 400. Safety classifiers can decline requests with `stop_reason: "refusal"`, there’s no server-side fallback, and Priority Tier isn’t supported. If you have your own harness, the Sonnet 5.5 breaking changes
overlap with several of these.

Recount your prompts against the new tokeniser before you trust any cost estimate. In Claude Code, `/claude-api migrate` can apply the mechanical parts.
