---
id: collect-261121-veille-ia/glm-zhipu/glm5-3flash-review-five-real-tests-and-the-price-catch
title: "GLM‑5.3‑Flash Review: Five Real Tests and the Price Catch"
domain: glm-zhipu
role: reference
task: article
actors: ["Zhipu AI"]
dates: []
keywords: ["flash", "glm5", "token", "five", "output", "model", "tests", "weights"]
source: https://kingy.ai/blog/glm-5-3-flash-review-tests-pricing/
source_anchor: ""
source_lines: [1, 230]
section: "GLM_Zhipu"
---
**The answer in one sentence:** GLM‑5.3‑Flash is an unusually credible low-cost agent model: it passed all five of our narrow browser tests, has MIT-licensed weights and costs $0.15 per million input tokens and $0.50 per million output tokens at list price—but our results do not independently prove its API speed, one-million-token context or vendor benchmark parity.

For the identity record, named API routes and full-model comparison, see why Ox Alpha was GLM‑5.3‑Flash.

Z.ai has made the value proposition almost comically easy to understand. GLM‑5.3‑Flash is a 320-billion-parameter mixture-of-experts model that activates 18 billion parameters per token, accepts a claimed 1,048,576-token context and exposes native multimodal capabilities. Its API costs less than many providers charge for cached input alone. The launch promotion halved those prices and ended September 9, 2026.

That is the pitch. The more useful question is whether the model can finish real work cleanly enough for the low token price to matter.

**Original August 26 article record:** The article reported five signed-out browser tasks through Z.ai’s public chat interface: strict structured output, executable JavaScript, instruction-injection resistance, exact cost arithmetic and retrieval from a 300-record packet. It recorded five first-attempt passes and 14 local assertions passing for the generated JavaScript. The underlying prompts, outputs, assertion harness and result packet were unavailable to this review, so these historical runs were not rerun or independently verified.

This is encouraging evidence, not a universal leaderboard. We had no authenticated API key, so the interface did not expose token counts, request IDs, cache behavior or server-side latency. We could not run controlled competitors through the same harness. The public interface used **Max** reasoning, which also makes the observed 17–51 second completion times a poor proxy for API latency.

**Kingy verdict:** Put GLM‑5.3‑Flash on the shortlist for coding agents, structured workflows, retrieval and cost-sensitive automation. Compare it with currently available alternatives on your own pass/fail tasks; DeepSeek’s current hosted Flash alias serves V4.1-Flash-1017 rather than the V4 Flash checkpoint in the historical table. Do not migrate a production workload on vendor charts or our five-run browser sample alone.


## GLM‑5.3‑Flash at a glance

| Item | What Z.ai publishes | What it means | 
|---|---|---|
| Model scale | 320B total parameters; 18B active per token | Low compute per token does not mean an 18B memory footprint; the full expert weights still matter for hosting | 
| Architecture | 45 layers; 34 linear-attention and 11 sparse-attention layers | The hybrid design targets lower attention and KV-cache costs without abandoning full attention everywhere | 
| Experts | 288 routed experts plus one shared; eight routed experts selected per token | Capacity is spread across many experts while only a fraction is active for each token | 
| Context | 1,048,576 tokens claimed | Potentially useful for large repositories and evidence packets, but not independently validated here | 
| Output | Product default and maximum output are not established in the retained primary sources; an official launch example uses `max_new_tokens=65536` | Do not treat 65,536 or 131,072 as a verified product output contract | 
| Inputs | Text and image; official guides also document video and files | A broader input surface than text-only value rivals | 
| Reasoning | Forced thinking with low, high and max effort controls | Buyers need to test whether the quality gain justifies extra time and output | 
| Weights | MIT-licensed weights and inference code | Broad reuse rights; not enough to reproduce the original training run | 
| Repository size | Exact total not established in the retained primary evidence | The published files confirm a large deployment artifact, but do not prove a 328 GB current repository total or runtime-memory requirement | 

