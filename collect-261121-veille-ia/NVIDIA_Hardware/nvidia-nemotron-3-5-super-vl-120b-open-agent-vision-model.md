---
id: collect-261121-veille-ia/nvidia-hardware/nvidia-nemotron-3-5-super-vl-120b-open-agent-vision-model
title: "NVIDIA Nemotron 3.5 Super VL: 120B Open Agent Vision Model"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: []
keywords: ["model", "nvidia", "nemotron", "super", "card", "vision", "says", "open"]
source: https://evermx.com/case/nemotron-3-5-super-vl
source_anchor: ""
source_lines: [1, 114]
section: "NVIDIA_Hardware"
---
# NVIDIA Nemotron 3.5 Super VL: 120B Open Agent Vision Model

NVIDIA released Nemotron 3.5 Super VL on Oct 15, an open-weights 120B vision-language model (12B active) with 1M-token context for enterprise agents.

## Introduction

On October 15, 2026, NVIDIA released Nemotron 3.5 Super VL, an open-weights vision-language model with 120 billion total parameters and 12 billion active parameters, aimed at enterprise agents. NVIDIA's blog post describes it as "the fastest task completion and highest accuracy for specialized always-on agents," and says it reaches accuracy comparable to NVIDIA's largest reasoning model at roughly one-fifth the parameter count. TechCrunch covered the launch on October 19. The release also included two smaller Bolt models for PCs, which are outside this review's scope beyond a brief note below.

This review focuses on what the model is, what the Hugging Face model card actually shows, and which of NVIDIA's performance claims can be checked against published numbers.

## Feature overview

**Architecture.** According to the model card, Super VL combines interleaved Mamba-2, mixture-of-experts (MoE) and attention layers with a C-RADIO vision encoder and a vision-language connector. It lists 512 routed experts per MoE layer and a Multi-Token Prediction head for native speculative decoding. The card gives 120B total and 12B active parameters.

**Context and inputs.** The card says the model supports up to 1M tokens of context, shared across prompt, images, conversation history, tool results and output, though individual deployment recipes may use a smaller window. Inputs are text, images and video (MP4 up to two minutes); output is text. The Hugging Face tags list English, Spanish, French, German, Italian and Japanese.

**What it reads.** NVIDIA says the vision strengths target what agents meet in practice: OCR, tables, charts and structured information in web pages, PDFs and other documents, so agents do not need to route work between separate text and vision systems.

**Training.** NVIDIA's post says the model was trained on about 26 trillion tokens across pretraining and continued pretraining, then post-trained with roughly 3.8 million samples and reinforcement learning across dozens of environments. The card says the language backbone was initialized from Nemotron 3 Super Base and the context was extended to 1M tokens in a 35B-token stage.

**Openness and license.** NVIDIA says the model ships with open weights, open datasets and training recipes, with datasets published on Hugging Face and reinforcement learning environments in NeMo Gym. The license is OpenMDW-1.1, which Hugging Face tags as "other." The license text grants permission to deal in the model materials without restriction, requires keeping a copy of the agreement and notices of origin when distributing, terminates the grant if the user starts patent or copyright litigation over the materials, and places no restrictions on outputs.

**Availability.** Hugging Face, OpenRouter, the ModelScope community and NVIDIA NIM. The BF16 card lists 4x H100 or 2x GB200 for deployment with vLLM or SGLang, and points to a separate NVFP4 release for more efficient inference.

## Benchmarks: reading the model card

NVIDIA's model card publishes a table of results collected through NeMo Gym, which NVIDIA says it documents for reproducibility. The model column is labeled "Nemotron 3.5 SuperGA BF16." The compared models are Qwen3.5-122B-A10B, Qwen3.8-Flash-Next, DeepSeek-V4-Flash0731, Nemotron 3 Ultra, Inkling-Small and GLM-5.3-Flash. A selection of rows (all NVIDIA-reported):

