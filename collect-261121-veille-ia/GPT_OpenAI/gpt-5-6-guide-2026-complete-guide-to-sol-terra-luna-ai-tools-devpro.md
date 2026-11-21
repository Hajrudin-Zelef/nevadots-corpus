---
id: collect-261121-veille-ia/gpt-openai/gpt-5-6-guide-2026-complete-guide-to-sol-terra-luna-ai-tools-devpro
title: "GPT-5.6 Guide (2026) | Complete Guide to Sol, Terra & Luna | AI Tools DevPro"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["gpt5", "terra", "luna", "model", "reasoning", "guide", "cost", "models"]
source: https://aitoolsdevpro.com/ai-tools/gpt-5-6-guide/
source_anchor: ""
source_lines: [1, 390]
section: "GPT_OpenAI"
---
## Introduction

On July 9, 2026, OpenAI officially launched the GPT‑5.6 series, introducing a new tiered model family designed to offer frontier intelligence at dramatically lower costs. Unlike previous generations that offered a single “best” model, GPT‑5.6 introduces three distinct tiers—**Sol, Terra, and Luna**—named after the Latin words for Sun, Earth, and Moon. This new structure allows developers and enterprises to match model capability to task complexity, optimizing both performance and cost.

The launch represents a significant shift in OpenAI’s strategy. While Sol delivers flagship-level reasoning and coding performance, Terra offers GPT‑5.5-level capability at half the cost, and Luna brings advanced agentic capabilities to high‑volume workloads at an unprecedented price point. Just three weeks after launch, OpenAI further reduced Terra and Luna API prices by 20% and 80% respectively, while introducing a Fast mode for Sol.

This guide provides a comprehensive overview of the GPT‑5.6 family, covering architecture, capabilities, pricing, benchmarks, use cases, and decision frameworks to help you choose the right model for your workload.

## What Is GPT‑5.6?

GPT‑5.6 is OpenAI’s flagship generation of large language models, succeeding GPT‑5.5. The series represents a major leap in **intelligence per dollar**—OpenAI’s stated goal with this release was to make every token produce more practical value, delivering stronger performance at lower cost.

### The Naming System

The GPT‑5.6 family uses a new naming convention: **the number (5.6) represents the generation**, while **the tier name (Sol, Terra, Luna) represents the capability level**. Each tier can evolve independently in future updates, allowing OpenAI to refine specific tiers without requiring a full generational release.

The three tiers map to distinct positions on the cost‑performance curve:

| Tier | Name | Positioning | 
|---|---|---|
| **Sol** | Sun | Flagship model for complex reasoning, coding, and agentic workloads | 
| **Terra** | Earth | Balanced model for everyday work, matching GPT‑5.5 at half the cost | 
| **Luna** | Moon | Fastest, most cost‑efficient model for high‑volume, cost‑sensitive tasks | 

### Availability

GPT‑5.6 is available across **ChatGPT, Codex, and the OpenAI API**. Sol is available to paid subscribers, while Terra and Luna are accessible to free users as well. The models became generally available on **Amazon Bedrock** on July 13, 2026.

## GPT‑5.6 Architecture

GPT‑5.6 builds on the transformer architecture with several key enhancements that improve reasoning efficiency, tool use, and long‑context handling.

### Core Architecture Highlights

- **1.05M token context window** – Supports long‑form documents, extended conversations, and complex multi‑step workflows.
- **128K max output tokens** – Enables generation of lengthy reports, codebases, and documents.
- **Knowledge cutoff** : February 16, 2026.
- **Multimodal capabilities** : All GPT‑5.6 models support text and image input, with text output.

### Reasoning Modes

GPT‑5.6 Sol introduces configurable reasoning effort levels that allow developers to trade off intelligence for speed and cost:

| Mode | Description | 
|---|---|
| **Standard** | Efficient reasoning for everyday tasks | 
| **Max** | Deeper reasoning for complex problems, exploring alternatives and running checks | 
| **Ultra** | Highest performance setting coordinating multiple parallel agents (up to 16) for complex tasks | 

Ultra mode is available only for Pro and Enterprise users in ChatGPT Work, and Plus and above in Codex.

### Agentic Harness

GPT‑5.6 is designed to work within an **agentic harness** that connects models to tools, context, and multi‑step workflows. This enables:

- **Tool calling** – Execute functions, query databases, and interact with external APIs.
- **Multi‑agent coordination** – Ultra mode orchestrates multiple agents in parallel.
- **Long‑running workflows** – Models can plan, execute, and iterate over extended time horizons.

## The GPT‑5.6 Family: Sol, Terra, and Luna

### GPT‑5.6 Sol (Flagship)

Sol is OpenAI’s most capable model, designed for **complex reasoning, coding, research, and agentic workflows**.

