---
id: collect-261121-veille-ia/mistral/mistral-large-4-explained-mistral-s-1t-parameter-model
title: "Mistral Large 4 Explained: Mistral's 1T-Parameter Model"
domain: mistral
role: reference
task: article
actors: ["Mistral AI"]
dates: []
keywords: ["mistral", "large", "model", "billion", "parameters", "artificial", "says", "analysis"]
source: https://www.layer3labs.io/guides/mistral-large-4-explained
source_anchor: ""
source_lines: [1, 79]
section: "Mistral"
---
# Mistral Large 4 Explained

Artificial Analysis scores Mistral Large 4 at 38 on its Intelligence Index, comparable to GPT-6 Luna at max effort.

Mistral Large 4 is Mistral's largest artificial intelligence (AI) model, a multimodal Mixture-of-Experts (MoE) design with about 1 trillion total parameters. Mistral released it in public preview on October 6, 2026. Mistral says Mistral Large 4 combines instruction following, reasoning and agentic work in one model, with 52 billion active parameters plus a 1.6 billion parameter vision encoder. Mistral provides initial access through a preview Application Programming Interface (API) on Mistral Studio. Mistral has published the open weights on Hugging Face under a modified MIT license, covered in our Mistral Large 4 open weights guide.

Mistral's announcement gives the nickname as unofficially ML4 and, very officially, le Chonk. Mistral Large 4 is the first milestone on the roadmap funded by Mistral's 3 billion euro Series D, announced in September 2026. Mistral trained it from scratch on 3,800 NVIDIA Grace Blackwell graphics processing units (GPUs) in its own datacenters in Europe. Mistral says it reads text and images. It writes text and is fluent in more than 160 languages.

Mistral's docs model page lists the current per-token API rates, which have varied since launch.

## What Mistral Large 4 Is

Mistral Large 4 represents Mistral's largest general-purpose multimodal model to date, designed to unite instruction following, multi-step reasoning, and tool automation inside a granular Mixture-of-Experts (MoE) structure. According to the Mistral Large 4 announcement, the model contains approximately one trillion total parameters, while Mistral technical documentation specifies 1.05 trillion total parameters, 52 billion active parameters per token, and a 1.6 billion parameter vision encoder. Artificial Analysis and VentureBeat reported 49 billion active parameters. Mistral's own blog and docs say 52 billion.

The model accepts text and image inputs and produces text outputs. Mistral's technical documentation lists a context window of 1 million tokens, though Artificial Analysis reported a context limit of 512,000 tokens during the initial preview API period. Artificial Analysis says the Mistral Large 4 API accepts 100 images per request, up from 8 on Mistral Large 3. Simon Willison reports that the Mistral API offers two reasoning levels, none and high. Setting none turns reasoning off for a request.

Mistral says the training data spanned more than 160 languages. That includes every official European Union language. Mistral also says Mistral Large 4 will be the base for a new generation of specialized Mistral models built for the industries its customers care about most.

- Maker: Mistral, based in France.
- Release date: October 6, 2026, as a public preview API on Mistral Studio.
- Parameter scale: Approximately 1 trillion total parameters (documented as 1.05 trillion) with 52 billion active parameters and a 1.6 billion vision encoder.
- Modalities: Multimodal input (text and images, accepting up to 100 images per API call) with text output.
- Status: public preview on Mistral Studio, under the model names mistral-large-4 and mistral-large-4-0.
- Weights: published on Hugging Face as Mistral-Large-4-1T-A52B under a modified MIT license.
- Nickname: Mistral's blog calls it unofficially ML4 and, very officially, le Chonk.

## Mistral Large 4 Benchmarks from Mistral and Artificial Analysis

