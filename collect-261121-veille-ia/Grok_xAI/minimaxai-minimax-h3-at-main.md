---
id: collect-261121-veille-ia/grok-xai/minimaxai-minimax-h3-at-main
title: "MiniMaxAI/MiniMax-H3 at main"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: []
keywords: ["minimaxai", "minimax-h3", "main", "import", "torch", "diffusionpipeline", "pipe", "prompt"]
source: https://huggingface.co/MiniMaxAI/MiniMax-H3/tree/main
source_anchor: ""
source_lines: [1, 6]
section: "Grok_xAI"
---
import torch
from diffusers import DiffusionPipeline
# switch to "mps" for apple devices
pipe = DiffusionPipeline.from_pretrained("MiniMaxAI/MiniMax-H3", dtype=torch.bfloat16, device_map="cuda")
prompt = "Astronaut in a jungle, cold color palette, muted colors, detailed, 8k"
image = pipe(prompt).images[0]
