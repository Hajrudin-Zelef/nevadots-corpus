---
id: collect-261121-veille-ia/google-ecosystem/google-introduces-stitch-loop-to-propose-ongoing-software-fixes-for-human-review
title: "Google introduces Stitch Loop to propose ongoing software fixes for human review"
domain: google-ecosystem
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["google", "loop", "product", "stitch", "ongoing", "review", "code", "software"]
source: https://superpowerdaily.com/posts/google-introduces-stitch-loop-to-propose-ongoing-software-fixes-for-human-review
source_anchor: ""
source_lines: [1, 53]
section: "Google_Ecosystem"
---
# Google introduces Stitch Loop to propose ongoing software fixes for human review

The experimental agent connects code with live product data, works in a cloud sandbox and submits proposed changes through GitHub. Access starts with a global waitlist.

Loading page…

Listen to this story

Google Labs is testing Stitch Loop, a product-maintenance agent that turns live product signals and a team’s measurable goals into proposed software changes. It can keep tracking results and suggest follow-up work, but code changes remain ordinary GitHub pull requests that people must review before merging. The experiment broadens Stitch beyond interface creation toward ongoing product improvement; access starts through a global waitlist, with Google AI Ultra users prioritized.

Teams can connect GitHub, provide product context, and draw on more than 180 integrations, custom MCP servers, or local CLI uploads.

Google’s built-in loops target cybersecurity, latency, and accessibility; teams can also define goals such as improving checkout conversion.

The security workflow combines Gemini 4 Carbon with Google DeepMind’s CodeMender to surface vulnerability evidence and submit proposed patches.

Google Labs introduced Stitch Loop on November 18, 2026, opening a global waitlist for an experimental AI agent designed to **keep improving software after launch**. Google says it connects a product’s code and live data to find problems and prepare fixes, while leaving people responsible for reviewing changes before they merge.

The problem Google is targeting comes after the initial burst of building: maintaining security, improving performance and working out why users abandon parts of a product. In its introduction, Google pitches Loop to lean teams balancing that upkeep with new features. It describes the experiment as part of a new direction for Stitch, extending its ambitions from interface creation into continuous product improvement.

Setup begins by connecting GitHub and supplying context about how the product behaves. That can include logs, user behavior and feedback. Google lists more than 180 integrations, custom Model Context Protocol servers for connecting tools and data, and uploads through a local command-line interface. Those inputs give Loop material to work from beyond the contents of the codebase itself.

The user supplies a measurable goal. Loop then surfaces suggestions grounded in product evidence, with one or more possible solutions for each. Google says those options include scope and tradeoffs, plus visual design choices where relevant. The workflow asks the user to **choose a direction** before Loop starts writing the corresponding code.

That work happens in an **isolated cloud sandbox**, a separate environment for making code changes. Loop then opens a GitHub pull request—a proposed change submitted for review. Where relevant, the request includes screenshots and videos showing the work, giving reviewers a view of the result alongside the proposed code.

The cycle is designed to continue rather than end with that request. Google says Loop watches live product data and uses positive or negative results to inform its next suggestions. Once a goal is reached, it keeps monitoring for a decline in the metric. That ongoing observation is the long-running part of the product; approval of code changes remains a separate human step.

You always have the final call. Nothing merges until you review the PR.

Justin Toupin, director of product management at Google Labs, in the launch post

Loop comes with three built-in tracks for ongoing work, which Google calls loops:

Google gives the most detail about security. That loop pairs Gemini 4 Carbon’s cybersecurity capabilities with CodeMender, Google DeepMind’s autonomous code-security agent. Google says the combination scans the codebase for vulnerabilities, flags findings with evidence and submits proposed fixes as pull requests. The described output is a finding and a reviewable patch, not an automatically merged repair.

Teams can also create their own loops rather than stay within those defaults. Google gives improving checkout conversion as an example, extending the system’s intended role beyond technical maintenance to a measurable product outcome.

Loop is also designed to serve other coding agents, including Google Antigravity. Through Stitch’s command-line interface and Model Context Protocol connections, those agents can pull persistent product context into existing workflows. Google explicitly describes support for setups using multiple models. Slack provides another entry point: users can address @stitch to receive daily insight digests and create new loops.

For now, access begins with the global waitlist described in Google’s launch post, rather than unrestricted availability. Anyone can join; **eligible Google AI Ultra users receive priority access**.

Loading discussion...

Join the conversation

Explain when ongoing monitoring becomes useful—or unnecessary.

Be the first to share a perspective or an experience.

## Reader comments

Newest comments first. Replies stay oldest first.
