---
id: collect-261121-veille-ia/gpt-openai/gpt-6-luna-benchmarks-pricing-specs
title: "GPT-6 Luna: Benchmarks, Pricing & Specs"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["luna", "task", "tokens", "effort", "index", "gpt-5", "gpt-6", "about"]
source: https://llmlearner.com/models/gpt-6-luna
source_anchor: ""
source_lines: [1, 28]
section: "GPT_OpenAI"
---
Price and value

### The cheapest per task in its class, if you can wait for max effort

**Our take:** GPT-6 Luna resets the price floor for capable small models. The trade-off is latency at max effort, not quality.

Standard pricing is $0.10 input and $0.50 output per million tokens, with cached reads at $0.01. Prompts above 272K input tokens cost $0.20 and $0.75. Batch and Flex are half price.

Artificial Analysis measures cost per Intelligence Index task, which includes reasoning tokens:

- Max effort: index **38.1** at**$0.068** per task
- High: 32.9 at $0.029
- Medium: 29.9 at $0.017

Against its predecessor, GPT-5.6 Luna Max scores 37.3 at $0.178 per task. GPT-6 Luna Max scores slightly higher and costs 62% less per task. Against GPT-5.4 mini at xhigh, the gap is larger: Luna Max scores 14 points higher (38.1 vs 24.1) while GPT-5.4 mini costs 6.6 times as much per task ($0.447). Users still on GPT-5.4 mini have little reason to stay, except for speed.

Speed is the cost of max effort. Luna generates about 129 tokens per second, while GPT-5.4 mini generates about 281. At max effort, Luna's median time to first answer token is about **108 seconds**; at high effort it is about 21 seconds. For chat interfaces and short agent steps, high or medium effort is usually the practical setting. Max effort suits batch work, where the 50% Batch discount also applies.

The low price does not extend to factual calibration. On AA-Omniscience, Luna Max gives a wrong answer instead of declining in 76.7% of the questions it does not answer correctly. That is better than GPT-5.6 Luna's 92.6% but well behind GLM 5.3 Flash's 27.6%. Use retrieval for factual work.

*Data read October 7, 2026.*

## 4 sources

- GPT-6 Luna model documentation1.05M context, 128K max output, text and image input, reasoning effort none to max; $0.10 input and $0.50 output per million tokens, with higher rates above 272K input tokens. Documentation2026-09-22
- GPT-6 Luna on Artificial AnalysisMax: Intelligence Index 38.1, $0.068 per index task, 50.0K output tokens per task, 129 tokens/s, about 108 s median to first answer token; High: 32.9, $0.029, about 21 s; Medium: 29.9, $0.017. Independent evaluation
- GPT-5.6 Luna on Artificial AnalysisMax: Intelligence Index 37.3, $0.178 per index task, list $0.20/$1.20; AA-Omniscience hallucination rate 92.6%. Independent evaluation
- GPT-5.4 mini on Artificial AnalysisXhigh: Intelligence Index 24.1, $0.447 per index task, list $0.75/$4.50 per million tokens, about 281 output tokens/s. Independent evaluation
