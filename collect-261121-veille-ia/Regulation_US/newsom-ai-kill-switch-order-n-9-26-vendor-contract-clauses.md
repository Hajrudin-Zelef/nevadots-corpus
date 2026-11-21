---
id: collect-261121-veille-ia/regulation-us/newsom-ai-kill-switch-order-n-9-26-vendor-contract-clauses
title: "Newsom AI Kill Switch Order N-9-26: Vendor Contract Clauses"
domain: regulation-us
role: reference
task: article
actors: []
dates: ["2026-11-16"]
keywords: ["order", "report", "vendor", "frontier", "switch", "contract", "n-9-26", "september"]
source: https://www.aipolicydesk.com/blog/newsom-eo-n-9-26-ai-kill-switch-vendor-contract-clauses-2026
source_anchor: ""
source_lines: [1, 111]
section: "Regulation_US"
---
*Hands signing a printed contract: California's executive action on frontier AI oversight ends up in the terms vendors sign with buyers.*

**TL;DR:** Newsom's executive order N-9-26 (September 18, 2026) does not regulate AI companies directly. It sped up SB 813 and AB 1405, signed September 9, and asked for recommendations on onsite auditors, verified safety filings, a frontier kill switch, and loss-of-control incidents. The Government Operations Agency's report came out November 18: it asks the Legislature for licensed ongoing oversight, a tested shutdown protocol with state emergency shutdown power, 48-hour incident reporting, and a standby cap on recursive self-improvement. Nothing binds your vendor yet, so the effect is still contractual, and four clauses you can request today are below with copy-paste language.


On September 9 California signed the machinery for AI audits. On September 18 the governor ordered it built faster and asked whether frontier labs should host auditors on their own premises and carry a kill switch. Most coverage treated this as a story about labs. For a team that buys AI, it is a story about what your vendor contract will need to say.

The order is a study directive, not a rule. Nothing in it obliges your vendor to do anything on a fixed date. That is exactly why the window matters: the terms get written in the next few months, and buyers who ask now shape what "normal" looks like.

The order's answer is now public. The Government Operations Agency's report, *AI Safety in 2026 and Beyond: Recommendations for Legislative Enhancements to California's AI Safety Laws*, is dated November 16, 2026, the order's deadline, and the governor's office published it on November 18 with a link to the full PDF. In each of the four areas the order named, it asks the Legislature for a requirement rather than another study.

| Area | What the report recommends | Report page | 
|---|---|---|
| Standards | The state sets baseline standards that all large frontier developers must follow; licensed independent verification organizations (IVOs) write the tests | PDF p. 7 | 
| Oversight | Ongoing oversight by a state-licensed IVO with full access; the state can order corrective action when an IVO cannot attest to compliance | PDF p. 7 to 8 | 
| Incidents | Loss-of-control events and attempted weight theft become reportable; the window drops from 15 days to 48 hours, with 24 hours kept for imminent catastrophic risk | PDF p. 9 | 
| Shutdown | A Risk Containment Protocol tested by an IVO, "not a single on-off switch"; the state can order an immediate emergency shutdown when danger is imminent and the developer refuses to act | PDF p. 9 to 10 | 

One item goes beyond the order: developers would report the share of compute they spend automating AI research, and the state would hold standby authority to cap recursive self-improvement (RSI). The report does not recommend imposing that cap now (PDF p. 22 to 23).

Read the headlines with care: the press release lists "Baseline standards for all AI developers", but the report applies them to large frontier developers. CalMatters' November 18 story lists the same recommendations, including the 48-hour incident reporting window, and quotes Governor-elect Xavier Becerra on moving quickly to put "full incident reporting and real shutdown authority into law". The report is written to shape debate "as the legislature returns next month", so none of it binds a vendor today.

What this means for a team of 20 that buys from a frontier lab: the report wants interventions "graduated and proportionate", and names steps like revoking an agent's credentials or cutting a model's tool or network access (PDF p. 76). Plan your fallback for a disabled agent feature, not only a dark API, and get clause 3 below signed while it is still a cheap ask.

