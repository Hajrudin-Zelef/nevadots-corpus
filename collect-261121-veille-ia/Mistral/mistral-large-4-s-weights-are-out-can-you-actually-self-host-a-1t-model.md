---
id: collect-261121-veille-ia/mistral/mistral-large-4-s-weights-are-out-can-you-actually-self-host-a-1t-model
title: "Mistral Large 4's Weights Are Out: Can You Actually Self-Host a 1T Model?"
domain: mistral
role: reference
task: article
actors: ["Mistral AI"]
dates: ["2026-10-29"]
keywords: ["model", "mistral", "official", "card", "token", "context", "large", "preview"]
source: https://dev.to/tng_thnhnhn_c933ee88b/mistral-large-4s-weights-are-out-can-you-actually-self-host-a-1t-model-1954
source_anchor: ""
source_lines: [1, 75]
section: "Mistral"
---
Mistral Large 4, nicknamed Le Chonk, is no longer an API-only preview. On October 29, 2026, Mistral published the weights, three weeks after the October 6 preview. That changes the question from "when can we try it?" to a much more practical one: can a person or a small team actually run it?

The short answer is: yes, but only on hardware that looks very different from a normal workstation. If you start from the headline number, you will get the hardware question wrong. This article is a reality check, not a setup tutorial.

The official repository is `mistralai/Mistral-Large-4-1T-A52B` on Hugging Face, published by Mistral AI. It ships in two official variants, FP8 and NVFP4, with community GGUF conversions also available from the llama.cpp team and Unsloth. The supported serving stacks on the official card are vLLM, SGLang, Transformers, and llama.cpp.

The released checkpoint is also not simply the preview build. Mistral says it reflects three more weeks of reinforcement learning and customer testing after the preview. Do not copy conclusions from preview-only write-ups without checking them against the released checkpoint.

Here are the verified figures from the official model card:

- About 1 trillion total parameters
- 49 billion active parameters per token, or 52 billion including embeddings and output layers
- 128 routed experts, with 4 active per token, plus 1 shared expert
- A 1.6-billion-parameter Pixtral vision encoder
- A 1-million-token context window
- Text and image input, text output

The distinction between total and active parameters is the whole story for self-hosting. For each token, the router only activates a small subset of experts, so the compute per token is closer to a 52-billion-parameter model. But you still have to store and load all 1 trillion parameters. Every expert has to be somewhere, even if it is not used for a given token.

So "a 52B model" is true for the per-token path and false for the download, the disk, and the memory budget. If your plan starts with "I have run 50B-class models before, so this should be similar," stop there. This is not that plan.

At FP8, each parameter takes roughly one byte. A trillion-parameter pool therefore means a download on the terabyte scale, before you count KV cache, activations, or any workspace for outputs. I am deliberately not quoting a single exact file size here, because the card itself presents checkpoints and recipes rather than one headline download number, and the total depends on which variant and files you pull. The safe planning assumption is hundreds of gigabytes at minimum, and roughly a terabyte at FP8.

NVFP4 is the smaller-footprint official variant. Mistral describes it as having a substantially smaller memory footprint and faster inference, with similar performance to the FP8 checkpoint. It targets Blackwell GPUs — B200, B300, GB200, and GB300 — and supports vLLM and SGLang. Smaller does not mean small. It is still a trillion-parameter model, just stored more compactly.

Context is another place where the headline number needs a second look. The card lists a 1-million-token context window, but that is a capability, not a free default. Long context costs KV cache memory. The official card itself tells you what to do if you run short of GPU memory: lower `max-model-len`, lower concurrency, or both. The Hugging Face vLLM example for NVFP4 uses a 131,072-token maximum model length, not 1 million. Treat 1M as "possible in principle, expensive in practice," and plan your first run with a much shorter context.

The official vLLM and SGLang recipes use tensor parallelism across 8 GPUs as their example configuration. That is the shape of deployment the card is written for: a server node, or rented equivalent, with the model split across devices.

This is the path to take seriously if you already have access to that kind of node, or you are budgeting for one, and you need the full model for internal work, research, or a product feature. It is also the only path where FP8 at anything like full quality and concurrency is a reasonable starting point.

It is not a path you can improvise on a single gaming GPU. There is no official recipe on the card that turns this into a one-card job at FP8.

