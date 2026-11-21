---
id: collect-261121-veille-ia/grok-xai/grok-4-7-review-what-spacexsi-claims-what-it-costs-and-what-changed-in-the-seven-weeks-since
title: "Grok 4.7 review: what SpaceXSI claims, what it costs, and what changed in the seven weeks since…"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: []
keywords: ["grok", "changed", "spacexsi", "launch", "spacexai", "model", "since", "september"]
source: https://medium.com/@mcthkayaa/grok-4-7-review-what-spacexsi-claims-what-it-costs-and-what-changed-in-the-seven-weeks-since-d84a7c045254
source_anchor: ""
source_lines: [1, 178]
section: "Grok_xAI"
---
# Grok 4.7 review: what SpaceXSI claims, what it costs, and what changed in the seven weeks since launch

Grok 4.7, released by SpaceXSI (SpaceX Super Intelligence, the company formerly styled xAI and then SpaceXAI) on 21 September 2026 as `grok-4.7`, lists at $2 input and $6 output per million tokens with a 500k context, unchanged since launch. Artificial Analysis's Intelligence Index v5.0.1 scores it 35 at xhigh, 41st of 74, for $7.77 per index task, down from 46.45 on v4.3.2 at launch because Artificial Analysis rebuilt its index; read 12 November 2026.

We sent no prompt to Grok 4.7. The page rests on SpaceXSI’s launch announcement, its 30-page model card, the xAI API docs, Wayback Machine copies of those pages, outside benchmark boards and 52 first-hand user reports, all read on 12 November 2026.

On coding boards outside SpaceXSI, Grok 4.7 ranks ninth of 31 on Cognition’s FrontierCode 2.0 and 19th of 36 on Artificial Analysis’s Coding Agent Index v2.0, and scores 46.3% on CursorBench 4.0, a board run by Cursor, which says it trained the model with SpaceXAI. SpaceXSI’s figures are labelled as SpaceXSI’s and our arithmetic as ours, and Artificial Analysis’s v4.3.2 and v5.0.1 scores are never mixed. Not established: a speed figure for the standard model from SpaceXSI, and which model answers in Grok Bot.

## What changed in Grok 4.7 since launch on 21 September?

The price did not change: xAI’s pricing page lists Grok 4.7 at $2 input, $0.50 cached and $6 output per million tokens on 12 November 2026, as in its Wayback copy of 22 September. Seven other things changed between 22 September and 2 November.

- **22 to 23 Sep** ; What changed: Grok 4.7’s Terminal-Bench 4.0 cell in the launch table edited from 38.0% to 37.6%; Where it shows: Wayback copies of the announcement
- **1 to 2 Oct** ; What changed: grok.com Expert and Heavy relabelled from Grok 4.6 to Grok 4.7; Where it shows: Wayback copies of grok.com
- **10 Oct** ; What changed: Docs “At a glance” table lists only the Responses API; Where it shows: xAI docs, Grok 4.7 page
- **October** ; What changed: Custom moderation controls added to the API; Where it shows: xAI release notes
- **12 Oct** ; What changed: The X Subscription launched, with one usage pool across Grok and Cursor; Where it shows: x.ai news
- **26 to 28 Oct** ; What changed: Announcement footer changes from SpaceXAI to SpaceX Super Intelligence; Where it shows: Wayback copies of the announcement
- **2 Nov** ; What changed: Grok Bot 1.0 announced, without naming Grok 4.7; Where it shows: x.ai news

### Has the Grok 4.7 launch table changed since 21 September?

Yes, one Grok 4.7 cell has changed: SpaceXSI’s announcement printed 38.0% for Grok 4.7 on Terminal-Bench 4.0 in the Wayback copy of 22 September, 13:13 UTC and 37.6% in the copy of 23 September, 03:30 UTC, and the page carries no correction note. The model card, revision 2026–09–21, still prints 38.0% [p.9]. Harbor’s Terminal-Bench 4.0 leaderboard lists Grok 4.7 at 37.6%, plus or minus 3.5 points, in the Grok Build harness at xhigh, so the edited cell now matches the board’s owner.

### Which Grok model do grok.com Expert and Heavy use now?

Grok 4.7: grok.com’s mode list, read 12 November 2026, labels Expert “Thinks hard · Grok 4.7” and Heavy “Team of Experts · Grok 4.7”, both marked as requiring an upgrade. Wayback copies show Grok 4.6 on those labels on 1 October and Grok 4.7 on 2 October, and no x.ai news post announces the switch. The default mode on grok.com, Fast, names no model.

### What is SpaceXSI, and is it the same company as xAI?

SpaceXSI (SpaceX Super Intelligence) is the name now printed on the same x.ai pages that launched Grok 4.7 under the SpaceXAI name, and the model card ties SpaceXAI to xAI: “SpaceXAI is a doing-business-as (DBA) name of XAI LLC. xAI and SpaceXAI may be used interchangeably throughout this card.” [p.4]. The announcement’s footer read “© 2026 SpaceXAI LLC” at launch and reads “© 2026 SpaceX Super Intelligence LLC” now; the switch falls between Wayback copies of 26 and 28 October 2026. The docs index calls itself “Official SpaceXSI (xAI) developer documentation”.

### What are The X Subscription and Grok Bot 1.0, and do they use Grok 4.7?

The X Subscription, launched on 12 October 2026, is one plan for Grok, Grok Bot, Cursor and X Premium at Free $0, Plus $8, Premium $30, Super $100 and Ultra $200 a month, with “a generous pool of Grok model usage shared across Grok and Cursor”; Grok Bot 1.0, announced on 2 November 2026, does not name Grok 4.7.

The Grok Bot FAQ answers “Can I pick the model?” with “No. Grok Bot picks the model for each task, and there is no model picker.” The chosen model “can be a first-party SpaceXSI model or a third-party model”, and “Grok Bot does not show the underlying model for a request”. The FAQ’s own example of a routing target is Claude Opus 5.5.

### What happened to the Grok 4.7 API docs and moderation controls?

The Grok 4.7 docs page now lists only the “Responses API” in its “At a glance” table, on a page “Last updated: October 10, 2026”, where the launch copy listed the Responses API and Chat Completions; the model card says API users “can call Grok 4.7 through the standard chat and completions endpoints” [p.5]. The same docs page still names the `x-grok-conv-id` header for Chat Completions, so in our reading only the summary table changed. In October, xAI's release notes added custom moderation controls, a `guidelines` setting for nsfw, violence and profanity.

## What does Grok 4.7 cost beyond the list price?

Once a prompt reaches 200k tokens, Grok 4.7 costs $4 input, $1 cached and $12 output per million, and xAI’s pricing page says such requests “are billed at the higher rate for all tokens in the request”. Below 200k, cached input costs $0.50 per million, a quarter of the input price (our arithmetic). Batch requests are 20% off, priority processing is 2x, and the US endpoint is 1.1x ($2.20 / $0.55 / $6.60).

The long-context rate covers the whole request, output included, once the prompt crosses the line. Cursor draws the line elsewhere: a Cursor staff member wrote in Cursor’s Grok 4.7 launch thread on 24 September 2026 that “On 4.7, requests with more than 256k of input are billed at 2x (3x with Fast)”, and that Grok 4.7 runs in Cursor at “256k by default, with an optional 500k”. OpenRouter applies its doubling at 200,000 prompt tokens, as xAI does.

Caching is the other cost lever. The docs say “We highly recommend setting a `prompt_cache_key`" and warn that "without it you often pay full input price on a cache-cold server", which means $2 per million input tokens in place of $0.50.

### Is Grok 4.7 really half the price of comparable models?

On list price, yes, against the models in SpaceXSI’s own launch table: Grok 4.7’s $2 / $6 is half GPT-5.6 Sol’s $4 input and 0.3 times its $20 output, and 0.2 and 0.12 times Fable 5.1’s $10 / $50 (our arithmetic). Per task the gap narrows, and on one board it reverses.

On Cursor’s CursorBench 4.0, Grok 4.7 at Extra High costs $6.01 per task against $8.23 for GPT-5.6 Sol at Max, 0.73 times (our arithmetic); Cursor, which owns the benchmark, says it trained Grok 4.7 jointly with SpaceXAI. On Artificial Analysis’s Intelligence Index v5.0.1, Grok 4.7 at xhigh costs $7.77 per index task against $6.24 for GPT-6 Astra at Max, a model whose $10 input price is five times Grok 4.7’s (our arithmetic). The reason is volume: Artificial Analysis says Grok 4.7 generated 148M tokens on the index, “very verbose in comparison to the median of 72.8M”, about twice as many (our arithmetic). SpaceXSI’s model card describes the opposite, Grok 4.7 “reaching results with fewer steps and fewer output tokens than other frontier models” [p.4]; its own CursorBench chart plots average output tokens per task [p.6].

## Do SpaceXSI’s Grok 4.7 benchmark claims hold up on owners’ boards?

Most of SpaceXSI’s Grok 4.7 launch-table cells match the benchmark owners’ boards, read 12 November 2026. The exceptions are two Grok 4.7 cells on no owner board as printed and one DeepSWE comparator value that the owner lists under an older model, plus smaller gaps marked in the table: EEBench’s Grok 4.6 cell (board 57.1%), EEBench’s Fable 5.1 setting (high, not Max), HealthBench Professional’s Grok cells (SpaceXSI’s own runs) and the “GDPval” chart (Artificial Analysis’s GDPval-AA). Against the table’s own columns, Grok 4.7 leads 2 of 7 benchmark rows, EEBench and Harvey; Fable 5.1 leads four and GPT-5.6 Sol one (our count).

Where four values share a cell below, the order is Grok 4.7, Grok 4.6, GPT-5.6 Sol, Fable 5.1.

- **CursorBench 4.0, all four** ; SpaceXSI figure: 46.3% / 40.4% / 41.7% / 51.8%; Owner board, version, setting: Cursor, CursorBench 4.0, Extra High / High / Max / Max; Board value: same; Result: matches; Cursor co-trained Grok 4.7
- **DeepSWE v1.1, Grok 4.7** ; SpaceXSI figure: 71.0% (high); Owner board, version, setting: Datacurve, v1.1, mini-swe-agent; Board value: no Grok 4.7 row; Result: not on the owner’s board
- **DeepSWE v1.1, Fable 5.1** ; SpaceXSI figure: 70.0%; Owner board, version, setting: same; Board value: no Fable 5.1 row; Claude Fable 5 69.91% (xhigh), 69.72% (max); Result: a Fable 5 value, in our reading of both documents
- **EEBench, Grok 4.7** ; SpaceXSI figure: 64.0%; Owner board, version, setting: atopile, V1, Grok CLI; Board value: 64.0%; Result: matches; the card says 66.0%
- **EEBench, Grok 4.6** ; SpaceXSI figure: 53.0%; Owner board, version, setting: same, current and 4 Sep data; Board value: 57.1%; Result: differs
- **EEBench, Fable 5.1 (Max)** ; SpaceXSI figure: 56.4%; Owner board, version, setting: same; Board value: 56.4% at high effort; Result: setting differs
- **AA-Briefcase v1.1, all four** ; SpaceXSI figure: 1,657 / 1,546 / 1,487 / 1,678; Owner board, version, setting: Artificial Analysis, launch day; Board value: 1657.2 / 1545.59 / 1487.42 / 1678.37; Result: matches at launch
- **Terminal-Bench 4.0, all four** ; SpaceXSI figure: 37.6% / 20.3% / 37.3% / 57.9%; Owner board, version, setting: Harbor, 4.0, each lab’s harness; Board value: same; Result: matches; Grok 4.7 was 38.0% at launch
- **Harvey Legal Agent Benchmark, Grok 4.7** ; SpaceXSI figure: 19.6%; Owner board, version, setting: Vals AI, HLab v1 at xhigh; HLAB-Vals-1.1; Board value: 12.50%; 18.33%; Result: not on the owner’s board as printed
- **HealthBench Professional, all four** ; SpaceXSI figure: 56.7% / 48.5% / 60.5% / 62.1%; Owner board, version, setting: no owner board; Sol and Fable from OpenAI’s and Anthropic’s own cards; Board value: Sol and Fable match; Artificial Analysis’s Grok 4.7 run: 55.78%; Result: Grok cells are SpaceXSI’s runs
- **“GDPval” chart: Grok 4.7 / Fable 5.1 / Grok 4.6 / GPT-6 Astra** ; SpaceXSI figure: 1695 / 1735 / 1605 / 1542; Owner board, version, setting: Artificial Analysis, GDPval-AA v2.1, launch day; Board value: 1695.21 / 1734.67 / 1605.49 / 1541.89; Result: AA’s GDPval-AA, not OpenAI’s GDPval; Grok 4.7 now 1716.87

*Boards read on each owner’s site, 05:42 to 06:04 UTC on 12 November 2026; launch-day values from Wayback copies. Every other comparator cell (DeepSWE for Grok 4.6 and GPT-5.6 Sol, EEBench for GPT-5.6 Sol on the 4 September data, Harvey for the other three) matches its owner’s board.*

### Which launch-table numbers differ from the Grok 4.7 model card?

Four launch-table numbers differ from SpaceXSI’s own Grok 4.7 model card: EEBench, where the announcement prints 64.0% for Grok 4.7 and the card says “66.0% at xhigh” [p.13]; EEBench’s Fable comparator, which the table prints as Fable 5.1 at 56.4% and the card as “Fable 5 (max, with fallback)” at 54.2% [p.13]; Terminal-Bench 4.0, edited to 37.6% after launch while the card keeps 38.0% [p.9]; and DeepSWE, where the table’s comparator is “Fable 5.1” at 70.0% and the card’s is “Fable 5 (max, with fallback)” at 69.7 [p.8]. On HealthBench Professional the numbers match, but the card’s chart puts GPT-6 Astra (max) first at 63.4% [p.15], a model the table has no column for, and labels Grok 4.6’s 48.5% “xhigh” where the table’s column says High. On that benchmark the card also says “Grok 4.6 (high) was used as the grader model for Grok 4.7 results” [p.15].

### Which Grok 4.7 numbers are not on the benchmark owner’s board?

Two Grok 4.7 cells are on no owner board as printed: Harvey’s Legal Agent Benchmark at 19.6% and DeepSWE at 71.0%. Vals AI, the operator SpaceXSI’s card names, lists Grok 4.7 at 12.50% at xhigh on its archived HLab v1 board and 18.33%, seventh of 37, on HLAB-Vals-1.1; the card describes its 19.6% as a run on 120 held-out tasks [p.12], and Vals AI says its 1.1 results “are not comparable with the archived v1 board”. Datacurve’s DeepSWE v1.1 board, last updated 22 September 2026, has no Grok 4.7 row, so 71.0% is SpaceXSI’s own figure.

The DeepSWE comparator labelled “Fable 5.1” at 70.0% has no matching row either: Datacurve lists no Claude Fable 5.1, and lists Claude Fable 5 at 69.91% at xhigh, displayed as “70%”. SpaceXSI’s own card labels the comparator “Fable 5 (max, with fallback)” at 69.7 [p.8], so in our reading of both documents the 70.0% value belongs to Fable 5.

### What does CursorBench show, and who owns the benchmark?

CursorBench 4.0 puts Grok 4.7 at 46.3% at Extra High for $6.01 per task, against 46.6% for $11.95 for Opus 5 at Max and 51.8% for $17.28 for Fable 5.1 at Max, and Cursor, the board’s owner, says on its Grok 4.7 page: “We trained Grok 4.7 jointly with SpaceXAI on a new, larger base model than Grok 4.6”. SpaceXSI’s card adds that Grok 4.7 “received supplemental training on anonymized Cursor workflow data to improve coding and agentic performance” [p.4].

SpaceXSI’s announcement says that on CursorBench 4.0 “Grok 4.7 is at the frontier in price-performance.” Against Opus 5 at Max, Grok 4.7 costs about half as much per task for 0.3 points less, and Fable 5.1 at Low scores 45.1% for $5.44, 1.2 points less for $0.57 less (our arithmetic). Cursor’s footnote says “small differences in scores may not be statistically meaningful”.

## Where does Grok 4.7 rank now against Claude Opus 5.5, Claude Fable 5.2 and GPT-6 Astra?

Grok 4.7 ranks 41st of 74 on Artificial Analysis’s Intelligence Index v5.0.1, read 12 November 2026, at 35.49 (xhigh), against 61.82 for Claude Fable 5.2 at High, 59.11 for Claude Opus 5.5 at Max and 56.85 for GPT-6 Astra at Max. Artificial Analysis’s own summary reads: “Grok 4.7 (Xhigh) is below average in intelligence, but reasonably priced when comparing to other models of similar price. It’s also notably slow per task and very verbose.”

At launch, on Intelligence Index v4.3.2, Grok 4.7 scored 46.45 and ranked 16th, and the value held through 19 October. The fall came with Artificial Analysis’s v5.0 rebuild in October, which moved every model: in the same captures Fable 5.1 went from 53.35 to 49.01 and GPT-6 Astra from 52.67 to 56.85, so we read the drop as a re-weighting of an unchanged Grok 4.7 result (our inference).

- **Claude Fable 5.2 (High)** ; Index v5.0.1: 61.82; Cost per index task: $9.08
- **Gemini 4 Carbon (High)** ; Index v5.0.1: 60.03; Cost per index task: $7.76; Note: about Grok 4.7’s cost
- **Claude Opus 5.5 (Max)** ; Index v5.0.1: 59.11; Cost per index task: $10.04
- **GPT-6 Astra (Max)** ; Index v5.0.1: 56.85; Cost per index task: $6.24
- **GPT-6.1 Sol (Max)** ; Index v5.0.1: 54.61; Cost per index task: $1.27
- **GPT-6 Astra (Medium)** ; Index v5.0.1: 51.55; Cost per index task: $2.73
- **Claude Opus 5.5 (Medium)** ; Index v5.0.1: 48.21; Cost per index task: $2.45; Note: Opus 5.5 API default
- **GPT-6.1 Terra (Medium)** ; Index v5.0.1: 38.17; Cost per index task: $0.17
- **Grok 4.7 (Xhigh)** ; Index v5.0.1: 35.49; Cost per index task: $7.77; Note: 41st of 74
- **Grok 4.7 (High)** ; Index v5.0.1: 35.21; Cost per index task: $6.18; Note: Grok 4.7 API default
- **GPT-6.1 Sol (Low)** ; Index v5.0.1: 34.80; Cost per index task: $0.16
- **Claude Opus 5.5 (Low)** ; Index v5.0.1: 34.75; Cost per index task: $0.98
- **Grok 4.7 (Low)** ; Index v5.0.1: 31.13; Cost per index task: $2.92; Note: no medium row
- **Mistral Large 4 (High)** ; Index v5.0.1: 26.12; Cost per index task: $2.06

*Artificial Analysis’s setting names, read 12 November 2026. Index v5.0.1 is not comparable with v4.3.2.*

On the same index, Claude Opus 5.5 at Medium scores 12.72 points above Grok 4.7 at xhigh for $5.32 less per index task, and Opus 5.5 at Low comes within a point of Grok 4.7 at xhigh for about an eighth of the cost (our arithmetic).

### How does Grok 4.7 compare with Gemini 4 Carbon and the cheaper GPT-6.1 Terra?

Gemini 4 Carbon at High scores 60.03 on Artificial Analysis’s Intelligence Index v5.0.1 for $7.76 per index task, 24.54 points above Grok 4.7 at xhigh for one cent less (our arithmetic), and GPT-6.1 Terra at Medium scores 38.17 for $0.17, above every Grok 4.7 row, where Grok 4.7’s cheapest row, Low, scores 31.13 for $2.92; read 12 November 2026.

On the owner boards behind SpaceXSI’s launch table, GPT-6 Astra scores above Grok 4.7 on DeepSWE (74.12% at xhigh, where Grok 4.7 has no row), EEBench (69.3% against 64.0%) and Terminal-Bench 4.0 (58.2% against 37.6%), and below it on Vals AI’s archived HLab v1 (5.42% against 12.50%). Cursor’s CursorBench 4.0, whose owner co-trained Grok 4.7, lists no GPT-6 model.

### What do the other outside boards say about Grok 4.7?

The two other outside boards that published a launch-day position now place Grok 4.7 lower: Vals AI’s Vals Index v2.2 has 58.62% at xhigh, 18th of 36, against 60.22% and 10th of 59 in Vals AI’s launch note, and Epoch AI’s Capabilities Index has 156, 39th of 240, where Epoch says “At the time of its release, it ranked 26th of 223”; Grok 4.6 scores 158 on the same scale.

arena.ai’s Text leaderboard puts Grok 4.7 160th of 421 at 1398.5, below Grok 4.6 at 96th, and its WebDev leaderboard 23rd of 156; the arena figures come from arena’s own Hugging Face dataset, because arena.ai returned a Cloudflare check when we tried to read it. Cognition’s FrontierCode 2.0 has 28.62% at xhigh, ninth of 31; Proximal’s FrontierSWE V2 has 29.5%, fifth of 10; Abundant AI’s SWE-Marathon v1.1 has 45.6% at high; Artificial Analysis’s Coding Agent Index v2.0 has Grok Build with Grok 4.7 at 46.6, 19th of 36. Zapier’s AutomationBench lists no xAI model, and METR and the UK AI Security Institute had published nothing on Grok 4.7.

## Is Grok 4.7 really twice as fast?

No outside measurement shows standard Grok 4.7 at twice the speed of Grok 4.6 or of any model in SpaceXSI’s table, and SpaceXSI’s own text gives the 2x to a separate, dearer variant. The announcement’s subtitle reads “Twice as fast, at half the price of comparable models.” Its body says Grok 4.7 is “Served at the same price and speed as Grok 4.6” and adds: “We also serve a fast variant with twice the output speed at twice the price.”

Artificial Analysis measured 82.6 output tokens per second at xhigh on 28 September, with its older method, against 66.7 for Grok 4.6 at high, 60.9 for Opus 5 and 95.5 for GPT-5.6 Sol: 0.86 to 1.36 times the models in SpaceXSI’s launch table and charts, and 1.24 times Grok 4.6 (our arithmetic). Since 3 November Artificial Analysis measures end-to-end speed including time to first token, so the two figures are not comparable; its current reading is 63.3 tokens per second and 27.5 minutes per index task. OpenRouter’s Grok 4.7 page lists 55 to 78 tokens per second across endpoints.

## Which reasoning effort should you use with Grok 4.7?

On Artificial Analysis’s Intelligence Index v5.0.1, high, the API default, gets Grok 4.7 within 0.28 points of xhigh for $1.59 less per index task: 35.21 for $6.18 against 35.49 for $7.77, while low scores 31.13 for $2.92 (our arithmetic; read 12 November 2026). Artificial Analysis has no medium row.

Grok 4.7 takes four settings, low, medium, high (the default) and xhigh, and xAI’s reasoning docs say the setting “Controls reasoning depth (cannot be disabled)”. An effort setting tells the model how long to reason before answering; higher settings spend more output tokens, billed at $6 per million. On CursorBench 4.0, whose owner co-trained the model, the four settings score 33.1%, 41.6%, 43.9% and 46.3% for $1.58, $3.49, $4.69 and $6.01 per task, from low to xhigh.

The docs give a 500,000-token context window and “No text output limit” at every setting, and a knowledge cutoff of May 2026; the model card says “Grok 4.7 has a pretraining data cutoff of June 2026” [p.5]. The two describe different things.

## What is Grok 4.7 Fast and where can you use Grok 4.7?

Grok 4.7 Fast is, per xAI’s docs, “the same model served on faster infrastructure”, priced at $4 input, $1 cached and $12 output per million tokens below 200k and $6 / $1.50 / $18 above, and “available only in Cursor and Grok Build”; standard Grok 4.7, at $2 / $6, is the model on the public API. The docs add: “It is not available on the public xAI API.” No outside board lists Grok 4.7 Fast, so its twice-the-speed-for-twice-the-price trade has no outside measurement, and in Cursor a staff member put Fast requests above 256k of input at 3x the standard rate.

### Can you call Grok 4.7 through the API, Cursor or a gateway?

Yes, Grok 4.7 is available through the xAI API at $2 / $6 per million tokens, and xAI’s docs also list a US endpoint at `https://us.api.x.ai/v1` "with token usage priced at a 10% premium", Grok Build as the default model, Cursor "available on all plans" and the gateways OpenRouter, Vercel and Cloudflare. SpaceXSI's card adds the Word, PowerPoint and Excel add-ins, and Snowflake and Databricks Mosaic among gateways [p.5]. xAI's Vertex AI page uses the model ID `xai/grok-4.7`, and the rate limits page lists Grok 4.7 by tier.

### Is Grok 4.7 in the grok.com app?

Yes, Grok 4.7 has run in grok.com’s paid Expert and Heavy modes since early October 2026 (the labels changed between Wayback copies of 1 and 2 October); the default mode, Fast, names no model, so the page does not tell a free user whether Grok 4.7 answers.

## What did the Grok 4.7 model card find on safety?

SpaceXSI’s model card reports a mixed set for Grok 4.7, all from its own runs: jailbreak compliance fell (StrongReject 2.0% against 3.9% for Grok 4.6) [p.23], while self-harm compliance rose to 1.05% from 0.84% [p.26]. On CVE-Bench, one of the card’s cyber tests, Grok 4.7’s reward is 36.6% at xhigh and 37.7% at high, below Grok 4.6’s 39.8% [p.18], although the card also reports “small cybersecurity-relevant capability gains over Grok 4.6” [p.17].

The card states: “We never silently downgrade intelligence or fall back to other models.” [p.4] The announcement’s “allowing only 3.3% of risky dual-use prompts through” comes from HackerBench, which the card calls “a SpaceXAI internal CTF-style suite” [p.18]: compliance with harmful or dual-use requests was 3.31% at high and 4.02% at xhigh, against 5.93% for Grok 4.6. The one outside check of a safety claim is LatchBio’s benchmarks.bio, where Grok 4.7 is first of 24 on Safeguards (BioSecBench-Refusal) at 62.4%, which matches SpaceXSI’s “topping LatchBio’s biosafety benchmark at 62.4%”. On the other side of the ledger, Cursor staff said launch-week `cyber_policy_review` blocks on ordinary projects "primarily impacts 4.7"; the date and link are in the users section below.

## What do users report about Grok 4.7?

We counted 52 first-hand reports about Grok 4.7 between 21 September and 12 November 2026, 13 on Hacker News, 26 on Reddit, 11 on Cursor’s forum and 2 on GitHub; most come from Cursor users and most are negative about token use and speed, with a positive minority on code review and on sticking to agreed plans. The five below carry figures or a direct comparison, and the last runs against the majority; Reddit posts were read through a public archive of Reddit’s own records.

- Dynamix86 on r/cursor, 21 September 2026, 23:23:12 UTC: with both models at extra high effort and fast mode off, “it took 34 minutes to go from 82% to 81%” of the quota on Grok 4.7, against 133 minutes for one point on Grok 4.6, and the post concludes “Grok 4.7 is about 2.5 times as expensive as 4.6”.
- mchusma on Hacker News, 21 September 2026, 21:43:18 UTC: “4.7 is definitely slower & more expensive.”
- suzumiya on Cursor’s forum, 22 September 2026, 03:10:39 UTC: “I’m currently using Grok 4.7 at about 8M tokens, and the cache hit rate is only around 73%”, against 90 to 91% on the same kind of work with Grok 4.6, in the user’s own figures.
- simondotau on Hacker News, 24 October 2026, 10:39:55 UTC: in cross-model code review, “every other model rate Opus 5.5 first, with Grok 4.7 a very close second, often finding real issues other models miss”.
- r8e8tion on r/cursor, 23 September 2026, 17:01:02 UTC, the counter-report on both speed and tokens: “Grok4.7 xHigh (256k) has been faster and comparable in token usage to 4.6 xHigh.”

Cursor staff, who are not counted as users, confirmed two problems on Cursor’s forum. Launch-week requests on projects unrelated to security were blocked with a `cyber_policy_review` error, which a staff member said on 22 September 2026 "primarily impacts 4.7". Staff also confirmed a Grok 4.7 incident on 14 October 2026 between 06:20 and 07:50 UTC, when requests were slow to start or got no response. None of the 52 reports showed a silent model swap, and none reported a list-price change.

## What is still unknown about Grok 4.7?

Four things about Grok 4.7 were still open as of 12 November 2026: SpaceXSI has published no speed figure for the standard Grok 4.7; Zapier, METR and the UK AI Security Institute have no result on it; Grok Bot does not show which model answers a request; and Datacurve has never added Grok 4.7 to DeepSWE, so SpaceXSI’s 71.0% has no outside check. The page gets a dated update when an outside board adds Grok 4.7 or SpaceXSI changes its price.

*Originally published at* *aitoolspolice.com**.*
