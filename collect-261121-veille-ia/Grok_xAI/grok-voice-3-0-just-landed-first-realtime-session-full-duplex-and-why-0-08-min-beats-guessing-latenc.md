---
id: collect-261121-veille-ia/grok-xai/grok-voice-3-0-just-landed-first-realtime-session-full-duplex-and-why-0-08-min-beats-guessing-latenc
title: "Grok Voice 3.0 Just Landed: First Realtime Session, Full Duplex, and Why $0.08/min Beats Guessing Latency"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: ["2026-10-14"]
keywords: ["voice", "grok", "session", "type", "announcement", "speech", "same", "duplex"]
source: https://dev.to/yong_yu_f98e15562e9b120a0/grok-voice-30-just-landed-first-realtime-session-full-duplex-and-why-008min-beats-guessing-mm0
source_anchor: ""
source_lines: [1, 140]
section: "Grok_xAI"
---
**Attributed compile + one small offline smoke test**

**Primary sources (xAI / SpaceXSI, ~2026-10-14/15):** Introducing Grok Voice 3.0 (announcement), Speech to Speech docs, Grok Voice 3.0 model page, and Release notes (October: Voice 3.0 available; Voice 2.0 deprecated)

Benchmark rows below are **Artificial Analysis** (Speech-to-Speech Index, dated October 13, 2026 on the announcement), **xAI** product claims, and **ServiceNow** EVA-Bench (Nitin Aggarwal quote on the announcement). No `XAI_API_KEY` on this box — **I did not open a live WebSocket**. The run below is an offline model-id / `session.update` / mock-event linter only.


xAI shipped **Grok Voice 3.0** into the Voice Agent API at the same list price as Voice 2.0: **$0.08 / min ($4.80 / hr)**. If your agents already use the `grok-voice-latest` alias, the announcement says they were switched to 3.0 automatically — same endpoint, same session fields, same events.

The product pitch is speech-to-speech for support-style agents: under-one-second class latency (xAI), Full Duplex Bench at **97%** (Artificial Analysis), and Tesla / Starlink production numbers on the announcement. This post is the practical first session: WebSocket URL, a clean `session.update`, duplex/`server_vad`, tools, cost arithmetic at list, and what the alias auto-upgrade means for pinned clients.

Artificial Analysis numbers as printed on the Grok Voice 3.0 post (source line: Artificial Analysis, October 13, 2026):

