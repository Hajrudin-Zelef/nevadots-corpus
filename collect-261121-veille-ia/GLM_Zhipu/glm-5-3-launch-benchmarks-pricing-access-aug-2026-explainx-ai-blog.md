---
id: collect-261121-veille-ia/glm-zhipu/glm-5-3-launch-benchmarks-pricing-access-aug-2026-explainx-ai-blog
title: "GLM-5.3 Launch: Benchmarks, Pricing & Access (Aug 2026) | explainx.ai Blog"
domain: glm-zhipu
role: reference
task: article
actors: ["Zhipu AI"]
dates: []
keywords: ["glm-5", "august", "model", "launch", "update", "open", "code", "weights"]
source: https://explainx.ai/blog/glm-5-3-launch-cyber-defense-benchmarks-august-2026
source_anchor: ""
source_lines: [1, 173]
section: "GLM_Zhipu"
---
**Update — August 20, 2026:** Artificial Analysis's independent evaluation now has GLM-5.3 tying Kimi K3 at 60 on its Intelligence Index — see GLM-5.3 Ties Kimi K3 on the AA Intelligence Index for the score breakdown and why Z.ai got there without a new base model.

**Update — August 21, 2026:** A trending aggregator headline claimed "GLM-5.3 Max ranks 2nd among open code models, beating Gemini 3.7 Flash." explainx.ai fact-checked that claim against Artificial Analysis, Z.ai Code Bench, and BenchLM — see GLM-5.3 Max "2nd Among Open Code Models": What the Numbers Actually Show for what holds up and what doesn't.

**Update — August 28, 2026:** The "staged" open-weight timeline referenced below missed its own target. Z.ai's Hugging Face placeholder for `zai-org/GLM-5.3` listed August 28 as the release date — that date passed with no weights published and no new date confirmed. Full timeline and what it means for self-hosting plans: GLM-5.3 open weights delayed — Z.ai misses its own Aug 28 target.

**Update — August 26, 2026 (evening):** Z.ai named Ox Alpha **GLM-5.3-Flash** — **320B-A18B**, **MIT license**, **1M multimodal context**, API **$0.15/$0.50** per M tokens. This is a **different SKU** from the **743B-base GLM-5.3** covered below (cyber-defense line, staged open weights). Full Flash launch: GLM-5.3-Flash official post.

**Update — August 26, 2026:** Z.AI **confirmed to Bloomberg** that OpenRouter stealth model **Ox Alpha** is a **new GLM iteration** — open weights promised that night. Ox Alpha hit **#1 on OpenRouter**, more than doubling DeepSeek usage. Forensics timeline: Ox Alpha confirmed.

**Update — August 22, 2026:** Independent serving-layer forensics **support the theory** that OpenRouter's free stealth model **Ox Alpha** runs on **Zhipu / Z.AI GLM infrastructure** — Java stack trace, shared error code 1214, 30/30 tokenizer match — **confirmed August 26**: Ox Alpha: what we know.

**Update context:** This is the confirmed launch of the model explainx.ai covered as community speculation in June, when Zhipu founder Jie Tang polled X on what GLM-5.3 should include. Vision won that poll. The actual GLM-5.3 launch is not about vision — it's about coding and cybersecurity.

On **August 14, 2026 at 10:47 AM**, Z.ai (@Zai_org) posted the announcement that ends six weeks of speculation:

*"Introducing GLM-5.3: Built to Code. Ready for Cyber Defense. — Top-tier coding and agentic capabilities, achieved through post-training on the 743B base model — A major leap in cybersecurity, setting a new standard among open models."*


The post drew 62,500+ views within hours. Unlike GLM-5.2's June launch — MIT-licensed weights on Hugging Face almost immediately — GLM-5.3 ships with a meaningfully different access model, and that difference is the real story underneath the benchmark chart.

## TL;DR

