---
id: collect-261121-veille-ia/xiaomi-mimo/hy4-preview-vs-mimo-v2-6-pro-every-known-benchmark-and-price
title: "Hy4 Preview vs MiMo V2.6 Pro: Every Known Benchmark and Price"
domain: xiaomi-mimo
role: reference
task: article
actors: ["Xiaomi"]
dates: ["2026-08-28"]
keywords: ["mimo", "preview", "both", "benchmark", "tencent", "xiaomi", "weights", "score"]
source: https://www.intelligentliving.co/hy4-preview-vs-mimo-v2-6-pro/
source_anchor: ""
source_lines: [1, 95]
section: "Xiaomi_MiMo"
---
Two open-weight flagships landed within four weeks of each other, and they could not look less alike on paper. Tencent released **Hy4 preview** on August 28, 2026, a 770-billion-parameter text-only mixture-of-experts model. Xiaomi released **MiMo V2.6 Pro** on September 21, 2026, a roughly 1-trillion-parameter multimodal model that now holds the top score among open-weight models on Artificial Analysis’s independent Intelligence Index.

Both claim frontier capability. Both ship open weights under permissive licenses. One of them costs less than half as much per token, and only one of them has an independent scoreboard entry so far. Here is the full benchmark-by-benchmark comparison of Hy4 Preview vs. MiMo V2.6 Pro, plus what the two pricing sheets mean for real workloads.

The specification gap is smaller than the release gap suggests.

Both models activate fewer than 50 billion parameters per token, both offer a 1-million-token context window, and both publish weights that anyone can download and self-host. The differences sit in modality, license, and the sheer size of the footprint required to run them.

| Specification | Tencent Hy4 preview | Xiaomi MiMo V2.6 Pro | 
|---|---|---|
| Released | August 28, 2026 | September 21, 2026 | 
| Total parameters | 770 billion | About 1.02 trillion | 
| Active parameters per token | 49 billion | 42 billion | 
| Context window | 1 million tokens | 1 million tokens | 
| Input modalities | Text only | Text, image, audio, video | 
| License | Apache 2.0 | MIT | 
| Weights published | BF16 (~1.56TB) and FP8 (~753GB) | Weights, technical report, RL code and environments | 
| Serving support | vLLM and SGLang day zero | Xiaomi API, OpenRouter, self-hosting | 
| Independent Intelligence Index score | Not yet ranked | 46 (Artificial Analysis v4.3.2) | 

Two consequences follow from that table. First, Hy4 Preview is a text-in, text-out system, while MiMo V2.6 Pro accepts images, audio, and video, which matters for anything involving screenshots, documents, or media. Second, self-hosting Hy4 preview means fitting more than 753GB of weights even in FP8, an 8-GPU node at minimum, while MiMo V2.6 Pro’s open-weights release comes with the training code and reinforcement-learning environments attached, a level of disclosure Xiaomi says remains rare at the frontier.

Comparing vendor benchmark tables across labs is always an exercise in caution. Tencent re-ran its rivals on its own evaluation harness, and Xiaomi’s numbers come from Xiaomi’s own setup. Even so, a dozen benchmarks appear in both published tables, and the overlap is large enough to read a pattern. Scores are on a 0 to 100 scale unless noted, and higher is better.

| Benchmark | Hy4 preview | MiMo V2.6 Pro | Reported edge | 
|---|---|---|---|
| CyberGym | 78.4 | **94.0** | MiMo +15.6 | 
| AutomationBench v1.0.6 | 32.1 | **53.1** | MiMo +21.0 | 
| Agents’ Last Exam | 22.8 | **31.6** | MiMo +8.8 | 
| CritPt (official) | 16.9 | **26.6** | MiMo +9.7 | 
| DeepSWE (v1.1 for MiMo) | 64.3 | **71.9** | MiMo +7.6 | 
| ProgramBench | 17.5 | **26.5** | MiMo +9.0 | 
| Terminal-Bench 2.1 | 85.4 | **89.9** | MiMo +4.5 | 
| Toolathlon-Verified | 74.1 | **76.9** | MiMo +2.8 | 
| JobBench | 61.7 | **62.0** | MiMo +0.3 | 
| GDPval-AA (Elo) | **1678** | 1673 | Hy4 +5 Elo | 
| Humanity’s Last Exam | 43.4 (no tools) | **49.4** | MiMo, see note | 

Three caveats apply before reading too much into any row. Tencent’s figures and Xiaomi’s figures were produced by different labs, so a two-point gap means very little. The DeepSWE rows are not identical tests: Xiaomi reports DeepSWE v1.1, and Tencent does not state a version. And Humanity’s Last Exam is measured differently too: Tencent publishes both a with-tools score of 55.4 and a no-tools score of 43.4, while Xiaomi’s 49.4 comes from Artificial Analysis’s independent run. On that benchmark, the honest summary is that the two models sit in the same band, with MiMo V2.6 Pro ahead of Hy4’s no-tools figure and behind its with-tools figure.

