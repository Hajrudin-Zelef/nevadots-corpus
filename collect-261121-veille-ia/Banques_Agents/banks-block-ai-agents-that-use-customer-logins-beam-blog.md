---
id: collect-261121-veille-ia/banques-agents/banks-block-ai-agents-that-use-customer-logins-beam-blog
title: "Banks Block AI Agents That Use Customer Logins | Beam Blog"
domain: banques-agents
role: reference
task: article
actors: []
dates: ["2026-10-22"]
keywords: ["agents", "banks", "agent", "logins", "customer", "jpmorgan", "techcrunch", "access"]
source: https://agentbeam.com/blog/us-banks-block-ai-agents-customer-credentials-read-only-mcp
source_anchor: ""
source_lines: [1, 125]
section: "Banques_Agents"
---
**US banks are drawing a line: an AI agent may look at your accounts through an approved connector, but it may not sign in as you.** According to The Decoder's report on an Axios story published October 22, 2026, several large banks are blocking agents that log in with a customer's credentials, whether driven by a browser tool such as Computer Use or an open-source agent such as OpenClaw. Other coverage shows JPMorgan and Charles Schwab describing the same policy. The security point is general: an agent holding your password is you, as far as the website can tell. If you run coding or browsing agents with stored logins, this is a policy-level warning about what that trust costs.

## Update (November 18, 2026): Citi, PNC, and liability even when the AI tool is hacked

**TechCrunch reports that Citibank updated its Online User Agreement the week of November 18, 2026**, telling customers who authorize **third-party AI tools** that they remain responsible for **all activity conducted through those agents**—including if the tool **leaks credentials or is hacked**. A Citi spokesperson quoted by TechCrunch said the change clarifies risks of sharing banking credentials and reinforces customer responsibility when granting account access to outside AI agents.

