---
id: collect-261121-veille-ia/kimi/how-to-self-host-kimi-k3-1-pro-2-8t-parameters-on-your-own-kubernetes-cluster-memory-per-precision-g
title: "How to self-host Kimi K3.1 Pro (2.8T parameters) on your own Kubernetes cluster: memory per precision, GPU instances and multi-node serving - Qovery Blog"
domain: kimi
role: reference
task: article
actors: ["Moonshot AI"]
dates: ["2026-10-24"]
keywords: ["kimi", "memory", "parameters", "precision", "kubernetes", "model", "checkpoint", "nodes"]
source: https://www.qovery.com/blog/kimi-k3-1-pro-self-host-kubernetes-gpu-memory-multi-node
source_anchor: ""
source_lines: [1, 215]
section: "Kimi"
---
- **Kimi K3.1 Pro is real, open-weight, and big.** Moonshot AI's model has 2.8T total parameters and 104B active per token, 896 experts (16 routed per token plus 2 shared), a 1,048,576-token context and native image input. The official checkpoint ships as MXFP4 weights trained with quantization-aware training and weighs 1,561 GB on Hugging Face.
- **Size memory from total parameters and the shipped precision.** MXFP4 (official): ~1.56 TB. FP8 would be ~2.8 TB and BF16 ~5.6 TB, and neither checkpoint exists. In practice you serve the MXFP4 checkpoint or NVIDIA's NVFP4 variant (1,605 GB).
- **One 8x B300 node is the smallest single-node layout the engines document.** vLLM, NVIDIA NIM and SGLang all publish 8-GPU Blackwell Ultra recipes. On AWS that is`p6-b300.48xlarge` at $142.416/hour in us-east-1. Two 8x B200 nodes or four 8x H100 nodes work too, with a multi-node fabric.
- **Multi-node on Kubernetes means LeaderWorkerSet plus RDMA.** vLLM's recipe runs two nodes with`--nnodes 2 --node-rank` and a headless worker. Without EFA or InfiniBand between nodes, tensor parallel across nodes collapses.
- **The license allows commercial self-hosting, with two thresholds.** A model-as-a-service business above $20M in revenue over 12 months needs a separate agreement, and products above 100M monthly active users or $20M monthly revenue must display "Kimi K3.1-Pro" (license). Internal use is exempt.

# How to self-host Kimi K3.1 Pro (2.8T parameters) on your own Kubernetes cluster: memory per precision, GPU instances and multi-node serving

Kimi K3.1 Pro is a 2.8-trillion-parameter open-weight MoE with a 1,561 GB MXFP4 checkpoint. Here is the memory math per precision, which GPU instances on AWS, Google Cloud, Azure, Scaleway and OVHcloud can hold it, and how to serve it on one node or several with vLLM, SGLang, NIM and LeaderWorkerSet on Kubernetes.

Kimi K3.1 Pro landed on Hugging Face in the first days of November and the first question customers asked me was "what do I need to rent to run it?". I'm Guillaume, Staff Solutions Architect at Qovery, and I spend my days helping teams put GPU workloads on Kubernetes in their own cloud accounts. This is the sizing sheet I wish I had on day one, with every number linked to the vendor or project that published it.

We already covered the general playbook in how to self-host an 800B+ open model on Kubernetes and the NVIDIA side in self-hosting open-weight models with the model-free NIM container. This one is narrower. One model, the exact memory per precision, the instances that hold it, and how to split it across nodes when one is not enough.

Kimi K3.1 Pro is Moonshot AI's 2.8-trillion-parameter Mixture-of-Experts model, and yes, the full weights are downloadable from Hugging Face under a custom "Kimi K3.1 Pro License". Moonshot introduced the model through its API on October 24, 2026 and published the weights on November 4 (TechNode). The Hugging Face repository itself was created on November 2 and last updated on November 5 according to the Hugging Face API.

Here is what the model card and its `config.json` state:

| Spec | Value | What it means for serving | 
|---|---|---|
| Total parameters | 2.8T | Drives the memory footprint. Every expert sits in GPU memory. | 
| Active parameters | 104B | Drives compute per token, not memory | 
| Experts | 896 routed, 16 per token, 2 shared | High expert count favours expert parallelism at scale | 
| Layers | 93 (69 Kimi Delta Attention + 24 full attention with MLA and a DSA indexer) | Only 24 layers keep a per-token KV cache | 
| Context | 1,048,576 tokens | Long context is cheap in KV terms, see below | 
| Weight precision | MXFP4 weights, MXFP8 activations, quantization-aware training | The 4-bit checkpoint is the reference, not a lossy afterthought | 
| Vision | MoonViT-V2 encoder, 401M parameters | Text and image input | 
| Checkpoint | 1,561 GB across the repository | Plan local NVMe or a shared filesystem above 1.6 TB | 
| License | Kimi K3.1 Pro License ( `other` on Hugging Face) | Commercial use with revenue and MAU thresholds | 

The architecture string in `config.json` is `KimiK31ForConditionalGeneration`. That string is how you check engine support before you rent a single GPU.

Moonshot recommends vLLM, SGLang and TokenSpeed as inference engines on the card. It also sells the model through its own API at $2.00 per million input tokens and $10.00 per million output tokens, with batch inference at 60% of the standard price (Kimi API pricing). Keep those numbers in mind for the break-even section.

The license grants the usual MIT-style rights ("use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies") with two commercial conditions (LICENSE):

- If you run a model-as-a-service business with aggregate revenue above $20 million over any 12 consecutive months, you must negotiate a separate agreement with Moonshot AI before commercial use.
- If your product has more than 100 million monthly active users or more than $20 million in monthly revenue, you must prominently display "Kimi K3.1-Pro" in the user interface.

Internal use, where third parties do not access the model, is exempt from both. For the platform teams I talk to, who want an internal coding assistant or a document pipeline inside their own VPC, neither threshold applies. If you resell inference, have your lawyer read the license.

Kimi K3.1 Pro needs about 1.56 TB of GPU memory for its official MXFP4 weights, before KV cache and runtime overhead. Multiply total parameters by bytes per parameter. Active parameters do not reduce the footprint, because the router can pick any of the 896 experts on the next token.

| Precision | Bytes per parameter | Weight footprint | Checkpoint exists? | Smallest layout that holds it | 
|---|---|---|---|---|
| BF16 | 2 | ~5.6 TB (2.8T x 2) | No, derived only | Three 8x B300 nodes (6.9 TB), with little room left | 
| FP8 | 1 | ~2.8 TB | No, derived only | Two 8x B300 nodes (4.3-4.6 TB) | 
| MXFP4 (official) | ~0.53 (4-bit values plus one 8-bit scale per 32 weights) | 1,561 GB measured | Yes, moonshotai/Kimi-K3.1-Pro | One 8x B300 node, two 8x B200 nodes, or four 8x H100 nodes | 
| NVFP4 (NVIDIA) | ~0.56 (4-bit values plus FP8 scales) | 1,605 GB measured | Yes, nvidia/Kimi-K3.1-Pro-NVFP4, published November 11 | Same as MXFP4, targets Blackwell FP4 tensor cores | 

The MXFP4 group size of 32 comes from the `quantization_config` in `config.json`. The measured checkpoint sizes come from the Hugging Face API for each repository. The 1,561 GB also includes the vision encoder and the layers Moonshot kept in higher precision, which is why it sits above the 1.49 TB you get from 2.8T x 4.25 bits.

I would not chase BF16 or FP8. Moonshot trained the model with quantization-aware training at MXFP4, so a higher-precision copy buys you more GPUs and no published quality gain.

Much less than you would expect for a model this size, because only 24 of the 93 layers are full-attention layers with a per-token cache. The other 69 are Kimi Delta Attention layers, a linear-attention design that keeps a fixed-size state per sequence instead of a cache that grows with every token.

My estimate from `config.json`, so treat it as an estimate:

- **Per-token cache.** The 24 MLA layers each cache a 512-wide compressed latent (`kv_lora_rank` ), and SGLang forces an FP8 KV cache for this model (SGLang cookbook). That is about 24 x 512 bytes, roughly 12 KB per token, plus the DSA indexer keys (128 values per layer). Call it 12-16 KB per token. A full 1,048,576-token sequence then needs about 13-17 GB.
- **Per-sequence state.** The 69 KDA layers each keep 96 heads of a 128 x 128 state matrix. At 2 bytes per value that is about 217 MB per active sequence, regardless of its length, and about 434 MB if the engine keeps the state in FP32.

So on one 8x B300 node with roughly 580-740 GB left after the weights, concurrency runs out long before context length does. A hundred concurrent agent sessions cost you tens of gigabytes of KDA state before the first long prompt arrives. Budget 10-20% of HBM for activations, CUDA graphs and the vision encoder, then size `--max-num-seqs` against what is left.

