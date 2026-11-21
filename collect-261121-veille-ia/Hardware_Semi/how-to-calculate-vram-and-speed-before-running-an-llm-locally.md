---
id: collect-261121-veille-ia/hardware-semi/how-to-calculate-vram-and-speed-before-running-an-llm-locally
title: "How to Calculate VRAM and Speed Before Running an LLM Locally"
domain: hardware-semi
role: reference
task: article
actors: []
dates: []
keywords: ["memory", "speed", "model", "decode", "bits", "running", "before", "models"]
source: https://www.mindstudio.ai/blog/how-to-estimate-llm-vram-requirements
source_anchor: ""
source_lines: [1, 92]
section: "Hardware_Semi"
---
## How do you calculate if an LLM will fit in VRAM?

Take the parameter count, multiply by the bits per weight, then divide by 8 to get bytes, converted to gigabytes. A model quantized at 4 bits rarely runs at a clean 4 bits across every layer, some layers stay higher precision for stability, so a more realistic multiplier is around 4.5 bits per weight. That works out to roughly parameters in billions times 0.56 to get gigabytes of memory. A 100-billion-parameter model at that quantization level needs about 56 GB. This is the first filter before you even download a model: if that number is bigger than your available high-speed memory, you’re already planning for a compromise.

## TL;DR

- **Memory math is simple arithmetic** : parameters times bits per weight divided by 8 gives you gigabytes, and using 4.5 bits instead of a clean 4 accounts for mixed-precision layers that most real quantizations use.
- **Not all memory is equal** : systems with both high-bandwidth GPU memory and larger unified or system memory can technically hold bigger models, but running in the slower pool tanks decode speed even if the model “fits.”
- **Inference has two distinct phases** : prefill (reading the prompt, compute-bound) and decode (writing the answer, memory-bandwidth-bound), and they behave completely differently under load.
- **You can estimate a speed ceiling yourself** : divide memory bandwidth by the model’s active parameter size in bytes to get a theoretical tokens-per-second ceiling, then expect real-world single-user speed to land around a third of that once attention, KV cache reads, and kernel overhead are factored in.
- **Mixture-of-experts models beat dense models of similar or even larger size** on decode speed, because only a fraction of total parameters activate per token, so total parameter count alone is a poor predictor of speed.
- **KV cache eats into your “free” memory budget** and grows with both context length and concurrent users, so leftover space after loading model weights isn’t actually free for more models.
- **Agentic workloads need a much higher tokens-per-second floor than chat** , because agents generate long reasoning traces and repeat tool calls, turning a merely adequate chat speed into an hour-long wait for one task.

### Built like a system. Not vibe-coded.

Remy manages the project — every layer architected, not stitched together at the last second.

## What’s the difference between prefill and decode speed?

Prefill happens when the model ingests your prompt: thousands of tokens get processed in parallel, every weight loaded gets reused across all of them, and this phase is compute-bound. More raw FLOPs makes prefill faster.

Decode is different. The model generates one token at a time (or a small handful with multi-token prediction), and for every single token it has to read all of its active weights from memory again. That makes decode memory-bandwidth-bound, not compute-bound. This is why two machines with similar compute specs can feel wildly different in practice: one might have strong tensor cores but mediocre memory bandwidth, which caps decode speed regardless of how fast its math units are. A useful rule of thumb: if a machine class has roughly 270 GB/s of bandwidth, prefill will look fine but decode will crawl, no matter how good the chip is otherwise.

## How do you estimate tokens-per-second before running a model?

Divide memory bandwidth by the size of the active weights per token. For example, a mixture-of-experts model with 18 billion active parameters at roughly 10 GB of memory traffic per token, running on hardware with around 7 terabytes per second of bandwidth, gives a theoretical ceiling near 700 tokens per second.

In practice, a single user typically sees something closer to a third of that theoretical number, because attention computation, KV cache reads, and kernel overhead all add cost that the simple bandwidth-divided-by-weights formula ignores. Techniques like speculative decoding can close some of that gap: a small draft model guesses several tokens ahead, and the larger model verifies them in a single pass, trading spare compute for fewer memory round-trips. This only helps when there’s compute headroom available. Once concurrency rises and that headroom disappears, speed drops back toward the unassisted number.

