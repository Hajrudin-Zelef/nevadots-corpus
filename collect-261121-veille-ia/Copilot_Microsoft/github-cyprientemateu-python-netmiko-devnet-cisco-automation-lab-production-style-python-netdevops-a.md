---
id: collect-261121-veille-ia/cisco/github-cyprientemateu-python-netmiko-devnet-cisco-automation-lab-production-style-python-netdevops-a
title: "GitHub - cyprientemateu/python-netmiko-devnet-cisco-automation-lab: Production-style Python NetDevOps automation for Cisco IOS-XE — Netmiko, drift detection, RESTCONF validation, compliance dashboard"
domain: cisco
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["netdevops", "validation", "compliance", "full", "cisco", "automation", "python", "drift"]
source: https://github.com/cyprientemateu/python-netmiko-devnet-cisco-automation-lab
source_anchor: ""
source_lines: [1, 318]
section: "Copilot_Microsoft"
---
Production-style Cisco IOS-XE network automation using Python, Netmiko, YAML inventory, Pydantic schema validation, Jinja2 templating, compliance validation, drift detection, remediation, RESTCONF API integration, structured logging, config archival, dry-run mode, modular `core/` architecture, sequential + parallel multi-device orchestration, Flask compliance dashboard with Gunicorn production deployment, and scheduled compliance jobs.

This project demonstrates real-world NetDevOps automation practices using Python and Netmiko against Cisco IOS-XE devices (DevNet Sandbox).

It has evolved from simple SSH command execution into a full data-driven automation framework including:

- YAML-driven device and interface inventory
- Jinja2 configuration templating
- Interface provisioning
- Multi-interface automation
- Validation engine
- Drift detection
- Automatic remediation
- Structured logging with audit trail
- Rendered config archival before deployment
- Dry-run mode for safe validation
- Backup system
- JSON + HTML reporting dashboard

- Configure multiple interfaces
- Support Layer-2 and Layer-3 modes
- Assign IP addresses automatically
- Enable/disable interfaces
- Jinja2-templated config generation
- YAML inventory-driven execution

- Pre-change configuration backup
- Post-change validation
- Automatic rollback support
- Idempotent execution (safe re-runs, proven)

- Desired state loaded from YAML (not hardcoded)
- Interface block extraction from running config
- Subset-based compliance check (avoids false positives)
- Drift detection and classification

- Web UI at `http://localhost:5000`
- Overview page with summary cards and device status
- Dual SSH + RESTCONF badges per interface
- Report detail page with expected vs actual side by side
- Live log viewer with color-coded WARNING/ERROR lines
- Run Dry-Run or Live directly from browser
- Full execution history table
- **Gunicorn production deployment** — 4 workers, debug off, WSL

- JSON structured reports
- HTML dashboard reports
- Timestamped outputs
- Execution mode stamped in every report

- Structured logging via Python `logging` module
- Rotating file handler — 5MB max, 5 backups
- Execution ID (`EXEC-YYYYMMDD-HHMMSS` ) on every log line
- Per-device log files (`logs/{device_host}.log` )
- Persistent audit trail in `logs/netdevops.log`
- INFO / WARNING / ERROR log levels

- Pydantic schema validation before any connection
- Pre-change configuration backup
- Post-change validation
- Dry-run mode (`--dry-run` ) — validate without pushing
- Idempotent execution (safe re-runs, proven)

```
.env (credentials)
        ↓
load_inventory.py
  ├── load_devices()     → inventory/devices.yml
  └── load_interfaces()  → inventory/interfaces.yml
        ↓
Backup Running Config     → backups/
        ↓
Render via Jinja2         → configs/generated/
        ↓
Extract Interface Blocks
        ↓
Subset Compliance Check
        ↓
Remediate on DRIFT only   ← skipped in --dry-run
        ↓
JSON + HTML Reports       → reports/
        ↓
Structured Logging        → logs/netdevops.log
```
- Python 3.11
- Netmiko
- Cisco IOS-XE (DevNet Sandbox)
- Jinja2
- PyYAML
- python-dotenv
- TextFSM
- HTML + JSON reporting
- GitHub Actions (CI/CD)
- PowerShell

