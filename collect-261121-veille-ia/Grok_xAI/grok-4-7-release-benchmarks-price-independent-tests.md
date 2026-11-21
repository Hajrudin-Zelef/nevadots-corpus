---
id: collect-261121-veille-ia/grok-xai/grok-4-7-release-benchmarks-price-independent-tests
title: "Grok 4.7 Release: Benchmarks, Price, Independent Tests"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: ["2026-09-21"]
keywords: ["grok", "price", "days", "model", "september", "release", "post", "xais"]
source: https://cellcog.ai/blog/grok-4-7-release-date/
source_anchor: ""
source_lines: [1, 303]
section: "Grok_xAI"
---
Grok 4.7 shipped on September 21, 2026, forty days after Grok 4.6 and thirty-one days after the first date its founder gave for it. xAI’s launch post, its model documentation and its @SpaceXAI account all moved within ten minutes of each other, and the model is live in Cursor, Grok Build and the API at Grok 4.6’s price. This page was a rumor tracker for thirteen days; from today it is the release record. The confirmed facts come first, then every claim made about the model before it existed, graded against what xAI published.

1. xAI released Grok 4.7 on September 21, 2026. The launch post went up on x.ai/news, the model docs list grok-4.7, and @SpaceXAI posted at 16:17 UTC: ‘Grok 4.7 is here. It’s a notable improvement over Grok 4.6 at the same price and speed.’ It is live in Cursor, Grok Build and the API.
2. The price is Grok 4.6’s: $2 per million input tokens and $6 per million output, $0.50 cached, 500k context, doubling above 200k prompt tokens. A fast variant runs at twice the output speed for twice the price. Knowledge cutoff May 2026.
3. On xAI’s own table, 4.7 beats 4.6 on all seven rows: CursorBench 4.0 46.3 percent from 40.4, Terminal-Bench 4.0 38.0 from 20.3, EEBench 64.0 from 53.0, the Harvey legal benchmark 19.6 from 15.8. Against Fable 5.1 Max it leads on two of seven and trails on the coding rows; against GPT-5.6 Sol Max it leads on four.
4. xAI’s description is a bigger base model, a longer reinforcement-learning run weighted toward many-hour tasks, better self-checking, and native understanding of the Grok Bot harness. No parameter count, no mention of SpaceX data. The 2.1 trillion figure and the SpaceX corpus were the founder’s claims and stay unconfirmed.
5. The frontier claim did not land. Musk wrote on August 12 that 4.7 ‘will exceed all current models’; xAI’s launch table puts Fable 5.1 Max ahead on CursorBench, Terminal-Bench, GDPval and HealthBench, and the post calls 4.7 ‘highly competitive in its class’. Musk’s September 14 downgrade to ‘roughly on par with Opus 5.0’ is closer to the table, and the table has no Opus row to grade it against.
6. The dates are graded and closed. First target August 21, actual release September 21: 31 days late. The September 1 ‘comes out in 10 days’ was 10 days late; the September 11 ‘a few more days’ became ten. Grok 4.5 was 46 days late and 4.6 five; 4.7 sits between them.
7. Safety is the part of the launch post with new numbers: 62.4 percent on LatchBio’s biosafety benchmark, 3.3 percent of risky dual-use prompts allowed through on xAI’s HackerBench v0.3, and invite-only red-team access for cybersecurity partners. CellCog does not route to Grok; the layer you choose above the engine is the decision that survives this release and the next.

## Â§ 01What xAI shipped

The post is titled âIntroducing Grok 4.7â and dated Sep 21, 2026. Its subhead: âSpaceXAIâs most powerful model for coding and knowledge work. Twice as fast, at half the price of comparable models.â Its first paragraph is more careful than its subhead: the model âworks longer on difficult tasks, checks its own work more carefully, and comes with our best-calibrated safeguards to dateâ, is âServed at the same price and speed asâ Grok 4.6, and âis highly competitive in its classâ.

