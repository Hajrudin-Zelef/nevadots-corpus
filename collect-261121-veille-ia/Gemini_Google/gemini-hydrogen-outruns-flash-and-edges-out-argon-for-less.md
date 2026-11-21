---
id: collect-261121-veille-ia/gemini-google/gemini-hydrogen-outruns-flash-and-edges-out-argon-for-less
title: "Gemini Hydrogen Outruns Flash and Edges Out Argon for Less"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["hydrogen", "gemini", "google", "flash", "high", "argon", "faster", "intelligence"]
source: https://www.intelligentliving.co/gemini-hydrogen-outruns-flash-edges/
source_anchor: ""
source_lines: [1, 99]
section: "Gemini_Google"
---
Google released Gemini 4 Hydrogen on November 12, 2026 as the direct replacement for Gemini 3.8 Flash, and it is a much bigger step than a Flash refresh. At its default high reasoning setting it finishes tasks 2.5 times faster than 3.8 Flash, more than triples Terminal-Bench 4.1 from 18.2% to 57%, and moves past Gemini 4 Argon on the Artificial Analysis Intelligence Index, 51 versus 50, rising to 54 at its new xhigh setting. It is now live across Google AI Studio, the Gemini API, Gemini for business, the Gemini app, Google Search, and Google Antigravity for paid Pro and Ultra subscribers.

Hydrogen is Google DeepMind’s everyday execution model for coding and agentic workflows. Google positions Carbon and Argon for deep, long horizon reasoning, while Hydrogen is built for rapid execution across diverse agentic tasks, either on its own or as the fast worker paired with Carbon in multi model sessions.

That pairing is now built into Antigravity through Orchestrator, which switches between Carbon and Hydrogen in the same session. On Terminal-Bench 4.0, Orchestrator reached near-Carbon task success while consuming 24% less quota and completing 19% faster, according to Google. For parallel agents, Carbon can coordinate while Hydrogen powers worker and verifier agents in isolated workspaces.

The practical spec keeps things simple. Hydrogen has a 1 million token input window with up to 1 million output tokens, supports text, image, speech and video input with text output, and offers four thinking levels: low, medium, high as the default, and a new xhigh tier. Pricing is flat across the full context at $1.50 per million input tokens and $7.50 per million output tokens, with 95% off cached input.

This is the upgrade that matters for Flash users. Artificial Analysis scores Gemini 3.8 Flash High at 31 on Intelligence Index v5.0.1, while Hydrogen High scores 51 and Hydrogen Xhigh scores 54. That is a 20 to 23 point jump on the same independent index, not a small iteration.

Google’s own developer numbers point the same way, with faster execution, fewer hallucinations, and cleaner recovery when a tool call fails or a search returns empty. Early partners also saw faster runs with fewer tokens on complex graph, SQL, and data-heavy enterprise work.

The table below collects Google’s published launch benchmarks for Hydrogen across coding, agentic reasoning, and multimodal knowledge work.

| Benchmark (Google launch report) | Gemini 4 Hydrogen | Comparison / note | 
|---|---|---|
| **Terminal-Bench 4.1** | **57.0%** | 18.2% on Gemini 3.8 Flash | 
| **HLE-Diamond** | **58.8%** | Leads its class | 
| **Agents’ Last Exam v1.1** | **71.6%** | End-to-end professional workflows | 
| **Multimodal Knowledge Agent** | **53.4%** | Dense visual file workflows | 
| **Agent Arena** | **#5** | Live multi-step agent leaderboard | 
| **Google Sheets canvas** | **>30% faster** | Interactive financial models vs prior models | 
| **Partner SQL / graph runs** | **2x–2.8x faster** | 12%–23% fewer tokens vs 3.8 Flash | 

Hydrogen High sits one point above Argon High (51 versus 50) on the Artificial Analysis Intelligence Index, and Hydrogen Xhigh extends that to four points (54 versus 50). A single index point is inside noise on a composite benchmark, so the honest reading is that the two are roughly level on headline intelligence at High, with Hydrogen pulling clearly ahead only at Xhigh. The per-task cost and speed gaps are the meaningful separators here, and the exact figures sit in the crossover table below rather than repeated in this paragraph.

In other words, the two are roughly matched on headline intelligence at High, while Hydrogen is far faster per task and costs less per task at current Artificial Analysis measured rates. Carbon still beats both on intelligence at 60, but at Argon style pricing of $2.00 input and $10.00 output. That combination is why Argon looks likely to be phased out over time, even though Google has not announced a discontinuation. This is analysis based on positioning and measured cost, not a Google statement.

For context on how Carbon took the frontier lead at Argon pricing, see our Gemini 4 Carbon breakdown.

**Read this before comparing any price below: every 3.8 Flash cost-per-task figure in this article is measured at its half-price introductory rate, which expires December 31, 2026.** The sticker can mislead. At list API rates Hydrogen at $1.50 and $7.50 looks expensive next to 3.8 Flash at $0.75 and $3.75. That Flash rate is an introductory discount. Our Gemini 3.8 Flash pricing comparison documents $0.75 and $3.75 through December 31, 2026, doubling to $1.50 and $7.50 on January 1, 2027. After that date the raw API sticker is identical.