```
python-netmiko-devnet-cisco/
│
├── archive/
│   ├── README.md
│   └── legacy_scripts/
│       ├── cisco_connect.py
│       ├── send_multi_command.py
│       ├── send_config_set.py
│       ├── multi_interfaces_validation_rollback.py
│       ├── multi_interfaces_validation_rollback_backup_device_local.py
│       ├── jinja2_interface_generator.py
│       ├── load_inventory_V1.py
│       ├── FULL_CONSOLIDATED_NETDEVOPS_SCRIPT.py
│       ├── FULL_CONSOLIDATED_NETDEVOPS_SCRIPT_V2.py
│       ├── FULL_CONSOLIDATED_NETDEVOPS_SCRIPT_V3.py
│       ├── FULL_CONSOLIDATED_NETDEVOPS_SCRIPT_V4.py
│       ├── FULL_CONSOLIDATED_NETDEVOPS_SCRIPT_V5.py
│       ├── FULL_CONSOLIDATED_NETDEVOPS_SCRIPT_V6.py
│       └── NETDEVOPS_DOCUMENTATION_V1.md
├── config/
│   └── scheduler_config.yml
├── dashboard/
│   ├── app.py
│   ├── wsgi.py
│   ├── templates/
│   │   ├── base.html
│   │   ├── index.html
│   │   ├── report.html
│   │   └── logs.html
│   └── static/
│       └── style.css
├── backups/
├── configs/
│   └── generated/
├── docs/
│   ├── NETDEVOPS_DOCUMENTATION.md
│   └── python_and_network_automation.docx
├── images/
│   └── screenshoots/
├── inventory/
│   ├── devices.yml
│   └── interfaces.yml
├── logs/
│   └── netdevops.log
├── reports/
│   ├── html/
│   └── json/
├── scripts/
│   ├── core/
│   │   ├── __init__.py
│   │   ├── connection.py
│   │   ├── backup.py
│   │   ├── rendering.py
│   │   ├── compliance.py
│   │   ├── remediation.py
│   │   ├── reporting.py
│   │   ├── validator.py
│   │   ├── orchestrator.py
│   │   └── restconf.py
│   ├── load_inventory.py
│   ├── logger.py
│   ├── scheduler.py
│   └── main.py
├── templates/
│   └── interface.j2
├── tests/
├── .github/
│   └── workflows/
│       └── netdevops-ci.yml
│
├── README.md
├── requirements.txt
├── .env
├── .gitignore
└── LICENSE
```
- `load_devices()` reads`inventory/devices.yml`
- `load_interfaces()` reads`inventory/interfaces.yml`
- Credentials injected from `.env` at runtime

- Captures running configuration via `show running-config`
- Stores timestamped backup in `/backups`

- Extracts per-interface config blocks from running config
- Compares against desired state using subset logic
- Avoids false positives from Cisco-generated default lines

- Classifies each interface as:
  - COMPLIANT
  - DRIFT

- Triggered only when DRIFT is detected
- Pushes full desired config (description, IP, mode, state)
- Skips interfaces already COMPLIANT (idempotent)

- Generates:
  - JSON report (machine-readable)
  - HTML dashboard (visual report)

- YAML-driven device and interface inventory
- Pydantic schema validation (pre-connection)
- Reusable inventory loader module
- Jinja2 configuration templating
- Multi-interface configuration
- Routed (L3) and switched (L2) interface support
- Drift detection (Netmiko CLI)
- Compliance validation (dual-layer: Netmiko + RESTCONF)
- Automated remediation (full config push)
- RESTCONF API integration (`core/restconf.py` )
- Flask compliance dashboard (`dashboard/` )
- Scheduled compliance jobs (`scripts/scheduler.py` )
- Structured logging (rotating, console + file audit trail)
- Execution IDs (per-run traceability)
- Per-device log files
- Rendered config archival (`configs/generated/` )
- Dry-run mode (`--dry-run` flag)
- Modular `core/` architecture (`main.py` + 9 focused modules)
- Sequential multi-device orchestration
- Parallel multi-device orchestration (`--parallel` flag)
- Device labeling (`label` field in`devices.yml` )
- Backup generation
- HTML reporting dashboard (with RESTCONF validation section)
- JSON reporting (includes RESTCONF results)
- Idempotent execution (proven in single and multi-device)
- GitHub CI pipeline integration

Issue:

`% Invalid input detected`
Fix:

```
no switchport
ip address X.X.X.X X.X.X.X
```
Issue:

- Exact diff comparison always returned DRIFT even after correct remediation
- Cisco IOS adds default lines (`negotiation auto` ,`spanning-tree` , etc.)
- `no shutdown` is never written to running-config (it is the default)

Fix:

- Extract only the relevant interface block from running config
- Use subset check: verify all expected lines are present, ignore extras
- Exclude `no shutdown` from expected lines

