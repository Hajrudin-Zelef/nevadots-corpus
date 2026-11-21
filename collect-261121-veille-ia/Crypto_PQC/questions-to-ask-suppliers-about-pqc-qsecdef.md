---
id: collect-261121-veille-ia/crypto-pqc/questions-to-ask-suppliers-about-pqc-qsecdef
title: "Questions to Ask Suppliers About PQC | QSECDEF"
domain: crypto-pqc
role: reference
task: article
actors: []
dates: ["2026-11-05"]
keywords: ["questions", "nist", "cryptography", "agencies", "suppliers", "good", "vendor", "cisa"]
source: https://www.quantumdefenceevent.com/insights/questions-to-ask-suppliers-about-pqc/
source_anchor: ""
source_lines: [1, 50]
section: "Crypto_PQC"
---
For most organisations, the hardest part of moving to post-quantum cryptography (PQC) will not happen inside their own walls. It will happen inside the products and services they buy. Your firewall, your cloud platform, your payment terminals and your building controls all contain cryptography that someone else wrote and only someone else can change.

That is why three allied cyber agencies have now published the questions buyers should put to suppliers. This article explains what they released and why it matters, then offers our own plain-English questions, with what a good answer looks like. If you want a full method for scoring suppliers, our guide to evaluating vendor quantum readiness goes deeper.

## What the three agencies published

The US Cybersecurity and Infrastructure Security Agency (CISA), Australia's ASD's ACSC and the US National Security Agency (NSA) "released joint guidance and an accompanying quick-reference list to help organizations assess vendor readiness" for post-quantum cryptography. CISA lists the publication date as 5 November 2026.<sup>[1]</sup> The full guidance sits on the Australian Cyber Security Centre website.

The agencies say the questions help organisations "determine whether a vendor understands quantum-related risks, maintains transparency over its cryptographic implementations, and supports a secure, cost-effective and timely transition to PQC."<sup>[1]</sup> According to reporting on the release, the questions are vendor-neutral, meant for procurement, contract renewals and ongoing vendor assurance, and grouped into five phases: locating cryptography, assessing risk, triaging systems, implementing PQC, and communicating with vendors and educating stakeholders.<sup>[2]</sup>

This is not entirely new. Australia's ASD first published guidance under the same title in July 2026.<sup>[3]</sup> And back in 2023, CISA, NSA and NIST told organisations to "ask vendors for lists of embedded cryptography within their products."<sup>[4]</sup> What has changed is that three agencies from two countries now put their names to one shared set of questions. In our view, that turns quantum readiness from a specialist topic into a normal line in a supplier questionnaire.

## Why your suppliers set your timeline

The agencies put it plainly: "effective PQC transition depends on vendor readiness, and early, structured engagement with these vendors is critical to planning efforts."<sup>[1]</sup> They point to systems that are difficult to update and tied to long asset lifecycles, such as industrial control systems and Internet of Things devices.<sup>[1]</sup>

The clock is set by NIST. Its final transition report says NIST "will deprecate quantum-vulnerable public-key cryptography after 2030 and disallow its use under NIST standards and guidelines after 2035."<sup>[5]</sup> It also notes that moving from a new standard to full use "can take 10 to 20 years."<sup>[5]</sup> For hardware, NIST is blunter: "it is a high priority for vendors to initiate the integration of PQC support immediately."<sup>[5]</sup> Our NIST IR 8547 timeline guide explains those dates in full.

## Ten questions in plain English, and what good looks like

What follows is **QSECDEF's own set of plain-English questions**, arranged under the five phases reported for the joint guidance. It is not a copy of the agencies' wording, and the official list is the one to use in a formal process. The "good answer" notes are our view, built on the CISA, NIST and GSA sources cited.

1. **Can you list the cryptography in your product?** Good answer: a written list, ideally a cryptographic bill of materials (CBOM), covering embedded components, as CISA, NSA and NIST asked for in 2023.<sup>[4]</sup> Our article on what regulators expect a CBOM to show covers the format.
2. **Which of our data and functions depend on quantum-vulnerable algorithms?** Good answer: specific features named, such as logins, signed updates or stored data, not "it depends".
3. **How long does your product assume our data must stay secret?** Good answer: they understand that data stolen today could be read later, and can tell you which settings protect long-lived data.
4. **Is any cryptography fixed in hardware or firmware?** Good answer: a clear yes or no, and if yes, whether an update can fix it or a replacement is needed. NIST points to the long lead times for hardware.<sup>[5]</sup>
5. **Which products and versions are in scope, and which come first?** Good answer: a ranked list that puts high-impact and long-lived products first.
6. **Which PQC algorithms will you support, and in which release?** Good answer: NIST-standard algorithms and a dated release. The 2023 CISA, NSA and NIST factsheet says strong roadmaps chart "timelines for testing PQC algorithms and integration into products."<sup>[4]</sup>
7. **Is your implementation validated?** Good answer: a validation certificate, or a clear path to one. The US GSA says vendors without one "should be able to demonstrate how they meet the requirements included in the certification process."<sup>[7]</sup>
8. **Can you switch algorithms again later?** Good answer: yes, by configuration or update. GSA asks buyers to "Ensure the vendor will be crypto-agile".<sup>[7]</sup>
9. **What will it cost us?** Good answer: PQC included in normal updates, or a clear statement of any charge. The same factsheet says roadmaps should include "the expected cost associated with migration to PQC."<sup>[4]</sup>
10. **How will you tell us about changes, and when will the old algorithms be switched off?** Good answer: named contacts, migration guides and dated deprecation steps that sit inside NIST's 2030 and 2035 window.<sup>[5]</sup>

## Answers that should prompt a follow-up

In our view, a vague answer is usually a sign that the work has not started, not a sign of bad faith. These deserve a second question:

- A promise to wait for the standards. NIST has published final PQC standards, so ask for dates.
- No inventory, or one that leaves out firmware and third-party libraries.
- A roadmap with no release numbers or no dates.
- PQC offered only as a paid add-on, with no word on cost in the contract.
- Nobody named as owner of the transition.

## Making it a normal procurement checkbox

Some buying guidance already exists. For product types where PQC-capable options are widely available, CISA says "organizations should acquire only PQC-capable products when planning acquisitions and procuring products in these categories." Its list includes cloud services, collaboration software, web software and endpoint security.<sup>[6]</sup> The 2023 factsheet also urged buyers to write PQC into contracts, so that new products arrive "with PQC built-in, and older products will be upgraded with PQC to meet transition timelines."<sup>[4]</sup>

Our view is that the organisations that find this easiest will do three things. They will add a short PQC section to every new tender and renewal. They will start with suppliers whose products are hardest to change. And they will make one person responsible for chasing the answers. That person is usually a programme manager, not a cryptographer.

If you will lead that work, the Certified PQC Programme Manager course covers planning, supplier engagement and running a migration programme. Organisations adopting quantum-safe technology can join practitioners doing the same work through Industry membership for end users, while suppliers answering these questions can explore Industry membership for vendors or QSECDEF company membership. To see who offers quantum security products and assessments, browse our directory of quantum security vendors, and for hardware buyers, our HSM procurement criteria. The member organisations below include advisers and suppliers already working on these questions.
