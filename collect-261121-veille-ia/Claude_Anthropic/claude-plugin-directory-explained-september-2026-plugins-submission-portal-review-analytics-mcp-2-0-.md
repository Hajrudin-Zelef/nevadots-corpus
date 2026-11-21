---
id: collect-261121-veille-ia/claude-anthropic/claude-plugin-directory-explained-september-2026-plugins-submission-portal-review-analytics-mcp-2-0-
title: "Claude plugin directory explained (September 2026): plugins, submission portal, review, analytics, MCP 2.0, MCP Apps, Enterprise Managed Auth"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["claude", "directory", "plugins", "plugin", "documentation", "marketplace", "september", "portal"]
source: https://max.nardit.com/articles/claude-plugin-directory-explained
source_anchor: ""
source_lines: [1, 226]
section: "Claude_Anthropic"
---
## Contents

On 25 September Anthropic opened the Claude directory to outside developers. Anyone on a paid Claude plan can now package an MCP server, a set of Agent Skills, or both, submit it through a portal, and, once it passes review, have it listed where Claude users browse for extensions. The announcement calls these packages plugins and says they are now “the main way to build third-party extensions for Claude.”

The shorthand I have seen most since then is “Anthropic launched a plugin marketplace.” It blurs two things. The plugin directory is free distribution with a review step that the launch post makes sound lighter than the documentation does, and nothing in it lets you charge. Paid products go through a separate application on Claude Marketplace and get bought out of a company’s committed Anthropic spend. And the trust that the word “reviewed” implies covers less than people assume.

I ship two open-source MCP servers, so I read the documentation the way a submitter would, and then checked both of my servers against the rules. Neither is ready to submit unchanged, and the reasons say a lot about what the directory is built for. Everything below is from Anthropic’s launch post, its documentation, the MCP specification, and the vendors’ own skill and plugin documentation where I compare them, as of 4 October 2026. Where the announcement and the documentation disagree, I follow the documentation.

Two things, two days apart. On 23 September, Claude Marketplace: a website for browsing plugins, connectors, partner products and service partners, which the announcement says already holds “more than 2,000” connectors and plugins from companies like Atlassian, Google, Microsoft, Notion and Salesforce. On 25 September, the part that matters to developers: a submission portal at `claude.`, open to anyone on a paid plan, with no partner program to join first.

The two are connected but not the same. For plugins and connectors, Claude Marketplace has no submission of its own: you submit to the directory, and the directory is what Claude users see under Customize > Plugins in the app. The documentation is explicit about it:

```
If you came here from Claude Marketplace, the website where you browse plugins, connectors,
partner products, and service partners, you're in the right place: there's no separate Claude
Marketplace submission, and you submit to the directory instead
```
What changed is who can submit. The directory that partner integrations were already in now takes submissions from anyone on a paid plan, through one portal and one review pipeline.

Marketplace does have a paid side, and it is easy to miss. The same announcement invites anyone “selling a Claude-powered agent or product” to apply to list it, and says teams “can then use a portion of their committed Anthropic spend to buy it.” That is a separate application, for products sold to companies with an Anthropic commitment, not a checkout for plugins. Keep the two apart and most of the confusion about this launch goes away.

A folder. Specifically, a folder with a manifest at `.claude-plugin/` and any combination of component folders next to it. It is the same format Claude Code has used for plugins since October 2025, which is the quiet headline of this launch: a Claude Code plugin and a directory plugin are the same artifact.

```
my-plugin/
  .claude-plugin/
    plugin.json        # name, version, description, author, license
  .mcp.json            # MCP servers the plugin brings
  skills/
    my-skill/
      SKILL.md         # an Agent Skill: name, description, instructions
  commands/            # slash commands (Markdown files)
  agents/              # subagents (Markdown files)
  hooks/
    hooks.json         # lifecycle hooks
  README.md
  LICENSE
```
The manifest is small. The example in Anthropic’s own documentation:

```
{
  "name": "expense-reports",
  "displayName": "Expense Reports",
  "version": "1.0.0",
  "description": "File, track, and approve expense reports from a conversation, using your finance system's connector and your company's approval rules.",
  "author": { "name": "Example Corp", "url": "https://example.com" },
  "license": "MIT"
}
```
MCP servers are declared in `.mcp.json`, either as a remote server by URL or as a local command the app starts. Note the `mcpServers` wrapper, which is easy to drop by accident:

```
{
  "mcpServers": {
    "expenses": {
      "type": "http",
      "url": "https://mcp.example.com/mcp"
    }
  }
}
```
Skills are plain `SKILL.md` files in the open Agent Skills format: a name, a description, and instructions that Claude loads when the description matches the task. One warning from the documentation is worth repeating in full, because people will get it wrong: don’t put API keys or other secrets in the MCP file, “because every person who installs the plugin receives its files.”

The directory also distinguishes two kinds of listing, and the difference shapes everything that follows.

|  | Plugin bundle | MCP connector | 
|---|---|---|
| What it is | A plugin folder: skills, commands, agents, hooks, MCP server references | One remote MCP server | 
| Where it comes from | A GitHub repository, public before the listing goes live | The server’s `https://` URL, no repository | 
| How it is reviewed | Validation and a security scan on every version, a person reviews each new listing | Automated policy scan, listed as Community by default | 

Skills alone are not a submission type. If all you have is a skill, it goes into a bundle. And one more name to keep separate: `claude-plugins-official`, Anthropic’s own Claude Code marketplace, doesn’t take submissions through this portal. Self-hosted Claude Code marketplaces, a Git repository with a marketplace file, still work as a distribution route of their own; they just don’t get the directory’s review or reach.

Not everywhere a plugin can be installed. A plugin added on claude.ai is saved to your account and follows you into Cowork and Claude Code, but each surface loads a different subset of it, per the platform support page. This table is the one I wish the launch post had led with:

| Component | Chat (web, desktop, mobile) | Cowork | Claude Code | 
|---|---|---|---|
| Skills | Load | Load | Load | 
| Commands | Load as skills | Load | Load | 
| Agents, hooks | Ignored | Load | Load | 
| Remote MCP server (fixed URL) | Connect it from the plugin’s Connectors tab | Loads once connected | Loads | 
| Local MCP server (a command the app starts) | Ignored | Only when the session runs on your computer | Loads | 
| Executables in a top-level `bin/` | Plugin can’t be installed | Plugin can’t be installed | Loads | 

The practical consequence: if your tool is a local process, a plugin delivers it to Claude Code and to Cowork on the user’s own machine, and to nobody in chat. Chat users get your skills and nothing else. Anything with a top-level `bin/` folder is refused outright by claude.ai and Cowork.

From Customize > Plugins > Discover in claude.ai or the desktop app. Directory plugins appear on Pro, Max, Team and Enterprise plans. Select one, read what it contains, select Add. It then reaches Cowork the next time you start a task, and Claude Code the next time you start a session signed in to the same account, where it shows up as a synced plugin. Claude Code users can also browse with `/plugin directory` from version 2.1.287.

Two details are easy to miss here. Adding a plugin does not connect anything: “Adding a plugin doesn’t add a connector to your account or sign you in to anything.” You connect a bundled remote server separately, from the plugin’s Connectors tab. And updates arrive silently:

```
Get updates: plugins from a marketplace or the directory update from their source. After a new
version syncs from the source, you get it on your account automatically, with nothing to accept.
```
To manage a plugin you can disable it with a toggle or remove it from its menu; a connector it brought along is disconnected separately. On Team and Enterprise plans, an Owner decides what members see (admin controls). Each plugin can be Not available, Available to install, Installed by default (members can turn it off), or Required (always on, members can’t remove it). The default access an Owner sets for Anthropic’s directory as a whole is limited to Not available or Available to install, and an Owner can remove the directory as a source altogether. Enterprise adds availability per user group. Claude Code on the command line has its own policy layer, managed settings that allowlist or block marketplaces and force installs.

Anyone on Pro, Max, Team or Enterprise. Free accounts can’t. On Team and Enterprise, an Owner submits, and on Enterprise an Owner can grant a Directory permission to other members through a custom role. The listing belongs to the organization you submit from, and for a bundle the first organization to submit a given repository folder holds it, so submit from the one that should own the listing long term. An organization can create up to ten submissions in any 24 hours, and saved drafts and withdrawn submissions count toward that.

You give the portal an `https://` URL and walk through Connection, Tools, Listing, Use cases, Company, Authentication, Data handling, Test & launch, Compliance, and Review. A few requirements will decide whether you pass:

- The server must be remote and reachable over HTTPS. A local server can’t be submitted as a connector; it goes into a bundle.
- Every tool needs a `title` and either a`readOnlyHint` or a`destructiveHint` annotation. The portal flags tools without them.
- You provide credentials for a fully populated test account, visible only to reviewers.
- Authentication is OAuth with dynamic client registration, OAuth with a Client ID Metadata Document, Anthropic-held client credentials, a custom connection where users bring their own URL or credentials, or none.
- Seven policy acknowledgments, all required, covering the directory guidelines, first-party API usage, financial transactions, AI media generation, prompt injection, conversation data collection and public documentation.
- Listing material: documentation and privacy policy URLs, a support contact, an icon, and a URL slug that is permanent once published.
- If your server uses MCP Apps, three to five PNG screenshots at least 1000 pixels wide, each with the prompt that produced it. Video and GIFs are not accepted.

The authentication page documents several Claude-specific OAuth requirements for hosted connectors: a `401` is required to start sign-in, Claude uses only the first entry in your `authorization_servers` metadata, it picks a Client ID Metadata Document only when your metadata advertises both `client_id_metadata_document_supported: true` and `none` as a token endpoint auth method, your server must support S256 PKCE, and the hosted redirect URI to register is `https://`. Claude Code signs in through a loopback callback instead, so your server must match `localhost` and `127.0.0.1` regardless of port. Discovery, registration and token endpoints get 10 seconds to respond, refresh requests 30. Anthropic-held client credentials and custom connections need arranging with the review team, and static-header auth is in a limited beta. A machine-to-machine `client_credentials` grant isn’t supported. Anthropic’s traffic comes from `160.79.104.0/21`, which is worth knowing if you filter by IP.

Before the portal, test locally: `claude plugin validate ./my-plugin` catches part of what the portal checks, and `claude --plugin-dir ./my-plugin` lets you try the plugin for real. Then test it on every surface you care about, because chat, Cowork and Claude Code load different parts of it. You connect your GitHub account on claude.ai (the portal checks you can push to the repository, and a private repository also needs the Claude GitHub app), then: Open the portal, Enter the source and validate (repository, optional plugin path, optional branch or tag), Check the listing details (read from `plugin.json` and the README), Answer the data handling questions (personal data, data sent to undeclared services, retention, intended for under-18s), Complete the compliance step, Review and submit.

The repository can stay private during review. The price is that its source is uploaded to Anthropic for scanning and is visible to reviewers, and it must be public before the listing goes live. A Claude Code marketplace repository with several plugins is not one submission: each plugin folder is validated and submitted on its own, and you can’t change the repository or folder after submitting.

After the first submission, you release the way you already do. Merge to the tracked branch, and the directory picks up the commit (by GitHub webhook by default, or on a schedule) and scans it. Publication then follows your plugin’s publishing setting, which by default means you request it and a reviewer publishes the version, as described below. Users keep the last published version until its replacement is published, and if a new version fails the security scan, later versions wait until a reviewer clears the plugin. If your `plugin.json` sets a version, raise it with every release. To leave, plugins are delisted from the portal; connectors by email.

Every bundle version is scanned, and new listings also get a human. That catches sloppy and deceptive packaging. It does not vouch for behaviour.

Validation runs when you press Validate and again on every new commit. Its results come in four levels: Blocks, Held for a reviewer, Warning, Note. The things that block a submission are mostly hygiene:

- a README shorter than 40 words, or no license;
- a name that is reserved (`claude` ,`anthropic` ,`official` ,`plugin` ,`mcp` or`test` as the whole name); a look-alike of a known brand is held instead;
- a launcher that runs an unpinned package. `npx <package>@1.2.3` or`uvx <package>==1.2.3` pass, a range or`@latest` does not;
- a real credential anywhere in the files, including documentation and examples;
- a remote MCP URL that isn’t `https://` or`wss://` ;
- any file over 5 MiB, or a repository over the limits (50 MiB archived, 256 MiB unpacked, 10,000 entries).

Some choices normally send the plugin to a human instead, unless another rule blocks it first: binaries and compiled executables under the size limit, files other than images or fonts over 256 KiB, more than 512 files, packages installed from a lockfile, a pinned `npx` or `uvx` package (because “the package’s own dependencies resolve at install time”), and a plugin that reads a credential already set in the user’s environment and sends it to a server. A hold is not a rejection; it means a person looks before the version goes further.

Then the security scan, in Anthropic’s words:

```
The security scan looks for behavior that a plugin doesn't disclose, such as sending data
elsewhere, running hidden code, or changing Claude's permission settings.
```
A first submission that fails the scan is rejected, and a later version that fails can’t go live. The documentation’s advice follows from that: describe in the README everything the plugin runs, sends or fetches, and commit readable source rather than compiled or minified code, because what the scan can’t read is held. It also adds, fairly, that a complete README doesn’t make a behaviour allowed.

And then the part the launch post softens. The post says “Publish when you’re ready.” The documentation says this:

```
A version that passes every check isn't live until it's published. When the version passes,
select Publish on the plugin's page. By default, the portal records this as a request for an
Anthropic reviewer, who then publishes the version.
```
Self-publishing exists in two forms, a reviewer publishing only your first version, or you publishing from the first version, and the documentation describes the second as a setting an Anthropic reviewer applies when approving your plugin. You don’t pick it yourself. Review time “isn’t fixed.” Statuses run Draft, Scanning, Needs changes, In review, Approved, Published, plus Not live yet, Delisted and Withdrawn, and the documentation is careful to note that Approved is not installable: only Published is.

Mostly, the future. A plugin bundle references a remote MCP server by URL, and the bundle scan reads the files you uploaded. The server gets its own review when it is listed as a connector: Community connectors are screened, and in a Verified review a person tests each tool. But that is a test of the server as it was that day, and Anthropic says plainly what it is not:

```
Verification means Anthropic has reviewed the connector more closely than a Community
connector. It isn't a security audit or a guarantee of how the connector will perform. The
developer operates the connector and controls its tools, which can change after review.
```
Put that next to two other facts from the same documentation. Tool changes on a connector deploy on the server, with no resubmission. Plugin updates reach users “automatically, with nothing to accept”, although every new bundle version is scanned again. The behaviour that matters lives where the developer controls it, and it can change after review without anyone being asked. I don’t read that as a flaw in Anthropic’s design. Reviewing a remote service once is all anyone can do. It is a reason to read “listed in the directory” as “this was checked when it was listed”, which is a smaller claim than “this is safe”. Review also covers only the directory: plugins added from a marketplace URL or uploaded by hand get none of it. I wrote about the general version of this a few weeks ago: a connector is an untrusted author, and the directory does not change that.

A Usage tab that covers up to 90 days and exports to CSV. It counts installs, active accounts and retention, splits installs by surface and by where they came from (the Discover tab, for instance), shows the share of accounts on each version, and tells you how often each skill, command, agent, hook and MCP server actually gets used. For quality it reports load errors per Claude Code version, plus calls, error rate and latency for each MCP server. And there is a directory funnel: listing views, install clicks, installs. Figures are computed once a day in UTC, and until real usage arrives the tab shows sample numbers marked Preview.

Connectors get a separate dashboard: a health badge (Healthy at up to 2% request errors, Worth a look above 2%, Degraded above 5%), a directory rank with a Trending tag, tool calls and error rate over 30 days, latency percentiles by product, and the top 15 tools.

The launch post also promises you will see “which searches lead people to it.” The documentation’s list of metrics does not include search terms, so treat that metric as a promise until it shows up in the tab. And the connector dashboard is frank about its blind spot: “Connections that people make to your server from other MCP clients, and local servers that run on a user’s own machine, aren’t visible to Anthropic and aren’t counted.” The plugin Usage tab doesn’t say whether a local server inside a plugin is counted, so don’t assume it is.

Not as a specification name. The MCP specification uses dated revisions, and the current one is 2026-07-28. The launch post says Claude supports “the latest MCP spec, commonly referred to as MCP 2.0”, which is a nickname, most likely borrowed from the SDKs: Claude Code’s documentation describes “MCP TypeScript SDK 2.0, which adds MCP protocol revision 2026-07-28.” MCP revisions are identified by date; “2.0” is not one of them.

The revision itself is real and breaking. Protocol-level sessions are gone, and so is the `initialize` handshake: every request declares its protocol version in `_meta`, servers must implement a `server/discover` call that returns their versions, capabilities and identity in one request, and clients can call it before anything else. There is a formal `extensions` mechanism. Roots, Sampling, Logging and Dynamic Client Registration are deprecated. The specification documents fallback paths that let implementations keep talking to older, handshake-based servers, so existing servers aren’t cut off by the revision itself.

One tension worth knowing if you build connectors: Claude’s connector authentication documentation still lists the 2025-03-26, 2025-06-18 and 2025-11-25 authorization specifications, and says Claude doesn’t yet support resource subscriptions, sampling, or “advanced or draft capabilities.” “Supports the latest spec” is accurate for the core. It is not a promise that every part of the latest spec works in Claude.

A way for an MCP server to attach an interactive interface to a tool, alongside the text result that clients without UI support still get. It is an official extension, `io.`, stable since January 2026. A tool’s description carries a `_meta.ui.resourceUri` pointing to a `ui://` resource, an HTML page with its scripts and styles. The host fetches it, renders it (web hosts typically in a sandboxed iframe, Claude mobile in a native WebView) and talks to it over `postMessage`. From the specification:

```
The sandbox prevents your app from accessing the parent window's DOM, reading the host's
cookies or local storage, navigating the parent page, or executing scripts in the parent
context.
```
The app can ask for extra permissions and declare which external origins it may load from, and the host decides what it allows. In Claude, the user is asked first: Allow, or Always allow for a server they trust.

Claude on the web, Claude Desktop and Claude mobile render MCP Apps. So do VS Code with GitHub Copilot, Microsoft 365 Copilot, Goose, Postman and several others, according to the MCP project’s client matrix. I could not find a Claude document saying whether Cowork or Claude Code render them, so if your plugin’s value is its interface, test it on the surfaces your users actually use. Since November, which MCP Apps features a user gets also depends on how they installed the connector: what a Community install loses.

Single sign-on for MCP servers, so employees don’t click through an OAuth consent screen for every connector. The specification calls it Enterprise-Managed Authorization. When a user signs in to Claude through the company’s identity provider, the client obtains a signed identity assertion, an ID-JAG, from that provider. It then exchanges the assertion for an access token at your server’s token endpoint, using the standard JWT bearer grant from RFC 7523. No browser redirect, no consent page, and the company’s identity provider decides who gets access.

It comes with limits. In Claude it is available on Team and Enterprise plans only, generally available since 24 August 2026. Okta is the only identity provider at launch. It can’t be combined with dynamic client registration. And it isn’t automatic for any company with SSO: an administrator has to configure it, and your authorization server has to trust the customer’s identity provider and advertise the JWT bearer grant. If your users are individuals rather than companies, this extension is irrelevant to you.

The skills, yes. The packaging, only with some work.

The skills travel. Agent Skills is an open format, and `SKILL.md` folders load in OpenAI Codex and ChatGPT, GitHub Copilot, Cursor, Gemini CLI and dozens of other clients. GitHub Copilot and Cursor even read `.claude/skills`. Write a good skill once and most of the industry can use it.

The bundle doesn’t. On 6 August 2026 AWS, Cursor, Microsoft, OpenAI and Vercel published Agent Plugins 1.0.0, a vendor-neutral plugin standard. Anthropic is absent from its published steering committee. Its manifest is a `plugin.json` at the plugin root and its MCP file is `mcp.json`, where Claude’s are `.claude-plugin/` and `.mcp.json`. Claude’s documentation doesn’t mention the Agent Plugins layout. OpenAI’s goes some way towards Claude: the ChatGPT desktop app reads a Claude-style `.claude-plugin/`, and Codex loads plugin hooks through its own extension. But its plugin documentation also warns against simply renaming the MCP file, because the portable format declares a transport `type` for each server.

So the practical answer is: one repository can serve Claude and the Agent Plugins clients if it carries both manifests, and both MCP files if it ships servers. Both formats keep skills in `skills/<name>/SKILL.md`, so the skills themselves are shared. Hooks are outside the portable core and stay client-specific. The industry converged on what goes in the repo. It has not converged on the wrapper.

Neither is ready to submit unchanged. The reasons say more about the directory than about my code.

clipboard-mcp is a Rust server that reads and writes the clipboard. As a connector it doesn’t work in its current shape. A remote connector runs on infrastructure I host, and Claude reaches it from Anthropic’s address range, so a hosted clipboard server would expose the clipboard of my server, not the user’s. Its existing HTTP mode doesn’t change that: it binds to localhost and has no authentication. Getting the user’s clipboard into a connector would take something new, an agent on the user’s machine and a secured relay to it. The general rule: a tool whose value is access to the user’s own machine needs more than a URL to become a connector.

As a bundle it runs into the binary rules. My local release build is about 7.9 MB, which exceeds the 5 MiB per-file limit, so committing it stops validation. A compiled executable under the limit would still be held for a reviewer. Putting it in a top-level `bin/` makes claude.ai and Cowork refuse the plugin. Pointing `.mcp.json` at a downloadable bundle is blocked, and standalone desktop-extension listings are gone; an `.mcpb` committed inside the plugin is held, and at this size the file limit stops it first anyway. Another option is to require a separately installed binary, which is held because the command isn’t a file inside the plugin, and which ends one-click install. Any of these would work in Claude Code and local Cowork only, because chat ignores local servers.

agent-recall is a Python memory server over a local SQLite file. As a connector it would need a remote transport and an access-control design it doesn’t have; it is stdio-only today, and local-first on purpose. As a bundle it is plausible but not clean. Its server and hooks depend on a prior `pip install`, and the checklist wants every file a hook or server uses inside the plugin folder. A pinned `uvx` launcher passes the pinning rule and is still held for review, because its dependencies resolve at install time. And packaging is only half of it: a memory server that saves things proactively has to square that with the directory policy’s limits on collecting conversation data, which a repository audit like this one can’t settle.

It also showed me the gotcha I would put first in any checklist. When an MCP server comes from a plugin, Claude Code namespaces its tools as `mcp__plugin_<plugin-name>_<server-name>__<tool-name>`. The documentation says it outright:

```
A hook matcher written against the bare server key, such as `mcp__database-tools__.*`, never
fires for a plugin-bundled server.
```
My own plugin is the odd case. It ships hooks but no server: its `.mcp.json` is empty, and the README has people add the server themselves under the key `memory`, so today the bare matcher is right. The moment I bundle the server into the plugin, that matcher stops firing, with no error, and so does the handler behind it, because it filters on the bare tool names too. Nobody would notice until memories quietly stopped being written. If you have hooks keyed to MCP tool names and you move the server into a plugin, rewrite the matchers and whatever code checks the names.

The pattern across both: the directory is built for remote servers with real authentication, plus skills that teach Claude to use them. Local tools reach Claude Code and local Cowork, and the rules that protect users (readable source, nothing resolved behind the reviewer’s back at install time, every file inside the folder) are exactly the rules that send local tools to a human reviewer.

If you run a remote service with a proper OAuth setup, the documentation’s own recommendation is the right one: submit twice. The server as an MCP connector, then a plugin bundle whose skills teach Claude how to use it, with the bundle’s `.mcp.json` pointing at the same URL so people who have both see one set of tools. You get a listing, review, a chance that Anthropic escalates you to Verified, analytics, and on Team and Enterprise, admins who can make your plugin the default.

If your tool is local, ship it to Claude Code users through a plugin, keep everything inside the plugin folder in readable source, and accept that chat users get only your skills.

If you hoped to sell a plugin, the directory won’t do it for you. There is no pricing, revenue share or payout anywhere in the launch post, the directory documentation, the Directory Terms or the Directory Policy, and the policy goes further: software that moves money or executes financial transactions is an unsupported use case, and so is software that serves ads or sponsored content. A business model sits behind your own server and your own accounts, as it did before. If what you sell is a whole product to companies, Marketplace’s separate paid application is the door to look at.

For plugins, a directory, not a marketplace: distribution and review, with the selling done elsewhere. Making the Claude Code plugin format the directory format was the smart call. If you already built a Claude Code plugin, you are most of the way there.

The review reads what you upload, and a connector review tests the server as it was that day. What the server does after that, and what the next silent update does, stays between the developer and the user. Read the listing badge accordingly. And if you publish, write the README as if it were the contract, because for the scan it is, while remembering the checklist’s own warning that describing a behaviour doesn’t make it allowed. For my two servers, the next step is not a submission. It is fixing the packaging, then answering the policy question honestly.

## **01** / Discussion

## **02** / More articles

1. ### Agent memory as Markdown documents: a readable memory is still a memoryMoving agent memory out of a vector store and into Markdown files makes it easy to inspect, and that is worth having. Staleness, conflicting writers and the page nobody opened were never problems of storage format, so they move into the folder intact, where a clean file can pass for a checked one.
2. ### Amazon vs Meta's Muse shopping agent: why session custody, not agent identity, is the lineAsking a shopping agent to say who it is gives the site a switch for turning the agent off. That is useful for commerce and permission, and it is not a safety line. The line that protects a user is custody of the logged-in session: what the agent can reach inside it, and whether the user can see that and revoke it.
3. ### Decision models vs LLM-as-a-judge: Jev, OpenAI's Decisions API, and who calibrates the probabilitySeven places vendors want a typed answer to replace a free-text judge, what Red Hat's benchmark found, and why every vendor in the category, including the one that sells calibration, ends its documentation by handing the threshold back to you.