| Item | What xAI published | 
|---|---|
| Release | September 21, 2026; @SpaceXAI 16:17 UTC, x.ai/news post, docs entry the same hour | 
| Base model | âa new, larger base modelâ than Grok 4.6; no parameter count | 
| Training | âa longer reinforcement learning run on a harder mix of tasks, weighted toward problems that take many hours to completeâ | 
| Grok Bot | trained âto natively understand theâ Grok Bot harness | 
| Context | 500k tokens | 
| Price, under 200k prompt tokens | $2.00 input, $0.50 cached input, $6.00 output, per million | 
| Price, 200k and above | $4.00 input, $1.00 cached, $12.00 output, per million | 
| Fast variant | âtwice the output speed at twice the priceâ | 
| Knowledge cutoff | May 2026 | 
| Availability | Cursor, Grok Build, the Grok API, third-party harnesses, model routers, cloud platforms | 
| Safety | 62.4 percent on LatchBioâs biosafety benchmark; 3.3 percent of risky dual-use prompts allowed through on HackerBench v0.3; invite-only red-team access for cybersecurity partners | 

The price row is the 4.6 row to the cent, and the docs confirm it: the two grok-4.7 lines in xAIâs pricing table match the two grok-4.6 lines. That is what âhalf the price of comparable modelsâ means in the subhead: not cheaper than 4.6, cheaper than the frontier models in xAIâs own comparison, which list at $4/$20 (GPT-5.6 Sol) and $10/$50 (Fable 5.1).

## Â§ 02xAIâs benchmark table

The launch post carries one table with four columns: Grok 4.7 at xHigh effort, Grok 4.6 at High, GPT-5.6 Sol at Max and Fable 5.1 at Max. These are xAIâs numbers, run by xAI. The first independent read, from Artificial Analysis, landed 21 minutes after launch and sits in the next section.

| Benchmark | Grok 4.7 | Grok 4.6 | GPT-5.6 Sol | Fable 5.1 | 
|---|---|---|---|---|
| Input price, per million tokens | $2 | $2 | $4 | $10 | 
| Output price, per million tokens | $6 | $6 | $20 | $50 | 
| CursorBench 4.0, software engineering | 46.3% | 40.4% | 41.7% | 51.8% | 
| DeepSWE v1.1, software engineering | 71.0% (high effort) | 65.2% | 72.7% | 70.0% | 
| EEBench, electrical engineering | 64.0% | 53.0% | 39.4% | 56.4% | 
| AA Briefcase v1.1, multi-hour office work | 1,657 | 1,546 | 1,487 | 1,678 | 
| Terminal-Bench 4.0, multi-hour terminal work | 38.0% | 20.3% | 37.3% | 57.9% | 
| Harvey Legal Agent Benchmark | 19.6% | 15.8% | 2.5% | 6.7% | 
| HealthBench Professional, clinical reasoning | 56.7% | 48.5% | 60.5% | 62.1% | 

Read across the rows: 4.7 beats 4.6 on all seven, with the largest jump on Terminal-Bench 4.0, which nearly doubles. Against Fable 5.1 Max it leads on EEBench and the Harvey legal benchmark, sits within a point on DeepSWE and AA Briefcase, and trails on the two coding benchmarks Cursor users care most about, on HealthBench and on GDPval. Against GPT-5.6 Sol Max it leads on four rows. The postâs own framing, âat the frontier in price-performanceâ, is the claim the table supports; âmost powerfulâ is the subheadâs word and the table does not support it against Fable.

| Model | Elo | 
|---|---|
| Fable 5.1 (max) | 1735 | 
| Grok 4.7 (xhigh) | 1695 | 
| Grok 4.6 (high) | 1605 | 
| GPT-6 Astra (max) | 1542 | 

## Â§ 03The first independent reads

Artificial Analysis posted its Intelligence Index result at 16:38 UTC, 21 minutes after xAIâs announcement. Cursorâs own post went up at 16:22 UTC. Neither is a full third-party benchmark table yet; both are the first numbers not run by xAI.

| Source | Read | Number | 
|---|---|---|
| Artificial Analysis | Intelligence Index, xhigh effort | 46, rank 16 of 655 models on its index | 
| Artificial Analysis | Output tokens per Intelligence Index task | 81,000 for Grok 4.7 (xhigh) against 36,000 for Grok 4.6 (xhigh) | 
| Artificial Analysis | Output speed on long prompts | about 188 tokens per second; about 7.1 minutes per index task | 
| Artificial Analysis | AA-Omniscience hallucination rate | 29 percent, from 34 percent for Grok 4.6 (high); accuracy 47 against 48 percent | 
| Cursor | Availability and price | âserved at the same price and speed as Grok 4.6â; Grok 4.6 remains available in Cursor | 
| xAI Grok Bot docs | Model under running Bots | the overview page now opens on a âMeet grok-4.7â link; whether running Bots moved is still not stated | 