**Key capabilities**:

- **State‑of‑the‑art coding** : Leads the Artificial Analysis Coding Agent Index at 80 points.
- **Advanced reasoning** : Scores 53.6 on Agents’ Last Exam, outperforming Claude Fable 5 by 13.1 points.
- **Multimodal vision** : Achieved 73.0% on vision benchmarks, up from 64.9% for GPT‑5.5.
- **Enterprise‑grade outputs** : Generates polished presentations, documents, and spreadsheets with improved accuracy.
- **Cybersecurity** : Matches Anthropic’s Mythos Preview on ExploitBench with one‑third the token usage.

**Best for**: Research, advanced coding, cybersecurity, complex agentic workflows, and tasks requiring the highest reasoning capability.

### GPT‑5.6 Terra (Balanced)

Terra is the **default choice for production workloads**—a balanced model that delivers GPT‑5.5‑level performance at approximately half the cost.

**Key capabilities**:

- **GPT‑5.5‑level intelligence** : Matches the previous generation’s capabilities at significantly lower cost.
- **Broad applicability** : Suitable for most everyday business and development tasks.
- **Cost‑efficient** : Priced at roughly half of Sol.

**Best for**: Daily development work, business applications, content generation, and general‑purpose AI tasks where Sol’s extra capability isn’t justified.

### GPT‑5.6 Luna (Cost‑Optimized)

Luna is the **fastest and most affordable** model in the family, designed for high‑volume, cost‑sensitive workloads.

**Key capabilities**:

- **Nearly GPT‑5.5 performance** : Luna approaches GPT‑5.5’s best performance at less than half the estimated cost.
- **Tool‑capable** : Can use tools and complete multi‑step workflows—not just simple classification.
- **High throughput** : Optimized for large‑scale, repetitive tasks.

**Best for**: High‑volume classification, extraction, routing, code review, monitoring, and multi‑step workflows where cost per task is critical.

## Core Features

### Coding & Agentic Workflows

GPT‑5.6 Sol leads all models in the **Artificial Analysis Coding Agent Index** at 80 points, outperforming Claude Fable 5 by 2.8 points while using **half the output tokens, half the time, and one‑third the cost**.

The models are designed for **agentic coding**—they can plan, execute, and iterate across multiple files and steps. Sol’s token efficiency in AI agent programming tasks is **54% higher** than the previous generation.

### Tool Calling & Multi‑Step Reasoning

All GPT‑5.6 models support **tool calling**, enabling them to query databases, execute functions, and interact with external APIs. Luna’s ability to use tools and handle multi‑step workflows makes it suitable for agentic tasks that were previously reserved for more expensive models.

### Long Context

With a **1.05M token context window**, GPT‑5.6 can process entire codebases, long research papers, and extended conversations in a single request.

### Vision & Multimodal

All GPT‑5.6 models support **text and image input**. Sol achieved 73.0% on vision benchmarks, a significant improvement over GPT‑5.5’s 64.9%. Sol also leads the **MMMU-Pro w/ Python** leaderboard at 84.6%.

### File & Document Generation

Sol significantly improves the quality of **presentations, documents, and spreadsheets**, producing more polished, accurate outputs. It can create fully editable presentations from scratch.

## Benchmarks

### Intelligence Index

In the **Artificial Analysis Intelligence Index**, GPT‑5.6 Sol (max) scores **59 points**, just 1 point below Claude Fable 5 (max), at approximately **one‑third the cost**.

| Model | Intelligence Index Score | Cost per Task | 
|---|---|---|
| GPT‑5.6 Sol (max) | 59 | ~$1.04 | 
| GPT‑5.6 Terra (max) | 55 | ~$0.55 | 
| GPT‑5.6 Luna (max) | 51 | ~$0.21 | 
| GPT‑5.5 | ~50 | — | 

### Coding Agent Index

GPT‑5.6 Sol (max) leads the **Artificial Analysis Coding Agent Index** at **80 points**, leading in all three evaluations: DeepSWE, Terminal‑Bench v2, and SWE‑Atlas‑QnA.

| Model | Coding Agent Index | 
|---|---|
| GPT‑5.6 Sol (max) | 80 | 
| GPT‑5.6 Terra (max) | 77 | 
| GPT‑5.6 Luna (max) | 75 | 

### Agents’ Last Exam

In the **Agents’ Last Exam**, a comprehensive evaluation of long‑horizon professional work, GPT‑5.6 Sol scored **53.6**, surpassing Claude Fable 5 (adaptive reasoning) by 13.1 points.

### Vision Benchmarks

Sol achieved **73.0%** on Roboflow’s vision benchmark, up from 64.9% for GPT‑5.5. Sol also leads the **MMMU‑Pro w/ Python** leaderboard at 84.6%.

