---
id: collect-261121-veille-ia/regulation-eu/nist-ir-8547-draft-to-final-sitg-consulting
title: "NIST IR 8547: Draft to Final | SITG-Consulting"
domain: regulation-eu
role: reference
task: article
actors: []
dates: []
keywords: ["final", "report", "draft", "nist", "transition", "post-quantum", "signatures", "cryptography"]
source: https://www.sitg-consulting.com/post/nist-ir-8547-what-changed-draft-to-final
source_anchor: ""
source_lines: [1, 161]
section: "Regulation_EU"
---
NIST IR 8547: What Changed Between the Draft and the Final Report

Brian Couzens

Oct 28

7 min read

Updated: Nov 1

l

NIST IR 8547: What Changed Between the Draft and the Final Report

Brian Couzens, CEO, SITG-Consulting | 28 October 2026

NIST published the final version of Interagency Report 8547 on 27 October 2026, nearly two years after the initial public draft appeared in November 2024. The report sets the transition timeline for moving from quantum-vulnerable public-key cryptography to post-quantum cryptographic (PQC) standards across U.S. federal systems.

The core threat model has not changed. Harvest now, decrypt later exposure, long transition lead times, public key infrastructure dependencies and the separation between confidentiality and authentication use cases were all set out in the draft. What changed is the operational posture: the final report moves from "prepare for post-quantum cryptography" to "prioritise, validate and deliver."

This analysis sets out the five material changes, what carried over unchanged, and what the final report means for organisations tracking the transition timelines, whether or not they sit inside the federal scope.

What NIST IR 8547 covers

NIST IR 8547, titled "Transition to Post-Quantum Cryptography Standards," is the National Institute of Standards and Technology's operational companion to the three post-quantum algorithm standards published in 2024: FIPS 203 (ML-KEM, key establishment), FIPS 204 (ML-DSA, digital signatures) and FIPS 205 (SLH-DSA, stateless hash-based signatures). It defines the deprecation and disallowance schedule for existing quantum-vulnerable schemes and sets out guidance on migration sequencing, hybrid approaches and hardware trust dependencies.

The report is not a new threat announcement. It codifies what has to happen, by when, and in what order.

What is not new in the final report

Five elements in the final report are carried forward from the 2024 draft without material change.

Harvest now, decrypt later. Long-lived encrypted data can be collected now and decrypted once quantum computing matures. This exposure case was the centrepiece of the draft's urgency argument, and it remains so.

Hybrid accommodation. The FIPS 140 accommodation for hybrid key establishment and dual signatures, including its cost, performance and complexity trade-offs, is draft-era text.

Long lead times. Standards development, product readiness, procurement cycles and integration work make this a multi-year transition, regardless of the algorithm timeline.

Public key infrastructure. Certificates, certification authorities and validation chains must evolve together. No single component migrates in isolation.

Use-case split. Confidentiality moves earlier than authentication. Verifiers that cannot be updated after deployment remain a distinct risk category.

One qualification applies: the final report adds that migrating digital signatures is also urgent, despite signatures carrying no harvest-now exposure, because the signature transition is longer and requires more parties to coordinate.

Five material changes in the final report

1. Two federal milestones now set the migration clock

The final report names two explicit deadlines for U.S. federal systems, scoped to high-value assets, high-impact systems and other high-sensitivity federal systems, excluding National Security Systems.

Post-quantum cryptography is required for key establishment by 31 December 2030. Post-quantum cryptography is required for digital signatures by 31 December 2031. These dates are set by Executive Order 14412 and OMB Memorandum M-26-15. NIST IR 8547 supports them.

The draft proposed 2035 disallowance for all security tiers and 2030 deprecation for the 112-bit tier only. The final introduces a staged obligation that the draft did not carry.

Additionally, agencies must support Transport Layer Security (TLS) version 1.3 no later than 2 January 2030. TLS is named as an early migration priority.

The planning implication is to build backward from product, validation and procurement lead times, rather than forward from the deadline date.

2. The 2030 deprecation step now covers all quantum-vulnerable algorithms

The draft deprecated only the 112-bit security tier (RSA-2048, P-224) after 2030. The final extends that deprecation to cover all approved quantum-vulnerable public-key algorithms, at every security tier, after 2030.

This means estates standardised on P-256, P-384, RSA-3072 or Ed25519 were outside the draft's 2030 step. They are inside it now. The language also hardened: "NIST intends to" became "NIST will."

All quantum-vulnerable public-key cryptography is disallowed after 2035, as before. But the 2030 deprecation step now catches a far larger installed base.

The final also names ML-KEM-768 as the recommended default parameter set for key establishment. The draft named no default.

3. Priority is set by exposure lifetime and criticality

The final report turns the draft's confidentiality-lifetime argument into an explicit operating instruction. Three lenses determine where to start.

Exposure lifetime: how long must confidentiality or authenticity hold? Migration runway: how long do product, validation and rollout dependencies take? Mission impact: what fails if the transition slips or a trust anchor cannot change?

The decision rule is to start where data lifetime plus migration runway reaches the quantum horizon, then weight by mission impact. Systems that cannot support PQC at all should be identified and prioritised for replacement or decommissioning.

4. Hardware trust and validation are the pacing items

Three clauses in the final report that did not appear in the draft set procurement lead times as a first-order constraint.

Hardware root of trust. The final calls for PQC in security-critical services, including software and firmware update integrity, and makes vendor integration an immediate priority. Stateful hash-based signatures are pointed at firmware signing.

Validation readiness. As directed by Executive Order 14412, NIST is revising Cryptographic Module Validation Program (CMVP) processes to accelerate validation of modules implementing PQC. That improves the path, but promises no fixed turnaround.

