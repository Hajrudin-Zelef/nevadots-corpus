---
id: collect-261121-veille-ia/modeles-open/aion-1-0-instruct-unannounced-hf-release-jan-2027-deadline
title: "Aion 1.0 Instruct: Unannounced HF Release, Jan 2027 Deadline"
domain: modeles-open
role: reference
task: article
actors: []
dates: ["2026-11-18"]
keywords: ["microsoft", "repository", "instruct", "aion", "weights", "model", "release", "windows"]
source: https://www.orcarouter.ai/blog/aion-1-0-instruct-quiet-release
source_anchor: ""
source_lines: [1, 121]
section: "Modeles_Open"
---
The repository **microsoft/Aion-1.0-Instruct-2.3B** was created at 17:05:50 UTC on 18 November 2026. Nine minutes later a second repository appeared beside it, carrying an INT4 ONNX build of the same weights. As of this morning the base repository has three likes, zero downloads and exactly one commit, whose message reads "Duplicate from asgmodels/Aion-Instruct-v1" — and that source repository now returns 401 to anyone who asks. There was no launch post, no thread, no model-card sentence beginning "we are excited". **Aion 1.0 Instruct** was not announced; it was published, which is a different act, and the gap between the two is the reason this piece exists.

What follows separates three kinds of claim, and never blurs them. Repository facts were read directly from the files this morning — the configuration, the weights index, the chat template, the card, the sibling repositories. Vendor claims are Microsoft's own numbers on Microsoft's own harness, reproduced as claims. And a third category matters more here than usual: Microsoft has already published a migration calendar for Windows developers on its own documentation, and that calendar is more consequential than anything in the model card. Where the repository is silent, this piece says so rather than inferring.

## Two clocks, and only one of them is quiet

The Hugging Face publication is the quiet clock. The loud one is older and has been running for months.

• 31 May 2026 — **microsoft/Aion-Instruct-Preview-Sample** goes public on GitHub, a WinUI 3 sample app for an on-device language model on Copilot+ PCs. Its README states plainly: "This package contains the final Aion Instruct model planned for release with Windows."

• 2 June 2026 — the Microsoft Edge developer blog announces Aion-1.0-Instruct as a pre-release model in Edge Canary and Dev, replacing the 4B Phi-4-mini behind the Prompt and Writing Assistance APIs, described as smaller and faster and able to run on CPU inference where no GPU exists. The same post promises a "planned open-source release on Hugging Face in July."

• 2 November 2026 — the GitHub sample ships release v1.1.0.2, adding AMD VitisAI support; the repository has been maintained steadily since June without a single public announcement.

• 16 November 2026 — Microsoft's Windows AI documentation for Phi Silica carries a notice, timestamped two days before the weights landed, saying "Phi Silica is being replaced by Aion Instruct" and telling developers to start planning the transition now.

• 18 November 2026, 17:05 UTC — the weights themselves, MIT-licensed, on Hugging Face. No accompanying post anywhere that a search can find.

So the honest description is not "Microsoft shipped a model in secret." It is that Microsoft has been telling Windows developers about this transition in documentation and sample code for nearly six months, promised open weights in July, delivered them four and a half months late, and did not mark the delivery with an announcement. If you only read launch coverage, you would not know Aion 1.0 Instruct exists. If you read the Phi Silica API docs — which is where a Windows developer actually lives — you have been on notice since at least mid-November.

## What the files prove, without needing Microsoft to confirm anything

None of the following depends on an announcement. It is all in the repository, and most of it is checkable against the weights file itself.

• Parameter count — the card says "~2.29B", and the arithmetic agrees: *model.safetensors* is 4,577,698,488 bytes of bfloat16, which is 2,288,849,244 parameters at two bytes each. Input and output embeddings are tied, so that count does not double at the head.

