---
id: collect-261121-veille-ia/liquid-lfm/bambocher-lfm3-1-7b-heretic-hugging-face
title: "bambocher/LFM3-1.7B-heretic · Hugging Face"
domain: liquid-lfm
role: reference
task: article
actors: ["Liquid AI"]
dates: []
keywords: ["lfm3-1", "model", "inference", "start", "local", "checkpoint", "quantization", "chat"]
source: https://huggingface.co/bambocher/LFM3-1.7B-heretic
source_anchor: ""
source_lines: [1, 260]
section: "Liquid_LFM"
---
# This is a decensored version of LiquidAI/LFM3-1.7B, made using Heretic v2.1.0

**This model is reproducible!**

See the README in the `reproduce` directory for more information.


| Parameter | Value | 
|---|---|
| **start_layer_index** | 5 | 
| **end_layer_index** | 23 | 
| **preserve_good_behavior_weight** | 0.9461 | 
| **steer_bad_behavior_weight** | 0.0089 | 
| **overcorrect_relative_weight** | 0.0330 | 
| **neighbor_count** | 2 | 

| Metric | This model | Original model (LiquidAI/LFM3-1.7B) | 
|---|---|---|
| **Refusals** | 0/100 | 93/100 | 
| **KL divergence** | 0.7091 | 0 *(by definition)* | 

LFM3-1.7B is a **text model for local agents and specialized applications**, based on our new tri-hybrid architecture.

- **Agentic training:** post-trained for agentic harnesses such as Hermes Agent, Pi, and OpenClaw, with a 256K-token context window.
- **Designed for the edge:** at 4-bit precision, it takes about 1 GB of memory, so it fits on any phone or laptop.
- **Made to be specialized:** adapt the model to your domains, formats, and tools via quick single-GPU fine-tuning.

Find more information about the LFM3 family in our release blog post.

The LFM3 family includes base, text, vision, audio, omni, and decision models.

| Property | Specification | 
|---|---|
| **Checkpoint** | Post-trained, text-only language model | 
| **Parameters** | 1.73B | 
| **Architecture** | 24 layers: 13 gated short convolutions, 5 sliding-window attention, 6 global attention | 
| **Context length** | 256K tokens (input + output) | 
| **Vocabulary** | 128,000 tokens | 
| **Languages** | English, Arabic, Chinese, French, German, Italian, Japanese, Korean, Portuguese, Spanish, Vietnamese, Thai, Indonesian, Hindi, Russian, Polish | 
| **License** | LFM Open License v1.0 | 

LFM3-1.7B is designed for local agents, tool calling, structured outputs, and domain-specific fine-tuning. Use retrieval or search for knowledge-heavy tasks. It is not recommended for agentic coding or complex multi-step reasoning due to its small size.

Choose a checkpoint variant for your inference framework and hardware.

| Checkpoint | Framework | Description | 
|---|---|---|
| BF16 | vLLM, SGLang, Transformers | BF16 weights for GPU inference. | 
| FP8 | SGLang | FP8 quantization for GPU inference. | 
| NVFP4 | SGLang | 4-bit floating-point quantization for GPU inference. | 
| GGUF | llama.cpp, Ollama, LM Studio | Local inference on CPUs and GPUs. | 
| MLX | MLX | 4-bit quantization for Apple Silicon. | 
| ONNX | ONNX Runtime | Cross-platform inference with ONNX Runtime. | 
| OpenVINO | OpenVINO | 4-bit quantization for Intel hardware. | 

**Sampling parameters:**

- **General** (chat, tool use, agents, coding):`temperature=0.1,top_k=50, repetition_penalty=1.1`
- **Math & STEM** (math, science):`temperature=1.0, top_p=0.95, top_k=20`

**Transformers quick start:**

```
from transformers import pipeline
generator = pipeline(
    "text-generation",
    model="LiquidAI/LFM3-1.7B",
    trust_remote_code=True,
)
messages = [{"role": "user", "content": "What is C. elegans?"}]
output = generator(
    messages,
    do_sample=True,
    temperature=0.1,
    top_k=50,
    repetition_penalty=1.1,
    max_new_tokens=2048,
)
print(output[0]["generated_text"][-1]["content"])
```
For local agent setup, see the Agent Harnesses guide.

## **Chat template**

LFM3 uses this ChatML format, generated from the checkpoint's chat template:

```
<|startoftext|><|im_start|>system
You are a helpful assistant trained by Liquid AI.<|im_end|>
<|im_start|>user
Explain how a heat pump works.<|im_end|>
<|im_start|>assistant
<think>
```
Use `tokenizer.apply_chat_template()` with `add_generation_prompt=True` to format messages automatically.

Reasoning is enabled by default. You can set `enable_thinking=False` to disable thinking traces, but this behavior can be inconsistent, especially for reasoning-heavy tasks.

Reasoning from turns before the latest user message is removed unless `preserve_thinking=True`.

## **Tool calling**

Pass function schemas through `tools=...` to `apply_chat_template()`. The template formats tool definitions and results automatically. Generated calls use XML:

```
<tool_call>
<function=get_weather>
<parameter=city>
Paris
</parameter>
</function>
</tool_call>
```
Execute the call in your application. Add the parsed assistant call to history as a `tool_calls` entry, then append the result as a `tool` message. Keep passing the tool schemas on subsequent requests and preserve the assistant's content and reasoning from the current tool-use turn.

`function.arguments` must be a Python mapping. Decode JSON strings with `json.loads()` before applying the template.

## **Tool search**

LFM3 supports deferred tools. Set `defer_loading=True` to keep tool definitions out of the prompt until your harness's search tool returns their names as `tool_reference` parts. The chat template then inserts their definitions into `<additional_tools>`.

```
tools = [
    search_tool,
    {"type": "function", "defer_loading": True, "function": get_weather_schema},
]
messages.append({
    "role": "tool",
    "tool_call_id": call_id,
    "content": [
        {"type": "text", "text": "Found 1 tool."},
        {"type": "tool_reference", "name": "get_weather"},
    ],
})
```
Your harness implements and runs the search. Keep the full `tools` list, including deferred tools, on every request. In the system prompt, tell the model when to search:

You only have one tool right now: search_tools, which searches a catalog of many more tools. If none of your current tools can handle a request, call search_tools to find one, then call the tool it returns.


Works with vLLM, SGLang, and LFM3-compatible llama.cpp builds. For llama.cpp, use a GGUF with the current chat template or load it with `--chat-template-file`.

See Deferred tools and tool search for the full guide.

LFM3-1.7B is designed for **easy customization** for a given domain, task, or application via TRL.

| Training | Use case | Notebook | 
|---|---|---|
| **SFT** | Extract structured JSON from messy documents |  | 
| **DPO** | Answer from retrieved documents, or decline |  | 
| **GRPO** | Call the right tools in a 108-tool phone app |  | 
| **CPT** | Learn a new language (Turkish) from raw text |  | 

LFM3-1.7B can power agentic applications that used to need 9B parameters.

| Benchmark | **LFM3 1.7B** | LFM2.5 2.6B | MiniCPM5 2B | Gemma 4 E4B | Qwen3.5 4B | Qwen3.5 9B | 
|---|---|---|---|---|---|---|
| Agents |  |  |  |  |  |  | 
| LiveClaw | **45.60** | 14.70 | 30.37 | 20.36 | 19.94 | __35.77__ | 
| Claw-Eval General (EN), average | **68.64** | 62.63 | 60.74 | 56.14 | 61.96 | __66.53__ | 
| Claw-Eval General (EN), pass^3 | __51.40__ | 37.52 | 34.58 | 22.43 | 42.99 | **51.87** | 
| BrowseComp-Plus (OpenClaw subset) | __37.00__ | 26.33 | 16.83 | 21.00 | 24.67 | **38.00** | 
| WebArena-Verified | **38.42** | 3.02 | 9.11 | 17.06 | 21.92 | __28.94__ | 
| EnterpriseOps-Gym | __28.20__ | 7.94 | 13.41 | 9.86 | 25.96 | **28.97** | 
| AutomationBench (public) | **41.29** | 0.94 | 4.71 | 1.92 | 8.67 | __15.10__ | 
| Tool use |  |  |  |  |  |  | 
| BFCL V4 | **71.39** | 55.89 | 62.65 | 48.25 | 55.12 | __62.83__ | 
| ToolSandbox | **80.36** | __78.04__ | 68.16 | 69.27 | 75.40 | 76.41 | 
| MCP-Atlas | 32.57 | 16.45 | 26.38 | 6.19 | __41.37__ | **42.67** | 
| τ³-bench (4 domains), average | __68.01__ | 56.77 | **69.44** | 33.53 | 65.98 | 67.11 | 
| Agentic coding |  |  |  |  |  |  | 
| SWE-bench Verified (OpenCode) | __43.40__ | 18.00 | 34.80 | 0.50 | 41.20 | **51.20** | 
| SWE-bench Multilingual (OpenCode) | 23.00 | 8.00 | 15.67 | 0.67 | __23.33__ | **35.00** | 
| Terminal-Bench 2.1 (OpenCode) | 12.92 | 0.00 | 5.62 | 0.00 | __13.48__ | **17.98** | 
| Coding |  |  |  |  |  |  | 
| LiveCodeBench v6 | **73.46** | 60.57 | 70.52 | __71.00__ | 61.99 | 69.86 | 
| LiveCodeBench Pro 25Q2 | __31.74__ | 19.16 | 29.34 | 29.94 | 24.25 | **34.43** | 
| SciCode | __28.47__ | 16.32 | 27.78 | 25.00 | 23.02 | **31.60** | 
| Instruction following |  |  |  |  |  |  | 
| IFEval | **96.66** | __95.39__ | 87.83 | 90.02 | 92.52 | 92.08 | 
| IFBench | **73.03** | 60.48 | __62.08__ | 44.56 | 38.16 | 48.84 | 
| IFStruct | **93.72** | 82.18 | 72.65 | 78.45 | 87.05 | __89.20__ | 
| Multi-IF | **89.10** | __84.80__ | 73.19 | 81.03 | 80.32 | 84.44 | 
| Knowledge |  |  |  |  |  |  | 
| GPQA Diamond | 63.37 | 57.78 | 65.15 | 57.88 | __75.76__ | **80.40** | 
| Humanity's Last Exam (no tools) | 7.17 | 5.88 | 8.20 | 4.77 | __12.84__ | **16.41** | 
| Humanity's Last Exam (with tools) | __13.90__ | 9.60 | 11.50 | 6.00 | 11.60 | **17.50** | 
| AA-Omniscience (public) | **0.50** | -33.17 | __-5.00__ | -15.33 | -57.17 | -28.00 | 
| AA-Omniscience accuracy (public) | 7.07 | 8.75 | 9.13 | 9.83 | **15.67** | __13.17__ | 
| AA-Omniscience non-hallucination (public) | **92.93** | 53.69 | __84.44__ | 72.09 | 13.64 | 52.59 | 
| Long context |  |  |  |  |  |  | 
| AA-LCR | __52.83__ | 6.00 | 49.34 | 39.33 | 52.67 | **62.00** | 
| RULER 128K | 65.56 | 1.15 | 37.96 | 18.82 | __84.63__ | **87.11** | 
| **Average** | **49.37** | 31.55 | 39.19 | 31.74 | 39.27 | __47.24__ | 

