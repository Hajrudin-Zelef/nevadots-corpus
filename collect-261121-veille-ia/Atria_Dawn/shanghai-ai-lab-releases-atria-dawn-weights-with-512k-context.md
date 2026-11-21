---
id: collect-261121-veille-ia/atria-dawn/shanghai-ai-lab-releases-atria-dawn-weights-with-512k-context
title: "Shanghai AI Lab releases Atria Dawn weights with 512K context"
domain: atria-dawn
role: reference
task: article
actors: []
dates: []
keywords: ["atria", "dawn", "model", "shanghai", "context", "weights", "release", "lab"]
source: https://latenode.com/news/shanghai-ai-lab-releases-atria-dawn-weights-with-512k-context
source_anchor: ""
source_lines: [1, 5]
section: "Atria_Dawn"
---
Shanghai AI Lab has published Atria Dawn as an open-weight release, with hosted access and local deployment both supported. The Atria Dawn access page shows OpenAI-compatible and Anthropic-compatible API options for the model.

On the Hugging Face model card, the full release is listed with a 512K context window, while preview variants are capped at 256K. The same card says Atria Dawn is built on a 744B-parameter MoE GLM-5.2 foundation model, and that the code and model weights are available under the MIT License.

For builders, that means a long-context, permissively licensed agent model that can sit behind familiar API patterns instead of a single vendor stack. The practical limit is deployment cost: a model at this size is not a lightweight self-hosting option, and the release is text-only, with image inputs explicitly rejected.
