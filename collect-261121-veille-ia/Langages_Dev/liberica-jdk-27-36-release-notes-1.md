---
id: collect-261121-veille-ia/langages-dev/liberica-jdk-27-36-release-notes-1
title: "Liberica JDK 27+36: Release Notes"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["release", "path", "matching", "liberica", "summary", "description", "offsets", "javax"]
source: https://docs.bell-sw.com/liberica-jdk/27b36/general/release-notes/
source_anchor: ""
source_lines: [1, 1500]
section: "Langages_Dev"
---
# Liberica JDK 27+36: Release Notes

Published: September 15, 2026

## 1. Version information

This document provides information about Liberica JDK 27 release. The full version string for this update release is 27+36. The version number is 27.

Liberica JDK 27 is distributed as `.apk`, `.rpm`, `.zip`, `.deb`, `.pkg`, and `.tar.gz` packages. Please select the most appropriate for your purposes.

## 2. What’s New

This release contains the following updates and new features.

### Notable Changes

This is the list of the notable issues fixed in this release.

| Issue ID |  | 
|---|---|
| JDK-8044609 | Summary: Enforcement Of Valid javax.net.debug Option Combinations In TLS Description: The parsing of the `javax.net.debug` system property for TLS/SSL debug logging has been updated to strictly enforce documented option and suboption relationships. Suboptions such as`verbose` ,`packet` , and`plaintext` now require their respective parent options, and only valid combinations have an effect. Invalid or unsupported combinations are silently ignored. Previously, some suboptions could be used independently or in incorrect combinations, contrary to the intended design. The help message (`-Djavax.net.debug=help` ) has also been updated to accurately reflect all supported and obsolete entries. Users who previously relied on undocumented or invalid combinations (for example, specifying only`-Djavax.net.debug=verbose` ) will need to update their configurations to match the documented usage. Please refer to the updated help output for the current list of valid options and examples of correct usage. | 
| JDK-8196182 | Summary: ServiceLoader Changed to Throw ServiceConfigurationError When a Linkage Error Is Encountered Description: [ServiceLoader] has been changed so that iteration over service providers now consistently throws [ServiceConfigurationError] when [NoClassDefFoundError] or other linkage errors are encountered. `ServiceConfigurationError` is also thrown when consuming a stream of service providers and a linkage error is encountered. | 
| JDK-8210336 | Summary: Predefined ISO-8601 Formatters Support Short Zone Offsets Description: Predefined ISO-8601 based formatters in the [DateTimeFormatter] class that accept zone offsets (for example, `ISO_DATE` ) now support short zone offsets for parsing. In previous releases, parsing offsets without seconds or nanoseconds (for example,`+01` ) resulted in a`DateTimeParseException` with some ISO formatters. These offsets are now parsed successfully by all predefined ISO formatters that accept zone offsets. | 
| JDK-8268850 | Summary: AST Model for var Variables Adheres More Closely to Source Code Description: The Compiler Tree API, which models Java source code with abstract trees (ASTs), has been changed to more precisely model variables with inferred types. See [com.sun.source.tree.VariableTree.getType()] for more information. | 
| JDK-8272758 | Summary: Behavior Change of HttpServer from String Prefix Matching to Path Prefix Matching Description: The behavior of the JDK built-in [com.sun.net.httpserver.HttpServer] implementation, when matching an incoming request path to an `HttpContext` path, is switched from**string prefix matching** to**path prefix matching** . This means the request path must begin with the context path, and all matching path segments must be identical. For instance, the context path`/foo` would match request paths`/foo` ,`/foo/` , and`/foo/bar` , but not`/foobar` . The old behavior,**string prefix matching** , can be enabled using the newly introduced [sun.net.httpserver.pathMatcher] system property. This property and the ability to restore the old behavior may be removed in a future release. | 
| JDK-8273874 | Summary: Thread Delivering Implementations of javax.naming.event.NamingEvent Will Use the System ClassLoader for its Context Class Loader Description: The `Thread` delivering events to implementations of`javax.naming.event.NamingListener` , enrolled with a`javax.naming.ldap.LdapContext` , will now have its thread context class loader (see [Thread.getContextClassLoader]) set to the system class loader. See Run-time Built-in Class Loaders. | 
| JDK-8284315 | Summary: DocTrees.getElement Is Now More Consistent with the javax.lang.model API Description: The implementation of [DocTrees.getElement(DocTreePath)] has been updated to align more closely with the [javax.lang.model] API. In particular, the method now returns `null` when the argument refers to a primitive or array type. Previously, it returned an invalid [TypeElement] for primitive types and the component type for array types. You can use [DocTrees.getType(DocTreePath)] to obtain valid [TypeMirror] instances for`DocTreePath` instances referring to primitive or array types. | 
| JDK-8296183 | Summary: java.naming.factory.control, java.naming.factory.object and java.naming.factory.state Standard JNDI Properties Are No Longer Set by Default by the LDAP Service Provider in the JDK Description: The JNDI/LDAP service provider, belonging to the `java.naming` module in the JDK, will no longer have any values set by default for the`java.naming.factory.control` ,`java.naming.factory.object` and`java.naming.factory.state` standard JNDI properties. These standard JNDI properties that specify lists of JNDI factories were previously being set by the LDAP service provider to class names of classes that did not belong to the JDK. Those classes may have been part of the application classpath. With this change, such applications will no longer see those factories being used. Applications that may have been relying on these three JNDI properties being set by the LDAP service provider are expected to set these values themselves as described in the section Provider Resource Files Link icon in [javax.naming.Context]. | 
| JDK-8314323 | Summary: Post-Quantum Hybrid Key Exchange for TLS 1.3 Description: Hybrid key exchange algorithms for TLS 1.3 have been implemented, enhancing the security of Java applications that require secure network communication. These algorithms defend against future quantum computing attacks by combining a quantum-resistant algorithm with a traditional algorithm. Applications that use the [javax.net.ssl] APIs will benefit from these improved algorithms by default, without requiring any code changes. The following three new hybrid key exchange algorithms are supported: `X25519MLKEM768` ,`SecP256r1MLKEM768` , and`SecP384r1MLKEM1024` . Of these, only`X25519MLKEM768` is placed at the front of the default named groups list, making it the most preferred group. For more details, see JEP 527: Post-Quantum Hybrid Key Exchange for TLS 1.3. | 
| JDK-8347938 | Summary: Added Support for the Latest ML-KEM and ML-DSA Private Key Encodings Description: The JDK now encodes ML-KEM and ML-DSA private keys in PKCS #8 format using the DER-encoded ASN.1 CHOICE formats defined in Section 6, "Private Key Format" of RFC 9935 and Section 6, "Private Key Format" of RFC 9881. Two new security properties, `jdk.mlkem.pkcs8.encoding` and`jdk.mldsa.pkcs8.encoding` , control the encoding used when generating new keys with a`KeyPairGenerator` or when translating keys with a`KeyFactory` . Valid values are`seed` ,`expandedKey` , and`both` (case-insensitive). If a system property of the same name is also specified, it supersedes the security property value. All three formats are supported when decoding previously encoded private keys with a`KeyFactory` . When these algorithms were introduced in JDK 24, the encoding format was equivalent to the`expandedKey` value. This release changes the default value to`seed` . As a result, ML-KEM and ML-DSA private keys generated by this JDK release will not be accepted by older JDK releases by default, although keys generated by older releases are still readable by this release. To enable older JDK releases to accept ML-KEM and ML-DSA private keys generated by this JDK release, set the`jdk.mlkem.pkcs8.encoding` or`jdk.mldsa.pkcs8.encoding` property (whichever property is relevant to your use case) to`expandedKey` , and use`KeyFactory.translateKey` to convert the key to the older format. This change is planned to be backported to Oracle’s JDK update releases that support ML-KEM and ML-DSA, so the interoperability impact is expected to be temporary. See Oracle JRE and JDK Cryptographic Roadmap for more information. | 
| JDK-8355522 | Summary: Removal of the `java.locale.useOldISOCodes` System Property Description: Support for the `java.locale.useOldISOCodes` system property in the [java.util.Locale] class has been removed. Originally introduced in JDK 17, this property allowed applications to revert to legacy ISO 639 language codes (`iw` for Hebrew,`ji` for Yiddish, and`in` for Indonesian). It was deprecated in JDK 25 and, starting with JDK 27, specifying this property has no effect and triggers a warning at runtime. Users who still rely on the old codes are encouraged to transition to the current standard codes:`he` for Hebrew,`yi` for Yiddish, and`id` for Indonesian. | 
| JDK-8357089 | Summary: Removed VFORK Launch Mechanism from Process Implementation (Linux) Description: On Linux, the `-Djdk.lang.Process.launchMechanism=VFORK` command-line option has been removed because the`VFORK` launch mechanism is inherently dangerous. Customers should stop using it and instead either rely on the default`POSIX_SPAWN` launch mechanism or replace it with`-Djdk.lang.Process.launchMechanism=FORK` . | 
| JDK-8360700 | Summary: Compact Object Headers Are Enabled by Default Description: Compact Object Headers was introduced in JDK 24 through JEP 450. This feature is now enabled by default, which reduces the Java heap footprint of applications and potentially improves performance. You can still disable this feature with the `-XX:-UseCompactObjectHeaders` flag, which is planned for deprecation and removal in a future release. The CDS archives for the JDK image,`classes_coh.jsa` and`classes_nocoops_coh.jsa` , provide equivalent startup performance and are configured with the default`UseCompactObjectHeaders` turned on. Alternate CDS archives are provided if the feature is turned off. | 
| JDK-8363996 | Summary: The UseCompressedClassPointers Option Is Now Obsolete Description: The HotSpot JVM option `UseCompressedClassPointers` was deprecated in JDK 25 and is now obsolete in JDK 27. If you use this option on the Java command line, either to use compressed class pointer mode or uncompressed class pointer mode, you will get a warning:`Ignoring option UseCompressedClassPointers; support was removed in 27.0` . The JVM now always compresses class pointers in Java objects. | 
| JDK-8364182 | Summary: New jcmd Command VM.security_properties Displays Active Security Properties at Runtime Description: A new diagnostic command, `VM.security_properties` , has been added to the [jcmd] tool. This command prints the current set of Java security properties for a running JVM process, similar to the existing`VM.system_properties` command for system properties. This enhancement provides an easy and scriptable way to diagnose and troubleshoot security-related configuration issues in production and development environments. | 
| JDK-8368692 | Summary: Added the Security Property jdk.security.password.allowSystemIn to Restrict the Java Platform from Reading from the Standard Input Stream Description: A new security property, `jdk.security.password.allowSystemIn` , has been added to control whether the Java platform can read passwords from the standard input stream if a console is not available. This typically occurs when standard input is redirected from a redirected file or connected to an interprocess pipe. If the property is set to`false` , then attempting to read passwords from the standard input stream without a console throws an exception. The default value is`true` . This default may change in a future release. The property applies to tools that prompt for passwords such as`keytool` and`jarsigner` as well as [PasswordCallback] objects processed by [TextCallbackHandler]. | 
| JDK-8369282 | Summary: Distrust TLS Server Certificates Anchored by Chunghwa Root Certificates and Issued After March 17, 2026 Description: The JDK will stop trusting TLS server certificates issued after March 17, 2026 and anchored by Chunghwa root certificates, in line with similar plans announced by Google and Mozilla. TLS server certificates issued on or before March 17, 2026 will continue to be trusted until they expire. Certificates issued after that date, and anchored by the Certificate Authority listed in the table below, will be rejected. The restrictions are enforced in the JDK implementation (the `SunJSSE` Provider) of the Java Secure Socket Extension (JSSE) API. A TLS session will not be negotiated if the server’s certificate chain is anchored by any of the Certificate Authorities in the table below and the certificate has been issued after March 17, 2026. An application will receive an exception with a message indicating the trust anchor is not trusted. The JDK can be configured to trust these certificates again by removing CHUNGHWA_TLS from the`jdk.security.caDistrustPolicies` security property in the`java.security` configuration file. | 
| JDK-8369736 | Summary: New Management Interface for AOT Cache Creation Description: A new management interface, [jdk.management.HotSpotAOTCacheMXBean], has been added to the `jdk.management` module to provide the operation`endRecording()` . This operation ends the AOT recording and creates the AOT cache and/or configuration file. As with any`MXBean` , the operation can be invoked either from within the application or from an external tool such as [jconsole]. | 
| JDK-8369917 | Summary: Additional Parameter Sets for HSS/LMS Description: The HSS/LMS signature algorithm now supports the additional parameter sets defined in RFC 9858, in addition to those originally defined in RFC 8554. These new parameter sets can produce smaller signatures. | 
| JDK-8371683 | Summary: `TYPE_USE` Annotations on a var Lambda Parameter Are Rejected Description: Type annotations in particular, annotations that have `@Target(TYPE_USE)` and are not applicable for local variables and parameter declarations will now be consistently rejected for all variables with inferred types (that is, using`var` ). This includes lambda parameters and for-each variables. | 
| JDK-8371748 | Summary: Removed ThreadPoolExecutor.finalize() Description: The `finalize()` method has been removed from [java.util.concurrent.ThreadPoolExecutor].`ThreadPoolExecutor.finalize()` was deprecated in JDK 9 as part of deprecating finalization for removal. See JEP 421. The method was re-specified in JDK 11 to "do nothing" and deprecated for removal in JDK 18. The removal of this method means that any existing code that extends`ThreadPoolExecutor` and overrides`finalize()` to call`super.finalize()` (or calls`finalize()` directly) will now call`Object.finalize()` . Such code might no longer compile because`Object.finalize()` declares`throws Throwable` , whereas the removed`ThreadPoolExecutor.finalize()` method declared no checked exceptions. Developers are strongly encouraged to stop using finalization. If necessary, however, the source compatibility issue can be worked around by using with a`try` -`catch` block. | 
| JDK-8371842 | Summary: Support for CLDR Version 48.2 Description: The locale data based on the Unicode Consortium’s CLDR has been upgraded to version 48.2. Besides the usual addition of new locale data and translation changes, there are notable changes from the upstream CLDR, affecting Date/Time/Number formats: CLDR-19106 - Unreadable date times in zh_Hant and yue; CLDR-13986 - fr_CH: Formatting should be CHF XXX’XXX.00 in my opinion; CLDR-19280 - BC moving to permanent DST. Note that locale data is subject to change in a future release of the CLDR. | 
| JDK-8371873 | Summary: The Java Compiler Now Correctly Rejects Characters That Appear in Source Code After an ASCII SUB Character Description: `javac` now enforces JLS rules for trailing ASCII SUB (`U+001A` , control-Z) characters in source files. If`U+001A` appears and any additional characters follow it, then those trailing characters are no longer silently ignored; they are treated as illegal input. | 
| JDK-8372351 | Summary: Added WISeKey Global GB and GC Root CA Certificates Description: Two WISeKey Global Root GB CA root certificates have been added to the `cacerts` truststore. | 
| JDK-8372526 | Summary: Added Support for ZLIB TLS Certificate Compression Description: The JDK now supports TLS 1.3 certificate compression with the `zlib` compression algorithm as specified in RFC 8879. This feature can reduce the size of certificate chains exchanged during TLS handshakes. The`brotli` and`zstd` compression algorithms are not supported. TLS certificate compression is enabled by default. To disable it, add the`compress_certificate` extension to the`jdk.tls.client.disableExtensions` and`jdk.tls.server.disableExtensions` system properties. | 
| JDK-8373426 | Summary: Removal of ffdhe6144 and ffdhe8192 from the Default List of TLS Named Groups Description: ffdhe6144 and ffdhe8192 were removed from the default list of TLS named groups, as they are almost never used in practice and require additional host resources to process. You can add them to the default list of named groups by setting the system property `jdk.tls.namedGroups` or by calling the`SSLParameters.setNamedGroups` method when configuring a TLS socket connection. | 
| JDK-8373476 | Summary: (tz) Update Timezone Data to 2025c Description: This release mostly changes code and commentary. The only changed data are leap second table expiration and pre-1976 time in Baja California. | 
| JDK-8373481 | Summary: Removal of -noclassgc, -noverify, -verifyremote and -Xverify:none Options Description: The `java` launcher options`-noclassgc` ,`-noverify` ,`-verifyremote` , and the`-Xverify:none` HotSpot VM option which had been deprecated, have been removed. Applications that were using`-noclassgc` when launching`java` may consider using its equivalent`-Xnoclassgc` instead. Similarly, applications that were using`-verifyremote` may consider using`-Xverify:remote` instead. There’s no replacement for the`-noverify` or`-Xverify:none` options, and applications should no longer use them. | 
| JDK-8374348 | Summary: -XX:AOTMode=required as alias for -XX:AOTMode=on Description: The JVM option `-XX:AOTMode=required` has been added as an alias to`-XX:AOTMode=on` in order to clarify the purpose and effects of this option.`-XX:AOTMode=required` should be used instead of`-XX:AOTMode=on` , which will be eventually be deprecated and removed. | 
| JDK-8374808 | Summary: New KeyStore and KeyStoreSpi Methods to Retrieve Creation Date as an Instant Description: New methods have been added to the `KeyStore` and`KeyStoreSpi` classes that return the creation dates of alias entries as`java.time.Instant` values: [KeyStore.getCreationInstant] and [KeyStoreSpi.engineGetCreationInstant]. | 
| JDK-8376698 | Summary: NullPointerException Thrown for Empty TreeMap and TreeSet Subsets from Spliterator and Stream Description: Empty subsets of `TreeMap` and`TreeSet` now throw`NullPointerException` when passing a null action to`Stream.forEach` ,`Spliterator.forEachRemaining` or`Spliterator.tryAdvance` . In prior releases, implementations of`Spliterator` and`Stream` for subsets of`TreeMap` and`TreeSet` returned`false` or completed normally when passed a null action if the subset was empty, instead of throwing`NullPointerException` as required by the specification. | 
| JDK-8377004 | Summary: Java Launcher No Longer Allows Inheriting a Package-Private main Method from Another Package Description: The `java` launcher now correctly determines which`main` method to invoke when a package-private`main` method is defined in a different package from the class being launched. The`java` launcher no longer invokes such a`main` method. | 
| JDK-8377013 | Summary: TimeZone.getDefault() Returns Obsolete ID on Windows (Asia/Calcutta) Description: The default time zone returned by [TimeZone.getDefault()] on Windows now reflects the latest IANA TZ database IDs, aligning with current IANA definitions. Previously, for some regions, such as India, the method returned obsolete IDs (for example, "Asia/Calcutta"), but it now correctly returns the current IDs (for example, "Asia/Kolkata"). | 
| JDK-8377550 | Summary: TLS 1.3 Key Agreement Now Uses the Generic Key Algorithm Description: TLS 1.3 key agreement has been changed to use the `Generic` key algorithm for Diffie-Hellman shared secrets instead of the`TlsPremasterSecret` algorithm. This implementation detail should be transparent to the users of the SunJSSE provider. Users of JCE providers that do not support`Generic` keys may observe TLS 1.3 handshakes failing with an`SSLHandshakeException` with the message "Could not generate secret". In case of compatibility issues, the original behavior can be restored by setting the system property`jdk.tls.t13KeyDerivationAlgorithm` to`TlsPremasterSecret` . | 
| JDK-8377797 | Summary: Remove Serviceability Agent Support for Printing MethodData and the printmdo Command Description: The Serviceability Agent (SA) `printmdo` command has been removed. The 'printmdo' is a subcommand of the`clhsdb` tool used for displaying`MethodData` , which is JVM internal compiler information. | 
| JDK-8380452 | Summary: ZipOutputStream.putNextEntry Now Throws ZipException When an Entry Name or Comment Cannot Be Encoded Description: `ZipOutputStream.putNextEntry` now throws`ZipException` when a`ZipEntry` name or comment string contains unmappable characters and cannot be encoded using the charset of the`ZipOutputStream` . Before this release, an unmappable name would cause`ZipOutputStream.putNextEntry` to throw an`IllegalArgumentException` . Similarly, an unmappable comment would cause an`IllegalArgumentException` to be thrown from`ZipOutputStream.finish` or`ZipOutputStream.close` . Any code catching`IllegalArgumentException` for such format errors should be updated to instead catch the specified`ZipException` . | 
| JDK-8381002 | Summary: The JSON Format Thread Dump Now Generates Thread Identifiers, Thread Counts, and the Process Identifier as Numbers Description: The JSON format thread dump generated by [com.sun.management.HotSpotDiagnosticMXBean.dumpThreads], and the [jcmd] `Thread.dump_to_file` command, now generate the JSON values for thread identifiers, thread counts, and the process identifier as numbers, for example,`{ "tid": 3, … }` . The values were previously generated as strings. Programs that parse the JSON format thread dump may need to be updated. As part of the change, the`threadDump` object now has a member named`formatVersion` for the thread dump format (with a value of 2), which will be updated as the thread dump format evolves. | 
| JDK-8381436 | Summary: Remove Obsolete Translation Resources Description: Obsolete localized resource have been removed from the JDK source repository. The JDK includes localized resource files for a number of components. However, apart from English, Japanese, German, and Simplified Chinese, the other localized resource files are unmaintained. Over time, these files are falling out of sync with their English counterparts. For example, in builds that include these unmaintained resource files, the output of the `java` launcher’s`--help` option would not contain information for`--enable-preview` in unmaintained languages. These localized resource files have been removed. The exception to this change is the [java.desktop] module, whose unmaintained localized resource files will remain available. As a result, users of builds that previously exposed messages in an unmaintained language may see English output instead. | 
| JDK-8382582 | Summary: Removal of the JVM Compiler Interface (JVMCI) Description: The experimental JVM Compiler Interface (JVMCI) has been removed in this release. These changes include the removal of JVMCI code from the HotSpot JVM; modules `jdk.internal.vm.ci` ,`jdk.graal.compiler` , and`jdk.graal.compiler.management` ; JVMCI-specific JIT-compilation policies; configure feature-selection flags; and all flags containing the string`JVMCI` as well as`-XX:+UseGraalJIT` . | 
| JDK-8382740 | Summary: Disable jdk.OldObjectSample event for generational ZGC Description: The JFR event `jdk.OldObjectSample` is disabled when using generational ZGC. The combination results in unacceptable performance overhead because the implementation relies on weak handles that, in generational ZGC, are processed only in the old generation. | 
| JDK-8383175 | Summary: (tz) Update Timezone Data to 2026b Description: British Columbia moved to permanent -07 on 2026-03-09. Some more overflow bugs have been fixed in zic. | 
| JDK-8384353 | Summary: SHA3 AVX2 and AVX512 intrinsics and improvements Description: SHA-3 intrinsics have been optimized in this release for x86_64 systems that support AVX-512. The ML-KEM and ML-DSA algorithms extensively use SHA-3. This has resulted in noticeable performance improvements for ML-KEM in key generation, decapsulation, and encapsulation, and ML-DSA key generation, signing, and verification. | 
| JDK-8385924 | Summary: GZIPInputStream.read() behaves differently on some Java versions Description: To determine whether additional GZIP members follow the current member, `java.util.zip.GZIPInputStream` checks for another member after reading the current member’s trailer. In releases prior to JDK 23, [GZIPInputStream] called`InputStream.available()` on the underlying stream before attempting to read the next member header. JDK 23 changed this behavior (see JDK-7036144) to skip the`available()` call and instead attempt the read directly. Because this change introduced compatibility issues for some applications,`GZIPInputStream` has been restored to its pre-JDK 23 behavior. It once again calls [InputStream.available()] before attempting to read the next member header. A new system property, [jdk.util.gzip.tryReadAheadAfterTrailer], has been added. Setting this property to`true` restores the pre-JDK 23 behavior:`GZIPInputStream` attempts to read the next member header without first calling`InputStream.available()` . This property is intended for applications that depend on that behavior. | 

