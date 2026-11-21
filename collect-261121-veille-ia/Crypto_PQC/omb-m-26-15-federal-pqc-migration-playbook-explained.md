---
id: collect-261121-veille-ia/crypto-pqc/omb-m-26-15-federal-pqc-migration-playbook-explained
title: "OMB M-26-15: Federal PQC Migration Playbook Explained"
domain: crypto-pqc
role: reference
task: article
actors: []
dates: []
keywords: ["migration", "systems", "must", "m-26-15", "federal", "days", "agency", "phase"]
source: https://postquantum.com/security-pqc/omb-m-26-15-pqc-migration/
source_anchor: ""
source_lines: [1, 117]
section: "Crypto_PQC"
---
## Table of Contents

**June 25, 2026** — The Office of Management and Budget published Memorandum M-26-15, “Execution of the Migration to Post-Quantum Cryptography,” on June 24, two days after President Trump signed Executive Order 14412 mandating the federal PQC transition. The bottom line: M-26-15 is the operational playbook for civilian agencies, and it arrived in two days despite the EO allowing ninety, which tells you it was written alongside the order, not in response to it.

The memo, signed by OMB Director Russell T. Vought, does three things. It lays out a five-phase migration timeline running from 2026 through 2035, the first time any federal document has published an explicit phased schedule for the full PQC transition. It sets an October 2026 deadline for every agency to submit a PQC migration plan. And it provides the most detailed technical implementation guidance OMB has ever issued on PQC, covering algorithm selection, hybrid architectures, crypto-agility, and zero-trust integration.

M-26-15 does not apply to National Security Systems. Those stay under CNSA 2.0 and the DoW PQC Strategy I analyzed yesterday.

## The News

### What M-26-15 Requires

**Agency migration plans within 120 days.** Every executive department and agency must develop and submit a PQC Migration Plan to OMB and the Office of the National Cyber Director (ONCD) no later than 120 days from June 24, putting the deadline at approximately **October 22, 2026**. Plans must include risk-based prioritization, phased timelines, automated inventory methodologies, a crypto-agility architecture plan, third-party coordination, resource and funding estimates, and governance roles.

**Risk-based prioritization.** Agencies must prioritize migration for high-impact systems (FIPS 199 “high”), High Value Assets (as defined by OMB M-19-03), and any system with highly sensitive data or systems the agency determines are likely to be particularly vulnerable to CRQC-based attacks. The memo also flags systems with asymmetric-encryption-based logical access controls (such as PKI) and systems containing data expected to remain mission-sensitive in 2030.

**The five-phase timeline:**

*Phase 1 (2026–2027): Strategy, Planning, and Discovery.* Inventory HVAs and high-impact systems, define strategy, establish governance, designate accountable officials, assess risk.

*Phase 2 (2027–2028): Pilots and Early Migration.* Execute pilot projects, begin migrating prioritized systems, refine plans based on lessons learned.

*Phase 3 (2028–2030): Prioritized Migration.* Migrate key establishment to PQC for all HVAs, high-impact systems, and systems with highly sensitive data. Ensure all systems are cryptographically agile.

*Phase 4 (2031): Signature Migration.* Migrate digital signatures to PQC for the same prioritized system categories.

*Phase 5 (2035): Full Migration.* Complete migration of remaining systems based on risk assessment and availability of commercial offerings.

**System modernization.** PQC must be a primary security consideration in all resource planning. Agencies must incorporate PQC upgrades into planned cloud migrations, software development lifecycles, and hardware-refresh schedules. Systems incapable of supporting PQC or hybrid cryptography must be identified and given priority for replacement or decommissioning.

**FICAM working group.** Within 60 days (approximately August 23, 2026), GSA must establish an interagency working group on modernizing Federal Identity, Credential, and Access Management to support PQC.

**FedRAMP coordination.** Agencies must engage their FedRAMP-authorized cloud service providers to delineate PQC migration responsibilities within the shared-responsibility model. CISA and the Department of War, in coordination with GSA, will lead PQC migration for FedRAMP-authorized services used at more than one agency.

### Technical Implementation Guidance (Appendix A)

The memo’s Appendix A is the most detailed technical PQC guidance OMB has ever published.

**Algorithm selection.** The memo lists three NIST-standardized PQC algorithms: ML-KEM (FIPS 203) for key encapsulation, ML-DSA (FIPS 204) for digital signatures, and SLH-DSA (FIPS 205) as a hash-based signature fallback. It also notes that future standards, including FN-DSA (FALCON) and HQC, may follow.

**Quantum-vulnerable algorithms.** The memo provides a table of algorithms that must be migrated: ECDH, MQV, ECDSA, Diffie-Hellman, RSA (both signature and key establishment), DSA, and “other non-PQC asymmetric algorithms.” At the bottom of the table: “Symmetric-key-based protocols should also be avoided.”

**Hybrid architectures.** M-26-15 describes hybrid implementation for both key exchange (combining classical and PQC shared secrets through a key derivation function) and digital signatures (dual signatures, both must validate). The memo characterizes hybrid approaches as “an intricate and resource-intensive stopgap,” leaving implementation to agency risk assessments.

**TLS 1.3.** Consistent with EO 14306, agencies must support TLS 1.3 or a successor by January 2, 2030. The memo provides a technical walkthrough of hybrid key exchange in TLS 1.3 (ClientHello with dual key shares, combined key derivation).

**Crypto-agility.** The memo calls for provider-based architectures (citing OpenSSL 3.x and Java JCA/JCE as examples), configuration-driven algorithm selection, cipher-suite negotiation in protocols, and agile key management infrastructure including HSMs capable of generating and managing PQC keys natively.

**Automation.** Manual approaches are “often inadequate.” Agencies should use automated cryptographic inventory tools, software composition analysis, static and dynamic application security testing for cryptographic functions, and network scanners for protocol and cipher-suite detection. The memo explicitly calls for populating a central Cryptographic Bill of Materials (CBOM).

**Zero Trust integration.** PQC is described as “a foundational dependency for advancing a durable zero-trust architecture.” The memo maps PQC requirements across all ZTA pillars: device attestation must use PQC, network infrastructure must support PQC key exchanges, application workloads must issue and validate PQC-signed tokens, and data must be re-encrypted with PQC-protected keys.

## My Analysis

### Two Days, Not Ninety

Section 4(b) of EO 14412 gave OMB 90 days to issue binding migration guidance. OMB delivered M-26-15 in two. Combined with the DoW PQC Strategy, which was cleared for publication in April but released June 23, the pattern is now obvious: the executive orders, the defense strategy, and the OMB memo were drafted as a coordinated package. The EO’s 90-day window was a ceiling, not a schedule. OMB treated the signing as the starting gun for releasing a document it had already written.

This is good execution. It also means that anyone waiting for the “implementation details” before acting has lost that excuse. The details are here. The plan-submission deadline is October. Agencies have 120 days to produce a document that must include phased timelines, automated inventory methodologies, resource estimates, and governance structures. Those are not things you assemble in 120 days if you have not started.

### The Five-Phase Timeline Fills a Gap

Before M-26-15, the federal PQC timeline had hard dates at the top (the EO’s 2030/2031) and an aspirational endpoint at the bottom (NIST IR 8547’s 2035 deprecation). What it did not have was a phased schedule connecting the two. M-26-15 provides that schedule, and three things about it matter.

The Phase 3/Phase 4 split mirrors EO 14412’s function-based split: key establishment by 2030, signatures by 2031. That alignment means agencies can build one plan that serves both the EO’s legal mandate and the memo’s operational guidance.

