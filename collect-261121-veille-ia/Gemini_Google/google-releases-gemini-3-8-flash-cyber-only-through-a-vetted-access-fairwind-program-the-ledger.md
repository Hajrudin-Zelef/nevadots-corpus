---
id: collect-261121-veille-ia/gemini-google/google-releases-gemini-3-8-flash-cyber-only-through-a-vetted-access-fairwind-program-the-ledger
title: "Google Releases Gemini 3.8 Flash Cyber Only Through a Vetted-Access Fairwind Program — The Ledger"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-09-02"]
keywords: ["google", "cyber", "flash", "model", "program", "gemini", "fairwind", "reported"]
source: https://www.the-ledger.net/events/google-gemini-flash-cyber-fairwind-program
source_anchor: ""
source_lines: [1, 32]
section: "Gemini_Google"
---
## Summary

On September 2, 2026, Google released Gemini 3.8 Flash, its third Flash-tier model in six weeks, alongside Gemini 3.8 Flash Cyber, a variant built for vulnerability discovery and automated patching. Google said the cyber model "ships with a more permissive set of mitigations for cybersecurity" under its Frontier Safety Framework and, for that reason, is available only through a new Fairwind Program limited to governments and national cyber authorities, critical-infrastructure operators and core technology platforms. Applicant organizations are background-checked and must agree to operational terms on who inside the organization may use the model and how. Google said the program already had more than 650 participating partners at launch.

## What Happened

Gemini 3.8 Flash arrived three weeks after 3.7 Flash, at the same introductory price of $0.75 per million input tokens and $3.75 per million output tokens. The announcement, by Tulsee Doshi and Google DeepMind security lead Raluca Ada Popa, disclosed that the introductory price expires on December 31, 2026, after which the rates double to $1.50 and $7.50. Google reported 54.9% on HLE-Verified and gains over 3.7 Flash on DeepSWE v1.1, Vals Finance Agent V2, Harvey's legal agent benchmark and Gray Swan's prompt-injection benchmark. Simon Willison, who added the model to his llm-gemini plugin the same day, noted that a test HTML-generation task took 13 seconds and cost 1.8 cents, and that Flash Cyber was limited to "trusted defenders."

For the cyber variant, Google reported a success rate above 70% on an internal benchmark requiring the model to find vulnerabilities in codebases spanning 20 programming languages, and 47.2% pass@1 on Collinear's CWE-Bench patching benchmark, which it said put the model on the cost-performance Pareto frontier against a leading model at 47.8%. The model page lists 86.2% pass@1 on CyberGym, against 77.5% for the earlier Gemini 3.5 Flash Cyber. Google said it had "prioritized [vulnerability fixing] over offensive capabilities like exploitation," and offered three internal data points: the Chrome Security team reported 2.6 times more correct patches to Chrome vulnerabilities than "the best commercial models that are much larger"; Wiz reported 7.5 to 9.7 points higher recall on an internal penetration-testing benchmark at 2.3 to 5.2 times lower cost; and Google's Cloud Vulnerability Research team said the model found "a critical foundational vulnerability" in under two hours. None of these figures has been independently reproduced.

The Fairwind Program was announced the same day by Four Flynn, Google's vice president for security and privacy, as "a limited access program for governments and trusted partners to use our most advanced cyber defense capabilities." Google DeepMind's program page describes managed access under which partners "may only grant Gemini 3.8 Flash Cyber access to internal cybersecurity, incident response, or penetration testing teams," must track employee access and use, must implement user-level authentication and phishing-resistant multi-factor authentication, must confine use to authorized threat simulation, reverse engineering and malware analysis for defensive and research purposes, and may not share, redistribute or sell model access. Google says it "conduct[s] background checks on organizations that apply, to verify security history and analyze their record of ethical operations." The program bundles the model with CodeMender, Google's agentic code-repair harness. Flynn's post also reported that Google.org's cumulative cybersecurity funding now exceeds $100 million, including $36 million to 35 cyber clinics serving more than 1,250 US hospitals, school districts and municipal utilities.

The model is a successor to Gemini 3.5 Flash Cyber, released on July 21, 2026, which TechCrunch reported was "exclusively available to governments and trusted partners as part of a limited access pilot program." Fairwind formalizes that pilot into a named program with published eligibility categories, diligence and operational terms.

## Why It Matters

This is the second frontier lab to publish a standing, vetted-access regime for a cyber-specialized model with loosened refusals, after OpenAI expanded its Trusted Access for Cyber program around GPT-5.4-Cyber in April 2026. The two designs differ in where trust is anchored. OpenAI's program verifies individual defenders in tiers; Google's vets the organization, then binds it to internal access rules and usage tracking. Together with OpenAI's September 1 Critical cyber designation for Astra and its Daybreak defender programs, the pattern across labs is now consistent: cyber capability is treated as a gated tier rather than a general release, and the gate is a contractual and identity-verification layer run by the vendor.

What the "more permissive set of mitigations" removes is not specified, and Google published no Frontier Safety Framework capability-level determination for the model in the launch materials. The vulnerability-discovery and patching claims rest on internal benchmarks and partner testimonials, and no external evaluation has been published. Whether vendor-run vetting scales to hundreds of organizations without leakage, and whether the "trusted defender" category holds when the same models are sold through cloud marketplaces, are questions the launch does not answer.

## § How to read the metadata

- Landmark
- Fundamentally alters the trajectory; 2–5 per year.
- Major
- Meaningfully shifts the landscape; 2–4 per month.
- Notable
- Worth documenting; significance can be upgraded later.
- Confidence
- High = primary sources corroborate. Medium = credible secondary only. Low = provisional. Disputed = credible sources disagree.
- Contestation
- Uncontested = no formal challenge. Contested = at least one challenge filed and not resolved by a correction. Corrected = a challenge was upheld and the entry corrected; see its revision history. Superseded = replaced by a later entry. Unresolved = dispute still open.
