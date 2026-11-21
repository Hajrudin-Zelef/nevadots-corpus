---
id: collect-261121-veille-ia/qwen/qwen3-8-max-features-benchmarks-and-pricing
title: "Qwen3.8-Max: Features, Benchmarks, and Pricing"
domain: qwen
role: reference
task: article
actors: ["Alibaba"]
dates: []
keywords: ["qwen3", "coding", "benchmarks", "claude", "max-0902", "agent", "pricing", "qwen"]
source: https://www.datacamp.com/blog/qwen3-8-max
source_anchor: ""
source_lines: [1, 400]
section: "Qwen"
---
**Update: September 2, 2026:** Alibaba has released Qwen3.8-Max-0902, an upgraded snapshot of Qwen3.8-Max focused on coding and agent performance. The section below has been added to reflect the new benchmarks.

 
## What's New in Qwen3.8-Max-0902

On September 2, 2026, Alibaba released Qwen3.8-Max-0902, a post-training upgrade to Qwen3.8-Max. The architecture is unchanged: 2.4 trillion parameters, 1M token context, and the same pricing ($2/1M input, $6/1M output). The focus is squarely on coding and long-horizon agent work.

The notable result is Code Arena WebDev, where Qwen3.8-Max-0902 ranks first overall with 1,691 points, 3 points above Claude Opus 5 Max (1,687), 17 above Kimi K3 Max (1,674), and 22 above the previous Qwen3.8-Max (1,669). It also holds the top position on the Pareto frontier at a blended $5 per million tokens.

### Benchmark improvements over Qwen3.8-Max

All 8 coding benchmarks improved. The largest gains are on TerminalBench 3.0 (11.3 to 29.0) and ProgramBench Almost Solved (10.5 to 28.0), both more than doubling. The table below shows the full picture across coding, agent, and multimodal categories.

| Benchmark | Qwen3.8-Max-0902 | Qwen3.8-Max | Claude Opus 5 | 
|---|---|---|---|
| **Coding** | 
| TerminalBench 3.0 | 29.0 | 11.3 | 42.7 | 
| DeepSWE 1.1 | 69.3 | 56.6 | 73.6 | 
| NL2Repo-Bench | 64.9 | 55.9 | 72.3 | 
| ProgramBench (Almost Solved) | 28.0 | 10.5 | 41.5 | 
| SWE-Marathon | 44.8 | 39.1 | 50.0 | 
| MLS-Bench-Lite | **50.1** | 41.0 | 49.8 | 
| SWE-Atlas QnA | **66.3** | 60.3 | 63.2 | 
| QwenSWEBench V2 | **70.0** | 55.1 | 68.0 | 
| **Agent** | 
| CoWorkBench | 76.1 | 74.8 | 79.6 | 
| JobBench | 64.0 | 53.4 | 67.8 | 
| Toolathlon Verified | 73.3 | 72.5 | 77.6 | 
| WorkArena (Elo) | **1468** | 1348 | 1437 | 
| **Multimodal** | 
| MMMU-Pro | **82.7** | 82.3 | 82.6 | 
| ERQA | **78.3** | 77.8 | 67.3 | 

*Bold indicates Qwen3.8-Max-0902 leads Claude Opus 5 on that benchmark. All results from Alibaba's published comparison table.*

Against Claude Opus 5, the 0902 update leads on three coding benchmarks (MLS-Bench-Lite, SWE-Atlas QnA, QwenSWEBench V2) and wins on WorkArena and both published multimodal evals. Claude still leads on TerminalBench 3.0, DeepSWE 1.1, and the agent coordination benchmarks. Pricing is unchanged, so if you were already using Qwen3.8-Max, this is a free upgrade with a meaningful coding lift. Qwen also released Qwen3.8-Omni-Flash on 18th September. 

**Original Article**

Alibaba's Qwen team released Qwen3.8-Max on August 3, 2026, and it lands squarely in the most contested part of the frontier market: autonomous software engineering and long-horizon enterprise work. This is the first time Qwen plans to open-source a Max-class model, with the weights slated for release next week alongside a smaller Qwen3.8-27B.

