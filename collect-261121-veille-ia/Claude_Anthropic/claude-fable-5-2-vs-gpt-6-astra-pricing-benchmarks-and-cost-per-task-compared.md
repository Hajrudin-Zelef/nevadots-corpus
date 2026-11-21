---
id: collect-261121-veille-ia/claude-anthropic/claude-fable-5-2-vs-gpt-6-astra-pricing-benchmarks-and-cost-per-task-compared
title: "Claude Fable 5.2 vs GPT-6 Astra: Pricing, Benchmarks and Cost per Task Compared"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-11-09"]
keywords: ["fable", "astra", "gpt-6", "model", "claude", "tokens", "anthropic", "openai"]
source: https://www.datastudios.org/post/claude-fable-5-2-vs-gpt-6-astra-comparison
source_anchor: ""
source_lines: [1, 779]
section: "Claude_Anthropic"
---
Claude Fable 5.2 vs GPT-6 Astra: Complete Comparison and Report on Pricing, Benchmarks, Cost per Task, and Long-Context Rates

Nov 10

15 min read

Claude Fable 5.2 is the model Anthropic released on November 9, 2026 as the generally available member of its Fable 5.2 and Mythos 5.2 pair, with API model ID claude-fable-5-2, a 1M-token context window and list prices of $8 per million input tokens and $40 per million output tokens. GPT-6 Astra is the OpenAI flagship that arrived on September 3, 2026, sold through the API at $10 and $50 per million tokens with a 1,050,000-token context window, and offered inside ChatGPT Work and Codex. Both expose five reasoning effort levels and cap a single response at 128,000 output tokens.

On list prices Fable 5.2 is 20% cheaper per token than Astra, and the gap widens to $8 against $20 per million input tokens once an Astra prompt passes 272,000 tokens, where OpenAI doubles input rates and raises output rates by half for the whole request. On the benchmark table Anthropic published at launch, Fable 5.2 scores above GPT-6 Astra on all 11 rows where both models have a figure, including FrontierSWE v2 (69.9% against 65.5%), OSWorld 2.1 (89.3% against 79.5%) and Humanity's Last Exam with tools (74.2% against 57.2%). Artificial Analysis ranks Fable 5.2 at max effort first on its Intelligence Index with 65 points against 57 for Astra at max effort. The same independent measurements also show the cost of that lead: Fable 5.2 at max effort spent $26.54 per Intelligence Index task against $6.24 for Astra, because it consumed about five times as many output tokens.

The practical split follows from those numbers. Fable 5.2 is the stronger choice when the score on hard, long-running tasks decides the outcome and when prompts routinely exceed 272,000 tokens; GPT-6 Astra is the cheaper model per completed task at the effort levels Artificial Analysis has priced, offers zero data retention to approved API customers, and is reachable from a $20 ChatGPT Plus subscription with limited usage. All prices in this report were checked on the official Anthropic and OpenAI pricing pages on November 10, 2026.

··········


CLAUDE FABLE 5.2 AND GPT-6 ASTRA: RELEASE TIMELINE AND MODEL IDENTITY.

GPT-6 Astra came first. OpenAI's system card for the model is dated September 3, 2026, and it describes Astra as the company's first model to reach the Critical level of cybersecurity capability under its Preparedness Framework. Artificial Analysis and Epoch AI both list September 3 as the release date. OpenAI has since published GPT-6.1 Sol and GPT-6.1 Terra as addenda to the Astra system card, and GPT-6.1 Sol, listed at $2 and $10 per million tokens, sits well below Astra on the API price list.

Claude Fable 5.2 replaces Fable 5.1 at a lower price. Anthropic describes Fable 5.2 and Mythos 5.2 as the same underlying model: Fable 5.2 ships with cyber and biology safeguards and is available to every API customer, while Mythos 5.2 lacks some of those safeguards and is limited to organizations admitted to Anthropic's Cyber Verification Program or Life Sciences Verification Program. This comparison covers Fable 5.2, the version any developer or subscriber can use. Our launch report on Claude Fable 5.2 and its price cut covers the release itself.

........

Item

Claude Fable 5.2

GPT-6 Astra

Developer

Anthropic

OpenAI

Release date

November 9, 2026

September 3, 2026

API model ID

claude-fable-5-2

gpt-6-astra

Amazon Bedrock ID

anthropic.claude-fable-5-2

openai.gpt-6-astra

Knowledge cutoff

June 2026

