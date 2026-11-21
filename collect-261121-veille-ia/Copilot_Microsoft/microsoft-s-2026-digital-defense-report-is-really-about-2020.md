---
id: collect-261121-veille-ia/copilot-microsoft/microsoft-s-2026-digital-defense-report-is-really-about-2020
title: "Microsoft's 2026 Digital Defense Report Is Really About 2020"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["microsoft", "report", "about", "models", "access", "digital", "defense", "months"]
source: https://jark.me/blog/microsofts-2026-threat-report-is-really-about-2020/
source_anchor: ""
source_lines: [1, 47]
section: "Copilot_Microsoft"
---
Microsoft released its 2026 Digital Defense Report on October 1. Like most vendor reports, it runs long. It covers July 2025 through June 2026, and the coverage has already flattened into one headline about autonomous AI ransomware. Of course the top finding would be about artificial intelligence. No surprises there, though a few of the findings caught me off guard.

Reading the whole thing lands a completely different signal. AI is not inventing new ways into a network. It is industrializing the old ones, and the single most detected vulnerability in the data is six years old. Five findings carry the weight, and they mean more read together than apart.

1. Weaponization now outruns patching. Microsoft counted nearly 40,000 CVEs in the first half of 2026 alone, putting the year on track to roughly double. The median time from a vulnerability surfacing in the wild to being weaponized has fallen to well below 24 hours. Enterprise remediation for critical external vulnerabilities, meanwhile, can still take 30 to 60 days. The gap between those two clocks is the whole problem.

Of course Microsoft gets to narrate this crisis from both sides of the podium. Those 40,000 CVEs are ecosystem-wide, so no, Redmond did not personally create all of them. But Microsoft has spent decades shipping one of the largest legacy software estates on earth, and its monthly patch ritual is famous enough to have its own name. Hearing it explain how enterprises are too slow to patch has a certain arsonist-running-the-fire-safety-seminar energy. Yes, customers need to remediate faster. Microsoft could also help by giving them less shit to remediate in the first place. Perhaps the pretty report and the secure code can coexist.

2. AI is moving from passenger to driver. The report traces a progression I have written a lot about lately. AI assists human threat actors, then AI directs the attack, then AI executes on its own. In one Microsoft evaluation, a model strung 32 attack stages together in a controlled environment. I am unaware of any real-world attack described as having 32 stages, which is why this read as a bit peculiar to me.

The underlying benchmark comes from the UK AI Security Institute, a state-backed research body testing the capabilities and risks of advanced models. The test is a 32-step simulated corporate-network intrusion spanning four subnets and roughly 20 hosts, something the institute estimates would take a human expert around 20 hours. No defenders or defensive tooling get in the way. That matters. This is not Microsoft inventing a sexy benchmark for a marketing deck, but it is still a lab. It measures potential capability, not actual field results.

Microsoft also points to the institute’s finding showing leading open-weight models now sit roughly four to seven months behind the closed frontier on cyber tasks. Models anyone can download, modify, and run privately are approaching capabilities the leading hosted models had only a few months earlier. The distinction matters because a provider can monitor a closed model, change its safeguards, suspend an account, or shut access down. Once the weights are out, those controls disappear.

Four to seven months is not a prophecy or some immutable countdown. It is a preparation window. The frontier demonstrates a capability under controlled access, and only a few months may separate it from models an adversary can run privately without the same provider controls. Open-weight models still lag behind, but nowhere near as far back as I expected.

3. Threat actors are logging in, not breaking in. The AI noise buries this finding, and it is not new at all. User execution accounted for 30 percent of observed initial access, and valid accounts another 20 percent. Half of all initial access came from someone executing something they should not have, or a threat actor simply using legitimate credentials. Phishing as an initial access vector tripled, from 7 to 23 percent.

ClickFix alone, the copy-paste-into-your-terminal trick I recently examined, ran on more than 1.1 million devices, a whopping eight-fold jump between February and May. This is social engineering and identity, not malware wizardry. The perimeter is a log-in screen.

