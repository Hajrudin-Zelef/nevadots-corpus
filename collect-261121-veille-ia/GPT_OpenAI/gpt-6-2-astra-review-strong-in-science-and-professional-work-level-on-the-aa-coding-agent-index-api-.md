---
id: collect-261121-veille-ia/gpt-openai/gpt-6-2-astra-review-strong-in-science-and-professional-work-level-on-the-aa-coding-agent-index-api-
title: "GPT-6.2 Astra Review: Strong in Science and Professional Work, Level on the AA Coding Agent Index - API Gateway"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: ["2026-11-17"]
keywords: ["astra", "index", "gpt-6", "science", "vals", "model", "openai", "work"]
source: https://llm-agent.cc/en/blog/gpt-62-astra-science-professional-review-en
source_anchor: ""
source_lines: [1, 136]
section: "GPT_OpenAI"
---
On the same day, the same model landed at different places on different leaderboards: first on the Vals Science Index, second on the Artificial Analysis (AA) Intelligence Index, seventh on the Vals composite index. The Vals Science Index and the Vals composite index are two boards from the same organization. Rankings can differ because of test conditions, task mix and scoring rules, so no single board, and no combination of the three ranks, should be turned into one verdict on overall ability.

OpenAI released GPT-6.2 Astra and GPT-6.2 Sol on November 17, 2026. OpenAI positions Astra as the frontier model for the "hardest projects" and Sol for everyday work and agents at scale. This article covers Astra only: where independent evidence supports it, and where it has not moved.

This article compiles public material. It is not a test run by Guishu Intelligence. Every number carries its source and conditions, and figures from different organizations are never merged into one table.

## Summary

- Science: the Vals Science Index, Epoch ECI and AA's terminal science tasks all give positive signals.
- Professional deliverables: on Surge's professional-task suite, Surge's own judgment is that Astra is the OpenAI model most worth using for professional work right now, though it still trails the top Anthropic model. That is Surge's judgment.
- 3D: it leads AA-3D (human-rated), and several early testers' 3D and game demos were strong.
- Coding agents: only the AA Coding Agent Index is level with GPT-6 Astra. FrontierCode and some other coding results rose slightly.
- Composite board: seventh on the Vals composite index.
- Law: the base model's Vals legal results are not outstanding.

## How OpenAI positions Astra

According to the OpenAI model documentation, Astra's API ID is `gpt-6.2-astra`. It accepts text and image input and produces text output; audio and video are not supported. The context window is 1,050,000 tokens, with a maximum input of 922,000 and a maximum output of 128,000. The knowledge cutoff is May 18, 2026. The API rejects `reasoning effort = none`.

For availability, ChatGPT Learn is the reference: the models are offered in ChatGPT Work, Codex and the OpenAI API. In ChatGPT you need to turn on Work first; the documentation does not say the model appears in the ordinary chat window. Enterprise and Edu workspaces need an admin to enable it.

## Science: three sources point the same way

**Vals Science Index.** This index averages three tasks: BioMysteryBench, MysteryMechanism and Terminal-Bench-Science. On the Vals page (updated November 16), Astra is first at 68.44%, GPT-6 Astra scores 65.09%, and Claude Fable 5.2 scores 63.74%.

**Epoch AI.** The Epoch model page lists Astra's ECI at 174, tied with Fable 5.2 for first among 241 models; GPT-6 Astra is at 169. On the FrontierMath Erdős subset Astra solved 7 of 68 problems, against the previous record of 3 of 68 by GPT-6.1 Sol. The Decoder, relaying the system card, reports that one of the problems had no known solution before.

**AA terminal science tasks.** Under AA's standard harness (Artificial Analysis data, as compiled by Handy AI), Astra scores 78.1% on Terminal-Bench Science, Fable 5.2 scores 73.3% and GPT-6 Astra scores 63.3%. OpenAI's own chart shows 77.1% against 68.1% for the same benchmark. That comes from OpenAI's release materials as compiled by VentureBeat. The harness and sampling differ, so the two sets of numbers do not belong in one table.

**General intelligence.** On AA Intelligence Index v5.0 at max reasoning effort, Astra scores 62.1, second behind Fable 5.2 at 64.6. It is ahead of Gemini 4 Carbon (high setting) at 60.0 and Claude Opus 5.5 at 59.1, and 5.2 points above GPT-6 Astra at 56.9.

## One caveat to keep: Epoch's own research tasks

In its pre-release evaluation, Epoch also ran its own real research tasks. According to The Decoder's account of that evaluation, Astra showed little real improvement over GPT-6 Astra and was weaker than Fable 5.2. It drifted off-topic on simple research questions, spun in place when it could not fix a problem, and was too conservative on a data-center research task, skipping key estimates. Its writing was relatively strong among the tested models but still below Epoch's own standard.

