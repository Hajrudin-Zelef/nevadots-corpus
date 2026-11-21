---
id: collect-261121-veille-ia/modeles-open/ling-3-1-flash-1019-by-vercel-ai-gateway-ai-model-information
title: "Ling 3.1 Flash 1019 by Vercel AI Gateway | AI model information"
domain: modeles-open
role: reference
task: article
actors: []
dates: []
keywords: ["vercel", "gateway", "model", "ling", "flash", "information", "weights", "ling-3"]
source: https://models.sulat.com/models/vercel-inclusionailing-31-flash-1019-17df16c9
source_anchor: ""
source_lines: [1, 3]
section: "Modeles_Open"
---
Vercel AI Gateway

InclusionAI, the AI lab of Ant Group, open-sourced the weights of Ling-3.1-flash-1019 on October 27, 2026, publishing it on Hugging Face and ModelScope under the MIT license, which permits commercial use, modification, and redistribution. A pre-post-training base checkpoint, Ling-3.1-flash-base, was released alongside it for users who want to adapt or train on their own data. The model card describes a mixture-of-experts design with 560B parameters and 25B active per token, activating 8 of 512 experts at each step, and a hybrid linear architecture where most layers use linear attention (KDA) and one in eight uses Gated MLA attention, enabling the claimed one-million-token context. The mostly FP8 weights total around 570 GB, and the reference SGLang setup requires eight H200 GPUs or four GB300s.
