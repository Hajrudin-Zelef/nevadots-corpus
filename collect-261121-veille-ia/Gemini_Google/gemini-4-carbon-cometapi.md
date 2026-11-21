---
id: collect-261121-veille-ia/gemini-google/gemini-4-carbon-cometapi
title: "Gemini 4 Carbon | CometAPI"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["carbon", "gemini", "google", "model", "tokens", "complex", "engineering", "output"]
source: https://www.cometapi.com/models/google/gemini-4-carbon/
source_anchor: ""
source_lines: [1, 161]
section: "Gemini_Google"
---
## Technical Specifications

The November 10 model card and developer documentation now provide concrete technical details. This replaces the earlier uncertainty about Carbon's public API availability.

| Specification | Gemini 4 Carbon | 
|---|---|
| Developer | Google DeepMind | 
| Model family | Gemini 4 | 
| Model ID | gemini-4-carbon | 
| Announcement | October 28, 2026 | 
| Public API availability | November 10, 2026 | 
| Context window | 1,048,576 tokens | 
| Maximum output | Up to 1,048,576 tokens with long decoding | 
| Standard single-request output | 65,536 tokens streaming; 32,768 non-streaming | 
| Input modalities | Text, images, audio, video, PDF and supported document files | 
| Output modality | Text | 
| Thinking | Low, Medium, High | 
| Default thinking level | Medium | 
| Context caching | Implicit caching | 
| Knowledge cutoff | March 2026 | 

Important token-limit distinction: Carbon has a unified 1M-token input-and-output budget. The 1M figure is not an additional 1M output tokens on top of the input. A normal foreground request also has a smaller output-segment limit; longer generations require the long-decoding mechanism.

## What Is Gemini 4 Carbon?

Gemini 4 Carbon is a frontier AI model developed by Google DeepMind, announced on October 28, 2026, and made generally available to developers, enterprises, scientists, and eligible consumers on November 10, 2026.

Carbon is designed for demanding, long-horizon tasks that require sustained reasoning, complex software engineering, scientific research, and cybersecurity defense. Compared with Gemini 4 Argon, its sibling model, Carbon uses more thinking tokens to explore hypotheses, investigate difficult problems, and perform extended verification loops.

Google positions Argon as the better choice for most everyday software engineering and knowledge-work tasks, while Carbon targets problems that push the limits of model intelligence.

## Key Features of Gemini 4 Carbon

### Long-Horizon Agentic Reasoning

Carbon is built to work through complex tasks over extended periods rather than simply responding to individual prompts. It can investigate large codebases, trace subtle bugs across interconnected systems, and carry out multi-step engineering workflows with repeated testing and verification.

Google's launch examples include multi-agent projects that reconstructed complex 3D environments and built interactive applications from extensive technical documents. These demonstrations illustrate its agentic potential, although they should not be interpreted as guaranteed production performance.

blog.google

### Advanced Cybersecurity and Vulnerability Research

Cybersecurity is one of Carbon's most distinctive strengths. Its reported capabilities span three stages of the vulnerability-defense lifecycle:

- Vulnerability discovery: Identify complex software vulnerabilities.
- Exploit validation: Determine whether discovered vulnerabilities can actually be exploited.
- Root-cause remediation: Generate code patches that address underlying security weaknesses.

Google reports 88.2% on SEC-bench Pro, 54.8% on ExploitGym, and 71.0% on CWE-bench. These evaluations were conducted without guardrails, and specialized cybersecurity capabilities are subject to access restrictions through Google's Fairwind Program.

### Advanced Coding and Software Engineering

Carbon is intended for difficult engineering work, including multi-package migrations, architectural redesigns, cross-system debugging, and full-stack application development.

It can work through build-and-verify cycles, inspect rendered interfaces, identify implementation errors, and refine the result. Google also reports a 65.2% score on Terminal-Bench 4.0 and a 61.2% score on FrontierSWE 2.

### Scientific and Mathematical Reasoning

Carbon is designed to support research workflows that involve analyzing experimental data, testing hypotheses, and reasoning through complex scientific problems.

Google highlights examples involving formal verification, protein-homolog discovery, enzyme assay analysis, and scientific simulation. These are research demonstrations rather than independent proof that the model can replace specialist scientific validation.

### Multimodal Understanding and Structured Knowledge Work

Carbon also targets knowledge-intensive workflows involving dense documents, charts, tables, blueprints, and technical diagrams. It can turn complex information into structured deliverables such as spreadsheets, presentations, reports, and interactive visualizations.

This makes it relevant to financial research, enterprise analysis, technical documentation, and data-heavy workflows.

## Gemini 4 Carbon Benchmark Performance

The following results come from Google's November 10, 2026 model card. They are vendor-published evaluations, not a single independent benchmark ranking.

### Overall Benchmark Results