Mistral Large 4's strongest published results are in cybersecurity. Artificial Analysis gives it 50 on its Cyber Index, and Mistral says that ranks it among the top five models globally. In the CyberGym-E2E-AA evaluation, the model scored 82 percent on vulnerability reproduction and patching. That result outperformed MiMo-V2.6-Pro at 79 percent and GPT-6 Luna at 78 percent. Mistral says Claude Opus 5.5 and GPT-6 Astra score near zero on the same test because they refuse to perform the task.

Mistral's coding numbers put Mistral Large 4 ahead of DeepSeek V4 Pro and Qwen 3.8 Max but behind the leaders on the live DeepSWE leaderboard. Mistral reported a score of 61.7 percent on DeepSWE version 1.1, 59.4 percent on SWE-Atlas-QnA, and 28.3 percent on Terminal-Bench 4. Mistral puts its Coding Agent Index score at 49.8 percent. VentureBeat noted that the live DeepSWE leaderboard lists GLM-5.3 and Kimi K3 at about 69 percent, and GPT-6 Astra, Gemini 3.8 Flash and Claude Opus 5 at about 74 percent. In blind human coding evaluations conducted with Surge AI on a 1-to-5 scale, Mistral Large 4 scored 3.74. It trailed Claude Opus 5 at 4.22 while leading GLM-5.3 at 3.60.

Artificial Analysis scores Mistral Large 4 at 38 on its Intelligence Index, comparable to GPT-6 Luna at max effort (38) and DeepSeek V4.1 Flash at max effort (39). Artificial Analysis calls Mistral Large 4 the most intelligent model from outside the United States and China. On GDP.pdf it measured 19 percent. That was an 18-point improvement over Mistral Large 3. In agentic tool execution, Mistral reported a 59.9 percent score on AutomationBench across 657 multi-application workflows, leading Kimi K3 and DeepSeek V4 Pro.

## European Infrastructure and Sovereign Training Footprint

Mistral trained Mistral Large 4 entirely from scratch on 3,800 NVIDIA Grace Blackwell GPUs housed in its own datacenters in Europe. The Next Web and VentureBeat reported about 4,000 GPUs, but Mistral's announcement says 3,800. VentureBeat and The Next Web report that training took roughly two months. The Next Web reports it used about 10 megawatts. The Next Web also reports more compute coming online through the first half of 2027.

The model relies on the same training, customization, and reinforcement learning pipeline that Mistral offers commercial customers through Mistral Forge. Mistral stated that at its 3,000-GPU operational scale, an active training run generates roughly 33 billion tokens per day. Approximately 16 billion are trainable completion tokens. The reinforcement learning run supporting Mistral Large 4 remains in progress. Mistral expects large and rapid improvements in the weeks and months to come.

Mistral Large 4 is the first milestone on the roadmap funded by Mistral's 3 billion euro Series D. Orrick, which advised Mistral, says the round was announced on September 8, 2026, at a post-money valuation of more than 21 billion euros. Samsung Electronics led it with the Scaleup Europe Fund, managed by EQT, and existing investor PSG Equity. VentureBeat cited a Reuters report that put the valuation at roughly $24 billion. Mistral serves the preview from its own European datacenters. Mistral says a European deployment it runs end to end, independent of other digital service providers and under European law, will be one of its regions.

## Application Programming Interface Access and Weight Release Details

Mistral Large 4 weights are available on Hugging Face as Mistral-Large-4-1T-A52B under the Mistral Ecosystem Open-Weight License, which modifies the MIT license for commercial and non-commercial use. The terms permit copying, modifying, and running the weights free of charge, but operating Mistral Large 4 as a service requires a separate commercial license from Mistral. Read the Mistral Large 4 open weights guide for hardware sizing and license details.

For managed access, Mistral serves Mistral Large 4 on Mistral Studio under the preview identifiers mistral-large-4 and mistral-large-4-0. The API endpoint supports Structured Outputs and Function Calling, so an app can get JavaScript Object Notation (JSON) back and call its own tools. It also supports Document QnA, Prefix, batch jobs, and agent workflows. Published per-token rates have varied since launch, so check the official Mistral documentation and the Mistral Large 4 pricing guide for current numbers.

