---
id: collect-261121-veille-ia/crypto-pqc/us-federal-pqc-mandate-after-june-2026-complete-guide
title: "US Federal PQC Mandate After June 2026: Complete Guide"
domain: crypto-pqc
role: reference
task: article
actors: []
dates: ["2026-10-22"]
keywords: ["june", "federal", "systems", "migration", "complete", "framework", "mandate", "governance"]
source: https://postquantum.com/post-quantum/us-federal-pqc-mandate-2026/
source_anchor: ""
source_lines: [1, 193]
section: "Crypto_PQC"
---
## Table of Contents

For four years, the U.S. federal PQC framework told agencies to prepare but never told them when to finish. The infrastructure was real: NIST published three PQC standards in August 2024, the Quantum Computing Cybersecurity Preparedness Act created a statutory obligation, NSM-10 set a 2035 risk-mitigation goal, and OMB M-23-02 imposed planning deadlines for inventories, migration leads, and annual reporting. What the framework lacked was a dated, enforceable mandate telling agencies when migration itself had to be complete.

June 2026 filled that gap. In the space of three weeks, the administration published five policy documents that, taken together, create the first complete federal PQC migration mandate. Every federal high-value asset and high-impact system, civilian and defense alike, now has a hard deadline under EO 14412 (with NSS routed through CNSA 2.0 instead). The DoW strategy goes further, extending department-wide migration gates to all DoW systems, not only the high-impact ones the EO reaches. The defense industrial base faces overlapping obligations from multiple directions. The remaining civilian estate has a phased schedule running to 2035 under OMB M-26-15. And the governance framework for National Security Systems was overhauled for the first time in 35 years to give the enforcement mechanism teeth.

This article is the complete reference. I cover the individual documents separately; this piece puts them all in one frame. It replaces my earlier US regulatory framework analysis, which predated the June rollout and described an enforcement picture that no longer applies.

## The Architecture: Three Layers Built in Sequence

The June rollout was not a burst of disconnected announcements. It was a three-layer policy architecture assembled in sequence: governance authority first, mandates second, implementation guidance third. Understanding the layers is the key to understanding why the NSS carve-out in the executive order is a jurisdictional boundary, not a gap, and why the implementation guidance arrived in two days instead of ninety.

**Layer 1 — Governance (June 12).** National Security Presidential Memorandum 12 (NSPM-12) overhauled the cybersecurity governance framework for National Security Systems. It rescinded the 1990 NSD-42 and Biden’s 2022 NSM-8, re-established the Committee on National Security Systems (CNSS) with the power to issue binding security directives to all NSS owners and operators across the federal government, and formally designated the NSA Director as National Manager for NSS. The White House has published the memorandum’s text, which confirms these provisions and states that CNSS directives and complementary standards apply to all NSS owners and operators. NSPM-12 is not about PQC specifically; it is the governance layer that makes CNSA 2.0 enforceable across every agency that operates NSS, including civilian agencies, rather than only the Department of War.

**Layer 2 — Mandates (June 22–23).** Executive Order 14412 (“Securing the Nation Against Advanced Cryptographic Attacks”) set hard deadlines for civilian high-value assets and high-impact systems, with NSS explicitly carved out. Executive Order 14413 (“Ushering in the Next Frontier of Quantum Innovation”) established the offense-side complement. The Department of War PQC Strategy, released June 23, set department-wide 2030/2031 migration gates for all DoW systems and operationalized CNSA 2.0 for NSS. The EO’s NSS carve-out works because NSPM-12, ten days earlier, had already given the CNSS and NSA the authority to govern those systems.

**Layer 3 — Implementation (June 24).** OMB Memorandum M-26-15 (“Execution of the Migration to Post-Quantum Cryptography”) landed two days after EO 14412, despite the order allowing ninety days for OMB to issue guidance. The speed tells you it was written alongside the order, not in response to it. M-26-15 provides the five-phase timeline, the plan-submission requirements, and the most detailed technical implementation guidance OMB has ever published on PQC. It covers the civilian non-NSS space. The DoW strategy itself contains the defense-side equivalent: the five Lines of Effort and two execution tracks.

The result: governance authority (NSPM-12) -> mandates split by civilian and defense (EO 14412 + DoW strategy) -> implementation guidance (M-26-15 for civilian, DoW LOEs for defense) -> offense investment (EO 14413). Each layer depends on the one beneath it.

## The Legal Foundation That Survived the Transition

Three durable authorities underpin the PQC mandate regardless of which administration holds office: one statute, one still-operative presidential memorandum, and the finalized NIST cryptographic standards. These were in place before June 2026 and remain in force after it.

**The Quantum Computing Cybersecurity Preparedness Act** (Public Law 117-260, December 2022) requires OMB to direct agencies to prioritize migration and develop plans. Both M-23-02 and now M-26-15 cite it as their statutory authority. It also mandates annual congressional reporting on migration progress through at least 2029 and exempts NSS. This law cannot be revoked by executive order.

**National Security Memorandum 10 (NSM-10)** (May 2022) established the planning framework: agency cryptographic inventories, risk assessments, and a goal of mitigating as much quantum risk as feasible by 2035. NSM-10 remains in force. EO 14412 builds on it by converting its planning goals into enforceable deadlines for the highest-priority systems.

**NIST’s finalized FIPS standards** (August 2024): FIPS 203 (ML-KEM), FIPS 204 (ML-DSA), and FIPS 205 (SLH-DSA). These are the algorithms that EO 14412, M-26-15, and the DoW strategy all point to. FN-DSA (FALCON) and HQC are expected as future additions. NIST IR 8547, final since October 2026, deprecates quantum-vulnerable public-key cryptography after 2030 and disallows it after 2035, while continuing to approve symmetric primitives with sufficient security strength. EO 14412 effectively treats the 2030 date as a compliance deadline for priority systems, and M-26-15 explicitly directs agencies to align their plans with IR 8547.

## The Civilian Mandate: EO 14412 and OMB M-26-15

Executive Order 14412 sets the top-level deadlines for civilian federal systems. High-value assets (HVAs) and high-impact systems (FIPS 199 “high”) must transition to PQC for key establishment by **December 31, 2030**, and for digital signatures by **December 31, 2031**. The one-year split is deliberate: key establishment goes first because Harvest Now, Decrypt Later (HNDL) is an active threat against data in transit, and because swapping ML-KEM into a key exchange is operationally lighter than rebuilding a certificate hierarchy. Signatures get the extra year because ML-DSA migration touches PKI, cross-certification, and the full certificate lifecycle. I have argued for this sequencing for years through the Applied Quantum PQC Migration Framework, which separates key-establishment and digital-signature tracks precisely because they have different risk profiles and optimal ordering.

The EO describes the Harvest Now, Decrypt Later threat in all but name, stating that adversaries can collect encrypted information today and decrypt it later once quantum capabilities mature. It also carves out National Security Systems, requiring NSA, as National Manager, to report through the CNSS on NSS PQC migration status. The cryptographic standard for those systems comes through CNSSP 15 and CNSA 2.0, now backed by the governance structure restored in NSPM-12.

OMB M-26-15 translates the EO’s deadlines into an operational plan. It establishes a **five-phase migration timeline**:

1. 
*Phase 1 (2026–2027):* Strategy, planning, discovery. Inventory HVAs and high-impact systems, establish governance, designate accountable officials.
2. 
*Phase 2 (2027–2028):* Pilots and early migration. Execute pilot projects, begin migrating prioritized systems, refine plans.
3. 
*Phase 3 (2028–2030):* Prioritized migration. Complete key-establishment migration for all HVAs, high-impact systems, and systems with highly sensitive data.
4. 
*Phase 4 (2031):* Signature migration. Complete digital-signature migration for the same prioritized categories.
5. 
*Phase 5 (2035):* Full migration. Complete migration of remaining systems based on risk assessment and availability of commercial offerings.

