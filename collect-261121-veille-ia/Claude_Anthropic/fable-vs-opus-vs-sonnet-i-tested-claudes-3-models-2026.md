---
id: collect-261121-veille-ia/claude-anthropic/fable-vs-opus-vs-sonnet-i-tested-claudes-3-models-2026
title: "Fable vs Opus vs Sonnet: I Tested Claude’s 3 Models (2026)"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-11-09"]
keywords: ["fable", "opus", "sonnet", "four", "each", "answer", "models", "model"]
source: https://aiempiremedia.com/claude-fable-vs-opus-vs-sonnet/
source_anchor: ""
source_lines: [1, 145]
section: "Claude_Anthropic"
---
*Updated November 2026*

Anthropic released Claude Fable 5.2 on November 9, and it changed the Fable vs Opus question overnight. Fable got faster and cheaper, while Opus 5.5 and Sonnet 5.5 stayed where they were. So which one should you actually use?

Instead of comparing spec sheets, I ran the same four everyday tasks on all three models in the Claude app: a logic puzzle, a small coding job, a client email and a list with strict rules. That’s 12 runs, each in a fresh chat, with effort set to High on every model.

Here’s what happened in this Fable vs Opus vs Sonnet test, what each model costs, and which one I’d pick for what.

|  | Fable 5.2 | Opus 5.5 | Sonnet 5.5 | 
|---|---|---|---|
| API price (input / output, per 1M tokens) | $8 / $40 | $4 / $20 | $2 / $10 | 
| Total time across my 4 tests | About 49 s | About 61 s | About 39 s | 
| Best result in my tests | Client email | Thorough coding answer | Flawed puzzle, rule-following, speed | 
| Weak spot in my tests | Said “tested” without sharing a file | Invented details in the email | None in these 4 tasks | 

Prices are from Anthropic’s pricing page. Times are single runs, measured roughly with a stopwatch.

**My verdict in one line:** for everyday work, Sonnet 5.5 was as good as the two bigger models, faster, and four times cheaper than Fable. My four tasks were short, though, so they don’t show where Fable’s extra power is supposed to matter.

Fable 5.2 launched on November 9, 2026, alongside Mythos 5.2 (CNBC). Two changes matter for this Fable vs Opus comparison:

- **It’s cheaper.** API pricing dropped from $10 / $50 to $8 / $40 per million tokens, a 20% cut on both input and output (Claude Developers on X).
- **It’s faster.** Anthropic says Fable 5.2 is up to 30% faster than Fable 5.1 (same source).

Opus 5.5 ($4 / $20) and Sonnet 5.5 ($2 / $10) were not changed. Even after the cut, Fable still costs twice as much as Opus and four times as much as Sonnet per token.

To keep it fair, I used the same setup for every run:

- **One new chat per test and per model** , so no model saw an earlier answer.
- **Effort set to High** on all three. High is Fable’s default, so this shows each model the way most people use it.
- **The exact same prompt** , pasted unchanged.
- **Time measured with a stopwatch** , from pressing send to the end of the answer. These are single runs, so treat a few seconds of difference as noise.

The four tests:

1. A logic puzzle with step-by-step reasoning
2. A Python function with clear rules and edge cases
3. A short client email with a word limit
4. A list with five strict formatting rules

I wrote a word problem about a shop that sells notebooks at €3 each, or €2.50 each if you buy five or more. A customer makes two separate purchases, the second of four notebooks, and pays €27.50 in total. How many did she buy the first time?

Here’s the honest part: I made a mistake writing it. If the four extra notebooks really cost €3 each, no first purchase adds up to €27.50. I only realised this when all three models told me.

That turned the test into something more useful: does the model notice when a question is wrong, or does it force an answer?

**All three caught the problem.** The difference was in how they handled it.

Fable 5.2 and Opus 5.5 both explained that the puzzle has no solution as written, then offered 7 as the likely intended answer if the discount applies to both purchases. Both checked their maths and flagged the assumption clearly.

