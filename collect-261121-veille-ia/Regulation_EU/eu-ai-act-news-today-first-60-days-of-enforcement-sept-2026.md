---
id: collect-261121-veille-ia/regulation-eu/eu-ai-act-news-today-first-60-days-of-enforcement-sept-2026
title: "EU AI Act News Today: First 60 Days of Enforcement (Sept 2026)"
domain: regulation-eu
role: reference
task: article
actors: []
dates: []
keywords: ["enforcement", "first", "gpai", "news", "since", "office", "transparency", "model"]
source: https://aiact-navigator.com/blog/eu-ai-act-news-enforcement-first-60-days-2026
source_anchor: ""
source_lines: [1, 104]
section: "Regulation_EU"
---
# EU AI Act News: What the First 60 Days of Enforcement Actually Produced

*Last updated: 28 September 2026. This is a news digest, not legal advice.*

If you search for "EU AI Act news today", you mostly find two things: timeline explainers and predictions. This roundup is different. It covers what has **actually happened** since 2 August 2026, the date the AI Office's powers over general-purpose AI (GPAI) models and the Article 50 transparency obligations started to apply, and separates binding law from guidance and from proposals.

For the full re-baselined timeline after the Digital Omnibus (Regulation (EU) 2026/1744), see our earlier post, *EU AI Act Implementation Status 2026: The Digital Omnibus Is Now Law*. Quick reminder of where things stand:

| Obligation | Status on 28 Sept 2026 | 
|---|---|
| Article 5 prohibitions (original list) | Applying since 2 Feb 2025 | 
| GPAI model obligations | Applying since 2 Aug 2025; AI Office enforcement powers since 2 Aug 2026 | 
| Article 50 transparency | Applying since 2 Aug 2026 (legacy generative systems: marking by 2 Dec 2026) | 
| New prohibited-practice dates | 2 Dec 2026 | 
| Annex III high-risk systems | 2 Dec 2027 | 
| Annex I product-embedded high-risk systems | 2 Aug 2028 | 

## 1. The AI Office sent its first requests for information to GPAI providers

**What happened:** Once its enforcement powers over GPAI model providers entered into application on 2 August, the Commission followed "promptly" with a first round of requests for information to AI companies, covering both safety and security and copyright-related obligations, according to CDT Europe's September AI Bulletin.

Around the same time, the AI Accountability Lab reviewed the public training-data summaries that GPAI providers must publish. Its "enforcement day" analysis assessed 39 summaries and reported that 20 were still missing (linked via CDT Europe).

**What it means for you:** If you are a GPAI model provider, a request for information is the realistic first enforcement touchpoint, not a fine. Have your technical documentation, copyright policy and training-data summary ready to hand over. If you are a downstream developer building on a third-party model, ask your provider whether they have received a request and whether their public summary is published. A missing summary is now a visible compliance gap.

## 2. The transparency Code of Practice passed 200 signatories

**What happened:** Article 50 transparency duties for chatbots, AI-generated content and deepfakes have applied since 2 August 2026. The Commission's Code of Practice on Transparency of AI-generated Content is voluntary, but it is currently the only EU-wide framework the Commission has endorsed for demonstrating compliance. CDT Europe reported 190 signatories; by the time Stephenson Harwood's September Neural Network went to press, the figure was 234, including Anthropic, Google, Meta, Microsoft, Mistral and OpenAI.

Providers are converging on a layered approach. Stephenson Harwood notes that no single technique currently satisfies every requirement, and that combining metadata, watermarking and content provenance (typically C2PA) is the interim industry standard.

**What it means for you:** Generative systems already on the market before 2 August 2026 have until **2 December 2026** to implement machine-readable marking. That is roughly nine weeks away. If you deploy a third-party model, check which marking layers your provider actually applies to the output formats you use (text, image, audio, video). Text remains the weakest link.

## 3. The Commission launched an AI Act complaints tool

**What happened:** The Commission published an AI Act complaints tool that lets individuals and organizations submit complaints to the AI Office about alleged infringements that fall under the AI Office's exclusive competence, which in practice means GPAI models.

**What it means for you:** Complaints are a classic enforcement accelerator under the GDPR, and the same dynamic is likely here. Civil-society organizations and competitors now have a formal channel. For matters outside the AI Office's competence, complaints go to national market surveillance authorities, whose designation is still uneven across member states.

## 4. Deepfake enforcement is arriving through the GDPR first

**What happened:** On 23 July 2026 the Italian data protection authority (Garante) issued a warning against broadcaster R.T.I. for airing satirical AI-generated deepfakes of well-known personalities, including a journalist shown commenting in his usual studio. The Garante found the videos were not adequately marked as AI-generated, breaching the GDPR's lawfulness, fairness and transparency principle. A verbal disclaimer was not enough, because viewers could tune in after it was given (Stephenson Harwood).

**What it means for you:** This is not an AI Act decision, but it lines up closely with the Article 50(4) deepfake disclosure logic and the Commission's guidance: the test is objective (no intent to deceive is needed) and must account for the realistic audience. If you publish synthetic media featuring real people, disclosure has to persist for the whole exposure, not appear once.

