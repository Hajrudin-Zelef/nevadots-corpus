---
id: collect-261121-veille-ia/hardware-semi/7-best-ways-to-self-host-ai-inference-on-cloud-gpus-in-2026-gcp-a5x-aws-and-scaleway-compared-qovery
title: "7 best ways to self-host AI inference on cloud GPUs in 2026 (GCP A5X, AWS and Scaleway compared) - Qovery Blog"
domain: hardware-semi
role: reference
task: article
actors: []
dates: []
keywords: ["nvidia", "cloud", "google", "inference", "gpus", "rubin", "scaleway", "vera"]
source: https://www.qovery.com/blog/best-ways-self-host-ai-inference-cloud-gpus-2026
source_anchor: ""
source_lines: [1, 288]
section: "Hardware_Semi"
---
- **Self-hosting inference beats a per-token API only above a utilization break-even.** Effective cost per 1M tokens = (GPU $/hour Ã· sustained tokens/hour) x 1,000,000. In the worked example below (gpt-oss-120B on 8x NVIDIA B200), self-hosting beats Together AI's $0.60 per 1M output tokens only above about 33% sustained utilization on Google Cloud DWS Flex-start pricing, and above about 58% on AWS on-demand.
- **Renting an A5X-class GPU is the small part of the job.** The bigger part is the GPU node pool, NVIDIA drivers and device plugin, an inference server (vLLM, SGLang or NVIDIA Dynamo/Triton), model weight storage and cold starts, GPU-aware autoscaling with scale-to-zero, and someone carrying the pager.
- **The three clouds win on different axes.** Google Cloud gets the newest NVIDIA silicon first (A5X on Vera Rubin NVL72, GA announced for November 2026). AWS has the deepest discount machinery (Savings Plans up to 72%, Spot up to 90%, Capacity Blocks for ML). Scaleway lists the cheapest H100 hour of the three, at â¬2.87 in Paris and Warsaw.
- **Idle GPUs in dev and staging are the most avoidable line on an inference bill.** A GPU node used 40 hours a week but left running all 168 hours spends about 76% of its time producing nothing.
- **If you want self-hosted economics without hiring a platform team,** run the inference service on Kubernetes inside your own AWS, GCP, Azure or Scaleway account (or your existing cluster) with an internal developer platform such as Qovery. The GPU bill, the committed-use discounts and the prompt data stay in your account.

# 7 best ways to self-host AI inference on cloud GPUs in 2026 (GCP A5X, AWS and Scaleway compared)

Google Cloud's A5X instances with NVIDIA Vera Rubin GPUs are generally available. Here is the real checklist to self-host inference on them, the break-even math against hosted APIs, how GCP, AWS and Scaleway GPU pricing compares, and the 7 platforms worth shortlisting.

Renting the newest GPU is the easy part. Keeping it busy is the expensive part.

Google Cloud just put NVIDIA Vera Rubin into its A5X instances, and my inbox is full of founders asking the same question: should we stop paying per token and run our own models? My answer is always the same. The GPU price is the number everyone compares, and it's the number that matters least. Utilization decides the bill.

So here is the full picture: what A5X actually is, the eight pieces of work behind a self-hosted endpoint, the break-even math with real numbers, how GCP, AWS and Scaleway prices compare, and the 7 options I would shortlist to run it. All prices below are pricing as of November 6, 2026.

## What are Google Cloud's A5X instances with NVIDIA Vera Rubin GPUs, and what changed at general availability?

**A5X is Google Cloud's bare-metal, accelerator-optimized instance family built on the NVIDIA Vera Rubin NVL72 rack, aimed at agentic inference, long-context serving and reinforcement learning at scale.** Google announced on October 21, 2026 that general availability is "coming next month", with access through your Google Cloud sales representative and planned expansion into new EMEA regions for data residency (Google Cloud).

What Google and NVIDIA have actually published:

- **GPU:** NVIDIA Rubin, packaged as Vera Rubin NVL72 (72 Rubin GPUs and 36 Vera CPUs per rack) (NVIDIA).
- **Memory per GPU:** 288 GB of HBM4 at 19.2 TB/s, per NVIDIA's platform spec (NVIDIA). Compare 141 GB at 4.8 TBps on the H200 in A3 Ultra (Google Cloud).
- **Interconnect:** sixth-generation NVLink at 3 TB/s per GPU and 216 TB/s per rack (NVIDIA), ConnectX-9 SuperNICs, and Google's Virgo Network scaling to 80,000 Rubin GPUs per site (NVIDIA blog).
- **GKE:** supported, with Managed DRANET for RDMA NICs and Kueue topology-aware scheduling (Google Cloud).
- **Not published yet:** GPUs per instance, vCPU, host RAM, local SSD, named regions and pricing. As of today, the Compute Engine machine docs stop at A4X Max. I won't guess these.

