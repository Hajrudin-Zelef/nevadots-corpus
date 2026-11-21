---
id: collect-261121-veille-ia/modeles-open/hyperai-2
title: "HyperAI"
domain: modeles-open
role: reference
task: article
actors: []
dates: ["2026-10-29"]
keywords: ["mistral", "model", "large", "open-weight", "hugging", "face", "multimodal", "deployment"]
source: https://hyper.ai/en/stories/d2988a995aa7e8c70188b263f54e3116
source_anchor: ""
source_lines: [1, 3]
section: "Modeles_Open"
---
# Mistral Releases Large 4 Open-Weight Model on Hugging Face

Mistral has released Mistral Large 4, internally called Le Chonk, on Hugging Face, making a large open-weight multimodal, multilingual Mixture-of-Experts model available for text and image inputs with text generation. The system is reported to have about 1 trillion total parameters and 49 billion active parameters, with a 1 million-token context window. It is published in FP8 and NVFP4 versions, with GGUF variants from Unsloth and the llama.cpp team, under the Mistral Ecosystem Open-Weight License, an MIT-derived license that restricts model-as-a-service offerings. The architecture pairs a sparse language model with a 1.6 billion-parameter Pixtral vision encoder. The decoder uses Multi-head Latent Attention and DeepSeek Sparse Attention to compress key-value state and select relevant earlier tokens, while a router activates a small set of experts for each token from a pool of 128, including one shared expert. This design targets efficient long-context reasoning, multimodal understanding and agentic tasks, with deployment supported by Transformers, vLLM and llama.cpp. As of October 29, 2026, Mistral Large 4 ranks fifth among open-weight models on the Artificial Analysis Intelligence index. It posted a combined Coding Agent Index score of 49.8%, ahead of DeepSeek V4 Pro 0813 and Qwen3.8 Max, with 61.7% on DeepSWE v1.1, 59.4% on SWE-Atlas-QnA and 28.3% on Terminal-Bench 4. The model also recorded 59.9% on AutomationBench and a 1,393 Elo rating on AA-Briefcase, indicating strength in business workflows and long-horizon professional tasks. Cybersecurity is a key focus. The model achieved 82% on the Artificial Analysis Cyber Index, which requires reproducing a real open-source software vulnerability and then patching it. Mistral said several closed frontier models refused or scored near zero on the same test, while Mistral Large 4 also proved useful in malware analysis, vulnerability prioritization and detection-rule generation. The company positions the model for defensive security because incident response inputs can resemble attacks, and open weights allow teams to control deployment and evaluate refusal behavior directly. It also scored 93.3% attack resistance on Lakera’s B3 benchmark. The release includes day-zero inference recipes, fine-tuning support through TRL, Hugging Face Inference Endpoints deployment and Dell Enterprise Hub support for Dell AI servers. The result gives developers, enterprises and security teams a broadly accessible model for coding, multimodal reasoning and defensive cyber workflows, while expanding the competitive open-weight ecosystem around large, expert-based and sparsely attended architectures.
