---
id: collect-261121-veille-ia/modeles-open/mercury-3-and-the-diffusion-llm-bet-what-1-000-tokens-per-second-changes-for-agents-and-what-it-does
title: "Mercury 3 and the Diffusion LLM Bet: What 1,000 Tokens per Second Changes for Agents (and What It Doesn't)"
domain: modeles-open
role: reference
task: article
actors: []
dates: ["2026-10-19"]
keywords: ["mercury", "tokens", "inception", "model", "time", "diffusion", "models", "steps"]
source: https://dev.to/m_t_ramkrushna/mercury-3-and-the-diffusion-llm-bet-what-1000-tokens-per-second-changes-for-agents-and-what-it-51bl
source_anchor: ""
source_lines: [1, 178]
section: "Modeles_Open"
---
Every step of an AI agent waits on the model.

Plan, call a tool, read the result, decide, call another tool. If each model call takes a few seconds, a ten-step task feels like forever, and a voice agent feels broken. Most "make it faster" work today is squeezing the same autoregressive models: smaller tiers, shorter prompts, fewer steps.

On **October 19, 2026**, Inception released **Mercury 3**, a model that attacks the problem from a different direction: it does not generate text one token at a time at all. It is a **diffusion LLM (dLLM)**, and Inception says it now matches speed-tier frontier models on quality while running at more than 1,000 tokens per second.

Here is what actually shipped, how diffusion text generation works, what the numbers do and don't tell you yet, and how I would evaluate it as an engineer.

From Inception's launch post and press release (all figures are vendor-reported):

- 
**Quality:** "on par or better than" GPT-6 Luna and Claude Haiku 5.5 across a range of benchmarks.
- 
**Speed:** 1,068 tokens per second on NVIDIA GPUs, which Inception describes as 4–8x faster than those models.
- 
**Scale:** more than 40x the total parameters of Mercury 1, at comparable speed. The exact size is not disclosed.
- 
**Context:** 1M tokens, up from 260K in Mercury 2.5.
- 
**Price:** $0.20 per 1M input and $0.70 per 1M output tokens, 50% off at launch ($0.10 / $0.35). Cached input tokens are 90% off.
- 
**Access:** OpenAI-compatible API on the Inception platform, plus OpenRouter, Baseten and Vercel AI Gateway. Weights are not released.

It is Inception's fourth release since Mercury 1 in June 2025: Mercury 2 added reasoning, Mercury 2.5 added tunable reasoning and the 260K window, and Mercury 3 is the biggest quality jump so far.

A normal (autoregressive) LLM writes left to right. Token 50 cannot exist until tokens 1–49 do, so output time grows with output length, one forward pass per token.

A diffusion LLM starts from a noisy or fully masked draft of the whole answer and **refines many positions in parallel** over a number of denoising steps.

The open research model **LLaDA** (Nie et al., 2025) is a clean way to picture it:

1. The response starts as a sequence of `[MASK]` tokens.
2. At each step, a Transformer (with no causal mask, so it sees the whole sequence) predicts every masked position at once.
3. The lowest-confidence predictions are re-masked and the rest are kept.
4. Repeat until nothing is masked.

The number of sampling steps is a knob: fewer steps means more tokens decided per forward pass and faster output, at some cost in quality. LLaDA's paper shows exactly that trade-off.

Mercury's own technical report describes the same idea as generating "in a coarse-to-fine way," modifying multiple tokens in parallel. Two details matter for engineers:

- 
**It is still a Transformer.** Diffusion changes the training objective and the sampling algorithm, not the network family. Inception says this lets Mercury reuse existing optimizations, and it serves the model behind an OpenAI-standard API.
- 
**Streaming looks different.** Inception's docs say streaming returns "block-by-block," where a block is a chunk of refined text rather than a single token. There is even a`diffusing: true` mode that streams the whole draft as it is denoised.

The pitch from Inception's chief scientist on Hacker News: because tokens are generated in parallel, "as reasoning models think longer per answer, the speed advantage over autoregressive models of the same size grows."

In a chat UI, a fast model is nice. In an agent, speed compounds:


```
task_time ≈ steps × (model_time + tool_time)
```
If `model_time` dominates, cutting it changes what the product can do, not just how it feels. Inception's early-access partners describe exactly that:

- 
**Real-time search (Octen AI):** in their smart-glasses tests, search took about 80 ms and "model generation was the bottleneck." With Mercury 3 they measured a median generation time of 0.9 s and a median end-to-end response of 2.2 s.
- 
**Web agents (TinyFish AI):** on 50 live Mind2Web navigation tasks in their production harness, Mercury 3 completed 50% of them, using the same token volume and the same number of agent steps as a frontier model, at a fraction of the per-token cost.
- 
**Tutoring (RevisionDojo):** hints, marked answers and definitions come back "in about a second," described as a drop-in swap.

Notice the pattern: these are **latency-bound, high-frequency** jobs. Voice turns, search answers, browser steps, support replies.

This is the part a launch post never leads with, so let's be explicit.

**1. The scores are vendor-reported.** As AICoder's launch coverage notes, they had not been independently reproduced at launch. The comparisons are against speed-tier models (Luna and Haiku 5.5, both at medium effort), not the top frontier tier.

**2. The benchmarks used medium reasoning effort, but the API defaults to low.** Mercury 3 accepts `reasoning_effort` of `instant`, `low`, `medium` or `high`, and the docs say `mercury-3` defaults to `low` if you omit it. A Hacker News commenter also asked why the benchmarks used medium and why there is no low-to-max curve. If you benchmark with defaults, you are not testing what the launch chart tested.

