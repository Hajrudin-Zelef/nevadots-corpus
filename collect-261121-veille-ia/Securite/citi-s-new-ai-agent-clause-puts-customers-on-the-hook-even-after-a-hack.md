---
id: collect-261121-veille-ia/securite/citi-s-new-ai-agent-clause-puts-customers-on-the-hook-even-after-a-hack
title: "Citi's New AI Agent Clause Puts Customers on the Hook, Even After a Hack"
domain: securite
role: reference
task: article
actors: []
dates: []
keywords: ["citi", "agent", "customers", "third-party", "agents", "bank", "section", "online"]
source: https://www.tftc.io/citi-ai-agent-liability-customers-section-h
source_anchor: ""
source_lines: [1, 61]
section: "Securite"
---
Citi buried a liability transfer in its November terms update, and the same playbook just ran at four other major banks.

**Key takeaways**

- Citi added Section H to its Online User Agreement in November 2026, making customers fully liable for any third-party AI agent's actions, including losses from credential leaks and hacks, while exempting Citi's own AI agents from the same rule.
- JPMorgan, Bank of America, PNC, and Citizens Bank adopted near-identical AI agent liability language within roughly one month, with no CFPB or OCC response issued.
- The carve-out for bank-owned AI is the tell: this is channel control dressed as a customer safety notice, and it has direct implications for anyone who wants to use independent fintech tools with their bank account.

Citi updated its Online User Agreement in November 2026 to add Section H, titled "Use of Third-Party Artificial Intelligence ('AI') Agents," making customers fully responsible for every action a third-party AI agent takes on their account, including actions taken after credentials are leaked or stolen. The update, first reported by TechCrunch on November 18, is not a Citi-only event: JPMorgan, Bank of America, PNC, and Citizens Bank have all adopted near-identical language within the same window, with no intervention from the CFPB or the OCC.

## What Section H Actually Says

The language in Citi's agreement is direct. If a customer gives account credentials to a third-party AI agent, "Citi will consider any access and actions taken by the agent as authorized by You." The hack clause is the part that should stop every reader: "If the Online Credentials that you provided to the third-party AI Agent are further shared, leaked, or otherwise distributed to other persons or AI agents, Citi will consider any access and actions taken with the Online Credentials as authorized by You."

Citi's separate Agreement for Online Funds Transfers, also updated in November 2026, adds matching language: agent-initiated transfers are deemed directly authorized by the customer.

A Citi spokesperson told TechCrunch: "We recently updated our Online User Agreement to provide greater clarity about those risks and to reinforce that customers who choose to grant account access to third-party AI agents are responsible for activities conducted through those agents."

PNC updated its Digital Services Agreement this week with similar terms. Citizens Bank emailed customers this week warning them they are responsible for AI assistant actions.

## The Carve-Out Is the Story

Citi's Section H explicitly exempts "any third-party AI Agent, tool, function or feature provided by Citi." The bank's own AI agents carry no additional customer liability. Only independent, third-party agents do.

This is a channel-control policy, not a security policy. Banks are pricing independent fintech AI out of the transaction layer while positioning themselves to sell their own agent products under a liability regime that favors the house. The TFTC piece on rogue AI agents flagged this structure before Section H had a name.

The Reg E problem sits underneath all of this, unresolved. The Electronic Fund Transfer Act limits consumer liability for unauthorized transfers to $50 to $500 depending on notification timing. Section H attempts to pre-empt Reg E by defining all agent-initiated transactions as "authorized." Courts have not ruled on whether this contractual language can override the statute. The CFPB has not issued guidance.

Banks are betting it holds. Customers carry the risk in the meantime. The pattern is identical to the 2008-era account arbitration clauses that stripped consumers of class-action rights, boilerplate that became binding law because regulators allowed it.

Five of the largest U.S. commercial banks adopted near-identical language in roughly one month. The financial system is writing its own rules while the debanking dynamic regulators have already documented continues unaddressed. Fed Governor Barr has warned that AI agents could trigger bank runs in minutes. The banks' answer is to make sure customers absorb whatever goes wrong.

This is also a clean illustration of the custodial model's core property: the institution defines what counts as your transaction, retroactively if needed. That is the architecture, not an edge case. It is exactly what self-custody of bitcoin is designed to route around. When you hold your own keys, no institution rewrites what you authorized.

For anyone weighing which AI tools to use alongside financial accounts, the credential-sharing risk is now explicit and contractually codified. Apps like Maple, which run AI models inside secure enclaves, exist precisely because credential and data exposure is the attack surface that matters.

## What to Watch

The enforceability question will eventually land in court or at the CFPB. If the bureau issues guidance that existing Reg E protections override banks' contractual "authorized by you" definitions, Section H's liability transfer weakens considerably.

Watch also for whether Citi launches a first-party AI agent product within the next twelve months: if it does, the competitive motive behind the third-party carve-out becomes explicit. If no bank-branded agent appears and no CFPB action follows, the terms quietly become the standard. That outcome is the more likely one.

## Sources

- Citi Online User Agreement, Section H
- Citi Agreement for Online Funds Transfers (November 2026 update)
- First reported by TechCrunch, November 18, 2026

## Frequently Asked Questions

## Does Reg E protect customers if a third-party AI agent drains their bank account?

Reg E limits liability for unauthorized electronic fund transfers, but Citi's Section H pre-emptively classifies all agent-initiated transactions as authorized by the customer. That contractual definition has not been tested in court and the CFPB has not ruled on whether it overrides the statute. Until it is challenged, customers bear the full loss.

## Why are banks' own AI agents exempt from the same liability terms?

Citi's Section H explicitly carves out "any third-party AI Agent, tool, function or feature provided by Citi." Bank-branded AI remains commercially viable under these terms; independent fintech AI does not. The liability asymmetry is structural, not incidental.

## Can customers opt out of Section H and keep their accounts?

No. These are non-negotiable adhesion contract updates. Continued use of Citi Online or Citi Mobile constitutes acceptance. Customers who object have no recourse short of closing accounts, and given that JPMorgan, Bank of America, PNC, and Citizens Bank have adopted near-identical terms in the same month, switching institutions does not change the exposure.
