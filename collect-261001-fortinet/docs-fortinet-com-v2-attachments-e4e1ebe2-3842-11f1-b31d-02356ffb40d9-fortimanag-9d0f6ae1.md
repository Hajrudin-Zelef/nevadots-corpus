API Best Practices Guide
FortiManager 8.0.0

FORTINET DOCUMENT LIBRARY
https://docs.fortinet.com
FORTINET VIDEO LIBRARY
https://video.fortinet.com
FORTINET BLOG
https://blog.fortinet.com
CUSTOMER SERVICE & SUPPORT
https://support.fortinet.com
FORTINET TRAINING & CERTIFICATION PROGRAM
https://www.fortinet.com/training-certification
FORTINET TRAINING INSTITUTE
https://training.fortinet.com
FORTIGUARD LABS
https://www.fortiguard.com
END USER LICENSE AGREEMENT
https://www.fortinet.com/doc/legal/EULA.pdf
FEEDBACK
Email: techdoc@fortinet.com
April 21, 2026
FortiManager 8.0.0 API Best Practices Guide
02-800-1281541-20260421

TABLE OF CONTENTS
Change Log 4
Introduction 5
Getting started 6
Best Practices 7
Authentication and access control 7
API requests 8
Error handling and resilience 13
Performance optimization 13
Security and compliance 21
Change management 22
Support and resources 22
FortiManager 8.0.0 API Best Practices Guide 3
Fortinet Inc.

Change Log
Date Change Description
2026-04-21 Initial release.
FortiManager 8.0.0 API Best Practices Guide 4
Fortinet Inc.

Introduction
The FortiManager JSON API allows you to perform configuration and monitoring operations on a FortiManager
appliance or VM. The JSON API is based on JSON-RPC, a remote procedure call protocol encoded in JSON. It
provides access to a wide range of features, including device management, policy configuration, object
manipulation, and task execution.
By using structured JSON requests over HTTPS, you can efficiently interact with FortiManager to streamline
operations, and scale network management tasks across multiple Fortinet devices.
This document contains the following information:
l Getting started on page 6
l Best Practices on page 7
FortiManager 8.0.0 API Best Practices Guide 5
Fortinet Inc.

Getting started
Getting started
1. Access the Fortinet Developer Network: Find detailed information about the FortiManager API on the
Fortinet Developer Network site, https://fndn.fortinet.net. The Fortinet Developer Network site provides
documentation for getting started with FortiManager APIs, setting up API users, making API requests, and
more.
Access to the Fortinet Developer Network requires an account. Please contact your Fortinet
representative or Fortinet support for more information on getting account access to the
Fortinet Developer Network.
2. Create an API User: Configure a FortiManager API user. The type of API user required depends on the type
of authentication you will be using:
REST API Admin with a
predefined API key
1. Create a Rest API Admin with Read/Read-Write privileges configured for
JSON API Access.
2. Apply an Admin Profile with the required permissions to complete the
desired requests.
3. Copy the automatically generated API key to use in the request header
using the bearer authentication scheme.
The generated API key is permanent, which means the same
user account will always share the same session and you do
not need to use thelogin/logout endpoints. Because of this
benefit, predefined API keys are useful for simplifying
automation workflows.
Session-based
authentication
1. Create a regular administrator with Read/Read-Write privileges
configured for JSON API Access.
2. Apply an Admin Profile with the required permissions to complete the
desired requests.
3. Use thelogin endpoint with the configured administrator credentials to
generate the session ID to be used within API requests.
3. Use a lab environment to test APIs: When using FortiManager APIs, it is recommended to begin first in a
lab environment to test and validate workflows before deployment to production.
FortiManager 8.0.0 API Best Practices Guide 6
Fortinet Inc.

