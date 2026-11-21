---
id: collect-261121-veille-ia/copilot-microsoft/xcode-27-3-adds-github-copilot-to-its-built-in-coding-assistant-9to5mac
title: "Xcode 27.3 adds GitHub Copilot to its built-in coding assistant - 9to5Mac"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-11-03"]
keywords: ["xcode", "coding", "github", "copilot", "device", "beta", "assistant", "previews"]
source: https://9to5mac.com/2026/11/03/xcode-27-3-adds-github-copilot-to-its-built-in-coding-assistant/
source_anchor: ""
source_lines: [1, 100]
section: "Copilot_Microsoft"
---
Apple released Xcode 27.3 beta 1 today with multiple new features, chief among them an expansion of its built-in coding assistant to support GitHub Copilot. Here are the details.

## GitHub Copilot comes to Xcode

Since Xcode 26.3, developers have been able to integrate AI coding agents directly into Xcode, to help with tasks ranging from writing boilerplate code to updating entire projects based on their documentation, file structure, and more.

Apple initially introduced ChatGPT integration in Xcode 26, followed by Anthropic’s Claude. Then, in Xcode 26.3, the company expanded both integrations with agentic coding capabilities through OpenAI’s Codex and Anthropic’s Claude Agent.

With Xcode 27, Apple added support for Google Gemini, alongside a redesigned interface, interactive planning, and multi-turn conversations.

Now, with Xcode 27.3, Apple is expanding the list of supported AI coding tools once again, this time adding GitHub Copilot.

The Xcode 27.3 beta 1 release notes include the following under the Coding Intelligence section:

### Coding Intelligence

#### New Features


- GitHub Copilot is now available in the coding assistant. (185837709)

As with the other agentic integrations, this is particularly good news for developers whose companies have standardized on GitHub Copilot, as they can now use their company’s preferred AI coding tool directly within Xcode.

Here are the full release notes for Xcode 27.3 beta 1:

## Overview

Xcode 27.3 beta includes Swift 6.4 and SDKs for iOS 27.3, iPadOS 27.3, tvOS 27.3, watchOS 27.3, macOS 27.3, and visionOS 27.3. Xcode 27.3 beta supports on-device debugging in iOS 17 and later, tvOS 17 and later, watchOS 10 and later, and visionOS. Xcode 27.3 beta requires a Mac running macOS Tahoe 26.6 or later.

See Xcode Support to learn more about compatible platforms and deployment targets.

### Coding Intelligence

#### New Features


- GitHub Copilot is now available in the coding assistant. (185837709)
### Device Hub

#### New Features


- You can now drag items onto a device in Device Hub. Drop an app (
`.app` or `.ipa`) to install it, or drop a link to open it on the device. On iOS simulators, drop any other file to save it with the Files app. (179246120) (FB23037189)
#### Resolved Issues


- Device Hub now supports using option-drag to simulate two-finger touch gestures for iPhones and iPads. (169537162)
- Device Hub now accepts keyboard and mouse input for OS simulators earlier than iOS 18.0, tvOS 18.0, watchOS 11.0, and visionOS 2.0. (190652440)
#### Known Issues


- Device Hub support for CarPlay requires devices to be physically connected via USB cable (not over the network). iOS Simulator is not supported for CarPlay. (179494052) (FB23101450)
- When using Device Hub via Sidecar or a Mac with Touch, only two touches can be sent to the target device. Three or more touches are interpreted by macOS as a system gesture. (185140491)
**Workaround:** Turn off touch gestures in Settings under Touch Display.
### Previews

#### New Features


- RenderPreview MCP tool snapshot filenames and metadata now include the applied overrides. (176531615)
- RenderPreview MCP tool now supports interactive delayed snapshots for macOS previews. (181244878)
#### Resolved Issues


- A clearer error is now shown when the sandbox blocks a library from loading in a preview. (147269145)
- macOS widget previews now fail with a clear error. (178635433)
- Fixed a crash that could occur when building a preview after the workspace closes. (183679367)
- Fixed excessive processing time for source files that contain long operator chains. (184132564)
- Fixed an issue where previews refreshed unnecessarily when an Xcode project or workspace had not changed. (187278576) (FB24753484)
- Previews now refresh when asset catalog contents change on disk. (187456273) (FB24785391)
- Fixed an issue where the RenderPreview MCP tool could report no previews before they finished loading. (188424749)
- Fixed an issue where the RenderPreview MCP tool could render a different preview than the one requested. (188817121)
- Fixed a crash that could occur when changing code while in the Resizable Canvas mode. (189463909)
### Simulator

#### Resolved Issues


- Fixed: Simulator runtimes might be inaccessible to users who are in the
`daemon` group. (User accounts should not be added to the `daemon` group.) (190649396)
### Swift Testing

#### New Features


- Xcode now supports writing UI tests with Swift Testing and
`XCUIAutomation`. (95593221)
### UI Automation

#### New Features


`XCUIAutomation` supports setting the hinge angle on iPhone Duo with a new `XCUIHinge` class. (187479559)

Xcode 27.3 is expected to be released in December, alongside macOS 27.3 and its counterparts.

To learn more about what’s coming to Xcode 27.3, follow this link.

## Comments
