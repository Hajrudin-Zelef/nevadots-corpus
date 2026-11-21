---
id: collect-261121-veille-ia/crypto-pqc/nist-and-nsa-pqc-timelines-what-they-mean-for-enterprise-crypto-agility
title: "NIST and NSA PQC Timelines: What They Mean for Enterprise Crypto Agility"
domain: crypto-pqc
role: reference
task: article
actors: []
dates: []
keywords: ["nist", "algorithm", "security", "support", "cannot", "standards", "federal", "systems"]
source: https://www.futurex.com/blog/nist-nsa-pqc-timelines-crypto-agility
source_anchor: ""
source_lines: [1, 129]
section: "Crypto_PQC"
---
A hardware security module (HSM) may support the Module-Lattice-Based Digital Signature Algorithm (ML-DSA), but a field device that cannot verify an ML-DSA signature still cannot accept a firmware update signed with it. That is the central challenge of post-quantum cryptography (PQC) migration: algorithm support in one component does not prove that the full system can use it.

On October 27, 2026, the National Institute of Standards and Technology (NIST) finalized __NIST IR 8547__, its roadmap for moving away from quantum-vulnerable public-key cryptography.  

NIST plans to deprecate affected algorithms after 2030 and disallow them under its standards and guidelines after 2035. The final report applies the 2030 deprecation to all approved strengths, including RSA-3072 and P-256, which the 2024 draft left acceptable until 2035. Separate federal directives and National Security Agency (NSA) policies set specific milestones for designated government systems, including the dates below: 2027, 2030, and 2031.

## What does this mean for your organization?

First, determine which requirements apply to your workloads, then identify where vulnerable algorithms run and which applications, devices, and suppliers must change accordingly. Evaluate a complete workflow, such as a Transport Layer Security (TLS) connection or firmware-signing process: can every participant use the new algorithm, what happens when one cannot, and how do you recover?

## What NIST and NSA finalized, and which timelines apply?

NIST IR 8547 sets the direction for the transition of standards. It identifies quantum-vulnerable public-key mechanisms and points to the finalized FIPS 203 (Module-Lattice-Based Key-Encapsulation Mechanism, ML-KEM) for key establishment and FIPS 204 (ML-DSA) and FIPS 205 (Stateless Hash-Based Digital Signature Algorithm, SLH-DSA) for digital signatures. Deployment architectures, procurement schedules, and application-specific implementations still require decisions by standards bodies, vendors, and organizations.

| **Who or what** | **Milestone** | **What it means** | 
| **New commercial National Security Systems (NSS)** | Beginning 2027 | Under Committee on National Security Systems Policy (CNSSP) 15, as described by the NSA, new commercial NSS must support quantum-resistant algorithms. | 
| **Legacy NSS without PQC support** | 2030 | NSA identifies phaseout for legacy NSS that cannot support quantum-resistant algorithms. | 
| **Federal high-value assets and high-impact systems** **(excluding NSS)** | Key establishment by Dec. 31, 2030; signatures by Dec. 31, 2031 | Executive Order (EO) 14412 sets these deadlines. Office of Management and Budget (OMB) Memorandum M-26-15 excludes NSS and also prioritizes other systems with highly sensitive data or particular quantum risk. | 
| **Quantum-vulnerable public-key use under NIST standards** | Deprecated after 2030 at all approved strengths; disallowed after 2035 | NIST’s standards-transition schedule. Earlier application-specific rules or federal migration requirements can apply. | 

Sources: NSA announcement (Oct. 1, 2026); NIST IR 8547; EO 14412; OMB M-26-15 (June 24, 2026).

Read the table by system, not by date. A milestone for a new commercial NSS is not automatically a deadline for a federal high-value asset or high-impact system (excluding NSS), or an ordinary commercial application.

These federal milestones do not by themselves create a universal commercial deadline. Check sector and customer requirements, data confidentiality lifetimes, supplier roadmaps, and equipment replacement cycles; NIST’s direction can also influence procurement requirements.

The difference between deprecation and disallowance matters.

Under NIST terminology, a deprecated algorithm can remain permitted for a specified use, though it carries recognized risk. A disallowed algorithm cannot be used for that purpose. NIST also recognizes limited legacy use, such as verifying an earlier signature or processing previously protected information, so neither term implies that every key, certificate, or archived document ceases to function on a single date.

