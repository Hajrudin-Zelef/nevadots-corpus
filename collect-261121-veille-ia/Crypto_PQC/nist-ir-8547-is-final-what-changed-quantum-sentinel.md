---
id: collect-261121-veille-ia/crypto-pqc/nist-ir-8547-is-final-what-changed-quantum-sentinel
title: "NIST IR 8547 Is Final: What Changed | Quantum Sentinel"
domain: crypto-pqc
role: reference
task: article
actors: []
dates: ["2026-10-27"]
keywords: ["nist", "final", "after", "draft", "bits", "deprecated", "quantum-vulnerable", "algorithm"]
source: https://medium.com/quantum-sentinel/nist-ir-8547-is-final-what-changed-quantum-sentinel-3ec8c0835ad7
source_anchor: ""
source_lines: [1, 87]
section: "Crypto_PQC"
---
**The short answer:** NIST published the final version of IR 8547, “Transition to Post-Quantum Cryptography Standards”, on 27 October 2026, replacing the November 2024 draft. The biggest change is in the dates. The draft deprecated only 112-bit quantum-vulnerable algorithms after 2030. The final deprecates every quantum-vulnerable digital signature and key-establishment algorithm after 2030, including ECDSA on P-256, EdDSA and RSA-3072, and still disallows them all after 2035.

For anyone who planned around the draft, that removes the gap between weaker and stronger classical keys. This post sets out what changed between the draft and the final text, what stayed the same, and what it means for a cryptographic inventory.

## What NIST published

NIST Internal Report 8547 was first released as an Initial Public Draft on 12 November 2024, with comments due by 10 January 2025. The final report is dated October 2026, was approved by the NIST Editorial Review Board on 21 October 2026, and appears in the document history on NIST’s Computer Security Resource Center as final on 27 October 2026, the same day the draft was marked as withdrawn. The authors are unchanged: Dustin Moody, Ray Perlner, Andrew Regenscheid, Angela Robinson and David Cooper.

The report still describes NIST’s approach to moving from quantum-vulnerable algorithms to post-quantum signatures and key establishment. What is new is its framing. The abstract now says it “supports the implementation of Executive Order 14412 and OMB Memorandum M-26–15”, and the introduction calls it “the initial step in a broader strategy to manage and guide the transition to post-quantum cryptography”.

## The headline change: one date for every quantum-vulnerable algorithm

In the draft, Tables 2 and 4 split each quantum-vulnerable family by security strength. Parameters at 112 bits were “Deprecated after 2030” and “Disallowed after 2035”. Parameters at 128 bits or more carried a single entry: “Disallowed after 2035”. In the final, each family has one row covering every parameter set from 112 bits upward (128 bits upward for EdDSA), and every row reads “Deprecated after 2030” and “Disallowed after 2035”.

The wording behind the table changed in the same way. The draft said NIST “intends to instead deprecate classical digital signatures at the 112-bit security level”. The final says NIST “will instead deprecate all quantum-vulnerable digital signature algorithms after 2030 and disallow their use under NIST standards and guidelines after 2035”, and applies the same rule to key establishment.

To place real keys in those rows, NIST SP 800–57 Part 1 Revision 5 gives comparable strengths: RSA with a 2048-bit modulus provides 112 bits and a 3072-bit modulus provides 128 bits, while elliptic curve keys of 224 to 255 bits provide 112 bits and keys of 256 to 383 bits provide 128 bits. Under the draft, RSA-3072 and ECDSA on P-256 had no 2030 stage. Under the final, they are deprecated after 2030 like RSA-2048.

## Deprecated is not the same as banned

IR 8547 uses the approval-status terms from NIST SP 800–131A. Deprecated “means that the algorithm and key length/strength may be used, but there is some security risk. The data owner must examine this risk potential and decide whether to continue to use a deprecated algorithm or key length.” Disallowed means it is “no longer allowed for the stated purpose”. Legacy use covers processing already protected information, such as decrypting ciphertext or verifying a signature.

So from 2031, under NIST standards, using any classical public-key algorithm becomes a risk decision that a data owner has to examine and accept. The report says organisations “may continue using these algorithms and parameter sets” while they migrate. After 2035 they are disallowed.

## How it now fits the US federal deadlines

The draft pre-dated the 2026 federal instruments. The final is written around them. Executive Order 14412, signed on 22 June 2026, requires agencies to move high value assets and high impact systems, excluding National Security Systems, to post-quantum key establishment by 31 December 2030 and to post-quantum digital signatures by 31 December 2031. OMB Memorandum M-26–15, issued on 24 June 2026, directs a risk-prioritised migration with the objective of mitigating as much quantum risk as feasible by 31 December 2030.

The final report keeps the 2035 goal from National Security Memorandum 10: “That overall goal remains in place.” It describes the 2030 to 2035 window as one that “accommodates lower-risk systems and applications with legacy constraints while full migration concludes”, and states that the window “does not alter the earlier deadlines” for high value assets, high impact systems and other high-sensitivity federal systems. For those systems, it says, “the continued use of these deprecated algorithms is constrained by the earlier migration deadlines established under EO 14412”.

There is a timing point for agencies and their suppliers. M-26–15 requires agencies to align their PQC Migration Plans with “NIST Internal Report (IR) 8547, Transition to Post-Quantum Cryptography Standards, or successor document”, and links to the draft for more information. Those plans were due 120 days after the memorandum, on 22 October 2026. The final report arrived five days later, so plans written against the draft’s 112-bit split are worth checking against the final tables.

## Other changes in the final text

None of the following appears in the November 2024 draft.

