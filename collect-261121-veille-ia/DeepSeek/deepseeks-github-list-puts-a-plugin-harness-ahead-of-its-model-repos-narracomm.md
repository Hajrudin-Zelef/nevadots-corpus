---
id: collect-261121-veille-ia/deepseek/deepseeks-github-list-puts-a-plugin-harness-ahead-of-its-model-repos-narracomm
title: "DeepSeek’s GitHub list puts a plugin harness ahead of its model repos - Narracomm"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: []
keywords: ["under", "stars", "updated", "about", "harness", "deepseek", "repos", "attention"]
source: https://www.narracomm.com/deepseeks-github-list-puts-a-plugin-harness-ahead-of-its-model-repos/
source_anchor: ""
source_lines: [1, 23]
section: "DeepSeek"
---
*The deepseek-ai organization shows 44 public repositories. DeepSeek Harness sits near 241,000 stars, while kernel libraries tied to attention, expert routing and GPU math were updated Sept. 30.*

DeepSeek’s public code shelf is no longer just a place to drop model cards. On Wednesday the deepseek-ai organization on GitHub listed 44 repositories. The busiest names were not the famous chat models. They were a plugin harness and a set of low-level libraries that teams use to run those models.

DeepSeek Harness leads the catalog. The repository description reads, “DeepSeek Harness: Everything is a Plugin.” It is written in TypeScript, carries an MIT license and showed about 241,000 stars and 29,000 forks, with a push dated Sept. 29. Two smaller companion repos, dsh-libreoffice-kit and dsh-node-addon-require-builtin, are labeled internal components used by the harness. One is JavaScript under the Mozilla Public License 2.0. The other is C++ under MIT. Both were updated Sept. 30.

That gap matters for anyone budgeting an agent stack. A harness with that star count is a distribution channel, not a side project. Developers can treat plugins as the extension point. Executives can treat the repo as a signal of where DeepSeek wants outside code to attach.

The model repositories remain large. They are quieter. DeepSeek-V3, a Python repo under MIT, showed about 105,000 stars and 17,000 forks. Its last listed update was Aug. 28, 2025. DeepSeek-R1, also MIT, showed about 92,000 stars and 12,000 forks, last updated June 27, 2025. The R1 README states that DeepSeek-R1 achieves performance comparable to OpenAI-o1 across math, code and reasoning tasks, and that the lab open-sourced DeepSeek-R1-Zero, DeepSeek-R1 and six dense models distilled from DeepSeek-R1 based on Llama and Qwen.

Star counts are not usage. They do show where outside attention pooled after the 2025 releases. V3 and R1 still anchor the brand. The commit clocks on those two repos have not matched the infrastructure work landing this week.

That infrastructure work is the part a platform team would actually clone. FlashMLA, described as “FlashMLA: Efficient Multi-head Latent Attention Kernels,” is C++ under MIT, with about 13,000 stars and 1,200 forks, updated Sept. 30. DeepEP, “an efficient expert-parallel communication library,” is CUDA under MIT, about 10,000 stars, also updated Sept. 30. DeepGEMM, “clean and efficient BLAS kernel library on GPU,” is CUDA under MIT, about 7,900 stars, same day. DeepSelect covers “TopK kernels for DeepSeek Sparse Attention (DSA) and Samplers.” DeepJIT is “a lightweight library for xPU kernel JIT compilation.” TileKernels is “a kernel library written in tilelang.” All three were updated Sept. 30.

Read together, those six repos are a stack diagram. Attention kernels. Expert-parallel communication. Matrix math. Top-K selection for sparse attention. Just-in-time compilation for accelerators. A tile-language kernel library. None of that is a chatbot. It is the machinery under one.

Other systems repos fill the training side. 3FS is described as “a high-performance distributed file system designed to address the challenges of AI training and inference workloads.” It is C++ under MIT, about 10,000 stars, last updated May 7. DualPipe is “a bidirectional pipeline parallelism algorithm for computation-communication overlap in DeepSeek V3/R1 training,” Python under MIT.

Application-layer repos sit beside that systems work, and they are not small. awesome-deepseek-integration, “Integrate the DeepSeek API into popular software,” showed about 39,000 stars under a Creative Commons Zero license, last updated Feb. 23. DeepSeek-Coder, “DeepSeek Coder: Let the Code Write Itself,” is Python under MIT, about 24,000 stars, last updated Nov. 11, 2025. DeepSeek-OCR, “Contexts Optical Compression,” is Python under MIT, about 24,000 stars, last updated Jan. 27.

The pattern is plain enough for a build-versus-buy meeting. Model weights and papers live in a few heavily starred repos whose commit dates sit in 2025. The September 2026 activity clusters on plugins, kernels and communication libraries, most of them MIT. A lab that publishes the attention kernel, the expert-parallel library and the file system is inviting outside engineers onto the same path it uses internally. A lab that leads its own org page with a plugin harness is inviting them to extend the product, not only the weights.

License terms still govern use. MIT on the kernel repos is not a support contract. Star totals move. The listing reviewed here is the public deepseek-ai organization page as it stood Sept. 30, 2026.