April 30, 2026

Input modalities

Text and images

Text and images

Output

Text

Text

Restricted sibling

Claude Mythos 5.2 (verified organizations)

GPT-6 Cyber (separate trusted-access model)

Retirement commitment

Not sooner than November 9, 2027

Not stated on the model page

........

Fable 5.2's training data runs about two months later than Astra's, and Anthropic commits on the model page to keep Fable 5.2 available until at least November 9, 2027, a commitment absent from the Astra page.

··········

API PRICING: CLAUDE FABLE 5.2 AT $8/$40 AGAINST GPT-6 ASTRA AT $10/$50.

The base rates put Fable 5.2 a fifth below Astra on both input and output tokens. Cache pricing differs in structure as well as level. Anthropic charges $0.20 per million tokens for cache reads on Fable 5.2, which is 2.5% of the input rate, and sells cache writes with a five-minute or one-hour lifetime. OpenAI charges $1.00 per million cached input tokens on Astra, a 90% discount, and sells cache writes with a 30-minute or 12-hour lifetime. Both vendors halve standard rates for batch work, and OpenAI applies the same 50% rate to its Flex tier.

........

Rate (USD per 1M tokens)

Claude Fable 5.2

GPT-6 Astra

Input

$8.00

$10.00

Output

$40.00

$50.00

Cache read

$0.20

$1.00

Cache write, short lifetime

$10.00 (5 minutes)

$12.50 (30 minutes)

Cache write, long lifetime

$16.00 (1 hour)

$25.00 (12 hours)

Batch input / output

$4.00 / $20.00

$5.00 / $25.00 (Batch and Flex)

Fast mode input / output

$16.00 / $80.00

$20.00 / $100.00

Ultrafast mode input / output

Not offered

$60.00 / $300.00

Prompts above 272K input tokens

No surcharge

2x input and cache rates, 1.5x output, whole request

US-only or regional processing

1.1x all token rates

10% uplift on regional endpoints for models released on or after March 5, 2026

........

The fast tiers keep the same ratio. Fable 5.2 fast mode costs exactly double its standard rates and, according to Anthropic, generates tokens up to 2.5 times faster; it is available on the Claude Platform, in Claude Code, on Claude models in Google Cloud and in Anthropic-hosted Microsoft Foundry deployments, and it cannot be combined with the Batch API. OpenAI's fast mode for Astra also costs twice the standard rates, and OpenAI notes that it renamed its former priority processing tier to Fast mode on July 30, 2026. Astra adds an Ultrafast tier at six times standard rates, which has no counterpart on the Anthropic side.

The cheaper per-token rate needs one qualification. Anthropic's pricing page states that Claude models from 4.7 onward, Fable 5.x included, use a tokenizer that produces roughly 30% more tokens for the same text than earlier Claude models. Tokens from two vendors' tokenizers are different units, so the per-token gap describes the rate card and says little by itself about the cost of the same document.

··········

CONTEXT WINDOW, OUTPUT LIMITS AND EFFORT CONTROLS IN FABLE 5.2 AND ASTRA.

The two context windows are close in size: 1M tokens for Fable 5.2 and 1,050,000 tokens for Astra. The commercial terms around them are far apart. Anthropic includes the full 1M-token window at standard pricing for Claude 4.6 and later models, so a 900,000-token prompt to Fable 5.2 is billed at $8 per million input tokens. OpenAI bills every Astra request whose prompt exceeds 272,000 input tokens at twice the input and cache rates and 1.5 times the output rate, which turns $10/$50 into $20/$75 for the entire request.

........

Control

Claude Fable 5.2

GPT-6 Astra

Context window

1,000,000 tokens

1,050,000 tokens

Long-context pricing

Standard rates across the full window

Surcharge above 272K input tokens

Maximum output

128,000 tokens

128,000 tokens

Effort levels

low, medium, high, xhigh, max

low, medium, high, xhigh, max

Default effort (API)

high

Not stated on the model page

Default effort (Claude apps and Claude Code)

medium

Not applicable

Thinking off switch

None, adaptive thinking is always on

Not documented on the model page

Forced tool choice

any and named tool return a 400 error

Function calling and structured outputs supported

Sampling parameters

temperature, top_p and top_k must be omitted

Not listed

........

