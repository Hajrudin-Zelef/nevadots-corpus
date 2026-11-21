---
id: collect-261121-veille-ia/huawei/huawei-says-ascend-supports-kimi-k3-1-pro-from-its-weights-release
title: "Huawei says Ascend supports Kimi K3.1 Pro from its weights release"
domain: huawei
role: reference
task: article
actors: ["Moonshot AI"]
dates: []
keywords: ["ascend", "kimi", "weights", "huawei", "says", "inference", "model", "supports"]
source: https://www.aiaffairs.com/technology/huawei-ascend-kimi-k3-1-pro-launch-day-inference-support/
source_anchor: ""
source_lines: [1, 5]
section: "Kimi"
---
On 5 November, Pandaily reported that Huawei’s Ascend computing team said it had adapted Moonshot AI’s Kimi K3.1 Pro for inference on its hardware. Moonshot AI released the model’s full weights on 4 November, and Huawei says developers could deploy it from that date using vLLM Ascend or SGLang on Atlas 800 A3 servers, Atlas 900 A3 SuperPoD and Ascend 950 SuperPoD systems.

Kimi K3.1 Pro has 2.8 trillion parameters, though its mixture-of-experts design activates roughly 104 billion for each inference step. It has a context window of about one million tokens. Huawei said its adaptation work included a new sparse-attention path and multimodal inference, building on support it had already developed for the earlier Kimi K3 model.

Reading a long document could make use of that context window if the model were deployed on the supported hardware. Keeping the model available for that task would also require accommodating its weights: Pandaily reported that they occupy about 5.6TB in BF16 format and still need around 1.4TB with INT4 quantisation, beyond a single consumer GPU.
