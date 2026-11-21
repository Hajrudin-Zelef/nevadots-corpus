---
id: collect-261121-veille-ia/crypto-pqc/cnsa-2-0-january-2027-deadline-pqc-checklist-for-nss-vendors
title: "CNSA 2.0 January 2027 deadline: PQC checklist for NSS vendors"
domain: crypto-pqc
role: reference
task: article
actors: []
dates: ["2026-11-01"]
keywords: ["cnsa", "january", "products", "product", "cnssp", "version", "niap", "vendors"]
source: https://ecorpit.com/nsa-cnsa-2-0-post-quantum-procurement-2027/
source_anchor: ""
source_lines: [1, 158]
section: "Crypto_PQC"
---
**Summary.** From 1 January 2027, CNSSP 15 requires CNSA 2.0 algorithms in new products and services that protect US National Security Systems (NSS), unless NSA messaging, a Protection Profile, a Capability Package or a waiver says otherwise. The suite: AES-256, ML-KEM-1024, ML-DSA-87, SHA-384 or SHA-512, plus LMS or XMSS for software and firmware signing. NSA's FAQ version 2.2 (October 2026) changes the vendor picture: where no compliant product exists, a buyer may accept a contractual promise of a later CNSA 2.0 update, but firmware signing should be quantum resistant at purchase. NIAP Policy Letter 33 adds its own gates: a product that misses CNSA 2.0 on any cryptographic function cannot enter evaluation from 1 January 2028 or join the compliant list from 1 January 2029. Equipment that cannot support CNSA 2.0 must go by 31 December 2030, and CNSA 2.0 is mandatory by 31 December 2031. Three traps catch vendors: default X25519MLKEM768 TLS uses the wrong parameter set, stateful signing needs its own pipeline, and algorithms alone do not qualify a product.

**About this article.** This is for product, security and compliance leads at vendors that sell into US NSS or supply those who do. On 1 November 2026 we checked it against CNSSP 15 (cover dated December 2024, signed 28 February 2025), NSA's FAQ version 2.2, NIAP Policy Letter 33 of 31 August 2026, Executive Orders 14306 and 14412, OMB M-26-15, NIST standards and IETF documents. It is not legal, export-control or procurement advice: contract language and Protection Profiles set the binding terms, so confirm with your contracting officer. No vendor paid for inclusion. Researched with AI assistance and reviewed by our editorial team.

## What 1 January 2027 means, precisely

CNSSP 15 is the binding text. From 1 January 2027 it requires CNSA 2.0 algorithms in all new products and services that give cryptographic protection to users or to updates, unless excepted through nsa.gov messaging, a Protection Profile, a Capability Package or a waiver. NSA's FAQ applies the date to all new NSS acquisitions. NIAP adds that government contracts from that date must require products used in NSS to implement post-quantum cryptography or be updateable to it.

- **It is an NSS rule.** NSA mandates nothing outside NSS. Most Department of War and Intelligence Community systems qualify; an NSA briefing in March 2026 listed classified networks, NIPRNet, department-owned industrial control systems, GPS and weapon systems. NIST SP 800-59 covers borderline cases.

- **There is a fallback when no product exists.** A deployment may be accepted on a vendor's contractual commitment to deliver a CNSA 2.0 update, with a realistic plan that prioritises partial quantum resistance and meets the 2030 deadline. It will be tracked and reported.

- **The fallback does not stretch to the root of trust.** NSA says firmware update options probably cannot change after purchase, and warns buyers they may have to replace products in 2030. From 2027, no new NSS product should have a burned-in root of trust that relies on a quantum-vulnerable primitive.

- **Existing certificates are on a clock.** FAQ version 2.1 let a validated NSS stay approved for the life of that validation. Version 2.2 drops that line: NSA expects NIAP to start archiving partially quantum-resistant and quantum-vulnerable certificates in late 2027, archived products stop complying with CNSSP 11, and everything certified before 2027 needs revalidation before the decade ends. Policy Letter 33 leaves the archive date open.

- **Waivers got harder.** CNSSP 15 cancelled all waivers to its previous version. Activities relying on them got six months to comply or 90 days to file a new request.

NIAP's gates test every cryptographic function, not a product's headline feature. Policy Letter 33 takes effect on 1 January 2027 and gives fully compliant products priority. Products below CNSA 1.0 minimums on any function also lose Assurance Maintenance extensions and are capped at two-year certificates. Test lab Lightship Security reads the NIAP dates below as evaluation and listing gates, not operating deadlines for deployed systems.

