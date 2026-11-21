---
id: collect-261121-veille-ia/gemini-google/google-launches-gemini-4-neon-with-opus-5-5-level-coding-performance-at-25-lower-api-token-prices
title: "Google launches Gemini 4 Neon with Opus 5.5-level coding performance at 25% lower API token prices"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["neon", "google", "gemini", "customer", "opus", "model", "said", "googles"]
source: https://venturebeat.com/technology/google-launches-gemini-4-neon-with-opus-5-5-level-coding-performance-at-25-lower-api-token-prices
source_anchor: ""
source_lines: [1, 172]
section: "Gemini_Google"
---
Google's new mid-ranged AI large language model is here: Gemini 4 Neon launched earlier today on the paid Gemini API in Google AI Studio and Android Studio, as well as in the Gemini app and AI-assisted development platform Google Antigravity for Google AI Pro and Ultra subscribers.

In a welcome surprise, Neon is not only highly performant and fast, but affordable, too, especially compared to similarly scoring proprietary models.

“In our early evaluations, it matched or exceeded Gemini 4 Carbon on overall quality at 3x the speed and 60% of the cost,” said Thai Tran, Glean’s AI product lead, in Google’s announcement. However, Google’s launch post gives no absolute completion times in seconds or minutes for a matched Neon and Carbon workload. Its clearest comparison on that measure comes from a customer.

Neon costs $3 per million input tokens and $15 per million output tokens. That puts its headline rates 50% *higher* those of Anthropic’s mid-range Claude Sonnet 5.5 and OpenAI’s mid-range GPT-6.2 Sol, but Google reports that Neon beats both on the Artificial Analysis Intelligence Index at its highest reasoning setting. 

More impressively, Neon costs 25% below Anthropic's larger, flagship Opus 5.5’s standard rates of $4 and $20 despite scoring within about a percentage point on that larger model on two engineering benchmarks.

## **Where Gemini 4 Neon shows promise at work**

Google and early users highlight several jobs for Gemini 4 Neon: tracing an unexpected charge across billing records, finding flaws in unfamiliar software, testing research hypotheses and building spreadsheets that colleagues can inspect and change. These tasks share a demand that a straightforward question rarely makes of an AI model. Neon has to follow evidence through several steps and produce something another person can use, whether that means identifying a questionable charge or handing over a working file.

Take a billing dispute. Google suggests asking Neon to examine utility statements and explain why a charge rose. In a related example, a customer can ask it to cross-check receipts and other records when seeking a refund. Neon can examine PDFs, spreadsheet entries and photographs together, so the customer can give it the original records without first transcribing everything into a single format. Google offers these as examples of what customers can try. It gives no success rate for detecting billing errors or recovering money.

Developers describe a similar role for Neon when they inherit software. Before an engineer can fix a defect, someone has to trace how the application works and narrow the search to the code that could explain it. Thoughtful Lab founder Karina Nguyen said Neon helped her team understand complex software architectures quickly. In the firm’s security evaluations, she said, it also spotted subtle vulnerabilities that other models missed. Cartwheel co-founder and chief scientist Andrew Carr described using Neon in machine-learning research, where it examined prior work, tested hypotheses and incorporated feedback as an experiment progressed.

A Google demonstration in Sheets illustrates what Neon can produce when the assignment calls for a working artifact. Google gave a group of Neon agents a single image. The agents reconstructed it across 100,000 spreadsheet cells and used more than 500,000 formulas to identify edges, map depth and change the lighting. They accomplished that without embedding the original picture or writing a script. The practical detail for a business user is what happens after the handoff: a colleague can open the sheet, inspect the formulas and change how the result looks.

Taken together, these examples make the strongest case for Neon on assignments where people can check its work. A customer can compare its explanation with the original bill, an engineer can investigate a flagged defect and an analyst can follow a spreadsheet formula back to its inputs. Google’s head-to-head tests also show why that scrutiny matters. Neon outscored GPT-6.2 Sol on business automation, while Sol read dense professional charts more accurately. A team choosing a model for a weekly report may find that moving information through several applications and reading the crucial figure correctly are two different jobs.

## **Where Neon lands against Sol, Sonnet and Google’s own models**

Google puts Neon between Hydrogen, its small, light, fast workhorse, and Carbon, the model it recommends for its hardest extended projects.

That hierarchy only partly predicts the results. At its highest setting, Neon edges past Carbon on the general intelligence index. On the professional tasks measured by Vals, all three Google models finish within about 2.5 percentage points of one another.

