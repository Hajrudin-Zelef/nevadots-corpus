---
id: collect-261121-veille-ia/grok-xai/musk-altman-hassabis-react-to-pace-the-frontier-2026-explainx-ai-blog
title: "Musk, Altman, Hassabis React to 'Pace the Frontier' (2026) | explainx.ai Blog"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: []
keywords: ["essay", "anthropic", "musk", "altman", "commitment", "frontier", "reaction", "within"]
source: https://www.explainx.ai/blog/emad-mostaque-elon-musk-altman-react-pace-the-frontier-2026
source_anchor: ""
source_lines: [1, 98]
section: "Grok_xAI"
---
Dario Amodei's September 12, 2026 essay proposing that AI companies "pace the frontier" drew a reaction that moved faster than the essay's own three-step plan anticipated. Within roughly an hour, Elon Musk posted support. Within the same day, OpenAI CEO Sam Altman went further than support — he committed OpenAI to matching Anthropic's core commitment. Not everyone agreed, and the disagreement is as informative as the agreement.

## TL;DR

| Reaction | Who | What they actually said or did | 
|---|---|---|
| Support (general) | Elon Musk | Posted "Dario is right" ~1 hour after the essay | 
| Support (specific commitment) | Sam Altman | Posted "We will do the same" — committing OpenAI to embedded evaluators | 
| Agreement | Aidan McLaughlin (OpenAI researcher) | Agreed with "basically every word" of the essay | 
| Extension | Clément Delangue (Hugging Face) | Announced an "Open Alignment Initiative," requested to join Anthropic's evaluator program | 
| Skepticism (power/structure) | Emad Mostaque (Stability AI) | Evaluators have "minimal power" — see explainx.ai's breakdown | 
| Skepticism (concentration) | Chamath Palihapitiya | Argues the plan concentrates power with Anthropic and threatens open-source AI | 
| Support (alternate mechanism) | Demis Hassabis (Google DeepMind) | Says the essay's direction is "correct," points to DeepMind's own proposal for an industry-wide AI standards body | 
| Mixed (startup dynamics) | Paul Graham | If labs actually slow down, a new model company becomes "slightly more feasible" — but only if the resulting regulation doesn't raise the entry bar more than the slowdown lowers it | 
| Radical (nationalization) | Travis McDonald (reply) | If AI is a national-security-level threat, Anthropic's equity, assets, and IP should be seized outright — no corporation should hold something this dangerous | 

## Musk's reversal is the headline, but Altman's commitment is the substance

Elon Musk's post — "Dario is right," reportedly within about an hour of the essay going live — is notable mainly for who said it. Musk has previously been openly hostile to Anthropic, saying the company "hates Western Civilization" and predicting it was "doomed to become the opposite of its name." A one-line endorsement of Amodei's framing, without any specific operational commitment from xAI, reads more as a signal that the underlying concern (recursive self-improvement, agent-swarm incidents) has crossed some threshold of seriousness even Anthropic's harshest public critics now take at face value, than as a policy shift at xAI itself.

Sam Altman's response is the more consequential one. **"We will do the same"** is a direct commitment to match Anthropic's specific first step — giving embedded third-party evaluators employee-like access to verify safety practices — not just a general statement of agreement. This matters because Amodei's essay explicitly structured Step 1 (embedded evaluators) as something Anthropic was doing "unilaterally," with Step 2 (industry-wide coordination) framed as something that would happen only "once embedded evaluators are operating within a critical mass of US AI companies." Altman's same-day commitment compresses that sequencing considerably — the essay's own Step 2 started moving within hours of Step 1 being announced, not after a critical mass had actually formed.

Inside OpenAI itself, the reaction wasn't monolithic. Researcher Aidan McLaughlin publicly agreed with "basically every word" of Amodei's essay — a notable amount of public alignment between researchers at rival labs on the underlying safety concern, even where the labs themselves compete aggressively on capability and product.

## Hugging Face extends the reaction beyond the two biggest labs