The timeline draws on CNSSP 15, NSA's FAQ and briefing, NIAP, NIST and two executive orders.

| Date | Milestone | 
|---|---|
| End of 2026 | All NSA-authored Protection Profiles due to be rewritten, at minimum with quantum-resistant update functions | 
| 1 January 2027 | New NSS products and services need CNSA 2.0; NIAP stops taking products below CNSA 1.0 minimums | 
| 1 July 2027 | NIAP stops posting products below CNSA 1.0 minimums | 
| Late 2027 | NSA expects NIAP certificate archiving to start | 
| 1 January 2028 | NIAP stops taking products that miss CNSA 2.0 on any function | 
| 1 January 2029 | NIAP stops posting those products | 
| 2 January 2030 | Agencies must support TLS 1.3 or a successor (NSS and non-NSS) | 
| 31 December 2030 | Equipment and services that cannot support CNSA 2.0 phased out | 
| 31 December 2031 | CNSA 2.0 mandated for all protocol use unless excepted or waived | 
| 2035 | NSM-10 goal for quantum resistance; NIST disallows quantum-vulnerable public-key algorithms after this year | 

## The CNSA 2.0 suite, and what does not count

CNSA 2.0 fixes its parameter sets for all classification levels. The last column matters as much as the second, because each item in it is something vendors already ship. The table follows NSA's FAQ and CNSSP 15 Annex B.

| Function | CNSA 2.0 requirement | Standard | Not acceptable for NSS | 
|---|---|---|---|
| Symmetric encryption | AES with 256-bit keys | FIPS 197 | AES-128 or AES-192 | 
| Key establishment | ML-KEM-1024 | FIPS 203 | ML-KEM-512, ML-KEM-768, pre-standard CRYSTALS-Kyber, larger RSA or ECC keys | 
| Digital signatures (any use) | ML-DSA-87 | FIPS 204 | ML-DSA-44 or ML-DSA-65, HashML-DSA, FN-DSA (Falcon), SLH-DSA | 
| Hashing | SHA-384 or SHA-512 | FIPS 180-4 | SHA-3 or SHAKE as a general-purpose hash | 
| Software and firmware signing | LMS or XMSS, all parameters, LMS SHA-256/192 recommended; ML-DSA-87 also allowed | NIST SP 800-208 | HSS, XMSS^MT, SLH-DSA | 
| Internal hardware integrity (for example secure boot) | SHA3-384 or SHA3-512 | FIPS 202 | Use outside the vendor's internal hardware, including VMs chained to a virtual root of trust | 

RSA and elliptic-curve cryptography cannot be fixed with bigger keys. SLH-DSA (FIPS 205) is not approved for any NSS use. HashML-DSA is not allowed at present, and NSA does not expect to add FN-DSA or the results of NIST's signature on-ramp. SHA-3 and SHA-256 are also out as building blocks inside DRBGs and key derivation functions unless NSA guidance names the use.

## Three traps for commercial vendors

### Trap 1: your post-quantum TLS is probably ML-KEM-768

X25519MLKEM768, the post-quantum default on the public internet, uses ML-KEM-768. Cloudflare says recent versions of all major browsers enable it by default. OpenSSL 3.5.0 (8 April 2025) made it a default key share. JDK 27, generally available since 15 September 2026, puts it first, leaves its SecP384r1MLKEM1024 hybrid switched off and does not implement pure ML-KEM for TLS. None of these defaults meets CNSA 2.0.

A larger hybrid is not the fix. RFC 10024 (August 2026) standardises SecP384r1MLKEM1024. NSA's draft TLS 1.3 profile still requires pure MLKEM1024 (code point 0x0202) as the first group offered, and a server building a CNSA-compliant connection must refuse anything else.

These documents are close, not final. The group's specification, draft-ietf-tls-mlkem-11, is in the RFC Editor queue. The CNSA 2.0 profiles for TLS, SSH, IPsec, S/MIME, certificates and CMC were all still Internet-Drafts at the end of October 2026. Pin your design to a named revision.

NSA does not require hybrids, and only a hybrid it has identified counts as compliant. Its named exception is IKEv2: ML-KEM-1024 is too large for the unencrypted first messages, so the profile runs a CNSA 1.0 exchange first and ML-KEM-1024 second, inside an encrypted exchange.

### Trap 2: signatures are stricter than key exchange

