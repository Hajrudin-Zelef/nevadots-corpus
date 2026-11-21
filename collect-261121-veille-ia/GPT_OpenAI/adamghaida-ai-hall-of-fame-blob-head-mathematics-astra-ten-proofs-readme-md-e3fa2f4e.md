---
id: collect-261121-veille-ia/gpt-openai/adamghaida-ai-hall-of-fame-blob-head-mathematics-astra-ten-proofs-readme-md-e3fa2f4e
title: "Adamghaida ai hall of fame blob head mathematics astra ten proofs readme md"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["lean", "mathematics", "results", "every", "conjecture", "manuscript", "openai", "model"]
source: ""
source_anchor: ""
source_lines: [1, 18]
section: "GPT_OpenAI"
---
Field: 🧮 Mathematics · When: August 2026
A 249-page manuscript, ten previously-open results in mathematics and theoretical computer science, and a machine-checked Lean 4 proof for every single one, published together under an open licence.
On August 1, 2026, OpenAI published Ten Advances in Mathematics and Theoretical Computer Science, a 249-page manuscript reporting ten new results produced by an unreleased internal model the company named Astra. What sets the release apart from the usual "AI solved a math problem" announcement is what shipped alongside it: a public GitHub repository, openai/ten-proofs, containing a Lean 4 formalization of every result, Apache-2.0 licensed.
The ten results, as listed by the repository and the manuscript abstract:
- High-dimensional sphere packing — the exact asymptotic strength of the Cohn–Elkies linear program is determined, giving the first improvement to the general high-dimensional sphere-packing exponent since 1978 (the Kabatianskii–Levenshtein exponent 0.59905… improves to 0.6044…).
- Binary and spherical codes — classical upper bounds improved by exponential factors at every minimum distance.
- Non-sofic groups — an explicit construction of a non-sofic group. Soficity was introduced by Gromov in 1999 and named by Weiss, who asked whether a non-sofic group exists; the manuscript proves the unit group of the binary Leavitt algebra is not sofic.
- Connes's rigidity conjecture — disproved, by constructing infinitely many pairwise non-isomorphic property-(T) groups sharing the same group von Neumann algebra.
- Arithmetic circuit complexity — for the permanent, division-free circuits require Ω(n² log log n) gates and formulas require Ω(n⁴ / log n) leaves.
- Quantum parallel repetition — exponential parallel repetition for every finite two-player entangled game.
- Closest vector problem — n^(1/400)-factor hardness of approximation for Euclidean CVP, via a direct reduction from 3SAT.
- Ehrhart's volume conjecture — the sharp bound (n+1)ⁿ/n! in every dimension.
- Multicolor Ramsey numbers — a superexponential lower bound proving R_k(3) = k^Θ(k), resolving Erdős problem 183.
- Compactness and degeneracy — counterexamples to the Erdős–Simonovits compactness conjecture and an Erdős degeneracy conjecture, resolving Erdős problems 146 and 180.
The repository's own formalization.yaml records sorry_count: 0 for the project and for each of the twelve named main declarations, with no axioms beyond Lean's standard three (propext, Classical.choice, Quot.sound). It also ships ComparatorChallenges/, a set of standalone Lean files that restate each headline theorem with the proof left as sorry, so an outside party can check that the shipped proofs really discharge those statements using the Comparator independent checker. OpenAI reported the division of labour plainly: the mathematical arguments came from the model, human researchers wrote them up into manuscripts, and the model then produced the Lean formalizations. Reported token cost was roughly $2,000 at GPT-5.6 Sol API rates — though sources disagree on whether that is the total across all ten problems or the figure for each one, and either way it prices inference tokens only (see CONTEXT.md).
Most AI-math announcements ask you to trust a PDF. This one attaches a proof object a skeptic can run themselves: lake build All either succeeds or it does not. That collapses one entire category of doubt (did the model hallucinate a step?) into a reproducible command, and it is the reason Thomas Bloom, who maintains erdosproblems.com and was the sharpest critic of OpenAI's botched October 2025 Erdős claims, called these results "big news."
It does not collapse the other category. A Lean build proves the Lean statement, and mathematicians still have to confirm that statement faithfully encodes the conjecture everyone thought was open. None of the ten results has been peer reviewed; the repository's own metadata marks its review status as agent-reviewed. See CONTEXT.md for the full set of caveats.
In this folder: PROMPT.md (a prompt to explore it with AI) · CONTEXT.md (background & caveats). Part of the AI Hall of Fame → 🧮 Mathematics.
