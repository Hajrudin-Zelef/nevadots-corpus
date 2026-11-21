---
id: collect-261121-veille-ia/qwen/how-to-run-qwen3-8-27b-locally-ollama-lm-studio-vllm
title: "How to Run Qwen3.8-27B Locally: Ollama, LM Studio, vLLM"
domain: qwen
role: reference
task: article
actors: ["Alibaba"]
dates: []
keywords: ["model", "ollama", "qwen3", "studio", "vllm", "locally", "local", "confirm"]
source: https://www.sitepoint.com/qwen3-8-27b-local-gpu-setup-ollama/
source_anchor: ""
source_lines: [1, 400]
section: "Qwen"
---
Running a frontier-class open model locally has become a realistic option for developers who want zero API costs, full data sovereignty, and offline availability. Qwen3.8-27B is an Apache 2.0 open-weights model that combines a 27-billion-parameter dense architecture, native 262K context, vision capabilities, and YaRN-extended 1M context in a package that fits on a single 24 GB consumer GPU at Q4_K_M quantization. This article walks through the practical setup across three serving tools: Ollama, LM Studio, and vLLM.

**Note:** Before following any steps in this article, confirm that the model exists under its exact name in the Ollama library and/or on HuggingFace. Model identifiers and tags must match the registry exactly; the commands below use placeholder names that you should replace with the verified identifiers.


## How to Run Qwen3.8-27B on a Local GPU

1. **Verify** your GPU has at least 24 GB VRAM by running`nvidia-smi --query-gpu=memory.total --format=csv` .
2. **Confirm** the exact model identifier exists at the Ollama library or HuggingFace before downloading.
3. **Choose** a serving tool: Ollama for simplicity, LM Studio for a GUI workflow, or vLLM for throughput.
4. **Download** the Q4_K_M GGUF (Ollama/LM Studio) or AWQ (vLLM) quantized variant (~18 GB).
5. **Configure** the context window to 32,768 tokens as a starting point on 24 GB cards.
6. **Launch** the server and test with an interactive chat prompt to confirm correct generation.
7. **Benchmark** tokens-per-second using the provided timing script to establish a performance baseline.
8. **Integrate** the REST or OpenAI-compatible API endpoint into your application code.

## Table of Contents

## What Makes Qwen3.8-27B Worth Running Locally

### Model Specs at a Glance

Qwen3.8-27B is a 27-billion-parameter dense transformer, not a mixture-of-experts model, meaning all parameters are active on every forward pass. Its native context window is 262,144 tokens, and YaRN (Yet another RoPE extensioN) scaling pushes that to ~1 million tokens when configured correctly at the serving layer. The model supports vision inputs natively, handling image understanding tasks such as diagram comprehension, screenshot analysis, and document parsing alongside standard text generation.

Qwen3.8-27B uses the Apache 2.0 license. There are no usage restrictions, no commercial-use carve-outs, and no registration requirements. Developers can deploy it in products, fine-tune it, and redistribute modified weights without negotiating license terms.

### Why Local Over API?

For iteration-heavy workflows like prompt engineering, agentic loops, or RAG pipeline development, per-token API costs compound fast. A local deployment eliminates that variable entirely, though it shifts the cost to upfront hardware, setup time, and ongoing maintenance (driver updates, model version management, disk space).

Privacy-sensitive projects handling medical records, legal documents, or proprietary codebases keep all data on the local machine.


Privacy-sensitive projects handling medical records, legal documents, or proprietary codebases keep all data on the local machine. Offline availability matters for air-gapped environments and field deployments. Deterministic reproducibility — same weights, quantization, temperature, and seed yielding the same output — is far easier to guarantee locally than through a remote API whose backend may shift without notice. Local inference also removes network round-trip latency from agentic and RAG pipelines, putting time-to-first-token under the developer's direct control.

## Hardware Requirements and Quantization Options

### Prerequisites

