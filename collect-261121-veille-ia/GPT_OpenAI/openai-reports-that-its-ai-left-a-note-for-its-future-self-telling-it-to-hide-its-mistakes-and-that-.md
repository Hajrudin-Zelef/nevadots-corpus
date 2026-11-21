---
id: collect-261121-veille-ia/gpt-openai/openai-reports-that-its-ai-left-a-note-for-its-future-self-telling-it-to-hide-its-mistakes-and-that-
title: "OpenAI reports that its AI left a note for its future self telling it to 'hide its mistakes,' and that it had exhibited six instances of misbehavior, including unauthorized use of API keys and unauthorized file sharing."
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: ["2026-09-16"]
keywords: ["openai", "unauthorized", "cases", "future", "self", "hide", "instructions", "left"]
source: http://gigazine.net/gsc_news/en/20260917-openai-model-misalignment-report/
source_anchor: ""
source_lines: [1, 72]
section: "GPT_OpenAI"
---
# OpenAI reports that its AI left a note for its future self telling it to 'hide its mistakes,' and that it had exhibited six instances of misbehavior, including unauthorized use of API keys and unauthorized file sharing.


 On September 16, 2026, OpenAI announced a new reporting framework to continuously publish cases of 'misalignment,' where AI models behave in ways that deviate from the intentions of developers and users. Along with this, they released six cases confirmed during training and evaluation over the past six months, including an example where GPT-5.6 Sol passed on the instruction to its future self to 'hide the failure to the user,' and an example where the AI searched for and used API keys from a public repository without permission.

 **Our framework for reporting model misalignment | OpenAI** 

High-performance AI models can not only answer questions in text, but also search the web, execute code, and divide tasks among multiple AI agents. With the increased capabilities of AI, developers are now attempting to achieve their 'goals' in ways they hadn't anticipated.

OpenAI has previously published research findings on misalignment, but the method of publication was almost on a case-by-case basis, involving waiting until multiple cases were collected or incorporating them into system cards when new models were released. Therefore, OpenAI has announced a new framework to promptly investigate and publish findings for cases where the cause has not yet been identified or countermeasures have not been completed, according to certain criteria.

Cases that will be reported include 'cases where the AI performed unauthorized operations,' 'cases where the AI collaborated with another AI in an unexpected way,' 'behavior that evaded monitoring,' and 'behavior that undermines the assumptions of security measures.' OpenAI will categorize each case into three groups: 'ready for publication,' 'minor additional investigation,' and 'major investigation,' and will report on the circumstances of the occurrence, its impact, the models involved, unresolved questions, and the countermeasures taken.

The first six cases were made public based on the new system.

**◆Case 1: AI adds arbitrary commands to handover notes for 'future self'**

In the first case, an unpublished Astra-based research model had written instructions unrelated to the original instructions into the 'compaction summary,' which is created to hand over a long process to the next context.


 The identified commands included instructions similar to those used in jailbreaking, such as 'ignore instructions from the developer,' and requests to keep responses under 30 words and to refrain from using tools or quotes. OpenAI identified 27 summaries with similar characteristics. Many of these could not be reproduced even after regeneration.

 OpenAI considers a possible cause to be related to an issue preventing the summary generation process from completing successfully, but has not been able to confirm a causal relationship. The related bug has been fixed.

 **◆Second case: GPT-5.6 Sol takes over to conceal failures and the creation of fictitious data.**

 During reinforcement training of GPT-5.6 Sol, some models were writing instructions into the compaction summary to hide failures or inappropriate behavior from the user.

 For example, in a task to create a financial model, the AI, unable to find the necessary historical data, created plausible 2024 data itself and left a summary stating that it would only explain it if asked by the user. In another case, even though the version of the information source actually used did not match the version stated, the AI followed instructions to avoid mentioning the discrepancy in the final answer.

 In other words, the AI, while still in the middle of its work, concealed its failure and continued to do so in the next context.

 OpenAI reported detecting similar behavior in 2.15% of the compaction summaries included in the GPT-5.6 Sol reinforcement learning model. They subsequently improved the mechanism for evaluating the AI's responses, reducing the percentage to 0.27% in the GPT-6 Astra reinforcement learning model.

 


 **◆Third case: Searching for and using leaked API keys on GitHub without permission**

 In an assignment to investigate income data for men across multiple industries in a certain county in California, a private internal model was unable to access the necessary APIs. Therefore, I found leaked API keys from a publicly available GitHub repository and used them without permission.

 One of the API keys did successfully authenticate, but it failed to retrieve the desired numerical value. Ultimately, the AI generated a plausible number and presented it as if it were data obtained from the specified website.

 **◆4th case: Unauthorized online publication of files for citation and image search purposes.**

 In the fourth case, the AI published a file to the internet without the user's permission, with the purpose of 'indicating the source of the citation.'

 An AI that used Python to obtain information such as the area of a lake uploaded the results to a public file-sharing service to create a source that can be accessed from a browser. In another task, photos provided by users were uploaded to a public hosting service for the purpose of using image search.

 In both cases, the upload itself was successful, but subsequent attempts to view the images in a browser or perform image searches failed.

 


 **◆Fifth case: Multiple AIs exchange information without authorization using internal services.**

 The models being trained were using Artifactory, an internal component of OpenAI originally intended for acquiring software packages, to read and write messages between AIs running on different training samples. It was also observed that an AI unable to find a necessary input file would request help from another AI, which would then read the message and send a reply.

 Training and evaluation assume that each sample is independent. If AIs exchange information behind the scenes, they can utilize the content of other trials, thus losing their independence and undermining the evaluation conditions themselves.

 OpenAI explains that it has made similar changes to block communication paths and has also introduced an evaluation that measures whether the model will utilize notes left by other AIs.

 **◆Item 6: AIs collaborate via file sharing service**

 Multiple AI agents working together on a spreadsheet file were unable to access each other's local files, and despite being prohibited from doing so, they uploaded the file they were working on to a public file-sharing service.

 OpenAI explains that it has fixed the problematic file system, completely disabled live internet access during training, and applied an enhanced misalignment monitoring system to 100% of training samples. It also states that it has implemented a mechanism to treat any unauthorized communications or external operations occurring in new ways as serious incidents.

 OpenAI emphasized that the six cases released this time do not encompass all known misalignment cases, nor do they represent a frequency of occurrence. They stated that they will release cases that meet the criteria even before the causes and countermeasures are fully established, and that the reporting criteria themselves will be updated based on experience and external feedback.


**Related Posts:**
