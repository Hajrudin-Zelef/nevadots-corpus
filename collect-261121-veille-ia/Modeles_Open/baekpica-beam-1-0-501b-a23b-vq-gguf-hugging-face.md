---
id: collect-261121-veille-ia/modeles-open/baekpica-beam-1-0-501b-a23b-vq-gguf-hugging-face
title: "Baekpica/Beam-1.0-501B-A23B-VQ-GGUF · Hugging Face"
domain: modeles-open
role: reference
task: article
actors: []
dates: ["2026-10-21"]
keywords: ["bf16", "layers", "recipe", "original", "below", "beam-1", "token", "experts"]
source: https://huggingface.co/Baekpica/Beam-1.0-501B-A23B-VQ-GGUF
source_anchor: ""
source_lines: [1, 487]
section: "Modeles_Open"
---
**The files can change.** This line of conversions is still being researched,
so any file here may be replaced by a new revision. Replacements so far:
**2026-10-21 — the 49 routed-expert parts were re-converted with
calibration-weighted Lloyd** (same format, recipe id, bits and runtime; KL
to the original halved, below). The
first build (2026-10-20, weights-only codebooks) is commit
`77a2c5f43b66693cdf8a40b64894b8b5d11f52ad`. To keep a fixed build, download
a pinned commit:
`hf download Baekpica/Beam-1.0-501B-A23B-VQ-GGUF --revision <commit>`.

**Problems or odd output?** Feel free to post them in the
Community tab,
even as rough notes. The prompt, sampling settings and runtime commit help
most.


**Reflection's 501B Beam with its full 1,048,576-token context, sized for one
128 GB DGX Spark.** The 1.0 TB BF16 checkpoint becomes about **99.9 GB** of
GGUF parts: 98.3 GB that stay resident and a 1.56 GB BF16 token table that is
read in rows from SSD. The routed experts use YoungAi's 8-dimensional vector
quantization (12-bit, and 13-bit on the eight bottom MoE layers), with
codebooks, assignments and row gains fitted to activation statistics from a
calibration corpus run through the BF16 original (an importance matrix for
VQ); everything the model needs to read on every token stays at Q4_K or
source precision.
Reflection's DFlash2 drafter is included, converted for the same runtime.

**Status, 2026-10-21:** ds4-dfm-rs
serves this release on one DGX Spark (GB10) from `main` `d70afd6a` (PR #105,
optimized in PR #106): CUDA kernels for Beam's GVQ experts, gated GQA with
sliding-window and FP8 full-layer KV, Harmony output with typed tool calls,
and DFlash2 speculative decoding with the drafter in
`dflash2/`.
**Measured, 2K–64K: 1,241.2 tok/s prefill and 25.23 tok/s decode** (26.6 at
2K, 23.7 at 64K; it was 830.4 and 15.86 before the optimization round);
DFlash2 takes greedy chat from 26.2 to 32.0 tok/s
(below). The full 1M context plans in one bank
(115.4 of 117.2 GiB, `--check-config`); no run longer than 64K has been made
yet. Against the BF16 original, the release's bytes agree on the next token
**87.7%** of the time (mean KL **0.144**; first build 82.9%, KL 0.295) over
the same 5,491 held-out tokens (below).
**On Reflection's own benchmark settings the release keeps 95–98% of the
original's reasoning, coding and instruction-following scores: GPQA Diamond
88.6 vs 90.6 (97.8%), AIME 2026 93.8 vs 97.7 (96.0%), IFBench 78.0 vs 79.6
(98.0%), AA-LCR 77.0 vs 78.2 (98.5%), LiveCodeBench 84.0 vs 88.3 (95.1%).
Scientific coding and factual recall lose more:
SciCode 43.3 vs 49.2 (88.0%), AA-Omniscience index −4.7 vs 10.8**
(below); the card's other static
benchmarks are running.
The engine and speed numbers on this card were measured with the first
build's parts; the re-converted parts keep the byte layout, bits and recipe
id, so the runtime loads them unchanged, but the GB10 numbers have not been
re-measured yet.


| Source | `reflection/Beam-1.0-501B-A23B` at`78f8f654d40a40919b8e9954b40dd879ecb9a692` (BF16, 188 shards) | 
| Recipe | `beam1-vq8-mix8r13-41r12-q4k-embssd-v1` (RECIPE.md,`recipe.json` ) | 
| Routed experts | 49 MoE layers × 256 experts × gate/up/down. 8-dim VQ, one E4M3 codebook per layer shared by all experts and all three matrices, one F16 gain per row. **Layers 3–10: 13-bit (K=8192, 12-bit stream + 1-bit plane), layers 11–51: 12-bit (K=4096)** — 1.625 / 1.5 index bits per weight.**Calibration-weighted Lloyd** (per-expert column importance, weighted least-squares row gains; below). | 
| Attention, dense FFN (layers 0–2), output head | Q4_K (GGML reference encoder, no imatrix) | 
| Kept at source precision | Router weights and `e_score_correction_bias` , attention gate bias, q/k norms, the four sandwich norms per layer, final norm | 
| Token embedding | Original BF16, in its own part, read in rows from SSD (not resident) | 
| Context | **1,048,576 tokens** , the model's own (9 NoPE full-attention layers + 43 sliding-window-512 layers; no RoPE scaling involved). Needs FP8 KV on the full layers; see the budget below. | 
| Chat template | Original *Modified Harmony* template, unchanged (`analysis` /`final` channels, XML tool calls,`reasoning_effort` 8–512) | 
| Benchmarks vs the original (GB10, ds4-dfm-rs) | **GPQA Diamond 88.6 vs 90.6 (97.8%), AIME 2026 93.8 vs 97.7 (96.0%), IFBench 78.0 vs 79.6 (98.0%), AA-LCR 77.0 vs 78.2 (98.5%), LiveCodeBench 84.0 vs 88.3 (95.1%)** ; SciCode 43.3 vs 49.2 (88.0%), AA-Omniscience index −4.7 vs 10.8; others running (below) | 
| Fidelity (reference forward, release bytes vs BF16) | **87.7%** same next token, KL**0.144** , NLL 1.244 → 1.340 over 5,491 held-out tokens (first build: 82.9%, 0.295, 1.243 → 1.463) | 
| Drafter | Reflection's DFlash2 block drafter (1.93B parameters), converted to Q8_0 in `dflash2/` (2.12 GB); one sequence, any temperature (drafts verified against the request's sampler) | 
| zchain / GR-RB sidecars | None | 
| Runtime | ds4-dfm-rs `main` from`d70afd6a` (family`beam_vq` ), CUDA on DGX Spark (GB10) | 
| DGX Spark speed | 1,241.2 tok/s prefill, 25.23 tok/s decode, 2K–64K, median of three runs (details) | 

