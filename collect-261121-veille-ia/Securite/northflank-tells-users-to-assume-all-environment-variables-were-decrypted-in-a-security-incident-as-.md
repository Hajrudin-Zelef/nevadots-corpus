---
id: collect-261121-veille-ia/securite/northflank-tells-users-to-assume-all-environment-variables-were-decrypted-in-a-security-incident-as-
title: "Northflank tells users to assume all environment variables were decrypted in a security incident as users report misused AI keys | SafeAI.watch"
domain: securite
role: reference
task: article
actors: []
dates: ["2026-10-30"]
keywords: ["users", "northflank", "incident", "keys", "environment", "variables", "assume", "decrypted"]
source: https://www.safeai.watch/latest/2026-10-30-northflank-tells-users-to-assume-all-environment-variables/
source_anchor: ""
source_lines: [1, 15]
section: "Securite"
---
# Northflank tells users to assume all environment variables were decrypted in a security incident as users report misused AI keys

- What happened
- Northflank opened a security incident on its status page at 1:40 PM UTC on October 30, 2026, telling users to "Rotate environment variables immediately."
- Key points
  - At 1:58 PM UTC it wrote: "Assume all environment variables have been decrypted. Disable/rotate all keys."
  - Later updates said "Multiple users are confirming that their AI keys have been used" and reported "unauthorised use of AI API keys supplied to the Northflank platform."
  - Northflank says these reports "may be consistent with the broader pattern of attacks targeting AI credentials," pointing to similar IP addresses in Wiz's "Black October" report.
  - By October 31 it had enabled secret rotation for Postgres, MongoDB, MySQL and Redis and turned on audit logs for all customers, including free users.
- Evidence
- The company's own status-page updates, posted while the incident stayed open. They give no cause, no count of affected users and no account of how attackers got in.
- Uncertain
- The incident was still marked "Identified" on October 31; Northflank's last investigation note, on October 30, said "Our investigation is ongoing." The AI-key misuse rests on user reports.

See it in the latest record