Best Practices
Best Practices
FortiManager offers powerful APIs to automate and integrate with your network and security management
workflows. Follow these best practices to ensure secure, efficient, and reliable use.
1. Authentication and access control on page 7
2. API requests on page 8
3. Error handling and resilience on page 13
4. Performance optimization on page 13
5. Security and compliance on page 21
6. Change management on page 22
7. Support and resources on page 22
Authentication and access control
Best practice Description
Uselogout requests when
using session-based
authentication
l Sendinglogout requests when using FortiManager’s JSON API is
critical to free up session slots. Neglecting this can inadvertently fill up
all available admin sessions, either at the individual user or system
level, resulting in denial of management access until sessions expire or
are forcefully ended.
l This is controlled by configurable parameters at both the user
(system.admin.user.login-max) and system level
(system.admin.setting.admin-max-login).
Example: Making a logout request
{
"method": "exec",
"params": [
{
"url": "/sys/logout"
}
],
"session": "...",
"id": 1
}
Leverage role-based access
control
l Use RBAC (Role-Based Access Control) to restrict API access to only
required resources. You can configure RBAC for your API administrator
using Admin Profiles configured in the FortiManager. For more
FortiManager 8.0.0 API Best Practices Guide 7
Fortinet Inc.

Best Practices
Best practice Description
information, see Administrator Profiles.
Restrict network access
l Limit access with an IP allowlist or using trusted source control. See
Trusted hosts.
l When additional flexibility is required (for example, if you require more
than 10 trusted hosts), you can leverage system.local-in policies.
API requests
Best practice Description
Use HTTPS l Use HTTPS only for all API communications.
Use proper formatting
l Structure requests using proper JSON format and required parameters.
Example: HTTP method and URL
POST https://<fmg_ip>/jsonrpc
Example: Standard JSON RPC body format
{
"method": "...",
"params": [ ... ],
"session": "...",
"id": 1
}
Leverage and monitor
asynchronous APIs
l Take advantage of the asynchronous mechanism available in certain
JSON API URLs.
If the API supports task creation, include flags likecreate_task and
nonblocking in your JSON request to initiate a new task. This approach
allows you to send multiple JSON requests sequentially, each creating
its own task. You can then wait for all tasks to finish asynchronously.
A common use case is adding multiple devices using separate JSON
requests. Alternatively, using theadd/dev-list method is also an
effective option.
FortiManager 8.0.0 API Best Practices Guide 8
Fortinet Inc.

Best Practices
Best practice Description
Example: Refresh multiple devices
{
"params": [
{
"url": "/dvm/cmd/update/dev-list",
"data": {
"adom": "root"
"flags": [
"create_task",
"nonblocking"
],
"update-dev-member-list": [
{
"name": "fgt_1"
},
{
"name": "hub_1"
},
{
"name": "hub_2"
}
]
}
}
],
"session": "...",
"id": 1
}
l Monitor asynchronous APIs to ensure task completion. If youlogout
while an API request run asynchronously, it might be reported as failed
in the Task Manager because FortiManager can't find a valid API
session to complete the task.
FortiManager 8.0.0 API Best Practices Guide 9
Fortinet Inc.

Best Practices
Best practice Description
Example: Monitor asynchronous APIs
{
"method": "get",
"params": [
{
"url": "/task/task/1"
}
],
"session": "...",
"id": 1,
"verbose": 1
}
Use option names in requests
l For any attribute that is defined as an option, always use name of
option instead of internal index value in JSON request. The name is
more intuitive and when used, it produces more readable and easier to
understand code.
For example, forfirewall/policy the action can includedeny,accept,
ipsec.
FortiManager 8.0.0 API Best Practices Guide 10
Fortinet Inc.

Best Practices
Best practice Description
Example: Using option names in a request
{
"method": "add",
"params": [
{
"url": "/pm/config/adom/root/pkg/test_
pkg/firewall/policy",
"data": [
{
"name": "Allow_HTTP",
"srcintf": "port1",
"dstintf": "port2",
"srcaddr": "all",
"dstaddr": "all",
"action": "accept",
"schedule": "always",
"service": "HTTP",
"logtraffic": "all"
}
]
}
],
"session": "...",
"id": 1
}
l Avoid the use of the numeric index values, such as0,1,2.
FortiManager 8.0.0 API Best Practices Guide 11
Fortinet Inc.

Best Practices
Best practice Description
Avoid: Using numeric index values in request
{
"method": "add",
"params": [
{
"url": "/pm/config/adom/root/pkg/test_
pkg/firewall/policy",
"data": [
{
"name": "Allow_HTTP",
"srcintf": "port1",
"dstintf": "port2",
"srcaddr": "all",
"dstaddr": "all",
"action": 1,
"schedule": "always",
"service": "HTTP",
"logtraffic": "all"
}
]
}
],
"session": "...",
"id": 1
}
l Add "verbose": 1 in your get request to receive option names in the
response instead of internal index values.
Example: Using verbose output
{
"method": "get",
"params": [ ... ],
"session": "...",
"id": 1,
"verbose": 1
}
FortiManager 8.0.0 API Best Practices Guide 12
Fortinet Inc.

