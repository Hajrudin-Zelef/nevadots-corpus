---
id: collect-261121-veille-ia/claude-anthropic/gpt-6-2-sol-vs-claude-opus-5-5-benchmarks-and-cost
title: "GPT-6.2 Sol vs Claude Opus 5.5: Benchmarks and Cost"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["opus", "gpt-6", "claude", "cost", "task", "effort", "coding", "benchmarks"]
source: https://www.datacamp.com/blog/gpt-6-2-sol-vs-claude-opus-5-5
source_anchor: ""
source_lines: [1, 237]
section: "Claude_Anthropic"
---
OpenAI has released GPT-6.2 Sol, the everyday tier of its GPT-6.2 family, at exactly the same API prices as GPT-6.1 Sol. When we compared GPT-6.1 Sol with Claude Opus 5.5, the split was clear: Sol for cost per task, Opus 5.5 for the highest success rate on hard agent runs at max effort. OpenAI's launch charts for GPT-6.2 Sol put that second half in doubt, and they include Opus 5.5 at max effort on almost every one.

In this article, I'll compare GPT-6.2 Sol and Claude Opus 5.5 across professional work and science, coding, business automation, and pricing, and then run both on the same hands-on shader build.

If you want a firmer grasp of how GPT models work and how to prompt them well before reading the comparison, I recommend the Intermediate ChatGPT course, which covers the architecture behind GPT models and advanced prompt writing. To keep up with the AI and data space, I recommend subscribing to our weekly newsletter, The Median.

## TL;DR

- GPT-6.2 Sol now beats Claude Opus 5.5 on about half of the benchmarks they share and comes close on the rest, at a fraction of the cost per task.
- Opus 5.5 keeps a clear lead in agentic coding on independent tests, plus narrow leads on business automation and the Artificial Analysis Intelligence Index.
- Sol still costs half as much per token, and far less per finished task because it writes fewer output tokens.
- In my hands-on shader test, Sol built the more realistic scene, while Opus made the scene react more clearly to the cursor.
- Choose GPT-6.2 Sol as the default for professional deliverables, science, and high-volume agents.
- Choose Opus 5.5 for coding agents, very long prompts, or a deployment through Cursor, Bedrock, or Vertex AI.

## What Is GPT-6.2 Sol?

GPT-6.2 Sol is OpenAI's efficient, mid-tier reasoning model, released in November 2026 alongside GPT-6.2 Astra.

OpenAI calls it its go-to model for everyday work, and its selling point is efficiency: better results than GPT-6.1 Sol on demanding tasks with fewer output tokens, at unchanged prices.

Our GPT-6.2 Astra and Sol guide covers the launch, with the full benchmark set and two hands-on tests.

## What Is Claude Opus 5.5?

Claude Opus 5.5 is Anthropic's model for long-running agentic coding and knowledge work, released on September 22, 2026, as the first model in the Claude 5.5 family.

Anthropic positions it at the level of Claude Fable 5.1 on most work while costing 40% less to run than Opus 5, and its thinking can no longer be switched off.

Our Claude Opus 5.5 guide covers the launch; our Opus 5.5 API tutorial builds an AI incident investigator with it.

## GPT-6.2 Sol vs Claude Opus 5.5: Head-to-Head Comparison

GPT-6.2 Sol now leads Claude Opus 5.5 on most shared benchmarks, and Opus 5.5 keeps narrow leads on coding and automation.

In our GPT-6.1 Sol vs Opus 5.5 comparison, Opus 5.5 led most of the max-effort results. With GPT-6.2 Sol, OpenAI's charts show Sol ahead on 5 of the 9 benchmarks both models appear on, level on 1, and less than 2 points behind on the other 3. Coding is the exception: on Artificial Analysis's independent Coding Agent Index, Opus 5.5 leads by 8 points.

