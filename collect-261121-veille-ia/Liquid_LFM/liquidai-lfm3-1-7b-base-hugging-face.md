---
id: collect-261121-veille-ia/liquid-lfm/liquidai-lfm3-1-7b-base-hugging-face
title: "LiquidAI/LFM3-1.7B-Base · Hugging Face"
domain: liquid-lfm
role: reference
task: article
actors: ["Liquid AI"]
dates: []
keywords: ["lfm3-1", "model", "b-base", "tokens", "liquidai", "checkpoint", "data", "pre-training"]
source: https://huggingface.co/LiquidAI/LFM3-1.7B-Base?library=transformers
source_anchor: ""
source_lines: [1, 74]
section: "Liquid_LFM"
---
LFM3-1.7B-Base is the **pre-trained checkpoint** behind LFM3-1.7B, a text model for local agents and specialized applications based on our new tri-hybrid architecture.

- **Pre-trained on 22T tokens:** the last 3.5T tokens are curated mid-training data with agentic traces and tool calls, and extend the context window to 256K tokens.
- **Designed for the edge:** at 4-bit precision, it takes about 1 GB of memory, so it fits on any phone or laptop.
- **Made to be specialized:** a starting point for continued pre-training and your own post-training.

Find more information about the LFM3 family in our release blog post.

The LFM3 family includes base, text, vision, audio, omni, and decision models.

| Property | Specification | 
|---|---|
| **Checkpoint** | Pre-trained, text-only base model | 
| **Parameters** | 1.73B | 
| **Architecture** | 24 layers: 13 gated short convolutions, 5 sliding-window attention, 6 global attention | 
| **Context length** | 256K tokens (input + output) | 
| **Vocabulary** | 128,000 tokens | 
| **Languages** | English, Arabic, Chinese, French, German, Italian, Japanese, Korean, Portuguese, Spanish, Vietnamese, Thai, Indonesian, Hindi, Russian, Polish | 
| **License** | LFM Open License v1.0 | 

LFM3-1.7B-Base is the checkpoint used to create LFM3-1.7B. It is only recommended for tasks that require heavy fine-tuning, like language-specific or domain-specific assistants, training on proprietary data, or experimenting with novel post-training approaches. For chat, tool calling, and agents, use LFM3-1.7B.

LFM3-1.7B-Base uses the same architecture and modeling code as LFM3-1.7B, with BF16 weights.

**Transformers quick start:**

```
from transformers import AutoModelForCausalLM, AutoTokenizer
model_id = "LiquidAI/LFM3-1.7B-Base"
model = AutoModelForCausalLM.from_pretrained(
    model_id,
    device_map="auto",
    dtype="bfloat16",
    trust_remote_code=True,
)
tokenizer = AutoTokenizer.from_pretrained(model_id, trust_remote_code=True)
prompt = "The nematode C. elegans is"
input_ids = tokenizer(prompt, return_tensors="pt").input_ids.to(model.device)
output = model.generate(input_ids, max_new_tokens=128, do_sample=False)
print(tokenizer.decode(output[0], skip_special_tokens=True))
```
Mid-training included chat data with and without thinking traces, formatted with the same chat template as LFM3-1.7B. With `apply_chat_template`, reasoning is enabled by default, so responses start with a thinking trace. Set `enable_thinking=False` to prefill an empty thinking trace and have the model answer directly.

LFM3-1.7B-Base is the starting point for substantial customization, such as adapting to new domains, languages, or behaviors that diverge significantly from those of LFM3-1.7B, through continued pre-training or full post-training via TRL.

These notebooks start from LFM3-1.7B. To start from the base model, set `model_id = "LiquidAI/LFM3-1.7B-Base"`.

| Training | Use case | Notebook | 
|---|---|---|
| **SFT** | Extract structured JSON from messy documents |  | 
| **CPT** | Learn a new language (Turkish) from raw text |  | 

**Pre-training:** two stages build LFM3-1.7B-Base on 22T tokens in total.

- **Main pre-training:** compared with LFM2, the mix adds more scientific papers, repository-level code, math, and a wider range of knowledge sources.
- **Mid-training:** the last 3.5T tokens use curated, higher-quality data with a larger share of agentic traces and tool calls. This stage also extends the context window to 256K tokens.

Post-training from this checkpoint (SFT, mixed RL, multi-domain on-policy distillation, and agentic RL) produces LFM3-1.7B.

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
- 15,137

`# Use a pipeline as a high-level helper from transformers import pipeline pipe = pipeline("text-generation", model="LiquidAI/LFM3-1.7B-Base", trust_remote_code=True) messages = [ {"role": "user", "content": "Who are you?"}, ] pipe(messages)`