Fable 5.2 changes how developers control reasoning. Adaptive thinking is always on: requests that set thinking to enabled with a token budget, or to disabled, return a 400 error, and the effort parameter is the only lever. Anthropic recommends starting at high, moving to xhigh for long coding and agentic work, and keeping max for measuring the ceiling. Its migration notes add that turns can end early on long multistep tasks at low and medium effort.

Astra's API page lists the same five effort names and a wider set of built-in Responses API tools, including web search, file search, code interpreter, a hosted shell, apply patch, computer use, MCP and tool search. Fine-tuning is not supported for Astra.

··········

ACCESS TO CLAUDE FABLE 5.2 AND GPT-6 ASTRA BY PLAN AND PLATFORM.

Consumer access is where the two models diverge most for individual users. On Claude plans, Fable 5.2 is included in the subscription only on Max ($100 a month), Ultra ($200 a month) and premium seats on Team and Enterprise, where it can use up to half of the weekly usage limit. Pro subscribers and Team standard seats reach Fable 5.2 through paid usage credits, with one exception: from November 9 to November 16, 2026, Anthropic's launch offer includes Fable models for up to 50% of weekly limits on Pro and Team standard seats and adds one Fable reset. Max, Ultra and premium seats receive one to three Fable resets depending on subscription length, valid until March 8, 2027.

On the OpenAI side, ChatGPT's pricing page lists GPT-6 Astra for Plus ($20 a month) and Standard Business with an estimated 5 to 45 local messages per five-hour period, used through ChatGPT Work and Codex, which share one allowance. Codex pricing states that the $100 and $200 Pro tiers and the $100 Business tier include Astra within the existing Codex allowance. Ultrafast mode is limited to the $500 Pro tier and eligible Enterprise and Edu plans.

........

Access route

Claude Fable 5.2

GPT-6 Astra

Free plan

Not available

Not listed

Entry paid plan

Pro ($20/mo or $17/mo annual): usage credits only

Plus ($20/mo): 5-45 messages per 5 hours in Work and Codex

Mid tier

Max ($100/mo): up to 50% of weekly limits

Pro $100 and $200: included in Codex allowance

Top individual tier

Ultra ($200/mo): up to 50% of weekly limits

Pro $500: adds Ultrafast mode

Team or Business

Premium seat ($100/seat annual): included; standard seat: credits

Standard Business: 5-45 messages per 5 hours; Business $100 tier: included

Enterprise

Premium seats included; usage-based plans at API rates

Admin enables under rate card

Coding tool

Claude Code 2.1.321 or later, fast mode available

Codex

API

Claude API, all customers

OpenAI API, from the Build tier (5,000 RPM, 1M TPM)

Cloud platforms

Amazon Bedrock, Claude Platform on AWS, Google Cloud (both managed modes), Microsoft Foundry

Amazon Bedrock; Google Cloud in private preview

........

For a reader on a $20 plan the difference is concrete. Astra is reachable on ChatGPT Plus within a small message allowance, while Fable 5.2 on Claude Pro is billed as usage credits after the launch week ends on November 16. Our guide to Claude usage limits across Free, Pro, Max, Team and Enterprise explains how weekly and five-hour limits interact on those plans.

··········

HOW THE FABLE 5.2 AND ASTRA BENCHMARK FIGURES WERE PRODUCED.

Three kinds of evidence exist for this pair, and they carry different weight. The largest set of head-to-head numbers comes from Anthropic's launch page for Fable 5.2, which includes GPT-6 Astra as a comparison column. Anthropic states that scores are reported with safeguards on unless marked otherwise and that each model is shown at its highest available effort unless marked otherwise. These are vendor-published figures from the company selling one of the two models. One footnote states that RoboDojo Sim was run internally by Anthropic.

OpenAI has not published a benchmark table that includes Fable 5.2, which is expected given the release order, and its main Astra announcement page could not be retrieved when this report was prepared.

The third source is independent measurement. Artificial Analysis has scored both models at all five effort levels on its Intelligence Index and published cost, token and speed data for each. Epoch AI lists Astra on its Capabilities Index. Independent figures in this report are attributed by organization and were read on November 10, 2026.

··········

FABLE 5.2 VS ASTRA ON ANTHROPIC'S PUBLISHED BENCHMARK TABLE.

The table below keeps only rows where Anthropic printed a figure for both Fable 5.2 and GPT-6 Astra, with Fable 5.1 shown for reference. Unless marked, each model ran at its highest available effort.

........

Benchmark (Anthropic launch table)

Claude Fable 5.2

GPT-6 Astra

Gap

Claude Fable 5.1

