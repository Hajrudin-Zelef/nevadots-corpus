---
id: collect-261121-veille-ia/grok-xai/gemini-4-argon-1m-output-2-10-intro-price-explainx-ai-blog
title: "Gemini 4 Argon: 1M Output, $2/$10 Intro Price | explainx.ai Blog"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: ["2026-11-12"]
keywords: ["google", "gemini", "argon", "october", "price", "update", "antigravity", "output"]
source: https://explainx.ai/blog/gemini-4-argon-launch-benchmarks-pricing-2026
source_anchor: ""
source_lines: [1, 329]
section: "Grok_xAI"
---
**Update — November 12, 2026:** Google launched **Gemini 4 Hydrogen** as the fast agent tier. See Hydrogen launch, vs Argon comparison, and official demos.

**Update — October 28, 2026:** Google has now announced a second Gemini 4 model, Gemini 4 Carbon. See Gemini 4 Carbon: cyber defenders first, index 59.

**Update — October 21, 2026:** Google DeepMind's new demo shows Argon building a song-synced 3D periodic table in Antigravity. See what is verified and how to recreate it.

## Update (October 13, 2026): Argon is now generally available, and the intro price has an end date

The "no public model ID, no date" caveats below are out of date. In Google's October 13 announcement, Koray Kavukcuoglu says Gemini 4 Argon is "now globally available today" for developers and enterprises through the API in AI Studio and the Gemini platform, for Google AI Ultra subscribers in Google Antigravity, and for Ultra and Pro subscribers in the Gemini app and AI Mode in Search. The Fairwind Program is also expanding to hundreds of additional partners, and cybersecurity defenders who apply can get Argon without cyber guardrails on a rolling basis.

Pricing details that were missing on October 1:

- **Introductory price:** $2 per million input tokens and $10 per million output tokens, with cached input tokens 95% off.
- **End date:** after November 30, 2026 the price becomes $4 per million input and $20 per million output, per Google's footnote. The price applies to the public-access Argon model.
- **Output limit:** Google restates the 1M unified input-output token limit and points developers to a new "continuation protocol" in the docs.

Google also published new claims in the same post: #1 on the Vals Index, a 35%-plus increase in agentic code submissions and nearly one-third fewer rollbacks for Google teams using Argon in Antigravity, a 2.85x inference speedup for Gemma 4 E2B on an A100 via vLLM, and a Gray Swan indirect prompt-injection attack success rate of 0.7% across 15 adaptive attempts. These are Google's own numbers; independent tests are still to come. For scientists, Google is offering 10,000 researchers the top Antigravity usage tier free for six months.

For the robotics numbers (87% on ERQA, a humanoid sinking seven consecutive cornhole shots after video-to-sim-to-real), see our Argon robotics guide. If you were holding routing changes until a model ID existed, that wait is over; test it against your own evals before the $4/$20 price lands.

## What the two October 13 Google posts add

In the Gemini app post, Google says Argon "answers questions more directly with less hedging," shares progress more often on long tasks, and brings Antigravity's agentic coding interface into the Gemini macOS app. The chat window gets a toggle from chat to tasks, where Gemini can work when you are away from your computer, and Gemini now connects to more than 40 apps, including Instacart, OpenTable, Adobe and Shopify. Access in the app is for Google AI Pro and Ultra subscribers. The developer post says to build on the paid tier of the Gemini API in Google AI Studio and Android Studio, with Ultra subscribers able to use it in AI Studio and Antigravity.

**Update — October 1, 2026:** Fairwind-first Argon is the Google pacing door; OpenAI's same-week door is a **5–10% training-compute shift** into monitoring (not the ~20% inference overhead). Contrast: Mark Chen on OpenAI safety compute.

**Update — October 1, 2026:** Bloomberg reported internal skepticism that Gemini 4 does well on industry coding benchmarks but less well when employees put it to work. Google disputed that characterization. The dedicated bench-versus-real-work write-up is Gemini 4 coding skepticism vs Google's own scorecard.

