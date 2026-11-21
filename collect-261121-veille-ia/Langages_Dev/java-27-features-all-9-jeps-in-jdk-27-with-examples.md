---
id: collect-261121-veille-ia/langages-dev/java-27-features-all-9-jeps-in-jdk-27-with-examples
title: "Java 27 Features: All 9 JEPs in JDK 27 with Examples"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-09-15"]
keywords: ["java", "default", "change", "features", "jeps", "final", "preview", "release"]
source: https://howtodoinjava.com/java/java-27-features/
source_anchor: ""
source_lines: [1, 159]
section: "Langages_Dev"
---
**Java 27 features are the nine JEPs delivered in JDK 27, a non-LTS release that reached general availability on 15 September 2026.** Four of them are final and change JVM defaults, such as the garbage collector on small machines or the TLS key exchange. The other five are previews or an incubator module, so they need extra flags.

We can install JDK 27 to test our apps with the new defaults and to try the preview APIs while they can still change. For production, Java 25 remains the LTS version until Java 29 arrives in September 2027.

## 1. Is Java 27 an LTS Release? Release Date and Support

No. Java 27 reached general availability on 15 September 2026, six months after Java 26. Oracle ships an LTS (long-term support) version every two years, so the next LTS after Java 25 is Java 29 in September 2027. The Oracle Java SE Support Roadmap gives Java 27 updates only until March 2027, when Java 28 replaces it.

## 2. All Java 27 Features and Their JEP Status

JDK 27 has nine JEPs (JDK Enhancement Proposals), fewer than Java 25 with 18 and Java 26 with 10. The removals appear only in the release notes, as section 9 shows.

| JEP | Feature | Status | What it does | 
|---|---|---|---|
| 523 | G1 as the Default GC in All Environments | Final | The JVM picks G1 even with one CPU or little memory. | 
| 527 | Post-Quantum Hybrid Key Exchange for TLS 1.3 | Final | TLS 1.3 offers the quantum-safe *X25519MLKEM768* first. | 
| 534 | Compact Object Headers by Default | Final | Each object header shrinks from 96 to 64 bits. | 
| 536 | JFR In-Process Data Redaction | Final | Recordings show *[REDACTED]* in place of passwords. | 
| 531 | Lazy Constants | Third preview | Removes *isInitialized()* and*orElse()* , adds*Set.ofLazy()* . | 
| 532 | Primitive Types in Patterns, instanceof, and switch | Fifth preview | Same as JDK 26, no change. | 
| 533 | Structured Concurrency | Seventh preview | Adds an exception type parameter, and *join()* throws*ExecutionException* . | 
| 538 | PEM Encodings of Cryptographic Objects | Third preview | *PEM* becomes a class,*DEREncodable* becomes*BinaryEncodable* . | 
| 537 | Vector API | Twelfth incubator | No API change; waits for Project Valhalla. | 

## 3. G1 Becomes the Default GC on Small Machines (JEP 523)

When we don’t choose a garbage collector (GC), the JVM picks one. Up to JDK 26, it picked the Serial collector on machines with one CPU or less than 1792 MB of physical memory, and G1 elsewhere. In JDK 27, the JVM picks G1 everywhere. The change matters most in one-CPU containers.

```
java -XX:ActiveProcessorCount=1 -Xlog:gc -version
# JDK 26: [0.007s][info][gc] Using Serial
# JDK 27: [0.023s][info][gc] Using G1
```
If a small service ran better with Serial, we add *-XX:+UseSerialGC*, because a collector we name takes precedence over the default.

## 4. Compact Object Headers ‘ON’ by Default (JEP 534)

Every Java object starts with a header that the JVM uses for locking and garbage collection. In JDK 27, the header shrinks from 96 to 64 bits on 64-bit platforms, so apps with many small objects use less heap. In one JEP test, heap use fell by 22% and CPU time by 8%.

JDK 25 needed *-XX:+UseCompactObjectHeaders* (JEP 519), and JDK 27 turns them on by default. If a tool or a native library expects the old header size, we start that app with *-XX:-UseCompactObjectHeaders*.

