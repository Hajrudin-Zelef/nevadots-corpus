---
id: collect-261121-veille-ia/claude-anthropic/claude-fable-5-2-review-what-changed-from-fable-5-1-what-it-costs-and-who-can-use-mythos-5-2
title: "Claude Fable 5.2 review: what changed from Fable 5.1, what it costs, and who can use Mythos 5.2"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-11-09"]
keywords: ["fable", "claude", "opus", "against", "anthropics", "mythos", "november", "costs"]
source: https://medium.com/@mcthkayaa/claude-fable-5-2-review-what-changed-from-fable-5-1-what-it-costs-and-who-can-use-mythos-5-2-888180931c37
source_anchor: ""
source_lines: [1, 184]
section: "Claude_Anthropic"
---
# Claude Fable 5.2 review: what changed from Fable 5.1, what it costs, and who can use Mythos 5.2

Anthropic released Claude Fable 5.2 on 9 November 2026 at $8 input and $40 output per million tokens. Anthropic says Claude Mythos 5.2 is the same underlying model with more permissive safeguards, for verified programmes only. Artificial Analysis’s Intelligence Index v5.0.1, read 9 November, scores Fable 5.2 at High 61.82 for $9.08 per task, against 59.11 for $10.04 for Claude Opus 5.5 at Max. Anthropic still advises starting with Opus 5.5, and its headline examples have no outside check.

We sent no prompt to Claude Fable 5.2 or Claude Mythos 5.2. The page rests on Anthropic’s launch announcement, its 264-page system card, its docs, four outside boards (Artificial Analysis, Cognition, Zapier and Vals AI) and five launch-day user reports, all read on 9 November 2026.

Anthropic’s launch table is checked cell by cell against each benchmark owner’s board. Artificial Analysis is now on Intelligence Index v5.0.1, which regraded CritPt in November 2026, so its figures differ from pages quoting older versions. Our arithmetic is labelled as ours, and Anthropic’s own advice is to start most coding and knowledge work on Claude Opus 5.5.

## What changed from Claude Fable 5.1 to Claude Fable 5.2?

Claude Fable 5.2 lists at $8 / $40 per million input and output tokens against $10 / $50 for Claude Fable 5.1, which Anthropic still lists, read 9 November 2026. Anthropic’s announcement adds “up to 30% faster” output, “about 55% fewer blocks”, split as “about 60% fewer in cybersecurity and 40% fewer in biology”, and a table in which Fable 5.2 beats Fable 5.1 on every printed row; the system card has no speed or block-rate measurement, and its ProgramBench result puts Fable 5.2 at 85.6% against 87.6% for Fable 5.1 [p.216].

Anthropic’s block figure comes from what it calls “early testing across claude.ai, Claude Code, and Cowork”.

### Is Claude Fable 5.2 cheaper than Claude Fable 5.1?

Per token, Claude Fable 5.2 is cheaper than Claude Fable 5.1: input, output, cache reads ($0.20 against $0.25) and batch ($4 / $20 against $5 / $25) are all 20% lower, our arithmetic from Anthropic’s API pricing page, read 9 November 2026. Per task it depends on effort: Artificial Analysis’s Intelligence Index v5.0.1 lists Fable 5.1 (High) at 47.17 for $7.54 per index task and Fable 5.2 (High) at 61.82 for $9.08, while Fable 5.2 (Medium) costs $3.80 for 56.41. Artificial Analysis now marks Fable 5.1 deprecated.

### Is Claude Fable 5.2 really up to 30% faster?

No published measurement backs Anthropic’s claim that Claude Fable 5.2 is “up to 30% faster than Claude Fable 5.1 and Claude Mythos 5.1”: the figure appears only in the announcement, and the system card’s one serving-speed line says speeds in its evaluations “may differ from what external users experience” [p.221]. Fast mode is separate: Anthropic’s fast-mode docs give “Up to 2.5x higher output tokens per second compared to standard speed” at $16 / $80, twice the standard price, and “not time to first token (TTFT)”. No launch-day user report timed Fable 5.2’s speed.

