---
id: collect-261121-veille-ia/deepseek/deepseek-v4-benchmarks-full-scores-and-model-guide
title: "DeepSeek V4 Benchmarks: Full Scores and Model Guide"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: ["2026-08-13"]
keywords: ["flash", "deepseek", "v4-pro", "benchmarks", "model", "scores", "benchmark", "v4-flash"]
source: https://www.layer3labs.io/guides/deepseek-v4-benchmarks
source_anchor: ""
source_lines: [1, 119]
section: "DeepSeek"
---
# DeepSeek V4 Benchmarks

DeepSeek V4.1-Flash beats the larger V4-Pro on all five shared benchmarks while trailing Google Gemini on factual recall.

The published DeepSeek V4 benchmarks show that the smaller DeepSeek-V4.1-Flash model scores higher than DeepSeek-V4-Pro across core math, coding, and terminal evaluation suites. At Layer3Labs, we build and run AI systems inside other people's businesses, and evaluation numbers on public datasets rarely match what happens inside an enterprise pipeline. Official score sheets released on model cards hosted on Hugging Face detail performance across five shared evaluations: Massive Multitask Language Understanding Pro (MMLU-Pro), HumanEval, Grade School Math 8K (GSM8K), DeepSWE v1.1, and Terminal-Bench 2.1.

The parameter counts make those results surprising. DeepSeek-V4-Pro launched in preview on 2026-04-24 with 1.6 trillion total parameters and 49 billion active parameters, followed by a general-availability update on 2026-08-13. DeepSeek-V4.1-Flash arrived on 2026-09-10 as a 552 billion parameter Mixture of Experts (MoE) architecture activating only 8 billion parameters for input and 16 billion for output. Despite running on a fraction of the active compute, V4.1-Flash posted higher numbers across all five shared tests.

The published figures also expose clear boundaries. While competitive programming and software engineering scores challenge leading closed systems, factual retrieval on SimpleQA-Verified trails Google Gemini-3.1-Pro by 17.7 points. DeepSeek also omitted direct benchmark comparisons against its prior DeepSeek-V3 generation or OpenAI's GPT-6 Astra on these identical evaluations. For engineering leads assessing the Application Programming Interface (API), public benchmark tables provide an architectural signal rather than a guarantee of workflow accuracy.

## DeepSeek V4 Benchmarks Headline Results

The headline takeaway from the published DeepSeek V4 benchmarks is that DeepSeek-V4.1-Flash outperforms the larger DeepSeek-V4-Pro across all five primary shared evaluations. On the MMLU-Pro academic reasoning benchmark, V4.1-Flash scored 74.1 compared to 73.5 for V4-Pro and 68.3 for the original V4-Flash. On HumanEval Python function generation, V4.1-Flash reached 79.4, ahead of V4-Pro at 76.8 and V4-Flash at 69.5.

The performance gap expands on agentic and software engineering tasks. On DeepSWE v1.1, which measures automated issue resolution in real software repositories, V4.1-Flash scored 74.2 while V4-Pro reached 62.7. On Terminal-Bench 2.1, which tests command-line tool execution, V4.1-Flash achieved 90.6 against 87.9 for V4-Pro and 82.7 for V4-Flash. On the GSM8K multi-step math benchmark, V4.1-Flash posted 93.0 compared to 92.6 for V4-Pro.

The economic comparison reinforces the benchmark gap. On the official DeepSeek platform, API model identifier `deepseek-flash` costs $0.30 per 1 million input cache-miss tokens during peak hours and $0.15 off-peak, with output priced at $1.20 peak and $0.60 off-peak. In contrast, `deepseek-v4-pro` bills at $1.32 peak and $0.66 off-peak for input, and $3.96 peak and $1.98 off-peak for output. Teams evaluating the family get higher benchmark marks on the model that costs roughly a quarter of the price.

- MMLU-Pro: V4.1-Flash scores 74.1, leading V4-Pro (73.5) and V4-Flash (68.3).
- HumanEval: V4.1-Flash achieves 79.4, leading V4-Pro (76.8) and V4-Flash (69.5).
- DeepSWE v1.1: V4.1-Flash leads by 11.5 points (74.2 versus 62.7 for V4-Pro).
- Terminal-Bench 2.1: V4.1-Flash scores 90.6, outpacing V4-Pro (87.9) and V4-Flash (82.7).
- GSM8K: V4.1-Flash reaches 93.0, narrowly edging out V4-Pro (92.6).

## Official Benchmark Scores for the DeepSeek V4 Family