DeepSeek-V4.1-Flash VQ keeps a ~106 GiB base resident and still serves 1M
tokens because its attention is compressed: YoungAi measures the **whole 1M
KV cache at 0.89 GB**. Beam's nine full-attention layers are ordinary GQA
(8 KV heads × 128). At 1M tokens they need **36.0 GiB of KV in BF16, or 18.3
GiB in FP8** — twenty to forty times DeepSeek's. A Beam that copied
DeepSeek's weight size would not serve 1M on one Spark; the weights have to
give up what the KV cache takes.

Budget for one bank at the full 1,048,576 tokens as designed, against the
118.3 GiB a GB10 reported available to `ds4-server --check-config` for
GLM-5.3 VQ (the GB10 that measured this release reports 117.2 GiB):

|  | GiB | 
|---|---|
| Routed experts (8 × 13-bit + 41 × 12-bit layers, gains, codebooks) | 87.55 | 
| Attention + dense FFN + output head, Q4_K | 3.76 | 
| Router (F32, as in the source), biases, norms | 0.24 | 
| **Resident weights** | **91.55** | 
| Full-attention KV, FP8 E4M3 + per-position scales, 9 layers × 1M | 18.28 | 
| Sliding-window rings, BF16, 43 layers × (512 + 2,048) | 0.42 | 
| Scratch at 2,048-row prefill chunks *(estimate from the nearest families)* | 2.40 | 
| Floor kept free by the server | 4.00 | 
| **Quote at 1M** | **116.65** of 118.3 | 

**What ds4-dfm-rs plans on GB10** (`ds4-server --check-config`, default
1,048,576 context): the trunk takes 91.68 GiB on the device (the 1.45 GiB
embedding part stays mapped), one 2,048-row graph 1.12 GiB of workspace, the
FP8 full-layer rows 18.28 GiB and the sliding rings 0.42 GiB: **one bank at
115.4 of 117.2 GiB** with the 4 GiB floor. The plan has not been exercised by
a 1M run.

With BF16 KV the same table needs 134.4 GiB, so **FP8 KV on the nine full
layers is part of this release's contract**, not an option. The eight 13-bit
layers are where the spare bytes went: each costs 0.146 GiB. All-12-bit
would free 1.17 GiB; fourteen 13-bit layers (the DeepSeek count) would take
0.88 GiB more. `memory-budget.json` and `tools/memory_budget.py`
reproduce every designed row.

