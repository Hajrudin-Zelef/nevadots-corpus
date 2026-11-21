---
id: collect-261121-veille-ia/langages-dev/consolidated-jdk-27-release-notes
title: "Consolidated JDK 27 Release Notes"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-09-15"]
keywords: ["security", "java", "version", "release", "critical", "patch", "update", "service"]
source: https://www.oracle.com/java/technologies/javase/27all-relnotes.html
source_anchor: ""
source_lines: [1, 585]
section: "Langages_Dev"
---
November 17, 2026

The full version string for this update release is 27.0.1.1+4 (where "+" means "build"). The version number is 27.0.1.1. This JDK conforms to version 27 of the Java SE Specification (JSR 402 2026-09-15).


JDK 27.0.1.1 contains IANA time zone data 2026e.

For more information, refer to Timezone Data Versions in the JRE Software.


The security baselines for the Java Runtime Environment (JRE) at the time of the release of JDK 27.0.1.1 are specified in the following table:

| **JRE Family Version** | **JRE Security Baseline (Full Version String)** | 
|---|---|
| 27 | 27.0.1.1+4 | 
| 25 | 25.0.5.1+4 | 
| 21 | 21.0.13.1+4 | 
| 17 | 17.0.21.1+4 | 
| 11 | 11.0.33.1+4 | 
| 8 | 1.8.0_513-b04 | 


Oracle recommends that the JDK is updated with each Critical Patch Update. In order to determine if a release is the latest, the Security Baseline page can be used to determine which is the latest version for each release family.

Critical patch updates, which contain security vulnerability fixes, are announced one year in advance on Critical Patch Updates, Security Alerts and Bulletins. It is not recommended that this JDK (version 27.0.1.1) be used after the next critical patch update scheduled for January 19, 2027.

Java Management Service, available to all users, can help you find vulnerable Java versions in your systems. Java SE Subscribers and customers running in Oracle Cloud can use Java Management Service to update Java Runtimes and to do further security reviews like identifying potentially vulnerable third party libraries used by your Java programs. Existing Java Management Service user click here to log in to your dashboard. The Java Management Service Documentation provides a list of features available to everyone and those available only to customers. Learn more about using Java Management Service to monitor and secure your Java Installations.



A cross-realm transited field check has been enabled for Kerberos service tickets that do not have the `TRANSIT_POLICY_CHECKED` flag set. The check uses the explicit paths configured in the `[capaths]` section of `krb5.conf`, or if that is not configured, uses an implicit hierarchical relationship between realm names.

To disable the check, set the `sun.security.krb5.acceptor.checkTransitedPolicy` security property to `false`. Setting the system property of the same name supersedes the security property value. Disable this check only after carefully considering the security implications.


This release contains fixes for security vulnerabilities described in the Oracle Critical Patch Update.

Release date: October 24, 2026

The full version string for this update release is 27.0.1.0.3+1 (where "+" means "build"). The version number is 27.0.1.0.3. This JDK conforms to version 27 of the Java SE Specification (JSR 402 2026-09-15).


JDK 27.0.1.0.3 contains IANA time zone data 2026e which contains the following changes:


The security baselines for the Java Runtime Environment (JRE) at the time of the release of JDK 27.0.1.0.3 are specified in the following table:

| **JRE Family Version** | **JRE Security Baseline (Full Version String)** | 
|---|---|
| 27 | 27.0.1.0.3+1 | 
| 25 | 25.0.5.0.3+1 | 
| 21 | 21.0.13.0.3+1 | 
| 17 | 17.0.21.0.3+1 | 
| 11 | 11.0.33.0.3+1 | 
| 8 | 1.8.0_511-b33 | 


Critical patch updates, which contain security vulnerability fixes, are announced one year in advance on Critical Patch Updates, Security Alerts and Bulletins. It is not recommended that this JDK (version 27.0.1.0.3) be used after the next critical patch update scheduled for November 17, 2026.




The following root certificates have been removed from the JDK `cacerts` keystore:

```
+ alias name "emsigneccrootcag3 [jdk]"
  Distinguished Name: CN=emSign ECC Root CA - G3, O=eMudhra Technologies Limited, OU=emSign PKI, C=IN
+ alias name "emsignrootcag1 [jdk]"
  Distinguished Name: CN=emSign Root CA - G1, O=eMudhra Technologies Limited, OU=emSign PKI, C=IN
+ alias name "emsignrootcag2 [jdk]"
  Distinguished Name: CN=emSign Root CA - G2, O=eMudhra Technologies Limited, OU=emSign PKI, C=IN
+ alias name "comodoaaaca [jdk]"
  Distinguished Name: CN=AAA Certificate Services, O=Comodo CA Limited, L=Salford, ST=Greater Manchester, C=GB
```

This release contains the fix for the security vulnerability described in the Oracle Security Alert Advisory - CVE-2026-114820.

Release date: October 21, 2026

The full version string for this update release is 27.0.1.0.2+1 (where "+" means "build"). The version number is 27.0.1.0.2. This JDK conforms to version 27 of the Java SE Specification (JSR 402 2026-09-15).


JDK 27.0.1.0.2 contains IANA time zone data 2026e which contains the following changes:


The security baselines for the Java Runtime Environment (JRE) at the time of the release of JDK 27.0.1.0.2 are specified in the following table:

| **JRE Family Version** | **JRE Security Baseline (Full Version String)** | 
|---|---|
| 27 | 27.0.1.0.2+1 | 
| 25 | 25.0.5.0.2+1 | 
| 21 | 21.0.13.0.2+1 | 
| 17 | 17.0.21.0.2+1 | 
| 11 | 11.0.33.0.2+1 | 
| 8 | 1.8.0_511-b32 | 


