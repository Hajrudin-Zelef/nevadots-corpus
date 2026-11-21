---
id: collect-261121-veille-ia/gemini-google/gemini-4-carbon-api-1m-context-deep-reasoning-kie-ai
title: "Gemini 4 Carbon API: 1M Context, Deep Reasoning | Kie.ai"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["gemini", "carbon", "google", "thinking", "context", "reasoning", "output", "tokens"]
source: https://kie.ai/gemini-4-carbon
source_anchor: ""
source_lines: [1, 62]
section: "Gemini_Google"
---
Automatically append previous messages to maintain multi-turn context. May increase token usage.

## README

Complete guide to using Gemini 4 Carbon

# Gemini 4 Carbon API for Deep Reasoning and Coding

## Key Capabilities of Gemini 4 Carbon

### Unified 1M-Token Context Window

### Function Calling and Google Search

### Coding With Build-and-Verify Loops

### Multimodal Knowledge Work

### Three Thinking Levels

## Gemini 4 Carbon Specifications

Model specifications for Gemini 4 Carbon from Google's documentation.

| Specification | Gemini 4 Carbon | 
|---|---|
| Developer |  | 
| Based on | Gemini 4 Argon | 
| Input | Text, image, video, audio, PDF | 
| Output | Text | 
| Context window | 1,048,576 tokens, shared by input, thinking and output | 
| Output limit | 65,536 tokens per response, thinking and answer combined | 
| Thinking levels | Low, medium (default), high; reasoning_effort or generationConfig.thinkingConfig.thinkingLevel on Kie.ai | 
| Sampling parameters | Custom temperature, top_p and top_k values are ignored | 
| Not supported | Image generation, audio generation, Live API | 
| Knowledge cutoff | March 2026 | 

## Gemini 4 Carbon Benchmark Results

Results reported by Google on the Gemini 4 Carbon model card, as of October 2026.

| Benchmark | Gemini 4 Carbon | Gemini 4 Argon | 
|---|---|---|
| AA Index v5.0 | 59 | 50 | 
| Vals Index v2.2 | 70.4% | 69.2% | 
| SWE-sweep | 9.6% | 4.3% | 
| FrontierSWE 2 | 61.2% | 55.0% | 
| Terminal-Bench 4.0 (AA) | 65.2% | 57.1% | 
| Agents' Last Exam v1.1 | 77.4% | 69.0% | 
| OSWorld-2.1 (offline subset) | 81.5% | 74.7% | 
| AutomationBench (Zapier) | 53.9% | 51.3% | 
| Chartography | 72.5% | 71.6% | 
| GraphWalks (256k to 1M, BFS, F1) | 96.2% | 84.2% | 
| RiemannBench | 80.0% | 76.0% | 
| Terminal-Bench-Science 0.1 (AA) | 64.8% | 52.9% | 

## How to Work With Gemini 4 Carbon

1. 1### Step 1: Use Carbon for the Hardest Tasks Use Carbon for complex root-cause debugging, architectural design, dense multimodal synthesis and scientific proofs. Gemini 4 Argon gives faster responses and lower token spend per turn on interactive coding and day-to-day feature work. For fast everyday coding and chat, Google also offers Gemini 4 Hydrogen, which is adapted from Carbon.
2. 2### Step 2: Choose the Lowest Thinking Level That Meets Your Bar Gemini 4 Carbon has three thinking levels. Low uses the fewest tokens, medium is the default for most tasks, and high suits hard math and science and the most difficult code. On Kie.ai, set reasoning_effort (OpenAI-compatible) or generationConfig.thinkingConfig.thinkingLevel (native). Google advises running evaluations at all three and keeping the lowest level that meets the quality bar.
3. 3### Step 3: Leave Room in the Output Limit Thinking tokens and the answer share one output budget, so leave room for the final answer on hard prompts. Google recommends that apps with a small output limit raise it to at least 32,768 tokens; the Kie.ai API docs list no output-limit parameter for this model.
4. 4### Step 4: Plan for Long Requests Google notes that hard prompts can take several minutes and recommends background execution or client timeouts of at least 15 minutes. In the OpenAI-compatible API on Kie.ai, set stream to true to receive the answer as server-sent events.
