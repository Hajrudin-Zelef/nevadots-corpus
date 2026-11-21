---
id: collect-261121-veille-ia/deepseek/deepseek-v4-1-flash-1017-update-benchmarks-api-weights
title: "DeepSeek-V4.1-Flash-1017 Update: Benchmarks, API, Weights"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: ["2026-09-10"]
keywords: ["model", "benchmarks", "flash", "deepseek-v4", "card", "flash-1017", "update", "weights"]
source: https://braindetox.kr/en/posts/deepseek_v4_1_flash_1017_update_2026.html
source_anchor: ""
source_lines: [1, 198]
section: "DeepSeek"
---
# DeepSeek-V4.1-Flash-1017 Update: Benchmarks, API, Weights

## A minor update to Septemberâs V4.1-Flash â the conditions behind 11 company benchmarks, the unchanged architecture and config, model names, pricing and peak hours, and checks for self-hosted weights

## What was released â 1017 is a minor update to V4.1-Flash

On its Hugging Face model card, DeepSeek introduced DeepSeek-V4.1-Flash-1017 as a âminor updateâ to DeepSeek-V4.1-Flash. The same card says the model delivers substantial improvements across reasoning, coding, agentic tasks and visual benchmarks, and that it reduces task execution time and output length so users can complete tasks faster and at lower cost.

The release note in the API documentation (2026/10/17) says the model has stronger benchmark performance, faster task completion and shorter outputs than the previous version, and it calls this a Pareto improvement in both performance and cost. The note says the model is already live on DeepSeekâs website, app and API.

- Model name: DeepSeek-V4.1-Flash-1017. The trailing number matches the release date, October 17.
- Base model: DeepSeek-V4.1-Flash. Its Hugging Face repository was created on September 10, 2026.
- Distribution: the release note names two places, Hugging Face and ModelScope.
- License: the model card says both the repository and the model weights are under the MIT License.
- Input type: the task type in the model card metadata is image-text-to-text, meaning it takes images and text and returns text.
- Optimization targets: the release note says the model is optimized for DeepSeek Harness, Tencent CodeBuddy, WorkBuddy and more.

GeekNews also carried an item describing this model as one that âintroducesâ the CED architecture and native image understanding. Both elements, however, were already described on the model card of V4.1-Flash, released on September 10, 2026. The config.json in the 1017 repository has the same contents as the one in the V4.1-Flash repository (a comparison of the two files as of October 17, 2026). The rest of this article therefore splits the topic in two: what changed in 1017 (benchmarks, output length, service rollout), and what had already changed in the V4.1 generation (architecture).

## What changed in 1017 â 11 company-reported benchmarks

The comparison table on the 1017 model card lines up 1017 and V4.1-Flash against K3, GLM-5.3, GPT-5.6 Sol and Opus 5 across 11 benchmarks. The table below carries over only the 1017, V4.1-Flash and Opus 5 columns, plus the difference between the two DeepSeek models (calculated).

| Benchmark | 1017 | V4.1-Flash | Difference (calculated) | Opus 5 | 
|---|---|---|---|---|
| HLE without tools | 42.6 (text 44.0) | 36.8 (text 39.1) | +5.8 (text +4.9) | 56.3 | 
| Terminal-Bench 4.0 | 42.3 | 31.2 | +11.1 | 51.8 | 
| DeepSWE v1.1 | 77.0 | 74.2 | +2.8 | 74.0 | 
| ProgramBench | 29.1 | 20.3 | +8.8 | 37.0 | 
| NL2Repo-Bench | 68.2 | 65.4 | +2.8 | 75.3 | 
| Automation-Bench v1.0.6 | 60.5 | 54.8 | +5.7 | 50.3 | 
| Agentsâ Last Exam | 35.4 | 31.8 | +3.6 | 28.6 | 
| Apex Bench v1.1 | 72.1 | 64.6 | +7.5 | 65.8 | 
| Chartography (with tools) | 83.0 | 78.9 | +4.1 | 84.0 | 
| BabyVision (with tools) | 92.5 | 89.6 | +2.9 | 94.1 | 
| ZeroBench-main (with tools) | 51.0 | 49.0 | +2.0 | 52.0 | 

