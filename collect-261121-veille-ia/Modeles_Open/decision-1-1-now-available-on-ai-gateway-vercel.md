---
id: collect-261121-veille-ia/modeles-open/decision-1-1-now-available-on-ai-gateway-vercel
title: "Decision-1.1 now available on AI Gateway - Vercel"
domain: modeles-open
role: reference
task: article
actors: []
dates: []
keywords: ["decision-1", "model", "request", "available", "gateway", "decision", "microsoft", "answers"]
source: https://vercel.com/changelog/decision-1-1-now-available-on-ai-gateway
source_anchor: ""
source_lines: [1, 22]
section: "Modeles_Open"
---
Decision-1.1 from Microsoft AI is now available on AI Gateway. It is more accurate, better calibrated, and faster than Decision-1.

The decision model returns structured answers instead of free-form text for tasks such as routing support requests, choosing which model should handle a request, selecting the next step in a workflow, or grading AI responses.

- Boolean: the probability that a condition is true
- Choice: a selected category and probabilities for each option
- Score: a numerical rating on a scale you define

Multiple questions can share the same input in one request. Better calibration means the model's probabilities track how often it is right, so your application can set thresholds for when to act, flag a case for review, or escalate.

Decision-1.1 accepts the same request format as Decision-1. To switch, change the model ID to `microsoft/decision-1.1`.

Call it through the AI SDK, the OpenAI-compatible Decisions API, or the AI CLI. The examples below require `ai` 7.0.128 or later for TypeScript, or the OpenAI Python SDK 3.26.0 or later for Python.

```
import { experimental_decide as decide } from 'ai';
const { answers } = await decide({  model: 'microsoft/decision-1.1',  state: 'I was charged twice for one order.',  questions: {    department: {      type: 'choice',      instructions: 'Which team should handle this?',      criteria: {        billing: 'Charges and refunds',        technical: 'Bugs and outages',      },    },  },});
console.log(answers.department.choice);
```
Decision requests appear in request logs, count toward budgets, and support decision fallbacks.

Try Decision-1.1 in the model playground, or see the decision models documentation for request formats and question types.