## Why does mixture-of-experts architecture matter for speed?

Total parameter count is a bad predictor of decode speed on its own. What matters is active parameters per token.

A dense model reads every single one of its parameters for every token it generates. A mixture-of-experts (MoE) model routes each token through only a subset of its total parameters, the “active” parameters, while the rest sit in memory unused for that step. This means a dense model with far fewer total parameters can actually be slower in decode than a much larger MoE model, because the dense model still has to touch all of its weights every time, while the MoE model only touches its active slice.

This doesn’t make dense models useless. Dense architectures are still commonly favored for coding tasks, and smaller dense models can be perfectly fast. But if you’re comparing two models and only looking at total parameter count, you’re missing the variable that actually drives decode throughput.

**Live with Dmitry Shapiro**Ex-Google product lead · Ex-MySpace CTO

What you'll learn · Dec 2

1. 01How enterprises really build with AI
2. 02What full-stack means and why it's critical for business software
3. 03Prompting that holds up in production
4. 04Build and deploy your own app, live

The catch with MoE models: all of the experts still have to be stored in memory somewhere, even though only a few activate per token. A huge MoE model with manageable active parameters can still fail to fit in fast memory, forcing part of it into slower memory, which then throttles the very speed advantage that made MoE attractive in the first place.

## How much does KV cache change the picture?

The KV cache is the memory used to track the running context of a conversation, the keys and values attention needs to reference everything said so far. It grows with context length and with the number of concurrent users or sessions.

This matters because KV cache isn’t optional overhead you can ignore when budgeting memory. After loading model weights, whatever space is “left over” isn’t automatically available for stacking more models, because that headroom is exactly where KV cache needs to live. Longer prompts and more simultaneous users both expand this footprint. If you’re serving many users or running agents with very long context windows, the KV cache can end up being a significant chunk of your total memory budget, not a rounding error.

There’s also a compounding effect with batching. Serving multiple users at once means every weight read from memory gets reused across all of those users simultaneously, which effectively turns a memory-bandwidth-bound decode problem back into a compute-bound one at scale. That’s good for throughput, but it means your KV cache sizing has to scale right alongside your expected concurrency.

## Is tokens-per-second speed equally important for every use case?

No, and this is one of the most practical things to get right when picking a model for a task. Chat and retrieval-augmented generation (RAG) workflows involve a human reading output as it streams, so somewhere around 30 tokens per second feels perfectly fine, since that roughly matches human reading speed.

Agentic workloads are a different story. An agent doesn’t read its own output: it generates long reasoning traces, writes code, calls tools, and repeats that cycle dozens of times to complete a single task. At a chat-friendly 30 tokens per second, generating a 3,000-token reasoning step takes close to two minutes. Multiply that by dozens of steps in one agentic rollout and you’re looking at over an hour for a single task. For that reason, agentic use cases generally need a floor closer to 80 tokens per second per user to stay practical, and that requirement multiplies further if you’re running sub-agents in parallel, since each one needs its own throughput.

## Frequently Asked Questions

### What’s a quick formula for estimating a model’s memory footprint?

Multiply the parameter count in billions by roughly 0.56 for a 4-bit-class quantization (accounting for mixed-precision layers), which gives you an approximate memory requirement in gigabytes.

### Why does the same model run at different speeds on different hardware?

Decode speed is bound by memory bandwidth, not compute, so a machine with more bandwidth per active parameter will generate tokens faster even if its raw compute specs look similar to another machine.

### Does a bigger model always mean slower inference?

No. A mixture-of-experts model with a large total parameter count but a small active-parameter count per token can outrun a smaller dense model, because decode speed depends on active parameters, not total size.

### How does concurrency affect per-user speed?

### Everyone else built a construction worker.

We built the contractor.

    One file at a time.

UI, API, database, deploy.

Serving more users at once lets each weight read be reused across all of them, which raises total throughput, but it also means each individual user’s tokens-per-second can drop unless there’s enough spare compute and memory bandwidth to support everyone.

### Why do agents need higher token speeds than chat applications?

Agents generate long reasoning and tool-call sequences without a human reading along in real time, repeating this process many times per task, so a speed that feels fine for chat can turn one agentic task into an hour-long wait.
