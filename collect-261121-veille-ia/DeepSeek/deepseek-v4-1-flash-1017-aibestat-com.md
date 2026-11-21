---
id: collect-261121-veille-ia/deepseek/deepseek-v4-1-flash-1017-aibestat-com
title: "DeepSeek-V4.1-Flash-1017 — aibestat.com"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: ["2026-10-17"]
keywords: ["deepseek-v4", "flash-1017", "version", "deepseek", "tokens", "flash", "scores", "model"]
source: https://aibestat.com/articles/deepseek-v4-1-flash-1017-coding/
source_anchor: ""
source_lines: [1, 13]
section: "DeepSeek"
---
Picture two tradespeople. One is a master who solves the hardest job in the building, but you pay by the hour and the meter runs fast. The other is nearly as handy on ordinary work, finishes sooner, talks less, and charges far less. For a repair queue with a thousand tickets, you hire the second one for most of them and keep the master on speed dial. **DeepSeek-V4.1-Flash-1017 is the second tradesperson.**

On October 17, 2026 DeepSeek swapped this version into its website, app and API under the same `deepseek-flash` name, with prices untouched. The standard rate is $0.15 per million input tokens and $0.60 per million output tokens; during weekday peak windows (01:00–04:00 and 06:00–10:00 UTC) it doubles to $0.30 and $1.20, so the practical range is $0.15–$0.30 in and $0.60–$1.20 out. The company calls it a minor update to September’s V4.1 Flash. The changes it lists are better reasoning, coding and visual scores, plus shorter outputs and faster tasks. Cheaper cost per task comes from that last pair. A model that reaches the same fix in fewer tokens is cheaper without any change to the price list.

Here is what that looks like in DeepSeek’s numbers. On DeepSWE v1.1, a test where the model must repair real software projects, it reports 77.0, up from 74.2. It lists Claude Opus 5 at 74.0 and GPT-5.6 Sol at 73.0. On the much harder Terminal-Bench 4.0 it reports 42.3, up from 31.2 and above the 37.9 it lists for GLM-5.3, yet far behind Opus 5’s 51.8. Its cost chart puts the top setting at about 77% success for roughly twenty cents a task; the chart does not say whether that assumes standard or peak rates, so assume the lower figure could roughly double in peak hours. Every one of those figures is DeepSeek’s, produced with harnesses DeepSeek selected, and the Terminal-Bench run used the Claude Code harness while DeepSWE used a different one. A good reader treats the table as a strong claim to confirm, not as a verdict.

The independent record is thinner. Artificial Analysis has scored the September V4.1 Flash, which has 552 billion total parameters and about 16 billion active per token: 39 on its Intelligence Index, version 4.3.2. On that same version GLM-5.3-Flash scores 42. So on broad intelligence, measured by a third party, DeepSeek’s Flash trailed its rival before this update. DeepSeek’s update targets agent work, where its own tables show the largest gains, and that is where independent testing should look next. (Earlier GLM-5.3 reviews on this site quote a higher index figure from an older version of the scale; scores from different versions cannot be compared.)

The model also sees. It accepts images, which lets a front-end agent compare a rendered page with the intended design instead of reasoning from code alone, and it offers a 1M-token context with up to 384K tokens of output. Weights are on Hugging Face under the MIT license. The checkpoint is roughly 763 billion parameters in storage, though, so self-hosting is a datacenter decision rather than a laptop one. That is why we cover it for coding as a hosted API and review its private-hosting side in Local / Private AI.

Two practical notes. DeepSeek recommends its own DeepSeek Harness, version 0.2.1-rc.1 or later, and says it also tuned for Tencent’s CodeBuddy and WorkBuddy, so results in other agent frameworks may differ. And its new Search and Fetch APIs, which cost $0.75 per thousand calls, are free through certain server-side tools only until October 31, 2026, so budget for them afterwards.

The sensible plan is a division of labor. Let DeepSeek-V4.1-Flash-1017 work through the long queue of routine repairs, run the tests, and retry cheaply. Send the few problems that decide a release, such as deep terminal sessions and tangled migrations, to a stronger model. Then check the independent scores once they exist, because they will tell us whether this price-to-performance claim survives contact with someone else’s test.
