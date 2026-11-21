---
id: collect-261121-veille-ia/claude-anthropic/what-anthropic-and-artificial-analysis-say-about-sonnet-5-5
title: "What Anthropic and Artificial Analysis Say About Sonnet 5.5"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-09-28"]
keywords: ["sonnet", "anthropic", "opus", "about", "artificial", "analysis", "says", "frontiercode"]
source: https://dev.to/technomadcode/what-anthropic-and-artificial-analysis-say-about-sonnet-55-254p
source_anchor: ""
source_lines: [1, 113]
section: "Claude_Anthropic"
---
Sonnet 5.5 came out on September 28. In Anthropic's own table it sits a few points behind Opus 5.5 on almost every test. The more interesting parts are a footnote under that table and what Artificial Analysis found when it ran the model at max effort.

Over the last week I wrote about what Anthropic's docs say improved in Opus 5.5 and then about the first independent benchmarks for it. Same routine here: what Anthropic says, then what an outside tester found, then what the prompting guide says to do about it.

Quick disclaimer: I didn't run any of these tests. The numbers come from Anthropic and Artificial Analysis, and I link each one to where I found it. I drew the charts myself from their numbers. The opinions are mine.

Anthropic's thread on X and launch post say Sonnet 5.5 is 30%+ faster than Sonnet 5 and costs up to 30% less per task. They call it a faster, lower-cost complement to Opus 5.5, strongest at well-scoped everyday work, bug fixes, and documents, slides and spreadsheets.

It also says outright that benchmark scores only show one side of a model, and that in its own testing and its outside testers' testing, Opus 5.5 is still clearly stronger at complex, open-ended work that needs sustained judgment. So the table below isn't “Sonnet equals Opus.”

The price per token didn't change from Sonnet 5: $2 per million input tokens and $10 per million output tokens. Opus 5.5 is $4 and $20. So the “up to 30% less” isn't a price cut. Anthropic says it comes from Sonnet 5.5 needing far fewer tokens to do the same work.

Here is the benchmark table from the launch post, retyped. It's all Anthropic's reporting, even though two rows are Artificial Analysis's results and one column is a competitor.

| Test | Sonnet 5.5 | Sonnet 5 | Opus 5.5 | GPT-6 Sol | 
|---|---|---|---|---|
| Terminal-Bench 4.0 (agentic coding) | **70.6%** | 10.3% | 66.4% | not reported | 
| FrontierCode 1.1 (agentic coding, main set) | **52.1% (xhigh), 46.2% (max)** | 42.4% | 54.4% | 49.3% | 
| CursorBench 4.0 (agentic coding) | **55.5%** | 34.1% | 57.8% | not reported | 
| GDPval-AA v2.1 (knowledge work, Elo) | **1844** | 1449 | 1846 | 1487 | 
| AA-Briefcase v1.1 (knowledge work, Elo) | **1811** | 1359 | 1822 | 1483 | 
| Humanity's Last Exam (reasoning, with tools) | **64.5%** | 54.9% | 67.7% | not reported | 
| OSWorld 2.1 (computer use, partial) | **80.1%** | 57.0% | 81.8% | not reported | 
| Chartography (chart reading, no tools) | **61.6%** | 15.6% | 64.4% | 53.6% | 

*Anthropic's numbers, retyped from its launch post. Opus 5.5's Terminal-Bench figure is at xhigh effort, its best. Sonnet 5.5's FrontierCode is shown at xhigh and max. Source: Anthropic.*

Against Sonnet 5, every row is up, and some are up a lot. Terminal-Bench 4.0 goes from 10.3% to 70.6%. Chartography, a chart-reading test, goes from 15.6% to 61.6%. Anthropic also says it's the first Sonnet to beat Pokémon Red from screenshots alone.

Against Opus 5.5, Sonnet 5.5 is ahead on Terminal-Bench 4.0 and behind on the other seven, mostly by two or three points. Against GPT-6 Sol it's ahead on the three rows where the comparison is simple. FrontierCode is the odd one, and it's the row I keep coming back to.

Anthropic lists Sonnet 5.5 twice on FrontierCode: 52.1% at xhigh effort and 46.2% at max. More effort, lower score. Footnote 2 says why.

FrontierCode checks whether a code change could be merged without human edits. It penalizes changes outside the task's scope, even good ones. According to Anthropic, at max effort Sonnet 5.5 more often ran Claude Code's code-review skill, which splits the review across many subagents. In two cases Cognition looked at, that led to a timeout or to extra edits beyond the task.

