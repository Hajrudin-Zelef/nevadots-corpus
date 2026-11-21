---
id: collect-261121-veille-ia/grok-xai/grok-voice-3-0-launches-xai-s-1-voice-agent-model
title: "Grok Voice 3.0 Launches: xAI's #1 Voice Agent Model"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: ["2026-10-13"]
keywords: ["voice", "grok", "agent", "model", "audio", "agents", "speech", "calls"]
source: https://saeedai.com/grok-voice-3-0-launch/
source_anchor: ""
source_lines: [1, 47]
section: "Grok_xAI"
---
xAI has launched **Grok Voice 3.0**, a new speech-to-speech AI model for phone agents and voice assistants. Announced on October 14, 2026, it is available now through xAI’s Voice Agent API at the same price as its predecessor — $0.08 per minute. xAI says Grok Voice 3.0 now ranks first on Artificial Analysis’s Speech-to-Speech Index, its first time topping an independent voice benchmark.

November 2026 update: Grok Voice 3.0 is still xAI’s current flagship voice model. No newer version has been announced, and the $0.08 per minute price and 86.9 benchmark score still stand.

## What Was Announced?

On October 14, 2026, xAI unveiled Grok Voice 3.0 as its new flagship voice model, replacing Grok Voice 2.0 in the Voice Agent API. Unlike older pipelines that convert speech to text and back, Grok Voice 3.0 listens to spoken audio and replies in spoken audio directly — making phone conversations with an AI feel far less delayed.

The headline change is in-app tool calling. Developers can now give the voice agent web search, X search, remote MCP servers, and custom functions it can run mid-conversation. Reasoning can be set to “high” for harder tasks or “none” for the fastest responses, and server-side voice activity detection handles interruptions automatically.

Existing developers were moved automatically: any agent pointing at the `grok-voice-latest` alias now runs 3.0.

## Key Details

xAI backed the launch with independent numbers from Artificial Analysis, dated October 13, 2026. Grok Voice 3.0 scores **86.9 on the Speech-to-Speech Index** — ahead of GPT-Live-1 (Astra, Medium) at 83.2, Gemini 3.8 Live (Extended Thinking) at 82.6, and its own predecessor at 81.3. Component scores include 98.5% on Big Bench Audio speech reasoning, a 1070 Elo in the Speech Agent Arena, a 94.8% task success rate, and 97.0% on Full Duplex Bench — the test measuring how naturally a model handles pauses, turn-taking, and interruptions. On tau-Voice, built from simulated support calls with accents and background noise, it scored 71.6%, which xAI describes as comparable to GPT-Live-1 at 40% lower latency.

Cost is a deliberate part of the pitch. The API price stays at **$0.08 per minute** (about $4.80 per hour), unchanged from 2.0. xAI also cited a measured $0.025 per task on the Big Bench Audio run, below $0.030 for GPT-Live-1 and $0.055 for GPT-Realtime-2.1. On ServiceNow’s EVA-Bench for enterprise voice agents, Grok Voice 3.0 reached 74% accuracy, reportedly pairing top accuracy with natural, responsive conversation.

The technical package includes 28 built-in voices plus custom voice cloning from up to 120 seconds of reference audio, 20 locales with automatic language detection, and WebSocket, WebRTC, and SIP transports for phone integration.

## Why It Matters

Voice agents are moving from demos into real call centers, and this launch shows why. xAI says Grok Voice already powers millions of conversations a day — Tesla vehicles handle calendar checks, email replies, and reservations hands-free, while Starlink call centers field around 20,000 customer calls a day, with roughly 70% resolving without a human and about one in four sales calls ending in a purchase.

Those are vendor-reported figures, not audited numbers, but they show the shape of the bet: a voice agent that answers calls around the clock for under five dollars an hour costs far less than a part-time receptionist — provided it performs as advertised. As we noted when Suno Speech launched, voice AI is fast becoming the quiet engine of customer support, and this release puts xAI directly against OpenAI and Google on that front.

## Availability and What’s Next

Grok Voice 3.0 is live now in xAI’s Voice Agent API, with a free trial in the xAI console. Agents on the `grok-voice-latest` alias were switched over automatically, so most teams change nothing to get the upgrade.

What comes next is scale: with the benchmark crown, tool calling, and unchanged pricing, xAI is betting businesses will trust voice agents with more complex, higher-stakes phone calls. The real test is how 3.0 performs at volume — and whether OpenAI and Google answer with price cuts or upgrades of their own. For more of the voice AI landscape, see our ElevenLabs v4, Suno Speech, and VoiceStudio coverage.

## FAQs

**How much does Grok Voice 3.0 cost?**

It costs $0.08 per minute through xAI’s Voice Agent API — about $4.80 per hour — unchanged from Grok Voice 2.0. Developers can try it free in the xAI console before paying for usage. A free trial is available in the xAI console, so you can test a few calls before paying anything.

**Is Grok Voice 3.0 better than OpenAI’s voice models?**

On Artificial Analysis’s independent Speech-to-Speech Index, Grok Voice 3.0 scores 86.9, ahead of GPT-Live-1 (Astra, Medium) at 83.2, with a lower reported per-task cost. These are vendor-cited results; test before switching providers. Scores change as new models arrive, so check the current index before choosing a provider.

**How do I upgrade from Grok Voice 2.0 to 3.0?**

If your agent uses the grok-voice-latest alias, you were migrated to 3.0 automatically with no code changes. To pin the release, use model ID grok-voice-3.0; 2.0 is now deprecated at the same price. If you pinned the old model name directly, update the model string to grok-voice-latest or 3.0.

Follow Saeed AI for daily AI news — launches, updates, and breakthroughs every day.
