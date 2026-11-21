---
id: collect-261121-veille-ia/securite/calif-uses-ai-to-find-java4shell-as-openjdk-and-oracle-ship-fixes
title: "Calif Uses AI to Find Java4Shell as OpenJDK and Oracle Ship Fixes"
domain: securite
role: reference
task: article
actors: []
dates: ["2026-11-17"]
keywords: ["openjdk", "calif", "java", "oracle", "java4shell", "uses", "compiler", "flaw"]
source: https://www.unite.ai/calif-uses-ai-to-find-java4shell-as-openjdk-and-oracle-ship-fixes/
source_anchor: ""
source_lines: [1, 39]
section: "Securite"
---
## Key PointsAI-assisted review of the JIT compiler surfaced a critical flaw in hand-written…

View more

- AI-assisted review of the JIT compiler surfaced a critical flaw in hand-written assembly that bypasses Java memory safeguards.
- Exploitation demands target-specific profiles and heap grooming so the flaw does not yield a universal payload.
- The Base64 optimization shipped to every affected runtime while benefiting only applications decoding enormous data volumes.

Calif on November 17, 2026 disclosed Java4Shell, an OpenJDK vulnerability discovered with AI assistance that it rated 9.2 under CVSS 4.0 and that can let an unauthenticated attacker corrupt memory and execute code remotely. OpenJDK and Oracle released fixes the same day.

Tracked as CVE-2026-114938, the flaw sits in hand-written x86-64 assembly used by OpenJDK HotSpot to accelerate Base64 decoding with AVX-512 VBMI instructions. Its location undercuts a familiar Java safeguard: applications benefit from managed memory and memory-access checks, but this part of the runtime is written in assembly, where those protections do not eliminate memory-corruption risk.

Calif reports that researcher Quang Luong directed Codex to examine the JIT compiler, intrinsics, widely accessible APIs and cryptography before the model surfaced the Base64 finding, with discovery taking one hour, the first working exploit another two hours and total token costs below $200. Luong chose the targets and supplied exploitation expertise, while Calif built pipelines and a heap tracer and visualizer to help AI agents develop later chains.

## A Padded Base64 Tail Corrupts the Heap

The optimized routine mishandles padding in malformed input. A 65-character string containing 64 `A` characters and `=` produces a 50-byte output allocation; an invalid negative output length then enables a 64-byte write and sends 62 bytes beyond that allocation. Java eventually rejects the input, but neighboring objects may already be corrupted. The report gives a 128-`A` plus `=` trigger for OpenJDK 17 because its threshold for reaching the affected code path is higher.

Calif reports that its chain corrupts a buffer length to upgrade the fixed overwrite into chosen-value writes, then uses another corrupted array to disclose heap contents such as private keys and tokens. Demonstrated impact includes pre-authentication RCE in default configurations of Keycloak, Atlassian Jira, Atlassian Confluence, JFrog Artifactory, Jenkins and PingFederate, and post-authentication RCE in Apache Kafka Broker, Apache Solr, Elasticsearch, Gerrit and Netflix Spinnaker. The researchers also demonstrated RCE in sample Apache Struts and Spring Boot applications, and the published product lists are non-exhaustive.

A vulnerable Java process need not expose a service of its own. Researcher samczsun separately shared a client-side exploit with Calif showing that a Java program opening a TLS connection to a malicious server can be compromised.

## Hardware Requirements and Target-Specific Exploits

Calif says Java4Shell affects OpenJDK 17.0.6 and later and distributions built from OpenJDK, including Amazon Corretto, Eclipse Temurin, Microsoft Build of OpenJDK, Azul Zulu, Red Hat OpenJDK, Oracle Java SE and Oracle GraalVM for JDK. The vulnerable code also requires an x86-64 processor with AVX-512 VBMI support. ARM64 processors are not affected; listed examples meeting the hardware requirement include Intel Xeon Ice Lake, Sapphire Rapids, Emerald Rapids and Granite Rapids, plus AMD EPYC and Ryzen processors based on Zen 4 or newer.

The severity ratings differ by scoring system. Calif’s 9.2 critical score uses CVSS 4.0, while the OpenJDK advisory and Oracle’s November update assign 8.1 under CVSS 3.1. OpenJDK marks Java 17, 21, 25 and 27 as affected and classifies the issue under hotspot/compiler. Oracle assesses it as remotely exploitable over a network without authentication, with high attack complexity and high impacts on confidentiality, integrity and availability.

Those demonstrations do not create a universal exploit payload. Calif says each target needs a bespoke profile containing a decoder route, a live writer, a reader or another feedback path, a suitable heap layout and an application action. Its remote-grooming approach is sensitive to round-trip time and jitter, and incorrect object placement can crash the JVM, though retries are often possible. Buffering complete request bodies can defeat some grooming profiles, but not all: the Elasticsearch chain works from one complete request, and configured WebSocket tunnels change the buffering conditions.

## Fixes and Temporary Mitigations

OpenJDK’s November 17, 2026 advisory recommends upgrading as soon as possible. Oracle’s JDK 17.0.21.1 release notes carry the same release date and link the security fixes to its November advisory, which contains 11 new Java SE patches and credits Quang Luong of Calif.io and Albert Yang for CVE-2026-114938.

The mainline OpenJDK change adds a four-line guard that sends a padded Base64 tail shorter than one complete block to the Java handler rather than the affected optimized routine. Until updates are applied, Calif lists `-XX:-UseBASE64Intrinsics` to disable hardware-accelerated Base64 encoding and decoding, or `-XX:UseAVX=2` to disable AVX-512, as temporary mitigations. The latter is only for x86-64 hosts; OpenJDK on ARM does not recognize it and will fail to start.

Calif published the vulnerability mechanics and a bounded standalone trigger but is withholding application-specific exploit chains while users upgrade. That limits the immediately available material to the underlying flaw and general exploitation technique rather than ready-made attacks against individual products.

Calif’s broader argument is that AI can also help maintainers identify and remove unnecessary features and optimizations. Java4Shell gives that position a concrete case: an optimization intended for applications decoding enormous amounts of data shipped to every affected runtime, exposing applications that gained little from it. The same AI-assisted workflow that found the bug is thus being presented as a way to reduce attack surface, not only to discover and weaponize flaws.