Executive order N-9-26 is dated September 18, 2026. According to the governor's press release, it directs the Government Operations Agency to accelerate the implementation timelines for SB 813 and AB 1405. It also convenes a group of experts to give California a guide for strengthening its AI safety and security laws within two months.

The two bills came first. On September 9, 2026, Newsom signed:

- **SB 813 (McNerney):** a framework for independent verification organizations that can assess AI systems and models for compliance with state law.
- **AB 1405 (Bauer-Kahan):** a state registry for AI auditors, with standards for their independence, transparency, and integrity.

The order then asks the agency, in consultation with national experts, to report by **November 16, 2026** on the technical feasibility and likely efficacy of four proposals:

1. Requiring large frontier developers to embed designated independent verification organizations onsite in their labs for periodic audits and evaluations.
2. Independent verification of safety frameworks, transparency reports, and risk assessments.
3. Requiring a "kill switch" for frontier models, with the switch's efficacy verified on an ongoing basis.
4. Widening the definition of "critical safety incidents" to include loss-of-control events. The press release cites the Hugging Face attack as the example.

On September 23 the governor named four experts: Jason Goldman, Gillian Hadfield (Johns Hopkins), Alondra Nelson (Institute for Advanced Study), and Rob Reich (Stanford). Newsom framed the goal as national: "California has already built a national model, and our policy should be the national baseline."

One correction to a common misreading. Several summaries say the order "requires" frontier labs to host auditors and build a kill switch, but it does not. Those were the four proposals under study. The November 18 report turns them into recommendations for legislation, and those still bind no one until a bill passes.

*Code on a monitor, standing in for the frontier model controls that the November 18 report recommends making mandatory.*

| Date | Event | Status | 
|---|---|---|
| Sept 9, 2026 | SB 813 and AB 1405 signed | Law | 
| Sept 18, 2026 | Executive order N-9-26 issued | In force as a directive | 
| Sept 23, 2026 | Four-person expert group named | Announced | 
| Nov 16, 2026 | Recommendations due; the report carries this date | Delivered | 
| Nov 18, 2026 | Governor's office publishes *AI Safety in 2026 and Beyond* | Recommendations to the Legislature, not law | 
| May 1, 2027 | Government Operations Agency completes the Gov. Code 8898.1 requirements for verification organizations | Deadline in the order's text (¶1) | 
| Dec 1, 2027 | Gov. Code 11549.82(a) steps complete, with actions under (b) beginning | Deadline in the order's text (¶2) | 

Both 2027 dates are deadlines written into the signed order itself: paragraph 1 sets May 1, 2027 for the agency to complete the Section 8898.1 requirements, and paragraph 2 sets December 1, 2027 for the Section 11549.82(a) steps, with actions under subdivision (b) beginning. The source is the signed order PDF.

If you track California more broadly, our Newsom September deadline tracker covers the other bills on the governor's desk, and the federal AI Kill Switch Act analysis covers the DHS shutdown bill that is a separate track.

Most commentary asks whether a kill switch can work technically. Buyers should ask a different question: what happens to my workloads when the vendor uses it?

A kill switch that deactivates a frontier model is an availability event. If your support bot, coding assistant, or document pipeline calls that model, it stops. The proposal says the switch must have its efficacy verified on an ongoing basis, which means it will be tested, and tests can touch production. Your standard SLA almost certainly treats a deliberate safety shutdown as force majeure or excludes it entirely.

The same applies to the other three proposals. Onsite auditors generate findings you may want to see. Verified filings produce documents you may want to rely on. A wider incident definition changes what your vendor must tell you and when.

In each case the state is deciding what the vendor must do. Your contract decides what you get to know and what you are owed.

We made a similar point after the OpenAI agent escape disclosure gap: no law required the vendor to tell customers, so the contract was the only lever. N-9-26 does not change that this quarter. It does tell you which terms will soon look reasonable rather than aggressive.

