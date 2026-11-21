---
id: collect-261121-veille-ia/qwen/qwen3-8-flash-specs-benchmarks-and-local-setup-guide-diego-rodriguez
title: "Qwen3.8-Flash: Specs, Benchmarks, and Local Setup Guide | Diego Rodriguez"
domain: qwen
role: reference
task: article
actors: ["Alibaba"]
dates: []
keywords: ["qwen3", "flash", "flash-next", "anchor", "official", "guide", "apache", "qwen"]
source: https://www.diego-rodriguez.work/blog/qwen3-2026-open-model-benchmarks-guide
source_anchor: ""
source_lines: [1, 180]
section: "Qwen"
---
Alibaba's Qwen team followed its August 14 Qwen3.8-27B launch with Qwen3.8-Flash-Next (Aug 26): a 125B MoE with 6B active per token that previews the Qwen4 architecture, served managed as Qwen3.8-Flash with 1M context by default. I tested the claims against the official model cards so you don't have to.

In this guide you get the verified Flash spec sheet, the real benchmark deltas versus the 27B anchor, a working quickstart with Ollama and vLLM, the honest limits, and my verdict on who should adopt it — plus a note on the September 2 Max-0902 snapshot.

## 1. Spec Sheet: What Qwen3.8-Flash Actually Is

Qwen3.8-Flash-Next is the open-weight MoE of the Qwen3.8 family, released August 26 2026 on Hugging Face (Qwen/Qwen3.8-Flash-Next, plus an official FP8 variant) and ModelScope. It is natively multimodal (text plus vision) and doubles as an early preview of the Qwen4 architecture. Qwen3.8-Flash is the managed production version built on it — 1M context by default with official built-in tools. The 27B dense model stays in this guide as the self-hostable anchor.

Parameters

125B MoE main model (6B active/token, 512 experts) + 51B n-gram embeddings + 4B MTP

Context

262,144 tokens native, extendable to 1M via YaRN; managed Flash defaults to 1M

Modality

Native multimodal: text + image and video understanding

License

Qwen Community License 1.0 — open weights, not Apache 2.0 (the 27B anchor stays Apache 2.0)

API price

qwen3.8-flash on QwenCloud: $0.16 per 1M input / $0.47 per 1M output

27B anchor

Qwen3.8-27B: 27B dense, Apache 2.0, 262K context — the card to beat below

### License nuance worth knowing

Flash-Next ships under the Qwen Community License 1.0, which carries naming and MaaS clauses — it is not Apache 2.0. The 27B anchor is Apache 2.0, with the file right there in its repo. Treat the two artifacts differently in compliance reviews.

### On pricing

Self-hosting Flash-Next costs you servers, not tokens — the full checkpoint is ~335 GB (FP8 ~173 GB) and the 51B n-gram table is designed to live in host memory with async prefetch. The managed qwen3.8-flash API at $0.16/$0.47 per 1M tokens is the cheap way to try it — always check the console for current promos before budgeting.

## 2. What Improves Versus the 27B Anchor

The official Flash-Next model card scores it against Qwen3.8-27B, Qwen3.7-Plus and Claude Opus 4.6 (Max) on the same harnesses. The pattern is consistent: Flash-Next beats its 27B sibling on every published bench, with the biggest gaps in deep agentic coding and professional office work.

### DeepSWE 1.1

42.2 (Qwen3.8-27B)→ 58.7

Deep agentic coding jumps 16 points — the headline number for agent builders, ahead of the 27B on either harness.

### SWE-bench Pro

61.7 (Qwen3.8-27B)→ 62.5

Real-world software engineering edges past the 27B and beats Claude Opus 4.6 Max at 53.4.

### SWE-bench Multilingual

73.8 (Qwen3.8-27B)→ 81.0

Multilingual repo work gains 7 points and beats Opus 4.6 Max at 77.5.

### CoWorkBench

70.7 (Qwen3.8-27B)→ 73.9

Long-horizon office workflows improve again — relevant if you automate documents and ops.

### JobBench

33.4 (Qwen3.8-27B)→ 55.7

Professional job tasks jump 22 points, far ahead of Qwen3.7-Plus at 27.6 and Opus 4.6 Max at 36.6.

### LiveCodeBench v6

90.3 (Qwen3.8-27B)→ 91.9

Competitive coding reaches 91.9, ahead of the 27B and of DeepSeek-V4-Flash at 90.6.

### And the Max-0902 snapshot?

On September 2 Alibaba cut qwen3.8-max-0902, a dated snapshot of Qwen3.8-Max (alias qwen3.8-max-2026-09-02) at $2 input / $6 output per 1M tokens. It is live on DashScope/QwenCloud, but third-party aggregators had not picked it up yet — treat it as managed-only until your provider lists it. Flash stays the cost-efficient tier at roughly a tenth of the input price.

## 3. Quickstart: Run It Locally in Minutes

Three verified paths, from easiest to most controllable. All commands below come straight from the official Flash-Next card, the repo, and the Ollama library — no invented flags.