| Feature | GPT-6.2 Sol | Claude Opus 5.5 | 
|---|---|---|
| Agents' Last Exam v1.1 (max effort, cost per task) | **73.0%** , $1.32 | 71.6%, $17.57 | 
| Terminal-Bench-Science 0.1 (max effort, cost per task) | **68.0%** , $2.21 | 63.3%, $23.21 | 
| FrontierCode 2.0 (max effort, cost per task) | 43.8%, $1.29 | **45.5%** , $12.82 | 
| AutomationBench 1.0.6 (max effort, cost per task) | 41.1%, $0.27 | **42.5%** , $1.44 | 
| Artificial Analysis Intelligence Index v5.0.1 (max effort, cost per task) | 57.3, $1.01 | **59.1** , $10.04 | 
| Artificial Analysis Coding Agent Index v2.1 (independent, best setting) | 50 (xhigh, in Codex) | **58** (max, in Claude Code) | 
| Context window / max output | 1.05M / 128K | 1M / 128K | 
| Knowledge cutoff | June 2026 | June 2026 | 
| Effort levels | low, medium, high, xhigh, max | low, medium, high, xhigh, max | 

### Professional work and science

This is where the 6.1 verdict flips. GPT-6.1 Sol trailed Opus 5.5 on Terminal-Bench-Science 0.1, which tests scientific work in a terminal, by 6 points at max effort. GPT-6.2 Sol now leads it, 68.0% to 63.3%, for less than a tenth of the cost per task.

The rest of OpenAI's professional-work charts point the same way:

| Benchmark | GPT-6.2 Sol (max) | Claude Opus 5.5 (max) | Notes | 
|---|---|---|---|
| GDPval v2 (OpenAI internal) | **46.9%** | 43.5% | Multistep consulting, finance, and design assignments; GPT-6.1 Sol: 41.5% | 
| Harvey LAB-AA v1.1 | **11.1%** , $1.92 | 4.2%, $17.93 | Legal deliverables that meet every criterion; GPT-6.1 Sol: 6.9% | 
| BenchCAD 2.1 Core | **73.1%** , $1.60 | 65.3%, $20.00 | CAD modeling tasks; GPT-6.1 Sol: 52.6% | 
| Vals Science Index v1 | 57.8%, $1.03 | **58.7%** , $8.71 | Interpreting biological data and running experiments; effectively tied | 

Agents' Last Exam, OpenAI's test of long-horizon professional workflows, is close to a tie at max effort. The more telling point on that chart is effort: Sol at high effort already sits above Opus 5.5 at max, for about $0.69 per task.

For documents, legal work, CAD, and scientific analysis, GPT-6.2 Sol is now the stronger model on the evidence available, not just the cheaper one.

### Coding

Coding is where Opus 5.5 still clearly leads, and the independent numbers put the gap well above what OpenAI's chart shows. On Artificial Analysis's Coding Agent Index, which combines FrontierCode, Terminal-Bench 4.1, and SWE-Atlas-QnA, Opus 5.5 in Claude Code scores 58 at max effort, against 50 for GPT-6.2 Sol in Codex at its best setting, xhigh.

GPT-6.2 Sol actually scores lower on that index than GPT-6.1 Sol did. Artificial Analysis traces the drop to SWE-Atlas-QnA, where the new model is more likely to omit key facts from its answers.

OpenAI's own FrontierCode 2.0 chart shows a gap of under 2 points at max effort. Opus 5.5 costs about ten times as much per task at max effort on the coding index (about $12.60 against about $1.15), but for repository work where the result has to be right, it is the stronger model.

### Business workflow automation

AutomationBench, Zapier's test of agents running multistep workflows across business apps, was the headline of the 6.1 comparison: Opus 5.5 led by 6 points at max effort. With GPT-6.2 Sol, the gap is down to 1.4 points.

Sol also flattens early. It scores about the same from medium effort upward, while Opus 5.5 climbs steadily and only overtakes Sol at max, where each task costs about five times as much. For a support bot or a sales-ops agent running thousands of workflows, Sol is now the obvious pick; Opus 5.5 only makes sense where a failed run costs more than the price gap.

### General reasoning

