---
id: collect-261121-veille-ia/crypto-pqc/nist-ir-8547-cnsa-2-0-pqc-migration
title: "NIST IR 8547, CNSA 2.0 & PQC Migration"
domain: crypto-pqc
role: reference
task: article
actors: []
dates: []
keywords: ["cnsa", "migration", "nist", "systems", "most", "must", "cryptographic", "fips"]
source: https://o3.security/compliance/post-quantum-cryptography-cnsa-2-migration
source_anchor: ""
source_lines: [1, 121]
section: "Crypto_PQC"
---
# Post-quantum cryptography migration:

NIST IR 8547, EO 14412, CNSA 2.0 and the FIPS 203/204/205 standards explained.

Published Last reviewed

Four sets of US federal documents now drive the move off quantum-vulnerable cryptography. NIST finalized the underlying algorithm standards on Aug 13, 2024: FIPS 203 (ML-KEM), FIPS 204 (ML-DSA) and FIPS 205 (SLH-DSA). The NSA's Commercial National Security Algorithm Suite 2.0 (CNSA 2.0, Sept 2022) names the approved algorithms for National Security Systems. Executive Order 14412 and OMB Memorandum M-26-15 then set the binding deadlines: the government's most sensitive systems must migrate key establishment by Dec 31, 2030 and digital signatures by Dec 31, 2031. NIST IR 8547 (final, Oct 27, 2026) is the technical blueprint for meeting them, covering which algorithms are being deprecated, hybrid classical/PQC transition techniques, and a plan for every cryptographic technology layer from protocols to hardware.

This page explains what each document requires, the migration deadlines that matter (key establishment by 2030, signatures by 2031, software signing exclusively CNSA 2.0 by 2030, most remaining NSS categories by 2033, all NSS quantum-resistant by 2035), and how to build the cryptographic inventory those mandates depend on.

## A "harvest now, decrypt later" threat with hard federal deadlines already on the clock.

A cryptographically relevant quantum computer would break RSA, Diffie-Hellman and elliptic-curve cryptography via Shor's algorithm. Adversaries do not have to wait for that machine. NIST IR 8547 frames the urgency formally through Mosca's theorem: if data must stay secure for X years and the migration itself takes Y years, organizations must start before X + Y exceeds the time until a cryptanalytically relevant quantum computer exists. Encrypted traffic captured today can be stored and decrypted later ("harvest now, decrypt later"), which is why migration timelines start now rather than when quantum hardware arrives.

Executive Order 14412 and OMB Memorandum M-26-15 set the binding dates: a risk-prioritized migration mitigating as much quantum risk as feasible by the end of 2030, with the government's most sensitive systems required to adopt PQC for key establishment by December 31, 2030 and for digital signatures by December 31, 2031. NIST IR 8547 (final, October 2026) calls these deadlines tight against history — past cryptographic transitions have taken 10 to 20 years — and says organizations should treat PQC migration as a current operational priority, starting with cryptographic inventories, migration planning and early deployments.

CNSA 2.0 sets the destination for National Security Systems: ML-KEM-1024 for key establishment, ML-DSA-87 for general signatures, and the stateful hash-based schemes LMS and XMSS for software and firmware signing (per NIST SP 800-208), alongside AES-256 and SHA-384 or SHA-512. The NSA wants software and firmware signing on CNSA 2.0 exclusively by 2030, new NSS acquisitions CNSA 2.0 compliant from January 1, 2027, equipment that cannot support CNSA 2.0 phased out by December 31, 2030 (CNSSP 15), most remaining categories exclusively CNSA 2.0 by 2033, and all NSS quantum-resistant by 2035. NIST IR 8547 adds two algorithms beyond CNSA 2.0's current picks: HQC as a backup key-encapsulation mechanism to ML-KEM, and a FALCON-derived signature scheme in development as an alternative to ML-DSA.

Earlier OMB guidance (M-23-02, which M-26-15 now builds on) made the first step concrete for civilian agencies: you cannot migrate what you cannot see. Agencies must inventory active cryptographic systems, prioritize High Value Assets and systems holding data that must stay secure past 2030, and estimate migration cost. That inventory is the foundation of every later step, and it is exactly where most organizations have the least visibility.

