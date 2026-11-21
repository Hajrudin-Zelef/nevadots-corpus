---
id: collect-261121-veille-ia/deepseek/hugging-face-harness-leaderboard-adds-deepseek-harness-dsh-statistically-tied-for-first-in-all-five-
title: "Hugging Face Harness Leaderboard Adds DeepSeek Harness: DSH Statistically Tied for First in All Five Evaluations, 2nd on DeepSWE, 6th on Terminal-Bench After Shutting Down Its Own Servers — AICoder"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: []
keywords: ["harness", "deepseek", "agent", "terminal-bench", "tied", "three", "deepswe", "hugging"]
source: https://aicoder.com/news/news-20261111-hf-harness-leaderboard-adds-deepseek-harness-dsh-tied-first-all-five-evals-deepswe-2nd-terminal-bench-6th
source_anchor: ""
source_lines: [1, 45]
section: "DeepSeek"
---
On November 11 Hugging Face researcher Joel Niklaus said DeepSeek Harness (DSH), DeepSeek's open-source coding agent, now appears on his Harness Leaderboard, which holds the model and reasoning effort fixed and swaps only the harness across 10 coding agents. By his figures DSH (developer preview 0.2.2-rc.1) is statistically tied for first in all five evaluations: DeepSeek-V4.1-Flash and GLM-5.3-Flash at low and high effort, plus Flint. Averaged over the three models it ranks 2nd on DeepSWE, 0.5 points behind Nous Research's Hermes Agent, but 6th on Terminal-Bench. The main reason: DSH shuts down every background program it started when it finishes, so on three tasks where the grader checks a server after the agent exits it passed only 2 of 36 attempts, costing 1.5 to 1.7 points per evaluation. It averages $0.53 per solved trial. This is an individually maintained research board, not an official DeepSeek or Hugging Face announcement.

## Key Takeaways

- ✓The board fixes the model and swaps only the harness across **10** coding agents; DSH is statistically tied for first in**5 of 5** evaluations (author figures)
- ✓DeepSWE, averaged over three models: **2nd** ,**0.5** points behind Hermes Agent; Terminal-Bench:**6th**
- ✓On three Terminal-Bench tasks that need a server left running, DSH passed **2 of 36** ; OpenCode, Pi and Hermes Agent passed**36 of 36**
- ✓Those three tasks cost DSH **1.5 to 1.7** points per evaluation, more than its**1.3** -point gap to the leader on DeepSeek-V4.1-Flash at high effort
- ✓Average cost **$0.53** per solved trial; tested version 0.2.2-rc.1 with web search off because it needs a DeepSeek account key

### Key Decision Metrics at a Glance

#### Turn your technical choice into a development budget

Compare 40 dev plans & simulate token costs vs $20/mo subscriptions

### Project Links & Resources

Direct Access
## In-Depth Technical Analysis

### Background and the pain point

The same open model can perform very differently inside different coding agents (harnesses), yet most leaderboards compare models, not harnesses. Joel Niklaus's Harness Leaderboard answers which harness to run a given model in: it pins weights, providers and reasoning effort, runs each task three times, and publishes every trajectory. DeepSeek Harness is DeepSeek's own open-source coding agent, and until now there was no third-party comparison.

### How the board works

The board runs Terminal-Bench 2.1 and DeepSWE v1.1 on Hugging Face Sandbox, Jobs and the inference router, and marks a harness as tied when the 95% bootstrap interval of its paired difference to the leader covers zero. Looking up published solutions or fetching the upstream code a task was cut from counts as a fail. The changelog says DSH talks to a metering proxy through an OpenAI-compatible endpoint, and 20 of its passes were overturned, for example for fetching upstream code.

### Results (author figures)

DSH is statistically tied for first in all five evaluations, ranks 2nd on DeepSWE and 6th on Terminal-Bench. Most of the gap comes from one habit: when it finishes, DSH stops every background program it started, while three tasks require a service, such as a model inference API, to still answer the grader after the agent exits. DSH passed 2 of 36 attempts on those.

### Practical guidance

For repository-level code changes on DeepSeek-V4.1-Flash or GLM-5.3-Flash, DSH is already in the top tier at about $0.53 per solved trial. If your task needs a service left running after the agent finishes, note that it cleans up its processes on exit; run the service under systemd or nohup, or say so in the prompt. The tested build is still a developer preview, so behavior may change in a stable release.

Benchmark side-by-side against alternatives, or calculate monthly token cost vs subscription break-even.

## Discussion & Comments

0
Sign in to join the discussion

Connect with AI developers to exchange benchmark insights.
