---
id: collect-261121-veille-ia/qwen/qwen4-max-scores-close-to-kimi-k3-1-pro-at-lower-cost
title: "Qwen4 Max Scores Close to Kimi K3.1 Pro at Lower Cost"
domain: qwen
role: reference
task: article
actors: ["Alibaba"]
dates: ["2026-11-17"]
keywords: ["qwen4", "work", "cost", "kimi", "tests", "business", "software", "model"]
source: https://winbuzzer.com/2026/11/17/qwen4-max-raises-ai-scores-at-lower-cost-xcxwbn/
source_anchor: ""
source_lines: [1, 71]
section: "Qwen"
---
- Stronger AI: Alibaba’s Qwen4 Max improves on Qwen3.8 Max in independent tests of reasoning, coding and business work.
- Cheaper Tasks: Artificial Analysis measured a $3.27 average task cost, down from $10.02 for its predecessor, using maximum reasoning effort.
- Uneven Strengths: Qwen4 Max scores close to Kimi K3.1 Pro when creating business files, but lower on software tasks using text commands.
- Hosted Access: Developers can connect software to the model online, but cannot download it to run on their own computers.

Alibaba has released Qwen4 Max, an AI model that improves on its predecessor in independent reasoning and work tests while cutting the evaluator’s average task cost by about two-thirds. Developers can use the model through a hosted API to process text, images and video and generate text responses.

The November 16 release pairs a stronger measured result with a different cost profile. External evaluator Artificial Analysis scored Qwen4 Max at 43 on its Intelligence Index, up from Qwen3.8 Max’s 34. Both were tested at Xhigh, their highest reasoning-effort setting, which allocates more processing to working through a problem before answering.

The model is available through QwenCloud, as well as Alibaba’s QwenStudio, QwenWork and Qoder applications. Its tool-calling support lets software connect it to external functions, while structured outputs let developers request responses in a prescribed format. These are useful building blocks for assistants that work with documents or business software.

 📢 Meet Qwen4-Max, your new AI teammate for work, creativity, and discovery!

Qwen4-Max is built on a new MoE architecture with native vision and a 1M-token context window.

Highlights:

 – Outperforms Qwen3.8-Max with up to ~50% fewer tokens, while average cost per task drops over… pic.twitter.com/2Rlkoc6Dm3

— Qwen (@Alibaba_Qwen) November 16, 2026


## **Better Business Work, Mixed Coding Results**

 Artificial Analysis recorded improvements across all nine evaluations in Intelligence Index v5.0.1. The index combines knowledge work, coding, science and general knowledge with equal weight for those four categories. Its score summarizes those selected tests rather than representing a percentage of everyday tasks completed successfully.

One large gain came on HLE-Diamond, a set of 1,000 difficult academic questions that includes image questions. Qwen4 Max answered 45% correctly, versus 24% for Qwen3.8 Max. That 21-percentage-point improvement supplies a concrete reasoning result behind the overall nine-point index gain.

The business-work tests ask for more than an answer to a question. In AA-Briefcase, models receive materials such as spreadsheets, PDFs and interview transcripts and must produce the requested deliverable files. Its 91 tasks span four business scenarios, with grading that considers analytical quality and presentation as well as rubric requirements. A separate evaluation, AutomationBench-AA, tests whether models can complete workflows through tools connected to simulated business applications.

On AA-Briefcase’s Elo rating, which expresses relative performance, Qwen4 Max scored 1686 and Moonshot AI’s Kimi K3.1 Pro scored 1674. The two reported ratings are close. Their coding results diverge more sharply: Qwen4 Max passed 48% of Terminal-Bench 4.0 tasks, compared with Kimi’s 67%. A terminal lets the model operate software by entering text commands. The model passes a task only when every verification test succeeds.

