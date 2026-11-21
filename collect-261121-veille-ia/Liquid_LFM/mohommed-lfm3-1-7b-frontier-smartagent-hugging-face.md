---
id: collect-261121-veille-ia/liquid-lfm/mohommed-lfm3-1-7b-frontier-smartagent-hugging-face
title: "mohommed/LFM3-1.7B-Frontier-SmartAgent · Hugging Face"
domain: liquid-lfm
role: reference
task: article
actors: ["Liquid AI"]
dates: []
keywords: ["context", "tool", "layers", "lfm3-1", "b-frontier-smartagent", "attention", "brain", "stage"]
source: https://huggingface.co/mohommed/LFM3-1.7B-Frontier-SmartAgent?local-app=llama.cpp
source_anchor: ""
source_lines: [1, 157]
section: "Liquid_LFM"
---
**`LFM3-1.7B-Frontier-SmartAgent`** is an autonomous reasoning, coding, and tool-calling language model built on top of **`LiquidAI/LFM3-1.7B`**. It combines Liquid AI's **24-layer Tri-Hybrid architecture** (Gated Short Convolutions + Sliding-Window Attention + Global Grouped-Query Attention) with **512K NTK-YaRN context scaling**, **Gram-Schmidt Damped Mid-Layer Orthogonalization**, and a **Two-Stage Post-Training Pipeline (Multi-Turn SFT + Multi-Objective GRPO)**.

Designed specifically for **on-device and edge agentic systems**, this model natively reasons through a structured **4-Stage Frontier Thinking Protocol (`<think>...</think>`)**, audits external context (`<context>`) and persistent associative memory (`<second_brain>`), and emits verified executable Python tool calls (`<tool_call>...</tool_call>`).

Extended 2.0× from LFM3's native 262,144 context window to **524,288 tokens** across both full and sliding-window attention layers using calibrated YaRN RoPE scaling (`factor = 2.0`, `beta_fast = 32.0`, `beta_slow = 1.0`, `mscale = 1.14`). This preserves high-frequency local positional resolution while enabling repository-scale AST dependency indexing.

Rather than applying destructive full-model weight abliteration—which degrades small-model perplexity and state-space convolution stability—refusal directions are isolated via Singular Value Decomposition (SVD), orthogonalized against harmless technical reasoning activations via **Gram-Schmidt projection**, and subtracted **exclusively from middle MLP down-projections (`w2` / `down_proj` in layers 9–19)** with a damping coefficient of $\alpha = 0.60$. Early `shortconv` feature extractors (layers 0–8) and final output calibration layers (layers 20–23) remain 100% untouched.

Unlike standard chat models whose chain-of-thought is unstructured or stripped during multi-turn conversations, `LFM3-1.7B-Frontier-SmartAgent` is trained with `preserve_thinking = true` to execute a strict 4-step cognitive audit before every action:

1. **`[1. CONTEXT & 2ND BRAIN AUDIT]`** : Inspects`<context>` ,`<second_brain>` , and`<tool_response>` to separate verified facts from unknowns.
2. **`[2. DAG DECOMPOSITION]`** : Breaks complex goals into an ordered Directed Acyclic Graph (DAG) of atomic tool invocations.
3. **`[3. HALLUCINATION & CONSTRAINT CHECK]`** : Verifies every URL, file path, Set-of-Marks ID`[#som_id]` , and function parameter signature against the active context.
4. **`[4. PRECISION EXECUTION]`** : Emits syntactically valid Python tool calls or a grounded final Markdown synthesis.

Trained to operate seamlessly with a persistent bi-temporal **Second Brain** combining:

- **Dense Vector Memory (`FAISS IndexFlatIP`)** with**Maximal Marginal Relevance (MMR)** deduplication.
- **Neuro-Symbolic Knowledge Graph (`NetworkX` Personalized PageRank / PPR)** for multi-hop entity association.
- **Playwright Set-of-Marks (`[#som_id]`) Browser Grounding** and**Voyager-style Dynamic Skill Synthesis** .

