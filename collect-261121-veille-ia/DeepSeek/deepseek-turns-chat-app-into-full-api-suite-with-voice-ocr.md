---
id: collect-261121-veille-ia/deepseek/deepseek-turns-chat-app-into-full-api-suite-with-voice-ocr
title: "DeepSeek Turns Chat App into Full API Suite with Voice, OCR"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: []
keywords: ["deepseek", "chat", "voice", "search", "developers", "suite", "full", "model"]
source: https://emergent.sh/news/deepseek-turns-chat-app-into-full-api-suite
source_anchor: ""
source_lines: [1, 23]
section: "DeepSeek"
---
# DeepSeek Turns Chat App into Full API Suite with Voice, OCR

DeepSeek on November 13, 2026 transformed its chat application into a modular API suite, exposing five tools that were previously locked inside its user interface. Developers can now access web search, file input, URL reading, safety checks, and speech-to-text/text-to-speech as standalone services, all powered by the new V4.1-Flash-1017 model across Messages, Responses, and Chat Completions endpoints.

## API Capabilities and Pricing Structure

The newly launched APIs cover the full spectrum of multimodal interaction. Web search and URL fetch services cost $0.75 per 1,000 requests, while optical character recognition (OCR) for documents runs $0.225 per 1,000 pages. Automatic speech recognition (ASR) and text-to-speech (TTS) are priced at a 50% launch discount through the end of the year. Developers can call these tools individually or integrate them into chat workflows where the model decides when to invoke search, read linked pages, or process uploaded PDFs, Office files, and images.

## Developer Integration Options

DeepSeek offers two implementation paths. Developers can enable tools in existing API requests, letting the model automatically trigger search queries or document analysis during reasoning. DeepSeek's servers handle the execution and combine results with the conversation context. Alternatively, developers can call individual interfaces for search, webpage retrieval, OCR, speech recognition, or text-to-speech when they need precise control over when and how each tool runs. This dual approach eliminates the infrastructure work developers would otherwise build to orchestrate tool calls and merge outputs.

## Open-Source Chat Demo Release

Alongside the API launch, DeepSeek released an open-source chat demo that supports multi-turn conversations, voice input, spoken replies, and imports of existing DeepSeek chat histories. Developers can fork the demo to build custom chat products that leverage the full API suite without starting from scratch. The demo showcases how to combine voice I/O with document processing and web search in a single interface, providing a reference implementation for production applications.

## Release Date and Availability

The API suite was officially launched on November 13, 2026, with immediate availability across all DeepSeek developer accounts. The V4.1-Flash-1017 model (referred to as deepseek-flash in API calls) powers every endpoint, ensuring consistent performance across search, voice, and document workflows. DeepSeek had previously offered limited access to Search and Fetch APIs, but this release marks the first time all five tools are publicly available with production pricing.

## What This Means for Developers

DeepSeek's API expansion removes the integration overhead that has historically slowed multimodal application development. By bundling search, OCR, and voice into a single model endpoint, developers can ship features like voice-driven document analysis or web-grounded chat without managing separate vendors for each capability. The open-source demo accelerates time to market, while the 50% speech discount makes real-time voice applications economically viable for startups and enterprise teams alike.