## 5. Automated decisions: Uber fined around EUR 825 million

**What happened:** The Dutch data protection authority (Autoriteit Persoonsgegevens) fined Uber nearly EUR 825 million for fully automated decisions to deactivate drivers' accounts based on algorithms tracking driver behavior and customer reviews, in violation of the GDPR.

**What it means for you:** The AI Act's high-risk rules for employment and worker management do not apply until December 2027. The GDPR's rules on automated decision-making apply today, and regulators are using them at scale. If you run algorithmic management, scoring or account-termination systems, the human-review and transparency controls you would build for Article 14 and Article 26 of the AI Act are the same ones that protect you under GDPR Article 22 now.

## 6. New rules around chatbots and minors are in the pipeline

**What happened:** The Commission designated ChatGPT as a very large online search engine under the Digital Services Act, bringing systemic-risk assessment duties. Separately, it proposed an "EU KIDS Act" with explicit obligations for AI companions and conversational chatbots interacting with minors, including bans on addictive design and limits on reusing information from a minor's earlier interactions (CDT Europe).

**What it means for you:** The KIDS Act is a **proposal**, not law. But if your product is a consumer chatbot, the direction of travel is clear: age-aware design, memory limits and risk testing for minors. Build them into your roadmap now rather than retrofitting later.

## 7. The Digital Omnibus fight has moved to the GDPR

**What happened:** The AI-side Omnibus changes are law. The GDPR-side negotiations resumed under the Irish Council presidency, and a leaked Council compromise would allow processing personal data for developing and operating AI systems under an existing legal basis with limited safeguards. noyb, BEUC and other civil-society groups have pushed back (CDT Europe).

**What it means for you:** Do not plan on a relaxed legal basis for AI training data. Nothing is agreed, and Parliament amendments range from deleting the changes entirely to broadening them. Keep documenting your legitimate-interest assessments under current law.

## 8. Politics: frontier AI security moved to the top of the agenda

**What happened:** In her September State of the Union address, Commission President von der Leyen set out industrial AI priorities in health, transport, agri-food, advanced manufacturing, defense and space, with initiatives to be announced in November. She also addressed frontier-AI security and announced a dialogue with frontier labs. Eight member states then endorsed a call for control of frontier AI models on the sidelines of the UN General Assembly (CDT Europe).

In the UK, Parliament's Joint Committee on Human Rights published a report on 14 September 2026 calling for a dedicated AI bill and a single statutory AI regulator (Stephenson Harwood).

**What it means for you:** Expect GPAI models with systemic risk to face the most scrutiny first. For most deployers, the practical impact is indirect: stronger security and evaluation demands will flow down through provider contracts.

## 9. Adjacent law: Cyber Resilience Act reporting went live

On 11 September 2026, manufacturers of products with digital elements became obliged to report actively exploited vulnerabilities and severe incidents through ENISA's Single Reporting Platform. Most AI-enabled software products are in scope. We cover the AI Act and CRA overlap in a separate post this week.

## What has *not* happened (yet)

Some sources claim a coordinated wave of AI Act inspections by national market surveillance authorities began in September. We have not been able to verify this from any official source, so treat it as unconfirmed. There have also been **no published AI Act fines** so far. The high-risk regime is still more than a year away.

## What to watch in Q4 2026

- **2 December 2026:** machine-readable marking deadline for legacy generative systems, and the new prohibited-practice dates.
- **November 2026:** the Commission's sector AI initiatives flagged in the State of the Union.
- **GDPR Omnibus:** whether the Council compromise on AI training data survives Parliament.
- **AI Office follow-ups:** whether the first requests for information turn into formal proceedings against GPAI providers.
- **National authorities:** continued designation of market surveillance authorities and single points of contact.

## Bottom line

The first 60 days of EU AI Act enforcement were about information, not penalties: requests to GPAI providers, a complaints channel, a voluntary code with broad uptake. The heaviest AI-related sanctions this quarter came from data protection authorities under the GDPR. For compliance teams, that is the key signal: the AI Act sets the framework, but regulators are already enforcing AI governance with the tools they have today.

## Related reading

### Article 14 EU AI Act: Engineering Human Oversight That Actually Works

Article 14 is a design obligation, not an HR policy. Here's how to engineer the five oversight capabilities, split provider vs deployer duties, handle the biometric two-person rule, and build an audit-ready evidence trail.

### EU AI Act Article 13: What Your Instructions for Use Must Contain, and What Deployers Should Demand From Vendors

Article 13 turns the instructions for use into the contract between a high-risk AI provider and its deployers. Here is what Article 13(3) requires, the evidence deployers should ask for, and where harmonised standards stand after 14 October 2026.

### GPAI Models With Systemic Risk: The Safety and Security Chapter of the Code of Practice, Explained

Providers of the most capable general-purpose AI models carry extra duties. Here is what the Safety and Security chapter of the GPAI Code of Practice asks for, the incident timelines, and the dates that matter now the AI Office can enforce.
