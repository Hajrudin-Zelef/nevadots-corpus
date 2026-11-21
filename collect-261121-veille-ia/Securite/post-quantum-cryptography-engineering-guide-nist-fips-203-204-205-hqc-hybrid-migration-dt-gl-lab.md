---
id: collect-261121-veille-ia/securite/post-quantum-cryptography-engineering-guide-nist-fips-203-204-205-hqc-hybrid-migration-dt-gl-lab
title: "Post-Quantum Cryptography Engineering Guide: NIST FIPS 203, 204, 205, HQC & Hybrid Migration | DT.gl Lab"
domain: securite
role: reference
task: article
actors: []
dates: []
keywords: ["fips", "hybrid", "classical", "post-quantum", "nist", "quantum", "migration", "x25519"]
source: https://dt.gl/privacy/post-quantum-cryptography-fips
source_anchor: ""
source_lines: [1, 103]
section: "Securite"
---
Even if cryptanalytically relevant quantum computers take years to arrive, your encrypted traffic is already on the clock. Intelligence collectors and adversaries capture classical RSA and X25519 handshakes on fiber links today so they can decrypt recorded archives once quantum hardware matures, which is why browsers, Cloudflare, Signal, and OpenSSH are shipping hybrid post-quantum key exchange now.

**1. Upgrade Key Exchange Now**

Enable hybrid `X25519MLKEM768` on TLS 1.3 proxies, OpenSSH `9.9+` servers, and `age` backups to stop Harvest-Now-Decrypt-Later immediately.

**2. Keep AES & Classical Fallbacks**

Do not deploy pure experimental lattice-only key exchange; hybrid combiners guarantee security never drops below battle-tested `X25519`.

## 1. Threat Asymmetry: Shor vs. Grover & the `NIST IR 8547` Clock

Shor's quantum period-finding algorithm breaks all integer factorization (`RSA`) and discrete-logarithm (`DH`, `ECDH`, `X25519`, `ECDSA`, `Ed25519`, `secp256k1`) assumptions once cryptanalytically relevant quantum computers (`CRQCs`) exist. Symmetric primitives (`AES-256`, `ChaCha20-Poly1305`, `SHA-384`, `SHAKE256`) face only Grover's sequential quadratic search and remain secure without replacement. Under NIST IR 8547 and CNSA 2.0 schedules, `112`-bit classical asymmetric schemes face **deprecation by 2030 and disallowance by 2035**:

1. **Key Exchange (`URGENT NOW`)** : Ephemeral TLS 1.3, OpenSSH, messaging, and`age` backups suffer**Harvest Now, Decrypt Later (`HNDL`)** archival interception today (Mosca's inequality: secrets needing`X` years of confidentiality plus`Y` years migration exceed time to`CRQC``Z` ). Deploy hybrid**`X25519MLKEM768`** immediately.
2. **Ephemeral vs. Long-Lived Signatures (`2026-2030`)** : A quantum computer in 2035 cannot spoof a verified 2026 TLS session,**but** hardware root CAs, Secure Element boot ROMs, and OS OTA signing anchors deployed today must embed hash-based (`SLH-DSA` ) or lattice (`ML-DSA` ) anchors now.
3. **Blockchain Public Keys (`STRUCTURAL`)** : Reused Bitcoin (`P2PKH` /`P2WPKH` ),`P2TR` (`Taproot` ), early`P2PK` outputs (`~1.7M+ BTC` ), and spent EVM`0x...` addresses already expose raw`secp256k1` points on-chain. Unspent fresh`bc1q` addresses keep keys shielded behind`RIPEMD160(SHA256(pk))` until spent (paired with`BIP-360``P2QRH` proposals).

#### NIST IR 8547 Transition Phasing & Deprecation Timetable

- **Phase 1 (2024-2030, Hybrid Migration Baseline)** : Organizations must catalog assets via automated Cryptographic Bill of Materials (`CBOM` ) tooling and deploy hybrid post-quantum key agreement (`X25519MLKEM768` ) across internet-facing TLS 1.3 edge proxies. Classical algorithms (`RSA-2048` ,`ECDSA P-256` ,`ECDH` ) remain permitted for general operational continuity while software stacks validate constant-time lattice primitives.
- **Phase 2 (2030-2035, Formal Deprecation Window)** : Classical public-key schemes face formal deprecation under federal information processing standards. Standalone classical key encapsulation without post-quantum augmentation is disallowed for sensitive infrastructure; classical digital signatures transition to legacy verification-only mode, mandating FIPS 204 (`ML-DSA` ) or FIPS 205 (`SLH-DSA` ) for all freshly issued certificates and code signatures.
- **Phase 3 (2035+, Complete Disallowance)** : Classical public-key algorithms are completely disallowed across federal procurement, defense supply chains (`CNSA 2.0` ), and regulated critical infrastructure. Systems must enforce pure post-quantum algorithms (`FIPS 203` ,`FIPS 204` ,`FIPS 205` , and`HQC` ).

## 2. NIST Post-Quantum Portfolio (`FIPS 203 / 204 / 205 / 206` & `HQC`)

- **`FIPS 203` (`ML-KEM`, ex-Kyber)** : Module-LWE over`R_q = Z_q[X]/(X^256+1)` (`q = 3329` ).**Final `ML-KEM-768` (`Cat. 3`) is wire-incompatible with draft `Kyber768Draft00`** because NIST bound`H(ek)` directly into Fujisaki-Okamoto shared-secret derivation (`G(m || H(ek))` ). Executes faster than`X25519` on`AVX2` /`NEON` .
- **`FIPS 204` (`ML-DSA`, ex-Dilithium)** : Module-LWE/MSIS general-purpose signatures (`q = 8,380,417` ). Defaults to**hedged (randomized) signing** against fault attacks. A pure 3-cert`ML-DSA-65` TLS chain adds`~15.8 KB` , requiring`RFC 8879` cert compression or IETF**Merkle Tree Certificates (`MTC`)** to fit cellular TCP`initcwnd` (`~14.6 KB` ).
- **`FIPS 205` (`SLH-DSA`, ex-SPHINCS+) & Draft `FIPS 206` (`FN-DSA`)** : Pure**stateless hash-only** signatures (`32-64 B` pubkeys,`7.8 KB+` sigs) immune to algebraic lattice breaks and ideal for firmware root CAs.`FN-DSA-512` (Falcon) shrinks`pk+sig` (`897 B + 666 B` ) for UDP/DNSSEC at the cost of constant-time IEEE-754 FPU arithmetic.
- **`HQC` (Hamming Quasi-Cyclic, March 2025 Round-4 Selection)** : Error-correcting linear code KEM providing mathematical diversity independent of structured lattices (`HQC-128` :`2,249 B` pk,`4,481 B` ct; tracked in DT.gl Advisories).

## 3. Master Byte-Budget & Cryptographic Comparison Table (2026)

| Algorithm & Standard | NIST Cat. | Hardness Assumption | Public Key (Bytes) | Ciphertext / Sig (Bytes) | Relative CPU & Primary Engineering Role | 
|---|---|---|---|---|---|
| **X25519***(Classical Baseline)* | 0 *(Shor broken)* | Elliptic-Curve ECDLP ( `Curve25519` ) | `32 B` | `32 B` | Baseline classical ECDH; vulnerable to Harvest-Now-Decrypt-Later | 
| **ML-KEM-512***(FIPS 203)* | Cat. 1 ( `~AES-128` ) | Module-LWE ( `k=2, q=3329` ) | `800 B` | `768 B` | Ultra-fast ( `faster than X25519` w/ AVX2); constrained IoT KEM | 
| **ML-KEM-768***(FIPS 203)* | Cat. 3 ( `~AES-192` ) | Module-LWE ( `k=3, q=3329` ) | `1,184 B` | `1,088 B` | **Global Recommended Default KEM** (`~0.04 ms` handshake cost) | 
| **ML-KEM-1024***(FIPS 203)* | Cat. 5 ( `~AES-256` ) | Module-LWE ( `k=4, q=3329` ) | `1,568 B` | `1,568 B` | CNSA 2.0 national-security profile & Signal `PQXDH` prekeys | 
| **X25519MLKEM768***(IETF Hybrid)* | Cat. 3 + Classical | Dual: `ECDLP`**AND**`Module-LWE` | `1,216 B` | `1,120 B` | **Production Standard** in TLS 1.3 (`0x11EC` ), OpenSSH &`age` | 
| **HQC-128***(NIST 5th PQC Selection)* | Cat. 1 ( `~AES-128` ) | Quasi-Cyclic Code Decoding ( `Hamming` ) | `2,249 B` | `4,481 B` | Non-lattice mathematical backup KEM if algebraic lattices weaken | 
| **Ed25519***(Classical Baseline)* | 0 *(Shor broken)* | Twisted Edwards ECDLP ( `SHA-512` ) | `32 B` | `64 B` | Compact classical signature; safe online short-term until CRQCs | 
| **ML-DSA-44***(FIPS 204)* | Cat. 2 ( `~SHA-256` ) | Module-LWE + Module-SIS ( `4x4` ) | `1,312 B` | `2,420 B` | Fast lattice signature for WebPKI leaf certs & API tokens | 
| **ML-DSA-65***(FIPS 204)* | Cat. 3 ( `~AES-192` ) | Module-LWE + Module-SIS ( `6x5` ) | `1,952 B` | `3,309 B` | **Recommended Default General-Purpose PQ Signature** | 
| **ML-DSA-87***(FIPS 204)* | Cat. 5 ( `~AES-256` ) | Module-LWE + Module-SIS ( `8x7` ) | `2,592 B` | `4,627 B` | CNSA 2.0 high-assurance code/firmware & PKI root signing | 
| **FN-DSA-512***(Draft FIPS 206)* | Cat. 1 ( `~AES-128` ) | NTRU Lattice ( `SIS` w/ FFT sampling) | `897 B` | `666 B` | Smallest PQ `pk + sig` footprint (`DNSSEC` ); tricky constant-time FPU | 
| **SLH-DSA-SHA2-128s***(FIPS 205)* | Cat. 1 ( `Hash-Only` ) | Pure `SHA-256` Stateless Merkle/FORS | `32 B` | `7,856 B` | Slow sign ( `~50-150 ms` ), fast verify;**ideal Root CA & OS Boot ROM** | 

## 4. Hybrid `X25519MLKEM768` Handshake & Key-Combiner Architecture Diagram

## 5. Production Infrastructure Configuration & Verification

### 5.1 TLS 1.3 (`X25519MLKEM768`), OpenSSH (`9.9+`) & `age` Encryption

- **TLS 1.3 (`0x11EC`)** : Enable`X25519MLKEM768:X25519:P-256` (`ssl_ecdh_curve` /`Groups` ) on Nginx, HAProxy, and Cloudflare/AWS-LC origins so clients negotiate`1,216 B` ClientHello key shares without extra RTTs. Verify your browser session via DT.gl's Encrypted Transport Echo.
- **OpenSSH (`9.9+` / `10.x`) `/etc/ssh/sshd_config`** :```
KexAlgorithms mlkem768x25519-sha256,sntrup761x25519-sha512,[email protected],curve25519-sha256
Ciphers [email protected],[email protected]
MACs [email protected],[email protected]
```
- **Long-Lived Backups (`age` `age1pq1...` vs OpenPGP `RFC 9580`)** : Encrypt object-store archives (`S3` /`B2` /`Arweave` ) with`age``mlkem768x25519` HPKE (`X-Wing` ) recipients or`RFC 9580` v6 OpenPGP composite`ML-KEM-768+X25519` keys; pair transit tunnels with PQXDH/SPQR messengers and WireGuard`Rosenpass` PSKs.

### 5.2 Constant-Time & `FIPS 203` Implementation Invariants

1. Enforce `FIPS 203` Section 7.2 modulus validation (`ByteEncode_12(ByteDecode_12(ek)) == ek` , ensuring coefficients`< 3329` ) prior to encapsulation.
2. Ban variable-time CPU integer division (`DIV` /`UDIV` ,`KyberSlash` class) via formally verified libraries (`libcrux` ,`aws-lc` ).
3. Preserve constant-time implicit Fujisaki-Okamoto rejection (`J(z || c)` ) inside`ML-KEM.Decaps` , never branching early on invalid ciphertexts.

## 6. WebPKI Bloat, Merkle Tree Certificates (`MTC`), & Blockchain Exposure

Replacing a `~550 B` classical TLS X.509 cert chain with naive `ML-DSA-65` expands handshake authentication past `20 KB` (`> 14.6 KB` TCP `initcwnd`). IETF **Merkle Tree Certificates (`MTC`)** resolve this by shipping CA Merkle roots inside browser trust stores so servers emit compact `~640-832 B` SHA-256 inclusion proofs instead of intermediate cert chains, while `RFC 8879` Brotli/Zstd compresses leaf payloads and `DNSSEC` moves oversize keys onto Encrypted DNS (`DoH`/`DoQ`).

On public blockchains, `BIP-39`/`SLIP-39` seeds and `BIP-32` `HMAC-SHA512` derivation remain quantum-resistant; **exposed `secp256k1` public keys** (`P2PK`, `Taproot` `P2TR`, and any address that signed at least one spend) face long-range Shor recovery. Park unspent cold vaults on fresh, never-reused `P2WPKH` (`bc1q`) or `P2WSH` scripts (Self-Custody Wallets) pending `BIP-360` (`P2QRH`).

## 7. `CBOM` Discovery, `SP 800-227` Combiners, & Embedded Secure Elements

1. **Generate a `CycloneDX v1.6+` `CBOM`** : Scan repositories and containers (`Semgrep` ,`Syft` ,`cbomkit` ) to detect hard-coded`RSA-2048` tokens, fixed C structs (`uint8_t pk[64]` overflowing`1,952 B``ML-DSA-65` ), and pinned legacy crates.
2. **`NIST SP 800-227` / `X-Wing` Key Combiners** : Bind raw PQ + classical shared secrets alongside public-key/ciphertext context (`SHA3-256(label || ss_pq || ss_ec || ct_ec || pk_ec)` ), then zeroize`2,400 B``dk_pq` buffers (`explicit_bzero` /`zeroize` ).
3. **Smartcard & Hardware Token Constraints** :`8-32 KB` SRAM Secure Elements (FIDO2 & Hardware Keys) require streaming`SHAKE128` NTT matrix expansion and ISO/IEC 7816-4 command chaining for multi-kilobyte`ML-DSA` /`SLH-DSA` transfers over tap NFC.

## Frequently Verified Technical Questions

### Why must key exchange (ML-KEM) be upgraded to post-quantum cryptography before digital signatures (ML-DSA)?

Primary Reference: NIST FIPS 203 (Module-Lattice-Based KEM)

### What is X25519MLKEM768 and why do browsers and OpenSSH prefer hybrid key exchange?

Primary Reference: IETF Hybrid Key Exchange in TLS 1.3

### Why did NIST select HQC in 2025 when ML-KEM (FIPS 203) was already standardized?

Primary Reference: NIST PQC Standardization Project

### Does post-quantum cryptography require doubling AES-256 or SHA-384 key sizes?

Primary Reference: NIST IR 8547 (Transition to Post-Quantum Cryptography Standards)

 **Published:**  |
**Last Verified:**  

-  `2026-11-14` : Integrated official NIST IR 8547 transition schedule, 2030 and 2035 deprecation-to-disallowance deadlines, and IETF X25519MLKEM768 deployment criteria.
-  `2026-11-11` : Initial V1 engineering specification covering NIST FIPS 203/204/205, March 2025 HQC selection, NIST IR 8547 deprecation timelines, and hybrid X25519MLKEM768 deployment.
