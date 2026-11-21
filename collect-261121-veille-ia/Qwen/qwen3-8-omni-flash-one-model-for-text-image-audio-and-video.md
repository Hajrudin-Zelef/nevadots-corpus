---
id: collect-261121-veille-ia/qwen/qwen3-8-omni-flash-one-model-for-text-image-audio-and-video
title: "Qwen3.8 Omni Flash: One Model for Text, Image, Audio and Video"
domain: qwen
role: reference
task: article
actors: ["Alibaba"]
dates: []
keywords: ["model", "audio", "video", "text", "qwen3", "flash", "image", "omni"]
source: https://mer.vin/news/qwen3-8-omni-flash-one-model-text-image-audio-video/
source_anchor: ""
source_lines: [1, 322]
section: "Qwen"
---
Alibaba’s Qwen team shipped Qwen3.8-Omni-Flash on September 14, 2026 — one model that reads text, looks at images and video, and listens to audio, then plans and executes multi-step agent tasks across all four. The headline claim: audio-visual performance close to Gemini 3.8 Flash, and audio performance that beats it, while API pricing for audio and audio-visual input drops by 93–98%.

```
%%{init: {"theme": "base", "themeVariables": {"background": "transparent", "lineColor": "#000000"}}}%%
graph TD
    A["Text, image, audio, video input"] --> B[Qwen3.8-Omni-Flash]
    B --> C{"Static or agentic mode?"}
    C -->|Static| D["Direct answer"]
    C -->|Agentic| E["Plan and call tools"]
    E --> F[Qwen-MM-Plugins]
    F --> G["Video2Note, Music2MV, dubbing, commentary"]
    E --> H["Qwen3.8-Omni-Flash-Realtime"]
    H --> I["Voice and camera session"]
    classDef agent fill:#8B0000,color:#fff
    classDef hook fill:#189AB4,color:#fff
    classDef decision fill:#444,color:#fff
    class B,E agent
    class F,H hook
    class C decision
```
## What is Qwen3.8-Omni-Flash?

It is a single model you call over an API that can take a picture, a video clip, and an audio file in the *same request*, understand how they relate to each other, and then act — write meeting minutes, dub a video into another language, or navigate a robot toward a sound. It replaces a pipeline that used to need a separate speech-to-text model, a separate vision model, and a separate text model glued together by hand.

| Question | Answer | 
|---|---|
| What does “Omni” mean here? | One model accepts text, image, audio and video as input, and can output text or synthesized speech — not four separate models behind a router. | 
| What does “Flash” mean? | The fast/cheap tier of the Qwen3.8 line, tuned for latency and cost rather than maximum raw reasoning depth. | 
| Is it open weight, like DeepSeek or other Qwen models? | No. It ships only through Alibaba Cloud’s DashScope API — no downloadable checkpoint was found on Hugging Face at publication time. | 
| What is new compared to the previous omni model? | Qwen3.5-Omni-Plus. The blog reports a 25%+ average score gain across 29 evaluations and a 1M-token context window. | 
| Can it run continuously, not just answer one prompt? | Yes — a separate “Realtime” variant streams audio/video in and speech out over WebSocket or WebRTC for live conversation. | 

|  | Old way | New way | Everyday analogy | 
|---|---|---|---|
| Understanding a video | Transcribe audio with one model, caption frames with another, stitch results by hand | One model watches and listens together, natively | Hiring a court stenographer *and* a sketch artist vs. hiring one witness who can both see and hear | 
| Long meeting recordings | Run ASR, then guess who said what from a flat transcript | Model aligns voice, face and screen share to tell speakers apart automatically | Reading a play script with no character names vs. watching the play | 
| Making a model good at a niche task | A human ML engineer builds a dataset, trains, evaluates, repeats for days | The Omni model designs its own eval set, listens to failures, and retrains a smaller model in hours | A senior engineer mentoring a junior overnight instead of weeks of code review | 

*These advances mean audio and video are evolving from perceptual inputs into core media through which agents understand their environment, reason, and execute tasks.* — Qwen team


## One model, four senses: what actually changed

The spec sheet is short, but two numbers do the heavy lifting: a 1M-token context window that the blog says holds text performance “comparable to a text-only model of the same size,” and a claim that audio-visual quality now sits close to Gemini 3.8 Flash.

