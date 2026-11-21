---
id: collect-261121-veille-ia/securite/post-quantum-readiness-the-vendor-questions-cisa-nsa-and-australia-s-acsc-say-to-ask-intentsec-core-
title: "Post-Quantum Readiness: The Vendor Questions CISA, NSA and Australia's ACSC Say to Ask | IntentSec Core Infrastructure"
domain: securite
role: reference
task: article
actors: []
dates: ["2026-11-05"]
keywords: ["questions", "post-quantum", "cryptography", "vendor", "most", "guidance", "acsc", "cisa"]
source: https://www.intentsec.com/blog/post-quantum-vendor-questions-cisa-nsa-acsc-guide
source_anchor: ""
source_lines: [1, 100]
section: "Securite"
---
Most organizations will not move to post-quantum cryptography by rewriting their own code. They will get there, or get stuck, through the cloud services, software, firewalls, identity systems and devices they buy. That is why cyber agencies in the US and Australia have published a list of questions to put to suppliers, and updated it on November 5, 2026.

The guidance, written by the Australian Signals Directorate's Australian Cyber Security Centre (ASD's ACSC) with the US Cybersecurity and Infrastructure Security Agency (CISA) and the National Security Agency (NSA), turns a vague worry about quantum computers into concrete procurement questions. This guide explains what the questions are, which ones matter most, and how to judge the answers.

## Key facts at a glance

- **Who wrote it:** ASD's ACSC, in partnership with**CISA and NSA** . It was first published on**July 16, 2026** and refreshed on**November 5, 2026** with more background and broader applicability.
- **What it covers:** vendor-neutral questions grouped into**five phases** of a post-quantum transition, plus a list of warning signs in vendor answers.
- **The key deadline:** vendors are asked whether products will be ready early enough to support customer rollout**before the end of 2030** , a milestone drawn from Australian guidance.
- **The threat:** a future**cryptographically relevant quantum computer (CRQC)** could break today's widely used public-key cryptography, so data captured now could be decrypted later.
- **A quick-reference list** accompanies the full publication for use in meetings and questionnaires.

## Who this guide is for

The agencies wrote the questions for **security leaders, procurement and vendor management teams, and technical staff** who assess third-party products. If you sign contracts for software as a service, managed services, identity platforms, backup services or network hardware, this applies to you.

It matters even more if you run equipment that is hard to update. The guidance singles out **operational technology, Internet of Things devices, building management systems, physical access control and legacy on-premises systems**, where cryptography is often embedded in firmware and tied to long asset lifecycles. In those cases a vendor may need to ship new hardware, not just a patch.

## Why vendors decide your post-quantum timeline

Post-quantum cryptography (PQC) means algorithms designed to resist attacks from both classical and quantum computers. The urgency comes from **"harvest now, decrypt later"**: an attacker can record encrypted traffic today and store it until a quantum computer can break the key exchange that protected it.

The problem for most organizations is that **vendors choose the cryptography**. The guidance notes that even an organization with strong in-house expertise and a refined plan can be held back by "weaknesses or delays in the supply chain." Engaging early helps avoid a late surprise, such as discovering that a critical product cannot support PQC in time.

## The five phases, and the questions that matter most

The questions follow ASD's **LATICE** framework: Locate, Assess, Triage, Implement, and Communicate and Educate. Here are the questions that tend to reveal the most.

### 1. Locate and inventory cryptographic dependencies

- **Do you maintain a cryptographic bill of materials (CBOM)** or equivalent inventory? A CBOM extends a software bill of materials by listing the algorithms, libraries, protocols and configurations in use.
- **Is any cryptography hardcoded, fixed or hardware-bound?** The ideal answer is no. Otherwise you want a list of affected components and a realistic fix, such as a firmware upgrade or replacement.
- **How is firmware authenticity verified, and can boot processes be updated to support PQC signature schemes?** Firmware signing is long-lived, and the guidance warns it may force device replacement if it was not designed to change.

### 2. Assess risk to individual systems

- **Which customer data flows rely on traditional asymmetric cryptography** , and for what: authentication, key establishment or integrity?
- **What does the product assume about how long your data lives?** Data that must stay confidential for many years is most exposed to harvest-now attacks.
- **Can you choose stronger settings today?** The agencies say larger traditional keys offer only "marginal, short-term risk reduction" and are**not a substitute** for moving to PQC.

