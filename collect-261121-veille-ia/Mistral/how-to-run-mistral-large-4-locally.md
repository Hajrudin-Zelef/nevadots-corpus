---
id: collect-261121-veille-ia/mistral/how-to-run-mistral-large-4-locally
title: "How to Run Mistral Large 4 Locally"
domain: mistral
role: reference
task: article
actors: ["Mistral AI"]
dates: ["2026-10-29"]
keywords: ["mistral", "about", "large", "model", "weights", "preview", "hugging", "face"]
source: https://dev.to/hassann/how-to-run-mistral-large-4-locally-4afd
source_anchor: ""
source_lines: [1, 189]
section: "Mistral"
---
Mistral Large 4 open weights went up on Hugging Face on October 29, 2026. You get the same 1.05T-parameter model Mistral serves on its API, under a modified MIT license, in FP8 and NVFP4 checkpoints plus community GGUF quants. You can download it without an approval step, serve it on 8 datacenter GPUs with vLLM or SGLang, or skip the hardware and call `mistral-large-4` on the Mistral API for $0.70 per million input tokens.

Below: what changed since the October 6 preview, download sizes, hardware per route and the license. For the model overview, read our Mistral Large 4 launch breakdown; for request formats and SDK code, see the Mistral Large 4 API guide.

The Mistral Large 4 open weights are the downloadable checkpoints of Mistral's flagship model, nicknamed "Le Chonk." It's a multimodal mixture-of-experts model with about 1.05T total parameters, about 49B active per token (52B counting embeddings and output layers), a 1.6B vision encoder and a 1M-token context window, released under the Mistral Ecosystem Open-Weight License.

The official model card lists 72 text layers and 128 routed experts, with 4 picked per token plus 1 shared expert.

The release isn't a frozen copy of the preview. Mistral's open-weight announcement says the model got three more weeks of reinforcement learning focused on coding and cybersecurity before the weights went out.

If you tested Large 4 during the preview, four things are different now.

| Area | October 6 preview | October 29 open release | 
|---|---|---|
| Weights | API only, weights promised later | FP8 and NVFP4 on Hugging Face, ungated | 
| Reasoning modes | `high` or`none` | `low` ,`medium` ,`high` (default`high` );`low` replaces`none` | 
| Terminal-Bench 4.0 (Mistral-reported) | 28% | 41%, with about 30% fewer output tokens | 
| B3 Agent Security attack resistance (Mistral-reported) | 93.3% | 97.1% | 
| API price per 1M tokens | Shown at a preview discount | $0.70 input, $0.07 cached input, $2.10 output | 

The reasoning change breaks code. If your preview integration sends `reasoning_effort: "none"`, switch it to `"low"`: the Hugging Face launch post notes that Transformers runs `none` at `high`, so a stale setting costs tokens instead of failing.

All benchmark numbers in this post are vendor-reported. Mistral also claims 19.7% on Cognition's private FrontierCode 2.0 benchmark, the top open-weight score in its chart, and 48% on the Artificial Analysis Coding Agent Index, roughly tied with GLM-5.3. Independent replication was still pending when we checked on November 2.

You have three routes, and the right one depends on how much hardware you control.

1. 
**Call the Mistral API.** Model id`mistral-large-4` , $0.70 input, $0.07 cached input and $2.10 output per million tokens. No GPUs.
2. 
**Use a hosted provider.** Mistral lists OpenRouter, NVIDIA-hosted endpoints and a NIM container, and Microsoft Foundry, which also offers private deployments on Azure.
3. 
**Self-host the weights.** Download from Hugging Face and serve with vLLM, SGLang, Transformers or llama.cpp on your own machines.

Start on route 1 or 2. Move to route 3 for a concrete reason: data that can't leave your network, a fine-tune you need to own, or volume that keeps an 8-GPU node busy.

The official repos aren't gated (the Hugging Face API reports `gated: false` for both), so any account or anonymous client can pull them. Size is the real gate.