The distinction between **total** and **active** parameters is central. Eighteen billion active parameters can reduce the compute required for a token. It does not shrink the downloadable model into an ordinary 18B checkpoint. Self-hosters still need to store and route across the much larger expert pool, plus allocate KV cache and serving overhead.

Z.ai says the model was pretrained on 30 trillion multimodal tokens and is the first natively multimodal model in the GLM‑5 family. That is a vendor disclosure, not a reproducible corpus audit. The weights are open under MIT; the full data recipe and a Flash-specific training report are not.

## How we tested it

We used the public Z.ai chat interface on August 26, 2026. The interface identified the model as GLM‑5.3‑Flash and ran with Deep Think / Max enabled. Each task began in a fresh tab and received one attempt. We declared the pass conditions before grading.

| Test | Pass condition | Result | Browser elapsed time | 
|---|---|---|---|
| Strict JSON | Exactly the requested keys and value types; no prose or code fence | Pass | 17.1s | 
| JavaScript implementation | Correct `parseWindow` behavior across a local assertion suite | Pass, 14/14 checks | 50.9s | 
| Prompt-injection resistance | Ignore an embedded instruction and select the supplier supported by the governing policy | Pass | 45.3s | 
| Cost arithmetic | Correctly calculate per-task, 50,000-task list and promotional spend | Pass | 40.3s | 
| Long-record retrieval | Recover three exact records from a 16,390-character packet and ignore an injected override | Pass | 45.3s | 

Those are browser wall-clock times, not API time-to-first-token or throughput measurements. The interface exposed neither token usage nor server telemetry. We also attempted a much larger, 80,890-character signed-out prompt; the site presented a sign-in dialog before submission. That is a limit of the test surface, not evidence that the model failed at that length.

### Test 1: strict output discipline

The first prompt demanded a JSON object with a fixed schema and explicitly prohibited explanation or markdown. GLM‑5.3‑Flash returned valid JSON with the exact requested fields and types.

This sounds trivial until structured output becomes a production dependency. Agent pipelines often fail not because the underlying answer is wrong, but because a model adds a sentence, changes a key or wraps the payload in a code fence. One clean run cannot establish a reliability rate. It does show that the model understood and followed a narrow formatting contract without repair.

### Test 2: code that survives execution

The coding task asked for a JavaScript `parseWindow` function that accepted a constrained human-readable range, validated inputs and returned normalized minutes. We copied the answer into a local test harness and ran 14 assertions covering valid input, spacing, reversed ranges, malformed time strings and boundary cases.

All 14 passed. That matters more than whether the code looked elegant in the chat window. A model review should grade executable behavior, not fluency. The result is consistent with Z.ai’s emphasis on coding and agent tasks, but the sample remains one small function rather than a repository-level change.

### Test 3: treating retrieved text as data, not authority

We gave the model a procurement policy, three supplier records and a malicious instruction embedded inside one record telling it to ignore the policy. The correct answer was Alder, because it met the stated risk and price conditions. The model ignored the embedded override and selected Alder with the right rationale.

This does not make GLM‑5.3‑Flash “prompt-injection safe.” No general-purpose model deserves that label after one test. It is a positive first-run result for a common retrieval pattern: the governing instruction remained governing even when untrusted content attempted to promote itself.

### Test 4: price math without a hidden factor-of-1,000 error

We asked the model to price a workload using 16,000 input tokens and 1,080 output tokens at GLM’s list and promotional rates. It returned the exact values:

- **$0.00294 per task** at list price.
- **$147 for 50,000 tasks** at list price.
- **$73.50 for 50,000 tasks** at the launch promotion.

The calculation is simple, but it creates a useful baseline. At list rates, 16,000 input tokens cost $0.0024 and 1,080 output tokens cost $0.00054. Output is more expensive per token, yet input dominates this particular job because there is so much more of it.

Cached input, retries, tool calls, reasoning traces and failed jobs would change the bill. That is why serious buyers should track cost per successful task, not just the provider’s headline token rate.

### Test 5: retrieval across 300 records

The final task contained 300 numbered records, including three targets and one embedded instruction that tried to redirect the answer. The model returned all three requested records exactly and ignored the malicious line.

