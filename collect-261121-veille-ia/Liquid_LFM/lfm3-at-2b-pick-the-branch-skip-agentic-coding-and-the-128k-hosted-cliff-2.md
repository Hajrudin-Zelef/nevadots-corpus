---
id: collect-261121-veille-ia/liquid-lfm/lfm3-at-2b-pick-the-branch-skip-agentic-coding-and-the-128k-hosted-cliff-2
title: "LFM3 at 2B: Pick the Branch, Skip Agentic Coding, and the 128K Hosted Cliff"
domain: liquid-lfm
role: reference
task: article
actors: ["Liquid AI"]
dates: ["2026-10-27"]
keywords: ["liquid", "lfm3", "liquidai", "agentic", "coding", "liquids", "lfm3-1", "models"]
source: https://yongboyu.hashnode.dev/lfm3-at-2b-pick-the-branch-skip-agentic-coding-and-the-128k-hosted-cliff
source_anchor: ""
source_lines: [1, 114]
section: "Liquid_LFM"
---
**Attributed compile + one small offline run on my box****Primary (Liquid AI / Hugging Face, launch 2026-10-27):** Build specialized multimodal models with LFM3 (LiquidAI authors on HF), Introducing LFM3 (Liquid AI blog), LFM3 collection.**Secondary:** VentureBeat — Carl Franzen, 2026-10-27 for hosted console prices, 128K vs 256K context, license revenue gate, memory footnotes, and the Liquid docs quote against agentic coding.

Benchmarks and on-device speed numbers below are **Liquid’s** (or VentureBeat’s reading of Liquid’s tables / console) unless I say otherwise. This box did **not** download multi-GB weights and did **not** call `generate()` / `system_one`. What I ran is a collection-id check, a dry first-call skeleton map, and list-price arithmetic on VentureBeat’s published console rates.


Liquid AI shipped **LFM3** on **2026-10-27**: six ~2B open-weight models branched from one **LFM3-1.7B-Base** backbone — text, vision-language (with video), audio, omni, and two decision models (`d1-2B`, `d1-Omni-2B`). The useful builder move is not “run the biggest generalist.” It is **pick the branch first**, then decide hosted vs downloadable — because the traps (context, license, coding fit) sit between those choices.

This is distinct from the earlier Liquid **d1** open CPU-triage write-up in this series: that post was about decision models as a product shape; this one is the **LFM3 family release** and how the branches wire into `transformers`.

From the HF / Liquid tables (vendor framing):

| Model | Reach for it when… | First `transformers` entry (HF blog) | 
|---|---|---|
| `LiquidAI/LFM3-1.7B` | Local agents, tool calling, structured text; 256K on the downloadable text model | `pipeline("text-generation", …)` | 
| `LiquidAI/LFM3-VL-2B` | Images + **video** → text; on-device visual Q&A | `AutoModelForImageTextToText` | 
| `LiquidAI/LFM3-Audio-2B` | Voice chat / ASR / TTS / tool calls from speech (18 languages claimed) | `AutoModelForMultimodalLM` | 
| `LiquidAI/LFM3-Omni-2B` | One on-device model that sees, listens, and talks back (~2.2B despite the “2B” name) | `AutoModelForMultimodalLM` | 
| `LiquidAI/d1-2B` | Many calibrated decisions in **one forward pass** over text/images/video | `AutoModel` +`system_one(...)` | 
| `LiquidAI/d1-Omni-2B` | Same decision shape when inputs include audio | (see omni model card; same idea) | 

Architecture note (Liquid, for intuition only): **24 layers**, tri-hybrid mix of **13** gated short convolutions + **5** SWA (256-token window) + **6** global attention; **22T** pretrain then **3.5T** mid-train that extends context to **256K**. Liquid says LFM3-1.7B beats their prior LFM2.5-2.6B with **~35% fewer parameters**, with the biggest gains on agentic browser/business tasks. Treat that as vendor positioning until you measure your own loop.

Liquid’s own table (reproduced and annotated by VentureBeat) is the honest “don’t oversell” slide. Selected Liquid-reported scores:

| Eval | LFM3-1.7B | Qwen3.5-9B | 
|---|---|---|
| WebArena-Verified | **38.42** | 28.94 | 
| AutomationBench (public) | **41.29** | 15.10 | 
| BFCL V4 | **71.39** | 62.83 | 
| SWE-bench Verified (OpenCode) | 43.40 | **51.20** | 
| GPQA Diamond | 63.37 | **80.40** | 
| RULER 128K | 65.56 | **87.11** | 
| MCP-Atlas | 32.57 | **42.67** | 

Liquid’s blog says larger models still win on **agentic coding and knowledge**. VentureBeat quotes Liquid’s documentation more bluntly: **not recommended for agentic coding or complex multi-step reasoning due to its small size**. That is the rule of thumb for this post: use LFM3-1.7B for **narrow local agents**, triage, tool routing, and browser/business loops where Liquid’s table shows strength — and **do not** treat it as a drop-in SWE / GPQA / long-RULER replacement for a ~9B (or larger) coder.

Three traps that show up before quality:

1. **Context.** Downloadable**LFM3-1.7B** is documented at**256K** . VentureBeat’s review of the signed-in Liquid console lists**128,000** context for the hosted LFM3 generation entries. If your agent prompt assumes the downloadable window on the API, you hit a**128K hosted cliff** .
2. **List prices (VentureBeat console, USD / 1M tokens).** LFM3-1.7B**$0.08** in /**$0.02** cached /**$0.20** out; VL**$0.10 / $0.02 / $0.50** ; Audio and Omni**$0.10 / $0.02 / $1.00** plus audio in**$2** / out**$10** per 1M audio tokens. Hosted**d1.1** is listed at**$0.02** in with**no output charge** — and VB notes that hosted identifier**may differ** from downloadable`d1-2B` .
3. **“~1GB” is weights, not the app.** Liquid’s edge pitch is ~**1GB at 4-bit** for the text model. That is**model storage** , not full process RAM. Liquid separately cites ~**1.6GB KV cache at 128K** ; VB’s table of Liquid-measured llama.cpp Q4_0 footprints on Ryzen AI Max+ 395 / Apple M5 Max / iPhone 17 Pro is higher still (e.g.**2.75GB / 2.25GB / 1.90GB** reported memory under that specific 32K-in workload). Local decode speed claims (**1.8–2.8×** the next model in Liquid’s comparison) are**Liquid-measured** on that hardware — label them as such.
4. **License.** LFM Open License**v1.0** : per Liquid’s terms as summarized by VentureBeat, businesses with**>$10M annual revenue** need a**separate commercial agreement** . Local weights remove per-token API charges; they do not erase licensing, hardware, or ops cost.

HF’s launch post is the practical card. Text:

```
from transformers import pipeline  # requires recent transformers; not installed on this box
generator = pipeline(
    "text-generation",
    model="LiquidAI/LFM3-1.7B",
    trust_remote_code=True,
)
messages = [{"role": "user", "content": "What is C. elegans?"}]
# NOT EXECUTED here — would need weights + generate()
```
VL uses `AutoModelForImageTextToText` + `AutoProcessor` (`transformers>=5.21.0` per HF). Audio / Omni use `AutoModelForMultimodalLM` (`transformers>=5.20.2`). Decision models use `AutoModel` and **`system_one(state, questions)`** — several named questions, one pass, calibrated answers:

```
from transformers import AutoModel  # skeleton only
# model = AutoModel.from_pretrained("LiquidAI/d1-2B", trust_remote_code=True, ...)
questions = {
    "refund": {"type": "noul", "instructions": "Is the customer asking for a refund?"},
    "team": {
        "type": "choice",
        "instructions": "Which team should handle this?",
        "criteria": {
            "billing": "Charges, refunds, invoices",
            "shipping": "Delivery and parcels",
            "technical": "App or site faults",
        },
    },
    "urgency": {
        "type": "score",
        "instructions": "How urgent is this ticket?",
        "criteria": ["low", "medium", "high"],
    },
}
# out = model.system_one("I was charged twice...", questions)  # NOT EXECUTED
```
HF also describes newer typed answers (Point / Read / Estimate / Pick) on the decision cards. I did not run those. The one-pass shape is the product point: classify / extract / route without a free-form chat turn when you do not need one.

`runs/28-lfm3/preflight.py` — no weight download. Trimmed output:

```
transformers: NOT INSTALLED
torch: NOT INSTALLED
Weights: NOT DOWNLOADED (by design; multi-GB)
HF collection LiquidAI/lfm3 — API OK:
  LiquidAI/LFM3-1.7B-Base, LFM3-1.7B, LFM3-VL-2B, LFM3-Audio-2B,
  LFM3-Omni-2B, d1-Omni-2B, d1-2B  (all expected core ids present)
Hosted list-price arithmetic (VB console; NOT a bill):
  LFM3-1.7B   20k in + 2k out, no cache $0.002000 | 80% cache $0.001040
  LFM3-VL-2B  20k in + 2k out, no cache $0.003000 | 80% cache $0.001720
  1M+1M text tokens: 1.7B $0.28 | VL $0.60 | Audio/Omni $1.10
  d1.1 1M in (no out charge): $0.02
Toy agent loop MY ASSUMPTIONS — 40 turns × (20k in + 2k out):
  LFM3-1.7B  no cache $0.0800 | 80% hit $0.0416
  LFM3-VL-2B no cache $0.1200 | 80% hit $0.0688
Dry skeletons for pipeline / AutoModel* / system_one: STATUS NOT EXECUTED
```
Costs are arithmetic on VentureBeat’s list rates under assumptions I state. They are not a bill, not latency, and not quality.

1. **Pick the branch** (text / VL / audio / omni / d1) — do not default to Omni “just in case.”
2. **Coding / hard reasoning?** Prefer a larger model; Liquid’s docs say so for this size class.
3. **Hosted?** Budget**128K** context and the console rate card; do not assume downloadable**256K** .
4. **Local?** Plan RAM beyond the ~1GB 4-bit weight number; check**LFM Open License v1.0** if revenue >$10M.
5. **Many structured questions?** Prefer**d1 `system_one`** over a chat model inventing JSON.
6. Wire an eval on *your* tickets, invoices, or tool traces. Vendor WebArena wins do not ship your workflow.

Specialize the small model when the job is narrow. Escalate when it is not.

YongBo Yu · Toronto AI engineer · https://yongbo-yu.vercel.app · GitHub YongBoYu1

No comments yet. Be the first to comment.
