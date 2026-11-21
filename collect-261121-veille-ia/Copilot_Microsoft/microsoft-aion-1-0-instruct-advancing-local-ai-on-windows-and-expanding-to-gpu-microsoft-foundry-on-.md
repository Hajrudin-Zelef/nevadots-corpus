---
id: collect-261121-veille-ia/copilot-microsoft/microsoft-aion-1-0-instruct-advancing-local-ai-on-windows-and-expanding-to-gpu-microsoft-foundry-on-
title: "Microsoft Aion 1.0 Instruct: Advancing local AI on Windows and expanding to GPU - Microsoft Foundry on Windows Blog"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["aion", "instruct", "windows", "microsoft", "model", "experiences", "local", "language"]
source: https://devblogs.microsoft.com/foundry-on-windows/aion-instruct-local-ai-windows-gpu/
source_anchor: ""
source_lines: [1, 77]
section: "Copilot_Microsoft"
---
Today we’re sharing the transition plan from Phi Silica to Aion 1.0 Instruct, Microsoft’s latest on-device language model for Windows. Aion 1.0 Instruct will begin rolling out to CP+ devices through the Windows Insider Program in early December, with full Windows Retail availability targeted for late January. We’re also expanding Aion 1.0 Instruct to supported GPUs, bringing local language model capabilities to a broader range of Windows devices.

Microsoft Aion 1.0 Instruct is designed to power fast, responsive language model experiences directly on your device. This transition gives Windows and Microsoft applications a newer, more efficient model foundation while preserving the familiar experiences users rely on today.

You can also tune in next Tuesday to THR888 at Ignite 2026 to learn more about Aion 1.0 Instruct (recording will be available).

## Microsoft Aion 1.0 Instruct: A new foundation for local AI on Windows

Local AI enables applications to deliver responsive experiences while keeping processing on device. Aion 1.0 Instruct builds on our investment in this area with improvements designed to help language model features run efficiently alongside the applications and workflows you already use on Windows.

| **Attribute** | **Phi Silica** | **Aion 1.0 Instruct** | 
| Parameters | 3.8B | 2.3B | 
| Input context length limit [tokens] | 3.5K | 32K | 

A technical report with more details on Aion 1.0 Instruct is coming soon to the Microsoft Command Line blog.

As Aion 1.0 Instruct becomes the foundation for more Windows experiences, users can expect:

- More responsive language model experiences

- An increased input context length limit (from 3.5K to 32K tokens)

- Improved performance and use of device resources

- Continued innovation across Windows and Microsoft applications

- A transition designed to preserve familiar workflows and capabilities

For most app developers, no action will be required. The change will happen behind the scenes as supported experiences move to Aion 1.0 Instruct. Action will be required for apps that rely on highly custom prompting (ex. Specific system prompts) and custom LoRAs, as well as apps that have custom model output parsing due to our new watermarking implementation. We’ve added invisible watermarking to Aion 1.0 Instruct to fulfill Text Provenance requirements in specific regions. Additionally, apps that are heavily reliant on Phi Silica’s specific behavior or output strings should be validated for quality. Since Aion 1.0 Instruct is a new model, output strings and model behavior are different. To get the most out of Aion 1.0 Instruct, take a look at our prompt guidance.

## Expanding Aion Instruct to GPU

Like Phi Silica today, we’re also expanding Aion 1.0 Instruct to supported GPUs, including popular dGPUs like NVIDIA’s RTX-series and AMD’s RDNA4 generation. This greatly increases the number of Windows devices that can benefit from Aion Instruct and gives developers greater flexibility when building language model experiences that run on-device.

Our goal is to provide a consistent local AI foundation across supported Windows hardware. In many scenarios, applications can take advantage of available device capabilities without developers requiring separate end user experiences for each type of hardware.

Hardware support will expand in stages as configurations meet our readiness and quality standards.

## What this means for developers

For developers building local language model experiences on Windows, Microsoft Aion 1.0 Instruct provides a newer foundation while continuing to work with familiar Windows AI capabilities and APIs.

We encourage developers to begin evaluating their applications with Aion 1.0 Instruct as it becomes available. In particular, developers should validate model-dependent behavior, review any application-specific customizations, and confirm that key scenarios continue to deliver the intended experience. Aion Instruct can be validated end to end now with the package available at aka.ms/tryaion. Additionally, the Windows Insider Program Beta External build targeted for release on 12/4 will allow full, end to end validation.

Because language models can differ in how they interpret and produce content, testing is an important part of any model transition–even when the underlying APIs remain familiar.

We’ll provide additional documentation and migration guidance as availability expands.

## Transition timeline

The transition will roll out in stages:

1. October 2026: A standalone package is available at aka.ms/tryaion for developers to begin testing Aion 1.0 Instruct and preparing their applications and customizations.
2. December 2026: Aion 1.0 Instruct is planned to begin rolling out to Windows Insider Preview devices on 12/4.
3. January 2027: Aion 1.0 Instruct is planned to roll out to retail devices, completing the transition from Phi Silica.

Developers can find the latest transition information in the Windows AI documentation:

## A transition designed for continuity

Phi Silica helped establish a foundation for local language-model experiences on Windows. Microsoft Aion 1.0 Instruct represents the next step in that journey, improving the platform while enabling it to reach more devices. With Aion 1.0 Instruct, we’ll also be removing key adoption barriers like the need for an LAF token (removal targeted for January).

The staged transition gives developers time to evaluate their experiences and prepare for broader adoption. Most existing AI-powered features will continue to work (with the exception of custom LoRAs) as they move to the newer model foundation.

## What’s next?

This is the beginning of the next phase of local AI on Windows. We’ll continue expanding Aion Instruct across supported hardware and working with developers to improve the experience.

As the rollout progresses, we’ll share updated guidance, documentation, and ways for developers to try Aion 1.0 Instruct and provide feedback.

We’re excited to see what you build with Aion Instruct on Windows. Be sure to check out THR888: Build On-Device AI Experiences w/ Windows AI APIs at Ignite 2026 to learn more about Aion Instruct (recording will be available).

To share feedback, create an Issue in the WinAppSDK repo: Issues · microsoft/WindowsAppSDK

## 0 comments

Be the first to start the discussion.
