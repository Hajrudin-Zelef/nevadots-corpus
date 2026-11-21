---
id: collect-261121-veille-ia/google-ecosystem/google-builds-simulated-internet-to-test-chrome-ai-agents-against-attacks
title: "Google builds simulated internet to test Chrome AI agents against attacks"
domain: google-ecosystem
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["agents", "google", "websites", "test", "simulated", "environment", "internet", "against"]
source: https://cryptobriefing.com/google-mirror-dimension-chrome-ai-agents/
source_anchor: ""
source_lines: [1, 27]
section: "Google_Ecosystem"
---
The isolated testing environment uses AI agents to recreate websites and simulate cyberattacks without exposing real users or services.

Google has developed a synthetic version of the internet called “mirror dimension” to test Chrome’s AI browsing agents in realistic environments without exposing users or live websites to security risks.

The system recreates thousands of simulated websites where AI agents can navigate pages, complete transactions, and interact with online services while engineers evaluate their behavior and defenses against malicious instructions, according to a Google report published Thursday.

The testing environment supports Chrome’s auto browse feature, which allows AI agents to perform tasks such as navigating websites, completing forms, and interacting with online services on behalf of users.

Rather than connecting agents to the public internet, Google runs them inside an isolated sandbox that intercepts web requests and redirects them to synthetic websites. The system preserves familiar website addresses, preventing agents from recognizing that they are operating in a simulated environment.

Google also uses a team of specialized AI agents to generate and maintain the simulated websites. Research agents define site requirements, builder agents create functional pages, and critic agents inspect the results for errors, accessibility issues, and broken interactions.

### AI, tech, and the markets they move—in one daily briefing.

Daily. Free. Join 34,000+ readers across crypto, finance, and policy.

The process relies on Chrome DevTools for Agents, allowing the AI systems to inspect rendered pages, execute clicks, enter text, and evaluate website functionality through real browser interactions.

The simulated environment also allows Google to test defenses against prompt injection attacks, in which malicious instructions embedded in websites attempt to manipulate AI agents into taking unauthorized actions.

Engineers can insert attacks into website text, images, accessibility labels, and tool responses while keeping the websites’ visible addresses unchanged. This allows Google to evaluate how agents respond to malicious content across different browsing scenarios.

Google said the automated testing pipeline generates more than 1,000 security test candidates per day for each security engineer, more than 30 times the number an engineer could manually create and test.

The company plans to expand the testing environment to support more advanced coding agents and additional browsing capabilities as AI systems become increasingly capable of completing complex online tasks.

**Disclosure:** This article was edited by Diego Almada Lopez. For more information on how we create and review content, see our Editorial Policy.