DeepSeek published official benchmark evaluations on its model repository cards hosted on Hugging Face following the release of DeepSeek-V4.1-Flash on 2026-09-10. The release documentation organizes numbers into two categories: shared evaluations comparing the three V4 iterations, and standalone tests published exclusively for DeepSeek-V4-Pro.

The primary shared benchmark table records scores across academic knowledge, unit test synthesis, school-level mathematics, software engineering, and terminal shell interaction. DeepSeek retired the original V4-Flash and V4-Flash-Vision-Exp variants on 2026-09-10, routing those API calls into V4.1-Flash. The published scores show the generational progression between the April 2026 preview models and the September 2026 release.

Three additional academic evaluations appear only on the DeepSeek-V4-Pro model card: MMLU 5-shot at 90.1, MATH 4-shot at 64.5, and LongBench-V2 1-shot at 51.5. DeepSeek did not publish corresponding figures for V4.1-Flash on these three evaluations, leaving teams without direct comparative data for long-context retrieval or high-school mathematics competitions. Model availability remains subject to operational adjustments, as DeepSeek announced the retirement of V4-Pro on 2026-09-10 before reversing course within four days to keep the endpoint, citing user demand.

- Shared evaluation MMLU-Pro: V4.1-Flash 74.1, V4-Flash 68.3, V4-Pro 73.5.
- Shared evaluation HumanEval: V4.1-Flash 79.4, V4-Flash 69.5, V4-Pro 76.8.
- Shared evaluation GSM8K: V4.1-Flash 93.0, V4-Flash 90.8, V4-Pro 92.6.
- Shared evaluation DeepSWE v1.1: V4.1-Flash 74.2, V4-Flash 54.4, V4-Pro 62.7.
- Shared evaluation Terminal-Bench 2.1: V4.1-Flash 90.6, V4-Flash 82.7, V4-Pro 87.9.
- V4-Pro exclusive scores: MMLU (5-shot) 90.1, MATH (4-shot) 64.5, LongBench-V2 (1-shot) 51.5.

## Evaluation Metrics and Test Definitions

Standard artificial intelligence evaluations measure distinct operational competencies ranging from multiple-choice reasoning to live terminal environment execution. Understanding the mechanics behind each test clarifies what high percentages represent in daily software operations.

Academic benchmarks isolate reasoning logic from operational tool use. MMLU-Pro expands the traditional Massive Multitask Language Understanding suite by offering ten answer choices per question and focusing on complex college-level reasoning, reducing the probability of lucky guesses. GSM8K evaluates whether a language model can parse word problems into sequential multi-step arithmetic without losing track of intermediate numerical states.

Programming benchmarks verify practical implementation syntax. HumanEval tests zero-shot Python function generation against pre-written unit test assertions. DeepSWE v1.1 tests whether an autonomous coding model can process entire code bases, reproduce reported defects, edit relevant files, and pass complete test suites. Terminal-Bench 2.1 evaluates execution inside an interactive command-line interface, scoring whether an autonomous agent issues correct shell commands, inspects directory paths, and resolves terminal errors.

- MMLU-Pro: College-level academic reasoning across STEM and humanities using a ten-choice answer format.
- HumanEval: Functional Python programming accuracy evaluated against automated unit tests.
- GSM8K: Multi-step arithmetic reasoning on grade-school math word problems.
- DeepSWE v1.1: End-to-end repository defect resolution including code search, editing, and regression testing.
- Terminal-Bench 2.1: Autonomous command-line operations evaluating shell execution and environmental state validation.

## Performance Differences Between Flash and Pro Architecture

The architectural difference between DeepSeek-V4-Pro and DeepSeek-V4.1-Flash explains why the smaller model scores higher across the shared benchmark catalog. DeepSeek-V4-Pro relies on a large MoE design containing 1.6 trillion total parameters with 49 billion active per token, using token-wise compression alongside DeepSeek Sparse Attention (DSA). Expert weights are stored in 4-bit floating point (FP4) format, with other parameters held in 8-bit floating point (FP8).

DeepSeek-V4.1-Flash introduces a Causal Encoder-Decoder architecture totaling 552 billion parameters. It requires only 8 billion active parameters for processing input and 16 billion for generating output. The model incorporates Engram conditional memory, a 196 billion parameter sparse structure that recalls persistent patterns without activating the primary transformer backbone. Its MoE layers distribute computation across 384 routed experts per layer, activating six routed experts plus one shared expert per token.

