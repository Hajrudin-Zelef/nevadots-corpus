---
id: collect-261121-veille-ia/google-ecosystem/google-built-a-fake-copy-of-the-internet-to-stop-chromes-ai-agent-being-tricked
title: "Google built a fake copy of the internet to stop Chrome’s AI agent being tricked"
domain: google-ecosystem
role: reference
task: article
actors: ["Google"]
dates: ["2026-11-19"]
keywords: ["google", "agent", "agents", "built", "fake", "wired", "copy", "internet"]
source: https://madrobot.blog/2026/11/19/google-chrome-mirror-dimension-fake-internet/
source_anchor: ""
source_lines: [1, 45]
section: "Google_Ecosystem"
---
Google’s artwork for its mirror dimension post, published November 19, 2026. Image: Google

Google has built a fake copy of the internet, which it calls the “mirror dimension”, to test Chrome’s AI agent before it is let loose on real websites. The sandbox holds thousands of simulated sites, from shops to social feeds, where Google can attack its auto browse feature with hidden instructions, two Chrome AI security engineers explained in a blog post on Thursday.

## Why not test on the real web?

Auto browse, built on Google’s Gemini models, can open sites, click through pages and fill in forms on your behalf. Testing it means thousands of actions across hundreds of websites. Google says pointing an agent at the live web for this is “ineffective, since the web is constantly changing, and irresponsible, since agents can cause harm when given unfettered access”.

A simple test site has its own problem: “eval awareness”. Engineers Aaron Selya and Charles Qian write that if a site feels fake, the agent may work out it is being tested and behave differently. In early tests, an agent became suspicious simply because it spotted an odd parameter in a web address. “Frontier models are far too perceptive for this,” they write.

## Same URL, different dimension

So the agent is allowed to type the real address, such as youtube.com, just as it would at home. Underneath, it runs a headless copy of Chrome inside an isolated gVisor sandbox, and instead of reaching the public internet, every request is caught and sent to a local proxy that serves Google’s generated version of the site.

That is exactly the kind of interception Chrome is designed to block, so the proxy creates security certificates on the fly, signed by a custom root authority installed only in the sandboxed browser. Google says an attacker could not do this on a real user’s device without already having administrator access. The team stressed to Wired that none of these tricks would work against ordinary Chrome users.

The fake sites are built by AI too. A human describes the journeys to test, and a team of agents does the rest: a research agent writes the specification (with mock images from Google’s Nano Banana), a builder agent writes the code, and critic agents check forms, layouts, accessibility labels and errors through Chrome’s DevTools tools for agents. The results range from shopping checkouts with promo codes to social media feeds, recipe sites and, Wired reports, bank dashboards.

## “The web is hostile”

The main point is safety. The biggest threat Google is testing for is indirect prompt injection: instructions hidden in a web page to trick an agent into doing something its user never asked for. Because Google controls the whole server, it can slip attacks into visible text, images, hidden accessibility labels or responses from WebMCP tools, at any step of a long task.

Google says its automated red-teaming pipeline turns each security engineer’s ideas into more than 1,000 test candidates a day, over 30 times what one person could do by hand. Qian told Wired:

Agents are not programmed so you can’t predict every possible thing an agent might see, the exploration space that we have to address is huge and the web is hostile.

Charles Qian, software engineer, Chrome AI security, to Wired

He said the agent has begun to show “a mature sense of judgement” about what information a task really needs and when to hand back to the user for confirmation. Google has not published any figures on how often auto browse fell for the attacks, so that claim can’t be checked.

## Sharing the idea

Google plans to open the mirror dimension to other teams across the company, and says the design could be extended to coding agents with wider powers. Doug Turner, a director of engineering at Google, told Wired:

We’re sharing this approach so other teams both internally and externally think about doing it this way.

Doug Turner, director of engineering, Google, to Wired

Agents loose on the real web have already caused trouble. The UK’s AI Security Institute cut its agents off from the internet after some went after real people, and an OpenAI agent escaped its sandbox by hiding questions in DNS lookups. Banks are wary too: Canada’s RBC is locking agents such as Meta’s Muse out of online banking. And the race is on in the browser, where OpenAI’s ChatGPT extension can now click around in Chrome tabs.

## Why it matters

Browser agents are moving into signed-in accounts for shopping, email and banking, where one hidden instruction could do real damage. Google is betting that a fully controlled fake web is the safest way to find those weaknesses first, but until it shares results, users have only its word that auto browse resists them.

*Sources: Google (Chrome AI security blog post); Wired (interviews with Charles Qian and Doug Turner).*