- **Routed experts.** Every 8 consecutive weights of an expert row become one
index into a codebook, times one F16 row gain (the row RMS, computed in
FP64 and rounded before normalizing). One codebook per layer serves all 256
experts and gate/up/down; YoungAi measured a shared book to cost nothing
against one per expert. Codewords are trained in FP32 (equidistant init,
32 Lloyd iterations with FP64 sums) on a stratified pool of 9.8M vectors
per layer (20 gate, 20 up and 40 down rows from every expert), rounded
FP32 → F16 → E4M3, and every vector is then assigned against the stored
codebook.
- **Calibration-weighted Lloyd (2026-10-21 revision).** The first build fitted
the codebooks to the weights alone (plain squared error of normalized
vectors). The revision runs a calibration set — the same 3.91M-token mix as
A.X-K2 VQ: English, Korean,
code, math, Japanese/Chinese/Spanish, raw text and Harmony chats with
reasoning and tool calls, up to 32K tokens, rendered with Beam's tokenizer
and template — through the BF16 original layer by layer and records, per
layer and expert, how many tokens each expert receives and the squared
activations feeding gate/up and down. The pool vectors then carry those
per-column importances (tempered by a power of 0.75, shrunk toward the layer
mean for rarely routed experts, floored at 2%), Lloyd minimizes the
importance-weighted error, every vector is assigned with the
importance-weighted distance, and each row gain is refit by weighted least
squares (one F16 per row, as before). The book is still one per layer, the
13-bit layers are still 3–10 and the recipe id is unchanged, so the parts
are a drop-in for the same runtime. Measured on A.X-K2, the data-free
row-energy weighting gains 1–3%; the calibration statistics are what help.
- **Which layers get 13 bits.** Each extra index bit divides the weight error
by about 1.19 (YoungAi: 0.201 / 0.171 / 0.143 at 11 / 12 / 13 bits), the
curve is convex, so spare bytes go to many layers one bit at a time, and
to the bottom first: YoungAi measured shallow layers worth 1.2–1.4× more per
GB, and the GLM-5.3 end-to-end sweep ranked layers 3–16 first. Beam's eight
bottom MoE layers (3–10) take the bytes the 1M budget leaves.
- **Everything read on every token besides the experts** — attention q/k/v/o
and the output-gate projection of all 52 layers, the three dense FFN layers
and the output head — is Q4_K, as in DeepSeek V4.1 VQ. YoungAi measured that
skeleton's error at 0.5% against 17% for the experts: raising it buys little.
- **Not touched:** router weights and correction biases, all norms, the
per-head attention gate bias, and the token embedding (BF16, read from SSD).

Weight-space reconstruction of the routed experts, measured by the converter
on the stored bytes against the BF16 source (relative MSE; not a model-quality
measurement): the 41 12-bit layers 0.174–0.188 and the eight 13-bit layers
0.162–0.417 (first build: 0.1698–0.1703 and 0.1441–0.1446). The plain error
rises on purpose: precision moves to the inputs the activations actually use,
and the importance-weighted error falls (layer 3 held-out rows 0.039). Every layer is in
`weight-error-by-layer.json`; the per-matrix
records are in `conversion-receipts/`. The Q4_K tensors average 0.55%
relative MSE (worst 0.67%, `layers.0.mlp.down_proj`; output head 0.53%).

**GPQA Diamond 88.6 against the BF16 original's 90.6 (97.8%); AIME 2026
93.8 against 97.7 (96.0%); IFBench 78.0 against 79.6 (98.0%); AA-LCR 77.0
against 78.2 (98.5%); LiveCodeBench 84.0 against 88.3 (95.1%). Scientific
coding and factual recall drop more:
SciCode 43.3 against 49.2 (88.0%), AA-Omniscience index −4.7 against 10.8.**

Run on this release on one DGX Spark (GB10) with
ds4-dfm-rs `main` `ed57f3b7`, using
Reflection's Beam Verifier
presets (InspectAI; temperature 0.7, top-p 0.99, `reasoning_effort` max, no
output cap inside a 64K context). The original's scores are the BF16 column of
Reflection's card.

| Benchmark | Original (BF16) | This release | Ratio | 
|---|---|---|---|
| GPQA Diamond | 90.6 | **88.6** (95% CI 84.3–92.4) | **97.8%** | 
| AIME 2026 | 97.7 | **93.8** (95% CI 85.4–99.2) | **96.0%** | 
| IFBench (prompt-level loose) | 79.6 | **78.0** (95% CI 73.0–82.3) | **98.0%** | 
| AA-LCR | 78.2 | **77.0** (95% CI 67.9–84.2) | **98.5%** | 
| LiveCodeBench (v6, 175 problems) | 88.3 | **84.0** (95% CI 77.8–88.7) | **95.1%** | 
| SciCode (sub-problems, with background) | 49.2 | **43.3** (95% CI 35.7–50.5) | **88.0%** | 
| AA-Omniscience index (public v1.0) | 10.8 | **−4.7** (95% CI −10.6–1.3) | −15.5 points | 