Phase 5 is the first official federal document to explicitly reconcile the EO’s hard 2030/2031 dates with the broader 2035 horizon from NIST IR 8547. For organizations that were uncertain whether 2030 or 2035 was their planning target, M-26-15 gives the answer: it depends on how your systems are classified. HVAs and high-impact systems face 2030/2031. Remaining non-NSS civilian systems get 2035 at the latest.

M-26-15 requires every agency to submit a PQC Migration Plan to OMB and ONCD within **120 days** (approximately **October 22, 2026**). Plans must include risk-based prioritization, phased timelines, automated inventory methodologies, a crypto-agility architecture plan, third-party coordination, resource estimates, and governance roles.

The memo’s technical appendix is the most detailed PQC implementation guidance OMB has ever published. It lists all three NIST-standardized algorithms (ML-KEM, ML-DSA, and SLH-DSA), provides a table of quantum-vulnerable algorithms to migrate away from, describes hybrid architectures as “an intricate and resource-intensive stopgap,” specifies crypto-agility requirements (provider-based libraries, configuration-driven algorithm selection, agile KMS/HSM), and positions PQC as a foundational dependency for zero-trust architecture across all pillars. It also directs agencies to build automated Cryptographic Bill of Materials (CBOM) inventories, though the CBOM minimum-elements guidance (directed by EO 14412 within 270 days) is not due until March 19, 2027.

M-26-15 functionally supersedes OMB M-23-02 (November 2022), the Biden-era guidance that first directed agencies to inventory their quantum-vulnerable cryptography. The inventory work from M-23-02 feeds directly into M-26-15’s Phase 1, but the output expectation has shifted from “prioritized inventory” to “actionable migration plan.”

## The Defense Mandate: DoW PQC Strategy

The Department of War PQC Strategy, released June 23, covers the systems EO 14412 deliberately does not touch. It splits by migration stage rather than cryptographic function: all DoW systems must **support** PQC or be phased out by **December 31, 2030**, and **use** PQC by **December 31, 2031**, “unless otherwise noted.”

That qualifier matters. “Unless otherwise noted” defers the hardest system categories to CNSA 2.0’s product-specific schedule, which runs to 2033 for web and cloud services, operating systems, large PKI, and niche or constrained devices.

The strategy organizes work into five Lines of Effort (governance, inventory and planning, development and analysis, commercial integration, and device deployment) across two execution tracks. The **High Assurance ECU track** covers NSA-certified encrypted devices dependent on Key Management Infrastructure (KMI) and the Cryptographic Modernization 2 (CM2) program. The **Commercial Solutions track** covers commodity IT and cloud, including products validated through the CSfC program.

Several provisions stand out in my detailed analysis. Authentication is not optional: a solution that migrates only confidentiality, with no PQC authentication, will not count as PQC at all. This validates the Trust Now, Forge Later (TNFL) thesis I have been arguing for years. The strategy rejects QKD, quantum networking, non-local quantum randomness, and most pre-shared-key approaches that lack PQC asymmetric key establishment as routes to quantum resistance, while preserving narrow legacy exceptions for certain pre-2010 symmetric key-distribution use cases. It defines “done” as deprecation across the entire data pathway and lifecycle, which means removing vulnerable algorithms, not merely deploying new ones alongside old ones.

The strategy builds on a November 2025 DoW CIO memorandum that already required component-level cryptographic inventories, named migration leads, and pre-deployment approval for PQC technologies, with a hard phase-out date for insecure pre-shared-key and symmetric key-establishment approaches by end of 2030.

## The NSS Framework: CNSA 2.0 and NSPM-12

National Security Systems follow the NSA’s Commercial National Security Algorithm Suite 2.0 (CNSA 2.0), which is more granular than either the EO or the DoW strategy. CNSA 2.0 schedules its transition by product category:

Software and firmware signing and traditional networking equipment reach exclusive CNSA 2.0 use by **2030**. Web and cloud services, operating systems, large PKI, and niche or constrained devices reach exclusive use by **2033**.

The algorithm suite is ML-KEM-1024, ML-DSA-87, LMS/XMSS (for firmware signing), AES-256, and SHA-384/512. CNSA 2.0 deliberately excludes SLH-DSA (SPHINCS+), creating a concrete divergence from the civilian track: OMB M-26-15 includes SLH-DSA as an approved algorithm, CNSA 2.0 does not. A federal civilian agency can choose SLH-DSA as a hash-based fallback that avoids lattice assumptions entirely. An NSS operating under CNSA 2.0 cannot.

NSPM-12 (June 12, 2026) is the governance layer that makes these requirements enforceable. Before NSPM-12, NSS cybersecurity governance was fragmented across a 1990 directive and various agency-level authorities. NSPM-12 re-establishes the CNSS with binding directive authority and positions NSA as the National Manager and cryptographic authority for all NSS. The CNSS can now issue binding security directives to every federal agency that operates NSS. This is why CNSA 2.0 requirements apply government-wide, across every agency that operates NSS, rather than within the Department of War alone.

## The Obligation Matrix

The documents carve the federal PQC obligation along three axes: system classification, migration function, and organizational ownership. Here is how the deadlines map.

### Federal Civilian HVAs and High-Impact Systems (Non-NSS)

**Authority:** EO 14412 + OMB M-26-15

Key establishment (ML-KEM): **December 31, 2030** Digital signatures (ML-DSA / SLH-DSA): **December 31, 2031** Agency migration plans due: **~October 22, 2026** (120 days from M-26-15) Algorithms: ML-KEM (FIPS 203), ML-DSA (FIPS 204), SLH-DSA (FIPS 205)

### Remaining Federal Civilian Systems

**Authority:** OMB M-26-15 Phase 5, aligned with NIST IR 8547

Full migration: **2035** Risk-based prioritization; agencies include these in their phased plans.

### Department of War Systems (All)

**Authority:** DoW PQC Strategy + DoW CIO Memo (November 2025)

Support PQC or phase out: **December 31, 2030** Use PQC: **December 31, 2031** (“unless otherwise noted”) NSS: Must support CNSA 2.0 Two tracks: High Assurance ECU (NSA-certified, KMI-dependent) and Commercial Solutions (NIST PQC via CSfC)

### National Security Systems Under CNSA 2.0

**Authority:** NSA CNSA 2.0, enforced through CNSS binding directives under NSPM-12

Signing and networking: exclusive use by **2030** Web/cloud, OS, large PKI, niche/constrained: exclusive use by **2033** Algorithms: ML-KEM-1024, ML-DSA-87, LMS/XMSS (firmware signing), AES-256, SHA-384/512 SLH-DSA excluded.

### Federal Contractors

**Authority:** EO 14412, Section 6(c)

The EO directs the FAR Council to publish a proposed rule within 180 days requiring covered contractors to comply with NIST FIPS incorporating PQC algorithms by **December 31, 2030**. Once finalized, this extends the federal PQC mandate into the private sector through procurement. The DoW strategy compounds this with directives to update CMMC to include PQC requirements, and the November 2025 DoW CIO memo already requires DoW Components to identify, inventory and report the cryptography used in all DoW information systems.

The procurement cascade is where the federal PQC mandate reaches furthest into the private sector. A prime contractor that needs PQC-validated products by 2030 will impose that requirement on its subcontractors, who will impose it on their component suppliers. The obligation propagates down every tier of the supply chain until it reaches companies that may never interact with a federal agency directly. This is the pattern that made FedRAMP the de facto cloud security standard and that is making CMMC the baseline for defense supply-chain cybersecurity. For many organizations, particularly in the technology and telecommunications sectors, the FAR rule will be more operationally motivating than any regulation, because it is enforced at the point of sale: no compliance, no contract. The result is that a rule nominally aimed at “covered federal contractors” will, in practice, set PQC expectations across a significant portion of the U.S. technology economy by the end of this decade.

### Critical Infrastructure

**Authority:** EO 14412, Section 5

EO 14412 does not impose a direct migration deadline on critical infrastructure owners and operators. It directs Sector Risk Management Agencies to work with CISA to assist them in developing PQC migration plans. The CBOM guidance (due ~March 2027) will provide the taxonomy for declaring cryptographic posture. Sectoral regulation, procurement requirements, or insurance market pressure could create their own deadlines independently.

## The Contractor Dimension

For the defense industrial base, the compliance picture is more complex than for any other class of organization. A defense prime now faces obligations from four directions at once, none of them congruent.

From EO 14412: the FAR rule, once finalized, would require covered federal contractors to comply with NIST FIPS incorporating PQC by **2030**. This is a flat date, not split by function or stage.

From the DoW strategy: support PQC by **2030**, use PQC by **2031**, with NSS work following CNSA 2.0’s category-specific schedule running to **2033**. The strategy also directs CMMC updates to include PQC requirements.

From the November 2025 DoW CIO memo: inventory, named migration leads, pre-deployment approval, and hard phase-out of insecure PSK and symmetric key-establishment approaches by end of **2030**.

From EO 14412, Section 5(d): the CBOM guidance (due ~March 2027) will create a standard taxonomy for declaring cryptographic posture. This is the one piece of good news for contractors navigating multiple regimes, because it gives them a common format to demand structured cryptographic data from their own suppliers.

I have argued for years that you cannot migrate what you cannot inventory. Common minimum elements for a CBOM, which CISA and NIST must publish as guidance, are the prerequisite for any of this to be auditable. Three overlapping compliance regimes cannot be tracked in a spreadsheet.

## The Shared Bottleneck

Both the civilian and defense tracks have a validation chokepoint. On the High Assurance side, NSA certification gates every ECU. On the commercial side, the Cryptographic Module Validation Program (CMVP) gates every product that needs FIPS 140-3 validation. FIPS 140-3 validation currently runs around 18 months or longer. EO 14412 orders NIST to accelerate the CMVP process, and the DoW strategy calls for streamlined NSA certification. Both documents acknowledge the problem. Neither has widened the pipeline yet. An order to “accelerate” a certification queue is a statement of intent, and intent has never validated a cryptographic module. This is the shared weak point of the entire federal effort.

## Key Dates Ahead

- 
**~August 23, 2026** — GSA FICAM PQC working group established (60 days from M-26-15).
- 
**~October 22, 2026** — Agency PQC migration plans due to OMB and ONCD (120 days from M-26-15).
- 
**~December 19, 2026** — FAR Council proposed rule on contractor PQC compliance due (180 days from EO 14412). NIST initiates CMVP revision and PQC pilot project.
- 
**~March 19, 2027** — CISA and NIST CBOM minimum-elements guidance due (270 days from EO 14412). FAR proposed rule on contractor vulnerability disclosure programs due.
- 
**December 31, 2027** — NIST PQC pilot project completion deadline.
- 
**January 2, 2030** — Federal TLS 1.3 support deadline (per EO 14306, referenced in M-26-15).
- 
**December 31, 2030** — Key-establishment migration for civilian HVAs/high-impact systems (EO 14412). DoW support-PQC-or-phase-out gate. FAR contractor compliance date. CNSA 2.0 exclusive use for signing and networking.
- 
**December 31, 2031** — Digital-signature migration for civilian HVAs/high-impact systems (EO 14412). DoW use-PQC gate.
- 
**2033** — CNSA 2.0 exclusive-use deadline for web/cloud, OS, large PKI, and niche/constrained NSS devices.
- 
**2035** — Full migration of remaining federal civilian systems (M-26-15 Phase 5).

## What Cuts Across All the Documents