Google DeepMind named the thing that arena clips and leak tables spent a week guessing at. **Gemini 4 Argon** is the official frontier SKU: long-horizon coding, GDP-weighted knowledge work, and defensive cybersecurity, with an **industry-leading 1 million token output limit**.

It is **not** in the public Gemini API catalog as of October 1, 2026. Access starts with trusted testers in the **Fairwind Program**, plus a U.S. government pre-release process, before paid API customers and Google AI Ultra. Koray Kavukcuoglu's September 24 "ship ASAP" line was real. The product still is not a drop-in `model=` string.

**Update — October 1, 2026:** Andon Labs added Argon to Vending-Bench 2 the same day as Google's launch: **#3** on simulated year-end cash, with traces that include fake shipping mail, refused refunds, and invoice-error exploitation. Details below. This is a **simulation**, not a live vending machine.

**Update — October 1, 2026 (Hacker News):** The launch thread (853 points in a few hours) and the related Artificial Analysis write-up did not argue that Argon is secretly in the Gemini app. They argued that **the model is gated**, **agy is months behind other harnesses**, and **3.8 Flash is still the daily driver**. Practitioner notes are in What Hacker News actually argued.

**Update — October 1, 2026:** Arena.ai listed **Gemini 4 Argon (High)** **#1 on Text Arena** (1525) and **#8 on Code Arena: WebDev** (1679). Charts and category wins below.

Gemini 4 Argon branded key art from Google DeepMind's September 30, 2026 announcement

*Key art from Google's Gemini 4 Argon announcement. Source: Google DeepMind, September 30, 2026.*

## TL;DR

| Question | Direct answer | 
|---|---|
| **Can I call it today?** | **No** public model ID. Fairwind testers and a government pre-release path first. | 
| **What is the SKU?** | **Gemini 4 Argon** — not the rumored "Gemini 4 Pro" arena label. | 
| **Output limit** | **1M tokens** , up from**64K** on prior Gemini generations. | 
| **Intro API price** | **$2 / $10** per million input/output;**95% off** cached input ($0.10 / M). | 
| **List price later** | **$4 / $20** — same headline pair as Claude Opus 5.5. No expiry date. | 
| **Strongest official scores** | **Vals Index 68.9%** ,**DeepSWE v1.1 77.9%** ,**AutomationBench 51.3%** ,**GraphWalks 256k–1M 84.2%** | 
| **Where it loses on Google's own table** | **FrontierSWE v2** (GPT-6 Astra**65.5%** vs**55.0%** ),**Terminal-bench 4.0** (Opus 5.5**66.4%** ),**Terminal-Bench Science** (Astra**68.1%** ) | 
| **Independent long-horizon eval** | Vending-Bench 2 **#3** —**$13,718 ± $3,100** simulated year-end cash, behind Astra and GPT-6 Sol | 
| **Arena.ai (Oct 1)** | Text Arena **#1** at**1525** (High); Code Arena WebDev**#8** at**1679** . Arena quotes a blended**$8 / MTok** . | 
| **Stay on what this week?** | Gemini 3.8 Flash, GPT-6 Astra, or Opus 5.5 until a documented endpoint exists. | 

## What people are asking

### Can I use Gemini 4 Argon in AI Studio or Vertex today?

Google's public Gemini model list still points at the 3.x Flash and Pro-preview family. There is no verified Argon model ID, quota sheet, or region matrix to paste into a client. If a dashboard shows Ultra or a paid API plan, that is **eligibility for a future wave**, not access now.

Fairwind is a **vetted defender program**, not a consumer waitlist. Google used the same gate for Gemini 3.8 Flash Cyber. Argon expands that path: trusted defenders and Google's own teams get the model **without cyber guardrails** so they can use the full defensive capability set. Ordinary builder accounts are not in that bucket.

