---
id: collect-261121-veille-ia/claude-anthropic/claude-for-government-keeps-the-only-copy-of-each-chat-on-a-laptop
title: "Claude for Government Keeps the Only Copy of Each Chat on a Laptop"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["claude", "government", "anthropic", "agencies", "only", "meter", "product", "copy"]
source: https://www.beri.net/article/claude-for-government-ga-fedramp-high-not-to-exceed-billing-local-history-records-retention
source_anchor: ""
source_lines: [1, 94]
section: "Claude_Anthropic"
---
**Claude for Government solves the problem that stops most federal AI purchases, which is the open meter. It leaves the next problem with the agency: records.** Anthropic made the product generally available on September 30 for federal and state agencies, inside a FedRAMP High environment. It has no seat fees, and usage is bought in fixed increments under a hard not-to-exceed cap. But conversation history "stays local on the agency-managed device," and the usage export is metering data only. Unless an agency builds its own capture path, the only copy of what employees asked Claude, and its answers, sits on each employee's laptop. Before you issue the ATO, decide who keeps that copy, under which records schedule, and what happens to it when the laptop is reimaged.

The billing design is good, and agencies should take it. The records design is defensible on security grounds. It is also a transfer of work, from the vendor to your records officer, that the announcement does not spell out.

## What Anthropic Shipped on September 30

General availability gives agencies a desktop app, a capped prepaid meter, and an admin console. Everything else is still in early access. Claude for Government has been in public beta since July 7, when Anthropic brought Claude Code and Claude Cowork into a FedRAMP High environment delivered as a desktop application. Agencies deploy it through their standard MDM platforms and need no separate cloud-provider contract. According to Anthropic's GA announcement, the generally available product includes:

- **Billing:** no seat fees. Agencies "pay for usage in fixed increments with a hard not-to-exceed cap," with burndown alerts before the balance runs out.
- **Administration:** department-level allocation to sub-agencies, SSO through the agency's identity provider, and SCIM group mappings that set rate and dollar limits per user tier.
- **Oversight:** an audit log of administrative actions, and two-person approval for sensitive operations on Anthropic's side.
- **Still early access:** the Claude Code command-line tool and Claude for Microsoft 365.

Those two early-access items matter more than their placement suggests. In the admin portal they are the only product switches that default to off, and the standalone CLI has its own connection and managed-settings setup. If your modernization team's business case depends on terminal-based Claude Code, or your program offices want Claude inside Word and Excel, scope those components as separate items in the ATO package. Do not assume they come with the desktop authorization.

On authorization, Anthropic's government solutions page lists Claude for Government at FedRAMP High and DoD IL5, next to Claude Enterprise and the Claude API on Bedrock and Vertex AI.

## Why the Not-to-Exceed Meter Fixes the Appropriations Problem

Prepaid increments under a hard cap are a consumption-priced AI offer built around how federal money actually works. The Antideficiency Act bars agencies from obligating or spending funds "in advance or in excess of an appropriation." An open token meter is hard to reconcile with that. A capped, prepaid increment is not: the obligation is the increment, and the cap stops spending when it runs out.

The timing helps too. FY27 began today on a continuing resolution that Congress enacted on September 3 and that runs to December 11. A contracting officer who can obligate a small, fixed increment now and add more after a full-year appropriation passes has an easier conversation than one asked to sign a seat commitment against a stopgap. We made the same argument about OpenAI's switch to metered OneGov pricing. Anthropic has built the ceiling into the product.

It also replaces a price that was never real. In August 2025 Anthropic offered Claude to all three branches of government for $1 as a one-year package. A capped meter is the first version of this product a budget office can forecast. Keep the per-tier dollar limits tight anyway. Our checklist for Claude Enterprise spend controls applies here almost line for line.

## Where the Conversation Record Actually Lives

The conversation record lives on the endpoint and nowhere else, unless you send it somewhere. Anthropic's security and data-handling guide for agency reviewers is unusually direct about it:

- Conversation content "stays on the user's device." The service proxies inference requests and "records only per-request metadata, not content."
- The server-side Compliance API "records identity and configuration events but never tool calls or conversation content."
- Each Chat and Cowork session writes a local audit log of tool calls, permission decisions and file operations, and "that log never leaves the device."
- The storage location "is fixed to the per-user application data directory." Anthropic's advice: "back up the application data directory through your endpoint management tools if you need to preserve it."
- The app does not encrypt these files itself. Protection at rest depends on BitLocker, FileVault or LUKS.
- Local history is not separated by organization. A user who moves between Claude for Government tenants on the same OS account sees the same conversations.

The desktop app's storage reference says what that means for deletion: "Conversation history exists only on this device... so deleting it is unrecoverable." Code-session transcripts are stored separately, in Claude Code's own folder under the user's home directory.

Steel-man this first, because it is a strong design. A vendor that holds no content cannot leak it, be subpoenaed for it, or train on it. For controlled unclassified information that is a real advantage. Critics such as Containment.ai argue that FedRAMP authorizes the infrastructure, not what employees paste into a session. The guide agrees on one point: Claude for Government has "no inline content-inspection or DLP gate."

## Why NARA's New AI Memo Makes This Your Problem

NARA's August guidance makes the agency, not the vendor, responsible for deciding which AI material is a record, and the device-local design leaves the agency nowhere else to look for it. The memo, issued August 21, holds that using an AI platform does not by itself create federal records. Its text makes three points that matter here:

1. **Capture is the trigger.** Information "passively retained by a third-party platform such as ChatGPT or Claude is not necessarily 'received' by an agency, unless it is downloaded or otherwise captured in an agency system and used for official purposes."
2. **Some prompts are records.** That includes prompts "captured and saved within an agency system" that are circulated or used for official purposes. One example is FOIA staff keeping the prompts they used to search records in the case file, "as evidence that may be necessary to document the search in a declaration."
3. **Each agency decides.** "Each agency has principal responsibility for determining whether an AI material is a federal record." The memo also says it "does not establish policy related to... e-discovery."

Read that against the architecture. On a web-hosted chatbot, NARA's "passively retained by a third-party platform" language works like a safe harbor (the memo says such material is "not necessarily" received): it sits with the vendor until someone captures it. In Claude for Government, it never sits with the vendor. It sits in an application folder on an agency-managed laptop. Whether that folder is an "agency system" is a question for your general counsel. The memo does not answer it, and your records officer should not have to guess.

The memo has critics. Writing in Just Security, one analysis argues that the "received" test is far narrower than NARA's 2015 guidance on electronic messages. It also notes that the memo leaves out the procurement language that earlier cloud guidance required of vendors. If that critique wins, through litigation or a revised memo, agencies whose AI records exist only on endpoints will be the ones re-collecting.

## The Last Time Records Lived Only on the Device

The failure here is a laptop refresh, not a hack. In January 2021 the Secret Service began factory-resetting its phones in a pre-planned, three-month migration. The DHS inspector general later said that texts from January 5 and 6 were erased after the IG had requested them. The agency disputed that account and said none of the requested texts had been lost. It did not matter. The months of argument were about one thing: no one could show that a copy existed anywhere except the devices.

A device-local AI history recreates the same exposure on every laptop that runs Claude for Government. A hardware refresh, a reimage after a malware alert, or an employee clicking "delete" each destroys the only copy. If a litigation hold or an FOIA request covered that history, your agency now has a spoliation problem instead of a production problem.

## The Controls That Close the Gap

The product ships the tools to close this gap. All of them are off by default, and switching them on is the agency's job.

- **Content capture to your own collector.** Set a Telemetry endpoint and Telemetry content capture in the admin portal. Claude Desktop then sends the categories you choose (prompts, Claude's responses, tool inputs, tool results, or full requests and responses) to an OpenTelemetry collector you run, "and never to Anthropic." Nothing is selected by default. Tool results are delivered only while the beta traces setting is on. One operational warning from the same page: if the collector refuses requests or is unreachable, "members see no error," and the telemetry reference says the app drops the affected batches. A collector outage is therefore a silent gap in your record. Monitor it the way you monitor journaling for email.
- **Retention and legal hold keys.** The desktop storage reference documents per-kind auto-deletion clocks from 1 to 3,650 days, and a`sessionRetentionHold` key that suspends all automatic deletion "for example under a legal hold." By default, sessions stay until the user deletes them. Confirm with your Anthropic representative which of these keys your tenant's configuration delivers, and test a hold on a real device before you depend on it.
- **Endpoint backup.** Because the location is fixed, your existing backup and eDiscovery collection tooling can target it by path. Add it to the reimage runbook, so nothing is wiped before the folder is collected.

None of this is exotic. It is the same discipline that Microsoft 365 Copilot's memory forced on incident responders, applied to a different storage location.

## What to Do Before You Sign the ATO

**This Week:**

1. Put your records officer, general counsel and CISO in one meeting with a single question on the agenda: is a Claude for Government conversation folder on an agency laptop an "agency system" under NARA's August 21 memo? Write the answer down. It decides everything below.
2. Ask your Anthropic representative in writing which retention, hold and content-capture settings your tenant supports, and on which Claude Desktop versions.
3. If you are budgeting under the CR, size the first not-to-exceed increment for a pilot only. Obligate a small amount now and more after December 11.

**This Month:**

1. Stand up an OpenTelemetry collector inside your boundary. Turn on content capture at least for prompts and responses, for the user groups whose work produces records: FOIA, acquisition, policy and casework. Alert on the collector, not just the app.
2. Map captured AI material to existing schedules. NARA's memo says most AI records follow the schedule of the output they produce, and short-lived ones fall under GRS 5.2 as transitory or intermediary records. Write that mapping into your AI use policy.
3. Add "collect the Claude application data folder" as a gated step in the device refresh and reimage runbook.

**Before the CLI and Microsoft 365 Leave Early Access:**

1. Scope Claude Code and Claude for Microsoft 365 as separate ATO line items. Claude Code transcripts live in a different folder from Desktop history, so your collection rules must cover both. Our notes on Claude Code's permission defaults list the managed settings to pin first.
2. Decide now whether you will allow Auto mode. It is blocked by default, and switching it on changes what the audit trail has to show.

## The Bottom Line

Agencies have done this before. Email moved records off paper and onto servers, and federal records rules spent years catching up with journaling and capture. Text messages moved them onto phones, and the gap appeared in the worst possible case. Claude for Government moves AI work onto the endpoint, deliberately and for good security reasons. It also hands the agency a meter it can budget for, which is more than most AI vendors have offered the government.

Take the meter. Then build the capture path before the first laptop refresh, not after the first FOIA request.

The cap tells you how much you will spend. Nothing in the product tells you what you will be able to produce.
