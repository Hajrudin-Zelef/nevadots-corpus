---
id: collect-261121-veille-ia/modeles-open/reflection-ai-beam-explained-501b-model-architecture
title: "Reflection AI Beam Explained: 501B Model Architecture"
domain: modeles-open
role: reference
task: article
actors: []
dates: ["2026-10-16"]
keywords: ["reflection", "beam", "model", "october", "tokens", "pretraining", "open-weight", "weights"]
source: https://www.layer3labs.io/guides/reflection-beam-explained
source_anchor: ""
source_lines: [1, 105]
section: "Modeles_Open"
---
# Reflection AI Beam Explained: The 501B Open-Weight Model

What Reflection AI's first open-weight model scored on its own benchmarks and where to get the Apache 2.0 weights.

Reflection AI Beam is a 501-billion-parameter open-weight language model built by Brooklyn-based startup Reflection AI to handle software engineering, multi-step reasoning, and autonomous agent tasks. This sparse Mixture-of-Experts (MoE) model uses 23 billion of those parameters for each token. Beam is Reflection's first open-weight model. TechCrunch reports it is pitched to rival Chinese open models at lower compute cost.

Reflection announced Beam on October 5, 2026. It is text-only and aimed at enterprises, the public sector and developers. Training the model required 23.8 trillion tokens during pretraining and more than 100 million rollouts during reinforcement learning across specialized clusters of NVIDIA Graphics Processing Units (GPUs).

Reflection AI released the model weights on Hugging Face under an open-source Apache 2.0 license on October 16, 2026. The developer application programming interface (API) is in public beta.

## What Reflection Beam Is

Reflection AI Beam is a sparse Mixture-of-Experts (MoE) foundation model comprising 501 billion total parameters, with 23 billion parameters active for any single token during inference. Developed by Brooklyn-based artificial intelligence laboratory Reflection AI, the model focuses specifically on coding, logical reasoning, and multi-step agentic workflows. As reported by TechCrunch, Reflection AI calls Beam a "workhorse model" for enterprises, the public sector and developers, at "a fraction of the token cost and inference time compute" of rivals.

The model is text-only. It does not process image, audio, or video inputs. Reflection AI announced Beam on Monday, October 5, 2026. According to the Reflection AI introduction blog post, Beam is the first model in a series. Reflection says it is "already training what comes next." Beam includes a configurable reasoning effort parameter that allows operators to balance response length against reasoning depth. Lower settings favor shorter responses, while higher settings allow longer reasoning.

Reflection AI is based in Brooklyn. That puts Beam among the American open-weight AI models. Reflection AI distributes Beam under an Apache 2.0 license, which permits commercial modification and self-hosted deployments. Reflection published the weights on Hugging Face on October 16, 2026.

- Maker: Reflection AI, based in Brooklyn, New York.
- Announcement date: Monday, October 5, 2026.
- Parameter scale: 501 billion total parameters with 23 billion active parameters per token.
- Pretraining volume: 23.8 trillion tokens from curated web collections and licensed datasets.
- License terms: Apache 2.0, published with the weights on October 16, 2026.
- Weights status: Published on Hugging Face on October 16, 2026.
- Context window: Up to 1 million tokens of effective context after midtraining.

## Pretraining Architecture and Reinforcement Learning Pipeline

Reflection AI trained Beam in three steps: pretraining, midtraining to extend context, and a large reinforcement learning (RL) run. Pretraining consumed 23.8 trillion tokens of web text and proprietary licensed data, completed end-to-end in under four weeks on 6,144 NVIDIA GB300 NVL72 GPUs. The engineering team reported a cluster goodput rate of 92.3 percent. Reflection reported nine semi-automatic rewinds during the run. Custom data filtering protocols retained approximately 1.8 trillion high-quality tokens that standard automated filters typically discard.

