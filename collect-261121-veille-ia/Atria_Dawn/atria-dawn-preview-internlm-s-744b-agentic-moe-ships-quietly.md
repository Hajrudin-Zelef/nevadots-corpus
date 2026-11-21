---
id: collect-261121-veille-ia/atria-dawn/atria-dawn-preview-internlm-s-744b-agentic-moe-ships-quietly
title: "Atria Dawn Preview: InternLM's 744B Agentic MoE Ships Quietly"
domain: atria-dawn
role: reference
task: article
actors: []
dates: ["2026-09-11"]
keywords: ["model", "preview", "card", "atria", "dawn", "agentic", "internlm", "quietly"]
source: https://www.orcarouter.ai/blog/atria-dawn-preview-quiet-release
source_anchor: ""
source_lines: [1, 83]
section: "Atria_Dawn"
---
Shanghai AI Laboratory's InternLM organization quietly published Atria Dawn Preview on Hugging Face this week — a preview of a 744B-parameter MoE agentic model that arrives with MIT-licensed weights, a 1M-token context window, and a benchmark table measured against DeepSeek V4 Pro, Kimi K3, Qwen3.8-Max, GLM-5.3, GPT-5.6 Sol, and Claude Opus 5. The repository went live on September 11, 2026 — three days before this piece — with a companion FP8 checkpoint following on September 12, and the vendor has not yet announced the model anywhere: no blog post, no paper, no pricing, no API. What exists today is a fully populated model card and roughly 1.5TB of open weights. That makes Atria Dawn Preview a shipped-quietly release, and the interesting questions are what the card lets you verify, what it asks you to take on faith, and whether this is the first step toward a larger Atria family from the lab that has spent the year shipping the Intern-S2 and InternLumina series.

## The card is the announcement

Because there is no announcement, the Hugging Face card is doing all the talking. It describes Atria Dawn Preview as "a preview version of a new-generation agentic model" developed by the Shanghai AI Laboratory, trained on a 744B-parameter MoE foundation model, and aimed at "research and engineering scenarios that require continuous environmental understanding, tool use, and multi-step task completion." The pitch line — "From Research Questions To Verifiable Results" — is about driving open-ended problems toward executable, verifiable, reproducible outcomes, with the model combining task objectives and environment feedback across problem analysis, solution design, tool use, code implementation, experiment execution, result analysis, and failure recovery.

The card organizes that agentic scope into four capability areas, each aimed at end-to-end delivery rather than chat:

• **Discovery** — retrieving and organizing evidence, deep research, turning research questions into executable experimental plans

• **Creation** — building software, interactive applications, games, data visualizations, and machine learning systems

• **Delivery** — transforming documents, data, and design requirements into reports, presentations, and other structured deliverables

• **Cybersecurity** — analyzing security issues, validating vulnerabilities, applying fixes, and re-validating in authorized environments

Also on the card but unexplained: a website (atria-asi.com), a GitHub org (atria-asi) mirroring the README, and an X account (@AtriaASI). The "ASI" suffix is the one genuinely strange detail — nothing in the card says what the branding is meant to imply, and we are not going to read more into it than the repo itself does. What is knowable is that this is a distinct product line codename, separate from the Intern-S2 and InternLumina names the lab has used all year.

## What the config actually says

The weights and configuration are downloadable today, which is where a quiet release stops being a rumor. The architecture string is GlmMoeDsaForCausalLM — the same DSA-style sparse MoE family the GLM line uses, with the config routing 8 of 256 routed experts per token across 78 layers (the active-parameter count is not published on the card; on a 744B MoE of this shape it lands in the tens of billions). The context window is set to 1,048,576 tokens — a full 1M, matching the long-context class the lab has pushed in its other recent releases. The tokenizer carries image, video, audio, and transcription markers, though the chat template explicitly tells the model it has no multimodal input ability and to say so if handed media — so treat the modality tags as inherited plumbing, not a claim of vision or audio input.

The weights tell the practical story:

• **Checkpoints** — two: the BF16/F32 instruct model and Atria-Dawn-Preview-FP8, an FP8-quantized instruct model

• **Size** — the BF16 release is about 1.5TB across 353 safetensors shards; the FP8 release is roughly 756GB

• **License** — MIT, for both code and weights, with no restriction clause on the card — unusually permissive for a lab release of this scale

• **Distribution** — Hugging Face and ModelScope, English and Chinese READMEs, a chat template and a generation config in the repo

• **Hosting** — no inference providers are listed on the card, and no API is mentioned anywhere

That last line matters for the framing: this is a weights release, not a service launch. Anyone who wants to run Atria Dawn Preview today must download about 756GB (FP8) or 1.5TB (BF16) and serve it themselves — no vendor endpoint, no pricing, no quota page.

## The benchmark table, read carefully

The card carries a 16-row benchmark table comparing Atria Dawn Preview against DeepSeek V4 Pro, Kimi K3, Qwen3.8-Max, GLM-5.3, GPT-5.6 Sol, and Claude Opus 5 across agentic, tool-use, coding, and research benchmarks. Every number in it is vendor-reported — the lab ran or commissioned the evals, and none of them have been independently reproduced, because no independent evaluation of Atria Dawn Preview exists yet. Artificial Analysis returns no page for the model as of today. So the right reading is "here is how the vendor says it compares," not "here is how it compares."

