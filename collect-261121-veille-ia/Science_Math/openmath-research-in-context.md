---
id: collect-261121-veille-ia/science-math/openmath-research-in-context
title: "OpenMath · Research in context"
domain: science-math
role: reference
task: article
actors: []
dates: []
keywords: ["smooth", "statement", "sources", "forced", "exact", "three-torus", "unforced", "velocity"]
source: https://openmath.dev/problems/navier-stokes?version=6
source_anchor: ""
source_lines: [1, 186]
section: "Science_Math"
---
## Overview

### Current assessment

OpenAI reports smooth forced breakdown in the whole space and on the three-torus, with complete formal proofs reported by the project. The new arguments are under assessment; unforced regularity and official evaluation have separate scopes.

### Open questions

Unforced alternatives A/B remain separate unresolved targets. The stronger manuscript construction has properties beyond the formal C/D nonexistence statements.

### Latest

Precise statement

Velocity u and pressure p satisfy and div u = 0, with viscosity ν > 0. The official question distinguishes whole-space ℝ³ from the periodic three-torus and unforced global regularity (A/B) from forced breakdown (C/D). Smooth, rapidly decaying data/force and uniformly bounded kinetic energy are required in the whole-space formulation.

### Definitions

Question paraphrased by OpenMath. Original wording is available in the sources.

## Landscape

Figures and explorable examples. Selecting a statement opens its exact wording.

## Results

### The question 1

### Proposed 3

### Conditional implications 1

### Variants and analogues 1

## Compare statements · 6

Each row is a separate assertion. Its assumptions determine which question it answers.

| Statement scopes and hypotheses |  |  |  |  | 
|---|---|---|---|---|
| Statement | Domain | Force | Viscosity | Data / conclusion | 
|---|---|---|---|---|
| Unforced global regularity in the whole space → Open Exact statement & sources | Whole space ℝ³ | None | Every ν > 0 | Rapidly decreasing smooth data; global smooth solution with uniformly bounded energy | 
| Unforced global regularity on the three-torus → Open Exact statement & sources | Unit three-torus | None | Every ν > 0 | Smooth periodic data; global smooth periodic solution | 
| Forced whole-space nonexistence → Proposed Exact statement & sources | Whole space ℝ³ | Smooth, rapidly decreasing in space-time | Every ν > 0 | Rapidly decreasing smooth data; no global smooth solution with uniformly bounded energy | 
| A singular forced flow starting from rest → Proposed Exact statement & sources | Whole space ℝ³ | Smooth and compactly supported | Every ν > 0 | Starts from rest; velocity and pressure supported in one fixed compact spatial set before time one | 
| Forced breakdown on the three-torus → Proposed Exact statement & sources | Unit three-torus | Smooth periodic forcing | Every ν > 0 | Smooth periodic data; forced breakdown | 
| Unbounded compactly supported velocity prevents smooth continuation → Proved Exact statement & sources | One fixed compact spatial support | Not specified by this continuity implication | Not an assumption of this implication | Unbounded velocity before T obstructs a smooth agreeing continuation; no existence construction is certified | 

## Connections

### Consequences 1

Assuming A singular forced flow starting from restProposed

- Finite energy alone does not ensure a smooth continuation ConditionalIf the reported compactly supported flow is valid, bounded kinetic energy can coexist with a velocity singularity that prevents a smooth continuation. Limit This consequence is conditional on the reported singular flow and supplies no unforced example. ## Sources & reviewIf a smooth velocity field on ℝ³ × [0,T) is supported in one fixed compact spatial set and its spatial supremum norm has infinite limsup as t ↑ T < ∞, then it has no smooth continuation to ℝ³ × [0,∞) that agrees with it before T. A conditional consequence of continuity on a compact space-time set; it does not certify existence of a singular Navier–Stokes solution. 
  - Finite Time Blowup for Navier–Stokes ↗Theorem 1.1; Corollary 10.6; Section 1.1
 Source-grounded explanation · independent explanation review: pending.

## Approaches

How the problem is attacked, and what each line of attack cannot yet reach.

### Amplify localized oscillations while controlling the force

The manuscript builds a leading flow, high-order corrections and localized oscillations, then constructs smooth compact forcing and a whole-space breakdown example.

Cannot yet reach The PDE-term schematic explains the setup, not the analytic estimates of the proof.

## Timeline

## Evidence

| Evidence by kind |  | 
|---|---|
| Literature | 1 preprint · 2 projects Status OpenMath assessment · Oct 13, 2026.  | 
|---|---|
| Formal | 2 complete proofs 2 formalizations below ↓ Reported in sources: Pinned Lean C/D nonexistence theorems are reported complete, with zero sorry and standard logical axioms. They are distinguished from the manuscript’s stronger detailed singular-flow construction; OpenMath has not executed the project. | 
| Computation | None recorded | 
| Review | Not yet reviewed OpenMath assessment: OpenAI reports smooth forced breakdown in the whole space and on the three-torus, with complete formal proofs reported by the project. The new arguments are under assessment; unforced regularity and official evaluation have separate scopes. Source comparison and reader curation; independent mathematical assessment remains pending. OpenMath pilot collection · OpenMath source curation · assessed Oct 13, 2026 · review due Jan 13, 2027 Collection last checked Oct 13, 2026. | 

