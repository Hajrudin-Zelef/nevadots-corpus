---
id: collect-261121-veille-ia/claude-anthropic/claude-fable-5-2-review-price-tests-and-one-week-of-work
title: "Claude Fable 5.2 review: price, tests and one week of work"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-11-09"]
keywords: ["fable", "claude", "model", "anthropic", "november", "never", "week", "launch"]
source: https://improvado.io/blog/claude-fable-5-2
source_anchor: ""
source_lines: [1, 184]
section: "Claude_Anthropic"
---
**Claude Fable 5.2 follows your instructions closely, old ones included. So read your instructions before you judge the model.**

Anthropic released Claude Fable 5.2 on 9 November 2026 at $8 per million input tokens and $40 per million output tokens, 20% below Fable 5.1. In Improvado's trial of three everyday jobs, it finished the same jobs as Opus 5.5 at about twice the cost. It also treats a rule that says "never" as absolute, old rules included.

A token is a small chunk of text, and models are priced by the million. Our working rule after one week follows Anthropic's own advice: Opus 5.5 for everyday work, Fable 5.2 for the long, hard jobs. First, audit the instructions you wrote for older models.

Next step: ask an AI agent your marketing data questions.

We counted about 106,000 agent turns in one operator's Claude sessions during launch week, which ended on 15 November 2026. An agent turn is one reply from a model.

## Stage 1: Claude Fable 5.2 price, specs and a slow launch week

Anthropic calls Claude Fable 5.2 its most capable generally available model. It holds 1 million tokens of context, the amount of text it can keep in view at once. It can write up to 128,000 tokens in one answer, per Anthropic's models overview, read on 15 November 2026.

Anthropic's launch post says Claude Fable 5.2 writes output up to 30% faster than Fable 5.1. The price fell by a fifth. An optional fast mode runs up to 2.5 times faster at double the price.

| Model | Input | Output | 
|---|---|---|
| Claude Fable 5.2 | $8 | $40 | 
| Claude Fable 5.2, fast mode | $16 | $80 | 
| Claude Fable 5.1 | $10 | $50 | 
| Claude Opus 5.5 | $4 | $20 | 

Prices in US dollars per million tokens, from Anthropic's pricing page, read on 15 November 2026.

Anthropic lists Claude Fable 5.2 on its Pro, Max, Ultra, Team and Enterprise plans. Developers reach it through Anthropic's API, the interface other software uses to call the model. It also runs on Amazon Web Services, Google Cloud and Microsoft Foundry. The model ID is claude-fable-5-2.

The model takes five effort levels: low, medium, high, xhigh and max. Effort sets how long the model thinks before it answers. Anthropic's API starts at high. Claude Code, Anthropic's coding tool, starts at medium. So one model can behave like two, depending on where you open it.

**How launch week went for us.** On launch evening our operator asked for a Fable 5.2 session. The session that opened ran Fable 5.1, because our session launcher knew only the older name. An internal workspace our own team uses had the same gap. Choosing Fable there still ran 5.1 until an engineer changed the setting on 12 November. That same day, three days after launch, our main codebase received its first code change credited to Fable 5.2.

| Model credited as co-author | Code changes | 
|---|---|
| Claude Opus 5.5 | 452 | 
| Claude Sonnet 5.5 | 133 | 
| Claude Fable 5.2 | 44 | 
| Claude Fable 5.1 | 11 | 

Our main codebase, 9 to 15 November 2026, counted on 15 November. A commit is one saved code change, and its message can credit an AI model as co-author. 670 commits credited a Claude model. Other entries are left out of the table, and a commit can credit more than one model.

Our count credits Fable 5.2 on 44 of those 670 commits, about 7%. Commits crediting Fable 5.1 kept arriving until 13 November. Check which model your tool reports, and set the effort level yourself.

## Stage 2: to Fable 5.2, never means never

Claude Fable 5.2 follows its instructions closely. Anthropic's prompting guide for the model, read on 15 November 2026, puts it in one sentence: "The model treats an instruction that contains 'never' as absolute." To Claude Fable 5.2, never means never.

The guide gives an example. It suggests "ask for approval before deleting files" where you want the model to keep responsibility over an action, in place of "never delete files". It then lists kinds of old text that now work against you. Three matter for a team with saved prompts.

Anthropic measured the first kind. It tested an approval rule that covered "sending messages to people outside this conversation". In simulated tasks where nobody was there to approve, the model held back about half of its routine requests to colleagues and other teams.

**What we saw.** Five days after launch one of our engineers flagged the same pattern to the team. Agents on the new model were asking for approvals they did not need. Two of our rules contradicted each other, so the model had to choose. Our own writing rule had the shape Anthropic warns about, a hard word limit per sentence. So we opened the file that each of our agent sessions reads first.

## Stage 3: the file that grew back

