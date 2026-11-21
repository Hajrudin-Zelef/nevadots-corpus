---
id: collect-261121-veille-ia/gpt-openai/java4shell-a-calif-researcher-used-openai-codex-to-find-an-openjdk-base64-memory-corruption-rce-in-o
title: "Java4Shell: A Calif Researcher Used OpenAI Codex to Find an OpenJDK Base64 Memory-Corruption RCE in One Hour for Under $200 in Tokens — AICoder"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["under", "openjdk", "calif", "base64", "researcher", "codex", "hour", "tokens"]
source: https://aicoder.com/news/news-20261117-java4shell-cve-2026-114938-openjdk-base64-rce-found-with-openai-codex-in-one-hour-under-200-dollars-calif
source_anchor: ""
source_lines: [1, 45]
section: "GPT_OpenAI"
---
Security firm Calif disclosed Java4Shell (CVE-2026-114938) on November 17, a memory-corruption bug in OpenJDK's AVX-512 VBMI accelerated Base64 decoder on x86-64, scored 9.2 under CVSS 4.0. Researcher Quang Luong had OpenAI Codex audit HotSpot, found the bug in about an hour and built the first working exploit in two more hours, for under $200 in tokens. Calif achieved unauthenticated RCE against default configurations of Keycloak, Jira, Confluence, Jenkins and others. OpenJDK and Oracle released fixes the same day.

## Key Takeaways

- ✓The bug: HotSpot's x86-64 Base64 intrinsic miscalculates the output length when handling trailing padding, writing up to 62 bytes out of bounds; 64 A characters plus one = trigger it
- ✓AI's role: Quang Luong had Codex audit the JIT, intrinsics and crypto code; early findings were mostly noise, and the real bug surfaced after about 20 minutes; 1 hour to find, 2 more to exploit, under $200 in tokens
- ✓Scope: OpenJDK 17.0.6 and later plus distributions such as Corretto, Temurin, Microsoft Build of OpenJDK, Zulu and Oracle Java SE, on x86-64 CPUs with AVX-512 VBMI; ARM64 is not affected
- ✓Demonstrated: unauthenticated RCE on default Keycloak, Jira, Confluence, Artifactory, Jenkins and PingFederate; authenticated RCE on Kafka, Solr, Elasticsearch and others
- ✓Severity: 9.2 under CVSS 4.0 per Calif and 8.1 under CVSS 3.1 in the OpenJDK advisory; the -XX:-UseBASE64Intrinsics flag is a temporary mitigation

### Key Decision Metrics at a Glance

#### Turn your technical choice into a development budget

Compare 40 dev plans & simulate token costs vs $20/mo subscriptions

### Project Links & Resources

Direct Access
## In-Depth Technical Analysis

### Background

Java is known for memory safety, but much of the JVM itself is written in C, C++ and assembly. Calif says this is its second high-severity bug in core software found with AI after PHP2Shell, at very low cost, a sign that AI is sharply lowering the bar for vulnerability research.

### How it works

On x86-64 CPUs with AVX-512 VBMI, OpenJDK decodes Base64 with hand-written assembly. For a final chunk under 64 bytes that ends in =, the output length goes negative, the store mask becomes all ones, and a full 64-byte write runs past the end of the array. Java eventually rejects the input, but neighboring objects may already be corrupted, which can leak memory or lead to remote code execution. The researcher had Codex audit the JIT and intrinsics like a security researcher and filtered out the false leads himself.

### Benchmarks

This is a security disclosure, not a model benchmark. Calif's figures: about 1 hour to find the bug, 2 more hours for the first exploit, under $200 in AI tokens, with most later application exploit chains built almost autonomously by AI agents. In its official X post, Wiz said its own AI vulnerability research harness, Wiz Atlas, produced a proof of concept in two hours.

### What it means for developers

Upgrade to a JDK build that includes the November 17 fix as soon as possible. If you cannot yet, start the JVM with -XX:-UseBASE64Intrinsics, or with -XX:UseAVX=2 on x86-64 only (ARM builds reject that flag and fail to start). On Linux, your hardware is affected only if /proc/cpuinfo lists all of avx512f, avx512bw, avx512vbmi and bmi2.

Benchmark side-by-side against alternatives, or calculate monthly token cost vs subscription break-even.

## Discussion & Comments

0
Sign in to join the discussion

Connect with AI developers to exchange benchmark insights.
