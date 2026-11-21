---
id: collect-261121-veille-ia/langages-dev/java-27-upgrade-guide-g1-default-and-compact-headers-a-codercops
title: "Java 27 Upgrade Guide: G1 Default and Compact Headers â CODERCOPS"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-09-15"]
keywords: ["default", "preview", "compact", "object", "headers", "java", "small", "collector"]
source: https://blog.codercops.com/blog/java-27-g1-default-compact-headers-upgrade-guide-2026
source_anchor: ""
source_lines: [1, 91]
section: "Langages_Dev"
---
Sponsored

Java 27 shipped on September 15, 2026, and two of its changes alter how every JVM you run behaves without you touching a line of code: G1 is now the default garbage collector in all environments, and compact object headers are on by default. Both are likely to help. Both deserve a measurement before you trust them, especially in small containers.

The JEP list below comes from the OpenJDK JDK 27 page and InfoWorldâs feature rundown.

## What is in JDK 27

JDK 27 makes G1 the default garbage collector in all environments (JEP 523) and turns on compact object headers by default (JEP 534). It also finalizes post-quantum hybrid key exchange for TLS 1.3 and JFR in-process redaction, while Lazy Constants, structured concurrency and the Vector API stay in preview or incubator.

| JEP | Title | Status | 
|---|---|---|
| 523 | Make G1 the Default Garbage Collector in All Environments | Final | 
| 527 | Post-Quantum Hybrid Key Exchange for TLS 1.3 | Final | 
| 534 | Compact Object Headers by Default | Final | 
| 536 | JFR In-Process Data Redaction | Final | 
| 531 | Lazy Constants (Third Preview) | Preview | 
| 532 | Primitive Types in Patterns, instanceof, and switch (Fifth Preview) | Preview | 
| 533 | Structured Concurrency (Seventh Preview) | Preview | 
| 537 | Vector API (Twelfth Incubator) | Incubator | 
| 538 | PEM Encodings of Cryptographic Objects (Third Preview) | Preview | 

Four finals, four previews, one incubator. Most of the headlines are about the finals, and two of those are defaults that move under existing applications.

## Should you run it in production?

InfoWorld describes JDK 27 as a non-LTS release with six months of Oracle support. That puts it in the same bucket as every between-LTS release: useful for testing and for teams that upgrade every six months, a poor fit for fleets pinned to long-term support. The practical use of 27 is to find out now what your LTS upgrade will do later, because these defaults will be in it.

## G1 as the default everywhere

JEP 523 removes the last exception to G1 being the default. Before this, the JVM chose a collector based on what it detected about the machine, and a small machine (one CPU, little memory) got the serial collector. Now G1 is used everywhere.

For most services that is invisible. The case to check is small containers: a sidecar, a cron job, a function with 1 CPU and a few hundred megabytes. G1 carries more bookkeeping than serial, and the JEPâs own stated condition is that throughput, latency, footprint and startup must not degrade significantly. Your container may or may not be inside the âsignificantlyâ threshold.

Find out what you run today:

`java -XX:+PrintFlagsFinal -version | grep -E "Use(G1|Serial|Parallel|Z)GC"`
Run that inside the same container image and resource limits as production, not on your laptop. If a small service was silently on serial and G1 costs it measurable memory or startup time, pin the old behavior:

`java -XX:+UseSerialGC -jar app.jar`
That is a one-line fix and an honest one. Say in a comment why it is pinned, so the next person doesnât remove it as clutter.

## Compact object headers

JEP 534 turns on compact object headers by default. InfoWorld reports the header shrinks from 96 bits to 64 bits on 64-bit architectures. Every Java object carries a header, so a heap dominated by many small objects (think maps of small records, large collections of boxed values, parsed JSON trees) gets smaller, which also helps cache behavior. A heap of a few large arrays barely moves.

Where it can hurt is code that assumes the old layout. Libraries and agents that peek at object internals, off-heap tricks that compute object sizes by hand, and some profilersâ memory estimates are the candidates. If something misbehaves, the opt-out is a JVM flag; check the JEP 534 text for the exact spelling in your build rather than trusting a blog post, including this one.

## What to measure

Donât read a benchmark from someone elseâs workload and apply it to yours. Do a before-and-after on your own service:

1. Run your load test on your current JDK and on JDK 27, same container limits, same dataset.
2. Record resident memory, heap after full collection, p50 and p99 latency, GC pause totals and cold start time.
3. Repeat with `-XX:+UseSerialGC` on JDK 27 for any service that used to land on serial.
4. Check any agent, APM tool or bytecode library you attach. Verify each supports JDK 27 before you test.

The headline benefit is lower memory use from smaller headers, so heap size is the first metric to compare. If it drops, you can often shrink the container request, which is where the money is.

## The finals you may actually use

**Post-quantum hybrid key exchange for TLS 1.3 (JEP 527).** Hybrid key exchange combines a classical algorithm with a post-quantum one so a recorded session isnât readable later if quantum computers mature. If you terminate TLS in the JVM, this is worth understanding for compliance conversations. Check that your load balancers and clients negotiate it, since both ends have to support it.

**JFR in-process data redaction (JEP 536).** Flight Recorder events can capture sensitive values. Redaction inside the process means secrets donât land in recording files in the first place, which matters when profiles get shared with vendors.

## What is still preview

Structured Concurrency is in its seventh preview. If you read our explainer on structured concurrency, the model is the same, but you still need preview flags and the API can change. Primitive patterns and Lazy Constants are previews too. Keep them out of code you canât recompile quickly, and donât build a library on the Vector API while itâs incubating.

For the related picture on the infrastructure side, see our Kubernetes 1.37 release guide, since container limits are where GC defaults matter most. And if you are staffing a JVM migration, how to hire a Java developer covers what to look for. Our engineering services page lists the migration work we take on.

The takeaway: test JDK 27 in your smallest container first. That is where the new default collector can surprise you, and where smaller headers can pay for the whole exercise.

## Frequently asked questions

- Is Java 27 an LTS release?
- InfoWorld's JDK 27 coverage lists it as a non-LTS feature release with six months of Oracle support. If you standardize on long-term support versions, treat 27 as a preview of what the next LTS will contain and stay on your current LTS for production.
- What does making G1 the default in all environments change?
- JEP 523 makes G1 the default collector regardless of machine size. Previously the JVM picked the serial collector on machines it considered small, such as single-CPU or low-memory containers. If you rely on that, set -XX:+UseSerialGC explicitly or test G1 at your container size.
- Are compact object headers safe to enable by default?
- JEP 534 is a final feature in JDK 27, so it has finished the incubation path. It shrinks each object header, which mainly helps heaps full of small objects. Code that depends on object layout, such as unsafe memory tricks or agents that inspect headers, is what to test.
- Is Structured Concurrency final in Java 27?
- No. JEP 533 lists Structured Concurrency in its seventh preview in JDK 27. You need preview flags to use it, and the API can still change.

Sources

Sponsored

Sponsored

Sponsored
