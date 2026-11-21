---
id: collect-261121-veille-ia/securite/java4shell-pre-auth-rce-in-java-puts-your-apis-at-risk
title: "Java4Shell: Pre-Auth RCE in Java Puts Your APIs at Risk"
domain: securite
role: reference
task: article
actors: []
dates: ["2026-11-18"]
keywords: ["java", "calif", "november", "java4shell", "openjdk", "oracle", "found", "runtime"]
source: https://www.datatheorem.com/blog/java4shell-cve-2026-114938-java-base64-api-security/
source_anchor: ""
source_lines: [1, 89]
section: "Securite"
---
*The flaw lives in the Java runtime, not a library, and the requests that reach it arrive before any login. Here is how to tell if you are exposed and what to do now.*

*Updated November 18, 2026: Calif published its full write-up, OpenJDK and Oracle released fixes, and CISA urged immediate patching. This post now covers the demonstrated remote code execution and the fixed builds.*

## TL;DR

Java4Shell (CVE-2026-114938) is a heap overflow in the Base64 decoder built into the Java runtime. Calif, the firm that found it with AI, has shown it can lead to remote code execution (RCE) with no login on the default configurations of widely used Java applications, including Keycloak, Jira, Confluence, Jenkins and PingFederate. It affects Java 17.0.6 and later on x86-64 servers with AVX-512 VBMI, wherever untrusted Base64 reaches the decoder, as it does in many API login and token flows. Fixed builds are out. Patch now, and use the JVM flag `-XX:-UseBASE64Intrinsics` where you cannot yet.

## What Is Java4Shell?

Calif researcher Quang Luong found the flaw. Details leaked before the fix, and on November 17, 2026, the OpenJDK Vulnerability Group confirmed it, Oracle shipped fixed builds and Calif published its write-up. The same day, the Cybersecurity and Infrastructure Security Agency (CISA) urged organizations to patch immediately. Calif rates it 9.2 (Critical) under version 4.0 of the Common Vulnerability Scoring System (CVSS); OpenJDK and Oracle score it 8.1 under CVSS 3.1.

The bug is in hand-written assembly that the Java Virtual Machine (JVM) uses to speed up Base64 decoding with AVX-512 instructions. A short malformed input ending in a stray padding character makes the routine miscalculate its output length and write about 62 bytes past the end of a buffer on the Java heap. Java then rejects the input with an ordinary error, but the damage is done. Because the bug lives in the runtime, not a library, it will not show up in a dependency scan.

The overflowed bytes are fixed, which is why early analyses predicted only a crash. Calif showed otherwise. By arranging the heap so the overflow enlarges a neighboring array, an attacker can read memory back through the application's normal responses, a step Calif calls JavaBleed, and from there reach code execution. That memory can hold keys, tokens and personal data. Calif also reports a client-side attack: a Java program that connects over TLS to a server an attacker controls can be compromised, even if it offers no service of its own.

There are real limits. Each application needs its own exploit, and a failed attempt can crash it, though attackers can often retry. Calif published a safe proof of concept that reproduces the corruption and is holding back its application-specific exploits for now. As of November 18, no exploitation in the wild has been reported, and the flaw is not in CISA's Known Exploited Vulnerabilities catalog.

CVE-2026-114938: Java4Shell, Pre-Authentication RCE Through the Java Base64 Decoder

A heap overflow in the Java runtime's AVX-512 Base64 decoder on x86-64 servers, in Java 17.0.6 and later, including the 21, 25 and 27 lines. Calif found it with AI assistance, and it leaked before disclosure on November 17, 2026. Calif demonstrated pre-authentication remote code execution on the default configurations of widely used Java applications and rates it 9.2 under CVSS 4.0; OpenJDK and Oracle score it 8.1 under CVSS 3.1. Oracle and OpenJDK released fixes on November 17, 2026. No exploitation in the wild has been reported as of November 18, and the flaw is not in CISA's Known Exploited Vulnerabilities catalog.

*Sources:* *CVE-2026-114938**;* *OpenJDK advisory**;* *Oracle November 2026 security update**;* *Calif write-up**.*

## Found in an Hour, Exploited in Three

Calif found Java4Shell the same way it found PHP2Shell: with AI. Quang Luong pointed an AI coding agent at the most complex parts of the Java runtime, spent about 20 minutes setting aside false leads, and had the bug within an hour. A first working exploit took two more hours. The total AI cost was under $200.

The exploit is the part that matters. Turning an overflow of fixed bytes into code execution used to take rare skill and a lot of time, and that difficulty quietly protected everyone. Calif built its strategy once, then let AI agents produce most of the application-specific chains almost on their own.

As we wrote about PHP2Shell and Node2Shell, AI adds capacity, not just capability. One team now does what used to take many. Waiting for a working exploit before you fix is no longer a strategy.

## Am I Affected?

You are exposed only if all three conditions hold on the same host. Running Java 17 alone does not put you at risk.