Best Practices
Error handling and resilience
Best practice Description
Monitor HTTP status codes
l Monitor standard HTTP status codes:
l 100: Request header received and waiting for the request body.
l 200: Success.
l 401: Invalid session or token.
l 403: Permission denied.
l 404: Not found. Unable to find the specified resource.
l 405: Method not allowed for this resource.
l 500: Internal error.
l 503: Service unavailable.
Use retry logic with
exponential backoff
l Implement retry logic with exponential backoff for transient errors to
prevent flooding the FortiManager with excessive repeat requests.
Log request/response for
auditing and troubleshooting
l Record the details of each API interaction, both in requests and
responses so that they can be assessed if an API calls or fails to return
data, and provide a clear record of the actions taken using the API.
Compare activities/tasks
performed from the GUI when
there are errors with the API
l Compare activities performed using the API with the same activities
performed using the GUI. Troubleshooting issues from the GUI
interface is an easier way to isolate problems and determine if the issue
is platform use/configuration or API related. If you have problems
performing a tasks using the GUI (for example, installs) then you'll likely
also face issues performing the same task using the API.
Performance optimization
Best practice Description
Limit request frequency l Avoid unnecessary polling or tight loops.
Use filter and fields
l Usefields,filter,range, andloadsub to retrieve only the necessary
data when querying large tables.
FortiManager 8.0.0 API Best Practices Guide 13
Fortinet Inc.

Best Practices
Best practice Description
Example: Getting a firewall address group containing member
"host_001"
{
"method": "get",
"params": [
{
"fields": [
"name",
"member"
],
"filter": [
"member",
"contain",
"host_001"
],
"loadsub": 0,
"url": "/pm/config/adom/root/obj/firewall/addrgrp"
}
],
"session": "...",
"id": 1
}
l Using thefilter parameter with thedelete method allows you to bulk
delete objects that match specific criteria. This can improve
performance when compared with deleting objects using individual
JSON requests. "confirm": 1 must be included in the JSON request to
confirm and execute the deletion.
FortiManager 8.0.0 API Best Practices Guide 14
Fortinet Inc.

Best Practices
Best practice Description
Example: Deleting objects matching a filter
{
"method": "delete",
"params": [
{
"confirm": 1,
"filter": [
"name",
"like",
"west%" <----- virtual IP that starts with
string west
],
"url": "/pm/config/adom/root/obj/firewall/vip"
}
],
"session": "...",
"id": 1
}
l Avoid loading full tables when only a subset of data is needed that
could be obtained using filter/fields.
Avoid: Loading full table when filter and fields can be used
{
"method": "get",
"params": [
{
"url": "/pm/config/adom/root/obj/firewall/addrgrp"
}
],
"session": "...",
"id": 1,
"verbose": 1
}
Cache data l Cache static or rarely changing data locally when appropriate.
Use object URLs for specific
items
l Access specific objects using the full object URL.
FortiManager 8.0.0 API Best Practices Guide 15
Fortinet Inc.

Best Practices
Best practice Description
Example: Using an object's URL
{
"method": "get",
"params": [
{
"url":
"/pm/config/adom/root/obj/firewall/address/Vancouver"
}
],
"session": "...",
"id": 1,
"verbose": 1
}
l Avoid using general table URLs when you only need access to a
specific object.
Avoid: Using general table URLs to access specific objects
{
"method": "get",
"params": [
{
"url": "/pm/config/adom/root/obj/firewall/address"
}
],
"session": "...",
"id": 1,
"verbose": 1
}
Leverage the target attribute
when using/sys/proxy/json
for multi-device FortiOS REST
API requests
l In order to send FortiOS REST API requests to multiple FortiGates at the
same time, use the JSON URL/sys/proxy/json and put all device
names in the target to achieve the asynchronous effect.
FortiManager 8.0.0 API Best Practices Guide 16
Fortinet Inc.

