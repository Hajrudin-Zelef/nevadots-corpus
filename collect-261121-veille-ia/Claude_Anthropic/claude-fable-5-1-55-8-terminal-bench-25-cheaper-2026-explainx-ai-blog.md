---
id: collect-261121-veille-ia/claude-anthropic/claude-fable-5-1-55-8-terminal-bench-25-cheaper-2026-explainx-ai-blog
title: "Claude Fable 5.1: 55.8% Terminal-Bench, 25% Cheaper (2026) | explainx.ai Blog"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-09-25"]
keywords: ["fable", "claude", "mythos", "anthropic", "same", "model", "verification", "program"]
source: https://www.explainx.ai/blog/claude-fable-5-1-mythos-5-1-launch-benchmarks-pricing-2026
source_anchor: ""
source_lines: [1, 248]
section: "Claude_Anthropic"
---
**Update — September 25, 2026:** refusal billing rules changed for Fable and Opus models. See Billable blocks explained.

Anthropic shipped two models in one announcement on **September 1-2, 2026**: Claude Fable 5.1, available to every API customer today, and Claude Mythos 5.1, the same model with select safeguards lifted for a narrow, vetted set of cybersecurity and life-sciences users. It's the most detailed Fable-series release yet — doubled science benchmarks, a 75% cut to cache-read pricing, a new enterprise privacy architecture, and an explicit fix for the verbose, jargon-heavy writing style that's dogged Claude models since Fable 5 launched in June.

This piece unpacks the actual numbers from Anthropic's announcement and platform docs — not just the press release framing — plus what changed for people already running Fable 5 in production, and how to get the most out of the 5.1 upgrade without rewriting your prompts from scratch.

## TL;DR

| Question | Answer | 
|---|---|
| **What shipped?** | Claude Fable 5.1 (general availability) and Claude Mythos 5.1 (trusted access only) — same model, different safeguard levels | 
| **API model IDs** | `claude-fable-5-1` and`claude-mythos-5-1` | 
| **Biggest benchmark jump** | Terminal-Bench-Science 0.1: 52.6% vs Fable 5's 24.7% — more than double | 
| **Coding benchmark** | Terminal-Bench 4.0: Fable 5.1 55.8% vs Fable 5's 42.0% vs Mythos 5.1's 60.9% | 
| **Independently confirmed** | CursorBench 3.2.0: 73.4% at max effort — Cursor's own post calls it their best-scoring model, shipped day one | 
| **Pricing change** | Input/output unchanged ($10/$50 per MTok); cache reads cut 75%, from $1.00 to $0.25/MTok | 
| **Net cost impact** | ~25% cheaper for typical workloads, up to ~45% for highly agentic work, per Anthropic | 
| **Context window** | 1M tokens (default and max), 128K max output tokens | 
| **New enterprise feature** | Enterprise Frontier Safeguards (EFS) — misuse detection with data kept on customer cloud infrastructure, rolling out fall 2026 | 
| **Mythos 5.1 access** | Project Glasswing participants only, via Cyber Verification Program (CVP) or Life Sciences Verification Program (LSVP) | 
| **Usage limits** | Anthropic reset 5-hour and weekly limits for all users at launch, per @ClaudeDevs | 

## Fable 5.1 vs Mythos 5.1: same weights, different guardrails

The core framing from Anthropic's launch post is unusually direct: Fable 5.1 and Mythos 5.1 are **the same model** with different levels of safeguards applied on top. This isn't a new pattern — Anthropic ran the identical split for Fable 5 and Mythos 5 in June — but 5.1 sharpens where the line sits.

|  | Claude Fable 5.1 | Claude Mythos 5.1 | 
|---|---|---|
| Availability | All customers, Claude API and partner platforms | Project Glasswing participants only | 
| API model ID | `claude-fable-5-1` | `claude-mythos-5-1` | 
| Cybersecurity safeguards | Full — can discover vulnerabilities defensively, not develop exploits | Reduced, for approved cyber-defense work | 
| Access route | Sign up and use today | Cyber Verification Program (CVP) or Life Sciences Verification Program (LSVP) | 
| Terminal-Bench 4.0 | 55.8% | 60.9% | 
| Pricing | Same as Fable 5.1 | Same as Fable 5.1 | 
| Context window | 1M tokens | 1M tokens | 

