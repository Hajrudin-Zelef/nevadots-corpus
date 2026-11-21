---
id: collect-261121-veille-ia/grok-xai/grok-voice-3-0-review-a-top-two-ai-voice-model-at-0-08-a-minute
title: "Grok Voice 3.0 review: a top-two AI voice model at $0.08 a minute"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: []
keywords: ["voice", "grok", "model", "speech-to-speech", "call", "phonic", "minute", "calls"]
source: https://aiwiz.uk/blog/grok-voice-3/
source_anchor: ""
source_lines: [1, 339]
section: "Grok_xAI"
---
Grok Voice 3.0 is SpaceXSI’s speech-to-speech model for real-time AI voice agents, released on 14 October 2026. In one session it can listen, reason, call tools and reply aloud. It is second on Artificial Analysis’s Speech-to-Speech Index with 86.9, 0.2 points behind Phonic 1, and costs $0.08 a minute. Every agent pointed at the `grok-voice-latest` alias switched to it on launch day.

The most revealing detail is where SpaceX already uses it. Starlink’s support line sends about 20,000 calls a day through Grok Voice, and SpaceXSI says seven in ten end without a human. That is the job to judge it on. On τ-Voice, the test that most resembles a real support call, it resolves 71.6% of calls, behind Phonic 1 on 79.9% and OpenAI’s GPT-Live-1 on 74.5%.

Prices are US list prices in dollars.

## The answer in brief

- **What it is:** SpaceXSI’s speech-to-speech model for AI voice agents, released on 14 October 2026 as`grok-voice-3.0` . It already runs Grok in Tesla cars, Grok Bot’s voice mode and Starlink’s support line.
- **How good is it?** As of 1 November 2026, second on Artificial Analysis’s Speech-to-Speech Index at 86.9, behind Phonic 1 (High) on 87.1 and ahead of OpenAI’s GPT-Live-1 with GPT-6 Astra at medium effort (83.2) and Google’s Gemini 3.8 Live Extended Thinking at high (82.6). Plain Gemini 3.8 Live scores 74.5. It starts replying in 0.81 seconds, against 0.61 for Phonic 1 and 1.34 for GPT-Live-1.
- **The weak spot:** on τ-Voice, the simulated support-call test, it resolves 71.6% of calls, against 79.9% for Phonic 1 and 74.5% for GPT-Live-1.
- **Price:** $0.08 a minute, unchanged from Grok Voice 2.0, with reasoning included. GPT-Live-1 costs $0.05 a minute for the voice layer alone.
- **For call centres:** a credible engine for routine calls such as account look-ups and order status, as long as the hand-off to people is designed as carefully as the agent.
- **For UK teams:** usable now, but SpaceXSI does not guarantee where voice traffic is processed, so agree a data processing agreement and transfer terms before live calls, and confirm whether Zero Data Retention applies or how long call data is kept.
- **Our view:** one of the two models to test first for a new AI voice agent, with Phonic 1. Pilot it on one low-risk queue and plan around 71.6%, not 86.9.

## What is Grok Voice 3.0?

Grok Voice 3.0 is the model inside an AI voice agent: the software that answers a call or in-app chat, works out what the person wants and completes the task, such as checking an order or booking an appointment. The business supplies the instructions, knowledge and tools; the model handles the conversation.

Grok Voice 3.0 is the third generation of Grok’s native speech-to-speech model. Developers reach it as `grok-voice-3.0` on the `/v1/realtime` WebSocket endpoint, where `grok-voice-latest` now points to it, according to the speech-to-speech documentation. Your app streams the caller’s audio in and receives spoken replies back over the same session.

The model reasons while it speaks. Reasoning is on by default (`reasoning.effort` is `"high"`) and can be switched off with `"none"`. Within a single session it can search the web or X, query your document collections, call your own functions and use remote MCP servers.

SpaceX is also its biggest customer. The model runs Grok in Tesla vehicles, the voice mode of Grok Bot and Starlink’s customer support line.

## Who makes it: xAI, SpaceXAI or SpaceXSI?

All three names refer to the same company. SpaceX acquired xAI in February 2026, the AI unit became SpaceXAI in July, and it took the name SpaceXSI (SpaceX Super Intelligence) in early October, as SiliconANGLE recorded. The developer docs still live at docs.x.ai and the API at api.x.ai, and Artificial Analysis lists the provider as SpaceXAI. Older code and articles that say xAI are describing the same platform.

The first two voice models were also marketed as Grok Voice Think Fast 1.0 and 2.0, a name that still appears in some URLs and third-party guides. The company’s text model is covered in our Grok 4.7 guide.

## How does Grok Voice 3.0 compare with GPT-Live-1 and Gemini?

