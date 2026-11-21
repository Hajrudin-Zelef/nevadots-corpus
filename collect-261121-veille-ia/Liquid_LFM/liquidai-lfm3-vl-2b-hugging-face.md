---
id: collect-261121-veille-ia/liquid-lfm/liquidai-lfm3-vl-2b-hugging-face
title: "LiquidAI/LFM3-VL-2B · Hugging Face"
domain: liquid-lfm
role: reference
task: article
actors: ["Liquid AI"]
dates: []
keywords: ["model", "lfm3-vl-2b", "video", "true", "inference", "text", "vision", "transformers"]
source: https://huggingface.co/LiquidAI/LFM3-VL-2B
source_anchor: ""
source_lines: [1, 363]
section: "Liquid_LFM"
---
LFM3-VL-2B is a **vision-language model for on-device visual understanding**. It takes text, images, and video as input, and combines the LFM3 language model with a new in-house vision encoder.

New in LFM3-VL-2B:

- **Video understanding:** the first LFM vision model that takes video as input. Ask questions about a recorded lecture, a walkthrough filmed on your phone, or a clip from a camera feed.
- **Thinking:** the model reasons step by step before it answers. This helps most with harder visual questions, such as math, charts, and multi-step problems.
- **Spatial and embodied reasoning:** the model finds objects and reasons about how they are placed relative to each other, a strong foundation for robotics applications.

Find more information about the LFM3 family in our release blog post.

The LFM3 family includes base, text, vision, audio, omni, and decision models.

| Property | Specification | 
|---|---|
| **Checkpoint** | Post-trained vision-language model | 
| **Parameters** | 1.92B: 1.73B language model, 164M vision encoder, 25M projector | 
| **Language model** | LFM3, 24 layers: 13 gated short convolutions, 5 sliding-window attention, 6 global attention | 
| **Vision encoder** | 14 layers: 8 gated depthwise 2D convolutions, 6 bidirectional attention, 16×16 patches with 2D rotary embeddings | 
| **Inputs** | Text, images, and video at native resolution. Each 2×2 block of patches becomes one language token. Video is sampled at 2 fps, with 4 to 768 frames by default. | 
| **Context length** | 128K tokens (input + output) | 
| **Vocabulary** | 128,000 tokens | 
| **Languages** | English, Arabic, Chinese, French, German, Italian, Japanese, Korean, Portuguese, Spanish, Vietnamese, Thai, Indonesian, Hindi, Russian, Polish | 
| **License** | LFM Open License v1.0 | 

LFM3-VL-2B is designed for on-device visual tasks: questions about video, reading documents, charts, and app screens, finding objects in an image, and spatial reasoning for robots. It is not recommended for coding, multi-turn conversations, or complex reasoning.

Choose a checkpoint variant for your inference framework and hardware.

| Checkpoint | Framework | Description | 
|---|---|---|
| BF16 | vLLM, SGLang, Transformers | BF16 weights for GPU inference. | 
| GGUF | llama.cpp, LM Studio | Local inference on CPUs and GPUs. | 
| MLX | MLX-VLM | 4-bit quantization for Apple Silicon. | 
| ONNX | ONNX Runtime | Cross-platform inference with ONNX Runtime. | 
| OpenVINO | OpenVINO | 4-bit quantization for Intel hardware. | 

**Sampling parameters:** `temperature=0.1, top_p=1.0, top_k=50, min_p=0.0, repetition_penalty=1.1`. For image and video preprocessing, use the values in `processor_config.json`.

**Transformers quick start** (`transformers>=5.21.0`):

```
pip install "transformers>=5.21.0" torch torchvision pillow
```
```
import torch
from transformers import AutoModelForImageTextToText, AutoProcessor
model_id = "LiquidAI/LFM3-VL-2B"
model = AutoModelForImageTextToText.from_pretrained(
    model_id, dtype=torch.bfloat16, device_map="auto", trust_remote_code=True
)
processor = AutoProcessor.from_pretrained(model_id, trust_remote_code=True)
messages = [
    {
        "role": "user",
        "content": [
            {"type": "image", "url": "https://placecats.com/300/200"},
            {"type": "text", "text": "Describe this image."},
        ],
    }
]
inputs = processor.apply_chat_template(
    messages,
    add_generation_prompt=True,
    tokenize=True,
    return_dict=True,
    return_tensors="pt",
).to(model.device)
with torch.inference_mode():
    output_ids = model.generate(
        **inputs,
        do_sample=True,
        temperature=0.1,
        top_k=50,
        repetition_penalty=1.1,
        max_new_tokens=16384,  # generate() needs an explicit limit; reasoning can be long
    )
generated_ids = output_ids[:, inputs["input_ids"].shape[1] :]
print(processor.batch_decode(generated_ids, skip_special_tokens=True)[0])
```
## **On-device inference with llama.cpp**