| Repo | Format | Size on disk | 
|---|---|---|
| `mistralai/Mistral-Large-4-1T-A52B` | FP8, official | about 1,061 GB | 
| `mistralai/Mistral-Large-4-1T-A52B-NVFP4` | NVFP4, official | about 610 GB | 
| `ggml-org/Mistral-Large-4-1T-A52B-GGUF` | Q8_0 | about 1,115 GB | 
| `ggml-org/Mistral-Large-4-1T-A52B-GGUF` | Q4_K_M | about 635 GB | 
| `ggml-org/Mistral-Large-4-1T-A52B-GGUF` | Q2_K | about 381 GB | 
| `ggml-org/Mistral-Large-4-1T-A52B-GGUF` | IQ2_XXS | about 298 GB | 

We summed these from each repo's file listing on November 2, 2026. Unsloth also publishes GGUF quants under `unsloth/Mistral-Large-4-1T-A52B-GGUF`.

Download only the files you need with the Hugging Face CLI:


```
pip install -U huggingface_hub
hf download mistralai/Mistral-Large-4-1T-A52B-NVFP4 \
  --local-dir ./mistral-large-4-nvfp4
```
The FP8 download takes a little over two hours at a sustained 1 Gbps.

Every official serving example assumes one node with 8 GPUs and tensor parallelism of 8. Here's what each engine expects.

| Engine | Checkpoint | Hardware in the official examples | 
|---|---|---|
| vLLM | FP8 | 8 GPUs, `--tensor-parallel-size 8` ; Hugging Face Inference Endpoints use 8x H200 | 
| vLLM | NVFP4 | 8 NVIDIA Blackwell GPUs | 
| SGLang | FP8 | 8 GPUs, `--tp-size 8` | 
| Transformers | FP8 | 8 GPUs via `torchrun` | 
| llama.cpp | Q2_K GGUF | One Apple Silicon Ultra machine with 512 GB unified memory | 

The model card recommends vLLM and ships a dedicated Docker image:


```
docker run --runtime nvidia --gpus all \
  -v ~/.cache/huggingface:~/.cache/huggingface \
  -p 8000:8000 \
  --ipc=host \
  vllm/vllm-openai:mistral-large-4 \
  --model mistralai/Mistral-Large-4-1T-A52B \
  --tensor-parallel-size 8 \
  --tool-call-parser mistral \
  --reasoning-parser mistral \
  --enable-auto-tool-choice \
  --gpu-memory-utilization 0.85
```
Keep both parser flags, or thinking text lands in `content` and tool calls arrive as raw text. For faster decoding, the card documents an optional DSpark speculative decoder: add `--speculative-config '{"method": "dspark", "num_speculative_tokens": 3}'`.

On 8x H200 with that drafter, Hugging Face measured about 65 tokens per second for one request and about 480 tokens per second total across 1,000 ShareGPT conversations under load.

SGLang works too: the model card's `sglang serve` example uses `--tp-size 8`, `--speculative-algorithm DSPARK` and `auto` reasoning and tool-call parsers.

The llama.cpp team shipped day-0 support. The Q2_K quant (381 GB) fits on one Apple Silicon Ultra machine with 512 GB of unified memory:


```
curl -LsSf https://llama.app/install.sh | sh
llama serve -hf ggml-org/Mistral-Large-4-1T-A52B-GGUF:Q2_K
```
Hugging Face reports about 90 tokens per second of prefill and 15 of generation on one M3 Ultra: fine for private experiments, slow for an agent loop. A 2-bit quant also loses quality, so test it on your own tasks first.

Mistral's recommended sampling is `temperature=1.0` and `top_p=0.95` at every reasoning level.

vLLM and SGLang both expose an OpenAI-compatible server, so the request looks the same as any chat completion:


```
curl http://localhost:8000/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "mistralai/Mistral-Large-4-1T-A52B",
    "messages": [{"role": "user", "content": "Summarize RFC 9110 section 9 in 5 bullets."}],
    "reasoning_effort": "medium",
    "temperature": 1.0,
    "top_p": 0.95
  }'
```
On the hosted API, send the same body to `https://api.mistral.ai/v1/chat/completions` with `"model": "mistral-large-4"` and a Bearer [REDACTED]. The vLLM OpenAI-compatible server docs list the fields it accepts.

A self-hosted model fails quietly: a missing parser flag leaks reasoning into `content`, or a tool call comes back as plain text. Let your coding agent check both routes with Apidog CLI instead of pasting curl commands by hand.

Set it up once:


```
npm install -g apidog-cli@latest
apidog login --with-token <YOUR_TOKEN>
npx -y skills add https://apidog.com   # pick Claude Code when prompted
```
Then give Claude Code one [REDACTED PROMPT]

