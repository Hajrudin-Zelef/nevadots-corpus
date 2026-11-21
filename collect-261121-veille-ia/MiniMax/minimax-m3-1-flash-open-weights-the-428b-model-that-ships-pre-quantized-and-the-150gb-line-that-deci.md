---
id: collect-261121-veille-ia/minimax/minimax-m3-1-flash-open-weights-the-428b-model-that-ships-pre-quantized-and-the-150gb-line-that-deci
title: "MiniMax M3.1 Flash Open Weights: The 428B Model That Ships Pre-Quantized, and the 150GB Line That Decides Who Runs It (2026)"
domain: minimax
role: reference
task: article
actors: ["MiniMax"]
dates: ["2026-10-23"]
keywords: ["model", "flash", "minimax", "open", "weights", "ships", "pre-quantized", "line"]
source: https://www.runaihome.com/blog/minimax-m3-1-flash-local-ai-hardware-guide-2026/
source_anchor: ""
source_lines: [1, 147]
section: "MiniMax"
---
# MiniMax M3.1 Flash Open Weights: The 428B Model That Ships Pre-Quantized, and the 150GB Line That Decides Who Runs It (2026)

**TL;DR**: MiniMax released M3.1 Flash open weights on October 23, 2026: a 428B-total / 23B-active multimodal MoE with 1M context that ships already quantized (NVFP4 experts, MXFP8 dense) at 252.4GB. Unslothâs 2-bit build is 143.9GB with 91.5% top-1 fidelity â the first frontier-agent model a 256GB Mac Studio holds with room to spare. llama.cpp mainline support is still an open PR.

|  | API ($0.15/$0.60) | Mac Studio M5 Ultra 256GB | 2Ã RTX PRO 6000 (192GB) | 
|---|---|---|---|
| Best for | Everyone this week | Running the 2-bit at home, quietly | CUDA stack, concurrency | 
| Price / Cost | ~$0.22 per agent task measured | $9,499 | ~$28,000â$40,000 | 
| The catch | Your data leaves the house | NVFP4 CPU-fallback bug, PR not merged | Costs 3-4Ã the Mac for the same fit | 

**Honest take**: Use the API this week â at $0.15/$0.60 per million tokens itâs absurdly cheap, and the local toolchain is five days old with open bugs. But if you already own (or were about to order) a 256GB M5 Ultra, this is the model that finally justifies the memory: wait for the llama.cpp merge, then run the 143.9GB 2-bit.


MiniMax put the M3.1 Flash weights on Hugging Face and ModelScope on October 23, four days after the API launch. We covered MiniMax M3 in June and the verdict then was blunt: a 428B model with a 143GB floor at 2-bit was a rent-or-API model for almost everyone. M3.1 Flash has the same 428B/23B-active shape â but three things changed the home-lab math since June: the shipped weights are natively quantized, a 2.3GB speculative-decoding drafter comes in the box, and the hardware that can hold it (the 256GB M5 Ultra) actually exists now. Check your own machine against the VRAM calculator before you plan anything around this model.

## What actually shipped on October 23

The repository is 252.4GB across 48 shards, plus a 2.3GB DSpark speculative-decoding drafter (per Orca Routerâs teardown and the llama.cpp support PR). Specs, from the release coverage:

- **428B total parameters, 23B active per token** â same MoE shape as M3
- **1M-token context window** , with sparse attention in every layer
- **Multimodal input** : text, image, and video (output is text)
- **Quantization-aware trained** : routed experts ship in NVFP4, dense layers and attention in MXFP8. There is no BF16 âoriginalâ to wait for a quant of â the 252.4GB file*is* the original.
- **License** : not Apache, not MIT. MiniMaxâs own community license requires attribution on commercial deployments and written authorization once your annual revenue passes $20 million. For a home lab this is a non-issue; for a product itâs a real clause to read.

The API launched October 19 at $0.15 per million input tokens / $0.60 output / $0.015 cached â half of M3âs $0.30/$1.20. Artificial Analysis measured about $0.22 per Intelligence Index task against roughly $0.51 for M3, a 57% drop, which is the independent number worth remembering; MiniMaxâs own â70% cheaper agent usageâ claim is vendor math.

