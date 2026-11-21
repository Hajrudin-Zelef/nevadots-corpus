---
id: collect-261121-veille-ia/securite/cisa-urges-immediate-patching-of-java4shell-flaw-that-could-allow-remote-code-execution
title: "CISA Urges Immediate Patching of Java4Shell Flaw That Could Allow Remote Code Execution"
domain: securite
role: reference
task: article
actors: []
dates: []
keywords: ["java", "flaw", "keycloak", "cisa", "code", "security", "remote", "execution"]
source: https://thehackernews.com/2026/11/cisa-urges-immediate-patching-of.html
source_anchor: ""
source_lines: [1, 66]
section: "Securite"
---
The U.S. Cybersecurity and Infrastructure Security Agency (CISA) has urged organizations to immediately patch a newly disclosed critical security flaw impacting Oracle Java.

The flaw, tracked as **CVE-2026-114938** (CVSS score: 8.1), refers to a case of out-of-bounds write that can lead to denial-of-service (DoS) or remote code execution under certain conditions.

"This critical memory-corruption vulnerability, known as 'Java4Shell,' affects the Base64 decoder of the x86-64 HotSpot Java Virtual Machine (JVM), which is part of the Java Development Kit (JDK)," CISA said. "Specially crafted Base64 input could crash the JVM and allow for unauthenticated remote code execution."

Patches for the flaw have been released by Oracle and OpenJDK. As of writing, there is no evidence the vulnerability has been exploited in the wild.

As reported previously by The Hacker News, Java4Shell can enable attackers without an account to execute arbitrary code on Keycloak, Atlassian Jira, Atlassian Confluence, JFrog Artifactory, Jenkins, and PingFederate, and with a login on Apache Kafka Broker, Apache Solr, Elasticsearch, Gerrit, and Spinnaker, under default settings.

Advisories for the flaw have been released by numerous security and software vendors, including Aikido, Amazon Linux, Blackpoint Cyber, Chainguard, Cisco Talos, Cycode, Datadog, Dynatrace, Endor Labs, Field Effect, Hadrian, Horizon3.ai, Miggo, Orca Security, Qualys, Rapid7, Red Hat, SentinelOne, Sophos, Tanium, Tenable, Upwind, VulnCheck, watchTowr, and Wiz.

In an update shared on November 18, 2026, the Keycloak team said an instance can be considered at risk if the following conditions apply -

- It runs an unpatched OpenJDK build from 17.0.6 onward, including unpatched OpenJDK 21 and 25 builds
- Its x86-64 host or virtual machine exposes the avx512vbmi CPU feature to Java
- The running JVM has Base64 intrinsics enabled and has not been limited to AVX2 with -XX:UseAVX=2

"The x86-64 Keycloak container images tagged 26.8.2-1 and 26.8.2-2 bundle an affected OpenJDK 21.0.13 build," the team said. "These image tags alone do not establish whether your deployment is exploitable: check the CPU feature and effective JVM flags."

"A Keycloak ZIP or TAR.GZ distribution started directly on a host can also be affected; the conditions depend on its Java runtime and CPU, not on whether it runs in a container."

Keycloak is also recommending that users run the below command to check the CPU feature on a host or virtual machine -

`grep -m1 '^flags' /proc/cpuinfo | grep -qw avx512vbmi && echo 'AVX-512 VBMI available'`
If the output of the command returns nothing, it indicates that the CPU does not expose AVX-512 VBMI and is not known to be affected by the flaw. Keycloak has released temporary mitigations that users can apply until it releases updated Keycloak and Keycloak Operator container images once patched OpenJDK builds are available for them.

"The vulnerability could be reached through a local Spring Boot application using Spring Security," Blackpoint Cyber researchers Sam Decker and Nevan Beal said.

"In this environment, approximately 28,000 unauthenticated HTTP requests carrying a malformed Base64 authentication value caused the JVM to terminate after roughly 20 seconds of sustained requests. Additional testing confirmed corruption of nearby Java objects and changes to live application data without necessarily causing an immediate crash."

CISA is recommending that organizations take the following steps -

- Identify affected Java installations.
- Review software inventory for JDKs and Java runtimes, including those bundled with third-party applications or included in container images and appliances.
- Apply relevant Java and vendor patches as soon as they are available.

"If immediate patching is not feasible, implement temporary compensating controls, such as restricting and validating Base64 input at the application boundary, enforcing input-size limits, and limiting unauthenticated access," CISA said.

### Update

In a statement shared with The Hacker News, watchTowr said it's observing initial attempts to exploit the flaw against its sensors.

"We are already seeing attackers spray the internet and hit our global honeypot network with requests decorated in 'java4shell-recon' User-Agents, alongside blind exploitation attempts using correctly shaped requests," Jake Knott, head of threat intelligence at watchTowr, said.

"Even though this latest vulnerability is far trickier to exploit, and requires specific prerequisites to be in use, where a Java-based application must be running on a particular CPU and JDK, and be handling attacker-controlled Base64 -- this will not stop attackers from trying their luck."

In a post shared on X, Cloudflare said it detected the "first instance of ARTEX and Java4Shell being used from the same IP address." ARTEX is an open-source artificial intelligence (AI) penetration testing framework that threat actors have increasingly adopted to automate cyber attacks.

Telemetry data from Previdian has recorded nine exploitation attempts against five of its sensors to date. The attack efforts have originated from six unique IP addresses in China, Germany, Romania, and the U.S.

Organizations are advised to identify internet-facing Java applications for patching and remediation. If immediate update is not possible, it's recommended to consider restricting access or implementing additional mitigating controls, like disabling Java's use of AVX-512.

Advisories and write-ups have also been published by a number of companies over the past 24 hours -

As of November 19, 2026, the Keycloak team has announced updated images containing the fixes for CVE-2026-114938. These include latest, 26.8, 26.8.2, and 26.8.2-3.

In a follow-up analysis, Aikido said it traced the vulnerable Base64 implementation back to a change that merged into OpenJDK on June 25, 2021.

"AI has reduced the bar for finding and exploiting vulnerabilities in the wild, causing mass panic every week with a new Something2Shell dropping," OX Security's Moshe Siman Tov Bustan and Nir Zadok said.

"And while some are actually relevant and exploitable, or just very cool (but impractical) to learn about, (like the *sshole OpenSSH vulnerability), the mass panic causes more harm than good. Security teams fight alert fatigue and the constant feeling of 'everything is important,' but in reality, exploitation of Java4Shell is more like Java4Shell_WITH_X86_AND_Base64_AND_OPENJDK – which isn’t as cool as just saying Java4Shell, right?"

"Teams that really want to keep their systems safe must first check the basics – Are we internet exposed? Is this connected to a real system, with valuable data? Do we meet all of the criteria for exploitation? – before they go into full-on panic mode."

*(The story was updated after publication to include the latest insights.)*