This structural shift yields operational efficiency. V4.1-Flash maintains a Key-Value (KV) cache footprint of 890 bytes per token. According to DeepSeek's technical disclosures, the design requires one quarter of the High Bandwidth Memory (HBM) and one eighth of the solid-state drive (SSD) storage needed by prior generation systems. In its release documentation, DeepSeek claimed that tests by multiple parties put V4.1-Flash ahead of V4-Pro on performance, operational cost, token speed, and total runtime.

- V4-Pro parameters: 1.6 trillion total parameters with 49 billion active parameters per token.
- V4.1-Flash parameters: 552 billion total parameters with 8 billion active for input and 16 billion for output.
- Memory efficiency: V4.1-Flash uses an 890-byte KV cache per token, reducing hardware memory demands.
- Concurrency headroom: V4.1-Flash supports up to 2,500 concurrent requests on the API, compared to 500 for V4-Pro.

## Competitive Scores Against Gemini and GPT Models

DeepSeek published selective head-to-head benchmark scores comparing its flagship reasoning setting against Google Gemini-3.1-Pro and OpenAI GPT-5.4. These numbers represent DeepSeek-V4-Pro-Max, an extended reasoning configuration referred to as Think Max mode. For this mode, DeepSeek recommends a context window allocation of at least 384,000 tokens with default sampling parameters set to a temperature of 1.0 and a top_p of 1.0.

On competitive algorithmic challenges, DeepSeek reported leading marks. On LiveCodeBench, DeepSeek V4-Pro-Max scored 93.5, edging past Google Gemini-3.1-Pro High at 91.7. On Codeforces competitive programming ratings, DeepSeek V4-Pro-Max recorded an Elo rating of 3206, finishing ahead of OpenAI GPT-5.4 xHigh at 3168.

The DeepSeek V4 versus Gemini results reveal a 17.7-point deficit in factual recall. On the SimpleQA-Verified benchmark, which measures short-form factual accuracy and penalizes hallucinations, DeepSeek V4-Pro-Max scored 57.9. Google Gemini-3.1-Pro High scored 75.6 on the same test, maintaining a 17.7-point advantage. DeepSeek acknowledged in its preview materials that while V4-Pro leads current open models on general knowledge, it trails Gemini-3.1-Pro.

DeepSeek did not publish comparative benchmark scores against OpenAI's GPT-6 Astra, which launched in September 2026. GPT-6 Astra features a 1,050,000-token context window and bills at $10.00 per 1 million input tokens and $50.00 per 1 million output tokens. DeepSeek has not published direct benchmark comparisons between DeepSeek V4 and GPT-6 Astra.

- LiveCodeBench: DeepSeek V4-Pro-Max scores 93.5 versus 91.7 for Gemini-3.1-Pro High.
- Codeforces rating: DeepSeek V4-Pro-Max achieves 3206 versus 3168 for GPT-5.4 xHigh.
- SimpleQA-Verified: DeepSeek V4-Pro-Max scores 57.9, trailing Gemini-3.1-Pro High (75.6) by 17.7 points.
- Unpublished comparisons: No official scores exist comparing DeepSeek V4 against GPT-6 Astra, Claude Opus 4.8, or DeepSeek-V3.

## What DeepSeek V4 Benchmarks Omit for Enterprise Production

Public artificial intelligence benchmarks measure narrow academic capability on static datasets and fail to capture production variables like latency stability, context contamination, and structured schema adherence. Evaluation numbers on public datasets rarely match what happens inside an enterprise pipeline.

The 17.7-point gap on SimpleQA-Verified illustrates the primary operational vulnerability. Workflows in legal operations, insurance claims, and financial accounting demand factual precision. When an enterprise model scores 57.9 on factual verification, unassisted prompt completions carry high hallucination risks unless grounded by rigorous retrieval-augmented generation pipelines.

Published benchmarks also omit infrastructure stability metrics. DeepSeek does not publish guaranteed requests-per-minute or tokens-per-minute rate limits. Instead, the provider publishes only concurrency ceilings: 2,500 simultaneous requests for `deepseek-flash` and 500 for `deepseek-v4-pro`. Furthermore, pricing swings between peak and off-peak billing windows. Peak hours run from 01:00 to 04:00 and 06:00 to 10:00 UTC, Monday through Friday. Running batch processing pipelines during peak windows doubles token expense.