Critical patch updates, which contain security vulnerability fixes, are announced one year in advance on Critical Patch Updates, Security Alerts and Bulletins. It is not recommended that this JDK (version 27.0.1.0.2) be used after the next critical patch update scheduled for November 17, 2026.



The JDK has stopped trusting the eMudhra intermediate CA certificates that have been compromised. For more details on this issue, see https://emudhra.we-have-a-problem.com/. The distrust includes 8 eMudhra intermediate CA certificates and 8 certificates cross-signed by Sectigo.

The certificates have been blocked by adding them to the `lib/security/blocked.certs` file.

For TLS applications, a TLS session will not be negotiated if the server's certificate chain contains one of the compromised certificates.

For signed code, verifying a signed JAR with the command `jarsigner -verify -strict $SIGNED_JAR` will fail if the JAR was signed with a certificate chain containing one of the compromised certificates.


This release also contains the fix for the security vulnerability described in the Oracle Security Alert Advisory - CVE-2026-91403.

➜ Issues fixed in 27.0.1.0.2:
| # | JBS | Component/Subcomponent | Summary | 
|---|---|---|---|
| 1 | JDK-8392377 | core-libs/java.time | (tz) Update Timezone Data to 2026d | 
| 2 | JDK-8392519 | core-libs/java.util:i18n | Cherry-pick metazone changes from CLDR for tzdata2026d, for JDK update release | 
| 3 | JDK-8392526 | core-libs/java.util:i18n | Cherry-pick BCP 47 time zone changes from CLDR for tzdata2026d, for JDK update releases | 
| 4 | JDK-8393337 | core-libs/java.time | (tz) Update Timezone Data to 2026e | 
| 5 | JDK-8393344 | core-libs/java.util:i18n | Cherry-pick metazone changes from CLDR to support tzdata2026e for JDK update releases | 
| 6 | JDK-8392538 | core-libs/java.util:i18n | Align the JDK with Microsoft's new timezone names for British Columbia/Alberta | 
| 7 | JDK-8393848 | core-libs/java.util:i18n | Align the JDK with Microsoft's new timezone name for Manitoba | 

The following sections summarize changes made in all Java SE 27.0.1 BPR releases. The BPR releases are listed below in date order, most current BPR first. Note that bug fixes in previous BPRs are also included in the current BPR.


The 27.0.1.0.1 BPR is available to all users here.

| BugId | Category | Subcategory | Summary | 
|---|---|---|---|
| JDK-8392377 | core-libs | java.time | (tz) Update Timezone Data to 2026d | 
| JDK-8392519 | core-libs | java.util:i18n | Cherry-pick metazone changes from CLDR for tzdata2026d, for JDK update release | 
| JDK-8392526 | core-libs | java.util:i18n | Cherry-pick BCP 47 time zone changes from CLDR for tzdata2026d, for JDK update releases | 
| JDK-8393337 | core-libs | java.time | (tz) Update Timezone Data to 2026e | 
| JDK-8393344 | core-libs | java.util:i18n | Cherry-pick metazone changes from CLDR to support tzdata2026e for JDK update releases | 
| JDK-8392538 | core-libs | java.util:i18n | Align the JDK with Microsoft's new timezone names for British Columbia/Alberta | 
| JDK-8393848 | core-libs | java.util:i18n | Align the JDK with Microsoft's new timezone name for Manitoba | 

October 20, 2026

The full version string for this update release is 27.0.1+9 (where "+" means "build"). The version number is 27.0.1. This JDK conforms to version 27 of the Java SE Specification (JSR 402 2026-09-15).


JDK 27.0.1 contains IANA time zone data 2026c which contains the following changes:


The security baselines for the Java Runtime Environment (JRE) at the time of the release of JDK 27.0.1 are specified in the following table:

| **JRE Family Version** | **JRE Security Baseline (Full Version String)** | 
|---|---|
| 27 | 27.0.1+9 | 
| 25 | 25.0.5+9 | 
| 21 | 21.0.13+10 | 
| 17 | 17.0.21+9 | 
| 11 | 11.0.33+9 | 
| 8 | 1.8.0_511-b09 | 


Critical patch updates, which contain security vulnerability fixes, are announced one year in advance on Critical Patch Updates, Security Alerts and Bulletins. It is not recommended that this JDK (version 27.0.1) be used after the next critical patch update scheduled for November 17, 2026.


The `com.sun.net.http.HttpServer` has been updated to validate the `Host` request header as described in RFC 9112. HTTP/1.1 requests without a `Host` header or with multiple `Host` headers are now rejected with an HTTP `400 Bad Request` response.

The validation can be disabled by setting the system property `sun.net.httpserver.disableHostValidation` to `true`.

The implementation of the WebRowSet.readXml methods has been updated to consistently report XML parsing and validation failures as SQLException.

`WebRowSet.readXml` validates XML input against the standard WebRowSet XML schema. Applications should expect malformed XML, schema validation failures, unsupported external entities, and other XML processing errors to be reported as `SQLException`. Validation can be disabled by setting the system property `jdk.sql.rowset.webrowsetValidation` to `false`.

The existing method signatures remain unchanged. No specification changes are included in this update.

The scope of the `com.sun.jndi.ldap.object.trustSerialData` system property is extended to cover transparent processing of URL RMI References obtained from an LDAP context.

A URL context factory is a special type of `ObjectFactory` that will be used to look up an object referred to from a JNDI `javax.naming.Reference` that does not specify any factory name, but contains a simple reference address in the form of a URL string. When deserialization of Java objects from LDAP is disabled, the JDK built-in implementation of the JNDI/LDAP provider will no longer transparently use any URL context factory when processing `Reference` objects containing a URL reference whose scheme is `rmi`.

