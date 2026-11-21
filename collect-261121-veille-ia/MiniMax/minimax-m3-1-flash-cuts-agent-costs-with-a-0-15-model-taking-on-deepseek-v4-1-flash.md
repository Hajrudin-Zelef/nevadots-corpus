---
id: collect-261121-veille-ia/minimax/minimax-m3-1-flash-cuts-agent-costs-with-a-0-15-model-taking-on-deepseek-v4-1-flash
title: "MiniMax M3.1 Flash Cuts Agent Costs With a $0.15 Model, Taking On DeepSeek V4.1 Flash"
domain: minimax
role: reference
task: article
actors: ["MiniMax"]
dates: ["2026-10-19"]
keywords: ["flash", "minimax", "model", "minimaxs", "deepseek", "about", "cost", "tokens"]
source: https://foundermag.co/launches/minimax-m3-1-flash/
source_anchor: ""
source_lines: [1, 58]
section: "MiniMax"
---
## What launched

On October 19, 2026 Shanghai’s MiniMax released **MiniMax M3.1 Flash**, a faster, cheaper follow-up to the M3 model it launched in June. MiniMax pitches it at agentic work, coding, computer use and cybersecurity. It claims online inference is nearly three times faster and that agentic tasks cost 70% less to run than on M3. In everyday use, MiniMax says output reaches 200+ tokens per second, with a median above 170. It credits the speed-up to “more aggressive sparsity.”

The launch also solved a small mystery. MiniMax confirmed that **Space Bunny**, an anonymous model that had been running on OpenRouter since late September, was an early M3.1 Flash build. MiniMax says it processed more than 200 trillion tokens before the reveal.

According to MiniMax’s developer docs, M3.1 Flash is multimodal, has a 1M-token context window and lets you tune how much it thinks. SiliconANGLE reports that reasoning is always on, with five effort levels from Low to Max, and that the model has about 428 billion parameters, roughly the size of M3. MiniMax’s own announcement doesn’t give a parameter count. The model supports interleaved thinking, meaning it reasons between tool calls. MiniMax’s docs warn that agent harnesses must send the model’s full previous response, including its thinking, back on every turn.

## Pricing

MiniMax charges a flat rate per million tokens:

|  | M3.1 Flash | M3 (≤512K input) | 
|---|---|---|
| Input | $0.15 | $0.30 | 
| Output | $0.60 | $1.20 | 
| Cached input | $0.015 | $0.06 | 

That is half of M3’s per-token price. MiniMax’s bigger 70% saving also counts the fact that M3.1 Flash uses fewer tokens per task. Artificial Analysis found the same pattern. On its Intelligence Index runs, M3.1 Flash used about 189 million output tokens and averaged about $0.22 per task, against about $0.51 for M3.

The comparison with DeepSeek is close. DeepSeek’s `deepseek-flash` (V4.1 Flash 1017) costs $0.30 input and $1.20 output at peak hours and exactly half that off-peak. So MiniMax’s all-day price equals DeepSeek’s off-peak price, with no time windows to schedule around. DeepSeek is still cheaper on cache hits ($0.006 at peak against MiniMax’s $0.015).

## How it compares

MiniMax published a benchmark table comparing M3.1 Flash with DeepSeek’s two V4.1 Flash builds, Moonshot’s Kimi K3, Zhipu’s GLM-5.3, GPT-5.6 Sol and Claude Opus 5. All of these are MiniMax’s own numbers, run in harnesses it picked:

| Benchmark | M3.1 Flash | DeepSeek V4.1 Flash 1017 | Kimi K3 | Claude Opus 5 | 
|---|---|---|---|---|
| Automation-Bench v1.0.6 | 62.2 | 60.5 | 46.7 | 50.3 | 
| BankerToolBench | 76.2 | – | 69.1 | 76.2 | 
| Terminal-Bench 4.0 | 38.1 | 42.3 | 12.6 | 51.8 | 
| DeepSWE v1.1 | 73.7 | 77.0 | 67.5 | 74.0 | 
| NL2Repo-Bench | 61.2 | 68.2 | 58.0 | 75.3 | 
| OSWorld 2.0 | 61.7 | – | 58.3 | 70.6 | 
| CyberGym | 82.2 | – | 80.0 | – | 
| HLE (no tools) | 43.4 | 42.6 | 43.5 | 55.6 | 

