---
id: collect-261121-veille-ia/modeles-open/fresh-weights-aion-instruct-lands-nov
title: "Fresh Weights: Aion Instruct Lands - Nov"
domain: modeles-open
role: reference
task: article
actors: []
dates: []
keywords: ["aion", "instruct", "model", "edge", "microsoft", "on-device", "weights", "small"]
source: https://medium.com/@shaaravraghu/fresh-weights-aion-instruct-lands-nov-c86c354d4055
source_anchor: ""
source_lines: [1, 29]
section: "Modeles_Open"
---
Microsoft’s small language model for the web just became generally available. On November 19, the Microsoft Edge team announced that Aion Instruct is now an open-weights model on Hugging Face and the new built-in small language model in Microsoft Edge, replacing Phi-4-mini.

## What launched

Aion Instruct is Microsoft’s 2.3B-parameter small language model for on-device web AI. It had been in developer preview as Aion-1.0-Instruct; the November 19 announcement says Microsoft incorporated early-adopter feedback and refined the model’s training before taking it to general availability.

Three updates shipped together: Aion Instruct’s general availability as open weights on Hugging Face and as Edge’s built-in model, an origin trial for the Prompt API so developers can call on-device AI from live sites, and a developer preview of an experimental Decisions API in Edge Canary and Dev channels.

## Official claims

**Smaller and broader than Phi:** at 2.3B parameters, Microsoft says Aion is smaller, faster, and available on many more devices than Phi, while delivering comparable quality on common web tasks: text synthesis and generation, summarization, instruction following, and structured outputs.

**Built into Edge, under user control:** Edge downloads Aion Instruct only when a site or feature first needs it, and only on devices that can run it well. Users can turn off on-device AI in Edge settings, which removes the model and stops future downloads.

**Windows next:** Microsoft says Aion Instruct is coming to Windows later this year, laying the foundation for richer on-device AI experiences across Windows and Copilot+ PCs.

**Decisions API:** powered by Aion Instruct, it answers yes/no, multiple-choice, or scoring questions about text in a single pass without text generation, returning probabilities and a confidence value. Microsoft’s examples: understanding user intent, sorting and routing user-generated content, and scoring or ranking items in real time, all in a few lines of client-side JavaScript.

## Pricing and availability

Aion Instruct is open-weights and free to download from Hugging Face today. In Edge it ships as the browser’s built-in model, so sites can use it through the Prompt API origin trial without standing up their own inference. The Decisions API is a developer preview in Edge Canary and Dev channels, and Microsoft cautions both the API and its implementation may change.

## Why it matters

On-device AI is quietly becoming a default web platform feature rather than a demo. For small businesses and indie developers, a free, browser-resident model means summarization, classification, and routing features with no API bill and no user data leaving the device, with a cloud fallback only when more power is needed. For the wider ecosystem, a Microsoft-backed open-weights model replacing Phi inside a major browser is a signal that the small-model layer is consolidating around open weights.

Official announcement: Hybrid intelligence on the Web: Introducing Aion Instruct and Prompt API in Microsoft Edge

Today’s AI news: Daily AI Caffeine (No BS)
