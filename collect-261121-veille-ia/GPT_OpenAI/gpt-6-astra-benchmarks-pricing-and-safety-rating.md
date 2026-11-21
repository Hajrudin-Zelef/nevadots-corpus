---
id: collect-261121-veille-ia/gpt-openai/gpt-6-astra-benchmarks-pricing-and-safety-rating
title: "GPT-6 Astra: Benchmarks, Pricing and Safety Rating"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: ["2026-09-03"]
keywords: ["astra", "claude", "against", "openai", "fable", "gpt-6", "announcement", "million"]
source: https://www.modperl.com/en/gpt-6-astra-benchmarks-pricing-cybersecurity/
source_anchor: ""
source_lines: [1, 140]
section: "GPT_OpenAI"
---
OpenAI launched GPT-6 Astra on 3 September 2026, at the same rate as Claude Fable 5.1: $10 per million input tokens and $50 per million output. The model is presented as state of the art on computer use, scientific research and cybersecurity, and it is the first OpenAI has placed at the “critical” threshold of its Preparedness Framework. On the Artificial Analysis index, published by OpenAI inside its own announcement, it still sits behind Claude Fable 5.1. The real shift is elsewhere, in the cost per task.

This is GPT-6 Astra. Anything you can do on a computer, Astra can do for you. Fast.

— OpenAI (@OpenAI) 3 September 2026

## What OpenAI actually announced

GPT-6 Astra lands three months after GPT-5.6 Sol and two days after Claude Fable 5.1. OpenAI presents it as its “most intelligent and aligned” model, state of the art on computer use, browsing, software engineering, cybersecurity, science and professional work.

Three scores serve as the saturation argument in the announcement: 97.6% on FrontierMath Tier 4, 99.9% on ARC-AGI-3 and 100% on ExploitBench. The model also helped improve two bounds on the gaps between prime numbers, one of which had not moved in more than eighty years.

If you follow the lineup closely, keep in mind that the useful comparison is not with GPT-5.6 Sol but with Claude Fable 5.1, released two days earlier at the same price.

### Pricing identical to Claude Fable 5.1

The standard grid mirrors that of its direct competitor, to the dollar on both input and output. Cache rates get a separate line that OpenAI does not detail in the announcement.

| Item | Rate | 
|---|---|
| Input | $10 per million tokens | 
| Output | $50 per million tokens | 
| Cache reads and writes | Separate rates, not published in the announcement | 
| Fast mode | Up to 2 times faster, at twice the standard rate | 
| API identifier | gpt-6-astra | 

Astra supports zero data retention for eligible API customers. Claude Fable 5.1, on the contrary, requires 30-day retention unless Anthropic expressly authorises otherwise.

### The rollout timeline

The rollout began on announcement day for a limited set of organisations. It extends “over the coming days” to every ChatGPT Plus, Pro, Business and Enterprise account, as well as to the OpenAI API, Microsoft Azure and AWS Bedrock. If you cannot see Astra in your account yet, that is expected.

Two housekeeping details are worth noting:

- Astra usage counts against existing subscription allowances, with credits available for purchase beyond them;
- on Enterprise, access is off by default at launch: the workspace administrator has to enable it.

The Pro, Business and Enterprise plans additionally get access to a GPT-6 Astra Pro variant.

## The official benchmarks, figure by figure

The values below come from the table published in OpenAI’s official announcement, with the comparison columns OpenAI itself selected: GPT-5.6 Sol, Claude Fable 5.1, Claude Fable 5, Claude Opus 5 and Gemini 3.8 Flash.

### Where Astra pulls clearly ahead

The widest gap lands on Terminal-Bench Science 0.1, the agentic scientific research test: 64.6% against 52.6% for Claude Fable 5.1 and 22.4% for GPT-5.6 Sol. OpenAI reports that score at an estimated API cost roughly 31% below that of Fable 5.1.

| Benchmark | Astra against Claude Fable 5.1 | 
|---|---|
| Terminal-Bench Science 0.1 | 64.6% against 52.6% | 
| Terminal-Bench 4.0 | 57.9% against 55.8% | 
| AutomationBench | 41.4% against 31.4% | 
| FrontierMath Tier 4 (v2) | 97.6% against 87.8% | 
| ARC-AGI-2 | 95.0% against 90.0% | 
| GPQA Diamond | 96.0% against 93.7% | 
| DeepSWE v1.1 | 74.1% against 67.4% | 

On computer use, Astra scores 59.3% on Agents’ Last Exam against 55.5% for Claude Opus 5, and 72.6% on OSWorld 2.0 in roughly 47% less time per task than GPT-5.6 Sol.

On long context, it holds 96.3% on the MRCR test between 512,000 and 1 million tokens, against 73.8% for Sol. If your agents work on very large repositories, that is the line to look at before any other.

### Where Claude is still ahead

The announcement does not hide the losing lines, and they carry weight. On the Artificial Analysis Intelligence Index v4.1.1, Astra scores 61.2 while Claude Fable 5.1 sits at 65.7 and Claude Opus 5 at 63.1. On the Coding Agent Index v1.4, it is 67.0 against 67.2 for Fable 5.1 and 68.1 for Fable 5.

Humanity’s Last Exam with tools remains clearly in Anthropic’s favour, 65.0% against 57.2%. On FrontierCode 1.1 Main, the three models are within half a point of each other.

OpenAI publishes, inside its own announcement, the index that places it behind: 61.2 for Astra against 65.7 for Claude Fable 5.1. That is the kind of line press pickups skip.

Three footnotes change how several lines should be read, and they are worth the detour:

- on ScreenSpot-Pro and ExploitGym, the scores credited to Claude actually come from Mythos, the variant with fewer safeguards;
- on BenchCAD, Claude’s scores reflect three modifications to the evaluation described in the Fable 5.1 system card;
- Claude Fable 5 and 5.1 are absent from LifeSciBench, GeneBench Pro and MedChemBench because they refuse the majority of questions in those test sets.

Another detail surfaces when you compare the two announcements: on Terminal-Bench Science 0.1, Anthropic credits Claude Opus 5 with 29.0% and OpenAI with 30.0%. One point apart on the same test, depending on who is measuring.

One figure deserves a closer look. OpenAI presents ARC-AGI-3 as saturated at 99.9%, but the ARC Prize foundation, which administers the test, publishes two values: 63% with its standard harness, and 99% only with a new adapter supplied for the occasion. The announcement’s own footnote half admits it, noting that the model ran with a harness that “changes two settings”.

GPT-6 Astra by @OpenAI achieves SOTA on ARC-AGI: Astra scores 63% on ARC-AGI-3, 99% via a new provider adapter harness. It surpasses human performance on 96% of ARC-AGI-3 levels. It builds the most precise symbolic model of novel environments we’ve seen.

— ARC Prize (@arcprize) 3 September 2026

### Cost per task, the real gap

Artificial Analysis, which evaluates both models with the same protocol, gives the figure that matters for a bill. At maximum effort, Astra solves an index task for $1.67 while producing roughly 15,000 tokens. Claude Fable 5.1 climbs to $3.69 and 45,000 tokens for the same task.

Twice as expensive per task at an identical list price: it all comes down to verbosity. Astra says three times less to get to the end, and output is what costs $50 per million.

My first “holy shit” moment with GPT-6 Astra: I asked it to create a world in Unreal Engine, and fill it with humans (each an Astra-powered agent) who all have to work together to survive.

— Matt Shumer (@mattshumer_) 3 September 2026

## First model rated “critical” for cybersecurity

This is the heaviest point in the announcement. Astra reaches the “critical” cybersecurity threshold of OpenAI’s Preparedness Framework, a first. Without production safeguards, it scores 100% on ExploitBench against 78.5% for GPT-5.6 Sol, and 42.4% on ExploitGym against 30.3%.

OpenAI built two fresh tests to rule out contamination from already-known flaws. On an ExploitBench limited to vulnerabilities from June to August 2026, Astra reaches 39.0% against 11.5% for Sol.

During that evaluation, the model discovered and exploited two zero-day flaws unknown until then, since reported to their maintainers.

On SRE-Bench, which measures reverse engineering of binaries without source code, Astra solves 88.0% of tasks on the first attempt and 99.2% within four, against 55.9% and 68.7% for Sol.

The version now online refuses advanced offensive tasks, starting with the production of proof-of-concept exploits. OpenAI plans to open less restrictive safeguards through its Daybreak programme, for vulnerability validation, malware analysis and detection engineering. The Astra system card details the methodology and the security testing.

## The alignment numbers OpenAI puts forward

OpenAI built an evaluation after the Hugging Face incident to measure whether a model faced with an impossible task steps outside its scope. Without production safeguards, GPT-5.6 Sol went beyond the authorised target in 48% of cases. Astra does so in 0% of cases.

The other internal counters point the same way: 2.4% of failures on the internal computer-use safety benchmark against 22.0% for Sol and 9.5% for Fable 5.1, and 4.2% on the capability-hallucination benchmark against 12.2%.

One admission tempers the picture, and it is in the announcement: Astra’s written reasoning is harder to monitor than Sol’s. OpenAI attributes this to the model solving simple tasks in fewer written steps, says it takes the matter seriously, and is deploying misalignment monitoring in production for models of this class.

## Astra or Claude Fable 5.1: the split comes down to three lines

Both models cost the same at list price and target the same audience. If you have to pick one, decide on three precise lines rather than on a general impression.

- **Real cost per task** leans clearly towards Astra, $1.67 against $3.69, because it produces three times fewer output tokens.
- **Raw reasoning quality** leans towards Claude, 65.7 against 61.2 on the Artificial Analysis index, and 65.0% against 57.2% on Humanity’s Last Exam with tools.
- **Compliance constraints** lean towards Astra if you need zero data retention, since Claude Fable 5.1 imposes 30 days.

On pure agentic coding, the two are level: 67.0 against 67.2 on the Coding Agent Index. Measure on your own requests before moving a production workload. If you are coming from an older model in the OpenAI lineup, our article on what the GPT-5 generation changed gives the intermediate comparison points, and the one on Claude Opus 5 covers the $5 and $25 tier that remains the best value for many uses.

## Frequently asked questions

### Which identifier should you use in the API?

Write `gpt-6-astra` on the OpenAI API. You will also find it through Microsoft Azure and AWS Bedrock. Fast mode doubles both the speed and the rate.

### Is Astra really an AGI?

That is a communication line, not a measured result. OpenAI’s president spoke of a “generational leap” that could one day be seen as the arrival of AGI. The published scores show a model leading on agentic research and cybersecurity, and trailing Claude Fable 5.1 on the most widely followed reasoning index.

### Can you use it for offensive cybersecurity?

No, not in the public version: Astra refuses advanced offensive tasks such as writing proofs of concept. You can, however, use it for secure code review and patching, and broader access is planned through the Daybreak programme.

### What does the new Codex context memory change?

Instead of compacting the history into a summary when the window fills up, Astra keeps notes that travel across context windows, and earlier windows remain searchable. You enable it in Codex’s `config.toml`; it will become the default behaviour in the coming weeks.
