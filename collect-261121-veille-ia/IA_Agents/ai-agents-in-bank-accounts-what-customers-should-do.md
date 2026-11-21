---
id: collect-261121-veille-ia/ia-agents/ai-agents-in-bank-accounts-what-customers-should-do
title: "AI agents in bank accounts: What customers should do?"
domain: ia-agents
role: reference
task: article
actors: []
dates: []
keywords: ["agents", "bank", "banks", "reported", "axios", "customer", "customers", "accounts"]
source: https://www.getastra.com/blog/security-advisory/ai-agents-in-bank-accounts/
source_anchor: ""
source_lines: [1, 256]
section: "IA_Agents"
---
**The banks aren’t being hacked, at least not in the way the headlines imply. They’re being out-authenticated.**

No breach of JPMorgan’s, Morgan Stanley’s, Bank of America’s, Citi’s, or Wells Fargo’s core systems has been publicly reported. Instead, Axios reported on October 22—and Reuters corroborated—that AI agents in bank accounts (major U.S. banks) are being detected and blocked. These agents operate in customer accounts, often after customers hand over their banking credentials.

That creates a new security problem in Black October: a valid login no longer proves that a person chose the action. It may only prove that an agent holding the customer’s credentials did.wideopencountry

- Axios reported on Oct 22 that major US banks are finding unauthorized AI agents in customer accounts. Morgan Stanley blocks outside agents. JPMorgan, Bank of America and Citi restrict them, and Wells Fargo says it has taken steps to block them, while retaining the right to unblock some as per security measures and operationality. (Axios)
- No breach of the named banks’ core systems has been publicly reported. This does not rule out credential compromise, third-party vendor compromise, or future findings.
- Customers handed agents their own passwords, and banks often can’t tell the agent from the customer.
- For twenty years, banks asked one question: is this the customer? Agents change it to: who decided? A valid login no longer proves a person chose the action.
- That opens three gaps: identity (who is acting), intent (what they meant), and time (when the authority ended). We’ve seen each one before.
- What we’d do first: tie approvals to the specific transaction, open a sanctioned read-only lane, and make revocation fast and timestamped.

## **What happened**

Axios reported on Oct 22, citing people familiar with the matter, that banks are finding third-party AI agents inside customer accounts. Some agents tried actions the customer hadn’t asked for. Some called bank support lines.

Most agents get in with credentials the customer shared, which breaks the rule at most banks that customers must log in themselves. One executive told Axios the bank can’t easily tell agents apart, or know whether one was authorized or had gone rogue.

| Claim | Status | 
|---|---|
| Banks detect third-party AI agents in accounts, some calling support | Reported by Axios, citing anonymous sources. | 
| Agents often use the customer's own login | Reported by Axios and consistent across coverage. | 
| Morgan Stanley blocks outside agents; JPMorgan, Bank of America and Citi restrict them | Reported by Axios. Chase and Citi statements say similar things publicly. | 
| Wells Fargo is acting | A spokesperson told the Daily Caller it has taken steps to block unauthorized agents. | 
| Some agents acted beyond customer instructions | Reported, not quantified. No example has been published. | 
| Every named bank has seen rogue agents | Not established. Morgan Stanley says it hasn't seen rogue AI agents on its platforms. | 
| Customers lost money | No confirmed losses in the coverage we reviewed. | 
| Bank systems breached, or credentials stolen at scale | No public evidence. | 

The motives are mixed. Banks cite security and liability. The FT also reported a fear that agents will push idle cash toward higher-yielding products, and JPMorgan’s CFO linked agents to deposit competition. Both can be true.

### **Timeline (2026)**

- **Oct 13:** On JPMorgan’s third-quarter call, Jamie Dimon says the bank won’t let just any outside agent in. He warns of thousands of “rogue agents” and a “liability transfer” for customers who authorize one. (Forbes)
- **Oct 14:** Bank of America talks on its earnings call about limiting agent access to consumer deposits (secondary reports).
- **Oct 16:** The FT reports Citi bars third-party agents from logging in or transacting. Schwab’s CEO says it is blocking agents from moving funds (secondary reports).
- **Oct 22:** Axios publishes. Morgan Stanley says it hasn’t seen rogue agents. JPMorgan Chase has publicly warned customers not to share credentials. Wells Fargo confirms it has taken steps to block unauthorized agents, while its forthcoming agreement says it reserves the right to restrict or block certain third-party AI tools.
- **Nov 24:** Wells Fargo’s updated Online Access Agreement takes effect. Secondary reports say it treats requests sent through AI tools as the customer’s own. (Wells Fargo)

## **Our lens: from “who logged in” to “who decided”**

