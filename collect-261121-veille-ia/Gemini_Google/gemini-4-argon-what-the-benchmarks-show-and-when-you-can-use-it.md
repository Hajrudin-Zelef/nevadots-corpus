---
id: collect-261121-veille-ia/gemini-google/gemini-4-argon-what-the-benchmarks-show-and-when-you-can-use-it
title: "Gemini 4 Argon: what the benchmarks show and when you can use it"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["argon", "google", "gemini", "tokens", "price", "table", "output", "million"]
source: https://dev.to/axrisi/gemini-4-argon-what-the-benchmarks-show-and-when-you-can-use-it-cpb
source_anchor: ""
source_lines: [1, 116]
section: "Gemini_Google"
---
Google announced Gemini 4 Argon on September 30, and the launch post calls it "our next era of frontier intelligence". It comes with a price, a 19-row benchmark table and a million-token output limit. It does not come with access: right now Gemini 4 Argon goes only to trusted cyber defenders in Google's Fairwind Program, and there is no date for anyone else. I read the table, the one independent leaderboard that has tested it, and the access rules, so here is what a developer can actually take away from Gemini 4 today.


- 
**Price at launch:** $2 per million input tokens and $10 per million output, cached input 95 % off. That is the same list price as OpenAI's GPT-6.1 Sol, which you can buy today.
- 
**Google's own table:** Argon is best on 13 of 19 rows against GPT-6 Astra, Claude Fable 5.1 and Claude Opus 5.5, ties one and loses five. It comes last of four on FrontierSWE v2 and Terminal-bench 4.0.
- 
**The outside number:** Artificial Analysis scores Argon (high) 53 on its Intelligence Index, tied with Astra and Fable. Claude Opus 5.5 leads at 58. Argon costs about a third as much per task.
- 
**Access:** Fairwind partners only, restricted to their security teams. Then "paid API customers and Google AI Ultra subscribers", with no date.

Argon is Google DeepMind's new frontier model, announced by Koray Kavukcuoglu, Google DeepMind's SVP and Chief AI Architect. The post lists three headline changes.

**Output length.** Google says one response can now run to an "industry-leading 1M tokens, up from the previous 64K tokens." Sixty-four thousand tokens was already a long report; a million is several novels in one reply. For developers the practical meaning is whole-file and whole-module rewrites in a single call, without stitching chunks together.

**Price.** "Argon will launch at an introductory price of $2 per million input tokens and $10 per million output tokens, with cached input tokens priced at 95% off input token price." The word "introductory" matters: Google gives no date for when the introductory period ends. The number itself lands exactly on GPT-6.1 Sol, which OpenAI shipped at DevDay the day before.

**Internal use.** Google says "thousands of Googlers" already use it. The most concrete example is code migration: "Argon agents are working on migrating C/C++ codebases to Rust across Google — scaling from tens of thousands of lines in core libraries like re2, libgav1 up to 800K+ lines for the Fuchsia Zircon kernel." For libgav1, the AV1 decoder, Google says Argon "replaced 32K lines of SIMD code" and produced "a memory-safe video decoder that runs 2.7x faster than the Rust port, with identical video output." The post adds that the ported code goes through "rigorous automated and manual auditing, emulation testing, and review before rolling out to production."

That Rust line got the best reaction on Hacker News, where the thread passed 1,400 points. One commenter, tazjin, remembered "back in the days when the cppnext team was refusing to even consider Rust, instead looking at absurd stuff like Carbon and Swift (!)". The migration people argued about for years is now a job Google hands to a model.

The post has one big comparison table: 19 rows, four columns. Argon is highlighted as best on 14 of them, and one of those is a tie, so the honest count is 13 wins, 1 tie and 5 losses. A Hacker News comment called it "only one real benchmark for comparison"; the table is bigger than that, but every number in it comes from Google.

The rows worth reading, as Google printed them:

| Benchmark | Gemini 4 Argon | GPT-6 Astra | Claude Fable 5.1 | Claude Opus 5.5 | 
|---|---|---|---|---|
| DeepSWE v1.1 | **77.9** | 74.1 | 67.4 | 74.2 | 
| Harvey's Legal Agent | **19.6** | 5.4 | 6.7 | 3.8 | 
| Vals Index | **68.9** | 63.1 | 65.8 | 67.0 | 
| FrontierSWE v2 | 55.0 | **65.5** | 56.3 | 62.3 | 
| Terminal-bench 4.0 | 57.4 | 58.2 | 57.9 | **66.4** | 

DeepSWE is the headline: a long-horizon coding benchmark where Argon leads by about four points. The legal row is the strangest win. Argon scores about 20 % where the others stay in single digits, which makes it the best grade on a test that every model fails.

The two rows the post's text never mentions are the ones I care about most. On FrontierSWE v2 and Terminal-bench 4.0, Argon comes **last of four**, on Google's own table. Terminal-bench puts an agent in a real shell, which is close to how most of us would use a coding model, and Opus 5.5 leads it by nine points. One Hacker News commenter, lanthissa, put it plainly: "deepswe vs frontierswe spread is huge. I think that should be a really bad sign, but hope its great."

The independent leaderboard I could find with Argon on it is Artificial Analysis. Its chart marks some models as "not publicly available", so it had pre-release access.

| Model (setting) | AA Intelligence Index | 
|---|---|
| Claude Opus 5.5 (max with fallback) | **58** | 
| Claude Fable 5.1 | 53 | 
| GPT-6 Astra (max) | 53 | 
| Gemini 4 Argon (high) | 53 | 
| GPT-6.1 Sol (max) | 52 | 

Argon ties for second, five points behind Opus 5.5, which has held the top spot there since the Opus 5.5 launch. Two caveats: Artificial Analysis tested Argon on the "high" setting, and its index (v4.3.2, ten evals) includes Terminal-Bench 4.0, one of the rows where Google's own table already had Argon last.

The fair part for Google is the bill. A run on the index costs **$1.99 per task** with Argon against **$5.98** with Opus 5.5, so about a third. Argon is also chatty: it used about 110 million output tokens to run the index, against a median of 82 million. At $10 per million output tokens, that verbosity is part of the price you would pay.

The real pitch of the launch is security. Google writes that "Argon can autonomously find, validate, and patch critical software vulnerabilities", and that "for trusted defenders and our own internal teams at Google, we'll be releasing Argon without cyber guardrails". Some videos described this as fixing zero-days on its own; the word "zero-day" does not appear in Google's post.

Two security results are in the post:

- 
**Wiz** used Argon to find "a critical vulnerability exposing sensitive personal information across healthcare software used by hospitals worldwide." Wiz also reports an internal black-box pentest benchmark: Argon 70.9 % against 58.2 % for Gemini 3.8 Flash Cyber.
- 
**CWE-bench v1** : "Argon ties for first place with a top score of 68%".

Context for the first one: Wiz is a Google company. Google completed the acquisition in March 2026, a deal TechCrunch put at $32 billion. So the pentest benchmark is a Google subsidiary grading a Google model. The result may well be real; it is still an internal number.

Context for the second one: the CWE-bench chart in the post shows three models at 68 %, with ties broken by pass@4. The bar drawn on the far left is Grok 4.7.

There is no date. The post says: "We'll continue to gather feedback from early testers as we iterate on guardrails before making Argon available to developers, enterprises, and consumers as soon as possible." The rollout order is "starting with paid API customers and Google AI Ultra subscribers." Google is also "actively engaged in the U.S. government's voluntary process for pre-release model access."

Sundar Pichai's post on X says the same thing in fewer words: it is "going to a set of trusted cyber defenders through our Fairwind Program today. We're going to make it available as soon as we can and as safely as we can. So hold tight". Demis Hassabis wrote "before wider availability soon".

Until then, access runs through the Fairwind Program, which Google launched a month ago with Gemini 3.8 Flash Cyber. It has "over 650 partners globally", and the rules are strict: partners "may only grant Gemini 4 Argon access to internal cybersecurity, incident response, or penetration testing teams, and must track employee access and use." On Hacker News, babelfish summed up the mood: "Gemini not beating the "can't release a model" allegations".

