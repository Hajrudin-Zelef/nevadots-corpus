---
id: collect-261121-veille-ia/langages-dev/java-27-released-what-s-new-in-jdk-27-tbskit-blog
title: "Java 27 Released: What's New in JDK 27? — tbskit Blog"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["java", "preview", "release", "final", "default", "garbage", "exchange", "production"]
source: https://www.tbskit.com/blog/news/java-27/
source_anchor: ""
source_lines: [1, 296]
section: "Langages_Dev"
---
Java 27 (JDK 27) reached general availability on **15 September 2026**, about six months after Java
26. The release ships **nine JDK Enhancement Proposals (JEPs)**, and most of them touch the layers
you rarely see but always feel in production: TLS cryptography, garbage collection, the size of
objects on the heap, concurrency, and the cryptographic APIs.

Java 27 is a **feature release (non-LTS)**, not a long-term-support release. The newest LTS remains
**Java 25**, so the decision that matters for production teams is not “is the feature list
interesting” but “does this version need to be installed now”.

This article covers what is new in Java 27, which changes are final and which are still previews, and what is worth checking before raising the version in a running project. Notes on other releases we consider significant live in the News & Releases topic.

## Java 27 at a glance

- **What it is:** the 27th feature release of the JDK, produced by the OpenJDK project against the
Java SE 27 specification.
- **Release date:** 15 September 2026 (general availability).
- **Status:** non-LTS. The newest long-term-support release is**Java 25** .
- **Contents:** nine JEPs — four final, four preview, one incubator.
- **Fastest payoff:** G1 as the default garbage collector in every environment and compact object
headers; both work without changing application code.
- **Oracle JDK 27 update window:** until around March 2027, when the cycle is picked up by JDK 28.

## The nine JEPs in Java 27

Java 27 is not about new syntax. The list below shows where the real changes are — and which ones are not yet safe to rely on in production.

| JEP | Title | Status | Main impact | 
|---|---|---|---|
| 523 | Make G1 the Default Garbage Collector in All Environments | Final | more consistent memory behaviour | 
| 527 | Post-Quantum Hybrid Key Exchange for TLS 1.3 | Final | long-term connection security | 
| 531 | Lazy Constants (Third Preview) | Preview | initialise values only when needed | 
| 532 | Primitive Types in Patterns, instanceof, and switch (Fifth Preview) | Preview | more consistent pattern matching | 
| 533 | Structured Concurrency (Seventh Preview) | Preview | managing parallel work | 
| 534 | Compact Object Headers by Default | Final | a smaller heap | 
| 536 | JFR In-Process Data Redaction | Final | sensitive data stays out of recordings | 
| 537 | Vector API (Twelfth Incubator) | Incubator | SIMD computation | 
| 538 | PEM Encodings of Cryptographic Objects (Third Preview) | Preview | encoding cryptographic objects | 

The status matters: **preview** features can still change in the next release and require the
`--enable-preview` flag when compiling and when running, while **incubator** modules have to be
added explicitly. Neither belongs in production code.

## Post-quantum hybrid key exchange for TLS 1.3

The most significant security change in Java 27 arrives with **JEP 527**. TLS 1.3 can now negotiate
a *hybrid key exchange*: a scheme that combines a classical key exchange with an algorithm designed
to withstand quantum computers — for example X25519 combined with ML-KEM.

What makes it practical is where it lands: applications that use the `javax.net.ssl` APIs — from
`HttpsURLConnection` to `HttpClient` — pick the new mechanism up without significant code changes. As
with other security updates in the platform, the benefit comes from the JDK version you run, not from
rewriting your connections.

For anyone running a public service this is not an emergency for today, but it is worth recording: TLS traffic captured now can be decrypted later if its key exchange is eventually considered weak.

## G1 becomes the default garbage collector in every environment

**JEP 523** changes a default that used to differ between environments. Previously, under certain
conditions — 32-bit architectures or resource-constrained environments, for instance — the HotSpot
JVM could still select the **Serial Garbage Collector** by default. From Java 27, a JVM that is not
given an explicit option always uses **G1**.

The benefit is not raw speed, it is **consistent behaviour**: a developer laptop, a CI container, and
a production server all run the same memory-management strategy, which makes measurements easier to
compare.

The consequence is worth knowing too: small workloads that used to run on the Serial GC will behave differently. To check what the JVM is actually using:

`java -XX:+PrintFlagsFinal -version | grep UseG1GC`
A value of `bool UseG1GC = true` means G1 is in use. If a workload turns out to do better with another
collector, that choice is still available explicitly (`-XX:+UseSerialGC`, `-XX:+UseZGC`,
`-XX:+UseParallelGC`) — and measurement is how you decide.

## Compact object headers by default

The second change that directly affects memory use comes from **JEP 534**. On 64-bit architectures,
**Compact Object Headers** is now the default configuration: the header of every object shrinks from
96 bits to 64 bits.

The effect shows up in applications with a large number of objects:

- **a smaller heap** — the same memory holds more objects;
- **better locality** — related objects sit in the same cache line more often;
- **less pressure on the garbage collector** , because there is less space to scan.

For applications holding millions of small objects — data processing, services with many sessions,
anything storing a lot of short values — the saving usually shows up in memory use straight away,
with no code changes. The caveat stands: memory behaviour has changed, so re-measure on a real
workload. If something specific misbehaves, `-XX:-UseCompactObjectHeaders` is still available to
restore the previous layout.

## Structured concurrency

**JEP 533** brings **Structured Concurrency** to its seventh preview. The idea is simple: a group of
related tasks is treated as one unit of work.

```
Task
├── Subtask A
├── Subtask B
└── Subtask C
```
When those subtasks share one scope, cancellation, error handling, and thread lifecycle become more structured: if one subtask fails or is cancelled, the others are handled with it, and no work continues without an owner.

The pattern matters most for server applications and microservices that call several services at once — not to add more threads, but to make failure behaviour predictable. Because it is still a preview, the API is not guaranteed to be stable between releases.

## Primitive types in patterns

**JEP 532** continues the pattern-matching work by bringing primitive types into `instanceof` and
`switch` constructs. The goal is to make the mental model already used for objects apply to primitive
values, so mapping and validation code does not have to switch between two styles.

This has now been through five previews, which means the design direction is fairly settled while the API details can still change. In Java 27 it remains a preview.

## Lazy constants

**JEP 531** moves **Lazy Constants** to its third preview. The idea: a value is created when it is
actually needed, not when the class is loaded.

```
Application starts
      ↓
Constant not used yet
      ↓
Nothing to initialise
      ↓
Constant accessed for the first time
      ↓
Value created once, then reused
```
Lazy constants are useful when creating a value is expensive — reading configuration, opening a
connection, preparing reference data — or when the initialisation logic needs to stay free of
ordering problems and race conditions. The feature was previously known as *Stable Values* and was
renamed as it evolved.

## JFR in-process data redaction

Java Flight Recorder (**JFR**) is the most practical way to investigate production problems — but a
recording can carry things that should never be stored. **JEP 536** adds redaction inside the JVM
process, before data reaches the recording.

Data sources that routinely contain sensitive information:

```
Command-line arguments
Environment variables
System properties
```
All three commonly hold passwords, tokens, API keys, or internal service addresses. With redaction happening inside the JVM process, the chance that such data ends up in a diagnostic file is reduced — which matters when recordings are shared with a third party or attached to a support ticket.

## Vector API

**JEP 537** brings the **Vector API** to its twelfth incubator round. It lets vector computations be
expressed explicitly and have the JVM translate them into SIMD instructions on CPUs that support
them, so several values are processed per instruction.

Workloads that typically benefit:

- data processing and analytics
- scientific and numerical computing
- machine learning and AI inference
- image and signal processing

It is still an **incubator**, so the module must be added explicitly
(`--add-modules jdk.incubator.vector`) and the API is not final.

## PEM encodings of cryptographic objects

**JEP 538** continues the API for **PEM Encodings of Cryptographic Objects** at its third preview.
PEM is the text format you meet most often around cryptographic objects:

```
Private key
Public key
Certificate
Certificate revocation list
```
This work used to depend on third-party libraries. The new API gives the platform a standard path for encoding and decoding those objects, so security code does not need an extra dependency for a straightforward job. It remains a preview.

## Java 27 and modern application development

Taken as a whole, Java 27 is not a “try the new syntax” release; it is a release that tidies up the fundamentals:

- cryptography and connection security
- garbage collection and memory use
- concurrency and managing parallel work
- computational performance
- observability and cleaner diagnostic data
- developer experience

