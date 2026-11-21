---
id: collect-261121-veille-ia/banques-agents/major-banks-move-to-block-or-limit-customer-directed-ai-agents
title: "Major Banks Move to Block or Limit Customer-Directed AI Agents"
domain: banques-agents
role: reference
task: article
actors: []
dates: []
keywords: ["agents", "banking", "agent", "customers", "access", "block", "customer", "banks"]
source: https://www.hunton.com/insights/legal/major-banks-move-to-block-or-limit-customer-directed-ai-agents
source_anchor: ""
source_lines: [1, 67]
section: "Banques_Agents"
---
Between October 13 and October 20, 2026, JPMorgan Chase, Charles Schwab, Citigroup, and Wells Fargo each moved to block or contractually reallocate the risk of third-party AI agents that log in to customer accounts with the customer’s own credentials. This legal update summarizes those developments and identifies considerations for potential enhancements across consumer deposit, digital banking (covering online and mobile banking), debit and credit cardholder, and treasury management agreements.

#### What the Banks Did

In the span of one week, four of the largest US banking organizations announced that they will not permit third-party AI agents to log in to customer accounts using the customer’s own credentials. Four chose a hard block; Wells Fargo instead rewrote its online access terms to put the risk for the customer’s use of AI agents on the customer. None of the four has banned AI outright: each continues to permit customer-authorized, read-only data sharing through open banking connectors, and several are building their own agent tools.

| **Institution** | **Date** | **Action** | **What Remains Permitted** | 
| JPMorgan Chase | Oct. 13, 2026 (Q3 earnings call) | CEO Jamie Dimon said that the bank will not allow outside agents, naming Meta’s Muse and Instinct, to act inside customer accounts, citing “rogue agents.” A spokeswoman confirmed the bank does not permit third parties to obtain customers’ credentials and log in on their behalf. An updated Digital Services Agreement (effective Jan. 1, 2027) provides that a customer who gives an AI agent access authorizes everything that AI agent does and that Chase will rely on the agent’s instructions as the customer’s own instructions. | Read-only data (e.g., balances) through open banking providers such as Plaid; no money movement or bill pay. No timeline for direct AI agentic access. | 
| Charles Schwab | Oct. 16, 2026 | CEO Rick Wurster told Bloomberg the firm blocks unauthorized agents, including Muse, that attempt to move client money 10 to 20 times per week. | Developing controlled connections that allow information access without permitting AI agents to move money; launched an in-house AI assistant “Charley.” | 
| Citigroup | Oct. 16, 2026 | Citi does not permit “third-party AI agents or assistants to log into a customer’s account or transact directly within our digital banking platforms.” | Limited, read-only access through certain open banking providers. | 
| Wells Fargo | Terms effective Nov. 24, 2026 | No outright block. New Online Access Agreement Section 15 (“Third-Party Artificial Intelligence Agents or Agentic AI”) defines an AI agent broadly, states that use is “at your own risk,” deems all AI agent-initiated transactions authorized by the customer, disclaims any duty to monitor AI agent activity, and reserves the right to limit or terminate AI agent access at any time. | AI agent use permitted at the customer’s sole risk. | 

The banking actions track parallel moves outside financial services: Amazon has implemented technical blocks against Muse and sent enforcement demands to Perplexity over its Comet browser agent, and Delta and Yelp have shut AI agents out as well.

#### **Why Now?**

The immediate trigger appears to be the launch of Meta’s Muse personal AI assistant several weeks earlier, which renewed concern that consumer-facing AI agents would be handed banking credentials and/or debit and credit card information and instructed to manage household cash. Four concerns drove the banks’ response:

- ***Deposit Flight*****.** Apollo Global Management chief economist Torsten Slok has warned that AI agents could automatically sweep household balances from checking accounts averaging roughly 0.1 percent into alternatives yielding 3.3 percent to 5 percent, eroding the low-cost deposit base that funds lending. One estimate put the potential cost to banks at $47 billion per year.
- ***Security*****.** An AI agent that logs in with a customer’s password is indistinguishable from the customer to the bank’s systems: it has the same permissions, no limits of its own, and no separate audit trail. Credential-based access defeats multi-factor authentication by design, and “fake agent” phishing campaigns against retail and payment infrastructure are already in active use.
- ***New Fraud and Risk Scenarios*** . Some emerging fraud vectors and associated risks specific to AI agents that banks are currently concerned about include the following:
  - *Credential Sharing and Account Takeover* : AI agents accessing financial accounts through customer credentials, and potential compromise of the security of those credentials on the AI agent side.
  - *Agent Impersonation* : Malicious bots pretending to be legitimate AI agents.
  - *Mandate Violations* : AI agents exceeding spending limits, changing beneficiaries of transactions, or acting beyond the human customer’s consent.
  - *Prompt Injection and Transaction Manipulation* : Fraudsters influencing an AI agent’s decisions or payment instructions.
  - *Automated Attacks* : Fraud rings scaling purchases, refunds, and account activity through AI agents.

- ***Regulatory Uncertainty*****.** Executives at major banks have raised the growth in AI agent-initiated transactions with the Federal Reserve. Regulators have privately welcomed the restrictions and have indicated that legal clarification may be needed on customer authorization and liability when autonomous software (such as AI agents) executes transactions.

The dispute echoes the earlier fight between JPMorgan and data aggregators such as Plaid over credential-based screen scraping, and it arrives while the status of the Consumer Financial Protection Bureau’s (CFPB) Section 1033 Open Banking Rule remains unsettled. On the AI agent side, Meta has said it is developing “Poppy,” a protocol intended to signal to companies when a personal AI agent is acting for a customer.

#### **The Open Banking Connection: CFPB Section 1033 Open Banking Rule**

