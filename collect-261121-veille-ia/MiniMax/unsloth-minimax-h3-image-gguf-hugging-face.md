---
id: collect-261121-veille-ia/minimax/unsloth-minimax-h3-image-gguf-hugging-face
title: "unsloth/MiniMax-H3-Image-GGUF · Hugging Face"
domain: minimax
role: reference
task: article
actors: ["MiniMax"]
dates: []
keywords: ["gguf", "image", "minimax", "unsloth", "lpips", "minimax-h3-image", "bf16", "diffusion"]
source: https://huggingface.co/unsloth/MiniMax-H3-Image-GGUF
source_anchor: ""
source_lines: [1, 298]
section: "MiniMax"
---
# Run MiniMax-H3-Image FP8 or GGUF with Unsloth Desktop

These are GGUF quantized versions of the MiniMax-H3-Image diffusion transformer (DiT), for text-to-image and image editing on GPUs that cannot hold the 66 GB bf16 DiT, on Apple Silicon and on the CPU. 

- **Recommended:**`UD-Q4_K_XL` (13.61 GB): same-seed LPIPS 0.172 vs the bf16 DiT at 1024x1024 (lower is better), 14.8 GiB peak VRAM on an A100. For output closest to bf16 use`Q8_0` (LPIPS 0.083); on tight memory use`UD-Q2_K_XL` (8.68 GB, LPIPS 0.347), which is clearly ahead of the smallest file,`Q2_K` (7.75 GB, LPIPS 0.438).
- Runs on a 24 GB L4 at about 1.2 s per step at 1024x1024 with `sd-cli` (1.7 s through Unsloth Studio), and with`--max-vram 12` it renders the same image byte for byte at 3.9 s per step. On the CPU alone, 512x512 takes about 47 s per image.
- Loads in upstream stable-diffusion.cpp (MiniMax-H3-Image support landed in #2324) and in unslothai/stable-diffusion.cpp. `UD-` files are Unsloth Dynamic quants.

| file | size | LPIPS 1024 vs bf16 (95% interval) | PSNR 1024 | LPIPS 2048 | peak VRAM, 1024x1024 | 
|---|---|---|---|---|---|
| `minimax_h3_image_pruned-Q2_K.gguf` | 7.75 GB | 0.438 (0.375 to 0.481) | 13.9 dB | 0.435 | 9.4 GiB | 
| `minimax_h3_image_pruned-UD-Q2_K_XL.gguf` | 8.68 GB | 0.347 (0.261 to 0.414) | 16.1 dB | 0.332 | 10.2 GiB | 
| `minimax_h3_image_pruned-Q3_K_S.gguf` | 8.77 GB | 0.347 (0.262 to 0.414) | 16.1 dB | 0.385 | 10.3 GiB | 
| `minimax_h3_image_pruned-Q3_K_M.gguf` | 10.42 GB | 0.271 (0.199 to 0.339) | 17.5 dB | 0.288 | 11.9 GiB | 
| `minimax_h3_image_pruned-UD-Q3_K_XL.gguf` | 11.36 GB | 0.242 (0.164 to 0.324) | 18.1 dB | 0.233 | 12.7 GiB | 
| `minimax_h3_image_pruned-Q4_K_S.gguf` | 11.54 GB | 0.225 (0.150 to 0.308) | 18.5 dB | 0.211 | 12.9 GiB | 
| `minimax_h3_image_pruned-Q4_K_M.gguf` | 12.72 GB | 0.195 (0.136 to 0.246) | 19.2 dB | 0.196 | 14.0 GiB | 
| `minimax_h3_image_pruned-UD-Q4_K_XL.gguf` (recommended) | 13.61 GB | 0.172 (0.110 to 0.235) | 20.4 dB | 0.171 | 14.8 GiB | 
| `minimax_h3_image_pruned-Q5_K_S.gguf` | 13.93 GB | 0.173 (0.107 to 0.242) | 21.0 dB | 0.197 | 15.2 GiB | 
| `minimax_h3_image_pruned-Q5_K_M.gguf` | 14.60 GB | 0.157 (0.098 to 0.210) | 21.4 dB | 0.133 | 15.8 GiB | 
| `minimax_h3_image_pruned-UD-Q5_K_XL.gguf` | 15.47 GB | 0.154 (0.092 to 0.211) | 22.2 dB | 0.113 | 16.8 GiB | 
| `minimax_h3_image_pruned-Q6_K.gguf` | 16.60 GB | 0.134 (0.076 to 0.194) | 22.8 dB | 0.158 | 17.6 GiB | 
| `minimax_h3_image_pruned-UD-Q6_K_XL.gguf` | 17.58 GB | 0.130 (0.072 to 0.185) | 23.2 dB | 0.128 | 18.8 GiB | 
| `minimax_h3_image_pruned-Q8_0.gguf` | 22.03 GB | 0.083 (0.039 to 0.129) | 26.8 dB | 0.048 | 23.0 GiB | 

Each file replaces the bf16 `transformer` and its per-step heads. The VAE is the release image VAE, `vae/diffusion_pytorch_model.safetensors`, and the text encoder is the MiniMax-H3 Qwen3-VL-32B encoder: a GGUF of it is in unsloth/MiniMax-H3-GGUF (`qwen3vl_32b_minimax_h3-Q4_K_M.gguf`, 18.2 GB). GGUF loading runs no arbitrary code. These files are Model Derivatives of MiniMax-H3-Image, distributed under its license: the DiT is quantized to GGUF and its time modulation stored in curve form.

Text-to-image:

```
sd-cli --diffusion-model minimax_h3_image_pruned-UD-Q4_K_XL.gguf \
  --vae diffusion_pytorch_model.safetensors \
  --llm qwen3vl_32b_minimax_h3-Q4_K_M.gguf \
  -p "A weathered lighthouse keeper in a yellow oilcloth coat stands on a wet stone pier at dusk ..." \
  -W 1024 -H 1024 --cfg-scale 1.0 --steps 6 --sampling-method euler --sigmas minimax-h3-image --rng cpu
```
Image editing: add one or more reference images with `-r` (or `--ref-image`); in argument order they are `<Picture 1>`, `<Picture 2>`, and so on in the prompt:

```
sd-cli --diffusion-model minimax_h3_image_pruned-UD-Q4_K_XL.gguf \
  --vae diffusion_pytorch_model.safetensors \
  --llm qwen3vl_32b_minimax_h3-Q4_K_M.gguf \
  -r reference1.png -p "Edit <Picture 1>. ..." \
  -W 1024 -H 1024 --cfg-scale 1.0 --steps 6 --sampling-method euler --sigmas minimax-h3-image --rng cpu
```
For editing, set `-W` / `-H` to the reference image's aspect ratio; a square size crops a wide reference. MiniMax-H3-Image is distilled to 6 steps with no CFG: keep `--cfg-scale 1.0`, `--steps 6` and the `minimax-h3-image` sigma preset. On small GPUs add `--offload-to-cpu` or cap the card with `--max-vram <GB>`; on Macs and CPU-only machines the whole pipeline runs from RAM.

Rendered with `UD-Q4_K_XL` and the `Q4_K_M` text encoder through Unsloth Studio's stable-diffusion.cpp backend on an NVIDIA L4, 1024x1024, 6 steps, seed 1234.

Image editing with the release example: the reference (left) and the edit (right), same settings.

Same prompts, seeds, settings and text-encoder conditioning for every file; only the DiT changes. LPIPS (AlexNet, lower is better) and PSNR against the bf16 DiT rendered by the same stable-diffusion.cpp CUDA build: 8 prompts x 2 seeds at 1024x1024 (95% interval over prompts), plus 4 prompts at 2048x2048, on the 6-step release schedule. A repeated bf16 render is bit-identical. Peak VRAM is DiT sampling plus VAE decode on an A100 40 GB; the text encoder runs first and needs its own memory (or `--offload-to-cpu`).

MiniMax-H3-Image is sensitive to small changes in its inputs: a text encoder quantized to Q4_K_M instead of bf16 alone moves a 1024x1024 image by LPIPS 0.17. Lower-bit files keep the subject, style and text of the prompt but move details and layout more, which is what the LPIPS column tracks.

The files were also run on an NVIDIA L4 (24 GB), an RTX PRO 6000 Blackwell and the CPU. On the L4, `Q8_0`, `UD-Q4_K_XL` and `UD-Q2_K_XL` rank in the same order as on the A100 on every prompt. Through Unsloth Studio, Q8_0 does not fit next to the compute buffers on a 24 GB card and partly runs from host RAM there (about 14 s per step), so on 24 GB cards `UD-Q4_K_XL` is the faster choice there; plain `sd-cli` on the same L4 ran Q8_0 at 1.24 s per step. Image editing with `UD-Q4_K_XL` on the L4 stays within LPIPS 0.025 to 0.066 of the bf16 edit on three release examples.

| `UD-Q4_K_XL` , 1024x1024, 6 steps | s per step | s per image, DiT + VAE | s per image with the Q4_K_M text encoder | 
|---|---|---|---|
| NVIDIA L4 24 GB, `sd-cli` | 1.12 to 1.18 |  | about 105 to 111 | 
| NVIDIA L4 24 GB, Unsloth Studio | 1.74 | 12.1 | 71.4 | 
| NVIDIA L4, `--max-vram 12` | 3.91 | 25.1 | 88.1 | 
| NVIDIA A100 40 GB | 1.25 |  |  | 
| RTX PRO 6000 Blackwell 96 GB | 0.40 | 2.8 | 3.9 | 
| CPU, 16 threads, 512x512 | 6.7 to 7.3 | 47 to 50 |  | 

On the L4 most of the end-to-end time is the 32B text encoder, which does not fit next to the DiT and runs largely from host RAM.

Use MiniMax-H3-Image directly via API.

- Global: platform.minimax.io | CN: platform.minimax.cn

API docs:

Use MiniMax-H3-Image directly via App.

- WebApp Global: hailuoai.video | CN: hailuoai.com
- Desktop Global: agent.minimax.io | CN: agent.minimax.cn

MiniMax-H3-Image is a state-of-the-art open-source model for image generation and editing from the MiniMax H3 family. It combines intelligent understanding of creative intent with the ability to build visual worlds: with precise instruction following, it develops a creative concept into concrete characters, scenes and atmosphere. Generation and editing are unified in one checkpoint, which generates images from text (text-to-image) and edits one or more reference images (image editing), giving creators and developers a flexible and controllable way to create images.

It generates images of about 16 megapixels at every aspect ratio, from 4096x4096 (1:1) to 6272x2688 (21:9), directly and without an upscaler (see Sizes). The checkpoint is distilled to 6 sampling steps (6 NFEs) with no classifier-free guidance. Its diffusion transformer (DiT) has the H3 architecture without the audio layers, and it uses the MiniMax-H3 text encoder unchanged.

**Highlights**

MiniMax-H3-Image brings together three core strengths: speed, image quality, and instruction following.

1. **16-megapixel images at a practical computational cost.** Compared with the 1 to 4 MP outputs
common today, 16 MP (4096 × 4096) images offer far more detail but typically require
substantially more computation. MiniMax-H3-Image keeps that computational cost manageable,
making 16 MP generation practical for everyday creation.
2. **Rich detail. Remarkable realism.** MiniMax-H3-Image goes beyond the familiar “AI look” to
produce images with intricate detail and lifelike realism, opening up more possibilities for
creative and professional use. It brings generated imagery closer to the richness and nuance
of the real visual world.
3. **Stronger instruction following.** Greater creative control. Great image generation goes
beyond beautiful pictures: it helps creators bring their ideas to life. MiniMax-H3-Image
delivers the strongest instruction following among open-source models and supports up to 16
reference images, giving creators more control over the final result.

| Model | Role | Precision | Size | Hugging Face | 
|---|---|---|---|---|
| **MiniMax-H3-Image** | Text-to-image and image editing | BF16 (DiT) | 68.65 GB | MiniMaxAI/MiniMax-H3-Image | 
| **MiniMax-H3-Image-PE** (optional) | Prompt expander for text-to-image | NVFP4 | 17.9 GB | MiniMaxAI/MiniMax-H3-Image-PE | 

MiniMax-H3-Image follows the H3-Base design with the audio branch removed. The text prompt and the reference images are encoded by the H3 text encoder; the reference images are also encoded by the image VAE. All tokens form one packed sequence for a single-stream transformer, which predicts the latent of the target image. The VAE decodes that latent into the image.

- **Text encoder** : the full Qwen3-VL text encoder of MiniMax-H3, unchanged. The DiT reads
the hidden states of its 50th layer.
- **Image VAE** : a CNN encoder and a 2D ViT decoder (24 layers), with 16x spatial
downsampling and 24 latent channels. The DiT patchifies the latent with a 2x2 patch, so each
transformer token covers 32x32 pixels.
- **Diffusion transformer** : 33.1B parameters, a dense single-stream transformer with 50
layers (hidden size 5376, 56 attention heads, FFN size 14336) and 3D multimodal RoPE, the same
as the H3-Omni-Transformer. The weights are BF16; the embedding, timestep and output layers
are FP32.

| Component | Content | Size | 
|---|---|---|
| `transformer/` | DiT: BF16, with the embedding, timestep and output layers in FP32. | 66.28 GB | 
| `pdd_fused_heads.safetensors` ,`pdd_config.json` | The per-step output heads that replace the transformer's output layer, and the sampling schedule. | 12 MB | 
| `vae/` | Image VAE, FP32. | 2.34 GB | 
| `tokenizer/` ,`processor/` | Qwen3-VL tokenizer and processor, identical to MiniMax-H3. | 23 MB | 
| Text encoder | Not in this repository. `model_index.json` loads it from`MiniMaxAI/MiniMax-H3` , folder`text_encoder/` . If you already have MiniMax-H3, it is not downloaded again. | 66.7 GB | 

```
MiniMax-H3-Image/
├── model_index.json
├── pdd_config.json
├── pdd_fused_heads.safetensors
├── processor/
├── tokenizer/
├── transformer/
└── vae/
```
Download this repository and the MiniMax-H3 text encoder into the Hugging Face cache:

```
hf download MiniMaxAI/MiniMax-H3-Image
hf download MiniMaxAI/MiniMax-H3 --include "text_encoder/*"
# optional prompt expander
hf download MiniMaxAI/MiniMax-H3-Image-PE
```
We recommend the following inference frameworks to serve the model:

- SGLang - see cookbook
- vLLM-Omni - see vllm-omni recipe
- diffusers - see diffusers docs

See the vLLM-Omni recipe for per-hardware deployment configurations and measured latency. Both NVIDIA and AMD accelerators are supported, including NVIDIA H200, RTX PRO 6000 Blackwell, AMD Instinct MI350X and more.

Start the server on one GPU:

```
vllm serve MiniMaxAI/MiniMax-H3-Image --omni --host 0.0.0.0 --port 8000
```
For lower latency, split each request over several GPUs with sequence parallelism (Ulysses), for example on 4 GPUs:

```
vllm serve MiniMaxAI/MiniMax-H3-Image --omni --host 0.0.0.0 --port 8000 \
  --num-gpus 4 --usp 4 --text-encoder-tp-size 4
```
Here we use SGLang as a deployment example. See the MiniMax-H3-Image deployment guide for additional deployment configurations.

Start the server on one GPU:

```
sglang serve --model-path MiniMaxAI/MiniMax-H3-Image --host 0.0.0.0 --port 30000 --warmup-mode off
```
To use a local copy of the MiniMax-H3 `text_encoder/` folder instead of the Hugging Face
cache, add `--text-encoder-path <path to text_encoder>`.

For lower latency, split each request over several GPUs with sequence parallelism (Ulysses), for example on 4 GPUs:

```
sglang serve --model-path MiniMaxAI/MiniMax-H3-Image --host 0.0.0.0 --port 30000 --warmup-mode off \
  --num-gpus 4 --ulysses-degree 4
```
Text-to-image requests are JSON sent to `/v1/images/generations`. Image editing requests send
the reference images as multipart form data to `/v1/images/edits`; their order defines
`<Picture 1>`, `<Picture 2>`, and so on in the prompt. Both return the PNG in
`data[0].b64_json`. Runnable examples are in Full Workflow.

| Field | Text-to-image | Image editing | Notes | 
|---|---|---|---|
| `prompt` | Required | Required | Used as given, truncated to 4096 tokens. See Full Workflow | 
| `size` | Required | Required | `WIDTHxHEIGHT` . The returned image has exactly this size; see Sizes | 
| `image` | - | One or more files | In upload order they are `<Picture 1>` ,`<Picture 2>` , ... in the prompt | 
| `seed` | Recommended | Recommended | Integer | 
| `generator_device` | `cpu` recommended | `cpu` recommended | Same seed, same initial noise on every machine | 
| `response_format` | `b64_json` | `b64_json` |  | 
| `min_generation_area` | Optional | Optional | Minimum generated area in pixels, 1024-16777216, default 4194304 (2048x2048). A smaller `size` is generated at the same aspect ratio on a larger canvas and resized back with LANCZOS; pass 1024 to turn this off. All sizes in the table below are above the default | 
| `num_inference_steps` | Omit or 6 | Omit or 6 | Other values return HTTP 400 | 
| `guidance_scale` | Omit or 1 | Omit or 1 | Other values return HTTP 400 | 
| `negative_prompt` | Not supported | Not supported | Rejected | 

The effective working range of MiniMax-H3-Image is about 4 MP to 20 MP, and it generally gives the best results at around 16 MP. Common 16 MP sizes are listed below:

| Aspect ratio | Size (width x height) | 
|---|---|
| 1:1 | 4096x4096 | 
| 4:3 / 3:4 | 4736x3552 / 3552x4736 | 
| 3:2 / 2:3 | 5024x3360 / 3360x5024 | 
| 16:9 / 9:16 | 5472x3072 / 3072x5472 | 
| 21:9 | 6272x2688 | 

MiniMax-H3-Image expects a detailed English description of the whole image. The MiniMax API
turns a short prompt in any language into such a description with **H3-Image-Context-IR** before
generation. We also provide
MiniMax-H3-Image-PE, which can be
deployed locally.

This section reproduces the quality of images generated directly by the MiniMax API with a
locally deployed SGLang server. The scripts are in
`examples/` and use only
the Python standard library; run them in that folder:

```
hf download MiniMaxAI/MiniMax-H3-Image --include "examples/*" --local-dir .
cd examples
```
Before running examples, set your MiniMax API endpoint and key to use the H3-Image-Context-IR API and the reference scripts. The scripts read them from the environment:

```
# MiniMax API endpoint (choose one)
# CN
export MINIMAX_API_BASE="https://api.minimax.cn"
# Global
# export MINIMAX_API_BASE="https://api.minimax.io"
# API key obtained from the MiniMax platform
export MINIMAX_API_KEY="<api-key>"
```
API docs:

- H3-Image-Context-IR: use /v1/h3_image_context_ir EN-docs, CN-docs
- Text-to-image: use /v1/images/generations EN-docs, CN-docs
- Image editing: use /v1/images/edits EN-docs, CN-docs

| Stage |  | Request | Result | 
|---|---|---|---|
| Prompt Enhancement | H3-Image-Context-IR (MiniMax API) | `python3 context_ir.py t2i` (script) | `revised_prompt.txt` ,`revised_size.txt` ; our run:`revised_prompt.t2i.txt` ,`revised_size.t2i.txt` (4736x3552) | 
|  | local alternative: MiniMax-H3-Image-PE | `scripts/expand_prompt.py` in the PE repository | Save `expanded_caption` as`revised_prompt.txt` and the size for`aspect_ratio` as`revised_size.txt` | 
| MiniMax-H3-Image generation: local SGLang |  | `python3 t2i.py --prompt revised_prompt.txt --size $(cat revised_size.txt)` (script) | `t2i.png` | 
| E2E API Reference: text-to-image by directly calling the MiniMax API |  | `python3 api_generate.py t2i` (script) | `api_t2i.png` | 


*t2i.png from local SGLang (left) and api_t2i.png from the MiniMax API (right), 4736x3552 (4:3 picked by H3-Image-Context-IR)*

`instruction.edit.txt`
with the reference image `reference1.png`
as `<Picture 1>`.

| Stage | Request | Result | 
|---|---|---|
| Prompt Enhancement: H3-Image-Context-IR (MiniMax API) | `python3 context_ir.py edit` (script) | `revised_prompt.txt` ,`revised_size.txt` ; our run:`revised_prompt.edit.txt` ,`revised_size.edit.txt` (4096x4096) | 
| MiniMax-H3-Image generation: local SGLang | `python3 edit.py --prompt revised_prompt.txt --size $(cat revised_size.txt)` (script) | `edit.png` | 
| E2E API Reference: image editing by directly calling the MiniMax API | `python3 api_generate.py edit` (script) | `api_edit.png` | 


*Reference image reference1.png (left), edit.png from local SGLang (middle) and api_edit.png from the MiniMax API (right), 4096x4096 (1:1 picked by H3-Image-Context-IR)*

Text-to-image and image editing results, shown downscaled.


*Photorealistic photography*


*Surreal editorial photography*


*Environment concepts*


*Characters, creatures and props*


*Painting and illustration styles*


*Text rendering with reasoning: logic puzzles in Chinese and English, solved correctly on the board*


*Image editing, one reference: an untextured 3D render (left) turned into a film still of a lunar research base (right)*


*Image editing, one reference: a game map (left) turned into a first-person 3D game view from the red arrow's position, with the map as a minimap (right)*


*Image editing, ten references: an empty room and nine pieces of furniture (top) composed into one living room (bottom)*

- MiniMax-H3-Image is released under the MiniMax-H3-Image Community License Agreement.
- The T2I PE Model (MiniMax-H3-Image-PE) is part of MiniMax-H3-Image and is accordingly subject to MiniMax-H3-Image Community License Agreement.
- The separate written authorization described in Section IV of the license is requested through Application form

Contact us at model@minimax.io.

- Downloads last month
- 209,624
