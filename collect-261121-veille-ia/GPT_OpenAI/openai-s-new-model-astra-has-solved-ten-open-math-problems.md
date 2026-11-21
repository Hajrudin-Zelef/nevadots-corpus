---
id: collect-261121-veille-ia/gpt-openai/openai-s-new-model-astra-has-solved-ten-open-math-problems
title: "OpenAI's New Model, Astra, Has Solved Ten Open Math Problems"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["astra", "field", "problems", "open", "theory", "proved", "openai", "model"]
source: https://www.datacamp.com/blog/open-ai-model-astra-solved-ten-open-math-problems
source_anchor: ""
source_lines: [1, 216]
section: "GPT_OpenAI"
---
On August 1, OpenAI published a report claiming its next model, internally called Astra (not available yet), had produced new results on ten different open problems in mathematics. (No, they are not the Millennium Prize problems, but they are significant nonetheless.)

What's noteworthy: These solutions are not incremental progress on the problems: they are genuine resolutions, verified with Lean. (Lean is a programming language and proof assistant that forces every step of a mathematical argument to be spelled out in machine-readable detail.) 

It's a lot to take in at once. In this article, I organized the problems by field and describe at a high-level what happened, and what this might imply for mathematics as a field, and glean what else we might know about Astra.

## What Are the Ten Problems?

Here's each result, in plain terms, along with the specific field of math or computer science it belongs to.

### Non-sofic groups

**Field:** Group theory

Astra produced an explicit construction of a group that cannot be approximated, no matter how closely, by large finite structures — closing a question open since the concept of a "sofic" group was introduced in 1999. The construction comes with a proof that no sequence of finite approximations can ever work, formalized in Lean so the logic can be checked mechanically.

Wikipedia has already been updated:


### Sphere packing

**Field:** High-dimensional geometry

The question is how densely identical, non-overlapping spheres can be packed together as the number of dimensions grows. Astra proved a tighter ceiling on that density in high dimensions, the first improvement to this particular bound since 1978. It doesn't hand over a better packing method — it narrows how good any future method could possibly be.

### Binary and spherical codes

**Field:** Coding theory

Error-correcting codes work by keeping valid messages far enough apart that small errors can't turn one into another. Astra proved dramatically tighter (exponentially improved) limits on how many such messages can exist for a given minimum distance, with a matching result for points spread across a high-dimensional sphere.

### Connes's rigidity conjecture

**Field:** Operator algebras

Alain Connes conjectured that certain groups could always be uniquely reconstructed from an algebraic structure, called a von Neumann algebra, built out of them. Astra disproved this by producing two genuinely different groups that give rise to the same algebra, showing the reconstruction isn't always one-to-one.

### Arithmetic circuit complexity

**Field:** Computational complexity theory

The "permanent," a single number computed from a grid of numbers, is expensive to calculate, and complexity theorists want to know the minimum number of arithmetic steps any method could possibly need. Astra proved a new, stronger lower bound on that minimum — a notoriously hard kind of result to move at all.

### Quantum parallel repetition

**Field:** Quantum complexity theory

Classical theory says that making two non-communicating players repeat a hard game many times in parallel makes cheating exponentially less likely to succeed. Astra proved the same guarantee holds even when the players share quantum entanglement, extending a foundational classical principle into the quantum setting.

### Closest vector problem

**Field:** Lattice-based cryptography

Given a repeating grid of points (a lattice) and a target location, this problem asks for the nearest grid point — a problem believed to be very hard in high dimensions, which is why it underpins some quantum-resistant encryption. Astra proved that even approximating the answer, within a specific polynomial factor, remains provably hard, reinforcing the cryptography built on top of it.

### Ehrhart's volume conjecture

**Field:** Discrete and convex geometry

For a convex shape whose only interior grid point sits exactly at its center of mass, mathematicians wanted to know the largest possible volume such a shape could have in any given dimension. Astra worked out that maximum volume for every dimension, settling the conjecture in full generality.

### Multicolor Ramsey numbers

**Field:** Ramsey theory / combinatorics

With enough people and enough categories of relationship between them, you're eventually guaranteed to find three people all connected by the same category. Astra proved that the minimum group size needed grows faster than any fixed exponential rate as the number of categories increases, resolving Erdős problem 183.

### Extremal number conjectures

**Field:** Extremal graph theory

This branch of math asks how many connections a network can have while still avoiding certain small, forbidden patterns. Astra resolved two related conjectures here, corresponding to Erdős problems 146 and 180, pinning down how dense such networks can get before those patterns become unavoidable.

Each of these had been open for at least a decade; several had stood for thirty years or more, including problems worked on by Turing Award winners in the theoretical CS side of the list.

## Wait, Is a Counterexample the "Easy" Kind of Proof?

Who am I to say anything negative here, but I know this is a common question or reaction, especially from people who know something about math.

The most common thing people say, by way of critique: Several of these results are counterexamples rather than new general theory. This idea is important because a counterexample answers a yes/no question but doesn't, by itself, tell you *why* the pattern breaks or hand you a family of similar objects to study next, whereas something else, like a classification theorem or a new technique opens more doors.

