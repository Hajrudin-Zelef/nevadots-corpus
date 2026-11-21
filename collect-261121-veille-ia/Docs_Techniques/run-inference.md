---
id: collect-261121-veille-ia/docs-techniques/run-inference
title: "Run inference"
domain: docs-techniques
role: reference
task: article
actors: []
dates: []
keywords: ["nvfp4", "muse", "spark", "open", "sglang", "inference", "http", "checkpoint"]
source: https://dev.meta.ai/docs/muse-spark-open/deploy
source_anchor: ""
source_lines: [1, 407]
section: "Docs_Techniques"
---
Run Muse Spark Open on your own infrastructure or call it through a hosted inference provider. The self-hosted runtimes load the released checkpoints and use the Muse Spark Open chat format described in the prompting guide.

| Runtime | Best for | Checkpoints | Serving API | 
|---|---|---|---|
| SGLang | High-throughput serving for concurrent users | BF16, NVFP4 | OpenAI-compatible HTTP | 
| vLLM | Production serving, high throughput | BF16, NVFP4 | OpenAI-compatible HTTP | 
| TensorRT-LLM | Optimized serving on NVIDIA Blackwell, on a single 4-GPU node | NVFP4 | OpenAI-compatible HTTP | 
| NVIDIA Dynamo | Disaggregated serving and KV-aware routing | NVFP4 | OpenAI-compatible HTTP | 
| Hugging Face Transformers | Native Python implementation for research and custom pipelines | BF16, NVFP4 | Python API | 
| llama.cpp | Portable inference across Metal, CUDA, ROCm, Vulkan, and CPU | ggml-org GGUFs | OpenAI-compatible HTTP | 
| Unsloth | Local inference on macOS, Windows, Linux, and WSL | Unsloth Dynamic GGUFs | OpenAI- and Anthropic-compatible HTTP | 
| MLX | Inference on Apple silicon, across two or four Mac Studio computers | NVFP4 | OpenAI-compatible HTTP | 

To call the model without provisioning GPUs, use a hosted provider.

Muse Spark Open is a mixture-of-experts model that serves across multiple GPUs. NVFP4 serves at TP4 on 4 × B300 and BF16 at TP8 on 8 × B300. Read Choose a checkpoint before you provision hardware.

The commands on this page load each checkpoint by its Hugging Face repository ID, such as `meta-models/Muse-Spark-Open-816B-A42B-NVFP4`, and the runtime downloads the weights from Hugging Face. If you downloaded the weights from Meta, replace the repository ID with the path to your local folder, such as `./muse-spark-open-nvfp4`.

Each tab is a quick start for one runtime: a command that starts a server or, for Transformers, a script that loads the model and generates a reply. Each runtime's section below covers prerequisites, checkpoint variants, and sending requests.

The SGLang and vLLM tabs both serve NVFP4 on 4 × B300; the SGLang tab adds MegaMoE. Each runtime's section below has the other variants.

Serve Muse Spark Open with SGLang(opens in new tab), an inference framework for low-latency, high-throughput serving on anything from a single GPU to a large distributed cluster.

SGLang blog post on Muse Spark Open(opens in new tab): the features SGLang supports for Muse Spark Open at launch.

SGLang cookbook page for Muse Spark Open(opens in new tab): detailed deployment options.

DFlash2 draft model(opens in new tab): a DFlash2 checkpoint trained for speculative decoding with Muse Spark Open.

Run Muse Spark Open with this Docker image:

For host setup, see the SGLang installation documentation(opens in new tab).

To serve NVFP4 checkpoint:

To serve BF16 checkpoint:

For the best prefill performance on the NVFP4 checkpoint, run prefill with sequence parallelism and the MegaMoE kernel (`--enable-layernorm-sp --enable-megamoe-for-layernorm-sp`, which both server commands above already pass). Prefill is sharded across the four ranks and its MoE layers run on MegaMoE; decode stays on the TensorRT-LLM MoE kernel.

To tune it, add `--attention-backend trtllm_mha --page-size 128` to the NVFP4 server command above, and set the MegaMoE token limit before starting it:

`SGLANG_OPT_DEEPGEMM_MEGA_MOE_NUM_MAX_TOKENS_PER_RANK` sets the maximum number of tokens that triggers MegaMoE. Raising it can improve prefill performance at larger batch sizes, but uses more device memory.