With that caveat nailed down, the highlights are genuinely interesting. Atria Dawn Preview tops the listed field on BrowseComp at 92.5 (against GPT-5.6 Sol's 92.2 and Claude Opus 5's 90.8), on CyberGym at 86.5 (ahead of DeepSeek V4 Pro's 83.3, GLM-5.3's 84.5, and GPT-5.6 Sol's 83.6), and on DeepSearchQA at 96.0. It also posts a strong BFCL v4 of 77.0 against DeepSeek V4 Pro's 71.4 and Kimi K3's 69.1. The weak spots are just as informative:

• **Terminal-Bench 2.1** — 78.3, well behind Qwen3.8-Max's 89.3 and Claude Opus 5's 90.2, suggesting the coding-agent chops are not yet flagship-tier

• **SWE-bench Pro** — 59.6, ahead of DeepSeek V4 Pro (58.3) but far behind Claude Opus 5's 74.7

• **MLE-bench Lite** — 86.2, which trails GPT-5.6 Sol (88.9) and Claude Opus 5 (88.0) despite the Discovery pitch

• **JobBench** — 50.3, lowest of the models the card lists, and one of several rows where an open 744B preview underperforms the closed flagships it is printed next to

The honest summary: the card claims real strengths in research retrieval, browser-grounded tasks, and security validation, while the classic coding and knowledge-work rows read mid-pack. A preview that cherry-picks nothing is more credible than one that wins everything — but none of it is verified, and a model at this scale with this much evaluation surface deserves an independent look before anyone bets a pipeline on it.

## Where it sits in the InternLM lineup

Atria Dawn Preview lands in the middle of an unusually busy stretch for the lab. The vision-and-science flagship Intern-S2-397B appeared on September 13, the same family whose smaller siblings (Intern-S2, Intern-S2 Mobius) have shipped since mid-year, and the vision-focused InternLumina-U2 went up at the start of September. Atria is a separate codename aimed at general agentic work rather than scientific or multimodal specialization — and unlike Intern-S2-397B, which carries an Apache-2.0 license and its own announcement ecosystem, Atria Dawn Preview arrived without ceremony under MIT. Whether Atria is a one-off preview or the first member of a new series is unconfirmed; the "Preview" suffix and the bare repo, with no roadmap text anywhere, argue for the cautious reading.

## What isn't known yet

The useful habit with a quiet release is to list what remains open. On Atria Dawn Preview, that list is long:

• **An announcement** — nothing from the lab, the website, or the X account as of September 14; the repo appeared first, which is how several recent Chinese-lab releases have gone, but "how they usually go" is not confirmation

• **A paper or technical report** — none linked; the training data, the RL pipeline, and the active-parameter count are all unpublished

• **The "Atria ASI" branding** — the domain and handle exist and the card links them, but what entity sits behind them and what the initials are meant to stand for is not stated

• **Independent scores** — none; no Artificial Analysis entry, no arena presence, nothing to cross-check the card's table against

• **Hosted availability** — no vendor API, and no major inference gateway is serving it yet; the only way to run it is to stand up roughly 756GB to 1.5TB of weights yourself

• **Serving support** — the architecture is a known DSA-MoE lineage, which is the kind of thing that tends to get runtime support quickly, but no framework support has been announced for this specific checkpoint

## What you can actually do with it

Concretely, today, three things are true. First, you can download Atria Dawn Preview — the MIT license covers both checkpoints, and the FP8 release makes the storage and serving math meaningfully easier than the BF16 original, though either is a multi-GPU proposition. Second, you cannot call it anywhere: there is no hosted endpoint, and that is exactly the situation where a routing platform earns its keep. We do not route Atria Dawn Preview yet — it is not on OrcaRouter, and this page is not pretending otherwise — but **the same key that does not have Atria today already reaches 196 other models through one API, with provider list price passed through at 0% markup and automatic failover between providers**. When Atria Dawn Preview reaches a routed provider, it will appear at list price the same day the vendor's numbers update, which is what pass-through pricing means in practice. Until then, the model is a download-and-run project, and the safest way to evaluate it is on hardware you control, with an independent benchmark of your own — not on the card's word.

## What to watch next

Four signals decide whether this quiet drop becomes a story you act on. The first is the announcement: a proper launch post or paper would answer the architecture, the training recipe, and what "Preview" is a preview of. The second is an independent evaluation — a model this size with a card this confident needs its BrowseComp and CyberGym claims checked against a neutral harness, and the absence of any Artificial Analysis entry is the single most conspicuous gap. The third is hosted availability: an open 744B MoE with MIT weights and a 1M context window is exactly the profile that tends to surface on inference providers within weeks, and the first one to serve it will set the practical price of using it. The fourth is the family question — whether Atria is a one-off or a series, and whether the "ASI" branding signals where the lab intends to take the line next. None of that is knowable from the repo, and that is the point: the repo gave us the model, and the next two weeks should give us the rest.

## Compared in this article3

Detected from this article · Benchmarks: Artificial Analysis · updated daily
