---
id: collect-261121-veille-ia/nvidia-hardware/armin1-nemotron-3-5-super-vl-120b-a12b-nvfp4-abliterated-hugging-face
title: "armin1/Nemotron-3.5-Super-VL-120B-A12B-NVFP4-abliterated · Hugging Face"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: []
keywords: ["model", "original", "code", "tensorfold", "base", "vllm", "nemotron-3", "nvidia"]
source: https://huggingface.co/armin1/Nemotron-3.5-Super-VL-120B-A12B-NVFP4-abliterated
source_anchor: ""
source_lines: [1, 101]
section: "NVIDIA_Hardware"
---
An **abliterated** (uncensored) build of
nvidia/NVIDIA-Nemotron-3.5-Super-VL-120B-A12B-NVFP4.
Refusal behaviour has been removed while the original model's quality and NVIDIA's NVFP4 quantization are preserved
exactly as shipped. The checkpoint is a drop-in replacement for the base model and loads unmodified on **both vLLM and
TensorFold**. Developed and measured on a single NVIDIA DGX Spark (GB10).

The modification is a lightweight, quantization-preserving weight edit. The procedure is proprietary and not disclosed.


- **Refusals removed, at no measurable quality cost.** Keyword refusals on 204 harmful + jailbreak prompts fall from**98 % to 2 %** , while benign prompts are still answered (**99/100** ).
- **Task, math and code scores unchanged, token distribution at the noise floor.** A 130-item task suite, an
executed-code set and GSM8K-300 all match the original head-to-head; teacher-forced KL from the original is 0.0157,
against a run-to-run noise floor of 0.0138 (see Evaluation).
- **Full speed, drafter intact.** On TensorFold with the built-in MTP drafter: 56.2 tok/s on code, 38.8 on chat
(≥ the original), with a mean 4.74 draft tokens accepted per round on code. No decode cost; speculative decoding is
unaffected.
- **Vision and long context preserved.** The vision tower, embeddings and the 1M-token context window are unchanged.

Nemotron-3.5-Super-VL is a NemotronH hybrid vision-language model: a Mamba-2 / attention / latent mixture-of-experts language backbone (120 B total, ~12 B active parameters), a C-RADIO vision tower, and a multi-token-prediction (MTP) draft head, in NVIDIA ModelOpt mixed-precision NVFP4. See the base model card for the complete specification.

Measured head-to-head on one DGX Spark (GB10), greedy decoding. Quality, speed and refusal numbers are both the
original and this model served identically on **TensorFold 1.1.2**; the faithfulness (KL) numbers are both served on
vLLM. Every "original" figure is the unmodified base checkpoint on the same harness.

**Quality — unchanged (TensorFold 1.1.2):**

| Metric | Original | This model | 
|---|---|---|
| 130-task suite — gsm8k50 / json / instruct / tools / agent | 49 / 15 / 8 / 20 / 10 | **49 / 15 / 8 / 20 / 10** | 
| Executed code (25) | 25/25 | **25/25** | 
| GSM8K-300 (thinking off) | 293/300 | **292/300** | 

**Faithfulness to the original (vLLM, teacher-forced over 64,319 positions of code / prose / chat / long-context /
reasoning documents; KL over the top-20 tokens, so a lower bound):**

|  | KL vs original | top-1 agreement | ΔNLL | 
|---|---|---|---|
| Run-to-run noise floor (original vs itself) | 0.0138 | — | — | 
| **This model** | **0.0157** | **95.74 %** | **+0.0013** | 

The output distribution sits barely past the measurement's own noise floor — to within the harness's resolution, the same model.

**Speed — no cost (TensorFold 1.1.2, single stream, built-in MTP drafter):**

| Workload | Original | This model | MTP draft tokens / round (this model) | 
|---|---|---|---|
| Code decode tok/s | 51.1 | **56.2** | 4.74 | 
| Chat decode tok/s | 39.2 | **38.8** | — | 

MTP drafter acceptance is identical to the original (4.74 tokens/round on code), so speculative decoding is unaffected.

**Refusals (TensorFold 1.1.2):**

| Set | Original | This model | 
|---|---|---|
| Harmful + jailbreak (204) | 200/204 refused (98.0 %) | **4/204 refused (2.0 %)** | 
| Benign answered (100) | 69/100 | **99/100** | 

Refusals are judged by a keyword rule over the model's final answer — a calibration aid, **not a safety evaluation**
(see Limitations). The "benign answered" row checks that the modification does not make the model
refuse ordinary prompts; it answers more of them than the original, not fewer.

The checkpoint is a drop-in replacement for the base model. Recommended sampling is the base model's own.

**vLLM**

```
vllm serve <repo> \
  --trust-remote-code \
  --served-model-name nemotron-3.5-super-vl \
  --max-num-batched-tokens 8192 \
  --media-io-kwargs '{"video": {"fps": 2, "num_frames": 256}}' \
  --video-pruning-rate 0.5 \
  --reasoning-parser nemotron_v3 \
  --enable-auto-tool-choice --tool-call-parser qwen3_coder
```
NVIDIA's LiLiCorr speculative-decoding drafter can be attached unchanged.

**TensorFold** (native CUDA engine, with the built-in MTP drafter)

```
tensorfold-native serve <path-to-model> \
  --name nemotron-3.5-super-vl --vision --context 1048576
```
Both expose an OpenAI-compatible API. `--vision` enables image and video input.

- **Uncensored.** Refusal behaviour is reduced, not removed, and the model can produce harmful, offensive or false
content. You are responsible for how you use it and for complying with all applicable laws and the base model's
license.
- The refusal numbers come from a keyword scorer on short, thinking-off answers; a keyword count can miscount an answer that complies but adds a caveat. They were not re-checked with a judge model, so treat the model as substantially, not perfectly, uncensored.
- Vision input is preserved structurally but was not separately benchmarked for this build; generative long-context retrieval was likewise not re-run.
- Benchmarks were produced with a private harness; compare its numbers with each other, not with scores published elsewhere.

Use of this model is governed by the base model's license, the OpenMDW License Agreement, version 1.1. This is a derivative of NVIDIA's Nemotron-3.5-Super-VL and inherits its terms.

- NVIDIA for Nemotron-3.5-Super-VL and the ModelOpt NVFP4 checkpoint.

*Not affiliated with or endorsed by NVIDIA.*

- Downloads last month
- 1,972
