---
id: collect-261121-veille-ia/muse/redhatai-muse-spark-open-816b-a42b-mxfp4xfp8-hugging-face
title: "RedHatAI/Muse-Spark-Open-816B-A42B-MXFP4xFP8 · Hugging Face"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: ["2026-11-10"]
keywords: ["model", "weights", "vllm", "mxfp4", "quantization", "activations", "attention", "dense"]
source: https://huggingface.co/RedHatAI/Muse-Spark-Open-816B-A42B-MXFP4xFP8
source_anchor: ""
source_lines: [1, 100]
section: "Muse"
---
- **Model Architecture:** MuseSparkForConditionalGeneration
  - **Input:** Text / Image / Video / Audio
  - **Output:** Text
- **Model Optimizations:**
  - **Weight quantization:** Mixed (MXFP4/FP8)
  - **Activation quantization:** FP8
- **Release Date:** 2026-11-10
- **Version:** 1.0
- **Model Developers:** RedHatAI

This model is a quantized version of meta-models/Muse-Spark-Open-816B-A42B. It was evaluated on several reasoning and agentic coding tasks to assess its quality. The unquantized model was not re-evaluated; the scores reported by Meta are shown for reference.

This is a mixed-precision quantization where the routed experts use MXFP4 weights and the attention and dense projection layers use FP8 weights, with FP8 activations throughout.

This model was obtained by quantizing the weights and activations of meta-models/Muse-Spark-Open-816B-A42B to mixed MXFP4 and FP8 precision, ready for inference with vLLM.

Typical MXFP4 quantization pairs MXFP4 weights with MXFP4 activations, a combination that is most performant on Blackwell GPUs. In this model, the routed experts, which hold the vast majority of the parameters, instead use MXFP4 weights with FP8 activations quantized dynamically on a per-group basis, making it well suited for inference on Hopper GPUs. The attention projections and the dense projections around the expert block are kept at FP8 precision with block-wise weight scaling and FP8 dynamic activations.

This optimization reduces the number of bits per parameter from 16 to 4 for the expert weights and to 8 for the attention and dense projection weights, reducing the disk size and GPU memory requirements by approximately 74% (from ~1.64 TB to ~428 GB). The vision and audio encoders, embeddings, expert router, and `lm_head` are kept in their original precision.

Only the weights and activations of the linear operators within transformer blocks are quantized using LLM Compressor.

Support for this checkpoint on NVIDIA GPUs is being added to vLLM in vllm-project/vllm#64389. Until that pull request is merged and released, install vLLM from the pull request branch:

```
git clone https://github.com/vllm-project/vllm.git
cd vllm
git fetch origin pull/64389/head:muse-spark-support
git checkout muse-spark-support
VLLM_USE_PRECOMPILED=1 uv pip install --editable .
```
Then serve the model:

```
vllm serve RedHatAI/Muse-Spark-Open-816B-A42B-MXFP4xFP8 \
  --tensor-parallel-size 8 \
  --enable-expert-parallel \
  --reasoning-parser muse \
  --enable-auto-tool-choice \
  --tool-call-parser muse
```
Additional model-specific options (1M-token context, speculative decoding, video sampling, etc.) are documented in the vLLM recipe for Muse Spark Open.

This model was created by applying LLM Compressor with a calibration-free mixed-precision recipe: MXFP4 weights with per-group FP8 dynamic activations for the routed experts, and FP8 block-wise quantization for the attention and dense projection layers. The quantized model was exported in the compressed-tensors format, as presented in the code snippet below.

```
from compressed_tensors.quantization import QuantizationConfig, QuantizationScheme
from compressed_tensors.quantization.quant_scheme import FP8_BLOCK, MXFP4xFP8
from llmcompressor import model_free_ptq
MODEL_ID = "meta-models/Muse-Spark-Open-816B-A42B"
SAVE_DIR = MODEL_ID.rstrip("/").split("/")[-1] + "-MXFP4xFP8"
# Apply MXFP4xFP8 to the routed experts, which hold most of the weights.
# Use FP8_BLOCK for attention (including the gate_proj output gate) and the
# dense projections around the expert block.
# Keep the embeddings, lm_head, the fp32 router and the multimodal towers unquantized.
model_free_ptq(
    model_stub=MODEL_ID,
    save_directory=SAVE_DIR,
    config=QuantizationConfig(
        config_groups={
            "dense": QuantizationScheme(
                **FP8_BLOCK,
                targets=[
                    # gate_proj here is the attention output gate
                    r"re:.*self_attn\.(q|k|v|o|gate)_proj$",
                    r"re:.*mlp\.(pre|post)_expert_proj$",
                ],
            ),
            "experts": QuantizationScheme(
                **MXFP4xFP8,
                # Matches the per-expert names produced after the split
                targets=[r"re:.*mlp\.experts\.\d+\.(gate|up|down)_proj$"],
            ),
        },
        ignore=[
            "lm_head",
            r"re:.*embed_tokens$",
            r"re:.*mlp\.gate$",
            r"re:model\.(audio_tower|vision_tower|vision_adapter|vision_projection).*",
        ],
    ),
    device="cpu",
)
```
This model was evaluated on GPQA Diamond, AIME 2026, Terminal-Bench 2.1, SWE-bench Verified, and DeepSWE 1.1 using Inspect AI (inspect_evals), served with vLLM (OpenAI-compatible API).

| Category | Benchmark | meta-models/Muse-Spark-Open-816B-A42B (reported by Meta*) | RedHatAI/Muse-Spark-Open-816B-A42B-MXFP4xFP8 | 
|---|---|---|---|
| **Reasoning** | GPQA Diamond (0-shot, pass@1) | 89.0% | 90.4% | 
|  | AIME 2026 (0-shot, pass@1) | – | 96.7% | 
| **Agentic Coding** | Terminal-Bench 2.1 (pass@1) | 76.4% | 79.8% | 
|  | SWE-bench Verified (pass@1) | – | 78.6% | 
|  | DeepSWE 1.1 (pass@1) | 60% | 57.5% | 

* The unquantized model was not evaluated as part of this work. The values in this column are the scores reported by Meta in the original model card (xhigh reasoning effort; Terminus 2 agent for Terminal-Bench 2.1 and Meta's preferred harness for DeepSWE 1.1) and were not reproduced. They were obtained with a different evaluation setup than the quantized model's scores, so the two columns are not directly comparable and no recovery is reported. Meta does not report AIME 2026 or SWE-bench Verified scores.

All tasks were evaluated 0-shot. Each task was run for a single epoch, and the reported score is pass@1 over that run. The model was served with vLLM built from vllm-project/vllm#64389. All evaluations were run with the setup in neuralmagic/evals-refresh, where the configuration and reproduction steps for each benchmark are available.

- Downloads last month
- 227
