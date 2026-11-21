---
id: collect-261121-veille-ia/regulation-eu/nist-ir-8547-final-what-changed-from-the-draft
title: "NIST IR 8547 Final: What Changed From the Draft"
domain: regulation-eu
role: reference
task: article
actors: []
dates: ["2024-11-12"]
keywords: ["nist", "draft", "final", "after", "under", "systems", "report", "post-quantum"]
source: https://postquantum.com/security-pqc/nist-ir-8547-final/
source_anchor: ""
source_lines: [1, 164]
section: "Regulation_EU"
---
## Table of Contents

**27 Oct 2026** – The U.S. National Institute of Standards and Technology (NIST) published the final version of NIST Internal Report (IR) 8547, *Transition to Post-Quantum Cryptography Standards*, its plan for retiring quantum-vulnerable public-key algorithms, on October 27, 2026. The initial public draft of November 12, 2024, was withdrawn the same day.

NIST kept the draft’s two dates for quantum-vulnerable public-key cryptography, deprecation after 2030 and disallowance after 2035. In the draft, only 112-bit parameter sets such as RSA-2048 and the P-224 curve were to be deprecated after 2030. P-256, RSA-3072, Ed25519 and other parameters at 128 bits or more were to stay acceptable until their disallowance after 2035.

In the final report, NIST deprecates these algorithms at every approved strength after 2030: RSA and ECDSA signatures at 112 bits or more, EdDSA at 128 bits or more, and Diffie-Hellman, MQV and RSA key establishment at 112 bits or more. All of them are disallowed under NIST standards and guidelines after 2035. Under NIST’s definition, a deprecated algorithm may still be used, but the data owner must examine the security risk and decide whether to continue.

NIST aligned the final report with Executive Order (EO) 14412, signed on June 22, 2026, and with Memorandum M-26-15, which the White House Office of Management and Budget (OMB) issued on June 24. Under those directives, federal agencies must move high-value assets and high-impact systems to post-quantum key establishment by December 31, 2030, and to post-quantum digital signatures by December 31, 2031. Agencies must give the same priority to other systems that hold highly sensitive data or are particularly exposed to quantum attack, including systems protecting data expected to remain mission-sensitive in 2030.

M-26-15 does not apply to national security systems, which follow separate requirements, including the National Security Agency’s CNSA 2.0 algorithm suite. NIST wrote that it intends the 2030–2035 deprecation window for lower-risk systems and applications with legacy constraints, and that the window does not change the earlier federal deadlines.

On hybrid key establishment, NIST wrote that the composite technique in NIST Special Publication (SP) 800-56C remains allowed even if one input comes from an unapproved source, including a disallowed algorithm. A combination with at least one input from an approved scheme, such as ML-KEM, can be validated under FIPS 140 even if the other inputs are disallowed, according to the report. NIST also said it will update SP 800-56C to allow the two shared secrets in either order.

NIST noted that it has selected HQC as an alternative to ML-KEM, with a FIPS under development, and that it has no current plans to select further key-encapsulation mechanisms (KEMs). It recommends ML-KEM-768 as the default ML-KEM parameter set. NIST said it expects any further signature or key-establishment schemes it standardizes to be quantum-resistant.

In a new passage, NIST names TLS as an early migration priority and says guidance on hybrid key exchange in TLS will come in the next revision of SP 800-52.

NIST said it is revising the processes of its Cryptographic Module Validation Program (CMVP), as the White House directed in EO 14412. The agency also asked hardware vendors to begin integrating post-quantum algorithms immediately, citing the long lead times for hardware design and fabrication.

“We understand there’s not one strategy that fits all use cases,” said Dustin Moody, the NIST mathematician who heads the PQC standardization project, in NIST’s announcement.

NIST received 25 written submissions on the draft before the comment period closed on January 10, 2025. They came from companies including Bosch, Ericsson, IDEMIA, Infineon, Intel and Thales, from the wireless industry association CTIA and from individual cryptographers.

## My Analysis

In the November 2024 draft, a P-256 key stayed acceptable until the end of 2035 and was disallowed from 2036, with no deprecation step in between. NIST now deprecates P-256 after 2030, on the same day as RSA-2048.