- **NVIDIA GPU** with verified VRAM: run`nvidia-smi --query-gpu=memory.total --format=csv` to confirm.
- **NVIDIA driver** ≥ 525: run`nvidia-smi` and check the driver version displayed.
- **CUDA toolkit** (for vLLM): CUDA 11.8 or 12.1+. Confirm with`nvcc --version` .
- **OS:** Linux (all methods), macOS (Ollama and LM Studio only), Windows + WSL2 (Ollama only).
- **Python 3.8–3.11** (for vLLM and the benchmarking script). Confirm with`python3 --version` .
- **NVMe SSD** with at least 20 GB free space for Q4_K_M GGUF (up to 54 GB for FP16).

### VRAM Budget by Quantization Level

The following table maps quantization level to VRAM footprint, quality impact, recommended GPU tier, and the practical maximum context length achievable without out-of-memory errors.

| Quantization | VRAM (model weights) | Quality Impact | Recommended GPU | Max Practical Context | 
|---|---|---|---|---|
| FP16 | ~54 GB | Baseline (no loss) | 2x A100 80 GB | 262K | 
| Q8_0 | ~27 GB | Negligible | A6000 48 GB (context limited to ~8K–16K on A100 40 GB) | 128K+ | 
| Q5_K_M | ~21 GB | Very minor | A6000 48 GB (insufficient KV headroom on 24 GB GPUs) | 64K | 
| Q4_K_M | ~18 GB | Minor; best cost/quality ratio | RTX 4090/5090 24 GB | 32K–48K | 
| Q3_K_M | ~14 GB | Noticeable on reasoning tasks | RTX 4070 Ti Super 16 GB | 16K–24K | 

Q4_K_M hits the sweet spot for 24 GB consumer cards such as the RTX 4090 and RTX 5090. It leaves ~6 GB of VRAM headroom for the KV cache, which directly determines how much context the model can actually use at runtime. The NVIDIA A5000 also has 24 GB of VRAM but is a professional Quadro-class GPU at a higher price point (~$2,000+ vs. ~$1,600 for an RTX 4090); it offers the same VRAM budget but targets workstation deployments.

**Important:** Q5_K_M (~21 GB weights) does **not** fit usefully on a 24 GB GPU. After loading the weights, only ~3 GB remains — far less than the ~6 GB minimum needed for a practical KV cache. Use Q5_K_M only on 48 GB+ cards such as the A6000.


### Minimum vs. Recommended Specs

The minimum viable GPU is 16 GB VRAM, running Q3_K_M with context limited to around 16K tokens. The recommended setup is a 24 GB card running Q4_K_M. System RAM should be at least 32 GB for Q4_K_M/Q3_K_M. FP16 serving requires 64 GB+ system RAM to accommodate full weight staging during load. During model loading, the host memory temporarily holds the full model before transferring layers to the GPU, and KV cache that exceeds VRAM spills into system RAM. Storage should be NVMe SSD. Model files range from ~14 GB (Q3_K_M GGUF) to 54 GB (FP16), and loading from a spinning disk adds minutes to startup compared to seconds on NVMe.

CPU offloading, where some transformer layers run on the CPU while others remain on the GPU, is technically possible in llama.cpp-based backends. In practice, offloading more than a handful of layers to the CPU tanks generation speed to under 2 tokens per second, making it viable only for short, non-interactive tasks.

## Method 1: Running Qwen3.8-27B with Ollama

### Install Ollama and Pull the Model

Ollama provides the fastest path from zero to running inference. Installation varies by platform.

