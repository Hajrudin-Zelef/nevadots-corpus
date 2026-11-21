---
id: collect-261121-veille-ia/minimax/run-minimax-m3-1-flash-and-m3-locally-the-realistic-path-and-m2-tokios
title: "Run MiniMax M3.1 Flash and M3 locally — the realistic path (and M2) - Tokios"
domain: minimax
role: reference
task: article
actors: ["MiniMax"]
dates: []
keywords: ["flash", "minimax", "community", "gguf", "model", "tokios", "minimax-m3", "connector"]
source: https://docs.tokios.com/models/minimax
source_anchor: ""
source_lines: [1, 61]
section: "MiniMax"
---
**MiniMax M3.1 Flash**(released October 23, 2026) is the current flagship — a

**~428B mixture-of-experts**model (about 23B active per token) with native multimodal input (text, image, video),

**1M-token context**, and DSpark speculative decoding. The earlier

**M3**is superseded by M3.1 Flash. The

**M2 line**(M2 → M2.5 → M2.7) is the earlier, coding-agent-oriented series at ~230B; it has the most mature community quantization ecosystem. None of these are single-consumer-GPU models, but the M2 line

*is*reachable on a big-memory Mac or workstation. MiniMax publishes only

**safetensors**(BF16/F32) in its own repos — no official GGUF, AWQ, or MLX. Every quantized build comes from the community (Unsloth, bartowski, and others). What that means in practice:

**big-memory**model, not a laptop-GPU one. If you have a 128 GB+ unified-memory Mac or a workstation with 128–512 GB of RAM, the M2 line runs via community GGUF. The M3.1 Flash and M3 need 249+ GB at Q4 — plan for 4x DGX Spark, 2x Mac Studio M5 Ultra 512 GB, or similar. Below that, MiniMax is effectively an API/cloud model.

`minimax-m3-1-flash`, `minimax-m3`, `minimax-m2.7`, `minimax-m2.5`, and `minimax-m2` all exist in the Ollama library — but each exposes only a **tag. Those proxy requests to MiniMax’s hosted API; the weights never download to your machine, so there’s nothing local for a connector to point at. To self-host MiniMax on hardware you control, use a community GGUF (**

`:cloud``llama.cpp`/LM Studio) or the safetensors (vLLM/SGLang), below — not Ollama’s `:cloud` tag.
- Community GGUF — M3.1 Flash (multi-GPU server)
- Community GGUF — M2 line (big-RAM Mac/workstation)
- vLLM / SGLang — M3.1 Flash (multi-GPU server)
- vLLM / SGLang — M3 (multi-GPU server, superseded)

`unsloth/MiniMax-M3.1-Flash-GGUF`. Q4_K_XL is ~249 GB — needs 4x DGX Spark, 2x Mac Studio M5 Ultra 512 GB, or similar. Experimental VQ-GGUF from Baekpica pushes routed experts to ~2.0 bpw for a smaller footprint. Run through `llama.cpp` or LM Studio for an OpenAI-compatible API on `http://127.0.0.1:8080/v1`.
**superseded**on MiniMax’s own docs — M3.1 Flash is current. Verify against the model card before standardizing.

`MiniMaxAI/MiniMax-M3.1-Flash`, `MiniMaxAI/MiniMax-M3`, `MiniMaxAI/MiniMax-M2`, MiniMax release notes, ollama.com/library/minimax-m3-1-flash.
**Licensing varies a lot by version.**M2 is MIT-with-attribution (permissive), M2.7 is

**non-commercial**(commercial use needs written authorization), and M3 uses the

**MiniMax Community License**(free below a revenue threshold, attribution “Built with MiniMax M3”). Read the

`LICENSE`/`LICENSE-MODEL` file on the specific model card before any commercial deployment.
- **M3.1 Flash** if you want the current flagship — DSpark speculative decoding, multimodal, 1M context — and have a multi-GPU server (249+ GB at Q4) or accept the`llama.cpp` sparse-attention limitation on a big-RAM box.
- **M3** if you need the earlier release for a specific reason (superseded by M3.1 Flash).
- **M2 / M2.7** if you’re on a 128 GB unified-memory Mac or big-RAM workstation and want a coding-agent model that runs*today* via mature community GGUFs. M2 has the deepest third-party tooling; M2.7 is the newer coding release (non-commercial license).

`llama.cpp`, vLLM, or SGLang exposing an OpenAI-compatible `/v1` — pair the Tokios connector so you can reach it as `https://api.tokios.com` from Claude Code, Cline, or any other compatible client, instead of only from that one machine.
Pair the Tokios connector

**Setup**tab, and click

**Start pairing**to get a one-time claim code (

`TKS-XXXX-XXXX`). Install and run the connector on the machine hosting MiniMax:
**approve**the device on the

**Setup**or

**Connectors**tab.

Point the connector at your runtime's /v1 API

Register a deployment

- MiniMax publishes **safetensors only** — every GGUF/AWQ/MLX build is community-made. Unsloth publishes GGUF builds for M3.1 Flash at`unsloth/MiniMax-M3.1-Flash-GGUF` . Check the quant author before you rely on one.
- **Ollama’s `minimax-*` tags are `:cloud`-only** — they run on MiniMax’s infrastructure, not yours. Self-host via community GGUF or the safetensors if you want it local.
- **`llama.cpp` doesn’t support MiniMax Sparse Attention yet** , so local GGUF runs of M3.1 Flash and M3 fall back to dense attention and lose the full long-context advantage — use vLLM/SGLang for the real thing.
- These are **big-memory MoE** models — all params must fit in memory even though only ~10–23B are active per token. M3.1 Flash at Q4 needs ~249 GB. Plan for a multi-GPU server, not a single consumer GPU.