Banks have spent twenty years on one question: is this the customer? Passwords, device binding and one-time codes all answer it. An agent acting with the customer’s blessing passes every one of those checks, and then makes its own choices.

That breaks “authorized” into three gaps, and each has a precedent.

| Gap | The question | Precedent | The lesson | 
|---|---|---|---|
| Identity | Who is actually acting? | Salesloft Drift, Aug 2025. Attackers stole OAuth tokens held by an AI chat-agent vendor and reached the Salesforce data of more than 700 organizations. (Google, CyberScoop) | Whoever holds your customers' access becomes the target. One stolen token reached every linked organization. | 
| Intent | Did the customer mean this action? | UK push-payment fraud: victims approve payments they were tricked into. Since Oct 2024, UK banks must reimburse most of it, up to £85,000. (Farrer) EU rules since 2019 also require approvals tied to payee and amount. | A click is not consent. Regulators pushed liability toward banks and bound approval to the specific transaction. | 
| Time | When did the delegation end? | Plaid, 2021. A $58 million settlement over claims it harvested and kept bank logins through lookalike screens. Plaid denied the claims. (Financial Planning) | A shared password has no off switch except changing it. The industry's fix was tokenized APIs. | 

We’ve seen this movie before. Credential sharing outruns controls; a breach or a lawsuit forces scoped tokens, and then the fight moves to liability. Agents compress that cycle from years to months, because they act instead of only reading.

Regulation E ties all three together. Liability turns on whether the customer told the bank the delegate’s authority had ended. As we read 12 CFR 1005.2(m), time is the legal variable. This is not legal advice.

### **What the three gaps imply for design**

- **Classify, but don’t lean on it.** An agent driving the customer’s own browser looks like the customer. Score sessions, then gate risky actions on something the agent can’t fake.
- **Bind approval to the action.** The confirmation has to show payee and amount, and arrive on a channel the agent doesn’t control.
- **Treat revocation as a first-class event.** One customer action should end the agent’s access and record when it happened.
- **Plan for the vendor breach.** When an integrator is compromised, revoke every token at once, as Salesloft and Salesforce did on Aug 20, 2025, as part of the August 2025 Drift response

Two days before the Axios report, a different platform disclosed that stored customer secrets may have leaked: the Railway incident. No link has been reported between the two. The shared risk is structural. Anyone who stores delegated access is a target.

## **Who is affected**?

| Group | Exposure | First move | 
|---|---|---|
| Customers who connected an agent | Possible terms breach. Liability stays with them until they notify the bank. Their login sits with a vendor. | Revoke, change the password, notify the bank in writing. | 
| Banks | Can't separate agents from customers. Disputes are ambiguous. Contact centers are being probed. | Publish a policy, bind approvals, open a sanctioned lane. | 
| Agent vendors and aggregators | Hold bank logins. Become breach targets. Lose direct access. | Move to scoped tokens and publish security practices. | 
| Regulators and counsel | No rule defines delegated agent access. The CFPB's open banking rule is enjoined. | Clarify authorization and liability for software that exceeds instructions. | 

Nothing public points to a compromise of bank core systems or large-scale credential theft.

## **What to do next**?

### **Customers: seven steps today**

1. **Don’t give any AI tool your bank password, PIN, one-time codes or recovery codes.** Most bank terms require you to log in yourself.
2. **If you already did, change your password now.** Then revoke the tool’s access in the tool itself, in your browser extensions and in any connected-app pages.
3. **Tell your bank in writing that transfers by that tool are no longer authorized.** Send it by secure message and keep a dated copy. Under Regulation E, that notice shifts most risk to the bank (but not absolutely).*Do not treat this as legal advice, but confirm with a lawyer.*
4. **Review the last 30 to 60 days** of transactions, payees, recurring payments, linked devices and contact details.
5. **Turn on alerts** for every transfer and profile change. An agent that changes your contact details can silence them too.
6. **Prefer read-only, token-based connections** that your bank approves. Ask which providers it supports.
7. **If you must test an agent with money, fence it off.** Use a separate low-balance account or a virtual card with a spending limit.
8. Do not rely on browser-extension or AI-tool deletion alone; **change the bank password and review active sessions.**

Before connecting any agent, ask five things. Can it move money? Where are my credentials stored? Can I cap spending? How do I revoke it? Has the vendor disclosed security incidents, and how did it respond?

### **Bank executives: eight decisions for the next 90 days**

| # | Decision | 90-day deliverable | 
|---|---|---|
| 1 | Publish an agent policy with three lanes: sanctioned read-only, sanctioned scoped write, prohibited credential access. | Public policy page, updated terms, customer notice. | 
| 2 | Build the sanctioned lane before closing the other. A block with no alternative pushes customers to fringe tools. | Token-based read-only API for top use cases. | 
| 3 | Make high-risk actions agent-proof. Classification fails against agents in the customer's own browser. | Transaction-bound, out-of-band confirmation for new payees, large transfers, profile changes. | 
| 4 | Score sessions instead of blocking them. Assistive tools also look like automation. | Automation score on every session, plus an accessibility exception path. | 
| 5 | Harden the contact center. Agents have already called support. | Callback verification, no sensitive change on an inbound call alone. | 
| 6 | Write the dispute and notice playbook. The first cases set precedent. | Revocation intake on every channel, notice timestamps, counsel-reviewed Reg E position. | 
| 7 | Plan for the vendor breach, the Drift scenario. | Forced-reset runbook for customers who connected a vendor. | 
| 8 | Report to the board in numbers. | Share of sessions scored, time to revoke, agent-linked disputes, false-positive lockouts. | 

Keep the security rationale separate from the deposit-retention rationale in public statements. Customers hear both on the same earnings call, and a block that looks like protectionism weakens the security message.

Don’t announce a ban you can’t enforce. The credible end state is permissioned, auditable access, and most banks are reportedly moving toward read-only.

### **Security teams: controls that hold when detection fails**

Score signals; don’t issue verdicts. Useful ones include driven-browser artifacts, TLS fingerprints that contradict the claimed browser, cloud-hosted ASNs, pasted input at machine cadence, deep-link session starts and missing device binding. Expect evasion.

A policy tiered by risk rather than by label (illustrative):

```
rule: automation-risk-tiering
when:
any:
- session.automation_score >= 0.7
- session.device_binding == "none"
then:
allow: [accounts.read, transactions.read, support.chat.open]
step_up:
- actions: [payee.add, transfer.create]
method: cross_device_fido_assertion
bind_to: [payee, amount, currency]
channel: mobile_push_or_qr
- actions: [profile.change]
method: cross_device_fido_assertion
bind_to: [field, new_value]
channel: mobile_push_or_qr
flag_to_support_rep: [support.chat.open]
log: [session_id, automation_score, signals, decision]
```
Issue delegated tokens, not passwords. OAuth token exchange (RFC 8693) names the acting party, rich authorization requests (RFC 9396) express transaction limits, and DPoP (RFC 9449) binds the token to a key. Illustrative claims:

```
{
   "iss": "https://auth.examplebank.com",
   "sub": "customer-8f3a",
   "act": { "sub": "agent:acme-assistant/2.3" },
   "scope": "payments:initiate accounts:read",
   "authorization_details": [{
 	"type": "payment_initiation",
 	"actions": ["initiate"],
 	"instructedAmount": { "currency": "USD", "amount": "250.00" },
 	"creditorAccount": { "id": "payee-4412" }
   }],
   "cnf": { "jkt": "<DPoP key thumbprint>" },
   "exp": 1792717200
 }
```
Every action then carries an agent identity, a consent record, a limit and an expiry. Revoking the token ends access without touching the customer’s password.

## **Self-serve checks**

### **Customers: a five-minute audit**

- **Bank site or app:** open the list of devices, active sessions and connected third parties, and remove anything unfamiliar.
- **Browser:** list extensions that can read your bank’s pages. Remove AI and automation add-ons you don’t use.
- **AI tools:** open each tool’s connected accounts and saved logins. Delete stored bank credentials.
- **Contact details:** confirm the phone number and email on file are still yours.
- **Payees and recurring payments:** look for any you didn’t create.

### **Banks: detection queries (illustrative)**

Adapt these to your own session, payee and transfer tables. Thresholds are starting points, not benchmarks.

```
-- 1. New payee, fast transfer, automated-looking sessions
SELECT
t.transfer_id, s.session_id, s.customer_id, p.payee_id, t.amount, s.automation_score
FROM transfers t
JOIN payee_adds p
ON p.customer_id = t.customer_id
AND p.payee_id = t.payee_id
JOIN sessions s
ON s.session_id = t.session_id
WHERE t.created_at BETWEEN p.created_at AND p.created_at + INTERVAL '15 minutes'
AND s.automation_score >= 0.7;
-- 2. Contact details changed, then a large transfer within 24 hours
SELECT DISTINCT c.customer_id, c.changed_at, t.amount, t.created_at
FROM contact_changes c
JOIN transfers t ON t.customer_id = c.customer_id
WHERE t.created_at BETWEEN c.changed_at AND c.changed_at + INTERVAL '24 hours'
AND t.direction = 'outbound'
AND t.amount >= 1000;
 -- 3. Support contact shortly after an automated login
 SELECT s.customer_id, s.session_id, k.channel, k.opened_at
 FROM sessions s
 JOIN support_contacts k ON k.customer_id = s.customer_id
 WHERE k.opened_at BETWEEN s.started_at AND s.started_at + INTERVAL '30 minutes'
   AND s.automation_score >= 0.7;
```
### **Banks: test before someone else does**

