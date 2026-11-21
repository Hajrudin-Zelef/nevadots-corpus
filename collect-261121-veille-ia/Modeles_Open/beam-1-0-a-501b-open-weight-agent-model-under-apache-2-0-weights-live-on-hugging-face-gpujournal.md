---
id: collect-261121-veille-ia/modeles-open/beam-1-0-a-501b-open-weight-agent-model-under-apache-2-0-weights-live-on-hugging-face-gpujournal
title: "Beam 1.0: a 501B open-weight agent model under Apache 2.0 — weights live on Hugging Face | GPUJournal"
domain: modeles-open
role: reference
task: article
actors: []
dates: []
keywords: ["beam", "model", "apache", "weights", "hugging", "face", "agent", "live"]
source: https://gpujournal.ai/story/reflection-beam-501b-open-weights
source_anchor: ""
source_lines: [1, 19]
section: "Modeles_Open"
---
NEWS → · Models · Reflection

# Beam 1.0: a 501B open-weight agent model under Apache 2.0 — weights live on Hugging Face

Oct 27, 2026

Launched by Reflection on Oct 26, 2026, with the open weights live on Hugging Face: Beam 1.0 is a 501-billion-parameter mixture-of-experts model for agentic coding and tool use — 23.4B active parameters, a 1M-token context window and an Apache 2.0 licence with no revenue gate. It is also a multi-GPU job: the BF16 weights alone are about 1,003GB.

Image: Reflection

What shipped, from the model’s own Hugging Face card and repository, read Oct 27, 2026: an instruction-tuned mixture-of-experts model (52 layers — 3 dense, 49 MoE; 256 experts, 8 active per token), 501.3B total and 23.4B active parameters, a 1M-token context window, English-first, knowledge cutoff Jun 30, 2026, a chat template and a full safety report. The release ships in three numerics — BF16 (188 files, about 1,003GB in the repository), FP8 and NVFP4 builds — public and ungated, with the Apache 2.0 licence file in the repo.

The scores are Reflection’s own, published on the card and stated as such: SWE-bench Verified 81.6, GPQA Diamond 90.6, AIME 2026 97.7, Terminal-Bench 2.1 80.1, SWE-Bench Pro 66.1 and HLE 51.1 with tools (36.0 without). The card’s efficiency claim is that Beam reaches comparable solution quality to leading open models using roughly 3x fewer reasoning tokens. No third party had reproduced the table when this story was published — treat every figure as maker-reported until outside runs agree.

The serving maths, by our published method (parameters × bytes, plus headroom): full BF16 needs about 1,003GB of weights — sixteen 80GB cards before KV cache; the 8-bit build halves that to roughly 501GB, an 8-GPU node; the NVFP4 build lands near 251GB, which fits four 80GB cards tightly. Nobody serves Beam on one card, and the 23.4B active count is what keeps its per-token cost closer to a 23B model than a 501B one. We have added Beam to the model catalogue with these figures. One honesty note: Reflection also sells a hosted API, and its card’s stated token prices disagree with launch-day reporting — so we publish no API rate for it.

### What it means if you rent GPUs

For builders, a frontier-scale Apache 2.0 checkpoint with no revenue gate or custom licence changes the build-versus-buy maths for agent systems: you can download it, fine-tune it and ship it commercially today. The limits are equally plain: the benchmark table is the maker’s own, the safety report is the maker’s own, and the rental bill starts at a four-card node even in the smallest build. When independent benchmarks land, we will update this story and the catalogue entry.
