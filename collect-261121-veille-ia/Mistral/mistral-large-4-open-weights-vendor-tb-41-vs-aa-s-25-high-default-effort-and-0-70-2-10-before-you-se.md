---
id: collect-261121-veille-ia/mistral/mistral-large-4-open-weights-vendor-tb-41-vs-aa-s-25-high-default-effort-and-0-70-2-10-before-you-se
title: "Mistral Large 4 Open Weights: Vendor TB 41% vs AA's 25%, High Default Effort, and $0.70/$2.10 Before You Self-Host"
domain: mistral
role: reference
task: article
actors: ["Mistral AI"]
dates: ["2026-10-29"]
keywords: ["mistral", "vendor", "large", "weights", "preview", "index", "open", "artificial"]
source: https://dev.to/yong_yu_f98e15562e9b120a0/mistral-large-4-open-weights-vendor-tb-41-vs-aas-25-high-default-effort-and-070210-3p4k
source_anchor: ""
source_lines: [1, 121]
section: "Mistral"
---
**Attributed compile + one small offline run on my box**

**Primary source:** Mistral Large 4 (Le Chonk) is now open weight by **Mistral AI**, published **2026-10-29** (weights after ~3 weeks of extra RL since the early-October public preview).

**Secondary:** Welcome Le Chonk: Mistral Large 4 on Hugging Face by **Aritra Roy Gosthipaty** (Hugging Face, 2026-10-29); VentureBeat — Carl Franzen for the Artificial Analysis vs Mistral Terminal-Bench / FrontierCode split and token-per-task notes (live VB fetch returned HTTP 429 on this draft day — numbers below follow Mistral’s post + the VB figures already confirmed for this series; do not treat a blocked fetch as missing coverage).

**Model id / card:** `mistralai/Mistral-Large-4-1T-A52B` (also FP8 / NVFP4 / GGUF siblings).

Benchmarks and prices below are **vendor-reported** (or clearly attributed to HF / VentureBeat’s reading of Artificial Analysis) unless I say otherwise. This box did **not** call chat completions and did **not** download weights. Offline preflight = public HTTP scrapes + list-price arithmetic only.


Mistral dropped **open weights** for **Mistral Large 4** (“**Le Chonk**”) on **2026-10-29**: a ~**1T**-parameter natively multimodal MoE that had already been in public preview earlier in October. The OSS-day story is not “a new model family name.” It is **more RL since preview**, a **reasoning-effort knob**, a **list price**, and a license with a **MaaS commercial gate**. For builders, the useful move is: separate **Mistral’s charts** from **Artificial Analysis’s own evals**, set effort deliberately (default is **high**), and decide **API vs self-host** before you swap a production model string.

Repo naming on Hugging Face is **`Mistral-Large-4-1T-A52B`**. The model card frames roughly **~1T total / ~52B active** (granular MoE: **128** routed experts, **4** active + **1** shared). An HF blog line once said **49B** active — prefer the **A52B** / card naming when you write configs. Context is **1M** tokens on the card. Architecture (short): MLA + DSA-style sparse attention, Pixtral vision encoder — enough to know it is sparse MoE + long context, not enough to skip a golden eval.

Mistral’s OSS post leads with coding agent claims. Label every row as **who measured it**.

| Claim (Mistral / named partner) | Figure | Notes | 
|---|---|---|
| Cognition FrontierCode 2.0 (private) | **19.7%** lead (vendor) | End-to-end “would a maintainer merge?” framing | 
| Artificial Analysis Coding Agent Index | **48%** , effectively tied with**GLM-5.3** | Mistral also claims **~47% fewer tokens/task** vs that peer on average | 
| Terminal-Bench 4.0 | **41%** (**+13 pp** vs preview) | Mistral says lead among open-weight in **their** chart;**~30%** fewer output tokens vs preview | 
| AA Cyber Index | **51%** | Defensive find / reproduce / patch framing (vendor) | 
| Malicious cyber refusal (Mistral measure) | avg **96.6%** | JailbreakBench / StrongREJECT / AgentHarm cyber prompts — **their** open-model comparison | 
| Finch | **73.3%** | Finance / spreadsheet workflows (vendor) | 
| BrowseComp | **84.5%** | From an SFT checkpoint Mistral cites at **75%** | 
| B3 agent security attack resistance | **97.1%** (preview**93.3%** ) | Prompt-injection / agent-attack resistance (vendor) | 