| Benchmark | Super VL | Qwen3.5-122B-A10B | Qwen3.8-Flash-Next | DeepSeek-V4-Flash0731 | Nemotron 3 Ultra | Inkling-Small | GLM-5.3-Flash | 
|---|---|---|---|---|---|---|---|
| GPQA-Diamond | 89.52 | 86.7 | 92.7 | 89.6 | 87.0 | 88.6 | 91.2 | 
| GraphWalks | 89.71 | 86.1 | 76.1 | 65.4 | 63.3 | 55.7 | 88.4 | 
| BrowseComp | 78 | 58.1 | 89.0 | 84.2 | 67.85 | 74.1 | 86.6 | 
| IFBench | 70.86 | 67.6 | 75.6 | 76.2 | 82.9 | 79.3 | 57.7 | 
| SWE-bench Verified | 71.10 | 71.9 | 78.7 | 75.4 | 73.9 | 75.27 | 79.10 | 
| Terminal-Bench 2.1 | 60.67 | 48.70 | 85.80 | 76.80 | 54.80 | 53.40 | 82.70 | 
| OCRBench v2, EN | 68.12 | 68.3 | 67.9 | N/A | N/A | 54.2 | 61.8 | 
| CharXiv reasoning | 80.82 | 83.5 | 85.8 | N/A | N/A | 77.8 | 81.9 | 

Three readings follow directly from the numbers. First, the results are mixed rather than uniformly leading: Super VL has the highest GraphWalks score in its row, but trails three of the listed models on Terminal-Bench 2.1 and three of the six on BrowseComp. Second, "comparable to our largest reasoning model" holds on some rows and not others: against Nemotron 3 Ultra, Super VL is higher on GPQA-Diamond (89.52 vs 87.0), BrowseComp (78 vs 67.85) and GraphWalks, but lower on IFBench (70.86 vs 82.9), which measures instruction following, and lower on the tau2-bench retail, airline and telecom rows (for example 69.24 vs 73.7 on retail). Third, on vision rows it is level with the Qwen models on OCRBench v2 English (68.12 vs 68.3 and 67.9) but behind them on CharXiv reasoning.

## The speed and competitor claims

NVIDIA's post says that on the Artificial Analysis Intelligence Index leaderboard the model achieves "up to 5x faster throughput compared to other models in its class," and calls it the fastest open agentic model in its class. TechCrunch adds that NVIDIA says Super VL completes tasks faster than models from Anthropic and OpenAI that it names as Opus 5, GPT 6.1 Sol, Luna and Astra, but only when post-trained on specific agentic tasks: document intelligence, financial accounting, scientific experimentation, medical insurance claims and security attack resilience. The text of NVIDIA's blog post does not name those proprietary models, and the model card's benchmark table lists no Anthropic or OpenAI models, so this review cannot confirm the comparison independently. NVIDIA also says serving on NVIDIA Dynamo lets organizations serve up to 8x more users with 85% lower latency, without stating the baseline in the post text.

## Who is using it

NVIDIA's post describes customization more than off-the-shelf use. Scale AI post-trained the model for Skydance's vendor credit finance agent and for EY's M&A due diligence agent. Ramp is customizing it for financial document intelligence, CrowdStrike for agentic blue teaming in enterprise security, and Salesforce for government-workflow agents. Perplexity is named for search and agentic tasks in Perplexity Search and Computer. TechCrunch summarizes these as customers that use the model, which is looser than NVIDIA's own wording. NVIDIA's Kari Briski, quoted by TechCrunch, said organizations can post-train Super VL on their own data and deploy it on their own infrastructure.

## Usability analysis

For a team that already runs vLLM or SGLang, the model card provides complete serving commands, a configurable reasoning mode (`enable_thinking`, with a medium-effort option) and an OpenAI-compatible client example. The hardware bar is real: BF16 serving is documented on four H100s or two GB200s, and the card points to the NVFP4 release for efficient inference on DGX Spark or data center GPUs. The example vLLM configuration limits each request to one image and one video, an example serving setting rather than a stated model limit.

## Context: the Bolt models

NVIDIA also released Nemotron 3.5 Bolt 74B-A9B 24GB, using a native 2-bit quantization to fit in 24GB for local agents on RTX and DGX Spark systems, and Bolt E4B 4GB, which NVIDIA says is due October 23.

## Outlook and conclusion

The release fits a pattern TechCrunch describes: enterprises wanting to run open models in their own data centers. Its practical value depends on post-training. A buyer with domain data and the GPUs to host a 120B model gets a documented, permissively licensed base with a long context window and native vision. A buyer expecting a drop-in replacement for proprietary frontier models should read the benchmark table first, since NVIDIA's speed claims are self-reported and the card shows several rows where listed models score higher. Independent leaderboard results will clarify the speed and accuracy story.

