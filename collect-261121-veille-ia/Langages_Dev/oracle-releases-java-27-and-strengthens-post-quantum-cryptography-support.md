---
id: collect-261121-veille-ia/langages-dev/oracle-releases-java-27-and-strengthens-post-quantum-cryptography-support
title: "Oracle Releases Java 27 and Strengthens Post-Quantum Cryptography Support"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-09-15"]
keywords: ["java", "oracle", "post-quantum", "cryptography", "applications", "data", "support", "security"]
source: https://www.oracle.com/news/announcement/oracle-releases-java-27-and-strengthens-post-quantum-cryptography-support-2026-09-15/
source_anchor: ""
source_lines: [1, 101]
section: "Langages_Dev"
---
# Oracle Releases Java 27 and Strengthens Post-Quantum Cryptography Support

Austin, Texas—Sep 15, 2026

Oracle today announced the availability of Java 27, the latest version of the world’s number one programming language and development platform. Featuring thousands of performance, stability, security, and productivity improvements, Java 27 (Oracle JDK 27) provides a strong foundation for continued Java innovation. To help organizations prepare for more secure communications in a post-quantum world, Java 27 advances its post-quantum cryptography (PQC) capabilities with hybrid key exchange for TLS 1.3. Oracle is also highlighting JDK 28 early access (EA) to preview future innovations such as Project Valhalla. In addition, Oracle is announcing aligned updates across the Java ecosystem and Oracle Java Verified Portfolio (JVP), including Helidon 27 for lightweight microservices, JavaFX 27 for rich client applications, and Oracle Jipher 20, now part of JVP, to help customers deploy Java applications in FIPS 140-regulated environments. To learn more about Java 27, join the livestream at 10am CT on Tuesday, September 15<sup>th</sup> here.

“Java’s security, reliability, and enterprise scale make it a critical foundation for AI agents that require trusted access to business systems and sensitive data,” said Arnal Dayaratna, research vice president, software development, IDC. “Java 27’s tools for AI development and post-quantum cryptography strengthen its role at the center of enterprise application development and help organizations prepare for a new generation of applications and security threats in the agentic era.”

“For more than three decades, Java has helped developers build powerful, reliable, and secure applications,” said Georges Saab, senior vice president, Oracle Java Platform and chair, OpenJDK governing board. “Java 27 continues that proud tradition by providing a stable foundation for today’s enterprise workloads and innovative AI and post-quantum cryptography capabilities. With Java 27, Oracle delivers a major post-quantum cryptography milestone and is executing on a roadmap to bring comparable capabilities to JDK releases with long-term support offered by Oracle. This will help customers protect business-critical applications and data with less disruption.”

## __Key JDK Enhancement Proposals (JEPs)__

## Post-Quantum Security, Compliance, and Trust Features

**JEP 527****: Post-Quantum Hybrid Key Exchange for TLS 1.3:** Helps organizations strengthen the protection of sensitive data and business-critical communications against “harvest now, decrypt later” threats while enabling developers to meet evolving cryptographic requirements with minimal application disruption. This is especially important for enterprise AI systems and business-critical applications that require secure communication across data services, models, agents, APIs, cloud platforms, and distributed application environments.

**JEP 538****: PEM Encodings of Cryptographic Objects (Third Preview):** Helps to enhance the secure handling of widely used cryptographic material while improving interoperability with common security tooling and infrastructure. As a result, Java applications can integrate more easily with modern authentication, encryption, and certificate-management systems, helping support secure enterprise AI deployments.

**JEP 536****: JFR In-Process Data Redaction:** Helps troubleshoot production systems more safely by reducing exposure of sensitive information. This supports compliance, privacy, and security requirements while preserving the operational value of JFR diagnostics, which is especially relevant for AI-enabled systems that may process sensitive customer, employee, operational, or regulated data.

## AI, Data, and Modernization Features

