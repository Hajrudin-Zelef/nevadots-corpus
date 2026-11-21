---
id: collect-261121-veille-ia/muse/how-to-self-host-an-800b-parameter-open-model-like-meta-s-muse-spark-open-on-kubernetes-in-your-own-
title: "How to Self-Host an 800B+ Parameter Open Model (Like Meta's 'Muse Spark Open') on Kubernetes in Your Own Cloud - Qovery Blog"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: []
keywords: ["model", "open", "muse", "meta", "spark", "weights", "kubernetes", "bf16"]
source: https://www.qovery.com/blog/self-host-800b-parameter-open-model-kubernetes-own-cloud
source_anchor: ""
source_lines: [1, 198]
section: "Muse"
---
- **Muse Spark Open is real and you can download it.** Meta Superintelligence Labs released it on October 15, 2026: 816B total parameters, about 42B active per token, 256 experts, a 1M-token context, BF16 and NVFP4 weights, under the Muse Model License (Hugging Face model card).
- **An 816B model is a memory problem first.** Weights take about 1.63 TB at BF16, about 816 GB at FP8, and about 530 GB in Meta's NVFP4 build. Meta's own floor is "at least 700GB of VRAM for NVFP4 and 2TB for BF16" once you add KV cache and runtime overhead (model card).
- **The entry point is one 8-GPU node with more than 1 TB of HBM.** 8xH200 (1,128 GB), 8xB200 (1,440 GB), or 8xMI300X (1,536 GB). BF16 needs multiple nodes on InfiniBand or EFA, and that node boundary is where most attempts break.
- **The Kubernetes stack is standard.** NVIDIA GPU Operator, vLLM or SGLang, LeaderWorkerSet for multi-pod replicas, DCGM metrics, and a shared filesystem for the weights.
- **Most teams should call an API.** Together AI lists Muse Spark Open at $1.25 input and $4.25 output per million tokens as of October 2026 (Together AI pricing). An 8xH200 node on AWS costs $63.30 per hour on demand. Self-hosting only wins with sustained high utilization, data that cannot leave your VPC, or customization an endpoint cannot give you.
- **vLLM or SGLang serves the model; Qovery runs the cluster and deployments around it.** It deploys the serving stack on GPU node pools in your own AWS, GCP, Azure or Scaleway account or existing Kubernetes cluster and stops environments on a schedule. Qovery does not sell GPUs or host the model.

# How to Self-Host an 800B+ Parameter Open Model (Like Meta's "Muse Spark Open") on Kubernetes in Your Own Cloud

Meta released Muse Spark Open on October 15, 2026: 816B total parameters, 42B active, BF16 and NVFP4 weights. Here is how to self-host it, or any 400B-1T open-weights model, on Kubernetes in your own AWS, GCP, Azure, or Scaleway account: GPU memory math, node shapes, vLLM vs SGLang, hourly cost, and the utilization where self-hosting beats a hosted API.

People keep asking me how to self-host Meta's 816-billion-parameter model. Ten days ago I would have answered that Meta had not released one. Now it has, and the harder problem shows up: most teams asking cannot keep the node busy enough to pay for it.

This is the playbook I would follow. The math works for Muse Spark Open and for any other 400B-1T open-weights model, so it stays useful when the next one ships.

Yes. Meta Superintelligence Labs released Muse Spark Open on October 15, 2026 as "the open weights version of Muse Spark", an 816B-parameter sparse mixture-of-experts model that activates roughly 42B parameters per token (Meta developer blog). The weights sit in the gated `meta-models/Muse-Spark-Open-816B-A42B` repository on Hugging Face, which is a different org from the `meta-llama` org that hosts Llama.

The original Muse Spark, announced in April 2026, stayed closed and app-only (Meta newsroom). The open model comes with 62 layers, grouped-query attention (128 query heads, 16 KV heads, head dimension 64), and text, image, video, and audio input (model card).

Dense vs MoE is the sizing fact that matters most. **Total parameters decide how much HBM you need. Active parameters decide how many FLOPs each token costs.** An 816B MoE with 42B active needs the same memory as a dense 816B model, but each token costs about the compute of a 42B dense model. Memory is what you end up paying for.

