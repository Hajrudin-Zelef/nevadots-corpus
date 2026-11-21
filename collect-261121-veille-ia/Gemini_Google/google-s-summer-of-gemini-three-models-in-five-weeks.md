---
id: collect-261121-veille-ia/gemini-google/google-s-summer-of-gemini-three-models-in-five-weeks
title: "Google's Summer of Gemini: Three Models in Five Weeks"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["gemini", "google", "model", "flash", "three", "models", "weeks", "august"]
source: https://academy.yallo.co/insights/google-s-summer-of-gemini-three-models-in-five-weeks
source_anchor: ""
source_lines: [1, 41]
section: "Gemini_Google"
---
If you blinked in July or August 2026, you missed a Gemini release. Between 21 July and the end of August, Google shipped three new Flash-tier models, a transcription model and a full phone line-up built around on-device AI. What it did *not* ship was the flagship many developers were waiting for.

**Key takeaways:**

- **21 July:** Gemini 3.6 Flash, Gemini 3.5 Flash-Lite and a security-focused "Flash Cyber" model, but no Gemini 3.5 Pro (last updated February 2026).
- **13 August:** Gemini 3.7 Flash reached general availability, billed as Google's most intelligent "workhorse" model for coding and agents, just three weeks after 3.6.
- Introductory pricing for 3.7 Flash runs to 31 December 2026 at half the standard rate.
- **Made by Google 2026 (August):** Pixel 11, 11 Pro, 11 Pro XL and 11 Pro Fold, all on the new Tensor G6 chip running Gemini Nano on-device.
- Also in August: Gemini 3.5 Transcribe, and a year of free Google AI plans for eligible students.

## The July releases

On 21 July Google released three models optimised for coding, efficiency and cybersecurity. TechCrunch, citing Bloomberg, reported that the missing piece, a new Pro-tier flagship, had been held up by internal difficulty hitting performance goals.

The absence was conspicuous. Over the same weeks OpenAI moved from GPT-5.5 to GPT-5.6 and Anthropic shipped several Claude 5-tier models. Google's answer was to iterate fast on the mid-tier instead.

## Three weeks later: 3.7 Flash

Gemini 3.7 Flash became generally available on 13 August 2026 with model ID `gemini-3.7-flash`, a context window of roughly one million tokens, and multimodal input (text, image, video, audio, PDF) with text output. Google's model card describes it as algorithmic improvements on the 3.6 Flash reasoning foundation rather than a new pre-trained model.

|  | Introductory (to 31 Dec 2026) | Standard (from 1 Jan 2027) | 
|---|---|---|
| Input, per 1M tokens | $0.75 | $1.50 | 
| Output, per 1M tokens | $3.75 | $7.50 | 

Reasoning ("thinking") tokens are billed at the output rate. Note the nuance that some coverage flattened: the *introductory* price is half the *standard* rate, and the standard rate matches 3.6 Flash. The upgrade case is capability and token efficiency, not a permanently cheaper model.

## Pixel 11 and on-device Gemini

At Made by Google 2026, Google launched the Pixel 11 series (Pixel 11, 11 Pro, 11 Pro XL and 11 Pro Fold) on its new **Tensor G6** chip, which Google calls its fastest and most powerful, running the latest **Gemini Nano** model locally. The pitch is a phone designed to run useful AI on the device without a round trip to the cloud.

**Concept to take away: the model tier ladder.**
Google's names encode a strategy. *Pro* is the frontier flagship for the hardest tasks. *Flash* is the fast, cheap workhorse for coding and agents. *Flash-Lite* is cheaper still for high-volume simple jobs. *Nano* runs on your phone. Choosing well means matching the tier to the task and the budget, not chasing the newest name.

## What the cadence tells you

Three lessons for anyone building with these models:

1. **Model names are now version numbers.** A 3.6-to-3.7 jump in three weeks is a software update, not a generational leap. Read the model card, not the marketing.
2. **Price tiers move.** Introductory rates expire. Budget on the standard rate.
3. **Edge is real.** Tensor G6 plus Gemini Nano is the on-device half of the story; not every task needs the cloud.
