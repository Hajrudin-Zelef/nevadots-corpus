---
id: collect-261121-veille-ia/minimax/minimax-m3-1-flash-benchmarks-price-agent-test
title: "MiniMax M3.1 Flash: Benchmarks, Price, Agent Test"
domain: minimax
role: reference
task: article
actors: ["MiniMax"]
dates: ["2026-10-19"]
keywords: ["minimax", "flash", "model", "agent", "same", "openrouter", "free", "october"]
source: https://atomicagent.io/blog/minimax-m3-1-flash/
source_anchor: ""
source_lines: [1, 143]
section: "MiniMax"
---
MiniMax released MiniMax M3.1 Flash on October 19, 2026, and in the same post confirmed what developers had guessed for weeks: **Space Bunny Alpha**, the anonymous model that topped OpenRouter’s usage charts in late September, was an early version of it. MiniMax prices it at **$0.15 per million input tokens and $0.60 per million output**, and says it cuts the cost of agent tasks by 70% compared with its predecessor, M3.

OpenRouter offered it for free for the first three days; that route has ended, and the model now costs MiniMax’s list price. On October 23 MiniMax also published the weights on Hugging Face under the MiniMax Community License.

We ran it the way most people will use it: as a coding agent. Same task, same agent, three runs at low effort and three at the default max effort, next to MiniMax M3, DeepSeek V4.1 Flash, GLM 5.3 Flash, Claude Haiku 5.5, and Claude Sonnet 5.5.

**Model IDs.** OpenRouter: `minimax/minimax-m3.1-flash`. MiniMax API: `MiniMax-M3.1-Flash`. Weights: MiniMaxAI/MiniMax-M3.1-Flash. In Atomic Agent, pick it from the live OpenRouter catalog with `/model`.


## What was Space Bunny Alpha?

Space Bunny Alpha was an anonymous model that OpenRouter listed on September 23, 2026, with a 1 million token context, text, image and video input, mandatory reasoning, and a price of zero. Tokenizer tests pointed to MiniMax on day one. On October 19 MiniMax confirmed it: Space Bunny “was an early version of MiniMax M3.1 Flash.”

The timeline, from CellCog’s record and MiniMax’s own posts:

- **September 23.** OpenRouter lists`stealth/space-bunny-alpha` as “an anonymous large model with blazing-fast inference, strong coding capabilities and native multimodal input support.” Three independent tokenizer tests match the MiniMax tokenizer family the same afternoon.
- **September 27.** MiniMax ships M3.1-Flash-Preview in its MiniMax Code app. Its config lists the same 1M context, forced reasoning, and low-to-max effort levels.
- **October 5.** The free stealth listing ends.
- **October 19.** MiniMax’s account posts “Welcome back. Space Bunny.” and publishes the launch post. MiniMax says the stealth model processed**more than 200 trillion tokens** while it ran anonymously.

During the stealth period OpenRouter’s listing allowed the provider to retain prompts, though not to train on them. Keep that in mind if you sent private code to Space Bunny.

## Is MiniMax M3.1 Flash free?

Not anymore. At launch the only MiniMax M3.1 Flash entry in OpenRouter’s model API was `minimax/minimax-m3.1-flash:free`, priced at $0, and OpenRouter said the free period would last 72 hours. By **October 23, 2026** the free route was gone, and OpenRouter listed only the paid `minimax/minimax-m3.1-flash`, served by MiniMax. If your agent config still points to the `:free` ID, switch it to the paid one.

The price is MiniMax’s list price: $0.15 input, $0.60 output, $0.015 cached input per million tokens, with no long-context surcharge. On MiniMax’s own platform the model is also included in the M Plan subscription and in MiniMax Code. The other free option is to run the open weights on your own hardware, which takes a multi-GPU server.

## How good is MiniMax M3.1 Flash as a coding agent?

It finished our task every time, and the effort level decided how long that took. All six MiniMax M3.1 Flash runs passed **all 32 hidden tests**. At the default max effort a run took **341 to 771 seconds** and up to 78 model calls. At low effort the same task took **32 to 62 seconds**. The model reasoned **48 times more** at max for the same result.

### How we tested

We used the same task as in our Claude Sonnet 5.5 test: a small Python package with five planted bugs and two missing features (a settlement function and a command-line tool), plus 32 hidden tests the agent never sees. The prompt is one line: read the task file, complete it, run the tests until they pass. Each model ran three times in Atomic Agent v0.6.9 through OpenRouter on October 19, 2026, with approvals off.

We pinned each model to one provider so OpenRouter couldn’t swap in a cheaper quantized copy: MiniMax for both MiniMax models, Together for DeepSeek, Z.AI for GLM. For “low” we sent OpenRouter’s `reasoning.effort: low`; “max” means we sent nothing and let the model use its default.

### Results