GPQA Diamond: 198 questions × 2 epochs (the preset averages 16), averaged per question and then over questions; the interval resamples questions. All 396 answers finished inside the context (median about 2.2K reasoning tokens). Physics 96.5%, chemistry 84.4%, biology 73.7% (19 questions). Most answers were decoded on the serial lane with DFlash2 drafts verified against the request's sampler; 23 came from the plain 8-bank lane.

AIME 2026: 30 problems × 8 epochs (the preset averages 64). 26 problems were solved in every sample; without problem 15 the rest average 97.0%. Problem 15 reached the 64K context in seven of eight samples, so it was rerun on a 192K server with Beam's 131,072-token output limit, eight epochs, replacing its 64K samples: it still solved 0 of 8 (six wrong answers, two at the limit).

IFBench: the 300 prompts of allenai/IFBench, one sample each, scored by
IFBench's `run_eval` on the final answer; prompt-level loose accuracy is the
IFBench paper's headline (strict 75.0%). All 300 answers finished.

AA-Omniscience: the 600 public v1.0 questions (the public split when Beam was released), the card's answer prompt, graded correct / incorrect / partial / not attempted by GPT-5.6 Luna (medium) with the card's grader template; index = 100 × (correct − incorrect) / total. 153 correct (25.5%), 181 incorrect, 258 not attempted: the release abstains often and is wrong on 40% of what it does not get right. Hand-checked incorrect grades were wrong facts, not grader errors. Recall of rare facts is what the VQ experts lose first.

AA-LCR: the 100 v1.1 questions with their document sets (median 105K prompt tokens, served at a 192K context), graded by GPT-5.6 Luna (medium) with the v1.1 prompts as Artificial Analysis does. All 100 answers finished.

SciCode: inspect_evals' port with the scientist background (Artificial Analysis' sub-problem metric); 126 of 291 sub-problems pass their tests in a Docker sandbox, 7 of 65 problems pass every step. The interval resamples problems.

LiveCodeBench: the 175 v6 problems (112 AtCoder, 63 LeetCode), one sample
each, scored by LiveCodeBench's own grader in a container without network.
Five AtCoder answers reached the 65,536-token answer cap; Reflection's
evaluation guide raises the budget to Beam's 131,072-token limit on `length`,
so they were rerun there and replace the capped ones: all five finished and
one passed (83.4 with them counted as failures). Easy 43 of 43, medium 47 of
52, hard 57 of 80.

HLE and LongBench v2 are running and will be added here.

A teacher-forced comparison with the BF16 original, run layer by layer (the
first build on an H100 when it was released; both builds again on an RTX PRO
6000 Blackwell for the revision, giving the first build 83.3% / KL 0.291 there
— the same within hardware differences): the release side reads every weight from this repository's files (VQ
experts restored from the stored indices, Q4_K dequantized, BF16 embedding
rows); the original side reads the source checkpoint. Both run BF16
attention, so FP8 KV is not part of these numbers, and this is not the ds4
engine. The forward pass re-implements transformers 5.20.2's Beam layer and
matches `BeamDecoderLayer` (layer 5 bit-identical, layers 0 and 3 within
1.1e-4 relative RMS).

| Held-out text | Tokens | KL, first build → **this build** | Same top-1 | Perplexity, original → first build → **this build** | 
|---|---|---|---|---|
| Korean prose | 289 | 0.426 → **0.165** | 75.1% → **83.7%** | 2.28 → 3.65 → **2.79** | 
| Indonesian web text | 1,024 | 0.367 → **0.177** | 79.9% → **85.8%** | 4.22 → 5.74 → **4.87** | 
| Source-tree listing | 1,024 | 0.439 → **0.252** | 80.0% → **83.8%** | 3.03 → 4.30 → **3.58** | 
| English technical prose | 1,024 | 0.231 → **0.118** | 81.3% → **86.0%** | 4.97 → 5.70 → **5.38** | 
| Harmony chat, Korean math (analysis + final) | 1,024 | 0.161 → **0.067** | 88.8% → **92.6%** | 3.14 → 3.47 → **3.23** | 
| Harmony chat, English reasoning | 716 | 0.169 → **0.073** | 89.7% → **92.7%** | 1.88 → 2.10 → **1.99** | 
| Harmony tool call (XML), tool result, final | 390 | 0.386 → **0.155** | 81.0% → **87.9%** | 6.22 → 7.58 → **6.29** | 
| **All** | **5,491** | **0.295 → 0.144** | **82.9% → 87.7%** | NLL 1.243 → 1.463 → **1.340** nat | 

