---
id: collect-261121-veille-ia/claude-anthropic/sonnet-5-5-vs-opus-5-5-real-cost-per-task-and-which-to-use
title: "Sonnet 5.5 vs Opus 5.5: Real Cost per Task and Which to Use"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["sonnet", "opus", "claude", "model", "anthropic", "task", "work", "costs"]
source: https://www.lindy.ai/blog/sonnet-5-5-vs-opus-5-5
source_anchor: ""
source_lines: [1, 272]
section: "Claude_Anthropic"
---
Claude Sonnet 5.5 costs half as much as Opus 5.5 per token. So why did Sonnet's average task cost $5.46 at Max effort in Artificial Analysis's testing, when Opus 5.5 matched its score one setting lower for $3.46?

That gap is the Sonnet 5.5 vs Opus 5.5 decision in a nutshell. On well-scoped work, the cheaper model stays cheaper. Push it to Max, though, and it writes so much more that the per-token savings disappear. (Half the price per token, about 58% more to reach the same score.)

I went through Anthropic's launch tables and pricing docs, Artificial Analysis's runs at all five effort levels, and two sets of hands-on tests. Here's which model to default to, where the cheaper one stops being cheaper, and the jobs where Opus 5.5 still earns its price.

## Sonnet 5.5 vs Opus 5.5: the short answer

**Use Sonnet 5.5 for well-scoped everyday work, and Opus 5.5 for vague, open-ended, or high-stakes work.** Sonnet costs $2 per million input tokens and $10 per million output tokens. Opus costs exactly double, at $4 and $20.

That split comes straight from Anthropic. Its Sonnet 5.5 launch post calls Sonnet strongest at well-scoped everyday tasks, fixing bugs, and building documents, slides, and spreadsheets. The same post says Opus 5.5 "remains clearly stronger at complex, open-ended work requiring sustained judgment."

**Choose Sonnet 5.5 if you:**

- Know what "done" looks like before you hit enter
- Want fast answers (under a second to the first token at Low and Medium effort)
- Run lots of bug fixes, docs, spreadsheets, or slide decks
- Use Claude's Free plan, where Opus isn't included

**Choose Opus 5.5 if you:**

- Hand Claude a fuzzy goal and want it to fill in the blanks
- Run complex code reviews, plan architecture, or run long agent sessions
- Need the top score on a task and would otherwise run Sonnet at Max effort
- Want fast mode, which only the Opus models offer

## What is Claude Sonnet 5.5?

**Claude Sonnet 5.5 is Anthropic's mid-tier model, released September 28, 2026,** as the second model in the Claude 5.5 family. Anthropic's models overview describes it as "the best combination of speed and intelligence," with the same 1M-token context window and 128K-token output cap as Opus 5.5.

It's also the model most people will meet first. Claude's Free plan includes Sonnet but not Opus, per the Claude plans page, and Simon Willison reports that Sonnet 5.5 is now the model behind the free tier on claude.ai.

The smallest sibling has landed too. Anthropic launched Claude Haiku 5.5 for high-volume, latency-sensitive work on October 7, according to its API release notes.

Opus 5.5 arrived six days before Sonnet, on September 22, and Anthropic pitches it at long-running agentic coding and knowledge work.

Its docs go further: if you're unsure which model to use, they say to start with Opus 5.5 for most workloads. (That's the safe default. The rest of this article is about when you can save money by not taking it.)

For the full rundown on the bigger model, see our guide to Claude Opus 5.5.

### Is Sonnet 5.5 cheaper than Sonnet 5?

**Per token, Sonnet 5.5 costs the same as Sonnet 5, except for cache reads.** Both charge $2 and $10 per million input and output tokens. Anthropic's notes on what's new in Sonnet 5.5 say cache reads now cost $0.10 per million tokens, half the Sonnet 5 rate.

Per task, it's cheaper. Anthropic says Sonnet 5.5 "typically needs far fewer tokens to do the same work" and, in its own testing, costs up to 30% less per task than Sonnet 5. It also writes output 30%+ faster.

## Specs and pricing side by side

The two models share a context window, an output cap, and a knowledge cutoff. **Sonnet 5.5 costs exactly half of Opus 5.5 on every price line they share,** and the rest of the differences come down to speed, defaults, and where you can use them.