```
# Linux — download and inspect the install script before running
curl -fsSL https://ollama.com/install.sh -o install.sh
cat install.sh          # review the script contents
sh install.sh
# macOS — install via Homebrew
brew install ollama
# Windows — use WSL2 with Ubuntu, then run the Linux install above
# WSL2 GPU access requires the NVIDIA Windows host driver (≥522.06) installed
# on the host and CUDA toolkit inside WSL2. nvidia-container-toolkit is only
# required for Docker-based Ollama deployments.
# Start the Ollama service (Linux/WSL2; macOS starts automatically)
ollama serve &
OLLAMA_PID=$!
# Wait for daemon to be ready (max 30 seconds)
for i in $(seq 1 30); do
  if curl -sf http://localhost:11434/ > /dev/null 2>&1; then
    echo "Ollama ready."
    break
  fi
  sleep 1
done
if ! curl -sf http://localhost:11434/ > /dev/null 2>&1; then
  echo "ERROR: Ollama did not start within 30 seconds." >&2
  kill "$OLLAMA_PID"
  exit 1
fi
# Pull the Q4_K_M quantized Qwen3.8-27B model
# IMPORTANT: Verify the exact tag at https://ollama.com/library before running.
ollama pull qwen3.8-27b:q4_K_M
# Verify the model is downloaded
ollama list
# Confirm GPU layers are assigned (while a model is loaded)
ollama ps
```
The `ollama list` output should show `qwen3.8-27b:q4_K_M` with its size. Running `ollama ps` during an active model load confirms that GPU layers are assigned. If no GPU layers appear, check that the NVIDIA driver is installed correctly and that `nvidia-smi` shows the GPU.

### Interactive Chat and Basic Configuration

For repeatable configuration, a Modelfile lets developers lock down the system prompt, context window, temperature, and sampling parameters.

```
# Filename: Modelfile-qwen3.8-27b
FROM qwen3.8-27b:q4_K_M
PARAMETER num_ctx 32768
PARAMETER temperature 0.7
PARAMETER top_p 0.9
PARAMETER stop "<|im_end|>"
PARAMETER stop "<|endoftext|>"
SYSTEM """You are a senior software engineer. Provide precise, working code with brief explanations. Always specify language and runtime versions."""
```
Build and run:

```
# Build the custom model from the Modelfile
ollama create qwen3.8-custom -f Modelfile-qwen3.8-27b
# Run interactively
ollama run qwen3.8-custom
```
The `num_ctx 32768` value is a practical default for 24 GB cards at Q4_K_M. Pushing this higher increases KV cache memory consumption linearly. At 32K context, expect ~4 GB of KV cache on top of the 18 GB model weights, leaving a slim but workable margin on a 24 GB card.

### Using the Ollama REST API

Programmatic access opens integration with scripts, agents, and application backends. Ollama exposes a REST API on `localhost:11434` by default.

```
# Multi-turn chat with vision — sending a base64-encoded image
# Requires: jq (apt install jq / brew install jq)
IMAGE_B64=$(python3 -c \
  "import base64,sys; sys.stdout.write(base64.b64encode(open('screenshot.png','rb').read()).decode())")
PAYLOAD=$(jq -n \
  --arg img "$IMAGE_B64" \
  '{
    model: "qwen3.8-custom",
    messages: [{
      role: "user",
      content: "Describe the layout of this UI screenshot and identify any usability issues.",
      images: [$img]
    }],
    stream: false
  }')
curl http://localhost:11434/api/chat \
  -H "Content-Type: application/json" \
  -d "$PAYLOAD"
```
The same call using the `ollama` Python library:

```
import ollama
import base64
import os
import sys
image_path = "screenshot.png"
if not os.path.isfile(image_path):
    sys.exit(f"ERROR: Image file not found: {image_path}")
try:
    with open(image_path, "rb") as f:
        image_data = base64.b64encode(f.read()).decode("utf-8")
except OSError as e:
    sys.exit(f"ERROR: Could not read image file: {e}")
response = ollama.chat(
    model="qwen3.8-custom",
    messages=[
        {
            "role": "user",
            "content": "Describe the layout of this UI screenshot and identify any usability issues.",
            "images": [image_data],
        }
    ],
)
print(response["message"]["content"])
```
For streaming responses, set `"stream": true` in the curl payload or pass `stream=True` to the Python method and iterate over the response chunks.

## Method 2: Running Qwen3.8-27B with LM Studio

