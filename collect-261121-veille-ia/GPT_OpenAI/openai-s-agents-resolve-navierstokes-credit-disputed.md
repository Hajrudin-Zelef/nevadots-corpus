---
id: collect-261121-veille-ia/gpt-openai/openai-s-agents-resolve-navierstokes-credit-disputed
title: "OpenAI's Agents Resolve Navier–Stokes, Credit Disputed"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["openai", "navierstokes", "agents", "millennium", "smooth", "disputed", "result", "prize"]
source: https://evermx.com/case/openai-navier-stokes-millennium-proof
source_anchor: ""
source_lines: [1, 116]
section: "GPT_OpenAI"
---
# OpenAI's Agents Resolve Navier–Stokes, Credit Disputed

OpenAI's agent system produced a Navier–Stokes singularity proof, but priority is contested by Buckmaster and Alpöge's related Euler result.

## Introduction

On September 8, 2026, OpenAI published "On the Navier–Stokes Millennium Prize Problem," describing a solution to the Navier–Stokes existence and smoothness problem, one of the Clay Mathematics Institute's seven Millennium Prize Problems. The proof shows that the Navier–Stokes equations, which govern fluid motion, can develop a singularity in finite time. OpenAI released both a proof writeup and a Lean formalization of the result. Whether smooth three-dimensional fluid motion can break down had remained an open question for roughly 90 years. The equations date to 19th-century work by Claude-Louis Navier and George Gabriel Stokes; in 1934, Jean Leray proved that generalized solutions exist without establishing whether they stay smooth. The Clay Institute named the problem one of seven Millennium Prize Problems in 2000, each carrying a $1 million award. OpenAI says the result came from an internal agent system, not a public model, and it arrived alongside a separate, disputed claim of related progress from mathematician Tristan Buckmaster and Anthropic researcher Levent Alpöge.

## Result Overview

OpenAI's result establishes statement "C," and also "D," of the Clay Institute's official formulation, meaning it shows solutions can fail to stay smooth for all time from certain initial data. The singularity takes the form of a vortex that spirals inward and grows increasingly elongated, which OpenAI compares to spaghetti. As the swirl narrows, the fluid inside it speeds up, while total energy stays finite. The setup starts from fluid that is smooth and at rest, with a smooth external force applied over time.

The underlying mathematical technique did not originate with either AI effort. Quanta Magazine reports both AI-assisted teams built on an analytic "infinite cascade of layers" strategy that Luis Martínez-Zoroa of CUNEF University introduced in his 2021 doctoral dissertation, developed together with Diego Córdoba of Spain's Institute for Mathematical Sciences. By 2023, the pair had shown that a version of the (viscosity-free) Euler equations with a deliberately messy forcing function develops singularities, but stacking their cascading layers spoiled the smoothness the forcing function needed to meet the Millennium criteria. Quanta describes closing that smoothness gap as the step both competing AI-assisted groups appear to have achieved.

## Analysis

### Method and Scale

OpenAI says it began training a new internal model on August 28, 2026. On September 1, after hearing "rumors that two Millennium Prize problems had been resolved," it launched an effort covering all remaining open Millennium Prize problems. The model is described only as "significantly more capable than GPT-6 Astra" and has not been released publicly.

The approach used coordinating agents equipped with a cached copy of the internet and code execution, organized into communicating groups; Codex consolidated useful insights across groups into follow-up prompts. The group that produced the Navier–Stokes resolution involved "on the order of 10,000 concurrent agents." Groups were prompted with different variants of the problem: two aimed at proving solutions stay smooth, two aimed at disproving it, and the disproof variant is what succeeded. On the related, unforced Euler regularity problem, "nearly 100 agents worked together for approximately 50 hours" to produce a disproof.

For Navier–Stokes, OpenAI's agents reached their resolution on Saturday, September 5, roughly 88 hours after the first agents launched; Lean formalization and verification took another 17 hours via GPT-6 Astra. Across all Millennium problems attempted, OpenAI reports 4.9 million messages and about 300 billion output tokens; for Navier–Stokes alone, roughly 2.7 million messages and 130 billion output tokens. OpenAI researcher Sébastien Bubeck estimates, per Quanta, that the compute cost ran into several million dollars.

### Verification and the Credit Dispute

Lean formalization gives the proof machine-checked logical consistency, but that is not the whole verification job. Quanta notes that "the crucial bit of verification that must still be done by humans is to guarantee that the statement being shown to be true in Lean is logically equivalent to what mathematicians set out to prove" — a step still outstanding.

