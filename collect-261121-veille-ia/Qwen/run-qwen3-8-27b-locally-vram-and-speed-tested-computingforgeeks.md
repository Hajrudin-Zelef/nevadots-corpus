---
id: collect-261121-veille-ia/qwen/run-qwen3-8-27b-locally-vram-and-speed-tested-computingforgeeks
title: "Run Qwen3.8-27B Locally: VRAM and Speed Tested | ComputingForGeeks"
domain: qwen
role: reference
task: article
actors: ["Alibaba"]
dates: []
keywords: ["card", "vllm", "qwen3", "ollama", "context", "vram", "speed", "tested"]
source: https://computingforgeeks.com/run-qwen3-8-27b-locally/
source_anchor: ""
source_lines: [1, 215]
section: "Qwen"
---
A single RTX 4090 runs Qwen3.8-27B at over 100 tokens per second and still answers correctly from a 197,000-token prompt. Neither result comes from the default settings. The speed comes from the multi-token prediction (MTP) head Qwen ships inside the model, and the long context comes from a quantized KV cache. Both are covered below with the exact flags that worked.

This guide shows how to run Qwen3.8-27B locally three ways: Ollama for the quick path, llama.cpp GGUF quants when you want the whole context window on a 24 GB card, and vLLM when one bigger GPU has to serve a team. You get a VRAM table per quant and context size, single-stream and 16-user tokens per second for BF16, FP8 and NVFP4, and the two startup errors we hit. Every number was measured in September 2026 on an RTX 4090 (Ollama 0.34.4, llama.cpp build b11200) and an RTX PRO 6000 Blackwell Server Edition (vLLM 0.30.0), both on Ubuntu 24.04, and the guide was last reviewed in November 2026.

## How much VRAM Qwen3.8-27B needs

Qwen3.8-27B needs about 17 GB of VRAM at 4-bit and 51 GiB at BF16 before any context is added. A 24 GB card is enough for the 4-bit builds up to the full 262,144-token context window if the KV cache is also quantized. The full-precision weights are 55.6 GB on disk (vision encoder and MTP head included), so BF16 needs a 64 GB or larger card.

The model is Apache-2.0 on Hugging Face: a dense 27B vision-language model with 64 layers, of which only 16 are full attention. The other 48 are Gated DeltaNet linear-attention layers with a fixed-size state. That layout is why long context is cheap here. Going from 8K to 64K context in llama.cpp added 3,640 MiB of VRAM for 57,344 tokens, about 64 KiB per token at F16. A classic 27B with a KV cache in every layer would cost four times that.

| Build | Weights in VRAM | Smallest practical card | Context tested | 
|---|---|---|---|
| Ollama `qwen3.8:27b` (Q4_K_M) | 17 GB | 24 GB card | 131K with `q4_0` KV cache | 
| unsloth UD-Q4_K_XL GGUF | 16.3 GiB | 24 GB card | 262K with `q4_0` KV cache | 
| unsloth UD-Q5_K_XL GGUF | 19.4 GiB | 24 GB card | 8K tested with MTP on | 
| NVFP4 (vLLM) | 19.9 GiB | 32 GB Blackwell card at about 32K context (untested) | Accepts 262K; 1.01M-token KV pool on 96 GB | 
| FP8 (vLLM) | 28.5 GiB | 48 GB card, roughly one full-context request (untested) | Accepts 262K; 876K-token KV pool on 96 GB | 
| BF16 (vLLM) | 51.1 GiB | 64 GB card and up (untested) | Accepts 262K; 528K-token KV pool on 96 GB | 

We tested on a 24 GB and a 96 GB card. The smaller-card vLLM entries are extrapolated from weights plus KV cost, not measured. The official vLLM recipe for a single 32 GB RTX 5090 caps `--max-model-len` at 32768 and needs `--enforce-eager`, so the 262K command below will not start on a card that size. The vLLM KV pools above come from startup logs; no long-context request was sent to vLLM. The chart has the single-stream decode speed for every build we measured. Grey bars are plain decoding, coloured bars have the MTP draft head switched on.

