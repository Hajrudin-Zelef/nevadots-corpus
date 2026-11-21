---
id: collect-261121-veille-ia/claude-anthropic/claude-fable-5-2-vs-gpt-6-astra-benchmarks-and-pricing
title: "Claude Fable 5.2 vs GPT-6 Astra: Benchmarks and Pricing"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["fable", "astra", "claude", "gpt-6", "anthropic", "work", "openai", "model"]
source: https://www.datacamp.com/blog/claude-fable-5-2-vs-gpt-6-astra
source_anchor: ""
source_lines: [1, 273]
section: "Claude_Anthropic"
---
Anthropic released Claude Fable 5.2 on November 9, 2026, two months after OpenAI launched GPT-6 Astra as "the world's most intelligent and aligned model." Fable 5.2 comes with a price cut against Fable 5.1, and Anthropic's launch table puts it ahead of Astra on every row where both models appear. OpenAI hasn't published a comparison against Fable 5.2 that we could find, so the obvious question for anyone choosing a frontier model is whether those numbers hold up.

In this article, I'll compare Claude Fable 5.2 and GPT-6 Astra across long-horizon knowledge work, coding, computer use, safeguards, and pricing, and then run one hands-on build task against both.

To keep up with the AI and data space, I recommend subscribing to our weekly newsletter, The Median.

## TL;DR

- Fable 5.2 beats GPT-6 Astra on every benchmark Anthropic reports for both models, by the widest margins on long-horizon knowledge work and robotics.
- Fable 5.2 is cheaper per token, and much cheaper for cached prompts and for requests above Astra's long-context threshold.
- In my hands-on shader test, Fable 5.2 built the clearly better scene, but its run cost more because it spent far more tokens.
- Choose Fable 5.2 for long agent runs, hard coding and research work, computer use, and visual builds.
- Choose GPT-6 Astra if your team lives in ChatGPT, Codex, or Azure, or needs the math and long-context results only OpenAI has published.
- Every head-to-head score comes from Anthropic, so treat the benchmark gap as Anthropic's case until independent runs appear.

## What Is Claude Fable 5.2?

Claude Fable 5.2 is Anthropic's model for its most demanding technical work, released to all users in November 2026 and built on the same underlying model as Claude Mythos 5.2, which drops Fable's cybersecurity and biology safeguards for vetted organizations.

Anthropic positions it for the most ambitious long-horizon technical work, and still recommends Claude Opus 5.5 as the default for everyday coding and knowledge tasks.

Our Claude Fable 5.2 and Mythos 5.2 guide covers the launch in full, and you can see the model in action in our Claude Fable 5.2 API tutorial.

## What Is GPT-6 Astra?

GPT-6 Astra is OpenAI's frontier flagship, launched in September 2026 as the successor to GPT-5.6 Sol.

OpenAI built its pitch around computer use and professional work, and around saturating benchmarks such as ARC-AGI-3 and FrontierMath Tier 4 that were designed to stay ahead of AI models.

Our GPT-6 Astra guide covers the launch; our GPT-6 Astra API tutorial walks through building a release check agent.

## Claude Fable 5.2 vs GPT-6 Astra: Head-to-Head Comparison

Fable 5.2 leads on every benchmark that scores both models, from coding to computer use, and only chart reading comes close to a tie. The catch is the source: all of these figures come from Anthropic's launch table, because OpenAI's own table predates Fable 5.2.

| Feature | Claude Fable 5.2 | GPT-6 Astra | 
|---|---|---|
| Developer | Anthropic | OpenAI | 
| Released | November 2026 | September 2026 | 
| Context window / max output | 1M / 128K tokens | 1.05M / 128K tokens | 
| Effort levels | low, medium, high, xhigh, max | low, medium, high, xhigh, max | 
| Remote Labor Index | **41.3%** | 20.8% | 
| Humanity's Last Exam (with tools) | **74.2%** | 57.2% | 
| FrontierCode 2.0 | **49.1%** | 42.8% | 
| OSWorld 2.1 | **89.3%** | 79.5% | 
| Chartography (no tools) | **73.4%** | 71.0% | 
| Hands-on shader test (rubric mean) | **5.0** | 3.7 | 

### Long-horizon knowledge work and research

Fable 5.2's largest lead is on open-ended, multi-step work. On the Remote Labor Index, which scores real freelance-style projects, Fable 5.2 completes 41.3% against Astra's 20.8%, roughly double.