This lines up with the eCrime ecosystem, where infostealers and initial access brokers work in unison to harvest credentials. Those valid credentials are then sold on dark web markets for as little as US$5 per credential pack. This has been happening for years and is in no way novel. My cynical take is Microsoft wants to push customers deeper into its identity security tooling more than it is flagging something unique. CrowdStrike and Google have written about this for the past few years.

4. More intelligence does not help without the ability to act on it. Microsoft gives a chapter to a problem vendors rarely admit. Organizations are drowning in threat data and cannot convert it into decisions fast enough to matter. This is the quiet finding, and for anyone running a security operations center it may be the truest. The bottleneck moved. It is no longer how much a team can see. It is how fast the team can act on what it already saw.

To be fair, Microsoft does say one thing here I completely agree with. Another feed or dashboard does not close this gap. Good. Someone in Redmond gets it. More intelligence is not the answer for an organization already incapable of operationalizing what it has.

This largely lines up with almost every customer conversation I have ever had about threat intelligence, and I have made the same argument myself. Hearing it from Microsoft, which does not have a prolific or even all-that-useful cyber threat intelligence product, is quite bizarre. Had CrowdStrike, Google, or Recorded Future said this in their yearly reports, I would likely weight it rather high.

This reeks of Microsoft marketing. Even so, the foundation of the finding is true. There is a very thin line between too much intelligence and too little. Most organizations are overwhelmed by CTI and unsure how to act on it or integrate it into their decision-making workflows.

5. The scariest number in the report is not a CVE count or some new threat vector. It is something insanely old in a threat landscape moving this fast. Microsoft analyzed the five most-detected CVEs in its data. Among those five, 58 percent of detections traced to a single vulnerability, CVE-2020-1472, better known as Zerologon. Disclosed in 2020. Patched in 2020. Still the most detected bug on the list in 2026.

I would normally act surprised, throwing out WTFs left and right. But Zerologon remaining a huge issue is no surprise. There was an intense patching push back in 2020, including a CISA emergency directive ordering federal agencies to patch within days, and apparently the urgency did not stick. The 58 percent figure is easy to misread. It is not 58 percent of all attacks. It is 58 percent of detections across the top five CVEs Microsoft tracked. The scoping does not soften the point. It sharpens it. Line up the handful of vulnerabilities doing the most work, and a six-year-old one is still winning.

Zerologon is not clever. It is a Netlogon privilege escalation flaw with a patch available for six years. Its continued dominance says nothing about threat actor capability. It says everything about defender inertia. The tools changed. The behavior did not. Patching remains woefully behind in far too many organizations.

Put findings one and five beside each other and the contradiction is brutal. Defenders are now racing an exploitation clock measured in hours while still getting hit by a vulnerability patched six years ago. They are simultaneously too slow for tomorrow’s bug and not finished with yesterday’s. AI does not create this mismatch. It accelerates the side already moving faster.

The deeper problem is organizational latency, not technology. The industry keeps treating cyber security like a technology race. The threat actor can compress discovery, testing, targeting, and execution toward machine speed. The defender still has asset owners, maintenance windows, approval chains, outage risk, legacy dependencies, change-control boards, and a ticket somebody promises to look at next Tuesday. Machine-speed offense is colliding with human-speed governance.

Every old vulnerability left standing becomes inventory for a faster adversary. Every stolen credential becomes something automation can test sooner. Every piece of intelligence sitting in a dashboard instead of changing a decision becomes time handed back to the threat actor. AI does not need to invent a brilliant new way through the front door when defenders keep preserving perfectly good old ones.

Read together, the five findings argue against the report’s own headline. AI is real in here, and it is accelerating. But the mechanics of compromise did not change. Threat actors still gain initial access through people, through credentials, and through vulnerabilities organizations could have closed years ago. AI just lets them do it faster, cheaper, and at greater scale.

The uncomfortable takeaway is the defenses mattering most are the least exciting. Patch the old thing. Watch the log-in. Shorten the distance between seeing a threat and acting on it. None of it trends on launch day. All of it would have stopped most of what Microsoft documents.

The robots are coming, but the embarrassing part is they may not need anything particularly futuristic when they arrive. They already wield machine speed. Give them a stolen credential and six years of defender inertia, and they can do plenty with what you already failed to fix. They will merely walk in through the door you left unlocked in 2020.
