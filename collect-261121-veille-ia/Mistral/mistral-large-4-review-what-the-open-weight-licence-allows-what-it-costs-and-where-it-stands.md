---
id: collect-261121-veille-ia/mistral/mistral-large-4-review-what-the-open-weight-licence-allows-what-it-costs-and-where-it-stands
title: "Mistral Large 4 review: what the open-weight licence allows, what it costs, and where it stands…"
domain: mistral
role: reference
task: article
actors: ["Mistral AI"]
dates: []
keywords: ["mistral", "licence", "model", "text", "large", "open-weight", "does", "rests"]
source: https://medium.com/@mcthkayaa/mistral-large-4-review-what-the-open-weight-licence-allows-what-it-costs-and-where-it-stands-7dc69bee8146
source_anchor: ""
source_lines: [1, 177]
section: "Mistral"
---
# Mistral Large 4 review: what the open-weight licence allows, what it costs, and where it stands against closed models

Mistral Large 4 is open-weight, not open source: Mistral released the weights on 29 October under a licence that sends a business operating a Model as a Service offering to a separate commercial licence. Mistral’s docs, read 30 October 2026, price the API at $0.70 input and $2.10 output per million tokens. Mistral Large 4 scores 26.02 on Artificial Analysis’s Intelligence Index v5.0 (High), against 58.41 for Claude Opus 5.5 (Max). Mistral does not say which checkpoint the API serves.

Mistral calls the model Le Chonk in its 29 October announcement. We did not run the model. The page rests on Mistral’s own documents, six outside evaluators and five dated user reports, all read on 30 October 2026.

Mistral states no memory requirement for self-hosting, and its documents do not settle whether the EU AI Act’s open-source exemption applies. Mistral opened a public preview on 6 October 2026 and released the weights of what it calls an improved checkpoint, a second saved set of weights, on 29 October. Its 29 October charts compare Mistral Large 4 with open-weight models only, bar one result against Claude Opus 5.5. Every figure below says whether it belongs to the preview or the release.

## Is Mistral Large 4 open source?

No: Mistral calls Mistral Large 4 open-weight and releases it under the Mistral Ecosystem Open-Weight License, a modified MIT licence with one condition aimed at businesses that run a Model as a Service offering. The weights download from the Mistral Large 4 model card on Hugging Face without an access request, and the licence text calls itself “a modified version of the MIT license”.

## What does the Mistral Ecosystem Open-Weight License allow?

The Mistral Ecosystem Open-Weight License lets anyone use, copy, modify, distribute, sell copies of, run, deploy and further train Mistral Large 4 if the copyright notice stays; condition 2, the condition aimed at businesses, turns on who the licensee is, and section 3 exempts internal use and Mistral’s own products.

- Run, deploy, fine-tune, build derivative works: Group: Allowed without asking; Licence text it rests on: Grant (b): “run, deploy, further train or otherwise modify the Model and create derivative works from it”; What the text does not settle: Condition 2 still reaches a licensee in a Model as a Service business
- Redistribute or sell copies: Group: Allowed without asking; Licence text it rests on: Grant (a): “sell copies of the Model”; condition 1: the notice “shall be included in all copies or substantial portions of the Model”; What the text does not settle: What a “substantial portion” is
- Internal use: Group: Allowed without asking; Licence text it rests on: Section 3(a): “any use that does not make the Model or its derivative works available to third parties”; What the text does not settle: “Third parties” is not defined
- Use through Mistral’s products: Group: Allowed without asking; Licence text it rests on: Section 3(b): “any use of the Model accessed through Mistral’s official products”; What the text does not settle: Which products count as official
- End-user product with the model inside a feature: Group: Allowed without asking; Licence text it rests on: Excluded from Model as a Service: “end-user products with model capabilities solely embedded within specific features or harnesses”; What the text does not settle: How much user control “solely embedded” allows
- Relaying requests to a model hosted elsewhere: Group: Allowed without asking; Licence text it rests on: Excluded: “mere relaying of requests to models hosted by others”; What the text does not settle: Nothing beyond those words
- API or hosted access giving third parties “meaningful control over the inputs, parameters, or training data”: Group: Needs Mistral’s commercial licence; Licence text it rests on: The Model as a Service definition; What the text does not settle: “Meaningful control” is not defined further
- Any commercial use by a licensee whose affiliate runs a Model as a Service business: Group: Needs Mistral’s commercial licence; Licence text it rests on: Condition 2: “If the Licensee or any of its affiliates operates a Model as a Service business”; What the text does not settle: “Affiliates” is not defined
- Price and terms of the commercial licence: Group: Not addressed; Licence text it rests on: The licence points only to Mistral’s contact form; What the text does not settle: No price, scope or duration
- Whether this is a “free and open-source licence” under the EU AI Act: Group: Not addressed; Licence text it rests on: The licence does not mention the Act; What the text does not settle: See the AI Act section