| Condition | At risk | Not at risk | How to check | 
|---|---|---|---|
| Java runtime | 17.0.6 and later, plus all 21, 25 and 27 releases, until patched; also Oracle Java SE 8u511b33-perf | Standard Java 8 and 11, and 17.0.0 to 17.0.5 | java -version on the host, or jcmd <PID> VM.version for a running process | 
| Processor | x86-64 with AVX-512 VBMI: Intel Xeon Ice Lake, Sapphire, Emerald and Granite Rapids; AMD EPYC and Ryzen Zen 4 and later | Older Xeons (Skylake, Cascade Lake), AMD EPYC Rome and Milan, all Arm (Graviton, Ampere, Apple Silicon) | On Linux, the command below must return all four flags | 
| Input path | Untrusted data reaches java.util.Base64 decoding | Services that never decode untrusted Base64 with java.util.Base64 | Review where request data is Base64-decoded | 

Every OpenJDK-based build carries the same code, including Oracle Java SE, Eclipse Temurin, Amazon Corretto and Microsoft Build of OpenJDK. Standard Java 8 and 11 are not affected, but apply Oracle's November update to them for its other fixes. The risk follows the runtime and the hardware, not your source code: the same container image can be safe on one cloud instance family and exposed on another.

- List every Java runtime, including container base images and runtimes bundled in vendor products, with vendor, version and build.
- Flag the hosts and instance types whose processors report all four flags.
- Mark every service that runs an affected runtime on those hosts.
- In that set, find every API that decodes request data from Base64, and every Java client that connects out over TLS.

## Why It Lands on Your APIs

Calif reached its targets through ordinary features such as authentication and file uploads. Java frameworks commonly decode HTTP Basic authentication headers and JSON Web Tokens (JWTs) before they check the password or signature, so the decoder sits in front of the login.

That puts login and token endpoints, API gateways, webhook receivers and any API that accepts Base64 on the front line. The hardest part of the response is not the patch. It is the list: every API you run, including internal, partner and mobile backend APIs, and the runtime behind each one. The APIs nobody inventoried are the ones that get missed.

Mobile apps on phones are very unlikely to be affected, since nearly all phones run on Arm. The exposure is in the Java services behind the app.

## What to Do Now

- **Prioritize pre-authentication decoding:** internet-facing APIs, login and token endpoints, API gateways and webhook receivers first, then internal services and Java clients that connect to outside servers.
- **Patch.** Oracle's fixed builds are 17.0.21.1, 21.0.13.1, 25.0.5.1, 27.0.1.1 and 8u513-perf, and Amazon has released Corretto 17.0.21.1.2. Other vendors ship on their own schedules. Check each advisory and build number, since a back-ported fix may not change the version string.
- **Update every container image.** Change each Java`FROM` line, in every build stage, argument and digest, to the patched release. Replace the deprecated`openjdk` image with a maintained distribution, rebuild with`docker build --pull --no-cache` , and replace running containers.
- **Mitigate where you cannot patch yet** with`-XX:-UseBASE64Intrinsics` .`-XX:UseAVX=2` also works, but only on x86-64: Java on Arm will fail to start with it.
- **Verify the live build** with`jcmd <PID> VM.version` . A patched runtime on disk does nothing for a JVM that is still running.
- **Watch JVM crash logs** (`hs_err_pid*.log` ) on services that handle authentication headers or tokens. Failed exploit attempts can crash the application.

## How Data Theorem Is Responding

We are treating Java4Shell as a P1 for affected runtimes.

- **Finding exposed customers.** We are reviewing customer inventories for Java 17 and later services and will contact affected customers directly.
- **Mapping the APIs in the line of fire.** API Secure discovers every API across your environment, including shadow APIs nobody inventoried, so you can start with the ones that face the internet.
- **Detecting vulnerable runtimes in your builds.** Code SAST Secure now flags any Dockerfile that selects a Java base image with an affected runtime, as a P1 finding on by default, and names the patched image to move to.
- **In-product alerts.** Customers with potentially affected assets will see an alert in the Data Theorem portal with the steps above.

If attackers can go from bug to working exploit in three hours, defense has to move at the same speed. AI Exploits confirms which findings can actually be exploited, so teams fix real risk first. AI Auto-Remediation fixes what you find without waiting for proof. And AI Active Protection blocks attacks against live apps and APIs while fixes roll out. It is cover while you fix, not a reason to skip the fix.

**Schedule a demo** to see every API you run, including the ones nobody inventoried, and how Data Theorem protects them.

## Sources

- Calif: Java4Shell: Write once, RCE everywhere
- OpenJDK Vulnerability Advisory, November 17, 2026
- Oracle November 2026 Critical Security Patch Update
- CISA: Critical Java Vulnerability Requires Immediate Patching
- CVE-2026-114938 on cve.org
- oss-security: CVE-2026-114938 OpenJDK pre-auth RCE via memory corruption in Base64 decoder
- BleepingComputer: Critical Java4Shell flaw allows RCE attacks on major Java applications
- CyberScoop: Researchers disclose critical flaw in Java with help of AI