On all 11 items, 1017 scores higher than V4.1-Flash. The largest gain is on Terminal-Bench 4.0 (11.1 points), followed by ProgramBench (8.8 points) and Apex Bench v1.1 (7.5 points). The smallest is ZeroBench-main (2.0 points).

Compared with Opus 5 in the same table, 1017 is higher on four items â DeepSWE v1.1, Automation-Bench, Agentsâ Last Exam and Apex Bench â while Opus 5 is higher on the other seven. This comparison holds only within the benchmarks DeepSeek chose and the measurement conditions DeepSeek set.

Neither the model card nor the release note gives numbers for the âshorter outputsâ and âfaster task completionâ claims. The release note includes a chart plotting DeepSWE v1.1 Pass@1 against cost per task, measured with the mini-swe-v2 harness, but no values are provided as text.

The release note names one change on the training side. It says 1017 uses reinforcement learning with self-feedback to improve end-to-end project development, from game creation to 3D modeling. The examples include a 3D model of the Forbidden City, a web-based racing game and a Portal 2 recreation built with the Godot engine.

## Conditions behind the numbers â harnesses, reasoning effort and card-to-card differences

A benchmark figure only means something when read together with its measurement conditions. Putting the 1017 card next to the V4.1-Flash card brings out the following conditions.

- The 1017 card says it used the Claude Code harness for Terminal-Bench 4.0 and the Mini-SWE-V2 harness for DeepSWE v1.1.
- The V4.1-Flash card says code agent benchmarks (including Terminal-Bench 2.1/3.0/4.0) were measured with the Minimal mode of DeepSeek Harness and a 1M-token context window.
- Both cards give V4.1-Flash the same Terminal-Bench 4.0 score of 31.2. Whether the V4.1-Flash column in the 1017 table was re-measured with the Claude Code harness is not stated on the card.
- The NL2Repo-Bench value for V4.1-Flash differs between the cards: 64.0 on the V4.1 card and 65.4 on the 1017 card.
- Apex Bench v1.1 appears only on the 1017 card. Its name differs from MathArena Apex (65.6) on the V4.1 card, and neither card explains whether they are the same benchmark.
- The instruct-model results on the V4.1 card use maximum reasoning effort (reasoning_effort=100) with temperature 1.0 and top_p 0.95. The 1017 card does not separately state the reasoning effort or sampling conditions for its comparison table.
- The â mark on HLE denotes the text-only subset. The values inside and outside the parentheses have different denominators.

How much a harness can move a score is shown by the per-harness table on the V4.1 card. The same V4.1-Flash scored 74.2 on DeepSWE v1.1 with mini-SWE and 65.5 with OpenCode, a gap of 8.7 points (calculated). On Terminal-Bench 2.1 it scored 90.6 with DSH Minimal and 84.1 with Codex, a gap of 6.5 points (calculated). That table used 8 samples per task on DeepSWE v1.1 and 3 on Terminal-Bench 2.1, with a maximum of 500 steps per agent and a 1M-token context.

The decision rule is this. If the harness you plan to use is the same as the one used on the card, the cardâs numbers can serve as a starting point. If it differs, suspect the harness first. Taking DeepSWE v1.1 as an example, the gap between 1017 and V4.1-Flash (2.8 points, calculated) is smaller than the spread across harnesses for a single model (8.7 points, calculated). A general order for reading technical reports and benchmark tables is covered in how to read an MoE technical report.

## What did not change â architecture, config, license and prompt format

The config.json in the 1017 repository matches the one in the V4.1-Flash repository in both size (3,311 bytes) and contents. Values such as 40 layers, a maximum position of 1,048,576, 384 routed experts, 1 shared expert and 6 experts per token are unchanged. That the architecture is the same follows from this comparison of the config files; it is not something the model card states in a sentence of its own.

