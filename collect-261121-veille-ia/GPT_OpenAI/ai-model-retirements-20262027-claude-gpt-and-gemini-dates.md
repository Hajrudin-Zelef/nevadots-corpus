---
id: collect-261121-veille-ia/gpt-openai/ai-model-retirements-20262027-claude-gpt-and-gemini-dates
title: "AI Model Retirements 2026–2027: Claude, GPT and Gemini Dates"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["models", "model", "openai", "claude", "dates", "retirement", "gemini", "gemini-2"]
source: https://tecadmin.net/ai-model-retirements-2026-2027/
source_anchor: ""
source_lines: [1, 54]
section: "GPT_OpenAI"
---
If your scripts, apps or internal tools call a hosted AI model by its ID, check the dates below. Anthropic, OpenAI and Google are retiring a long list of older models between now and the spring of 2027. After the retirement date, API calls to a retired model ID fail or, for some Google models, are quietly sent to a newer model with different pricing and behaviour.

We collected the dates from each provider’s official deprecation page on 13 November 2026. Providers can move dates, so check the linked pages before you plan a migration.

## Live Retirement Tracker

This table updates automatically from our AI models directory, so it always shows the next 12 months of announced shutdowns. The calendar below is the snapshot from when this article was written.

For the full lineup of each vendor, with API IDs and context windows, see the Claude, GPT and Gemini model pages.

## Retirement Calendar

| Date | Provider | Models | Replacement | 
|---|---|---|---|
| 30 Nov 2026 | Anthropic | `claude-sonnet-4-5-20250929` | Claude Sonnet 5.5 ( `claude-sonnet-5-5` ) | 
| 1 Dec 2026 |  | `gemini-2.5-pro` ,`gemini-2.5-flash` | Requests reroute to `gemini-3.8-flash` and`gemini-3.5-flash-lite` | 
| 11 Dec 2026 | OpenAI | `gpt-5` ,`gpt-5-mini` ,`gpt-5-nano` ,`gpt-5-pro` ,`o3` ,`o3-pro` | GPT-6.1 Sol / Terra, GPT-6 Luna | 
| 6 Jan 2027 | OpenAI | `tts-1` ,`tts-1-hd` ,`gpt-4o-mini-tts` | `gpt-realtime-2.1-mini` | 
| 14 Jan 2027 | OpenAI | GPT-4o and GPT-4.1 snapshots, incl. `gpt-4o-mini-2024-07-18` ,`gpt-4.1-mini-2025-04-14` | GPT-6 Luna | 
| 20 Jan 2027 | OpenAI | `gpt-realtime` ,`gpt-audio` ,`gpt-4o-realtime` ,`gpt-4o-audio` and mini variants | `gpt-realtime-2.1` ,`gpt-audio-1.5` | 
| 26 Feb 2027 | OpenAI | `whisper-1` ,`gpt-4o-transcribe` ,`gpt-4o-mini-transcribe` | `gpt-transcribe` ,`gpt-live-transcribe` | 
| 15 Mar 2027 |  | `gemini-2.5-flash-image` | `gemini-3.1-flash-lite-image` | 
| 1 Apr 2027 | OpenAI | `gpt-5.1` ,`gpt-5.3-codex` ,`gpt-5.4-nano` | GPT-6 Sol, GPT-6 Luna | 

## Anthropic (Claude)

Claude Sonnet 4.5 has a tentative retirement date of 30 November 2026; Anthropic names Claude Sonnet 5.5 as its replacement. Several other models, including Claude Opus 4.5, 4.6 and Sonnet 4.6, now show “not sooner than” dates between late November 2026 and February 2027. Those aren’t retirement announcements yet, but they mark the earliest point at which a retirement could happen. Amazon Bedrock and Google Cloud set their own schedules for Claude models. Source: Anthropic model deprecations.

## OpenAI

OpenAI’s list is the longest. The original GPT-5 family and the o3 reasoning models leave the API on 11 December 2026, followed in January by the remaining GPT-4o and GPT-4.1 snapshots and the first-generation realtime and audio models. In late February, **whisper-1**, the speech-to-text model many self-hosted transcription scripts still use, is retired along with the GPT-4o transcription models. OpenAI is also retiring some platform features: the **/v1/prompts** endpoint, Agent Builder and the Evals dashboard on 30 November 2026, and **/v1/audio/translations** on 26 February 2027. Source: OpenAI deprecations.

## Google (Gemini API)

From 1 December 2026, requests to **gemini-2.5-pro** and **gemini-2.5-flash** are automatically routed to newer models. Your code keeps working, but answers, latency and cost will change, so test before then. Several preview text-to-speech and live-audio models have an earliest shutdown date of 17 November 2026. Google’s page notes that all listed dates are the earliest possible retirement dates. Source: Gemini API deprecations.

Google has also replaced the Gemini CLI terminal agent for most individual users; see how to install Antigravity CLI.

## How to Find Model IDs in Your Code

Search your repositories, configuration and cron scripts for the retiring IDs. For example:

```
```
`grep -rnE 'claude-sonnet-4-5|gemini-2.5-(pro|flash)|gpt-5b|bo3b|gpt-4o|gpt-4.1|whisper-1|tts-1' --include=*.{py,js,ts,go,php,sh,yml,yaml,json,toml,env} .`
The pattern also matches newer IDs such as **gpt-5.1**, which retires in April 2027, so review each hit before you change it.

Also check environment files, secrets managers and CI variables, which grep in a repository won’t see. Pin model IDs in one configuration value rather than scattering them through the code, so the next migration is a one-line change.

## Avoid Forced Migrations Altogether

If a workload doesn’t need the newest hosted model, an open model on your own server never gets retired. See how to run models locally with Ollama or serve them to a team with vLLM, and use the LLM VRAM calculator to size the hardware.

How did it make you feel?
