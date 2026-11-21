---
id: collect-261121-veille-ia/muse/avoid-meta-muse-ai-zero-day-fixed-three-reasons-remain
title: "Avoid Meta Muse AI? Zero-Day Fixed, Three Reasons Remain"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: []
keywords: ["meta", "metas", "muse", "zero-day", "account", "says", "access", "setting"]
source: https://tech-insider.org/4-reasons-avoid-meta-muse-ai-2026/
source_anchor: ""
source_lines: [1, 73]
section: "Muse"
---
The case against Meta’s Muse agent, which Meta launched on September 8, 2026, comes down to four reasons: the access it asks for, an always-on cloud machine, a Mac zero-day, and Meta’s privacy record. Sorted against the sources, the zero-day is the one a hotfix has closed. The other three rest on design choices Meta defends, a training setting that is on by default, and a Confidential VM that Meta’s help page still describes as a future option, as checked on November 12, 2026.

| Reason | What the record shows | What Meta says | Status as checked November 12, 2026 | 
|---|---|---|---|
| Broad permissions | CNET lists the ask as passwords, full disk access, email, calendar, contacts, Notes and WhatsApp chats. A journalist who withheld Messages access reportedly still had messages synced. | Meta’s launch post says people can change access or disconnect a service whenever they want. CNET reports that Meta disputed the Messages claim. | Design choice Meta defends; Messages case reported by Kaspersky and CNET, with Meta’s dispute noted by CNET | 
| Always-on cloud machine | Each user gets a dedicated cloud computer. Muse’s creators write that the agent is not fully protected against prompt injection (Kaspersky). | Meta’s launch post says each Muse runs on its own dedicated Secure Virtual Machine and that nothing Muse does reaches the internet unless the Sentinel approves it. A Confidential VM is promised for later in 2026. | Design choice Meta defends; Confidential VM still described as a future choice | 
| Mac zero-day | An undocumented dictation setting let any local process redirect transcription and reach the account token. | Meta’s David Singleton called it a local privilege escalation, not a remote exploit, and said a hotfix was issued. | Patched | 
| Meta’s privacy record | Training on conversations is on by default, Surfshark counts 31 of 35 data types, conversation sharing was paused, and a human call-handling test ran without warning. Kaspersky reports Meta shut that test down and called running it unannounced a mistake. | Meta says people can opt out of training. | Opting out of training is left to the user; sharing paused October 20, 2026 | 

Muse launched in the US on iOS, Android and muse.ai, with AI glasses listed as coming soon. Our Muse launch report covers the launch in full. Meta says it built Muse to be “safe, secure, private, and widely available”, and Kaspersky, citing Meta’s figures, reports more than 10 million downloads by October 21, so the questions below apply to a large installed base.

## The Mac zero-day: a hidden dictation setting, an account token and a quarrel over the word local

Security researcher Patrick Wardle disclosed the flaw on September 21. According to The Register, the Mac app had an undocumented setting called endo_voyager_dictation_endpoint that code running locally could change without special privileges. Wired explains the effect: the setting let processes change the endpoint where transcription happens, normally a Meta server, and pointing it elsewhere gave any app or terminal command access to the token that authenticates a user to their Muse account. That token gives control of the account.

The disagreement is over how much comfort the word “local” gives. Meta’s David Singleton said, “This was a local privilege escalation attack, not a remote exploit,” and added that misuse needs malicious code already running under the user’s account. Wardle’s answer, per Wired, is that a simple variation of a ClickFix attack, the trick of getting a person to run a command themselves, is all it takes to take over a Muse account. CNET reports that Wardle disagrees with Meta’s local-only framing and says remote exploitation is possible. Wired reports Wardle’s account of the effect: anyone exploiting the flaw could manipulate the agent and use its privileges to do whatever they wanted, so an attacker did not need to write Mac malware of their own.

Both statements can be true. Meta’s description of the mechanics is thin as reassurance, because the precondition (code running as you) is exactly what a ClickFix lure supplies. Wardle also calls the exposure a design problem. The Register reports that he points to Apple’s on-device dictation, and that had Meta used that API the vulnerability would not exist. He argues, per The Register, that when agents hold broad permissions, endpoint detection products cannot tell whether commands come from the user, an agent or an attacker, which matters for any IT team.

The route itself is closed. InfoQ reports that Meta’s hotfix stripped the internal setting from production macOS builds, and VentureBeat says Meta hot-fixed the Mac app within a day by removing the setting from production builds. The hotfix removed one setting; Wardle’s wider argument about agents and cloud dictation concerns design, not that setting. Our report on the hidden Muse setting and its patch covers the disclosure in detail, and our report on Muse’s video avatars and Mac control covers more of the Mac app. Of the four reasons, this is the one a current Mac build has already answered.

## Where Muse is reported to have reached past a grant: Messages, a Marketplace address and human call handlers

Meta’s product page says Muse “checks with you before important actions like sending an email or making a purchase”, and its launch post says Muse has no visibility into passwords or payment methods: credentials a person shares go into secure storage, so Muse can use them without seeing them. Meta also says users choose which services Muse connects to. Those statements cover actions and credentials. They do not speak to message content, and that is where the strongest counter-evidence sits.

Kaspersky reports that journalist Jason Aten installed Muse on his iPhone and Mac and deliberately withheld access to Messages. Muse had still been syncing from the local message database on his Mac, and the sync had reached row 187,462. CNET describes the same case as roughly 187,000 lines of Apple Messages synced with Full Disk Access turned off, and notes that Meta disputed the claim. CNET, drawing on AppleInsider, adds a point no setting fixes: “Even people who refuse to use Muse may have their data collected by the AI if they text someone who does have it installed.” Our report on the Messages privacy dispute covers that case in more detail.

