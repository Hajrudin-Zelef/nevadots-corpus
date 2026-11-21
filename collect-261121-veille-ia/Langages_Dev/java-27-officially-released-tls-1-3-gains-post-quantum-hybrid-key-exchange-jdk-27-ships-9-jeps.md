---
id: collect-261121-veille-ia/langages-dev/java-27-officially-released-tls-1-3-gains-post-quantum-hybrid-key-exchange-jdk-27-ships-9-jeps
title: "Java 27 Officially Released: TLS 1.3 Gains Post-Quantum Hybrid Key Exchange, JDK 27 Ships 9 JEPs"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["preview", "hybrid", "final", "default", "exchange", "jeps", "java", "post-quantum"]
source: https://pbxscience.com/java-27-officially-released-tls-1-3-gains-post-quantum-hybrid-key-exchange-jdk-27-ships-9-jeps/
source_anchor: ""
source_lines: [1, 73]
section: "Langages_Dev"
---
Oracle announced the general availability of JDK 27 on September 15, 2026, making the release available to developers, enterprises and end users. JDK 27 is the reference implementation of Java SE 27 as specified by JSR 402, and shipped as build 35 after a release-candidate period during which no P1 bugs were reported.

The release delivers nine enhancements substantial enough to warrant their own JDK Enhancement Proposals (JEPs), the process OpenJDK uses to propose and track platform changes. Four of the nine are preview features and one is an incubator feature; the remaining four are final. Alongside them sit dozens of smaller non-JEP improvements and thousands of bug fixes.

### The nine JEPs at a glance

| JEP | Feature | Status | 
|---|---|---|
| 523 | Make G1 the Default Garbage Collector in All Environments | Final | 
| 527 | Post-Quantum Hybrid Key Exchange for TLS 1.3 | Final | 
| 531 | Lazy Constants | Third Preview | 
| 532 | Primitive Types in Patterns, `instanceof` and`switch` | Fifth Preview | 
| 533 | Structured Concurrency | Seventh Preview | 
| 534 | Compact Object Headers by Default | Final | 
| 536 | JFR In-Process Data Redaction | Final | 
| 537 | Vector API | Twelfth Incubator | 
| 538 | PEM Encodings of Cryptographic Objects | Third Preview | 

### Security: hybrid key exchange arrives in TLS 1.3

The headline security change is JEP 527, which implements hybrid key exchange algorithms for TLS 1.3. These combine a quantum-resistant algorithm with a traditional one, so the handshake stays secure as long as either component holds. The threat model behind it is “harvest now, decrypt later”: an adversary can capture encrypted traffic today and decrypt it once large-scale quantum computers exist.

Applications built on the `javax.net.ssl` APIs pick up the new algorithms by default, with no changes to existing code. The three hybrid named groups are `X25519MLKEM768`, `SecP256r1MLKEM768` and `SecP384r1MLKEM1024`. Of these, only `X25519MLKEM768` is placed at the head of the default named-group list, making it the highest-priority group the JDK offers.

JEP 538, in its third preview, rounds out the cryptography story with an encoding API for Privacy-Enhanced Mail (PEM). It encodes objects representing cryptographic keys, certificates and certificate revocation lists into the PEM text format and decodes them back again, reducing hand-rolled parsing errors and easing interoperability in enterprise, cloud and regulated environments.

### Language: primitive types in patterns, fifth preview

JEP 532 continues its long preview run by removing restrictions that make primitive types awkward in pattern matching, `instanceof` and `switch`. It also refines the definition of unconditional exactness and applies tighter dominance checks in `switch` constructs, letting the compiler catch a wider range of coding errors before they reach runtime.

### Performance: G1 everywhere, smaller object headers

With JEP 523, the Garbage-First collector becomes the default in all environments rather than only on server-class machines. When no collector is named on the command line, the HotSpot JVM will now always select G1. In the scenarios where the JVM previously fell back to Serial, Oracle states that throughput, latency, memory footprint and startup time should not degrade significantly.

JEP 534 makes compact object headers the default layout in HotSpot. On 64-bit architectures the object header shrinks from 96 bits to 64 bits, cutting heap size, improving deployment density and increasing data locality. For allocation-heavy workloads this is one of the more consequential defaults in the release, since it applies without any code or configuration change.

### Libraries: lazy constants, structured concurrency, vectors

JEP 531 previews an API for lazy constants — objects holding unmodifiable data whose initialization can be deferred. The JVM treats them as true constants, so they achieve the same performance as a field declared `final` while giving applications flexibility about when the work happens, which matters for startup time in cloud-native and AI-powered services.