The timing of OpenAI's announcement is tied to a separate, unresolved priority dispute. Quanta reports OpenAI's post came about 12 hours after a statement from Buckmaster, shared just before midnight on September 7, saying he and Alpöge had resolved several closely related problems using a variety of AI models, including OpenAI's. Quanta reports the pair "hadn't quite" solved a Millennium Prize problem themselves, though they say they have an unverified proof of blowup for a "somewhat easier" version of Navier–Stokes, alongside their forced Euler result.

OpenAI says the September 1 rumor is what triggered its own effort. After completing Lean verification on September 6, it reached out to Buckmaster and Alpöge to propose a joint announcement, and says it learned then that the pair's resolution covered the forced Euler problem specifically. OpenAI states it "recognize[s] the priority of their work on forced Euler." Buckmaster and Alpöge moved up their own announcement after word of their progress reportedly leaked to OpenAI.

OpenAI states that it and its agents "did not see any of their work through any means until they released it publicly — in particular, no specific user data was accessed," adding: "While unlikely, we cannot rule out that de-identified data derived from their usage of our products helped improve our models." Quanta reports that "the details of the interaction between Buckmaster, Alpöge, and OpenAI remain murky — different parties to the conversation are presenting different versions," and that Buckmaster "appears to suggest that the OpenAI researchers or their AI agents may have gained access to (and benefited from)" his and Alpöge's work. Neither account is independently confirmed.

Buckmaster was candid about his own process, writing that "the first LLM generated proof Levent sent me was the most horrendous I have ever read," and that one of their three released papers "can only be described as AI slop. I am sorry for this." He credited the underlying mathematics to Martínez-Zoroa, saying "I believe Luis Martínez-Zoroa deserves a Fields Medal." OpenAI states it does not intend to claim the Millennium Prize.

## Pros and Cons

**Pros**

- The agent system produced a full proof plus a Lean formalization within about 88 hours of agent work, with disclosed message counts, token totals, and a timeline.
- OpenAI states it will not claim the $1 million prize and addresses the data-access question directly rather than leaving it unaddressed.
- The same method scaled down to the smaller Euler regularity case (about 100 agents, roughly 50 hours), not just one high-cost run.
- OpenAI acknowledges Buckmaster and Alpöge's priority on the specific forced Euler problem rather than claiming that result too.

**Cons**

- The model behind the result is an unreleased internal system "significantly more capable than GPT-6 Astra"; the process cannot be reproduced with a publicly available model.
- Lean verification confirms the formal statement is logically valid, not that it is equivalent to the intended mathematical claim; per Quanta, that human check is still outstanding.
- Priority and the extent of contact between OpenAI and Buckmaster/Alpöge's work are contested, with Quanta describing the accounts as "murky" and differing.
- The result concerns idealized equations; as OpenAI's own framing notes, real fluids are made of molecules, so the singularity has no direct real-world fluid-dynamics consequence.

## Outlook

Quanta writes that "if the result holds up to further scrutiny, it is, by a significant margin, the most important mathematical proof to have been arrived at by an artificial-intelligence model to date." Charles Fefferman of Princeton, author of the Clay Institute's official problem description, said he was "thrilled that the problem was solved," but named Córdoba and Martínez-Zoroa, not either AI effort, as the heroes of the underlying mathematics. That framing suggests the result may be received as validation of an analytic technique built by human mathematicians, applied at agent scale, rather than as mathematics invented by the agents themselves.

The unresolved dispute over what OpenAI's agents may have seen, and when, is likely to draw continued scrutiny, since the current accounts from OpenAI and Buckmaster do not agree. How that dispute settles, and whether independent mathematicians confirm the Lean statement matches the intended theorem, will likely matter more than the raw agent and compute figures OpenAI has disclosed.

## Conclusion

OpenAI's Navier–Stokes announcement combines a large-scale demonstration of coordinated AI agents on an open mathematical problem with an outstanding human verification step and an active, disputed priority question involving Buckmaster and Alpöge. It is a meaningful data point for researchers tracking AI-assisted mathematics and formal verification, not a finished, independently confirmed result. Mathematicians, AI researchers, and Millennium Prize followers are best served by this story; the practical takeaway for others is that the singularity concerns idealized equations, not real-world fluid behavior.

## Editor's Verdict

OpenAI's Agents Resolve Navier–Stokes, Credit Disputed earns a solid recommendation within the research space.