Trust infrastructure. Personal Identity Verification (PIV) credentials, their issuance systems, cards, readers, middleware and supporting public key infrastructure all follow multi-year replacement cycles. Updates must start early.

The procurement test for any vendor or product: ask for the algorithm roadmap, validation status, firmware-signing path and replacement lead time.

5. Hybrids are conditional bridges, not default destinations

The draft's FIPS 140 accommodation for hybrid key establishment and dual signatures carries forward, along with the requirement that cost, performance, complexity and independent security review justify the bridge and the expectation of a second transition to post-quantum only.

What the final adds: explicit inclusion of hybrids combining post-quantum algorithms built on different mathematical assumptions. An approved composite key-establishment technique can be FIPS 140 validated if at least one input comes from an approved scheme, even when another input is disallowed. Security needs case-by-case analysis.

HQC (Hamming Quasi-Cyclic) is confirmed as the backup key-encapsulation mechanism, selected in the fourth round as a structurally independent alternative to ML-KEM. Its FIPS standard is still under development. This is not a new 2026 selection.

The final also adds an explicit caution: early implementations may introduce implementation errors and side-channel vulnerabilities. SP 800-227 on key-encapsulation mechanisms, which was forthcoming in the draft, is now published and cited.

What this means for your migration planning

NIST IR 8547 is scoped to U.S. federal systems. The deadlines are not worldwide mandates. But the underlying logic applies to any organisation holding long-lived data, operating public key infrastructure, or procuring cryptographic products: the transition is a multi-year programme, and hardware, validation and trust infrastructure set the pace, not the algorithm standards alone.

The fastest credible path runs through three phases.

In the first 90 days: name one accountable migration owner; inventory algorithms, protocols, public key infrastructure and hardware; classify data by confidentiality and authenticity life. Our PQC Discovery Sprint is built for exactly this stage.

Between three and six months: rank systems by impact and migration runway; define target patterns and hybrid exception criteria; put PQC, validation and upgrade paths into vendor requirements. A structured PQC Readiness Assessment turns the inventory into a prioritised roadmap.

Between six and twelve months: pilot supported PQC in priority use cases; test interoperability, performance and recovery; fund the roadmap and date exceptions and exits. The Cryptographic Transformation and Modernisation Methodology provides the lifecycle framework for sustained delivery.

Three questions for the next leadership meeting: what information must still be confidential in 2030? Which trust anchors cannot be upgraded after deployment? Which purchases now need a PQC and validation roadmap?

Frequently asked questions

What is NIST IR 8547?

NIST IR 8547 is the National Institute of Standards and Technology's report setting out the transition to post-quantum cryptographic standards. It defines deprecation and disallowance timelines for quantum-vulnerable public-key algorithms across U.S. federal systems.

What changed between the NIST IR 8547 draft and the final report?

Five material changes: two explicit federal deadlines (2030 for key establishment, 2031 for digital signatures), wider algorithm deprecation covering all quantum-vulnerable tiers rather than only 112-bit, explicit hardware and validation dependencies, conditional hybrid guidance, and a priority framework based on exposure lifetime and mission impact.

When must federal agencies migrate to post-quantum cryptography?

Post-quantum cryptography is required by 31 December 2030 for key establishment and by 31 December 2031 for digital signatures, on high-value assets and high-impact U.S. federal systems. These dates are set by Executive Order 14412 and OMB Memorandum M-26-15.

Which algorithms are deprecated under NIST IR 8547?

All quantum-vulnerable public-key algorithms approved under NIST standards are deprecated after 2030 and disallowed after 2035. This includes RSA-2048, RSA-3072, P-224, P-256, P-384 and Ed25519, at every security tier.

What is ML-KEM-768?

ML-KEM-768 is a parameter set of the Module-Lattice-Based Key-Encapsulation Mechanism standardised in FIPS 203. The final report recommends it as the default for key establishment. The draft named no default.

What is HQC?

HQC (Hamming Quasi-Cyclic) is a code-based key-encapsulation mechanism selected by NIST as a structurally independent backup to ML-KEM. It uses a different mathematical foundation from the lattice-based ML-KEM, providing algorithmic diversity. Its FIPS standard is still under development.

Does NIST IR 8547 apply outside the United States?

The federal deadlines apply to U.S. government systems. However, the underlying logic applies to any organisation holding long-lived data or procuring cryptographic products: the transition timeline, hardware dependencies, and validation constraints are structural, not jurisdictional. See our FAQ on Quantum Risk, PQC Governance and Cyber Resilience for further background.

Sources

NIST Interagency Report 8547, "Transition to Post-Quantum Cryptography Standards," final, 27 October 2026.

NIST Interagency Report 8547 ipd, initial public draft, November 2024.

Executive Order 14412, referenced in NIST IR 8547 for the 2030 and 2031 federal deadlines.

FIPS 203, Module-Lattice-Based Key-Encapsulation Mechanism Standard (ML-KEM), August 2024.

FIPS 204, Module-Lattice-Based Digital Signature Standard (ML-DSA), August 2024.

FIPS 205, Stateless Hash-Based Digital Signature Standard (SLH-DSA), August 2024.

NIST IR 8545, Status Report on the Fourth Round of the NIST Post-Quantum Cryptography Standardization Process (HQC selection), March 2025.

SP 800-227, Recommendations for Key-Encapsulation Mechanisms, 2026.

The full NIST IR 8547 comparison briefing can be downloaded. For more on how SITG-Consulting supports organisations through the post-quantum transition, visit our services page.
