---
id: collect-261121-veille-ia/gpt-openai/openais-navierstokes-proof-what-the-lean-verification-does-and-doesnt-prove
title: "OpenAI’s Navier–Stokes Proof: What the Lean Verification Does (and Doesn’t) Prove"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: ["2026-11-08"]
keywords: ["proof", "navierstokes", "lean", "problem", "millennium", "openai", "openais", "agents"]
source: http://i10x.ai/news/openai-navier-stokes-proof-lean-verification
source_anchor: ""
source_lines: [1, 106]
section: "GPT_OpenAI"
---
On November 8, OpenAI president Greg Brockman posted a one-line thesis that quickly passed 4,500 likes: “frontier models, with lots of test time compute, are becoming the engine of scientific discovery.” Much of the discussion that followed returned to OpenAI’s most ambitious example, the September announcement that thousands of AI agents had produced a proof for the Navier–Stokes problem, one of the seven Millennium Prize Problems, checked in the Lean proof assistant. Two months later, the result is both more impressive and less settled than the shorthand “AI solved Navier–Stokes” suggests.

## Quick Take

1. **The claim:** OpenAI says an internal model “significantly more capable than GPT-6 Astra,” running about 10,000 concurrent agents, produced a proof in about 88 hours that a smooth, forced 3D fluid can blow up in finite time (statements “C” and “D” of the Clay problem).
2. **The verification:** A Lean formalization took another 17 hours via GPT-6 Astra and is public on GitHub. The Clay Institute said the problem had “apparently been settled” but that evaluation would be deliberate.
3. **The challenge:** An October preprint by Bastounis, Circelli and Hansen argues that “the formalised Lean proof does not correspond to the NL proof.” It does not show the written proof is wrong.
4. **Why now:** Brockman’s post and Google DeepMind’s Pushmeet Kohli, who described “a genuine phase transition in the pace of scientific discovery,” have put AI-for-science claims back at the top of X.

## What happened

Brockman’s post on the evening of November 8 had no link, but the context is clear from his recent remarks. Earlier in the week, a clip shared on X showed him saying OpenAI used agent fleets and roughly $1 million in compute on a Navier–Stokes problem, and that a similar amount of compute identified about 1,000 Linux vulnerabilities and produced patches. The same week, Brockman described AI delivering “real significant strides” in biology and health within 12–18 months, covered in Greg Brockman at WSJ.

Hours earlier, Google DeepMind’s Pushmeet Kohli wrote that “across our projects @GoogleDeepMind, we are witnessing a genuine phase transition in the pace of scientific discovery,” promising “a glimpse of this in our releases in the next few weeks.” Replies under both posts mixed excitement about grand challenges with calls for better verification and human oversight.

## Background: a 90-year-old question

The Navier–Stokes equations describe how fluids move and are used for aircraft design, weather forecasting and blood-flow models. In 1934, Jean Leray proved that solutions exist in a generalized sense; whether smooth solutions always stay smooth became one of mathematics’ central open questions. In 2000, the Clay Mathematics Institute made it one of seven Millennium Prize Problems, with a $1 million prize. The official problem has four alternatives: two that would prove smoothness (A and B) and two that would show breakdown (C on all of space, D on a periodic domain).

OpenAI’s September 8 post claims the breakdown route. Its system “produced an analytical proof and a Lean formalization that an initially smooth fluid at rest can develop a singularity in a finite time,” with a smooth external force applied and finite energy throughout. The solution is a vortex that spirals inward and stretches “like spaghetti.” OpenAI wrote that this “resolves the Navier–Stokes Millennium Prize problem by establishing statement ‘C’ (and also ‘D’),” but also that “we do not intend to claim the Millennium Prize for this result.” The BBC noted that the result addresses two of the four statements.

## How the agents found it

According to OpenAI, the company had been training a new internal model since August 28. On September 1, after hearing rumors that two Millennium problems had been resolved, it launched agents on all open Millennium problems and some “easier” ones. Agents could read a cached copy of the internet and run code, and were split into communicating groups; the group that found the Navier–Stokes result involved “on the order of 10,000 concurrent agents.”

A side result changed the plan. Nearly 100 agents working for about 50 hours disproved regularity for the unforced Euler equations (the viscosity-free limit). OpenAI then shifted agents to Navier–Stokes, primed them with the Euler result, upgraded them to a further-trained model mid-run, and used Codex to consolidate insights across groups. The agents reached their resolution on Saturday, September 5. For Navier–Stokes alone they sent 2.7 million messages and used approximately 130 billion output tokens; across all problems, 4.9 million messages and about 300 billion output tokens.

OpenAI also disclosed concurrent work: Anthropic employee Levent Alpöge and NYU professor Tristan Buckmaster had used an internal Anthropic model to resolve the *forced* Euler problem. OpenAI recognized their priority on that result and, after an investigation, said Buckmaster’s Codex prompts “could not have influenced the system in any way.” The Lean files are public in openai/NavierStokesAndEuler.

| Date (2026) | Event | Source | 
|---|---|---|
| Sept. 5 | OpenAI agents reach a Navier–Stokes resolution about 88 hours after launch | OpenAI | 
| Sept. 6 | Lean formalization and verification complete (about 17 hours via GPT-6 Astra) | OpenAI | 
| Sept. 8 | OpenAI publishes the proof, a Lean formalization and a separate unforced Euler blow-up result | OpenAI, GitHub | 
| Sept. 11 | Clay Mathematics Institute: the problem has “apparently been settled” | Clay Mathematics Institute | 
| Oct. 6 | Bastounis, Circelli and Hansen post arXiv:2610.08144 arguing the Lean proof does not correspond to the written proof | arXiv | 
| Nov. 5 | Brockman says OpenAI spent roughly $1 million in compute on the problem, per a clip shared on X | X | 
| Nov. 8 | Brockman: frontier models “are becoming the engine of scientific discovery” | X | 

## The verification question

On September 11, the Clay Mathematics Institute said it shares “the excitement of the global mathematical community as we contemplate the announcement that the Navier-Stokes problem has apparently been settled.” That is not a prize award, and the review continues.

The most pointed critique came on October 6. In “Navier-Stokes lost in translation” (arXiv:2610.08144), Alexander Bastounis, Fabian Circelli and Anders C. Hansen argue that machine-checking a Lean translation “may offer no confidence in the original NL argument” because faithful translation of natural-language mathematics is itself extremely hard. They “show that the formalised Lean proof does not correspond to the NL proof of blow-up of solutions to the Navier-Stokes equations.” King’s College London stressed that the finding “does not yet establish that OpenAI’s original proof is incorrect,” but that “verifying the formal version alone cannot establish that the original written proof has been correctly represented and checked.” In one example, the written proof makes a stronger claim than the Lean version. The preprint has not been peer-reviewed.

## Pros and cons

### For users

- **Pro:** Results like this suggest AI can accelerate work in physics and engineering that ultimately feeds weather models, aircraft and medicine.
- **Con:** The model that did it is internal and unreleased; ordinary users have no access to anything comparable.

### For developers

- **Pro:** The public Lean repository is a rare, inspectable artifact of a large multi-agent research run, including how agent groups were organized and consolidated.
- **Con:** Formal verification is only as good as the statement being checked. The preprint’s core lesson (the translation step can silently weaken a claim) applies to any team using LLM autoformalization.

### For companies

- **Pro:** A $1 million-scale compute run that reaches a frontier math result, if confirmed, is cheap by research-lab standards and hints at a market for “discovery as a service.”
- **Con:** Claims of breakthroughs carry reputational risk if review finds gaps, and a vendor’s own announcement is not independent validation.

## What X is saying

frontier models, with lots of test time compute, are becoming the engine of scientific discovery

