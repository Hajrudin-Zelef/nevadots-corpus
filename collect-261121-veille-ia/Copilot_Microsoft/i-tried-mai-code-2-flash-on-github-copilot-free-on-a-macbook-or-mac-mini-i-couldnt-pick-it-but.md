---
id: collect-261121-veille-ia/copilot-microsoft/i-tried-mai-code-2-flash-on-github-copilot-free-on-a-macbook-or-mac-mini-i-couldnt-pick-it-but
title: "I Tried MAI Code 2 Flash on GitHub Copilot Free on a MacBook or Mac mini — I Couldn’t Pick It, but…"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-11-18"]
keywords: ["copilot", "free", "code", "flash", "github", "pick", "model", "account"]
source: https://medium.com/@iamsupcat/i-tried-mai-code-2-flash-on-github-copilot-free-on-a-macbook-or-mac-mini-i-couldnt-pick-it-but-520ec3934388
source_anchor: ""
source_lines: [1, 85]
section: "Copilot_Microsoft"
---
# I Tried MAI Code 2 Flash on GitHub Copilot Free on a MacBook or Mac mini — I Couldn’t Pick It, but Auto Selection Did | AI Trend First Look · Nov 18, 2026

## GitHub says Microsoft’s new coding model works on Copilot Free. On my free account it never showed up in the model list, yet it handled every task I gave it.

**Quick facts first**

On November 17, Microsoft announced **MAI Code 2** and a lighter **MAI Code 2 Flash** at Ignite. GitHub’s changelog lists Flash as available on **Copilot Free**, so I tried it right away.

- I tested in a **clean macOS virtual machine** on my MacBook. It works the same on a Mac mini.
- The account is **my own Copilot Free account** (as of Nov 18). I didn’t touch any billing or overage settings.
- The tool is the terminal **Copilot CLI** , and I gave the same tasks to**Claude Code** to compare.

The short version: **on my free account I couldn’t pick MAI Code 2 Flash myself. It was used when auto selection (Copilot Fusion) chose it**, and in my tests Fusion chose it all three times.

## 1. Install and sign in

Copilot CLI installs with npm (it needs Node.js).

```
npm install -g @github/copilot
copilot
```
On first run it asks whether to trust the folder; then `/login` signs you in to GitHub. Pick **device code** and you get an "XXXX-XXXX" code to enter at github.com/login/device. **Approving it from your phone's browser works too.** (My first code expired because I didn't enter it within 15 minutes; I just got a new one.)

## 2. Trying to pick the model

`/model` lists a long row of GPT, Claude and Gemini models, but **MAI Code 2 Flash wasn't there.** Searching "mai" found only the older mai-code-1.1-flash, marked as not included in the free plan.

So I went back to the docs. GitHub’s “Supported AI models in GitHub Copilot” page (checked Nov 18) says:

- Above the per-plan table: **“Copilot Student and Copilot Free users have access to models through auto model selection only.”**
- The same page lists **MAI Code 2 Flash** among the models used by auto model selection and Copilot Fusion.

So “available on Copilot Free” seems to mean **“auto selection on Free can use MAI Code 2 Flash.”** Neither the changelog nor the docs mention region limits or a staged rollout. The changelog does say the models are for “eligible GitHub Copilot subscribers on annual plans” and are billed under usage-based billing, so what you see may depend on your account. If you can pick it directly on a free account, I’d love to hear about it in the comments.

## 3. Task 1: explain the code

I gave both tools the same small file with three planted bugs (stats.js: mean, median, mode) and six tests.

`Explain what stats.js does in five short bullets. Do not change any files.`
The top image is Copilot. It first said **“Copilot Fusion selected MAI Code 2 Flash”** and answered in five bullets in under 20 seconds. I hadn’t asked for bugs, but it **flagged the median bugs** (text-order sort, wrong even-length index). One slip: one bullet calls it “a sorted copy,” while the last one says it sorts in place.

## 4. Task 2: fix the bugs

`Three tests in stats.test.js fail. Fix the bugs in stats.js, then run node --test and show the result.`
Copilot CLI defaults to **Manual Approval**: it asks before editing a file and before running a command.

The two fixes were **character-for-character identical.** For simple bug fixes, the free Flash model looks perfectly capable.

## 5. Task 3: build a small page

`Make a single-file index.html with a Pomodoro timer: 25-minute focus and 5-minute break, with Start, Pause and Reset buttons. No external files or libraries.`
I checked both in Chrome on the Mac: **Start, wait 3 s → 24:57; Pause → stays at 24:57; Reset → 25:00.** Both worked. Copilot’s is lean and does exactly what was asked; Claude’s adds features nobody asked for.

## 6. How much can you use for free?

`/usage` shows the free plan gets **200 AI credits a month.** My three tasks used 0.68, which shows as "1 / 200, 0% used."

## Side by side

- **Explain:** Copilot under 20 s, flagged the bugs (one inconsistent line). Claude Code 9 s, all three bugs with an example.
- **Fix:** Copilot about 30 s including two approvals, 6/6. Claude Code 10 s, 6/6. Identical code.
- **Timer page:** Copilot about 3 min including three approvals, works. Claude Code 29 s, works, plus extras.
- **Cost:** Copilot free (0.68 of 200 credits). Claude Code on a paid subscription.

**To be fair:** Copilot’s times include waiting for my approvals, and Claude Code ran in auto mode on a larger paid model, so this isn’t a same-weight-class match. Still, **getting real bug fixes for free, right away,** is genuinely useful.

## If you get stuck

- **MAI Code 2 Flash isn’t in the model list:** per the docs, Free and Student plans use auto selection (Copilot Fusion) only. When you give it a task, the first line and the bottom right show which model Fusion picked.
- **The sign-in code expired:** enter it within 15 minutes. Press any key for a new one. Your phone’s browser works for approving.
- **It asks for approval every time:** that’s the default Manual Approval. Choose option 2 in the prompt to stop asking in that folder.
- **Worried about charges:** the free plan runs within its 200 credits; paying for overage is a separate setting you’d have to turn on. I didn’t.

## Summary

- **Can you use it for free?** On my free account,**I couldn’t pick it directly, but Fusion used it.** All three of my tasks went to MAI Code 2 Flash.
- **How good is it?** Explanations and bug fixes came out nearly the same as Claude Code. The page worked, and it caught its own typo.
- **Downsides:** you can’t choose the model yourself, and the free plan is capped at 200 credits a month.

## Next

When a new AI model or tool comes out, I try it that day and write it up. If there’s something you’d like me to test, leave a comment.

*This post was written with help from AI. The screenshots are real screens from my Mac and a clean macOS virtual machine.*
