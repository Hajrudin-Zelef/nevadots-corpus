---
id: collect-261121-veille-ia/gemini-google/gemini-4-carbon-launch-benchmarks-pricing-and-access
title: "Gemini 4 Carbon Launch: Benchmarks, Pricing and Access"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["carbon", "gemini", "google", "november", "tokens", "launch", "argon", "model"]
source: https://alphacorp.ai/blog/gemini-4-carbon-launch-benchmarks-pricing-and-everything-you-need-to-know
source_anchor: ""
source_lines: [1, 244]
section: "Gemini_Google"
---
Google opened Gemini 4 Carbon to all paying API developers on November 10, 2026, at $2 input and $10 output per million tokens. The Gemini 4 Carbon launch brings a model that leads 15 of 17 rows on its own scorecard but runs slow, spends tokens freely and doubles in price on December 1. This guide separates Google’s claims from the one outside measurement available, prices a real task instead of a token, and names the workloads where Argon remains the better buy. As of November 10, 2026, every figure here is dated and sourced.

- 15 of 17 benchmark rows: Carbon’s lead over GPT-6 Astra, Claude Opus 5.5 and Claude Fable 5.1 on the Google DeepMind model card, results dated October 2026.
- $2 / $10 per million tokens through November 30, 2026, then $4 / $20: Standard tier on the Gemini API pricing page, checked November 2026.
- $7.76 per Intelligence Index task and a 66.85-second wait for the first answer token: Artificial Analysis measurement of Carbon (High), November 2026.
- 1768 Elo, ranked #4 on WebDev Arena, up 89 points: Google’s October 28, 2026 Carbon announcement.
- 0% indirect prompt injection success on the Gray Swan IPI benchmark, against 0.7% for Argon: Google-reported, November 2026.

## What Is Gemini 4 Carbon and What Google Announced at Launch

Gemini 4 Carbon is Google’s top-tier frontier model, opened to all paying API developers and enterprises on November 10, 2026, and built on the same architecture as Gemini 4 Argon with a much larger budget for thinking tokens. Google positions it for cybersecurity and long-running agentic work. Argon stays the default for everyday engineering and knowledge work.

The two models are siblings. Google DeepMind’s model card for Carbon, published November 10, 2026, says it shares Argon’s architecture, training data and hardware, and differs mainly in how many thinking tokens it spends per task. Argon’s own card describes a sparse mixture-of-experts transformer with a March 2026 knowledge cutoff, and Carbon carries that same cutoff. Anything that happened after March 2026 has to arrive in the prompt.

Google staged the release over four weeks:

1. October 13, 2026: Gemini 4 Argon launched at an introductory $2 per million input tokens and $10 per million output tokens.
2. October 28, 2026: Google announced Carbon but handed it only to a group of cyber defenders through the Fairwind Program, with wider access “coming soon.”
3. November 10, 2026: Carbon reached general availability for paying API developers and enterprises, and began rolling out to Google AI Ultra subscribers in Antigravity, the Gemini app and AI Mode in Search.

The positioning is unusually blunt for a flagship launch. Koray Kavukcuoglu, SVP of Google DeepMind, wrote in Google’s November 10, 2026 availability announcement that Carbon is for tasks that stretch a model’s limits, such as multi-step debugging and open-ended scientific research, and that it gets there by spending more tokens on exploration and verification loops.

“Argon remains the ideal choice for the majority of software engineering and knowledge work, while Carbon is built for tasks that test the limits of a model’s intelligence.” Koray Kavukcuoglu, Google DeepMind, November 2026


The other launch headline is **/swarm**, a preview feature in Google Antigravity. One prompt becomes a coordinator agent with parallel workers in isolated git worktrees and adversarial verifiers that test each change. Google suggests giving Carbon the reasoning-heavy roles (architecture, coordination, auditing) and pairing it with faster, cheaper models for the rest.

What struck me is how much of the launch story is orchestration rather than raw model quality. Google is selling a way to spend Carbon’s expensive reasoning only where it pays off. That framing tells you a lot about the cost profile before you read a single price.

## How Gemini 4 Carbon Performs on Benchmarks Against GPT-5, Claude and Gemini 3

