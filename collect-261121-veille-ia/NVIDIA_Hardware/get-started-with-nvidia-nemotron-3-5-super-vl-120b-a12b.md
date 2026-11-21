---
id: collect-261121-veille-ia/nvidia-hardware/get-started-with-nvidia-nemotron-3-5-super-vl-120b-a12b
title: "Get Started with NVIDIA-Nemotron-3.5-Super-VL-120B-A12B#"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: []
keywords: ["reasoning", "nvidia-nemotron-3", "super-vl-120b-a12b", "tool", "model", "profile", "video", "chat"]
source: https://docs.nvidia.com/nim/large-language-models/2.0.13/get-started/advanced/get-started-nemotron-3.5-super-vl.html
source_anchor: ""
source_lines: [1, 589]
section: "NVIDIA_Hardware"
---
# Get Started with NVIDIA-Nemotron-3.5-Super-VL-120B-A12B#

This page is the model-specific guide for NVIDIA-Nemotron-3.5-Super-VL-120B-A12B. For the generic NIM LLM and VLM onboarding path, refer to Quickstart. For full endpoint examples, refer to API Reference. For tool calling and MCP integration, refer to Tool Calling and MCP Integration.

NVIDIA-Nemotron-3.5-Super-VL-120B-A12B is a vision language model built on the Nemotron-H hybrid Mamba-2 and Transformer architecture with a RADIO vision encoder. It accepts text, image, and video input and returns text; it does not accept audio input. Refer to the Support Matrix for NIMs for supported profiles and GPUs.

The inference backend is vLLM 0.31.0.

**What NIM configures for you.** Each packaged profile carries the full set of
engine arguments validated for its GPU and precision: tensor parallelism,
context length (up to 1,048,576 tokens), Mamba cache settings, the
`nemotron_v3` reasoning parser, the `qwen3_coder` tool-call parser, the chat
template, video decoding, and speculative decoding (the built-in MTP head on
all profiles except NVIDIA A100, which runs without speculative decoding). BF16 profiles
also enable LoRA adapter serving. You do not need to pass engine flags through
`NIM_PASSTHROUGH_ARGS`. Overriding `NIM_PASSTHROUGH_ARGS` replaces the packaged
chat-template argument, so only set it when you intend to change engine
behavior.

The token-in/token-out endpoint `/inference/v1/generate` and the render
endpoints `/v1/chat/completions/render` and `/v1/completions/render` are not
served by default and return 404. To enable them, start the container with
`--enable-scale-out` and keep the packaged chat template:

```
 NIM_PASSTHROUGH_ARGS="--enable-scale-out --chat-template /opt/nim/etc/chat_templates/nemotron-3.5-super-vl-reasoning-effort.jinja"
```
## Known Issues#

This release includes the following known issues and limitations:

- **Select the profile explicitly.** Automatic profile selection does not
filter on GPU model or per-GPU memory, so on some systems it can pick a
profile packaged for a different GPU. List the profiles in the container and
set`NIM_MODEL_PROFILE` to the one that matches your GPU, as shown in
How to Choose the Right Model Profile.
- **Video requests can run out of `max_tokens` while reasoning.** With
reasoning on, the model can spend more than 2,048 tokens reasoning about a
video before it writes the answer; the response then ends with`finish_reason: "length"` and empty`content` . Set`max_tokens` to 8192 or
higher for video requests, or turn reasoning off for the request with`"chat_template_kwargs": {"enable_thinking": false}` .
- **`thinking_token_budget` leaves `content` empty when the budget is
reached.** Control reasoning with`reasoning_effort` or`enable_thinking` instead, as described in Control Reasoning.
- **Forced tool choice returns arguments for tools that take none.** When a
toolâs`parameters` declare no properties (`"properties": {}` ) and the
request forces the call with`"tool_choice": "required"` or a named function,
the returned`arguments` can contain invented keys instead of`{}` , and
frameworks that validate arguments reject the call before running the tool.
This comes from vLLM. For tools without parameters, use`"tool_choice": "auto"` , which returns`{}` .
- **`json_object` output can omit the fields you ask for.**`"response_format": {"type": "json_object"}` guarantees valid JSON, not a
particular shape; the model can return`{}` or leave out a requested field.
When the response must contain specific fields, use`"response_format": {"type": "json_schema", ...}` with those fields listed as`required` .

## Prerequisites#

Before deploying a NIM LLM and VLM container, ensure your environment meets the following requirements:

### Hardware Requirements#

The following are the minimum required specifications for supported hardware components:

### Software Requirements#

Minimum required versions for supported software components.

| Requirement | Specification | 
|---|---|
| **Operating System** | Ubuntu 22.04 LTS or later recommended | 
| **Container Toolkit** | 1.14.0 or later | 
| **CUDA SDK** | 12.9 or later | 
| **GPU Driver** | 580 or later | 
| **Docker** | 24.0 or later | 

#### Operating System#

While other Linux distributions can be compatible with NIM, they have not been officially validated.

We recommend using Ubuntu 22.04 LTS or later for the best experience.

#### CUDA SDK#

Install CUDA SDK by following the CUDA installation guide for Linux.

#### GPU Drivers#

Install the NVIDIA GPU drivers by following the NVIDIA Driver Installation Guide.

#### Docker#

Docker is required to run the containerized NIM services.

1. Install Docker Engine for your Linux distribution by following the Docker Engine installation guide.
2. Verify that the Docker daemon is running and that your user can execute `docker` commands without`sudo` . Add your user to the`docker` group if needed:groupadd docker sudo usermod -aG docker $USER
3. Log out and back in for the group change to take effect.

#### Container Toolkit#

The NVIDIA Container Toolkit enables Docker containers to access the host GPU.

1. Install the toolkit by following the NVIDIA Container Toolkit installation guide.
2. Configure Docker to use the NVIDIA runtime by following the Docker configuration steps.
3. Restart the Docker daemon after configuration: systemctl restart docker

### NIM Container Access#

To download and deploy NIM containers, you need one of the following:

- A free NVIDIA Developer Program membership.
- An NVIDIA AI Enterprise license. To request a free 90-day evaluation license, refer to Ways to Get Started With NVIDIA AI Enterprise and Activate Your NVIDIA AI Enterprise License.

### Generate Access Credentials#

An NGC Personal API key is not required to access most NVIDIA NIM containers and
models hosted on NGC. An NGC API key is only required to download Production
Branch (PB) models. This Get Started documentation uses a feature branch model
as an example, so creating an NGC Personal API key is **optional**.

If you want to create an NGC Personal API key, do the following:

1. Generate the Personal API Key on the Setup API Keys page.
2. When creating the Personal API key, select at least **NGC Catalog** from the**Services Included** list. You can also include additional services if you want
to use the same key for other purposes.

Warning

Legacy API keys are not supported by NIM. Always use a Personal API Key.

### Verify NVIDIA Runtime Access#

To ensure that your setup is correct, run the following command:

```
 run --rm --runtime=nvidia --gpus all ubuntu nvidia-smi
```
This command should produce output similar to one of the following, where you can confirm CUDA driver version, and available GPUs.

```
+-----------------------------------------------------------------------------------------+
| NVIDIA-SMI 580.95.05              Driver Version: 580.95.05      CUDA Version: 12.9     |
|-----------------------------------------+------------------------+----------------------+
| GPU  Name                 Persistence-M | Bus-Id          Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |           Memory-Usage | GPU-Util  Compute M. |
|                                         |                        |               MIG M. |
|=========================================+========================+======================|
|   0  NVIDIA H100 80GB HBM3          On  |   00000000:1B:00.0 Off |                    0 |
| N/A   36C    P0            112W /  700W |   78489MiB /  81559MiB |      0%      Default |
|                                         |                        |             Disabled |
+-----------------------------------------+------------------------+----------------------+
+-----------------------------------------------------------------------------------------+
| Processes:                                                                              |
|  GPU   GI   CI        PID   Type   Process name                              GPU Memory |
|        ID   ID                                                               Usage      |
|=========================================================================================|
|  No running processes found                                                             |
+-----------------------------------------------------------------------------------------+
```
## Configuration#

Use environment variables to control authentication and model caching.

### Export NGC API Key#

Note

An NGC API key is only required to download Production Branch (PB) models. You can skip this section if you did not create an NGC API key.

1. Export the variable in your shell (temporary), replacing `<VALUE>` with your actual API key:export NGC_API_KEY=<VALUE>
2. Persist the variable (optional): If using bash: echo "export NGC_API_KEY=$NGC_API_KEY" >> ~/.bashrc If using zsh: echo "export NGC_API_KEY=$NGC_API_KEY" >> ~/.zshrc
3. Verify the variable is set: echo "$NGC_API_KEY"

### Model Cache#

