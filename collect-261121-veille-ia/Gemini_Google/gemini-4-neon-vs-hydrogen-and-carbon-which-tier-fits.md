---
id: collect-261121-veille-ia/gemini-google/gemini-4-neon-vs-hydrogen-and-carbon-which-tier-fits
title: "Gemini 4 Neon vs Hydrogen and Carbon: Which Tier Fits"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["carbon", "neon", "hydrogen", "november", "gemini", "argon", "tokens", "december"]
source: https://www.digitalapplied.com/blog/gemini-4-neon-vs-hydrogen-vs-carbon
source_anchor: ""
source_lines: [1, 68]
section: "Gemini_Google"
---
A developer pricing a million output tokens on November 19 would pay $10 for Gemini 4 Carbon and $15 for Gemini 4 Neon. A developer pricing the same work for December 1 gets $20 for Carbon and $15 for Neon. Google positions Neon between Hydrogen and Carbon on capability. The price order only starts to resemble that description in December. Gemini Developer API rates

Keep Hydrogen as a starting point for frequent requests with straightforward checks. Try Neon where Hydrogen repeatedly needs help. Give Carbon a hard problem worth its reasoning time, even if the November rate happens to make it the cheaper experiment. An engineering lead choosing a budget needs two dates and a measure of work completed. Someone choosing a model in the Antigravity editor also needs to know which model is actually doing the work.

1. 01November and December have different price orders.Carbon and Argon are $2 input and $10 output per million through November 30. From December 1 they rise to $4 and $20. Neon’s published rates are $3 and $15.
2. 02A default thinking level is part of the decision.Hydrogen and Neon default to high, while Carbon and Argon default to medium. A change of model can change reasoning spend without any change to your setting.
3. 03Neon earns its premium by reducing work.A cheaper first attempt becomes expensive if another model must reread the same inputs. Compare spend and review time per accepted result.
4. 04Antigravity offers a different choice.Orchestrator (Neon) uses Neon for planning and review, with Hydrogen implementing. The option has plan and configuration rules of its own.

## 01 — Release historyOctober’s launch and *November’s prices*

Argon reached the general paid Gemini API on October 13. Google announced Carbon on October 28, starting with a restricted group and its Fairwind programme. Public paid API access for Carbon followed on November 10. An October description of access to Carbon can therefore be accurate for its date and misleading for a developer choosing a model now. Google’s October Carbon announcement · Gemini API release notes

Hydrogen launched on November 12 with a free API tier and paid rates of $1.50 input and $7.50 output per million tokens. Neon joined the paid tier on November 18 at $3 and $15. Those releases gave developers two additional choices while Carbon and Argon were still at their introductory prices. On November 19, Google’s standard price table lists a December change for Carbon and Argon. It lists single rates for Hydrogen and Neon.

An API buyer extending a November pilot into a December contract should treat those as separate periods. Keep the dates attached to any public price comparison, including internal notes from the October Argon release. A copied cost estimate with the date removed can make a successful pilot look suddenly wasteful when the invoice reflects the scheduled rate change. The earlier Argon coverage records how the public access schedule evolved.

## 02 — API ratesThe same tokens at *two prices*

The table gives standard paid Gemini API token rates in US dollars per million, as published on November 19. Each cell lists ordinary input, implicitly cached input and output in that order. Thinking tokens count as output. The free tier for Hydrogen has eligibility and usage limits of its own. The other three models in this comparison are paid-only on the direct API. Google’s standard rate card

An introductory Carbon token and an Argon token have the same list price in each category. That gives an existing Argon user a reason to compare answers before December without introducing a different per-token rate. It gives a finance team no basis for declaring the models interchangeable. Each may consume a different number of thinking tokens, take a different number of turns or ask for different tool calls before a human accepts the result.

Cache pricing applies to tokens that actually receive an implicit cache hit. When estimating from a trace, take the cached tokens out of the ordinary input count before applying the two rates. Keep grounding, external tools, separate processing tiers and any human review outside the table. If one route browses and another uses only a supplied source, the work and bill have changed along with the model.

## 03 — Worked exampleWhat a December *invoice* could show

Suppose a review uses 80,000 ordinary input tokens, another 40,000 cached input tokens and 10,000 billed output tokens across its calls. At these assumed volumes, Hydrogen costs $0.198 and Neon costs $0.396. Carbon or Argon costs $0.264 through November 30. The December rate takes the same Carbon or Argon review to $0.528. These are calculations using Google’s prices, with identical consumption and no separate tool charges.

Here is the December Carbon calculation. The ordinary input costs $0.32, cached input $0.008 and output $0.20. For 10,000 such reviews the model-token bill would be $5,280, against $3,960 for Neon or $1,980 for Hydrogen. A team that committed to November Carbon consumption would have budgeted $2,640. The spend changes before a developer has altered a single prompt.

