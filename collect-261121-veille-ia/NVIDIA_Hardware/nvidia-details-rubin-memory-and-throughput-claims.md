---
id: collect-261121-veille-ia/nvidia-hardware/nvidia-details-rubin-memory-and-throughput-claims
title: "NVIDIA details Rubin memory and throughput claims"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: ["2026-11-04"]
keywords: ["nvidia", "rubin", "claims", "says", "news", "vera", "throughput", "nvfp4"]
source: https://tensorjournal.com/en/news/nvidia-rubin-memory-ai-factory-claims
source_anchor: ""
source_lines: [1, 26]
section: "NVIDIA_Hardware"
---
NVIDIA says it set out its co-design account on November 5, 2026, covering Rubin GPUs, Vera CPUs, NVLink and software for managing cached model context.

For a Rubin GPU, NVIDIA lists HBM4 capacity of up to 288 GB, with HBM bandwidth of 19.2 TB/s.

NVIDIA claims the Vera CPU is 1.8x faster single-threaded performance compared with the prior generation, and lists up to 1.5 TB of LPDDR5X per CPU. The NVLink-C2C link between the Vera CPU and Rubin GPU has 1.8 TB/s of bandwidth.

NVIDIA says NVFP4 reduces model-weight footprint relative to FP16 by approximately 3.5x. In its company-reported comparisons across benchmarks and mixture-of-experts architectures, NVFP4 reaches up to 4x the throughput of FP8 at the same interactivity. NVIDIA also claims that NVFP4 cuts KV-cache footprint compared with FP8 by up to 50%.

On SemiAnalysis AgentX, NVIDIA claims Vera Rubin NVL72 delivers up to 30x throughput per megawatt compared with GB300 NVL72, at the per-user token rate the benchmark specifies. The figure is NVIDIA's own result, not an independent measurement.

NVIDIA's wide expert-parallel example uses DeepSeek-R1 at EP64, where each GPU holds four experts per layer.

NVIDIA gives aggregate NVLink bandwidth of 130 TB/s for GB200 NVL72.

In NVIDIA Dynamo, the router tracks where KV blocks reside and uses cached-prefix overlap and worker load as placement inputs. Its KVCR block-location tracking covers configured tiers only: GPU, pinned CPU and storage tiers such as local SSDs and mounted network filesystems.

NVIDIA also says a new G3.5 tier for CMX context memory storage uses Ethernet-attached flash shared at pod level.

## Related

- News Most figures are NVIDIA's own claims. The agentic throughput-per-dollar increase of up to 30x is reported on SemiAnalysis AgentX, and the NVFP4 weight comparison has not been independently verified.
- News Every figure is NVIDIA's own claim; the agents-per-watt multiple is recorded as not independently verified.
- News NVIDIA dates its account November 4, 2026 and says customers can request access to its developed cluster operations agents through an account manager.
- News NVIDIA says Vera Rubin support is ready for enterprise production. In this release, Tile-SIMT interoperability is a public preview covering library functions only.
- News In NVIDIA's company-reported example, development and held-out pass rates rise from 3/6 to 6/6 and from 4/6 to 6/6 after the harness change.
- News NVIDIA says an open-source implementation, NCCL M2N Reshard, is on GitHub, and shrink and grow resiliency support is planned for future updates.