Opus 5.5 keeps a small lead on the Artificial Analysis Intelligence Index, a composite of 9 evaluations across knowledge work, coding, and science. At xhigh effort, Opus 5.5 scores level with Sol at max, for about six times the cost per task. They tie on the Vals Science Index, so general reasoning is close to parity with a slight Anthropic tilt.

### Pricing: what you actually pay

GPT-6.2 Sol is half the price of Opus 5.5 per token and far cheaper per finished task.

GPT-6.2 Sol keeps GPT-6.1 Sol's rates, so the token-price gap is the same as before. The cost-per-task gap is not, because Sol now needs fewer output tokens to finish the same work. Our own single shader run, covered below, points the same way, with Opus being almost 6x as expensive as Sol ($0.21 against $1.20).

#### Token rates side by side

GPT-6.2 Sol costs exactly half of Opus 5.5 on every standard rate, until a prompt passes 272K input tokens:

| Rate | GPT-6.2 Sol | Claude Opus 5.5 | 
|---|---|---|
| Input, per 1M tokens | $2.00 | $4.00 | 
| Output, per 1M tokens | $10.00 | $20.00 | 
| Cached input read, per 1M tokens | $0.10 | $0.20 | 
| Cache write, per 1M tokens | $2.50 (30-minute) | $5.00 (5-minute), $8.00 (1-hour) | 
| Long-context surcharge | Over 272K input tokens: 2x input and cache, 1.5x output for the whole request | None | 
| Batch discount | 50% | 50% | 
| Fast mode | 2x standard rates | $8 / $40 per 1M tokens | 
| Ultrafast | 3x standard rates through February 28, 2027 | – | 

Both models bill reasoning tokens as output, and Opus 5.5's thinking can't be switched off, so its output rate is the one that matters most.

#### What a real workload costs

| Workload | GPT-6.2 Sol | Claude Opus 5.5 | Difference | 
|---|---|---|---|
| Balanced assistant: 1M in / 250K out | $4.50 | $9.00 | $4.50 (50% less) | 
| Retrieval, each request under 272K: 10M in / 1M out | $30 | $60 | $30 (50% less) | 
| Retrieval, each request over 272K: 10M in / 1M out | $55 | $60 | $5 (8% less) | 

Each total is (volume ÷ 1M) × rate, summed across input and output, at standard rates.

- **Balanced assistant:** the plain half-price case, which most chat and agent workloads resemble.
- **Retrieval under the threshold:** still half price, so retrieval setups that keep each prompt small get the full saving.
- **Retrieval over the threshold:** the surcharge brings Sol's input rate up to Opus 5.5's, and the saving shrinks to $5. If your prompts routinely carry whole codebases, budget as if the two cost the same.

#### Why the same task costs different amounts

The two models use different tokenizers, but the bigger difference is how many output tokens each spends. OpenAI's GDPval v2 chart puts Opus 5.5 at 191,241 output tokens per task at max effort against GPT-6.2 Sol's 33,428, and at 61,121 against 12,420 at high effort.

Those are OpenAI's own measurements, but they compare the two subjects directly. If Opus 5.5 needed about 3 times Sol's output tokens on the balanced assistant workload, as it did in our own test, its monthly bill would be $19.00 against Sol's $4.50.

That is why the cost-per-task gaps in the summary table run far wider than the 2x token-price gap.

Every number so far comes from OpenAI's charts, so I ran one build task against both models with an identical prompt and identical tooling.

**The task**: a single HTML page with a full-window, animated GLSL fragment shader of an underwater kelp forest at dusk, hosted by p5.js loaded from a CDN.

The hard part is that the scene has several elements, and the cursor has to change all of them:

- Tall kelp strands swaying in the current, shafts of fading sunlight from the rippling surface, drifting particles, and a sandy floor with rocks, all drawn in one shader.
- A diver's torch that follows the cursor, lights up the kelp, particles, and seabed where it points, and pushes the nearest strands away.

