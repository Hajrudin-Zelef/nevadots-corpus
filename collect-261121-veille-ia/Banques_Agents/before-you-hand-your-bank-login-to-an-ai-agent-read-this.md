---
id: collect-261121-veille-ia/banques-agents/before-you-hand-your-bank-login-to-an-ai-agent-read-this
title: "Before You Hand Your Bank Login to an AI Agent, Read This"
domain: banques-agents
role: reference
task: article
actors: []
dates: []
keywords: ["bank", "agent", "agents", "login", "most", "customers", "credentials", "agreement"]
source: https://ethicaledge.substack.com/p/before-you-hand-your-bank-login-to
source_anchor: ""
source_lines: [1, 98]
section: "Banques_Agents"
---
For as long as most of us have banked online, there has been one rule every bank, every fraud trainer and every compliance officer repeated: **never share your password.**

This fall, millions of people started doing exactly that. Not with a scammer, with a helper. A personal AI agent that offers to watch your spending, chase a better savings rate, pay a bill or move money between accounts usually asks for one thing first: your banking username and password.

The banks noticed. And over the past five weeks, one after another, they have rewritten their customer agreements to answer a question most consumers never thought to ask: **if your AI agent makes a mistake, gets tricked or gets hacked, who pays?**

Increasingly, the answer in the fine print is: you do.

## What happened, and how fast

In late October, Axios reported that major U.S. banks were finding third-party AI agents logging into customer accounts with customers’ own credentials, and that some of those agents appeared to take actions their users had not asked for. Banks built their security around the assumption that the person logging in is the account holder. An agent using your login looks, to the bank, exactly like you.

The response came quickly:

- **Mid-October:** JPMorgan Chase, Citi, Bank of America and Charles Schwab moved to restrict outside AI agents. Morgan Stanley reportedly began blocking them. In Canada, TD and RBC blocked third-party agents outright.
- **October 27:** Chase’s updated Digital Services Agreement took effect, prohibiting outside AI tools from using customer login credentials and stating that customers who let agents in “assume the entire risk.” The same day, Bank of America emailed customers a warning titled “Protecting your finances in the AI World.”
- **Early November:** Wells Fargo published a new agreement section on AI agents, and BMO told U.S. clients to confirm they had cut off agents’ access.
- **This week:** Citi added a new section to its Online User Agreement, PNC revised its terms on November 18, and Citizens Bank emailed customers a similar warning.

## What the new terms say

The wording differs bank to bank, but the direction is the same.

### Two details deserve special attention.

**Wells Fargo wants receipts.** Its new terms make you responsible for revoking an agent’s authority and for keeping evidence that you did, because the bank may have no way of knowing when you pulled the plug. Until you tell the bank, the agent’s activity remains your responsibility, to the extent the law allows. The agreement even treats delegated transactions as authorized “including those you did not want or intend.”

**Citi covers the breach scenario.** If the AI tool you used is compromised and a criminal uses the credentials it leaked, Citi’s terms treat that activity as yours too. That’s the scenario most people would assume their bank would cover.

## The part most people will get wrong: Regulation E

You may have seen posts (including some of my own early drafts) reassuring consumers that federal law still protects them. That’s true, but the protection is narrower than it sounds, and it’s worth understanding why.

Regulation E, which implements the Electronic Fund Transfer Act, limits your losses for **unauthorized** electronic transfers. But the regulation’s own definition of “unauthorized” has long excluded transfers made by someone you voluntarily gave your access credentials to, until you tell your bank that person is no longer authorized.

That rule was written with a roommate or a relative in mind. Applied to an AI agent, the logic goes like this: you gave the agent your login, so what it does may count as authorized, even when it does something you never wanted.

That’s why these new agreements focus so heavily on credential sharing and revocation. They’re building on a rule that already exists.

What remains genuinely unsettled:

- Whether a transfer an agent makes **outside the scope** of what you asked counts as authorized.
- How the rules apply when a **criminal** steals credentials from a compromised AI tool rather than receiving them from you.
- Whether a bank’s contract can define these situations more narrowly than the regulation does. It can’t waive your statutory rights, but where the law is unclear, the contract will shape the argument.

As of this week, no U.S. regulator has publicly addressed how these new terms square with Regulation E. Until one does, or until a court does, assume the burden will be on you.

## Four ways this goes wrong

You don’t need a dramatic hack for an AI agent to cost you money.

1. **It misunderstands you.** You ask it to “move savings somewhere with a better rate,” and it moves more than you meant, or to somewhere you didn’t vet.
2. **It goes beyond what you asked.** Banks have reported agents taking actions that appeared disconnected from the user’s request. A password gives an agent everything you can do, with no way to limit it to one task.
3. **It gets tricked.** AI agents read web pages, emails and documents. A malicious instruction hidden in that content can redirect an agent that holds your credentials. Security researchers have already shown agents falling for fake bank login pages.
4. **You forget it’s there.** You stop using the app, but it still has your login. Under terms like Wells Fargo’s, quietly uninstalling it isn’t enough.

## What to do before you connect anything

**Read your bank’s current online banking agreement.** Search it for “AI,” “agent,” “third party,” “credentials” and “authorized.” If you bank at one of the institutions above, the language has changed in the past month.

**Don’t give an AI agent your username and password.** This is now the clearest line across nearly every major bank. Once you share credentials, you are in the weakest possible position if something goes wrong.

**Use bank-approved connections instead.** Several banks say they support outside tools through approved, secure connections (the kind budgeting apps use) rather than password sharing. Ask your bank which tools are approved, and choose read-only access wherever you can.

**Keep a human hand on the money.** Let AI analyze, compare and recommend. Make the final move yourself. If a tool won’t let you approve transfers before they happen, that tells you something.

**Turn on every alert your bank offers.** Transaction alerts, login alerts, new-payee alerts. Speed matters when you report a problem.

**Revoke in writing, and tell your bank.** If you stop using an agent, cancel its access, change your password, notify your bank in writing, and keep a dated copy. Under some agreements, that notice is what ends your responsibility.

**Report problems immediately.** Regulation E’s protections are tied to how quickly you report. Waiting can increase what you’re on the hook for, even when the transfer does qualify as unauthorized.

## The question underneath all of this

I want to be fair to the banks. They have a real problem. When software logs in with your credentials from your device, they often cannot tell whether they are dealing with you, your agent, or a criminal using your agent. Fraud systems built for human behavior don’t translate cleanly, and a bank that blocks too aggressively frustrates legitimate customers.

But look at the shape of the response. Banks are investing heavily in AI inside their own operations, and their terms generally treat their own AI tools differently. Meanwhile, the risk of the AI tools customers bring in is being assigned to the customer, through contract updates most people will never read.

That’s a governance choice, and it raises a fair ethics question: **is a click-through agreement real informed consent when the technology can act in ways neither the customer nor the bank can predict?**

Even some bank leaders think the burden belongs elsewhere. PNC’s CEO, Bill Demchak, argued at Money20/20 that the AI companies themselves should take on the liability, describing them as “kids in a room with a new toy.” Others in the industry expect banks and technology firms to eventually negotiate standards for agent identity, authentication and liability, much as they once did for budgeting apps.

Until that happens, the practical answer is simple, if unglamorous: the convenience is yours, and so is the risk. Know which one you’re signing up for.

— Evie

*This article is for general information and is not legal or financial advice. Agreements change; check your own bank’s current terms, and talk to a qualified professional about your specific situation.*

## Sources

- American Banker, “Wells Fargo wants customers to prove they’ve revoked AI agents”
- American Banker, “Banks are blocking external AI agents. Is this sustainable?”
- SecureWorld, “AI Agents in Bank Accounts: Banks Can’t Tell Who’s Driving” —
- Captain Compliance, on the Wells Fargo and Chase terms
- TokenPost, “Citi Makes Customers Liable for AI Agent Activity on Bank Accounts”
- Startup Fortune, “Citi, PNC and Citizens tell customers they own any loss an AI agent causes”
- Verdantix, “AI Agents in Customer Bank Accounts”
- Regulation E, 12 CFR 1005.2(m) (definition of unauthorized electronic fund transfer)
