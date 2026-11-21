---
id: collect-261121-veille-ia/google-ecosystem/google-builds-isolated-synthetic-web-to-test-chrome-ai-agents
title: "Google Builds Isolated Synthetic Web to Test Chrome AI Agents"
domain: google-ecosystem
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["agents", "google", "chrome", "agent", "test", "isolated", "mirror", "dimension"]
source: https://vectorwire.ai/article/google-builds-isolated-synthetic-web-to-stress-test-chrome-ai-agents-20353f
source_anchor: ""
source_lines: [1, 17]
section: "Google_Ecosystem"
---
Google's Chrome team built an expansive, fully isolated replica of the internet — internally called the "Mirror Dimension" — to train and evaluate the AI browsing agents that power its auto browse feature, the company disclosed on November 19<sup>2,3</sup>.

The environment is a controlled web ecosystem comprising thousands of simulated websites modeled on everyday user journeys, including social media platforms, shopping sites, and bank dashboards, all constructed with the components and attributes of real websites. Agents operating inside it must navigate pages, click through subpages, scroll, and type into input fields in Chrome exactly as they would on the live internet.

## Why a parallel web

The project addresses a core tension in agent development. Training browsing agents in limited test environments is "only so effective," Wired reported, because models can develop "eval awareness" — detecting that they are in a simulation and altering their behavior accordingly. Releasing unrefined agents onto the open internet, meanwhile, risks real-world harm. The Mirror Dimension splits the difference: a realistic but sandboxed web where engineers can observe agent behavior without external consequences.

The Chrome team used Gemini models to power the browsing features under test. To make the sandbox credible to agents running in real Chrome, engineers had to forge web certificates for validity checks so the browser's standard security pipeline would function inside the controlled environment. A purpose-built AI harness gathered website and web-app development agents to generate realistic web services and data flows across the simulated sites.

## Security gauntlet

Beyond functional testing, the Mirror Dimension subjects agents to adversarial resilience checks, including hidden prompt-injection attacks designed to probe their defenses. Google's researchers said the browsing assistant agents they have been testing are benign and do not possess cyber capabilities. Google's goal is to make the Mirror Dimension available to teams across the company for safe, controlled agent testing.

The disclosure arrives the same day Google extended Gemini's computer-use agent capabilities to the macOS desktop app, enabling the AI to click, type, and navigate local applications and the web on a user's behalf<sup>[2]</sup>.

ANALYSIS Taken together, the two moves outline a dual-track approach: expanding what Gemini agents can do on real machines while building infrastructure to validate agent safety before deployment.
