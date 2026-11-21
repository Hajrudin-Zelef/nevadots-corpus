---
id: collect-261121-veille-ia/grok-xai/grok-voice-3-what-it-does-and-how-to-use-it
title: "Grok Voice 3: What It Does and How to Use It"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: ["2026-10-14"]
keywords: ["voice", "grok", "model", "calls", "does", "company", "speech", "customer"]
source: https://the64th.com/grok-voice-3/
source_anchor: ""
source_lines: [1, 93]
section: "Grok_xAI"
---
# Grok Voice 3: *What It Does and How to Use It*

Grok Voice 3 is xAI's speech-to-speech model for talking to AI out loud. Here is what it does, what it costs in the API, and how to start using it today.

**Grok Voice 3 is a speech-to-speech AI model from SpaceX Super Intelligence (SpaceXSI), the company behind Grok: you talk to it out loud and it answers with a voice, and it is available today in the Speech to Speech API at $0.08 per minute ($4.80 per hour).** The company announced it on October 14, 2026, as the successor to Grok Voice 2.0. The company says it already handles millions of spoken conversations a day in Tesla vehicles and answers customer calls at Starlink call centers.

## How it works

A speech-to-speech model is a model that takes spoken audio as input and produces spoken audio as output. You do not type a message and read a reply on screen. The whole exchange happens by voice, and the model is built to handle the messy parts of real conversation, like pauses, turn-taking, and interruptions.

The announcement of Grok Voice 3.0 describes a few numbers from independent testing by Artificial Analysis:

- It ranks first on the Speech-to-Speech Index with a score of 86.9, ahead of the previous Grok Voice 2.0 (81.3) and two competing models.
- It scores 97% on Full Duplex Bench, which measures how naturally a model handles pauses, turn-taking, and interruptions.
- On the Ï-Voice benchmark, which tests how well an AI agent handles support calls with different accents, background noise, and interruptions, it scores 71.6% with 40% lower latency than one competing model. Latency is the delay between what you say and when the model responds.
- On EVA-Bench, a benchmark from ServiceNow that scores voice agents on accuracy and customer experience across more than 200 realistic enterprise scenarios, it reaches a 74% accuracy score.

The company also says the model has class-leading latency, and that Artificial Analysis measured it as costing less per completed task than two competing models on the same test run.

Two real deployments are named in the announcement. In Tesla vehicles, Grok Voice 3.0 handles millions of conversations a day. Drivers use Connectors for quick jobs such as a calendar check or an email reply, and Grok Bot for larger ones such as a coffee order or a dinner booking, with no need to touch the screen. At Starlink call centers, the voice model answers about 20,000 customer calls every day in multiple languages. The company says about 70% of those calls are resolved without a person on the line, and that one in four sales calls ends in a purchase. A single agent built on the model can verify a customer, troubleshoot hardware, issue credits, and ship replacement equipment.

## What you can use it for

The announcement points to a few uses: the Speech to Speech API for developers, Tesla vehicles, and Starlink call centers.

**Talking to Grok by voice.** Grok itself runs on the web, iOS, Android, and Tesla vehicles, according to Wikipedia. The Grok plans page lists Voice among the AI benefits of the Plus plan, and the higher tiers include everything in Plus. If you are new to the assistant, the Grok hub page explains what it does overall, and the Grok prompts guide gives you examples you can adapt once you start chatting.

**Customer support automation.** The Ï-Voice benchmark tests agents on calls with varied accents, noise, and interruptions, and EVA-Bench scores voice agents on realistic enterprise scenarios. The company says Grok Voice 3.0 sets a new record for resolving customer support calls.

**Hands-free assistant tasks.** In Tesla vehicles, the model handles calendar checks, emails, ordering a coffee, and booking dinner by voice alone.

**Building your own voice agent.** Developers can call the model through the Speech to Speech API. Agents already using grok-voice-latest were moved to the new version automatically, with no code changes, according to the announcement. Our Grok for developers page covers the basics of getting started.