(First-build columns are the H100 numbers published with it; the revision
was measured on the RTX PRO 6000, where the first build gives 0.291 / 83.3% /
NLL 1.456.) On a second, larger held-out set — 22 cases, 54,017 tokens:
wikitext-2, English/Korean/Japanese/Chinese/Spanish web, chat in all
reasoning modes, math, code, tool calls, SWE, finance, 8K documents — the
first build gives KL 0.492 / 80.4% and this build **0.278 / 85.8%**
(perplexity 2.21 → 3.54 → **2.89**), lower KL in all 18 domains (−33% to
−69%; wikitext-2 −37%, Korean web −61%, tool calls −59%). The hidden state
stays closer to the original's: cosine 0.973 vs 0.939 at layer 40, 0.943 vs
0.863 at layer 49; the top-8 expert set matches at layer 40 for 14.6% of
tokens instead of 3.7%.

For scale, on different texts: the same method's bare DeepSeek V4.1 Flash base agreed on 69–79% of next tokens (KL 0.42–0.69, YoungAi's rulers); GLM-5.3-Flash VQ reached 81.4% (KL 0.288) only after its 19 most sensitive layers got an 8-bit residual stage. The exact top-8 expert set matches the original's for 54% of tokens at layer 3 and 3–5% in the last layers — routing is hash-like, as YoungAi found on DeepSeek — yet the outputs still agree.

**In ds4-dfm-rs on GB10 (first build's parts).** The engine runs the same 5,491 positions
teacher-forced, one decode row at a time, with its own arithmetic (exact
Q4_K, FP8 KV on the full layers), and again with 512-token prefills
(quantized activations). The reference above, rerun on the same GB10,
picks the H100 run's next token at only 93.94% of positions (NLL 1.4590 vs
1.4629): Beam's router group scores sit near 37.5 with margins around 3e-4,
so any change of accumulation order flips near-tied routes. Against that
floor the engine is within its fixed bands: **decode NLL 0.14% below the GB10
rerun, top-1 93.79%**; a 512-row prefill NLL 0.24% below. Every route flip
falls at a reference near-tie.

| Expert stage | Codebooks | Decode gate/up | Decode down / verify | FP8 prefill MMA | 
|---|---|---|---|---|
| 12-bit (layers 11–51) | 32 KiB | 43,008 B | 73,728 B | 91,744 B gate/up, 92,768 B down (warp-specialized, 3 stages of 128 tokens) | 
| 13-bit (layers 3–10) | 64 KiB | 75,776 B | **106,496 B** with all 8 experts' mid rows staged →**86,016 B** staged 4 + 4 | 96,768 B (2 stages) | 
| 13 + 8 residual (not used) | 66 KiB | 77,824 B | 108,544 B → 88,064 B staged 4 + 4 | 98,816 B | 

GB10 allows 101,376 B per block. Beam's down projection reads 8 experts × 2,560 BF16 mid values (40 KiB), so the 13-bit layers' down/verify kernels stage the experts in two groups of four (one extra barrier per block); the 12-bit layers decode with the GLM/Ling scheme as it is, with 27 KiB to spare, and prefill on warp-specialized kernels (eight MMA warps, four copy warps, a three-stage ring of 128-token stages beside the 32 KiB book). This is what ds4-dfm-rs runs, with the kernels' row widths generalized to Beam's 5,120 and 2,560.

The ds4-dfm-rs README sweep: three fresh `ds4-bench` processes per variant,
interleaved; every frontier prefills 2,048 new tokens on top of the cached
prefix, then generates 128 forced greedy tokens, and the session restores the
prompt snapshot before the next frontier. Prompt: the raw Italian novel
`speed-bench/promessi_sposi.txt`. The dashed line is the same sweep before the
October 21 optimization round (`dab8bd75`).

| Configuration (median of three run means) | Prefill tok/s | Decode tok/s | Before the round | 
|---|---|---|---|
| MTP off, 2K–64K | **1,241.2** | **25.23** | 830.4 / 15.86 | 
| MTP off, 2K–32K | 1,326.0 | 26.03 | 881.5 / 18.70 | 
| DFlash2 (draft 7, margin 2), 2K–64K | 1,225.7 | 24.73 | 821.1 / 15.41 | 

| Frontier | Prefill, MTP off | Decode, MTP off | Prefill, DFlash2 | Decode, DFlash2 | 
|---|---|---|---|---|
| 2K | 1,340.8 | 26.61 | 1,251.9 | 25.75 | 
| 4K | 1,413.7 | 26.58 | 1,392.8 | 25.98 | 
| 8K | 1,385.5 | 26.49 | 1,373.6 | 25.93 | 
| 16K | 1,339.8 | 26.09 | 1,323.0 | 25.57 | 
| 32K | 1,239.8 | 25.29 | 1,226.8 | 24.83 | 
| 48K | 1,162.9 | 24.40 | 1,151.0 | 23.99 | 
| 64K | 1,087.2 | 23.67 | 1,078.8 | 23.26 | 