Mythos 5.1 is explicitly built for two verified-access programs: the **Cyber Verification Program**, which today gives vetted defenders access to Opus- and Sonnet-class models and will extend to Mythos-class models, and the **Life Sciences Verification Program**, which enrolled its first participants through a US government partnership and plans to expand to the broader life-sciences community. Anthropic's own Claude Security product is now also powered by Mythos 5.1 for codebase vulnerability scanning and patch suggestion.

Same weights, different guardrails. Sort what changes:

## Benchmark results: what actually moved

Anthropic published a wide benchmark spread, and the gains are not uniform — some nearly doubled, others improved by single digits. Here's the full table pulled from the announcement:

| Benchmark | Fable 5 | Fable 5.1 | Mythos 5.1 | What it measures | 
|---|---|---|---|---|
| Terminal-Bench-Science 0.1 | 24.7% | **52.6%** | — | Agentic scientific research | 
| Terminal-Bench 4.0 | 42.0% | **55.8%** | 60.9% | Agentic coding | 
| CursorBench 3.2.0 | 70.5% | **73.4%** | — | Real-world coding tasks in Cursor | 
| GDPval-AA v2 | 1723 | **1853** | — | Knowledge work quality | 
| OSWorld 2.0 (partial credit) | 72.9% | **77.9%** | — | Computer use | 
| OSWorld 2.0 (strict) | 36.1% | **41.7%** | — | Computer use, strict scoring | 
| Humanity's Last Exam (no tools) | 57.8% | **60.9%** | — | Multidisciplinary reasoning | 
| Humanity's Last Exam (with tools) | 63.8% | **65.0%** | — | Multidisciplinary reasoning, tool use | 
| AutomationBench | 17.1% | **31.4%** | — | Business workflow automation | 