| 📋 Spec | ⚡ Sonnet 5.5 | 🧠 Opus 5.5 | 
|---|---|---|
| Release date | Sept. 28, 2026 | Sept. 22, 2026 | 
| Input / output (per 1M tokens) | $2 / $10 | $4 / $20 | 
| Cache reads (per 1M) | $0.10 | $0.20 | 
| Cache writes, 5 min / 1 hour | $2.50 / $4 | $5 / $8 | 
| Batch input / output | $1 / $5 | $2 / $10 | 
| Fast mode | Not offered | $8 / $40 | 
| Context / max output | 1M / 128K | 1M / 128K | 
| Knowledge cutoff | June 2026 | June 2026 | 
| Anthropic's latency label | Fast | Moderate | 
| Default effort, API | High | Medium | 
| Default effort, Claude Code | Medium | Medium | 
| Thinking | Adaptive, can drop to thinking between tool calls only | Adaptive, always on | 
| Claude Free plan | Included | Not included | 
| Claude Code default | No (pick it with /model) | Yes | 
| Earliest retirement | Sept. 28, 2027 | Sept. 22, 2027 | 

Prices come from Anthropic's pricing docs, as of October 2026. Specs come from the models overview, Anthropic's effort docs, the Claude Code docs, and the Claude plans page.

**The cache-read price changed after launch.** Anthropic cut Sonnet 5.5 cache reads from $0.20 to $0.10 per million tokens on October 7, and left every other price alone. That helps most in agent and coding work, where the same context gets read back over and over.

For the rest of Claude's plans and API rates, see our full Claude pricing breakdown.

## What Sonnet 5.5 and Opus 5.5 cost per task

Your real bill is the token price times how many tokens each model burns to finish the job, and at Max effort, Sonnet 5.5 burns a lot more.

Artificial Analysis runs both models through the same 10 evaluations at all five effort levels and reports a score on its Intelligence Index plus the average cost per task. **Here's where things stood in mid-October:**

| ⚙️ Effort | ⚡ Sonnet 5.5 score | ⚡ Sonnet cost per task | 🧠 Opus 5.5 score | 🧠 Opus cost per task | 
|---|---|---|---|---|
| Low | 36 | $0.35 | 42 | $0.55 | 
| Medium | 41 | $0.48 | 51 | $1.34 | 
| High | 47 | $0.88 | 54 | $1.82 | 
| Extra high | 52 | $2.01 | 56 | $3.46 | 
| Max | 56 | $5.46 | 58 | $5.98 | 

**At any matched effort level, Sonnet 5.5 is cheaper per task.** At Medium, it costs about 36% of what Opus does.

Now read the table diagonally. **Opus 5.5 at Extra high scores 56 for $3.46, the same score Sonnet only reaches at Max, for $5.46.** That's about 37% less to hit the same mark.

The same pattern shows up lower down. Opus at Medium (51, $1.34) lands within a point of Sonnet at Extra high (52, $2.01) and costs a third less.

So if you're asking whether to run Opus on a low setting or Sonnet on a high one, the data leans Opus. Sonnet wins when the task is easy enough that its lower settings do the job. Anthropic's charts show the same pattern one step up the lineup: Fable 5.2 at High roughly matches Opus 5.5 at Max on FrontierCode for about half the cost per task, as our Fable 5.2 vs Opus 5.5 comparison explains.

### Why Sonnet gets pricey at the top

**Sonnet 5.5 writes a lot more at high effort.** At Max, Artificial Analysis counted about 197,000 output tokens per task for Sonnet against 119,000 for Opus (many of its tests are multi-step agent tasks, so one task spans many responses). Its Sonnet 5.5 model page sums it up as "very verbose."

Anthropic's own launch post backs this up. It says Sonnet 5.5 "complements Opus 5.5 best when running at lower effort settings, where it costs less per task," and that at higher settings "it can perform comparably at a similar cost."

You'll also see Anthropic claim Sonnet 5.5 costs "up to 30% less" per task. That figure compares it with Sonnet 5, its predecessor, and Anthropic doesn't put a percentage on the saving against Opus.

### What this means on a Claude plan