• Shape — 28 layers, hidden size 2,048, MLP intermediate 8,192 with SiLU, 24 query heads against 8 key/value heads at head dimension 128, RMSNorm with epsilon 1e-6, vocabulary of 200,029 tokens.

• Attention pattern — the config's *layer_types* array lists 21 *sliding_attention* layers and 7 *full_attention* layers, interleaved. The sliding window is 2,048 tokens.

• Context — 32,768 tokens, reached by YaRN rather than by native training length. More on that below, because it is the single most misreadable number in the file.

• Licence — MIT, copyright Microsoft Corporation, with a LICENSE file present in the repository. The front-matter's *license_link*, however, points at *asgmodels/Aion-Instruct-v1/blob/main/LICENSE*, which is the private repository that returns 401. The licence you can read is the one in the tree; the one the card links to is not reachable.

• Custom code — this is not a stock architecture. Running it requires *trust_remote_code=True* and pulls in *configuration_aion_instruct.py* and *modeling_aion_instruct.py*. It was exported with transformers 5.x, so older toolchains will not load it without attention.

• Chat format — *<|system|>*, *<|user|>*, *<|assistant|>*, every turn closed by *<|end|>*, which is also the EOS token. Tool schemas go into the system prompt between *<|tool|>* and *<|/tool|>*; calls come back as *<|tool_call|>{"name": …, "arguments": …}<|/tool_call|>*; results return inside a user turn prefixed *<|tool_response|>*. The template throws if a leading system message contains any of the reserved control tokens, which is a sensible guard and an unusual one to see in a card.

• Languages — English, German, Spanish, Portuguese, Italian, French, Chinese and Japanese. The card states multilingual data is about 6% of the training mix.

• Knowledge cutoff — December 2025.

• The quantised build — *microsoft/Aion-1.0-Instruct-2.3B-ONNX-WebGPU*, created at 17:14:58 UTC the same evening, nine minutes after the base repo. INT4 block-wise weights exported from a quantization-aware-trained checkpoint, FP16 activations and KV cache, opset 21, roughly 1.3 GB, targeting WebGPU through ONNX Runtime GenAI.

The most useful thing in that list is the last two items taken together. An unquantised bf16 release plus a QAT-derived INT4 build, published within nine minutes of each other, tells you the shipping artefact was ready before the transparency artefact was. That is the shape of a release that was held back and then quietly let go, not one that was rushed.

## The 32K context is a 2K model stretched sixteen times

Read the *rope_parameters* block carefully and the context number changes character. The full-attention layers declare *rope_type: "yarn"* with *original_max_position_embeddings: 2048* and *factor: 16.0*. Two thousand and forty-eight multiplied by sixteen is 32,768. The sliding-attention layers declare plain *rope_type: "default"* at theta 32,000, with the same 2,048 window.

The reading that follows from the config, and it is a reading rather than a vendor statement, is that this model was trained at a 2,048-token position range and extended to 32K afterwards through RoPE scaling applied only to seven of its twenty-eight layers. Three quarters of the stack can never attend further back than 2,048 tokens by construction; long-range mixing happens in the seven full-attention layers alone. That is a legitimate and increasingly common design for on-device models, where KV cache size is the binding constraint on memory — a sliding window keeps the cache flat no matter how long the conversation runs, which is exactly what you want on a laptop that also has to render a desktop.

It also means the 32,768 figure should not be read as "handles a 32K document as well as a model trained at 32K." Microsoft's own card is careful here, describing the window as enough to process "multi-page documents, lengthy email threads, and complex technical specifications entirely on-device." It does not claim retrieval quality at the far end. If long-context recall is what you need, that is the first thing to measure yourself, and it is not in the card.

## The benchmarks are Microsoft's own, and two of them are flagged loose

Every number below is vendor-reported. The card says all of them ran through EleutherAI's lm-evaluation-harness where a task implementation existed and through each benchmark's official repository otherwise, with GPT-4o as the judge on the LLM-judged tasks. There is no independent reproduction, no published harness config, and no eval script in the repository, so none of it can be re-run from what shipped.

