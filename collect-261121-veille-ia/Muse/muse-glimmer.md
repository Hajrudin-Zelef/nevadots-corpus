---
id: collect-261121-veille-ia/muse/muse-glimmer
title: "Muse Glimmer"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: []
keywords: ["muse", "glimmer", "text", "model", "multimodal", "weights", "spark", "local"]
source: https://dev.meta.ai/docs/muse-glimmer?team_id=1628803112160091&project_id=1608520037448754
source_anchor: ""
source_lines: [1, 33]
section: "Muse"
---
Muse Glimmer is Meta's 30-billion-parameter open-source multimodal model, distilled from Muse Spark and built for local agentic workflows. Run it on your own hardware through supported runtimes. It ships with open weights, reads text and images, and reasons step by step before it answers.

Point your existing tooling at a local Muse Glimmer server and keep building — you own the weights, the runtime, and the data.

| Model | Parameters | Architecture | Context window | 
|---|---|---|---|
| Muse Glimmer | 30B | Dense, multimodal (text + image input; text output) | 128K tokens by default; longer contexts supported | 

Muse Glimmer is a dense, decoder-only multimodal transformer with a built-in vision encoder. It is trained from Muse Spark's outputs rather than from scratch. It accepts text and image input, produces text output, and has a default context window of 128K tokens with support for longer contexts.

Muse Glimmer was trained on data from more than 100 languages.

Muse Glimmer weights are released under the Apache License 2.0(opens in new tab).

The Muse Glimmer launch ecosystem includes:

- AMD
- Arm
- Dell
- Fireworks AI
- Hugging Face
- Intel
- llama.cpp
- LM Studio
- NVIDIA
- Ollama
- OpenRouter
- SGLang and RadixArk
- Together AI
- Unsloth
- vLLM and Inferact

Now that you know what Muse Glimmer offers, get the model and run your first inference. If you're ready to optimize, explore quantization to fit the model on smaller hardware, or speculative decoding to accelerate generation. To adapt Muse Glimmer to your domain, see post-training.