Qwen3.8-Max is a 2.4-trillion-parameter mixture-of-experts (MoE) model with 95B active parameters, a 1M-token context window, and native text, image, and video input. The headline claim is that it scores 86.1 on OSWorld-Verified, ahead of GPT-5.6 Sol Max at 83.2, Claude Fable 5 at 85.0, and Gemini 3.1 Pro at 76.2. It also posts the highest reported PaperBench score at 93.0 and prices at $2/$6 per million input/output tokens.

In this article, I'll cover everything new with Qwen3.8-Max, looking at its coding and agentic features, exploring the benchmarks, and setting up hands-on tests for you to run. For related coverage, see our guides to Grok 4.5, GPT-5.6 Terra vs Claude Sonnet 5, and our complete guide to the Claude API. You can also read about the new Qwen3.8-Flash.

## What Is Qwen3.8-Max?

Qwen3.8-Max is the most capable model in the Qwen family, a sparse MoE architecture with 2.4 trillion total parameters and roughly 95B active per forward pass.

It is built on the architectural foundation of Qwen 3.5 and targets four tracks specifically:

- Coding
- Real-world work
- Long-horizon tasks
- Multimodal agents.

The biggest structural change from Qwen3.7-Max is scale and the open-weight commitment. Qwen has never released weights for a Max-class model before, and the company says both Qwen3.8-Max and Qwen3.8-27B will hit Hugging Face and ModelScope next week.

Alibaba has not yet disclosed the license, so it remains unclear whether this ships under something permissive like Apache 2.0 or a more restrictive custom license.

The big news on the benchmark front that matters most is agentic computer use.

On OSWorld-Verified, which measures how well an agent operates a real desktop environment, Qwen3.8-Max leads every proprietary model Qwen tested it against.

For a model that will be self-hostable, that is a genuinely different proposition than the usual open-weight release that trails the frontier by a generation.

## What's New With Qwen3.8-Max

The theme running through Qwen3.8-Max is autonomy over long horizons.

Qwen frames every capability around the same idea: the model doesn't follow a fixed plan; it self-corrects through feedback loops across days of continuous work.

Here are the features that stand out most to me.

### Multi-day autonomous coding

Qwen3.8-Max can take a coding project from an empty folder to a finished deliverable over days of continuous work without a human in the loop.

The company's flagship demonstration ran for roughly 16 days of fully autonomous operation, building a self-evolving CLI harness that accumulated 265 commits, 127 PRs, and 151 issues.

The mechanism is a feedback loop: requirements enter as GitHub issues, agents claim and execute them, then trigger build, unit tests, end-to-end tests, and CI checks before merging.

For practitioners, this points at a model designed to run unattended engineering pipelines rather than answer one-shot code questions.

The obvious caveat here is that these are vendor-run showcases.

A 16-day autonomous run in a controlled environment is not the same as pointing this at your production monorepo and walking away.

### Research reproduction and self-improvement

Qwen3.8-Max can read a research paper, reproduce its experiments in code from scratch, and then try to beat the original method.

In Qwen's example, the model worked for about 5 days (~125 hours), wrote roughly 7,600 lines of code, took over 1,100 actions, and ran 33 rounds of GPU training with no starter code provided.

It first reproduced the paper's six main findings, then ran a self-improving loop of hypothesis, code, GPU run, and analysis across 4 rounds and 18 ideas.

The final method it invented beat the paper's own approach by 2.71 points on AIME24, a competition-level math benchmark.

This is the kind of workflow that normally consumes a research team's week of setup before any experiments even start.

### Dynamic Workflows for parallel agent orchestration

Dynamic Workflows let Qwen3.8-Max plan a task programmatically and dispatch large numbers of sub-agents in parallel.

In a quant-research demonstration, the model decomposed 6 short factor descriptions into 50 research directions each, dispatched around 330 sub-agents, and completed roughly 6,000 backtests, adapting the workflow mid-run.

What makes this different from a fixed script is that the model acted on evidence.

When it spotted overfitting signals, it pruned redundant factors round by round; when three-model ensembling looked less robust than fixed-direction synthesis on small cross-sections, it switched frameworks on its own.

The selected factors hit excess Sharpe ratios of 0.64 to 1.48, which is a concrete, checkable outcome rather than a vibe.

Qwen3.8-Max uses vision throughout execution, not only to read inputs.

During a task, it inspects its own intermediate output, spots problems like a misaligned interface or an object facing the wrong way, and revises the plan to fix them.