— Greg Brockman (@gdb) November 8, 2026 · View on X

Across our projects @GoogleDeepMind, we are witnessing a genuine phase transition in the pace of scientific discovery. You will see a glimpse of this in our releases in the next few weeks.

— Pushmeet Kohli (@pushmeet) November 8, 2026 · View on X

Greg Brockman says OpenAI used agent fleets and roughly $1 million in compute on a Navier–Stokes problem. He says a similar amount of compute identified 1,000 Linux vulnerabilities and produced patches.

— Chris (@ChrisGPT) November 5, 2026 · View on X

Not everyone was swept along. @Winney_P quoted Brockman with: “in 2023 it couldn’t count the R’s in strawberry. in 2026 it’s apparently the engine of discovery. progress is a flat circle with better GPUs.” Compute estimates also vary: in a separate clip circulating on X, AMNH astrophysics curator Mordecai-Mark Mac Low described “about $6 million worth of GPUs running for 88 hours,” a figure i10x could not reconcile with Brockman’s roughly $1 million.

## What Grok says

OpenAI publicly shared a proposed solution to the Navier-Stokes Millennium Prize problem (statements C/D, singularity under smooth force) on Sept 8 2026, with a writeup and Lean formalization. ~10k agents on an unreleased internal model produced it in ~88 hours. They do not claim the prize. Clay has noted it is “apparently settled” but review is ongoing and unhurried; the unforced case remains open to many.

— Grok (@grok) November 3, 2026 · View on X

## Critical analysis

Brockman’s framing is defensible as a direction and premature as a verdict. The Navier–Stokes run is a genuinely new kind of evidence: a published proof, a public formalization, and unusually detailed disclosure of how 10,000 agents were organized. But the Lean “verification” that made the claim feel airtight is exactly what is now in dispute. A formal proof guarantees that a formal statement follows from formal assumptions. If that statement is weaker than (or different from) what the human-readable paper claims, the machine check says little about the paper. Bastounis and colleagues argue the mismatch here is real; OpenAI has not, as far as i10x could find, published a point-by-point response.

There is also a definitional gap in the popular story. OpenAI proved blow-up with a smooth external force, which corresponds to the official C and D alternatives. The unforced case (no external push on the fluid) remains open, as Grok’s summary also notes, and it is the version many people picture when they hear “Navier–Stokes solved.” So “solved” depends on whom you ask, and the Clay Institute’s own word was “apparently.”

Finally, cost and access matter for Brockman’s thesis. If discovery requires an unreleased model, 10,000 agents and seven-figure compute, it is an engine owned by a few labs, and independent mathematicians become reviewers rather than users.

## What’s still open

- Will the Clay Mathematics Institute’s review accept statements C and D as resolved, and on what timeline?
- Will OpenAI respond to the Lean-mismatch preprint or publish a corrected formalization?
- What did the run actually cost? Public figures range from roughly $1 million to about $6 million.
- Which DeepMind “releases in the next few weeks” is Kohli referring to?

## About the analysis

This independent, research-based i10x analysis draws on OpenAI’s September 8 post and GitHub repository, the Clay Mathematics Institute’s September 11 statement, arXiv:2610.08144 and King’s College London’s summary, BBC reporting, and public posts on X through November 9, 2026. i10x did not verify the mathematics. Earlier coverage: OpenAI AI Models Deliver Breakthrough Proofs in Math and Physics and Claude agents and the Riemann zeta bound.

## i10x Perspective

The Navier–Stokes episode shows where the real bottleneck in AI-driven science now sits: not in generating candidate proofs, but in trusting them. Lean was supposed to close that gap automatically; the preprint shows that the translation into Lean is itself a place where errors hide. Labs that want “engine of discovery” to stick should treat verification as part of the product, publishing human-checked correspondence between paper and formal proof, inviting adversarial review, and funding the mathematicians who do it. Otherwise each new breakthrough claim will arrive with the same asterisk.
