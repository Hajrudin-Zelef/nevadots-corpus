---
id: collect-261121-veille-ia/docs-techniques/leaderboard-methodology-8fc6d2de
title: "Leaderboard methodology"
domain: docs-techniques
role: reference
task: article
actors: []
dates: []
keywords: ["model", "task", "user", "signals", "models", "leaderboards", "each", "agent"]
source: ""
source_anchor: ""
source_lines: [1, 48]
section: "Docs_Techniques"
---
How Arena ranks models
Arena’s leaderboards are based on real work done by real people. Some leaderboards use agentic traces from a single model, while others use pairwise preference voting to evaluate model performance.
Two kinds of leaderboards
Agent leaderboards
In Agent Mode, each session goes to a randomly chosen orchestrator model. We evaluate the model based on explicit and implicit signals from the agentic trace.
- Input
- Implicit and explicit signals mined from session traces
- Method
- Trace mining
- Score
- Weighted average of signals (normalized and rescaled)
Battle leaderboards
In a battle, two anonymous models perform the same task. The user votes for the better response.
- Input
- Pairwise votes from battles
- Method
- Bradley-Terry regression
- Score
- Elo-like rating of model strength
Agent leaderboards
In Agent Mode, each session can be thought of as a randomized controlled trial: we randomize the choice of the orchestrator model for each session. We then measure the impact the orchestrator has on task success metrics, agentic tool use, and user satisfaction.
| Index | What it measures | Signals | 
|---|---|---|
| Agent Arena Index | How well the orchestrator does real tasks, with all four signals combined. | Task completion, praise vs complaint, steerability, bash reliability | 
| Task Outcome Index | Whether the model finishes the task and earns praise. | Task completion, praise vs complaint | 
| Steering Index | Whether the model corrects its work when the user asks for a change. | Steerability | 
| Tool Use Index | Whether the model’s shell commands run without failures. | Bash reliability | 
Our trace mining methodology consists of four steps designed to reliably measure model performance from the natural distribution of human traces. First, we randomly assign an orchestrator model to each session, ensuring that differences in outcomes can be attributed to the model. Next, we analyze each session trace to extract implicit and explicit signals of the orchestrator’s performance. We then compute model scores as weighted, normalized averages of these signals, with greater weight assigned to recent sessions. Finally, we apply a constant offset to all model scores, chosen to maintain stability of the scores over time.
How we measure signals
A signal is a property observable from a trace that is indicative of the orchestrator model’s capability. The signals are derived from a combination of explicit user feedback (e.g. button clicks), implicit user feedback (e.g. user frustration), and automated pipelines designed to grade model outputs directly (e.g. monitoring the reliability of tool use).
- Bash Reliability SourceTool useWhat it measuresThe proportion of shell commands that do not involve an error caused by the model.
- Steerability SourceModel responseWhat it measuresWhen a user corrects the agent ("no, do X instead"), we check the next response. The signal measures whether the model successfully corrected its work, based on subsequent user feedback.
- Praise vs Complaint SourceUser messagesWhat it measuresWe find messages with clear praise ("this is exactly what I needed") or clear complaints ("this is broken"). The signal measures whether praise outnumbers complaints in a given task.
- Task Completion SourceUser clicksWhat it measuresAfter each turn, the user is asked whether the task was successful. Task completion measures the success rate based on the user responses to this question.
Each signal becomes one column of the leaderboard.
Battle leaderboards
In a battle, two anonymous models perform the same task side by side, and the user votes for the better response. The Bradley-Terry model is a statistical model for paired comparisons, closely related to the Elo model used in chess. It models the pairwise win probabilities between two models as a function of their "strengths," and fits these strengths by observing pairwise battles across the pool of models. The model can take additional features into account, which allows us to control for response length, sentiment, and other stylistic features.
- Step 1 · VoteTwo anonymous models perform the same task. The user picks A, B, tie, or both bad.
- Both model identities appear after the vote
- Step 2 · Fit ratingsWe fit the model strengths that give the best approximation of the observed win probabilities between models. The final reported score is , where is an arbitrary anchor whose choice does not affect the rankings. Style control adds response length and formatting to the statistical model, so the rating reflects model quality independent of the style it uses.
- Step 3 · Confidence intervalsWe compute confidence intervals on the model strengths using the central limit theorem. Each score with its 95% confidence interval. The range contains the true model score with probability 95%. Intervals get narrower as more sessions come in. More votes give a narrower interval
Reading the charts
- 01Ranking listEach model’s leaderboard score in rank order.
- 02Confidence intervalsEach score with its 95% confidence interval. The range contains the true model score with probability 95%. Intervals get narrower as more sessions come in.
- 03Battle heatmapOn a battle leaderboard, the number of battles between each pair of models, ties excluded. The scale beside the chart gives the number of battles of each shade.
- 04Pareto frontierEach model’s score against its measured median cost per task, from cheap on the left to expensive on the right. A model under the line is beaten by one that is both cheaper and better.
- 05Category strengthsOne model’s score in each category of Code Arena. Farther from the center is better. The scale starts a little below the lowest score, not at zero.
- 06Leaderboard trendsHow the top models’ scores change over time, grouped by license, region, or lab.
