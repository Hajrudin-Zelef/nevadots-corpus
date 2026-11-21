---
id: collect-261121-veille-ia/copilot-microsoft/2026-microsoft-outage-wikipedia
title: "2026 Microsoft outage - Wikipedia"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-11-13"]
keywords: ["microsoft", "outage", "november", "servers", "services", "gateway", "networking", "caused"]
source: https://en.wikipedia.org/wiki/2026_Microsoft_outage
source_anchor: ""
source_lines: [1, 111]
section: "Copilot_Microsoft"
---
| 2026 Microsoft outage |  | 
|---|---|
| Date | November 12–14, 2026 | 
|---|---|
| Location |  | 
| Type | IT outage | 
| Cause | Faulty server updates | 
| Outcome | Worldwide Microsoft 365 outage, causing global disruption of critical services | 

A major global Microsoft outage occurred from November 12 to 14, 2026. Problems were reported with numerous Microsoft 365 services, including Outlook, Teams, and Copilot.

The outage began on November 12, between 17:00 and 18:00 UTC, initially affecting most Microsoft 365 services.<sup>[1]</sup><sup>[2]</sup> At 17:54 UTC, Microsoft first acknowledged a problem in the Americas with users not being able to access Exchange Online mailboxes. At 18:07 UTC, they warned about broader problems globally. At 23:04 UTC, Microsoft said they were returning some affected servers to service.<sup>[3]</sup>

The outage followed two days after Microsoft's November Patch Tuesday update, which covered 1,094 vulnerabilities.<sup>[4]</sup> Microsoft credited artificial intelligence (AI) as the reason for such a large number of bug fixes during a monthly update.<sup>[5]</sup><sup>[6]</sup>

The outage continued into November 13, affecting businesses, airlines, and government agencies worldwide.<sup>[7]</sup> At 04:36 UTC, Microsoft reported "partial recovery" in some regions.<sup>[3]</sup> After 16 hours, the outage was mostly narrowed down to Microsoft Exchange, which powers Microsoft's email services including Outlook.<sup>[8]</sup> Microsoft reported some "encouraging recovery progress" by early morning UTC on November 13,<sup>[9]</sup> and that email queues were "approaching normal levels" by the afternoon.<sup>[3]</sup>

The services were fully restored by the morning of November 14.<sup>[10]</sup>

Charles Lamanna, the Executive Vice President of Copilot, Agents, and Platform at Microsoft, summarized the root cause analysis in a post on X<sup>[11]</sup> indicating that there were three issues in Microsoft's networking gateway servers that combined with response and cloud fabric failures to cause the outage:

1. A previously undetected defect that caused networking gateway servers to become unresponsive in certain load conditions. At peak more than 80% of the servers hosting the networking gateway service were unresponsive.
2. A change in traffic allocation in the network fabric increased load and caused the defect to become more likely to manifest.
3. Maintenance applied to the networking gateway service hosts reducing the fleet capacity by 14%.

Automated rollback mechanisms could not mitigate issue 1 as the change causing the error was deployed in July 2026.

The management abstraction of "capacity units" to separate infrastructure failed to isolate the issue, causing it to impact across regions meaning the outage spanned worldwide. Email services provided by Exchange Online could not be moved from the primary group of networking gateway instances to the secondary group of network gateway instances. Other products could, reducing impacts to Teams, OneDrive, SharePoint, and Office apps.

As the code fix to an address 1 was deployed, traffic overwhelmed the servers coming up — an instance of cascading failure caused by stampeding requests. Automated traffic routing had to be disabled to give hosts the ability to recover before receiving requests.

Microsoft 365 customers have been directed to `MO1550821` in their admin center tool for further updates.<sup>[12]</sup>

Microsoft said that the outage was not caused by a cyberattack or AI agents,<sup>[3]</sup> and blamed the outage on settings applied to servers after an update.<sup>[13]</sup><sup>[14]</sup><sup>[15]</sup> They said that a process that establishes secure connections was becoming overwhelmed, which led servers to stop handling requests. They began rolling back the recent network configuration change.<sup>[4]</sup>

The outage had significant impact on public and private organizations around the world. Around 430 million people globally use Microsoft 365, and issues were reported across more than 70 countries.<sup>[16]</sup> Internet search tools making use of Microsoft's services, such as Microsoft Bing, DuckDuckGo and ChatGPT, were also affected.<sup>[17]</sup> Many jokes and memes circulated online in response to the disruption.<sup>[18]</sup>