*FrontierCode 1.1 (main set). Higher is better. Anthropic's numbers, from its launch post; chart by me.*

If you read my post on the Opus 5.5 docs, you can see why this jumped out. Extra review passes and armies of subagents were two of the big complaints about Opus 5, and here they are again, this time costing points on a benchmark.

The prompting guide describes the same behavior. At xhigh and max, it says, the model can start its own rounds of review and verification once a task is done, sometimes with subagents, and make related fixes it noticed along the way. The fix Anthropic suggests is a short instruction in the system prompt: when the work and its checks are done, stop and report, don't start extra review rounds, and don't launch reviewer subagents unless the user asked for a review. In Anthropic's coding tests at max effort, that cut session cost by about a third with no change in quality. That's their test, not mine, and they say it makes the behavior less frequent, not gone.

My takeaway: on this model, max isn't a free upgrade. If a task has a clear scope, more effort can mean more work you didn't ask for.

Artificial Analysis ran Sonnet 5.5 at max effort on its Intelligence Index, which combines ten tests. It scored 56. Opus 5.5 got 58, and GPT-6 Astra and Fable 5.1 got 53 each, the same numbers as in my last post. Sonnet 5 scored 38, according to OfficeChai's write-up of the results.

One of the ten tests is Terminal-Bench 4.0, so that's where I can put Anthropic's number next to an outside one. Per OfficeChai, Artificial Analysis has Sonnet 5.5 at 63.6% against 59.6% for Opus 5.5 and 59.1% for Astra. The numbers are lower than Anthropic's, but they point the same way.

The catch is tokens. Per OfficeChai, Artificial Analysis says Sonnet 5.5 used more output tokens per task than any model it has measured. The model's page has the total: 410 million output tokens to run the whole index, against a median of 88 million. OfficeChai puts it at about 193,000 tokens per task, roughly 60% more than Opus 5.5 or Sonnet 5 at their own max settings.

Tokens are what you pay for, so the cost per task looks like this:

*Average cost per Intelligence Index task, all at max effort. Lower is better. Data: Artificial Analysis; chart by me.*

Sonnet 5.5 costs half as much per token as Opus 5.5, but at max effort a task came out at $7.60 against $5.98 for Opus 5.5. OfficeChai adds that it's about 50% more than Sonnet 5 costs at max.

That doesn't have to contradict Anthropic's claim of up to 30% less per task, which is measured against Sonnet 5 in Anthropic's own testing. The guide says effort levels were recalibrated on 5.5, so a level doesn't give the same amount of thinking it gave on Sonnet 5. Max on 5.5 may simply be more thinking than max on 5. Anthropic's post also says Sonnet 5.5 works best next to Opus 5.5 at lower effort, where it costs less per task, and that at higher settings it can perform comparably at a similar cost.

Two more things about these numbers. First, Artificial Analysis ran a pre-release build with a bug that could hurt responses that use structured outputs. Anthropic says it's fixed and expects the effect on the scores, if any, to be small and to understate the model. Per OfficeChai, Artificial Analysis plans to re-run the affected tests, so treat these as first results. Second, its page showed no speed number for Sonnet 5.5 when I looked, so nothing outside Anthropic has checked the 30%+ faster claim yet.

Anthropic's post also quotes a lot of customers, and several talk about tokens. Slack says about 14% fewer output tokens on its Slackbot tests, with no prompt changes. Balyasny Asset Management reports about 121,000 tokens per answer on its finance tasks, against 497,000 for Sonnet 5. Lovable says a third fewer tool calls. Anthropic picked those quotes for its own launch post, so I read them as things that can happen, not things that will happen to you.

This is where the effort setting matters. Anthropic's charts say that at low or medium effort Sonnet 5.5 beats Sonnet 5's best score on several benchmarks for about a tenth of the cost per task. The default in Claude Code and the Claude apps is medium, and the API defaults to high. Per OfficeChai, Artificial Analysis found high effort was Sonnet 5.5's most cost-competitive setting. So the max-effort bill above probably isn't the one you'll get.

The Sonnet 5.5 prompting guide opens by saying existing Sonnet 5 prompts should keep working. Then it lists what behaves differently. These are the parts I'd pay attention to:

- 
**Redo your effort test.** A level doesn't give the same amount of thinking it gave on Sonnet 5. Anthropic says to start at high (the API default) unless the work is agentic or latency-sensitive. For agentic coding, medium for well-specified tasks and high for harder ones. For chat, medium or low. Keep xhigh and max for work where you've measured a better result. In Claude Code,`/effort` changes it (docs).
- 
**Don't ask it to think less.** The guide says telling it to think less in the system prompt doesn't reliably work. Lower the effort instead. From medium up it thinks briefly before nearly every reply, even a greeting, and at low it skips thinking on most simple requests. Same lesson as with Opus 5.5: a line in your rules file isn't a setting.
- 
**Low effort has habits.** At low it can report a change as done without running a real check. At low and medium, on long tasks, it's more likely to stop and check in before finishing. Anthropic gives a short prompt for each, and says the keep-going one makes sessions longer and pricier.
- 
**It adds things you didn't ask for.** Tests, docs and small supporting files that match your repo, at every effort level and more at higher ones. Anthropic thinks most teams will like that. If you don't, one short instruction tells it to mention them at the end instead.
- 
**Open-ended asks get built.** Something like “show me what you can do with this” can turn into a slide deck. If you want ideas first, say so.
- 
**It may not search when it should.** When it has a search tool, the guide says it sometimes answers from what it learned in training where a search would catch something that changed, like what's allowed, required or charged. The fix is to tell it to search for those, even when it feels sure.
- 
**Switching models drops its thinking.** Sonnet 5.5 can't read thinking from Opus 5, Opus 5.5 or the Fable and Mythos models, and no other model can read Sonnet 5.5's. Move a conversation between them and the turns after the switch run without that earlier reasoning. The request still works, and the dropped blocks aren't billed. It's the same for accounts: Sonnet 5.5's thinking only works in the account that produced it, and Anthropic specifically mentions switching accounts in the middle of a Claude Code session.

If you use the API, three things to check. Sending `thinking: {"type": "disabled"}` now returns a 400 error, and you use `between_tools` instead, at high effort or below. Forced tool use, meaning `tool_choice` set to `any` or to a named tool, also returns a 400. And prompts that ask the model to put its reasoning in the reply can trigger a `reasoning_extraction` refusal. The what's new page has the full list.

In my Opus 5.5 post I explained the CLAUDE.md I use. I went through the Sonnet 5.5 guide with that file open, to see if Sonnet needs its own version.

Mostly it doesn't. The main problems the guide describes are already covered:

- My scope rule (“make routine choices yourself; ask when missing information materially changes the result or blocks safe progress”) covers the check-in habit.
- “Create review subagents only when the user requests them” covers the xhigh and max behavior from the footnote.
- My rule to look up current docs before editing, even when the model thinks it knows the API, is close to the guide's advice about searching for things that may have changed.

Three things I'd add if Sonnet 5.5 were my main model:

1. Don't add tests, docs or files that weren't asked for. Mention them at the end.
2. Before saying the work is done, run a real check on the change. If none can run, say which one didn't.
3. When I ask for ideas or a plan, give that and stop.

One thing I wouldn't add is a line telling it to think less, since the guide says that doesn't work. Effort is the setting for that. This is my reading of the docs, not something I've tested.

A few things would make me more confident about the numbers. Artificial Analysis re-running its tests after the bug fix. Someone measuring speed, since the 30%+ claim has no outside check yet. And a secure-code test like the Endor Labs one from my last post, where Opus 5.5 was quick and cheap but third on secure code. I couldn't find a Sonnet 5.5 result from them yet.

Until then, the most useful test is a few of your own tasks at medium effort, where you know what a good result looks like. If it finishes them with less fuss than what you use now, that tells you more than any table here.

- Introducing Claude Sonnet 5.5, Anthropic, September 28, 2026. Source of the benchmark table and its footnotes, the pricing, the effort defaults and the customer quotes.
- Anthropic's launch thread on X, September 28, 2026. I could only read its first two posts without an account, so the launch post above is where I got the rest.
- Prompting Claude Sonnet 5.5 and What's new in Claude Sonnet 5.5, Anthropic's docs. Source of everything I attribute to “the guide.”
- Claude Sonnet 5.5, Artificial Analysis. Source of the Intelligence Index scores, the cost per task and the 410 million output tokens, as they stood on the day I wrote this.
- Claude Sonnet 5.5 Scores 56 On Artificial Analysis Intelligence Index, 3 Points Ahead Of GPT-6 Astra, OfficeChai. Source of Sonnet 5's score of 38, the per-task token count, the Terminal-Bench numbers from Artificial Analysis and its plan to re-run the tests.
- 
Model configuration, Claude Code docs. Source of the `/effort` command.

*Originally published on promptquick.ai.*

## Top comments (0)
