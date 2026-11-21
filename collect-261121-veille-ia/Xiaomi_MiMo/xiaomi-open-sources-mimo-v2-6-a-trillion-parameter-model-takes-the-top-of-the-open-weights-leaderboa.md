---
id: collect-261121-veille-ia/xiaomi-mimo/xiaomi-open-sources-mimo-v2-6-a-trillion-parameter-model-takes-the-top-of-the-open-weights-leaderboa
title: "Xiaomi open-sources MiMo-V2.6: a trillion-parameter model takes the top of the open-weights leaderboard"
domain: xiaomi-mimo
role: reference
task: article
actors: ["Xiaomi"]
dates: []
keywords: ["xiaomi", "mimo-v2", "model", "open-weights", "flash", "series", "models", "under"]
source: http://dev.to/aifrontierpost/xiaomi-open-sources-mimo-v26-a-trillion-parameter-model-takes-the-top-of-the-open-weights-3j2d
source_anchor: ""
source_lines: [1, 33]
section: "Xiaomi_MiMo"
---
*Originally published at AI Frontier Post.*

Xiaomi has open-sourced its latest MiMo series. Over the past two days, the phone maker turned AI lab released MiMo-V2.6 — two omnimodal models, led by a flagship it says outranks every other open-weights model on a composite industry benchmark. The checkpoints went up on Hugging Face ungated, under an MIT license: anyone can download them, fine-tune them, and use them commercially.

The headline score is vendor-reported, so treat it as a claim rather than a verdict: 46.32 on Artificial Analysis's Intelligence Index v4.3. What is harder to wave away is everything that shipped around the weights. Xiaomi streamed the six-day reinforcement-learning run that produced the models live, then published the training environments and the RL code alongside the checkpoints. This is a release engineered to be reproduced, not just admired.

The series comes in three flavors. MiMo-V2.6-Pro is the flagship, which Xiaomi describes as its most capable model to date. MiMo-V2.6-Flash is the lighter, cheaper sibling built for high-volume work. And MiMo-V2.6-Pro-UltraSpeed is a serving option that pushes output up to 20 times faster at roughly ten times the price — for workflows where latency is the whole game.

Under the hood, Pro is a sparse mixture-of-experts model: 1.02 trillion total parameters with about 42 billion active per token, spread across 384 routed experts of which eight fire on any given token. The backbone is 70 layers mixing sliding-window and global attention, a dedicated vision tower handles images, and separate encoders handle audio. Both models accept text, image, video, and audio as input and emit text, with a one-million-token context window. An independent speed measurement through Xiaomi's API recorded roughly 130 output tokens per second.

The number Xiaomi is leading with is the Artificial Analysis Intelligence Index: Pro lands at 46.32, ahead of Z AI's GLM-5.3 (45) and Moonshot's Kimi K3 (44) — the top open-weights result on that board. For calibration, the same board lists the closed-source Grok 4.7 at 46, and Artificial Analysis prices Pro's inference at about $0.13 per Index task — the figure that will get CFOs' attention.

Xiaomi's own claim is bolder: that Pro performs on par with Claude Opus 5 and GPT-5.6 Sol across most agent benchmarks. The fine print in its own tables is less flattering. On GDPval 2.1, Pro scores 1,673 against 1,735 for Claude Fable 5.1 and 1,708 for Opus 5. On DeepSWE v1.1 it trails DeepSeek V4.1 Flash (74.2), Opus 5 (74.0), and GPT-6 Astra (74.0) with 71.9. The honest read: best of the open lot, still short of the best closed systems. All of this is vendor-reported until independent evaluators replicate it.

API pricing is unchanged from the V2.5 series — Pro at $0.435 per million input tokens and $0.87 for output, Flash at $0.14 and $0.28. Xiaomi's framing is that intelligence went up while the price stood still, pushing the cost-performance frontier outward.

Benchmarks are marketing; training stacks are moats. And here Xiaomi did something genuinely unusual: it streamed the production reinforcement-learning run live, as it happened. In under six days, Flash and Pro each completed 30 RL steps across roughly 750,000 trajectories, at reported costs of about $850,000 and $2.62 million respectively. Average pass rates on the training tasks rose 25% and 12% in relative terms, and on DeepSWE v1.1 — a held-out software-engineering benchmark — Flash climbed from 48.8 to 65.68 and Pro from 58.4 to 72.57.

The engineering behind it is aggressive: a fully asynchronous architecture processing 1,568 samples per update and 3.5 to 3.7 billion tokens per step, a multi-task suite spanning coding, general agents, visual and cybersecurity work, and group-based reward signals to sharpen the feedback. Notably, Xiaomi also documents explicit defenses against reward hacking — a frozen router to limit drift, adversarial evaluation, anomaly detection, and cross-checking between verifiers — the kind of plumbing detail labs usually keep to themselves.

Then the company published it: the technical report, more than 7,000 task environments, and the RL code, framed as a reproducible experiment in scaled reinforcement learning and model self-improvement. One observer noted the release was weight-first — checkpoints and model card appeared on Hugging Face before Xiaomi's own blog carried any announcement. The repository came first; the narrative later.

An MIT-licensed model at the top of the open-weights leaderboard changes the economics of building. Anyone can now self-host a model in the top tier of open systems instead of renting one by the token — or start from its weights and specialize it. The models are live in AI Studio, MiMo Code, MiMo Desktop (which leaves early access with this release), Xiaomi's API platform, and OpenRouter.

Xiaomi's demos reach past chatbots into what it calls "Vibe World": coordinating agents to build and visually test interactive 3D scenes, generating Blender assets, steering a Franka Panda robotic arm in closed loop from camera feeds, producing frontends and presentations, assembling videos, and composing music — including an orchestral piece the model scored and converted to MIDI itself. Two research showcases go further: designing metal-organic framework candidates for capturing PFAS chemicals alongside the company's materials experts, and a 6,000-plus-line Lean 4 formalization of the "Period Three Implies Chaos" theorem that the Lean kernel verified with no unfinished proofs.

First, reproduction. Xiaomi handed the community the environments and the code; if independent teams can replicate the RL recipe, this becomes a playbook for the whole open ecosystem rather than just a checkpoint drop.

Second, the chase. Kimi K3, GLM-5.3, and Qwen3.8 Max now have a new leader to hunt. The Chinese open-weights race is the fastest-moving leaderboard in AI, and it just got reset.

Third, the closed-lab response. When the open frontier costs $0.435 per million input tokens, every closed provider has to justify its premium — on reliability, on features, or on something the open models still cannot do.

## Top comments (0)
