---
id: collect-261121-veille-ia/regulation-us/fedramp-high-vs-moderate-for-ai-vendors-explained-antm
title: "FedRAMP High vs Moderate for AI Vendors Explained | ANTM"
domain: regulation-us
role: reference
task: article
actors: []
dates: ["2017-11-15"]
keywords: ["fedramp", "high", "moderate", "class", "services", "authorization", "anthropic", "post"]
source: https://www.antm.ai/articles/fedramp-high-vs-moderate-how-to-read-ai-vendor-government-claim
source_anchor: ""
source_lines: [1, 49]
section: "Regulation_US"
---
When Anthropic announced an expanded agreement with the US General Services Administration on 27 October 2026, the post carried two kinds of claims side by side: a price (40% off) and a set of compliance labels (FedRAMP High, FedRAMP Moderate "in process", DoD impact levels).<sup>[1]</sup> The price is easy to read. The labels are not, and they are what a security reviewer actually has to decide on. This guide explains **FedRAMP High vs Moderate**, what the newer FedRAMP 20x pathway changes, and six checks you can apply to any AI vendor's "authorized for government" claim. Anthropic's post is the worked example because it is recent and unusually specific; the checks apply equally to OpenAI, Google, Microsoft or anyone else.

## What the levels mean

FedRAMP's own guidance ties its levels to FIPS 199, the NIST standard for categorizing federal information and systems, first published in February 2004.<sup>[6]</sup> The categories depend on how much harm an event could cause to an organisation's mission, assets, legal duties, day-to-day operations or individuals, and they are assessed across three objectives: confidentiality, integrity and availability.<sup>[3]</sup> In the archived FedRAMP page we read (dated 15 November 2017, so check the current rules for control details):

| Level | Wording from FedRAMP's guidance | Example given | 
|---|---|---|
| **Low** | A loss "would result in limited adverse effects on an agency's operations, assets, or individuals" | A SaaS app holding only login data such as usernames and emails | 
| **Moderate** | A loss "would result in serious adverse effects" | The page says Moderate covers nearly 80% of the applications that received FedRAMP authorization at the time | 
| **High** | A loss "could be expected to have a severe or catastrophic adverse effect" | Law enforcement, emergency services, financial and health systems | 

Two practical readings follow, and both are our interpretation, not FedRAMP's. First, the level describes the data and mission the service may be used for, so "High" is not a quality score; a Moderate service can be perfectly adequate for the work most agencies do. Second, a higher level means a larger set of controls to satisfy and evidence to show, which is why vendors announce it as an achievement.

## What FedRAMP 20x adds

FedRAMP 20x is, in FedRAMP's words, a new approach to assessment and authorization that moves "assurance away from paperwork and toward evidence."<sup>[4]</sup> Its certification classes map onto the same impact levels: Class B to Low, Class C to Moderate, and Class D, still in development, to High, with Class A for mature programmes entering the federal market.<sup>[4]</sup> The FedRAMP site says Classes A, B and C are available now and that Class D is planned for a later pilot phase.<sup>[4]</sup>

The FedRAMP Marketplace, a searchable database of certified cloud services, authorizing agencies and recognised assessors, shows 20x pilot certifications separately from full 20x certifications. On the day we read it (5 November 2026) it listed 36 FedRAMP 20x certified services out of 554 certified services overall, and the site noted its first full Class C certification on 2 November 2026.<sup>[5]</sup> For a buyer, the practical consequence is that "in process through 20x" is a real pathway with a published scoreboard, but still not an authorization.

## A worked example: Anthropic's October 2026 post

Read closely, the post makes product-specific statements:<sup>[1]</sup>

| Product or channel | Status as stated by Anthropic | 
|---|---|
| **Claude for Government** | FedRAMP High authorization; generally available since 30 September; described as authorized for "the most sensitive unclassified work" | 
| **Claude on Amazon Bedrock** | FedRAMP High; select models authorized for DoD IL4 and IL5 in AWS GovCloud | 
| **Claude on Google Cloud Vertex AI** | FedRAMP High | 
| **Claude Enterprise** | "in process for FedRAMP Moderate authorization through the FedRAMP 20x program" | 

The September product announcement adds that Claude for Government is delivered through a "FedRAMP High authorized environment", that agencies "do not need a separate cloud-provider relationship to get started", and that Anthropic's FedRAMP Secure Configuration Guide is available through its trust center.<sup>[2]</sup> Those are vendor statements. We did not check the FedRAMP Marketplace entries for each product, and we could not open the DoD cloud security guide to confirm what IL4 and IL5 cover, so we do not define them here.

The same post is a useful lesson in not mixing categories. The GSA part is about price: 40% off Claude Enterprise, Claude for Government and the Claude API, no minimum spend, and eligibility that now includes state, local, tribal and territorial governments.<sup>[1]</sup> It replaces the structure of the August 2025 OneGov deal, which GSA announced at a nominal fee of $1 per agency for up to a year, with executive-branch agencies buying through GSA's Multiple Award Schedule.<sup>[7]</sup> Neither the discount nor the schedule tells you what data the service may hold.

## Six checks for any AI vendor's government claim

1. **Which product?** Authorization attaches to a specific offering. The same model can sit behind a High-authorized product and an unauthorized one, as the table above shows. Ask for the product name as it appears on the FedRAMP Marketplace.
2. **Which level, and does it match your data?** Map your data to Low, Moderate or High using FIPS 199-style reasoning: what would a loss of confidentiality, integrity or availability do?<sup>[3]</sup><sup>[6]</sup>
3. **Authorized or in process?** Treat "in process", "pursuing" and "pilot" as roadmap, not status. The Marketplace is the place to check.<sup>[5]</sup>
4. **Which deployment surface?** A model reached through a cloud provider's authorized service, through the vendor's own environment, and through a consumer app can have different statuses. Confirm that your integration uses the covered one.
5. **What evidence can you get?** Anthropic's post refers to a secure configuration guide and to audit logs and documentation for agencies' authorization work.<sup>[2]</sup> Ask for the equivalent from any vendor, and for the shared-responsibility split.
6. **What is the price vehicle adding?** Discounts, schedules and zero-seat-fee models change cost, not risk. Keep them in a separate row of your evaluation.

## What remains uncertain

We have not seen the agreement itself or any GSA text for the October 2026 expansion; the figures come from Anthropic's post, and the GSA page we read was the August 2025 release. The savings estimate of $1 billion in that post is Anthropic's. We did not read the current FedRAMP baseline control catalogues, so we state no control counts. For adjacent reading, our enterprise AI vendor data retention checklist covers what to ask about data handling, and our guide to the NIST AI Risk Management Framework covers the voluntary framework that sits alongside these cloud-security labels.

*Retrieved and read 2026-11-05. Updated: 2026-11-05.*