Organizations should review their migration plans for two reasons: the new deprecation date for P-256 and the other 128-bit parameters, and NIST’s answer on hybrid key exchange after 2035. The federal deadlines did not change. The White House set them in EO 14412 in June, and NIST rewrote the report around them.

### What Changed Between the 2024 Draft and the Final Report

| Topic | November 2024 draft | October 2026 final | 
|---|---|---|
| Signatures at 112 bits (RSA-2048, P-224) | Deprecated after 2030, disallowed after 2035 | Unchanged | 
| Signatures at 128 bits or more (P-256, P-384, RSA-3072, Ed25519) | No deprecation step; disallowed after 2035 | Deprecated after 2030, disallowed after 2035 | 
| Classical key establishment (finite-field and elliptic-curve DH and MQV, RSA) | Same split by strength as signatures | Deprecated after 2030 at all approved strengths, disallowed after 2035 | 
| Federal deadlines | 2035 goal from National Security Memorandum 10 (NSM-10) | EO 14412 and M-26-15: key establishment for high-value assets, high-impact systems and other priority systems by Dec 31, 2030, signatures by Dec 31, 2031; TLS 1.3 by Jan 2, 2030; NSM-10’s 2035 goal kept | 
| Hybrid key establishment with a disallowed component | Not addressed explicitly | Can be FIPS 140-validated if at least one input comes from an approved scheme; NIST will update SP 800-56C to allow either order | 
| Hybrid signatures | Accommodated if one component is NIST-approved | Unchanged, plus a caution that their security properties need careful analysis | 
| Second KEM | One or more alternatives to ML-KEM expected | HQC selected, FIPS under development; no current plans for further KEM selections | 
| FALCON-based signature (FN-DSA) | NIST intends to develop a FIPS | NIST is developing a FIPS | 
| ML-KEM parameter set | Not addressed | ML-KEM-768 recommended as the default | 
| New classical algorithms | Not addressed | No further quantum-vulnerable signature or key-establishment schemes expected | 
| Symmetric tables | AES, SHA-1, SHA-2, SHA-3 | Ascon (SP 800-232) added; SHAKE256 preimage strength corrected from 512 to 256 bits | 
| Application guidance | Short mentions of TLS and PIV | TLS named an early priority, with hybrid TLS guidance promised for the next SP 800-52 revision; new paragraph on PIV and federal identity management | 
| Systems that cannot migrate | Not addressed | Identify and prioritize for replacement or decommissioning, citing M-26-15 | 
| Changes to the schedule | Not addressed | NIST will monitor developments and adjust transition efforts as appropriate | 

### From 2031, Every Approved RSA and ECC Key Size Is Deprecated

NIST’s earlier plan, in SP 800-57 Part 1, was to disallow 112-bit public-key schemes on January 1, 2031. In the 2024 draft of IR 8547, NIST relaxed that to deprecation, citing the need to migrate to quantum-resistant algorithms in the same period. Stronger parameters got no intermediate step. IDEMIA was among the commenters who pointed out that users would be reluctant to move while an algorithm stayed acceptable until a few months before its prohibition.

A deprecated algorithm is not banned. NIST defines deprecated as usable with some security risk, which the data owner must examine before deciding whether to continue. Disallowed means no longer allowed for the stated purpose. NIST classes decrypting old ciphertext or verifying old signatures as a separate status, legacy use. IR 8547 does not say how long that status will last for these algorithms.

Any migration plan that kept P-256 or RSA-3072 on a 2035 schedule now contains a deprecated algorithm from 2031. From that date, organizations that follow NIST’s statuses will need a risk decision for every classical-only use of RSA or elliptic-curve cryptography they keep, including ECDHE over P-256 and P-384 in TLS. My recommendation is to record the decision at the system or service level, with an accountable owner, a justification and a migration date.

I expect auditors, insurers and regulators that adopt NIST’s tables to ask for that record. A board that would not fund migration against a Q-Day forecast is more likely to fund it when an auditor asks, because, as I have argued, boards fund obligations, not threats.

