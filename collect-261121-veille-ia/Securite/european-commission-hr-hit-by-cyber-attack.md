---
id: collect-261121-veille-ia/securite/european-commission-hr-hit-by-cyber-attack
title: "European Commission HR hit by cyber attack"
domain: securite
role: reference
task: article
actors: []
dates: []
keywords: ["artex", "commission", "said", "attack", "cyber", "tool", "european", "attackers"]
source: https://www.csoonline.com/article/4251068/european-commission-hr-hit-by-cyber-attack.html
source_anchor: ""
source_lines: [1, 47]
section: "Securite"
---
Barely a week after the US Federal Bureau of Investigation (FBI) warned companies of the cyber attacker interest in an open source penetration testing tool called ARTEX, the European Commission has confirmed that attackers armed with that tool broke into its HR systems.

The attack, which Commission officials detected on October 22, forced them to shut off much of the access to the systems for more than 10 days.

“As a protective and preventive measure, we temporarily took down several applications and most of them have since then been restored,” Commission spokesperson Thomas Regnier said Thursday. “We have not detected any evidence of a data breach affecting Commission staff as a whole.”

During a news media briefing, a transcript of which was provided to CSOonline, he said that the European Data Protection Supervisor (EDPS) was informed of the attack, “so there was no hiding whatsoever.”

Regnier would not go into detail about the attacks, given that the matter is still being investigated, but he did say, “it is a campaign, so there was a series of incidents.”

In an email, Commission officials added, “we are also preventively checking other systems while staying in close contact with CERT-EU and the EDPS.”

## ARTEX popular with attackers

The November 13 FBI statement about ARTEX, described as an autonomous penetration testing framework, said that the tool is becoming very popular with attackers.

“Since at least August 2026, a variety of malicious cyber actors have used ARTEX to conduct campaigns targeting organizations globally across multiple sectors, including financial services, government, technology, defense, semiconductor, energy, healthcare, manufacturing, telecommunications, and legal industries,” the FBI advisory said. “ARTEX significantly reduces the time required to chain attack techniques, with some intrusions progressing from initial targeting to privileged access in under three hours. The framework enables threat actors with minimal technical expertise to conduct complex, multi-target operations simultaneously, fundamentally accelerating attack velocity and expanding operational scale.”

Although ARTEX can handle a wide range of pentesting functions, it appeared to be primarily used in the Commission attack to search for and locate credentials.

The FBI said that ARTEX can “automatically map effective permissions of Azure credentials–SAS tokens, Service Principals, Kudu credentials, Storage Account keys–using sub-second capability checks.”

ARTEX has been known to SOC operations since GitHub first released the tool in July, but widespread misuse of the tool prompted GitHub to make it unavailable on October 8. “Despite this action, ARTEX remains widely accessible through independent routes and mirrors on code-sharing platforms,” the FBI said.

## Uses different tactics

Brian Levine, a partner with consulting firm Control Risks, added that ARTEX’s AI-fueled tactics are different from what many cybersecurity teams are used to.

“ARTEX doesn’t need to steal your keys. It finds the ones you left lying around, like a SAS token in a code repo, a storage key in a config file, or a Kudu credential nobody rotated. Then it tries each one against every door in under a second,” he said. “Picture a burglar walking the block at 3 a.m. and checking every car door handle. He isn’t breaking glass. He’s taking inventory of what you left unlocked and AI lets him work a thousand blocks at once.”

## Companies need a secure fallback

Noah Kenney, principal consultant at Digital 520, said he was unimpressed with the Commission’s attempts to tell EU citizens that all was well.

“The Commission says it has no evidence of a breach affecting its staff ‘as a whole,’ which is very different from saying that there is no evidence of a breach affecting any of the staff,” Kenney said. “The wording doesn’t give me much comfort.”

Kenney also said that the attackers’ decision to target HR was not an accident, and that the Commission’s decision to temporarily shut down some systems may have inadvertently helped the intruders.

“When payroll, leave, and medical reimbursement systems go down, people start using spreadsheets, email and manual approvals. Those workarounds often have weaker access controls and worse audit trails than the systems taken offline,” he said. “Enterprises need to define a secure fallback process before the attack, because improvising one often leads to greater exposure.”

## HR systems a prize

Levine also said that the choice of an HR victim was understandable.

“HR systems are a prize. Names, home addresses, pay grades and health insurance records for EU civil servants make a ready-made targeting list for espionage and social engineering,” he said. “Whether or not data left the building, every Commission employee should prepare to receive a very convincing phishing email soon.”

His advice to CISOs: “Get secrets out of code and pipelines and rotate anything that has been exposed. Give every token and service principal only the access it needs, so a found key opens a closet, not the vault. Put cloud admin interfaces behind private access.”
