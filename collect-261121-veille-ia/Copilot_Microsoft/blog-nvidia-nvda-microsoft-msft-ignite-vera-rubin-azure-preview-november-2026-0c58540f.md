---
id: collect-261121-veille-ia/copilot-microsoft/blog-nvidia-nvda-microsoft-msft-ignite-vera-rubin-azure-preview-november-2026-0c58540f
title: "Blog nvidia nvda microsoft msft ignite vera rubin azure preview november 2026"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["nvidia", "microsoft", "azure", "rubin", "vera", "models", "preview", "ignite"]
source: ""
source_anchor: ""
source_lines: [1, 24]
section: "Copilot_Microsoft"
---
Key points
- Nvidia and Microsoft expand Azure's Vera Rubin preview
- Microsoft's confidential Rubin VMs still in development
- EY tests Nvidia's Nemotron models on Microsoft Foundry
Nvidia (NVDA) and Microsoft (MSFT) are expanding access to Azure virtual machines built on Nvidia's Vera Rubin NVL72 systems, which Nvidia said "are running in Azure data centers." Nvidia calls Azure's offering a preview, and its announcement names no virtual machine series, region, or price.
Nvidia also listed new open models on Microsoft's Foundry service, support for its chips in Microsoft's on-premises Azure Local product, and Microsoft's own use of Nvidia hardware for Bing. Dave Salvator of Nvidia published the details in a company blog post on Nov. 19, during Microsoft's Ignite conference in San Francisco.
Nvidia shares closed down 3.3% at $145.98 on Thursday. Microsoft closed at $498.30 on Thursday, after a $508.29 close the day before.
Vera Rubin on Azure is a preview
Microsoft says it became the first cloud to bring up a Vera Rubin NVL72 system for validation, in March. It "delivered the first production Vera Rubin systems in our datacenters" in August, Microsoft wrote in a Nov. 12 post ahead of Ignite.
Nvidia's post refers to "Azure's Vera Rubin preview." Neither company's post gives a date for general availability, the stage at which any customer can sign up.
Oracle (ORCL) has already announced it. On Oct. 27, Oracle and Nvidia announced the general availability of Vera Rubin NVL72 on Oracle Cloud Infrastructure. That release named no regions either.
Azure already runs Nvidia's current systems at large scale. In the latest MLPerf training round, Azure ran a DeepSeek-V3 test on 10,240 GPUs using Nvidia's GB200 NVL72 systems, which Nvidia called the largest scale in the round.
Microsoft's confidential Rubin preview starts with bare metal
Confidential computing is meant to protect a customer's data and AI models while they are being processed. On Nov. 17, Microsoft introduced a confidential computing "technology preview" for Vera Rubin NVL72 that starts with confidential bare-metal instances. Microsoft did not say which customers can use it.
Microsoft's Ignite demonstration used one compute node with four Rubin GPUs to run a 397-billion-parameter model. Confidential virtual machines on the platform, along with Foundry integration, "remain in development," Microsoft wrote in a separate Ignite post.
EY tested Nemotron models on Foundry
Foundry Managed Compute now offers Nvidia's Nemotron 3.5 open models. They include Super, for long-running agents, and Lightning, for fast responses and tool use. Nvidia listed two more, Bolt and Transcribe, as "coming soon."
EY used Programmed Training, a Foundry tool for tuning models, to adapt Nemotron models to classify corporate purchasing records. Accuracy on EY's evaluation dataset improved by 55%, and cost was 15 times lower than with proprietary frontier models, according to Nvidia. Nvidia did not say whether the 55% is a relative gain or percentage points, and it did not give the test conditions or name the models EY compared against.
Azure Local adds Nvidia chips for robots and factories
Azure Local runs Azure services on a customer's own hardware. Nvidia said Azure Local "will also support NVIDIA RTX PRO 6000 Blackwell GPUs from hardware partners," without naming the partners or a date. Azure Local is also expanding to Nvidia's Jetson Thor and Jetson Orin platforms for machine vision, robotics, and autonomous operations.
At Ignite, the companies are showing AI models running directly on Windows PCs built on Nvidia's RTX Spark chip. Microsoft's first such laptop, the Surface Laptop Ultra, went on sale Oct. 16 starting at $2,599.
Microsoft uses Nvidia chips for Bing and its own models
"Microsoft AI uses NVIDIA Dynamo to support reinforcement learning rollout inference during post-training," Nvidia wrote. Dynamo is Nvidia software that coordinates inference, the work of running a trained model, across a large number of GPUs. Microsoft also uses RTX PRO 6000 Blackwell GPUs for Bing search models, according to Nvidia.
Microsoft designs its own AI chips as well. In October, it said its Maia chips would become "the primary inference system for Microsoft products and services" starting in 2027. Scott Guthrie, Microsoft's executive vice president of Cloud and AI, said at the time, "We're big partners of NVIDIA and AMD, and we'll continue to be big partners of them."