| Benchmark | Claude Fable 5.2 | GPT-6 Astra | Notes | 
|---|---|---|---|
| Remote Labor Index (RLI) | **41.3%** | 20.8% | Fable 5.2 at high effort | 
| AA-Briefcase v1.1 (knowledge work) | **66.1% (1785 Elo)** | 52.0% (1569 Elo) | Rubric score and Elo | 
| Humanity's Last Exam (with tools) | **74.2%** | 57.2% | OpenAI reports the same Astra score | 
| Terminal-Bench-Science 0.1 | **74.6%** | 68.1% | OpenAI reports 64.6% for Astra | 
| AutomationBench (business workflows) | **51.4%** | 41.4% | OpenAI reports the same Astra score | 
| WANDR (agentic search) | 82.1% | Not published | Perplexity's data-collection benchmark | 

The pattern holds across the table: the more a task looks like a long project rather than a single answer, the wider Fable 5.2's lead. Humanity's Last Exam is no surprise: OpenAI's own launch table already showed Astra trailing Claude Fable 5.1 on it.

Anthropic's own showcase points the same way. Fable 5.2 formalized the proof of Fermat's Last Theorem in Lean in 30 hours with about 1,000 agents, a project an earlier internal research model needed 11 days for.

Two caveats before you treat this as settled:

- Every Astra number in this table was run by Anthropic, not OpenAI.
- Anthropic's Terminal-Bench-Science score for Astra is higher than OpenAI's own figure, which suggests the two labs ran it differently.

Fable 5.2 is the stronger model for long, ambiguous research and analysis projects, and the margins are large enough that a harness difference alone is unlikely to close them.

### Coding and agentic workflows

Fable 5.2 leads both coding benchmarks Anthropic reports, though by less than on knowledge work. On FrontierCode 2.0, which asks whether open-source maintainers would merge a model's changes, Fable 5.2 reaches 49.1% at xhigh effort against Astra's 42.8%.

| Benchmark | Claude Fable 5.2 | GPT-6 Astra | Notes | 
|---|---|---|---|
| FrontierCode 2.0 | **49.1%** | 42.8% | Fable 5.2 at xhigh effort; at high effort it scores 45.3% | 
| FrontierSWE v2 | **69.9%** | 65.5% | Anthropic-run | 

Anthropic adds that Fable 5.2 at high effort beats Astra's best FrontierCode score at about the same cost per task. OpenAI's own coding table uses older benchmark versions and compares Astra against Fable 5.1, so there is no OpenAI-side check yet. Fable 5.2 is the better coding model on the evidence available.

### Computer use, vision, and robotics

Fable 5.2 leads on computer use and robotics, while chart reading is close to a tie. Robotics is the outlier: on RoboDojo Sim, a simulated two-arm robot, Fable 5.2 scores 64.9% against Astra's 29.8%.

| Benchmark | Claude Fable 5.2 | GPT-6 Astra | Notes | 
|---|---|---|---|
| OSWorld 2.1 (offline set, partial) | **89.3%** | 79.5% | OpenAI reports Astra on OSWorld 2.0, a different version | 
| Chartography (no tools) | **73.4%** | 71.0% | Within a few points | 
| RoboDojo Sim | **64.9%** | 29.8% | Run internally by Anthropic | 

OpenAI called Astra "the world's best computer use model," but it made that claim against Claude Fable 5.1 and Claude Opus 5 on OSWorld 2.0. On Anthropic's newer version, Fable 5.2 takes the lead.

### Math, abstract reasoning, and long context: Astra's unanswered claims

Astra's strongest launch results sit on benchmarks Anthropic didn't report for Fable 5.2, so there is no head-to-head here, only claims on one side. The clearest is long-context retrieval: Astra scores 96.3% on OpenAI's MRCR v2 8-needle test in the 512K to 1M token band.

OpenAI also reports near-saturated scores on FrontierMath Tier 4 and ARC-AGI-3, though our GPT-6 Astra guide notes that the ARC-AGI-3 figure depends on a stateful harness that plain API calls don't get. If your work hinges on retrieval across a million-token prompt, Astra is the only one of the two with published evidence.

### Safeguards and data retention

Both models block offensive cybersecurity work by default, and they differ more on data retention than on refusals. Fable 5.2's classifiers block penetration testing, exploit generation, and most research-level biology, though Anthropic says it produces fewer unnecessary blocks than Fable 5.1; vetted organizations can apply for Claude Mythos 5.2 instead.

