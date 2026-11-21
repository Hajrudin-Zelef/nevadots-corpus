---
id: collect-261121-veille-ia/nvidia-hardware/nvidia-has-unveiled-the-nemotron-3-5-lightning-a-high-speed-model-for-ai-agents-and-also-introduced-
title: "NVIDIA has unveiled the 'Nemotron 3.5 Lightning,' a high-speed model for AI agents, and also introduced 'NeMo Switchyard,' a tool for selecting the optimal AI model."
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: ["2026-08-11"]
keywords: ["model", "nvidia", "models", "nemotron", "agents", "also", "nemo", "switchyard"]
source: https://gigazine.net/gsc_news/en/20260812-nvidia-nemotron-3-5-lightning/
source_anchor: ""
source_lines: [1, 51]
section: "NVIDIA_Hardware"
---
# NVIDIA has unveiled the 'Nemotron 3.5 Lightning,' a high-speed model for AI agents, and also introduced 'NeMo Switchyard,' a tool for selecting the optimal AI model.


 On August 11, 2026, NVIDIA announced 'NVIDIA Nemotron 3.5 Lightning,' an open model for AI agents, and 'NVIDIA NeMo Switchyard,' an open-source library that automatically selects the most suitable AI model from multiple models for a given task.

 **NVIDIA Nemotron 3.5 Lightning and NeMo Switchyard Deliver Faster, Smarter, More Efficient Agentic AI | NVIDIA Blog** 

**Route AI Agents Across Models with NVIDIA NeMo Switchyard | NVIDIA Technical Blog
https://developer.nvidia.com/blog/route-ai-agent-workloads-across-models-with-nvidia-nemo-switchyard**

https://developer.nvidia.com/blog/route-ai-agent-workloads-across-models-with-nvidia-nemo-switchyard

 Introducing NVIDIA Nemotron 3.5 Lightning

 It is an open MoE model with 3 billion active parameters and 30 billion parameters, built to enable always-on agents to process large amounts of specialized tasks much faster.

 Compared to models of similar size, up to 4... pic.twitter.com/qR0JRsidhA



 When AI agents perform large-scale processing while manipulating external tools, using high-performance, large-scale AI models even for relatively simple tasks such as code verification and information classification increases processing time and costs. On the other hand, relying solely on small models may result in insufficient response quality when complex reasoning is required, such as in planning. NVIDIA explains that for AI agents running for extended periods, the concept of a 'system of models,' which combines multiple models with different roles, becomes crucial.

 The newly announced AI model, 'Nemotron 3.5 Lightning,' is designed to rapidly process a large volume of specialized tasks. It is a Mixture of Experts (MoE) model with a total of 30 billion parameters, of which 3 billion are active parameters used during processing.

 The Nemotron 3.5 Lightning is positioned as an extension of the Nemotron 3 family, following the Nemotron 3 Nano, and is designed for specialized tasks that AI agents repeatedly handle, such as code reviews, tool operation, security alert monitoring, and billing inquiries. According to NVIDIA, it offers up to four times the output speed compared to models in its class, and can complete tasks by AI agents 30% faster. It can also be further trained to match the data and business procedures held by the organization.

 


 Hugging Face offers two versions: 

**an NVFP4 version**that prioritizes inference speed and processing efficiency. Both support context lengths up to 1 million tokens and also support Japanese. The NVFP4 version is said to run on a single DGX Spark or H100 processor. Hugging Face also provides quantization models for use with Ollama and LM Studio.

However, even with lightweight and fast models, if humans have to manually decide 'which model should handle which process' each time, building an AI agent becomes complicated. To automate model selection, the open-source library 'NeMo Switchyard' for model routing has also been released.

NeMo Switchyard analyzes requests received from AI agents and automatically distributes them to the most suitable model from among multiple models registered by developers. It can combine not only NVIDIA models but also open models and proprietary models. The distribution method can also be adjusted according to conditions such as whether to prioritize quality or to reduce response speed and cost.


 In addition to selecting a model based on the request content, other methods are also available, such as switching models based on the stage the AI agent is in during the task, or initially having a low-cost model handle the processing and then handing it over to a high-performance model if persistent problems are detected.

 NVIDIA's internal benchmarks report that using NeMo Switchyard to combine Claude Opus 4.8 with NVIDIA Nemotron 3.5 Lightning, Gemma 3 26B, and Qwen 3.6 35B resulted in significant cost reductions while maintaining a task completion rate comparable to Claude Opus 4.8 alone.

 


 As of the time of writing, Nemotron 3.5 Lightning is available from Hugging Face, ModelScope, OpenRouter, and NVIDIA's build.nvidia.com, and LM Studio and Ollama have also announced support. The source code for NeMo Switchyard is publicly available on GitHub, and NVIDIA plans to expand Switchyard to partner platforms in the future.

 Furthermore, NVIDIA emphasizes that as AI systems increasingly combine multiple models, model routing—selecting the most suitable model for each task while balancing efficiency, quality, and cost—will become crucial.


**Related Posts:**
