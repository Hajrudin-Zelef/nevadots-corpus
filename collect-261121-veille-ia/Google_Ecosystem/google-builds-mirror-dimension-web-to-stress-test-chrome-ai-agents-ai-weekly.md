---
id: collect-261121-veille-ia/google-ecosystem/google-builds-mirror-dimension-web-to-stress-test-chrome-ai-agents-ai-weekly
title: "Google builds 'Mirror Dimension' web to stress-test Chrome AI agents | AI Weekly"
domain: google-ecosystem
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["google", "chrome", "agents", "mirror", "dimension", "sites", "stress-test", "security"]
source: https://aiweekly.co/alerts/google-builds-mirror-dimension-web-to-stress-test-chrome-ai-agents
source_anchor: ""
source_lines: [1, 25]
section: "Google_Ecosystem"
---
## TL;DR

- Google's Chrome AI Security team detailed Mirror Dimension, a synthetic web of social, shopping and banking sites used to stress-test its Gemini-powered browser agents.
- The harness slips prompt-injection payloads through a custom header called X-Sandbox-Injection-Key, so agents see clean URLs and pages that behave normally.
- Google says each engineer can now produce more than 1,000 agent attack test candidates a day, over 30 times the manual rate.

Google's Chrome AI Security team has detailed an isolated copy of the web it calls the 'Mirror Dimension', hundreds of synthetic social media sites, shops, and bank dashboards built so its Gemini-powered browser agents cannot tell they are being tested.

The point is to probe the single failure mode that has shadowed agentic browsers all year: indirect prompt injection. Inside the sandbox, Chrome runs in an isolated gVisor environment, and attack payloads ride in on a custom header called X-Sandbox-Injection-Key, so the surface the agent sees is a clean URL and a page that behaves.

In its write-up, Google says the setup turns each security engineer's ideas into 'more than 1,000 agent attack test candidates a day, over 30 times the manual rate, on synthetic sites built so agents cannot tell they are being tested.' To stand up the mock internet quickly, the team used AI development agents to generate the sites, and had to bypass some of Chrome's own protections, including forging web certificates, so real Chrome could load them. Two of the researchers we follow shared the link within hours of it going up.

Google says it plans to open the environment to other teams across the company. What the post does not publish is the figure most readers will want next: the actual pass-rates Chrome's agents post against those 1,000 daily probes.

## Shared on Bluesky by 3 AI experts

- 
        
                  Of course my mind went to the Star Trek mirror universe, but I guess they probably didn't call it that on purpose because everyone in the mirror universe is so aggro www.wired.com/story/google... View on Bluesky →

          Originally reported by
          **wired.com**
        

Original headline: Google Built a Giant ‘Mirror Dimension’ Internet to Test Its Chrome AI Agents
