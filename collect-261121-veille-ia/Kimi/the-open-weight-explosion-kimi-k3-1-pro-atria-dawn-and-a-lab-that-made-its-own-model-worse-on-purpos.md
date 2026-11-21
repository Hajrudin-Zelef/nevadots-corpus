---
id: collect-261121-veille-ia/kimi/the-open-weight-explosion-kimi-k3-1-pro-atria-dawn-and-a-lab-that-made-its-own-model-worse-on-purpos
title: "The Open-Weight Explosion: Kimi K3.1 Pro, Atria Dawn, and a Lab That Made Its Own Model Worse on Purpose"
domain: kimi
role: reference
task: article
actors: ["Moonshot AI"]
dates: []
keywords: ["model", "moonshot", "open-weight", "kimi", "atria", "dawn", "weights", "cyber"]
source: http://dev.to/976905690/the-open-weight-explosion-kimi-k31-pro-atria-dawn-and-a-lab-that-made-its-own-model-worse-on-4jho
source_anchor: ""
source_lines: [1, 51]
section: "Kimi"
---
For a few days at the end of October, the most interesting question in AI was not which model is best but what did the lab choose to leave out. The answer, from Moonshot AI, is a small landmark: it deliberately removed cyber-offense training data from its newest open-weight model, making that model measurably worse at finding and exploiting security flaws, and then shipped the weights anyway.

Kimi K3.1 Pro: the largest open model, by design

On October 24, Moonshot AI released Kimi K3.1 Pro, a model it calls its most capable 3-trillion-class open-weight model to date. It is live on Kimi.ai and the API now, with full weights promised by November 4, though, as of release, no license had been named. The headline numbers are the ones open-weight watchers have learned to expect from Chinese labs: a model in the roughly 3-trillion-parameter class, a 1-million-token context window, and benchmark scores that sit within a hair of the closed frontier. On Terminal-Bench 4.0, Moonshot reports 65.7 percent, against 66.4 percent for Claude Opus 5.5, close enough that the gap is now a rounding error rather than a generation.

The capability demo is more interesting than the table. In an internal test, engineers asked K3.1 Pro to build a simulated humanoid walking controller. The model analyzed results, located problems, and iterated on the controller until the simulated body went from collapsing after a few steps to alternating its feet and walking continuously for more than eleven seconds, correcting, by the company's account, 36 separate torque-arm errors along the way. That is not a benchmark score. It is a model doing closed-loop engineering, and it is the kind of result that matters more for real deployment than another point on a leaderboard.

The deliberate downgrade

Here is the twist that makes this run worth writing about. As Moonshot trained the model, it kept getting better at cyber tasks, finding and exploiting security flaws. The company traced the improvement to cyber material in both its general pretraining data and the practice tasks used for reinforcement learning. So, for the version it would release, it removed that material from both stages.

The effect is measurable and the company published it: with the cyber data removed, the released model solved 81.0 percent of Moonshot's in-house exploit tasks; a version trained with the data left in solved 90.5 percent. Moonshot says it aims not to release open weights that exceed the best cyber capability any model has reached. In plain terms: the lab chose to ship a slightly dumber, safer model rather than the strongest one it could build.

Why does that matter? Because it reframes a debate. The conventional story is that open-weight models lag the closed frontier by six to seven months, Epoch AI's estimate, and that the lag is a symptom of being behind. Kimi's report suggests that at least part of the lag is now a choice: a lab deciding what goes into training, not merely a lab that cannot keep up. The best open model on Epoch's cyber index, Z.ai's GLM-5.3, reaches a score the closed models hit months ago. Some of that distance is capability. Some of it, apparently, is conscience.

Atria Dawn: MIT weights and a 512K window

Moonshot was not alone. On October 27, Shanghai AI Laboratory moved Atria Dawn from preview to full release, publishing open weights plus an FP8 build on Hugging Face and ModelScope under the MIT license. It is built on the 744-billion-parameter mixture-of-experts model GLM-5.2 and targets long-horizon research and engineering agent work. The context window doubled from the preview's 256K to 512K, with text-only input.

The vendor's own table is honest about where it wins and where it does not. Atria Dawn leads the listed models on AutomationBench (54.7), CyberGym (86.8), SkillsBench (67.8) and BFCL V4 (78.4). It still trails Claude Opus 5 on coding: SWE-bench Pro 61.7 versus 76.5, and Terminal-Bench 2.1 81.0 versus 90.2. That is the shape of the current open-weight wave in one table: best-in-class on agentic and tool-use tasks, still behind on the hardest software engineering, and free, under a permissive license, downloadable today.

MiniMax and the price floor

The third leg of the stool is price. MiniMax's M3.1 Flash, open-sourced on October 23, is pitched at roughly 70 percent lower agent costs than its predecessor with inference nearly three times faster, at 0.15 dollars per million input tokens. The company says the model processed more than 100 trillion tokens in the 72 hours after it went open. When a competent open model is that cheap and that fast, the economics of routing every agent task to a closed API start to look optional.

Taken together, the month tells a clear story. At least four top Chinese labs shipped major models in October. On OpenRouter, global weekly token volume reached 188 trillion in the week of October 19 to 25, up nearly 15 percent week over week, with Chinese models accounting for 93.54 trillion, more than the United States for a 26th consecutive week. The open-weight tier is no longer the discount aisle. It is where a large share of the world's inference actually runs.

Why it is exciting, surprising, and just a little frustrating

It is exciting because a 3-trillion-parameter model with a million-token context, released with weights, closes a gap that used to define the industry. It is surprising, genuinely, that a lab would publish a model it intentionally made worse at hacking, and put the number in the report. And it is frustrating for anyone selling closed-model premium: when the open tier is this good and this cheap, the capability gap you charge for shrinks every quarter, and we are safer becomes a harder sales pitch than we are stronger.

The next fight is not about who has the biggest model. It is about who can be trusted with the weights, and, increasingly, who is willing to ship less than their best on purpose.

Key Facts

Kimi K3.1 Pro: Moonshot AI, released October 24, 2026; roughly 3-trillion-parameter class, 1-million-token context; weights due by November 4; Terminal-Bench 4.0 65.7 percent versus Claude Opus 5.5 at 66.4 percent.

Deliberate downgrade: Moonshot removed cyber-offense data from pretraining and RL for the released model; exploit-task success fell from 90.5 percent (data left in) to 81.0 percent (data removed).

Atria Dawn: Shanghai AI Laboratory, full release October 27, 2026; 744-billion-parameter MoE GLM-5.2; 512K context; MIT license; BF16 and FP8 weights on Hugging Face and ModelScope.

Atria Dawn scores (vendor): AutomationBench 54.7, CyberGym 86.8, SkillsBench 67.8, BFCL V4 78.4; trails Opus 5 on SWE-bench Pro (61.7 vs 76.5) and Terminal-Bench 2.1 (81.0 vs 90.2).

MiniMax M3.1 Flash: open weights October 23, 2026; about 70 percent lower agent cost, nearly 3x faster inference, 0.15 dollars per million input tokens; over 100 trillion tokens processed in 72 hours.

Market context: OpenRouter weekly volume 188 trillion tokens (October 19-25), up 14.63 percent; Chinese models 93.54 trillion, ahead of the US for a 26th straight week.

Sources: Moonshot AI release notes, On The Wire and Relay AI benchmark tables, Shanghai AI Laboratory and Hugging Face model cards, MiniMax, OpenRouter, Epoch AI, boringai.tech AI Digest (October 19-25 2026), threerouter.

## Top comments (0)