| GCP family | GPU | GPU memory per GPU | GPUs per instance | GPU interconnect | Status and access | Typical inference use case | 
|---|---|---|---|---|---|---|
| A5X | NVIDIA Vera Rubin NVL72 | 288 GB HBM4, 19.2 TB/s (NVIDIA) | Not published | NVLink 6, 3 TB/s per GPU (NVIDIA) | GA announced for November 2026, bare metal, via sales | Trillion-parameter MoE, long-context agentic serving | 
| A4X Max | NVIDIA GB300 | 279 GB HBM3e, 8 TBps | 4 (NVL72 domain) | NVLink 1,800 GBps | Bare metal only, reservation required | Large reasoning models at rack scale | 
| A4X | NVIDIA GB200 | 186 GB HBM3e, 8 TBps | 4 (NVL72 domain) | NVLink 1,800 GBps | Reservation required | Large MoE models, high concurrency | 
| A4 | NVIDIA B200 | 180 GB HBM3e, 8 TBps | 8 | NVLink 1,800 GBps | Reservation, Spot or Flex-start | 70B to 120B-class models on one node | 
| A3 Ultra | NVIDIA H200 | 141 GB HBM3e, 4.8 TBps | 8 | NVLink 900 GBps | Reservation, Spot or Flex-start | 70B-class models, mature software stack | 

A4X Max, A4X, A4 and A3 Ultra values come from Google Cloud's accelerator-optimized machine docs and GPU docs.

How big is the jump for inference? The best public data point is MLPerf Inference v6.1. On DeepSeek-R1 in the Server scenario, NVIDIA's preview Vera Rubin NVL72 submission delivered 1,175,890 tokens/s across 72 GPUs, about 16,300 tokens/s per GPU. NVIDIA's own GB300 NVL72 submission did about 8,300 per GPU and GB200 NVL72 about 5,500 (MLCommons raw results). That's roughly 2x Blackwell Ultra and 3x Blackwell per GPU, on a preview system.

What GA changes for you operationally:

- **Capacity is the constraint.** Google already requires reservations for A4X and A4X Max, and reservations, Spot, Flex-start or a resize request for A4 and A3 Ultra (Google Cloud). Expect A5X to be at least as tight. Plan for quota lead time.
- **Dynamic Workload Scheduler (Flex-start and Calendar mode)** is how you get scarce GPUs on a schedule. It's also often the cheapest price on the page (see the pricing section).
- **GKE is the intended runtime,** so whatever you run on A5X will be a Kubernetes workload.

Who actually needs A5X: teams serving very large MoE models, long contexts or thousands of concurrent agent sessions. Who doesn't: most production inference. A 7B to 70B model serving an internal product runs fine on L4, L40S, H100 or H200 at a fraction of the price. I'd start there and move up only when a benchmark tells you to.

**Self-hosting an inference endpoint is eight concrete pieces of engineering work, and the GPU instance is only the first.** Expect several weeks of platform work for the first production endpoint, plus a permanent owner afterwards.

1. **Capacity.** GPU quota, reservations, region choice, Spot vs on-demand, and a plan for when your region is out of stock.
2. **Node layer.** A GPU node pool, NVIDIA driver and CUDA version pinning, the NVIDIA device plugin or GPU Operator, taints and labels, and MIG or time-slicing decisions.
3. **Runtime.** Pick an inference server (vLLM, SGLang, NVIDIA Dynamo/Triton, TGI), then tune continuous batching, KV cache, quantization and tensor parallelism.
4. **Model weights.** Object storage, baked into the image, or a persistent volume. Pulling tens of GB and loading them into GPU memory is your cold start. Google says GKE Pod snapshots can cut AI inference start-up by up to 89% (Google Cloud), which tells you how much time the default path wastes.
5. **Scaling.** HPA or KEDA driven by queue depth and GPU utilization, scale-to-zero for dev and off-hours, and warm pools to protect a time-to-first-token SLO.
6. **Reliability.** Readiness probes that actually run a prompt, rolling model upgrades, GPU node failures and Xid errors, and cluster upgrades that don't drop in-flight requests.
7. **Observability and cost.** Tokens/s, time-to-first-token, inter-token latency, GPU utilization, and $ per million tokens as a first-class metric.
8. **Security and compliance.** Network isolation, who can reach the endpoint, where prompts and completions get logged, data residency and retention.

