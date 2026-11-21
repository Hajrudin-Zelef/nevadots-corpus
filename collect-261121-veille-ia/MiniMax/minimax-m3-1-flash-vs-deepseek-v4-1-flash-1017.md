---
id: collect-261121-veille-ia/minimax/minimax-m3-1-flash-vs-deepseek-v4-1-flash-1017
title: "MiniMax M3.1 Flash vs DeepSeek-V4.1-Flash-1017"
domain: minimax
role: reference
task: article
actors: ["MiniMax"]
dates: ["2026-10-19"]
keywords: ["tools", "minimax", "unspecified", "reasoning", "deepseek-v4", "flash-1017", "flash", "shared"]
source: https://llmlearner.com/compare/minimax-m3-1-flash-preview-vs-deepseek-v4-1-flash-1017
source_anchor: ""
source_lines: [1, 176]
section: "MiniMax"
---
Across 20 comparable benchmarks, DeepSeek-V4.1-Flash-1017 has better reported results on 19 and worse on 1. 1 further shared result is listed below, each with the reason it is not counted.

Price basis: Standard rates vs peak-hour rates. The price section below shows the applicable periods.

| Key facts for MiniMax M3.1 Flash and DeepSeek-V4.1-Flash-1017 |  |  | 
|---|---|---|
| Fact |  |  | 
|---|---|---|
|  | 1M tokens | 1M tokens | 
|  | $0.150 | $0.300 | 
|  | $0.600 | $1.20 | 
|  | $0.143 | $0.268 | 
|  | Available | Available | 
|  | October 19, 2026 | October 17, 2026 | 

Blended is 4:1 input to output with 90% cache hits at listed rates — set your own mix.

Editor’s take

LLMLearner Editorial · Figures as of · AI-assisted

1017 has the higher recorded coding-suite results here; MiniMax’s formal release supports a business-automation trial. Their task profiles matter even when the illustrative off-peak token bill ties.

**Capability evidence, checked November 3.** Vibe Code Bench v1.1 records 1017 at 87.73% and MiniMax at 73.94%, both Max with tools. Their separately reported DeepSWE v1.1 scores are 77.0% and 73.7%; DeepSeek specifies Mini-SWE-V2, so that pair should not be called a matched-harness win. On Automation-Bench, the provider records are 60.5 for 1017 with effort unspecified and 62.8 for MiniMax at Max. That small cross-configuration gap does not prove an automation winner. Vibe Code v1.1 tests web-application construction with OpenHands; it is not a generic score for every coding task.

**When I would choose each.** Start a repository-heavy trial with 1017. Include MiniMax for repeated business-system workflows, supported by its final-release BankerToolBench report of 76.2 and its low flat tariff. Test the actual chain of tools and documents. MiniMax identifies Space Bunny as an early version, so anonymous preview scores cannot silently become final-release scores; the retained preview URL is a link-preservation decision, not a release-status claim.

**Same workload, explicit assumptions.** For 100,000 uncached input and 10,000 output tokens, MiniMax’s published flat tariff gives $0.021. DeepSeek 1017 costs $0.042 peak or $0.021 off-peak. Equal token bills off-peak do not mean equal completed-task cost. Cache-hit rates differ, and tool fees are additional.

deepseek-v4-1-flash-1017 pricing. minimax-m3-1-flash-preview pricing. The example uses zero cache hits; the page’s default 4:1 blend uses 80% cache hits and answers a different question.

Mean of 11 shared benchmarks grouped by capability on a 0–100 scale. How averages are built

No shared comparable results for Abstract reasoning, Mathematics, Science & health, Research & search, Long context, Cybersecurity.