*Not legal advice. The list above quotes the licence as read on Hugging Face on 30 October 2026 and does not decide how any clause applies to a given business.*

## Can you host Mistral Large 4 for customers or sell access to it?

Hosting Mistral Large 4 for customers with meaningful control over inputs, parameters or training data is Model as a Service under the Mistral Ecosystem Open-Weight License, so the licensee needs a separate commercial licence from Mistral, and the licence names no price or terms for it. The trigger is who the licensee is: once you or any affiliate run such a business, condition 2 applies “before using the Model or its derivative works for any commercial purpose”, beyond the hosted offering itself.

Mistral’s announcement says it more plainly: “businesses that operate a Model-as-a-Service offering must obtain a separate commercial license from Mistral before using the model commercially.”

## How does Mistral Large 4 compare with Claude Opus 5.5 and other closed models?

Outside boards’ aggregate indexes place Mistral Large 4 well below the closed frontier models: Artificial Analysis scores it 26.02 on Intelligence Index v5.0 (High), against 59.43 for Gemini 4 Carbon (High) and 58.41 for Claude Opus 5.5 (Max), read 30 October 2026. Vals AI ranks the release 17th of 30 on Vals Index v2.2 at 56.42% (reasoning effort high), against 65.02% for GPT-6 Astra (reasoning effort max), read 30 October 2026.

## Is Mistral Large 4 better than Claude Opus 5.5?

No, by Mistral’s own account: in Surge Code blind pairwise evaluations, Mistral reports graders preferred Mistral Large 4’s answer on 22% of prompts and Claude Opus 5.5’s on 78% (post of 29 October 2026), “ranking behind only Claude Opus 5.5 among the models evaluated”. Surge AI publishes no Surge Code board; its one row for Mistral Large 4 (High), on DAYJOB: Healthcare, is 3.6%, tied 21st and undated, against Claude Opus 5.5 (Adaptive/Max) at 48.3%, read 30 October 2026.

Outside boards on this page with figures for both put Opus 5.5 ahead on all but two: Artificial Analysis’s Cyber Index v1, where Opus 5.5 refuses most of one task set, and Vals AI’s HLAB-Vals-1.1, 15.83% to 9.17% (Mistral Large 4 OSS at reasoning effort high), read 30 October 2026.

## Where does Mistral Large 4 sit on the Artificial Analysis Intelligence Index?

Mistral Large 4 (High) scores 26.02 on Artificial Analysis Intelligence Index v5.0, row 54 of 83, read 30 October 2026, with Medium at 23.08, the deprecated preview at 25.29 and no Low row. The model card’s “the highest among western open-weight models” holds; eight Chinese open-weight rows sit above it, led by DeepSeek V4.1 Flash 1017 (Max) at 32.49.

Closed rows at their best setting: Gemini 4 Carbon (High) 59.43, Claude Opus 5.5 (Max) 58.41, GPT-6 Astra (Max) 56.85, Claude Sonnet 5.5 (Max) 54.24 and Gemini 4 Argon (High) 50.41. One effort row of those models scores lower: Sonnet 5.5 at Low, 25.30. Scores from the older v4.3 index sit on a different scale.

