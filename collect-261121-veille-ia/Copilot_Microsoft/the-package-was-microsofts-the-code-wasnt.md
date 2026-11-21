---
id: collect-261121-veille-ia/dev-langages/the-package-was-microsofts-the-code-wasnt
title: "The Package Was Microsoft’s. The Code Wasn’t."
domain: dev-langages
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["package", "azure", "python", "agents", "stealer", "every", "code", "tokens"]
source: https://ongoingideas.com/the-package-was-microsofts-the-code-wasnt/2026/cyber-security/1926/
source_anchor: ""
source_lines: [1, 35]
section: "Copilot_Microsoft"
---
Two poisoned releases of a Microsoft package sat on PyPI for less than an hour on Monday evening. That was long enough. Versions 1.26.1 and 1.26.2 of **agent-framework** — the meta-package for Microsoft Agent Framework, the open-source toolkit for building and orchestrating AI agents in Python — carried a credential stealer that runs every time Python starts. Both versions have since been quarantined on PyPI, but anyone who installed or updated the package in that window should assume their cloud credentials were taken.

The two malicious releases went up 33 minutes apart on November 17, 2026, and were flagged within hours by eight security firms — Aikido, Chainguard, Endor Labs, OX Security, SafeDep, Socket, StepSecurity, and Wiz. How the attackers got publishing access to a Microsoft package is still unknown. One firm, OX Security, said the attackers appear to have hijacked the wheel-creation flow — the step that builds the installable package — to slip malicious logic inside.

## A hook that runs before your code does

The trick is a small file with an outsized reach. Each malicious wheel drops a `.pth` file into the site-packages folder — the directory where Python keeps installed packages. Python runs the import lines in `.pth` files automatically on every interpreter start, so the stealer launches whenever Python starts in an affected environment, even if nothing ever imports Agent Framework. It runs when you run pip itself.

The filenames are the tell: `agent_framework_meta.pth` in version 1.26.1, and `af_runtime_diag.pth` in version 1.26.2. The first file holds a single import that launches the payload. The second version goes further, importing a new top-level module, `af_runtime_diag.py`, and widening the search for credential-related environment variables.

## What the stealer takes

Once running, the stealer sweeps the machine for anything that can open a cloud door. It grabs environment variables matching credential-related patterns, and reads credential files for Azure CLI, .npmrc, pip.conf, .netrc, .git-credentials, Docker, Hugging Face, Azure DevOps agents, plus .env and JSON files. It also pulls Azure DevOps OIDC tokens — the short-lived identity tokens pipelines use to prove who they are — along with Azure managed-identity tokens and up to 8,000 characters of Azure instance metadata.

Version 1.26.2 adds GitHub Actions OIDC tokens to the haul, which matters because those tokens are exactly what CI pipelines use to deploy to Azure. Everything collected is packed into a JSON document, compressed, Base64-encoded, and sent in an HTTPS POST to a hard-coded webhook.site address.

The worst part is what rotation cannot fix. As SafeDep put it, besides reading credentials already on disk, the code asks the platform to issue new ones — so rotating stored secrets does not cover those. In an Azure DevOps pipeline, two environment values are enough to request a federated token. On Azure compute, the stealer asks the managed-identity endpoint for an access token for each cloud resource it wants, using the App Service identity endpoint or the instance metadata address 169.254.169.254.

## Why this one stings

The package attracts about 190,000 downloads a month, and its users are exactly the people building AI agents and the CI pipelines behind them. A compromised developer tool does not hit one company — it fans out through every build, every runner, and every deployment that pulled the update. AI agents have already been caught stealing keys at machine speed; this time the keys were handed to the attacker by the tool the agents themselves run on.

It is also the second Microsoft-related PyPI package compromised this year. In May 2026, the actors behind TeamPCP pushed three malicious versions of durabletask — Microsoft’s Python client for the Durable Task workflow framework — to distribute the Mini Shai-Hulud worm. The pattern is now unmistakable: attackers are aiming at the AI developer ecosystem itself, and at the pipelines that ship its code.

## Four checks to run today

**1. Pin to the safe version — and rebuild clean.** Pin agent-framework to 1.26.0, the most recent safe release, and restore from a clean environment rather than trying to scrub a poisoned one. A machine that ran the bad versions cannot be trusted to clean itself.

**2. Audit what ran in the window.** Check for exposure to the malicious versions between 8:35 and roughly 9:20 p.m. UTC on November 17, 2026. Isolate affected hosts and runners first — CI secrets have walked out before, and a runner that executed the stealer may have handed over tokens for every system it can reach.

**3. Rotate from a clean system — and audit the identities, not just the secrets.** Rotate exposed credentials from a machine you know is clean, then audit Azure identities and review sign-in and activity logs. Because the stealer can request fresh tokens, check what those tokens could have opened, and audit GitHub Actions workflows that deploy to Azure.

**4. Hunt the exfiltration address.** Search proxy and DNS logs for requests to webhook.site/d381fd80-ec62-42ad-8f5c-d1b15103c9bb. Any hit is a confirmed compromise, not a false alarm — that address exists for one purpose.

The package was Microsoft’s. The code inside it wasn’t — and for the 45 minutes the poisoned versions were live, the difference was invisible to everyone who installed them. Pin the version, check the window, and rotate from clean ground. The next compromised package will not announce itself either.