Sonnet 5.5 went further. It said plainly “There is no valid answer”, showed why 7 is the tempting shortcut that doesn’t add up (it gives €29.50, not €27.50), and suggested two likely typos with the answer for each.

**Winner: Sonnet 5.5.** It refused to guess and told me exactly what was wrong with my question, which is what you want when the stakes are real. All three took 9 to 10 seconds.

I asked for a Python function, `split_bill(total, people, tip_percent)`, that splits a bill plus tip between people. The rules: round to the cent, raise an error if the number of people is zero or negative, and give any leftover cents to the first person so the parts always add up exactly.

All three wrote nearly the same solution: work in whole cents with Python’s `Decimal` to avoid rounding errors, then hand the remainder to the first person.

To check them properly, I ran all three functions on 20,000 random bills with different totals, group sizes and tip rates. **All three were exact to the cent every time, and gave identical results.** Each also raised the right error for zero or negative people.

The differences were in the extras:

- **Opus 5.5** (about 20 s) shared a downloadable file and added a check I didn’t ask for, which it pointed out so I could remove it.
- **Sonnet 5.5** (about 17 s) also shared a file and added the most input checks, such as rejecting a negative total.
- **Fable 5.2** (about 18 s) said its code was “tested and working” but didn’t share a file. Its example outputs turned out to be correct when I ran them.

**Winner: a tie on the code itself.** For a task this size, Fable vs Opus makes no difference, and Sonnet is just as reliable.

The brief: tell a client their project will be one week late because a supplier sent the wrong components, offer 10% off the final invoice, stay under 120 words, avoid clichés like “we apologize for any inconvenience”, and end with a clear next step.

All three stayed well under the limit and avoided the cliché.

- **Fable 5.2** (about 11 s, 89 words): the most direct. “The delay is ours to answer for” takes responsibility without grovelling, and it asks the client to confirm the new date by a set day.
- **Sonnet 5.5** (about 10 s, 89 words): warm and practical, and the only one to ask whether anything on the client’s side depends on the old date.
- **Opus 5.5** (about 13 s, 106 words): polished, but it**invented facts** I never gave it, such as “our quality checks caught it before anything was installed” and “everything else is on track”. To its credit, it warned me to change those lines if they weren’t true.

**Winner: Fable 5.2,** with Sonnet close behind. This was the clearest Fable vs Opus gap in my tests. Opus wrote well, but an email that adds facts you have to remove is a risk if you send it in a hurry.

The prompt: exactly five grocery-saving tips, one sentence each, 15 words or fewer, no mention of coupons or apps, numbered 1 to 5, then one line saying which tip saves the most and why, with no introduction or conclusion.

**All three followed every rule.** I counted the words: every tip from every model was between 9 and 13 words. The tips themselves were similar (plan meals, buy store brands, compare unit prices, buy seasonal produce, use leftovers).

The real difference was speed: **Sonnet 5.5 answered in about 3 seconds**, Fable 5.2 in about 10, and Opus 5.5 in about 18.

**Winner: Sonnet 5.5,** for matching the quality in a fraction of the time.

Per-token prices are hard to picture, so here’s a simple example: a workload that sends 1 million tokens to the model and gets 200,000 tokens back, at standard API prices.

| Model | Input cost | Output cost | Total | 
|---|---|---|---|
| Fable 5.2 | $8.00 | $8.00 | $16.00 | 
| Opus 5.5 | $4.00 | $4.00 | $8.00 | 
| Sonnet 5.5 | $2.00 | $2.00 | $4.00 | 

Prices are from Anthropic’s pricing page. Anthropic’s Batch API halves these prices for jobs that don’t need an instant answer, and prompt caching lowers the cost of repeated input further.

If you use the Claude app on a subscription rather than the API, you don’t pay per token, but bigger models and higher effort use up your usage allowance faster. For a full breakdown of plans, see my Claude pricing guide, and for Opus specifically, my Opus 5.5 pricing breakdown.