FrontierCode 2.0 (agentic coding)

49.1% (xhigh)

42.8%

+6.3 pts

32.4% (xhigh)

FrontierSWE v2

69.9%

65.5%

+4.4 pts

56.3%

AutomationBench (business workflows)

51.4%

41.4%

+10.0 pts

31.4%

Terminal-Bench-Science 0.1

74.6%

68.1%

+6.5 pts

52.6%

OSWorld 2.1 (offline, partial credit)

89.3%

79.5%

+9.8 pts

83.5%

RoboDojo Sim (run by Anthropic)

64.9%

29.8%

+35.1 pts

27.6%

AA-Briefcase v1.1 (rubric)

66.1%

52.0%

+14.1 pts

61.5%

AA-Briefcase v1.1 (Elo)

1785

1569

+216

1669

Remote Labor Index

41.3% (high)

20.8%

+20.5 pts

17.9%

Humanity's Last Exam (with tools)

74.2%

57.2%

+17.0 pts

65.6%

Chartography (no tools)

73.4%

71.0%

+2.4 pts

44.8%

........

Fable 5.2 leads on all 11 rows. The narrowest gaps are on Chartography (2.4 points) and FrontierSWE v2 (4.4 points). The widest gaps are on RoboDojo Sim, which Anthropic ran itself, and on the Remote Labor Index, where Fable 5.2 was measured at high effort and still doubled Astra's score. The FrontierCode 2.0 figure for Fable 5.2 used xhigh effort; Anthropic adds that Fable 5.2 scores 45.3% at high effort, which still sits above Astra's 42.8%.

CyScenarioBench was left out because it pits Mythos 5.2 against GPT-6 Cyber on lab-specific tasks that Anthropic's own footnote calls non-comparable.

··········

[[ADPLUS_300x250_1]]

BENCHMARKS ONLY ONE VENDOR PUBLISHED FOR FABLE 5.2 OR ASTRA.

Several results exist for one model only. For Fable 5.2, Anthropic reports 82.1% on WANDR (relaxed score) against 72.3% for Opus 5.5, and 83.5 on BenchCAD 2.1 at max effort against 65.3 for Opus 5.5 and 44.4 for Fable 5.1, with no Astra figure on either. The cyber and biology rows on the launch page belong to Mythos 5.2, which is not the model a general customer receives.

For Astra, OpenAI's post on Astra for work cites 74% on DeepSWE v1.1 with no stated effort or setup, and claims 74.7% fewer unintended outcomes than Fable 5.1 on an internal computer-use safety benchmark. The Astra system card reports HealthBench Professional at 64.7 against 60.5 for GPT-5.6 Sol, indirect prompt-injection resistance of 99.79% on OpenAI's internal test, and an 8.5% attack success rate on the Gray Swan IPI Arena with 15 attempts per scenario. None of these has a Fable 5.2 counterpart, so they place Astra against OpenAI's earlier models and say nothing direct about Anthropic's new one.

··········

INDEPENDENT EVALUATIONS: ARTIFICIAL ANALYSIS AND EPOCH AI.

Artificial Analysis publishes Intelligence Index scores for each effort setting, which allows a like-for-like reading across the full range of effort.

........

Effort level

Fable 5.2 Index

Astra Index

Fable 5.2 time per task

Astra time per task

Fable 5.2 output speed

Astra output speed

Max

65

57

46.4 min

22.6 min

98 tok/s

38 tok/s

Xhigh

62

55

26.4 min

11.6 min

102 tok/s

38 tok/s

High

62

55

17.9 min

9.9 min

91 tok/s

39 tok/s

Medium

56

52

8.3 min

6.9 min

86 tok/s

44 tok/s

Low

54

43

6.5 min

3.8 min

91 tok/s

40 tok/s

........

At every effort level Fable 5.2 scores higher on the Index, with the gap ranging from 4 points at medium to 11 points at low. Fable 5.2 at max effort is ranked first of 74 models; Astra at max effort is ranked eighth. Fable 5.2 at high effort (62) already exceeds Astra at max (57). Astra finishes each Index task faster at every level, roughly half the time at max, xhigh and high, even though Fable 5.2 streams tokens about 2.6 times faster, because Fable 5.2 generates far more tokens per task.