At launch, Grok Voice 3.0 led Artificial Analysis’s Speech-to-Speech Index by 3.7 points over OpenAI’s GPT-Live-1. Phonic 1 (High), announced on 29 October, has since edged ahead, on 87.1 against 86.9. In the launch figures below, Grok Voice 3.0 also leads the two rivals SpaceXSI chose to compare against on four of the five individual results.

If the table extends beyond the screen, scroll sideways to view all columns.

| Benchmark (what it tests) | Grok Voice 3.0 | Grok Voice 2.0 | GPT-Live-1 (Astra, Medium) | Gemini 3.8 Live (Ext. Thinking, High) | 
|---|---|---|---|---|
| Speech-to-Speech Index (composite) | Grok Voice 3.0 **86.9** | Grok Voice 2.081.3 | GPT-Live-1 (Astra, Medium)83.2 | Gemini 3.8 Live (Ext. Thinking, High)82.6 | 
| Big Bench Audio (spoken reasoning) | Grok Voice 3.0 **98.5%** | Grok Voice 2.097.2% | GPT-Live-1 (Astra, Medium)90.1% | Gemini 3.8 Live (Ext. Thinking, High)97.7% | 
| τ-Voice (simulated support calls) | Grok Voice 3.071.6% | Grok Voice 2.056.5% | GPT-Live-1 (Astra, Medium) **74.5%** | Gemini 3.8 Live (Ext. Thinking, High)68.6% | 
| Task success rate (correct final tool call) | Grok Voice 3.0 **94.8%** | Grok Voice 2.094.6% | GPT-Live-1 (Astra, Medium)87.4% | Gemini 3.8 Live (Ext. Thinking, High)89.1% | 
| Speech Agent Arena (blind human preference, Elo) | Grok Voice 3.0 **1070** | Grok Voice 2.01011 | GPT-Live-1 (Astra, Medium)1048 | Gemini 3.8 Live (Ext. Thinking, High)990 | 
| Full Duplex Bench (pauses, turn-taking, interruptions) | Grok Voice 3.0 **97.0%** | Grok Voice 2.095.1% | GPT-Live-1 (Astra, Medium)94.9% | Gemini 3.8 Live (Ext. Thinking, High)91.9% | 

Source: SpaceXSI launch post, quoting Artificial Analysis testing dated 13 October 2026. On the live leaderboard on 16 October 2026 the index, τ-Voice and task success figures were unchanged, while the live Arena ratings had moved to 1056 for GPT-Live-1 and 993 for Gemini. The best score in each row is in bold. GPT-Live-1 was tested with OpenAI’s GPT-6 Astra handling the reasoning at medium effort, as SiliconANGLE explains; our GPT-6 Astra guide covers that model.

The index gives equal weight to four results: Big Bench Audio, τ-Voice, Arena preference and task success rate. Full Duplex Bench is reported beside it and does not count towards the score. The Arena component uses a rating frozen when a model first qualifies, so the live Arena Elo can drift without moving the index.

ChatGPT’s own voice mode has just moved on. On 15 October 2026, the day after Grok’s launch, OpenAI began rolling out GPT-Live-1.1 in ChatGPT, with 13 new voices and better non-English performance, and its ChatGPT voice help page now names GPT-Live-1.1 for Plus, Pro and Business and GPT-Live-1.1 Quip for Free and Go. OpenAI says API access is coming soon, its API model page still listed only GPT-Live-1 on 16 October, and no benchmarks for 1.1 have been published, so the table compares the models a business can build on today. Our GPT-Live-1.1 guide covers the new voices and plan limits.

One result comes from outside Artificial Analysis. On EVA-Bench, ServiceNow’s open-source test of enterprise voice agents across more than 200 scenarios, Grok Voice 3.0 posted the top accuracy score at 74%. SpaceXSI says it was also the best model when accuracy and conversational experience were scored together. ServiceNow’s Nitin Aggarwal said it “pairs top accuracy with a natural, responsive conversation”. SpaceXSI did not publish rival EVA-Bench scores alongside it.

## Where does Grok Voice 3.0 fall short?

Its weakest result is on the test that looks most like the job it is sold for. τ-Voice puts each model on a simulated support call (an airline change, a disputed retail charge, a telecom fault) with varied accents, background noise and a caller who talks over the agent. Success means the database ends in exactly the right state. Grok Voice 3.0 resolves 71.6% of those calls, GPT-Live-1 74.5%. SpaceXSI calls the two “comparable”; on the published figures, OpenAI is ahead, and Phonic 1 (High), announced on 29 October, is further ahead on 79.9%.