DeepSeek V4 is not suitable for organizations requiring dedicated on-premise deployments without internal machine learning engineering staff, nor is it appropriate for teams handling highly sensitive customer data subject to strict sovereignty controls that prohibit third-party API processing in foreign jurisdictions. Those teams should deploy validated open-weights models inside private cloud infrastructure or use enterprise cloud providers with explicit data residency guarantees.

Our evaluation would change if DeepSeek introduced native enterprise service-level agreements with guaranteed uptime, published verified per-minute rate limits, or if independent audit suites demonstrated that V4.1-Flash closed the 17.7-point factual accuracy deficit on SimpleQA.

## Model Evaluation Strategies for Engineering Teams

Engineering teams evaluating DeepSeek V4 benchmarks should construct an internal golden evaluation dataset rather than relying exclusively on public academic leaderboards. Static test collections like HumanEval and GSM8K are frequently ingested into pre-training corpora, inflating scores relative to novel real-world inputs.

A rigorous internal evaluation protocol requires four structured steps.

DeepSeek operates a prepaid pay-as-you-go system with no monthly subscriptions or seat licensing. For a monthly production workload consuming 10 million input cache-miss tokens and 2 million output tokens, `deepseek-flash` costs $5.40 during peak hours ($2.70 off-peak). The same workload on `deepseek-v4-pro` totals $21.12 peak ($10.56 off-peak). By comparison, processing that volume on OpenAI's GPT-6 Astra at $10.00 per 1 million input tokens and $50.00 per 1 million output tokens costs $200.00.

- Extract 100 to 200 real production prompts from production logs, including ambiguous prompts, complex multi-turn conversations, and tasks requiring strict JavaScript Object Notation (JSON) output schemas.
- Run automated assertions across peak and off-peak hours, measuring execution accuracy, formatting compliance, and latency.
- Model token economics on the published rate card, confirmed on DeepSeek's API pricing documentation.
- Pin a model version identifier such as DeepSeek-V4-Pro-0813 or deepseek-flash in application code, and re-check the DeepSeek pricing page before you rerun the DeepSeek V4 benchmarks on your own data.

## Frequently Asked Questions

- DeepSeek V4 shows competitive performance on code generation and mathematics, with DeepSeek-V4.1-Flash scoring 79.4 on HumanEval and 93.0 on GSM8K. On competitive programming, its Think Max mode reaches a 3206 Codeforces rating, surpassing GPT-5.4 xHigh at 3168. However, its factual knowledge retrieval remains weak, scoring only 57.9 on SimpleQA-Verified compared to 75.6 for Google Gemini-3.1-Pro.
- DeepSeek did not publish comparative benchmark scores against Anthropic Claude Opus 4.8. Published evaluations for the DeepSeek V4 family compare performance exclusively against Google Gemini-3.1-Pro and OpenAI GPT-5.4 across a limited set of three evaluations. Anyone claiming a verified benchmark win over Claude Opus 4.8 is extrapolating beyond official data.
- For most production workloads, DeepSeek-V4.1-Flash is the best DeepSeek model. It outscores the larger DeepSeek-V4-Pro across MMLU-Pro (74.1 vs 73.5), HumanEval (79.4 vs 76.8), GSM8K (93.0 vs 92.6), DeepSWE (74.2 vs 62.7), and Terminal-Bench (90.6 vs 87.9). It also costs roughly a quarter of the price of V4-Pro, provides multimodal vision support, and offers a higher concurrency limit of 2,500 simultaneous requests.
- In head-to-head testing published by DeepSeek for its V4-Pro-Max reasoning mode, DeepSeek scored 93.5 on LiveCodeBench compared to 91.7 for Google Gemini-3.1-Pro High. However, on the SimpleQA-Verified factual knowledge test, Gemini-3.1-Pro High scored 75.6 while DeepSeek V4-Pro-Max scored 57.9. Google maintains a substantial 17.7-point lead in factual recall accuracy.
- Yes, DeepSeek released the weights for DeepSeek-V4.1-Flash under the Massachusetts Institute of Technology (MIT) license on Hugging Face. The model is supported in inference engines including vLLM, SGLang, and Transformers, with quantized versions compatible with Ollama, llama.cpp, and LM Studio. Running the 552 billion parameter Mixture of Experts model requires substantial hardware, though its 890-byte Key-Value cache reduces memory overhead compared to prior releases.

### The complete AI playbook for your team

Cut your AI bill with Chinese open-weight models — without the risk: Safety, pricing and savings for Kimi K3, DeepSeek, Qwen and z.ai GLM — the four-vendor comparison for owners and IT leads.

Get the guide — $59 (reg. $89)