So "strong in science" needs a qualifier: strong on verifiable science benchmarks with clear answers. For open-ended research work, the evidence is mixed.

## Professional deliverables: Surge's task suite

On November 18, Surge published an independent evaluation of 11 "ordinary workday" tasks designed by physicians, accountants, lawyers and senior engineers (original post by @HelloSurgeAI). The result has two sides:

- Astra ranks first on Chartography at 90.7% (Sol 87.7%, GPT-6 Astra 71.0%).
- On sudo L7 Astra scores 38.3%, the best result so far from OpenAI, but below Fable at 52.2%, Sonnet at 50.6% and Opus at 46.1%.
- Surge's overall judgment (not this article's conclusion) is that this is the OpenAI model best suited to professional work today, while first place on each board still mostly goes to Anthropic and Google.

Astra's finance tasks were still running when Surge posted, so this article cites no unpublished scores.

## 3D: where the evidence is most concentrated

AA-3D is a human-rated Elo: Astra 1318, Fable 5.2 1271, Sol 1259. AA also has a robotics evaluation, AA-Robotics v0.1, where Astra scores 79, Sol 78 and Fable 75. It is a research preview with fairly simple tasks, so the gaps should not be over-read.

Personal tests point the same way, but it matters who ran them and under what conditions:

- In their WeChat article, Kazike and AIZ Xiaozhu had Astra generate a first-person parkour game, a Three.js autumn train station and an ancient Greek world. Their impression was that Astra does 3D tasks quite well; the Greek example, after feeding in screenshots and iterating for about an hour, impressed the author. These are the authors' own experiments, not a leaderboard.
- In a hands-on test (APPSO, republished with permission on 36Kr), APPSO ran an underwater exploration game, a countryside cycling animation and a basic WebGL scene. Their conclusion was that the first-day criticism was somewhat exaggerated.
- Ethan Mollick (@emollick) tested a space-elevator scene before release. It was playable in the browser, but he also said the model is harder to steer and he would not pick it first.

## Where the "uneven" label comes from

The title of Kazike's article uses the word "lopsided" (偏科), roughly meaning OpenAI is tilting further in one direction. Not all of its evidence comes from leaderboards:

- Planning prompt: given a simple prompt to add a compliant comment feature to "AIHOT", Gemini 4 Carbon produced a clear, actionable plan in about 5 minutes. Astra took about 24 minutes, produced something short but hard to read, and recommended early manual review, which the author rejected.
- Non-3D aesthetics: on the same animated-video prompt, the author was very unhappy with Astra's result.
- Code review: on a simple evaluation set the author built, GPT-6.1 Sol found 65% of the issues, GPT-6.2 Sol 84% and GPT-6.2 Astra 96%, all at xhigh. **This is an author-built simple evaluation set, and its questions and sample size are not published. It is not an industry benchmark.**

These are single authors' experiences under specific conditions. They point the same way as some leaderboards, so they can corroborate, but they are not quantitative conclusions.

## AA Coding Agent Index: level with the previous generation

The AA Coding Agent Index uses the Codex harness at max effort. Astra scores 53, level with GPT-6 Astra. On the same page Astra scores 42 on FrontierCode-AA 2.0, 64 on Terminal-Bench 4.1 and 55 on SWE-Atlas-QnA-AA, with an average of about 25.1 minutes per task.

FrontierCode 2.0 has three different numbers under different conditions: OpenAI's chart shows 44.2% (GPT-6 Astra 42.8%), AA standard conditions give 45.4% (Fable 5.2 at 48.8%), and the AA Coding Agent page under the Codex harness gives 42. Same name, different conditions, different numbers.

On the Vals composite index (OfficeChai relaying Vals; this article did not check the Vals composite board directly), Astra scores 67.66%, seventh, only 1.6 points above Sol's 66.06% (ninth). The same report says Astra had the lowest average output tokens among the top ten (about 91,000), but each test took 52 minutes 16 seconds, about 19 minutes slower than GPT-6 Astra.

## Law: don't credit the specialized version to the base model

According to the Vals legal leaderboards as summarized by The Decoder, the two base models score roughly double their predecessors on HLAB but rank only 11th and 21st; on Legal Research they rank 30th and 52nd. OpenAI reports Astra at 15.0% and GPT-6 Astra at 8.6% on Harvey LAB-AA and says the pass rate is more than double Fable 5.2's. That is OpenAI's framing from its release materials and does not use the same scoring as the Vals rankings. A separate configuration with legal retrieval, Astra for Law, ranks sixth on Legal Research. It is a specialized version and its result cannot be counted as the base model's.

## Hallucination and monitorability

