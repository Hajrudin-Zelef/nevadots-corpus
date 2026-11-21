---
id: collect-261121-veille-ia/gemini-google/gemini-4-argon-early-access-review
title: "Gemini 4: Argon - Early Access Review"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["game", "argon", "like", "games", "early", "access", "some", "level"]
source: https://jgunnink.substack.com/p/gemini-4-argon-early-access-review?utm_source=tldrdev
source_anchor: ""
source_lines: [1, 88]
section: "Gemini_Google"
---
## Introduction

As part of the Google Develop Experts program, occasionally we are invited to test new Google products early to help the teams iterate on products and improve them before they go out to everyone. A couple of weeks ago, I was given early access to Gemini’s new frontier model, Argon, aka Gemini 4. I decided to give it three separate tasks to see how it would fare across different domains of coding work, since I was only given access via Antigravity.

1. Build an Android/Mobile game from scratch
2. Iterate on a complex user interface for RocketFlag and simplify it
3. Perform a security and performance review of a highly distributed backend system.

The TL;DR is that I think Argon performed mostly pretty well. I'd put it at around Opus 5.5. I mostly used "high" reasoning as I wanted to test Argon's best work. In my view, it excels at upfront architectural planning and rapid multi-file code generation that passes static checks, but it consistently exhibits visual blind spots and concurrency edge cases without live execution feedback.

**Most importantly:** integrating closed-loop verification (such as automated visual harnesses, runtime logs, or structured self-review) enables it to diagnose and resolve these flaws in a single pass.

## Building An Android Game

I should probably preface this section by saying I know *nothing* about building mobile phone apps, games or anything around either of them, like game designs, levels, user interfaces controller systems etc. Nothing. What I do know is that I like some games, and I very much dislike **most** mobile games you find on the app stores these days. They might advertise some tiny part of the game that’s fun, but the rest of the game is completely different, time-based bullshit that makes you pay to “go faster” so that you can get back to the actual fun part of the game. But, who am I to judge, these games rake in millions, so there’s a business model there. Just not for me.

Any who, growing up as a kid, I played a lot of video games, not insane amounts like you hear about on the internet, but a fair amount. My go-tos were any of the Zelda series, mario kart, diablo and age of empires. With the recent release of StarFox (and the upcoming Ocarina of time), I decided to try my hand with argon to build out a game which would be mobile friendly with some level progression and increasing difficulty. I spent probably 3 or 4 hours with the model asking questions and it clarifying with me the aspects of the game. The whole thing game me an appreciation for the complexity of games and reminded me how amazing software can be very complex under the hood, but extremely simple to the end user. Things like sounds and haptics (feedback) when the user does something, like collect a power up really stand out when they’re absent.

So what are we building? A rail based shooter - inspired by StarFox. I figured the base premise of piloting a plane along a rail (the level) and getting to the end shooting down enemy ships would be a simple enough concept that it would work on a phone without too much prodding and poking and time spent since I didn’t have months or years to build out a storyline, character development and so forth.

Based on all the prompts, questions and designs, this is the first cut of the app I made:

Then with the basic app running (although some bugs, which once pointed out were fixed in one shot) I moved on to some level designs. A dusty canyon, a space theme near a bunch of ships, inside a planet mine, an asteroid belt, and inside a gigantic space ship, sort of like the death-star.

First “application” of the dusty canyon level.

After a few more refinements, we have the opening level.

I also made sure it worked on a variety of screen sizes and shapes, so this is what it looks like on a fold (and the other levels)

The game is actually a lot of fun. There’s barrel rolls, bombs, gates to shoot open, the bosses are challenging and you can even do the charge shot with lock-on!

I didn’t really know what to expect, but I found the experience of building with Argon pretty good here. It was able to connect to my phone and pull in debug logs and crashes, fix them automatically and tell me what went wrong. Overall it’s quite capable in a number of scenarios I was playing with it in, and the output is pretty good.

## Iterating on a complex user interface, and simplifying it.

If you’ve been following me for any length of time, you’re probably aware that I run a feature flagging application SaaS, RocketFlag. I’ve been building this for more than two years now, and have paying customers so it’s been a fun ride so far. I love working on it, it’s my passion project. But after two years, the interface hadn’t really changed much. It had become cluttered with more “stuff” I had added over time and so I took the opportunity with Argon to work with it’s vast knowledge as a frontier model to build out an updated refreshed design attempting to tidy everything up, make it more accessible and readable and removing shall we say, questionable design choices.

The initial back and forth I had with Argon was good, I felt like we were getting somewhere with the design conversation, but the first output was very similar to what most other commentators had been saying on the various channels. Argon produced a very noisy cluttered user interface with so much going on. I had to shave so much off from the initial design into what made it into the final product. What was good about it though, was that Argon took one or two pages from the app, and rapidly prototyped several html-only pages which allowed me to get a feel for the design and look of what would become the actual implementation. This rapid prototyping phase was *excellent* and enabled fast feedback loops and cycles on the model were not wasted doing the full build only to have to change it again later.

Other areas which were successful:

- **Architectural UX Diagnosis:** Auditing the frontend repository immediately identified the core structural problem: ~25% of the app used a modern layout, while ~75% suffered from 3-column table alignment drift across multi-environment views, a 2.3-second artificial blocking animation on every flag toggle, and a disjointed 3-modal editing workflow.
- **Automated Playwright Visual Verification Loop:** Once the local application stack was running and connected with a headless Playwright script, 14 targeted UI and layout refinements all joined up in single passes.
- **High E2E Test Survival Rate:** Preserving DOM structure and test identifiers kept 91 of 98 Playwright E2E scenarios green on the first CI run, with the remaining 7 failures diagnosed via cloud build logs and fixed in one commit.