MTP lifts single-stream speed by 1.6x to 2.3x on every engine. The 4090 numbers come from a real 512-token completion; the vLLM numbers come from `vllm bench serve` with random tokens, which drafts worse than real text, so treat the two groups as separate comparisons. For how these three engines compare on other models, see our Ollama vs vLLM vs llama.cpp benchmark on the same GPU.

## Run Qwen3.8-27B with Ollama

Ollama is the shortest path and the default tag already turns on MTP. Install a current NVIDIA driver first; the NVIDIA driver and CUDA guide for Ubuntu covers that part. Install Ollama with the official script:

`curl -fsSL https://ollama.com/install.sh | sh`
Pull the model. The `qwen3.8:27b` tag is Q4_K_M (17 GB loaded) and needs Ollama 0.32.12 or newer:

`ollama pull qwen3.8:27b`
Check the parameters baked into the tag. The line that matters is `draft_num_predict 4`, which makes Ollama draft four tokens ahead with the built-in MTP head:

`ollama show qwen3.8:27b --modelfile | grep PARAMETER`
The tag ships the recommended thinking-mode sampling values:

```
PARAMETER presence_penalty 0
PARAMETER repeat_penalty 1
PARAMETER temperature 1
PARAMETER top_k 20
PARAMETER top_p 0.95
PARAMETER draft_num_predict 4
PARAMETER min_p 0
```
Run a prompt with timing stats. Thinking is on by default at `medium` in Ollama; `--think=false` skips it for a quick answer:

`ollama run qwen3.8:27b --verbose --think=false "In one sentence, what does the Linux OOM killer do?"`
On the 4090 the model loaded in 7.6 seconds and sat at 19.7 GiB of VRAM with Ollama’s default 32K context:

That 32-token answer is too short to measure speed. On 512-token completions the default tag held 95.5 tok/s. Setting `"draft_num_predict": 0` in a request’s options turns MTP off, and speed fell to 47.7 tok/s while VRAM dropped by 1.1 GiB. Leave it on unless you are squeezing for context.

### Give Ollama 131K of context on a 24 GB card

Past 64K, the default F16 KV cache spills layers to the CPU and speed collapses to 16 tok/s. Switching Ollama to a 4-bit KV cache keeps 131K fully on the GPU. Open a systemd override for the service:

`sudo systemctl edit ollama`
Add these lines in the override section:

```
[Service]
Environment="OLLAMA_FLASH_ATTENTION=1"
Environment="OLLAMA_KV_CACHE_TYPE=q4_0"
```
Restart Ollama so the new cache type applies:

`sudo systemctl restart ollama`
Context size is set per request through `num_ctx`. This is the request shape we measured with:

`curl -s http://127.0.0.1:11434/api/generate -d '{"model":"qwen3.8:27b","prompt":"Write a Bash script that rotates Nginx logs older than 7 days.","stream":false,"think":false,"options":{"num_ctx":131072,"num_predict":512}}'`
Here is what each combination did on the 4090. Placement comes from `ollama ps`, speed is the median of three 512-token runs with MTP on:

| KV cache | Context | VRAM used | Placement | tok/s | 
|---|---|---|---|---|
| f16 (default) | 8K | 18.3 GiB | 100% GPU | 95.5 | 
| f16 (default) | 32K | 19.7 GiB | 100% GPU | 103.7 | 
| f16 (default) | 64K | 21.9 GiB | 100% GPU | 96.4 | 
| f16 (default) | 131K | 22.0 GiB | 30% CPU / 70% GPU | 16.4 | 
| q8_0 | 131K | 22.0 GiB | 12% CPU / 88% GPU | 32.6 | 
| q4_0 | 131K | 20.9 GiB | 100% GPU | 101.8 | 
| q4_0 | 262K | 22.1 GiB | 20% CPU / 80% GPU | 21.1 | 

Ollama loads the vision projector and the MTP draft head next to the weights, so at 262K it runs out of room where a text-only llama.cpp setup does not. llama.cpp is the next section. More Ollama tags and their measured speeds are in the Ollama models cheat sheet.

## Run the Qwen3.8-27B GGUF with llama.cpp