| Field | Value | 
|---|---|
| Release date | September 14, 2026, 15:00 Beijing time | 
| Modalities in | Text, image, audio, video | 
| Modalities out | Text, synthesized speech | 
| Context window | 1,000,000 tokens | 
| Predecessor | Qwen3.5-Omni-Plus | 
| Reported average gain vs. predecessor | >25% across 29 evaluations | 
| Realtime variant | Qwen3.8-Omni-Flash-Realtime (WebSocket / WebRTC) | 
| Speech recognition languages | 74 languages + 39 Chinese dialects | 
| Speech generation languages | 29 languages + 7 Chinese dialects | 
| API compatibility | OpenAI-compatible Chat Completions and Responses APIs | 
| Access | Qianwen AI Platform / Alibaba Cloud DashScope API only — no public weights found | 
| reasoning_effort levels | xhigh (default), medium, low | 

The blog frames the release around a system-level problem, not just a bigger model: long audio and video are expensive to store and re-process across multiple inference rounds, and most agent harnesses have no native concept of a video or audio attachment at all. Qwen’s answer is to ship the model alongside two pieces of tooling — Qwen-MM-Plugins and Qwen-Live Harness — covered below.

## Static vs. agentic understanding: why watching less can mean seeing more

For a two-hour video, feeding every frame and every second of audio into the model wastes budget on footage the question never touches. Qwen3.8-Omni-Flash can instead run “agentically”: it decides what to watch and listen to, gathers evidence in coarse-to-fine passes, and stops once it has enough.

| Mode | OmniVideoBench accuracy ↑ | Tokens per query ↓ | 
|---|---|---|
| Static (watch everything) | 63.4 | 145,736 | 
| Agentic (decide what to watch) | 67.8 | 79,117 (−45.7%) | 

- Higher accuracy *and* fewer tokens — the agentic pass isn’t a cost/quality trade, per the vendor’s own numbers.
- Gemini 3.8 Flash’s own agentic mode still scores higher in absolute terms on this benchmark: 65.2 static → 70.1 agentic, ahead of Qwen at both points.
- On LVOmniBench, Qwen’s agentic score (73.6, cited in the walkthrough copy) closes most of the gap to Gemini’s flat 70.7 — Gemini shows no agentic lift there at all.

## Long meetings: from minutes to action

Meeting audio is the release’s strongest single number, and also its most suspicious-looking one. The blog reports AliMeeting multi-speaker diarization error (DER) and word error (cpWER) falling from 88.1 / 89.6 for the predecessor model to 3.4 / 17.2 for Qwen3.8-Omni-Flash.

| Model | AliMeeting DER ↓ | AliMeeting cpWER ↓ | 
|---|---|---|
| Qwen3.8-Omni-Flash | 3.4 | 17.2 | 
| Qwen3.5-Omni-Plus (predecessor) | 88.1 | 89.6 | 
| Gemini 3.8 Flash | 72.6 | 53.1 | 
| Seed 2.0 Lite | 75.1 | 76.1 | 
| Muse Spark 1.2 | 93.7 | 92.7 | 

A DER near 88–94% is close to random-chance speaker assignment, which every other model in the table except Qwen still posts. That’s a bigger gap than a one-generation model refresh usually produces, and it’s worth reading as “multi-speaker diarization went from essentially broken to usable” rather than “20% better than last time.” Treat the exact figure as vendor-reported until an independent multi-speaker benchmark reproduces it.

The capability itself is a real workflow change: point the model at a full meeting recording and it maps who-said-what, drafts minutes, lists action items, and — with tool access — sends the follow-up emails and opens the first coding tasks the meeting generated, using the video to resolve references audio alone gets wrong (“that thing on the screen” only makes sense with vision).

## From editing room to API: Music2MV, drama dubbing, film commentary

Three of the launch demos target production work that currently means switching between four or five separate tools.

| Workflow | What it replaces | What the agent does | 
|---|---|---|
| Music2MV | Manually storyboarding a music video to match rhythm and lyrics | Reads song structure, rhythm, mood and vocals; outputs line-level timestamped lyrics aligned to visuals | 
| Short drama translation | Separate transcription, translation, voice-cloning and dubbing tools, coordinated by hand | Speaker-aware translation, voice-cloned dubbing, remix and QA from one sentence of instruction | 
| Long-form film commentary | Re-watching a film, writing a script, recording voiceover, editing manually | Extracts key plot points, writes and voices commentary, interleaves it with original audio and adjusts pacing automatically | 