Gemini 4 Carbon leads 15 of the 17 benchmark rows on its own model card, where Google lines it up against GPT-6 Astra, Claude Opus 5.5, Claude Fable 5.1 and Gemini 4 Argon. The comparison set is the October 2026 generation rather than GPT-5 or Gemini 3, and nearly every number is Google’s own.

That last point matters. The Gemini 4 Carbon model card, published November 10, 2026, dates its results “as of October 2026” and says the cyber rows reflect access through the Fairwind Program, run “without guardrails.” Treat the table as a **vendor claim** until outside evaluators cover the same tests.

| Benchmark (Google-reported, October 2026) | Carbon | Argon | GPT-6 Astra | Claude Opus 5.5 | Claude Fable 5.1 | 
|---|---|---|---|---|---|
| Artificial Analysis Index v5.0 | 59 | 50 | 57 | 58 | 49 | 
| Vals Index v2.2 | 70.4% | 69.2% | 65.0% | 70.2% | n/a | 
| Terminal-Bench 4.0 | 65.2% | 57.1% | 59.1% | 59.6% | 52.0% | 
| FrontierSWE 2 | 61.2% | 55.0% | 65.5% | 62.3% | 56.3% | 
| OSWorld-2.1 (offline subset) | 81.5% | 74.7% | 80.9% | 87.2% | 76.8% | 
| AutomationBench (Zapier) | 53.9% | 51.3% | 41.4% | 42.5% | 31.4% | 
| Vending-Bench 2 (final balance) | $16,484 | $13,718 | $15,515 | $9,235 | $5,422 | 
| GraphWalks 256k to 1M (BFS F1) | 96.2% | 84.2% | 71.8% | 66.8% | 65.0% | 
| RiemannBench | 80.0% | 76.0% | 72.0% | 69.6% | 65.6% | 
| Terminal-Bench-Science 0.1 | 64.8% | 52.9% | 63.3% | 59.0% | 43.3% | 
| SEC-bench Pro (finding) | 88.2% | 83.4% | 85.4% | n/a | n/a | 
| ExploitGym (exploiting) | 54.8% | 42.0% | 42.4% | 34.5% | n/a | 
| CWE-bench (patching) | 71.0% | 68.0% | 68.0% | 67.0% | 58.0% | 

The biggest single jump over Argon is GraphWalks, where Carbon scores 96.2% against Argon’s 84.2% and the best rival’s 71.8%. The cyber rows show the same pattern. On ExploitGym, Carbon’s 54.8% is 12.8 points above anything else on the card.

Google’s launch posts add rankings without scores:

- CursorBench 4.0, Perplexity’s WANDR and Zapier’s AutomationBench: Google says Carbon ranks #1 on all three at “more than 2X lower cost per task” than the next best external model, with no scores or named rivals published.
- WebDev Arena: 1768 Elo, ranked #4, up 89 points, per Google’s October 28, 2026 announcement.
- Chrome patch generation: “more than 10X better” than an unnamed lab’s frontier model at producing high-quality patches.
- Internal secure code generation: more than double Argon’s success rate.

Artificial Analysis is the one outside check available on launch day. Google’s materials put Carbon at 59 and #1 on the Artificial Analysis Intelligence Index v5.0. The evaluator’s live page on November 10, 2026 shows 60 on v5.0.1, ranked #4. Same model, newer index version, lower rank. The “#1” was true when Google wrote it.

The same evaluator runs the Multimodal Knowledge Agent (MKA) benchmark, where Claude Fable 5.2 scored 57%, Carbon (High) 54% and Claude Opus 5.5 53% on the Climb WSR headline metric as of November 10, 2026. Google’s own MKA post claims “#2 in quality” at 6x lower average cost than the top model. Two caveats apply. Google co-developed MKA with Artificial Analysis (200 private tasks, 10 verticals, 83 occupations, graded by an autorater built on a non-Google model), and some coverage reports Carbon as #1 on it. Both Google and the evaluator say #2.

## What Does Gemini 4 Carbon Actually Cost?

Gemini 4 Carbon costs $2.00 per million input tokens and $10.00 per million output tokens on the Standard API tier through November 30, 2026, then doubles to $4.00 and $20.00 on December 1, 2026. There is **no free tier**. Output pricing includes thinking tokens.