llama.cpp gives you control over quant, KV cache type and whether the MTP head loads at all. This catches people out first: the `releases/latest` tag of llama.cpp currently points at `v0.5.0`, which has no binaries attached. The CUDA builds live on the numbered `bNNNNN` releases, so pin one. The CUDA 12.8 build needs an R570 or newer driver. Download the CUDA 12.8 build and its runtime libraries:

```
export LLAMA_TAG=b11200 #https://github.com/ggml-org/llama.cpp/releases
mkdir -p ~/lcpp && cd ~/lcpp
curl -sL -o lcpp.tgz "https://github.com/ggml-org/llama.cpp/releases/download/${LLAMA_TAG}/llama-${LLAMA_TAG}-bin-ubuntu-cuda-12.8-x64.tar.gz"
curl -sL -o cudart.tgz "https://github.com/ggml-org/llama.cpp/releases/download/${LLAMA_TAG}/cudart-llama-${LLAMA_TAG}-bin-ubuntu-cuda-12.8-x64.tar.gz"
tar -xzf lcpp.tgz && tar -xzf cudart.tgz
export LD_LIBRARY_PATH=$HOME/lcpp/llama-${LLAMA_TAG}:$HOME/lcpp/cudart-llama-${LLAMA_TAG}-bin-ubuntu-cuda-12.8-x64
```
Pull the unsloth dynamic 4-bit GGUF and the separate MTP draft file with the Hugging Face CLI (`pip install -U huggingface_hub` provides `hf`):

```
hf download unsloth/Qwen3.8-27B-GGUF Qwen3.8-27B-UD-Q4_K_XL.gguf --local-dir ~/lcpp/gguf
hf download unsloth/Qwen3.8-27B-GGUF --include "MTP/*" --local-dir ~/lcpp/gguf
```
The MTP file is `mtp-Qwen3.8-27B-Q4_0.gguf` at 1.4 GB. Start the server with it for the fastest short-context setup:

```
cd ~/lcpp
./llama-${LLAMA_TAG}/llama-server --host 127.0.0.1 --port 8081 -ngl 99 -fa on -c 8192 --jinja \
  -m gguf/Qwen3.8-27B-UD-Q4_K_XL.gguf \
  --spec-type draft-mtp -md gguf/MTP/mtp-Qwen3.8-27B-Q4_0.gguf -ngld 99
```
With MTP on, the three runs came in at 102.8, 109.4 and 113.2 tok/s. The server’s own timings showed 1,034 of 1,493 drafted tokens accepted (69%). Without the draft file the same quant does 47.0 tok/s.

| GGUF | Context / KV cache | MTP | VRAM used | tok/s | 
|---|---|---|---|---|
| UD-Q4_K_XL | 8K / f16 | off | 17.0 GiB | 47.0 | 
| UD-Q4_K_XL | 8K / f16 | on | 20.0 GiB | 109.4 | 
| UD-Q5_K_XL | 8K / f16 | on | 22.8 GiB | 95.8 | 
| UD-Q6_K | 8K / f16 | on | does not load |  | 
| UD-Q4_K_XL | 64K / f16 | off | 20.6 GiB | 47.0 | 
| UD-Q4_K_XL | 131K / q8_0 | off | 21.3 GiB | 46.6 | 
| UD-Q4_K_XL | 262K / q8_0 | off | does not load |  | 
| UD-Q4_K_XL | 262K / q4_0 | off | 22.2 GiB | 46.5 | 

Raw `llama-bench` generation speed without MTP was 47.9 tok/s for Q4_K_XL, 41.5 for Q5_K_XL and 39.6 for Q6_K, with prompt processing between 2,600 and 3,000 tok/s. Q6_K fits on its own, but not with the draft head beside it.

### Use the full 262K context on a 24 GB GPU

Drop the MTP file and quantize the KV cache to `q4_0`. This is the only combination that opened a 262,144-token context on the 4090:

```
./llama-${LLAMA_TAG}/llama-server --host 127.0.0.1 --port 8081 -ngl 99 -fa on -c 262144 \
  -ctk q4_0 -ctv q4_0 --jinja -m gguf/Qwen3.8-27B-UD-Q4_K_XL.gguf
```
To prove the window is usable and not just allocated, we fed it a 196,971-token log dump with one line buried 60% of the way in (“the deployment passphrase for cluster orion is TANGERINE-4471”) and asked for the passphrase with thinking off. It answered `TANGERINE-4471`. Prefill took 121.5 seconds (1,621 tok/s), generation at that depth ran at 26.6 tok/s over the 9-token answer, and VRAM read 22.2 GiB right after the request. Expect a two-minute wait before the first token on a prompt that size.