Phase 5 puts 2035 on the record as the full-migration date for remaining systems. This confirms what I noted in my DoW analysis: the EO’s hard 2030/2031 dates apply to HVAs and high-impact systems, not the entire federal estate. Lower-priority systems get a longer runway. For organizations that were uncertain whether 2030 or 2035 was their planning horizon, M-26-15 gives the answer: it depends on how your systems are classified. If you hold federal HVA data or operate a high-impact system, 2030. If not, 2035 at the latest.

Phase 1 and 2 are where the real test comes. The memo’s phasing assumes agencies will spend 2026–2027 on inventory and planning, then 2027–2028 on pilots. The agencies that completed NSM-10 and M-23-02 inventories are positioned for that timeline. The ones that treated M-23-02 as optional paperwork are now staring at a four-year compression of work that was supposed to stretch to 2035.

### SLH-DSA Is In — And That Creates a Civilian-Defense Split

Appendix A lists three PQC signature algorithms: ML-DSA (FIPS 204) and SLH-DSA (FIPS 205), with FN-DSA (FALCON) as a future addition. CNSA 2.0, which governs the NSS covered by the DoW strategy, deliberately excludes SLH-DSA. The NSA chose to go with ML-DSA-87 for general signing and LMS/XMSS (stateful hash-based schemes) for firmware signing, leaving the stateless alternative out.

The result is a concrete algorithm divergence between the civilian and defense PQC tracks. A federal civilian agency can choose SLH-DSA as a conservative, hash-based fallback that avoids lattice assumptions entirely. A DoW system operating under CNSA 2.0 cannot. For organizations straddling both worlds, as defense contractors routinely do, this means tracking which algorithm set applies to which system.

The divergence is defensible. NSA has reasons for preferring stateful schemes in controlled signing environments, and it has reasons for keeping its algorithm set tight. But the split is now explicit, and it adds another layer to the compliance matrix I described in my DoW strategy analysis.

### “Intricate and Resource-Intensive Stopgap”

That is how M-26-15 describes hybrid cryptography. Most of the global PQC community treats hybrid as a recommended best practice during migration, including BSI and several European regulators. OMB’s framing is cooler: hybrid “can be a useful tool for managing risk during the migration” but “introduces its own risks and complexities.”

The framing matters because it signals that OMB does not expect agencies to stay in hybrid mode permanently. The target is full PQC, with hybrid as a transitional state for systems that cannot switch cleanly. Agencies considering a hybrid-first strategy should note that OMB is already positioning it as temporary, which means the eventual expectation is classical-algorithm removal, not indefinite dual-stack operation.

### “Symmetric-Key-Based Protocols Should Also Be Avoided”

One line at the bottom of the quantum-vulnerable algorithms table reads: “Symmetric-key-based protocols should also be avoided.” This aligns with the DoW strategy’s position that pre-shared-key and symmetric-only key-establishment approaches do not count as quantum-resistant, and with the November 2025 DoW CIO memo’s hard phase-out date for such approaches by the end of 2030.

The reasoning is simple. Symmetric algorithms themselves (Grover’s algorithm aside, with its debatable practical feasibility) are not the concern. The concern is symmetric *key establishment and distribution* without PQC asymmetric protection. If your key agreement relies on pre-shared keys distributed through a channel that itself uses quantum-vulnerable cryptography, the symmetric protection is only as strong as the key-distribution mechanism. M-26-15 and the DoW strategy close this gap by requiring PQC asymmetric key establishment as the baseline.

### Crypto-Agility Gets Operational

Previous federal guidance treated crypto-agility as a principle. M-26-15 treats it as an implementation requirement with named architectural patterns. The guidance on provider-based cryptographic libraries (OpenSSL 3.x providers, Java JCA/JCE), configuration-driven algorithm selection, and agile KMS/HSM is the most operationally specific federal crypto-agility guidance to date.