### Terminal‑Bench 2.1 (Coding)

Sol scored **88.8% in standard mode** and **91.9% in Ultra mode**, outperforming Claude Mythos 5’s 88.0%.

### ARC‑AGI‑3

GPT‑5.6 Sol is the **first model to win an ARC‑AGI‑3 public game** (ft09, 87%), averaging 13.33% on Public and 7.78% on Semi‑Private.

## Pricing

### Standard API Pricing (Effective July 31, 2026)

Following significant price reductions on July 30, 2026, GPT‑5.6 API pricing is as follows:

| Model | Input (per 1M tokens) | Output (per 1M tokens) | 
|---|---|---|
| **GPT‑5.6 Sol** | $5.00 | $30.00 | 
| **GPT‑5.6 Terra** | $2.00 | $12.00 | 
| **GPT‑5.6 Luna** | $0.20 | $1.20 | 

**Key pricing notes**:

- Luna’s price dropped **80%** from initial launch pricing ($1/$6).
- Terra’s price dropped **20%** from initial launch pricing ($2.50/$15).
- Sol’s price remains unchanged, but **Fast mode** offers up to 2.5× faster processing at 2× the price.
- Pricing changes apply to **Codex and ChatGPT Work** subscription usage as well.

### Reason for Price Reduction

OpenAI attributed the price cuts to **efficiency gains from GPT‑5.6 itself**. The model helped optimize production GPU kernels (reducing service costs by 20%) and improved speculative decoding (increasing token generation efficiency by 15%).

## Use Cases

### Developers & Software Engineering

**Best model**: Sol (for complex tasks), Terra (for daily work)

- Generate, review, and refactor code
- Debug and troubleshoot
- Build agentic coding workflows
- Automate code review (Luna now powers auto‑review in Codex CLI)

### Enterprise Knowledge Work

**Best model**: Terra (default), Sol (for complex analysis)

- Generate reports, presentations, and spreadsheets
- Analyze documents and data
- Synthesize research findings
- Automate workflow documentation

### Customer Support & Service

**Best model**: Luna (high‑volume), Terra (complex queries)

- Automated response generation
- Ticket classification and routing
- Knowledge base search and summarization
- Multi‑step support workflows

### Research & Scientific Computing

**Best model**: Sol

- Literature review and synthesis
- Data analysis and interpretation
- Hypothesis generation
- Complex reasoning tasks

### High‑Volume Automation

**Best model**: Luna

- Classification and extraction
- Routing and triage
- Code review and monitoring
- Multi‑step workflows at scale

### Agentic Workflows

**Best model**: Sol (planning), Luna (execution)

- Use Sol for planning and resolving uncertainty
- Use Luna for implementing well‑specified changes, writing tests, and evaluating results

## GPT‑5.6 vs GPT‑5.5

| Aspect | GPT‑5.6 Sol | GPT‑5.5 | 
|---|---|---|
| **Intelligence** | ~59 on Intelligence Index | ~50 | 
| **Coding Agent Index** | 80 | Lower | 
| **Vision** | 73.0% | 64.9% | 
| **Pricing** | $5/$30 (same as GPT‑5.5) | $5/$30 | 
| **Token efficiency** | 54% more efficient for agentic coding | — | 

Terra delivers GPT‑5.5‑level performance at **half the cost**. Luna approaches GPT‑5.5’s peak performance at **less than half the cost**.

## GPT‑5.6 vs Claude

| Aspect | GPT‑5.6 Sol | Claude Fable 5 | 
|---|---|---|
| **Intelligence Index** | 59 (max) | 60 (max) | 
| **Cost per task** | ~$1.04 | ~$3.12 (one‑third the cost) | 
| **Coding Agent Index** | 80 | 77.2 | 
| **Agents’ Last Exam** | 53.6 | 40.5 | 
| **Pricing (per 1M tokens)** | $5/$30 | $10/$50 | 

**Key insight**: Sol offers comparable intelligence to Claude Fable 5 at **one‑third the cost**, while outperforming it significantly in coding and agentic benchmarks.

## GPT‑5.6 vs Gemini

GPT‑5.6 Luna (max) matches or exceeds the intelligence of **Gemini 3.5 Flash** at a lower cost. Sol outperforms Gemini in coding and reasoning benchmarks, while offering competitive pricing.

## GPT‑5.6 vs Grok

GPT‑5.6 Sol ties **Grok 4.5** on the SWE‑Atlas‑QnA evaluation while being more cost‑efficient.

## Best Practices

### Choosing the Right Model

**Use Sol when**:

- The task involves complex reasoning, planning, or multi‑step tool orchestration
- You need the highest‑quality code, research, or analysis
- The cost of error is high, and extra intelligence justifies the premium

**Use Terra when**:

- You need GPT‑5.5‑level performance at half the cost
- You’re unsure which model to choose—it’s the rational default
- Most production workloads require balanced intelligence and cost

**Use Luna when**:

- You’re running high‑volume, cost‑sensitive workloads
- Tasks involve classification, extraction, routing, or well‑specified implementation
- You need to scale agentic workflows without breaking the budget

### Cost Optimization

1. 
**Use Luna for implementation** : After Sol plans a solution, use Luna to implement well‑specified changes, write tests, and evaluate results.
2. 
**Enable prompt caching** : OpenAI offers caching discounts for repeated inputs.
3. 
**Choose appropriate reasoning mode** : Not every task requires Max or Ultra reasoning. Use Standard mode for routine tasks.
4. 
**Monitor token usage** : Luna and Terra price reductions apply to Codex and ChatGPT Work subscriptions as well.

### Prompt Engineering

- Provide clear context and constraints
- Use structured outputs for reliable parsing
- Leverage tool calling for external data and actions
- For agentic workflows, break complex tasks into well‑defined steps

### Enterprise Deployment

- Use Sol for planning and resolving uncertainty
- Use Luna for implementing well‑specified changes
- Monitor usage and costs per task
- Implement guardrails for sensitive operations

## Frequently Asked Questions

### What is GPT‑5.6?

GPT‑5.6 is OpenAI’s flagship model generation, released in July 2026. It introduces three tiers: Sol (flagship), Terra (balanced), and Luna (cost‑optimized).

### Which model should I choose?

**Terra is the rational default** for most production workloads. Choose **Sol** for complex reasoning, coding, and agentic tasks. Choose **Luna** for high‑volume, cost‑sensitive workloads.

### What is Sol?

Sol is GPT‑5.6’s **flagship model**, designed for complex reasoning, coding, research, cybersecurity, and agentic workflows.

### What is Terra?

Terra is the **balanced model** for everyday work, delivering GPT‑5.5‑level performance at approximately half the cost.

### What is Luna?

Luna is the **fastest and most cost‑efficient model**, designed for high‑volume, cost‑sensitive workloads.

### Which is cheapest?

**Luna** is the cheapest at $0.20 per million input tokens and $1.20 per million output tokens.

### Which is best for coding?

**Sol** leads all models in the Coding Agent Index at 80 points. Terra (77) and Luna (75) also perform strongly.

### Is Terra better than GPT‑5.5?

Terra delivers **GPT‑5.5‑level performance at half the cost**.

### Is Luna capable of agentic tasks?

Yes. Luna can use tools, handle multi‑step workflows, and is now used for code review in Codex CLI—tasks that previously required more expensive models.

### What is Fast mode?

Fast mode is a new API option for Sol that delivers **up to 2.5× faster processing** at twice the price, replacing the previous Priority Processing offering.

### What is Ultra mode?

Ultra is Sol’s **highest performance setting**, coordinating multiple parallel agents (up to 16) to complete complex tasks faster.

## Related Guides

- **GPT‑5.6 Tutorial** – Getting started with the API
- **GPT‑5.6 Pricing Guide** – Detailed pricing and cost optimization
- **GPT‑5.6 API Guide** – API reference and integration
- **GPT‑5.6 Prompting Guide** – Prompt engineering best practices
- **GPT‑5.6 Sol vs Terra vs Luna** – Detailed model comparison
- **GPT‑5.6 Benchmarks** – Comprehensive benchmark analysis

## Related AI Tool Guides

## Conclusion

GPT‑5.6 represents a fundamental shift in how OpenAI delivers AI capability. The introduction of **Sol, Terra, and Luna** gives developers and enterprises the ability to match model intelligence to task complexity—paying only for the capability they need.

The performance improvements are substantial. Sol matches or exceeds Claude Fable 5’s intelligence at **one‑third the cost**, while Terra delivers GPT‑5.5 capability at **half the price**. Luna brings advanced agentic capabilities to high‑volume workloads at **$0.20 per million input tokens**—making AI automation practical at unprecedented scale.

**When to use each model**:

- **Sol** : Complex reasoning, coding, research, cybersecurity—when the extra capability justifies the premium
- **Terra** : The rational default for most production workloads—GPT‑5.5 performance at half the cost
- **Luna** : High‑volume, cost‑sensitive workloads—classification, extraction, routing, implementation, and monitoring

The price reductions announced on July 30, 2026—80% for Luna and 20% for Terra—reflect OpenAI’s commitment to passing efficiency gains to customers. As models continue to improve and costs decline, the range of practical AI applications will only expand.

Whether you’re a developer building agentic applications, an enterprise scaling AI across your organization, or a researcher pushing the boundaries of what’s possible, GPT‑5.6 offers a tier of intelligence for every need.