It probes OpenAI's claim that GPT-6.2 builds frontends that look and work better, including 3D scenes. Both models ran in OpenCode with a pinned tool surface (file read and edit only), at high reasoning effort, with one attempt each in a fresh session and the prompt delivered byte-for-byte.

Here is what GPT-6.2 Sol built:

OpenAI has released GPT-6.2 Sol, the everyday tier of its GPT-6.2 family, at exactly the same API prices as GPT-6.1 Sol. When we compared GPT-6.1 Sol with Claude Opus 5.5, the split was clear: Sol for cost per task, Opus 5.5 for the highest success rate on hard agent runs at max effort. OpenAI's launch charts for GPT-6.2 Sol put that second half in doubt, and they include Opus 5.5 at max effort on almost every one.

In this article, I'll compare GPT-6.2 Sol and Claude Opus 5.5 across professional work and science, coding, business automation, and pricing, and then run both on the same hands-on shader build.

If you want a firmer grasp of how GPT models work and how to prompt them well before reading the comparison, I recommend the Intermediate ChatGPT course, which covers the architecture behind GPT models and advanced prompt writing. To keep up with the AI and data space, I recommend subscribing to our weekly newsletter, The Median.

## TL;DR

## What Is GPT-6.2 Sol?

GPT-6.2 Sol is OpenAI's efficient, mid-tier reasoning model, released in November 2026 alongside GPT-6.2 Astra.

OpenAI calls it its go-to model for everyday work, and its selling point is efficiency: better results than GPT-6.1 Sol on demanding tasks with fewer output tokens, at unchanged prices.

Our GPT-6.2 Astra and Sol guide covers the launch, with the full benchmark set and two hands-on tests.

## What Is Claude Opus 5.5?

Claude Opus 5.5 is Anthropic's model for long-running agentic coding and knowledge work, released on September 22, 2026, as the first model in the Claude 5.5 family.

Anthropic positions it at the level of Claude Fable 5.1 on most work while costing 40% less to run than Opus 5, and its thinking can no longer be switched off.

Our Claude Opus 5.5 guide covers the launch; our Opus 5.5 API tutorial builds an AI incident investigator with it.

## GPT-6.2 Sol vs Claude Opus 5.5: Head-to-Head Comparison

GPT-6.2 Sol now leads Claude Opus 5.5 on most shared benchmarks, and Opus 5.5 keeps narrow leads on coding and automation.

In our GPT-6.1 Sol vs Opus 5.5 comparison, Opus 5.5 led most of the max-effort results. With GPT-6.2 Sol, OpenAI's charts show Sol ahead on 5 of the 9 benchmarks both models appear on, level on 1, and less than 2 points behind on the other 3. Coding is the exception: on Artificial Analysis's independent Coding Agent Index, Opus 5.5 leads by 8 points.

73.0%, $1.3268.0%, $2.2145.5%, $12.8242.5%, $1.4459.1, $10.0458(max, in Claude Code)
## Professional work and science

This is where the 6.1 verdict flips. GPT-6.1 Sol trailed Opus 5.5 on Terminal-Bench-Science 0.1, which tests scientific work in a terminal, by 6 points at max effort. GPT-6.2 Sol now leads it, 68.0% to 63.3%, for less than a tenth of the cost per task.

The rest of OpenAI's professional-work charts point the same way:

46.9%11.1%, $1.9273.1%, $1.6058.7%, $8.71
Agents' Last Exam, OpenAI's test of long-horizon professional workflows, is close to a tie at max effort. The more telling point on that chart is effort: Sol at high effort already sits above Opus 5.5 at max, for about $0.69 per task.

For documents, legal work, CAD, and scientific analysis, GPT-6.2 Sol is now the stronger model on the evidence available, not just the cheaper one.

## Coding

Coding is where Opus 5.5 still clearly leads, and the independent numbers put the gap well above what OpenAI's chart shows. On Artificial Analysis's Coding Agent Index, which combines FrontierCode, Terminal-Bench 4.1, and SWE-Atlas-QnA, Opus 5.5 in Claude Code scores 58 at max effort, against 50 for GPT-6.2 Sol in Codex at its best setting, xhigh.

