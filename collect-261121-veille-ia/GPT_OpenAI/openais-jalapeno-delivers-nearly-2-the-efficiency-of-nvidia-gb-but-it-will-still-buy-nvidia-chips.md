---
id: collect-261121-veille-ia/gpt-openai/openais-jalapeno-delivers-nearly-2-the-efficiency-of-nvidia-gb-but-it-will-still-buy-nvidia-chips
title: "OpenAI’s Jalapeño Delivers Nearly 2× the Efficiency of NVIDIA GB, but It Will Still Buy NVIDIA Chips"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["jalapeno", "nvidia", "test", "openai", "inference", "first", "efficiency", "results"]
source: https://www.topcpu.net/en/news/openai-jalapeno-inference-benchmarks-gb200-gb300
source_anchor: ""
source_lines: [1, 29]
section: "GPT_OpenAI"
---
OpenAI has published the first InferenceX benchmark results for its custom Jalapeño inference chip, developed with Broadcom. Compared with NVIDIA GB200 and GB300 systems, it achieved 1.5–1.9× higher peak throughput per watt and up to 3.6× lower end-to-end latency.

On August 25, 2026, OpenAI published the first performance test results for Jalapeño, its first custom inference-focused ASIC developed jointly with Broadcom.

The chip is optimized specifically for large language model inference and was first unveiled publicly in June 2026.

The tests used the InferenceX inference benchmark suite developed by SemiAnalysis. They covered three large models—GPT-OSS 120B, DeepSeek R1 670B, and Kimi K2.5 1T—and compared Jalapeño with NVIDIA GB200 and GB300 systems.

The results show that Jalapeño delivered 1.5–1.9× higher peak throughput per watt than the comparison products, while reducing end-to-end latency by 1.7–3.6×.

In the DeepSeek R1 670B test, Jalapeño recorded an end-to-end latency of 1.65 seconds, approximately 1/3.6 that of GB300. Its peak throughput per watt was 1.7× higher than GB300’s.

In the GPT-OSS 120B test, Jalapeño’s peak throughput per watt was 1.9× that of GB200, while end-to-end latency fell from 1.80 seconds to 1.03 seconds.

The efficiency comparison was normalized using the package power figures publicly disclosed by each vendor: Jalapeño is rated at 700W, GB200 at 1200W, and GB300 at 1400W.

OpenAI also disclosed that Jalapeño’s sustained measured power consumption under the test workload peaked at approximately 550W, below its rated figure.

OpenAI explicitly stated that the test results apply only to inference performance under specific models, system configurations, and operating points. They do not cover training tasks, other data types, or other AI workloads, and should not be taken as a general conclusion that Jalapeño outperforms NVIDIA’s Blackwell series across all scenarios.

The test data was provided by OpenAI. SemiAnalysis engineers worked with the OpenAI team to verify the relevant test runs in a laboratory, but did not independently complete the full benchmark suite. The results therefore do not constitute a fully independent third-party product test.

Jalapeño’s architecture is deeply optimized for large language model inference, with a focus on reducing the efficiency losses caused by data movement during inference. It attempts to keep model state, including KV Cache, stored locally as much as possible, while accommodating the workload requirements of prefill, decoding, and communication. The design aims to avoid the throughput-versus-latency trade-offs often seen in general-purpose architectures.

OpenAI said that the first Jalapeño systems are expected to be deployed to its own computing infrastructure by the end of 2026. The second-generation product has entered a deep development phase, while the third generation is also under development.

At the same time, OpenAI will not completely replace its existing hardware. It will continue purchasing accelerators from NVIDIA and other partners to support both training and inference requirements.

As the first publicly disclosed test of a custom inference ASIC developed by a large model provider, Jalapeño’s laboratory data demonstrates the efficiency advantages of a specialized architecture under fixed workloads. Its real-world performance still requires long-term validation after deployment in production data centers.
