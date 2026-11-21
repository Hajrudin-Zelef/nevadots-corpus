---
id: collect-261121-veille-ia/google-ecosystem/google-labs-web-observer
title: "Google Labs - Web Observer"
domain: google-ecosystem
role: reference
task: article
actors: ["Google"]
dates: ["2026-11-18"]
keywords: ["loop", "google", "stitch", "product", "security", "labs", "agent", "pull"]
source: https://web-observer.com/tag/google-labs/
source_anchor: ""
source_lines: [1, 85]
section: "Google_Ecosystem"
---
**Takeaway:** On November 18, 2026, Google Labs introduced Stitch Loop, a new experiment described as a long-running product agent that connects to your codebase and live metrics, finds what needs fixing, and keeps working 24/7 on your product until your goals are met. Nothing merges until a person reviews the GitHub pull request. A global waitlist opened the same day, with eligible Google AI Ultra users getting priority access.

The announcement is by Justin Toupin, Director, Product Management, Google Labs. Existing Stitch, Google says, already turns ideas into working interfaces in minutes. Together, Stitch and Stitch Loop are presented as steps toward a broader aim: agents that help lean teams from ideation through continuous optimization. The post does not announce general availability, pricing, or a date when the waitlist will convert into open access, and we are not filling those gaps in.

## What Stitch Loop is for

Google’s framing starts with a familiar gap. Coding agents can help a team build and ship features quickly, but their job ends when a pull request merges. After that, the work of patching vulnerabilities, speeding up checkout, and diagnosing signup drop-offs still sits with the team. Stitch Loop is the company’s experiment in keeping an agent on that post-launch work.

You start by connecting GitHub and your product context — logs, user behavior, feedback, and similar signals — with over 180 integrations, custom MCP servers, or local CLI uploads. Grounded in that context, Stitch Loop runs what Google calls evergreen loops toward the measurable outcomes you care about. Three default loops come out of the box, and you can add your own at any time.

## The four steps in each loop

Each loop, Google says, runs through four steps:

- **Suggestions:** Stitch Loop surfaces opportunities to make progress toward your goals, grounded in evidence from your product.
- **Solutions:** Every suggestion comes with one or more solutions, each with tradeoffs and scoping, plus visual design options where applicable.
- **Build:** Once you choose a direction, Stitch Loop writes the code in an isolated cloud sandbox and opens a GitHub pull request for your approval. When relevant, the pull request includes screenshots and videos of the work Loop did.
- **Reflect:** Stitch Loop monitors live product data as it works toward your goal, learning from every positive or negative result to sharpen the next round of insights. Once the goal is met, it keeps watching in case the metric slips.

The approval point is explicit. “You always have the final call. Nothing merges until you review the PR.” Human review is required; this is not a claim that the agent will ship changes on its own.

## Built-in loops, including security with Gemini 4 Carbon

Stitch Loop ships with built-in loops for cybersecurity, latency, and accessibility, and Google says you can create custom loops, giving “improving checkout conversion” as an example.

The security loop is described in some detail. Google says it pairs Gemini 4 Carbon’s cyber security capabilities with CodeMender, Google DeepMind’s autonomous agent that improves code security. Together, they scan the codebase for security vulnerabilities, and Stitch Loop flags each issue it finds with evidence and opens a pull request with a proposed fix for review.

That model choice matters. The official Stitch Loop post names Gemini 4 Carbon, not Gemini 4 Neon. Neon, which Google also introduced on November 18, is a related model in the Gemini 4 family, and Google has previously described Carbon as the model for harder, long-horizon work, including in our coverage of Carbon’s November 10 general availability. Those are useful pieces of context, but they should not be read back into the Stitch announcement. The security loop, as Google described it on November 18, uses Carbon and CodeMender.

## Slack, the Stitch CLI, and other coding agents

Google also positions Stitch Loop as a service layer for coding agents, naming Google Antigravity as an example, or as something you can bring into places a team already works, such as Slack. Whatever coding agent you prefer, the post says, can pull persistent product context from Stitch Loop via the Stitch CLI and MCP. In Slack, you can use @stitch to get daily insight digests and to create new loops.