## 5. Post-Quantum Hybrid Key Exchange in TLS 1.3 (JEP 527)

Someone can record our HTTPS traffic today and read it years later, once quantum computers can break the encryption keys we use today. Java 27 adds a new way to agree on these keys in TLS 1.3 that mixes a quantum-safe method with the current one. The connection stays safe as long as one of the two is not broken. The new method, *X25519MLKEM768*, is on by default, so our apps get it without any code change.

Java offers the server a list of key exchange methods, called named groups. In JDK 27, the new method comes first and two *ffdhe* groups are dropped.

```
JDK 26: [x25519, secp256r1, secp384r1, secp521r1, x448, ffdhe2048, ffdhe3072, ffdhe4096, ffdhe6144, ffdhe8192]
JDK 27: [X25519MLKEM768, x25519, secp256r1, secp384r1, secp521r1, x448, ffdhe2048, ffdhe3072, ffdhe4096]
```
Calls through *HttpClient* or other *javax.net.ssl* code get the new method, unless the code sets its own list with *SSLParameters.setNamedGroups()* or the *jdk.tls.namedGroups* system property. A failed connection after the upgrade is debugged like any other SSLHandshakeException.

## 6. JFR Redacts Secrets in Recordings (JEP 536)

JDK Flight Recorder (JFR) records what a running JVM does into a *.jfr* file, which we share for debugging. The file also stores the JVM command line, system properties and environment variables. Before JDK 27, a password passed as *-Ddb.password=…* appeared there in plain text. In JDK 27, JFR writes *[REDACTED]* in place of a value when its name contains words such as *password*, *secret* or *token*.

For example, we record a small app started with *-Ddb.password=secret123*, the arguments *–password secret456 –verbose* and the environment variable *DB_TOKEN*, and read the file with *jfr print*.

```
jvmArguments = "-XX:StartFlightRecording:filename=shop.jfr [REDACTED] -Dapp.name=shop"
javaArguments = "com.howtodoinjava.java27.ShopApp [REDACTED] [REDACTED] --verbose"
key = "db.password"   value = "[REDACTED]"
key = "app.name"      value = "shop"
key = "DB_TOKEN"      value = "[REDACTED]"
```
We add our own word with *-XX:FlightRecorderOptions:redact-key=+confidential*, where the *+* keeps the default words, and *redact-argument=none,redact-key=none* turns redaction off.

## 7. Trying the Java 27 Preview Features

A preview feature is complete but can still change. The *javac* and *java* commands reject it unless we pass *–enable-preview*, and the compiled classes run only on JDK 27.

```
javac --release 27 --enable-preview Main.java
java --enable-preview Main
```
A Maven build passes the same flag to the *maven-compiler-plugin* in *compilerArgs* and to Surefire in *argLine*.

### 7.1. Lazy Constants with the New *Set.ofLazy()* (JEP 531)

A lazy constant computes its value on the first *get()* call and keeps it from then on. It replaces a hand-written double-checked singleton, because the function runs at most once, even across threads, and must not return *null*.

The API started as *StableValue* in Java 25 and became *LazyConstant* in Java 26. The third preview removes *isInitialized()* and *orElse()*, and adds *Set.ofLazy()*, which checks each element only the first time we ask about it.

```
LazyConstant<String> greeting = LazyConstant.of(() -> loadGreeting());  // nothing computed yet
String first = greeting.get();                                          // "Hello", loadGreeting() runs once
String again = greeting.get();                                          // "Hello", no second call
Set<String> flags = Set.ofLazy(Set.of("dark-mode", "beta"), flag -> isEnabled(flag));
boolean dark = flags.contains("dark-mode");                             // true
boolean beta = flags.contains("beta");                                  // false
```
### 7.2. Primitive Types in Patterns, *instanceof* and *switch* (JEP 532)

JEP 532 lets switch expressions and pattern matching for instanceof use primitive types such as *int* and *byte*. The test *value instanceof byte b* is true only when the value fits in a *byte* without loss, so we skip manual range checks. The fifth preview has no changes.

