---
id: collect-261121-veille-ia/unifi-ubiquiti/microsoft-foundry-on-windows-blog
title: "Microsoft Foundry on Windows Blog"
domain: unifi-ubiquiti
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["windows", "microsoft", "local", "foundry", "blog", "compiler", "aion", "instruct"]
source: https://devblogs.microsoft.com/foundry-on-windows/
source_anchor: ""
source_lines: [1, 13]
section: "Copilot_Microsoft"
---
# Microsoft Foundry on Windows Blog

News and guidance on the unified platform for local AI development on Windows

Additional Contributors: Angela Serrano Brummett, Nick Eubanks, Pawan Tahiliani With the release of Windows ML 2.9.42 Experimental, we are expanding on the Windows ML Runtime API, our new Windows-native inferencing path announced in October. This release is the first step in supporting a new MLIR-based backend, the DirectX Compute Graph Compiler (DX CGC) announced at GDC 2026, that we’ve designed and have been developing in conjunction with our partners at AMD, Intel, NVIDIA, and Qualcomm Technologies, Inc. Included alongside this release are new docs and samples to guide you through using these new API...

Today we're sharing the transition plan from Phi Silica to Aion 1.0 Instruct, Microsoft's latest on-device language model for Windows. Aion 1.0 Instruct will begin rolling out to CP+ devices through the Windows Insider Program in early December, with full Windows Retail availability targeted for late January. We're also expanding Aion 1.0 Instruct to supported GPUs, bringing local language model capabilities to a broader range of Windows devices. Microsoft Aion 1.0 Instruct is designed to power fast, responsive language model experiences directly on your device. This transition gives Windows and Microsoft app...

Additional Contributors: Justin Stoecker, Sheil Kumar Overview One of the biggest challenges in building AI/ML platform stacks for mobile, client, or server is creating a machine learning (ML) compiler that is both scalable yet performant. A common approach is to build specialized compilers for specific AI hardware, frameworks, or workload types. For example, independent hardware vendors (IHVs) provide their own frameworks to deploy models on their hardware, and PyTorch includes torch.compile as a framework-specific compiler. Other ML compiler efforts include MLIR (Multi-Level Intermediate Representation), whic...

Powered by NVIDIA RTX Spark, we are bringing the promise of unmetered local intelligence to increasingly demanding coding workloads—giving developers greater freedom to experiment, delegate, and iterate on their own hardware, without per-token cloud charges. Highly capable mixture-of-experts (MoE) models are making this opportunity tangible. These models tackle complex coding tasks spanning reasoning, implementation, testing, and debugging, while advances in efficiency are bringing more of that capability onto local devices. With 128 GB of unified memory, RTX Spark PCs enables us to ask a crucial question: how...

Today we are highlighting improvements across a few key open-source projects many of you already use, as well as updates to Windows ML. To empower developers, we must support the broad range of tools for experimentation and exploration across inference and training, in addition to our production grade native inference stack. Available today, Windows adds experimental llama.cpp support to Windows ML so you can run GGUF models locally through new task-specific APIs. Windows ML is the unified, high-performance local AI inferencing framework for Windows. Our experimental Windows-native Runtime API is now in preview ...