A certificate authority that creates a classical signing key in 2026 and plans to issue certificates with it into the 2040s would be using a deprecated algorithm from 2031. Under NIST’s transition statuses, it could not sign new certificates with that key after 2035. Long-term keys that sign code or documents face the same arithmetic. I would give every PKI plan three separate dates: the end of classical issuance, the introduction of post-quantum trust anchors and the retirement of the classical chains that depend on them. The details will depend on SP 800-131A Revision 3 and on each CA’s certificate policy.

NIST also kept the draft’s decision not to disallow 112-bit keys in 2031, as SP 800-57 had planned. It set a single end date, after 2035, for every strength. I agree with both decisions. I would rather organizations spend their 2031 budgets on post-quantum algorithms than on RSA-3072. A deprecated status with no end date would leave each organization to choose its own stopping point. Budget holders postpone dates that nobody enforces.

### Hybrid Key Exchange Can Still Be FIPS 140-Validated After 2035

In the draft, NIST did not say what happens to a hybrid key-establishment scheme when its classical component is disallowed, the question commenters raised most often. During the comment period, Andrew Regenscheid of NIST said on the pqc-forum mailing list that the 2035 disallowance was not intended to apply to hybrid modes that pair an approved post-quantum algorithm with a quantum-vulnerable one. In Sections 3.2.1 and 4.1.2 of the final report, NIST puts that answer in writing for key establishment.

The clearest case is X25519MLKEM768, offered by default in current releases of Chrome, Edge, Firefox and Safari and in OpenSSL 3.5, and marked as recommended in RFC 10024. It is negotiated only when the server supports it as well. X25519 is not a NIST-approved key-agreement scheme. Under SP 800-56C Revision 2, a hybrid secret `Z || T` qualifies for approved key derivation when its first part, `Z`, comes from an approved scheme, and NIST counts an ML-KEM secret as `Z`.

The IETF therefore placed ML-KEM’s secret ahead of X25519’s in X25519MLKEM768. In SecP256r1MLKEM768 and SecP384r1MLKEM1024, it put the ECDH secret in front. Section 5 of RFC 10024 explains the choice. FIPS requires a certified implementation of the approved component, which for some years will be easier to find for P-256 than for ML-KEM.

For X25519MLKEM768, the approved input is already first, so FIPS 140 validation requires a validated ML-KEM implementation. For the two NIST-curve groups, the approved input after 2035 will be the ML-KEM secret in second position. In SP 800-227, published in September 2025, NIST already approves combiners with the approved secret in any position. In IR 8547, though, NIST ties the either-order rule to its promised update of SP 800-56C, the publication that governs the composite technique. Until that update appears, vendors shipping the ECDHE-first groups should confirm with their validation labs how those groups will be treated after 2035. Whether a product complies still depends on its validated module and configuration.

NIST still writes that hybrid solutions are typically expected to be temporary measures leading to a second transition to post-quantum-only cryptography. Under the final report’s rules, though, an organization could still run X25519MLKEM768 with a validated ML-KEM implementation in 2036. NIST sets no date for the second transition in IR 8547. Application-specific guidance such as the next SP 800-52 revision could still set one.

Organizations that follow both NIST and the European agencies that recommend hybrid key establishment, such as France’s ANSSI and Germany’s BSI, gain the most. I described the difference between those positions in The Hybrid Question. A hybrid deployment can now meet both after 2035, if it uses the algorithms, construction and validated implementation each requires. US federal agencies still have to weigh OMB’s description of hybrid, in M-26-15, as “an intricate and resource-intensive stopgap.”

NIST describes hybrids as a hedge against a flaw in one component. In the final report, it added a caution that early implementations of new algorithms may introduce implementation errors and side-channel vulnerabilities. This year Daniel J. Bernstein demonstrated ML-DSA key recovery through implementation bugs, and wolfSSL fixed flaws in its ML-KEM ciphertext check that Bhabani Sankar Das and Nicholas Carlini reported. NIST also clarified that a hybrid can combine two post-quantum algorithms built on different mathematical assumptions.

