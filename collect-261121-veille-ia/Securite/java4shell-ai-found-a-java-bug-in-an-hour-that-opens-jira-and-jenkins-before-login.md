---
id: collect-261121-veille-ia/securite/java4shell-ai-found-a-java-bug-in-an-hour-that-opens-jira-and-jenkins-before-login
title: "Java4Shell: AI Found a Java Bug in an Hour That Opens Jira and Jenkins Before Login"
domain: securite
role: reference
task: article
actors: []
dates: []
keywords: ["java", "java4shell", "calif", "november", "oracle", "before", "login", "jenkins"]
source: https://mean.ceo/tpost/java4shell
source_anchor: ""
source_lines: [1, 120]
section: "Securite"
---
**TL;DR: What is Java4Shell?****Java4Shell** (CVE-2026-114938) is a memory-corruption bug in the Base64 decoder of **OpenJDK**, the code under almost every Java runtime. Security firm Calif disclosed it on **17 November 2026**, the same day Oracle shipped fixed builds. A short malformed Base64 string makes Java write 62 bytes past a memory buffer, and Calif showed it can lead to **takeover before login** on default installs of Keycloak, Jira, Confluence, Artifactory, Jenkins and PingFederate. Only **Java 17.0.6 and newer on Intel and AMD servers with AVX-512 VBMI** run the vulnerable code; ARM is not affected. Calif found it with OpenAI's Codex in about an hour. Patch, restart, and check the Java that ships inside your products.
                                

                                
                                **Developing story.** Last updated 19 Nov 2026, 10:30 CET. We'll update this article as Oracle, OpenJDK and software vendors publish more.**Update 19 Nov, 10:30 CET:** Probing has started: GreyNoise counted 187 Java4Shell-shaped requests from 5 IP addresses, with no successful intrusion reported, and the bug is still off CISA's exploited list. Red Hat, Ubuntu, Microsoft, Azul and Eclipse Temurin (Linux) have shipped fixed builds, while Debian stable, Temurin for Windows and macOS, the official Temurin Docker images and Oracle GraalVM were still waiting. Java 8 and 11 are confirmed not affected. Jenkins and Keycloak published fixes, and Atlassian says its Cloud customers need to do nothing.
                                

**Java4Shell**is the security story every company running Java woke up to on 18 November. Calif published its write-up on 17 November under the title

*"Write once, RCE everywhere"*, a play on Java's old slogan, and national cyber agencies in the US, Canada, Switzerland and the EU told organisations to patch straight away.

Here is what we know: what the bug does, which machines and Java versions are exposed, which fixed builds exist, what the exploitation status is, and the order in which a startup should patch. Small business owners without a tech team can start with our plain-language check for small businesses.

## What happened with Java4Shell on 17 November 2026?

Java turns Base64 text, the format used for login headers, tokens, cookies and certificates, back into bytes millions of times a second. Since OpenJDK 17.0.6, it does this on newer Intel and AMD processors with a hand-written fast path in assembly that uses the AVX-512 VBMI instruction set. According to Calif's disclosure, that fast path miscalculates the output length for short inputs that end in an = padding sign. Feed it 64 letters "A" followed by "=" and it writes 64 bytes into a 50-byte buffer, 62 bytes past the end.

That overflow does three things an attacker wants. It leaks heap memory, which Calif compares to Heartbleed and nicknames "JavaBleed". It crashes the Java process. And with enough work on each target, it gives 

**remote code execution without a password**. Oracle's own CVE record describes the impact as takeover of the product when data reaches the affected APIs, for instance through a web service.
Calif's researcher Quang Luong found the bug. Oracle fixed it in its November 2026 security patch update, released at 13:00 Pacific time on 17 November, and the CVE record went live at 21:32 UTC. Calif says details leaked before the fix, which forced the timing.

## Which Java versions and servers are affected by Java4Shell?

The bug needs a specific mix of software and hardware. Red Hat's bulletin lists four conditions, and all of them must hold:

1. **OpenJDK 17.0.6 or later** , including Java 21, 25 and 27, before the November fix. This covers Oracle Java, Amazon Corretto, Eclipse Temurin, Microsoft's build, Azul Zulu, Liberica, SapMachine and Red Hat's builds, which all share the same code.
2. **An x86-64 CPU with AVX-512 VBMI:** Intel Xeon Ice Lake, Sapphire Rapids, Emerald Rapids and Granite Rapids, or AMD Zen 4 and newer (EPYC Genoa and Turin, Ryzen 7000 and 9000). VulnCheck adds 2019-2021 Intel laptop and desktop chips (Ice Lake, Tiger Lake, Rocket Lake).
3. **No -XX:UseAVX=2 or lower** in the Java start-up flags.
4. **Attacker-controlled Base64 reaches Java's standard decoder,** through HTTP Basic logins, SAML messages, JWTs, CSRF tokens, cookies or MIME headers.

