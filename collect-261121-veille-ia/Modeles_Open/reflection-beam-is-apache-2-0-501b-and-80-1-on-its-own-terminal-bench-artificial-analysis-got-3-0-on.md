---
id: collect-261121-veille-ia/modeles-open/reflection-beam-is-apache-2-0-501b-and-80-1-on-its-own-terminal-bench-artificial-analysis-got-3-0-on
title: "Reflection Beam Is Apache 2.0, 501B, and 80.1% on Its Own Terminal-Bench. Artificial Analysis Got 3.0% on the Newer One."
domain: modeles-open
role: reference
task: article
actors: []
dates: ["2026-10-16"]
keywords: ["reflection", "beam", "open", "index", "model", "muse", "spark", "apache"]
source: https://www.llmmatchmaker.com/blog/reflection-beam
source_anchor: ""
source_lines: [1, 101]
section: "Modeles_Open"
---
Yesterday we wrote that Meta's Muse Spark Open was the most capable US open-weight model and that Reflection AI's Beam, the only other US open model in Meta's comparison table, had not shipped weights. Today, October 16, 2026, it shipped them, and the line in our post is now wrong. Here is the correction and the post that goes with it.

Beam is 501B total parameters with 23B active, a 1M-token context, and a license that needs no footnote: Apache 2.0, ungated, with BF16, FP8, and NVFP4 builds and a speculative drafter on Hugging Face. Muse Spark Open carries an attribution requirement and a $20 million revenue clause; Beam carries nothing. On the question of which big US model is actually open, Beam wins on the day it ships.

On the question of which is better, the independent numbers are not close. Artificial Analysis scores Beam at 30 on its Intelligence Index against Muse Spark Open's 39, and its own Terminal-Bench 4.0 run gives Beam 3.0%. Reflection's launch post prints 80.1% on Terminal Bench, version 2.1, a different and easier test. We read Reflection's post, the Hugging Face card, AA's model page and launch article, and Vercel's Gateway listings. Here is what held up.

## What You Are Downloading

|  | Reflection Beam 1.0 | 
|---|---|
| **Released** | October 16, 2026 | 
| **Weights** | `reflection/Beam-1.0-501B-A23B` on Hugging Face, plus FP8, NVFP4, and DFlash2 drafter repos; ungated; repo created October 7 | 
| **License** | Apache 2.0 | 
| **Architecture** | 501.3B total / 23.4B active; 52 layers (3 dense, 49 MoE); 256 experts; 152K vocabulary; modified Harmony chat template | 
| **Context** | 1M tokens; text only | 
| **Knowledge cutoff** | June 30, 2026 | 
| **Tooling** | Mirror CLI (Python front end, Rust core); tested with Pi, OpenCode, and Hermes; 40+ launch partners; community GGUF from unsloth | 
| **AA Intelligence Index** | 30; #20 of 121 in its class | 

**The license is the news.** Apache 2.0 with no gate is the most permissive terms any US lab has attached to a model this size. If your legal team has been reading the Muse Model License, this is the one they will prefer, and that is Beam's strongest argument.

**501B is not a laptop model.** Even the NVFP4 build needs datacenter-class memory. Reflection does not state a hardware requirement per quantization, and we will not estimate one.

## The Benchmarks, Sorted by Who Reported Them

House rules: independent first, with the source; Reflection's numbers labeled as Reflection's, in a separate table, because the two Terminal-Bench figures must never share a column.

**Artificial Analysis, Intelligence Index v4.3.2, October 16.** Beam scores 30, second among US open-weight models behind Muse Spark Open at 39, ahead of NVIDIA's Nemotron 3.5 Super VL at 26, and one point behind K2 Horizon at 31. It is frugal: about 26K output tokens per task against about 52K for K2 Horizon, and 79M tokens to run the whole index against a class median of 140M. It is also fast and cheap: 324.2 tokens per second, $0.70 in and $2.10 out per million as the median across providers, $0.57 per index task.

| Metric (Artificial Analysis, Oct 16) | Reflection Beam | 
|---|---|
| Intelligence Index | 30, #2 US open-weight (Muse Spark Open 39) | 
| Terminal-Bench 4.0 | 3.0% (Muse Spark Open 8.1%, Qwen3.8 27B 5.6%) | 
| AutomationBench-AA | 48.5% | 
| CritPt | 13.1% | 
| AA-Briefcase (Elo) | 976 | 
| AA-Omniscience | 33% correct; 47% hallucination rate (K2 Horizon 26%) | 
| Cyber Index | 34%, leading US open model | 
| Coding Agent Index, in Mirror | 28 (leaders 68) | 
| Output tokens per task | about 26K (K2 Horizon about 52K) | 
| Speed and cost | 324.2 tokens/s; $0.57 per index task | 