Read the license before you design anything. The Muse Model License requires a "Built with Muse Spark Open" notice, and anyone selling it as a model-as-a-service with more than $20M revenue in 12 months has to sign a separate agreement with Meta (model card). Internal use is exempt from that clause.

| Model | Total params | Active per token | Architecture | Native / official precision | License | Weights at FP8 | Weights at BF16 | 
|---|---|---|---|---|---|---|---|
| Meta Muse Spark Open | 816B | ~42B | MoE, 256 experts, 8 active | BF16, NVFP4 (~530 GB) | Muse Model License | ~816 GB | ~1,632 GB | 
| Meta Llama 4 Maverick | 400B | 17B | MoE, 128 routed + 1 shared | BF16, FP8 | Llama 4 Community License | ~400 GB | ~800 GB | 
| Meta Llama 3.1 405B | 405B | 405B | Dense | BF16 | Llama 3.1 Community License (700M MAU clause) | ~405 GB | ~810 GB | 
| DeepSeek-V3 / R1 | 671B | 37B | MoE, 256 routed + 1 shared, 8 active, MLA | FP8 | MIT | ~671 GB | ~1,342 GB | 
| Moonshot Kimi K2 | ~1T | 32B | MoE, 384 experts, 8 active + 1 shared | FP8 | Modified MIT | ~1,000 GB | ~2,000 GB | 
| Qwen3-235B-A22B | 235B | 22B | MoE, 128 experts, 8 active | BF16 | Apache 2.0 | ~235 GB | ~470 GB | 

Sources: Llama 4 announcement, Llama 3.1 405B card and license, DeepSeek-V3, Kimi K2, Qwen3-235B-A22B. Llama 4 Behemoth (288B active, nearly 2T total) was announced as "still training" and has no public weights.

Weight memory is parameters x bytes per parameter. For 816B that gives 1.63 TB at BF16, 816 GB at FP8, and about 408 GB at a pure 4-bit format. Meta's NVFP4 build lands at about 530 GB because only the experts are quantized (Meta developer blog).

Then add the rest of the bill. Meta's stated floor of 700 GB for NVFP4 and 2 TB for BF16 is 23-32% above the weight size, which matches the 20-40% overhead I budget for KV cache, activations, and CUDA graphs.

**KV cache per token = 2 (K and V) x layers x KV heads x head dim x bytes.** For Muse Spark Open at BF16: 2 x 62 x 16 x 64 x 2 = about 254 KB per token. One 128K-token conversation holds about 33 GB of KV cache, and ten concurrent ones hold about 333 GB. Those are upper bounds, because Meta says the model uses sliding-window attention, which caps the cache on the layers that use it.

Attention design changes this by an order of magnitude:

- **Llama 3.1 405B** (126 layers, 8 KV heads, GQA): about 516 KB per token, or 68 GB for one 128K sequence. Without GQA, with 128 KV heads, it would be 16x larger.
- **DeepSeek-V3** (61 layers, MLA with a 512 + 64 compressed latent): about 70 KB per token, or 9 GB at 128K. DeepSeek reported that MLA cut KV cache by 93.3% versus its earlier dense model (DeepSeek-V2 paper).

Model configs: Llama 3.1 405B architecture, DeepSeek-V3 config. Use vLLM's PagedAttention so this memory actually gets used: the paper found earlier systems used only 20.4-38.2% of KV memory for real token state (vLLM paper).

Keep tensor parallelism inside a node and put pipeline or expert parallelism across nodes. NVLink gives each Hopper GPU 900 GB/s and each Blackwell GPU 1.8 TB/s (NVIDIA NVLink). An AWS p5en node gets 3,200 Gbps of EFA for all eight GPUs (AWS P5), which works out to 50 GB/s per GPU, 18x less than NVLink. Run tensor parallelism over that link and throughput collapses without any error message.

Quantization costs less quality than people fear. Red Hat ran over 500,000 evaluations on quantized Llama 3.1 models up to 405B and found every scheme kept over 99% of the BF16 baseline on the Open LLM Leaderboard v1 tasks, with INT4 W4A16 recovering 98.9% on HumanEval (Red Hat). Run your own evals on Muse Spark Open before you trust that number for it.

