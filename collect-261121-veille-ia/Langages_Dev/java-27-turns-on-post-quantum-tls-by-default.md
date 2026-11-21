---
id: collect-261121-veille-ia/langages-dev/java-27-turns-on-post-quantum-tls-by-default
title: "Java 27 Turns On Post-Quantum TLS by Default"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["hybrid", "default", "handshake", "exchange", "ml-kem", "classical", "peers", "post-quantum"]
source: https://dev.to/neticslabs/java-27-turns-on-post-quantum-tls-by-default-2ccc
source_anchor: ""
source_lines: [1, 37]
section: "Langages_Dev"
---
- JDK 27, released September 15, 2026, ships JEP 527: hybrid key exchange for TLS 1.3 combining ML-KEM with classical ECDHE.
- Three schemes from RFC 10024 are supported; X25519MLKEM768 sits first in the default named-groups list.
- Applications using javax.net.ssl get the improved handshake by default with no code change.
- Backports are already scheduled: Oracle JDK 25 in October 2026, with announced plans for JDK 21, 17, 11 and 8.
- Netics' take: the quiet part is interoperability — a hybrid handshake only engages when both peers support the same named group, so the migration is a coordination problem bigger than one runtime, not a JVM flag.

Reproduction of the official OpenJDK JEP 527 page (Post-Quantum Hybrid Key Exchange for TLS 1.3, Closed/Delivered, release 27); source: https://openjdk.org/jeps/527

On September 15, JDK 27 went GA, and buried inside the release notes is a sentence that deserves more attention than the crypto roadmap section usually gets: the JDK's TLS 1.3 implementation now supports hybrid key exchange by default, combining the post-quantum ML-KEM algorithm with classical elliptic-curve Diffie-Hellman. This is JEP 527, and it is the first time a mainstream, mass-deployed runtime makes a quantum-resistant TLS handshake the preferred option rather than a provider-specific extra.

The mechanism matters as much as the timing. A hybrid scheme does not replace ECDHE with ML-KEM — it runs both, and the session key remains secure as long as either algorithm survives. That design choice is precisely what makes deployment possible today: classical peers can still negotiate classical groups, while both-hybrid peers get the quantum-resistant handshake without anyone having to trust that ML-KEM is battle-tested yet. The IETF standardized this as the hybrid key exchange framework (RFC 9954) and defined the three specific schemes in RFC 10024.

Original Netics diagram: hybrid key exchange — a TLS 1.3 handshake combining ML-KEM with ECDHE, secure as long as either algorithm survives; source: JEP 527, OpenJDK

JDK 27 supports the three RFC 10024 schemes: X25519MLKEM768, SecP256r1MLKEM768, and SecP384r1MLKEM1024. Only X25519MLKEM768 is enabled at the front of the default named-groups list — it combines the widely deployed X25519 with ML-KEM-768 and is the most interoperable choice today. The others are available via the `jdk.tls.namedGroups` system property or `SSLParameters.setNamedGroups`. In other words: the switch exists, but most estates will never touch it, and that is the point.

Here is the part most release coverage misses. A hybrid handshake is only used when **both** peers offer the same hybrid group. A JDK 27 server talking to a JDK 17 client silently falls back to classical ECDHE — no error, no log line that your monitoring will notice, just a quieter security posture than the release notes imply. The migration is therefore not "set a flag and forget it": it is an inventory of which peers support X25519MLKEM768, and a plan to move them together. The JVM did its part; the estate still has to coordinate.

Original Netics diagram: the three RFC 10024 hybrid schemes supported in JDK 27 and which one is first in the default list; source: JEP 527, OpenJDK

Java's quiet strength is that it sits underneath enterprise workloads nobody markets. Every bank, insurer, and government agency on this blog's reading list runs Java-based services — and that is exactly the population the "harvest now, decrypt later" threat targets: traffic recorded today, decrypted years from now when a quantum computer exists. The harvest-now problem is time-sensitive by construction, which is why a default-on mitigation in a widely deployed runtime matters more than a roadmap slide from a crypto vendor.

The backport plan is the multiplier. The JDK 27 security-enhancements post by Sean Mullan (September 15, listed in Sources) confirms the feature lands in the Oracle JDK 25 update release in October 2026, and it cites the Java Cryptographic Roadmap's plans for the long-term-support lines — JDK 21, 17, 11 and 8. That is not a future-state promise; that is a compatibility baseline forming within the next two release cycles. For a French or Moroccan enterprise still running JDK 11 or 17, the relevant question is not "when do we adopt Java 27" but "when does our vendor's TLS stack negotiate X25519MLKEM768 with us" — and the answer is now on a published calendar.

Reproduction of the official JDK 27 release notes section on post-quantum hybrid key exchange for TLS 1.3, released September 15, 2026; source: https://jdk.java.net/27/release-notes

The surrounding release quietly reinforces the direction. JDK 27 also upgrades ML-KEM and ML-DSA private key encodings (with a seed-based default that older JDKs will not read without the new compatibility properties), adds significant performance work on ML-KEM, ML-DSA, X25519 and Ed25519, and exposes active security properties through `jcmd VM.security_properties`. Taken together, the platform is not just flipping a handshake default — it is normalizing post-quantum crypto operations as an everyday runtime concern.

Three moves make sense this quarter. First, inventory: check which of your peers (load balancers, CDNs, databases, partner APIs) advertise X25519MLKEM768, and flag anything still forcing classical groups. Second, verify on one JDK 27 staging node that your `javax.net.ssl` traffic actually negotiates the hybrid group — the setting may be default-on, but the negotiation depends on the peer. Third, put the October JDK 25 backport on the calendar as the real migration milestone, because LTS adoption is where the enterprise handshake actually happens.

Original Netics diagram: JEP 527 key facts — GA date, default group, no-code benefit, and the backport schedule; source: JEP 527 and the JDK 27 release notes

Post-quantum readiness in 2026 is less about exotic hardware and more about baseline hygiene: knowing which cryptographic agreements your estate negotiates and updating them on a schedule. Java 27 just made that concrete for the largest runtime population on earth — but only if both ends of the connection update together. That coordination problem is exactly what Netics helps teams map: which TLS groups negotiate today, what the backport calendar looks like, and where the quiet fallbacks are. Start from the Netics homepage for a practical review of your TLS estate.

*Originally published on the Netics blog.*

## Top comments (0)