The Hugging Face release provides the full-precision weights with an embedded speculative-decoding draft head, alongside an NVFP4 (NVIDIA 4-bit floating point) checkpoint designed for Blackwell GPUs. Self-hosters can run the quantized checkpoint using vLLM or SGLang, while GGUF (a quantized file format for llama.cpp) builds support llama.cpp. Mistral provides no minimum memory specification on the model card, though its deployment examples run across eight GPUs using 8-way tensor parallelism.

## Enterprise Workload Fit and Target Implementation Profiles

Mistral aims Mistral Large 4 at cybersecurity teams, citing malware analysis, vulnerability prioritisation and detection rules from its internal testing, and says it will run on private cloud or on-premise for security work. For industrial work, Mistral names engineering drawings, gigapixel satellite imagery, earth observation and manufacturing. The Next Web adds converting technical drawings into computer-aided design (CAD) models. Mistral also targets knowledge work. That includes spreadsheets, slides, PDFs, legal and finance tasks. It also targets buyers who want a European deployment under European law.

Mistral Large 4 does not fit organizations that require a plain Apache 2.0 license. Mistral published the weights on Hugging Face under a modified MIT license that permits internal commercial use but requires a separate commercial license to operate Mistral Large 4 as a service. This overview summarizes the terms, so inspect the official license file directly, or look at Mistral Large 3 if your stack requires Apache 2.0.

That assessment would change if Mistral revises the license. Because per-token API rates have varied, check the Mistral docs model page and the Mistral Large 4 pricing guide for current figures. If you plan to self-host, consult the Mistral Large 4 open weights guide for hardware sizing.

## Lineup Evolution from Mistral Large 3

Mistral Large 4 expands the scale of Mistral's model lineup by moving from 675 billion total parameters in Mistral Large 3 to more than one trillion parameters. Mistral's docs list Mistral Large 3 with 41 billion active and 675 billion total parameters under an Apache 2.0 license. Simon Willison notes that Mistral Large 3 was released last December. He notes it scored 9 on Artificial Analysis.

Buyers who need a plain Apache 2.0 license can compare Mistral Large 4 alternatives. Those include open-weight models from US labs.

To try Mistral Large 4 now, send a sample of your own prompts through the preview API on Mistral Studio. Use both reasoning levels, none and high, before you decide whether to self-host.

## Frequently Asked Questions

- Mistral Large 4 is open weight under a modified MIT license rather than plain MIT, because operating Mistral Large 4 as a service requires a commercial license from Mistral. Internal use is permitted free of charge. This summarizes the license text, so read the official LICENSE file for the complete legal terms.
- Mistral Large 4 has approximately one trillion total parameters (listed as 1.05 trillion in documentation), 52 billion active parameters per token, and a 1.6 billion parameter vision encoder. Artificial Analysis, VentureBeat, The Next Web and Simon Willison reported 49 billion active parameters, while Mistral's blog and docs say 52 billion.
- Yes, Mistral published the weights on Hugging Face as Mistral-Large-4-1T-A52B, following the preview API launch on October 6, 2026. The files include the full-precision repository, an NVFP4 checkpoint for Blackwell GPUs, and GGUF builds for llama.cpp. Mistral's model card does not state an exact release date for the files, but you can review memory and node sizing in our Mistral Large 4 open weights guide.
- Users can access Mistral Large 4 through the public preview API on Mistral Studio using the identifiers mistral-large-4 or mistral-large-4-0. Mistral has not confirmed whether the model is available inside its consumer interface, Le Chat.
- Mistral has not explained why it chose the nickname, stating only in its release announcement: 'Unofficially ML4, very officially: le Chonk.'

### Architect Your Enterprise AI Integration

Evaluate how Mistral Large 4 fits your private cloud, on-premise, or sovereign API requirements. Book a consultation with our implementation engineers.

Book a Consultation
