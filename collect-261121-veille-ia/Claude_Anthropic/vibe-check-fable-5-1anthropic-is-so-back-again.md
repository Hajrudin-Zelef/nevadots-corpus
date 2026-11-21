---
id: collect-261121-veille-ia/claude-anthropic/vibe-check-fable-5-1anthropic-is-so-back-again
title: "Vibe Check: Fable 5.1—Anthropic Is So Back (Again)"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["fable", "anthropic", "about", "claude", "coding", "tokens", "code", "percent"]
source: https://stage.every.to/vibe-check/fable-5-1-vibe-check
source_anchor: ""
source_lines: [1, 217]
section: "Claude_Anthropic"
---
We called the first Fable a “warp drive” because it was the most powerful coding model we’d ever tested. But it was slow and hard to understand—it felt like a power tool only for the most AI-pilled of users.

Fable 5.1 is different.

In our testing it’s more powerful than Fable 5, running for days at a time to tackle coding challenges that stumped its predecessor. But it’s also fast and friendly, and, importantly, it speaks English like a normal person, er, AI. Unlike the original Fable and Anthropic’s recent Sonnet 5 and Opus 5 models, Anthropic has returned to form and created a model we actually enjoy talking to.

Oh, and there’s more: In our testing, it uses about half the tokens of Opus 5 for similar tasks. It should make Fable-level work available within an Opus budget. So it’s priced for everyone too.

Even more interesting, “everyone” now includes enterprises. Fable 5.1 comes with a zero-data retention option, removing a barrier that precluded most large companies from using it.

Fable 5.1 is a rare combination: the strongest coding model we’ve used and a collaborator we can recommend to people who don’t write code. It should be the first glimpse for most knowledge workers of truly delegated AI work—previously only the domain of programmers. You can give it a task like building a slide deck and move on to other things, confident that it’ll come back great in an hour or two.

We’ve been testing it for about a week, across coding, writing, and knowledge work tasks. Here’s our day-zero Vibe Check.

Anthropic says Fable 5.1 makes its strongest capabilities easier and cheaper to use. Its main claims:

More reliable on big assignments

Anthropic says the model can pursue a goal over extended runs and make better decisions about how to solve the underlying problem. Kieran’s experience supports that: Compared with Fable 5 on a task to build a clone of Every’s document editor Proof from scratch, it went deeper, added useful details he hadn’t requested, and showed better judgment about what worked.

Clearer communication

Anthropic promises less stock phrasing, shorter updates, and closer attention to writing instructions. We saw this too: Its writing had fewer AI tells, and the team found its explanations easier to follow. It still routinely exceeded requested word counts.

Strong results at lower effort

Anthropic says Low and Medium can deliver performance at least as good as Fable 5 at substantially lower cost. High is the default in Claude Code; Medium is the default in Claude.ai and Cowork. Kieran liked the coding results at all three settings, though our writing tests favored High and Extra-high.

Lower bills

Anthropic estimates costs about 25 percent below Fable 5 for typical API workloads and Claude Code extra usage, with savings approaching 45 percent for extensive autonomous work. Our tests offer evidence of token efficiency: In Every’s Slack assistant, it used less than half as many tokens as Opus 5 with comparable results. We haven’t measured the cost savings against Fable 5.

Better adherence to boundaries

Anthropic says it is less likely than Mythos 5 to disregard constraints, rationalize its decisions, or game an evaluation. We didn’t run those safety tests, but ordinary instruction-following still had gaps: At Extra-high, Kieran sometimes found it kept working when he interrupted to ask what it was doing.

Pricing

Standard API rates remain $10 per million input tokens and $50 per million output tokens, matching Fable 5. Reading previously cached input now costs $0.25 per million tokens, a 75 percent reduction. Anthropic estimates overall savings of about 25 percent for typical API workloads and Claude Code extra usage, rising to roughly 45 percent for extensive autonomous work.