| Node shape (8 GPUs) | Total HBM | Muse Spark Open at BF16 (~1,632 GB) | Muse Spark Open at FP8 (~816 GB) | Muse Spark Open at NVFP4 (~530 GB) | KV per sequence, 32K / 128K (upper bound) | On-demand $/hour | 
|---|---|---|---|---|---|---|
| 8xH100 80 GB (AWS p5.48xlarge) | 640 GB | No | No | No, below Meta's 700 GB floor | 8 GB / 33 GB | $55.04 | 
| 8xH200 141 GB (AWS p5en.48xlarge) | 1,128 GB | No, needs 2+ nodes | Yes, ~310 GB left before overhead, tight | Yes, Meta names "eight H200s" | 8 GB / 33 GB | $63.30 | 
| 8xB200 180 GB (AWS p6-b200.48xlarge) | 1,440 GB | No, needs 2+ nodes | Yes, comfortable | Yes | 8 GB / 33 GB | $113.93 | 
| 8xAMD MI300X 192 GB | 1,536 GB | No, just short | Yes, with AMD's FP8 build (tuned for MI355X) | NVFP4 is NVIDIA-only | 8 GB / 33 GB | Varies by provider | 
| NVIDIA GB200 NVL72 (72 GPUs, one NVLink domain) | 13.4 TB | Yes, in one rack | Yes | Yes | 8 GB / 33 GB | Contract pricing | 

HBM per GPU: H100 80 GB, H200 141 GB, B200 180 GB, MI300X 192 GB, GB200 NVL72 13.4 TB. AWS us-east-1 Linux on-demand prices as of October 2026 come from the EC2 on-demand pricing page. Meta also says NVFP4 runs on four Blackwell Ultra GPUs.

Four instance families hold Muse Spark Open on a single node: AWS p5en, GCP A3 Ultra, GCP A4, and Azure ND H200 v5, all 8xH200 or 8xB200. Scaleway's largest H100 instance needs several nodes. As of October 2026, the GPU cost per hour runs from about $42 to about $114 per node, so the reservation model decides your bill more than the inference engine does.

| Instance | GPUs and HBM | Inter-node network | On-demand $/hour | Reserved / scheduled $/hour | One node holds 816B at FP8? | 
|---|---|---|---|---|---|
| AWS p5en.48xlarge | 8xH200, 1,128 GB | 3,200 Gbps EFA (3rd gen) | $63.30 | $63.16 (Capacity Block) | Yes | 
| AWS p5e.48xlarge | 8xH200, 1,128 GB | 3,200 Gbps EFA | Not offered on demand in the US regions checked | $54.92 (Capacity Block) | Yes | 
| AWS p6-b200.48xlarge | 8xB200, 1,440 GB | EFA | $113.93 | $113.67 (Capacity Block) | Yes | 
| GCP a3-ultragpu-8g (A3 Ultra) | 8xH200, 1,128 GB | Up to 3,600 Gbps | See GCP pricing | $42.40 Flex-start, $59.36 Calendar mode (DWS) | Yes | 
| GCP a4-highgpu-8g (A4) | 8xB200, 1,440 GB | Up to 3,600 Gbps | See GCP pricing | $64.44 Flex-start, $90.22 Calendar mode (DWS) | Yes | 
| Azure Standard_ND96isr_H200_v5 | 8xH200, 1,128 GB | 8 x 400 Gb/s InfiniBand (3.2 Tb/s per VM) | $84.80 (East US 2) | Reservations available | Yes | 
| Scaleway H100-SXM-8-80G | 8xH100, 640 GB | 20 Gbps | â¬25.33 | n/a | No, multi-node only | 

Sources: AWS P5, AWS Capacity Blocks pricing, GCP accelerator machines, GCP DWS pricing, Azure ND H200 v5, Azure retail prices API, Scaleway GPU pricing. Prices as of October 2026, Linux, before tax.

Plan capacity in weeks. EC2 Capacity Blocks can be booked up to eight weeks ahead for up to six months (AWS), and GCP sells 8-GPU shapes through Dynamic Workload Scheduler. Our GPU capacity guide covers where to look when your region is sold out.

Avoid spot for this model. Every reclaim means pulling 530 GB to 1.6 TB of weights back into HBM before you serve a token, and that takes minutes even from fast storage.

**Break-even:** divide the node price by your measured throughput. Say your benchmark shows 5,000 output tokens per second at full load on one 8xH200 node (a placeholder: measure your own):

