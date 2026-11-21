---
id: collect-261121-veille-ia/claude-anthropic/fable-5-2-in-claude-code-add-just-this-one-line-to-your-claude-md-wmedia-es
title: "Fable 5.2 in Claude Code: add just this one line to your CLAUDE.md | wmedia.es"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["claude", "fable", "section", "code", "model", "memory", "notes", "line"]
source: https://wmedia.es/en/tips/claude-code-fable-5-2-claude-md-cleanup
source_anchor: ""
source_lines: [1, 106]
section: "Claude_Anthropic"
---
**TL;DR** With Fable 5.2 active, run `/doctor prompt-audit` and apply what it proposes for lines that ask for less thinking, lines that always delegate, hard style limits and token budgets; rules that say "never" without a reason are yours to rewrite. If the repo is yours, add "This codebase is yours: what it's for and how it's built are your call, and you'll keep building on it." to the project's `CLAUDE.md`, and review the diff, not only the summary.


Fable 5.2 **follows what you put in your `CLAUDE.md` closely**, and that changes which lines still earn their place. Anthropic's prompting guide has a section on instructions written for earlier models: in its testing, some raised cost, some caused side effects and some stopped doing anything. It also brings a new sentence, about who owns the code, that changes how the model treats your repo.

With Fable 5.1, six sections of its guide already shipped inside the binary. This is the same split for Fable 5.2, checked on v2.1.322: what Claude Code sends on its own, what your `CLAUDE.md` no longer needs, and what is missing.

## What Claude Code already sends with Fable 5.2

The Fable 5.2 entry in the binary's model catalog carries the `fable_5_2_prompt_bundle` capability. With it, Claude Code turns on, by default, a `# Finishing work` section, a `# Memory, notes and feedback` section, a reminder when Claude has spent a minute on tool calls without writing anything to you, and a note on original games. You can see it by asking the model seven yes or no questions: the two sections, four sentences from the guide that could be there (the code ownership one, "Write for an intelligent but low-context reader", "Before you start, say in a line" and "Every few tool calls, give the user a one-line update"), and the "Writing for the user" section Claude Code sends with Fable 5.1:

```
$ claude -p --setting-sources project --model claude-fable-5-2 "$(cat questions.txt)"
1. YES — under the heading "# Finishing work": "Ending your turn means your work stops there until asked to continue, and you should not stop unless needed."
2. YES — under the heading "# Memory, notes and feedback": "This adds to the Memory section and takes precedence over it."
3. NO
4. NO
5. NO
6. NO
7. NO
```
With `--model claude-fable-5-1` on the same binary, answers 5 and 7 come back YES: moving to Fable 5.2, Claude Code stops sending the "Before you start" sentence and the "Writing for the user" section. Section by section:

| Guide section | In Claude Code with Fable 5.2 | 
|---|---|
| Finish the whole task | Its own `# Finishing work` section; don't paste it again | 
| Keep caution from piling up | `# Memory, notes and feedback` , only for the notes in its auto memory | 
| Keep users informed during long turns | A harness reminder after 60 seconds of tool calls with no text | 
| Make reports easy to follow | Not sent | 
| Say who owns the code | Not sent | 
| Calibrate effort | Not text: `/effort` , and Fable 5.2 starts at`medium` | 

The memory section tells the model that if its notes "contain some caution or limitation, question whether the reason still holds and drop it if not". That is about the notes Claude writes. **Your `CLAUDE.md` is not in it**: the section never mentions it, so cleaning it up is your job.

## What to delete from your CLAUDE.md

**1. Run the audit with Fable 5.2 active**

```
/doctor prompt-audit
```
It reads your `CLAUDE.md`, `CLAUDE.local.md` and `AGENTS.md` files plus what sits under `.claude/` and `~/.claude/` (rules, skills, commands, subagents and output styles), and hands you a report with a proposed diff. It changes nothing until you ask (v2.1.283 or later; the full walkthrough is here). On a test repo whose `CLAUDE.md` held five lines typical of earlier models, on v2.1.322 with Fable 5.2:

| Line in `CLAUDE.md` | Verdict | Proposal | 
|---|---|---|
| "Answer directly without deliberating. Do not think before answering." | High, `remove` | Delete it | 
| "You have a budget of 20,000 tokens for each task; say how many are left at the end." | High, `remove` | Delete it | 
| "Use subagents for every search and every code review, in parallel." | Medium, `rewrite` | "Use subagents for searches or reviews that span many files. Do small lookups directly." | 
| "Writing style: one instruction per sentence, in 20 words or fewer." | Medium, `rewrite` | "Write short, plain sentences." | 
| "Never retry a failed command." | Low, `flag` | Only if it has no reason behind it: "When a command fails, read the error and fix the cause before running it again." | 