In Apidog project , create two environments: "ml4-self-hosted" with base URL

http://localhost:8000 and "ml4-mistral-api" with base URL https://api.mistral.ai.

Add a POST /v1/chat/completions endpoint and a test case that sends a tool-calling

request with reasoning_effort "low", then asserts status 200, that choices[0].message

has a tool_calls array, and that message.content holds no thinking text.

Run it against both environments and tell me where they differ.


```
The agent reads each schema, validates its payload and runs the case:
```

bash

apidog environment create ml4-self-hosted --project  --base-url http://localhost:8000

apidog environment create ml4-mistral-api --project  --base-url https://api.mistral.ai

apidog cli-schema get endpoint-create

apidog cli-schema get test-case-create

apidog cli-schema validate test-case-create --file case.json

apidog test-case create --project  --file case.json

apidog run --test-case  --project  -e  -r cli,json

apidog run --test-case  --project  -e  -r cli,json

```
Add `--upload-report` to view the run in the Apidog app, or put the same `apidog run ... -r junit` line in CI so a vLLM upgrade can't ship with broken tool calls. Our [Apidog CLI in Claude Code guide](http://apidog.com/blog/apidog-cli-in-claude-code?utm_source=dev.to&utm_medium=wanda&utm_content=n8n-post-automation) covers the setup in more detail. Apidog's Free plan covers up to 4 users.
## The license: what you can and can't do
The [Mistral Ecosystem Open-Weight License](https://huggingface.co/mistralai/Mistral-Large-4-1T-A52B/blob/main/LICENSE) is MIT with one added condition.
**You can:** run, deploy, fine-tune and modify the model and its derivatives, for commercial and non-commercial use, as long as you keep the copyright and permission notice in copies.
**You need a separate commercial license from Mistral if:** you run it as a "Model as a Service," which the license defines as giving a third party access to the model or its derivatives for inference or post-training.
**That condition doesn't apply to:** internal use, end-user products where the model sits inside specific features or harnesses, simply relaying requests to a model someone else hosts, and use through Mistral's own products.
An internal coding assistant or a product feature built on the model is fine; reselling raw inference needs a deal. This is a summary, not legal advice.
For a similar walkthrough on another open-weight model, see our [GLM-5.3 self-hosting guide](http://apidog.com/blog/self-host-glm-5-3-open-weights?utm_source=dev.to&utm_medium=wanda&utm_content=n8n-post-automation).
## Should you self-host or call the API?
Call the API when your volume is modest, you want the 1M context without tuning server limits, or you don't already run 8-GPU nodes. At $0.70 input and $2.10 output per million tokens, 100 million input tokens plus 20 million output tokens a month comes to $112, before any caching discount.
Self-host when your data can't leave your network, when you want to own a fine-tune (Hugging Face shows LoRA training with TRL on an 8x B300 node), or when steady volume keeps a node busy. Mistral's Forge service is the managed option for tuning on your own data.
## FAQ
### Are the Mistral Large 4 open weights free to download?
Yes. Both official repos on Hugging Face are ungated, so you can download them without requesting access. Running them is the cost: the official FP8 checkpoint is about 1,061 GB and the serving examples use 8 datacenter GPUs.
### Can I run Mistral Large 4 on a single GPU?
No official recipe covers one GPU. Every vLLM, SGLang and Transformers example uses 8 GPUs with tensor parallelism. The smallest documented setup is llama.cpp with the 381 GB Q2_K quant on one Apple Silicon Ultra machine with 512 GB of unified memory.
### Is Mistral Large 4 open source?
It's open weight, not open source in the OSI sense. You get the weights under a modified MIT license that allows commercial use, but offering the model to third parties as an inference or post-training service needs a separate commercial license from Mistral.
### Is the open-weight model the same as the API model?
Mistral says the released checkpoint got three more weeks of reinforcement learning after the October 6 preview, and the API serves the updated model as `mistral-large-4`. The reasoning modes changed too: use `low`, `medium` or `high`.
### How much does the Mistral Large 4 API cost?
Mistral lists $0.70 per million input tokens, $0.07 per million cached input tokens and $2.10 per million output tokens, as of November 2, 2026.
```
## Top comments (0)