### Discontinued OS support

Note that Liberica JDK 27 is not available for and cannot be run on the following operating systems:

- 
Windows x86 (32-bit)
- 
macOS 10.x

### IANA TZ Data version

This release of Liberica JDK 27 comes with the 2026b version of the in-tree copy of the IANA timezone database. The following are the key features of this version.

#### Briefly

British Columbia moved to permanent -07 on 2026-03-09.

Some more overflow bugs have been fixed in zic.

#### Changes to future timestamps

British Columbia’s 2026-03-08 spring forward was its last foreseeable clock change, as it moved to permanent -07 thereafter. (Thanks to Arthur David Olson.) Although the change to permanent -07 legally took place on 2026-03-09, temporarily model the change to occur on 2026-11-01 at 02:00 instead. This works around a limitation in CLDR v48.2 (2026-03-17). This temporary hack is planned to be removed after CLDR is fixed.

#### Changes to code

- 
zic no longer mishandles a last transition to a new time type.
- 
zic no longer overflows a buffer when generating a TZ string like "PST-167:59:58PDT-167:59:59,M11.5.6/-167:59:59,M12.5.6/-167:59:59", which can occur with adversarial input. (Thanks to Naveed Khan.)
- 
zic no longer generates a longer TZif file than necessary when an earlier time zone abbreviation is a suffix of a later one. As a nice side effect, zic no longer overflows a buffer when given a long series of abbreviations, each a suffix of the next. (Buffer overflow reported by Arthur Chan.)
- 
zic no longer overflows an int when processing input like ‘Zone Ouch 2147483648:00:00 - LMT’. The int overflow can lead to buffer overflow in adversarial cases. (Thanks to Naveed Khan.)
- 
zic now checks for signals more often.

## 3. Known Issues

This release does not contain any known issues.

## 4. Fixed CVEs

This release does not contain any fixed Common Vulnerabilities and Exposures.

## 5. Resolved Issues

### JDK issues

This is the list of general JDK issues fixed in this release.

