---
id: collect-261121-veille-ia/securite/the-european-commission-hack-shows-where-the-cyber-money-actually-goes
title: "The European Commission Hack Shows Where the Cyber Money Actually Goes"
domain: securite
role: reference
task: article
actors: []
dates: []
keywords: ["commission", "european", "most", "reported", "cyber", "campaign", "data", "tool"]
source: https://www.ainvest.com/news/european-commission-hack-shows-cyber-money-2611/
source_anchor: ""
source_lines: [1, 81]
section: "Securite"
---
On Nov. 19, POLITICO reported that a cyber campaign had disrupted the European Commission for more than ten days and may have exposed large amounts of employee data. The Commission's HR portal, Sysper, and its medical-reimbursement system, MyPMO, went down. Most salaries were paid; some staff could not submit hours, request leave, or file medical claims. The Corporate Management Board, the Commission's main internal crisis body, met in emergency session earlier this month.

Reports call it an AI-powered attack, built on a free tool called ARTEX. That label is doing more work than the evidence yet supports. What is established is less cinematic: a free, open-source framework that its developer could not take back; a set of internal applications taken offline; and a bill that will be paid to the vendors that already own identity, vulnerability management, and cloud telemetry.

That matters for anyone tempted to buy cybersecurity stocks on the headline. The breach is real. The revenue it creates is not yet visible. And the companies most often cited as "AI security" winners are priced for a much larger story than the Commission can currently document.

## What the Commission actually disclosed

The known facts are operational. CERT-EU detected the campaign this month. The Commission took several applications offline as a precaution, restored most of them, and saw Sysper crash again on Wednesday. An EU official told POLITICO the Commission believes it has contained most of the attack, but the extent of any data theft remains unclear, and CERT-EU has not established whether ARTEX enabled the breach or was used for reconnaissance. Spokesperson Thomas Regnier said individual systems were targeted and affected people were notified.

The role of ARTEX is the most important unresolved detail. A tool that finds the hole is a scanning story. A tool that walks through the hole, escalates privileges, and exits with data is an exploitation story. Only the second supports the strong version of the "AI is the new attacker" thesis.

The Commission's recent history is a better guide than the headline. This is the third significant incident in ten months. In January, attackers exploited vulnerabilities in Ivanti Endpoint Manager Mobile, the software managing the Commission's corporate phones; staff names and phone numbers were potentially exposed. In March, a supply-chain flaw in the Trivy tool led to the Amazon-hosted Europa.eu environment, where the ShinyHunters extortion group claimed 350 GB of databases, contracts, and email dumps; the episode affected nearly thirty European entities.

| Date | Reported entry point | What was at risk | Evidence grade | 
|---|---|---|---|
| Jan. 30, 2026 | Ivanti Endpoint Manager Mobile flaws | Staff names and phone numbers | Reported; contained in nine hours | 
| Mar. 24, 2026 | Trivy supply-chain flaw into AWS Europa.eu | 350 GB claimed by ShinyHunters; about 30 EU entities | Extortion group's claim; reported | 
| Nov. 2026 | ARTEX-linked campaign | HR, payroll, leave, medical-reimbursement data | Commission confirms campaign; theft unconfirmed | 

Grade the claims before trading them. The outage and the crisis meeting come from POLITICO's reporting, with the Commission confirming that individual systems were targeted. The 350 GB figure is ShinyHunters' claim. The $1,280 exploit cost below is OpenAI's estimate. The 2,250 ConnectWise figure means "affected" in the FBI's telling and "attempted" in Microsoft's. None of these distinctions kills the story. Each changes how much weight it can carry.

Across the two disclosed entry points, the common thread is third-party software and credentials. The November incident is consistent with that pattern, but its entry point is not yet public. The Commission had already tightened its cybersecurity rules after the 2022-23 attacks. It still got hit three times. That is the first check on the "buy more AI security" reflex: the failures are occurring in layers that are already supposed to be covered.

## What ARTEX changes, and what it doesn't

ARTEX is a real capability, and its design explains why it spread. It is an open-source framework written in Go, released in July 2026 by a developer using the name Autumn-27. It has no model of its own. It connects to external large language models - Anthropic's Claude, OpenAI's GPT models, DeepSeek, GLM, Grok, and Kimi - and coordinates agents that scan, exploit, escalate privileges, and document findings.

The FBI's Nov. 13 advisory describes the effect in one sentence: some intrusions moved from initial targeting to privileged access in under three hours, and the framework lets actors with minimal technical expertise run complex, multi-target operations at once. The techniques are ordinary - weak passwords, exposed applications, missing multi-factor authentication, hardcoded secrets, SQL injection, server-side request forgery. What changed is tempo. The tool is noisy, according to the FBI, but it is faster than a human defender's patch cycle.

The confirmed cases fit that pattern. In South Korea, ARTEX-linked attacks on financial institutions exposed data on at least 68,000 people, according to the FBI. The ConnectWise supply-chain breach reached MicrosoftMSFT-- 365 customer environments; the FBI says at least 2,250 customers were affected, while Microsoft says the actor attempted sign-ins to more than 2,250 tenants and most failed. The difference between "affected" and "targeted" is the difference between a breach investigation and a log entry.

The free part is the point. The developer made ARTEX closed-source on Oct. 8. The FBI says the previous open-source version still circulates and remains widely accessible. You cannot recall code.

Every ARTEX agent still needs a model API, the one chokepoint a vendor could own. Attackers have routed around it. The FBI says attacker-controlled instances mostly use Chinese models, with DeepSeek's Flash family the most prevalent because of its capability, low cost, and lack of observable guardrails when requests are framed as penetration testing. Amazon's threat-intelligence team watched ARTEX agents hunt for exposed model-provider keys, validate them, and pool them - a self-sustaining operation in which stolen keys paid for further credential hunting.