Hugging Face co-founder Clément Delangue announced an **"Open Alignment Initiative"** and requested to join Anthropic's embedded-evaluator program — extending the reaction beyond the closed-model frontier labs into the open-weight ecosystem specifically. explainx.ai has separately covered Hugging Face's Open Alignment team, announced days earlier and framed around safety and cybersecurity work for open-weight models specifically, in the wake of the July 2026 OpenAI-Hugging Face incident that Amodei's essay itself cites as a motivating event. Delangue's move to formally request participation in Anthropic's own evaluator program is a distinct, additional step — an open-model company asking to be audited by the same standard a closed-model company set for itself, rather than only building parallel open-model safety infrastructure.

## Inside Anthropic itself: Thariq's "cracks are showing" reply

Not every reaction to the essay came from outside Anthropic. Thariq, who works on Claude Code at Anthropic, replied directly under Amodei's post with a notably candid framing rather than a simple endorsement: "If you showed me Claude Code today in 2018, I would have thought it was AGI. We have already absorbed a dramatic amount of change in the profession of software engineering and society as a whole. But still, we're starting to see cracks." He described the pace of change as outrunning his own ability to track it — "most people I know in AI are tired but powering through. I fear we will need more than that" — and framed the essay's ask as needing "time to harden our systems, and for society to deliberate in how this technology is used and deployed," while explicitly stating he holds "a fairly low p(doom)" and believes humanity is resilient enough to navigate it collectively.

This is worth separating from the CEO-level reactions above: Thariq isn't announcing a policy or a commitment, and he isn't a spokesperson for Anthropic's official position. What his reply adds is a firsthand practitioner's account — someone building the product — corroborating the essay's underlying premise (things are moving faster than the people closest to it can comfortably track) without needing any of the "is this regulatory capture" framing that dominates the CEO-level reactions. Replies to his post ranged from skepticism about "p(doom)" as a meaningful concept absent an actual defense-in-depth strategy, to blunt dismissal that current models "still fail on simple tasks" and are "just bigger and more expensive" text prediction, not evidence of anything AGI-adjacent — a reminder that the "is this even happening" skepticism runs through every level of this reaction, not just the outsider critiques from Mostaque and Palihapitiya.

## Hassabis and DeepMind: a standards body, not a matching commitment

Google DeepMind CEO Demis Hassabis's response is structurally different from Musk's or Altman's. Where Musk offered general endorsement and Altman committed to matching Anthropic's specific mechanism, Hassabis pointed to something DeepMind had already proposed separately: **an industry-wide standards body for frontier AI**. His post called Amodei's essay's "direction correct" while flagging that "the details need working through" — agreement on the goal, but an implicit argument that a permanent standards body, not lab-by-lab embedded-evaluator deals, is the right vehicle for getting there.

This matters for reading the "not yet confirmed" line below differently than it read a few hours earlier: DeepMind hasn't adopted Anthropic's embedded-evaluator model, but it isn't staying silent either — it's counter-proposing a different coordination mechanism instead. Whether a standards body (likely slower to stand up, harder for any single lab to unilaterally launch, but potentially more durable and less dependent on one company's own program) or embedded evaluators (faster to launch, already operational at Anthropic, but concentrated in each lab's own choice of evaluator) ends up being the mechanism that sticks is now a live, three-way framing question across the industry's three biggest labs.

## Paul Graham and the incumbent-versus-startup question

Y Combinator co-founder Paul Graham raised a narrower but practically important question for builders: **if the frontier labs actually slow down, does that help or hurt a new model company trying to get started?** His initial framing was cautiously optimistic — a real slowdown narrows the capability gap a new entrant needs to close, making starting a new lab "slightly more feasible." But he immediately qualified it in a reply: any regulatory apparatus that comes with pacing "may be easier to deal with than a hundred billion dollar fundraising gap," implicitly conceding that regulation is a real cost, just possibly a smaller one than the capital gap it might narrow.

The pushback in his replies sharpened the same regulatory-capture concern already raised by Mostaque and Palihapitiya, applied specifically to new entrants rather than to open-weight models: if the "pacing" regulation that governments end up writing requires the kind of embedded, employee-level evaluator access and compliance infrastructure Anthropic is building, that's a fixed cost a well-funded incumbent can absorb far more easily than a two-person startup — meaning the same policy could simultaneously slow the frontier down (helping new entrants close the capability gap) and raise the compliance floor (hurting new entrants trying to clear it). One reply put it bluntly: "they're not slowing down, they're asking everyone else to."

## The dissent: Chamath Palihapitiya and the regulatory-capture critique

Investor Chamath Palihapitiya's criticism sharpens a concern explainx.ai already covered from Stability AI founder Emad Mostaque in the original essay breakdown: that a "pacing" framework, however well-intentioned, concentrates power with whichever lab writes the terms — in this case, Anthropic, which designed both the evaluator-access model and the language calling on governments to require it of competitors. Palihapitiya's specific addition to that critique is that this structure poses a distinct threat to **open-source AI development**, since a compliance framework built around embedded, employee-level access is far more feasible for a well-resourced closed lab with dedicated legal and security teams than for a distributed open-weight community with no single point of "embedding" evaluators into.

This is the same tension Mostaque raised with a different emphasis: Mostaque argued evaluators would have "minimal power" (an enforcement critique — the mechanism is too weak), while Palihapitiya argues the mechanism, if it does have teeth, concentrates that power in the wrong place (a structural critique — the mechanism, even if it works, favors incumbents). Both critiques can be true simultaneously and point at different fixes: Mostaque's implies the plan needs stronger enforcement (his own suggested alternative is pausing training runs outright); Palihapitiya's implies the plan needs a structure that doesn't require an entity like Hugging Face to formally "request permission" to join an audit program another company designed and controls.

## What actually changed as a result of one day's reaction

It's worth being precise about what moved from "essay" to "commitment" within roughly 24 hours, and what didn't:

- **Confirmed commitment:** OpenAI, via Altman, has publicly committed to matching Anthropic's embedded-evaluator access model. No operational details (which evaluator organization, what access scope, what publication rights) have been specified for OpenAI's version as of this writing — Altman's post is a commitment to match, not yet a launched program.
- **Confirmed extension:** Hugging Face has formally requested inclusion in Anthropic's evaluator program and separately stood up its own Open Alignment initiative for the open-model ecosystem.
- **Confirmed, but different mechanism:** Google DeepMind, via Hassabis, endorsed the essay's direction but did not adopt Anthropic's embedded-evaluator model — it pointed instead to its own pre-existing proposal for an industry-wide AI standards body as its preferred coordination vehicle.
- **Not yet confirmed:** xAI has not made a reciprocal commitment matching Anthropic's or OpenAI's. Musk's endorsement of the essay's framing is not the same as xAI adopting Step 1.
- **Not yet confirmed:** Nothing in this reaction wave touches Amodei's Step 3 (global coordination including China) — every reaction documented here is domestic, US-lab-to-US-lab coordination, consistent with Amodei's own framing that Step 3 is the hardest and most speculative tier.

For builders and researchers tracking whether "pacing the frontier" becomes an industry norm rather than a single company's PR move, the signal to watch next is whether OpenAI's embedded-evaluator program actually launches with comparable publication rights to Anthropic's — a contractual right to publish findings without lab editorial control, with only narrow security/legal redactions — since that specific detail, not the general commitment, is what determines whether this becomes real verifiability or a second company's version of the same critique Mostaque leveled at the first.

## Why the speed of this reaction matters more than its content

The most striking thing about this episode isn't any single reaction — it's the compressed timeline. Amodei's essay explicitly sequenced its own plan: Step 1 (Anthropic's unilateral embedded-evaluator commitment) was supposed to precede Step 2 (industry-wide coordination), which itself was framed as contingent on "a critical mass of US AI companies" already having evaluators in place. In practice, a competing lab's CEO committed to matching Step 1 within the same news cycle, before Anthropic's own program had operational details, let alone before any "critical mass" had formed.

There are two ways to read that speed. The generous read is that the underlying safety concern — recursive self-improvement accelerating, plus the OpenAI-Hugging Face incident Amodei's essay cites directly — has become salient enough across the industry that competing labs no longer need extended negotiation to agree on a baseline transparency commitment; the fact that even Musk, a longtime Anthropic critic, endorsed the framing supports this reading. The skeptical read, aligned with both Mostaque's and Palihapitiya's critiques, is that a same-day "we'll do the same" commitment with zero operational detail costs a competing lab nothing reputationally while requiring nothing concrete — it's cheap to match a rival's PR framing verbally and considerably harder to match its actual contractual terms (publication rights without editorial control, specific access scope, a named evaluator organization).

