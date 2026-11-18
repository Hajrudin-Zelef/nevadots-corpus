 ICS 35.030
 CCS L 80


                                                        CCIA
China Cybersecurity Industry Alliance Technical Specification
                                                             T/CCIA XXX-2026




              人工智能防火墙安全技术要求
 Security technical requirements for artificial intelligence firewall




Released on XXXX-XX-XX                           Effective since XXXX-XX-XX

              Released by China Cybersecurity Industry Alliance
             Security technical requirements for
                     artificial intelligence firewall

Introduction
As AI technologies continue to transform networks and information systems, security threats
targeting vulnerabilities specific to AI systems are becoming increasingly severe. Traditional
threats can also be enhanced and amplified by attackers using AI-generated tools.

As of 2026, general-purpose firewalls are inadequate for protecting AI systems. There is an
urgent need for firewalls specifically designed to provide security protection for AI systems,
while also incorporating AI-enhanced security capabilities.

Relevant industry demand and preliminary analyses for such AI firewalls have already
emerged around the world:

    •   According to Gartner, firewalls have evolved through several successive generations,
        including packet filtering firewalls, stateful inspection firewalls, next-generation firewalls
        (NGFWs), unified threat management (UTM) systems, and hybrid mesh firewalls (HMFs).
        More recently, industry demand analysis related to AI-Gateways have also emerged.

    •   Network security vendors such as Akamai, Fortinet, and Palo Alto Networks have also
        introduced related offerings, including AI WAF, FortiAI-Protect, and AI-Powered NGFW.

This document specifies additional security defense technical requirements for artificial
intelligence firewalls, on top of the Chinese National Firewall Standard GB/T 20281—2020,
Information Security Technology—Security Technical Requirements and Testing Assessment
Approaches for Firewall.



1. Scope
This document specifies the technical requirements for AI firewalls.

This document is applicable to the design, development, testing and deployment of AI
firewalls.



2. Normative references
The contents of the following documents constitute indispensable provisions of this
document through normative references in the text. For dated references, only the edition
corresponding to the cited date applies to this document. For undated references, the latest
edition of the referenced document (including any amendments) applies to this document.
GB/Z 185.1-2026 Artificial Intelligence—Agent Interconnection—Part 1:General Architecture

GB/T 25069-2022 Information Security Techniques—Terminology

GB/T 20281-2020 Information Security Technology—Security Technical Requirements and
Testing Assessment Approaches for Firewall



3. Terminology
The terms and definitions specified in GB/T 25069—2022, GB/T 20281-2020 and those given
below apply to this document.


3.1.     artificial intelligence firewall
A network security product that parses data flows associated with artificial intelligence systems,
implements access control and security protection functions, and provides artificial
intelligence-enhanced security protection capabilities.


3.2.     token
The smallest unit of measurement for information processed by a model.


3.3.     tool
A device, software, or system that provides specific functionality and can be used.


3.4.     skills
A modular capability extension encapsulated in a structured manner.

NOTE: Typically used in the fields of AI and AI agents.


3.5.     tool calling
The behavior of an AI model or agent invoking external tools, Application Programming
Interfaces (APIs), and services.


3.6.     caller
An access subject that invokes resources, including human users, agents, business
applications, and other entities.
3.7.     callee
Resources being invoked, including LLM services, agents, tools, APIs, and other resources.


3.8.     AI agent
An entity capable of perceiving its environment, making autonomous decisions, and
proactively taking actions to achieve specific goals.



4. Abbreviation
    The following abbreviations apply to this document:

    A2A: Agent to Agent Protocol

    ACP: Agent Communication Protocol

    AI: Artificial Intelligence

    ANP: Agent Network Protocol

    API: Application Programming Interface

    EDR: Endpoint Detection and Response

    gRPC: Google Remote Procedure Call

    HTTP: HyperText Transfer Protocol

    HTTPS: HyperText Transfer Protocol Secure

    IAM: Identity and Access Management

    IP: Internet Protocol

    IPS: Intrusion Prevention System

    IT: Information Technology

    LLM: Large Language Model

    MCP: Model Context Protocol

    NHI: Non-Human Identity

    OAuth 2.0: Open Authorization 2.0

    RAG: Retrieval Augmented Generation

    SLIM: Secure Low-Latency Interactive Messaging

    SSL: Secure Socket Layer
    SSRF: Server-Side Request Forgery

    SQL: Structured Query Language

    TLS: Transport Layer Security



5. Overview
An AI firewall is a new type of boundary security protection product that, on the basis of a
general-purpose firewall, can effectively address security risks specific to AI systems and
provides unified security management and control capabilities. The differences in
characteristics between AI firewalls and general-purpose firewalls include, but are not limited
to, the following:

——identification, classification, and control of non-human identities (NHIs), such as AI
agents;

——multidimensional perception of underlying security attack intents and semantics;

——AI-enhanced capabilities for identifying rapidly evolving variants and unknown threats;

——autonomous security reasoning, intelligent learning and generation of security policies,
automated policy upload and deployment, coordinated blocking;

——monitoring and observability capabilities for intelligent computing resource invocation,
such as AI agents and models and the consumption of computing power.

The security technical requirements for AI firewalls specified in this document refer to the
specific security requirements that products must additionally meet on the basis of satisfying
the relevant enhanced-level requirements specified in GB/T 20281-2020.