| Property | Specification | 
|---|---|
| **Base Architecture** | `Lfm3ForCausalLM` (Tri-Hybrid: Gated`shortconv` + Sliding-Window GQA + Full GQA) | 
| **Parameters** | ~1.7 Billion ( `24` Layers,`2048` Hidden Dim,`7680` SwiGLU FFN Dim) | 
| **Attention Heads** | `16` Query Heads /`4` Key-Value Heads (GQA) on Attention Layers | 
| **ShortConv Cache** | Kernel Size `L_cache = 7` | 
| **Vocabulary Size** | `125,050` (Native Byte-Level BPE via`PreTrainedTokenizerFast` ) | 
| **Max Context Length** | **524,288 tokens (512K)** via NTK-YaRN (`factor=2.0` ,`mscale=1.14` ) | 
| **Stage 1 Post-Training** | Multi-Turn Supervised Fine-Tuning ( **SFT** ) on 4-Stage`<think>` +`<tool_call>` trajectories (`LoRA r=32, alpha=64, lr=1.5e-4` , final loss`0.089` ) | 
| **Stage 2 Post-Training** | Group Relative Policy Optimization ( **GRPO** ) with Dense AST, Signature-Binding & Context-Grounding Rewards (`num_generations=4, lr=5e-6` ) | 

```
<think>
[1. CONTEXT & 2ND BRAIN AUDIT]: <context> indicates the workspace is clean and report.txt is not yet written. <second_brain> lists the target URL https://example.com/.
[2. DAG DECOMPOSITION]:
  - Step 1: Call `browser_goto(url='https://example.com')` to extract the live Set-of-Marks DOM.
  - Step 2: Call `computer_write_file(filename='report.txt', content=dom_snap)` to persist the DOM.
  - Step 3: Call `computer_bash(command='head -n 8 report.txt')` to verify the file on disk.
[3. HALLUCINATION & CONSTRAINT CHECK]: All URLs and filenames are explicit and match tool parameter schemas.
[4. PRECISION EXECUTION]: Emit executable Python inside <tool_call>.
</think>
<tool_call>
dom_snap = browser_goto(url="https://example.com")
write_res = computer_write_file(filename="report.txt", content=dom_snap)
verify_res = computer_bash(command="head -n 8 report.txt")
print(write_res)
print(verify_res)
</tool_call>
```
```
<think>
[1. CONTEXT & 2ND BRAIN AUDIT]: <tool_response> confirms report.txt (1068 bytes) was written and verified with EXIT 0.
[2. DAG DECOMPOSITION]: All 3 DAG nodes succeeded; no further tool calls required.
[3. HALLUCINATION & CONSTRAINT CHECK]: Cite exact byte count (1068 bytes), page title (Example Domain), and Set-of-Marks [#1].
[4. PRECISION EXECUTION]: Output final verified Markdown summary.
</think>
### Execution Verified
1. **Browser Navigation:** Loaded `https://example.com/` (`Example Domain`).
2. **Second Brain Upgrade:** Saved interactive Set-of-Marks `[#1]` (`<a> "Learn more"`).
3. **File Write:** Persisted `report.txt` — `1068 bytes`.
4. **Disk Verification:** Confirmed `report.txt` exists (`EXIT 0`).
```
All GGUF files are hosted in the companion repository: **`mohommed/LFM3-1.7B-Frontier-SmartAgent-GGUF`**.

💡 **Quantization Recommendation:** Because `LFM3-1.7B` is a compact 1.7B Tri-Hybrid model utilizing stateful gated short-convolution (`shortconv`) layers, **`Q8_0` (1.84 GiB)** or **`Q6_K` (1.42 GiB)** is strongly recommended to retain **99.8% of full-precision coding, reasoning, and tool-calling accuracy**.


| Filename | Quantization | Size | Best Use Case | 
|---|---|---|---|
| `lfm3-1.7b-frontier-f16.gguf` | `F16` | ~3.45 GiB | Uncompressed 16-bit reference weights | 
| `lfm3-1.7b-frontier-q8_0.gguf` | **`Q8_0`** | **~1.84 GiB** | **⭐ Recommended for SOTA Agentic & Coding Quality** | 
| `lfm3-1.7b-frontier-q6_k.gguf` | `Q6_K` | ~1.42 GiB | Near-lossless quality with low memory footprint | 
| `lfm3-1.7b-frontier-q5_k_m.gguf` | `Q5_K_M` | ~1.22 GiB | Balanced quality and inference speed | 
| `lfm3-1.7b-frontier-q4_k_m.gguf` | `Q4_K_M` | ~1.02 GiB | Fast CPU / laptop deployment | 
| `lfm3-1.7b-frontier-q3_k_m.gguf` | `Q3_K_M` | ~0.82 GiB | Ultra-compact mobile & embedded devices | 

```
import torch
from transformers import AutoModelForCausalLM, AutoTokenizer
model_id = "mohommed/LFM3-1.7B-Frontier-SmartAgent"
tokenizer = AutoTokenizer.from_pretrained(model_id, trust_remote_code=True)
model = AutoModelForCausalLM.from_pretrained(
    model_id,
    trust_remote_code=True,
    torch_dtype=torch.bfloat16 if torch.cuda.is_bf16_supported() else torch.float16,
    device_map="auto",
).eval()
system_prompt = """You are LFM3-1.7B-Frontier, an elite autonomous AI agent equipped with a 512K context window and a Real-Time Second Brain Memory system.
### MANDATORY 4-STAGE THINKING PROTOCOL
Inside `<think>...</think>`, structure your reasoning using:
[1. CONTEXT & 2ND BRAIN AUDIT]
[2. DAG DECOMPOSITION]
[3. HALLUCINATION & CONSTRAINT CHECK]
[4. PRECISION EXECUTION]"""
messages = [
    {"role": "system", "content": system_prompt},
    {
        "role": "user",
        "content": (
            "<context>Workspace active.</context>\n"
            "<second_brain>No cached memories.</second_brain>\n"
            "Goal: Write a thread-safe LRU cache in Python with O(1) get and put operations."
        ),
    },
]
prompt = tokenizer.apply_chat_template(messages, tokenize=False, add_generation_prompt=True)
inputs = tokenizer(prompt, return_tensors="pt").to(model.device)
with torch.no_grad():
    outputs = model.generate(
        **inputs,
        max_new_tokens=512,
        temperature=0.15,
        top_p=0.90,
        repetition_penalty=1.08,
        do_sample=True,
        pad_token_id=tokenizer.eos_token_id,
    )