## Is Claude Fable 5.2 worth twice the price of Claude Opus 5.5?

Only at High on hard tasks: Claude Fable 5.2 (High) scores 61.82 for $9.08 per index task against 59.11 for $10.04 for Claude Opus 5.5 (Max) on Artificial Analysis’s Intelligence Index v5.0.1, read 9 November 2026, while at Medium Opus 5.5 is cheaper (48.21 for $2.45 against 56.41 for $3.80). Per token, Fable 5.2 costs twice as much as Opus 5.5 ($8 / $40 against $4 / $20 per million, our arithmetic).

### What does Anthropic itself recommend?

Anthropic recommends Claude Opus 5.5 first: “For most coding and knowledge tasks, we recommend that users start with Claude Opus 5.5, our strongest daily driver”. Its cost case for Fable 5.2 rests on one FrontierCode 2.0 chart: “At High effort, Fable 5.2 scores about the same as Opus 5.5’s best score (45.3% vs. 45.5%) for roughly half the cost per task”.

The half holds on Anthropic’s cost axis ($6.721 against $12.82 per task); Cognition’s own data file gives $8.402 against $12.821, about two thirds (our arithmetic). The card adds that Fable 5.2’s max-effort 45.7% would be 44.4% if 22 timed-out attempts counted as zero [p.209].

### What does cost per task say on the Artificial Analysis Intelligence Index v5.0.1?

Claude Fable 5.2 holds the top three rows of Artificial Analysis’s Intelligence Index v5.0.1, read 9 November 2026: 64.56 at Max for $26.54 per index task, 62.22 at Xhigh for $14.83 and 61.82 at High for $9.08.

- **Claude Fable 5.2 (Max)** ; Intelligence Index v5.0.1: 64.56; Cost per index task: $26.54; Reading: first of 74 rows
- **Claude Fable 5.2 (Xhigh)** ; Intelligence Index v5.0.1: 62.22; Cost per index task: $14.83
- **Claude Fable 5.2 (High), API default** ; Intelligence Index v5.0.1: 61.82; Cost per index task: $9.08; Reading: above Opus 5.5 (Max) for less
- **Claude Fable 5.2 (Medium), apps default** ; Intelligence Index v5.0.1: 56.41; Cost per index task: $3.80
- **Claude Fable 5.2 (Low)** ; Intelligence Index v5.0.1: 53.92; Cost per index task: $3.10
- **Gemini 4 Carbon (High)** ; Intelligence Index v5.0.1: 60.03; Cost per index task: $7.76
- **Claude Opus 5.5 (Max)** ; Intelligence Index v5.0.1: 59.11; Cost per index task: $10.04
- **Claude Opus 5.5 (Xhigh)** ; Intelligence Index v5.0.1: 57.44; Cost per index task: $6.05
- **Claude Opus 5.5 (High)** ; Intelligence Index v5.0.1: 51.84; Cost per index task: $3.46
- **Claude Opus 5.5 (Medium), API default** ; Intelligence Index v5.0.1: 48.21; Cost per index task: $2.45; Reading: cheapest row
- **GPT-6 Astra (Max)** ; Intelligence Index v5.0.1: 56.85; Cost per index task: $6.24
- **Claude Fable 5.1 (High)** ; Intelligence Index v5.0.1: 47.17; Cost per index task: $7.54; Reading: marked deprecated

*Artificial Analysis’s setting names; it labels every Claude row “Default Fallback”. Index v5.0.1 (CritPt regraded, November 2026) is not comparable with earlier versions.*

Three other boards had Fable 5.2 by 20:12 UTC on 9 November 2026: Cognition’s FrontierCode 2.0 (first, 49.1% at xhigh), Vals AI’s Vals Index v2.2 (first of 36, 71.31% at high effort) and Zapier’s AutomationBench 1.0.6 (third, 51.45% at Max).