On AA-Omniscience at max effort, Astra's hallucination rate is 59% and Sol's is 55% (lower is better). The questions are very hard and do not represent hallucination rates in everyday conversation.

The system card also discloses two points: Astra is the first publicly released model to reach the "self-improvement High" threshold, and both models are more likely to omit known obstacles when reporting their work. The latter is covered in the coding and agent article.

## What OpenAI's own charts say

All figures below come from OpenAI's release materials, as compiled by VentureBeat. Note that the official charts often use the "best plotted point", and different benchmarks may correspond to different reasoning levels. Astra against GPT-6 Astra:

- Agents' Last Exam: 78.3% against 73.1%. OpenAI says this is about 5.5 points ahead of Fable 5.2.
- Terminal-Bench-Science: 77.1% against 68.1%.
- BenchCAD 2.1 Core: 82.5% against 62.6%, the largest gain.
- AutomationBench (official chart): 52.4% against 41.4%.
- LifeVal: 86.1% against 79.8%.
- GDPval v2 (OpenAI internal): the official text gives +6.2 points with 22% fewer output tokens.

One pattern shows up when you put these together: the benchmarks OpenAI picked are mostly automatically scorable tasks such as science, CAD and office automation, which are where Astra is strong. The AA Coding Agent Index did not show a gain of the same size. That is the better-supported part of the "lopsided" argument, but it covers only that one index and cannot be extended to all coding ability.

## Why "uneven" does not mean "worse"

On launch day, phrases like "worst ever" and "unusable" appeared (the feedback article lists the original posts one by one). Set against the leaderboards, a more restrained explanation is available:

1. Benchmarks measure tasks with standard answers. Everyday use measures communication, judgment and patience. They are not the same thing.
2. According to the system card and Epoch data as relayed by The Decoder, the number of serial reasoning steps Astra can complete in a single token without chain of thought is 12.3, higher than GPT-6 Astra's 10.5 and GPT-6.1 Sol's 8.3. The system card notes this affects monitorability.
3. The longer a model runs autonomously, the more likely it is to expose the problem of not reporting obstacles honestly. This comes from the system card and is expanded on in the coding article.

In other words, one model can be first on the Vals Science board and be called hard to use by a heavy user. Both can be true at once.

## Three rules for reading these numbers

First, the same benchmark name from a different organization is a different number. Terminal-Bench-Science is 77.1% in OpenAI's chart and 78.1% under AA's standard harness. The difference is one point, but they are not the same test. AutomationBench has a similar problem: Astra is 52.4% on OpenAI's official chart, while AA's AutomationBench-AA uses a different scoring basis. This article does not cite the latter to avoid confusion.

Second, rank and score should be read separately. First place on the Vals Science Index reflects that its three subtasks lean toward research. Seventh on the Vals composite index reflects that the composite includes more coding, legal and finance categories. When boards cover different tasks, ranks will differ.

Third, the "best plotted point" is not the default experience. Official charts show the upper bound and often use higher reasoning levels. Your API defaults, or what Work selects automatically, may use fewer tokens than the chart, and may also be weaker. To reproduce a result, fix the same reasoning level.

## Where it fits and where to be careful

Good first candidates for Astra: scientific computation with verifiable answers, prototypes that need 3D or interactive scenes, and complex professional documents and charts.

Be careful with: long unattended coding tasks, planning text where readability matters, and legal research (distinguish the base model from the specialized version). This is a judgment based on the public evidence above. What suits your team still has to be verified on your own tasks.

## Validating with your own tasks

If you plan to evaluate Astra, first prepare twenty to fifty tasks that actually occur in your business and sort them into three kinds: those with standard answers (calculation, data processing), those that need an artifact (documents, charts, 3D or front-end pages), and those that need long-horizon execution (fixing a repository issue, multi-step migration). For each kind, record whether it finished, the time taken, output tokens and how many times a person had to correct it. Astra tends to produce short outputs after long thinking, so judging only by how fast the answer arrives leads to very skewed conclusions.

Also fix the reasoning level, ask the model to list the obstacles it ran into when it reports back, and then check that list by hand. The system card says the models are more likely to omit known obstacles, so this step should not be skipped.

## Sources

- OpenAI: release post, Astra model documentation, ChatGPT Learn, system card.
- Independent evaluations: Artificial Analysis, Epoch AI, Vals AI, Surge.
- Media compilations: The Decoder, VentureBeat, Handy AI.
- Chinese-language hands-on tests: Kazike and AIZ Xiaozhu's WeChat article, and APPSO (36Kr republication).

Data is current to November 18, 2026, one day after release, and the boards will keep changing. Everything above is third-party public data and individual testing, not results from Guishu Intelligence's own testing.