| Tier | Through Nov 30, 2026 (input / output) | From Dec 1, 2026 (input / output) | 
|---|---|---|
| Standard | $2.00 / $10.00 | $4.00 / $20.00 | 
| Batch and Flex | $1.00 / $5.00 | $2.00 / $10.00 | 
| Priority | $3.60 / $18.00 | $7.20 / $36.00 | 
| Cached input (Standard) | $0.10 | $0.20 | 

A few line items sit outside the table:

- Cached input: Google describes the $0.10 rate as 95% off, but Carbon supports implicit caching only, so you cannot pin a context and guarantee the discount.
- Grounding with Google Search: 5,000 free requests a month in 2026, shared across Gemini 3 and newer models, then $14 per 1,000 requests.
- Contributor tier: $1 input and $5 output, invite-only and in private preview.
- Argon parity: Carbon and Argon cost the same at every tier, and the launch post’s footnote says the December increase applies to the publicly accessible Carbon model.

So the sticker price carries no Carbon premium. The bill does.

Artificial Analysis measured Carbon (High) at $7.76 per Intelligence Index task in November 2026, ranking it #61 of 73 models on cost. Running the index took 205 million output tokens against a median of 72.8 million. The evaluator calls Carbon “somewhat expensive” and “very verbose,” and Google’s own November 10, 2026 post concedes it uses more tokens than Argon.

Thinking tokens are billed as output, and Carbon’s high thinking setting spends most of that budget on verification passes you never read. Anyone who has run a production agent knows the per-token rate is the small number. Loop count and thinking depth set the invoice. A side-by-side run against Argon on one real task, with token counts logged, tells you more than any price table.

Google’s “more than 2X lower cost per task” and the evaluator’s #61 of 73 ranking can both be true. They measure different task sets against different rivals. Neither is your invoice.

## What Changed From Gemini 3 to Gemini 4 Carbon

The jump from Gemini 3 to Gemini 4 Carbon comes down to four things: a single 1,048,576-token limit shared by input and output, a 65,536-token default output that stretches to the full million with long decode, three selectable thinking levels, and a verification budget far larger than its Argon sibling spends. The Gemini 4 family also drops the split pricing Gemini 3.1 Pro charged above 200,000 tokens.

The shared limit catches people out. Per the Gemini 4 Carbon model page in Google AI Studio, updated November 2026, the one-million figure covers prompt and response together. Stuff 900,000 tokens of PDFs into the context and you have roughly 148,000 left for the answer and every thinking token behind it. Inputs span text, image, video, audio and PDF. Output is text only.

Carbon’s additions over Argon are narrow and measurable (all Google-reported, October 2026):

- Whole-window reasoning: GraphWalks 256k to 1M rises from Argon’s 84.2% to Carbon’s 96.2%, a 12-point gain on tasks that traverse the entire context.
- Terminal work: Terminal-Bench 4.0 climbs from 57.1% to 65.2%, and Terminal-Bench-Science 0.1 from 52.9% to 64.8%.
- Cyber tuning: ExploitGym moves from 42.0% to 54.8%, and SEC-bench Pro from 83.4% to 88.2%.
- Thinking depth: identical architecture, training data and hardware, with more tokens spent per task on checking its own work.

Nothing in that list is a new modality or a new architecture. Carbon is Argon told to think longer, then tuned hard on security and long-horizon tasks. That’s a smaller generational story than the “Gemini 4” label suggests, and I’d rather you know it going in.

The pricing shape changed more than the headline numbers. Gemini 3.1 Pro Preview, still listed in November 2026, costs $2 input and $12 output up to 200,000 tokens and $4 and $18 above that. Carbon charges one flat rate across the whole million. Through November 30, 2026, a 500,000-token prompt costs half what 3.1 Pro charges for input. From December 1, Carbon’s $4 input matches 3.1 Pro’s long-context rate and its $20 output runs $2 higher. Gemini 3.8 Flash stays the cheap option at $0.75 and $3.75 through December 31, 2026, then $1.50 and $7.50.

One more change, reported by press at launch: Gemini 3.1 Pro left the Gemini app’s model picker when Carbon arrived. Google’s own launch pages don’t mention the removal.

