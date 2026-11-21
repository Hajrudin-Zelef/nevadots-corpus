---
id: collect-261121-veille-ia/securite/the-state-of-post-quantum-cryptography-in-2026-blog
title: "The State of Post-Quantum Cryptography in 2026 Blog"
domain: securite
role: reference
task: article
actors: []
dates: []
keywords: ["ml-kem", "algorithm", "fips", "cryptography", "nist", "post-quantum", "lattice-based", "signature"]
source: https://dev.to/quantumsequrity/the-state-of-post-quantum-cryptography-in-2026-blog-1g6p
source_anchor: ""
source_lines: [1, 138]
section: "Securite"
---
Research

14 min read

For eight years, from 2016 to 2024, the global cryptography community participated in NIST's Post-Quantum Cryptography Standardization Project. Hundreds of researchers from dozens of countries submitted 82 candidate algorithms. These candidates went through multiple rounds of public analysis, attack attempts, and performance benchmarking. In August 2024, NIST published three final standards:

- 
**FIPS 203 (ML-KEM):** Module Lattice-Based Key Encapsulation Mechanism. This is the algorithm that protects encryption keys during exchange. When two parties need to agree on a shared secret to encrypt data, ML-KEM provides that exchange with quantum resistance.
- 
**FIPS 204 (ML-DSA):** Module Lattice-Based Digital Signature Algorithm. This is for digital signatures, the cryptographic equivalent of a handwritten signature that proves a document has not been altered and verifies who created it.
- 
**FIPS 205 (SLH-DSA):** Stateless Hash-Based Digital Signature Algorithm. A backup signature algorithm based on hash functions rather than lattice math, providing algorithm diversity in case a weakness is ever found in lattice-based approaches.

Those standards are no longer drafts or proposals. They are final, published NIST Federal Information Processing Standards, carrying the same authority as AES (FIPS 197) and SHA-2 (FIPS 180-4). Products can now ship with these algorithms and claim NIST compliance. And as of April 2026, many products already do.

NIST is also working on a fourth standard, FIPS 206 (draft), for FN-DSA (originally called FALCON). FN-DSA is another lattice-based signature scheme, but it uses a different mathematical structure (NTRU lattices) from ML-DSA (module lattices). The draft standard was published in late 2024. FN-DSA produces smaller signatures than ML-DSA, which makes it attractive for applications where bandwidth is limited, such as certificates in constrained network protocols. The final standard is expected in 2026 or early 2027.

In March 2025, NIST announced the selection of HQC (Hamming Quasi-Cyclic) as an additional KEM algorithm for standardization. HQC is based on error-correcting codes, a completely different mathematical foundation from ML-KEM's lattice problems. The reason for selecting a second KEM is algorithm diversity: if a breakthrough attack is discovered against lattice-based schemes (which ML-KEM relies on), organizations need an alternative that would not be affected by the same attack. HQC provides that backup.

HQC is available today in QNSQY v7.x on the Business tier in three security levels (HQC-128, HQC-192, HQC-256). Standardization as a formal FIPS document is expected to be complete by 2027.

The most visible deployment of post-quantum cryptography is in web browsers. When you visit a website over HTTPS, your browser and the web server negotiate a shared encryption key using a key exchange algorithm. Until recently, that algorithm was always ECDH (based on elliptic curve math vulnerable to quantum attacks). Now, major browsers are using ML-KEM in hybrid mode alongside ECDH.

- 
**Chrome 131+ (released November 2024):** Google enabled ML-KEM-768 in hybrid mode by default for all TLS 1.3 connections. Chrome previously used Kyber-768 (the pre-standard version) starting in Chrome 124, then switched to the final ML-KEM standard. As of early 2026, ML-KEM is active in Chrome on all platforms (Windows, macOS, Linux, Android, ChromeOS).
- 
**Firefox 135+:** Mozilla added ML-KEM support in hybrid mode. Firefox's implementation follows the same hybrid TLS approach as Chrome, combining ML-KEM with X25519 so that the connection is protected by both algorithms.
- 
**Safari:** Apple has been shipping post-quantum key exchange for iMessage since iOS 17.4 (PQ3 protocol, February 2024). Safari browser support for ML-KEM in TLS is expected in a 2026 update.