The lesson here: have lots of automated tests for your application folks! It allows you to do enormous sweeping changes like this in one squashed-commit out to prod.

But let’s talk about the failures. Because this took a while, and I think it’s worth pointing out the failures of Argon, where I spent a lot of time having to steer it.

- **Premature Blocking Question Tool:** The model blocked the user with a multiple-choice prompt before providing the necessary written architectural analysis to make an informed choice.
- **Generic “Vibe-Coded” Initial Aesthetic:** As mentioned, the initial design defaulted to cliché SaaS styling and required six iterative cycles of strict creative direction to achieve a refined industrial look. Too much noisy cluttered interface, trying to be helpful.
- **The Gap Between Passing Tests and Browser Fidelity:** Despite 895 unit tests passing on the first run, the live UI had major visual defects, including restyling an old modal instead of building the specified drawer and misconfiguring Chakra UI border and switch styling.
- **Dark-Mode Legibility & WCAG Contrast Blind Spot:** Initial dark-theme choices created failing 2.6:1 text contrast and unreadable 10px fonts, requiring an accessibility overhaul after third-party feedback to meet WCAG standards.
- **Silent Feature, Accessibility & RBAC Regressions:** The refactor silently removed navigation and edit controls, broke accessible menu names with unhidden badges, and exposed privileged edit controls to read-only viewers.

So there’s a few things going on there, but overall these failures were relatively small, and all addressed quickly. I think Argon performed mostly well here, but did require a bit of supervision.

If you’re interested in reading more on the design overhaul about RocketFlag you can read it on the RocketFlag blog here: https://rocketflag.app/blog/2026/carbon-graphite-and-brushed-silver/

## Perform a security and performance review of a highly distributed backend system.

The objective of this task was to conduct a comprehensive security, correctness and resilience audit/review of a multi-tenanted SaaS platform with a Go backend, NoSQL db, pubsub messaging cache invalidation and payment integration. Then once compeleted, organise the findings into an prioritised list with proposed suggestions and improvements that can be made.

This ended up in nearly 1,300 agent steps which resulted in 32 ranked findings across 5 waves of production-hardening. On the first pass, we had quite a lot of success.

- **Parallel Domain-Isolated Discovery:** Dividing the codebase audit across three specialized subagents and checking past post-mortems identified 32 valid issues without blowing the context window or rediscovering fixed bugs.
- **Clean First-Pass Backend Remediations:** Core backend vulnerabilities and reliability gaps, including cache key collisions, cross-project IDOR, missing HTTP limits, Stripe webhook race conditions, and invite seat-cap races, were fixed cleanly on the first attempt.
- **Cross-PR Regression Detection:** A targeted re-audit right after merging the UI overhaul confirmed earlier fixes while catching five new regressions, including an RBAC leak, a missing production toggle confirmation, and a broken clipboard hook.

However, just like the other projects, there were a few times I had to steer the conversation, direction and what I wanted from the output.

- **Subtle Distributed-Systems & State Bugs in First-Pass Code:** Something I like to do is get the agent to review it’s own work. You’d be amazed and how much it finds. In this case, I mandated an adversarial self-review before opening any PRs which caught a couple of critical production-breaking flaws that compiled and passed tests, including an infinite Pub/Sub reconnect loop, cross-node cache staleness, invite privilege escalation, and loose route matching.
- **Mocked Unit Tests vs. Out-of-Band Database Writes:** This one came out of the review too. In-memory cache warming passed mocked unit tests but failed in real CI integration tests because external Firestore updates bypassed invalidation events, overwriting background dormancy states until an authoritative database read was enforced.

The defining engineering pattern of this conversation was connecting rapid multi-file implementation with an explicit, adversarial self-review pass. So: Commit → Hostile Self-Review → Hardening Commit, and then real integration testing. Every single time I stopped to review/attack the diff, or ran against real Firestore instead of mocked repos, we caught subtle distributed-systems, state-machine, or fail-open bugs before they could reach production. 😅

## Conclusion

Overall, it’s good to see that Google is attempting to catch up with a new frontier model. There was a long time between drinks (in the AI land anyway) between Gemini 3.1 Pro and now it’s next “pro”(?) model Gemini 4 Argon. I think that if you’re in the Google eco system, then it’s good to see it. Google is pitching Argon at equal to or better than Opus for most tasks, although not everything and I think that this is sort of accurate, sometimes. I don’t really like benchmarks, which is why blog articles and reviews from others are so important.

But lately it’s not so much about the model’s capability, but rather how you instruct it. I’ve noticed harnesses from a lot of labs are taking the capability of even fast models, like flash or sonnet to new levels. I think the reasoning capabilities of Argon are about on par with other frontier models, but the problem is that the out of the box experience many developers have with it are such that the harnesses like Antigravity or AI Studio or even the chat app, don’t appear to be maximising the thinking and reasoning as much as they should and what tends to happen is that Gemini (I say gemini because this applies to all harnesses built around it by Google) jumps to conclusions too soon and starts building. You really need to slow it down, create plans and so on before starting on meaningful work.

Argon is a very much needed step forward, but there’s still plenty of work to be done.

Thanks for reading! This was a long one, but if you got all the way to the end, I’d love to hear your thoughts!

JK
