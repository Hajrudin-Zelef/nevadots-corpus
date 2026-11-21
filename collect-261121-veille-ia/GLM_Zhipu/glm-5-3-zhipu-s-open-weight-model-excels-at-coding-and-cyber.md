---
id: collect-261121-veille-ia/glm-zhipu/glm-5-3-zhipu-s-open-weight-model-excels-at-coding-and-cyber
title: "GLM 5.3: Zhipu's Open-Weight Model Excels at Coding and Cyber"
domain: glm-zhipu
role: reference
task: article
actors: ["Zhipu AI"]
dates: ["2026-08-14"]
keywords: ["zhipu", "model", "more", "training", "coding", "cyber", "about", "benchmark"]
source: http://dev.to/jamilxt/glm-53-zhipus-open-weight-model-excels-at-coding-and-cyber-1m86
source_anchor: ""
source_lines: [1, 82]
section: "GLM_Zhipu"
---
Zhipu AI released GLM 5.3 on August 14, 2026, and the announcement opens with a one-sentence thesis: "Scaling post-training is all we did for GLM-5.3."

No new base model. No bigger architecture. GLM 5.3 is the same roughly 744-billion-parameter Mixture-of-Experts brain as GLM 5.2, with about 40B parameters active per token (per Zhipu's GLM 5.2 specs). Every claimed gain comes from training methods, not model size. That is a statement about where the AI frontier is moving: the next jump may not come from a bigger model, but from training the one you already have more cleverly.

The release has two headlines. First, coding: Zhipu says GLM 5.3 is 50% better than GLM 5.2 on its in-house coding benchmark and reaches open-source state-of-the-art scores on Terminal-Bench 3.0 and Agents' Last Exam (CLI). Second, security: the team says it developed "emergent cyber capability" faster than it expected. GLM 5.3 tops CyberGym, a vulnerability-discovery benchmark, and more than doubles GLM 5.2 on exploitation benchmarks.

A few things upfront. All benchmark numbers below are vendor-published. I have not run GLM 5.3 myself (the standalone API is still listed as "coming soon"), and the weights will not be public for about two weeks. Treat the scores as Zhipu's claims, not independent verification.

The "how" matters more than the "what" here. GLM 5.3 was trained on the same stack that produced GLM 5.2: IndexShare for long-context efficiency, SAO for reinforcement learning on long-horizon tasks, and an open-source RL framework called slime (from THUDM, the lab behind Zhipu). What changed over the past month is scale: more task environments, more diverse tasks, more compute.

The environments matter most. Zhipu built pipelines that synthesize training tasks end to end: research agents collect patterns from real engineering work and turn them into runnable, multi-step environments with hidden state. A judge agent then attempts each task to confirm it is actually solvable. Verifiers are synthesized without access to the reference solution, and solver trajectories are used to discover and close reward shortcuts, the failure mode that typically breaks RL training. Some environments represent several days of work for an experienced engineer, like diagnosing bottlenecks on a training stack with access to clusters, storage, codebases, docs, and experiment results.

The system-level payoff: optimizations that include workload-aware scheduling improved end-to-end RL training throughput by more than 2.3x for long-horizon coding tasks, which is what made scaling practical.

Zhipu says GLM 5.3 improved 50% over GLM 5.2 on Z.ai Code Bench, its private benchmark, which also showed a token-efficiency gain. At Max effort, GLM 5.3 scored 34.5% at roughly 75K output tokens per task, versus GLM 5.2's 23.4% at 96K. At High effort it scored 31.4% at about 50K tokens, ahead of Claude Opus 4.8's 29.5% at 120K. Zhipu's post notes GLM 5.3 still trails Fable 5 at 39.5% on Max effort. (Naming note: Zhipu's table lists this Anthropic model as "Fable 5 (w/ fallback)", while its cyber section calls the same scores "Mythos 5". I use the table name throughout.) Better results with fewer tokens is the kind of gain developers actually feel in cost and latency.

On public benchmarks, the vendor-reported numbers against GLM 5.2:

- 
**Terminal-Bench 3.0:** 28.3, up from 4.6. Context: Kimi K3 scored 17.4, Opus 4.8 scored 21.1, and the top closed models (Fable 5, GPT-5.6 Sol) sit at 33.7 and 34.6.
- 
**Terminal-Bench 2.1:** 88.2, up from 81.0. Kimi K3 (88.3) edges it out, and GPT-5.6 Sol leads at 88.8.
- 
**DeepSWE v1.1:** 66.9, up from 46.2. Kimi K3 (67.5) and the top closed models (69.7, 72.7) stay slightly ahead.
- 
**Agents' Last Exam (CLI):** 28.5, up from 23.8 - the best open-weight score (Kimi K3: 27.6), just behind the closed GPT-5.6 Sol (28.6).
- 
**FrontierSWE:** 78.1, up from 67.5, now ahead of Opus 4.8 (66.5), though Fable 5 leads at 88.2.
- 
**SWE-Marathon v1.1:** 42.5, up from 19.4, though Kimi K3 (48.1) and Opus 4.8 (48.8) still lead.
- 
**PostTrainBench:** 39.8, up from 31.7, ahead of Opus 4.8 (32.9), though Fable 5 (41.8) and GPT-5.6 Sol (36.2) stay ahead.