FrontierCode-AA 2.0 evaluates changes to software repositories, collections of code for a project. It uses held-out tests reserved for evaluating the submitted work, together with a grading rubric. Qwen4 Max and Kimi each scored 26%, while Tencent’s Hy4 scored 31%. Hy4’s lower overall index score therefore coexists with a stronger result on that particular coding evaluation.

| Measure | Qwen4 Max | Kimi K3.1 Pro | Tencent Hy4 | 
|---|---|---|---|
| Intelligence Index v5.0.1 | 43 | 46 | 35 | 
| AA-Briefcase Elo | 1686 | 1674 | 1446 | 
| Terminal-Bench 4.0 | 48% | 67% | 37% | 
| FrontierCode-AA 2.0 | 26% | 26% | 31% | 
| Weighted cost per task | $3.27 | $3.68 | $1.03 | 

Elo ratings, task scores and index points use different scales. Dollar figures are weighted average costs per Intelligence Index task.

### **Fewer Generated Tokens Lower the Task Bill**

 Artificial Analysis attributes Qwen4 Max’s lower measured task cost to both reduced token use and lower prices. Tokens are the small units of text that models process and generate, including their reasoning output. The evaluator recorded about 105,000 output tokens per weighted index task, down from roughly 171,000 for Qwen3.8 Max.

At the same time, fresh-input and output prices fell 25%, while the price of cached input fell 70%. Fresh input costs $1.50 per million tokens and output costs $4.50 per million tokens. Previously processed input retrieved from a cache costs $0.075 per million tokens.

Those changes brought the weighted average cost from $10.02 to $3.27 per index task, a reduction of about 67%. Artificial Analysis prices each evaluation’s input, cached input, cache writes, reasoning and answer tokens, divides by its task count, then applies the index weights. Its calculation uses typical live cache-hit rates; a customer’s mix of tasks and repeated input will produce a different bill.

Across the full index suite, Qwen4 Max generated 127 million tokens, against a 78.6 million median for models in its comparison price class.

Lower token prices also coexist with more expensive tasks than some rivals. In the same index version, OpenAI’s GPT-6.1 Sol at Max effort scores 55 at $1.27 per weighted task, despite listed input and output rates of $2 and $10 per million tokens. Its full-suite output total is 63.7 million tokens, roughly half Qwen4 Max’s. Generating fewer billable tokens can outweigh a higher price for each one.

### **Reasoning Settings and Reused Input Change the Trade-Off**

 Developers already had thinking and non-thinking modes in the original Qwen3 family. Qwen4 Max’s API default is High, below the Xhigh setting used for the reported benchmark. Matched High-mode results remain unknown.

Qwen4 also adds automatic placement of explicit cache boundaries and a choice of five-minute or one-hour cache lifetimes. A developer repeatedly supplying the same long document can reuse an identical leading section of at least 1,024 tokens while the cache remains valid. That input is then billed at the lower cache-read rate. The lifetime starts when the cache is processed, so response generation consumes part of it. Requests made one after another can renew the cache on a hit, extending expiry when the new request’s lifetime would end later.

Creating the cache costs $1.875 per million tokens for a five-minute write and $3 for a one-hour write. Expired content must be written again, and upgrading a valid five-minute cache to one hour charges the upgraded portion at the one-hour creation rate. Output tokens retain their usual price. Savings depend on enough reuse to cover those write charges.

Artificial Analysis’s blended price of $0.80 per million tokens assumes seven cached-input tokens, two fresh-input tokens and one output token. Cache-creation charges are separate.

### **A Long Context Window with Hosted Access**

 The model’s one-million-token context window is the combined capacity for input and output in a request. In thinking mode, the maximum input is 983,616 tokens, while reasoning and the final answer share an output ceiling of 262,144 tokens.

Qwen4 Max’s trained model weights, needed to run the model on a user’s own computers, are unavailable to download. The earlier Qwen model Qwen3-Max-Preview also kept its weights private while offering hosted commercial access. Alibaba says smaller Qwen4 models will receive open weights, but has given no release date for them.
