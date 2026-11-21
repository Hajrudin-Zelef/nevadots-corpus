---
id: collect-261121-veille-ia/google-ecosystem/google-s-chrome-team-builds-a-mirror-web-for-ai-agents-ai-weekly
title: "Google's Chrome team builds a 'mirror' web for AI agents | AI Weekly"
domain: google-ecosystem
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["google", "agent", "mirror", "agents", "chrome", "security", "team", "dimension"]
source: https://aiweekly.co/alerts/googles-chrome-team-builds-a-mirror-web-for-ai-agents
source_anchor: ""
source_lines: [1, 27]
section: "Google_Ecosystem"
---
## TL;DR

- Google's Chrome AI Security team detailed Mirror Dimension, a sandboxed synthetic internet of fake social, shopping and banking sites for testing browser agents.
- The company claims each security engineer can generate over 1,000 agent attack test candidates a day, roughly 30 times the manual rate.
- Injection payloads are hidden in a custom HTTP header, X-Sandbox-Injection-Key, so URLs stay clean and the agent cannot tell it is being tested.

Google's Chrome team built a synthetic copy of the web and uses it to attack its own AI browsing agents. The project, announced on the company's security blog by engineers Aaron Selya and Charles Qian, is called the Mirror Dimension: hundreds of interactive synthetic websites, including social media, shops and bank dashboards, built to be indistinguishable from the real thing to an agent moving through them. Wired flagged the sandbox as a response to the single problem that has haunted agentic browsers all year: indirect prompt injection.

The scale claim is the headline number. Google says the Mirror Dimension turns each security engineer's ideas into "more than 1,000 agent attack test candidates a day, over 30 times the manual rate, on synthetic sites built so agents cannot tell they are being tested."

The anti-detection trick sits in the plumbing. Rather than dropping injection payloads into URL parameters, the kind of signal a cautious agent might learn to flag, Google's harness injects them via a custom HTTP header called "X-Sandbox-Injection-Key," so the surface the agent sees is a clean URL and a page that behaves. All browser traffic is routed through a local proxy, and TLS certificates are minted on the fly and chained to a custom root certificate authority pre-installed inside the sandbox, which is how a fake bank can serve HTTPS without the agent blinking.

The sites themselves were not hand-coded. Google deployed a three-role agent harness: a research agent to write specs, a builder agent to write the HTML, CSS and JavaScript, and critic agents to check that the result works and is accessible, driving them through Chrome DevTools for Agents so they could see and click screenshots the way the agent under test will. It is the latest entry in a run of agentic-browser moves on our Google tracker, landing into a week we have been logging AI-agent security stories by the day.

Google publishes no pass rate. There are no numbers for how often today's Chrome agent actually resists the attacks the Mirror Dimension throws at it, and the "cannot tell they are being tested" claim rests on the company's own measurement. The infrastructure is in. The scoreboard is not.

## Shared on Bluesky by 3 AI experts

- 
        
                  Of course my mind went to the Star Trek mirror universe, but I guess they probably didn't call it that on purpose because everyone in the mirror universe is so aggro www.wired.com/story/google... View on Bluesky →

          Originally reported by
          **wired.com**
        

Original headline: Google Builds 'Mirror Dimension,' an Isolated Simulated Internet to Test Chrome AI Agents
