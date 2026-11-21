---
id: collect-261121-veille-ia/outils-agents/is-devin-s-swe-2-flash-worth-it-pricing-speed-and-the-benchmarks-explained
title: "Is Devin's SWE-2 Flash Worth It? Pricing, Speed, and the Benchmarks, Explained"
domain: outils-agents
role: reference
task: article
actors: []
dates: []
keywords: ["flash", "cognition", "swe-2", "post", "devin", "table", "models", "speed"]
source: https://www.pondero.ai/coding/guides/swe-2-flash-devin-pricing-worth-it/
source_anchor: ""
source_lines: [1, 120]
section: "Outils_Agents"
---
## Table of Contents

# Is Devin's SWE-2 Flash Worth It? Pricing, Speed, and the Benchmarks, Explained

If you already pay for Devin, yes: switch your routine tickets to SWE-2 Flash before November 30, 2026, when Cognition's free window closes, per Cognition's launch post. For everyone else, Flash is no reason to sign up. It has no public API and no public weights, and on Cognition's own benchmark table, Claude Haiku 5.5 scores higher on seven of the eight rows. Inside that one product it is a good cheap model; outside it, you can't buy it.

## What Cognition shipped on November 4

SWE-2 Flash has 276B total parameters, 12B of them active, and is built on Thinking Machines' Inkling-Small, per Cognition's November 4 announcement. What changed is the training recipe: Cognition added a mid-training stage on "a few trillion tokens" of scientific articles, blogs, code, and other sources before reinforcement learning, which it says it had not done before. On Terminal-Bench 4.0 that took the base model from 0.9 to 20.0, and on FrontierCode 2.0 from 3.2 to 24.0, per the same post.

Its larger sibling, SWE-2, is listed in that post at 2.8T total and 104B active parameters. Flash runs "at one-fifth the price and 3x the speed" of SWE-2 in Devin Desktop and Devin CLI, and the post's call-to-action reads "Free through 11/30." LetsDataScience and RuntimeWire both describe that free access as applying to paid Devin users.

Cognition names the jobs it built Flash for: "automations, code review, feature development, and updating documentation," per the launch post. Cognition's own benchmark table backs about half of that list.

## The benchmark table, re-sorted for a buyer

Cognition published scores for Flash against eight other models. Every number below is self-reported by Cognition in that post and has not been reproduced by a third party as of November 12, 2026. We cut the table to the five small models a buyer would actually weigh against each other, added SWE-2 as the ceiling, and ranked Flash within the small-model group.

| Benchmark | SWE-2 Flash | Claude Haiku 5.5 | GPT-6 Luna | DeepSeek V4.1 Flash | GLM 5.3 Flash | SWE-2 | Flash rank (5 small models) | Source | 
|---|---|---|---|---|---|---|---|---|
| FrontierCode 2.0 | 24.0 | 27.7 | 21.9 | 19.3 | 14.2 | 23.8 | 2nd | Cognition | 
| FrontierCode 1.1 Main | 45.5 | 46.4 | 42.4 | 43.9 | 31.8 | 50.0 | 2nd | Cognition | 
| SWE-Bench Pro v2 Hard | 89.9 | 90.2 | 80.4 | 86.3 | 88.2 | 90.6 | 2nd | Cognition | 
| Terminal-Bench 2.1 | 89.2 | 94.4 | 82.0 | 91.0 | 84.3 | 92.8 | 3rd | Cognition | 
| DeepSWE 1.1 | 60.1 | 59.3 | 66.6 | 77.0 | 63.4 | 73.0 | 4th | Cognition | 
| SWE-Atlas QA | 50.0 | 56.5 | 45.2 | 71.0 | 56.5 | 56.3 | 4th | Cognition | 
| ExploitBench | 34.8 | 38.0 | 44.7 | 31.1 | 36.0 | 36.1 | 4th | Cognition | 
| Terminal-Bench 4.0 | 20.0 | 39.2 | 16.4 | 42.3 | 41.8 | 27.3 | 4th | Cognition | 

Scores as published in Cognition's SWE-2 Flash post, dated November 4, 2026. Ranks are our sort of those numbers.