## The waitlist

The waitlist opened on November 18, 2026, “today” in the language of the post, for all users, globally, with eligible Google AI Ultra users getting priority access. Google points people to stitch.google.com/loop/waitlist and asks for feedback in the Google Labs Discord. Priority access for eligible Ultra users is not the same as a public launch, and the post does not say when waitlisted users will get in, who counts as eligible, or what Stitch Loop will cost.

## Key facts

| Item | What Google’s announcement says | 
|---|---|
| Date | November 18, 2026, according to the official Google Labs blog post. | 
| Who announced it | Justin Toupin, Director, Product Management, Google Labs. | 
| What it is | A new experiment: a long-running product agent that connects to your codebase and live metrics, finds what needs fixing, and keeps working 24/7 until your goals are met. | 
| How it starts | Connect GitHub and product context (logs, user behavior, feedback, and similar signals) with over 180 integrations, custom MCP servers, or local CLI uploads. | 
| Loop shape | Three default loops out of the box, and you can add your own. Each loop runs Suggestions, Solutions, Build, then Reflect. | 
| Human approval | Code is written in an isolated cloud sandbox and opened as a GitHub pull request. Nothing merges until you review the PR. | 
| Built-in loops | Cybersecurity, latency, and accessibility, plus custom loops such as improving checkout conversion. | 
| Security loop | Pairs Gemini 4 Carbon’s cyber security capabilities with CodeMender. It flags issues with evidence and opens a PR with a proposed fix. The official post does not name Gemini 4 Neon for this loop. | 
| Workflows | Can be used as a service layer for coding agents such as Google Antigravity, or in Slack with @stitch for daily insight digests and new loops. Coding agents can pull product context via the Stitch CLI and MCP. | 
| Access | A global waitlist opened November 18, 2026, at stitch.google.com/loop/waitlist, with eligible Google AI Ultra users getting priority access. Feedback is invited in the Google Labs Discord. Pricing and general-availability timing were not published. | 

## FAQ

### What is Stitch Loop?

It is a Google Labs experiment announced on November 18, 2026: a long-running product agent that connects to your codebase and live metrics, finds what needs fixing, and keeps working 24/7 toward your goals. It sits alongside existing Stitch, which turns ideas into working interfaces.

### When was it announced, and who wrote the post?

The official Google Labs blog post is dated November 18, 2026, and is by Justin Toupin, Director, Product Management, Google Labs.

### How do the four steps work?

Google describes four steps in each loop: Suggestions, grounded in evidence from your product; Solutions, with tradeoffs, scoping, and visual design options where applicable; Build, in an isolated cloud sandbox that opens a GitHub pull request; and Reflect, which monitors live product data and keeps watching after the goal is met.

### Does Stitch Loop merge code on its own?

No. Google says nothing merges until you review the pull request, and that you always have the final call. Human approval is required.

### What is the security loop, and which Gemini model does it use?

The built-in security loop, according to the official post, pairs Gemini 4 Carbon’s cyber security capabilities with CodeMender, Google DeepMind’s autonomous agent that improves code security. It flags issues with evidence and opens a pull request with a proposed fix. The announcement does not say the loop uses Gemini 4 Neon.

### How do I join the waitlist, and who gets priority?

Google says the waitlist opened on November 18, 2026, for all users, globally, at stitch.google.com/loop/waitlist. Eligible Google AI Ultra users get priority access. The post does not define eligibility beyond that phrase or say when access will be granted.

### Can I use Stitch Loop with Slack or other coding agents?

Google says you can use it as a service layer for coding agents such as Google Antigravity, or in Slack. Coding agents can pull persistent product context via the Stitch CLI and MCP. In Slack, @stitch is the command given for daily insight digests and for creating new loops.

### Is Stitch Loop generally available, and what does it cost?

The November 18 announcement describes a global waitlist with priority access for eligible Google AI Ultra users. It does not publish a general-availability date or a price, so those details should not be treated as known.