- License: both the repository and the weights are under the MIT License, the same as V4.1-Flash.
- Chat template: no Jinja-format template is included. Instead, the encoding folder holds a Python reference implementation (encoding.py) with test cases. They cover multi-turn conversations, tool calling, thinking mode, numeric reasoning effort, mid-conversation system messages and interleaved image content.
- Production tooling: deepseek-recipe is a set of Rust libraries with Python bindings. It encodes Messages, Chat Completions and Responses API requests into V4 and V4.1 prompts or token IDs, and parses output back. The card says model inference, tool execution and HTTP transport are left to the caller.
- Recommended sampling: the 1017 card recommends temperature 1.0, top_p 1.0, a 1M-token context and max_tokens of at least 256K. The V4.1 card listed top_p as â0.95 or 1.0â.
- Inference reference implementation: the README in the inference folder says it is a readable reference implementation rather than a production serving engine.

The fact that the config file is the same, and that both model cards give the same prompt-encoding guidance (encoding.py and deepseek-recipe) word for word, gives a starting point for comparing the two versions by swapping only the weights while keeping an existing V4.1-Flash serving setup. DeepSeek has not, however, published a separate guide to that swap. Weight sizes and GPU memory arithmetic are covered in the memory math of fitting a large MoE on one GPU.

## What already changed in the V4.1 generation â CED and native image understanding

The architectural changes covered by the second GeekNews item arrived with V4.1-Flash. In its September 10 release note, DeepSeek introduced V4.1-Flash as âthe smallest model in our new architecture family,â with native visual understanding. The table below carries over only values stated on the V4.1-Flash card.

| Item | DeepSeek-V4-Flash | DeepSeek-V4.1-Flash | 
|---|---|---|
| Backbone parameters | 284B | 552B | 
| Activated parameters (per token) | 13B | 8B (prefill) / 16B (decode) | 
| Layer layout | â | 40 layers = 20-layer causal encoder + 20-layer decoder | 
| Global KV cache (per token) | â | 890 bytes, about 1/4 of V4-Flash | 
| Persistent KV cache | â | about 1/8 of V4-Flash | 
| Pre-training | â | 45T tokens, context extended to 1M tokens at 34T tokens | 

The card explains CED (Causal Encoder-Decoder) as follows. A 40-layer Transformer is organized as a 20-layer causal encoder followed by a 20-layer decoder, and the decoderâs global KV cache is projected from the encoderâs final hidden states rather than derived from each decoder layerâs own hidden states. As a result, the card says, the model activates only 8B parameters per token during prefill and 16B during decode, improving cost efficiency for input-heavy agentic workloads. The release note put the same point as â8B active parameters for input, 16B for output.â

On native image understanding, the card says a vision encoder trained from scratch (DeepSeek-ViT, with 2D-RoPE and 3Ã3 pixel-unshuffle downsampling) and a two-layer MLP projector turn images into visual embeddings, which are processed jointly with text embeddings from the start of language-model pre-training. The September release note said V4-Flash and V4-Flash-Vision-Exp had been retired.

The other components the card lists are SWA Bounded Replay, CSA2 (which statically assigns each layer a Full, Reindex or Reuse mode), FP4 main KV caching, Single-Pass mHC, Engram conditional memory (196B parameters) and DSpark speculative decoding. Each MoE layer has 1 shared expert and 384 routed experts, with 6 activated per token, and reasoning effort is set as an integer from 1 to 100. The details of KV cache compression and its relation to pricing are covered in KV cache compression and inference pricing, so they are not repeated here.

## What changes for API users â model names, pricing and time windows

The pricing page (as of October 17, 2026) lists DeepSeek-V4.1-Flash-1017 as the version behind the model name deepseek-flash. The legacy names deepseek-v4-flash and deepseek-v4-flash-vision-exp are still accepted, but the page says the corresponding models have been retired, their requests are served by 1017 and they are billed at the Flash price.

| deepseek-flash, per 1M tokens | Off-peak | Peak | 
|---|---|---|
| Input (cache hit) | $0.003 | $0.006 | 
| Input (cache miss) | $0.15 | $0.3 | 
| Output | $0.6 | $1.2 | 