All three are demoed, not benchmarked — the blog links video demos rather than publishing a quality score for dubbing accuracy or commentary coherence, so treat this section as capability description, not a verified result.

## Self-improving models: the Sichuan dialect case study

The most concrete agentic example in the launch is narrower and better-specified than the production demos: Qwen3.8-Omni-Flash was given 12 hours to improve a much smaller model, Qwen2.5-Omni-3B, at recognizing Sichuan-dialect speech.

1. The larger model picked the WenetSpeech-Chuan evaluation set itself and fixed a baseline.
2. It listened to failing audio samples, diagnosed the error pattern, and built targeted training data — 3,413 examples across four rounds.
3. After each round it re-evaluated, kept improvements that worked, and rolled back ones that didn’t.

| Metric | Before | After | 
|---|---|---|
| Character error rate (Qwen2.5-Omni-3B, Sichuan dialect) | 25.79% | 15.30% (−40.7% relative) | 
| Training examples generated | 3,413, across 4 rounds |  | 
| Wall-clock budget | 12 hours |  | 

This is a single vendor-run case study on one dialect, not a general claim that every fine-tuning task compresses to 12 hours — but as a demonstration of “big model designs the eval and the training loop for a small model,” it’s a concrete, checkable claim rather than marketing language.

## Benchmark results: the omni comparison table (vendor-reported)

Every number below comes from Qwen’s own announcement, evaluated by Qwen using harnesses the blog names in its footnotes (Claude Code, mini-SWE-agent, or none, depending on the benchmark). None of it has been independently reproduced yet — read it as the vendor’s account of its own model, not a neutral leaderboard.

| Benchmark | Qwen3.8-Omni-Flash | Qwen3.5-Omni-Plus | Gemini 3.8 Flash | Seed 2.0 Lite | 
|---|---|---|---|---|
| WildClawBench-MM (tool use) | 71.0 | 34.5 | 58.9 | 41.9 | 
| UniClawBench (tool use) | 69.6 | 67.1 | 69.0 | 61.2 | 
| AgenticVBench (tool use) | 36.8 | 14.5 | 45.0 | 10.0 | 
| OmniGAIA (web search) | 74.0 | 57.2 | 78.6 | 64.4 | 
| DailyOmni | 85.1 | 85.1 | 84.0 | 81.4 | 
| WorldSense | 68.5 | 63.9 | 69.6 | 67.3 | 
| AVUT | 86.6 | 85.9 | 88.0 | 81.5 | 
| JoinAVBench | 75.9 | 74.1 | 70.4 | 70.6 | 
| OmniVideoBench (static) | 63.4 | 53.8 | 65.2 | 58.5 | 
| Video-MME-v2 | 65.0 | 47.9 | 71.0 | 64.9 | 
| LVOmniBench | 63.3 | 53.2 | 70.7 | — | 
| OmniCloze (caption) | 63.2 | 64.2 | 60.9 | 56.3 | 
| OmniCap-IF (CSR / ISR) | 80.6 / 28.2 | 72.1 / 14.1 | 81.9 / 28.3 | 74.6 / 18.1 | 
| QIVD | 69.6 | 65.6 | 69.1 | 62.0 | 
| StreamingBench | 80.8 | 57.1 | 79.9 | 77.2 | 
| MMAU (audio) | 81.8 | 81.9 | 76.9 | 77.2 | 
| MMAR (audio) | 79.8 | 79.8 | 78.5 | 77.7 | 
| MMSU (audio) | 82.1 | 83.0 | 83.3 | 80.2 | 
| FLEURS-S2TT (BLEU, 60 languages) | 31.8 | 32.2 | 33.0 | 24.8 | 
| SpotSoundBench (grounding) | 67.2 | 64.2 | 39.7 | 59.6 | 
| MuchoMusic-RUL | 72.6 | 71.6 | 53.7 | 61.7 | 

Reading straight down the “highest score” column, Qwen3.8-Omni-Flash wins about half of these 20 rows and Gemini 3.8 Flash wins most of the rest — this is a competitive, not dominant, release on general audio-visual understanding. Qwen’s clearest edges are agentic tool-use (WildClawBench-MM, StreamingBench) and audio grounding (SpotSoundBench); Gemini’s clearest edges are long-video reasoning (Video-MME-v2, LVOmniBench) and web-search agent tasks (OmniGAIA, AgenticVBench).

