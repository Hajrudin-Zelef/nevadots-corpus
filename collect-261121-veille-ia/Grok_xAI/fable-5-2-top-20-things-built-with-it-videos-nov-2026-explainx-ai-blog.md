---
id: collect-261121-veille-ia/grok-xai/fable-5-2-top-20-things-built-with-it-videos-nov-2026-explainx-ai-blog
title: "Fable 5.2: Top 20 Things Built With It (Videos, Nov 2026) | explainx.ai Blog"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: ["2026-11-09"]
keywords: ["fable", "anthropic", "built", "verified", "says", "hours", "post", "hypervisor"]
source: https://explainx.ai/blog/top-20-things-built-with-claude-fable-5-2-matt-shumer-anthropic-mollick-2026
source_anchor: ""
source_lines: [1, 154]
section: "Grok_xAI"
---
Claude Fable 5.2 shipped on November 9, 2026, and the build showcase started within hours. Matt Shumer, an AI investor and early tester, posted a thread titled "I spent over 400B tokens testing Fable 5.2" with six demos. Anthropic's own launch post added an operating system, a hypervisor and a theorem proof. Ethan Mollick published a stack of live sites.

This post ranks the 20 best Fable 5.2 builds found so far. Each one is labeled: **verified** (primary source or partner statement), **live** (you can open it), or **claim** (the creator says it, we could not confirm). Pricing and migration details live in our Fable 5.2 launch guide. For Mollick's broader **Jagged AGI** framework on judgment gaps, see our expert-judgment companion.

## TL;DR: what people built and how much to trust it

| Question | Answer | 
|---|---|
| What is the biggest build? | An operating system from an empty folder, 1,000+ subagents, eight days (Anthropic) | 
| What is the most playable? | Shumer's voxel engine, Mirror Earth and Hogwarts Royale (creator claims) | 
| What can I open today? | Mollick's Portals, Auden course and research sites, linked below | 
| Is "one prompt" literal? | Only for the starting message. Many runs used many agents for hours or days | 
| What does it cost to repeat? | $8 input and $40 output per million tokens on the API | 
| Is it better than Opus 5.5? | Anthropic's own tests say yes on long builds; see Fable 5.2 vs Opus 5.5 | 

## What does "built with Fable 5.2" actually mean?

Read these demos with one rule. The headline says one prompt, the footnote says many agents. Anthropic's own examples use a coordinator model that spawns hundreds or thousands of subagents that write, test and review code. Shumer's 400B-token figure also tells you the compute behind the showcase was large.

That does not make the results fake. It means the useful skill is not typing a clever prompt. It is writing a start message with a clear definition of done, then letting a loop of agents run against it. Our loop engineering guide covers that pattern in detail.

## The top 20 builds

### 1. An operating system, from an empty folder (verified by Anthropic)

One Anthropic engineer started Fable 5.2 from an empty folder. The model coordinated more than 1,000 subagents for eight days, around the clock, writing, testing and reviewing code. Anthropic says the result boots on a laptop and includes its own core, disk, keyboard and network drivers, persistent file storage, windows, a login screen, an installer and Claude built in.

This is the largest autonomous build published for any model. It is also an internal experiment. No outside team has booted the image.

### 2. A hypervisor with 3,500+ machine-checked proofs (verified by partner)

iGent, whose co-founder and CEO Sean Ward is quoted in Anthropic's post, reports that Fable 5.2 built a hypervisor that isolates virtual machines in one session. Over 3,500 machine-checked proofs cover its core. Proofs checked by a machine are the part to trust, because a proof checker does not care how confident the model sounded.

Fable 5.2 builds a verified hypervisor with machine-checked proofs sealing each isolation boundary

### 3. Fermat's Last Theorem formalized in 30 hours (verified by Anthropic)

About 1,000 Fable 5.2 agents fully formalized Fermat's Last Theorem in Lean in 30 hours with minimal human input. Anthropic says the proof is seven times shorter than the first formalization, which took 11 days. Opus 5.5 with the same setup completed 26% in 18 hours and barely moved after 30. For a smaller Lean example, see our Opus 5.5 CPython formalization post.