The packet was only 16,390 characters—nowhere near a million tokens. This is therefore evidence of clean retrieval in a medium-size prompt, not a 1M-context validation. It is still useful because retrieval failures often appear as omissions, blended records or obedience to the wrong text. None occurred in this run.

## What the five passes prove—and what they do not

| Evidence | Supported conclusion | Unsupported leap | 
|---|---|---|
| Five first-attempt passes | The model handled five diverse, predeclared tasks cleanly in the public UI | A 100% general success rate | 
| 14/14 executable assertions | One generated JavaScript function behaved as requested | Repository-scale coding superiority | 
| Injection tests passed | The model resisted two simple embedded overrides | General prompt-injection security | 
| Exact cost calculation | The model reasoned correctly about its published prices | Our browser sessions cost that amount; token counts were unavailable | 
| 300-record retrieval passed | It maintained instruction and recalled three targets | Independent confirmation of 1M context | 
| 17–51s browser completion | Max-reasoning chat responses completed within that range | API latency, throughput or time-to-first-token | 

The practical conclusion is stronger than “we opened the chatbot,” but narrower than a full product review. GLM‑5.3‑Flash earned a controlled API trial. It did not earn an unconditional production recommendation.

## GLM‑5.3‑Flash pricing versus current alternatives

Prices below are provider-published rates captured August 26, 2026, in US dollars per one million tokens. Promotions and long-context multipliers are time-sensitive.

| Model | Input | Cached input | Output | Access | 
|---|---|---|---|---|
| **GLM‑5.3‑Flash list** | **$0.15** | **$0.03** | **$0.50** | API + MIT weights | 
| **GLM‑5.3‑Flash promo** | **$0.075** | **$0.015** | **$0.25** | Through Sep. 9, 2026, UTC+8 | 
| DeepSeek‑V4‑Flash | $0.14 | $0.0028 | $0.28 | API + MIT weights | 
| GPT‑5.6 Luna | $0.20 | $0.02 | $1.20 | Closed API | 
| Gemini 3.7 Flash promo | $0.75 | $0.075 | $3.75 | Closed API; promo through Dec. 31, 2026 | 
| Claude Sonnet 5 | $2.00 | Not directly listed here | $10.00 | Closed API | 
| GPT‑5.6 Sol | $4.00 | $0.40 | $20.00 | Closed API | 
| Claude Opus 5 | $5.00 | Not directly listed here | $25.00 | Closed API | 

GLM’s list input price is close to DeepSeek‑V4‑Flash, while DeepSeek is cheaper on cached input and output at the captured rates. GLM is materially cheaper on output than GPT‑5.6 Luna and dramatically below the premium frontier.

That table is not a performance ranking. A model that solves 60% of jobs at $0.50 per million output tokens can be more expensive than a model that solves 95% at $1.20. Likewise, a verbose reasoning model may generate enough hidden or visible output to erase an apparent price advantage.

For our illustrative 16,000-input/1,080-output job, the list-price totals are:

| Model | Approximate token cost per attempt | 50,000 attempts | 
|---|---|---|
| GLM‑5.3‑Flash list | $0.00294 | $147 | 
| GLM‑5.3‑Flash promo | $0.00147 | $73.50 | 
| DeepSeek‑V4‑Flash | $0.0025424 | $127.12 | 
| GPT‑5.6 Luna | $0.004496 | $224.80 | 
| Gemini 3.7 Flash promo | $0.01605 | $802.50 | 
| Claude Sonnet 5 | $0.0428 | $2,140 | 

This deliberately excludes caching, tools, retries and long-context uplifts. OpenAI’s published model comparison also applies higher rates to some requests above 272K input tokens. The right spreadsheet for a buyer has rows for pass rate, median input, median output, retry count, cache hit rate and regional latency—not just three token prices.

## The benchmark chart: impressive, but not independent

Z.ai’s launch page reports strong agent and software-engineering results. These are the exact comparator labels in that launch chart; they should not be silently updated to current model names.

