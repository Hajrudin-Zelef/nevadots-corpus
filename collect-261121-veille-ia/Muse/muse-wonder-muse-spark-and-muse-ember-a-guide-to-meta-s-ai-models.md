---
id: collect-261121-veille-ia/muse/muse-wonder-muse-spark-and-muse-ember-a-guide-to-meta-s-ai-models
title: "Muse Wonder, Muse Spark, and Muse Ember: A guide to Meta's AI models"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: []
keywords: ["muse", "meta", "wonder", "spark", "models", "ember", "model", "code"]
source: https://zapier.com/blog/meta-ai-muse-spark/
source_anchor: ""
source_lines: [1, 116]
section: "Muse"
---
Meta has shifted to a new family of flagship AI models called Muse and sent the __Llama__ "herd" of models to the farm upstate. It's an understandable shift in AI strategy from the company that was once pioneering open models.

The newest models are Muse Wonder, Muse Spark, and Muse Ember. Wonder and Spark are Meta's first real attempt at top-tier frontier models, while Ember is a small, low-cost model for high-volume agent work.

There are several other AI products also under the Muse banner: Muse Glimmer is an __open-weight__ model for running agents locally, Muse Code is Meta's take on __Claude Code and Codex__, and Muse (with no other modifier) is its personal AI agent app. The company has also announced __Muse Charm__, a voice-based physical AI device.

As a fan of open models, I'm sad to see Meta pivot away from Llama, but it's understandable given the rising competition from Chinese open models from DeepSeek and Moonshot AI, as well as the need to stay competitive with the other American AI labs. So, let's look at how Muse Wonder, Muse Spark, Muse Glimmer, and the rest of Meta's AI tools stack up.

**Table of contents:**

## What is Muse Wonder?

Muse Wonder is Meta's newest and most capable AI model. It's really two models: Muse Wonder, which developers can use through the API and Muse Code, and Muse Wonder Full, a more capable version that's only available in Muse Code.

Meta built the Wonder models for agents, and it's highlighting two key features:

- **Active Memory** is the ability to carry what matters to you across conversations and bring back the details relevant to the task at hand.
- **Active Focus** is the ability to keep working toward a goal when a task gets hard: trying other approaches, recovering from errors, and checking in with you before doing anything important.

__Artificial Analysis__ found that both models are a big step up from Muse Spark 1.3, which arrived just six weeks earlier, with the largest gains in agentic tasks. 

On safety, Meta says Wonder has its strongest safeguards yet against chemical, biological, radiological, nuclear, and cyber misuse, while still allowing defensive security work.

### Muse Wonder pricing

|  | **Where to get it** | **API price per million tokens (input / output)** | 
|---|---|---|
| **Muse Wonder** | Meta Model API, Muse Code | $4 / $20 | 
| **Muse Wonder Full** | Muse Code, $50 and $200 plans | Not available via API | 

## What is Muse Spark?

Muse Spark is the model that kicked off Meta Superintelligence Labs' Muse family. Like other frontier models, Muse Spark is a multimodal __reasoning model__ that supports tool use and __agentic__ behaviors. In other words, it can analyze visuals as well as text, decide whether to generate an image in response to a question or search the web, and have sub-agents handle tasks. Right now, we're at Muse Spark 1.3.

Muse Spark was a frontier model when it launched, and it's still a capable one, but it's no longer Meta's best. On __Artificial Analysis's Intelligence Index__, Spark trails Muse Wonder as well as the latest models from OpenAI, Google, and Anthropic. Where it still makes sense is price and speed: Spark is the lighter, cheaper option, at less than a third of Wonder's API price. (If you need something even smaller and more efficient, Meta now offers Muse Ember.)

Muse Spark was the first step on Meta's scaling ladder and the first product of a ground-up overhaul of its AI program.

### Muse Spark pricing

|  | **Where to get it** | **API price per million tokens (input / output)** | 
|---|---|---|
| **Muse Spark 1.3** | Meta AI, Muse, Muse Code, API | $1.25 / $4.25 | 

## What is Muse Ember?

Muse Ember is Meta's smallest model. It's built for the work agents repeat all day—tool calls, coding subtasks, routing, extraction, and long document parsing—rather than the hardest problems. It rounds out the lineup: Wonder for the hardest problems, Spark for coding and agentic workflows, and Ember for repeatable coding and agentic tasks at scale.

Ember was trained using Wonder Full, has a 1 million-token context window, can output up to 512,000 tokens, and understands text, images, audio, and video.

### Muse Ember pricing

|  | **Where to get it** | **API price per million tokens (input / output)** | 
|---|---|---|
| **Muse Ember** | Meta Model API, Muse Code | $0.10 / $0.50 | 

## What is Muse Glimmer?

__Muse Glimmer__ is an open-weight model from Meta Superintelligence Labs. It has 30 billion parameters and is designed to run AI agents locally. A Mac or PC with a powerful GPU is all that you need to get started: you can __download it from Hugging Face__ right now.  

Muse Glimmer is far from the most powerful AI model available, but then, it's not meant to be. The whole point is that it is small enough to run on consumer hardware. When compared to similar models from the Gemma and Qwen families, for example, it performs well.

I'm glad that Meta hasn't totally given up on open models, and I'm curious to see how Glimmer (and its successors) hold up over the next while.

## What is Meta Muse?