The pattern: GLM 5.3 leads the open-weight class on Terminal-Bench 3.0, Agents' Last Exam, FrontierSWE, and PostTrainBench, while Kimi K3 still holds Terminal-Bench 2.1, DeepSWE, and SWE-Marathon. Either way, it closes much of the gap to the closed frontier that was visible in GLM 5.2. One caveat: HN users noticed the comparison table skips Anthropic's Opus 5 and xAI's Grok 4.6, so the picture of the "closed frontier" is not complete.

The surprise of this release is security. Zhipu says it added vulnerability-discovery data to the training mix expecting the model to get better at finding flaws. Instead, as training scaled, the model started chaining multiple stages of exploitation into coherent plans.

Vendor-reported scores:

- 
**CyberGym** (vulnerability discovery from source code): 84.5%, up from 77.2%, the best score on the benchmark, ahead of every closed model (Fable 5: 83.8; GPT-5.6 Sol: 83.6).
- 
**ExploitBench** (deeper exploitation reasoning): 54.4%, up from 24.4%, more than double GLM 5.2 and ahead of Kimi K3 (32.2) and Opus 4.8 (40.0). The closed leaders stay far ahead: Fable 5 at 78.0 and GPT-5.6 Sol at 76.5.
- 
**ExploitGym** (exploits completed under time budgets): 105 tasks in 2 hours and 130 in 6 hours, versus 29 and 39 for GLM 5.2. Opus 4.8 manages 80 and 120; the leaders are at 181 and 247 (Fable 5) and 216 and 293 (GPT-5.6 Sol).

The gains are largest exactly at the hardest end of the chain - where the absolute gap to the leaders is still widest.

The capability is not theoretical. Zhipu runs a public disclosure effort at cvd.z.ai. Its models, working with security teams, identified 2,436 vulnerabilities across 269 projects, including 1,097 medium-to-high severity issues. The oldest flaw dates to 1981, and on average a vulnerability lived 26.6 years before discovery. Of those findings, 53 are publicly disclosed and 2,383 are still under embargo.

This is where the community splits. The Hacker News thread (971 points and 483 comments within hours) is divided on it. One position: open models let defenders use the same tools as the attackers, who currently face fewer restrictions. The other asks the question, as one commenter put it: "What safety evaluation? What safety hardening? They already evaluated it and found it to be highly capable at exploiting security vulnerabilities." Zhipu says the two-week delay before releasing weights exists precisely for safety evaluation and hardening. Whether that is enough is an open question, not a settled one.

- 
**Coding Plan:** GLM 5.3 is live for all GLM Coding Plan users today. The plan moved to a points-based quota, and model calls outside peak hours (14:00 to 18:00, UTC+8, Monday to Friday) consume 50% of the standard points.
- 
**Standalone API:** listed as "coming soon" with no date. The model ID will be glm-5.3.
- 
**Thinking modes:** three effort levels now: low, high, and max. This is a breaking change.`thinking.type: "disabled"` is no longer supported, and such requests will fail. Migrate to`enabled` with`reasoning_effort: "low"` before switching model IDs.
- 
**ZCode:** Z.ai's own coding agent advertises a 98%+ cache hit rate and a limited-time 1.5x quota boost through August 31.
- 
**Weights:** due in roughly two weeks, after safety work. The license is not confirmed yet. GLM 5.2 was MIT, and the community is watching closely to see if 5.3 keeps it.
- 
**Text-only:** no multimodal support, like GLM 5.2. That was a recurring complaint on HN.
- 
**OpenRouter:** GLM 5.3 is not listed there yet as of writing.

GLM 5.3 is the strongest open-weight coding model yet announced, by Zhipu's numbers, and it got there without a new base model. That is the real signal: post-training is now a first-class lever for frontier progress, and the open labs are executing it fast.

In June, GLM 5.2 launched with a handful of benchmark numbers. Two months later, GLM 5.3 shipped on the same base model - with a full comparison table and a win over 5.2 on every benchmark in it. That contrast is worth pausing on.

The cyber capability changes the conversation. A model this good at finding and exploiting vulnerabilities, with weights on the way, is a dual-use moment. It is good news for defenders who review code at scale, and dual-use capabilities like this usually draw government scrutiny.

For developers, the practical picture is simple: if you are on the GLM Coding Plan, the upgrade is live today. If you want the API or the weights, wait the two weeks and watch the license announcement. I write about open-source AI and developer tooling every week - subscribe, it's free.

Are you on the GLM Coding Plan, or waiting for the API and the weights? I would like to hear how 5.3 holds up on real work once you try it.

*Sources: Z.ai Blog - GLM-5.3, Z.ai Docs - GLM-5.3, Z.ai Security Disclosure Ledger, slime on GitHub, Hacker News thread*

## Top comments (1)

The part I would measure is not whether the tool works once, but whether it leaves a cleaner path for the next run: logs, constraints, and a smaller surface area for mistakes.