| Question | Answer | 
|---|---|
| **What shipped?** | GLM-5.3, post-trained on a **743B parameter base model** | 
| **Tagline** | "Built to Code. Ready for Cyber Defense." | 
| **Available now?** | Yes — via **GLM Coding Plan** and**ZCode** | 
| **Open weights?** | **Not yet.** Staged rollout "following rigorous safety evaluations" | 
| **API access?** | Also staged, same safety-review gate | 
| **Strongest benchmark lead** | **AutomationBench 48.2%** ,**GDPVal-AA v2 1769 Elo** ,**CyberGym 84.5%** | 
| **Weakest vs rivals** | **ExploitBench 54.4%** vs Fable 5's 78.0% and GPT-5.6 Sol's 76.5% | 
| **Positioning** | Defensive cybersecurity, not offensive exploit generation | 
| **Pricing (Coding Plan)** | Lite $12.6/mo · Pro $56/mo · Max $117.6/mo | 

## What's Actually New vs the June Speculation

The June 29 community poll put **vision** at the top of the GLM-5.3 wishlist — screenshots, PDFs, UI designs, an Opus-class multimodal jump. Z.ai's actual launch post makes **no mention of vision**. The headline capabilities are:

1. **Coding and agentic capability** , from post-training on a**743B parameter base model**
2. **Cybersecurity** — specifically framed as**defense** , not general capability
3. A **gated release model** that is new for the GLM line

That last point deserves attention before the benchmarks do.

## The Access Shift: Staged Rollout, Not Day-One Open Weights

This is the most notable departure from GLM-5.2's pattern, and Z.ai stated it plainly in a follow-up post:

*"GLM-5.3 is available now through GLM Coding Plan and ZCode. API access and open weights will be released in stages following rigorous safety evaluations."*


Compare that to GLM-5.2's rollout: weights landed on Hugging Face under an **MIT license** within days of the Coding Plan launch, fast enough that Cline built a $9.99/month subscription around bundled access almost immediately, and George Hotz was running it as a daily driver inside weeks.

GLM-5.3 does not follow that script. Right now:

| Access path | Status | 
|---|---|
| **GLM Coding Plan** | Live | 
| **ZCode** | Live | 
| **API** | Staged — safety review pending | 
| **Open weights** | Staged — safety review pending | 
| **Select partners** | Live, gated, "safeguards and usage policies in place" | 

Z.ai's third launch post confirms the partner motion is deliberate, not a placeholder:

*"An initial group of partners is now offering GLM-5.3-powered services through our official service, with its safeguards and usage policies in place. We're expanding partner access through a consistent and responsible process and will share updates publicly."*


We won't over-editorialize the cause, but the fact pattern lines up: a model explicitly marketed for **cybersecurity** capability, including offensive-adjacent benchmarks like ExploitBench and ExploitGym in its own published chart, is exactly the kind of dual-use release a lab would want to gate before handing out raw weights. Whether that reasoning is stated internally or not, it's the plainest explanation for why this launch reads differently from GLM-5.2's.

## The Benchmarks: Z.ai's Own Chart, Read Honestly

Z.ai published a nine-benchmark chart ("LLM Performance Evaluation") comparing GLM-5.3 against **GLM-5.2**, **Kimi K3**, **Mythos/Fable 5**, and **GPT-5.6 Sol**. As with any first-party vendor chart, treat it as Z.ai's own reporting, not an independent audit — but it's detailed enough to be useful, and it does not show GLM-5.3 winning everything.

| Benchmark | GLM-5.3 | GLM-5.2 | Kimi K3 | Mythos/Fable 5 | GPT-5.6 Sol | 
|---|---|---|---|---|---|
| Terminal Bench 3.0 | 28.3% | 4.6% | 17.4% | **33.7%** | **34.6%** | 
| DeepSWE | 66.9% | 46.2% | 67.5% | 69.7% | **72.7%** | 
| Agents' Last Exam (CLI) | 28.5% | 23.8% | 27.6% | 23.8% | **28.6%** | 
| AutomationBench | **48.2%** | 26.2% | 46.7% | 46.2% | 45.8% | 
| HLE w/ Tools | 62.5% | 54.7% | 59.8% | 63.9% | **64.5%** | 
| GDPVal-AA v2 (Elo) | **1769** | 1508 | 1682 | 1743 | 1730 | 
| CyberGym | **84.5%** | 77.2% | 80.0% | 83.8% | 83.6% | 
| ExploitBench | 54.4% | 24.4% | 32.2% | **78.0%** | 76.5% | 
| ExploitGym (2hr / 6hr) | 105 / 130 | 29 / 39 | 36 / 70 | **181 / 247** | unclear from chart | 