So on a 24 GB card you pick one: MTP speed (about 110 tok/s up to 8K) or long context (about 47 tok/s to 262K). Every attempt to combine the draft head with 64K or more either failed to start or crashed on the first request.

### Error: “failed to create MTP context”

This is what llama.cpp prints when the draft head does not fit next to a long context. At 64K with an F16 KV cache the log reads:

```
E alloc_tensor_range: failed to allocate CUDA0 buffer of size 268435456
E llama_init_from_model: failed to initialize the context: failed to allocate buffer for kv cache
E common_speculative_init_result: failed to create MTP context
```
Other combinations fail with different lines for the same reason: Q6_K plus MTP at 8K, and Q4_K_XL at 131K with MTP, print `failed to load draft model`; 262K with MTP prints `failed to allocate compute pp buffers`. The fix is to lower `-c` or remove `--spec-type draft-mtp -md ...`. At 98K with a `q8_0` cache and MTP the server did start at 23.5 GiB, then aborted with `CUDA error: out of memory` on the first request, so a clean startup at the edge of VRAM is not proof it will serve.

## Serve Qwen3.8-27B with vLLM

vLLM is the engine for serving many users from one card. We ran the official `vllm/vllm-openai` image on a 96 GB RTX PRO 6000 Blackwell with the model’s full 262K `--max-model-len`. The NVFP4 checkpoint from NVIDIA is the fastest build on Blackwell. Start it with the qwen3 reasoning parser and MTP:

```
docker run --gpus all --ipc=host -p 8000:8000 \
  -v ~/.cache/huggingface:/root/.cache/huggingface \
  vllm/vllm-openai:v0.30.0 nvidia/Qwen3.8-27B-NVFP4 \
  --served-model-name qwen3.8-27b --max-model-len 262144 --kv-cache-dtype fp8 \
  --reasoning-parser qwen3 --enable-auto-tool-choice --tool-call-parser qwen3_xml \
  --speculative-config '{"method":"mtp","num_speculative_tokens":3}' #https://github.com/vllm-project/vllm/releases
```
With the FP8 KV cache, the startup log reports room for 1.7 million tokens:

`INFO 09-26 17:47:17 [kv_cache_utils.py:2395] GPU KV cache size: 1,722,242 tokens, Maximum concurrency for 262,144 tokens per request: 6.57x`
Keep `--reasoning-parser qwen3`. The chat template opens every reply with a think block, and without the parser the reasoning lands in `message.content`. For FP8 swap the model for `Qwen/Qwen3.8-27B-FP8`, for BF16 use `Qwen/Qwen3.8-27B` and add `--max-num-seqs 256` (explained in the error below). Our benchmark runs used the default KV dtype, one run per build, measured with `vllm bench serve`:

That NVFP4 run shows 8.03 ms per output token and a 53.9% draft acceptance rate on random tokens. The acceptance column below comes from the server counters summed over both the 1-user and 16-user runs. The full matrix, with 1,024 input and 512 output tokens per request:

| Checkpoint | Weights | KV pool at 262K | 1 user tok/s | 16 users tok/s | MTP acceptance | 
|---|---|---|---|---|---|
| BF16 | 51.1 GiB | 528,131 | 26.1 | 340.7 | n/a | 
| BF16 + MTP | 52.0 GiB | 461,618 | 46.3 | 435.1 | 47% | 
| FP8 | 28.5 GiB | 876,375 | 44.4 | 542.0 | n/a | 
| FP8 + MTP | 29.0 GiB | 784,903 | 71.9 | 614.7 | 51% | 
| NVFP4 | 19.9 GiB | 1,012,444 | 60.8 | 711.0 | n/a | 
| NVFP4 + MTP | 20.8 GiB | 904,129 | 114.3 | 707.0 | 54% | 