To make it possible to have a finer-grain control over which URL schemes should be disabled when processing URL string reference addresses from the built-in JNDI/LDAP provider, a new system property, `jdk.jndi.ldap.disabledURLSchemes`, is introduced. The value of this property is a comma-separated list of URL schemes that are disabled. The JDK built-in implementation of the JNDI/LDAP provider will no longer transparently use any URL context factory when processing `Reference` objects containing a URL reference whose scheme corresponds (case insensitively) to one of the schemes in this list of disabled schemes. The default value for that property is "rmi" if `trustSerialData` is `false` (the default), and empty if `trustSerialData` is `true`. If a custom value for this new property is provided on the Java command line then only schemes listed in that property value are disabled, regardless of the value of `trustSerialData`.

If `rmi` is included in this list (the default when no custom values have been supplied for `com.sun.jndi.ldap.object.trustSerialData` or `jdk.jndi.ldap.disabledURLSchemes`), then a `javax.naming.Reference` object will be returned to the application instead of the object that would have been created by processing the RMI reference and looking up its URL from the context returned by a URL context factory.

An application that depends on following RMI URLs contained in a `Reference` obtained from a JNDI/LDAP context may need to process the `Reference` object explicitly, or provide a custom value for the `jdk.jndi.ldap.disabledURLSchemes` system property that does not include the `rmi` scheme.

An application that wants to prevent transparent processing of other kinds of URL schemes can now do so by supplying a custom value for the `jdk.jndi.ldap.disabledURLSchemes` system property that also includes them.

The following root certificate has been removed from the `cacerts` keystore:

```
+ alias name "luxtrustglobalrootca [jdk]"
  Distinguished Name: CN=LuxTrust Global Root, O=LuxTrust s.a., C=LU
```

The way JShell's remote agents are invoked has changed. Existing agents will need to be updated to use the new protocol. See the `jdk.jshell.execution.RemoteExecutionControl` JavaDoc API documentation for the new protocol.

Deserialization of the `com.sun.org.apache.xalan.internal.xsltc.trax.TemplatesImpl` class is now disabled by default. (This is an internal class that implements the `javax.xml.transform.Templates` interface.) An application attempting to deserialize an instance of this class will receive `UnsupportedOperationException`. To re-enable deserialization of this class, set the system property `jdk.xml.enableTemplatesImplDeserialization` to `true`. To take effect, this property must be set on the command line, in the `jaxp.properties` file, or in a user-defined JAXP configuration file.


| # | JBS | Component/Subcomponent | Summary | 
|---|---|---|---|
| 1 | JDK-8390380 | core-libs/java.util:i18n | Cherry-pick metazone changes from CLDR to support tzdata2026c for JDK update releases | 
| 2 | JDK-8388080 | hotspot/compiler | Increase static size of some stubs for APX code | 
| 3 | JDK-8386852 | hotspot/compiler | Lower peak throughput with AOTCache | 
| 4 | JDK-8385454 | hotspot/gc | Provide more NUMA related information in hsinfo/hserr files | 
| 5 | JDK-8385817 | tools/jconsole | Headless jdk still contains bin/jconsole | 

Publication Date: 15 September 2026

The following sections are included in these Release Notes:

These notes describe important changes, enhancements, removed APIs and features, deprecated APIs and features, and other information about JDK 27. In some cases, the descriptions provide links to additional detailed information about an issue or a change. This page does not duplicate the descriptions provided by the Java SE 27 ( JSR 402) Platform Specification, which provides informative background for all specification changes and might also include the identification of removed or deprecated APIs and features not described here. The Java SE 27 ( JSR 402) specification provides links to:

**Annex 1:** The complete
Java SE 27 API Specification.

**Annex 2:** An
annotated API specification
showing the exact differences between Java SE 26 and Java SE 27. Informative background for these changes may be
found in the list of approved Change Specification Requests for this release.

**Annex 3:** Java SE 27 Editions of
The Java Language Specification
and
The Java Virtual Machine Specification.
The Java SE 27 Editions contain all corrections and clarifications made since the Java SE 26 Editions, as well
as additions for new features.

You should be aware of the content in the Java SE 27 ( JSR 402) specification as well as the items described in this page.

The descriptions on this Release Notes page also identify potential compatibility issues that you might encounter when migrating to JDK 27. The Kinds of Compatibility page on the OpenJDK wiki identifies the following three types of potential compatibility issues for Java programs that might be used in these release notes:

**Source:** Source compatibility preserves the ability to compile existing source code without error.

**Binary:** Binary compatibility is defined in The Java Language Specification as preserving the ability to link
existing class files without error.

**Behavioral:** Behavioral compatibility includes the semantics of the code that is executed at runtime.

See CSRs Approved for JDK 27 for the list of CSRs closed in JDK 27 and the Compatibility & Specification Review (CSR) page on the OpenJDK wiki for general information about compatibility.

The full version string for this release is build 27+35 (where "+" means "build"). The version number is 27.

**IANA Data 2026b**

JDK 27 contains IANA time zone data version 2026b. For more information, refer to Timezone Data Versions in Java Runtimes.



**➜ Primitive Types in Patterns, instanceof, and switch (Fifth Preview)**


Enhance pattern matching by allowing primitive types in all pattern contexts,
and extend `instanceof` and `switch` to work with all primitive types. This is a
preview language feature.


**➜ Lazy Constants (Third Preview)**