On the input side, it handles PDFs over 200 pages across text, charts, and layout, and can process videos longer than 100 hours by building a video memory graph of people, events, and timestamps.

Qwen also shipped Qwen-MM-Plugins, a harness extension library that adds image and video processing, multimodal memory, and visual tool use to existing agent frameworks.

For anyone building document-heavy or video-heavy agents, the memory graph is the piece worth watching.

### Controllable reasoning effort

Qwen3.8-Max exposes a `reasoning_effort` parameter so you can trade cost against depth. The three levels let you tune how much the model thinks before answering:

- `xhigh` (default): for complex tasks that need thorough analysis
- `medium` : balancing accuracy and speed
- `low` : efficient reasoning optimized for speed and cost

Worth flagging: one r/LocalLLaMA commenter noted that Qwen models tend to overthink, and that DeepSeek V4 Flash spends less time reasoning even when it produces lower-quality output. If your workload is latency-sensitive, drop to `low` before assuming the default is the right call.

## Qwen3.8-Max Benchmarks

Qwen's benchmark suite leans heavily toward long-horizon execution rather than static exams, which fits the model's positioning.

Across the published tables, Qwen3.8-Max leads or stays highly competitive on agentic computer use, research reproduction, and coding, while trailing GPT-5.6 Sol Max and Fable 5 on a few software-engineering evals.

It offers one of the broadest balanced profiles rather than dominating every category.

| Benchmark (Category) | Qwen 3.8 Max | Qwen 3.7 Max | Opus 4.8 | Fable 5 | Gemini 3.1-Pro | GPT5.6 Sol (max) | 
|---|---|---|---|---|---|---|
| SWE-Pro (Software Engineering) | 67.7 | 60.6 | 69.2 | 80.0 | - | 64.6 | 
| TerminalBench-2.1 (Terminal Agent) | 86.6 | 74.5 | 84.6 | 84.6 | - | 88.8 | 
| PaperBench (Research Reproduction) | 93.0 | 64.8 | 80.3 | 88.8 | - | 90.5 | 
| FrontierSWE (Software Engineering) | 73.5 | 40.7 | 70.0 | 88.8 | - | - | 
| QwenReactBench (React Frontend) | 1724 | 1538 | 1694 | 1770 | - | 1564 | 
| CoWorkBench (Professional Cowork Tasks) | 74.8 | 64.6 | 72.3 | 75.9 | - | 71.5 | 
| JobBench (Agents Empower Human Workflows) | 53.4 | 31.3 | 48.4 | 57.4 | - | 45.4 | 
| Agents' Last Exam (Long-Horizon Valuable Tasks) | 52.4 | 31.1 | 45.1 | - | - | 53.6 | 
| BabyVision (Visual Reasoning)* | 82.0 / 91.3 | 64.7 / 70.4 | 28.4 / 81.2 | 42.5 / 90.5 | 55.9 / 68.3 | 65.5 / 88.9 | 
| CharXiv (RQ) (Chart Reasoning)* | 88.4 / 93.5 | 85.8 / 85.9 | 78.5 / 89.9 | 87.9 / 93.5 | 84.4 / 89.9 | 85.1 / 89.1 | 
| ERQA (Embodied Reasoning) | 77.8 | 69.8 | 57.2 | 70.0 | 68.0 | 70.0 | 
| PerceptionBench (Visual Perception) | 63.5 | 51.1 | 47.2 | 57.2 | 56.2 | 59.7 | 
| LVBench (Long Video Understanding) | 81.8 | 76.2 | 67.3 | 75.1 | - | 78.8 | 
| Vision2Web (Visual Web Development) | 69.0 | 42.1 | 62.4 | 70.5 | - | 62.1 | 
| MobileWorld (Agentic Mobile Use) | 77.8 | 51.2 | 67.5 | 85.5 | 58.1 | 76.9 | 
| OSWorld-Verified (Agentic Computer Use) | 86.1 | 73.3 | 83.4 | 85.0 | 76.2 | 83.2 | 

**Important Notes:**

* ***Dual Scores:** For benchmarks detailing dual metrics (BabyVision and CharXiv), values are formatted as **w/o python / w/ python** based on the inner and top numbers displayed on those specific bars.*

* ***Missing Data (-):** Hyphens indicate benchmarks where the specific model was either not tested or the score was excluded from the charts.*