OpenAI rates Astra at the Critical cybersecurity threshold of its Preparedness Framework and gates exploit creation behind its Daybreak program. Astra supports Zero Data Retention for eligible API customers, while Fable 5.2 requires 30-day retention for safety monitoring unless an enterprise uses Anthropic's Enterprise Frontier Safeguards. For regulated teams that already rely on zero retention, Astra is the easier fit.

### Pricing: what you actually pay

Fable 5.2 is cheaper than GPT-6 Astra on every rate both vendors publish, and the gap is not uniform. Astra costs 1.25x as much for input, output, and cache writes, but 5x as much for cached reads, and only Astra adds a long-context surcharge.

On a plain balanced workload the difference is modest. It becomes a deciding factor once prompts are cached or run past 272K tokens.

#### Token rates side by side

Both vendors list standard rates per million tokens, plus cache, batch, and fast-mode pricing.

| Rate | Claude Fable 5.2 | GPT-6 Astra | 
|---|---|---|
| Input, per 1M tokens | $8.00 | $10.00 | 
| Output, per 1M tokens | $40.00 | $50.00 | 
| Cached input read, per 1M tokens | $0.20 | $1.00 | 
| Cache write, per 1M tokens | $10.00 (5-minute), $16.00 (1-hour) | $12.50 | 
| Long-context surcharge | None listed | Above 272K input tokens: 2x input and cache rates, 1.5x output, for the full request | 
| Batch discount | 50% | 50% (Batch and Flex) | 
| Fast mode | $16 / $80, up to 2.5x faster | 2x the applicable rates, up to 2x faster | 
| Consumer access | Eligible paid Claude plans, including Pro and Team | ChatGPT Plus, Pro, Business, Enterprise | 

The cached-read rate is the line most teams will feel, because agent loops re-send the same long prefix on every step.

#### What a real workload costs

Each row below is a monthly volume, priced as (volume ÷ 1M) × rate, summed across input and output.

| Workload | Claude Fable 5.2 | GPT-6 Astra | Difference | 
|---|---|---|---|
| Balanced assistant: 1M in / 250K out | $18.00 | $22.50 | +$4.50 (+25%) | 
| Retrieval, sub-threshold: 10M in / 1M out, each request under 272K tokens | $120 | $150 | +$30 (+25%) | 
| Retrieval, over-threshold: 10M in / 1M out, each request over 272K tokens | $120 | $275 | +$155 (+129%) | 
| Cache-heavy loop: 100K prefix written once, 1,000 cached reads, 5K fresh in / 1K out per request | $101 | $201 | +$100 (+99%) | 

- **Balanced and sub-threshold retrieval:** the premium is the flat 25% the headline rates imply.
- **Over-threshold retrieval:** the same volume costs more than twice as much on Astra, because the surcharge doubles input and raises output by half for every request above 272K tokens. Fable 5.2 lists no surcharge across its 1M window.
- **Cache-heavy loop:** Astra's cached reads cost 5x Fable's, so a long reused prefix almost doubles the bill. The row uses Fable's 5-minute cache-write rate.

Neither vendor has published token counts for a shared workload, so treat these totals as a rate comparison rather than a bill. My hands-on run below is a reminder of why: there, the cheaper model per token was the more expensive run.

Benchmarks from one vendor's launch post only go so far, so I ran one build task against both models with an identical prompt and identical tooling.

The task: a single HTML page with a full-window animated GLSL fragment shader of a lighthouse on a rocky coast during a night storm, with rolling waves breaking on the rocks, slanted rain, and low clouds lit from beneath. The lighthouse beam had to follow the mouse and light up the waves, rain, and clouds it sweeps across. p5.js could only host the canvas, run the loop, and pass the mouse position and time into the shader.

The full prompt:

```
Using p5.js (loaded from a CDN — that is the only external dependency allowed), build a single-file HTML page with a full-window animated shader. The scene: a **lighthouse on a rocky coast during a night storm, with rolling waves breaking on the rocks, slanted rain streaks, and low clouds lit from beneath**. The lighthouse beam should react to the mouse — the beam follows the cursor, and the waves, rain, and clouds it sweeps across light up where it falls. Write the visual effect as a **GLSL fragment shader**; use p5 only to host the canvas, run the animation loop, and pass the mouse position and time into the shader as uniforms.
Ship it as one working file named `index.html` that starts animating on load. Do not install packages. Do not open, screenshot, or headless-render the page (no Playwright, Puppeteer, or Chrome). Do not ask me clarifying questions — make reasonable assumptions and note them briefly in a comment at the top of the file.
```

