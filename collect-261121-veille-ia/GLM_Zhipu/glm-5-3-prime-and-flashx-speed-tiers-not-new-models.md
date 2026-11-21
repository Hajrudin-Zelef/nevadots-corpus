---
id: collect-261121-veille-ia/glm-zhipu/glm-5-3-prime-and-flashx-speed-tiers-not-new-models
title: "GLM-5.3 Prime and FlashX: Speed Tiers, Not New Models"
domain: glm-zhipu
role: reference
task: article
actors: ["Zhipu AI"]
dates: ["2026-08-25"]
keywords: ["glm-5", "prime", "flashx", "model", "weights", "flash", "text", "models"]
source: https://codersera.com/blog/glm-5-3-prime-flashx-guide-2026/
source_anchor: ""
source_lines: [1, 166]
section: "GLM_Zhipu"
---
**Quick answer.**GLM-5.3 Prime and GLM-5.3 FlashX are not new models. Both are accelerated serving tiers of weights Z.ai already shipped: Prime runs GLM-5.3 at 1.5–2× throughput for $2.80/$8.80 per 1M tokens, FlashX runs GLM-5.3-Flash at up to 200 tokens/s for $0.37/$1.25. Neither has downloadable weights of its own.

Z.ai added two names to the GLM-5.3 family in September 2026, and the naming is doing real damage to people's mental models. "Prime" sounds like a bigger, smarter GLM-5.3. "FlashX" sounds like a new small model. Neither reading is right.

Both are **inference-acceleration tiers** layered over checkpoints that already existed. Prime serves the same GLM-5.3 weights faster; FlashX serves the same GLM-5.3-Flash weights faster. The intelligence is identical to the model underneath — what you buy is throughput, and the premium is steep. Prime costs exactly 2× GLM-5.3 for a claimed 1.5–2× speedup; FlashX costs about 2.5× GLM-5.3-Flash. If you picked Prime assuming it was the strongest GLM, you overpaid for a property you may not need.

Here is the whole lineup, verified against Z.ai's pricing docs, the Hugging Face `zai-org` model cards, and OpenRouter's live model API.

## What are the five GLM-5.3-era models, side by side?

| Model | Listed | Total / active | Context | Modality | Weights to download? | Licence | Price in / out per 1M | 
|---|---|---|---|---|---|---|---|
| **GLM-5.2** | 16 Jun 2026 | 753B / 40B | 1,048,576 | Text | Yes — `zai-org/GLM-5.2` | **MIT** | $1.40 / $4.40 | 
| **GLM-5.3** | 18 Aug 2026 | 753B / 40B | 1,048,576 | Text | Yes — `zai-org/GLM-5.3` | Bespoke `glm-5.3` | $1.40 / $4.40 | 
| **GLM-5.3-Flash** | 26 Aug 2026 | 320B / 18B | 1,048,576 | Text + image + video | Yes — `zai-org/GLM-5.3-Flash` | **MIT** | $0.15 / $0.50 | 
| **GLM-5.3-FlashX** | 18 Sep 2026 | 320B / 18B *(same as Flash)* | 1,048,576 | Text + image + video | **No** — no separate repo | MIT weights underneath | $0.37 / $1.25 | 
| **GLM-5.3-Prime** | 23 Sep 2026 | 753B / 40B *(same as GLM-5.3)* | 1,000,000 | Text | **No** — no separate repo | Bespoke `glm-5.3` underneath | $2.80 / $8.80 | 

Two things jump out of that table. First, the two September additions are the only rows where you cannot download anything — the `zai-org` organisation on Hugging Face stops at GLM-5.3 and GLM-5.3-Flash, both published 25 August 2026. Nothing named Prime or FlashX exists there. Second, Prime is the most expensive model in the family and it is *not* the most capable one, because it is the same weights as a model costing half as much.

Note also the oddity in Prime's context window: 1,000,000 tokens flat against 1,048,576 everywhere else, so a prompt that fits GLM-5.3 can in principle overflow Prime.

## What is GLM-5.3 Prime?