CNET also reports that Muse shared the home address of Toronto tech reviewer Matt Robb with a potential buyer on Facebook Marketplace. Neither this nor the Messages case, as reported, is a software vulnerability in the Wardle sense. Both describe the agent doing something its owner did not intend, which is the failure the “checks with you” promise is meant to prevent.

Kaspersky reports a third episode: Meta had been testing a handoff of Muse’s phone calls to trained call-center operators with its own employees. After criticism, Meta shut the experiment down, and, according to Kaspersky, company representatives said running it without warning was a mistake and promised to improve the feature. Kaspersky also notes that Muse’s creators write that the agent is not fully protected against prompt injections, meaning hidden malicious commands aimed at the AI. Taken together, the permissions reason looks stronger than the zero-day as of November 12, 2026, because these problems come from how the product is built and run.

## Collection breadth, training on by default and the sharing links Meta paused

Surfshark, counting the data types each app discloses in the Apple App Store, found that Muse collects or tries to collect 31 of 35 listed types, second overall among AI chatbots. It puts the average AI chatbot app at 13.

| Assistant | Data types collected, of 35 | 
|---|---|
| Meta AI | 33 | 
| Meta Muse | 31 | 
| Google Gemini | 24 | 
| ChatGPT | 17 | 
| Average AI chatbot app | 13 | 

The count measures breadth of data types, not how much data is taken or how it is used, so it supports the privacy reason without proving misuse.

Training is the setting most readers can act on. Kaspersky reports that training on user data is on by default and that you turn it off under Settings, then Data controls, then “Help improve our AI models”. Meta’s help page gives the same route and says that where training is allowed, it still removes names, email addresses, phone numbers and social security numbers. CNET’s summary is that Meta feeds conversations into training by default and leaves the opt-out to the user.

Conversation sharing was a separate exposure. TechSpot reports that Meta paused the feature on October 20, 2026, after security researchers discovered thousands of links in public DNS records; the researchers identified more than 6,800 share hostnames. Shared pages could expose users’ personal files and companies’ internal documents. If you shared a Muse conversation earlier, treat the link as exposed.

Meta’s September 8 launch post promised a Muse Confidential VM for later in 2026, in which the whole VM, including a person’s data and conversations, is encrypted with a key only the user holds, so that, in Meta’s words, not even Meta can access it. Meta’s help page, checked on November 12, 2026, still words it as a choice users will have in the future.

## Tier prices, model access and where Muse is offered

Meta’s Help Center lists three tiers: a free tier with a usage limit, a Power plan at $20 a month with 500M Muse tokens a week, and a Maximum plan at $100 a month with 3B Muse tokens a week. The paid tiers are described as weekly token allowances, so the free tier is a low-cost way to see how Muse behaves before you grant it broad access.

Muse Code, the coding product, is priced separately. Meta’s Muse Code page lists Everyday at $5, High at $15, Power at $50 and Maximum at $200 a month, and says the Power Usage plan includes access to the most powerful Muse models, including Muse Wonder Full. Our report on the Muse custom voices leak also covers the Muse Code plan prices.

Meta’s Muse Wonder announcement says Muse Wonder is available through the Meta Model API and Muse Code, while Muse Wonder Full is available for Muse Code subscribers. Meta’s Muse Wonder documentation says Wonder Full is available only in Muse Code with a subscription, lists it under the Power Usage and Maximum Usage plans, and shows no public API access for it, so check which model a plan includes before paying.

Shopping has its own limit. Wired reports that roughly 12 hours before Wardle disclosed the zero-day, Amazon started blocking people from using Muse to shop on its site, and users who tried saw a message calling Muse an “unauthorized AI agent [that] violates Amazon’s Conditions of Use.”

Outside the US, Meta announced Singapore availability on October 27, on iOS, Android and muse.ai, with Gmail, Google Calendar and Instagram connectors. Early local connector partners are Shopee, Grab, Singapore Airlines, Agoda, ShopBack, Klook, Trip.com and Lazada. Each connector is another place for the access questions above to apply.

## A grant order for Mac and phone, and when an IT team should hold

Our assessment, as of November 12, 2026: the zero-day is the weakest reason to skip Muse, because a hotfix closed it. The strongest reported case is the Messages one, because Kaspersky and CNET describe messages syncing with access switched off, which is the one thing a permission screen is supposed to prevent; CNET notes that Meta disputed the claim, so it is reported, not established. Training on by default is the cheapest to deal with, since it is a single toggle. A team that manages Macs for other people has a firmer reason to hold Muse back for now: it acts with the user’s privileges, Wardle says endpoint tools cannot tell an agent from an attacker, Meta’s help page still words the Confidential VM as a future choice, and Muse’s creators say the agent is not fully protected against prompt injection. If you go ahead, grant access in this order:

1. Update the Mac app first and confirm your build is newer than Meta’s September 2026 hotfix, which, InfoQ reports, removed the setting from production builds.
2. Turn off training before connecting anything: Settings, Data controls, “Help improve our AI models”, Turn off.
3. Do not paste a command from a web page or chat to “fix” or “verify” anything. ClickFix is the delivery route Wardle describes.
4. Keep conversations that touch personal or work files out of shared links if the feature returns.
5. Connect one app at a time, starting with low-stakes ones such as calendar, and disconnect any you stop using; Meta says people can change access or disconnect a service whenever they want.
6. Leave Messages, financial apps and Full Disk Access for last, or skip them: Aten’s case, as Kaspersky reports it, suggests a missed grant could still leak, so check what Muse reaches after each connection.
