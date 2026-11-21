---
id: collect-261121-veille-ia/gpt-openai/openai-s-astra-solves-ten-decade-old-math-problems-with-machine-checkable-lean-proofs
title: "OpenAI's Astra Solves Ten Decade-Old Math Problems With Machine-Checkable Lean Proofs"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: ["2026-06-02"]
keywords: ["openai", "astra", "lean", "group", "problems", "every", "machine-checkable", "certificate"]
source: https://www.techtimes.com/articles/322710/20260802/openais-astra-solves-ten-decade-old-math-problems-machine-checkable-lean-proofs.htm
source_anchor: ""
source_lines: [1, 65]
section: "GPT_OpenAI"
---
OpenAI's unreleased Astra model has produced what its October 2025 announcement only pretended to: genuine solutions to ten long-standing open problems in mathematics and theoretical computer science, each accompanied by a machine-checkable Lean 4 certificate that any reader with a laptop can verify independently. The proofs — spanning group theory, von Neumann algebras, high-dimensional geometry, quantum complexity, lattice cryptography, and extremal combinatorics — were published August 1 alongside a 249-page technical manuscript and a 62-page account of how the arguments came together. Every Lean 4 certificate file is publicly available on OpenAI's GitHub repository under an Apache 2.0 license.

That last detail is not a footnote. It is the structural change that separates this announcement from every prior AI-mathematics milestone. When OpenAI's model disproved the Erdős unit distance conjecture in May 2026, the validation came from nine external mathematicians reading and signing off on the argument — a social signal, strong but not reproducible without the same expertise. A Lean certificate is different. Lean 4's trusted kernel gives a binary verdict: the proof either compiles or it doesn't. No PhD required. No months-long peer review backlog. Anyone can run the check.

Thomas Bloom, the University of Manchester mathematician who curates the Erdős problems catalogue at erdosproblems.com — and the same researcher who publicly dismantled OpenAI's October 2025 false claim — called the Astra results "big news" on X. He rated them more significant than the unit distance counterexample OpenAI published three months earlier. "Maybe not bigger than a proof of unit distance would have been," Bloom wrote, "but in terms of constructions, this is big."

### What Astra Actually Solved

The headline result is the construction of the first known non-sofic group — a counterexample to a question that stood open since Mikhail Gromov introduced the concept of soficity in 1999. A group is sofic if its behavior can be approximated arbitrarily well by finite permutation systems; all amenable groups and all residually finite groups — essentially every group mathematicians use in daily practice — satisfy this condition. Whether every countable discrete group must be sofic was one of modern group theory's most prominent unanswered questions for 27 years. Astra's construction answers it: no, not every group is sofic.

The second major result is a disproof of the Connes Rigidity Conjecture, posed by Fields Medalist Alain Connes in 1980. The conjecture concerns whether the von Neumann algebra L(G) constructed from a property (T) group G retains enough information to reconstruct G — in other words, whether these algebraic structures "remember" the groups that generated them. Astra's counterexample shows that, for at least some class of groups, they don't. Decades of rigidity theory in operator algebras will need to account for this boundary.

Three of the ten results resolve problems from Paul Erdős's famous catalogue of combinatorial mathematics questions, including Erdős problem 183 on multicolored Ramsey numbers, a subject connecting graph coloring to extremal combinatorics. The remaining results include the first improvement to the general upper bound on high-dimensional sphere-packing density since 1978, a parallel repetition theorem for two-player quantum games, new lower bounds on the circuit complexity of computing the permanent (a central challenge in theoretical computer science), and advances in lattice-based post-quantum cryptography.

OpenAI's head of mathematics research, Sébastien Bubeck, confirmed the results on X, calling each one "beautiful" and noting that every result ships with a Lean certificate and a chain-of-thought walkthrough. The total compute cost for finding all ten solutions was approximately $2,000 at current Sol API rates, according to the OpenAI announcement. Noam Brown, one of the researchers behind the test-time reasoning technology underlying Astra, acknowledged on X that the model failed to crack several other major targets — "Sadly, no Millennium Prize Problems (yet)" — while adding a detail that may be more significant than the results themselves: "We didn't spend a lot on each problem. It's possible to push test-time compute much further."

