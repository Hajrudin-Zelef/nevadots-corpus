---
id: collect-261121-veille-ia/modeles-open/reflection-releases-beam-an-open-weight-model-from-an-independent-u-s-based-lab
title: "Reflection Releases Beam, an Open Weight Model From an Independent U.S.-Based Lab"
domain: modeles-open
role: reference
task: article
actors: []
dates: []
keywords: ["model", "reflection", "beam", "open", "tokens", "weights", "second", "models"]
source: https://www.deeplearning.ai/the-batch/nvidia-courts-a-new-partner
source_anchor: ""
source_lines: [1, 32]
section: "Modeles_Open"
---
Reflection AI, a closely-watched, Nvidia-backed U.S. startup, announced its first model. Beam ranks second among U.S.-made open weights models (behind Meta’s Muse Spark Open) but generates fewer tokens per task than open weights peers.

**What’s new:** Reflection introduced __Beam__, an 501 billion parameter mixture-of-experts model designed for coding and agentic tasks. Developers can join a waitlist for early access, and Reflection says it will release the weights, a technical report, and a model card later in October.

- **Input/output:** Text in (up to 1 million tokens), text out (up to 128,000 tokens), 228 tokens/second
- **Architecture:** Mixture-of-experts transformer, 501 billion parameters, 23 billion active per token, 256 experts
- **Features:** Five reasoning levels (low, medium, high, xhigh, max), reasoning always on and defaults to max, tool calling, structured outputs
- **Knowledge cutoff:** June 30, 2026
- **Performance:** Second among U.S.-made model on Artificial Analysis’ Intelligence Index (30)
- **Availability:**__Weights__ available via Hugging Face, waitlist access via Reflection’s beta API
- **Price** : $0.70/$2.10 per 1M input/output tokens
- **Weights/license:** Weights available under the Apache 2.0 license, which allows commercial use
- **Undisclosed:** Specific training datasets

**How it works:** Beam’s architecture interleaves attention layers that process nearby tokens with layers that process the entire input. Reflection pretrained a base model, then trained it further to handle long inputs. It fine-tuned two copies, one using reinforcement learning (RL) on coding, agentic, and STEM tasks and another to follow safety and behavior guidelines. It merged these two copies into Beam.

- Reflection pretrained the model on 23.8 trillion tokens from the web, other public sources, and licensed datasets. It used 6,144 GPUs over less than four weeks. Its filters discarded around 95 percent of raw web text.
- A midtraining stage on code repositories, long documents, and long-running requests extended Beam’s effectiveness on long-context tasks up to its input limit of 1 million tokens, Reflection says.
- For post-training RL, Reflection assembled around one million training environments, mostly using synthetic data, and filtered out tasks the model consistently solved or failed to solve. Running on 10,500 GPUs for four weeks, the model made more than 100 million attempts at these tasks. Rewards favored successful solutions and penalized unnecessary tokens. The model kept training while its attempts were still running, so some attempts came from versions of the model that were more than a day old. Reflection says learning remained stable regardless.
- Reflection fine-tuned a second copy of the base model via supervised learning and RL on data that reflects Reflection’s behavior guidelines for the model. It merged the two fine-tuned models into Beam using multi-teacher on-policy distillation, which trains a single model to imitate several others on outputs it generates itself.

**Performance:** In Artificial Analysis’ independent benchmark tests, Beam is behind Muse Spark Open and several China-built models with open weights. It’s unusually fast and token-efficient for its level of performance. 

- On the Intelligence Index, a weighted average of 10 evaluations across math, science, coding, and reasoning, Beam (30) ranks second among U.S.-made open weight models, behind Meta’s Muse Spark Open (39) and trails Xiaomi's MiMo-V2.6-Pro (46) and Z.ai’s GLM-5.3 (45). Surprisingly, Qwen 3.8-27B (34) also outperforms it. Beam used around 26,000 output tokens per task, the fewest among open weights models in Artificial Analysis’ comparison.
- Beam is very fast, with a throughput of 228 tokens per second (just behind Claude Haiku 5.5) and a latency of 10.1 seconds before it generates its first token (behind only Nvidia Nemotron 3.5 Super VL). Its time per Intelligence Index task (1.4 minutes) is the fastest Artificial Analysis has measured.
- Reflection estimates that on reasoning benchmarks, Beam uses about one-quarter to one-third as much compute as GLM-5.2. To estimate this, Reflection multiplies each model’s active parameter count by the number of tokens generated. It leaves out the computation needed to read the prompt and the cost of serving the model, so it approximates computation rather than measuring cost per task.

**Behind the news:** Reflection was founded in 2024 by former DeepMind researchers Misha Laskin and Ioannis Antonoglou. It has partnered with the Pentagon and the Department of Energy, agreed to develop models for U.S. allies such as South Korea, and was valued at $25 billion in a March funding round. The Trump administration hopes Reflection’s models will rival inexpensive models from China, according to the *__Financial Times__*. On October 10, The *Financial Times* and *Reuters* __reported__ that Nvidia, which has invested $800 million in Reflection, is in early talks to either invest more, acqui-hire Reflection’s staff, license its technology, or buy the company outright.

**Why it matters:** Most open weight models that outperform Beam are either a trillion parameters or approach that figure. Beam is half that. It only has 23 billion active parameters per token, versus Kimi K3’s 104 billion. At four-bit precision, Beam requires only 300GB of memory, meaning it can run on four Nvidia H100 chips, or 2 B200s. At full precision (1.2 TB of memory), it could run on 8 B200s. These relatively modest requirements make mid-tier intelligence much more accessible. However, Meta’s Muse Spark Open and DeepSeek-V4.1-Flash outperform it without requiring much more compute, and Qwen 3.8-27B requires far less. Both generate more tokens than Beam, but the performance gains might be worth the tradeoff.

**We’re thinking:** Nvidia has good reason to be interested in Beam — as compute shifts from training to inference, Nvidia is well positioned to serve inference tokens, and open weight models will help drive this demand. Further, while Nvidia builds open weights models of its own, including its Nemotron family, none of them approach Beam’s performance, let alone that of larger models like Muse Spark Open. An acquisition of or deeper partnership with Reflection could help Nvidia secure business from organizations and governments looking for open alternatives, but would like an alternative to China's models.
