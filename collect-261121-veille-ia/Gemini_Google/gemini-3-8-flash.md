---
id: collect-261121-veille-ia/gemini-google/gemini-3-8-flash
title: "Gemini 3.8 Flash"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-11-16"]
keywords: ["flash", "google", "gemini", "model", "high", "variant", "index", "about"]
source: https://ridgebench.com/models/gemini-3.8-flash
source_anchor: ""
source_lines: [1, 74]
section: "Gemini_Google"
---
## Where it stands

Each line is one board. Faint dots are every other model on it; the coloured one is 3.8 Flash.

## About 3.8 Flash

Gemini 3.8 Flash is Google DeepMind's third Flash-tier release in about six weeks, landing three weeks after 3.7 Flash on the same post-training cadence. It is built on top of 3.7 Flash rather than a fresh pretrain, and Google pitches it as the same speed and price tier with a real step up on long-horizon coding and agent tasks.

Ridge tracks it as its own row, separate from 3.7 Flash. Google shipped a second, access-gated variant alongside it — Gemini 3.8 Flash Cyber, for vetted cybersecurity defenders — which is a distinct model and not covered by this entry.

- Outperforms most larger frontier models on DeepSWE v1.1 for complex software-engineering tasks (company-reported).
- Exceeds other frontier models on the Vals Finance Agent V2 and Harvey Legal Agent benchmarks (company-reported).
- 54.9% on HLE-Verified for multi-step reasoning across STEM and professional fields (company-reported).
- Meaningful gains in prompt-injection robustness on Gray Swan evaluations (company-reported).

Chat talk is thin. Most Flash posts were about the separate TTS model.

- Model id
- gemini-3.8-flash
- Context
- 1,048,576 tokens input
- Max output
- 65,536 tokens
- Knowledge cutoff
- March 2026 (some domains limited to January 2025)
- Modalities
- Text, image, video, audio, PDF in; text out
- Reasoning
- Selectable thinking levels: low / medium (default) / high (no minimal tier)
- Tools
- Function calling, code execution, file search, URL context, Maps grounding
- Price (intro, through 2026-12-31)
- $0.75 input / $3.75 output per 1M tokens
- Price (standard, from 2027-01-01)
- $1.50 input / $7.50 output per 1M tokens
- Weights
- Closed
- Lab
- Google DeepMind

Gemini API · Google AI Studio · Android Studio · Google Antigravity · Stitch · Gemini Enterprise · Gemini app (Pro/Ultra) · Google Search AI Mode · Google Sheets

## Every number on this page

Where each figure was published and when Ridge read it. If it isn’t here, Ridge doesn’t claim it.

| AA Intelligence Index v5.0 best effort/variant: high | 31 | Artificial Analysis | 2026-11-16 | 
| SWE-bench Verified | 80% | Vals AI | 2026-11-20 | 
| Arena Elo early listing | 1,505 | Arena+ | 2026-11-20 | 
| Terminal-Bench 4.0 Mini-SWE-agent; #42 of 69; 3 provider refusals scored as failures | 19.7% | Vals AI | 2026-11-20 | 
| Terminal-Bench 2.1 (archived) Terminus 2; #8 of 76; Terminal-Bench 2.1 | 81.27% | Vals AI | 2026-10-06 | 
| AA Cyber Index variant: high. Declines about a third of tasks on safety grounds; refuses most CyberGym tasks. | 25 | Artificial Analysis | 2026-11-20 | 
| Harvey LAB-AA v1.1 variant: high; #40 of 45; 13.96 material hallucinations per task; 6.7% before the hallucination gate | 0% | Artificial Analysis | 2026-11-20 | 
| AA Intelligence Index v4.3.2 (archived) best effort/variant: high | 41 | Artificial Analysis | 2026-10-27 | 
| CursorBench 4.0 (archived) high (best published) | 39.6% | Cursor | 2026-09-11 | 
| Google Blog — Introducing Gemini 3.8 Flash and 3.8 Flash Cyber |  | blog.google |  | 
| Google AI for Developers — Gemini 3.8 Flash model page |  | ai.google.dev |  | 
| Google DeepMind — Gemini 3.8 Flash model card |  | deepmind.google |  | 

## Where it came up

### Haiku 5.5 is the cheap Claude people will actually use. Just watch the 100K line.

7 October. The last Claude 5.5 model is fast, very cheap on short prompts, and the strongest small model on the AA Index. Long prompts cost five times as much.

1 Oct · ranking
### Half-week opinion: Sonnet 5.5 and GPT-6.1 Sol open at 3; everyone else holds

Public posts on X as of 1 October. Two first ratings. No moves on existing stars.

30 Sept · release
### Gemini 4 Argon: Google is back near the top, but you cannot use it yet

30 Sep. Google's first big model since the spring ties GPT-6 Astra on the AA Index. For now it goes to cyber defenders only.