The cleanest fit is a single 8x B300 node: 8 x 288 GB of HBM3e (NVIDIA) holds the 1,561 GB checkpoint with room for KV cache and state. Everything smaller than B300 forces you onto two or more nodes, which means an RDMA fabric and a harder deployment.

Prices below are on-demand list prices I read on November 15, 2026, excluding tax.

| Provider | Instance | GPUs and memory | Fits on one node? | List price | Where | 
|---|---|---|---|---|---|
| AWS | `p6-b300.48xlarge` | 8x B300, 2,144 GB | Yes, ~580 GB headroom | $142.416/h ($17.80 per GPU-hour) | us-east-1, us-west-2; London at $217.54/h (AWS on-demand) | 
| AWS | `p6-b200.48xlarge` x2 | 2 x 8x B200, 2,864 GB | No, two nodes over EFA | 2 x $113.9328/h | us-east-1, us-east-2, us-west-2, Stockholm, Paris, Mumbai | 
| Google Cloud | `a4-highgpu-8g` x2 | 2 x 8x B200, 2,880 GB | No, two nodes | Reservation-based | GPU machine types | 
| Google Cloud | `a4x-maxgpu-4g-metal` x2 | 2 x 4x B300 (GB300), 2,232 GB | No, two trays over NVLink | Reservation-based | GPU machine types | 
| Azure | `Standard_ND128isr_GB300_v6` x2 | 2 x 4x B300, 2,304 GB | No, two VMs over NVLink and 800 Gbps InfiniBand per GPU | Pricing calculator | ND GB300 v6 | 
| Scaleway | `B300-SXM-8-288G` | 8x B300, 288 GB each | Yes | â¬60/h (â¬1 per minute) | fr-par-2 only (Scaleway catalog), listed as "shortage" by the availability API on November 15 | 
| OVHcloud | `h200-1920` | 8x H200, 1,128 GB | No, and 25 Gbit/s private network is too thin for multi-node | $49.56/h | EU-WEST-PAR (OVHcloud prices) | 
| OVHcloud | B300 | Announced for November | Not yet orderable from the price page | Not published | OVHcloud Q3 update | 

AWS lists the B300 node at 2,144 GB of GPU memory rather than 2,304 GB, so I used AWS's figure for its row (EC2 P6). The GB300-based instances on Google Cloud and Azure carry four GPUs per VM, so even with B300 silicon you need two of them, joined by NVLink inside the rack.

Two layouts I would skip:

- **One 8x H200 node (1,128 GB).** It does not hold the 1,561 GB checkpoint. You need two, and Hopper has no native FP4 tensor cores, so you pay for memory without getting the FP4 compute.
- **Two Scaleway or OVHcloud nodes.** Both list 20-25 Gbit/s between instances. Tensor parallel across nodes on that link will crawl. These are single-node clouds for this model.

If you are comparing B300 prices specifically across AWS, Scaleway and OVHcloud, my colleague MÃ©lanie wrote the B300 price comparison, with commitment discounts and egress.

Start with vLLM. It had day-0 support on November 4, a published B300 single-node recipe, and a two-node recipe you can map onto Kubernetes directly. Pick SGLang when you want its prefill/decode and context-parallel modes for throughput, and NVIDIA NIM when your organization standardizes on NVIDIA's supported containers.

| Engine | Kimi K3.1 Pro support | Image or version | Single-node layout | Multi-node layout | Notes | 
|---|---|---|---|---|---|
| vLLM | Day 0, Nov 4 blog | `vllm/vllm-openai:kimi-k31` , vLLM >= 0.34.0 nightly (recipe) | 8x B300, TP8 | 2 nodes of GB300 NVL4 trays, TP8, `--nnodes 2` | Also AMD MI355X (TP8) and TPU v7x | 
| SGLang | Day 0 branch `kimi-k3.1-day-0` | `lmsysorg/sglang:dev-kimi-k31-pro` (cookbook) | MI355X 1 node | B300/GB300 two trays (2x4 over MNNVL); H100 TP32 Â· EP32 | Forces FP8 KV, page size 64 | 
| NVIDIA NIM (experimental) | Validated on B300 and H20-3e | `nvcr.io/nim/nvidia/vllm-model-free-nim-experimental:2.1.6-variant-kimi-k3.1-pro` (support matrix) | 8x B300-SXM6-AC, TP8, PP1 | H20-3e, 2 nodes PP2 or 4 nodes PP4, RDMA required | GB300 not supported in this release | 
| llm-d | No Kimi K3.1 Pro guide found | Wide expert parallelism path on vLLM (llm-d docs) | n/a | LeaderWorkerSet, disaggregated prefill/decode | Worked example is DeepSeek-R1; adapt it yourself | 

