---
id: collect-261121-veille-ia/modeles-open/ling-3-0-and-3-1-architecture-the-rabbit-hole
title: "Ling 3.0 and 3.1 architecture — The Rabbit Hole"
domain: modeles-open
role: reference
task: article
actors: []
dates: ["2026-11-01"]
keywords: ["layers", "attention", "ling", "kimi", "delta", "latent", "gated", "flash"]
source: https://ruben.galvao.co.uk/rabbit-hole/ai/models/ling-3-0-flash/
source_anchor: ""
source_lines: [1, 78]
section: "Modeles_Open"
---
The Rabbit Hole / AI / Model architectures

Ant Group (inclusionAI) · Architecture map
# Ling 3.0 and 3.1

Hybrid Kimi Delta and gated latent-attention models, with 3.1 expanding to 560B parameters and a 7:1 layer pattern.

Ling 3.0 flash/tiny and Ling 3.1 flash, including the Flash 1019 weight update · Ling release timeline

## Inside Ling 3.0 and 3.1

## Inside linear attention · Layer 1

Earlier context dimmed · current token bright · dashed path carries state to the next step.

## Inside latent attention · Gated MLA layers 4, 8, 12 and so on to 24

Earlier context dimmed · current token bright · dashed regions contain cached entries.

## Model-specific details

- Layer 1 · 1 repetitions
- Kimi Delta Attention with a dense feed-forward network (intermediate size 4608).
- Kimi Delta Attention layers (three of every four) · 17 repetitions
- Linear attention, then 128 routed experts (size 512), 8 chosen per token, plus 1 shared expert (size 512).
- Gated MLA layers 4, 8, 12 and so on to 24 · 6 repetitions
- Full softmax attention with a latent KV cache and a head-wise output gate. Same MoE block as the other layers.
- Attention and positions
- 18 layers use Kimi Delta Attention (16 heads of size 128, short convolution of width 4). 6 layers, the fourth of every four, use latent attention with a per-head sigmoid output gate: 16 heads, query rank 256, KV latent 512, key 128 content plus 64 rotary, value 128.
- Memory
- MLA layers cache a 512-value KV latent plus a 64-value rotary key per token. Kimi Delta Attention layers keep a fixed-size state and a short convolution buffer.

## Inside linear attention · Layers 1–4

Earlier context dimmed · current token bright · dashed path carries state to the next step.

## Inside latent attention · Gated MLA layers

Earlier context dimmed · current token bright · dashed regions contain cached entries.

## Model-specific details

- Layers 1–4 · 4 repetitions
- Kimi Delta Attention followed by a dense 10240-wide feed-forward network.
- Kimi Delta Attention layers · 59 repetitions
- Seven linear layers per group of eight. 512 routed experts, eight selected, plus one shared expert; expert width 1280.
- Gated MLA layers · 9 repetitions
- Every eighth layer uses 64-head latent attention, query rank 1536, KV rank 512, 128 content and 64 rotary key values, value width 128, and a head-wise sigmoid output gate.
- Attention and positions
- 63 Kimi Delta Attention layers and nine gated MLA layers in a 7:1 pattern. The MLA query rank is 1536 and its latent KV cache has width 512 plus 64 rotary values.
- Memory
- The nine MLA layers cache 576 values per token each. KDA layers keep recurrent state and a width-four convolution buffer.

## Three hybrid variants

- **3.0 flash** places gated MLA every sixth layer;**3.0 tiny** uses it every fourth layer.
- **3.1 flash** expands to width 4096 and 72 layers: seven Kimi Delta Attention layers followed by one gated MLA layer.
- The first four 3.1 layers use dense feed-forward networks; the rest select eight of 512 routed experts plus one shared expert.
- 3.1 compresses queries to 1536 values before expanding them into attention heads. Its MLA cache remains 512 latent values plus 64 rotary values per token.

## Flash 1019 checkpoint

Ling-3.1-flash-1019 has published MIT-licensed weights. It updates post-training while retaining the 560B / 25B-active, 63 KDA + nine gated-MLA layout of 3.1. The earlier checkpoint remains a separate benchmark result.

## How it scores

### AA Intelligence Index

Version 4.3.2 · higher is better · checked 2026-11-01. Separate from Epoch ECI.

| Checkpoint / size | Result | Evaluation | 
|---|---|---|
| Ling 3.0 Flashflash | **20** | AA Index 4.3.2Published checkpoint | 
| Ling 3.0 Tinytiny | **11** | AA Index 4.3.2Published checkpoint | 
| Ling 3.1 Flashflash 3.1 | **41** | AA Index 4.3.2Published checkpoint | 
| Ling 3.1 Flash 1019flash 3.1 | **44** | AA Index 4.3.21019 checkpoint; published evaluated setting | 

**44**. Distribution of 97 scored variants in this atlas, using each variant’s best checkpoint. Known estimates excluded.
