---
id: collect-261121-veille-ia/gpt-openai/github-openai-ten-proofs-lean-certificates-accompanying-ten-proofs-in-mathematics-and-theoretical-co
title: "GitHub - openai/ten-proofs: Lean certificates accompanying ten proofs in mathematics and theoretical computer science"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["lean", "lake", "mathematics", "theoretical", "computer", "science", "openai", "bounds"]
source: https://github.com/openai/ten-proofs
source_anchor: ""
source_lines: [1, 53]
section: "GPT_OpenAI"
---
This repository contains Lean 4 formalizations of the results presented in Ten advances in mathematics and theoretical computer science by OpenAI.

1. 
**High-dimensional sphere packing:** Improved asymptotic upper bounds on
sphere-packing density, reaching the Cohn–Elkies threshold.
(`SpherePacking.lean` )
2. 
**Binary and spherical codes:** Exponentially stronger upper bounds for
binary codes at every minimum distance, together with corresponding bounds
for spherical codes. (`MetricCodes.lean` )
3. 
**Non-sofic groups:** A construction of a non-sofic group, resolving whether
every group admits finite permutation approximations.
(`NonSoficGroup.lean` )
4. 
**Connes’s rigidity conjecture:** A counterexample to the conjecture that
certain groups are determined by their group von Neumann algebras.
(`ConnesRigidity.lean` )
5. 
**Arithmetic circuit complexity:** New lower bounds for computing the
permanent with arithmetic circuits and formulas, including an$n^4 / \log n$ formula lower bound. (`Permanent.lean` )
6. 
**Quantum parallel repetition:** Exponential parallel repetition for
arbitrary finite, two-player quantum games.
(`QuantumParallelRepetition.lean` )
7. 
**Closest vector problem:** Polynomial-factor hardness of approximation for
the closest vector problem, with related consequences for decoding and
lattice problems. (`GapCVP.lean` )
8. 
**Ehrhart’s volume conjecture:** The sharp maximum volume in every dimension
for a convex body whose centroid is its only interior lattice point.
(`EhrhartVolumeInequality.lean` )
9. 
**Multicolor Ramsey numbers:** A superexponential lower bound for multicolor
triangle Ramsey numbers, resolving Erdős problem 183.
(`MulticolorTriangleRamsey.lean` )
10. 
**Extremal number conjectures:** Counterexamples to the compactness and
degeneracy conjectures in extremal graph theory, resolving Erdős problems
146 and 180.
(`CompactnessAndDegeneracy.lean` )

The project uses Lean 4.32.0, mathlib, and Lake. With elan installed, fetch the mathlib cache and build all ten formalizations with:

```
lake exe cache get
lake build All
```
To build an individual formalization, pass its module name to Lake:

`lake build SpherePacking`
For instructions on checking the formalizations with Comparator, see the ComparatorChallenges README.
