---
id: collect-261121-veille-ia/windows-os/what-are-windows-ai-apis
title: "What are Windows AI APIs?"
domain: windows-os
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["model", "windows", "apis", "devices", "availability", "first", "download", "supported"]
source: https://learn.microsoft.com/bs-latn-ba/windows/ai/apis/
source_anchor: ""
source_lines: [1, 126]
section: "Windows_OS"
---
Windows AI APIs are the ready-to-use model APIs in Microsoft Foundry on Windows. They let you add supported features without finding, packaging, or optimizing your own machine learning model. These APIs run models locally on supported Windows 11 devices; the required Windows build, SDK release channel, and NPU, GPU, or CPU capabilities vary by API.

## Supported hardware

Windows AI APIs are expanding beyond Copilot+ PCs to support a broader range of hardware. The following table shows the current hardware support for each API.

Note

On a Copilot+ PC, supported APIs generally use the **NPU** by default, but selection and fallback behavior vary by API. Video Super Resolution selects the device before the quality level, once at creation, using a fixed NPU, then GPU, then CPU priority limited to the scenario's supported devices. It doesn't benchmark devices or automatically switch devices or models during frame processing. See VSR device selection and fallback and the individual API pages for SDK availability and hardware requirements.

 
Note

GPU support for Phi Silica is available on NVIDIA GeForce RTX 30 series and newer (6+ GB vRAM) and AMD Radeon RX 9060 series and newer (6+ GB vRAM). Phi Silica GPU inference requires **Developer Mode** to be enabled (**Settings** > **System** > **For developers**) and the latest GPU driver installed directly from the manufacturer (see Phi Silica â GPU driver requirements). For CPU processing, Video Super Resolution recommends at least **16 threads**, while Speech Recognition recommends at least **4 physical cores**. Both recommend a **3 GHz or higher base clock** and **32 MB or more of L3 cache**; see the individual API pages for details and a runtime check.

 
Video Super Resolution has separate GPU requirements, including native FP16 shader operations and a driver-reported total lane count of at least 512.

### Model availability

The way the underlying AI model reaches a device depends on the API:

- **Phi Silica** â On Copilot+ PCs the model is**preinstalled** on the NPU. On GPU and CPU devices the model is**not** preinstalled â it is downloaded on demand the first time your app calls`EnsureReadyAsync` . Downloads can be several GB and run in the background through Windows Update. End users can remove or reinstall the model at**Settings** >**System** >**AI Components** . Apps should check`GetReadyState` first and show a consent dialog before triggering the download. See Phi Silica â Model availability and download for the recommended UX pattern.
- **Image Generation** â Runs on the NPU only, but the model is**not preinstalled** because of its install size. It is downloaded on demand the first time your app calls`EnsureReadyAsync` , and users can later remove it at**Settings** >**System** >**AI Components** . Apps should check`GetReadyState` first and show a consent dialog before triggering the download. See Image Generation â Model availability and download for the recommended UX pattern.
- **Video Super Resolution** â Apps use the SDK's deployment model for required runtime and model assets. Readiness calls are optional capability checks, not model downloads or device preparation, and don't guarantee successful creation or frame processing. See Create a VideoScaler session.
- **Speech Recognition** â On Copilot+ PCs the model is**preinstalled** on the NPU. On CPU-only devices the model is**not** preinstalled â it is downloaded on demand the first time your app calls`EnsureReadyAsync` , and users can later remove it at**Settings** >**System** >**AI Components** . Apps should check`GetReadyState` first and show a consent dialog before triggering the download on CPU. See Speech Recognition â Model availability and download for the recommended UX pattern.

See the Windows AI APIs with WinUI sample app for how to use Microsoft Foundry on Windows with WinUI.

### SDK and Windows availability

Don't use a historical feature-introduction version as a recommendation for the package you install today. Select a supported release from Windows App SDK release channels, then check the prerequisites for each API and overload you use. That page links to the current release notes and servicing policy.

Some features require a preview or experimental SDK and a Windows Insider build; see the release-channel differences. A newer stable package doesn't automatically include experimental APIs, grant restricted-feature access, or make an unsupported device compatible. Check runtime availability and model readiness before exposing a feature in your UI.

Note

Video Super Resolution's creation-options overload and input color properties belong to the **experimental contract version 2** surface, not the baseline API availability listed above. Confirm the exposed members and Windows prerequisites for your selected SDK release. See Prerequisites and API availability.

 
## Build your first app with Windows AI APIs

Tip

To improve accessibility and readability, this page displays still images by default. In some cases, you can click an image to see an animated version.

 
To build your first Windows app with these APIs, review the prerequisites and follow the example code in Get started building an app with Windows AI APIs.

You can then follow focused tutorials for the Phi Silica walkthrough, imaging walkthrough, and text recognition walkthrough.

## Try the APIs and models on your PC

AI Dev Gallery is a demo appâavailable from the Microsoft Storeâthat lets you quickly download, try out, and use Windows AI APIs and models.

In AI Dev Gallery, select the **Windows AI APIs tab** menu item, then select the *Phi Silica* sample. If the model is already available on your device, then that sample will run straight away. Otherwise, select **Request model** to download the model. Once downloaded, that sample will be activated. Learn more about the AI Dev Gallery in What is the AI Dev Gallery?.

## Overview of available APIs

Here are some ready-to-use capabilities that you can add to your Windows app:

### Phi Silica

Important

**Phi Silica is being replaced by Aion Instruct**, a new on-device model. Aion Instruct begins rolling out to Windows Insider Preview devices in December 2026 and to retail devices in January 2027, at which point Phi Silica will be removed. See Get started with Phi Silica for transition details and timeline.

 
Similar to Large Language Models (LLM), Phi Silica is a Small Language Model (SLM) developed by Microsoft Research to perform language-processing tasks on a local device (see Get started with Phi Silica). Phi Silica is designed for Windows devices with a Neural Processing Unit (NPU) or a supported GPU, allowing text generation and conversation features to run in a high performance, hardware-accelerated way directly on the device. *Phi Silica is not available in China.*


### Text recognition

The text recognition APIs recognize text in images and convert scanned documents, PDF files, and camera images into editable and searchable data on the device. See Get started with text recognition.


### Imaging

Scale and sharpen images (Image Super Resolution), identify objects within an image (Image Object Extractor), generate natural-language descriptions of images (Image Description), and remove objects from images (Object Erase). See the imaging overview.

#### Image Super Resolution

The Image Super Resolution APIs enable image sharpening and scaling.


Also see Image Super Resolution.

The Image Object Extractor APIs enable identifying objects within images.


Also see Image Object Extractor.

#### Image Description

The Image Description APIs describes images in natural language.

Note

Image Description features are not available in China.

 

Also see Image Description

#### Object Erase

You can use the Object Erase APIs to remove objects from images.


Also see Object Erase

### Planned features

The following capabilities are planned but not yet available for use:

- **Live Translation (Not yet supported)** . Help everyone using Windowsâincluding those who are deaf or hard of hearingâbetter understand audio by viewing captions of spoken content (even when the audio content is in a language that's different from the system's preferred language).

## Content moderation

Learn how content is moderated by the Windows AI APIs, and how you can adjust sensitivity filters. See Content safety moderation with the Windows AI APIs.

When adding model-powered features, review Responsible Generative AI Development on Windows.

## Additional resources

## See also
