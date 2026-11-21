---
id: collect-261121-veille-ia/nvidia-hardware/cobusgreyling-nemotron-3-5-lightning-blob-head-blog-md-1cf580aa
title: "Cobusgreyling nemotron 3 5 lightning blob head blog md"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: []
keywords: ["nemotron", "lightning", "switchyard", "model", "open", "nemo", "agents", "models"]
source: ""
source_anchor: ""
source_lines: [1, 143]
section: "NVIDIA_Hardware"
---
Subtitle: A 30B MoE with 3B active parameters — open, customizable, up to 4× faster in class — plus NeMo Switchyard for routing every step to the right model.
Tags: NVIDIA · Nemotron · Agents · MoE · NeMo Switchyard · Open Models · Local AI
Long-running agents do not spend most of their life thinking. They spend it doing.
Tool calls. Result validation. Subagent handoffs. git pull. Format this JSON. Check that billing line. Scan the security alert. Call the same three tools again with slightly different arguments.
If you send every one of those steps to a frontier reasoning model, you pay frontier prices and frontier latency for work that does not need frontier cognition. Always-on agents make that mistake expensive: the execution layer dominates the token budget.
NVIDIA Nemotron 3.5 Lightning is NVIDIA’s answer for that layer — and NeMo Switchyard is how you wire it into a system of models without rewriting your agent stack.
This post is a practitioner’s guide based on NVIDIA’s launch materials for Nemotron 3.5 Lightning and NeMo Switchyard.
| Dimension | Nemotron 3.5 Lightning | 
|---|---|
| Architecture | 30B mixture-of-experts (~3B active / 30B-A3B) | 
| Role | High-volume specialized execution for always-on agents | 
| Family position | Smallest member of the Nemotron 3 open model family | 
| Speed claim | Up to 4× faster output vs similar-sized models; ~30% faster agentic task completion in class | 
| Task efficiency | PinchBench: ~86% accuracy while completing 10k tasks ~35% faster than Qwen3.6-35B at similar accuracy | 
| Intelligence framing | Same intelligence class as Qwen3.6, ~30% faster; smarter than Gemma 4 (NVIDIA briefing) | 
| Openness | Weights, data, and recipes under OpenMDW-1.1 (permissive open) | 
| Inference | Multi-token prediction (MTP), speculative decoding (DFlash, DSpark), BF16 + NVFP4 | 
| Where it runs | Edge → PC → workstation → data center → cloud (RTX, DGX Spark/Station, Jetson, NIM, partners) | 
| Companion | NeMo Switchyard — open-source model routing library for agents | 
One-line thesis: frontier models plan; Lightning executes — Switchyard decides which is which, step by step.
Modern agent systems are not a single endpoint with a clever system prompt. They are ensembles:
| Layer | Typical model | Job | 
|---|---|---|
| Orchestration / planning | Nemotron 3 Ultra, GPT-class frontier | Decompose goals, long-horizon plans, hard reasoning | 
| Specialized execution | Nemotron 3.5 Lightning | Tool use, validation, domain micro-tasks, high QPS | 
| Routing | NeMo Switchyard | Send each step to the right model for quality / latency / cost | 
NVIDIA’s framing is explicit: open models win when customers need control over where AI runs, how it is customized, and how it evolves — across edge devices, PCs, workstations, data centers, and the cloud.
Lightning is not trying to be the only model in the loop. It is designed to be the high-volume specialist inside a multi-model agent.
From the technical materials, Lightning is built for the execution layer of long-running agents:
- High call volume
- Low latency
- Tool-heavy turns
- Harness generalization (OpenClaw, Hermes Agent, and other open harnesses)
- Local + hybrid deployment when privacy or cost matters
It inherits techniques proven across Nemotron 3 Super and Ultra, including multi-token prediction (MTP) baked in during training, and ships with draft models for speculative decoding:
| Draft / mode | Best for | 
|---|---|
| MTP | Medium → high concurrency (shorter draft length as concurrency grows) | 
| DSpark | DGX Spark–class local inference | 
| DFlash | Low-concurrency data-center workloads | 
Quantization is first-class: an NVFP4 checkpoint ships alongside BF16, using the same specialized kernels that power Nemotron 3 Ultra across Blackwell, Hopper, and Ampere. The same artifact is meant to travel from a desktop DGX Spark to a dense data-center fleet.
A 30B MoE with ~3B active parameters is a deliberate product shape:
- Quality headroom from a larger sparse model
- Serving cost closer to a small dense model
- Customization that is faster and cheaper than fine-tuning a giant dense frontier model
That last point is not marketing fluff for enterprises. Lightning-class models fine-tune on modest hardware with LoRA or full SFT, and they are easier to post-train on domain tools and workflows with NeMo.
Token throughput is necessary but not sufficient. Agent efficiency is how quickly useful work completes.
| Signal | Claim | 
|---|---|
| Output speed | Up to 4× vs similar-sized models | 
| Agentic completion | ~30% faster task completion in class | 
| PinchBench | ~86% accuracy; 10,000 tasks ~35% faster than Qwen3.6-35B at similar accuracy | 
| Artificial Analysis | Wins the accuracy-vs-speed Pareto for small open models (Intelligence Index vs output speed) | 
| Local Pareto | Sits on the EXO Labs local.ai Pareto frontier for small open models | 
NVIDIA’s narrative is consistent: Lightning is the model you want when the agent is busy, not when it is philosophizing.
Specialized agents win when the execution model speaks the organization’s language: tools, policies, tickets, code conventions, risk taxonomies.
Lightning is released as openly as possible under OpenMDW-1.1 so you can:
- Fine-tune with LoRA or full SFT via NeMo Automodel and NeMo Megatron Bridge
- Run RL with NeMo RL and NeMo Gym (e.g. GRPO-style recipes)
- Audit and retrain using open datasets and training techniques
- Deploy a domain-specific specialist without starting from a closed API
Alongside Lightning, NVIDIA is releasing Nemotron-RL-Agentic-Terminal-Pivot — an open agentic reinforcement-learning dataset used to train coding-agent capabilities.
| Organization | Domain angle | 
|---|---|
| CrowdStrike | Cybersecurity agentic tasks | 
| Harvey + Trajectory | Legal services | 
| CodeRabbit + Baseten | Code review (“remarkably easy to customize”) | 
| Lila Sciences | Physical and life sciences reasoning | 
| Fastino Labs | Software development, finance, healthcare accuracies | 
The pattern: take the open base → post-train on domain trajectories → drop into always-on agent loops.
If Lightning is the specialist, NeMo Switchyard is the traffic controller.
Agent workflows change step by step. A fixed model choice is wrong almost by definition:
- One default frontier model → overspend and extra latency
- Manual routing tables → integration debt and brittle product code
An open-source routing library for AI agents that directs each request to the most capable and suitable model for that step. Developers can tune routing algorithms for:
- quality
- latency
- cost
…and plug into tools they already use — without rewriting the agent application.
| Partner | Reported outcome | 
|---|---|
| Cadence | ~9.9% efficiency improvement (ChipStack AI Super Agent, formal verification) | 
| Classmethod | Initial tests: ~27% cost reduction while maintaining quality (opencode / Fireworks) | 
| Kong | Native Switchyard routing via Kong AI Gateway | 
| Ramp | Matched frontier performance while cutting costs ~58% and runtime ~33% (Ramp SWE-Bench) | 
| Siemens | Benchmarking for Fuse EDA AI Agent efficiency | 
| Internal NVIDIA | Frontier-level accuracy while reducing task completion cost to ~1/3 of Opus 4.8 alone | 
Routing strategy in one sentence:
Plans route up to the frontier; execution routes down to Lightning.
That is how multi-model agents get better tokenomics without dumbing down the hard steps.
Lightning is positioned for local AI, hybrid AI, and cloud in the same product line:
- NVIDIA RTX PCs and RTX PRO workstations
- NVIDIA DGX Spark and DGX Station
- NVIDIA Jetson edge devices
- Data centers and cloud via NIM, OpenRouter, and partner platforms
Industry-standard local stacks called out: Ollama, llama.cpp, Unsloth.
This is the privacy/control story: high-volume specialized tasks can stay on-prem or on-device when that is the constraint — and still participate in a larger routed system when the hard reasoning steps need a frontier model elsewhere.
| Channel | Use | 
|---|---|
| Hugging Face | Open weights (BF16 / NVFP4) | 
| build.nvidia.com | NIM microservice / playground | 
| OpenRouter | Hosted inference routing | 
| Partner clouds / post-training / inference platforms | Enterprise deployment | 
| GitHub (Switchyard) | Routing library | 
User / scheduler
      │
      ▼
┌─────────────────────┐
│  Agent harness      │  OpenClaw / Hermes / custom
│  (tools, memory)    │
└─────────┬───────────┘
          │ every step
          ▼
┌─────────────────────┐
│  NeMo Switchyard    │  quality / latency / cost policy
└─────────┬───────────┘
     ┌────┴────┐
     ▼         ▼
 Frontier    Lightning
 (plan)      (execute)
     │         │
     └────┬────┘
          ▼
     Tools / APIs / code / tickets
Rules of thumb:
- Default execution traffic to Lightning (or a domain-fine-tuned Lightning).
- Escalate planning, ambiguous failures, and hard reasoning to Ultra / frontier.
- Measure task completion time and $ per successful job, not only tokens/sec.
- Post-train Lightning on your tool traces before blaming the router.
Three shifts are stacking at once:
- Agents are systems of models, not single-model chat apps.
- Open models + open recipes let enterprises own accuracy, privacy, and cost curves.
- Routing is becoming product infrastructure — as important as the models themselves.
Nemotron 3.5 Lightning is NVIDIA doubling down on the specialist slot in that stack: small enough to deploy anywhere, open enough to customize, fast enough to survive high-volume agent loops. NeMo Switchyard is the glue that makes “the right model for this step” an engineering default rather than a custom project.
- Technical report — Nemotron 3.5 Lightning
- Weights — Hugging Face BF16 / NVFP4
- Deploy — vLLM / SGLang / TensorRT-LLM guides
- Route — NeMo Switchyard docs & GitHub
- Customize — NeMo Automodel, Megatron Bridge, NeMo RL / Gym recipes
- Dataset — Nemotron-RL-Agentic-Terminal-Pivot
- Try — build.nvidia.com · OpenRouter · NIM
For a structured, copy-pasteable resource pack — interactive plan vs execute lab, tool-loop examples, 20-task eval harness, cookbooks, and claim ledger — see this repository (make demo-offline, make eval).
Synthesized from NVIDIA materials for Nemotron 3.5 Lightning + NeMo Switchyard (August 2026). Partner metrics and benchmark figures are as stated in NVIDIA docs; verify against the public tech report and Artificial Analysis / PinchBench leaderboards.