Government departments in Australia, Japan, the Philippines, Singapore, South Africa and the United States were affected by the disruption.<sup>[19]</sup> Spy and federal agencies in Australia suffered from minor disruption.<sup>[20]</sup><sup>[21]</sup>

The Japanese government confirmed disruption to the Government Solution Service (GSS), a network system that serves as the foundation for government employees to carry out their daily work.<sup>[22]</sup> The Supreme Court of the Philippines was affected, warning that "responses to emails and other transactions may be delayed".<sup>[20]</sup> Singapore also suffered issues in their court systems.<sup>[20]</sup>

In the United Kingdom, the minister for defence readiness and industry, Luke Pollard, urged for "greater resilience" against technology failures.<sup>[23]</sup>

In the United States, House of Representatives staff told Politico that "a day of legislative work" had been ended by the outage.<sup>[20]</sup> Law enforcement agencies also reported issues, including for example in processing prisoners' bail payments.<sup>[24]</sup>

In Brazil, the president of the Supreme Federal Court, Edson Fachin, at the request of the Order of Attorneys of Brazil, ordered on November 13 the suspension of the counting of procedural deadlines for the 12 and 13 of that month, and the count should be resumed on Monday, November 16.<sup>[25]</sup><sup>[26]</sup>

Lost productivity from the outage is expected to be high.<sup>[27]</sup> The Anderson Economic Group think tank estimated it could be about $7 billion.<sup>[28]</sup> Experts in Australia warned that the outage could cost billions of dollars to businesses in the country, with small businesses hit the hardest.<sup>[29]</sup>

Due to the outage, companies including Richemont delayed reporting quarterly financial results.<sup>[9]</sup><sup>[30]</sup>

On November 13, 2026, guests at Phantasialand's Hotel Ling Bao in Brühl, Germany suffered injuries after an automatic chlorine dosing system malfunctioned. This malfunction caused excess chlorine levels to be pumped into pool water, affecting guests. 15 people were checked by paramedics, with 5 suffering injuries as a result. The hotel's monitoring system runs on Outlook, and as a result of the outage it was unable to send an email notifying hotel staff of the excess chlorine levels.<sup>[31]</sup>

Several hospital networks in Toronto, Canada, including University Health Network, The Hospital for Sick Children, and Sunnybrook Health Sciences Centre reported that they experienced operational difficulties due to the lack of Outlook availability. Unity Health Toronto declared a code grey, indicating infrastructure failure.<sup>[32]</sup>

The outage caused widespread disruption across the UK's National Health Service (NHS), affecting hundreds of hospitals, general practitioner (GP) practices and community health services. The NHS cancelled some operations, with much of the NHS calendar using Microsoft systems.<sup>[33]</sup> Many NHS staff were unable to access NHS.net Connect, the platform used for email, meetings and file sharing, and some resorted to using paper patient records. The outage also disrupted the transmission of patient information and urgent referrals, including those relating to cancer and other serious conditions. The Medicines and Healthcare products Regulatory Agency (MHRA) separately reported problems with its email services. The National Pharmacy Association (NPA) warned that the loss of NHS email access was disrupting communication between pharmacies and GP practices, potentially delaying some prescriptions.<sup>[34]</sup><sup>[35]</sup>

In the UK, the outage caused communication issues in schools. Exam watchdog Ofqual, school leaders' union ASCL, the Department of Education, and many schools across England suffered from major disruption.<sup>[36]</sup>

Reinier van Lanschot, the lead MEP for the EU sovereign cloud, and Sergey Lagodinsky an MEP and reporter for opinion of the EU’s Cloud and AI Development Act, emphasized how vulnerable Europe is for being dependent of a single company.<sup>[37]</sup><sup>[38]</sup> Rupert Goodwins reported this will increase alternatives to Microsoft in Europe.<sup>[39]</sup><sup>[40]</sup> Portuguese legislative assembly member Fabian Figueiredo noted that “in Portugal, Law No. 36/2011 establishes the adoption of open standards in the state’s computer systems, precisely to ensure interoperability and avoid the dependence of a single supplier” and he asked the Government how this law is being complied with and what evaluation it makes of its application.<sup>[41]</sup>