I would say this critique holds up in general, but it doesn't land as a blanket dismissal of this whole batch of problems. First, the non-sofic groups result isn't a small tweak to an existing near-miss — it's the first construction of its kind after 27 years where no one had one at all, and the technique behind it is expected to generalize to finding others.

Second, several of the other nine results, including the sphere-packing bound and the CVP hardness result, aren't counterexamples at all; they're direct improvements to standing bounds. 

## What's Still Unresolved

A few things are worth tracking as this gets picked apart by the field over the coming days, weeks, and months:

- **No peer review yet.** These are Lean-verified and reviewed informally by mathematicians who've seen preprints, but none have gone through a refereed journal process.
- **Authorship is still being negotiated.** OpenAI says it takes responsibility for the manuscripts and Lean formalizations, while attributing the mathematical arguments themselves to the model. Independent replication of the*process* (as opposed to verification of the*proofs* ) is hard to do right now.

## What This Means for Mathematics

The most immediate shift is in what mathematicians might actually spend their time on. If a well-posed open problem can be handed to a model and checked in Lean, the bottleneck moves from "can anyone solve this" to "did we ask the right question and formalize it correctly." The skill of posing a good problem and knowing which one is worth attacking is a real skill that mathematicians develop from experience. 

There's also a funding and credibility question simmering. Research grants, tenure cases, and prizes have historically been built around scarcity: these problems were hard enough that solving one said something about the solver. If AI-assisted results become routine, the field will need new ways to signal what's genuinely hard versus what's now within reach of a few thousand dollars of inference. Of course, the average person doesn't fully understand these problems. The people will real training do, and that doesn't change. So math knowledge is more valuable than ever. 

## How People Are Reacting

Reaction on social media split less over whether the proofs check out, and more over what they're evidence of.

Some read the pace itself as the real story: ten decades-old problems landing at once, across unrelated fields, faster than experts can even review them. A question for the future: "Will we be able to keep up with checking all this?"

Others push back that this says less about AI in general than it seems to. Math is a rare domain where a model's work can be checked automatically and completely. Most real-world problems don't offer that kind of built-in, automatic answer key. By that view, the achievement is real, but it may say more about math being unusually well-suited to AI.

A third strand of commentary: A correct proof that nobody has fully audited or absorbed hasn't really been understood yet, just verified. Discovering a theorem and understanding what it means, in this view, are two different jobs.

## Final Thoughts

Mathematicians are saying non-sofic groups result looks like the real thing: a genuine, decades-old open question in group theory, closed by an explicit construction that mathematicians in the field are taking seriously. The other nine results, taken together, represent a broad and technically substantial batch of progress across pure math.

What hasn't happened yet is the slower part: peer review, replication of the search process, and the field actually building on top of these results. That part takes longer than a blog post, and it's the part that will really tell us how big this was. We'll keep you updated.

On August 1, OpenAI published a report claiming its next model, internally called Astra (not available yet), had produced new results on ten different open problems in mathematics. (No, they are not the Millennium Prize problems, but they are significant nonetheless.)

What's noteworthy: These solutions are not incremental progress on the problems: they are genuine resolutions, verified with Lean. (Lean is a programming language and proof assistant that forces every step of a mathematical argument to be spelled out in machine-readable detail.)

It's a lot to take in at once. In this article, I organized the problems by field and describe at a high-level what happened, and what this might imply for mathematics as a field, and glean what else we might know about Astra.

## What Are the Ten Problems?

Here's each result, in plain terms, along with the specific field of math or computer science it belongs to.

## Non-sofic groups

Field:Group theory
Astra produced an explicit construction of a group that cannot be approximated, no matter how closely, by large finite structures — closing a question open since the concept of a "sofic" group was introduced in 1999. The construction comes with a proof that no sequence of finite approximations can ever work, formalized in Lean so the logic can be checked mechanically.

Wikipedia has already been updated:


## Sphere packing

Field:High-dimensional geometry
The question is how densely identical, non-overlapping spheres can be packed together as the number of dimensions grows. Astra proved a tighter ceiling on that density in high dimensions, the first improvement to this particular bound since 1978. It doesn't hand over a better packing method — it narrows how good any future method could possibly be.

## Binary and spherical codes

Field:Coding theory
Error-correcting codes work by keeping valid messages far enough apart that small errors can't turn one into another. Astra proved dramatically tighter (exponentially improved) limits on how many such messages can exist for a given minimum distance, with a matching result for points spread across a high-dimensional sphere.

## Connes's rigidity conjecture

Field:Operator algebras
Alain Connes conjectured that certain groups could always be uniquely reconstructed from an algebraic structure, called a von Neumann algebra, built out of them. Astra disproved this by producing two genuinely different groups that give rise to the same algebra, showing the reconstruction isn't always one-to-one.

## Arithmetic circuit complexity

Field:Computational complexity theory
The "permanent," a single number computed from a grid of numbers, is expensive to calculate, and complexity theorists want to know the minimum number of arithmetic steps any method could possibly need. Astra proved a new, stronger lower bound on that minimum — a notoriously hard kind of result to move at all.