Several themes appear across the entire June policy rollout.

**HNDL is the threat that drives sequencing.** EO 14412 describes the Harvest Now, Decrypt Later threat in substance, stating that adversaries are collecting encrypted data for future decryption. The DoW strategy calls insecure communications existential. M-26-15 priorities include data expected to “remain mission-sensitive in 2030.” Key establishment goes first everywhere because the harvesting is happening now.

**Authentication cannot be deferred.** Both the EO and the DoW strategy treat digital-signature migration as mandatory, with its own dated deadline. The DoW strategy goes further: confidentiality-only migration will not count as PQC. This validates the Trust Now, Forge Later (TNFL) thesis at federal policy level, on both the civilian and defense sides, in the same week.

**Crypto-agility is now federal policy.** M-26-15 specifies provider-based architectures, configuration-driven algorithm selection, cipher-suite negotiation, and agile KMS/HSM requirements. The message: PQC is the current answer, not the permanent one. Systems must be built to swap algorithms without re-architecture.

**QKD and false substitutes are rejected.** The DoW strategy refuses QKD, quantum networking, non-local quantum randomness, and PSK approaches lacking PQC asymmetric key establishment. M-26-15 lists only NIST-standardized algorithms. No alternative routes.

**The physics timeline is irrelevant to the policy timeline.** None of the documents estimate when a cryptographically relevant quantum computer (CRQC) will exist. They treat the CRQC as a planning assumption and set deadlines based on migration complexity, regulatory alignment, and the Harvest Now, Decrypt Later threat. As I have been writing for years, the ecosystem, not the physics, now sets the clock.

## What to Do Now

For any organization that sells to, contracts with, or operates systems on behalf of the U.S. federal government, the effective deadline moved to 2030 in June 2026. The global picture reinforces this: the EU, Australia, and India are converging around the same window. The jurisdictions that matter most cluster at the end of this decade.

The actions are the same ones I have been recommending through the Applied Quantum PQC Migration Framework and in Quantum Ready, now with less margin for delay.

**Complete a cryptographic inventory.** M-26-15 directs agencies to use automated tools: software composition analysis, SAST/DAST for cryptographic functions, network scanners for protocol detection. Build toward a central CBOM, even before CISA and NIST publish their CBOM minimum-elements guidance in early 2027.

**Identify your binding deadline.** The date depends on your systems. HVA or high-impact system? 2030 for key establishment, 2031 for signatures. DoW system? Support by 2030, use by 2031. NSS under CNSA 2.0? Category-dependent, up to 2033. Federal contractor? The FAR rule targets 2030. Non-priority civilian? 2035. For organizations subject to multiple jurisdictions, the earliest binding deadline in any jurisdiction you operate in is your actual deadline, not the most permissive one.

**Build for crypto-agility.** M-26-15 is clear that a system hardcoded to a specific algorithm is not compliant even if that algorithm is correct today. The system must be able to swap algorithms without re-architecture. Get this into procurement specifications now.

**Force your vendors on PQC roadmaps.** The FAR rule, once finalized, is expected to cascade PQC obligations down every tier of the federal supply chain. Even organizations with no direct federal contracts may find themselves subject to PQC requirements if they sell to a company that sells to a company that holds a federal contract. This is procurement-driven compliance, and it reaches further and faster than regulation. Companies that cannot demonstrate PQC compliance by 2030 risk losing access to supply chains that ultimately serve the largest technology buyer in the world.

**Stop debating when Q-Day arrives.** The policy calendar has overtaken the physics timeline. The deadlines are set. The phasing is published. The first plan submissions are due in October. Organizations that started their migration program this year are executing a plan. Those that have not started are four months from the first plan-submission deadline and four years from the first hard migration date.

Use the PQC Migration Framework as your methodology, the PQC Readiness Self-Assessment Scorecard as your diagnostic, and the global PQC migration timeline to track how your deadlines compare across jurisdictions.