Whether an older signature remains acceptable in your system depends on the applicable policy, its purpose, archival or evidentiary requirements, verifier behavior, and future application guidance. Review those conditions before replacing a trust chain or discarding old verification material.

## Which cryptography and dependencies need to change?

Crypto agility is the ability to change cryptographic algorithms across dependent systems while preserving security and keeping operations running. In practice, that means updating and testing the relevant protocols, certificates, applications, and devices together. A stable interface to cryptographic services can reduce changes to application code, but the receiving verifier and protocol must also support the new algorithm.

NIST IR 8547 distinguishes quantum-vulnerable public-key schemes from approved symmetric cryptography. RSA and elliptic-curve mechanisms used for signatures and key establishment are within the scope of the transition. Approved symmetric primitives with adequate strength, including appropriately configured AES, do not face the same blanket PQC replacement requirement. 

Some weaker algorithms and parameter sets have separate phaseout rules.

An attacker can record encrypted traffic today and try to decrypt it later with a sufficiently capable quantum computer. This harvest-now, decrypt-later risk matters most when information must stay confidential for many years, making quantum-vulnerable key establishment a migration priority.

Digital signatures present a different problem.

The certificate issuer, application, and receiving verifier must all recognize the chosen signature scheme. A signing service might produce a PQC signature that an installed device cannot verify, particularly when the device’s verification software cannot be updated.

## Which cryptography and dependencies need to change?

NIST IR 8547 distinguishes quantum-vulnerable public-key schemes from approved symmetric cryptography. RSA and elliptic-curve mechanisms used for signatures and key establishment are within the scope of the transition. Approved symmetric primitives with adequate strength, including appropriately configured AES, do not face the same blanket PQC replacement requirement.

Some weaker algorithms and parameter sets have separate phaseout rules.

An attacker could store encrypted traffic today and try to decrypt it later with a sufficiently capable quantum computer.

This Harvest-Now, Decrypt-Later risk is especially relevant when confidential information must remain protected for many years. It makes quantum-vulnerable key-establishment methods a migration priority.

Digital signatures present a different problem.

The certificate issuer, application, and receiving verifier must all recognize the chosen signature scheme. A signing service might produce a PQC signature that an installed device cannot verify, particularly when the device’s verification software cannot be updated.

For NSS software and firmware signing, check the applicable Commercial National Security Algorithm Suite (CNSA) 2.0 profile and validation requirements. CNSA 2.0 specifies ML-DSA-87 for digital signatures, including firmware and software signing.

A migration, therefore, crosses far more than the HSM. TLS and virtual private network (VPN) implementations, software libraries, public key infrastructure (PKI) and certificate issuance, applications, firmware, device identity, and external counterparties can all be involved. NIST’s updated crypto agility guidance (CSWP 39upd1) covers the wider design and operational changes.

The tests differ by workload.

1. **TLS.** The client and server must negotiate a compatible key establishment. If only one side supports the intended PQC or hybrid group, the connection may use a mutually supported classical group when policy allows, or fail if no shared group is permitted. Confirm the negotiated key-exchange group, then measure handshake size and performance.
2. **Signed firmware.** The signing service may produce a new signature, but the field device has to recognize it before accepting the update. That verifier may be embedded in thousands of deployed units. Test trust anchors, signature formats, device limits, and recovery after failed verification.

NIST IR 8547, Section 3.2, also discusses hybrid classical/PQC approaches. A classical/PQC hybrid combines both components in one key-establishment scheme or requires both signatures on the same message; running separate classical and PQC sessions is not, by itself, a hybrid construction. These approaches can suit defined risks and protocols, but add engineering, interoperability, validation, and potentially another transition later. NIST accommodates suitably combined hybrid key establishment and dual signatures in FIPS 140 validation; each implementation still needs the applicable validation.

## Why the migration timeline matters when buying long-lived infrastructure


Consider an industrial controller purchased in 2027 for a 15-year service life. It may still operate in 2042. Its core functions could remain useful even as the algorithms behind device identity, communications, certificate chains, or firmware verification change; if the controller cannot accept those updates, an otherwise functional asset may need to be replaced early.