## Benchmark results: text and vision (vendor-reported)

The blog also claims the omni model doesn’t sacrifice text or vision quality against Qwen’s own text-only siblings, comparing it to Qwen3.8-Flash, Qwen3.8-27B, Qwen3.7-Plus, DeepSeek-V4-Flash-0731, and Claude-Opus-4.6 (Max). Bold marks the best score the source reports per row.

| Text benchmark | Omni-Flash | Qwen3.8-Flash | Qwen3.8-27B | Qwen3.7-Plus | DeepSeek-V4-Flash-0731 | Claude-Opus-4.6 (Max) | 
|---|---|---|---|---|---|---|
| DeepSWE 1.1 | 57.8 | **58.7** | 42.2 | 16.5 | 54.4 | — | 
| SWE-bench Pro | **63.3** | 62.5 | 61.7 | 55.8 | 56.0 | 53.4 | 
| SWE-bench Multilingual | 80.5 | **81.0** | 73.8 | 75.8 | — | 77.5 | 
| NL2Repo-Bench | 48.9 | 48.1 | 42.3 | 41.1 | **54.2** | 47.6 | 
| CoWorkBench | **75.3** | 73.9 | 70.7 | 65.1 | 45.1 | 68.2 | 
| IFBench | **81.5** | 81.3 | 79.5 | 79.1 | 79.2 | 62.5 | 
| GPQA Diamond | 91.0 | **91.7** | 89.2 | 90.3 | 90.8 | 91.3 | 
| HLE | 36.5 | 35.9 | 30.8 | 34.7 | 33.8 | **40.0** | 
| LiveCodeBench v6 | **92.6** | 91.9 | 90.3 | 89.6 | 90.6 | 88.8 | 

| Vision benchmark | Omni-Flash | Qwen3.8-Flash | Qwen3.8-27B | Qwen3.7-Plus | Claude-Opus-4.6 (Max) | 
|---|---|---|---|---|---|
| ClawEval-MM (avg) | **61.9** | 60.4 | 56.9 | 60.1 | 54.7 | 
| AndroidWorld (mobile use) | **87.1** | 84.5 | 81.9 | 81.0 | 62.0 | 
| Vision2Web | 62.9 | **64.0** | 62.9 | 42.1 | — | 
| ERQA (embodied) | 71.0 | **72.3** | 65.5 | 69.8 | 40.8 | 
| LVBench | **76.9** | 76.6 | 72.4 | 76.2 | 63.0 | 
| RealWorldQA | 87.7 | **88.5** | 85.9 | 86.9 | 73.9 | 
| MathVision (with CI) | **96.2** | 95.7 | 94.6 | 88.4 | — | 
| CharXiv (with CI) | **91.4** | 90.6 | 90.2 | 85.9 | — | 

Two caveats the footnotes disclose directly: SWE-bench Pro scores everyone except Claude-Opus-4.6 with the Claude Code harness at temp=1.0, but Claude’s own number is “the officially published score” from elsewhere — not run under the same conditions. And DeepSWE 1.1 reports “the highest score across two harnesses” for Qwen models specifically. Neither is hidden, but both mean this isn’t a single controlled experiment.

## Real-time throughput and latency

For the Realtime API — the variant meant for live voice/video conversation — Qwen published observed production latency rather than a lab-only number.

| Input scenario | Text output TPS | Time to first token (ms) | Time to first audio packet (ms) | Audio generation RTF | 
|---|---|---|---|---|
| Audio, 6s | 84.87 | 591.26 | 978.36 | 0.1538 | 
| Audio, 12s | 83.18 | 604.80 | 982.74 | 0.1537 | 
| Audio, 20s | 81.06 | 617.98 | 1026.39 | 0.1538 | 
| Audio-visual, 6s | 84.89 | 837.96 | 1214.73 | 0.1524 | 
| Audio-visual, 12s | 84.33 | 911.85 | 1268.07 | 0.1527 | 
| Audio-visual, 20s | 83.00 | 981.01 | 1350.49 | 0.1528 | 