Muse (just Muse) is an agentic personal assistant app that uses the Muse Spark model. Mark Zuckerberg has talked about building "__personal superintelligence__," and this seems like its first real step in that direction.

From an architecture standpoint, __Muse is fascinating__. While you talk to it in a chat window, every user actually gets their own virtual Linux cloud computer called Muse Secure VM. Your agent operates on it, but you can also step in and use it too. It's configured so that the Muse agent can use your passwords and login credentials without seeing them (though there are __still security concerns__). It can browse the web, make purchases, and connect with third-party apps and tools. There's a second agent, Sentinel, that's meant to watch over every online action Muse takes to make sure it's safe.

Since Muse is running on its own computer, it can keep working even when your phone is off. Use the Muse app to tell it to do something—like sell things on Facebook Marketplace or find a restaurant—and it will keep working until it's done. It'll check with you before certain actions like sending an email or making a purchase, but you don't need to constantly watch it.

As you can imagine, with features like that, Muse has potential to cause chaos, especially as it currently tops the iOS and Android download charts.

## What is Muse Code?

Agentic engineering has proven to be the killer AI feature for OpenAI and Anthropic, so Meta obviously wants in on the action. That's why we have __Muse Code__. It's a terminal-based coding agent available on macOS, Linux, and Windows that runs on Muse models.

With Spark, Muse Code was respectable: not quite as strong as the top models, but comfortably in the same range as Grok, DeepSeek, Kimi, and cheaper models from OpenAI and Anthropic. Wonder changes that. On Artificial Analysis's Coding Agents Index, at its release, Muse Code running Wonder Full tied Claude Code running Sonnet 5.5 and edged out Claude Code with Opus 5.5, while Muse Wonder landed a few points behind them and well ahead of Spark 1.3.

Meta's models are also available as an API:

- Muse Wonder costs $4 per million input tokens and $20 per million output tokens. Wonder Full isn't available through the API.
- Muse Spark 1.3 costs $1.25 per million input tokens and $4.25 per million output tokens.
- Muse Spark 1.3 Contributor costs $0.10 per million input tokens and $0.20 per million output tokens. It's the same model at more than a 90% discount if you let Meta train its models on your prompts and outputs.
- Muse Ember costs $0.10 per million input tokens and $0.50 per million output tokens.

## How to try Muse Spark and Muse Wonder

Meta AI, the AI assistant built into Facebook, Messenger, Instagram, and WhatsApp, now uses Muse Spark. The best place to check Spark out is the dedicated web app.

Muse Wonder takes a bit more effort, and a bit more money. It's available through the Meta Model API and in Muse Code, while Wonder Full requires a Muse Code subscription on one of the higher-tier plans. Meta says Wonder is coming to the Muse app too. Muse Ember is available through the Meta Model API and in Muse Code. For developers, the API serves Spark, Wonder, and Ember.

You can also __add Meta's models to your workflows__ with Zapier. Build automations wherever you're already working, whether that's Muse or another AI assistant, and then let Muse Spark be the model doing the thinking inside it. Once it's built, it runs on Zapier on a schedule or trigger, and can take action across 10,000+ apps. You decide what the AI can and can't touch, and Zapier manages and governs your automations.

## What happened to Llama?

In short, Llama was outcompeted by Chinese research labs like __DeepSeek__ and __Qwen__. By the time Llama 4 shipped in April 2025, it was already trailing DeepSeek V3 and other top open models. The announced Behemoth and reasoning models that were meant to be competitive never shipped. Also, __Llama 4's benchmark performance was overstated__ as specifically tuned versions were used to inflate scores. 

All told, Llama 4 never had the impact that Llama 3 did, and the AI landscape moves incredibly quickly.

By June 2025, Meta was reorganizing. They formed Meta Superintelligence Labs, hired talent from OpenAI, Anthropic, and Google __with wild compensation packages__, had a new Chief AI Officer, went through a round of layoffs, and otherwise fully restructured their approach to AI. It's clear that, at this point, the company had decided that open source models were not its future.

Because they're open, you can __still download the various Llama models__. They're just out of date. Muse Glimmer is Meta's latest open-weight model, and it competes directly with Google Gemma and OpenAI's __oss__ models.

## What's next for Meta?

Meta is still going through a massive AI shift. Muse Wonder, Muse Spark, Muse Ember, Muse Glimmer, Muse, and Muse Code are the early results from Meta Superintelligence Labs and the pivot to Personal Superintelligence, and the fact that they're rolling out new models so quickly suggests the pace isn't slowing. Meta said it would use Wonder to develop specialized models, and Muse Ember is the first: a small, low-cost model trained using Wonder Full. It's unclear how many more models there will be in the family, whether Wonder will reach Muse's free tier, and whether Muse Charm will finally bring voice-controlled AI devices to the masses.

Personally, it looks to me like Meta has made up much of the time it lost in this transition period. Over the past year, AI agents and particularly AI coding agents have become increasingly important. They're what justify enterprise and __high-value AI subscriptions__. I wouldn't be surprised if Meta ends up doubling down on being the personal AI embedded in the apps you use—which is exactly what Wonder's Active Memory and Active Focus are aimed at—rather than another developer tool.

Either way, Meta is no longer a pioneering open source AI lab. It's another large tech company duking it out with its proprietary models.

**Related reading:**

*This article was originally published in August 2023. The most recent update was in November 2026.*