On the shared coding rows, MiMo V2.6 Pro leads every time, but the margins vary in a telling way. The closest calls are JobBench (62.0 vs 61.7) and Toolathlon-Verified (76.9 vs 74.1), while the widest gaps land on exactly the tasks that have defined 2026’s agent race: CyberGym at 94.0 vs 78.4 and AutomationBench at 53.1 vs 32.1. The public CyberGym leaderboard corroborates both numbers, placing MiMo V2.6 Pro second overall at 94.0% behind only Xiaomi’s own MiMo V2.6 Flash at 95.1%, with Hy4 preview in 17th place at 78.4%. That is one of the few places where a third-party tracker confirms both vendors’ claims simultaneously.

Hy4 preview’s strengths lie elsewhere in the coding stack. It publishes a 65.7 on SWE-bench Pro and an 82.9 on SWE-bench Multilingual, both scores Xiaomi does not report for MiMo V2.6 Pro, and Tencent’s appendix shows it holding the top open-model result on the SWE Atlas trio of codebase Q&A, test writing, and refactoring. Anyone choosing between the two on repo-level software engineering benchmarks will find Hy4 preview has published more of them, even if MiMo V2.6 Pro’s DeepSWE v1.1 result of 71.9 clears Hy4’s 64.3.

This is the one category where Hy4 preview posts a genuinely impressive standalone number: 92.3 on GPQA Diamond, a graduate-level science question set, which outside trackers place in the top third of all published models. Xiaomi does not publish a GPQA Diamond figure for MiMo V2.6 Pro, so there is no direct comparison to make.

Where the two overlap on hard reasoning, MiMo V2.6 Pro leads. CritPt goes 26.6 to 16.9, and Humanity’s Last Exam favors MiMo’s 49.4 against Hy4’s 43.4 without tools. Hy4 preview does hold the higher Elo on the GDPval-AA economic-work ratings, 1678 vs 1673, though a five-point Elo gap is inside the noise of any realistic rerun.

Xiaomi leaned hard into cybersecurity evaluations that Tencent does not run at all. Beyond the 94.0 on CyberGym, MiMo V2.6 Pro posts 47.9 on ExploitBench, 66.3 on SEC Bench Pro, and 17.8 on ExploitGym, while closing at 82.0 on OSWorld-Verified for computer-control tasks. None of those appear in Tencent’s appendix, so Hy4 preview simply has no answer on the board. Conversely, Tencent’s agentic-search block, WideSearch at 83.9, OneMillionBench at 65.4, and MCP-Atlas at 83.7, has no MiMo counterpart.

Here is the single biggest difference between the two releases, and it has nothing to do with any vendor’s table.

- **MiMo V2.6 Pro has an independent score.** Artificial Analysis tested it at 46 on the Intelligence Index v4.3.2, the highest figure among open-weight models and equal to closed-source Grok 4.7. The same independent run recorded 49.4% on Humanity’s Last Exam, 34.8% on Terminal-Bench 4.0, 60.9% on SciCode, 26.6% on CritPt, and an output speed of about 130 tokens per second.
- **Hy4 preview has no Artificial Analysis entry yet.** As of early September 2026, the industry’s closest thing to a neutral cross-lab scoreboard lists an independent evaluation as forthcoming. The interim signals are mixed: an LLM Stats composite of 51.3 puts it around 13th overall, a Vals Index assessment ranks it 21st of 58 models, and Vals independently measured Terminal-Bench 2.1 at 55.06%, far below Tencent’s reported 85.4. A LMArena Code Arena WebDev placement of roughly 5th overall, 3rd among open models, is the strongest independent corroboration Hy4 preview currently has.

The Terminal-Bench discrepancy deserves emphasis because it is the clearest case of a vendor number meeting a third-party harness. Xiaomi reports 89.9 for MiMo V2.6 Pro on Terminal-Bench 2.1; Tencent reports 85.4 for Hy4 preview; Vals measured Hy4 preview at 55.06. Until both models land on the same independent leaderboard, the shared-benchmark table above should be treated as directional evidence, not a verdict.

Pricing is where this comparison stops being close. Both labs undercut the closed frontier, but Xiaomi’s numbers are in a different league, and unlike Tencent, Xiaomi held prices flat generation over generation.

| Rate (per 1M tokens) | Hy4 preview | MiMo V2.6 Pro | MiMo advantage | 
|---|---|---|---|
| Input | $0.834 | $0.435 | About 48% cheaper | 
| Output | $2.501 | $0.87 | About 2.9x cheaper | 
| Cached input | $0.042 | $0.0036 | About 11.7x cheaper | 
| Blended (3:1 input to output) | About $1.25 | About $0.544 | About 2.3x cheaper | 
| Batch option | Not listed | Half price ($0.2175 / $0.435) | MiMo only | 
| Fast tier | Not listed | UltraSpeed: $4.35 / $8.70 | MiMo only | 

Both rate cards are confirmed by their distribution channels: OpenRouter lists Hy4 preview at the same $0.834 and $2.501 figures Tencent Cloud TokenHub charges, converted from the domestic listing of 6 yuan input and 18 yuan output per million. Xiaomi’s rates come straight from its official pricing page, unchanged from the V2.5 series.

