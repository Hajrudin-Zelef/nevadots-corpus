---
id: collect-261121-veille-ia/gpt-openai/openai-says-navier-stokes-fluids-can-blow-up-in-finite-time
title: "OpenAI says Navier-Stokes fluids can blow up in finite time"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: ["2026-09-08"]
keywords: ["lean", "openai", "proof", "says", "finite", "time", "navier-stokes", "fluids"]
source: https://ademb10.com/claims/navier-stokes
source_anchor: ""
source_lines: [1, 54]
section: "GPT_OpenAI"
---
# OpenAI says Navier-Stokes fluids can blow up in finite time

Part of a $1M Clay Millennium problem: can a fluid reach unbounded speed in finite time?

Claimed by OpenAI on 8 Sep 2026 · last checked 18 Nov 2026

## Is it true?

Not settled. An outside team found OpenAI's Lean proof does not match its written proof, and no expert or journal has ruled.

Disputed means: A named expert raised a specific objection, and it is not resolved. How claims are checked

## What was claimed

OpenAI says an internal model proved, with a Lean formalisation, that a fluid at rest that is pushed by a smooth outside force can reach unbounded speed in finite time, which settles the 'breakdown' alternatives C and D of the Clay Navier-Stokes problem.

Claimed by OpenAI, which says an unreleased model running as about 10,000 agents found the proof in 88 hours and that a model then formalised it in Lean

AI: an unreleased OpenAI model

## Who checked it

- Scientific American13 Oct 2026reports that several research groups are using the Lean code and the paper, and that top experts find the proof hard to read; it gives no verdict on correctness
- Alexander Bastounis, Fabian Circelli and Anders C. Hansen6 Oct 2026compared the Lean code with the written proof and found it is not a faithful translation, for example Lemma 8.6 needs 4 extra derivatives in the paper and 5 in Lean; they make no claim about whether the written proof is correct
- Francesco Antonio De Luca11 Sep 2026rebuilt the Lean proof, ran Comparator with Lean's kernel and a second kernel, nanoda, and found no sorry and only the three standard axioms in both final theorems
- Formal Conjectures, Google DeepMind11 Sep 2026marked alternatives C and D as solved and linked OpenAI's Lean proof
- Clay Mathematics Institute11 Sep 2026says the problem "has apparently been settled" and that its process is "deliberately unhurried"
- Lean, run by OpenAI8 Sep 2026OpenAI's Lean proofs of alternatives C and D are public, with a Comparator setup against the Formal Conjectures statements that allows only Lean's three standard axioms

Free weekly AI × math briefing

Every Saturday: the week's AI math claims and who checked them. Read past issues

- Free
- Every Saturday
- Unsubscribe in one click
- Read past issues

## What would settle it

An expert in the field from outside the team saying the written proof holds, or a journal accepting it.

## Sources

## Our video

## Want to know when one of these claims moves?

One email every Saturday: which claims moved that week, which ones are new, and who did the checking.

- Free
- Every Saturday
- Unsubscribe in one click
- Read past issues