The critical purchasing question is whether the supplier can demonstrate a supported cryptographic upgrade path, including the code that verifies firmware updates. Firmware-upgradeable does not automatically mean crypto agile. That is why algorithm change belongs in procurement criteria, not only in a later migration project.


 NIST notes that past cryptographic transitions have taken 10 to 20 years to move from published algorithms into deployed products and infrastructure. That is comparable to the controller’s entire 15-year service life.

## Five questions to ask before buying PQC-ready infrastructure

1. Dependencies: Where do the current algorithms, keys, certificates, libraries, protocols, and hardware operate? Which systems, suppliers, and counterparties depend on them?

2. Upgrade path: Which changes are supported through configuration or firmware, and which require application updates, new hardware, or replacement?

3. Workload capacity: Can the proposed system handle the chosen algorithm’s processing cost, key and signature sizes, memory, bandwidth, and latency within the actual workload?

4. Interoperability and recovery: What representative end-to-end tests demonstrate peer compatibility, throughput, failure handling, rollback, and recovery?

5. Update authentication: How will a long-lived device verify its next firmware update if its existing signature-verification method becomes unsuitable?

Question 5 is the easiest to overlook. A device can accept firmware updates and still reject the signed update that would introduce its next verification algorithm. NIST IR 8547, Section 3.1.1, addresses software-signing verification on long-lived devices, and the Internet Engineering Task Force (IETF) Internet-Draft “Adapting Constrained Devices for Post-Quantum Cryptography” (revision -11, October 29, 2026) examines the complications for constrained devices. That draft is a work in progress, not a finalized standard.

Ask for evidence rather than a general “PQC-ready” commitment: exact product and firmware versions, supported signing and verification mechanisms, operational limits, representative end-to-end test results, an upgrade procedure, and a recovery or replacement plan. Tie that evidence to the actual firmware build and workload, rather than a product family.

## From NIST roadmap to a tested migration: where Futurex can help

Treat NIST’s standards direction as a destination, not an installation plan. Start with a defined workload and named technical and business owners. The following five stages are a practical enterprise method, not a NIST-required implementation checklist.

Excrypt HSMs safeguard the keys used in these workflows, and CryptoHub manages keys, certificates, and policy across them. Your team remains responsible for application changes and acceptance testing.


1. Inventory: Identify algorithms, keys, certificates, protocols, libraries, devices, services, and owners within a defined scope; record evidence and any gaps.

2. Prioritize: Rank long-lived confidential data, applicable government or contractual requirements, critical trust chains, supplier readiness, and devices lacking upgrade paths. Select one representative workload before committing to enterprise-wide rollout.

3. Test: Validate target algorithms and complete transactions or signing workflows. Excrypt HSMs support ML-KEM, ML-DSA, and SLH-DSA alongside classical algorithms. CryptoHub provides management and orchestration across supported cryptographic services. Confirm the exact model, firmware, API, parameter sets, performance, and applicable validation status for the proposed deployment.

4. Transition: Stage deployment with defined cutover, fallback, and recovery criteria.

5. Govern: Keep owners, exceptions, operating evidence, and revalidation current.

For a deeper look at these stages, see Futurex’s crypto agility guide. The article, “Five Crypto Agility Design Choices,” covers architectural options for abstracting or managing change.

For a firmware-signing pilot, identify the signing key and service, create a test artifact using the proposed algorithm, and verify it on the target device. Then test failed verification and recovery.

A useful Futurex HSM evaluation defines a test plan with measurable acceptance criteria. Identify the Excrypt operation and CryptoHub service in scope, participating applications and verifiers, and required firmware and API versions. Product documentation establishes supported operations; customer-specific testing demonstrates whether they function within the complete environment. 

See why Futurex was named 2026 PQC Platform of the Year.

## What should your organization do now?

Choose one important workflow, confirm which requirements apply to it, document its public-key dependencies and suppliers, and ask for version-specific evidence of the intended change. Test it end-to-end (whether that is a TLS handshake, certificate chain, signature verification, or device update path), with failure and recovery in the acceptance criteria. Use the results to sequence the next phase.

Bring Futurex one representative workload, its key and certificate architecture, any applicable deadlines, and the systems outside the HSM that must participate. Define the technical evaluation and acceptance criteria before proposing production changes.

 __Discuss your infrastructure's cryptographic migration requirements with Futurex__.
