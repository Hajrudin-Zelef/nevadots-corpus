---
id: collect-261121-veille-ia/nvidia-hardware/nvidia-vera-rubin-mlperf-10x-is-provisional-not-verified
title: "Nvidia Vera Rubin MLPerf 10x Is Provisional, Not Verified"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: ["2026-11-16"]
keywords: ["nvidia", "vera", "rubin", "does", "training", "provisional", "mlperf", "gb300"]
source: https://alfw.ai/article/nvidia-vera-rubin-mlperf-10x-provisional/
source_anchor: ""
source_lines: [1, 28]
section: "NVIDIA_Hardware"
---
## Full coverage, against its own previous system

Start with what is solid. MLPerf Training v6.1 measures how fast a system trains a set of models to a target accuracy. Nvidia says its platform was the only one with results on every benchmark in this round, covering large language models, image generation, recommender systems and reinforcement-learning post-training. Vera Rubin NVL72, in its debut, delivered up to 2x more performance per GPU than the GB300 NVL72 system it succeeds.

That is a reported generational gain, and it is a comparison with Nvidia's own hardware. The cited up-to-2x comparison is against Nvidia's own GB300 system; benchmark coverage alone does not establish cross-vendor superiority. Twenty organizations submitted Training results on Nvidia hardware, which speaks to the size of the ecosystem around it.

The scaling figure is the most checkable training number in the release. On the DeepSeek-V3 benchmark test, quadrupling from 256 to 1,024 GPUs cut the time to the benchmark's target from 16 minutes to about four, which Nvidia puts at about 97% efficiency. Nvidia reports approximately 97% scaling efficiency on this test; the post does not establish equivalent scaling across other workloads.

## The 10x number, and the footnote under it

The headline figure for agentic inference needs slower reading. In its first MLPerf Endpoints submission, Nvidia says Vera Rubin NVL72 delivers more than 10x the throughput per watt of GB300 NVL72 on Kimi K3, a large open model built for agentic workflows, at the same response speed.

Nvidia's own footnote defines that phrase narrowly and discloses the rest. "Same response speed" means a shared minimum of 50 tokens per second per user in average interactivity. The two runs were not set up alike: the Vera Rubin run used 32 accelerators at 1,024 concurrent queries, the GB300 run 48 accelerators at 48 concurrent queries. ALFW's calculation from Nvidia's figures gives about 410.5 versus 36.8 tokens/sec/kW, a ratio of about 11.1x, so the "more than 10x" holds arithmetically. On time to first token at the 90th percentile, the Vera Rubin run was slower, 4,673.9 ms against 2,305.1 ms. That does not invalidate the ratio; it establishes that the shared interactivity threshold is not equal latency. The 10x is a ratio produced by one specific configuration pairing, not a general statement that Vera Rubin answers agents ten times better.

Then there is the status line, again in Nvidia's own words. The GB300 results were verified by the MLCommons Association. The Vera Rubin results are provisional, pending that verification, and subject to change on peer review. A provisional result is a submitted result. It is not a confirmed one, and a launch headline built on it should carry that word with it.

## What this does and does not settle

Taken at face value, the Training results support a straightforward reading: Vera Rubin is a substantial step over GB300 per GPU, it scales well on at least one large training test, and Nvidia's partner ecosystem showed up in force. That is enough to take the platform seriously before any Endpoints number is confirmed.

What the release does not settle is the buying question. It gives no price, no shipping timeline and no general availability date for Vera Rubin, and no cost figure for a full deployment. Throughput per watt is a useful efficiency measure. It is not cost per token at a quoted price, and it is not a delivery date.

The test to watch is short. Does MLCommons verify the provisional Kimi K3 result as submitted, or does peer review move it? And when Nvidia next talks about Vera Rubin, does it attach a price and a date? Until then, keep the two numbers in separate boxes: reported gains of up to 2x per GPU in training, from a completed benchmark round; more than 10x per watt for agents, provisional, from one configuration pairing that Nvidia itself has flagged as unverified.

## Sources

1. [1] Nvidia, "NVIDIA Sweeps MLPerf Training and Endpoints as Vera Rubin Debuts With Leading Performance", Nvidia Blog, November 16, 2026 (primary source; all benchmark claims, footnote figures and the provisional-status wording above; read first-hand by ALFW)Read source
2. [2] MLCommons, MLPerf Training v6.1 and MLPerf Endpoints v1.0 results (benchmark authority; the entry IDs and figures above are as cited in Nvidia's post)Read source