**Not affected:**ARM servers such as AWS Graviton and Ampere, older x86 chips such as Intel Cascade Lake and AMD Zen 3, and OpenJDK 17.0.0 to 17.0.5. OpenJDK 8 and 11 are not affected: Red Hat, Eclipse Temurin, Ubuntu and Azul all say so, and Oracle's advisory names only the Java 8 Enterprise Performance Pack build. Their November updates fix other bugs, so install them anyway. The cloud matters here: Upwind reproduced the bug on an AWS c6i instance, and Red Hat lists AWS C6i, M6i, C7i and C7a, Google Cloud C3, C4 and N4, and Azure D-series v5 to v7 as machines to check first.

Scores differ, which tells you how hard the attack is to pull off:

Source: Oracle's CVE record, Calif and Red Hat, 17-18 November 2026. The NVD record shows only Oracle's 8.1 so far.

## Which apps did researchers break with Java4Shell?

Calif tested default configurations and says its list is incomplete. Each app needs its own exploit chain, so there is no single universal attack:

That list reads like the back office of a typical software startup: logins, tickets, wiki, builds and artifacts. None of those vendors had published a Java4Shell statement when we checked on 18 November. Atlassian's security bulletin of 17 November fixes other bugs and does not mention CVE-2026-114938, so don't treat those Jira and Confluence versions as a Java4Shell fix.

## Is Java4Shell being exploited?

Attackers are probing. GreyNoise's tracker, updated at 09:19 UTC on 19 November, counted 187 Java4Shell-shaped requests from 5 IP addresses and 0 successful intrusions, and SecurityWeek reports scanning seen by watchTowr, Cloudflare and Previdian. No successful attack has been confirmed, and the bug was still missing from CISA's Known Exploited Vulnerabilities list on 18 November. The background:

- **Calif published only a bounded test** that proves a JDK is vulnerable without harming it, and is holding back its full exploit chains for later.
- **VulnCheck built a working exploit** and gave it to its paying customers at 23:00 UTC on 17 November. It still expects the bug to be hard to exploit widely, because remote code execution needs heap grooming and an address-layout bypass per target. A crash is trivial.
- **watchTowr** rates exploitation in the wild as "highly likely", and Cisco Talos expects probing soon after disclosure.
- **CISA** issued an alert urging immediate patching on 17 November, and CERT-EU followed for EU institutions at 09:11 UTC on 18 November.

## How did AI find Java4Shell?

This is the part founders should sit with. Calif says Quang Luong pointed OpenAI's 

**Codex**at the JIT compiler, intrinsics, widely used APIs and cryptography, ignored the noise, and kept prompting for about 20 minutes. In Calif's words,*"It took our own Quang Luong an hour to discover the vulnerability, and two more hours to develop the first working exploit."*The total token bill was under $200.
The New York Times, as relayed by Anadolu Agency, reported the model as OpenAI's "GPT-6 Cyber"; Calif's own page names only Codex. Either way, a bug in one of the most studied codebases on earth took an afternoon and less than a dinner bill to find and weaponise. Expect more of these. We covered the same pattern from the attacker's side with the ARTEX AI hacking tool, and this week's GPT-6.2 reviews show how fast these models change.

## What should founders do about Java4Shell today?

1. **Patch internet-facing identity providers first** (Keycloak, PingFederate, any SSO in front of your apps).
2. **Then internet-facing Jira, Confluence, Jenkins and Artifactory** , then any other Java service that handles logins, tokens or uploads, then internal Kafka, Solr, Elasticsearch, Gerrit and Spinnaker.
3. **Move to fixed builds:** Oracle Java 17.0.21.1, 21.0.13.1, 25.0.5.1 or 27.0.1.1 (8u513-perf for the Java 8 Performance Pack); Amazon Corretto 17.0.21.1.2, 21.0.13.1.2, 25.0.5.1.2 or 27.0.1.1.4; Microsoft's build 17.0.21.1, 21.0.13.1 or 25.0.5.1; Azul Zulu and Eclipse Temurin 17.0.21.1+2, 21.0.13.1+2, 25.0.5.1+2 or 27.0.1.1+4. Red Hat (RHEL 8, 9 and 10) and Ubuntu (all supported releases) shipped packages on 18 November. Still waiting on 19 November: Debian stable, Temurin for Windows and macOS, the official eclipse-temurin Docker images and Oracle GraalVM.
4. **Restart every JVM and rebuild every container image.** A patched package on disk does nothing for a process that is still running.
5. **Find the Java you didn't install.** Many products bundle their own runtime, which operating system updates never touch. Use the vendor's update path for those.
6. **Check your CPUs:** on Linux, grep -o avx512vbmi /proc/cpuinfo | head -1 returns a line on affected hardware, and java -version shows the runtime.
7. **Can't patch today?** Calif, Red Hat, Eclipse Temurin, Jenkins, Keycloak and Elastic now recommend the start-up flag -XX:-UseBASE64Intrinsics, without a prefix, which turns off the fast path. Oracle and OpenJDK still don't list it as a workaround, and you should remove it after patching. -XX:UseAVX=2 also works on x86, and stops Java from starting on ARM.
8. **Watch for signs of attack:** unexpected JVM crashes, hs_err_pid files, bursts of Base64 decoding errors, and Java processes starting shells or new outbound connections.