### Download and GPU Configuration

LM Studio provides a GUI-based workflow. Open the model browser, search for "Qwen3.8-27B," and select the Q4_K_M GGUF variant. The download manager automatically verifies file integrity.

Once downloaded, navigate to the model settings panel. Set GPU offload layers to the maximum the card can support. On a 24 GB GPU with Q4_K_M weights, offloading all layers to the GPU is typically feasible. Set the context length in the same panel. Starting at 32,768 tokens is reasonable; monitor VRAM usage via `nvidia-smi -l 1` and adjust downward if the system runs out of memory during generation.

### OpenAI-Compatible API Server

LM Studio can expose a local server that speaks the OpenAI Chat Completions API. Enable it from the server tab in the UI. The default endpoint is `http://localhost:1234/v1`.

**Important:** The model identifier used in API calls must match the exact string shown in LM Studio's server tab after loading the model. Copy that string and use it as the `model` value below. The identifier shown here is an example and may not match your installation.


```
from openai import OpenAI
import os
client = OpenAI(
    base_url="http://localhost:1234/v1",
    api_key=os.environ.get("LM_STUDIO_API_KEY", "lm-studio"),
)
stream = client.chat.completions.create(
    model="qwen3.8-27b-q4_k_m",  # replace with exact ID from LM Studio server tab
    messages=[
        {"role": "system", "content": "You are a concise technical assistant."},
        {"role": "user", "content": "Explain the difference between PagedAttention and standard KV cache allocation in three sentences."},
    ],
    stream=True,
)
for chunk in stream:
    if not chunk.choices:          # guard: empty choices on error chunks
        continue
    delta_content = chunk.choices[0].delta.content
    if delta_content:
        print(delta_content, end="", flush=True)
print()
```
This is a drop-in replacement for any code that already targets the OpenAI SDK. Change `base_url` and `model`, and the rest of the application logic remains untouched.

## Method 3: Running Qwen3.8-27B with vLLM (for Throughput)

### When to Choose vLLM Over Ollama or LM Studio

vLLM targets throughput. Its PagedAttention mechanism manages GPU memory for the KV cache in non-contiguous blocks, similar to virtual memory paging, dramatically reducing memory waste when serving multiple concurrent requests. Continuous batching means vLLM folds new requests into the in-flight batch without waiting for existing requests to finish. These features make vLLM the right choice for batch inference jobs, load testing, and production-adjacent serving. The trade-off is setup complexity: vLLM requires Linux, a compatible CUDA toolkit (CUDA 11.8 or 12.1+), and as of recent versions, GGUF support may be experimental or absent — verify with your installed version via `vllm --version` and the vLLM changelog. AWQ and GPTQ are the recommended quantization formats for vLLM.

### Installation and Model Serving

```
# Create a virtual environment and install vLLM
python3 -m venv vllm-env
source vllm-env/bin/activate
pip install vllm==0.4.3  # pin to a verified stable release; update after testing
# Verify installation and CUDA compatibility
python3 -c "import vllm; print('vLLM version:', vllm.__version__)"
python3 -c "import torch; print('CUDA available:', torch.cuda.is_available(), '| CUDA version:', torch.version.cuda)"
nvcc --version  # should show CUDA 11.8 or 12.1+
# Verify --rope-scaling flag name for your installed version
vllm serve --help 2>&1 | grep -i rope
# Launch the server with an AWQ-quantized Qwen3.8-27B
# IMPORTANT: Replace the model ID below with the verified HuggingFace repo ID
# from https://huggingface.co/Qwen. The ID must match exactly, including
# capitalization. An incorrect ID will cause a 404 error.
vllm serve Qwen/Qwen3.8-27B-AWQ \
  --quantization awq \
  --max-model-len 32768 \
  --gpu-memory-utilization 0.85 \
  --dtype auto \
  --host 127.0.0.1 \
  --port 8000
# --host 127.0.0.1: binds to localhost only. Use 0.0.0.0 only with firewall/
#   network isolation in place, as vLLM has no built-in authentication.
# --max-model-len: caps the context window; lower this if OOM occurs
# --gpu-memory-utilization: fraction of VRAM vLLM is allowed to use; 0.85 is a
#   conservative starting point. Increase to 0.90 if stable, but monitor for OOM
#   on long contexts.
# --quantization: must match the weight format (awq, gptq, or omit for FP16)
```
**CUDA version mismatch:** Installing vLLM via `pip install vllm` defaults to a CUDA 12.1 wheel. If your system uses a different CUDA version, check the vLLM installation docs for instructions on installing the correct wheel. A CUDA mismatch produces cryptic runtime errors.