That gap is less settled than it looks. Artificial Analysis notes that GPT-Live-1’s τ-Voice score rests on a single trial, while most models are averaged over three. A 2.9-point lead from one run could narrow or widen on retest.

The comparison set also flatters the new model in places. Read against the full leaderboard rather than the two chosen rivals:

- **Conversational dynamics.** StepAudio 3 Realtime (98.9%), Qwen Audio 3.0 Realtime Plus (98.4%) and GPT-Live-1 in its Sol, low configuration (97.3%) all edge Grok’s 97.0% on Full Duplex Bench.
- **Spoken reasoning.** StepAudio 3 Realtime and two Qwen models score a little higher than Grok’s 98.5% on Big Bench Audio.
- **Human preference.** On the live Arena on 16 October, Gemini 3.1 Flash Live Minimal (1100) and the standard Gemini 3.8 Live (1088) sat above Grok’s 1070.
- **Running cost.** On Artificial Analysis’s cost per hour of input audio, Gemini 3.8 Live Extended Thinking (High) comes in at $3.50 and plain Gemini 3.8 Live at $0.84, against $4.80 for Grok Voice 3.0, and Phonic 1 (High) costs $4.20. SpaceXSI’s cost-per-task chart left Gemini out.

Apart from Phonic 1, none of those rivals has a higher index score, because each falls down on another component. Qwen Audio 3.0 Realtime Plus, for example, manages 54.6% on τ-Voice. Grok Voice 3.0 sits near the top of the index by scoring well on all four components at once.

The production figures are the least verifiable part of the launch. The Starlink and Tesla numbers come from SpaceXSI about its own parent company, and nobody outside has audited them.

## Is Grok Voice 3.0 really 40% faster?

Yes, against GPT-Live-1, and Artificial Analysis’s own timing backs it. SpaceXSI’s claim is that Grok Voice 3.0 matches GPT-Live-1 on τ-Voice at 40% lower latency. Artificial Analysis measures Grok Voice 3.0’s time to first audio at 0.81 seconds and GPT-Live-1 (Astra, Medium) at 1.34 seconds, a gap of just under 40%. Gemini 3.8 Live Extended Thinking (High) takes 1.35 seconds. GPT-Live-1 in its Astra configuration resolves more simulated calls but takes over half a second longer to start speaking. Phonic 1 (High), announced on 29 October, is quicker still at 0.61 seconds and resolves more calls than either.

The 40% does not compare the new model with its predecessor, as some launch coverage reported. Grok Voice 2.0 was quicker to first audio at 0.70 seconds, so 3.0 trades about a tenth of a second for its accuracy gains.

Two limits apply to these figures. Time to first audio is measured on Big Bench Audio questions, so it shows how fast a reply starts and says nothing about how long a full support call takes. The architectures also differ: GPT-Live-1 hands its reasoning to a separate backend model, so its timing also depends on that model. Test on your own calls and network before treating 0.81 seconds as a promise.

## What changed from Grok Voice 2.0?

The big change is agentic performance: τ-Voice success rose from 56.5% to 71.6%, a gain of 15.1 points in eleven weeks. Everything else moved by smaller margins. The index rose 5.6 points, Arena preference 59 Elo points and Full Duplex Bench 1.9 points, while task success rate barely shifted (94.6% to 94.8%).

The developer surface did not change at all. The endpoint, session parameters, events and price are the same as Grok Voice 2.0, and `grok-voice-2.0` was marked deprecated on launch day.

One warning for anyone tracking scores over time: Grok Voice 2.0 launched in July with an index score of 82.9, and under Artificial Analysis’s current index it scores 81.3. The index has been revised since then, so compare scores only within the same snapshot.

If the table extends beyond the screen, scroll sideways to view all columns.

| Date | Release | Note | 
|---|---|---|
| 14 October 2026 | ReleaseGrok Voice 3.0 | Note `grok-voice-latest` moved to 3.0 the same day; 2.0 deprecated | 
| 29 July 2026 | ReleaseGrok Voice 2.0 | NoteAlias moved from 1.0 on 5 August; priced at $0.08 a minute | 
| 23 April 2026 | ReleaseGrok Voice 1.0 | NoteAvailable in the Speech to Speech API | 
| 16 December 2025 | ReleaseSpeech to Speech API | NoteGeneral availability of the realtime voice API | 

Source: SpaceXSI release notes, checked 15 October 2026.

## How much does the Grok Voice API cost?

Grok Voice 3.0 costs $0.08 a minute ($4.80 an hour) of audio, plus $0.004 per text input, the same as Grok Voice 2.0, according to SpaceXSI’s pricing page. That rate covers the whole model, reasoning included. SpaceXSI contrasts this with voice stacks that bill separately for speech recognition, reasoning, the synthesised voice and the platform.

