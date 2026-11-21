---
id: collect-261121-veille-ia/gemini-google/google-s-gemini-4-neon-is-out-on-vertex-ai-at-3-15-per-million-tokens
title: "Google's Gemini 4 Neon is out on Vertex AI at $3 / $15 per million tokens"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["google", "gemini", "neon", "million", "model", "tokens", "input", "vertex"]
source: https://artificialwatch.com/wire/google-ships-gemini-4-neon-on-vertex-ai-at-3-15
source_anchor: ""
source_lines: [1, 31]
section: "Gemini_Google"
---
✳ the wire · analysis

# Google's Gemini 4 Neon is out on Vertex AI at $3 / $15 per million tokens

An email when every new Google model answers on a public API, about 15 minutes after members, plus every frontier launch · no card

UPDATE, Nov 18, 20:53 UTC: Gemini 4 Neon is now generally available on the Gemini API. Google's release notes list gemini-4-neon on the paid tier at $3 per million input tokens and $15 per million output, with four thinking levels and the same 1M-token context; it is not on the free tier.

Gemini 4 Neon is live in Google Cloud's Vertex AI Model Garden as gemini-4-neon, at $3 per million input tokens and $15 per million output. Google's model page says "The Gemini 4 Neon model is now available on Gemini platform." ArtificialWatch's radar had no hunt armed for it, so we sent the launch alert by hand.

THE PRICE

From Google Cloud's public SKU catalog: $3 per million input tokens, $15 per million output, $0.15 per million cached input. Batch and Flex input is $1.50, and Priority input is $5.40. That puts Neon between Gemini 4 Hydrogen ($1.50 / $7.50) and Gemini 4 Carbon's $4 / $20 list price. Carbon's introductory $2 / $10 runs until Nov 30, so until then Neon costs more per token than Carbon.

WHAT GOOGLE SAYS IT IS

Google calls Neon its fast reasoning model. Its model page says Neon builds on Gemini 4 Hydrogen with deeper reasoning and brings quality close to Gemini 4 Carbon on coding, reasoning and agentic tasks, at Flash-tier speed and cost. That is Google's description, not an independent result.

THE SPECS

A 1M-token context window shared by input, thinking and output; 64K output tokens per segment, up to 1M with Long Decode. Four thinking levels: Low, Medium, High (the default) and XHigh. It takes audio, images, video, text, PDF and Office and OpenDocument files (Word, Excel, PowerPoint and their open equivalents), and it outputs text.

WHERE IT RUNS

Vertex AI Model Garden first, where it showed up around 19:12 UTC. Leak accounts on X said it reached Google AI Studio earlier in the afternoon. Some users report that it is limited to certain accounts for now.

WHAT WE DO NOT KNOW

Google had not published a blog post or a Gemini API release note when we wrote this, and OpenRouter did not list the model. There are no independent benchmark scores yet. The same AI Studio code that names Neon also names a "cobalt" model, which Google has not mentioned. We now watch for it as a rumor.

ArtificialWatch is not affiliated with Google.
