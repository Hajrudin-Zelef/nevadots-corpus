---
id: collect-261121-veille-ia/gpt-openai/openai-daybreak-gpt-5-5-cyber-codex-security-patch-the-planet
title: "OpenAI Daybreak: GPT-5.5-Cyber, Codex Security & “Patch the Planet”"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["codex", "security", "openai", "cyber", "gpt-5", "patch", "model", "daybreak"]
source: https://aiwork-4ab7f.web.app/guides/openai-daybreak-gpt-5-5-cyber-codex-security
source_anchor: ""
source_lines: [1, 57]
section: "GPT_OpenAI"
---
## What Shipped

Daybreak now has four pieces. First, GPT-5.5-Cyber went to full release: a version of GPT-5.5 tuned for defensive security work. OpenAI reports it hits 85.6% on the CyberGym benchmark (up from 81.8% for standard GPT-5.5), 39.5% on ExploitGym (versus 25.95%), and 69.8% on SEC-bench Pro (versus 63.1%). Access stays restricted to vetted defenders through OpenAI's “Trusted Access for Cyber” program, so it is not something you call with an ordinary API key.

Second, the Codex Security plugin, which lives inside OpenAI's Codex coding agent, got an update. Since its March research preview it has scanned more than 30 million commits across more than 30,000 codebases; human reviewers marked over 70,000 findings as fixed and more than 500,000 were automatically determined to be fixed. Third, a Daybreak Cyber Partner Program lets security vendors build GPT-5.5 into their own products, with launch partners including Accenture, Cisco, CrowdStrike, IBM, Okta, Palo Alto Networks, and Wiz. Fourth, “Patch the Planet,” founded with Trail of Bits and HackerOne, funds researchers to fix critical open-source software, with 30+ projects signed on including cURL, the Go project, Python, Sigstore, and pyca/cryptography.

## Why It Matters

The interesting part isn't a single benchmark, it's the shape of the workflow. Codex Security doesn't just flag issues; it traces whether a flaw is actually reachable, drafts a patch, and verifies the fix before a human looks at it. That reachability + verification loop is what makes “find, fix, and prove it’s fixed at scale” plausible rather than a wall of low-signal alerts.

It also marks a clear strategy: keep the most capable offensive-leaning model (GPT-5.5-Cyber) behind vetting, while pushing the safer, defender-friendly surface (Codex Security, partner products, open-source funding) out broadly. For builders, that means the realistic on-ramp is the Codex plugin and partner tooling, not the frontier cyber model itself.

## Using The Codex Security Plugin

For most teams this is the part you'll touch first. Codex Security runs inside the Codex coding agent: you point it at a repo, it surfaces vulnerabilities with reachability context, and it can draft and self-verify a patch for you to review. The exact subcommands follow OpenAI's Codex Security docs, but the loop is consistent: scan, review the reachable findings, then generate and verify a fix.

```
# Codex Security runs inside OpenAI's Codex coding agent. It scans a repo,
# traces whether a flaw is actually reachable, drafts a patch, and verifies the
# fix before a human reviews it. Exact subcommands follow the Codex Security docs;
# the workflow is: scan -> review reachable findings -> generate + verify a patch.
# Install / update the Codex CLI, then enable the security plugin on a repo:
npm install -g @openai/codex          # the Codex coding agent CLI
codex security scan ./                 # surface vulnerabilities + reachability
codex security patch <finding-id>      # draft a fix, then auto-verify it
# Since its March research preview, Codex Security has scanned 30M+ commits across
# 30,000+ codebases; humans marked 70,000+ findings fixed and 500,000+ were
# auto-verified as fixed.
```
## Calling GPT-5.5-Cyber (If You're Vetted)

If your organization is approved under Trusted Access for Cyber, the model itself is reachable through the normal OpenAI Chat Completions API, just with a model name of `gpt-5.5-cyber`. Keep the system prompt scoped to authorized, defensive work; OpenAI gates this model precisely because the same capabilities can be misused.

```
# GPT-5.5-Cyber speaks the normal OpenAI Chat Completions API.
# IMPORTANT: it is gated. Access is restricted to vetted defenders through
# OpenAI's "Trusted Access for Cyber" program — a standard API key will not
# return the cyber model. For most teams the practical entry point is the
# Codex Security plugin (below), not the raw model.
# pip install openai
from openai import OpenAI
client = OpenAI(api_key="YOUR_TRUSTED_ACCESS_KEY")
resp = client.chat.completions.create(
    model="gpt-5.5-cyber",
    messages=[
        {"role": "system",
         "content": "You are a defensive security analyst. Only assist with "
                    "authorized triage and remediation."},
        {"role": "user",
         "content": "Triage this finding and propose a verified patch:\n<diff or stack trace>"},
    ],
)
print(resp.choices[0].message.content)
```
## Who It's For

Security and platform teams that want AI-assisted vulnerability triage and patching wired into their existing code workflow, and open-source maintainers of critical projects who may now get funded help via Patch the Planet. It's less relevant today if you want the raw GPT-5.5-Cyber model and aren't part of a vetted defender program, that access is intentionally narrow. If you're comparing options, read it alongside Anthropic's Project Glasswing approach before committing.