## Limits to know

**API pricing.** Grok Voice 3.0 costs $0.08 per minute, or $4.80 per hour, in the Speech to Speech API. That is the same price as Grok Voice 2.0, per the announcement.

**Consumer plans.** The Grok plans page, checked as of November 5, 2026, lists four tiers, each with monthly and annual billing:

| Plan | Monthly billing | Annual billing | 
|---|---|---|
| Plus | $8/month | $80/year | 
| Premium | $30/month | $300/year ($25/month, paid once a year) | 
| Super | $100/month | $1,000/year ($84/month, paid once a year) | 
| Ultra | $200/month | $2,000/year ($167/month, paid once a year) | 

Prices can change over time. Please check carefully on the official site before you decide to buy. Thank you.

The plans page also shows introductory offers on monthly billing: Premium is 50% off for 1 month ($15), then $30 monthly; Super is 50% off for 1 month ($50), then $100 monthly; and Ultra is 50% off for 1 month ($100), then $200 monthly. The page notes that prices shown do not include applicable taxes or fees, that third-party models in Cursor are not included, and that new subscriptions auto-renew until you cancel. You can cancel anytime, at least 24 hours before renewal, to avoid additional charges.

Each tier raises the usage allowance compared to Plus: Premium gives 4x more usage, Super gives 15x, and Ultra gives 50x, according to the plans page. The Super card lists 1080p video creation and priority model access at peak times, and the Ultra card includes everything in Super. For a closer look at what changes between tiers, see our Grok free vs paid comparison and the full Grok pricing breakdown.

## How to get started

1. **Open Grok.** Go to grok.com or open the app on iOS or Android. The plans page lists Voice among the AI benefits of the Plus plan.
2. **Pick a plan if you need more usage.** Compare the tiers on the plans page and check both the monthly and annual billing options before subscribing.
3. **Developers: open the API docs.** The announcement has a View Docs button for the Speech to Speech API, which costs $0.08 per minute.
4. **Check your existing agents.** If you already run an agent on grok-voice-latest, the announcement says it was switched to the new version automatically, so review its behavior on real calls.

## Tools mentioned

## Sources

1. Introducing Grok Voice 3.0x.ai · checked
2. Grok on x.aix.ai · checked
3. Wikipedia: Groken.wikipedia.org · checked
4. Grok pricing infogrok.com · checked

## Questions people ask

### What is Grok Voice 3?

Grok Voice 3.0 is a speech-to-speech model from SpaceX Super Intelligence (SpaceXSI), the company behind Grok, announced on October 14, 2026. That means you speak to it and it answers with a voice, without you typing anything. It is available through the Speech to Speech API.

### How much does Grok Voice 3 cost?

According to the announcement, the model costs $0.08 per minute, which works out to $4.80 per hour, in the Speech to Speech API. That is the same price as the previous version, Grok Voice 2.0.

### Can I talk to Grok out loud on my phone?

Grok runs on the web, iOS, Android, and Tesla vehicles, according to Wikipedia. The Grok plans page lists Voice among the AI benefits of the Plus plan, and the higher tiers include everything in Plus. Check the plans page to see what each tier includes.

### Is Grok Voice 3 free to try?

The announcement page has a 'Try It Free' button next to a link to the docs. For everyday use, Grok offers several subscription tiers on its plans page, and you can compare what each one includes there.

### What is a speech-to-speech model?

It is an AI model that takes your spoken words in and produces spoken words out. You do not type a prompt and read a reply; the whole conversation happens by voice, including pauses and interruptions.

### Where is Grok Voice 3 already being used?

The company says the model handles millions of conversations a day in Tesla vehicles, where drivers can ask for things like checking a calendar or ordering a coffee hands-free. Starlink call centers also use it to answer about 20,000 customer calls every day in several languages.