## Find it before attackers do. Catch it if they try. Fix it fast.

Across these requirements, O3 runs one continuous loop on your own software so issues surface, get caught, and get fixed before they become an incident.

### Test like an AI-assisted attacker

An agentic pentest probes your app across 50+ vulnerability classes the way a real attacker chains them, so exploitable flaws surface in your build before someone outside finds them.

### Detect and block exploitation at runtime

A kernel-level eBPF agent watches process trees and syscalls, recognises an attack sequence as it unfolds, and restricts the offending process on the spot rather than taking down the host.

### Auto-patch what is reachable

Reachability ranks what genuinely matters, then autofix opens a pull request with the change, so remediation starts inside tight fix windows instead of sitting in a queue.

## How to build a cryptographic inventory for EO 14412 and OMB M-26-15

Both the original OMB M-23-02 and its successor M-26-15 start from one deliverable: a prioritized inventory of cryptographic systems, with High Value Assets, high-impact systems, and anything holding data that must stay secure past 2030 listed first. NIST IR 8547 names cryptographic inventory, migration planning, and early PQC deployment as the three things agencies should be doing right now, not after a cryptanalytically relevant quantum computer exists.

O3 generates a CBOM/QBOM in CycloneDX directly from source and dependencies, inventorying cryptographic assets and the libraries that implement them, which feeds the EO 14412 / M-26-15 inventory and CNSA 2.0 planning. SCA and SBOM surface the crypto-library dependencies behind each finding.

## How to map your stack to CNSA 2.0 approved algorithms

CNSA 2.0 specifies the algorithms that replace today's public-key cryptography for National Security Systems. Knowing the targets lets you plan substitutions rather than discovering them mid-migration.

## How to sequence migration against the EO 14412 and CNSA 2.0 timelines

The deadlines are staggered, so sequencing matters. Key establishment leads under EO 14412 (2030), signatures follow a year later (2031), software/firmware signing and the phase-out of non-CNSA 2.0 NSS equipment track CNSA 2.0's own 2030 dates, and new acquisitions must support CNSA 2.0 well before exclusivity dates.

A continuously updated QBOM lets you watch quantum-vulnerable usage trend toward zero across the 2030 and 2031 milestones instead of relying on point-in-time audits.

## How to use hybrid classical/PQC cryptography during the transition

NIST IR 8547 dedicates a section to PQC-classical hybrid protocols: pairing a quantum-vulnerable algorithm with a post-quantum one so a system stays protected even if a flaw is later found in the PQC side, while gaining quantum resistance before every downstream component has caught up.

O3's CBOM surfaces which protocols, libraries and certificates in your stack already support hybrid or PQC modes versus which are still classical-only, so hybrid rollout can be sequenced against real inventory rather than vendor claims.

## How to design for crypto agility so the next transition is cheaper

Hard-coded algorithms are why this migration is painful. Crypto agility means cryptographic choices are configurable and discoverable, so future changes (including algorithm deprecations) do not require rewrites.

O3's source-level CBOM identifies where cryptographic primitives are called directly in code, helping locate the hard-coded usage that blocks crypto agility.

## How to assess vendors and dependencies for PQC readiness

Much of your cryptography ships inside third-party libraries, products and services. NIST IR 8547 is explicit that software cryptographic libraries, PKI infrastructure and cryptographic hardware all need their own vendor-driven updates before applications can migrate — EO 14412 and CNSA 2.0 only succeed if the supply chain moves too, which makes vendor visibility part of the inventory.

SCA and SBOM enumerate the open-source crypto libraries you depend on, and reading cosign/SLSA provenance supports assessment of the software signing chain that CNSA 2.0 targets first.

## How to report quantum-vulnerable cryptography under EO 14412

Crypto inventory and migration planning is not only an internal exercise under EO 14412 and OMB M-26-15, which requires agencies to submit PQC migration plans to OMB and ONCD. Earlier M-23-02 established the practice of reporting quantum-vulnerable cryptography to ONCD and CISA and submitting prioritized inventories, and NIST IR 8547 builds the technical framework agencies report against.

A CycloneDX CBOM/QBOM is exportable, machine-readable evidence of cryptographic assets and quantum-vulnerable usage, suitable for repeated reporting rather than manual spreadsheets.