## Evaluation details

All models were served with vLLM in BF16 at their full context length: 262,144 tokens for LFM3-1.7B, Qwen3.5-4B, and Qwen3.5-9B, and 131,072 tokens for LFM2.5-2.6B, MiniCPM5-2B, and Gemma 4 E4B. Unless listed below, every eval uses temperature 0.6 and top_p 0.95, with no output limit other than the context length. The Average row is the unweighted mean of every score in the table.

**BFCL V4**: greedy decoding, up to 32,768 output tokens.

**ToolSandbox**: greedy decoding, up to 1,024 tokens per turn.

**LiveClaw**: OpenClaw agent, up to 8,192 tokens per turn.

**Claw-Eval**: greedy decoding, 3 trials per task.

**BrowseComp-Plus**: OpenClaw agent, greedy decoding, up to 8,192 tokens per turn.

**WebArena-Verified**: up to 40 steps.

**MCP-Atlas**: up to 256 turns.

**EnterpriseOps-Gym** and **AutomationBench**: up to 50 turns.

**SWE-bench**: OpenCode agent, up to 250 turns.

**Terminal-Bench 2.1**: OpenCode agent, up to 100 turns.

At 32K context, LFM3-1.7B prefills at least 2.5x faster and decodes at least 1.8x faster than every compared model on all three devices, while using the least memory.

## Evaluation details

All models run as Q4_0 GGUF files in llama.cpp with 32,768 tokens of context, batch size 1, 100 output tokens, and GPU offload. Prefill is the prompt processing speed and decode is the generation speed, both in tokens per second. LFM3-1.7B uses a llama.cpp build with LFM3 support, and the other models use llama.cpp release b11744.

**AMD Ryzen AI Max+ 395**: Radeon 8060S integrated GPU, Windows, Vulkan backend.

**Apple M5 Max**: macOS, Metal backend.

**iPhone 17 Pro**: Apple A19 Pro, iOS app built on llama.cpp.

**Memory**: peak usage in GB (10<sup>9</sup> bytes). On AMD, it adds the host and GPU peaks from the same run, which can slightly overstate usage if the two peaks do not coincide. On Apple devices, the process footprint already includes Metal allocations.

At 1,024 concurrent requests, LFM3-1.7B can produce 3.6 billion tokens/day on one NVIDIA B300.

**Pre-training:** two stages build LFM3-1.7B-Base on 22T tokens in total.

- **Main pre-training:** compared with LFM2, the mix adds more scientific papers, repository-level code, math, and a wider range of knowledge sources.
- **Mid-training:** the last 3.5T tokens use curated, higher-quality data with a larger share of agentic traces and tool calls. This stage also extends the context window to 256K tokens.

**Post-training:** four stages develop conversation, reasoning, and tool-use capabilities.

- **Supervised fine-tuning (SFT):** 1.75T tokens across various tasks, including chat, reasoning, tool calling, and agentic traces.
- **Mixed reinforcement learning:** many domains trained at once, from math and code to SQL and structured outputs. A length penalty in the reward keeps answers concise.
- **Multi-domain on-policy distillation (MOPD):** six specialist RL teachers (code, math, tool use, long context, agentic work, and enterprise workflows) are distilled back into a single model.
- **Agentic RL:** tasks from web browsing to office work are run inside real agent harnesses, preparing the model for deployment.

```
@article{liquidAI2026LFM3,
  author  = {Liquid AI},
  title   = {Introducing LFM3: Every Modality for Specialized Intelligence},
  journal = {Liquid AI Blog},
  year    = {2026},
  note    = {www.liquid.ai/blog/lfm3},
}
```
For questions and feedback, join Discord. For custom deployment and fine-tuning support, contact Liquid AI.

- Downloads last month
- 63