On Pro or Max, you pay in usage limits, and high effort spends them faster. Every Claude plan has limits that reset on a rolling five-hour window, and paid plans add weekly limits on top, per Anthropic's plans page.

If your weekly limit keeps vanishing on Sonnet, check the effort setting before you blame the model. Anthropic's effort settings guide says Low and Medium work well for routine tasks and stretch your usage further.

## Benchmarks: whose numbers say what

Two sets of benchmark numbers are floating around for these models, and they don't match. That's normal. Different testers use different test setups at different effort levels, so **always check who ran the test before you quote it.**

### Anthropic's launch numbers

Anthropic's Sonnet 5.5 launch table puts the two models **close on almost everything:**

| 📊 Benchmark (Anthropic-reported) | ⚡ Sonnet 5.5 | 🧠 Opus 5.5 | 
|---|---|---|
| Terminal-Bench 4.0 (agentic coding) | 70.6% | 66.4% | 
| FrontierCode 1.1 (coding) | 52.1% | 54.4% | 
| CursorBench 4.0 (coding) | 55.5% | 57.8% | 
| GDPval-AA v2.1 (knowledge work) | 1844 Elo | 1846 Elo | 
| Humanity's Last Exam (with tools) | 64.5% | 67.7% | 
| OSWorld 2.1 (computer use) | 80.1% | 81.8% | 

**Terminal-Bench 4.0 is the only row where Sonnet beats Opus.** Opus's 66.4% there is its best score, run at Extra high effort.

Sonnet's FrontierCode score is its Extra high result, since it scored lower at Max (46.2%). The GDPval-AA run was done by Artificial Analysis on a pre-release version of Sonnet 5.5.

Anthropic's table also includes OpenAI's GPT-6 Sol, which trails both Claude models on FrontierCode and GDPval-AA. If you're weighing Claude against OpenAI's newest models, our Opus 5.5 vs Astra comparison runs that matchup on real tasks.

### Artificial Analysis's numbers

Artificial Analysis ran both models on its own test setup. At Max effort, the two come out like this:

| 📊 Benchmark (Artificial Analysis, Max effort) | ⚡ Sonnet 5.5 | 🧠 Opus 5.5 | 
|---|---|---|
| Intelligence Index | 56 | 58 | 
| Terminal-Bench 4.0 | 63.6% | 59.6% | 
| AutomationBench-AA (business workflows) | 71.8% | 69.5% | 
| AA-Briefcase (knowledge work) | 1823 Elo | 1807 Elo | 
| GDPval-AA (knowledge work) | 1838 Elo | 1866 Elo | 
| Humanity's Last Exam | 55.0% | 61.4% | 
| AA-Omniscience (factual knowledge) | 32 | 46 | 

Same story on Terminal-Bench, lower numbers. Anthropic says 70.6% vs 66.4%, Artificial Analysis says 63.6% vs 59.6%, and Sonnet leads in both.

**The lead flips below Max.** At Medium, Artificial Analysis has Opus at 52.5% on Terminal-Bench and Sonnet at 29.8%. Opus also leads clearly on factual recall (AA-Omniscience), which is the one to watch if you use Claude for research. Google's Gemini 4 Carbon, announced October 28, now tops that test, 54 to Opus 5.5's 48 on AA-Omniscience v1.1, though only a subset of cyber defenders can use it so far. Our Gemini 4 Carbon vs Claude Opus 5.5 comparison has the details.

One note on AutomationBench: Anthropic published a score only for Opus 5.5 (40.0%, run by Zapier). The 71.8% and 69.5% above come from Artificial Analysis's own version, so don't mix the two.

### Speed

**Sonnet 5.5 streams faster at every effort level, but at Max, Opus finishes first.** At Max, Artificial Analysis clocked Sonnet at 135 output tokens per second and Opus at 95.

That's because Sonnet has so much more to write at Max: the average task took it about 932 seconds to Opus's 800. At Low and Medium, Sonnet starts answering in under a second, while Opus takes 12 to 25 seconds to produce its first token.

## What happened in hands-on tests

Two hands-on testers put both models on everyday work, from landing pages and investor decks to coding bug fixes, and their results line up with the cost data.

### Nate Herk's 7 tasks