Among the open-weight models Mistral compares it with, GLM-5.3 (Max) edges Mistral Large 4 on the Coding Agent Index v2.0 (48.92 against 48.28) at a higher cost per Intelligence Index v5.0 task ($5.01 against $2.06), while DeepSeek V4.1 Flash 1017 (Max) leads it on the Intelligence Index v5.0 (32.49), Artificial Analysis’s FrontierCode 2.0 run (28.47%) and the Cyber Index v1 (56.81).

## Is Mistral Large 4 cheaper to run than Claude Opus 5.5?

Per Intelligence Index v5.0 task, yes: Artificial Analysis lists $2.06 per task for Mistral Large 4 (High) against $10.04 for Claude Opus 5.5 (Max), read 30 October 2026, about a fifth of the cost, for 26.02 against 58.41, under half the score (our arithmetic). GLM-5.3 (Max) costs $5.01 per task, GPT-6 Astra (Max) $6.24 and Gemini 4 Carbon (High) $7.76. Against Mistral Large 3, Mistral’s per-token list price is higher: $0.70 input and $2.10 output per million tokens against $0.50 and $1.50 on Mistral’s docs pricing page, up 40% on both (our arithmetic).

## Does Mistral Large 4 beat GPT-6 Astra on legal and finance?

Partly: on Vals AI’s boards, read 30 October 2026, Mistral Large 4 OSS at reasoning effort high leads GPT-6 Astra at max on Finance Agent v2.1 (61.72% against 60.43%) and HLAB-Vals-1.1 (15.83% against 7.08%), and trails on the Vals Legal Index (67.68% against 68.39%) and Legal Research Bench (35.58% against 39.42%). The first two are the boards behind the 6 October post’s claim that “the model exceeds GPT-6-Astra in both cases”. On the Vals Index as a whole, Astra leads by 65.02% to 56.42%.

## Which benchmark numbers in the Mistral Large 4 announcement check out?

Five of ten figures from Mistral’s two Mistral Large 4 announcements match a benchmark owner’s board to the rounding; FrontierCode 2.0 differs, the Surge Code result has no outside board, and three figures are self-reported, two of them from the preview. The Terminal-Bench 4.0 match holds only in Mistral’s own agent harness.

- Intelligence Index 26: Who ran it: Artificial Analysis; Version and setting: v5.0, High; Board figure on 30 October 2026: 26.02, row 54 of 83; Result: matches
- Coding Agent Index 48: Who ran it: Artificial Analysis; Version and setting: v2.0, Mistral Vibe, High; Board figure on 30 October 2026: 48.28, 15th of 34; Result: matches
- Coding Agent Index 35: Who ran it: Artificial Analysis; Version and setting: v2.0, Mistral Vibe, Medium; Board figure on 30 October 2026: 35.41; Result: matches
- Terminal-Bench 4.0 41.4%: Who ran it: Artificial Analysis; Version and setting: Coding Agent Index v2.0 component, Mistral Vibe, High; Board figure on 30 October 2026: 41.41%; Result: matches
- FrontierCode 2.0 19.7%: Who ran it: Not named by Mistral; Version and setting: No harness named; Artificial Analysis’s own run at High; Board figure on 30 October 2026: 14.96%, 21st of 28; Result: differs: 14.96%
- Cyber Index 50.9: Who ran it: Artificial Analysis; Version and setting: v1, High; Board figure on 30 October 2026: 50.89, 7th of 25; Result: matches
- Surge Code, 22% against Claude Opus 5.5: Who ran it: Surge AI, per Mistral; Version and setting: Not stated; Board figure on 30 October 2026: No Surge Code board; Result: not published by the owner
- DeepSWE v1.1 61.7% (preview): Who ran it: Mistral; Version and setting: Not stated; Board figure on 30 October 2026: Not listed; Result: self-reported; no outside result found as of 30 October 2026
- Cybench 93% (preview): Who ran it: Mistral; Version and setting: Not stated; Board figure on 30 October 2026: Not listed; Result: self-reported; no outside result found as of 30 October 2026
- B3 97.1% (preview 93.3%): Who ran it: Mistral, on Lakera’s dataset; Version and setting: Not stated; Board figure on 30 October 2026: No leaderboard; Result: self-reported; no outside result found as of 30 October 2026