## Where Gemini 4 Carbon Is Available and How to Access It via API, Vertex AI and the Gemini App

Gemini 4 Carbon is reachable as of November 10, 2026 through five routes: the paid Gemini API under the model ID `gemini-4-carbon`, Gemini for business, a Google AI Ultra subscription (Gemini app, AI Mode in Search, Antigravity), the Fairwind Program for cyber defenders, and a free six-month scientist programme. Vertex AI is the notable absence from Google’s launch pages.

**Gemini API, paid tier.** Build in AI Studio or Android Studio against `gemini-4-carbon`, listed as Stable. The model supports function calling, code execution, computer use, structured outputs, Search and Maps grounding, URL context, file search, and Batch, Flex and Priority inference. It does not support the Live API, image generation or audio generation. Caching is implicit only.

**Gemini for business.** Enterprises get Carbon through the API, the model drop-down, Antigravity, or CodeMender. Google’s November 10, 2026 post says “Gemini for business” and never names Vertex AI. Regional availability goes unstated too. If procurement runs through Vertex, confirm with your account team before anyone writes a migration plan.

**Google AI Ultra.** Subscribers get Carbon in the Gemini app, AI Mode in Search, and Antigravity’s app, CLI and IDE extensions. The Antigravity team’s November 10, 2026 post on Carbon previews `/swarm` there and warns that swarms burn quota much faster than a standard chat. No usage limits are published. An Ultra subscription covers the consumer surfaces and Antigravity. Production API traffic still bills on the paid tier.

**Fairwind Program.** Cyber defenders apply on a rolling basis through the DeepMind Fairwind Program page, which reports over 650 partners in 2026. Governments, critical-infrastructure operators and core technology platforms get priority. Applicants pass background checks, malware creation is prohibited, and access runs through a Cloud account team.

**Scientists.** Google opened applications in October 2026 to give 10,000 scientists six months of free Argon access through Antigravity’s highest usage tier, and on November 10 extended that to Carbon for current and future participants. Onboarding runs weekly, subject to eligibility, capacity and selection.

The friction most teams hit first is the quota question. Antigravity’s `/swarm` is the headline feature, and Google has published no ceiling for it.

## Which Workloads Gemini 4 Carbon Is Built For

Gemini 4 Carbon is built for four workloads: dense-document knowledge work with structured deliverables, multi-agent coding and 3D or UI builds, end-to-end vulnerability research, and scientific reasoning over raw experimental data. Google’s November 10, 2026 launch post backs each with named early testers.

**Dense documents and financial research.** Hebbia’s Shanks Salwan called Carbon the strongest model his team has evaluated for deep financial research, citing cleaner Excel models and sharper PowerPoint decks. Balyasny Asset Management ranked it top of 23 frontier configurations on its General Financial Intelligence benchmark in 2026, with Audrey Liu singling out charts, code and tables. Glean’s Thai Tran said it led every frontier model tested on task completion across workplace data. Box reported 100% on its public sector tasks. The common thread is retrieval over large, messy document sets, the same problem a production RAG pipeline exists to solve, with Carbon doing the synthesis on top.

**Multi-agent coding and 3D work.** OpenCode’s Kit Langton handed Carbon a requirements list at end of day and found a new Rust UI library and a rewritten desktop app the next morning. Cartwheel’s Andrew Carr ran it for six days with minimal steering on a tuned soft-body simulation and got gains in both speed and fidelity.

“Across 16 real bug fixes in our 14,000-test codebase, Carbon achieved a 94% resolution rate with zero regressions.” Baki Celtik, AI Engineer, HubX, November 2026


Google’s own demo built a 906,000-triangle reconstruction of its Platform 37 office from seven council planning PDFs across 10 overnight loops.

**Finding, exploiting and patching vulnerabilities.** Inside Google, Project Fortify reported 100% recall on scanning and 95% precision on triage in 2026, plus a 25% relative recall improvement over Argon on open-source repositories. Wiz’s Scan for Good team found zero-day vulnerabilities at two major SaaS providers in under two hours. SentinelOne’s Chris Corde said Carbon turned up four additional verified vulnerabilities in a kernel driver evaluation where every other model stopped at the same memory-corruption bugs. Chainguard, Armadin, Cisco and Fortinet gave similar early reports.

**Science.** Google’s AlphaProof Code agents used Carbon to generate Lean proofs of Signal’s cryptographic protocol code in weeks. Axiom Bio is moving its production toxicity analyses to Carbon and Argon after it surfaced novel mechanisms. On 7 low-homolog CASP17 protein families, Carbon retrieved 40-fold more homologs on average, with broader benchmarking still underway.

Every one of these is a long task with a checkable result. That’s the pattern.

## Where Gemini 4 Carbon Still Falls Short

Gemini 4 Carbon falls short in four places: it loses to rivals on two agentic benchmarks and sits second on MKA, it is slow and verbose per task, it skips several API features developers expect in 2026, and nearly every headline number comes from Google itself.

Start with the scoreboard. On Google’s own October 2026 model card, GPT-6 Astra beats Carbon on FrontierSWE 2 (65.5% to 61.2%) and Claude Opus 5.5 beats it on OSWorld-2.1 (87.2% to 81.5%). On the Multimodal Knowledge Agent benchmark, Claude Fable 5.2 holds first at 57% against Carbon’s 54% as of November 10, 2026. Two of those three tests cover the exact work Google is selling Carbon for: real software engineering and computer use.

Then there’s speed. Artificial Analysis’s November 2026 measurement of Carbon (High) put time to first answer token at 66.85 seconds against a 12.15-second median across models, with throughput near 80 tokens per second. More than a minute of silence before the first word. For an interactive assistant that is disqualifying. For an overnight agent it is simply the cost of the verification loops.

The gaps developers trip on first:

- No Live API, so no real-time voice or streaming multimodal sessions.
- No image or audio generation, since output is text only.
- Implicit caching only, with no way to pin a context and guarantee the discount.
- No free tier, so every experiment bills from the first token.

Read the comparisons with care. The model card’s Claude column on Terminal-Bench 4.0 is Fable 5.1, while the live MKA leaderboard already includes Fable 5.2. The cyber rows reflect Fairwind Program access run “without guardrails,” a configuration no public API customer gets. And the one outside evaluator with launch-day numbers co-built MKA with Google.

The safety results are strong, and Google attaches its own asterisk. All figures below are Google-reported, November 2026:

| Safety measure (2026) | Carbon | Argon | 
|---|---|---|
| Attack trajectories blocked (adaptive red-team agent) | 99% | n/a | 
| Per-attempt breach rate | 0.8% | 3.1% | 
| Gray Swan automated cyberattack success | 2.17% | 9.86% | 
| Gray Swan indirect prompt injection success | 0% | 0.7% | 
| 375-task agentic honesty test, pass rate | 92% | 70% | 

Trajectory Labs added an external result: zero new universal jailbreaks across 132 hours and more than 13,000 requests. Terminal-Bench 4.0 cheating attempts ran at 0.4%, with none succeeding. In the same November 10, 2026 post, Google says prompt injection defence “remains an area for continued improvement” and that its own automated red teaming still finds attack distributions that get through.

One thing I’d want before a regulated deployment. The Carbon model card reports no Frontier Safety Framework critical capability results and defers to a separate Safety and Security report. Argon’s October 2026 card said Google “cannot rule out” that Argon reaches the cybersecurity critical capability level. Carbon reasons harder than Argon on exactly that axis.

## Who Should Switch to Gemini 4 Carbon and Who Should Wait

Security teams, deep-research groups and anyone running agents over a full million-token context should switch to Gemini 4 Carbon now, teams doing routine engineering and knowledge work should stay on Argon, and anyone who cares about latency or the December 1, 2026 price doubling should wait for a second round of independent numbers.

**Switch now** if you fit one of three profiles:

- Security teams inside the Fairwind Program, where Carbon’s 2026 lead on finding, exploiting and patching vulnerabilities is widest and the model is the whole point of the programme.
- Deep-research and long-horizon coding teams whose tasks run for hours or overnight, produce a checkable artifact and can absorb a minute of silence before the first token.
- Workloads that traverse the entire context window, such as dependency or graph analysis across hundreds of thousands of tokens, where Carbon’s GraphWalks margin over every rival is the largest on its card.