Which read is correct will be determined by what happens next, not by what was said this week. If OpenAI's evaluator program launches within the next month or two with terms comparable to Anthropic's — a named organization, contractual publication rights, narrow and specific redaction categories — that's real evidence the generous read holds. If it doesn't materialize, or materializes with substantially weaker terms (no independent publication rights, broader redaction categories, no fixed timeline), that's evidence for the skeptical read, and worth revisiting this post to note explicitly.

## What this means for anyone building on these platforms

None of this changes anything about model access, API terms, or product roadmaps today — Amodei's own essay is explicit that pacing doesn't mean halting training or technical progress, and nothing in this reaction wave introduces any new restriction on what Claude, GPT, or Grok can currently be used for. The practical relevance for builders is longer-horizon: if this coordination holds and expands to Amodei's proposed "checkpoint" model — where a model demonstrating certain capabilities must be accompanied by certified alignment properties before release — that could eventually affect release timelines for future frontier models across multiple providers simultaneously, rather than any single lab moving faster or slower than its competitors. That's a meaningfully different competitive dynamic than the current capability race, and it's the concrete thing to watch for over the coming months, not the individual tweets from this particular news cycle.

**Update — September 14, 2026:** Two days after endorsing Amodei's pacing essay, Musk reposted Tim Urban's 2015 Wait But Why intelligence-curve cartoon — see why the "steep curve" meme went viral again.

**Update — September 15, 2026:** A third pole emerged in this debate — Jack Dorsey's "open the frontier" essay agrees scrutiny is warranted but rejects industry-wide limits negotiated by incumbent labs like the ones reacting here, arguing for open weights and reproducible evaluations instead. See Jack Dorsey's "Open the Frontier" Answers Amodei — With Open Weights.

**Update — September 15, 2026:** The endorsement side of this reaction (Musk, Altman) now has company on both flanks — investor Gavin Baker argues "pacing" mostly means spending more compute on evals, not a real slowdown, while Michael Burry, Gary Marcus, and Palantir's Shyam Sankar publicly questioned whether the safety framing is sincere at all, and Trump and Harris staked out opposite political positions on it. See Pace the Frontier Goes Cross-Partisan: Baker, Burry, Trump, and Harris React.

## Related reading

- **Update — September 25, 2026:** A Google DeepMind chip-design engineer resigned citing AI pace directly, distinct from this reaction thread and from the safety-researcher resignations earlier in the month: Robert O'Callahan's resignation, explained →
- **Update — September 20, 2026:** The "power grab" reaction some critics voiced here now has a legal echo — a reported lawsuit accuses Anthropic, OpenAI, Google, and SpaceXAI of colluding to slow releases as a cartel. The AI slowdown cartel lawsuit, explained →
- Pace the Frontier Goes Cross-Partisan: Baker, Burry, Trump, and Harris React
- Elon Musk reposts Wait But Why's 2015 AI intelligence curve
- Dario Amodei Wants to "Pace the Frontier" — Here's the Actual Plan
- What is an embedded evaluator? — the mechanism OpenAI committed to matching
- The Hugging Face OpenAI Attack: Full Timeline and What the Reports Say
- Hugging Face Open Alignment Team: What Builders Can Use Today
- What Is Recursive Self-Improvement (RSI) in AI?
- Sen. Josh Hawley Opens Senate Probe Into OpenAI Over Hugging Face Breach
- Alignment as a Gating Factor: Wang, Musk, and the Open-Source Fight — the same-day Musk reply about open source colliding with Congress pausing recess for AI legislation
- Jack Dorsey's "Open the Frontier" Answers Amodei — With Open Weights — the open-source/decentralization pole of this debate, published three days later

*This post reflects public statements circulating on X and covered by mainstream outlets (CNBC, Yahoo/Fox News wire) as of September 13, 2026. None of the labs mentioned have published a full operational program matching these commitments as of publication — check each company's own communications for the current state before treating any commitment as a launched program.*
