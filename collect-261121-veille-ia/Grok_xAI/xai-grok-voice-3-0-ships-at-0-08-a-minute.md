---
id: collect-261121-veille-ia/grok-xai/xai-grok-voice-3-0-ships-at-0-08-a-minute
title: "xAI Grok Voice 3.0 ships at $0.08 a minute"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: ["2026-10-14"]
keywords: ["voice", "grok", "model", "minute", "speech-to-speech", "same", "call", "index"]
source: https://dev.to/techaiwire/xai-grok-voice-30-ships-at-008-a-minute-1p9l
source_anchor: ""
source_lines: [1, 44]
section: "Grok_xAI"
---
The AI company xAI released Grok Voice 3.0 on October 14, 2026, a speech-to-speech model for building voice agents. It costs $0.08 a minute in the Voice Agent API, the same as the model it replaces. The company says it now ranks first on Artificial Analysis's Speech-to-Speech Index. It matters because voice agents are moving into call centers, and a better model at an unchanged price lowers the cost of each call.

A speech-to-speech model listens to audio and answers in audio directly. It skips the older chain of converting speech to text, running a text model and turning the answer back into speech. That shortcut is what makes a phone conversation with an AI feel less delayed.

Grok Voice 3.0 runs in xAI's Voice Agent API and streams audio and text both ways over a WebSocket, a connection that stays open for live data, Blockchain.News reports. It is now xAI's flagship voice model, and Grok Voice 2.0 is deprecated.

The price stays at $0.08 a minute, about $4.80 an hour, the same as Grok Voice 2.0. AI Weekly, citing SiliconANGLE, puts rival voice models at $0.10 to $0.18 a minute.

Moving over takes no work for many developers. Apps that point at `grok-voice-latest` were switched to 3.0 automatically, according to Blockchain.News. The endpoint, session settings and events are unchanged, and client-side apps still use short-lived tokens.

xAI's headline number is a score of 86.9 on the Speech-to-Speech Index from Artificial Analysis, a group that benchmarks AI models:

| Model | Speech-to-Speech Index | 
|---|---|
| Grok Voice 3.0 | 86.9 | 
| GPT-Live-1 (OpenAI) | 83.2 | 
| Gemini 3.8 Live (Google) | 82.6 | 
| Grok Voice 2.0 | 81.3 | 

xAI's announcement lists more results, using Artificial Analysis data as of October 13. On the Big Bench Audio reasoning test, Grok Voice 3.0 scores 98.5% at an average cost of $0.025 per task, against 90.1% and $0.030 for GPT-Live-1. The table is not a clean sweep, though. On τ-Voice, a test of voice agents completing tasks, GPT-Live-1 scores 74.5% to Grok Voice 3.0's 71.6%.

xAI also reports 97% on Full Duplex Bench, which tests talking and listening at the same time, and 74% on EVA-Bench, an open benchmark from ServiceNow. Nitin Aggarwal, ServiceNow's Senior Director of Product Management for Agentic Evals and RAI, said the model "sets a new Pareto frontier on EVA-Bench." The company also claims latency under one second, 40% lower than GPT-Live-1 at comparable accuracy on τ-Voice.

xAI says the model handles real traffic. Starlink's call centers take about 20,000 calls a day in several languages, and about 70% are resolved without a human. One in four sales calls ends in a purchase before the caller hangs up, xAI says. Tesla uses it for hands-free tasks in its cars.

AI Weekly notes that no independent audit backs the Starlink figures.

If you already use Grok Voice through `grok-voice-latest`, you are on 3.0 now. Re-run your own call recordings and test scripts, since a new model can change tone, timing and how it handles interruptions, even when the API is the same.

If you pin a specific model version, plan the move to 3.0 before 2.0 is switched off, since it is now deprecated.

For teams comparing voice providers, cost per minute is the easy number. Run the same 50 real calls through each candidate and measure resolution rate and handoffs to humans, the metrics xAI itself highlights. Benchmark scores rarely match a specific call script.

Treat the deployment numbers as marketing until someone outside xAI confirms them. A 70% self-service rate is a strong claim, and it depends heavily on what kinds of calls a center receives.

*This article was first published on Tech AI Wire.*

Deutsch · 日本語 · Français · Español · Português

- Introducing Grok Voice 3.0 - xAI
- xAI Grok Voice 3.0 Ships at $0.08/Min, Tops Index at 86.9 - AI Weekly
- Grok Voice 3.0 launches with unchanged API pricing and auto-migration - Blockchain.News

## Top comments (0)