Nate Herk ran both models on High effort with identical prompts across seven tasks in his video I Tested Sonnet 5.5 vs Opus 5.5. **Sonnet took four rounds and Opus took three,** and he's upfront that the score is about value, since Opus produced the better first draft more often.

1. **Landing page:** Sonnet, on value. Opus built the better page, but Sonnet ran 28 minutes to Opus's 52 at about half the cost.
2. **Motion design:** Opus. Its output was much better, at $20.55 to Sonnet's $14.37.
3. **A vague 90-day growth plan:** Opus. This was his intentionally vague prompt, and Opus handled the open brief better.
4. **Spreadsheet and investor deck:** Opus. Outputs were close, and Opus was both cheaper and faster on this one.
5. **Visual explainer:** Sonnet, on value.
6. **AI news dashboard:** Sonnet, on value, even though Opus had the better one-prompt build.
7. **YouTube resource guide using a skill:** Sonnet. With a skill defining what good looks like, Sonnet followed the instructions closely.

**Give Sonnet a clear definition of done, and it often matches Opus for less.** Leave the brief open, and Opus fills the blanks better. (Yes, the cheaper model also won the round about following instructions. Sometimes less imagination is a feature.)

### The New Stack's coding test

Jessica Wachtel at The New Stack gave both models three coding tasks at max effort, five runs each, graded against hidden test suites.

**Sonnet 5.5 passed all 15 runs and Opus 5.5 passed 13,** with both Opus misses on a concurrency test, where it spent all 128,000 output tokens thinking and never produced an answer.

There was a hiccup, though. On the first attempt at the agentic bug-fix task, Sonnet thought so long in single steps that it hit a 32,000-token per-step limit in four of five runs. Wachtel raised the limit to 128,000, reran those four, and all of them passed.

Counting the failed attempts, Sonnet's total came to $14.09 against $22.07 for Opus, still about 36% less.

Wachtel's own verdict splits the work. She'd make Sonnet 5.5 the default for hard coding work, with its output limit set high, and stick with Opus 5.5 for agent loops, where it finished about 35% faster and, counting Sonnet's failed runs, cost less.

The lesson for your own setup: if your agent framework caps tokens per step, give Sonnet more headroom or drop its effort level.



## Which model to use for what

Based on Anthropic's own guidance and the tests above, **here's how I'd split real work between the two:**

| 🧩 Task | ✅ Pick | 💡 Why | 
|---|---|---|
| Bug fixes and well-specified code changes | Sonnet 5.5 | Anthropic's stated sweet spot; leads Terminal-Bench | 
| Documents, spreadsheets, and slides | Sonnet 5.5 | Named in Anthropic's launch post; Opus won one deck test, so compare if quality is close | 
| Quick questions and chat | Sonnet 5.5 at Low or Medium | Sub-second first token | 
| Subagents and high-volume steps | Sonnet 5.5 (or Haiku 5.5 for the simplest steps) | Half Opus's token price, faster streaming | 
| Complex or high-stakes code reviews | Opus 5.5 | Built for careful judgment | 
| Vague goals and open-ended research | Opus 5.5 | Won the fuzzy-prompt test; stronger factual recall | 
| Long-running agents | Opus 5.5 | Anthropic's pitch for it; won The New Stack's agentic test | 
| Architecture and planning | Opus 5.5 | Strongest on open-ended work that needs sustained judgment | 

### Use both: Opus plans, Sonnet builds

Claude Code has a built-in mode for this split: **opusplan** uses Opus while you're in plan mode, then switches to Sonnet to execute, per Anthropic's Claude Code model configuration docs.

One customer in Anthropic's launch post describes the same habit. Kevin Ngo, a creative coder at Creator, says that when Opus 5.5 sets the architecture, he'd feel confident letting Sonnet 5.5 implement it.

The same split works outside the terminal. If your team runs its AI work through Slack, Lindy is an AI teammate that lets you choose the model for each task (most major models are available), so routine asks and judgment-heavy work don't have to share one model.

## How to switch between Sonnet 5.5 and Opus 5.5

**A few details trip people up when they move between the two:**