```
String text = switch (status) {
  case 200 -> "OK";
  case int code when code >= 500 -> "server error " + code;   // status 503: "server error 503"
  case int code -> "client error " + code;                     // status 404: "client error 404"
};
String small = value instanceof byte b ? "byte " + b : "does not fit in a byte";  // value 300: "does not fit in a byte"
```
### 7.3. Structured Concurrency Gets an Exception Type Parameter (JEP 533)

Structured concurrency lets a method run subtasks in virtual threads and wait for them in one *StructuredTaskScope*. If one subtask fails, the scope cancels the others.

The seventh preview breaks code written for earlier previews. *StructuredTaskScope* and *Joiner* get a third type parameter for the exception that *join()* throws. The default scope from *open()* throws *ExecutionException* when a subtask fails. The joiner *awaitAll()* is removed, and *Joiner.onTimeout()* is replaced by *timeout()*.

```
try (var scope = StructuredTaskScope.open()) {
  Subtask<String> name = scope.fork(() -> findName());
  Subtask<Integer> age = scope.fork(() -> findAge());
  scope.join();                                          // ExecutionException if a subtask fails
  String profile = name.get() + " is " + age.get();      // "Lokesh is 37"
} catch (ExecutionException e) {
  Throwable cause = e.getCause();                        // IllegalStateException: age service down
}
```
### 7.4. PEM API Renames Before the Final Version (JEP 538)

With PEMEncoder and *PEMDecoder*, we write keys and certificates as the text of *.pem* files and read them back. JDK 27 renames *DEREncodable* to *BinaryEncodable* and *PEMDecoder.withFactory()* to *withFactoriesOf()*. The *PEM* record also becomes a class. The API becomes final with JEP 542 in JDK 28, so we keep it out of production code until then.

### 7.5. Vector API Without Changes (JEP 537)

The Vector API runs array loops on several values per CPU instruction. It is still an incubator module, an early API that needs *–add-modules jdk.incubator.vector*, and JDK 27 brings no change. It moves to preview only after Project Valhalla adds value classes.

## 8. Upgrading an App to Java 27

Most apps move to JDK 27 without a code change, because the four final JEPs change runtime behavior only. An upgrade from Java 25 or Java 26 still needs a few checks.

- Compare GC logs on one-CPU containers, and check that no monitoring script reads values that JFR hides as *[REDACTED]* .
- Recompile preview code, because lazy constants, structured concurrency and PEM have new method names or signatures.
- Update tools that read class files, such as Gradle, the IDE and ASM, to versions that know class file version 71, or they fail with an unsupported class file major version error.
- Remove the JVM options listed in section 9. For example, the JVM refuses to start with *-noverify* and prints “Unrecognized option”.
- Retest HTTPS calls through proxies and load balancers, because the new key exchange makes the first TLS message larger.

## 9. Release-Note Removals That Break Startup Scripts

The JDK 27 release notes list removals that have no JEP. Most of them are options deprecated in earlier releases, so a startup script that still passes them fails.

| Change | What we do | 
|---|---|
| The method *ThreadPoolExecutor.finalize()* is removed | Remove overrides that call *super.finalize()* , because they stop compiling. We should not use finalize() anyway. | 
| The *-XX:+UseGraalJIT* option and its JVMCI interface are removed | Use GraalVM itself to run the Graal JIT compiler. | 
| The options *-noverify* ,*-Xverify:none* ,*-noclassgc* and*-verifyremote* are removed | Use *-Xnoclassgc* or*-Xverify:remote* ;*-noverify* has no replacement. | 

Small additions without a JEP include TLS certificate compression (RFC 8879) and *KeyStore.getCreationInstant()*.

## 10. Conclusion

JDK 27 is a small release whose final JEPs change defaults. G1 runs everywhere, objects take less memory, HTTPS gets a quantum-safe key exchange, and JFR hides secrets, all without code changes.

The previews and the Vector API are for experiments, and preview code from Java 26 needs edits. Oracle updates Java 27 only until March 2027, so we test with it today and plan the production move for Java 29.

## 11. References

Happy Learning !!