Freelancers and agencies who run these tools for clients have a 24-hour Java4Shell patch plan, and engineering teams can read what Java4Shell means for build servers, artifact stores and design data. Last month's PHP vulnerability played out the same way, with leaked details and a scramble to patch, and our November cybersecurity news roundup tracks the rest of the month.

## What we know about Java4Shell so far

- **17 Nov:** Calif discloses CVE-2026-114938 and Oracle ships fixed Java builds in its November patch update.
- **17 Nov:** CISA and the Canadian Centre for Cyber Security tell organisations to patch immediately. Amazon ships Corretto 17.0.21.1.2 and 27.0.1.1.4.
- **17 Nov, 23:00 UTC:** VulnCheck has a working exploit for paying customers. Calif's public test stays bounded.
- **18 Nov:** CERT-EU, the Dutch NCSC and the Swiss NCSC publish advisories. Red Hat, Ubuntu and Microsoft ship fixed builds, Eclipse Temurin starts with Linux, and Jenkins tells admins to update the controller's Java and restart. Atlassian says Cloud customers need no action and gives Data Center admins the -XX:UseAVX=2 mitigation until patched products ship.
- **19 Nov:** Keycloak marks its 26.8.2-3 image and later as fixed. GreyNoise counts 187 probing requests and no successful intrusion.

## What we don't know about Java4Shell yet

- **The last fixed builds:** Debian stable, Temurin for Windows and macOS, the eclipse-temurin Docker images and Oracle GraalVM.
- **Patched products from Atlassian (Data Center), JFrog Artifactory, Elastic and Ping,** and a fix for Siemens Industrial Edge Management, which Siemens says has none yet and should stay off the internet.
- **When Calif publishes its full exploit chains,** and whether any of the probing turns into a successful attack.
- **Who leaked the details** before the fix.

- Java vulnerability Java4Shell: is your small business affected, and who has to patch what? (Fe/male Switch Foundation)
- Java4Shell patch plan: what freelancers and small agencies should fix on client Java servers in 24 hours (Fe/male Switch)
- Java4Shell engineering guide: what CAD and PLM teams should patch first and ask their vendors (CADChain)

## FAQ: Java4Shell

### What is Java4Shell?

Java4Shell is CVE-2026-114938, a heap overflow in OpenJDK's Base64 decoder disclosed by Calif on 17 November 2026. A short malformed Base64 string can leak memory, crash Java or lead to remote code execution on affected servers.

### Am I affected by Java4Shell?

Only if you run OpenJDK 17.0.6 or newer, including Java 21, 25 and 27, before the November 2026 fix, on an Intel or AMD CPU with AVX-512 VBMI, and your app decodes Base64 from users. ARM servers are not affected.

### Is there a patch for Java4Shell?

Yes. Oracle shipped Java 17.0.21.1, 21.0.13.1, 25.0.5.1 and 27.0.1.1 on 17 November 2026, and Amazon, Azul, Microsoft, Red Hat, Ubuntu and Eclipse Temurin (Linux first) followed by 18 November. Debian stable, Temurin for Windows and macOS and the Temurin Docker images were still waiting on 19 November. Restart Java after patching.

### Which apps are vulnerable to Java4Shell?

Calif showed takeover before login on default Keycloak, Jira, Confluence, JFrog Artifactory, Jenkins and PingFederate, and after login on Kafka, Solr, Elasticsearch, Gerrit and Spinnaker. Its list is incomplete.

### Is Java4Shell being exploited?

Attackers started probing by 19 November 2026: GreyNoise counted 187 requests from 5 IP addresses and no successful intrusion. It is not on CISA's Known Exploited Vulnerabilities list yet. VulnCheck has a private working exploit and Calif plans to publish its chains later.

### Was Java4Shell found by AI?

Yes. Calif says its researcher found it with OpenAI's Codex in about an hour, built the first exploit in two more hours, and spent under $200 on tokens. The New York Times reported the model as GPT-6 Cyber.

## About the Author

Violetta Bonenkamp, also known as MeanCEO, is an experienced startup founder with an impressive educational background including an MBA and four other higher education degrees. She has over 20 years of work experience across multiple countries, including 5 years as a solopreneur and serial entrepreneur. Throughout her startup experience she has applied for multiple startup grants at the EU level, in the Netherlands and Malta, and her startups received quite a few of those. She’s been living, studying and working in many countries around the globe and her extensive multicultural experience has influenced her immensely.