Look past the headline and the picture is mixed. M3.1 Flash leads on business-workflow automation (Automation-Bench) and ties Opus 5 on BankerToolBench. But on coding, DeepSeek’s equally priced Flash beats it on all three coding-heavy tests where both have scores: Terminal-Bench, DeepSWE and NL2Repo. On computer use, MiniMax says its OSWorld 2.0 and Agents’ Last Exam (29.6) scores are the best of any Chinese model, but they’re still behind GPT-5.6 Sol and Opus 5. Its strongest selling point is cost. In MiniMax’s chart of Agents’ Last Exam pass rate against total cost, its 29.6% run cost about $111, the cheapest of every model plotted, including Qwen 3.8 Max and Kimi K3.

Independent testing tells a similar story. Artificial Analysis scores M3.1 Flash at 42 on its Intelligence Index, up from 29 for M3. That’s level with GLM-5.3 Flash, one point above Gemini 3.8 Flash and three behind DeepSeek V4.1 Flash 1017 (45), at a lower cost per task than DeepSeek ($0.22 vs. about $0.26). It also found a real weakness. On its AA-Omniscience knowledge test, M3.1 Flash answered 56% of questions correctly, more than DeepSeek (43%) or GLM-5.3 Flash (28%). But it almost never declines to answer, so 93% of its non-correct answers were confident wrong answers rather than “I don’t know.”

## Why it matters

Chinese labs are now competing hard on cheap, fast models built for agents, and this race moves quickly. DeepSeek refreshed V4.1 Flash two days before this launch, and M3.1 Flash lands at the same price point. For anyone building an agent, the per-token price matters less than the cost to finish a task. MiniMax’s case rests on finishing tasks with fewer tokens at a flat price, and Artificial Analysis’s cost-per-task numbers back it up.

The tough part is reliability. A model that would rather guess than admit it doesn’t know is risky in agent loops that act on their own outputs: filing tickets, editing records, answering customers. If you use M3.1 Flash for that kind of work, add grounding (retrieval, tool checks) and verification steps instead of trusting it to flag its own uncertainty.

The “open” part isn’t settled yet either. Coverage, including Vercel’s changelog, calls M3.1 Flash an open-weight model. But at launch MiniMax only says “model weights will be released later,” and there is no M3.1 Flash repository on MiniMax’s Hugging Face page yet. VentureBeat and The Decoder report that MiniMax plans to release them on October 23. The license hasn’t been published. M3, its predecessor, ships under MiniMax’s own community license rather than a standard open-source one. As with any China-based provider, some teams can’t send code or data to MiniMax’s API for compliance reasons. Once the weights are out, self-hosting or a third-party host becomes the workaround. MiniMax also says a more capable **Pro** model is coming, without a date.

## Who should try it

- **Teams running business-workflow or computer-use agents on a budget:** this is where MiniMax’s own numbers are strongest. Test it against your current model on cost per completed task, not cost per token.
- **Coding-agent users already on DeepSeek V4.1 Flash:** on MiniMax’s own coding benchmarks DeepSeek still leads at the same off-peak price, so switching only makes sense if your evals say otherwise or you want a flat rate around the clock.
- **Anyone needing factual reliability:** test carefully. The 93% hallucination rate Artificial Analysis measured is the main reason to hold back.

## Where to try it

M3.1 Flash is live as `MiniMax-M3.1-Flash` on the MiniMax API, in the M Plan subscription and in the MiniMax Code desktop app. Vercel’s AI Gateway serves it as `minimax/minimax-m3.1-flash` for free until October 22 when the request is routed to MiniMax, and it works with Chat Completions, Responses and Anthropic Messages clients such as Claude Code and Codex.
