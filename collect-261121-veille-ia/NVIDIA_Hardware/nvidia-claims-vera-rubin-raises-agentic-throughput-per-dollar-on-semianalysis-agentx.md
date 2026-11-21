---
id: collect-261121-veille-ia/nvidia-hardware/nvidia-claims-vera-rubin-raises-agentic-throughput-per-dollar-on-semianalysis-agentx
title: "NVIDIA claims Vera Rubin raises agentic throughput per dollar on SemiAnalysis AgentX"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: ["2026-11-05"]
keywords: ["nvidia", "says", "rubin", "vera", "claims", "throughput", "dollar", "context"]
source: https://tensorjournal.com/en/news/nvidia-vera-rubin-agentic-throughput-claims
source_anchor: ""
source_lines: [1, 30]
section: "NVIDIA_Hardware"
---
NVIDIA says HBM capacity per GPU changes by roughly 3% from Blackwell Ultra to the Vera Rubin platform, in an announcement dated November 5, 2026. The same announcement sets out several platform comparisons, which NVIDIA presents as its own results.

On SemiAnalysis AgentX, NVIDIA claims agentic throughput per dollar rises by up to 30x from Blackwell Ultra to Vera Rubin. For DeepSeek V4 Pro, it claims that at one hundred tokens per second per user, throughput per dollar on the GB300 NVL72 is up to 10x higher compared with the H200 platform.

In a separate comparison at one hundred twenty-five tokens per second per user, NVIDIA claims throughput per dollar on Vera Rubin NVL72 is up to 30x higher compared with the GB300 NVL72. The two comparisons use different per-user rates, so they do not describe the same operating point.

NVIDIA also says HBM capacity per GPU on the GB300 NVL72 is roughly doubling that of the H200. It describes the maximum agentic trajectory context processed as up to 3 million tokens, and says weights can be distributed across up to 72 GPUs in an NVL72 system.

For model weights, NVIDIA says NVFP4 takes less space than FP8. That comparison is the company's own claim and has not been independently verified.

For the KV cache, NVIDIA says applying NVFP4 instead of FP8 reduces its footprint, with the effect that more context stays resident in HBM.

NVIDIA says its Dynamo inference software moves context that is not actively in use to NVIDIA CMX context memory storage, with inactivity as the trigger. It also describes Dynamo KV Cache Reuse as managing the KV cache as one system-wide resource across GPU memory, CPU memory and storage.

NVIDIA lists nine named partners for CMX: Cloudian, DDN, Dell Technologies, Everpure (formerly Pure Storage), HPE, IBM, NetApp, VAST and WEKA.

NVIDIA states that a RAG knowledge base never occupies HBM, and that only the passages the model needs enter context. NVIDIA claims its Nemotron Retriever models produce smaller embeddings and higher retrieval accuracy; that claim has not been independently established.

For paused context, NVIDIA describes the Vera CPU's large LPDDR memory, linked to Rubin GPUs by NVLink-C2C, as a nearby place to wait, with quick return to the GPUs.

NVIDIA also claims it was first to adopt HBM3e and HBM4 into an accelerated computing stack, a priority claim that rests on NVIDIA's statement alone.

## Related

- News NVIDIA says each Rubin GPU has up to 288 GB of HBM4 memory; its NVFP4 footprint and throughput comparisons are company-reported.
- News Every figure is NVIDIA's own claim; the agents-per-watt multiple is recorded as not independently verified.
- News NVIDIA dates its account November 4, 2026 and says customers can request access to its developed cluster operations agents through an account manager.
- News NVIDIA says Vera Rubin support is ready for enterprise production. In this release, Tile-SIMT interoperability is a public preview covering library functions only.
- News In NVIDIA's company-reported example, development and held-out pass rates rise from 3/6 to 6/6 and from 4/6 to 6/6 after the harness change.
- News NVIDIA says an open-source implementation, NCCL M2N Reshard, is on GitHub, and shrink and grow resiliency support is planned for future updates.