| Node and price | Utilization | Output tokens per hour | Self-hosted $ per 1M output tokens | Together AI list price | 
|---|---|---|---|---|
| AWS p5en on demand, $63.30/h | 30% | 5.4M | $11.72 | $4.25 | 
| AWS p5en on demand, $63.30/h | 60% | 10.8M | $5.86 | $4.25 | 
| AWS p5en on demand, $63.30/h | 90% | 16.2M | $3.91 | $4.25 | 
| GCP A3 Ultra DWS Flex-start, $42.40/h | 60% | 10.8M | $3.93 | $4.25 | 

At that throughput you break even at 83% utilization on AWS on demand and at 55% on GCP Flex-start. Double the throughput and the threshold halves. Also count egress, a shared filesystem (FSx for Lustre, Filestore, or Azure NetApp Files), and nights and weekends when the node sits idle. In ClearML's 2024 survey, only 7% of companies said their GPUs exceed 85% utilization at peak (ClearML).

The stack has five layers, all open source: the NVIDIA GPU Operator, vLLM or SGLang, LeaderWorkerSet when one replica spans several pods, DCGM exporter with Prometheus, and a shared read-only filesystem for the weights.

- **GPU Operator** installs the driver, container toolkit, device plugin, GPU feature discovery, and DCGM monitoring (NVIDIA docs).
- **LeaderWorkerSet** is "an API for deploying a group of pods as a unit of replication" and reached v1.0.0 in October 2026 (kubernetes-sigs/lws). A Deployment or StatefulSet cannot express "one model, two nodes, restart together", so rolling updates and scaling break without it. vLLM documents a 2-node, 16-GPU LWS example (vLLM LWS docs).
- **Node pool:** a dedicated, tainted GPU pool in one placement group or cluster network, plus the EFA or RDMA device plugin.
- **Probes:** a startupProbe with a long failure threshold. A default liveness probe kills the pod mid-load and you get CrashLoopBackOff forever.
- **Metrics:** HBM used, tokens per second, time to first token, inter-token latency, queue depth, KV cache usage.
- **Weights:** stage them once on a shared filesystem or node-local NVMe, or stream them. Run:ai Model Streamer loaded an 8B model from S3 into GPU memory in 4.88 seconds versus 37 seconds for Tensorizer (benchmarks), and vLLM supports it natively (vLLM docs).

| Serving option | Multi-node | Parallelism | Muse Spark Open support | OpenAI-compatible API | Kubernetes fit | Best for | 
|---|---|---|---|---|---|---|
| vLLM | Yes, Ray or LWS | TP, PP, EP, DP | Verified by Meta on NVIDIA and AMD | Yes | LWS, KServe, llm-d | Default choice for most teams | 
| SGLang | Yes, `--nnodes` | TP, EP, prefill/decode disaggregation | BF16 and NVFP4 (Meta) | Yes | LWS, Helm | MoE throughput, prefix-heavy agent traffic | 
| TensorRT-LLM | Via NVIDIA Dynamo | TP, PP, EP | Single four-GPU Blackwell node (Meta) | Through Dynamo or Triton | NVIDIA operators | Peak NVIDIA performance | 
| NVIDIA Dynamo | Yes, datacenter scale | Disaggregated serving across engines | Meta's recommendation beyond one node | Yes | Kubernetes operator | Multi-node fleets | 
| Ray Serve (Anyscale) | Yes | Wraps vLLM | Through vLLM | Yes | KubeRay | Teams already on Ray | 
| llama.cpp / Ollama | No | Single host | Unsloth GGUF down to ~2-bit | Yes | Weak | Local tests, not production | 

Sources: vLLM parallelism docs, SGLang multi-node, Meta developer blog, NVIDIA Dynamo, llm-d, KServe.

**vLLM vs SGLang multi-node:** start with vLLM for its documentation and the LWS path. Move to SGLang when MoE throughput dominates your bill. LMSYS ran DeepSeek on 12 nodes of 8xH100 with prefill/decode disaggregation and large-scale expert parallelism and measured 22,282 output tokens per second per decode node (LMSYS).

There are seven steps: GPU node pool, GPU Operator, weights on shared storage, engine as a LeaderWorkerSet, private endpoint, benchmark, auto-stop.

