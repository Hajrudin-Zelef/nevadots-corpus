---
id: collect-261121-veille-ia/securite/java4shell-malformed-base64-can-let-attackers-run-code-in-java-17-21-25-and-27-apps-on-newer-x86-64-
title: "Java4Shell: malformed Base64 can let attackers run code in Java 17, 21, 25 and 27 apps on newer x86-64 servers; shown before login on Jira, Confluence, Jenkins and Keycloak, exploit attempts seen (CVE-2026-114938)Patch available"
domain: securite
role: reference
task: article
actors: []
dates: []
keywords: ["java", "before", "keycloak", "exploit", "calif", "code", "login", "jenkins"]
source: https://infrastack.ai/live/TECH-202611-nqwi4f
source_anchor: ""
source_lines: [1, 172]
section: "Securite"
---
Java4Shell: malformed Base64 can let attackers run code in Java 17, 21, 25 and 27 apps on newer x86-64 servers; shown before login on Jira, Confluence, Jenkins and Keycloak, exploit attempts seen (CVE-2026-114938)Patch available

Java4Shell (CVE-2026-114938) is a memory-corruption bug in the hand-written AVX-512 code that HotSpot uses to speed up java.util.Base64 decoding on x86-64: a malformed Base64 value makes the decoder write past the end of its output buffer before Java rejects the input (Calif; OpenJDK files it under hotspot/compiler). It affects Java 17 from 17.0.6, 21, 25 and 27 in every OpenJDK-based build before the 17 Nov 2026 releases (17.0.21.1, 21.0.13.1, 25.0.5.1, 27.0.1.1), and only on CPUs with AVX-512 VBMI (Intel Ice Lake or newer, AMD Zen 4 or newer). ARM64 and standard Java 8 and 11 are not affected, and Quarkus notes that IBM Semeru (OpenJ9, not HotSpot) is not either. Oracle rates it 8.1 (CVSS 3.1, difficult to exploit), Red Hat Critical (9.8), Calif 9.2 (CVSS 4.0). Calif demonstrated remote code execution before login on default installs of Keycloak, Jira, Confluence, JFrog Artifactory, Jenkins and PingFederate, and after login on Kafka, Solr, Elasticsearch, Gerrit and Spinnaker; each target needs its own exploit chain and Calif is holding those back. Calif also reports that a Java client opening a TLS connection to a malicious server can be attacked. Crashing a JVM is far easier (VulnCheck: a burst of requests reliably took a service down in its tests), and Red Hat says the bug can expose heap memory holding credentials, keys and session tokens. Public code only triggers the bug (Calif's trigger PoC); VulnCheck offers a full exploit to its customers and Immersive's Kevin Breen says he built one. Akamai's patch analysis (19 Nov) reports no gap in the upstream fix and no exploitation. The bug was public months before the fix, per the disclosure timeline Calif added on 19 Nov: a user hit it and reported it in an Adoptium support issue on 9 Jun 2026; a reproducer and the root cause followed on 8 Sep, and Albert Yang proposed a fix in an OpenJDK pull request on 9 Sep that was never merged (closed 4 Nov, as Will Dormann pointed out). Calif's Quang Luong found it independently and reported it to Oracle on 24 Oct with a post-login RCE in Elasticsearch, adding pre-login RCE in Jenkins and Keycloak on 31 Oct; Oracle shipped the fix on 17 Nov, crediting both. Exploit attempts are under way: GreyNoise has logged 261 trigger-shaped Basic or Bearer Authorization headers from 7 IPs since 17 Nov, aimed at Confluence, Spring Boot actuator, login and Jenkins paths (/crumbIssuer/api/json, /whoAmI/api/json, /cli), with no successful intrusion, and tracks a scanner with the user agent java4shell-scanner/1.0 from 4 addresses; watchTowr, Cloudflare and Previdian report blind attempts (SecurityWeek). Cisco PSIRT knows of no malicious use, and the CVE was not in CISA KEV as of 19 Nov. Status at 21 Nov 01:50 UTC: Red Hat has shipped the RHEL 8, 9 and 10 packages and fixed UBI OpenJDK runtime images (19 Nov), but its Keycloak, Hardened Images and JBoss EAP OpenJDK 17 images are still listed affected; AWS is fixing its managed services in place (Glue, EMR Serverless, Aurora and RDS infrastructure, Bedrock components and others) and finished phase 1 of the Lambda runtimes; Azure App Service has updated its built-in runtimes on Linux and Windows (Windows apps need a restart); Docker Hardened Images for Corretto, SapMachine and Zulu are rebuilt; Temurin binaries are out for 17 and 25 on every primary platform and for 21 and 27 on most, but Adoptium's tracker at 00:00 UTC on 21 Nov still showed no Docker image for any line; Cisco's advisory (v1.2, 19 Nov) adds ten affected products and clears IOS XE and UCS Central, with every fix still TBD; Atlassian, Elastic and Siemens have no fixed releases; Keycloak ships fixed images and Ping Identity has republished images with the workaround set; Cloudflare's Free-plan rule is still pending. ComputingForGeeks and Docker both note that Elasticsearch ignores JAVA_TOOL_OPTIONS, so set the flag there the way Elastic documents.

Java4Shell (CVE-2026-114938) is a memory-corruption bug in the hand-written AVX-512 code that HotSpot uses to speed up java.util.Base64 decoding on x86-64: a malformed Base64 value makes the decoder write past the end of its output buffer before Java rejects the input (Calif; OpenJDK files it under hotspot/compiler).

It affects Java 17 from 17.0.6, 21, 25 and 27 in every OpenJDK-based build before the 17 Nov 2026 releases (17.0.21.1, 21.0.13.1, 25.0.5.1, 27.0.1.1), and only on CPUs with AVX-512 VBMI (Intel Ice Lake or newer, AMD Zen 4 or newer).

ARM64 and standard Java 8 and 11 are not affected, and Quarkus notes that IBM Semeru (OpenJ9, not HotSpot) is not either. Oracle rates it 8.1 (CVSS 3.1, difficult to exploit), Red Hat Critical (9.8), Calif 9.2 (CVSS 4.0).

Calif demonstrated remote code execution before login on default installs of Keycloak, Jira, Confluence, JFrog Artifactory, Jenkins and PingFederate, and after login on Kafka, Solr, Elasticsearch, Gerrit and Spinnaker; each target needs its own exploit chain and Calif is holding those back.

Calif also reports that a Java client opening a TLS connection to a malicious server can be attacked. Crashing a JVM is far easier (VulnCheck: a burst of requests reliably took a service down in its tests), and Red Hat says the bug can expose heap memory holding credentials, keys and session tokens.

Public code only triggers the bug (Calif's trigger PoC); VulnCheck offers a full exploit to its customers and Immersive's Kevin Breen says he built one. Akamai's patch analysis (19 Nov) reports no gap in the upstream fix and no exploitation.

The bug was public months before the fix, per the disclosure timeline Calif added on 19 Nov: a user hit it and reported it in an Adoptium support issue on 9 Jun 2026; a reproducer and the root cause followed on 8 Sep, and Albert Yang proposed a fix in an OpenJDK pull request on 9 Sep that was never merged (closed 4 Nov, as Will Dormann pointed out).

Calif's Quang Luong found it independently and reported it to Oracle on 24 Oct with a post-login RCE in Elasticsearch, adding pre-login RCE in Jenkins and Keycloak on 31 Oct; Oracle shipped the fix on 17 Nov, crediting both.

What to do

Update the Java runtime, rebuild container images and restart every JVM; a package update does not touch runtimes bundled in products or images, or a JVM that is already running. Fixed images often keep the same tag: deploy by digest, build with docker build --pull so a cached base image is not reused, and note that Kubernetes nodes with imagePullPolicy: IfNotPresent keep the old image (Docker, Quarkus).

Fixed in27.0.1.125.0.5.121.0.13.117.0.21.1+35 more lines

01Oracle JDK and OpenJDK: 17.0.21.1, 21.0.13.1, 25.0.5.1, 27.0.1.1 (Oracle builds +4). Oracle Java SE 8 Enterprise Performance Pack: 8u513-perf. Standard Java 8 and 11 need nothing for this CVE.

02

How to check

Are you affected?

17 INDICATOR VALUES

17.0.21.0.3, 21 up to 21.0.13.0.3, 25 up to 25.0.5.0.3 and 27 up to 27.0.1.0.3 (OpenJDK, Oracle); a fixed build shows the trailing .1 (17.0.21.1, 21.0.13.1, 25.0.5.1, 27.0.1.1). The 17 Nov release has no build for 18-20, 22-24 or 26, and Apache Kafka lists every release of those lines as affected with no fix. Standard Java 8 and 11 are not affected; Oracle Java SE 8 Enterprise Performance Pack 8u511b33-perf is.

Does this hit you?

Check your public exposure to OpenJDK.

Start with your app. Your first report is free.

Your stack

Ask your security agent. It answers from what it knows about your app: your versions, hosts and vendors.

Revisit 07:57 on 21 Nov: no change since 01:52. Temurin release tracker last updated 21 Nov 00:00 UTC (still no Docker images for any line); GreyNoise unchanged (261 trigger-shaped requests, 7 IPs, 0 intrusions, java4shell-scanner/1.0 cluster of 4); no Atlassian or Elastic fixed releases, no KEV listing and no new vendor statements found. No fields rewritten; verified badges kept.

13 · Nov 21

Revisit 01:52 on 21 Nov. AWS updated its bulletin at 20 Nov 20:00 UTC with per-service status: Glue 5.0/5.1/6.0 and EMR Serverless 7.x/8.x x86 labels fixed in place (pre-initialized capacity needs a restart), AWS-managed parts of Aurora, RDS, Bedrock, Lake Formation, ElastiCache, DynamoDB, DMS and Neptune being updated, Redshift and AWS-provided Athena connectors not affected; added to fix and how-to-check. Azure App Service finished the Windows rollout (20 Nov 15:00 PT; Windows apps on auto-update need a manual restart). GreyNoise: 261 trigger-shaped requests from 7 IPs, 0 intrusions, plus a java4shell-scanner/1.0 user-agent scanner from 4 addresses. Temurin tracker 00:00 UTC: 17 and 25 complete on primary platforms, still no Docker images. Unchanged: Atlassian, Elastic, Cisco, KEV. Products and who-is-affected untouched.

12 · Nov 20

For your coding agent

Let Claude Code, Codex or Cursor check your repository.

It reads this threat and your lock files, read-only, and says whether you are affected, with the file and line.

PROMPT

Use the InfraStack skill; install it first if you do not have it: npx skills add infrastackai/skills
Then check Java4Shell (TECH-202611-nqwi4f) against our stack, starting from this repository: https://infrastack.ai/live/TECH-202611-nqwi4f

Evidence and sources

Where every fact on this page comes from.

Exploitation

Reported exploited in the wild by the sources below; not on CISA's KEV list yet.

Checked by an analyst

An InfraStack analyst checked the fix, the affected versions, how to check and who is affected against the vendor's own text.

Exploit attempts are under way: GreyNoise has logged 261 trigger-shaped Basic or Bearer Authorization headers from 7 IPs since 17 Nov, aimed at Confluence, Spring Boot actuator, login and Jenkins paths (/crumbIssuer/api/json, /whoAmI/api/json, /cli), with no successful intrusion, and tracks a scanner with the user agent java4shell-scanner/1.0 from 4 addresses; watchTowr, Cloudflare and Previdian report blind attempts (SecurityWeek).

Cisco PSIRT knows of no malicious use, and the CVE was not in CISA KEV as of 19 Nov.

Status at 21 Nov 01:50 UTC: Red Hat has shipped the RHEL 8, 9 and 10 packages and fixed UBI OpenJDK runtime images (19 Nov), but its Keycloak, Hardened Images and JBoss EAP OpenJDK 17 images are still listed affected; AWS is fixing its managed services in place (Glue, EMR Serverless, Aurora and RDS infrastructure, Bedrock components and others) and finished phase 1 of the Lambda runtimes; Azure App Service has updated its built-in runtimes on Linux and Windows (Windows apps need a restart); Docker Hardened Images for Corretto, SapMachine and Zulu are rebuilt; Temurin binaries are out for 17 and 25 on every primary platform and for 21 and 27 on most, but Adoptium's tracker at 00:00 UTC on 21 Nov still showed no Docker image for any line; Cisco's advisory (v1.2, 19 Nov) adds ten affected products and clears IOS XE and UCS Central, with every fix still TBD; Atlassian, Elastic and Siemens have no fixed releases; Keycloak ships fixed images and Ping Identity has republished images with the workaround set; Cloudflare's Free-plan rule is still pending.

ComputingForGeeks and Docker both note that Elasticsearch ignores JAVA_TOOL_OPTIONS, so set the flag there the way Elastic documents.

Who is affected

A Java service is exposed when it runs an unpatched OpenJDK-based runtime from 17.0.6 on (Java 17, 21, 25 or 27 from any vendor, including distro packages and the copy bundled inside a product or container image) on an x86-64 CPU with AVX-512 VBMI, such as AWS C6i, M6i, R6i, C7i, M7i, R7i, C7a, M7a or R7a, and decodes Base64 an outsider can send: HTTP Basic credentials, SAML artifacts, CSRF tokens and MIME encoded words are often decoded before login, so a login page is no protection (Red Hat). Look first at internet-facing Keycloak, Jira, Confluence, Artifactory, Jenkins and PingFederate, where Calif ran code before login, then Kafka, Solr, Elasticsearch, Gerrit and Spinnaker (login needed) and Java clients that connect to untrusted TLS servers. You can skip hosts on ARM64 (AWS Graviton, Fargate ARM64, the default AWS Lambda environment) or on x86 CPUs without AVX-512 VBMI, standard Java 8 and 11, and JVMs that run with -XX:-UseBASE64Intrinsics or -XX:UseAVX=2.

03Eclipse Temurin: 17.0.21.1+2, 21.0.13.1+2, 25.0.5.1+2, 27.0.1.1+4. Binaries are out for 17 and 25 on every primary platform and for 21 and 27 on most (aarch64 Linux for 21 and 27 and Windows x64 for 27 still in testing), but as of 21 Nov 00:00 UTC no Docker Hub image had been published for any line (Adoptium release status): rebuilding on eclipse-temurin images still gets the old build, so keep the flag until the images appear.

05Microsoft Build of OpenJDK: 17.0.21.1, 21.0.13.1, 25.0.5.1.

06Azul: the 17 Nov Azul Core (Zulu) CSPU builds (Docker reports Zulu 17.0.21.1+2, 21.0.13.1+2, 25.0.5.1+2 and 27.0.1.1+4 in its hardened images); Azul Prime 26.08.110.0 or 26.02.510.0.

07Red Hat (19 Nov): RHEL 8 java-17-openjdk 1:17.0.21.1.2-2.1.el8 (RHSA-2026:100563) and java-21-openjdk 1:21.0.13.1.2-1.1.el8 (RHSA-2026:100567); RHEL 9 java-17-openjdk 1:17.0.21.1.2-2.2.el9 (RHSA-2026:100564), java-21-openjdk 1:21.0.13.1.2-1.2.el9 (RHSA-2026:100567) and java-25-openjdk 1:25.0.5.1.2-1.1.el9 (RHSA-2026:100571); RHEL 10 java-21-openjdk 1:21.0.13.1.2-1.2.el10 (RHSA-2026:100568) and java-25-openjdk 1:25.0.5.1.2-1.1.el10 (RHSA-2026:100572); the same advisories cover the EUS, AUS, TUS and E4S streams. Portable 17.0.21.1, 21.0.13.1 and 25.0.5.1 and Windows 17.0.21.1 and 21.0.13.1 shipped on 18 Nov (RHSA-2026:100565, 100566, 100569, 100570, 100573). UBI OpenJDK runtime images were fixed on 19 Nov: ubi8/openjdk-17-runtime and openjdk-21-runtime 1.23-7, ubi9/openjdk-17-runtime 1.24-6, ubi9/openjdk-21-runtime and openjdk-25-runtime 1.24-5, ubi10/openjdk-21-runtime and openjdk-25-runtime 1.24-21; the UBI 9 1.23 stream will not be fixed, so move to 1.24 (Quarkus). The Red Hat build of Keycloak image, Red Hat Hardened Images and the JBoss EAP 7 and 8 OpenJDK 17 images are still listed Affected: use the flag.

08Ubuntu: openjdk-17 17.0.21.1+2-1~<release>.1, openjdk-21 21.0.13.1+2-1~<release>.1, openjdk-25 25.0.5.1+2-1~<release>.1, openjdk-27 27.0.1.1+4-1~26.10.1 (20.04 and 18.04 through Ubuntu Pro); no fix yet for the -crac packages.

Holds only for traffic through them. Patching is still the fix.

AWSBlocked at their edgeHTTP requests containing malformed Base64 text to AWS WAF web ACLs pinned to the KnownBadInputs Managed Rule Group version 1.28, via its new Java4Shell rules. source

Can’t patch yet? · 7 workarounds

Java Lambda functions (JAVA_TOOL_OPTIONS) and Lambda Managed Instances: Add -XX:-UseBASE64Intrinsics to the JAVA_TOOL_OPTIONS environment variable in the function configuration source

Amazon Corretto and OpenJDK on affected x86-64 instances: Set the JVM option -XX:-UseBASE64Intrinsics until the JDK can be updated source

Amazon Corretto or OpenJDK on affected x86-64 instances: Set the JVM option -XX:-UseBASE64Intrinsics until the JDK can be updated source

AWS Lambda Event Functions on Lambda Managed Instances: Add -XX:-UseBASE64Intrinsics to the JAVA_TOOL_OPTIONS environment variable in the function configuration; a zero-change update is not sufficient for the runtime fix source

Amazon Corretto and OpenJDK x86-64 Java workloads: Set the JVM option -XX:-UseBASE64Intrinsics until the updated JDK is installed and the application restarted source

OpenJDK / Oracle Java JVM (HotSpot) on AVX-512 x86-64 hosts: Add -XX:-UseBASE64Intrinsics or -XX:UseAVX=2 (or UseAVX=1 or 0) to the JVM command line until the patched runtime is installed source

17.0.21.1.2-2.1.el8, 21.0.13.1.2-1.1.el8; RHEL 9: 17.0.21.1.2-2.2.el9, 21.0.13.1.2-1.2.el9, 25.0.5.1.2-1.1.el9; RHEL 10: 21.0.13.1.2-1.2.el10, 25.0.5.1.2-1.1.el10) (Red Hat). Ubuntu openjdk-17-crac, -21-crac and -25-crac are still vulnerable (Ubuntu). Amazon Linux repository metadata does not list the CVE yet, so update without waiting for a scanner (AWS).

21.3.20.1 as 17.0.20.1+4 (October 21.3.20 used 17.0.21+9), so confirm with java -version that you are on the November build.

Report in about a minute

SOC 2 Type II

Revisit 19:34 on 20 Nov. New: the Quarkus status page (updated 20 Nov 08:00, Red Hat's Quarkus team) lists the UBI 8/9/10 OpenJDK runtime images fixed on 19 Nov with build tags, says the UBI 9 1.23 stream (Quarkus 3.27 default) will not be fixed, that Mandrel native executables and IBM Semeru are not affected, and that the Quarkus builder images carry the flag. Red Hat's security data agrees: no UBI image is listed affected any more, while the Red Hat build of Keycloak, Hardened Images and JBoss EAP 7/8 OpenJDK 17 images still are (JWS no longer listed). Updated fix and how-to-check and linked Quarkus as fix. Temurin tracker 17:39 UTC: 25 complete on primary platforms, still no Docker images. XBOW's new post is only an explainer link. Unchanged: GreyNoise (197 requests, 7 IPs, 0 intrusions), Atlassian (no patches), Elastic (no release), Azure page. Products and who-is-affected untouched.

11 · Nov 20

Revisit 15:26 on 20 Nov. Only Temurin moved: the release tracker (14:12 UTC) now shows Windows x64 builds for 17, 21 and 25 and macOS x64 and aarch64 Linux for 25, but still no Docker images for any line (aarch64 Linux for 17, 21 and 27 is still in testing). Updated the Temurin bullet in fix and the summary status line. Unchanged: GreyNoise (197 requests, 7 IPs, 0 intrusions, no new source since 19 Nov), Azure App Service page (18 Nov), no Elastic or Atlassian fixed releases found (the Atlassian page did not render through WebFetch), not in KEV. How-to-check, products and who-is-affected untouched.

10 · Nov 20

Revisit 09:20 on 20 Nov. Cisco advisory v1.2 (19 Nov 20:22 GMT): IOS XE and UCS Central are now not affected; ten more products are affected (Crosswork Assurance UX and Workflow Manager, AppDynamics On-Prem Enterprise Console, NextFusion, Ultra Cloud Core NRF, Finesse, UCCE, Unified CVP, Unified Intelligence Center, Virtualized Voice Browser); planned fixed releases are named but all TBD. Updated how-to-check and fix. Docker's 19 Nov post: its hardened Corretto, SapMachine and Zulu images are rebuilt, the Temurin ones are not; also cautions to deploy by digest and to keep the flag out of JAVA_TOOL_OPTIONS on Java 8 JVMs, which will not start with it. Added to fix and linked as mitigation. Temurin tracker 08:56 UTC: still no Docker images. GreyNoise unchanged (197 requests, 7 IPs, 0 intrusions); Azure App Service page unchanged; no Atlassian or Elastic fixes. Products and who-is-affected unchanged.

09 · Nov 20

Revisit 03:15 on 20 Nov. New: Calif's post of 19 Nov 23:13 (@calif_io, 34.6K followers) points to a disclosure timeline added to its write-up: user report in an Adoptium support issue on 9 Jun, reproducer and root cause on 8 Sep, unmerged OpenJDK fix PR on 9 Sep, Calif's independent report to Oracle on 24 Oct, fix on 17 Nov. Summary timeline updated; I read only the timeline section, not the exploitation part of the page. No change at Temurin (tracker 00:38 UTC, still no Docker images), GreyNoise (197 requests, 7 IPs, 0 intrusions) or the Azure App Service page. Fix, how-to-check and products unchanged and still verified.

08 · Nov 19

Revisit 23:42. AWS updated its bulletin at 19 Nov 22:00 UTC: Lambda managed Java runtimes finished phase 1 (phase 2 by 22 Nov) and AWS names the INIT_START runtime versions that confirm the patch; added to fix and how-to-check. The extractor's 'AWS platform-fix since 2026-11-22' mitigation misstates this: phase 1 is already done, and functions still need a code or configuration update. Akamai's 19 Nov patch analysis (already linked as advisory) reports no gap in the fix and no exploitation; its Guardicore queries test only avx512f and bmi2 (confirmed on the page), so their POTENTIALLY VULNERABLE verdict over-reports; caveat added to how-to-check. Temurin tracker at 23:07 UTC: still no Docker images (aarch64 Linux and Windows still in testing). GreyNoise: 197 requests from 7 IPs, 0 intrusions; a new address probes Jenkins endpoints. Azure App Service page unchanged since 18 Nov; no Atlassian, Elastic or Cisco fixes yet. Products and who-is-affected unchanged.

07 · Nov 19

Revisit 18:36: no material change since 15:10. Adoptium's Temurin tracker (17:56 UTC) still shows no Docker images for any line (only JDK 27 aarch64 Mac binaries added); GreyNoise unchanged at 192 trigger-shaped requests from 6 IPs, 0 intrusions; the CFchangelog post of 19 Nov 17:01 re-announces the six 18 Nov Log-mode rules, and the WAF changelog has no newer Java4Shell entry, so no Free-plan rule yet; no KEV listing and no new Atlassian, Elastic or Cisco fixes found. The two XBOW posts (pentest vendor) were not reviewed. Only the summary status line was updated; fix and how-to-check stay as verified at 15:10.

06 · Nov 19

Revisit 15:02 (now PRIORITY #1). New since 11:42: Red Hat shipped the RHEL 8/9/10 OpenJDK errata on 19 Nov (RHSA-2026:100563, 100564, 100567, 100568, 100571, 100572), added as rhel rows; Temurin release status at 14:25 UTC shows Linux x64 binaries out but no Docker images for any line; GreyNoise 192 trigger-shaped requests from 6 IPs, 0 intrusions; Apache Kafka guidance added; Oracle 27.0.1.1 release notes confirm the +4 builds; OpenJDK PR 32781 (Albert Yang, public 9 Sep, closed unmerged 4 Nov, via Will Dormann) put the bug in public view before disclosure. No change at Cisco, AWS, Atlassian or Elastic. Re-sent the four Ubuntu rows, which the Products line no longer showed. The collector linked an unrelated BleepingComputer Uber breach article and many third-party GitHub exploit/scanner repos; not opened or relied on. Still the same threat as CVE-2026-114938 (merge).

05 · Nov 19

Run 4757 was cut off before it wrote, so this visit rewrote every field from primary pages (OpenJDK advisory, Oracle CNA record, Oracle release notes and ops.java, Oracle GraalVM notes, Adoptium, AWS, Microsoft, Red Hat RHSB and security data, Ubuntu, Azul, Keycloak, Jenkins, Atlassian, Elastic, Cisco, Siemens ADP, Cloudflare, Akamai, Azure, Apigee, Ping) and verified them. The title no longer claims one string takes over a server. New since 06:34: Keycloak fixed images 26.8.2-3, Red Hat portable and Windows RHSAs (RHEL RPMs still unfixed), Ubuntu fixes, Cisco and Siemens affected lists, Akamai, Apigee and Ping actions, GreyNoise up from 117 to 187 trigger-shaped requests from 5 IPs. Exploited kept true for in-the-wild attempts (GreyNoise, watchTowr, Cloudflare, Previdian; no confirmed intrusion; not in KEV); sibling CVE-2026-114938 is the same threat and says false, so merge them. Added repo check java4shell-runtime-image (draft).

04 · Nov 19

Exploitation attempts in the wild since 18 Nov (SecurityWeek 19 Nov, THN): GreyNoise 150+ attack-shaped requests from 5 IPs, no successful intrusions seen; WatchTowr honeypots hit with 'java4shell-recon' User-Agents plus blind trigger requests; Cloudflare saw one IP probe then send the trigger ~17h later disguised as Chrome, and an IP running both ARTEX and Java4Shell; Previdian 9 attempts from 6 IPs in 4 countries. No compromise confirmed. Full RCE exploits now exist (VulnCheck for customers, Immersive). New vendor impact: Cisco 12+ products (Catalyst Center, Secure Network Analytics, Secure Workload, SSM On-Prem, AppDynamics On-Prem VA, Intersight VA) with no fix or workaround yet; Siemens Industrial Edge Management (take offline); Elasticsearch 7.17.3+ (RCE with read creds, unauthenticated DoS), Logstash 8.6.2+ crash; RHEL 8/9/10 Critical; Keycloak images 26.8.2-1/-2 vulnerable, fixed in 26.8.2-3; Atlassian Data Center: disable AVX-512 until patches.

03 · Nov 18

Cloudflare released six WAF managed rules for Java4Shell (paid plans, log-only by default)

02 · Nov 18

Blackpoint Cyber independently reproduced the bug: it crashed a Java server with about 28,300 malformed login requests in 20 seconds and showed memory disclosure in 30/30 local tests. Full RCE is still not independently verified. The Hacker News also says Azul has shipped patches (alongside Oracle and Amazon Corretto); Red Hat fixes were still pending. Source: The Hacker News on X (docs 182536, 182539).

01 · Nov 17

Calif published the write-up: Base64 AVX-512 VBMI decoder overflow with the trigger, pre-auth RCE shown in Keycloak, Jira, Confluence, Artifactory, Jenkins and PingFederate, plus a client-side TLS route. The OpenJDK 2026-11-17 advisory confirms the CVE and affected versions (17/21/25/27; not 8/11). Upwind reproduced it, gave the CPU requirement and the -XX:-UseBASE64Intrinsics workaround. Severity raised to critical; not exploited yet.
