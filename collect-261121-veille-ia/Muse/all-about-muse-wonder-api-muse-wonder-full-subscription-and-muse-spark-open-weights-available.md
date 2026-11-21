---
id: collect-261121-veille-ia/muse/all-about-muse-wonder-api-muse-wonder-full-subscription-and-muse-spark-open-weights-available
title: "All About Muse Wonder (API), Muse Wonder Full (Subscription), and Muse Spark Open (Weights Available)"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: []
keywords: ["muse", "wonder", "open", "spark", "full", "model", "available", "models"]
source: https://www.deeplearning.ai/the-batch/meta-releases-wonder-opens-spark
source_anchor: ""
source_lines: [1, 27]
section: "Muse"
---
Meta marched all the way back to the AI frontier to offer a Muse model for everyone: a high-end agentic model (comparable to Claude Sonnet 5.5) that costs $50 per month to access, its moderately-priced sibling (comparable to GPT-6.1 Sol), and an open model that outperforms any yet released by a company outside China.

**What’s new:** Meta made __three multimodal models__ available in one week, including the top-of-the-line Muse Wonder Full, the multimodal Muse Wonder, and the locally deployable Muse Spark Open. Muse Wonder is available via API and Muse Spark Open’s weights are free to download, but Muse Wonder Full is only available to Muse Code subscribers.

- **Input/output:** Text, image, video, and audio in (up to 1 million tokens), text out (up to 128,000 tokens), 139 tokens/second (Muse Wonder) and 143 tokens/second (Muse Spark Open)
- **Architecture:** Mixture-of-experts transformer, 816 billion parameters, 42 billion active per token, 256 experts (Muse Spark Open)
- **Features:** Six reasoning levels (minimal, low, medium, high, xhigh, max), reasoning always on, tool calling, structured outputs
- **Performance:** Tied for second on Artificial Analysis’ Intelligence Index (56, Muse Wonder Full); seventh (52, Muse Wonder); seventh among open weight models, first among U.S. open models (39, Muse Spark Open)
- **Availability:** API and subscription (Muse Wonder), subscription only (Muse Wonder Full),__weights__ available via Meta and Hugging Face (Muse Spark Open)
- **Price** : $4/$20 per 1M input/output tokens (Muse Wonder); $50/month (Muse Wonder Full); $1.25/$4.15 input/output (Muse Spark Open)
- **Weights/license:** Weights for Muse Spark Open available under the__Muse Model license__ , which allows for commercial and noncommercial use for up to $20 million of revenue; display of “Built with Muse Spark Open” required for products and services using the model; separate licenses available for other users; Muse Wonder and Wonder Full are proprietary
- **Undisclosed:** Knowledge cutoff, parameters and architecture for Muse Wonder models, specific training datasets and methods

**How it works:** Meta released a __model card__ for Muse Wonder and Wonder Full, but it reveals very little about the model’s architecture or how it was trained.

**Performance:** Muse Wonder Full is the top-performing of the three models, two points behind Claude Opus 5.5 and identical with Claude Sonnet 5.5 on Artificial Analysis’ Intelligence Index. Muse Spark Open is the top open weights model outside China, ahead of Mistral Large 4 and Reflection’s Beam. Its Intelligence Index average is identical with DeepSeek-V4.1-Flash’s, but is still behind MiMo-V.2.6-Pro’s and GLM-5.3’s.

- On AA’s __Intelligence Index__ , a weighted average of ten independent benchmarks, Muse Wonder Full achieved a 56, Muse Wonder a 52, and Muse Spark Open a 39. Meta’s former flagship model, Muse Spark 1.3, achieved an average of 48.
- Muse Wonder costs $4.52 per Intelligence Index task, fifth highest among current models; Wonder Full’s cost per task cannot be measured, since it’s not available via API. Muse Spark Open costs $0.84 per task, more than GPT-6.1 Sol, DeepSeek V4.1 Flash, and MiMo-V2.6-Pro.
- Artificial Analysis notes Muse Wonder and Wonder Full’s low knowledge marks but also partly offset this through their low hallucination rates. On the AA-Omniscience Index, Muse Wonder Full (30) and Wonder (25) place 8th and 10th respectively.
- Muse Spark Open only requires about 1 TB of memory at 4-bit precision. This means it can be run locally on 2 Mac Studios with 512 GB of memory each (or 4 with 256 GB, etc.). Meta and Apple touted their work together to make the model capable of running on Apple’s consumer-grade silicon.

**Behind the news:** Since 2023, Meta has developed both open and closed models with different levels of success. The __first Llama model__ was only 65 billion parameters but inspired developers to deploy, distill, and fine-tune it on their own machines. The 405 billion parameter __Llama 3.1__ was state-of-the-art when it launched in 2024, surpassing GPT-4. After later setbacks, Meta hired Scale.AI’s __Alexandr Wang__ in 2025 to lead AI development, followed by a widely publicized __hiring spree__ that targeted top machine learning engineers, not all of whom stayed. In Spring 2026, Meta released a series of low-cost, proprietary __Muse Spark__ models that __improved and cut prices__ with each version. A month ago, it released __Muse Agent__ for its Facebook, Instagram, and WhatsApp platforms, suggesting that for the time, it would focus on proprietary models powering its own services. Muse Wonder, Wonder Full, and Spark Open upend that logic, returning Meta to the open and closed frontiers.

**Why it matters:** No other leading models, open or closed, can input video or audio. Many of the top open weight models are text-only. Both of the Muse Wonder models and Muse Spark Open can handle text, image, video, and audio, giving them versatility the market has lacked. The fact that you can plausibly deploy Muse Spark Open on a home-built personal computer network is a welcome bonus.

**We’re thinking:** We are very glad to see Meta return to open weights. Reflection’s Beam, Mistral Large 4, and Nvidia’s Nemotron 3.5 Super VL have more permissive licenses, but between its multimodal inputs and overall intelligence metrics, Muse Spark Open is a much more powerful option. Competition and transparency are a powerful combination from which every developer can benefit.