What makes it hard:

- Four scene elements, sea, rocks, rain, and clouds, all computed per pixel inside one shader, with no textures or geometry to lean on.
- One moving light source that has to fall correctly on every one of them, wherever the cursor points.
- Doing it as real shader math, rather than faking the scene with p5's 2D drawing calls.

Anthropic's launch post quotes Unity's Sam Zhang saying Fable 5.2's clearest gains are in "building higher-quality 3D scenes from scratch," so this is a direct test of that claim. It's also a rematch of sorts: in our GPT-6 Astra vs Claude Fable 5.1 comparison, Astra won a physics simulation test against the previous Fable.

Both models ran in OpenCode with a pinned tool surface (file read and edit only), at `high` reasoning effort, one attempt each, in a fresh session, with the prompt delivered byte-for-byte.

Here are my screen recordings of both pages, moving the mouse across the scene. Fable goes first:

Anthropic released Claude Fable 5.2 on November 9, 2026, two months after OpenAI launched GPT-6 Astra as "the world's most intelligent and aligned model." Fable 5.2 comes with a price cut against Fable 5.1, and Anthropic's launch table puts it ahead of Astra on every row where both models appear. OpenAI hasn't published a comparison against Fable 5.2 that we could find, so the obvious question for anyone choosing a frontier model is whether those numbers hold up.

In this article, I'll compare Claude Fable 5.2 and GPT-6 Astra across long-horizon knowledge work, coding, computer use, safeguards, and pricing, and then run one hands-on build task against both.

To keep up with the AI and data space, I recommend subscribing to our weekly newsletter, The Median.

## TL;DR

## What Is Claude Fable 5.2?

Claude Fable 5.2 is Anthropic's model for its most demanding technical work, released to all users in November 2026 and built on the same underlying model as Claude Mythos 5.2, which drops Fable's cybersecurity and biology safeguards for vetted organizations.

Anthropic positions it for the most ambitious long-horizon technical work, and still recommends Claude Opus 5.5 as the default for everyday coding and knowledge tasks.

Our Claude Fable 5.2 and Mythos 5.2 guide covers the launch in full, and you can see the model in action in our Claude Fable 5.2 API tutorial.

## What Is GPT-6 Astra?

GPT-6 Astra is OpenAI's frontier flagship, launched in September 2026 as the successor to GPT-5.6 Sol.

OpenAI built its pitch around computer use and professional work, and around saturating benchmarks such as ARC-AGI-3 and FrontierMath Tier 4 that were designed to stay ahead of AI models.

Our GPT-6 Astra guide covers the launch; our GPT-6 Astra API tutorial walks through building a release check agent.

## Claude Fable 5.2 vs GPT-6 Astra: Head-to-Head Comparison

Fable 5.2 leads on every benchmark that scores both models, from coding to computer use, and only chart reading comes close to a tie. The catch is the source: all of these figures come from Anthropic's launch table, because OpenAI's own table predates Fable 5.2.

41.3%74.2%49.1%89.3%73.4%5.0
## Long-horizon knowledge work and research

Fable 5.2's largest lead is on open-ended, multi-step work. On the Remote Labor Index, which scores real freelance-style projects, Fable 5.2 completes 41.3% against Astra's 20.8%, roughly double.

41.3%66.1% (1785 Elo)74.2%74.6%51.4%
The pattern holds across the table: the more a task looks like a long project rather than a single answer, the wider Fable 5.2's lead. Humanity's Last Exam is no surprise: OpenAI's own launch table already showed Astra trailing Claude Fable 5.1 on it.

Anthropic's own showcase points the same way. Fable 5.2 formalized the proof of Fermat's Last Theorem in Lean in 30 hours with about 1,000 agents, a project an earlier internal research model needed 11 days for.

Two caveats before you treat this as settled:

Fable 5.2 is the stronger model for long, ambiguous research and analysis projects, and the margins are large enough that a harness difference alone is unlikely to close them.

## Coding and agentic workflows

Fable 5.2 leads both coding benchmarks Anthropic reports, though by less than on knowledge work. On FrontierCode 2.0, which asks whether open-source maintainers would merge a model's changes, Fable 5.2 reaches 49.1% at xhigh effort against Astra's 42.8%.

49.1%69.9%
Anthropic adds that Fable 5.2 at high effort beats Astra's best FrontierCode score at about the same cost per task. OpenAI's own coding table uses older benchmark versions and compares Astra against Fable 5.1, so there is no OpenAI-side check yet. Fable 5.2 is the better coding model on the evidence available.