NIST’s paragraph on the generic concatenation technique is a simplification. In Section 4.6.3 of SP 800-227, NIST explains that a combiner using only the shared secrets does not, in general, preserve IND-CCA security, the standard notion of protection against active attackers, when only one component remains secure. NIST encourages combiners that also take in the ciphertexts. The authors of RFC 10024 base their security analysis on the TLS 1.3 handshake transcript and warn that the construction cannot be assumed secure in other protocols. Teams building hybrids for other protocols should use a construction with its own security analysis, following SP 800-227.

### NIST Rewrote IR 8547 Around the Deadlines in EO 14412

NIST proposed the 2030 deprecation date itself in the 2024 draft. The 2035 goal came from National Security Memorandum 10. In June 2026, the White House made 2030 a hard deadline for key establishment in federal priority systems through EO 14412. OMB then told agencies, in M-26-15, to align their migration plans with IR 8547 or any successor document. NIST now deprecates classical key establishment from January 1, 2031, the day after that deadline.

OMB gave agencies 120 days to submit the plans, a deadline that fell around October 22, five days before NIST published the final report. The priority-system deadlines are the same in the final report, so I expect little rework in those plans. Agencies planning lower-risk systems, which can finish migrating later in the period up to 2035, now have to account for a deprecated status from 2031 on any 128-bit classical algorithm they keep.

NIST also removed the draft’s statement that “flexibility in migration planning is essential.” In NIST’s view, timelines will still vary by use case, now within the federal dates. For systems that cannot support post-quantum cryptography, NIST repeats OMB’s instruction to identify them and prioritize them for replacement or decommissioning. NIST offers no blanket exemption in IR 8547 for embedded devices that stay in service for decades or for old mainframes, beyond legacy use of data that is already protected.

NIST deleted the draft’s description of its standards as covering federal and nonfederal systems alike. It now calls the report a blueprint for critical infrastructure owners and operators and other organizations. Outside the federal government, IR 8547 is guidance unless regulators or customers write it into rules or contracts. I expect regulators and auditors in other countries to copy its transition tables.

Federal contractors are in a different position. In EO 14412, the White House gave the Federal Acquisition Regulatory Council, the body that writes federal procurement rules, 180 days to propose a rule requiring covered contractors to comply with NIST’s FIPS, including those for post-quantum algorithms, by December 31, 2030, as I covered in my analysis of the federal PQC mandate.

### Other Changes in the Final Report

- **HQC as the alternative KEM.** NIST has selected HQC, a code-based scheme, as the alternative to the lattice-based ML-KEM, and has no current plans for further KEM selections. Organizations planning crypto-agility for key establishment now know which alternative NIST plans to standardize. At comparable security categories, HQC’s keys and ciphertexts are larger than ML-KEM’s, so protocols and hardware need room for them.
- **ML-KEM-768 as the default.** NIST recommends ML-KEM-768, as it already did in FIPS 203. It permits other parameter sets where ML-KEM-768 is impractical or higher security is required. ML-KEM-1024 meets both NIST’s guidance and CNSA 2.0, which requires it for national security systems. Vendors serving both markets will still need ML-KEM-768 to interoperate with X25519MLKEM768 and other protocols built on it.
- **A schedule NIST can change.** NIST now says it will monitor developments and adjust its transition efforts as appropriate. I would treat 2035 as the latest date NIST accepts today, and one that NIST could still move.
- **Ascon for constrained devices.** NIST added the Ascon family, which SP 800-232 standardized for constrained devices, to its symmetric tables. In the new table, NIST printed the authenticated-encryption entry as “AES-AEAD128” by mistake.
- **Corrections.** NIST fixed the mislabeled ML-KEM parameter sets in Table 5, an error seven commenters reported, and corrected SHAKE256’s preimage strength from 512 to 256 bits.
- **PIV credentials.** NIST describes the work needed to move Personal Identity Verification (PIV) credentials, the smart cards federal employees and contractors use, to post-quantum algorithms. Because credentials follow multi-year issuance cycles, NIST says updates to standards, modules and products must begin early.
- **Firmware signing.** NIST now names software and firmware signing as the intended use of the stateful hash-based schemes LMS and XMSS. It also says hardware modules must use post-quantum algorithms to protect the integrity of software and firmware updates.

### What NIST Left Unresolved

