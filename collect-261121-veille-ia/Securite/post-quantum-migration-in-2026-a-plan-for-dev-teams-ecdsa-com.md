---
id: collect-261121-veille-ia/securite/post-quantum-migration-in-2026-a-plan-for-dev-teams-ecdsa-com
title: "Post-quantum migration in 2026: a plan for dev teams — ECDSA.com"
domain: securite
role: reference
task: article
actors: []
dates: []
keywords: ["plan", "post-quantum", "draft", "keys", "after", "ecdsa", "migration", "exchange"]
source: https://ecdsa.com/learn/post-quantum-migration-2026
source_anchor: ""
source_lines: [1, 192]
section: "Securite"
---
**Short answer.** In 2026 a large part of the post-quantum work for *encryption in transit* is done by
upgrading. Chrome, Firefox, Apple's 26 releases, OpenSSL 3.5, Go 1.24+, JDK 27 and OpenSSH 10 negotiate hybrid
ML-KEM key exchange by default. Your job is to find the connections that still don't: old libraries, pinned
group lists, middleboxes, TLS 1.2, VPNs, internal services. *Signatures and certificates* are the slow part.
Public CAs cannot issue ML-DSA certificates for browsers yet, so inventory now, pilot ML-DSA in private PKI,
and design for algorithm agility. The planning dates are the ones in NIST's draft
IR 8547: 112-bit keys such as RSA-2048 deprecated after 2030, all
quantum-vulnerable public-key algorithms disallowed after 2035. NSA's CNSA 2.0 sets earlier dates for US
national security systems.

## Two problems with two clocks

A cryptographically relevant quantum computer would break RSA, finite-field Diffie-Hellman and elliptic-curve cryptography (ECDH, ECDSA, EdDSA) with Shor's algorithm. Symmetric cryptography and hashes survive with today's key sizes, AES-256 and SHA-256 or larger. Nobody can tell you when such a machine will exist. OpenSSH's project page puts the published estimates at "5-20 years, with many observers expecting them to arrive in the mid-2030s". The migration splits into two problems with very different urgency:

- **Confidentiality, which is urgent.** Encrypted traffic recorded today can be decrypted once the quantum
computer exists. This is harvest now, decrypt later. Anything that
must stay secret for more than a few years is already exposed if its key exchange is classical. The fix is
post-quantum key exchange, and it is deployable today.
- **Authentication, which has a deadline.** A forged signature only matters when someone relies on it, so the
quantum computer has to exist at that moment. But signing keys and trust anchors live for a long time.
Root certificates, firmware keys, code-signing keys and archived signed documents are trusted for decades,
and replacing them takes years.

Plan the work in that order: key exchange first, then signatures.

## The deadlines that actually exist

| Source | Status | What it says | 
|---|---|---|
| NIST IR 8547 | Initial public draft, November 2024. No final version on csrc.nist.gov as of 20 October 2026 | RSA, ECDSA, ECDH and finite-field DH at 112-bit strength (RSA-2048, DH-2048, P-224): deprecated after 2030. All quantum-vulnerable public-key algorithms, including P-256 and RSA-3072: disallowed after 2035 | 
| NSM-10 (US, 2022), quoted in IR 8547 | Policy | Mitigate "as much of the quantum risk as is feasible by 2035" | 
| CNSA 2.0 (NSA, 2022) | Requirements for US national security systems | Category 5 parameters (ML-KEM-1024, ML-DSA-87), LMS or XMSS for firmware. Web browsers, servers and cloud services: support and prefer by 2025, use exclusively by 2033. Software and firmware signing: exclusively by 2030. Networking equipment: exclusively by 2030 | 
| EU coordinated roadmap (June 2025) | Recommendation to member states | Start national plans by end of 2026, finish high-risk use cases by end of 2030, medium-risk by end of 2035 | 
| UK NCSC (March 2025) | Guidance | Discovery and plan by 2028, highest-priority migration by 2031, complete by 2035 | 

Three things to note about this table:

1. **IR 8547 is still a draft.** The 2030 and 2035 dates are what agencies and vendors plan around, but they are
not yet final NIST policy. In the draft, "deprecated" means you may still use the algorithm if you accept
the risk, and "disallowed" means you may not use it at all.
2. **The 2030 step hits 112-bit keys, above all RSA-2048.** RSA-2048 is still on many certificates and in
many token signers (two of the six endpoints in the scan below use it). P-256 and RSA-3072 are 128-bit and
keep going until 2035, but moving to them buys five years at most, because the draft disallows everything
quantum-vulnerable after 2035.
3. **CNSA 2.0's footnote matters for hybrids.** Hybrid modes are allowed, but CNSA 2.0 algorithms "will become
mandatory to select at the given date", so a classical-only configuration stops being acceptable.

## What is already post-quantum by default

These components negotiate a hybrid ML-KEM key exchange without any configuration, if both ends are recent enough:

| Component | Since | Notes | 
|---|---|---|
| Chrome | Chrome 131 (Nov 2024) | `X25519MLKEM768` , replacing the earlier Kyber draft group | 
| Firefox | 132 (Oct 2024) for TLS, 135 for HTTP/3 | Pref `security.tls.enable_kyber` , on by default | 
| Apple URLSession and Network framework | iOS, macOS and the other 26 releases | Apple Support 122756 | 
| OpenSSL | 3.5.0 (April 2025), the current LTS, supported to 2030-04-08 | Default key shares `X25519MLKEM768` and`X25519` | 
| Go `crypto/tls` | 1.24 (Feb 2025) | When `Config.CurvePreferences` is nil | 
| Java JSSE | JDK 27 (Sept 2026), JEP 527 | `X25519MLKEM768` first in the default list | 
| OpenSSH | 9.0 (sntrup761 hybrid), 10.0 ( `mlkem768x25519-sha256` default) | 10.1+ warns when a session is not post-quantum | 

The server side has moved too. A scan from OpenSSL 3.5.7 on 20 October 2026, using the script from the inventory section, found this:

Two columns tell the story. Key exchange is mixed: some services have the hybrid, some don't. The certificate columns are uniformly classical, ECDSA P-256 or RSA-2048. The same holds for SSH, where GitHub already picks the hybrid for an OpenSSH 10 client:

## Step 1: inventory

You cannot migrate what you cannot list. A useful cryptographic inventory answers, for each use of public-key cryptography: what algorithm and key size, which library and version, who controls the configuration, how long the protected data must stay confidential, and how long the key or signature must be trusted.

Start with what you can measure from outside, your TLS endpoints:

It must run with an OpenSSL 3.5 client, because older clients never offer the hybrid and every server looks classical to them. For many domains at once, the bulk domain checker reports key exchange, certificate algorithms and expiry per host.

Then go inside, where most of the work is:

- **Libraries and runtimes.** List the OpenSSL, BoringSSL, Go, JDK, .NET, Node.js and Python versions in every
image and on every host. OpenSSL 3.0 is out of upstream support and has no ML-KEM, and Ubuntu 24.04 still
ships it. That one fact explains many results.
- **Explicit configuration.** Search for group and cipher lists such as`ssl_ecdh_curve` ,`SSLOpenSSLConfCmd Groups` ,`CurvePreferences` ,`jdk.tls.namedGroups` and`KexAlgorithms` . Each one is a
place where a default upgrade will not reach.
- **Code.** Search for direct uses of RSA, ECDSA, ECDH and Ed25519 APIs, key sizes, JWT`alg` values, and
stored public keys in databases. Signatures stored in data formats are the hardest part to change later.
- **Keys and certificates with long lives.** Private CA roots, code-signing and firmware keys, document
signing, SSH CAs, HSM-held keys. Note each one's expiry date and the algorithms the HSM supports.
- **Vendors.** Load balancers, WAFs, TLS-inspecting proxies, VPN concentrators, managed databases, SaaS APIs.
Ask each vendor for their ML-KEM and ML-DSA roadmap in writing.

Record the result in a machine-readable form. CycloneDX added a Cryptography Bill of Materials (CBOM) for exactly this. It describes algorithms, keys, certificates and protocols as components of your software inventory, next to the package SBOM you may already generate.

## Step 2: finish key exchange

For each TLS termination point you control:

1. **Upgrade the TLS library.** OpenSSL 3.5, Go 1.24 or later, JDK 27, or a current BoringSSL or AWS-LC build.
2. **Remove pinned group lists** , or put`X25519MLKEM768` first in them. A hardening line copied from an old
guide is the most common reason a modern server negotiates plain X25519. The mechanics are in
X25519MLKEM768 explained.
3. **Test through the real network path.** The hybrid ClientHello is about 1.5 KB. Some firewalls, load
balancers and TLS-inspecting proxies fail on ClientHellos that span two packets. Compare a hybrid and a
classical-only handshake through the same path before you blame the server.
4. **Check both legs.** Behind a CDN or load balancer, the edge-to-origin connection is a separate TLS session.
Traffic recorded on that leg is just as harvestable.
5. **Retire TLS 1.2 where you can.** Hybrid groups are defined for TLS 1.3 only.

Then the non-web channels:

- **SSH.** OpenSSH 9.0 and later already negotiate a post-quantum hybrid when both ends support it. Old
servers, network appliances and embedded SSH stacks often don't. OpenSSH 10.1 and later clients print a
warning that names the risk: "This session may be vulnerable to 'store now, decrypt later' attacks."
- **VPNs and IPsec.** IKEv2 supports additional key exchanges (RFC 9370),
which vendors use to add ML-KEM. Check your gateway's firmware notes.
- **Application-level encryption.** Anything that wraps data keys with RSA or ECDH, such as envelope
encryption, encrypted backups, or end-to-end messaging, needs an ML-KEM or hybrid KEM path. HPKE (RFC 9180)
with a hybrid KEM is the usual building block. The HPKE playground
shows how the pieces fit together.

## Step 3: signatures and certificates

Here the ecosystem is not ready to do it for you, so the work in 2026 is preparation:

- **Public web certificates: wait, but get ready.** No browser accepts ML-DSA certificates from public CAs.
Chrome said in February 2026
that it has no immediate plan to add X.509 certificates with post-quantum keys to its root store. It is
building a separate quantum-resistant root store around Merkle Tree Certificates, being standardized in the
IETF PLANTS working group. Until that lands, the useful work is automation. A team that can rotate any
certificate in minutes will switch algorithms easily, and the shorter certificate
lifetimes push in the same direction.
- **Private PKI: pilot ML-DSA now.** Internal mTLS, service meshes, device identity, and VPN certificates are
under your control on both ends. OpenSSL 3.5, Go 1.27, Java 24+, .NET 10 and BouncyCastle all handle ML-DSA
keys and certificates. Expect a leaf certificate of about 5.5 KB with ML-DSA-65 and test what breaks: buffer
sizes, header limits, HSM support. ML-DSA for developers has working code
and the key format pitfalls.
- **Long-lived trust anchors first.** A root or firmware key created in 2026 will still be trusted after 2035.
For firmware and software update signing, CNSA 2.0 requires the stateful hash-based schemes LMS or XMSS
(SP 800-208) and expects exclusive use by 2030. Stateless
SLH-DSA (FIPS 205) is the conservative alternative when you
cannot manage signature state.
- **Signed artifacts that must stay verifiable.** Documents, archives and logs signed today with ECDSA have to be
re-protected before their signatures stop being trustworthy. Plan time-stamp renewal and archive
re-signing, the way long-term PDF validation already does for expiring
certificates.
- **Tokens and APIs.** JOSE and COSE have code points for ML-DSA (RFC 9964).
A 3.3 KB signature makes a JWT about 4.5 KB in base64url, which matters for cookies and headers. Measure
before you switch.

## Step 4: make the next change cheap

The migration will not be a single switch. Hybrid key exchange may later give way to pure ML-KEM. ML-DSA may be joined by FN-DSA, which NIST plans as FIPS 206. Composite and hybrid signatures are still being designed. Crypto agility is what keeps each of those changes from becoming another multi-year project:

- Algorithm identifiers travel with the data (an OID, a JOSE `alg` , a version byte), and verifiers dispatch on
them instead of assuming.
- Key and signature sizes are not hard-coded. Columns, buffers and protocol fields have room for kilobytes.
- Keys are generated, stored and rotated by automation. No human copies a PEM file into a config.
- Configuration lives in one place per stack, so the next group or algorithm change is one edit.

## Common mistakes

- **Testing with an old client.**`openssl s_client` from OpenSSL 3.0, or curl linked against it, never offers
the hybrid. Every server then looks classical, including ones that are already migrated.
- **Counting the CDN edge as the whole connection.** The browser-to-edge leg is often post-quantum, while the
edge-to-origin leg on the same request is not.
- **Disabling the hybrid to fix a broken middlebox.** That removes the protection from every connection. Fix
or replace the device.
- **Waiting for public ML-DSA certificates before doing anything.** The inventory, the library upgrades and
the certificate automation are all needed anyway, and they take longer than the wait.
- **Treating "deprecated after 2030" as "allowed until 2035".** Data you encrypt in 2029 with classical key
exchange is exposed for its whole confidentiality lifetime. The deadlines are for compliance, not for
safety.

## A checklist for the rest of 2026

1. Run an OpenSSL 3.5 scan of every public and internal TLS endpoint, and record group, certificate algorithm and library version.
2. Upgrade TLS libraries in base images. Remove pinned group lists. Re-scan.
3. Test the hybrid through every middlebox in the path, including the edge-to-origin leg.
4. Upgrade SSH servers to OpenSSH 9.9 or later for ML-KEM (9.0 and later already offer the sntrup761 hybrid), and network appliances where firmware exists.
5. List every long-lived signing key and trust anchor with its expiry and the algorithms its HSM supports.
6. Stand up an ML-DSA test CA and issue certificates to one internal service.
7. Automate certificate issuance and rotation everywhere it is still manual.
8. Ask your vendors, in writing, for ML-KEM and ML-DSA support dates.

## Sources

- NIST IR 8547 ipd, Transition to Post-Quantum Cryptography Standards (November 2024), Tables 2–4.
- NSA, Announcing the Commercial National Security Algorithm Suite 2.0 (CSA PP-22-1338, September 2022): algorithms and transition timeline.
- A Coordinated Implementation Roadmap for the Transition to Post-Quantum Cryptography, EU PQC Workstream, version 1.1, 11 June 2025.
- NCSC, Timelines for migration to post-quantum cryptography (March 2025).
- FIPS 203, FIPS 204, FIPS 205, SP 800-208.
- OpenSSH, Post-Quantum Cryptography, and OpenSSH release notes for 9.0, 9.9, 10.0 and 10.1.
- OpenSSL release strategy (3.5 LTS to 2030-04-08). Go 1.24 release notes, JEP 527.
- Google Security Blog, Cultivating a robust and efficient quantum-safe HTTPS (February 2026).

Scans were run on 20 October 2026 from `node:24.22.0-trixie` (OpenSSL 3.5.7, OpenSSH 10.0p2).
