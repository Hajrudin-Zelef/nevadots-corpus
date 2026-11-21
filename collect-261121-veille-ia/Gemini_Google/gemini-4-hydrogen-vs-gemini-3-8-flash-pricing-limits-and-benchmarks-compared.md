---
id: collect-261121-veille-ia/gemini-google/gemini-4-hydrogen-vs-gemini-3-8-flash-pricing-limits-and-benchmarks-compared
title: "Gemini 4 Hydrogen vs Gemini 3.8 Flash: Pricing, Limits and Benchmarks Compared"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-11-12"]
keywords: ["gemini", "flash", "hydrogen", "google", "model", "pricing", "agentic", "carbon"]
source: https://www.datastudios.org/post/gemini-4-hydrogen-vs-gemini-3-8-flash-comparison
source_anchor: ""
source_lines: [1, 857]
section: "Gemini_Google"
---
Gemini 4 Hydrogen vs Gemini 3.8 Flash: Complete Comparison and Report on Pricing, Output Limits, Thinking Levels, and Agentic Benchmarks

Nov 13

16 min read

Gemini 4 Hydrogen is the model Google released on November 12, 2026 as the fast, cost-efficient member of the Gemini 4 series, adapted from Gemini 4 Carbon and positioned by Google as the upgrade path for teams running Gemini 3.8 Flash. Gemini 3.8 Flash arrived on September 2, 2026, built on Gemini 3.7 Flash, and Google described it at launch as its most intelligent workhorse model. Both models accept text, images, audio and video, both have a 1M-token context window, and both stay on sale in the Gemini API today, so developers can choose between them on cost and capability rather than on availability.

The two models are closer in price than they first appear. Hydrogen costs $1.50 per 1M input tokens and $7.50 per 1M output tokens, flat across its full window. Gemini 3.8 Flash costs $0.75 and $3.75 only until December 31, 2026; from January 1, 2027 its standard rate becomes $1.50 and $7.50, the same list price as Hydrogen. On the measurements that both models share, Hydrogen leads clearly: 51 versus 31 on the Artificial Analysis Intelligence Index at the high setting, and 57% versus 18.2% on Terminal-Bench 4.1 in Google's own runs. Gemini 3.8 Flash keeps a real advantage for one more month on raw token cost, and it keeps two lasting advantages: published Batch, Flex and Priority prices, and explicit context caching, neither of which Google has listed for Hydrogen.

The practical split is clear. Agentic coding, tool-heavy workflows and long outputs favor Hydrogen now; high-volume, latency-tolerant batch jobs and workloads that depend on explicit caches favor Gemini 3.8 Flash until Google publishes Hydrogen's batch terms, and the price case for Flash ends on January 1. Prices as listed on Google's official pricing and model pages on November 13, 2026.

··········


GEMINI 4 HYDROGEN AND GEMINI 3.8 FLASH: RELEASE TIMELINE AND MODEL IDENTITY.

Gemini 3.8 Flash was announced on September 2, 2026 together with Gemini 3.8 Flash Cyber, a restricted security variant offered only to trusted defenders through Google's Fairwind Program. The general model went generally available for production use on Google's enterprise agent platform the same day, and Google's model card describes it as the next iteration of the Gemini 3 family with gains in software engineering and agentic knowledge work. Its architecture is the one documented for Gemini 3.7 Flash, and Google kept Gemini 3.7 Flash fully supported for efficiency-first workloads when 3.8 Flash shipped.

Gemini 4 Hydrogen followed ten weeks later. Google's model card states that Hydrogen is adapted from Gemini 4 Carbon and refers to the Carbon card for architecture, training data and hardware, so Hydrogen belongs to a different model generation from Gemini 3.8 Flash. Google calls it a "daily driver for agentic workflows", designed for fast execution on its own or as a sub-agent working beside Gemini 4 Carbon and Gemini 4 Argon. The Gemini API documentation lists Hydrogen as the recommended upgrade from Gemini 3.8 Flash and from earlier Gemini 3 and 2.5 models.

........

Item

Gemini 4 Hydrogen

Gemini 3.8 Flash

Release date

November 12, 2026

September 2, 2026

API model ID

gemini-4-hydrogen

gemini-3.8-flash

Base model

Adapted from Gemini 4 Carbon

Built on Gemini 3.7 Flash

Inputs

Text, images, audio, video, dense PDFs

Text, images, audio, video, PDF

Output

Text

Text

Knowledge cutoff

March 2026

March 2026 for some domains, January 2025 for others

Google's positioning

Daily driver for agentic workflows

Most intelligent workhorse model at launch

Recommended successor listed by Google

None

Gemini 4 Hydrogen

........

Neither model has a retirement date on Google's pages as of November 13. The Gemini 3.8 Flash model page was last updated on October 30, 2026 and still lists it as the stable version, and Google's agent platform release notes recorded regional general availability for 3.8 Flash in Canada and Japan on November 9, three days before Hydrogen launched.

··········

GEMINI 4 HYDROGEN VS GEMINI 3.8 FLASH API PRICING AND THE JANUARY 1 RATE CHANGE.

Hydrogen's price is a single flat rate: $1.50 per 1M input tokens and $7.50 per 1M output tokens, with output billing that includes thinking tokens, and no long-context surcharge anywhere in the 1M window. Cached input costs $0.075 per 1M tokens through implicit caching, a 95% discount that applies automatically to prompts of at least 4,096 tokens. The free tier in Google AI Studio and the Gemini API includes Hydrogen at no charge, subject to rate limits that Google has not yet published for this model.

Gemini 3.8 Flash has a two-stage price list. Google's pricing page shows every rate through December 31, 2026 and a doubled rate starting January 1, 2027, and the 3.8 Flash model card gives the same "regular" prices of $1.50 and $7.50. It also has three service tiers that Hydrogen's documentation does not mention: Batch, Flex and Priority.

........

Price per 1M tokens (USD)

Gemini 4 Hydrogen

Gemini 3.8 Flash until Dec 31, 2026

Gemini 3.8 Flash from Jan 1, 2027

Standard input

$1.50

$0.75

$1.50

Standard output (incl. thinking)

$7.50

$3.75

$7.50

Cached input

$0.075 (implicit only)

$0.075

$0.15

Cache storage per 1M tokens per hour

Not applicable (no explicit caching)

$0.50

$1.00

Batch input / output

Not published

$0.375 / $1.875

$0.75 / $3.75

Flex input / output

Not published

$0.375 / $1.875

$0.75 / $3.75

Priority input / output

Not published

$1.35 / $6.75

$2.70 / $13.50

Free tier

Yes, rate-limited

Yes

Yes

........

Grounding with Google Search is billed per query for Hydrogen according to its developer guide. For Gemini 3.8 Flash the pricing page lists 5,000 free search requests per month, shared across all Gemini 3 and newer models, then $14 per 1,000 requests. Google's main pricing table did not yet include a Hydrogen row on November 13; the Hydrogen prices above come from Google's launch post and the Gemini API "What's new in Gemini 4 Hydrogen" guide. Our Gemini API pricing guide tracks the full Gemini price list across Argon, Carbon and the Flash models.

··········

CONTEXT WINDOW, OUTPUT LIMITS AND THINKING LEVELS IN GEMINI 4 HYDROGEN AND GEMINI 3.8 FLASH.

The headline context figure is identical, 1,048,576 tokens, yet the two models budget it differently. Gemini 3.8 Flash has a 1M input window and a separate cap of 65,536 output tokens. Hydrogen uses one unified window in which input, thinking and output tokens all share the same 1,048,576 tokens. A synchronous Hydrogen request still returns at most 65,536 tokens per hop, the default, but setting `background` to true with a higher `max_output_tokens` lets the model decode up to the full window minus the input. Google suggests 131,072 or 262,144 output tokens as starting values for long multi-step outputs.

Thinking controls changed as well. Gemini 3.8 Flash offers low, medium and high thinking, with medium as the default. Hydrogen offers low, medium, high and a new xhigh level, and high is the default. Both models reject the `minimal` level with an error.

........

Setting

Gemini 4 Hydrogen

Gemini 3.8 Flash

Context window

1,048,576 tokens, shared by input, thinking and output

1,048,576 input tokens

Output cap per synchronous request

65,536 tokens

65,536 tokens

Long output

Up to about 1M tokens with background long decode

Not offered

Thinking levels

Low, medium, high, xhigh

Low, medium, high

Default thinking level

High

Medium

Minimal thinking

Rejected with an error

Rejected with an error

Explicit context caching

Not supported

Supported

Implicit caching

On by default from 4,096 tokens

Caching supported (pricing listed)

Temperature, top_p, top_k

Ignored by the server

Deprecated, Google says to remove them

........

The change of default matters for budgets. A request migrated from 3.8 Flash without an explicit thinking level moves from medium to high on Hydrogen, which raises thinking tokens and therefore output cost. Google's own advice in the Hydrogen guide is to test all four levels with a high `max_output_tokens` and keep the lowest level that meets the quality bar. Our guide to Gemini API context windows and output caps covers the older Flash limits in more depth.

··········

MIGRATING FROM GEMINI 3.8 FLASH TO GEMINI 4 HYDROGEN: BREAKING CHANGES FOR DEVELOPERS.

The Gemini API guide lists the specific edits a 3.8 Flash integration needs before it runs well on Hydrogen. The model ID changes to `gemini-4-hydrogen`. Explicit context caching is gone, so calls to `cachedContents` and `caches.create` must be removed along with any `cached_content` fields, and code that relied on paying storage for a long-lived cache now depends on implicit caching instead. Hardcoded low `max_output_tokens` values such as 8,192 should be removed or raised to 65,536, because the cap covers thinking and visible output together and Hydrogen thinks at high by default.

Two further rules concern agents. Client-managed histories must return earlier model turns and thought signatures unchanged, and a thought signature must not be copied across parallel `functionCall` parts. Tools should be declared natively, since prompting Hydrogen for raw-text JSON tool calls with no declared tools can return a `MALFORMED_FUNCTION_CALL` error. Multi-turn state in the Interactions API uses `previous_interaction_id`, and Google states that Carbon and Hydrogen can be mixed in one session through that field with no request schema changes.

On the capability side, the Gemini 3.8 Flash model page lists code execution, computer use, file search, function calling, Google Maps grounding, Search grounding, structured outputs and URL context, and excludes audio generation, image generation and the Live API. Hydrogen's guide confirms function calling, structured outputs, background execution, parallel read and search tool calls, and agentic video understanding that inspects specific frames. Google had not published a full capability matrix for Hydrogen on its model pages by November 13.

··········

WHERE GEMINI 4 HYDROGEN AND GEMINI 3.8 FLASH ARE AVAILABLE: APP, SEARCH, API AND CLOUD.

Distribution is wider for Hydrogen on consumer surfaces. Google says Hydrogen is available in Google Search for all users globally, in the Gemini app model dropdown, and for free Gemini app users through automatic model selection when it fits the prompt. Gemini 3.8 Flash reached consumers through Google AI Pro and Ultra subscriptions in the Gemini app, AI Mode in Search and Gemini in Google Sheets.

For enterprises the picture is less even. Google's announcement names Gemini for business (Gemini Enterprise) for Hydrogen, through the API or the model dropdown, but does not name Vertex AI or Google's enterprise agent platform, and the platform's release notes contained no Hydrogen entry on November 13. Gemini 3.8 Flash is generally available on that platform and was still the default model in Google's CodeMender tool on November 11.

........

Surface

Gemini 4 Hydrogen

Gemini 3.8 Flash

Gemini API and Google AI Studio

Yes, free and paid tiers

Yes, free and paid tiers

Android Studio

Yes

Yes

Gemini app

Model dropdown; free users through auto selection

Google AI Pro and Ultra at launch

Google Search

All users globally

AI Mode (Pro and Ultra at launch)

Gemini in Google Sheets

AI Pro, Ultra and eligible business plans

AI Pro and Ultra at launch

Google Antigravity

Paid AI Pro and Ultra; Orchestrator for Ultra only

Yes

Gemini Enterprise

Yes

Yes

Vertex AI / enterprise agent platform

Not listed as of Nov 13, 2026

Generally available since Sep 2, 2026

Batch, Flex, Priority API tiers

Not published

Yes

........

In Google Antigravity, Hydrogen is rolling out to paid AI Pro and Ultra subscribers, and a new Orchestrator option that pairs Hydrogen with Gemini 4 Carbon is limited to AI Ultra. Google's Antigravity post reports that on Terminal-Bench 4.1 the Orchestrator reached near-Carbon task success while consuming 24% less quota and finishing 19% faster; the launch post cites the same figures against Terminal-Bench 4.0, one of several version inconsistencies in Google's materials. The Hydrogen launch article covers the consumer rollout in more detail.

··········

BENCHMARK METHODOLOGY AND SOURCE RELIABILITY FOR THE HYDROGEN AND 3.8 FLASH SCORES.

Google did not publish a single table that places Hydrogen and Gemini 3.8 Flash side by side across many benchmarks. Hydrogen's model card compares it with Claude Sonnet 5.5 and GPT-6.1 Terra, and the 3.8 Flash card compares it with Gemini 3.7 Flash, Claude Opus 5, Claude Sonnet 5, GPT-5.6 Sol and GPT-5.6 Terra. Direct pairs therefore come from two places: the explicit head-to-head figures in Google's Hydrogen launch post, and Artificial Analysis, which runs its Intelligence Index on both models independently.

Google's evaluation document for Hydrogen states that its scores are pass@1 unless noted, single attempt with no majority voting, run through the Gemini API at the highest thinking setting unless noted. Terminal-Bench 4.1 used the mini-swe-agent vision setup with five attempts per task, Average Reward, high effort and cheating-adjusted accuracy. The Gemini 3.8 Flash card gives fewer settings, labeling Terminal-bench 4.0 as "general agent capabilities" and OSWorld-2.0 as a partial score with the batch tool enabled.

Several benchmarks changed version between the two launches, which rules out naive comparison. Hydrogen reports OSWorld-2.1 on an 82-task offline subset, while 3.8 Flash reports OSWorld-2.0. Hydrogen reports HLE-Diamond from the Artificial Analysis leaderboard, while 3.8 Flash reports HLE-Verified. Google's 3.8 Flash card lists 19.1% on Terminal-bench 4.0, and the Hydrogen launch post gives 18.2% for 3.8 Flash on Terminal-Bench 4.1.

··········

[[ADPLUS_300x250_1]]

GEMINI 4 HYDROGEN VS GEMINI 3.8 FLASH ON DIRECTLY COMPARABLE MEASUREMENTS.

The table keeps only figures where the same test, version and source cover both models. The Artificial Analysis figures were read on the organization's model pages on November 13, 2026; the Terminal-Bench 4.1, speed and fabrication figures are Google's own claims from the November 12 launch post.

........

Measurement

Setting

Gemini 4 Hydrogen

Gemini 3.8 Flash

Source

Artificial Analysis Intelligence Index

High thinking

51

31

Artificial Analysis

Artificial Analysis Intelligence Index

Low thinking

27

23

Artificial Analysis

Artificial Analysis Intelligence Index

Hydrogen xhigh / medium

54 / 41

No xhigh level; medium not listed

Artificial Analysis

Terminal-Bench 4.1

High effort, 5 attempts, cheating-adjusted

57%

18.2%

Google launch post

Task completion speed

Hydrogen at default high

2.5x faster

Baseline

Google launch post

Fabricated claims

Google internal measurement

More than 5x fewer

Baseline

Google launch post

Partner workflows (graph, SQL, data-heavy)

Partner testing

2x to 2.8x faster, 12% to 23% fewer tokens

Baseline

Google launch post

........

The Intelligence Index gap at high thinking is 20 points, and Hydrogen at medium thinking (41) already scores ten points above Gemini 3.8 Flash at high. On Terminal-Bench 4.1, Hydrogen's 57% is a little more than three times the 18.2% Google reports for 3.8 Flash. Google also places Hydrogen at default high one point above Gemini 4 Argon on the Intelligence Index, 51 against 50, which puts a Flash-priced model level with the frontier Argon tier on that composite.

··········

BENCHMARKS PUBLISHED FOR ONLY ONE MODEL: HYDROGEN'S AGENT TESTS AND 3.8 FLASH'S DOMAIN TESTS.

Most of the published numbers cover one model only, because each card was written against the competitors of its own month. Hydrogen's card focuses on newer agent and knowledge-work tests, while the 3.8 Flash card leans on coding, finance, legal and biology suites from late summer. These figures describe each model's strengths but cannot be subtracted from each other.

........

Benchmark (one model only)

Score

Model

Notes from Google's pages

SWE-sweep

13.5%

Gemini 4 Hydrogen

Antigravity CLI 1.4.8, subtask-weighted reward

Agents' Last Exam v1.1

71.6%

Gemini 4 Hydrogen

122 tasks, 5-hour window, rubric score

AutomationBench-AA

76.1%

Gemini 4 Hydrogen

Artificial Analysis leaderboard

Vending-Bench 2

$14,549 bank balance

Gemini 4 Hydrogen

Andon Labs leaderboard

HLE-Diamond

58.8%

Gemini 4 Hydrogen

Artificial Analysis leaderboard

OSWorld-2.1 (82-task offline subset)

77.6%

Gemini 4 Hydrogen

Best of 3 runs, screenshot-only

MKA / Chartography

53.4% / 66.6%

Gemini 4 Hydrogen

Self-computed by Google

Vals Tax Agent / Legal Research

52.3% / 55.8%

Gemini 4 Hydrogen

Ranked first / tied first, per Google

DeepSWE v1.1

73.7%

Gemini 3.8 Flash

Long-horizon software engineering

Terminal-bench 2.1 / 4.0

89.4% / 19.1%

Gemini 3.8 Flash

Agentic terminal coding / general agent

GDPVal-AA v2

1545 Elo

Gemini 3.8 Flash

Knowledge work

Vals Finance Agent v2 / Harvey Legal Agent

61.4% / 10.0%

Gemini 3.8 Flash

Harvey: all-pass rate

HLE-Verified

54.9%

Gemini 3.8 Flash

Multidisciplinary reasoning

OSWorld-2.0

59.0%

Gemini 3.8 Flash

Partial score, batch tool enabled

CharXiv Reasoning / LVBench

86.2% / 87.8% agentic

Gemini 3.8 Flash

No tools / long video

........

Two pairs in this table invite comparison and should be read with care. OSWorld-2.1 at 77.6% for Hydrogen and OSWorld-2.0 at 59.0% for 3.8 Flash use different benchmark versions and task sets. HLE-Diamond at 58.8% and HLE-Verified at 54.9% are different subsets of Humanity's Last Exam. In the competitor columns of its own card, Hydrogen trails Claude Sonnet 5.5 on Terminal-Bench 4.1 (57.0% against 63.9%), OSWorld-2.1 (77.6% against 83.9%) and Terminal-Bench-Science 0.1 (51.4% against 59.9%), which shows where Google's new Flash-tier model still sits below a mid-tier rival.

··········

INDEPENDENT EVALUATIONS OF GEMINI 4 HYDROGEN AND GEMINI 3.8 FLASH BY ARTIFICIAL ANALYSIS.

Artificial Analysis is the only independent organization with published results for both models at the time of writing. Its pages also report how many tokens each model spent to complete the Intelligence Index, which is the closest public proxy for verbosity and real cost.

........

Artificial Analysis metric (read Nov 13, 2026)

Hydrogen high

Hydrogen medium

Hydrogen xhigh

3.8 Flash high

3.8 Flash low

Intelligence Index

51

41

54

31

23

Output tokens to run the Index

110M

54.4M

304M

126M

Not listed

Input tokens to run the Index

5.34B

2.07B

11.3B

10.2B

Not listed

Cost per Index task (at AA's listed prices)

$1.87

$0.80

$3.16

$7.77

Not listed

Output speed

Not listed

177 tokens/s

Not listed

112 tokens/s

102 tokens/s

Time to first answer token

Not listed

Not listed

Not listed

17.53 s

4.40 s

Blended price (7:2:1 cache/input/output)

$1.10

$1.10

$1.10

$0.58

Not listed

........

The token columns favor Hydrogen at matched settings. At high thinking, Hydrogen used 110M output tokens and 5.34B input tokens to complete the Index, against 126M and 10.2B for Gemini 3.8 Flash, and Artificial Analysis lists a lower cost per task for Hydrogen high ($1.87) than for 3.8 Flash high ($7.77), even though Flash is priced at the introductory $0.75 and $3.75 on that page. The xhigh level reverses the picture: 304M output tokens, ranked 76th of 77 models for verbosity by Artificial Analysis, for three more Index points. Artificial Analysis had not yet published output speed or latency for Hydrogen at high or xhigh on November 13, so Google's "2.5x faster" claim has no independent confirmation yet.

··········

COST PER TASK FOR GEMINI 4 HYDROGEN AND GEMINI 3.8 FLASH ON THREE TYPICAL WORKLOADS.

The figures below were computed in Python from the official list prices above, before and after Gemini 3.8 Flash's January 1 change. The assumptions are stated for each workload; token counts are held equal across models unless a row says otherwise, which leaves out Hydrogen's different thinking default.

Workload A is a coding-agent task with 400,000 input tokens per task, 70% of them cache hits, and 40,000 output tokens including thinking. Workload B is a customer-support chatbot handling 10,000 requests a day for 30 days, with 3,000 input tokens per request (2,000 of them a cached system prompt) and 400 output tokens. Workload C is an overnight summarization run of 1,000 documents at 50,000 input tokens and 2,000 output tokens each, with no caching, sent through the Batch API where a batch price exists.

........

Workload

Gemini 4 Hydrogen

Gemini 3.8 Flash until Dec 31

Gemini 3.8 Flash from Jan 1

A: one coding-agent task

$0.501

$0.261

$0.522

A: 1,000 coding-agent tasks

$501

$261

$522

A: Hydrogen with 12% fewer tokens (partner low end)

$0.441

n/a

n/a

A: Hydrogen with 23% fewer tokens (partner high end)

$0.386

n/a

n/a

B: support chatbot, one month

$1,395

$720

$1,440

C: 1,000-document batch run

$90 (standard rate, no batch price)

$22.50 (Batch)

$45 (Batch)

........

Through December 31, Gemini 3.8 Flash costs roughly half as much as Hydrogen on every workload at equal token counts. From January 1, the interactive workloads converge: Hydrogen is 3% to 4% cheaper on workloads A and B because its cached-input rate stays at $0.075 while 3.8 Flash's doubles to $0.15. If the 12% to 23% token savings that Google's partners reported hold on a given workload, Hydrogen's coding-agent task falls to between $0.386 and $0.441. The batch workload stays the clear exception: with no published batch rate for Hydrogen, Gemini 3.8 Flash remains half the price even after January 1. Our Hydrogen vs GPT-6.1 Terra and Claude Sonnet 5.5 cost comparison runs similar arithmetic against OpenAI and Anthropic's mid-tier models.

··········


SAFETY RESULTS, FRONTIER SAFETY FRAMEWORK STATUS AND KNOWN LIMITATIONS OF BOTH MODELS.

Google's Hydrogen card reports automated safety results against Gemini 3.8 Flash directly. Text-to-text safety improved by 0.80 percentage points, multilingual safety by 1.10 points and image-to-text safety by 7.00 points, while tone slipped by 0.65 points and unjustified refusals rose by 5.57 points, a regression Google attributes mainly to multimodal prompts. The launch post adds agentic claims: up to 81% less overreach, up to 79% less under-service, 12x fewer misaligned actions and shortcuts across tool-use and coding evaluations, up to 2x lower likelihood of bypassing permission boundaries, 96.7% on Google's internal agent honesty evaluation, and more than 2.3x higher resilience to indirect prompt injection on Gray Swan's test.

Under Google's Frontier Safety Framework the two models sit at different levels. The 3.8 Flash card states that the model has no meaningful new capabilities in framework domains compared with Gemini 3.7 Flash, which reached no tracked or critical capability level, and on that basis considers 3.8 Flash unlikely to reach any. Hydrogen was judged to show no material capability increase over Gemini 4 Argon, but targeted tests found that it reached the alert thresholds for the biological CBRN uplift and cybersecurity uplift critical capability levels without reaching the levels themselves, and Google says mitigations continue.

........

Safety item

Gemini 4 Hydrogen

Gemini 3.8 Flash

Frontier Safety Framework basis

Targeted CBRN and cyber tests; other domains carried forward from Gemini 4 Argon

Carried forward from Gemini 3.7 Flash (no T/CCL reached)

Biological CBRN uplift

Alert threshold reached, CCL not reached

Considered unlikely to reach any T/CCL

Cybersecurity uplift

Alert threshold reached, CCL not reached

Considered unlikely to reach any T/CCL

Unjustified refusals vs the other model

Up 5.57 points vs 3.8 Flash

Up 1.1 points vs 3.7 Flash

Full safety and security report

Listed as "coming soon"

Model card only

Known limitations

Hallucination, jailbreaks, timeouts, high token use at high levels

Hallucination, jailbreaks, timeouts, high token use at high effort

........

The two cards use opposite sign conventions for some safety rows, with Hydrogen's table marked "higher is better" and the 3.8 Flash table marked "lower is better" for the same categories, so the figures above are quoted as each card states them.

··········

WHAT GOOGLE HAS NOT PUBLISHED FOR GEMINI 4 HYDROGEN AND GEMINI 3.8 FLASH.

The gaps fall mostly on the newer model. Hydrogen has no published Batch, Flex or Priority prices, no numeric rate limits on Google's rate-limit page, no row on Google's main Gemini API pricing table as of November 13, and no listing on Google's enterprise agent platform. Its full safety and security report is still pending, and Artificial Analysis had not measured its speed at the default high setting.

For Gemini 3.8 Flash, Google never ran the newer agent suites that headline Hydrogen's card, such as SWE-sweep, Agents' Last Exam v1.1 or AutomationBench-AA, and Hydrogen's card does not report the Gemini 3.8 Flash suites such as DeepSWE v1.1, Vals Finance Agent v2 or Terminal-bench 2.1. Google's Search post for Hydrogen does not say which model it replaced, and the comparison screenshots in the main launch post are captioned against Gemini 3.5 Flash while the text compares against Gemini 3.8 Flash. The launch post also cites Terminal-Bench 4.0 and 4.1 for the same Orchestrator result, and one chart's alt text reads "AA Index 5.01" where the text says 51.

··········

WHICH TO CHOOSE: GEMINI 4 HYDROGEN OR GEMINI 3.8 FLASH BY WORKLOAD.

The deciding facts are a 20-point Intelligence Index lead and a threefold Terminal-Bench 4.1 lead for Hydrogen, a price gap that closes on January 1, and batch and caching options that only Gemini 3.8 Flash documents today.

........

Workload or situation

Recommended model

Reason

Agentic coding and terminal tasks

Gemini 4 Hydrogen

57% vs 18.2% on Terminal-Bench 4.1 in Google's runs

Long generated outputs above 64K tokens

Gemini 4 Hydrogen

Background long decode up to the 1M window

Interactive chat or agents from January 2027

Gemini 4 Hydrogen

Same list price, cheaper cached input ($0.075 vs $0.15)

Lowest cost through December 31, 2026

Gemini 3.8 Flash

$0.75 / $3.75 vs $1.50 / $7.50

Overnight batch processing

Gemini 3.8 Flash

Batch at $0.375 / $1.875 now, $0.75 / $3.75 in 2027; no Hydrogen batch price

A team running an agent or coding assistant on Gemini 3.8 Flash should test Hydrogen now and plan the switch before January 1, when the introductory Flash rate expires and Hydrogen becomes the cheaper of the two on cached interactive traffic. A team running nightly batch jobs on Gemini 3.8 Flash should stay put, because a 50% batch discount with no Hydrogen equivalent outweighs Hydrogen's benchmark lead for work that does not need it. Teams whose code depends on explicit caches face a rewrite before migrating, and that work, more than price, sets their timetable.
