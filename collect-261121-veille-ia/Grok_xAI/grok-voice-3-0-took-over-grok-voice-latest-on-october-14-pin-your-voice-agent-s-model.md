---
id: collect-261121-veille-ia/grok-xai/grok-voice-3-0-took-over-grok-voice-latest-on-october-14-pin-your-voice-agent-s-model
title: "Grok Voice 3.0 Took Over grok-voice-latest on October 14. Pin Your Voice Agent's Model"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: []
keywords: ["voice", "model", "grok", "grok-voice-latest", "alias", "agent", "october", "speech"]
source: https://dev.to/gabrielanhaia/grok-voice-30-took-over-grok-voice-latest-on-october-14-pin-your-voice-agents-model-435
source_anchor: ""
source_lines: [1, 108]
section: "Grok_xAI"
---
- 
**Book:** Observability for LLM Applications
- 
**Also by me:***Thinking in Go* (2-book series) — Complete Guide to Go Programming + Hexagonal Architecture in Go
- 
**My project:** Hermes IDE | GitHub — an IDE for developers who ship with Claude Code and other AI coding tools
- 
**Me:** xgabriel.com | GitHub

If your voice agent connects to xAI with `model=grok-voice-latest` and you tested it before October 14, it is not running the model you tested. On October 14, xAI launched Grok Voice 3.0, and its release notes say: "`grok-voice-latest` now points to `grok-voice-3.0`, so sessions that use the alias are upgraded automatically."

No deploy on your side. No config change. Your callers started talking to a different model once the alias moved.

That is the quiet half of the launch. The loud half is the scoreboard: xAI's new speech-to-speech model sits at the top of the Artificial Analysis Speech to Speech Index at 86.9, ahead of OpenAI's GPT-Live-1 (Astra, medium) at 83.2.

Here is the whole update, from xAI's own docs:

- 
`grok-voice-3.0` is available for Speech to Speech "at the same price as Grok Voice 2.0": $0.08 a minute, or $4.80 an hour, plus $0.004 per text input, per the pricing page.
- 
`grok-voice-2.0` is marked deprecated. The release notes say: "To upgrade, set`model` to`grok-voice-3.0` or`grok-voice-latest` , at the same price." Neither the release notes nor the models page gives a retirement date yet.
- The alias `grok-voice-latest` now resolves to 3.0. Per the release notes, the API itself is unchanged, so your WebSocket code should keep working.

The endpoint stays the same WebSocket: the voice docs show `wss://api.x.ai/v1/realtime?model=grok-voice-latest`, with your API key in an `Authorization: Bearer` header. Their own example uses the alias, so any agent copied from that example is on it.


*An alias is a switch on the track. When the vendor throws it, every session on it changes direction.*

xAI's announcement publishes these numbers and credits them to Artificial Analysis, dated October 13, 2026:

| Benchmark | Grok Voice 3.0 | Grok Voice 2.0 | GPT-Live-1 (Astra, Medium) | 
|---|---|---|---|
| Big Bench Audio | 98.5% | 97.2% | 90.1% | 
| Full Duplex Bench | 97.0% | 95.1% | 94.9% | 
| τ-Voice (agent success) | 71.6% | 56.5% | 74.5% | 

Artificial Analysis also publishes an overall score on its own Speech to Speech leaderboard, which lists Grok Voice 3.0 first at 86.9, then GPT-Live-1 (Astra, medium) at 83.2, Gemini 3.8 Live Extended Thinking (High) at 82.6 and Grok Voice 2.0 at 81.3.

Look at the τ-Voice row, though. It's the agentic row, a benchmark of support calls with "diverse accents, background noise, and interruptions" per the announcement, and on the chart xAI published, GPT-Live-1 still leads it, 74.5% to 71.6%. xAI's pitch there is latency: the announcement claims 40% lower latency than GPT-Live-1 on that benchmark, and "class-leading latency at under one second." Unite.AI's launch coverage reports both scores and notes that xAI calls its result comparable.

The jump from 2.0 matters more for you than the lead over OpenAI. τ-Voice went from 56.5% to 71.6% between xAI's own two models (announcement). That's a large behavioral change. A model that completes more tasks also gets there by other routes, from when it yields the turn to when it hands off or asks again. Your prompts were tuned against the old routes.

I haven't run Grok Voice 3.0 myself. Treat every number above as reported, not measured by me.

A `-latest` alias trades control for convenience. xAI's models page describes the general scheme: `<modelname>-latest` points to the latest version, while a dated `<modelname>-<date>` name "will not be updated and is for workflows that demand consistency." The same page recommends the aliases for most users. That's a reasonable default. It just means the alias promises the newest model and says nothing about keeping it the same.

The docs don't say whether a versioned ID like `grok-voice-3.0` can change under you. Check the models page for a dated voice ID, and pin that if one is listed.

For a chat feature, a model swap shows up as slightly different wording. For a voice agent on a phone line, it can show up as:

- a script that now interrupts callers sooner, or waits longer before answering
- a different rate of escalations to humans
- tool or web-search calls at different moments, with different costs
- an instruction you relied on that the new model weighs differently

None of that is a claim about Grok Voice 3.0 in particular. It's what changes when any model changes under a prompt, and it's why you want to pick the moment it happens.


*Pin the version you tested. Move to the next one on your schedule, after your own checks.*

First, find out whether you're on the alias:


```
grep -rn "grok-voice-latest" .
```
If you are, pin the versioned ID. The URL shape comes from the voice docs; only the `model` value changes:


```
import os
MODEL = "grok-voice-3.0"  # pinned, not grok-voice-latest
URL = f"wss://api.x.ai/v1/realtime?model={MODEL}"
HEADERS = {
    "Authorization": f"Bearer {os.environ['XAI_API_KEY']}",
}
```
If you were on the alias before October 14, you have been on 3.0 since that day. Pinning `grok-voice-3.0` takes you off the alias, so the next time xAI moves it, your agent stays where it is. If you'd rather stay on what you originally tested, you can pin `grok-voice-2.0`, but it is deprecated per the release notes, so that only buys time.

Then run the upgrade like any other dependency bump:

1. Keep a set of recorded or scripted test calls that cover your real flows: happy path, interruptions, a caller who changes their mind, a request that should escalate.
2. Run them against the old pinned model and against `grok-voice-3.0` , with the same`session.update` config.
3. Compare task completion, escalations, turn count and minutes per call. At $0.08 a minute on both versions (pricing), minutes per call is your cost signal.
4. Switch the pinned ID when the new numbers are at least as good, and log the model ID on every session so you can tell the two apart afterwards.

Here is a trimmed version of the session config the voice docs show, which stays identical across both runs:


```
{
  "type": "session.update",
  "session": {
    "voice": "eve",
    "instructions": "You are a support agent for Acme.",
    "turn_detection": { "type": "server_vad" }
  }
}
```
Every vendor with a moving alias can do this to you. One cheap guard: a CI check that fails when any `-latest` model ID shows up in production config. Then a model change only happens as a deploy you chose.

Do you pin model IDs in your voice or chat agents, or do you ride `-latest` and watch the dashboards?

A model swap that nobody deployed is exactly the kind of change you only catch if you trace your LLM calls and run evals against them. Observability for LLM Applications covers that: tracing, evals, cost and quality regressions for LLM features in production, so a silent upgrade shows up as a diff in your numbers instead of a surprise in your support queue.

And if you work with coding agents, I build Hermes IDE, a free, source-available IDE for running Claude Code, Codex and other agents side by side (GitHub).

## Top comments (0)