The DFlash2 draft model(opens in new tab) proposes a block of eight draft tokens per step, and Muse Spark Open verifies them in a single pass. Both server commands above enable it with `--speculative-algorithm DFLASH` and the three `--speculative-*` flags after it. Remove those four flags to serve without speculative decoding.

HiCache offloads the KV cache to pinned host memory, so evicted prefixes are restored instead of recomputed. Both server commands above enable it with `--enable-hierarchical-cache --hicache-size 200`.

`--hicache-size` is the pinned host memory per tensor-parallel rank, in GB: the NVFP4 command (four ranks) pins 800 GB and the BF16 command (eight ranks) pins 1.6 TB. To size the host tier relative to the device KV pool instead, pass `--hicache-ratio N`. Remove both flags to serve without HiCache.

Prefill runs MegaMoE and sequence parallelism with HiCache. Decode runs the TensorRT-LLM MoE kernel with DFlash2. Run prefill and decode as two servers on four GPUs each, with a router in front that takes client requests:

- **B300:** both servers share one 8-GPU node, prefill on GPUs 0–3 and decode on GPUs 4–7.
- **GB300:** each server takes a full 4-GPU node, so prefill and decode run on separate nodes.

**1. Set the environment for your hardware.** Run the matching block before starting each server.

On B300, in the prefill terminal:

On B300, in the decode terminal:

On GB300, on both the prefill node and the decode node:

**2. Start the prefill server** (MegaMoE, sequence parallelism, and HiCache) on port 30001:

**3. Start the decode server** (TensorRT-LLM MoE kernel and DFlash2) on port 30002, in a second terminal on B300 or on the decode node on GB300:

**4. Start the router** once both servers are up. Replace `<prefill-host>` and `<decode-host>` with addresses the router can reach; on B300, both are `127.0.0.1`.

`8998` after the prefill URL is the prefill server's bootstrap port. Send client requests to the router on port 8000: in the script under Send requests, change the port from 30000 to 8000.

Here is a simple script for sending requests:

For other ways to send requests, such as curl or Python `requests`, see the SGLang tutorial(opens in new tab).

To run Muse Spark Open on AMD GPUs or Intel Xeon, see Hardware platforms. The SGLang cookbook page(opens in new tab) has more deployment options.

For more on SGLang, see the SGLang website(opens in new tab) and the LMSYS blog(opens in new tab).

Serve Muse Spark Open with vLLM(opens in new tab) behind an OpenAI-compatible endpoint.

Prebuilt images are available for both vendors:

- NVIDIA:`vllm/vllm-openai:muse-spark-open`
- AMD:`vllm/vllm-openai-rocm:muse-spark-open`

Use one of the Docker images above to run Muse Spark Open.

NVFP4 on 4 × B300, with DSpark speculative decoding:

For BF16, serve `meta-models/Muse-Spark-Open-816B-A42B` with `--tensor-parallel-size 8` on 8 × B300 and keep the other flags.

`--max-model-len 1048576` serves the full 1,048,576-token context window. KV memory on the full-attention layers grows with context length, so if your workload needs less, lower it as described in Context length.

vLLM exposes an OpenAI-compatible endpoint on port 8000:

Reasoning effort defaults to `xhigh`; set `reasoning_effort` to `low`, `medium`, `high`, or `xhigh`. See Reasoning strength.

Reasoning is returned separately from `content`, and tool calls arrive as a standard `tool_calls` array. Parsing happens server-side, so an OpenAI-compatible harness needs no model-specific handling.

Muse Spark Open needs to stop on `<|eot|>` and the end-of-text token. Never stop on `<|eom|>`, which ends a message while the turn continues. See Stop tokens.

NVIDIA TensorRT-LLM(opens in new tab) serves Muse Spark Open on NVIDIA Blackwell GPUs through an OpenAI-compatible server (`trtllm-serve`). The validated deployment runs the NVFP4 checkpoint on a single 4-GPU Blackwell node (tensor parallel 4, expert parallel 4). The configuration uses an FP8 KV cache, CUDA graphs, chunked prefill, and KV-cache block reuse. The server supports image, video, and audio input, guided decoding (JSON mode and JSON schema), tool calling, and optional speculative decoding.

Muse Spark Open support is in the TensorRT-LLM `feat/musespark-open` branch. Build TensorRT-LLM from that branch; no pre-built release container or wheel includes Muse Spark Open support.

