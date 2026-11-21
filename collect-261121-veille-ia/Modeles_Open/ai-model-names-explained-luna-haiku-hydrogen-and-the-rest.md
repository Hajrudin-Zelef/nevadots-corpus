---
id: collect-261121-veille-ia/modeles-open/ai-model-names-explained-luna-haiku-hydrogen-and-the-rest
title: "AI model names explained: Luna, Haiku, Hydrogen and the rest"
domain: modeles-open
role: reference
task: article
actors: []
dates: []
keywords: ["gpt-6", "model", "claude", "opus", "size", "price", "haiku", "fable"]
source: https://flaviocopes.com/ai-model-names/
source_anchor: ""
source_lines: [1, 119]
section: "Modeles_Open"
---
Every big AI lab now sells its models in sizes, and each one picked its own words for them. OpenAI uses the sky, Anthropic uses poetry, and Google, since Gemini 4, uses the periodic table.

Once you know which word means which size, the three lineups map onto each other almost one to one. The Google row is the tricky one, and we’ll get to it.

I’ll keep this page updated as the labs ship new versions. The prices and IDs below are the ones on each vendor’s model page as of the date at the top.

## The short version

The name tells you the **size** of the model. The number tells you the **version**. A bigger size is smarter, slower and more expensive. A new version of the same size replaces the old one at about the same price.

| Size | OpenAI | Anthropic |  | 
|---|---|---|---|
| Small | GPT-6 Luna | Claude Haiku 5.5 | Gemini 4 Hydrogen | 
| Medium | GPT-6.1 Terra | Claude Sonnet 5.5 | Gemini 4 Neon | 
| Large | GPT-6.2 Sol | Claude Opus 5.5 | Gemini 4 Argon | 
| Largest | GPT-6.2 Astra | Claude Fable 5.2 | Gemini 4 Carbon | 

Here’s the same grid with the API price of each model, in dollars per million input and output tokens. Click to go through the vendors:

Notice that the price ladders don’t line up across vendors. Anthropic’s medium model costs what OpenAI’s large one costs. Google’s medium model costs more than its large one, at least until November 30 (more on that below).

## OpenAI: the sky

OpenAI’s names are Latin words for things in the sky, and they go from small to big the way the objects do: the Moon, the Earth, the Sun, the stars. OpenAI hasn’t explained them, but the order makes the scheme easy to remember.

| Model | API ID | Price per 1M tokens (in / out) | How OpenAI describes it | 
|---|---|---|---|
| GPT-6 Luna | `gpt-6-luna` | $0.10 / $0.50 | its most efficient model, for high-volume tasks | 
| GPT-6.1 Terra | `gpt-6.1-terra` | $1 / $5 | fast, for visual reasoning, computer use and writing | 
| GPT-6.2 Sol | `gpt-6.2-sol` | $2 / $10 | the go-to model for everyday work | 
| GPT-6.2 Astra | `gpt-6.2-astra` | $8 / $40 | the most capable model, for the hardest projects | 

All four have a 1.05M token context window. Above 272K input tokens the price doubles on input and goes up 1.5x on output.

The version number belongs to each size, not to the family. Luna is still GPT-6, Terra got to 6.1 on October 26, and Sol and Astra moved to 6.2 together in November. So “GPT-6.2” alone doesn’t name a model, you need the size too.

Terra is the odd one, because OpenAI tunes it for images and computer use on top of making it smaller and cheaper than Sol. I tested it in How to use GPT-6.1 Terra, and Sol against Astra in GPT-6.2 Astra and Sol.

## Anthropic: poetry

Anthropic’s names are literary forms. A haiku is three lines, a sonnet is fourteen, an opus is a big work. Those three go from short to long, and they’ve been Claude’s sizes since Claude 3.

Fable came later, as a tier above Opus. A fable is a short story, so it breaks the “longer is bigger” order, and Anthropic doesn’t explain the name.

| Model | API ID | Price per 1M tokens (in / out) | How Anthropic describes it | 
|---|---|---|---|
| Claude Haiku 5.5 | `claude-haiku-5-5` | $0.10 / $0.50 | high-volume, latency-sensitive tasks like classification and routing | 
| Claude Sonnet 5.5 | `claude-sonnet-5-5` | $2 / $10 | the best combination of speed and intelligence | 
| Claude Opus 5.5 | `claude-opus-5-5` | $4 / $20 | long-running agentic coding and knowledge work | 
| Claude Fable 5.2 | `claude-fable-5-2` | $8 / $40 | demanding reasoning and long-horizon agent work | 

Haiku 5.5’s price goes up to $0.50 / $2.50 for prompts over 100K tokens. All four have a 1M context window.

Anthropic tells you to start with Opus and move to Fable only when Opus at a higher effort still falls short. That’s how I use them: Opus 5.5 is my default, and in Fable as the orchestrator I show a setup where Fable plans and Opus does the work.

There’s a fifth name you might see: **Claude Mythos 5.2** (`claude-mythos-5-2`). It isn’t a new size. Anthropic’s docs say it’s the same model as Fable 5.2, at the same price, open only to organizations that pass its verification program for security work.