### OSWorld-Verified

Qwen3.8-Max scores 86.1 on OSWorld-Verified, ahead of Fable 5 (85.0), GPT-5.6 Sol Max (83.2), and Gemini 3.1 Pro (76.2).

This benchmark measures how reliably a computer-use agent operates a real desktop environment, clicking through interfaces and completing multi-step tasks.

For anyone building agents that drive actual applications rather than call APIs, this is the number that matters most. Qwen3.7-Max scored only 73.3 here, so the jump is significant within the family.

### PaperBench

On PaperBench, Qwen3.8-Max posts 93.0, the highest reported score, ahead of GPT-5.6 Sol at 90.5 and Fable 5 at 88.8.

PaperBench measures a model's ability to reproduce a research paper's experiments in code, which maps directly to the 5-day research reproduction showcase.

This is a large gap over Qwen3.7-Max at 64.8. If your work involves turning papers into working pipelines, this is a strong signal.

### Terminal-Bench 2.1

Qwen3.8-Max scores 86.6 on Terminal-Bench 2.1, between GPT-5.6 Sol at 88.8 and the 84.6 posted by both Opus 4.8 and Fable 5.

This benchmark tests an agent working in a terminal to complete engineering tasks under a time limit.

It is a real improvement over Qwen3.7-Max's 74.5, and it puts an open-weight model within striking distance of the top proprietary scores on command-line agentic work.

### SWE-bench Pro

Here Qwen3.8-Max trails. It scores 67.7 on SWE-bench Pro, behind Fable 5 at 80.0 and Opus 4.8 at 69.2, though ahead of GPT-5.6 Sol at 64.6.

SWE-bench Pro measures resolving real GitHub issues in professional codebases.

This is the honest weak spot. When we reviewed Grok 4.5, Fable 5 also led SWE-Bench Pro at 80.4, so Anthropic's model remains the one to beat on this specific eval.

### GPQA Diamond and general capabilities

On GPQA Diamond, a graduate-level science reasoning benchmark, Qwen3.8-Max scores 92.6, level with Fable 5 (92.6) and just behind GPT-5.6 Sol at 94.1. It also leads on IFBench (instruction following) at 82.8, HealthBench at 60.2, and PRBench-Finance at 58.3.

The pattern holds across the board: strong on agentic and applied work, competitive on reasoning, and occasionally behind on the hardest reasoning exam, Humanity's Last Exam, where it scores 43.6 against Fable 5's 53.3.

## Qwen3.8-Max Pricing and Availability

Qwen3.8-Max is available now through QwenCloud via the API model ID `qwen3.8-max`. The API supports OpenAI-compatible chat completions and responses endpoints as well as an Anthropic-compatible interface, so you can point Claude Code or Codex at it with minimal config changes.

Pricing sits in the mid tier and undercuts the top US proprietary models it benchmarks against:

- **Input** : $2.00 per million tokens
- **Output** : $6.00 per million tokens
- **Implicit caching** : $0.25 per million tokens

That $8 combined total is less than a third of Claude Opus 5's $30 and under a quarter of GPT-5.6 Sol Standard's $35. It matches Grok 4.5, which also prices at $2/$6, as we noted in our Grok 4.5 coverage. The open weights for both Qwen3.8-Max and Qwen3.8-27B are due on Hugging Face and ModelScope next week, though the license terms are not yet public.

## Final Thoughts

Qwen3.8-Max is Alibaba making a statement about where open weights can compete. Topping OSWorld-Verified and PaperBench against Fable 5 and GPT-5.6 Sol Max, with a self-hostable model priced at $2/$6, is the kind of move that pressures the entire proprietary tier on cost per agentic task.

The caveats are real. The 16-day autonomous coding run and 5-day research reproduction are vendor showcases, not independent reproductions, and the license for next week's weights is still unknown. On r/LocalLLaMA, the more interesting debate was about the smaller Qwen3.8-27B, with several commenters more excited about squeezing intelligence onto a single consumer GPU than about a 2.4T behemoth that only a handful of setups can run.

My take: if you are building agents that operate real interfaces or reproduce research pipelines, Qwen3.8-Max is worth testing seriously, especially once the weights land and you can self-host. If your work is pure professional software engineering on gnarly GitHub issues, Fable 5 still leads SWE-bench Pro, so run both before committing.

