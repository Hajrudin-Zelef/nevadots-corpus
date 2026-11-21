---
id: collect-261121-veille-ia/gemini-google/ai-daily-digest-november-1-2026-google-folds-jules-into-antigravity-claude-ultra-drops-the-5-hour-cl
title: "AI Daily Digest November 1, 2026: Google Folds Jules into Antigravity, Claude Ultra Drops the 5-Hour Clock, an Agent Breaks into Chainalysis"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["jules", "google", "antigravity", "november", "ultra", "claude", "hour", "coding"]
source: https://dev.to/hiroki-ii-ai/ai-daily-digest-november-1-2026-google-folds-jules-into-antigravity-claude-ultra-drops-the-3e5e
source_anchor: ""
source_lines: [1, 71]
section: "Gemini_Google"
---
November opens with consolidation on several fronts at once. Google is retiring one of its best-known coding agents to concentrate everything behind a single platform. Anthropic is reshuffling its subscription ladder and, separately, describing a monitoring mechanism that has been running silently inside Claude for four months. The five largest AI builders now carry more than three trillion dollars in future spending commitments between them, and a security disclosure put agentic AI on the attacker's side of the table. Here are the seven stories that mattered.

Google announced on October 29 that Jules, its asynchronous coding agent, will wind down by the end of the year. Sessions stop accepting new work after November 11, when the product turns read-only, and the service goes offline entirely on December 31. Jules launched in December 2024 as an experiment in background coding: you connected a GitHub repository, it cloned the codebase into a cloud virtual machine, and it fixed bugs, wrote tests, updated dependencies, and opened pull requests while you did something else.

The official explanation is focus, not failure. Google said the lessons from Jules are being folded into Antigravity, the agent-first development platform it introduced alongside Gemini 3 in November 2025 and upgraded to version 2.0 at I/O this year. The goal is one place that offers both what Jules did best, autonomous execution of tasks in a remote environment, and what Antigravity does best, a development environment built around agents that plan, edit, run commands, and check their own work.

There is an exit ramp but no automatic migration. Export is already available and covers account settings, environment variables, scheduled tasks, connected repositories, saved memories, setup scripts, and the full history of every session, including prompts, plans, activity logs, and code patches. The code itself was never the problem, since branches and pull requests already live on GitHub. What only existed inside Jules, the memories and session context, has to be carried out by hand before the deadline.

The same day, Antigravity launched a Plugin Marketplace for the app and the CLI. Each plugin bundles up to five primitives, skills, MCP servers, rules, hooks, and custom agents, into a one-click install, with launch partners including Atlassian for Jira, Confluence, and Bitbucket, plus Figma, Stripe, Datadog, Databricks, Canva, CrowdStrike, Dropbox, and Unity. What Google has not said is when Jules-style background tasks actually arrive in Antigravity, or whether saved sessions will migrate automatically. Teams that built routines on Jules have weeks, not months, to rebuild them.

— Google · The Verge

Anthropic renamed its consumer subscription tiers on Saturday. The $100 plan, previously Max 5x, is now simply Max. The $200 plan, previously Max 20x, is now Ultra. Prices and benefits stay the same and existing subscribers migrate automatically, which leaves Claude with a four-tier ladder: Free, Pro at $17 a month, Max, and Ultra.

The substantive change sits on top of the renaming. Anthropic is removing the 5-hour rolling session limit for verified Ultra accounts, so a long coding session can now run until the weekly allowance runs out instead of stalling mid-task every few hours. Most existing subscribers already qualify. New accounts keep a temporary limit until verification completes, which runs automatically on account and payment history, and users can confirm their status in Settings under Usage, where a verified account no longer shows a current-session row. The weekly ceiling itself has not moved. It stays at ten times the Pro allowance, so the total amount of work per week is unchanged. What changed is the shape of access: instead of rationing the week into 5-hour windows, Anthropic now lets subscribers spend their quota continuously.

Subscription product lead Michael Overell explained the reasoning and apologized in the announcement thread. The 20x name made sense in April 2025, when the top tier had only a 5-hour window worth twenty times a Pro user's quota. After weekly caps arrived, the window stayed at 20x but the week was capped at 10x, and the name kept implying both were 20x. Dropping the number was the cheapest way to end the confusion.

The competitive frame is hard to miss. A day earlier OpenAI doubled the usage allowances on its Pro tiers for Codex and ChatGPT Work, and the ChatGPT lead showed up in the replies to suggest a 28-day reset window would be better still. All of this lands weeks before Anthropic's expected IPO, with a Financial Times report the same day saying bond investors have been told to expect a BB rating, the top of speculative grade, after listing. Quota generosity is becoming a front in the subscriber war.

— Anthropic · 智东西

Anthropic published a description of a safety mechanism it calls self-evaluation, and the notable part is how long it has been live. Since the June release of Claude Fable 5, every newer Claude model has, after answering a user, received one extra instruction the user never sees: review this conversation for safety. The same model that just wrote the answer then reuses its cached context and emits a small JSON verdict, roughly ten tokens long, stating whether the exchange was harmful, what category it falls into, how urgently a human should review it, and whether it had refused.

The cost engineering is the reason this scales. A conventional design would send the whole transcript to a separate, heavier classifier, and re-reading long conversations is expensive. Here the prefill work is almost zero because the audit instruction is all that gets processed fresh, and the decode step generates about ten tokens. Anthropic measured the overhead at under two percent of total inference compute on long contexts, and the cost sits with the platform rather than the user's bill.

The reported numbers are specific. On Fable 5, at a one percent false-positive rate, the self-review caught 78 percent of the violations that a heavier classifier flags, and detection of high-risk abuse reached 94 percent. Recall scales with model size: the smallest of four model sizes caught around 16 percent while the largest reached 96 percent under the same false-positive budget. Anthropic is also explicit about the boundary. The verdicts do not block anything in real time. They feed offline aggregation and human review, which is a monitoring system, not a censor in the loop.

Transparency arrived from a second direction the same week. An email screenshot published on October 28 showed that text watermarking, which newer Claude models already carry, extends to Mythos 5, Opus 4.7, Opus 4.6, and Sonnet 4.6 on November 3, ahead of the December 2 compliance deadline in the EU AI Act. Users can neither check the watermark themselves nor turn it off. One company, two invisible layers on every conversation, and both now publicly documented.

— Anthropic · 凤凰网科技

Bloomberg totted up the fine print this week and got a number that dwarfs the capex headlines. Across Alphabet, Meta, Microsoft, Amazon, and Oracle, disclosed commitments for leases, construction, energy, and equipment purchases tied to the AI buildout now exceed three trillion dollars. The figure comes from regulatory filings and the latest earnings reports, with Microsoft's contribution drawn from its annual report through the end of June, the most recent disclosure available.

Alphabet alone accounts for more than a trillion dollars in contractual commitments, close to eight times the level of a year ago. Meta's future spending tops 800 billion, and nearly half of it sits in data center leases that have not even started yet, with payment schedules stretching up to thirty years. CFO Susan Li told analysts the company expects to keep investing heavily to support the core business and emerging AI projects. Summed across the five companies, future lease payments run to about 1.3 trillion and other purchase commitments to about 1.9 trillion.

The caveats are as instructive as the totals. The disclosures are not standardized, so the comparison is rough. Some of Meta's number covers consumer hardware in Reality Labs, while Alphabet and Amazon include content licensing. None of it counts existing leases or debt already raised. And the filings land in a market that has started flinching: the Nasdaq fell almost 3 percent on Friday for a third straight weekly loss, the Philadelphia semiconductor index dropped 7.2 percent, and Intel suffered its worst single-day decline in over four decades. Oracle, the first giant to go free-cash-flow negative on data centers, told investors it expects to return to positive free cash flow by 2028. The commitments are real, and so is the question of who pays for them.

— SEC filings · Bloomberg

NVIDIA released Nemotron 3.5 Transcribe as an open-weight model on October 28, and it reads less like another ASR checkpoint and more like a compression of an entire speech stack. One model handles transcription in 40 languages, translation between 31 of them and English, speaker attribution in 13 languages with up to eight speakers, word-level timestamps, and audio question answering, on recordings up to an hour long. The architecture pairs a speech encoder with a hybrid Mamba-Transformer mixture-of-experts language model, 30 billion parameters total with about 4.3 billion active per token.

The benchmark sheet backs the single-checkpoint claim. On the Hugging Face Open ASR Leaderboard it leads all open-weight models in English at a 5.33 percent average word error rate, and it posts the lowest open-weight numbers on the multilingual and long-form tracks as well. On Artificial Analysis it ties for first among open models at 2.8 percent word error rate while activating roughly 4 billion parameters against the 24 billion used by the model it ties. Speaker separation is where it pulls furthest ahead: the lowest diarization error rate among 14 systems on David AI's 21-language benchmark, at 18.26 percent. On one RTX PRO 6000 Blackwell GPU with NVFP4 quantization, it processes 100 hours of audio in under three minutes.

The data side may matter more than the model. Alongside the weights, NVIDIA opened Granary v2, a curation pipeline and dataset manifest covering 463,062 hours of audio across 48 languages, nearly double the language coverage of the first version. Fine-tuning recipes ship with it, and a medical case study shows what adaptation buys: on recordings of Indian doctors, word error rate for held-out speakers fell from 22.44 to 12.17 percent, a 45.8 percent relative reduction, for about 14 GPU-hours on a single GB300.

The license is OpenMDW 1.1, the model runs on a single GPU from a Jetson AGX Thor to a Blackwell server, and quantized versions for RTX Spark laptops are promised. Two limits are worth noting before deploying: it works on recorded audio, not live streams, and the official guidance still asks users to spot-check names and speaker changes against the recording.

— NVIDIA · Hugging Face

Aeon, Japan's largest retailer, moved a distribution hub in Kawaguchi, Saitama into full robotic operation on October 30. The Kawaguchi-Higashi XD center supplies about 200 My Basket convenience-style supermarkets in the Tokyo area, and the site now runs more than 90 robots, including 82 automated guided vehicles that ferry individual items between zones. Sensors identify incoming cases and place them on conveyors, and robot arms stack outgoing goods onto racks organized by destination store.

The part that separates this from a typical warehouse automation story is what the robots decide for themselves. Instead of repeating pre-programmed motions, the arms work out stacking order and placement based on each item's weight and shape, so loads arrive without collapsing. Aeon says this is the first time the full flow, from sorting products to loading them store by store, has been automated end to end. The control software comes from Mujin, a Japanese robotics company that has specialized in letting machines handle objects without teaching them each motion in advance.

The economics are the reason to watch this one. About half of the hub's work is now automated, cutting labor needs by roughly 39 people a day, and Aeon puts the robots' productivity at three to four times that of human workers. The company targets an 80 percent automation rate by 2030 and plans to extend the approach to logistics sites serving its supermarkets and drugstores. Japan's logistics industry is short tens of thousands of workers, and this hub is a working answer to a question the whole sector is asking: which parts of a supply chain can run without waiting for a hire that will not come.

— AEON · Mujin

🔗 AEON

Chainalysis, the blockchain analytics firm that helps trace stolen crypto, disclosed on October 30 that it was itself the target of a security incident, and the wording is the story. The company said it detected an external actor using offensive agentic techniques to gain unauthorized access to its systems and internal tools. That is one of the first times a breached vendor has put agentic AI on the intrusion method line of an official disclosure, rather than describing a familiar exploit chain.

The blast radius, per the company's own account, is contact-level data. User IDs, full names, email addresses, and organizational role labels were accessed for users provisioned on the platform. Chainalysis found no evidence that investigation content such as graphs, notes, or cases, transaction or query results, cleartext passwords, MFA secrets, or session tokens were touched. It also stated plainly that the incident is unrelated to the recent theft at Binance, a case in which Chainalysis was among the firms attributing the attack to North Korea with high confidence, a detail that makes the disclaimers feel personal.

The response reads like a preview of how security teams will work from here. Chainalysis engaged outside cybersecurity experts and said it is using large language models to support a broader review of its platform's security. AI on both sides of the same incident, attackers chaining multi-step access with agents and defenders running model-assisted audits, in a single disclosure. The company advised customers to enable two-factor authentication and to treat unexpected password resets, authentication prompts, and anyone offering fund recovery or recruiting out of the blue as hostile. It also fit a rough week: the same coverage cycle carried news of a malicious Claude Code extension distributed through Cursor's marketplace that installed persistent SSH backdoors.

— Chainalysis · Bleeping Computer

KD Agentic · AI Daily Digest

## Top comments (0)