## Quantum parallel repetition

Field:Quantum complexity theory
Classical theory says that making two non-communicating players repeat a hard game many times in parallel makes cheating exponentially less likely to succeed. Astra proved the same guarantee holds even when the players share quantum entanglement, extending a foundational classical principle into the quantum setting.

## Closest vector problem

Field:Lattice-based cryptography
Given a repeating grid of points (a lattice) and a target location, this problem asks for the nearest grid point — a problem believed to be very hard in high dimensions, which is why it underpins some quantum-resistant encryption. Astra proved that even approximating the answer, within a specific polynomial factor, remains provably hard, reinforcing the cryptography built on top of it.

## Ehrhart's volume conjecture

Field:Discrete and convex geometry
For a convex shape whose only interior grid point sits exactly at its center of mass, mathematicians wanted to know the largest possible volume such a shape could have in any given dimension. Astra worked out that maximum volume for every dimension, settling the conjecture in full generality.

## Multicolor Ramsey numbers

Field:Ramsey theory / combinatorics
With enough people and enough categories of relationship between them, you're eventually guaranteed to find three people all connected by the same category. Astra proved that the minimum group size needed grows faster than any fixed exponential rate as the number of categories increases, resolving Erdős problem 183.

## Extremal number conjectures

Field:Extremal graph theory
This branch of math asks how many connections a network can have while still avoiding certain small, forbidden patterns. Astra resolved two related conjectures here, corresponding to Erdős problems 146 and 180, pinning down how dense such networks can get before those patterns become unavoidable.

Each of these had been open for at least a decade; several had stood for thirty years or more, including problems worked on by Turing Award winners in the theoretical CS side of the list.

## Wait, Is a Counterexample the "Easy" Kind of Proof?

Who am I to say anything negative here, but I know this is a common question or reaction, especially from people who know something about math.

The most common thing people say, by way of critique: Several of these results are counterexamples rather than new general theory. This idea is important because a counterexample answers a yes/no question but doesn't, by itself, tell you

whythe pattern breaks or hand you a family of similar objects to study next, whereas something else, like a classification theorem or a new technique opens more doors.
I would say this critique holds up in general, but it doesn't land as a blanket dismissal of this whole batch of problems. First, the non-sofic groups result isn't a small tweak to an existing near-miss — it's the first construction of its kind after 27 years where no one had one at all, and the technique behind it is expected to generalize to finding others.

Second, several of the other nine results, including the sphere-packing bound and the CVP hardness result, aren't counterexamples at all; they're direct improvements to standing bounds.

## What's Still Unresolved

A few things are worth tracking as this gets picked apart by the field over the coming days, weeks, and months:

No peer review yet.These are Lean-verified and reviewed informally by mathematicians who've seen preprints, but none have gone through a refereed journal process.Authorship is still being negotiated.OpenAI says it takes responsibility for the manuscripts and Lean formalizations, while attributing the mathematical arguments themselves to the model. Independent replication of theprocess(as opposed to verification of theproofs) is hard to do right now.
## What This Means for Mathematics

The most immediate shift is in what mathematicians might actually spend their time on. If a well-posed open problem can be handed to a model and checked in Lean, the bottleneck moves from "can anyone solve this" to "did we ask the right question and formalize it correctly." The skill of posing a good problem and knowing which one is worth attacking is a real skill that mathematicians develop from experience.

There's also a funding and credibility question simmering. Research grants, tenure cases, and prizes have historically been built around scarcity: these problems were hard enough that solving one said something about the solver. If AI-assisted results become routine, the field will need new ways to signal what's genuinely hard versus what's now within reach of a few thousand dollars of inference. Of course, the average person doesn't fully understand these problems. The people will real training do, and that doesn't change. So math knowledge is more valuable than ever.

## How People Are Reacting

Reaction on social media split less over whether the proofs check out, and more over what they're evidence of.

Some read the pace itself as the real story: ten decades-old problems landing at once, across unrelated fields, faster than experts can even review them. A question for the future: "Will we be able to keep up with checking all this?"

Others push back that this says less about AI in general than it seems to. Math is a rare domain where a model's work can be checked automatically and completely. Most real-world problems don't offer that kind of built-in, automatic answer key. By that view, the achievement is real, but it may say more about math being unusually well-suited to AI.

A third strand of commentary: A correct proof that nobody has fully audited or absorbed hasn't really been understood yet, just verified. Discovering a theorem and understanding what it means, in this view, are two different jobs.

## Final Thoughts

Mathematicians are saying non-sofic groups result looks like the real thing: a genuine, decades-old open question in group theory, closed by an explicit construction that mathematicians in the field are taking seriously. The other nine results, taken together, represent a broad and technically substantial batch of progress across pure math.

What hasn't happened yet is the slower part: peer review, replication of the search process, and the field actually building on top of these results. That part takes longer than a blog post, and it's the part that will really tell us how big this was. We'll keep you updated.
