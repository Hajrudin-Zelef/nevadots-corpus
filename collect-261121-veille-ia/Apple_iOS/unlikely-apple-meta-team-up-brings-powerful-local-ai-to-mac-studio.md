---
id: collect-261121-veille-ia/apple-ios/unlikely-apple-meta-team-up-brings-powerful-local-ai-to-mac-studio
title: "Unlikely Apple & Meta team-up brings powerful & local AI to Mac Studio"
domain: apple-ios
role: reference
task: article
actors: ["Apple"]
dates: []
keywords: ["model", "apple", "muse", "meta", "spark", "open", "memory", "studio"]
source: https://appleinsider.com/articles/26/10/15/unlikely-apple-meta-team-up-brings-powerful-local-ai-to-mac-studio
source_anchor: ""
source_lines: [1, 35]
section: "Apple_iOS"
---
Meta worked with Apple to bring its massive Muse Spark Open AI model to M5 Ultra Mac Studios, letting developers run advanced AI locally instead of relying on cloud servers.

Announced on October 15, Muse Spark Open is an open-weight AI model designed for coding, software engineering, and tasks that require multiple steps. Meta developed the model with 816 billion parameters, although only 42 billion are active during processing.

Running the model requires at least two M5 Ultra Mac Studios with 512GB of unified memory each, or four machines configured with 256GB each, using hardware introduced in Apple's recent Mac Studio refresh. The four-machine configuration is available now, but Apple's 512GB option isn't expected to ship until late October.

## How Mac Studio runs Meta's massive AI model

The collaboration takes advantage of Apple's unified memory architecture, which allows the CPU and GPU to access the same memory pool. The M5 Ultra provides up to 512GB of unified memory and 1.2TB/s of memory bandwidth.

Even that much memory isn't enough for Muse Spark Open on a single machine. According to Apple's research, the model's compressed weights occupy approximately 493GB before accounting for the additional memory required during processing.

The open-source MLX framework handles the Meta model's execution by distributing the workload across multiple Macs. The machines communicate using Thunderbolt 5 and remote direct memory access, or RDMA, which allows them to exchange data with less CPU involvement.

Apple updated MLX to support Muse Spark Open, improving communication between connected Macs and adding support for the M5 Ultra's GPU Neural Accelerators. The framework can split the model across multiple computers or distribute calculations between them to speed up processing.

In Apple's benchmarks, four M5 Ultra Mac Studios generated more than 36 tokens per second with an 8,192-token context. Actual performance depends on the workload and context length, but the results show the model can deliver responses at a practical speed.

Meta demonstrated the model handling several complex workflows entirely on local hardware. Using its Muse Code development tool, the model modified Ice Cubes, an open-source Mastodon client for iOS, fixing bugs, adding features, and completing a build.

Another demonstration showed Muse Spark Open controlling DaVinci Resolve to review video footage, identify useful segments, and crop them into vertical clips. The demonstrations show the model completing specific tasks but don't establish how reliably Muse Spark Open performs across other workflows.

## Powerful local AI comes at a substantial cost

The biggest obstacle is the hardware requirement. The M5 Ultra Mac Studio starts at $5,499, but the memory configurations needed for Muse Spark Open push a multi-machine setup into the tens of thousands of dollars.

The hardware costs make Muse Spark Open better suited to professional development and research than occasional use. Developers with occasional needs may prefer hosted services, while frequent users can avoid cloud usage fees.

The collaboration is also notable given the companies' history. Apple and Meta have clashed over privacy, advertising restrictions, and platform control, but their interests align when it comes to running advanced AI models on Apple's hardware.

For Apple, Muse Spark Open demonstrates the Mac Studio's AI capabilities. For Meta, the partnership offers another way to run its models without relying on cloud infrastructure or dedicated GPU servers.

Muse Spark Open is available through Hugging Face, with support for additional hardware from AMD, Intel, and Nvidia. Developers without the necessary equipment can also access the model through hosted API providers.

Open-weight availability doesn't mean the model is unrestricted. Meta distributes Muse Spark Open under its own license, which permits many uses without charge but includes commercial limitations.