6. Technical Requirements

6.1.     Security functional requirements

6.1.1. Networking and deployment

6.1.1.1. Deployment mode
The product must meet the relevant requirements specified in 6.1.1.1 of GB/T 20281—2020.


6.1.1.2. Deployment location
The product must support the following deployment locations:
    1)   between callers (including users, AI agents, or business applications) and callees
         (including AI inference servers, public LLM service APIs, AI agents, tools, APIs, etc.),
         including:

         — in-domain callers and out-of-domain callees;

         — in-domain callers and in-domain callees.

    2)   between the network boundaries of internal zones within an AI information system,
         such as training zones, inference zones, and storage zones.


6.1.1.3. Deployment scenarios
The product must support the following deployment scenarios:

    1)   scenarios involving Internet egress of cloud services and internal network zone
         boundaries;

    2)   scenarios involving internal network zone boundaries in data centers (including
         intelligent computing, general-purpose computing, management, and other zones);

    3)   scenarios involving enterprise campus network boundaries and internal network
         zone boundaries.


6.1.1.4. Routing
In addition to meeting the relevant requirements specified in 6.1.1.2 of GB/T 20281—2020,
the product must also provide AI-oriented routing capabilities:

    1)   The product must support interconnection with multiple AI services and be capable
         of forwarding requests to corresponding capability endpoints based on access
         destinations, request types, request modalities, security policies or business policies;

    2)   The product must support capability routing based on the identity, Internet Protocol
         (IP) address, network zone, data sensitivity level, and request modality of the request
         caller, as well as security policies or business policies, and must support the
         configuration of static routing, policy-based routing, or dynamic routing;

    3)   The product must support parsing application-layer authentication information in
         traffic, such as API keys, user tokens, and service identity identifiers, and dynamically
         allowing or denying requests based on predefined routing policies, as a supplement
         to network-layer information.


6.1.1.5. High availability
In addition to meeting the relevant requirements specified in 6.1.1.3 of GB/T 20281—2020,
the product must also provide AI-oriented load balancing functions:

    1)   The product must support dynamic load sharing across model services and
         computing power zones based on computing power resource utilization, for
         example, through token usage and coordinated sensing with computing servers;

    2)   In the event of an error, the product must automatically retry using other models or
         computing resource clusters to achieve automatic failover.


6.1.2. Network-layer control

6.1.2.1. Access control

6.1.2.1.1. Packet filtering
In addition to meeting the relevant requirements specified in 6.1.2.1.1 of GB/T 20281—2020,
the product must also meet the following requirements for the packet filtering function:

    1)   User-defined security policies must be supported. Such security policies must
         support combinations of some or all of the following: MAC address, IP address, port,
         user identity, AI agent identity, terminal type, application type, protocol type, and
         time.


6.1.2.1.2. Network address translation
The product must meet the relevant requirements specified in 6.1.2.1.2 of GB/T 20281—2020.


6.1.2.1.3. Stateful inspection
The product must meet the relevant requirements specified in 6.1.2.1.3 of GB/T 20281—2020.


6.1.2.1.4. Dynamic open ports
The product must meet the relevant requirements specified in 6.1.2.1.4 of GB/T 20281—2020.


6.1.2.1.5. IP/MAC address binding
In addition to meeting the relevant requirements specified in 6.1.2.1.5 of GB/T 20281—2020,
the product must also support intelligent learning of IP/MAC address binding relationships
and anomaly detection.


6.1.2.1.6. IP Blacklisting and Whitelisting
The product must support IP address whitelist and blacklist functions, including but not
limited to the following:

    1)   The product must support the configuration of source/destination IP address
         whitelists and blacklists;
         — For example, for sensitive sessions initiated by NHI, an whitelists and blacklists -
         based access control mode must be supported, whereby sessions are permitted to
         be established only with the IP addresses and ports of pre-authorized destinations,
         such as public network resources, internal network models, and necessary services.

    2)   Batch distribution of whitelists and blacklists must be supported.


6.1.2.2. Traffic management

6.1.2.2.1. Bandwidth management
In addition to meeting the relevant requirements specified in 6.1.2.2.1 of GB/T 20281—2020,
the product must also support the following bandwidth management functions:

    1)   Bandwidth must be configurable separately for upstream and downstream traffic,
         and upstream and downstream traffic must be limited separately;

    2)   Bandwidth allocation and limitation based on AI service session attributes must be
         supported, and independent bandwidth quotas must be configurable for different
         unique AI agent identifiers and session service types;

    3)   Limits on the number of concurrent sessions based on unique AI agent identifiers
         and session service types must be supported;

    4)   Session-based traffic rate limiting must be supported, enabling the data
         transmission rate of an individual AI service session to be limited;

    5)   Identification and rate limiting of abnormal traffic sessions must be supported,
         including sessions with sudden traffic surges, sessions carrying unusually large
         volumes of traffic, and sessions suspected of remote control via covert channels.


6.1.2.2.2. Connection control
The product must support limiting the maximum number of concurrent sessions and the new
connection establishment rate for an individual AI agent, so as to prevent network
performance from being affected by a large number of unauthorized connections.