1. **Node pool.** 8xH200 or 8xB200 instances in one placement group, tainted, with EFA or InfiniBand enabled.
2. **GPU Operator.** Check that nodes advertise`nvidia.com/gpu: 8` and the RDMA resource.
3. **Weights.** Accept the license on Hugging Face, download once, verify checksums, and store them on a filesystem inside your VPC.
4. **Engine.** Set tensor parallelism to GPUs per node and pipeline parallelism to node count, as vLLM recommends. Then tune`gpu-memory-utilization` and`max-model-len` , the two settings that actually control memory. Don't default to 1M context.
5. **Endpoint.** Expose`/v1/chat/completions` behind internal ingress with auth and rate limits. Never put it on a public LoadBalancer.
6. **Benchmark.** Use GuideLLM or vLLM's serving benchmark. Record tokens per second, TTFT, inter-token latency, and p99 under real concurrency.
7. **Auto-stop.** Schedule non-production GPU environments to stop at night before the first monthly bill arrives.

Three failures eat a week each. OOM during load comes from setting `gpu-memory-utilization` or the context length too high. A misconfigured NCCL or EFA setup across nodes gives a silent throughput collapse with no error in the logs. A liveness probe or Helm timeout kills the pod before the weights finish loading.

No, for most teams. Muse Spark Open is listed at $1.25 per million input tokens and $4.25 per million output tokens on Together AI, at the same list price through OpenRouter's Fireworks, Baseten, Modal, and Nebius endpoints, and with self-deploy options in SageMaker AI, Google Cloud Model Garden, and Microsoft Foundry (Together AI, OpenRouter, Meta). That ships this week.

Self-hosting is defensible for three reasons:

- **Regulated data** that contractually cannot leave your VPC, which matters for health, finance, and public-sector workloads under GDPR or the EU AI Act.
- **Sustained utilization** high enough to pass the break-even point above, measured on your own traffic.
- **Customization** an endpoint will not give you: your own LoRA stack, speculative decoding with a custom draft model, request routing, or modified attention.

Most teams land on a middle path: self-host a 30B-235B model that fits one node, and route the hardest queries to a frontier API. Meta's own Muse Glimmer 30B, distilled from Muse Spark (OpenRouter), and Qwen3-235B-A22B both fit that role. Quality per dollar often favors a well-served 235B MoE over a half-idle 816B.

| Approach | Main cost driver | Cost per 1M output tokens | Data residency | Customization | Ops burden | Time to production | 
|---|---|---|---|---|---|---|
| Self-hosted Muse Spark Open in your own cloud | GPU hours, idle or not | $3.91 at 90% to $11.72 at 30% in the p5en example | Stays in your VPC | Full | GPU nodes, NCCL, drivers, on-call | Weeks | 
| Hosted API (Together AI, Fireworks AI, Baseten, Modal, Nebius) | Tokens | $4.25 list for Muse Spark Open | Provider's region and terms | Limited | None | Same day | 
| Hosted catalogs (SageMaker AI, Vertex AI Model Garden, Microsoft Foundry) | Dedicated instance hours | Similar to self-hosting | Your cloud provider | Medium | Low to medium | Days | 
| Hybrid routing | Small self-hosted model + API fallback | Blended | Sensitive traffic stays home | High where it matters | One GPU node | 1-2 weeks | 

This is not a Qovery pitch. If an API is right for you, use an API.

Qovery deploys and operates the serving stack inside your own AWS, GCP, Azure, or Scaleway account, or on your existing Kubernetes cluster. The weights, the VPC, and the GPU reservations stay yours, and your team deploys without a dedicated DevOps engineer. **Qovery does not sell GPUs, does not host models, and is not an inference platform.** vLLM or SGLang serves the model, and Qovery runs the cluster and the deployments around it.

- **Your cloud account (BYOC).** Capacity Blocks, DWS reservations, and committed-use discounts stay in your name, and that is the whole economic case for self-hosting.
- **Kubernetes without managing it directly.** Qovery-managed EKS clusters have a GPU node pool option, and you can attach an existing cluster on any provider, so the GPU Operator and LeaderWorkerSet slot in normally.
- **Git-push deployments** for the serving layer and the app in front of it, plus preview environments for the application tier.
- **Access control per environment** , so only the ML platform team touches the GPU environment while product teams deploy against its endpoint.
- **Environment auto-stop.** Deployment rules start and stop environments on a schedule. When a node bills $63-114 per hour and nobody works at 3am, this is the biggest saving you have.
- **Managed cluster upgrades** , because CUDA, driver, and Kubernetes version drift is how a working GPU cluster stops working.