You're in good company: 66% of organizations hosting generative AI models already use Kubernetes for some or all of their inference (CNCF).

The honest headline: the GPU is the cheap part of self-hosting. The platform around it is the expensive part.

**Self-hosting beats a per-token API only when you keep the GPU busy enough that your effective cost per token drops below the API's published price.** Here is the formula:

**Effective cost per 1M tokens = (GPU $/hour Ã· sustained tokens/hour) x 1,000,000**

Here's the full worked example, every input sourced:

- **Model:** gpt-oss-120B, an open-weight model.
- **Throughput:** 90,205 tokens/s on 8x NVIDIA B200 in the MLPerf Inference v6.1 Server scenario (CoreWeave submission), so about 324.7M tokens per hour at 100% (MLCommons).
- **GPU price:** AWS p6-b200.48xlarge (8x B200) at $113.93/hour on-demand in us-east-1 (AWS), and Google Cloud a4-highgpu-8g (8x B200) at $64.44/hour on DWS Flex-start in us-central1 (Google Cloud).
- **API baseline:** Together AI serves gpt-oss-120B at $0.15 input and $0.60 output per 1M tokens (Together AI). Amazon Bedrock lists $0.1545 and $0.618 in the Sydney region (AWS).

| Sustained utilization | AWS p6-b200 on-demand, $ per 1M tokens | GCP A4 DWS Flex-start, $ per 1M tokens | Together AI API (output price) | 
|---|---|---|---|
| 100% | $0.35 | $0.20 | $0.60 | 
| 80% | $0.44 | $0.25 | $0.60 | 
| 50% | $0.70 | $0.40 | $0.60 | 
| 20% | $1.75 | $0.99 | $0.60 | 
| Break-even vs $0.60 | about 58% | about 33% | - | 

Two caveats keep this honest. MLPerf submissions are tuned to the limit, so your vLLM deployment will probably sustain less, which pushes break-even up. On the other side, I'm comparing against the API's output price only, while the API also bills every input token. That pushes break-even down.

Utilization dominates everything because you pay by the hour whether or not a request arrives. The same GPU at 15% utilization costs about 5x more per token than at 80%. VentureBeat Research found that roughly 83% of enterprises running their own GPUs report utilization of 50% or less (VentureBeat). Most teams sit on the wrong side of that table.

My rule of thumb: steady, predictable, high-volume traffic favors self-hosting. Spiky, low-volume or exploratory traffic favors a hosted API or a per-second serverless GPU.

Some reasons override the math entirely:

- Data residency and sovereignty requirements.
- No third-party logging of prompts and completions.
- Fine-tuned or open-weight models that no API offers.
- Predictable tail latency you control.
- Escaping per-token pricing that grows linearly with your success.

Cost lines teams forget: egress, persistent disks and object storage for weights, idle dev and staging GPUs, load balancers, log volume, and the engineer-hours to own the stack. Flexera's 2026 State of the Cloud report puts wasted cloud spend at 29%, rising for the first time in five years as AI workloads move to production (Flexera).

One more factual point. Whoever's cloud account the GPUs run in earns the committed-use discounts and keeps the prompt data.

|  | Self-hosted GPU in your cloud account | Serverless GPU (Modal, Baseten) | Hosted per-token API (Together AI, Bedrock, Vertex AI) | 
|---|---|---|---|
| Cost model | Per GPU-hour, whether busy or idle | Per GPU-second while containers run | Per input and output token | 
| Break-even condition | Wins at sustained high utilization | Wins for bursty traffic with real idle gaps | Wins at low or unpredictable volume | 
| Latency control | Full: you own batching, placement, warm capacity | Partial: cold starts on scale-up | Limited: shared capacity, provider's limits | 
| Data residency | Your account, your region | Vendor's account and regions | Provider's account; regional options vary | 
| Model and runtime freedom | Any model, any inference server | Any container you bring | Provider's model catalog only | 
| Ops burden | Highest without a platform layer | Low | None | 
| Realistic time to production | Weeks for the first endpoint | Days | Hours | 

