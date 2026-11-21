---
id: collect-261121-veille-ia/securite/ai-red-team-tool-artex-likely-behind-cyber-attack-on-eu-body-computer-weekly
title: "AI red team tool Artex likely behind cyber attack on EU body | Computer Weekly"
domain: securite
role: reference
task: article
actors: []
dates: []
keywords: ["artex", "cyber", "attack", "body", "computer", "said", "likely", "european"]
source: https://www.computerweekly.com/news/366654974/AI-red-team-tool-Artex-likely-behind-cyber-attack-on-EU-body
source_anchor: ""
source_lines: [1, 69]
section: "Securite"
---
Maren Winter - stock.adobe.com

Artex, an open source penetration testing framework powered by artificial intelligence (AI), may have been implicated in a cyber incident affecting the European Union’s (EU) executive body, the European Commission (EC) in Brussels.

The attack, which unfolded at the end of October, appears to have targeted the commission’s Sysper human resources (HR) platform, leaving staff unable to undertake some key tasks. The EC swiftly took down affected systems and Computer Weekly understands Sysper is back online at the time of writing, although, according to *Euronews*, other systems – including a health insurance program – are still offline.

Speaking on Thursday 19 November, the EC spokesperson for digital, Thomas Regnier, said: “The investigation is still ongoing. We are taking any necessary steps based on the findings as they come…. We have not detected any evidence of a data breach affecting commission staff as a whole.”

 **Chris Dimitriadis, ISACA**
  

The European Union’s Computer Emergency Response Team (CERT-EU) said: “CERT-EU has observed similar instances of AI-driven activities against its constituency. These activities highly likely involved the use of Artex.”

ISACA chief global strategy officer Chris Dimitriadis said: “On the eve of the EU-UK summit, the news that the European Commission has suffered a significant cyber attack exposes Europe’s executive body.

“While alarming, this attack is not surprising. Thanks to AI, attackers now hack at the speed of intent – a pace the public and private sector cannot outrun without serious investment in the cyber workforce. Until organisations deploy AI defensively as fast as criminals deploy it aggressively, we will continue on this trajectory.

“In the short term, the commission will investigate whether staff data has been compromised and what weakness hackers were able to use Artex to exploit. They should be bolstering their cyber security and building long-term resilience by investing in recruiting, training and upskilling privacy professionals who can build defences that are stronger than the AI tools now freely available to attackers.”

At its core, the legitimate Artex framework consists of a large language model (LLM)-powered agentic platform, written in Go with a Next.js front end.

It can be used to automate offensive cyber tasks and was recently identified by threat intelligence teams and law enforcement agencies as increasingly finding favour among cyber criminals thanks to its ability to shrink the time needed to chain various offensive cyber tasks.

Indeed, some intrusions orchestrated using Artex are believed to have moved from initial targeting to full privileged access in less than three hours, moving up cyber attack timelines.

But its value to a threat actor lies not just in its speed, but in its scale and persistence as well. Artex is able to probe huge numbers of low-complexity issues, such as misconfigurations, that human eyes would miss, and of course it does not get tired or need tea breaks.

Microsoft said the tool was being heavily used by a Chinese state-aligned threat actor tracked as Storm-0940 to obtain access to its victims’ cloud infrastructure – including Microsoft 365 environments. Redmond’s findings have been corroborated by Google’s Threat Intelligence Group (GTIG) and its Mandiant stablemate.

Artex was first released on GitHub by a user known by the handle Autumn-27 in July of this year and reached between 1,500 and 2,000 users in the space of 10 weeks, according to the FBI in the US.

Autumn-27 had allegedly stated that Artex was supposed to be used for “authorised security testing” within defined scopes, and in fact pulled its repository on 8 October after reports that it was being abused.

Despite this, Artex persists and continues to proliferate through community forks, localised variants and a commercial version, Artex Pro.

CERT-EU said: “We assess that Artex’s public availability will likely accelerate adoption by threat actors, resulting in more frequent, faster and more scalable automated attacks with a higher success rate.

“In April 2026, in our blog post *AI is changing the economics of vulnerability discovery*, we warned that autonomous offensive capabilities would progressively reach a wider range of actors. Artex is a concrete example of that shift materialising.”

CERT-EU has made available a list of tasks for security teams to undertake to safeguard against Artex.

In the immediate future, priorities should be to audit publicly exposed IT assets and limit exposure; ensure management and control panels are isolated from the public internet or untrusted networks; prioritise emergency patching for all assets that must be exposed to the internet; and to monitor privileged and administrator account activity for evidence of suspicious behaviour or abuse.

In the short term, security teams should increase their use of multifactor authentication (MFA), and hunt for and alert on connections to known or suspected threat infrastructure to identify potential compromises, particularly artefacts and behaviours consistent with Artex. Endpoint detection and response (EDR) tools can also be pressed into service to block strange processes.

Security teams should continuously sanitise error responses from internet-exposed devices to minimise threat actor enumeration of their assets, protect said assets with web application firewalls (WAFs), adopt detection and logging best practices for incident response, ensure monitoring covers AI tools and assets using them, conduct regular vulnerability scans and penetration tests, and retain all findings – even those that don’t seem very severe – in the remediation cycle, which can help stop tools like Artex from chaining a series of minor problems into a great big one.

Looking ahead, Snehal Antani, CEO of Horizon3, said the clear jump in attack volume and velocity seen in recent weeks was likely attributable to frameworks like Artex and similar tools that enable bad actors to harness fleets of agents, essentially industrialising cyber attacks using proven, basic techniques.

“Artex and the forks that followed it are the multiplier. One operator can now run many targets in parallel, and the agents keep iterating even when the human walks away,” said Antani.

  **Snehal Antani, Horizon3**
   

In emailed comments, Antani told Computer Weekly that Artex and similar frameworks will drive a type of attack standardisation that makes attribution much harder.

“When different groups – state-aligned or purely criminal – are all running near-identical agent harnesses, models and playbooks, their operational fingerprints start to look the same. You lose a lot of the traditional signals that used to separate actors. 

“Going forward, expect this to become normal rather than exceptional. The skill barrier keeps dropping, the tooling keeps forking, and the economics favour volume. Organisations have significant cyber debt that is easily exploitable by frameworks like Artex, so we should expect persistent disruption of digital services – travel, banking, commerce, etc – in the coming months as these organisations scramble to quickly improve their cyber resilience.”

Antani warned that any defenders still inclined to treat exposure management as a quarterly scanning or annual penetration testing exercise will find themselves swiftly overwhelmed.

“Machine-speed offence is already here; machine-speed defence has to match it,” he said.

### Read more about AI-powered cyber attacks

- A spate of high-profile IT outages in the US is surfacing questions about what resilience looks like in an AI-driven world.
- In a worst-case scenario, self-replicating artificial intelligence could launch cyber attacks at will against UK targets by 2028.
- Anthropic’s revelation that a model broke out of its test environment three times and accessed external systems shows the power of these models.
