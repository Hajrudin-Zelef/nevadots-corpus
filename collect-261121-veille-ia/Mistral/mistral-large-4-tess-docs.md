---
id: collect-261121-veille-ia/mistral/mistral-large-4-tess-docs
title: "Mistral Large 4 - Tess Docs"
domain: mistral
role: reference
task: article
actors: ["Mistral AI"]
dates: []
keywords: ["preview", "source", "every", "mistral", "model", "price", "spreadsheet", "before"]
source: https://docs.tess.im/en/mistral-large-4
source_anchor: ""
source_lines: [1, 53]
section: "Mistral"
---
**multimodal work and tool-using workflows**: reading documents, charts, and technical drawings, financial and legal analysis, and agents that check more than one source before they deliver a result. What stands out is the mix of scale and practical use. It is a Mixture-of-Experts with

**1.05 trillion parameters**, of which 52 billion are active at each step, plus a 1.6 billion-parameter vision encoder. Mistral trained the model from scratch in its own datacenters in Europe and announced that the weights would be released by the end of October 2026. Until then, access is through the preview API.

**38 on the Intelligence Index**, ranking 64th among 226 models in the same class. The median for that price range is 26. In that measurement the model generated text at 116 tokens per second, with 1.46 seconds to the first token. Average cost was $1.13 per index task, calculated at list price ($1.36 / $4.18), not the promotional price. The same evaluation flags two caveats. The model was verbose: 200 million output tokens on the index, against a median of 81 million. And the window measured there was about 524K tokens, while Mistral’s official card states 1M.

- On coding judged by people, **Claude Opus 5 was clearly ahead** (4.22 versus 3.74).
- On Terminal-Bench 4.0, 28.3% is not the level of closed models built for long agentic coding.
- On Artificial Analysis, the preview writes much more than the median for its range. Ask for short answers when the format matters.
- The weights are not published yet. Preview behavior can change before the release.

**Expected result:**a check in which every number or part points back to the visual source. Statements, notes, and reports do not share a format. The model walks the sources, does the math, and separates what is still missing.

**Expected result:**a spreadsheet and a summary in which every value has a source, and missing data is explicit. The task does not fit in one reply: something has to be read, recorded, and returned as a deliverable. Large 4 was measured on this kind of business workflow.

**Expected result:**the step is recorded in the right tool, and anything that could not be finished comes back as a pending item, not as a success.

Pick work that includes a file or an image

Lock the format and the length

Set tools and approvals

Ask for the source of every number

Treat the preview as a preview

**Input:**the image or PDF and the list of what must be verified.

**Tools:**file reading.

**Done when:**every item has a value and a source, or is marked as missing.

**Input:**reports, notes, and the business question.

**Tools:**file reading and spreadsheet generation.

**Done when:**every number has a source, discrepancies are explicit, and the spreadsheet matches the summary.

**Input:**the incoming message, the tracking spreadsheet, and the rule for what to record.

**Tools:**email and spreadsheet, with confirmation before writing.

**Done when:**the row exists only after confirmation, and an incomplete case stays pending.

- The API is in public preview. The weights were announced for the end of October 2026 and are not yet the production artifact.
- The promotional price ($0.68 / $2.09) is half the list published on the same card. Confirm the current price before you estimate cost.
- The official card states a 1M-token context. Artificial Analysis measured about 524K on the preview it evaluated. Do not design a flow that depends on the higher ceiling without testing it.
- A long context does not replace selecting information. Send only relevant content.
- On cybersecurity, Mistral reports strong performance and, at the same time, a higher refusal rate on malicious requests than other open models. Defensive work still follows your organization’s policy.
- The model was trained on data in more than 160 languages, including every official language of the European Union. That does not replace reviewing the text in the language you will deliver.
- Review numbers, citations, and files before publishing or sending.
- Keep human approval for financial, legal, clinical, or high-impact decisions.