Cloudflare, one of the largest CDN and web infrastructure providers, reported in their 2025 year-in-review that a significant and growing share of TLS connections to their network used post-quantum key exchange. The majority of these came from Chrome and Firefox users on desktop and Android. This means that for a large portion of everyday web browsing, the key exchange is already quantum-resistant.

Post-quantum algorithms are being built into operating system cryptography libraries, making them available to every application on the system:

- 
**Windows 11 24H2:** Microsoft's CNG (Cryptography Next Generation) API added ML-KEM support. Applications that use CNG for TLS or key exchange can use ML-KEM without shipping their own implementation.
- 
**macOS Sequoia:** Apple's Security framework includes post-quantum primitives. Apple's CryptoKit already supports PQ3 for iMessage, and the underlying algorithms are available to third-party developers.
- 
**Linux:** The Linux kernel's crypto subsystem added ML-KEM support. OpenSSL 3.x has experimental PQC support via the OQS (Open Quantum Safe) provider. BoringSSL (used by Chrome and Android) shipped ML-KEM support in 2024.

The strongest driver of PQC adoption is government mandates. The NSA published the CNSA 2.0 (Commercial National Security Algorithm Suite) guidance in September 2022, setting firm deadlines for when National Security Systems (NSS) must transition to post-quantum algorithms:

| Use Case | CNSA 2.0 Deadline | Required Algorithm | 
|---|---|---|
| Software/firmware signing | 2025 | ML-DSA or LMS/XMSS | 
| Web servers/browsers (TLS) | 2025 | ML-KEM | 
| Networking equipment (VPN, MACsec) | 2026 | ML-KEM + ML-DSA | 
| Operating systems | 2027 | ML-KEM + ML-DSA | 
| Custom/legacy applications | 2033 | All PQC algorithms | 

These deadlines are not optional for defense contractors, intelligence agencies, or any organization handling classified information. They are also influencing the broader private sector, because many companies follow NIST and NSA guidance even when not legally required to.

The White House also issued National Security Memorandum NSM-10 (May 2022), directing federal agencies to inventory their cryptographic systems and develop migration plans. This has created a wave of "crypto discovery" projects across the U.S. federal government, where agencies are cataloging every system that uses cryptography and planning upgrades.

Moving an entire organization from classical to post-quantum cryptography is not a weekend project. It involves four phases, and most enterprises are still in the early stages:

1. 
**Discovery.** Inventorying every system, application, protocol, and library that uses cryptography. This includes TLS certificates, VPN configurations, database encryption, data encryption, code signing, SSH keys, API authentication, and more. Most large organizations have thousands of cryptographic touchpoints, many of which are undocumented.
2. 
**Planning.** Prioritizing which systems to migrate first based on the sensitivity of the data they protect and the difficulty of migration. Long-lived secrets (healthcare data, legal records, government classified) take priority. Short-lived session keys (web TLS) are lower priority because they are already being addressed by browser vendors.
3. 
**Hybrid deployment.** Running post-quantum algorithms alongside classical algorithms. This is the recommended approach because it provides quantum resistance while maintaining backward compatibility. If a PQC algorithm turns out to have an unexpected weakness, the classical algorithm still provides protection. QNSQY uses this approach by default (ML-KEM + X25519).
4. 
**Full migration.** Removing classical cryptography dependencies entirely. Very few organizations have reached this stage. Most experts recommend staying in the hybrid phase for at least several years until PQC implementations have been thoroughly battle-tested in production.

According to industry surveys and reports from consulting firms, most large enterprises are in Phase 1 (Discovery) or Phase 2 (Planning) as of early 2026. Some early adopters in finance and government are in Phase 3 (Hybrid deployment). No major organization has publicly claimed to be in Phase 4 (Full migration).

