---
id: collect-261121-veille-ia/glm-zhipu/swe-2-flash-vs-glm-5-3-flash-glm-wins-on-price
title: "SWE-2 Flash vs GLM 5.3 Flash: GLM Wins on Price"
domain: glm-zhipu
role: reference
task: article
actors: ["Zhipu AI"]
dates: []
keywords: ["flash", "cognition", "swe-2", "price", "wins", "weights", "glm", "models"]
source: http://dev.to/shaam_ai/swe-2-flash-vs-glm-53-flash-glm-wins-on-price-3l8i
source_anchor: ""
source_lines: [1, 95]
section: "GLM_Zhipu"
---
In the SWE-2 Flash vs GLM 5.3 Flash comparison, GLM 5.3 Flash wins on price and access, while the two models are effectively level on quality. On the eight-benchmark table in Cognition's launch post of November 4, 2026, the models split 4-4 and average 51.7 (SWE-2 Flash) against 52.0 (GLM 5.3 Flash). A workload of 10M input and 2M output tokens costs about $0.47 (Rs 45) on the cheapest OpenRouter tier for GLM 5.3 Flash, converted at 94.80 INR per USD. SWE-2 Flash has no published API price and no public weights.

Choose GLM 5.3 Flash if you self-host, want inspectable weights, or want the cheapest tokens. Choose SWE-2 Flash if you already work in Devin Desktop or Devin CLI and want to use it free until November 30, 2026.

- Quality is a tie on Cognition's own table: 4-4 across eight evaluations, with averages of 51.7 and 52.0 (Cognition).
- DeepSeek V4.1 Flash has the highest average on that table, at 57.7, ahead of both models (Cognition).
- GLM 5.3 Flash costs from $0.0285 input and $0.0923 output per million tokens on OpenRouter (OpenRouter).
- SWE-2 Flash publishes only a relative price, "one-fifth the price" of the larger SWE-2, and keeps its weights private for now (Cognition).
- SWE-2 Flash is free for Pro, Max and Teams users in Devin Desktop and Devin CLI through November 30, 2026, but not in Devin cloud sessions (Cognition).
- Every score here is self-reported by Cognition on its own harness. No independent benchmark has been attached yet.

SWE-2 Flash is a mixture-of-experts coding model with 276B total and 12B active parameters, released by Cognition on November 4, 2026. Cognition describes it as a fully American coding model and says it is the first of its models to add a mid-training stage before reinforcement learning (Cognition).

It is built on Inkling-Small, a model Thinking Machines released in July 2026 with open weights, a 1M-token context window, and a reported SWEBench-Verified score above 80% (Thinking Machines). Cognition reports that its extra training lifted Inkling-Small on Terminal-Bench 4.0 from 0.9% to 20.0%, and on FrontierCode 2.0 from 3.2% to 24.0% (Cognition).

GLM 5.3 Flash comes from Z.ai (Zhipu), is listed at 320B total and 18B active parameters in Cognition's table, and ships with open weights. It has a 1,048,576-token context and is served by more than 36 providers on OpenRouter (OpenRouter). The practical difference is access: GLM 5.3 Flash can be downloaded, hosted and priced by competing vendors, while SWE-2 Flash is currently reachable through Cognition's own products. Cognition says it is "not sharing the weights publicly yet" and plans a programme for trusted partners and research labs in a few weeks (Cognition).

Neither. The table below reproduces Cognition's self-reported numbers from its launch post, with the larger SWE-2 (2.8T total, 104B active) shown for reference.

| Benchmark | SWE-2 Flash (276B-A12B) | GLM 5.3 Flash (320B-A18B) | DeepSeek V4.1 Flash (763B-A16B) | SWE-2 (2.8T-A104B) | 
|---|---|---|---|---|
| FrontierCode 2.0 | **24.0** | 14.2 | 19.3 | 23.8 | 
| FrontierCode 1.1 Main | **45.5** | 31.8 | 43.9 | 50.0 | 
| DeepSWE 1.1 | 60.1 | **63.4** | 77.0 | 73.0 | 
| Terminal-Bench 4.0 | 20.0 | **41.8** | 42.3 | 27.3 | 
| Terminal-Bench 2.1 | **89.2** | 84.3 | 91.0 | 92.8 | 
| SWE-Atlas QA | 50.0 | **56.5** | 71.0 | 56.3 | 
| SWE-Bench Pro v2 Hard | **89.9** | 88.2 | 86.3 | 90.6 | 
| ExploitBench | 34.8 | **36.0** | 31.1 | 36.1 | 

Bold marks the winner between the two Flash models in each row. Our arithmetic on Cognition's figures:

- SWE-2 Flash wins four: FrontierCode 2.0, FrontierCode 1.1 Main, Terminal-Bench 2.1 and SWE-Bench Pro v2 Hard.
- GLM 5.3 Flash wins four: DeepSWE 1.1, Terminal-Bench 4.0, SWE-Atlas QA and ExploitBench.
- Our analysis of the eight rows gives simple averages of 51.7 for SWE-2 Flash, 52.0 for GLM 5.3 Flash and 57.7 for DeepSeek V4.1 Flash, computed from Cognition's published scores.