The same article says **PNC** (eighth-largest US bank by assets in TechCrunch's framing) **changed its terms on November 18, 2026**, and that **Citizens Bank** emailed customers warning they are **on the hook for anything their AI assistant does** (per an email TechCrunch reviewed). **JPMorgan** and **Bank of America** announced similar policies **last month** per TechCrunch; this wave adds **explicit hacked-tool liability** language beyond “do not share passwords.”

**Anthropic's** personal finance launch is contrasted as **read-only**—it **cannot move, manage, or invest money** per TechCrunch—matching the bank-preferred pattern in our October coverage: **bank-run MCP or Plaid-style connectors**, not full-account password agents. TechCrunch notes banks also view external agents as **competitive threats** to fee income, but the **security posture** for builders is unchanged: **scoped, revocable access** beats stored logins on a coding agent's laptop.

| Question | Short answer | 
|---|---|
| What changed? | Banks are blocking outside agents that use customer logins or browser automation | 
| Which banks? | JPMorgan, Schwab, Citi and Bank of America in several reports; Morgan Stanley and Wells Fargo named by The Decoder | 
| What is allowed? | Read-only connectors and bank-run MCP servers with fixed tools | 
| Is the Axios original verified here? | No. The page returned 403 to our fetch, so we rely on secondary coverage | 
| What should I do? | Stop sharing passwords with agents; use scoped, revocable access | 

## What was reported

The Decoder, citing Axios, says users are giving agents their bank logins so the agent can act in their accounts. Agents reach the sites through a browser, using tools like Computer Use or the open-source OpenClaw, and some appear to act without direct user instructions. Banks often cannot tell whether an agent is acting for the account holder or on its own, and their policies require account holders to log in personally. The Decoder says Morgan Stanley blocks AI agents entirely, that JPMorgan, Bank of America and Citi are reportedly doing the same, and that Wells Fargo is restricting external agent access. It adds that most banks want agents limited to read-only access, that Morgan Stanley said it has not seen rogue agents on its platforms, and that blocking is hard because users can run agents themselves in a browser. It also notes that OpenAI launched a ChatGPT service for financial tasks in the US with read-only access.

A more detailed account from BERI covers Schwab and JPMorgan. It quotes Schwab CEO Rick Wurster on the Q3 2026 call: "First, we are blocking unauthorized agents who try to log in on behalf of clients." It reports that JPMorgan CEO Jamie Dimon said on October 13 that the bank would not let "just any outside agent" into its systems.

## What is allowed instead

The sanctioned paths are narrower than a login. Per BERI:

- **Schwab** lets clients connect their own LLMs to Schwab's bank-run MCP servers, which expose a fixed tool list under Schwab's authentication. Read access and research are open; a trader pilot, starting with Claude, also allows trades. Schwab's own assistant can read account data and take a short list of account-management actions.
- **JPMorgan** allows agents to view balances read-only through authorized connectors such as Plaid. They cannot send money or pay bills, and no timeline for change was given.

Neither lets an outside agent move money. This follows a familiar security pattern: replace a shared secret with a scoped, bank-controlled interface whose actions are enumerated and attributable.

## Why the banks say they are doing it

Wurster asked "who's liable?" and argued neither bank nor client can control what an outside agent does. Dimon described a "liability transfer" to customers who use outside agents and warned of potentially "thousands" of rogue agents, according to BERI. From a security standpoint, three problems are real.

1. **Attribution.** A login by an agent looks identical to a login by the customer. Fraud systems tuned to human behaviour see automated sessions with valid credentials.
2. **Over-broad authority.** A password carries everything the user can do, not just the task the agent was asked to perform. A scoped token can be limited to balances.
3. **Injection exposure.** An agent browsing the web while logged into a bank can be steered by hostile page content. This is the attack class in our write-up of agentjacking through MCP: the instruction comes from data, and the authority comes from the stored login.

## What is unconfirmed

Reporting is not uniform. The Axios original was not retrievable for us, so the Morgan Stanley and Wells Fargo claims rest on The Decoder's summary. In our search for other coverage, Morgan Stanley appeared in reports about executives raising AI-related cyber risks on earnings calls, but we did not find a separate announcement of an agent block. We found no Citi statement in the sources we could read, so Citi's inclusion rests on The Decoder's "reportedly". Treat the list of banks as reported, not confirmed policy for each.

There is also a commercial context. BERI notes critics saying the banks have financial reasons, citing Schwab's net interest revenue on sweep cash that agents could move elsewhere and JPMorgan's 2025 move to charge fintechs for customer data access. It also says the CFPB's 2024 open banking rule has been set aside and reopened, so no binding federal rule requires banks to open a write channel for agents. Security and business interest can both explain the same policy.

## What this means if you run AI coding agents

You are probably not giving a coding agent your bank password, but the same pattern appears everywhere: cloud consoles, registries, SaaS dashboards, password-manager sessions. The bank case is a clean example of the design question.

1. **Prefer scoped tokens over logins.** Use API tokens, OAuth grants or read-only roles that match the task, and set expiry.
2. **Separate identities.** Where a service allows it, give the agent its own account rather than yours so its actions are attributable. Google's design for agent identities goes further, using short-lived workload identities and swapping credentials in at a gateway; see our write-up of Agent Compute Engine.
3. **Keep a human on money and destructive actions.** BERI's builder advice is to separate read, trade and money-movement permissions and to keep a named person approving payment release.
4. **Inventory what is stored.** Look for saved sessions, browser profiles and`.env` files the agent can reach. Our post on agents harvesting credentials shows how quickly an agent finds them.
5. **Treat bank-run MCP servers as software with bugs.** BERI points out that a server-side request forgery flaw appeared across several MCP servers, including one from JPMorgan; our MCP security guide covers how to review a server before connecting it.

## A practical checklist for teams with agents that log into portals

BERI's suggested steps for builders translate directly into an internal review. Inventory every agent, script and bot that signs into a bank, brokerage or other financial portal with stored credentials, including treasury and accounts-payable automation, because the restrictions are not described as limited to retail. Classify each action the agent can take as read, trade or money movement, and decide which of those the agent should ever do unattended. Ask each institution in writing whether it plans an agent API or MCP server, and what the terms are. Keep a named person approving payment release. And negotiate liability for agent-initiated actions into contracts rather than discovering the gap after an incident.

For a developer, the equivalent is a short audit of your own stored sessions. List which browser profiles your agent can drive, which of them are logged in to production consoles, and which credentials sit in files the agent can read. If an agent only needs to read a dashboard, give it a read-only account. If it needs to deploy, give it a deploy token with an expiry rather than your SSO session. Each substitution turns an unattributable login into a bounded, revocable grant, which is what the banks are asking their customers to accept.

Two caveats from the sources. First, the reported alternatives are early: Schwab's trading pilot is limited, and JPMorgan gave no timeline for anything beyond read-only. Second, blocking is leaky. The Decoder notes that users can run agents themselves in a browser, so a bank-side block shifts risk but does not remove it; detection of automated sessions remains an arms race. The durable fix is the scoped interface, not the filter.

## What Beam can and cannot do here

Beam does not sit between a browser agent and a bank. What it can do is local: it watches what Claude Code, Codex and similar agents do on your machine, flags credential exposure when a command or file contains secrets, redacts secrets before storing events, and can deny specific actions through Claude Code and Codex PreToolUse hooks. Everything stays on-device (how it works). It will not stop a browser agent from using a logged-in session, and it cannot enforce a bank's policy. The deny is hook-based, not a kernel sandbox, and it fails open when no cached policy exists. For command-level rules, see stopping dangerous Claude Code commands.

## The wider lesson

The bank decision is an access-control decision that every team running agents eventually faces. Passwords were designed for people. An agent that uses one inherits unlimited, unattributable authority, which is why defenders prefer interfaces where the permitted actions are listed in advance. Microsoft framed a similar idea in its guidance on treating agents as insiders (our summary): give them identities, limit them, and watch them. The expected end state is an ecosystem of scoped connectors rather than an arms race of agents impersonating users.

## Frequently asked questions

### Which banks are blocking AI agents?

Reporting names JPMorgan, Charles Schwab, Citi and Bank of America as blocking or restricting outside agents that log in for customers. The Decoder, citing Axios, also lists Morgan Stanley and Wells Fargo; we could not confirm a Morgan Stanley block in other coverage.

### Why do banks object to agents using customer credentials?

Bank policies expect the account holder to log in personally. When an agent uses the customer's credentials, the bank cannot tell whether the customer, the agent following instructions, or an agent acting on its own performed an action, and liability is unclear.

### What access is still allowed?

Reported alternatives are read-only data connectors such as Plaid at JPMorgan and bank-run MCP servers with a fixed tool list under the bank's own authentication at Schwab. Money movement by outside agents is not allowed at either.

### Does this affect people who use coding agents?

Indirectly. The same issue applies to any agent holding a stored password: it acts as you, with all your permissions. Prefer scoped tokens, separate accounts and approvals over sharing logins.

### Is the commercial motive worth noting?

Yes. Critics quoted in secondary coverage say banks also have financial reasons to restrict access, such as sweep cash and data-access fees. Security reasons and commercial reasons can both be true.

### What can Beam do about stored credentials?

Beam can flag credential exposure when a local coding agent reads or runs something that contains secrets and redacts them before storage. It cannot control what a bank allows or what a browser agent does on a website.

### Are banks liable if my AI agent gets hacked and drains my account?

TechCrunch's November 18, 2026 reporting says **Citibank's updated agreement** makes customers responsible for third-party AI agent activity **even when the tool is compromised**. Treat that as a **contractual** position, not a technical control—use **read-only bank connectors** instead of storing passwords in agent skills.

### Did Citi update its terms for AI agents in November 2026?

Yes, per TechCrunch and a Citi spokesperson quote in the same story.

### Is Anthropic personal finance safe for bank logins?

TechCrunch contrasts Anthropic's **read-only** finance tool with password-based agents. That design matches what JPMorgan and Schwab described in October, but builders should still read **bank integration docs** and avoid putting primary account passwords into coding-agent environment files.

## Related reading

- AU Small Finance Bank breach claim: ScarlettGroup, claimed vs verified
- Agentjacking through MCP
- Agents and credential harvesting
- Contractor's Claude Code agent on a DOD network: claimed vs verified
- MCP security: a practical guide
- Goldman Sachs limits desk access to Claude Code
- Google's microVM-per-agent design
- Pomerado website-to-MCP: keyless trials and saved logins (November 2026)
- The Decoder report and BERI summary
- South Korea bank hack: ARTEX and Claude Code exposure — why password agents raise attribution risk

*Details reflect secondary reporting as of October 22, 2026 and may change as banks publish formal policies. Beam is a local prototype; features described are as documented at the time of writing.*
