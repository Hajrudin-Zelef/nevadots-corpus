---
id: collect-261121-veille-ia/langages-dev/sqlite-euvd-2026-113787-medium-6-8-vuln-today
title: "SQLite EUVD-2026-113787 (MEDIUM 6.8) - vuln.today"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["sqlite", "untrusted", "collate", "database", "view", "depth", "recursion", "process"]
source: https://vuln.today/euvd/EUVD-2026-113787
source_anchor: ""
source_lines: [1, 72]
section: "Langages_Dev"
---
## AIDerived

Hypothetical attack flow derived from CVE metadata – not a confirmed exploit chain

1. 
                
                AccessSupply SQL or untrusted DB view with ~175,000 chained COLLATE operators
2. 
                
                ExploitBypass SQLITE_LIMIT_EXPR_DEPTH, drive unbounded recursion in sqlite3WalkExprNN()
3. 
                
                ImpactExhaust stack and crash the host process (denial of service)

|  | Exploitation requires the attacker to control SQL text (or an untrusted database/view) that SQLite parses, and to include approximately 175,000 chained COLLATE postfix operators on an expression so recursion in sqlite3WalkExprNN() exceeds the stack. Key limiting factors: the vector is local (AV:L) and involves passive user interaction (UI:P) such as opening an untrusted database, the impact is availability-only (process crash, no data exposure or code execution), and the application must actually route untrusted SQL or untrusted database files into SQLite - hardened apps that only run fixed, parameterized queries are not exposed. Full remediation is upgrading to SQLite 3.54.0, which enforces the depth check for COLLATE. | 
|  | This is a moderate, availability-only issue rather than a top-priority emergency. The CVSS 4.0 base score is 6.8 with VA:H but VC:N/VI:N - the only impact is a process crash (CWE-674 uncontrolled recursion / stack exhaustion), with no confidentiality or integrity loss and no code execution. The vector is AV:L/AC:L/PR:N/UI:P: low complexity and no privileges, but a local attack vector and passive user interaction, meaning exploitation hinges on the attacker being able to feed untrusted SQL text or an untrusted database/view to the SQLite engine. The vuln.today decision label is 'this_month', not act_now or emergency, which is consistent with a reliably-triggerable but non-critical DoS. There is no CISA KEV listing and no EPSS score was supplied, so widespread automated exploitation is neither confirmed nor quantifiable - the absence of EPSS is a data gap, not evidence of low risk. A public research write-up (abraxaslabs) and a VulnCheck advisory describe the flaw, and the vendor fix ships a regression test demonstrating the trigger, so the mechanism is well understood even though no weaponized exploit is confirmed. Net: treat as a routine-priority patch - real and easy to trigger wherever untrusted SQL reaches SQLite, but bounded to denial of service. | 
| Derived | An attacker who can submit SQL to an application backed by SQLite, or cause it to open an untrusted database containing a crafted view, supplies an expression with roughly 175,000 chained COLLATE postfix operators. This bypasses the SQLITE_LIMIT_EXPR_DEPTH guard and drives unbounded recursion through sqlite3WalkExprNN(), exhausting the stack and crashing the process. A public PoC reference and vendor regression test demonstrate the trigger; they are not verified as working weaponized exploit code, and the attack requires a local vector plus the application passing attacker-controlled SQL or opening an untrusted database. | 

## AIv1 – Oct 30, 2026

SQLite releases before 3.54.0 fail to enforce SQLITE_LIMIT_EXPR_DEPTH for chained COLLATE postfix operators, so an expression carrying roughly 175,000 COLLATE clauses drives unbounded recursion in sqlite3WalkExprNN() and crashes the process. The flaw is rated 6.8 with local attack vector, no privileges, and passive user interaction; impact is availability-only - no data disclosure or code execution - and exploitation requires the application to parse attacker-controlled SQL text or open an untrusted database/view. An upstream fix commit and the 3.54.0 release address the issue; no public exploit code or CISA KEV entry is associated with this CVE at time of analysis, and EPSS data was not provided.

## AI