6.1.2.2.3. Session management
  The product must support the following session management functions:

    1)   The product must terminate a session when the session remains inactive for a
         specified period, when an abnormal condition occurs (e.g. the number of session
         establishments and terminations per unit time exceeds a threshold), or after the
         session ends;

    2)   The product must provide session state monitoring and attribute management
         capabilities to support AI service scenarios, establish standard five-tuple sessions for
        network traffic traversing the boundary, and perform full-lifecycle management of
        such sessions;

   3)   The product must support the configuration of independent session timeout periods
        for different types of AI service sessions. The configuration granularity must reach
        the session service type level so as to meet the requirements of long-duration
        sessions for services such as model training and batch inference;

   4)   The product must support the configuration of access control policies based on
        extended attributes of AI service sessions, including session initiator type, unique AI
        agent identifier, and session service type;

   5)   The product should extend AI service sessions with the following verifiable and
        configurable dedicated session attributes. All such attributes must be directly
        accessible to modules such as access control and traffic management:

        ——Session initiator type: identifies the identity type of the session initiator, with
        values including “human user”, “NHI”, “process”, and others;

        ——Unique AI agent identifier: an identifier used to identify an AI agent at the
        network layer. It may be derived from IP address bindings, preconfigured mapping
        tables, fixed port ranges, information carried in packets, or other means;

        ——Session service type: identifies the category of AI service corresponding to the
        session, with values including “model invocation”, “training data”, “inter-agent
        communication”, “model synchronization”, and others.


6.1.2.3. Network isolation
   1)   The product must support security domain partitioning, enabling AI service
        components, such as AI agents, model services, and training clusters, to be deployed
        in independent security domains and isolated at the network layer from the office
        network, business system zone, database zone, Internet egress zone, and operations
        and maintenance management zone;

   2)   All network sessions between different security domains, including AI service
        sessions, must pass through the firewall. The firewall must serve as the sole ingress
        and egress point for inter-domain communication;

   3)   Intra-domain or inter-domain access control policies must be established based on
        session attributes, specifying the session types permitted to communicate and
        prohibiting unauthorized sessions.


6.1.2.4. Encrypted traffic parsing
   1)   The product must support Secure Socket Layer (SSL)/ Transport Layer Security (TLS)
        proxy decryption of HTTPS/HTTP traffic, including TLS 1.2 and TLS 1.3;

   2)   The product must support AI-enhanced feature analysis of encrypted traffic.
6.1.3. Application-layer control

6.1.3.1. Application protocol control
   1)   The product must support the parsing and control of general-purpose application-
        layer protocols, such as HTTP/HTTPS, WebSocket, and gRPC;

   2)   The product must support the parsing and control of AI agent communication
        protocols, such as Agent to Agent Protocol (A2A), including Message content
        scanning and Artifact content identification;

   3)   The product should support the parsing and control of emerging AI agent
        communication protocols, such as ACP, ANP, and SLIM;

   4)   The product must support the parsing of model context protocols, such as Model
        Context Protocol (MCP), and identify and control context information and tool call
        requests transmitted through such protocols;

   5)   The product must support the parsing and control of OpenAI-compatible protocols
        and interfaces, including Function Calling, Tool Use, and the Responses API;

   6)   The product must support semantic-level parsing of protocol messages, identify
        invocation intents such as tool calls, resource access, and prompt retrieval, and
        enforce controls based on the identified intents.


6.1.3.2. Identity and authorization management
   1)   The product must support authentication-based network access control;

   2)   The product must support the identification and authentication of external NHIs that
        invoke internal resources, using credentials such as X.509 certificates, OAuth 2.0
        tokens, and JWTs;

   3)   The product must support the identification of internal NHIs, such as AI agents,
        digital humans, embodied intelligent entities, workloads, machine identities,
        application accounts, and process IDs.

   4)   The product should support integrating NHIs into Identity and Access Management
        (IAM), establishing a unified identity management system, applying access control
        models such as Role-Based Access Control (RBAC), Policy-Based Access Control
        (PBAC), and Attribute-Based Access Control (ABAC), and integrating them with
        network-layer security policies;

   5)   The product must support identity traceability for enterprise users accessing
        sensitive resources, such as publicly available external LLM (for example including
        ChatGPT, DeepSeek, and Qwen), and must identify and record the identity of the
        user initiating the request, the department to which the user belongs, and terminal
        device information;
   6)   The product must support behavior and intent analysis for NHIs and human users,
        construct and train self-learning intelligent behavior models by collecting
        multidimensional behavioral features, and use such models to identify and block
        threat entities and behaviors.


6.1.3.3. Application-layer resource general access control
   1)   Application resources subject to access restrictions must perform unified
        authentication of callers using API keys, X.509 certificates, ID tokens, or other means;

   2)   The product must support dynamic authorization and access control, evaluate the
        trustworthiness of accessing entities based on information such as the invoking user’
        s identity, role, scenario, and context, grant resource accessors the minimum
        necessary privileges according to the evaluation results, and perform real-time
        blocking and/or privilege freezing when anomalies occur;

   3)   The product must intercept high-concurrency malicious requests targeting
        application-layer resources and support protection against application-layer
        denial-of-service (DoS) attacks.


