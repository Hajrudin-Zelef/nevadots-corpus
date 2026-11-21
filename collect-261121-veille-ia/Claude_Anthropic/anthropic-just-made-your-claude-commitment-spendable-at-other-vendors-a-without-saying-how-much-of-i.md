---
id: collect-261121-veille-ia/claude-anthropic/anthropic-just-made-your-claude-commitment-spendable-at-other-vendors-a-without-saying-how-much-of-i
title: "Anthropic just made your Claude commitment spendable at other vendors â without saying how much of it | Pick Right"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-09-23"]
keywords: ["anthropic", "commitment", "claude", "customers", "software", "marketplace", "partner", "right"]
source: https://pick-right.com/news/anthropic-claude-marketplace-committed-spend-drawdown-undisclosed-portion-2026-09-23/
source_anchor: ""
source_lines: [1, 91]
section: "Claude_Anthropic"
---
**TL;DR:** Anthropic launched the **Claude Marketplace** on **23 September 2026** â **2,000+ connectors and plugins**, purchasable partner products (**CrowdStrike, Cursor, Harvey, Legora, Lovable, Snowflake**), and service partners (**Accenture, BCG, Deloitte**) in one catalogue. The catalogue is not the story. The purchasing mechanic is: **eligible customers can put a portion of their committed Anthropic spend toward partner software**, invoiced by Anthropic. That is **AWS Marketplace drawdown**, arriving at an AI lab. AWS **publishes** its cap â 25% of an annual commitment, negotiable to ~30â35%. Anthropic has published **no number, no eligibility rule, and no fee structure**. And the catalogue is a **distribution channel, not a trust boundary**: Anthropicâs own directory states it âcannot verifyâ what a third-party plugin contains or whether it changes after you install it.

## What shipped

On 23 September 2026 Anthropic put three things that already existed separately behind one door.

The first shelf is connectors and plugins â over 2,000 of them, built on the Model Context Protocol and Agent Skills, the two open standards Anthropic has spent the past two years seeding. Atlassian, Google, Microsoft, Notion and Salesforce are the names in the announcement; the long tail is considerably longer.

The second shelf is products you buy. Claude-powered software from CrowdStrike, Cursor, Harvey, Legora, Lovable and Snowflake, listed for purchase rather than installation.

The third is people. Consultancies and systems integrators from the Claude Partner Network â Accenture, Boston Consulting Group and Deloitte at the premier tier â available to be engaged for rollouts.

Anthropic frames this as a discovery problem solved: âa single destination to find the right tools and servicesâ for customers, âan easier way to reach teams using Claudeâ for builders. That framing is accurate and slightly beside the point. Anthropic has run a limited-preview marketplace since earlier in 2026, with roughly six partners; the plugin directory has lived inside Claude Code longer still. Catalogues of this kind are not scarce and are rarely decisive.

What is genuinely new sits in one clause, and most of the coverage walked past it.

## The clause that matters

Eligible customers can put **a portion of their committed Anthropic spend** toward those partner purchases, with Anthropic consolidating invoicing under the existing customer relationship.

Anthropic supplies three worked examples: CodeRabbit applying funds toward Vercel, and Power Digital and ThoughtSpot directing theirs to Snowflake.

If that mechanism sounds familiar it is because it is the single most successful commercial construct in enterprise cloud. AWS Marketplace lets customers retire part of an Enterprise Discount Program or Private Pricing Agreement commitment through third-party software purchases. The cap is public: **up to 25% of the annual commitment**, negotiable to roughly 30â35% for organisations whose stacks lean heavily on third-party SaaS, and since May 2025 restricted to products that actually run on AWS. Google Cloud and Azure run close variants.

The mechanism is elegant because every party gains something immediately. The customer converts commitment risk â money committed and potentially unconsumed â into software it was going to buy regardless. The software vendor gets a procurement path that skips a new vendor-onboarding cycle, which in a large enterprise is measured in quarters. And the cloud provider gets a commitment that is easier to consume, which means customers sign larger ones and renew them more readily.

Anthropic has now built the same instrument. It is a more consequential move than any single model launch, because it changes the shape of the contract rather than the price of a token.

## The number Anthropic did not publish

Here is where the launch stops being usable.

AWS tells you 25%. It tells you what qualifies, what does not, and what is negotiable. A finance team can take that to a renewal conversation and model it.

Anthropic says âa portion.â There is no published percentage, no minimum commitment that makes a customer eligible, no statement of whether the cap is standard or negotiated deal by deal, and no disclosure of fees or revenue share with listed partners. The route in is the one every enterprise-sales motion uses at this stage: contact your account team.

That gap is not a rounding error in the story â it *is* the story. A drawdown allowance of 25% changes which tools an organisation buys and how large a commitment it is willing to sign. A drawdown allowance of 2%, or one gated behind an eight-figure commitment, is a rebate with a press release attached. Those two worlds are indistinguishable from the announcement, and the difference between them is the entire buyer-side question.

Until Anthropic publishes the figure, the correct treatment is straightforward: the Claude Marketplace is a catalogue you can browse today and a budget instrument you cannot yet plan around. Anyone building next yearâs tooling budget on the assumption of meaningful drawdown is modelling a number nobody has stated.

## Listing is distribution, not assurance

The second thing worth reading carefully is what a listing certifies.

Anthropicâs own plugin directory answers this without hedging. It separates plugins built by Anthropic from external submissions, and external submissions pass a quality and security review before approval. It also carries this: Anthropic âdoes not control what MCP servers, files, or other software are included in plugins and cannot verify that they will work as intended or that they wonât change.â

That last clause carries the weight. A plugin is not a package of frozen code â it is a pointer to a server a third party operates and can change after you install it. Approval is a gate at the door, not a guarantee about the room.

This is the same exposure that has already produced real incidents. Microsoftâs Copilot connector chain was the vector in the Cosnitch memory-poisoning disclosure, where the trusted surface was not the model but what the model was allowed to read. The Langflow CVE that leaked OpenAI and AWS keys sat in the glue layer between an agent and the services it reached, not in the agent. And as Claudeâs MCP surface has become a metered product inside other vendorsâ platforms, the number of parties holding a token that can read your systems has gone up, not down.

A first-party catalogue raises the perceived trustworthiness of everything in it. On the evidence of Anthropicâs own disclaimer, that perception runs ahead of the assurance. The install-time review â who runs this server, what scopes does it want, what can it read, what happens when it updates â is still yours, and the self-hosted sandbox and tunnel patterns remain the containment story for anyone who needs one.

## What this does to the competitive picture

Two effects are worth tracking.

The first is on partners. A vendor listed here has traded something for the distribution: part of its billing relationship now runs on Anthropicâs paper. That is a familiar bargain â it is how the cloud marketplaces work â and it is usually worth taking. But it concentrates leverage. OpenAIâs Astra for Law launch made the same structural point from the other side: when a labâs partner roster becomes the market, the roster is a competitive asset the lab controls. Note that Harvey and Legora, direct competitors in legal AI, are both on this list, which is the lab keeping optionality rather than picking.

The second is on the neutral middle. Routers and gateways have argued that buying capacity through a vendor-neutral layer beats buying it from a lab. Drawdown is a direct counter-offer: keep the money inside the labâs contract and we will make it spend further. The counter-argument has not changed â a commitment you can only spend in one ecosystem is a commitment you have to renew â but the offer is now materially better than it was, which is precisely how this mechanism works.

## What to do with this

**If you hold an Anthropic commitment**, ask your account team for the specific drawdown percentage, the eligibility threshold, and which listings qualify, in writing, before it factors into any plan. Treat âa portionâ as zero until it is a number. The right time to establish this is during a renewal conversation, not after.

**If you are choosing tools**, keep the drawdown out of the shortlist. Pick the coding tool or agent platform that wins on merit â the Cursor versus Claude Code question is decided by how your team works, not by whose invoice it arrives on â and only then ask whether the winner can be bought with committed money. Reversing that order is how organisations end up with a stack selected by a procurement mechanic.

**If you are installing plugins**, the catalogue changes nothing about your review. Inspect the manifest, identify who operates the MCP server, scope credentials narrowly, and re-check after updates. Developers treating a first-party listing as a substitute for that review are extending trust that Anthropic has explicitly declined to offer.