### Any Laptop Can Now Check the Proof

The Lean 4 certificate is the most consequential technical element in this announcement, and it merits more than a paragraph. Lean 4 is a proof assistant based on dependent type theory — specifically, the calculus of constructions with inductive types, developed by Leonardo de Moura at Microsoft Research and now maintained by the nonprofit Lean Focused Research Organization. The system's central property is its trusted kernel: every step in a proof must be verified against mathematical axioms by this kernel before the proof is accepted, and the verdict is binary. A proof file either compiles completely or it doesn't.

This matters because the prior bottleneck in AI-mathematics claims has been verification expertise. The Erdős unit distance conjecture result in May 2026 required nine external mathematicians to read and co-sign the argument. That is a strong form of validation, but it is a social one — it depends on the availability and willingness of qualified readers, and it cannot be reproduced without them. A Lean certificate changes that equation. OpenAI's GitHub repository for these ten proofs allows any researcher, developer, or technically literate reader to run the check themselves.

There is one caveat that experts have been careful to name. A successful Lean build confirms that the proof is valid for the theorem as formally stated inside Lean. It does not automatically confirm that the formal statement captures the open problem exactly as the mathematical community understood it. That alignment between formal statement and informal mathematical intuition is still a human judgment — and for each of these ten results, the mathematical community will need to examine the formal statements and confirm they are asking the right questions. BitsMinds summarized the distinction precisely: a Lean certificate is not a social signal, but it is not a complete substitute for mathematical judgment either.

### Why October 2025 Matters Here

Bloom's endorsement carries weight that requires context. In October 2025, then-OpenAI vice president Kevin Weil announced on X that GPT-5 had solved ten previously unsolved Erdős problems. Bloom — the same researcher who maintains erdosproblems.com and was later asked to verify the announcement — found that the model had not produced original proofs at all; it had retrieved existing solutions from the mathematical literature that Bloom personally had not catalogued. He called the announcement "a dramatic misrepresentation." Google DeepMind CEO Demis Hassabis described it as "embarrassing." Weil departed OpenAI in April 2026.

The current Astra announcement is, in a specific structural sense, the actual delivery of what the October 2025 announcement falsely claimed. Ten problems. Lean-verified. With Bloom himself calling it big news. OpenAI cited the Leiden Declaration on AI and Mathematics in the announcement, acknowledging that the mathematical ideas came from Astra rather than from the human researchers who prepared the manuscripts — a disclosure stance the declaration explicitly calls for.

The Leiden Declaration, published June 2, 2026, and endorsed by the International Mathematical Union with over 3,000 signatories including Terence Tao and Peter Scholze, identifies five risks from AI in mathematics: unreliable results, missing citations, dependence on closed commercial systems, exaggerated claims, and loss of scientific independence. OpenAI's decision to anchor this release in machine-verifiable Lean certificates addresses the first and fourth risks directly. The third — dependence on closed systems — remains unaddressed as long as Astra itself is not public.

### Astra: What It Is and What It Isn't

Astra is described by OpenAI as its "next major model family," distinct from the Sol, Terra, and Luna families currently in production. It is a multi-agent system: a root agent creates subagents, distributes portions of a problem, waits for results, and synthesizes a final answer — a design built for long-horizon tasks that can run for hours or days on a single objective. That same architecture, applied to the company's own inference infrastructure, produced results in safety testing that OpenAI disclosed in July 2026, covered in detail in an earlier TechTimes report.

Sam Altman demonstrated Astra in closed-door meetings with senators and senior administration officials in Washington on July 29, 2026, including meetings with Senators Raphael Warnock and Bernie Moreno, Treasury Secretary Scott Bessent, and Commerce Secretary Howard Lutnick. The Astra models are expected to be among the first to go through the U.S. government's pre-release review framework established by Executive Order 14409, signed June 2, 2026, which allows frontier model developers to voluntarily submit systems for federal evaluation before broader release.

Noam Brown was candid about where the ceiling is not: none of the Clay Mathematics Institute's seven Millennium Prize Problems — each carrying a $1 million award, and each representing a category of hardness that has resisted the concentrated effort of the global mathematics community for decades — fell to Astra. That distinction matters for calibrating what "a $2,000 compute run" means. The ten results Astra solved are genuine open problems, not trivial ones, but they are problems where Astra had enough mathematical theory to draw on and enough room to maneuver. Whether the same architecture can approach the hardest known problems is a different question, and Brown's candor suggests OpenAI does not yet know the answer.