6.1.3.3.1. Model resource control
   1)   Resources such as AI agents and LLM must be exposed as APIs when providing
        services externally;

   2)   The product must support registration and admission control, as well as fine-
        grained authorization, for MCP service resources;

   3)   The product must support monitoring the utilization status of computing power
        resources for business application models and regulating or denying abnormal
        behaviors, such as low-priority processes preempting high-priority resources and
        prolonged resource exhaustion;

   4)   The product must support control over token usage by business application model
        inference tasks or sessions and support a circuit breaker mechanism based on
        token-level budgets.


6.1.3.3.2. Tool calling control
   1)   For specific sensitive scenarios, an allowlist and denylist mechanism for tool calls
        must be established, and tool calls must comply with predefined authorization
        policies;

   2)   The product should support legitimacy checks on tool calls made through
        mechanisms such as MCP and Skills, and detect attacks initiated through tool
        parameters, including Structured Query Language (SQL) injection, command
        injection, path traversal, and Server-Side Request Forgery (SSRF);
   3)   The product should support the redirection of high-risk operations.


6.1.3.3.3. API call control
   1)   The product must support real-time monitoring of API access status. When the API
        access volume exceeds a specified threshold, access must be blocked to prevent
        access channels from becoming congested;

   2)   The product must support data leakage detection for API response content, identify
        whether responses contain sensitive data, and trigger alerts or blocking actions;

   3)   The product must support auditing of API call chains, record and generate complete
        call chain logs, and facilitate post-event auditing and leakage tracing;

   4)   The product must support risk assessment of third-party API calls and identify risky
        behaviors, such as excessive data sharing and unauthorized transmission, when
        enterprise applications call external LLM APIs.


6.1.3.4. Content Output control
   1)   The product must support protection against prompt injection attacks, covering
        both direct and indirect prompt injection, such as persistent inducement across
        multiple turns, context concatenation, and concealed content in front-end interfaces;

   2)   The product must support the detection of malicious instructions hidden in
        multimodal content, including images, audio, video, and files;

   3)   The product must support the configuration of policies that prohibit or restrict users
        from uploading sensitive internal enterprise data, such as code repositories,
        customer information, financial data, and internal documents, to external publicly
        available LLM;

   4)   The product should support content parsing and sensitive information scanning of
        network traffic and multimodal or multi-format files uploaded to LLM, such as PDF,
        Word, Excel, PowerPoint, and archive files, so as to prevent internal data leakage
        through attachments;

   5)   The product must support the detection and filtering of files downloaded by users
        from LLM responses, identify whether downloaded files contain prohibited or
        sensitive content, and support truncation during the output process;

   6)   The product must support the identification and control of attack-related anomalies,
        such as model topic deviation and access beyond authorized privileges by AI agents;

   7)   Before model invocation, user input must undergo anti-virus and intrusion
        prevention inspection, semantic analysis, sensitive instruction identification, and
        content compliance checks;

   8)   During the model output stage, sensitive information filtering and blocking,
        together with compliance checks, must be performed to prevent the model from
        generating content containing account credentials, private data (such as bank card
        numbers, personal IDs, and telephone numbers), commercially sensitive information,
        business secrets, or other sensitive content. Sensitive information outputs must be
        intercepted to prevent data leakage;

   9)   The product must support values-based content filtering, review input and output
        content, detect illegal or non-compliant content including politically sensitive topics,
        discrimination, harmful content, or other prohibited matters, and automatically
        block non-compliant inputs and outputs during interactions with LLM;

   10) The product must support real-time desensitization of content submitted by
       enterprise users to publicly available LLM. Before a request is sent, sensitive data,
       such as names, identity card numbers, customer information, and source code
       snippets, must be automatically identified and desensitized through measures such
       as replacement, masking, or blocking, so as to prevent sensitive data leakage.


6.1.4. Attack protection

6.1.4.1. Network-layer attack protection
   1)   The product must support the detection and blocking of various malformed packet
        attacks, including but not limited to:

        ——Local Area Network Denial (LAND) attacks;

        ——Smurf attacks;

        ——Teardrop attacks;

        ——Ping of Death attacks;

       ——Internet Control Message Protocol (ICMP) redirect or destination unreachable
   message attacks;

       — — invalid Transmission Control Protocol (TCP) packet flag settings, such as
   Acknowledgment (ACK), Synchronize (SYN), and Finish (FIN) flags;

   2)   The product must support protection against network-layer DoS attacks, including
        but not limited to:

        ——SYN Flood attacks;

        ——ACK Flood attacks;

        ——ICMP Flood attacks;

        ——User Datagram Protocol (UDP) Flood attacks;

   3)   The product must support protection against network scanning attacks and must
        detect and record network scanning behavior;
   4)   The product must support the detection and blocking of lateral attacks;

   5)   The product must support the identification and active termination of abnormal
        sessions, for example:

        ——zombie sessions that have no data transmission for longer than the specified
        timeout period;

        ——oscillating sessions for which the number of establishments and terminations
        per unit time exceeds a threshold;

        ——unauthorized sessions with abnormal transport-layer protocol states;

        ——sessions suspected of remote control via covert channels;

   6)   The product must support AI-enhanced detection and blocking of malformed
        network-layer packets, protocol anomalies, and unknown threats.


6.1.4.2. Application-layer attack protection
   1)   The product must support protection against application-layer DoS attacks;

   2)   The product should support semantic consistency validation of metadata across
        protocols to prevent cross-protocol attacks.


