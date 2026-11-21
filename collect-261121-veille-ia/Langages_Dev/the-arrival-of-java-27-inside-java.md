---
id: collect-261121-veille-ia/langages-dev/the-arrival-of-java-27-inside-java
title: "The Arrival of Java 27! - Inside.java"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-09-15"]
keywords: ["java", "developers", "applications", "helps", "security", "performance", "improve", "productivity"]
source: https://inside.java/2026/09/15/jdk-27-available/
source_anchor: ""
source_lines: [1, 113]
section: "Langages_Dev"
---
Oracle is proud to announce the general availability of JDK 27 for developers, enterprises, and end-users.

JDK 27 delivers nine enhancements that are significant enough to warrant their own JDK Enhancement Proposals (JEPs), including four preview features and one incubator feature. These features cover innovations to the Java language; security, performance and runtime advancements; and library improvements.

Enhance the security of Java applications that require secure network communication by implementing hybrid key exchange algorithms for TLS 1.3. Such algorithms defend against future quantum computing attacks by combining a quantum-resistant algorithm with a traditional algorithm. Applications that use the `javax.net.ssl` APIs will benefit from these improved algorithms by default, without change to existing code.

Helps developers improve productivity and enhance Java application security across widely used security formats via a new encoding API. The API encodes objects that represent cryptographic keys, certificates, and certificate revocation lists into the widely used, privacy-enhanced mail transport format, and decodes it back into objects. This reduces the risk of errors, simplifies compliance, and enhances the portability and interoperability of secure Java applications by streamlining cryptography setup and integration for enterprise, cloud, and regulatory needs.

Helps developers increase productivity and streamline the development of applications that integrate AI inferencing by making Java more uniform and expressive. It eliminates multiple restrictions pertaining to primitive types that impose friction when using pattern matching, `instanceof`, and `switch`. To help developers further improve productivity, it also enhances the definition of unconditional exactness and applies tighter dominance checks in switch constructs, enabling the compiler to identify and reduce a wider range of coding errors.

Make the Garbage-First (G1) garbage collector the default collector in all environments, rather than just server environments. When no garbage collector is specified on the command line:

The HotSpot JVM will always select G1.

In scenarios in which the JVM previously selected the Serial collector, the performance metrics of throughput, latency, memory footprint, and startup time should not degrade significantly.

Make compact object headers the default object header layout in the HotSpot JVM. Compact object headers reduce object headers from 96 bits down to 64 bits on 64-bit architectures, thereby reducing heap size, improving deployment density, and increasing data locality.

Helps developers increase productivity and resource efficiency by offering greater flexibility in the timing of their initialization, which is particularly valuable for AI and data-driven applications. Via a new API for lazy constants, which are objects that hold unmodifiable data, the JVM treats lazy constants as true constants to enable the same performance as declaring a field final. In addition, by enabling Java applications and their cloud native and AI-powered services to launch faster and use computing resources more efficiently, it helps developers execute agile and scalable deployments that can lead to cost savings and a better experience for end users.

Helps developers improve the maintainability, reliability, and observability of multithreaded code, which is especially beneficial for improving the scalability and resilience of AI and cloud native workloads. It simplifies concurrent programming via an API for structured concurrency, which treats groups of related tasks running in different threads as a single unit of work and helps reduce common risks arising from cancellation and shutdown, such as thread leaks and cancellation delays.

Helps developers improve the performance and cost efficiency of their Java applications by enabling them to deliver more insights and value with less hardware. The vector API expresses vector computations that reliably compile at runtime to optimal vector instructions on supported CPU architectures, which results in faster processing for data analytics, AI inference, and scientific computing workloads. This gives developers the ability to achieve performance superior to equivalent scalar computations often used in AI inference and compute scenarios.

Enhances JDK Flight Recorder (JFR) to redact command-line arguments and the initial values of environment variables and system properties in recordings. This data is redacted before it leaves the process, helping prevent sensitive information from leaking through diagnostic data.