Post-quantum algorithms produce larger keys and signatures than classical algorithms. An ML-KEM-768 public key is 1,184 bytes, compared to 32 bytes for X25519. An ML-DSA-65 signature is 3,309 bytes, compared to 64 bytes for Ed25519. For data encryption (like QNSQY), this overhead is negligible because the keys and signatures are tiny compared to the file being encrypted. For protocols like TLS, where every connection involves a key exchange and certificate chain, the extra bytes add up. A TLS handshake with ML-KEM and ML-DSA certificates can add several kilobytes to the handshake, increasing connection latency on slow networks.

Cryptographic code is embedded deeply in applications, libraries, and hardware. Many systems use RSA or ECDH in places the developers never explicitly chose, because the cryptography is handled by underlying libraries. Changing the algorithm requires updating the library, testing for compatibility, updating configuration, and often modifying the application itself. For organizations with hundreds or thousands of applications, this is a multi-year project.

While OpenSSL, BoringSSL, and platform APIs are adding PQC support, the developer experience is still rough. Documentation is sparse. Testing tools are limited. Most PQC libraries are young and have not been through the years of production hardening that OpenSSL's RSA implementation has. This is improving rapidly, but as of early 2026, implementing PQC correctly still requires more expertise than implementing classical cryptography.

ML-KEM is actually faster than RSA for key exchange. An ML-KEM-768 encapsulation takes roughly 100 microseconds, compared to several milliseconds for RSA-2048. So for encryption, PQC is often a performance improvement, not a penalty. The overhead is primarily in signatures: ML-DSA signing and verification are slower than Ed25519, and the larger signatures consume more bandwidth. For most applications, this overhead is acceptable. For extremely latency-sensitive or bandwidth-constrained systems (real-time trading, IoT sensors on cellular networks), it requires careful engineering.

All of this migration effort is driven by the anticipation of future quantum computers. Where are they today?

As of early 2026, the most advanced quantum computers have roughly 1,000 to 1,500 physical qubits. IBM's Heron processor and Google's Willow chip are in this range. These are "noisy intermediate-scale quantum" (NISQ) devices. They can perform certain specialized calculations, but they cannot run Shor's algorithm at a scale that threatens any currently deployed encryption.

Breaking RSA-2048 with Shor's algorithm would require an estimated 4,000+ logical qubits, each of which requires thousands of physical qubits for error correction. The total physical qubit count needed is in the millions. Current error correction rates are improving, but the gap between where quantum hardware is now and where it needs to be for cryptographic relevance is still enormous.

Expert consensus, as reflected in surveys by the Global Risk Institute and statements from NIST, places the arrival of a cryptographically relevant quantum computer (CRQC) at 10 to 20 years from now. Some researchers are more optimistic (under 10 years), others more conservative (over 25 years). No one in the serious research community claims it is impossible. The question is "when," not "if."

The reason to migrate now, despite the computer being years away, is the "harvest now, decrypt later" threat. Data encrypted today with classical algorithms and intercepted by an adversary can be stored indefinitely and decrypted once a CRQC exists. If your data needs to stay secret for 20 years and a CRQC arrives in 15 years, you have a 5-year window where your data is exposed. The only way to close that window is to encrypt with PQC before the data is intercepted.

- 
**FIPS 206 (draft) (FN-DSA) finalization:** Expected 2026-2027. Adds a compact lattice-based signature scheme with smaller signatures than ML-DSA.
- 
**HQC standardization:** The code-based KEM selected in March 2025, expected to be formalized as a FIPS standard by 2027.
- 
**Hybrid protocol standardization:** IETF working groups are developing standards for hybrid key exchange and hybrid signatures in TLS, SSH, and other protocols. These standards will define exactly how PQC and classical algorithms should be combined.
- 
**Hardware acceleration:** Intel has announced plans for CPU instructions that accelerate lattice operations, expected in future processor generations. ARM is updating its CryptoCell IP block for mobile and IoT devices.
- 
**Additional signature schemes:** NIST's additional signature scheme evaluation (launched in 2023) is reviewing candidates for standardization. These address use cases where ML-DSA or SLH-DSA may not be optimal, such as very short signatures for constrained devices.