## Every mandate and algorithm across NIST IR 8547, EO 14412, CNSA 2.0 and the NIST FIPS standards.

All 26 requirement areas, each with the reference and a vendor-neutral note on how teams meet it.

| Every mandate and algorithm across NIST IR 8547, EO 14412, CNSA 2.0 and the NIST FIPS standards. — 26 requirement areas, each with its reference and how teams meet it. |  |  | 
|---|---|---|
| Requirement area | Ref | How teams meet it | 
|---|---|---|
| ML-KEM-1024 for key establishment | FIPS 203 / CNSA 2.0 | Replace RSA and ECC key exchange | 
| ML-DSA-87 for general signatures | FIPS 204 / CNSA 2.0 | Replace RSA and ECDSA signatures | 
| SLH-DSA stateless hash-based signatures | FIPS 205 | Use where stateful keys are impractical | 
| LMS / XMSS for software & firmware signing | CNSA 2.0 / SP 800-208 | Adopt stateful hash-based signing | 
| HQC as backup key-encapsulation mechanism | NIST IR 8547 / IR 8545 | Track as ML-KEM alternative, FIPS pending | 
| FALCON-derived signature scheme | NIST IR 8547 | Track as ML-DSA alternative, FIPS in development | 
| AES-256 symmetric encryption | CNSA 2.0 | Confirm symmetric baseline | 
| SHA-384 or SHA-512 hashing | CNSA 2.0 | Confirm hash baseline | 
| Key establishment migrated for HVA/high-impact systems | EO 14412 / OMB M-26-15 | Complete by Dec 31, 2030 | 
| Digital signatures migrated for HVA/high-impact systems | EO 14412 / OMB M-26-15 | Complete by Dec 31, 2031 | 
| Software & firmware signing exclusively CNSA 2.0 | CNSA 2.0 timeline | Complete by 2030 | 
| New NSS acquisitions support CNSA 2.0 | CNSA 2.0 timeline | Procurement requirement by Jan 1, 2027 | 
| Phase out NSS equipment lacking CNSA 2.0 | CNSSP 15 / CNSA 2.0 FAQ | Complete by Dec 31, 2030 | 
| CNSA 2.0 exclusivity for browsers, cloud, OS | CNSA 2.0 timeline | Complete by 2033 | 
| Inventory active cryptographic systems | NIST IR 8547 / OMB M-26-15 | Build prioritized crypto inventory | 
| Prioritize High Value Assets first | OMB M-26-15 | Order inventory by criticality | 
| Report quantum-vulnerable crypto | OMB M-23-02 §II | Flag RSA, DH and ECC usage | 
| Migration cost and effort estimate | NIST IR 8547 | Produce per-system estimate against 10-20yr baseline | 
| Crypto agility in architecture | NIST SP 1800-38B | Make algorithm choices configurable | 
| Discover crypto in code & dependencies | NIST SP 1800-38 | Scan source, libraries and protocols | 
| PQC-classical hybrid key establishment | NIST IR 8547 §3.2.1 | Pair ECDH with ML-KEM during transition | 
| PQC-classical hybrid signatures | NIST IR 8547 §3.2.2 | Dual-sign for mixed verifier support | 
| CMVP acceleration for PQC modules | EO 14412 | Track updated module validation timelines | 
| Supply-chain PQC readiness | NIST IR 8547 / CNSA 2.0 | Assess vendors, OSS libraries, HSM/TPM support | 
| Signing-chain / provenance assessment | CNSA 2.0 | Review build and firmware signing | 
| Hybrid / transition planning | NIST IR 8547 | Sequence waves toward 2030/2031/2033 | 

## Build the cryptographic inventory your PQC migration depends on

NIST IR 8547, EO 14412 and CNSA 2.0 all start with one question: where is your cryptography? O3 generates a CycloneDX CBOM and QBOM straight from your code and dependencies, inventorying cryptographic assets, flagging quantum-vulnerable usage, and giving you machine-readable evidence you can re-run on every deadline. See how O3 maps your stack against the 2030 and 2031 post-quantum deadlines.

Read NIST IR 8547
## See O3 Security in Action

See why security and engineering leaders trust O3

to secure their entire software supply chain.