For a smaller model on the same pattern, see our guide to running multiple open-weight models on one GPU cluster.

Install the Qovery Agent Skills with one curl command, then ask your coding agent in plain language to deploy the inference service into your own cloud account. The skills handle onboarding, deployment, troubleshooting, and cost optimization. The MCP Server operates what already exists and is read-only by default.

```
curl -fsSL https://skill.qovery.com/install.sh | bash
# Or scope the install to the current project
curl -fsSL https://skill.qovery.com/install.sh | bash -s -- --project
# Add the Qovery MCP Server to Claude Code (OAuth, read-only by default)
claude mcp add --transport http qovery https://mcp.qovery.com/mcp --callback-port 4242
# Write access, only when you want it (also enable it in the Qovery Console)
claude mcp add --transport http qovery "https://mcp.qovery.com/mcp?read_write=true" --callback-port 4242
```
The skills that matter here are `qovery-onboard` to connect your cloud account, `qovery-deploy` to ship, `qovery-troubleshoot` for failing pods, `qovery-optimize` and `qovery-speedup` for cost and build time, `qovery-preview` for preview environments, and `qovery-terraform` for infrastructure as code. Then ask: "deploy this vLLM service to my AWS account with Qovery and schedule it on the GPU node pool."

Details: AI agent quickstart and Claude Code integration.

## How do I self-host Meta's Muse Spark Open model (816 billion parameters) on my own cloud with Kubernetes?

Accept the Muse Model License on Hugging Face, then serve the NVFP4 weights with vLLM or SGLang on one 8xH200 or 8xB200 node in your own AWS, GCP, Azure, or Scaleway account. Install the NVIDIA GPU Operator, stage the weights on shared storage, and give the pod a long startup probe. BF16 needs multiple nodes, and LeaderWorkerSet ties them into one replica.

## How many H100 or H200 GPUs do I need to run an 816B-parameter model?

Eight H200s, per Meta's guidance for the NVFP4 build. Eight H100s (640 GB) fall below Meta's 700 GB minimum, so H100s need two nodes. BF16 needs about 2 TB, which means at least two 8xH200 nodes on InfiniBand or EFA.

## Can an 800B-parameter model run on a single 8-GPU node?

Yes, at FP8 or 4-bit precision on a node with more than 1 TB of HBM: 8xH200 (1,128 GB), 8xB200 (1,440 GB), or 8xMI300X (1,536 GB). At BF16 the weights alone are 1.63 TB, so no single 8-GPU node of those types holds it.

## How much does it cost per hour to self-host an 800B-parameter model on AWS, GCP, Azure, or Scaleway?

About $42 to $114 per node-hour as of October 2026. AWS p5en (8xH200) is $63.30 on demand, GCP A3 Ultra is $42.40 on DWS Flex-start, and Azure ND H200 v5 is $84.80 in East US 2. Scaleway's 8xH100 is â¬25.33 but needs several nodes for an 816B model.

## Is vLLM or SGLang better for multi-node LLM inference on Kubernetes?

vLLM is the safer default: Meta verified it for Muse Spark Open on NVIDIA and AMD, and it documents multi-node serving with Ray and LeaderWorkerSet. SGLang often wins on MoE throughput and prefix-heavy agent traffic thanks to RadixAttention and prefill/decode disaggregation. Benchmark both on your own traffic.

## Does Qovery host the GPUs and the model, or do I bring my own cloud account?

You bring your own cloud account. Qovery does not sell GPUs or host models. It deploys and operates the Kubernetes cluster and your services, including vLLM or SGLang, inside your AWS, GCP, Azure, or Scaleway account or your existing cluster.

Romaric founded Qovery to make Kubernetes accessible to every engineering team. He writes about platform strategy, developer experience, and the future of cloud infrastructure.

### Run your own models, on your own cloud account.

Qovery gives your team self-service deployments on your own AWS, GCP, Azure, or Scaleway account, or your existing Kubernetes cluster, GPU node pools included. Start deploying in under 10 minutes.