| **Model** | **Input / cached input / output per million tokens** | **Artificial Analysis Intelligence Index (effort)** | **Vals Index** | 
|---|---|---|---|
| **Gemini 4 Neon** | **$3 / $0.15 / $15** | **61 (xhigh)** | **70.9%** | 
| Claude Opus 5.5 | $4 / $0.20 / $20 | 59 (rounded) | 70.14% | 
| GPT-6.2 Sol | $2 / $0.10 / $10 | 57 (max) | 66.06% | 
| Claude Sonnet 5.5 | $2 / $0.10 / $10 | 55 (max, default fallback) | 70.85% | 
| Gemini 4 Hydrogen | $1.50 / $0.075 / $7.50 | 54 (xhigh) | 68.49% | 
| Gemini 4 Carbon | $2 / $0.10 / $10 through Nov. 30, then $4 / $0.20 / $20 | 60 (high) | 70.45% | 

Google supplies Neon’s Vals result. Artificial Analysis confirmed on X that Neon placed third on its intelligence index.

The peer results come from the Artificial Analysis leaderboard and the Vals Index, whose public snapshot predates Neon’s release.

Reasoning settings apply to the Artificial Analysis column. Providers define those settings separately, so two models running at maximum effort can spend very different amounts of time and money on a question.

The price comparison also changes with the calendar and the workload. Carbon’s introductory rate makes it cheaper per token than Neon through November.

Beginning December 1, Neon’s input and output rates are 25% below Carbon’s scheduled rates. And under OpenAI’s long-context pricing, a Sol request exceeding 272,000 input tokens costs $4 per million input tokens and $15 per million output tokens for the whole request. At that length, Sol loses its headline output-price advantage.

| **Model** | **Input ($/1M)** | **Output ($/1M)** | **Total ($/1M)** | **Source** | 
|---|---|---|---|---|
| Muse Spark 1.2 / 1.3 Contributor | $0.10 | $0.20 | $0.30 | Meta | 
| MiMo-V2.6-Flash | $0.14 | $0.28 | $0.42 | Xiaomi | 
| GPT-6 Luna | $0.10 | $0.50 | $0.60 | OpenAI | 
| Muse Ember — ≤200K input tokens | $0.10 | $0.50 | $0.60 | Meta | 
| Claude Haiku 5.5 — ≤100K prompt tokens | $0.10 | $0.50 | $0.60 | Anthropic | 
| DeepSeek-V4.1-Flash-1017 — off-peak | $0.15 | $0.60 | $0.75 | DeepSeek | 
| MiniMax-M3.1-Flash | $0.15 | $0.60 | $0.75 | MiniMax | 
| MiMo-V2.6-Pro | $0.435 | $0.87 | $1.305 | Xiaomi | 
| MiniMax-M3 | $0.30 | $1.20 | $1.50 | MiniMax | 
| LongCat-2.5 | $0.30 | $1.20 | $1.50 | LongCat / Vercel | 
| DeepSeek-V4.1-Flash-1017 — peak hours | $0.30 | $1.20 | $1.50 | DeepSeek | 
| Muse Ember — >200K input tokens | $0.30 | $1.50 | $1.80 | Meta | 
| Claude Haiku 5.5 — >100K prompt tokens | $0.50 | $2.50 | $3.00 | Anthropic | 
| Muse Spark 1.1 / 1.2 / 1.3 | $1.25 | $4.25 | $5.50 | Meta | 
| Muse Spark Open — Together AI serverless | $1.25 | $4.25 | $5.50 | Together AI | 
| GLM-5.3 | $1.40 | $4.40 | $5.80 | Z.AI | 
| Grok 4.7 — ≤200K prompt tokens | $2.00 | $6.00 | $8.00 | xAI | 
| Qwen3.8-Max | $2.00 | $6.00 | $8.00 | QwenCloud | 
| Gemini 4 Hydrogen | $1.50 | $7.50 | $9.00 |  | 
| GPT-6 Sol — Standard, ≤272K input | $2.00 | $10.00 | $12.00 | OpenAI | 
| GPT-6.2 Sol — Standard, ≤272K input | $2.00 | $10.00 | $12.00 | OpenAI | 
| Claude Sonnet 5.5 | $2.00 | $10.00 | $12.00 | Anthropic | 
| Gemini 4 Argon — through Nov. 30, 2026 | $2.00 | $10.00 | $12.00 |  | 
| Kimi K3.1 Pro | $2.00 | $10.00 | $12.00 | Moonshot AI | 
| Gemini 4 Carbon — through Nov. 30, 2026 | $2.00 | $10.00 | $12.00 |  | 
| Grok 4.7 — >200K prompt tokens | $4.00 | $12.00 | $16.00 | xAI | 
| Grok 4.7 Fast (Cursor) — ≤256K input | $4.00 | $12.00 | $16.00 | Cursor | 
| GPT-5.4 | $2.50 | $15.00 | $17.50 | OpenAI | 
| **Gemini 4 Neon** | **$3.00** | **$15.00** | **$18.00** | **Google** | 
| GPT-6 Sol — Standard, >272K input | $4.00 | $15.00 | $19.00 | OpenAI | 
| GPT-6.2 Sol — Standard, >272K input | $4.00 | $15.00 | $19.00 | OpenAI | 
| Claude Opus 5.5 | $4.00 | $20.00 | $24.00 | Anthropic | 
| Grok 4.7 Fast (Cursor) — >256K input, up to 500K | $6.00 | $18.00 | $24.00 | Cursor | 
| Gemini 4 Argon — starting Dec. 1, 2026 | $4.00 | $20.00 | $24.00 |  | 
| Muse Wonder 1.0 | $4.00 | $20.00 | $24.00 | Meta | 
| Gemini 4 Carbon — starting Dec. 1, 2026 | $4.00 | $20.00 | $24.00 |  | 
| GPT-6 Sol — Fast, ≤272K input | $4.00 | $20.00 | $24.00 | OpenAI | 
| GPT-6.2 Sol — Fast, ≤272K input | $4.00 | $20.00 | $24.00 | OpenAI | 
| Sakana Fugu Ultra (≤272K) | $5.00 | $30.00 | $35.00 | Sakana AI | 
| GPT-6 Sol — Fast, >272K input | $8.00 | $30.00 | $38.00 | OpenAI | 
| GPT-6.2 Sol — Fast, >272K input | $8.00 | $30.00 | $38.00 | OpenAI | 
| Claude Opus 5.5 — Fast mode | $8.00 | $40.00 | $48.00 | Anthropic | 
| Claude Fable 5.2 / Claude Mythos 5.2 | $8.00 | $40.00 | $48.00 | Anthropic | 
| GPT-6.2 Astra — Standard, ≤272K input | $8.00 | $40.00 | $48.00 | OpenAI | 
| Claude Fable 5.1 / Claude Mythos 5.1 | $10.00 | $50.00 | $60.00 | Anthropic | 
| GPT-6 Astra — Standard, ≤272K input | $10.00 | $50.00 | $60.00 | OpenAI | 
| GPT-6.2 Astra — Standard, >272K input | $16.00 | $60.00 | $76.00 | OpenAI | 
| GPT-6 Astra — Standard, >272K input | $20.00 | $75.00 | $95.00 | OpenAI | 
| GPT-6.2 Astra — Fast, ≤272K input | $16.00 | $80.00 | $96.00 | OpenAI | 
| GPT-6 Cyber | $20.00 | $100.00 | $120.00 | OpenAI | 
| GPT-6 Astra — Fast, ≤272K input | $20.00 | $100.00 | $120.00 |  | 

## **Professional work reveals a different contest**

The Vals evaluation combines finance, software, legal and tax tasks, weighting the sectors by their contribution to the U.S. economy. Google puts Neon second overall at 70.9% and first on both legal research and tax agent tasks.

The aggregate matters less for a law firm than the legal result, but for a buyer looking across departments, the published scores give little reason to distinguish Neon from Sonnet on overall accuracy alone.

Sol offers a different bargain. Vals lists Sol at $3.18 per test, against $16.18 for Sonnet. Sol trails Sonnet by nearly five percentage points, yet the two have identical standard short-context input and output prices.

The difference illustrates how much the model’s behavior during a job can change its cost. Google’s announcement does not give an equivalent Vals dollar figure for Neon, leaving an important part of that three-way comparison open.

## **How much faster is Neon than Carbon?**

Once it starts generating output, Neon streams about **110 tokens per second** at xhigh, according to the launch-day reading from benchmark tracker Priors. 

Artificial Analysis lists Carbon at **59 tokens per second** on high, its highest setting. Those readings put Neon about 51 tokens per second ahead, or roughly **1.9X faster than Carbon’s output rate**. 

That **places Neon close to** **Gemini 3.8 Flash** **at 116 tokens/second on high**. Hydrogen still has the advantage for rapid output at 167 on high and 171 on xhigh.

A fast stream can still take a while to start. Priors lists about 57 seconds to the first token for Neon at xhigh. A developer waiting on a repository fix also has to account for how long the agent spends reading files, running tests and revising its patch.

Google separately reports a 32% reduction in cost per task against Carbon on the Artificial Analysis index at Neon’s default high setting. At xhigh, where Neon earned its 61, Artificial Analysis reported that the cost advantage narrowed to about 17% against Carbon running at high.

## **In coding, the software around the model can triple its score**

On Cognition’s FrontierCode 2.0, which grades code changes against criteria set by reviewers, Google reports 46.2% for Neon at default high effort. Opus scored 47.3% in Google’s comparison, a 1.1-point lead.

For context, Artificial Analysis results compiled by The Aggregate put Sol at 44.38% and Sonnet at 41.7%, both at max. Those figures give developers a sense of the competitive range. The evaluations use different configurations, and Google’s own results show just how consequential that can be.

On a separate coding evaluation called SWE-sweep, Google reports 12.8% with the mini-SWE agent software and 39.0% with Antigravity. The underlying model stayed the same.

In Antigravity, Neon can divide a project among multiple Hydrogen agents working in parallel. For an engineering team running its own agent software, the threefold spread is a stronger reason to test Neon inside that environment than any single leaderboard rank.

## **On X, early reactions turn on what Google chose to measure**

“It's cheaper, faster, and better behaved than Carbon,” Varun Mohan wrote on X on launch day, describing his experience using Neon internally.

Omar Sanseviero, Lead of Developer Experience at Google DeepMind, singled out a shorter wait for the first token, a difference a developer can feel before an agent finishes the job.

Google DeepMind technical staffer Logan Kilpatrick acknowledged that Google was having “a little too much fun with naming” and shared a guide with a practical distinction for subscribers: Pro users can select Neon in the Gemini app, while Carbon requires Ultra.

Outside Google, the X account @synthwavedd highlighted Neon’s lead over Opus 5.5 and Sol on most rows of Google’s chart. “I’m impressed DeepMind,” the account wrote.

The post also described Neon as cheaper than both rivals, a claim that needs a qualifier for Sol given its lower standard token rates. The chart measures performance, and the cost comparisons depend on which job the agents run.

The benchmark chart shared by DeepMind co-founder Demis Hassabis shows where that enthusiasm has the most support. On Zapier’s AutomationBench, Neon scored 56.3%, against 42.5% for Opus and 41.3% for Sol.

On Terminal-Bench Science, a test a Google spokesperson highlighted in an email to VentureBeat, Neon reached 70.5%, ahead of Sol at 68.0% and Opus at 63.3%. The order flipped on other tasks. Opus edged Neon 65.8% to 65.2% on Terminal-Bench 4.1, while Sol led Neon 75.1% to 73.5% on Chartography, a test of how well models read complex charts.

## **Google’s frontier comeback moves beyond the leaderboard**

Seven weeks ago, Google introduced Gemini 4 Argon with results that put it ahead of GPT-6 Astra and Claude Opus 5.5 on 12 of the 18 tests Google chose to publish.

Most developers could only read the results. Google initially restricted access to approved cybersecurity partners. Carbon followed on October 28, taking first place on the newly revised Artificial Analysis Intelligence Index under a similar access restriction. By the time Google opened Carbon to paying customers on November 10, Anthropic’s Claude Fable 5.2 had already overtaken it.

Google now has an answer for three different budgets: Hydrogen for high-volume work, Neon for harder jobs that recur often, and Carbon for extended projects. The lasting part of Google’s comeback may be that customers can keep more of those jobs on one platform even as leadership on individual tests changes hands.

The timing also gives Google an opening with developers weighing OpenAI’s new GPT-6.2 Astra and Sol. As VentureBeat reported a day before Neon’s launch, Every CEO Dan Shipper found that Astra sometimes stopped early on end-to-end builds despite matching its predecessor on most of his team’s coding tests. Wharton’s Ethan Mollick got Astra to build a playable space elevator but found it difficult to steer.

Those observations concern Astra in particular. Sol remains a cheaper rival to Neon, and the early reactions point to something a buyer can actually test across all three: how much intervention it takes to turn a promising first attempt into a finished piece of work.

## **Which enterprise workloads justify the premium?**

Neon’s clearest practical distinction may be the material it can take in. Google says its million-token window can hold a mix of video, audio, charts, spreadsheets and PDFs. Consider an analyst investigating a billing dispute who has a recorded call, the contract and a ledger.

Neon can take all three forms of evidence together. Sol’s API supports text and image inputs and a 1.05-million-token context window, but a team using it would need another way to process the recording. Sonnet also offers a million-token window, so capacity alone does little to distinguish the three.

For security teams, Google is splitting the rollout. A specialized cyber version will reach eligible Fairwind Program participants in the coming days. Google reports 90.5% on a vulnerability-finding evaluation and 70.7% on a patching test for that version. Enterprises can request access through their Google Cloud account team.

For buyers choosing a default model, the arithmetic sets a useful threshold. At standard short-context rates and the same mix of input and output, Neon would need to use roughly one-third fewer tokens than Sol or Sonnet to erase its 50% price premium.

A workflow that finishes sooner or requires less human review could still justify the difference. Glean has reported that kind of gain against Carbon. Whether Neon earns the same place against Sol or Sonnet depends on what a team spends to get a finished, usable piece of work.
