---
id: collect-261121-veille-ia/gemini-google/hydrogen-carbon-or-argon-a-cost-first-guide-to-gemini-4-for-every-team
title: "Hydrogen, Carbon or Argon? A Cost-First Guide to Gemini 4 for Every Team"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["carbon", "gemini", "model", "argon", "cyber", "hydrogen", "google", "models"]
source: https://dev.to/gde/hydrogen-carbon-or-argon-a-cost-first-guide-to-gemini-4-for-every-team-205b
source_anchor: ""
source_lines: [1, 222]
section: "Gemini_Google"
---
In the last few weeks, Google shipped three new Gemini 4 models: Argon (GA October 13, 2026), Carbon (GA November 10), and Hydrogen (GA November 12). Then came cyber versions of two of them, plus a world model called Genie 4. The names come from the periodic table, so they tell you nothing about which one is bigger, faster, or cheaper. Hydrogen is the lightest element, but it is not a "lite" model in the old sense.

Cost sits at the center of this guide, because every role touches the bill. You'll find a section for each team: developers, SREs and platform engineers, security and compliance reviewers, engineering leaders and architects, and of course FinOps. Each one ends with your own cost lever: the one thing in your hands that moves the bill.

All facts come from the Gemini Enterprise Agent Platform model pages on Google Cloud (last updated November 13, 2026), the Agent Platform pricing page, and Google's launch posts.

| If you are... | Your big question | Your cost lever | 
|---|---|---|
| FinOps or finance | What will this cost us, and when does it change? | Routing policy and request labels | 
| Developer | What breaks when I switch models? | Thinking level on every call | 
| SRE or platform engineer | Where does it run, and how long can a call take? | Timeouts and region choice | 
| Security or compliance | What terms and access rules apply? | Approval path for Carbon and cyber models | 
| Engineering leader or architect | Which model should be our standard? | Default model and escalation rules | 

- 
**Gemini 4 Hydrogen (`gemini-4-hydrogen`):** the fast, cheaper workhorse for everyday coding and agent work. Google calls it the upgrade path from Gemini 3.8 Flash. Think of it as your default.
- 
**Gemini 4 Carbon (`gemini-4-carbon`):** Google's strongest model. It thinks the longest and spends the most thinking tokens. Use it for planning, deep debugging and hard root-cause work.
- 
**Gemini 4 Argon (`gemini-4-argon`):** the first frontier Gemini 4 model. Same per-token price as Carbon, but it uses fewer thinking tokens. It fits a narrow middle ground: jobs where Hydrogen falls short and Carbon is too slow.
- 
**Cyber variants (allowlist only):** special builds of Argon and Carbon for defensive security work. Most teams will never touch these.
- 
**Genie 4:** not a text or code model at all. It's a world model in research preview. Skip it for app building.

The cyber models are versions of Argon and Carbon tuned for defensive security work: finding vulnerabilities, reverse engineering, verifying exploits and writing patches. You can't just turn them on. Access is allowlist only, through your Google team or the Fairwind Program.

|  | Gemini 4 Argon Cyber Permissive | Gemini 4 Carbon for cyber | 
|---|---|---|
| Model ID | `gemini-4-argon-cyber-permissive` | `gemini-4-carbon-cyber-permissive` | 
| Based on | Gemini 4 Argon | Gemini 4 Carbon | 
| Default thinking level | HIGH (normal Argon is MEDIUM) | HIGH (normal Carbon is MEDIUM) | 
| Computer use | Not supported | Not supported | 
| Regions | global, us, eu | global, us | 
| Price, global, per 1M tokens in/out | $4 / $20 (no intro promo) | $4 / $20 (no intro promo) | 
| Terms | Section 31 of Service Specific Terms | Section 31 of Service Specific Terms | 

Three things to watch:

- 
**The naming is not consistent.** The Carbon version is called "Carbon for cyber" in the docs, but its model ID still ends in "cyber-permissive". Use the model ID, not the display name, in your code and docs.
- 
**Access does not carry over.** If you already have Argon Cyber Permissive or Gemini 3.8 Flash Cyber, you still need to apply again for Carbon for cyber.
- 
**Higher default thinking means higher token spend.** Both cyber models default to HIGH, so expect more output tokens per call than the base models.

On Google Cloud the three base models look almost the same on paper. The real differences are thinking behavior, price, regions, file types and who can get access.

|  | Hydrogen | Argon | Carbon | 
|---|---|---|---|
| Model ID | `gemini-4-hydrogen` | `gemini-4-argon` | `gemini-4-carbon` | 
| GA date | Nov 12, 2026 | Oct 13, 2026 | Nov 10, 2026 | 
| Best for | Everyday coding, agents, execution | Long tasks, fewer thinking tokens than Carbon | Deepest reasoning, planning | 
| Thinking levels | LOW, MEDIUM, HIGH (default), XHIGH | LOW, MEDIUM (default), HIGH | LOW, MEDIUM (default), HIGH | 
| Output per segment | 65,536 (streaming or not) | 65,536 streaming, 32,768 non-streaming | 65,536 streaming, 32,768 non-streaming | 
| Office and OpenDocument files | Yes | No (PDF and text only) | Yes | 
| Regions | global, us, eu | global, us, eu, europe-west2, asia-southeast1 | global, us, eu | 
| Access | Public | Public | Verified enterprise customers, select regions | 
| Knowledge cutoff | March 2026 | Not listed | March 2026 | 
| Google Cloud price, global, per 1M tokens in/out | $1.50 / $7.50 | $2 / $10, then $4 / $20 from Dec 1 | $2 / $10, then $4 / $20 from Dec 1 | 

Prices are Standard PayGo on the global endpoint. Regional (non-global) endpoints cost 10% more on every model.

What all three share: a 1,048,576-token limit that covers input and output together, long decodes up to that limit, implicit caching only (4,096-token minimum), fixed sampling settings, no Live API, no tuning and no RAG Engine. All three support grounding, function calling, code execution, URL context, computer use, batch, Provisioned Throughput, Standard PayGo and Priority PayGo.

The biggest update: Google says Hydrogen at its default HIGH setting slightly outscores Argon on the Artificial Analysis Intelligence Index (51 vs 50, and 54 at XHIGH), at a lower price. If that holds on your workloads, Argon's role gets even smaller.

| Date | What happened | Why it matters | 
|---|---|---|
| Nov 13, 2026 | Google Cloud starts billing long completions (past 32K tokens) even when they fail on a timeout | Timeouts now cost money, not just time | 
| Nov 12, 2026 | Hydrogen GA, replacing Gemini 3.8 Flash; now powers the Gemini app and AI Mode in Search | Google says it's 2.5x faster than 3.8 Flash at its default HIGH level | 
| Nov 12, 2026 | Orchestrator in Google Antigravity: Carbon plans, Hydrogen executes, in one session | Google reports near-Carbon success with 24% less quota and 19% faster runs on Terminal-Bench 4.0 | 
| Nov 10, 2026 | Carbon GA, plus a /swarm preview in Antigravity that turns one prompt into a team of agents | Parallel agents multiply token use, so watch spend | 
| Oct 13, 2026 | Argon GA | First Gemini 4 frontier model | 

Dates coming up:

| Date | What changes | 
|---|---|
| Nov 30, 2026 | Intro credits for Argon and Carbon end | 
| Dec 1, 2026 | Argon and Carbon go to $4 / $20 per 1M tokens (global) | 
| Jan 1, 2027 | Gemini 3.8 Flash doubles to $1.50 / $7.50 on the Gemini API, the same as Hydrogen | 
| Jan 12, 2027 | Hydrogen gets cache control with TTL; cache writes at $1.875 per 1M tokens | 
| Oct 13, 2027 or later | Argon's listed retirement on Google Cloud | 

The headline: from December 1, 2026, Argon and Carbon cost about 2.67 times more per token than Hydrogen. Model choice is now one of the biggest levers on your AI bill.

Here is a simple example. One request with 20,000 input tokens and 3,000 output tokens, at Google Cloud Standard PayGo prices, per 10,000 requests:

| Model | Global endpoint | Non-global endpoint | 
|---|---|---|
| Hydrogen | about $525 | about $578 | 
| Argon or Carbon, until Nov 30 | about $700 | about $770 | 
| Argon or Carbon, from Dec 1 | about $1,400 | about $1,540 | 

This keeps token counts equal, which real traffic won't. Carbon spends more thinking tokens than Argon, so the real gap is often bigger.

Three details hide in the footnotes of the Google Cloud pricing page:

- 
**The intro price is a credit, not a lower rate.** Argon and Carbon get 50% credits back on net spend through November 30, 2026. Your invoice may show full price with a credit line, so warn your finance team.
- 
**Cyber models get no promo.** Argon Cyber Permissive and Carbon Cyber Permissive cost $4 / $20 per 1M tokens from day one.
- 
**Failed long calls still bill.** Since November 13, 2026, any completion past 32K tokens is charged for the tokens that can be retrieved, even when it fails from a client or server timeout.

Where the money hides:

- 
**Thinking tokens are billed as output.** Output costs 5 times input. A model that thinks more costs more, even on the same prompt.
- 
**Hydrogen's HIGH default.** Teams moving from 3.8 Flash (which defaulted to MEDIUM) may see a jump. Start at MEDIUM and compare on your own test set.
- 
**Caching changes.** All three use implicit caching only. Put stable content (system instructions, reference docs) at the start of the prompt so cache hits happen. From January 12, 2027, Hydrogen gets cache control with TTL, with cache writes at $1.875 per 1M tokens on global (1.25 times input). Cache reads are cheap on every model: 5% of the input price, or $0.075 per 1M tokens on Hydrogen.
- 
**Use batch for non-urgent work.** It has its own lower rates on the Batch tab of the pricing page.
- 
**Re-test before December 1.** If you picked Argon in October, check whether Hydrogen now does the job.
- 
**Tag your requests.** Use request labels by model and use case so you can see which team or feature drives Carbon spend.

**Your cost lever:** the routing policy and the labels. Default to Hydrogen, track Carbon spend by team, and re-test anything on Argon or Carbon before December 1.

Switching to Gemini 4 is more than changing the model string. A few old habits now break or silently stop working.

- 
**No more temperature tuning.**`temperature` ,`top_p` and`top_k` are ignored. They are fixed at 1.0, 0.95 and 64. If your prompts relied on low temperature for steady output, test again.
- 
**`thinking_budget` and MINIMAL are gone.** Sending`thinking_budget` , or`thinking_level` set to MINIMAL, returns an HTTP 400 error. Use thinking levels instead.
- 
**Hydrogen thinks harder by default.** Its default is HIGH, while Argon and Carbon default to MEDIUM. Set the level on purpose. Don't just accept the default.
- 
**XHIGH is Hydrogen only.** Save it for hard multi-step problems. On simple jobs like extraction it can overthink.
- 
**Long answers need long decodes.** One segment stops at 65,536 output tokens (32,768 for Argon and Carbon when not streaming). The Gen AI SDK and Agent Studio continue automatically. A custom loop needs`continuation_token` , which only works on the v1beta1 API for now.
- 
**Office files work on Hydrogen and Carbon.** You can send .docx, .pptx, .xlsx and OpenDocument files directly, no conversion. Hydrogen even accepts them inside function responses. Argon still takes only PDF and plain text.
- 
**Planner plus executor works well.** Google shows Carbon doing the planning turns and Hydrogen doing the execution turns in one session, linked with`previous_interaction_id` . This is a natural fit for multi-agent apps.

**Your cost lever:** set the thinking level on every call, and log thinking tokens per feature. Don't let Hydrogen's HIGH default decide your bill.

For reliability, the big questions are where the model runs, how you pay for capacity and how long a call can take.

- 
**Regions decide a lot.** Argon is the only one with single regions (europe-west2 and asia-southeast1). Hydrogen and Carbon run only on global, us and eu. If your data must stay in London or Singapore, Argon is your only Gemini 4 option today.
- 
**Carbon is gated.** Google limits it to verified enterprise customers in select regions, to protect capacity. Don't build a production path on Carbon until your project is approved.
- 
**No fixed quota.** All three run on Standard PayGo with usage tiers, Priority PayGo, Provisioned Throughput or batch. Plan capacity with Provisioned Throughput for steady, critical traffic.
- 
**Expect long calls.** HIGH and XHIGH thinking plus long decodes can run for minutes. Raise client and gateway timeouts, and stream where you can. Timeouts now cost money too: since November 13, 2026, a completion that runs past 32K tokens is billed for what can be retrieved, even if the call fails on a timeout.
- 
**Big inputs are allowed.** Up to 500 MiB compressed on GenerateContent, 3,000 images or documents per prompt, about 45 minutes of video with audio, and around 8.4 hours of audio. Watch your request size limits upstream.
- 
**Plan for retirement.** Argon's listed retirement is October 13, 2027 or later. Put the model ID in config, not in code, so you can swap it.

**Your cost lever:** timeouts and region choice. A timed-out call past 32K tokens still bills, and non-global endpoints cost 10% more. Use global unless data residency says otherwise, and keep Provisioned Throughput for steady traffic you can predict.

For security teams, Carbon and the cyber models come with extra terms. Read them before you sign off.

- 
**Carbon is a "Google Frontier Model".** It falls under Section 34 of the Service Specific Terms, which includes abuse monitoring. Your legal team should review what that means for your prompts and data.
- 
**Cyber models are "Frontier Cyber Models".** They fall under Section 31 and need allowlist approval. Plan for an approval step in your project timeline.
- 
**Data residency depends on region.** All three list security controls for data residency, CMEK, VPC-SC and Access Transparency on the model page. Confirm which ones apply to your region and use case before go-live.
- 
**Free tier prompts may be used by Google.** On the Gemini API (not Google Cloud), free-tier Hydrogen prompts may be used to improve Google's products. Paid tier is not. For company data, use Google Cloud or a paid tier.
- 
**Computer use is off for cyber models.** The base Argon, Carbon and Hydrogen support it, but the cyber versions do not.

**Your cost lever:** the approval path. Approve the cheapest model that meets policy. Make Carbon and the cyber models (both $4 / $20 per 1M tokens with no promo for cyber) a named exception, not a default.

Your job is to set one simple standard so every team doesn't pick its own model.

- 
**Pick a default and an escalation rule.** Hydrogen as the default. Carbon only for named use cases, like planning and deep debugging. Argon only when an eval proves it beats Hydrogen for that job.
- 
**Design for mixing models.** The Carbon-plans, Hydrogen-executes pattern is now built into Google Antigravity's Orchestrator. Google reports near-Carbon results at 24% less quota. Build your own agents the same way.
- 
**Watch out for swarms.** Antigravity's /swarm preview runs many agents in parallel. Great for big tasks, but each agent burns tokens. Set budgets before teams try it.
- 
**Plan access early.** Carbon needs enterprise verification, and cyber models need allowlist approval. Start those requests now if you need them next quarter.
- 
**Keep model IDs in config.** Three Gemini 4 models and two cyber versions shipped in about a month. Expect more changes, and make swapping a config change, not a code change.

**Your cost lever:** the default model. Whatever you set as the standard is what most of your traffic will use.

Start cheap and move up only when your tests say so. Walk through these in order and stop at the first match.

1. 
**Is it defensive security work (vulnerability research, patching) and are you allowlisted?** Use Carbon for cyber or Argon Cyber Permissive.
2. 
**Must the data stay in a single region like europe-west2 or asia-southeast1?** Use Argon. It's the only Gemini 4 model there.
3. 
**Is it everyday work: coding, chat, extraction, summaries, agent steps?** Use Hydrogen. Set the thinking level yourself, starting at MEDIUM.
4. 
**Is it a hard, high-stakes thinking job: planning a big change, deep debugging, complex analysis?** Use Carbon, if your project has access.
5. 
**Does Hydrogen fall short but Carbon is too slow?** Try Argon, and prove it on your own eval set.
6. 
**Is it a multi-step agent?** Mix them. Carbon plans, Hydrogen does the work.

When in doubt, the rule of thumb is simple: Hydrogen by default, Carbon by exception, Argon by evidence.

- 
**Prices here are Google Cloud Standard PayGo list prices.** Priority PayGo, Batch, Provisioned Throughput and any discounts your company has will change the math. Check the pricing page for your real rates.
- 
**The cost example is a simple model.** It holds token counts equal across models. Measure real thinking tokens on your own traffic.
- 
**Benchmarks are vendor reported.** Run your own evals before you commit.
- 
**This is moving fast.** Argon, Carbon and Hydrogen all shipped within a month. Prices, regions and access rules can change. Everything here is as of November 14, 2026.

## Top comments (2)

Routing policy and request labels are the right FinOps levers because they make cost attributable before it becomes an invoice surprise. I’d pair them with a quality floor: sample task classes, define an acceptable success rate or reviewer-correction rate, and alert when a cheaper route silently increases rework. The useful cost metric is cost per accepted outcome, not tokens in isolation.

Yo os puedo decir algo con respecto a estos temas de que si son peras o son manzanas, o si naranjas 0 ROI. O si se trata de neto robo hormiga al opensource o al badaje de la Humanidad. Si es que hay alguien de google ahi leyendo estas notas. La gente de a pie no es que andemos sobrados de dinero pero si ok podriamos pagar una mensualidad para usar un modelo mas capaz y competente, pero el problema es que las companias piden el numero de cuenta, y eso (tal como esta el cibercrimen y las estafas)claro que no es prudente pedirselo al usuario, ya bastantes datos y metadatos extraen del usuario. Si propusieran otra forma de pago, como por ejemplo eso que hacen las loterias en los estancos que vas a la tienda y pides un ticket con un codigo, previo paga clora de la mesualidad de 10 o 15 o 18 o 20 dolares para usar un buen modelo de frontera ;) que te permita crear cosas bonitas y utiles, sin meter tus datos ni una tarjeta de banco, y asi vuelves a tu terminal con tu codigo y metes el codigo o la clave, y listo tienes ya un buen rato de uso sin limites para todo el mes. Asi daria mas confianza en vez de usar los metodos habituales y los contratos opacos. O como lo ve vuestra merced?

Aloha, gracias por tanto. La IA es una maravilla :)