Bold marks the row leader. Three honest takeaways:

- **GLM-5.3 clearly beats GLM-5.2** on every row — a real generational jump for Z.ai's own line, and the comparison Z.ai most wants readers to make.
- **GLM-5.3 does not lead the field.** Mythos/Fable 5 and GPT-5.6 Sol beat it on Terminal Bench 3.0, DeepSWE, and HLE w/ Tools — and beat it*badly* on ExploitBench and ExploitGym.
- **GLM-5.3's real differentiation is narrow and specific** : AutomationBench, GDPVal-AA v2, and CyberGym — a coherent cluster around automation and defensive security rather than raw coding or exploit generation.

### Why trailing on ExploitBench actually supports the "cyber defense" framing

The instinct is to read GLM-5.3 trailing Fable 5 (78.0%) and GPT-5.6 Sol (76.5%) on ExploitBench by 20+ points as a weakness. It's worth separating what each benchmark actually measures:

- **CyberGym** — defensive cybersecurity capability (finding, patching, hardening)
- **ExploitBench / ExploitGym** — offensive exploit generation, under time budgets

Z.ai's tagline is "**ready for cyber defense**," not "best at offense." A model that leads CyberGym while trailing on exploit generation is showing exactly the capability skew its own marketing claims — not an accident, and not a contradiction. If GLM-5.3 had topped ExploitGym instead, that would arguably be the more concerning outcome for a model being positioned toward defenders. Read alongside the staged, safety-gated open-weights rollout, the whole release reads as a consistent stance rather than a mixed message.

For a deeper look at what ExploitBench actually measures — the five-tier capability ladder, the 41 V8 vulnerabilities it runs against, and how Anthropic's Mythos Preview scored 21 of 41 on full code execution — see explainx.ai's ExploitBench explainer.

## Pricing: GLM Coding Plan Tiers

GLM-5.3 is live today through Z.ai's existing **GLM Coding Plan** structure and **ZCode 3.0**, Z.ai's first-party coding agent (comparable to Claude Code, Codex, or Cursor), available on macOS, Windows, and Linux:

| Tier | Price | Quota | Notes | 
|---|---|---|---|
| **Lite** | $12.6/mo | 10,000 credits/week | 20+ agent tool support, including ZCode and Claude Code | 
| **Pro** | $56/mo | 6x Lite usage | Adds MCP tool support | 
| **Max** | $117.6/mo | 14x Lite usage | Highest quota tier | 

These tiers were built around GLM-5.2; Z.ai has not published GLM-5.3-specific pricing changes, so treat the numbers above as the current baseline rather than a GLM-5.3 launch price. Notably, ZCode explicitly supports **routing GLM through other harnesses**, including Claude Code — not locking usage into its own IDE, which matters for teams who've already standardized on Claude Code, Codex, or Gemini CLI workflows and want to swap the underlying model rather than the tooling.

## What People Are Saying

Early reactions on X are a mix of hands-on praise and pointed community asks.

**AshutoshShrivastava** (@ai_for_success), who had early access:

*"Thanks for early access had fun playing with it.. Massive massive upgrade.."*


**Xiaopu Peng** (@XiaopuPeng), appearing Z.ai-affiliated, tied the launch back to prior teasers:

*"We said soon… and we mean it 👀"*


**Unsloth AI** (@UnslothAI) flagged both a benchmark question and a concrete product ask:

*"Congrats guys! Does this make GLM-5.3 the strongest open model to date so far? 🤯 We can't wait to make quants for it for the people who can run it. Would also be incredible if you guys could create smaller models like before like GLM-4.7-Flash!"*


Unsloth's question about "strongest open model" is worth sitting with given the staged rollout above — there's no open-weight model to quantize yet, so the quant-and-benchmark cycle the community ran on GLM-5.2 is paused until Z.ai completes its safety review.

**Ahmad Awais** (@MrAhmadAwais), building Command Code AI, a coding agent product, shared a partner integration note along with an interesting internal eval anecdote (his post was cut off mid-sentence, quoted faithfully rather than completed here):