Introduce an API for *lazy constants*, which are objects that hold unmodifiable data. Lazy constants are treated as true constants by the JVM, enabling the same performance optimizations that are enabled by declaring a field `final`. Compared to `final` fields, however, lazy constants offer greater flexibility as to the timing of their initialization. This is a preview API.


**➜ Structured Concurrency (Seventh Preview)**


Simplify concurrent programming by introducing an API for *structured concurrency*. Structured concurrency treats groups of related tasks running in different threads as single units of work, thereby streamlining error handling and cancellation, improving reliability, and enhancing observability. This is a preview API.


**➜ Vector API (Twelfth Incubator)**


Introduce an API to express vector computations that reliably compile at run time to optimal vector instructions on supported CPUs, thus achieving performance superior to equivalent scalar computations. This is an incubating API.

See JEP 537

**➜ Post-Quantum Hybrid Key Exchange for TLS 1.3**


Enhance the security of Java applications that require secure network communication by implementing hybrid key exchange algorithms for TLS 1.3. Such algorithms defend against future quantum computing attacks by combining a quantum-resistant algorithm with a traditional algorithm. Applications that use the `javax.net.ssl` APIs will benefit from these improved algorithms by default, without change to existing code.


**➜ PEM Encodings of Cryptographic Objects (Third Preview)**


Introduce an API for encoding objects that represent cryptographic keys, certificates, and certificate revocation lists into the widely-used Privacy-Enhanced Mail (PEM) transport format, and for decoding from that format back into objects. This is a preview API.

See JEP 538

**➜ Make G1 the Default Garbage Collector in All Environments**


Make the Garbage-First (G1) garbage collector the default collector in all environments, rather than just server environments.

See JEP 523

**➜ Compact Object Headers by Default**


Make compact object headers the default object header layout in the HotSpot JVM. Compact object headers reduce object headers from 96 bits down to 64 bits on 64-bit architectures, thereby reducing heap size, improving deployment density, and increasing data locality.

See JEP 534

**➜ JFR In-Process Data Redaction**


Enhance JDK Flight Recorder (JFR) to redact command-line arguments and the initial values of environment variables and system properties in recordings. Redact this data before it leaves the process, so that sensitive information does not leak.

See JEP 536


This section describes some of the enhancements in Java SE 27 and JDK 27. In some cases, the descriptions provide links to additional detailed information about an issue or a change. The APIs described here are provided with the Oracle JDK. It includes a complete implementation of the Java SE 27 Platform and additional Java APIs to support developing, debugging, and monitoring Java applications. Another source of information about important enhancements and new features in Java SE 27 and JDK 27 is the Java SE 27 ( JSR 402) Platform Specification, which documents the changes to the specification made between Java SE 26 and Java SE 27. This document includes descriptions of those new features and enhancements that are also changes to the specification. The descriptions also identify potential compatibility issues that you might encounter when migrating to JDK 27.


The Foreign Function and Memory API now supports initializing the thread-local execution state before downcalls. This complements the preexisting call state capturing mechanism, which can already be used to read thread-local execution state after downcalls. The structure and contents of the execution state are platform-dependent and described by `Linker.Option.captureStateLayout()`.

When call state capturing is enabled through `Linker.Option.captureCallState(String... capturedState)`, a memory segment acts as a buffer for the thread-local data. Prior to the downcall, the contents of the buffer are used to initialize thread-local data. Immediately after the downcall, the thread-local data is copied into the buffer.

See `Linker.Option.captureCallState(String... capturedState)` in the JavaDoc API documentation for further information and a usage example


A Bash autocompletion script for the `jcmd` diagnostic tool has been added.

On Linux, to enable autocompletion, run `source <JDK_HOME>/conf/bash-completion/jcmd` at a Bash prompt. For system-wide availability, place the script in the `/usr/share/bash-completion/completions/` directory.


The JVM option `-XX:FlightRecorderOptions` has been extended with a `help` option that lists the available suboptions. Usage:

```
java -XX:FlightRecorderOptions:help
```

Starting with JDK 27, JFR redacts sensitive command-line arguments and the initial values of environment variables and system properties by default. You can explicitly select the information that JFR should redact by adding application-specific filters with `-XX:FlightRecorderOptions:redact-key=+<filter>` and `-XX:FlightRecorderOptions:redact-argument=+<filter>`.

Use `-XX:FlightRecorderOptions:redact-key=none,redact-argument=none` to disable the default redaction filters.

See JEP 536: JFR In-Process Data Redaction for more information.


The JVM option `-XX:AOTMode=required` has been added as an alias to `-XX:AOTMode=on` in order to clarify the purpose and effects of this option. `-XX:AOTMode=required` should be used instead of `-XX:AOTMode=on`, which will be eventually be deprecated and removed.


A new diagnostic command, `VM.security_properties`, has been added to the `jcmd` tool. This command prints the current set of Java security properties for a running JVM process, similar to the existing `VM.system_properties` command for system properties. This enhancement provides an easy and scriptable way to diagnose and troubleshoot security-related configuration issues in production and development environments.


The output of the `jcmd` `VM.info` diagnostic command and HotSpot fatal error logs (`hs_err_pid`) now include the current number of open file descriptors for the running Java process. On supported platforms, the new output displays the active file descriptor count in the `SYSTEM` section (for example, `Open File Descriptors: 52`). This enhancement helps diagnose and troubleshoot resource exhaustion and `Too many open files` issues on UNIX platforms.

Note that the maximum file descriptor limit for the process is already reported in the same output as the `NOFILE` value (for example, `NOFILE 10240/infinity`).


