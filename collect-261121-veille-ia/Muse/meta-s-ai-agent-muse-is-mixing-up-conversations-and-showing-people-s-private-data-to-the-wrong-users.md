---
id: collect-261121-veille-ia/muse/meta-s-ai-agent-muse-is-mixing-up-conversations-and-showing-people-s-private-data-to-the-wrong-users
title: "Meta's AI Agent Muse Is Mixing Up Conversations — and Showing People's Private Data to the Wrong Users"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: []
keywords: ["muse", "agent", "meta", "balance", "reuters", "conversations", "private", "wrong"]
source: https://dev.to/paullovelace/metas-ai-agent-muse-is-mixing-up-conversations-and-showing-peoples-private-data-to-the-wrong-1bb7
source_anchor: ""
source_lines: [1, 56]
section: "Muse"
---
Imagine asking your AI assistant about the weather and getting, unprompted, someone else's bank balance in reply. Not your balance. Someone else's. That is what Meta employees say is happening inside Muse, the company's AI agent — and if the reports are right, it is the kind of bug that should make every developer building agentic AI stop and think.

A Reuters investigation published November 18, 2026, followed by TechCrunch coverage on November 19, reports that Muse has been mixing up conversations between different accounts and surfacing private information to the wrong people. Meta calls the incidents isolated. The details in the reports are specific enough to take seriously — and honest enough about what is not yet proven that the story deserves a careful read, not a panic.

According to Reuters, Meta staff described the problem in internal posts: Muse showed them childcare requests, career advice, product orders and bank balances that were not theirs. One employee's chat about weather and tides suddenly included the line: "BTW, two bank updates, your Capital One balance is $2722.58, your Charles Schwab balance is $5899.57." The balances were not his.

Another employee reported getting a reminder on her personal phone to reply to her sister's message on Signal — an end-to-end encrypted app that Muse should have no way of reading. In a separate test of the Signal issue, Reuters found the problem was still happening.

Muse launched in September 2026 and has passed 10 million downloads. It is Meta's big bet on the agentic AI race — an assistant that acts on your behalf across apps and services. That ambition is exactly what makes this kind of failure so consequential: an agent that acts for you needs to know, with absolute certainty, who "you" are.

Meta spokesperson Daniel Roberts described the incidents as "isolated" and said many of them were hallucinations that have already been fixed. That is a meaningful defence, and it is worth taking seriously: large language models do invent plausible-looking numbers, and a fabricated bank balance that looks real is not the same thing as a leaked one.

Here is the honest uncertainty, and it matters: Reuters itself could not confirm whether the balances shown were real data belonging to another person, or hallucinations. The distinction is everything. A hallucinated balance is a quality bug — embarrassing, but contained. A real balance shown to the wrong account is a privacy breach — a different category entirely, with different legal and trust consequences.

What is harder to wave away is the Signal case. A reminder about a message on an encrypted app, in a test where Reuters found the behaviour still occurring, points at something more structural than a one-off hallucination: the agent appears to be crossing boundaries between accounts and apps that should be sealed.

This is not the first time Muse's boundaries have looked porous. In October 2026, researchers found that Muse's share links could be discovered through passive DNS — around 6,800 conversations exposed to anyone who knew where to look. That was a sharing design flaw: links that were meant to be private were effectively public.

The new reports describe a different failure mode — the agent itself confusing one user for another — but the pattern is the same. When an AI agent is given access to your messages, your accounts and your apps, the walls between users have to be perfect. "Mostly perfect" is not a passing grade.

For anyone building with agentic AI, this is the lesson to take from the story, whatever the final verdict on the balances turns out to be:

- 
**Identity matching is not a feature, it is the foundation.** An agent that books, orders, messages and pays on your behalf has to resolve "which user" correctly on every single turn, not just at login. A conversation that starts with one identity and silently drifts into another is the worst kind of bug — the user sees a confident answer and has no reason to doubt it.
- 
**Permissions need to be enforced at the data layer, not the prompt layer.** If the agent can even*see* another account's data in its context window, the system is one bad retrieval away from leaking it. Access control has to happen before the model, not inside it.
- 
**Cross-app access multiplies the blast radius.** The Signal example is the scary one. Each app an agent can touch is another boundary that has to hold. Every integration you add is another place where a mix-up can surface someone's private life to the wrong person.
- 
**Hallucination and leakage look identical to the user.** A wrong balance is a wrong balance. The person staring at it cannot tell whether it was invented or stolen — and neither can you, from the outside. That is why "it was probably a hallucination" is a weak defence for a product that handles real financial data.

Let's be straight about what we do not know:

- 
**Reuters could not verify the balances.** We do not know whether the dollar figures were real data from another account or hallucinated numbers. Do not repeat the claim as settled fact — it is not.
- 
**Meta says many cases were hallucinations and are fixed.** That may be true for some of them. The Signal test, where the problem was still reproducible, is the counterweight to that claim.
- 
**We have not tested Muse ourselves.** This is researched analysis of the Reuters investigation and the TechCrunch report, not hands-on testing. The scale of the problem — how many users, how often — is still unknown.

None of that makes the story unimportant. Even in the best case — every balance a hallucination, every case fixed — an agent that confidently presents invented financial data as fact is a product problem. In the worst case, it is a privacy failure at the heart of Meta's biggest AI bet. Either way, the engineering question is the same: how do you prove, in a system built on probabilistic generation, that this conversation belongs to this user and no one else?

1. 
**If you build agentic features, audit your identity resolution.** Trace a single request end to end: how does the system know which user it is serving at every step, and what happens if a retrieval returns another user's data? If the answer is "the model should know," that is not an answer.
2. 
**Enforce permissions before the model sees data.** Scope every retrieval, every tool call and every app integration to the authenticated user at the data layer. The model should never receive data it is not allowed to show.
3. 
**Treat cross-account contamination as a top-severity bug class.** Test for it deliberately: seed test accounts with distinctive data, run multi-account scenarios, and check whether anything from account A ever surfaces in account B's session.
4. 
**If you use Muse, check what it knows about you.** Review connected apps and permissions, and treat any answer that mentions data you do not recognise as a red flag — report it, do not dismiss it.
5. 
**Watch for Meta's follow-up.** The company has called these isolated incidents. The test that matters is whether independent checks — like the Signal test Reuters ran — stop reproducing the problem.

*Sources: Reuters, "Meta bets big on Muse AI agent, early safety mishaps surface," November 18, 2026; TechCrunch, "Meta's AI agent Muse is mixing up conversations and exposing private data, report says," November 19, 2026. Reuters could not confirm whether the bank balances shown were real data belonging to another person or hallucinations; Meta described the incidents as isolated and said many were hallucinations that have been fixed. This article is researched analysis, not hands-on testing.*

## Top comments (0)
