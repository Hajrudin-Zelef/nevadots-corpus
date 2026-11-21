---
id: collect-261121-veille-ia/copilot-microsoft/microsoft-sets-out-aion-instruct-a-2-3b-on-device-model-to-replace-phi-silica
title: "Microsoft sets out Aion Instruct, a 2.3B on-device model, to replace Phi Silica"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-11-13"]
keywords: ["aion", "instruct", "says", "model", "windows", "microsoft", "on-device", "silica"]
source: https://aiintelglobal.com/news/2026-11-13-microsoft-aion-instruct
source_anchor: ""
source_lines: [1, 13]
section: "Copilot_Microsoft"
---
# Microsoft sets out Aion Instruct, a 2.3B on-device model, to replace Phi Silica

Microsoft's Windows developer blog is dated 13 November 2026. It says the company is sharing the transition plan from Phi Silica to Aion Instruct, which it calls its latest on-device language model for Windows. It says Aion Instruct will begin rolling out to CP+ devices through the Windows Insider Program in late November, with full Windows Retail availability targeted for late January. It says Aion Instruct is also expanding to supported GPUs.

Source: Microsoft (opens in a new tab), 13 Nov 2026

Microsoft's Windows developer blog is dated 13 November 2026. It says the company is sharing the transition plan from Phi Silica to Aion Instruct, which it calls its latest on-device language model for Windows. It says Aion Instruct will begin rolling out to CP+ devices through the Windows Insider Program in late November, with full Windows Retail availability targeted for late January. It says Aion Instruct is also expanding to supported GPUs.

A table on the post lists Phi Silica at 3.8B parameters with an input context length limit of 3.5K tokens, and Aion Instruct at 2.3B parameters with a limit of 32K tokens. The post says a technical report with more detail is coming soon on the Microsoft Command Line blog. It says invisible watermarking was added to Aion Instruct to fulfil Text Provenance requirements in specific regions. It says apps that rely on highly custom prompting, custom LoRAs, or custom parsing of model output will need work, because Aion Instruct is a new model and its output strings and behaviour differ.

The post says GPU support includes NVIDIA's RTX series and AMD's RDNA4 generation, and that hardware support will expand in stages. The timeline on the page is a standalone package in October 2026 at aka.ms/tryaion, a planned rollout to Windows Insider Preview devices on 11/20, and a planned rollout to retail devices in January 2027. It says removal of the need for an LAF token is targeted for January. The post does not state a token price.

**What it means for business:** this is an on-device model, not a cloud API with a published price. The post says custom prompts, custom LoRAs and custom output parsing need a check, and that watermarking was added for text-provenance rules in specific regions. Retail availability is described as targeted for late January, not as a date already reached. This describes the post. It is not advice on a deployment.