#### One Date for Key Establishment and Signatures

NIST explains in several places that key establishment is more urgent than signatures because of harvest now, decrypt later (HNDL) attacks. In Tables 2 and 4, it nonetheless gives both the same dates. Federal priority systems get one extra year for signatures under EO 14412. Beyond that, NIST leaves any earlier deadline for key establishment to application-specific guidance, such as the next revision of SP 800-52 for TLS. A non-federal team that copies Tables 2 and 4 into its policy will find no priority for key establishment there.

#### Authentication and the Arrival of a CRQC

In Section 3.1.2, NIST still writes that authentication systems may keep using quantum-vulnerable algorithms until a cryptanalytically relevant quantum computer (CRQC) becomes available, and must then transition immediately. NIST does not reconcile that paragraph with Table 2, which disallows the same algorithms after 2035 whether or not a CRQC exists. Readers should not take Section 3.1.2 as permission to wait for a public CRQC.

An immediate switch requires post-quantum authentication that is already deployed and knowledge that a CRQC exists. Whoever builds the first CRQC has no obligation to announce it. If a capable machine does become public, customers and counterparties are likely to stop trusting classical signatures almost at once, as I argued in Q-Day Isn’t an Outage – It’s a Confidence Crisis. Only organizations that deployed post-quantum authentication well before they needed it will be able to switch immediately.

Anja Lehmann of the Hasso Plattner Institute and Anna Lysyanskaya of Brown University cited the same paragraph in a January 2025 comment on digital identity. The EU’s eIDAS regulation, they wrote, requires the European Digital Identity Wallet, due by the end of 2026, to support pseudonymous and unlinkable authentication, and all practical, mature schemes with those properties relied on discrete-logarithm or RSA assumptions. NIST did not answer their request to clarify that the 2035 disallowance is not a general ban on classical cryptography.

#### Signed Records That Must Stay Verifiable for Decades

NIST added a sentence that signatures are not vulnerable to HNDL attacks but still need urgent migration, because the transition takes longer and involves more parties. A second reason is the risk I call Trust Now, Forge Later (TNFL): once a CRQC can forge signatures, an attacker can sign anything under a classical public key that verifiers already accept. NIST covers that risk for code signing in Section 3.1.1 and for firmware updates in its section on cryptographic hardware. It does not mention contracts, property registries, archived evidence and other records whose signatures must be checked for decades.

Preserving such signatures takes evidence records, such as those defined in RFC 4998, that cover the document, its signature and its validation data, renewed with post-quantum algorithms before the algorithms protecting them weaken. Once classical signatures can be forged, nobody can use a new timestamp to prove that an existing signature was made earlier.

#### Hybrid Signatures Remain Underspecified

NIST describes a dual signature as two or more signatures on a common message, all of which must verify. It repeats the draft’s position that its standards accommodate dual signatures provided at least one component is NIST-approved. It specifies no encoding or binding mechanism and says nothing about a dual signature whose classical component is disallowed after 2035. Regenscheid wrote his forum answer about hybrid modes in general, so the one-approved-component rule could be read to accept such a signature, but NIST does not say so, as it now does for key establishment.

In the straightforward construction that Bertram Poettering analyzed in his comment on the draft, where each component signature is verified separately, pairing ML-DSA with ECDSA loses strong unforgeability. ECDSA signatures are malleable, so an attacker can produce a second valid signature on a message that was already signed. The attacker still cannot sign new messages, but systems that treat a signature as a unique identifier can break.

The IETF’s specification for composite ML-DSA signatures in X.509 certificates has reached the RFC Editor queue but is not yet a published RFC. Its authors state that none of its combinations is strongly unforgeable against a quantum adversary and advise against using composites in applications sensitive to the difference. PKI teams designing dual-signature hierarchies can work from it, without an explicit NIST statement on post-2035 status to cite.

#### NIST Has Not Finished the Companion Documents

IR 8547 is an internal report. The approval statuses that NIST’s module validation program applies come from SP 800-131A. In the final report’s references, NIST cites SP 800-131A Revision 2 from 2019 and notes the initial public draft of Revision 3 from October 2024, which is still a draft. NIST lists three revisions as future work: SP 800-131A, with more detailed guidelines and schedules; SP 800-56C, on the order of hybrid secrets; and SP 800-52, with hybrid TLS guidance.