| Setup | Hidden tests | Avg time | Cost per task | Avg steps | Output tokens | Reasoning tokens | 
|---|---|---|---|---|---|---|
| **M3.1 Flash, low effort** | 96/96 | **48 s** | **~$0.012** | 17 | 5.5K | 1.3K | 
| **M3.1 Flash, max effort (default)** | 96/96 | **9.5 min** | **~$0.086** | 61 | 76.8K | 60.6K | 
| MiniMax M3 | 96/96 | 2.6 min | $0.074 | 34 | 12.0K | 4.5K | 
| DeepSeek V4.1 Flash 1017 | 96/96 | 27 s | $0.016 | 14 | 6.4K | 0.9K | 
| Claude Haiku 5.5 | 96/96 | 36 s | $0.011 | 8 | 7.7K | 1.7K | 
| GLM 5.3 Flash | 96/96 | 14.1 min | $0.059 | 34 | 37.9K | 26.7K | 
| Claude Sonnet 5.5 | 96/96 | 49 s | $0.183 | 6 | 6.6K | 0.7K | 

*Three runs per setup, averages shown. Token counts are the provider’s native counts from OpenRouter’s generation records. Costs for M3.1 Flash are estimated from its native token counts at MiniMax’s list price, because our runs used the free route that was live at launch; all other costs are what OpenRouter billed.*

Every setup solved the task in every run, so this table doesn’t rank the models by intelligence. It shows **how much work each one does to get there**, and that work is what you pay for in time and money.

At max effort, M3.1 Flash produced about **14 times more output** than at low effort, almost all of it reasoning. Because every agent step resends the whole conversation, the extra steps also multiplied input: about 2.3 million input tokens per run at max versus 380,000 at low. The provider served most of that input from cache, which is why the estimated bill grew about 7 times while the time grew about 12 times.

Atomic Agent also flagged the long runs. Its loop detector logged a “no-progress loop” in **all three max-effort runs** and in none of the low-effort runs. The model kept repeating the same shell and test commands. The same warning also fired in two of three runs for MiniMax M3 and for GLM 5.3 Flash, so it is a sign of long reasoning loops in general, not only of this model.

**Limits of this test.** One small, well-specified Python task, three runs per setup, on one day. It shows cost and speed on a task every model can solve. It says nothing about hard, open-ended work, where max effort is supposed to pay off. The M3.1 Flash cost is an estimate from token counts at MiniMax’s list price, which is also what OpenRouter’s paid route charges.

Download Atomic Agent to repeat this on your own repo: switch the model per run and keep the same tools and prompt.

## How do I use MiniMax M3.1 Flash in Claude Code or Atomic Agent?

The quickest route is any agent that speaks to OpenRouter. In Atomic Agent the model appears in the live OpenRouter catalog on day one, because the agent fetches the live list from OpenRouter (with a built-in list as a fallback). Set the effort to low in the provider config if you want the fast behavior from our test.

Install and start the agent:

On Windows, run `irm https://atomicagent.io/install.ps1 | iex` in PowerShell, or download the desktop app. In the first-run setup choose **Cloud models**, then **OpenRouter**, paste your key, and pick `minimax/minimax-m3.1-flash`. Later you can switch models with `/model`. The full steps are in our OpenRouter guide.

To run at low effort, add `extraBody` to the OpenRouter entry under `llm.providers` in `~/.atomic-agent/config.json`:

For Claude Code, MiniMax publishes its own setup guide through its Anthropic-compatible API.

If you want to compare it with other models on your own work, Atomic Agent lets you switch models per run while the tools and prompt stay the same. That’s how we produced the table above. In Fusion mode a cheap cloud model like this one can plan and review while a local model does the edits.

## How much does MiniMax M3.1 Flash cost per task?

At list price, about **$0.012 per task at low effort and $0.086 at max** in our test. Per token it’s half the price of MiniMax M3, but per task the effort level matters more than the price list: at max effort M3.1 Flash cost a little more than M3, and at low effort about 84% less.

| Model | Input | Output | Cached input | 
|---|---|---|---|
| MiniMax M3.1 Flash | $0.15 | $0.60 | $0.015 | 
| MiniMax M3 (MiniMax as provider) | $0.30 | $1.20 | $0.06 | 
| DeepSeek V4.1 Flash 1017 (Together) | $0.30 | $1.20 | $0.006 | 
| Claude Haiku 5.5 | $0.10 | $0.50 | $0.01 | 

*Prices per million tokens, from MiniMax for M3.1 Flash and from OpenRouter’s provider listings for the others, on October 19, 2026.*

MiniMax’s claim is that “for agentic tasks, the model’s overall usage cost is 70% lower than its predecessor.” Our numbers back that at low effort and contradict it at the default max effort. In an agent loop almost all input comes from cache, so the cached-input price and the number of steps decide the bill more than the headline input price.

## Is M3.1 Flash better than MiniMax M3?

At low effort it is faster and cheaper. In our runs it took **48 seconds and about $0.012 per task**, against 157 seconds and $0.074 for M3. MiniMax also says it is nearly three times faster at inference and reports gains on agent, coding, computer-use, and security benchmarks. Both models have the same reported size.