reply = "<think>\n" + tokenizer.decode(outputs[0][inputs.input_ids.shape[1]:], skip_special_tokens=False).split("<|im_end|>")[0].strip()
print(reply)
```
```
# Run the recommended 8-bit quantization directly from Hugging Face:
ollama run hf.co/mohommed/LFM3-1.7B-Frontier-SmartAgent-GGUF:Q8_0
# Or run the 4-bit quantization:
ollama run hf.co/mohommed/LFM3-1.7B-Frontier-SmartAgent-GGUF:Q4_K_M
```
```
./llama-cli \
  -hf mohommed/LFM3-1.7B-Frontier-SmartAgent-GGUF:Q8_0 \
  -c 32768 \
  --temp 0.15 \
  --repeat-penalty 1.08 \
  -cnv
```
- **Temperature:**`0.10 – 0.20` (Low temperature keeps AST code generation and tool parameter binding deterministic).
- **Top-P:**`0.90 – 0.92`
- **Repetition Penalty:**`1.08`
- **Context Packing:** While the architecture supports up to**512K tokens** , for single-GPU consumer hardware (8 GB–16 GB VRAM), pairing the model with**FAISS + PageRank MMR retrieval** (injecting the top 8K–32K highest-signal tokens per turn) maximizes prefill speed and attention sharpness.

This model is released under the **Apache-2.0 License**, inheriting from `LiquidAI/LFM3-1.7B`.

```
@misc{lfm3_frontier_smartagent_2026,
  title        = {LFM3-1.7B-Frontier-SmartAgent: 512K-Context Hybrid Reasoning and Second-Brain Autonomous Agent},
  author       = {mohommed},
  year         = {2026},
  publisher    = {Hugging Face},
  howpublished = {\url{https://huggingface.co/mohommed/LFM3-1.7B-Frontier-SmartAgent}}
}
```
- Downloads last month
- 4,524

## Install (macOS, Linux)

`# Start a local OpenAI-compatible server with a web UI: llama serve -hf mohommed/LFM3-1.7B-Frontier-SmartAgent:``# Run inference directly in the terminal: llama cli -hf mohommed/LFM3-1.7B-Frontier-SmartAgent:`