If you want to get comfortable working with models like this through their APIs, I recommend our AI Fundamentals skill track to build the foundations first.

Update: September 2, 2026:Alibaba has released Qwen3.8-Max-0902, an upgraded snapshot of Qwen3.8-Max focused on coding and agent performance. The section below has been added to reflect the new benchmarks.
## What's New in Qwen3.8-Max-0902

On September 2, 2026, Alibaba released Qwen3.8-Max-0902, a post-training upgrade to Qwen3.8-Max. The architecture is unchanged: 2.4 trillion parameters, 1M token context, and the same pricing ($2/1M input, $6/1M output). The focus is squarely on coding and long-horizon agent work.

The notable result is Code Arena WebDev, where Qwen3.8-Max-0902 ranks first overall with 1,691 points, 3 points above Claude Opus 5 Max (1,687), 17 above Kimi K3 Max (1,674), and 22 above the previous Qwen3.8-Max (1,669). It also holds the top position on the Pareto frontier at a blended $5 per million tokens.

## Benchmark improvements over Qwen3.8-Max

All 8 coding benchmarks improved. The largest gains are on TerminalBench 3.0 (11.3 to 29.0) and ProgramBench Almost Solved (10.5 to 28.0), both more than doubling. The table below shows the full picture across coding, agent, and multimodal categories.

Coding50.166.370.0Agent1468Multimodal82.778.3Bold indicates Qwen3.8-Max-0902 leads Claude Opus 5 on that benchmark. All results from Alibaba's published comparison table.
Against Claude Opus 5, the 0902 update leads on three coding benchmarks (MLS-Bench-Lite, SWE-Atlas QnA, QwenSWEBench V2) and wins on WorkArena and both published multimodal evals. Claude still leads on TerminalBench 3.0, DeepSWE 1.1, and the agent coordination benchmarks. Pricing is unchanged, so if you were already using Qwen3.8-Max, this is a free upgrade with a meaningful coding lift. Qwen also released Qwen3.8-Omni-Flash on 18th September.

Original Article
Alibaba's Qwen team released Qwen3.8-Max on August 3, 2026, and it lands squarely in the most contested part of the frontier market: autonomous software engineering and long-horizon enterprise work. This is the first time Qwen plans to open-source a Max-class model, with the weights slated for release next week alongside a smaller Qwen3.8-27B.

Qwen3.8-Max is a 2.4-trillion-parameter mixture-of-experts (MoE) model with 95B active parameters, a 1M-token context window, and native text, image, and video input. The headline claim is that it scores 86.1 on OSWorld-Verified, ahead of GPT-5.6 Sol Max at 83.2, Claude Fable 5 at 85.0, and Gemini 3.1 Pro at 76.2. It also posts the highest reported PaperBench score at 93.0 and prices at $2/$6 per million input/output tokens.

In this article, I'll cover everything new with Qwen3.8-Max, looking at its coding and agentic features, exploring the benchmarks, and setting up hands-on tests for you to run. For related coverage, see our guides to Grok 4.5, GPT-5.6 Terra vs Claude Sonnet 5, and our complete guide to the Claude API. You can also read about the new Qwen3.8-Flash.

## What Is Qwen3.8-Max?

Qwen3.8-Max is the most capable model in the Qwen family, a sparse MoE architecture with 2.4 trillion total parameters and roughly 95B active per forward pass.

It is built on the architectural foundation of Qwen 3.5 and targets four tracks specifically:

The biggest structural change from Qwen3.7-Max is scale and the open-weight commitment. Qwen has never released weights for a Max-class model before, and the company says both Qwen3.8-Max and Qwen3.8-27B will hit Hugging Face and ModelScope next week.

Alibaba has not yet disclosed the license, so it remains unclear whether this ships under something permissive like Apache 2.0 or a more restrictive custom license.

The big news on the benchmark front that matters most is agentic computer use.

On OSWorld-Verified, which measures how well an agent operates a real desktop environment, Qwen3.8-Max leads every proprietary model Qwen tested it against.

For a model that will be self-hostable, that is a genuinely different proposition than the usual open-weight release that trails the frontier by a generation.

## What's New With Qwen3.8-Max

The theme running through Qwen3.8-Max is autonomy over long horizons.