- **Start with inventory.** Organisations “should treat the migration to PQC as a current operational priority, beginning with cryptographic inventories, migration planning, and early deployments of PQC-enabled products and services”.
- **Replace what cannot migrate.** “Systems that cannot support post-quantum cryptography should be identified and prioritized for replacement or decommissioning.”
- **TLS 1.3 first.** The report calls TLS “an early priority for the migration”, notes that agencies must support TLS 1.3 no later than 2 January 2030, and says guidance on quantum-resistant key establishment in TLS, including hybrid key exchange, will be in the next revision of SP 800–52.
- **A default ML-KEM parameter set.** “NIST recommends using ML-KEM-768 as the default parameter set, as it provides a large security margin at a reasonable performance cost.”
- **HQC.** HQC was selected as an alternative to ML-KEM, and a FIPS specifying it is under development. Once published, HQC will also be an approved post-quantum key-establishment scheme.
- **Hybrid key establishment.** The generic composite technique in SP 800–56C “is allowed, even if T is derived from an unapproved source (including a disallowed algorithm)”, and the order of the two shared secrets may be reversed.
- **Faster module validation.** As directed by EO 14412, NIST is revising Cryptographic Module Validation Program processes to accelerate validation of modules that implement PQC algorithms.
- **Hardware lead times.** Because hardware takes years to design and build, it is “a high priority for vendors to initiate the integration of PQC support immediately”.
- **Critical trust infrastructure.** A new paragraph uses the Personal Identity Verification standard as its example: issuance systems, smart cards, readers, middleware and PKI must all be updated, and because credentials follow multi-year replacement cycles, that work “must begin early in the transition period”.
- **Symmetric tables.** Ascon from SP 800–232 now appears alongside AES, SHA-2 and SHA-3, and a footnote states that SHA-1 “will be disallowed for use in FIPS-validated cryptographic modules after 2030”.

Some things did not change. ML-KEM remains the only approved post-quantum key-establishment scheme based on public-key cryptography. ML-DSA, SLH-DSA, LMS and XMSS remain the post-quantum signature standards, with a FALCON-derived standard still in development. NIST still does not expect to transition away from its symmetric standards as part of the migration, and its symmetric standards at the 112-bit level are still disallowed in 2030.

## What it means for a cryptographic inventor

The draft made key size the first question: RSA-2048 and RSA-3072 sat in different rows with different dates. Under the final, every RSA, ECDSA, EdDSA, Diffie-Hellman and MQV use at approved strengths carries the same two dates. The useful questions move elsewhere.

- **Purpose.** Separate key establishment from signatures. For federal high value assets and high impact systems they have different deadlines, 2030 and 2031, and key establishment carries the “harvest now, decrypt later” risk.
- **What protects long-lived data.** The report says systems protecting data with long-term confidentiality needs “warrant the earliest transitions”.
- **What cannot be upgraded.** Firmware, embedded devices and hardware modules that cannot take PQC belong on a replacement or decommissioning list.
- **Protocol versions.** Federal agencies must support TLS 1.3 by 2 January 2030, and the report treats it as the practical path to quantum-resistant key establishment in transit.
- **SHA-1 and 112-bit symmetric use.** These have their own 2030 dates, separate from the public-key schedule.
- **Risk decisions.** From 2031, every remaining classical use is a deprecated algorithm that someone has to accept the risk of.

Within the Enterprise Cryptographic Exposure Management (ECEM) lifecycle — Discover, Inventory, Assess, Prioritise, Transition, Monitor — the final IR 8547 mostly changes the Assess and Prioritise steps. An Enterprise CBOM that records algorithm, key size, purpose and location for each use can be re-scored against the final tables as a query, not a new discovery exercise.

## Frequently asked questions

## Is NIST IR 8547 final?

Yes. NIST published the final version of IR 8547, Transition to Post-Quantum Cryptography Standards, on 27 October 2026. It replaces the Initial Public Draft of 12 November 2024, which NIST has withdrawn.

## What changed between the draft and the final IR 8547?

The main change is the deprecation date. The draft deprecated only 112-bit quantum-vulnerable algorithms after 2030. The final deprecates all quantum-vulnerable digital signature and key-establishment algorithms after 2030 and disallows them after 2035. The final also aligns with EO 14412 and OMB M-26–15, recommends ML-KEM-768 as the default parameter set, and names TLS 1.3 as an early priority.

## Is ECDSA P-256 deprecated after 2030?

Yes, under the final IR 8547. ECDSA at 112 bits of classical security strength or more, which includes P-256, is deprecated after 2030 and disallowed after 2035. The draft had listed P-256 and other 128-bit parameters as disallowed after 2035 with no 2030 stage.

## Does deprecated mean RSA is banned after 2030?

No. In NIST’s terms, deprecated means the algorithm may still be used, but there is some security risk and the data owner must examine it and decide whether to continue. Disallowed, which applies after 2035, means it is no longer allowed for the stated purpose. Federal high value assets and high impact systems face earlier deadlines under EO 14412.

## Which ML-KEM parameter set does NIST recommend?

The final IR 8547 recommends ML-KEM-768 as the default parameter set, because it provides a large security margin at a reasonable performance cost. Other parameter sets may be used where ML-KEM-768 is impractical or higher security is required.

Sources: NIST, IR 8547, Transition to Post-Quantum Cryptography Standards (final, October 2026) and the full text; NIST, IR 8547 Initial Public Draft (12 November 2024) and its full text; NIST, SP 800–57 Part 1 Revision 5, Table 2; The White House, Executive Order 14412, Securing the Nation Against Advanced Cryptographic Attacks (22 June 2026); OMB, M-26–15, Execution of the Migration to Post-Quantum Cryptography (24 June 2026).

Further reading: what “deprecated after 2030” means in NIST IR 8547, what OMB M-26–15 and EO 14412 mean for suppliers, the 22 October PQC migration plans and building an Enterprise CBOM.
