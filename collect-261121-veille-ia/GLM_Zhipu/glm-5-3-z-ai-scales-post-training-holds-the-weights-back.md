---
id: collect-261121-veille-ia/glm-zhipu/glm-5-3-z-ai-scales-post-training-holds-the-weights-back
title: "GLM-5.3: Z.ai Scales Post-Training, Holds the Weights Back"
domain: glm-zhipu
role: reference
task: article
actors: ["Zhipu AI"]
dates: ["2026-08-14"]
keywords: ["glm-5", "post-training", "weights", "against", "base", "more", "gpt-5", "model"]
source: https://rits.shanghai.nyu.edu/ai/glm-5-3-post-training-scales-cyber/
source_anchor: ""
source_lines: [1, 27]
section: "GLM_Zhipu"
---
**On August 14, 2026, Z.ai (formerly Zhipu AI) released GLM-5.3** â and, unusually, without a new base model. GLM-5.3 sits on the same 743-billion-parameter Mixture-of-Experts base as June’s GLM-5.2, and every reported gain comes from extended post-training alone. The model is live now through Z.ai’s API, its GLM Coding Plan, and the ZCode client, but the weights are not: Z.ai says it will publish them in roughly two weeks, after a safety evaluation prompted by how far the model’s vulnerability-discovery ability ran ahead of expectations.

## Post-Training, Not a New Base

“Scaling post-training is all we did for GLM-5.3,” Z.ai wrote in its launch post. The stack it scaled on was built for GLM-5.2: IndexShare for long context, SAO for reinforcement learning over long-horizon tasks, and slime, Z.ai’s open-source framework for large-scale asynchronous RL. What changed this round was volume and variety â more task environments, more environment types, and longer training runs, including simulated professional work environments that the company says take a human engineer several days each to complete.

The jumps against its own predecessor are the clearest signal. On Terminal-Bench 3.0, GLM-5.3 scores 28.3 against GLM-5.2’s 4.6. DeepSWE goes from 46.2 to 66.9, AutomationBench from 26.2 to 48.2, and GDPVal-AA v2 from 1508 to 1769. Against frontier closed models the picture is more mixed than the “strongest open-weights coder” framing suggests: GLM-5.3 leads AutomationBench (48.2 vs 46.7 for Kimi K3, 46.2 for Fable 5, 45.8 for GPT-5.6 Sol) and GDPVal-AA v2, but trails on Terminal-Bench 3.0 (33.7 for Fable 5, 34.6 for GPT-5.6 Sol) and DeepSWE (69.7 and 72.7 respectively).

The more interesting number is the one about cost per task. On Z.ai’s internal Code Bench, GLM-5.3 reports 31.4% at roughly 50,000 output tokens, against Claude Opus 4.8’s 29.5% at about 120,000 â a comparable score for well under half the generated tokens. Claude Fable 5 still leads that benchmark at 39.5%. Thinking is now mandatory on GLM-5.3, exposed as three effort levels: low, high, and max.

## The Cyber Results, and Why the Weights Are Late

Z.ai trained GLM-5.3 on data and environments built for finding software vulnerabilities, and reports that the capability compounded faster than the training schedule predicted. Per the launch post, the model “began to reason across multiple stages of exploitation, forming coherent plans for complete exploitation chains” â behaviour Z.ai frames as emerging from scale rather than from a targeted objective.

On CyberGym, a vulnerability-reasoning evaluation, GLM-5.3 posts 84.5% â ahead of Mythos 5 (83.8%), GPT-5.6 Sol (83.6%), Kimi K3 (80.0%), and its own predecessor (77.2%). ExploitBench more than doubles, from 24.4% to 54.4%, but stays well behind Mythos 5 (78.0%) and GPT-5.6 Sol (76.5%). On ExploitGym, GLM-5.3 completes 105 tasks within a two-hour budget and 130 within six hours, against 29 and 39 for GLM-5.2 â and against 181 and 247 for Mythos 5.

The field results are what pushed the weights into review. Z.ai maintains a public disclosure ledger at cvd.z.ai, which records 2,436 vulnerabilities found across 269 open-source projects since GLM-5.2, 1,097 of them rated critical or high severity. Fifty-three carry published CVEs; 2,383 remain under embargo. The registry notes the defects span 45 years, with the earliest traceable to 1981 and an average latency of 26.6 years. Named projects include the Linux kernel, WebKit, FreeBSD, GStreamer, Suricata, and Joomla.

## What This Means

Two things are worth separating here. The first is a training result: GLM-5.3 is evidence that a fixed base can still be moved a long way by post-training alone, if you are willing to spend the compute on environment diversity. Terminal-Bench going from 4.6 to 28.3 on an unchanged 743B base is a large claim about where the remaining headroom sits, and it is the kind of claim that becomes checkable the moment the weights land.

The second is about the release itself. “Open-weights” is doing forward-looking work in Z.ai’s framing â the label describes what is coming, not what anyone can download today. A staged release, with a capability evaluation between announcement and weights, is closer to the pattern the large closed labs have used than to the same-day Hugging Face drops that made GLM-5, GLM-5.1, and GLM-5.2 notable. Whether that becomes the norm for open-weight releases with offensive-security capability is the thing to watch over the next two weeks; the alternative reading, that the delay is a marketing window, will be settled one way or the other by whether the weights actually ship.

For readers evaluating it practically: there is no published per-token API rate for GLM-5.3 yet, and access runs through the subscription Coding Plan and ZCode, with third-party clients including Claude Code and OpenCode supported. Z.ai’s rate card still tops out at GLM-5.2.

*This post was drafted with AI assistance and reviewed by RITS staff.*