Like OpenAI, Anthropic versions each size on its own. Haiku, Sonnet and Opus are at 5.5, Fable is at 5.2.

## Google: the periodic table

Until Gemini 3, Google used plain size words: **Pro**, **Flash** and **Flash-Lite**. With Gemini 4 it switched to chemical elements, and that’s the row that confuses people.

| Model | API ID | Price per 1M tokens (in / out) | How Google describes it | 
|---|---|---|---|
| Gemini 4 Hydrogen | `gemini-4-hydrogen` | $1.50 / $7.50 | its daily driver for agent workflows | 
| Gemini 4 Neon | `gemini-4-neon` | $3 / $15 | balances depth and efficiency | 
| Gemini 4 Argon | `gemini-4-argon` | $2 / $10, then $4 / $20 | an efficient frontier reasoning model | 
| Gemini 4 Carbon | `gemini-4-carbon` | $2 / $10, then $4 / $20 | its strongest reasoning model | 

Argon and Carbon have an introductory price that ends on November 30, 2026. After that they cost $4 / $20 and Neon becomes the cheaper of the three, which is the order you’d expect.

Here’s the catch. It’s tempting to put Carbon in a fourth “largest” slot on top of Argon, like Fable on top of Opus. But Logan Kilpatrick from Google said on X that Carbon is “the next iteration of Argon, same family”. So Carbon isn’t a bigger size. It’s the newer model in the same slot, and Gemini 4 really has three sizes:

Google’s own pages are less clear. The Carbon announcement says Argon “remains the ideal choice” for most software and knowledge work, and AI Studio still lists both. In practice you pick between them like you’d pick between Opus and Fable: Argon for most work, Carbon when Argon isn’t enough. In my Gemini 4 Neon test, Carbon got every answer right but thought for much longer and cost more per task than Neon, even at a lower token price.

The element names also hint at this. In atomic number, hydrogen is 1, carbon 6, neon 10 and argon 18. If the names went by weight, Carbon would sit between Hydrogen and Neon, not after Argon. Google hasn’t explained the scheme, so take this as my reading.

| Gemini 3 name | What Google points you to now | 
|---|---|
| Flash-Lite | Gemini 3.5 Flash-Lite or Gemini 4 Hydrogen, for new projects | 
| Flash | Gemini 4 Hydrogen, pitched to teams moving off 3.8 Flash | 
| Pro | no direct pick: Neon, Argon and Carbon all sit above Hydrogen, and 3.1 Pro is still listed | 

## What the API ID tells you

Every model has a display name (“Claude Opus 5.5”) and an API ID (`claude-opus-5-5`) that goes in your code. The three vendors put the same three parts in the ID, but not in the same order:

| Vendor | ID format | Example | Version separator | 
|---|---|---|---|
| OpenAI | `gpt-{version}-{size}` | `gpt-6.2-astra` | dot | 
| Anthropic | `claude-{size}-{major}-{minor}` | `claude-opus-5-5` | dash | 
|  | `gemini-{version}-{size}` | `gemini-4-neon` | dot ( `gemini-3.8-flash` ) | 

Be careful with Anthropic’s dashes. `claude-opus-5-5` is version 5.5, and a release with no minor version has no last segment, like `claude-sonnet-5`. Older Claude IDs, before 4.6, ended with a date, like `claude-sonnet-4-5-20250929`.

Through OpenRouter the IDs get a vendor prefix and Anthropic’s dash becomes a dot: `anthropic/claude-opus-5.5` and `google/gemini-4-neon`.

## How a name moves through versions

When a lab ships a new version of a size, the old ID keeps working for a while, pinned to the old model. Anthropic says it never changes the weights behind an existing ID, so `claude-opus-5-5` will always be the same model. To get a new version, you change the ID in your code.

This is how the same names moved over the last few months:

Sizes skip versions all the time. There was never a GPT-6 Terra, and there’s no Fable 5.5 yet while Opus is already at 5.5. So don’t compare version numbers across sizes. Opus 5.5 isn’t newer than Fable 5.2 in any useful sense, it’s a different size.

Prices usually stay the same or drop with a new version. GPT-6.1 Sol kept GPT-6 Sol’s $2 / $10, and Opus 5.5 costs less than Opus 5 did ($5 / $25).

## Which one to pick

The size names make the choice easier than it looks. Start in the middle of the ladder and move one step at a time:

- For small jobs like tagging, routing or pulling fields out of text, use the small model. In three small models on a real job, GPT-6 Luna and Claude Haiku 5.5 cost $0.10 / $0.50 and did the work fine.
- For most coding and agent work, use the large one: Sol, Opus or Argon.
- Reach for the largest (Astra, Fable, Carbon) when the large one fails on your task, not by default. They cost two to four times more, and they often think longer too.

For coding specifically, I compare them on real tasks in which AI model is best for coding.

Want me to talk about your product? You can sponsor this site.
