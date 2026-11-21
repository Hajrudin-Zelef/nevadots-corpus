---
id: collect-261121-veille-ia/grok-xai/minimaxai-minimax-h3-image-at-main
title: "MiniMaxAI/MiniMax-H3-Image at main"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: []
keywords: ["minimaxai", "minimax-h3-image", "diffusers", "main", "import", "torch", "diffusionpipeline", "pipe"]
source: https://huggingface.co/MiniMaxAI/MiniMax-H3-Image/tree/main
source_anchor: ""
source_lines: [1, 10]
section: "Grok_xAI"
---
How to use MiniMaxAI/MiniMax-H3-Image with Diffusers:

pip install -U diffusers transformers accelerate

import torch
from diffusers import DiffusionPipeline
# switch to "mps" for apple devices
pipe = DiffusionPipeline.from_pretrained("MiniMaxAI/MiniMax-H3-Image", dtype=torch.bfloat16, device_map="cuda")
prompt = "Astronaut in a jungle, cold color palette, muted colors, detailed, 8k"
image = pipe(prompt).images[0]