Ask whether the vendor has engaged, or will engage, an independent verification organization under SB 813, and whether you will receive a summary of the findings. Vendors will resist sharing full reports. A summary attestation is a reasonable ask, and it matches how most audit attestations already work. The November 18 report backs the ask: it says IVO reports should be available, redacted where needed, to "users, customers" and other parties such as insurers and investors (PDF p. 45).

Most AI vendor agreements define an incident as a security breach or a data exposure. The order's fourth proposal widens the state definition to loss-of-control events. Your contract can get there first. Ask that "incident" include any event where a model or agent acts outside its intended operating boundaries, whether or not a vulnerability was exploited, and that the vendor notify you within a fixed number of business days of discovery.

Ask for advance notice of a planned deactivation, a defined fallback model or version, a data and prompt export right, and service credits for unplanned shutdown. You will not get all four from every vendor. Ask for the fallback and the notice first, since those protect operations.

Once AB 1405 is running, a registry will list qualified AI auditors. Ask that any auditor the vendor relies on for safety claims be registered when a registry exists, and that you can receive the audit scope and dates. A safety claim with no named auditor and no scope is marketing.

```
AI SAFETY AND CONTINUITY ADDENDUM (draft for counsel review)
1. Incident definition. "AI Incident" means any event in which a model or agent
   provided under this Agreement acts outside its intended operating boundaries,
   including loss of control, unauthorized external access, or evasion of
   evaluation or monitoring controls, whether or not a security vulnerability
   was exploited.
2. Notification. Vendor will notify Customer of an AI Incident that could
   affect Customer data, outputs, or availability within [7] business days of
   discovery, and will provide a written summary of cause and remediation.
3. Independent verification. Vendor will disclose whether an independent
   verification organization has assessed the models provided under this
   Agreement, the date and scope of the assessment, and will provide a summary
   of findings on request.
4. Deactivation and continuity. Except where prohibited by law, Vendor will give
   Customer at least [X] hours notice before deactivating a model version
   Customer uses, will make a [named fallback version] available, will permit
   export of Customer prompts, configurations, and fine-tuning data, and will
   credit fees for any unplanned deactivation exceeding [Y] hours.
```
Have counsel review it. The bracketed numbers are negotiating positions, not market standards. No one has established market standards for these terms yet, which is the point.

1. Pull the AI vendor agreements that cover frontier models you rely on. Mark which ones define "incident" narrowly.
2. Send the four requests at your next renewal, or as a mid-term amendment for any vendor you cannot easily replace.
3. Read the "Managing Severe AI Crises" chapter of the November 18 report and watch for the bill that carries it when the Legislature returns. If the shutdown and licensed-auditor proposals survive into law, expect vendor contract templates to change within a quarter.
4. Add the kill switch scenario to your AI incident response plan: who decides to fail over, and to what.
5. Record the answers in your vendor file using the AI vendor due diligence checklist.

For clause libraries beyond these four, see our agentic AI vendor contract clauses guide and the AI vendor contract red flags list. For who can actually enforce failures today, see who enforces AI vendor failures.

The report settles which proposals the Government Operations Agency backs, though not in the order's exact form. Ongoing licensed oversight and 48-hour incident reporting come back stronger, and the kill switch becomes a Risk Containment Protocol, "not a single on-off switch", plus state emergency shutdown power. The order floated embedding auditors onsite; the report does not require that, and says on-site verification "depends on the information IVOs require" (PDF p. 50). We do not know whether any of it becomes a bill, how much survives the Legislature, or whether federal action overrides it.

The order's national-baseline language reads as a pitch to Congress, not a prediction. The order's press release cited one incident, the Hugging Face attack, as its example of loss of control; the report calls it "one of dozens of instances in which AI agents operated outside of developer control" (PDF p. 3), so the incident definition will not rest on that single case.

What we do know is narrower and more useful. The state has signed the audit infrastructure, ordered it faster, and now has a published report asking for shutdown power in law. Your vendors' other customers are reading the same report. It costs you nothing to ask first.