The two reads agree on the shape. Artificial Analysisâs own framing is that âGrok 4.7âs gains come with higher token usageâ: the model thinks more than twice as long per task as 4.6 to earn its score, which is what xAIâs âworks longer on difficult tasksâ means in tokens. At the same per-token price as 4.6, a task that used to cost one unit now costs about two. Cursor confirms the price and keeps 4.6 on the menu, which is the honest option for anyone whose tasks did not need the extra thinking.

Two platforms posted their own availability notes the same day, which is where most of the modelâs first paid traffic will come from.

| Platform | Post | What it says | 
|---|---|---|
| GitHub Copilot | github.blog changelog, September 21 | Grok 4.7 âis now rolling out in GitHub Copilotâ, âdesigned for agentic coding and complex, multistep workflowsâ | 
| Vercel AI Gateway, fx and eve | vercel.com/changelog, September 21 | Available on all three, â40% off for a week starting September 21â | 
| Cursor | forum.cursor.com, 16:22 UTC | âsame price and speed as Grok 4.6â; 4.6 stays available | 

## Â§ 04Week one: the independent tests

By September 26 the first third-party results had moved past launch-day reads. None of them is run by xAI.

| Source | Date | Read | 
|---|---|---|
| Arena (LMArena) | Sep 22 | grok-4.7-xhigh added to the Code Arena: WebDev leaderboard | 
| Arena (LMArena) | Sep 24 | Grok 4.7 (xHigh) added to the Agent Arena leaderboard | 
| Arena (LMArena) | Sep 25 | grok-4.7-xhigh added to the Text Arena leaderboard, 4 days after release | 
| Snorkel AI, Senior SWE-Bench | Sep 22 | 40.0 percent tasteful pass@3, from 38.9 for Grok 4.6; sixth overall at $0.24 per trial | 
| Cursor forum | Sep 22 | Users reported Grok 4.7 not found in Cloud Agents and Cloud Chats on launch plus one; local chat unaffected | 

Snorkelâs benchmark is the most useful of these for a buyer, because it grades code quality as well as correctness: âIt reaches a tasteful pass@3 of 40.0%, up from 38.9% for Grok 4.6.â That is a 1.1-point gain, far smaller than the 17.7 points xAIâs own table shows on Terminal-Bench 4.0, and a reminder that the vendor picks the rows. Arenaâs changelog records the additions but not a settled rank; ranks on young entries move as votes arrive.

## Â§ 05The Cyber Index: first on a new benchmark

On September 28, 2026, Artificial Analysis launched the Artificial Analysis Cyber Index, a defensive-security benchmark that averages three tests on real software: finding vulnerabilities (CWE-Bench-AA), validating them (DeepsecBench-AA) and patching them without breaking the code (CyberGym-E2E-AA). Grok 4.7 at xhigh effort scored 56 percent, tied for first with Xiaomiâs MiMo-V2.6-Pro among the 17 models on the launch chart.

| Model | Score | 
|---|---|
| Grok 4.7 (xhigh) | 56% | 
| MiMo-V2.6-Pro | 56% | 
| GPT-6 Luna (max) | 53% | 
| GLM-5.3-Flash | 50% | 
| Muse Spark 1.3 (xhigh) | 44% | 

The frontier models from OpenAI and Anthropic sit lower for a reason the index states itself: âGPT-6 Astra, GPT-6 Sol, Claude Fable 5.1, Claude Opus 5.5, Qwen3.8 2.4T A95B, and Qwen3.8 27B refuse at least 98% of tasks, making it difficult to assess frontier model performance.â Part of Grok 4.7âs lead reflects what it attempts, not only what it solves. For a security team that is the practical question: a model that declines the patching task scores zero on it, whatever it could have done.

## Â§ 06VulcanBench: top three on an outside coding board

On October 4, 2026, VulcanBench updated its Frontier v4 leaderboard with 92 runs of Grok 4.7 through Cursorâs agent CLI: 23 tasks at each of the four effort levels Cursor offers (it has no Max). Grok 4.7 took first, second and third place, 93.15 at Extra-high, 92.71 at High and 92.30 at Medium, ahead of Fable 5.1 at max (91.84) and Opus 5.5 at high (91.11). Low came 12th at 89.42. The tasks are behavioral reconstructions graded by hidden tests; the combined score weights functional correctness at 50 percent, lint and complexity at 8.5, security at 8.5 and Code quality, judged by a model panel for a human reader, at 33.

