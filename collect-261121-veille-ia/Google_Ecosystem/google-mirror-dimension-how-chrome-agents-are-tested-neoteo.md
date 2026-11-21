---
id: collect-261121-veille-ia/google-ecosystem/google-mirror-dimension-how-chrome-agents-are-tested-neoteo
title: "Google Mirror Dimension: How Chrome agents are tested | NeoTeo"
domain: google-ecosystem
role: reference
task: article
actors: ["Google"]
dates: ["2026-11-19"]
keywords: ["test", "agents", "google", "chrome", "mirror", "dimension", "environment", "sites"]
source: https://neoteo.com/en/how-googles-mirror-dimension-tests-chrome-agents
source_anchor: ""
source_lines: [1, 29]
section: "Google_Ecosystem"
---
On November 19, 2026, Google described Mirror Dimension, a synthetic web environment for testing Chrome browsing agents. Headless Chrome runs inside an isolated gVisor sandbox, where a local proxy routes requests to generated websites instead of public internet destinations. Google’s technical description also outlines how the company builds those sites and varies the attacks agents encounter.

## What Mirror Dimension is built to test

Mirror Dimension is designed to evaluate protections for Chrome’s auto browse feature in realistic web journeys. Google says the environment contains thousands of simulated websites and describes hundreds of interactive, stateful pages across settings such as marketplaces, social feeds, and recipe sites.

The aim is to test how browser agents handle ordinary tasks and hostile content while their test traffic stays within the controlled environment. The simulated pages can behave like working websites, with interactive elements and changing states, rather than static mockups.

A useful test has to challenge the agent with realistic web content while keeping the browser’s network route inside a controlled environment.

## How the sandbox routes requests to synthetic sites

Google’s test setup runs a headless Chrome browser in gVisor, an isolated sandbox. A local proxy intercepts its requests and serves generated pages; the test path does not reach external DNS or the public internet.

The proxy also handles encrypted connections. It creates TLS certificates for the synthetic sites that chain to a custom root certificate installed in the sandboxed Chrome instance. That arrangement lets the browser trust the proxy’s test certificates within the controlled environment.

Agents navigate to ordinary-looking URLs, while custom HTTP headers carry hidden attack identifiers. This keeps those identifiers out of visible query parameters as the system varies test content.

## How Google creates test sites and varies attacks

Google’s site-building process combines human-defined user journeys with research, builder, and critic agents. The critic agents check features such as accessibility semantics, form validation, JavaScript errors, and responsive layouts. Google also describes using Chrome DevTools for Agents to inspect rendered pages and perform interactions such as clicks and keystrokes.

The red-team tests can place hostile instructions in text and images, use malicious ARIA labels, or alter WebMCP tool responses. ARIA labels are accessibility metadata that helps describe interface elements; changing them lets a test examine content beyond what is plainly visible on a page. These attacks can appear as part of multi-step workflows.

## What Google’s reported test figures measure

Google says its automated red-teaming pipeline generates **more than 1,000 attack-test candidates per security engineer per day**. The company characterizes that pace as more than 30 times what one engineer could test manually. The figures describe candidate generation for the testing pipeline.

Google said it plans to expand its internal security-testing infrastructure as browser agents gain richer capabilities.