OpenRouter's listing, where the model went live on 23 September 2026 under the slug `z-ai/glm-5.3-prime`, is unambiguous: "GLM-5.3-Prime is the high-speed variant of Z.ai's GLM-5.3, inheriting its full capabilities while delivering 1.5–2× the output throughput through inference acceleration."

"Inheriting its full capabilities" is the operative phrase. Prime is not a new post-training run, not a larger checkpoint, and not a different base. It targets coding and agentic work — specifically long-horizon multi-turn agent orchestration, real-time conversation, and streaming code generation, all cases where tokens-per-second is the bottleneck rather than reasoning depth.

Specs: 1M-token context, up to 131,072 output tokens, text in and text out only. Reasoning is always on and cannot be disabled; `low`, `high` and `max` effort levels are supported, with `max` as the default — the same arrangement as GLM-5.3 itself.

How Prime is distributed is itself revealing. It appears nowhere on `docs.z.ai` — not on the pricing table, not on the GLM-5.3 model page, and with no dedicated page in the docs sitemap. Meanwhile the only provider OpenRouter lists for it is **Alibaba**, not Z.AI, whereas FlashX is served by Z.AI directly and does appear on Z.ai's price list. The practical read: Prime is currently a partner-hosted acceleration tier rather than a first-party Z.ai API product. Put bluntly, **OpenRouter is the only surface on which GLM-5.3-Prime can be verified at all** — no docs.z.ai page, no row on Z.ai's pricing table, no Hugging Face repo, and no Artificial Analysis record. Treat its specifications as single-sourced until Z.ai documents it.

If you are weighing Prime against the base model, our GLM-5.3 launch guide covers what the underlying checkpoint actually does.

## What is GLM-5.3 FlashX?

FlashX follows the same pattern one tier down. OpenRouter lists it from 18 September 2026 as "the high-speed variant of Z.ai's GLM-5.3-Flash, a native multimodal model delivering inference speeds of up to 200 tokens/s," and makes the shared-weights point explicitly: "built on the same hybrid sparse and linear attention architecture (320B total parameters, 18B active)."

Z.ai's own documentation says the same thing in its own words: "GLM-5.3-FlashX is now live, delivering inference speeds of 200 tokens/s for faster responses and a smoother experience." Context window and max output are identical to Flash at 1M and 128K respectively, and it keeps the full multimodal input set — video, image, text and file.

One operational catch worth knowing before you plan around it: Z.ai's docs note that **FlashX is not yet available on the GLM Coding Plan** subscription, whereas standard GLM-5.3-Flash is. If your workflow runs through that plan rather than metered API billing, FlashX is not reachable yet.

The "X" suffix is not new, incidentally, and that is the clearest evidence this is a deliberate product line rather than a one-off. Z.ai's pricing page still lists GLM-4.5 at $0.60/$2.20 alongside GLM-4.5-X at $2.20/$8.90, and GLM-4.5-Air at $0.20/$1.10 alongside GLM-4.5-AirX at $1.10/$4.50. In every case X means the same model served faster for several times the price. FlashX and Prime are that pattern applied to the GLM-5.3 generation.

## How do they score on independent benchmarks?

This is where the serving-tier framing stops being pedantic and starts saving you money: **neither Prime nor FlashX has an independent benchmark score, because there is nothing new to score.** Artificial Analysis evaluates GLM-5.2, GLM-5.3 and GLM-5.3-Flash; it lists no separate entry for either September variant. Same for the weights-based leaderboards, which need a downloadable checkpoint.

Here are the independent numbers that do exist, all measured by Artificial Analysis on **Intelligence Index v4.3.2** (October 2026). Two things to read carefully. AA rebased the index from v4.1.1 to v4.3.2 and there is no conversion between the two, so these are not comparable with any older GLM score you may have seen. And AA publishes two rank denominators — **/224** for the whole field and **/118** for open-weights models only; every rank below is the open-weights field, which is the relevant one for GLM:

| Model (effort tier) | AA Index v4.3.2 (open-weights rank) | Cost per Index task | Output speed | Time to first token | 
|---|---|---|---|---|
| GLM-5.2 (Max) | 33.71 — #13 / 118 | $1.47 | 94.3 tok/s | 2.93 s | 
| GLM-5.3 (Max) | **44.78 — #2 / 118** | $2.01 | 71.9 tok/s | 2.73 s | 
| GLM-5.3-Flash | 41.81 — #4 / 118 | **$0.25** | 51.4 tok/s | 3.31 s | 
| GLM-5.3-FlashX | *Not separately evaluated — same weights as Flash* |  |  |  | 
| GLM-5.3-Prime | *Not separately evaluated — same weights as GLM-5.3* |  |  |  | 

Read the speed column next to the vendor's throughput claims and the value case gets clearer. Artificial Analysis measured GLM-5.3 at 71.9 tokens per second; Prime's claimed 1.5–2× would put it somewhere around 108–144 tok/s. Flash measured 51.4 tok/s, which AA flagged as "notably slow" for its class, against FlashX's claimed "up to 200 tok/s" — the biggest proportional jump in the family, and the one most likely to be worth paying for.

The cost column is the one that should change your default. AA also publishes what it spent running the index, and **GLM-5.3-Flash completed an Index task for $0.25 against $2.01 for GLM-5.3 at `max` effort** — eight times cheaper for 2.97 index points less. Flash's whole-suite bill was $280.28. One caveat on that absolute figure: AA runs different task counts per model, so suite totals are not comparable between rows and only the per-task column is a fair ratio.

On Z.ai's own reported numbers, kept separate as they should be, GLM-5.3's gains over GLM-5.2 came entirely from post-training — the model card states plainly that "GLM-5.3 uses the same base model as GLM-5.2." Z.ai reports Terminal Bench 3.0 at 28.3 against GLM-5.2's 4.6, CyberGym at 84.5 against 77.2, and Toolathlon Verified at 73.0 against 59.9. One figure is third-party-run: the GDPval-AA v2 score of 1769 (against 1508) was, per Z.ai's own footnote, evaluated by Artificial Analysis.

If you want the GLM-5.3 vs GLM-5.2 comparison in depth, we covered it in GLM-5.3-Flash vs GLM-5.2.

## What do they cost, and did GLM-5.3-Flash's promotional pricing end?

Yes — and this is the most actionable thing on this page if you budgeted on Flash earlier in the year.

GLM-5.3-Flash launched with a promotion that halved its rates to roughly $0.075/$0.25 per 1M tokens, running until 24:00 on 9 September 2026 (UTC+8). **That promotion has expired.** As of today, both Z.ai's pricing page and OpenRouter show the full list rate of $0.15 / $0.50 — so the effective cost of GLM-5.3-Flash has doubled for anyone who modelled spend against promo pricing. Nothing was announced as a price rise, which is exactly why a lot of coverage still quotes the half-price figures.

| Model | Input / 1M | Output / 1M | Cached input read | Multiple vs its base tier | 
|---|---|---|---|---|
| GLM-5.3-Flash | $0.15 | $0.50 | $0.03 | — | 
| GLM-5.3-FlashX | $0.37 | $1.25 | $0.09 | **2.5×** Flash | 
| GLM-5.2 | $1.40 | $4.40 | — | — | 
| GLM-5.3 | $1.40 | $4.40 | — | — | 
| GLM-5.3-Prime | $2.80 | $8.80 | $0.56 | **2.0×** GLM-5.3 | 

Both acceleration tiers keep a roughly 76–80% discount on cached input reads, so prompt-caching economics survive the upgrade. That is the saving grace for agent loops that re-send a large stable system prompt on every turn.

Separately, there *is* a live Z.ai campaign on Flash right now, but it is a quota event rather than a discount, and it is easy to confuse with the expired price promo. Z.ai's developer notice describes zero quota consumption for unlimited usage via ZCode/AutoClaw and doubled quota via other agents, available in a nightly window from 23:00 to 09:00 Singapore time. It started 3 September 2026 and runs to **7 October 2026**, already extended once from an original 20 September end date. If you are on a GLM Coding Plan, that window closes in a couple of days.

## Are GLM-5.3 Prime and FlashX open source?

No, and this deserves a careful answer because the GLM audience self-hosts more than most.