- 
**Plan with the models you can call.** Argon's list price equals GPT-6.1 Sol's, and Opus 5.5 still leads the independent index. Nothing in your stack has to change this week.
- 
**Budget for verbosity.** If Argon reaches the API at $10 per million output tokens and keeps writing about a third more tokens than the median model, measure cost per finished task, which is how Artificial Analysis reports it.
- 
**Run your own shell benchmark on day one.** The rows where Argon trails are the agentic, terminal-style ones. If your use case is an agent in a repo, test exactly that before switching.

Netlify rebuilt Edge Functions, which run "about a billion" times a day. They used to go out to a hosted V8-isolate service; now they run in Firecracker microVMs, built with Unikraft, inside Netlify's own edge network. Netlify reports warm invocations at about 5–6 ms at the median, down from 25–40 ms, roughly 5x faster. Cold starts hit about 1.2 % of calls and take around 9 ms.

On Hacker News, nchmy pointed out that Cloudflare Workers are V8 isolates too and run "vastly faster" than 25–40 ms, so most of the gain likely comes from the request staying inside Netlify's network. CodesInChaos flagged the snapshot design, since Netlify snapshots each microVM after boot: "forked RNG states can lead to catastrophic failures in UUID generators or cryptography." And jedberg reminded everyone where Firecracker comes from: "The next time you want to curse AWS, remember they gave us Firecracker".

I stamped Gemini 4 Argon **NEEDS REVIEW**. The independent number I found has it tied for second, the two coding rows I care about have it last on Google's own table, and the strongest security claim is graded by a company Google owns. The price looks good and the Rust migration is a real story. I'll review it properly the day I can call it.

**Is Gemini 4 Argon available?**

Only to trusted cyber defenders in Google's Fairwind Program, and only for their security teams. Paid API customers and Google AI Ultra subscribers come next.

**When is the Gemini 4 release date for developers?**

Google has not given one. The post says "as soon as possible"; Sundar Pichai wrote "So hold tight".

**How much does Gemini 4 Argon cost?**

An introductory $2 per million input tokens and $10 per million output tokens, with cached input 95 % off.

**Is Gemini 4 Argon better than Claude Opus 5.5?**

On Google's table it wins most rows but loses Terminal-bench 4.0 to Opus by nine points. On Artificial Analysis, Opus 5.5 scores 58 and Argon 53, at about three times Argon's cost per task.

- Google, "Gemini 4 Argon: our next era of frontier intelligence": https://blog.google/innovation-and-ai/models-and-research/gemini-models/gemini-4-argon/
- Hacker News discussion: https://news.ycombinator.com/item?id=49913571
- Google's benchmark table (image): https://storage.googleapis.com/gweb-uniblog-publish-prod/original_images/gemini-4-argon_table_blog.gif
- Artificial Analysis, Gemini 4 Argon: https://artificialanalysis.ai/models/gemini-4-argon
- Google DeepMind, Fairwind Program: https://deepmind.google/fairwind-program/
- Google Cloud, Wiz acquisition completed: https://cloud.google.com/blog/products/identity-security/google-completes-acquisition-of-wiz
- TechCrunch, $32B Wiz deal: https://techcrunch.com/2026/03/11/google-completes-32b-acquisition-of-wiz/
- Sundar Pichai on X: https://x.com/sundarpichai/status/2105387952478277979 and https://x.com/sundarpichai/status/2105387954474746264
- Demis Hassabis on X: https://x.com/demishassabis/status/2105417239432200636
- Google DeepMind on X: https://x.com/GoogleDeepMind/status/2105388084154056939
- Netlify, Edge Functions on Firecracker microVMs: https://www.netlify.com/blog/edge-functions-firecracker-microvms/
- Hacker News discussion (Netlify): https://news.ycombinator.com/item?id=49912444

*This article expands on an episode of **The Daily Diff**, a five-minute daily video on what shipped and what broke in tech. Watch the episode · Subscribe on YouTube · the written diff lands in your inbox every morning at thedailydiff.dev.*

## Top comments (0)
