---
id: collect-261121-veille-ia/robotics-kyutai/reki-shisso-robotics
title: "REKI - Shisso Robotics"
domain: robotics-kyutai
role: reference
task: article
actors: []
dates: ["2026-10-23"]
keywords: ["reki", "memory", "same", "runs", "paper", "chunk", "orin", "smolvla"]
source: https://shissorobotics.com/blog/2026/10/23/paper-vs-hardware-reki/
source_anchor: ""
source_lines: [1, 132]
section: "Robotics_Kyutai"
---
# REKI¶

Juliette Marrie, Paul Pacaud, Adrien Ramanana Rahary et al. · arXiv 2610.22178

Check for pose oscillation in your failed runs: 24.0 percent of mine oscillated, as the paper describes.

They said robot policies without memory get stuck going back and forth between poses. I measured that back-and-forth in my 69 recorded runs, then trained the memory version. 24.0 % of failed runs oscillated; with memory, 1 run in 40 did.

## If you build robots

If your policy dithers between poses, a short memory of past frames may help. It succeeded 19/20 on both instructions here, but it does not run on the Jetson.

## If you do research

The oscillation rate reproduces with the paper's own metric. The low rate with memory is not credited to memory: the memory-off control is not run. Served from an RTX 3090.

## What I ran

**Latency check (G12), voided the same day.** The bar was memory ≤ 8 GB and chunk ≤ 250 ms on the Orin; the script reported 3.46 GB and 318 ms. The re-check found it timed a random-init Qwen2.5-1.5B with no vision tower and a hand-written random expert; the parts did not sum (135 + 230 ≠ 318). REKI's Orin latency is **unmeasured**.

| Test | Pass mark | Result | On this rig | 
|---|---|---|---|
| Do my policies oscillate the way the paper describes? | at least 10 % of failed runs oscillate | 24.0 % of failed runs | unclear | 
| Does it work on my arm? | not worse than SmolVLA (9/10) by more than 2 in 20 | 19/20 and 19/20 | works | 
| Does it run on the Orin within 8 GB and 250 ms per chunk? | at most 8 GB and 250 ms per chunk | voided, not measured | unclear | 

## What the gap means¶

My oscillation audit is the cleanest external replication of a mechanism in my register: a memoryless policy on a SO-101 dithers at about the rate a memoryless 2.4B VLA dithers on RoboTwin, under the same definition. What I cannot yet say is whether memory fixes it here, because adding registers to SmolVLA means retraining. The paper's Fig. 5a points at the cheap half: for any policy that has a past-frame cache, the same checkpoint evaluated with and without it is a free experiment. SmolVLA has none, so the port is the only route, and it costs $5–10 plus a robot session; the amendments put the free comparison and the real latency measurement before it.

What would change it: the same REKI checkpoint at K = 1 (memory off) on the same 40-trial protocol.

## Update 2026-10-28: REKI trained and run on my arm¶

**Training.** REKI's frozen-VLM recipe (their repo at `cc16b94`), 3,500 steps, batch 30 × 4, on my bowls data. One rented RTX PRO 5000, 16.1 GPU hours, **$11.25** plus a $0.60 smoke test. Training loss 0.315 → 0.008. One seed.

**Serving.** REKI doesn't run on the Orin at all: it needs flash-attn, FLA / Triton and causal-conv1d, which only build for x86 CUDA. So the policy ran on a rented RTX 3090 (~100 ms per chunk) and the Orin drove the arm as a client over an SSH tunnel. $1.20 over two sessions. Every chunk is a network round trip, so the arm pauses about 10 s of every 25 s trial.

**Protocol.** Same as my SmolVLA gate: 25 s per trial, judged at rest by me, 20 trials per instruction. Bar, fixed before the first counted trial: not worse than SmolVLA (9/10) by more than 2 in 20.

| instruction | REKI | failures | 
|---|---|---|
| blue bowl in pink | **19/20** | one set down short (the pink bowl started outside the teleop area) | 
| pink bowl in blue | **19/20** | one missed grasp | 

