---
id: collect-261121-veille-ia/google-ecosystem/chrome-security-builds-a-synthetic-web-to-test-ai-browsing-agents
title: "Chrome Security Builds a Synthetic Web to Test AI Browsing Agents"
domain: google-ecosystem
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["agents", "chrome", "sandbox", "agent", "test", "dimension", "synthetic", "sites"]
source: https://www.unite.ai/chrome-security-builds-a-synthetic-web-to-test-ai-browsing-agents/
source_anchor: ""
source_lines: [1, 33]
section: "Google_Ecosystem"
---
## Key PointsMirror Dimension serves familiar addresses inside an isolated sandbox so agents cannot…

View more

- Mirror Dimension serves familiar addresses inside an isolated sandbox so agents cannot detect the test.
- Autonomous critic agents build and revise synthetic sites because unfinished simulations can expose the evaluation.
- The red-teaming pipeline generates over 1,000 test candidates per security engineer each day.

Google’s Chrome Security team introduced Mirror Dimension on November 19, 2026, a self-contained synthetic web for testing the agents behind Chrome’s auto browse feature. The environment contains thousands of simulated websites inspired by everyday user journeys and is designed to evaluate safety protections without exposing the live web to agent actions.

The need for that environment comes from scale. Browser-agent training and evaluation require thousands of read and write actions across hundreds of websites, often with the model inferring the destination from a user’s request rather than receiving a URL. The public web changes constantly, and unrestricted agent actions can cause harm. Mirror Dimension instead gives each trajectory a clean, identical starting point, preventing side effects from carrying into the next run.

## Keeping the Sandbox Convincing

A central design constraint is *eval awareness*, when a model suspects it is being tested and changes its behavior. Hosting every site beneath an obviously artificial domain, or naming that domain in the prompt, can create that problem. Mirror Dimension therefore lets the agent request familiar addresses such as www.youtube.com while a headless Chrome instance runs inside an isolated gVisor sandbox.

Instead of allowing the request to reach external DNS or the public internet, the sandbox captures the connection and sends it to a local proxy serving generated sites. Because Chrome is built to block this sort of interception, the proxy dynamically mints TLS leaf certificates chained to a custom root certificate authority installed in the controlled browser. Google notes that an outside network attacker cannot install such a root on a real user’s device without privileged administrative access; the team can establish that trust anchor at startup because it controls the sandbox.

## Agents Build and Critique the Sites

Populating the environment requires more than static pages. The test sites must support multi-step workflows across local service marketplaces, e-commerce checkouts, social feeds and recipe pages, since an agent can detect cues from an unfinished simulation. A human defines high-level user journeys, then an autonomous multi-agent development harness designs, builds and revises the sites inside the sandbox.

A research agent turns the request into specifications and interface assets; a builder writes HTML, CSS and JavaScript; and critic agents check accessibility semantics, form validation, JavaScript console errors, element boundaries and responsive layouts. Chrome DevTools for Agents lets the sub-agents inspect screenshots, click, type, navigate and observe application behavior, allowing them to exercise rendered user journeys rather than only analyze source code. The critique loop prioritizes fidelity over completion speed, with critics rechecking fixes that can introduce new problems.

## Testing Prompt-Injection Defenses

That control also supports dynamic red-teaming against indirect prompt injection, where hidden instructions in untrusted web content try to make an agent take unauthorized actions. The server can insert payloads into images, text, accessibility labels or structured WebMCP tool responses at chosen points in multi-step workflows, covering the visual, DOM and tool interfaces a browser agent perceives.

Chrome initially identified payloads through visible URL parameters, but agents sometimes recognized abnormal parameters and became eval aware. The red-teaming harness now passes payload identifiers through custom HTTP headers. The sandbox filters those headers and serves the altered page while leaving the visible URL unchanged, preserving the appearance of an ordinary site.

Chrome reports that its automated pipeline creates more than 1,000 test candidates per security engineer each day. That is more than 30 times the few dozen realistic attacks an engineer can handcraft and test daily, allowing specialists to concentrate on finding new attack techniques while the pipeline refines and evaluates existing ideas.

The current implementation focuses on browser-use agents bounded to web primitives and tool calls. Chrome says the architecture could also be extended to general-purpose coding agents with broader toolsets. For browser agents, the design makes realism a safety requirement alongside containment: an agent that recognizes the sandbox may no longer show how it would behave on the public web.
