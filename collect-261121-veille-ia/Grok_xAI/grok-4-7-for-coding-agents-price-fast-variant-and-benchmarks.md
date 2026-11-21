---
id: collect-261121-veille-ia/grok-xai/grok-4-7-for-coding-agents-price-fast-variant-and-benchmarks
title: "Grok 4.7 for Coding Agents: Price, Fast Variant and Benchmarks"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: ["2026-11-03"]
keywords: ["grok", "model", "high", "card", "xhigh", "coding", "effort", "read"]
source: https://aq.dev/guides/grok-4-7-for-coding-agents/
source_anchor: ""
source_lines: [1, 96]
section: "Grok_xAI"
---
# Grok 4.7 for Coding Agents: Price, Fast Variant and How to Run It

Updated November 3, 2026 · first published September 22, 2026 · by the AQ team

Grok 4.7 is good at long, multi-step coding work and priced low, but it is not the strongest model on independent terminal benchmarks and it spends many tokens per task. It is xAI's current flagship (released September 21, 2026), takes text and image input, has a 500K context window, and costs $2 input and $6 output per million tokens below 200K prompt tokens ($4 and $12 above). It runs on the xAI API as `grok-4.7`, as the default model in Grok Build, and in Cursor. xAI's model card reports 38.0% on Terminal-Bench 4.0 at xhigh effort and 46.3% on CursorBench 4.0, while Artificial Analysis, as read on November 3, 2026, puts it below the median of comparable models on its Intelligence Index. Choose it for hours-long agent runs where list price matters, and measure cost per task on your own repository before making it a default. (xAI's launch post and API docs use the name SpaceXSI; the model card calls the developer SpaceXAI, a doing-business-as name of XAI LLC. The API, docs and model IDs live under x.ai.)

## What xAI says it is

Unlike Grok 4.6, Grok 4.7 is presented as a new base model. The model card (revision 2026-09-21) says it improves on long-horizon coding by considering edge cases, working persistently on hard problems and checking its own work more effectively, and that it received supplemental training on anonymized Cursor workflow data. The launch post is dated September 21, 2026. The API release notes list it under September, and the Grok Build changelog lists it as Grok Build 1.0.40 ("Grok 4.7 has arrived"); we could not read that page directly and give its version as shown in search results.

## Benchmarks: xAI's model card, per effort level

These are xAI's figures from the model card, revision 2026-09-21. The effort level is the one xAI states for each score; the Grok 4.6 column shows the effort level xAI used for that comparison.

| Benchmark | Grok 4.6 | Grok 4.7 | Effort (4.7) | 
|---|---|---|---|
| Terminal-Bench 4.0 (Grok Build harness) | 20.3% (high) | 38.0% | xhigh | 
| DeepSWE v1.1 | 65.2% (high) | 71.0% | high | 
| CursorBench 4.0 | 40.4% (high) | 46.3% / 43.9% | xhigh / high | 
| SWE-Marathon v1.1 | 31.9% (high) | 46.0% | high | 
| FrontierSWE V2 (mean@5, partial credit) | 25.3% (xhigh) | 29.0% | xhigh | 
| EEBench | 60.0% (xhigh), 53.0% (high) | 66.0% | xhigh | 

The card also lists peers on some of these. On Terminal-Bench 4.0 it shows Claude Fable 5.1 (max) at 57.9% and GPT-5.6 Sol (max) at 37.3%; on DeepSWE v1.1, GPT-5.6 Sol (max) at 72.7%; on SWE-Marathon v1.1, Claude Opus 5 (max) at 50.0%; on FrontierSWE V2, Claude Fable 5.1 (max) at 56.3%. The CursorBench 4.0 figure for Grok 4.6 comes from xAI's launch post, and the card notes that CursorBench 4.0 scores are not comparable with version 3.2. The card itself says absolute Terminal-Bench scores "remain sensitive to the agent harness".

## The independent read, November 3, 2026

Artificial Analysis's page for Grok 4.7 (xhigh), read on November 3, 2026, shows an Intelligence Index score of 35, rank 33 of 60 and under the median of 38 for comparable models. We previously cited a score of 46 from launch week; that figure is not on the page now, so treat the two as measured on different versions of the index. The same page lists 150M output tokens to run the index (median for comparable models 71M), a cost of $7.77 per index task, a speed of 80.5 output tokens per second (median about 89), a 500K context window and text and image input. On Artificial Analysis's Terminal-Bench 4.0 page, the leaders on the day we read it were Kimi K3.1 Pro (67.2%), Gemini 4 Carbon at high effort (65.2%) and Claude Sonnet 5.5 at max (63.6%); Grok 4.7 was not among the top three. We did not find a Grok 4.7 Fast entry or a Coding Agent Index row for Grok Build on the pages we could read.

### Why vendor and independent numbers differ

xAI runs Terminal-Bench 4.0 with its own Grok Build harness and picks the effort level it reports; independent evaluators use their own harness, tool configuration and token budgets. The Intelligence Index is a broad mix of tasks, so it can rank a model lower than coding-specific agent benchmarks do. Both can be correct. Our guide to benchmarking the model or the harness covers these gaps in more detail.

## Pricing and the long-context tier

From xAI's models page and API pricing page: `grok-4.7` costs $2.00 input, $0.50 cached input and $6.00 output per million tokens for prompts below 200K tokens, and $4.00, $1.00 and $12.00 at 200K or more. The models page adds that a request whose prompt reaches the threshold is billed at the higher rate for all tokens in the request, so one long prompt reprices the whole call. The release notes describe the same tier as "below 200k" and "above"; the models table says "≥ 200k". Many-hour agent sessions are the ones whose prompts grow past 200K.

On cost per task, the independent source still points the same way as before: Artificial Analysis shows Grok 4.7 using roughly twice the median output tokens to run its index, at a task cost of $7.77 that ranks 53rd of 58 models. A low per-token price does not settle cost per completed task, so the number to compute is spend per finished task on your own repository.

## Grok 4.7 Fast

xAI's launch post says it also serves "a fast variant with twice the output speed at twice the price". The API release notes and the Grok 4.7 developer guide describe Grok 4.7 Fast as the same model on faster infrastructure that costs 2x the standard token rates, or 1.5x for long-context requests. It is available only in Cursor and Grok Build and is not on the public xAI API; it is billed through your plan in those products, and the developer guide says it is not included in Grok Build's free tier. xAI's API pricing page lists it at $4.00 input, $1.00 cached input and $12.00 output per million tokens below 200K prompt tokens, and $6.00, $1.50 and $18.00 above. Cursor's models page lists Grok 4.7 with a Fast variant.

Speed matters most when a person is waiting on each step of the loop: interactive pairing, short edit-and-test cycles, quick fixes. For an overnight run nobody watches, the standard model at half the rate is the cheaper pick. xAI gives no tokens-per-second figure for Fast, so measure it on your own tasks.

## How to run it

**The xAI API.** Model ID `grok-4.7`. Reasoning effort is low, medium, high (the default) or xhigh. It accepts text and image input (images up to 20 MiB, jpg or png, with no stated limit on the number of images) and produces text output. Grok 4.7 Fast is not available here.

**Grok Build, xAI's terminal coding agent.** Grok 4.7 is its default model. xAI's launch post for Grok Build (May 25, 2026) describes plan mode with step-by-step approval, parallel subagents that can run in their own git worktrees, headless mode (`-p`), ACP support, and AGENTS.md, plugins, hooks, skills and MCP servers working out of the box. At launch it was available to SuperGrok and X Premium Plus subscribers; xAI's current developer docs also refer to a Grok Build free tier, so check x.ai/build for the current sign-in and plan terms. xAI's Grok Build page highlights a native subagent view, Plan Mode integration, mouse support and a fullscreen terminal UI with Grok 4.7.

**Cursor.** The model card says Grok 4.7 is available to all Cursor users on every plan tier, and Cursor's models page lists it, including Fast variants.

**GitHub Copilot.** GitHub's supported-models page lists Grok 4.7 as generally available. Check your plan and organization policy for whether it is enabled for you.

**Next to other agents in AQ.** See the last section.

## Grok Build versus "Grok Code" searches

People searching for "grok code" or "grok4code" are usually looking for xAI's coding agent. On the xAI pages we read, that product is called Grok Build: a CLI you install from x.ai, powered by Grok 4.7 by default, with its own `grok-build-0.1` model available on the API at $1.00 input and $2.00 output per million tokens (256K context). None of the xAI pages we could read use "Grok Code" as a product name.

## When to pick it, when not to

Pick Grok 4.7 for long agent sessions that plan, run and verify over hours, for work where the $2 and $6 list price or a flat plan keeps spend predictable, and for tasks that involve screenshots or other images. Use Fast when someone is waiting on each step.

Look elsewhere when you need the strongest terminal-driving model (the leaders on Artificial Analysis's Terminal-Bench 4.0 page score above 63%, against xAI's own 38.0%), when you pay per token and verbosity would erase the price advantage, or when latency matters and you cannot use Fast (80.5 tokens per second on Artificial Analysis's page).

## Test it against your incumbent, on your own repo

Benchmarks are somebody else's repository. Give Grok 4.7 and your current default the same real task from your backlog, in parallel isolated checkouts, and compare the sessions along with the diffs. Our guide to reviewing an AI coding session covers what to look for beyond the patch: how the agent explored, what it verified, where it guessed.

## Where AQ fits

AQ is the multiplayer coding harness where teams build and run bots with every AI coding agent, like Claude Code and Codex: shared live terminals, review and merge, app previews and standing jobs, in your own cloud. Grok runs in AQ as the real Grok CLI, signed in with your own xAI plan, next to Claude Code, Codex, Cursor Agent, Kimi and plain shells. Each work item gets its own git worktree on your team's machine or in your cloud, and teammates can watch the same live session. To compare Grok 4.7 with your current default, open two work items on the same repository with the same task and review both sessions. AQ never marks up usage on your own subscriptions. AQ's built-in agent can also run on AQ models at a per-token price shown before you pick one. If you use Grok Bot, connect it to AQ so it can start work and return a pull request for review.

## Frequently asked questions

### Is Grok 4.7 good for coding?

It is strong on long, multi-step agent work and cheap per token, with mixed independent results. xAI's model card reports 71.0% on DeepSWE v1.1 at high effort and 38.0% on Terminal-Bench 4.0 at xhigh, but Artificial Analysis (November 3, 2026) ranks it below the median of comparable models on its Intelligence Index and shows it using about twice the median output tokens. Test it on your own repository before making it a default.

### What does Grok 4.7 cost?

On the xAI API, $2 input, $0.50 cached input and $6 output per million tokens for prompts below 200K tokens, and $4, $1 and $12 at 200K or more, with the higher rate applying to every token in a request that reaches the threshold. Grok 4.7 Fast costs 2x those rates (1.5x for long-context requests) and is billed through your Cursor or Grok Build plan.

### What is Grok 4.7 Fast?

According to xAI's docs, it is the same model on faster infrastructure, with twice the output speed at twice the token price. It is available only in Cursor and Grok Build, not on the public xAI API, and is not included in Grok Build's free tier. It helps most in interactive loops where someone waits on each step.

### How big is Grok 4.7's context window and does it take images?

The context window is 500K tokens. Grok 4.7 accepts text and image input and produces text output; xAI's docs allow images up to 20 MiB in jpg or png format. Prompts of 200K tokens or more are billed at the higher long-context rate.

### How do I run Grok 4.7 in a coding agent?

Call `grok-4.7` on the xAI API, use Grok Build, where it is the default model, or pick it in Cursor. In AQ, the real Grok CLI runs in its own git worktree next to Claude Code and Codex, signed in with your own xAI plan, so your team can run the same task on two agents and compare the sessions.