Vendor benchmarks (self-reported, no independent replication yet): 73.7% DeepSWE v1.1, 61.7 OSWorld V2, 82.2 CyberGym, 29.6 Agentsâ Last Exam. Treat all of these the way we treated Kimi K3.1 Proâs launch numbers â as claims, until third parties run them.

## The pre-quantized twist: why âwait for the Q4â doesnât apply here

With a normal BF16 release, the home-lab playbook is: ignore the 850GB original, wait 48 hours for a Q4 GGUF at roughly 60% size reduction. M3.1 Flash breaks that playbook because the training already did the compression. Unslothâs Q4_K_XL build is 249.4GB â within 3GB of the 252.4GB native checkpoint, because there was almost nothing left to squeeze at 4-bit. The llama.cpp PR keeps the routed experts in their native NVFP4 rather than re-quantizing them, and the converted GGUF matched the reference GPU implementation on 95.5â99.1% of top-1 tokens.

The real compression happens below 4-bit, and this is where the QAT pays off:

| Build | File size | Unsloth top-1 fidelity | Holds it (weights + headroom) | 
|---|---|---|---|
| Native NVFP4/MXFP8 | 252.4GB | reference | 512GB Mac (not orderable), 4Ã RTX PRO 6000, servers | 
| UD-Q4_K_XL | 249.4GB | 98.0% | Same as native â skip it, run the original | 
| UD-Q2_K_XL | 143.9GB | 91.5% | 256GB M5 Ultra, 192GB unified x86, 2Ã RTX PRO 6000, ~150GB+ RAM | 
| UD-IQ1_M | 129.4GB | ~89.9% | Same tier as Q2 â the 14GB saved buys little | 

File sizes and fidelity numbers are from Unslothâs run guide; MiniMax itself amplified the 2-bit build with ârun on 150GB RAM.â A 91.5% top-1 agreement at 2-bit is unusually high â compare the hand-wringing we did over 2-bit quality in the MiniMax M3 guide â and itâs high *because* the experts were trained to survive 4-bit from day one, so the 2-bit pass starts from a model that already knows how to be small.

## The 150GB line: what fits and what doesnât

Hereâs the part that decides whether this article is about your hardware or someone elseâs.

**128GB machines are out.** The smallest build, IQ1_M at 129.4GB, is bigger than the ~120GB a Strix Halo box under Linux GTT can allocate, and bigger than the ~96GB macOS wires by default on a 128GB Mac. No flag fixes arithmetic. The entire 128GB unified-memory class â EVO-X2, Framework Desktop, DGX Spark, 128GB Macs â misses by under 10GB, which is the cruelest kind of miss.

**192GB unified x86 is the cheapest resident fit.** The 2-bitâs 143.9GB sits inside the ~160GB GPU-allocatable window of the new 192GB LPDDR5X machines we covered in the Tiiny AI Pocket Lab piece ($6,799 DIY â but note batch 1 sold out and units ship in November, so this is a fit on paper until stock exists). The catch is bandwidth: 273 GB/s, more on that below.

**A 256GB Mac Studio M5 Ultra holds the 2-bit with ~100GB to spare.** 143.9GB of weights fits under even the default macOS wired-memory ceiling (75% of 256GB â 192GB), leaving real room for KV cache. At $9,499 it is the cheapest *currently orderable* single machine that runs a 400B-class agent model resident â the 512GB config that would hold the native 252.4GB checkpoint still has no order page as of October 28.

**Discrete GPUs: this is RTX PRO 6000 territory, twice.** Two 96GB cards pool 192GB â enough for the 2-bit. At $13,998â$19,999 per card (street, SeptemberâOctober 2026), thatâs a $28,000+ pair to match what the $9,499 Mac holds. A used RTX 3090 quad at ~$1,456â$1,558 each gives you 96GB of VRAM â not even the IQ1_M fits resident. The hybrid path (experts in system RAM, llama.cppâs new MoE expert cache on the GPU) is exactly the workload that feature was built for, but youâd need ~150GB of DDR5 on top of the card, and at 2026 DRAM prices thatâs a four-figure line item before you benchmark a single token.

## How fast? Nobody has published a number yet â hereâs the ceiling math

