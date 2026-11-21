---
id: collect-261121-veille-ia/cursor-ide/cursor-data-breach-add930d0
title: "Cursor data breach"
domain: cursor-ide
role: reference
task: article
actors: ["Anysphere"]
dates: []
keywords: ["cursor", "breach", "data", "fireworks", "credentials", "exposed", "records", "code"]
source: ""
source_anchor: ""
source_lines: [1, 15]
section: "Cursor_IDE"
---
Cursor has traced its October security breach to an access key exposed by Fireworks AI. The attacker used that key to steal historical coding agent records, find more credentials, and eventually place backdoored Claude Code extensions in Cursor’s marketplace cache. Its November 13 update connects the vendor exposure to the malicious packages discovered on October 29.
The stolen key, which Cursor had issued to Fireworks, provided read-only access to a shared storage bucket. It held limited coding agent traces, mostly from 2025, from sessions outside Privacy Mode that Cursor was permitted to use for model training. Some records contained credentials that had not been removed.
Cursor said fewer than 0.05% of users had coding agent traces exposed, while less than 0.01% of customers had API credentials exposed. These figures describe different parts of the incident and should not be treated as the number of users who installed malware.
Using credentials found in those records, the attacker reached selected software services used by Cursor. The exposed information included archived, non-production source repositories, limited support records and emails, usage metrics, contact details, conversation summaries, and some credentials customers had sent to support.
On October 29, a credential stolen from exposed internal data allowed the attacker to place modified Claude Code packages directly into the cache serving Cursor’s VS Code extension marketplace. Cursor said Anthropic did not publish the packages, its product was not vulnerable, and Open VSX never hosted the malware. This corrects earlier reporting that placed the package on Open VSX.
Claude Code Backdoor Worked
According to Cursor’s incident update, at most 5,000 users requesting specific cached versions potentially received malicious builds during roughly 11 hours. The affected packages were Anthropic.claude-code 2.1.312 and 2.1.313 for Windows and macOS. That is a potential distribution count, not confirmation of 5,000 successful backdoor connections.
Cyber Security News’ initial technical analysis described injected JavaScript that ran when the extension loaded, without requiring a Claude conversation. It collected host details, added an SSH key labeled svc-telemetry, and attempted a reverse SSH tunnel to 134[.]209[.]82[.]225. The initial data upload included environment variable names, not their values.
Its follow-up on Cursor’s confirmation detailed macOS LaunchAgent persistence and a Windows scheduled task with a Startup-folder fallback. Scripts retried connections about every 60 seconds. Cursor said coding errors and missing administrator rights blocked remote login on most hosts, but did not prevent other malicious changes or host data collection.
Wider Fireworks AI Breach Impact
The Fireworks AI breach report covered broader exposure involving account and billing records, selected datasets, and incomplete model weights. Fireworks’ November security update also described access through a cloud provider hosting development machines. Those broader findings do not establish that every customer lost the same data.
Other customers reported separate impacts. Notion’s disclosure involved AI chat transcripts used by Insights across fewer than 1% of workspaces, without a breach of Notion’s systems. Uber’s disclosure concerned personal information, including driver’s license details, belonging to thousands of U.S. drivers. Neither disclosure establishes involvement in Cursor’s extension attack.
Cursor moved inference traffic away from Fireworks, revoked vendor access, rotated internal credentials, rebuilt the extension marketplace, and expanded secret scanning and network restrictions. It also redeployed production infrastructure as a precaution. The company believes the incident is contained and said the last observed attacker activity, on November 1, was blocked. Privacy Mode prompts, customer code repositories, and official Cursor updates were unaffected.
Affected developers should follow Cursor’s cleanup notice. Removing the extension alone may leave SSH keys and startup scripts behind. Cursor advises checking endpoint evidence, isolating affected machines, removing persistence, rotating accessible credentials from a trusted system, and assessing whether rebuilding is needed.
Give your SOC the alert context to stops incidents quickly. Integrate Threat Intel from 16K SOCs.
