---
id: collect-261121-veille-ia/claude-anthropic/claude-fable-5-2-for-voice-agents-where-it-fits-retell-ai
title: "Claude Fable 5.2 for Voice Agents: Where It Fits | Retell AI"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["claude", "fable", "model", "retell", "voice", "agents", "anthropic", "most"]
source: https://www.retellai.com/blog/claude-fable-5-2-voice-agents
source_anchor: ""
source_lines: [1, 111]
section: "Claude_Anthropic"
---
# Claude Fable 5.2 for Voice Agents: Where It Fits and Where It Doesn't

Claude Fable 5.2 is a strong model for building, testing and reviewing voice agents, and a poor default for answering the caller. Anthropic released it on November 9, 2026, for what it calls the most ambitious, open-ended technical work, and recommends starting with Claude Opus 5.5 for most coding and knowledge tasks. On a phone call every reply is on a clock, so the model that speaks should be chosen for speed and cost per minute. The work with no caller waiting is where Fable 5.2 pays for itself.

*Retell AI publishes this guide and offers Claude models, though not Fable 5.2, on its voice agent platform.*

## Claude Fable 5.2 at a glance

|  | **Claude Fable 5.2** | 
|---|---|
| **Developer** | Anthropic | 
| **Released** | November 9, 2026 | 
| **Model ID** | claude-fable-5-2 | 
| **Positioning** | Built for the most ambitious, open-ended technical work. Anthropic recommends starting with Claude Opus 5.5 for most coding and knowledge tasks. | 
| **API price** | $8 per million input tokens and $40 per million output tokens | 
| **Speed** | Generates output up to 30% faster than Claude Fable 5.1. An optional fast mode speeds up token generation by up to 2.5 times, at $16 per million input tokens and $80 per million output tokens. | 
| **Availability** | All platforms, including Amazon Web Services, Google Cloud and Microsoft Azure | 
| **Claude Mythos 5.2** | The same underlying model without certain cybersecurity and biology safeguards, available to vetted organizations | 
| **On Retell AI** | Not in Retell's model list as of November 10, 2026 | 

Source: Anthropic's launch post for Claude Fable 5.2 and Claude Mythos 5.2, read November 10, 2026. Retell availability from Retell's API reference and pricing page, read the same day.

## Why Fable 5.2 is not the model on the call

A voice agent has to answer inside a conversational pause. In a cascading agent, each turn goes through speech-to-text, a language model and text-to-speech, and the language model's time to its first speakable words is one of the delays the caller hears. A model positioned for long, open-ended work has not been shown to fit that clock.

Three things from Anthropic's launch post inform our view:

- **The recommendation.** "For most coding and knowledge tasks, we recommend that users start with Claude Opus 5.5, our strongest daily driver."
- **The price.** Fable 5.2 costs $8 per million input tokens and $40 per million output tokens, which Anthropic presents as a lower price than before. Our reading is that it still suits work where the answer is worth waiting and paying for more than it suits a per-minute phone call.
- **The silence on voice.** The post does not discuss voice agents, real-time conversation or customer service. It reports results on coding, science, computer use and business workflows.

The post does say Fable 5.2 generates output up to 30% faster than Fable 5.1 and offers a fast mode. It gives no time-to-first-response figure for a conversational turn, and Retell has not measured one. We would not claim it is too slow for a call. The evidence that it is the right choice for one has not been published.

There is also a practical answer. Retell's model list for voice agents includes Claude 5.5 Opus, Claude 5.5 Sonnet and Claude 5.5 Haiku. It does not include Fable 5.2.

## Where Fable 5.2 helps a voice team

Most of the work that makes a voice agent good happens when nobody is on the line. A model that can hold a long, messy task and finish it is useful there.

| **Job** | **Fable 5.2's role** | **How it reaches Retell** | 
|---|---|---|
| **Answering the caller, turn by turn** | Not today. Use a model from Retell's list | Chosen in the agent's model selector | 
| **Writing and rewriting agent prompts** | Good fit. The prompt decides how every call goes | Claude Code through the Retell MCP server | 
| **Writing a regression suite** | Good fit. It can turn failure patterns into graded test cases | Creates simulation test cases through the API | 
| **Reviewing calls at volume** | Good fit. It can group failures by cause and quote the turn | Lists calls and reads each call's transcript, latency and post-call analysis | 
| **Changing many agents at once** | Good fit. Long, multi-step work across a workspace | Reads and updates agents one approved call at a time | 
| **Playing the caller in a simulation** | Not today. The simulated caller runs on a model you pick in Retell | Set on the test case | 