Upgrade to SQLite 3.54.0 or later, which adds the expression-height check for COLLATE and rejects chains exceeding SQLITE_LIMIT_EXPR_DEPTH with 'Expression tree is too large' (release notes: https://sqlite.org/releaselog/3_54_0.html; fix commit: https://github.com/sqlite/sqlite/commit/d54522a5b832422ab893a1e9fa7a99ab314ea4e2, mirrored at https://sqlite.org/src/info/252c0b7bf5a727d2). Applications that bundle SQLite must rebuild against 3.54.0 rather than relying on an OS package alone. Because exploitation depends on untrusted SQL or database/view input reaching the parser, a compensating control for systems that cannot upgrade immediately is to prevent untrusted SQL text and attacker-supplied database files from being fed to SQLite - enforce fixed, parameterized statements in application code and avoid opening databases from untrusted sources; the trade-off is loss of functionality for any feature that legitimately accepts ad-hoc SQL or third-party database files. Lowering SQLITE_MAX_EXPR_DEPTH at compile time does not by itself block this technique, since the vulnerable path skipped the depth accounting entirely; do not treat that as a fix. Patch availability is confirmed from the vendor, and no public exploit code or active exploitation is reported in the available data.

### ENISA


## AI · show details

SQLite maintains a parse-time guard, SQLITE_MAX_EXPR_DEPTH (surfaced via SQLITE_LIMIT_EXPR_DEPTH), that rejects expression trees deeper than the configured limit with 'Expression tree is too large'. The COLLATE postfix operator is applied by sqlite3ExprAddCollateToken()/sqlite3ExprAddCollateString() in src/expr.c, and prior to the fix these functions did not increment the new expression node's nHeight nor call sqlite3ExprCheckHeight() when attaching a collation name, while the surrounding interfaces also took a const Parse* and therefore could not report the error. An attacker-controlled statement or view containing a very long COLLATE chain therefore builds an arbitrarily deep expression tree that passes the depth check and later causes unbounded recursion while the tree is walked in sqlite3WalkExprNN(), exhausting the stack and terminating the process - CWE-674 (uncontrolled recursion). The upstream commit d54522a5b832422ab893a1e9fa7a99ab314ea4e2 (mirrored as sqlite.org/src/info/252c0b7bf5a727d2) changes the Parse* parameters to non-const, sets pNew->nHeight = pExpr->nHeight+1, invokes sqlite3ExprCheckHeight() under SQLITE_MAX_EXPR_DEPTH>0, and adds sqllimits1-9.1b to test/sqllimits1.test to confirm the depth error is raised for a COLLATE chain. The affected product per CPE is cpe:2.3:a:sqlite:sqlite, all versions below 3.54.0 per the EUVD affected-version data.

`CVSS:4.0/AV:L/AC:L/AT:N/PR:N/UI:P/VC:N/VI:N/VA:H/SC:N/SI:N/SA:N`
                        
                    Attacker supplies untrusted SQL/database locally (AV:L) with low complexity and no privileges (AC:L/PR:N); opening an untrusted DB implies UI:R; impact is a crash only, so C:N/I:N/A:H.

Primary rating from Vendor (VulnCheck).

## Vendor: VulnCheck

## CVE.org

SQLite before 3.54.0 contains a stack exhaustion vulnerability that allows attackers to bypass the SQLITE_LIMIT_EXPR_DEPTH limit by chaining COLLATE postfix operators. Attackers can supply roughly 175,000 chained COLLATE operators in SQL or an untrusted database view to trigger unbounded recursion in sqlite3WalkExprNN() and crash the process.

The engine reads each signal below and applies a fixed list of rules from top to bottom. The first rule that matches sets the recommended action – like a statement, every line is shown.

## ENISA

Multiple unspecified vulnerabilities in SQLite before 3.8.10.2, as used in Apple iOS before 9, have unknown impact and a

SQLite3 from 3.6.0 to and including 3.27.2 is vulnerable to heap out-of-bound read in the rtreenode() function when hand

SQLite 1.0.12 through 3.39.x before 3.39.2 sometimes allows an array-bounds overflow if billions of bytes are used in a

The getNodeSize function in ext/rtree/rtree.c in SQLite through 3.19.3, as used in GDAL and other products, mishandles u

Memory corruption in SQLite versions before 3.50.2 allows network-based attackers with low privileges to manipulate aggr

An exploitable use after free vulnerability exists in the window function functionality of Sqlite3 3.26.0. Rated high se

Arbitrary DLL loading in SQLite's sqldiff.exe utility on Windows allows attackers to achieve code execution by abusing t

Memory corruption in SQLite versions before 3.53.2 enables attackers to crash processes, exhaust memory, or potentially

SQLite 3.32.2 has a use-after-free in resetAccumulator in select.c because the parse tree rewrite for window functions i

SQLite through 3.31.1 allows attackers to cause a denial of service (segmentation fault) via a malformed window-function

A segmentation fault can occur in the sqlite3.exe command-line component of SQLite 3.36.0 via the idxGetTableInfo functi

OAuth and SCIM identity confusion in Open WebUI 0.6.41-0.10.x (SQLite-backed deployments only) allows unauthenticated re