For most on-device use cases, we recommend llama.cpp b11960 or newer with the GGUF checkpoints. To run it in a desktop app such as Llama.app or LM Studio, click **Use this model** on the GGUF page and pick the app.

From a terminal, start a web chat UI and an OpenAI-compatible API at `http://localhost:8080`. With `-hf`, llama.cpp downloads the model and the matching vision projector (mmproj):

```
llama serve -hf LiquidAI/LFM3-VL-2B-GGUF:Q4_0 \
  --temp 0.1 --top-p 1.0 --top-k 50 --min-p 0.0 --repeat-penalty 1.1 \
  --no-reasoning-preserve --port 8080
```
To ask one question from the command line, use `llama cli` with the same flags, `--image <IMAGE PATH>` or `--video <VIDEO PATH>`, and `-p "<PROMPT>"`. Video input needs `ffmpeg` in `PATH`.

## **GPU inference with vLLM and SGLang**

For high-throughput serving on GPU, use vLLM or SGLang. Both serve an OpenAI-compatible API. Support for LFM3-VL in SGLang is in review in sgl-project/sglang#46858, and support in vLLM is in progress. We will update this card when their releases support it.

**vLLM**:

```
vllm serve LiquidAI/LFM3-VL-2B \
  --reasoning-parser qwen3 \
  --enable-auto-tool-choice --tool-call-parser qwen3_coder \
  --chat-template-content-format openai \
  --structured-outputs-config '{"backend": "xgrammar", "disable_any_whitespace": true}' \
  --media-io-kwargs '{"video": {"video_backend": "qwen3_vl"}}'
```
**SGLang**:

```
sglang serve --model-path LiquidAI/LFM3-VL-2B --trust-remote-code \
  --reasoning-parser auto \
  --tool-call-parser auto \
  --constrained-json-disable-any-whitespace
```
Send an image-and-text request with the OpenAI client. Change `base_url` to `http://localhost:30000/v1` for SGLang, or `http://localhost:8080/v1` for llama.cpp:

```
from openai import OpenAI
client = OpenAI(base_url="http://localhost:8000/v1", api_key="EMPTY")
response = client.chat.completions.create(
    model="LiquidAI/LFM3-VL-2B",
    messages=[
        {
            "role": "user",
            "content": [
                {"type": "image_url", "image_url": {"url": "https://placecats.com/300/200"}},
                {"type": "text", "text": "Describe this image."},
            ],
        }
    ],
    temperature=0.1,
    extra_body={"top_k": 50, "repetition_penalty": 1.1},
)
print(response.choices[0].message.content)
```
## **Video frame rate and frame limit**

By default, LFM3-VL-2B samples video at 2 fps, with at least 4 and at most 768 frames per video. Each frame adds vision tokens, so for long videos a lower frame rate or frame limit uses less context and memory.

- **Transformers:** set the frame rate per call with`processor.apply_chat_template(..., processor_kwargs={"fps": 1})` . Set the frame limit when you load the processor:`AutoProcessor.from_pretrained(model_id, trust_remote_code=True, max_frames=128)` .
- **vLLM:** set a server default in`--media-io-kwargs` , for example`'{"video": {"video_backend": "qwen3_vl", "fps": 1, "max_frames": 128}}'` . Override it per request with`extra_body={"media_io_kwargs": {"video": {"fps": 1, "max_frames": 128}}}` .
- **SGLang:** change`fps` ,`min_frames` , and`max_frames` under`video_processor` in`processor_config.json` in a local copy of the checkpoint.
- **llama.cpp:** always uses 2 fps and 4 to 768 frames for this model.`--video-fps` has no effect.

## **Chat template**

LFM3-VL uses this ChatML format, generated from the checkpoint's chat template:

```
<|startoftext|><|im_start|>system
You are a helpful assistant trained by Liquid AI.<|im_end|>
<|im_start|>user
<|vision_start|><|image|><|vision_end|>What species is in this picture?<|im_end|>
<|im_start|>assistant
<think>
```
Use `processor.apply_chat_template()` with `add_generation_prompt=True` to format messages automatically. The model thinks in a `<think>...</think>` block before it answers.

The template inserts `<|vision_start|><|image|><|vision_end|>` for each image and `<|vision_start|><|video|><|vision_end|>` for each video. Do not put these tags in your message content.

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
LFM3's tool call format has changed from the custom LFM2/LFM2.5 format to a superset of `qwen3_coder`.


Execute the call in your application. Add the parsed assistant call to history as a `tool_calls` entry, then append the result as a `tool` message. Keep passing the tool schemas on subsequent requests and preserve the assistant's content and reasoning from the current tool-use turn.

`function.arguments` must be a Python mapping. Decode JSON strings with `json.loads()` before applying the template.

See the Tool Use documentation for the full guide.

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
Your harness implements and runs the search. Keep the full `tools` list, including deferred tools, on every request.

See Deferred tools and tool search for the full guide.

LFM3-VL-2B is designed for **easy customization** for a given domain, task, or application via TRL.

| Training | Use case | Notebook | 
|---|---|---|
| **SFT** | Extract structured JSON from receipt photos |  | 
| **GRPO** | Transcribe images of equations to LaTeX |  | 

Across 62 vision benchmarks, LFM3-VL-2B scores at least 8 points ahead of all models of similar size. It matches Gemma 4 12B, is within 2 points of GPT-5.6 Luna, and scores higher than Gemini 3.5 Flash Lite.

| Category | **LFM3-VL 2B** | **LFM3-VL 2B (no think)** | LFM2.5-VL 3B | MiniCPM-V 4.7 1B | Qwen3.5 2B | Qwen3.5 4B | InternVL3.5 4B | Gemma 4 E2B | Gemma 4 12B | GPT-5.6 Luna | Gemini 3.5 Flash Lite | 
|---|---|---|---|---|---|---|---|---|---|---|---|
| General | 66.0 | 56.4 | 57.6 | 59.8 | 64.0 | 73.0 | 61.2 | 56.9 | __74.0__ | 72.2 | **74.1** | 
| Perception | __75.6__ | 68.9 | 67.9 | 65.5 | 71.2 | **77.1** | 63.2 | 59.1 | 73.5 | 71.0 | 73.0 | 
| Document, OCR & Chart | 83.9 | 71.6 | 71.2 | 73.0 | 79.1 | 84.3 | 72.1 | 72.3 | **85.7** | __84.4__ | 82.3 | 
| Math & logical reasoning | 68.8 | 41.0 | 40.6 | 51.8 | 61.0 | __78.4__ | 46.4 | 51.8 | **79.3** | 77.3 | 66.7 | 
| Visual grounding | **77.3** | 72.6 | 68.3 | 51.1 | 61.2 | 67.0 | 63.6 | 41.2 | __74.1__ | 72.7 | 63.5 | 
| Embodied reasoning | **64.4** | 57.9 | 48.0 | 45.3 | 51.2 | __63.2__ | 48.9 | 41.5 | 59.1 | 61.5 | 50.3 | 
| GUI understanding | **83.1** | 81.5 | 66.0 | 45.3 | 67.2 | 81.1 | 68.5 | 44.3 | 76.1 | __82.5__ | 71.0 | 
| Video | 61.2 | 54.0 | 41.3 | 53.7 | 57.0 | 67.5 | 59.7 | 46.6 | 58.9 | **72.8** | __70.0__ | 
| Multilingual | 84.4 | 76.2 | 81.2 | 77.4 | 78.6 | 83.9 | 79.0 | 73.4 | **86.1** | 82.1 | __85.8__ | 
| Agentic | 42.5 | 32.9 | 21.1 | 16.9 | 15.2 | 31.3 | N/A | 23.8 | 43.6 | __56.1__ | **62.4** | 
| **Average** | 71.4 | 62.1 | 57.6 | 57.5 | 64.0 | **73.4** | 61.1 | 53.6 | __71.7__ | **73.4** | 70.0 | 