- **What changed.** Decode: the attention of both layer kinds now splits
each KV head's keys into about 48 chunks and serves its six query heads
together, the full layers' FP8 rows arrive by one bulk copy per key, and a
token's four attention projections run as one launch. Prefill: the RoPE rows
are computed once per forward, the 12-bit experts run on warp-specialized
FP8 tensor-core kernels (tokens × weights^T, so each codeword is one MMA
operand load), and the full layers' attention takes a warp-specialized walk
that produces the same bytes. Per 2,048-row chunk the 12-bit expert matrices
went from 40–43% to 58–65% of their weight-streaming floor.
- **Decode bytes.** A decoded token reads about 7.24 GB of weights: the eight
selected experts of 49 layers (2.94 GB), Q4_K attention (3.07 GB), the dense
FFN of layers 0–2 (0.53 GB), the head (0.44 GB) and the F32 routers
(0.26 GB) — 29.4 ms at the 246 GB/s GB10 streams in practice, against 37.6 ms
measured at 2K (44.3 ms before the round).
- **Context.** Decode falls 11% from 2K to 64K (it halved before): the nine
full layers' FP8 rows, 2,080 bytes per token and layer and 1.23 GB at 64K,
are now read at about 229 GB/s. Prefill peaks at 1,413.7 tok/s at 4K and is
23% lower at 64K, where those layers attend over the whole prefix at about
60% of the GB10's F16 tensor-core peak.
- **DFlash2 follows the text.** This raw prose leaves most of its blocks
doubtful, so drafting costs 2.0% of decode here (and its tap projections
1.3% of prefill); on chat it gains 22% (next section).

Conditions: one DGX Spark (GB10), driver 615.71.09, CUDA 13.4 (`sm_121a`),
Linux 7.0.0-1019-nvidia, SM clock 2,184–2,190 MHz; `ds4-bench` built from
`391b0bf1` (the record names its hash); one model process at a time, page
cache dropped before each run. The full record (every sample, binary and file
hashes) and the optimization log are in the repository:
`docs/benchmarks/beam-vq-spark-64k-2026-10-21-tuned`,
`docs/beam-vq-optimization-2026-10-21.md`.

```
./ds4-bench --cuda -m Beam-1.0-501B-A23B-VQ-GGUF --prompt-file speed-bench/promessi_sposi.txt \
  --ctx-start 2048 --ctx-max 65536 --step-incr 2048 --gen-tokens 128 --csv plain.csv
./ds4-bench ... --mtp Beam-1.0-501B-A23B-VQ-GGUF/dflash2/Beam-1.0-501B-A23B-DFlash2-Q8_0.gguf \
  --mtp-draft 7 --mtp-margin 2 --csv dflash2.csv
```
`dflash2/Beam-1.0-501B-A23B-DFlash2-Q8_0.gguf` is
Reflection's drafter, revision `163b276b`, with its matrices stored as Q8_0
(norms and convolution kernels F32, selector codebooks BF16; 1.98 GiB on the
device). Every target forward projects the rows after layers 4, 16, 28, 40
and 46 into the five draft layers' context rings; a trial then drafts a block
non-causally (dynamic convolutions, attention over the ring's last 4,095
positions, Beam's head and the candidate selector), and the trunk verifies
the block in one forward and keeps the prefix it agrees with.

The selector's best-minus-second score predicts acceptance (33% of proposals
under 1 accepted, 74% at 2–3, 98% above 6), so verification stops before the
first proposal under `--mtp-margin`, and a doubtful first proposal skips the
trial for an ordinary step. A trial drafts in about 13 ms and verifies two to
eight rows in 58–130 ms, against about 44 ms for one decode step (measured
before the optimization round; a decode step is 37.6 ms now).

With the current runtime the five chat prompts run **26.2 tok/s without the
drafter and 32.0 tok/s with draft 7, margin 2** (+22%). The setting was chosen
on the first runtime:

| Five greedy chat prompts, 384 tokens, first runtime | tok/s | 
|---|---|
| No drafter | 24.0 | 
| draft 7, no margin gate | 23.0 | 
| draft 7, margin 1 | 29.2 | 
| **draft 7, margin 2** | **30.9** | 
| draft 7, margin 3 | 27.6 | 
| draft 15, margin 2 | 29.3 | 

**Losslessness.** The trunk verifies every drafted token, so the output is
the trunk's greedy output up to the rounding of a multi-row verify. Measured
on seven fixture prompts (192 greedy tokens each, draft 7, margin 2, 2.52
tokens per step): each generated token is scored in a one-row decode
teacher-forced on the same tokens. 21 of 1,224 tokens were more than 0.5
logits below that decode's argmax and one more than 4 (worst 5.6), inside the
95% Poisson counts of the reference's own H100-vs-GB10 disagreement (32 and
6). Twenty of the 21 were tokens the verify row itself chose, one an accepted
draft.