That is the economics in one line: the attack framework is free, the model access is cheap or stolen, and the marginal cost of the next intrusion is close to zero. The defense is a subscription with a sales team.

## The cost curve that actually moved

The most useful number in this story is not the 10-day outage. It is the collapse in the cost of finding a working exploit.

On Oct. 28, OpenAI said its reasoning models autonomously found and validated nearly 1,000 security flaws in the core Linux operating system, at an estimated cost of about $1,280 per working exploit. OpenAI put the traditional cost at $18.9 million to $66.3 million. Treat the comparison as the company's estimate, not an audited benchmark. Even with a wide error bar, the order of magnitude is the story. If finding a flaw costs four orders of magnitude less than it did, the scarce input is no longer discovery. It is the capacity to fix, verify, and monitor.

That is the demand signal, and it points away from the product category the headline implies. Palo AltoPANW-- CEO Nikesh Arora told analysts that "so many vulnerabilities being discovered on a daily basis that it's impossible for enterprises to patch them." The buyer's problem is asset inventory, identity, patching, and response time. Those are the layers where ARTEX found its openings, and where the budgets have to go if the buyer wants the outage to stop.

## Where the money went before this breach

The market has already made its choice, and it is not "AI security" in the narrow sense. It is identity and platform consolidation.

Palo Alto Networks closed its acquisition of CyberArk, an identity-security company, on Feb. 11. The deal was announced at roughly $25 billion; Palo Alto's annual report valued the consideration at $21.1 billion at closing. The 112 million Palo Alto shares issued to CyberArk holders were worth about $18.5 billion then. At the Nov. 10 closing price, they were worth about $36.6 billion - an increase of nearly 98%. The market added roughly the value of the original stock consideration to the identity asset in nine months.

The operating numbers show the same pull. In its fiscal first quarter ended Oct. 31, Palo Alto reported revenue of $3.44 billion, up 39% year over year, with remaining performance obligations of $21 billion, up 35%, and net retention above 120% for platform customers. It guided fiscal 2027 revenue to about $14.55 billion, up 26% to 27%.

The AI-specific line is real but small. Palo Alto's Prisma AIRS product passed $100 million in annual recurring revenue - about 0.7% of its guided fiscal 2027 revenue. Gartner, as cited in the same analysis, expects spending on securing AI systems to rise from about $3 billion in 2026 to $7.7 billion in 2028. That is a growing market. It is not the market a $253 billion company is valued on.

## The trade ran before the headline

The chronology is the cleanest receipt in the file. Cybersecurity stocks were the standout of the Nov. 18 U.S. session: Palo Alto and CrowdStrikeCRWD-- rose substantially while the Dow fell 1.13%. Palo Alto closed up 9.9%. That was before the company reported. After the bell, it beat and raised, announced the $4.5 billion Applied Compute deal, and the shares rose another 8.5% after hours. The Commission story broke the next day, Nov. 19.

So neither move can be attributed to the Commission breach. The first was a rotation into security while the broader market fell. The second was earnings. The breach arrived as confirmation of a narrative that was already priced.

That narrative had been through a violent October. Palo Alto shares had fallen nearly 19% from their early October peak before the Nov. 18 report, even as the threat headlines got worse. The market was not short of threat news; it was short of patience for valuations.

The valuations are the part a beginner should look at first. At $339.92 on Nov. 18, Palo Alto had a market value of about $253 billion, or roughly 17 times its guided fiscal 2027 revenue. It traded at 82 times forward earnings and 70 times the next year's expected earnings.

A 10% day at Palo Alto is about $25 billion of market value. That is 1.7 times its entire guided annual revenue, and 5.6 times the $4.5 billion it agreed to pay for Applied Compute, a start-up that helps companies adapt and run open-weight AI models, in an all-stock deal announced the same day. For a single government contract to move Palo Alto's revenue by 1%, it would have to be worth about $145 million a year. The Commission's breach does not create that.

## The innocent reading, and the break condition

There is a version of this story in which the breach is a genuine catalyst. The Commission is a large, visible buyer; a third intrusion in ten months could accelerate procurement, tighten EU-wide rules, and push other agencies to consolidate onto a smaller set of vendors. The AI-security market is small but compounding at a pace that could justify a premium if it persists. Palo Alto's 26% revenue guide and 39% quarterly growth are not the numbers of a business losing pricing power.

The break condition is not another breach headline. It is a change in who captures the spend. Three facts would settle it.

CrowdStrike's next report is the first test. If AI-enabled demand is broad, the company should raise its outlook, not merely beat it. A beat with a flat guide would suggest the "protection from AI" trade is borrowing demand from 2027.

Palo Alto's Applied Compute deal and CyberArk integration have to show up in annual recurring revenue and retention, not just in the acquisition price. If AI-security ARR stays near $100 million while the multiple stays above 80 times forward earnings, the premium is being paid for a story that has not arrived.

The Commission's procurement and disclosure would change the beneficiary list. A confirmed data theft, a regulatory finding, or a visible shift toward sovereign European vendors would matter. So would the opposite: a quiet consolidation onto Microsoft 365, AWS, or a single platform, which would confirm that the money goes to the bundle rather than the specialist.

The attackers got the productivity gain for free. Palo Alto is being sold at 82 times forward earnings. Both statements can be true. The breach is a serious event; it is not, by itself, evidence that the buyers of cyber stocks will earn a return from it. The next checkable fact is not a headline about ARTEX. It is the revenue line, the retention rate, and the price paid for them.
