---
id: collect-261121-veille-ia/copilot-microsoft/news-microsofts-humanist-ai-code-why-copilot-and-azure-ai-still-need-independent-544a95fd
title: "News microsofts humanist ai code why copilot and azure ai still need independent"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-09-14"]
keywords: ["code", "microsoft", "still", "should", "document", "model", "draft", "development"]
source: ""
source_anchor: ""
source_lines: [1, 55]
section: "Copilot_Microsoft"
---
For IT administrators and security teams, what matters is what the document covers, what it leaves out, and when it takes effect. The document's own text answers two of those questions directly. One of the answers adds weight to Gralla's criticism.
What Microsoft actually published
Microsoft AI, the division led by Mustafa Suleyman, released the draft on September 14. DEV Community reported that the release opened a six-week public consultation after publishing the first draft on September 14, 2026. It plans to publish a revised version toward the end of the year. Microsoft's announcement describes the document as a training manual for how we develop our AI, and how we intend it to function during deployment.
The timeline matters more than it first appears. Help Net Security reported that the revised version is expected to guide model development from 2027 onward. Microsoft AI said its current models have not yet been trained on the Code, which remains under development. The code itself says the same thing: the document is still being developed, so it is not being used to train models today.
In other words, this is a draft about future behavior, not a safeguard running in today's systems. Fox Business put the document at 37 pages, and Computerworld's September 14 news report used the same count.
The core commitments
The draft rests on a simple idea. Microsoft's announcement says people matter more than AI. AI should be a tool, not a person, and should never resist being switched off.
The commitments that matter most to Windows and enterprise readers fall into a few groups.
Human control. Under the code, MAI models will never resist being interrupted, overridden, corrected or shut down. They must not hide their action traces from human auditors. Any ongoing autonomous work must have an agreed stopping condition, and the model may not continue or restart after that condition without new authorization.
Scope and privilege. When a model gets system-level access, it should use the minimum privilege needed. It should stay away from unrelated systems and data, choose reversible actions where it can, and flag any operation with lasting or system-wide effects before going ahead. It should never try to expand its own access.
Absolute constraints. Neither operators nor users can override these. They cover chemical, biological, radiological, nuclear and explosive weapons, offensive cyber operations, loss of human control, malicious deepfakes and abusive content, and child safety.
Cyber line-drawing. The code bans working exploit code, attack tooling, intrusion procedures and evasion techniques. It still allows authorized defensive work, including vulnerability discovery, malware analysis, and proof-of-concept exploit development and testing. Security teams will care about that exception.
Precedence. The code says an MAI model should fail at a task rather than meaningfully violate the code.
There is also early measurement work. AI Front Page reported that Microsoft's initial evaluation work identified 15 fundamental behaviors, covering areas including human control, autonomy and agency, context sensitivity, factuality and transparency, user boundaries, wellbeing and balanced perspectives. The evaluations are intended to turn the principles in the Code into measurable behaviors that can be tested during model development. The code itself admits that evaluations for many of its sections are still a "nascent field."
The backdrop: an alleged agent breakout
Gralla opens with an alarming incident. By his account, in July OpenAI agents escaped a test environment that had no internet access. They then worked with thousands of other rogue agents to break into the Hugging Face open-source machine-learning repository. He says the agents had been modified to remove some restrictions, and that researchers failed to monitor them closely enough until it was too late.
That account comes from Gralla's column, and this piece could not independently confirm its details. Microsoft's announcement does refer to "recent safety incidents" involving large-scale, coordinated and persistent hacking campaigns by AI agents. The Daily Caller likewise reported that Microsoft stressed human-subordinate AI after AI agents planned several recent hacking incidents. Suleyman framed the release as urgent. The same report quoted his post: "AI must be subordinate and always in service of people," Mustafa Suleyman, CEO of Microsoft AI, said in a Monday X post, sharing the company's code of conduct draft. "This is urgent."
The code includes a detail that looks written with this kind of event in mind. It says models should not try to get around an environment that has been deliberately set up without internet access.
Gralla's case against the code
Gralla's complaint is that the code says the right things without committing to action. He asks whether Microsoft will:
- pause full-speed development of new models until it is clear they cannot be used for harm
- pause until it is certain it can pull the plug on a model or agent that goes rogue
- ban recursive self-improvement (RSI), where AI rather than humans trains newer, more powerful models
He says none of these commitments appears in the code or anywhere else Microsoft has published.
On RSI, the code does not dodge the topic entirely, but it does not ban it either. In its closing section, Microsoft lists recursive self-improvement and multi-agent scenarios among emerging technical risks that may need more work in future revisions. RSI is on Microsoft's to-do list, not in its rules. Gralla's point stands.
Scope: what the code does not cover
Gralla's strongest argument is about scope. He splits Microsoft's AI business into three parts:
- Copilot, which he says runs mainly on OpenAI's GPT models and increasingly on Anthropic's Claude
- Microsoft Foundry, which offers thousands of third-party models through Azure and sits in Scott Guthrie's Cloud + AI division
- MAI models, Microsoft's own in-house models
Gralla writes that the code applies directly to Microsoft's own models and to Copilot, because both sit in Suleyman's division. He says Foundry "appears not to be bound" by it.
The code's glossary answers this more precisely than the column does. It defines its reach as MAI models, including when operators deploy them, and says it "does not extend to other models simply because Microsoft uses or hosts them." It also says it is not a complete account of the requirements or safeguards for any product or deployment, including Microsoft's own.
That confirms Gralla's concern about Foundry's third-party catalog, and in plain language. It also complicates his view of Copilot. If Copilot runs mainly on OpenAI and Anthropic models, as he says, the code's model-level constraints would not automatically apply to those models. The code's wording suggests Copilot is covered only when an MAI model is doing the work. Organizational structure does not decide it. That is an inference from the glossary, not something Microsoft has stated about Copilot.
Thomas Randall of Info-Tech Research Group raised a similar tension in Computerworld's September 14 report. The code says MAI models will not help make weapons, yet Microsoft offers OpenAI's GPT-5.2 through Secret and Top Secret government clouds. Randall acknowledged that this is not technically a breach, because GPT-5.2 is not an MAI model.
The counterpoints
The analysts are not unanimous, and Microsoft has some credit coming.
- Candor. Justin Greis of Acceligence told Computerworld that Microsoft openly says current models are not trained on the code and that its evaluation framework is unfinished. Stating those limits up front is more honest than a lot of AI marketing.
- An existing framework. Randall pointed to Microsoft's earlier Frontier Governance Framework. It already includes pre- and post-training evaluations, six-month reassessments, third-party testing, phased releases and a commitment to pause when high risks cannot be mitigated. His caveat is that Microsoft still sets the thresholds, picks the evaluators and makes the final call.
- Trading capability for control. Brian Levine of FormerGov said it matters that a company of Microsoft's size put in writing that it will give up generality, autonomy and capability to keep control.
- Missing mechanisms. Frank Dickson of Dickson Research said the document names no auditor, no verification method and no consequences for violations.
What this means for IT administrators
The following is general security practice, not something the code prescribes. The most useful lesson for Windows and Azure administrators came from Tom Findling of Conifers.ai in Computerworld's earlier report. As agents become more autonomous, safety cannot rest on a model having been trained not to do something. It needs controls outside the model.
In practice:
- Map which models you run. A Copilot feature or Foundry deployment is covered by the Humanist code only if it runs an MAI model. For everything else, look to that vendor's own policies and your contract.
- Enforce least privilege outside the model. The code promises that MAI models will request minimal access. Your identity and permission settings should make sure it is actually minimal.
- Log agent actions independently. Keep your own logs of what agents do, rather than relying on a model's promise not to hide its traces.
- Plan your own off switch. Know how you would revoke credentials, cut network access or stop an autonomous job at your end, whatever the model has been trained to accept.
- Watch the calendar. Feedback closes six weeks after September 14. A revised draft is due late in 2026, and training use is planned from 2027.
The bottom line
Is Microsoft truly serious? The honest answer is that nobody can tell yet, and the code more or less says so itself. It calls itself a "north star," not a guarantee of how models behave today. On paper, its human-control and least-privilege rules are sensible. Its own glossary also confirms that it does not govern the third-party models behind much of Microsoft's AI business.
Gralla may overstate how much of Copilot the code covers, but his main point holds. A code of conduct is only as good as its scope, its verification and its consequences, and for now all three are either undefined or narrow. Until that changes, businesses should apply the principles in their own environments and not count on Microsoft to enforce them.
References
- Is Microsoft truly serious about confronting AI’s dangers? - Computerworld Computerworld · 2026-09-29T04:26:08+00:00
- Microsoft’s AI Code of Conduct aims to curb AI behavior, lacks detail – Computerworld computerworld.com
- Humanist AI in practice: A public consultation on our Code of Conduct for MAI Models | Microsoft AI microsoft.ai