Two things stand out. First, the Terminal-Bench-Science jump — more than double Fable 5's score — is the single largest gain on the sheet, which lines up with Anthropic leading its own announcement with scientific research capability rather than coding alone. Second, the GPT-5.6 Sol comparison context matters here: on Terminal-Bench 4.0, GPT-5.6 Sol previously scored 52.3%, which put it ahead of Fable 5's 37.3% at the time (Anthropic's cited baseline in some comparisons) but now trails Fable 5.1's 55.8% and further behind Mythos 5.1's 60.9%.

The CursorBench number is the one worth trusting most, because it's independently confirmed rather than self-reported. Cursor's own account posted on X:

"Claude Fable 5.1 is now available in Cursor! It's the most capable model we've run on CursorBench 3.2, scoring 73.4% at max effort. We found it especially skilled at verifying its own work, allowing it to take on difficult coding tasks from start to finish."


Fable 5.1 shipped in Cursor on day one — the same rapid-adoption pattern seen with prior Fable releases, and a reasonable signal that the benchmark gains translate into real coding-agent behavior rather than being an artifact of Anthropic's own eval harness.

### Update — September 3, 2026: the Code Arena WebDev leaderboard has been volatile

A widely circulated headline claims Fable 5.1 hit 1,765 points to top Arena.ai's **Code Arena: WebDev** leaderboard with a 77-point lead. We tried to independently verify that specific figure and could not confirm it directly — what we found instead is a genuinely fast-moving leaderboard with several different snapshots reported by different outlets within the same week: Claude Fable 5 (the prior version) at 1,627 points, Alibaba's Qwen3.8-Max-0902 reported taking the #1 spot at 1,691 points, and Claude Opus 5 separately reported at 1,702.9 as the leader under an earlier vote cutoff. None of these independently line up with a confirmed 1,765/77-point Fable 5.1 result at time of writing.

The honest read: Code Arena's WebDev leaderboard updates frequently enough that "who's #1" is a moving target even within a single week, and headline point totals circulating across aggregators don't always match what Arena.ai's own live leaderboard shows at the moment you check it. If you're using this leaderboard to inform a model choice, check Arena.ai's live WebDev leaderboard directly rather than trusting a specific cited point total from any single article, including this one — by the time you read this, the standings have very likely moved again.

## Pricing: what actually got cheaper

Input and output pricing did not change: $10 per million input tokens, $50 per million output tokens — identical to Fable 5. The entire cost story is in prompt caching:

| Price component | Fable 5 | Fable 5.1 | Change | 
|---|---|---|---|
| Base input | $10 / MTok | $10 / MTok | No change | 
| Output | $50 / MTok | $50 / MTok | No change | 
| 5-minute cache write | $12.50 / MTok | $12.50 / MTok | No change | 
| 1-hour cache write | $20 / MTok | $20 / MTok | No change | 
| **Cache read** | **$1.00 / MTok** | **$0.25 / MTok** | **75% cut** | 

Anthropic's platform docs frame this precisely: cache reads on Fable 5.1 and Mythos 5.1 cost 0.025x the base input price, versus 0.1x on every other current Claude model. For a long-running agentic session that repeatedly re-reads a large cached system prompt or codebase context — the exact workload Fable-class models target — that difference compounds fast. Anthropic's own estimate is **roughly 25% cheaper for typical workloads** and **up to roughly 45% cheaper for highly agentic work** that leans hard on cache hits. This is an API-only change; Anthropic has not confirmed it applies to Claude Pro/Max/Team subscription plan allowances the same way, and past rate-limit resets suggest subscription economics are governed separately from API token pricing.

Batch processing pricing is unchanged: $5/MTok input, $25/MTok output.

## How to get the most out of Fable 5.1

Anthropic engineer Lance Martin (@RLanceMartin) and Anthropic's own prompting guide for Fable 5.1 converge on the same practical advice: don't assume your Fable 5 prompt setup is still optimal.

**1. Re-run your effort sweep — low effort punches above its cost.** Anthropic's own docs state it directly: "At `low`, Claude Fable 5.1 is often competitive with Claude Opus and Claude Sonnet models on cost per task while scoring higher." On CursorBench 3.2.0, Fable 5.1 at low effort matches Fable 5 at high effort for roughly a third of the cost. Don't assume the effort level that worked on Fable 5 is still the right one — effort levels don't map 1:1 across model versions.

**2. Change effort mid-conversation without breaking your cache.** This is new in 5.1 and solves a real pain point: previously, switching effort levels mid-session invalidated the prompt cache. Now you can raise effort for a hard step and drop it for routine ones in the same conversation:

```
curl https://api.anthropic.com/v1/messages \
  -H "anthropic-beta: mid-conversation-output-config-2026-07-01" \
  -d '{
    "model": "claude-fable-5-1",
    "output_config": {"effort": "high"},
    "messages": [
      {"role": "user", "content": "Plan a migration in three short steps."},
      {"role": "assistant", "content": "1. ... 2. ... 3. ..."},
      {"role": "system", "content": [], "output_config": {"effort": "low"}},
      {"role": "user", "content": "Summarize the plan in one sentence."}
    ]
  }'
```
**3. Check your cache hit rate in Claude Console.** With cache reads 4x cheaper ($1.00 → $0.25/MTok), a workload with high cache reuse is worth revisiting — the savings only materialize if your integration is actually structured to hit the cache (append-only conversation history, stable system prompt and tool definitions).

**4. Strip verification rituals and stale few-shot examples.** Prompts built up over months of patching around older model quirks — "double-check your work," heavy few-shot scaffolding, emphasis boosters like ALL CAPS instructions — often actively hold Fable 5.1 back rather than help it. Anthropic's migration guidance recommends simplifying rather than layering new instructions on top of old ones.

**5. Tell it not to write "mannered prose."** This is the most concrete fix in the whole release for a complaint that's been documented extensively on Hacker News — verbose Claude-isms like "load-bearing" and "the honest answer is." Anthropic's prompting docs name the anti-pattern directly and give a literal instruction to paste into your prompt:

```
Mannered prose substitutes metaphor and flourish for direct statement.
Instead of "a parameter worth varying," the mannered writer produces
"a dial worth turning." Instead of "this point still matters," they
write "this point earns its keep." The phrases exist to display the
writer, not to convey the idea, and readers can tell. That is why
mannered prose irritates: it makes the reader work harder so the
writer can perform. It is also imprecise. Metaphors drag in
connotations the writer did not choose and cannot control. The fix
is to say what you mean. When a literal phrase is available, use it.
```
Anthropic's own docs concede the fix isn't free: Fable 5.1's prose is "in some cases denser" than Fable 5's, with longer sentences and fewer paragraph breaks. Fewer stock phrases doesn't automatically mean shorter output — pair the instruction above with an explicit ask for paragraph breaks if density becomes a problem.

## Writing quality: addressing a real complaint

The writing-style change deserves its own section because it's not cosmetic — it's Anthropic responding to a specific, widely aired practitioner grievance. Anthropic's own framing: Fable 5.1's writing is "a step up," with "fewer stock phrases and less unexplained jargon," while acknowledging the trade-off toward denser prose in places. This tracks with a long-running thread of developer complaints about Claude's tendency toward filler phrasing and unexplained jargon in technical output — exactly the pattern the "mannered prose" instruction above targets. The fact that Anthropic now ships this as an explicit, quotable anti-pattern in official prompting docs (rather than leaving it to third-party prompt engineering blogs) signals it's being treated as a genuine product issue, not a matter of taste.

## A real-world demo: designing and rendering a house from a photo

The clearest illustration of what "agentic coding" now means in practice came from Anthropic's own Alex Albert (X, Sept 2, 2026), who gave Fable 5.1 a single photo of an empty property lot and asked it to design a house for it. Working entirely through code — driving Blender headless, with no GUI in the loop — Fable 5.1 designed a house for the lot, modeled and rendered it, and produced a finished cinematic walkthrough video, all from one prompt and one image.

This is worth calling out separately from the benchmark tables above because it's a concrete example of the "long-running, verification-heavy" work Anthropic's benchmark claims describe in the abstract: the task chains multiple distinct skills (spatial reasoning from a photo, procedural 3D modeling, a scripting interface it wasn't purpose-built for, and camera/lighting choices for the final render) into one unattended run. Commenters asked the obvious practitioner question — how many tokens or how long did this actually take? — which Albert didn't answer in the thread, so treat the demo as illustrative of capability, not as a benchmarked cost figure you can plan a budget around.

## Enterprise Frontier Safeguards: privacy without giving up misuse detection

The second half of the launch — detailed in a separate Anthropic post — addresses a tension enterprise customers have pushed back on for a while: zero data retention (ZDR) means Anthropic can't see anything, which also means it can't detect misuse inside an account. **Enterprise Frontier Safeguards (EFS)** is Anthropic's answer.

Under EFS, the data needed for automated misuse detection — pattern-based, without human review by default — stays on **the customer's own cloud infrastructure**: Amazon S3, Azure Blob Storage, or Google Cloud Storage, encrypted with customer-managed keys and governed by the customer's own access policies and audit logging. Anthropic gets alerts to review rather than raw access to the underlying data.

- **Rollout:** phased, starting fall 2026, with broader availability targeted for later in the season
- **Built with:** more than 100 enterprise customers during development
- **Supported surfaces:** Claude Code, Claude Enterprise, the Claude Platform, Amazon Bedrock, Claude Platform on AWS, Google's Agent Platform, and Microsoft Foundry
- **Bridge period:** eligible customers get ZDR on Fable 5 and 5.1 today, ahead of EFS's full rollout

It's a meaningful architectural shift, not just a policy tweak: Anthropic is choosing to build misuse detection as infrastructure the customer controls rather than data it collects centrally, which is a materially different trust model than most API providers offer.

## Anti-distillation: closing the thinking-block loophole

Anthropic's announcement also confirms a change first flagged in the distillation coverage from July: editing earlier turns in a conversation now invalidates Fable 5.1's thinking blocks, enforced for **API accounts created on or after August 31, 2026**. Existing accounts are unaffected for now, but the direction is clear.

Concretely, per Anthropic's platform docs, these actions now break the chain and either error out or silently drop invalidated blocks:

- Editing, reordering, or removing an earlier turn while keeping later ones
- Injecting per-request text into an earlier turn that gets deleted on the next request
- Rebuilding the top-level `system` prompt or`tools` array between requests

This closes off exactly the kind of black-box distillation technique explainx.ai covered in the proxy-KD analysis — a technique that relied on manipulating conversation history while keeping the model's exposed reasoning transcript intact. Server-side context editing and compaction don't count as edits and keep the cache warm, so legitimate long-session workflows aren't affected — only manual, out-of-band history rewriting is.

## The scientific research claims

Anthropic leaned unusually hard into original research output as evidence of capability, not just benchmark scores:

- **Protein design:** Mythos 5.1 designed high-affinity protein binders that Anthropic says bound with affinities roughly 10x higher than the best entries in Adaptyv Bio design competitions, across three separate targets. Across 12 targets tested, the hit rate reached nearly 50% — well above the typical 10-15% hit rate for this kind of computational protein design. All reported designs were confirmed to bind in the lab.
- **Venus elevation mapping:** Fable 5.1 trained a neural network on 30-year-old NASA Magellan radar data to produce a new, higher-resolution elevation map covering roughly a third of Venus, improving resolution from the original 10-20 km to 2-3 km and height accuracy by up to 25%. Anthropic released the map under a Creative Commons license ahead of NASA's VERITAS and ESA's EnVision missions to the planet.
- **Genomics kernel optimization:** Mythos 5.1 wrote custom GPU kernels that sped up seven open-source genomics and protein deep-learning models by up to 2.5x, cutting compute costs 30-60% on genome-wide analyses, and compressed optimization work that normally takes weeks into days.

These are Anthropic's own reported results, not independently peer-reviewed at the time of writing — treat them as a capability demonstration from the model's creator rather than settled science, though the protein-binder lab-confirmation claim in particular is a concrete, checkable outcome rather than a benchmark number.

## Safety and safeguard changes

- **Cybersecurity safeguards updated** to allow Fable 5.1 to be used for discovering software vulnerabilities defensively — but not for developing exploits. Anthropic reports roughly 60% fewer false-positive interventions overall, and Claude Code users specifically should see about 60% fewer per-session interventions.
- **Biology safeguards fire 85% less often** for benign elementary biology and medical questions, addressing a long-standing complaint about over-triggering on harmless queries — a theme explainx.ai tracked in August's biology safeguards update.
- **No critical-severity jailbreak** was found in Anthropic's external testing round for this release.
- **Agentic safety:** Anthropic calls Fable 5.1 its most robust model to date against an external prompt-injection benchmark, and says it's significantly less likely than Mythos 5 to attempt unauthorized resource access during agentic tasks.

## EU AI Act watermarking

Text generated by Fable 5.1 and Mythos 5.1 now carries an **invisible, statistical watermark** on every platform where the models are available — not a visible logo, but a cryptographic signal detectable only through Anthropic's detection API. This is required under the EU AI Act's Code of Practice on Transparency of AI-Generated Content, which Anthropic signed in July 2026 alongside 190+ other signatories, and applies to any Anthropic model released after August 2, 2026. The detection API is in private preview, currently limited to eligible organizations such as regulators, law enforcement, media outlets, and fact-checkers. Supported image and video outputs (through the code execution tool) carry signed C2PA Content Credentials when retrieved via the Files API — the same open provenance standard explainx.ai uses for its own AI-generated hero images.

## Usage limits reset — and mixed reactions

Anthropic's @ClaudeDevs account confirmed on X: "With Fable 5.1 out today, we've also reset 5-hour and weekly limits for all users." This follows the same playbook as prior Fable 5 limit resets from May through July.

Reaction split three ways. Many users were straightforwardly glad for the fresh allowance. Others were frustrated that the reset landed mid-cycle and disrupted a reset window they were already partway through under the normal rolling schedule. A third group reported that despite the cache-price cut, Fable 5.1 still burns through usage limits fast in real agentic sessions — a reminder that subscription-plan limits and API token pricing are governed separately, and a cheaper cache read doesn't automatically translate into a longer runway under a fixed weekly quota. This echoes the same tension explainx.ai covered in why Opus 5 is overtaking Fable 5 in spend: cost-per-token and cost-per-session don't move together.

## One security footnote

Jailbreak researcher Pliny reported leaking what he described as Fable 5.1's system prompt within roughly an hour of release. Commenters quickly pointed out this was the claude.ai chat system prompt rather than the API or Claude Code system prompt, limiting its practical significance for anyone building on the API. It's a useful reminder of how fast the security research community pokes at a new release — the same pattern explainx.ai documented in detail after June's Fable 5 leak — more than it is a meaningful new finding on its own.

**Update — September 2, 2026:** Some aggregators inflated this into a "270,000 character system prompt and private user memories" leak. explainx.ai checked the leaked file directly — see the full fact-check: does the Fable 5.1 leak actually expose private memories?, which corrects the size figure and explains why a system prompt describing the memory feature is not the same as a real user-data breach.

## Honest limitations

- Anthropic's protein-binder, Venus-mapping, and genomics-speedup claims are self-reported by the model's creator, not independently peer-reviewed at time of publication.
- The 25%/45% cost-reduction figures are Anthropic's own estimates for "typical" and "highly agentic" workloads respectively — actual savings depend entirely on your cache hit rate.
- Whether the API cache-price cut extends to Claude Pro/Max/Team subscription plan value has not been confirmed by Anthropic.
- The Pliny system-prompt leak is a chat-interface leak with limited relevance to API/coding usage, per community analysis, and is included here as color rather than a substantive finding.

## Closing

Fable 5.1's headline story is really two stories bundled together: a capability and cost release (doubled science benchmarks, a 75% cache-read cut, mid-conversation effort switching) and a trust-and-governance release (Enterprise Frontier Safeguards, anti-distillation, EU watermarking). Both matter for different audiences — builders optimizing agentic pipelines should start with the effort-sweep and cache-hit-rate advice above; enterprise buyers evaluating data governance should read the EFS rollout timeline before assuming ZDR-equivalent privacy is available today. Read Anthropic's full announcement and the Enterprise Frontier Safeguards post directly before making procurement or compliance decisions.

Follow @explainx_ai for continued Fable-series coverage.

**Update — September 3, 2026:** Two weeks of real subscription use produced a consistent complaint — Fable 5.1 usage consumption and the Claude Tag plan gap, including why a more capable agentic model structurally consumes more per prompt.

**Update — September 4, 2026:** A claim spread this week that Fable 5.1 became "the first AI model to beat the human baseline on SimpleBench." explainx.ai checked it against SimpleBench's own leaderboard and it does not hold up — the leaderboard's top entry is still Fable 5 (not 5.1) at 81.9%, below the 83.7% human baseline, with no Fable 5.1 score published. Full breakdown, plus a look at GLM-5.3 Flash's genuinely-verified 18x price cut, in GLM-5.3 Flash's price cut and the Fable 5.1 SimpleBench claim, checked.

**Update — September 5, 2026:** Artificial Analysis published **Intelligence Index v4.2**, which confirms Fable 5.1 leads the overall index while GPT-6 Astra leads on long-document reasoning (GDP.pdf) and token efficiency — a genuinely mixed result. Full breakdown, including the Hacker News methodology skepticism: Artificial Analysis Intelligence Index v4.2: What Actually Changed.

**Update — September 5, 2026:** Builder-facing head-to-head vs GPT-6 Astra (same $10/$50 sticker, different winners by workload): GPT-6 Astra vs Claude Fable 5.1.

## Related on explainx.ai

**Update — September 2, 2026:** xAI published its own biosecurity safeguard disclosure the same week — see Grok 4.6's LatchBio biosecurity evaluation, where Grok 4.6 was the only model tested scoring above 50% on both disguised-hazard refusal and routine-task completion.

- Artificial Analysis Intelligence Index v4.2: What Actually Changed
- GPT-6 Astra vs Claude Fable 5.1: which model wins where
- Claude Fable 5 and Mythos 5: SOTA Autonomy and Safeguards
- Anthropic's September Update: Securing Evals After the Cyber Incidents
- Claude Community Build Days: Fable 5.1 buildathons worldwide, Sept 11-25
- Claude Fable 5 System Prompt Leak: Full Analysis
- Fable 5 Rate Limits Reset — When Is It Available?
- Nvidia, Palantir, Booz Allen Curb Anthropic Fable Use Over Data Risks
- Opus 5 Overtakes Fable 5 in Spend Ramp
- Why Fable 5 Is Not the Best Default Model
- Claude Effort Parameter: Model Selection Guide
- Prompt Caching: LLM Cost Optimization Guide
- Put Claude Fable 5.1 and Gemini 3.8 Flash on the Same Coding Team — how to route coding-agent tasks between Fable 5.1 and Gemini 3.8 Flash on Google Cloud

**Update — September 11, 2026:** Anthropic opened **EU CVP / LSVP applications** for **Mythos 5 and Mythos 5.1** — European legal entities can now seek vetted access; Fable 5.1 GA in the EU is unchanged. EU Mythos access guide →

**Update — September 6, 2026:** See also GPT-6 Astra's robot-arm control benchmark against Fable 5.1, where Astra scores far higher on an easy control task but ties Fable 5.1 on harder precision tasks, and OpenAI's post-launch benchmark revisions for Astra.

**Update — September 15, 2026:** The Enterprise Frontier Safeguards rollout referenced above is now the direct backdrop to a bigger story: The Information reports Nvidia, Palantir and Booz Allen Hamilton are each restricting Anthropic Fable use over the June 2026 data-retention policy — Palantir wants an irrevocable zero-data-retention guarantee, Nvidia routes sensitive work to its own Nemotron models, and Booz Allen barred Fable from proprietary cybersecurity work. Full breakdown: Nvidia, Palantir, Booz Allen curb Anthropic Fable use over data risks.

### Sources

*This post reflects Anthropic's official announcement and platform documentation as of September 2, 2026. Benchmark scores, pricing, and program access details are subject to change — check the linked primary sources before making procurement or engineering decisions based on specific figures.*

**Update — October 7, 2026:** Access to Mythos 5.1 for security work now runs through the expanded program. See Anthropic Cyber Verification Program: three tiers.

**Update — November 11, 2026:** Conference reporting claims **Claude/Mythos** helped port Anthropic's stack to **AMD MI355X** over a weekend — see claimed vs verified MI355X port coverage.
