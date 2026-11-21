---
id: collect-261121-veille-ia/regulation-eu/eu-ai-act-article-5-prohibited-practices
title: "EU AI Act Article 5: Prohibited Practices"
domain: regulation-eu
role: reference
task: article
actors: []
dates: ["2025-02-02"]
keywords: ["point", "article", "points", "harm", "social", "practices", "behaviour", "person"]
source: https://venvera.com/learn/eu-ai-act/article-5-prohibited-practices
source_anchor: ""
source_lines: [1, 119]
section: "Regulation_EU"
---
Article 5 of the EU AI Act lists the AI practices the Union bans outright. Almost every guide you will find says there are eight of them. As of the consolidated text there are ten lettered points, because Regulation (EU) 2026/1744, the Digital Omnibus on AI, inserted two new ones and two paragraphs that define how far they reach.

The eight original points have applied since 2 February 2025. The two new points, (ba) and (bb), together with the new Article 5(1a) and 5(1b), apply from 2 December 2026. That is the number that belongs in your plan today: they are not yet in force, and the window to screen for them is now.

| Point | What it prohibits | Applies from | 
|---|---|---|
| Art. 5(1)(a) | Subliminal, purposefully manipulative or deceptive techniques that materially distort behaviour and cause or are likely to cause significant harm. | 2 Feb 2025 | 
| Art. 5(1)(b) | Exploiting vulnerabilities due to age, disability or a specific social or economic situation, with the same distortion and harm test. | 2 Feb 2025 | 
| Art. 5(1)(ba) | Generating or manipulating realistic intimate or sexually explicit material of an identifiable person without their explicit consent. | 2 Dec 2026 | 
| Art. 5(1)(bb) | Generating or manipulating the material or performance defined in Article 2, points (c) and (e), of Directive 2011/93/EU, subject to the national without right defence. | 2 Dec 2026 | 
| Art. 5(1)(c) | Social scoring: evaluating people over time on social behaviour or personal characteristics, leading to detrimental treatment. | 2 Feb 2025 | 
| Art. 5(1)(d) | Predicting the risk of a person committing a criminal offence based solely on profiling or personality traits. | 2 Feb 2025 | 
| Art. 5(1)(e) | Creating or expanding facial recognition databases through untargeted scraping of facial images from the internet or CCTV footage. | 2 Feb 2025 | 
| Art. 5(1)(f) | Inferring emotions in the workplace and in education institutions, except for medical or safety reasons. | 2 Feb 2025 | 
| Art. 5(1)(g) | Biometric categorisation to deduce race, political opinions, trade union membership, religious or philosophical beliefs, sex life or sexual orientation. | 2 Feb 2025 | 
| Art. 5(1)(h) | Real time remote biometric identification in publicly accessible spaces for law enforcement, outside three narrow objectives. | 2 Feb 2025 | 

## What does Article 5 of the EU AI Act prohibit?

Ten lettered points, grouped into four families once you read them next to each other.

### Manipulation and exploitation

Points (a) and (b) are the behavioural pair. Point (a) covers subliminal techniques beyond a person's consciousness and purposefully manipulative or deceptive techniques that appreciably impair the ability to make an informed decision. Point (b) covers exploiting vulnerabilities arising from age, disability or a specific social or economic situation. Both require the same two elements: material distortion of behaviour, and significant harm that is caused or reasonably likely.

That harm test is what most inventories get wrong. Persuasive design is not automatically prohibited. A system becomes a candidate for point (a) when the manipulation is subliminal or purposefully deceptive and the resulting decision is likely to cause significant harm.

### Scoring and prediction

Point (c) prohibits social scoring where the score leads either to detrimental treatment in contexts unrelated to where the data was collected, or to treatment that is unjustified or disproportionate to the behaviour. Point (d) prohibits assessing or predicting the risk that a person will commit a criminal offence based solely on profiling or personality traits, and carves out systems that support a human assessment already grounded in objective, verifiable facts linked to criminal activity.