Context matters on both sides of the ledger.

Hy4 preview costs roughly 5 to 6 times more than the Hy3 model it replaces, a jump Tencent has not explained beyond capability gains. MiMo V2.6 Pro’s predecessor, MiMo V2.5 Pro, was already cheap, and Xiaomi kept every rate identical while the Intelligence Index score jumped from 26 to 46. Artificial Analysis calculates the cost of running its entire index on MiMo V2.6 Pro at $206.66, about 13 cents per task. No equivalent figure exists for Hy4 preview because no independent index run has been published. And because Tencent’s model card admits Hy4 preview overthinks complex tasks and over-verifies its own work, real agentic costs can run above what the headline per-token price suggests, a caveat that hits output-token bills hardest.

For context on how both models sit against the rest of the field, our LLM API pricing comparison tracks the wider rate card, and our look at why Chinese AI models undercut Western APIs explains the pricing strategy behind both releases.

The evidence points in a consistent direction, with one significant asterisk.

- **Choose MiMo V2.6 Pro if price matters, or if you need an independent number.** It is cheaper on every rate, cheaper again with batch pricing and the 99% cache discount, it is the only one of the two with a verified Intelligence Index score, and it leads on 9 of the 11 benchmarks both labs publish. It also accepts image, audio, and video input, ships under the MIT license, and runs at roughly 130 tokens per second.
- **Choose Hy4 preview if your workload is repository-level software engineering or graduate-level science.** Its published SWE-bench Pro (65.7), SWE-bench Multilingual (82.9), and GPQA Diamond (92.3) results have no MiMo counterpart, its Apache 2.0 license is the least-restricted in the pair, and its 1M context matches Xiaomi’s. It also has a live LMArena placement that suggests the coding gains are real.
- **The asterisk is verification.** Every Hy4 preview benchmark row remains vendor-reported pending reproduction, while Xiaomi’s own tables face the same criticism even though Artificial Analysis independently confirmed the headline 46. Teams deciding this month should weight the independent evidence accordingly and revisit both tables when Artificial Analysis publishes its Hy4 evaluation.

One practical note regardless of choice: Xiaomi’s own early testing surfaced launch-day API errors and erratic parallel tool calls in MiMo V2.6 Pro, problems that do not show up in any benchmark because they stem from first-week capacity rather than capability. Teams deploying this week should build in retries and keep a fallback model configured, an approach that applies to any day-zero frontier release, including Hy4 preview’s own preview-status caveats.

On the benchmarks both labs publish, MiMo V2.6 Pro leads 9 of 11 rows, costs roughly half as much per input token and 2.9 times less per output token, and holds an independent Intelligence Index score of 46 that Hy4 preview has not yet been measured against. Hy4 preview answers with stronger published results on SWE-bench Pro, SWE-bench Multilingual, and GPQA Diamond, benchmarks Xiaomi does not report. The honest verdict is that MiMo V2.6 Pro has the better overall case today, while Hy4 preview’s independent evaluation is still pending.

MiMo V2.6 Pro is cheaper on every published rate: $0.435 vs $0.834 per million input tokens, $0.87 vs $2.501 per million output tokens, and $0.0036 vs $0.042 per million cached input tokens. Xiaomi also offers a half-price Batch API, which widens the gap further.

Yes. Tencent lists Hy4 preview at 1,048,576 tokens, and Xiaomi lists the same 1,048,576-token window for the full MiMo V2.6 family. The difference is modality: Hy4 preview is text-only, while MiMo V2.6 Pro accepts text, images, audio, and video as input.

Not as of early September 2026. Artificial Analysis lists an independent evaluation as forthcoming. The closest third-party signals for Hy4 preview are an LLM Stats composite of 51.3, a Vals Index rank of 21st of 58, and an LMArena Code Arena WebDev placement of roughly 5th overall.

Partially. Both the Tencent and Xiaomi comparison tables are vendor-reported. Independent confirmation exists in three places: Artificial Analysis’s 46 score and component results for MiMo V2.6 Pro, the public CyberGym leaderboard that matches both vendors’ CyberGym figures, and LMArena’s placement of Hy4 preview. Everything else, including most shared rows in this article’s table, remains a vendor claim until a neutral lab reruns it.

The headline reading of Hy4 Preview vs MiMo V2.6 Pro is that Xiaomi built the stronger all-around package: it leads on nearly every shared benchmark, it costs between 2 and 12 times less depending on the rate, it is multimodal, and it is the only one of the two carrying an independent Intelligence Index score of 46. Tencent’s model counters with the deeper published suite in software engineering and science, a genuinely permissive Apache 2.0 license, and an arena placement suggesting its coding claims will hold up. The tiebreaker is time. Artificial Analysis has Hy4 preview’s evaluation pending, both labs’ remaining tables need neutral reruns, and given how fast these two Chinese labs have been trading places on the leaderboards, this comparison may need updating before the month is out.
