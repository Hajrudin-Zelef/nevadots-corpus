---
id: collect-261121-veille-ia/divers-tech/github-would-like-to-be-everywhere-you-write-software-adtmag
title: "GitHub Would Like to Be Everywhere You Write Software -- ADTmag"
domain: divers-tech
role: reference
task: article
actors: []
dates: ["2026-08-31"]
keywords: ["github", "copilot", "developers", "would", "agent", "like", "software", "plugins"]
source: https://adtmag.com/blogs/watersworks/2026/08/github-would-like-to-be-everywhere-you-write-software.aspx
source_anchor: ""
source_lines: [1, 126]
section: "Divers_Tech"
---
### GitHub Would Like to Be Everywhere You Write Software

    There was a time when explaining GitHub was easy. It's where developers keep their code.

Technically, that's still true, but it's quickly becoming about as complete a description of GitHub as saying an iPhone is something you use to make phone calls.

Spend a few minutes browsing the GitHub Changelog for August, and you might get the feeling that GitHub would prefer developers never have to leave. Or, more accurately, that GitHub would like to come along wherever devs happen to go.

GitHub Copilot is moving into Slack and Microsoft Teams. Agent plugins can travel among compatible coding agents. Copilot can remember things between sessions. Ollama models can run through Copilot in JetBrains IDEs. Enterprises get more control over agents and plugins. GitLab customers can migrate repositories directly into GitHub Enterprise Cloud.

    
There's a lot going on in what was once a simple Git repository hosting service. In fact, it's worth remembering how modestly this all began.

 From Git Repository to Development Platform
  GitHub was 

founded in February 2008
, initially as a hosted service built around Git, the distributed version control system Linus Torvalds created for Linux development.


The genius of GitHub wasn't Git itself. It was making Git social.

Repositories became places where developers could see one another's work, fork projects, submit pull requests, discuss changes, and build communities around software. GitHub eventually became one of the centers of the open-source world and, increasingly, a home for enterprise development.

Microsoft saw where things were going early on. It announced plans to acquire GitHub for $7.5 billion in 2018, promising that it would remain an open platform. Redmond completed the acquisition that October.

The reaction among developers was, shall we say, mixed. Microsoft had spent decades as one of the companies open-source developers loved to hate, and GitHub was one of their favorite places on the Internet. So, sticking with the platform required a certain amount of trust.

  Eight years later, GitHub is still GitHub. But it's also Actions, Codespaces, Advanced Security, Copilot, enterprise governance, CI/CD infrastructure, and now an increasingly ambitious collection of AI agents.

  The repository was the beginning; the platform around it keeps getting bigger.

Now Copilot Has a Memory
One of the more interesting August changes sounds almost trivial: GitHub Copilot can now remember things.

An update to GitHub Copilot for JetBrains adds persistent memory across agent chat sessions. Instead of repeatedly explaining the same project details or preferences, developers can allow Copilot to retain useful context.

Anyone who has spent time repeatedly telling an AI coding tool the same thing can appreciate the appeal.

The JetBrains update also adds Ollama as a Bring Your Own Key provider, allowing developers to select Ollama models through Copilot. GitHub expanded enterprise controls at the same time, including management of MCP server access and plugin availability.

That combination is revealing.

GitHub is not merely trying to supply the AI model anymore. It is increasingly trying to supply the environment in which developers choose models, give them context, connect them to tools, and decide what they are allowed to do.

Which brings us to plugins.

One Plugin, Many Agents
  On Aug. 12, GitHub announced support for 

Agent Plugins 1.0
 across 

VS Code
, 

Copilot CLI
, the 

GitHub Copilot SDK
, and the GitHub Copilot app. Agent Plugins 1.0 is an open standard published with AWS, Anysphere, Microsoft, OpenAI, and Vercel. Google joined as a core maintainer.


The idea is straightforward: build an agent plugin once and use it across compatible agent clients.

Before this, developers could package the same underlying capabilities for different agents, but they had to maintain separate manifests and directory structures. Agent Plugins provides a common packaging format for things such as agent skills and MCP servers.

That may sound like plumbing, but hey, plumbing matters.

One of the questions hanging over agentic software development is whether every AI coding environment will become its own little kingdom, with proprietary extensions and incompatible ways of connecting agents to tools.

Agent Plugins represent an attempt to keep at least part of that ecosystem portable.

It also creates another governance problem, so GitHub is addressing that, too. Business and Enterprise customers can use managed settings to approve or block plugins and control which plugin marketplaces developers can use.

Apparently, even autonomous coding agents eventually acquire an IT department.


Copilot Has Left the IDE
  Then things get more interesting.

GitHub announced Aug. 21 that Copilot is coming to Slack in public preview.

Mention @GitHub in a Slack conversation, and Copilot can use the discussion and permitted GitHub context to investigate a problem, work on code, and open a pull request.

The agent can keep working asynchronously while everyone else continues arguing in Slack about what the feature was supposed to do in the first place.

More seriously, this changes where development work can begin. A bug report does not necessarily have to become an issue, then a developer assignment, then an IDE session before anyone touches code. The conversation itself can become the starting point for an agent session.

GitHub is doing something similar with Microsoft Teams. Teams users can bring Copilot into a discussion and collaboratively direct an agent session.

That is a subtle but important shift. Copilot started as something developers encountered while writing code. Now GitHub wants the agent present when people are deciding what code needs to be written.

And GitLab Users, Come on Over

  Not every August announcement was about AI. 

GitHub also made self-service migrations from GitLab to GitHub Enterprise Cloud generally available through GitHub Enterprise Importer. Organizations can migrate repositories from GitLab.com and supported GitLab Self-Managed installations using GitHub's `gh gl2gh` command-line extension. The tooling supports individual repository migrations and scripted bulk migrations.

There is nothing subtle about the business strategy there. GitHub would be delighted to help you leave GitLab. But the timing makes the announcement more interesting than a routine migration-tool update.

GitHub is competing for more than just repository hosting. A company choosing GitHub today is increasingly choosing an ecosystem that encompasses source control, CI/CD, security, AI coding assistance, agent governance, and development workflows that extend to collaboration tools.

Migration tooling lowers the cost of entering that ecosystem.

The Repository Is Becoming the Center of Gravity

  This is where all those August announcements start fitting together. GitHub began by hosting the thing developers produced: code. Then it accumulated more of the work around that code.

GitHub Actions brought automation and CI/CD closer to the repository. Codespaces brought the development environment into GitHub's orbit. Advanced Security moved application security closer to the source. Copilot put AI into the act of writing software.

Agents are pushing the boundary outward again.

Now the work can begin in Slack. The agent can continue working in the cloud. Plugins can give it additional capabilities. Memory can carry context between sessions. Enterprise administrators can decide which of those capabilities developers are allowed to use.

Eventually somebody opens a pull request. And where does it land? *GitHub*.

That may be the most interesting thing about the company's current direction. GitHub does not necessarily need developers to perform every task on GitHub.com. It just needs GitHub to remain the connective tissue between those tasks.

That's a long way from a website for hosting Git repositories. 

Then again, GitHub has been moving away from that description almost since the day it launched.

 
	
Posted by John K. Waters on August 31, 2026