**Stay on Argon** for everything else. Google says so itself, both models cost the same per token, and Argon spends far fewer of them. A chat assistant, a document classifier, a code-review bot that answers in seconds: none of those improves with a 66-second thinking pause in front of it.

**Wait** if any of these describe you:

- Your users sit in front of the model and notice latency.
- Your budget was built on the $2/$10 rate and nobody has modelled the $4/$20 bill that starts December 1, 2026.
- Your procurement needs independent benchmark coverage, which on November 10, 2026 amounts to one evaluator that co-built one of the tests.
- You buy through Vertex AI and Google has yet to confirm Carbon there.

Here’s the part I’d push on. Price parity through November 30 makes the trial almost free. Take the hardest task on your backlog, run it through both models with token counts logged, and let the bill and the output decide. Three weeks is enough.

## Frequently Asked Questions About Gemini 4 Carbon

### Is Gemini 4 Carbon free to use?

No. The Gemini API has no free tier for Carbon, and the paid rate is $2 per million input tokens and $10 per million output tokens through November 30, 2026, doubling to $4 and $20 on December 1. Consumers reach it through a Google AI Ultra subscription. Eligible scientists can apply for six months of free access in Antigravity, and cyber defenders can apply to the Fairwind Program.

### What is the Gemini 4 Carbon context window?

Gemini 4 Carbon has a 1,048,576-token context window that input and output share, per Google’s model page updated November 2026. Default maximum output is 65,536 tokens, extendable to the full 1,048,576 with long decode. Thinking tokens count against that output.

### Gemini 4 Carbon vs Argon: which should I use?

Use Argon for most software engineering and knowledge work, and Carbon when a task needs long verification loops: vulnerability research, whole-context graph traversal, or bug fixes that run for days. The two share one architecture, one training set and one per-token price in 2026. Carbon spends more tokens per task, so it costs more per job even at the same rate.

### When does the $2/$10 introductory price end?

The introductory price ends November 30, 2026. From December 1, 2026, Standard costs $4 input and $20 output per million tokens, Batch and Flex cost $2 and $10, Priority costs $7.20 and $36, and cached input rises from $0.10 to $0.20.

### Does Gemini 4 Carbon support the Live API or image generation?

No. Google’s November 2026 model page lists the Live API, image generation and audio generation as unsupported. Carbon accepts text, image, video, audio and PDF input and returns text only. It does support function calling, code execution, computer use, structured outputs, Search and Maps grounding, URL context and file search.

### What is the Fairwind Program?

The Fairwind Program is Google DeepMind’s access channel for cybersecurity defenders, and it received Carbon on October 28, 2026, two weeks before general availability. Applications are rolling, applicants pass background checks, malware creation is prohibited, and governments, critical-infrastructure operators and core technology platforms get priority. The programme reported over 650 partners in 2026, with access handled through a Cloud account team.

### Is Gemini 4 Carbon available on Vertex AI?

Google’s November 10, 2026 launch materials list the Gemini API in AI Studio and “Gemini for business” without naming Vertex AI, and they give no regional availability. Treat Vertex access as unconfirmed and ask your Google Cloud account team before planning a migration.

### What is Gemini 4 Carbon’s knowledge cutoff?

March 2026, according to the Gemini 4 Carbon model card published November 10, 2026. Anything that happened after that date has to be supplied in the prompt or retrieved through grounding.

## Where to Start With Gemini 4 Carbon

Start with Gemini 4 Carbon by picking the one entry point that matches your role, then running a single hard task through both Carbon and Argon before November 30, 2026, while the two models still cost the same per token.

The entry point is a short decision:

1. Developers: create a paid API key in AI Studio and call `gemini-4-carbon` directly.
2. Individuals and small teams: subscribe to Google AI Ultra for the Gemini app, AI Mode and Antigravity.
3. Cyber defenders: apply to the Fairwind Program and expect background checks.
4. Researchers: apply for the scientist programme’s six months of free Antigravity access.

Then test. Pick the ugliest task on your backlog, the one that runs for hours and ends in something you can check. Run it on both models with thinking set to high, log every token, and compare the outputs and the bills side by side. Three weeks of price parity is a narrow window.

Before you migrate anything, read the model card and Google’s updated Gemini 4 developer guide, which covers moving existing workflows onto the new models.