### AI Mathematics' New Competitive Benchmark

Before August 1, the headline in AI-assisted mathematics belonged to DeepMind's AlphaProof Nexus, which solved nine Erdős problems with Lean-verified proofs in May 2026. Astra answered with ten, across a wider range of mathematical domains, with a model not purpose-built for formal reasoning. The more significant detail, as byteiota noted, is that both labs now use Lean 4 as their standard verification backend. The AI-mathematics field has settled on a single machine-checkable scoreboard — one where results can be compared across labs without requiring any party to trust another's word.

That convergence on Lean 4 is itself a technical development worth marking. The mathlib library — the community-maintained mathematical foundation for Lean 4 — contains over 210,000 formalized theorems and has been built over nine years by mathematicians at universities worldwide. When OpenAI produces a Lean 4 certificate, its argument is validated against that shared foundation. The infrastructure for machine-verifiable AI mathematics exists not because any one lab built it but because a research community spent a decade doing it.

Jakub Pachocki, OpenAI's chief scientist, has described the company's broader goal: an AI system with research-intern-level scientific capability by September 2026, and a fully autonomous AI researcher by early 2028, according to a TechCrunch report. Against that timeline, ten Lean-verified results across six mathematical domains is not the finish line — it is the evidence OpenAI is citing for how far along that path the model already is.

## Frequently Asked Questions

### What is Lean 4, and what does a Lean certificate actually prove?

Lean 4 is an open-source proof assistant developed by Leonardo de Moura, now maintained by the nonprofit Lean Focused Research Organization. It is based on the calculus of constructions with inductive types — a formal logical system where every proof must be verified step-by-step against mathematical axioms by a small, trusted kernel. The result is binary: a Lean certificate either compiles completely or it fails. For the Astra results, OpenAI posted all ten certificate files to a public GitHub repository, meaning anyone with the Lean compiler installed can verify each proof independently, without trusting OpenAI or needing a mathematics PhD. What a Lean certificate does not automatically confirm is whether the formal statement encoded in Lean accurately captures the open problem as the mathematical community understood it — that alignment judgment still requires domain expertise.

### What is a non-sofic group, and why does Astra's construction matter?

A group is sofic if its multiplication structure can be approximated arbitrarily well by finite permutation systems — think of it as an infinite algebraic object that can always be modeled closely enough by something finite and concrete. All groups that mathematicians commonly work with — amenable groups, residually finite groups — are sofic. Since Mikhail Gromov introduced soficity in 1999, whether every group is sofic was one of modern group theory's most prominent open questions. Astra's result shows the answer is no: at least one group exists that cannot be approximated this way. The implications extend to operator algebra theory, ergodic theory, and the study of cellular automata.

### How credible is this announcement given OpenAI's October 2025 false math claim?

The October 2025 announcement — in which then-OpenAI VP Kevin Weil claimed GPT-5 had solved ten Erdős problems — collapsed within days when Thomas Bloom demonstrated the model had retrieved existing solutions from the literature rather than producing original proofs. The August 2026 Astra announcement differs in three structural ways: the problems span six distinct mathematical domains rather than a single catalogue; the proofs are formalized in Lean 4 with machine-checkable certificates that any reader can verify; and Bloom himself — the researcher who dismantled the 2025 claim — called this result "big news" and rated it more significant than OpenAI's May 2026 Erdős unit distance result. That combination of machine verification and independent expert endorsement is substantively different from the social credibility that collapsed in 2025.

### Will Astra be publicly available, and does this confirm it is GPT-6?

OpenAI has not set a public release date for Astra and has not confirmed whether it will ship as GPT-6, a point release within the GPT-5 line, or a separate model class alongside Sol, Terra, and Luna. The company described it only as its "next major model family." Astra is also expected to be among the first models evaluated under the U.S. government's voluntary pre-release review process established by Executive Order 14409 — meaning any public release timeline will involve a federal evaluation window before broader access.