The pattern matters more than the averages. SWE-2 Flash leads on the FrontierCode evaluations and on SWE-Bench Pro v2 Hard. GLM 5.3 Flash leads on the two long-horizon agent evaluations, DeepSWE 1.1 and Terminal-Bench 4.0, where its 41.8 is roughly double SWE-2 Flash's 20.0. If your workload is long terminal sessions, that gap deserves more weight than the tied average.

It is accurate as worded, and narrower than it sounds. The launch post says SWE-2 Flash "outperforms all American open-weight models, even models that are 3x larger" (Cognition). The qualifier "American" removes GLM 5.3 Flash and DeepSeek V4.1 Flash from the comparison, and those are the two open-weight models that tie or beat SWE-2 Flash on Cognition's own table.

There is a second wrinkle. The weights for SWE-2 Flash are not public yet, so it does not currently meet the usual definition of an open-weight model. The claim is best read as a statement about a narrow peer group, not a ranking of fast coding models in general.

GLM 5.3 Flash is priced per token and varies sharply by provider. The cheapest standard OpenRouter tier (Relace) charges $0.0285 per million input tokens and $0.0923 per million output tokens (OpenRouter). Z.ai's own API charges $0.15 input and $0.50 output (sfsi.exchange tracker), more than five times as much for the same model.

For a workload of 10M input and 2M output tokens, computed from those prices and converted at 94.80 INR per USD:

| Option | Cost per workload | In rupees | 
|---|---|---|
| GLM 5.3 Flash, cheapest OpenRouter tier | $0.47 | Rs 45 | 
| GLM 5.3 Flash, Z.ai direct | $2.50 | Rs 237 | 
| SWE-2 Flash | Not published | Free in Devin Desktop and CLI until November 30, 2026 | 

Cognition describes SWE-2 Flash as one-fifth the price and three times the speed of the larger SWE-2, but gives no absolute rate (Cognition). Without a per-token price, a cost comparison is impossible beyond the free window. The same post reports a demo of about 200 tokens per second against about 60 for SWE-2, from replayed runs rather than a live benchmark. A third-party launch report confirms the November 4 date, the 276B-A12B size, the free period and the gated weights.

For further price context across model families, see our comparison of Claude Sonnet pricing against GLM 5.3.

Pick GLM 5.3 Flash if you:

- run your own inference or need to audit and fine-tune the weights;
- want to shop between 36+ providers for the lowest token price;
- run long, multi-step terminal or repository tasks, where it scored higher on Cognition's own evaluations.

Pick SWE-2 Flash if you:

- already use Devin Desktop or Devin CLI on a Pro, Max or Teams plan and can use it at no extra charge before November 30, 2026;
- care most about FrontierCode-style tasks and SWE-Bench Pro v2 Hard, where it led;
- value speed inside Cognition's tooling and can accept closed weights.

If you are choosing a whole agent stack rather than a model, our OpenHands, Devin, Cline and Cursor verdict covers the tooling side. For other model matchups, see Kimi K3.1 Pro vs Claude Opus 5.5 and Qwen vs Claude vs Kimi.

Every number in the benchmark table comes from Cognition, run on Cognition's own harness, on the day it launched its own model. No third party has reproduced them, and Cognition itself publishes a limitations section in its post. The Flash models also differ in size, so the comparison is not like for like: GLM 5.3 Flash is listed with more active parameters (18B against 12B).

A simple average treats all eight evaluations as equally important, which is a choice we made, not a standard. Cost figures for GLM 5.3 Flash depend on provider availability and can change; we checked them on November 5, 2026. We also cannot say what SWE-2 Flash will cost after the free period, because Cognition has not said. Re-test on your own repositories before committing. Our corrections and testing policy lives on /pages/how-we-work.

**Q: Is SWE-2 Flash better than GLM 5.3 Flash?**

**A:** Not clearly. On Cognition's eight-benchmark table the models split 4-4 and average 51.7 against 52.0, which is a tie (Cognition). SWE-2 Flash is stronger on FrontierCode, while GLM 5.3 Flash is stronger on the long-horizon agent evaluations.

**Q: How much does GLM 5.3 Flash cost per million tokens?**

**A:** The cheapest OpenRouter tier charges $0.0285 input and $0.0923 output per million tokens (OpenRouter). Z.ai's direct API charges $0.15 input and $0.50 output (sfsi.exchange).

**Q: Is SWE-2 Flash open source or open weight?**

**A:** Not yet. Cognition says it is not sharing the weights publicly and plans a programme for trusted partners and research labs in a few weeks (Cognition). Its base model, Inkling-Small, does have open weights (Thinking Machines).

**Q: Can I use SWE-2 Flash for free?**

**A:** Yes, if you are on a Pro, Max or Teams plan, in Devin Desktop and Devin CLI, through November 30, 2026. The offer does not cover Devin cloud sessions (Cognition).

**Q: Which fast coding model scores highest on Cognition's table?**

**A:** DeepSeek V4.1 Flash, with an eight-benchmark average of 57.7 compared with 51.7 for SWE-2 Flash and 52.0 for GLM 5.3 Flash. The averages are our own arithmetic on Cognition's published scores.

## Top comments (0)