Preview features are fully specified and fully implemented Language or VM Features of the Java SE Platform; and yet impermanent. They are made available in JDK Feature Releases to allow for developer feedback based on real-world uses, before becoming permanent in a future release. This also affords tool vendors the opportunity to work towards supporting features before they are finalized into the Java SE Standard. Experimental features are early-stage functionalities, primarily at the Virtual Machine (VM) level, that are still undergoing development and testing and are used to gather early feedback on potentially impactful enhancements to the HotSpot VM. APIs in Incubator modules put non-final APIs and non-final tools in the hands of developers and users so that we can gather feedback that will ultimately improve the quality of the Java platform.

In addition to the nine JEPs, Java 27 offers dozens of improvements that help organizations enhance application security, reliability, and performance. These updates offer adaptation to underlying OS and firmware updates and standards. Users and application developers usually benefit from these changes without noticing them; access the JDK 27 release notes for the full set of non-JEP enhancements.

Some JDK 27 enhancements worth mentioning include strengthening Java’s post-quantum cryptography foundation with updated ML-KEM and ML-DSA private key encodings; significant performance gains for ML-KEM, ML-DSA, X25519, and Ed25519 algorithms; and new runtime visibility into active security properties through the `jcmd VM.security_properties` command. Additional security and cryptography updates improve TLS efficiency, runtime visibility, and certificate and key management, helping organizations operate Java applications with greater security and confidence. Together, these updates help further advance Java’s security, cryptographic agility, and operational readiness as organizations prepare for future security requirements.

Application performance and efficiency are improved through a broad set of updates across the JVM, compiler, garbage collection, and I/O areas. Enhancements include G1 heap sizing updates, C2 and Vector API optimization work, more efficient file and native I/O operations, and vector algorithm improvements such as a new dot-product implementation using fused multiply-add operations. These updates help Java applications run more efficiently across cloud, microservices, analytics, and AI-enabled workloads, helping support better throughput, scalability, and infrastructure efficiency.

Developers and administrators also benefit from improvements that make Java applications easier to diagnose, manage, and maintain in production. JSON thread dumps now emit key identifiers and counts as JSON numbers, improving machine readability for monitoring and incident-response tooling. VM diagnostics now report current open file descriptors, helping teams identify resource issues faster, while Bash autocompletion for jcmd helps improve administrator productivity. Additional refinements to `HttpServer` routing, JavaDoc behavior, ISO-8601 formatting, CLDR locale data, source-code validation, and `ServiceLoader` behavior help reduce developer friction, improve reliability, and strengthen the overall Java application lifecycle.

Helidon 27 aligns with the JDK 27 release and OpenJDK cadence, reinforcing its role within the Oracle Java Verified Portfolio (JVP) as an Oracle-backed framework for building secure, cloud-native Java microservices and AI-enabled applications. Helidon continues to build on modern Java capabilities such as virtual threads, helping developers create highly concurrent services with simpler code and improved resource efficiency.

Helidon 27 advances developer productivity and enterprise integration through declarative APIs, Helidon Data JDBC, Helidon Messaging, enhanced telemetry, MicroProfile alignment, and data integration updates. These capabilities help Java developers build production-ready microservices and enterprise application services using the Java platform they already know. Get the latest Helidon release at helidon.io.

JavaFX 27 aligns with the JDK 27 release cycle and continues to provide a modern Java client application framework for building rich, interactive user experiences. Highlights include the Metal rendering pipeline on macOS for smoother performance on modern Apple hardware, richer text editing controls, and continued accessibility improvements.

As part of the Java Verified Portfolio, JavaFX gives Oracle Java SE Universal Subscription customers a supported path for building and maintaining sophisticated Java user interfaces, including analytics dashboards, operational consoles, and data visualization tools that support modern enterprise and AI-driven application experiences.

The Oracle Java Platform Extension for Visual Studio Code continues to help developers build, run, debug, test, and modernize Java applications in VS Code, while supporting access to the latest Oracle JDK and early-access builds. Recent updates help improve the developer experience across notebooks, run configuration settings, Maven configuration, formatting, and JDK download workflows.

As part of the Java Verified Portfolio, the extension gives Oracle Java SE Universal Subscription customers an Oracle-supported development experience for adopting new Java releases and capabilities with greater confidence.

