---
id: collect-261121-veille-ia/securite/fireworks-ai-breach-cluster-threat-detection-logs-and-iocs
title: "Fireworks AI breach cluster threat: detection, logs and IOCs"
domain: securite
role: reference
task: article
actors: []
dates: []
keywords: ["fireworks", "cursor", "logs", "breach", "vendor", "traces", "storage", "credentials"]
source: https://www.spectrum.security/threats/fireworks-ai-breach-cluster
source_anchor: ""
source_lines: [1, 52]
section: "Securite"
---
## Summary

An unattributed attacker used an access key that Cursor issued to its AI inference vendor Fireworks AI, exposed in that vendor's October breaches, to copy coding-agent traces from a shared storage bucket and harvest unredacted credentials from them, reaching Cursor's internal SaaS tools and planting trojanized Claude Code extension builds for about 11 hours, Cursor disclosed (A/1). Fireworks' widened disclosure of November 9 adds account and billing data, stored secrets, datasets and incomplete model weights, and the same breach cluster reached Uber, which is notifying about 150,000 drivers, and Notion, which lost AI chat transcripts from under one percent of workspaces. Fewer than 0.01 percent of Cursor customers had API credentials exposed in the traces, which came from coding sessions not using Privacy Mode. Railway, OpenCode, Warp and Fireworks disclosed similar AI-key thefts between October 6 and 23, which Railway assesses to be a series of attacks on platforms that hold AI keys.

## Breakdown

### Countries

- United States

### Targets

- Organization: Cursor
- Organization: Fireworks AI
- Organization: Uber
- Organization: Notion
- Industry: AI developer platforms

### Technical exposure

- Vendor-issued access keys exposed in the vendor's own environment
- Coding-agent traces stored unredacted in shared storage buckets
- AI coding sessions run without Privacy Mode retaining credentials
- Credentials shared with support teams stored in SaaS tools
- Marketplace distribution caches writable with a stolen credential

### Adversary

Unattributed (financially motivated per Fireworks and Uber disclosures) · Cursor, Fireworks AI (A/1)

## Procedures (4)

1. ### Logs
  - Cloud storage audit logs · Bulk reads of a read-only bucket used for vendor data sharing, authenticated with the vendor's access key, outside expected transfer patterns (S3 data events or equivalent object-storage audit trail)
2. ### Logs
  - Storage access logs · Reads of coding-agent trace storage covering sessions without Privacy Mode; traces containing API credentials that were never redacted
  - DLP scan results · Secret patterns (API keys, tokens) detected in stored AI coding-agent traces and training-data buckets
3. ### Trojanized extension planted in marketplace cache
  - T1195.002Supply Chain Compromise: Compromise Software Supply Chain
 ### Logs
  - Extension install logs · Installs of Anthropic.claude-code versions 2.1.312 or 2.1.313 from the Cursor marketplace on October 29 (builds live about 11 hours, up to 5,000 users); extension attempts to add an SSH key to authorized_keys and create a reverse SSH tunnel

## Sources

- A/1Cursor: An update on our recent security incident
- A/1Fireworks AI: Update on October 2026 security incident
- B/3SecurityWeek: Cursor Supply Chain Attack Started at Fireworks AI
- B/3BleepingComputer: Cursor confirms security breach led to malicious Claude Code extension

Grades are source reliability (A–F) and information credibility (1–6).

Questions, detections or sightings of Fireworks AI breach cluster? Bring them to the community.
