---
id: collect-261121-veille-ia/docs-techniques/hardware-platforms
title: "Hardware platforms"
domain: docs-techniques
role: reference
task: article
actors: []
dates: []
keywords: ["hardware", "nvidia", "runtime", "inference", "tensorrt-llm", "platforms", "muse", "spark"]
source: https://dev.meta.ai/docs/muse-spark-open/hardware-platforms?team_id=2096920474558192&project_id=1661600634933790
source_anchor: ""
source_lines: [1, 17]
section: "Docs_Techniques"
---
Run Muse Spark Open on NVIDIA, AMD, Intel, and Apple hardware. Each section lists the checkpoints, parallelism, and runtime for its platform, with sample commands. Pick a platform to jump to it.

For SGLang and vLLM on NVIDIA GPUs, see Run inference.

Validated serving configurations for Muse Spark Open are maintained in the runtime recipe databases; start from those rather than hand-tuning.

Hardware notes not covered by the recipe sources:

The NVFP4 checkpoint fits on a single NVIDIA Blackwell (`SM100` family) 4-GPU node, with attention tensor-parallel 4-way and the 256 routed experts sharded 4-way. The configuration above was validated on GB300 NVL with TensorRT-LLM. The FP8 KV cache is required on `SM103` when chunked prefill or KV-cache block reuse is enabled, and the recommended TensorRT-LLM configuration enables both; see Deploy with TensorRT-LLM.

Throughput mode:

Lowest latency mode:

MLX splits the model across the Macs over RDMA on Thunderbolt 5, which needs macOS 26.2 or later and a direct link between every pair of Macs. For setup and commands, see Run inference › MLX.

For every runtime and its full flag set, see Run inference. To size parallelism and context length for your workload, see Recommended settings.