There is no `zai-org/GLM-5.3-Prime` and no `zai-org/GLM-5.3-FlashX` on Hugging Face. The newest GLM text checkpoints in that organisation are GLM-5.3 and GLM-5.3-Flash, both published 25 August 2026. Prime and FlashX are API-only. OpenRouter's metadata reflects this too — neither carries a Hugging Face repo ID, while GLM-5.3 and GLM-5.3-Flash both do.

The licences of the models *underneath* them differ sharply, and this is where a lot of reporting has been wrong:

- **GLM-5.3-Flash is genuinely MIT.** The model card declares`license: mit` with no additional terms. FlashX sits on these weights.
- **GLM-5.3 is not MIT.** Its card declares`license: other` with`license_name: glm-5.3` , a bespoke Z.ai licence. Prime sits on these weights.

The bespoke GLM-5.3 licence is permissive for almost everyone — it grants use, modification, fine-tuning, distribution and commercial sale. The catch is clause 2, which defines "Model as a Service" and then adds: if a licensee or its affiliates operates a MaaS business and aggregate revenue "exceeds 10 billion US dollars... in total over any consecutive 12 months," the licensee "must pass Z.AI's security review before using the Software or its derivative works for any commercial purpose," with scope and method "reasonably determined by Z.AI."

In practice that gate catches hyperscalers and nobody else. GLM-5.2, by contrast, is plain MIT — so the family actually moved *away* from MIT at the top end between 5.2 and 5.3, while keeping MIT on the Flash branch.

The honest summary for self-hosters: you cannot download either September variant, and the speed advantage you would be chasing is a property of the serving stack, not of the weights. Downloading GLM-5.3 and running it yourself does not get you Prime's throughput.

## Which GLM model should you use?

The decision rule, in order:

1. **Start with GLM-5.3-Flash.** At $0.15/$0.50 it scores 41.81 on AA Intelligence Index v4.3.2 against GLM-5.3 (Max)'s 44.78 — under three index points, for roughly a tenth of the per-token price and an eighth of AA's measured cost per task, plus multimodal input and a clean MIT licence. For most workloads this is the correct default, and the burden of proof sits with anything more expensive.
2. **Move to GLM-5.3 only if you measured a quality gap.** It is the strongest model in the family on independent scoring (#2 of 118 open-weights models on v4.3.2) and Z.ai's reported Terminal Bench 3.0 jump is real, but you are paying roughly 9× Flash's rate for it.
3. **Add FlashX when latency is the complaint, not quality.** Flash's measured 51.4 tok/s is the family's weak spot. FlashX is the family's best speed-per-dollar upgrade: 2.5× the price for a claimed near-4× throughput. Check your plan supports it first.
4. **Reach for Prime last, and only for interactive work.** 2× the price for 1.5–2× throughput is close to break-even per token delivered, so it only pays when wall-clock time has independent value — a developer waiting on streaming output, or a real-time agent loop.
5. **Skip GLM-5.2 for new work.** Same $1.40/$4.40 as GLM-5.3, 11 index points lower on AA v4.3.2 (33.71 against 44.78, both at`max` ), and its own Hugging Face card now points to GLM-5.3 as the successor. Its one remaining edge is the MIT licence at the 753B tier.

| If your situation is… | Use | Why | 
|---|---|---|
| High-volume batch coding, cost-sensitive | GLM-5.3-Flash | 41.81 AA v4.3.2 at $0.15/$0.50 | 
| Hardest agentic or security work | GLM-5.3 | Top independent score in the family | 
| Images or video in the prompt | Flash or FlashX | Only multimodal tiers | 
| Interactive IDE streaming feels sluggish | GLM-5.3-FlashX | Up to 200 tok/s claimed | 
| Real-time agent orchestration at top quality | GLM-5.3-Prime | 1.5–2× GLM-5.3 throughput | 
| Self-hosting, want MIT, need max capability | GLM-5.2 or GLM-5.3-Flash | Only MIT options | 
| On the GLM Coding Plan today | GLM-5.3-Flash | FlashX not yet on the plan | 

## Can you self-host Prime or FlashX?

Not as such. What you can self-host is the checkpoint underneath, and then your throughput is whatever your own serving stack delivers.

Both downloadable models have first-class support across SGLang, vLLM, TokenSpeed, Transformers, KTransformers and Unsloth, and GLM-5.3 additionally documents Ascend NPU deployment. GLM-5.3-Flash is much the easier target at 320B total / 18B active versus 753B / 40B, and its hybrid sparse-plus-linear attention design is specifically aimed at cutting long-context serving cost — which is what makes a 1M-token window tractable outside a datacentre.

If you are going down this road, start with our guide to running GLM-5.3-Flash locally, and the full GLM-5.3-Flash guide for architecture and benchmark detail. For the previous generation's licensing and hardware picture, see the GLM-5.2 complete guide.

## FAQ

### What is GLM-5.3 Prime?

GLM-5.3 Prime is an accelerated serving tier of GLM-5.3, listed on OpenRouter on 23 September 2026. It runs the same weights as GLM-5.3 with 1.5–2× the output throughput through inference acceleration, offers a 1M-token context and up to 131,072 output tokens, and is text-only. It is not a new or stronger checkpoint.

### What is GLM-5.3 FlashX?

GLM-5.3 FlashX is the high-speed tier of GLM-5.3-Flash, listed 18 September 2026. It serves the same 320B-total / 18B-active multimodal weights at up to 200 tokens per second, keeping Flash's 1M-token context, 128K max output, and video, image, text and file inputs. Z.ai notes it is not yet available on the GLM Coding Plan.

### How much do GLM-5.3 Prime and FlashX cost?

Prime is $2.80 per 1M input tokens and $8.80 per 1M output tokens — exactly double GLM-5.3's $1.40/$4.40. FlashX is $0.37/$1.25, about 2.5× GLM-5.3-Flash's $0.15/$0.50. Cached input reads are $0.075 for FlashX on Z.ai's own pricing table — an 80% discount — and $0.56 for Prime, though that one is OpenRouter-only since Prime has no Z.ai price row.

### Are GLM-5.3 Prime and FlashX open source?

No. Neither has weights published on Hugging Face — the `zai-org` organisation stops at GLM-5.3 and GLM-5.3-Flash, both published 25 August 2026. Both September variants are API-only serving tiers. The weights beneath them are downloadable, but the speed advantage is not, since it comes from the serving stack.

### Which GLM model should I use?

Default to GLM-5.3-Flash: it scores 41.81 on Artificial Analysis's Intelligence Index v4.3.2 against GLM-5.3 (Max)'s 44.78, at roughly a tenth of the price, and it is MIT-licensed and multimodal. Move up to GLM-5.3 only if you have measured a quality gap, add FlashX if latency rather than quality is the problem, and consider Prime only for interactive or real-time work.

### Is GLM-5.3 Prime better than GLM-5.3?

Not in quality. Prime inherits GLM-5.3's capabilities unchanged, so its intelligence is identical — Artificial Analysis does not score it separately because there is nothing new to score. It is faster and twice the price, and its context window is marginally smaller at 1,000,000 tokens versus 1,048,576. Prime is better only if throughput is what you need.

### Did GLM-5.3-Flash's promotional pricing end?

Yes. The launch promotion that halved Flash's rates to about $0.075/$0.25 per 1M tokens expired at 24:00 on 9 September 2026 (UTC+8). Z.ai's pricing page and OpenRouter now both show the full list rate of $0.15/$0.50, so effective costs doubled. A separate quota campaign — zero or doubled quota in a nightly 23:00–09:00 Singapore-time window — runs until 7 October 2026.

### Is GLM-5.3 MIT-licensed like GLM-5.3-Flash?

No, and this is widely misreported. GLM-5.3-Flash is MIT. GLM-5.3 ships under a bespoke `glm-5.3` licence that is permissive for most users but requires any Model-as-a-Service operator whose group revenue exceeds $10 billion over any consecutive 12 months to pass a Z.ai security review before commercial use. GLM-5.2 is plain MIT.

## What should you take away?

Z.ai now sells the same two GLM-5.3 checkpoints at four price points, and the suffixes encode speed, not intelligence. Flash and GLM-5.3 are the models; FlashX and Prime are those models served faster for 2–2.5× the money. Pick the base model first and treat acceleration as a separate, measurable purchase — and if you costed GLM-5.3-Flash before 9 September, reprice it, because the promo is gone.
