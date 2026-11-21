---
id: collect-261121-veille-ia/grok-xai/xai-releases-grok-voice-3-0-a-speech-to-speech-model-tops-voice
title: "xAI releases Grok Voice 3.0 â speech-to-speech model tops voice..."
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: ["2026-10-14"]
keywords: ["voice", "model", "speech-to-speech", "grok", "live", "without", "support", "artificial"]
source: https://mlllm.io/articles/3546-xai-releases-grok-voice-3-0-speech-to-speech-model-tops-voice-agent-rankings/
source_anchor: ""
source_lines: [1, 31]
section: "Grok_xAI"
---
Company xAI released Grok Voice 3.0 on October 14, 2026 â a speech-to-speech model that listens and responds by voice without intermediate text and, according to the company, reacts in under a second. According to Artificial Analysis data from October 13, 2026, the model topped the Speech-to-Speech Index with a score of 86.9, outperforming OpenAI's GPT-Live-1 and Google's Gemini 3.8 Live, although it lagged behind its competitor in support call scenarios. Grok Voice 3.0 is already handling Starlink support calls and is available via the Voice Agent API at the previous price, but it does not yet support Russian.

### What happened

The release went live immediately with industrial availability: the model is open via the Voice Agent API without price changes â 8 cents per minute, or $4.80 per hour, and all agents on the grok-voice-latest alias were automatically migrated to the new version. Grok Voice 3.0 perceives the interlocutor's voice and responds directly by voice, without intermediate conversion to text, and xAI states that the model's response time is under one second. In the Speech-to-Speech Index published by aggregator Artificial Analysis on October 13, 2026, the model scored 86.9 points compared to 83.2 for OpenAI's GPT-Live-1 and 82.6 for Google's Gemini 3.8 Live. On the Full Duplex Bench, it showed 97.0%, but on tau-Voice, it received 71.6% compared to 74.5% for GPT-Live-1 â although its latency in this scenario was 40% lower. xAI also points to live deployment: the model works in Tesla vehicles and answers in Starlink support, where it handles about 20,000 calls a day and resolves approximately 70% of them without human involvement.

### Context

Voice agents are usually assembled from a chain: automatic speech recognition, a language model for generating responses, and voice synthesis. Each cascading stage adds latency and, along with it, erases the live features of conversation â pauses, interruptions, and topic changes mid-sentence. The speech-to-speech format eliminates intermediate conversions: the network listens to sound and immediately speaks, so its main advantages are low latency and natural behavior in dialogue. Direct competitors of Grok Voice 3.0 are OpenAI's GPT-Live-1 and Google's Gemini 3.8 Live, and independent comparisons of speech models are published by Artificial Analysis: its summary Speech-to-Speech Index and scenario tests set the standard structure for these comparisons, where Full Duplex Bench tests handling of pauses and interruptions, and tau-Voice simulates support calls. For this reason, live deployment metrics in Starlink mean more for evaluation than synthetic runs, because this is a count of real dialogues with real customers, not laboratory tests.

### Why this matters for the industry

According to Artificial Analysis, Grok Voice 3.0 became the first public speech-to-speech engine that simultaneously leads in dialogue quality and task cost â approximately $0.025 compared to about $0.030 for GPT-Live-1, which, combined with the previous API pricing, makes the model the default choice for English-speaking voice agents. The confirmed deployment in Starlink shows that voice agents have already moved from the demo stage to mass deployment in support and sales, and the benchmark of "about 70% of calls without a human" will push new companies to pilots. Technical teams should recalculate their latency budget: speech-to-speech removes cascading recognition and synthesis stages, old timings are no longer relevant, and interruptions and pauses are available without custom code for managing turn-taking. Within the next six months, counter-moves from OpenAI and Google regarding latency and price are likely, as is a possible expansion of xAI's language coverage and the first independent reproduction of Artificial Analysis figures, while scenario tests like tau-Voice will likely carry more weight than aggregated indices. If the level of about 70% autonomous resolutions is reproduced by other operators, voice will restructure the economics of support: humans will remain escalation handlers, and product differentiation will shift from "which engine" to domain knowledge, memory, and integrations.

### Why this matters for users

Those who have already built voice agents on the grok-voice-latest alias received the update automatically and without changing terms, but before continuing operation, it is worth checking for behavioral regressions in production, because the character of the model's responses has changed. New support pilots can be launched without changing contracts: $4.80 per hour of operation compared to an hour of a live operator makes trials cheap. UX capabilities are also immediately available: natural interruptions and pauses in dialogue work without custom code for managing turns, and the end user of an English-speaking service receives an almost instant response from an agent that behaves like a human interlocutor. It makes sense to run your own voice scenarios, as the summary index may mask weakness in a specific type of task. For Russian-speaking scenarios, nothing changes: the model does not support Russian, so it is currently inapplicable in Russian support and Russian-speaking bots.

### What is still unknown / limitations

All key comparative figures â 86.9, 83.2, 82.6, 97.0%, and 71.6% â come from a single source, the aggregator Artificial Analysis, as of October 13, 2026. This is data from a third-party measurer, not a peer-reviewed publication, and there is no independent reproduction by third parties yet, so it is premature to interpret the leadership "in both quality and price" as a completed scientific shift. The architecture, training data, and training methodology of the model are not disclosed in the sources, so the claim of speech-to-speech without intermediate text and the stated sub-second latency remain corporate claims by xAI that need to be verified on your own scenarios. Starlink operational statistics are also not a complete picture: the baseline quality of humans, the definition of a "resolved" call, and escalation statistics are not disclosed. Finally, a single aggregated number hides the spread across scenarios: the loss on tau-Voice shows that in complex support calls, the model is currently weaker than GPT-Live-1, and Russian is not supported in any case.

### Sources

- Introducing Grok Voice 3.0 (xAI News)
- Grok Voice 3.0 from SpaceXSI outscores OpenAI and Google on speech-to-speech index (SiliconANGLE)
- xAI Introduces Grok Voice 3.0 Speech-to-Speech Model for Voice Agents (Unite.AI)

### Author

Look at AI, editorial team