## Computer use, vision, and robotics

Fable 5.2 leads on computer use and robotics, while chart reading is close to a tie. Robotics is the outlier: on RoboDojo Sim, a simulated two-arm robot, Fable 5.2 scores 64.9% against Astra's 29.8%.

89.3%73.4%64.9%
OpenAI called Astra "the world's best computer use model," but it made that claim against Claude Fable 5.1 and Claude Opus 5 on OSWorld 2.0. On Anthropic's newer version, Fable 5.2 takes the lead.

## Math, abstract reasoning, and long context: Astra's unanswered claims

Astra's strongest launch results sit on benchmarks Anthropic didn't report for Fable 5.2, so there is no head-to-head here, only claims on one side. The clearest is long-context retrieval: Astra scores 96.3% on OpenAI's MRCR v2 8-needle test in the 512K to 1M token band.

OpenAI also reports near-saturated scores on FrontierMath Tier 4 and ARC-AGI-3, though our GPT-6 Astra guide notes that the ARC-AGI-3 figure depends on a stateful harness that plain API calls don't get. If your work hinges on retrieval across a million-token prompt, Astra is the only one of the two with published evidence.

## Safeguards and data retention

Both models block offensive cybersecurity work by default, and they differ more on data retention than on refusals. Fable 5.2's classifiers block penetration testing, exploit generation, and most research-level biology, though Anthropic says it produces fewer unnecessary blocks than Fable 5.1; vetted organizations can apply for Claude Mythos 5.2 instead.

OpenAI rates Astra at the Critical cybersecurity threshold of its Preparedness Framework and gates exploit creation behind its Daybreak program. Astra supports Zero Data Retention for eligible API customers, while Fable 5.2 requires 30-day retention for safety monitoring unless an enterprise uses Anthropic's Enterprise Frontier Safeguards. For regulated teams that already rely on zero retention, Astra is the easier fit.

## Pricing: what you actually pay

Fable 5.2 is cheaper than GPT-6 Astra on every rate both vendors publish, and the gap is not uniform. Astra costs 1.25x as much for input, output, and cache writes, but 5x as much for cached reads, and only Astra adds a long-context surcharge.

On a plain balanced workload the difference is modest. It becomes a deciding factor once prompts are cached or run past 272K tokens.

## Token rates side by side

Both vendors list standard rates per million tokens, plus cache, batch, and fast-mode pricing.

The cached-read rate is the line most teams will feel, because agent loops re-send the same long prefix on every step.

## What a real workload costs

Each row below is a monthly volume, priced as (volume ÷ 1M) × rate, summed across input and output.

Balanced and sub-threshold retrieval:the premium is the flat 25% the headline rates imply.Over-threshold retrieval:the same volume costs more than twice as much on Astra, because the surcharge doubles input and raises output by half for every request above 272K tokens. Fable 5.2 lists no surcharge across its 1M window.Cache-heavy loop:Astra's cached reads cost 5x Fable's, so a long reused prefix almost doubles the bill. The row uses Fable's 5-minute cache-write rate.
Neither vendor has published token counts for a shared workload, so treat these totals as a rate comparison rather than a bill. My hands-on run below is a reminder of why: there, the cheaper model per token was the more expensive run.

## How Claude Fable 5.2 and GPT-6 Astra Performed

Benchmarks from one vendor's launch post only go so far, so I ran one build task against both models with an identical prompt and identical tooling.

The task: a single HTML page with a full-window animated GLSL fragment shader of a lighthouse on a rocky coast during a night storm, with rolling waves breaking on the rocks, slanted rain, and low clouds lit from beneath. The lighthouse beam had to follow the mouse and light up the waves, rain, and clouds it sweeps across. p5.js could only host the canvas, run the loop, and pass the mouse position and time into the shader.

The full prompt:

What makes it hard:

Anthropic's launch post quotes Unity's Sam Zhang saying Fable 5.2's clearest gains are in "building higher-quality 3D scenes from scratch," so this is a direct test of that claim. It's also a rematch of sorts: in our GPT-6 Astra vs Claude Fable 5.1 comparison, Astra won a physics simulation test against the previous Fable.

Both models ran in OpenCode with a pinned tool surface (file read and edit only), at

`high`reasoning effort, one attempt each, in a fresh session, with the prompt delivered byte-for-byte.
Here are my screen recordings of both pages, moving the mouse across the scene. Fable goes first:
