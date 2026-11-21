---
id: collect-261121-veille-ia/crypto-pqc/binance-hack-north-korea-s-human-risk-playbook
title: "Binance Hack: North Korea's Human Risk Playbook"
domain: crypto-pqc
role: reference
task: article
actors: []
dates: []
keywords: ["binance", "more", "north", "korea", "facestealer", "target", "hack", "billion"]
source: https://nisos.com/blog/binance-hack-north-korea/
source_anchor: ""
source_lines: [1, 108]
section: "Crypto_PQC"
---
Blog

Nation-state threats

# The Binance Hack and North Korea's Playbook: When the Attack Surface Is a Person

### Key takeaways

- On October 29, North Korean actors stole more than $6 billion from Binance, the largest crypto theft on record.
- A separate DPRK campaign, which the FBI calls FaceStealer, uses AI deepfakes and stolen video to impersonate people victims already trust. The FBI says it has taken more than $400 million in 18 months.
- Both cases point to the same pattern: attackers target people first and systems second.
- Verifying who is on the other end of a meeting, a job offer, or a hiring process is becoming a critical security control.

## What happened in the Binance hack

On October 29, an attacker moved roughly $6.36 billion out of Binance wallets across ten blockchains. Most of it left in about 20 minutes, according to TRM Labs. That is about four times the size of the Bybit theft in February 2025.

TRM attributes the attack, with high confidence, to TraderTraitor, North Korea's elite hacking unit. The Department of Justice describes TraderTraitor as members of the North Korean military. It says they stole more than $6 billion in virtual assets from Binance.

The speed of the theft is what stands out, but the preparation matters more. TRM found that staging began at least 36 hours before the theft. The attacker tested and rehearsed the code, then installed it on a Binance hot wallet. TRM also says the evidence points to a compromise of Binance's wallet signing infrastructure, because valid signatures were produced across chains with different key types. Binance has not yet published a root cause.

Law enforcement and industry moved fast too. More than $2.7 billion has been frozen. The DOJ has already obtained court authorization to seize or restrain more than $1.2 billion of that. The rest of the stolen funds are still being laundered through bridges, mixers, and exchanges.

## FaceStealer: how North Korea uses AI deepfakes for social engineering

The DOJ announced two actions together. One covers the Binance theft. The other targets FaceStealer, the FBI's name for a social engineering scheme that has cost victims more than $400 million in 18 months. The DOJ press release treats the two as separate cases. It also says FaceStealer is now aimed at people who build and maintain open-source software.

The scheme works like this:

1. **A trusted contact reaches out.** The invitation to a video meeting comes from an account stolen from a previous victim.
2. **The meeting looks real.** The attackers record the target's webcam and play prerecorded video of someone the target knows. They also use AI-generated deepfakes of well-known figures in crypto and web3.
3. **A fake technical problem appears.** The hackers claim the target can't be heard and offer a "fix."
4. **The fix is malware.** Once installed, it gives the attackers access to the target's data, accounts, and contacts.
5. **The cycle repeats.** The attackers mine each victim's contacts to find the next, more valuable target. In several cases they waited months, or more than a year, before acting.

The FBI seized more than 900 webcam videos of FaceStealer victims. The FBI's Los Angeles field office put it plainly:

"If you can't imagine a scenario where you might fall prey to it, you are not thinking creatively enough."

## A fake recruiter, a real breach: the human side of DPRK cyber threats

The people behind FaceStealer rely on the lure of a job or an investment. Tom's Hardware points to the March 2026 compromise of the Axios JavaScript library, which sees about 100 million downloads a week. The article says the developer was approached by someone posing as a company interested in hiring him. The attacker copied a real CEO's persona and built a Slack workspace with believable channels. They then invited him to a Teams meeting with multiple "people."

Tom's Hardware's coverage also ties the Binance breach to a compromised developer. The DOJ treats the two cases as separate efforts. The exact path into Binance is still unconfirmed, so it's worth keeping those threads apart until Binance publishes its findings.

The point holds either way. In both cases, the attackers didn't break through a firewall. They persuaded a person.

## Why traditional security tools miss threats that start with people

Traditional endpoint detection, network monitoring, and access controls watch what happens inside your environment. FaceStealer begins outside it. It starts with a message from a familiar name, a recruiter on a professional network, or a meeting invitation from a real colleague's hijacked account. By the time your tools see something, a trusted person has already installed the malware.

This is the gap human risk intelligence addresses. Many threats originate from people, and they target people. Closing the gap means looking outside the firewall, at the personas, relationships, and signals that tools inside the firewall can't see.

## What security teams can do: make identity verification a control

You can't patch trust. You can, however, make it harder to exploit. Here is where to start.

