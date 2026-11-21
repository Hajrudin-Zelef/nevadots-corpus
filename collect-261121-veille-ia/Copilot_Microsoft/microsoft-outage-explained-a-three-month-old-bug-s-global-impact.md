---
id: collect-261121-veille-ia/copilot-microsoft/microsoft-outage-explained-a-three-month-old-bug-s-global-impact
title: "Microsoft outage explained: a three-month-old bug's global impact"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-11-16"]
keywords: ["microsoft", "outage", "servers", "defect", "lamanna", "caused", "explained", "july"]
source: https://mobilesyrup.com/2026/11/16/microsoft-365-outage-explainer/
source_anchor: ""
source_lines: [1, 17]
section: "Copilot_Microsoft"
---
Microsoft has finally explained what caused last week’s massive outage, which disrupted businesses and governments worldwide.

The Microsoft 365 outage, which started on Nov. 12 and continued until the morning of Nov. 14, was caused by a combination of issues, including a defect introduced in a July 2026 patch.

Charles Lamanna, Microsoft’s executive vice president of Copilot, Agents, and Platform, took to Twitter/X to share the details behind the outage. First, Lamanna noted that “a latent defect caused servers to become overwhelmed and unresponsive under certain load conditions.” Notably, Lamanna said the defect was the “result of a fix that was prepared specifically for Microsoft 365 networking gateway servers” and didn’t impact broader Windows deployments.

The defect was introduced in July, but did not cause any issues until now. Moreover, Lamanna said the defect wasn’t detected by Microsoft’s internal tests, which focused on “weaker hardware SKUs with fewer cores.” The defect, however, was “worsened on machines with more cores and newer hardware.”

The second issue Lamanna highlighted was a surge in load caused by a Teams issue that happened roughly 15 hours before the Microsoft 365 outage. Lamanna explained that Microsoft maintains multiple network gateway systems for its services so that if there’s an issue with one, there’s a fallback. The Teams issue caused it to move to the same gateway used for Exchange Online (which powers email services like Outlook), significantly increasing the load on that gateway.

Finally, Lamanna said that “nearly 14% of server capacity for the networking gateway was out of service due to an unrelated issue from the previous day.”

Combined, these issues triggered the July defect, leading requests to build up until servers ultimately stopped responding in the midst of North America’s peak traffic window. Worse, Microsoft’s traffic management systems respond to outages like this by automatically routing traffic to healthy servers. But that caused the July bug to trigger again, resulting in multiple servers failing — at the worst point, over 80 per cent of the company’s servers were unresponsive.

Lamanna explained that recovery took longer than usual because as servers came back online, waiting traffic immediately overwhelmed them again, causing the July defect to trigger and make the servers unresponsive. And since the bug was several months old, Microsoft wasn’t able to quickly revert the change. Instead, the company had to patch the bug, restart the servers in batches, and slowly bring things back online.

Microsoft is expected to publish a final post-incident report on Nov. 20, which will likely include more details about the cause of this outage. Still, the fact that this outage happened is cause for concern, and yet another valid reason for getting off U.S. tech. At the very least, Microsoft should be held accountable for the mess this caused — perhaps a Canadian law firm will do just that.
