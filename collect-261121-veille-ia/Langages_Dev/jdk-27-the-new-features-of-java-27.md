---
id: collect-261121-veille-ia/langages-dev/jdk-27-the-new-features-of-java-27
title: "JDK 27: The new features of Java 27"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["java", "features", "data", "constants", "object", "objects", "collector", "default"]
source: https://www.infoworld.com/article/4202901/jdk-27-the-new-features-of-java-27.html
source_anchor: ""
source_lines: [1, 23]
section: "Langages_Dev"
---
Java Development Kit 27, an Oracle-driven, planned update to standard Java, is now generally available. This latest update to standard Java has features ranging from making the Garbage-First (G1) garbage collector the default collector and adding structured concurrency to simplify concurrent programming.

Overall, there are nine features listed on what is described as a reference implementation of JDK 27. The first five features include making G1 the default garbage collector, post-quantum hybrid key exchange for TLS 1.3, lazy constants, structured concurrency, and primitive types in patterns, `instanceof`, and `switch`. The next four include compact object headers, JFR (JDK Flight Recorder) in-process data redaction, a vector API, and PEM (Privacy-Enhanced Mail) encodings for cryptographic objects.

JDK 27 can be downloaded at jdk.java.net.

With JDK 27, G1 becomes the default garbage collector in all environments, rather than just in server environments. Goals are to ensure that the HotSpot JVM will always select G1 and that performance including throughput, latency, memory footprint, and startup time does not degrade significantly.

With post-quantum hybrid key exchange for TLS 1.3, the goal is to enhance the security of Java applications that require secure network communication by implementing hybrid key exchange algorithms. Such algorithms defend against future quantum computing attacks by combining a quantum-resistant algorithm with a traditional algorithm.

Lazy constants introduces an API for objects that hold unmodifiable data. Lazy constants are treated as true constants by the JVM, enabling the same performance optimizations that are enabled by declaring a field `final`. Compared to `final` fields, however, lazy constants offer greater flexibility in the timing of their initialization. This feature is in a third preview. 

The goal of primitive types in patterns, `instanceof`, and `switch` is to enhance pattern matching by allowing primitive types in all pattern contexts, and to extend `instanceof` and `switch` to work with all primitive types. This feature is in a fifth preview. 

Structured concurrency simplifies concurrent programming by introducing an API that treats groups of related tasks running in different threads as single units of work. Goals include streamlining error handling and cancellation, improving reliability, and enhancing observability. Structured concurrency is in its seventh preview.

JDK 27 makes compact object headers the default object header layout in the HotSpot JVM. Compact object headers reduce object headers from 96 bits down to 64 bits on 64-bit architectures, reducing heap size, improving deployment density, and increasing data locality.

JFR data in-process redaction allows users to redact command-line arguments and the initial values of environment variables and system properties in JFR recordings. JFR recording files may contain events that include sensitive data, such as secrets in command-line arguments, access tokens in environment variables, and passwords in system properties. This data is (optionally) redacted before it leaves the process, so that sensitive information does not leak.

The vector API introduces an API to express vector computations that reliably compile at run time to optimal vector instructions on supported CPUs, thus achieving better performance than equivalent scalar computations. This is an incubating API in its 12th incubation.

PEM encodings of cryptographic objects introduces an API for encoding objects that represent cryptographic keys, certificates, and certificate revocation lists in the PEM transport format, and for decoding from PEM back into objects. This is a preview API.
