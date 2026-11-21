---
id: collect-261121-veille-ia/regulation-eu/cnsa-2-0-timeline-nsa-deadlines-2027-2030-2031-duokey
title: "CNSA 2.0 Timeline: NSA Deadlines 2027, 2030, 2031 | DuoKey"
domain: regulation-eu
role: reference
task: article
actors: []
dates: []
keywords: ["cnsa", "timeline", "algorithms", "fips", "cnssp", "national", "security", "december"]
source: https://duokey.com/en/resources/cnsa-2-0-timeline-nsa-quantum-resistant-deadlines
source_anchor: ""
source_lines: [1, 168]
section: "Regulation_EU"
---
The **CNSA 2.0 timeline** is the schedule set by the NSA and the Committee on National Security Systems (CNSSP 15) for moving US National Security Systems (NSS) to the quantum-resistant Commercial National Security Algorithm Suite 2.0. CNSA 2.0 algorithms are required in all new NSS acquisitions from **1 January 2027**. Equipment and services that cannot support CNSA 2.0 must be phased out by **31 December 2030**, and CNSA 2.0 is mandated for use by **31 December 2031**, ahead of the 2035 goal in National Security Memorandum 10.

This page lists only dates published by the NSA, sourced to its *CNSA 2.0 and Quantum Computing FAQ* (version 2.2, October 2026). It covers the algorithms, the dates, what they mean for vendors and operators, why firmware signing is treated as urgent, and how CNSA 2.0 relates to the civilian US federal timeline.


## Table of Contents

1. CNSA 2.0 Timeline at a Glance
2. Who CNSA 2.0 Applies To
3. The CNSA 2.0 Algorithms
4. What Each Date Means
5. Why Software and Firmware Signing Comes First
6. Hybrid Cryptography and CNSA 2.0
7. CNSA 2.0 vs the Civilian Federal Timeline
8. A CNSA 2.0 Checklist for Vendors
9. How DuoKey Helps
10. FAQ


## CNSA 2.0 Timeline at a Glance

| Date | Milestone | Source | 
|---|---|---|
| **January 2025** | NSA updates its CNSA 2.0 FAQ after NIST publishes FIPS 203 and FIPS 204, listing the standards that define the suite | NSA, pqc-forum announcement | 
| **1 January 2027** | CNSA 2.0 algorithms required in all new acquisitions for NSS; NSA expects new deployments to be CNSA 2.0 compliant unless a profile explicitly says otherwise | CNSSP 15, as summarized in the NSA FAQ | 
| **Late 2027** | NIAP expected to start archiving certificates of partially quantum-resistant and quantum-vulnerable products; archived products no longer comply with CNSSP 11 | NSA FAQ | 
| **31 December 2030** | All NSS equipment and services that cannot support CNSA 2.0 phased out unless granted an exception; fielded equipment transitions completed; NSA expects encryption to be broadly quantum-resistant by default | CNSSP 15, NSA FAQ | 
| **31 December 2031** | CNSA 2.0 algorithms mandated for use, unless otherwise noted, to achieve full quantum resistance for NSS | CNSSP 15, NSA FAQ | 
| **2035** | Goal of quantum resistance across federal systems set by NSM-10; NSA intends NSS to get there "well ahead" of it | NSM-10, NSA FAQ | 

The NSA also notes that NIAP validation certificates now have a limited life, so products certified before 2027 will need revalidation before the end of the decade.


## Who CNSA 2.0 Applies To

The NSA states that the requirements are primarily for **NSS owners and operators** and for **vendors** who need to know what to implement for NSS, and that it "does not mandate requirements on any other entity outside of NSS". The FAQ is also written as guidance for the Department of War and the Defense Industrial Base.

In practice, CNSA 2.0 reaches further than NSS because:

- vendors sell the same product lines to NSS and commercial customers;
- NIAP Protection Profiles and the Commercial Solutions for Classified (CSfC) program are being updated to CNSA 2.0;
- other regulators and buyers use CNSA 2.0 as a reference for algorithm choices.

Civilian federal agencies follow a different instrument, OMB M-26-15, covered in our US post-quantum regulation guide.


## The CNSA 2.0 Algorithms

The NSA FAQ lists the suite as follows:

| Algorithm | Function | Specification | Parameters | 
|---|---|---|---|
| **AES** | Symmetric encryption | FIPS 197 | 256-bit keys for all classification levels | 
| **ML-KEM** (previously CRYSTALS-Kyber) | Key establishment | FIPS 203 | ML-KEM-1024 for all classification levels | 
| **ML-DSA** (previously CRYSTALS-Dilithium) | Digital signatures in any use case, including firmware and software signing | FIPS 204 | ML-DSA-87 for all classification levels | 
| **SHA** | Hashing | FIPS 180-4 | SHA-384 or SHA-512 | 
| **LMS and XMSS** | Signing firmware and software only | NIST SP 800-208 | All parameters approved; LMS with SHA-256/192 recommended | 
| **SHA-3** | Internal hardware functions only, for example boot-up integrity checks | FIPS 202 | SHA3-384 or SHA3-512 | 

What is **not** in CNSA 2.0, according to the same FAQ:

- **SLH-DSA (SPHINCS+)** "is not part of CNSA and is not approved for any use in NSS", even though it is a NIST standard.
- **HSS and XMSS^MT** , the multi-tree variants in SP 800-208, are not allowed; only LMS and XMSS are.
- **ML-KEM-512 and -768, ML-DSA-44 and -65** are NIST-approved but below the CNSA 2.0 parameter choices.

For the algorithm details, see our guides to ML-KEM and ML-DSA, and the overview of post-quantum cryptography.


## What Each Date Means

### 1 January 2027: new acquisitions

From this date CNSA 2.0 algorithms are required in all new NSS acquisitions, and NIAP Protection Profiles are being updated so products can be certified before it. Where no compliant product exists yet, the NSA says a deployment "may be accepted" on the basis of a contractual commitment from the vendor to deliver a CNSA 2.0 update, combined with a realistic deployment plan that meets the 2030 deadline. It adds that some components, such as firmware update mechanisms, likely cannot be changed after purchase and should be quantum-resistant when bought.

### 31 December 2030: phase-out of non-capable equipment

Equipment and services that cannot support CNSA 2.0 must be phased out by this date unless an exception is granted. The NSA expects fielded equipment transitions to be complete, and notes this may require hardware refreshes. It recommends that even equipment bought before support is mandated has enough memory and processing power to run new algorithms through a software update.

### 31 December 2031: CNSA 2.0 mandated

CNSA 2.0 algorithms are mandated for use, unless otherwise noted, to achieve full quantum resistance for NSS. Until then, the NSA expects a long period in which CNSA 1.0 and CNSA 2.0 run side by side, with components configured to prefer CNSA 2.0 and, as products mature, to accept only CNSA 2.0.

### 2035: the NSM-10 goal

NSM-10 set 2035 as the goal for quantum resistance across federal systems. The NSA's dates above are intended to complete the NSS transition well before it. NSS owners keep annual reporting duties under NSM-10 for any component that still relies on quantum-vulnerable cryptography.

### What about the 2025 and 2033 dates?

Many vendor articles still quote a per-category table with "support and prefer" and "exclusively use" dates running from 2025 to 2033. That table comes from earlier NSA material published with CNSA 2.0 in September 2022. The current NSA FAQ (version 2.2, October 2026) does not include it and gives the CNSSP 15 dates above instead. If a contract or requirement refers to the older category dates, check which document it cites.


## Why Software and Firmware Signing Comes First

The NSA explains that firmware signature validation "is not easily updated", so a firmware signing algorithm is often locked in for the life of a system. A quantum-resistant root of trust may be needed in firmware years before the rest of the system changes. That is why CNSA 2.0:

- allows LMS and XMSS, small hash-based signatures that suit constrained chips that already accelerate hashing;
- also allows ML-DSA-87 for software and firmware signing;
- requires NSS signers to generate LMS and XMSS signatures as specified in SP 800-208, which requires hardware validated by NIST's CMVP, while products that only verify signatures need CAVP-validated verification code.

The practical consequences, including state management for LMS and XMSS, are in our guide to post-quantum code signing.


## Hybrid Cryptography and CNSA 2.0

The NSA's position is specific:

- It has confidence in CNSA 2.0 algorithms and "will not require" NSS developers to use hybrid certified products.
- A system is compliant only when it uses CNSA 2.0 algorithms in an NSA-approved manner. Something "marketed as hybrid or composite" is not automatically quantum-resistant.
- Where a protocol needs a hybrid-like construction, for example IKEv2 because of message-size limits, only the solution the NSA has identified with the standards body counts as compliant.
- You do not need a composite signature to keep a quantum-vulnerable root of trust running during the transition.

For TLS, an NSA-authored IETF draft profile requires ML-KEM-1024 for key establishment. The X25519MLKEM768 hybrid group used by browsers is therefore not, by itself, a CNSA 2.0 configuration. See hybrid post-quantum TLS.


## CNSA 2.0 vs the Civilian Federal Timeline

|  | CNSA 2.0 (NSS) | OMB M-26-15 (civilian agencies) | 
|---|---|---|
| Who | NSS owners, operators and their vendors | Federal civilian agencies, excluding NSS | 
| Algorithms | ML-KEM-1024, ML-DSA-87, LMS/XMSS, AES-256, SHA-384/512 | NIST FIPS 203, 204, 205 | 
| Key dates | 1 January 2027 new acquisitions; 31 December 2030 phase-out; 31 December 2031 mandated | 31 December 2030 key establishment and 31 December 2031 signatures on high-value and high-impact systems | 
| Hybrid | Not required; compliant only where NSA-approved | Accepted as part of a phased, crypto-agile plan | 

Details of EO 14412 and OMB M-26-15 are in the US post-quantum regulation guide.


## A CNSA 2.0 Checklist for Vendors


## How DuoKey Helps

According to DuoKey's product pages and documentation:

- **Inventory and remediation targets.** DuoKey CPM discovers certificates and keys, flags RSA and ECDSA as quantum-vulnerable, produces a CBOM and maps remediation to jurisdiction profiles.
- **Regulatory gap analysis.** The free Quantum Risk Score report maps a domain's observable posture against NIST IR 8547, CNSA 2.0 and other national deadlines.
- **Post-quantum certificates for testing and private PKI.** DuoKey Cockpit's certificate management supports ML-DSA-44/65/87, including ML-DSA-87, the CNSA 2.0 parameter set.

Nothing on this page is a CNSA 2.0 or NIAP compliance claim for a DuoKey product.


## FAQ

**Q: What is the CNSA 2.0 timeline?**

CNSA 2.0 algorithms are required in all new National Security System acquisitions from 1 January 2027. Equipment that cannot support CNSA 2.0 must be phased out by 31 December 2030, and CNSA 2.0 is mandated for use by 31 December 2031. The overall goal set by NSM-10 is 2035.

**Q: Which algorithms are in CNSA 2.0?**

AES-256, ML-KEM-1024 (FIPS 203), ML-DSA-87 (FIPS 204), SHA-384 or SHA-512, plus LMS and XMSS (SP 800-208) for firmware and software signing and SHA3-384/512 for internal hardware functions. SLH-DSA is not approved for NSS.

**Q: Does CNSA 2.0 apply to private companies?**

Only if they own or operate National Security Systems or supply products for them. The NSA says it does not mandate requirements outside NSS, but its requirements often serve as a reference for other buyers and for vendors that sell to both markets.

**Q: What happens on 1 January 2027?**

CNSA 2.0 algorithms become a requirement for new NSS acquisitions, and NSA expects new deployments to be CNSA 2.0 compliant unless a profile says otherwise. If no compliant product is available, a vendor commitment to deliver a CNSA 2.0 update, with a plan that meets the 2030 deadline, may be accepted.

**Q: Is the 2033 deadline still valid?**

The current NSA FAQ (version 2.2, October 2026) gives 31 December 2030 for phasing out equipment that cannot support CNSA 2.0 and 31 December 2031 for mandated use. The 2025 to 2033 category table quoted in many articles comes from earlier NSA material published in 2022.

**Q: Is hybrid TLS compliant with CNSA 2.0?**

Not automatically. The NSA does not require hybrid and considers a system compliant only when it uses CNSA 2.0 algorithms in an NSA-approved manner. An NSA-authored TLS profile requires ML-KEM-1024, while the common hybrid group X25519MLKEM768 uses ML-KEM-768.


## References