*Mistral’s figures from its posts of 6 and 29 October 2026; board figures read on each owner’s site on 30 October 2026, 06:29 to 07:40 UTC.*

## Which harness produced Mistral Large 4’s Terminal-Bench 4.0 score of 41.4%?

Mistral Large 4’s 41.4% Terminal-Bench 4.0 score comes from Mistral’s own agent, Mistral Vibe: it is the Terminal-Bench 4.0 component of Artificial Analysis’s Coding Agent Index v2.0, read 30 October 2026, where each configuration runs a model inside a named agent product, the program that wraps the model, and Mistral Vibe with Mistral Large 4 (High) scores 41.41%.

In Artificial Analysis’s Intelligence Index v5.0, where every model runs the mini-swe-agent harness, the release scores 24.75% and the preview 26.77%; Vals AI’s own Mini-SWE-agent run gives 26.26% and 27.78%. Mistral’s “improved by 13 percentage points” compares the Vibe result with a preview point its chart labels “ML4 Preview — self reported” at 28%. On both one-harness runs the release sits below the preview, by 2.02 and 1.52 points (our arithmetic).

## Is Mistral Large 4’s FrontierCode 2.0 score of 19.7% confirmed?

No outside run matches Mistral Large 4’s 19.7% FrontierCode 2.0 score: Mistral prints it first on its chart, without naming who ran the private benchmark or with which harness, while Artificial Analysis’s FrontierCode 2.0 run scores Mistral Large 4 (High) 14.96%, 21st of 28. Mistral’s chart puts GLM-5.3 at 17.8%; Artificial Analysis has GLM-5.3 (Max) at 21.59% and DeepSeek V4.1 Flash 1017 (Max) at 28.47%. We found no board from Cognition, the benchmark’s owner, so the two may not be the same measurement.

## Does Mistral Large 4 lead open-weight models in coding?

Mistral Large 4 does not lead open-weight models on Artificial Analysis’s Coding Agent Index v2.0, read 30 October 2026: it trails by 0.64 points (our arithmetic), with Mistral Vibe and Mistral Large 4 (High) at 48.28, 15th of 34 and second among open-weight configurations, behind Opencode with GLM-5.3 (Max) at 48.92, while using 8.45 million tokens per task against 15.91 million. Mistral’s phrase “effectively tied” fits the board.

Claude Code with Opus 5.5 (Max) leads at 61.65, and Mistral Large 4 edges Codex with GPT-6.1 Terra (Xhigh) at 48.22. The 29 October summary that the model “leads open-weight benchmarks in coding” fits the Terminal-Bench component and not the combined index. The 6 October post gave the preview a “combined Coding Agent Index score of 49.8%”; Artificial Analysis lists no preview configuration on v2.0.

## Can you run Mistral Large 4 on your own hardware?

Yes, on hardware Mistral and Hugging Face have shown: an 8-GPU node for the FP8 weights, a 4-GPU Blackwell node for NVFP4, or one Apple Silicon Ultra with 512 GB of unified RAM for 2-bit quants; Mistral itself states no memory requirement (the GPU RAM column on its docs model page reads “N/A”, and neither model card gives a figure, read 30 October 2026). Mistral publishes checkpoint sizes instead: FP8 1,060.6 GB in 54 files and NVFP4 610.0 GB in 32 files.

Its serving examples use a tensor-parallel size of 8, “for a node of 8 GPUs”, and 4 for NVFP4 “for a Blackwell node”; both cards add: “For optimal performance, we recommend using the Mistral AI API if local serving is subpar.” Hugging Face’s launch-day blog post reports:

- “The 2-bit quants run on a single Apple Silicon Ultra computer with 512 GB of unified RAM. The others require distributed inference.”
- Q2_K on one M3 Ultra: about 90 tokens per second prefill and 15 tok/s generation.
- NVFP4 on 4 x RTX6000 Pro: “32K context size, prefill 130–590 t/s, gen speed ~10 t/s”.
- An 8 x NVIDIA H200 endpoint recipe with the FP8 weights that “serves up to 131K tokens of context and processes up to 16 requests at a time”.

GGUF quants come from the llama.cpp team (ggml-org) and from Unsloth; Hugging Face’s post lists day-0 support in transformers, vLLM and llama.cpp, the model card adds SGLang serving instructions, and neither mentions Ollama.

## Has anyone reported running the Mistral Large 4 weights locally yet?

One run of the released weights had surfaced by about 07:45 UTC on 30 October 2026, and it failed: Jaddy2 on Hugging Face, 30 October 2026, 05:07:37 UTC, after the release, served the model “via vLLM behind a LiteLLM gateway” for Claude Code and reported “4 out of 4 sessions failed” on a chat-template error, “Unexpected role ‘tool’ after role ‘user’”. Jaddy2 states no hardware, and reports that DeepSeek-V4.1-Flash and GLM-5.3-Flash returned no errors on the same requests. No independent report with hardware, quantisation and speed existed, so every speed figure above is Hugging Face’s.

## Which checkpoint does the Mistral Large 4 API serve?

Mistral’s versioned documents do not say: on 30 October 2026 its docs list `mistral-large-4` and `mistral-large-4–0` as “Public Preview”, dated 6 October 2026, v26.10, and the docs changelog has no 29 October entry. The model card says “The model is also served on the Mistral API as `mistral-large-4`”, but no model ID separates preview from release.

OpenRouter’s one slug, `mistralai/mistral-large-4–0`, keeps a canonical slug dated 20261006; arena.ai carries only the preview, and Epoch AI dates its entry to the 6 October preview. No user report read for this page describes a before-and-after difference through the API. Artificial Analysis scores the release 26.02 and the deprecated preview 25.29 on Index v5.0, and the two one-harness Terminal-Bench 4.0 runs put them 2.02 and 1.52 points apart (our arithmetic), so on those boards the two checkpoints sit close together whichever one the API serves. On context, Mistral’s docs list 1M tokens, OpenRouter’s main endpoint 1,048,576 and its EU endpoint 524,288, and a preview-period user reported an HTTP 400 from Mistral’s own API on requests larger than the 524K that OpenRouter listed at the time (see the users section).

## Did Mistral cut the Mistral Large 4 API price on 29 October?

Yes, in effect: Mistral’s archived docs list $1.36 input, $0.14 cached and $4.18 output per million tokens at 02:59 UTC on 29 October 2026, and the docs pricing page read on 30 October shows $0.70, $0.07 and $2.10, about half (input down 48.5%, output 49.8%, our arithmetic). Mistral’s post states the new price without calling it a cut.

- 6 October 2026, Mistral’s changelog: “Launch pricing: 50% off for 2 weeks.”
- 7 October 2026, 13:58 UTC, archived pricing page: $1.36 / $0.14 / $4.18, on sale at $0.68 / $0.07 / $2.09.
- 22 October 2026, 13:28 UTC, archived copy of 22 October: $1.36 / $0.14 / $4.18, no sale.
- 29 October 2026, 02:59 UTC, archived copy of 29 October: still $1.36 / $0.14 / $4.18.
- 29 October 2026, Mistral’s announcement: “Starting today, Mistral Large 4 is priced at $0.70 input / M, $0.07 cached input / M, and $2.10 output / M tokens.”
- 30 October 2026, Mistral’s docs: $0.70 / $0.07 / $2.10; Batch $0.35 / $0.035 / $1.05.

Artificial Analysis’s 6 October article on the preview quoted “standard pricing of $1.36/$4.18 per 1M input/output tokens”, the older figure.