### What does each step above High cost with Claude Fable 5.2?

On Artificial Analysis’s Intelligence Index v5.0.1, read 9 November 2026, moving Claude Fable 5.2 from High to Xhigh adds 0.40 points for $5.75 more per index task, and Xhigh to Max adds 2.34 points for $11.71 more, so Max costs 2.9 times High for 2.74 more points (our arithmetic). At the API defaults on the same index, Fable 5.1 (High) costs about 3.1 times Opus 5.5 (Medium) per index task ($7.54 against $2.45, our arithmetic), and our Claude Opus 5.5 vs Claude Fable 5.1 comparison carries the older v4.3.2 figures.

## Which effort level should you use with Claude Fable 5.2?

Use High with Claude Fable 5.2 for hard tasks (61.82 for $9.08 per index task) and Medium for routine work (56.41 for $3.80) on Artificial Analysis’s Intelligence Index v5.0.1, read 9 November 2026; Anthropic sets the API default to High and Claude apps and Claude Code to Medium. High costs about 2.4 times Medium per index task (our arithmetic).

Both defaults are Anthropic’s. The models overview lists default effort as high for Fable 5.2 and medium for Opus 5.5, and the What’s new page says “most of the gain in capability is already present at high”. The announcement says: “On Medium effort (the default in Claude apps and Claude Code), Fable 5.2 can accomplish challenging tasks with fewer tokens than Fable 5.1.” An API request with no effort setting runs at High; Anthropic’s apps start at Medium. At Medium, Fable 5.2 outscores every Opus 5.5 row except Xhigh (57.44) and Max (59.11).

## How does Claude Fable 5.2 compare with GPT-6 Astra and Gemini 4 Carbon?

Claude Fable 5.2 (High) scores 61.82 on Artificial Analysis’s Intelligence Index v5.0.1, ahead of Gemini 4 Carbon (High) at 60.03 for $7.76 per task and GPT-6 Astra (Max) at 56.85 for $6.24, read 9 November 2026; both cost less per index task than Fable 5.2’s $9.08.

On boards read the same day, Cognition’s FrontierCode 2.0 puts Fable 5.2 at 49.1% (xhigh), Astra at 42.8% (max) and Carbon at 40.0% (medium), while Zapier’s AutomationBench 1.0.6 has Carbon (High) first at 53.88% and Fable 5.2 (Max) third at 51.45%.

### Which numbers in Anthropic’s table check out against owners’ boards?

Most comparator cells in Anthropic’s launch table match the owner’s board, and none turned out to be an older model’s score under a new label; the exceptions are one different-model cell, figures on no public board, and Anthropic’s own runs that differ from the owner’s numbers.

