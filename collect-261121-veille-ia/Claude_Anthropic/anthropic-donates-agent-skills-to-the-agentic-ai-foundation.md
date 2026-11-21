---
id: collect-261121-veille-ia/claude-anthropic/anthropic-donates-agent-skills-to-the-agentic-ai-foundation
title: "Anthropic Donates Agent Skills to the Agentic AI Foundation"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["agent", "anthropic", "skills", "foundation", "agentic", "standard", "github", "skill"]
source: https://dev.to/raxxostudios/anthropic-donates-agent-skills-to-the-agentic-ai-foundation-5ecp
source_anchor: ""
source_lines: [1, 52]
section: "Claude_Anthropic"
---
- On October 22, Anthropic gave Agent Skills, its name, logo, GitHub org and domain to the Agentic AI Foundation
- More than 70 agent platforms, including ChatGPT, Cursor and GitHub Copilot, already support Agent Skills
- Over 1 million skills are published and nearly 280 million have been installed since the October 2025 launch
- Nothing breaks for anyone writing or installing a skill today, but a security-guidance roadmap is coming

On October 22, Anthropic handed over something it didn't have to give up: Agent Skills, the open standard it built for packaging specialized knowledge into Claude Code, along with the standard's name, logo, GitHub organization and the agentskills.io domain. The destination is the Agentic AI Foundation, a vendor-neutral fund under the Linux Foundation that Anthropic co-founded with Block and OpenAI back in December 2025. It's the second big piece of plumbing Anthropic has moved there, after the Model Context Protocol made the same trip.

Agent Skills is the format behind the folders of instructions, scripts and reference files that teach an AI agent how to do one specific job well, instead of relying on whatever the model happens to already know. A skill for writing release notes in a house style is a skill. A skill for running a specific test suite correctly is a skill. Claude Code reads them, and so, as of this announcement, do a growing list of other agents that have nothing to do with Anthropic.

Handing the standard to the Agentic AI Foundation means Anthropic no longer owns the final word on where it goes next. The foundation already held the Model Context Protocol, donated in December 2025 alongside Block's goose and OpenAI's AGENTS.md as the fund's three founding projects. Platinum members include AWS, Google, Microsoft and Cloudflare on top of the three co-founders, so the group making decisions about Agent Skills going forward isn't one company's product team anymore.

Not just the spec, either. The name, the logo, the GitHub organization and the agentskills.io domain went with it. Anthropic could have kept the branding and donated only the technical document. It didn't.

I've read a few of these donation announcements now, and the pattern is always the same shape: a company builds something, it gets adopted past the point where one vendor controlling it still makes sense, and the standard moves to a neutral home. What's different here is the speed. MCP took a while to reach that point. Agent Skills got there in about a year.

Anthropic names two Lead Maintainers, David Soria Parra and Mahesh Murag, who'll run the spec going forward. The announcement is clear that maintenance now happens as individuals, not as representatives of whichever company signs their paycheck, with engineers from Anthropic, Google, OpenAI and others all contributing under the same process. Nobody at any one company gets the deciding vote by default anymore.

This is the part that actually surprised me, because I'd assumed Agent Skills was still mostly a Claude Code thing with a few stragglers bolting it on for compatibility points. That's not what the numbers say. More than 70 agent platforms have integrated the standard at this point, and the list isn't padded with unknowns. ChatGPT, Cursor, GitHub Copilot and VS Code are all in there.

That's a strange sentence to write. A standard born inside one company's coding tool is now something a competitor's product reads natively. It only makes sense if you stop thinking about Agent Skills as a Claude Code feature and start thinking about it as a packaging format, closer to a zip file with a manifest than to a proprietary plugin API. The format doesn't care which model reads it, which is exactly why a rival could adopt it without feeling like they were adopting someone else's walled garden.

Competing products rarely share plumbing this directly. They usually route around each other instead.

I build and ship small tools for a living, and the thing I keep coming back to is how much this lowers the cost of writing a skill in the first place. If a skill I write for my own workflow in Claude Code also runs, unmodified, in whatever agent a reader of this blog happens to use, the time I spend writing it stops being locked to one platform's lifespan. That's a real change in what "worth building" means for something this specific. A skill stops being a bet on one vendor staying relevant and starts being closer to a script I'd write anyway, one that happens to also work somewhere else for free.

The adoption numbers back up the donation, but the usage numbers are the ones that actually stopped me. Over 1 million skills have been published since the format launched in October 2025, and close to 280 million installs sit on top of that. Anthropic's own announcement notes that developers published thousands of skills on GitHub within weeks of launch, before most of the wider agent ecosystem had even picked it up.

Scale like that changes what a donation means in practice. Handing over a format nobody used yet would be a gesture. Handing over a format with hundreds of millions of installs already in the wild, while it's still growing, is Anthropic giving up unilateral control over something that's already load-bearing for a lot of other people's workflows. That's a harder thing to do than it sounds, and it's worth noticing when a company actually does it instead of just announcing intent to eventually figure it out.

A gesture costs nothing. This one did.

It also means the foundation isn't starting from a blank page. There's an existing install base, existing conventions, and existing expectations about what a skill is allowed to assume about its environment. The roadmap has to respect all of that from day one, which probably explains why the first items on it are cautious rather than ambitious. Nobody wants to be the foundation that broke a million published skills in its first year running the thing.

Mechanically, nothing changes this week. A skill that works in Claude Code today keeps working exactly the same way tomorrow. What's actually shifting is who gets a say in what the format becomes, and Anthropic has laid out where that's headed: a roadmap covering versioned releases, a formal proposal process, and security guidance aimed specifically at people who write and install skills.

That last item is the one I'd actually pay attention to if I were building on this seriously. A skill is instructions and sometimes scripts that an agent runs with real permissions, and right now the ecosystem is mostly running on trust. A format with 280 million installs and no formal security guidance yet is a gap, not a scandal, but it's the kind of gap that gets exploited the longer it stays open.

Trust doesn't scale forever on its own. Eventually something has to replace it.

The fact that the roadmap names security guidance explicitly, rather than something quietly skipped, is the detail that actually reassures me here.

Claude Blueprint, the setup kit I sell through RAXXO, ships six skills as part of the install. I didn't write them expecting the format to end up under a neutral foundation a year later, and I don't need to change anything about them today. But I'll be watching the proposal process once it's live, the same way I watched MCP's own spec changes earlier this year, because a format I build on being governed somewhere I can actually read the discussion is strictly better than one company deciding alone and telling me after the fact.

Anyone curious about where this goes can follow the project's Discord or its GitHub discussions directly, which is more than most open standards offer a solo developer at this stage. No membership fee, no application, no waiting for a company to decide your feedback is worth reading.

I also think the versioned-releases part matters more than it sounds like it should. Right now a skill written against one point in the spec's life mostly just works everywhere, because the format has been loose enough not to need strict version checks. That gets harder to promise once the spec starts moving under a committee process instead of one team's internal roadmap. A formal proposal process is good for the standard's long-term health. It also means a skill I write today could, in principle, need updating a year from now to stay compatible with wherever the spec lands. That is a small cost next to what cross-platform reach is worth, but it is a cost, and pretending otherwise would be dishonest.

A standard that started as one company's packaging format for Claude Code now runs across more than 70 agent platforms, has crossed a million published skills, and just left that company's direct control for a vendor-neutral foundation. None of that changes a single line of code anyone has already written.

What it changes is where the next decision gets made, and that's the part worth tracking rather than shrugging off as housekeeping. I'd keep building skills exactly as I have been, and I'd put the security-guidance roadmap on my own list to revisit once it actually ships, because that's the item in this announcement with real teeth, not the press release framing around it.

## Top comments (0)