Hydrogen at $1.50 and $7.50 is also only about 25% below the current $2.00 and $10.00 discounted rates for Argon and Carbon shown on Artificial Analysis. The real crossover is cost per completed task, which includes reasoning and answer tokens, not just the per million rate.

Artificial Analysis measures the current discounted Flash pricing and Hydrogen still wins on task cost by a wide margin. Flash High costs $7.77 per Index task and Flash Medium costs $6.26 per task, while Hydrogen High costs $1.87 and Hydrogen Xhigh costs $3.16, so Hydrogen Xhigh is cheaper per task than Flash Medium even though Flash is measured at its half price intro rate. These four figures also appear as rows in the crossover table below, which is where readers should compare them side by side. Medium is already the value pick inside Hydrogen at 41 on the Index for $0.80 per task and 3.7 minutes per task, already above every 3.8 Flash setting measured on v5.0.1.


No single number decides value. The table below puts the crossover in one place: capability index, raw API price, measured cost per task, and speed. All Artificial Analysis figures below were read directly from live model pages on Index v5.0.1. Vals.ai figures are from the live Hydrogen model page and update post.

| Model and setting | AA Intelligence Index v5.0.1 | Raw API, per 1M input / output | Cost per Index task | Output speed | Time per Index task | Time to first answer | 
|---|---|---|---|---|---|---|
| **Gemini 4 Hydrogen High, default** | **51** | $1.50 / $7.50 | **$1.87** | 170 tok/s | 8.3 min | 18.89s | 
| **Gemini 4 Hydrogen Xhigh** | **54** | $1.50 / $7.50 | **$3.16** | 190 tok/s | 13.5 min | 37.77s | 
| Gemini 3.8 Flash High | 31 | $0.75 / $3.75 intro, then $1.50 / $7.50 | $7.77 | 112 tok/s | 14.7 min | 17.53s | 
| Gemini 3.8 Flash Medium | 30 | $0.75 / $3.75 intro, then $1.50 / $7.50 | $6.26 | 105 tok/s | 13.0 min | 12.75s | 
| Gemini 4 Argon High | 50 | $2.00 / $10.00 current, $4.00 / $20.00 list | $4.97 | 63 tok/s | 30.6 min | 41.30s | 
| Gemini 4 Carbon High | 60 | $2.00 / $10.00 current, $4.00 / $20.00 list | $5.48 | 73 tok/s | 28.1 min | 63.37s | 
| GPT-6.1 Sol Max | 55 | $2.00 / $10.00 | $1.27 | 49 tok/s | 23.3 min | 174.46s | 
| Claude Sonnet 5.5 Max | 55 | $2.00 / $10.00 | $10.02 | 139 tok/s | 34.1 min | 240.17s | 

The crossover is clear: Hydrogen delivers a major intelligence jump over 3.8 Flash at a fraction of the task cost, matches Argon on headline intelligence at High while pulling ahead at Xhigh, and undercuts Argon on both cost and completion time. Against frontier pricing, Hydrogen High sits close to GPT-6.1 Sol Max on task cost and far below Sonnet 5.5 Max, while trailing both by only a point on intelligence at Xhigh.

The headline Intelligence Index combines nine independent evaluations. The table below breaks down Gemini 4 Hydrogen’s component scores alongside 3.8 Flash, Argon, Carbon, GPT-6.1 Sol, and Claude Sonnet 5.5 from Artificial Analysis model pages and our Gemini 4 Carbon report.

| AA v5.0.1 Evaluation | Hydrogen | 3.8 Flash (High) | Argon (High) | Carbon (High) | GPT-6.1 Sol (Max) | Sonnet 5.5 (Max) | 
|---|---|---|---|---|---|---|
| **AA-Briefcase v1.1 (Elo)** | 1567 | 1203 | 1484 | 1609 | 1555 | **1816** | 
| **AutomationBench-AA** | 76.1% | 59.9% | 77.5% | **77.8%** | 64.9% | 71.8% | 
| **Terminal-Bench 4.0** | 53.5% | 19.7% | 57.1% | **65.2%** | 56.1% | 63.6% | 
| **FrontierCode-AA 2.0** | 39.0% | 18.7% | 25.7% | 40.6% | 40.6% | **41.7%** | 
| **Terminal-Bench-Science 0.1** | 49.5% | 10.0% | 52.9% | **64.8%** | 58.1% | 53.3% | 
| **HLE-Diamond** | 58.8% | 33.1% | 56.7% | **69.1%** | 58.8% | 53.3% | 
| **GDP.pdf** | 27.6% | 21.0% | 21.8% | 28.4% | **31.0%** | 25.8% | 
| **CritPt v2 (Hard Subset)** | 66.0% | 46.5% | 52.5% | 63.0% | **69.5%** | 59.0% | 
| **AA-Omniscience v1.1 Accuracy** | 50% | 51% | 48% | **58%** | 62% | 51% | 
| **AA-Omniscience Non-Hallucination** | 92% | 45% | **96%** | 89% | 48% | 57% | 
| **AA-Omniscience v1.1 Net Score** | 46 | 23 | 46 | **54** | 42 | 30 | 

