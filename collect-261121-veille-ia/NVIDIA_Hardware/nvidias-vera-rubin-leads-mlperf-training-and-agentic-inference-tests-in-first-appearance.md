---
id: collect-261121-veille-ia/nvidia-hardware/nvidias-vera-rubin-leads-mlperf-training-and-agentic-inference-tests-in-first-appearance
title: "NVIDIA’s Vera Rubin Leads MLPerf Training and Agentic Inference Tests in First Appearance"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: ["2026-11-17"]
keywords: ["vera", "rubin", "training", "mlperf", "nvidia", "nvidias", "minutes", "platform"]
source: https://creati.ai/ai-news/2026-11-17/nvidias-vera-rubin-leads-mlperf-training-and-agentic-inference-tests-in-first-appearance/
source_anchor: ""
source_lines: [1, 55]
section: "NVIDIA_Hardware"
---
NVIDIA’s new Vera Rubin platform led MLPerf training and agentic inference tests, pointing to major gains for AI workloads constrained by power and scale.

NVIDIA’s Vera Rubin platform has made a strong first appearance in the latest MLPerf results, leading the company’s submissions across training, post-training and agentic inference. NVIDIA says Vera Rubin NVL72 delivered the fastest result on six of seven MLPerf Training v6.1 benchmarks and, in provisional MLPerf Endpoints v1.0 testing, produced more than 10 times the throughput per kilowatt of the previous-generation GB300 NVL72 on one agentic workload.

The results matter because AI infrastructure is increasingly judged on more than raw accelerator speed. Developers need to train large models faster, run reinforcement-learning post-training cycles more efficiently and serve agents that may make repeated reasoning and tool calls while staying responsive to users. The Vera Rubin results suggest NVIDIA is targeting that entire pipeline with a combined hardware, networking and software platform rather than a standalone GPU upgrade.

The strongest performance claims come from NVIDIA’s own analysis of MLPerf data. The Endpoints figures for Vera Rubin remain provisional and are pending MLCommons Association verification, while the GB300 comparison results cited by NVIDIA were verified. That distinction is important when interpreting the generational gains.

In MLPerf Training v6.1, NVIDIA says Vera Rubin NVL72 recorded the fastest time on six of the seven benchmarks. The suite covers large language models, image generation, recommendation systems and reinforcement-learning post-training, giving the company an opportunity to demonstrate performance across different model architectures and workload types.

On the DeepSeek-V3 671B Mixture-of-Experts benchmark, a particularly communication-intensive test, Vera Rubin completed training in 4.14 minutes on 1,024 GPUs. NVIDIA’s cited GB300 NVL72 comparison took 8.03 minutes at the same scale. The company characterizes that as roughly twice the performance per GPU, although the broader result reflects the interaction between accelerators, memory, networking and the software stack.

Vera Rubin also led the new Qwen 3.5 397B GRPO reinforcement-learning post-training benchmark. NVIDIA reported a 10.44-minute result using its NeMo RL and TensorRT-LLM configuration at 1,024 GPUs, compared with 13.78 minutes for GB300 NVL72. A second Vera Rubin submission using vLLM completed the workload in 10.94 minutes.

The benchmark is relevant to AI agents because post-training increasingly involves generating rollouts, evaluating behavior and updating model weights. Those steps can force inference and training components to exchange large amounts of data. NVIDIA says its Vera CPU architecture can support more than 22,500 concurrent reinforcement-learning environments across 256 CPUs in a single rack, but that figure is also a vendor-reported platform capability rather than an independent deployment result.

NVIDIA’s GB300 platform still produced the fastest absolute DeepSeek-V3 training time in the round by scaling to 10,240 GPUs, completing the job in 1.38 minutes. That result is not a direct Vera Rubin-versus-GB300 comparison at identical scale, but it demonstrates the importance NVIDIA places on cluster-level deployment as well as per-GPU performance.

The Vera Rubin results highlight how much large-model training depends on communication between processors. NVIDIA says its Vera Rubin NVL72 maintained more than 97% scaling efficiency when expanding the DeepSeek-V3 workload from 256 to 1,024 GPUs. In the company’s broader description, quadrupling the GPU count reduced training time from about 16 minutes to roughly four minutes.

The platform combines a sixth-generation NVLink fabric inside the rack with ConnectX-9 SuperNICs and either Quantum-X800 InfiniBand or Spectrum-X Ethernet for scale-out networking. NVIDIA says the internal NVLink 6 spine provides 3.6 terabytes per second per GPU. These components are intended to keep thousands of accelerators operating as a coordinated system, particularly for Mixture-of-Experts models that require frequent all-to-all communication.

For AI builders, the practical implication is that advertised accelerator throughput may not translate into shorter training runs unless the cluster can move model states and activations efficiently. The MLPerf results therefore provide a test of NVIDIA’s full infrastructure design, although they do not establish how every customer workload will perform in production.

