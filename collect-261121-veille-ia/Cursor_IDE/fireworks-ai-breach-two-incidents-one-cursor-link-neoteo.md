---
id: collect-261121-veille-ia/cursor-ide/fireworks-ai-breach-two-incidents-one-cursor-link-neoteo
title: "Fireworks AI breach: Two incidents, one Cursor link | NeoTeo"
domain: cursor-ide
role: reference
task: article
actors: ["Anysphere"]
dates: []
keywords: ["fireworks", "cursor", "october", "access", "secrets", "says", "incident", "exposed"]
source: https://www.neoteo.com/en/fireworks-ai-breach-two-incidents-and-the-cursor-link
source_anchor: ""
source_lines: [1, 23]
section: "Cursor_IDE"
---
Fireworks AI’s November 17, 2026, update describes two separate security incidents, not one continuous breach. It dates the first to August 3–October 7 and the second to October 23–30; the later incident exposed account and billing information for accounts created before October 30, along with datasets, models, and/or secrets for a small number of customers, according to Fireworks’ update.

| Fireworks incident | Period | Reported access path | Reported data accessed | 
| First | August 3–October 7, 2026 | Stolen Fireworks credentials and a vulnerability in one service | Secrets, model files, and datasets for a small number of customers | 
| Second | October 23–30, 2026 | An unauthorized party reached development machines hosted by a third-party cloud provider and part of Fireworks’ production systems | Account and billing information for accounts created before October 30; datasets, models, and/or secrets for a small number of customers | 

Fireworks says it did not store or expose payment-card numbers. It reported that Unit 42 had completed its forensic investigation of the first incident. In the same November 17 update, Fireworks said AprosSyber was investigating the second; the company also described that incident as contained and remediated.

## How the reported chain reached Cursor

Cursor says an access key it issued to Fireworks was exposed there and used to copy a shared storage bucket with read-only access. That permission allowed the bucket’s contents to be copied. Some of the historical coding-agent traces inside it contained credentials that had not been redacted, which Cursor says enabled access to selected SaaS tools. Read-only access limited what the key could change; it did not prevent the copying of secrets stored in the bucket. Cursor’s incident update describes the chain.

Cursor says the traces were mostly from 2025 sessions outside Privacy Mode that it was permitted to use for model training. It reports that traces were exposed for fewer than 0.05% of users who were not using Privacy Mode, while API credentials were exposed for less than 0.01% of customers. Those percentages measure different things across different groups. Cursor says it identified and directly notified users whose traces were exposed, and that Privacy Mode prompts and code were not affected.

The credentials also provided access to selected archived, non-production repositories; limited support data and email; product-usage metrics and contact details; summaries of some coding-agent conversations; and some credentials sent to Cursor support, according to the company.

## What the extension estimate means

Cursor says malicious copies of Anthropic’s Claude Code versions 2.1.312 and 2.1.313 were served from the cache for its VS Code extension marketplace for about 11 hours on October 29. Cursor estimated that at most 5,000 users who requested specific cached versions may have received a malicious build. That is a potential-reach estimate, not a confirmed infection count.

For details on checking for the affected builds and removing their artifacts, see NeoTeo’s earlier guide to checking and cleaning up the affected Claude Code versions.

Read-only access can still expose secrets when those secrets are stored in data the key can copy.