JEP 533 brings structured concurrency to a seventh preview. It treats groups of related tasks running in different threads as a single unit of work, which reduces the familiar hazards of cancellation and shutdown such as thread leaks and cancellation delays, and improves error handling, reliability and observability of multithreaded code.

JEP 537 keeps the Vector API in incubation for a twelfth round. The API expresses vector computations that compile reliably at runtime to optimal vector instructions on supported CPU architectures, targeting data analytics, AI inference and scientific computing where scalar equivalents leave performance on the table.

### Tooling: JFR redacts sensitive data in-process

JEP 536 enhances JDK Flight Recorder to redact command-line arguments along with the initial values of environment variables and system properties. Crucially, the redaction happens before the data leaves the process, which prevents credentials and other sensitive values from leaking through diagnostic recordings shared with support teams or observability pipelines.

### Beyond the JEPs

The non-JEP changes are extensive. ML-KEM and ML-DSA private key encodings have been updated, and ML-KEM, ML-DSA, X25519 and Ed25519 all received performance work. A new `jcmd VM.security_properties` command exposes active security properties at runtime, and `jcmd` also gains Bash autocompletion.

- **Runtime and compiler:** G1 heap sizing defaults change (`MinHeapFreeRatio` and`MaxHeapFreeRatio` are adjusted so that G1 heap resizing is effectively disabled by default), plus C2 and Vector API optimization work, more efficient file and native I/O, and a new dot-product implementation using fused multiply-add.
- **Diagnostics:** thread identifiers, thread counts and process identifiers in JSON thread dumps are now emitted as JSON numbers rather than strings, which is easier for monitoring and incident-response tooling to parse. VM diagnostics can now report the number of currently open file descriptors.
- **Libraries and locale data:** CLDR locale data moves to version 48.2, with refinements to ISO-8601 formatting,`HttpServer` routing, JavaDoc behavior, source-code validation and`ServiceLoader` .

**Removals to check before upgrading.** The experimental JVM Compiler Interface (JVMCI) has been removed in JDK 27, including JVMCI code in HotSpot, the `jdk.internal.vm.ci`, `jdk.graal.compiler` and `jdk.graal.compiler.management` modules, JVMCI-specific JIT-compilation policies, and every flag containing “JVMCI” as well as `-XX:+UseGraalJIT`. The deprecated launcher options `-noclassgc`, `-noverify` and `-verifyremote`, and the `-Xverify:none` VM option, are also gone. Anyone plugging an external compiler into HotSpot should plan accordingly.

### Support window and ecosystem

Oracle describes JDK 27 as the 18th feature release delivered on time under the six-month cadence. As a short-term release, it will receive updates until March 2027, when Oracle JDK 28 supersedes it. Teams that need a longer runway should stay on the most recent long-term-support line.

Supporting products moved in step. Helidon 27 aligns with the release, advancing declarative APIs, Helidon Data JDBC, Helidon Messaging, telemetry and MicroProfile alignment. JavaFX 27 continues with the Metal rendering pipeline on macOS, richer text editing controls and accessibility work. The Oracle Java Platform Extension for Visual Studio Code continues to support the latest Oracle JDK and early-access builds.

Oracle Jipher 20, which wraps a FIPS 140-3 validated OpenSSL cryptographic module for Java applications, is now included in the Java Verified Portfolio and adds support for ML-KEM and ML-DSA, KDF and HKDF through standard Java APIs, and FFM-based OpenSSL integration from JDK 25 onward. Oracle Cloud Infrastructure is the first cloud provider to support Oracle JDK 27, and the Oracle Java SE Universal Subscription now includes the Java Verified Portfolio.

### Who built it

Oracle credits contributors working for organizations including Alibaba, Amazon, ARM, Google, IBM, ISCAS, Microsoft, NTT Data, NVIDIA, Red Hat, Rivos and SAP, along with smaller firms such as Bellsoft, Bytesoft and Loongson. Independent developers collectively contributed 16 percent of the fixes in Java 27. Feedback from FOSS projects in the OpenJDK Quality Outreach program — among them Apache Lucene, jOOQ and JUnit 5 — helped shape the early-access builds.

### Getting it

Production-ready GPL-licensed OpenJDK binaries are available from Oracle at https://jdk.java.net/27/, with builds from other vendors following shortly. The full set of non-JEP changes is documented in the JDK 27 release notes.