| Benchmark | Grok Voice 3.0 | Grok Voice 2.0 | GPT-Live-1 (Astra, Medium) | Gemini 3.8 Live (Ext. Thinking, High) | 
|---|---|---|---|---|
| Overall AA Speech-to-Speech Index | **86.9** (#1) | 81.3 | 83.2 | 82.6 | 
| Speech Reasoning Big Bench Audio | 98.5% | 97.2% | 90.1% | 97.7% | 
| Human Preference Speech Agent Arena Elo | 1070 | 1011 | 1048 | 990 | 
| Task Success Rate | 94.8% | 94.6% | 87.4% | 89.1% | 
| Full Duplex Bench | **97.0%** | 95.1% | 94.9% | 91.9% | 

τ-Voice (support calls with accents, noise, interruptions): **71.6%**, described as comparable to GPT-Live-1 (Astra) at roughly **40% lower latency**. On ServiceNow's **EVA-Bench**, the announcement cites **74%** accuracy and a Pareto frontier vs accuracy + customer experience; Nitin Aggarwal (ServiceNow) is quoted there saying 3.0 pairs top accuracy with a natural, responsive conversation.

Production color from the same post (vendor numbers, not mine): millions of conversations a day across Tesla and SpaceX; Starlink ~**20,000** calls/day, roughly **70%** no human, **1 in 4** sales calls ending in a purchase. Tesla framing splits **Connectors** (calendar / email hands-free) from **Grok Bot** (bigger tasks). Anecdotes, not a public SLA. I did not reproduce any benches.

Docs endpoint:


```
wss://api.x.ai/v1/realtime?model=grok-voice-3.0
```
Auth: Bearer `XAI_API_KEY` on the server, or an ephemeral token (`xai-client-secret.…`) for browsers. Model query params that matter:

| Model id | Meaning | 
|---|---|
| `grok-voice-latest` | Alias → currently **3.0** | 
| `grok-voice-3.0` | Pin this release | 
| `grok-voice-2.0` | **Deprecated** (same price; upgrade to 3.0 / latest) | 

A minimal `session.update` after connect (shape from the Speech to Speech docs):


```
{
  "type": "session.update",
  "session": {
    "voice": "eve",
    "instructions": "You are a concise support agent. Ask before issuing credits.",
    "turn_detection": { "type": "server_vad" },
    "tools": [
      { "type": "web_search" },
      {
        "type": "function",
        "name": "issue_credit",
        "description": "Issue a billing credit",
        "parameters": {
          "type": "object",
          "properties": { "amount_usd": { "type": "number" } },
          "required": ["amount_usd"]
        }
      }
    ],
    "reasoning": { "effort": "high" },
    "replace": { "Acme Mobile": "Acme Mobull" }
  }
}
```
With `turn_detection.type = server_vad`, stream mic audio via `input_audio_buffer.append` (or binary if `audio.input.transport` is `binary`) and let the server handle turn boundaries — the duplex / barge-in path. Without VAD you commit/clear yourself.

Untested live sketch (`websockets` + a real key; **not run against the API**):


```
import asyncio, json, os
import websockets  # untested live against api.x.ai
async def voice_agent():
    url = "wss://api.x.ai/v1/realtime?model=grok-voice-3.0"
    headers = {"Authorization": f"Bearer {os.environ['XAI_API_KEY']}"}
    async with websockets.connect(url, additional_headers=headers) as ws:
        await ws.send(json.dumps({
            "type": "session.update",
            "session": {
                "voice": "eve",
                "instructions": "You are a helpful assistant.",
                "turn_detection": {"type": "server_vad"},
            },
        }))
        # ... append audio / conversation.item.create / response.create ...
        async for raw in ws:
            event = json.loads(raw)
            print(event.get("type"))
# asyncio.run(voice_agent())  # do not run without a key and audio path
```
Same-session tools: `web_search`, `x_search`, `file_search`, `mcp`, and client `function` tools. Server-side tools run on SpaceXSI; only custom functions need your `conversation.item.create` + `response.create` loop. Pronunciation `replace` fixes spoken audio without changing the visible transcript.

Announcement list: **$0.08 / min = $4.80 / hr**, unchanged from Voice 2.0.

Illustrative only:

- 1,000 support calls × 3 minutes = 3,000 minutes → **$240** at list.
- Starlink-scale illustration using the announcement's ~20k calls/day: at 2 / 4 / 6 minute averages → **$3,200 / $6,400 / $9,600 per day** at list.

Arithmetic on published list rates and call volume — not an invoice. If duplex cuts average handle time by a minute, list saving is $0.08 per call before quality metrics.

No xAI key on the box, so `runs/19-grok-voice-3/preflight.py` only checks shapes:

- 
`grok-voice-3.0` /`grok-voice-latest` → OK;`grok-voice-2.0` → deprecated warning; prompt-shaped and fake ids → rejected.
- Clean `session.update` (voice`eve` ,`server_vad` , web/x/function tools,`replace` , reasoning effort, json transport) → OK.
- Missing voice / bad `turn_detection` / unknown tool type → flagged.
- List-price math prints the $240 and Starlink-scale day figures above.
- Hand-built mock stream (`session.updated` →`response.output_audio.delta` →`response.done` ) parses; missing`delta` and unknown event types raise`ValueError` .

Smoke test of **your client contract**, not of Grok Voice 3.0. Full stdout: `run_output.txt` (2026-10-15, Python 3.13, exit 0).

Release notes and the Speech to Speech upgrade section agree: **`grok-voice-latest` already points at 3.0**, Voice 2.0 is deprecated, API surface unchanged, price unchanged. Two practical implications:

1. 
**If you used the alias** , you are already on 3.0 — verify behavior (reasoning default is`"high"` on 3.0 per the migration notes) rather than assuming a silent quality-only bump.
2. 
**If you pinned `grok-voice-2.0`** , change the query param to`grok-voice-3.0` (or latest). Deprecation is not the same as free forever; pin deliberately for prod, track release notes for the next alias flip.

The Tesla product split is a useful design hint: **Connectors-style** tools for short reversible actions (calendar, email) vs **Grok Bot-style** loops for multi-step work with approvals. Same voice model, different authority.

1. 
**Pin `grok-voice-3.0` in a staging WebSocket** and send one`session.update` with`server_vad` + a single`function` tool you already trust.
2. 
**Measure barge-in and tool overlap audiowise** — docs warn that sending`response.create` before prior audio finishes causes overlap; wait for playback (or nearly) after function outputs.
3. 
**Budget at $0.08/min** with a hard max session length before you chase Elo screenshots.
4. 
**Keep irreversible tools behind confirmation** — credits, shipments, and outbound messages are still your harness's job.

Speech-to-speech rankings move weekly. What is copyable here is boring and useful: a stable realtime URL, unchanged session schema, an AA Full Duplex number, flat per-minute list price, and an alias that already moved. Build the duplex + tool harness once; swap pins when the next index drops.

I have not called the Voice Agent API. Start with a keyed ping and one scripted support turn with approvals on.

*YongBo Yu is an AI engineer in Toronto building LLM workflows and agent systems. More at yongbo-yu.vercel.app and GitHub.*

## Top comments (0)