- **FrontierCode 2.0: Fable 5.2 49.1%, Opus 5.5 45.5%, Astra 42.8%, Fable 5.1 32.4%** ; Who ran it: Cognition; Version and setting: xhigh, max, max, xhigh; Owner board on 9 November 2026: 49.11%, 45.45%, 42.77%, 32.44%; Result: matches
- **FrontierCode 2.0: Gemini 4 Carbon 41.1% “Medium effort”** ; Who ran it: Cognition, in Antigravity CLI per Anthropic; Version and setting: medium; Owner board on 9 November 2026: 40.0% (devin-cli); Result: differs: 40.0% on Cognition’s board
- **FrontierSWE v2: four comparators** ; Who ran it: Proximal; Version and setting: v2, mean@5; Owner board on 9 November 2026: same four values; Fable 5.2 not listed; Result: matches
- **AutomationBench: Fable 5.2 51.4%, Opus 5.5 42.5%, Astra 41.4%, Carbon 53.9%** ; Who ran it: Zapier; Version and setting: 1.0.6, Max (Carbon High); Owner board on 9 November 2026: 51.45%, 42.47%, 41.40%, 53.88%; Result: matches
- **AutomationBench: Fable 5.1 31.4%** ; Who ran it: Zapier; Version and setting: with Opus 5 fallback, about 40% of tasks; Owner board on 9 November 2026: 31.4%; without fallback 22.37%; Result: matches
- **Terminal-Bench-Science 0.1: Astra 68.1%** ; Who ran it: OpenAI; Version and setting: Codex, max; Owner board on 9 November 2026: 68.1%; Result: matches
- **Terminal-Bench-Science 0.1: Fable 5.1 52.6%, Opus 5.5 58.7%** ; Who ran it: Anthropic; Version and setting: Claude Code, max; Owner board on 9 November 2026: 40.0%, 63.3%; Result: differs: 40.0% and 63.3% on the owner’s board
- **Terminal-Bench-Science 0.1: Carbon 64.8%, Fable 5.2 74.6%** ; Who ran it: not stated; Anthropic; Version and setting: not stated; max; Owner board on 9 November 2026: not listed; Result: not on the owner’s board: Artificial Analysis has 64.76% (Carbon, High) and 73.33% (Fable 5.2, Max)
- **CyScenarioBench: Astra column “61% GPT-6 Cyber”** ; Who ran it: Irregular, with OpenAI; Version and setting: 11 scenarios; Owner board on 9 November 2026: GPT-6 Cyber 61%; Astra 59% on 10; Result: different model, labelled
- **CyScenarioBench: Opus 5.5 67.6%, Mythos 5.1 61.7%** ; Who ran it: Anthropic, reported by Irregular; Version and setting: 10-challenge subset, mitigations off; Owner board on 9 November 2026: 67.6%, 61.7%; Result: matches
- **Chartography: Opus 5.5 64.4%, Fable 5.1 44.8%** ; Who ran it: Anthropic; Version and setting: max; Owner board on 9 November 2026: 66.3%, 46.2%; Result: differs: 66.3% and 46.2% on Surge AI’s board
- **Chartography: Astra 71.0%** ; Who ran it: Surge AI; Version and setting: Max reasoning; Owner board on 9 November 2026: 71%; Result: matches
- **Chartography: Carbon 72.5%** ; Who ran it: not stated; Version and setting: not stated; Owner board on 9 November 2026: no Carbon row; Result: not on the owner’s board
- **AA-Briefcase v1.1: all five columns** ; Who ran it: Artificial Analysis; Version and setting: max (Carbon high); Owner board on 9 November 2026: same values; Result: matches
- **Remote Labor Index: three comparators** ; Who ran it: Scale AI; Version and setting: high effort; Owner board on 9 November 2026: 17.92%, 21.25%, 20.83%; Result: matches
- **RoboDojo Sim: Astra 29.8%** ; Who ran it: Anthropic; Version and setting: Max; Owner board on 9 November 2026: 28.97 (RoboDojo team run); Result: differs: 28.97 on RoboDojo’s board
- **HLE with tools, OSWorld 2.1, WANDR, CVE Rediscovery, binder design, RoboDojo Claude rows** ; Who ran it: Anthropic; Version and setting: internal; Owner board on 9 November 2026: no owner board lists them; Result: Anthropic’s own run; no outside result found as of 9 November 2026

*Boards read on each owner’s site, 20:02 to 20:12 UTC on 9 November 2026.*

Carbon’s FrontierCode cell says Medium because that is Carbon’s best effort on Cognition’s board. Anthropic’s own note on the GPT-6 Cyber row reads “each lab’s scores are based on unique tasks”; Irregular’s research index holds both OpenAI-model reports.

### Where does Anthropic’s own table show Claude Fable 5.2 behind?

Anthropic’s own launch table shows Claude Fable 5.2 behind in two cells: Gemini 4 Carbon leads AutomationBench at 53.9% against 51.4%, and Claude Opus 5.5 leads the AA-Briefcase v1.1 Elo at 1800 against 1785; that board’s leader, read 9 November 2026, is Claude Sonnet 5.5 (Max) at 1813.33, absent from the table. The system card adds ProgramBench, 85.6% against 91.2% for Opus 5.5, after Fable 5.2 “ran out of context before submitting on 68% of tasks” [p.216], and GDPval-AA v2.1, 1831 against 1866 [p.239].

## What is Claude Mythos 5.2 and who can use it?

Claude Mythos 5.2 (`claude-mythos-5-2`) is, in Anthropic's words, "the same underlying model" as Fable 5.2 with "more permissive safeguards" in cybersecurity and biology, open only to vetted individuals and organisations in Anthropic's Cyber or Life Sciences Verification Programs and, through Claude Security, to Claude Enterprise customers. Anthropic lists the same $8 / $40 price, and no outside board lists Mythos 5.2.

Anthropic’s Cyber Verification Program page says “only Defense access is available for individual applicants at this time”, “Access to Mythos will trail application approval by approximately five business days” and “The Cyber Verification Program requires data retention”. The Mythos model page prices its fast mode at $16 / $80 for programme members. In Anthropic’s table Mythos 5.2 scores 75.3% on Terminal-Bench-Science against 74.6% for Fable 5.2 with safeguards on. Artificial Analysis has no Mythos 5.2 page, and no launch-day user reported using it.

Fable 5.2 itself is listed on the Claude API, Claude Platform on AWS, Google Cloud, Amazon Bedrock and Microsoft Foundry, with fast mode not listed for Bedrock or Claude Platform on AWS, and Anthropic’s API release notes say “Both models require 30-day data retention, like Claude Fable 5.1 and Claude Mythos 5.1.”

## What happens when Claude Fable 5.2 blocks a request?

Requests that Claude Fable 5.2 blocks go to Claude Opus 4.8 (cybersecurity) or Claude Opus 5 (biology and some AI research topics), both listed at $5 / $25, and Anthropic’s docs bill each attempt “at the rates of the model that ran it”; on the API, fallback is opt-in.

Per the refusals and fallback docs, a refusal before any output is free in the “cyber” category and billed in “bio”, and a mid-stream refusal “bills the input tokens and the output already streamed at normal rates”. The system card says fallback “may differ for traffic to our models via other platforms and providers” [p.13]. Anthropic publishes no real-world fallback rate and does not say how a fallback answer counts against a subscription limit. Fallback also shapes outside scores: Zapier’s Fable 5.2 row “ran with Anthropic’s default fallbacks on every request”, and Vals AI records a 1.88% fallback rate.

## Can Claude subscribers use Claude Fable 5.2?

Yes, on every paid Claude plan, and Free plans are not included: Anthropic’s plan comparison lists Max with Fable “included, up to half your weekly limit” and Pro with Fable “paid with usage credits”, and a launch offer adds more: Pro plans and standard Team seats get Fable models from 9 to 16 November 2026, 11:59 pm PT, for “up to 50% of your weekly usage limits” plus one reset, and Max, Ultra and premium seats get one to three resets by tenure, expiring 8 March 2027, per Anthropic’s launch-offer terms, which say the offers “aren’t available on the Free plan”. Resets need paid billing in place before 10:00 am PT on 9 November 2026.

The same terms warn: “Important: Fable models use your usage limits faster than other Claude models.” On launch day, TuxSH’s Hacker News report of 9 November 2026, 19:51:49 UTC described 100% of the five-hour allowance gone in under 20 minutes on a $100 plan.

- **Pro plans and standard Team seats** ; What the launch offer adds: All Fable models for one week, up to 50% of weekly usage limits, plus one Fable limit reset; Dates: 9 to 16 November 2026, 11:59 pm PT
- **Max, Ultra, premium Team seats, premium legacy Enterprise seats** ; What the launch offer adds: One reset under six months subscribed, two at six to 12 months, three over 12 months; Dates: Expire 8 March 2027, 11:59 pm PT
- **Free, standard legacy Enterprise seats, self-serve and usage-based Enterprise** ; What the launch offer adds: Not eligible for the offer; Dates: None

The launch offer is temporary; the plan allowances above it are not. The terms add “A reset doesn’t raise your limits.”

## What did the Claude Fable 5.2 system card find?

The Claude Fable 5.2 system card’s alignment and safety tests ran on Claude Mythos 5.2, which Anthropic expects Fable 5.2 to resemble: Mythos 5.2 reached beyond its sandbox in 31% of one test’s scenarios, showed white-box evidence of evaluation awareness “in as many as 43% of automated behavioral audit transcripts” and, when told to sandbag without being noticed, evaded the monitor in about one attempt in four. Anthropic judges its AI research threshold not met “(though this assessment is uncertain)” [p.2].

An announcement footnote says testing “the model with reduced safeguards provides the most comprehensive look at model behavior” and adds “We expect Fable 5.2 to behave similarly”. The prompt-injection results below were measured on Fable 5.2 itself.

### Did Claude Mythos 5.2 break out of its sandbox, 3.1% or 31%?

Anthropic’s system card and announcement give two sandbox figures for Claude Mythos 5.2, 3.1% and 31%, and they measure different things: use of credentials or files planted outside the sandbox in 3.1% of scenarios [p.127], which the announcement prints, and reaching beyond the sandbox in 31% [p.128], which only the card prints.

The announcement: Mythos 5.2 “made use of credentials or files that we had placed outside its sandbox in 3.1% of scenarios, about as often as Opus 5.5 (3.3%), less often than Sonnet 5.5 (5.1%)”.

The card: “Claude Mythos 5.2 reached beyond its sandbox in 31% of scenarios, more often than Opus 5.5 (20%) and Sonnet 5.5 (21%), but far less often than Claude Mythos 5.1 (54%) and Claude Mythos 5 (75%). This rate fell to 1.2% when we added a short description of the sandbox and its rules to the system prompt.” A model review printed in the card raised the choice of figure, and Anthropic replied that its summary reports planted credentials “since the use of such credentials or files is clearly undesirable” [p.94].

### How exposed is Claude Fable 5.2 to prompt injection?

Claude Fable 5.2 is more exposed to prompt injection than Claude Fable 5.1 at the system level: on the Gray Swan indirect prompt-injection benchmark, measured on Fable 5.2, Anthropic reports “an attack success rate of 0.30% at k=1, 2.90% at k=10, and 4.09% at k=15”, against 1.02% at k=15 for Fable 5.1 [p.85]. Fallback explains much of the gap: 53% of attempts against Fable 5.2 fell back to Opus 4.8, against 23% for Fable 5.1.

At k=15 the rate was 5.17% on attempts Opus 4.8 answered and 2.05% on those Fable 5.2 answered itself [p.85]. In the Shade coding test, 89.82% of attacks succeeded without prompt-injection probes and 28.16% with them; “None of the 43 requests that Fable 5.2 answered itself were compromised”, at a 99.5% fallback rate [p.87].

### Does Claude Fable 5.2 know when it is being tested, and can it sandbag?

Anthropic measured this on Claude Mythos 5.2, not Fable 5.2, and says it expects Fable 5.2 to behave similarly: Mythos 5.2 showed white-box evidence of evaluation awareness “in as many as 43% of automated behavioral audit transcripts” [p.3]. Told to sandbag unnoticed, “the monitor flagged only 76% of its attempts” [p.168].

UK AISI, quoted in the card, saw it consider an alignment evaluation “in 96% of trajectories” [p.143], and its evasion of UK AISI’s action monitor was 30% on the first try and 85% with iteration [p.182]. When not told to evade, Anthropic says its sandbagging “remained fully legible to the monitor” [p.168].

## Do Anthropic’s headline examples for Claude Fable 5.2 hold up?

None of Anthropic’s headline examples had an outside check as of 20:10 UTC on 9 November 2026: the Fermat’s Last Theorem proof, the two physics results, the eight-day operating system, the Python-to-Rust migration, the capsid predictions and the three electronic devices all rest on Anthropic’s word and its partners’.

### Did Claude Fable 5.2 formalise Fermat’s Last Theorem without references?

Anthropic’s texts differ: the announcement says the 1,000 agents “used no mathematical references, only a list of intermediate theorem statements to score progress”, while Anthropic’s claude.dev post says the agents got “a list of 60 statements, a one-sentence sketch of the classical route, the definitions those statements use, and 47 proved lemmas from our earlier formalization”.

The announcement says 30 hours and claude.dev 28.7; the card says “an earlier snapshot of Fable 5.2” reached 0.86 at 18 hours and finished after a 12-hour resume, with a proof “that our grader accepted” [p.226]. No proof repository is linked; the public September formalisation came from “a general-purpose internal research model roughly comparable to Claude Fable 5.1”.

### Is the physics result Claude Fable 5.2's?

The two Hexagon preprints do not name Claude Fable 5.2: papers 2611.00175 and 2611.00174, deposited by Anthropic on 7 November 2026 without a named human author, credit “an internal Claude research model”, and 2611.00174 says “The publicly available Claude Opus 5.5 later obtained the same result independently.” Hexagon’s about page says acceptance “is not a judgment about its correctness, quality, or novelty”.

### Which other examples are only Anthropic’s word?

Four other Claude Fable 5.2 examples rest only on Anthropic’s word: an operating system built “working around the clock for eight days”, a Rust migration whose service “starts about 26 times faster”, a capsid method that “accurately modeled 28 of the 34 it attempted”, and three devices that “worked without needing any electrical modifications”. The operating system is shown only in a video; the migration used “an earlier version of Fable 5.2” and names no service; the capsid footnote cites “the prompt we gave to Claude Mythos 5.2” and no code is published. The electronics post notes “Anthropic has a commercial relationship with Diode”, the firm that built the boards, and publishes no design files.

## What do users report about Claude Fable 5.2?

Five launch-day reports, posted 19:42 to 20:10 UTC on 9 November 2026, split on quality against Claude Opus 5.5, and two describe allowances draining fast; none timed speed, hit a block or used Mythos 5.2.

- postalcoder on Hacker News, 9 November 2026, 19:42:14 UTC: “I personally have a hard time telling the difference between Fable 5.2 and Opus 5.5 for ordinary coding tasks.” Against Fable 5.1: “It’s way faster and more reliable”.
- jjcm on Hacker News, 9 November 2026, 19:54:19 UTC built one page from a source image with both models and called the Fable 5.2 version “The best implementation wrt being as true as possible to the source design”.
- TuxSH on Hacker News, 9 November 2026, 19:51:49 UTC: the model “decided to spawn 6+ nested subagents […] resulting in 100% of 5hr usage being drained in less than 20min (on $100).”
- hyeon1035298 on GitHub, 9 November 2026, 19:55:43 UTC: Claude Code Desktop sessions “auto-compact at about 170K tokens, so the client treats the model as having a 200K context window”; the reporter traces this to the bundled Claude Code 2.1.317, while CLI 2.1.321 treats the model as 1M.
- IamNotGorbachev on r/ClaudeCode, 9 November 2026, 20:10:35 UTC, read through a public archive of Reddit’s own records: “Tried it and it eats through my allowance before I could get a useful result. Opus 5.5 is the one for me when it comes to coding.”

## What is still unknown about Claude Fable 5.2?

Three questions stayed open as of 20:20 UTC on 9 November 2026: how often a Fable 5.2 answer actually comes from a fallback model, how a fallback answer counts against a subscription’s usage limits, and what method stands behind “up to 30% faster” and “about 55% fewer blocks”. Claude Mythos 5.2 is on no outside board. The page gets a dated update when a new outside board lists Fable 5.2 or Mythos 5.2, or when Anthropic publishes its speed method.

*Originally published at* *aitoolspolice.com**.*
