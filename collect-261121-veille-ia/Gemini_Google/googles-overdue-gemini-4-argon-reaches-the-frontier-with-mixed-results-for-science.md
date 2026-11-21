---
id: collect-261121-veille-ia/gemini-google/googles-overdue-gemini-4-argon-reaches-the-frontier-with-mixed-results-for-science
title: "Google’s overdue Gemini 4 Argon reaches the frontier with mixed results for science"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-10-01"]
keywords: ["gemini", "google", "argon", "coding", "flash", "frontier", "googles", "results"]
source: https://www.rdworldonline.com/googles-overdue-gemini-4-argon-reaches-the-frontier-with-mixed-results-for-science/
source_anchor: ""
source_lines: [1, 68]
section: "Gemini_Google"
---
*Update, Oct. 6: Argon remains limited to Fairwind participants and does not yet appear in the Gemini API’s public model list as of October 6. Google has not given a date for wider release.*

Google is rolling Argon out first to cybersecurity defenders in Fairwind, a program it opened Sept. 2 alongside 3.8 Flash Cyber for governments, national cyber authorities, critical infrastructure operators and core technology platforms. Google says the paid Gemini API and Google AI Ultra come next. It has not yet provided a date.

The continued rapid pace of frontier model development appears to have caught Google by surprise. At its I/O developer conference in May, Google promised Gemini 3.5 Pro within a month. But it was delayed, and then delayed some more, and was apparently scrapped for a new model. Bloomberg reported in July that 3.5 Pro was months behind schedule, in part because its coding performance fell short of internal goals.

The months in between belonged to Google’s less expensive Flash tier. Gemini 3.5 Flash arrived at I/O, followed by 3.6 Flash on July 21, 3.7 Flash on Aug. 13 and 3.8 Flash on Sept. 2.

While Gemini 2.5 Pro helped reshape the AI race in March 2025, Google has broadly trailed in AI coding since then. Menlo Ventures, an Anthropic investor, estimated last December that Anthropic held 54% of enterprise spending on coding models, versus 21% for OpenAI and 11% for Google. While OpenAI has made gains in coding traction since, Gemini largely hasn’t. In September, Google opened Anthropic’s Claude Opus 5 to its engineers through its Antigravity coding environment, while saying Gemini remains its primary model for internal development. Some Google employees questioned Argon’s performance on practical coding tasks, while others believed it had caught up with leading rivals, according to Bloomberg.

Google disputed the characterization. The company claims that Gemini 4 Argon leads the frontier on a majority of benchmarks. Independent testing shows competitive, if not chart-topping, performance.

How Gemini 4 Argon compares

Results from Google, Artificial Analysis and Arena as of Oct. 1, 2026. Best result in each row.