## Editor's Verdict

NVIDIA Nemotron 3.5 Super VL: 120B Open Agent Vision Model earns a solid recommendation within the Other LLM space.

The strongest case for paying attention: the hybrid Mamba-2, attention and mixture-of-experts design keeps active parameters at 12B out of 120B, which NVIDIA pairs with native speculative decoding. That alone raises the bar for what readers should expect in this space. Reinforcing that, a context window of up to 1M tokens, plus text, image and video input, lets one model handle long documents that mix text and charts — practical value rather than just headline appeal. The broader signal worth registering is straightforward: the pitch centers on post-training on private data and self-hosting, and most customer examples in NVIDIA's post are customizations rather than off-the-shelf use. On the other side of the ledger, one constraint is real rather than a marketing footnote: the speed and competitor claims are NVIDIA's own and rest on a leaderboard comparison with unnamed models in its class. It should factor into any serious decision. Layered on top of that, a 120B-parameter model needs data-center class hardware, with the BF16 card listing four H100s or two GB200s — which narrows the set of teams for whom this is an obvious yes.

For enterprise AI teams with domain data to post-train on and the GPUs to self-host a 120B model, this is a serious evaluation candidate, not just a curiosity to bookmark. For everyone else, the safer posture is to monitor coverage and revisit once the use cases that matter to your team are demonstrated in the wild.

### Pros

- The hybrid Mamba-2, attention and mixture-of-experts design keeps active parameters at 12B out of 120B, which NVIDIA pairs with native speculative decoding
- A context window of up to 1M tokens, plus text, image and video input, lets one model handle long documents that mix text and charts
- Open weights, datasets and recipes under OpenMDW-1.1 allow post-training on private data and deployment on an organization's own infrastructure
- Availability through Hugging Face, OpenRouter, ModelScope and NIM, with vLLM and SGLang serving recipes, shortens the path to a first deployment

### Cons

- The speed and competitor claims are NVIDIA's own and rest on a leaderboard comparison with unnamed models in its class
- A 120B-parameter model needs data-center class hardware, with the BF16 card listing four H100s or two GB200s
- The model card's table shows several rows where listed models score higher, including Terminal-Bench 2.1 at 60.67 against 85.80 for one model

### References

### Comments0

### Key Insights

- The pitch centers on post-training on private data and self-hosting, and most customer examples in NVIDIA's post are customizations rather than off-the-shelf use
- NVIDIA's own model card shows mixed results against the listed open models, with the highest GraphWalks score in its row but lower scores on Terminal-Bench 2.1 and IFBench
- The 'comparable to our largest reasoning model' framing holds on some rows such as GPQA-Diamond and BrowseComp, but Nemotron 3 Ultra scores higher on IFBench and the tau2-bench rows
- The comparison to Opus 5, GPT 6.1 Sol, Luna and Astra comes from TechCrunch's account of NVIDIA's claim and applies only to post-trained agentic tasks; NVIDIA's blog text does not name those models
- The 5x throughput figure is NVIDIA's reading of the Artificial Analysis leaderboard against models in its class, not an independent test
- OpenMDW-1.1 places no restrictions on outputs and requires keeping the agreement and notices when distributing the materials
- A hybrid Mamba-2 and MoE design with Multi-Token Prediction targets long-context, high-volume agent workloads where throughput matters
- Customer wording matters, since NVIDIA says Skydance, EY and others are post-training or customizing the model while TechCrunch describes them as using it

## Related AI Reviews

### Grok Bot Background Check-Ins: 31 Unattended Calls Logged

NewCore says two Grok Bots made 31 unattended reads of work apps; an admin toggle arrived Nov 12, on by default. NewCore sells the gateway.

### Grok Build Apps Exposed: 35,578 Open Links Found by Claroc

Claroc tried 43,551 grok.me app addresses; 35,578 opened with no sign-in, including 24 built by SpaceXSI and X staff.

### Underdog Woof: A 31.7MB Speech Model That Runs in a Browser

Underdog's Woof text-to-speech model is a 31.7 MB browser download with 30.7M parameters; weights are licensed for non-commercial use.

### Meta Muse Spaces: Build Your Own Tools by Describing Them

Meta added spaces to Muse on Nov 6: custom trackers, planners and games built from a description. Sharing spaces with others is still being explored.