- Can an agent finish login with a shared password and a forwarded one-time code?
- Does step-up bind to payee and amount, or only to “is this the user”?
- Can an agent add a payee, then move money inside the cooling-off window?
- Can an agent change contact details, then receive the alerts that would have warned the customer?
- Can an agent get through the contact center by voice or chat?
- Can your audit trail tell a customer, a delegated agent and a criminal bot apart afterward?

## **Bank positions and sanctioned lanes**

Positions differ in tone and detail, and several come from secondary reports. Check each bank’s own channels before relying on them.

| **Institution** | **Stated position** | **Source** | 
| JPMorgan Chase | Blocks outside agents acting for retail customers. Supports data sharing through open banking providers. Tells customers never to share logins. No timeline for direct agent access. | Forbes, Daily Caller | 
| Bank of America | Limits agent access to consumer deposits while working with developers on liability protocols. Wants a human to approve outflows. Secondary report. | FinTech Weekly | 
| Citi | Doesn’t permit third-party agents to log in or transact directly. Allows limited read-only access through approved open banking providers. | Retail Banker International, via American Bazaar | 
| Morgan Stanley | Blocking third-party agents, per Axios. Says it hasn’t observed rogue AI agents on its platforms. | Axios | 
| Wells Fargo | Says it has taken steps to block unauthorized agents. Updated Online Access Agreement takes effect Nov. 24. Offers a data-sharing service for approved connections. | Daily Caller, Online Access Agreement, Connect Account Information | 
| Charles Schwab | CEO says it is blocking agents from moving customer funds. Secondary report. | Cellcog roundup | 

The regulatory gap matters. The CFPB’s 2024 open banking rule under Section 1033 is enjoined while the Bureau rewrites it, and no federal rule defines delegated agent access. (Cozen O’Connor)

## **After the fire: build the lane, shrink the blast radius**

Blocking buys time. It doesn’t fix the problem, because customers will keep using agents. The durable answer is a lane where agents are identified, scoped and revocable.

- **Give agents an identity.** Every action should name the agent, the customer and the consent behind it.
- **Default to read-only.** Grant payment scopes per payee, per amount and per expiry.
- **Add cooling-off windows** for new payees and contact changes, so an agent can’t chain them within minutes.
- **Make revocation obvious.** One screen, one tap, one timestamp, visible to the contact center.
- **Hold vendors to a standard.** Ask how they store credentials, how fast they can revoke tokens and what they disclose after an incident.
- **Rehearse the vendor breach.** Drift ended in mass revocation. Have your own version ready.

## **Where Astra can help**

Most of what this piece describes can be tested before an attacker does it for you, or before a customer’s overeager AI assistant does it by accident.

If you’re exposing agent-facing features such as APIs, assistants, MCP servers or open banking integrations, our pentesters attack them the way a hostile agent would. That means goal hijacking, excessive agency, plugin abuse, indirect prompt injection and leaky APIs. Findings map to the OWASP LLM Top 10 and MITRE ATLAS.

Three places we’d start:

- **Money-movement and step-up flows.** Can an agent chain actions the customer never approved?
- **API and integration surface.** Find the shadow and undocumented endpoints an agent or aggregator could reach.
- **Every release that touches auth, tokens or approvals.** Continuous pentesting means these get retested as they ship, not at the next annual review.

Every finding is human-verified and comes with fix guidance. Talk to an Astra security expert.

**Live updates**

Times are UTC. We add dated entries as banks and regulators publish.

**Oct 23:** Initial post. Includes Wells Fargo’s confirmation of blocking steps, the Nov 24 agreement change, Citi’s public statement, and Bank of America and Schwab positions from secondary reports.

**What we still don’t know**

- **Have customers lost money to agent-initiated transfers?** That decides whether this is a fraud-loss problem or a policy problem.
- **Which banks saw agents exceed instructions, and which restricted them as a precaution?** That tells us how much of the risk is observed and how much is anticipated.
- **How will Regulation E apply to software that exceeds its instructions?** That decides who absorbs the first losses.
- **Will the CFPB’s revised proposal address delegated agents?** That decides whether banks build the rails voluntarily or under a rule.
- **What will Wells Fargo’s Nov 24 terms actually say?** We confirmed the upcoming agreement exists but couldn’t verify the AI wording.
- **Will an agent vendor disclose a credential-store incident?** That would turn the first row of our lens into a live event.