Reflection AI applied midtraining procedures that extended Beam's effective context window to 1 million tokens. Following pretraining, the team executed a large-scale Reinforcement Learning (RL) run lasting four weeks across 10,500 NVIDIA GB300 GPUs. This phase completed over 100 million rollouts across nearly one million distinct environments. Execution and evaluation used roughly 1.3 billion containerized sandboxes. Maximum context length during reinforcement learning reached 256,000 tokens.

The RL infrastructure operated across two cloud providers and four geographical regions. It maintained an average of 110,000 concurrent rollouts and peak loads of 170,000 concurrent sandboxes. Reflection AI dedicated 80 million rollouts specifically to training the model's reasoning expert. Active inference-to-training GPU allocations ranged between 3.9 to 1 and 5.4 to 1, with updated model weights transferring to the serving cluster in a median time of 12 seconds. Reflection handled 71 inference incidents without stopping training. Capacity came back in a median of 8 minutes, and 0.02 percent of serving GPU-minutes were lost.

## Reported Benchmark Scores and Comparative Performance

Reflection AI says Beam is competitive with GLM 5.2 and approaching Qwen 3.8 Max on coding and agentic tasks. On the SWE-bench Verified coding benchmark, Beam scored 80.9 percent, surpassing Thinking Machines Lab's Inkling at 77.6 percent and Nemotron 3 Ultra at 70.7 percent. On SWE-bench Multilingual, Beam recorded 78.0 percent compared to Nemotron 3 Ultra's 67.7 percent. On SWE-Bench Pro version 1, Beam reached 65.5 percent. That outperformed GLM 5.2 at 62.1 percent and Inkling at 54.3 percent, while trailing Qwen 3.8 Max at 67.7 percent.

On harder coding and agent tests in Reflection's table, Kimi K3, GLM 5.3 and DeepSeek V4.1 Flash score higher than Beam. On SWE-Bench Pro version 2 Hard, Beam scored 77.2 percent. That finished ahead of Inkling at 56.9 percent. It trailed GLM 5.3 at 84.3 percent and Kimi K3 at 88.2 percent. On Terminal Bench version 2.1, Beam scored 80.1 percent. DeepSeek V4.1 Flash scored highest of the models Reflection listed, at 90.6 percent, ahead of Kimi K3 at 88.3 percent and GLM 5.3 at 88.2 percent. On DeepSWE version 1.1, Beam scored 44.4 percent. That matched GLM 5.2 at 44.0 percent while finishing below Kimi K3 at 68.0 percent and DeepSeek V4.1 Flash at 74.2 percent.

On math and academic reasoning tests, Beam scores above Inkling and below GLM 5.2. On the American Invitational Mathematics Examination (AIME) 2026, Beam scored 97.8 percent, compared to 97.1 percent for Inkling and 99.2 percent for GLM 5.2. On Humanity's Last Exam (HLE) without external tools, Beam achieved 36.2 percent. That exceeded Inkling at 29.7 percent while trailing GLM 5.2 at 40.5 percent and Kimi K3 at 46.9 percent. On Graduate-Level Google-Proof Q&A (GPQA Diamond), Beam achieved 90.5 percent, against 87.2 percent for Inkling, 91.2 percent for GLM 5.2, 92.6 percent for Qwen 3.8 Max, and 93.5 percent for Kimi K3. On the Model Context Protocol benchmark (MCP Atlas), Beam reached 78.7 percent. That finished ahead of Inkling at 76.0 percent but behind Qwen 3.8 Max at 84.5 percent.

- SWE-bench Verified: Beam 80.9 percent versus Inkling 77.6 percent and Nemotron 3 Ultra 70.7 percent.
- SWE-Bench Pro v2-Hard: Beam 77.2 percent versus GLM 5.3 84.3 percent and Kimi K3 88.2 percent.
- Terminal Bench v2.1: Beam 80.1 percent versus Kimi K3 88.3 percent and DeepSeek V4.1 Flash 90.6 percent.
- DeepSWE v1.1: Beam 44.4 percent versus GLM 5.2 44.0 percent and DeepSeek V4.1 Flash 74.2 percent.
- AIME 2026: Beam 97.8 percent versus Inkling 97.1 percent and GLM 5.2 99.2 percent.
- GPQA Diamond: Beam 90.5 percent versus GLM 5.2 91.2 percent and Kimi K3 93.5 percent.
- MCP Atlas: Beam 78.7 percent versus Inkling 76.0 percent and Qwen 3.8 Max 84.5 percent.