### 4. Three working electronic devices (verified by Anthropic and Diode)

From a one-paragraph prompt, Claude designed a home voice-assistant hub, a USB speakerphone and a handheld virtual pet. It wrote specifications, chose parts from US distributors, routed four-layer boards, modeled 3D-printed enclosures and wrote the software. Anthropic says each board was ready to manufacture in under a day and all three worked without electrical modifications. Humans at Diode fabricated and assembled them.

Watch the official video of the voice-assistant hub below. Full numbers are in our Fable 5.2 PCB design breakdown.

### 5. A voxel destruction engine that runs on a MacBook (claim)

Shumer says Fable 5.2 built this engine from a single prompt and calls the quality "insane" for something that runs on a MacBook. The demo is on Spawn at spawn.co under his handle. The page loads, but it carries no description or model credit, and we found no independent testing. Treat the performance claim as unverified until others run it.

Fable 5.2 voxel engine concept showing a block world being built and destroyed one block at a time

### 6. A video made entirely by the model (live)

Shumer's post says Fable 5.2 one-shotted a complete video and "makes Opus look like a toy." The video is the Part II episode from the Something Big Is Happening channel. Judge it yourself:

The Opus comparison is his opinion. For measured comparisons use our model routing guide.

### 7. Mirror Earth, a playable destructible planet (claim)

Shumer calls it a mirror Earth as a game: go anywhere, destroy anything, with weather and more matching the real Earth in real time. One source, no independent hands-on report yet. The claim that weather matches in real time depends on a live data feed, so ask whether it is real data or a simulation before you repeat it.

Fable 5.2 Mirror Earth game concept with a toy car driving a road that wraps around a globe

### 8. Hogwarts Royale, a Harry Potter battle royale (claim, IP risk)

The prompt was "create a mashup of Harry Potter and Fortnite." The Spawn page describes it as a battle royale at Hogwarts: drop in, find a wand, outlast the ring. It is credited as made in conversation with an assistant called Savi. It uses a third-party franchise, so expect it to be taken down or renamed if the rights holder objects. Do not copy this approach for anything you plan to sell.

### 9. A robot you can actually build in an apartment (claim)

Shumer asked for a robot design he could build himself and asked followers what they think. No spec sheet was published in the thread. This is the weakest of his six items, but the idea is the useful one: ask for a bill of materials and a build you can afford, not a sci-fi render.

### 10. Robots that solve a puzzle cube (verified by Anthropic)

Anthropic's launch post says Fable 5.2 controlled robots that solved a puzzle cube, unscrewed a bottle cap and drew a picture of a robot. On the RoboDojo Sim benchmark at max effort, Fable 5.2 scored 64.9% against 39.4% for Opus 5.5 and 27.6% for Fable 5.1.

Fable 5.2 robot control demo with a humanoid silhouette mid-step

### 11. Portals: 450+ procedural 3D worlds (live)

Ethan Mollick built a hands-off tour through procedurally drawn 3D worlds with no pre-made models or image files. Teams of agents grew it to more than 450 worlds, guided by a rule book that reached 204 sections and about 87,500 words. He had to bring in a second team of agents to trim the rule book. Open it at portals-endless-worlds.netlify.app or read the code on GitHub.

### 12. A keyboard turned into a game console (verified by Mollick)

Mollick asked Fable to turn a Codex Micro USB keyboard into a game console. It worked out how to set key colors, read the knob and joystick, and wrote six games, including Simon and a safecracking game played with the knob. This is a clean example of hardware discovery: the model read the code of two companion apps to learn the device.

### 13. A full recreation of Auden's 1941 course (live)

"Fate and the Individual" recreates W. H. Auden's 1941 University of Michigan course with a page for each of 35 major works, plus blackboard notes, records and assignments. See the live site and the code. It is a strong template for course-building agents.

### 14. A 182-page research paper on LLC fees (live, with caveats)

"The One-Dollar Company" studies how LLC filing fee changes affected business registrations in Colorado and the UK. It runs 182 pages. Mollick states plainly that it was not actually preregistered, which is the right caveat: a model-written paper is a draft for an expert to check, not a result. The paper is at the-one-dollar-company.netlify.app.

### 15. A study of 1,186 stories asking if plots share a shape (live)

Fable analyzed 1,186 stories from around the world to test whether plots share a universal shape. The result is published at is-there-a-universal-story.netlify.app. Large-corpus analysis with a public method page is where these models pay off fastest.

### 16. Prebid Brief, a business idea screened and prototyped (live)

Fable screened 22 business ideas and proposed a service that tracks park and water projects before they go to bid, reading public agendas from 201 city and county governments. Mollick stopped before launch and put the odds of success at 20% to 40%. See the demo. The honest odds are the most useful part.

### 17. A Python-to-Rust migration of a production service (verified by Anthropic)

A team of agents running an earlier Fable 5.2 version migrated a critical Anthropic production service from Python to Rust. Anthropic says it is more efficient and reliable, and starts about 26 times faster. This is the build most likely to match your own backlog.

### 18. A 10 to 12 hour security audit with about 170 reviewer agents (verified by partner)

Uber used Fable 5.2 for a security audit that ran 10 to 12 hours with about 170 reviewer agents. The output was a write-up, fix pull requests and a policy proposal. If you run agents near sensitive code, read our note on stopping agents before they take dangerous actions at AgentBeam.

### 19. Quantum ESPRESSO's phonon code made about 36x faster (verified by partner)

Discovered Materials reports that Fable 5.2 made the phonon code in Quantum ESPRESSO about 36 times faster on average, cutting 13 benchmark cases from 3.3 hours to under four minutes. Performance tuning on old scientific code is an underrated use.

### 20. Better 3D scenes from scratch in Unity (verified by partner)

Unity reports that Fable 5.2 builds higher-quality 3D scenes from scratch and can rebuild an experience from a visual reference. It is the most direct link between the game demos above and a shipping engine.

## What people are asking about these builds

### Is this really one prompt?

For the starting message, often yes. For the work, no. The operating system run used 1,000+ subagents for eight days. Independent roundups also note that several builds ran many hours. Your first prompt should read like a spec, not a wish.

### Why do the game demos matter if they are claims?

They show where the capability sits: long, self-checking builds with a visible, testable result. A game either runs or it does not. That makes games a good proxy for end-to-end agent work, even when a single demo is unverified.

### What should I try first?

Start with something that has a clear test. A migration with an existing test suite, a data analysis with a public method page, or a small game with a win condition. Those match the verified builds above.

## How to repeat these builds

1. **Write a one-page spec.** State the goal, the finish line, and the checks that prove it.
2. **Add a verifier.** Machine-checked proofs, tests or a benchmark. Every verified build above had one.
3. **Split the work.** Use a coordinator and reviewer agents, the pattern in what are agent skills.
4. **Cap the spend.** At $8 and $40 per million tokens, a 400B-token test campaign is a large bill, so set budgets before you start.
5. **Keep a human on the claims.** Mollick's paper, Prebid odds and Shumer's Earth game all need an expert to confirm before you repeat a headline.

For ready-made tooling, see the Claude Code command reference and our Karpathy HackerTube build, which shows the pattern one model generation earlier.

## What is still unconfirmed

- Shumer's voxel engine, Mirror Earth, Hogwarts Royale and robot are his statements. We found no independent reproduction.
- Anthropic's operating system, hypervisor and Fermat runs are company statements with partner quotes. The system card and METR review of Fable 5.2 are the place to check method.
- We could not find an official video of the operating system build. Anthropic's post refers to videos without linking them.

## Related reading

Sources: Matt Shumer's thread on X, Anthropic's Fable 5.2 and Mythos 5.2 announcement, Ethan Mollick, Fable 5.2 and Jagged AGI.

*Details are accurate as of November 10, 2026. Demos and claims change fast; re-check the linked sources before you rely on them.*

**Update — November 11, 2026:** One more build to add to the list: Age of Dragon, a non-coder's browser open-world RPG.