Availability

At launch, Fable 5.1 will be available in Claude.ai, Claude Code, and Cowork, through Anthropic’s API, and through Amazon Web Services, Google Cloud, and Microsoft Azure.

What’s new

Fable performance without Fable 5 quirks

It handles the same work Fable did, from one-prompt app builds to coding runs that last all day, and it’s easier to work with while it does it. It answers faster, explains what it’s doing in plain language, and changes course when you tell it to instead of arguing.

Comes with a zero data retention (ZDR) policy

Fable 5.1 supports zero data retention agreements, so eligible business customers can use it without Anthropic storing their prompts and responses, removing a barrier for companies whose privacy rules couldn’t accommodate Fable 5’s mandatory 30-day retention.

Language you can understand—finally

Dan’s first message to the team about it: “I can actually understand what it’s saying.” The outputs I measured read at a lower grade level than Opus 5 or GPT-5.6 Sol, meaning the text is accessible to more people, with fewer AI tells.

Still does the absolute most

Asked for 1,000 words, it wrote 1,288. Asked for three to six themes, it gave eight. Asked for eight to 12 quotes, it pulled 43, and five of 27 quotes weren’t in the source. At the highest effort setting, it spun up subagents it didn’t need and kept going when Dan asked it to stop and explain.

The Reach Test

Dan Shipper

The multi-threaded CEO

🥇

“It’s a fantastic model. All of my coding tasks are in it, and it’s constantly working in the background while my day-to-day is still in Codex. It’s an extremely good coder and it actually speaks like a regular person. The thing that pushes me to gold is where Anthropic was before this release. They went from down so bad to so back. And the fact that it’s faster and uses half the number of tokens in our agent eval, that’s crazy.”

Kieran Klaassen

Father of compound engineering

🥇

“I used Fable 5 for long running tasks, but found it hard to use in a more iterative in-the-loop session. But this version of Fable 5.1 replaced faster models, and I can use it both in the loop and in long-running tasks, which is great because I don’t like to switch. Best of both worlds. Where the comments from users about Fable were, ‘This is a brilliant model that’s shitty to work with and not for most people,’ this model is Fable for everyone.”

Mike Taylor

Eval-builder extraordinaire

“It didn’t really fail on anything I tried, so I’ve been trying to one-up it every time. I just gave it an entire market analysis with eight stages, and it seems to be working. I keep my old threads in Codex because they feel Codex-shaped, but every new thread is on this. From a model perspective, this is definitely back, but UX also matters. This feels like all the intelligence I’ll ever need. I still think they’ll do better, and I’m saving the gold for that.”

Katie Parrott

AI-pilled writer by day, vibe coder by night

“I’ve been burned by two Claude models in a row, so I keep waiting for this one to mess up or get annoying, and it hasn’t. I don’t do a lot of long-haul, send-it-off tasks unless I’m LARPing as a software developer, but I’ve been using it for writing and liking its style. It’s so fast and so much less obstinate that the back-and-forth is a lot better than the last two. I’m starting to reach for it naturally. It’s been gradual, but my trust issues with Claude are starting to heal.”

Marcus Moretti

Agent-whisperer

“Basically as smart, a better writer, and about twice as token-efficient. Our agent prompts were tuned for Opus 5, so a better writing score under Opus-shaped prompts is impressive. The one caveat is that because it acts faster and uses way fewer tokens than Opus 5, it can do worse on heavy multi-tool tasks.”

Legend:

🥇Paradigm shift

Psyched about this release

It’s okay, but I wouldn’t use it every day

Trash release

Coding: Fable 5 performance without Fable 5 token use

For coding, Fable 5.1 does what Fable 5 did, and it’s easier to work with while it does it. Dan moved all of his coding tasks over from Codex. Mike sends all of his new projects to it.

Kieran asked it to rebuild Proof from a single prompt—the same test Fable 5 completed in June. Fable 5.1 also built a working version in one shot, but Kieran found it went deeper, adding useful details he hadn’t specified. He also saw more style and better judgment about what worked. As he put it, “What a time to be alive to get this in one shot.”

It also handled his LFG workflow, where the model plans, writes, reviews, and tests without a person in the loop. By the end of the week, he’d updated the compound engineering plugin with instructions tailored to it. “It’s the right balance of having an opinion and getting shit done versus pushing back,” he said.

Dan still uses ChatGPT far more for everyday work, but he’s delegating gigantic coding jobs to Fable 5.1 and letting them run. After he got access, Claude Code was making roughly five times as many requests to the model each day.

Mike asked it to move a blog off Webflow. It did it in one prompt. He then asked it to build a version of Stanford’s AI-town simulation, which follows AI characters through their daily routines to see how they form relationships and spread information. In one shot, it created 25 characters with memories and daily routines, who chatted and spread news about a party and a mayoral race happening in the town. “At this point, I’m not even sure what the model can’t do,” Mike said.

Dan ran security-related coding work that Fable had refused in June, when Fable’s safeguards sent those requests to Opus 4.8. Dan noted that refusals on the basis of security risks are “way down”—the model trusts the human’s judgment more on what does or does not constitute a security risk. Kieran also hit fewer false positives during normal engineering work, whereas Fable 5 sometimes mistook routine coding tasks for security work and blocked them. The one refusal Kieran hit: It wouldn’t log in with a test password during an automated testing loop, which he called “a bit extra.”

As for effort levels, Kieran ran the same kinds of tasks at every effort setting and found that Low, Medium, and High were all “super good.” Extra-high “rips” on long runs but hands off too much work to subagents. He’d rather not pick at all: “I just wish they don’t have effort levels and it will do this automatically.” Most of the team settled on High for work with which they stay in the loop.

Writing: Words you can understand, facts you have to check

This is the first Claude model in a year that the writers on our team want to draft with. It writes clear prose in the right order, and it takes an edit without arguing. Still, it needs a fact-checker on long jobs, it hasn’t displaced Opus 5 in our automated editing pipeline, and whether its style beats GPT-5.6 Sol’s is a matter of taste.

On Every’s writing bench, which tasks models with six editorial tasks, the new model’s best work was the long-form ones, such as writing an introduction from scratch or filling in a missing section of an essay. On the missing passage assignment, which asks the model to define AI slop from Dan’s essay “After Automation,” it outscored every model we’ve run, Fable 5 included. Its passages used more of the particulars in the surrounding essay, built the argument so that each paragraph made one point, and stayed faithful to the source material where Fable 5 and Opus 5 tended to infer. Fable 5.1 outputs also carried fewer AI tells than any other model’s, and they read at a seventh-grade level, a grade below Fable 5 and two below Sol.

The X post is the one job where it trailed, landing near the bottom, above only Sonnet 5 while Sol led every Anthropic model. The bench has always split along this line: Anthropic models like to extend an argument, GPT models want to compress it. The new model is the strongest extender we’ve tested and a below-average compressor. Its X posts stack a second and third idea where the brief asked for one.

When you kick the effort level up, the model exercises better judgment about what to leave out. High and xHigh scored best. The gap between medium and high was widest on the paragraph-filling assignment, where the model has to read the surrounding argument and decide what belongs in the gap to make the argument complete, and higher settings wrote shorter: The same introduction ran 805 words at Medium and 657 at xHigh. As with previous models, I settled on High as the best option for drafting.

The same instinct that makes it a good drafter makes it a risky reporter. Given a tight brief and set of sources, it’s faithful. But if you give it long source documents and room to elaborate, it can get creative in the wrong ways: Mike asked it to produce a Vibe Check using eight to 12 exact quotes from the Every team. It produced 43 quotes, including several that were missing from the supplied text.

As an editor it’s fast and timid. Jannik had it edit 20 past Every drafts the way Kate Lee, Every’s editor in chief, had edited them. It found Kate’s exact edits about as often as Opus 5 in less than half the time, but threw away about half of what it had found; Opus 5 kept most of its own and Sol nearly all. It can see the edit; it just doesn’t trust itself enough to show you, which is the reverse of its drafting problem. Jannik’s call is that Opus 5 stays the model for that pipeline; Dan’s is that the Fable 5.1 is a little worse and a lot faster.

Knowledge work: Still doing the absolute most

Give the new model a big consulting assignment and it finishes the whole thing, and on judgment-heavy tasks like slide decks and persona interviews, it beats GPT-5.6 Sol. Give it a brief with a word count, a theme count, or a quote count, and it goes over.

Mike’s EC (executive consulting) Bench covers 12 consulting tasks: dashboards, a training-needs synthesis, a workshop run sheet, a branded slide deck, a roleplayed interview, and others. Sol scored 100 on six of the structured building tasks, then 44 on the blog post and 33 on the roleplayed interview. The new model did the reverse. It scored 88 on persona answers, 93 on the slide deck, and 53 on the roleplay, which Mike still called “basically a fail” but which beat Sol. It lost points where it ignored explicit limits.

Mike asked the model to design three hands-on projects for an executive learning to build with AI, complete with prompts and realistic practice data. One was a dashboard tracking four company acquisitions. The model supplied spreadsheets, status emails, and Slack conversations, including warnings about a project delay that hadn’t yet reached the tracker. In just under 23 minutes, it produced a training pack Mike called “close to perfect.”

Limits are where it loses points. A hotel recommendation with a 1,000-word cap came back at 1,288 words. A training-needs synthesis that called for three to six themes came back with eight. A brief that allowed eight to 12 supporting quotes got 43. A request for markdown got HTML. Opus 5, on the same tasks, stayed inside every limit and timed out twice.

Design is the other weak spot. Mike asked it to build an app that lets people fill out forms by talking to an AI interviewer. The interview worked, but the app had a generic purple-on-black design, large stretches of empty space, and a stray “false” displayed beneath the controls. Fable produced the same purple screen in June. Opus 5 didn’t.

Agent behavior: Half the tokens per step

In automated pipelines like Slack assistants designed to execute longform tasks that require tool use, Fable 5.1 does the same work as Opus 5 on about half the tokens and in about 60 percent of the time. Left alone at the highest effort setting, it keeps working, keeps spawning subagents, and doesn’t always stop when you ask. The moral of the story: Set a budget before you start.

Marcus tested both models inside the Every Agent, our Slack-based AI assistant, using internal checks of its writing and ability to complete tasks with tools. Both ran at Medium effort, with instructions originally tuned for Opus 5. Marcus judged the models’ outputs to be comparable, but Fable 5.1 used less than half as many tokens and responded faster. In Jannik’s editing test, it finished in less than half the time of Opus 5.

Marcus’s caveat: Because it acts fast and spends few tokens, it “can perform worse on heavy multi-tool tasks.” If your pipeline needs a model to chain eight tool calls carefully, Opus 5 is still the safer pick.

Kieran ran long tasks at xHigh and found it “extremely thorough, maybe too much, especially delegating to subagents,” and said that “sometimes [it] does ignore me too.” His xHigh sessions “go for like a day at a time.” When he interrupted one to ask, “Can you explain what you’re doing?” he said it “just ignored me and continued to use a billion subagents.” Mike’s exercise pack ran more than twice his normal time. Kieran put 1.8 billion tokens through it in a single day. It is cheap per step and will take as many steps as you let it, and the effort setting is what decides how many that is.

A faster, friendlier Fable for all

Fable 5.1 is Fable with less waiting and better manners. It’s faster, writes more clearly, uses fewer tokens per step, argues less, and handles long jobs as well as Fable 5 did. Mike moved his new work to it. Dan moved his coding to it. Katie moved her writing to it, two months after deciding Fable wasn’t for her.