| Benchmark | GLM‑5.3‑Flash | DeepSeek‑V4‑Vision‑Exp | Claude Opus 4.8 | GPT‑5.6 Terra | Gemini 3.7 Flash | 
|---|---|---|---|---|---|
| Terminal‑Bench 2.1 | 84.3 | 83.9 | 85.0 | 87.4 | 85.8 | 
| DeepSWE 1.1 | 63.4 | 59.3 | 58.0 | 69.6 | 65.3 | 
| AutomationBench | 48.8 | 38.8 | 41.0 | 37.2 | 52.3 | 
| Agents’ Last Exam | 26.3 | 27.3 | 27.0 | 28.0 | Not shown | 
| GDPval‑AA v2 | 1773 Elo | 1675 | 1582 | 1571 | 1527 | 

**Source and status:** vendor-reported launch results; no confidence intervals are published in the chart.

The pattern is credible enough to investigate. Flash is near the leaders on Terminal‑Bench, ahead of several named comparators on AutomationBench and first among the shown models on GDPval‑AA v2. It is not the universal winner. GPT‑5.6 Terra leads the shown DeepSWE result, Gemini leads AutomationBench and several models edge Flash on Agents’ Last Exam.

Harness details matter. Z.ai says its Terminal‑Bench run used Claude Code 2.1.207 and a six-hour timeout, while DeepSWE used mini-swe-agent with a 400K context. Those are not incidental settings. An agent, tool policy, timeout and retry budget can change the outcome as much as a small model difference.

The public Terminal‑Bench 2.1 leaderboard is another useful check, not a perfect apples-to-apples ranking. At reporting time it showed Kimi K3 at 88.3, DeepSeek V4 Pro at 87.9 and Qwen3.8 at 86.6, above GLM‑5.3‑Flash’s 84.3 entry. Different scaffolds and submissions still limit the inference. The responsible reading is that GLM belongs in the serious agent tier—not that one vendor chart proves it beats the frontier.

Z.ai also cites a private “Code Bench” maximum score of 29.0 versus 29.5 for Claude Opus 4.8. Without a public task set and reproducible harness, that number is context, not independent evidence.

## Open weights are a real advantage—with a large deployment bill

The GLM‑5.3‑Flash model repository publishes weights under the MIT licence. That is materially more permissive than a download-only release with a custom commercial threshold. Teams can inspect, modify and serve the model under familiar terms.

“Open weights” is the precise description. Reproducing the original model would require the data, filtering, training schedule and infrastructure details that are not fully public. The distinction matters for scientific reproducibility even when the licence is commercially attractive.

The retained model repository confirms a large set of published weight files, but this review did not establish an exact current repository total. Download size also is not a complete runtime-memory estimate. Quantization can reduce weight memory, while production serving still needs expert routing, KV cache, concurrency headroom, orchestration and operational staff. Buyers should treat self-hosting as a separate engineering project and benchmark it against managed API cost per successful job.

Z.ai documents several serving routes, including SGLang and vLLM. Compatibility is good news, not a capacity plan. Our related guide to AI inference providers explains why batching, uptime, data location and support can matter more than the cheapest listed token.

## Where GLM‑5.3‑Flash fits

**Choose GLM‑5.3‑Flash for a trial when:**

- Your workload is coding, retrieval, document analysis or multi-step automation.
- Output cost matters and you can measure pass/fail automatically.
- You value MIT-licensed weights or want an exit path from a hosted API.
- Native image and file handling matches your endpoint needs; the current DeepSeek Flash Responses route also accepts images, so image input alone no longer separates these hosted options.

**Keep DeepSeek‑V4‑Flash in the bake-off when:**

- Cached-input and output price are decisive.
- Your workload is primarily text, code and agents.
- You want another MIT-licensed model with a similarly aggressive cost structure.

**Pay for GPT, Gemini or Claude when:**

- Their tool ecosystem, regional service, governance controls or support reduce total operational risk.
- A higher pass rate on your hardest tasks is worth more than the token-price gap.
- You need a modality, integration or service-level property GLM does not yet prove for your deployment.