ML-DSA-87 is the only general signature algorithm. For PKI, NSA expects a parallel ML-DSA-87 hierarchy and warns against novel ways of carrying several algorithms in one certificate.

Firmware comes first because verification code is often fixed for the life of a system, so NSA wants the move to LMS or XMSS to start now. CNSSP 15 adds that distributed, high-volume or virtualised signing systems may be unable to meet SP 800-208 and should move to ML-DSA-87.

On-device verification code is expected to pass NIST's Cryptographic Algorithm Validation Program (CAVP), and a product that only verifies needs nothing more. Signers that are themselves NSS must use hardware validated under the Cryptographic Module Validation Program (CMVP), or follow other NSA guidance, with no waivers. Vendor build environments are often not NSS: NSA then validates only the on-device verifier, but still advises signing and state management in hardware such as an HSM, with backups that prevent state reuse. SP 800-208 explains why: two messages signed with one one-time key make forgery feasible. Our software supply chain security playbook covers the wider signing controls.

One catch: NIST's ACVP index lists LMS tests but none for XMSS, and Lightship reported in September 2026 that CAVP does not yet cover XMSS. NSA prefers LMS SHA-256/192 anyway.

### Trap 3: algorithms are necessary, validation is what you sell

NSA says CNSSP 11 requires NIAP validation for commercial products used in NSS, generally with CNSA 2.0 compliance and typically CMVP-validated modules. Without CNSA 2.0 algorithms or specific written NSA guidance, a commercial product cannot protect NSS, and NSA takes no direct vendor requests to validate novel cryptography.

FIPS 140-2 is finished for new work. CMVP now allows those modules for existing systems only, and its search showed no active FIPS 140-2 certificates on 1 November 2026. Plan on FIPS 140-3. Executive Order 14412 gives NIST until about 19 December 2026 to revise CMVP processes and speed validations.

Protection Profiles are the moving part. NSA says nearly all now have quantum-resistant options. Lightship says they do not yet consistently support CNSA 2.0 on every function. Read your own profile.

## Vendor checklist by product type

The table combines NSA's FAQ with its draft TLS, IPsec and certificate profiles.

| Product type | What a 2027 NSS bid should show | Evidence buyers will ask for | 
|---|---|---|
| VPN, routers, network encryptors | IKEv2 per NSA's draft profile, AES-256, SHA-384, a dated ML-DSA-87 certificate roadmap | Protection Profile status, CAVP certificates, CSfC listing where relevant | 
| TLS servers, web applications, APIs | TLS 1.3 with MLKEM1024, ML-DSA-87 and AES-256-GCM with SHA-384; a CNSA mode that prefers and later enforces CNSA 2.0 | Configuration guide, cryptographic bill of materials (CBOM) | 
| Firmware, embedded and hardware | Quantum-resistant root of trust at purchase, using LMS or XMSS verified boot; headroom for ML-KEM and ML-DSA | CAVP certificate for verification code; signing architecture, with CMVP-validated hardware where the signer is NSS | 
| Packaged software and update services | Updates signed with LMS, XMSS or ML-DSA-87 | Signing architecture, state-management procedure | 
| PKI and certificate authorities | ML-DSA-87 roots and issuing CAs in a parallel hierarchy | Certificate profiles, HSM validation, interoperability test results | 

The actions behind that table:

1. **Confirm the buyer is an NSS.** Ask the programme office.

1. **Read the contract clause,** and agree what any delivery commitment and deployment plan must contain.

1. **Build a CBOM.** Inventory every public-key use, then score each function against CNSA 1.0 and 2.0, because NIAP does.

1. **Ship the right parameter sets,** with a CNSA mode that cannot silently fall back to ML-KEM-768. NSA asks for components that prefer CNSA 2.0 first and later accept only it.

1. **Fix the firmware root of trust first.** LMS SHA-256/192 with a CAVP-tested verifier, and signing in hardware with tested state backup.

1. **Get CAVP certificates early,** then plan FIPS 140-3 module validation.

1. **Map your NIAP dates.** Know your profile and the evaluation start that beats 1 January 2028, then book lab capacity.

1. **Strip non-CNSA options from NSS builds.**

1. **Leave hardware headroom** so upgrades can ship as software, as NSA asks.

1. **Test with large messages:** MTU limits, IKEv2 fragmentation, certificate chains and constrained links. Public certificates add a related job: 100-day TLS lifetimes make rotation automation essential.

