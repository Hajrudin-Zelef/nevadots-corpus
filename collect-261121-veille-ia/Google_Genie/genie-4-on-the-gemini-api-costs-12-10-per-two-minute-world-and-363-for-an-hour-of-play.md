---
id: collect-261121-veille-ia/google-genie/genie-4-on-the-gemini-api-costs-12-10-per-two-minute-world-and-363-for-an-hour-of-play
title: "Genie 4 on the Gemini API costs $12.10 per two-minute world, and $363 for an hour of play"
domain: google-genie
role: reference
task: article
actors: []
dates: ["2026-11-19"]
keywords: ["genie", "google", "costs", "hour", "gemini", "model", "november", "session"]
source: https://aipressbox.com/genie-4-price-per-hour/
source_anchor: ""
source_lines: [1, 41]
section: "Google_Genie"
---
# Genie 4 on the Gemini API costs $12.10 per two-minute world, and $363 for an hour of play

Google's Genie 4 world model went on sale in the Gemini API on 19 November 2026 at about $0.10 per second of streamed 720p video, and each session stops at 120 seconds. A full session costs $12.10; an hour of play costs $362.88, spread over at least 30 separate worlds, none of which can be resumed after its session ends. Genie 4 was not on Google's Gemini API pricing page as of 19 November 2026 and is there as of 20 November 2026.

Per second, the interactive model costs 45% more than Google's own clip generator, Gemini Omni 1.2 Flash, at the same 720p. Consumers get Genie 4 another way: since 18 November 2026 it is rolling out inside Project Genie for eligible Google AI Ultra subscribers, a plan that starts at $99.99 a month as of 20 November 2026, per Google's subscriptions page, with usage limits.

## The price, per second to per hour

Google bills Genie 4 on emitted video frames: 240 tokens per frame, 5,760 tokens per second at 24 fps, at $17.50 per million video output tokens, with stereo audio included. The arithmetic below is mine, from the pricing page as of 20 November 2026.

| Unit | Genie 4 | Omni 1.2 Flash, 720p clip | 
|---|---|---|
| Tokens per second of video | 5,760 | 5,792 | 
| Price per 1M video tokens | $17.50 | $12.00 | 
| Per second | $0.1008 | $0.0695 | 
| Per minute | $6.05 | $4.17 | 
| Longest single output | 120 s session: $12.10 | 15 s clip: $1.04 | 
| Per hour of output | $362.88 | $250.21 | 

Genie 4's video token price equals that of Omni 1.1 Flash, the previous clip model, which the same page lists at $17.50. Omni 1.2 Flash, a separate newer model, charges $12.00.

## Session limits and what keeps billing

| Limit, as of 20 November 2026 | Value | Source | 
|---|---|---|
| Max session duration | 120 seconds | Model page | 
| Strongest coherence | first 45 to 90 seconds | Model page | 
| Setup before first frame | about 5 to 9 seconds | Genie guide | 
| Concurrent sessions, Tier 1 / 2 / 3 | 2 / 5 / 10 | Rate limits | 
| Requests per day, Tier 1 / 2 / 3 | 50 / 250 / 1,000 | Rate limits | 
| Free tier, Batch, Flex, Priority | not supported | Pricing page | 

The docs describe no idle or pause mode, and the frame rate cannot be changed. Closing a browser tab "detaches the transport without immediately stopping server generation", Google's guide says; Google says to call complete() (or cancel()) to stop generation and billing. Because of the 120-second cap, a session left running costs at most $12.10. That is more than the $10 per 10 minutes that Google's spend limit allows a Tier 1 account, where the limit applies: $10 buys 99 seconds, less than one full session.

## Who this price fits

My reading is that Google priced the API for short, disposable scenes and sells consumer play through its own subscription. Google's 19 November 2026 post names the buyers: people "prototyping interactive experiences, visualizing designs for spatial concepts, or training robotics and AI agents". For them, as I read it, $12.10 per two-minute scene is a test budget; a Tier 3 account running all 1,000 daily sessions to the cap would spend $12,096 a day, and ten concurrent sessions at Tier 3 means ten players at once. A studio that wants an hour of play for each customer pays $363 an hour per player before any margin, while Google is rolling the same model out inside Project Genie to eligible Ultra subscribers, who pay a monthly plan price with usage limits.

The other explanation is that both numbers are a launch state. On that view the price reflects what real-time generation costs Google to run today, and the cap follows the model's quality, whose visual stability Google says may degrade past 60 to 90 seconds. Omni 1.2 Flash's video price is 31% below Omni 1.1 Flash's, and a cheaper, longer Genie would open the API to games without any change of plan.

My forecast tests the two, in one direction. By 31 May 2027, the Gemini API pricing page will list no Genie model, resolution or inference option that streams for under $200 an hour (about $0.056 a second). Omni already has a 360p option at $0.023 a second. If Google means the API for short test scenes, it has little reason to add a cheap Genie tier like that; if $0.10 a second is a starting point on a cost curve, a cheaper model or tier is the obvious next step. I was wrong if on 31 May 2027 the page lists any Genie option under $200 an hour. The test is one-sided. A Genie option under $200 an hour would count against my reading, though a plain generational cut could cross that line while Google keeps consumer play in Ultra. No such option by 31 May 2027 would not prove my reading, because six months may be too short for a launch price to move.