6.1.4.2.1. Model attack protection
   1)   The product must support the detection of malicious payloads concealed in model
        files, including but not limited to the following:

        ——identifying model files in network traffic and comparing them based on feature
        values, model fingerprints, or other characteristics;

        ——identifying model files in network traffic and detecting and parsing malicious
        Portable Executable (PE) headers, malicious scripts, and other malicious content
        through static file scanning;

        — — placing models in external isolated sandboxes for execution through
        coordinated sandbox-based isolation, and jointly detecting abnormal outbound
        network connections or API call behavior.

   2)   For potential vulnerabilities in models, the product must support the detection of
        plaintext traffic and SSL-encrypted traffic and detect potential triggers contained in
        prompts and payloads, so as to defend against attacks that exploit vulnerabilities
        through malicious traffic;

   3)   The product must detect and block high-risk operations on model weight files and
        training datasets, such as modification, overwriting, and deletion;

   4)   The product must support protection against model theft and detect potentially
        sensitive model files in traffic;
5)   The product must support the detection and blocking of resource-exhaustion
     attacks against model services;

6)   The product must support the detection and blocking of attacks in which malicious
     file uploads induce models to perform path traversal, command injection, or other
     malicious behavior;

7)   The product should support the detection of and protection against attacks that
     trigger model performance degradation or service disruption by constructing
     oversized Context Window Overflow;

8)   The product must support user-defined intrusion behavior patterns and allow
     corresponding detection and blocking rules to be customized according to user
     requirements;

9)   The product must be capable of blocking direct and indirect prompt injection attacks,
     including but not limited to injected instructions involving jailbreak attacks, role-
     playing, topic deviation, inducive instructions, and other techniques;

10) The device must include a built-in AI attack signature database covering
    vulnerability-related indicators, attack samples, and malicious behavior rules related
    to LLM and AI agents;

11) The product must support AI-enhanced detection and blocking of unknown threats
    and variant attacks and automatically generate protection policies based on
    detection results;

12) The AI attack signature database must be updated in coordination with the
    traditional Intrusion Prevention System (IPS) signature database. Information such
    as detection signatures and policies must support real-time upload to the cloud,
    multi-point synchronization, automated updates, and manually triggered updates;

13) The product must support centralized distribution of custom AI attack signature
    rules by the management platform, ensure rapid response to and coordinated
    defense against emerging AI attacks, and support coordinated response with
    firewalls at the network layer;

14) Secure images from enterprise private repositories and officially certified sources,
    with code signatures, must be used. Runtime environments and dependency
    versions must be standardized, and image signatures must be verified during image
    transfer to prevent untrusted images from entering the production environment;

15) The product should support protection against man-in-the-middle (MITM) attacks
    targeting enterprise users accessing external LLM, detect and alert on abnormal
    proxy behavior or certificate hijacking, and ensure the security of communication
    channels.
6.1.4.2.2. Tool calling attack protection
   1)   The product must support static security scanning during the registration of third-
        party Skills and MCP Servers, including source verification, signature verification,
        declaration of requested sensitive permissions, and matching against known
        vulnerabilities;

   2)   The product must be capable of protecting against tool description poisoning
        attacks, such as by blocking the poisoning of Skills repositories and prohibiting the
        automatic installation of Skills. All Skills must undergo security review before
        installation;

   3)   The product must support the detection of malicious Skills, identify requests to
        invoke malicious Skills, and block such requests in time;

   4)   The product must support the detection of attacks involving malicious registration
        of or tampering with tool calls (Function Calling/Tool Use), so as to prevent attackers
        from inducing models to call unauthorized internal APIs or perform dangerous
        operations;

   5)   The product must support call chain correlation analysis for auditing and traceability.


6.1.4.2.3. API attack protection
   1)   The product must support security protection for model service APIs, detect and
        block attacks such as SQL injection and cross-site request forgery (CSRF), and
        provide semantic analysis capabilities to improve the detection rate and accuracy;

   2)   The product must provide security protection capabilities for internal APIs, establish
        API baselines, identify shadow APIs and zombie APIs, and detect anomalous API
        attack behavior;

   3)   The product must support the detection and blocking of behavior that uses an LLM
        as a stepping stone to probe or attack internal networks, including SSRF, port
        scanning, and service discovery.


6.1.5. Security auditing, alerting, and statistics

6.1.5.1. Security auditing
   1)   The product must support security auditing functions, including but not limited to
        the following:

        a)   recording event types;

        b)   access requests matched by the product’s security policies;

        c)   detected attack behavior;
     d)   events involving autonomous access by AI agents to resources such as tools,
          MCP, APIs, and bare models;

     e)   events involving user access to sensitive resources, such as external LLMs
          including ChatGPT;

     f)   AI policy matching and response events.

2)   Log content:

     a)   the date and time of occurrence of the event;

     b)   the subject, object, and description of the event, where data packet logs include
          the protocol type, source address, destination address, source port, destination
          port, and other information;

     c)   a description of the attack event;

          ——In particular, for security attacks targeting inference and AI agents, the
          product must support comprehensive full-chain logging of user identity,
          prompt input, inference results, tool calls, execution results, data flows, and
          other relevant information, and must support chain reconstruction;

     d)   computing power resource access logs, including but not limited to the
          following:

          ——the identity of the human user or NHI caller;

          ——the name of the accessed model, AI agent, MCP service, tool, or knowledge
          base;

          ——the request type, request modality, and session identifier;

          ——the access result and policy enforcement result;

          ——session context association information, such as correlation indexes, to
          ensure that multi-turn requests within the same session are not recorded
          separately without correlation.

