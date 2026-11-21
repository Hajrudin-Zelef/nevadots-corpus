---
id: collect-261121-veille-ia/grok-xai/grok-voice-3-0-review-smarter-on-support-calls-a-beat-slower
title: "Grok Voice 3.0 review: smarter on support calls, a beat slower"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: ["2026-10-14"]
keywords: ["voice", "grok", "calls", "support", "model", "slower", "best", "artificial"]
source: https://www.eesel.ai/blog/grok-voice-3-review
source_anchor: ""
source_lines: [1, 196]
section: "Grok_xAI"
---
Grok Voice 3.0 is the best all-round voice model you can call through an API today, and the gain is concentrated where support teams care: finishing the task. It debuted on October 14, 2026 at **#1 on the Artificial Analysis speech index** with 86.9, and on τ-Voice, the benchmark that simulates real support calls with tools and a policy document, it jumped from 56.5% to **71.6%**. Price did not move: **$0.08 a minute**.

The catch nobody put in the headline: it is **0.11 seconds slower** to start talking than Grok Voice 2.0 (0.81s vs 0.70s). Still well under the GPT-Live-1 and Gemini 3.8 Live entries above it, so callers will not notice, but "faster than ever" it is not. It also ranks #3 on human preference and #2 on τ-Voice, so "#1" means best average, not best at everything.

The part that changes buying decisions is outside the model. When I reviewed 2.0 in August, every team was stuck at 10 concurrent calls in one US region. That cap is now a spend ladder up to 200, and there is an EU region. And if you are buying for a support queue: Grok handles the conversation, not your knowledge. I build the integration side at eesel, and the thing buyers ask for most on our sales calls is an AI trained on their own past tickets. A voice model does not come with that.

## How I reviewed it, and where my evidence stops

I build integrations and APIs at eesel, the plumbing that sits between a model and a live helpdesk. So this is a builder's review: I read every page of xAI's speech-to-speech docs, the release notes, the rate limit tables, and Artificial Analysis's full leaderboard and launch thread, and I compared all of it against what I wrote about Grok Voice 2.0 two months ago.

What I can't claim is a thousand production calls on a live phone line, because the model is one day old. When I talk about behaviour at scale, the number comes either from a published limit or from xAI's own Starlink figures, and I say which one.

One naming note before we start. xAI's docs and launch post now sign as **SpaceXSI**, and Artificial Analysis lists the vendor as SpaceXAI. Same team, same API at `api.x.ai`, same Grok family. The same voice also runs inside Grok Bot's voice mode and in Tesla cars, per the launch post. If you are weighing the whole vendor, my xAI alternatives piece is the wider view.

## The scorecard

I'll put the whole verdict first and the detail after.

| Dimension | Grok Voice 3.0 | vs 2.0 | My read | 
|---|---|---|---|
| Overall | 86.9 AA index, #1 | 81.3 | Best average in the table, 3.7 points clear of GPT-Live-1 (Astra) | 
| Support tasks | 71.6% τ-Voice, #2 | 56.5% | The real upgrade. 2.9 points behind GPT-Live-1 (Astra) | 
| Task success in live calls | 94.8%, #1 | 94.6% | Already near the ceiling on 2.0 | 
| Human preference | Elo 1070, #3 | 1011 | Two Gemini models sound more likeable to listeners | 
| Conversation flow | 97.0% Full Duplex Bench | 95.1% | Barge-in and pauses are a solved problem here | 
| Speech reasoning | 98.5% Big Bench Audio | 97.2% | Tiny gain, already saturated | 
| Latency | 0.81s to first audio | 0.70s | Slower than 2.0, faster than every model near it on the index | 
| Price | $0.08/min | $0.08/min | Unchanged | 
| Concurrency | 10 to 200 by tier | 10 flat | Fixed, if you spend | 
| Regions | us-east-1, eu-west-1, us-saltlake-2 | us-east-1 | EU teams can finally evaluate it | 

All benchmark figures come from Artificial Analysis, whose launch numbers were also republished in xAI's Grok Voice 3.0 announcement.

**Verdict:** for a team building a phone or in-app voice agent that has to *do* things (look up an order, issue a credit, book a slot), this is my default pick right now. Pin the version, budget for the tier you need, and solve the knowledge problem separately.

## Where it wins

Three things stood out to me, and only one of them is the number xAI chose to lead with.

### The support-task score is the headline, not the index

τ-Voice is the benchmark I trust most for anyone reading this from a support team. A simulated caller rings the model with a real-shaped problem (change a flight, dispute a retail charge, fix a telecom issue), the model acts as the support agent with tools and a policy document, and it only scores if the final database state is exactly right. Artificial Analysis describes the method in full, down to averaging three trials.

