---
id: collect-261121-veille-ia/securite/java4shell-flaw-lets-attackers-take-over-java-services-without-login
title: "Java4Shell flaw lets attackers take over Java services without login"
domain: securite
role: reference
task: article
actors: []
dates: []
keywords: ["java", "services", "update", "flaw", "systems", "code", "login", "affected"]
source: https://cyberinsider.com/java4shell-java-flaw-remote-code-execution-without-login/
source_anchor: ""
source_lines: [1, 25]
section: "Securite"
---
Oracle released a Java security update on November 17 after researchers demonstrated that an attacker could take control of Jira, Jenkins, and several other services without signing in.

The flaw, dubbed Java4Shell and tracked as CVE-2026-114938, affects the Java runtime on certain Intel and AMD systems. Applications that ship their own copy of Java may need a separate update.

Quang Luong of security firm Calif found the bug while auditing HotSpot, the virtual machine that runs programs built for OpenJDK. Luong asked OpenAI’s Codex to examine several areas where Java hands work to optimized machine code. After discarding early findings that an attacker could not realistically reach, he investigated one in Java’s Base64 decoder.

Base64 lets applications carry binary data as text, including in login requests and file uploads. To speed up decoding, HotSpot can process many bytes at once using special processor instructions. Luong found that malformed input could make the decoder write beyond the memory set aside for its output. Depending on how an application uses the decoder, an attacker could read sensitive data from memory or run code with the application’s privileges.

### Which systems are exposed?

Calif traced the affected code back to OpenJDK 17.0.6. It requires an x86-64 processor with AVX-512 VBMI support, found in Intel Ice Lake Xeon servers and AMD Zen 4 processors, among others. ARM64 systems do not use the vulnerable code. Oracle also lists affected GraalVM releases and a specific Java 8 performance build, 8u511b33-perf.

On susceptible systems, Calif demonstrated code execution before login in default configurations of Keycloak, Jira, Confluence, JFrog Artifactory, Jenkins, and PingFederate. Its demonstrations against Apache Kafka, Solr, and Elasticsearch required an authenticated account. Java clients can also be exposed when they connect to a malicious server.

Oracle rates the flaw 8.1 out of 10 under CVSS 3.1 and considers it difficult to exploit. As of November 17, the Multi-State Information Sharing and Analysis Center had reported no known cases of active exploitation.

### What users should do now

Home users should update affected applications and devices through their official update mechanisms. If a program bundles Java, its publisher may deliver the fix as an application or firmware update. Updating the system-wide Java installation alone can leave that private copy in place.

Administrators should prioritize internet-facing services and check which Java runtime each application actually uses, including copies inside container images. Oracle’s patched builds include 17.0.21.1, 21.0.13.1, 25.0.5.1, and 27.0.1.1. For other distributions, check the supplier’s advisory for its fixed package. Rebuild affected images, restart or redeploy services, and verify the version in the running application.

If a patch cannot be deployed immediately, Calif recommends restarting the affected Java process with `-XX:-UseBASE64Intrinsics` to disable the vulnerable acceleration. Treat this as a temporary measure. On previously exposed systems, review logs for unusual Java crashes or unexpected child processes. Patching closes the vulnerability, but an earlier intrusion still requires investigation.

## Leave a Reply
