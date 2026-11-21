---
id: collect-261121-veille-ia/langages-dev/new-features-in-java-27-baeldung
title: "New Features in Java 27 | Baeldung"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["java", "default", "code", "features", "exchange", "jeps", "four", "final"]
source: https://www.baeldung.com/java-27-new-features
source_anchor: ""
source_lines: [1, 187]
section: "Langages_Dev"
---
## 1. Overview

Oracle released version 27 of the Java platform.

Its predecessor, Java 26, brought HTTP/3 to the HTTP Client API and removed the Applet API. **Java 27 has nine JEPS: four are final, and five remain in preview or incubation.** No feature graduated from preview, and the four final JEPs are the ones **we notice at runtime, not in our code.**

In this tutorial, we’ll look at what changes without any code adjustments and what moved in the preview features.

## 2. New Defaults in the JVM Runtime

The following changes don’t require code adjustments or startup flags.

Java 25 introduced Compact Object Headers (JEP 519) as a full feature, disabled by default. Subsequent tests and reports showed it could **reduce the memory footprint and allow for faster processing and garbage collection.** Consequently, this feature is now enabled by default in Java 27.vWe can still disable it with *-XX:-UseCompactObjectHeaders*, but the old layout is planned for deprecation, so that’s a temporary workaround.

Enabling compact headers by default will have the biggest impact for applications that use many small objects, because the header is a fixed cost per object. The smaller the object, the larger the share it occupies. REST services built around DTOs and entities are typical examples.

Note that if we previously sized -Xmx or a container memory limit by measurement, those values are now too generous. Smaller headers mean the same workload holds a smaller live set. Nothing will break, but re-measuring lets us lower the limit and shrink the container. This **turns into an actual cost saving.**

### 2.2. G1 as the Default Garbage Collector Everywhere

Since Java 9, G1 has been the default garbage collector, except on machines with a single CPU or less than 1792 MB of memory, where the JVM uses the Serial GC. Java 27 removes that exception, following the G1 improvements in Java 26 (JEP 522).

In practice, this **affects small containers: CI runners, sidecars, and serverless environments.**

## 3. Security Enhancements

Two of the four final JEPs improve security, and both take effect without any change to our code.

### 3.1. Post-Quantum Hybrid Key Exchange for TLS 1.3

Java 27 combines the classical ECDHE key exchange with the quantum-resistant ML-KEM in TLS 1.3. Applications using *javax.net.ssl* negotiate this hybrid scheme automatically as soon as the peer supports it.

**The enhancement targets attackers who record encrypted traffic today and plan on waiting for quantum computers to become able to decrypt it.**

Combining the two schemes means that breaking one is not enough for an attack to succeed. However, ML-KEM is much newer than ECDHE and has seen far less cryptanalysis, so it’s added alongside the classical exchange rather than replacing it. Should it prove weak, the connection is no worse than today.

To see which scheme a connection actually negotiates, we can turn on the JSSE handshake log:

`java -Djavax.net.debug=ssl,handshake -jar app.jar`

In the logged messages, the named group is the key exchange both sides agree on. Java 27 supports three hybrid groups:

-  *X25519MLKEM768*
- *SecP256r1MLKEM768*
- *SecP384r1MLKEM1024*

A plain value such as *x25519* means the peer doesn’t support any of them.

### 3.2. JFR In-Process Data Redaction

Java 27 masks sensitive values in JFR recordings inside the JVM, before they reach the recording file. Values that would previously appear in the recording now show up as *[REDACTED]*.

This matters because **recordings capture command-line arguments, environment variables, and system properties, where passwords and tokens are typically stored.** Attaching one to a support ticket used to mean shipping those secrets along.

We should be aware that redaction is name-based. The JVM matches property, variable, and argument names against a list of glob patterns. The default settings cover names such as **password** or **token**, but we can add our own patterns with a JVM flag:

`java -XX:FlightRecorderOptions:redact-key=*credential*,*secret* -jar app.jar`

Here, ***redact-key* applies to the names of system properties and environment variables.** With the patterns above, the values of *db.credential* and *client.secret* will also be replaced in the recording, whereas a name such as *auth.key* still passes through. **Command-line arguments are handled by a separate *redact-argument* option.**

## 4. Still in Preview

Five features remain in preview or incubation. Most of them are resubmitted with minor refinements, and one has changes that break existing code.

### 4.1. Structured Concurrency

This preview brings code breaking changes in comparison to the sixth preview:

- The joiners now throw *ExecutionException* instead of*FailedException*
- *Joiner* gains a third type parameter for the exception type
- *awaitAll()* is gone
- *onTimeout()* becomes*timeout()*

Breaks that are due to the exception type will probably be the most common in practice, for example:

```
try (var scope = StructuredTaskScope.open()) {
    Subtask<User> user = scope.fork(() -> loadUser(id));
    Subtask<List<Order>> orders = scope.fork(() -> loadOrders(id));
    scope.join();
    return new Profile(user.get(), orders.get());
} catch (ExecutionException e) {
    throw new ProfileException(e.getCause());
}
```

Every catch clause written against the sixth preview has to be updated, and *ExecutionException* is checked, so the compiler will point them out.

### 4.2. Lazy Constants

