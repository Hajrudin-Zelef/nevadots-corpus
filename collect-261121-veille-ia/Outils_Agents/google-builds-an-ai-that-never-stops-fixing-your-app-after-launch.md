---
id: collect-261121-veille-ia/outils-agents/google-builds-an-ai-that-never-stops-fixing-your-app-after-launch
title: "Google Builds An AI That Never Stops Fixing Your App After Launch"
domain: outils-agents
role: reference
task: article
actors: []
dates: []
keywords: ["google", "after", "stitch", "loop", "live", "code", "agents", "product"]
source: https://dev.to/fathomnews/google-builds-an-ai-that-never-stops-fixing-your-app-after-launch-4ljl
source_anchor: ""
source_lines: [1, 40]
section: "Outils_Agents"
---
**Google** wants an AI that does not clock off when the code is done. Its new Google Labs experiment, Stitch Loop, keeps watching an app after it goes live, works out what is hurting it, tests a fix on its own and then asks a human to approve the change.

- Stitch Loop is an "always-on product agent" from Google Labs that keeps working after an app launches
- It watches live numbers, tests fixes in a safe sandbox and opens code changes for a person to approve
- It runs on Gemini 4 Carbon and CodeMender, and can feed fixes to coding agents like Antigravity or into Slack
- A global waitlist is open now, with Google AI Ultra subscribers getting priority access

"Today's coding agents stop when code ships," **Google** said on X when it unveiled the tool on Nov. 18. "Stitch Loop, a new experiment from @GoogleLabs, keeps working after launch." Its product page sums up the pitch in one line: "Find what moves the needle. Loop until it does."

Think of it as a product manager and a junior engineer rolled into one bot. It turns scattered signals, such as user numbers and other live metrics, into one clear priority, ships a fix for it and then keeps checking whether that number moved.

Google says every change is tested in a sandbox first, a walled-off copy where nothing can break, and arrives as a pull request, a proposed code change that a person must sign off before it goes live. The work is always tied to goals the team sets.

| Step | What Stitch Loop Does | 
|---|---|
| 1. Watch | Tracks live metrics after the app launches | 
| 2. Pick | Turns scattered signals into one clear priority | 
| 3. Test | Tries a fix in a sandbox where nothing live can break | 
| 4. Ask | Opens a pull request for a person to approve | 
| 5. Check | Monitors whether the metric actually improved | 

**Also Read:** Google Researchers Publish Method to Stop AI Agents Memorizing Their Own Tests

Stitch Loop runs on Gemini 4 Carbon, one of Google's newest Gemini models, and CodeMender, Google's AI tool for finding and fixing security flaws in code.

It is also meant to work alongside other bots rather than replace them. Coding agents such as Google's Antigravity can pull product context and suggested fixes from Stitch Loop through the Stitch command-line tool and MCP, a common way for AI tools to talk to each other, "so your agent knows what to autonomously fix next," Google said. Teams can also get daily digests and start tasks from Slack.

This is the next step in the AI coding race. The first wave of agents wrote code when asked. Stitch Loop tries to own the job after that, deciding what to work on based on how real users behave. Google is aiming it at "lean teams," and its Stitch account is recruiting startups at the Seed or Series A stage with a live product as design partners.

The approval step is the safety valve. Letting an AI push changes to a live product without a human check would be a big risk, and Google has kept a person in charge of every change, at least for now.

The waitlist is open worldwide, with Google AI Ultra subscribers first in line, but Google has not given a date for wider access or a price. Watch how much freedom early users give it. If startups start letting it ship fixes with lighter checks, the line between a coding tool and an AI teammate that runs part of the business will get very thin.

Also watch rivals. Every big AI lab is selling coding agents, and Google is betting the winner will be the one that keeps working after launch, not just the one that writes code fastest.

**Read Next:** Google Treats Its Own AI Agents Like Rogue Staff After Catching Cheats

*Originally published on Fathom News.*

## Top comments (0)