## How safe is Mistral Large 4 for cyber work?

Mistral reports that Mistral Large 4 refuses 96.6% of malicious cyber requests and also sees “a potential for uplift to conduct real attacks”; Artificial Analysis’s only refusal figure, 0%, comes from its capability tasks, a different prompt set, so neither number settles the other.

## Does Mistral Large 4 refuse malicious cyber prompts?

Mistral says Mistral Large 4 refuses 96.6% of malicious cyber requests across JailbreakBench, StrongREJECT and AgentHarm cyber prompts (preview 95.3%), “the highest we measure among open models”. Its caveat in full: “Though we observed the model refusing to comply with black-box attacks against live targets, we see a potential for uplift to conduct real attacks. We are modifying our guardrails accordingly, and will continue updating them in the months to come.”

## Does Mistral Large 4 lead open-weight models on the Artificial Analysis Cyber Index?

No: Artificial Analysis Cyber Index v1 scores Mistral Large 4 (High) 50.89, 7th of 25 and third among open-weight rows, behind DeepSeek V4.1 Flash 1017 (Max) at 56.81 and MiMo-V2.6-Pro at 56.07, read 30 October 2026. Mistral’s own chart shows the same order, beside a summary saying the model leads open-weight benchmarks in “defensive security”.

GPT-6 Astra (Max) at 33.42, Claude Opus 5.5 (Max) at 28.66 and Claude Fable 5.1 (Max) at 25.02 score lower because they refuse the CyberGym-E2E-AA tasks (100%, 98.47% and 99.24%); on DeepsecBench-AA, where none of them refuses, Mistral Large 4’s 14.96 is below all three.

## Does the EU AI Act exemption apply to Mistral Large 4?

Mistral’s documents do not settle whether the EU AI Act’s open-source exemption applies to Mistral Large 4, and this page does not decide it. Article 53(2) of the EU AI Act lifts the obligations in Article 53(1), points (a) and (b), for models “released under a free and open-source licence that allows for the access, usage, modification, and distribution of the model” whose weights and related information “are made publicly available”, and adds: “This exception shall not apply to general-purpose AI models with systemic risks.”

Article 51(2) presumes high-impact capabilities, one ground for systemic risk, when training compute “is greater than 10²⁵” floating point operations. Mistral publishes no training-compute figure, saying only that the model was “trained from scratch on 3,800 NVIDIA Grace Blackwell GPUs”, so its documents do not show which side of that line Mistral Large 4 sits.

Mistral calls its licence open-weight; whether it counts as a “free and open-source licence” in the Act’s sense is a legal question Mistral’s documents leave open. As our EU AI Act Transparency Audit recorded from the same text, Article 53(2) does not reach point (d), the public summary of training content, so that summary is owed either way. Text: consolidated version of 27 July 2026, read on EUR-Lex on 30 October 2026.

## What do users report about Mistral Large 4?

Release-week evidence is thin: the one report of running the released Mistral Large 4 weights is a vLLM chat-template failure on Hugging Face, and most user reports date from the 6 to 28 October preview, describing API context limits below the documented 1M tokens and slow responses.

- Preview period: WasserEsser on r/MistralAI, 8 October 2026, 12:49:27 UTC: “On OpenRouter, the max context size is set to 524K, and when sending a response larger than that to Mistral’s own API, I get a HTTP 400 saying the context size is exceeded.” The same post adds: “Mistral still lists ML4 as having 1M context size on their models page.”
- Preview period: EveYogaTech on r/MistralAI, 13 October 2026, 19:47:50 UTC: “for GLM 5.3 I get avg 2–3s respond time for a simple ‘Hi’ and with Mistral Large 4 is about 8 seconds.”
- Preview period: arrrthurr on GitHub, 27 October 2026, 15:35:22 UTC, on Mistral’s Vibe CLI 3.0.3: “The session budget appears to stay at 262,144 tokens.”
- After 29 October: almindor on Hacker News, 29 October 2026, 15:07:25 UTC: “still too expensive and very slow when thinking” and “I use Opus 5.5 at work and it’s still much better than this”, without saying which checkpoint answered.
- After 29 October: Jaddy2’s vLLM report on Hugging Face, 30 October 2026, 05:07:37 UTC, the failure described above.

