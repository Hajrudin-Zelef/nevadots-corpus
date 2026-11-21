---
id: collect-261121-veille-ia/nvidia-hardware/nvidia-nemotron-3-5-lightning-the-ai-agent-workhorse
title: "NVIDIA Nemotron 3.5 Lightning: The AI Agent Workhorse"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: []
keywords: ["model", "lightning", "nemotron", "agent", "reasoning", "nvidia", "execution", "should"]
source: https://www.analyticsvidhya.com/blog/2026/08/nvidia-nemotron-3-5-lightning/
source_anchor: ""
source_lines: [1, 272]
section: "NVIDIA_Hardware"
---
Long-running AI agents often spend most of their time on routine execution rather than difficult reasoning. After making a plan, they may perform hundreds of tool calls, file reads, validations, commands, and formatting steps, so using a frontier reasoning model for every action can become unnecessarily slow and expensive.

NVIDIA’s Nemotron 3.5 Lightning takes a different approach: a fast, efficient model designed for high-volume agent execution. The idea is simple: use the expensive model to think and the fast model to work. In this article, we examine whether that architecture can reduce cost without sacrificing agentic performance.

Furthermore, NVIDIA Nemotron 3.5 Lightning is an open-weight reasoning and instruction model designed primarily for the execution layer of agentic systems.

Its core specifications are:

| **Specification** | **Nemotron 3.5 Lightning** | 
|---|---|
| Total parameters | 30B | 
| Active parameters | 3B | 
| Architecture | Hybrid Mamba-2 + MoE + Attention | 
| Context window | Up to 1M tokens | 
| Input | Text | 
| Output | Text | 
| Reasoning | Supported and configurable | 
| Tool calling | Supported | 
| Quantization | NVFP4, W4A16 options | 
| Full precision checkpoint | BF16 | 
| Speculative decoding | MTP, DSpark, DFlash | 
| Recommended temperature | 1.0 | 
| Recommended top-p | 0.95 | 
| License | OpenMDW 1.1 | 
| Release date | August 11, 2026 | 

NVIDIA’s official NVFP4 model card also lists single-GPU deployment on a DGX Spark GB10 or H100, with support spanning Blackwell, Hopper and Ampere hardware depending on quantization.

The model is primarily intended for English and programming languages, while Spanish, French, German, Italian and Japanese are also officially supported.

This is important because Nemotron 3.5 Lightning should not be evaluated as simply “another 30B model.”

Of course, its intended job is much more specific.

Consider a coding agent.

It may first need to understand a bug and develop a plan. That is a difficult reasoning problem.

But after the plan exists, the agent may need to:

The first step may deserve a frontier model.

*Do all the others?*

**Probably not.**

NVIDIA argues that long-running agents spend a substantial portion of their workloads on exactly these high-volume execution operations, such as tool calls, validation and delegation. Using a frontier reasoning model for every execution step increases both cost and latency.

In short, Nemotron 3.5 Lightning is NVIDIA’s answer.

A possible production architecture becomes:

Next, this changes how we should think about model selection.

Instead of asking:

*Finally, which single model should power my agent?*

the more useful question becomes:

*Similarly, which model should handle each type of work inside my agent?*

That is the architectural idea behind Lightning.

Meanwhile, Nemotron 3.5 Lightning uses one of the more interesting architectures among current smaller agent models.

NVIDIA describes it as a hybrid:

```
Mamba-2
   +
Mixture-of-Experts
   +
Selective Attention
   +
Multi-Token Prediction
```
The combination matters because each component solves a different efficiency problem.

Nemotron 3.5 Lightning contains approximately 30 billion total parameters but activates only around 3 billion for each token.

In a dense 30B model, essentially the whole network participates in inference.

In an MoE model:

On the other hand, the router chooses only a small subset of experts.

You therefore retain much of the representational capacity of a larger model while doing computation closer to a significantly smaller model.

That is central to Lightning’s throughput advantage.

Published runtime configuration also exposes 128 routed experts plus a shared expert, with six routed experts selected per token. The configuration contains 52 hidden layers. Its hybrid layer pattern resolves to Mamba, MoE and sparse Attention components rather than using full self-attention at every layer. These are implementation-level configuration details, so developers should verify them against the exact checkpoint and runtime they deploy.

Traditional Transformers rely heavily on attention.

Attention is extremely powerful, but long sequences become computationally expensive.

Although Mamba is based on state-space modeling and can process sequences more efficiently.

Nevertheless, Nemotron 3.5 Lightning does not abandon attention entirely. Instead, NVIDIA uses Mamba-2 for much of the sequence processing while preserving selected Attention layers where global token interaction remains valuable.

Conceptually:

This hybrid design is particularly relevant for long-context agents.

Instead of paying full attention costs throughout the entire network, the model mixes mechanisms optimized for different jobs.

Attention is still important when tokens must directly compare information across distant parts of the sequence.

That matters for:

Nemotron therefore keeps selected attention layers instead of switching to a pure state-space architecture.

The architectural philosophy is not “Mamba instead of Transformer.”

It is **use expensive global attention only where it adds sufficient value.**

Normal autoregressive LLMs learn:

Token 1 → predict Token 2

Token 2 → predict Token 3

Token 3 → predict Token 4

Nemotron 3.5 Lightning includes Multi-Token Prediction, or MTP, layers that learn to predict multiple future tokens during training. Moreover, NVIDIA added a dedicated continued-pretraining stage for these MTP layers.

MTP improves training signals, but it also becomes useful during inference.

Instead of proposing only:

next token

the system can speculate about:

token t+1

token t+2

token t+3

...

Those candidates can then be verified efficiently.

As a result, this is one of the mechanisms behind Lightning’s high generation throughput.

On the other hand, its speed does not come from one optimization. It is the combination of several.

The best option therefore depends on concurrency.

There is no universally fastest configuration.

While Nemotron 3.5 Lightning combines strong intelligence with up to 4x output speed of similar-sized models, placing it on the accuracy-speed Pareto frontier for high-volume agent workloads.

NVIDIA publishes both BF16 and NVFP4 results across knowledge, reasoning, coding, agents, instruction following and long context.

In fact, the important observation is that quantization does not dramatically collapse model quality.

Here are the official reported results. Benchmark-native units are preserved, so not every value should be interpreted as a percentage.

| **Benchmark** | **BF16** | **NVFP4** | 
|---|---|---|
| MMLU Pro | 81.94 | 81.62 | 
| AA-Omniscience | 17.50 | 16.63 | 
| GPQA Diamond, no tools | 75.44 | 75.57 | 
| HLE, text-only, no tools | 11.72 | 10.47 | 
| SciCode | 32.60 | 31.38 | 
| SWE-bench Verified | 51.56 | 52.80 | 
| SWE-bench Multilingual | 39.33 | 36.47 | 
| Terminal-Bench 2.1 | 24.58 | 23.46 | 
| PinchBench | 85.37 | 83.43 | 
| BrowseComp | 36.97 | 36.81 | 
| τ³-bench Banking | 9.28 | 9.48 | 
| GDPval-AA-V2 | 832 | 865 | 
| IFBench loose | 71.88 | 72.88 | 
| AA-LCR | 52.00 | 49.19 | 

Moreover, NVIDIA says these evaluations were run through a consistent NeMo Gym and NeMo Evaluator-based harness and has published benchmark recipes for reproducibility.

In contrast, an interesting result is how close NVFP4 remains to BF16.

However, pricing is slightly more complicated than a single number because the model is open-weight and available through multiple routes.

The following reflects publicly listed pricing on August 12, 2026.

| **Access Method** | **Current Cost** | **Context** | **Best For** | 
|---|---|---|---|
| NVIDIA Build API | Free prototype endpoint | 1M | Testing | 
| OpenRouter free route | Free | 1M | Quick experimentation | 
| OpenRouter standard | $0.05 input / $0.20 output per 1M tokens | 262K | Simple hosted API | 
| Fireworks serverless | Similarly, $0.05 input / $0.01 cached / $0.20 output per 1M | 262K | Production serverless | 
| Ollama | No per-token model fee | Runtime dependent | Local/private use | 
| Self-hosted vLLM | Infrastructure cost | Up to 1M | Enterprise/self-hosting | 

Next, NVIDIA currently offers a free API endpoint for prototyping through build.nvidia.com.

Meanwhile, OpenRouter lists both a free Nemotron 3.5 Lightning route with a 1M context and a standard route currently priced at $0.05 per million input tokens and $0.20 per million output tokens. The standard OpenRouter route currently advertises a 262K context rather than the full 1M model capability.

Finally, Fireworks currently lists exactly $0.05 per million input tokens, $0.01 per million cached input tokens and $0.20 per million output tokens, with a 262K serverless context window.

Pricing and context limits can change quickly, particularly during the first weeks after a model release.

First, at launch, there are already several practical ways to use the model.

`ollama run nemotron-3.5-lightning”`
You can also use OpenRouter to run this model. Of course, its listed as a Free model on OpenRouter. Instead, grab an API key and start to use it

Nevertheless, NVIDIA exposes the model through an OpenAI-compatible endpoint. The official example uses nvidia/nemotron-3.5-lightning-30b-a3b.

Install the client:

`pip install openai`
Set your API key:

`export NVIDIA_API_KEY="your_api_key"`
Now create a simple request:

```
import os
from openai import OpenAI
client = OpenAI(
    base_url="https://integrate.api.nvidia.com/v1",
    api_key=os.environ["NVIDIA_API_KEY"]
)
response = client.chat.completions.create(
    model="nvidia/nemotron-3.5-lightning-30b-a3b",
    messages=[
        {
            "role": "user",
            "content": """
            A customer has submitted a warranty claim.
            Purchase date: 2025-04-12
            Claim date: 2026-03-02
            Warranty duration: 12 months
            Damage type: manufacturing defect
            Determine whether the claim is within the warranty period.
            Return JSON with:
            decision
            rationale
            """
        }
    ],
    temperature=1.0,
    top_p=0.95,
    max_tokens=2000,
    extra_body={
        "chat_template_kwargs": {
            "enable_thinking": True
        },
        "reasoning_budget": 4000
    }
)
print(response.choices[0].message.content)
```
**Output:**

```
{
  "decision": "approved",
  "rationale": "The warranty period begins on the purchase date of 2025-04-12 and lasts for 12 months, ending on 2026-04-12. The claim was submitted on 2026-03-02, which falls within the active warranty period. Additionally, the damage is listed as a manufacturing defect, which is typically covered under standard warranty terms."
}
```
NVIDIA’s main argument is that future production AI systems may depend less on a single giant model and more on a coordinated architecture of planners, routers, specialized workers, fast execution models, and verification layers. This represents a shift from maximizing model size to optimizing how different models work together.

In that architecture, Nemotron 3.5 Lightning does not need to be the smartest model available. Its value comes from being efficient, fast, and capable enough to handle most routine agent tasks while recognizing when harder work should be escalated. NVIDIA is therefore optimizing for practical, scalable agent execution rather than simply competing for the largest or most intelligent model.

A. NVIDIA provides open model weights, training data, and recipes under the OpenMDW 1.1 license. It is best described as an open-weight model; please review the governing license.

A. It contains approximately 30B total parameters while activating about 3B parameters per token.

A. The model supports up to 1 million tokens, although individual providers can expose smaller limits.
