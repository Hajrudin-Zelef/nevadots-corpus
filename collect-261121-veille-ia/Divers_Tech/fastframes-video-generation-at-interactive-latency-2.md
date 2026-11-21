---
id: collect-261121-veille-ia/divers-tech/fastframes-video-generation-at-interactive-latency-2
title: "FastFrames: Video Generation at Interactive Latency"
domain: divers-tech
role: reference
task: article
actors: []
dates: []
keywords: ["video", "fastframes", "generation", "interactive", "latency", "models", "arxivlabs", "them"]
source: https://arxiv.org/abs/2611.23226v1
source_anchor: ""
source_lines: [1, 23]
section: "Divers_Tech"
---
# Computer Science > Computer Vision and Pattern Recognition

# Title:FastFrames: Video Generation at Interactive Latency

View PDF HTML (experimental)
            Abstract:Autoregressive video diffusion models generate frames based on new observations and inputs, making them a promising medium for interactive visual experiences. However, existing models are orders of magnitude too slow for fluid interaction. This paper introduces FastFrames, a family of fast autoregressive video diffusion models that achieves interactive latency without compromising quality. We profile the inference pipeline of state-of-the-art models and find that operations often treated as fixed implementation details, such as VAE decoding and KV cache updates, in fact account for over half of end-to-end inference latency. We challenge the assumption that these components are indispensable and replace them with lightweight alternatives. Our 1.3B variant runs at 36 ms per frame, or 28 responses per second (RPS), on a single B200 GPU while matching the generation quality of a state-of-the-art model that is nearly 8x slower. Our 14B variant runs at 45 ms per frame, or 22 RPS, on an 8x B200 GPU node and yields significantly better visual quality and realism. We show that FastFrames enables real-time interaction in applications such as camera-conditioned and physics-conditioned video generation.
    

# Bibliographic and Citation Tools

# Code, Data and Media Associated with this Article

# Demos

# Recommenders and Search Tools

# arXivLabs: experimental projects with community collaborators

arXivLabs is a framework that allows collaborators to develop and share new arXiv features directly on our website.

Both individuals and organizations that work with arXivLabs have embraced and accepted our values of openness, community, excellence, and user data privacy. arXiv is committed to these values and only works with partners that adhere to them.

Have an idea for a project that will add value for arXiv's community? **Learn more about arXivLabs**.