VentureBeat’s roundup is useful because it **puts Mistral’s numbers next to Artificial Analysis Intelligence Index rows** without pretending they are the same harness:

| Metric | Mistral claim | AA Intelligence Index eval (per VentureBeat) | 
|---|---|---|
| Terminal-Bench 4.0 | **41%** | **25%** (ML4**high** ) | 
| FrontierCode | **19.7%** | **15%** | 

Same model name, different eval contexts. **Do not** paste “41% TB4” into a slide that cites AA’s Intelligence Index table, and **do not** call AA’s **25%** a Mistral walk-back. HF’s launch blog still surfaces some **preview-era** coding rows (e.g. Terminal-Bench **28.3%**, Coding Agent Index ~**49.8%** in that post’s table). Treat those as **not** the OSS-day Terminal-Bench claim of **41%** after more RL. When in doubt: cite the **date + source**, or re-run your own tasks.

Token efficiency (Mistral + VB reading): on Coding Agent Index work, Mistral positions ML4 near GLM-5.3 on score while using far fewer tokens — VB cites roughly **~8.5M tokens/task** vs GLM **~15.9M** (~**47%** less). That is **cost/latency leverage**, not permission to skip quality gates.

OSS-day ML4 exposes three reasoning modes (Mistral table):

| Mode | Mistral’s recommended use | 
|---|---|
| **Low** | Simple tasks; **replaces** the old**None** mode | 
| **Medium** | Everyday tasks | 
| **High** (**default** ) | Hardest / high-precision work | 

Hugging Face’s Transformers / Inference Endpoints notes match the trap: `reasoning_effort` accepts **`low` | `medium` | `high`**, default **`high`**, and **`none` still maps to high** — reasoning is not truly off. If your migration code still sends `none` from an older Mistral preview client, you are **not** getting a cheap no-think path; you are paying **high** by default.

Builder rule: set effort **explicitly** per route. Use **low** for classification / short edits, **medium** for normal agent turns, **high** only where your eval shows it earns the tokens. Leaving the default untouched is a silent bill and latency multiplier.

Mistral says that **starting today** (OSS announce day) API list price is **$0.70** input / **$0.07** cached input / **$2.10** output **per 1M tokens**. Hosted surfaces named in the post: **Mistral Studio**, **Mistral API**, **OpenRouter**, plus **NVIDIA NIM** / hosted endpoints and **Microsoft Foundry**. Self-host path: Hugging Face weights + **vLLM** or **SGLang** (FP8 and **NVFP4** official; GGUFs from Unsloth / llama.cpp).

Offline preflight used those three rates only. **Assumptions:** one trajectory = **80k input + 20k output**; “80% cache hit” means 80% of *input* tokens bill at the cached rate; no tax, retries, or tool-overhead tokens.

| Scenario | USD (list math) | 
|---|---|
| 1 traj, no cache | **$0.0980** | 
| 1 traj, 80% input cache | **$0.0577** | 
| 40-turn loop, no cache | **$3.9200** | 
| 40-turn loop, 80% input cache | **$2.3072** | 
| 1M in + 1M out, no cache | **$2.8000** | 

Cache is the swing factor on long agent loops. Measure real hit rates before you put the middle rows in a forecast. Effort=**high** will also inflate **output** tokens relative to **low** — price the effort knob and the rate card together.