Artificial Analysis also reports a blended price of $5.74 per million tokens for Fable 5.2 and $7.70 for Astra, computed with a 7:2:1 ratio of cache hits, input and output. Epoch AI lists GPT-6 Astra with an Epoch Capabilities Index of 169, ranked first among its tracked models, and 98% on FrontierMath Tier 4 (v2); the Epoch page for Fable 5.2 could not be retrieved, so no ECI comparison is possible here.

··········

OUTPUT-TOKEN CONSUMPTION AND COST PER COMPLETED TASK FOR FABLE 5.2 AND ASTRA.

Token consumption reverses the per-token price advantage. To complete the Intelligence Index at max effort, Fable 5.2 used 275 million output tokens and 28.3 billion input tokens; Astra used 54.3 million output tokens and 1.68 billion input tokens. Fable 5.2 therefore generated about 5.1 times as many output tokens and processed about 16.8 times as many input tokens. Artificial Analysis puts the resulting cost per Index task at $26.54 for Fable 5.2 at max effort and $6.24 for Astra at max effort, a ratio of about 4.3 to 1. At low effort the figures are $3.10 and $1.42, a ratio of about 2.2 to 1. Artificial Analysis has not published per-task costs for the medium, high and xhigh settings.

Measured cost per task is the better guide when the model decides how long a job runs. When the application fixes the token counts, the rate card applies directly; the table below computes three such workloads in Python with identical token counts for both models.

........

Workload (assumptions)

Claude Fable 5.2

GPT-6 Astra

Astra vs Fable

Coding agent session: 60 turns, 250K context per turn, 92% cache reads, 8% new tokens written to cache, 1,500 output tokens per turn

$18.36

$33.30

1.81x

Long-document review: one 600K-token prompt, 10K-token answer, no cache

$5.20

$12.75

2.45x