In the threads read for this page, 14 accounts called the model slow, mostly against GLM 5.3 and mostly during the preview, and two called it fast; the count is our reading of those threads, not a sample.

## Who should use Mistral Large 4, and who should skip it?

Use Mistral Large 4 if cost per task and the right to self-host or fine-tune under the licence matter more than the top of the leaderboard; skip it if you need leaderboard-top capability, if you host models for third parties and have not cleared condition 2 with Mistral, or if you need an independent local-run report today. On Intelligence Index v5.0, eight Chinese open-weight rows and every closed model in the Artificial Analysis rows in this review, at its best setting, score higher. The licence lets anyone run, fine-tune and redistribute the weights without asking, while a Model as a Service business needs Mistral’s commercial licence first, on terms Mistral has not published. The only reported run of the released weights so far failed on a chat-template error.

## What is still unknown about Mistral Large 4?

Mistral’s technical report is “coming soon” and was unpublished on 30 October 2026, so the training compute, any memory requirement and the checkpoint the API serves remain unstated. No independent local run had been reported, Artificial Analysis had no article on the release, and the official Terminal-Bench 4.0, DeepSWE, SWE-bench Pro and Cybench boards did not list the model. We will update the page when the report or the first independent run appears.

**Verdict.** Choose Mistral Large 4 if cost per task and licence terms matter more than the top of the leaderboard, since Artificial Analysis’s Intelligence Index v5.0, read 30 October 2026, puts Mistral Large 4 (High) at $2.06 per task against $10.04 for Claude Opus 5.5 (Max), with scores of 26.02 against 58.41, and read condition 2 of the licence before you host it for anyone else.

Six organisations outside Mistral had published results on Mistral Large 4 by 07:40 UTC on 30 October 2026, and only two of them, Artificial Analysis and Vals AI, label a row as the 29 October release. Artificial Analysis scores the release 26.02 at High and 23.08 at Medium on Intelligence Index v5.0, row 54 of 83 for High, at $2.06 per index task; gives Mistral Vibe with the model at High 48.28 on Coding Agent Index v2.0, 15th of 34; scores it 50.89 on Cyber Index v1, 7th of 25; and records 24.75% on Terminal-Bench 4.0 under the mini-swe-agent harness and 14.96% on its own run of FrontierCode 2.0. Vals AI, at reasoning effort high, ranks Mistral Large 4 OSS 17th of 30 on Vals Index v2.2 at 56.42% plus or minus 1.26, with 26.26% on Terminal-Bench 4.0. arena.ai carries the preview only, licensed “Proprietary” in its dataset: Text 95th at 1430.1, WebDev 48th at 1544.3 and Vision 42nd at 1272.2. Epoch AI dates its entry to the 6 October preview and gives an ECI of 150.48, 59th of 256, against 165.98 to 168.92 for seven closed models. Surge AI lists Mistral Large 4 (High) at 3.6% on DAYJOB: Healthcare, tied 21st, with no date for the row, and has no Surge Code board. KORA lists a preview row only. Cognition, Lakera, tbench.ai, Datacurve, SWE-bench, Scale, Cybench, Zapier, Andon Labs, Collinear, Proximal, UC Berkeley RDI, XLANG, ARC Prize and METR had published nothing on the model. Mistral’s own DeepSWE, Cybench, B3 and Surge Code figures have no outside result as of 30 October 2026, and Mistral’s charts are not counted here.

*Disclosure. Our writing workflow runs on Anthropic models, including Claude Opus 5.5, the one closed model Mistral compares Mistral Large 4 with in its 29 October post. So every judgement on this page rests on Mistral’s own documents and on outside evaluators, each named with its setting. We run no hands-on tests; this review is built from Mistral’s published documents, outside evaluations and dated user reports.*
