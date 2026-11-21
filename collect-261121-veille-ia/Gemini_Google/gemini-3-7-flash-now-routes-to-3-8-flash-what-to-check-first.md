---
id: collect-261121-veille-ia/gemini-google/gemini-3-7-flash-now-routes-to-3-8-flash-what-to-check-first
title: "Gemini 3.7 Flash now routes to 3.8 Flash: what to check first"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-10-08"]
keywords: ["flash", "gemini", "gemini-3", "model", "google", "token", "calls", "more"]
source: https://codedtx.com/insights/gemini-3-7-flash-routed-to-3-8-flash
source_anchor: ""
source_lines: [1, 58]
section: "Gemini_Google"
---
Google has deprecated Gemini 3.7 Flash, and every request that names gemini-3.7-flash is now answered by Gemini 3.8 Flash. Apps that never changed their model string are already running on a different model, so teams should re-run their evals, watch token use and decide whether to move to 3.8 Flash or 3.6 Flash.

## What was released

A model change was announced by Google on 8 October 2026 in the Gemini API release notes. Gemini 3.7 Flash is deprecated and requests to `gemini-3.7-flash` are automatically routed to `gemini-3.8-flash`. In the same note, `gemini-3.5-flash` is deprecated and routed to `gemini-3.6-flash`, and the `deep-research-pro-preview-12-2025` agent will be shut down on 23 October 2026.

Gemini 3.8 Flash itself is not new: Google made it generally available on 2 September 2026. What is new is that teams who stayed on 3.7 Flash no longer have the choice of when to move.

## What actually changed

- **No shutdown date, no error.** The deprecations table lists gemini-3.7-flash with no shutdown date announced and gemini-3.8-flash as its replacement. Calls keep succeeding, which is exactly why the switch is easy to miss.
- **The interface stays the same.** The 3.8 Flash model page shows the same input types, context window, output limit, tools and thinking levels (low, medium and high) as 3.7 Flash. Medium is the default on both, and minimal returns an error on both.
- **Token use can rise.** Google says in its Gemini 3.8 Flash guide that 3.8 Flash can use more tokens on long, complex tasks by design, because it takes smaller reasoning steps, calls tools more often and checks its own work.
- **The price is introductory.** The Gemini API pricing page lists 3.8 Flash at an introductory rate through 31 December 2026, with standard input and output prices that double from 1 January 2027. Gemini 3.6 Flash is on the same introductory schedule.
- **Managed Agents default.** The Antigravity agent in Gemini Managed Agents, and the Antigravity SDK, now use Gemini 3.8 Flash by default.

## Why a silent switch matters

A model change behind an unchanged ID breaks the link between what your code says and what actually runs. Prompts, tool schemas and output parsers that were tuned against 3.7 Flash are now being tested in production against 3.8 Flash. Your own logs and traces that record only the requested model will still say 3.7 Flash, so a quality or cost shift can look like a bug in your own code.

The token point matters most for agents. A model that calls tools more often and verifies its work can be more accurate, but it can also hit step limits, rate limits or per-task budgets that were sized for the older model. A loop that finished in a handful of tool calls may now take more, and each extra call is also a chance for a side effect.

## What to check first

In CodeDTX's view, these are the practical steps, in order:

1. **Find every caller.** Search code, configuration and prompt templates for`gemini-3.7-flash` and`gemini-3.5-flash` , including scheduled jobs and internal tools that nobody watches day to day.
2. **Compare before and after 8 October.** Check token use per request, tool calls per task, latency and error rates either side of the switch. Our guide to controlling what an AI agent costs to run covers which numbers to track.
3. **Re-run your evals on 3.8 Flash.** Treat it as a model upgrade, not a rename. Our post on what AI agent evals catch explains which failures show up first.
4. **Pick the target deliberately.** Update the model string to`gemini-3.8-flash` if the evals pass, or to`gemini-3.6-flash` , which Google says remains supported, if the extra token use is not worth it for the workload. Lowering the thinking level on 3.8 Flash is the other lever Google points to for everyday tasks.
5. **Budget for January.** Any cost case built on the introductory rate needs a second line for standard pricing from 1 January 2027.
6. **Move Deep Research callers now.** Requests that name`deep-research-pro-preview-12-2025` need the`agent` parameter changed to`deep-research-preview-04-2026` or`deep-research-max-preview-04-2026` before 23 October 2026.

## When not to switch to 3.8 Flash yet

Stay on a pinned 3.6 Flash, or keep 3.8 Flash at low thinking, if the workload is short, high volume and cost-sensitive, such as classification, extraction or chat replies, and your evals show no gain. The case for 3.8 Flash is long, multi-step agent and coding work, where Google positions the model. For a wider playbook on handling these events, see what to do when a model is deprecated. The release note covers the Gemini API, so teams on Google's enterprise platform should confirm the routing on that platform's own documentation.

## Frequently asked questions

### Is Gemini 3.7 Flash still available?

Not as a separate model. Google deprecated Gemini 3.7 Flash on 8 October 2026, and every Gemini API request that names gemini-3.7-flash is now automatically routed to gemini-3.8-flash. The calls still succeed and no shutdown date has been announced, but the answers come from Gemini 3.8 Flash, so you are already using the newer model whether or not you changed your code.

### Will my Gemini costs go up after the routing change?

They can. Google says Gemini 3.8 Flash may use more tokens on long, complex tasks because it reasons in smaller steps, calls tools more often and checks its work. Per-token prices are introductory until 31 December 2026, and the standard input and output prices from 1 January 2027 are double that rate. Compare token use per request before and after 8 October to see your actual change.

### Do I need to change code to move to Gemini 3.8 Flash?

The model string is the main change. Google recommends updating it to gemini-3.8-flash rather than relying on the automatic routing. The thinking levels, built-in tools and limits match Gemini 3.7 Flash, and minimal thinking still returns an error. Teams coming from older models may also need the Gemini 3 parameter changes listed in Google's migration checklist, such as removing sampling parameters.

### Should I move to Gemini 3.6 Flash instead?

It depends on the workload. Google says Gemini 3.6 Flash remains supported and suggests it, or a lower thinking level on 3.8 Flash, for everyday tasks that do not need extra verification. For short, high-volume work such as extraction or classification, run your evals on both and compare token use. For long agent and coding tasks, 3.8 Flash is the model Google designed for that work.

### What happens to Gemini 3.5 Flash and the Deep Research preview agent?

Gemini 3.5 Flash is deprecated in the same way: requests to gemini-3.5-flash are now routed to gemini-3.6-flash, and Google recommends updating the model string. The deep-research-pro-preview-12-2025 agent is different, because it will be shut down on 23 October 2026. Callers need to switch the agent parameter to one of the April 2026 Deep Research versions before that date.