1. **Publish a dated transition roadmap,** as NSA's March 2026 slides ask.

## Worked example: the handshake byte budget

Take a TLS 1.3 server following NSA's draft profile: an ML-KEM-1024 key exchange and a two-certificate chain (leaf plus intermediate, root not sent), all ML-DSA-87. Sizes come from FIPS 203 and FIPS 204, before X.509 overhead.

| Handshake element | Bytes | 
|---|---|
| ML-KEM-1024 encapsulation key (client) | 1,568 | 
| ML-KEM-1024 ciphertext (server) | 1,568 | 
| Leaf certificate: public key 2,592 + issuer signature 4,627 | 7,219 | 
| Intermediate certificate: public key 2,592 + signature 4,627 | 7,219 | 
| CertificateVerify signature | 4,627 | 
| Post-quantum key and signature material | 22,201 | 

For comparison, X25519MLKEM768 uses 1,216 bytes from the client (1,184 + 32) and 1,120 from the server (1,088 + 32), or 2,336 bytes, against 3,136 for ML-KEM-1024. Authentication dominates: 7,219 + 7,219 + 4,627 = 19,065 of the 22,201 bytes, about 86%. Test constrained radios and older middleboxes before a bid.

Firmware signing has its own arithmetic. SP 800-208 allows LMS tree heights of 5, 10, 15, 20 and 25, and a tree of height h holds 2^h one-time keys. A pipeline signing 40 images a day for 250 working days needs 40 x 250 = 10,000 signatures a year. A height-15 tree (32,768 keys) lasts about 3.3 years. A height-20 tree (1,048,576 keys) lasts about 105 years but takes longer to generate. Choose before the first device ships, because the verifier may live in boot ROM. HSS is not allowed, so if one tree cannot cover your volume, NSA points to ML-DSA-87.

## Selling to non-NSS federal and defence buyers

Outside NSS, NIST sets the standards, and FIPS 203 defines all three ML-KEM parameter sets, so ML-KEM-768 hybrids remain a reasonable civilian choice. Federal actions still reach commercial vendors:

- **Executive Order 14412 (22 June 2026)** excludes NSS. High value assets and high impact systems must reach post-quantum key establishment by 31 December 2030 and signatures by 31 December 2031. The FAR Council must propose, by about 19 December 2026, a rule requiring covered contractors to comply with NIST's FIPS, including post-quantum ones, by 31 December 2030. Our Federal Register search on 1 November 2026 found no such proposal yet.

- **OMB M-26-15 (24 June 2026)** has agencies file migration plans and suggests writing post-quantum readiness into vendor requirements.

- **Executive Order 14306 (6 June 2025)** set the TLS 1.3 date of 2 January 2030, with requirements from NSA for NSS and from OMB for other systems.

- **CISA's list (23 January 2026)** says to buy only post-quantum-capable products in its widely available categories, such as PaaS and IaaS cloud, browsers, web servers and full-disk encryption. Networking hardware, identity systems and HSMs are still transitioning. NIST IR 8547, final since October 2026, deprecates quantum-vulnerable public-key algorithms after 2030.

Defence suppliers sit in both worlds. NSA also aims its FAQ at the Department of War and the Defense Industrial Base, so corporate IT may follow NIST while the product delivered into a weapon system follows CNSA 2.0.

## India angle: suppliers inside US defence supply chains

Indian engineering firms are most likely to meet CNSA 2.0 indirectly, through firmware, cryptographic libraries, network operating systems, PKI software and services for US primes. We expect four asks from the prime: ML-KEM-1024 and ML-DSA-87 support, LMS signed firmware, a CBOM and SBOM, and CAVP evidence. That is our reading, not a published NSA rule.

Keep cryptography behind a provider interface so parameter sets are configuration, as our crypto-agility playbook describes. Export controls can apply to keys, HSMs and source code, so involve trade counsel before shipping them across borders. Our post-quantum readiness roadmap covers the inventory that comes first.

## How eCorpIT can help

eCorpIT is a Gurugram-based technology organisation, founded in 2021, assessed at CMMI Level 5 and MSME certified, with senior-led engineering teams working across AWS, Microsoft and Google platforms. Through our cybersecurity services we build cryptographic inventories and CBOMs, refactor applications behind crypto-agile provider layers and test ML-KEM-1024, ML-DSA-87 and LMS signing configurations, while formal NIAP and CMVP evaluations stay with accredited labs. Talk to us at /contact-us/.

Last updated: 1 November 2026.
