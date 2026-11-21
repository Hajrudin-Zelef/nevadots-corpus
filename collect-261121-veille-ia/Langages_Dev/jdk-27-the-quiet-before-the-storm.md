---
id: collect-261121-veille-ia/langages-dev/jdk-27-the-quiet-before-the-storm
title: "JDK 27: The quiet before the storm"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-10-05"]
keywords: ["first", "release", "changes", "features", "valhalla", "patch", "preview", "feature"]
source: https://www.azul.com/blog/jdk-27-the-quiet-before-the-storm/
source_anchor: ""
source_lines: [1, 66]
section: "Langages_Dev"
---
JDK 27 brought mostly incremental changes, with five of its nine JEPs continuing as previews or incubators, while JDK 28 is where the first real Project Valhalla features arrive. In this post, Azul Deputy CTO Simon Ritter walks through what changed in JDK 27, what is coming in JDK 28, and why a release candidate delayed by the first Critical Security Patch Update (CSPU) is a reason to re-evaluate your JDK patch strategy.

In this post you will learn:

- What is final in JDK 27, including compact object headers by default, G1 as the default garbage collector, post-quantum hybrid key exchange for TLS 1.3, and in-process redaction in JFR
- Which JEPs continue as previews or incubators, and why most of JDK 27 is incremental
- Why the first Critical Security Patch Update delayed the first release candidate by two weeks
- What is targeted for JDK 28, including a JSON API and the first Valhalla features

First published on *InfoWorld*, Oct 5, 2026

Every September, with metronomic regularity, we have a new version of the Java platform. One small point about timing is that the first release candidate of JDK 27 was delayed by two weeks due to the release of the first Critical Security Patch Update (CSPU) for the JDK. The need for a CSPU is a direct implication of AI models, such as Anthropic’s Claude Mythos, becoming much better at identifying vulnerabilities and developing exploits. If you’re not already re-evaluating your patch strategy for the JDK, you definitely should be.

As usual, the new features in each release of Java are primarily driven by a set of JDK Enhancement Proposals (JEPs). In JDK 27, we have nine JEPs, and five of them are features that continue to be revised under the preview feature and incubator modules concept.

Let’s start with those, as the changes are intentionally minor.

JEP 537 brings the vector API back for a record-breaking twelfth incubator. This is not because the API has significant issues; it is because it is a component of the larger Project Valhalla. The API authors don’t want to finalize it until Valhalla is delivered (more on Valhalla later). As such, there are no changes from the version in JDK 26.

Structured concurrency returns for a solid seventh preview under JEP 533. This is part of the larger OpenJDK Project Loom, a set of features to improve the scalability and reliability of multi-threaded applications. Only minor changes in this release, primarily in the `Joiner` interface.

Lazy constants (JEP 531) return for a third preview. The idea behind this feature is deferred initialization and JVM trust. Lazy constants are data-carrying objects but are treated as true constants by the JVM. This enables the same performance optimizations as declaring a `field` final. Compared to `final` fields, however, lazy constants offer greater flexibility for developers in when they are initialized. There are only three small changes to the API in this release.

One feature of particular interest to me is JEP 532: primitive types in patterns, `instanceof`, and `switch`. I’ve been delivering a “Java puzzlers” session at conferences and JUGs, which has led to some interesting discussion points on how to use this feature. JDK 26 tightened some of the rules around pattern dominance in switches, but it is delivered without change in JDK 27.

The last preview feature in JDK 27 is JEP 538, PEM (Privacy-Enhanced Mail) encodings of cryptographic objects, now in its third preview. This API encodes and decodes cryptographic keys, certificates, and certificate revocation lists between objects and the widely used PEM transport format. This includes a number of minor changes to the API. One that caught my attention was the change in the primary PEM class from a normal object to a record. The reasoning for this is the inclusion of new constructors that accept Base64-encoded content in byte arrays.

Let’s move on to the remaining features, all of which are final.

JEP 534 now turns compact object headers on by default. Although in JDK 25 compact object headers were made final, they required an explicit command-line flag to enable them at run time. As a fundamental change to the JVM, they have now proven stable enough to be the default configuration. The benefit they bring is improved performance through reduced heap usage (the JEP cites a 22% reduction in heap space and an 8% reduction in CPU utilization for the SPECjbb2015 benchmark).