Qwen frames every capability around the same idea: the model doesn't follow a fixed plan; it self-corrects through feedback loops across days of continuous work.

Here are the features that stand out most to me.

## Multi-day autonomous coding

Qwen3.8-Max can take a coding project from an empty folder to a finished deliverable over days of continuous work without a human in the loop.

The company's flagship demonstration ran for roughly 16 days of fully autonomous operation, building a self-evolving CLI harness that accumulated 265 commits, 127 PRs, and 151 issues.

The mechanism is a feedback loop: requirements enter as GitHub issues, agents claim and execute them, then trigger build, unit tests, end-to-end tests, and CI checks before merging.

For practitioners, this points at a model designed to run unattended engineering pipelines rather than answer one-shot code questions.

The obvious caveat here is that these are vendor-run showcases.

A 16-day autonomous run in a controlled environment is not the same as pointing this at your production monorepo and walking away.

## Research reproduction and self-improvement

Qwen3.8-Max can read a research paper, reproduce its experiments in code from scratch, and then try to beat the original method.

In Qwen's example, the model worked for about 5 days (~125 hours), wrote roughly 7,600 lines of code, took over 1,100 actions, and ran 33 rounds of GPU training with no starter code provided.

It first reproduced the paper's six main findings, then ran a self-improving loop of hypothesis, code, GPU run, and analysis across 4 rounds and 18 ideas.

The final method it invented beat the paper's own approach by 2.71 points on AIME24, a competition-level math benchmark.

This is the kind of workflow that normally consumes a research team's week of setup before any experiments even start.

## Dynamic Workflows for parallel agent orchestration

Dynamic Workflows let Qwen3.8-Max plan a task programmatically and dispatch large numbers of sub-agents in parallel.

In a quant-research demonstration, the model decomposed 6 short factor descriptions into 50 research directions each, dispatched around 330 sub-agents, and completed roughly 6,000 backtests, adapting the workflow mid-run.

What makes this different from a fixed script is that the model acted on evidence.

When it spotted overfitting signals, it pruned redundant factors round by round; when three-model ensembling looked less robust than fixed-direction synthesis on small cross-sections, it switched frameworks on its own.

The selected factors hit excess Sharpe ratios of 0.64 to 1.48, which is a concrete, checkable outcome rather than a vibe.

## Vision as a feedback loop, not just input

Qwen3.8-Max uses vision throughout execution, not only to read inputs.

During a task, it inspects its own intermediate output, spots problems like a misaligned interface or an object facing the wrong way, and revises the plan to fix them.

On the input side, it handles PDFs over 200 pages across text, charts, and layout, and can process videos longer than 100 hours by building a video memory graph of people, events, and timestamps.

Qwen also shipped Qwen-MM-Plugins, a harness extension library that adds image and video processing, multimodal memory, and visual tool use to existing agent frameworks.

For anyone building document-heavy or video-heavy agents, the memory graph is the piece worth watching.

## Controllable reasoning effort

Qwen3.8-Max exposes a

`reasoning_effort`parameter so you can trade cost against depth. The three levels let you tune how much the model thinks before answering:`xhigh`(default): for complex tasks that need thorough analysis`medium`: balancing accuracy and speed`low`: efficient reasoning optimized for speed and cost
Worth flagging: one r/LocalLLaMA commenter noted that Qwen models tend to overthink, and that DeepSeek V4 Flash spends less time reasoning even when it produces lower-quality output. If your workload is latency-sensitive, drop to

`low`before assuming the default is the right call.
## Qwen3.8-Max Benchmarks

Qwen's benchmark suite leans heavily toward long-horizon execution rather than static exams, which fits the model's positioning.

Across the published tables, Qwen3.8-Max leads or stays highly competitive on agentic computer use, research reproduction, and coding, while trailing GPT-5.6 Sol Max and Fable 5 on a few software-engineering evals.

It offers one of the broadest balanced profiles rather than dominating every category.

Important Notes:
*

Dual Scores:For benchmarks detailing dual metrics (BabyVision and CharXiv), values are formatted asw/o python / w/ pythonbased on the inner and top numbers displayed on those specific bars.
*

Missing Data (-):Hyphens indicate benchmarks where the specific model was either not tested or the score was excluded from the charts.
## OSWorld-Verified

