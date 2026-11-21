---
id: collect-261121-veille-ia/copilot-microsoft/microsoft-s-aion-instruct-goes-ga-in-edge-replacing-phi-4-mini-freshsynapse
title: "Microsoft's Aion Instruct Goes GA in Edge, Replacing Phi-4-mini | FreshSynapse"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-11-20"]
keywords: ["microsoft", "aion", "edge", "freshsynapse", "instruct", "model", "on-device", "replacing"]
source: https://freshsynapse.com/articles/microsoft-aion-instruct-edge-ga-2026-11-20/
source_anchor: ""
source_lines: [1, 15]
section: "Copilot_Microsoft"
---
*Photo: Hippopx (public domain). Illustrative image.*

**What happened.** Microsoft has made Aion Instruct generally available as an open-weights model on Hugging Face, and as Microsoft Edge’s built-in small language model, replacing Phi-4-mini. The announcement, published on the Microsoft Edge blog on 19 November, is one of the seven-model MAI wave from Ignite — see our report on the seven in-house MAI models — but this is the on-device piece of that story, and it is the one that lands in users’ browsers.

**Smaller, faster, more widely available.** At 2.3 billion parameters, Microsoft says Aion is smaller and faster than Phi while delivering comparable quality on common web tasks such as text synthesis, summarisation, instruction following and structured outputs — and because it is smaller, it runs on many more devices. Edge downloads Aion only when a site or feature first needs it, and only on devices that can run it well. Users stay in control: on-device AI can be turned off in Edge settings, which removes the model and stops future downloads. Aion is also coming to Windows later this year, laying the groundwork for on-device AI across Copilot+ PCs.

**Two APIs ride along.** The same post opens an origin trial for the Prompt API, so developers can call Aion from a live site in Edge rather than only in Canary and Dev builds — the point is to test on-device AI features with a real audience and feed the results into web-standardisation discussions. And there is a developer preview of an experimental Decisions API, powered by Aion: it answers yes/no, multiple-choice or scoring questions about a piece of text in a single pass, returning probabilities and a confidence value without generating any text — useful for sorting user-generated content, routing, or scoring reviews in real time. Google has been exploring the same idea with Gemini Decision 1.

**Why it matters.** The browser is becoming the most widely distributed AI runtime on earth, and whoever owns the built-in model owns the default. By putting a small, open, on-device model into Edge — and giving websites a standard way to call it — Microsoft is betting that a large share of everyday AI work never needs to leave the device at all.

## Sources

- Microsoft Edge Blog — “Hybrid intelligence on the Web: Introducing Aion Instruct and Prompt API in Microsoft Edge” (19 November 2026): https://blogs.windows.com/msedgedev/2026/11/19/hybrid-intelligence-on-the-web-introducing-aion-instruct-and-prompt-api-in-microsoft-edge/
- FreshSynapse — “Microsoft Launches Seven In-House MAI Models at Ignite”: https://freshsynapse.com/articles/microsoft-mai-seven-models-ignite-2026-11-18/
- FreshSynapse — “Google’s Gemini Decision 1 Brings Decisions to the API”: https://freshsynapse.com/articles/google-gemini-decision-1-decisions-api-2026-11-12/