### Formalizations

## Lean: forced whole-space breakdownComplete formal proof reported externally

**Complete formal proof reported externally**

The formal theorem matches official alternative C with explicit decay/force and energy assumptions. The stronger from-rest compact singular-flow statement is separate and has no complete formalization asserted by this link. Metadata reports zero sorry and standard logical axioms only.

Statement match: Linked by OpenMath · independent review separate · checked Oct 13, 2026

## Declaration & revision

`NavierStokes.Comparator.navier_stokes_breakdown_R3`
Commit f9e8bc5b38b6e212696e8a30e3e91517af887bbd

## Lean: forced periodic breakdownComplete formal proof reported externally

**Complete formal proof reported externally**

Exact periodic nonexistence theorem for every positive viscosity. The reference definitions and independent execution are separate checks; no unforced theorem is linked to this declaration.

Statement match: Linked by OpenMath · independent review separate · checked Oct 13, 2026

## Declaration & revision

`NavierStokes.Comparator.navier_stokes_breakdown_periodic`
Commit f9e8bc5b38b6e212696e8a30e3e91517af887bbd

External proof reports do not establish an OpenMath run, independent statement correspondence, or independent mathematical review.

## Sources

Choose a starting point below. Each result links to its exact statement, cited passages and checks.

### Start here

- Original question
 Existence and Smoothness of the Navier–Stokes Equation →Consult the original question and its hypotheses.
- Proof source
 Finite Time Blowup for Navier–Stokes →Read the mathematical argument; inspect its statement for assessment and scope.
- Formal source
 Navier–Stokes and Euler formal proof sources →Inspect the formal statement, assumptions and reported checks.

### Bibliography 5

- Existence and Smoothness of the Navier–Stokes Equation →Charles L. Fefferman · Published · 2000 Equations (1)–(9) and alternatives (A)–(D)
- Finite Time Blowup for Navier–Stokes →OpenAI · Preprint · Sep 8, 2026 Theorem 1.1; Corollary 10.6; Section 1.1
- Clay Mathematics Institute: Navier–Stokes announcement →Clay Mathematics Institute · Project documentation · Sep 11, 2026 Announcement dated 11 September 2026; evaluation process
- On the Navier–Stokes Millennium Prize Problem →OpenAI · Project documentation · Sep 8, 2026 Our process; concurrent work; Euler and Navier–Stokes scope
- Navier–Stokes and Euler formal proof sources →OpenAI · Project documentation · Sep 8, 2026 formalization.yaml; exact main declarations and Comparator instructions

## Research activity · 1 entry

Formalization

### Compare manuscript, comparator statement and proof dependencies

Check the official decay/periodic predicates, force regularity, energy condition and imported proof adapters at the pinned theorems.

The inspected metadata is self-assessed; a local build and independent correspondence check are not attached.

Related statements · context, not additional proved progress

Selected literature; source observations and coverage limits are retained in the publication details.

### Contributors

OpenAI is the printed manuscript author and credits an internal multi-agent research-model workflow for the mathematical argument. The announcement and formal metadata credit GPT-6 Astra/Codex for formalization. Fefferman and CMI own the official statement and evaluation process; Lean, Mathlib and Formal Conjectures/Comparator contributors support the proof infrastructure. Alpöge and Buckmaster’s separately credited forced-Euler work is not this Navier–Stokes theorem.

## Authors & roles

Charles L. Fefferman

No specific AI contribution is attributed by this source record.

OpenAI

OpenAI attributes the argument to its internal research model. Bibliographic authorship is printed as OpenAI; individual human preparation and checking roles are not inferred.

Clay Mathematics Institute

No specific AI contribution is attributed by this source record.

OpenAI

OpenAI reports an internal multi-agent research-model workflow and later GPT-6 Astra formalization. It recognizes Alpöge and Buckmaster’s concurrent forced-Euler work, which is distinct from its unforced Euler result.

OpenAI

The project metadata names GPT-6 Astra with Codex for formalization and marks alignment review as self-assessed. Lean, Mathlib and independent statement/Comparator infrastructure have separate supporting roles.

These are source credits; they do not imply OpenMath accounts, endorsements or independent mathematical review.

## Record details

- Record version
- 6
- Classification
- MSC2020 35Q30 / 76D03
- Prerequisite level
- specialist

Assess the forced C/D proofs and exact statement correspondence. CMI’s official evaluation is a separate process.

Source observations & version history →
