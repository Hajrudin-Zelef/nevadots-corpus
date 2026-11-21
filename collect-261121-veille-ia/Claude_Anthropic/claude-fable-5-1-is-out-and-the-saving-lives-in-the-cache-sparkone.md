---
id: collect-261121-veille-ia/claude-anthropic/claude-fable-5-1-is-out-and-the-saving-lives-in-the-cache-sparkone
title: "Claude Fable 5.1 is out, and the saving lives in the cache — SparkOne"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-09-01"]
keywords: ["claude", "cache", "fable", "model", "mythos", "through", "same", "price"]
source: https://sparkone.nl/en/blog/claude-fable-5-1-what-changes/
source_anchor: ""
source_lines: [1, 144]
section: "Claude_Anthropic"
---
Of every model release I have followed this year, Fable has the strangest arc. Fable 5 arrived on June 9, the first Mythos-class model anyone could use. I put an afternoon of real work through it and wrote up what happened. Three days later it was pulled offline under a US government directive, and the question stopped being what the model could do and became whether you dared build on it.

Today, September 1, there is a successor. Claude Fable 5.1 and Claude Mythos 5.1, announced on Anthropic’s blog and pushed out in parallel as an eight-post thread, running word for word the same on the official Threads account and on X. I read both threads, then the announcement, then the docs. The number that moves your bill the most sits fifth in the thread, and under the cost heading on the announcement page.

Anthropic also put out a video of just over a minute, in which Alex Albert of Research Product Management walks through the release. It is below.

## What was announced

These are two models running on one set of weights. Fable 5.1 is generally available; Mythos 5.1 is the same model with looser safeguards, reachable only through vetted access programmes. That split existed at version 5 too. A footnote on that release explains that *fabula* and *mythos* both mean “that which is told”, and that the safeguards are the only difference between the names.

| What | Fable 5.1 | Where it comes from | 
|---|---|---|
| Model ID | `claude-fable-5-1` | Claude API, Bedrock, Google Cloud, Microsoft Foundry, Claude Platform on AWS | 
| Input / output price | $10 and $50 per million tokens | Unchanged from Fable 5 | 
| Cache reads | $0.25 per million tokens | Was $1, so 75% off | 
| Cache writes | $12.50 (5 min) and $20 (1 hour) | Unchanged | 
| Context window | 1M tokens, output up to 128K | Same class as Opus 5 and Sonnet 5 | 
| Default effort | `high` | Cowork and claude.ai default to medium | 
| Knowledge cutoff | June 2026 | Both training and reliable knowledge | 
| Retirement | Not sooner than September 1, 2027 | Model status page in the docs | 

Mythos 5.1 costs exactly the same and sits in the price table marked “limited availability”. Access runs through two programmes: the Cyber Verification Program for defensive security work, which will add Mythos-class models in the near future, and the Life Sciences Verification Program, whose first participants Anthropic says were enrolled in partnership with the US government. Claude Security, the product that scans codebases for vulnerabilities, now runs on Mythos 5.1.

## The price change sits in the cache

In the thread, the cache price shows up as the fifth post, wedged between benchmark claims and safety claims, and I read straight past it the first time through.

Prompt caching works on the front of your request. Every turn, an agent resends the whole context, and the API checks whether the start of it matches something it processed before. If it matches, that stretch is billed at the cache rate instead of the input rate. On nearly every Claude model that rate is 0.1 times the input price. On Fable 5.1 and Mythos 5.1 it is 0.025 times, according to the docs.

In money: cache reads went from $1 to $0.25 per million tokens. Anthropic works out what that does to a total bill, and that figure does not come from a model. It comes from four weeks of actual usage in August 2026, measured at the default effort level. For a typical workload spanning Claude Enterprise, Claude Code and the API, the indexed cost drops from 100 to 75. For context-heavy, tool-heavy workloads, where cache reads are most of the bill, it drops to 55.