Five days after the weights dropped, we could not find a single published tokens-per-second measurement on consumer or prosumer hardware â not on r/LocalLLaMA, not in the llama.cpp PR (which reports only accuracy checks, no speed). Thatâs itself a signal: the toolchain isnât settled enough for people to benchmark.

What we can do is bound it. Decode speed is memory-bandwidth-limited: divide bandwidth by bytes read per token. At 2-bit, 143.9GB / 428B parameters â 0.34 GB per billion, and ~23B active parameters means roughly **7.7GB read per token** (our derivation â treat every number in this section as an estimate until measurements land):

| Hardware | Bandwidth | Ceiling (2-bit) | Realistic guess | 
|---|---|---|---|
| Mac Studio M5 Ultra (1.2 TB/s) | 1,200 GB/s | ~155 tok/s | likely 50â110 tok/s | 
| Mac Studio M3 Ultra used (819 GB/s) | 819 GB/s | ~106 tok/s | likely 35â75 tok/s | 
| 192GB x86 unified (273 GB/s) | 273 GB/s | ~35 tok/s | likely 12â25 tok/s | 

We widened the usual 70â95%-of-ceiling band downward because sparse attention, MoE routing overhead, and a five-day-old implementation all tax early numbers â MiniMax M3 on Apple Silicon landed well under its ceiling at launch, then climbed with llama.cpp releases. Even the pessimistic end is usable: 12 tok/s is above comfortable reading speed, and the bundled DSpark drafter exists precisely to multiply these numbers for agent-style output.

## Running it today: the unmerged-PR reality

Mainline llama.cpp cannot load these GGUFs yet. PR #31192 â the converter for the mixed NVFP4/MXFP8 checkpoint, sparse attention on every layer, and the DSpark drafter â was opened October 26 and is in review, not merged. Until it lands youâre on Unslothâs fork, the same early-adopter position Kimi K3 users were in for weeks. The intended invocation, from the PR:

`llama-server -m MiniMax-M3.1-Flash-UD-Q2_K_XL.gguf --jinja -fa on -np 1`
And with the speculative drafter:

```
llama-server -m MiniMax-M3.1-Flash-UD-Q2_K_XL.gguf --jinja -fa on -np 1 \
  --spec-type draft-dspark -md MiniMax-M3.1-Flash-DSpark-BF16.gguf --spec-draft-p-min 0
```
Two traps are already documented:

**Trap 1 â sparse attention silently degrades to dense.** The sparse-attention path only engages with flash attention on (`-fa on`) and a single slot (`-np 1`, or `--no-kv-unified`). Get the flags wrong and the model still runs â it just falls back to dense attention with a warning in the log, and at long context your prefill times balloon. If a 100K-token prompt feels impossibly slow, read the server log before blaming the hardware; the fallback warning is the tell.