Off-peak rates are half of peak rates. Peak hours are 01:00â04:00 and 06:00â10:00 UTC, Monday through Friday, excluding Chinese public holidays. In Korean time that is 10:00â13:00 and 15:00â19:00 (calculated). Weekends and Chinese public holidays are off-peak in full.

- The context length is 1M tokens and the maximum output is 384K.
- Both non-thinking and thinking modes are supported, with thinking mode as the default.
- Vision input is supported on deepseek-flash and not supported on deepseek-v4-pro.
- The concurrency limit for deepseek-flash is 2500.
- The base URLs are https://api.deepseek.com for the OpenAI format and https://api.deepseek.com/anthropic for the Anthropic format.
- FIM completion (beta) is available in non-thinking mode only.
- Worked example: 1M cache-miss input tokens plus 1M output tokens cost $1.5 at peak and $0.75 off-peak (calculated).

The release note does not say whether prices changed along with the 1017 release. The pricing page says product prices may vary and that DeepSeek reserves the right to adjust them.

The same release note says the Search and Fetch APIs are now open to everyone. Each endpoint costs $0.75 per 1,000 calls, which the pricing page phrases as $0.75 per 1,000 successful requests. The note says Search and Fetch calls via server-side tools in DeepSeek Harness, Claude Code and Codex are currently free for DeepSeek API users, and that billing starts on October 31, 2026, at 16:00 UTC (01:00 on November 1 in Korean time, calculated).

## Who is affected, and how

The same update means different things depending on how you use the model. The table below groups, by reader, only the changes that have a basis in the source documents.

| Reader | What changes | What to do | 
|---|---|---|
| Developers calling the API as deepseek-flash | Requests under the same name are served by 1017 | Compare output length and cost with earlier records | 
| Code using legacy names (deepseek-v4-flash and others) | 1017 serves them at the Flash price instead of the retired models | Move the model name to deepseek-flash | 
| DeepSeek Harness users | Optimization tuned for 1017 | Upgrade to v0.2.1-rc.1 or later | 
| DeepSeek API users running Search and Fetch through a harness | Billing from October 31, 2026, 16:00 UTC | Tally call volume before billing starts | 
| Operators deploying the weights themselves | Same config file; different weights and recommended top_p | Swap the weights in the existing setup and compare | 
| Decision-makers choosing models by benchmark | Gains on 11 company-reported items | Re-measure with your own harness | 
| Web and app users | Already live, per the release note | â | 

The structure and permission checks of DeepSeek Harness itself are covered in checking the DeepSeek Harness desktop app. The release note recommends upgrading to DeepSeek Harness v0.2.1-rc.1 or later for the best experience with the new model.

## Checks you can run now

If you use the model through the API, these are the checks you can run.

- Search your code and configuration for the model value and see whether deepseek-v4-flash or deepseek-v4-flash-vision-exp remain. They are still accepted, but the name the pricing page tells you to use is deepseek-flash.
- Compare output token counts and elapsed time in logs of the same tasks before and after October 17. The company has not published numbers for the shorter outputs and faster completion it claims, so your own logs are the basis for judgment.
- Batch jobs that are not time-sensitive cost half as much per token if moved outside peak hours (weekdays 10:00â13:00 and 15:00â19:00 Korean time).
- Because thinking mode is the default, decide the setting for calls that need short replies by reading the thinking-mode switching guide that the pricing page points to.
- If you use Search and Fetch as server-side tools in a harness, tally your call volume before billing starts.

If you handle the weights yourself, these are the points to look at.

- A converted checkpoint uses one file per tensor-parallel rank. Conversion uses the --model-parallel and --expert-dtype fp4 options, and the README example uses MP=8.
- The model.py self-test runs one prefill and 22 decode steps with uninitialized weights, checking only shapes and kernel plumbing. It is not a check of numerical accuracy.
- Build prompts with encoding.py or deepseek-recipe. Check first in your serving toolâs configuration that there is no Jinja template.
- Align your settings with the recommended sampling (temperature 1.0, top_p 1.0, max_tokens of at least 256K), compared against your V4.1-era settings (if you used top_p 0.95).