A new security property, `jdk.security.password.allowSystemIn`, has been added to control whether the Java platform can read passwords from the standard input stream if a console is not available. This typically occurs when standard input is redirected from a redirected file or connected to an interprocess pipe. If the property is set to `false`, then attempting to read passwords from the standard input stream without a console throws an exception. The default value is `true`. This default may change in a future release.

The property applies to tools that prompt for passwords such as `keytool` and `jarsigner` as well as `PasswordCallback` objects processed by `TextCallbackHandler`.


New methods have been added to the `KeyStore` and `KeyStoreSpi` classes that return the creation dates of alias entries as `java.time.Instant` values: `KeyStore.getCreationInstant` and `KeyStoreSpi.engineGetCreationInstant`.


The JDK now encodes ML-KEM and ML-DSA private keys in PKCS #8 format using the DER-encoded ASN.1 CHOICE formats defined in Section 6, "Private Key Format" of RFC 9935 and Section 6, "Private Key Format" of RFC 9881. Two new security properties, `jdk.mlkem.pkcs8.encoding` and `jdk.mldsa.pkcs8.encoding`, control the encoding used when generating new keys with a `KeyPairGenerator` or when translating keys with a `KeyFactory`. Valid values are `seed`, `expandedKey`, and `both` (case-insensitive). If a system property of the same name is also specified, it supersedes the security property value. All three formats are supported when decoding previously encoded private keys with a `KeyFactory`.

When these algorithms were introduced in JDK 24, the encoding format was equivalent to the `expandedKey` value. This release changes the default value to `seed`. As a result, ML-KEM and ML-DSA private keys generated by this JDK release will not be accepted by older releases by default, although keys generated by older releases are still readable by this release. To enable older JDK releases to accept ML-KEM and ML-DSA private keys generated by this JDK release, set the `jdk.mlkem.pkcs8.encoding` or `jdk.mldsa.pkcs8.encoding` property (whichever property is relevant to your use case) to `expandedKey`, and use `KeyFactory.translateKey` to convert the key to the older format.


The JDK now supports TLS 1.3 certificate compression with the `zlib` compression algorithm as specified in RFC 8879. This feature can reduce the size of certificate chains exchanged during TLS handshakes. The `brotli` and `zstd` compression algorithms are not supported.

TLS certificate compression is enabled by default. To disable it, add the `compress_certificate` extension to the `jdk.tls.client.disableExtensions` and `jdk.tls.server.disableExtensions` system properties. For example, the following Java command-line options disable TLS certificate compression for both the client and the server:

```
-Djdk.tls.client.disableExtensions=compress_certificate -Djdk.tls.server.disableExtensions=compress_certificate
```

This section describes the APIs, features, and options that were removed in Java SE 27 and JDK 27. The APIs described here are those that are provided with the Oracle JDK. It includes a complete implementation of the Java SE 27 Platform and additional Java APIs to support developing, debugging, and monitoring Java applications. Another source of information about important enhancements and new features in Java SE 27 and JDK 27 is the Java SE 27 ( JSR 402) Platform Specification, which documents changes to the specification made between Java SE 26 and Java SE 27. This document includes the identification of removed APIs and features not described here. The descriptions below might also identify potential compatibility issues that you could encounter when migrating to JDK 27. See CSRs Approved for JDK 27 for the list of CSRs closed in JDK 27.


On Linux, the `-Djdk.lang.Process.launchMechanism=VFORK` command-line option has been removed because the `VFORK` launch mechanism is inherently dangerous. Customers should stop using it and instead either rely on the default `POSIX_SPAWN` launch mechanism or replace it with `-Djdk.lang.Process.launchMechanism=FORK`.


The `finalize()` method has been removed from `java.util.concurrent.ThreadPoolExecutor`.

`ThreadPoolExecutor.finalize()` was deprecated in JDK 9 as part of deprecating finalization for removal. See JEP 421. The method was re-specified in JDK 11 to "do nothing" and deprecated for removal in JDK 18.

The removal of this method means that any existing code that extends `ThreadPoolExecutor` and overrides `finalize()` to call `super.finalize()` (or calls `finalize()` directly) will now call `Object.finalize()`. Such code might no longer compile because `Object.finalize()` declares `throws Throwable`, whereas the removed `ThreadPoolExecutor.finalize()` method declared no checked exceptions. Developers are strongly encouraged to stop using finalization. If necessary, however, the source compatibility issue can be worked around by using with a `try`-`catch` block.


`java.locale.useOldISOCodes` System Property
Support for the `java.locale.useOldISOCodes` system property in the `java.util.Locale` class has been removed. Originally introduced in JDK 17, this property allowed applications to revert to legacy ISO 639 language codes (`iw` for Hebrew, `ji` for Yiddish, and `in` for Indonesian). It was deprecated in JDK 25 and, starting with JDK 27, specifying this property has no effect and triggers a warning at runtime. Users who still rely on the old codes are encouraged to transition to the current standard codes: `he` for Hebrew, `yi` for Yiddish, and `id` for Indonesian.


The JNDI/LDAP service provider, belonging to the `java.naming` module in the JDK, will no longer have any values set by default for the `java.naming.factory.control`, `java.naming.factory.object` and `java.naming.factory.state` standard JNDI properties.

These standard JNDI properties that specify lists of JNDI factories were previously being set by the LDAP service provider to class names of classes that did not belong to the JDK. Those classes may have been part of the application classpath. With this change, such applications will no longer see those factories being used.

Applications that may have been relying on these three JNDI properties being set by the LDAP service provider are expected to set these values themselves as described in the section Provider Resource Files Link icon in `javax.naming.Context`.


Obsolete localized resource have been removed from the JDK source repository.