The price has not always been $0.08. Grok Voice Think Fast 1.0 cost $0.05 a minute, and the rate rose when Grok Voice 2.0 arrived in July. Some older guides and cached pages still quote $0.05, so check the date on any figure you find.

OpenAI’s GPT-Live-1 looks cheaper at $0.05 a minute, billed per second. That price buys only the voice layer. OpenAI bills the backend model that does the reasoning, and any tools, separately, and its strongest benchmark results come from pairing GPT-Live-1 with GPT-6 Astra.

Artificial Analysis’s cost per task on its Big Bench Audio run gives a like-for-like view:

If the table extends beyond the screen, scroll sideways to view all columns.

| Model | Cost per task | 
|---|---|
| **Grok Voice 3.0** | Cost per task **$0.025** | 
| GPT-Live-1 (Astra, Medium) | Cost per task$0.030 | 
| GPT-Realtime-2.1 (High) | Cost per task$0.055 | 

Source: SpaceXSI launch post, quoting Artificial Analysis, checked 15 October 2026.

Artificial Analysis’s cost per hour of input audio adds Google’s models, and setting it beside each model’s support-call score shows what the money buys:

If the table extends beyond the screen, scroll sideways to view all columns.

| Model | Cost per hour of input audio | τ-Voice (support calls completed) | Speech-to-Speech Index | 
|---|---|---|---|
| **Grok Voice 3.0** | Cost per hour of input audio$4.80 | τ-Voice (support calls completed)71.6% | Speech-to-Speech Index86.9 | 
| Phonic 1 (High) | Cost per hour of input audio$4.20 | τ-Voice (support calls completed) **79.9%** | Speech-to-Speech Index **87.1** | 
| GPT-Live-1 (Astra, medium) | Cost per hour of input audio$5.83 | τ-Voice (support calls completed)74.5% | Speech-to-Speech Index83.2 | 
| Gemini 3.8 Live Extended Thinking (High) | Cost per hour of input audio$3.50 | τ-Voice (support calls completed)68.6% | Speech-to-Speech Index82.6 | 
| Gemini 3.8 Live | Cost per hour of input audio **$0.84** | τ-Voice (support calls completed)30.1% | Speech-to-Speech Index74.5 | 

Source: Artificial Analysis Speech-to-Speech leaderboard, checked 1 November 2026. The best figure in each column is in bold. GPT-Live-1’s τ-Voice score rests on one trial; the others average three.

The per-minute comparison turns round here. GPT-Live-1’s voice layer costs $3.00 an hour, but the system Artificial Analysis tested, with GPT-6 Astra doing the reasoning, came to $5.83 an hour, more than Grok Voice 3.0. Phonic 1 (High) costs less than Grok, at $4.20 an hour, and completes more simulated calls. Plain Gemini 3.8 Live costs under a fifth of Grok’s rate and completes fewer than half as many support tasks, which suits simple routing better than resolution. Both measures come from test questions, so for a contact centre the number that matters is cost per resolved call on your own traffic.

Neither the pricing page nor the models page says whether server-side tools used inside a voice session, such as web search at $5 per 1,000 calls in the text API, add to the per-minute rate (checked 16 October 2026). Ask SpaceXSI before you budget a tool-heavy agent.

The rest of the voice stack is priced separately: text-to-speech at $15 per million characters, and speech-to-text at $0.10 an hour in batch or $0.20 an hour streaming.

## Is Grok Voice 3.0 good for call centres and customer service?

It makes a serious case for handing routine calls to an AI voice agent, as long as you design for the calls it will not finish. The model resolves about seven in ten simulated support calls, starts speaking in under a second and connects to existing phone systems over SIP. The remaining three in ten decide whether an AI call centre feels better or worse to customers.

### What does the Starlink deployment show?

Starlink is the closest thing to a reference customer, with the caveat that it belongs to the same company. SpaceXSI says Grok Voice answers about 20,000 Starlink calls a day in several languages and resolves roughly 70% with no human involved. One in four sales calls ends in a purchase before the caller hangs up. A single agent verifies the customer, troubleshoots hardware, issues credits and ships replacement equipment.

That 70% sits close to the model’s 71.6% on τ-Voice, which suggests the benchmark is a reasonable planning figure. SpaceXSI has not said how it counts a resolution, though, and Starlink has advantages most operators lack: one product line, back-end systems built by the same parent company, and an engineering team that can tune the agent daily.

### Which calls should you automate first?

Start with calls that end in a clear system action and leave the emotionally loaded ones to people. This is our assessment, based on SpaceXSI’s documentation and the published benchmarks:

If the table extends beyond the screen, scroll sideways to view all columns.

| Call type | Fit for Grok Voice 3.0 | What to watch | 
|---|---|---|
| Identity checks and account look-ups | Fit for Grok Voice 3.0Strong; structured tool calls are where it scores 94.8% task success | What to watchRead numbers back digit by digit and confirm them | 
| Order, delivery and service status | Fit for Grok Voice 3.0Strong; read-only look-ups the docs say to run without asking | What to watchKeep facts in the prompt or a knowledge base | 
| Guided troubleshooting | Fit for Grok Voice 3.0Proven at Starlink for hardware faults | What to watchCap failed attempts before escalating | 
| Credits, refunds, bookings and cancellations | Fit for Grok Voice 3.0Good with a confirmation step | What to watchThe docs advise confirming before any action that changes something | 
| Inbound sales and renewals | Fit for Grok Voice 3.0Promising; Starlink reports one in four sales calls converting | What to watchSales disclosure rules apply as they do to human agents | 
| Complaints and distressed or vulnerable callers | Fit for Grok Voice 3.0Hand to a person | What to watchThe prompting guide makes an empathetic safety path and a transfer mandatory in a crisis | 
| Noisy lines, strong accents, unlisted languages | Fit for Grok Voice 3.0Test before committing | What to watchThis is where τ-Voice scores drop for every model | 

Source: AIWIZ assessment, 15 October 2026, from SpaceXSI’s prompting guide and launch post and the Artificial Analysis results.

### How do you set up a Grok voice agent?

There are three routes to a Grok voice agent, from a no-code AI voice agent platform to the raw API.

- **No code: Voice Agent Builder.** SpaceXSI’s console tool, launched in July and still in beta, sets up an agent from a written call flow, uploaded documents, tools and guardrails. Each account gets a free phone number, with telephony on it charged at an extra $0.01 a minute. Every call is recorded and transcribed, so supervisors can review what the agent said and which tools it used.
- **Your own numbers over SIP.** You register a number you already own, point your carrier, contact-centre platform or PBX at SpaceXSI’s SIP address over TLS, and your server joins each call through a webhook and a WebSocket. The SIP guide gives set-up steps for Twilio, Telnyx and Plivo, and the agent speaks the G.711 codecs phone networks use, so there is no transcoding step.
- **Through your telephony platform.** Voximplant offers a native Grok connector covering phone, SIP, WhatsApp Business calling and call transfer. 3CX publishes a call script that connects inbound calls on its AI Edition to a Grok agent that can search an approved staff directory and transfer callers.

Teams still choosing a general-purpose agent builder before they commit to a voice model can start with our Botpress vs Voiceflow comparison, which includes worked costs and a UK procurement checklist.

The call-control features are the ones a contact centre needs:

- **Transfer to a human.** A`refer` request sends the caller to any phone or SIP destination and reports back only once the destination answers. If the transfer fails, the caller stays on the line with the agent.
- **Keypad input.** Phone keypresses reach the model as text, submitted when the caller presses #, pauses for 2.5 seconds or starts speaking.
- **Call-backs.** Session resumption lets a caller who rings back within 30 minutes pick up where they left off.
- **Scripted lines.**`force_message` plays fixed wording, such as a recording notice, and can be made uninterruptible.

Two gaps are worth knowing before you start. The SIP guide covers inbound calls and does not document outbound dialling (checked 16 October 2026). Custom SIP headers are not sent with a transfer, so pass the call summary to the human agent through your CRM or contact-centre platform.

### How should the hand-off to humans work?

The hand-off deserves as much design time as the agent itself. SpaceXSI’s prompting guide suggests transferring at once when the caller asks for a person, when they are clearly dissatisfied, after two failed tool attempts on the same task, or after three unclear or silent turns in a row. The agent should say a short line, such as telling the caller it is connecting them to a specialist, while it triggers the transfer.

The same guide carries habits that experienced contact-centre teams will recognise. The agent answers only from approved facts and says when it does not know. It confirms before changing anything on an account, reads codes back character by character, and fills tool delays with a brief “I’m checking that now”. Where callers might switch language, you can lock the agent to one.

### What will it cost per call?

At $0.08 a minute, model cost scales with call length. The figures below are illustrative and cover the voice model only:

If the table extends beyond the screen, scroll sideways to view all columns.

