---
id: collect-261121-veille-ia/claude-anthropic/haiku-5-5-as-a-claude-code-subagent-explore-override-wins-parallel-research-does-not
title: "Haiku 5.5 as a Claude Code Subagent: Explore Override Wins, Parallel Research Does Not"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-10-08"]
keywords: ["subagent", "haiku", "claude", "code", "explore", "opus", "parallel", "list"]
source: https://www.aiviolabs.com/en/blog/haiku-5-5-as-a-subagent
source_anchor: ""
source_lines: [1, 83]
section: "Claude_Anthropic"
---
Anthropic shipped Haiku 5.5 with the usual pitch: the small model is now good enough for most of the delegated work inside an agent system, and it is fast. I run a fleet of Claude Code sessions, so I had a concrete question rather than a general one. Where in my stack does a faster small model actually shorten the time from "I asked" to "it's done"?
I do not care much about the per-call price of a subagent. I care about the wall-clock time of the whole job, with the quality held steady. Cost is the third number, not the first. So I ran two experiments in one afternoon: one where Haiku replaced the model behind Claude Code's built-in file-search subagent, and one where it replaced the web-research subagent inside a longer consulting pipeline. The first paid off immediately. The second did not, and the reason it did not is the more useful finding.
## The short version
- **As Claude Code's `Explore` subagent, Haiku 5.5 at medium effort was 33% faster than the Opus 5.5 default, at about 1/39 the cost, with no loss of recall on 36 read-only inventory tasks.** I switched my machine over the same day.
- **Raising Haiku's effort to high or xhigh made it slower and no more correct.** On rule-driven search tasks, extra thinking is extra writing.
- **Inside a longer pipeline, swapping one Sonnet research subagent for eight parallel Haiku subagents did not shorten the job at all.** Eight searchers took slightly longer in wall time than one. The planner that has to read everything they produce got slower, and the whole thing came out no quicker and 51% more expensive.
- **The lever for total time is the integrator's input, not the subagent's speed.** A fast subagent that returns four times as much text just moves the wait downstream.
## Experiment A: Haiku as the file-search subagent
Claude Code has a built-in subagent called `Explore`. The main session hands it a question like "which scripts in this repo reference the sync tool?", it sweeps the tree, and returns a list. In my setup the main session runs Fable 5.1, and Anthropic's docs say the built-in `Explore` then runs on the `opus` alias. I checked rather than trusted: the subagent transcript showed `claude-opus-5-5`. So the baseline here is Opus 5.5 at medium effort, which is what my user-level settings pin for Opus.
The docs also say a user-level or project-level subagent with the same name overrides the built-in one, including its model. That is the whole mechanism I was testing.
### Setup
I wrote a small driver that launches read-only `claude -p` sessions against one of my repos. Each session can use Read, Glob, Grep and a short list of read-only shell commands; Write, Edit, nested agents and the web are blocked. Three questions, all of the "inventory" shape that `Explore` exists for:
1. List every script in `bin/`, one line each with its purpose, then find every place those scripts are referenced from documentation or other scripts, with `file:line`.
2. Find every mention of a particular sync script across the repo, including inside skill folders, as a `file:line` table. Completeness required.
3. List every open ticket in a backlog folder with filer and subject, then flag which ones relate to a specific mechanism and say why.
Four configurations, three questions, three runs per question: 36 sessions in all. The four configurations ran in parallel with each other; runs within a configuration ran one after another. Every number below comes straight from Claude Code's `--output-format json` output. Nothing is estimated from list prices.
### Results
| Configuration | Q1 (s) | Q2 (s) | Q3 (s) | Mean wall (s) | Mean cost (USD) | Mean output tokens |
|---|---|---|---|---|---|---|
| Opus 5.5, medium (the default) | 23.9 | 43.6 | 42.9 | **36.8** | 0.621 | 3.8K |
| Haiku 5.5, medium | 17.4 | 28.2 | 28.4 | **24.7** | 0.016 | 4.7K |
| Haiku 5.5, high | 24.6 | 69.6 | 39.9 | **44.7** | 0.028 | 9.8K |
| Haiku 5.5, xhigh | 48.3 | 92.8 | 68.1 | **69.7** | 0.026 | 15.6K |
Each cell is the mean of three runs. The spread is worth knowing: Haiku medium ranged from 16.7 s to 35.0 s across its nine runs, Opus medium from 19.4 s to 46.2 s. Haiku high has one outlier, a Q2 run that took 106 s over 42 turns and cost six times what its siblings did; without it, Haiku high's Q2 mean drops to about 51 s, which is still slower than Opus. The ranking does not change either way.
Quality was scored against a ground-truth list for each question, then spot-checked by hand. All 36 runs got full marks: both scripts found on Q1, all ten files found on Q2, all fourteen tickets found on Q3, no missed file and no invented path. The difference was density, not correctness. Opus gave 20 to 23 `file:line` citations on Q1 where Haiku medium gave 12 to 14. On Q2 Opus listed 32 to 37 rows and Haiku medium 16 to 24, because Haiku merged multiple lines from the same file into one row and separately flagged a near-miss filename that matched the search string. That is a reasonable way to compress, and it is what I would have asked for.
### What I take from it
**Haiku medium is the right `Explore` for this kind of work.** A third faster, 1/39 the cost, same recall. I did not find a case in these 36 runs where I would have wanted Opus back.
**Higher effort cost time and bought no correctness.** Medium to high was 1.8× slower, medium to xhigh 2.8× slower, with output tokens doubling and tripling along the way, and the answers were not better. The xhigh run of Q2 took 93 seconds, twice as long as Opus medium. When the task is "find all the files that match", there is nothing to think harder about. The extra effort shows up as a longer report.
**On Haiku, effort does not change the bill much.** The three Haiku tiers cost US$0.016, 0.028 and 0.026 per run on average. Output tokens are cheap enough that tripling them barely registers. So on Haiku, the effort setting trades time, not money, and you should set it as low as the task allows.
**The scope is narrow, deliberately.** These are inventory questions: find, list, cite. I did not test judgment-style exploration, the "does this design make sense" kind of question, and I am not extrapolating to it.
### Making the switch
The override is one file. I created a user-level subagent named `Explore` with `model: haiku`, `effort: medium`, the built-in description copied verbatim, the same read-only tool list, and a short body describing how a read-only exploration subagent should behave. Two things caught me:
- A same-name override replaces the built-in entirely, body included. If you leave the body empty, your `Explore` runs with no system prompt at all.
- A running session does not pick up a new subagent file. I created the override, dispatched `Explore` again in the same session, and it was still Opus. A fresh session showed `claude-haiku-5-5` in its model usage. Restart before you conclude the override did not work.
Rollback is deleting the file.
## Experiment B: Haiku as the research subagent in a longer pipeline
One of our departments runs a consulting pipeline. A Fable 5.1 session at high effort takes a question, dispatches a research subagent to go read the web, asks a notebook of the department's own prior material a few times, and writes an opinion. A full run is around nine minutes. The research stage is one Sonnet subagent that handles every sub-question in sequence.
The obvious move after Experiment A: give each sub-question its own Haiku subagent and run them in parallel. Eight sub-questions, eight Haiku searchers, all dispatched in one message. The main session stayed on Fable high. I re-ran the exact same question that had produced the baseline four days earlier.
| | Baseline: 1 Sonnet searcher | 8 Haiku searchers, parallel |
|---|---|---|
| Total job time | **520 s** | **558 s** |
| Searcher stage, wall | 112 s | 129 s (single searchers took 31 to 100 s) |
| Searcher web calls | 28 | 89 |
| Searcher output tokens, incl. thinking | 18K | **192K** |
| Main-session input tokens, incl. cache | 1.55M | 2.57M (+66%) |
| Main-session output tokens | 25K | 33K |
| Opinion length | 12.9K chars | 20.3K chars (+57%) |
| Notebook queries by the main session | 3 | 1 |
| Cost | US$7.57 | **US$11.45** |
The parallelism worked in the narrow sense: eight searchers whose API time summed to about 13 minutes finished in two minutes of wall time. But that stage still took 129 s against the baseline's 112 s, so relative to one Sonnet searcher it saved nothing, and it was only a fifth of the job anyway. Downstream, the Fable session read 192K tokens of searcher output and then wrote an opinion 57% longer than before.
![Two runs of the same job. With one Sonnet searcher the search stage took 112 s and the whole job 520 s. With eight parallel Haiku searchers the search stage took 129 s, the integrator read 192K tokens of searcher output instead of 18K, and the whole job took 558 s.](https://bpnycnjporbaoxnwntmi.supabase.co/storage/v1/object/public/blog-images/2026/10/haiku-5-5-as-a-subagent/fig-1-integrator-input.png)
### Capping the searchers
The natural fix is to make each searcher return less. So I ran it a third time with hard caps: at most four searchers, each limited to 8 facts, 12 web calls and 1,200 words.
| | Baseline | 8 Haiku, no caps | 4 Haiku, capped |
|---|---|---|---|
| Total job time | 520 s | 558 s | **766 s** |
| Searcher stage, wall | 112 s | 129 s | 102 s |
| Searcher web calls | 28 | 89 | 37 |
| Searcher output tokens | 18K | 192K | 70K |
| Main-session input tokens | 1.55M | 2.57M | 2.29M |
| Main-session output tokens | 25K | 33K | 37K |
| Opinion length | 12.9K | 20.3K | 17.1K |
| Cost | US$7.57 | US$11.45 | US$10.37 |
The searcher stage came in under the baseline, web calls fell from 89 to 37, and the subagents' share of the bill fell to twenty cents. And the total job time was the longest of the three.
### How to read that
Each of the three runs is a single sample. A Fable high session on this task swings between eight and thirteen minutes run to run, and that swing alone is bigger than the entire searcher stage. So the timing comparison is inconclusive, and I am not claiming that caps made it slower. What I am claiming rests on the token counts, which are stable: parallel Haiku searchers produced four to ten times the text, the main session's input grew by half to two-thirds, and its output grew too. Whatever the searchers saved, the integrator spent.
There is one behavior change I want to watch rather than conclude on. In both Haiku runs the main session queried the department's own notebook once instead of three times. Given a large pile of fresh external facts, it leaned on them and asked its own memory less. That might be fine or might be a quality regression; I have not read the three opinions side by side yet.
The decision was to put the pipeline back the way it was, one Sonnet searcher, and keep the two Haiku variants on disk for a later retry. If I want that job to finish sooner, the place to work is the Fable session's effort setting and the skeleton of the opinion it writes. The searcher layer is not the lever.
## Where Haiku belongs
Both experiments point at the same rule, from opposite directions.
**A subagent is fast when its output is small and its job is bounded.** `Explore` returns a list of paths and lines. Haiku produces that list a third faster than Opus and there is nothing for the caller to digest beyond the list itself. That is the shape where a small fast model wins outright.
**A subagent is not fast when it feeds a planner.** Research subagents produce prose, and prose is input for the next model in line. Eight of them in parallel finish together and then hand the integrator eight reports. The job's critical path moves from "wait for the searcher" to "wait for the planner to read the searchers", and the planner is your most expensive, slowest model by design.
So, three rules I am now using when I hand work to Haiku:
1. **Give it hard caps on what it returns**: number of facts, number of web calls, number of words. Caps on the searcher side are not enough on their own, as Experiment B showed, but without them the planner's input is unbounded.
2. **Limit how many run at once**, because their outputs add up in the integrator's context even if their wall times do not.
3. **Measure the whole job and the integrator's input tokens**, not the subagent's own seconds. A subagent benchmark that stops at the subagent is measuring the wrong thing.
And keep Haiku at medium effort. In every Explore configuration I tried, more effort meant more output, and more output is exactly what the next model downstream does not want.
## Getting started
- Check which model your built-in `Explore` actually runs. Dispatch it once and look at the model usage in the session output; do not assume.
- To override it, add a user-level subagent file named `Explore` with `model: haiku` and `effort: medium`. Copy the built-in description verbatim, keep the tools read-only, and write a body, because an empty body means no system prompt. Restart your session before you test.
- If you are parallelizing research subagents, log the integrator's input tokens before and after. If that number goes up by more than the subagent stage saved, you have moved the wait, not removed it.
- All figures above are from Claude Code's own `--output-format json`, on Claude Code 2.1.294, 2026-10-08.
The model that finishes its own part fastest is not the one that gets the job done soonest. In my case it took one afternoon and a 51% larger bill to see that.