3)   Log management:

     a)   Only authorized administrators must be permitted to access logs, and functions
          such as log viewing and export must be provided;

     b)   The product must support intelligent aggregation and retrieval of session-level
          logs by authorized administrators based on dimensions such as session
          identifier, user identity, and time range;

     c)   Audit events must be searchable by criteria such as date, time, subject, and
          object;

     d)   Logs must be stored in non-volatile storage media that retain data in the event
          of power loss;
         e)   The log retention period must be set to no less than six months;

         f)   When the storage space reaches a specified threshold, the product must be
              capable of notifying authorized administrators and ensuring the normal
              operation of the auditing function;

         g)   Logs must support automated backup to other storage devices.


6.1.5.2. Security alerting
    1)   The product must support alerting on attack behavior defined in 7.1.5, variants of
         similar attacks, abnormal access behavior, policy violations, anomalies in AI
         capability invocation, cases where the confidence level of AI model inference results
         continuously falls below a threshold, and other abnormal conditions. Alert
         information must include at least the following:

         ——event subject;

         ——event object;

         ——event description;

         ——severity level;

         ——handling status;

         ——date and time of occurrence of the event.

    2)   The product must be capable of aggregating frequently recurring alert events of the
         same type. The product should use AI capabilities to perform correlation analysis on
         large volumes of logs, extract summaries, and aggregate risks, so as to reduce
         duplicate alerts and prevent alert storms.


6.1.5.3. Statistics

6.1.5.3.1. Network traffic statistics
The product must support the graphical display of network traffic information, including but
not limited to the following:

    1)   Network traffic statistics must be supported based on criteria such as IP address,
         time period, protocol type, NHI, AI service session, or any combination thereof.


6.1.5.3.2. Computing power usage statistics
The product must support statistics on model and computing power usage at the granularity
of resource callers and tasks, including but not limited to the following:

    1)   the number of times users, applications, and AI agents access model services;
    2)   the number of model inference requests and token usage.


6.1.5.3.3. Intelligent asset statistics
The product must support the discovery, statistics, and display of AI assets, including but not
limited to the following:

    1)   Model assets:

         ——model name;

         ——model type;

         ——service address;

         ——model version;

         ——last access time;

    2)   AI agent assets:

         ——AI agent name;

         ——AI agent version;

         ——associated user or organization;

         ——associated terminal information;

         ——associated model;

         ——times of accesses;；

         ——process information;

         ——last activity time;

    3)   MCP and tool assets:

         ——service name;

         ——protocol type;

         ——service address;

         ——times of calls;

         ——last access time;

    4)   Knowledge base and workflow assets:

         ——name;

         ——service type;

         ——call statistics;
         ——last update time.


6.1.5.3.4. Attack event statistics
    1)   When computing power resource usage exceeds a reasonable quota, the event
         must be recorded as a resource exhaustion event;

    2)   When the number of resource requests issued exceeds a reasonable quota, the
         event must be recorded as a DoS event;

    3)   The product must support categorized statistics on prompt injection attacks, tool
         abuse attacks, unauthorized model access attacks, tool call attacks, and anomalous
         AI agent behavior;

    4)   When multi-stage attacks or complex call-chain attacks occur, the product must
         support attack path tracing and call chain reconstruction to reconstruct and record
         the attack process, attack targets, and attack intent;

    5)   The product must support intelligent simplification of statistical results to prevent
         operational fatigue caused by massive volumes of logs.


6.2.     Security requirements of the Product Itself

6.2.1. Identification and authentication
The security requirements for identification and authentication of the product include but are
not limited to the following:

    1)   The product must identify and authenticate users, and user identities must be
         uniquely identifiable;

    2)   The product must securely protect user authentication information and ensure its
         confidentiality during storage and transmission;

    3)   The product must provide failed login attempt handling functions, such as limiting
         the times of consecutive invalid login attempts;

    4)   The product must provide login timeout handling functions and automatically
         terminate a login session when the session times out;

    5)   When password-based authentication is used, the product must check the
         complexity of user-defined passwords to ensure that user passwords meet specified
         complexity requirements;

    6)   Where default passwords exist in the product, users must be prompted to change
         them so as to reduce the risk of user identity impersonation;

    7)   Authorized administrators must be authenticated using a combination of two or
         more authentication techniques;
    8)   The product must identify and authenticate callers of AI model inference services to
         ensure that only authorized application systems or users can invoke AI model
         inference interfaces, thereby preventing unauthorized access and model theft;

    9)   The product must identify AI models themselves to ensure that each model can be
         uniquely identified during loading, updating, and invocation, thereby preventing
         model tampering or replacement.


