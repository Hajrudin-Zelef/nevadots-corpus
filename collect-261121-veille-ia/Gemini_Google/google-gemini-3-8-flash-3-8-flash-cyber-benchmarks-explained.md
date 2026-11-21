---
id: collect-261121-veille-ia/gemini-google/google-gemini-3-8-flash-3-8-flash-cyber-benchmarks-explained
title: "Google Gemini 3.8 Flash & 3.8 Flash Cyber Benchmarks Explained"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-09-02"]
keywords: ["flash", "google", "opus", "gemini", "cyber", "coding", "table", "behind"]
source: https://www.vellum.ai/blog/gemini-3-8-flash-benchmarks-explained
source_anchor: ""
source_lines: [1, 78]
section: "Gemini_Google"
---
## Google Gemini 3.8 Flash & 3.8 Flash Cyber Benchmarks Explained

Google released Gemini 3.8 Flash and 3.8 Flash Cyber on September 2, 2026, its third Flash release in six weeks: 3.6 Flash shipped July 21 and 3.7 Flash on August 13, per Fello AI. The framing is workload economics. Google calls 3.8 Flash "our best reasoning & coding model yet, at the same speed and low cost of 3.7," and ships it as two variants of one foundational model: a general-purpose Flash for everyone, and a Cyber twin restricted to vetted defenders through the new Fairwind Program. The claim that ties the pair together is the spiciest one in the post: the coding and reasoning gains across the shared core were driven by a set of innovations that Google says includes rigorous training in the highly demanding domain of cybersecurity.

The launch evaluation table (methodology) pits 3.8 Flash against Gemini 3.7 Flash, Claude Opus 5, Claude Sonnet 5, and GPT-5.6 Sol and Terra across 14 benchmark rows. It wins 8 outright, sits within 0.3 points of Opus 5 on the coding row that matters most, and loses three rows by enough that the table itself is the most honest part of the launch. Here's the full walk, in the order Google published it.

## 1. The price row, and the meter behind it

Through December 31, 2026, 3.8 Flash costs **$0.75 per million input tokens and $3.75 per million output tokens**. On January 1, 2027, those become $1.50 and $7.50. Cached input runs $0.075 per million, per Artificial Analysis numbers compiled by Intelligent Living. Claude Opus 5 lists at $5 and $25, GPT-5.6 Sol at $4 and $20, Sonnet 5 at $2 and $10. Against Opus 5, 3.8 Flash is roughly 6.7 times cheaper on both sides of the ledger.

The catch is that the meter runs longer. Google's own framing is that 3.8 Flash "works harder": on complex tasks it executes extra reasoning steps and calls tools iteratively, and at higher effort levels it spends more tokens. Emergent's read of the evaluation report puts the gains at roughly 30% more output tokens per task and more turns on agentic evaluations. The rate is unchanged; the model just keeps the meter running. Developers who want the old economics can drop the effort level or stay on 3.7 Flash, which Google says remains fully supported. The shipped spec is otherwise familiar: a 1,048,576-token context window, 65,536-token output, and three thinking levels, defaulting to medium.

## 2. Coding: DeepSWE v1.1 and Terminal-bench 2.1

On DeepSWE v1.1, long-horizon software engineering, 3.8 Flash scores **73.7%**, up from 3.7 Flash's 65.3% and within 0.3 points of Claude Opus 5's 74.0%. Google's separate DeepSWE chart, run with Datacurve, plots accuracy against average cost per task and puts 3.8 Flash in Opus 5's accuracy band at a fraction of the cost per task, with the rest of the Flash line and most smaller open models trailing well behind on both axes.

Terminal-bench 2.1, agentic terminal work, is the row where 3.8 Flash leads the entire table: **89.4%** against Opus 5's 89.1% and Sol's 88.8%. A Flash-priced model holding the top score on terminal coding is the single most consequential number in the launch, because terminal work is where the Flash line had trailed Anthropic and OpenAI, per AI Release Tracker.

## 3. Knowledge work: GDPVal-AA v2

GDPVal-AA v2 measures general professional knowledge work on an Elo scale, and this is where the ceiling shows. 3.8 Flash scores **1545**, ahead of 3.7 Flash's 1482 but 279 Elo behind Opus 5's 1824 and 165 behind Sol's 1710.