License is **Mistral Ecosystem Open-Weight** (modified MIT). Mistral’s summary: free to **use / modify / fine-tune / commercialize**, with one hard condition — businesses that operate a **Model-as-a-Service** offering need a **separate commercial license** before commercial MaaS use. Self-hosting inside your product is not the same as renting the weights out as a public model API. Read the LICENSE on the HF card before you put ML4 behind a multi-tenant endpoint you sell.

Separately: Mistral mentions a **specialized cybersecurity variant** for vetted design partners (e.g. Orange Cyberdefense) under contractual safeguards. That is **access-gated**, not the public HF checkpoint. Do not assume the public OSS weights are that partner build.

1. 
**Freeze a golden eval** on*your* agent failures (repo patches, terminal tasks, tool traces). Vendor TB**41%** and AA’s**25%** are both someone else’s harness.
2. 
**Pin the model id** — start from`mistralai/Mistral-Large-4-1T-A52B` (or the NVFP4 / FP8 sibling you actually serve). Do not guess strings. Docs also surface API-style ids such as`mistral-large-4-0` ; confirm against the surface you bill on.
3. 
**Set `reasoning_effort` explicitly.** Default is**high** . Treat`none` as**still high** .
4. 
**Price both columns:** list $0.70/$2.10/$0.07*and* expected output tokens at your chosen effort. Re-run the arithmetic with your real prompt sizes.
5. 
**Self-host only if** you have GPU budget (HF / partners discuss multi-GPU / Blackwell-class NVFP4 paths; VB notes quantized serving on**4× B200/B300** class setups — treat as secondary hardware rumor until you size it yourself), ops for vLLM/SGLang, and a license read for any MaaS-shaped product.
6. 
**API / OpenRouter first** when you need a same-day A/B without multi-GPU bring-up — then decide whether weight ownership is worth the ops.
7. 
**Cyber workloads:** use the**public** model under your own policy and sandbox. Partner cyber variants are gated; public refusal rates are Mistral’s measure, not your compliance program.
8. 
**Do not conflate** Mistral charts, HF blog tables, and AA Intelligence Index rows in one “leaderboard” screenshot.

`/workspace/reputation/runs/30-mistral-large-4/` — 2026-10-30 America/Toronto:


```
MISTRAL_API_KEY: NOT SET
OPENROUTER_API_KEY: NOT SET
Chat completions: NOT ATTEMPTED
HTTP 200: mistral.ai/news/mistral-large-4-oss/
HTTP 200: huggingface.co/mistralai/Mistral-Large-4-1T-A52B
HTTP 200: huggingface.co/blog/mistral-large4
HTTP 200: mistral.ai/pricing ; docs.mistral.ai models page
Needle hits on OSS page: $0.70 / $2.10 / $0.07, TB 41%, reasoning modes, MaaS license language
Model card: A52B id + FP8/NVFP4 siblings present
List-price math (assumptions above):
  1 traj no cache $0.0980 | 80% cache $0.0577
  40-turn no cache $3.9200 | 80% cache $2.3072
  1M+1M $2.8000
NOT tested: live chat; weight download; vLLM/SGLang; effort behavior;
Terminal-Bench / Coding Agent re-score; real cache hits; partner cyber variant
```
Costs are arithmetic on Mistral’s stated list rates under assumptions I state. They are not a bill, not latency, and not quality.

Only after a callable path (API or your own serve) and a golden eval win on *your* tickets:

1. Coding / terminal / cyber-defense workloads where your current open or mid-frontier default fails.
2. You have chosen **effort per route** , not left**high** everywhere.
3. Cost model survives realistic cache **and** high-effort output inflation.
4. License review is done if anything you ship looks like **MaaS** .
5. You can roll back without rewriting orchestration.

Weights on Hugging Face are the story this week. Treat Mistral’s **41%** and AA’s **25%** as two labeled columns, set effort on purpose, and run the $0.70/$2.10 math on *your* trajectories before you call the swap done.

YongBo Yu · Toronto AI engineer · https://yongbo-yu.vercel.app · GitHub YongBoYu1

## Top comments (0)