**Trap 2 â Apple Silicon NVFP4 CPU fallback.** Unslothâs tracker (issue #15409) reports NVFP4 operations falling back to CPU on Apple Silicon even with every layer offloaded to GPU â which would gut exactly the M5 Ultra scenario this model is best suited for. If your Mac runs it far below the ceiling math above, this bug is the likely reason. Watch the issue and the PR before spending a weekend tuning; this is a wait-for-the-merge problem, not a your-config problem.

If youâd rather let someone else carry the integration risk, Ollama and LM Studio support will follow the mainline merge, as it did for MiniMax M3 â see our self-hosting coverage on aifoss.dev for how that pipeline usually sequences.

## What to actually buy

Prices as of October 2026, all verified above or in the linked articles:

| Your situation | The machine | Price | Where | 
|---|---|---|---|
| Want M3.1 Flash output tonight | MiniMax API / OpenRouter | $0.15/$0.60 per M tokens | platform.minimax.io | 
| Want it resident at home, orderable today | Mac Studio M5 Ultra 256GB | $9,499 | Check price | 
| Need CUDA + the native checkpoint | 2â3Ã RTX PRO 6000 Blackwell 96GB | $13,998â$19,999 per card | Check price | 
| Want to test the real thing before any purchase | Rented 2Ã H200 (282GB), ~$4.39/hr per GPU | ~$8.80/hr | RunPod | 
| 24GB card, honest with yourself | Keep running 27â35B models locally, use the API for this one | $0 | What 24GB runs best | 

Two rows deserve a sentence. The rental row is the right first move even if you intend to buy: two hours on rented H200s tells you whether M3.1 Flashâs agent behavior is actually better *for your tasks* than the Qwen3.6-35B you already run free â before you commit $9,499. And the $0 row is where most readers honestly land: a model you run twice a month does not justify new hardware, and at $0.22 per task the API break-even against a $9,499 machine is over 40,000 agent tasks before electricity.

If youâre starting from zero hardware entirely, the GPU buying guide is the better entry point than any 428B model.

## Where this leaves the frontier-at-home question

October 2026 now has a clear pattern: Kimi K3.1 Pro (weights November 4) needs ~1.56TB and isnât a home model under any quantization; Mistral Large 4âs promised weights havenât landed; and M3.1 Flash ships at 252GB native with a credible 144GB 2-bit. MiniMax is the only frontier lab currently shipping agent-class weights that a single desk-side machine can hold. Whether thatâs a deliberate strategy (QAT from day one strongly suggests it) or a side effect of optimizing their own serving costs, home labs benefit either way. If you orchestrate coding agents against local backends, the harness side of that stack is covered on aicoderscope.com.

The June M3 verdict was âuse the API, wait for distills.â The M3.1 Flash verdict is narrower and more interesting: use the API *this week*, and if you own 256GB of unified memory, watch one GitHub PR â because the day it merges, the biggest model your Mac has ever held becomes a download away.

## FAQ

**Is the 2-bit quality actually usable, or is 91.5% top-1 marketing?**
91.5% top-1 agreement (Unslothâs own seven-domain evaluation) is the published number, and itâs measurably better than typical 2-bit results on non-QAT models. But top-1 agreement is not task accuracy â nobody has published SWE-bench or agent-task numbers for the 2-bit build. If your use case is high-stakes code edits, benchmark it against the API output on your own tasks first.

**Why canât my 128GB Strix Halo / DGX Spark / Framework Desktop run the 1-bit?**
The IQ1_M file is 129.4GB and the usable GPU-memory ceiling on every 128GB machine is 96â120GB depending on OS and kernel flags. Thereâs no smaller official build, and below ~1.5 bits quality falls off a cliff â a hypothetical 100GB build would likely not be worth running.

**Does the 1M context actually work locally?**
The architecture supports it, but KV cache at 1M tokens adds tens of GB on top of the weights, and Unsloth recommends starting at 16K context and raising it as memory allows. On a 256GB Mac with the 2-bit build you have ~45GB of practical headroom under the default wired limit â six figures of context, not seven. Raise the ceiling with `sudo sysctl iogpu.wired_limit_mb` if you need more.

**Should I wait for the 512GB Mac Studio to run the native checkpoint?**
Appleâs late-October window for the 512GB M5 Ultra is closing with no order page and reports of January delivery estimates. The 2-bit on a 256GB machine at 91.5% fidelity is available now; waiting months for the 98%-fidelity native fit is a bad trade unless you need it for something else (like MiniMax M3âs 265GB Q4, which genuinely requires 512GB).

**Can I fine-tune it at home?**
No. Fine-tuning needs the model in higher precision plus optimizer state â multiples of the 252GB inference footprint. This is inference-only hardware math; tuning stays on rented clusters.

## Sources

*Last updated October 28, 2026. Prices and specs change; verify current rates before purchasing. Speed figures in the ceiling-math section are derivations, not measurements â flagged as such in the text.*

## Recommended Gear

Products linked in this article:

- Mac Studio M5 Ultra 256GB â $9,499, the cheapest orderable machine that holds the 2-bit build resident
- RTX PRO 6000 Blackwell 96GB â $13,998â$19,999/card; two pool 192GB for the CUDA path
- Used RTX 3090 24GB â $1,456â$1,558; not for this model, but still the value king for the 27â35B class youâll actually run daily

Was this article helpful?

Thanks for the feedback â it helps improve future articles.

## Get the numbers before you buy

New GPU and mini-PC benchmarks, VRAM thresholds, and price checks â sent when there's something worth acting on, not on a schedule. No spam, unsubscribe anytime.