What changed for developers:

- **Reasoning is always on.** M3 could switch reasoning off. M3.1 Flash has five effort levels (low, medium, high, xhigh, max) and defaults to max.
- **Half the token price.** $0.15 and $0.60 against $0.30 and $1.20 for M3 from MiniMax.
- **Faster output.** MiniMax quotes “200+ TPS” peak and a median above 170 tokens per second.
- **Same scale.** MiniMax’s model page lists about 428B total parameters with about 23B active, the same as M3.
- **What changed inside.** The Hugging Face model card names three changes: sparse attention in every layer (M3 kept three full-attention layers), quantization-aware training for the experts and attention, and a DSpark speculative decoding head in place of M3’s multi-token prediction.

## How does MiniMax M3.1 Flash score on benchmarks?

MiniMax reports **73.7% on DeepSWE v1.1**, **61.7 on OSWorld V2**, **29.6 on Agents’ Last Exam**, **76.2 on BankerToolBench**, and **82.2 on CyberGym**. It says the model ranks first among Chinese models on OSWorld V2 and Agents’ Last Exam. All of these are MiniMax’s own numbers. The launch post didn’t name the agent harnesses; the Hugging Face model card added them on October 23, for example mini-SWE-agent for DeepSWE, Claude Code for CyberGym, and OpenCode for BankerToolBench.

| Benchmark | What it measures | M3.1 Flash | 
|---|---|---|
| DeepSWE v1.1 | Real software tasks with tests | 73.7% | 
| OSWorld V2 | Operating a computer across apps | 61.7 | 
| Agents’ Last Exam | Professional tasks with economic value | 29.6 | 
| BankerToolBench | Multi-step finance work with tools | 76.2 | 
| CyberGym | Finding and validating vulnerabilities | 82.2 | 

*Vendor-published, from MiniMax’s launch post. Not our tests.*

Other vendors use other harnesses and versions, so these scores still can’t be compared one to one with their tables. DeepSeek V4.1 Flash, the other new low-cost model this month, ran the same task in 27 seconds for $0.016 in our test; we compare the two in detail in DeepSeek V4.1 Flash vs MiniMax M3.1 Flash.

## What are the known problems with MiniMax M3.1 Flash?

The main one is overthinking. Max effort is the default, and on a simple task it spends minutes repeating checks, which we saw in every max-effort run. Developers have also noticed terse “caveman” reasoning, one user reported pauses on long runs, and OpenRouter’s stealth listing showed some tool-call errors.

- **Overthinking at the default effort.** In our test max effort used about 60,000 reasoning tokens per run for a task low effort solved with about 1,300.
- **Clipped reasoning text.** A r/LocalLLaMA thread noticed it reasons in compressed “caveman” style. The poster says final answers are unaffected; a commenter disagreed.
- **Long runs.** One OpenCode user reported that Space Bunny stops every 8 to 12 minutes on long tasks; others in the thread said it ran for hours. Our runs were too short to check this.
- **Tool calls.** OpenRouter’s stealth listing showed a 3.16% tool-call error rate in the first days, according to CellCog’s snapshot.

## Where are the MiniMax M3.1 Flash weights?

On Hugging Face, since October 23, 2026: MiniMaxAI/MiniMax-M3.1-Flash. The model card lists about **428B total parameters with 23B active per token**, text, image and video input, and a context window of up to 1 million tokens. The full download is about **250 GB**. It’s smaller than the parameter count suggests because the experts and attention were quantized during training.

**License.** The weights use the MiniMax Community License, the same family as M3, not MIT or Apache. Non-commercial use is free. Commercial use is allowed if you show “Built with MiniMax M3.1-Flash” in your product or docs and send MiniMax a one-time notice; above $20 million in yearly revenue you need MiniMax’s written permission first. The license also lists prohibited uses, including any military purpose.

**How to run it.** MiniMax recommends SGLang, vLLM, or TokenSpeed and links a recipe for each, with `temperature=1.0` and `top_p=0.95`. The effort levels from the API (low to max) are set through `reasoning_effort` in the chat template. Support in Transformers, KTransformers, and Unsloth is listed as “in progress.” On October 23 there was no GGUF build on Hugging Face, so Ollama and LM Studio can’t load it yet.

This still isn’t a laptop model. 250 GB of weights, before any room for the context cache, means a multi-GPU server. If you want an agent that runs fully on your own machine today, a mid-size open model is the practical choice; we tested one in Qwen 3.8 27B as a local agent.

## The takeaway

MiniMax M3.1 Flash is cheap, fast at low effort, and capable enough to finish a real bug-fixing task every time we tried. The default max effort made the same job about 12 times slower and about 7 times more expensive, with no gain on our task. Set effort to low or medium for routine agent work, and save max for problems that actually need it. To pick the agent around it, see our open-source Claude Code alternatives.

It costs $0.15 per million input tokens and $0.60 per million output, half the token price of MiniMax M3, and since October 23 the weights are open under the MiniMax Community License.