| Average call length | Grok Voice 3.0 | Grok Voice 3.0 on a SpaceXSI number | GPT-Live-1 voice layer | 
|---|---|---|---|
| 3 minutes | Grok Voice 3.0$0.24 | Grok Voice 3.0 on a SpaceXSI number$0.27 | GPT-Live-1 voice layer$0.15, plus backend model | 
| 6 minutes | Grok Voice 3.0$0.48 | Grok Voice 3.0 on a SpaceXSI number$0.54 | GPT-Live-1 voice layer$0.30, plus backend model | 
| 10 minutes | Grok Voice 3.0$0.80 | Grok Voice 3.0 on a SpaceXSI number$0.90 | GPT-Live-1 voice layer$0.50, plus backend model | 

Source: AIWIZ calculation from SpaceXSI’s $0.08 a minute API rate and $0.01 a minute telephony on a free number, and OpenAI’s $0.05 a minute GPT-Live-1 session price, checked 15 October 2026.

Resolution rate changes the picture. If seven in ten six-minute calls resolve and the rest run their full length before transfer, the model cost per resolved call is about $0.69, or $0.77 on a SpaceXSI number. Carrier minutes, tool calls, your platform and the human time on escalated calls all come on top. The fair comparison is with what a resolved call costs you today.

### What about compliance?

SpaceXSI’s voice overview says that with Zero Data Retention (ZDR) switched on, audio is processed in real time and never stored or used for training. Without ZDR, its security FAQ says API requests and responses are kept for 30 days for abuse checks, though SpaceXSI does not train on them. ZDR covers a whole API team and also switches off features such as the Batch API, Files and Collections, which voice agents use to search your documents. The overview also lists SOC 2 Type II, HIPAA eligibility with a business associate agreement, GDPR data processing agreements and EU data residency options. The prompting guide also tells agents to be honest that they are an AI.

Two things need checking before a regulated rollout: where SpaceXSI processes your callers’ audio, and what you must tell callers. For UK businesses, the next section covers both.

## Can UK businesses use Grok Voice 3.0?

Yes, once two questions are settled: where callers’ audio is processed, and what callers are told. SpaceXSI’s regional endpoints page says requests to the global endpoint, `api.x.ai`, may be routed between regions for capacity or reliability, so the processing location is not guaranteed. Its only regional endpoint is in the US, and it does not serve the voice APIs (checked 16 October 2026). The voice overview mentions EU data residency options and GDPR data processing agreements without saying how to arrange them, and the regions page sends contractual residency requests to SpaceXSI’s sales team. Mistral’s Voxtral 3 offers a public EU endpoint instead, though it excludes Mistral’s stateful Agents and doesn’t say where its new voice agents fit.

Location matters under UK GDPR, because a caller’s voice and transcript are personal data. Making them available to an organisation outside the UK is a restricted transfer, which the ICO’s guide to international transfers says needs UK adequacy regulations, an appropriate safeguard such as the ICO’s international data transfer agreement backed by a transfer risk assessment, or one of a few narrow exceptions. Ask SpaceXSI for its data processing agreement and the transfer mechanism it relies on before a live call goes through it, and confirm in that agreement whether Zero Data Retention applies or how long call audio and transcripts are kept.

Telling callers is the second question. If you record or monitor calls, as Voice Agent Builder does for every call, one condition for lawful business monitoring under regulation 4 of the 2018 interception regulations is that the business has made all reasonable efforts to tell users that their calls may be intercepted. A short `force_message` line at the start of each call can carry that notice and say the caller is speaking to an AI. Callers in the EU add a legal reason for the second part: Article 50 of the EU AI Act, which applies from 2 August 2026, requires AI systems that talk directly to people to tell them so, unless it is obvious. This is a starting checklist, so take legal advice on your own set-up.

## Where else does Grok Voice 3.0 fit?

It fits wherever someone needs to talk to software with their hands or eyes busy, and the conversation should end in an action. The car is the largest deployment so far: SpaceXSI says Grok Voice handles millions of conversations a day in Tesla vehicles, where drivers check calendars and answer email by voice and hand bigger errands, such as ordering a coffee or booking dinner, to Grok Bot.

If the table extends beyond the screen, scroll sideways to view all columns.

| Use case | Why it fits | What to check | 
|---|---|---|
| In-car and hands-free assistants | Why it fitsProven at Tesla scale; tools and connectors turn requests into actions | What to checkDistraction rules, and patchy mobile coverage (session resumption helps on reconnect) | 
| Voice inside apps and websites | Why it fitsEphemeral tokens keep API keys off devices; web, WebRTC and iOS sample apps; LiveKit and Pipecat integrations | What to checkNetwork latency adds to the model’s 0.81 seconds | 
| Healthcare front desks | Why it fitsHIPAA eligible with a business associate agreement; tool calls into booking systems | What to checkClinical questions must go to staff | 
| Multilingual service desks | Why it fits20 listed languages, automatic detection and switching mid-call, a per-call language hint | What to checkThe docs say quality varies outside the listed languages | 
| Internal IT and HR helpdesks | Why it fitsCollections search over your own documents; remote MCP servers for internal systems | What to checkWhat the agent is allowed to look up, and for whom | 
| Branded voice products | Why it fits28 built-in voices, custom voices from a short clip, a pronunciation map for product names | What to checkPermission from anyone whose voice you clone | 

