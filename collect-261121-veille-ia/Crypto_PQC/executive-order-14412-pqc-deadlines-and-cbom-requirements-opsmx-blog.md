---
id: collect-261121-veille-ia/crypto-pqc/executive-order-14412-pqc-deadlines-and-cbom-requirements-opsmx-blog
title: "Executive Order 14412: PQC Deadlines and CBOM Requirements | OpsMx Blog"
domain: crypto-pqc
role: reference
task: article
actors: []
dates: []
keywords: ["order", "cbom", "migration", "executive", "december", "cryptographic", "deadlines", "federal"]
source: https://www.opsmx.com/blog/executive-order-14412-pqc-deadlines-and-cbom-requirements/
source_anchor: ""
source_lines: [1, 246]
section: "Crypto_PQC"
---
*Last updated: October 5, 2026*

## Key takeaways

**Executive Order 14412**, signed June 22, 2026, requires federal agencies to move high value assets and high impact systems to post-quantum cryptography (PQC) by December 31, 2030 for key establishment and by December 31, 2031 for digital signatures. It also orders CISA and NIST to publish minimum elements for a cryptographic bill of materials (CBOM) by March 19, 2027.

- **Two hard migration dates:** PQC key establishment by December 31, 2030, and PQC digital signatures by December 31, 2031, for agency high value assets and high impact systems (Sec. 4(b)).
- **The CBOM is named for the first time:** Sec. 5(d) orders public guidance on CBOM minimum elements that “enable the automated assessment” of cryptographic assets, due within 270 days (March 19, 2027).
- **Contractors are next:** the FAR Council must propose a rule by December 19, 2026, requiring covered contractors to comply with NIST FIPS, including PQC standards, by December 31, 2030 (Sec. 6(c)).
- **OMB M-26-15 is the operating manual:** agencies must file PQC migration plans by October 22, 2026, and OMB says the inventory should be automated and feed a central CBOM built from SBOM analysis, code testing and network scanning.
- **Private companies are affected indirectly:** through federal procurement, cloud and SaaS requirements, and sector risk management agencies helping critical infrastructure plan.
- **Start with discovery now:** OMB puts inventory in Phase 1 (2026 to 2027). Teams that already generate SBOMs in CI/CD have the fastest path to a CBOM.

## What is Executive Order 14412?

Executive Order 14412, “Securing the Nation Against Advanced Cryptographic Attacks,” is the US executive order that sets fixed deadlines for moving federal systems to NIST’s post-quantum cryptography standards. It was signed on June 22, 2026 and published in the Federal Register on June 25, 2026.

The order names the threat directly: adversaries collecting US information now and decrypting it later, once large-scale quantum computers work. That is why data encrypted today with RSA or elliptic curve cryptography is already at risk, even though OMB notes that a cryptographically relevant quantum computer (CRQC) “is not yet known to exist.”

The White House also issued a companion order, EO 14413, “Ushering in the Next Frontier of Quantum Innovation,” which deals with quantum innovation policy. The migration deadlines and CBOM requirements in this guide all come from EO 14412.

The order has seven sections. The ones that matter for security and engineering teams:

| Section | What it does | 
|---|---|
| Sec. 2 | Defines key terms, including the **PQC migration lead** , who owns agency-wide cryptographic inventory management | 
| Sec. 4 | Sets agency deadlines for HVAs and high impact systems, and a NIST migration pilot | 
| Sec. 5 | Directs help for critical infrastructure, annual NSA reporting on National Security Systems, and **CBOM minimum elements** | 
| Sec. 6 | Procurement: faster cryptographic module validation and **two proposed FAR rules** for contractors | 

The order builds on earlier federal PQC policy, including NSM-10 and OMB M-23-02, which required annual cryptographic inventories. What it adds is hard dates and an explicit CBOM. If you already track software components under CISA’s 2026 SBOM Minimum Elements, the CBOM is the cryptographic layer on top of that inventory.

## What are the EO 14412 deadlines?

The nearest deadline is October 22, 2026, when agency PQC migration plans are due to OMB, followed by the proposed contractor FIPS rule by December 19, 2026. The order states each deadline as days from signing; the calendar dates below are calculated from June 22, 2026. Status is as of the last update date above.