**On published on-demand list prices, Scaleway has the cheapest H100 hour of the three, AWS has the deepest discount machinery, and Google Cloud is where the newest NVIDIA generation lands first.** Google Cloud's list prices look expensive, but its committed-use and Flex-start prices change the ranking.

Pricing as of November 6, 2026, normalized to $ (or â¬) per GPU-hour, US regions for GCP and AWS (their cheapest), Paris and Warsaw for Scaleway:

| Provider | Instance | GPU | On-demand per GPU-hour | Discount mechanisms (max published) | Spot? | Example regions | Best for | 
|---|---|---|---|---|---|---|---|
| Google Cloud | a3-highgpu-8g | H100 80GB | $11.06 ($88.49 per 8-GPU VM) | 3-year resource CUD $4.86 (56% off), DWS Flex-start $4.79, Spot up to 91% off | Yes | us-central1 | Committed fleets, H100 on a schedule | 
| Google Cloud | a3-ultragpu-8g | H200 141GB | $10.60 ($84.81 per VM) | 3-year CUD $4.65, Flex-start $5.30 | Yes | us-central1 | 70B-class models | 
| Google Cloud | a4-highgpu-8g | B200 180GB | No on-demand list price | Flex-start $8.06, 3-year CUD $7.09, Spot $4.95 | Yes | us-central1 | 120B-class models on one node | 
| Google Cloud | A5X | Vera Rubin | Not published | Not published | Not published | EMEA expansion announced | Frontier-scale serving | 
| AWS | p5.4xlarge / p5.48xlarge | H100 80GB | $6.88 | Savings Plans up to 72%, Spot up to 90%, Capacity Blocks $5.97 | Yes | us-east-1 | Discount stacking, single-GPU H100 | 
| AWS | p5en.48xlarge | H200 141GB | $7.91 | Same, Capacity Blocks $7.90 | Yes | us-east-1 | 70B-class models | 
| AWS | p6-b200.48xlarge | B200 180GB | $14.24 | Same, Capacity Blocks $14.21 | Yes | us-east-1 | Blackwell without reservations | 
| Scaleway | H100-1-80G | H100 PCIe 80GB | â¬2.87 | Per-minute billing; no Spot market in its public catalog | No | fr-par-2, pl-waw-2 | Cheapest H100 hour in the EU | 
| Scaleway | H100-SXM-8-80G | H100 SXM 80GB | â¬3.17 (â¬25.33 per 8-GPU instance) | Per-minute billing | No | fr-par-2 | Multi-GPU 70B serving in the EU | 
| Scaleway | L40S-1-48G / L4-1-24G | L40S / L4 | â¬1.47 / â¬0.79 | Per-minute billing | No | fr-par-2, pl-waw-2 | Small and quantized models | 