And note what this is, in sequence. Anthropic spent September cutting effective Opus pricing through defaults rather than list price, tying capability access to verification programmes, and now making the commitment itself more spendable. None of those is a model announcement. All of them make a Claude contract harder to leave â which, on the evidence of how quickly platform layers get retired elsewhere, is the variable worth watching more closely than the benchmark table.

## Frequently asked questions

## What actually launched on 23 September 2026?

A single catalogue with three shelves. The first is connectors and plugins â more than 2,000 of them, built on the Model Context Protocol and Agent Skills, covering Atlassian, Google, Microsoft, Notion and Salesforce among others. The second is purchasable Claude-powered products from partners including CrowdStrike, Cursor, Harvey, Legora, Lovable and Snowflake. The third is service partners: consultancies and systems integrators from the Claude Partner Network, with Accenture, Boston Consulting Group and Deloitte named at the top tier. None of the three shelves is brand new on its own â Anthropic ran a limited-preview marketplace with a handful of partners earlier in 2026, and the plugin directory has existed inside Claude Code for longer than that. What is new on 23 September is that they are one destination, at scale, with a purchasing mechanic attached.

## How does committed-spend drawdown work, and what portion can I use?

The principle is simple: if your organisation has signed a spending commitment with Anthropic, you can direct some of that committed money at partner software in the marketplace instead of at Claude tokens, and Anthropic invoices it under your existing relationship rather than making you open a new vendor account. Anthropic's own examples are CodeRabbit putting funds toward Vercel, and Power Digital and ThoughtSpot directing theirs to Snowflake. The portion is the problem. Anthropic's announcement says 'a portion' and stops there â no percentage, no floor, no published eligibility threshold, and no statement of whether the cap is uniform or negotiated per contract. For comparison, AWS publishes its equivalent: Marketplace purchases can retire up to 25% of an annual EDP or PPA commitment, negotiable upward to roughly 30â35% for third-party-heavy stacks. Until Anthropic names its number, no finance team can model this.

## Does listing in the marketplace mean Anthropic has vetted the plugin?

No, and Anthropic says so directly. Its own plugin directory carries the warning that Anthropic 'does not control what MCP servers, files, or other software are included in plugins and cannot verify that they will work as intended or that they won't change.' The directory does separate internally-built plugins from external submissions, and external ones pass a quality and security review before approval, but that is an admission gate rather than a continuing guarantee. A plugin is a pointer to a third-party MCP server that the publisher controls and can update after you install it. The security review that matters is the one you run at install time and again after updates: who operates the server, what scopes it requests, what it can read. Presence in a first-party catalogue raises trust without earning it.

## Should this change which coding or legal AI tool I buy?

Only at the margin, and only if the drawdown applies to you. If you hold a large Anthropic commitment you are at risk of underconsuming, then buying Cursor or Harvey through the marketplace converts money you would have forfeited into software you would have bought anyway â that is a real saving and a legitimate reason to route the purchase this way. If you have no commitment, or no risk of underconsuming it, the marketplace changes nothing about the merits: the same tools cost the same money and are available directly. The failure mode to avoid is letting the drawdown decide the shortlist. A tool that is 15% worse for your workload is not rescued by being purchasable with money you had already committed, and the moment a marketplace purchase becomes a reason to renew a commitment you would otherwise shrink, the discount has been paid back with interest.

## Is this a lock-in mechanism?

It is the most effective one the cloud industry has produced, and it works without any coercion. Drawdown makes a commitment easier to consume, which makes a larger commitment easier to sign, which makes the next renewal harder to shrink â because by then part of your third-party software budget flows through the same contract, and cutting the commitment means unpicking procurement relationships that have nothing to do with tokens. AWS has run this playbook for years and enterprises sign EDPs anyway, because the discount is real. The honest framing is not that the mechanism is a trap, but that its cost is deferred and its benefit is immediate. Price the renewal, not the first invoice. And note what it means for partners too: a vendor whose customers pay through Anthropic's paper has handed Anthropic part of its billing relationship.