| Date | Deadline | Who | Source | Status | 
|---|---|---|---|---|
| Jul 22, 2026 | Each agency names a PQC migration lead | Agencies | EO Sec. 4(a) | Date passed | 
| Sep 20, 2026 | OMB issues migration guidance | OMB | EO Sec. 4(b) | Met early (M-26-15, June 24) | 
| Oct 22, 2026 | Agency PQC migration plans due to OMB and ONCD | Agencies | M-26-15 | Upcoming | 
| Dec 19, 2026 | Proposed FAR rule: contractors comply with NIST FIPS incl. PQC | FAR Council | EO Sec. 6(c) | Upcoming | 
| Dec 19, 2026 | First NSA report on National Security Systems migration | NSA | EO Sec. 5(c) | Upcoming | 
| Dec 19, 2026 | NIST revises Cryptographic Module Validation Program processes to speed validations | NIST | EO Sec. 6(b) | Upcoming | 
| Dec 19, 2026 | NIST starts its own PQC migration pilot | NIST | EO Sec. 4(c) | Upcoming | 
| **Mar 19, 2027** | **CBOM minimum elements guidance published** | CISA with NIST | EO Sec. 5(d) | Upcoming | 
| Mar 19, 2027 | Proposed FAR rule: contractor VDPs must cover cryptographic vulnerabilities | FAR Council | EO Sec. 6(d) | Upcoming | 
| Jan 2, 2030 | Agencies support TLS 1.3 or later | Agencies | M-26-15 (EO 14306) | Upcoming | 
| **Dec 31, 2030** | **PQC key establishment for HVAs and high impact systems; contractor FIPS compliance** | Agencies, covered contractors | EO Sec. 4(b), 6(c) | Upcoming | 
| **Dec 31, 2031** | **PQC digital signatures for HVAs and high impact systems** | Agencies | EO Sec. 4(b) | Upcoming | 
| 2035 | Remaining systems migrated | Agencies | M-26-15 Phase 5 | Upcoming | 

National Security Systems are carved out of the Sec. 4(b) inventory review and OMB’s implementing memo; the NSA reports on their migration separately.

## What does EO 14412 say about the CBOM?

Section 5(d) is the first federal directive to name the cryptographic bill of materials. It reads:

“Within 270 days of the date of this order, the Secretary of Homeland Security, through the Director of CISA, and in coordination with the Director of NIST, shall release public guidance describing the agencies’ considered view as to the minimum elements for a cryptographic bill of materials. These elements shall enable the automated assessment of the cryptographic assets utilized by a hardware or software element.”


Three phrases in that sentence shape what your CBOM will need to do:

- **“Minimum elements”** is the same framing the government used for SBOMs, which started with NTIA’s 2021 baseline and now continues in CISA’s 2026 update. Expect a defined list of required fields.
- **“Automated assessment”** rules out spreadsheets. A CBOM has to be machine readable, so tools can check it against policy.
- **“Hardware or software element”** indicates the CBOM describes a specific component or product rather than an entire enterprise, the same way SBOMs work.

**What a CBOM records.** A CBOM is a structured inventory of the cryptography inside a system: algorithms and their parameters, key lengths, protocols and cipher suites, certificates, and the libraries that implement them, plus which component uses each one. For a full primer, see what a CBOM is and how it works. OpsMx’s Beyond SBOM brief covers how the CBOM sits alongside SBOM, DBOM, QBOM and HBOM.

**Why do a CBOM and an SBOM belong together?** An SBOM tells you a service ships OpenSSL 1.1.1. A CBOM tells you that the service uses RSA-2048 for key exchange in two places and ECDSA for signing tokens. The first answers “what is in it.” The second answers “what breaks when a quantum computer arrives.”

## What does OMB M-26-15 add beyond the order?

OMB Memorandum M-26-15, issued June 24, 2026, turns the order into operating instructions, and it is far more specific about the CBOM than the order itself. Most coverage merges the two. Keeping them apart matters, because the EO sets the mandate while the memo tells you how agencies will judge readiness.

**The CBOM recipe.** Appendix A says the foundation of every migration plan is “a dynamic, continuously updated inventory of all cryptographic assets,” built with automated tools wherever possible:

1. Software Composition Analysis (SCA) that analyzes SBOMs
2. SAST and DAST to find cryptographic functions in code
3. Network scanners to detect protocols and cipher suites

That data “should populate a central Cryptographic Bill of Materials (CBOM)” giving a real-time view of cryptographic posture, feeding dashboards that track migration progress.

**Five migration phases.**

| Phase | Years | Focus | 
|---|---|---|
| 1. Strategy, planning, discovery | 2026 to 2027 | Inventory, assessment, governance | 
| 2. Pilots and early migration | 2027 to 2028 | Pilots, first prioritized systems | 
| 3. Prioritized migration | 2028 to 2030 | PQC key establishment, crypto agility | 
| 4. Signature migration | 2031 | PQC digital signatures | 
| 5. Full migration | 2035 | Remaining systems | 

**Other requirements that reach vendors:**

- Plans must describe “the methodologies and automated tools used for the cryptographic inventory” and include a third-party coordination plan.
- Agencies should reference CISA’s product categories list and require PQC integration for products in those categories. OMB’s sample role allocation has program offices ensure vendor requirements include PQC readiness and crypto agility provisions.
- CISA and the Department of War, with GSA, will lead PQC migration for FedRAMP cloud providers and multi-agency SaaS, PaaS and IaaS.
- Secure development practices must mandate PQC-agile libraries for new applications, and OMB recommends keeping algorithm choice in configuration rather than compiled into binaries.
- Plans must align with NIST IR 8547, still an initial public draft, which proposes deprecating quantum-vulnerable algorithms at 112-bit security strength after 2030 and disallowing all quantum-vulnerable algorithms after 2035.

The memo also lists which algorithms to treat as quantum vulnerable: RSA, ECDH, ECDSA, DH, MQV and DSA, and it encourages agencies to treat any asymmetric algorithm not definitively known to be quantum resistant as vulnerable.

## Who does EO 14412 affect?

EO 14412 binds federal agencies directly, reaches contractors through a proposed FAR rule, and reaches everyone else through procurement and critical infrastructure programs.

| Audience | How the order reaches you | What to have ready | 
|---|---|---|
| Federal agencies (civilian) | Direct: Sec. 4 deadlines and M-26-15 plans | Inventory of HVAs and high impact systems, automated CBOM, migration plan | 
| Federal contractors | Proposed FAR rule (due Dec 19, 2026) with a Dec 31, 2030 FIPS compliance target; proposed VDP rule for cryptographic vulnerabilities | Product-level CBOM, PQC roadmap, VDP that accepts crypto findings | 
| Cloud, SaaS, PaaS, IaaS providers to government | M-26-15: CISA and Department of War lead migration for FedRAMP and multi-agency services | Shared responsibility map for PQC, CBOM of the runtime environment | 
| Critical infrastructure operators | Sec. 5(a): Sector Risk Management Agencies help build PQC migration plans | Discovery of long-lived sensitive data and quantum-vulnerable systems | 
| Vendors in CISA’s PQC product categories | M-26-15: agencies should require PQC integration in those product categories | Evidence of PQC support and crypto agility in each release | 
| Other private companies | Indirect: customer questionnaires and contract clauses modeled on federal requirements | A CBOM you can hand over on request | 

If you sell software into regulated markets outside the US, the same inventory work serves other rules. PCI DSS v4.0.1 Requirement 12.3.3 already requires an inventory of cipher suites and protocols in use. Indian banks face RBI and CERT-In expectations covered in our unified BOM guide for RBI-regulated entities, and EU manufacturers face the SBOM duties in our EU CRA compliance guide.

## What EO 14412 does not say

Several claims circulating about the order go further than its text. Getting these right keeps your compliance plan, and your auditors, grounded in what is actually required.