Four more trials were thrown out and replaced: after a network stall of 5 s or more the server cleared REKI's memory mid-trial. I wrote that rule before seeing the last affected trial's outcome, and applied it to all four (three were successes).

**Oscillation.** With the metric above, **1 of 40** REKI trials oscillates (2.5 %), against 20.3 % on my earlier policies. That's the direction the paper predicts, but it isn't their ablation: different policies, and I computed it on REKI's commanded trajectory rather than recorded states. The clean test is the same REKI model with memory switched off (K = 1) on the same setup. I haven't run it.

*Two successes and the missed grasp, 47 s. Network pauses are cut, so it plays faster than real time.*

REKI is a 2.4B VLA (Qwen3.5-2B plus a flow-matching expert) with **32 register tokens per frame** in the expert. The registers read the registers of the K − 1 previous frames (K = 3) and are cached across chunks, so memory costs +0.1 ms. The paper counts an episode as oscillating if an arm **reverses direction at three or more consecutive chunks**: **4.1 % with memory, 14.8 % when the same model is evaluated without it, 18.6 % trained without**, on RoboTwin with 20 matched seeds each.

## Full record: test by test, their number and mine, claims and hypotheses, contradictions

Papers tested

**Goal (the paper's):** Give a VLA a short memory of past frames with register tokens cached across steps, so it stops oscillating between visually aliased poses; trained on public data only and evaluated on LIBERO-Plus, RoboTwin, RoboCasa and real robots.

**Its hypotheses:**

- Policies without memory oscillate between poses in a measurable share of episodes; register memory cuts it. (Fig. 5b (18.6 % without, 4.1 % with)) reproduces
- Most of the gain comes from reading the first past frame at inference. (Fig. 5a) not tested here
- A 2.4B VLA pretrained only on public data matches larger in-house models. (Fig. 4) not tested here

**What ran:** the authors' method, my bar · **On this rig:** unclear · **Run:** authors' method · **Claim tested:** Policies without memory oscillate between poses at a measurable rate. · **Bar:** ≥ 10 % of failed episodes oscillate (their criterion). · **n:** 69 published rollouts

**Theirs vs mine:** 18.6 % without memory, 4.1 % with vs 20.3 % of all episodes, 24.0 % of failures; REKI trained on my data: 19/20 + 19/20 on the arm, oscillates in 1/40 · **Not proven:** That memory causes the drop to 1/40: the K = 1 (memory off) control is not run. REKI does not build for the Orin (served from an RTX 3090).

**Paper:** arXiv 2610.22178 · **Code:** kyutai-labs/reki

**Claims:**

- reproduces policy without memory at least pass mark on oscillating share of failures — theirs 18.6 % of all episodes and about half of the failed ones oscillate without memory (RoboTwin Clean, §6.2, Fig. 5b) · mine 24.0 % of failures, 20.3 % of all episodes (paper, 18.6 % without memory, 4.1 % with)
- reproduces REKI at least pass mark on success on the arm — theirs UR5 + WSG50 real-robot success · mine 19/20 + 19/20 (paper §7, Fig. 8)

**Compute:** External GPU: yes for REKI (vast.ai): training $11.25 + smoke $0.60, serving $1.20, total $13.05. Oscillation audit on the Orin NX: $0. · **Reproduce:** 2026-10-23-paper-vs-hardware-reki · **First published:** 2026-10-23 · **Updated:** 2026-10-28 (REKI trained and run on the arm) · **Findings:** REKI 2.4B on the Orin →

**Tested:** 2026-10-23

The claim that decided it: policy without memory at least the pass mark on oscillating share of failures; my result 24.0 % of failures, 20.3 % of all episodes, bar ≥ 10 % of failed episodes oscillate.

**Gates:** G14, WS-025, WS-026 in the Experiment log

### What the paper claims, and how it measures it (full)

A 2.4B VLA (Qwen3.5-2B + a flow-matching expert) with **32 register tokens per frame** in the expert: they cross-attend the frame's context, self-attend the registers of the K − 1 previous frames (K = 3), never read the action tokens, and are cached as keys/values across chunks — so memory costs +0.1 ms. Pretrained on public data only; 21.8 ms per chunk on an H100. The augmentation detail that matters for a webcam rig: random camera dropout and shuffled camera order, worth > 20 points under viewpoint change.

Evaluation: LIBERO-Plus 90.4; RoboTwin Clean2Rand 65.8; RoboCasa365 38.4; baselines quoted from their papers. The memory ablation (Fig. 5a): a K = 3 model evaluated at K = 1 scores like a K = 1 model — the gain comes from *reading* past frames at inference, mostly the first one. The oscillation result (Fig. 5b): an episode oscillates if an arm **reverses direction at three or more consecutive chunks**; **4.1 % with memory, 14.8 % when the same model is evaluated without it, 18.6 % trained without**; two-arm tasks 24.5 → 3.1; RoboTwin, 50 tasks, **20 matched seeds each**. Mechanism offered: perceptual aliasing — two poses each look like demonstration states heading toward the other.

## How it was tested

- Claim tested
- Policies without memory oscillate between poses at a measurable rate.
- Bar
- ≥ 10 % of failed episodes oscillate (their criterion).
- Sample
- 69 published rollouts
- Result
- 24.0 % of failures, 20.3 % of all episodes
- Interval
- episodes that oscillate: 14 of 69; 95 % Wilson interval, computed at build: 12 % to 31 %
- Repro folder
- the folder in paper-vs-hardware (scripts, expected files, REPRODUCE.md)

## History

- 2026-10-31 reki: L1 full command (labels ship with the dataset), checked cold
- 2026-10-29 REKI: state why it was served off-board (no Orin build of flash-attn, FLA/Triton, causal-conv1d)
- 2026-10-28 Last updated
- 2026-10-28 REKI costs: training 11.25 + smoke 0.60 + serving 1.20 = 13.05; totals updated
- 2026-10-27 REKI: arm follow-up (19/20 + 19/20, oscillation 1/40) and highlight video
- 2026-10-26 Add 2026-10-23-paper-vs-hardware-reki (L0 checked)
- 2026-10-23 First written up

REKI (Marrie, Pacaud, Ramanana Rahary, Chen, Böhle; Kyutai / Inria; arXiv 2610.22178) gives a VLA a memory through register tokens and measures the oscillation it removes. I voided my own latency measurement of it and then found the same oscillation in my policies with their metric.

### What I ran, test by test

**Oscillation audit (G14).** REKI's oscillation criterion on my 69 published rollouts: reversal at ≥ 3 consecutive chunks, cosine < −0.5, > 3° per chunk. **Failed episodes 6/25 = 24.0 %** (CI 11.5–43.4 %); succeeded 8/44; **all episodes 14/69 = 20.3 %**, which is the number comparable with REKI's (theirs is over all episodes). Trajectory check: the flagged failures are real re-grasp loops and pan dithering; two unflagged loops were missed. All my policies are memoryless.

### Their number, my number

|  | Paper | This arm | 
|---|---|---|
| Oscillation, no memory | 18.6 % (trained without), 14.8 % (evaluated without) | 20.3 % of all episodes (24.0 % of failed) | 
| With memory | 4.1 % | 1/40 = 2.5 % (REKI on my arm; no memory-off control) | 
| Task success | — | 19/20 and 19/20 (SmolVLA 9/10, same protocol) | 
| Criterion | reversal ≥ 3 consecutive chunks, 20 seeds/task | same criterion, 69 episodes | 
| Latency on Orin | — | void; REKI doesn't build for the Orin (served from an RTX 3090) | 

Folder · Compute: External GPU: yes for REKI (vast.ai): training $11.25 + smoke $0.60, serving $1.20, total $13.05. Oscillation audit on the Orin NX: $0. · **Papers tested, 13 of 37.** ← Papers tested 12: VLA-JEPA · Papers tested 14: Real-Time Chunking →