### Option A — Ollama (easiest, quantized NVFP4 build)

```
ollama run qwen3.8-flash-next:125b-a6b-nvfp4
# chat via API once it is serving:
curl http://localhost:11434/api/chat \
  -d ’{
    "model": "qwen3.8-flash-next:125b-a6b-nvfp4",
    "messages": [{"role": "user", "content": "Hello!"}]
  }’
```
### Option B — vLLM (production serving, 262K context)

vllm serve Qwen/Qwen3.8-Flash-Next --port 8000 \
  --tensor-parallel-size 4 \
  --max-model-len 262144 \
  --reasoning-parser qwen3 \
  --enable-auto-tool-choice --tool-call-parser qwen3_coder

### Option C — OpenAI-compatible SDK (managed Flash on QwenCloud)

```
from openai import OpenAI
client = OpenAI(
    api_key="sk-xxx",  # or os.getenv("DASHSCOPE_API_KEY")
    base_url="https://dashscope-intl.aliyuncs.com/compatible-mode/v1",
)
completion = client.chat.completions.create(
    model="qwen3.8-flash",
    messages=[{"role": "user", "content": "Refactor this function."}],
    extra_body={"enable_thinking": True},
    reasoning_effort="xhigh",
    stream=True,
)
```
### Sampling parameters that matter

Thinking is on by default at xhigh effort; medium and low trade depth for speed. Keep temperature 1.0, top_p 0.95 for thinking runs. Prefer the FP8 checkpoint (Qwen/Qwen3.8-Flash-Next-FP8) with tensor-parallel 4 for serving.

### Tip: plan memory before you serve

Budget ~335 GB for BF16 or ~173 GB for FP8, plus 51 GB of host memory for the n-gram table offload — TP4 is the recommended serving shape. For a 1M run, extend with the YaRN rope overrides from the card. If that is too much iron, the managed qwen3.8-flash API or the 27B anchor is the sane fallback.

## 4. Honest Limits

No model is free lunch, and the card itself hints at the trade-offs. Here is what to watch before you bet a project on Qwen3.8-Flash.

### 125B needs server iron, not a workstation

The full checkpoint is ~335 GB (FP8 ~173 GB) with TP4 recommended and 51 GB of host memory for the n-gram offload. This is multi-GPU server territory — grab the Ollama quant or the managed API first.

### Community license, not Apache 2.0

Flash-Next carries the Qwen Community License 1.0 with naming and MaaS clauses. If your policy requires Apache 2.0, the 27B anchor is the compliant pick.

### HLE still trails the frontier

Humanity's Last Exam moves 30.8 → 35.9 but stays behind Claude Opus 4.6 Max at 40.0. For pure hard-science reasoning, Flash is better value, not better peak.

### Thinking at xhigh costs latency

Reasoning is on by default at xhigh effort. Long agent chains are powerful but slow and token-hungry — drop to medium or low when you want speed over depth.

### 1M context needs YaRN surgery

Open weights serve 262K natively; beyond that you must apply the rope overrides yourself. Only the managed qwen3.8-flash gives you 1M by default.

### What I would not do

I would not self-host Flash-Next before pricing the managed API against your volume — at $0.16/$0.47 per 1M tokens it is hard to beat for bursty work. And I would not replace your eval harness with vendor tables alone — reproduce DeepSWE and SWE-bench Pro on your own tasks before migrating production agents.

## 5. Verdict: Who Should Adopt It

If you run high-volume coding agents or tool-driven workflows, Qwen3.8-Flash is the most convincing cost-efficiency story of this cycle: 27B-beating scores everywhere, 6B active per token, and managed pricing at roughly a tenth of Max. The 27B anchor stays the pick when you need Apache 2.0 or a single-box setup.

If you only need a cheap chat model or you already serve the 27B happily for Q&A, Flash won't change your life — it buys you agentic depth at server scale, not a smaller footprint. For everyone shipping agents at volume: test the managed API this afternoon, measure it on your repo, and then decide on self-hosting.

### My call

Strong adopt-and-measure for high-volume agent builders (managed first), keep the 27B anchor for Apache-2.0 or single-box needs. Flash earns its place as the default Qwen for cost-aware coding agents in 2026.

## Conclusion

Qwen3.8-Flash packages the two things agent teams actually want: better agentic coding numbers than the 27B at a fraction of Max's price. Keep the Community license terms in mind, size your servers honestly, and let your own benchmarks have the last word.

All numbers above come from the official Qwen sources linked below — no invented scores, no borrowed hype. If you run it locally or via the managed API, I'd love to hear how it behaves on your stack.

### Cheat sheet

- • 125B MoE (6B active) + 51B n-gram table, Community 1.0, 262K native
- • DeepSWE 58.7, SWE-bench Pro 62.5, JobBench 55.7 — all beat the 27B
- • Managed qwen3.8-flash: $0.16/$0.47 per 1M; 27B stays the Apache pick