Each agent session at our company starts by reading one shared instruction file. It holds the house rules. Those rules say where knowledge lives, what needs a person's approval and how a report is written. Claude Code reads a file like this at the start of every session.

Our code host keeps each saved version of the file, and we read the sizes there on 15 November 2026. In May a cleanup cut it from 52.7 kilobytes (KB) to 10.7 KB, a cut of 80%. Then it grew back.

By 30 September the file on our code host had passed its old size and reached 58.5 KB. A second cut on 1 October took it down to 37.4 KB. On 14 November it stood at 48.6 KB. That is 223 lines and about 6,600 words, read by each session before its first task.

The May cleanup shows the intent was a short file. The file changed 130 times since 20 May, cuts included, and September alone brought 36 changes. The file itself suggests how. Many rules carry the date of the problem that prompted them. One rule in the file even says to add a rule only when all sessions need it. No check enforced that rule.

An older model may have skipped a stale rule. Claude Fable 5.2 follows it. Our audit of the 14 November file counted:

- 79 uses of "never" or "always": 69 of the first and 10 of the second.
- 22 dates written into the rules.
- 2 pairs of rules that carry the same number.
- 2 direct conflicts between the file and our shared memory notes, the saved notes that agents reread.

The audit listed 14 findings, 7 of them with high confidence. One was the word limit from stage 2. Another was a rule on sending messages, written one way in the file and the opposite way in a memory note. A model given both has to guess which one we meant.

## Stage 4: Fable 5.2 vs Opus 5.5, twice the cost for the same jobs

The second lesson of the week was about money. Claude Fable 5.2 costs twice what Opus 5.5 costs per token. Anthropic's model guide, read on 15 November 2026, gives this advice: "For everyday work, Opus 5.5 at medium is still the place to start."

On hard tests the gap is real. Anthropic's launch table puts Claude Fable 5.2 first of five models on agentic coding. On business workflows it sits second, behind Gemini 4 Carbon.

| Model | FrontierSWE v2, agentic coding | AutomationBench, business workflows | 
|---|---|---|
| Claude Fable 5.2 | 69.9% | 51.4% | 
| Claude Fable 5.1 | 56.3% | 31.4% | 
| Claude Opus 5.5 | 62.3% | 42.5% | 
| GPT-6 Astra | 65.5% | 41.4% | 
| Gemini 4 Carbon | 61.2% | 53.9% | 

Scores from the table in Anthropic's launch post, read on 15 November 2026. The post does not say who ran each competitor's score.

Anthropic adds a cost claim in the same post. In early testing on its FrontierCode 2.0 test, Fable 5.2 at high effort scored about level with Opus 5.5 at max effort, for roughly half the cost per task.

Everyday work told a different story. On 10 November we ran a small trial. Both models got three jobs: find scripts that use a shared module, write a tested function, and scan three daily logs. Fable 5.2 ran each job at high and at medium effort. Opus 5.5 ran each at medium. That makes nine runs, one per cell.

| Model and effort | Cost of the three jobs | Against Opus 5.5 | 
|---|---|---|
| Claude Fable 5.2, high | $3.28 | 2.1 times | 
| Claude Fable 5.2, medium | $2.96 | 1.9 times | 
| Claude Opus 5.5, medium | $1.58 | 1.0 times | 

Our own trial, 10 November 2026, one run per cell. Each cost includes the instruction text a session loads at its start.

All nine runs finished without an error. None asked a question, and none started a helper agent. A separate, harder test found no quality gap at medium effort. It hid bugs in three sets of code and ran each model five times per set. Fable 5.2 at medium averaged 0.94, 1.00 and 0.86 out of 1.00. Opus 5.5 at medium averaged 0.94, 1.00 and 0.84.

Instructions add to that bill. Each session pays to read its instruction file before the first task. Read fresh, that text costs twice as much on Fable 5.2 as on Opus 5.5. Read from the cache, it costs the same on both: $0.20 per million tokens, per Anthropic's pricing page on 16 November 2026.

### Fable 5.2 on marketing data: three questions, eighteen runs

Some marketing questions are table work, so we tested that as well. On 16 November 2026 both models got the same small table from our own lead data. It holds eight weeks of email submits and booked calls for five traffic segments. We anonymized it first. Segment names became letters, except the label Other, which holds two small segments added together. Each count was multiplied by one hidden constant, then rounded.

Each model answered three questions, three times each. Each run was set to medium effort and finished in one turn, with no tool call.

| Question | Claude Fable 5.2 | Claude Opus 5.5 | 
|---|---|---|
| Which segment lost the most email submits from one week to the next? | 3 of 3 right | 3 of 3 right | 
| Booked calls per 100 email submits: which segment is highest, and which lowest? | 3 of 3 right | 3 of 3 right | 
| Which week had the most email submits, and what share came from each segment? | 3 of 3 right | 3 of 3 right | 
| Cost of the nine runs | $0.71 | $0.28 | 
| Average time per run | 4.8 seconds | 6.0 seconds | 