Implementers can start without them. ML-KEM, ML-DSA and SLH-DSA are final standards, and organizations can migrate to them without waiting for FN-DSA and HQC. The White House gave NIST until about December 19, 2026, to revise CMVP processes and speed up validations. NIST writes that the work is under way.

### What Organizations Should Do With the Final Report

My recommendation for any organization, federal or not, is to treat December 31, 2030, as the latest date for ending classical-only key establishment. For federal priority systems, that date is already a deadline; for everyone else, I treat it as a planning backstop. An adversary who records traffic before an organization switches can decrypt it once a CRQC exists, so systems that protect data needing confidentiality for many years should move earlier wherever possible. From 2031, every approved quantum-vulnerable key-establishment scheme is deprecated.

For services on modern TLS stacks, hybrid key exchange such as X25519MLKEM768 is the most accessible first step. NIST has now confirmed a FIPS 140 path for it after 2035, which requires a validated ML-KEM implementation. Some deployments can enable it with a library upgrade and a configuration change. Others depend on load balancers, appliances, cloud services or validated modules that vendors must upgrade first. Hybrid key exchange protects the session keys only. The server still authenticates with its RSA or ECDSA certificate chain, which needs its own migration.

With an X25519MLKEM768 key share, the TLS ClientHello grows by about 1.2 KB and can arrive in more than one packet. Some servers fail to read a ClientHello split that way and reject the connection instead of negotiating classical key exchange, a problem documented at tldr.fail. The bug is not specific to post-quantum cryptography, but servers hit it far more often with post-quantum key shares.

Organizations should test peers, TLS termination points and middleboxes, then log the key-exchange group that each production connection negotiates. A server can offer X25519MLKEM768 and still complete classical-only handshakes with clients that do not support it. Each hop needs its own check: post-quantum key exchange between a browser and a CDN edge does not extend to the connection from the edge to the origin server.

Signature migration takes longer, with work in PKI, HSMs, smart cards and PIV credentials. An ML-DSA-44 signature is 2,420 bytes, more than 30 times the 64–72 bytes of an ECDSA signature on P-256, so certificate chains and handshakes grow. Organizations should ask their HSM, TPM and smart-card vendors for dated roadmaps for ML-DSA and, for firmware, LMS or XMSS.

For a device that could outlast the arrival of a CRQC, support for post-quantum signature verification is not enough: its owner also needs a way to make it reject classical-only firmware updates, for example by updating its trust anchors and signature policy. In Section 3.1.1, NIST says that devices which cannot update their verification code should be designed to require quantum-resistant signatures if they may still be in use after a CRQC appears.

NIST tells organizations to begin with cryptographic inventories. In a report released publicly this month, the U.S. Government Accountability Office (GAO), the audit agency of the U.S. Congress, found that none of 24 federal agencies had fully addressed all three preparatory practices it assessed. Only one had fully addressed the activities for building a prioritized inventory of systems with vulnerable cryptography. The audit ran from February 2024 to September 2025. OMB’s deadline for the first inventories was May 2023, as I noted in my coverage of the report.

Over the next year, I will be watching for these documents:

- The proposed procurement rule on contractor compliance with NIST’s FIPS, due around December 19, 2026.
- NIST’s revised CMVP processes, due around the same date.
- Guidance on the minimum elements of a cryptographic bill of materials (CBOM), which the Cybersecurity and Infrastructure Security Agency (CISA) must publish with NIST around March 19, 2027.
- SP 800-131A Revision 3 in final form, which NIST says will bring more detailed guidelines and schedules.
- NIST’s update to SP 800-56C allowing either order of hybrid secrets.
- A new revision of SP 800-52, with guidance on hybrid key exchange in TLS.
- Final standards for FN-DSA, expected as FIPS 206, and for HQC.

P-256 had no deprecation date in the 2024 draft. NIST has now set one: January 1, 2031. By then, every classical dependency an organization keeps should have an owner, a migration date and a replacement path.
