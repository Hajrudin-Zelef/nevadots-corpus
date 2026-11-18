HFortix FortiOS
Python SDK for FortiGate/FortiOS API - Complete, type-safe, production-ready.
⚠️ BETA STATUS - Version 0.5.34 (January 8, 2026)
Breaking Changes: See v0.5.33 and v0.5.32 for important return type changes in dict/object mode. Status: Production-ready but in beta until v1.0 with comprehensive unit tests. What's New: See below for all recent improvements, fixes, and new features!
Version: 0.5.34 Status: Beta (100% auto-generated, production-ready, optimized for performance)
🚀 What's New in v0.5.34 (January 2026)
Major Improvements and Breaking Changes (v0.5.32–v0.5.34)
- 
Enhanced type stub overloads for better IDE autocomplete 
  - All 1,000+ endpoints now have specific overloads for default mode (when response_mode is not specified)
  - Pylance now correctly infers types for queries by mkey without explicit type annotations
  - No longer need to annotate: rule: RuleResponse = ... — just userule = ...
  - Improved overload ordering for better type inference
- All 1,000+ endpoints now have specific overloads for default mode (when 
- 
Dict/Object mode query by name returns single item 
  - When querying by name/mkey with response_mode="dict" orresponse_mode="object" , now returns a single dict/object instead of a list
  - Example: group = fgt.api.cmdb.firewall.service.group.get(name="test") returns adict orFortiObject , not a list
  - Breaking Change: Tests and code expecting a list for single-item queries must be updated
- When querying by name/mkey with 
- 
Nested typed classes for table field children 
  - Table fields now have their own typed classes (e.g., GroupMemberObject )
  - Enables full IDE autocomplete for nested table attributes like .name ,.id , etc.
- Table fields now have their own typed classes (e.g., 
- 
Keyword argument support for mkey parameters 
  - Both get("name") andget(name="name") infer the correct return type
- Both 
- 
IDE autocomplete for table field members in object mode 
  - Table fields now return typed objects instead of generic FortiObject
  - Full attribute autocomplete for nested objects
- Table fields now return typed objects instead of generic 
- 
Universal table field normalization with schema awareness 
  - Handles custom mkeys: interface-name ,id ,index ,seq-num ,priority , etc.
  - All string values are automatically stripped of whitespace
- Handles custom mkeys: 
- 
Enhanced parameter documentation 
  - All POST/PUT method parameters now show field descriptions from the FortiOS schema in IDE tooltips
- 
Type annotations for FortiOS client attributes 
  - Improved type inference and autocomplete for fgt.api ,fgt.api.cmdb , etc.
- Improved type inference and autocomplete for 
- 
Other Notable Fixes and Improvements: 
  - Fixed stub generator comment truncation (no more broken comments in stubs)
  - Enhanced error messages for duplicate name/unique field conflicts
  - set() now accepts all field parameters (not just payload_dict)
  - Singleton endpoints now return a single object, not a list
  - FortiObject now supports both attribute and dictionary-style access
  - Filter parameter now accepts both string and list
  - Interactive help system for all API endpoints: endpoint.help()
  - Formatting utilities: to_json() ,to_csv() ,to_dict() , etc.
See the complete changelog for all details and previous versions.
Overview
Complete Python client for FortiOS 7.6.5 REST API with 100% endpoint coverage (1,219 endpoints), full type safety, and enterprise features. All code is auto-generated from FortiOS API schemas.
Installation
pip install hfortix-fortios
This automatically installs:
- hfortix-core - Core utilities and HTTP client
- hfortix-fortios-stubs - Type stubs for optimal IDE/type checker performance
For minimal installation (without stubs, smaller size):
pip install --no-deps hfortix-fortios
pip install hfortix-core  # Then install only runtime dependencies
For everything (includes future products):
pip install hfortix[all]
Quick Start
from hfortix_fortios import FortiOS
# Connect to FortiGate
fgt = FortiOS(
    host="192.168.1.99",
    token="your-api-token",
    verify=False
)
# Get system status
status = fgt.monitor.system.status()
print(f"Hostname: {status['hostname']}")
print(f"Version: {status['version']}")
# Manage firewall addresses
fgt.api.cmdb.firewall.address.create(
    name="web-server",
    subnet="192.168.1.100 255.255.255.255"
)
# 🎯 NEW! IDE autocomplete with Literal types (v0.5.4+)
fgt.api.cmdb.firewall.policy.create(
    name="allow-web",
    action="accept",      # 💡 IDE suggests: 'accept', 'deny', 'ipsec'
    status="enable",      # 💡 IDE suggests: 'enable', 'disable'
    logtraffic="all"      # 💡 IDE suggests: 'all', 'utm', 'disable'
)
API Coverage
FortiOS 7.6.5 - 100% Coverage (1,219 Endpoints):
- CMDB API: 886 endpoints - Full configuration management (firewall, system, VPN, routing, etc.)
- Monitor API: 295 endpoints - Real-time monitoring (sessions, stats, resources, etc.)
- Log API: 38 endpoints - Log queries (disk, memory, FortiAnalyzer, FortiCloud, search)
All endpoints are 100% auto-generated with:
- Complete .pyi type stub files
- Schema-based parameter validation
- Auto-generated basic tests
- Comprehensive error handling
Key Features
🎯 IDE Autocomplete with Literal Types (NEW in v0.5.4!)
15,000+ parameters with intelligent IDE autocomplete! Every enum parameter provides instant suggestions:
# ✨ Autocomplete for ALL enum fields
fgt.api.cmdb.firewall.policy.create(
    action='accept',      # 💡 IDE: 'accept', 'deny', 'ipsec'
    status='enable',      # 💡 IDE: 'enable', 'disable'
    nat='enable',         # 💡 IDE: 'enable', 'disable'
    logtraffic='all'      # 💡 IDE: 'all', 'utm', 'disable'
)
# 🛡️ Type safety catches errors at development time
fgt.api.cmdb.system.interface.create(
    mode='static',        # 💡 IDE: 'static', 'dhcp', 'pppoe'
    type='physical',      # 💡 IDE: 'physical', 'vlan', 'tunnel', ...
    role='lan'            # 💡 IDE: 'lan', 'wan', 'dmz', 'undefined'
)
Benefits: ⚡ Instant autocomplete • 🛡️ Type safety • 📚 Self-documenting • ✅ 100% backward compatible
🎯 Complete API Coverage
Access every FortiOS endpoint with clean, Pythonic syntax:
# CMDB (Configuration)
fgt.api.cmdb.firewall.policy.get()
fgt.api.cmdb.system.interface.get(name="port1")
fgt.api.cmdb.router.static.create(...)
# Monitor (Real-time data)
sessions = fgt.api.monitor.firewall.session.get()
resources = fgt.api.monitor.system.resource.usage.get()
# Log (Query logs)
vpn_logs = fgt.api.log.disk.event.vpn.get(rows=50)
traffic = fgt.api.log.memory.traffic.forward.get(rows=100)
🎨 Pretty Printing with FortiObject (NEW in v0.5.19!)
Clean, readable output for FortiOS data using response_mode="object":
# Enable object mode for pretty methods
fgt = FortiOS(
    host="192.168.1.99",
    token="your-token",
    response_mode="object"  # Returns FortiObject instead of dict
)
# Get policies and print cleanly
policies = fgt.api.cmdb.firewall.policy.get()
for policy in policies:
    print(f"\nPolicy {policy.policyid}: {policy.name}")
    print(f"  {policy.join('srcintf')} → {policy.join('dstintf')}")
    print(f"  {policy.join('srcaddr')} → {policy.join('dstaddr')}")
    print(f"  Service: {policy.join('service')} [{policy.action.upper()}]")
# Output:
# Policy 11: allow-web
#   port3 → port4
#   login.windows.net → gmail.com
#   Service: SAMBA [DENY]
FortiObject Methods:
- obj.join('field') - Join list values into comma-separated string
- obj.join('field', ' | ') - Custom separator
- obj.pretty('field') - Alias for join() with default separator
- Auto-flattens member_table fields: ['port1'] instead of[{'name': 'port1'}]
Benefits:
- 📊 Clean console output
- 🎯 No manual list comprehension needed
- ✨ Works with all FortiOS list fields
- 🔄 Original data always accessible via .to_dict()
🎨 Direct API Access
All 1,219 endpoints are accessed directly - no wrappers needed:
# Service Management
fgt.firewall.service_custom.create(
    name="custom-app",
    tcp_portrange="8080-8090",
    comment="My application"
)
# Schedules
fgt.firewall.schedule_recurring.create(
    name="business-hours",
    day=["monday", "tuesday", "wednesday", "thursday", "friday"],
    start="08:00",
    end="17:00"
)
# Traffic Shaping
fgt.firewall.traffic_shaper.create(
    name="critical-apps",
    guaranteed_bandwidth=50000,
    maximum_bandwidth=100000,
    bandwidth_unit="kbps"
)
# IP/MAC Binding
fgt.firewall.ipmacbinding_table.create(
    ip="10.0.1.100",
    mac="00:11:22:33:44:55",
    name="Server-01"
)
Available Wrappers:
- Service Management: service_custom ,service_category ,service_group
- Schedules: schedule_onetime ,schedule_recurring ,schedule_group
- Traffic Shaping: traffic_shaper ,shaper_per_ip
- IP/MAC Binding: ipmacbinding_table ,ipmacbinding_setting
- SSH/SSL Proxy: ssh_host_key ,ssh_local_ca ,ssh_local_key ,ssh_setting ,ssl_setting (⚠️ with API limitations)
- Firewall Policies: policy with 150+ parameters
Note: Some wrappers have FortiOS API limitations (e.g., SSH CA deletion requires CLI/GUI). See documentation for details.
⚡ Advanced Features
Async/Await Support:
import asyncio
async def main():
    async with FortiOS(host="...", token="...", mode="async") as fgt:
        # All methods support await
        addresses = await fgt.api.cmdb.firewall.address.list()
        # Concurrent operations
        addr, pol, svc = await asyncio.gather(
            fgt.api.cmdb.firewall.address.list(),
            fgt.api.cmdb.firewall.policy.list(),
            fgt.api.cmdb.firewall.service.custom.list()
        )
asyncio.run(main())
Error Handling:
from hfortix_core import (
    APIError,
    ResourceNotFoundError,
    DuplicateEntryError
)
try:
    fgt.api.cmdb.firewall.address.create(name="test", subnet="10.0.0.1/32")
except DuplicateEntryError:
    print("Address already exists")
except ResourceNotFoundError:
    print("Resource not found")
except APIError as e:
    print(f"API Error: {e.message} (code: {e.error_code})")
Read-Only Mode & Operation Tracking:
# Safe testing - block all write operations
fgt = FortiOS(host="...", token="...", read_only=True)
# Audit logging - track all API calls
fgt = FortiOS(host="...", token="...", track_operations=True)
operations = fgt.get_operations()
Performance Testing:
# Test your device and get optimal settings
results = fgt.api.utils.performance_test()
print(f"Recommended settings: {results['recommendations']}")
🔧 Enterprise Features
- Audit Logging: Built-in compliance logging with SIEM integration (SOC 2, HIPAA, PCI-DSS)
- Observability: Structured logging, distributed tracing with trace_id , user context tracking
- HTTP/2 Support: Connection multiplexing for better performance
- Automatic Retry: Handles transient failures (429, 500, 502, 503, 504) with exponential/linear/fibonacci backoff
- Circuit Breaker: Prevents cascade failures with automatic recovery
- Request Tracking: Correlation IDs for distributed tracing
- Validation Framework: 832 auto-generated validators
🔍 Debugging & Monitoring (v0.4.0)
Quick Debug Mode:
# Enable debug logging with simple boolean
fgt = FortiOS(host="...", token="...", debug=True)
Connection Pool Monitoring:
# Real-time connection statistics
stats = fgt.connection_stats
print(f"Active: {stats['active_requests']}/{stats['max_connections']}")
print(f"Total requests: {stats['total_requests']}")
print(f"Pool exhaustion: {stats['pool_exhaustion_count']}")
Request Inspection:
# Debug slow or failed requests
result = fgt.api.cmdb.firewall.address.list()
info = fgt.last_request
print(f"Endpoint: {info['endpoint']}")
print(f"Response time: {info['response_time_ms']}ms")
print(f"Status: {info['status_code']}")
Debug Session:
from hfortix_fortios import DebugSession
# Comprehensive session monitoring
with DebugSession(fgt) as session:
    # Make API calls
    fgt.api.cmdb.firewall.address.list()
    fgt.api.cmdb.firewall.policy.list()
    # Auto-prints summary on exit:
    # - Duration, total requests, success/failure counts
    # - Avg/min/max response times
    # - Connection pool deltas
Performance Profiling:
from hfortix_fortios import debug_timer
# Time individual operations
with debug_timer("Fetch all addresses") as timing:
    result = fgt.api.cmdb.firewall.address.list()
print(f"Took {timing['duration_ms']:.1f}ms")
Enhanced Logging:
from hfortix_fortios import configure_logging
# JSON logging for ELK/Splunk
configure_logging(
    level="INFO",
    format="json",
    include_trace=True,  # Add request_id to all logs
    output_file="/var/log/fortios.log"  # Log to file
)
# Text logging with colors for development
configure_logging(
    level="DEBUG",
    format="text",
    use_color=True
)
Type Hints & IDE Support:
# Full type hints for better autocomplete
from hfortix_fortios import FortiOS
from hfortix_core import APIResponse, ListResponse
fgt: FortiOS = FortiOS(host="...", token="...")
response: APIResponse = fgt.api.cmdb.firewall.address.get(name="test")
See docs/fortios/DEBUGGING.md for complete debugging guide.
- Type Safety: Full type hints with IDE autocomplete
- Structured Logging: Machine-readable JSON logs for ELK/Splunk/CloudWatch
Import Patterns
Recommended (New)
from hfortix_fortios import FortiOS
Legacy (Still Supported)
from hfortix import FortiOS
from hfortix.FortiOS import FortiOS
API Structure
# Configuration Management (CMDB)
fgt.api.cmdb.firewall.policy.*
fgt.api.cmdb.firewall.address.*
fgt.api.cmdb.system.interface.*
fgt.api.cmdb.router.static.*
fgt.api.cmdb.vpn.ipsec.*
# Monitoring
fgt.api.monitor.system.status()
fgt.api.monitor.firewall.session.*
fgt.api.monitor.system.resource.*
# Logging
fgt.api.log.disk.traffic.*
fgt.api.log.disk.event.*
fgt.api.log.disk.virus.*
# Convenience Wrappers
fgt.firewall.policy.*
fgt.firewall.service_custom.*
fgt.firewall.schedule_recurring.*
fgt.firewall.traffic_shaper.*
Documentation
Main Guides:
- Quick Start - Getting started guide
- Async Guide - Async/await patterns
- API Reference - Complete method reference
Convenience Wrappers:
- Overview Guide - All wrappers
- Service Wrappers - Service management
- Schedule Wrappers - Schedule management
- Shaper Wrappers - Traffic shaping
Advanced Features:
- Validation Guide - Using validators
- Filtering Guide - FortiOS filtering
- Performance Testing - Optimization
Full Documentation:
- Complete Changelog - Version history
- Main Repository - Complete docs
Requirements
- Python 3.10+
- FortiOS 7.0+ (tested with 7.6.5)
- hfortix-core >= 0.4.0-dev1
Development Status
Beta - All APIs are functional and tested against live FortiGate devices. The package remains in beta status until version 1.0.0 with comprehensive unit test coverage.
Current Test Coverage:
- 226 test files (145 CMDB, 81 Monitor)
- 75%+ pass rate
- ~50% of endpoints have dedicated tests
- All implementations validated against FortiOS 7.6.5
Examples
Firewall Policies
# Create policy
fgt.firewall.policy.create(
    name="Allow-Web",
    srcintf=["port1"],
    dstintf=["port2"],
    srcaddr=["all"],
    dstaddr=["web-servers"],
    action="accept",
    schedule="always",
    service=["HTTP", "HTTPS"],
    logtraffic="all"
)
# Check if exists
if fgt.firewall.policy.exists(policy_id=10):
    fgt.firewall.policy.update(policy_id=10, status="disable")
Address Management
# Create address
fgt.api.cmdb.firewall.address.create(
    name="web-server",
    subnet="192.168.1.100 255.255.255.255",
    comment="Production web server"
)
# Create address group
fgt.api.cmdb.firewall.addrgrp.create(
    name="internal-networks",
    member=["subnet1", "subnet2", "subnet3"],
    comment="All internal networks"
)
VPN Configuration
# Create IPsec Phase 1
fgt.api.cmdb.vpn.ipsec.phase1_interface.create(
    name="site-to-site",
    type="static",
    interface="wan1",
    ike_version=2,
    peertype="any",
    proposal="aes256-sha256",
    remote_gw="203.0.113.10"
)
License
Proprietary - See LICENSE file
Support
Author
Herman W. Jacobsen
- Email: herman@wjacobsen.fo
- LinkedIn: linkedin.com/in/hermanwjacobsen
- GitHub: @hermanwjacobsen