**Now the vendor table.** These are **Reflection's self-reported numbers**, run in BF16.

| Benchmark (Reflection's table) | Beam | 
|---|---|
| Terminal Bench v2.1 | 80.1% | 
| SWE-bench Verified | 81.6% | 
| SWE-Bench Pro v1 / v2-Hard | 66.1% / 76.5% | 
| DeepSWE v1.1 | 43.3% | 
| AIME 2026 | 97.7% | 
| GPQA Diamond | 90.6% | 
| Humanity's Last Exam, no tools / with tools | 36.0% / 51.1% | 
| MCP Atlas | 80.8% | 
| AA-LCR | 78.2% | 
| AutomationBench, public split | 39.3% | 

Three honest observations:

- **Two Terminal-Benches, one name.** Reflection's 80.1% is version 2.1. AA's 3.0% is version 4.0, a harder test that AA runs itself. Both are real; neither predicts the other, and a reader who sees "Terminal-Bench 80%" next to this model has been shown the wrong one.
- **The hallucination rate is the catch for knowledge work.** 47% on AA-Omniscience, against 26% for K2 Horizon, means that when Beam does not know, it tends to answer anyway. Retrieval and checks are not optional.
- **Token frugality is the independent check on a vendor claim.** Reflection says "about 3x fewer reasoning tokens." AA measured about 2x against K2 Horizon. The direction holds; the multiple is smaller.

## Where You Can Run It, and What It Costs

| Provider (Vercel AI Gateway, per 1M tokens) | Input | Output | 
|---|---|---|
| Reflection (free listing; end date not stated) | $0 | $0 | 
| Nebius | $0.25 | $1.00 | 
| Together AI | $0.40 | $1.20 | 
| Baseten | $1.30 | $4.30 | 

OpenCode made Beam free for a limited time today. The Gateway's model description says 262,144 tokens of context while the endpoint listings say 1M; we flag the discrepancy and print the Hugging Face card's 1M. Reflection's own platform pricing page did not render for us, so first-party paid rates are unverified.

## Verified vs Unconfirmed: The Scorecard

| Claim | Verdict | 
|---|---|
| Released Oct 16; Apache 2.0; ungated; 501B / 23B active; 1M context; June 30, 2026 cutoff | **Verified** (Reflection post, Hugging Face card) | 
| AA Intelligence Index 30; #2 US open-weight; 3.0% on Terminal-Bench 4.0; 47% hallucination rate | **Verified** (Artificial Analysis, Oct 16) | 
| Terminal Bench 80.1% | **Verified as Reflection's number on version 2.1** ; not comparable to AA's 4.0 figure | 
| "About 3x fewer reasoning tokens" | **Vendor claim** ; AA measured about 2x vs K2 Horizon | 
| Free on Reflection's Gateway listing and on OpenCode | **Verified** ; end dates**Unverified** | 
| Context: 1M vs 262,144 | **Contradicted** between the Gateway description and the endpoint data; card says 1M | 
| Reflection first-party pricing; hardware per quantization | **Unverified** (not published or not readable) | 

## Who Should Use It

**Use Beam** if your legal requirement is a permissive license at scale and your workload is agentic or tool-driven rather than fact-driven; AA's AutomationBench and Cyber Index placements are respectable, the token appetite is the lowest in its class, and it is free to try on OpenCode and Reflection's listing right now, then $0.25 per million input on Nebius.

**Skip it** for factual question-answering or research without retrieval, where a 47% hallucination rate will find you, and for terminal-heavy coding agents, where AA's 3.0% is the number that counts. If you were choosing between Beam and Muse Spark Open on capability alone, Muse Spark Open is nine points ahead; if you were choosing on terms, there is no contest.

**The frontier is elsewhere:** among US open weights the order today is Muse Spark Open at 39, Beam at 30, Nemotron 3.5 Super VL at 26, all well under the Chinese open models and the closed flagships above them. What Beam changes is the license floor, not the score ceiling. Our model comparison has the current standings side by side.

## Update, October 29, 2026

- **The index changed scale.** Artificial Analysis replaced Index v4.3.2 with Intelligence Index v5 on October 27. Every Artificial Analysis figure in this post is a v4.3.2 figure on the old scale. On v5, released open weights top out around 32 (DeepSeek V4.1 Flash 1017 at max and GLM-5.3 at max) against closed leaders in the high 50s, and 15% of the score is a private Cognition benchmark. Beam's own v5 row was not in AA's launch article; the 30 and the US open-weight ranking above are v4.3.2, and should be read from the live v5 table before being reprinted.

## Sources

*Not sure whether a permissive license or a higher score matters more for what you are building? Take the free 2-minute quiz and get matched. Then read about the model Beam is measured against, and the license it does not have: Muse Spark Open.*