The MLPerf Endpoints v1.0 results address a different bottleneck: serving agents at scale while preserving interactive response times. The benchmark uses coding and enterprise workflows delivered through API endpoints. It tracks throughput and latency, and includes checks intended to confirm that generated tool calls and workflow intent remain correct. Separate SWE-bench Verified evaluations assess end-to-end task completion.

For Kimi K3, NVIDIA reported more than 10 times the throughput per kilowatt for Vera Rubin NVL72 compared with GB300 NVL72 at a shared minimum end-to-end interactivity level of 50 tokens per second per user. The provisional Vera Rubin run used 32 accelerators at 1,024-query concurrency, while the cited GB300 run used 48 accelerators at 48-query concurrency. Because the configurations and concurrency levels differ, the power-efficiency comparison should be read within the benchmark’s stated methodology rather than as a universal measure of application performance.

Across Kimi K3, DeepSeek-V4.1-Flash and Qwen3.6-35B-A3B, NVIDIA says Vera Rubin delivered five to 10 times higher throughput per kilowatt and three to five times higher end-to-end interactivity at matched concurrency than GB300 NVL72. These are NVIDIA-reported interpretations of the MLPerf Endpoints dataset, and the Vera Rubin entries were still awaiting verification when the developer documentation was published.

The platform’s inference gains are not attributed to silicon alone. NVIDIA points to HBM4 memory, Tensor Cores and sixth-generation NVLink, alongside prefix caching, state reuse, disaggregated prefill and decode serving, and NVIDIA Dynamo routing. Model-specific optimizations, including a DeepSeek MegaMoE kernel and Qwen multi-token prediction with Tensor Core verification, are also part of the submitted configurations.

That software detail is significant. Agentic workloads repeatedly reuse conversation history and tool definitions, but they also alternate between prompt processing, decoding and external actions. Infrastructure that cannot preserve useful context or balance those phases may waste much of its theoretical hardware capacity.

NVIDIA was the only organization with submissions across all seven MLPerf Training v6.1 benchmarks, according to its developer materials. The company also said 20 organizations contributed NVIDIA-based training submissions, including cloud providers, system manufacturers and universities. Those facts indicate broad participation around NVIDIA’s platform, but they do not independently measure customer adoption or commercial revenue.

The benchmark records provide a more useful basis for comparison than NVIDIA’s general launch claims, but they still represent selected workloads and carefully tuned systems. MLPerf results can show what a platform achieves under standardized conditions; they cannot predict performance for an organization’s proprietary models, data pipelines, software versions or service-level requirements.

The distinction is especially important for the Endpoints results. The Vera Rubin figures are provisional and subject to change through peer review and MLCommons verification. NVIDIA’s claims of continuing improvement from newer CUDA software builds are likewise unverified rerun results, not settled benchmark records.

For model developers, the training results could reduce the time between experiments, particularly for large Mixture-of-Experts models and reinforcement-learning post-training. Faster iteration can be valuable when teams are tuning agent behavior, testing tool use or adapting models to new data. However, buyers will need to evaluate total cluster cost, power availability, networking requirements and software portability rather than relying on per-GPU comparisons.

For enterprises serving AI agents, throughput per watt is becoming a deployment metric alongside latency and model quality. Higher efficiency can allow more concurrent users within a fixed data-center power budget, but only if the application benefits from the same context reuse, routing and model-specific optimizations used in the benchmark runs.

The results also reinforce NVIDIA’s competitive advantage in software-hardware co-design. TensorRT-LLM, vLLM, NeMo RL and Dynamo are not interchangeable proof points for every team, yet their presence in the submissions suggests that the platform is being optimized across both training and inference paths. Competing systems will need to show comparable performance with similarly mature kernels, networking and serving tools—not merely announce faster individual accelerators.

The first signal will be MLCommons’ verification and final publication of the Vera Rubin MLPerf Endpoints results. Any changes to the provisional Kimi K3, DeepSeek-V4.1-Flash or Qwen3.6-35B-A3B entries will affect the headline efficiency comparisons.

Infrastructure buyers should also watch independent replication of the training results, especially at 1,024-GPU scale and on reinforcement-learning post-training workloads. Cloud availability, pricing, power requirements and production customer results will show whether benchmark gains translate into accessible economics.

Finally, software updates will be critical. NVIDIA says Vera Rubin performance will continue to improve through its CUDA stack, while model developers are rapidly changing agent architectures and serving patterns. The durability of the advantage will depend on how quickly those optimizations generalize beyond the benchmark configurations.

Vera Rubin’s debut is notable less for a single record than for the breadth of the claim: NVIDIA is presenting one platform as a system for pretraining, post-training and agentic inference. The MLPerf results support that direction, but the evidence is strongest for tightly optimized configurations and should not be mistaken for a guarantee of production outcomes.

For builders and buyers, the practical lesson is to assess the whole serving and training stack. Power efficiency, interconnect performance, context management and software support may determine the economics of agent deployment as much as accelerator specifications. NVIDIA has supplied an influential benchmark opening statement; independent verification and real-world operating data will determine how decisive it becomes.