The pattern to hold onto for the rest of the table: 3.8 Flash closes gaps sharply when the work has a specialized shape, and stays a tier behind when it doesn't.

## 4. Domain agents: Vals Finance Agent v2 and Harvey's Legal Agent Benchmark

This is the row pair Google is loudest about, and the table backs it. On Vals AI's Finance Agent v2, 3.8 Flash scores **61.4%**, the best figure published, ahead of Opus 5 at 58.6% and 3.7 Flash at 59.0%. On Harvey's Legal Agent Benchmark, scored as an all-pass rate, 3.8 Flash posts **10.0%** against Opus 5's 6.7% and Sol's 2.5%. The absolute Harvey numbers look small because all-pass is brutal: every document in a legal workflow has to clear for a single point. On both rows, a model at 1/6.7th Opus 5's input price finishes first.

## 5. The general-agent ceiling: Terminal-bench 4.0 and OSWorld-2.0

Now the honest part. Terminal-bench 4.0 tests general agent work over long horizons, and 3.8 Flash scores **19.1%** to Opus 5's **51.8%**. Sol lands at 37.3%, Terra at 23.6%, Sonnet 5 at 12.4%. On OSWorld-2.0, computer use under a batch tool setting, 3.8 Flash reaches **59.0%** against Opus 5's **75.4%**, with Sol at 62.6%. Document comprehension tells the same story: on GDP.PDF, another all-pass measure, Sol leads at 40.0% with Opus 5 at 37.0% and 3.8 Flash at 35.0%.

Launch-day coverage split along exactly this seam. eesel's review framed the divide as throughput versus latency: for batch agents grinding through a repo overnight, 3.8 Flash wins on economics, and for interactive workloads where a person waits, the effort dial changes the experience. beam.ai's early benchmark read landed on the same operating answer: most production stacks will route across both Opus 5 and 3.8 Flash rather than standardize on either.

## 6. Reasoning and perception: HLE-Verified, CharXiv, LVBench

On HLE-Verified, multidisciplinary expert reasoning, 3.8 Flash posts **54.9%**, the best figure in the table, in a three-way squeeze with Sol at 54.5% and Opus 5 at 54.4%. Sonnet 5 sits at 31.0%.

Two more perception rows round out the section, both Google wins. On CharXiv reasoning over charts without tools, 3.8 Flash scores **86.2%**, ahead of Terra at 85.9%, Sol at 85.8%, and Opus 5 at 83.7%. On LVBench long-video understanding, it posts **87.8%** in agentic mode and **87.1%** static, against Opus 5's 75.4% and Sol's 82.1%. For a model whose price implies a text-and-code workhorse, the multimodal lead is quietly one of the strongest parts of the table.

## 7. Science workflows: BioMysteryBench and LABBench2

BioMysteryBench splits its bioinformatics tasks by human difficulty, and the hard half is where 3.8 Flash makes its biggest relative move: **56.5%** against Opus 5's 49.4%, Sol's 44.7%, and 3.7 Flash's 43.5%. On the human-solvable half, Opus 5 stays ahead at 90.1% to 3.8 Flash's 88.8%. On LABBench2, real-world biology research tasks, 3.8 Flash posts **86.2%**, the best figure published, with Opus 5 at 84.2%.

## 8. The second door: Gemini 3.8 Flash Cyber

3.8 Flash Cyber is built on the same foundational intelligence, tuned for cybersecurity, and gated to vetted defenders: government authorities, critical infrastructure operators, and software maintainers, through the Fairwind Program. Google bundles it with the CodeMender harness for finding, verifying, and fixing vulnerabilities at agentic scale, per Google's companion post by Four Flynn. Google says it prioritized defensive capability like vulnerability fixing over offensive capability like exploitation.

On CyberGym, the standard industry benchmark for autonomous vulnerability discovery, 3.8 Flash Cyber posts **86.2%** pass@1, ahead of GPT-5.5 Cyber at 85.6%, Anthropic's unrestricted-tier Mythos 5 at 83.8%, GPT-5.6 Sol at 83.6%, and predecessor 3.5 Flash Cyber at 77.5%, per Google's cyber methodology page.

CyberGym draws on C and C++ codebases, so Google also ran an internal benchmark spanning 20 programming languages to approximate real defensive work. There, 3.8 Flash Cyber succeeds on **71.0%** of tasks, against 58.9% for 3.7 Flash and 46.6% for 3.5 Flash Cyber.

