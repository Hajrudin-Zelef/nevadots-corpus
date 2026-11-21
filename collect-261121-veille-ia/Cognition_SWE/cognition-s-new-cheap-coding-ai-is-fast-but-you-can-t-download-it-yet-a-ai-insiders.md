---
id: collect-261121-veille-ia/cognition-swe/cognition-s-new-cheap-coding-ai-is-fast-but-you-can-t-download-it-yet-a-ai-insiders
title: "Cognition's new cheap coding AI is fast, but you can't download it yet â AI Insiders"
domain: cognition-swe
role: reference
task: article
actors: ["Cognition"]
dates: ["2026-11-04"]
keywords: ["model", "cognition", "percent", "coding", "work", "swe-2", "november", "stage"]
source: https://syndicate.aiinsiders.net/article/cognitions-new-cheap-coding-ai-is-fast-but-you-cant
source_anchor: ""
source_lines: [1, 17]
section: "Cognition_SWE"
---
Cognition, the company behind the Devin coding agent, released SWE-2 Flash on 4 November. It is a smaller model priced at one-fifth of SWE-2, Cognitionâs flagship, and it generates text three times faster. It is free in Devin Desktop and the Devin command-line tool through 30 November.

The model is built on Inkling-Small, a model from Thinking Machines that has 276 billion parameters in total and 12 billion active at any moment. Cognition says its own training lifted that base model sharply. On Terminal-Bench 4.0, a test of agentic work in a command line, the score went from 0.9 percent to 20.0 percent. On FrontierCode 2.0, it went from 3.2 percent to 24.0 percent. Cognitionâs blog post describes the result as the strongest of any American open-weight model, including some three times larger. Those comparisons are Cognitionâs own, and the post does not link independent runs.

The recipe has two parts. Before the usual reinforcement learning stage, where a model practices tasks and gets scored, Cognition added a mid-training stage for the first time. That stage fed the model a few trillion tokens of scientific articles, blogs, code, and similar material, in three rounds, with the scarcest and best data saved for the end. The team picked its data mix by measuring how many tokens each candidate needed to match the validation loss of a baseline run.

The reinforcement learning stage then did most of the remaining work, and Cognition credits the tasks more than the algorithm. The company says it checked the instructions, the setup, and the answer checkers for every task, and added new task types to cover a far wider range of skills than SWE-2 saw. That is a plain admission that the data and the grading mattered more than any clever training trick.

The release also carries a security story. Cognition aimed some of the training at cyber defense work, and the modelâs score on ExploitBench rose from 11.3 percent to 34.8 percent. The company notes that the same skill helps attackers, citing a recent wave of cyber attacks, so it is withholding the weights from the public. Instead it plans a program to share them with vetted industry partners and research labs, with details promised in a few weeks.

That puts SWE-2 Flash in an awkward spot for a model pitched on transparent access. Governments and large enterprises that want to host a coding model on their own servers, including on-prem, cannot do so today. Researchers who want to study how the model behaves, or how it could be misused, face the same wall until the sharing program opens. Cognition has not said who will qualify or what the terms will be.

The commercial logic is easier to read. Cognition argues that smaller models can now take on a large share of agent work: automations, code review, feature work, and documentation updates. Those jobs run constantly and in volume, so a model at a fifth of the price changes the bill far more than a few extra benchmark points would. A 20 percent score on Terminal-Bench 4.0 is also a reminder of how hard that test is: a model that handles one task in five is useful for routine chores, not for unsupervised engineering.

For teams that spend heavily on coding agents, the free window until 30 November is a cheap way to run SWE-2 Flash against real tickets and measure how much work can move off the expensive model. The price after that date, and the terms of the weights program, are the two numbers that will decide whether it stays in the stack.

*Reported by the Cognition blog on 4 November 2026.*