Artificial Analysis measures broad knowledge, coding, and reasoning, while Vals.ai tests professional work at xhigh thinking effort. Hydrogen’s strongest outside proof point here is a first-place finish on Tax Agent Bench ahead of both Gemini 4 Carbon and Claude Fable 5.2.

On Legal Research the two sources disagree, and the disagreement matters more than either number alone. Google’s launch materials report 55.8% as tied for first; the live Vals model page at the time of reading shows 53.85% at number 6 of 94. The table below uses the live Vals page figure, not Google’s launch figure, and does not average the two. The gap likely reflects different harnesses or reruns as Vals adds models, so treat Google’s figure as the launch-harness result and the live page as the current independent run.

| Vals.ai Benchmark (xhigh) | Gemini 4 Hydrogen | Rank | Comparison / note | 
|---|---|---|---|
| **Vals Index v2.2** | **68.49%** | #6 / 37 | $9.84 per test, 32m 5s latency | 
| **Tax Agent Bench** | **52.30%** | #1 / 85 | Ahead of Carbon (51.78%) and Fable 5.1 (50.75%) | 
| **Vals Finance Index** | **66.14%** | #2 / 40 | Behind Carbon (67.05%) | 
| **EMB v1.1** | **75.80%** | #6 / 42 | Top-6 finish | 
| **Legal Research Bench** | **53.85%** | #6 / 94 | Google launch table reports 55.8% (harness difference) | 
| **Terminal-Bench 4.0** | **54.04%** | #11 / 64 | Mid-pack coding | 
| **Vibe Code Bench v1.1** | **87.78%** | #13 / 128 | Mid-pack coding | 

Hydrogen’s four thinking levels scale cost and latency in a clear ladder on Artificial Analysis: Medium is the value setting for routine work, High is the default for harder coding and agent runs, and Xhigh is reserved for tasks where three extra Index points justify almost double the output tokens. Low answers first in 5.71 seconds. The exact Artificial Analysis figures for High and Xhigh sit in the crossover table above, with Low (27 on the Index, $0.29 per task, 1.3 minutes) and Medium (41 on the Index, $0.80 per task, 3.7 minutes) below them.

Safety is also a core advance over 3.8 Flash. Google reports up to 81% less overreach and up to 79% less underservice in agentic tasks, 12 times fewer misaligned actions and task shortcuts, up to 2 times less likelihood of bypassing permission boundaries when blocked, and 96.7% honesty on its internal agent honesty evaluation. It preserves 99.9% chain of thought legibility across more than 200,000 audited turns, improves resilience to indirect prompt injection by more than 2.3 times on Gray Swan IPI testing, and keeps false refusals under 0.4% on HealthBench Pro.

Yes. Google describes Hydrogen as a major standalone upgrade for teams migrating from 3.8 Flash, with 2.5 times faster task completion at the default setting and Terminal-Bench 4.1 rising from 18.2% to 57%. After Flash introductory pricing ends, both list at $1.50 input and $7.50 output, which removes any sticker reason to stay on Flash.

List API pricing is $1.50 per million input tokens and $7.50 per million output tokens, flat across 1M context, with 95% off cached input. The per-setting task costs behind that sticker sit in the table above. Note the Flash figures in that table expire with its intro discount on December 31, 2026.

On the Artificial Analysis Intelligence Index the two are roughly level at High, with Hydrogen pulling clearly ahead only at Xhigh. Hydrogen High also costs less per task than Argon and finishes in less than a third of the time; the exact figures sit in the crossover table above. Carbon remains ahead of both on intelligence, at Argon-style pricing.

Google has not announced that. Argon looks stranded on value because Hydrogen beats it on index, speed and task cost while Carbon beats it on intelligence at the same price card, but discontinuation remains analysis rather than confirmed fact.

Google reports 2.5 times faster task completion than 3.8 Flash at default settings, and 2 to 2.8 times faster with 12% to 23% fewer tokens in partner SQL and data heavy workflows. Artificial Analysis output speed and time-per-task figures are tabled above and not repeated here.

Starting November 12, 2026 it is rolling out across AI Studio on all tiers, the Gemini API, Gemini for business, the Gemini app, Google Search for all users, and Antigravity for paid Pro and Ultra subscribers, plus Gemini in Sheets for eligible business subscriptions.

Hydrogen is the rare mid tier model that changes the lineup above it. It gives Flash users a clear reason to move, with far higher intelligence and far lower cost per task even before Flash introductory pricing expires. It passes Argon while running faster and cheaper, which leaves Argon without a clear role unless Google keeps it for a specific niche. Carbon remains the answer for maximum intelligence at 60, but Hydrogen at 51 to 54 covers everyday coding, research, tax, legal retrieval and multimodal knowledge work at a fraction of the task cost and time. Test Medium for routine volume, default to High for agent work, and reserve Xhigh for high value tasks where the last points matter.
