---
id: collect-261121-veille-ia/datacenters-infra/fireworks-ai-breach-exposes-uber-data-and-highlights-supply-chain-risks
title: "Fireworks AI Breach Exposes Uber Data and Highlights Supply Chain Risks"
domain: datacenters-infra
role: reference
task: article
actors: []
dates: []
keywords: ["uber", "access", "data", "provider", "fireworks", "incident", "credentials", "breach"]
source: https://securityaffairs.com/203339/data-breach/fireworks-ai-breach-exposes-uber-data-and-highlights-supply-chain-risks.html
source_anchor: ""
source_lines: [1, 43]
section: "Datacenters_Infra"
---
Uber is notifying nearly 150,000 people after attackers used compromised credentials belonging to one of its technology providers to access personal information stored in the company’s external environment.

*“On October 9, 2026, a service provider notified Uber that an unauthorized third party had compromised the service provider’s internal credentials and used them to access Uber’s account with the service provider. After being notified of this incident, Uber immediately severed data connections with the service provider and launched an investigation. On October 13, 2026, our investigation determined that the unauthorized party had used the compromised credentials to access Uber data connected to the service provider.” reads the data breach notification shared with the California Attorney General’s Office. “We are notifying you about this incident because our investigation concluded that your personal information was among the information accessed.”*

The stolen data includes images of drivers’ licenses and other identity details, turning a security incident at an AI infrastructure supplier into a significant privacy issue for the ride-hailing company.

The incident dates back to October, when Uber learned that an unauthorized party had compromised the internal credentials of a third-party provider. According to notifications filed with U.S. state authorities, the provider informed Uber on October 9 that its credentials had been stolen and used to access the company’s data.

Uber found that attackers had accessed the data on October 7, two days before the company was notified. Uber then cut off the affected provider and began investigating the incident. The company said the provider’s stolen login details did not give attackers access to other Uber systems. This suggests that the breach was limited to the supplier’s environment.

The provider was identified as Fireworks AI, a cloud platform that gives developers access to open AI models. It allows companies to run and fine-tune these models without managing the required infrastructure themselves.

The stolen data includes drivers’ names, home addresses, dates of birth and driver’s license numbers. Attackers also accessed images of driver’s licenses and information from other government-issued IDs, according to Uber’s disclosures to regulators.

The scale of the incident became clearer after Uber filed a notification with Oregon authorities, reporting that 149,346 individuals had been affected. The company has also submitted breach notifications in several other states, including California, Iowa, Massachusetts, South Carolina, Texas and Vermont. However, Uber hasn’t disclosed how many of those affected are drivers, nor whether the total number could increase.

TechCrunch reports that Uber said it already had security alerts enabled to detect unauthorized access to its environment at Fireworks. They didn’t work because the attackers used legitimate internal credentials belonging to the compromised provider.

*“In Uber’s case, the company said it had alerts enabled to detect unauthorized access to its Fireworks environment, but because the hackers used Fireworks’ own internal credentials, “no alerts were triggered.”” states TechCrunch.*

This is a clear example of how access controls and monitoring systems can fail to identify malicious activity when an attacker operates through an account that the environment already trusts.

The breach is part of a wider security incident at Fireworks AI. In a November 9 update, the company said attackers had accessed customer account and billing details, as well as secrets, datasets and unfinished AI models belonging to a small number of customers. Fireworks hired Palo Alto Networks Unit 42 to investigate the incident and notified law enforcement.

The company linked the attack to a financially motivated cybercriminal group that wanted to sell stolen AI access tokens through unauthorized services. This type of attack is known as **LLMjacking**. It allows criminals to use stolen credentials or tokens to access AI services and run models at the victim’s expense. No known cybercriminal group has publicly claimed responsibility for the Uber data theft.

Uber was not the only Fireworks customer affected. Notion and Cursor also confirmed that attackers had accessed some of their data during the incident, according to TechCrunch. The case shows how a security breach at one technology provider can expose several customers, even when they have no direct connection with one another.

Fireworks has taken steps to contain the incident with the help of Palo Alto Networks Unit 42. The company strengthened its security by replacing compromised credentials, limiting privileged access, isolating affected systems, restricting access to stored data, and improving logging and monitoring to detect further suspicious activity.

Fireworks pointed out it had found no evidence of further successful unauthorized access to the customer-facing Fireworks platform after October 25.

Uber is offering affected individuals free credit monitoring and fraud assistance through Cyberscout, a TransUnion company. Recipients have 90 days from the date of their notification letter to enroll. Reuters reported that Uber had not identified any misuse of the exposed information at the time of its November 13 report.

For affected drivers, the exposure of license images and personal details creates risks that go beyond the immediate breach. Criminals could use this information to support identity theft, impersonation attempts, fraudulent applications or highly targeted phishing campaigns. Combining a person’s identity details with knowledge of their professional relationship with Uber could also make fraudulent communications more convincing.

This is also Uber’s second publicly known data breach in 2026. In August, the company was reportedly investigating a separate incident involving its Uber Freight trucking business, after an extortion group claimed to have stolen data.

The latest incident raises a broader question about how organizations monitor the access they grant to external technology providers. Uber had alerts in place, but those controls didn’t identify the activity because the attackers used credentials that the system considered valid. The company said it had not identified any misuse of the data to date. That is encouraging, but it doesn’t remove the need for affected individuals to remain alert to identity fraud and targeted social engineering.

**Follow me on Twitter:** **@securityaffairs** **and** **Facebook** **and** **Mastodon**

**(****SecurityAffairs** **– hacking, newsletter)**
