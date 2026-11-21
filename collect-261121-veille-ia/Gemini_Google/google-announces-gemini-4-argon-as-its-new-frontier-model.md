---
id: collect-261121-veille-ia/gemini-google/google-announces-gemini-4-argon-as-its-new-frontier-model
title: "Google announces Gemini 4 Argon as its new frontier model"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-09-30"]
keywords: ["argon", "google", "gemini", "model", "tokens", "across", "frontier", "reasoning"]
source: https://9to5google.com/2026/09/30/gemini-4-argon-announcement/
source_anchor: ""
source_lines: [1, 42]
section: "Gemini_Google"
---
Gemini 4 Argon is Google’s “next era of frontier intelligence” and built for “deep reasoning across complex, long-horizon workflows.” It follows the cancellation of Gemini 3.5 Pro and focus on 3.8 Flash.

With this new model (which has a new naming scheme), Google wants to offer “frontier-level capabilities” across coding, knowledge work, cybersecurity defense, and creative writing.

Gemini 4 Argon’s output token limit is 1M tokens (up from 64K) to allow for longer and more complex use cases. This is paired with expanded coding, reasoning, and multimodal capabilities.

When the model has the headroom to think deeply and generate hundreds of thousands of tokens in a single trajectory, it adds a new level of depth in reasoning to solve tough problems in one go.


In terms of benchmarks, Google touts a DeepSWE v1.1 score of 77.9%. Claude Opus 5.5 comes in at 74.2% followed by GPT-6 Astra’s 74.1%.

Google trained Gemini 4 Argon to be “highly capable at cybersecurity defense” with leaps over 3.8 Flash Cyber. The model will be made available “without cyber guardrails” to trusted defenders and internal Google teams so that they can “leverage its full frontier-level cybersecurity defense capabilities.”

- “On CWE-bench v1, which evaluates the model’s ability to remediate security vulnerabilities, Argon ties for first place with a top score of 68%”

Outside of coding, Gemini 4 Argon also has “leading performance across other domain specific evaluations,” such as Vals Finance Agent v2 (multi-step financial research) and Harvey’s Legal Agent Benchmark (legal research and drafting).

- “On AutomationBench, Zapier’s benchmark measuring end-to-end execution across core business functions, Argon ranks #1 with a score of 51.3%.”
- “…on LVBench, which measures long video understanding, Argon is state of the art with a score of 91.7%.”

Gemini 4 Argon is “rolling out soon,” starting with Google AI Ultra subscribers and paid API customers.

- “Argon will launch at an introductory price of $2 per million input tokens and $10 per million output tokens, with cached input tokens priced at 95% off input token price.”
- “After the introductory period expires, the price of $4 per 1M input tokens and $20 per 1M output tokens will apply.”

So far, it has been made available to trusted testers and cyber defenders (via Fairwind Program). Before the broader release, Google is focusing on:

- **Defending against misuse** from cyber or chemical, biological, radiological, and nuclear (CBRN) attacks: Google has improved “techniques to monitor the model’s internal activations to spot misuse.”
- **Defending against prompt injection attacks** : Google’s “most resilient model yet against indirect prompt injections” with “leading in prompt injection robustness on the Gray Swan’s Indirect Prompt Injection (IPI) Benchmark.”
- **Hardening systems** : “we are hardening our sandboxed environments by isolating and sealing them before high-risk training or evaluations begin.”
- **Monitoring for misalignment** : Google is “deploying misalignment mitigations that monitor Argon’s chain-of-thought and actions and stop execution when necessary.”
  - “We used a similar system to monitor our training runs and send alerts to a dedicated incident response team, taking careful precautions against feeding the findings back into training so as to not risk shaping Argon’s reasoning to evade our monitoring.”

Inside Google, Gemini 4 Argon is already powering internal workflows with “thousands of Googlers highlighting the model’s strengths in specialized coding tasks, conducting deeper research, and writing quality.” The company shared some examples today:

**Memory efficiency:** A team of Argon agents analyzed fleet-wide profiling telemetry to autonomously identify and apply memory optimizations across Google’s data centers, freeing up over 300 TiB of memory once rolled out, with an estimated 500 TiB to 1 PiB in total savings.

**Large Scale Codebase Migrations and Optimizations:** Argon agents are working on migrating C/C++ codebases to Rust across Google—scaling from tens of thousands of lines in core libraries like re2, libgav1 up to 800K+ lines for the Fuchsia OS Zircon kernel. Given the criticality of many of these systems, such large-scale rewrites are undergoing rigorous automated and manual auditing, emulation testing, and review before rolling out to production.

For libgav1, Google’s open source software for decoding video, Argon agents took an existing Rust port and replaced 32K lines of SIMD code by running many rounds of profile-guided experiments, studying the compiler’s output, producing safe Rust so the compiler would vectorize it automatically. The end result is a memory-safe video decoder that runs 2.7x faster than the Rust port, with identical video output, bringing it closer to the optimized C++.

## Comments