| Category | Benchmark | Gemini 4 Argon | GPT-6 Astra | Claude Fable 5.1 | Claude Opus 5.5 | 
|---|---|---|---|---|---|
| Google’s reported results (methodology) |  |  |  |  |  | 
| Knowledge work | Vals Index | 68.9% | 63.1% | 65.8% | 67.0% | 
|  | AutomationBenchScore | 51.3% | 41.4% | 31.4% | 42.5% | 
|  | Vals Finance Agent v2 | 65.4% | 53.5% | 58.9% | 58.6% | 
|  | Harvey’s Legal Agent Benchmark | 19.6% | 5.4% | 6.7% | 3.8% | 
| Agentic coding | DeepSWE v1.1 | 77.9% | 74.1% | 67.4% | 74.2% | 
|  | FrontierSWE v2 | 55.0% | 65.5% | 56.3% | 62.3% | 
|  | Vibe Code Bench | 91.9% | 89.6% | 90.3% | 90.3% | 
|  | Terminal-Bench 4.0 | 57.4% | 58.2% | 57.9% | 66.4% | 
| ML engineering | PostTrainBench | 45.3% | 44.3% | 40.2% | 49.3% | 
| Science and math | Terminal-Bench Science 0.1 | 57.6% | 68.1% | 52.6% | 63.3% | 
|  | LABBench2 | 88.8% | 85.4% | 68.6% | 73.1% | 
|  | RiemannBench | 76.0% | 72.0% | 65.6% | 69.6% | 
| Long context | GraphWalksUp to 128K, BFS (F1) | 99.7% | 98.7% | 91.4% | 90.6% | 
|  | GraphWalks256K to 1M, BFS (F1) | 84.2% | 71.8% | 65.0% | 66.8% | 
| Computer use | Agent’s Last ExamPass rate | 39.5% | 34.2% | — | 38.2% | 
|  | OSWorld-2.0Offline subset, partial score | 69.2% | 72.6% | — | — | 
| Multimodal | Chartography | 71.6% | 71.0% | 46.2% | 66.3% | 
|  | LVBench | 91.7% | 87.5% | 79.7% | 83.7% | 
| Cybersecurity | CWE-bench v1 | 68.0% | 68.0% | 58.0% | 67.0% | 
| Independent: Artificial Analysis (analysis) |  |  |  |  |  | 
| Overall | Intelligence IndexScore; Argon at highest reasoning setting | 53 | 53 | 53 | 58 | 
|  | Cost per index taskArgon at introductory price; $3.98 at standard price | $1.99 | $3.26 | — | — | 
|  | Output tokens per index taskLower is more efficient | ~62,000 | ~27,000 | — | — | 
| Independent: Arena (Agent Arena leaderboard) |  |  |  |  |  | 
| Agents | Agent Arena overall rankArgon: 3,417 sessions; rank range 3rd–16th | 8th | — | — | — | 

# **What independent tests show**

Artificial Analysis provides independent support for Google’s progress. Argon, using its highest reasoning setting, scores 53 on the firm’s Intelligence Index, matching the reported scores of Astra and Claude Fable 5.1. That is 23 points above Gemini 3.1 Pro Preview, the last Pro model Google shipped. The current index chart places Claude Opus 5.5, tested at maximum reasoning with fallback, at 58.

Arena’s Agent Arena leaderboard offers a second view, focused on tool-using performance. Argon ranked eighth overall when checked Oct. 1, based on 3,417 sessions. Its reported rank range stretched from third to 16th.

# **The science results split**

In Google’s comparison, Argon scores 88.8% on LABBench2, ahead of Astra at 85.4% and Opus 5.5 at 73.1%. On Terminal-Bench Science 0.1, Argon’s 57.6% trails Astra’s 68.1% and Opus 5.5’s 63.3%. Argon beats Claude Fable 5.1 on both.

LABBench2, the benchmark from Edison Scientific, tests tasks such as finding evidence in scientific literature, working with biological sequences and troubleshooting experimental protocols. Its scores speak to capabilities a biologist might use while preparing or interpreting an experiment.

Terminal-Bench Science asks agents to complete 70 bounded research workflows through a computer terminal, producing outputs that can be checked. It probes whether a system can carry a computational assignment through to a verifiable result.

Google’s methodology says it ran all four models on LABBench2 with terminal access, scientific software and the internet. For Terminal-Bench Science, Google tested Argon itself and took the competing scores from the benchmark’s leaderboard.

# **Frontier performance for a fraction of the token price**

Google set introductory API prices of $2 per million input tokens and $10 per million output tokens, half its standard rates. Argon can also generate up to 1 million output tokens in a response, up from a 64,000-token ceiling on earlier Gemini releases, which leaves room for the long, multi-step runs that research agents require.

Artificial Analysis found that Argon consumed about 62,000 output tokens per index task, versus Astra’s 27,000. Its lower introductory prices nevertheless brought the measured cost to $1.99 per task, against Astra’s $3.26. At standard prices, Argon’s cost would rise to $3.98. Google’s 50% promotion therefore changes which model is cheaper on this particular workload; its end date has not been confirmed.

Arena also placed Argon on its cost-performance frontier, identifying a competitive trade-off at launch prices.