Best Practices
Best practice Description
Example: Using /sys/proxy/json
{
"method": "exec",
"params": [
{
"data": {
"action": "get",
"resource": "/api/v2/monitor/router/ipv4"
"target": [
"adom/root/device/fgt1"
]
},
"url": "sys/proxy/json"
}
],
"session": "...",
"id": 1
}
Example: Setting the target when there are multiple
devices/device groups
"target": [
"adom/root/group/emea_devices",
"adom/root/group/apac_devices",
"adom/root/device/fgt1",
"adom/root/device/fgt2"
]
Monitor system resources
l The number of JSON API tasks that can run simultaneously depends on
the currently available resources. This includes the free memory size,
number of CPUs, clock speed of CPU, free disk size, etc. Avoid running
too many tasks simultaneously without considering system limitations.
Clone large objects
l Cloning is faster than manually recreating complex objects and all sub-
objects from scratch
FortiManager 8.0.0 API Best Practices Guide 17
Fortinet Inc.

Best Practices
Best practice Description
Example: Cloning an ADOM
request = {
"method": "clone",
"params": [
{
"data": {
"name": "adom2"
},
"url": "/dvmdb/adom/adom1"
}
],
"session": "...",
"id": 1
}
Create a baseline
l Baseline your FortiManager with and without your API client tools
running. Depending on how you use FortiManager you might find
performance issues by creating a baseline with GUI only so you can
determine if your API calls are negatively impacting the system.
Use high-availability mode to
distribute installations
l If installation operations are slow, configure a FortiManager HA cluster
and enableperform-improve-by-ha {enable | disable} to distribute
the workload of installation tasks. See the CLI Reference for more
information.
Minimize table size to improve
performance in the GUI
l Consider how much data your tables present considering the API can
exponentially grow tables with ease. If possible, moving to a strategy
that creates fewer objects with common names but adds per-device
mapping per object will decrease the number of objects in the table
and therefore improve the experience in the GUI.
l As an example, if your address table(s) within an ADOM has tens or
hundreds of thousands of objects, you might notice a slowdown in
presenting this data in the GUI while the API with a proper filter does
not.
Optimize FortiManager
architecture for API efficiency
l Plan your FortiManager and FortiOS architecture before starting API
development to reduce complexity and technical debt. Leverage built-
in features like FortiManager variables and dynamic CLI/Jinja scripts to
minimize API calls and simplify configuration management.
l If you find yourself updating lots of objects in the form of lists and/or
groups that are time sensitive, consider creating dynamic lists in the
FortiGate using the SDN connectors. See Public and private
SDN connectors.
FortiManager 8.0.0 API Best Practices Guide 18
Fortinet Inc.

Best Practices
Best practice Description
Use multiplexing to regroup
multiple APIs into a single
request when appropriate
l Multiplexing can be used to group multiple APIs operations into a single
request.
For example, you can use data multiplexing when you need to create
multiple firewall addresses using one API call.
Example: Creating multiple firewall addresses using data
multiplexing
{
"method": "add",
"params": [
{
"data": [
{
"name": "test_001",
"type": "ipmask",
"subnet": [
"10.0.0.1",
"255.255.255.0"
]
},
{
"name": "test_002",
"type": "ipmask",
"subnet": [
"10.0.0.2",
"255.255.255.0"
]
},
{
"name": "test_003",
"type": "ipmask",
"subnet": [
"10.0.0.3",
"255.255.255.0"
]
}
],
"url": "/pm/config/adom/root/obj/firewall/address"
}
],
"session": "...",
"id": 1
}
l Parameter multiplexing can also be used to improve performance when
FortiManager 8.0.0 API Best Practices Guide 19
Fortinet Inc.

Best Practices
Best practice Description
all the JSON URLs in the request have the same prefix, for example
/pm/config.
Example: Retrieving information about specific objects in an
ADOM using parameter multiplexing
{
"method": "get",
"params": [
{
"url": "/pm/config/adom/root/obj/webfilter/profile",
"fields": ["name"],
"loadsub": 0
},
{
"url": "/pm/config/adom/root/obj/firewall/address",
"fields": ["name"],
"loadsub": 0
},
{
"url":
"/pm/config/adom/root/obj/firewall/service/group",
"fields": ["name"],
"loadsub": 0
}
],
"session": "...",
"id": 1
}
l Using parameter multiplexing in requests that contain URLs with
different prefixes is not supported.
FortiManager 8.0.0 API Best Practices Guide 20
Fortinet Inc.