Sources: Google Cloud accelerator-optimized pricing, Google Cloud Spot VMs, AWS EC2 On-Demand pricing, AWS Savings Plans, AWS Spot, AWS Capacity Blocks pricing, Scaleway GPU pricing (per-minute rates from Scaleway's public product catalog, converted to hourly). The GCP CUD percentage is computed from Google's own 3-year and on-demand prices for a3-highgpu-8g.

What the table tells me:

- **Scaleway's â¬2.87 H100 PCIe hour is less than half of AWS's $6.88 US H100 hour** at any recent EUR/USD rate. PCIe H100s are slower than SXM, so compare on measured tokens/s, not on the label. Scaleway's catalog is small and centers on H100, L40S and L4.
- **AWS is the cheapest on-demand H100 of the two hyperscalers,** and Capacity Blocks for ML let you reserve GPU windows in advance at a dynamic rate charged upfront (AWS).
- **Google Cloud's sticker price is the highest,** yet its 3-year CUD and Flex-start H100 prices undercut AWS on-demand by about 30%.
- **Vera Rubin is Google Cloud-first.** AWS has announced Rubin and Rubin Ultra GPUs for deployment in 2027-2028, with no instance names yet (Amazon).

Now the reversal, using the B200 example from above (gpt-oss-120B, 90,205 tokens/s per 8 GPUs):

- **At 80% utilization,** GCP Flex-start costs $0.25 per 1M tokens and AWS on-demand $0.44. Both beat Together AI's $0.60 output price. Self-hosting wins.
- **At 20% utilization,** GCP rises to $0.99 and AWS to $1.75. The $0.60 API now wins by a wide margin.

Same model, same GPUs, same prices. Only utilization changed, and the ranking flipped.

Data residency is a pricing-adjacent factor. Scaleway is a French provider running these GPUs in Paris and Warsaw, while the EU regions of GCP and AWS are operated by US-headquartered companies. For regulated workloads, operator jurisdiction can rule a provider out before price even comes up.

**We scored every option on seven criteria that decide whether a team can keep a GPU endpoint alive in production for a year.** Marketing claims and cherry-picked benchmarks didn't count.

1. **Runs in your own cloud account (BYOC) or the vendor's.** This decides who gets the GPU bill, the committed-use discounts and the prompt data.
2. **GPU scheduling handled for you:** node pools, drivers, device plugin, taints, MIG and time-slicing.
3. **Autoscaling on GPU-relevant signals,** plus genuine scale-to-zero for non-production.
4. **Model and runtime freedom:** any container, any inference server, any open-weight model.
5. **Operational burden:** who upgrades the cluster, patches drivers and carries the pager at 3am.
6. **Pricing model transparency:** per GPU-second, per seat, per cluster, usage-based, or free and open source.
7. **Multi-cloud reality:** works on AWS, GCP, Azure, Scaleway and an existing Kubernetes cluster, or is locked to one provider.

**Here is the shortlist, ordered by how much GPU platform work each option removes while still keeping the GPUs in your own cloud account.** Options 6 and 7 don't run in your account. I include them as the honest baseline.

| # | Option | Best for | Runs in your own cloud account? | GPU ops handled for you? | Scale-to-zero? | Pricing model | Main drawback | Pick this if | 
|---|---|---|---|---|---|---|---|---|
| 1 | Qovery | Product teams without a platform team | Yes: AWS, GCP, Azure, Scaleway or your own cluster | Cluster, environments and deployments managed; you still pick the inference server | Environment auto-stop for non-production | Platform subscription; GPUs billed by your cloud | Doesn't tune your model or pick your runtime | You want BYOC economics without owning Kubernetes | 
| 2 | GKE with NVIDIA GPU Operator | Teams that need A5X and Vera Rubin first | Yes, GCP only | Partly: managed drivers, you own the rest | Node pools can scale to zero | GCP compute plus GKE fee | You own the whole platform | You have Kubernetes engineers and need the newest silicon | 
| 3 | Amazon EKS with Karpenter | Spot-heavy, discount-optimized fleets | Yes, AWS only | Partly: Karpenter provisions nodes, you own the rest | Yes, with Karpenter | AWS compute plus EKS fee | Deepest AWS lock-in of the list | You live on Savings Plans and Spot | 
| 4 | Scaleway Kubernetes Kapsule | EU data residency on a budget | Yes, Scaleway only | Partly | Node pools can scale down | Scaleway compute, per minute | Smaller GPU catalog, fewer regions | Your users and regulators are in the EU | 
| 5 | KServe / Ray Serve / vLLM on your cluster | Maximum throughput tuning | Yes, wherever your cluster runs | No: you own all of it | KServe via Knative | Free and open source | You own upgrades, glue code and cold starts | Throughput per dollar is your core business | 
| 6 | Modal and Baseten | Bursty traffic, per-second billing | No: vendor's account | Yes | Yes | Per GPU-second or per minute | Prompt data and GPUs live outside your account | Traffic is spiky and you want zero infra | 
| 7 | Vertex AI, Amazon Bedrock, Together AI | Low or unpredictable volume | No: provider's account | Yes | Not applicable | Per token | Cost scales linearly with usage; catalog limits | You're below the break-even utilization | 

**Qovery is the right pick when you've decided to keep GPUs in your own AWS, GCP, Azure or Scaleway account, or your existing Kubernetes cluster, and you don't want an engineer permanently owning clusters, environments and deployment plumbing.** I'm obviously biased here, so I'll be specific about what it does and doesn't do.

Qovery is an internal developer platform that deploys and operates applications on Kubernetes inside your own cloud account (BYOC). The GPU bill, the committed-use discounts and the prompt data stay in your name.

What it gives an inference team:

- **Git-push deployments** of your inference container.
- **Managed cluster upgrades,** so Kubernetes version bumps stop being a quarterly project.
- **Per-environment RBAC** to control who can deploy or reach the endpoint.
- **Preview environments per pull request** to test a new model or runtime version before production.
- **Environment auto-stop for non-production.**
- **Databases backed by managed cloud services** for the app around the model.

Auto-stop is where the GPU money is. A GPU node used 40 hours a week but left running all 168 hours pays for 128 idle hours, about 76% waste. Stopping dev, staging and preview environments outside working hours removes most of it. On a GCP a3-highgpu-8g at $88.49/hour, that's about $11,300 a week in idle time per node. I wrote more about this in our GKE fractional GPU guide.

The boundary, so the comparison stays credible: Qovery doesn't resell GPUs, doesn't host your model and doesn't choose your inference server. You still pick vLLM, SGLang or NVIDIA Dynamo, and your model weights.

Who should not pick Qovery:

- Teams that want a per-token hosted endpoint with zero infrastructure. A hosted API is cheaper and faster for you.
- A one-off demo that will never see sustained traffic.
- Teams with an existing platform team chasing the last few percent of GPU efficiency with custom kernels, bespoke schedulers or bare metal. Go raw GKE or EKS.

**Raw managed Kubernetes is the most flexible, lowest-markup route to self-hosted inference, and it's the right answer when you already have engineers who own clusters full time.**

- **GKE:** first access to A5X and Vera Rubin, managed GPU drivers or the NVIDIA GPU Operator, Dynamic Workload Scheduler for scarce capacity, and llm-d 1.0 for cache-aware routing across GPU and TPU fleets (Google Cloud).
- **EKS with Karpenter:** strong Spot interruption handling, the deepest discount ecosystem, and Capacity Blocks for ML to reserve GPU windows in advance.
- **Scaleway Kapsule:** the cheapest European GPU capacity in this comparison, run by a European provider, with a smaller GPU catalog and fewer regions.

The honest cost of this route: driver and CUDA pinning, cluster upgrades, GPU autoscaler tuning, node failure handling, and an on-call rotation that understands both Kubernetes and CUDA.

When it clearly beats everything else: large sustained GPU fleets where a few percent of efficiency is worth more than a senior engineer's salary.

**The open-source inference stack complements your platform choice: vLLM or SGLang is the engine, KServe or Ray Serve is the routing and autoscaling layer, and something still has to run the Kubernetes cluster underneath.**

- **vLLM and SGLang:** continuous batching and paged KV cache, the core of tokens-per-dollar.
- **KServe:** model CRDs, canary rollouts and scale-to-zero through Knative.
- **Ray Serve:** composing several models into one pipeline.
- **NVIDIA Dynamo:** disaggregated prefill and decode serving across GPUs.

Strengths: no per-GPU-second markup, portable across clouds, and the highest throughput-tuning ceiling of anything on this list. Weaknesses: you own the upgrades, GPU fragmentation, cold starts and every line of glue code.

Choosing vLLM doesn't answer who runs the cluster. That's still Qovery, GKE, EKS or Kapsule. For a throughput figure to plug into the break-even formula, start with the MLPerf v6.1 results (MLCommons), then measure your own model, prompt lengths and runtime, because that number is the one that counts.

**Serverless GPU and hosted-model platforms win whenever traffic is spiky or low-volume, because you stop paying the moment nobody calls the model.** That's exactly the condition under which self-hosting loses the break-even math.

- **Modal:** per-second GPU billing, for example $0.001097/s for an H100 (about $3.95/hour) and $0.001736/s for a B200 (about $6.25/hour), plus CPU and memory, with a 1.15-1.75x multiplier for region selection (Modal).
- **Baseten:** per-minute dedicated GPUs, $6.50/hour for an H100 and $9.98/hour for a B200 (Baseten).
- **Together AI:** gpt-oss-120B at $0.15/$0.60 per 1M input/output tokens, and dedicated H100s at $5.49 per GPU-hour (Together AI).
- **Vertex AI and Amazon Bedrock:** the hosted baseline inside an existing GCP or AWS contract. Easiest compliance and procurement story, least model and runtime freedom.

Modal's B200 rate looks cheap next to an always-on VM, and it is when containers scale down between bursts. Leave containers warm around the clock, though, and you're back to paying for idle GPUs in someone else's account.

The trade to accept: no committed-use discounts accrue on your own cloud contract, prompt data leaves your account, and per-token cost scales linearly with your success.

My crossover rule: below a sustained request floor, pay per token. Above it, pay per GPU-hour and keep the GPU busy.

**Install the Qovery Agent Skills once, then ask your coding agent in plain language to deploy the inference service into your own cloud account. The agent runs the steps and asks you to approve each change before it touches anything.**

Install the skills globally for every compatible agent (Claude Code, Cursor, Codex, OpenCode, Gemini CLI):

```
curl -fsSL https://skill.qovery.com/install.sh | bash
```
Or keep them scoped to the current repository:

```
curl -fsSL https://skill.qovery.com/install.sh | bash -s -- --project
```
Add the Qovery MCP Server to Claude Code over OAuth, read-only by default, so the agent can inspect what already runs:

```
claude mcp add --transport http qovery https://mcp.qovery.com/mcp --callback-port 4242
```
Enable write access only when you want the agent to change things (it also needs write access enabled in the Qovery Console):

```
claude mcp add --transport http qovery "https://mcp.qovery.com/mcp?read_write=true" --callback-port 4242
```
What each skill does:

- `qovery-onboard` : connect your cloud account and cluster.
- `qovery-deploy` : deploy the inference container.
- `qovery-preview` : per-pull-request environments.
- `qovery-troubleshoot` : failed pods and GPU scheduling errors.
- `qovery-optimize` : idle environments and cost.
- `qovery-speedup` and`qovery-terraform` : faster builds and infrastructure as code.

The MCP Server operates what already exists. Then ask: "Deploy this vLLM service to my GCP account with Qovery and run it on a GPU node pool." Review each proposed action before you approve it.

Docs: AI agent quickstart and Claude Code integration.

## What are Google Cloud A5X instances with NVIDIA Vera Rubin GPUs, and when did they become generally available?

A5X is Google Cloud's bare-metal instance family built on NVIDIA Vera Rubin NVL72, with 288 GB of HBM4 per Rubin GPU according to NVIDIA. Google announced on October 21, 2026 that general availability would follow in November 2026, with access through Google Cloud sales. Per-instance specs, regions and prices weren't yet published in Compute Engine documentation as of November 6, 2026.

## Is it cheaper to self-host AI inference on cloud GPUs than to use a hosted API like Amazon Bedrock, Vertex AI or Together AI?

Only above a utilization break-even. For gpt-oss-120B on 8x B200, self-hosting beats Together AI's $0.60 per 1M output tokens above roughly 33% sustained utilization on Google Cloud DWS Flex-start pricing and roughly 58% on AWS on-demand. Below that, the hosted API is cheaper.

## How does GPU pricing compare between Google Cloud, AWS and Scaleway in 2026?

As of November 6, 2026, an H100 costs â¬2.87/hour on Scaleway (H100 PCIe, Paris and Warsaw), $6.88/hour on AWS on-demand (us-east-1) and $11.06/hour on Google Cloud on-demand (us-central1). Google Cloud's 3-year committed-use price drops its H100 to $4.86/hour, and AWS Savings Plans offer up to 72% off. Google Cloud is the only one of the three announcing Vera Rubin capacity this year.

## What do I need to run an LLM inference server like vLLM on Kubernetes GPU nodes?

You need a GPU node pool with pinned NVIDIA drivers and the device plugin or GPU Operator, an inference server such as vLLM, a storage strategy for model weights, and autoscaling driven by queue depth or GPU utilization. Add model-aware readiness probes, tokens/s and time-to-first-token metrics, and network controls on who can reach the endpoint.

## Can I self-host AI inference in my own cloud account without hiring a DevOps or platform engineer?

Yes, if a platform layer handles the cluster for you. An internal developer platform such as Qovery deploys and operates your inference service on Kubernetes in your own AWS, GCP, Azure or Scaleway account, or your existing cluster. You still choose the inference server and the model; the GPU bill and discounts stay in your account.

## How do I stop paying for idle GPUs in development and staging environments?

Stop non-production environments outside working hours and let GPU node pools scale to zero. A GPU node used 40 hours a week but running 168 hours wastes about 76% of its cost. Qovery's environment auto-stop does this on a schedule for dev, staging and preview environments.

Romaric founded Qovery to make Kubernetes accessible to every engineering team. He writes about platform strategy, developer experience, and the future of cloud infrastructure.

### Run your GPUs in your own cloud account, without running Kubernetes.

Qovery deploys and operates your inference services inside your own AWS, GCP, Azure or Scaleway account - or your existing Kubernetes cluster. The GPU bill and the committed-use discounts stay yours. Start in under 10 minutes.