**3. Coding is not uniformly ahead.** The vendor table shows DeepSWE v1 at 46.31 vs Haiku 5.5's 51.62, and an HN commenter asked about exactly that gap. The co-founder's answer is useful: in their daily use the model is "Haiku- or Luna-level," it "won't yet replace Opus or Fable as the primary driver of a harness," but it works well for **subagents**.

**4. Serving economics are not public.** Asked for batch-size vs tokens-per-second figures on specific hardware, Inception said it has been "happy with how performance has scaled with batch size" but did not publish numbers. Tokens per second at batch size 1 and at production load are different questions.

**5. Closed weights, undisclosed size.** You cannot self-host it, and you cannot compare parameter-for-parameter.

None of this makes the release less interesting. It tells you where to point it.

Keep your strongest model as the planner. Put the fast model where the latency is:

- 
**Subagents in an agent harness:** executing well-scoped steps a bigger model planned.
- 
**Voice and live chat turns:** where seconds of silence break the experience. Inception also offers Mercury Voice, tuned for time to first answer token.
- 
**Search agents:** reading many documents and answering while the user is still looking at the screen.
- 
**High-volume classification:** Inception has a separate decision model, Mercury Decide, for that.
- 
**Inline code edits and autocomplete:** via`mercury-edit-2` and the fill-in-the-middle endpoint.

And keep a fallback route. It is one more provider in your stack, so treat it like any other dependency.

Because Mercury 3 is OpenAI-compatible, you can compare it against your current model with the same client and the same prompts. This script measures time to first chunk, total time and output size for any OpenAI-compatible endpoint, with an explicit reasoning effort:


```
import os, time, statistics
from openai import OpenAI
PROMPTS = [
    "Summarize this support ticket in 2 sentences: ...",
    "Given these search results, answer the question with citations: ...",
    # use 20-50 real prompts from your own logs
]
def bench(client, model, extra=None, runs=3):
    ttft, total, chars = [], [], []
    for prompt in PROMPTS:
        for _ in range(runs):
            start = time.perf_counter()
            first = None
            out = []
            stream = client.chat.completions.create(
                model=model,
                messages=[{"role": "user", "content": prompt}],
                max_completion_tokens=500,
                stream=True,
                extra_body=extra or {},
            )
            for chunk in stream:
                if chunk.choices and chunk.choices[0].delta.content:
                    first = first or time.perf_counter()
                    out.append(chunk.choices[0].delta.content)
            end = time.perf_counter()
            ttft.append((first or end) - start)
            total.append(end - start)
            chars.append(len("".join(out)))
    p = lambda xs, q: statistics.quantiles(xs, n=20)[q]  # q=9 -> p50, q=18 -> p95
    return {
        "ttft_p50": round(p(ttft, 9), 3),
        "total_p50": round(p(total, 9), 3),
        "total_p95": round(p(total, 18), 3),
        "avg_chars": int(statistics.mean(chars)),
    }
mercury = OpenAI(api_key=os.environ["INCEPTION_API_KEY"],
                 base_url="https://api.inceptionlabs.ai/v1")
# Benchmark at the effort you will actually ship, not just the default.
for effort in ["low", "medium"]:
    print(effort, bench(mercury, "mercury-3", {"reasoning_effort": effort}))
# Then run the same PROMPTS against your current model and compare.
```
Three rules when you read the output:

- 
**Compare end-to-end, not tokens per second.** Your user waits for the whole task, tools included.
- 
**Look at p95, not just the median.** Agents feel slow at the tail.
- 
**Score quality on the same prompts.** A faster wrong answer is still wrong, so run your existing eval set alongside the timings.

1. 
**Pick one latency-bound step** in your product: a voice turn, a search answer, a subagent action.
2. 
**Record the current baseline:** p50/p95 end-to-end time and your eval pass rate on 30–50 real inputs.
3. 
**Run the harness at matched reasoning effort** for both models.
4. 
**Swap behind a flag or router** , keep the old model as a fallback, and ship to a slice of traffic.
5. 
**Decide on task-level numbers:** time to completed task and pass rate, not the launch chart.

- Mercury 3 is a diffusion LLM: it refines whole drafts in parallel instead of writing one token at a time.
- Inception reports 1,068 tok/s, a 1M context, Luna/Haiku-level quality and launch pricing of $0.10 / $0.35 per 1M tokens. All vendor-reported.
- The win is biggest where latency compounds: agents, voice, search, support.
- Benchmarks used medium effort; the API defaults to low. Test the setting you will ship.
- Use it as a fast subagent or real-time model first, keep a frontier planner, and measure end-to-end on your own prompts.

- Inception, Diffusion at Scale: Introducing Mercury 3 (Oct 19, 2026)
- Business Wire via Yahoo Finance, Inception Launches Mercury 3, Setting a New Pareto Frontier in Speed and Intelligence (Oct 19, 2026)
- Hacker News, Mercury 3 launch thread with Inception's co-founder
- AICoder, Inception launches Mercury 3 diffusion LLM (benchmark table summary, Oct 20, 2026)
- Vercel, Mercury 3 now available on AI Gateway
- Inception docs: Reasoning Efforts, Streaming & Diffusion, Integrations
- Khanna et al., Mercury: Ultra-Fast Language Models Based on Diffusion (2025)
- Nie et al., Large Language Diffusion Models (LLaDA) (2025)

*Where would a 1,000 tok/s model change your product first: agents, voice, search, or nowhere yet?*

## Top comments (0)