Test the endpoint:

```
curl http://localhost:8000/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "Qwen/Qwen3.8-27B-AWQ",
    "messages": [
      {"role": "user", "content": "Write a Python function that merges two sorted lists in O(n) time."}
    ],
    "max_tokens": 512
  }'
```
### Benchmarking Your Setup

Measuring tokens per second across any of the three serving methods can be done with a simple timing script.

```
# Install dependency (requests is not part of the Python standard library)
pip install requests
```
```
import time
import requests
API_URL = "http://localhost:8000/v1/chat/completions"  # adjust port per tool
PAYLOAD = {
    "model": "Qwen/Qwen3.8-27B-AWQ",
    "messages": [{"role": "user", "content": "Explain quantum entanglement in 200 words."}],
    "max_tokens": 256,
    "stream": False,  # Python False -> JSON false via requests json= kwarg
}
start = time.perf_counter()
try:
    raw = requests.post(API_URL, json=PAYLOAD, timeout=120)
    raw.raise_for_status()
    resp = raw.json()
except requests.exceptions.RequestException as e:
    raise SystemExit(f"HTTP request failed: {e}")
except ValueError as e:
    raise SystemExit(f"Response was not valid JSON: {e}
Body: {raw.text[:200]}")
elapsed = time.perf_counter() - start
usage = resp.get("usage")
if not usage:
    raise SystemExit(f"Missing 'usage' field in response: {resp}")
completion_tokens = usage["completion_tokens"]
prompt_tokens = usage["prompt_tokens"]
# Wall time includes TTFT + prompt processing; true generation TPS is higher.
end_to_end_tps = completion_tokens / elapsed
print(f"Model: {PAYLOAD['model']}")
print(f"Prompt tokens:     {prompt_tokens}")
print(f"Completion tokens: {completion_tokens}")
print(f"Wall time:         {elapsed:.2f}s")
print(f"End-to-end TPS:    {end_to_end_tps:.1f}  (includes TTFT; generation-only TPS is higher)")
```
As a reference point, comparable 27B-parameter models at Q4_K_M / AWQ quantization on an RTX 4090 with 32K context typically achieve 20 to 35 tokens per second for generation with llama.cpp-based backends. An A100 80 GB at FP16 can push past 60 tokens per second under continuous batching via vLLM. Actual throughput for this specific model should be verified after confirming model availability.

## Practical Tips for Longer Contexts and Vision Tasks

### Scaling Context Without Running Out of VRAM

The KV cache grows linearly with context length. At Q4_K_M on a 24 GB card, the model weights consume around 18 GB, leaving ~6 GB for the KV cache. Each additional 1K tokens of context costs 100 to 200 MB of KV cache memory, depending on the attention head configuration. For this specific model architecture, measure empirically by monitoring `nvidia-smi` while incrementing `num_ctx`. This makes 32K to 48K tokens the practical ceiling on a 24 GB GPU. The 262K native context and 1M YaRN-extended context are achievable only on cards with substantially more VRAM (80 GB+) or through aggressive memory optimization.

For vLLM, YaRN scaling can be enabled by passing the appropriate RoPE scaling configuration. For example:

`--rope-scaling '{"type":"yarn","factor":4.0,"original_max_position_embeddings":262144}'`
Consult the model card for the correct `factor` and `original_max_position_embeddings` values. This allows the model to extrapolate beyond its training-time context when the VRAM budget permits.

### Feeding Images to the Model

Qwen3.8-27B accepts JPEG and PNG images. The vision encoder internally resizes input, but feeding images above the encoder's maximum input resolution (check the model card for the exact value) increases preprocessing time without proportional quality gains. Practical use cases include screenshot-to-code workflows, diagram comprehension for documentation generation, and UI analysis for accessibility audits.

```
# Screenshot-to-HTML workflow via Ollama API
# Requires: jq (apt install jq / brew install jq)
IMAGE_B64=$(python3 -c \
  "import base64,sys; sys.stdout.write(base64.b64encode(open('landing-page-screenshot.png','rb').read()).decode())")
PAYLOAD=$(jq -n \
  --arg img "$IMAGE_B64" \
  '{
    model: "qwen3.8-custom",
    messages: [{
      role: "user",
      content: "Generate a complete, responsive HTML and CSS implementation that faithfully reproduces this landing page design. Use semantic HTML5 elements and modern CSS (flexbox/grid).",
      images: [$img]
    }],
    stream: false
  }')
curl http://localhost:11434/api/chat \
  -H "Content-Type: application/json" \
  -d "$PAYLOAD"
```
This sends a screenshot to the model and requests a working HTML/CSS reproduction, a workflow directly applicable to rapid prototyping in web development.

## Implementation Checklist

1. Confirm GPU VRAM is at least 24 GB (or at least 16 GB with Q3_K_M and reduced context trade-offs). Run `nvidia-smi --query-gpu=memory.total --format=csv` .
2. Verify CUDA driver version compatibility with the chosen serving tool's requirements.
3. **Confirm model availability:** check the exact model tag at Ollama library and/or the exact HuggingFace repo ID at huggingface.co/Qwen before proceeding.
4. Choose a serving tool: Ollama for simplicity, LM Studio for a GUI-driven workflow, vLLM for throughput and batched inference.
5. Download the correct quantized model variant (Q4_K_M GGUF for Ollama/LM Studio, AWQ or GPTQ for vLLM).
6. Configure context length appropriate to VRAM budget (start at 32,768 tokens on 24 GB cards).
7. Test interactive chat with a baseline prompt to confirm the model loads and generates correctly.
8. Validate the vision pipeline by sending a sample image and checking the response.
9. Run the throughput benchmark script to establish a tokens-per-second baseline.
10. Integrate via the REST or OpenAI-compatible API into the target application.
11. Monitor VRAM usage under load with `nvidia-smi -l 1` and adjust context length or quantization if memory pressure appears.

## Troubleshooting Common Issues

### Out-of-Memory Errors

Reduce context length first: lower `num_ctx` in Ollama, the context setting in LM Studio, or `--max-model-len` in vLLM. If that is not sufficient, drop to a smaller quantization (Q3_K_M) or reduce the number of GPU offload layers to spill some weights to system RAM. If using vLLM, try lowering `--gpu-memory-utilization` to 0.80 as well.

### Slow Generation Speed

Check GPU utilization with `nvidia-smi`. If utilization is low, the bottleneck is likely CPU offloading or disk I/O. Disable partial CPU offload and ensure the model file is on an NVMe SSD. For vLLM, increase `--gpu-memory-utilization` to allow more VRAM for the KV cache.

### Garbled or Incoherent Output

The most common cause is a mismatch between the GGUF file and the chat template the serving tool applies. Verify that the quantized file matches the expected architecture and that the serving tool uses the correct prompt template (ChatML for Qwen models). A minimal ChatML-format exchange looks like:

```
<|im_start|>system
You are a helpful assistant.<|im_end|>
<|im_start|>user
Hello!<|im_end|>
<|im_start|>assistant
```
In Ollama, the Modelfile's `FROM` directive must point to the exact model tag pulled. Consult the Qwen model card for the definitive chat template specification.