Best Practices
Best practice Description
Avoid: Using parameter multiplexing with requests that
include different URL prefixes
{
"method": "get",
"params": [
{
"fields": [
"name"
],
"loadsub": 0,
"url": "/dvmdb/group"
},
{
"fields": [
"name"
],
"loadsub": 0,
"url": "/pm/config/adom/root/obj/firewall/address"
}
],
"session": "...",
"id": 1
}
When working in Workspace
mode, lock the ADOM once,
perform all necessary
changes, then commit and
unlock the ADOM
l Each commit performed in Workspace mode triggers a database copy
operation. To reduce the time taken to perform each copy operation,
plan your changes ahead, group them logically, and apply them in one
session before committing.
For example, lock the ADOM, add multiple objects, commit the
changes, and unlock the ADOM.
l Avoid locking the ADOM, making a single change, committing,
unlocking the ADOM, and repeating this process multiple times.
Security and compliance
Best practice Description
Change passwords and API
sessions
l Change admin passwords periodically to reduce the risk of
unauthorized access and exposure from compromised credentials.
l Rotate API sessions regularly by performinglogout andlogin
requests.
FortiManager 8.0.0 API Best Practices Guide 21
Fortinet Inc.

Best Practices
Best practice Description
Don't embed credentials l Avoid embedding credentials in scripts or config files.
Monitor your API activity
l Use the FortiManager Event Log to monitor API activity for anomalies.
For more information, see Event Log.
You can configure JSON API logging using theset jsonapi-log
command in the FortiManager CLI:
config system global
(global)# set jsonapi-log
all logging both jsonapi request & response.
disable disable jsonapi log.
request logging jsonapi request.
response logging jsonapi response.
Change management
Best practice Description
Maintain version compatibility
l Maintain version compatibility during FortiManager upgrades. Consult
the FortiManager Release Notes to verify which managed devices are
supported by your FortiManager version.
Support and resources
Best practice Description
Consult the Fortinet Developer
Network (FNDN)
l Refer to https://fndn.fortinet.net for API docs and tools.
l Join the Fortinet Developer Community for questions and examples.
Compare APIs between two
FortiManager versions using
the API Comparison feature
l Use the API Comparison feature in FortiAPI on the Fortinet Developer
Network to compare API information between two FortiManager
versions, and see APIs that have been updated, removed, or added.
FortiManager 8.0.0 API Best Practices Guide 22
Fortinet Inc.

Best Practices
Best practice Description
Contact Fortinet Support for
further assistance
l Contact Fortinet Support for API-related assistance, or use the
Fortinet Support Community to find answers to questions in the articles
and support forums.
FortiManager 8.0.0 API Best Practices Guide 23
Fortinet Inc.

www.fortinet.com
Copyright© 2026 Fortinet, Inc. All rights reserved. Fortinet®, FortiGate®, FortiCare® and FortiGuard®, and certain other marks are registered trademarks of Fortinet, Inc., and other Fortinet
names herein may also be registered and/or common law trademarks of Fortinet. All other product or company names may be trademarks of their respective owners. Performance and other
metrics contained herein were attained in internal lab tests under ideal conditions, and actual performance and other results may vary. Network variables, different network environments and
other conditions may affect performance results. Nothing herein represents any binding commitment by Fortinet, and Fortinet disclaims all warranties, whether express or implied, except to
the extent Fortinet enters a binding written contract, signed by Fortinet’s Chief Legal Officer, with a purchaser that expressly warrants that the identified product will perform according to
certain expressly-identified performance metrics and, in such event, only the specific performance metrics expressly identified in such binding written contract shall be binding on Fortinet.
For absolute clarity, any such warranty will be limited to performance in the same ideal conditions as in Fortinet’s internal lab tests. Fortinet disclaims in full any covenants, representations,
and guarantees pursuant hereto, whether express or implied. Fortinet reserves the right to change, modify, transfer, or otherwise revise this publication without notice, and the most current
version of the publication shall be applicable.