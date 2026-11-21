---
id: collect-261121-veille-ia/xiaomi-mimo/xiaomimimo-mimo-v2-6-flash-mopd-hugging-face
title: "XiaomiMiMo/MiMo-V2.6-Flash-MOPD · Hugging Face"
domain: xiaomi-mimo
role: reference
task: article
actors: ["Xiaomi"]
dates: []
keywords: ["mimo-v2", "teachers", "model", "layers", "encoder", "flash-mopd", "size", "mopd"]
source: https://huggingface.co/XiaomiMiMo/MiMo-V2.6-Flash-MOPD
source_anchor: ""
source_lines: [1, 135]
section: "Xiaomi_MiMo"
---
**This is the MOPD upgrade of the MiMo-V2.6-Flash-RL checkpoint.**


**MOPD2** (👉 **Technical Report §5.6**)

Fuses several domain-specialized teachers into one model, extending to domains where reliable training-time verification is hard, such as long-horizon game development, scientific research and embodied intelligence.
**Diagnosing and Mitigating Tool-Call Repetition in MiMo-V2.6** (👉 **Technical Blog**)

An easy-to-overlook failure mode in which the model keeps issuing the same or highly similar tool calls, appearing busy while making no progress. Nothing fails outright, so it tends to go unnoticed. The MOPD stage handles it efficiently, with a short specialized-teacher run that converges quickly.

MOPD2 distills several domain-specialized teachers into the student on-policy. The teachers fall into two families: **mixRL teachers**, trained on verifiable tasks, and **SFT teachers**, trained on synthetic demonstrations for open-domain tasks where a reliable reward is hard to design. Three streams contribute to a single update:

- **Standard MOPD** : mixRL teachers supervise full autonomous rollouts.
- **Teacher-Prefix OPD** : prefixes come from teacher rollouts. A trajectory with*k* assistant turns yields*k* history prefixes, one per turn. The model generates a single new turn from each, and the teacher scores it against the same history.
- **SFT-Prefix OPD** : prefixes come from SFT demonstrations. The demonstration supplies the history, and the model writes its own continuation.

Method details are in Technical Report §5.6.

Following the release of MiMo-V2.6, tool-call repetition emerged as one of the most noticeable issues in agentic settings: the model would sometimes issue the same or highly similar tool calls repeatedly, consuming time and context without making progress. This checkpoint mitigates it.

*Figure: response-level repetition rate on MiMo-V2.6-Flash, RL-stage versus this checkpoint, across context lengths and agent harnesses.*

The technical blog has the full diagnosis. The fix is lightweight to train: a short specialized-teacher run that folds into the normal MOPD pass.

- **Architecture** : Sparse MoE (Mixture of Experts), 309B total / 15B activated parameters
- **Context Length** : 1M tokens
- **Modalities** : Text, Image, Video, Audio
- **Vision Encoder** : 681M-param MiMo ViT (28 layers: 24 SWA + 4 Full)
- **Audio Encoder** : 308M AudioTokenizer + 127M audio patch encoder
- **Multi-Token Prediction (MTP)** : 5-layer speculative decoder

*Figure 1. MiMo-V2.6 architecture.*

| Model | Download | 
|---|---|
| **MiMo-V2.6-Pro-RL** | 🤗 HuggingFace · 🤖 ModelScope | 
| **MiMo-V2.6-Flash-RL** | 🤗 HuggingFace · 🤖 ModelScope | 
| **MiMo-V2.6-Pro-MOPD** | 🤗 HuggingFace · 🤖 ModelScope | 
| **MiMo-V2.6-Flash-MOPD** | 🤗 HuggingFace · 🤖 ModelScope | 

| Component | MiMo-V2.6-Flash-MOPD | 
|---|---|
| Layers (Total / SWA / GA) | 48 / 39 / 9 | 
| Hidden Size | 4096 | 
| SWA Heads (Q/KV) | 64 / 8 | 
| GA Heads (Q/KV) | 64 / 4 | 
| Head Dimensions (QK / V) | 192 / 128 | 
| Sliding Window Size | 128 | 
| Routed Experts (Total / Activated) | 256 / 8 | 
| Max Context Length | 1M | 
| MTP / Speculative Decoder | 5 SWA layers, window 1024 | 

The first Transformer block uses global attention with a dense FFN. Remaining blocks interleave local SWA and GA; both use sparse MoE FFNs without shared experts.