Clone the branch and build from source following the build-from-source guide(opens in new tab). The only differences from that guide are the branch and the Blackwell CUDA architecture (`100-real`):

Then, inside the TensorRT-LLM development container (see the guide for the `devel` container tag and the required `docker run` flags), build and install in editable mode, targeting Blackwell:

You also need a complete Hugging Face-format Muse Spark Open checkpoint with its tokenizer and processor files: meta-models/Muse-Spark-Open-816B-A42B-NVFP4(opens in new tab) (a BF16 variant, meta-models/Muse-Spark-Open-816B-A42B(opens in new tab), is also published). The NVFP4 checkpoint ships the routed-expert weights pre-quantized to NVFP4; no additional quantization step is needed.

Muse Spark Open at TP4/EP4 fits on a single Blackwell 4-GPU node, so `trtllm-serve` launches directly (no multi-node launcher needed). Write the recommended settings (next section) to a YAML file and pass it with `--config`:

Pass `--custom_tokenizer musespark_open`, `--tool_parser musespark`, and `--reasoning_parser musespark` explicitly; the two parsers enable tool-call and reasoning-content extraction on `/v1/chat/completions`. The server listens on `localhost:8000`. Add `--host 0.0.0.0 --port 8000` if it needs to be reachable from other hosts. The server is ready when the log shows `Application startup complete`; the first query may take longer due to warmup.

Start from the validated aggregated serving configuration, `serve_config.yaml`, and tune it for your workload:

Notes:

- **FP8 KV cache is required on GB300 (`SM103`) with chunked prefill or KV reuse.** For this model's head dimension (64), the fused paged-context attention kernel exists only for the FP8 KV-cache dtype; a 16-bit KV cache in that combination is refused at startup.
- `max_seq_len: 49152` is the validated serving window; the model supports up to 1M tokens of context. Longer windows need proportionally more KV-cache memory.
- On out-of-memory errors, reduce`kv_cache_config.free_gpu_memory_fraction` ,`max_batch_size` , or`max_seq_len` .

The server exposes the OpenAI-compatible API:

Muse Spark Open is a reasoning model: with `--reasoning_parser musespark`, each
chat response returns the chain-of-thought in a separate `reasoning_content`
field and the user-facing answer in `content`. Both are drawn from the same
`max_tokens` budget, so set `max_tokens` large enough to cover the reasoning
*and* the answer. Too small a budget is spent entirely on reasoning and returns
an empty `content` with `"finish_reason": "length"`.

Guided decoding (JSON mode and JSON schema via the `xgrammar` backend) works through the standard `response_format` field. The constraint applies to `content` (the answer), so the budget must also leave room for the model to finish reasoning first:

Image, video, and audio inputs are accepted through the standard multimodal chat-completions content parts; multimodal token budgets count against `max_num_tokens` and `max_seq_len` like text tokens.

- The TensorRT-LLM deployment guide for Muse Spark Open(opens in new tab) covers key YAML options, multimodal token budgets, speculative decoding, benchmarking with`tensorrt_llm.serve.scripts.benchmark_serving` , and troubleshooting.

NVIDIA Dynamo(opens in new tab) provides an OpenAI-compatible frontend, reasoning and tool-call parsing, disaggregated serving, and KV-aware routing.

See the Dynamo installation guide(opens in new tab). Use the Muse Spark Open-enabled image and checkpoint specified in the model recipe, including the latest tokenizer, processor, and chat template.

Follow the model recipe for launch commands and backend settings. The Dynamo quickstart(opens in new tab) explains how the frontend and model worker run together.

See the Dynamo recipes(opens in new tab) for recommended deployment settings.

See the request examples in the Dynamo quickstart(opens in new tab) and model recipe. Chat completions separate `reasoning_content` from `content` and return structured `tool_calls` when tools are supplied.

Muse Spark Open comes with a native Hugging Face Transformers implementation, so you can run it using familiar APIs and integrate it into your workflows. It supports text, images, audio, and video, as well as agentic use cases including tool calls. Checkpoints are available in BF16 and NVFP4 formats. You can post-train Muse Spark Open with Transformer Reinforcement Learning (TRL) to adapt it to your own tasks and datasets; see Post-train with TRL.

Install Hugging Face Transformers with:

See the Transformers installation guide(opens in new tab) for more options.

The Muse Spark Open implementation provides `MuseSparkForConditionalGeneration` for text generation from multimodal inputs and `MuseSparkProcessor` for preparing conversations. The processor combines the tokenizer, image and video processors, audio feature extractor, and chat template.

Use a `DistributedConfig` to configure the parallelism settings. `tp_size` is the number of devices across which the dense layers are sharded, and `ep_size` is the number of devices across which the experts are distributed.

The example below shows how to define the distributed config, load the model with `AutoModelForMultimodalLM`, preprocess the inputs, and generate and decode the output.

Save the script above as `run_muse_spark_open.py` and launch it with `torchrun` on every node using Slurm:

NVFP4 quantized weights(opens in new tab) allow running the model with lower memory requirements on compatible hardware. The inference code is the same apart from the model ID, but NVFP4 needs a Transformers build from a pull request branch, installed below.

NVFP4 support requires Transformers from the NVFP4 pull request branch(opens in new tab):

The weights can then be loaded by setting the model name to `"meta-models/Muse-Spark-Open-816B-A42B-NVFP4"` as demonstrated below.

Launch it with `torchrun` and the same Slurm script as Run the model.

See the full Transformers documentation for Muse Spark(opens in new tab) to see how to use the `pipeline` API and run inference for all modalities, including image, video, and audio.

The Hugging Face blog post on Muse Spark Open(opens in new tab) covers the model in depth and more ways to deploy it with Transformers.

Portable inference across Metal, CUDA, ROCm, Vulkan, and CPU. llama.cpp and the Llama app support Muse Spark Open.

Install a prebuilt llama binary for your platform:

`llama serve -hf` downloads, caches, and serves a model in one command. Muse Spark Open supports up to 1,048,576 tokens of context.

To serve a specific quantization, add its suffix. ggml-org ships three quantizations for this release: Q4_K_S, Q8_0, and Q2_K.

The server exposes an OpenAI-compatible endpoint. Open `http://localhost:8080` to chat with Muse Spark Open in the Llama WebUI, which supports images, audio, and Model Context Protocol (MCP) tools.

Useful flags:

| Flag | Why | 
|---|---|
| `-a muse-spark-open` | API alias. By default, this is the checkpoint path. | 
| `--chat-template-kwargs` | Sets `reasoning_effort` , see below. Template default is`xhigh` . | 
| `-np N` | Concurrent slots. The context length is divided across the slots, so long-context agents should use `-np 1` . | 
| `--api-key` | Optional key to gate endpoints. `/health` and`/v1/models` still answer without it. | 

Smoke test (`--noproxy` ensures the loopback request connects directly):

The chat template reads `reasoning_effort` and defaults to `xhigh`.

Set it server-wide with `--chat-template-kwargs`, or per request:

The model supports four levels: `low`, `medium`, `high`, `xhigh`.

Reasoning can't be turned off. The chat template rejects `"reasoning_effort": "none"`, and `--reasoning off` leaves the output unchanged. Set `reasoning_effort` to `low` to spend fewer thinking tokens.

You can upload images through the Web UI. For applications that use the server endpoint, send a remote URL, a `data:image/...;base64,...` URI, or a local path:

Similarly to vision, you can either upload through the Web UI or send base64 encoded files to the endpoint. Audio follows the OpenAI `input_audio` shape: raw base64 in `data`, not a data URL, and the file type (`wav` or `mp3`) in `format`.

You can also use `llama cli` to interact with the model through CLI.

After you launch `llama cli`, use the `/image` command to pass in images. You can then enter a text prompt.

Similarly, to pass audio:

The command above returns finish_reason: `"tool_calls"`, a `tool_calls` array containing `get_weather(city="Paris", units="celsius")`, and reasoning in `reasoning_content`. Tool-call parsing occurs server-side, so OpenAI-compatible clients require no Muse Spark Open–specific logic.

Check `n_ctx_slot` in the startup log to determine the per-generation context limit. Don't read the limit from `-c`: it sets the total context length, shared across slots. When a generation exceeds its slot limit, the server may return neither an answer nor an error, making batch and evaluation failures difficult to diagnose.

To increase concurrency while retaining the same per-slot limit, increase `-c` in proportion to `-np`:

Run Muse Spark Open locally with Unsloth Desktop and Unsloth Dynamic GGUFs. Use the desktop interface to download and chat with the model, or serve an authenticated API for your applications and coding agents. Unsloth runs on macOS, Windows, Linux, and WSL, with CPU and GPU inference powered by llama.cpp.

Muse Spark Open is Meta's 816B-parameter mixture-of-experts model with about 42B active parameters, built for multimodal reasoning, coding, and agentic workflows. It supports text, image, video, and audio input with text output. This recipe covers text chat and API serving; multimodal input depends on the inference backend and the model assets you load.

Start with a Dynamic 2-bit GGUF and choose a variant that fits your hardware. Unsloth's guide gives the following approximate memory planning ranges: system RAM plus VRAM, or unified memory on Apple silicon. Requirements vary with the exact quantization, context length, KV-cache format, and runtime configuration.

| Quantization | Approximate total memory | 
|---|---|
| 2-bit | 300-315 GB | 
| 3-bit | 365-390 GB | 
| 4-bit | 470-500 GB | 
| 6-bit | 670-700 GB | 
| 8-bit | 870-950 GB | 

BF16 language-model weights alone require approximately 1.63 TB before runtime overhead. Leave memory beyond the downloaded model size for the operating system, runtime, and KV cache. Longer contexts increase memory requirements. Offloading layers to system RAM reduces VRAM requirements; if memory pressure causes disk paging, generation can become much slower.

Download Unsloth(opens in new tab) and complete the installer for your platform:

Alternatively, install from a terminal:

Use a recent Unsloth release with Muse Spark Open support. See the platform installation guides(opens in new tab) for additional setup instructions.

In Unsloth Desktop:

1. Launch Unsloth from your installed app shortcut.
2. Open**Model Hub** , search for "Muse Spark Open", and select the Unsloth GGUF repository linked above.
3. Download a quantization that fits your available memory. Start with Dynamic 2-bit; the CLI example below selects`UD-Q2_K_XL` .
4. Load the model from**Select model** on the Chat page and send a short message to confirm that it responds.
5. Open**Settings > API** , create an API key, and copy it. Use the endpoint URL shown in Unsloth when configuring your client.

Unsloth provides automatic per-model inference presets and lets you edit sampling settings, context length, and the chat template. Check the values against the recommended settings below when loading Muse Spark Open.

To load the model and start its API from the command line with the Unsloth CLI:

The `:UD-Q2_K_XL` suffix selects the quantization. Unsloth starts the server, loads the UI, and prints the endpoint URL and API key. Keep this terminal running while sending requests.

This example starts with a 32,768-token context to limit KV-cache memory. Increase `--context-length` as your available memory allows. The `medium` reasoning level is a starting choice for interactive use; change it to another supported level as needed.

For a larger-memory configuration, select `:UD-Q4_K_XL` instead. Additional llama-server runtime flags let you adjust GPU layers, CPU threads, sampling, and networking; see the Unsloth API guide(opens in new tab).

| Setting | Recommended value | 
|---|---|
| Temperature | `1.0` | 
| Top-p | `1.0` | 
| Min-p | `0.0` in the CLI example | 
| Starting quantization | Dynamic 2-bit | 
| Example serving context | `32768` tokens; adjust for available memory | 
| Model maximum context | `1048576` tokens | 
| Supported reasoning effort | `low` ,`medium` ,`high` ,`xhigh` | 

Use `--reasoning-effort` to pass the selected effort level to the model's chat template. The level takes effect only when the loaded template and backend support it. Choose `low` or `medium` for lighter reasoning, and `high` or `xhigh` for more demanding tasks. Muse Spark Open evaluations use `xhigh` for maximum reasoning strength.

The model's maximum context is not a recommended allocation for every machine. Context includes the input and generated tokens, and a larger KV cache needs additional memory. Allow enough output tokens for both reasoning and the final answer.

Unsloth exposes OpenAI-compatible `/v1/chat/completions` and `/v1/responses` endpoints, plus the Anthropic-compatible `/v1/messages` endpoint. Authenticate with your Unsloth API key. The examples below use Chat Completions.

In a second terminal, set the base URL and the API key copied from Unsloth. These shell examples use Bash on macOS, Linux, or WSL, and require Python 3 for model discovery.

Use the URL printed by your server if it differs. The `/v1/models` endpoint lists loaded and locally cached models. Select the loaded Muse Spark Open model's exact ID:

Send a chat request. The Python example needs the OpenAI SDK (`python3 -m pip install -U openai`) and runs in the same environment as the exported client variables:

`min_p` and `enable_tools` are Unsloth extensions to the Chat Completions format. Sending sampling values explicitly also applies them when using the desktop workflow. `enable_tools: false` keeps this request as text generation. For streaming, set `"stream": true` and add `--no-buffer` to curl.

Send an OpenAI-style tool definition to request a structured function call:

Unsloth supports this client-side function-calling format; successful tool use also depends on the loaded model and its chat template. Check for a structured `choices[0].message.tool_calls` array. Your application executes client-defined functions: parse the arguments, run the function, then send the assistant's tool-call message and a `role: "tool"` result with the matching `tool_call_id` in the next request. This request disables Unsloth's built-in server-side tools so your application handles the defined function.

See the curl and HTTP guide(opens in new tab) and Python SDK guide(opens in new tab) for additional endpoint examples.

Install your coding agent if it is not already available, load Muse Spark Open in Unsloth, and connect the agent:

You can also select the model while launching the agent:

On the default local address, passing `--model` starts a temporary server if Unsloth is not already running; that server stops when the agent exits. If Unsloth is already running, the launcher connects to it. Use `--temperature` for the Unsloth agent launcher. See Unsloth Start(opens in new tab) for supported agents and configuration options.

Run Muse Spark Open on Apple silicon with MLX and the NVFP4 checkpoint. The model doesn't fit in one Mac's memory, so serve it across two Mac Studio computers with M5 Ultra and 512 GB of unified memory each, or four with 256 GB each. Four Macs serve fastest.

- **MLX 0.30.1 or later.**
- **A direct Thunderbolt 5 link between every pair of Macs.** Connect each pair with two cables to double the link bandwidth. With four Macs, every Mac connects to each of the other three.
- **Passwordless SSH from the primary Mac to every other Mac.** These instructions call the primary`mac-studio-1.local` and the others`mac-studio-2.local` and so on; adjust the names to match your machines.

Check that the primary can reach each other Mac without a password prompt:

If that prompts for a password, set up key-based access first:

`mlx.launch` starts the program over SSH on every Mac, so install mlx-lm on each one:

Each Mac loads the model from its own disk, so each needs disk space for the full NVFP4 checkpoint.

`mlx.launch` reads which Macs to use from a hostfile. List every Mac in it, two or four. For the `jaccl` backend, each entry also carries the host's Thunderbolt IP addresses and RDMA devices. The MLX distributed documentation(opens in new tab) describes the format, and `mlx.distributed_config` can generate the file. These instructions save it as `hostfile.json` on the primary.

Run this on the primary. It starts `mlx_lm.server` on every Mac in the hostfile and serves an OpenAI-compatible API on port 8080:

Everything after `--` is the program that `mlx.launch` runs on each Mac. `--backend jaccl` selects the RDMA-over-Thunderbolt backend. `--env MLX_METAL_FAST_SYNCH=1` turns on an experimental MLX feature for fast CPU-GPU synchronization, which cuts communication overhead between the CPU, which drives the Thunderbolt link, and the GPU, which does the compute.

To test without a server, run `mlx_lm.generate` through the same launcher. The chat template defaults to `xhigh` reasoning, so leave room for reasoning tokens before the answer, and pass the recommended sampling settings:

Call Muse Spark Open through a hosted provider when you'd rather not provision GPU capacity yourself. These providers serve the open weights:

| Provider | Model string | Get a key | 
|---|---|---|
| Together AI | `meta-models/muse-spark-open` | Together AI API keys(opens in new tab) | 
| Fireworks AI | `accounts/fireworks/models/muse-spark-open` | API keys page(opens in new tab) | 
| Baseten | `meta-models/muse-spark-open` | Baseten app(opens in new tab), Settings → API Keys | 
| Groq | `meta-models/muse-spark-open` | GroqCloud console(opens in new tab), once your organization has access | 
| Nebius Token Factory | `meta-models/muse-spark-open` | Token Factory API keys(opens in new tab) | 

You can also call Muse Spark Open through Modal and OpenRouter.

Pick your provider's tab:

Together AI serves Muse Spark Open on serverless and on-demand dedicated endpoints through an OpenAI-compatible API.