## All 62 benchmarks

| Benchmark | **LFM3-VL 2B** | **LFM3-VL 2B (no think)** | LFM2.5-VL 3B | MiniCPM-V 4.7 1B | Qwen3.5 2B | Qwen3.5 4B | InternVL3.5 4B | Gemma 4 E2B | Gemma 4 12B | GPT-5.6 Luna | Gemini 3.5 Flash Lite | 
|---|---|---|---|---|---|---|---|---|---|---|---|
| General |  |  |  |  |  |  |  |  |  |  |  | 
| MME | 86.9 | 76.7 | 74.3 | 82.7 | 83.9 | __88.3__ | 82.2 | 79.6 | 85.7 | 74.8 | **89.2** | 
| MMStar | 74.4 | 62.2 | 63.3 | 68.1 | 71.5 | __76.6__ | 65.3 | 61.2 | **77.7** | 72.2 | 73.1 | 
| SimpleVQA | 36.3 | 27.7 | 32.0 | 36.5 | 38.0 | 45.8 | 33.4 | 28.4 | 42.5 | __53.2__ | **56.9** | 
| SEED-Bench (image) | 78.6 | 76.8 | 77.7 | 76.6 | 76.9 | 78.9 | 76.4 | 75.2 | **80.6** | 76.2 | __79.5__ | 
| MMBench (dev EN v1.1) | 86.1 | 76.5 | 81.0 | 83.4 | 80.0 | __86.5__ | 81.1 | 70.9 | **88.2** | 84.4 | **88.2** | 
| MMMU (val) | 55.7 | 48.6 | 47.2 | 51.0 | 58.1 | 74.6 | 57.8 | 57.3 | **76.7** | __75.2__ | 72.0 | 
| MMMU-Pro | 45.7 | 35.0 | 33.2 | 37.3 | 50.3 | 67.2 | 38.4 | 42.7 | __69.1__ | **70.9** | 63.2 | 
| MM-Vet (v1) | 57.4 | 44.3 | 48.2 | 46.8 | 53.4 | 59.9 | 65.3 | 44.8 | 64.1 | __67.1__ | **73.3** | 
| MM-IFEval | 72.8 | 54.7 | 60.6 | 58.9 | 62.0 | 79.5 | 57.3 | 61.6 | **83.0** | __82.0__ | 73.2 | 
| MuirBench | 66.4 | 61.2 | 58.2 | 57.0 | 65.5 | **72.8** | 54.8 | 47.5 | 71.9 | 66.0 | __72.7__ | 
| Perception |  |  |  |  |  |  |  |  |  |  |  | 
| BLINK | **71.1** | 68.7 | 61.0 | 60.7 | 63.5 | 69.8 | 57.4 | 52.9 | __70.9__ | 68.6 | 68.9 | 
| CountBenchQA | __93.0__ | 86.4 | 87.1 | 86.4 | 87.1 | 92.6 | 81.3 | 78.2 | **95.9** | 91.0 | 90.1 | 
| HallusionBench | 65.6 | 52.5 | 46.6 | 53.6 | 61.1 | __66.2__ | 58.0 | 56.7 | **68.9** | 59.0 | 65.6 | 
| POPE | 88.7 | 86.2 | __88.9__ | **89.2** | 88.1 | 88.1 | 88.3 | 84.2 | 87.6 | 85.0 | **89.2** | 
| PerceptionBench | 37.1 | 28.4 | 27.6 | 26.8 | 29.5 | 39.8 | 27.3 | 28.2 | __43.5__ | **48.8** | 37.7 | 
| HR-Bench (4K) | __82.3__ | 71.0 | 71.3 | 67.4 | 76.9 | **88.1** | 55.6 | 65.5 | 77.4 | 80.1 | 79.2 | 
| HR-Bench (8K) | __76.2__ | 64.9 | 62.1 | 61.4 | 74.4 | **82.1** | 54.6 | 57.0 | 70.6 | 60.0 | 75.7 | 
| ZoomBench (full + crop average) | __62.6__ | 60.4 | 61.6 | 52.0 | 56.4 | 61.9 | 52.7 | 43.7 | 59.4 | 59.7 | **65.0** | 
| V*Bench | __86.4__ | 80.6 | 81.2 | 71.7 | 85.9 | **89.5** | 71.2 | 44.5 | 69.6 | 79.6 | 74.9 | 
| RealWorldQA | __79.3__ | 73.1 | 72.9 | 68.2 | 75.7 | **81.0** | 67.8 | 61.2 | 76.9 | 72.4 | 74.4 | 
| CV-Bench | **88.8** | 85.4 | 86.3 | 82.6 | 84.3 | __88.4__ | 80.4 | 77.6 | 87.5 | 76.9 | 82.7 | 
| Document, OCR & Chart |  |  |  |  |  |  |  |  |  |  |  | 
| DocVQA (val) | 96.7 | 94.9 | 91.8 | 91.8 | 95.6 | **97.1** | 90.9 | 90.4 | __97.0__ | 94.8 | 96.2 | 
| TextVQA (val) | 87.9 | 85.8 | **90.4** | 87.4 | 85.0 | 88.0 | 84.9 | 80.6 | 88.4 | 85.5 | __88.9__ | 
| OCRBench v1 | __86.3__ | 82.2 | 84.7 | 82.1 | 83.6 | 86.0 | 83.5 | 71.8 | 81.1 | 79.8 | **87.9** | 
| OCRBench v2 (En) | 63.5 | 48.1 | 48.3 | 44.1 | 55.4 | __65.7__ | 50.3 | 51.3 | **67.2** | 64.8 | 55.5 | 
| ChartQA (test) | **92.7** | 81.4 | 80.6 | 88.8 | 85.2 | 85.8 | 86.0 | 82.3 | 89.4 | __90.2__ | 88.2 | 
| InfographicVQA (val) | 86.2 | 70.6 | 65.2 | 72.3 | 85.9 | __90.6__ | 67.0 | 71.1 | 89.3 | **92.4** | 84.0 | 
| CharXiv (descriptive, val) | 87.5 | 65.5 | 69.4 | 73.4 | 84.0 | 92.4 | 67.6 | 85.0 | **95.8** | __94.3__ | 88.2 | 
| CharXiv (reasoning, val) | 70.2 | 44.5 | 38.9 | 44.5 | 58.1 | 68.6 | 46.5 | 46.3 | **77.1** | __73.6__ | 69.2 | 
| Math & logical reasoning |  |  |  |  |  |  |  |  |  |  |  | 
| LogicVista | 60.4 | 30.2 | 34.9 | 46.1 | 54.4 | 69.8 | 37.6 | 48.3 | **76.3** | __74.7__ | 57.0 | 
| MATH-Vision (full) | 51.6 | 28.4 | 22.9 | 30.2 | 41.8 | 72.0 | 33.8 | 40.1 | __73.8__ | **79.9** | 57.5 | 
| MathVista (mini) | __84.2__ | 68.5 | 68.9 | 71.2 | 75.5 | **84.8** | 72.7 | 62.7 | 80.4 | 76.9 | 77.7 | 
| We-Math (testmini, strict) | 68.6 | 20.1 | 23.0 | 47.8 | 59.8 | **81.0** | 26.2 | 44.2 | __78.2__ | 70.3 | 59.1 | 
| DynaMath (average accuracy) | 79.2 | 57.7 | 53.3 | 63.8 | 73.6 | __84.6__ | 61.5 | 63.9 | **87.6** | __84.6__ | 82.2 | 
| Visual grounding |  |  |  |  |  |  |  |  |  |  |  | 
| RefCOCO-avg | **90.5** | 87.4 | 87.9 | 86.3 | 78.3 | 87.4 | __88.8__ | 68.0 | 82.2 | 81.8 | 81.6 | 
| RefCOCO-M | **95.2** | 92.9 | 94.4 | 92.8 | 90.6 | 93.3 | __94.6__ | 76.5 | 88.8 | 89.5 | 87.6 | 
| Point-Bench | **73.8** | 69.6 | 58.7 | 16.8 | 54.0 | 56.7 | 47.3 | 14.3 | __73.4__ | 66.8 | 51.4 | 
| RefSpatial-Bench | 49.8 | 40.6 | 32.1 | 8.5 | 21.7 | 30.7 | 23.5 | 6.0 | __51.7__ | **52.5** | 33.6 | 
| Embodied reasoning |  |  |  |  |  |  |  |  |  |  |  | 
| EmbSpatial-Bench | **82.9** | 77.1 | 74.3 | 71.5 | 77.0 | __82.4__ | 71.0 | 67.9 | 80.2 | 75.5 | 75.3 | 
| ERQA | 48.2 | 40.5 | 39.8 | 44.8 | 41.2 | 52.7 | 37.0 | 37.8 | **59.5** | __56.5__ | 50.5 | 
| MindCube (tinybench) | **56.5** | 51.3 | 45.5 | 47.0 | 43.1 | 52.7 | 39.9 | 37.5 | __54.9__ | 45.9 | 37.5 | 
| SAT (val + test) | **80.2** | 71.0 | 57.2 | 56.3 | 66.7 | __80.1__ | 59.5 | 59.0 | 74.8 | 71.0 | 62.8 | 
| RoboSpatial-Home | 63.4 | 51.4 | 48.6 | 41.7 | 47.7 | **67.4** | 45.4 | 46.9 | __66.9__ | 63.7 | 41.4 | 
| Where2Place | 44.9 | 42.8 | 42.5 | 6.4 | 30.2 | 47.2 | 24.5 | 18.2 | __49.0__ | **51.6** | 21.0 | 
| OpenEQA (episodic memory, video) | 76.3 | 66.0 | 49.2 | 64.2 | 63.8 | 72.8 | 60.6 | 43.2 | 56.4 | **78.5** | __77.5__ | 
| VSI-Bench | __63.1__ | **63.3** | 26.5 | 30.6 | 39.8 | 50.5 | 52.9 | 21.3 | 31.2 | 49.2 | 36.5 | 
| GUI understanding |  |  |  |  |  |  |  |  |  |  |  | 
| ScreenSpot-Pro | __55.5__ | 49.3 | 22.8 | 4.9 | 38.1 | 50.6 | 21.5 | 1.3 | 25.2 | **57.4** | 39.2 | 
| ScreenSpot-v2 Desktop | __94.6__ | **95.5** | 78.4 | 51.5 | 75.7 | 91.6 | 81.7 | 52.1 | 93.1 | __94.6__ | 86.5 | 
| ScreenSpot-v2 Mobile | 92.2 | 91.6 | 80.6 | 68.1 | 82.2 | __92.4__ | 87.8 | 71.1 | **93.0** | 88.8 | 80.0 | 
| ScreenSpot-v2 Web | __89.9__ | 89.7 | 81.9 | 56.8 | 72.5 | 89.7 | 82.8 | 52.9 | **92.9** | 89.0 | 78.3 | 
| Video |  |  |  |  |  |  |  |  |  |  |  | 
| MVBench | 69.4 | 59.5 | 50.7 | 53.9 | 64.5 | __71.3__ | **72.3** | 45.5 | 54.6 | 68.2 | 64.5 | 
| ActivityNet-QA (test) | **71.2** | 59.1 | 40.8 | 58.0 | 59.4 | 64.1 | 63.0 | 47.7 | 57.2 | __66.8__ | 65.6 | 
| LVBench | 41.2 | 40.9 | 34.2 | 39.9 | 35.2 | 47.3 | 42.5 | 27.5 | 36.5 | **58.9** | __58.7__ | 
| MLVU (dev, M-Avg) | 71.3 | 63.9 | 47.0 | 64.9 | 68.3 | 76.1 | 66.2 | 45.1 | 53.9 | __77.6__ | **78.2** | 
| MMVU (val) | 52.9 | 45.1 | 38.9 | 45.9 | 53.8 | 67.5 | 52.0 | 47.7 | 65.5 | **77.6** | __76.3__ | 
| MotionBench (dev) | __64.0__ | 56.8 | 50.2 | 51.1 | 56.1 | 61.8 | 58.7 | 41.5 | 53.9 | **67.7** | 61.0 | 
| Video-MME (v1, no subtitles) | 62.8 | 57.9 | 44.3 | 57.9 | 59.4 | 71.1 | 61.3 | 48.4 | 62.1 | **75.7** | __74.0__ | 
| Video-MME (v1, subtitles) | 69.0 | 63.1 | 43.1 | 62.1 | 65.1 | __78.3__ | 63.0 | 59.1 | 73.3 | **81.6** | 77.8 | 
| Video-MMMU | 49.6 | 39.3 | 22.3 | 50.1 | 51.1 | 69.8 | 58.0 | 57.1 | 72.7 | **81.1** | __73.9__ | 
| Multilingual |  |  |  |  |  |  |  |  |  |  |  | 
| MMMB | 85.2 | 80.4 | 83.0 | 79.6 | 81.4 | 85.1 | 81.5 | 78.8 | **86.9** | 82.1 | __85.4__ | 
| Multilingual MMBench | 83.6 | 72.0 | 79.5 | 75.3 | 75.8 | 82.8 | 76.5 | 68.0 | __85.4__ | 82.1 | **86.2** | 
| Agentic |  |  |  |  |  |  |  |  |  |  |  | 
| MM-ToolSandbox (partial credit) | 42.5 | 32.9 | 21.1 | 16.9 | 15.2 | 31.3 | N/A | 23.8 | 43.6 | __56.1__ | **62.4** | 

