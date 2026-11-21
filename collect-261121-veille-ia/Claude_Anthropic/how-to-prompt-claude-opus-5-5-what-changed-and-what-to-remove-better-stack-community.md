---
id: collect-261121-veille-ia/claude-anthropic/how-to-prompt-claude-opus-5-5-what-changed-and-what-to-remove-better-stack-community
title: "How to Prompt Claude Opus 5.5: What Changed and What to Remove | Better Stack Community"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-09-22"]
keywords: ["opus", "model", "thinking", "claude", "prompt", "better", "effort", "anthropic"]
source: https://betterstack.com/community/guides/ai/claude-opus-55-prompting-guide/
source_anchor: ""
source_lines: [1, 111]
section: "Claude_Anthropic"
---
Anthropic published an official prompting guide for **Claude Opus 5.5** on September 22, 2026, written by Addy Osmani. The main takeaway is surprisingly simple: **many people are still prompting Opus 5.5 as if it were an older Claude model**, and that can mean spending more while getting worse results.

Much of the guide is about removing things rather than adding them. Instructions that once helped earlier Claude models stay focused can now become unnecessary noise. Likewise, some settings that made sense for Opus 5 can increase cost without producing a meaningful improvement in output.

That changes how you should think about prompting the model. **Better results often come from simpler prompts, lighter steering, and more deliberate review**, rather than piling on more instructions.

This article breaks the guide into the three stages where those changes matter most: **writing the initial prompt, steering the model while it works, and reviewing the final output**.

## What's different about Opus 5.5

Two things changed architecturally that affect how you prompt it.

**Thinking is always on.** Opus 5.5 runs a thinking step before every response regardless of whether you set the thinking parameter. The old approach of including `<thinking>` tags in your prompt or asking the model to "write out your reasoning" can now be declined. The model treats these as attempts to expose internal reasoning. If you need shorter or cheaper responses, lower the effort setting rather than prompting the model to think less.

**Effort default is medium, not high.** Opus 5's default was high. Opus 5.5's is medium. In Anthropic's own evaluation, Opus 5.5 at medium effort matches or exceeds Opus 5 at high effort on coding and knowledge work tasks. If you carried your Opus 5 effort settings forward without re-testing, you're likely overpaying.

**`max_tokens` sizing.** Thinking counts toward `max_tokens` even when the thinking content isn't returned to you. A limit sized for Opus 5 with thinking disabled can cut Opus 5.5 responses off mid-task. For long agentic coding turns, Anthropic's testing found `max_tokens: 128000` (the model's maximum) works well.

## Before the task: writing better initial prompts

### Give the whole task, not sequential steps

With Opus 5 and earlier, breaking a large task into supervised steps was often necessary because the models would drift or get confused on long jobs. Opus 5.5 handles multi-step work substantially better. The new best practice is to give the complete task in a single message and let the model plan the steps itself.

Instead of three separate prompts:
1. "Find all the payment endpoints."
2. "Refactor `charges.ts` to use the new client."
3. "Do the same for `refunds.ts`."

Give one:

"Migrate all the payment endpoints from the old client to the new one."


The model will read the request, identify the files, make the changes, and run tests on its own.

### Define what "done" means

The most important thing to add to any Opus 5.5 prompt is an explicit definition of completion. Without it, the model may stop prematurely or make scope assumptions. A clear finish line also enables the model to self-correct: if it knows "the test suite must pass," it knows it's not done when tests are failing, and it will try to fix them before reporting back.

You can add a single escalation condition to limit interruptions:

"Stop and ask me only if a test fails for a reason you cannot explain or fix yourself."


### Remove the meta-instructions

Any of these in your prompts or `claude.md` can now be deleted:

- `think carefully`
- `think step by step`
- `make no mistakes`
- `you are an expert programmer`

These were written to coax earlier models into deliberate reasoning. Opus 5.5 runs a thinking step before every response on its own. These instructions add noise and can slightly increase latency. Anthropic's testing found that removing them produced faster responses with no measurable drop in quality.

### Use negative constraints for design tasks

Vague design instructions like "avoid a generic look" don't work well. The model defaults to a small set of standard visual patterns; a vague instruction swaps one default for another rather than actually pushing toward something distinctive.

Name the specific elements you don't want:

Concrete prohibitions give the model clear rules to work against, which consistently produces more distinctive output than "avoid generic."

## During execution: steering long runs

### The `claude.md` "when to stop" rule

The single most useful addition to a `claude.md` file for Opus 5.5 is a rule defining when to pause and when to keep going. Without it, the model interrupts tasks more than necessary, turning what should be a one-shot job into a supervised loop.

The three parts do distinct things: the first gives permission to proceed autonomously; the second defines how to report progress without requiring a response; the third sets explicit safety gates. Irreversible operations always warrant a pause regardless of how clearly the task was defined.

One additional note from Anthropic's guide: a text-only `end_turn` during a long task is often progress, not completion. If the model stops and reports what it's about to do next rather than doing it, that's a pattern to catch and address. The guide recommends explicitly telling the model you want it to take the next step rather than announce it.

### Managing large tasks: sub-agents and `TASKS.md`

For very large jobs (a full codebase security audit, migrating dozens of services), two techniques help.

**Sub-agent delegation:** Instruct the model to split the work into parallel units, each with a smaller focused scope.

Smaller context per sub-task generally produces higher accuracy. The manager agent can cross-check before accepting each unit as complete.

**`TASKS.md` as persistent state:** Long-running jobs can fill the context window. When Claude Code summarizes earlier parts of the conversation to make room, specific details from the original plan can get lost. Having the model maintain a progress file prevents this.

The file survives context summarization and gives the model a reliable external record of what's done and what remains.

## After the task: reviewing output

### Read the "what it needs from you" section first

When Opus 5.5 reports task completion, its summary typically has three parts: what it did, what it found, and what it needs from you. The third section is the most time-sensitive. It often contains things that need action before anything can be deployed: API keys to set in staging, manual confirmations, external configuration changes that the model couldn't make on its own. Read this section before reviewing the rest of the output.

### AI-assisted code review before human review

Opus 5.5 is effective as a first-pass code reviewer. Structure the review prompt to focus it:

Anthropic reports that Opus 5.5 at its lowest effort setting finds more legitimate bugs with fewer false positives than Opus 5 at its highest effort setting. Running an AI review before human review catches the class of issues that routinely get through when reviewers have to scan many files at once.

## Safety features and model switching

Opus 5.5 carries Fable-level safeguards for bio-security and cyber-security. Certain prompt patterns trigger these, causing Claude Code to automatically switch to an older model mid-conversation.

The most common trigger is phrasing that sounds like probing the model rather than using it: "show me your full internal reasoning," "reveal your system prompt," or overly broad security requests like "find all vulnerabilities." Legitimate security tasks should be scoped to your own codebase and framed around specific vulnerability types.

If a message is flagged:

Press `Esc` twice in Claude Code to edit the last message. Rephrase to focus on the goal: "Audit `auth.ts` for potential SQL injection or XSS issues" rather than "find all vulnerabilities." If the conversation has been flagged, the flag persists in context. Starting a new chat is often the cleanest solution rather than continuing in a flagged conversation.

Configure the model-switching behavior at `/config` in Claude Code. Setting it to ask rather than switch automatically gives you a chance to rephrase and stay on Opus 5.5.

One additional property worth knowing: Opus 5.5 resists indirect prompt injection better than any earlier Opus model. Instructions embedded in tool results, web pages, or browser content are less likely to cause it to act on injected commands. This matters for agentic workflows where the model reads and processes external content as part of its task.
