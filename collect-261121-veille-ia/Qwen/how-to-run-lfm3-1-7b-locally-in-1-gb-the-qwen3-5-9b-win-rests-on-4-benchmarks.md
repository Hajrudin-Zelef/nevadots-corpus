---
id: collect-261121-veille-ia/qwen/how-to-run-lfm3-1-7b-locally-in-1-gb-the-qwen3-5-9b-win-rests-on-4-benchmarks
title: "How to Run LFM3-1.7B Locally in 1 GB: The Qwen3.5-9B Win Rests on 4 Benchmarks"
domain: qwen
role: reference
task: article
actors: ["Alibaba"]
dates: ["2026-10-27"]
keywords: ["lfm3-1", "llama", "qwen3", "liquid", "agent", "model", "context", "lfm3"]
source: https://dev.to/alanwest/how-to-run-lfm3-17b-locally-in-1-gb-the-qwen35-9b-win-rests-on-4-benchmarks-201d
source_anchor: ""
source_lines: [1, 128]
section: "Qwen"
---
Liquid AI's own benchmark table puts LFM3-1.7B, its new 1.7B text model, ahead of Qwen3.5-9B on average. It fits in about 1 GB at 4-bit, reads 256K tokens of context, and was post-trained for tool calling inside agent harnesses. The same model card tells you not to use it for agentic coding or complex reasoning. Both are true, and reading them side by side tells you what the model is for.

If you just want it running:


```
ollama run lfm3
```
The rest of this post covers llama.cpp (GGUF), MLX and ONNX setup, which sampling settings to use, which quant to pick, what the license allows, and how the "beats Qwen3.5-9B" average is built. Short version on that last one: drop four of the 29 benchmarks and Qwen3.5-9B leads again. I haven't run it myself yet. Every number here comes from Liquid's model card, the format repos, or the LFM3 launch post from October 27, 2026. This one is text only.

- 1.73B parameters
- 24 layers: 13 gated short convolutions, 5 sliding-window attention, 6 global attention
- 256K context (input + output), 128K vocab, 16 languages
- Pre-trained on 22T tokens, then SFT, RL, distillation from six specialist teachers, and agentic RL
- Post-trained for Hermes Agent, Pi and OpenClaw

The architecture matters most for memory. According to the launch post, a full 128K-token context needs a 1.6 GB KV cache, against 4.3 GB for Qwen3.5-4B and 5.6 GB for MiniCPM5-2B. With only 6 global-attention layers there's very little cache to grow, which is how long context fits on a laptop.

The command at the top needs Ollama v0.40.12 or newer. The GGUF page lists it for macOS only, so on Linux or Windows go through llama.cpp or LM Studio instead.

You need llama.cpp b11960 (v0.8.0) or newer, the first build Liquid lists with LFM3 support.


```
llama-cli -hf LiquidAI/LFM3-1.7B-GGUF:Q4_K_M \
  --temp 0.1 --top-k 50 --repeat-penalty 1.1
```
For an OpenAI-compatible endpoint that your agent harness can use, Liquid's llama.cpp guide uses `llama serve`:


```
llama serve -hf LiquidAI/LFM3-1.7B-GGUF:Q4_K_M
```
The Hermes Agent, Pi and OpenClaw examples in Liquid's agent harness guide all point at `http://localhost:8080/v1`, but the client example in the llama.cpp guide uses port 9931, so check which port your server actually prints.

Pick the quant on purpose:

| File | Size | Note | 
|---|---|---|
| Q4_0 (QAD) | 1.05 GB | Quantization-aware distilled checkpoint | 
| Q4_K_M | 1.1 GB | Default in the main examples | 
| Q5_K_M | 1.26 GB |  | 
| Q6_K | 1.43 GB |  | 
| Q8_0 | 1.85 GB |  | 
| NVFP4 | 1.28 GB | Needs b12088+; FP4 tensor cores on Blackwell | 
| BF16 | 3.47 GB |  | 

I'd try the Q4_0 file first. `LFM3-1.7B-QAD-Q4_0.gguf` was trained to recover the accuracy lost to Q4_0 quantization, and Liquid ran its own edge speed numbers on Q4_0 GGUFs:


```
llama-cli -hf LiquidAI/LFM3-1.7B-GGUF \
  --hf-file LFM3-1.7B-QAD-Q4_0.gguf \
  --temp 0.1 --top-k 50 --repeat-penalty 1.1
```
Avoid NVFP4 on older builds. The repo warns that "Older builds load the file but produce incorrect output." So you get wrong answers and no error, which is miserable to debug in an agent loop.

LM Studio needs llama.cpp engine 2.65.1 or newer.

The weights in the MLX-4bit repo are 973 MB. One thing caught me off guard: you need mlx-vlm here, because at the time of writing no mlx-lm release loads LFM3, and support is still in review in ml-explore/mlx-lm#2016.


```
pip install -U "mlx-vlm>=0.7.11"
python -m mlx_vlm.generate --model LiquidAI/LFM3-1.7B-MLX-4bit \
  --prompt "What is the capital of Australia?" --enable-thinking \
  --temperature 0.1 --top-k 50 --repetition-penalty 1.1 --max-tokens 16384
```
Reasoning is off on this path unless you pass `--enable-thinking`. On the Transformers chat template it's on by default, which is easy to miss when you compare outputs across runtimes.

The ONNX repo ships q4 (1.63 GB, CPU), q4f16 (1.38 GB, GPU), q8, fp16 and others. Two gotchas from the card:

- On GPU, use fp16, q4f16 or q8f16. The CUDA provider has no fp32 GroupQueryAttention kernel, so other graphs fall back to CPU for attention.
- In the browser, use `model_q4f16` with`import * as ort from 'onnxruntime-web/webgpu'` , and prefill in chunks of at most 256 tokens.

Note that the "1 GB" claim applies to the GGUF and MLX builds. The smallest ONNX file is 1.38 GB.

Liquid's recommendation for chat, tool use, agents and coding:


```
temperature 0.1, top_k 50, repetition_penalty 1.1
```
For math and STEM: temperature 1.0, top_p 0.95, top_k 20.

What bugs me is that the benchmark table was produced with vLLM in BF16 at temperature 0.6 and top_p 0.95. So the scores you're reading weren't measured at the recommended settings, or at 4-bit. A Korean review on tilnote points out the same thing: these are BF16 server scores, and the published material doesn't say whether they hold up after quantization. The QAD Q4_0 file suggests Liquid knows quantization costs something.

Liquid's headline numbers: LFM3-1.7B averages 49.37 across 29 benchmarks, against 47.24 for Qwen3.5-9B and 31.55 for LFM2.5-2.6B. I checked the arithmetic against the table and it holds. What goes into that average is another matter.

| Benchmark | LFM3-1.7B | Qwen3.5-9B | 
|---|---|---|
| BFCL V4 | **71.39** | 62.83 | 
| AutomationBench | **41.29** | 15.10 | 
| IFBench | **73.03** | 48.84 | 
| AA-Omniscience | **0.50** | -28.00 | 
| AA-Omniscience non-hallucination | **92.93** | 52.59 | 
| GPQA Diamond | 63.37 | **80.40** | 
| SWE-bench Verified (OpenCode) | 43.40 | **51.20** | 
| Terminal-Bench 2.1 | 12.92 | **17.98** | 
| RULER 128K | 65.56 | **87.11** | 
| MCP-Atlas | 32.57 | **42.67** | 

LFM3 wins 14 of the 29 rows. Four rows (AutomationBench, IFBench, and the two AA-Omniscience scores) give it margins of 24 to 40 points. If I drop those four and re-average the other 25, Qwen3.5-9B leads 51.26 to 48.96. That's my own arithmetic on their table; I didn't run any evals.

The table also counts AA-Omniscience three times: the overall score, accuracy and non-hallucination each get a row. LFM3 wins two of those three by 28.5 and 40.34 points, so one benchmark carries a lot of the average.

The AA-Omniscience result is still interesting. 92.93% non-hallucination with only 7.07% accuracy means the model mostly declines when it doesn't know. For a small agent that's a real feature. It's also no help when you need it to know things.

RULER 128K deserves a look too. The 256K context exists, but at 128K the model scores 65.56 against 84.63 for Qwen3.5-4B. You can fit long context in memory. I wouldn't count on accurate retrieval from the far end of it.

The model card is honest about limits: "not recommended for agentic coding or complex multi-step reasoning," and use retrieval or search for knowledge-heavy work. The launch post says "Larger models still win on agentic coding and knowledge, where size matters most." Given that, here's where it fits:

- Tool routing with a small, well-described toolset. BFCL V4 71.39 and ToolSandbox 80.36 are its best tool-use scores. Use `defer_loading=True` so tool schemas stay out of the prompt until a search tool surfaces them.
- Structured extraction. IFEval 96.66 and IFStruct 93.72. Pulling JSON out of messy documents is a good fit, and Liquid's SFT notebook does exactly that.
- Inbox triage and calendar work. These are Liquid's own examples, and they're short-horizon, tool-heavy tasks.
- A fine-tuned specialist. There are TRL notebooks for SFT, DPO, GRPO (a 108-tool phone app) and CPT. To me, a 1.7B model you can tune on one GPU is the most useful thing in this release.

What I wouldn't give it: anything that edits your repo unattended, anything where a confident wrong answer costs money, and MCP setups with lots of tools (MCP-Atlas 32.57 vs 42.67).

One parsing note: tool calls come out as XML (`<tool_call>` wrapping `<function=NAME>` with `<parameter=NAME>` children), not JSON. Liquid's docs describe the format as a superset of `qwen3_coder`, so a parser written for Qwen3-Coder is a reasonable starting point. If your harness expects OpenAI-style JSON from raw completions, you'll need one.

The weights use the LFM Open License v1.0. Commercial use is licensed while your entity's annual revenue is under the threshold, defined as "annual revenue of 10 million United States dollars ($10,000,000) or more." Above that, commercial use isn't covered by the license. Qualified nonprofits are exempt from the threshold for non-commercial or research use. Fine-tunes are allowed. You must keep notices and ship the license, and if you sue claiming the work infringes a patent, your patent license ends. Fine for side projects and startups. If you work at a bigger company, talk to legal first, because this isn't Apache 2.0.

A 1 GB model that handles tool calls this well is good news for local agents, and the KV cache numbers are what make it work on a phone. The "beats Qwen3.5-9B" framing depends on which benchmarks went into the average. I'd treat it as a router and extractor and keep a bigger model around for reasoning. Before you trust it, run your own tool-call evals at Q4.

So: what's the smallest model you've trusted with real tool calls in production? And what would you actually hand to a 1 GB agent, or is the answer "nothing that matters"?

## Top comments (0)