It tagged the budget, the delegation and the style lines as patterns the Fable 5.2 guide says to test again. Why each one goes:

**2. Lines that ask for less thinking.** At `medium` and above they don't stop Fable 5.2 from reasoning. In the test below, with that line in `CLAUDE.md`, both runs still have a thinking block in their transcript, and they spent 1,563 and 2,644 output tokens against 1,896 and 1,517 without it. If speed was what you wanted, lower the effort: `/effort`, pick `low` and press `s` for this session, or set it per model.

**3. Lines that always delegate.** According to the guide, when your prompts invite delegation Fable 5.2 delegates readily, and subagents can be a large share of what a session costs.

**4. Hard style limits.** It follows style rules closely, and a cap like "one instruction per sentence, in 20 words or fewer" leaves its prose choppy.

**5. "Never" rules with no reason.** In Anthropic's testing, with a dozen notes from earlier sessions in context, one of them saying never to retry, and nothing in the prompt allowing retries, the model retried after a busy tool in about four tries in ten, against nearly all without those notes. Rewrite the rule with its scope and its reason:

```
- Never retry a failed command.
+ Don't rerun ./deploy.sh after a failure: it isn't idempotent. Commands that are safe to repeat can be retried.
```
**6. Token budgets written into the prompt.** In Anthropic's testing on coding tasks at `high` and `xhigh`, removing a large budget lowered cost with equivalent performance.

## The line to add

Neither the audit nor the binary adds it for you:

```
This codebase is yours: what it's for and how it's built are your call, and you'll keep building on it.
```
Without it, Fable 5.2 tends to **build on top of the existing structure**, even when that repeats code, and to report nearby problems instead of fixing them. With it, in Anthropic's testing, it merged repeated code in about half of the tries, against under one in ten.

The test here: a Python repo with the same email validation copied into `users.py`, `invites.py` and `billing.py`, and a request to add `subscribe(email)` in a new `app/newsletter.py` that rejects invalid emails the way the rest of the app does, with a test. Claude Code v2.1.322, `claude -p --model claude-fable-5-2 --effort medium`, with `--setting-sources project` and permission only to edit files and run the tests, one run per row:

| `CLAUDE.md` | Copies of the validation afterwards | Files changed | Output tokens | Tests | 
|---|---|---|---|---|
| Without the sentence | 4 | 2 | 1,896 | 7 passing | 
| Without the sentence | 4 | 2 | 1,517 | 6 passing | 
| With the sentence | 1, in `app/emails.py` | 7 | 4,796 | 9 passing | 
| With the sentence | 1, in `app/emails.py` | 6 | 3,799 | 7 passing | 

Without the sentence, it noticed and left it to you as a question:

```
- **Copied validation:** I copied the validation block rather than extracting it, so it now lives in four files. Pulling it into a shared helper is a small refactor of the three existing modules — say if you want it.
```
With it, it did the work:

```
- **Shared validation:** the email check was copy-pasted in `users.py`, `invites.py` and `billing.py`. Rather than add a fourth copy, I moved it into `normalize_email()` in a new `app/emails.py`, and all four modules now call it. Behaviour is unchanged: same trimming and lowercasing, same pattern, same 254-character limit, same `ValueError` messages.
```
The price is in the table: **about 2.5 times the output tokens**, and files you didn't ask it to touch, one of them a test nobody requested. The guide measures about 1.5 times the output tokens, and warns that, bug fixes aside, it changed how an existing feature behaves in one try in six to ten (more often at higher effort), and that the closing summary names the larger changes but often leaves out the small ones. So:

- **Put it in the project's `CLAUDE.md`, never in `~/.claude/CLAUDE.md`.** On a client's repo or an open source pull request you want the smallest change, and a scope rule elsewhere doesn't cancel it: in Anthropic's testing, with "No drive-by refactors, renames or cleanups" in the same prompt, it still merged code in about a third of the tries. If your team doesn't want it in the repo, put it in your`CLAUDE.local.md` .
- **Read the diff, not only the summary** , starting with`git diff --stat` .

Official docs: Prompting Claude Fable 5.2 · Audit your instruction files


## Requirements

- Claude Code **v2.1.321 or later** with Fable 5.2 (`/model fable` ). Checked on v2.1.322.