NIM downloads model weights to a cache on the host that you mount into the container. Artifacts persist across restarts, so you do not pull the full model on every run.

#### Local Cache#

An essential variable to configure on your host system is the cache path directory. This directory is mapped from the host machine to container; assets (for example, model weights) are downloaded to this host directory and persist across container restarts. Configuring a local cache is highly recommended, as it avoids re-downloading large model files upon subsequent container restarts. You can name the environment variable containing the path to the local cache whatever you want.

Create the cache directory and export an environment variable:

```
export LOCAL_NIM_CACHE=~/.cache/nim
mkdir -p $LOCAL_NIM_CACHE
# Optionally add sticky bit to avoid issues writing to the cache if the container is running as a different user
chmod -R a+rwxt $LOCAL_NIM_CACHE
```
When you start the NIM container, you must map your host machineâs local cache directory (`$LOCAL_NIM_CACHE`) to the containerâs internal cache path (`/opt/nim/.cache`) using a Docker volume mount, such as `-v "$LOCAL_NIM_CACHE:/opt/nim/.cache"`. This mapping ensures that the large model weights downloaded by the container are saved to your host machine. Because containers are ephemeral, any data stored only inside the container is lost when it stops. By using a volume mount, subsequent container runs detect the existing model files in your local cache and skip the lengthy download process, allowing the NIM to start up faster.

#### Cache Directory Permissions#

The NIM container runs as a non-root user with GID 0 (root group). The cache directory on your host must be writable by GID 0:

```
 chgrp -R 0 "$LOCAL_NIM_CACHE"
sudo chmod -R g+rwX "$LOCAL_NIM_CACHE"
```
Run the container with the cache mounted:

```
 run --gpus all \
  -v "$LOCAL_NIM_CACHE:/opt/nim/.cache" \
  ...
```
To run as a custom user (for example, your host user), pass `-u <uid>:0`:

```
 run --gpus all -u $(id -u):0 \
  -v "$LOCAL_NIM_CACHE:/opt/nim/.cache" \
  ...
```
Important

When using `-u <uid>`, you **must** include `:0` to set GID 0 (for example,
`-u $(id -u):0`). The containerâs writable directories are group-owned by GID 0.
Without it, the container will fail with `PermissionError` when writing to
cache, config, or log paths.

Tip

To make this setting permanent across terminal sessions, you can add `export LOCAL_NIM_CACHE=~/.cache/nim` to your `~/.bashrc` or `~/.zshrc` profile.

## Installation#

Before running a NIM LLM and VLM container, configure your environment and pull the container image. Most current public NIM images support keyless access from NGC, so Docker login is optional unless your image, tag, or model artifact requires credentials.

### Docker Login#

If you created an NGC API key, authenticate with the NVIDIA Container Registry using the following command:

```
echo "$NGC_API_KEY" | docker login nvcr.io --username '$oauthtoken' --password-stdin
```
Use `$oauthtoken` as the username and `NGC_API_KEY` as the password. The `$oauthtoken` username is a special name that indicates that you will authenticate with an API key and not a user name and password.

### Pull the Container Image#

Use the `docker pull` command to fetch the NIM container image.

```
 pull nvcr.io/nim/nvidia/nemotron-3.5-super-vl-120b-a12b:2.0.13-variant-build.1
```
## How to Choose the Right Model Profile#

NIM LLM and VLM selects a model profile based on the detected
hardware, but for this model set `NIM_MODEL_PROFILE` explicitly (refer to
Known Issues). List the profiles packaged in the container,
then pick the profile ID for your GPU, precision, and workload:

```
 run --rm --gpus=all \
  -e NGC_API_KEY=$NGC_API_KEY \
  nvcr.io/nim/nvidia/nemotron-3.5-super-vl-120b-a12b:2.0.13-variant-build.1 \
  list-model-profiles
```
`list-model-profiles` prints each profile ID next to a name that includes the
GPU, precision, and TP size, for example
`vllm-nvidia-gb200-nvfp4-tp1-pp1-throughput-0.0-agentx`. Set
`NIM_MODEL_PROFILE` to either the ID or the name.

Choose the profile by GPU, precision, and tuning objective:

| GPU | NVFP4: GPUs | NVFP4: tuning objectives | BF16 (with LoRA): GPUs | 
|---|---|---|---|
| NVIDIA B200 | 1 | agentX | 2 | 
| NVIDIA B300 | 1 | agentX | 2 | 
| NVIDIA GB200 | 1 | 50k/2k/1k, agentX | 2 | 
| NVIDIA GB300 | 1 | agentX | 2 | 
| NVIDIA H200 | 1 | 50k/2k/1k, agentX | 4 | 
| NVIDIA H100 80GB | 2 | 50k/2k/1k, agentX | 8 | 
| NVIDIA A100 80GB | 2 | General | â | 
| NVIDIA RTX PRO 6000 Blackwell Server Edition | 2 | General | â | 
| NVIDIA DGX Station (GB300) | 1 | General | â | 
| NVIDIA DGX Spark (GB10) | 1 | General | â | 

- **50k/2k/1k** : multi-turn conversations that share a 50K-token system prompt,
with about 2K new input tokens and 1K output tokens per turn.
- **agentX** : multi-turn coding-agent sessions with tool calls and long,
growing contexts. The agentX profiles offload KV cache to CPU memory and use
200 GiB of host memory per GPU (400 GiB for the 2-GPU H100 profile), in
addition to the memory the container otherwise uses.
- **General** : the only profile for that GPU and precision.
- **BF16** : the only profiles that support LoRA adapters.

Each profile runs only on the GPU model and GPU count it lists; there is no generic fallback profile. For the profile IDs, names, and the minimum memory per GPU of each profile, refer to the support matrix.

For more information, refer to Model Profiles.

## Run NIM#

Run the container. Replace `<profile-id>` with the ID you selected in
How to Choose the Right Model Profile.

You can omit the `-e NGC_API_KEY=$NGC_API_KEY` line from `docker run` commands
when the selected image and model artifacts support keyless access.

```
 run --gpus=all \
  -e NGC_API_KEY=$NGC_API_KEY \
  -e NIM_MODEL_PROFILE=<profile-id> \
  -v "$LOCAL_NIM_CACHE:/opt/nim/.cache" \
  -p 8000:8000 \
  nvcr.io/nim/nvidia/nemotron-3.5-super-vl-120b-a12b:2.0.13-variant-build.1
```
Except on DGX Spark, the profiles reserve 10 GiB of GPU memory
(`mm_ipc_gpu_memory_gb`) for decoding image and video input. Media requests
wait for space in that reservation instead of running out of memory. A smaller
value leaves more memory for the KV cache but serializes large or concurrent
video requests sooner; it does not change image or output quality.

## Deploy a Fine-Tuned Checkpoint#

You can serve a fine-tuned checkpoint (for example, after SFT) with this NIM and
keep the packaged profileâs engine configuration, as long as the fine-tuning
changes weights only and keeps the architecture, tokenizer, and precision of
the profile you start from. The workflow materializes a profile into a local
model store, replaces the weights, and serves the store through
`NIM_MODEL_PATH`.

1. Pick the profile for your GPU and precision, as described in How to Choose the Right Model Profile. Use a BF16 profile for a BF16 fine-tune. To serve an NVFP4 profile, quantize the fine-tuned checkpoint to NVFP4 first.
2. Create a model store from that profile: export MODEL_STORE=/path/to/model-store docker run --rm --gpus=all \ -e NGC_API_KEY=$NGC_API_KEY \ -v "$LOCAL_NIM_CACHE:/opt/nim/.cache" \ -v "$MODEL_STORE:/model-store" \ nvcr.io/nim/nvidia/nemotron-3.5-super-vl-120b-a12b:2.0.13-variant-build.1 \ create-model-store -p <profile-id> -m /model-store Besides the checkpoint files, the store contains `runtime_config.json` ,
which holds the profileâs engine arguments (memory utilization, maximum
concurrency, prefix caching, parsers, video settings, speculative decoding,
and so on) and its tensor parallel size, pipeline parallel size, and LoRA
setting. The command behaves the same through the container entrypoint and
as the bare`create-model-store` executable (for example, in Kubernetes or an
interactive shell).
3. Replace the checkpoint files in the store with your fine-tuned files: the `*.safetensors` shards,`model.safetensors.index.json` , and any config or
tokenizer files you changed. Keep`runtime_config.json` .
4. Run the NIM against the store. The engine arguments, tensor parallel size, and pipeline parallel size come from `runtime_config.json` :run --gpus=all \ -e NIM_MODEL_PATH=/model-store \ -v "$MODEL_STORE:/model-store" \ -p 8000:8000 \ nvcr.io/nim/nvidia/nemotron-3.5-super-vl-120b-a12b:2.0.13-variant-build.1 You can also set `NIM_MODEL_PROFILE` to the profile ID you created the store
from; the NIM resolves it to the storeâs configuration.