Same review at 200K-token prompt (below Astra's threshold)

$2.00

$2.50

1.25x

Support chatbot: 200,000 requests a month, 3,000 cached system tokens, 1,000 new input tokens, 500 output tokens each

$5,720

$7,600

1.33x

Artificial Analysis Index task, max effort (measured)

$26.54

$6.24

0.24x

Artificial Analysis Index task, low effort (measured)

$3.10

$1.42

0.46x

........

The arithmetic behind the agent row is per turn: Fable 5.2 costs 20,000 new tokens at the $10 five-minute cache-write rate ($0.20), 230,000 cache reads at $0.20 per million ($0.046) and 1,500 output tokens at $40 per million ($0.06), or $0.306 a turn and $18.36 for 60 turns. Astra costs $0.25 for the same writes at its $12.50 30-minute rate, $0.23 for cache reads at $1.00 per million and $0.075 for output, or $0.555 a turn and $33.30 in total. Astra's 90% cache discount is smaller than Anthropic's 97.5%, which is why cache-heavy agent loops favor Fable 5.2 when token counts are held equal.

The long-document row shows the 272,000-token threshold at work. At 600,000 input tokens Astra's surcharge raises the request cost to $12.75 against $5.20 on Fable 5.2; at 200,000 tokens the gap shrinks to the plain 20% list-price difference.

··········

SPEED AND LATENCY: FABLE 5.2 STREAMS ABOUT 2.6 TIMES FASTER THAN ASTRA.

Artificial Analysis measured Fable 5.2 at max effort at 98 output tokens per second against 38 for Astra at max effort, a ratio of about 2.6. Anthropic separately states that Fable 5.2 outputs are up to 30% faster than Fable 5.1. Time to first answer token, which includes thinking time, was 215.95 seconds for Fable 5.2 and 232.57 seconds for Astra at max effort, so both models think for several minutes before answering at that setting. At low effort Fable 5.2's time to first answer token falls to 6.54 seconds.

Because Fable 5.2 produces several times more tokens per task, its faster stream still leaves it taking about twice as long as Astra per Index task at max, xhigh and high effort. Fast mode on either model cuts wall-clock time at double the standard rates.

··········


SAFEGUARDS, REFUSAL FALLBACKS AND DATA RETENTION IN FABLE 5.2 AND ASTRA.

The two vendors handle high-risk requests in different ways, and the difference affects production code. Fable 5.2 runs cyber and biology classifiers that block high-risk cyber activity and most research biology tasks. A declined request returns HTTP 200 with stop_reason set to refusal, and on the Claude API and Anthropic-managed Google Cloud deployments a beta fallbacks setting retries refused requests on Claude Opus 4.8 or Claude Opus 5, depending on the category. Anthropic reports about 55% fewer blocks than Fable 5.1, with about 60% fewer in cybersecurity and 40% fewer in biology.

OpenAI's system card classifies Astra as Critical for cybersecurity capability and lists Trusted Access for Cyber, actor-level enforcement, realtime misuse monitors and a more conservative refusal boundary for users flagged as high risk. It reports a misaligned outcome rate of 3.4% for Astra in realistic work environments against 18.8% for GPT-5.6 Sol.

........

Safety and data item

Claude Fable 5.2

GPT-6 Astra

Cyber capability rating

Not stated on the launch page

Critical (OpenAI Preparedness Framework)

High-risk request handling

Classifiers; refusals routed to Opus 4.8 (cyber) or Opus 5 (biology)

Refusal boundary, realtime misuse monitors, Trusted Access for Cyber

Unrestricted sibling

Mythos 5.2, verification programs only

GPT-6 Cyber, trusted access

Default data retention

30 days, required

Not stated on the pages read

Zero data retention

Only if Anthropic expressly authorizes; Enterprise Frontier Safeguards in private beta

Offered to eligible API customers on supported endpoints, subject to approval

Anti-distillation

Reported 90% to 99% reduction in data harvested by distillers

Not stated

........

Retention is the item most likely to settle a procurement decision: customers who need zero data retention on Fable 5.2 must wait for Enterprise Frontier Safeguards or obtain Anthropic's explicit authorization.

··········

WHAT ANTHROPIC AND OPENAI HAVE NOT PUBLISHED ABOUT THESE TWO MODELS.

Some evidence a buyer would want is missing on both sides. OpenAI has not published any benchmark that includes Fable 5.2, so every direct score in this report comes either from Anthropic or from Artificial Analysis. Anthropic does not state whether it measured each Astra figure in its table itself, and OpenAI's DeepSWE v1.1 figure for Astra carries no settings.

Several operating details are also absent. The Fable 5.2 model page does not list API rate limits, and the launch page does not state an AI Safety Level. The Astra model page does not name a default effort level or a retirement date. Artificial Analysis has priced only the low and max settings per task, so the cost of Fable 5.2 and Astra at the high and xhigh settings most teams use for coding is not yet measured. No independent software engineering score for Fable 5.2 appeared on the evaluation pages reviewed for this report.

··········

CHOOSING BETWEEN CLAUDE FABLE 5.2 AND GPT-6 ASTRA BY WORKLOAD.

The evidence points to a different model for different workloads, as the table below sets out.

........

Workload or constraint

Recommended model

Reason

Hardest multi-hour agentic coding, where success rate matters most

Claude Fable 5.2

Leads FrontierCode 2.0, FrontierSWE v2 and Terminal-Bench-Science on Anthropic's table; Index 65 vs 57

Cost-capped agents billed per completed task

GPT-6 Astra

$6.24 vs $26.54 per Index task at max effort; $1.42 vs $3.10 at low

No long-context surcharge; Astra bills 2x input and 1.5x output

Fixed-length, cache-heavy API traffic

Claude Fable 5.2

Lower list rates and $0.20 cache reads vs $1.00

Computer use and desktop agents

Claude Fable 5.2

OSWorld 2.1: 89.3% vs 79.5% on Anthropic's table

Zero data retention required

GPT-6 Astra

ZDR offered to approved customers; Fable 5.2 requires 30-day retention by default

$20 subscription

GPT-6 Astra

Included on ChatGPT Plus with 5-45 messages per 5 hours; Fable 5.2 on Claude Pro uses credits after November 16

Shortest wall-clock time per task

GPT-6 Astra

About half the time per Index task at max, xhigh and high effort

Fastest token streaming

Claude Fable 5.2

98 vs 38 tokens per second at max effort

........

Teams that already use Claude Code at Max or Ultra and work with large repositories get the most from Fable 5.2: they receive it inside their plan, pay no long-context premium and gain the highest Intelligence Index score Artificial Analysis has recorded. Teams that run open-ended agents at scale through the API, or that need zero data retention, should expect GPT-6 Astra to finish the same tasks for roughly a quarter to a half of Fable 5.2's measured per-task cost, with an eight-point lower Index score at max effort. For readers comparing the earlier generation, our GPT-6 Astra vs Claude Fable 5.1 comparison shows how far Fable 5.2 has moved: on Anthropic's launch table Fable 5.1 trailed Astra on FrontierSWE v2 by 9.2 points, and Fable 5.2 now leads it by 4.4.