- **Treat unexpected meeting invitations as unverified.** This includes invitations from known contacts. Confirm through a second channel before joining or installing anything.
- **Never install a "fix" mid-meeting.** A technical error that prompts a download should end the call. Route the fix through IT.
- **Scope your protective intelligence. I** dentify who has the most access: engineers, developers with publish rights to code repositories, finance and treasury staff, executives, and the assistants who manage their calendars. FaceStealer lures are built from real relationships and real context, so look at what an attacker can learn about these people from public sources before they do.
- **Check the meeting link, not just the name.** According to Tom's Hardware, the fake meetings run on attacker-controlled servers with typo-squatted domains like "uu01webzoom.us" or "teams.livemeet.us." Train people to read the URL before they join.
- **Lock down publishing and CI/CD access.** The DOJ says the attackers go after the software repository accounts that open-source maintainers control, which sets up software supply-chain attacks. Use hardware-backed authentication, separate publishing credentials from daily-use devices, and limit who can push releases.
- **Assume a trusted contact's account may be compromised.** The attackers send invitations from accounts stolen from earlier victims. A familiar name proves nothing, and a familiar face on video proves nothing either.
- **Plan for a long dwell time.** The DOJ notes that in several cases the hackers waited months, or more than a year, before stealing funds or hijacking accounts. If someone on your team joined a suspicious meeting last year, treat it as a potential incident now.
- **Brief your people without blaming them.** The FBI says FaceStealer can deceive "even the most experienced technical and security conscious computer user." Awareness training works best when it says so plainly.

## Why HR belongs in the conversation on DPRK cyber threats

FaceStealer often begins with a job offer. In the Axios case, the lure was a recruiter-style approach from a company that appeared to want to hire the developer. Your employees' job-seeking behavior, public profiles, and professional networks are part of your attack surface. If you're in HR or talent acquisition, you're also on the receiving end of the reverse problem. DPRK-linked operators don't only pose as recruiters. They also pose as candidates, using synthetic identities and AI-generated credentials to get hired. Nisos has researched DPRK-linked IT worker fraud, and the pattern is consistent: the attacker's identity looks credible until someone checks who is really behind it.

Security and HR are looking at two sides of the same threat, and a few practices help both teams:

- **Give employees a way to report odd outreach.** If a developer gets an unusual job pitch or a meeting invitation that feels off, they should know who to tell and expect a quick, judgment-free response.
- **Verify the person, not only the paperwork.** Document checks confirm that a record exists. They don't confirm that the person on the video call is the person named in it. For sensitive roles, add steps that corroborate identity through independent sources.
- **Share signals across teams.** When security sees a suspected impersonation attempt, HR should know. When HR spots a candidate whose identity doesn't hold up, security should know.
- **Keep it scoped and defensible.** Focus on defined roles and specific, documented risks rather than blanket scrutiny of the workforce. Bring Legal in early, especially for hiring decisions where FCRA and privacy rules apply.

## Corroborating identity: what the Binance hack reveals about human risk

Both stories end the same way. Someone trusted a person who wasn't who they claimed to be. In FaceStealer, that was a deepfaked colleague or a recorded video of a real one. In the Axios case, it was a cloned CEO persona and a convincing Slack workspace.

AI makes these personas cheaper to build and harder to spot. That's why identity verification can't stop at the login screen or the background check. Teams need to be able to answer a simple question: **who is this person, really, and what else can we find out about them?**

That question is about attribution: tying a digital persona to a real human, and a real human to their wider footprint. It requires looking outside the firewall, at public records, social media, breach data, and open-source signals that your internal tools never see. This is the idea behind identity integrity and the core of human risk intelligence: understanding threats to people and threats by people, and acting before they reach your perimeter.

## What the Binance hack means for your security program

The attackers behind the Binance theft prepared for at least 36 hours, according to TRM. The people behind FaceStealer sometimes wait more than a year. These operations require patience and preparation, and the DOJ says they steal to fund the DPRK regime and its weapons program. Technical controls matter, but they won't be enough on their own against an attacker who starts by befriending your engineer.

Three questions can guide your next review:

1. **Who in our organization could be approached this way?** Think of developers, finance and treasury teams, executives, and anyone with privileged access.
2. **How would we know if a trusted contact or a new hire wasn't who they claimed to be?** If the answer is "we wouldn't," you've found a gap.
3. **Can we see risk outside our own environment?** If every signal you have comes from inside the firewall, you'll learn about these threats after the damage is done.

## Know who is on the other end

The Binance theft happened in minutes, but the groundwork took longer. FaceStealer operators sometimes wait a year or more. The common thread is that someone trusted a person who wasn't who they seemed.

Nisos helps security and HR teams answer a simple question: who is this person, really? We work outside the firewall and use publicly and commercially available information to connect personas to people. If you want to see what an attacker could learn about your engineers, finance team, or executives, or you need to corroborate the identity of a high-risk hire, see how Identity Integrity works or talk to our team.

## Frequently Asked Questions

Platform

## See how Nisos works

Take a guided tour of the platform, or explore the solutions built for your team.