| Common claim | What the text says | 
|---|---|
| “Every company must submit a CBOM by March 2027.” | March 19, 2027 is the deadline for CISA to **publish guidance** on CBOM minimum elements. The order sets no CBOM submission date for anyone. | 
| “Contractors must be PQC compliant under the FAR today.” | Sec. 6(c) orders a **proposed** rule by December 19, 2026. Until a final rule and contract clause exist, the 2030 date is a target, not an enforceable clause. | 
| “The EO mandates ML-DSA by 2031.” | The order sets a PQC deadline for digital signatures but does not name an algorithm. FIPS 203 (ML-KEM), FIPS 204 (ML-DSA) and FIPS 205 (SLH-DSA) are discussed in M-26-15 Appendix A. | 
| “The EO requires hybrid cryptography.” | It does not. M-26-15 calls hybrid a possibly useful but “intricate and resource-intensive stopgap” and leaves the choice to each agency’s risk assessment. | 
| “CycloneDX is the required CBOM format.” | No format has been selected. The order only requires that elements enable automated assessment. | 
| “National Security Systems follow the same dates.” | NSS are carved out of the Sec. 4(b) review and M-26-15. The NSA reports on their migration annually. | 

The practical takeaway: nothing in the order lets you wait. OMB expects discovery in 2026 and 2027, and inventory is the slowest step. The UK NCSC estimates that large organizations need two to three years for discovery and assessment, a migration strategy and an initial plan.

## How do you build a CBOM from your software supply chain?

The fastest route to an EO 14412-ready CBOM runs through the pipeline you already use to build SBOMs. M-26-15’s recipe (SCA on SBOMs, code testing, network scanning) maps to four layers:

1. **Dependencies.** Start from each build’s SBOM and flag cryptographic libraries and their versions: OpenSSL, BoringSSL, Bouncy Castle, libsodium, Go crypto, Java JCA providers. This tells you where cryptography can exist.
2. **Code.** Scan source and binaries for algorithm calls, key sizes and hardcoded parameters. This tells you which cryptography your own code actually uses, including the RSA-2048 key exchange no dependency list will show.
3. **Configuration and runtime.** Capture TLS versions, cipher suites and certificates on load balancers, gateways, service meshes and APIs. M-26-15 names these network terminators specifically. Code scanning alone misses them.
4. **Delivery.** Tie each finding to the artifact, the release and the environment it runs in, so the CBOM stays current with every deployment. A delivery bill of materials (DBOM) provides that link.

**Fields to capture now**, while CISA drafts the official list:

| Field | Why it matters | 
|---|---|
| Algorithm, mode and parameters | Decides quantum vulnerability (RSA, ECC, DH) | 
| Key length or security strength | NIST IR 8547 treats 112-bit strength differently | 
| Primitive (key establishment, signature, hash, symmetric) | Maps to the 2030 and 2031 deadlines | 
| Library and version implementing it | Tells you what to patch or swap | 
| Component and location (file, service, endpoint) | Tells you who owns the fix | 
| Protocol and cipher suite | Covers network use M-26-15 asks for | 
| Certificate details (issuer, algorithm, expiry) | Signature migration and PKI planning | 
| Data sensitivity and retention | Prioritizes data that must stay secret past 2030 | 
| Quantum status (vulnerable, PQC, hybrid) | Tracks migration progress | 

Capturing these in a machine-readable format today means you only need to map fields, not re-inventory, when the minimum elements arrive.

## A 90-day EO 14412 readiness plan

A 90-day plan started this quarter finishes around the time the proposed contractor FIPS rule is due (December 19, 2026) and well before CISA’s CBOM guidance lands in March 2027. It gets you a working CBOM before the official minimum elements exist.

### Days 1 to 30: scope and ownership

- Name an owner for cryptographic inventory, mirroring the EO’s PQC migration lead role
- List the products, services and systems federal customers or regulators rely on, and mark those handling data that must stay confidential past 2030
- Confirm SBOM generation runs in CI/CD for every one of them
- Identify cloud providers and vendors whose cryptography you depend on

### Days 31 to 60: automated discovery

- Generate a CBOM for each in-scope build, covering libraries, code-level algorithm use and certificates
- Scan endpoints for TLS versions and cipher suites, and merge results into the same inventory
- Tag every quantum-vulnerable finding (RSA, ECC, DH, DSA) by primitive: key establishment or signature
- Flag hardcoded algorithms that block crypto agility

### Days 61 to 90: policy and evidence

- Set pipeline policies: block new deprecated algorithms and new hardcoded crypto in releases
- Rank findings by data sensitivity and exposure, and assign owners with target dates
- Draft a migration roadmap aligned to M-26-15 phases and the 2030 and 2031 dates
- Update your vulnerability disclosure policy to accept cryptographic findings, ahead of the Sec. 6(d) rule
- Package the CBOM and roadmap as evidence you can hand to a federal customer or auditor