Run a long session where the same project context comes past a hundred times and you pay a quarter of the old rate for that stretch. Fire off short one-shot requests with fresh context every time and you will barely notice, because input and output pricing did not move. In my piece on what my daily loop costs I argued that a task running once a day never has a warm cache to come back to. That still holds. The discount applies to re-reads inside a session, and a daily task does have a prefix that recurs within its own run, but nothing carries over between two runs.

## What changes in your agent without you touching anything

The migration guide lists three behaviour changes that show up with no code change, and the first one costs you money in exactly the place the cache discount hands it back.

**Parallel tool calling is more variable.** In long agent loops where the next independent reads are only implied by the task, Fable 5.1 may issue one tool call per turn where Fable 5 batched several. The docs name the places this surfaces: custom coding agents, bash-and-editor harnesses, computer use. Every extra turn costs tokens, a round trip and wall-clock time. What does not get worse, according to the same source, is the answer. Requests that name several things to fetch still run in parallel, and the prompting guide carries a one-line instruction that restores the old behaviour.

**Fewer progress updates during long tool runs.** The model writes less user-facing text between tool calls, more so at higher effort, and its summaries after a coding session are shorter. If your interface renders those updates, set `thinking.display` to `"updates"` or `"summarized"` and ask explicitly for an opening line, periodic updates and a closing recap.

**Fewer search and retrieval calls.** The third item in the migration guide points the same way: left alone, the model does less of it.

Two hard breaking changes sit alongside those, and the first only bites if you build the `messages` array yourself. Modify anything that sits before a thinking block, meaning the system prompt, the tools, or an earlier turn, and the next request errors. Claude Code, claude.ai, Managed Agents and the Agent SDK keep that prefix intact for you. The check is enforced for accounts created on or after August 31, 2026; older accounts only get it when a request opts in through a parameter. Mythos 5.1 does not run the check at all.

What those three changes add up to in day-to-day work is not something a benchmark shows. The sharpest description I saw came from Ado, a user who posted on X eight minutes after the announcement: the model gets further into a task before it needs your input, it says when it is stuck instead of pretending otherwise, and it skips the easy-looking shortcut to fix the actual root cause. That is one person’s observation on launch day, not an official claim and not measured, so treat it as a hypothesis to test yourself. The two hard claims in that same post, same price as Fable 5 and cache reads 75% cheaper, do match the price table.

The second breaking change is subtler and matters to anyone running a router or a fallback between models. Every thinking block records which model produced it, and that is preserved in one direction only: Fable 5.1 reads earlier models’ thinking blocks, and no earlier model reads Fable 5.1’s. Switch mid-conversation back to Opus 5 and the API drops those blocks before the model sees them. They do not count toward `input_tokens` and you are not billed for them, but without a beta header you get no notice that it happened.

## The benchmarks, including the number that reads worse

The thread quotes hard figures: 52.6% on Terminal-Bench-Science 0.1, more than double Fable 5, and 55.8% on Terminal-Bench 4.0 against 42.0% for Fable 5. The announcement page carries the same series as charts, where only the axis labels come through as text. Anthropic ran the same thread on X, and there the benchmark post carries a table image where the numbers are readable, including a competitor column that appears nowhere as text on the announcement page.

| Benchmark | Fable 5.1 | Fable 5 | Opus 5 | GPT-5.6 Sol | 
|---|---|---|---|---|
| Agentic scientific research (Terminal-Bench-Science 0.1) | 52.6% | 24.7% | 29.0% | 22.4% | 
| Agentic coding (Terminal-Bench 4.0) | 55.8% | 42.0% | 52.3% | 37.3% | 
| Knowledge work (GDPval-AA v2) | 1853 | 1723 | 1824 | 1711 | 
| Computer use (OSWorld 2.0) | 77.9% / 41.7% | 72.9% / 36.1% | 75.4% / 39.6% | no score | 
| Multidisciplinary reasoning (Humanity’s Last Exam) | 60.9% / 65.0% | 57.8% / 63.8% | 56.6% / 63.6% | no score | 
| Business workflows (AutomationBench) | 31.4% | 17.1% | 26.9% | 19.6% | 
| Agentic coding (CursorBench 3.2.0) | 73.4% | 70.5% | 70.0% | 67.2% | 

