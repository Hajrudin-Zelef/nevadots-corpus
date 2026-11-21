---
id: collect-261121-veille-ia/modeles-open/reflection-beam-501b-open-weight-model-explained
title: "Reflection Beam: 501B Open-Weight Model Explained"
domain: modeles-open
role: reference
task: article
actors: []
dates: []
keywords: ["model", "reflection", "beam", "reasoning", "card", "open-weight", "agentic", "using"]
source: https://test.eigent.ai/blog/reflection-beam-open-weight-model
source_anchor: ""
source_lines: [1, 69]
section: "Modeles_Open"
---
Reflection has released **Beam**, its first open-weight model, with the weights on Hugging Face under an Apache 2.0 license. Beam is a 501-billion-parameter sparse Mixture-of-Experts model built for coding, reasoning, and long-horizon agentic work. This guide covers what shipped, how it benchmarks, what it costs through a hosted API, and how to run it yourself — using the numbers from Reflection's own model card.

Beam is a sparse Mixture-of-Experts (MoE) model with **501 billion total parameters and ~23 billion active per token**, released as `Beam-1.0-501B-A23B`. It is text-in, text-out, and was built for coding, reasoning, and agentic workloads, with a stated focus on token-efficient reasoning.

According to the Hugging Face model card, Beam has 52 layers (3 dense + 49 MoE), 256 experts, a **1M-token context window**, a 152k-token vocabulary, and a knowledge cutoff of June 30, 2026. It uses gated grouped-query attention with sliding-window attention and a modified Harmony chat format.

The headline pitch is efficiency rather than peak capability: Reflection says Beam reaches solution quality comparable to leading open models while using roughly **3x fewer inference-time reasoning tokens**, as noted on the model card.

The most important detail for teams is the license. Reflection released Beam's weights on Hugging Face under an Apache 2.0 license — one of the most permissive options available, allowing commercial use, redistribution, and modification.

The model card also states that Reflection does not prohibit distillation, using outputs to train other models, fine-tuning, parameter-efficient adaptation, quantization, or merging. That permissiveness is what keeps an open model in the conversation next to releases like Kimi K3 and DeepSeek, and it matters directly if you plan to ship Beam inside a product.

Reflection's final numbers live in the model card, evaluated at temperature 0.7 and top_p 0.99. Here are the BF16 results across the categories that matter for agent builders:

- **SWE-bench Verified: 81.6%** — resolving real GitHub issues
- **Terminal-Bench 2.1: 80.1%** — command-line task completion
- **SWE-bench Multilingual: 78.3%**
- **GPQA Diamond: 90.6%** — graduate-level science reasoning
- **AIME 2026: 97.7%** — competition mathematics
- **LiveCodeBench: 88.3%**
- **MCP Atlas: 80.8%** — using real-world tools via MCP servers
- **BrowseComp (with context management): 76.8%**
- **HLE: 36.0% without tools, 51.1% with tools**

These are strong agentic and reasoning scores for an open model, especially given the token-efficiency framing. On its own blog, Reflection positions Beam as competitive with larger open models like GLM 5.2 and approaching Qwen 3.8-Max on coding and agentic tasks. Independent write-ups have noted that models like Kimi K3 still lead on raw capability, so treat Beam as an efficiency play, not an across-the-board frontier win.

One note on numbers: early coverage citing Baseten's launch post listed slightly different figures (for example SWE-bench Verified at 80.9%). The model card values above are Reflection's final published benchmarks, so use those as the canonical reference.

Beam is free to download and self-host under Apache 2.0. If you'd rather call a hosted endpoint, Reflection opened a free-to-start public beta of its first-party API.

Among the launch partners, Baseten listed Model API pricing of $1.30 per million input tokens and $0.13 per million output tokens, plus $4.30 per million cache-input tokens. Prices vary by provider, so check the specific endpoint you plan to use on the day you test.

Reflection shipped Beam to run almost anywhere out of the box.

**Weights and formats.** The release includes **BF16, FP8, and NVFP4** precision formats plus a speculative-decoding drafter model. Reflection says it worked with partners on fused kernels and serving configs to cut memory and speed up generation.

**Day-zero framework support.** Per Reflection's announcement, Beam has native support in Transformers (Hugging Face), vLLM, SGLang, Unsloth, Axolotl, TokenSpeed, and NVIDIA Dynamo — so you can go from weights to a production endpoint without custom plumbing.

**Platforms.** Beam launched across a broad set of providers, including Amazon Web Services, Google Cloud, Baseten, CoreWeave, Databricks, Fireworks AI, Together AI, OpenRouter, Vercel, LM Studio, and Ollama. For on-premises, sovereign, and air-gapped setups, Dell and HPE deliver it behind the firewall.

Reflection also open-sourced **Mirror CLI**, the terminal agent harness it trained Beam against. Mirror pairs a Python frontend (prompts, skills, developer tooling) with a Rust agent core that drives the model. It supports resumable conversations, parallel agents in separate worktrees, project instructions, MCP connections, and Python extensions.

Reflection says it built Mirror for internal engineering first — with isolated workspaces, background sessions, and Slack, Linear, and GitHub integrations — before open-sourcing it. Beam also works with other harnesses, including Pi, OpenCode, and Hermes.

Reflection frames the launch as more than a weight drop. It calls the package an **"open operating system for agents,"** with the model as the kernel and reference agentic integrations, deployment containers, inference integrations, and customization pipelines open above the infrastructure layer — backed by an ecosystem of 40+ partners.

For customization, Reflection points to recipes and integrations for fine-tuning and large-scale reinforcement learning (with partners such as NVIDIA, Hugging Face TRL, Anyscale, and Axolotl), and says it will release self-distillation recipes and adopt the OpenEnv environment standard for future runs.

- **Agent and coding-tool builders** who want a permissively licensed model with strong SWE-bench and MCP tool-use scores.
- **Teams watching inference cost** , since the token-efficiency framing targets fewer reasoning tokens per task.
- **Regulated or on-prem deployments** that need the same stack running on a laptop and behind a firewall.

If raw peak capability on the hardest tasks is your only criterion, the current open leaders may still edge Beam out. If permissive licensing, agentic tool use, and efficiency matter more, Beam is a serious option on day one.

Beam is built for exactly the kind of long-horizon, tool-using work that multi-agent systems automate — reading repositories, running commands, and completing multi-step tasks. Eigent is an open-source "Cowork" desktop app that turns models like these into a local AI workforce that runs real workflows, from reviewing GitHub PRs to multi-step research. Because Eigent is model-flexible, you can point it at an open-weight model and keep control of your stack. Download Eigent and build your own agent team.

## Recent Posts

### Eigent Release Notes v1.0.8: A New Home, Global Search & Clearer Conversations

Eigent v1.0.8 introduces a redesigned Home, search across Spaces, a new composer, and a default conversation timeline that makes agent work easier to follow.

### GPT-6.2 Astra and Sol: What's New, Pricing, and Benchmarks

GPT-6.2 Astra and Sol are OpenAI's newest models for ChatGPT Work, Codex, and the API. Here's what changed, the pricing, the benchmarks, and which to use.

### What Is NVIDIA Nemotron for Science? Open Models for Research Agents

NVIDIA Nemotron for Science explained: the open model, LitmusBench v0.2 chemistry dataset, and post-training recipes for building AI agents for scientific research.