## How OpsMx helps you meet EO 14412

OpsMx generates the CBOM from the same pipeline that already produces your SBOM, so cryptographic inventory becomes a byproduct of every build instead of a separate project.

- **One scan, multiple BOMs.** OpsMx produces SBOM, AI-BOM, CBOM, DBOM, QBOM and HBOM from a single platform. The QBOM flags quantum-vulnerable cryptography and tracks migration progress. See advanced BOM reporting.
- **Policy gates in CI/CD.** Application security policy enforcement blocks releases that introduce deprecated algorithms or non-compliant components, the “policy enforcement” automation M-26-15 asks for.
- **Signed provenance.** Build provenance and SLSA attestations prove which artifact, built from which source, carries which cryptography.
- **Audit-ready evidence.** Compliance automation keeps a continuous record of policy decisions and approvals, the kind of progress data OMB wants on dashboards.
- **Owner, risk and fix on every finding.** Each finding is routed to the team that owns it, with a risk rating and a recommended fix, so migration work moves instead of piling up.

One limit to plan for: pipeline scanning covers code, dependencies and artifacts. Pair it with network scanning for TLS configuration on infrastructure you do not build, as M-26-15 recommends.

**Get a CBOM for your own pipeline.** Request priority access to OpsMx Extended BOM and see your cryptographic exposure against the 2030 and 2031 deadlines.

## Frequently asked questions

### What is EO 14412?

EO 14412 is the June 22, 2026 executive order “Securing the Nation Against Advanced Cryptographic Attacks.” It requires federal agencies to move high value assets and high impact systems to post-quantum cryptography by 2030 for key establishment and 2031 for digital signatures.

### What are the PQC migration deadlines under EO 14412?

Federal agencies must use post-quantum key establishment for high value assets and high impact systems by December 31, 2030, and post-quantum digital signatures by December 31, 2031. OMB M-26-15 expects remaining systems to be migrated by 2035.

### When is the CBOM guidance due under EO 14412?

CISA, with NIST, must publish CBOM minimum elements guidance by March 19, 2027, 270 days after the order was signed.

### Does EO 14412 require companies to submit a CBOM?

No. The order sets no CBOM submission date for anyone. It directs CISA, with NIST, to publish guidance on CBOM minimum elements by March 19, 2027. Companies feel the requirement indirectly through federal procurement, FedRAMP and customer contract clauses.

### Does EO 14412 apply to federal contractors?

Not directly yet. It orders the FAR Council to propose a rule by December 19, 2026 requiring covered contractors to comply with NIST FIPS, including PQC standards, by December 31, 2030.

### What is OMB M-26-15?

M-26-15 is OMB’s June 24, 2026 memo implementing EO 14412. It requires agency PQC migration plans by October 22, 2026, sets five migration phases through 2035, and recommends an automated, central CBOM.

### Which algorithms are quantum vulnerable under M-26-15?

RSA, ECDH, ECDSA, Diffie-Hellman, MQV and DSA. OMB also encourages agencies to treat any asymmetric algorithm not definitively known to be quantum resistant as vulnerable.

### What is the difference between an SBOM and a CBOM?

An SBOM lists software components and versions. A CBOM lists the cryptography those components use, including algorithms, key lengths, protocols and certificates, and where each is used.

### Is there a bill to make EO 14412 law?

Yes. A House bill to codify EO 14412 was introduced in the 119th Congress and referred to committee. Track its status on Congress.gov.

## Primary sources

- The White House, Executive Order 14412: Securing the Nation Against Advanced Cryptographic Attacks, June 22, 2026
- Federal Register, Executive Order 14412, June 25, 2026
- OMB, M-26-15: Execution of the Migration to Post-Quantum Cryptography, June 24, 2026
- NIST CSRC, Executive Order 14412 topic page
- NIST, IR 8547 (Initial Public Draft): Transition to Post-Quantum Cryptography Standards, November 12, 2024
- CISA, Product Categories for Technologies That Use Post-Quantum Cryptography Standards
- UK NCSC, Timelines for migration to post-quantum cryptography, March 20, 2025
- Congress.gov, Bill to codify Executive Order 14412
