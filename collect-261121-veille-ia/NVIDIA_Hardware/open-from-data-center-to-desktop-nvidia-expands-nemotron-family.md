---
id: collect-261121-veille-ia/nvidia-hardware/open-from-data-center-to-desktop-nvidia-expands-nemotron-family
title: "Open From Data Center to Desktop: NVIDIA Expands Nemotron Family"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: ["2026-10-16"]
keywords: ["nvidia", "open", "nemotron", "super", "bolt", "model", "data", "agentic"]
source: https://techbytes.app/posts/open-from-data-center-to-desktop-nvidia-expands-nemotron-family/
source_anchor: ""
source_lines: [1, 135]
section: "NVIDIA_Hardware"
---
# Open From Data Center to Desktop: NVIDIA Expands Nemotron Family

NVIDIA released Nemotron 3.5 Super VL, an open 120-billion-parameter model that it says is 5x faster than the next best open agentic model in its class.

By Dillip Chowdary â¢ Oct 16, 2026 â¢ Source: NVIDIA Blog

At a glance

- Released
- Super VL on October 15, 2026
- Parameters
- Super VL has 120 billion
- Speed
- 5x faster than next best open agentic model in class
- Users served
- Up to 8x more on NVIDIA Dynamo
- Local memory
- Bolt 74B-A9B 24GB, native 2-bit quantization
- Availability
- Super VL, Bolt 74B-A9B now; Bolt E4B Oct. 23

In this post

The section lengths are in range. I'll add the two missing words and emit the article.NVIDIA released Nemotron 3.5 Super VL on October 15, 2026, an open 120-billion-parameter model for the fastest task completion and the highest accuracy on specialized always-on agents, according to NVIDIA Blog's report.

Nemotron 3.5 Bolt 74B-A9B 24GB joins it for local agentic workloads, balancing intelligence and speed per GB. Nemotron 3.5 Bolt E4B 4GB is tuned for intelligence per GB on fast conversational agents.

Super VL and Bolt 74B-A9B are available now. Bolt E4B comes Oct. 23.

Teams running agents on NVIDIA machines get an open model per tier, from enterprise servers to edge devices and local hardware. Super VL and Bolt join Lightning, released in August for high-throughput work, and Ultra, for the hardest reasoning tasks. This piece covers sizes, speed, training, post-training work, install paths, and later dates.

## 01What shipped

Nemotron 3.5 Super VL, built with the Nemotron Coalition, targets leading accuracy on instruction following, tool use, multistep business workflows, and visual understanding. NVIDIA says that matches its largest reasoning model at roughly one-fifth the parameter count: frontier-level intelligence for high-volume agentic workflows.

It is the fastest open agentic model in its class, 5x faster than the next best, including workflows that span days or weeks. On the Artificial Analysis Intelligence Index leaderboard, throughput is up to 5x other models in its class.

Bolt 74B-A9B 24GB runs on NVIDIA RTX/DGX Spark, fitting in 24GB beside other applications via native 2-bit quantization. NVIDIA says it is "smart enough to get most jobs done, yet much faster than the smartest models." Local harnesses research the web, write, and debug code without the cloud.

It is #1 on PinchBench and #2 on BrowseComp among open models in its class. Bolt E4B 4GB is built for physical and conversational agents on GeForce RTX, Jetson, and RTX/DGX Spark, with natural multi-turn conversation for voice assistants, interactive characters, and embedded interfaces.

## 02What changed for builders

NVIDIA NeMo, Python pipelines, and NeMo Gym customize it. Hugging Face datasets cover agentic tool use, finance, instruction following, long context, fairness, and vision-language understanding. On NVIDIA Dynamo, NVIDIA states up to 8x more users and 85% lower latency.

Dozens of organizations post-train Super VL on their data, tools, policies, and workflows: Scale AI for Skydance's vendor credit finance agent and EY's M&A due diligence agent; AfterQuery for Hebbia credit agreement analysis; AI21 and Alice for cybersecurity attack resilience; Mercor on APEX-SWE for agents that plan, write, and debug code; ModelScope ms-swift for specialized multimodal applications; Ramp for financial document intelligence; Dream for sovereign AI-powered cybersecurity research and vulnerability analysis; Lila Sciences for scientific discovery and search; R1 for medical claims and revenue cycle management. TrainLoop used Nolla dermatology data. NVIDIA says Nolla is the first company approved for AI to write initial patient prescriptions.

| Stated comparison | Figure in this release | 
|---|---|
| Parameter count versus NVIDIA's largest reasoning model | Super VL, 120 billion parameters, about one-fifth the count | 
| Speed versus the next best open agentic model in class | 5x faster | 
| Throughput versus other models in class on the Artificial Analysis Intelligence Index leaderboard | Up to 5x faster | 
| Users served with the model running on NVIDIA Dynamo | Up to 8x more users | 
| Latency with the model running on NVIDIA Dynamo | 85% lower | 
| Local memory for Bolt 74B-A9B | 24GB with native 2-bit quantization | 
| Memory class for Bolt E4B | 4GB | 
| Tokens across pretraining and continued pretraining | About 26 trillion | 
| Post-training samples, plus reinforcement learning across dozens of environments | About 3.8 million | 
| PinchBench rank, open models in class | #1 for Bolt 74B-A9B 24GB | 
| BrowseComp rank, open models | #2 for Bolt 74B-A9B 24GB | 