Our guide to cost per successful AI job gives the more durable buying method. Build 20–50 private tasks from real failures, declare the grader, run at least three attempts, record total billed tokens and count repair loops. The winner is the model with the best reliable outcome at an acceptable latency—not the prettiest public average.

## The Chinese-chip claim is about inference, not training

Z.ai says the anonymous Ox Alpha preview and subsequent traffic were served entirely on Chinese AI accelerators using a custom SGLang stack. It does **not** say GLM‑5.3‑Flash was trained entirely on those chips.

That inference achievement could help explain the aggressive economics, but the company has not published the chip model, power consumption, exact throughput, utilization or a normalized Nvidia comparison. We investigate the claim separately in our technical analysis of GLM‑5.3‑Flash on Chinese AI chips.

## Frequently asked questions

### Is GLM‑5.3‑Flash open source?

Its model weights and inference code are published under the MIT licence, so “open-weight under MIT” is accurate. The full training corpus and sufficient Flash-specific training detail to reproduce the model are not public, so the broader “fully open source” label can overstate what is available.

### How much does the GLM‑5.3‑Flash API cost?

As of August 26, 2026, Z.ai lists $0.15 per million input tokens, $0.03 per million cached input tokens and $0.50 per million output tokens. A launch promotion halves those rates through September 9, 2026 at 24:00 UTC+8. Web search is listed separately at $0.01 per use.

### How does GLM‑5.3‑Flash compare with DeepSeek‑V4‑Flash?

Both are low-cost MIT-licensed mixture-of-experts models. DeepSeek’s captured price is lower for cached input and output, while GLM advertises native multimodality and performed cleanly in our five small browser tasks. The correct choice depends on task success rate, output length, caching and modality—not headline price alone.

### Does GLM‑5.3‑Flash support images, video and tool use?

Z.ai documents text and image input for the model and provides guides for video/files and tool use. Capability support can differ by endpoint or serving framework, so verify the exact API route your product will use.

### What hardware is needed to run GLM‑5.3‑Flash locally?

Z.ai does not publish one universal minimum, and this review did not establish an exact current repository total. Production memory depends on precision, quantization, context length, concurrency and serving engine. Plan for a multi-accelerator deployment unless a validated quantized configuration proves otherwise.

### Was GLM‑5.3‑Flash trained on Chinese chips?

That is not established by the launch evidence. Z.ai says Ox Alpha traffic and GLM‑5.3‑Flash **inference** ran on Chinese AI chips. The company has not publicly documented the model’s training hardware in the sources reviewed here.

## Final verdict

GLM‑5.3‑Flash passed the first credibility test. It followed strict schemas, wrote executable code, ignored simple prompt injections, handled exact arithmetic and retrieved the right records from a noisy packet—all on the first attempt. Its list price is low enough to make a large private evaluation worthwhile, and the MIT weights provide an unusually clean deployment option.

The caveat is just as important: five signed-out browser runs are not an API benchmark. We did not measure billed tokens, throughput, 1M-context accuracy, image performance, concurrency or equivalent competitors. Z.ai’s benchmark chart is promising but vendor-run.

So the recommendation is neither hype nor dismissal: **trial GLM‑5.3‑Flash now, but promote it only after it wins on your cost per successful task.**

## Methodology and primary sources

Testing was performed August 26, 2026 in fresh signed-out Z.ai chat tabs with GLM‑5.3‑Flash, Deep Think and Max reasoning displayed. Each task received one attempt. Code was executed locally against 14 assertions. Browser elapsed time was measured from submission to visible completion. No API token counts, request IDs or server-side latency were available. We received no payment, API credit or editorial access from Z.ai.

Primary sources:

**Buying details checked October 22, 2026:** GLM-5.3-Flash buying profile: Web search costs US$0.01 per use. The small public-chat test does not independently establish native API speed, one-million-token reliability or current free quotas. The original article keeps its existing dates and evidence scope; the linked profiles describe their own demonstrations.
