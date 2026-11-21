---
id: collect-261121-veille-ia/securite/java4shell-flaw-lets-attackers-run-code-on-java-servers
title: "Java4Shell flaw lets attackers run code on Java servers"
domain: securite
role: reference
task: article
actors: []
dates: ["2026-11-18"]
keywords: ["java", "openjdk", "code", "flaw", "reported", "java4shell", "oracle", "update"]
source: https://www.thefivenines.com/security/java4shell-flaw-lets-attackers-run-code-on-java-servers
source_anchor: ""
source_lines: [1, 29]
section: "Securite"
---
Security researchers at Calif disclosed a critical memory-corruption flaw in OpenJDK's Base64 decoder on Tuesday, BleepingComputer reported. CVE-2026-114938, nicknamed Java4Shell, affects OpenJDK 17.0.6 and later and Java distributions built from that code, such as Oracle Java SE, Amazon Corretto, Eclipse Temurin, Microsoft's OpenJDK build, Azul Zulu, and Red Hat's OpenJDK distribution. The issue appears only on x86-64 chips with AVX-512 VBMI support; ARM64 systems are not affected. Sending 64 'A' characters plus an equals sign can set off the corruption, while older OpenJDK 17 builds need 128 'A' characters. The decoder computes a length incorrectly and places 64 bytes into a buffer that might hold as few as 50, damaging nearby memory. Calif rated the bug 9.2 under CVSS 4.0, while Oracle assigned it 8.1 under CVSS 3.1.

The researchers showed the memory corruption can be turned into remote code execution (RCE). They showed unauthenticated RCE in Keycloak, Jira, Confluence, Artifactory, Jenkins and PingFederate, and authenticated RCE in Kafka Broker, Solr, Elasticsearch, Gerrit and Spinnaker. They also exploited sample applications built with Apache Struts and Spring Boot. Calif said there is no universal exploit; each target needs an application-specific path to deliver malicious Base64 to the vulnerable decoder. The group published a proof-of-concept for the memory corruption but withheld weaponized exploits until users have time to upgrade.

The bug was found by Quang Luong after about one hour auditing OpenJDK with OpenAI's Codex at a cost of less than $200 in computing tokens, and AI agents produced most of the group's application-specific exploit chains nearly on their own, CyberScoop reported. BleepingComputer reported that Luong built an initial exploit in another two hours. Oracle shipped patched Java builds Tuesday afternoon as part of its November Critical Patch Update, and OpenJDK published a security advisory the same day. CISA published a notice late Tuesday urging immediate patching. By the time of publication, no in-the-wild exploitation had been seen and CISA had not listed the CVE in its Known Exploited Vulnerabilities catalog. Oligo Security's co-founder and CTO, Gal Elbaz, said that not every Java application is vulnerable and exploitation is not straightforward, and that organizations should treat the flaw as a serious exposure requiring compensating controls to block exploitation until patches are rolled out.

For teams that cannot patch immediately, BleepingComputer reported that disabling the vulnerable hardware-accelerated Base64 function by launching OpenJDK with -XX:-UseBASE64Intrinsics or setting -XX:UseAVX=2 to disable AVX-512 can serve as temporary mitigations. The report said developers should update Java runtimes to the latest version. Defenders should inventory OpenJDK installations, check for AVX-512 VBMI processors, and apply Oracle's November update.

## Update, November 18, 2026

SOC Prime reported on Wednesday that Amazon has posted corrected Corretto builds and Azul has issued updated Zulu releases, while Red Hat had not yet shipped fixes for affected RHEL and OpenJDK packages at the time of the report. The report also says affected releases include Java 21, 25, and 27 builds derived from the vulnerable OpenJDK code, and notes that Oracle's Java SE 8 Performance Pack build, version 8u511b33-perf, appears in the affected list even though standard Java 8 and Java 11 lack that code path. It advises teams to inventory bundled and containerized JVMs and confirm the running JVM uses the patched build after updating, because downstream distributions and appliances may patch on different schedules.

## Update, November 18, 2026

Cisco said in a security advisory on Wednesday that it is investigating its product line and has confirmed that Java4Shell affects Catalyst Center, Smart PHY, Connected Mobile Experiences, Secure Network Analytics, Secure Workload, Native Broadband Network Gateway, Smart Software Manager On-Prem, Optical Network Planner, Services as Code Common Automation Framework and Intersight Virtual Appliance. Most fixes are still to be determined; Secure Network Analytics is listed for release 7.6.0 and Connected Mobile Experiences for 11.2. Identity Services Engine, IOS XE, IOS XR, NX-OS and UCS Central remain under investigation. Cisco said there are no workarounds. Cisco's PSIRT said it is aware that proof-of-concept exploit code is available but has not seen malicious use.

## Update, November 18, 2026

Fortinet's PSIRT said in a PSIRT advisory on Wednesday that it is investigating whether CVE-2026-114938 affects dozens of its products, including FortiVoice, FortiMail, FortiManager, FortiAnalyzer, FortiSandbox, FortiClient EMS and FortiSIEM. The company listed FortiOS, FortiProxy, FortiWeb and FortiClient desktop and mobile apps among products confirmed not affected and said no virtual patch is available.

Cloudflare said in a blog post the same day that it has released six managed WAF rules to detect the malformed Base64 input patterns that trigger Java4Shell. The rules are available on Pro, Business and Enterprise plans and are enabled in log-only mode by default because blocking could disrupt legitimate traffic, the company said. Cloudflare said it has seen no indication the flaw has been exploited on its infrastructure and is working on a free-plan rule.

## Update, November 19, 2026

SecurityWeek reported on Thursday that threat actors have begun targeting the Java4Shell flaw. GreyNoise had recorded more than 150 requests matching the shape of a Java4Shell attack from five IP addresses as of early November 19, but had not seen successful intrusions on its sensors. WatchTowr said it saw attackers scan widely and hit its honeypot network with requests carrying "java4shell-recon" User-Agents and blind exploitation attempts using correctly shaped requests. Previdian said its sensors collected nine exploitation attempts from six addresses on November 18 and 19, though it cautioned that the data cannot confirm any target was compromised. Cloudflare's Cloudforce One said some scanners are hiding their tools: one IP probed a target with a user agent that named Java4Shell, then came back roughly 17 hours later carrying the exploit trigger while presenting as Chrome. Cloudforce One also saw the same IP address appear in both Artex and Java4Shell traffic, a combination it said it had not observed before. Siemens, Elastic, Red Hat, Keycloak, Atlassian and Jenkins published impact guidance as fixes continue to roll out, SecurityWeek reported.

Sources: BleepingComputer, CyberScoop, SOC Prime, Cisco, FortiGuard Labs, Cloudflare Blog, SecurityWeek

**Corrections.** Report an error to corrections@thefivenines.com. We list substantive corrections on the corrections page.