*"Super excited to partner up and ship GLM 5.3 in @CommandCodeAI it's an impressive model. A fun internal eval we run at Command Code: deliberately trap the model in a loop and see what happens. Every GLM model so far just... keeps looping. GLM 5.3 is the first one to notice, go [cut off]"*


If accurate, that's a genuinely useful signal for agent-harness builders: loop-detection and self-correction are exactly the kind of failure mode that separates "benchmark-strong" from "production-reliable" in long-horizon agent runs — a gap prior GLM-5.2 harness coverage has flagged before.

## What This Means If You're Building on GLM

**If you're already on the GLM Coding Plan or ZCode:** GLM-5.3 is a straightforward upgrade path today — no weight download required, and the benchmark table shows a real jump over GLM-5.2 on every row.

**If you were waiting for open weights to self-host or quantize:** you're waiting longer than the GLM-5.2 cycle taught you to expect. Track Z.ai's GitHub and Hugging Face orgs, but budget for a safety-review-length delay rather than a same-week drop.

**If you're comparing open-weight coding models broadly:** GLM-5.3 is not a blanket leader. For raw terminal/CLI coding and general reasoning-with-tools work, Fable 5 and GPT-5.6 Sol still post higher numbers on Z.ai's own chart. GLM-5.3's case is narrower and more specific: automation workflows, GDPVal-style agentic tasks, and defensive cybersecurity postures.

**If you're evaluating for security work specifically:** the CyberGym lead is real and worth testing against your own defensive workloads, but do not read it as offensive capability — ExploitBench and ExploitGym numbers say the opposite, and that gap is by design, not oversight, per Z.ai's own "cyber defense" framing.

**Update — September 30, 2026:** Anthropic's Frontier Red Team evaluated these **open weights** on offensive harnesses — ExploitBench end-to-end **12%** vs Mythos Preview **14%**, not a CyberGym rerun. GLM-5.3 cyber capabilities.

## Related Reading

- **Update — Sep 26, 2026:** LongCat 2.5 launched with an explicit autonomous-agent framing — see how Meituan's 1.6T model compares to GLM-5.3 for agentic coding.
- **Update — Aug 28, 2026:** GLM-5.3's**open weights are out** — Z.ai released them on Hugging Face (`zai-org/GLM-5.3` ) after a two-week safety review, calling it "our most capable model for agentic coding and cyber defense." The model also ties Kimi K3 at 60 on the Artificial Analysis Intelligence Index; its CyberGym and Terminal-Bench figures stay provider-reported until independently rerun.
- **Update — Aug 26, 2026 (evening):** Ox Alpha is**GLM-5.3-Flash** (320B-A18B, MIT) — distinct from this**743B GLM-5.3** post: Flash launch · forensics timeline · OpenRouter setup.
- **Update — Aug 16, 2026:** ZCode's operator prompt leaked — 391,439 characters revealing tool names and skill playbooks that closely mirror Claude Code.
- **Update — Aug 16, 2026:** Is the 84.5% CyberGym score verified yet? No — here's Z.ai's actual staged validation timeline.
- ExploitBench Explained — the five-tier capability ladder behind GLM-5.3's 54.4% ExploitBench score
- GLM-5.3: Zhipu Asks the Community — Vision Leads the Wishlist — the June speculation this post resolves
- GLM-5.2 Beats Fable 5 on Reasoning — the original GLM-5.2 launch context
- GLM-5.2 Goes Fully Open Under MIT — Code Arena Adoption — how the GLM-5.2 open-weights cycle actually played out
- Cline's $9.99/mo GLM-5.2 Plan — what fast open access enabled last cycle
- Fable 5 vs Grok 4.6 vs GPT-5.6 Sol vs Qwen3.8-Max — independent benchmark cross-reference for the models in Z.ai's chart
- How to Run GLM-5.2 on Agent Harnesses — setup guide, relevant while GLM-5.3 API/weights are staged
- Claude Code vs Codex vs Gemini CLI vs GLM-5.2 — harness comparison for ZCode's cross-harness routing

*Benchmark and access details reflect Z.ai's official launch posts and Tech Blog as of August 14, 2026. Open weights and API access were not yet available at publication time — check z.ai/blog/glm-5.3 for staged-rollout updates.*