GPT-6.2 Sol actually scores lower on that index than GPT-6.1 Sol did. Artificial Analysis traces the drop to SWE-Atlas-QnA, where the new model is more likely to omit key facts from its answers.

OpenAI's own FrontierCode 2.0 chart shows a gap of under 2 points at max effort. Opus 5.5 costs about ten times as much per task at max effort on the coding index (about $12.60 against about $1.15), but for repository work where the result has to be right, it is the stronger model.

## Business workflow automation

AutomationBench, Zapier's test of agents running multistep workflows across business apps, was the headline of the 6.1 comparison: Opus 5.5 led by 6 points at max effort. With GPT-6.2 Sol, the gap is down to 1.4 points.

Sol also flattens early. It scores about the same from medium effort upward, while Opus 5.5 climbs steadily and only overtakes Sol at max, where each task costs about five times as much. For a support bot or a sales-ops agent running thousands of workflows, Sol is now the obvious pick; Opus 5.5 only makes sense where a failed run costs more than the price gap.

## General reasoning

Opus 5.5 keeps a small lead on the Artificial Analysis Intelligence Index, a composite of 9 evaluations across knowledge work, coding, and science. At xhigh effort, Opus 5.5 scores level with Sol at max, for about six times the cost per task. They tie on the Vals Science Index, so general reasoning is close to parity with a slight Anthropic tilt.

## Pricing: what you actually pay

GPT-6.2 Sol is half the price of Opus 5.5 per token and far cheaper per finished task.

GPT-6.2 Sol keeps GPT-6.1 Sol's rates, so the token-price gap is the same as before. The cost-per-task gap is not, because Sol now needs fewer output tokens to finish the same work. Our own single shader run, covered below, points the same way, with Opus being almost 6x as expensive as Sol ($0.21 against $1.20).

## Token rates side by side

GPT-6.2 Sol costs exactly half of Opus 5.5 on every standard rate, until a prompt passes 272K input tokens:

Both models bill reasoning tokens as output, and Opus 5.5's thinking can't be switched off, so its output rate is the one that matters most.

## What a real workload costs

Each total is (volume ÷ 1M) × rate, summed across input and output, at standard rates.

Balanced assistant:the plain half-price case, which most chat and agent workloads resemble.Retrieval under the threshold:still half price, so retrieval setups that keep each prompt small get the full saving.Retrieval over the threshold:the surcharge brings Sol's input rate up to Opus 5.5's, and the saving shrinks to $5. If your prompts routinely carry whole codebases, budget as if the two cost the same.
## Why the same task costs different amounts

The two models use different tokenizers, but the bigger difference is how many output tokens each spends. OpenAI's GDPval v2 chart puts Opus 5.5 at 191,241 output tokens per task at max effort against GPT-6.2 Sol's 33,428, and at 61,121 against 12,420 at high effort.

Those are OpenAI's own measurements, but they compare the two subjects directly. If Opus 5.5 needed about 3 times Sol's output tokens on the balanced assistant workload, as it did in our own test, its monthly bill would be $19.00 against Sol's $4.50.

That is why the cost-per-task gaps in the summary table run far wider than the 2x token-price gap.

## How GPT-6.2 Sol and Claude Opus 5.5 Performed

Every number so far comes from OpenAI's charts, so I ran one build task against both models with an identical prompt and identical tooling.

The task: a single HTML page with a full-window, animated GLSL fragment shader of an underwater kelp forest at dusk, hosted by p5.js loaded from a CDN.
The hard part is that the scene has several elements, and the cursor has to change all of them:

It probes OpenAI's claim that GPT-6.2 builds frontends that look and work better, including 3D scenes. Both models ran in OpenCode with a pinned tool surface (file read and edit only), at high reasoning effort, with one attempt each in a fresh session and the prompt delivered byte-for-byte.

Here is what GPT-6.2 Sol built:
