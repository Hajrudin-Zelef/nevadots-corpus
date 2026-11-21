---
id: collect-261121-veille-ia/grok-xai/gemini-4-post-training-google-aims-to-ship-asap-2026-explainx-ai-blog
title: "Gemini 4 Post-Training: Google Aims to Ship ASAP (2026) | explainx.ai Blog"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: ["2026-10-01"]
keywords: ["gemini", "google", "post-training", "september", "kavukcuoglu", "explainx", "ship", "earlier"]
source: https://www.explainx.ai/blog/google-gemini-4-fast-track-koray-kavukcuoglu-post-training-2026
source_anchor: ""
source_lines: [1, 155]
section: "Grok_xAI"
---
Google is trying to **skip a generation in public** and land Gemini 4 while rivals already ship GPT-6 and Claude Mythos tiers. On September 24, 2026, new DeepMind chief **Koray Kavukcuoglu** told The Information that **Gemini 4 is in post-training** and that Google wants to release an early build **as soon as possible** — **much earlier than end of 2026**, though without a calendar date.

For builders on Vertex, AI Studio, or Antigravity, this is the first on-record confirmation that Google's answer to a missing Gemini 3.5 Pro is **jump straight to 4**.

**Update — October 1, 2026:** Google named the SKU. Gemini 4 Argon launched September 30 to Fairwind testers (1M output tokens, $2/$10 intro). Andon Labs has it **#3 on Vending-Bench 2**. It is not a public API model yet. Same-day Bloomberg reporting described internal skepticism on real-world coding versus benchmarks; Google disputed that framing — bench vs desk write-up.

**Update — September 27, 2026:** A wider set of 3D clips is circulating, and feed summaries are shortening his "much earlier than end of 2026" line into "before year's end." The demo catalog and that correction are in Gemini 4 arena demos. The floatplane deep-dive still covers the September 26 Opus comparison.

**Update — September 26, 2026:** Unofficial arena clips comparing a rumored **Gemini 4 Pro** checkpoint to **Claude Opus 5.5** on a **3D floatplane** prompt went viral on X (Pranav Reddy / @AI_Screening). Google has not confirmed the SKU; explainx.ai's floatplane leak deep-dive separates hype from evidence.

That strategy only works if post-training closes the gap OpenAI and Anthropic opened in September 2026. Google’s **Flash** lines stay competitive on cost and latency; the **flagship** story is what enterprise bake-offs still score. Kavukcuoglu’s “as soon as possible” line is Google signaling it will not cede another holiday season without a top-tier answer.

## TL;DR: Gemini 4 status

| Question | Answer | 
|---|---|
| Stage | Post-training (safety, guardrails, behavior refinement) | 
| Internal use | Powers Antigravity trials (per Kavukcuoglu) | 
| Public date | Not announced; faster than end of 2026 is the hint | 
| Skipped release | Gemini 3.5 Pro (teased, not broadly shipped) | 
| Last flagship | Gemini 3 Pro, November 2025 | 
| Rivals | GPT-6 Astra, Claude Mythos / Fable 5.1 (September 2026) | 
| Leadership context | Demis Hassabis stepped back; Brin hands-on on Gemini 4 per prior reporting | 

## What Kavukcuoglu actually said

Coverage from The Verge, 9to5Google, and Times of India quotes the same core line:

Our intention is to, like — as soon as possible — to release an early post-training output because we see the results and we are excited … [to] continue the fast-paced iterations.


He added Google hopes the model arrives **much earlier than the end of 2026**. That is executive optimism, not a schedule — but it is stronger language than Google's July 2026 blog post that merely said Gemini 4 pre-training had started (Gemini 3.6 Flash launch recap).

Post-training is where **safety evals, instruction tuning, and product guardrails** land. Kavukcuoglu noted internal testing already uses Gemini 4 inside **Antigravity**, Google's agentic coding surface — a hint that developer tooling may lead consumer marketing again.

## Why Gemini 3.5 Pro vanished

Google CEO Sundar Pichai promised a major **Gemini 3.5 Pro** update around I/O 2026; it never reached general availability. Kavukcuoglu framed the pivot as focusing on **Flash iterations** (recently Gemini 3.8 Flash and cyber-tuned variants) instead of shipping a Pro tier that could not win headlines.

Community and press speculation — that 3.5 Pro lost internal bake-offs to OpenAI and Anthropic — aligns with explainx.ai's earlier Gemini 3.5 Pro benchmark leak coverage, still unverified but directionally consistent.

Skipping 3.5 Pro is rational if 4's post-training curves look better on **Frozen v2 / TPU** infrastructure (pre-training chip story, Sergey Brin return). It is risky if enterprises waited on a Pro upgrade that never came — they may have already switched APIs.

## Release timeline: Gemini flagship cadence (2025–2026)

| Milestone | Approx. date | Notes | 
|---|---|---|
| Gemini 3 Pro GA | November 2025 | Last widely shipped Google flagship | 
| I/O 2026 tease | Mid-2026 | Gemini 3.5 Pro promised, not broadly GA | 
| July 2026 | Gemini 3.6 / 3.8 Flash, cyber variants | Volume and speed tier wins | 
| July 2026 blog | Gemini 4 pre-training started | Flash launch recap | 
| Sept 2026 rivals | GPT-6 Astra, Claude Mythos / Fable 5.1 | Enterprise eval churn | 
| Sept 24 2026 summit | Kavukcuoglu: Gemini 4 in post-training, ASAP ship | No calendar date | 
| Next likely beats | AI Studio preview, Antigravity internal → limited API | Historical Google pattern | 

Aggregator sites may float **October 2026** dates; Google did not confirm them on stage. Plan internally for **Q4 2026–Q1 2027** early access with wide error bars.

## What “post-training” means for Gemini 4

Kavukcuoglu used post-training in the **Google workflow sense**: everything after base pre-training that turns raw capability into a shippable product.

| Phase | Typical work | Builder-visible output | 
|---|---|---|
| Instruction / RLHF-style tuning | Follow system prompts, tool formats | Stable chat and function calling | 
| Safety classifiers and policies | Refusals, category blocks | Policy changes week to week in previews | 
| Red team and eval gates | Internal + possible external suites | Delay or scope cuts before GA | 
| Product integration | Search, Workspace, Antigravity | Feature-specific behavior | 
| Infra bake | Latency, batching on TPU / GPU | Vertex pricing and quotas | 

“Fast-track” in headlines means **calendar compression between phases**, not skipping them entirely — though external auditors may still lag industry SAFA-style standards.

## Leadership transition: Hassabis stepping back, Kavukcuoglu forward

Demis Hassabis remains a public face for Google DeepMind science; Kavukcuoglu’s September 24 stage time marks **operational ownership** of getting Gemini 4 out. Prior explainx.ai coverage on Hassabis’s frontier AI framework essay still describes **how Google talks about safety**; Kavukcuoglu’s quotes describe **when Google ships**.

Sergey Brin’s hands-on Gemini 4 push adds a third axis — founder-level attention on quality bars. For developers, the practical read is **more internal dogfooding before your keys** (Antigravity first), not a consumer marketing date.

## Flash-first strategy: why Google skipped 3.5 Pro

Google shipped multiple **Flash** iterations while 3.5 Pro stalled. That split strategy has clear tradeoffs:

| Strategy | Wins | Costs | 
|---|---|---|
| Flash-first | Cheap tokens, low latency, context caching wins | Loses flagship benchmark headlines | 
| Ship 3.5 Pro anyway | Narrative parity with rivals | Risk shipping a tier that loses bake-offs | 
| Jump to Gemini 4 | One flagship story, cleaner SKU line | Longer gap since Gemini 3 Pro | 
| Antigravity dogfood | Real agent trajectories before GA | Preview instability | 

If your app is **cost-bound**, staying on Gemini 3.8 Flash through Q4 2026 is rational. If you **sell** “best model” marketing, you need a Gemini 4 eval branch ready day one.

## Developer migration checklist (before GA)

1. **Pin eval suite:** 30–50 production-critical prompts + tool traces; store baseline scores on current Gemini 3.x.
2. **AI Studio watch:** Enable notifications for AI Studio preview docs — Google often documents breaking changes only there first.
3. **Vertex quotas:** Pre-request quota bumps for a parallel Gemini 4 canary project; launch-week throttling is common.
4. **Tool schema audit:** Revalidate function calling and JSON modes; post-training tweaks break strict parsers.
5. **Multimodal paths:** If you use image or audio, rerun those subsets — flagship upgrades sometimes regress secondary modalities temporarily.
6. **Policy overlay:** US-first access politics may affect**who** receives keys first even after post-training completes.
7. **Compare rivals same day:** Run identical harness against GPT-6 pricing context and Claude 5.1 benchmarks — do not migrate on Google blog claims alone.

## Antigravity as early signal

Kavukcuoglu said Gemini 4 already powers **Antigravity** internal trials — Google’s agentic coding surface. Historically, Google exposes developer-facing models **after** internal agent products stress-test tool use.

If you maintain a coding agent, track Google CC Family Agent Labs and Antigravity release notes as **leading indicators** for API stability, not as guarantees of GA parity.

## Benchmark hygiene when Gemini 4 drops

Use explainx.ai’s how to read AI benchmarks guide: public leaderboard jumps rarely match **your** RAG + tools stack. When Gemini 4 preview APIs appear, publish internally:

- Median latency at your context lengths
- Tool success rate on your MCP or function definitions
- Cost per successful task (tokens + retries)

World models and Odyssey sit on a different product axis — do not conflate Gemini 4 chat/agent GA with generative world-model releases.

## Competitive table for builders

| Model line | Status (late Sept 2026) | Typical builder use | 
|---|---|---|
| GPT-6 Astra | Shipped; Pro tier strained | Agents, coding, reasoning | 
| Claude Mythos / Fable 5.1 | Shipped; policy gates | Coding, enterprise, long context | 
| Gemini 3.x Flash | Shipped, cheap/fast | High-volume, latency-sensitive | 
| Gemini 4 | Post-training, no GA | Wait for eval rerun | 
| Grok 4.x | Rapid iteration | Alternative frontier tests | 

When Gemini 4 GA drops, rerun **your** harness — not LMArena screenshots. Compare against GPT-6 pricing context and Claude 5.1 launch benchmarks.

## What this means for what you build or pay

**Do not freeze on rumors.** October 2026 dates circulating on aggregator sites are **unconfirmed**. Ship with Gemini 3.8 Flash or stable 3 Pro where they clear your evals today.

**Prepare a same-day eval playbook.** Pin 30–50 production prompts, agent trajectories, and RAG queries now. When Google drops an early post-training API, run the suite within 24 hours — early builds change weekly.

**Watch Antigravity and AI Studio.** Google often exposes models to developers before consumer apps. Follow Gemini API docs in AI Studio preview patterns from prior releases.

**Policy overlay:** US-first testing politics may delay **who** gets keys first even when post-training finishes (White House access story).

## What people are asking

### Is Gemini 4 trained on Frozen v2 yet?

Pre-training started on existing infrastructure; Frozen v2 is the efficiency bet for inference-scale deployment. Google has not tied Kavukcuoglu's comments to a specific chip generation in the summit quotes we reviewed.

### Will Google ship Gemini 3.5 Pro after 4?

Kavukcuoglu did not revive 3.5 Pro. Treat it as shelved unless Google explicitly undeads it — betting on 4 is Google's public strategy.

### Does fast-track mean unsafe?

Post-training explicitly includes safety testing in Google's description. Fast-track means **product calendar**, not skipping eval — though external auditors may still lag SAFA-style standards (private safety body plans).

## Related on explainx.ai

*Quotes and stage descriptions reflect The Information AI Agenda Live Summit coverage September 24, 2026. Google may change release plans without notice.*