Two things stand out. MTP adds 60% to 90% for one user, but its value shrinks as the batch fills: at 16 users it added 28% on BF16 and 13% on FP8, and nothing on NVFP4 (711 vs 707 tok/s), while always costing KV space. And NVFP4 is 2.3x faster than BF16 for one user (60.8 vs 26.1 tok/s) while holding almost twice the KV cache. If you run vLLM on Kubernetes, the same flags drop into a ServingRuntime; our KServe vLLM guide shows where they go.

### Error: “max_num_seqs (1024) exceeds available Mamba cache blocks (669)”

The BF16 checkpoint refused to start on the 96 GB card with vLLM’s defaults:

`ValueError: max_num_seqs (1024) exceeds available Mamba cache blocks (669). Each decode sequence requires one Mamba cache block, so CUDA graph capture cannot proceed. Please lower max_num_seqs to at most 669 or increase gpu_memory_utilization.`
BF16 with MTP failed the same way with 585 blocks. Each concurrent sequence needs its own recurrent state for the 48 linear-attention layers, and 51 GiB of weights leaves room for only 669 of them. The fix is `--max-num-seqs 256`, which covers both. With it the server was ready in 155 seconds. FP8 and NVFP4 leave enough memory that the default 1,024 works.

## Thinking mode and reasoning effort

Qwen3.8 thinks by default, and in vLLM the default effort is `xhigh`. You can switch thinking off or lower the effort per request through `chat_template_kwargs`:

```
curl -s localhost:8000/v1/chat/completions -H 'Content-Type: application/json' -d '{
  "model": "qwen3.8-27b",
  "messages": [{"role": "user", "content": "How many prime numbers are there between 100 and 130? Answer with the number."}],
  "max_tokens": 8192,
  "chat_template_kwargs": {"reasoning_effort": "low"}
}'
```
We sent that question three ways on the NVFP4 server. With `{"enable_thinking": false}` the model replied in 0.1 seconds with 13, which is wrong. At `low` effort it reasoned for 516 tokens and answered 6 in 3.0 seconds; at `xhigh` it used 345 tokens and answered 6 in 2.5 seconds. The correct count is six (101, 103, 107, 109, 113, 127). Turn thinking off for chat and extraction, and keep it on for anything with arithmetic or logic in it.

Tool calling works with `--tool-call-parser qwen3_xml`. Given a `run_ssh` tool and “What is the disk usage of /var/log on web01?”, the response came back with `finish_reason` set to `tool_calls` and clean arguments:

`[{"id": "chatcmpl-tool-879f3bbd752a9551", "type": "function", "function": {"name": "run_ssh", "arguments": "{\"host\": \"web01\", \"command\": \"du -sh /var/log\"}"}}]`
That makes the local server a workable backend for coding agents. Our DeepSeek vs Qwen DevOps test shows how the Qwen family handles real infrastructure tasks. Arete-27B, the Nous Research fine-tune of this model that ships without the MTP head, is tested on CPU in our guide to run Arete-27B locally.

## Which Qwen3.8-27B build to run on your GPU

| Your hardware | Run this | What you get | 
|---|---|---|
| 24 GB card, chat and coding | Ollama `qwen3.8:27b` , or llama.cpp UD-Q4_K_XL with the MTP file | 95 to 110 tok/s up to 8K (Ollama holds it to 64K) | 
| 24 GB card, long documents | llama.cpp UD-Q4_K_XL, `-c 262144 -ctk q4_0 -ctv q4_0` , no MTP | Full 262K window at about 47 tok/s | 
| 24 GB card, Ollama only | `OLLAMA_KV_CACHE_TYPE=q4_0` | 131K context at about 100 tok/s | 
| 32 GB Blackwell card | vLLM NVFP4 with `--enforce-eager` and about 32K`--max-model-len` (per the vLLM recipe) | Untested here; short-context serving only | 
| 80 GB and up, team serving | vLLM NVFP4 or FP8, MTP on for latency | 540 to 710 tok/s at 16 users | 

Do not buy a bigger card to run BF16. On the same GPU it was the slowest build we measured and held the least context. Whether your card clears the bar for other models is covered in how much VRAM you need to run an LLM, and the DeepSeek V4 Flash local test is the next step up if 27B is not enough model for the job.