On patching, the external measure is CWE-Bench, run by Collinear AI. Google reports 3.8 Flash Cyber at **47.2% pass@1** against a leading frontier model at 47.8%, and calls the model Pareto-optimal: on Google's chart the frontier model's point sits at roughly three times 3.8 Flash Cyber's cost per rollout. Google doesn't list a public price for 3.8 Flash Cyber access, but the Flash sticker and the Fairwind framing make the Pareto argument for them.

## 9. Safety posture: Gray Swan, CBRN safeguards, and a gated twin

The quiet headline is prompt-injection resistance. Gray Swan's benchmark measures attack success rate within a number of attempts, lower is better, and both 3.8 variants land at the top of Google's chart: **5.5%** for 3.8 Flash and 6.0% for 3.8 Flash Cyber, against Opus 5's 4.8%, Fable 5's 6.5%, and 3.7 Flash's 9.2%. The frontier models that dominate capability leaderboards sit far down the chart: GPT-5.6 Sol at 27.0%, GPT-5.6 Luna at 50.0%, DeepSeek V4 Pro at 60.1%.

On misuse, 3.8 Flash ships with safeguards against chemical, biological, radiological, and nuclear harms and cyber offense under Google's Frontier Safety Framework. 3.8 Flash Cyber carries a deliberately more permissive set of cyber mitigations, which is exactly why access runs through Fairwind vetting rather than a checkout page.

## What it is already patching inside Google

Google isn't presenting the Cyber model as a benchmark exercise. The Chrome Security team reports 2.6 times more correct patches from 3.8 Flash Cyber than from the best commercial models it compared against, models Google describes as much larger. Wiz measured 7.5 to 9.7 points higher recall on its internal penetration-testing benchmark at 2.3 to 5.2 times lower cost. And Google's Cloud Vulnerability Research team used it to find a critical foundational vulnerability in under two hours, a discovery Google says usually takes months. The Fairwind roster already includes Armadin, Palo Alto Networks, Snowflake, and Wiz.

## What the early reads say

Artificial Analysis clocked 3.8 Flash at about 305 output tokens per second, the fastest output speed it has measured, and 59 on its Intelligence Index at high effort. AI Release Tracker counts it as the fourth Flash-tier upgrade in four months, the one aimed at coding, the area where the line had trailed Anthropic and OpenAI. And Fello AI makes the sharpest contextual point: three Flash releases landed in 43 days while Gemini 3.5 Pro, announced in May 2026 with a promised rollout the following month, still doesn't exist. Google's fast tier is where its shipping energy lives right now.

## Takeaways

- The Flash tier is now frontier-adjacent on shaped work at 1/6.7th Opus 5's price. Best-in-table on Vals Finance (61.4%), Harvey's Legal (10.0%), Terminal-bench 2.1 (89.4%), CharXiv (86.2%), LVBench, HLE-Verified (54.9%), and LABBench2 (86.2%).
- The ceiling is equally real. Terminal-bench 4.0 goes 51.8% to 19.1%, OSWorld-2.0 goes 75.4% to 59.0%, and GDPVal-AA v2 goes 1824 to 1545 in Opus 5's favor. Long-horizon general agency still has a tier above Flash.
- The meter runs longer even when the rate doesn't move: roughly 30% more output tokens per task per Emergent's read, plus an effort dial, plus an introductory price that becomes $1.50/$7.50 on January 1, 2027. Model the token burn, then the sticker.
- The Cyber variant is the strategically interesting door: CyberGym 86.2% beats GPT-5.5 Cyber and Anthropic's unrestricted-tier Mythos 5, a 71% success rate across 20 languages, and Pareto pricing on CWE-Bench, all gated to vetted defenders through Fairwind.
- Google's spiciest claim, that cybersecurity training drove the general coding and reasoning gains, at least rhymes with the evidence: the biggest 3.8-over-3.7 jumps are in adversarial, multi-step domains, and prompt-injection success fell from 9.2% to 5.5%.
- Cadence is the strategy. Three Flash releases in 43 days is Google telling you where its deployment-tier energy is going.

If you want this model pointed at your actual work instead of a leaderboard, that's the Vellum thesis: an assistant that runs the model you choose through custom LLM credentials, holds your context across every conversation, and lives where you do, on Mac, iOS, Android, web app, voice, email, Telegram, and Slack.