Dan and Kieran give it a gold star for the turnaround and the token efficiency. Mike is holding their gold for a price that lets more people afford Fable-grade work, and Anthropic hadn’t shared a price when we filed this piece. Kieran’s caveat on the comeback story: “Fable never sucked.” Anthropic’s bad months were about Fable being unavailable and two other models missing the mark, while GPT-5.6 came out.

Reach for it if…

You work with the model in the loop. Writing, editing alongside it, and coding where you read as it goes. High effort is where most of the team landed.

You want Fable-grade results on long jobs without Fable’s wait. Delegated builds, big refactors, and whole-project rebuilds finished for Kieran and Mike at the same level as Fable, faster.

You have Opus 5 agent prompts and a token bill. Marcus got the same pass rate on task completion in the Every Agent at Medium on half the tokens, with a better writing score, without rewriting a prompt.

Keep Opus 5 or Sol nearby if…

The brief has hard limits. Word counts, theme counts, quote counts, and output formats all got exceeded in our testing. Opus 5 stayed inside every one.

Quotes have to be right the first time. Five of 27 checkable quotes on one task weren’t in the source. Verify every quotation and every added detail before it leaves your hands.

The task is heavy multi-tool work or a tuned editing pipeline. Marcus and Jannik both found Opus 5 steadier there.

Nobody has set a budget. At xHigh it will take the whole day if you let it.

Whether this is a 4.5 moment—the kind of release that helps a whole new group of people realize what AI makes possible—or just a very good Fable depends on packaging and pricing decisions Anthropic hadn’t shared as of this writing. Either way, the team is using Claude again.

Disclosure: Anthropic provided Every with pre-launch access to Fable 5.1. The company had no input on this review.

Frequently asked questions

What is Claude Fable 5.1?

Fable 5.1 is Anthropic's newest AI model, released September 2026. It matches the original Fable's coding power but is faster, uses about half the tokens, and communicates in plain language—“Fable for everyone.” At launch it's available in Claude.ai, Claude Code, and Cowork, through Anthropic's API, and on AWS, Google Cloud, and Microsoft Azure.

Is Fable 5.1 better than Fable 5?

In Every's testing, yes. It handles the same work—one-prompt app builds, all-day coding runs—while answering faster, explaining itself clearly, and changing course without arguing. It also supports zero data retention agreements, unlike Fable 5, which requires 30-day retention.

How much does Fable 5.1 cost?

API pricing is unchanged from Fable 5: $10 per million input tokens and $50 per million output tokens. Cached input drops 75 percent to $0.25 per million tokens. Anthropic estimates savings of about 25 percent for typical workloads, approaching 45 percent for extensive autonomous work.

Is Fable 5.1 good for coding?

It's the strongest coding model Every has tested. It rebuilt a document editor from a single prompt, ran multi-stage projects for days, and refused far fewer legitimate security-related tasks than Fable 5. Dan Shipper moved all his coding tasks to it from Codex.

Is Fable 5.1 good for writing?

It's the first Claude model in a year Every's writers want to draft with. Its prose carries fewer AI tells than any other model tested and reads at a seventh-grade level. It still needs a fact-checker on long jobs—asked for 8–12 quotes, it produced 43, several fabricated. It's a below-average compressor for short formats like X posts.

How does Fable 5.1 compare to Opus 5 and GPT-5.6 Sol?

In agentic pipelines Fable 5.1 does the same work as Opus 5 on about half the tokens in 60 percent of the time. It beats GPT-5.6 Sol on judgment-heavy tasks like slide decks and persona interviews. Sol wins on structured tasks and compression.

What are Fable 5.1's weaknesses?

It ignores explicit limits—word caps, theme counts, quote counts—and overshoots them. At the highest effort setting it spawns unneeded subagents and doesn't always stop when asked. Its app design instincts remain generic.
