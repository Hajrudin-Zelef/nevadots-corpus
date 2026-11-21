---
id: collect-261121-veille-ia/modeles-open/beam
title: "Beam"
domain: modeles-open
role: reference
task: article
actors: []
dates: []
keywords: ["beam", "reflection", "reasoning", "model", "cloud", "post", "effort", "tokens"]
source: https://lmstudio.ai/models/beam
source_anchor: ""
source_lines: [1, 35]
section: "Modeles_Open"
---
Reflection's text-only Mixture-of-Experts model for coding, reasoning, and agentic work, with 501B total parameters and 23B active per token. Available through LM Studio Cloud in Bionic.

Beam is a text-only Mixture-of-Experts model for coding, reasoning, and agentic work, developed by Reflection, an AI lab building open models. It has 501B total parameters with 23B active per token, combining software-engineering and tool-use capabilities with a focus on efficient inference.

*Publisher-reported results from Reflection's Beam launch post, including its October 8 benchmark update. The figure's 1M context figure describes the model, not a verified LM Studio Cloud endpoint limit.*

Beam is available through LM Studio Cloud in Bionic. It supports text input, text output, and tool calling. **Beam does not accept image input.** This page lists the Cloud model only, with no local downloads attached.

Beam always reasons. Bionic offers Minimal, low, medium, high, and xhigh reasoning effort. The off setting is treated as minimal effort—it does not disable reasoning. These settings control effort, not fixed reasoning-token or response lengths.

For API requests, Beam also supports max effort and defaults to max when no effort is specified; none is treated as minimal. The default output budget is 65,536 tokens, with explicit budgets supported up to 131,072 tokens. Multi-turn API clients should preserve `reasoning_content` from every prior assistant turn, including tool calls.

Reflection trained Beam with a focus on software engineering and multi-step tasks that use tools and respond to environment feedback. Its examples include building applications, navigating codebases, working in a terminal, and preparing machine-learning workflows.

Beam was pretrained on 23.8 trillion tokens and refined through large-scale reinforcement learning. Reflection reports more than 100 million RL rollouts across coding, agentic, and STEM environments. The training process rewards successful solutions while discouraging unnecessary reasoning tokens, aiming to improve the tradeoff between capability and inference cost.

Its sparse MoE architecture activates 23B parameters per token rather than the full 501B. Interleaved local and global attention supports long-context work. Reflection describes extending the model's context to 1M tokens during midtraining; Baseten's enforced hosted limit has not yet been confirmed for this Cloud configuration.

Reflection reports the following scores in its launch post:

| Benchmark | Beam | 
|---|---|
| DeepSWE v1.1 | 44.4 | 
| SWE-Bench Pro v2-Hard | 77.2 | 
| SWE-Bench Pro v1 | 65.5 | 
| Terminal Bench v2.1 | 80.1 | 
| SWE Atlas Codebase QnA | 34.6 | 
| SWE-Bench Multilingual | 78.0 | 
| SWE-Bench Verified | 80.9 | 

These are Reflection's reported results, not measurements of LM Studio Cloud. Evaluation harnesses, reasoning effort, and tool access affect scores; consult the original post for comparison details and methodology.

Reflection introduced Beam on October 5, 2026. Its launch post announces an Apache 2.0 release of the weights and developer artifacts later in October; this page does not imply that downloadable weights are already available. Baseten lists Beam as a preview Model API.

See Reflection's launch post, developer documentation, and reasoning guide, plus Baseten's Beam listing, for model details and serving information.