The most difficult frontier for post-quantum migration is the world of IoT (Internet of Things) and embedded devices. These are the smart locks, medical monitors, industrial sensors, automotive controllers, and satellite receivers that run on tiny processors with limited memory and bandwidth. Many of these devices were designed with 20-year lifespans and have firmware that is rarely updated.

ML-KEM-768 requires roughly 2,400 bytes for a public key and ciphertext combined. That is trivial for a laptop or server, but for an embedded device with 32 KB of RAM communicating over a cellular modem, those extra bytes matter. Work is underway in several areas to address this:

- 
**Lightweight ML-KEM implementations.** Optimized implementations of ML-KEM for ARM Cortex-M processors (common in IoT devices) have been demonstrated by the PQClean project and the ARM PSA Crypto API.
- 
**Hardware accelerators.** ARM is updating its CryptoCell IP block, which is embedded in billions of chips, to support lattice-based operations natively. This would make ML-KEM nearly as fast as ECDH on resource-constrained devices.
- 
**Protocol optimization.** IETF working groups are developing protocols that minimize the number of PQC key exchanges required, caching and reusing post-quantum session keys where possible to reduce bandwidth overhead.

For devices that cannot be updated to support PQC, the recommended approach is network-level protection: ensure that the communication channels to and from these devices are PQC-protected, even if the device itself still uses classical cryptography internally. This is a stopgap measure, not a permanent solution, but it buys time until the device can be replaced.

Most of the discussion about PQC migration focuses on enterprises and governments. But individuals also have data worth protecting for decades: personal photos, family documents, financial records, medical information, private journals. If you use GPG, Age, or another tool that relies solely on classical algorithms, your encrypted archives are vulnerable to future quantum attacks.

The good news is that switching to post-quantum encryption for personal files is far simpler than an enterprise migration. There is no infrastructure to audit, no legacy systems to untangle, and no compliance framework to satisfy. You download a tool like QNSQY, encrypt your files, and you are done. The free tier provides ML-KEM-512 + X25519 hybrid encryption, which is more than sufficient for personal use.

The files most worth re-encrypting are the ones with the longest sensitivity periods: family photos you want to keep private forever, personal financial records, medical information, legal documents, and private correspondence. Start with those and work outward.

1. 
**Start now.** Do not wait for quantum computers to appear. The "harvest now, decrypt later" threat means data encrypted today with classical algorithms is already at risk. Every day you wait is another day of data that could be collected for future decryption.
2. 
**Use hybrid mode.** Combine PQC with classical algorithms. This is the recommended approach from NIST, NSA, and every major security organization. If either algorithm has an undiscovered weakness, the other still provides protection. QNSQY uses hybrid mode by default.
3. 
**Prioritize long-lived secrets.** Not all data is equally sensitive. Focus first on data that needs to remain confidential for more than 10 years: medical records, legal documents, trade secrets, personal archives, government classified material.
4. 
**Inventory your cryptography.** Know where RSA, ECDH, and ECDSA are used in your systems. You cannot migrate what you have not found. Many organizations are discovering cryptographic dependencies they did not know existed.
5. 
**Test with larger key sizes.** PQC keys are larger than classical keys. Test your systems to make sure they can handle the additional bytes in certificates, key exchanges, and signatures. Most modern systems handle this fine, but legacy systems may have buffer size limits.
6. 
**Stay informed.** The PQC landscape is evolving rapidly. FIPS 206 (draft) and the HQC standard are still in progress. IETF hybrid protocol standards are being developed. New hardware acceleration is coming. Follow the NIST PQC project page and your platform vendor's security announcements for updates.

- NIST Post-Quantum Cryptography Timeline: 2016-2026
- NIST FIPS 203/204/205: The Complete Guide
- When Will Quantum Computers Break Encryption?

QNSQY implements the latest NIST post-quantum standards.

*Originally published at quantumsequrity.com.*

## Top comments (0)