• Knowledge — MMLU 55.15, MMLU-Pro 33.35, multilingual MMMLU 45.51, GPQA 28.18.

• Maths — GSM8K 74.91 under loose matching, GSMPlus 49.56, MATH-500 48.00.

• Instruction following — IFEval 83.90 as the mean of prompt-level strict and instruction-level strict, IFBench 32.00 at prompt-level loose, Multi-IF 64.70.

• Translation — chrF++ 43.18.

Two of those deserve a second look before anyone quotes them. GSM8K at 74.91 is explicitly loose matching, which credits an answer that contains the right number even when the reasoning or formatting around it is wrong; strict GSM8K on a 2.3B model would land meaningfully lower. IFBench at 32.00 is prompt-level loose, which is the most forgiving variant of the most forgiving instruction-following benchmark in the set. The strict-flavoured numbers in the same table — MMLU-Pro at 33.35, GPQA at 28.18 — are what a 2.3B model's world knowledge actually looks like, and they are unremarkable, which is what the card itself says in its limitations section: small models "can miss fine details, provide misstatements, or show inability to follow complex multi-step reasoning."

The absence that matters more than any single score: the card never compares Aion 1.0 Instruct to Phi Silica, the model it replaces. It says the training data "is an extension of the data used for training Phi Silica" and stops there. For a developer being told to migrate in January, the one delta that would inform the decision — does the replacement score better or worse than what I have — is not in the document.

## The safety numbers are a picture, not a table

The card names three metrics it wants to improve: XPIA, DIC and ECI. It then declines to give you a number for any of them. The figures live only in *images/aion_safety_bchart.png*, a bar chart headed "Azure AI Safety Suite", with axis categories for adversarial content safety, election critical information, indirect prompt injection and document injection, scored on a direction-normalised 0-to-100 scale where the value is one minus the failure rate, times one hundred. There is no numeric column anywhere in the text.

That is a real gap and worth stating plainly rather than working around: the three metrics Microsoft itself flags as needing work are the three you cannot read without OCRing a PNG. The validators did publish a methodology footnote — prompts type-identical, verified by SHA-256 — which is more rigour than most cards show. They just did not publish the values. And the parallel transparency notes for the Windows validation package go further, stating that cross-prompt injection attacks and emotional inference are untested for Responsible AI compliance and that the package "may have vulnerabilities." For a model whose headline feature is running unattended over your local documents, prompt injection is not a footnote.

## What Microsoft has already told developers, and what breaks

This is the part a reader can act on today, and it does not depend on the Hugging Face release at all. It is on the Phi Silica page of Microsoft Learn, whose updated timestamp reads 16 November 2026.

• Phi Silica is being replaced by Aion Instruct. The notice says so directly and tells developers to start planning the transition now.

• December 2026 — Aion 1.0 Instruct begins rolling out to Windows Insider Preview devices. Phi Silica remains present; a Windows Controlled Feature Rollout decides which model is active, and developers can force a side-by-side comparison through a registry key.

• January 2027 — the model rolls out to retail devices and Phi Silica is removed.

• The access gate disappears. Phi Silica's APIs are a Limited Access Feature requiring an unlock token; the documentation states that "unlike Phi Silica, LAF tokens are no longer needed with Aion Instruct." For anyone who has ever filed the LAF request form and waited, that is the most consequential sentence in the entire migration.

• Your LoRAs do not come with you. The sample repository is explicit: Phi Silica-compatible LoRAs are not compatible with Aion Instruct, and new adapters must be built with Foundry Toolkit.

• Outputs get watermarked. The new stack uses invisible watermarking to support EU AI Act compliance, and the notes warn the mark may become visible when output is pasted into certain applications.