The strongest case for paying attention: coordinated agent system produced a full proof plus Lean formalization within about 88 hours, with disclosed agent counts, message totals, and token usage. That alone raises the bar for what readers should expect in this space. Reinforcing that, OpenAI states it will not claim the $1 million prize and directly addresses the data-access question raised by the dispute rather than ignoring it — practical value rather than just headline appeal. The broader signal worth registering is straightforward: OpenAI's internal agent system, using a model more capable than GPT-6 Astra but not publicly released, produced a proof that Navier–Stokes equations can develop a finite-time singularity. On the other side of the ledger, one constraint is real rather than a marketing footnote: the model behind the result is an unreleased internal system 'significantly more capable than GPT-6 Astra'; the process cannot be reproduced with a publicly available model. It should factor into any serious decision. Layered on top of that, Lean verification confirms logical validity of the formal statement, not that it is equivalent to the intended mathematical claim; per Quanta, that human confirmation step is still outstanding — which narrows the set of teams for whom this is an obvious yes.

For ML researchers, technical leads, and readers tracking the underlying science behind new capabilities, this is a serious evaluation candidate, not just a curiosity to bookmark. For everyone else, the safer posture is to monitor coverage and revisit once the use cases that matter to your team are demonstrated in the wild.

### Pros

- Coordinated agent system produced a full proof plus Lean formalization within about 88 hours, with disclosed agent counts, message totals, and token usage.
- OpenAI states it will not claim the $1 million prize and directly addresses the data-access question raised by the dispute rather than ignoring it.
- The same method scaled down successfully on the related unforced Euler regularity problem (about 100 agents, roughly 50 hours).
- OpenAI explicitly acknowledges Buckmaster and Alpöge's priority on the forced Euler problem instead of claiming that result too.

### Cons

- The model behind the result is an unreleased internal system 'significantly more capable than GPT-6 Astra'; the process cannot be reproduced with a publicly available model.
- Lean verification confirms logical validity of the formal statement, not that it is equivalent to the intended mathematical claim; per Quanta, that human confirmation step is still outstanding.
- Priority and the extent of contact between OpenAI and Buckmaster/Alpöge's work are contested, with Quanta describing the parties' accounts as differing and 'murky'.
- The result applies to idealized fluid equations; per OpenAI's own framing, real fluids are made of molecules, so it carries no direct real-world fluid-dynamics consequence.

### References

### Comments0

### Key Insights

- OpenAI's internal agent system, using a model more capable than GPT-6 Astra but not publicly released, produced a proof that Navier–Stokes equations can develop a finite-time singularity.
- The Navier–Stokes effort used on the order of 10,000 concurrent agents, about 2.7 million messages, and roughly 130 billion output tokens over about 88 hours, plus 17 hours of Lean verification.
- Lean formalization confirms the proof's logical validity, but a human step confirming the Lean statement matches the intended mathematical claim remains outstanding, per Quanta Magazine.
- OpenAI's announcement came about 12 hours after a statement from Tristan Buckmaster and Levent Alpöge describing related but distinct progress, including a forced Euler result and an unverified Navier–Stokes-adjacent proof.
- Quanta Magazine reports the accounts of contact between OpenAI and Buckmaster/Alpöge remain murky, with Buckmaster suggesting OpenAI's agents may have benefited from his and Alpöge's work.
- OpenAI states it will not claim the $1 million Millennium Prize for the result and explicitly acknowledges Buckmaster and Alpöge's priority on the forced Euler problem.
- Both AI-assisted efforts built on an analytic 'infinite cascade' technique developed by Diego Córdoba and Luis Martínez-Zoroa, whose 2023 forced Euler work fell short of Millennium criteria on a smoothness technicality.
- The singularity concerns idealized fluid equations; OpenAI's own framing notes real fluids are made of molecules, so the result carries no direct practical fluid-dynamics consequence.

## Related AI Reviews

### Apple Health AI Study Seeks Up to 200,000 iPhone Users

Apple and OpenEvidence open a US research study of up to 200,000 participants on how people use clinically grounded health AI. Not a product launch.

### AlphaGenome 2 Review: DNA Variant Effects by Cell Type

AlphaGenome 2 predicts DNA variant effects per cell type and state; DeepMind says it finds nearly twice as many expression-altering variants.

### METR: Anthropic Models Improving at About 2x Historical Rate

METR's 11-week review at Anthropic estimates well-specified capability gains run near 2x the earlier rate, with high uncertainty and heavy redactions.

### OpenAI Says AI Disproved the Exponential Time Hypothesis

OpenAI says its AI disproved the ETH and a Lean check passed. An outside group led by Impagliazzo, Paturi and Williams will verify it.
