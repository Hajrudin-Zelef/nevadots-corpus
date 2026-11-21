---
id: collect-261121-veille-ia/gpt-openai/openai-jalapeao-benchmark-promises-faster-ai
title: "OpenAI JalapeÃ±o Benchmark Promises Faster AI"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["openai", "jalapeao", "inference", "benchmark", "chip", "said", "models", "faster"]
source: https://www.techrepublic.com/article/news-openai-jalapeno-ai-chip-benchmark/
source_anchor: ""
source_lines: [1, 34]
section: "GPT_OpenAI"
---
In June 2026, Broadcom CEO Hock Tan and President Charlie Kawwas delivered JalapeÃ±o to OpenAI CEO Sam Altman and President Greg Brockman. The custom AI inference chip was designed by OpenAI and co-developed with Broadcom.

OpenAI has now published its first benchmark results for JalapeÃ±o following tests using InferenceX, a public AI inference benchmark from SemiAnalysis. The company said the chip delivered 1.5 to 1.9 times greater peak performance per watt than the Nvidia Blackwell systems used for comparison.

The results were produced by OpenAI rather than an independent testing organization, and the company normalized them using each acceleratorâs published power rating.

## What JalapeÃ±o brings

JalapeÃ±o was designed to work across different models, not only OpenAIâs systems. The ASIC (Application-Specific Integrated Circuit) was tested with GPT-OSS 120B and two non-OpenAI models: DeepSeek R1 670B and Kimi K2.5 1T. OpenAI said the results demonstrate that the architecture is not restricted to its own models.

AI inference has several phases with different bottlenecks. During prefill, the system processes the userâs prompt, which is compute-intensive. During decode, it generates the response token by token and relies more heavily on memory bandwidth. Communication between cores and chips can add further latency and reduce an AI modelâs responsiveness.

JalapeÃ±o has been designed to combine both high batch throughput and real-time responsiveness. OpenAI says that it has built a flexible AI accelerator âthat can support changing model architectures, excel at both prefill and decode, and adapt as the balance between them changes, a defining feature of agentic workloads.â

One architectural feature behind this performance is a localized KV cache. OpenAI says model data can be explicitly placed and kept close to the required compute resources, reducing the time and power spent moving data during inference.

OpenAI says the resulting design can support high batch throughput and real-time responsiveness without making the same trade-off between throughput and latency found in some existing systems.

### More must-read AI coverage

- SS&C Intralinks DealCentre AI vs. Datasite: Which platform is built for the future of dealmaking?
- SS&C Intralinks FundCentre AI vs. Juniper Square: Which platform better supports modern private markets fund managers?
- Why Data, Not Models, Determines AI Success
- The Rise of the AI-Native Factory: How Physical AI Is Transforming Manufacturing

## What JalapeÃ±o means for enterprise users

Earlier this month, OpenAI launched a limited preview of an Ultrafast service tier that runs GPT-5.6 Sol at up to 14 times the speed of Standard processing. The service, powered by Cerebras, can generate up to 750 output tokens per second.

The Cerebras announcement came less than two months after OpenAI unveiled JalapeÃ±o. Together, the announcements show that OpenAI is pursuing a multi-vendor infrastructure strategy rather than immediately replacing Nvidia and Cerebras hardware with its own chip.

OpenAI has confirmed that JalapeÃ±o will complement rather than replace its partner-supplied accelerators. The company said: âMeeting growing demand for AI will require more compute from every available source. We will continue to widely deploy accelerators from NVIDIA and other partners for both training and inference workloads.â

For enterprise customers, JalapeÃ±o could eventually mean faster AI responses, greater service capacity and lower inference costs. OpenAI plans to begin deploying the chip within its own infrastructure by the end of 2026, but it has not said whether customers will be able to select the hardware directly or how the efficiency gains will affect API pricing. Those details will determine whether the benchmark produces a measurable advantage for businesses.