Source: AIWIZ assessment from SpaceXSI’s voice overview and speech-to-speech documentation, checked 15 October 2026.

Some work should stay with people for now. SpaceXSI’s own guardrail template keeps agents away from medical, legal and financial advice and sends anyone in crisis to a human. Very noisy settings are another weak spot, since background noise is part of what pulls every model’s τ-Voice score down.

## What can developers build with the Grok Voice API?

The Voice Agent API is built for phone and in-app agents, and it is compatible with the OpenAI Realtime API. Moving an existing Realtime app across means changing the base URL to `wss://api.x.ai/v1/realtime`, swapping the API key and picking a Grok voice model. A handful of OpenAI events are renamed or unsupported. Grok Voice 3.0 is also available in SpaceXSI’s Voice Agent Builder, reached from the developer console.

If the table extends beyond the screen, scroll sideways to view all columns.

| Area | What the docs say | 
|---|---|
| Connection | What the docs sayWebSocket; ephemeral tokens for browser and mobile clients; SIP for phone and contact-centre calls; sample apps for Twilio, WebRTC and iOS; LiveKit and Pipecat integrations | 
| Audio | What the docs sayPCM from 8 to 48 kHz (24 kHz default), G.711 μ-law and A-law, Opus at 24 kHz | 
| Languages | What the docs say20 listed with automatic detection, including three Arabic variants and regional Spanish and Portuguese; a language hint and up to 100 key terms bias transcription | 
| Voices | What the docs say28 built-in voices, `eve` by default (two have British accents); custom voices cloned from a clip of up to 120 seconds | 
| Tools | What the docs sayCollections search, web search, X search, remote MCP servers and custom functions, with parallel tool calls | 
| Call control | What the docs sayServer-side voice activity detection for barge-in; idle re-engagement timer; speech speed from 0.7x to 1.5x | 
| SpaceXSI extensions | What the docs say `force_message` for verbatim lines such as recording disclosures; session resumption for up to 30 minutes of inactivity; a pronunciation map that fixes spoken audio without changing the transcript | 
| Compliance | What the docs sayWith Zero Data Retention, audio processed in real time and never stored or used for training; otherwise API data kept for 30 days, not used for training; SOC 2 Type II; HIPAA eligible with a business associate agreement; GDPR with EU data residency options | 

Source: SpaceXSI voice overview and speech-to-speech documentation, checked 15 October 2026; compliance row rechecked against the voice overview and security FAQ on 1 November 2026.

The prompting advice for 3.0 is to write less. SpaceXSI recommends much shorter system prompts than older models needed, and stripping out instructions that only existed to patch earlier bugs.

## How do you upgrade from Grok Voice 2.0?

If your agent uses `grok-voice-latest`, it is already running Grok Voice 3.0. If you pinned `grok-voice-2.0`, set `model` to `grok-voice-3.0`; nothing else in the integration needs to change. SpaceXSI has deprecated 2.0 but has not published an end-of-life date.

The automatic switch cuts both ways. SpaceXSI’s own docs recommend pinning a versioned model in production for stability, and teams on the alias got a new model mid-flight. Before you leave it there, rerun your evaluation calls, listen to a sample of live conversations, and trim any prompt workarounds written for 2.0.

## Is Grok Voice 3.0 the best AI voice agent model to build on?

It is one of the two strongest, though no longer first on every measure. Phonic 1 (High), announced on 29 October, now edges it on the only independent composite ranking (87.1 against 86.9), on support calls and on speed. Grok Voice 3.0 keeps a higher task success rate (94.8% against 92.3%), starts speaking about half a second sooner than GPT-Live-1 and Gemini 3.8 Live Extended Thinking, and its flat $0.08 a minute includes the reasoning that OpenAI bills separately. For a new AI voice agent, test it alongside Phonic 1; contact centres have a credible option for routine calls either way.

**For contact-centre leaders,** plan around the τ-Voice result of 71.6%. Pilot it on one busy, low-risk queue such as order status or account look-ups. Measure containment, repeat calls and complaints against your human baseline, and widen the scope only once transfers to people work smoothly.

**For product teams** building voice into apps or cars, the case is simpler. Latency and turn-taking are among its strongest results, and Realtime code written for OpenAI moves across with a new base URL and key.