• Hardware support is three execution providers and no fallback. The validation package targets ARM64 Snapdragon through catalog-managed QNN at minimum EP 2.2480.49, x64 Intel Lunar Lake through OpenVINO at 1.8.95, and x64 AMD Ryzen AI through VitisAI at 1.8.80. There is "no CPU fallback when a supported NPU provider is unavailable." Note the gap against the June Edge post, which advertised CPU inference as the way Aion reached devices without a GPU — that was the browser build, and the Windows API path is narrower.

• The validation package is not shippable. "It is not for end-user or customer use. Do not ship any part of this package to end consumers." The model is final; the delivery around it is not, and performance measured today is explicitly not representative of the January build.

## What is still unconfirmed, and how to hold it

• No announcement. Nothing on Microsoft's blogs, no model-card statement of intent beyond "an introduction to what's coming", no technical report. The only public trace outside the repo is a Hacker News submission from 18 November with one point.

• No independent evaluation. As of 19 November the model does not appear on Artificial Analysis, and no third party has published a run. Every number in the table above is Microsoft grading Microsoft.

• No GGUF. The card lists "Library names: PyTorch, ONNX, GGUF", but no GGUF repository exists under the Microsoft organisation. The only quantised artefact published is the ONNX WebGPU build. Treat the GGUF line as a statement of intent until a file exists.

• No training detail. Token counts, compute, data mixture beyond "public documents plus synthetic data" and the 6% multilingual share — none of it is stated.

• No pricing, because there is nothing to price. This is an OS component and a weights download, not a hosted endpoint. It is also not in the OrcaRouter catalogue, along with every other Microsoft on-device model.

## If you ship a Windows app, the decision is already on your calendar

The practical reading is that Aion 1.0 Instruct is now something you can prototype against months before you are forced to, which is the whole point of a transparency release. Pull the weights, run your own prompt set, measure the two things the card will not tell you — long-context recall past 2,048 tokens, and behaviour under prompt injection over local documents — and decide whether your summarisation and rewriting paths survive the swap. The December Insider window with its side-by-side registry key is the cheapest test harness you will get, and it closes in January.

The architectural question that follows is where the on-device model stops. A 2.3B model with a 32K window that runs on an NPU is excellent at summarise, rewrite, classify and extract, and it is not going to do multi-step reasoning over a codebase. Most apps that adopt it will end up hybrid: local model for the cheap, private, latency-sensitive passes, and a cloud model behind an API for the escalation path. That escalation layer is the part worth designing deliberately rather than bolting on. OrcaRouter exists for exactly that half of the problem — one API across 200-plus cloud models with no markup on provider list prices, automatic failover when a provider degrades, and a routing DSL for composing several models into one call — and it does not serve Aion 1.0 Instruct, because nothing about this model runs in a datacentre. The honest split is local model, local decision, cloud escalation, and the two halves need different tooling.

There is also a cheap way to hedge the unproven parts. Nothing above is independently benchmarked, and the January build may differ from this checkpoint. Failover is what makes it safe to put an unscored model on a path that matters: keep the fallback configured before you need it, and a bad generation becomes a retry rather than an incident. That advice applies to whichever half of the hybrid you are unsure about.

## What to watch

Four signals would change this picture, and three of them are cheap to monitor. An actual announcement post with training detail would upgrade the card from transparency artefact to launch. A GGUF repository would make the model usable outside Windows and outside ONNX Runtime. A first independent benchmark run — Artificial Analysis, LMArena, or any reproducible harness output — would replace the loose-matching caveats with something quotable. And the Edge Canary and Dev channels are the live test: if the Prompt and Writing Assistance APIs switch from the Phi-4-mini path to Aion 1.0 Instruct in the browser, you will see the swap in behaviour long before the January Windows rollout.

Until then, the correct description of Aion 1.0 Instruct is the one the evidence supports and no more: a finished 2.29B on-device model, MIT-licensed, technically complete and undocumented, published at 17:05 UTC on a Tuesday evening with a quantised sibling nine minutes behind it and a replacement deadline it did not set for itself.
