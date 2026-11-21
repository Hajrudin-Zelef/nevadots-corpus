---
id: collect-261121-veille-ia/muse/muse-wonder-tess-docs
title: "Muse Wonder - Tess Docs"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: ["2026-10-12"]
keywords: ["wonder", "muse", "model", "task", "dollars", "screen", "full", "action"]
source: https://docs.tess.im/en/muse-wonder
source_anchor: ""
source_lines: [1, 54]
section: "Muse"
---
**agents that operate the screen, change the repository, and pick the task back up when a step breaks**. Meta launched the series on October 12, 2026, together with Muse Wonder Full. Wonder is on the API, with the identifier

`muse-wonder-1.0`. Full is the most advanced model in the series and, for now, is available only through a Muse Code subscription. This page is about Wonder.
Meta names two parts of that behavior. Active Memory carries what matters across conversations and brings back the right detail when the task needs it. Active Focus stays on the goal when the task stalls: it tries another path, recovers from an error, asks for clarification, and checks in before an important action.
**52 on the Intelligence Index**, ranking 9th among 216 models in the same class. The median for that price range is 27. In that run the model generated text at 104.1 tokens per second, above a median of 83.6, and took 19.73 seconds to the first token, against a median of 5.87 seconds. That time includes reasoning. Average cost was 4.52 dollars per index task, at a list price of 4 dollars for input and 20 dollars for output per million tokens. On BenchLM, the partial aggregate is

**70.44 out of 100**, ranking 16th among 891 models. Coverage is still partial (21 benchmarks), so the score is conservative.

- On from-scratch programming, **Opus 5.5 opens a wide gap** (70.5% versus 50.0% on ProgramBench).
- On terminal coding and computer use, Opus 5.5 and GPT-6 Astra stay ahead.
- Time to first token rises while the model is reasoning. For a short classification, another model answers sooner.
- In Artificial Analysis, the max variant writes more than the median for the range: 130 million output tokens on the index, against 85 million. Ask for short answers when the format matters.
- Muse Wonder Full scores higher on Meta’s table. It is not the API model.

**Expected result:**the change you asked for, verified, with no extra file outside the scope. The plan breaks on a tool, a source, or a step that will not close. The model tries another path, says where it stopped, and continues an important action only after you confirm.

**Expected result:**the task finishes, or it stops with the blocker written down, with no silent action. The figure is in a system, a PDF, and a spreadsheet, and someone has to work across all three. Wonder sees the screen, joins the sources, and returns what it checked.

**Expected result:**every number points to the screen or the file it came from.

Pick a task with a screen, code, or several tools

Lock what counts as done

Set tools and approvals

Ask for an explicit recovery

Sample before you let it run

**Input:**the repository, the expected behavior, and the tests that already exist.

**Tools:**reading and editing files, running the project’s tests.

**Done when:**the requested tests pass and the diff stays inside the scope.

**Input:**the goal, the sources, and the action that needs confirmation.

**Tools:**the process integrations, read-only until confirmation.

**Done when:**the result is done, or the blocker is written down, and no important action went out without confirmation.

**Input:**the PDF or spreadsheet and the system where the same figure appears.

**Tools:**file reading and the system screen.

**Done when:**every number has both sources, or the disagreement is explicit.

- Muse Wonder Full is not this model. It leads Meta’s table, and access today is through a Muse Code subscription.
- A 1M-token context does not replace selecting information. Send only relevant content.
- List price on Meta’s API is 4 dollars for input and 20 dollars for output per million tokens. A cache read costs 0.20 dollars.
- Meta describes strong safeguards against chemical, biological, radiological, nuclear, and cyber misuse, and says defensive security work remains possible. A full safety report was still to come at launch.
- Review a sample before publishing, sending, or automating decisions.
- Keep human approval for financial, legal, clinical, or high-impact decisions.