```
2026-07-02 20:03:14 | INFO  | [EXEC-20260702-200309] | NetDevOps Framework — LIVE | SEQUENTIAL
2026-07-02 20:03:14 | WARNING | [EXEC-20260702-200309] | [device-1] [DRIFT] GigabitEthernet1/0/2 → DRIFT
2026-07-02 20:03:16 | INFO  | [EXEC-20260702-200309] | Remediation successful — GigabitEthernet1/0/2
2026-07-02 20:03:22 | INFO  | [EXEC-20260702-200309] | RESTCONF GET successful
2026-07-02 20:03:22 | INFO  | [EXEC-20260702-200309] | [device-1] RESTCONF [GigabitEthernet1/0/2] → COMPLIANT
2026-07-02 20:03:26 | INFO  | [EXEC-20260702-200309] | [device-2] [OK] GigabitEthernet1/0/2 → COMPLIANT
2026-07-02 20:03:27 | INFO  | [EXEC-20260702-200309] | [device-2] RESTCONF [GigabitEthernet1/0/2] → COMPLIANT
```
```
2026-05-31 16:35:43 | INFO  | [EXEC-20260531-163543] | NetDevOps Framework — LIVE | PARALLEL
2026-05-31 16:35:43 | INFO  | [EXEC-20260531-163543] | Connecting to devnetsandboxiosxec9k.cisco.com...
2026-05-31 16:35:43 | INFO  | [EXEC-20260531-163543] | Connecting to devnetsandboxiosxec9k.cisco.com...
2026-05-31 16:35:45 | INFO  | [EXEC-20260531-163543] | [device-2] [OK] GigabitEthernet1/0/2 → COMPLIANT
2026-05-31 16:35:45 | INFO  | [EXEC-20260531-163543] | [device-1] [OK] GigabitEthernet1/0/2 → COMPLIANT
2026-05-31 16:35:46 | INFO  | [EXEC-20260531-163543] | [device-1] Processing complete ✔
2026-05-31 16:35:46 | INFO  | [EXEC-20260531-163543] | [device-2] Processing complete ✔
2026-05-31 16:35:46 | INFO  | [EXEC-20260531-163543] | NetDevOps Framework — Execution Complete
```
```
2026-05-26 18:34:39 | INFO     | [EXEC-20260526-183439] | NetDevOps Framework — DRY-RUN MODE
2026-05-26 18:34:39 | INFO     | [EXEC-20260526-183439] | Inventory validation passed — 1 device(s), 3 interface(s)
2026-05-26 18:34:41 | INFO     | [EXEC-20260526-183439] | [OK]    GigabitEthernet1/0/2 → COMPLIANT
2026-05-26 18:34:41 | INFO     | [EXEC-20260526-183439] | [OK]    GigabitEthernet1/0/3 → COMPLIANT
2026-05-26 18:34:41 | INFO     | [EXEC-20260526-183439] | [OK]    GigabitEthernet1/0/4 → COMPLIANT
2026-05-26 18:34:41 | WARNING  | [EXEC-20260526-183439] | DRY-RUN MODE — remediation skipped, no changes pushed
2026-05-26 18:34:41 | INFO     | [EXEC-20260526-183439] | NetDevOps Framework — Execution Complete
```
```
2026-05-18 22:22:30 | WARNING  | [DRIFT] GigabitEthernet1/0/2 → DRIFT
2026-05-18 22:22:30 | WARNING  | [DRIFT] GigabitEthernet1/0/3 → DRIFT
2026-05-18 22:22:30 | WARNING  | [DRIFT] GigabitEthernet1/0/4 → DRIFT
2026-05-18 22:22:33 | INFO     | Remediation successful — GigabitEthernet1/0/2
2026-05-18 22:22:34 | INFO     | Remediation successful — GigabitEthernet1/0/3
2026-05-18 22:22:35 | INFO     | Remediation successful — GigabitEthernet1/0/4
```
```
2026-05-21 21:37:59 | INFO     | NetDevOps Framework — LIVE MODE
2026-05-21 21:38:02 | INFO     | [OK]    GigabitEthernet1/0/2 → COMPLIANT
2026-05-21 21:38:02 | INFO     | [OK]    GigabitEthernet1/0/3 → COMPLIANT
2026-05-21 21:38:02 | INFO     | [OK]    GigabitEthernet1/0/4 → COMPLIANT
2026-05-21 21:38:02 | INFO     | All interfaces COMPLIANT — no remediation needed
2026-05-21 21:38:02 | INFO     | NetDevOps Framework — Execution Complete
```
- Always backup before changes
- Validate after configuration
- Avoid manual verification
- Ensure idempotent automation
- Use structured reporting
- Externalize inventory and credentials
- Separate concerns (inventory, templating, execution)

- Scheduler as background service (systemd)
- PyATS/Genie validation (via WSL)
- Multi-vendor support
- Intent-based networking

This project demonstrates:

- Real-world network automation design
- Infrastructure as Code thinking
- Validation-first engineering approach
- Data-driven automation (YAML + Jinja2)
- Scalable modular software architecture
- Idempotent execution model (single and multi-device)
- Production-grade observability (logging + dry-run)
- Defensive automation (schema validation before execution)
- Concurrent network automation (parallel multi-device)
- API-native validation (RESTCONF dual-layer compliance)
- Full-stack NetDevOps (automation + web dashboard)
- Continuous compliance (scheduled automation jobs)

Cyprien Carlos Temateu

NetDevOps Practice Lab
