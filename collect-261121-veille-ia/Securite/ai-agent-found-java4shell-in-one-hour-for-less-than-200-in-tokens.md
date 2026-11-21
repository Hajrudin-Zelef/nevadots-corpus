---
id: collect-261121-veille-ia/securite/ai-agent-found-java4shell-in-one-hour-for-less-than-200-in-tokens
title: "AI Agent Found Java4Shell in One Hour for Less than $200 in Tokens"
domain: securite
role: reference
task: article
actors: []
dates: []
keywords: ["java", "calif", "java4shell", "memory", "applications", "hour", "tokens", "weakness"]
source: https://insidetelecom.com/vulnerable-java-found-by-ai-in-a-single-hour/
source_anchor: ""
source_lines: [1, 49]
section: "Securite"
---
On November 18, *BBC* reported a cybersecurity researcher has used AI to uncover a critical weakness in vulnerable Java software, finding a security flaw in a component whose design dates back to more than a decade, despite the programming language’s widespread use across banking, government, and technology systems. 

The discovery, made by cybersecurity startup Calif, questioned how much older code remains unchecked inside everyday applications. Known as Java4Shell and tracked as CVE-2026-114938, the vulnerability affects OpenJDK, a widely used Java runtime, and could give attackers access to information that should remain protected.

## AI Finds Vulnerable Java in Memory Protection

Java has been around since 1995, and much of today’s digital infrastructure still depends on it. According to *BBC*, the discovery is troubling because Java was built with safeguards against memory interference, yet the weakness sits in one of its most trusted components. 

The problem sits inside OpenJDK’s Base64 decoder, which processes encoded data exchanged between applications. Researchers identified a memory corruption flaw that could allow specially prepared inputs to interfere with protected memory. In affected systems, attackers might access confidential information or execute commands without authorization, in some cases without needing credentials at all.

Calif rated the flaw 9.2, or critical, under version 4.0 of the Common Vulnerability Scoring System. Oracle assessed it at 8.1, or high, under the earlier 3.1 scale, leaving administrators with two accounts of how urgent it is.

The investigation took surprisingly little time. Calif researcher, Quang Luong, pointed at Codex, an AI coding agent, at parts of the Java Development Kit and identified the weakness through AI bug hunting in about an hour. Calif reported spending less than $200 on AI tokens, a small cost given the software involved.

“It took our own Quang Luong an hour to discover the vulnerability, and two more hours to develop the first working exploit,” Calif said in its technical write-up.

The one-hour discovery of Java4Shell has drawn attention to what automated security research can accomplish on a small budget. Researchers using AI code auditing can inspect large amounts of software without reviewing every instruction, finding weaknesses that survived years of conventional testing.

For developers, an AI audit for code quality offers another way to examine older applications before problems become serious. The discovery also raises concerns about application security vulnerabilities in Java, particularly where companies know little about the components inside their systems.

The issue is harder for businesses running vulnerable Java installations across multiple services. Some applications include older runtimes supplied by outside developers, leaving administrators with limited visibility into which versions are installed and whether they are exposed.

## Java4Shell Tests How Quickly Companies Can Respond

*TechTarget* reported that Calif demonstrated attacks against several widely used applications, including Jira, Confluence, Jenkins, Keycloak, JFrog Artifactory, and PingFederate. These products serve different purposes, yet they can share the same underlying Java components, allowing a single weakness to affect several parts of an organization’s operations. 

The memory corruption flaw could expose authentication tokens, private keys and other information held in memory. However, exploitation depends on particular hardware and software conditions, so not every installation faces the same risk.

The affected code reaches OpenJDK 17 from version 17.0.6 onward, plus Java 21, 25, and 27 releases built from it, and runs only on x86-64 processors supporting AVX-512 instructions, covering recent Intel and AMD server platforms. Standard Java 8 and 11 don’t contain the affected path, and ARM64 isn’t known to be affected. Downstream distributions, including Oracle Java SE, Amazon Corretto, and Eclipse Temurin, inherit it.

“A successful exploit chain can read Java heap memory before it runs any attacker code,” warned Saeed Abbasi of Qualys’ Threat Research Unit.

On November 17, Oracle and OpenJDK released coordinated OpenJDK security patches, the same day Calif disclosed the flaw, but getting those fixes into every affected system is a separate problem. Administrators hunting vulnerable Java builds must identify installed Java versions, check software supplied by other developers, and decide which services need attention first.

For businesses maintaining vulnerable Java environments, that work can take longer than expected. Deploying OpenJDK patches may require testing to ensure important applications continue operating normally, especially where older software is involved. Vendor timetables differ, with some distributions patched within days and others still preparing fixes.

The incident also highlights how AI discovered vulnerabilities in java open source dependencies affect numerous products sharing code that receive little direct attention.

Companies considering Java runtime optimization solutions for enterprises may need to weigh whether the speed gained is worth the exposure.

The case has also increased interest in the best AI tools for automated bug discovery, as researchers show how quickly weaknesses can be identified. AI bug hunting gives security teams more chances to examine aging software, similar capabilities could eventually help attackers search for weaknesses before updates become available.

That leaves developers facing an imbalance. AI code auditing can shorten the time needed to find vulnerabilities, but repairing them still involves testing, coordination and sometimes interruptions to essential services. Even with OpenJDK security patches available, businesses cannot always replace affected components immediately.

As of time of writing, no active exploitation had been confirmed, and the flaw had not been added to the US catalogue of known exploited vulnerabilities, which gives administrators a window rather than a reprieve.

For organizations relying on vulnerable Java systems, Java4Shell brings an old problem into sharper focus. Software that has worked reliably for years can still contain serious security mistakes, and as AI makes them easier to find, companies have less time to act before someone else does.

*Inside Telecom provides you with an extensive list of content covering all aspects of the tech industry. Keep an eye on our* *Cybersecurity* *sections to stay informed and up-to-date with our daily articles.*
