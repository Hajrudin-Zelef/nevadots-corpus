---
id: collect-261121-veille-ia/meituan-longcat/meituan-longcat-longcat-video-avatar-1-6-at-main
title: "meituan-longcat/LongCat-Video-Avatar-1.6 at main"
domain: meituan-longcat
role: reference
task: article
actors: []
dates: []
keywords: ["meituan-longcat", "longcat-video-avatar-1", "diffusers", "main", "import", "torch", "diffusionpipeline", "pipe"]
source: https://huggingface.co/meituan-longcat/LongCat-Video-Avatar-1.6/tree/main
source_anchor: ""
source_lines: [1, 12]
section: "Meituan_LongCat"
---
Instructions to use meituan-longcat/LongCat-Video-Avatar-1.6 with libraries, inference providers, notebooks, and local apps. Follow these links to get started.

How to use meituan-longcat/LongCat-Video-Avatar-1.6 with Diffusers:

pip install -U diffusers transformers accelerate

import torch
from diffusers import DiffusionPipeline
# switch to "mps" for apple devices
pipe = DiffusionPipeline.from_pretrained("meituan-longcat/LongCat-Video-Avatar-1.6", dtype=torch.bfloat16, device_map="cuda")
prompt = "Astronaut in a jungle, cold color palette, muted colors, detailed, 8k"
image = pipe(prompt).images[0]