| Configuration | Value | 
|---|---|
| Layers (Total / SWA / GA) | 28 / 24 / 4 | 
| Hidden Size | 1280 | 
| Attention Heads (Q / KV) | 32 / 8 | 
| Head Dimension | 64 | 
| Patch Size (T × H × W) | 2 × 16 × 16 | 
| Sliding Window (Left / Right) | 64 / 64 | 
| Spatial Merge Size | 2 × 2 | 
| Parameters | 681M | 

AudioTokenizer encoder: 24 layers (12 SWA / 12 GA), hidden 1024, 20 RVQ codebooks, 308M parameters. Audio patch encoder: 6 layers, 127M parameters; four frames per patch (25 Hz → 6.25 Hz).

5-layer SWA MTP drafter (DFlash-style). Predicts 7 subsequent tokens per forward pass for parallel verification.

For best performance, follow the SGLang MiMo cookbook. Docker image: `lmsysorg/sglang:latest`.

```
sglang serve \
  --trust-remote-code \
  --model-path XiaomiMiMo/MiMo-V2.6-Flash-MOPD \
  --tp 8 \
  --dp 2 \
  --enable-dp-attention \
  --enable-dp-lm-head \
  --mm-enable-dp-encoder \
  --mem-fraction-static 0.65 \
  --chunked-prefill-size 16384 \
  --speculative-algorithm EAGLE \
  --speculative-num-steps 3 \
  --speculative-eagle-topk 1 \
  --speculative-num-draft-tokens 4 \
  --enable-multi-layer-eagle \
  --reasoning-parser mimo \
  --tool-call-parser mimo \
  --host 0.0.0.0 \
  --port 30000
```
Follow the vLLM MiMo-V2.5 recipe. Stable vLLM may lag; pre-built image: `docker pull vllm/vllm-openai:mimov25-cu129`.

```
vllm serve XiaomiMiMo/MiMo-V2.6-Flash-MOPD \
  --tensor-parallel-size 4 \
  --trust-remote-code \
  --gpu-memory-utilization 0.95 \
  --max-model-len auto \
  --reasoning-parser mimo \
  --tool-call-parser mimo \
  --enable-auto-tool-choice \
  --generation-config vllm
```
Recommended sampling: `temperature=1.0`, `top_p=0.95`.

Also available in AI Studio, MiMo Code, Xiaomi MiMo Desktop, Xiaomi MiMo Open Platform API, and OpenRouter.

```
@misc{mimo2026v26flashmopd,
  title={MiMo-V2.6-Flash-MOPD},
  author={{Xiaomi MiMo Team}},
  year={2026},
  howpublished={\url{https://huggingface.co/XiaomiMiMo/MiMo-V2.6-Flash-MOPD}},
}
```
For questions or feedback, reach us at mimo@xiaomi.com or join our community:

You are solely responsible for your use of XiaomiMiMo/MiMo-V2.6-Flash-MOPD and any outputs, actions, or consequences arising therefrom, and you agree not to use the XiaomiMiMo/MiMo-V2.6-Flash-MOPD or any derivatives thereof:

- For any unlawful, fraudulent, or malicious purpose, or in any manner that violates any applicable laws or regulations；
- To infringe upon the intellectual property rights, privacy rights, publicity rights, or other lawful rights or interests of any third party;
- To exploit, harm, harass, defame, unlawfully discriminate against, or otherwise adversely affect any individual or group, including minors or vulnerable persons;
- For any military purpose or application;
- To engage in or facilitate malicious cyber activities, including but not limited to malware, ransomware, phishing, credential theft, or unauthorized access, or any activity intended to compromise or unlawfully control computer systems, networks, or encryption systems;
- To access, collect, process, disclose, or disseminate personal data or other protected information without appropriate authorization or lawful basis; or to access, interact with, scrape, or automate any third-party platform or service in violation of any applicable laws, policies, or authorization requirements;
- To use the XiaomiMiMo/MiMo-V2.6-Flash-MOPD in a manner that autonomously executes high-risk actions without appropriate human oversight or authorization.

This notice is an informational reminder only and does not amend the applicable model licence or impose additional licence conditions. The permissions granted under that licence remain unaffected. Nothing in this notice restricts uses permitted by applicable law, including applicable exceptions and limitations.

mimo-legal@xiaomi.com. For information on submitting a complaint, please visit the https://mimo.xiaomi.com.

- Downloads last month
- 25,619