Prices for all three, and what they cost compared with other providers, are kept up to date in our **AI Price Tracker**.

Based on my tests and the prices, here’s how I’d settle Fable vs Opus vs Sonnet:

- **Sonnet 5.5 for most everyday work.** Emails, summaries, short code, structured lists: it matched the bigger models in all four tests, was the fastest overall, and costs a quarter of Fable’s price.
- **Opus 5.5 when you want extra thoroughness.** It added the most safety checks and explanations. Read its writing carefully, though: in my email test it filled gaps with details I hadn’t given it.
- **Fable 5.2 for the hardest, longest jobs.** Anthropic positions it for complex technical work, and it wrote the best email in my test. My four tasks were short, so they couldn’t show where Fable’s extra capability pays off. For everyday prompts, I saw no reason to pay four times Sonnet’s price.

**The honest limit of this test:** four tasks, one run each. A bigger difference may appear on long coding projects, large documents or multi-step agent work. If that’s your use case, run your own task on all three before committing, which is exactly what this kind of test makes easy.

### 1. Is Fable 5.2 better than Opus 5.5?

Fable 5.2 is Anthropic’s more capable and more expensive model, at $8 / $40 per million tokens versus $4 / $20 for Opus 5.5. In my four everyday tests, Fable wrote the best email and Opus was slightly more thorough on code, but neither was clearly better overall. The difference is more likely to show on long, complex tasks.

### 2. What is the difference between Opus and Sonnet?

Opus 5.5 is the larger model and costs twice as much as Sonnet 5.5 ($4 / $20 versus $2 / $10 per million tokens). In my tests, Sonnet was faster and just as accurate, while Opus added more extra checks and longer explanations.

### 3. How much does Claude Fable 5.2 cost?

On the API, Fable 5.2 costs $8 per million input tokens and $40 per million output tokens, down from $10 / $50 for Fable 5.1. In the Claude app, it’s included in paid plans and counts against your usage allowance.

### 4. Which Claude model is fastest?

In my tests, Sonnet 5.5 was the fastest, at about 39 seconds across all four tasks, versus about 49 for Fable 5.2 and 61 for Opus 5.5, all on High effort. It answered the simplest task in about 3 seconds.

### 5. Is Sonnet 5.5 good enough for coding?

For small and medium tasks, yes. In my test, Sonnet’s bill-splitting function was exact to the cent on 20,000 random bills, the same as Fable and Opus. For large codebases or long agentic coding sessions, the bigger models may do better, but my test didn’t cover that.

### 6. Should I use High effort for every task?

Not necessarily. High effort gives the model more room to think, which helps on hard problems but makes answers slower and uses more of your allowance. For quick everyday tasks, a lower setting is often enough.

The Fable vs Opus vs Sonnet choice is less dramatic than the price gap suggests. On four everyday tasks, all three were accurate, all three caught the mistake in my puzzle, and all three followed strict rules.

What separated them was speed, cost and style: Sonnet 5.5 was the fastest and cheapest, Fable 5.2 wrote the cleanest email, and Opus 5.5 was the most thorough but also the most willing to fill in facts on its own.

Start with Sonnet 5.5, and move up to Opus or Fable only when a task proves it needs more. For a wider view of what AI models cost across providers, see my AI pricing 2026 breakdown.

**Mahdi Ayadi** writes and edits AI Empire Media. He covers AI models, tools and pricing with one question in mind: is this actually worth paying for?

Before writing about software, Mahdi spent more than seven years selling it, in B2B SaaS sales and account management across cybersecurity, marketing technology and travel technology. That experience shapes how he reads vendor pricing pages, benchmark claims and feature lists, and it’s why his articles focus on real costs, trade-offs and sources readers can check for themselves.

He also builds AI-powered automation for sales and onboarding workflows, which informs his coverage of AI agents and automation tools. He works in English, French, Arabic and Russian.

Connect on LinkedIn →