Oracle Jipher 20 wraps a FIPS 140-3 validated OpenSSL cryptographic module for Java applications and is now included in Java Verified Portfolio, offering Oracle-backed support, governance, and clearer roadmap visibility. Jipher 20 adds support for newer JDK releases, ML-KEM and ML-DSA post-quantum algorithms, KDF and HKDF through standard Java APIs, and FFM-based OpenSSL integration from JDK 25 onward. To learn more, read the Oracle Jipher 20 blog.

Java delivers increased innovation, performance, efficiency, and cost savings when deployed on OCI, which is the first cloud provider to support Oracle JDK 27. By delivering Oracle Java SE and advanced features such as the Java Management Service at no additional charge on OCI, Java 27 helps developers create and deploy applications that run faster in the cloud, better, and with optimized cost-performance.

The Oracle Java SE Universal Subscription provides customers with best-in-class support. It now includes Java Verified Portfolio in addition to the Java SE Subscription Enterprise Performance Pack, Java Management Service, triage support for the entire Java portfolio, and the flexibility to upgrade at the pace of customers’ businesses. This helps IT teams manage complexity, mitigate security risks, and contain costs.

JDK 27 is the 18th Feature Release delivered on time through the six-month release cadence. This level of predictability allows developers to easily manage their adoption of innovation thanks to a steady stream of improvements.

Java continues to evolve as a trusted platform, delivering the performance, stability, and security enterprises need to run and modernize business-critical applications. Oracle will provide updates to JDK 27 until March 2027 when it will be superseded by Oracle JDK 28.

As with previous releases, Java 27 is the result of the contributions of many individuals and organizations in the OpenJDK Community — we all build Java together!

Oracle would like to thank the developers working for organizations including Alibaba, Amazon, ARM, Google, IBM, ISCAS, Microsoft, NTT Data, NVIDIA, Red Hat, Rivos, and SAP for their notable contributions. We are also thankful to see contributions from smaller organizations such as Bellsoft, Bytesoft, and Loongson, as well as independent developers who collectively contributed 16% of the fixes in Java 27. The following individuals provided invaluable feedback on build quality, logged good quality bugs, or offered frequent updates:

David Karnok (RxJava)

Mark Thomas (Apache Tomcat)

Nikhil J. Nanivadekar (Eclipse Collections)

Rafael Winterhalter (Byte Buddy)

Robert Scholte (Apache Maven)

Stefan Bodewig (Apache Ant)

Stefano Cordio (AssertJ)

Additionally, through the OpenJDK Quality Outreach program we would like to thank the following FOSS projects and individuals who provided excellent feedback on testing Java 27 early access builds to help improve the quality of the release:

Apache Lucene (Uwe Schindler)

JOOQ (Lukas Eder)

JUnit 5 (Marc Philipp)

Oracle is expanding Learn.java with the official @Learn-Java YouTube channel, bringing video lessons together with written Java learning resources. Oracle’s Java in Education team has also launched new programs including MCQ Mondays, Duke’s Debugging Challenge, and an AP CSA-focused lecture series with supporting materials, labs, and AI activities for students and teachers.

Java continues to be the #1 Language in overall enterprise organizational use. As the on-time delivery of improvements with Java 27 demonstrates, through continued thoughtful planning and ecosystem involvement, the Java platform is well-positioned for modern development, growth in the cloud, and enabling AI-focused application use-cases.

Keep learning and stay connected with Java news, updates, and community resources by:

Visiting Dev.java (Oracle’s dedicated portal to advance your Java knowledge and community participation).

Visiting Ops.java (Oracle’s trusted hub for Java operations, support, troubleshooting, security, performance, and serviceability guidance).

Visiting Learn.java (Oracle’s destination for beginners, students, and teachers learning Java).

Visiting Inside.java (news and views by the Java Team at Oracle).

Listening to the Inside.java podcasts (an audio show for Java Developers brought to you directly from the people that make Java at Oracle).

Watching Inside.java Newscasts (a video show extending the Inside.java podcast).

Watching Java on YouTube (Oracle’s Java videos to help grow your Java expertise).

Watching JEP Café (Oracle’s technical exploration into popular JDK Enhancement Proposals).

Joining the OpenJDK mailing lists (to learn about the progress of your favorite OpenJDK projects).

Following Java on X.

Subscribing to the Inside Java Newsletter (a publication summarizing many of the key Java technology and community updates from Oracle).

Subscribing to the official Java community on LinkedIn.
