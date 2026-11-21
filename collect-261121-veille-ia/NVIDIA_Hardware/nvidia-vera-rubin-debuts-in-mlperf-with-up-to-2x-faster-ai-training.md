---
id: collect-261121-veille-ia/nvidia-hardware/nvidia-vera-rubin-debuts-in-mlperf-with-up-to-2x-faster-ai-training
title: "Nvidia Vera Rubin Debuts in MLPerf With Up to 2x Faster AI Training"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: []
keywords: ["vera", "rubin", "mlperf", "nvidia", "training", "results", "test", "more"]
source: https://aiparabellum.com/nvidia-vera-rubin-mlperf-debut-2x-training/
source_anchor: ""
source_lines: [1, 61]
section: "NVIDIA_Hardware"
---
Nvidia’s Vera Rubin platform made its first appearance in the MLPerf benchmarks today, and the company says it trained AI models up to twice as fast per GPU as its current Blackwell Ultra systems. The Vera Rubin MLPerf results cover both AI training and a new test for agentic AI, where Nvidia claims more than 10x better throughput per watt. The numbers come from MLPerf Training v6.1 and the first round of MLPerf Endpoints, industry benchmarks run by the MLCommons group.

## Vera Rubin MLPerf Training Results at a Glance

MLPerf Training measures how quickly a system can train a set of AI models to a fixed accuracy target. This round has seven tests, covering language models, image generation, recommendations and reinforcement learning.

According to Nvidia, the Vera Rubin NVL72 rack posted the fastest time to train on six of the seven benchmarks. On the seventh, a large DeepSeek-V3 test, Nvidia’s older GB300 systems took the fastest overall time by running on a huge cluster of 10,240 GPUs.

| Result | Vera Rubin NVL72 | GB300 NVL72 | 
|---|---|---|
| DeepSeek-V3 on 1,024 GPUs | 4.14 minutes | 8.03 minutes | 
| Performance per GPU | Up to 2x | Baseline | 
| Scaling efficiency, 256 to 1,024 GPUs | Above 97% | Not stated | 
| Fastest absolute DeepSeek-V3 time | Not the record holder | 1.38 minutes on 10,240 GPUs | 

Outside of the DeepSeek-V3 test, Nvidia says Vera Rubin ran roughly 1.4x to 1.6x faster than GB300 across the rest of the suite. The company notes that the biggest gains showed up on the largest, most communication-heavy model.

### A New Test for AI Agents

This round added the first reinforcement learning post-training benchmark, using a Qwen 3.5 397B model. Post-training is how AI labs teach models to reason, call tools and finish multi-step tasks, so it matters for anyone building AI agents.

Nvidia says its Vera CPU handles the simulated environments that agents practice in, while the GPUs generate tokens and update the model. A single Vera CPU rack ran more than 22,500 environments at once, according to the company.

## Vera Rubin Agentic Results on MLPerf Endpoints

MLPerf Endpoints is a newer benchmark that tests real agentic coding and enterprise workflows and checks that the agents still complete their tasks correctly. Nvidia says Vera Rubin led all of its agentic tests in its first submission.

In a provisional result using the open Kimi K3 model, Nvidia claims Vera Rubin NVL72 delivered more than 10x the throughput per watt of GB300 NVL72 at the same response speed. That metric matters because AI data centers are limited by power, so more tokens per watt means more users served.

## Who Took Part This Round

Nvidia says 80% of closed-division submissions ran on its platforms. Twenty partner organizations submitted results on Nvidia hardware, including:

- Cloud providers such as Microsoft Azure, Google Cloud, Oracle, CoreWeave, Lambda and Nebius
- Server makers such as Dell Technologies, HPE, Supermicro, ASUS and Cisco
- Research users such as the University of Florida

Azure matched Nvidia’s largest run with its own 10,240-GPU GB200 submission. Nvidia also shared some newer software reruns, but it labels those as unverified results that MLCommons has not reviewed.

### Keep the Source in Mind

These figures come from Nvidia’s own write-ups of the results. MLPerf is a respected industry benchmark, but vendors choose which tests to enter and how to frame them, and rival chipmakers did not appear across every test. Our earlier coverage of Vera Rubin designs explains how the platform fits into Nvidia’s data center plans.

## What This Means for You

You will not buy a Vera Rubin rack, but these results shape the AI tools you use every day. Here is the practical angle:

1. Faster training means AI labs can test more ideas, which can shorten the gap between model releases.
2. Better tokens per watt can lower the cost of running agents, which may show up in cheaper or faster AI services.
3. Cloud providers are already offering Vera Rubin systems, so apps built on them may get speed gains without any change on your end.
4. For a closer look at running models yourself, see our piece on local AI agents.

For more chip and model updates as they happen, check our latest AI news.

## Sources

Primary source: Nvidia’s MLPerf announcement. Additional detail from Nvidia’s MLPerf Training technical blog.

## Conclusion

Vera Rubin’s MLPerf debut gives Nvidia a strong opening set of numbers, with up to 2x faster training per GPU than Blackwell Ultra and big claimed gains in agentic inference efficiency. The results also show older GB300 clusters still holding the overall speed record at massive scale. Since the figures come from Nvidia’s own reporting, independent results from MLCommons and rival chipmakers will give a fuller picture. Either way, faster and more efficient hardware tends to flow down into the AI apps people use every day.