Flash never places first among the small models. Haiku 5.5 beats it everywhere except DeepSWE 1.1, where Flash wins by 0.8 points, per Cognition's table. DeepSeek V4.1 Flash leads three rows outright.

Cognition's headline claim is that Flash outperforms all American open-weight models. That covers less than it sounds. The headline bar charts in Cognition's post set Flash against Inkling-Small, Beam 1.0, and Muse Spark Open, and size Muse Spark Open at 3x Flash. GLM 5.3 Flash and DeepSeek V4.1 Flash come from Chinese labs; Haiku 5.5 and GPT-6 Luna are closed models from Anthropic and OpenAI. Even inside the American group, Muse Spark Open scores 54.0 on SWE-Atlas QA to Flash's 50.0, and the two sit 0.1 apart on DeepSWE 1.1, per the same post.

## Flash holds up on code edits and drops off on long terminal work

Read down the rank column and Flash splits into two groups. It is 2nd of five on FrontierCode 2.0, FrontierCode 1.1, and SWE-Bench Pro v2 Hard, and 4th on Terminal-Bench 4.0, DeepSWE 1.1, SWE-Atlas QA, and ExploitBench, per Cognition's scores. The widest gap is Terminal-Bench 4.0, where Flash's 20.0 is roughly half of what DeepSeek V4.1 Flash (42.3), GLM 5.3 Flash (41.8), and Haiku 5.5 (39.2) post.

A second way to read the table is to divide Flash's score by SWE-2's, since SWE-2 is what Flash replaces inside Devin. That ratio tells you how much of the big model's result you keep for a fifth of the price.

| Benchmark | Flash / SWE-2 score ratio | Route in Devin | Source | 
|---|---|---|---|
| FrontierCode 2.0 | 1.01 | Flash | Cognition | 
| SWE-Bench Pro v2 Hard | 0.99 | Flash | Cognition | 
| Terminal-Bench 2.1 | 0.96 | Flash | Cognition | 
| ExploitBench | 0.96 | Flash, with review | Cognition | 
| FrontierCode 1.1 Main | 0.91 | Flash | Cognition | 
| SWE-Atlas QA | 0.89 | Either | Cognition | 
| DeepSWE 1.1 | 0.82 | SWE-2 or a frontier model | Cognition | 
| Terminal-Bench 4.0 | 0.73 | SWE-2 or a frontier model | Cognition | 

Ratios are Pondero's arithmetic on Cognition's published scores; the routing column is our recommendation.

For a Devin workspace, the routing falls straight out of those ratios. Send PR review, small feature tickets, bug fixes with a clear repro, and doc updates to Flash; that matches Cognition's own list and the rows where Flash keeps 0.91 or more of SWE-2's score. Keep long, multi-step terminal sessions (environment setup, migrations that run for an hour, anything that resembles Terminal-Bench 4.0) on SWE-2 or a frontier model. And treat security work as reviewed output: Cognition trained Flash for it and reports ExploitBench rising from 11.3 to 34.8, per the launch post, but GPT-6 Luna still posts 44.7 on that row.

## What "3x the speed, one-fifth the price" actually measures

The speed figure comes from a replay demo. Cognition's post replays each model's own run on one Terminal-Bench 4.0 task "at a simulated 200 vs. 60 tokens per second," Flash against SWE-2, per Cognition. That is 3.3x on paper. Your wall-clock gain depends on how much of a session is model generation versus tool calls, test runs, and waiting on CI, none of which get faster.

The price figure has no dollar amount attached. Cognition's launch post gives no per-token rate for Flash or SWE-2, and the LLM Reference tracker shows "No tracked provider pricing is available yet" for SWE-2 and SWE-2 Flash as of November 12, 2026. What a fifth of the price buys you is more quota. Devin's paid plans come with a usage allowance that refreshes daily and weekly, and "cost per message varies based on the model used," per Devin's pricing page. Extra usage beyond the allowance "is consumed at API pricing." A cheaper model per message means the Pro allowance ($20/month) or Max allowance ($200/month) lasts longer before you hit overage, per the same page.

December is where the math gets uncertain. After November 30 Flash goes back to drawing on your quota, and until Cognition publishes a rate, "one-fifth" is the only number you have to plan with.