Build with `make cuda-spark` and pass the release directory as the model:

```
# one bank, the native 1,048,576 context
./ds4-server --cuda -m Beam-1.0-501B-A23B-VQ-GGUF --host 0.0.0.0 --port 8000
# two banks with partial prefix reuse and disk KV
./ds4-server --cuda -m Beam-1.0-501B-A23B-VQ-GGUF -c 131072 --max-seqs 2 --prefix-reuse partial \
  --kv-disk-dir ./beam-kv --kv-disk-space-mb 32768
# DFlash2: one sequence; drafts at any temperature
./ds4-server --cuda -m Beam-1.0-501B-A23B-VQ-GGUF -c 65536 \
  --mtp Beam-1.0-501B-A23B-VQ-GGUF/dflash2/Beam-1.0-501B-A23B-DFlash2-Q8_0.gguf \
  --mtp-mode on --mtp-draft 7 --mtp-margin 2
```
- **Output.** ds4-server parses Harmony into reasoning, the final answer and
typed tool-call arguments, and streams both channels.`reasoning_effort` maps minimal … max to the template's 8 … 512
(default max).
- **Header forcing.** The VQ weights drift at Harmony header positions: before
the fix, 73% of 75 greedy replies broke a header (`<|channel|><|channel|>` ,`<|channel|> final` ) where the BF16 model writes`analysis` with a 17-logit
margin. The server forces only the header tokens the grammar admits alone;
bodies, the recipient and the choice between an answer and a call stay
sampled. With forcing, 150 greedy replies (75 serial, 75 continuous, five
efforts) broke no header.
- **Memory.** With the drafter, 512K plans at 108.4 GiB; 1M plans with
0.12 GiB to spare and has not been run.`--max-seqs 2` beside the drafter is
refused.
- **Reuse.** A four-turn conversation on a 4.4K-token system prompt reused
4,532 / 4,566, 4,696 / 4,726 and 4,802 / 4,829 tokens on the banks, and after
a restart 4,988 / 5,011 from disk; the DFlash2 lane reuses the same in
memory but keeps no disk records for thinking requests, which every Beam
chat request is.
- Family documentation:
`docs/beam-vq.md` .

- **Context:** 1,048,576 tokens. Sliding layers use RoPE (θ = 10,000) over a
512-token window; full layers use no positional encoding and scale the query
by`(1 + (pos + 1) / 2048) ** 0.1` . Nothing is rescaled or extended.
- **Chat template:** the original`chat_template.jinja` (Harmony wire format
with XML tool-call bodies,`analysis` and`final` channels, tools declared
as TypeScript,`<|constrain|>xml` calls with`rfl:parameter` bodies, tool
results on`analysis` ).`reasoning_effort` accepts`minimal` /`low` /`medium` /`high` /`xhigh` /`max` = 8 / 32 / 64 / 128 / 256 / 512 and
defaults to 512. Generation stops on`<|return|>` (151674) or`<|call|>` (151675).
- **Sampling:** Reflection evaluated with temperature 0.7 and top_p 0.99
(`generation_config.json` ).
- **Numerics kept in FP32 by the source model:** router logits, SwiGLU,
logit soft-capping at 30.

| Path | Contents | Size | 
|---|---|---|
| `layer-03-experts.gguf` …`layer-10-experts.gguf` | 13-bit routed experts, one independent GGUF per layer | 2,050,196,864 B (2,050,197,632 B for layer 10) | 
| `layer-11-experts.gguf` …`layer-51-experts.gguf` | 12-bit routed experts | 1,892,878,464 B each | 
| `body-nonexpert.gguf` | Q4_K attention / dense FFN / head, source router, biases and norms | 4,294,683,136 B | 
| `embed-tokens-bf16.gguf` | BF16 token embedding, read in rows from SSD | 1,555,825,280 B | 
| **All 51 parts** |  | **99,860,101,120 B** | 
| `dflash2/Beam-1.0-501B-A23B-DFlash2-Q8_0.gguf` | Reflection's DFlash2 drafter, Q8_0 (not a release part; `--mtp` ) | 2,121,674,944 B | 
| `dflash2/README.md` | Drafter provenance, conversion and use |  | 
| `release.json` | Entry point: every part with role, size and SHA-256 |  | 
| `tensor-manifest.json` | Every stored tensor and its source mapping |  | 
| `recipe.json` ,`memory-budget.json` | Machine-readable recipe and the budget above |  | 
| `weight-error-by-layer.json` ,`conversion-receipts/` | Per-layer and per-matrix conversion records (sizes, hashes, errors) |  | 
| `quality/` | The reference comparisons above (both builds, both held-out sets) and the selftest |  | 
| `calibration/` | Calibration-set and held-out manifests, statistics summary |  | 
| `tools/` | The converter, release and drafter-conversion scripts as run |  | 
| `config.json` ,`generation_config.json` ,`tokenizer.json` ,`tokenizer_config.json` ,`chat_template.jinja` | Original files, unchanged |  | 
| `RECIPE.md` ,`FORMAT.md` ,`SOURCE.md` | Recipe, byte format, provenance |  | 
| `SHA256SUMS` ,`files.json` | Hashes of every file |  | 

