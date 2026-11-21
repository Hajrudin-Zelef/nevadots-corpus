---
id: collect-261121-veille-ia/google-ecosystem/google-built-a-mirror-copy-of-the-web-to-test-its-chrome-ai-agents-against-prompt-injection
title: "Google built a mirror copy of the web to test its Chrome AI agents against prompt injection"
domain: google-ecosystem
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["google", "chrome", "test", "agents", "built", "mirror", "copy", "prompt"]
source: https://mlq.ai/news/google-built-a-mirror-copy-of-the-web-to-test-its-chrome-ai-agents-against-prompt-injection/
source_anchor: ""
source_lines: [1, 25]
section: "Google_Ecosystem"
---
# Google built a mirror copy of the web to test its Chrome AI agents against prompt injection

- The Mirror Dimension runs headless Chrome in a gVisor sandbox and routes all traffic to a local proxy serving synthetic sites instead of the internet <sup>[1]</sup> .
- AI agents research, build and critique the test sites; Google says the pipeline yields over 1,000 test candidates a day per security engineer, more than 30 times manual output <sup>[1]</sup> .
- Tests target indirect prompt injection hidden in page text, images, DOM elements, ARIA labels or WebMCP tool responses <sup>[1]</sup> .

Google has built a sealed, synthetic copy of the web to test the AI agents that browse inside Chrome, Wired reported on Nov. 19 <sup>[2]</sup>. In a companion post, Chrome AI security engineers Aaron Selya and Charles Qian described the system, called the Mirror Dimension, as a hermetic environment of simulated sites where agents such as Chrome's auto browse can be attacked safely <sup>[1]</sup>.

The main target is indirect prompt injection: instructions hidden in web content that try to push an agent into actions its user never asked for <sup>[1]</sup>. Testing on the live web risks real accounts and real purchases, and Google wrote that it designed the sandbox to avoid signals, such as odd URL parameters, that could tip an agent off that it is being evaluated <sup>[1]</sup>.

## How the copy is built

A multi-agent site development harness writes the sites. A research agent drafts a specification, a builder agent writes the HTML, CSS and JavaScript, and critic agents test the result, while a human defines the user journeys to be exercised <sup>[1]</sup>. The research agent can draw interface mockups with Google's Nano Banana image model, and sub-agents use Chrome DevTools for Agents to click, type and take screenshots <sup>[1]</sup>. Google's post describes hundreds of interactive, stateful pages <sup>[1]</sup>.

Headless Chrome runs inside a gVisor sandbox, and its traffic goes to a local interception proxy rather than public DNS. The proxy issues TLS certificates on the fly that chain to a custom root certificate installed in the sandboxed browser, so the fake sites load as ordinary HTTPS pages <sup>[1]</sup>. Attack payloads are selected through HTTP headers such as X-Sandbox-Injection-Key, which keeps the visible URL clean <sup>[1]</sup>.

Security engineers can hand-craft a few dozen realistic attacks a day; the automated pipeline produces more than 1,000 test candidates per engineer per day, which Google puts at over 30 times manual output <sup>[1]</sup>. Wired reported that Doug Turner, a Google director of engineering, said the company plans to keep investing in the approach and wants to open it to other research teams inside Google <sup>[2]</sup><sup>[3]</sup>. Google has not published success rates or error rates from the tests, and it has not said it will release the sandbox outside the company <sup>[1]</sup><sup>[3]</sup>.

## Companies mentioned

## Further sources

This article was produced by MLQ.ai's AI newsroom and checked against primary sources such as company filings, official releases and court documents. Reports from a single outlet are attributed. Editor: Peter Foy. How we work · Spotted an error? Tell us.

The stories that matter, in one email. Free — unsubscribe anytime.