Our own test, 16 November 2026. Code checked each answer against the table. Cost is what the command-line tool reported for each run, its own start-up text included. The table, the three prompts and the eighteen raw answers are in one data file.

Take the first question. All six answers named Segment D, from week 2 to week 3, a drop of 29. That is the right answer. On these three questions, the cheaper model gave the same right answers. The everyday jobs above measured only cost and completion.

This is a narrow test. Three questions on one clean table do not show how either model handles a messy export or a vague request.

In the sessions we counted, Fable 5.2 took a small share. One operator's week shows the split.

In our count Fable 5.2 took 5.4% of the turns, all inside 10 main sessions. No helper agent ran on it. We give it the long jobs: research that runs for hours, and audits like the one in stage 3.

The Improvado AI Agent, which answers customers' data questions, did not move either. No usage event it recorded in launch week named a Fable model.

Anthropic draws the same line in its model guide: "Move up to Claude Fable 5.2 for the hardest technical projects in coding, finance and the sciences."

## Stage 5: the fix, a shorter file with a 20 KB cap

The fix we recommend sorts each line of the file into one of three places.

A fix needs two guards against regrowth. Give the core a size limit. We would set 20 KB for our file, under half its size today. Then let an automatic check hold each new change against that limit.

Behavior should be measured as well. Run five typical tasks on the old instructions and on the new ones, and compare what the agents do. Our own takeaway is to repeat the audit at each model release. Rules written for last year's model become this year's delays. As of 15 November 2026 we have not shipped a shorter file.

## Claude Fable 5.2 prompting: run the same audit on your prompts

The same check should work for a team that keeps instructions for an AI model. That covers a brand voice prompt, a reporting agent's rules, or project notes saved months ago.

1. Ask your tool which model and which effort level it is running. Write both down.
2. Search your saved prompts, memory notes and instruction files for "never" and "always". Count them.
3. For each hit, pick one of three: keep it with its reason, change it to "ask before", or delete it.
4. Find rules that contradict each other and keep one.
5. Delete lines that tell the model to think less, and set the effort level in their place. Rewrite hard word limits as a goal for the reader.
6. Run your three longest tasks again. Note each approval the model asks for and does not need.
7. Give the file an owner and a size limit before the next model arrives.

Data has the same problem. An agent that reads two definitions of one metric has to guess, like an agent that reads two rules. Improvado connects Claude to governed marketing data through its Model Context Protocol (MCP) server. MCP is the open standard that lets a model call outside tools. See the MCP server for marketing data.

## Claude Fable 5.2 FAQ

### What is Claude Fable 5.2?

Claude Fable 5.2 is Anthropic's most capable generally available model, released on 9 November 2026. It has a 1 million token context window, five effort levels and an optional fast mode. Its model ID is claude-fable-5-2.

### How much does Claude Fable 5.2 cost?

Claude Fable 5.2 costs $8 per million input tokens and $40 per million output tokens, per Anthropic's pricing page on 15 November 2026. Fast mode costs $16 and $80. Reading cached text costs $0.20 per million tokens. Anthropic lists the model on its Pro, Max, Ultra, Team and Enterprise plans.

### Is Claude Fable 5.2 better than Fable 5.1?

Anthropic's launch table lists Claude Fable 5.2 at 69.9% on the FrontierSWE v2 coding test, against 56.3% for Fable 5.1. Anthropic also says it writes up to 30% faster, and it costs 20% less. Plan for one change in behavior: it follows instructions closely, so review prompts written for earlier models.

### Should I use Claude Fable 5.2 or Opus 5.5?

Start with Opus 5.5 for everyday work, which is Anthropic's own advice. Move up to Claude Fable 5.2 for long, hard projects. In our three-job trial both models finished each job, and Fable 5.2 cost about twice as much.

### What is the difference between Claude Fable 5.2 and Mythos 5.2?

Anthropic says Claude Fable 5.2 and Claude Mythos 5.2 are the same underlying model. Fable 5.2 carries cybersecurity and biology safeguards and is open to all users. Mythos 5.2 goes only to vetted organizations in cybersecurity and life sciences.

### Does Claude Code support Claude Fable 5.2?

Yes. Claude Fable 5.2 needs Claude Code version 2.1.321 or later, per Anthropic's Claude Code documentation on 15 November 2026. Claude Code runs it at medium effort unless you change the level. Depending on your plan, Fable usage can bill to usage credits.

## The one thing to remember

**Claude Fable 5.2 follows your instructions closely, old ones included. So read your instructions before you judge the model.** Our shared file was cut to a fifth of its size in May and passed its old size 124 days later. The new model's first week sent us back to read it.

**Related reading:** MCP server for marketing data, AI agent for marketing data, marketing analytics for marketing teams