| Issue ID | Summary | 
|---|---|
| JDK-4197755 | Arc2D.getBounds() returns an unnecessarily large bounding box | 
| JDK-4337898 | Serializing DefaultTableCellRenderer changes colors | 
| JDK-4393297 | Doc probs in java.awt.image.DataBufferInt | 
| JDK-4398379 | java.awt.image.DataBufferShort constructor should verify size of array | 
| JDK-4398381 | java.awt.image.DataBufferShort should check for negative size | 
| JDK-4459231 | Focus of JTabbedPane(with Scrollable tablayout) changes on change in LookAndFeel | 
| JDK-4696824 | In Metal and other L&F Button.focusInputMap, CheckBox.focusInputMap … are same | 
| JDK-4765299 | componentResized() not always called with nested JSplitPanes | 
| JDK-5107379 | Component orientation in JOptionPane is not proper in Motif L&F. | 
| JDK-6223700 | XP L&F: Non-TopLevel JMenu’s painting error | 
| JDK-6292135 | DefaultTableModel.setColumnIdentifiers() Clears JTable Row Heights | 
| JDK-6328248 | JProgessBar doesn’t show if printed on paper with PrintJob (1.1 Graphics API) | 
| JDK-6434110 | Color constructor parameter name is misleading | 
| JDK-6441373 | Editing JTable is not Serializable | 
| JDK-6517125 | FontStrike.getGlyphVectorOutline() not used | 
| JDK-6562639 | Wrong pixel bounds from TextLayout with white font | 
| JDK-6681958 | Maximization state of JInternalFrames is corrupted by WindowsDesktopManager | 
| JDK-6726690 | SwingUtilities.replaceUI*Map() methods do not remove previously installed maps | 
| JDK-6960970 | Debugger very slow during stepping | 
| JDK-7067310 | 3 tests from closed/javax/sound/sampled caused BSOD on win 7 x86 | 
| JDK-7186009 | [macOS] No way to hide(), dispose() FileDialog | 
| JDK-8042054 | JTree.updateUI uses out-of-date item size information | 
| JDK-8044609 | javax.net.debug options not working and documented as expected | 
| JDK-8068378 | [TEST_BUG]The java/awt/Modal/PrintDialogsTest/PrintDialogsTest.java instruction need to update | 
| JDK-8074292 | nsk/jdb/kill/kill001: generateOopMap.cpp assert(bb→is_reachable()) failed | 
| JDK-8078744 | Right half of system menu icon on title bar does not activate when clicked in Metal L&F | 
| JDK-8080226 | G1: Replace collector state booleans with explicit state variable(s) | 
| JDK-8134541 | latent concurrency bug in ScriptRunData | 
| JDK-8158801 | [TEST_BUG] Mixing tests fail because of focus workaround trick | 
| JDK-8167268 | StandardGlyphVector.getGlyphMetrics creates metrics with erroneous bounds for characters with no outline (e.g., the space character ' ') | 
| JDK-8170896 | TEST_BUG: java/rmi/server/Unreferenced/leaseCheckInterval/LeaseCheckInterval.java failed with unreferenced() not invoked after 20.0 seconds | 
| JDK-8177841 | Some java/awt/Robot tests can be improved | 
| JDK-8179187 | Misleading compilation error on annotated fully-qualified classname | 
| JDK-8179918 | EnumSet spliterator should report SORTED, ORDERED, NONNULL | 
| JDK-8180483 | sun.net.www.protocol.http.HttpClient::available may consume one character | 
| JDK-8188131 | [PPC] Increase inlining thresholds to the same as other platforms | 
| JDK-8196182 | ServiceLoader.iterator().hasNext()/.next() may throw a LinkageError | 
| JDK-8198665 | Many java/awt/Modal/ModalBlockingTest fail on mac | 
| JDK-8208752 | Calling a deserialized Lambda might fail with ClassCastException | 
| JDK-8210336 | DateTimeFormatter predefined formatters should support short time zone offsets | 
| JDK-8214934 | Wrong type annotation offset on casts on expressions | 
| JDK-8219203 | Use StringBuilder instead of StringBuffer in com.sun.jndi.dns.ResourceRecord | 
| JDK-8225186 | G1: Compiler code cache requested GC deadlocks while WhiteBox has control | 
| JDK-8227106 | Rename InitiatingHeapOccupancyPercent to G1IHOP and make G1IHOP G1 specific | 
| JDK-8227493 | Return a more useful error message from lookupAllHostAddr if getaddrinfo results in EAI_SYSTEM error | 
| JDK-8230421 | Java cannot handle Euro sign at the Simplified Chinese Windows Command Prompt | 
| JDK-8230518 | printSource doesn’t handle enums correctly | 
| JDK-8237220 | [macOS] AWT text test CorrectTextComponentSelectionTest.html sometimes fails | 
| JDK-8238686 | G1 may waste lots of space or fail to uncommit when observing MinHeapFreeRatio during sizing after full gc | 
| JDK-8248122 | java launcher should launch normally if JavaFX modules not found | 
| JDK-8248165 | Missing whitespace in jdb message printed when setting value of invalid type | 
| JDK-8255463 | java/nio/channels/spi/SelectorProvider/inheritedChannel/InheritedChannelTest.java failed with ThreadTimeoutException | 
| JDK-8261289 | Incorrect cleanup in LDAP TLS handling | 
| JDK-8265429 | Improve GCM encryption | 
| JDK-8265647 | DataBufferUShort constructors check 'dataArray' for 'null' after dereference | 
| JDK-8267961 | JInternalFrame.getNormalBounds() returns normalBounds when maximized state is false instead of bounds | 
| JDK-8268675 | RTE from "Printable.print" propagates through "PrinterJob.print" | 
| JDK-8268850 | AST model for 'var' variables should more closely adhere to the source code | 
| JDK-8269613 | vmTestbase/vm/mlvm/mixed/stress/java/findDeadlock/TestDescription.java fails with OOME in CodeCache | 
| JDK-8269888 | Thai text rendered incorrectly using some AffineTransform-derived fonts | 
| JDK-8272758 | Improve HttpServer to avoid partial file name matches while mapping request path to context path | 
| JDK-8273874 | LdapClient can trigger memory leak | 
| JDK-8277489 | Rewrite JAAS UnixLoginModule with FFM | 
| JDK-8278102 | containers/docker/TestJcmd.java failed with "RuntimeException: Could not find specified process" | 
| JDK-8278591 | Jpackage post installation information message | 
| JDK-8278857 | C2: optimize (x << 2) & -4 to x (and similar patterns) → KnownBits for LShift | 
| JDK-8279196 | Test: jdk/jfr/event/gc/stacktrace/TestG1OldAllocationPendingStackTrace.java timed out | 
| JDK-8280283 | Dead compiler code found during the JDK-8272058 code review | 
| JDK-8284315 | DocTrees.getElement is inconsistent with Elements.getTypeElement | 
| JDK-8286032 | keytool -list -alias should not assume it is always a certificate | 
| JDK-8286258 | [Accessibility,macOS,VoiceOver] VoiceOver reads the spinner value wrong and sometime partially | 
| JDK-8287062 | com/sun/jndi/ldap/LdapPoolTimeoutTest.java failed due to different timeout message | 
| JDK-8290892 | C2: Intrinsify Reference.reachabilityFence | 
| JDK-8291986 | ProcessBuilder.redirectErrorStream(true) leaves error stream available | 
| JDK-8294152 | AArch64: frame::id() and frame::is_older() broken for interpreted frames with large max_stack | 
| JDK-8296183 | jndiprovider.properties contains properties pointing to non-existing classes | 
| JDK-8297191 | [macos] Printing a page range with starting page > 1 results in missing pages | 
| JDK-8298098 | Remove SCCS id’s from hotspot tests | 
| JDK-8303470 | containers/docker/TestMemoryAwareness.java failed with "'memory_limit_in_bytes:.*512000 k' missing from stdout/stderr" | 
| JDK-8305250 | Unnecessary "unknown enum constant" warning emitted by javac when dependency has optional annotations with enums | 
| JDK-8306591 | SA and hotspot generate different hprof records for GC roots | 
| JDK-8307537 | Print blobs/nmethods/adapters stats per code heap in CodeCache::print_summary | 
| JDK-8307574 | ClipIsRunningAfterStop.java failed with "../nptl/pthread_mutex_lock.c:81: *pthread_mutex_lock: Assertion `mutex→* data.__owner == 0' failed." | 
| JDK-8308395 | javax/sound/sampled/Clip/ClipFlushCrash.java timed out | 
| JDK-8308637 | AssertionError when using Trees.getScope in plug-in | 
| JDK-8308878 | com/sun/net/httpserver/simpleserver/OutputFilterTest.java fails with assertion errors in IPv6 only environment | 
| JDK-8309041 | Swingutilities2 methods requesting focus can have a Cause | 
| JDK-8309748 | Improve host selection in `External Specifications` page | 
| JDK-8310138 | Socket.isInputShutdown and isOutputShutdown don’t specify what is returned when socket is closed | 
| JDK-8312116 | GenShen: make instantaneous allocation rate triggers more timely | 
| JDK-8313713 | Allow CompileCommand flag to specify compilation level | 
| JDK-8314258 | Add integer_cast for checking conversions don’t change the value | 
| JDK-8314323 | Implement JEP 527: TLS 1.3 Hybrid Key Exchange | 
| JDK-8314599 | [GenShen] Couple adaptive tenuring and generation size budgeting | 
| JDK-8316439 | Several jvmti tests fail with -Xcheck:jni | 
| JDK-8316465 | 10 nsk debugger tests ignore VM flags while starting debugee | 
| JDK-8316466 | 8 com/sun/jdi tests ignore VM flags | 
| JDK-8317124 | use reproducible random in HotSpot tests | 
| JDK-8319161 | GC: Make TestParallelGCThreads use createTestJavaProcessBuilder | 
| JDK-8319326 | GC: Make TestParallelRefProc use createTestJavaProcessBuilder | 
| JDK-8319589 | Attach from root to a user java process not supported in Mac | 
| JDK-8320897 | compiler/vectorapi/reshape/TestVectorReinterpret.java fails on ppc64(le) platforms | 
| JDK-8322256 | Define and document GZIPInputStream concatenated stream semantics | 
| JDK-8323792 | ThreadSnapshot::initialize can cause assert in Thread::check_for_dangling_thread_pointer (possibility of dangling Thread pointer) | 
| JDK-8325108 | POSIX map_memory_to_file calls release_memory unnecessarily | 
| JDK-8326205 | Grouping frequently called C2 nmethods in CodeCache | 
| JDK-8326487 | ZipFileSystem.getPath("").getFileName() returns null instead of an empty Path | 
| JDK-8326547 | StrictMath.pow returns results outside of a 1 ulp error bound. | 
| JDK-8328046 | Need to keep leading zeros in TlsPremasterSecret of TLS1.3 DHKeyAgreement | 
| JDK-8328306 | AArch64: MacOS lazy JIT "write xor execute" switching | 
| JDK-8329548 | Change KeyUpdate messages from TLS 1.3 | 
| JDK-8330363 | JDI ThreadDeathEvent/thread/thread001 timed out with UT enabled | 
| JDK-8331994 | Adapt MAKEFLAGS check for GNU Make 4.4.1 | 
| JDK-8332189 | Enable -Wzero-as-null-pointer-constant for gcc/clang | 
| JDK-8332632 | Redundant assert "compiler should always document failure: %s" with possible UB | 
| JDK-8334403 | Unify behavior of os::committed_in_range across OSes | 
| JDK-8334549 | [Sound] Test timed out: javax/sound/sampled/Clip/OpenNonIntegralNumberOfSampleframes.java | 
| JDK-8334617 | Build always compiles gtest depending on Serial GC, failing build if disabled | 
| JDK-8334868 | Ensure CheckUninstallModalHook is called in WPageDialogPeer._show | 
| JDK-8335355 | Shenandoah: Fix race condition in gc/shenandoah/mxbeans/TestPauseNotifications.java | 
| JDK-8336654 | [lworld] Tests depending on sun.awt.AppContext can fail when run with migrated classes | 
| JDK-8337853 | Remove SunLayoutEngineKey and SunLayoutEngineFactory and its cache. | 
| JDK-8338594 | Update nsk/aod tests to use driver instead of othervm | 
| JDK-8339886 | runtime/os/TestHugePageDecisionsAtVMStartup.java ignores vm flags | 
| JDK-8340470 | G1: Adopt PartialArrayState to consolidate marking stack in Full GC | 
| JDK-8340830 | Console.readLine() and Console.printf() are mutually blocking | 
| JDK-8341272 | Factory to create wide iinc instruction with small arguments | 
| JDK-8341382 | EXCEPTION_ACCESS_VIOLATION in awt.dll after JDK-8185862 | 
| JDK-8341496 | Improve JMX connections | 
| JDK-8341630 | G1: Adopt PartialArrayState to consolidate marking stack in concurrent marking | 
| JDK-8342095 | Add autovectorizer support for subword vector casts | 
| JDK-8342772 | Assert in LateInlineMHCallGenerator::do_late_inline_check | 
| JDK-8342786 | Shenandoah: Emit AllocationRequiringGC jfr events | 
| JDK-8343606 | Test FinalizerTest.java intermittent fails Debuggee heap OOM | 
| JDK-8343809 | Add requires tag to mark tests that are incompatible with exploded image | 
| JDK-8344212 | Bring back @see GraphicsConfiguration to MouseInfo.getPointerInfo() | 
| JDK-8344345 | test/hotspot/gtest/x86/x86-asmtest.py has trailing whitespaces | 
| JDK-8345277 | TLSv1.2 - unable to sign a secp256r1 key with ecdsa_secp384r1_sha384 | 
| JDK-8345301 | GenShen: TestShenandoahEvacuationInformationEvent.java intermittent fails | 
| JDK-8345501 | GenShen: Test TestEvilSyncBug.java#generational intermittent timed out | 
| JDK-8345631 | TestRegionSamplingLogging.java#generational-rotation intermittent fails | 
| JDK-8345954 | Revisit Class Initializers and Locking in X509TrustManagerImpl | 
| JDK-8346133 | Refactor java.time.ZoneOffset caching | 
| JDK-8346154 | [XWayland] Some tests fail intermittently in the CI, but not locally | 
| JDK-8346256 | Optimize UMIN/UMAX reduction operations for x86 targets | 
| JDK-8346420 | C2: IfNode::fold_compares_helper() wrongly folds two CmpI nodes to a single CmpU node | 
| JDK-8347112 | Copy nested directories in doc-files by default | 
| JDK-8347248 | Rename arg_count to arg_size | 
| JDK-8347365 | C2: Fix the handling of depends_only_on_test | 
| JDK-8347396 | Efficient TypeFunc creations | 
| JDK-8347418 | ConsoleIOContext.countTrailintBackslashes causes NullPointerException | 
| JDK-8347462 | Improve TraceClassLoading to add link information | 
| JDK-8347938 | Add Support for the Latest ML-KEM and ML-DSA Private Key Encodings | 
| JDK-8348014 | Enhance certificate processing | 
| JDK-8348519 | [s390x] test failure TestVectorReinterpret.java | 
| JDK-8350208 | CTW: GraphKit::add_safepoint_edges asserts "not enough operands for reexecution" | 
| JDK-8350330 | C2: PhaseIdealLoop::add_parse_predicate() should mirror GraphKit::add_parse_predicate() | 
| JDK-8350481 | ServiceLoader throws NCDEF instead of ServiceConfigurationError | 
| JDK-8350603 | Several more compiler tests ignore vm flags and don’t have vm.flagless | 
| JDK-8350711 | [JMH] test Signatures.RSASSAPSS failed for 2 threads config | 
| JDK-8350971 | C2: assert(idx == alias_idx) failed: Following Phi nodes should be on the same memory slice | 
| JDK-8351847 | C2: Add "TraceSplitIf" flag to get more information about the optimization in Split-If | 
| JDK-8351889 | C2 crash: assertion failed: Base pointers must match (addp 344) | 
| JDK-8351892 | GenShen: Remove vestigial young generation sizing options | 
| JDK-8352567 | [s390x] ProblemList JFR tests requiring JFR stubs | 
| JDK-8352728 | InternalError loading java.security due to Windows parent folder permissions | 
| JDK-8352914 | Shenandoah: Change definition of ShenandoahSharedValue to int32_t to leverage platform atomics | 
| JDK-8353115 | GenShen: mixed evacuation candidate regions need accurate live_data | 
| JDK-8353290 | C2: Refactor PhaseIdealLoop::is_counted_loop() | 
| JDK-8353564 | Fail fatally if os::release_memory or os::uncommit_memory fails | 
| JDK-8353567 | Tighten NMT lock scope in os::release_memory and os::uncommit_memory | 
| JDK-8354282 | C2: more crashes in compiled code because of dependency on removed range check CastIIs | 
| JDK-8354853 | Clean up x86 registers after 32-bit x86 removal | 
| JDK-8354901 | javax/swing/JTable/8236907/LastVisibleRow.java fails on macosx-aarch64 due to difference in before and after image comparison | 
| JDK-8355522 | Remove the `java.locale.useOldISOCodes` system property | 
| JDK-8356116 | [macos] Add logging of sign commands in jpackage | 
| JDK-8356223 | ProblemList-AotJdk.txt should be added automatically if AOT mode is tested | 
| JDK-8356544 | Implement additional tests for ciphersuites disabled with wildcards | 
| JDK-8356684 | jpackage error messages are not helpful | 
| JDK-8357037 | Verifier rejects method ending in switch instruction | 
| JDK-8357089 | Remove VFORK launch mechanism from Process implementation (linux) | 
| JDK-8357258 | x86: Improve receiver type profiling reliability | 
| JDK-8357404 | jpackage should attempt to get a package version from the JDK’s release file if the --version option is not specified | 
| JDK-8357439 | Add bash autocompletion for jcmd | 
| JDK-8357471 | GenShen: Share collector reserves between young and old | 
| JDK-8357551 | RISC-V: support CMoveF/D vectorization | 
| JDK-8357593 | Crash in Matcher::init_first_stack_mask() during production run | 
| JDK-8358342 | G1: G1CodeRootSet performance breaks down on even moderate load | 
| JDK-8358521 | Optimize vector operations by reassociating broadcasted inputs | 
| JDK-8358560 | C2: convert some uses of GrowableArray<Node*> to Unique_Node_List | 
| JDK-8358766 | Improve detection of changes to print IGVN steps | 
| JDK-8358957 | [ubsan]: The assert in layout_helper_boolean_diffbit() in klass.hpp needs UB to fail | 
| JDK-8359115 | [s390x] Test CreateSymbolsReproducibleTest.java failure | 
| JDK-8359166 | C2 asserts in Assembler::addr_nop_5 with -XX:-UseAddressNop | 
| JDK-8359335 | Template-Framework Library: Primitive Types subtyping | 
| JDK-8359433 | The final modifier on Windows L&F internal UI classes prevents extending them in apps | 
| JDK-8359501 | Enhance Handling of URIs | 
| JDK-8359665 | JFR: Specification should use long and boolean, not Long and Boolean | 
| JDK-8359706 | Add file descriptor count to VM.info | 
| JDK-8360121 | Add -XX:+StressLoopPeeling to CTW | 
| JDK-8360160 | ubuntu-22-04 machine is failing client tests | 
| JDK-8360192 | C2: Make the type of count leading/trailing zero nodes more precise | 
| JDK-8360271 | String.indexOf intrinsics fail with +EnableX86ECoreOpts and -CompactStrings | 
| JDK-8360498 | [TEST_BUG] Some Mixing test continue to fail | 
| JDK-8360595 | OOM in java/foreign/TestUpcallStress.java | 
| JDK-8360654 | AArch64: Remove redundant dmb from C1 compareAndSet | 
| JDK-8360700 | Implement JEP 534: Compact Object Headers by Default | 
| JDK-8360934 | Add AVX-512 intrinsics for ML-KEM - enhancement on AVX512_VBMI | 
| JDK-8361493 | RepaintManager.invalidComponents uses different equality when searching element | 
| JDK-8362083 | JDI VirtualMachine/dispose/dispose001 failed with FATAL ERROR in native method: JDWP cannot set thread local storage, jvmtiError=JVMTI_ERROR_WRONG_PHASE(112) | 
| JDK-8362087 | Test containers/docker/ShareTmpDir.java intermittent fails | 
| JDK-8362239 | Reconcile enter_internal and reenter_internal in the ObjectMonitor code | 
| JDK-8362268 | NPE thrown from SASL GSSAPI impl when TLS is used with QOP auth-int against Active Directory | 
| JDK-8362308 | Enhance Bitmap operations | 
| JDK-8362350 | recompute_enable hit assertion "should never come here before live phase" if happens in post_vm_start | 
| JDK-8362632 | Improve HttpServer Request handling | 
| JDK-8362658 | sun/security/ssl/SSLEngineImpl/* tests duplicate jvm flags | 
| JDK-8363996 | Obsolete UseCompressedClassPointers | 
| JDK-8364121 | DES[ede]KeySpec do not consistently check for valid parameters | 
| JDK-8364148 | Test java/foreign/StdLibTest.java timed out after passing on linux-x64-zero | 
| JDK-8364167 | Test java/foreign/TestHandshake.java crashed with access violation in jdk.internal.misc.Unsafe.getShortUnaligned | 
| JDK-8364182 | Add jcmd VM.security_properties command | 
| JDK-8364214 | Enhance polygon data support | 
| JDK-8364373 | Transform Affine transformations | 
| JDK-8364393 | Allow templates to have # character without variable replacement | 
| JDK-8364465 | Enhance behavior of some intrinsics | 
| JDK-8364490 | Fatal error on large SpecTrapLimitExtraEntries value | 
| JDK-8364584 | Inconsistency in setting up cpu feature bits | 
| JDK-8364655 | Loading class with nested annotations causes stack overflow in VM | 
| JDK-8364766 | C2: Improve Value() of DivI and DivL for non-constant inputs | 
| JDK-8365058 | Enhance CopyOnWriteArraySet | 
| JDK-8365147 | AArch64: Replace DMB + LD + DMB with LDAR for C1 volatile field loads | 
| JDK-8365271 | Improve Swing supports | 
| JDK-8365280 | Enhance JOptionPane | 
| JDK-8365313 | GTK LaF does not respect system color scheme with Gnome | 
| JDK-8365381 | [asan] exclude tests under ASAN build which rely on vm signal handling | 
| JDK-8365498 | jdk/jfr/event/os/TestCPULoad.java fails with Expected at least one event | 
| JDK-8365570 | C2 fails assert(false) failed: Unexpected node in SuperWord truncation: CastII | 
| JDK-8365732 | RISC-V: implement AES CTR intrinsics | 
| JDK-8365883 | Fix P11Cipher to throw BadPaddingException for PKCS11 CKR_ENCRYPTED_DATA_INVALID error | 
| JDK-8366020 | Assert that java code is not executed during the AOT assembly phase | 
| JDK-8366041 | Shenandoah: Late Barrier Expansion | 
| JDK-8366058 | Outdated comment in WinCAPISeedGenerator | 
| JDK-8366138 | Parse::jump_switch_ranges() could cause stack overflow when compiling huge switch statement | 
| JDK-8366241 | NMT: Consolidate [Virtual/Committed/Reserved]Regions into one structure | 
| JDK-8366272 | The os::xxx APIs do not manage errno correctly | 
| JDK-8366441 | AArch64: Support WFET in OnSpinWait | 
| JDK-8366444 | Add support for add/mul reduction operations for Float16 | 
| JDK-8366446 | Test java/awt/geom/ConcurrentDrawPolygonTest.java fails intermittently | 
| JDK-8366457 | Add ResourceArea and Arena allocators for the RBTree | 
| JDK-8366478 | BigDecimal roots | 
| JDK-8366522 | CodeSource.getCodeSigners() throws NPE within empty certs | 
| JDK-8366659 | ObjectMonitor::wait() liveness problem with a suspension request | 
| JDK-8366671 | Refactor Thread::SpinAcquire and Thread::SpinRelease | 
| JDK-8366736 | Closed System.out causes child process to hang on Windows | 
| JDK-8366807 | JNI exception pending in Java_sun_security_pkcs11_wrapper_PKCS11_initializeLibrary of p11_general.c:106 | 
| JDK-8366861 | Phase AFTER_LOOP_OPTS printed even though the method has no loops | 
| JDK-8366957 | Amalloc may return null despite contrary AllocFailType | 
| JDK-8367024 | JNI exception pending in Java_sun_security_pkcs11_wrapper_PKCS11_C_1DeriveKey of p11_keymgmt.c:950 | 
| JDK-8367028 | compiler/c2/irTests/TestFloat16ScalarOperations.java failing intermittently because of constant folding | 
| JDK-8367129 | Move input validation checks to Java for java.lang.StringLatin1 intrinsics | 
| JDK-8367158 | C2: create better fill and copy benchmarks, taking alignment into account | 
| JDK-8367277 | Fix copyright header in JMXInterfaceBindingTest.java | 
| JDK-8367284 | (fs) Support current working directory target in SecureDirectoryStream.move | 
| JDK-8367319 | Add os interfaces to get machine and container values separately | 
| JDK-8367326 | Refactor PEM API tests requiring PEMRecord usage | 
| JDK-8367330 | Loosen container detection to include soft limits | 
| JDK-8367332 | Replace BlockTree tree logic with an intrusive red-black tree | 
| JDK-8367341 | C2: apply KnownBits and unsigned bounds to And / Or operations | 
| JDK-8367342 | Doclet documentation is incomplete | 
| JDK-8367399 | C2 SuperWord: add IR rules for MemorySegment cases from JDK-8329077 | 
| JDK-8367463 | Improved Arena allocations | 
| JDK-8367478 | Improve UseAVX setting and add cpu descriptions for zhaoxin processors | 
| JDK-8367530 | The exhaustiveness errors could be improved | 
| JDK-8367541 | Parallel: Make young and old generation fields nonstatic in ParallelScavengeHeap | 
| JDK-8367584 | Implement JEP 536: JFR In-Process Data Redaction | 
| JDK-8367627 | C2: Missed Ideal() optimization opportunity with MemBar | 
| JDK-8367761 | [VectorAPI] AssertionError with several vector API mathlib tests with MaxVectorSize=32 | 
| JDK-8367765 | G1: Merge G1UpdateRegionLivenessAndSelectForRebuild task and collection set choosing | 
| JDK-8367949 | JFR: MethodTrace double-counts methods that catch their own exceptions | 
| JDK-8367993 | G1: Speed up ConcurrentMark initialization | 
| JDK-8367994 | test/jdk/sun/security/pkcs11/Signature/ tests pass when they should skip | 
| JDK-8368032 | Enhance Certificate Checking | 
| JDK-8368041 | Enhance TLS certificate handling | 
| JDK-8368091 | Use JUnit Jupiter API in sun/net/ext, sun/net/www and sun/net/spi tests | 
| JDK-8368175 | Replace TEST_THAW_ONE_CHUNK_FRAME with develop flag | 
| JDK-8368493 | Disable most test JSSE debug output by default, and increase the test default maximum output log size | 
| JDK-8368524 | Tests are skipped and shown as passed in test/jdk/sun/security/pkcs11/Cipher/KeyWrap | 
| JDK-8368692 | Restrict Password::readPassword from reading from System.in | 
| JDK-8368701 | CDS VerifierTest_1A.java failed on machines with 512 GB RAM | 
| JDK-8368702 | [macosx] Printing text with composite fonts loses font transform | 
| JDK-8368841 | X25519 implementation differs from the specification in RFC 7748 | 
| JDK-8368864 | Confusing error message (or wrong error) when record component has @deprecated Javadoc tag | 
| JDK-8368977 | Provide clear naming for AVX10 identifiers | 
| JDK-8369020 | Test compiler/intrinsics/TestLongUnsignedDivMod.java completed and timed out | 
| JDK-8369048 | GenShen: Defer ShenFreeSet::available() during rebuild | 
| JDK-8369150 | NMethodRelocationTest fails when JVMTI events not published before JVM exit | 
| JDK-8369181 | InflaterOutputStream: writing after finish() results in IllegalStateException instead of an IOException | 
| JDK-8369205 | AIX build break in forbiddenFunctions.hpp | 
| JDK-8369227 | Virtual thread stuck in PARKED state | 
| JDK-8369282 | Distrust TLS server certificates anchored by Chunghwa ePKI Root CA | 
| JDK-8369327 | On macOS List loses selection when added to Frame | 
| JDK-8369393 | NMT: poison the malloc header and footer under ASAN build | 
| JDK-8369455 | XAWT: List.getSelectedItem may throw ArrayIndexOutOfBoundsException | 
| JDK-8369503 | [Linux] Move machine-specific queries to the OSContainer layer | 
| JDK-8369515 | Deadlock between JVMTI and JNI ReleasePrimitiveArrayCritical | 
| JDK-8369561 | sun/java2d/OpenGL/DrawBitmaskImage.java#id0: Incorrect color for first pixel (actual=ff000000) | 
| JDK-8369564 | Provide a MemorySegment API to read strings with known lengths | 
| JDK-8369575 | Enhance crypto algorithm support | 
| JDK-8369595 | HttpClient: HttpHeaders.firstValueAsLong failures should be converted to ProtocolException | 
| JDK-8369699 | Template Framework Library: add VectorAPI types and operations | 
| JDK-8369736 | Add management interface for AOT cache creation | 
| JDK-8369917 | LMS/HSS RFC 9858 Support | 
| JDK-8369950 | TLS connection to IPv6 address fails with BCJSSE due to IllegalArgumentException | 
| JDK-8370044 | TraceBytecodes shouldn’t break up lines | 
| JDK-8370102 | Print method signatures in edge cases when using TraceBytecodes | 
| JDK-8370112 | Remove VM_Version::supports_fast_class_init_checks() in platform-specific code | 
| JDK-8370138 | C2: clean up Matcher::use_asm_for_ldiv_by_con can | 
| JDK-8370198 | Test gc/arguments/TestShrinkHeapInSteps.java crashed: assert(left >= right) failed: avoid underflow | 
| JDK-8370200 | Crash: assert(outer→outcnt() >= phis + 2 - be_loads && outer→outcnt() ⇐ phis + 2 + stores + 1) failed: only phis | 
| JDK-8370249 | Test jdk/javadoc/doccheck/checks/jdkCheckLinks.java fails due to JVMS/JLS links after JDK-8349681 | 
| JDK-8370314 | Update signals_posix with new Linux signal codes | 
| JDK-8370416 | C2: Optimizing away arraycopy leads to wrong execution | 
| JDK-8370441 | Remove unnecessary/confusing null check in Verifier::verify() | 
| JDK-8370489 | Some compiler tests miss the @key randomness | 
| JDK-8370502 | C2: segfault while adding node to IGVN worklist | 
| JDK-8370504 | InterpreterMacroAssembler::profile_virtual_call parameter 'receiver_can_be_null' is unused | 
| JDK-8370519 | C2: Hit MemLimit when running with +VerifyLoopOptimizations | 
| JDK-8370529 | Enhance Path Factories Redux | 
| JDK-8370615 | Improve Kerberos credentialing | 
| JDK-8370648 | TestOldGrowthTriggers.java fails 'Trigger (Old): Old has overgrown' missing from stdout/stderr | 
| JDK-8370655 | Check EINTR handling InetAddress implementation and NET_ThrowNew | 
| JDK-8370656 | Re-examine use of InterruptedIOException in sun.security.ssl code | 
| JDK-8370666 | VectorAPI: Add clear comments for vector relative code in c2 | 
| JDK-8370688 | java.util.jar.JarEntry.getCodeSigners() and getCertificates() should specify that they return a copy of the arrays | 
| JDK-8370731 | Tests in vmTestbase/nsk/monitoring/GarbageCollectorMXBean/CollectionCounters/ failed: OutOfMemoryError | 
| JDK-8370846 | Support execution of mlvm testing with test thread factory | 
| JDK-8370863 | VectorAPI: Optimize the VectorMaskCast chain in specific patterns | 
| JDK-8370885 | Default namedGroups values are not being filtered against algorithm constraints | 
| JDK-8370890 | Start of release updates for JDK 27 | 
| JDK-8370893 | Add SourceVersion.RELEASE_27 | 
| JDK-8370894 | Add source 27 and target 27 to javac | 
| JDK-8370910 | Cleanup terminology of UUID vs Global Identifiers in UUID | 
| JDK-8370922 | Template Framework Library: Float16 type and operations | 
| JDK-8370947 | Mitigate Neoverse-N1 erratum 1542419 negative impact on GCs and JIT performance | 
| JDK-8370970 | DocCheck failure in jdkDoctypeBadcharsCheck.java and jdkCheckHtml.java | 
| JDK-8370986 | Enhance Zip file reading | 
| JDK-8370995 | Enhance ZipFile usage | 
| JDK-8371014 | Dump JFR recording on CrashOnOutOfMemoryError is incorrectly implemented | 
| JDK-8371155 | Type annotations on local variables are classified after the local var initializer has been type checked | 
| JDK-8371162 | Compiler warns about implicit cast from long to int in shift operation | 
| JDK-8371182 | [macos] Improve error messages for "invalid mac bundle identifier" | 
| JDK-8371187 | [BigEndian Platforms] Vector lane reversal error | 
| JDK-8371284 | GenShen: Avoid unnecessary card marking | 
| JDK-8371333 | Optimize static initialization of SSLContextImpl classes and improve logging | 
| JDK-8371347 | Move the ObjectMonitorTable to a separate new file | 
| JDK-8371408 | [Linux] VM.info output for container information is confusing | 
| JDK-8371409 | Wrong lock ordering between FullGCALot_lock and ThreadsLockThrottle_lock/MethodCompileQueue_lock | 
| JDK-8371420 | Still sporadic failures of gc/TestAlwaysPreTouchBehavior.java#<gcname> on Linux after JDK-8359104 | 
| JDK-8371438 | jpackage should handle the case when "--mac-sign" is specified without signing identity options | 
| JDK-8371446 | VectorAPI: Add unit tests for masks from various long values | 
| JDK-8371459 | [REDO] AArch64: Use SHA3 GPR intrinsic where it’s faster | 
| JDK-8371502 | serviceability/jvmti/vthread/ThreadListStackTracesTest/ThreadListStackTracesTest.java failing | 
| JDK-8371503 | RETAIN_IMAGE_AFTER_TEST do not work for some tests | 
| JDK-8371536 | C2: VerifyIterativeGVN should assert on first detected failure | 
| JDK-8371559 | Intermittent timeouts in test javax/net/ssl/Stapling/HttpsUrlConnClient.java | 
| JDK-8371590 | runtime/ErrorHandling/TestDwarf.java fails with clang toolchain | 
| JDK-8371596 | Nits and typos found during review of fix for JDK-8366659 | 
| JDK-8371603 | C2: Missing Ideal optimizations for load and store vectors on SVE | 
| JDK-8371651 | [AArch64] Populate CPU _features flag on Windows | 
| JDK-8371656 | HashMap.putAll() optimizations | 
| JDK-8371657 | [macosx] Programmatically selecting/deselecting List item triggers an ItemEvent | 
| JDK-8371683 | TYPE_USE annotation on var lambda parameter should be rejected | 
| JDK-8371685 | C2: Add flag to disable Loop Peeling | 
| JDK-8371716 | C2: Phi node fails Value()'s verification when speculative types clash | 
| JDK-8371718 | (sc) Channels.new{Input,Output}Stream can allocate unbounded memory for a socket channel | 
| JDK-8371721 | Refactor checkTrusted methods in X509TrustManagerImpl | 
| JDK-8371748 | Remove the (empty) ThreadPoolExecutor.finalize() method | 
| JDK-8371762 | Incorrect use of checked_cast in Arguments::process_settings_file | 
| JDK-8371777 | Clean up preferred address of G1’s archive region | 
| JDK-8371817 | javac with annotation processor throws AssertionError: Cannot add metadata to this type: METHOD when dealing with local classes | 
| JDK-8371830 | Enhance certificate chain validation | 
| JDK-8371842 | Update CLDR to Version 48.2 | 
| JDK-8371871 | libSharedCloseAgent.cpp crashes VS2019 and older VS2022 compiler | 
| JDK-8371873 | javac: U+001A (SUB / control-Z) after the last token makes the rest of the file silently ignored, which contradicts JLS §3.3 and §3.5 | 
| JDK-8371920 | [TEST] Enable CMove tests on other platforms | 
| JDK-8371924 | --mac-app-store should be accepted option for app image signing | 
| JDK-8371935 | Enhance key generation | 
| JDK-8371948 | TestStackOverflowDuringInit.java fails xss too small on linux-aarch64 | 
| JDK-8371949 | Upcall exception handler crashes with bad oop | 
| JDK-8371950 | The jdk.httpclient.keepalive.timeout system networking property does not specify the behaviour for values ⇐ 0 | 
| JDK-8371955 | Support AVX10 floating point comparison instructions | 
| JDK-8371968 | RISC-V: implement AES CBC intrinsics | 
| JDK-8371975 | Apply java.io.Serial annotations in java.security.sasl | 
| JDK-8371979 | Convert java/nio/file/FileStore/Basic.java to JUnit | 
| JDK-8372016 | G1: Root scanning not yielding to safepoints causes crashes | 
| JDK-8372040 | Remove Prefetch header vs inline header separation | 
| JDK-8372044 | Implementation Review Based on ML-KEM Security Considerations | 
| JDK-8372098 | Move AccessFlags to InstanceKlass | 
| JDK-8372231 | Test gtest/NMTGtests.java#nmt-summary crashed | 
| JDK-8372242 | Gtest LogTagSet.defaults should run in OTHER VM | 
| JDK-8372245 | GTest globalDefinitions.format_specifiers cannot run without VM | 
| JDK-8372246 | LogOutputList gtests should not use LogConfiguration LogOutputs | 
| JDK-8372247 | OSX: Semaphore.trywait requires os::Bsd::clock_init | 
| JDK-8372248 | GTest istream.coverage depends on istream.basic | 
| JDK-8372250 | Merge PtrQueue into SATBMarkQueue | 
| JDK-8372273 | C2: Extend the refactoring for IGV printing to include rest of the node properties | 
| JDK-8372301 | Improve error message for jimage command line tool regarding version mismatch | 
| JDK-8372302 | C2: IGVN verification fails because ModXNode::Ideal creates unused intermediate nodes | 
| JDK-8372321 | TestBackToBackSensitive fails intermittently after JDK-8365972 | 
| JDK-8372325 | Refactor tests under jdk/java/net/httpclient to use ${test.main.class} | 
| JDK-8372348 | Adjust some UL / JFR string deduplication output messages | 
| JDK-8372351 | Add 2 WISeKey roots | 
| JDK-8372353 | API to compute the byte length of a String encoded in a given Charset | 
| JDK-8372377 | Test java/io/File/GetXSpace.java failed: The system cannot find the path specified | 
| JDK-8372382 | Invalid RuntimeVisibleTypeAnnotations for compact record constructor | 
| JDK-8372442 | 2 Null pointer dereference defect groups in jvmtiExport.cpp | 
| JDK-8372493 | [asan] java/foreign/sharedclosejvmti/TestSharedCloseJvmti.java triggers heap-use-after-free | 
| JDK-8372498 | [genshen] gc/TestAllocHumongousFragment.java#generational causes intermittent SIGSEGV crashes | 
| JDK-8372526 | Add support for ZLIB TLS Certificate Compression | 
| JDK-8372543 | Shenandoah: undercalculated the available size when soft max takes effect | 
| JDK-8372546 | UnixMultiResolutionSplashTest.java fails intermittently on Ubuntu_24.04 | 
| JDK-8372554 | Test windows-x64-cmp-baseline failed due to differences with splashscreen object file | 
| JDK-8372555 | Test com/sun/jdi/ExceptionEvents.java failed: ObjectCollectedException | 
| JDK-8372564 | Convert StringDedup to use Atomic<T> | 
| JDK-8372589 | VM crashes on init when NonNMethodCodeHeapSize is set too small and UseTransparentHugePages is enabled | 
| JDK-8372591 | assert(!current→cont_fastpath() \|\| freeze.check_valid_fast_path()) failed | 
| JDK-8372592 | Adjust logger usage in java2d tests | 
| JDK-8372617 | Save and restore stubgen stubs when using an AOT code cache | 
| JDK-8372634 | C2: Materialize type information from instanceof checks | 
| JDK-8372635 | Lambdas do not copy over SYNTHETIC flag for local variables | 
| JDK-8372641 | [s390x] Test failure TestMergeStores.java | 
| JDK-8372646 | C2: Stress Counted Loop creation | 
| JDK-8372649 | C2 compilation fails with "there should be an oop in OopMap instead of a live raw oop at safepoint" | 
| JDK-8372661 | Add a null-safe static factory method to "jdk.test.lib.net.SimpleSSLContext" | 
| JDK-8372688 | Test out-of-bounds indexing for List.ofLazy()::get | 
| JDK-8372696 | Allow boot classes to explicitly opt-in for final field trusting | 
| JDK-8372731 | Detailed authentication failure messages | 
| JDK-8372746 | Some httpserver files could benefit from some formatting cleanup | 
| JDK-8372754 | Add wrapper for <cstdlib> | 
| JDK-8372759 | Test build/AbsPathsInImage.java fails after JDK-8370438 | 
| JDK-8372797 | [VectorAPI] Missing Min/Max identity transforms | 
| JDK-8372801 | tools/sincechecker/modules/java.base/JavaBaseCheckSince.java fails with JDK 27 | 
| JDK-8372817 | Remove test/jdk/sun/security/rsa silent skips | 
| JDK-8372844 | Improve usage of test/jdk/java/text/testlib/TestUtils.java locale methods | 
| JDK-8372860 | TestCodeCacheUnloadDuringConcCycle fails on ARM32 | 
| JDK-8372942 | AArch64: Set JVM flags for Neoverse V3AE core | 
| JDK-8372948 | Store end positions directly in JCTree | 
| JDK-8372950 | Pem.pemEncoded should cache the Pattern | 
| JDK-8372974 | Add missing @Override annotations in "com.sun.java.swing.plaf.gtk" package | 
| JDK-8372978 | [VectorAPI] Fix incorrect identity values in UMIN/UMAX reductions | 
| JDK-8372980 | [VectorAPI] AArch64: Add intrinsic support for unsigned min/max reduction operations | 
| JDK-8372988 | Test runtime/Nestmates/membership/TestNestHostErrorWithMultiThread.java failed: Unexpected interrupt | 
| JDK-8373001 | LauncherFromOptions.create() not properly handling FileAssociationNoExtensionsException | 
| JDK-8373016 | Passes instead of skips/ignoring the platform in sun/security/mscapi, jarsigner, keytool and pkcs12 tests | 
| JDK-8373018 | Update OpenSSL version to 3.5.4 | 
| JDK-8373021 | aarch64: MacroAssembler::arrays_equals reads out of bounds | 
| JDK-8373022 | serviceability/sa/ClhsdbScanOops.java assumes no GC should occur | 
| JDK-8373025 | test/hotspot/jtreg/gc/cslocker/TestCSLocker.java may deadlock | 
| JDK-8373026 | C2 SuperWord and Vector API: vector algorithms test and benchmark | 
| JDK-8373039 | Remove Incorrect Asserts in shenandoahScanRemembered | 
| JDK-8373040 | Mark compiler/codecache/CodeCacheSegmentSizeTest.java as flagless | 
| JDK-8373041 | Mark gc/g1/TestCodeCacheUnloadDuringConcCycle.java as flagless | 
| JDK-8373054 | Shenandoah: Remove unnecessary BarrierSetNMethod::arm in shenandoahCodeRoots | 
| JDK-8373056 | Shenandoah: Remove unnecessary use of ShenandoahAllocRequest.type() | 
| JDK-8373059 | Test sun/security/provider/acvp/ML_DSA_Intrinsic_Test.java should pass on Aarch64 | 
| JDK-8373065 | Test sun/awt/image/bug8038000.java failed: Pixels differ | 
| JDK-8373068 | Revisit details of Float16 to decimal conversion algorithm | 
| JDK-8373069 | RISC-V: implement GHASH intrinsic | 
| JDK-8373086 | Make isexceeded001.java more robust | 
| JDK-8373087 | Parallel: Rename PSGenerationPool to PSOldGenerationPool | 
| JDK-8373094 | javac may fail because of unattributed break in a loop | 
| JDK-8373097 | Save command should create missing parent directories | 
| JDK-8373099 | Problem list intermittently failing test sun/awt/image/bug8038000.java | 
| JDK-8373100 | Genshen: Control thread can miss allocation failure notification | 
| JDK-8373101 | JdkClient and JdkServer test classes ignore namedGroups field | 
| JDK-8373102 | com/sun/jdi/MethodInvokeWithTraceOnTest.java can fail with ObjectCollectedException when run with a small heap | 
| JDK-8373105 | Test tools/jpackage/share/AsyncTest.java failed: problem running hdiutil | 
| JDK-8373106 | JFR suspend/resume deadlock on macOS in pthreads library | 
| JDK-8373110 | jstack --mixed frames are broken on macOS after JDK-8371194 | 
| JDK-8373111 | Test java/lang/management/MemoryMXBean/MemoryManagement.java timed out | 
| JDK-8373113 | Fix whitespace in RunTests.gmk | 
| JDK-8373116 | Genshen: arraycopy_work should be always done for arrays in old gen during young concurrent marking | 
| JDK-8373117 | Update build doc link in README.md | 
| JDK-8373119 | JDK 26 RDP1 L10n resource files update | 
| JDK-8373120 | Virtual thread stuck in BLOCKED state | 
| JDK-8373122 | JFR build failure with CDS disabled due to -Werror=unused-function after JDK-8365400 | 
| JDK-8373125 | Add defensive screening of modifiers for Field and Parameter toString() results | 
| JDK-8373127 | Update nsk/monitoring tests to support virtual thread factory testing | 
| JDK-8373134 | C2: Min/Max users of Min/Max uses should be enqueued for GVN | 
| JDK-8373143 | C2: verify that adr_type and adr’s type match in LoadNode::make and StoreNode::make | 
| JDK-8373145 | [BACKOUT] Remove ThreadLocalAllocBuffer::_reserve_for_allocation_prefetch | 
| JDK-8373207 | Make DeferredStatic class template constant initializable | 
| JDK-8373208 | Make Atomic class template constant initializable | 
| JDK-8373225 | GenShen: More adaptive old-generation growth heuristics | 
| JDK-8373227 | Test java/net/httpclient/http2/StreamFlowControlTest.java failed: should sleep time be raised? | 
| JDK-8373231 | ECDSAOperations::toAffinePoint is redundant | 
| JDK-8373239 | Test java/awt/print/PrinterJob/PageRanges.java fails with incorrect selection of printed pages | 
| JDK-8373246 | JDK-8351842 broke native debugging on Linux | 
| JDK-8373248 | C2: CastPP should not change the type of the oop | 
| JDK-8373251 | C2: Ideal() returns nullptr for shift nodes after having modified the shift amount input | 
| JDK-8373253 | Re-work InjectGCWorkerCreationFailure for future changes | 
| JDK-8373255 | Unexpected iobj and ipdb files after JDK-8370438 | 
| JDK-8373262 | Parallel: gc/metaspace/CompressedClassSpaceSizeInJmapHeap.java fails | 
| JDK-8373266 | Strengthen constant CardTable base accesses | 
| JDK-8373270 | GCC 14.2.0 reports warning: '%s' directive output may be truncated | 
| JDK-8373272 | Genshen: ShenandoahOldGenerationTest fails after JDK-8373056 | 
| JDK-8373275 | Improve DTLS handshaking | 
| JDK-8373285 | Update JCov for class file version 71 | 
| JDK-8373289 | Test tools/javac/patterns/Exhaustiveness.java timed out then passed | 
| JDK-8373290 | Update FreeType to 2.14.1 | 
| JDK-8373293 | Change the exception handling in TestNestHostErrorWithMultiThread.java | 
| JDK-8373295 | Wrong log tag for UseCompressedOops ergo setting | 
| JDK-8373297 | Test com/sun/jdi/AfterThreadDeathTest.java failed with unexpected ObjectCollectedException | 
| JDK-8373298 | Test java/foreign/TestBufferStackStress.java timed out then completed | 
| JDK-8373335 | Serial: Clean up SerialHeap members by access specifies | 
| JDK-8373343 | C2: verify AddP base input only set for heap addresses | 
| JDK-8373355 | C2: CompileCommand PrintIdealPhase should also print nodes that are not "reachable from below" | 
| JDK-8373362 | Http3TestServer should not log an exception stack trace when it is stopping normally | 
| JDK-8373366 | HandshakeState should disallow suspend ops for disabler threads | 
| JDK-8373367 | interp-only mechanism fails to work for carrier threads in a corner case | 
| JDK-8373369 | [REDO] Remove ThreadLocalAllocBuffer::_reserve_for_allocation_prefetch | 
| JDK-8373388 | Reenable LTO for libsplashscreen | 
| JDK-8373389 | Two jdk/incubator/vector/ tests fails after JDK-8371446 | 
| JDK-8373392 | Replace CDS object subgraphs with @AOTSafeClassInitializer | 
| JDK-8373396 | Min and Max Ideal missing AddNode::Ideal optimisations | 
| JDK-8373403 | [TESTBUG] TestG1ClassUnloadingHWM.java could fail with large G1HeapRegionSize and small InitialHeapSize | 
| JDK-8373408 | SHA1withECDSA is not required for ECDHE and ECDSA | 
| JDK-8373409 | java/net/httpclient/http3/H3ErrorHandlingTest.java failed due to deadlock | 
| JDK-8373411 | Crash when PrintSharedArchiveAndExit is enabled but shared heap is disabled | 
| JDK-8373420 | C2: Add true/false_proj*() methods for IfNode as a replacement for proj_out*(true/false) | 
| JDK-8373421 | Parallel: Rename young generation eden and survivor space pool | 
| JDK-8373426 | Remove ffdhe6144 and ffdhe8192 from default list of TLS named groups | 
| JDK-8373427 | StructuredTaskScope::join not clear if called with interrupted status set | 
| JDK-8373428 | Refine variables with the same name in nested scopes in PhaseChaitin::gather_lrg_masks | 
| JDK-8373429 | gc/g1/TestCodeCacheUnloadDuringConcCycle fails on various platforms | 
| JDK-8373439 | Deadlock between flight recorder & VM shutdown | 
| JDK-8373441 | Remove DCmdFactory::_enabled | 
| JDK-8373443 | Update --release 26 symbol information for JDK 26 build 27 | 
| JDK-8373446 | Update --release 26 symbol information for JDK 26 build 29 | 
| JDK-8373447 | Suspicious sign extension after integer promotion in imageDecompressor.cpp | 
| JDK-8373448 | jpackage: StackOverflowError when processing a very long argument | 
| JDK-8373449 | Parallel: Obsolete deprecated PSChunkLargeArrays | 
| JDK-8373453 | C2 SuperWord: must handle load slices that have loads with different memory inputs | 
| JDK-8373464 | Test JdkManagementCheckSince.java fails after JDK-8369736 | 
| JDK-8373474 | 2 Unintentional format string defect groups in jabswitch.cpp | 
| JDK-8373475 | Unintentional format string in logString of AccessInfo.cpp | 
| JDK-8373476 | (tz) Update Timezone Data to 2025c | 
| JDK-8373481 | Remove the deprecated -noclassgc, -noverify, -Xverify:none and -verifyremote options from the java launcher | 
| JDK-8373485 | JFR Crash during sampling: assert(jt→has_last_Java_frame()) failed: invariant | 
| JDK-8373487 | Out-of-bounds access in AlignmentGapAccess test | 
| JDK-8373490 | JFR Leak Profiler: path-to-gc-root very slow for large object arrays | 
| JDK-8373497 | SpinCriticalSection should use SpinYield | 
| JDK-8373502 | C2 SuperWord: speculative check uses VPointer variable was pinned after speculative check, leading to bad graph | 
| JDK-8373508 | C2: sinking CreateEx out of loop breaks the graph | 
| JDK-8373513 | C2: Move ProjNode::other_if_proj() to IfProjNode | 
| JDK-8373515 | Migrate "test/jdk/java/net/httpclient/" to null-safe "SimpleSSLContext" methods | 
| JDK-8373517 | Revert the macos Tahoe specific change done in JDK-8359830 | 
| JDK-8373521 | Bump minimum boot jdk to JDK 26 | 
| JDK-8373522 | Remove expired flags in JDK 27 | 
| JDK-8373524 | C2: no reachable node should have no use | 
| JDK-8373525 | C2: assert(_base == Long) failed: Not a Long | 
| JDK-8373526 | Add tooltips to API documentation elements | 
| JDK-8373537 | Migrate "test/jdk/com/sun/net/httpserver/" to null-safe "SimpleSSLContext" methods | 
| JDK-8373538 | Migrate all tests to null-safe "SimpleSSLContext" methods | 
| JDK-8373552 | ExactConversionsSupport: bad JLS links in javadoc | 
| JDK-8373557 | Remove stale comments after JDK-8372584 | 
| JDK-8373561 | Replace usages of -verify java launcher option with -Xverify:all JVM option | 
| JDK-8373566 | Performance regression with java.text.MessageFormat subformat patterns | 
| JDK-8373570 | Javac stack overflow on method-local class with nested record referring to enclosing type | 
| JDK-8373577 | C2: Cleanup adr_type of CallLeafPureNode | 
| JDK-8373579 | Problem list compiler/runtime/Test7196199.java | 
| JDK-8373591 | C2: Fix the memory around some intrinsics nodes | 
| JDK-8373593 | Support latest VS2026 MSC_VER in abstract_vm_version.cpp | 
| JDK-8373595 | A new ObjectMonitorTable implementation | 
| JDK-8373599 | Cleanup arguments.hpp includes | 
| JDK-8373604 | Operations on peer reset tokens are slow | 
| JDK-8373615 | Improve HotSpot debug functions findclass() and findmethod | 
| JDK-8373623 | Refactor Serialization tests for Records to JUnit | 
| JDK-8373625 | CPUTimeCounters creates a total counter for unsupported GCs | 
| JDK-8373626 | [asan] read past end of buffer in sun.awt.image.ImagingLib.convolveBI | 
| JDK-8373627 | assert(!is_vthread_transition_disabler()) failed: no suspend allowed for vthread transition disablers | 
| JDK-8373628 | jpackage doesn’t print to console until completetion | 
| JDK-8373630 | r18_tls should not be modified on Windows AArch64 | 
| JDK-8373631 | Improve classes in the "jdk.jpackage.internal.util.function" package | 
| JDK-8373632 | Some sound tests failing in CI due to lack of sound key | 
| JDK-8373635 | C2: Wrong constant in GraphKit::basic_plus_adr() | 
| JDK-8373638 | RBTree public interface does not check all input parameters for validity | 
| JDK-8373642 | Test vmTestbase/nsk/monitoring/GarbageCollectorMXBean/CollectionCounters/CollectionCounters003/TestDescription.java failed | 
| JDK-8373643 | Test serviceability/jvmti/vthread/ThreadListStackTracesTest/ThreadListStackTracesTest.java still failing | 
| JDK-8373645 | Test vmTestbase/nsk/jvmti/scenarios/events/EM02/em02t003/TestDescription.java failed: Unexpected event JVMTI_EVENT_COMPILED_METHOD_LOAD | 
| JDK-8373647 | Avoid fstat when opening file for write with RandomAccessFile or FileOutputStream | 
| JDK-8373649 | Convert simple AtomicAccess usage in ConcurrentHashTable to use Atomic<T> | 
| JDK-8373650 | Test "javax/swing/JMenuItem/6458123/ManualBug6458123.java" fails because the check icons are not aligned properly as expected | 
| JDK-8373654 | Tests in sources/ should only run once | 
| JDK-8373668 | Add override keyword to *Klass classes | 
| JDK-8373676 | Test javax/net/ssl/HttpsURLConnection/SubjectAltNameIP.java fails on a machine without IPV6 | 
| JDK-8373677 | Clear text HttpServer connection could fail fast if receiving SSL ClientHello | 
| JDK-8373679 | Link color accessibility issue in dark theme | 
| JDK-8373682 | Test compiler/loopopts/superword/TestReinterpretAndCast.java fails on x86_64 with AVX but without f16c | 
| JDK-8373690 | Unexpected Keystore message using jdk.crypto.disabledAlgorithms | 
| JDK-8373695 | G1: Using a value near integer max for ActiveProcessorCount causes fatal crash | 
| JDK-8373699 | JLink: ModuleReader should be closed in JlinkTask.getReleaseInfo(mref) | 
| JDK-8373704 | Improve "SocketException: Protocol family unavailable" message | 
| JDK-8373710 | Improve jpackage error reporting | 
| JDK-8373714 | Shenandoah: Register heuristic penalties following a degenerated GC | 
| JDK-8373716 | Refactor further java/util tests from TestNG to JUnit | 
| JDK-8373718 | jdk/internal/misc/VM/RuntimeArguments.java test fails in Virtual threads mode | 
| JDK-8373720 | GenShen: Count live-at-old mark using Snapshot at Beginning | 
| JDK-8373722 | [TESTBUG] compiler/vectorapi/TestVectorOperationsWithPartialSize.java fails intermittently | 
| JDK-8373723 | Deadlock with JvmtiTagMap::flush_object_free_events() | 
| JDK-8373724 | Assertion failure in TestSignumVector.java with UseAPX | 
| JDK-8373727 | New XBM images parser regression: only the first line of the bitmap array is parsed | 
| JDK-8373731 | C2: Missed optimization opportunity for AddI | 
| JDK-8373778 | java.util.NoSuchElementException in HttpURLConnection.doTunneling0 when connecting via HTTPS | 
| JDK-8373789 | No PCH release build failure after JDK-8372543 | 
| JDK-8373793 | TestDynamicStore.java '/manual' disables use of '/timeout' | 
| JDK-8373796 | Refactor java/net/httpclient/ThrowingPublishers*.java tests to use JUnit5 | 
| JDK-8373798 | Refactor java/math tests to use JUnit | 
| JDK-8373800 | Remove ScopedValueBindingsResolver | 
| JDK-8373801 | Adopt arraycopy OopCopyResult from the lworld branch | 
| JDK-8373807 | test/jdk/java/net/httpclient/websocket/DummyWebSocketServer.java getURI() uses "localhost" | 
| JDK-8373808 | Refactor java/net/httpclient qpack and hpack tests to use JUnit | 
| JDK-8373819 | Genshen: Control thread can miss allocation failure notification (redux) | 
| JDK-8373820 | C2: Robust Node::uncast_helper infinite loop check | 
| JDK-8373829 | Refactor test/jdk/java/time/tck tests to use JUnit over TestNG | 
| JDK-8373830 | Refactor test/jdk/java/time/test tests to use JUnit over TestNG | 
| JDK-8373832 | Test java/lang/invoke/TestVHInvokerCaching.java tests nothing | 
| JDK-8373833 | "error.cert.not.found" and "error.explicit-sign-no-cert" errors duplicate each other | 
| JDK-8373836 | add anchors to the java options in the java man page | 
| JDK-8373837 | "assert(thread→is_VM_thread() \|\| thread→is_Java_thread()) failed: Must be VMThread or JavaThread" on macosx-aarch64-26 | 
| JDK-8373839 | Disable JVM TI suspension during JNI critical regions | 
| JDK-8373847 | Test javax/swing/JMenuItem/MenuItemTest/bug6197830.java failed because The test case automatically fails when clicking any items in the Nothing menu in all four windows (Left-to-right)-Menu Item Test and (Right-to-left)-Menu Item Test | 
| JDK-8373866 | Refactor java/net/httpclient/ThrowingSubscribers*.java tests to use JUnit5 | 
| JDK-8373867 | Improve robustness of Attach API for finding tmp directory | 
| JDK-8373869 | Refactor java/net/httpclient/ThrowingPushPromises*.java tests to use JUnit5 | 
| JDK-8373876 | StackWalkNativeToJava print more output in case of failures | 
| JDK-8373877 | QUIC connections are removed too early | 
| JDK-8373887 | jpackage tests may potentially deadlock | 
| JDK-8373893 | Refactor networking http server tests to use JUnit | 
| JDK-8373894 | G1: Count evacuation-failed garbage collections in gc cpu usage | 
| JDK-8373898 | RepeatCompilation does not repeat compilation after bailout | 
| JDK-8373909 | JSpec and ToolGuide taglets use incorrect relative path | 
| JDK-8373913 | Refactor serialization tests to use JUnit | 
| JDK-8373917 | test/hotspot/jtreg/vmTestbase/nsk/monitoring: -iterations setting misused in tests | 
| JDK-8373922 | Enhance Taglet API to support relative URLs | 
| JDK-8373924 | Remove unreferenced ImageDecompressor::image_decompressor_close | 
| JDK-8373928 | 4 Dangling pointer defect groups in java.c | 
| JDK-8373931 | Test javax/sound/sampled/Clip/AutoCloseTimeCheck.java timed out | 
| JDK-8373935 | Migrate java/lang/invoke tests away from TestNG | 
| JDK-8373941 | Epsilon: Robust counter updates in early VM phases | 
| JDK-8373944 | ObjectMonitor::ExitOnSuspend can call java_lang_VirtualThread::set_onWaitingList() while in safepoint | 
| JDK-8373945 | Use WB.fullGC() in ClassUnloader.unloadClass to force GC for vmTestbase tests | 
| JDK-8373946 | Synth ProgressBarUI implementation confuses background painting with border painting | 
| JDK-8373967 | [macos] User interactions with List do not trigger ItemEvent after programmatic change | 
| JDK-8373983 | java/util/Locale/UseOldISOCodesTest.java fails with JTREG_AOT_JDK=onestep | 
| JDK-8373984 | Check for macos 11 in CGraphicsDevice.m can be removed | 
| JDK-8373998 | RISC-V: simple optimization of ConvHF2F | 
| JDK-8374001 | sun/security/ skip without Exceptions | 
| JDK-8374020 | Inconsistent handling of type updates in typeWithAnnotations | 
| JDK-8374038 | JDI EventRequestManager javadoc has unrendered @link tags inside an @code block | 
| JDK-8374043 | C2: assert(_base >= VectorMask && _base ⇐ VectorZ) failed: Not a Vector | 
| JDK-8374044 | Docs build fails: Cannot determine current path | 
| JDK-8374048 | Genshen: Backout fix for missed cancellation notice | 
| JDK-8374051 | Incorrect parameterized testing of exceptions in AbstractDateTimeTest.java | 
| JDK-8374052 | Use java/util/Calendar in ReplaceCriticalClasses.java test | 
| JDK-8374056 | RISC-V: Fix argument passing for the RiscvFlushIcache::flush | 
| JDK-8374058 | Enhance JPEG handling | 
| JDK-8374078 | C2_MacroAssembler::verify_int_in_range has incorrect early return condition | 
| JDK-8374082 | net_util.c should use logical && when setting IPv6_available | 
| JDK-8374113 | Taughtological if check in Reflection::array_set | 
| JDK-8374145 | Remove legacy locking remnants from markWord | 
| JDK-8374151 | Cleanup minor markWord function disorder | 
| JDK-8374164 | Potential null pointer dereferences in regeneratedClasses.cpp | 
| JDK-8374168 | Resolve disabled warnings in JDWP agent | 
| JDK-8374170 | I/O Poller updates | 
| JDK-8374176 | Update --release 26 symbol information for JDK 26 build 32 | 
| JDK-8374177 | Update @since of HotSpotAOTCacheMXBean after JDK-8373607 | 
| JDK-8374178 | Missing include in systemDictionary.cpp after JDK-8365526 | 
| JDK-8374180 | C2 crash in PhaseCCP::verify_type - fatal error: Not monotonic | 
| JDK-8374181 | failure_handler: The cores.html file is formatted incorrectly and so hides the core dump information | 
| JDK-8374190 | Convert ConcurrentHashTable atomic lists to use Atomic<T> | 
| JDK-8374191 | Shenandoah: Consider an assert in get_object_age | 
| JDK-8374195 | TestReplaceNarrowPhiWithBottomPhi fails on ppc64 platforms in (fast)debug | 
| JDK-8374200 | jdk/internal/platform/cgroup/TestCgroupMetrics.java fails with common prefix metrics | 
| JDK-8374202 | Simplify significand normalization in BigDecimal(double, MathContext) constructor | 
| JDK-8374210 | [BACKOUT] Move input validation checks to Java for java.lang.StringCoding intrinsics | 
| JDK-8374215 | [macos] Clean and fix "lic_template.plist" to correctly work with multiple languages | 
| JDK-8374216 | Assorted changes to jpackage without functional impact | 
| JDK-8374217 | Remove IO.java test from AOT ProblemList | 
| JDK-8374219 | Fix issues in jpackage’s Executor class | 
| JDK-8374222 | jpackage will exit with error if it fails to clean the temp directory | 
| JDK-8374232 | Comment cleanup in diagnosticCommand.cpp | 
| JDK-8374233 | Overloaded constructor MountUnmountDisabler(jthread thread) is missed | 
| JDK-8374293 | Jshell throws an error and crashes when using keyword Public | 
| JDK-8374296 | Comment clean up in os_linux.cpp | 
| JDK-8374304 | MultiResolutionSplashTest.java fails in CI: "Image with wrong resolution is used for splash screen!" | 
| JDK-8374308 | ImageBufferCache has no effect and can be removed | 
| JDK-8374310 | Update copyright year to 2025 for client-libs in files where it was missed | 
| JDK-8374316 | Update copyright year to 2025 for hotspot in files where it was missed | 
| JDK-8374317 | Change GCM IV size to 12 bytes when encrypting/decrypting TLS session ticket | 
| JDK-8374321 | Fix undefined reference to 'png_init_filter_functions_lsx' after 8371914 | 
| JDK-8374322 | TestMemoryWithSubgroups.java fails Permission denied | 
| JDK-8374323 | Update copyright year to 2025 for the build system in files where it was missed | 
| JDK-8374326 | Update copyright year to 2025 for jdk.jpackage in files where it was missed | 
| JDK-8374327 | Update copyright year to 2025 for files in java.base added/updated by commits in 2025 | 
| JDK-8374328 | Convert simple AtomicAccess uses in gc/shared to use Atomic<T> | 
| JDK-8374340 | FontRenderContext instance variables should be final | 
| JDK-8374343 | Fix SIGSEGV when lib/modules is unreadable | 
| JDK-8374345 | Restore the original copyright year in ExtremeFontSizeTest.java | 
| JDK-8374348 | Implement -XX:AOTMode=required as alias for -XX:AOTMode=on | 
| JDK-8374349 | [VectorAPI]: AArch64: Prefer merging mode SVE CPY instruction | 
| JDK-8374350 | Convert hotspot gtests to use Atomic<T> | 
| JDK-8374351 | RISC-V: Small refactoring for crypto macro-assembler routines | 
| JDK-8374352 | Update copyright year to 2025 for test/langtools/tools/javac/ in files where it was missed | 
| JDK-8374354 | Update copyright year to 2025 for jdk.javadoc in files where it was missed | 
| JDK-8374355 | Update copyright year to 2025 for demo in files where it was missed | 
| JDK-8374358 | Update copyright year to 2025 for test/hotspot in files where it was missed | 
| JDK-8374360 | Update copyright year to 2025 for test/jdk/jdk/jfr in files where it was missed | 
| JDK-8374361 | Update copyright year to 2025 for jdk.hotspot.agent in files where it was missed | 
| JDK-8374363 | Update copyright year to 2025 for test/micro in files where it was missed | 
| JDK-8374365 | Update copyright year to 2025 for test/jdk in files where it was missed | 
| JDK-8374371 | Failed assertion in G1HeapRegion gtest | 
| JDK-8374373 | Typo in VirtualThreadSchedulerMXBean.setParallelism javadoc | 
| JDK-8374377 | PNGImageDecoder Slow For 8-bit PNGs | 
| JDK-8374378 | Update copyright year to 2025 for jdk.internal.vm.ci in files where it was missed | 
| JDK-8374379 | Type annotation in new array dimension expression causes java.lang.AssertionError | 
| JDK-8374382 | (aio) AsynchronousFileChannel writes wrong content using heap ByteBuffer when position != 0 | 
| JDK-8374383 | Update the copyright year to 2025 in the remaining files under test/ where it was missed | 
| JDK-8374391 | Update the copyright year to 2025 in the remaining files under src/ where it was missed | 
| JDK-8374397 | test/hotspot/jtreg/gc/shenandoah/options/TestPassiveModeWithCardBarrier.java fails with -Xcomp after JDK-8369013 | 
| JDK-8374432 | TimeoutResponseBodyTest.java#retriesEnabledForResponseFailure fails run with -Xcomp | 
| JDK-8374433 | java/util/Locale/PreserveTagCase.java does not run any tests | 
| JDK-8374434 | Several JShell tests report JUnit discovery warnings | 
| JDK-8374435 | assert(addp→is_AddP()) failed: must be AddP during EA with -XX:-UseCompressedOops | 
| JDK-8374436 | compiler/igvn/IntegerDivValueTests.java failed with division by zero | 
| JDK-8374441 | (fs) FileSystemProvider.readAttributesIfExists throws "Not a directory" when element in path is not directory should return null for ENOTDIR (unix) | 
| JDK-8374444 | Fix simple -Wzero-as-null-pointer-constant warnings | 
| JDK-8374445 | Fix -Wzero-as-null-pointer-constant warnings in JfrSet | 
| JDK-8374446 | Fix -Wzero-as-null-pointer-constant warnings in test_compressedKlass.cpp | 
| JDK-8374449 | Shenandoah: Leaf locks used by Shenandoah need lower ranks | 
| JDK-8374450 | GTest opto.canonicalize_constraints cannot run without VM | 
| JDK-8374454 | Test sun/security/ssl/CipherSuite/DisabledCipherSuitesNotNegotiated.java from JDK-8356544 shows intermittent timeouts | 
| JDK-8374456 | JVM crashes with "assert(resolved_method→method_holder()→is_linked()) failed: must be linked" when run with large value for PreallocatedOutOfMemoryErrorCount | 
| JDK-8374465 | Spurious dot in documentation for JVMTI ClassLoad | 
| JDK-8374467 | Incorrect ranges in jdk.internal.util.ByteArray JavaDoc | 
| JDK-8374469 | Mixed jstack does not work on Windows | 
| JDK-8374471 | Check bin and lib folder of JDK image for unwanted files | 
| JDK-8374482 | SA does not handle signal handler frame in mixed jstack | 
| JDK-8374483 | Eliminate :serviceability_ttf_virtual group and mark svc non-virtual tests with requires | 
| JDK-8374489 | Template Library: need to tag Float16.float16ToRawShortBits as having non-deterministic result because of multiple NaN bit patterns | 
| JDK-8374490 | Test jdk/jfr/event/runtime/TestSafepointEvents.java failed: Expected event 'jdk.SafepointStateSynchronization' to be present | 
| JDK-8374493 | Add missing @Override annotations in "com.sun.java.swing.plaf.motif" package | 
| JDK-8374496 | C2: assert(val→find_edge(con) > 0) failed | 
| JDK-8374497 | C2: assert(verify(phase)) failed: missing Value() optimization with -XX:+StressReflectiveCode | 
| JDK-8374506 | Incorrect positioning of arrow icon in parent JMenu in Windows L&F | 
| JDK-8374507 | GHA: Limit debug symbols generation to conserve disk space | 
| JDK-8374513 | AArch64: Improve receiver type profiling reliability | 
| JDK-8374516 | -version asserts with "-XX:+UseAESCTRIntrinsics -XX:-UseAES": "need AES instructions and misaligned SSE support" in generate_counterMode_AESCrypt_Parallel() | 
| JDK-8374518 | C1: Remove dead LinearScanStatistic::Counter::counter_fpu_stack | 
| JDK-8374521 | Support fine-grained native debug levels | 
| JDK-8374524 | Investigate and cleanup test/jdk/sun/security/provider/SecureRandom/StrongSecureRandom.java and convert to junit | 
| JDK-8374525 | RISC-V: Several masked float16 vector operations are not supported | 
| JDK-8374528 | C2 SuperWord: TestAliasingFuzzer.java strengthen no-multiversioning IR rule | 
| JDK-8374538 | Wrong specification of MethodHandles.constant(…) | 
| JDK-8374540 | Add comment describing implementation choices of Math.fma | 
| JDK-8374544 | Add SleepyCat diagnostics for all platforms | 
| JDK-8374549 | Extend MetaspaceClosure to cover non-MetaspaceObj types | 
| JDK-8374555 | No need for visible input warning in s.s.u.Password when not reading from System.in | 
| JDK-8374557 | Enhance TLS connection handling | 
| JDK-8374570 | Assertion failure in ClearArray.java with -XX:+EnableX86EcoreOpts | 
| JDK-8374575 | [PPC64] Remove support for SuperwordUseVSX on Power 8 | 
| JDK-8374576 | Disable MemoryEaterMT for VirtualThread | 
| JDK-8374577 | Heap dump from core does not contain HPROF_GC_ROOT_JAVA_FRAME | 
| JDK-8374582 | [REDO] Move input validation checks to Java for java.lang.StringCoding intrinsics | 
| JDK-8374622 | StressIncrementalInlining should also randomize the processing order | 
| JDK-8374623 | Move DependentAlwaysFalse variable template to its own file | 
| JDK-8374629 | Type test pattern with array type in switch fails to parse | 
| JDK-8374632 | Broken list layout in the man page of jlink | 
| JDK-8374639 | Static archive with AOTClassLinking breaks dynamic archive | 
| JDK-8374641 | Remove java/nio/channels/AsyncCloseAndInterrupt.java from problem list | 
| JDK-8374642 | EscapeHash macro fails with GNU make 4.3 and 4.4 | 
| JDK-8374643 | Fix reference to implMethodKind in LambdaToMethod debug printf statement | 
| JDK-8374644 | Regression in GZIPInputStream performance after JDK-7036144 | 
| JDK-8374654 | Inconsistent handling of lambda deserialization for Object method references on interfaces | 
| JDK-8374658 | Consider use of "included" rather than "enclosed" in jx.a.p.Processor | 
| JDK-8374662 | Remove unused type check functions from javaClasses.hpp | 
| JDK-8374676 | ZGC: Convert zAbort to use Atomic<T> | 
| JDK-8374677 | ZGC: Convert zArray to use Atomic<T> | 
| JDK-8374678 | ZGC: Convert zForwarding to use Atomic<T> | 
| JDK-8374679 | ZGC: Convert zForwardingAllocator to use Atomic<T> | 
| JDK-8374680 | ZGC: Convert zGeneration to use Atomic<T> | 
| JDK-8374681 | ZGC: Convert zJNICritical to use Atomic<T> | 
| JDK-8374682 | ZGC: Convert zLiveMap to use Atomic<T> | 
| JDK-8374683 | ZGC: Convert zLock to use Atomic<T> | 
| JDK-8374684 | ZGC: Convert zMark to use Atomic<T> | 
| JDK-8374686 | ZGC: Convert zMarkTerminate to use Atomic<T> | 
| JDK-8374687 | ZGC: Convert zNMethodTableIteration to use Atomic<T> | 
| JDK-8374690 | ZGC: Convert zRelocate to use Atomic<T> | 
| JDK-8374695 | ZGC: Convert zTLABUsage to use Atomic<T> | 
| JDK-8374698 | Stub names should look more like identifiers | 
| JDK-8374706 | HttpClient: Ensure that request body publishers support multiple subscriptions | 
| JDK-8374711 | Hotspot runtime/CommandLine/OptionsValidation/TestOptionsWithRanges fails without printing the option name | 
| JDK-8374712 | AOTMappedHeapWriter::relocate_field_in_buffer should use CompressedOops::narrow_oop_cast | 
| JDK-8374717 | Unclear wording in docs for recursion for List, Map and LazyConstant | 
| JDK-8374718 | Generation of CompilerProperties can fail in subtle ways | 
| JDK-8374721 | containers/docker/ShareTmpDir.java timed out after 8362087 | 
| JDK-8374725 | C2: assert(x_ctrl == get_late_ctrl_with_anti_dep(x→as_Load(), early_ctrl, x_ctrl)) failed: anti-dependences were already checked | 
| JDK-8374727 | Audio configuration Platform class - use nio for getting endianness of the underlying platform | 
| JDK-8374743 | G1 starts a concurrent mark when allocating humongous objects during initialization | 
| JDK-8374744 | Enable dumping of APX EGPRs (R16, R31) in JVM fatal error logs | 
| JDK-8374745 | Test vmTestbase/nsk/monitoring/GarbageCollectorMXBean/CollectionCounters/CollectionCounters004/TestDescription.java failed | 
| JDK-8374749 | Clarify AnnotationValue specification | 
| JDK-8374752 | Add more JLS links to javax.lang.model.element.* | 
| JDK-8374754 | jtreg failure handler - replace inline javascript and inline event handlers with same origin javascript files | 
| JDK-8374755 | ML-KEM’s 12-bit decompression can be simplified on aarch64 | 
| JDK-8374767 | Amend JDK-8374521 with new option name | 
| JDK-8374768 | S390X builds are failing after JDK-8372754 | 
| JDK-8374769 | PPC: MASM::pop_cont_fastpath() should reset _cont_fastpath if SP == _cont_fastpath | 
| JDK-8374782 | Parallel: Remove specialized objArray iteration code | 
| JDK-8374785 | Template Library: need to tag Float16.copySign as having non-deterministic result because of multiple NaNs with different sign bits | 
| JDK-8374789 | C2: refactor GraphKit code that create AddP nodes for raw memory to use helper method | 
| JDK-8374796 | CompressedOops versions of runtime/cds/TestDefaultArchiveLoading.java aren’t run | 
| JDK-8374798 | C2: Missing Identity optimization opportunity with RShiftI and LShiftI | 
| JDK-8374802 | java/net/DatagramSocket/SendReceiveMaxSize.java fails on AIX due to small default RCVBUF size | 
| JDK-8374807 | Crash in MethodData::extra_data_lock()+0x0 when running -XX:+TraceDeoptimization -XX:-ProfileTraps -XX:-TieredCompilation -Xcomp -version | 
| JDK-8374808 | Add new methods to KeyStore and KeyStoreSpi that return the creation date as an Instant instead of Date | 
| JDK-8374819 | jpackage and jpackage tests leave some I/O streams unclosed | 
| JDK-8374825 | vmTestbase comment typo: lunch | 
| JDK-8374828 | Save load_barrier_on_oop_field_preloaded in aot CodeCache | 
| JDK-8374830 | Improve Hotspot crash message in CDSTestUtils | 
| JDK-8374839 | Improve jpackage information messages | 
| JDK-8374862 | assert(false) failed: Attempting to acquire lock MDOExtraData_lock/nosafepoint-1 out of order with lock tty_lock/tty — possible deadlock (running with -XX:+Verbose -XX:+WizardMode -XX:+PrintDeoptimizationDetails) | 
| JDK-8374872 | Cleanup outdated SAP AG copyright header info | 
| JDK-8374875 | Improve perfMemory warning about 'Insufficient space for shared memory file' | 
| JDK-8374876 | Epsilon: Convert to use Atomic<T> | 
| JDK-8374878 | Add Atomic<T>::compare_set | 
| JDK-8374879 | NMethodRelocationTest fails with -Xcomp after 8369150 | 
| JDK-8374886 | CAInterop.java#microsoftrsa2017 test fails as EE certificate does not specify OCSP responder | 
| JDK-8374888 | Implement internal test cache to help UserIterCount test performance | 
| JDK-8374889 | C2 VectorAPI: must handle impossible combination of signed cast from float | 
| JDK-8374896 | Min/Max identity optimization opportunities missing for int and long | 
| JDK-8374903 | C2 VectorAPI: assert(vbox→as_Phi()→region() == vect→as_Phi()→region()) failed | 
| JDK-8374905 | Clarify ZonedDateTime#toString() documentation regarding omitted zero seconds | 
| JDK-8374910 | Use of containsTypeEquivalent in array type equality test seems bogus | 
| JDK-8374915 | Move assertion in GenericTaskQueue<>::pop_global() | 
| JDK-8374922 | Build failure after JDK-8372040 | 
| JDK-8374923 | runtime/cds/ServiceLoaderTest.java fails with mismatch between cds and non-cds | 
| JDK-8374926 | EnableX86ECoreOpts was not enabled on some hybrid CPU | 
| JDK-8374943 | TestDirectBufferStatisticsEvent failed with too few statistics events | 
| JDK-8374945 | Avoid fstat in os::open | 
| JDK-8374953 | Add note on about implicit state when comparing TypeMirrors | 
| JDK-8374969 | Incorrect results of LoadStoreNode::adr_type and SCMemProj::adr_type | 
| JDK-8374984 | Convert workerUtils to use Atomic<T> | 
| JDK-8374990 | Check include and jmods folder of JDK image for unwanted files | 
| JDK-8374998 | Failing os::write - remove bad file | 
| JDK-8375006 | [Linux] Remove obsolete O_CLOEXEC check in os::open | 
| JDK-8375010 | C2 VectorAPI: assert(vbox→is_CheckCastPP()) failed: should be expanded | 
| JDK-8375011 | OldJTable.java - NullPointerException when columnData is null | 
| JDK-8375015 | CompletionAPITest::testDocumentation failed - AssertionFailedError: expected: <null> but was: <jshelltest.JShellTest> | 
| JDK-8375038 | C2: Enforce that Ideal() returns the root of the subgraph if any change was made by checking the node hash | 
| JDK-8375040 | Clearer names for non-metadata oop iterators in ObjArrayKlass | 
| JDK-8375045 | Incorrect Interpretation of POSIX TZ Environment Variable on AIX | 
| JDK-8375046 | C2: Incremental inlining step asserts when processing empty late inlines list | 
| JDK-8375050 | Simplify process management in jpackage tests | 
| JDK-8375054 | Removed "signed" property from jpackage app image file | 
| JDK-8375055 | C2: Better dead loop detection printout | 
| JDK-8375057 | Update HarfBuzz to 12.3.2 | 
| JDK-8375061 | Multiple jpackage tool providers may share the same logging config | 
| JDK-8375062 | vmTestbase/nsk/jdb/exclude/exclude001/exclude001.java fails to output debuggee prompt after cont cmd | 
| JDK-8375063 | Update Libpng to 1.6.54 | 
| JDK-8375065 | Update LCMS to 2.18 | 
| JDK-8375066 | Test tools/sincechecker/modules/java.base/JavaBaseCheckSince.java broken by JDK-8369564 | 
| JDK-8375076 | Two JDI scenario tests are failing with out of wait time after completing successfully | 
| JDK-8375093 | Convert GlobalCounter to use Atomic<T> | 
| JDK-8375094 | RISC-V: Fix client builds after JDK-8368732 | 
| JDK-8375119 | SwitchBoostraps.enumSwitch does not throw an NPE when lookup is null in some cases | 
| JDK-8375125 | assert(false) failed: "Attempting to acquire lock NativeHeapTrimmer_lock/nosafepoint out of order with lock ConcurrentHashTableResize_lock/nosafepoint-2 — possible deadlock" when using native heap trimmer | 
| JDK-8375130 | [BACKOUT] Scalability issue when submitting virtual threads with almost empty tasks | 
| JDK-8375177 | Gtest os_linux.decoder_get_source_info_valid_vm fails with -ffunction-sections | 
| JDK-8375183 | Remove unused SSLConfiguration.maximumProtocolVersion variable | 
| JDK-8375221 | Update code to get PrinterResolution from CUPS/IPP print service | 
| JDK-8375231 | Refactor util/ServiceLoader tests to use JUnit | 
| JDK-8375232 | Refactor util/StringJoiner tests to use JUnit | 
| JDK-8375233 | Refactor util/Vector tests to use JUnit | 
| JDK-8375237 | Document existing exceptional behavior of divideUnsigned and remainderUnsigned | 
| JDK-8375240 | Make bundling progress messages issued by jpackage consistent across platforms | 
| JDK-8375241 | Simplify --with-native-debug-symbols-level option implementation | 
| JDK-8375242 | [macos] Improve jpackage signing coverage | 
| JDK-8375271 | [IR Framework] Rename IREncoding to ApplicableIRRules and driver/flag/test VM to Driver/Flag/Test VM | 
| JDK-8375272 | [IR Framework] Miscellaneous clean-ups | 
| JDK-8375275 | Error handling to raise illegal_parameter or internal_error alert in hybrid key exchange | 
| JDK-8375282 | G1: Fix wrong indendation introduced by JDK-8374743 | 
| JDK-8375285 | Port fdlibm asinh to Java | 
| JDK-8375294 | (fs) Files.copy can fail with EOPNOTSUPP when copy_file_range not supported | 
| JDK-8375297 | ZGC: Remove obsolete O_CLOEXEC definition | 
| JDK-8375311 | Some builds are missing debug helpers | 
| JDK-8375314 | Parallel: Crash iterating over unloaded classes for ObjectCountAfterGC event | 
| JDK-8375323 | Improve handling of the "--app-content" and "--input" options in jpackage | 
| JDK-8375325 | add anchors to the options in the security man pages | 
| JDK-8375338 | sun/awt/image/ImageRepresentation/LUTCompareTest.java fails with -Xcheck:jni | 
| JDK-8375342 | jdk/javadoc/doccheck/checks/jdkCheckHtml.java failed with duplicate anchors | 
| JDK-8375350 | Remove usage of AppContext from javax.imageio implementation | 
| JDK-8375351 | Remove usage of AppContext from print implementation | 
| JDK-8375352 | java/net/httpclient/ConnectTimeoutWithProxy*.java tests fail on EC2 | 
| JDK-8375359 | Improve GC serviceability init staging | 
| JDK-8375362 | Deadlock with unmount of suspended virtual thread interrupting another virtual thread | 
| JDK-8375364 | [macos] Some jpackage signing tests fail after JDK-8375240 | 
| JDK-8375367 | vmTestbase tests reported variable uninitialized by clang23 | 
| JDK-8375370 | XRBackendNative.c reported variable uninitialized by clang23 | 
| JDK-8375433 | jar should validate automatic module names | 
| JDK-8375436 | G1: Convert G1CardSet classes to use Atomic<T> | 
| JDK-8375437 | G1: Convert G1EvacFailureRegions to use Atomic<T> | 
| JDK-8375438 | G1: Convert G1HeapRegion related classes to use Atomic<T> | 
| JDK-8375439 | G1: Convert G1MonotonicArena class to use Atomic<T> | 
| JDK-8375442 | C2: Notify nodes that inspect the graph deeply of changes far away during IGVN | 
| JDK-8375443 | AVX-512: Disabling through UseSHA doesn’t affect UseSHA3Intrinsics | 
| JDK-8375455 | G1: Remove unused G1HeapRegionStats::coarsen_stats() | 
| JDK-8375457 | Test serviceability/jvmti/vthread/SelfSuspendDisablerTest/SelfSuspendDisablerTest.java#default timed out | 
| JDK-8375458 | Check legal folder of JDK image for unwanted files | 
| JDK-8375463 | G1: Remove AtomicAccess include from files that do not use it | 
| JDK-8375477 | CoreUtils support for SA tests should attempt to locate and unzip core files when they have been zipped | 
| JDK-8375480 | Remove usage of AppContext from javax/swing/text | 
| JDK-8375483 | Should set flagThisUninit whenever uninitializedThis is on the stack | 
| JDK-8375485 | Tests in vmTestbase/nsk are failing due to missing class unloading after 8373945 | 
| JDK-8375498 | [VectorAPI] Dump primary vector IR details with -XX:+TraceNewVectors | 
| JDK-8375530 | PPC64: incorrect quick verify_method_data_pointer check causes poor performance in debug build | 
| JDK-8375534 | Debug method 'pp' should support compressed oops | 
| JDK-8375535 | G1: Convert CardTableBarrierSet and subclasses to use Atomic<T> | 
| JDK-8375536 | PPC64: Implement special MachNodes for floating point CMove | 
| JDK-8375541 | G1: Race in G1BarrierSet::write_ref_field_post() | 
| JDK-8375544 | JfrSet::clear should not use memset | 
| JDK-8375549 | ConcurrentModificationException if jdk.crypto.disabledAlgorithms has multiple entries with known oid | 
| JDK-8375567 | Remove AppContext usage from Swing Motif L&F classes | 
| JDK-8375568 | Shenandoah: Abbreviate thread names in display when length constraints apply | 
| JDK-8375569 | Store Java mirrors in AOT configuration file | 
| JDK-8375571 | Compiler crash when using record pattern matching with a generic type parameter shadowing a record class | 
| JDK-8375573 | JTable ignores setPreferredWidth during initial layout when AUTO_RESIZE_LAST_COLUMN is enabled | 
| JDK-8375575 | AttachNotSupportedException constructor missing @since 27 | 
| JDK-8375580 | Avoid using ArrayDeque in jdk.internal.loader.URLClassPath | 
| JDK-8375585 | Test java/lang/ProcessBuilder/PipelineLeaksFD.java failed | 
| JDK-8375598 | VM crashes with "assert((labs(val) & 0xFFFFFFFF00000000) == 0 \|\| dest == (address)-1) failed: must be 32bit offset or -1" when using too high value for NonNMethodCodeHeapSize | 
| JDK-8375616 | G1: Convert G1BatchedTask to use Atomic<T> | 
| JDK-8375618 | Incorrect assert in CastLLNode::Ideal | 
| JDK-8375620 | G1: Convert G1CardTableClaimTable to use Atomic<T> | 
| JDK-8375621 | Move RBTree implementation to inline file to minimize included headers | 
| JDK-8375622 | G1: Convert G1CodeRootSet to use Atomic<T> | 
| JDK-8375624 | G1: Convert G1JavaThreadsListClaimer to use Atomic<T> | 
| JDK-8375626 | G1: Convert G1CollectionSetChooser to use Atomic<T> | 
| JDK-8375630 | G1: Convert G1ConcurrentMark to use Atomic<T> | 
| JDK-8375631 | VectorAPI part number: reformulate documentation and improve exception message | 
| JDK-8375633 | C2: Missed Ideal optimization opportunity with ConvD2F/ConvF2HF and -XX:+StressIncrementalInlining | 
| JDK-8375640 | MinMaxIdentity test fails on some machines after 8373134 | 
| JDK-8375643 | G1: Convert G1RegionMarkStatsCache to use Atomic<T> | 
| JDK-8375646 | Some parser flags seem unused | 
| JDK-8375649 | idea.sh script adds source paths in a single, enormous, line to jdk.iml | 
| JDK-8375653 | C2: CmpUNode::sub is not monotonic | 
| JDK-8375654 | Exclude all array classes from dynamic CDS archive | 
| JDK-8375657 | RISC-V: Need to check size in SharedRuntime::is_wide_vector | 
| JDK-8375668 | Compiler warning implicit-const-int-float-conversion by clang23 | 
| JDK-8375683 | Add notes for sctp tests | 
| JDK-8375684 | Avoid leak in KeystoreImpl.m when using CFArrayCreateMutable | 
| JDK-8375688 | C2: Missed Ideal optimization opportunity with VectorMaskToLong and -XX:+StressIncrementalInlining | 
| JDK-8375692 | Hotspot container tests assert with non-ascii vendor name | 
| JDK-8375712 | Convert java/lang/runtime tests to use JUnit | 
| JDK-8375717 | Outdated link in jdk.jfr.internal.JVM javadoc | 
| JDK-8375737 | Fix -Wzero-as-null-pointer-constant warnings in arm32 code | 
| JDK-8375738 | Fix -Wzero-as-null-pointer-constant warnings in MacOSX/bsd code | 
| JDK-8375742 | Test java/lang/invoke/MethodHandleProxies/Driver.java does not run Unnamed.java | 
| JDK-8375747 | ZGC: ZForwardingTest is unable to commit memory on Windows | 
| JDK-8375775 | JDK 26 RDP2 L10n resource files update | 
| JDK-8375787 | compiler/vectorapi/TestCastShapeBadOpc.java fails with release VMs | 
| JDK-8375905 | Add a missing bailout check in PhaseCFG::schedule_late and a test to exercise more bailout code paths | 
| JDK-8375964 | G1: Convert G1BuildCandidateRegionsTask to use Atomic<T> | 
| JDK-8375966 | G1: Convert G1UpdateRegionLivenessAndSelectForRebuildTask to use Atomic<T> | 
| JDK-8375971 | G1: Convert G1EvacStats to use Atomic<T> | 
| JDK-8375974 | G1: Convert G1FullCollector to use Atomic<T> | 
| JDK-8375977 | G1: Convert JVMCICleaningTask to use Atomic<T> | 
| JDK-8375978 | G1: Convert G1Policy to use Atomic<T> | 
| JDK-8375981 | G1: Convert G1RemSet helper classes to use Atomic<T> | 
| JDK-8375982 | G1: Convert G1YoungCollector helper classes to use Atomic<T> | 
| JDK-8375983 | G1: Convert G1ConcurrentRefineStats to use Atomic<T> | 
| JDK-8375999 | com/sun/jndi/ldap/LdapPoolTimeoutTest.java fails sporadically on Windows | 
| JDK-8376031 | HttpsURLConnection.getServerCertificates() throws "java.lang.IllegalStateException: connection not yet open" for the HEAD method | 
| JDK-8376038 | Refactor java/sql tests to use JUnit | 
| JDK-8376050 | awt/event/MouseEvent/AltGraphModifierTest/AltGraphModifierTest.java fails on Ubuntu | 
| JDK-8376051 | gc/stress/TestStressG1Uncommit.java fails assertLessThan: expected that xxx < xxx | 
| JDK-8376052 | Use AttachOperationFailedException rather than AttachNotSupportedException in findTargetProcessTmpDirectory() | 
| JDK-8376089 | Increase QUIC idle timeout in H3FixedThreadPoolTest to collect more diagnostic | 
| JDK-8376104 | C2 crashes in PhiNode::Ideal(PhaseGVN*, bool) accessing NULL pointer | 
| JDK-8376113 | PPC64: Implement special MachNodes for floating point Min / Max | 
| JDK-8376115 | G1: Convert G1CMRootRegions to use Atomic<T> | 
| JDK-8376118 | java/net/httpclient/StreamingBody.java fails intermittently on Windows | 
| JDK-8376119 | G1: Convert volatiles in G1CMMarkStack to Atomic<T> | 
| JDK-8376125 | Out of memory in the CDS archive error with lot of classes | 
| JDK-8376126 | G1: Convert remaining volatiles in G1ConcurrentMark to Atomic<T> | 
| JDK-8376131 | Convert ContiguousSpace to use Atomic<T> | 
| JDK-8376151 | Test javax/swing/JFileChooser/4966171/bug4966171.java is failing with OOME | 
| JDK-8376152 | Test javax/sound/sampled/Clip/bug5070081.java timed out then completed | 
| JDK-8376162 | SwitchBoostraps.typeSwitch does not throw IAE as expected | 
| JDK-8376164 | Optimize AES/ECB implementation using full-message intrinsic stub and parallel RoundKey addition | 
| JDK-8376169 | JPopupMenu.setInvoker(null) causes NPE | 
| JDK-8376174 | [IR Framework] Refactor Test VM socket communication | 
| JDK-8376185 | NoSuchFieldError thrown after a record with type annotation retransformed | 
| JDK-8376186 | [VectorAPI] Nomenclature change for concrete vector classes | 
| JDK-8376187 | [VectorAPI] Define new lane type constants and pass them to intrinsic entries | 
| JDK-8376191 | Remove AtomicAccess include from files that do not use it in gc/shared | 
| JDK-8376199 | Convert CodeCacheUnloadingTask to use Atomic<T> | 
| JDK-8376218 | Improve KeyUtil::getKeySize coverage | 
| JDK-8376220 | C2: Refactor the logic to in MemNode::find_previous_store | 
| JDK-8376226 | CharsetEncoder.canEncode(CharSequence) is much slower than necessary | 
| JDK-8376232 | Remove AppContext from Swing synth related classes | 
| JDK-8376233 | Clean up code in Desktop native peer | 
| JDK-8376234 | Migrate java/lang/constant tests away from TestNG | 
| JDK-8376253 | [macOS] FileSystemView may not report system icons when -Xcheck:jni is enabled | 
| JDK-8376264 | Mixed jstack could not unwind optimized frame | 
| JDK-8376269 | Mixed jstack cannot find function in vDSO | 
| JDK-8376271 | ZipFile comment confusingly refers to "native" ZIP file implementation | 
| JDK-8376274 | JSpec preview support and output enhancement | 
| JDK-8376276 | Add javafx to allowed-list of CheckFiles | 
| JDK-8376277 | Migrate java/lang/reflect tests away from TestNG | 
| JDK-8376281 | Remove USE_XLC_BUILTINS macro usage in AIX code | 
| JDK-8376284 | New test serviceability/sa/TestJhsdbJstackMixedCore.java from JDK-8374482 fails on Linux Alpine | 
| JDK-8376285 | AArch64: Support fusion optimization for SVE destructive instructions | 
| JDK-8376287 | Crashes when using -XX:ObjArrayMarkingStride=0 | 
| JDK-8376290 | SocketChannel.finishConnect() contains confusing "getsockopt" in exception message for a failed connect() on Windows | 
| JDK-8376291 | [IR Framework] Create classes for separate Test VM messages | 
| JDK-8376293 | Bad copyright header in g1ConcurrentRefineStats.inline.hpp breaks the build | 
| JDK-8376294 | ZipFile.Source.Key should not hold on to its BasicFileAttributes instance | 
| JDK-8376295 | "assert(BytecodeVerificationRemote) failed: Should not be here" when running class redefinition test with -XX:-BytecodeVerificationRemote | 
| JDK-8376297 | ArrayIndexOutOfBoundsException Not Documented for SinglePixelPackedSampleModel.getSampleSize(int) | 
| JDK-8376300 | Avoid more leaks in KeystoreImpl.m because of missing CFRelease calls | 
| JDK-8376302 | os::Machine::used_memory reports container used memory when running containerized | 
| JDK-8376308 | java/net/httpclient/CancelRequestTest.java fails intermittently with "Expected CancellationException not received" | 
| JDK-8376324 | [IR Framework] Name methods in a CompileCommand-friendly way | 
| JDK-8376325 | [IR Framework] Detect and report overloads | 
| JDK-8376328 | Convert PLABStats to use Atomic<T> | 
| JDK-8376335 | Convert PreservedMarks classes to use Atomic<T> | 
| JDK-8376350 | Convert ReferenceProcessorPhaseTimes to use Atomic<T> | 
| JDK-8376351 | Parallel: Convert ParallelScavengeHeap to use Atomic<T> | 
| JDK-8376353 | Parallel: Convert PSParallelCompact classes to use Atomic<T> | 
| JDK-8376355 | Update to use jtreg 8.2.1 | 
| JDK-8376356 | Parallel: Convert PSCardTable to use Atomic<T> | 
| JDK-8376357 | Parallel: Convert MutableSpace classes to use Atomic<T> | 
| JDK-8376398 | [TESTBUG] Testing of Unsafe native (re)allocation is sensitive to current JDK native memory use | 
| JDK-8376400 | C2: folding ifs may cause incorrect execution when trap is taken | 
| JDK-8376402 | Dependencies::print_statistics() and AbstractClassHierarchyWalker::print_statistics() are not called from PRODUCT code | 
| JDK-8376403 | Avoid loading ArrayDeque in java.util.zip.ZipFile | 
| JDK-8376405 | Virtual thread crash: assert(!_current→is_suspended()) failed: must be | 
| JDK-8376410 | G1: Task queue statistics not reset properly on mark abort | 
| JDK-8376419 | (fs) Minor improvement of java/nio/file/attribute/UserDefinedFileAttributeView/Basic.java | 
| JDK-8376420 | Remove AppContext from javax/swing/ImageIcon.java | 
| JDK-8376421 | C2: Missing branch on SubTypeCheck node | 
| JDK-8376422 | Run compiler/corelibs/OptionalFold.java with tiered compilation | 
| JDK-8376423 | Test javax/swing/plaf/metal/MetalUtils/bug6190373.java failed: ClassCastException: class java.lang.Character cannot be cast to class javax.swing.Painter | 
| JDK-8376432 | Remove AppContext from sun/swing/DefaultLookup.java | 
| JDK-8376433 | Remove AppContext from Swing Windows L&F implementation | 
| JDK-8376434 | Remove AppContext from awt ImageFetcher implementation | 
| JDK-8376472 | Shenandoah: Assembler store barriers read destination memory despite the decorators | 
| JDK-8376477 | Avoid loading empty Lock classes in Shutdown and ReferenceQueue | 
| JDK-8376479 | Http3 test server thread deadlock in ThrowingPublishersInRequest | 
| JDK-8376491 | ZGC: crash in __memset_evex_unaligned_erms when initializing heap using high values for -XX:ConcGCThreads | 
| JDK-8376509 | [process] Problemlist Test java/lang/ProcessBuilder/PipelineLeaksFD.java | 
| JDK-8376510 | Raster.createBandedRaster(int, int, int, int, int[], int[], Point) does not check for negative scanlineStride | 
| JDK-8376521 | Verifier: disallow acmp & ifnull on 'uninitialized' types | 
| JDK-8376523 | Move interned strings into AOT heap roots array | 
| JDK-8376531 | Genshen: Convert ShenandoahOldGeneration to use Atomic<T> | 
| JDK-8376533 | Remove test dependencies on ReferenceQueue$Lock in preparation for JDK-8376477 | 
| JDK-8376534 | Source launcher instantiates wrong class on inherited instance main | 
| JDK-8376568 | Change Thread::getStackTrace to use handshake op for all cases | 
| JDK-8376570 | GrowableArray::remove_{till,range} should work on empty list | 
| JDK-8376572 | RISC-V: Interpreter: Load array index as signed int | 
| JDK-8376585 | bin/update_copyright_year.sh could allow updating a specified list of files | 
| JDK-8376587 | Fatal "dead path discovered by TypeNode during igvn" after C2 compilation | 
| JDK-8376591 | TestLoopNestTooManyTraps fails with various stress flags | 
| JDK-8376602 | [Vector API] Upgrade SLEEF from 3.6.1 to 3.9.0 | 
| JDK-8376604 | C2: EA should assert is_oop_field for AddP with oop outs | 
| JDK-8376610 | Test javax/net/ssl/TLS/TestJSSEClientDefaultProtocol.java times out then completed | 
| JDK-8376621 | Should not suspend thread in start_transition if _is_disable_suspend set | 
| JDK-8376626 | Shenandoah: Remove command line options for setting min and max region sizes | 
| JDK-8376627 | Remove AppContext from javax/swing/plaf/metal classes | 
| JDK-8376629 | Refactor javax/sql tests to use JUnit | 
| JDK-8376630 | java/lang/ProcessBuilder/PipelineLeaksFD.java intermittent timed out | 
| JDK-8376645 | Test java/net/httpclient/http3/H3IdleExceedsQuicIdleTimeout.java failed: no response from peer | 
| JDK-8376650 | os::release_memory_special may not be needed anymore | 
| JDK-8376664 | Find a better place for the Atomic<HeapWord*> vmstructs toplevel declaration | 
| JDK-8376665 | Port fdlibm acosh to Java | 
| JDK-8376666 | Convert G1BlockOffsetTable to use Atomic<T> | 
| JDK-8376684 | Compile OpenJDK in headless mode without required X11 libraries | 
| JDK-8376688 | Gtest os.attempt_reserve_memory_between_small_range_fill_hole_vm fails on AIX 7.3 | 
| JDK-8376698 | Add Spliterator tests for TreeMap sub-maps | 
| JDK-8376700 | java/nio/file/DirectoryStream/SecureDS.java fails AtomicMoveNotSupportedException | 
| JDK-8376703 | Some coding in libjimage seems to be not called at all or not called from PRODUCT code | 
| JDK-8376707 | Template-Framework Library: Primitive Types Abbreviation Methods | 
| JDK-8376747 | Remove AppContext from Swing LayoutStyle | 
| JDK-8376751 | add preview project anchors to main-line ProblemList files | 
| JDK-8376755 | Remove AppContext from Swing javax/swing/plaf/basic classes | 
| JDK-8376756 | GenShen: Improve encapsulation of generational collection set choosing | 
| JDK-8376758 | Fix -Wzero-as-null-pointer-constant warnings in AIX code | 
| JDK-8376760 | VerifyJimage.java#compare intermittent failed with fastdebug | 
| JDK-8376761 | ARM32: Constant base assert after JDK-8373266 | 
| JDK-8376777 | Consistent use of nonstatic instead of non_static in ci files | 
| JDK-8376781 | Problemlist compiler/longcountedloops/TestLoopNestTooManyTraps.java | 
| JDK-8376794 | Enable copy and mismatch Partial Inlining for AMD AVX512 targets | 
| JDK-8376810 | Make Atomic<T> default constructor non-explicit | 
| JDK-8376811 | Implement JEP 531: Lazy Constants (Third Preview) | 
| JDK-8376822 | UseCompactObjectHeaders: fill Klass alignment gaps in AOT cache | 
| JDK-8376839 | GenShen: Improve performance of evacuations into the old generation | 
| JDK-8376855 | ASAN reports out-of-range read in strncmp in MethodHandles::is_basic_type_signature | 
| JDK-8376885 | StressGetTotalGcCpuTimeDuringShutdown.java intermittently timed out | 
| JDK-8376889 | Enhance JfrRecorder::on_create_vm_3() assert output | 
| JDK-8376891 | [VectorAlgorithms] add more if-conversion benchmarks and tests | 
| JDK-8376892 | Allow conversion warnings in subsets of the code base | 
| JDK-8376954 | Typos in CharacterRangeInfo and AsynchronousServerSocketChannel | 
| JDK-8376956 | Add JVMTI phase entering/setting to hserr event log | 
| JDK-8376969 | Shenandoah: GC state getters should be inlineable | 
| JDK-8376970 | Shenandoah: Verifier should do basic verification before touching oops | 
| JDK-8376992 | Remove AppContext from SystemTray implementation | 
| JDK-8376994 | Remove AppContext from sun/awt/datatransfer/DataTransferer.java | 
| JDK-8376996 | Remove AppContext usage from SunClipboard.java | 
| JDK-8376998 | [macOS] Remove AppContext from AppEventHandler | 
| JDK-8377000 | [BACKOUT] JDK-8376126 G1: Convert remaining volatiles in G1ConcurrentMark to Atomic<T> | 
| JDK-8377004 | Java Launcher incorrectly allows inheriting a package-private main from another package | 
| JDK-8377008 | [REDO] G1: Convert remaining volatiles in G1ConcurrentMark to Atomic<T> | 
| JDK-8377010 | Source code launcher cannot call inherited main from another package/module | 
| JDK-8377011 | Shenandoah: assert_bounds should be only called when boundaries have changed | 
| JDK-8377013 | TimeZone.getDefault() returns obsolete id on Windows (Asia/Calcutta) | 
| JDK-8377015 | ConnectionRefusedMessage::testFinishConnect test fails on AIX with java.net.ConnectException: Connection refused | 
| JDK-8377018 | Convert java/nio/file/DirectoryStream/SecureDS.java to junit | 
| JDK-8377035 | [s390x] Disable JSR166 test cases which uses virtual threads | 
| JDK-8377043 | Shenandoah: Convert ShenandoahHeapRegion related code to use Atomic<T> | 
| JDK-8377044 | Shenandoah: Convert ShenandoahHeap related code to use Atomic<T> | 
| JDK-8377045 | Shenandoah: Convert ShenandoahLock related code to use Atomic<T> | 
| JDK-8377048 | Shenandoah: shenandoahLock related improvments | 
| JDK-8377063 | Add EchoPassword.java to manual group | 
| JDK-8377070 | Update jimage format to support classes compiled with preview feature enabled | 
| JDK-8377096 | Refactor AOTMapLogger::OopDataIterator implementations | 
| JDK-8377116 | Refactor java/util/zip TestNG tests to JUnit | 
| JDK-8377126 | Shenandoah: Convert ShenandoahVerifier related code to use Atomic<T> | 
| JDK-8377128 | Add missing @Override annotations in "java.beans.*" packages | 
| JDK-8377138 | VMError::report should handle out-of-stackbounds errors gracefully | 
| JDK-8377141 | G1: Remove unused local declaration in G1BarrierSetC2 | 
| JDK-8377142 | Jtreg test gc/shenandoah/oom/TestThreadFailure.java triggers assert(young_reserve + reserve_for_mixed + reserve_for_promo ⇐ old_available + young_available) failed | 
| JDK-8377143 | Parallel: Remove special treatment in JstatGcCapacityResults.java | 
| JDK-8377148 | Remove obsolete functions Matcher::is_spillable_arg() and Matcher::interpreter_frame_pointer_reg() | 
| JDK-8377150 | G1: Full GC should not execute barrier code during reference processing | 
| JDK-8377158 | Enhance XBM image support | 
| JDK-8377161 | G1: Remove unnecessary member G1FullGCScope::_g1h | 
| JDK-8377163 | C2: Iteration split must take into consideration sunk stores | 
| JDK-8377164 | G1: Clean up g1BlockOffsetTable.hpp | 
| JDK-8377165 | G1: Introduce common G1 GC Mark to collect scoped objects | 
| JDK-8377167 | javax/imageio/ReadAbortTest.java throw NPE when x11 unavailable | 
| JDK-8377172 | Change datatype of CodeEntryAlignment to uint | 
| JDK-8377179 | Improve and document racy use of start/end in ThreadLocalAllocBuffer | 
| JDK-8377180 | Shenandoah: make escalation from degen to full more conservative | 
| JDK-8377181 | HttpClient may leak closed QUIC connection objects | 
| JDK-8377183 | Impossible or redundant condition in AwtFrame::_NotifyModalBlocked of awt_Frame.cpp:1635 | 
| JDK-8377191 | Remove AppContext from KeyboardFocusManager | 
| JDK-8377192 | Remove AppContext from MenuSelectionManager | 
| JDK-8377193 | Remove AppContext from SwingUtilties3 | 
| JDK-8377199 | Remove AppContext from AWTKeyStroke | 
| JDK-8377200 | Shenandoah: Convert shenandoahSharedVariables and related code to use Atomic<T> | 
| JDK-8377223 | Port fdlibm atanh to Java | 
| JDK-8377224 | Initialization cleanups after Atomic<T> conversion | 
| JDK-8377225 | RISC-V: Improve receiver type profiling reliability | 
| JDK-8377226 | Inline Thread::set_allocated_bytes | 
| JDK-8377231 | Build jpackage with SIZE optimization | 
| JDK-8377244 | Update ModuleReaderTest to JUnit prior to making changes | 
| JDK-8377245 | AbstractMemorySegmentImpl#getString with length should be @ForceInline | 
| JDK-8377300 | Use64KPages flag cleanup on AIX | 
| JDK-8377302 | HttpServer::stop uses full timeout duration if handler throws | 
| JDK-8377307 | Refactor code for AOT cache pointer compression | 
| JDK-8377309 | Remove PhaseIterGVN::verify_Identity_for exclusion for Min/Max find identity op | 
| JDK-8377313 | java/lang/Thread/virtual/stress/GetStackTraceALotWhenPinned.java#id0 timeout | 
| JDK-8377315 | test/jdk/sun/security/pkcs11/Cipher/TestPKCS5PaddingError.java is failing with "Expected BPE NOT thrown" | 
| JDK-8377318 | Force bad padding exception in TestPKCS5PaddingError.java at all times | 
| JDK-8377326 | [PPC64] build without C1 and C2 broken | 
| JDK-8377331 | jpackage: improve sign errors reporting | 
| JDK-8377334 | Test framework used by langtools regression tests can produce false positives | 
| JDK-8377338 | URLJarFile$URLJarFileEntry need not clone the arrays returned by getCertificates() and getCodeSigners() | 
| JDK-8377347 | jdk/jfr/event/gc/detailed/TestZAllocationStallEvent.java intermittent OOME | 
| JDK-8377352 | Parallel: Incorrect capacity in GC overhead log | 
| JDK-8377355 | VectorAPI source generation broken after JDK-8371187 | 
| JDK-8377359 | TestOpaqueConstantBoolNodes fails on PPC64 | 
| JDK-8377365 | [BACKOUT] Mixed jstack cannot find function in vDSO | 
| JDK-8377368 | [REDO] Mixed jstack cannot find function in vDSO | 
| JDK-8377388 | Replacing "new StringReader(String)" by "Reader.of(CharSequence);" in Scanner | 
| JDK-8377389 | C2: Missed Ideal optimization opportunity in PhaseIterGVN for URShiftI and LShiftI | 
| JDK-8377392 | jpackage: Fix member function called from the CfgFile.Referencies compact canonical constructor | 
| JDK-8377395 | serviceability/sa/TestJhsdbJstackMixedCore.java fails due to NPE because "because "this.cfa" is null" | 
| JDK-8377396 | GenShen: Consolidate and simplify in place region promotions | 
| JDK-8377411 | java/lang/Thread/virtual/stress/ParkAfterTimedPark.java only testing pinned case | 
| JDK-8377413 | [MacOS aarch64] guarantee(StressWXHealing) failed: We should not reach here unless StressWXHealing | 
| JDK-8377425 | Test runtime/os/TestWXHealing.java fails on macosx-aarch64 product build | 
| JDK-8377428 | VoiceOver Cursor Navigates Invisible Components | 
| JDK-8377429 | Warning as error in asan build in test_nmt_cornercases.cpp on Linux | 
| JDK-8377433 | Test com/sun/jmx/remote/NotificationMarshalVersions/TestSerializationMismatch.java failed | 
| JDK-8377435 | Problem list serviceability/sa/TestJhsdbJstackMixedCore.java | 
| JDK-8377438 | [VectorAPI] Add new carrier type to LaneType class | 
| JDK-8377442 | More fixes to ThreadLocalAllocBuffer after JDK-8377179 | 
| JDK-8377443 | G1: Remove unnecessary cast in ResizeTLABAndSwapCardTableTask | 
| JDK-8377446 | Improve parameter naming in pointer_delta() | 
| JDK-8377447 | [VectorAPI] Assert wrappers to convert float16 (short) value to float before invoking testng Asserts | 
| JDK-8377449 | Strengthen vector IR validation in TestVectorAlgorithms.java for AArch64 | 
| JDK-8377454 | TestZGCWithCDS.java should also test different combos of UseCompactObjectHeaders | 
| JDK-8377455 | Replace LinkedList with GrowableArray in VM.ThreadDump’s OMTable | 
| JDK-8377456 | GetObjectSizeIntrinsicsTest.java, ARRAY_HEADER_SIZE wrong for 32-bit | 
| JDK-8377457 | java/util/logging/ParentLoggersTest.java failed intermittently | 
| JDK-8377461 | Add ZipFile test for incorrect number of entries reported in ENDTOT | 
| JDK-8377486 | com.sun.jndi.ldap.sasl.SaslOutputStream.write() throws NullPointerException if it is already closed | 
| JDK-8377498 | Improve HttpServer handling | 
| JDK-8377502 | Add anchors to the options in the Serviceability tool man pages | 
| JDK-8377506 | Implement JEP 538: PEM Encodings of Cryptographic Objects (Third Preview) | 
| JDK-8377507 | Store cpu features in AOTCodeCache | 
| JDK-8377509 | Add licenses for gcc 14.2.0 | 
| JDK-8377512 | AOT cache creation fails with invalid native pointer | 
| JDK-8377513 | jpackage: fix Win8365790Test test | 
| JDK-8377514 | jpackage: support passing multiple exceptions to the top-level error handler | 
| JDK-8377517 | AArch64: TestUseSHA3IntrinsicsWithUseSHADisabledOnUnsupportedCPU.java fails after JDK-8375443 | 
| JDK-8377526 | Update Libpng to 1.6.55 | 
| JDK-8377534 | Test java/awt/print/PrinterJob/PrintNullString.java fails with FAILURE: No IAE for empty iterator, int | 
| JDK-8377541 | C2: Memory Barrier IR nodes not eliminated for stable array element access | 
| JDK-8377549 | [BACKOUT] Need to keep leading zeros in TlsPremasterSecret of TLS1.3 DHKeyAgreement | 
| JDK-8377550 | [REDO] Need to keep leading zeros in TlsPremasterSecret of TLS1.3 DHKeyAgreement | 
| JDK-8377554 | Load card table base and other values via AOTRuntimeConstants in AOT code | 
| JDK-8377561 | Parallel: Large allocations cause Full GC storm without heap expansion | 
| JDK-8377568 | DataBuffer constructors and methods do not specify required exceptions | 
| JDK-8377601 | JavacTemplateTestBase not reporting javac crashes | 
| JDK-8377602 | Create automated test for PageRange | 
| JDK-8377603 | Review worst-case testing of inverse hyperbolic methods | 
| JDK-8377626 | The macOS build is broken after JDK-8332189 | 
| JDK-8377629 | jpackage: Tighten up output validation in tests | 
| JDK-8377651 | [s390x] build failure without c1 & c2 compiler | 
| JDK-8377652 | C2: Simplify Compile::flatten_alias_type for array | 
| JDK-8377655 | C1: in x86, avoid 0x00s leftover after PatchingStub | 
| JDK-8377656 | JUnit test java/util/zip/ZipFile/ZipFileInputStreamSkipTest.java uses private methods for BeforeAll/AfterAll | 
| JDK-8377658 | Cleanup files for enabling conversion warnings for g1ConcurrentMark.cpp | 
| JDK-8377665 | JFR: Symbol table not setup for early class unloading | 
| JDK-8377666 | Fedora 41 based devkit build should load more packages from archive location | 
| JDK-8377671 | Step Over doesn’t stop after receiving MethodExitEvent | 
| JDK-8377675 | java.net.http tests should not depend on ../../../com/sun/net/httpserver test classes | 
| JDK-8377690 | serviceability/sa/TestJhsdbJstackMixedWithXComp.java tests failing | 
| JDK-8377701 | Shenandoah: Convert ShenandoahEvacOOMHandler to use Atomic<T> | 
| JDK-8377702 | Disable AArch64 SpinPause tests on Windows | 
| JDK-8377703 | Assert that all AOT heap objects have valid classes | 
| JDK-8377704 | Shenandoah: Convert ShenandoahNMethod to use Atomic<T> | 
| JDK-8377707 | [Linux Alpine] Build failure after JDK-8377368 | 
| JDK-8377710 | Test serviceability/sa/TestJhsdbJstackMixed.java encountered Driver timeout | 
| JDK-8377712 | ConstantPool of WeakReferenceKey is not deterministic in CDS archive | 
| JDK-8377713 | Shenandoah: Convert ShenandoahReferenceProcessor to use Atomic<T> | 
| JDK-8377715 | Thawing frame can undo deoptimization | 
| JDK-8377726 | Remove unused macros in register.hpp | 
| JDK-8377727 | Ghost caret and focus appear in nonâ€‘editable text fields | 
| JDK-8377729 | Running jtreg tests with -agent… option causes some tests to fail due to duplicate -agent options provided to a subprocess | 
| JDK-8377745 | VoiceOver Identifies Hyperlink as Text | 
| JDK-8377769 | Only use large pages sizes that have any pages configured | 
| JDK-8377777 | Improve logging when rejecting assets from the AOT archive | 
| JDK-8377796 | java.net.http.HttpClient.send() and sendAsync() never complete when BodyHandler.apply() returns null | 
| JDK-8377797 | Remove SA support for MethodData and the printmdo command | 
| JDK-8377798 | Hotspot build on macOS aarch64 with unused-functions warning reports some unused functions | 
| JDK-8377808 | compiler/c2/irTests/TestFloat16ScalarOperations.java is fragile in the presence of special constants | 
| JDK-8377826 | Eliminate code duplication in XbmImageDecoder | 
| JDK-8377833 | Enhance Jar file processing | 
| JDK-8377845 | Restore regtest for JDK-8324881 with DiagnoseSyncOnValueBasedClasses=2 | 
| JDK-8377857 | Add since checker test for java.naming module | 
| JDK-8377862 | Jtreg is unable to detect SkippedException because it is wrapped by CompileFramework | 
| JDK-8377863 | [s390x] Increase inlining thresholds to the same as other platforms | 
| JDK-8377878 | Problem list compiler/vectorization/TestVectorAlgorithms.java on AIX and Linux s390x | 
| JDK-8377880 | Enable unused function warnings in hotspot with clang too | 
| JDK-8377895 | Create sizeof_auto to reduce narrowing conversions | 
| JDK-8377897 | jpackage: make jdk.jpackage.internal.MockUtils available from other packages | 
| JDK-8377898 | Hotspot build on AIX with unused-functions warning reports some unused functions | 
| JDK-8377903 | ArraysSupport::mismatch should document that they return the smallest index | 
| JDK-8377905 | gcc.md included with every build | 
| JDK-8377907 | (process) Race in ProcessBuilder can cause JVM hangs | 
| JDK-8377909 | Replace SummaryDiff’s array implementation with a hashtable | 
| JDK-8377910 | Minor cleanup of java/io/FileDescriptor/Sharing.java | 
| JDK-8377914 | Typos in HKDFParameterSpec.Builder::thenExpand | 
| JDK-8377921 | Add anchors to the options in the Core Libs tool man pages | 
| JDK-8377924 | Inconsistent parsing of XBM files after JDK-8361748 | 
| JDK-8377932 | AOT cache is not rejected when JAR file has changed | 
| JDK-8377936 | VoiceOver Reads Button Incorrectly | 
| JDK-8377937 | [macos] GlyphMetrics advance does not consider font rotation | 
| JDK-8377938 | VoiceOver Uses Mouse to Activate JButtons in German | 
| JDK-8377944 | LowMemoryTest2.java#id1 intermittent fails OOME: Metaspace | 
| JDK-8377946 | Use DWARF in mixed jstack on Linux AArch64 | 
| JDK-8377947 | Test serviceability/sa/TestJhsdbJstackMixedCore.java failed on linux-x64 | 
| JDK-8377948 | The ThreadWXEnable use of PerfTraceTime is not safe during VM shutdown | 
| JDK-8377949 | TestZRelocationSetEvent.java intermittent fails OOME | 
| JDK-8377950 | Refactor ClassUnloader to provide explicit waiting API | 
| JDK-8377975 | Fails to parse PEM text with trailing whitespace on the first line | 
| JDK-8377976 | GenShen: Explicit GC requests must clear concurrent gc request cancellation | 
| JDK-8377983 | (zipfs) ZipFileSystem.initCEN needlessly reads END header | 
| JDK-8377986 | C2: New method to add specific users to the worklist | 
| JDK-8377990 | Zero: Replace Java math ops with UB-safe implementations | 
| JDK-8377991 | TestLimitsUpdating.java fails with runtime exception even after JDK-8370492 is fixed | 
| JDK-8377992 | (zipfs) Align ZipFileSystem END header validation with the ZipFile implementation | 
| JDK-8377995 | ZGC: Fix implicit narrowing conversion in zBitField | 
| JDK-8377996 | [REDO] NMT: Consolidate [Virtual/Committed/Reserved]Regions into one structure | 
| JDK-8377997 | [BACKOUT] 8366241: NMT: Consolidate [Virtual/Committed/Reserved]Regions into one structure | 
| JDK-8378003 | JarURLConnection.getCertificates() and getCodeSigners() incorrectly return null for signed JAR files after JDK-8377338 | 
| JDK-8378005 | Exclude boxing calls from guaranteed safepoint check in loop opts | 
| JDK-8378050 | Add missing @Override annotations in "java.awt.color" package | 
| JDK-8378057 | CAccessibility roleKey and AWTAccessor.AccessibleBundleAccessor are Redundant | 
| JDK-8378076 | Improve inlining around oop_oop_iterate | 
| JDK-8378080 | Zero: JNIEnv argument is corrupted in native calls | 
| JDK-8378083 | Mark shenandoah/generational/TestOldGrowthTriggers.java as flagless | 
| JDK-8378098 | Problem list compiler/vectorization/TestVectorAlgorithms.java with#<subtest> on AIX and Linux s390x | 
| JDK-8378100 | Unused code in rewriter.hpp | 
| JDK-8378102 | Validly imported symbols of unresolved types do not have their FQN reflected in the Element/TypeMirror instances passed to annotation processors | 
| JDK-8378107 | Data cache zeroing is used even when it is prohibited | 
| JDK-8378110 | Add -XX: prefix to settings-file flags in RuntimeMXBean.getInputArguments() | 
| JDK-8378111 | Migrate java/util/jar tests to JUnit | 
| JDK-8378112 | Test runtime/ErrorHandling/MachCodeFramesInErrorFile.java fails with RuntimeException: 1 < 2 | 
| JDK-8378113 | Add sun/java2d/OpenGL/ScaleParamsOOB.java to the ProblemList.txt file | 
| JDK-8378115 | (fs) sun.nio.fs.NativeBuffer no longer needs a cleaner | 
| JDK-8378118 | Test tools/jimage/JImageBadFileTest.java failed on Windows | 
| JDK-8378119 | GenShen: Restore log message about characteristics of promotion efforts | 
| JDK-8378128 | Make PLABStats data members private | 
| JDK-8378138 | G1: Assertion failure from G1CollectedHeap::block_start processing during error reporting | 
| JDK-8378149 | [s390] Non-C2 / minimal JVM crashes in the build | 
| JDK-8378152 | Upstream AOT heap object improvements from Leyden repo | 
| JDK-8378153 | Robot.getPixelColor() may return stale pixels due to missing Toolkit.sync() | 
| JDK-8378157 | Section hyperlink in doc/testing.md refers to building.html instead of building.md | 
| JDK-8378163 | test/jdk/java/net/httpclient/*.java: convert tests that use ITestContext to JUnit | 
| JDK-8378164 | test/jdk/java/net/httpclient/http3/*.java: convert tests that use ITestContext to JUnit | 
| JDK-8378166 | C2 VectorAPI: NBody / particle life demo | 
| JDK-8378171 | JFR: Copy of a closed recording should not be available | 
| JDK-8378176 | Concurrent GC worker threads may suffer from priority inversion | 
| JDK-8378177 | AllowExtshm flag cleanup on AIX | 
| JDK-8378178 | Change Thread::_allocated_bytes from jlong to uint64_t | 
| JDK-8378180 | Compiling OpenJDK with C23 C-Compiler gives warning: initialization discards "const" qualifier from pointer target type | 
| JDK-8378191 | [BACKOUT] JDK-8378076 Improve inlining around oop_oop_iterate | 
| JDK-8378192 | Remove AppContext from SwingUtilities2 | 
| JDK-8378193 | Remove AppContext from JinternalFrame | 
| JDK-8378194 | Protect process_pending_interp_only() work with JvmtiThreadState_lock | 
| JDK-8378195 | guarantee(static_cast<int>(_oops_size) == align_up(code_buffer→total_oop_size(), oopSize)) failed: failed: 12272 != 77808 | 
| JDK-8378197 | Remove AppContext from sun/swing/plaf/DesktopProperty.java | 
| JDK-8378201 | [OGL] glXMakeContextCurrent() drops the buffers of the unbound drawable | 
| JDK-8378202 | Remove AppContext from XAWT classes | 
| JDK-8378203 | Remove AppContext from jdk.unsupported.desktop | 
| JDK-8378204 | Remove AppContext from two Swing UI classes | 
| JDK-8378205 | Remove AppContext from Swing MenuComponent | 
| JDK-8378211 | Test ChangedJarFile.java failed: missing "timestamp has changed" | 
| JDK-8378218 | MSYS2 reports cygwin triplet causing bash configure failure | 
| JDK-8378228 | Replace jQuery UI autocomplete component in JavaDoc search | 
| JDK-8378233 | depends_only_on_test_impl() assertion hit after JDK-8347365 | 
| JDK-8378239 | C2: Incorrect check in StoreNode::Identity | 
| JDK-8378240 | C2: MemNode::can_see_stored_value assumes this can never be a StoreVector | 
| JDK-8378243 | [IR Framework] Create separate VMInfo and ApplicableIRRules JavaMessage classes | 
| JDK-8378245 | Obsolete ParallelRefProcBalancingEnabled | 
| JDK-8378250 | C2 VectorAPI : wrong result with MUL reduction at various AVX levels | 
| JDK-8378251 | Extend word delimiters used in jshell | 
| JDK-8378257 | [IR Framework] Add identity for socket connection and prepare for parsing C2 dumps | 
| JDK-8378266 | Update atomicAccess include after Atomic<T> changes | 
| JDK-8378267 | Test sun/security/util/HexDumpEncoderTests.java fails when unexpected warning appears | 
| JDK-8378268 | Thread.join can wait on Thread, allows joinNanos to be removed | 
| JDK-8378276 | Refactor test/jdk/java/net/httpclient/quic/ TestNG tests to JUnit | 
| JDK-8378291 | Test vector in test/jdk/java/util/jar/JarEntry/GetMethodsReturnClones.java is effectively unsigned | 
| JDK-8378293 | GenShen: Simplify selection of aged regions | 
| JDK-8378295 | Update scalar AVX10 floating point min/max definitions | 
| JDK-8378296 | Remove AppContext from java.awt.event.FocusEvent | 
| JDK-8378297 | Remove AppContext from several Swing component and related classes | 
| JDK-8378298 | Remove obsolete CDS string tests | 
| JDK-8378312 | [VectorAPI] libraryUnaryOp/libraryBinaryOp failed to intrinsify | 
| JDK-8378319 | Obsolete the MaxRAM flag | 
| JDK-8378330 | Do not malloc the GrowableArrays in async_get_stack_trace | 
| JDK-8378331 | G1: WeakProcessor IsAlive and KeepAlive closures not handling humongous candidates correctly | 
| JDK-8378336 | G1: j.l.ref.Reference processor always keeps alive all humongous object referents | 
| JDK-8378338 | Shenandoah: Heap-used generic verification error after update-refs | 
| JDK-8378344 | Refactor test/jdk/java/net/httpclient/*.java TestNG tests to JUnit | 
| JDK-8378347 | AIX version checks for 7.1 and 5.X are obsolete | 
| JDK-8378348 | Obsolete ParallelRefProcEnabled | 
| JDK-8378353 | [PPC64] StringCoding.countPositives causes errors when the length is not a proper 32 bit int | 
| JDK-8378354 | Faulty assertion in checkInvariants method of ConcurrentHashMap | 
| JDK-8378369 | TestJhsdbJstackMixedWithVDSOCallCore.java fails if missing vdso64.so | 
| JDK-8378371 | Default classlist not deterministic due to JIT activity | 
| JDK-8378376 | DaCapo-h2-large regression after JDK-8238686 | 
| JDK-8378377 | Remove use of AppContext from JEditorPane | 
| JDK-8378378 | Remove references to obsolete jdk.net.usePlainDatagramSocketImpl property from tests | 
| JDK-8378379 | Remove reference to obsolete jdk.net.usePlainSocketImpl property from SSLSocketReset test | 
| JDK-8378385 | Remove AppContext from AWT Windows implementation classes | 
| JDK-8378386 | Remove AppContext from AWT ModalEventFilter.java | 
| JDK-8378387 | Remove AppContext from several macOS AWT classes | 