Create an API key(opens in new tab) and export it:

The base URL is `https://api.together.xyz/v1`. Any OpenAI-compatible client works.

Fireworks serves Muse Spark Open on serverless inference. You pay per token and send requests to an OpenAI-compatible endpoint. There is no deployment to provision. The model accepts text and images, supports tool calling, and has a context window of 1,048,576 tokens. You can set how much the model reasons on each request with `reasoning_effort`.

For dedicated capacity, guaranteed throughput, or private networking, run the same model on an on-demand deployment.

3. Export the key:

The base URL is `https://api.fireworks.ai/inference/v1`. Any OpenAI-compatible client works.

With the OpenAI Python SDK:

**Reasoning effort.** Muse Spark Open always reasons. Set `reasoning_effort` to `low`, `medium`, `high`, or `xhigh`. The default is `xhigh`. Higher settings think longer and cost more output tokens. Use `xhigh` for hard reasoning, coding, and agentic tasks. Reasoning is returned in `reasoning_content`, separate from `content`. Thinking cannot be turned off, so `reasoning_effort="none"` is rejected.

**Images.** Pass image URLs or base64 data URLs in the message content.

**Tool calling.** Pass `tools` in the OpenAI function-calling format. Tool calls come back in `message.tool_calls`.

Streaming (`stream=True`) and JSON mode (`response_format={"type": "json_object"}`) work the same way they do for other Fireworks chat models.

- Prompt caching is automatic. Repeated prefixes, such as long system prompts and agent histories, are billed at the cached-input rate.

Baseten serves Muse Spark Open on Model APIs (serverless, pay per token) and Dedicated Inference (single-tenant capacity), with the full 1,048,576-token context window and text, image, video, and audio input.

2. Go to Settings → API Keys and create a key.

The endpoint is OpenAI-compatible. Point any OpenAI client at Baseten:

Image, video, and audio inputs use the standard OpenAI content-part format (`image_url`, `video_url`, `audio_url`). Send media larger than ~74 MB by URL rather than inline base64; video inputs by URL are supported up to 200 MB.

- The video encoder samples at most 96 frames per clip: a 720p clip costs about 144 tokens per second, capped at ~6.9K tokens from about 48 seconds on, so longer clips (up to 300 s) cost no more than that.

Modal serves Muse Spark Open. See Muse Spark Open on Modal(opens in new tab) for setup and request examples, and the Modal blog post on Muse Spark Open(opens in new tab).

OpenRouter routes requests to upstream providers rather than hosting the model, so the provider serving your request depends on your routing configuration. See Muse Spark Open on OpenRouter(opens in new tab) for the model string and setup.

GroqCloud serves Muse Spark Open through an OpenAI-compatible API. Access requires approval for your organization; to request it, contact the Groq team(opens in new tab).

On GroqCloud the model has a 131,072-token context window and supports tool calling, structured outputs, and image input.

Once your organization has access, create an API key in the GroqCloud console(opens in new tab) and export it:

The base URL is `https://api.groq.com/openai/v1`. Use a Groq SDK, or any OpenAI SDK with `base_url` set to that URL.

`reasoning_effort` accepts `low`, `medium`, `high`, or `xhigh`, and defaults to `xhigh`. Lower settings respond faster. See Reasoning strength.

For access questions, contact Groq sales(opens in new tab).

Nebius Token Factory serves Muse Spark Open on a serverless endpoint with an OpenAI-compatible API. You pay per token. To send a request, you need an API key and the model ID below.

The model ID is case-sensitive:

Set your client's base URL to:

4. Create a key and copy it. You can't view it again later.
5. Export the key:

Keep the key out of source code and browser applications. Check that the key can list models:

Look for `meta-models/muse-spark-open` in the response. If it's missing, check model availability for the project that owns your key.

The answer is in `choices[0].message.content`.

With the OpenAI Python SDK:

**Tool calling.** Pass `tools` in the OpenAI function-calling format. This request asks for a `get_weather` call with a `city` argument:

The request doesn't fetch the weather itself. Your application runs the function and sends the result back with the returned call ID. See the Chat Completions reference(opens in new tab) for the `tools` and tool-message fields.

Once your server is up, set the sampling parameters, parallelism, and stop tokens from the recommended settings, and frame your requests with the prompting guide. To fine-tune the model, see Post-training.