Notes:

- **Speculative decoding.** The packaged profiles, except NVIDIA A100, use the
checkpointâs built-in MTP head. If your fine-tune does not update or keep the MTP layers, acceptance
drops and speculative decoding can be slower than none. Set`NIM_SPECDEC_ENABLE=false` to serve without it.
- **Accuracy and performance.** NVIDIA validates the packaged checkpoints. A
fine-tuned checkpoint uses the same engine configuration, but its accuracy
and performance are yours to validate.

## License and Governing Terms#

The container and the model weights are governed by separate agreements. Both texts ship inside the container image:

| File | Covers | Governing terms | 
|---|---|---|
| `/opt/nim/LICENSE` | The NIM container | NVIDIA Software License Agreement and Product-Specific Terms for NVIDIA AI Products | 
| `/opt/nim/MODEL_LICENSE` | The model weights | OpenMDW License Agreement, version 1.1 (OpenMDW-1.1) | 

`/opt/nim/NOTICE` carries the copyright notice and the open source archive
reference. To read any of them from the image:

```
 run --rm --entrypoint cat \
  nvcr.io/nim/nvidia/nemotron-3.5-super-vl-120b-a12b:2.0.13-variant-build.1 /opt/nim/LICENSE
```
## Interact with the API#

The NIM exposes OpenAI-compatible and Anthropic-compatible inference endpoints. The OpenAI-compatible endpoints are the following:

- **Chat Completions** :`/v1/chat/completions`
- **Text Completions** :`/v1/completions`
- **Responses** :`/v1/responses`

The Anthropic-compatible endpoints are the following:

- **Messages** :`/v1/messages`
- **Count Tokens** :`/v1/messages/count_tokens`

Reasoning is on by default, and reasoning tokens count toward `max_tokens`. The
shared `/v1/messages` examples in the API Reference use
64- and 256-token budgets, which this model can spend entirely on reasoning
before it writes a visible answer (`stop_reason: "max_tokens"`). With this
model, set `max_tokens` to 4096 or higher for those examples.

The examples on this page use text-only Chat Completions requests. For image and video input with supported vision language models, refer to Image, Audio, and Video Input.

### Find the Model Name#

Replace the `"model"` value in the examples below with the model name served by
your NIM container. To find it, query the models endpoint:

```
 -s http://localhost:8000/v1/models
```
The `id` field in the response is the model name to use in your requests. For model-specific NIMs, this matches the model identifier (for example, `nvidia/nemotron-3.5-super-vl-120b-a12b`).

### Send a Chat Completion Request#

Once the server is running, you can send a request to the chat completion endpoint:

```
 http://localhost:8000/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "nvidia/nemotron-3.5-super-vl-120b-a12b",
    "messages": [
      {
        "role": "user",
        "content": "Hello! How are you?"
      }
    ],
    "max_tokens": 100,
    "chat_template_kwargs": {"enable_thinking": false}
  }'
```
Reasoning is on by default. With reasoning on, the model returns its
chain of thought in the `reasoning` field of the message before the final
answer in `content`, so a small `max_tokens` can end inside the reasoning and
leave `content` empty. This example turns reasoning off with
`chat_template_kwargs`; with reasoning on, set `max_tokens` high enough for both.

The expected response:

```
{
  "id": "chatcmpl-bbf5a3958b820172",
  "object": "chat.completion",
  "created": 1791404342,
  "model": "nvidia/nemotron-3.5-super-vl-120b-a12b",
  "choices": [
    {
      "index": 0,
      "message": {
        "role": "assistant",
        "content": "Hello! I'm doing well, thank you for asking. How can I help you today?",
        "refusal": null,
        "annotations": null,
        "audio": null,
        "function_call": null,
        "reasoning": null
      },
      "logprobs": null,
      "finish_reason": "stop",
      "stop_reason": null
    }
  ],
  "system_fingerprint": "vllm-...",
  "usage": {
    "prompt_tokens": 22,
    "total_tokens": 42,
    "completion_tokens": 20,
    "completion_tokens_details": {
      "reasoning_tokens": 0
    }
  }
}
```
### Control Reasoning#

Reasoning is on by default. Control it per request with these fields of
`/v1/chat/completions`:

| Field | Values | Effect | 
|---|---|---|
| `"chat_template_kwargs": {"enable_thinking": false}` | `true` (default),`false` | `false` turns reasoning off; the model answers directly. | 
| `"reasoning_effort"` | `"low"` ,`"medium"` ,`"high"` | `"low"` and`"medium"` select a shorter, efficient reasoning mode;`"high"` (or omitting the field) selects full reasoning. | 

For example, to request shorter reasoning:

```
 http://localhost:8000/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "nvidia/nemotron-3.5-super-vl-120b-a12b",
    "messages": [{"role": "user", "content": "What time is 3 hours and 40 minutes after 2:45 pm?"}],
    "max_tokens": 4096,
    "reasoning_effort": "low"
  }'
```
Do not use `thinking_token_budget` to cap reasoning length. When the budget is
reached, the model does not finish its answer: generation continues until
`max_tokens` and the response ends with `finish_reason: "length"` and empty
`content`. Use `reasoning_effort` or `enable_thinking` instead.

### Send an Image or Video#

Pass local media inline as a base64 data URL. This example sends a local image and a local video in one request each with the OpenAI Python SDK:

```
import base64
from openai import OpenAI
client = OpenAI(base_url="http://localhost:8000/v1", api_key="not-used")
model = "nvidia/nemotron-3.5-super-vl-120b-a12b"
def data_url(path, mime):
    with open(path, "rb") as f:
        return f"data:{mime};base64," + base64.b64encode(f.read()).decode()
image = client.chat.completions.create(
    model=model,
    messages=[{"role": "user", "content": [
        {"type": "image_url", "image_url": {"url": data_url("image.jpg", "image/jpeg")}},
        {"type": "text", "text": "Describe this image in one sentence."},
    ]}],
    max_tokens=1024,
    extra_body={"chat_template_kwargs": {"enable_thinking": False}},
)
print(image.choices[0].message.content)
video = client.chat.completions.create(
    model=model,
    messages=[{"role": "user", "content": [
        {"type": "video_url", "video_url": {"url": data_url("video.mp4", "video/mp4")}},
        {"type": "text", "text": "Describe what happens in this video."},
    ]}],
    max_tokens=8192,
)
print(video.choices[0].message.content)
```
With reasoning on, give video requests a large `max_tokens` (refer to
Known Issues). To pass a URL instead, put an `https://` URL in
`image_url.url` or `video_url.url`.

### Verify Health Endpoints#

You can verify that the NIM container is running and ready to accept requests by checking its health endpoints. By default, these endpoints are served on port `8000`. If you set `NIM_HEALTH_PORT`, use that port instead.

#### Live Endpoint#

Perform a liveness check to see if the server is running:

```
 -v http://localhost:8000/v1/health/live
```
Example response:

```
GET /v1/health/live HTTP/1.1
Host: localhost:8000
User-Agent: curl/7.81.0
Accept: */*
HTTP/1.1 200 OK
Server: nginx/1.18.0 (Ubuntu)
Content-Type: application/json
Content-Length: 61
Connection: keep-alive
Cache-Control: no-store, no-cache, must-revalidate
{
  "object": "health.response",
  "message": "live",
  "status": "live"
}
```
#### Ready Endpoint#

Perform a readiness check to see if the model is fully loaded and ready for inference:

```
 -v http://localhost:8000/v1/health/ready
```
Example response:

```
GET /v1/health/ready HTTP/1.1
Host: localhost:8000
User-Agent: curl/7.81.0
Accept: */*
HTTP/1.1 200 OK
Server: nginx/1.18.0 (Ubuntu)
Content-Type: application/json
Content-Length: 63
Connection: keep-alive
Cache-Control: no-store, no-cache, must-revalidate
{
  "object": "health.response",
  "message": "ready",
  "status": "ready"
}
```
### Streaming#

To receive responses incrementally as they are generated, you can enable streaming by adding `"stream": true` to your request payload. This is supported across the `/v1/chat/completions`, `/v1/completions`, `/v1/responses`, and `/v1/messages` endpoints.

When streaming is enabled, the API returns a sequence of Server-Sent Events (SSE).

```
 http://localhost:8000/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model":"nvidia/nemotron-3.5-super-vl-120b-a12b",
    "messages": [
      {
        "role": "user",
        "content": "Write a short poem about a robot."
      }
    ],
    "max_tokens": 100,
    "chat_template_kwargs": {"enable_thinking": false},
    "stream": true
  }'
```
The response will be streamed back in chunks, with each chunk containing a `data` JSON object. The stream terminates with a `data: [DONE]` message:
