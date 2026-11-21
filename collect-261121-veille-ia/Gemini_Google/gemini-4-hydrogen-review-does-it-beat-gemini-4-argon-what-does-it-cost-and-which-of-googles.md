---
id: collect-261121-veille-ia/gemini-google/gemini-4-hydrogen-review-does-it-beat-gemini-4-argon-what-does-it-cost-and-which-of-googles
title: "Gemini 4 Hydrogen review: does it beat Gemini 4 Argon, what does it cost, and which of Google’s…"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["gemini", "hydrogen", "googles", "argon", "does", "index", "high", "cost"]
source: https://medium.com/@mcthkayaa/gemini-4-hydrogen-review-does-it-beat-gemini-4-argon-what-does-it-cost-and-which-of-googles-0d8130262774
source_anchor: ""
source_lines: [1, 162]
section: "Gemini_Google"
---
# Gemini 4 Hydrogen review: does it beat Gemini 4 Argon, what does it cost, and which of Google’s claims hold?

Gemini 4 Hydrogen is Google DeepMind’s lower-priced Gemini 4 model, released on 12 November 2026 as `gemini-4-hydrogen`, at $1.50 per million input tokens and $7.50 per million output tokens, flat across a 1M window, with a free tier. On Artificial Analysis's Intelligence Index v5.0.1, read 13 November 2026, it scores 50.787 at high against Gemini 4 Argon's 50.412, for $1.87 per index task against $4.97.

We sent no prompt to Gemini 4 Hydrogen. The page rests on Google’s announcement, its model card and the Gemini API model page, outside boards read between 05:53 and 06:12 UTC on 13 November 2026, and 106 first-hand user reports from about the first 12 hours after launch, so the user evidence is about half a day old.

Google’s figures are labelled as Google’s and our arithmetic as ours, and every outside figure names its board, version and setting. Not established: Google’s “#5 on Agent Arena” and the full Safety & Security Report.

## What is Gemini 4 Hydrogen, and how does it relate to Gemini 4 Carbon and Argon?

Gemini 4 Hydrogen is the lowest-priced of Google’s three Gemini 4 models, at $1.50 / $7.50 per million tokens against $2 / $10 for Gemini 4 Carbon and Gemini 4 Argon until 30 November, and its model card says “Gemini 4 Hydrogen is adapted from Gemini 4 Carbon.” [p.2]. Google’s announcement says Carbon and Argon “excel at deep, long-horizon reasoning on your hardest problems”, while “Hydrogen is built for rapid execution across diverse agentic workflows”. The card gives a knowledge cutoff of March 2026 [p.6] and inputs of text, images, audio and video [p.2], and the Gemini API changelog marks Hydrogen generally available on 12 November 2026, two days after Carbon.

### Is Gemini 4 Hydrogen the replacement for Gemini 3.8 Flash?

Gemini 4 Hydrogen is presented by Google as the upgrade from Gemini 3.8 Flash for API users, but Google keeps 3.8 Flash on its price list, at $0.75 / $3.75 until 31 December 2026 and at Hydrogen’s $1.50 / $7.50 from 1 January 2027. Google’s What’s new guide carries a “Migrate from Gemini 3.8 Flash and earlier” checklist: Hydrogen defaults to high where 3.8 Flash used medium, shares one 1,048,576-token window across input, thinking and output, and drops explicit caching. In the Gemini app, eight of the 106 first-hand reports, from Plus, AI Plus, Pro and Workspace accounts among others, describe Hydrogen replacing an older Flash model on 12 November. For a 3.8 Flash API user the trade today is a higher list price for a lower cost per index task, $1.87 against $7.77 at high on Artificial Analysis’s Intelligence Index v5.0.1, and from 1 January 2027 the two list prices are the same (our arithmetic).

## Does Gemini 4 Hydrogen really beat Gemini 4 Argon?

On Artificial Analysis’s Intelligence Index v5.0.1, read 13 November 2026, Gemini 4 Hydrogen beats Argon by 0.375 points at high, 50.787 against 50.412, for $1.87 per index task against $4.97, about 0.38 times the cost, and in 7.90 minutes per task against 29.96 (our arithmetic). Google’s announcement says “Hydrogen surpasses Argon on the Artificial Analysis Intelligence Index at its default high thinking level (51 vs. 50, and reaching 54 at its highest xhigh thinking setting)”; those are Artificial Analysis’s rounded displays of 50.787, 50.412 and 53.919.

Cost per index task is Artificial Analysis’s average bill for one task of its nine-evaluation index at the model’s API price; Argon’s $4.97 is at its introductory $2 / $10, which doubles on 1 December. Artificial Analysis has a single Argon row, at high, which Google’s guide gives as Argon’s ceiling: Carbon and Argon “default to `medium` and cap at `high`". Gemini 4 Carbon at high scores 60.026 for $5.48 per index task, 9.239 points above Hydrogen at high for about 2.9 times the cost (our arithmetic); Google's guide calls Carbon "Our strongest frontier model".

Google’s model card measures something else: “Gemini 4 Hydrogen did not show a material increase in general capability under our MCC assessment compared with Gemini 4 Argon” [p.9]. The MCC is a safety check and the index a benchmark average; both texts stand.

Other boards split. Argon is above Hydrogen on arena’s Text board (second against eighth, from arena’s Hugging Face dataset published 11 November) and on the Vals Index (69.39% against 68.49%), and below it on Andon Labs’ Vending-Bench 2 ($13,718.16 against $14,549.49).

Users split too. AcidReaper1 on r/Bard wrote at 20:26:41 UTC on 12 November 2026 that “Hydrogen produces roughly the same analysis as Argon”, in “about 3–4 seconds on medium thinking”, where Argon “took about 2 minutes”. simonw on Hacker News, at 22:38:23 UTC the same day, called a drawing test “quite a nice illustration of how Hydrogen really is significantly less powerful than Carbon and Argon”.

### How does Gemini 4 Hydrogen compare with GPT-6.1 Sol, Claude Sonnet 5.5 and GPT-6.1 Terra?

GPT-6.1 Sol at high scores 50.329 on Artificial Analysis’s Intelligence Index v5.0.1 for $0.59 per index task, 0.458 points below Gemini 4 Hydrogen at high for about a third of the cost, and Sol at xhigh scores 52.458 for $0.76 (our arithmetic; read 13 November 2026). Claude Sonnet 5.5 at max scores 54.541 for $10.02, 0.622 points above Hydrogen at xhigh for 3.17 times the cost per task, and GPT-6.1 Terra at max scores 51.327 for $0.96, 0.54 points above Hydrogen at high for about half the cost (our arithmetic).

Google’s results table uses only Sonnet 5.5 and Terra as columns. GPT-6.1 Sol appears in one footnote and no column, and Claude Opus 5.5 and GPT-6 Astra are absent; on the same index Claude Opus 5.5 at high scores 51.836 for $3.46 per index task.

- **Gemini 4 Carbon (High)** ; Index v5.0.1: 60.026; Cost per index task: $5.48; Minutes per task: 28.36
- **Claude Sonnet 5.5 (Max)** ; Index v5.0.1: 54.541; Cost per index task: $10.02; Minutes per task: 34.13; Note: column in Google’s table
- **Gemini 4 Hydrogen (Xhigh)** ; Index v5.0.1: 53.919; Cost per index task: $3.16; Minutes per task: 13.65; Note: Google’s “54”
- **GPT-6.1 Sol (Xhigh)** ; Index v5.0.1: 52.458; Cost per index task: $0.76; Minutes per task: 10.47; Note: not in Google’s table
- **GPT-6.1 Terra (Max)** ; Index v5.0.1: 51.327; Cost per index task: $0.96; Minutes per task: 14.64; Note: column in Google’s table
- **Gemini 4 Hydrogen (High)** ; Index v5.0.1: 50.787; Cost per index task: $1.87; Minutes per task: 7.90; Note: API default; Google’s “51”
- **Gemini 4 Argon (High)** ; Index v5.0.1: 50.412; Cost per index task: $4.97; Minutes per task: 29.96; Note: Argon’s only row; Google’s “50”
- **GPT-6.1 Sol (High)** ; Index v5.0.1: 50.329; Cost per index task: $0.59; Minutes per task: 7.74
- **Claude Sonnet 5.5 (High)** ; Index v5.0.1: 41.515; Cost per index task: $1.64; Minutes per task: 6.86
- **Gemini 4 Hydrogen (Medium)** ; Index v5.0.1: 41.233; Cost per index task: $0.80; Minutes per task: 3.71
- **Gemini 3.8 Flash (High)** ; Index v5.0.1: 30.847; Cost per index task: $7.77; Minutes per task: 14.71
- **Gemini 4 Hydrogen (Low)** ; Index v5.0.1: 26.894; Cost per index task: $0.29; Minutes per task: 1.31

*Artificial Analysis Intelligence Index v5.0.1 and Performance v3.0.0, read at 05:56 UTC on 13 November 2026, with Artificial Analysis’s setting names and each model’s price on that date.*

## How much does Gemini 4 Hydrogen cost, and what changes on 1 December and 1 January?

Gemini 4 Hydrogen costs $1.50 per million input tokens and $7.50 per million output tokens, thinking tokens included, flat across its 1,048,576-token window, per Google’s pricing page read 13 November 2026; Batch and Flex cost $0.75 / $3.75. Gemini 4 Argon and Carbon cost $2 / $10 “through November 30, 2026” and $4 / $20 from 1 December, so Hydrogen’s list price falls from 0.75 times theirs to 0.375 times. Gemini 3.8 Flash costs $0.75 / $3.75 through 31 December 2026 and $1.50 / $7.50 from 1 January 2027, so Hydrogen lists at twice 3.8 Flash’s price until then and at the same price after (our arithmetic, both ratios).

Cached input costs $0.075 per million through implicit caching, which Google’s guide says “runs by default on prompts of at least 4,096 tokens”; explicit caching is not supported, and the guide says “We count and bill every token the model processes, including thinking tokens”.

### Is Gemini 4 Hydrogen free to use?

Yes, Gemini 4 Hydrogen is free of charge and rate-limited on the Gemini API and in Google AI Studio: Google’s pricing page lists Hydrogen’s free tier as “Free of charge”, with rate limits, while Argon and Carbon have none. Under “Used to improve our products” the page says Yes for the free tier and No for the paid tier, and grounding with Google Search is “Not available” on the free tier; on the paid tier it includes 5,000 free requests a month, then $14 per 1,000.

### Is Gemini 4 Hydrogen cheaper per task than Gemini 3.8 Flash?

Yes, Gemini 4 Hydrogen costs less per Artificial Analysis index task than Gemini 3.8 Flash: $1.87 at high against $7.77 for Gemini 3.8 Flash at high, read 13 November 2026, about a quarter of the cost, although Hydrogen’s list price is twice 3.8 Flash’s introductory price (our arithmetic). declanjackson, writing on Hacker News as an Artificial Analysis employee at 22:49:37 UTC on 12 November, gave a cause: “This is due to 3.8 Flash having a much lower cache hit rate compared to Gemini 4 Hydrogen”.

One user measured the opposite. data-ottawa on Hacker News, at 20:27:54 UTC on 12 November, wrote: “My use case benchmarks 37% more expensive per task but 40–60% of the total time per task (median and p90 had the same improvement)”, with “Both models on medium reasoning.” No day-one report showed an invoice.

## Which of Google’s Gemini 4 Hydrogen benchmark claims hold on the owners’ boards?

Seven of the eleven Google claims about Gemini 4 Hydrogen below match the owners’ boards read on 13 November 2026, two after rounding; Vals Legal Research differs, Hydrogen’s MKA and Terminal-Bench 4.1 scores are Google’s own runs, and “#5 on Agent Arena” could not be checked (our count). “Leads its class” holds against the two rivals in Google’s table, Claude Sonnet 5.5 and GPT-6.1 Terra; on AutomationBench-AA, Claude Fable 5.2 (76.42), Argon (77.51) and Carbon (77.82) score above Hydrogen’s 76.14.

- **Beats Argon on AA’s index** ; Google’s figure: 51 vs. 50; Board, version, setting: Artificial Analysis, Intelligence Index v5.0.1, high vs high; Board value: 50.787 vs 50.412; Result: matches after rounding
- **AA’s index at xhigh** ; Google’s figure: 54; Board, version, setting: same, xhigh; Board value: 53.919; Result: matches after rounding
- **#1 on Vals Tax Agent Bench** ; Google’s figure: 52.3%; Board, version, setting: Vals AI, updated 11 Nov; Board value: 52.30%, 1st of 85; Result: matches
- **Ties for #1 on Vals Legal Research** ; Google’s figure: 55.8%; Board, version, setting: Vals AI, updated 11 Nov; Board value: 53.85%, 6th of 95; Result: differs; 55.77% is Carbon’s
- **HLE-Diamond, leads its class** ; Google’s figure: 58.8%; Board, version, setting: Artificial Analysis, xhigh; Board value: 58.8; Result: matches; ties GPT-6.1 Sol (Max)
- **AutomationBench-AA, leads its class** ; Google’s figure: 76.1%; Board, version, setting: Artificial Analysis, xhigh; Board value: 76.14; Result: matches; three pricier models higher
- **Vending-Bench 2** ; Google’s figure: $14,549; Board, version, setting: Andon Labs, average across runs; Board value: $14,549.49, 3rd of 75; Result: matches
- **MKA** ; Google’s figure: 53.4%; Board, version, setting: Artificial Analysis MKA (Climb WSR); Board value: no Hydrogen value; Result: Google’s own run
- **Terminal-Bench 4.1, Hydrogen** ; Google’s figure: 57%; Board, version, setting: Harbor, official 4.1 board; Board value: not listed; Result: Google’s own run
- **Terminal-Bench 4.1, 3.8 Flash** ; Google’s figure: 18.2%; Board, version, setting: same, mini-SWE-agent, high; Board value: 18.2%; Result: matches
- **Agent Arena** ; Google’s figure: #5; Board, version, setting: arena.ai; Board value: page blocked; 11 Nov dataset has no Hydrogen row; Result: not checked

*Boards read on each owner’s site between 05:56 and 06:12 UTC on 13 November 2026, arena through its Hugging Face dataset. Google’s figures from its announcement and its evaluation notes.*

In Google’s own table Sonnet 5.5 beats Hydrogen on 3 of 10 rows. Google’s evaluation notes say rival results are “sourced from providers’ self-reported numbers unless otherwise mentioned”, and Hydrogen’s SWE-sweep, Agents’ Last Exam, OSWorld-2.1, Chartography and Terminal-Bench-Science values are self-computed and on no owner board.

### Does Gemini 4 Hydrogen tie for first on Vals Legal Research?

No, Gemini 4 Hydrogen does not tie for first on the board Vals AI shows: its Legal Research board, updated 11 November and read 13 November 2026, lists Gemini 4 Hydrogen at 53.85%, sixth of 95, and the 55.77% at the top is Gemini 4 Carbon’s, first on its own. Google’s announcement says Hydrogen “ties for #1 on the Vals Legal Research Bench (55.8%)”. The Wayback Machine has no copy of the page from after launch (the last is from 9 November), so an earlier Hydrogen value cannot be checked.

### Does Gemini 4 Hydrogen lead the new MKA benchmark?

No outside result shows Gemini 4 Hydrogen leading MKA yet: Google’s 53.4% is its own run (“results for Gemini 4 Hydrogen are self-computed”), and Artificial Analysis’s MKA board, read 13 November 2026, has no Hydrogen value. Google’s announcement says Hydrogen “outperforms other models on the new Multimodal Knowledge Agent (MKA) benchmark (53.4%)”. Google’s post introducing MKA says “Artificial Analysis runs the 200-task evaluation set”, and on that board Claude Fable 5.2 scores 56.56 and Carbon 54.34, both above 53.4.

### Is Gemini 4 Hydrogen really 2.5 times faster than Gemini 3.8 Flash?

Gemini 4 Hydrogen is 2.5 times faster than Gemini 3.8 Flash only on Artificial Analysis’s launch-evening data: a Wayback copy from 21:26 UTC on 12 November shows Hydrogen at high taking 6.01 minutes per index task, against 14.83 for Gemini 3.8 Flash at high in a copy from 18:37 UTC, 2.47 times faster, while the live figures on 13 November give 7.90 against 14.71, 1.86 times (our arithmetic). Google says Hydrogen finishes “tasks 2.5 times faster at its high default setting”.

Artificial Analysis’s Performance method v3.0.0 averages “the workloads we measured over the past 5 days”, so in our inference the launch-evening figure rested on fewer measurements. Output speed is 179.2 tokens per second now against 112.1 for 3.8 Flash, 1.60 times (our arithmetic), and Google’s partner figure, “2x to 2.8x faster”, has no published source.

### What do Google’s Terminal-Bench figures for Gemini 4 Hydrogen show?

Google’s “57% vs. 18.2%” for Gemini 4 Hydrogen on Terminal-Bench 4.1 is half on the owner’s board: 3.8 Flash’s 18.2% matches Harbor’s official Terminal-Bench 4.1 leaderboard, read 13 November 2026, while Hydrogen’s 57% is Google’s own run and the board has no Gemini 4 row. In Google’s own table Claude Sonnet 5.5 scores 63.9%, above Hydrogen, and Google leaves the Terra cell as a dash where the board lists GPT-6.1 Terra (Codex, max) at 53.3%.

Google’s Terminal-Bench 4.0 chart says its Sonnet 5.5 and Terra results “are taken from the official public leaderboard”, but its top points, about 70.6% and 50.3% (read by eye), equal Anthropic’s and OpenAI’s own published figures; tbench.ai shows 61.8% and 51.8%.

## Which thinking level should you use with Gemini 4 Hydrogen?

High, the default, is the level at which Gemini 4 Hydrogen passes Argon on Artificial Analysis’s Intelligence Index v5.0.1: 50.787 for $1.87 per index task, against 26.894 for $0.29 at low, 41.233 for $0.80 at medium and 53.919 for $3.16 at xhigh, read 13 November 2026. A thinking level sets how much the model reasons before answering, and thinking tokens are billed as output. Google’s guide calls high “Balanced intelligence and speed” and says to “Reserve” xhigh “for hard, multi-step coding and agentic problems where `high` falls short".

### Is xhigh worth the extra tokens on Gemini 4 Hydrogen?

On Artificial Analysis’s figures, Gemini 4 Hydrogen at xhigh adds 3.13 index points over high for $1.29 more per index task, 1.73 times the minutes and 2.75 times the output tokens, 303.6M against 110.4M (our arithmetic); Artificial Analysis calls the setting “slower than average per task and very verbose”, 76th of 77 on verbosity. Google’s guide says “Using `xhigh` increases latency and token use and can cause overthinking on simpler prompts."

nekrosstratia on r/GeminiAI, at 18:40:45 UTC on 12 November 2026, reported that “Hydrogen used 3x as many tokens when set to xhigh vs high” across “3–4 tests I ran of my real production workload”; the posted table gives 16,615 thinking tokens at high and 43,607 at xhigh in the first test, against 28,356 for 3.8 Flash.

## Where can you use Gemini 4 Hydrogen?

Gemini 4 Hydrogen is in Google AI Studio, the Gemini API, Gemini for business, the Gemini app, Google Search and Google Sheets, and in Google Antigravity for paid Google AI Pro and Ultra subscribers, per the announcement and the model card [p.3 to p.4]. The changelog says Hydrogen “is now the default model for the Antigravity agent in Gemini Managed Agents”, and the model page lists function calling, computer use and the Batch API as supported and the Live API as not. OpenRouter, read 13 November 2026, lists Hydrogen at 142 tokens per second through Google Vertex and 136 through Google AI Studio.

### Do free Gemini app users get Gemini 4 Hydrogen?

Free Gemini app users get Gemini 4 Hydrogen only when Google’s auto selection picks it: footnote 1 of the announcement says “In the Gemini app, free users will get access to Gemini 4 Hydrogen via auto model selection when it’s the appropriate model for the prompt.” Among the 106 first-hand reports, two describe free accounts answered by Hydrogen with auto selection on, and one a free app held on Flash-Lite; Google’s support page on auto selection was quoted by a Reddit user, not read by us.

### What is Orchestrator in Google Antigravity, and does it use Gemini 4 Hydrogen?

Orchestrator is a model option in Google Antigravity that pairs Gemini 4 Hydrogen with Carbon or Argon, and Google says it reached “near-Carbon task success rates while, on average, consuming 24% less quota and completing 19% faster”; nobody outside Google has measured that. Google’s Antigravity post says Carbon “can start in a read-only phase to explore dependencies, resolve ambiguities, and build a concrete plan”, after which “Hydrogen could then take over to write edits, update packages, and run tests”, handing back if it “hits friction”.

The post gives that result “On Terminal-Bench 4.1” and the announcement “On Terminal-Bench 4.0”; both are Google’s runs. The post says Orchestrator rolls out “as Orchestrator (Carbon) and Orchestrator (Argon) for Google AI Ultra subscribers, and as Orchestrator (Argon) for paid Google AI Pro subscribers”.

## What does the Gemini 4 Hydrogen model card say about safety?

Gemini 4 Hydrogen’s unjustified refusals rose 5.57 percentage points against Gemini 3.8 Flash on Google’s automated test [p.8], and the card says “unjustified refusals are higher, particularly for multimodal prompts” [p.7]. Image-to-text safety rose 7.00 points and refusal tone fell 0.65 [p.8].

Under its Frontier Safety Framework (April 2026), Google ran a Material Capability Change (MCC) assessment, its check of whether Hydrogen has “meaningful new capabilities or material increases in performance” over Argon, and Hydrogen showed none, so “a full suite of critical capability evaluations was not required” [p.9]. Targeted tests found Hydrogen “does not reach the CBRN or cyber CCLs, though it reaches the CCL alert thresholds for both domains” [p.9]; CCLs are the framework’s critical capability levels, each with an alert threshold below it. The card says “The full Safety & Security Report for Gemini 4 Hydrogen will be available in the coming days.” [p.7]; it was not out on 13 November 2026.

Google’s announcement adds figures against 3.8 Flash, such as “a 96.7% honesty rate on our internal agent honesty evaluation”, which no outside board measures; one of the 106 first-hand reports describes heavier safeguards than 3.8 Flash.

## Is Gemini 4 Hydrogen good for coding and agentic tasks?

On outside coding boards Gemini 4 Hydrogen ranks in the top fifteen, not first: 54.3% at high on Cursor’s CursorBench 4.0, 12th of 90, where Carbon leads with 60.4%, and 38.9% at xhigh on Cognition’s FrontierCode 2.0, seventh of 32, behind Sonnet 5.5 (42.2%) and GPT-6.1 Sol (39.6%) at max; arena’s WebDev board has it 10th of 156. Agentic office-work boards place it higher: fifth of 137 on Zapier’s AutomationBench 1.0.6 at 51.14% (high), third of 75 on Vending-Bench 2 and sixth of 37 on the Vals Index (68.49%), all read 13 November 2026. Epoch AI had no Hydrogen page.

## What do users report about Gemini 4 Hydrogen on day one?

We read 106 first-hand day-one reports on Gemini 4 Hydrogen from about the first 12 hours, 70 on Reddit, 24 on Hacker News and 12 on Google’s AI developer forum, so the evidence is about half a day old. Most praise speed and light subscription quota, split on quality against Argon and Claude Sonnet 5.5, and complain about Antigravity suspensions. The five below carry a measurement or a direct comparison, the last against the majority; Reddit was read through a public archive of Reddit’s own records.

- data-ottawa, Hacker News, 12 November 2026, 20:27:54 UTC: “37% more expensive per task” than 3.8 Flash but “40–60% of the total time per task”, with “Both models on medium reasoning.”
- AcidReaper1, r/Bard, 12 November 2026, 20:26:41 UTC: on financial-statement summaries, “2 customers used 2% of the 5-hour quota” on Hydrogen, against 30% for one customer on Argon.
- nekrosstratia, r/GeminiAI, 12 November 2026, 18:40:45 UTC: about three times the thinking tokens at xhigh, and “hydrogen does MUCH better than 3.8 does”.
- Kevin_Watkins, Google AI developer forum, 12 November 2026, 20:44:13 UTC: suspended “as soon as I try Hydrogen for a routine check on a process”, then an edit to the same post: “Looks like we’re back”.
- simonw, Hacker News, 12 November 2026, 22:38:23 UTC, the counter-report: “Surprisingly plain pelicans for a Gemini model”, which the post reads as Hydrogen being “significantly less powerful than Carbon and Argon”.

News reposts and promotional benchmark posts were left out of the count, and no verified Google staff account appears among the authors read.

### What happened with Antigravity account suspensions after the Gemini 4 Hydrogen update?

After the Gemini 4 Hydrogen update, on 12 November 2026, a Google AI developer forum topic collected 40 posts between 19:31 and 22:03 UTC from Antigravity users whose accounts returned “This service has been disabled in this account for violation of Terms of Service”, several right after selecting Hydrogen; several posters reported access back between 21:02 and 21:43 UTC. The opening post says its suspension was likely triggered by a third-party wrapper or extension; later posts, including Kevin_Watkins’s above, describe Google’s own Antigravity apps. A forum account signing “Google Support” called it a “known issue” at 21:41 UTC, but the account has no staff flag, no Google staff account confirmed a cause in anything we read, and we state none.

## What is still unknown about Gemini 4 Hydrogen?

As of 13 November 2026, Google had not published the Safety & Security Report for Gemini 4 Hydrogen, and no board we could read listed Hydrogen on Agent Arena, Artificial Analysis’s MKA board, Harbor’s Terminal-Bench 4.1 board or Epoch AI. Nobody outside Google has measured Orchestrator’s 24% and 19%, Google has not said how often auto selection gives free users Hydrogen, and the user evidence covers about 12 hours, not a week of real use. The page gets a dated update when an outside board adds Hydrogen or Google publishes the safety report.

*Originally published at* *aitoolspolice.com**.*