6.2.2. Management capabilities
The security requirements for the management capabilities of the product include but are not
limited to the following:

    1)   The product must provide authorized administrators with functions for setting and
         modifying data parameters related to security management;

    2)   The product must provide authorized administrators with functions for setting,
         querying, and modifying various security policies;

    3)   The product must provide authorized administrators with functions for managing
         audit logs;

    4)   The product must support updates to its own system, including software system
         upgrades and upgrades to various signature databases;

    5)   The product must be capable of synchronizing system time with a Network Time
         Protocol (NTP) server;

    6)   The product must support the synchronization of information such as logs and alerts
         to a log server through the Syslog protocol;

    7)   The product must distinguish administrator roles and support the division of roles
         into system administrator, security operator, and security auditor, with the privileges
         of the three administrator roles mutually constrained;

    8)   The product must provide security policy validity checking functions, such as
         checking security policy matching status;

    9)   The product should provide authorized administrators with AI model lifecycle
         management functions, including model loading, unloading, version rollback,
         staged rollout, and full release, to ensure that the model update process is
         controllable and traceable;

    10) The product should provide authorized administrators with functions for configuring
        and adjusting AI inference policies, including model inference threshold settings,
        confidence level adjustment, and inference result handling rules, enabling
        administrators to flexibly control AI inference behavior according to actual security
        requirements;

    11) The product should provide AI model operational status monitoring functions,
         including real-time display of and alerting on key indicators such as model inference
         latency, resource utilization, and inference success rate.


6.2.3. Management auditing
The security requirements for management auditing of the product include but are not limited
to the following:

    1)   The product must log operations such as user account login and logout, system
         startup, significant configuration changes, addition, deletion, or modification of
         administrators, and saving or deletion of audit logs;

    2)   The product must generate alerts for abnormal states of the product and its modules
         and record such states in logs;

    3)   Log records must include the following information: date and time of occurrence of
         the event, event type, event subject, and operation result;

    4)   Only authorized administrators must be permitted to access logs;

    5)   The product should log lifecycle operations on AI models, such as loading, updating,
         unloading, and rollback, including information such as operation time, operator,
         model version, and operation result;

    6)   The product should maintain audit records of key decision-making behavior during
         AI model inference, including the source of the inference request, inference result,
         confidence level, and security policy actions triggered or generated, so as to ensure
         that the AI decision-making process is traceable and auditable;

    7)   The product should generate alerts and log AI model inference anomalies, including
         inference failures, inference timeouts, abnormal fluctuations in output results, and
         confidence levels that deviate significantly from the normal range.


6.2.4. Management methods
The security requirements for the management methods of the product include but are not
limited to the following:

    1)   The product must support local management through a console port;

    2)   The product must support remote management through network interfaces and
         must be capable of restricting the IP addresses and MAC addresses from which
         remote management is permitted;

    3)   During remote management, all communication data between the management
         terminal and the product must be transmitted in non-plaintext form;

    4)   The product must support monitoring and management through the Simple
         Network Management Protocol (SNMP);
    5)   The product must support separation of management interfaces from service
         interfaces;

    6)   The product must support centralized management through a centralized
         management platform, including monitoring operational status, distributing security
         policies, upgrading system versions, and upgrading signature database versions;

    7)   The product must support management of AI model services through RESTful APIs
         or other standardized interfaces, including operations such as model loading,
         unloading, status queries, and inference testing, and must enforce access control
         and rate limiting on API calls;

    8)   The product must support version updates and configuration changes of AI models
         through secure remote management methods. During the update process, integrity
         verification of model files must be ensured to prevent tampering with or
         replacement of model files during transmission;

    9)   The product must provide health check interfaces for AI model services, enabling
         external monitoring systems to obtain information such as model operational status,
         inference performance metrics, and resource utilization in real time.


6.2.5. Security-supporting systems
The security requirements for the support system of the product include but are not limited
to the following:

    1)   Necessary tailoring must be performed so that no redundant components or
         network services are provided;

    2)   Security policies and log information must not be lost during system restart;

    3)   The product must contain no known medium- or high-risk security vulnerabilities;

    4)   The AI model runtime environment should be securely isolated from the business
         processing environment to prevent abnormalities in the model runtime environment
         from affecting the normal operation of business processing and to prevent
         unauthorized interference by business processing processes with the model runtime
         environment;

    5)   The AI model runtime environment must be configured in accordance with the
         principle of least privilege. Only system resources and network access permissions
         necessary for model inference must be enabled, and the file system access, network
         connection, and system call capabilities of the model runtime environment must be
         restricted;

    6)   The libraries and frameworks on which the AI model runtime environment depends
         must contain no known medium- or high-risk security vulnerabilities. An inventory
         of dependencies and vulnerability scanning mechanism must be established to
         regularly inspect and update components with potential security risks.
6.2.6. AI model security
The security requirements for AI models of the product include but are not limited to the
following:

    1)   The product must perform integrity verification of AI model files. Before a model is
         loaded, hash verification, digital signatures, or other means must be used to verify
         that the model files have not been tampered with, replaced, or corrupted, so as to
         prevent the loading of malicious models from rendering security policies ineffective;

    2)   The product must be capable of resisting adversarial example attacks. Anomaly
         detection and filtering must be performed on input data to identify and reject
         carefully crafted adversarial example inputs, thereby preventing attackers from
         misleading model decisions through minor perturbations to input data;

    3)   The product should be capable of resisting model extraction attacks and must take
         effective measures to prevent attackers from extracting model parameters, model
         architecture, or characteristics of training data through means such as issuing large
         numbers of queries to interfaces and observing output results. Such measures may
         include limiting the precision of confidence scores in model outputs and adding
         perturbations to outputs;

    4)   The product should establish security boundaries for AI model inference behavior,
         including restrictions on the data range of inference inputs, the types of decisions in
         inference outputs, and the frequency of inference invocations, so as to prevent
         models from being used for unintended purposes or in scenarios beyond their
         designed scope;

    5)   The product should establish an AI model security testing mechanism. Security
         performance testing must be conducted before model deployment and periodically
         during operation, including adversarial robustness testing, model extraction defense
         testing, and data poisoning defense testing, so as to ensure model reliability under
         security threat scenarios.