**A lazy constant holds a value that is computed on the first access and then behaves like a constant.** The JVM can optimize them much like a final field, which plain lazy initialization behind a synchronized block doesn’t allow. The typical use is an expensive object we would rather not build during startup. For instance:

`private static final LazyConstant<ExpensiveParser> PARSER = LazyConstant.of(ExpensiveParser::new);`

The third preview removes *isInitialized()* and *orElse()*, two low-level methods that invite inspecting the initialization state, exactly what the abstraction is meant to hide.

In return, *Set.ofLazy()* arrives, so *List*, *Set*, and *Map* now all have lazy variants.

**This is the only new API element in this release.**

### 4.3. PEM Encodings of Cryptographic Objects

*PEM* is the text format that wraps Base64-encoded DER data between markers, and we use it for certificates, CRLs, and keys. Before this API, the JDK offered no standard way to read or write those data. We stripped the markers and decoded the Base64 by hand, or reached for a third-party library.

*PEMEncoder* and *PEMDecoder* handle both directions, and *withEncryption(char[])* and *withDecryption(char[])* cover password-protected private keys.

The third preview is mostly refinement:

- *DEREncodable* becomes*BinaryEncodable* , a name that no longer ties the interface to DER.
- *X509Certificate* and*X509CRL* now implement it.*EncryptedPrivateKeyInfo* gains methods for encrypting and decrypting keys directly.
- A new *CryptoException* reports failures, and constructors taking Base64-encoded byte arrays were added.

### 4.4. Primitive Types in Patterns, *instanceof*, and *switch*

Pattern matching and *switch* have long worked on reference types but stopped at primitives. JEP 532 closes that gap: *instanceof* and *switch* accept primitive type patterns, and a pattern matches only when the value fits into the target type without loss.

So. the following sample is valid:

```
static String describe(int value) {
    return switch (value) {
        case byte b -> "fits in a byte: " + b;
        case short s -> "fits in a short: " + s;
        default -> "needs an int: " + value;
    };
}
```

**Java 27 resubmits the feature unchanged from Java 26.** It has been in preview since Java 23.

### 4.5. Vector API

The Vector API lets us express computations that the JIT compiles into the CPU’s vector instructions.

**The API is still in incubation and unchanged from Java 26.** **The only difference is the bundled SLEEF library,** which supplies the vector math intrinsics on ARM and RISC-V and **moves from *3.6.1* to *3.9.0*.**

Twelve incubation rounds sound like a stalled feature, but the reason is a dependency. **The API waits for the value types from Project Valhalla** so that vector values can be flattened instead of allocated. JEP 401 has since been integrated into JDK 28 as a preview, so the next round may be the one that promotes the Vector API out of incubation.

## 5. Smaller Changes and Removals

Beyond the JEPs, Java 27 brings a number of smaller changes to the class library and tooling:

- *Math* and*StrictMath* gain the inverse hyperbolic functions*acosh()* ,*asinh()* , and*atanh()*
- *String.encodedLength(Charset)* returns the byte length in a given charset without building the array
- *BigDecimal.rootn(int, MathContext)* computes*n* th roots
- *DateTimeFormatter* accepts short zone offsets such as*+02*
- *HashMap.putAll()* is 66–86% faster when copying from another*HashMap*
- *jcmd VM.security_properties* prints the effective security properties of a running JVM

The *JVM Compiler Interface (JVMCI)* has been removed. It was a socket for plugging an externally built JIT compiler into HotSpot, and its main user, the Graal compiler, already left the JDK in Java 17. The GraalVM distribution and Native Image are unaffected.

## 6. Feature Summary

Java 27 spreads its nine JEPs across three maturity levels, and only the final ones are active out of the box:

| **Feature** | **JEP** | **Status** | 
| Compact Object Headers by Default | 534 | Final | 
| G1 as the Default Garbage Collector Everywhere | 523 | Final | 
| Post-Quantum Hybrid Key Exchange for TLS 1.3 | 527 | Final | 
| JFR In-Process Data Redaction | 536 | Final | 
| Lazy Constants | 531 | Preview | 
| PEM Encodings of Cryptographic Objects | 538 | Preview | 
| Primitive Types in Patterns, instanceof, and switch | 532 | Preview | 
| Structured Concurrency | 533 | Preview | 
| Vector API | 537 | Incubation | 

The preview features need *–enable-preview* at both compile and run time, and the compiler requires us to specify the release:

```
javac --enable-preview --release 27 Main.java
java --enable-preview Main
```

Note that the Vector API isn’t a preview feature but an incubating module, so it takes a different flag:

```
javac --add-modules jdk.incubator.vector Main.java
java --add-modules jdk.incubator.vector Main
```

Both kinds of API may change or disappear in the next release, meaning they belong in experiments rather than in production code.

## 7. Conclusion

Java 27 is a release we notice at runtime rather than in our code: smaller objects, one garbage collector everywhere, a quantum-resistant TLS handshake, and recordings that no longer leak credentials.

Nothing graduated from preview, so there’s no new production API to build on. But Java 29 will be the next LTS, and testing against 27 now lets us find the obsolete startup flags and the TLS handshakes that older network equipment rejects one release early, instead of all at once during the LTS upgrade.
