---
id: collect-261121-veille-ia/langages-dev/java-27-is-out-post-quantum-tls-1-3-key-exchange-and-g1-as-default-gc-besoft
title: "Java 27 Is Out: Post-Quantum TLS 1.3 Key Exchange and G1 as Default GC | Besoft"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["java", "default", "preview", "post-quantum", "exchange", "memory", "release", "early"]
source: https://besoft.vn/en/blog/java-27-ra-mat-mat-ma-hau-luong-tu-g1-mac-dinh
source_anchor: ""
source_lines: [1, 44]
section: "Langages_Dev"
---
Right on its six-month rhythm, Java has a new release. On September 15, 2026, Oracle announced **Java 27 (JDK 27)** with 9 enhancement proposals (JEPs). It is not a long-term support release, but it contains several changes Java teams should look at early, especially in security ahead of the quantum computing era and JVM memory performance.

## What happened?

JDK 27 is the second non-LTS release since JDK 25 (the latest LTS). According to InfoQ, five of the nine JEPs are still in preview or incubator status, meaning you need to enable flags to try them and they may still change. The other four are finalized features. Meanwhile, JDK 28 is already in early access and is expected in March 2027.

## Notable JEPs

### Security: preparing for the post-quantum era

- **JEP 527 – Post-quantum hybrid key exchange for TLS 1.3** : combines a traditional algorithm with a quantum-resistant one in the TLS handshake. The goal is to counter "harvest now, decrypt later" attacks, where an attacker stores encrypted traffic today to decrypt it in the future. This JEP builds on JEP 496, available since JDK 24.
- **JEP 538 – PEM encodings of cryptographic objects (third preview)** : a standard API for reading and writing keys and certificates in PEM format, so you no longer need to write your own conversion functions or depend on external libraries. In this preview, the`DEREncodable` interface is renamed`BinaryEncodable` .
- **JEP 536 – Redacting sensitive data in JFR** : removes sensitive information from Java Flight Recorder data inside the process, useful when sending performance recordings to third parties.

### Performance: leaner memory, unified GC

- **JEP 534 – Compact Object Headers on by default** : object headers on the heap are made smaller, reducing memory use for applications that create many small objects.
- **JEP 523 – G1 as the default garbage collector in all environments** : previously, on very resource-constrained machines the JVM could pick the Serial GC. Now G1 is a uniform default, making behavior more predictable, especially in small containers.

### Language and APIs maturing

- **JEP 531 – Lazy Constants (third preview)** : constants are initialized on demand but still optimized by the JVM like final values.
- **JEP 532 – Primitive types in patterns,** **`instanceof`** **and** **`switch`** **(fifth preview)** .
- **JEP 533 – Structured Concurrency (seventh preview)** : manages groups of concurrent tasks as a single unit, working well with virtual threads.
- **JEP 537 – Vector API (twelfth incubator)** : still waiting on Project Valhalla before being finalized.

Beyond the JDK, Oracle also introduced components of the Java Verified Portfolio such as Helidon 27 (versioned in line with OpenJDK) and JavaFX 27 with the Metal graphics pipeline on macOS.

## Why does this matter?

Post-quantum cryptography is no longer an academic topic. Financial institutions, governments and companies handling long-lived sensitive data are being advised to plan their transition early. With hybrid key exchange built into the JDK's TLS 1.3, Java applications can experiment without external libraries. Oracle also says it will bring similar capabilities to future LTS releases.

On performance, compact object headers by default can cut memory costs for Java services running on Kubernetes, where every MB of RAM translates into money.

## What to do next

1. If you run JDK 21 or 25 LTS in production, there is no rush to move to 27, but add JDK 27 to your CI to catch compatibility issues early.
2. Re-measure memory use and GC latency of your services on JDK 27, especially containers with little CPU, since the default GC may change.
3. Security teams should test post-quantum hybrid key exchange in internal environments and check compatibility with load balancers and proxies.
4. Developers can try Structured Concurrency and the PEM API in preview mode to prepare for when they are finalized.

## Conclusion

Java 27 is a "prepare for the future" release: post-quantum cryptography, better memory efficiency and a set of APIs that are steadily maturing. For most businesses, the sensible strategy remains running production on an LTS, while using releases like JDK 27 to experiment and prepare for the next LTS.