If you do not have a multi-GPU node, the only route that even approaches a single machine is a heavily quantized GGUF build through llama.cpp. Hugging Face's launch post says the 2-bit quants run on a single Apple Silicon Ultra computer with 512 GB of unified RAM. It also reports, for a Q2_K build on a single M3 Ultra, roughly 90 tokens per second of prefill and 15 tokens per second of generation.

Read that as an experiment, not as a production coding agent. Generation at that speed, with aggressive quantization, is for trying the model, testing prompts, and learning its behavior — not for serving a team at high concurrency. Long-context performance on the most compressed builds drops sharply in the same Hugging Face post. And a community GGUF conversion is not Mistral's own FP8 or NVFP4 checkpoint, so check the source and the exact quant before you trust its output.

If your available hardware is a laptop, a single consumer GPU, or a small cloud instance, there is no verified official path in this release that fits. Downloading hundreds of gigabytes first and discovering you cannot load the model is the most expensive way to learn that.

Fine-tuning is a step above inference, not a shortcut around it. The card lists fine-tuning routes through Axolotl, Unsloth, and TRL, but training or further training a trillion-parameter model needs even more memory and orchestration than serving it. Do not budget a fine-tuning project on the same hardware you would struggle to serve the model on.

The model is released under the Mistral Ecosystem Open-Weight License, a modified version of the MIT license. The official license file in the repository is short, and you should read it yourself before making any commercial decision. The key points, in plain language:

- Use, modification, distribution, and commercial use are permitted in general, with the copyright and permission notice included in copies.
- If you or your affiliates operate a Model-as-a-Service business, you must enter into a separate commercial license with Mistral before using the model or its derivatives for any commercial purpose.
- "Model as a Service" is defined in the license as giving a third party access to the model or its derivatives for inference or post-training, by any means, in a way that lets that third party exercise meaningful control over inputs, parameters, or training data.
- That definition explicitly excludes end-user products where model capabilities are solely embedded within specific features or harnesses, and mere relaying of requests to models hosted by others.
- Internal use — use that does not make the model or its derivatives available to third parties — is exempt from the separate-license requirement, as is use accessed through Mistral's official products.
- The model is provided "as is," without warranty.

The practical boundary is therefore not "commercial versus non-commercial." It is "are you selling access to the model itself?" If your plan is to offer Large 4 to others through an API or hosted endpoint, get the commercial license first. If your plan is internal tooling or a product feature with the model embedded, the license as written treats that differently — but the boundary cases are exactly where you should read the full text, not rely on a summary. This article is not legal advice.

Try it if you check most of these boxes:

- You already have access to a multi-GPU node, preferably Blackwell-class hardware for NVFP4, or a 512 GB unified-memory machine for a 2-bit experiment.
- You can accept a quantized build, shorter context, and lower concurrency for your first run.
- You have a concrete reason to keep the model on your own infrastructure, such as data control, offline use, or custom evaluation, rather than just curiosity.
- You have read the license and your planned use does not cross into Model-as-a-Service without a commercial license.

Wait if any of these are true:

- Your only hardware is a laptop, a single consumer GPU, or a small VPS.
- You need the full 1-million-token context at high concurrency on day one.
- You plan to resell access to the model without first sorting out the commercial license.
- You expected a 52-billion-parameter download and a 52-billion-parameter serving bill. You will get neither.

A useful first check costs nothing: before you start a download, write down the hardware you actually have access to, the context length you actually need, and how many concurrent requests you actually need. If those three numbers do not fit the official 8-GPU recipe or the 512 GB single-machine experiment, the honest answer is not "try harder." It is "not yet, on this hardware."

That is also what makes this release interesting. Open weights do not make a trillion-parameter model cheap to run. They move the cost from an API bill to hardware, storage, and engineering time — and they give you control in return. For a small team, the question is not whether Large 4 is impressive. It is whether that trade is worth it for the specific job you have in mind.

- Official model card: https://huggingface.co/mistralai/Mistral-Large-4-1T-A52B
- Official license in the same repository: https://huggingface.co/mistralai/Mistral-Large-4-1T-A52B/blob/main/LICENSE
- Hugging Face launch post on the release, serving recipes, and quantized builds: https://huggingface.co/blog/mistral-large4

## Top comments (0)