An RTF (real-time factor) around 0.15 means audio generates roughly 6-7x faster than playback speed — the model can keep up with a live conversation with headroom left over. Adding video roughly doubles time-to-first-audio-packet (978ms → 1,215ms at the 6-second mark) without materially hurting steady-state token throughput.

## Pricing: what actually got cheaper

The 93–98% price cuts are for audio and audio-visual *input* specifically, priced by the hour rather than per token — and the methodology is disclosed rather than left vague.

| Item | Detail | 
|---|---|
| Audio input, hourly price | Down >98% vs. predecessor pricing | 
| Audio-visual input, hourly price | Down >93% vs. predecessor pricing | 
| Methodology | “Hourly” price = 30 × the input cost of a 2-minute source clip | 
| Audio-visual test condition | 720p video sampled at 1 fps | 
| Text-only pricing | Not published in this announcement — check DashScope’s live pricing page before estimating a bill | 

Because the baseline isn’t stated as a flat per-token rate, don’t extrapolate “98% cheaper” onto a text-heavy workload — it specifically describes long audio and audio-visual ingestion, the exact use case the rest of the launch is built around.

## Getting started: API and reasoning_effort

The API is OpenAI-compatible (Chat Completions and Responses), reachable through the official `openai` Python SDK by pointing `base_url` at DashScope, and it exposes a `reasoning_effort` parameter with three levels: `xhigh` (default, for complex tasks), `medium`, and `low`.

```
from openai import OpenAI
import os
api_key = os.environ.get("DASHSCOPE_API_KEY")
if not api_key:
    raise ValueError("DASHSCOPE_API_KEY is required.")
client = OpenAI(
    api_key=api_key,
    base_url=os.environ.get(
        "DASHSCOPE_BASE_URL",
        "https://dashscope-intl.aliyuncs.com/compatible-mode/v1",
    ),
)
messages = [
    {
        "role": "user",
        "content": [
            {"type": "image_url", "image_url": {"url": "https://example.com/dog_and_girl.jpeg"}},
            {"type": "input_audio", "input_audio": {"data": "https://example.com/cherry.wav", "format": "wav"}},
            {"type": "text", "text": "Describe the image and tell me what is said in the audio."},
        ],
    },
]
completion = client.chat.completions.create(
    model="qwen3.8-omni-flash",
    messages=messages,
    extra_body={"enable_thinking": True},
    reasoning_effort="xhigh",
    stream=True,
)
for chunk in completion:
    if not chunk.choices:
        continue
    delta = chunk.choices[0].delta
    if getattr(delta, "content", None):
        print(delta.content, end="", flush=True)
```
For structured captioning work, Qwen’s own best-practice advice is to put a target word count in the prompt (e.g. “describe in 2,000–3,000 words”) and to supply a full JSON Schema when you need machine-parseable output — the blog includes a worked schema covering storyline, visible on-screen text, and a speaker/transcript block with timestamps.

## Qwen-MM-Plugins: multimodal eyes and ears for any agent harness

Rather than keep the workflow logic inside one closed app, Qwen shipped **Qwen-MM-Plugins** as a separate, Apache-2.0-licensed toolkit that plugs into existing coding/agent harnesses — the README lists Codex, Claude Code, Qwen Code, Gemini CLI, Coder, CodeBuddy and OpenClaw as supported hosts. I confirmed both repositories are live, not placeholders: Qwen-MM-Plugins ships a real architecture diagram in its docs, and Qwen Code (the harness pictured above) is Qwen’s own 28k-star open-source coding agent — one of the seven hosts this plugin suite targets.

| Plugin | What it adds | 
|---|---|
| core | Read images, video frames, PDFs, Office docs, code, data and 3D files | 
| api | Image understanding, OCR, object localization, audio-visual transcription, speaker diarization, segmentation | 
| omni-chatcut | Music videos, long-form film commentary, video speech translation | 
| omni-video2note | Turns video tutorials into PDF notes with key screenshots | 
| omni-skill-creator | Turns instructional videos into reusable Agent Skill.md files | 
| omni-memory | Builds persistent memory of people, dialogue, sounds and events across long videos | 

Install is a one-liner:

```
curl -fsSL https://raw.githubusercontent.com/QwenLM/Qwen-MM-Plugins/main/install.sh | bash
# or, inside an agent session:
# "Help me install the core, api, and omni-related plugins from
#  https://github.com/QwenLM/Qwen-MM-Plugins."
```
Once installed, invoking a plugin from inside a harness looks like an ordinary chat message with an attachment reference:

- `@song.mp3 Generate a complete MV based on the song's rhythm and content.`
- `@tutorial.mp4 Turn the tutorial into PDF notes with key screenshots and step-by-step instructions.`
- `@weekly_report_sop.mp4 Turn this screen recording into a Skill.md file.`

## Qwen-Live Harness: the open-source repo that 404s

The blog states plainly that the team “open-sourced Qwen-Live Harness as a native runtime for continuous, real-time omnimodal interaction,” links `github.com/QwenLM/Qwen-Live-Harness` at the top of the post, and gives a three-line install:

```
npm install -g qwen-live-harness
qwen-live-harness init
qwen-live-harness
```
I checked both halves of that claim independently on September 19, 2026, five days after launch. The GitHub repository the blog links to returns a straightforward 404 — Hacker News commenters noticed the same thing on launch day. But the npm package is real: `qwen-live-harness` is published, currently at version 0.4.2, Apache-2.0 licensed, with a substantial README describing a macOS overlay app, a WebSocket daemon protocol, and adapters for `qwen serve` and ACP-compatible agents (Qwen CLI, Qoder CLI, Gemini CLI).

| What I checked | Result | 
|---|---|
| github.com/QwenLM/Qwen-Live-Harness | 404 Not Found (verified 2026-09-19) | 
| npm package `qwen-live-harness` | Live, v0.4.2, Apache-2.0 (verified 2026-09-19) | 
| github.com/QwenLM/Qwen-MM-Plugins | Live, real README and architecture.svg (verified 2026-09-19) | 

Practically: you can install and run Qwen-Live Harness today through npm, but the source code the blog promises as “open-source” isn’t publicly browsable at the URL it names, at least as of this writing. If you’re evaluating it for a real project, install from npm and don’t wait on the GitHub link — but also don’t assume you can audit or fork the source yet.

## Limitations and where Gemini 3.8 Flash still wins

Reading the full comparison tables rather than the headline claim, four gaps stand out:

- **No open weights.** Every other model in this launch’s comparison tables that touts open access (Qwen3.8-27B, Qwen3.8-Flash) has a Hugging Face presence; Qwen3.8-Omni-Flash does not appear to. It’s API-only.
- **Long-video reasoning trails Gemini.** Video-MME-v2 (65.0 vs. 71.0) and LVOmniBench (63.3 vs. 70.7 in static mode) both favor Gemini 3.8 Flash by a wide margin.
- **Web-search agent tasks trail Gemini.** OmniGAIA (74.0 vs. 78.6) and AgenticVBench (36.8 vs. 45.0) both favor Gemini.
- **The open-source harness link is broken.** See above — a real, if minor, launch-day rough edge that the vendor blog doesn’t acknowledge.

The blog’s own footnotes are unusually candid about evaluation caveats for a marketing post — naming which harness scored which model, flagging when a competitor’s number is self-reported rather than freshly run, and marking cells “not yet available” instead of omitting them silently. That transparency doesn’t make the numbers independently verified, but it does make them easier to audit than most vendor benchmark posts.

## At a glance

| Item | Qwen3.8-Omni-Flash | 
|---|---|
| Released | September 14, 2026 | 
| Modalities | Text, image, audio, video in; text and speech out | 
| Context window | 1,000,000 tokens | 
| Access | Alibaba Cloud DashScope API; no open weights found | 
| Strongest claim | Audio performance exceeding Gemini 3.8 Flash (vendor-reported) | 
| Biggest single number | AliMeeting DER 88.1 → 3.4 (vendor-reported, unusually large swing) | 
| Open-source tooling | Qwen-MM-Plugins (verified live) + Qwen-Live Harness (npm live, GitHub 404) | 
| Price change | Audio input −98%, audio-visual input −93% (hourly, vs. predecessor) | 
| Where it trails | Long-video reasoning and web-search agent tasks vs. Gemini 3.8 Flash | 

Bottom line for engineers: if your workload is long audio or meeting video and you’re already comfortable calling an API rather than hosting weights, the pricing and diarization numbers make this worth a trial run this week. If you need open weights to self-host, or your bottleneck is long-video web-search agent tasks, Gemini 3.8 Flash’s own numbers — as reported by Qwen itself — still lead.
