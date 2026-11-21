---
id: collect-261121-veille-ia/langages-dev/java-27-changes-your-app-without-you-touching-the-code-4-defaults-to-check-before-you-upgrade
title: "Java 27 Changes Your App Without You Touching the Code: 4 Defaults to Check Before You Upgrade"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-09-15"]
keywords: ["java", "changes", "without", "code", "check", "before", "default", "upgrade"]
source: https://medium.com/@ascendrixtech/java-27-changes-your-app-without-you-touching-the-code-4-defaults-to-check-before-you-upgrade-96f4095e4fe7
source_anchor: ""
source_lines: [1, 119]
section: "Langages_Dev"
---
# Java 27 Changes Your App Without You Touching the Code: 4 Defaults to Check Before You Upgrade

Most Java release coverage focuses on new language features. Java 27, released on September 15, 2026, is different. Its most important changes don’t need any new code at all.

Of its nine JEPs, **four are final, and they take effect as soon as you run your application on JDK 27.** No flags, no preview mode. Your heap gets smaller, your garbage collector may change, your TLS connections become quantum-resistant, and your diagnostic recordings stop leaking secrets.

That’s mostly good news, but “changes without you asking” also means you should know exactly what’s different before upgrading production. Here’s a practical guide, plus a look at what’s coming in JDK 28.

### First, a note on release type

Java 27 is a **short-term release**, not a long-term support (LTS) release. Java 25 is the current LTS. Many enterprises stay on LTS versions, but testing each short-term release is the best way to avoid surprises when the next LTS arrives, since its changes will be included there.

### 1. Compact object headers are on by default (JEP 534)

Every Java object carries a small header with information the JVM needs. On 64-bit systems, that header used to be **96 bits**. Java 27 shrinks it to **64 bits by default.**

That sounds small, but applications create millions of objects, so it adds up. Typical heap usage falls by roughly **10 to 20 percent**, and objects pack more tightly in memory, which improves CPU cache efficiency.

What this means for you:

- Many services will use less memory with no code changes.
- Capacity planning and container memory limits may be worth revisiting after the upgrade.
- If you hit a compatibility problem, for example with a tool that inspects object layout directly, you can temporarily turn it off:

`java -XX:-UseCompactObjectHeaders -jar app.jar`
### 2. G1 is the default garbage collector everywhere (JEP 523)

Before Java 27, the JVM chose a garbage collector based on the environment. On machines it considered “small,” such as a container with one CPU or limited memory, it quietly fell back to the simpler **Serial** collector.

That special case is gone. **G1 is now the default in every environment.**

This matters most for **small containers and serverless functions.** G1 makes different trade-offs between throughput, latency, and memory overhead than Serial. Many apps will see more consistent pause times, but very memory-constrained services may behave differently.

Check which collector you’re actually using:

`java -XX:+PrintFlagsFinal -version | grep -E "UseG1GC|UseSerialGC|UseCompactObjectHeaders"`
If testing shows a small service really does better with Serial, choose it explicitly:

`java -XX:+UseSerialGC -XX:MaxRAMPercentage=75.0 -jar app.jar`
Being explicit is good practice anyway, because your behaviour no longer depends on how the JVM interprets the container.

### 3. Post-quantum TLS is on by default (JEP 527)

Java 27 adds **post-quantum hybrid key exchange for TLS 1.3**. It combines a classical algorithm with ML-KEM, a quantum-resistant algorithm standardized by NIST, so connections stay secure even if one of the two is broken in the future.

By default, only the **X25519MLKEM768** variant is enabled. Two others, **SecP256r1MLKEM768** and **SecP384r1MLKEM1024**, are available if you opt in through the `jdk.tls.namedGroups` system property:

`java -Djdk.tls.namedGroups="X25519MLKEM768,SecP256r1MLKEM768,x25519,secp256r1" -jar app.jar`
Why this matters: attackers can record encrypted traffic today and decrypt it once quantum computers become powerful enough. Hybrid key exchange protects against that, and Java now does it in the platform itself, without third-party libraries.