## Evaluation details

We evaluated all models with vLLM 0.27.1, using each model's recommended generation parameters when available.

We used reasoning mode for all models except LFM2.5-VL-3B, which does not support it, and the LFM3-VL-2B no-thinking configuration, for which we disabled thinking. For InternVL3.5, we report the higher score from reasoning and non-reasoning modes on each benchmark. GPT-5.6 Luna and Gemini 3.5 Flash Lite used their default reasoning effort: medium for Luna and minimal for Gemini.

For models without native video support, we sampled frames at 2 FPS, up to a maximum of 384 frames per video, and provided them to the model as images.

All scores are normalized to a 0–100 scale. Each category score is the unweighted mean of its benchmark scores, and the Average row is the unweighted mean across all 62 benchmarks.

We used GPT-6 Luna as a judge for benchmarks that require an LLM judge.

**Original score scales:** MME uses a 0–2800 scale; OCRBench, 0–1000; and DocVQA and InfographicVQA, 0–1.

**RefCOCO-avg:** Mean P@1 across eight splits: RefCOCO (val, testA, testB), RefCOCO+ (val, testA, testB), and RefCOCOg (val, test).

**POPE:** F1 over all examples from the adversarial, popular, and random subsets.

**MMMU-Pro:** The multiple-choice variant with 10 answer options per question.

LFM3-VL-2B has the highest prefill throughput and the lowest memory use of all compared models on all three devices.

## Evaluation details

All models run in llama.cpp with a Q4_0 GGUF language model and an F16 vision projector (mmproj). Each request has one 512×512 image and 1,024 text tokens, and generates 256 tokens. Prefill is the prompt processing speed and decode is the generation speed, both in tokens per second. Memory is peak use in GB.

On one NVIDIA B300, LFM3-VL-2B produces more than 12,000 tokens/s at 1,024 concurrent requests, more than any compared model.

## Evaluation details

All models run in BF16 on SGLang v0.5.22 with LFM3 support. Each request has one 512×512 image and 1,024 text tokens, and generates at most 256 tokens. Throughput is end-to-end output tokens per second, as the mean of 3 runs.

**Pre-training:** LFM3-VL-2B starts from LFM3-1.7B-Base, which is pre-trained in two stages on 22T tokens in total.

- **Main pre-training:** compared with LFM2, the mix adds more scientific papers, repository-level code, math, and a wider range of knowledge sources.
- **Mid-training:** the last 3.5T tokens use curated, higher-quality data with a larger share of agentic traces and tool calls. This stage also extends the context window to 256K tokens.

**Post-training:** four stages develop visual understanding, reasoning, and tool use.

- **Vision mid-training:** LFM3-1.7B-Base is joined with our new in-house vision encoder, which combines convolution and attention, and trained on image and video data.
- **Supervised fine-tuning (SFT):** a wide mix of instruction following, reasoning, tool calling, agentic traces, grounding, and multimodal perception and understanding.
- **Vision reinforcement learning:** many domains trained at once, to improve reasoning and general intelligence.
- **Multi-domain on-policy distillation (MOPD):** a vision teacher model and the Mixed RL model from LFM3-1.7B are distilled back into a single model.

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
- 30,713
