---
id: collect-261121-veille-ia/gemini-google/google-unveils-tpu-8tx-and-tpu-8i-max-with-2x-compute-and-enhanced-security-for-ai-agents
title: "Google Unveils TPU 8tx And TPU 8i Max With 2X Compute And Enhanced Security For AI Agents"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["google", "information", "compute", "computing", "agents", "tpu", "memory", "within"]
source: https://pulse2.com/google-unveils-tpu-8tx-and-tpu-8i-max-with-2x-compute-and-enhanced-security-for-ai-agents/
source_anchor: ""
source_lines: [1, 89]
section: "Gemini_Google"
---
The new systems address two major challenges for enterprises deploying advanced AI models: protecting sensitive information during processing and providing enough compute and memory for applications that require extremely large context windows.

TPU 8tx focuses on confidential computing and low-latency inference, while TPU 8i Max delivers higher compute density and memory bandwidth for demanding AI workloads.

The launch follows Google’s introduction of TPU 8t and TPU 8i in April 2026.

TPU 8t was developed for large-scale AI model training, while TPU 8i was optimized for real-time reasoning and inference.

According to Google, those earlier systems deliver up to 2.7 times better training performance and 80% better inference price-performance, respectively, compared with the company’s previous TPU generation.

With TPU 8tx and TPU 8i Max, Google is expanding its hardware portfolio to address the requirements of enterprises and AI developers deploying increasingly sophisticated autonomous systems.

The company said its TPU business now serves nearly 100 customers, including frontier AI laboratories, financial trading firms, retailers, universities, and other organizations.

Google has also expanded how customers access its technology, supporting TPU deployments through Google Cloud and, more recently, directly within customer-owned data centers.

TPU 8tx is designed for organizations that need to process sensitive information while running proprietary AI models and autonomous agents.

The system targets regulated industries and government organizations that require additional protections for confidential data.

Traditional computing environments typically encrypt information while it is stored or transmitted but must decrypt it when the information is actively processed.

Google’s new TPU 8tx introduces hardware-level confidential computing capabilities intended to protect sensitive information and model weights even during active execution.

The system uses the same underlying silicon as TPU 8t and is paired with fifth-generation AMD EPYC processors.

It also incorporates Google’s low-latency Boardfly networking technology, originally developed to support real-time reasoning workloads.

According to Google, TPU 8tx can deliver up to twice the latency reduction of TPU 8t while adding almost no performance penalty from its security capabilities.

The architecture supports both confidential inference and smaller-scale model training, allowing organizations to execute multiple AI development activities on a single system.

Enterprises can use TPU 8tx to fine-tune models with proprietary datasets, distill larger models into smaller systems, and train autonomous agents through reinforcement learning.

By combining these activities within one computing environment, Google aims to reduce the need to move sensitive information between separate computing clusters.

The second new system, TPU 8i Max, focuses on the computing and memory requirements associated with large-context AI applications.

As autonomous agents analyze extensive software repositories, long video recordings, and large collections of documents, memory bandwidth can become a major performance constraint.

TPU 8i Max addresses this limitation by combining two low-latency TPU 8i chips within a single package.

The design provides twice the compute capacity, twice the SRAM, and twice the high-bandwidth memory bandwidth of the standard TPU 8i system.

This increased capacity is intended to support AI applications that require substantial amounts of information to remain accessible during processing.

Google said the configuration also doubles the amount of silicon within an existing rack footprint, increasing the computing resources that data centers can deploy within their available space.

The design supports greater compute density per rack while addressing the growing importance of power efficiency and infrastructure utilization.

Beyond the hardware announcements, Google introduced several software capabilities intended to make TPUs easier to deploy and manage.

The company recently open-sourced TorchTPU, enabling developers to run PyTorch applications and Hugging Face models natively on TPU hardware without rewriting their existing code.

Google also introduced TPU backends for open-source inference engines, including vLLM and SGLang.

A new open-source TPU agent suite, currently in preview, integrates with AI coding environments such as Claude Code, Cursor, and Google Antigravity.

The tools are designed to help developers configure TPU environments, identify performance bottlenecks, and optimize custom computing kernels through natural-language instructions.

Google also introduced XLA Serving Engine, an open-source system that allows developers to train models using Python and deploy them through a low-latency C++ runtime across TPUs, GPUs, and CPUs.

Another addition, Agent Compute Engine, is designed to improve infrastructure efficiency when autonomous agents temporarily stop working while awaiting external tools, database responses, or human feedback.

The software automatically suspends idle agent sessions and transfers working memory away from the accelerator, reducing unnecessary compute usage.

Google is also open-sourcing TPU Sync, a model-state management layer designed to coordinate memory and storage resources across TPU clusters.

The technology combines high-bandwidth memory, system memory, shared storage, and data center networking into a coordinated memory hierarchy.

According to Google, TPU Sync can reduce key-value cache transfer latency and time to first token by more than 90%, helping accelerate AI inference involving large context windows.

Google also announced broader availability of Cloud Pathways through Google Kubernetes Engine.

The technology allows organizations to coordinate large TPU deployments through a single controller, supporting AI pretraining, reinforcement learning, and distributed inference.

Meanwhile, Google Kubernetes Engine dynamic slicing is now generally available, enabling organizations to reconfigure TPU hardware resources dynamically.

Google said the capability can reduce workload startup times by up to five times and accelerate fault recovery by up to 4.5 times.

These developments reflect Google’s broader strategy of combining specialized AI accelerators, networking systems, memory architecture, and open-source software into an integrated computing platform.

The company is also using AI internally to accelerate hardware development, including Google DeepMind’s AlphaChip technology and Gemini-assisted chip design.

Google said these tools can help reduce certain chip layout development tasks from weeks to hours.

With four eighth-generation TPU systems now available in its portfolio, Google is positioning its custom silicon to meet the growing demands of AI training, real-time reasoning, confidential computing, and autonomous agents.

The new hardware and software capabilities are intended to help enterprises deploy increasingly complex AI applications while improving performance, security, and infrastructure efficiency.