## 03How to install or upgrade

Bolt 74B-A9B 24GB is on Hugging Face, with llama.cpp, Ollama, LM Studio, Unsloth, and vLLM. It is built for RTX Spark laptops arriving Oct. 16 and also runs on GeForce RTX PCs, DGX Spark, DGX Station, and RTX PRO workstations. Bolt E4B 4GB is dated Oct. 23.

Advertisement

Tech Pulse Daily

### Get tomorrow's pulse first

Join engineers who read Tech Pulse before stand-up. Free, weekday mornings.

The Super VL open-weights id is nvidia/NVIDIA-Nemotron-3.5-Super-VL-120B-A12B-NVFP4. Install or update vLLM, then serve that id to pin Super VL as the server default.

Bolt's Ollama and llama.cpp id is nvidia/NVIDIA-Nemotron-3.5-Bolt-74B-A9B-24GB-GGUF. The vLLM id is nvidia/NVIDIA-Nemotron-3.5-Bolt-74B-A9B-24GB.

This release publishes no slash command and no extra default flag. Post-training uses NVIDIA NeMo, Python pipelines, and ms-swift. The commands below now pin Super VL, the GGUF Bolt, and the vLLM Bolt.

`pip install vllm``vllm serve "nvidia/NVIDIA-Nemotron-3.5-Super-VL-120B-A12B-NVFP4"``ollama run hf.co/nvidia/NVIDIA-Nemotron-3.5-Bolt-74B-A9B-24GB-GGUF``vllm serve "nvidia/NVIDIA-Nemotron-3.5-Bolt-74B-A9B-24GB"``llama cli -hf nvidia/NVIDIA-Nemotron-3.5-Bolt-74B-A9B-24GB-GGUF`
## 04Gotchas and compatibility

The three sizes are not substitutes. Super VL is 120 billion parameters for high-volume multimodal agents.

Bolt 74B-A9B is the 24GB 2-bit model that runs beside other apps. Bolt E4B is the 4GB conversational and physical model, and it is not out yet.

Ultra still covers the hardest reasoning. Lightning, from August, still covers high-throughput work.

One-fifth the size compares Super VL with NVIDIA's largest reasoning model on those agentic tasks, not with every Ultra score. The 5x figures are in-class speed, including Artificial Analysis Intelligence Index throughput.

Super VL vision covers optical character recognition, tables, charts, and structured information in visual documents, web pages, PDFs, and images. It extracts text, reads graphs and charts, and returns insights.

One model carries context from document to data to decision. Teams run the weights where security and data-residency rules require and can adapt them.

Insurance claims can be scanned for fraud markers on repeat. Credit scores can be checked against a lender's own criteria.

NVIDIA says, "The model stays on premises. The data stays private."

## 05What to watch next

Atlassian is integrating Super VL for agentic search across Jira and Confluence. CrowdStrike is customizing it for agentic blue teaming in enterprise security.

Dassault SystÃ¨mes is using it for engineering design and simulation. Perplexity is using it for search and agentic tasks in Perplexity Search and Computer.

Salesforce is customizing Nemotron so agents digitize and automate complex government workflows in secure customer environments through Missionforce Operations. Synopsys software for semiconductor design verification also interoperates with that same open model today.

Bolt E4B 4GB comes Oct. 23 for GeForce RTX, Jetson, and RTX/DGX Spark and is not yet in today's downloads. NVIDIA NIM, a production-ready microservice with performance-optimized, validated configurations and enterprise-grade support, refreshes every two weeks with security patches and performance improvements. Hugging Face datasets and NeMo Gym support further Super VL post-training.

## Frequently asked questions

### What is Nemotron 3.5 Super VL?

It is NVIDIA's open 120-billion-parameter model released on October 15, 2026 for specialized always-on agents. NVIDIA says it matches the company's largest reasoning model at about one-fifth the parameter count and is 5x faster than the next best open agentic model in its class.

### When can you download Nemotron 3.5 Bolt E4B?

Bolt E4B 4GB comes Oct. 23. Super VL and Bolt 74B-A9B 24GB are available now. E4B is the 4GB model for fast conversational and physical agents on GeForce RTX, Jetson, and RTX/DGX Spark.

### How much memory does Nemotron 3.5 Bolt 74B-A9B need?

It uses native 2-bit quantization to fit in 24GB and run beside other applications on NVIDIA RTX/DGX Spark. NVIDIA also lists GeForce RTX PCs, DGX Spark, DGX Station, and RTX PRO workstations, and RTX Spark laptops arriving Oct. 16.

### How fast is Super VL, and what does Dynamo change?

NVIDIA calls Super VL the fastest open agentic model in its class, 5x faster than the next best, and up to 5x faster throughput than other models in its class on the Artificial Analysis Intelligence Index leaderboard. With the model running on NVIDIA Dynamo, NVIDIA says organizations can serve up to 8x more users with 85% lower latency.

### Where are the weights, and which id do you serve?

Super VL is on Hugging Face, Open Router, the ModelScope community, and NVIDIA NIM. The open-weights id is nvidia/NVIDIA-Nemotron-3.5-Super-VL-120B-A12B-NVFP4. Bolt 74B-A9B is on Hugging Face for llama.cpp, Ollama, LM Studio, Unsloth, and vLLM.

## Sources

## Related on Tech Bytes

Advertisement