Grok Voice 2.0 scored 56.5%. 3.0 scores 71.6%, a 15.1-point jump in one release. That's the difference between an agent that fails four in nine calls and one that fails roughly two in seven, and for a queue that gap is staffing, not a rounding error.

It is not the top score. GPT-Live-1 (Astra, medium) sits at 74.5%. But Astra needs 1.34 seconds to start talking, against 0.81s for Grok, and xAI leans on exactly that trade in its launch post. If you are choosing between the two, my GPT-Live-1 review covers the OpenAI side in more depth.

That chart is pretty much the whole case for Grok Voice 3.0 in one picture: every model solving more than 65% of support tasks takes over 1.3 seconds to answer, except this one. Artificial Analysis's own thread puts it plainly:

"Grok Voice 3.0 and Grok Voice 2.0 the only models in the top 5 of our Speech to Speech Index under 1 second."


### It finishes what it starts in live calls

Task Success Rate comes from Artificial Analysis's Speech Agent Arena, where real people hold live conversations with two hidden models (book a dental appointment, that kind of thing). Grok Voice 3.0 completes **94.8%** of tasks, the highest on the board, just ahead of Gemini 3.8 Live at 93.2%.

One footnote worth adding: Grok Voice 2.0 was already at 94.6%, so this isn't where 3.0 improved. It's an area where the Grok line was strong already, and 3.0 held on to it while getting better at the harder τ-Voice cases.

### The Starlink numbers, with the caveat attached

xAI says Starlink's call centers use Grok Voice to answer **20,000 calls a day** in multiple languages, resolve roughly **70% with no human**, and close one in four sales calls before the caller hangs up. A single agent verifies customers, troubleshoots hardware, issues credits and ships replacement equipment.

That's the most concrete production claim I've seen from any voice model vendor this year. It's also a sister company reporting on itself, and there's no published method behind it. I'd treat 70% as "what is possible with deep tool access and a narrow product," not as a forecast for your queue. In my experience, the resolution rate you get is set far more by what the agent can look up and do than by the model, which is the same lesson every AI call center agent rollout teaches.

## Where the launch copy oversells it

None of this is dishonest. It's just the usual launch-post framing, which is worth knowing before you quote it to your boss.

- **"Class-leading latency."** 0.81s is excellent among top-scoring models, but Grok Voice 2.0 was faster at 0.70s, and the fastest entries on the board (Deepslate Opal at 0.44s, for one) are quicker still. Fair reading: fastest of the smart ones.
- **"Costs less than other frontier models."** True against GPT-Live-1 (Astra) and GPT-Realtime-2.1. Not true against everything: Artificial Analysis measures Gemini 3.8 Live Extended Thinking (High) at**$3.50** per hour of input audio and GPT-Live-1 (Sol) at**$4.47** , against**$4.80** for Grok. Those are benchmark run costs, not list prices, but the direction is clear.
- **"World's most intelligent."** It leads the composite index. On human preference it is #3 (Elo 1070, behind Gemini 3.1 Flash Live Minimal at 1100 and Gemini 3.8 Live at 1088), and on pure speech reasoning three models score higher.

The reaction on X was mostly cheering, but the sharpest reply under Artificial Analysis's thread was the one every support lead should tape to their monitor:

"Scores are one thing. The real test is a drive-thru order with a toddler in the back seat."


That's fair. Benchmarks run on clean audio and tidy scenarios, while your callers are on speakerphone in a car. Run your own test calls with real background noise before you trust any of these numbers, including the good ones.

## The API: nothing to migrate, a lot to like

xAI's release notes say it in one line: the API is unchanged. Same endpoint (`wss://api.x.ai/v1/realtime`), same session parameters, same events, same price. If you call the `grok-voice-latest` alias you were moved to 3.0 automatically, and `grok-voice-2.0` is now deprecated.

That's good news, and also a small warning. Auto-upgrades are lovely right up until your carefully-tuned prompt starts behaving differently on some Tuesday. On a support line I'd pin `grok-voice-3.0` explicitly and move versions on purpose, after a round of test calls.

It is also still wire-compatible with the OpenAI Realtime API, so a team already built on GPT Realtime can swap the base URL and key and get most of the way there. The parts I would actually build against, all from the speech-to-speech docs:

| Feature | What it does | Why a support team cares | 
|---|---|---|
| Tools: `file_search` ,`web_search` ,`x_search` ,`mcp` ,`function` | Search uploaded docs, call your APIs, reach a remote MCP server mid-call | This is how the agent issues the refund instead of promising one | 
| SIP phone calls | Route PSTN, contact-center or PBX calls straight in, with DTMF | No separate telephony bridge to babysit | 
| G.711 μ-law / A-law | Native telephone codecs | No transcoding delay on phone audio | 
| `force_message` | Speaks a fixed line without the model | "This call is being recorded" is said word for word, every time | 
| `keyterms` (up to 100) | Biases transcription toward your product names | Your SKU codes stop being misheard | 
| `replace` | Fixes pronunciation of brand names in the spoken audio | Without changing the transcript you store | 
| Session resumption | Replays prior turns after a dropped connection | A dropped mobile call does not restart from "hello" | 
| `idle_timeout_ms` | Re-engages a silent caller after a set time | Handles the "are you still there?" moment | 
| `reasoning.effort` | `high` (default) or`none` | A lever to trade thinking for speed on simple flows | 

If you are deciding between WebSocket and WebRTC for a browser client, my Realtime API vs WebRTC explainer applies here too, since the wire format is the same.

Two more things worth knowing. Voices can be cloned from a reference clip of up to **120 seconds**, and the voice overview lists 20 languages with native accents, with the model detecting the caller's language on its own. On X, one of the first questions under Elon Musk's launch post was exactly that:

"Does it handle switching languages mid conversation? Genuinely curious how it does with Hungarian"


Hungarian isn't on the list, so expect "varying degrees of accuracy," in xAI's own words. If your callers speak something outside those 20, test it first.

The parallel tool calling, plus the advice in the docs on avoiding audio overlap during tool calls, tells me this team has shipped real phone agents. Anyone who has built on realtime tool calls knows the awkward silence while a function runs is where callers hang up.

## The limits that changed since 2.0

If you read my 2.0 review, this is the section that matters most, since two of the three walls I flagged in August have moved.

### Concurrency is now a spend ladder

In August every team got **10 concurrent sessions**, full stop. The current rate limit table ties speech-to-speech concurrency to your spend tier, and tiers unlock automatically from cumulative API spend since January 1, 2026. They never go back down.

| Tier | Lifetime API spend | Concurrent voice sessions | 
|---|---|---|
| Tier 0 | $0 | 10 | 
| Tier 1 | $50 | 20 | 
| Tier 2 | $250 | 50 | 
| Tier 3 | $1,000 | 100 | 
| Tier 4 | $5,000 | 200 | 
| Enterprise | On request | Custom | 

At $0.08 a minute, $5,000 is 62,500 minutes of conversation. A queue taking 500 four-minute calls a day clears that in about a month, so the ceiling stops being a blocker for anyone in production. It does still matter for pilots: a new account cannot load-test 50 simultaneous calls on day one without prepaying credits to unlock the tier.

### There is an EU region now

The Grok Voice 3.0 model page lists **us-east-1, eu-west-1 and us-saltlake-2**. In August it was `us-east-1` only, which ended the conversation for a lot of European teams. xAI's voice docs also list SOC 2 Type II, HIPAA eligibility with a BAA, GDPR data processing agreements, and a statement that audio is processed in real time and "never stored or used for training." I have not audited any of that. It is what the vendor publishes, and it is enough to get a security review started.

### The one that did not change: it knows nothing about you

The model is smarter, but it isn't yours. Every number above assumes a policy document and tools someone wrote for the benchmark. On your line, the agent knows exactly what you connect it to: an uploaded collection, your functions, an MCP server. Most of the work in a real voice support rollout is there, plus deciding when to hand off to a human.

The teams I see do this well treat the phone agent like a knowledge base chatbot with a voice: the answers live in one maintained source, and every channel reads from it. If you are still deciding whether to automate phone support at all, start there, not with the model choice.

## What it costs, with the math done

Grok's voice pricing is the simplest in the category, straight from the xAI pricing page:

| Voice API | Price | 
|---|---|
| Speech to speech ( `grok-voice-3.0` ) | $0.08 per minute ($4.80 per hour) + $0.004 per text input | 
| Speech to speech ( `grok-voice-2.0` , deprecated) | Same rate | 
| Speech to text | $0.10 per hour (REST), $0.20 per hour (streaming) | 
| Text to speech | $15.00 per 1M characters | 

There are no seats or platform fees, and no minimum either, just one rate for every minute whether the model thought hard or not. That makes the bill easy to forecast. Plug in your own numbers:

For a sense of scale: a team taking 500 calls a day at 4 minutes each spends about **$4,800 a month**, and at its busiest hour has about 7 calls running at once, inside the free tier's 10. Concurrency only starts to bite above a few thousand calls a day. Starlink's 20,000 calls a day, at the same 4-minute guess (xAI does not publish call length), would be 80,000 minutes and **$6,400 a day**. For the cheaper-model angle, my Grok Voice 2.0 pricing post compares per-minute rates across the category, and my human agent cost comparison puts the per-call number next to a salary.

## How it stacks up against the field

A quick map of the models you would shortlist alongside it, all from the Artificial Analysis leaderboard (cost is AA's measured run cost per hour of input audio, not a list price):

| Model | AA index | τ-Voice | Time to first audio | AA cost per hour | 
|---|---|---|---|---|
| Grok Voice 3.0 | 86.9 | 71.6% | 0.81s | $4.80 | 
| GPT-Live-1 (Astra, medium) | 83.2 | 74.5% | 1.34s | $5.83 | 
| Gemini 3.8 Live Extended Thinking (High) | 82.6 | 68.6% | 1.35s | $3.50 | 
| Grok Voice 2.0 | 81.3 | 56.5% | 0.70s | $4.80 | 
| GPT-Live-1 (Sol, low) | 80.1 | 59.3% | 1.24s | $4.47 | 
| GPT-Realtime-2.1 High | 75.2 | 45.7% | 1.21s | $10.75 | 

On the outside view, xAI also points to EVA-Bench, ServiceNow's open benchmark for enterprise voice agents across 200+ scenarios, where Grok Voice 3.0 posts the top accuracy score at 74%.

If you want a whole platform rather than a model, the trade looks different. Tools like Retell AI, Vapi and ElevenLabs wrap models in call flows and dashboards, plus analytics on top.

If you live inside a helpdesk, check what it ships natively first: Zendesk voice AI agents and Freshcaller's voice agents are both worth a look. My roundup of the best AI voice agents covers the rest.

If you only need the voice and want to bring your own brain, text-to-speech engines like Cartesia Sonic 3 sit lower in the stack, and the wider AI voice companies map shows who sells what. For alternatives inside the same family question, my Grok Voice 2.0 alternatives list still holds for the competitors.

## Who should pick it up, and who should wait

The short answer to can AI answer support calls is now a firm yes for the model layer. Who should act on that:

**Switch now if** you already run Grok Voice 2.0 (it is the same API and price, so the only cost is a round of regression calls), or you are building a phone agent that has to complete multi-step tasks and every 100ms of silence costs you callers.

**Run a bake-off first if** you are already on GPT-Live-1 (Astra). It still wins τ-Voice by 2.9 points. Whether Grok's 0.5-second head start beats that depends on how tool-heavy your calls are.

**Wait or look elsewhere if** your callers speak languages outside the supported 20, you need a no-code call flow builder (start with xAI's own Voice Agent Builder or a platform above), or your blocker is not the voice at all but the answers. My guide to the best AI for voice support covers the packaged options. A brilliant voice reading out the wrong refund policy is still the wrong refund policy, and it sits at the top of every list of chatbot problems that make customers give up.

## Grok handles the call. eesel handles what it should say.

Grok Voice 3.0 is a conversation engine. It does not know your return window, your last 50,000 tickets, or which customers get the goodwill credit. That is the half eesel does: an AI helpdesk teammate that joins Zendesk, Freshdesk or Gorgias, learns from your past tickets and help center, and works the email and chat queue that sits behind every phone line. Before it goes live, you can run it against your own past tickets and see how its answers compare with what your team actually sent.

For builders, the two connect cleanly on paper. Grok's `mcp` tool takes a server URL, an `authorization` token and custom headers. Every eesel workspace is an MCP server: run `npx @eesel/cli mcp token` from the eesel CLI and it prints the URL, a 30-day token and the `X-Eesel-Agent-Id` header, which is exactly the shape Grok asks for. The same CLI lets you script and inspect the agent from a terminal or hand it to a coding agent like Claude Code. I have not run that pairing on a live phone line yet, so treat it as a promising build, not a shipped integration, and keep an eye on the extra latency of a tool call mid-sentence.

Pricing is a fixed monthly credit plan: $299 a month for 500 credits, where a ticket or chat counts as one credit, with 100 free credits to try it and no card required. Try eesel on a slice of your queue before your next voice pilot, so the agent on the phone has something worth saying.

## Frequently Asked Questions

`grok-voice-latest`, you are already on 3.0. If you pinned `grok-voice-2.0`, change the model name to `grok-voice-3.0`; the endpoint, session parameters and events stay the same. Apps built on the OpenAI Realtime API can still move over with a base URL and key swap.