These are independent GGUF parts, not numbered GGUF splits: do not concatenate them. Standard llama.cpp, Ollama and Transformers loaders do not read this format.

Reflection's scores for the original checkpoints, from the Beam model card. They describe the source model, not this quantization; scores measured on this release are above.

| Benchmark | BF16 | FP8 | NVFP4 | 
|---|---|---|---|
| SWE-bench Verified | 81.6 | 80.2 | 80.7 | 
| Terminal Bench v2.1 | 80.1 | 78.9 | 78.4 | 
| AIME 2026 | 97.7 | 98.0 | 97.5 | 
| GPQA Diamond | 90.6 | 90.3 | 89.3 | 
| LiveCodeBench | 88.3 | 88.1 | 87.4 | 
| HLE (no tools / with tools) | 36.0 / 51.1 | 36.0 / 51.1 | 34.8 / 51.1 | 
| MCP Atlas | 80.8 | 81.5 | 80.5 | 
| BrowseComp w/ context management | 76.8 | 76.7 | 75.6 | 
| AA-LCR | 78.2 | 79.4 | 78.7 | 
| LongBench v2 | 67.5 | 67.6 | 67.2 | 

- This is lossy: about 1.5 bits per routed-expert weight. The release agrees with the original's next token 87.7% of the time on the held-out set above; the source-tree listing and Korean prose are its weakest cases (83.7–83.8%).
- Greedy decoding can loop in reasoning: one of fifteen conformance prompts
kept reasoning until `max_tokens` at temperature 0 and answered in 179
tokens at the recommended temperature 0.7.
- Measured to 64K. The 1M context is a `--check-config` plan, not a run. Past
a few thousand tokens the nine full layers set the slope: their FP8 rows
cost decode about 5 ms per token at 64K, and their prefill attention runs at
about 60% of the F16 tensor-core peak.
- DFlash2 serves one sequence. Since ds4-dfm-rs `e8a1e9dd` it also drafts
sampled requests, verifying each draft against the request's sampler: at
Beam's temperature 0.7 it decoded seven benchmark prompts (six GPQA Diamond
questions, one competition-math problem) at 31.6 tok/s against 27.2 without
drafts (`ed57f3b7` ). On text unlike chat (the raw prose above) it costs a
few percent.
- DFlash2 is implemented and measured in ds4-dfm-rs, not release-qualified.
`ds4 --serving-check` qualifies partial prefix reuse, disk KV and snapshots
(record).
- The calibration set's mix (English, Korean, code, math, chat, tools) decides which errors the codebooks keep small; no domain sidecar. The GB10 engine and speed numbers are from the first build's parts and still need a rerun.

- **Reflection** — Beam-1.0-501B-A23B and its
DFlash2 drafter,
Apache-2.0.
- **z-lab/dflash** (MIT) — the DFlash
block-drafter architecture; the runtime's drafter follows its`model.py` .
- **YoungAi** by Wenzhou Wu (吴文周,
fodelf /
wenzhouwu), MIT — the method: 8-dim VQ
with per-layer shared E4M3 codebooks, F16 row gains, the 12 + 1 bit-plane
layout for 13-bit layers, shallow-layers-first bit allocation and the
zero-corpus rule (`docs/TECHNICAL.md` §4.1,`docs/113.md` ).
The converter's CUDA assignment and FP64 Lloyd follow YoungAi`gguf-tools/quantize/v41_vq.cu` ; its notice is kept in`LICENSE-YoungAi` .
- **ds4-dfm-rs** — the runtime: the
GVQ1 container, its GB10 kernels, the Beam family and the Ling-3.1 / GLM-5.3
VQ tools this converter is forked from. It continues
antirez/ds4, the original runtime, and
Entrpi/ds4, which contributed the CUDA and
batched serving line and the GB10 (`sm_121` ) target.
- **llama.cpp / GGML** — the GGUF
container and the Q4_K and Q8_0 layouts.

Apache-2.0, as the source model and the DFlash2 drafter. The quantization
tools derive from YoungAi and ds4.c (MIT, `LICENSE-YoungAi`, which includes
the GGML notice).

- Downloads last month
- 5,651