That direction explains who modern Java is built for: server applications, cloud-native services and microservices, data processing, and AI workloads. Almost every improvement in Java 27 is felt most in long-running services holding many sessions at once.

## Should you upgrade to Java 27?

The answer depends on the kind of system you run, not on how attractive the feature list looks.

Java 27 is a good fit when:

- you are building or studying the newest Java features and want to meet the latest defaults early;
- you want to test application readiness before the next LTS arrives;
- you have workloads that may benefit from smaller object headers or hybrid TLS negotiation.

Java 27 is not the first choice when:

- your services run in production and need a long support window — **Java 25 LTS** is still the better
fit;
- you depend on preview APIs, whose behaviour can change between releases;
- your team has no test and rollback path for a runtime version change.

Before raising the version in production, check compatibility across the whole chain:

```
Framework
Library
Build tool
Application server
Database driver
Monitoring agent
CI/CD pipeline
Container image
```
Especially if the application still runs on a much older version: jumping from Java 8, 11, or 17 is usually not about the JDK, but about dependencies that are not ready.

## Checking your Java version

Once JDK 27 is installed, the fastest way to confirm which version is active:

```
java --version
javac --version
```
The first command prints the runtime in use, the second the compiler. Both matter, because on a
machine with several JDKs, `java` and `javac` can point at different installations — a common cause
of “why is this syntax not recognised”.

## Frequently asked questions

### Is Java 27 an LTS release?

No. Java 27 is a non-LTS feature release. The newest long-term-support release is Java 25, and the next LTS follows Oracle’s two-year cadence.

### How long is Oracle JDK 27 supported?

Oracle schedules updates for Oracle JDK 27 until around March 2027, after which the cycle continues with JDK 28. Java 27 is therefore a version to test and learn on, not one to install and forget.

### What is new in Java 27?

Nine JEPs: G1 as the default GC, post-quantum hybrid key exchange for TLS 1.3, compact object headers by default, JFR in-process data redaction, four preview features (lazy constants, primitive types in patterns, structured concurrency, PEM encodings), and one incubator (Vector API).

### Is G1 always the collector now?

Almost always: since Java 27 the JVM defaults to G1 in every environment, unless you select another collector explicitly.

### Can compact object headers be disabled?

Yes, with `-XX:-UseCompactObjectHeaders`. Disabling it restores the previous object header layout, and
that decision is best made from measurements.

### Are preview features safe in production?

Technically possible, practically not advisable. Preview features can change or be removed in the next
release, and the code requires `--enable-preview`, which is tied to a specific JDK version.

### Can I jump straight from Java 21 to Java 27?

For many applications, yes — but the JDK is not the only thing to check. Review dependencies, database drivers, monitoring agents, build plugins, and container images. Where possible, test in a production-like environment and have a rollback step ready before deciding.

### Where can I download JDK 27?

From Oracle, or as an OpenJDK distribution from vendors such as Eclipse Temurin, Amazon Corretto, Azul, and Microsoft Build of OpenJDK. Choose the build whose support window matches your needs, not just the one that installs fastest.

## Conclusion

Java 27 continues Java’s evolution with a clear focus on the things that decide long-term cost: connection security, memory efficiency, consistent defaults, and parallel work that is easier to reason about.

The highlights of this release:

- post-quantum hybrid key exchange for TLS 1.3 (JEP 527)
- G1 as the default garbage collector in all environments (JEP 523)
- compact object headers by default (JEP 534)
- in-process data redaction for JFR (JEP 536)
- structured concurrency, primitive types in patterns, lazy constants, and PEM encodings, all still in preview
- the Vector API, still incubating

Because it is not an LTS release, Java 27 makes most sense as a test bed: measuring framework and library compatibility, meeting the new defaults early, and preparing a calm upgrade path for when the next LTS lands. For production systems that need long support, Java 25 LTS stays the reference — while Java 27 is the safe place to learn.

Running Java workloads in production and weighing an upgrade? Send us a short description of your environment and we will help sequence the testing and the rollback step with the least risk. Related reading: our code standards, the guides we maintain, and the ops habits that keep versions under control.

## References

Sources we used while writing this article:

- OpenJDK — JDK 27
- Oracle Java Blog — The Arrival of Java 27
- OpenJDK — JEP Index
- Oracle — Java SE Documentation