This is a change from a week ago. When we published the NIM container guide, Kimi K3.1 Pro was not on NVIDIA's list. As of the support matrix updated on November 14, NVIDIA ships an experimental variant image with a single-node B300 recipe. The general model-free NIM idea still holds: the container can only serve what its bundled engine supports, so check the architecture string first. We also explain the vLLM and SGLang NIM containers in this Kubernetes walkthrough.

vLLM published numbers for GB300 and Rubin only. On 8x GB300 with TP8, vLLM reports 375.9 tokens per second per user at the latency-sensitive end and 37.8K total tokens per second per GPU at the throughput end (vLLM). Expect your B300 node to land in the same family, and measure it on your own prompts before you size a fleet.

On one 8x B300 node, Kimi K3.1 Pro is a plain Kubernetes Deployment with one pod that requests eight GPUs and runs vLLM with tensor parallel 8. No cross-node networking, no LeaderWorkerSet.

The vLLM blog's NVIDIA command is the whole engine config:

```
vllm serve moonshotai/Kimi-K3.1-Pro \
  --tensor-parallel-size 8 \
  --trust-remote-code \
  --load-format fastsafetensors
```
The recipe adds `--enable-auto-tool-choice --tool-call-parser kimi_k3 --reasoning-parser kimi_k3` for agent workloads. On Kubernetes I wrap it like this:

1. **GPU node pool.** One 8x B300 node type, tainted so nothing else lands on it, with the NVIDIA GPU Operator installed. Check that the node advertises`nvidia.com/gpu: 8` .
2. **Weights on local NVMe.** The`p6-b300.48xlarge` has 8 x 3.84 TB of local NVMe. Download the 1,561 GB once per node into a hostPath or a local PersistentVolume, or stage it on a shared filesystem in the same region. Pulling 1.56 TB from the internet on every pod restart is how you lose an afternoon.
3. **Startup probe with patience.** Loading 1.56 TB takes a while even with`fastsafetensors` . Give the startupProbe a failure threshold measured in tens of minutes, or the kubelet kills the pod mid-load.
4. **Context and concurrency limits.** Set`--max-model-len` to what your workload needs, not to 1,048,576 by default, and cap`--max-num-seqs` against the KDA state math above.
5. **Private endpoint.** Expose the OpenAI-compatible`/v1` API behind an internal ingress with auth. Never on a public LoadBalancer.
6. **Benchmark.** Run your real prompts at your real concurrency, then record tokens per second, time to first token and p99 latency.

Kimi K3.1 Pro always reasons. SGLang exposes a `reasoning_effort` field with `low`, `high` or `max`, defaulting to `max` (SGLang cookbook). Long reasoning traces cost output tokens, so set the effort per use case before you compare costs.

When one node cannot hold the model, you run one model replica across several pods, one per node, and you manage them as a group with LeaderWorkerSet. Each pod gets a rank, the leader pod exposes the API, and if one pod dies the whole group restarts together.

vLLM's two-node recipe for GB300 trays shows the pattern (vLLM recipe):

```
vllm serve moonshotai/Kimi-K3.1-Pro --trust-remote-code \
  --tensor-parallel-size 8 --nnodes 2 --node-rank 0 \
  --master-addr $HEAD_IP --load-format fastsafetensors
# Worker node (rank 1): same command with --node-rank 1 and --headless
```
Mapping that onto Kubernetes:

- **LeaderWorkerSet.** Define a group size equal to the node count. The leader template runs rank 0 and the worker template runs`--headless` with its rank from the LWS index. A Deployment or StatefulSet cannot express "these two pods are one replica" (kubernetes-sigs/lws).
- **Parallelism choice.** On GB300 or GB200 trays joined by NVLink, tensor parallel across trays works because the link is NVLink. On separate HGX nodes joined by EFA or InfiniBand, keep tensor parallel inside the node and use pipeline or expert parallel across nodes. NVIDIA's H20-3e recipes do exactly this: TP8 per node, PP2 or PP4 across nodes, and the support matrix states that "H20 requires a working RDMA fabric".
- **Fabric.** On AWS that means EFA, with the EFA device plugin and the nodes in one cluster placement group. The`p6-b200.48xlarge` lists 3.2 Tbps of network bandwidth and the`p6-b300.48xlarge` 6.4 Tbps (EC2 P6). Without RDMA, NCCL falls back to TCP and throughput collapses without an obvious error.
- **Gang scheduling.** All pods of a group must land together. A half-scheduled group holds expensive GPUs idle while it waits.
- **Wide expert parallelism at scale.** With 896 experts, Kimi K3.1 Pro is a natural fit for llm-d's wide-EP path: attention with data parallelism, MoE layers with expert parallelism, prefill and decode disaggregated. It is the most efficient layout at high request volume and the most complex to run. Start with TP8 on one B300 node and move here only when traffic justifies it.

| Layout | Nodes | Interconnect needed | Complexity | When to use it | 
|---|---|---|---|---|
| TP8, one 8x B300 node | 1 | None across nodes | Low | Default for most teams | 
| TP8 over two GB300 NVL4 trays | 2 | NVLink (rack-scale) | Medium | Google Cloud A4X Max, Azure ND GB300 v6 | 
| TP8 per node + PP2 | 2 | EFA or InfiniBand | Medium-high | Two 8x B200 nodes, two 8x H200 nodes | 
| TP32 Â· EP32 | 4 | EFA or InfiniBand | High | 8x H100 fleets you already own (SGLang) | 
| Wide EP with P/D disaggregation (llm-d) | 4+ | RDMA, NIXL | Highest | Sustained high volume, many replicas | 

For most teams, the API is cheaper until GPU utilization is high and steady. One `p6-b300.48xlarge` on-demand in us-east-1 runs 730 hours a month at $142.416, about $103,964. At $10 per million output tokens on the Kimi API, that buys about 10.4 billion output tokens a month, before you count input tokens or the 60% batch price.

On commitment, AWS's own price list shows the same node at $112.15/hour on a 1-year no-upfront Compute Savings Plan and $53.55/hour on a 3-year all-upfront EC2 Instance Savings Plan in us-east-1 (AWS Savings Plans price list). Scaleway's B300 node in Paris is â¬60/hour, about â¬43,800 a month. The break-even moves a lot with those choices.

Self-hosting wins on three things the API cannot give you:

- **Data that cannot leave your account.** Prompts, documents and outputs stay in your VPC, in the region you picked.
- **Steady, high utilization.** An internal coding agent fleet or a document pipeline that keeps the node busy around the clock.
- **Control over the model.** Your own reasoning effort defaults, routing, LoRA adapters or a pruned variant, and no surprise deprecation.

If none of those apply, call the API. If they apply only to some traffic, route the sensitive part to your own node and the rest to the API. That hybrid is what I recommend most often.

Qovery creates and operates the Kubernetes cluster and the GPU node pools inside your own AWS, GCP, Azure or Scaleway account, or installs on a cluster you already run, so your team ships the vLLM service without a dedicated DevOps engineer. Qovery does not host the model and does not sell GPUs. vLLM or SGLang serves Kimi K3.1 Pro, and the cluster, weights and bill stay in your name.

- **Your cloud account.** Savings Plans, reservations and committed-use discounts stay yours. On AWS, Qovery-managed EKS uses Karpenter to provision GPU nodes.
- **GPU node pools per service.** Pin the inference service to the B300 or B200 pool on any Qovery-managed cluster.
- **Existing clusters.** Already running OVHcloud Managed Kubernetes or your own cluster? Install Qovery on it (Kubernetes 1.31+) and deploy the same way.
- **Helm services.** Install LeaderWorkerSet and the GPU Operator as Helm services next to the model, versioned with the rest of the environment.
- **Environment auto-stop.** A`p6-b300.48xlarge` idling 12 hours a night costs about $1,709 per night on demand. Deployment rules stop non-production GPU environments on a schedule.
- **RBAC and audit logs.** Only the ML platform team changes the GPU environment, and every change is logged.

Install the Qovery Agent Skills with one command, then ask your coding agent in plain language to create the cluster and deploy the inference service in your own cloud account. The skills deploy, and the MCP Server operates what already exists.