Anthropic describes this division of labor itself. The launch post says Fable 5.2 can play the role of an Advisor, or of an Orchestrator that delegates simpler tasks to smaller models. A voice stack is that pattern with a phone attached: a fast model handles each turn, and a heavier model works on the prompts, the tests and the review between calls.

For the hands-on version, see how to build a voice agent with Claude Code and Claude Fable 5.2.

## What if you want Fable 5.2 on the call anyway?

You can bring it yourself. Retell's custom LLM option lets you run your own model behind a voice agent: Retell handles telephony, transcription, turn-taking and speech synthesis, and opens a WebSocket to your server for each call. Your server calls whichever model you choose and streams back what the agent should say.

That route has costs Retell's documentation spells out. You take on latency, uptime and reconnection handling. The LLM playground cannot test a custom LLM agent, and simulation and batch testing reject them, so web and phone calls are the only way to test. Fast fallback, which runs a backup model alongside the primary so a slow response does not hold up the conversation, does not support custom LLM agents. Retell recommends single-prompt or conversation flow agents for most agents.

## How to choose the model that answers the call

Do not pick the model on the call from a launch post, including this one. Pick it from your own calls.

- **Start from Retell's model list.** It is what the platform's testing, fallback and analytics features are built around.
- **Measure latency per call.** Retell's Call History shows end-to-end latency for each call, and the Get Call API returns a breakdown that includes the language model's time to its first speakable chunk.
- **Run the same simulation suite against each candidate.** A model that is faster but fails your transfer or disclosure cases is not faster in any way that matters.
- **Add a backup for slow turns.** Fast fallback starts the primary and a backup model together and lets the backup take over if the primary has not started responding in time.

Our wider guide to choosing an LLM for voice agents covers the trade-offs between model families. For OpenAI's full-duplex voice model, see how GPT-Live-1 works on phone calls. Retell's own LLM, Tell-1, is one more candidate for those tests; the Tell-1 page has its benchmark scores.

## Frequently asked questions

### Can I use Claude Fable 5.2 for a voice agent?

Not as a listed model on Retell as of November 10, 2026. Retell's model list includes Claude 5.5 Opus, Claude 5.5 Sonnet and Claude 5.5 Haiku. You can run Fable 5.2 behind a Retell agent through the custom LLM option, where your own server generates each reply, or use it off the call to build and review agents.

### Is Claude Fable 5.2 fast enough for live phone calls?

Anthropic has not published a figure that answers that. Its launch post says Fable 5.2 generates output up to 30% faster than Fable 5.1 and has a fast mode that speeds up token generation by up to 2.5 times, but it gives no time-to-first-response number for conversation. Measure it on your own calls before relying on it.

### How much does Claude Fable 5.2 cost?

Anthropic charges $8 per million input tokens and $40 per million output tokens. Fast mode costs $16 per million input tokens and $80 per million output tokens.

### What is the difference between Claude Fable 5.2 and Claude Mythos 5.2?

Anthropic says they are the same underlying model. Fable has cybersecurity and biology safeguards so it can be offered to all users. Mythos 5.2 runs without certain of those safeguards and is available only to vetted organizations.

### Which Claude model should a voice agent use?

Choose from the Claude models your voice platform offers and decide on measured latency and pass rate, not on which model is newest. Anthropic itself recommends starting with Claude Opus 5.5 for most coding and knowledge tasks.

### What is Claude Fable 5.2 good for in voice AI?

The work between calls: writing agent prompts, generating test cases, reviewing call transcripts at volume and making changes across many agents. Claude Code can do all of that against a Retell workspace through Retell's MCP server.

## Put Fable 5.2 to work between calls

Keep a fast model on the call and give the heavy model the work around it. Connect Claude Code to the Retell MCP server, have it write a test suite for an agent you already run in Retell, and see what it finds in your own calls.

See how much your business could save by switching to AI-powered voice agents.

Your submission has been sent to your email

## ROI Result

Total Human Agent Cost

AI Agent Cost

Estimated Savings