## How to run SWE-2 Flash before November 30

Flash lives in two places: Devin Desktop (the product Windsurf was renamed to, per the banner on Devin's pricing page) and Devin CLI, per Cognition. The CLI is the faster way to try it against a real repo. Install, per Devin's CLI quickstart:

```
# macOS / Linux / WSL
curl -fsSL https://cli.devin.ai/install.sh | bash
# or, on macOS with Homebrew
brew install --cask devin-cli
```
Then open a project and pick the model from the selector. Running `/model` with no argument opens the picker, per Devin's models docs:

```
cd ~/code/your-repo
devin
# inside the session:
/model
# choose SWE-2 Flash from the list, then hand it a routine ticket
```
To make Flash the default for every new session, set it in `~/.config/devin/config.json`. The models docs still show `swe-1-6-fast` as the example ID, so copy Flash's exact ID from the picker rather than guessing:

```
{
  "agent": {
    "model": "<SWE_2_FLASH_MODEL_ID>",
    "pinned_models": ["<SWE_2_FLASH_MODEL_ID>", "opus"]
  }
}
```
Pinning both lets you flip between Flash and a frontier model with `Alt+N` (`Opt+N` on macOS) when a session turns out harder than it looked, per the same docs. Skip the short name `swe`, which "always resolves to the latest version in that model family," so it may land on SWE-2 instead of Flash. Use the full ID.

## No API and no weights limit where Flash can go

You cannot call Flash from your own code, and you cannot self-host it. Cognition says it is "not sharing the weights publicly yet," citing a "recent surge in cyber attacks," and is "building a program to securely share the weights with trusted industry partners and research labs," with details promised "in a few weeks," per the launch post. LetsDataScience reports that "no source reports an API or a Hugging Face release," per its write-up.

Teams on regulated infrastructure feel this most. The on-prem hosting and fine-tuning access Cognition describes in its "Origins & Cyber" section belongs to that future partner program, and there is no sign-up for it yet.

## If you're not on Devin: the cheap-model options you can buy today

Each of these puts a lower-cost model on routine work without switching editors for a single model release.

**Claude Code.** The `haiku` alias resolves to Haiku 5.5 on the Anthropic API, and to Haiku 4.5 on Bedrock and some other providers, per Claude Code's model configuration docs. Haiku 5.5 is the model that out-scored Flash on seven of eight rows in Cognition's own table. Our Claude Code model and pricing guide covers how the aliases and Fable pricing interact.

**GitHub Copilot.** Haiku 5.5 is generally available on Copilot Pro, Pro+, Max, Business, and Enterprise, and GitHub describes it as built for "subagents, quick edits, and terminal tasks," per GitHub's October 7 changelog. Pro is $10/month with $15 in monthly credits, per GitHub's plans page. See our GitHub Copilot review or go straight to GitHub Copilot.

**Cursor.** Cursor's lower-cost option is its own Composer 2.5, whose fast variant lists at $3 per million input tokens and $15 per million output, per Cursor's model docs. Our Cursor review walks through which models the included pool covers. Try it at Cursor.

**Cline.** If you want DeepSeek V4.1 Flash, the model that led three rows in Cognition's table, Cline's v4.1.26 release added "DeepSeek V4.1 Flash 1017" to ClinePass and made it the new default for some other providers, per the v4.1.26 release notes. The extension itself is free for individual developers who bring their own API keys, per Cline's pricing page. Details in our Cline review, or get Cline.

## The pick

For a paid Devin user, move PR review, small tickets, bug fixes, and doc work to SWE-2 Flash now and log how far your quota stretches before November 30; that log is your pricing data when the free window ends. Keep Terminal-Bench-shaped work (long shell sessions, environment setup) on SWE-2 or a frontier model, where Flash keeps only 0.73 of SWE-2's score per Cognition's table.

For a solo developer not on Devin, take the cheap-model slot from the tool you already use: Haiku 5.5 through Claude Code or Copilot Pro, or DeepSeek V4.1 Flash through Cline. Cognition's own numbers favor those models over Flash.

For a team weighing self-hosted coding models, wait for Cognition's partner weight-sharing program. Until its terms are published, Flash runs only inside Devin's own apps.
