---
id: collect-261121-veille-ia/cursor-ide/the-cursor-extension-incident-shows-how-one-vendor-key-reaches-a-marketplace
title: "The Cursor extension incident shows how one vendor key reaches a marketplace"
domain: cursor-ide
role: reference
task: article
actors: ["Anysphere"]
dates: []
keywords: ["extension", "marketplace", "cursor", "data", "credentials", "traces", "vendor", "incident"]
source: https://community.benhalpern.com/stark_zhuang_df5076f35c68/the-cursor-extension-incident-shows-how-one-vendor-key-reaches-a-marketplace-4n5
source_anchor: ""
source_lines: [1, 25]
section: "Cursor_IDE"
---
Cursor's November 13 incident update traces a path from a third-party access key to a malicious extension in a developer marketplace. The sequence matters because no single step looks like a conventional server exploit. The attacker used a key issued to AI infrastructure provider Fireworks, copied data from a read-only storage bucket shared with that provider, found credentials that had not been redacted in a small number of agent traces, and then moved through selected SaaS tools.

The affected bucket contained limited coding-agent traces, mostly from 2025, from sessions outside Privacy Mode. Cursor says fewer than 0.01 percent of customers had API credentials exposed and that traces for fewer than 0.05 percent of users were exposed. From the SaaS access, the attacker reached selected archived non-production source repositories, limited support data and email conversations, product usage data, summaries of some agent conversations, and credentials users had sent to support.

On October 29, using a credential obtained from the internal system, the attacker placed modified copies of Anthropic's Claude Code extension directly into the cache serving the Cursor VS Code extension marketplace. The versions were 2.1.312 and 2.1.313. Anthropic did not publish them, the marketplace source Open VSX did not contain the malware, and Cursor says the malicious files were available for approximately eleven hours. At most 5,000 users who requested those cached versions may have received them.

A read-only share can still contain high-value material. In this case, the data included traces of agent activity, and some traces contained credentials that should have been removed before storage or sharing. A trace can include tool calls, environment variables, request headers, error output, URLs, and snippets of configuration. A storage permission that prevents the vendor from writing does not prevent copied data from enabling access elsewhere.

The second lesson is key scope. A vendor key should identify a workload, a destination, and a purpose. If the same key can read a broad bucket and the provider's own systems can use the resulting data, the attacker receives an identity that bridges two organizations. Cursor says it deactivated the key, moved inference off Fireworks, cut off access to shared data sets, and rotated credentials. Those are containment steps after the trust relationship failed, not controls that existed before the incident.

The marketplace path shows another bridge. The attacker did not need Anthropic's signing key or the upstream extension registry. It placed files in a cache used by the marketplace. That made the distribution layer an independent security boundary. The integrity of the source project, the marketplace database, and the CDN or cache are separate claims. A validation process that checks one of them does not automatically check the other two.

Cursor's notice says uninstalling the extension alone may not be enough. Organizations that installed or updated the named versions should follow the vendor's indicators and remediation script, inspect developer endpoints for persistence, and rotate credentials that were available to the extension. Do not assume that deleting the extension folder removes an SSH backdoor, a modified shell profile, or a token copied to another location.

Inventory developer tools separately from production servers. Coding assistants often run with the developer's file access, shell, cloud credentials, source repositories, and model API keys. A marketplace extension can therefore reach more than its visible permissions suggest. Use dedicated developer identities, short-lived cloud credentials, scoped repository tokens, and an endpoint policy that can block unapproved extension versions.

The provider relationship also needs an inventory. List which vendors receive traces, logs, prompts, code, or support data. Mark which fields have been redacted and which are allowed to leave the organization. Apply the same access review to a read-only share that you would apply to a production API. A vendor that has no write access can still disclose a credential that grants write access somewhere else.

Search for extension versions 2.1.312 and 2.1.313, unexpected SSH keys, new scheduled tasks, and outbound connections that began during the exposure window. Review support messages for credentials that were pasted into tickets. Rotate cloud, source-control, package-registry, model-provider, and SaaS tokens that were present in developer environments or shared data.

Cursor says the last attacker activity it observed was on November 1 and was blocked. It also lists unaffected systems, including production infrastructure, official Cursor updates, release-signing keys, active inference requests, customer code repositories, and other marketplace extensions. Those claims define the vendor's current scope. They do not remove the need for each customer to verify the local endpoint and its own credentials.

The durable control is a design that limits what one compromised relationship can read, what one unredacted trace can unlock, and what one cache entry can distribute. Marketplace provenance should bind the publisher, exact build, hash, and delivered artifact to the client request.

## Top comments (0)