6.2.7. Data security and privacy protection
The data security and privacy protection requirements for the product include but are not
limited to the following:

    1)   Temporary data and intermediate results generated during AI model inference
         should be securely managed. Upon completion of inference, temporary data must
         be promptly cleared to prevent intermediate results from being accessed or used
         without authorization;

    2)   Storage protection should be provided for AI model training datasets and feature
         data. Measures such as encrypted storage and access control should be adopted to
         prevent data leakage, tampering, or unauthorized copying.
6.2.8. AI algorithm security
The AI algorithm security requirements for the product include but are not limited to the
following:

    1)   The product must establish human review or secondary confirmation mechanisms
         for significant decisions made by AI models. Where AI model inference results would
         lead to major changes to security policies or affect critical business operations,
         execution must proceed only after confirmation by an authorized administrator, so
         as to prevent uncontrollable risks arising from autonomous AI decision-making;

    2)   The product should conduct security assessments of the design and implementation
         of AI model algorithms, including algorithm security analysis, identification of
         potential risks, and detection of security defects, to ensure that the algorithms
         themselves contain no exploitable security vulnerabilities or backdoors;

    3)   The product should record and manage complete version information of AI models,
         including metadata such as model version numbers, training dataset versions,
         feature repository versions, training parameter configurations, and training time, to
         ensure model traceability and reproducibility;

    4)   The product may establish continuous monitoring and model degradation detection
         mechanisms for AI models, periodically evaluate model performance in real-world
         operating environments, and generate timely alerts and trigger model update
         processes when significant model performance degradation is detected.


6.3.     Centralized management requirements

6.3.1. Platform integration
    1)   The product must support standardized integration with unified firewall
         management platforms and integrated security management platforms, as well as
         centralized management, and must support unified management and control of
         multiple distributed devices to eliminate isolated silos of standalone device
         management;

    2)   The product must provide standardized external interfaces through standard
         protocols such as NETCONF, RESTful API, Syslog over TLS, and SNMPv3, to ensure
         compliant and stable interoperability with management platforms.


6.3.2. Centralized device management
    1)   The product must support centralized lifecycle management of devices by the
         management platform, including batch registration, remote operations and
         maintenance, firmware version upgrades, security signature database updates,
         centralized configuration backup, and one-click rollback;

    2)   The product must support normalized reporting to the management platform of
         data such as device operational status, computing power load, network-wide traffic
         logs, AI security attack events, and protection statistics metrics, so as to support
         unified monitoring, statistics, and analysis by the platform.


6.3.3. Centralized policy management and control
    1)   The product must support centralized orchestration and batch distribution of AI-
         specific security policies by the management platform, thereby uniformly
         implementing AI security protection capabilities such as prompt injection protection,
         sensitive content filtering, model access control, and rate limiting and blocking of
         abnormal traffic;

    2)   The product must support policy version management, conflict detection, staged
         rollout, and global withdrawal capabilities to ensure the consistency and
         effectiveness of protection policies for multi-node AI firewalls across the network.


6.3.4. Collaborative security response capabilities
    1)   The product must support receiving dynamic security response instructions issued
         by the security management platform and enable coordinated response across all
         domains to perform real-time blocking of malicious IP addresses, risky accounts,
         and anomalous model invocation behavior, thereby establishing a closed-loop
         coordinated security response system.

    2)   The product must support the use of threat information received from endpoint-
         side threat detection and response products, other firewalls, and other sources to
         perform collaborative security event analysis, reasoning, and protection.


6.4.     Performance requirements
The relevant requirements specified in 6.4 of GB/T 20281—2020 should apply.


6.5.     Security Assurance requirements
The relevant requirements specified in 6.5 of GB/T 20281—2020 should apply.
Appendix: AI Firewall Reference Deployment Architecture Examples

Deployment scenarios for AI firewalls include two typical use cases: data centers (including
general data centers, AI data centers, etc.) and campus branch interconnections. The
specific networking topology is shown in the figure below.

As shown from the typical networking topology, with the widespread deployment of AI
systems, typical east-west traffic interactions have emerged within data centers and
between campus branches, in addition to traditional north-south traffic. By deploying AI-
agent firewalls at the boundaries of network zones within data centers and at the
boundaries of enterprise campus branches, organizations can effectively isolate not only
north-south security risks but also east-west security risks (e.g., when an intelligent agent
calls resources within a domain), thereby enhancing the overall security posture.




        Figure 1. Reference Deployment Architecture of AI Firewalls in Data Centers




 Figure 2. Reference Deployment Architecture of AI Firewalls for Campus Branch Networks
