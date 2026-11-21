---
id: collect-261121-veille-ia/modeles-open/microsoft-aion-1-0-instruct-2-3b-onnx-webgpu-hugging-face
title: "microsoft/Aion-1.0-Instruct-2.3B-ONNX-WebGPU · Hugging Face"
domain: modeles-open
role: reference
task: article
actors: []
dates: []
keywords: ["model", "onnx", "config", "microsoft", "runtime", "tokenizer", "webgpu", "length"]
source: https://huggingface.co/microsoft/Aion-1.0-Instruct-2.3B-ONNX-WebGPU
source_anchor: ""
source_lines: [1, 89]
section: "Modeles_Open"
---
This repository contains the **ONNX WebGPU** quantized version of **Aion Instruct v1**.
For the full model description, intended uses, limitations, safety information and evaluation results, see the original model card: microsoft/Aion-1.0-Instruct-2.3B.


| **Base model** | microsoft/Aion-1.0-Instruct-2.3B | 
| **Format** | ONNX (opset 21, `com.microsoft` contrib ops) | 
| **Target hardware** | GPU via WebGPU (browser and native) | 
| **Runtime** | ONNX Runtime GenAI / ONNX Runtime (Web) with the WebGPU execution provider | 
| **Quantization** | INT4 weights (block-wise, QAT), FP16 activations and KV cache | 
| **Model size** | ~1.3 GB | 
| **Parameters** | ~2.29B (base model) | 
| **Context length** | 32,768 tokens | 
| **Languages** | English, German, Spanish, Portuguese, Italian, French, Chinese, Japanese | 
| **License** | MIT | 

| Component | Scheme | 
|---|---|
| Linear layers (attention q/k/v/o, MLP gate/up/down) | 4-bit symmetric block-wise weights ( `MatMulNBits` , block size 32), exported from a quantization-aware-trained (QAT) checkpoint | 
| Token embedding / tied LM head | 4-bit block-wise weights ( `GatherBlockQuantized` /`MatMulNBits` , block size 64) | 
| Activations, norms, logits | FP16 | 
| KV cache | FP16 | 
| Attention | `GroupQueryAttention` with fused RoPE; 21 sliding-window layers (window 2048) + 7 full-attention layers | 

The model is a single ONNX decoder graph (opset 21 + `com.microsoft` contrib ops) with an FP16 KV cache, so it runs on any ONNX Runtime build that provides the WebGPU execution provider, including ONNX Runtime Web in the browser.

Run with ONNX Runtime GenAI using the WebGPU execution provider. `onnxruntime-genai` is installed with `--no-deps` so that it uses the WebGPU build of ONNX Runtime instead of the default CPU build:

```
hf download microsoft/Aion-1.0-Instruct-2.3B-ONNX-WebGPU --local-dir Aion-Instruct-v1-ONNX-WebGPU
pip install numpy onnxruntime-webgpu==1.27.0
pip install --no-deps onnxruntime-genai==0.14.1
```
```
import onnxruntime_genai as og
model_dir = "Aion-Instruct-v1-ONNX-WebGPU"
config = og.Config(model_dir)
config.clear_providers()
config.append_provider("webgpu")
model = og.Model(config)
tokenizer = og.Tokenizer(model)
chat_template = open(f"{model_dir}/chat_template.jinja", encoding="utf-8").read()
messages = '[{"role": "user", "content": "Give me a short introduction to large language models."}]'
prompt = tokenizer.apply_chat_template(messages, template_str=chat_template, add_generation_prompt=True)
params = og.GeneratorParams(model)
params.set_search_options(max_length=4096)
generator = og.Generator(model, params)
generator.append_tokens(tokenizer.encode(prompt))
stream = tokenizer.create_stream()
while not generator.is_done():
    generator.generate_next_token()
    print(stream.decode(generator.get_next_tokens()[0]), end="", flush=True)
```
`max_length` (prompt + generated tokens) defaults to 4,096 in `genai_config.json`; it can be raised up to the model's 32,768-token context, at the cost of a larger pre-allocated KV cache. To run on CPU instead, omit the `clear_providers` / `append_provider` calls.

| File | Description | 
|---|---|
| `model.onnx` | ONNX decoder graph | 
| `model.onnx.data` | Quantized weights (external data, ~1.3 GB) | 
| `genai_config.json` | ONNX Runtime GenAI configuration | 
| `config.json` ,`generation_config.json` | Model and generation configuration | 
| `tokenizer.json` ,`tokenizer_config.json` | Tokenizer | 
| `chat_template.jinja` | Chat template | 
| `LICENSE` | MIT License | 

| Name | Type | Shape | 
|---|---|---|
| `input_ids` | int64 | `[batch, sequence_length]` | 
| `attention_mask` | int64 | `[batch, total_sequence_length]` | 
| `past_key_values.{0..27}.key` /`.value` | float16 | `[batch, 8, past_sequence_length, 128]` | 
| `logits` (output) | float16 | `[batch, 1, 200029]` (last position only) | 
| `present.{0..27}.key` /`.value` (outputs) | float16 | `[batch, 8, total_sequence_length, 128]` | 

This quantized model uses the same chat template as the base model. See the Chat Format section of the original model card.

Quantization can affect output quality. Quality relative to the bfloat16 base model will be reported here. For the reference results of the unquantized model, see the Evaluation section of the original model card.

This model inherits all limitations of the base model, and quantization may introduce additional quality degradation. Please read the Bias, Risks, and Limitations section and the Out-of-Scope Use section of the original model card before using this model.

| Version | Link | 
|---|---|
| Original (bfloat16, Transformers) | microsoft/Aion-1.0-Instruct-2.3B | 
| GGUF (llama.cpp) | asgmodels/Aion-Instruct-v1-GGUF | 
| ONNX (WebGPU) | microsoft/Aion-1.0-Instruct-2.3B-ONNX-WebGPU | 
| ONNX (NPU) | asgmodels/Aion-Instruct-v1-ONNX-NPU | 

This model is released under the MIT License, the same as the base model.

- Downloads last month
- 127