## Computational Efficiency Claims and Evaluation Caveats

Reflection AI claims that Beam delivers performance comparable to GLM 5.2 on advanced reasoning tasks while consuming three to four times less inference compute. In a report published by VKTR, independent benchmarking group Artificial Analysis noted that early indicators suggest Beam will be one of the most token-efficient open models relative to its intelligence tier. For scale, TechCrunch puts GLM 5.2 at roughly 744 billion total parameters with 40 billion active, against Beam's 23 billion active.

Reflection AI emphasizes that its inference efficiency calculation is derived from theoretical floating-point operations (FLOPs), estimated as twice the active parameters multiplied by mean generated tokens. This metric represents an approximate compute comparison rather than measured real-world serving expenditures, because it excludes initial prompt prefill, attention memory operations, and hosting infrastructure overhead. Additionally, reporting from TechCrunch and silicon.co.uk confirms that Reflection AI's published benchmark scores have not yet undergone independent verification.

On information-retrieval benchmarks requiring context management, Beam trails leading Chinese systems. On BrowseComp with context management, Beam scored 77.4 percent, matching Inkling at 77.1 percent while finishing behind Kimi K3 at 91.2 percent. On DeepSearchQA with context management, Beam scored 80.1 percent compared to 95.0 percent for Kimi K3. Reflection's table also includes evals run by Artificial Analysis. Beam scored 79.3 on AA-LCR against 88.7 for Kimi K3, and 13.0 on the public split of AA Omniscience against 14.2 for Inkling.

## Safety Distillation and Reasoning Controls

Beam incorporates a dedicated safety model developed through Supervised Fine-Tuning (SFT) and reinforcement learning before distillation into the primary weights. Reflection AI structured its alignment process around two distinct teacher systems: a large-scale reinforcement learning teacher and a dedicated safety teacher. Both teachers merged through multi-teacher on-policy distillation (MOPD).

The model's alignment principles follow three hierarchical tiers. These include strict safety rules and identity boundaries, behavioral qualities such as factual accuracy and uncertainty acknowledgment, and default stylistic preferences. Alignment was refined using deliberative alignment methods combined with iterative adversarial red-teaming. Reflection AI published a Beam Safety Technical Report with the October 16 release describing its safety methodology and evaluations.

Reflection AI has not published named levels for Beam's reasoning effort setting.

## Weights Release and API Access

On October 16, 2026, Reflection AI released Beam's model weights on Hugging Face under an Apache 2.0 license. Weights ship in BF16, FP8, and NVFP4 formats alongside a speculative decoding drafter model. Accompanying files include a model card with final benchmarks and a Beam Safety Technical Report.

Reflection released the weights on Hugging Face in BF16, FP8 and NVFP4 formats. Developers who want to test first can call the public beta API from the Reflection AI developer portal.

Hosted programmatic access runs in a public beta through the Reflection AI developer portal. Reflection AI says access is free to start. The API offers an OpenAI-compatible endpoint and a Reflection client endpoint, but the documentation checked on October 16, 2026 lists no per-token rates.

## Commercial Strategy and Sovereign AI Deployments

Reflection AI operates a commercial business model focused on selling enterprise software, customization tooling, and private infrastructure rather than charging per-token margins on closed models. On the Sources podcast, as quoted by VKTR, co-founder and CEO Misha Laskin said: "Closed models are the equivalent in real estate to renting an apartment."