### 3. Triage and prioritise

- **Which products will be PQC-ready early enough to support customer rollout before the end of 2030?** Look for product-specific, versioned commitments, not general promises.
- **Which components need hardware replacement or re-architecture?** These are long-lead items that drive cost and schedule.
- **If you support hybrid modes, when will you move to PQC-only?** The guidance treats post-quantum/traditional (PQ/T) hybrid schemes as a**temporary** measure and asks for a documented plan to retire the classical part.

### 4. Implement PQC algorithms

- **Which standards-based PQC algorithms will you support, and when will they be generally available?**
- **Is the implementation production-ready** , with support agreements, or still a preview?
- **Is PQC included in base licensing?** The agencies warn against security controls being "paywalled," which could leave some customers less protected than others.

### 5. Communicate and educate

- **How will you notify customers of cryptographic changes** , and do you provide transition guides?
- **What is your deprecation timeline** for traditional cryptography, including any interim hybrid use?
- **Are support and engineering staff trained** on PQC, with updated runbooks?

## How to judge the answers

The guidance lists common warning signs in vendor responses. None automatically means unacceptable risk, but each deserves a follow-up:

| Warning sign | What to ask next | 
|---|---|
| Limited visibility of cryptographic use | Request a CBOM or a written plan to produce one | 
| General assurances only | Ask for product names, versions and dates | 
| Unclear or deferred timelines | Ask how the roadmap supports rollout before 2030 | 
| Proprietary or opaque cryptography | Ask which standard libraries are used and how they are validated | 
| PQC as an optional or premium feature | Ask for PQC in base licensing terms | 
| Heavy reliance on compensating controls | Ask what replaces them once PQC is available | 
| No governance or ownership | Ask which executive owns the PQC program | 

The agencies also stress that some vendors will not have detailed answers yet. A supplier's **willingness to engage transparently**, explain constraints and show progress is itself a useful sign of maturity.

## What a concrete answer can look like

Recent cloud announcements show the level of detail worth asking for. On November 9, Microsoft said **Azure App Service and Azure Functions** are beginning a phased rollout of the TLS 1.3 hybrid key exchange group **X25519MLKEM768**, which combines classical X25519 with NIST-standardized ML-KEM-768. Microsoft spelled out the scope: inbound connections to apps on App Service infrastructure, with **no customer setting** to enable or require it, and no coverage yet for the internal front-end-to-worker connection or for Functions on Azure Container Apps.

Days earlier, on November 6, Microsoft said **Azure Front Door** now negotiates hybrid post-quantum groups, including SecP256r1MLKEM768 and X25519MLKEM768, **by default** for client connections, with classical fallback.

Both are useful answers to the guidance's questions on scope, enablement and hybrid use. They also show why the follow-up matters: both deployments are hybrid, so the next question is the one the agencies recommend, which is when and how the vendor plans to move to PQC-only.

## Fitting the questions into your process

The guidance suggests using the questions in **procurement, contract renewals and ongoing vendor assurance**, and adjusting depth to risk. In practice:

1. **Start with your highest-risk suppliers:** those handling long-lived sensitive data, identity and authentication, or hard-to-update devices.
2. **Add the questions to security questionnaires and RFPs** , and record answers so you can track whether roadmaps slip.
3. **Write commitments into contracts at renewal** , such as notification of cryptographic changes and PQC availability without extra fees.
4. **Revisit answers yearly.** A vendor that had no plan in 2026 should have dated milestones by 2027.

Regulators elsewhere are pushing in the same direction. In Hong Kong, the Hong Kong Monetary Authority and the Hong Kong University of Science and Technology launched a PQC toolkit for banks on November 2, after the regulator's Quantum Preparedness Index gave the sector an initial score of **2.3 out of 10**. The aim is full readiness, a score of 10, by 2030.

## Takeaways

1. **List the suppliers that control your cryptography** and rank them by data sensitivity, exposure and how hard their products are to update.
2. **Ask for evidence, not reassurance:** a CBOM, product-specific dates and a plan for any hardware-bound cryptography.
3. **Treat hybrid support as a first step.** Ask every vendor offering hybrid TLS when it will offer PQC-only.
4. **Push for PQC in base licensing** and for advance notice of cryptographic changes in contracts.
5. **Use the 2030 milestone as a planning anchor** , while following the timelines that apply in your own country and sector.