The JDK includes localized resource files for a number of components. However, apart from English, Japanese, German, and Simplified Chinese, the other localized resource files are unmaintained. Over time, these files are falling out of sync with their English counterparts. For example, in builds that include these unmaintained resource files, the output of the `java` launcher's `--help` option would not contain information for `--enable-preview` in unmaintained languages.

These localized resource files have been removed. The exception to this change is the `java.desktop` module, whose unmaintained localized resource files will remain available. As a result, users of builds that previously exposed messages in an unmaintained language may see English output instead.


The experimental JVM Compiler Interface (JVMCI) has been removed in this release. These changes include the removal of JVMCI code from the HotSpot JVM; modules `jdk.internal.vm.ci`, `jdk.graal.compiler`, and `jdk.graal.compiler.management`; JVMCI-specific JIT-compilation policies; configure feature-selection flags; and all flags containing the string `JVMCI` as well as `-XX:+UseGraalJIT`.


The `java` launcher options `-noclassgc`, `-noverify`, `-verifyremote`, and the `-Xverify:none` HotSpot VM option which had been deprecated, have been removed.

Applications that were using `-noclassgc` when launching `java` may consider using its equivalent `-Xnoclassgc` instead. Similarly, applications that were using `-verifyremote` may consider using `-Xverify:remote` instead.

There's no replacement for the `-noverify` or `-Xverify:none` options, and applications should no longer use them.



The default time zone returned by `TimeZone.getDefault()` on Windows now reflects the latest IANA TZ database IDs, aligning with current IANA definitions. Previously, for some regions, such as India, the method returned obsolete IDs (for example, "Asia/Calcutta"), but it now correctly returns the current IDs (for example, "Asia/Kolkata").


The G1 garbage collector changes the default values of the `-XX:MinHeapFreeRatio` and `-XX:MaxHeapFreeRatio` options from 40 and 70 to 0 and 100, respectively. As a result, G1 heap resizing due to these constraints is effectively disabled by default.

The previous default values could trigger heap expansion or shrinkage after a Full GC, potentially causing unnecessary heap resizing. In particular, for applications that frequently invoke `System.gc()`, a Full GC could change the heap size because of these options, and the regular heap sizing algorithm could immediately undo these modifications.

Applications that explicitly set `-XX:MinHeapFreeRatio` or `-XX:MaxHeapFreeRatio` retain the heap resizing behavior based on their specified settings.


The following notes describe additional changes and information about this release. In some cases, the following descriptions provide links to additional detailed information about an issue or a change.


The behavior of the JDK built-in `com.sun.net.httpserver.HttpServer` implementation, when matching an incoming request path to an `HttpContext` path, is switched from *string prefix matching* to *path prefix matching*. This means the request path must begin with the context path, and all matching path segments must be identical. For instance, the context path `/foo` would match request paths `/foo`, `/foo/`, and `/foo/bar`, but not `/foobar`. The old behavior, *string prefix matching*, can be enabled using the newly introduced `sun.net.httpserver.pathMatcher` system property. This property and the ability to restore the old behavior may be removed in a future release.


Predefined ISO-8601 based formatters in the `DateTimeFormatter` class that accept zone offsets (for example, `ISO_DATE`) now support short zone offsets for parsing. In previous releases, parsing offsets without seconds or nanoseconds (for example, `+01`) resulted in a `DateTimeParseException` with some ISO formatters. These offsets are now parsed successfully by all predefined ISO formatters that accept zone offsets.


`ServiceLoader` has been changed so that iteration over service providers now consistently throws `ServiceConfigurationError` when `NoClassDefFoundError` or other linkage errors are encountered. `ServiceConfigurationError` is also thrown when consuming a stream of service providers and a linkage error is encountered.


To determine whether additional GZIP members follow the current member, `java.util.zip.GZIPInputStream` checks for another member after reading the current member's trailer.

In releases prior to JDK 23, `GZIPInputStream` called `InputStream.available()` on the underlying stream before attempting to read the next member header. JDK 23 changed this behavior (see JDK-7036144) to skip the `available()` call and instead attempt the read directly.

Because this change introduced compatibility issues for some applications, `GZIPInputStream` has been restored to its pre-JDK 23 behavior. It once again calls `InputStream.available()` before attempting to read the next member header.

A new system property, `jdk.util.gzip.tryReadAheadAfterTrailer`, has been added. Setting this property to `true` restores the pre-JDK 23 behavior: `GZIPInputStream` attempts to read the next member header without first calling `InputStream.available()`. This property is intended for applications that depend on that behavior.


`ZipOutputStream.putNextEntry` now throws `ZipException` when a `ZipEntry` name or comment string contains unmappable characters and cannot be encoded using the charset of the `ZipOutputStream`.

Before this release, an unmappable name would cause `ZipOutputStream.putNextEntry` to throw an `IllegalArgumentException`. Similarly, an unmappable comment would cause an `IllegalArgumentException` to be thrown from `ZipOutputStream.finish` or `ZipOutputStream.close`.

Any code catching `IllegalArgumentException` for such format errors should be updated to instead catch the specified `ZipException`.


Empty subsets of `TreeMap` and `TreeSet` now throw `NullPointerException` when passing a null action to `Stream.forEach`, `Spliterator.forEachRemaining` or `Spliterator.tryAdvance`. In prior releases, implementations of `Spliterator` and `Stream` for subsets of `TreeMap` and `TreeSet` returned `false` or completed normally when passed a null action if the subset was empty, instead of throwing `NullPointerException` as required by the specification.


The locale data based on the Unicode Consortium's CLDR has been upgraded to version 48.2. Besides the usual addition of new locale data and translation changes, there are notable changes from the upstream CLDR, affecting Date/Time/Number formats:

CLDR-19106 Unreadable date + times in zh_Hant and yue

CLDR-13986 fr_CH: Formatting should be CHF XXX'XXX.00 in my opinion

CLDR-19280 BC moving to permanent DST

Note that locale data is subject to change in a future release of the CLDR. Although not all locale data changes affect the JDK, users should not assume stability across releases. For more information, see the Unicode Consortium's CLDR release notes and search https://unicode-org.atlassian.net for locale data deltas.


The `Thread` delivering events to implementations of `javax.naming.event.NamingListener`, enrolled with a `javax.naming.ldap.LdapContext`, will now have its thread context class loader (see `Thread.getContextClassLoader`) set to the system class loader. See Run-time Built-in Class Loaders.


The JSON format thread dump generated by `com.sun.management.HotSpotDiagnosticMXBean.dumpThreads`, and the `jcmd` `Thread.dump_to_file` command, now generate the JSON values for thread identifiers, thread counts, and the process identifier as numbers, for example, `{ "tid": 3, ...  }`. The values were previously generated as strings. Programs that parse the JSON format thread dump may need to be updated.

As part of the change, the `threadDump` object now has a member named `formatVersion` for the thread dump format (with a value of 2), which will be updated as the thread dump format evolves.


`RuntimeMXBean.getInputArguments()` now adds the `-XX:` prefix for arguments passed through a settings file. `RuntimeMXBean.getInputArguments()` returns arguments passed on the command line as well as any given through settings files (`-XX:Flags=<path>`). Previously, arguments from a settings file were returned without the `-XX:` prefix, for example, `+UseG1GC`. Now, these arguments are returned with the `-XX:` prefix, for example, `-XX:+UseG1GC`.


The command line option `-XX:InitiatingHeapOccupancyPercent` has been renamed to `-XX:G1IHOP` to reflect its usage with the G1 garbage collector.

The old option `-XX:InitiatingHeapOccupancyPercent` is still available for backward compatibility; this alias is now deprecated.


To avoid leaking sensitive information from processes running on the same computer, the `jdk.SystemProcess` event no longer emits command-line arguments. The process ID and the name of the process are still part of the event, which is typically sufficient for troubleshooting.


The JFR event `jdk.OldObjectSample` is disabled when using generational ZGC.

The combination results in unacceptable performance overhead because the implementation relies on weak handles that, in generational ZGC, are processed only in the old generation.


Compact Object Headers was introduced in JDK 24 through JEP 450. This feature is now enabled by default, which reduces the Java heap footprint of applications and potentially improves performance.

You can still disable this feature with the `-XX:-UseCompactObjectHeaders` flag, which is planned for deprecation and removal in a future release.

The CDS archives for the JDK image, `classes_coh.jsa` and `classes_nocoops_coh.jsa`, provide equivalent startup performance and are configured with the default `UseCompactObjectHeaders` turned on. Alternate CDS archives are provided if the feature is turned off.


The HotSpot JVM option `UseCompressedClassPointers` was deprecated in JDK 25 and is now obsolete in JDK 27. If you use this option on the Java command line, either to use compressed class pointer mode or uncompressed class pointer mode, you will get a warning: `Ignoring option UseCompressedClassPointers; support was removed in 27.0`.

The JVM now always compresses class pointers in Java objects.


The Serviceability Agent (SA) `printmdo` command has been removed. The 'printmdo' is a subcommand of the `clhsdb` tool used for displaying `MethodData`, which is JVM internal compiler information.


The following root certificates have been added to the `cacerts` truststore:

```
+ WISeKey
  + wisekeyglobalrootgbca
    DN: CN=OISTE WISeKey Global Root GB CA, OU=OISTE Foundation Endorsed, O=WISeKey, C=CH
+ WISeKey
  + wisekeyglobalrootgcca
    DN: CN=OISTE WISeKey Global Root GC CA, OU=OISTE Foundation Endorsed, O=WISeKey, C=CH
```

The HSS/LMS signature algorithm now supports the additional parameter sets defined in RFC 9858, in addition to those originally defined in RFC 8554. These new parameter sets can produce smaller signatures.


A new system and security property, `com.sun.security.crl.maxSize`, has been added to limit the maximum length of a CRL that is downloaded through URIs in the CRL Distribution Points certificate extension during path validation. The value of these properties is the size in bytes of the DER-encoded CRL. For protocols that can return multivalue responses, such as LDAP, the size threshold is the sum of all CRLs downloaded from a single search query. CRLs that exceed this length will not be processed during certificate path validation. This size limit does not apply to CRLs that are imported through non-network-based means. A negative value disables this size limitation. A non-numeric value will be ignored, and the default size will be used instead. The default size limit is 20 MiB. For cases where both the security and system properties are set, the system property takes precedence. Enabling certificate path logging by setting `java.security.debug=certpath` will output the current size limit and note any discarded CRLs.


Curve25519 field operations have been optimized in this release, which has resulted in noticeable performance improvements for X25519 key generation and key agreement, and for Ed25519 key generation and signing. As a result, the following gains were measured:

The following benchmarks were used to measure performance:

`micro:org.openjdk.bench.javax.crypto.full.KeyPairGeneratorBench.XDH``micro:org.openjdk.bench.javax.crypto.full.KeyAgreementBench.XDH``micro:org.openjdk.bench.javax.crypto.full.KEMBench.JSSE_DHasKEM``micro:org.openjdk.bench.java.security.SSLHandshake``micro:org.openjdk.bench.javax.crypto.full.KeyPairGeneratorBench.EdDSA``micro:org.openjdk.bench.javax.crypto.full.SignatureBench.EdDSA`