```
curl -fsSL https://skill.qovery.com/install.sh | bash
# Project-scoped instead of global
curl -fsSL https://skill.qovery.com/install.sh | bash -s -- --project
# MCP Server for Claude Code: OAuth, read-only by default
claude mcp add --transport http qovery https://mcp.qovery.com/mcp --callback-port 4242
# MCP Server with write access, only when explicitly wanted (also needs write access enabled in the Qovery Console)
claude mcp add --transport http qovery "https://mcp.qovery.com/mcp?read_write=true" --callback-port 4242
```
- The first command installs the skills (`qovery-onboard` ,`qovery-deploy` ,`qovery-troubleshoot` ,`qovery-optimize` ,`qovery-speedup` ,`qovery-preview` ,`qovery-terraform` ) globally. Your agent asks you to approve each action they take.
- The second scopes the skills to the current project only.
- The third adds the MCP Server to Claude Code. The OAuth login runs in your browser, and the server stays read-only.
- The fourth gives write access. Use it only when you want the agent to change resources, and enable write access in the Qovery Console first.

Then ask, for example: "create a Kubernetes cluster in my AWS account in us-east-1 with Qovery, add a p6-b300.48xlarge GPU node pool, and deploy vLLM serving moonshotai/Kimi-K3.1-Pro with tensor parallel 8, auto-stopped at night." Start from the AI agent quickstart or the Claude Code integration.

## How do I self-host Kimi K3.1 Pro on my own Kubernetes cluster?

Rent one 8x B300 node (or two 8x B200 nodes with RDMA), install the NVIDIA GPU Operator, stage the 1,561 GB MXFP4 checkpoint on local NVMe, and run `vllm serve moonshotai/Kimi-K3.1-Pro --tensor-parallel-size 8 --trust-remote-code` from the `vllm/vllm-openai:kimi-k31` image. On one node that is a single pod requesting eight GPUs. Across nodes, wrap the pods in a LeaderWorkerSet so they start, fail and restart as one replica.

## How much GPU memory does Kimi K3.1 Pro need?

The official MXFP4 checkpoint is 1,561 GB, and NVIDIA's NVFP4 version is 1,605 GB. FP8 would need about 2.8 TB and BF16 about 5.6 TB, but neither checkpoint is published. Add KV cache (about 12-16 KB per token by my estimate) and per-sequence linear-attention state (roughly 217-434 MB per active sequence) on top of the weights.

## Can Kimi K3.1 Pro run on 8x H100 or 8x H200?

Not on one node. 8x H100 is 640 GB and 8x H200 is 1,128 GB, both below the 1,561 GB checkpoint. SGLang documents a four-node H100 layout (TP32 Â· EP32), and two 8x H200 nodes hold the weights, but you need an RDMA fabric between nodes and Hopper lacks native FP4 tensor cores.

## Is Kimi K3.1 Pro free for commercial use?

Mostly. The Kimi K3.1 Pro License allows commercial use, modification and redistribution. A model-as-a-service business with more than $20 million in revenue over 12 months needs a separate agreement with Moonshot AI, and products above 100 million monthly active users or $20 million monthly revenue must display "Kimi K3.1-Pro". Internal use is exempt.

## Which cloud instance is cheapest for Kimi K3.1 Pro?

On published on-demand list prices checked November 15, 2026, Scaleway's `B300-SXM-8-288G` at â¬60 an hour in Paris is the cheapest single node that holds the model, though its availability API showed it as "shortage". AWS's `p6-b300.48xlarge` costs $142.416 an hour on demand in us-east-1, and drops with Savings Plans. OVHcloud has announced B300 for November but has not put it on its price page yet.

## Does vLLM or SGLang support Kimi K3.1 Pro?

Both do. vLLM shipped day-0 support on November 4, 2026 with the `vllm/vllm-openai:kimi-k31` image and recipes for B300, GB300, MI355X and TPU v7x. SGLang serves it from the `kimi-k3.1-day-0` branch and the `lmsysorg/sglang:dev-kimi-k31-pro` image, and NVIDIA ships an experimental model-free NIM variant validated on 8x B300.

Guillaume is a Staff Solutions Architect at Qovery. He helps customers troubleshoot production infrastructure and writes about Kubernetes operations, AI agents, and reliable cloud platforms.

### Run Kimi K3.1 Pro in your own cloud account.

Qovery creates and operates the Kubernetes cluster and GPU node pools in your AWS, GCP, Azure or Scaleway account, or installs on the cluster you already run. The weights and the GPU bill stay yours.
