---
id: collect-261121-veille-ia/nvidia-hardware/nvidia-s-provisional-mlperf-endpoints-v1-0-results-vera-rubin-nvl72-delivers-5x-to-10x-the-throughpu
title: "NVIDIA's provisional MLPerf Endpoints v1.0 results: Vera Rubin NVL72 delivers 5x to 10x the throughput per kilowatt of GB300 NVL72 on agentic inference Â· Darius"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: ["2026-11-17"]
keywords: ["nvl72", "results", "endpoints", "vera", "rubin", "throughput", "kilowatt", "gb300"]
source: https://www.darius.wiki/en/daily/2026-11-17/vera-rubin-mlperf-endpoints/
source_anchor: ""
source_lines: [1, 12]
section: "NVIDIA_Hardware"
---
## // Key points

- Endpoints v1.0 measures the balance between serving capacity and responsiveness on coding and enterprise workflows through API endpoints. Generated tool calls and workflow intent are checked against reference values during runs, and separate SWE-bench Verified evaluations assess end-to-end task solving.
- Across Kimi K3, DeepSeek-V4.1-Flash and Qwen3.6-35B-A3B, Vera Rubin NVL72 achieves 5x to 10x the throughput per kilowatt of GB300 NVL72 and 3x to 5x higher end-to-end interactivity at matched concurrency; on Kimi K3 it exceeds 10x throughput per kilowatt at a minimum of 50 tokens per second per user.
- The Vera Rubin results are provisional, pending MLCommons verification; the GB300 results are verified.
- NVIDIA credits prefix caching, state reuse, Dynamo routing and disaggregated prefill and decode, plus model-specific work such as DeepSeek's MegaMoE kernel and Qwen's multi-token prediction.

Builder's takeFor teams without their own hardware, this suggests hosted open-model prices still have room to fall, but the results are provisional and I would not sign long contracts on them. The practical part is the techniques, which match today's Hugging Face and Bedrock items: most of an agent workload's cost is repeated context, so get caching and session affinity right first.

1. Nov 17 Hugging Face moves Inference Providers onto OpenRouter's router: open-weight models served by 80+ providers are callable with one Hugging Face token, with no markup
2. Nov 17 Open-weight models on Amazon Bedrock now support explicit prompt caching alongside implicit caching, through the Chat Completions API
3. Nov 16 tiny-voca is released under Apache 2.0: a 4.5M-parameter English TTS model that runs about 70x faster than real time on four CPU threads