Laskin and CTO Ioannis Antonoglou founded Reflection AI in 2024. Both are former Google DeepMind researchers. Reflection AI's 2025 announcement says it raised $2 billion. Backers include NVIDIA, Sequoia Capital, Lightspeed Venture Partners, B Capital, Citi, CRV, Disruptive, DST, Eric Schmidt, Zoom Ventures, and 1789. Reflection AI's newsroom lists an April 2026 report that its latest round closed at a $25 billion pre-money valuation. Total funding is reported, not disclosed. TechCrunch puts it at roughly $4.7 billion, and VKTR says $4.6 billion.

Reflection AI's homepage lists what it sells around the models. Offerings include deployable containers, customization recipes, agentic harnesses, an API platform, and private cloud, on-prem, air-gapped and edge deployment. Its sovereign solutions page pitches a turnkey stack "at a managed cost" instead of "perpetual per-token API margins." Reflection AI's about page says it is building a sovereign AI cloud in South Korea with Shinsegae, "backed by both the U.S. and Korean governments." The about page also lists Reflection AI as "validated on Dell AI Factory with NVIDIA." Reflection AI participates in the United States Department of Energy (DOE) Genesis Mission. Its newsroom lists compute deals with SpaceX (up to $6.3 billion) and Nebius ($1 billion). TechCrunch says the deals, worth more than $7 billion together, secure NVIDIA GB300 access through 2029.

## Implementation Evaluation and Operational Tradeoffs

Organizations evaluating Reflection AI Beam should separate immediate hosted testing from long-term self-hosted production deployments. You can download the Apache 2.0 weights now or call Beam through the public beta API. If your coding tools call OpenAI-compatible endpoints, test Beam's latency, instruction following and tool use.

Beam represents a strong evaluation candidate for enterprises seeking to reduce inference expenses on large-scale agent pipelines. It also avoids closed-model vendor lock-in. If you are also testing Mistral Large 4, run both on the same internal tasks. Reflection's own table favors other models for long-context search or terminal tasks. Kimi K3 beat Beam on DeepSearchQA, 95.0 to 80.1, and DeepSeek V4.1 Flash beat it on Terminal Bench v2.1, 90.6 to 80.1.

Engineering teams should begin at Reflection's developer site. Next, assemble standardized internal test suites. Use them to evaluate Beam's coding accuracy against proprietary baselines.

## Frequently Asked Questions

- Reflection AI released Beam under an open-source Apache 2.0 license on October 16, 2026, granting full commercial and modification rights. The weights are on Hugging Face in BF16, FP8 and NVFP4 formats.
- Beam features 501 billion total parameters arranged in a sparse Mixture-of-Experts architecture, activating 23 billion parameters per token during inference.
- Reflection AI released the weights on October 16, 2026, eleven days after the October 5 announcement, together with a model card and a safety technical report.
- On Reflection AI's own benchmarks, Beam trails both. DeepSeek V4.1 Flash leads it on Terminal Bench v2.1 (90.6 vs 80.1) and DeepSWE v1.1 (74.2 vs 44.4). Kimi K3 leads on SWE-Bench Pro v2-Hard (88.2 vs 77.2) and DeepSearchQA (95.0 vs 80.1), and Reflection says Kimi K3 is still ahead on raw capability. None of these scores had been independently verified as of October 2026.
- Reflection AI was founded in 2024 by former Google DeepMind researchers Misha Laskin, who serves as Chief Executive Officer, and Ioannis Antonoglou, who serves as Chief Technology Officer.
- Beam supports up to 1 million tokens of effective context after midtraining, while its reinforcement learning phase was conducted with a maximum context length of 256,000 tokens.
- Developers can download the weights from Hugging Face, call the public beta API documented at developers.reflection.ai through native and OpenAI-compatible endpoints, or deploy through cloud and inference partners such as Amazon SageMaker JumpStart.

### Plan Your Open-Weight AI Architecture

Evaluate how emerging open-weight models like Reflection AI Beam fit into your private cloud, on-premises infrastructure, or agentic automation pipelines.

Book a Consultation