1. **Claude Code defaults to Opus 5.5.** On Pro, Max, Team, Enterprise, and the API, the default model is Opus 5.5. Type /model to switch to Sonnet mid-session, or start with claude --model claude-sonnet-5-5.
2. **Update Claude Code first.** Sonnet 5.5 needs version 2.1.284 or later, and Opus 5.5 needs 2.1.280 or later.
3. **Check what "sonnet" means on your platform.** The sonnet shortcut points to Sonnet 5.5 only on Anthropic's own API. On Amazon Bedrock and Google Cloud, it still points to Sonnet 4.5, so use the full model name there.
4. **Set effort on purpose.** On the API, Sonnet 5.5 defaults to High and Opus 5.5 to Medium, so leaving both on defaults gives the cheaper model more thinking. In Claude Code, both default to Medium.
5. **Know the thinking rules.** Opus 5.5 always thinks before answering. Sonnet 5.5 can't switch thinking off either, but on the API you can limit it to thinking between tool calls, which is its lowest setting.
6. **Expect different safety fallbacks.** Sonnet 5.5 is the first Sonnet with cyber fallbacks like the ones Anthropic built for its top models. Flagged cybersecurity requests re-run on Sonnet 5, while Opus 5.5 hands them to Opus 4.8, per the Claude Code docs.



## Start on Sonnet 5.5 and save Opus 5.5 for the fuzzy stuff

For most people, the Sonnet 5.5 vs Opus 5.5 choice comes down to one question: do you know what the finished work should look like?

If you do, **start on Sonnet 5.5 at Low or Medium effort.** It's cheaper per task at every matched setting, much faster to respond, and close to Opus on well-defined work.

If you don't, or the work is high-stakes, **go straight to Opus 5.5.** And the moment you're tempted to push Sonnet to Max, switch models. Opus at Extra high gets the same score for less.

If you're comparing Opus with the top of the Claude lineup too, our Opus 5.5 vs Fable 5.1 breakdown covers when the priciest model earns its spot.

Still deciding between Claude and OpenAI's chatbot? Our Claude vs ChatGPT comparison covers that call. And if Claude's limits don't fit your work at all, there are solid Claude alternatives worth a look.

## Frequently asked questions

### Should I use Opus or Sonnet?

**Use Sonnet 5.5 for well-scoped tasks and Opus 5.5 for open-ended or high-stakes work.** Sonnet costs half as much per token and less per task at matched effort levels. Opus scores higher on judgment-heavy work and reaches Sonnet's top Intelligence Index score for less money.

### Is Sonnet 5.5 cheaper than Sonnet 5?

**Sonnet 5.5 costs the same per token as Sonnet 5, except cache reads are half price.** Both charge $2 and $10 per million input and output tokens. Anthropic says Sonnet 5.5 costs up to 30% less per task because it needs fewer tokens.

### Is Sonnet 5.5 better than Opus 5.5 at coding?

**Sonnet 5.5 beats Opus 5.5 on Terminal-Bench 4.0, but Opus leads on most other coding tests.** Anthropic reports 70.6% vs 66.4% on Terminal-Bench, while Opus leads FrontierCode and CursorBench. At High effort and below, Artificial Analysis shows Opus well ahead on Terminal-Bench (52.5% vs 29.8% at Medium).

### How much more does Opus 5.5 cost than Sonnet 5.5?

**Opus 5.5 costs twice as much as Sonnet 5.5 per token,** at $4 and $20 per million input and output tokens vs $2 and $10. Per task, Artificial Analysis measured Opus at $1.34 vs $0.48 at Medium effort and $5.98 vs $5.46 at Max.

### Are Sonnet 5.5 and Opus 5.5 both in Claude Code?

**Yes, both Sonnet 5.5 and Opus 5.5 are available in Claude Code** on Pro, Max, Team, Enterprise, and the API. Opus 5.5 is the default, and you can switch with /model or claude --model claude-sonnet-5-5. Claude Code isn't included on the Free plan.

### Is Claude Sonnet 5.5 free?

**Yes, Claude Sonnet 5.5 is available on Claude's Free plan,** on web, iOS, and Android. Opus 5.5 isn't included on Free and starts with the Pro plan at $20 a month.

## About the editorial team

**Flo Crivello**Founder and CEO of Lindy

Flo Crivello is the founder and CEO of Lindy. Before that, he founded Teamflow and was a product manager at Uber. He writes about technology, startups, and the future of work on his blog.