| Shared benchmark comparison between MiniMax M3.1 Flash and DeepSeek-V4.1-Flash-1017; the gap column measures MiniMax M3.1 Flash against DeepSeek-V4.1-Flash-1017, in percentage points where the metric runs to 100 and as a multiple otherwise, with each model’s rank on the full board beneath. |  |  |  | 
|---|---|---|---|
| Benchmark |  |  | Gap | 
|---|---|---|---|
| DeepSWERepository Engineering · Pass@1 (DeepSWE v1.1) | 73.7Max · tools | 77.0≠Unspecified Reasoning · tools | −3.3 pts#15 vs #4 of 70 | 
| FrontierCS 2Repository Engineering · FCI | 3.3Unspecified Reasoning · tools· fci | 25.8Unspecified Reasoning · tools· fci | 0.13×#21 vs #15 of 21 | 
| NL2Repo-BenchRepository Engineering · Average test pass rate | 61.2Unspecified Reasoning · tools | 68.2Unspecified Reasoning · tools | −7.0 pts#7 vs #2 of 21 | 
| SWE-sweepRepository Engineering · Bugs resolved (%) | 0.67Max · tools | 1.7Max · tools | −1.0 pts#17 vs #13 of 20 | 
| Terminal-Bench 4.0Agentic Development · Resolution rate (%) | 38.1Unspecified Reasoning · tools | 42.3Unspecified Reasoning · tools | −4.2 pts#30 vs #25 of 90 | 
| Vibe Code Bench v1.1Code Generation & Editing · Pass rate (%) | 73.9Max · tools | 87.7Max · tools | −13.8 pts#45 vs #15 of 119 | 
| Agents' Last Exam v1.0Tool Orchestration · Pass rate | 29.6Thinking High · tools· pass-rate | 35.4≠Unspecified Reasoning · tools· pass-rate | −5.8 pts#15 vs #6 of 47 | 
| Agents' Last Exam v1.1Tool Orchestration · Pass rate | 29.5Max · tools· pass-rate | 36.1≠Thinking High · tools· pass-rate | −6.6 pts#17 vs #14 of 21 | 
| AutomationBench-AATool Orchestration · Guardrail-safe objective completion rate | 72.5Thinking · tools | 66.6≠Max · tools | +5.9 pts#8 vs #12 of 15 | 
| BabyVisionVisual Understanding · Accuracy | 91.0Unspecified Reasoning · tools | 92.5Unspecified Reasoning · tools | −1.5 pts#9 vs #8 of 17 | 
| CharXiv RQDocuments & Charts · Score | 82.5Unspecified Reasoning · tools | 85.6≠Max · tools | 0.96×#17 vs #14 of 21 | 
| MMMU-ProVisual Understanding · Accuracy | 79.1Thinking · no tools | 82.5≠Max · tools | −3.4 pts#37 vs #18 of 134 | 
| AutomationBenchOffice & Business · Pass RateNot counted · Different or unrecorded test environments — excluded from the tally | 62.8Max · tools· zapier-public-600 | 60.5≠Unspecified Reasoning · tools· legacy-default | —#1 vs #2 of 64 | 
| GDPxCross-industry Work · GDPx — Score | 4.0Highest · source-reported) (with tools· overall/score | 5.4Highest · source-reported) (with tools· overall/score | 0.74×#14 vs #11 of 20 | 
| Legal Research BenchLegal · Score | 34.1Max · tools | 45.2Max · tools | 0.76×#51 vs #21 of 91 | 
| Tax Agent BenchFinance · Accuracy (%, rubric × citation quality) | 32.1Unspecified Reasoning · tools | 44.4≠Max · tools | −12.2 pts#35 vs #26 of 74 | 
| HLEKnowledge Exams · Accuracy | 48.0Reported best | 58.8≠Max · tools | −10.8 pts#58 vs #17 of 151 | 
| AA Intelligence Index v4.3Capability Indices · Index (0-100) | 42.4Reported best | 45.1≠Max · task tools | 0.94×#14 vs #13 of 16 | 
| AA Intelligence Index v5.0.1Capability Indices · Index (0-100) | 29.1Max · task tools | 32.6Max · task tools | 0.89×#33 vs #25 of 61 | 
| LiveBench 2.0Cross-capability Suites · Overall score (equal-weight mean of 7 suite scores, 0-100) | 73.1Max · task tools | 79.2Max · task tools | 0.92×#22 vs #10 of 51 | 
| Vals Index v2.2Capability Indices · GDP-weighted accuracy (%) | 53.8Max · task tools | 61.3Max · task tools | −7.5 pts#24 vs #15 of 34 | 

Repository Engineering · Pass@1 (DeepSWE v1.1)

Repository Engineering · FCI

Repository Engineering · Average test pass rate

Repository Engineering · Bugs resolved (%)

Agentic Development · Resolution rate (%)

Code Generation & Editing · Pass rate (%)

Tool Orchestration · Pass rate

Visual Understanding · Accuracy

Documents & Charts · Score

Office & Business · Pass Rate

Cross-industry Work · GDPx — Score

Legal · Score

Finance · Accuracy (%, rubric × citation quality)

Knowledge Exams · Accuracy

Capability Indices · Index (0-100)

Capability Indices · GDP-weighted accuracy (%)

A dash means no result is available.

Time-of-day providers use peak-hour rates; fixed-price providers use standard rates. Off-peak discounts remain in the published details.

Drag the handle above the bar for cache hits, the one below it for the input : output mix · 5% steps

Blended price = effective input rate at 90% cache hits × 80% + output rate × 20%

MiniMax M3.1 Flash costs47%lessthan DeepSeek-V4.1-Flash-1017 per million tokens.Using standard rates vs peak-hour rates at this token mix.

MiniMax M3.1 Flash: Standard pricing. MiniMax international platform (platform.minimax.io) pay-as-you-go list price, flat across context lengths. Official independent currency quote; no conversion.

DeepSeek-V4.1-Flash-1017: Peak-hour pricing. DeepSeek official USD pricing (peak: 01:00–04:00 and 06:00–10:00 UTC Mon–Fri exc. CN holidays) DeepSeek official USD pricing (peak output)

| Standard rates vs peak-hour rates: API price comparison between MiniMax M3.1 Flash and DeepSeek-V4.1-Flash-1017 in US dollars per million tokens |  |  |  | 
|---|---|---|---|
| Price / 1M tokens | MiniMax M3.1 Flash | DeepSeek-V4.1-Flash-1017 | Ratio | 
|---|---|---|---|
| Input | $0.15 | $0.30 | 0.50× | 
| Output | $0.60 | $1.20 | 0.50× | 
| Cached input | $0.015 | $0.006 | 2.50× | 
| Cache write | — | — | — | 
| Blended | $0.143 | $0.268 | 0.53× | 

Estimates use the selected token mix. Cache writes, tool fees and long-context surcharges are excluded.

Reported cost per task, paired with the score from the same run. Settings and sources may differ; costs do not affect the verdict.

| Reported benchmark task costs and scores for MiniMax M3.1 Flash and DeepSeek-V4.1-Flash-1017 |  |  | 
|---|---|---|
| Benchmark | MiniMax M3.1 Flash | DeepSeek-V4.1-Flash-1017 | 
|---|---|---|
| AA Intelligence Index v4.3 | Not reported |  | 
| AA-Briefcase | Not reported |  | 
| AA-LCR | Not reported |  | 
| AA-Omniscience | Not reported |  | 
| Agent Arena | Not reported |  | 
| Agent Arena Index | Not reported |  | 
| Agents' Last Exam v1.0 |  | Not reported | 
| ARC-AGI-1 | Not reported |  | 
| ARC-AGI-2 | Not reported |  | 
| Code Migration | Not reported |  | 
| CritPt | Not reported |  | 
| CursorBench 4.0 | Not reported | $0.77 / task Average · Task score: 45.6 Thinking High (With Tools) · cursorbench-4-0-cursor-agent $0.60 / task Average · Task score: 44.9 Thinking Low (With Tools) · cursorbench-4-0-cursor-agent | 
| EMB (Excel Modeling) | Not reported |  | 
| Finance Agent v2 | Not reported |  | 
| GDP.pdf | Not reported |  | 
| GDPval-AA v2 | Not reported |  | 
| Harvey's Legal Agent Benchmark | Not reported |  | 
| SciCode | Not reported |  | 
| Terminal-Bench 4.1 | Not reported |  | 
| Vals Index v2.1 | Not reported |  | 
| WeirdML v3 | Not reported |  | 

| Specification comparison between MiniMax M3.1 Flash and DeepSeek-V4.1-Flash-1017 |  |  | 
|---|---|---|
| Attribute | MiniMax M3.1 Flash | DeepSeek-V4.1-Flash-1017 | 
|---|---|---|
|  | Available | Available | 
|  | MiniMaxAI | DeepSeek-AI | 
|  | Coding model | Multimodal model | 
|  | October 19, 2026 | October 17, 2026 | 
|  | 1M tokens | 1M tokens | 
|  | — | 384K tokens | 
|  | 428B | 552B | 
|  | 23B | 16B | 
|  | Mixture of Experts (MoE) | Mixture of Experts (MoE) | 
|  | Reasoning, Coding | Reasoning, Multilingual, Coding, Function calling, Tool use, Structured output, Long context | 
|  | Text, Image, Video | Text, Image | 
|  | Text | Text | 
|  | Open weights | Open weights | 
|  | MiniMax M3.1-Flash Community License | MIT License | 
|  | Proprietary | Commercial use allowed | 

These are reported-score comparisons, not controlled head-to-head tests.

What people found comparing these models, or using either one. Comments also appear on the models' own pages.

Used MiniMax M3.1 Flash or DeepSeek-V4.1-Flash-1017? Sign in with Google to share what worked, what did not, and which API or setup you tested.

No community comments on either model yet.

Alternatives with the most benchmark overlap for this matchup.