**Look elsewhere in two cases.** If cost per hour is the binding constraint, Gemini 3.8 Live Extended Thinking is cheaper on Artificial Analysis’s measure, and plain Gemini 3.8 Live costs under a fifth as much for calls that only need routing. If support-call accuracy matters most, Phonic 1 (High) leads τ-Voice at 79.9%, and GPT-Live-1 paired with GPT-6 Astra also beats Grok’s 71.6%, on a single trial.

The rankings are moving quickly. Phonic 1 took first place fifteen days after Grok Voice 3.0 launched, and OpenAI says GPT-Live-1.1 is coming to its API; once Artificial Analysis tests it, the comparison above will need rerunning. A pilot built on Grok Voice 3.0 keeps the OpenAI option open, because both APIs follow the same Realtime event model.

If you want help planning a voice agent pilot on one queue, including the hand-off rules and a scorecard, see our AI adoption services or browse all AIWIZ services.

## Frequently asked questions

## What is Grok Voice 3.0?

Grok Voice 3.0 is SpaceXSI's real-time speech-to-speech model for AI voice agents. It listens, reasons, calls tools and replies by voice, and it powers Grok in Tesla vehicles and Starlink's support line.

## When was Grok Voice 3.0 released?

SpaceXSI released it on 14 October 2026, eleven weeks after Grok Voice 2.0.

## How much does Grok Voice 3.0 cost?

It costs $0.08 a minute, or $4.80 an hour, plus $0.004 per text input (US list prices). That is the same price as Grok Voice 2.0, and it includes the model's reasoning.

## Is Grok Voice 3.0 better than GPT-Live-1?

On Artificial Analysis's overall index, yes: 86.9 against 83.2. GPT-Live-1 still resolves more simulated support calls on τ-Voice (74.5% against 71.6%), while Grok Voice 3.0 starts replying about half a second sooner.

## Can Grok Voice 3.0 run a call centre on its own?

No. It can handle a large share of routine calls, and Starlink reports about 70% resolved without a human. The rest need a well-designed transfer to people.

## Which languages does Grok Voice support?

The docs list 20 languages with automatic detection, including English, Arabic, Chinese, French, German, Hindi, Japanese, Spanish and Portuguese. Other languages work with varying accuracy.

## How do I upgrade from Grok Voice 2.0?

Agents on grok-voice-latest already run 3.0. If you pinned grok-voice-2.0, change the model name to grok-voice-3.0; the API is otherwise unchanged. Rerun your evaluation calls before you rely on it.

## Can UK businesses use Grok Voice 3.0?

Yes. SpaceXSI lists no UK or EU regional endpoint and says requests to its global endpoint may be processed in any region, so treat callers' audio as a restricted transfer under UK GDPR and get SpaceXSI's data processing agreement and transfer terms first, including whether Zero Data Retention applies; without it, the API keeps request data for 30 days. Tell callers if calls are recorded, and that they are speaking to an AI.

## Is there a Grok Voice API?

Yes. Developers use it through the Voice Agent API at wss://api.x.ai/v1/realtime, with the model set to grok-voice-3.0 or grok-voice-latest. The API is compatible with OpenAI's Realtime API.

## What is Grok Voice Agent Builder?

It is SpaceXSI's no-code tool for building phone agents on Grok Voice, in beta since July 2026. It bundles a free phone number, a knowledge base, tools, guardrails, and recordings and transcripts of every call. It is billed at the API rate plus $0.01 a minute for telephony on the free number.

## Was Grok Voice called Think Fast?

Yes. The first two releases were marketed as Grok Voice Think Fast 1.0 and 2.0. Version 3.0 drops the Think Fast name.

## Is Grok Voice 3.0 the same as Grok voice mode?

Not exactly. Grok voice mode is the consumer feature in the Grok app, while Grok Voice 3.0 is the model businesses build agents on. SpaceXSI says 3.0 powers Grok in Tesla vehicles and Grok Bot's voice mode; its launch post does not say whether the main Grok app uses it.

## Is Grok voice better than ChatGPT voice?

For businesses building agents, Grok Voice 3.0 scores higher overall than GPT-Live-1 on Artificial Analysis's index, 86.9 against 83.2, starts replying about half a second sooner and cost less per hour in Artificial Analysis's test ($4.80 against $5.83). GPT-Live-1 still resolves more simulated support calls. ChatGPT itself began moving to GPT-Live-1.1 on 15 October 2026, and no independent benchmarks for that model have been published yet.

## Can Grok Voice clone a voice?

Yes. SpaceXSI's custom voices API creates a voice from a reference clip of up to 120 seconds, and the result works in both live conversations and text-to-speech. Get written permission from anyone whose voice you clone.
