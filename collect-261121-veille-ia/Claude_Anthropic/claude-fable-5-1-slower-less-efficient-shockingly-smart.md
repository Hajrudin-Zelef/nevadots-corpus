---
id: collect-261121-veille-ia/claude-anthropic/claude-fable-5-1-slower-less-efficient-shockingly-smart
title: "Claude Fable 5.1: Slower, Less Efficient, Shockingly Smart"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["fable", "claude", "model", "performance", "slower", "less", "anthropic", "effort"]
source: https://www.davidgreenwald.com/blog/fable-5-1-review/
source_anchor: ""
source_lines: [1, 83]
section: "Claude_Anthropic"
---
## TL;DR

- Slower and less token-efficient than Fable 5, with token use scaling up intensely by level
- Smarter in many categories and responsive to CLAUDE.md-style rules
- Still challenged by hallucinations, especially without rules to counter them
- Astounding real-world performance in bug fixing and investigation
- `medium` effort the likely sweet spot for performance and output

Fable 5.1 landed today, becoming the best large language model available. Its day on the throne may be short-lived, as OpenAIâs Astra comes closer to release, but for the moment, this genuinely seems like the best AI you can buy.

Hereâs my experience and evaluation so far.

## Real-world performance

With every new Anthropic model, I try to come up with an interesting challenge for it. In the past, Iâve had it run a performance audit on my day jobâs formidable, old-enough-to-drink database cluster. I have now done a few of those, and Claude has solved most of our problems along the way, so instead today, I pointed it at our observability platform, Datadog. I asked it to do a Pareto 80/20 analysis and categorize our most important user-facing problems across all services.

It came back with the most unbelievable report Iâve ever seen. Two dozen issues, clear investigation paths, a jaw-dropping ability to group similar issues together and surface patterns that have not occurred, at least to me, in years of working on these projects. I managed to get several PRs out this evening and I look forward to attempting to fix every serious bug on our platform over the next week. I am not exaggerating!

On an unreleased personal project, an Obsidian plugin Iâm almost ready to share, it leveraged the Obsidian plugin profiler I built recently (also due for release very soon!) and was able to improve performance on one process from 15 seconds to 3. I have a few more passes to go once my five-hour session limits reload, but I had the goal of making this extension just comically fast, and now it looks like Iâll be able to achieve that.

So: so far, A+.

I will say that the Anthropic 5-family models continue to have issues with detail despite their big brains. In all of these cases, I benefited from using the ever-trustworthy Opus 4.8 to code review, fact-check, and drive implementation after Fable 5.1 investigates and runs the proof-of-concept work.

The cost of Fable 5.1 (though improved significantly from 5!) makes it prohibitive to run as your always-on daily driver, but model capabilities are becoming distinct enough that if you arenât doing some degree of model routing in your pipelines and day-to-day prompting, you will have gaps in both your output and your wallet.

## The Greenwald Evals

The Greenwald Evals are a private and growing set of tests based on my real tasks and code, mostly at a toy size and covering writing code, review, web research, fact-checking, adversarial review, and a few other topics. They test model traits such as groundednessâwill the model change its mind under pressure?âand crying wolf, inventing a problem because a prompt asked it to instead of assessing a codebase correctly as clean.

It is, again, prohibitively expensive to run Fable 5.1, though Anthropic has improved the cost of cache hits meaningfully, and they claim that API usage costs should drop as much as 45% from Fable 5. Thatâs a step in the right direction given the summer price wars that Anthropic has largely sat out of. But at any rate, I have not run what would usually be 3-5 runs of each of these tests, stopping at one each, so unfortunately I canât offer total statistical confidence here.

I ran a total of 91 testsâeach eval gets several âarmsâ with different instruction sets: Prompt-only, daily driver CLAUDE.md, specialized skill, CLAUDE.md plus skill. A key part of the methodology here is to test my harness and not just the model.

Some highlights:

- Fable 5.1 really scales up its token use with effort. At `high` effort, it burned 40% more than Fable 5, and used more tokens-per-task than Fable 5 across the board.
- Itâs also slowerâ31% average across `low` ,`medium` , and`high` , though mainly at`high` . It was*faster* at`medium` , which may be the sweet spot here.
- With no CLAUDE.md or extra instructions, it regresses at `low` effort against Fable 5; with my base CLAUDE.md, it beats it squarely. Both Fable versions had a rough time at`medium` effort with the CLAUDE.md, and became more successful when a skill was added.
- Letâs end on a positive note: despite the `low` effort regressions, it won on overall arm passage, 77 to Fable 5âs 73.

Where it struggled:

- Over-claiming: It did not verify an issue in a code review case that no effort level passed, a real regression from Fable 5
- Objectivity: In one case, it hallucinated a command and its output!
- Skepticism: Another review case was approved with caveats instead of providing an expected warning

My CLAUDE.md tinkering was born out of trying very hard to get Claude not to hallucinate, so it was good to see those instructions make a real difference, particularly in `low` effort performance. Despite Anthropicâs suggestion with these models that itâs time to throw out our old prompts, thatâsâ¦ why Iâm doing the evals.

You can also see the need to keep Opus 4.8 around here to backstop these issues, or GLM-5.3-Air, which swept my evals during its testing as Ox Alpha.

My CLAUDE.md instructions in these evals, for the record:

Calibrated uncertainty. Say what you know, flag what you donât, abstain rather than guess. A claim not grounded in a tool result or primary source this turn is marked unverified. Missing context is a reason to ask or abstain â not to fill the gap with a plausible answer.


Evidence requirements â every finding must include: File path and line number â read from source, never from recall. After emitting a file:line citation, re-open that exact range and confirm the claimed code is on those linesâ¦ run the measurement tool and include its exact invocation and output inlineâ¦ Counts without inline tool output stay in drafts.


Hold findings on evidence. A challenge is not counter-evidence: re-verifyâ¦ and if the finding holds, restate it with the evidence rather than softening or retracting. Retract only when new evidence overturns it.


Surface findings that affect production trust. A finding qualifies if you can name a specific consequenceâ¦ When unsure, state the finding as a question, not a recommendation.


Also on the positive side, it beat Fable 5 in groundedness, web research, and fact-checkingâall important categories to me.

Fable 5 at low effort remains a real secret weaponâitâs as fast as Sonnet, is equally token-efficient (though at a higher cost per token), and capable of great feats of code review and problem-solvingâso keep that in mind if you need frontier intelligence and donât have a highly calibrated CLAUDE.md.

The Fable 5.1 prompting guide sets `high` effort as the default, and notes:

At `medium`, results roughly match Claude Fable 5 at lower cost, so step down to `medium` or `low` where your evals show quality holds.


Well, my evals show it does, as long as it gets the right instructions. See, harness engineering lives!

The guide also notes that at `low` effort itâll forget to search and will use training data insteadâyour mileage may vary, but I make any model search at the start of almost anything I ask it to do.

## Closing thoughts

This is an incredible model that has also maxed out all my 5-hour subscription windows across my personal and work accounts today. It is brilliant, a little flawed, and tough to use at its full power due to the token burn.

Set it to `medium`, throw it at your observability platform, and see what it can do.
