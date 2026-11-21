---
id: collect-261121-veille-ia/copilot-microsoft/github-niromuru-jabelk-nornir-demo
title: "GitHub - niromuru/jabelk-nornir-demo"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["task", "handling", "network", "error", "nornir", "execution", "result", "data"]
source: https://github.com/niromuru/jabelk-nornir-demo
source_anchor: ""
source_lines: [1, 51]
section: "Copilot_Microsoft"
---
This repository contains a series of demo scripts showcasing Nornir's capabilities for network automation. Network device is from Cisco DevNet Sandbox.

- Basic Nornir setup and initialization
- Simple task execution to greet hosts
- Basic host information display

- Working with Nornir's Result objects
- Task result handling and inspection
- Error handling and exception management
- Custom task result formatting

- Device connection using Netmiko
- Command execution on network devices
- Output processing
- Basic error handling

- Running multiple tasks in sequence
- Data passing between tasks
- Task result aggregation
- Programmatic task control

- Template-based configuration
- Jinja2 template rendering
- Configuration application
- Dynamic config generation

- Multi-vendor support with NAPALM
- Data collection and processing
- CSV output generation
- Structured data handling

- Error handling and debugging
- Failed task inspection
- API integration example
- Exception handling patterns

- **Inventory Management** : YAML-based inventory with groups and variables
- **Task Execution** : Parallel and sequential task execution
- **Plugin System** : Integration with Netmiko and NAPALM
- **Template Support** : Jinja2 templating for configurations
- **Error Handling** : Comprehensive error handling and debugging
- **Data Processing** : Collection, transformation, and output
- **Multi-vendor Support** : Working with different network platforms

- Python 3.8+
- Nornir and its plugins (see `requirements.txt` )
- Network device access (DevNet sandbox or similar)

Each script can be run independently:

`python <script_name>.py`