What to check: hybrid key shares are larger than classical ones. Most modern servers and proxies handle this fine, but **very old load balancers, proxies, or security appliances** in your network path may not. Test your outbound connections, especially to legacy internal systems, before rolling out.

### 4. JFR now redacts secrets automatically (JEP 536)

Java Flight Recorder (JFR) is a powerful, low-overhead diagnostic tool. But its recordings used to capture **command-line arguments, environment variables, and system properties**, which often contain passwords, API keys, and database credentials.

Sharing a JFR file with a vendor, a colleague, or a support ticket could leak secrets without anyone noticing. Java 27 now **redacts these values in recordings by default.**

Two practical notes:

- This is a strong reason to keep using JFR in production: it’s now safer to share recordings.
- It doesn’t remove the underlying problem. **Passing secrets as command-line arguments is still risky** , since they can appear in process listings and logs. A secrets manager or mounted files is still the better approach.

### The preview features worth trying

Java 27 also includes several preview features you can try with `--enable-preview`:

- **Structured Concurrency (seventh preview):** treats a group of related tasks running in different threads as one unit, making error handling and cancellation much simpler.
- **Lazy Constants (third preview):** values initialized on first use but treated as constants by the JVM for optimization. This version adds lazy sets.
- **Primitive Types in Patterns (fifth preview):** pattern matching and`switch` now work smoothly with primitive types.
- **PEM Encodings (third preview):** a standard API for reading and writing keys and certificates in the common PEM format.
- **Vector API (twelfth incubator):** high-performance vector computations for numeric work and AI inference.

For example, primitive patterns let you write clean `switch` expressions with conditions on primitive values:

```
static String classify(int status) {
    return switch (status) {
        case 200 -> "OK";
        case 404 -> "Not found";
        case int s when s >= 500 -> "Server error " + s;
        default -> "Other";
    };
}
```
Compile and run with:

```
javac --release 27 --enable-preview Status.java
java --enable-preview Status
```
### **Looking ahead to JDK 28**

JDK 28 is scheduled for March 2027. JEPs currently targeted to it include:

- **JEP 545: Faster Startup and Warmup with ZGC** , so ZGC gets and prepares memory more quickly as an application needs it.
- **JEP 546: Adaptive Heap Sizing for ZGC** , which adjusts the heap size based on available resources and the needs of neighbouring applications.
- **JEP 543: Structured Concurrency** , finalized without change after seven rounds of preview.
- **JEP 535: Shenandoah GC: Generational Mode by Default.**
- **JEP 401: Value Objects (Preview)** , a major step for Project Valhalla.

The list is still changing, so check openjdk.org/projects/jdk/28 for the latest status.

The direction is clear: Java keeps improving memory efficiency, startup time, and concurrency, the areas that matter most for cloud-native and containerized applications.

### Upgrade checklist

1. **Run your test suite on JDK 27** in CI alongside your current version.
2. **Check your GC** with the`PrintFlagsFinal` command, especially in small containers.
3. **Compare memory use** before and after. Expect a smaller heap thanks to compact headers.
4. **Test TLS connections** to all external and legacy internal systems.
5. **Review how secrets are passed** to the JVM. Even with JFR redaction, avoid command-line secrets.
6. **Set explicit JVM flags** for anything you depend on, rather than relying on defaults.

### Practical recommendation

Even if you stay on Java 25 LTS for production, add JDK 27 to your CI pipeline now. The four default changes, especially compact headers and G1 everywhere, are very likely to be in the next LTS. Testing them today means your next LTS upgrade will be routine instead of a surprise. And for memory-hungry services, the free heap savings alone may justify moving to Java 27 sooner.

*Sources: OpenJDK JDK 27 General Availability announcement (Mark Reinhold), Oracle’s “The Arrival of Java 27,” Inside.java, and InfoQ’s Java News Roundup for October 5, 2026.*