Another interesting change is JEP 523, which makes the G1 garbage collector (GC) the default in all situations. Since JDK 9, G1 has been the default for server-side applications, but the serial collector remained the default in resource-constrained environments (those with a single CPU or less than 1792 MB of physical memory). More recent changes to G1 allow it to perform as well as the serial collector in all environments. I suspect that, based on this change, we will soon see the serial collector deprecated and removed from OpenJDK.

An impressive-sounding feature is JEP 527, post-quantum hybrid key exchange for TLS 1.3. Post-quantum cryptography (PQC) refers to encryption and digital signature algorithms designed to remain secure against future attacks by quantum computers. Current cryptographic standards like RSA and ECC (Elliptic Curve Cryptography) rely on mathematical problems, such as factoring large numbers, that take standard computers thousands of years to solve. Quantum computers running Shor’s algorithm can solve these problems in hours or minutes. Even though quantum computers capable of this are not readily available at the moment, cybercriminals are already harvesting encrypted data, assuming they will be able to decrypt it with quantum computers in the future. This JEP integrates quantum-resistant algorithms into Java’s standard web and network security layer (TLS 1.3).

Finally, we have JEP 536, JFR (Java Flight Recorder) in-process data redaction. Java Flight Recorder is a low-overhead diagnostics framework integrated into the JVM. It writes runtime and application information as timestamped events to a recording file. JFR recordings typically include events that capture command-line arguments, environment variable values, and system properties, so you can see how the process was started and configured. These events can contain sensitive data, such as secrets in command-line arguments, access tokens in environment variables, and passwords in system properties. JFR will now redact this data before it leaves the process, so that sensitive information does not leak. Which information is redacted can be controlled via specific command-line arguments.

So, why is the title of this post “The quiet before the storm”?

Looking at the JEPs in JDK 27, it’s fairly quiet on new features, mostly incremental changes to preview features and a few small additions.

Which brings us to the next release, JDK 28. Even though JDK 27 has just been released, we already have a picture of some of the JEPs that will be included in JDK 28. At the time of writing, there are six JEPs targeted. Two of those JEPs make changes that have long been coming and are the source of much discussion.

First, we will finally get a JSON API in Java, something that has been in scope since the introduction of JEP 128, back in 2018. This parser is exceptionally strict and does not offer any lenient modes (unlike other third-party APIs like Jackson and Gson). We’ll have to wait and see what the community’s feedback is on this.

Second, there is JEP 541, which deprecates the macOS/x64 port. With Apple’s move to Arm-based M-class chips, the Intel port for Macs is going to be retired.

Then we come to the really big news in JDK 28: the first real parts of Project Valhalla. There are two JEPs for this: JEP 401, which is value objects, and JEP 539 (notice the difference in numbers), which is strict field initialization in the JVM.

I’ll wait until JDK 28 is ready for release before talking about this in detail, but it’s an exciting time for Java, with some big changes afoot. Stay tuned!

**What are the main changes in JDK 27?**

JDK 27 is a mostly incremental release: five of its nine JEPs continue as previews or incubators, including the Vector API in its twelfth incubator, structured concurrency in its seventh preview, and lazy constants in their third. The final features include compact object headers as the default (JEP 534), G1 as the default garbage collector in all environments (JEP 523), post-quantum hybrid key exchange for TLS 1.3 (JEP 527), and in-process data redaction in Java Flight Recorder (JEP 536). Azul Core includes builds for Java 27 as well as every current LTS version.

**How do compact object headers and the G1 default change JDK performance?**

JEP 534, which makes compact object headers the default in JDK 27, cites a 22% reduction in heap space and an 8% reduction in CPU utilization on the SPECjbb2015 benchmark. JEP 523 makes G1 the default garbage collector in all situations; recent G1 changes let it perform as well as the serial collector even in single-CPU or low-memory environments.

l**How should I adjust my JDK patching strategy for faster security updates?**

The first release candidate of JDK 27 was delayed by two weeks by the first Critical Security Patch Update for the JDK, and Azul’s Simon Ritter ties the need for such updates to AI models getting better at finding vulnerabilities and building exploits. Ritter advises teams that aren’t already re-evaluating their JDK patch strategy to start. Azul Core provides security-only quarterly, monthly, and out-of-cycle updates, so a security fix doesn’t have to wait for a full update.

**What is coming in JDK 28?**

Six JEPs are targeted for JDK 28 so far. They include a standard JSON API, which is strict and has no lenient modes, the deprecation of the macOS/x64 port (JEP 541), and the first Project Valhalla work: value objects (JEP 401) and strict field initialization in the JVM (JEP 539). Azul’s Simon Ritter expects JDK 28 to bring the really big news.
