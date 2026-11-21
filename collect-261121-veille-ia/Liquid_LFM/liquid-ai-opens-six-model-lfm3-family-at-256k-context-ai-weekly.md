---
id: collect-261121-veille-ia/liquid-lfm/liquid-ai-opens-six-model-lfm3-family-at-256k-context-ai-weekly
title: "Liquid AI opens six-model LFM3 family at 256K context | AI Weekly"
domain: liquid-lfm
role: reference
task: article
actors: ["Liquid AI"]
dates: []
keywords: ["liquid", "lfm3", "family", "context", "models", "text", "model", "base"]
source: https://aiweekly.co/alerts/liquid-ai-opens-six-model-lfm3-family-at-256k-context
source_anchor: ""
source_lines: [1, 21]
section: "Liquid_LFM"
---
## TL;DR

- Liquid AI released LFM3, a family of six 2B-class open-weight models covering text, vision, audio, omni and two decision variants, all branched from a single 1.7B base backbone.
- The text model is pre-trained on 22T tokens with a 256K context window and fits in roughly 1 GB at 4-bit precision.
- The company says LFM3-1.7B decodes 1.8 to 2.8 times faster than the next model across the three on-device setups it tested.

Liquid AI released LFM3 on October 27, a family of six 2B-class open-weight models covering text, vision, audio, omni and two decision variants, all branched from one 1.7B base. In its launch post, the company writes that "the first LFM3 release includes six models, all branched from the same LFM3-1.7B-Base backbone," and that "LFM3-1.7B-Base was pre-trained on 22T tokens." The text model carries a 256K context window.

The backbone is a tri-hybrid of gated short-convolution layers, sliding-window attention and global attention. The mix is 13 convolution layers, 5 sliding-window attention layers with a 256-token window, and 6 global attention layers across 24 blocks, with a kernel size of 7 and a 128K vocabulary. Liquid says the 1.7B text model beats its own prior LFM2.5-2.6B while using 35% fewer parameters, posting 45.60 on LiveClaw, 71.39 on BFCL V4 tool-calling, 96.66 on IFEval, 80.36 on ToolSandbox and 52.83 on AA-LCR.

Where the pitch lands is footprint. The 1.7B base fits in roughly 1 GB at 4-bit precision. The post reports that "LFM3-1.7B is the fastest model on all three devices, decoding 1.8 to 2.8 times faster than the next one," and that a single NVIDIA B300 can produce 3.6 billion tokens per day. Supported targets listed include Samsung Galaxy S26 Ultra, OnePlus 15, Raspberry Pi 5, Apple M4 Max, AMD Ryzen AI and Qualcomm Hexagon NPU.

Ambarella's Senior Director for AI, Malhar Palkar, framed the brief in a launch comment: "The challenge is delivering these capabilities within the compute, memory, and power constraints of edge devices." "The base and post-trained models are open-weight and available today on Hugging Face," the post states. The release lands amid a steady run of nearly a hundred multimodal releases we have tracked over the past 90 days.

## Shared on Bluesky by 1 AI expert

          Originally reported by
          **liquid.ai**
        

Original headline: Liquid AI Opens LFM3 Family of 2B-Class Multimodal Models With 256K Context and ~1GB 4-Bit Footprint
