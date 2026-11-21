---
id: collect-261121-veille-ia/securite/one-padding-character-sixty-two-bytes-inside-java4shell-the-openjdk-base64-bug
title: "One Padding Character, Sixty-Two Bytes: Inside Java4Shell, the OpenJDK Base64 Bug"
domain: securite
role: reference
task: article
actors: []
dates: ["2026-11-17"]
keywords: ["bytes", "calif", "character", "inside", "openjdk", "base64", "code", "java"]
source: https://medium.com/@raka824pratama/one-padding-character-sixty-two-bytes-inside-java4shell-the-openjdk-base64-bug-e0adb74d7aee
source_anchor: ""
source_lines: [1, 90]
section: "Securite"
---
# One Padding Character, Sixty-Two Bytes: Inside Java4Shell, the OpenJDK Base64 Bug

## Introduction

Base64 is one of the least glamorous pieces of code in a Java application. It decodes a JWT, a SAML assertion, an HTTP basic-auth header, a certificate, a cookie. Almost every network-facing Java service calls it on data that an unauthenticated stranger controls.

On November 17, 2026, the security firm Calif published a write-up of a memory-corruption bug in exactly that code. It named the flaw Java4Shell and tracked it as CVE-2026–114938. Oracle shipped fixes the same day in its November 2026 Critical Patch Update. According to Calif, the bug lives in an optimized x86–64 routine inside the HotSpot JVM, and with some application-specific work it can be steered from “out-of-bounds write” to “remote code execution” in products such as Keycloak, Jenkins, Jira and Confluence.

The story is more interesting than the headline. The bug is small, the trigger is a 65-character string, and the affected code only runs on certain CPUs. This article walks through the mechanism, separates what is established from what is still disputed, and gives a practical checklist.

## What happened

The timeline below comes from Calif’s own write-up (dates in Pacific Time) and is worth reading closely, because it shows that this was not a surprise to everyone:

- **June 9** => A user (“sundarms”) opens an Adoptium support issue after hitting the bug in practice
- **Sept 8–9** => A proof of concept and the root cause are identified publicly; a fix is proposed in`openjdk/jdk#32781` , which Calif says was closed without being merged
- **Oct 24** => Calif finds the bug (using OpenAI's Codex, per Calif) and reports it to Oracle
- **Nov 13** => Oracle says the fix is ready and will ship on Nov 17
- **Nov 17** => Oracle releases patches; Calif publishes its write-up

BleepingComputer, citing Calif, credits researcher Quang Luong with the find and says discovery took roughly an hour with AI assistance, with a first working exploit taking about two more hours and under $200 of AI usage. Those are Calif’s numbers as relayed by one outlet; I could not independently confirm them. The detail that matters more for defenders is that the bug had been publicly visible, as a crash report and a rejected fix, months before the coordinated disclosure.

## How the bug works

The vulnerable code is not Java source. It is an AVX-512 intrinsic: a hand-written, JIT-compiled assembly routine that the C2 compiler substitutes for `java.util.Base64`'s decoder when the CPU supports the AVX-512 VBMI instruction set. JFrog's analysis, the most detailed I found, describes the failure like this:

1. The routine computes how many output bytes the remaining input will produce as `(len / 4) * 3` , then subtracts one for each trailing`=` padding character.
2. For a malformed tail such as `=` ,`X=` or`===` , that arithmetic goes negative and ends up as`-1` .
3. The store mask is built with a `BZHI` instruction, which only looks at the low 8 bits of the count.`-1` becomes`255` . Any value of 64 or more clears no bits, so the mask is all ones.
4. A masked `VMOVDQU8` store then writes a full 64 bytes where it should have written none.

JFrog’s worked example uses a 50-byte output array that already has 48 bytes filled. The bad store starts at offset 48, so two bytes land inside the array and 62 bytes land beyond it, on the heap. Java then throws `IllegalArgumentException` because the input was invalid, but by then the write has already happened.

The trigger reported by Calif, BleepingComputer and Orca is 64 `A` characters followed by a single `=`. On older OpenJDK 17 builds, 128 `A`characters are needed. Calif's write-up includes a self-contained Java trigger that warms up the decoder and then decodes that input; on an affected JVM it demonstrates corruption, not a full exploit.

From corruption to code execution is a longer road. As JFrog summarizes it, the overflow can enlarge the length field of an adjacent array, which gives out-of-bounds reads (a heap-disclosure primitive) and writes. Writes can then reach the JVM’s code cache. Because this depends on heap layout, there is no universal exploit: each application needs its own chain. Calif says it is withholding the weaponized chains until people have had time to upgrade.

## Who is affected, and who is not

Three conditions must all hold.

The JVM is in a vulnerable range. Sources agree on “OpenJDK 17.0.6 and later.” JFrog’s table lists the affected Oracle builds as 17.0.21.0.3, 21.0.13.0.3, 25.0.5.0.3 and 27.0.1.0.3, plus the Java 8 Enterprise Performance Pack build 8u511b33-perf. Standard Java 8 and 11 and OpenJ9 are reported as unaffected. The downstream builds named by Calif include Amazon Corretto, Eclipse Temurin, Microsoft Build of OpenJDK, Azul Zulu, BellSoft Liberica, SAP SapMachine, Red Hat OpenJDK and Oracle GraalVM.

The CPU supports AVX-512 VBMI. Calif and Wiz name Intel Xeon from Ice Lake onward (Ice Lake, Sapphire Rapids, Emerald Rapids, Granite Rapids) and AMD EPYC and Ryzen at Zen 4 or newer. ARM64 hosts, including AWS Graviton and Apple Silicon, are reported as unaffected, and JFrog says the scalar and AVX2 decode paths are not affected.

The application feeds attacker-controlled Base64 to the decoder. This is the condition that varies. Calif and JFrog claim pre-authentication RCE in products including Keycloak, Jira, Confluence, JFrog Artifactory, Jenkins and PingFederate, and post-authentication RCE in Kafka, Solr, Elasticsearch, Gerrit and Spinnaker. JFrog also describes a client-side variant, where a Java application connecting to a malicious TLS server can be attacked. I could not verify these product-level claims; they come from the researchers and the vendors repeating them.

This is why cloud fleets deserve attention. Many teams run mixed instance types, and a service that is safe on a Graviton node can be exploitable on an Intel Sapphire Rapids node running the same container image.

## What is still uncertain

Reading the sources side by side exposes several disagreements, and a careful reader should know about them.

- Severity. Calif scores the flaw 9.2 (Critical) under CVSS 4.0. BleepingComputer, JFrog and Sonatype report Oracle’s score as 8.1 (High) under CVSS 3.1. Sonatype and Orca note, however, that Oracle’s pre-release notice did not name this CVE, so tying Oracle’s 8.1 to it is not fully confirmed. None of the pages I read gave a CVSS vector string.
- Patch confirmation. JFrog, Wiz and BleepingComputer treat the November 17 builds as the fix. Sonatype and Orca, written the same day, say Oracle had not yet confirmed the CVE among them. JFrog lists the fixed Oracle builds as 17.0.21.1, 21.0.13.1, 25.0.5.1, 27.0.1.1 and 8u513-perf. Calif’s page does not list fixed version numbers. Downstream vendors ship their own build numbers, so confirm with your supplier.
- Exploitation in the wild. None of the sources I read reports attacker exploitation. Orca explicitly says it knew of none at the time of writing. Wiz says adversaries “should” be expected to target Java applications, which is a forecast, not a report.
- Workaround strength. Calif and Wiz recommend either `-XX:-UseBASE64Intrinsics` or`-XX:UseAVX=2` . Orca cautions that neither workaround was validated by Oracle or OpenJDK, and that on Java 21 and later`UseAVX=2` still uses an AVX2 path, so the Base64 intrinsic stays active there. Sonatype says the flags are not a substitute for patching and can affect performance.
- Java 11 and 8. Orca mentions releases for 8 and 11 in the same update. JFrog says standard Java 8 and 11 are not affected. Treat only the Performance Pack build as in scope until your vendor says otherwise.
- Details I could not check. The OpenJDK advisory page returned HTTP 403 and the SentinelOne analysis returned HTTP 429, so nothing here comes from them.

## What to do now

The order matters, because restarting is the step people forget.

1. Inventory. Find every JVM in your estate that is Java 17.0.6 or newer, including bundled JREs inside products such as Jenkins, Keycloak, Atlassian tools and Artifactory, and inside container base images.
2. Check the hardware. On Linux, look at the CPU flags. Wiz publishes a `grep` against`/proc/cpuinfo` for`avx512f` ,`avx512bw` ,`avx512vbmi` and`bmi2` . One caution from my own knowledge, not from the sources: the Linux kernel commonly reports the VBMI flag as`avx512_vbmi` , with an underscore. If a flag-matching script says "not capable" on a modern Intel or AMD server, verify the flag spelling on that host before trusting the result.
3. Patch. Move to the fixed build from your vendor and rebuild container images on patched base images. Sonatype stresses that replacing runtime files does not update a JVM that is already running, so restart the service.
4. If you cannot patch yet, consider one of the two JVM flags above, test it first, and remember it is a stopgap. `-XX:UseAVX=2` makes a JVM on a non-x86 CPU fail at startup with`Unrecognized VM option 'UseAVX=2'` (JFrog), so apply it only on x86-64 hosts.
5. Reduce exposure. Wiz recommends restricting internet access to management portals such as Jira, Jenkins and Keycloak until they are patched. Calif notes that request-body buffering in nginx is not a general mitigation.

## Why this matters beyond one CVE

First, the vulnerable code was an optimization. The intrinsic exists to make Base64 faster, and it is the fast path that broke. Performance-oriented, hand-written vector code is hard to review and often bypasses the safety guarantees that make Java attractive. It is a recurring bug class, not a one-off.

Second, hardware-conditional bugs break your mental model of “same image, same risk.” Exposure depended on which CPU a pod landed on. Vulnerability scanners that look only at package versions will over- or under-report.

Third, AI-assisted vulnerability research is compressing timelines. Calif says Codex surfaced this bug in about an hour, and Wiz says its own AI harness produced a proof of concept within two hours. Both are vendor claims, but the direction is consistent across sources: the gap between “patch released” and “working exploit exists” is shrinking, and here the bug had also been public as a crash report since June.

## Conclusion

Java4Shell is a good reminder that the dangerous bugs are not always in the code we think of as risky. A decoder that handles a few dozen bytes at a time, a negative number that becomes 255, and a mask that is all ones were enough to turn a malformed padding character into a heap write. The facts that are solid are clear: an AVX-512 VBMI code path, a 62-byte out-of-bounds write, fixes shipped on November 17. The facts still being settled, namely the exact severity score, the vendors’ confirmation of the CVE mapping and the strength of the workarounds, are exactly why you should read your own vendor’s advisory rather than trust a headline. Patch, restart, and check what your servers’ CPUs actually are.

## References

- Calif, “Java4Shell” write-up and disclosure timeline: https://calif.io/java4shell
- BleepingComputer, “Critical Java4Shell flaw allows RCE attacks on major Java applications”: https://www.bleepingcomputer.com/news/security/critical-java4shell-flaw-allows-rce-attacks-on-major-java-applications/
- JFrog Security Research, “Java4Shell: What We Know About the Critical OpenJDK RCE Vulnerability (CVE-2026–114938)”: https://research.jfrog.com/post/java4shell-vulnerability/
- Wiz, “CVE-2026–114938 OpenJDK RCE (Java4Shell): Everything You Need to Know”: https://www.wiz.io/blog/cve-2026-114938-rce-java4shell-openjdk
- Orca Security, “Java4Shell CVE-2026–114938: Affected Versions & Mitigation”: https://orca.security/resources/research/java4shell-critical-openjdk-base64-decoder-flaw-could-allow-unauthenticated-remote-code-execution/
- Sonatype, “Critical OpenJDK RCE CVE-2026–114938: What to Know”: https://www.sonatype.com/blog/critical-openjdk-rce-cve-2026-114938-what-to-know