**JEP 537****: Vector API (12<sup>th</sup> Incubator):** Helps improve throughput without requiring specialized native code by accelerating analytics, AI inference, and workloads for scientific computing and media. By accelerating data processing and delivering more insights and value with less hardware, the vector API improves Java application performance while preserving the portability, maintainability, and security expected from Java.

**JEP 533****: Structured Concurrency (Seventh Preview):** Helps reduce development risk for cloud, microservices, and AI-enabled applications by enabling developers to build scalable, resilient services with fewer concurrency defects. This is relevant for AI workloads that coordinate many parallel tasks such as model calls, data retrieval, enrichment, ranking, orchestration, and real-time decisioning. In addition, by improving reliability, cancellation, error handling, and observability for multithreaded code, it helps developers accelerate their development projects.

**JEP 532****:** **Primitive Types in Patterns, instanceof, and switch (Fifth Preview):** Helps teams integrate AI inference into business logic faster and with fewer coding errors by making Java more uniform and expressive. This improves developer productivity and code clarity for data-heavy and decision-oriented applications, including AI, analytics, and event-processing workflows that use mixed primitive and object data.

**JEP 531****: Lazy Constants (Third Preview):** Helps improve resource efficiency by providing greater control over initialization timing while preserving performance and correctness benefits of constants. This can improve the user experience and reduce operating costs for AI applications where responsiveness, scalability, and resource efficiency are critical.

## Performance and Runtime Efficiency Updates

**JEP 534****: Compact Object Headers by Default:** Helps accelerate application modernization by reducing JVM memory overhead with compact object headers enabled by default. As a result, customers can run existing applications on a newer JDK and reduce memory footprint. For customers running large Java estates, this can improve application density, reduce infrastructure cost, and increase the scalability of cloud services and microservices as well as data-intensive and AI-enabled workloads.

**JEP 523****: Make G1 the Default Garbage Collector in All Environments:** Helps simplify the migration and modernization of applications by giving developers predictable performance and operational consistency across deployment environments. This helps development teams run cloud, microservices, and AI-enabled Java applications with greater confidence and fewer environment-specific configuration differences.

## Additional Security, Reliability, and Performance Enhancements

Beyond the nine featured JEPs, Java 27 includes non-JEP updates that strengthen security through post-quantum cryptography, improved cryptographic performance, greater runtime visibility, and enhanced diagnostics.

The features Java 27 are a result of continuous collaboration between Oracle and members of the global Java developer community via OpenJDK and the Java Community Process (JCP). For more details on the features in Java 27, please read the Java 27 technical blog post.

## Forward-Looking Innovations with Project Valhalla and Project Leyden

Oracle is highlighting the first major release phase of Project Valhalla in JDK 28 EA, offering a preview of platform innovations moving closer to the JDK mainline. Project Valhalla gives developers early visibility into efforts to bridge the long-standing gap between Java objects and primitives, starting with value classes and objects that could benefit data-heavy workloads in analytics, financial services, scientific computing, and AI. Delivered in stages, the project aims to make Java data representation denser and more efficient while preserving the Java programming model. Valhalla gives developers and enterprises early visibility into future improvements for data-heavy applications, including workloads in analytics, financial services, scientific computing, and AI-enabled systems.

Oracle is also advancing Project Leyden to improve startup time, time to peak performance, and memory footprint, including through ahead-of-time compilation. Together, Project Valhalla and Project Leyden show how Java continues to evolve through carefully designed platform innovation.

## Helidon 27 Helps Developers Build Fast, Lightweight Microservices

Helidon 27 is the latest version of the cloud-native, open‑source Java framework for writing microservices that run on a fast web core powered by Java virtual threads. Notable features offered in Helidon 27 include:

- **Virtual thread foundation:** Helps developers write straightforward synchronous code while achieving the scalability required for cloud-native services. With the adoption of Scoped Values, Helidon 27 continues to build on its Java virtual threads foundation to help developers build highly concurrent applications with simpler code, improved scalability, and greater resource efficiency.
- **Declarative APIs:** Helps developers leverage new modules, including gRPC and messaging, for a familiar, annotation-driven development experience that is transparent to run, lean by design, and optimized for high performance with fewer third-party dependencies.
- **Helidon Messaging:** Helps developers write messaging applications that are connector-neutral by providing a lean messaging stack with fewer third-party dependencies and pluggable connectors for various messaging systems, including Kafka and JMS.
- **Helidon Data JDBC:** Helps developers leverage a lightweight approach that simplifies relational database access while giving them greater control over their SQL and reducing runtime complexity.

With Helidon 27, Helidon’s version numbering and release cadence align with OpenJDK. Helidon is included in Java Verified Portfolio to give customers clearer support, compatibility, and upgrade planning.

## JavaFX 27 Helps Developers Build Rich, Interactive User Experiences

JavaFX 27 gives developers a modern Java client application framework for building rich, interactive user experiences on the Java platform. It continues to improve the foundation for visually rich Java applications, including AI-powered analytics dashboards, operational consoles, and data visualization tools.

JavaFX 27 helps improve performance, usability, and accessibility with a metal rendering pipeline on macOS, richer text editing controls, and continued accessibility enhancements. As part of Java Verified Portfolio, JavaFX is backed by Oracle commercial support, helping customers build and maintain modern Java user interfaces with greater confidence.

## Oracle Jipher 20 Helps Java Applications Meet FIPS 140 Requirements and is Now Included in JVP

Oracle Jipher 20 wraps a FIPS 140-3 validated OpenSSL cryptographic module for Java applications and is now included in Java Verified Portfolio, offering Oracle-backed support, governance, and clearer roadmap visibility. Jipher 20 adds support for newer JDK releases, ML-KEM and ML-DSA post-quantum algorithms, KDF and HKDF through standard Java APIs, and FFM-based OpenSSL integration from JDK 25 onward. To learn more, read the Oracle Jipher 20 blog post.

## Supporting the Global Java Community with Innovations in the Cloud

Java delivers increased innovation, performance, efficiency, and cost savings when deployed on Oracle Cloud Infrastructure (OCI), which is the first cloud provider to support Oracle JDK 27. Java 27 helps developers create and deploy applications that run faster, better, and with optimized cost-performance by delivering Oracle Java SE and advanced features such as the Java Management Service at no additional charge on OCI.

The Oracle Java SE Universal Subscription helps IT teams manage complexity, mitigate security risks, and contain costs by providing customers with best-in-class support. It now includes support for JVP, Java SE Subscription Enterprise Performance Pack, Java Management Service, portfolio-wide triage, and the flexibility to upgrade at a customer’s pace. This helps IT teams manage complexity, mitigate security risks, and contain costs.

To learn more about Java and its global ecosystem, please visit:

- Dev.java: The official destination for Java developers
- Ops.java: The new hub for Java administrators and operations
- Inside.java: News and views from the members of the Java Team at Oracle
- Java YouTube: The official Java YouTube channel for Java learning videos

## Additional Resources

- Download Oracle JDK 27
- Read the Java 27 technical blog post
- Learn more about Oracle Java Verified Portfolio, JavaFX, Helidon, and Jipher
- Learn more about the Oracle Java SE Universal Subscription
- Learn more about the Java Management Service

## About Oracle

Oracle offers integrated suites of applications plus secure, autonomous infrastructure in the Oracle Cloud. For more information about Oracle (NYSE: ORCL), please visit us at www.oracle.com.

## Trademarks

Oracle, Java, MySQL, and NetSuite are registered trademarks of Oracle Corporation. NetSuite was the first cloud company—ushering in the new era of cloud computing.

## Future Product Disclaimer

The preceding is intended to outline our general product direction. It is for informational purposes only and may not be incorporated into any contract. The development, release, timing, and pricing of any features or functionality described for Oracle's products may change at Oracle Corporation’s sole discretion.