The service decision can still favour Carbon. A 13.2-cent difference between the December Carbon and Neon jobs is modest if Carbon saves a reviewer several minutes or avoids sending an incorrect result downstream. It is substantial across many thousands of automatically accepted results. Price the task whose outcome the reviewer signs off, including the expensive exceptions that required another attempt.

## 04 — Thinking levelsCompare the *setting* as well as the name

Hydrogen and Neon offer low, medium, high and extra high, with high as the default. Carbon and Argon default to medium and top out at high. An integration that always sends extra high to every model can therefore request an unsupported setting after a fallback. One that leaves the setting implicit will change effort when it changes models. Preserve the actual effort in the trace used to cost the result. Google’s thinking-level and migration guidance

The figures below are Artificial Analysis Intelligence Index v5.0.1 scores from all four model pages retrieved on November 19. The default and high columns answer different questions. A buyer can compare the configurations an unmodified application would request, or hold the name of the effort level constant. The same word still allows each model to do different amounts of work.

Neon and Carbon both reach 60 at high on this index. Combining nine evaluations into one rounded score can conceal quite different strengths. A team comparing them for a codebase migration should include a case where the correct change depends on an old configuration file, not only a case where the fix appears in the error message. Score the patch and the explanation against that case before treating the models as substitutes. Source pages: Hydrogen · Neon · Argon · Carbon

## 05 — Workload fitWhere *Neon* earns a turn

Google’s developer guide recommends Hydrogen for everyday coding, interactive applications and documents, Neon for complex work that occurs frequently, and Carbon for the hardest problems. An actionable routing rule needs a signal the application can detect. A failing acceptance test is better than guessing from prompt length. A long table with one simple extraction rule may belong on Hydrogen, while a short bug report describing a rare data loss may justify Carbon immediately. Google’s model-selection guide

Take the hypothetical review priced above. If Hydrogen costs $0.198 per attempt and a full retry on Neon costs another $0.396, then always trying Hydrogen first costs less than sending every job straight to Neon only while fewer than half of the jobs need that full retry. At a 40% retry rate the average is $0.3564. At 50% it is $0.396. This assumes equal acceptance after escalation and ignores review time. A customer waiting for a response pays the extra latency on every failed first attempt.

If the evidence shows that one class of job escalates almost every time, send that class directly to Neon. For the rare cases where the constraint itself is unclear, bring Carbon in early enough to examine the evidence before two cheaper attempts have filled the history. Keep a working Argon integration in the comparison if it already has accepted outputs for those cases. It faces the same December price change as Carbon, but can have a different record on your data.

## 06 — In the IDEWhat *Orchestrator* actually runs

Orchestrator (Neon) in Antigravity is a paired mode in the app, CLI and eligible editor experience. Neon makes the plan and reviews progress. Hydrogen handles code changes, package updates and tests, handing control back when implementation reveals a problem. A run can visit both models more than once. Standalone Neon keeps all those steps on one model. Orchestrator has a fixed configuration, so choose a standalone model if you need to set a particular thinking level. Antigravity’s model and Orchestrator guide

Google’s Antigravity team reports about 32% lower cost per task for standalone Neon at its default high setting versus Carbon, in the context of Antigravity quota. That is Google’s product-specific comparison. It gives neither a measured saving for Orchestrator (Neon) nor a 32% discount on the Gemini Developer API rate card. The November rates make the distinction especially visible: one Neon output token still costs more than one Carbon output token before December. Antigravity’s rollout and quota comparison

On paid Google AI Pro and Ultra, the announcement includes standalone Neon and Orchestrator (Neon). A Pro trial qualifies only with an eligible premium device. Enterprise access through Gemini for business covers standalone Neon and excludes Orchestrator (Neon). Free and Plus lack both choices at launch. The developer API’s free Hydrogen tier answers a different access question. An API key does not by itself entitle a user to the paired Antigravity experience.

## 07 — DecisionGive the paired mode a *fair trial*

Use a task with a clear planning step followed by several routine edits to see whether splitting the work helps. For example, ask the planner to choose how to preserve a file format while changing a parser, then let the implementer update the parser and fixtures. The reviewer should check the legacy fixture, a malformed input and a round trip through the writer. If the work is one tiny edit, planning and handoff can consume effort that a standalone Hydrogen run might avoid.

Compare the result against standalone Neon and, for a genuinely hard case, Carbon. Keep the same starting commit, acceptance tests and tool access. Record elapsed time, whether the reviewer requested changes and either the app quota used or the API token categories billed. Record which of those two you measured. The planner and worker model guide explains why the number of handoffs can outweigh a cheaper implementation step.

For the detailed Neon limits, see the Neon launch guide. The comparison before Neon’s release provides the earlier baseline. An AI integration evaluation can use one set of accepted tasks and reprice it under both periods before choosing December’s default.

### Reprice the route before renewing it

A November Carbon trial can be the cheapest way to answer a difficult question. A December production queue may belong on Hydrogen or Neon. Keep the usage trace, reprice it for the actual month, then select the route that passes the team’s acceptance checks.