| Rank | Model, harness, effort | Combined | Tasks passed | Minutes per task | Cost per task | 
|---|---|---|---|---|---|
| 1 | Grok 4.7, Cursor, extra-high | 93.15 | 23/23 | 28.5 | unavailable | 
| 2 | Grok 4.7, Cursor, high | 92.71 | 22/23 | 25.4 | unavailable | 
| 3 | Grok 4.7, Cursor, medium | 92.30 | 21/23 | 27.2 | unavailable | 
| 4 | Fable 5.1, Claude Code, max | 91.84 | 23/23 | 27.1 | $9.06 | 
| 5 | Opus 5.5, Claude Code, high | 91.11 | 22/22 | 21.4 | $3.27 | 
| 12 | Grok 4.7, Cursor, low | 89.42 | 18/23 | 20.2 | unavailable | 
| 13 | GPT-6 Astra, Codex, max | 89.30 | 23/23 | 10.3 | $2.57 | 
| 17 | GPT-6.1 Sol, Codex, extra-high | 88.78 | 23/23 | 15.4 | $0.43 | 

Two things set this column apart from the rest of the board. The judges changed. Every other column is scored by Muse Spark 1.3 and Grok 4.6, but Grok 4.6 âis not neutral for an xAI modelâ, so VulcanBench seated GPT-6.1 Sol in its place for Grok 4.7. The board itself says GPT-6.1 Sol rates Grok 4.7âs code about 6 points above Muse, so the column is ânot strictly comparable with the other columnsâ. VulcanBench reran the check on Muse Spark 1.3 alone for every column: Grok 4.7 then scores 88.64 to 92.27, still first at Medium, High and Extra-high, and second at Low behind Fable 5.1 (89.46). The order survives the single-judge check; the exact margins depend on the panel.

The cost is missing. In the boardâs words, â$/task is unavailable, not $0â: the sweep ran on a Cursor subscription and VulcanBench has no list price for Grok 4.7, so it sits off the cost chart. Time is published, and it tells the same story as the token counts above: 28.5 minutes per task at Extra-high, about level with Fable 5.1 at max (27.1) and nearly three times GPT-6 Astra at max (10.3). Medium is judged on 22 of 23 tasks, because one run hit the 3-hour bound and counts as failed.

Musk shared the board on October 6 at 15:32 UTC, quoting a @XFreeze post of the top three: âGrok 4.7 is excellent at dealing with large code repositoriesâ. It is the first general coding board in this record to put 4.7 ahead of Fable 5.1. xAIâs own launch table had Fable 5.1 Max ahead on CursorBench and Terminal-Bench, and Snorkelâs Senior SWE-Bench put 4.7 sixth. Three coding benchmarks, three answers: which one predicts your work depends on whether your tasks look like hidden-test reconstructions, long terminal sessions, or code a senior engineer has to approve.

## Â§ 07What Grok 4.7 costs, by where you buy it

| Where | Input | Cached input | Output | 
|---|---|---|---|
| xAI API, grok-4.7 | $2.00 | $0.50 | $6.00 | 
| OpenRouter, xAI endpoint | $1.60 | $0.40 | $4.80 | 
| Grok 4.7 Fast (Cursor and Grok Build only) | $4.00 | $1.00 | $12.00 | 

xAIâs own price did not move in week one: its pricing page still lists grok-4.7 at Grok 4.6âs rates, and above 200k prompt tokens both double. xAI describes Fast as âthe same Grok 4.7 model served on faster infrastructure, at twice the standard token rates,â sold only through Cursor and Grok Build. OpenRouterâs catalog lists xAIâs own endpoint 20 percent under the direct rate. Price per token is only half the bill: Artificial Analysis measured Grok 4.7 using about twice the output tokens per task of Grok 4.6, so the same task costs more even where the rate is unchanged.

## Â§ 08The rumors, graded against the launch

This page existed because, for two months, everything known about Grok 4.7 came from one X account. Now the company has spoken, and each claim can be marked.

| Claim | Source | Grade | 
|---|---|---|
| A new, larger model than 4.6 | Musk, July 28 | Confirmed: âa new, larger base modelâ | 
| 2.1 trillion parameters | Musk, July 28 | Unconfirmed: xAI publishes no parameter count | 
| SpaceX engineering data in supplemental training | Musk, July 21 and August 12 | Absent: the launch post does not mention SpaceX data; EEBench is the only engineering row, and 4.7 leads it | 
| âSlightly slower to serveâ than 4.6 | Musk, July 28 | Contradicted: âServed at the same price and speed asâ Grok 4.6 | 
| Better token efficiency | Musk, July 28 | Not addressed in the post | 
| âWill exceed all current modelsâ | Musk, August 12 | Not met: Fable 5.1 Max leads on four of seven rows and GDPval | 
| âRoughly on par with Opus 5.0, not 5.1â | Musk, September 14 | Ungradable: Opus 5 appears only on the postâs cost chart, not in the table | 
| Gives up on hard tasks too early, weak self-checking | Musk, September 11 | Addressed in the fix description: âworks longer on difficult tasks, checks its own work more carefullyâ | 
| Multimodal performance needs fixing | Musk, September 14 | Not addressed: no multimodal row in the launch table | 
| Release in Cursor and Grok Build first, as 4.6 did | Our read | Confirmed: âavailable today inâ Cursor and Grok Build | 
| Grok Bot gets it | Open on this page | Partly: trained to understand the Grok Bot harness; whether running Bots move is not stated | 
| Leaked benchmarks | Various | None ever existed; the first numbers are xAIâs, today | 

## Â§ 09The dates, closed

Every date came from Musk. Here is how they held, now that the company has shipped.

| Posted | Muskâs target | Implied date | Outcome | 
|---|---|---|---|
| July 24 | âGrok 4.7 in 4 weeksâ | About August 21 | Missed by 31 days | 
| August 12 | âShould be ready in 3 to 4 weeksâ | September 2 to 9 | Missed by 12 days from the late end | 
| September 1 | âComes out in 10 daysâ | September 11 | Missed by 10 days | 
| September 11 | âNeeds a few more days to cookâ | Mid-September | âA fewâ became ten | 

| Model | First target (date posted) | xAI release | Gap | 
|---|---|---|---|
| Grok 4.5 | âHopefully out by late Mayâ (April 18) | July 16, 2026 | About 46 days after May 31 | 
| Grok 4.6 | âIn 2 weeksâ (July 24), âaround August 7â (July 28) | August 12, 2026 | 5 days | 
| Grok 4.7 | âIn 4 weeksâ (July 24), so about August 21 | September 21, 2026 | 31 days | 

The pattern this page described on September 8 held: optimism, not fiction. The model was real, it shipped, and it shipped after the dates. 4.7âs slip lands between 4.6âs five days and 4.5âs seven weeks. The one thing that changed between the September 14 grade and the launch is the founderâs tone: âroughly on par with Opus 5.0â became, on launch day, âa strong combination of intelligence, speed & low costâ (Musk, 16:25 UTC).

## Â§ 10The chain, dated and closed

| Date | Event | Source type | 
|---|---|---|
| Jul 21 | Musk: SpaceX data goes into âsupplemental training of the 2T runâ | Musk on X | 
| Jul 24 | Musk: âGrok 4.6 in 2 weeks and Grok 4.7 in 4 weeksâ | Musk on X | 
| Jul 28 | Musk: 4.7 âthe 2.1T modelâ, a few weeks after 4.6, slightly slower to serve | Musk on X | 
| Aug 12 | Grok 4.6 released; Musk: 4.7 âready in 3 to 4 weeksâ, âwill exceed all current modelsâ | xAI official + Musk | 
| Aug 15 | Musk: 4.7 âhas a good chance of exceeding all current models in intelligenceâ | Musk on X | 
| Aug 20 to 26 | Ox Alpha appears on OpenRouter, guessed as 4.7; revealed as GLM-5.3-Flash | Community + Z.ai | 
| Aug 21 | First target passes | Our read | 
| Sep 1 | Musk: âGrok 4.7 comes out in 10 daysâ | Musk on X | 
| Sep 11 | Target passes; Musk: âneeds a few more days to cookâ, RL penalized response length | Musk on X | 
| Sep 13 | Musk names Grok 4.8: 2.5T, C++ stack, training finishing, RL next | Musk on X | 
| Sep 14 | Musk grades 4.7 âroughly on par with Opus 5.0, not 5.1â | Musk on X | 
| Sep 21, 16:17 UTC | @SpaceXAI: âGrok 4.7 is here.â Launch post, docs entry, OpenRouter listing within minutes | xAI, official | 
| Sep 21, 16:25 UTC | Musk: âa strong combination of intelligence, speed & low costâ | Musk on X | 

## Â§ 11The founderâs record, word for word

Kept for the record; these are the posts the tracker graded, with their sources.

**July 24:** âGrok 4.6 in 2 weeks and Grok 4.7 in 4 weeksâ (source)

**July 28:** âGrok 4.7 will be the 2.1T model released a few weeks later. This will be better than 4.6 in every way, except slightly slower to serve, albeit with even better token efficiency.â (source)

**August 12:** âGrok 4.7 is significantly better than 4.6 and should be ready in 3 to 4 weeks. Initial training is complete and now weâre adding a massive amount of SpaceX company data in supplemental training.â (source) Six minutes later: âGrok 4.7 will exceed all current models.â (source)

**September 1:** âGrok 4.7 comes out in 10 daysâ (source)

**September 11:** âGrok 4.7 needs a few more days to cook. We might have penalized response length too much (or something) in RL, as it still gives up on hard tasks (that it can do!) too early and isnât yet sufficiently rigorous in checking its work.â (source)

**September 14:** âGrok 4.7 should be roughly on par with Opus 5.0, not 5.1. Better in some ways, worse in others. We need to fix multimodal performance. Grok 4.8 will be a noticeable improvement. Grok 4.9 is probably Astra/Fable class. Grok 5 maybe better than anything. We shall see.â (source)

**September 21, 16:25 UTC:** âGrok 4.7 is a strong combination of intelligence, speed & low costâ (source)

The September 11 post is the one the launch text answers: xAIâs model âworks longer on difficult tasks, checks its own work more carefullyâ. The July 28 post is the one it contradicts: not slower to serve, the same speed. The July 21 and August 12 SpaceX-data posts are the ones it ignores.

## Â§ 12The 4.x cadence, closed

| Model | Announced by xAI | Days since previous | 
|---|---|---|
| Grok 4 | July 9, 2025 | n/a | 
| Grok 4.1 | November 17, 2025 | 131 | 
| Grok 4.20 (public beta) | February 17, 2026 | 92 | 
| Grok 4.5 | July 16, 2026 | 149 | 
| Grok 4.6 | August 12, 2026 | 27 | 
| Grok 4.7 | September 21, 2026 | 40 | 

Grok 4 Fast and Grok 4.1 Fast are left out as speed variants of existing models. The 4.20 row uses Muskâs February 17 public-beta announcement; the 4.5 row uses the x.ai news date of July 16. The next model on the founderâs ladder, Grok 4.8, has its own tracker; the September 13 post said its training would finish that week and RL would start, which on this tableâs cadence points at October at the earliest.

## Â§ 13What this means if you run agents

For anyone on Grok 4.6, the swap is a model id at the same price with a vendor table that says every row improves; the Terminal-Bench jump is the one that matters for long agentic work. For anyone choosing an engine, xAIâs table places 4.7 as the price-performance option below Fable 5.1 Max on coding, and the safety numbers are the first hard ones xAI has published for a Grok launch. For anyone choosing where agents run, none of this changes the decision: an engine swap is a version string, and what persists is the layer around it.

## Â§ 14What we are watching for

- **Settled ranks.** Arena added Grok 4.7 to its Text, WebDev and Agent boards between September 22 and 25; a settled rank on each, an Epoch read and a SWE-bench Verified score are still open. Every row in xAIâs own table is still xAIâs.
- **Grok Bot.** The Grok Bot docs now open on a âMeet grok-4.7â link; xAI has still not said whether running Bots moved to 4.7 or move automatically.
- **The fast variantâs price row** on docs.x.ai, and whether the 4.6 fast row is retired.
- **The Opus comparison.** Musk graded 4.7 against Opus 5.0; xAIâs table omits it and Artificial Analysisâs launch-day posts did not place the two side by side. VulcanBenchâs October 4 board is the first to place 4.7 beside Opus 5.5 (not 5.0): ahead of Opus 5.5âs best column at Medium, High and Extra-high.
- 
**Grok 4.8.** The successor named on September 13; its tracker carries the chain.
- 
**The week after the discount.** Vercelâs 40 percent is for one week from September 21; Copilot has not published the modelâs premium-request multiplier. Both prices, and whether Cursor keeps 4.6 alongside 4.7 past the first week, land here.

## Â§ 15The record

As of September 21, 2026, 16:40 UTC: x.ai/news/grok-4-7 returned 200 to a browser-agent read at 16:29 UTC with the launch text quoted above; docs.x.ai/developers/models.md lists grok-4.7 (500k context, $2.00/$0.50/$6.00 under 200k, $4.00/$1.00/$12.00 above, knowledge cutoff May 2026); OpenRouterâs x-ai/grok-4.7 entry carries a created time of 16:19:01 UTC. X posts are id-clocked: @SpaceXAI 16:17:53 and 16:17:54 UTC (four posts), @elonmusk 16:25:47 UTC. All benchmark figures are xAIâs, read from the launch post.

## Â§ 16Update log

This is a living page; when the story moves, the update lands here.

As of September 8, 2026: page opened. Latest Musk target is about September 11; no official movement to log.

September 11, 2026: the September 11 target passed with no release; Musk: âneeds a few more days to cookâ, blaming RL that penalized response length. First stated reason for a delay.

September 13, 2026: with 4.7 still unreleased, Musk named its successor, Grok 4.8; a separate tracker opened.

September 14, 2026: Musk graded the model for the first time, âroughly on par with Opus 5.0, not 5.1â; no new date.

September 21, 2026, 16:17 UTC: **Grok 4.7 released.** Page flipped from tracker to release record on the same URL: spec and price table from the launch post and docs, xAIâs seven-row benchmark table and GDPval chart, the pre-launch claims graded (2.1T unconfirmed, SpaceX data absent, âexceed all current modelsâ not met, âslower to serveâ contradicted), the four dates closed (31 days from the first target), the cadence table closed at 40 days. The rumor-era hero replaced.

September 21, 2026, 19:05 UTC: first independent read folded. Artificial Analysis (16:38 UTC): Intelligence Index 46, rank 16 of 655, 81,000 output tokens per task against 36,000 for 4.6, about 188 tokens per second, hallucination rate 29 from 34 percent. Cursorâs post (16:22 UTC) confirms the 4.6 price and keeps 4.6 available. Grok Bot docs open on a âMeet grok-4.7â link, no running-Bot statement. New section, FAQ and watch list updated.

September 22, 2026, 01:30 UTC: distribution rows added. GitHub (changelog, September 21) has Grok 4.7 rolling out in Copilot; Vercel (changelog, September 21) serves it on AI Gateway, fx and eve at 40 percent off for a week. New watching item on the two prices.

September 26, 2026, 07:10 UTC: week-one section added. Arena added grok-4.7-xhigh to Code Arena: WebDev (September 22), Agent Arena (September 24) and Text Arena (September 25); Snorkelâs Senior SWE-Bench put it at 40.0 percent tasteful pass@3, sixth, $0.24 per trial; price rows for the xAI API, OpenRouterâs xAI endpoint and Fast; the Cursor Cloud Agent reports of September 22. Title now leads on benchmarks and price.

September 29, 2026, 02:15 UTC: Cyber Index section added. Artificial Analysisâs new Cyber Index (September 28) puts Grok 4.7 (xhigh) at 56 percent, tied first with MiMo-V2.6-Pro of 17 models; GPT-6 Astra, GPT-6 Sol, Fable 5.1 and Opus 5.5 refuse at least 98 percent of its patching tasks.

October 6, 2026, 16:09 UTC: VulcanBench section added. Its Frontier v4 board (updated October 4) puts Grok 4.7 through Cursor first, second and third (93.15 Extra-high, 92.71 High, 92.30 Medium), ahead of Fable 5.1 at max (91.84) and Opus 5.5 at high (91.11); GPT-6.1 Sol replaced Grok 4.6 as second judge for this column, the board calls it not strictly comparable, and on Muse Spark 1.3 alone 4.7 stays first at Medium to Extra-high. Cost per task unavailable. Musk shared it at 15:32 UTC.

## Â§ 17Sources

- 
Arena, leaderboard changelog, entries of September 22, 24 and 25, 2026. Snorkel AI, Grok 4.7 on Senior SWE-Bench, September 22, 2026. xAI, pricing and rate limits, read September 26, 2026. OpenRouter, Grok 4.7 endpoints, read September 26, 2026.
- 
xAI, Introducing Grok 4.7, September 21, 2026; read September 21, 2026, 16:29 UTC.
- xAI, Models and pricing, read September 21, 2026.
- @SpaceXAI, launch thread on X, September 21, 2026, 16:17 UTC; read in Chrome.
- Elon Musk, posts on X linked inline above, July 24 to September 21, 2026.
- xAI, Introducing Grok 4.6, August 12, 2026.
- Our own records: Grok 4.8 release date; What is Grok Bot; GLM-5.3-Flash and Ox Alpha. Artificial Analysis, Grok 4.7 (xhigh) and its launch-day thread, read September 21, 2026, 19:03 UTC; Cursor forum, Grok 4.7 is now live, 16:22 UTC; Grok Bot overview, read 19:03 UTC.
- GitHub, Grok 4.7 is now available in GitHub Copilot, September 21, 2026; read September 22, 2026.
- Vercel, Changelog, September 21, 2026 entries; read September 22, 2026.
- VulcanBench, Frontier v4 leaderboard, updated October 4, 2026; read October 6, 2026. @XFreeze, October 6, 2026, 10:17 UTC, and Elon Musk, October 6, 2026, 15:32 UTC, on X; read in Chrome.

### Q1When was Grok 4.7 released?

September 21, 2026. The x.ai/news post ‘Introducing Grok 4.7’ is dated that day; @SpaceXAI’s first post is id-clocked at 16:17:53 UTC; OpenRouter’s x-ai/grok-4.7 entry was created at 16:19 UTC; Musk posted at 16:25 UTC. The docs list grok-4.7 with a 500k context and a May 2026 knowledge cutoff.

### Q2What are the Grok 4.7 specs?

What xAI published: a new, larger base model than Grok 4.6, trained with a longer reinforcement learning run on a harder mix of many-hour tasks, better at verifying its own work and managing longer context, and trained to natively understand the Grok Bot harness. 500k context, May 2026 knowledge cutoff, encrypted reasoning content on the Responses API. No parameter count; the 2.1 trillion figure remains Musk’s July 28 claim, and the SpaceX engineering data he described is absent from the launch post.

### Q3Is Grok 4.7 better than Fable 5.1 or GPT-5.6 Sol?

Per xAI’s own table: against Fable 5.1 Max, 4.7 leads on EEBench (64.0 vs 56.4) and the Harvey legal benchmark (19.6 vs 6.7), ties within a point on DeepSWE (71.0 high-effort vs 70.0) and AA Briefcase (1,657 vs 1,678), and trails on CursorBench (46.3 vs 51.8), Terminal-Bench (38.0 vs 57.9), HealthBench (56.7 vs 62.1) and GDPval (1695 vs 1735 Elo). Against GPT-5.6 Sol Max it leads on CursorBench, EEBench, AA Briefcase and Harvey, trails on DeepSWE and HealthBench, and edges Terminal-Bench. At $2/$6 against Fable’s $10/$50 and Sol’s $4/$20, the price-performance claim is the one the table supports. On VulcanBench Frontier v4 (October 4), an outside coding board, it takes the top three places ahead of Fable 5.1 at max, with a judge swap the board says makes its score not strictly comparable.

### Q4Were the leaked Grok 4.7 benchmarks real?

There never were any. No sheet from xAI or a recognized evaluator named a tested Grok 4.7 before September 21; the posts titled ‘Grok 4.7 leaks’ from late July carried no files. Ox Alpha, the stealth model guessed to be 4.7 in August, was GLM-5.3-Flash. The first numbers are xAI’s own, published at launch, and this page carries them labeled as vendor numbers until an independent table exists.

### Q5Should I switch to Grok 4.7?

If you use Grok 4.6 today, the swap is a model-id change at the same price and the vendor table says you gain on every row. If you are choosing an engine, the honest read is a strong price-performance option with coding rows below Fable 5.1 Max. If you are choosing where your agents run, the model matters less than the layer around it: memory across working sessions, a task board, an inbox, approvals. CellCog does not route to Grok; it runs on Anthropic’s Claude models, and a standing AI employee keeps its role across every engine swap. You pay for the work, plus a monthly fee for each AI employee.