| Benchmark | Gemini 4 Carbon | Gemini 4 Argon | GPT-6 Astra | Claude Opus 5.5 | 
|---|---|---|---|---|
| Artificial Analysis Index v5.0 | 59 | 50 | 57 | 58 | 
| Vals Index v2.2 | 70.4% | 69.2% | 65.0% | 70.2% | 
| SWE-sweep | 9.6% | 4.3% | 9.5% | 3.6% | 
| FrontierSWE 2 | 61.2% | 55.0% | 65.5% | 62.3% | 
| Terminal-Bench 4.0 | 65.2% | 57.1% | 59.1% | 59.6% | 
| Agents' Last Exam v1.1 | 77.4% | 69.0% | 73.1% | 71.6% | 
| OSWorld-2.1 | 81.5% | 74.7% | 80.9% | 87.2% | 
| AutomationBench | 53.9% | 51.3% | 41.4% | 42.5% | 
| Chartography | 72.5% | 71.6% | 71.0% | 66.3% | 
| GraphWalks (256K–1M) | 96.2% | 84.2% | 71.8% | 66.8% | 
| RiemannBench | 80.0% | 76.0% | 72.0% | 69.6% | 
| CWE-bench | 71.0% | 68.0% | 68.0% | 67.0% | 
| ExploitGym | 54.8% | 42.0% | 42.4% | 34.5% | 

### What Do These Results Tell Us?

General intelligence: Carbon leads the listed models on Artificial Analysis Index v5.0 with a score of 59, narrowly ahead of Claude Opus 5.5 at 58 and GPT-6 Astra at 57.

Agentic coding: Carbon achieves 9.6% on SWE-sweep, slightly above GPT-6 Astra's 9.5%. However, GPT-6 Astra leads FrontierSWE 2 with 65.5%, compared with Carbon's 61.2%. Carbon is therefore not the top performer on every coding benchmark.

Computer use: Carbon reaches 77.4% on Agents' Last Exam v1.1, but Claude Opus 5.5 leads OSWorld-2.1 at 87.2%.

Long-context reasoning: Carbon's 96.2% GraphWalks result is a substantial lead over the other listed models. This benchmark evaluates graph traversal reasoning across context lengths from 256K to 1M tokens.

Cybersecurity: Carbon leads the listed competitors on all three displayed cybersecurity benchmarks: SEC-bench Pro, ExploitGym, and CWE-bench.

The overall picture is that Carbon is particularly strong in long-context reasoning, agentic execution, and cybersecurity. It should not be described as universally superior to GPT-6 Astra or Claude Opus 5.5.

## Gemini 4 Carbon vs. Gemini 4 Argon

Gemini 4 Carbon is an evolution of Gemini 4 Argon, rather than an entirely separate model family.

| Dimension | Gemini 4 Carbon | Gemini 4 Argon | 
|---|---|---|
| Primary positioning | Long-horizon engineering and agentic workflows | General frontier reasoning | 
| Artificial Analysis Index | 59 | 50 | 
| SWE-sweep | 9.6% | 4.3% | 
| Terminal-Bench 4.0 | 65.2% | 57.1% | 
| GraphWalks | 96.2% | 84.2% | 
| SEC-bench Pro | 88.2% | 83.4% | 
| ExploitGym | 54.8% | 42.0% | 
| CWE-bench | 71.0% | 68.0% | 
| Thinking-token usage | More thinking tokens for complex tasks | Lower relative usage | 
| Model architecture | Based on Argon | Base model | 

The key improvement is not simply higher benchmark scores. Carbon is designed to sustain more complex reasoning and verification loops, making it better suited to tasks where a model must work through multiple stages before delivering a result.

## Gemini 4 Carbon vs. Claude Opus 5.5 vs. GPT-6 Astra

For developers selecting a frontier model, these three systems have different performance profiles.

| Area | Gemini 4 Carbon | Claude Opus 5.5 | GPT-6 Astra | 
|---|---|---|---|
| General intelligence index | 59 | 58 | 57 | 
| SWE-sweep | 9.6% | 3.6% | 9.5% | 
| FrontierSWE 2 | 61.2% | 62.3% | 65.5% | 
| Terminal-Bench 4.0 | 65.2% | 59.6% | 59.1% | 
| OSWorld-2.1 | 81.5% | 87.2% | 80.9% | 
| GraphWalks | 96.2% | 66.8% | 71.8% | 
| CWE-bench | 71.0% | 67.0% | 68.0% | 

Choose Gemini 4 Carbon when long-horizon agentic execution, extended context, scientific reasoning, or cybersecurity defense is central to the workload.

Consider Claude Opus 5.5 when computer-use performance is a major priority, given its stronger OSWorld-2.1 result in this comparison.

Consider GPT-6 Astra for coding workloads where FrontierSWE 2 performance is especially important.

These recommendations reflect the reported benchmark profiles, not a claim that one model will outperform the others on every real-world workload.

## Gemini 4 Carbon is available on CometAPI

Use model ID `gemini-4-carbon` through the native Gemini API or the OpenAI-compatible Chat API. Both formats support ordinary and streaming responses.

Google made Carbon generally available on November 10, 2026. It is designed for complex software engineering, mathematical and scientific problem solving, and sustained multi-step agent workflows.

### Official model specifications

Google lists a unified input-and-output context window of 1,048,576 tokens and a default synchronous maximum output of 65,536 tokens. Thinking tokens count toward output usage. Applications should reserve sufficient output budget for both thinking and the visible answer.

Google also documents multimodal inputs and an optional long-decode mode. Availability of optional capabilities depends on the API format and configuration.

### API access

Use your CometAPI API key with either the Gemini API or Chat API.