```
python -m pip install -r requirements.txt
                    python model.py
```
Both commands come from the README in the inference folder of the 1017 repository. The first installs the dependencies, and the second is the self-test described above. Because the self-test uses uninitialized weights and checks only shapes and kernel plumbing, not numerics, comparing model quality requires running your own tasks on the actual weights.

## What could not be confirmed

- The size of the reductions in output length and task time. The model card and the release note text give no numbers.
- The reasoning effort and sampling conditions behind the 1017 comparison table. They are not stated on the 1017 card.
- Whether the V4.1-Flash column in the 1017 table was re-measured or carried over from the September card. NL2Repo-Bench values differ between the two cards, and the harness label for Terminal-Bench 4.0 differs.
- The specific method of reinforcement learning with self-feedback. The release note carries only one sentence.
- Whether prices changed at the time of the 1017 release. The 1017 note does not mention prices, and the text of the September note contains no price figures either.
- Details from the technical report (PDF). The article relies only on sentences from the model cards and release notes.
- Whether CED is DeepSeekâs first use of such an architecture. The September note only says V4.1-Flash is the smallest model in a ânew architecture family.â
- Third-party reproductions. All figures in the comparison tables are DeepSeek-reported values.

## References

- DeepSeek-V4.1-Flash-1017 model card â Hugging Face. Minor-update description, 11-benchmark table, recommended sampling, MIT License.
- DeepSeek-V4.1-Flash model card â Hugging Face. CED architecture, vision encoder, KV cache, measurement conditions, per-harness table.
- 1017 inference reference implementation guide â Hugging Face. Conversion options and self-test.
- DeepSeek-V4.1-Flash-1017 release note â DeepSeek API Docs. Rollout scope, Harness version, Search and Fetch billing start date.
- DeepSeek-V4.1-Flash release note â DeepSeek API Docs. New architecture family, handling of legacy model names.
- Models and pricing â DeepSeek API Docs. deepseek-flash version, rates, peak hours.
- GeekNews item (benchmark update) â GeekNews.
- GeekNews item (CED architecture) â GeekNews.

## Frequently asked questions

### Is DeepSeek-V4.1-Flash-1017 architecturally different from V4.1-Flash?

DeepSeek describes 1017 as a minor update to V4.1-Flash. As of October 17, 2026, the config.json files in the two Hugging Face repositories have identical contents. The CED architecture and native image understanding were already part of V4.1-Flash, released on September 10, 2026.

### How much did performance change?

In the comparison table on the 1017 model card, 1017 scores higher than V4.1-Flash on all 11 benchmarks. Terminal-Bench 4.0 goes from 31.2 to 42.3 and ProgramBench from 20.3 to 29.1. These are company-reported numbers, and measurement conditions such as the harness and reasoning effort can move them substantially.

### What should I change if I use the API?

The pricing page lists DeepSeek-V4.1-Flash-1017 as the version behind the model name deepseek-flash. The legacy names deepseek-v4-flash and deepseek-v4-flash-vision-exp are still accepted, but requests are served by 1017 and billed at the Flash price. Switch the model name in your code to deepseek-flash, then compare output length and cost with your earlier records.

### What does deepseek-flash cost on the API?

Per the pricing page as of October 17, 2026, peak prices per 1M tokens are $0.3 for cache-miss input, $0.006 for cache-hit input and $1.2 for output. Off-peak is half that. Peak hours are 01:00â04:00 and 06:00â10:00 UTC on weekdays, which is 10:00â13:00 and 15:00â19:00 in Korea (calculated). Prices may change.

### Can I download the weights and run them myself?

The model card says both the repository and the weights are under the MIT License, and they are on Hugging Face and ModelScope. The inference folder, however, is a reference implementation rather than a production serving engine, and prompts are built with encoding.py or deepseek-recipe instead of a Jinja chat template.