### Biometrics

Point (e) bans untargeted scraping of facial images from the internet or CCTV to build or expand facial recognition databases. Point (f) bans emotion inference in workplaces and education institutions, with an exception for medical or safety purposes. Point (g) bans biometric categorisation used to deduce sensitive characteristics, while expressly not covering the labelling or filtering of lawfully acquired biometric datasets.

Point (h) is different in kind: it prohibits a use rather than a placing on the market, and only real time remote biometric identification in publicly accessible spaces for law enforcement. Three objectives survive: targeted search for victims of abduction, trafficking or sexual exploitation and for missing persons; prevention of a specific, substantial and imminent threat to life or physical safety or a genuine and present or foreseeable terrorist attack; and localisation or identification of a suspect for offences listed in Annex II that carry a maximum custodial sentence or detention order of at least four years in the Member State concerned. Each use needs prior judicial or independent administrative authorisation, a fundamental rights impact assessment under Article 27, and registration in the EU database under Article 49.

### Synthetic intimate material

Points (ba) and (bb) are the new pair, and they are covered in the next section.

## What did Regulation (EU) 2026/1744 add?

Two prohibitions and two paragraphs that bound them.

Point (ba) prohibits placing on the market, putting into service or using an AI system that generates or manipulates realistic images, video, audio or similar material of an identifiable person's intimate parts, or of an identifiable person engaged in sexually explicit activities, without that person's freely given, specific, informed, unambiguous and explicit consent. The recitals name the target directly: the nudification applications that made an explicit prohibition urgent.

Point (bb) prohibits the same acts for AI systems that generate or manipulate the material or performance defined in Article 2, points (c) and (e), of Directive 2011/93/EU, which is child sexual abuse material and a pornographic performance involving a child, except where a without right defence applies under national law. The recitals give the intended shape of that exception: activities under domestic legal powers, such as material generated by authorities to investigate crime, and legitimate red teaming to test whether a system complies with the prohibition itself.

Both start on 2 December 2026, not on the date the amendment was published. That transition period is the whole planning window for anyone shipping a generative model.

## Who is caught: the provider or the deployer?

Article 5(1a) answers this, and it narrows the new prohibitions considerably. Read it before concluding that image generation is now banned, because it is not.

For providers, placing on the market or putting into service is prohibited in two cases only. First, where generating or manipulating that material is the intended purpose of the system. Second, where the system's design, training, architecture, capabilities or user facing functionality make that generation a reasonably foreseeable and reproducible outcome without significant technical modification, and the system lacks reasonable and adequate technical safety measures and other safeguards to reliably prevent it and to correct observed or reported misuse. The recitals list what those safeguards can look like: data cleaning, refusal training, safe prompt design, runtime guardrails, content classification and filtering, usage restrictions, abuse detection, and notice and action mechanisms.

For deployers, use is prohibited only where the deployer uses the system for the purpose of generating or manipulating that material. Lawful use of a system that happens to lack adequate provider side safeguards is not caught, and neither is accidental generation.

Article 5(1b) narrows point (ba) further: manipulating material in a way that does not increase the exposure of depicted intimate parts or alter the nature of depicted sexually explicit activities is not manipulation for these purposes. Changing a background or adjusting brightness on existing material is outside the prohibition. Increasing the level of exposure is inside it.

## What are the penalties, and who enforces them?

Article 99(3) sets the top ceiling for Article 5: up to EUR 35 000 000, or up to 7% of total worldwide annual turnover for the preceding financial year if the offender is an undertaking, whichever is higher. It is the highest tier in the Act and applies to nothing else.

Beneath it, Article 99(4) covers most operator and notified body obligations at up to EUR 15 000 000 or 3%, and Article 99(5) covers supplying incorrect, incomplete or misleading information to notified bodies or national competent authorities at up to EUR 7 500 000 or 1%. Article 99(6) then inverts the formula for SMEs including start-ups: each fine is capped at the percentage or the amount, whichever is lower.