Sâmia Bomfim, a Brazilian federal deputy, proposed an amendment for the adoption of domestic and open-source technologies.<sup>[42]</sup>

- 2024 CrowdStrike-related IT outages
- 2026 FAA nationwide ground stop – occurred the day after the outage began
- November 2026 technology outages

1. ↑ "What we know about the global Microsoft outage". *www.bbc.com* . November 13, 2026. Archived from the original on November 13, 2026. Retrieved November 13, 2026.
2. ↑ Sharwood, Simon (November 12, 2026). "Microsoft struggling to fix massive cloud outage that's taken down email and Intune". *The Register* . Retrieved November 13, 2026.
3. 1 2 3 4 Matthews, Jenny; Pandey, Manish; Gerken, with technology reporters Tom; McCallum, Shiona; Cress, Laura (November 13, 2026). "Microsoft says it is 'deeply sorry' for global outage after almost 24 hours of disruption - follow live". *BBC News* . Archived from the original on November 13, 2026. Retrieved November 13, 2026.
4. 1 2 "Microsoft 365 Outage Disrupts Outlook, Exchange Online and Teams". *SecurityWeek* . November 12, 2026. Archived from the original on November 13, 2026. Retrieved November 13, 2026.
5. ↑ "Microsoft Fixes Nearly 1,100 Security Vulnerabilities – Krebs on Security". November 10, 2026. Archived from the original on November 12, 2026.
6. ↑ Blog, Windows Experience; Costa, Nuno (November 5, 2026). "A new standard for vulnerability management in the AI era". *Windows Experience Blog* . Archived from the original on November 9, 2026.
7. ↑ Antonio Pequeño IV. "Microsoft 365 Outage Extends Into Friday—Governments, Airlines Report Disruptions". *Forbes* . Archived from the original on November 13, 2026. Retrieved November 13, 2026.
8. ↑ Taylor, Josh; Sweney, Mark (November 13, 2026). "A global Microsoft 365 outage has hit millions – what happened and when will it be fixed?" – via The Guardian.
9. 1 2 Whitehead, Jamie; Moon, Jenna (November 13, 2026). "Microsoft outage updates: Global issue hits schools, GPs and businesses - follow live". *BBC News* . Archived from the original on November 13, 2026. Retrieved November 13, 2026.
10. ↑ "Microsoft says services fully restored after day-long global outage". *BBC News* . November 13, 2026. Retrieved November 14, 2026.
11. ↑ "Microsoft 365 Outage Cause Summary 11/16/2026 6:06:00 AM UTC". *X* . Retrieved November 16, 2026.
12. ↑ "Microsoft 365 Status X Account Update 11/14/2026 6:09:00". *X* . Retrieved November 16, 2026.
13. ↑ Sweney, Mark; Taylor, Josh; Elsworthy, Emma (November 13, 2026). "Microsoft scrambles to fix global outage affecting Teams and Outlook". *The Guardian* . ISSN 0261-3077. Retrieved November 13, 2026.
14. ↑ "Microsoft outage live updates: Services including Outlook down globally | CNN Business". *CNN* . Archived from the original on November 13, 2026. Retrieved November 13, 2026.
15. ↑ Reid, Jenni; Subin, Samantha (November 12, 2026). "Widespread Microsoft outage takes down Outlook and other 365 services". *CNBC* . Archived from the original on November 13, 2026. Retrieved November 13, 2026.
16. ↑ Cress, Laura (November 14, 2026). "Microsoft outage 'stark reminder' of how much we rely on tech". *BBC News* . Retrieved November 14, 2026.
17. ↑ Team, HowPremium (September 4, 2026). "Microsoft Outage: Why Bing, Copilot, DuckDuckGo and ChatGPT Search Failed". *HowPremium* . Retrieved November 15, 2026.
18. ↑ "Microsoft outage live updates: 'Encouraging recovery progress' in global outage that has impacted millions | CNN Business". *CNN* .
19. ↑ "All the major organisations affected by the Microsoft outage". *The Independent* . November 13, 2026. Retrieved November 13, 2026.
20. 1 2 3 4 "Courts, spies and MPs: How the Microsoft outage has been felt around the world". *BBC News* . November 13, 2026. Retrieved November 13, 2026.
21. ↑ "Global Microsoft Outage: Friday, 13 November 2026". *www.capetown.gov.za* . Retrieved November 15, 2026.
22. ↑ "Japan Govt Hit by Microsoft Massive Outage". *Nippon.com* . November 13, 2026. Retrieved November 13, 2026.
23. ↑ Quinn, Ben; Gregory, Andrew (November 13, 2026). "Microsoft outage hits hospitals, GPs, schools and police across UK". *The Guardian* . ISSN 0261-3077. Retrieved November 13, 2026.
24. ↑ Issy Ronald, Anna Cooban, Hanna Ziady, Karina Tsui, Gage Goulding, Kathleen Magramo, Laura Sharman, Helen Regan, Hilary Whiteman, Ross Adkin, Brian Stelter, Chris Isidore. "Microsoft outage live updates: Email service is 'approaching normal levels' following widespread global outage". *CNN* . Archived from the original on November 13, 2026. Retrieved November 13, 2026.`{{cite web}}` :  CS1 maint: multiple names: authors list (link)
25. ↑ "STF suspende prazos após falha nos serviços da Microsoft". *Migalhas* (in Brazilian Portuguese). November 13, 2026.
26. ↑ Lobato, Alan. "Falha global da Microsoft reacende debate sobre riscos da privatização da Celepar – Capital Digital" (in Brazilian Portuguese).
27. ↑ Nagraj, Aarti; Cabral, Alvin R. "Microsoft outage to have 'cascading impact' on businesses". *The National* . Retrieved November 13, 2026.
28. ↑ Issy Ronald, Chris Isidore, Anna Cooban, Hanna Ziady, Karina Tsui, Gage Goulding, Kathleen Magramo, Laura Sharman, Helen Regan, Hilary Whiteman, Ross Adkin, Brian Stelter, August Phillips. "Microsoft says email service is 'approaching normal levels' following widespread global outage | CNN Business". *CNN* . Retrieved November 14, 2026.`{{cite web}}` :  CS1 maint: multiple names: authors list (link)
29. ↑ "Outlook outage expected to cost Australian businesses billions of dollars". *ABC News* . November 13, 2026. Archived from the original on November 13, 2026. Retrieved November 13, 2026.
30. ↑ Carson, Ruth; Toh, Bernadette; Montijano, Macarena Munoz (November 13, 2026). "Microsoft Outage Sends Traders Back to Phones and Fax". *Bloomberg.com* . Retrieved November 13, 2026.
31. ↑ Page, Carly (November 16, 2026). "Microsoft Outlook outage blamed after 15 injured in hotel pool chlorine incident". *theregister* . Retrieved November 17, 2026.
32. ↑ "Microsoft's global outage extends into 2nd day, company not aware of malicious activity". *CBC News* . November 13, 2026. Archived from the original on November 13, 2026. Retrieved November 13, 2026.
33. ↑ Whitehead, Jamie; Pandey, Manish; Gerken, with technology reporters Tom; McCallum, Shiona; Cress, Laura (November 13, 2026). "Microsoft outage updates: Global issue hits courts and schools - follow live". *BBC News* . Retrieved November 13, 2026.
34. ↑ Dodd, Vikram (November 13, 2026). "How Microsoft outage has affected key public services in the UK". *The Guardian* .
35. ↑ "UK pharmacies hit by Microsoft outage". *The Pharmaceutical Journal* . November 13, 2026.
36. ↑ Chantler-Hicks, Lydia (November 13, 2026). "Microsoft outage: Schools, DfE and Ofsted affected". Retrieved November 13, 2026.
37. ↑ "Microsoft outage renews calls for European tech sovereignty". *Rapporteur* . November 13, 2026.
38. ↑ "Após horas de blecaute global, Microsoft diz que serviços foram totalmente normalizados". *dw.com* (in Portuguese).
39. ↑ Habib, Heba. "Microsoft hit by global outages: What happened and why?". *Al Jazeera* .
40. ↑ "Após horas de blecaute global, Microsoft diz que serviços foram totalmente normalizados". *dw.com* (in Portuguese).
41. ↑ "Apagão da Microsoft reabre debate sobre soberania digital". *www.esquerda.net* (in European Portuguese). November 16, 2026.
42. ↑ "Brazilian lawmaker urges digital sovereignty after global Microsoft outage". *Anadolu* (in Turkish). November 13, 2026.

- Media related to 2026 Microsoft outage at Wikimedia Commons