For organizations building or procuring systems today, this section is the one to hand to your engineering team. The memo is saying, in effect, that a system hardcoded to ML-KEM-768 is not compliant even if ML-KEM-768 is the right algorithm today. The system must be able to swap algorithms without a re-architecture. That is a design constraint, not a deployment checkbox, and it needs to be in procurement specifications now, not retrofitted in 2029.

### ZTA as a PQC Dependency

The memo’s Section 7 positions PQC as foundational to zero trust, mapping PQC requirements across devices (TPM attestation), networks (TLS/IPsec termination), applications (PQC-signed tokens), and data (re-encryption with PQC-protected keys). The framing is notable: PQC is not an addition to ZTA but a prerequisite for it. A zero-trust architecture built on quantum-vulnerable cryptography is, by the memo’s logic, structurally compromised.

This has procurement implications. Federal ZTA investments that do not account for PQC key management and PQC-signed authentication are building on a foundation that will need to be replaced within the migration window. Agencies and their vendors should be evaluating ZTA products against PQC readiness now, before the 2030 deadline locks in architectures that cannot be upgraded.

### What M-26-15 Replaces

The memo does not explicitly rescind OMB M-23-02, the November 2022 Biden-era guidance that first directed agencies to inventory their quantum-vulnerable cryptography. But M-26-15 functionally supersedes it. Both fulfill OMB’s obligation under the Quantum Computing Cybersecurity Preparedness Act. M-26-15 establishes new plan-submission requirements, a new phased timeline, and new technical guidance that go well beyond M-23-02’s inventory-and-report framework.

The practical consequence: agencies that were tracking their PQC obligations against M-23-02’s annual inventory requirements now need to rebase against M-26-15’s 120-day plan deadline and five-phase migration schedule. The inventory work from M-23-02 feeds directly into M-26-15’s Phase 1, but the output expectation has shifted from “prioritized inventory” to “actionable migration plan.”

On the NSS side, the governance picture changed too. NSPM-12, signed June 12, overhauled NSS cybersecurity governance by re-establishing the Committee on National Security Systems (CNSS) with binding directive authority and designating the NSA Director as National Manager. That is why M-26-15 can cleanly exclude NSS: NSPM-12 already placed those systems under a separate authority chain where the CNSS and NSA enforce CNSA 2.0. M-26-15 for civilian systems and the DoW PQC Strategy for defense systems are parallel implementation documents, each operating under its own governance layer. NSPM-12 is the reason the NSS carve-out in EO 14412 is a jurisdictional boundary, not a gap.

### What Is Still Missing

M-26-15 does not define what “transition” means in operational terms. Does a system count as migrated when it supports PQC, when it prefers PQC, or when classical algorithms are disabled entirely? Cloudflare raised this question in its same-day analysis of EO 14412, and M-26-15 does not answer it. A system that supports ML-KEM but still allows a classical-only TLS handshake is vulnerable to downgrade attacks. The distinction between “support” and “exclusive use” is the gap between a capability milestone and an actual security improvement. Agencies will need NIST or CISA to close this definitively.

The memo also does not address the CMVP bottleneck. EO 14412 orders NIST to accelerate the Cryptographic Module Validation Program, and I have written about why that pipeline does not fit a 2030 deadline. M-26-15 tells agencies to use NIST PQC algorithms but does not address the reality that validated modules for those algorithms may not be available on the timeline the plan requires.

Finally, the memo is silent what a CBOM must contain. EO 14412 directs CISA and NIST to define minimum CBOM elements within 270 days. M-26-15’s automation section tells agencies to build a central CBOM, but it cannot reference guidance that does not yet exist. Agencies building their automated inventory tooling now will need to rebuild it when the CBOM minimum-elements guidance arrives, unless they anticipate its likely shape. I have argued for years that a CBOM is the prerequisite for any auditable migration program. The fact that the plan-submission deadline (October 2026) precedes the deadline for the CBOM minimum-elements guidance (March 2027) means agencies are being asked to build migration plans before the inventory taxonomy is finalized. That sequencing gap will cost rework.