Penalties are set and applied by Member States, which lay down the rules on penalties and other enforcement measures and notify them to the Commission. There is no single EU regulator issuing Article 5 fines. Our guide to EU AI Act penalties and fines works through the tiers in detail.

## What the other results get wrong

Three errors dominate the published guidance on Article 5.

The first is the count. Article 5(1) is still described almost everywhere as eight prohibited practices lettered (a) to (h). The consolidated text runs (a), (b), (ba), (bb), (c) to (h). If your screening checklist has eight rows, it is missing the two that start in December.

The second is overreading the new points. Headlines describing a ban on AI that can produce explicit imagery skip Article 5(1a) entirely. The provider prohibition turns on intended purpose or on foreseeable, reproducible output without adequate safeguards, and the deployer prohibition turns on the deployer's purpose. Capability alone is not the test.

The third is the date. Several summaries treat the amendment as effective on publication in July 2026. Article 113, as amended, applies points (ba) and (bb) and paragraphs 5(1a) and 5(1b) from 2 December 2026.

## Screen your own inventory

Fill this in against your AI system inventory. Any row you cannot answer is screening you have not done.

| Question | Your answer | Point it engages | 
|---|---|---|
| Does any system score people on behaviour or characteristics across unrelated contexts? |  | Art. 5(1)(c), live since 2 Feb 2025. | 
| Does any system predict individual criminal risk from profiling alone? |  | Art. 5(1)(d). The human assessment carve out is narrow. | 
| Do you infer emotion from staff or students anywhere? |  | Art. 5(1)(f), unless the purpose is medical or safety. | 
| Does any model you ship generate or edit images of identifiable people? |  | Art. 5(1)(ba) from 2 Dec 2026, read with Art. 5(1a). | 
| If so, can you evidence safeguards that reliably prevent and correct misuse? |  | Art. 5(1a)(a)(ii). This is the provider test, not the capability. | 
| Do you buy any of the above rather than build it? |  | Deployer use is caught by Art. 5(1a)(b) only where you use it for that purpose. | 

If most rows are blank, the next step is an inventory rather than a legal opinion. Our guide to EU AI Act policies and documentation sets out what a provider and a deployer each have to hold, and a free compliance check gives you a starting position across the EU AI Act obligations.

## Frequently asked questions

### How many prohibited practices are there in Article 5?

Ten lettered points in Article 5(1) of the consolidated text: (a), (b), (ba), (bb), and (c) to (h). Guidance written before Regulation (EU) 2026/1744 lists eight.

### When did the prohibitions start to apply?

Chapters I and II apply from 2 February 2025, with the exception of points (ba) and (bb) and Article 5(1a) and (1b), which apply from 2 December 2026. Every other application date in the Act is set out in our guide to EU AI Act deadlines and dates.

### Is real time facial recognition banned outright?

No. Point (h) prohibits real time remote biometric identification in publicly accessible spaces for law enforcement unless the use is strictly necessary for one of three listed objectives, and even then it needs prior judicial or independent administrative authorisation, a fundamental rights impact assessment and registration in the EU database.

### Does point (ba) ban general purpose image generators?

No. Under Article 5(1a) a provider is caught where the generation is the intended purpose, or where it is a reasonably foreseeable and reproducible outcome and the system lacks reasonable and adequate safeguards to prevent and correct it. A general purpose generator with effective safeguards is not the target.

### What if a practice also breaches other EU law?

Article 5(8) states that the Article does not affect prohibitions that apply where an AI practice infringes other Union law. A single system can be assessed under the AI Act and under data protection law at the same time, by different authorities.

## Primary sources

The text of Article 5, Article 99 and Article 113 above is taken from the consolidated text of Regulation (EU) 2024/1689 as amended, and the inserted points, limiting paragraphs and recitals from Regulation (EU) 2026/1744 of 8 July 2026, the Digital Omnibus on AI. Confirm the current consolidated text before relying on a specific point or date.