Qwen3.8-Max scores 86.1 on OSWorld-Verified, ahead of Fable 5 (85.0), GPT-5.6 Sol Max (83.2), and Gemini 3.1 Pro (76.2).

This benchmark measures how reliably a computer-use agent operates a real desktop environment, clicking through interfaces and completing multi-step tasks.

For anyone building agents that drive actual applications rather than call APIs, this is the number that matters most. Qwen3.7-Max scored only 73.3 here, so the jump is significant within the family.

## PaperBench

On PaperBench, Qwen3.8-Max posts 93.0, the highest reported score, ahead of GPT-5.6 Sol at 90.5 and Fable 5 at 88.8.

PaperBench measures a model's ability to reproduce a research paper's experiments in code, which maps directly to the 5-day research reproduction showcase.

This is a large gap over Qwen3.7-Max at 64.8. If your work involves turning papers into working pipelines, this is a strong signal.

## Terminal-Bench 2.1

Qwen3.8-Max scores 86.6 on Terminal-Bench 2.1, between GPT-5.6 Sol at 88.8 and the 84.6 posted by both Opus 4.8 and Fable 5.

This benchmark tests an agent working in a terminal to complete engineering tasks under a time limit.

It is a real improvement over Qwen3.7-Max's 74.5, and it puts an open-weight model within striking distance of the top proprietary scores on command-line agentic work.

## SWE-bench Pro

Here Qwen3.8-Max trails. It scores 67.7 on SWE-bench Pro, behind Fable 5 at 80.0 and Opus 4.8 at 69.2, though ahead of GPT-5.6 Sol at 64.6.

SWE-bench Pro measures resolving real GitHub issues in professional codebases.

This is the honest weak spot. When we reviewed Grok 4.5, Fable 5 also led SWE-Bench Pro at 80.4, so Anthropic's model remains the one to beat on this specific eval.

## GPQA Diamond and general capabilities

On GPQA Diamond, a graduate-level science reasoning benchmark, Qwen3.8-Max scores 92.6, level with Fable 5 (92.6) and just behind GPT-5.6 Sol at 94.1. It also leads on IFBench (instruction following) at 82.8, HealthBench at 60.2, and PRBench-Finance at 58.3.

The pattern holds across the board: strong on agentic and applied work, competitive on reasoning, and occasionally behind on the hardest reasoning exam, Humanity's Last Exam, where it scores 43.6 against Fable 5's 53.3.

## Qwen3.8-Max Pricing and Availability

Qwen3.8-Max is available now through QwenCloud via the API model ID

`qwen3.8-max`. The API supports OpenAI-compatible chat completions and responses endpoints as well as an Anthropic-compatible interface, so you can point Claude Code or Codex at it with minimal config changes.
Pricing sits in the mid tier and undercuts the top US proprietary models it benchmarks against:

Input: $2.00 per million tokensOutput: $6.00 per million tokensImplicit caching: $0.25 per million tokens
That $8 combined total is less than a third of Claude Opus 5's $30 and under a quarter of GPT-5.6 Sol Standard's $35. It matches Grok 4.5, which also prices at $2/$6, as we noted in our Grok 4.5 coverage. The open weights for both Qwen3.8-Max and Qwen3.8-27B are due on Hugging Face and ModelScope next week, though the license terms are not yet public.

## Final Thoughts

Qwen3.8-Max is Alibaba making a statement about where open weights can compete. Topping OSWorld-Verified and PaperBench against Fable 5 and GPT-5.6 Sol Max, with a self-hostable model priced at $2/$6, is the kind of move that pressures the entire proprietary tier on cost per agentic task.

The caveats are real. The 16-day autonomous coding run and 5-day research reproduction are vendor showcases, not independent reproductions, and the license for next week's weights is still unknown. On r/LocalLLaMA, the more interesting debate was about the smaller Qwen3.8-27B, with several commenters more excited about squeezing intelligence onto a single consumer GPU than about a 2.4T behemoth that only a handful of setups can run.

My take: if you are building agents that operate real interfaces or reproduce research pipelines, Qwen3.8-Max is worth testing seriously, especially once the weights land and you can self-host. If your work is pure professional software engineering on gnarly GitHub issues, Fable 5 still leads SWE-bench Pro, so run both before committing.

If you want to get comfortable working with models like this through their APIs, I recommend our AI Fundamentals skill track to build the foundations first.
