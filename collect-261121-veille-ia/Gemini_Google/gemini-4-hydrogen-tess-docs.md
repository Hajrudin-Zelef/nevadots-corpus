---
id: collect-261121-veille-ia/gemini-google/gemini-4-hydrogen-tess-docs
title: "Gemini 4 Hydrogen - Tess Docs"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["high", "hydrogen", "answer", "default", "argon", "xhigh", "gemini", "times"]
source: https://docs.tess.im/en/gemini-4-hydrogen
source_anchor: ""
source_lines: [1, 47]
section: "Gemini_Google"
---
`gemini-4-hydrogen`.
- On the Artificial Analysis Intelligence Index, default high thinking scores 51, ahead of Argon at 50. At `xhigh` it reaches 54.
- Against Gemini 3.8 Flash, Google says default high finishes the task 2.5 times faster, lifts Terminal-Bench 4.1 from 18.2% to 57%, and cuts fabricated claims by more than 5 times by flagging missing context instead of filling it in.
- On HLE-Diamond the announcement cites 58.8%. On Artificial Analysis that figure is the `xhigh` score. High is 54.9%, and Argon high is 56.7%.
- Agents’ Last Exam v1.1 is 71.6%. Multimodal Knowledge Agent is 53.4%. Agent Arena rank is 5th.
- Partners cited by Google saw 2 to 2.8 times the speed and 12% to 23% fewer tokens than 3.8 Flash, on graphs, SQL, and data workflows.

`gemini-4-hydrogen` default, and for xhigh.
- Google still points Carbon and Argon at long reasoning. Hydrogen is the fast model, not a replacement for Carbon.
- On Terminal-Bench 4.0 and AutomationBench-AA, high trails Argon high.
- `xhigh` raises the score and the wait. On the index, time to first answer moves from 16 seconds to 44.
- `temperature` ,`top_p` , and`top_k` are ignored inside the allowed range. Outside that range the API returns 400. The`minimal` level returns an error.
- The model card knowledge cutoff is March 2026.

**Expected result:**the diff for this step, the test that passed, and what was left out. The material mixes a PDF, a table, and a chart. The answer needs a source, and it needs to say when the file does not contain the figure.

**Expected result:**the requested text, the source of each number, and the gap when the document does not say. The task has a tool, a search, and a hole in the context. The announcement says Hydrogen flags what is missing instead of filling it in, with more than 5 times fewer fabricated claims than 3.8 Flash.

**Expected result:**what the tool returned, what was missing, and the question that still needs an answer.

Start on Hydrogen

Define what done means

Set tools and approvals

Pick the thinking level

Sample before you repeat

**Input:**the file, the expected behavior, and the test for this step.

**Done when:**the step test passes and what was left out is written down.

**Input:**the PDF or the spreadsheet, the question, and the answer format.

**Done when:**each number points to a source or to a gap.

**Input:**the task, the enabled tools, and what counts as an answer.

**Done when:**the answer separates what the tool confirmed from what stayed open.

- Default thinking on `gemini-4-hydrogen` is HIGH. It spends more tokens than a MEDIUM default.
- A 1,048,576-token window does not replace selection. The reply can use only what remains after the input.
- Explicit cache does not exist. Put the stable prefix at the start of the prompt. Implicit cache looks at prefixes from 4,096 tokens.
- A synchronous request still stops at 65,536 tokens. Going past that needs background execution or continuation.
- Keep a human approval for financial, legal, clinical, or other high-impact decisions.