Two values appear in two rows: for OSWorld the partial and the strict count, for Humanity’s Last Exam without and with tools. Mythos 5.1 reaches 60.9% on Terminal-Bench 4.0 against Fable 5.1’s 55.8%. That gap comes from the safeguards, and I come back to it below.

The GPT-5.6 Sol column carries no source and was run by Anthropic itself. A table where the vendor measures both its own model and a rival’s is not a comparative test. That column is missing for OSWorld, and a footnote explains why: those tasks come from the August 2026 release and are not comparable to previously published OSWorld results.

The footnote under those charts is more useful than the scores themselves. The standard error on Terminal-Bench-Science 0.1 is 3.5 to 4.5 points per model. That benchmark’s public leaderboard, running three trials per task in a Claude Code harness, puts Opus 5 at 30.0% and Fable 5 at 21.4%; Anthropic reproduces those two in its own setup at 29.0% and 24.7% and calls both within noise. A three-point gap in a table like that means very little.

The unflattering number sits in the same footnote. Fable 5.1 was evaluated with production safeguards enabled. On tasks where those safeguards intervened, Fable 5.1 and Fable 5 scored zero on OSWorld 2.0, and Fable 5 scored zero on AutomationBench. For all other interventions, cybersecurity tasks were completed by Opus 4.8 and biology tasks by Opus 5. Anthropic writes that this likely depresses both models’ scores on those benchmarks. The OSWorld results also ran against the benchmark authors’ August 2026 task release, which is why no competitor score is shown at all.

Mythos 5.1 and Fable 5.1 are the same model, so the gap between those two lines on Terminal-Bench 4.0 consists entirely of the tasks where the older, blunter cyber safeguards intervened, and Anthropic expects the gap to shrink now that those safeguards are more precise. A benchmark score here partly measures how tightly your own gatekeeper is tuned.

## Science is the actual claim

The second post in the thread says the research capabilities offer “an early glimpse” of how AI will contribute to scientific progress. That is the kind of sentence I normally skim. Under the announcement sit three examples that are more concrete than the sentence.

On molecular design, Mythos 5.1 got open-source protein design and folding tools, and its designs went to two external organisations for lab validation. On three targets, binding affinities came in ten times higher than the best entries in Adaptyv Bio’s protein design competitions. The hit rate, meaning the share of designs that actually bind, reached nearly 50% across twelve targets, where 10 to 15% is typical in the field. That follows on from the earlier protein work I covered, with a higher strike rate across fewer targets.

On computation, Fable 5.1 trained a neural network that turned thirty-year-old radar imagery from NASA’s Magellan mission into a new elevation map covering a third of Venus at 300-metre resolution. One of the images shows a shield volcano fifteen kilometres across.

On computational biology, Mythos 5.1 wrote custom GPU kernels and cached intermediate results, speeding up seven open-source models by up to 2.5 times with identical outputs. Per model the gain runs from 1.4 times for Enformer and Evo 2 40B to 2.5 times for ProGen2. Across genome-wide analyses that cuts estimated GPU costs by 30 to 60%: three million ClinVar variants through Evo 2 40B goes from an estimated $18k to $8k at cloud list price. Anthropic notes that this sort of optimisation normally takes a team of performance engineers weeks.

I hold results like these lightly, because they come from the vendor and no third party has reproduced them. They do come in a form someone else can check: a hit rate gets measured in a lab, and a GPU bill shows up on your own invoice.

## For European organisations, EFS is the real news

A separate announcement landed the same day, covering Enterprise Frontier Safeguards. Your data stays on cloud infrastructure you control rather than Anthropic’s, and human review is done by you by default. The effect is the privacy of a zero data retention agreement without giving up misuse detection.

Anthropic says it developed EFS with more than 100 customers across financial services, healthcare, manufacturing, telecom, law, retail and the public sector, together with AWS, Google Cloud and Microsoft Azure. It is supported on Claude Code, Claude Enterprise, the Claude Platform, Amazon Bedrock, Claude Platform on AWS, Google’s Agent Platform and Microsoft Foundry. Rollout happens in phases starting later this autumn, and until then eligible customers get plain zero data retention on Fable 5 and 5.1.

For the environment I work in during the day, public service delivery, that shift matters more than a few benchmark points. Data retention is usually where a pilot dies. What is not written down anywhere yet is how a Dutch or European organisation fits into this, and what “cloud infrastructure controlled by the customer” means for anyone pinned to EU regions. Worth asking before you sign anything.

## What happens to the filters

The safeguards are getting more precise, and that claim has two sides. At the Fable 5 launch in June, the filters were deliberately conservative and fired in under 5% of sessions, with a blocked request handed to Opus 4.8 and the user told about it. Today Anthropic says the biology safeguards for Fable 5.1 and Fable 5 fire 85% less often on benign questions about elementary biology and medical topics, measured against the filters Fable 5 launched with. The thread adds that cybersecurity safeguards flag benign requests about 60% less often. That figure appears word for word in both the Threads and the X thread from the Anthropic account, while the announcement makes the claim without a number, as “less likely to flag benign content”.

The boundary itself has not moved. Questions about research and development in the life sciences still route to the Opus models. Anthropic also writes that Mythos 5.1 is more capable than Mythos 5 but, by its own evaluations, still short of the next risk tier in the Responsible Scaling Policy, which is why it ships with the same safeguards.

## The caveat

**The default shifts toward more expensive habits.** A 75% cut on cache reads makes long, context-heavy agent sessions cheaper, and therefore more normal. Cost per session falls while the number of sessions rises, and only the first of those two shows up on your invoice. Work out what you actually spent in August before concluding you are saving 25%.

**Oversight moves to the customer.** EFS puts your data on your infrastructure and makes you the default human reviewer. That is a privacy win and a workload you inherit: somebody inside your organisation has to be able to do that review, and that is a role with authority attached and a name on it. Request EFS purely to satisfy legal and you have bought an obligation.

**Visibility drains away.** Fewer progress updates between tool calls, shorter summaries after a coding session, and thinking blocks silently discarded when you fall back to an older model. Each of those makes the work smoother and makes it harder to see what happened. Anthropic itself admits the model can still slip past approvals and auto-mode classifiers, and that its automated behavioural audit has less visibility into very long-context work and multi-agent settings. Those are exactly the two situations where you need the narration most.

## What I am doing about it

I am switching nothing today. This site runs on a chain of gates I built myself, tuned to models that behave a certain way in long loops. The note about fewer parallel tool calls hits precisely that, so I want to see what it does to the time and token cost of an ordinary article run before I make anything the default.

What I am redoing straight away is the caching arithmetic. If reads get four times cheaper while writes hold steady, the trade-off between the five-minute cache and the one-hour cache changes shape. That calculation sits in the daily loop piece and needs another pass.

And I am leaving the EFS-in-Europe question alone until there is something concrete to read. Once there is, I will hold it against the requirements that sink pilots where I work.

## Frequently asked questions

### What is the difference between Claude Fable 5.1 and Claude Mythos 5.1?

They are the same underlying model with different safeguards. Fable 5.1 is generally available; Mythos 5.1 has looser filters for cybersecurity and life sciences work and is only reachable through vetted access programmes. Pricing is identical at $10 per million input tokens and $50 per million output tokens.

### Will Claude Fable 5.1 actually be cheaper for me?

Only if your workload leans on prompt cache reads. Input and output pricing is unchanged; the cache rate goes from $1 to $0.25 per million tokens. Measured over four weeks of real usage in August 2026, Anthropic puts the reduction at around 25% for a typical workload and up to 45% for context-heavy, tool-heavy work. Short one-shot requests with no repeated context see no change at all.

### Do I need to change my agent to migrate?

If you use Claude Code, claude.ai, Managed Agents or the Agent SDK, they keep the thinking prefix intact for you and there is nothing to do. If you build the `messages` array yourself, verify that you never modify anything sitting before a thinking block, because that now errors on accounts created on or after August 31, 2026. If you run long agent loops, watch the tool calls: the model batches them less often on its own, and the prompting guide provides an instruction line to restore the old pattern.

## Sources

- Anthropic, “Introducing Claude Fable 5.1 and Claude Mythos 5.1”, September 1, 2026 — https://www.anthropic.com/claude-fable-and-mythos-5-1
- Anthropic, video “Introducing Claude Fable 5.1” with Alex Albert, Anthropic and Claude channel, September 1, 2026 — https://www.youtube.com/watch?v=ROF2Nv_KjOM
- @claudeai on Threads, eight-post announcement thread, September 1, 2026 — https://www.threads.com/@claudeai/post/DcwPo7aEZvU
- @claudeai on X, the same thread with the benchmark table, September 1, 2026 — https://x.com/claudeai/status/2094848572143407483 and https://x.com/claudeai/status/2094848579558953258
- Ado (@adocomplete) on X, an individual observation at launch, September 1, 2026 — https://x.com/adocomplete/status/2094850674181951958
- Anthropic, “Developing Enterprise Frontier Safeguards with our customers”, September 1, 2026 — https://www.anthropic.com/news/enterprise-frontier-safeguards
- Claude Platform Docs, “Claude Fable 5.1” (overview), accessed September 1, 2026 — https://platform.claude.com/docs/en/models/fable-5-1/overview
- Claude Platform Docs, “What’s new in Claude Fable 5.1”, accessed September 1, 2026 — https://platform.claude.com/docs/en/models/fable-5-1/whats-new-fable-5-1
- Claude Platform Docs, “Migrating to Claude Fable 5.1 and Claude Mythos 5.1”, accessed September 1, 2026 — https://platform.claude.com/docs/en/models/fable-5-1/migration-guide
- Claude Platform Docs, “Pricing”, accessed September 1, 2026 — https://platform.claude.com/docs/en/about-claude/pricing
- Anthropic, “Claude Fable 5 and Claude Mythos 5”, June 9, 2026 — https://www.anthropic.com/news/claude-fable-5-mythos-5

Checked on September 1, 2026, a few hours after the announcement. The core holds up against the primary source: the announcement on anthropic.com carries today’s date, the docs list `claude-fable-5-1` as “Active (latest)” with a September 1, 2026 release date, and the price table shows the $0.25 cache rate. Four numbers initially appeared only in the Threads thread: 52.6% on Terminal-Bench-Science 0.1, 55.8% against 42.0% on Terminal-Bench 4.0, and the 60% reduction in false positives on the cyber safeguards. The charts on the announcement page do not expose their values as readable text. After reading the X thread from the same account, all four appear word for word on a second official channel, with a table image where the figures are legible. Two channels from the same sender are not a second measurement, so the numbers remain Anthropic’s own. I have not read the system card, so the safety section above rests on Anthropic’s own summary of it. The video embedded above has not been transcribed either, so no claim in this piece rests on it alone. No independent verification of the benchmarks exists right now: per Anthropic, the public Terminal-Bench-Science 0.1 leaderboard lists only Opus 5 and Fable 5. The GPT-5.6 Sol column was likewise measured by Anthropic and not checked against a source from OpenAI. What happens on subscription plans is documented nowhere; for Fable 5 in June that was spelled out on day one, and its absence now is an open question rather than confirmation that access is included.