SHA-3 intrinsics have been optimized in this release for x86_64 systems that support AVX-512. The ML-KEM and ML-DSA algorithms extensively use SHA-3. This has resulted in noticeable performance improvements for ML-KEM in key generation, decapsulation, and encapsulation, and ML-DSA key generation, signing, and verification as follows:

`ML-KEM` key generation: +48%`ML-DSA` key generation: +60%`micro:org.openjdk.bench.javax.crypto.full.KeyPairGeneratorBench.MLKEM``micro:org.openjdk.bench.javax.crypto.full.KEMBench.MLKEM``micro:org.openjdk.bench.javax.crypto.full.KeyPairGeneratorBench.MLDSA``micro:org.openjdk.bench.javax.crypto.full.SignatureBench.MLDSA`

ffdhe6144 and ffdhe8192 were removed from the default list of TLS named groups, as they are almost never used in practice and require additional host resources to process. You can add them to the default list of named groups by setting the system property `jdk.tls.namedGroups` or by calling the `SSLParameters.setNamedGroups` method when configuring a TLS socket connection.


The parsing of the `javax.net.debug` system property for TLS/SSL debug logging has been updated to strictly enforce documented option and suboption relationships. Suboptions such as `verbose`, `packet`, and `plaintext` now require their respective parent options, and only valid combinations have an effect. Invalid or unsupported combinations are silently ignored.

Previously, some suboptions could be used independently or in incorrect combinations, contrary to the intended design. The help message (`-Djavax.net.debug=help`) has also been updated to accurately reflect all supported and obsolete entries.

Users who previously relied on undocumented or invalid combinations (for example, specifying only `-Djavax.net.debug=verbose`) will need to update their configurations to match the documented usage. Please refer to the updated help output for the current list of valid options and examples of correct usage.


TLS 1.3 key agreement has been changed to use the `Generic` key algorithm for Diffie-Hellman shared secrets instead of the `TlsPremasterSecret` algorithm. This implementation detail should be transparent to the users of the SunJSSE provider. Users of JCE providers that do not support `Generic` keys may observe TLS 1.3 handshakes failing with an `SSLHandshakeException` with the message "Could not generate secret".

In case of compatibility issues, the original behavior can be restored by setting the system property `jdk.tls.t13KeyDerivationAlgorithm` to `TlsPremasterSecret`.


Hybrid key exchange algorithms for TLS 1.3 have been implemented, enhancing the security of Java applications that require secure network communication. These algorithms defend against future quantum computing attacks by combining a quantum-resistant algorithm with a traditional algorithm.

The following three new hybrid key exchange algorithms are supported: `X25519MLKEM768`, `SecP256r1MLKEM768`, and `SecP384r1MLKEM1024`. The X25519MLKEM768 group is enabled by default and is the most preferred group. This feature works out of the box. No API or configuration change is needed, as long as both peers support the X25519MLKEM768 group and it is the most preferred group. X25519MLKEM768 is the most widely deployed hybrid group.

To verify that a post-quantum hybrid group was negotiated by the TLS handshake, enable JSSE debugging by setting the system property `javax.net.debug` to `ssl:handshake` and inspect the contents of the `key_share` extension of the `ServerHello` message, ex:

```
"key_share (51)": {
  "server_share": {
    "named group": x25519mlkem768
  }
}
```
For more details, see JEP 527: Post-Quantum Hybrid Key Exchange for TLS 1.3.


The `java` launcher now correctly determines which `main` method to invoke when a package-private `main` method is defined in a different package from the class being launched. The `java` launcher no longer invokes such a `main` method.


The Compiler Tree API, which models Java source code with abstract trees (ASTs), has been changed to more precisely model variables with inferred types. See `com.sun.source.tree.VariableTree.getType()` for more information.


The implementation of `DocTrees.getElement(DocTreePath)` has been updated to align more closely with the `javax.lang.model` API. In particular, the method now returns `null` when the argument refers to a primitive or array type. Previously, it returned an invalid `TypeElement` for primitive types and the component type for array types.

You can use `DocTrees.getType(DocTreePath)` to obtain valid `TypeMirror` instances for `DocTreePath` instances referring to primitive or array types.


`javac` now enforces JLS rules for trailing ASCII SUB (`U+001A`, control-Z) characters in source files. If `U+001A` appears and any additional characters follow it, then those trailing characters are no longer silently ignored; they are treated as illegal input.


Type annotations—in particular, annotations that have `@Target(TYPE_USE)` and are not applicable for local variables and parameter declarations—will now be consistently rejected for all variables with inferred types (that is, using `var`). This includes lambda parameters and for-each variables.


The `javadoc` tool now recursively copies subdirectories of `doc-files` directories to the generated output. The `-docfilessubdirs` option, which was previously used to enable this behavior, is no longer required and may be removed in a future release.

The `-excludedocfilessubdir` option now supports `*` as an argument to exclude all `doc-files` subdirectories from being copied, thereby restoring the previous default behavior.


Although we have stated the goal to have Oracle JDK and OpenJDK binaries be as close to each other as possible, there remain several differences between the two options.

The current differences are:

`msi`, `rpm`, `deb`, etc.) which not only place the JDK binaries in your system but also contain update rules and in some cases handle some common configurations like set common environmental variables (such as, JAVA_HOME in Windows) and establish file associations (such as, use `java` to launch `.jar` files). OpenJDK is offered only as compressed archive (`tar.gz` or `.zip`).`java -version` is different. Oracle JDK returns java and includes the Oracle-specific identifier. OpenJDK returns OpenJDK and does not include the Oracle-specific identifier.`\legal\java.desktop\freetype.md` is therefore different.