The AI agent question is really the next chapter of the Section 1033 open banking fight. The CFPB’s October 2024 Personal Financial Data Rights rule required data providers to make account data available to consumers and their “authorized third parties” through developer interfaces, barred fees for that access, and was built to move the industry away from credential-based screen scraping. Bank trade groups sued the same day, a federal court in the Eastern District of Kentucky enjoined enforcement, and the April 1, 2026, compliance date for the largest banks passed without effect.

The current CFPB opened a reconsideration by publishing an August 22, 2025, an advance notice of proposed rulemaking (ANPR) (comments closed October 21, 2025), and in August 2026, the CFPB sent a proposed “Personal Financial Data Rights Reconsideration” rule to the Office of Management and Budget’s Office of Information and Regulatory Affairs (OIRA) for review; the final rule has not yet been published, and the open issues are precisely the ones the agent debate turns on: whether data providers may charge fees for access, who qualifies as a consumer’s authorized representative, and how to balance consumer choice against privacy and cybersecurity. The distinction every bank drew recently, permitting customer-authorized read-only access through open banking connectors while blocking credential-based login and money movement, is the Section 1033 architecture applied to a new class of third party (AI agents).

If the reconsidered Open Banking Rule confirms a tokenized, permissioned interface as the sanctioned channel and allows reasonable access fees, banks will have both the legal footing and the economic incentive to treat an AI agent as an authorized third party that must come through that door, subject to scoped permissions and the bank’s security procedures, rather than as a customer-impersonating login to be blocked.

AI agent provisions that can be added to customer-facing agreements can also be drafted to fit that outcome by including a carve-out for “an access method we expressly offer for that purpose” such as an individual bank’s Section 1033 interface, under which any future AI agent-specific permissioning can be cited. Banks should therefore consider drafting the credential-sharing prohibition and the data-access terms as a single framework, and should expect the final shape of the AI agent rules to be set as much by the reconsidered Section 1033 Open Banking Rule and card-network AI agentic commerce standards as by the banks’ customer agreements themselves.

#### **Customer-Facing Agreement AI Agent Provision Implementation Considerations**

Banks may want to consider enhancing customer-facing agreements, such as consumer deposit, digital banking (covering online and mobile banking), debit and credit cardholder, and treasury management agreements. However, any such enhancements should take the following into consideration:

- **Conspicuousness and UDAAP.** The section should be conspicuous, in plain language, and presented at the point where the customer is most likely to be enabling an AI agent, ideally an in-app interstitial when a new device or authenticator is registered, not only in the master agreement.
- **A Real Revocation Path.** If the customer can revoke only by notifying the bank, the bank must be able to act on that notice. Consider using staff scripts that capture the AI agent’s name and provider, and consider adopting the ability to rotate credentials and reset device registrations on request.
- **Bank-Sanctioned Access.** Decide whether the bank will offer a sanctioned path (tokenized, scoped API access with per-agent permissions and limits), as Schwab has signaled. If so, account for the bank’s sanctioned path in customer-facing agreements and govern it by its own terms. A sanctioned path potentially strengthens the argument that credential sharing outside it was unreasonable.
- **Section 1033 Alignment.** The CFPB Section 1033 Open Banking Rule’s status remains unsettled. If the bank’s data-access terms address screen scraping, align the AI agent definition with them so that an aggregator-connected agent (for example, one reading balances through Plaid) is not treated inconsistently across the two sets of terms.

- **Card Network Agentic Credentials.** Card transactions initiated through tokenized agentic credentials (Visa Intelligent Commerce, Mastercard Agent Pay) will route through network rules and the token’s consent mandate rather than the bank’s AI agent provision. If the issuer or processor will enable network agentic tokens, address them in a separate paragraph tied to the network’s rules, since chargeback and liability allocation will be set there.

- **Change-in-Terms Mechanics.** Review customer-facing agreements’ amendment provisions for required notice of changes. For credit cards, a change-in-terms notice is prudent even though the new term does not trigger the 45-day significant-change notice, and the posted agreement must be updated under 12 CFR § 1026.58.

- **Third-Party and Fintech Programs.** Sponsor banks arrangements or partnerships with fintechs should confirm that program partners’ customer-facing terms and authentication flows carry equivalent provisions and that the program agreement allocates agent-initiated losses consistently with them.

- **State Overlays.** Check the provisions against state AI-accountability statutes in the bank’s footprint and against the existing arbitration and class-waiver provisions for any interaction.

#### **Recommended Next Steps**

1. Inventory every customer-facing agreement that governs credentialed access or payment initiation for potential enhancements: deposit, online and mobile banking (digital banking), consumer and business debit and credit card, and treasury management agreements. (Note that for banks that use a deposit agreement that is generated by a third-party “forms” vendor, banks may have to get specific permission to amend such a deposit agreement, which will entail conversations with, and approval from, the third-party vendor.)
2. Decide the bank’s posture on agent access (block, permit on the customer’s risk, or build a sanctioned path) and make sure the agreement, the digital banking controls, and the fraud-detection rules say the same thing.
3. Update error-resolution and dispute procedures so that investigators ask whether, and when, the customer furnished credentials to an agent, and so that the “fake agent” and compromised-agent scenarios are routed to the unauthorized-transaction process.
4. Deliver the change through a conspicuous notice and an in-app disclosure at the authenticator-registration step.
5. Monitor for regulatory guidance; Federal Reserve staff have indicated that clarification on authorization and liability for autonomous transactions may follow.

We are currently assisting numerous bank clients in addressing these AI agent issues and considerations, including preparing AI agent provisions for customer-facing agreements. Please contact any of the authors of this Client Alert to discuss how these developments affect your institution’s agreements and controls.