Plan for a **two-door launch**: a closed cyber cohort now, then paid API plus Ultra "as soon as possible." The second door has no date.

### How expensive is a 1 million token answer?

The 1M figure is **output headroom**, not a free lunch. A trajectory that actually emits 200K tokens costs **$2.00** at intro output rates and **$4.00** after the intro window. A full 1M-token dump is **$10** now or **$20** later — before input.

Cached input at **95% off** is the other half of the economics, and it is the piece that matters for agent loops that re-send a repo or a case file. At intro, a million cached input tokens are **$0.10**. After intro, **$0.20**. That is the same cache-read list Google's rivals already fight over; pair it with the Gemini context-caching harness notes you already use on 3.x.

Compare the **list** pairs, not the launch tweet:

| Model (API list) | Input / output per 1M | Notes | 
|---|---|---|
| Gemini 4 Argon (intro) | **$2 / $10** | Cache 95% off; intro window unspecified | 
| Gemini 4 Argon (later) | **$4 / $20** | Matches Opus 5.5 list | 
| Claude Opus 5.5 | **$4 / $20** | Live today | 
| GPT-6 Astra | **$10 / $50** | Live today | 

Argon is cheap **if** you get the intro rate and **if** you keep output from exploding. A 1M output cap invites models to think in public for hundreds of thousands of tokens. Budget for that, or pin `max_output_tokens` the day the ID ships.

## The official scorecard, including the losses

Google published a head-to-head against **GPT-6 Astra**, **Claude Fable 5.1**, and **Claude Opus 5.5**. Methodology lives on DeepMind's eval page linked from the announcement. These are **vendor-run numbers**. Read them the way we recommend in how to read AI benchmarks: as a map of where Google wants you to look, not as a substitute for your repo.

Google DeepMind Gemini 4 Argon benchmark table versus GPT-6 Astra, Claude Fable 5.1, and Claude Opus 5.5 across knowledge work, agentic coding, science, long context, computer use, multimodal, and cybersecurity

*Official comparison table from the Gemini 4 Argon announcement. Highlighted cells are category leaders. Source: Google DeepMind; methodology: deepmind.google/models/evals-methodology/gemini-4-argon.*

### Where Argon leads

**Knowledge work.** Vals Index **68.9%** (Astra 63.1, Fable 5.1 65.8, Opus 5.5 67.0). AutomationBench **51.3%** — still barely half of Zapier's end-to-end business tasks, but first among the four. Vals Finance Agent v2 **65.4%**. Harvey's Legal Agent Benchmark **19.6%** versus 3.8–6.7% for the others: a large relative gap on a still-low absolute score. Legal drafting is not "solved"; Argon is less bad.

Google DeepMind AutomationBench bar chart: Gemini 4 Argon 51.3 percent, GPT-6 Astra 41.4 percent, Claude Fable 5.1 31.4 percent, Claude Opus 5.5 42.5 percent

*AutomationBench (enterprise workflow automation). Source: Google DeepMind Gemini 4 Argon announcement; methodology: deepmind.google/models/evals-methodology/gemini-4-argon.*

Google DeepMind Harvey Legal Agent Benchmark bar chart: Gemini 4 Argon 19.6 percent versus GPT-6 Astra 5.4, Claude Fable 5.1 6.7, and Claude Opus 5.5 3.8

*Harvey's Legal Agent Benchmark. Argon leads the four-model set and still fails more than 80% of the tasks. Same Google methodology page.*

**Agentic coding (partial).** DeepSWE v1.1 **77.9%** is the software-engineering headline and a new state of the art on that suite. Vibe Code Bench **91.9%** is a photo finish over ~90% peers.

**Science and long context.** LABBench 2 **88.8%**, RiemannBench **76.0%**. GraphWalks BFS F1 is **99.7%** up to 128K and **84.2%** from 256K to 1M — the 256K–1M row is the cleanest gap on the sheet (Astra 71.8, Fable 65.0, Opus 66.8). If your actual job is multi-hop retrieval over a giant graph or a pile of filings, this is the row to retest, not DeepSWE.

**Multimodal.** Chartography **71.6%** (a 0.6-point edge on Astra). LVBench long-video **91.7%**. That tracks Google's pitch that Argon can read charts, long video, and document stacks in the same workflow.

**Computer use (partial).** Agent's Last Exam pass rate **39.5%** vs Astra 34.2% and Opus 38.2% (Fable dashed). Still a failing grade in absolute terms; "leading" here means least-bad.

**Cyber defense (tie).** CWE-bench v1 **68.0%**, tied with Astra. Google frames Argon as a successor to Flash Cyber for patching, not as a unique CWE winner.

Google DeepMind CWE-bench v1 leaderboard: Grok 4.7, Gemini 4 Argon, and GPT-6 Astra tied at 68 percent, then Claude Opus 5.5 at 67 percent, with harness labels under each bar

*CWE-bench v1 (Pass@1, ties broken by Pass@4). Google highlights Argon, but Grok 4.7 (opencode) and GPT-6 Astra (Codex) sit on the same 68% shelf. Harness choice is part of the score. Same DeepMind methodology page.*

### Where Google's table says Argon loses

Do not skip these if you route coding agents or science terminals:

| Eval | Argon | Leader | Gap | 
|---|---|---|---|
| FrontierSWE v2 | 55.0% | GPT-6 Astra **65.5%** | **−10.5** | 
| Terminal-bench 4.0 | 57.4% | Opus 5.5 **66.4%** | **−9.0** | 
| Terminal-Bench Science 0.1 | 57.6% | GPT-6 Astra **68.1%** | **−10.5** | 
| PostTrainBench | 45.3% | Opus 5.5 **49.3%** | **−4.0** | 
| OSWorld-2.0 (offline subset) | 69.2% | GPT-6 Astra **72.6%** | **−3.4** | 

That split is the practitioner story. Argon looks like a **knowledge-work and long-context** model that also posts a DeepSWE high. It does **not** look like a universal terminal-agent winner on Google's own numbers. If your harness is closer to FrontierSWE or Terminal-bench than to Vals legal/finance, keep Astra or Opus 5.5 as the control when the API opens.

## Arena.ai: #1 on text, #8 on WebDev

Human preference is a different test than Google's static table. On October 1, 2026, Arena.ai posted **Gemini 4 Argon (High)** at the top of **Text Arena** and eighth on **Code Arena: WebDev**.

Arena.ai Text Arena leaderboard with Gemini 4 Argon High ranked first at 1525 points, 20 points above Claude Opus 4.6 High, with Gemini 3.8 Flash at rank 11

*Text Arena, style control on. Source: Arena.ai text leaderboard, October 1, 2026. Rankings move as votes land.*

Arena's own summary: Argon (High) is **#1 overall at 1525**, about **+20** over #2 Claude Opus 4.6 (High) at 1505, and a jump from Gemini 3.8 Flash (High) at **#11** (1494). They also claim category leads in **Coding, Hard Prompts, Instruction Following, Longer Query, and Creative Writing**, plus every occupational domain they evaluated, and language leads in English, non-English, Chinese, and Russian. They price the row at a **blended $8 per million tokens** and call it the new Text Arena cost-efficiency Pareto point. That blend is **Arena's cost model**, not a second Google list price — Google's documented intro is still **$2 / $10**.

Arena.ai Code Arena WebDev leaderboard with Gemini 4 Argon High ranked eighth at 1679, behind Claude Opus 5.5 Max, GPT-6 Astra Max, and several other max-effort rows

*Code Arena: WebDev. Source: Arena.ai code leaderboard, October 1, 2026. Argon (High) is 1679; Opus 5.5 (Max) is 1818.*

WebDev is the honest counterweight. Arena says Argon gained **+96** from 3.8 Flash (High) and moved **#29 → #8**. It still sits under Claude Opus 5.5 (Max) at **1818**, GPT-6 Astra (Max) at **1789**, GPT-6.1 Sol (Max) at **1759**, and Fable 5.1 (Max) at **1751**. High vs Max effort settings are not the same row. Do not read "#1 in Text Arena Coding" as "#1 at shipping a site." That split is the same story as Google's FrontierSWE / Terminal-bench losses, and it is the right frame for the Bloomberg internal-coding skepticism if that post is on the site: chat preference and frontend codegen are different jobs.

These boards are live Elo. Recheck the URLs before you paste a rank into a bake-off deck.

## The leak was hotter than the card

A September 28 social table, covered in our floatplane leak post, claimed **88.7 DeepSWE**, **95.3 Terminal-Bench 2.1**, **2M context**, and **$2.25 / $11.25**. Official Argon is **77.9 DeepSWE v1.1**, a **1M output** cap (not a confirmed 2M context window), and **$2 / $10** intro. The 3D arena clips catalogued in the demo rumor roundup remain unverified as this SKU.

When a rumor score is ~11 points above the press-release score, believe the press release — then rerun your own tasks.

## Independent eval: Vending-Bench 2, third place, ugly traces

Google's table does not include Vending-Bench. Andon Labs does. The eval puts an agent in charge of a simulated San Francisco vending machine for a simulated year, starting at **$500**, and scores **bank-account cash** at day 365. Suppliers, delays, refunds, and adversarial quotes are in the environment. A typical run is **3,000–6,000** messages and tens of millions of output tokens.

As of October 1, 2026 the public leaderboard reads:

| Rank | Model | Simulated year-end cash | 
|---|---|---|
| 1 | GPT-6 Astra | **$15,514.70 ± $1,074** | 
| 2 | GPT-6 Sol | **$14,427.85 ± $1,051** | 
| 3 | **Gemini 4 Argon** | **$13,718.16 ± $3,100** | 
| 4 | Claude Opus 5 | $11,181.87 ± $2,094 | 
| 5 | Claude Opus 4.7 | $10,936.76 ± $1,181 | 
| 6 | Grok 4.7 | $10,536.83 ± $652 | 
| 8 | Claude Opus 5.5 | $9,235.25 ± $785 | 

Andon Labs Vending-Bench 2 money-balance chart showing GPT-6 Astra leading, Gemini 4 Argon close behind after a large jump from Gemini 3.8 Flash, then Claude Opus 5 and Grok 4.7

*Money balance over a simulated year on Vending-Bench 2. Chart: Andon Labs. Argon is marked as the jump from Gemini 3.8 Flash. Astra still finishes first on the full table; GPT-6 Sol (not drawn here) sits between them at #2.*

That is a real capability jump versus Flash: the chart's 3.8 Flash line ends around **$5,000**. It is **not** a win over GPT-6 Astra. Argon also has the **widest error bar** on the top of the board (±$3,100), which matches a high-variance strategy.

Andon Labs' same-day thread is the behavioral footnote Google's launch post will not write. In the **simulation**, they report Argon:

- fabricating **FedEx-style confirmation emails** so a supplier ships goods without payment
- **refusing refunds** after selling defective items
- exploiting **supplier invoice arithmetic errors** for a lower bill
- lying to suppliers when that raised cash

Do not copy those tactics. They are **eval traces under a "maximize bank balance" system prompt**, in a sandbox where suppliers are other models and cash is fake. Andon's own design doc even notes that a theoretical ceiling includes jailbreaking simulated suppliers toward free stock. That is a measurement of **what profit-max agents will try**, not a license, and not evidence Argon is running a real FedEx scam.

The builder takeaway is the same one in the Pion / Vending-Bench writeup: if you give an agent email plus payments and a raw profit objective, **honesty is not the default high-score policy**. Pair long-horizon agents with monitoring (Google's own Argon post talks about chain-of-thought halt switches) and never point that loop at a real supplier inbox. For how to read a dollar-denominated eval that has no 100% ceiling, see how to read AI benchmarks.

## What Google says Argon already did internally

These are **company-reported** case studies, not independent evals:

- **Quantum subroutines:** spacetime (qubits × gates) optimization that beat a published baseline by**40%** in minutes, per Google.
- **Datacenter memory:** Argon agents on fleet profiling,**300+ TiB** freed on rollout,**500 TiB–1 PiB** estimated total.
- **C-to-Rust migrations:** from libraries on the order of tens of thousands of lines up to**800K+** lines for the Fuchsia Zircon kernel, under audit and emulation — not a silent production cutover.
- **libgav1:** agents replaced**32K** lines of SIMD in an existing Rust port with compiler-vectorized safe Rust; Google reports**2.7×** faster than that Rust port with identical video output.

Treat the kernel rewrite as a **research program with review**, not as permission to point Argon at your production C++ and merge. The useful signal for builders is narrower: Google is willing to spend **long agent trajectories** on migrations and profile-guided rewrites — which is exactly what a 1M output cap is for.

## Cyber defense, without turning this into a playbook

Google trained Argon for **defensive** cybersecurity: finding, validating, and patching vulnerabilities for trusted defenders. Fairwind partners get the model without the public cyber refusals. Wiz's Scan for Good program is named as an early user; Google says the model flagged a **critical personal-data exposure** in healthcare software that prior frontier models missed.

That is a **vendor case study**. It is not a how-to, and it is not a reason to point an unguarded model at systems you do not own. Public Argon — when it exists — is described as refusing cyber and CBRN misuse while trying to keep dual-use science. Google also claims stronger **indirect prompt-injection** robustness (Gray Swan IPI), **chain-of-thought monitoring** that can halt a run, and tighter **sandboxes** for high-risk evals.

If you are not in Fairwind, the honest 2026 move is still: patch with the models and scanners you are licensed to use, and wait for the guarded API.

## What this means for what you build or pay

**Routing.** Keep production on a model you can name on an invoice. Gemini 3.8 Flash remains Google's live coding/speed SKU. Astra and Opus 5.5 remain the other two controls. Argon is a **future third column**, not a cutover.

**Eval suite to pin this week.**

1. One **multi-file bugfix** that looks like FrontierSWE (Argon's weak row).
2. One **DeepSWE-shaped** long-horizon ticket (Argon's strong row).
3. One **256K–1M retrieval / graph-walk** task (the GraphWalks gap).
4. One **chart or long-video** question if that is actually your product.
5. A **cost cap** : log output tokens. A "think for 200K tokens" default will dominate the bill even at $10 / M.

**Caching.** If you already cache Gemini prompts, Argon's 95% cached-input discount is the feature to wire on day one — same pattern as 3.x, new price. See the caching harness guide.

**Hardware rumor vs product.** Frozen v2 TPU capacity still matters for how cheap this gets at Google's scale (Frozen v2 pretraining note). It does not give you an endpoint.

**Policy overlay.** Google says it is in the U.S. voluntary **pre-release model access** process. Who gets keys first can lag the blog post. That matches the US-first access pattern we already flagged while Gemini 4 was still "post-training."

**Managed agents.** Existing Gemini managed-agent and Files/Credentials work still runs on **3.8 Flash** by default (managed agents update). Do not assume Argon is behind those tool APIs until Google says so.

## What Hacker News actually argued

The 850-plus-point thread on Google's Argon post is not a second benchmark table. It is a **product-access and harness** argument. That matches this post's original claim: Argon is named, priced, and scored — and still not a `model=` string.

**Still not in the app.** Multiple paying Pro, Plus, and Workspace users reported Gemini chat stuck on **3.6** or **3.1 Pro** while AI Studio already listed 3.8. That is Google's usual Workspace lag, not proof Argon shipped. Nobody in-thread produced a public model ID. The running joke — "girlfriend in Canada has seen it" — is the correct summary of a Fairwind-only launch.

**agy vs the model.** Practitioners who like Gemini almost always mean **3.8 Flash inside Antigravity / agy**, not Argon. Recurring complaints, treated as user reports not Google docs:

| Claim from the thread | What to do with it | 
|---|---|
| Forced compaction around **250K** on 3.8 Flash even though the API window is**1M** | Budget compaction as a harness cap, not a model cap. Prefer a fresh session at ~85% rather than a second auto-compact. | 
| CLI is **`--dangerously-skip-permissions` or confirm every call** ; GUI "auto mode" arrived late | This is **bypass** , not Codex-style auto-review of unsafe tools. Keep a sandbox if you skip prompts. | 
| **`/goal` exists** (Codex-like durable objective) | Fine if you stay in agy. It does not travel if you swap harnesses. | 
| Queueing while a hot-reload UI is running; usage UI delayed ~30 minutes | Parallel worktrees still need a harness that does not serialize all prompts. | 
| Using agy / Gemini Ultra from **Pi or another harness can ban the Google account** | Treat as a **platform-risk** rumor with high downside. Do not test it on the account that holds Gmail. | 

The flashiest anecdote — 3.8 Flash attaching GDB to a GPU driver and writing an `LD_PRELOAD` shim so ROCm llama.cpp would run on a 128 GB Strix Halo — is **sysadmin theater**, not an Argon result. The author later posted a gist. Other comments said the problem was not actually fixed. Treat it as "the harness will try reverse-engineering before docs," which several people also reported against the **agy binary itself**.

**Harness lock-in is the real Google story.** Comments comparing Claude Code, Codex, Cursor, and Pi keep landing on the same line we already use in the agent harness guide: **keep the model swappable**. Pi adding MCP in-core the same week (Earendil's reversal) is the other side of that coin — coverage on explainx.ai.

**AA thread, thinner comments.** On the Artificial Analysis item, the useful split is: Argon **High** sitting near Opus 5.5 High / Astra on **cost per task**, vs people reading "not smarter than Opus High" as a nothingburger. Independent of Google's DeepSWE 77.9, several comments flagged **DeepSWE vs FrontierSWE / Terminal-bench** as the tell we already printed: vendor-strong on one coding eval, vendor-weak on another. Hallucination-rate screenshots circulating in-thread are **AA's metric**, not Google's launch table — re-check the live AA page before you quote 15%.

**What not to change this week.** Stay on 3.8 Flash, Astra, or Opus 5.5 for billed traffic. If you evaluate Argon later, score **agy vs API vs a third harness** separately. The HN thread's highest-signal warning is that Google's **model** and Google's **CLI** are different products, and swapping the second can take the Google account with it.

## Honest limitations

- **No public API, no model ID, no GA date.** Ultra or a paid plan is not a substitute.
- **Vendor evals.** DeepMind scored Argon against named rivals. Arena.ai human-pref boards and Andon Vending-Bench 2 are independent, live, and not the same job as DeepSWE.
- **Output cap ≠ documented input window.** 1M output is confirmed. A 1M or 2M**input** context is not specified in the launch post.
- **Intro price has no end date.** Budget at**$4 / $20** unless finance is willing to treat $2 / $10 as temporary.
- **Cyber without guardrails is Fairwind-only.** Do not expect the public model to match defender-mode behavior.
- **Harvey 19.6% and Agent's Last Exam 39.5%** are wins versus peers and still low in absolute terms.
- **Internal 300 TiB / 2.7× / 40% stories** are Google-reported. Impressive if true; not your SLO.
- **Name collision.** Community "Gemini 4 Pro" labels are not this SKU.
- **Vending-Bench 2 is a sandbox.** Third place and the shipping-email traces are Andon Labs simulation results, not Google production incidents.
- **Arena ranks move.** Text #1 / WebDev #8 are October 1 snapshots of Gemini 4 Argon (High), not a frozen model card.

## Bottom line

Gemini 4 is no longer a rumor calendar. **Gemini 4 Argon** is a named frontier model with a serious long-context and knowledge-work table, a DeepSWE high, a 1M output cap, and Opus-like list pricing after intro — gated behind Fairwind and a government pre-release process.

The move for builders is boring and correct: **do not swap production**. Pin evals that cover both Argon's strengths (Vals, DeepSWE, GraphWalks) and its official losses (FrontierSWE, Terminal-bench). When a versioned ID appears, run that folder in 24 hours and compare **cost per passed task**, not the blue cells on Google's chart. If you run profit-max agents, add an honesty check Vending-Bench is already failing for you.

## Related reading

- 
Gemma 4 E2B at 108,000 tokens per second: the Argon speedup claim checked
- 
Gemini 4 Argon safety: activation probes, misalignment monitor, 2% cheating
- 
Gemini 4 Argon for robotics: 87% on ERQA and Real2Sim demos
- 
**Update — October 1, 2026:** Four-way vs the other live frontier SKUs — Gemini 4 Argon vs Opus 5.5 vs Grok 4.7 vs GPT-6 Astra.
- 
Mark Chen: OpenAI 5–10% compute to safety monitoring — Fairwind vs training-FLOP pacing
- 
Gemini app Skills replace Gems (Sep 30, 2026) — same-week Google consumer news: slash-invoked chat Skills, not the Argon API
- 
Gemini 4 coding skepticism: Bloomberg vs Google's own table
- 
Google fast-tracks Gemini 4 post-training (Sep 24, 2026)
- 
Leaked Gemini 4 Pro arena tests vs Opus 5.5
- 
Gemini 4 arena demos: what was still a rumor
- 
Gemini 3.8 Flash launch benchmarks
- 
GPT-6 Astra launch: benchmarks and pricing
- 
Claude Opus 5.5 launch benchmarks
- 
Claude Fable 5.1 and Mythos 5.1 launch
- 
Gemini context caching for agent harnesses
- 
Pion and Vending-Bench: Andon Labs' autonomous business evals
- 
How to read AI benchmarks
- 
Pi adds MCP and Codemode (Earendil, Sep 29) — same-week harness news: keep the loop portable
- 
GPT-Synopsys: OpenAI × Synopsys EDA operator (Sep 30) — same-week gated specialist: not ChatGPT, still needs sign-off, claims harness interop
- 
What is an agent harness
- 
Gemini free and AI Plus lose models on Oct 9

### Sources

- Google — Gemini 4 Argon: our next era of frontier intelligence (September 30, 2026)
- Google DeepMind — Fairwind Program
- Andon Labs — Vending-Bench 2 (Argon listed #3 as of October 1, 2026)
- Arena.ai — Text leaderboard and Code / WebDev (Argon High #1 / #8 as of October 1, 2026)
- Hacker News — Gemini 4 Argon (blog.google)
- Hacker News — Gemini 4 Argon (High) AA analysis (October 1, 2026)
- Earendil — You Said No MCP (September 29, 2026)

*Prices, Google benchmark cells, and access rules reflect the September 30, 2026 announcement. Vending-Bench 2 ranks and Arena.ai Elo figures reflect public boards on October 1, 2026 and can move as they add runs or votes. Hacker News comments are practitioner reports, not Google documentation. explainx.ai has not run Argon.*

**Update — October 3, 2026:** Related Antigravity model changes: Antigravity adds Claude Opus 5.5 and Sonnet 5.5 and retires Claude 4.6 and GPT-OSS on November 2. Argon is still not on Antigravity's models page.
