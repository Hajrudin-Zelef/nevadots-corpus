This project originally was taken on in order for me to build an MCP server to connect to my UniFi network devices. Unfortunately, there was not any good documentation to support this, so I started on a quest of identifying all of the API endpoints that a controller or device could have.
UniFi uses multiple different types of APIs and authentication. In my review, I researched the backend APIs, the frontend JavaScripts, and also their new official network API and compiled them into a single place here.
I hope this to be a comprehensive, community-driven documentation project for UniFi Network Controller APIs (v9.4+). This repository contains detailed OpenAPI specifications reverse-engineered from the UniFi Network Controller interface.
This project aims to document all available APIs in the UniFi Network ecosystem through collaborative effort. The API specifications have been compiled using multiple reverse engineering methods including:
- Analysis of UniFi Controller web interface network traffic
- Inspection of JavaScript bundles and UI components
- Official documentation cross-referencing
- Community testing and validation
# Clone the repository
git clone https://github.com/tmcpro/unifi-network-api.git
cd unifi-network-api
# Start a local HTTP server
python -m http.server 8000
# Open your browser to view the documentation
# http://localhost:8000
Simply open the index.html file in your web browser to view the OpenAPI documentation. This is loading a remote JS to render the OpenAPI Spec.
# Install Redoc CLI
npm install -g redoc-cli
# Generate static documentation
redoc-cli serve openapi/openapi.yaml --watch
# Or build static HTML
redoc-cli build openapi/openapi.yaml --output docs/index.html
The UniFi Network Controller supports multiple authentication methods depending on the API version and use case:
- Path Pattern: /integration/v1/*
- Authentication: X-API-Key header
- Setup: Generate API keys in the UniFi Controller under Settings → Integrations
curl -H "X-API-Key: YOUR_API_KEY" \
  "https://{{your-controller}}/proxy/network/integration/v1/sites"
- Path Pattern: /v2/api/* and/api/*
- Authentication: Session cookies + CSRF tokens
- Use Case: Browser-based applications, advanced controller features
- Setup: Login via /api/login endpoint
# Login and save cookies
curl -c cookies.jar -X POST \
  -H "Content-Type: application/json" \
  -d '{"username":"admin","password":"your_password"}' \
  "https://{{your-controller}}/proxy/network/api/login"
# Use cookies for subsequent requests
curl -b cookies.jar \
  "https://{{your-controller}}/proxy/network/v2/api/info"
- Integration v1: Limited subset focused on common operations (15 endpoints)
- Controller v2: Complete feature set with full controller functionality (227+ endpoints)
- v1 Responses: Include pagination metadata (offset ,limit ,count ,totalCount )
- v2 Responses: Use data +meta envelope pattern
All APIs return structured error responses:
{
  "code": "ERROR_CODE",
  "message": "Human readable error description",
  "details": {}
}
API endpoint availability depends on your UniFi hardware and firmware:
- UniFi Dream Machine (UDM/UDM Pro): Likely Full API support
- CloudKey Gen2+: Likely API support
- Self-hosted Controllers: Likely Partial API support
- Legacy Hardware: Limited endpoint availability
- UniFi OS vs Traditional Controller: Some endpoints may differ
Note: Always test endpoints with your specific hardware configuration before production deployment.
We encourage community contributions to improve and validate this documentation:
- Fork this repository
- Test API endpoints with your UniFi setup
- Document your findings (success/failure, hardware compatibility)
- Submit pull requests with improvements
- Report issues for incorrect or missing documentation
- Validate endpoints before submitting changes
- Include hardware/firmware version information
- Provide example requests and responses
- Follow the existing OpenAPI specification format
- Add appropriate tags and descriptions
- Validate all documented endpoints across different hardware
- Expand coverage of undocumented APIs
- Improve example requests and responses
- Document hardware-specific limitations
- Create SDKs and client libraries
The following table lists all 327 documented API endpoints organized by category:
| Category | Endpoint | Method | Description | 
|---|---|---|---|
| About Application | /v1/info | GET | Get Application Info | 
| Sites | /v1/sites | GET | List Local Sites | 
| UniFi Devices | /v1/sites/{siteId}/devices | GET | List Site Devices | 
|  | /v1/sites/{siteId}/devices/{deviceId} | GET | Get Device Details | 
|  | /v1/sites/{siteId}/devices/{deviceId}/statistics | GET | Get Device Statistics | 
|  | /v1/sites/{siteId}/devices/{deviceId}/actions | POST | Execute Device Action | 
|  | /v1/sites/{siteId}/devices/{deviceId}/interfaces/ports/{portIdx}/actions | POST | Execute Port Action | 
| Clients | /v1/sites/{siteId}/clients | GET | List Site Clients | 
|  | /v1/sites/{siteId}/clients/{clientId} | GET | Get Client Details | 
|  | /v1/sites/{siteId}/clients/{clientId}/actions | POST | Execute Client Action | 
| Hotspot Vouchers | /v1/sites/{siteId}/hotspot/vouchers | GET | List Vouchers | 
|  | /v1/sites/{siteId}/hotspot/vouchers | POST | Create Voucher | 
|  | /v1/sites/{siteId}/hotspot/vouchers/{voucherId} | GET | Get Voucher Details | 
|  | /v1/sites/{siteId}/hotspot/vouchers/{voucherId} | DELETE | Delete Voucher | 
| Category | Endpoint | Method | Description | 
|---|---|---|---|
| Settings | /api/login | POST | Authenticate to Controller | 
|  | /api/logout | POST | End Session | 
|  | /v2/api/info | GET | Get Controller Info | 
|  | /v2/api/log-levels/defaults | GET | Get Default Log Levels | 
|  | /v2/api/timezones | GET | List Supported Timezones | 
|  | /v2/api/site/{site}/settings/mgmt | GET | Get Management Settings | 
|  | /v2/api/site/{site}/settings/mgmt | PUT | Update Management Settings | 
|  | /v2/api/site/{site}/settings/snmp | GET | Get SNMP Settings | 
|  | /v2/api/site/{site}/settings/snmp | PUT | Update SNMP Settings | 
|  | /v2/api/site/{site}/settings/radio-ai | GET | Get Radio AI Settings | 
|  | /v2/api/site/{site}/settings/radio-ai | PUT | Update Radio AI Settings | 
|  | /v2/api/site/{site}/settings/syslog | GET | Get Syslog Settings | 
|  | /v2/api/site/{site}/settings/syslog | PUT | Update Syslog Settings | 
|  | /v2/api/site/{site}/settings/super-mgmt | GET | Get Super Management Settings | 
|  | /v2/api/site/{site}/settings/super-mgmt | PUT | Update Super Management Settings | 
|  | /v2/api/site/{site}/settings/radius | GET | Get RADIUS Settings | 
|  | /v2/api/site/{site}/settings/radius | PUT | Update RADIUS Settings | 
|  | /v2/api/site/{site}/settings/mdns | GET | Get mDNS Settings | 
|  | /v2/api/site/{site}/settings/mdns | PUT | Update mDNS Settings | 
|  | /v2/api/site/{site}/settings/country | GET | Get Country Settings | 
|  | /v2/api/site/{site}/settings/country | PUT | Update Country Settings | 
|  | /v2/api/site/{site}/settings/locale | GET | Get Locale Settings | 
|  | /v2/api/site/{site}/settings/locale | PUT | Update Locale Settings | 
|  | /v2/api/site/{site}/settings/network-optimization | GET | Get Network Optimization Settings | 
|  | /v2/api/site/{site}/settings/network-optimization | PUT | Update Network Optimization Settings | 
|  | /v2/api/site/{site}/settings/baresip | GET | Get Baresip Settings | 
|  | /v2/api/site/{site}/settings/baresip | PUT | Update Baresip Settings | 
|  | /v2/api/site/{site}/settings/super-identity | GET | Get Super Identity Settings | 
|  | /v2/api/site/{site}/settings/super-identity | PUT | Update Super Identity Settings | 
|  | /v2/api/site/{site}/settings/super-mail | GET | Get Super Mail Settings | 
|  | /v2/api/site/{site}/settings/super-mail | PUT | Update Super Mail Settings | 
|  | /v2/api/site/{site}/settings/super-smtp | GET | Get Super SMTP Settings | 
|  | /v2/api/site/{site}/settings/super-smtp | PUT | Update Super SMTP Settings | 
|  | /v2/api/site/{site}/settings/super-sdn | GET | Get Super SDN Settings | 
|  | /v2/api/site/{site}/settings/super-sdn | PUT | Update Super SDN Settings | 
|  | /v2/api/site/{site}/settings/super-cloudaccess | GET | Get Super Cloud Access Settings | 
|  | /v2/api/site/{site}/settings/super-cloudaccess | PUT | Update Super Cloud Access Settings | 
|  | /v2/api/site/{site}/settings/usw | GET | Get USW Settings | 
|  | /v2/api/site/{site}/settings/usw | PUT | Update USW Settings | 
|  | /v2/api/site/{site}/settings/guest-access | GET | Get Guest Access Settings | 
|  | /v2/api/site/{site}/settings/guest-access | PUT | Update Guest Access Settings | 
|  | /v2/api/site/{site}/settings/connectivity | GET | Get Connectivity Settings | 
|  | /v2/api/site/{site}/settings/connectivity | PUT | Update Connectivity Settings | 
|  | /v2/api/site/{site}/settings/element-adopt | GET | Get Element Adoption Settings | 
|  | /v2/api/site/{site}/settings/element-adopt | PUT | Update Element Adoption Settings | 
|  | /v2/api/site/{site}/settings/auto-speedtest | GET | Get Auto Speedtest Settings | 
|  | /v2/api/site/{site}/settings/auto-speedtest | PUT | Update Auto Speedtest Settings | 
|  | /v2/api/site/{site}/settings/dpi | GET | Get DPI Settings | 
|  | /v2/api/site/{site}/settings/dpi | PUT | Update DPI Settings | 
|  | /v2/api/site/{site}/settings/porta | GET | Get Porta Settings | 
|  | /v2/api/site/{site}/settings/porta | PUT | Update Porta Settings | 
|  | /v2/api/site/{site}/settings/broadcast | GET | Get Broadcast Settings | 
|  | /v2/api/site/{site}/settings/broadcast | PUT | Update Broadcast Settings | 
|  | /v2/api/site/{site}/settings/usg | GET | Get USG Settings | 
|  | /v2/api/site/{site}/settings/usg | PUT | Update USG Settings | 
|  | /v2/api/site/{site}/settings/ntp | GET | Get NTP Settings | 
|  | /v2/api/site/{site}/settings/ntp | PUT | Update NTP Settings | 
|  | /v2/api/site/{site}/settings/ips | GET | Get IPS Settings | 
|  | /v2/api/site/{site}/settings/ips | PUT | Update IPS Settings | 
|  | /v2/api/site/{site}/settings/teleport | GET | Get Teleport Settings | 
|  | /v2/api/site/{site}/settings/teleport | PUT | Update Teleport Settings | 
|  | /v2/api/site/{site}/settings/super-fwupdate | GET | Get Super Firmware Update Settings | 
|  | /v2/api/site/{site}/settings/super-fwupdate | PUT | Update Super Firmware Update Settings | 
|  | /v2/api/site/{site}/settings/lcm | GET | Get LCM Settings | 
|  | /v2/api/site/{site}/settings/lcm | PUT | Update LCM Settings | 
|  | /v2/api/site/{site}/settings/global-ap | GET | Get Global AP Settings | 
|  | /v2/api/site/{site}/settings/global-ap | PUT | Update Global AP Settings | 
|  | /v2/api/site/{site}/settings/global-network | PUT | Update Global Network Settings | 
|  | /v2/api/site/{site}/settings/global-switch | PUT | Update Global Switch Settings | 
|  | /v2/api/site/{site}/settings/magic-site-to-site-vpn | GET | Get Magic Site-to-Site VPN Settings | 
|  | /v2/api/site/{site}/settings/magic-site-to-site-vpn | PUT | Update Magic Site-to-Site VPN Settings | 
|  | /v2/api/site/{site}/settings/doh | GET | Get DNS over HTTPS Settings | 
|  | /v2/api/site/{site}/settings/doh | PUT | Update DNS over HTTPS Settings | 
|  | /v2/api/site/{site}/settings/ether-lighting | GET | Get Ethernet Lighting Settings | 
|  | /v2/api/site/{site}/settings/ether-lighting | PUT | Update Ethernet Lighting Settings | 
|  | /v2/api/site/{site}/settings/evaluation-score | GET | Get Evaluation Score Settings | 
|  | /v2/api/site/{site}/settings/evaluation-score | PUT | Update Evaluation Score Settings | 
|  | /v2/api/site/{site}/settings/ssl-inspection | GET | Get SSL Inspection Settings | 
|  | /v2/api/site/{site}/settings/ssl-inspection | PUT | Update SSL Inspection Settings | 
|  | /v2/api/site/{site}/settings/dashboard | GET | Get Dashboard Settings | 
|  | /v2/api/site/{site}/settings/dashboard | PUT | Update Dashboard Settings | 
|  | /v2/api/site/{site}/settings/global-nat | GET | Get Global NAT Settings | 
|  | /v2/api/site/{site}/settings/global-nat | PUT | Update Global NAT Settings | 
|  | /v2/api/site/{site}/settings/netflow | GET | Get NetFlow Settings | 
|  | /v2/api/site/{site}/settings/netflow | PUT | Update NetFlow Settings | 
|  | /v2/api/site/{site}/settings/traffic-flow | GET | Get Traffic Flow Settings | 
|  | /v2/api/site/{site}/settings/traffic-flow | PUT | Update Traffic Flow Settings | 
|  | /v2/api/site/{site}/settings/roaming-assistant | GET | Get Roaming Assistant Settings | 
|  | /v2/api/site/{site}/settings/roaming-assistant | PUT | Update Roaming Assistant Settings | 
| Category | Endpoint | Method | Description | 
|---|---|---|---|
| Networks | /v2/api/site/{site}/networks/lan | GET | List LAN Networks | 
|  | /v2/api/site/{site}/networks/lan | POST | Create LAN Network | 
|  | /v2/api/site/{site}/networks/lan/{id} | GET | Get LAN Network Details | 
|  | /v2/api/site/{site}/networks/lan/{id} | PUT | Update LAN Network | 
|  | /v2/api/site/{site}/networks/lan/{id} | DELETE | Delete LAN Network | 
|  | /v2/api/site/{site}/networks/wan | GET | List WAN Networks | 
|  | /v2/api/site/{site}/networks/wan | POST | Create WAN Network | 
|  | /v2/api/site/{site}/networks/wan/{id} | GET | Get WAN Network Details | 
|  | /v2/api/site/{site}/networks/wan/{id} | PUT | Update WAN Network | 
|  | /v2/api/site/{site}/networks/wan/{id} | DELETE | Delete WAN Network | 
|  | /v2/api/site/{site}/networks/wifi | GET | List WiFi Networks | 
|  | /v2/api/site/{site}/networks/wifi | POST | Create WiFi Network | 
|  | /v2/api/site/{site}/networks/wifi/{id} | GET | Get WiFi Network Details | 
|  | /v2/api/site/{site}/networks/wifi/{id} | PUT | Update WiFi Network | 
|  | /v2/api/site/{site}/networks/wifi/{id} | DELETE | Delete WiFi Network | 
| Category | Endpoint | Method | Description | 
|---|---|---|---|
| Devices | /v2/api/site/{site}/devices | GET | List All Devices | 
|  | /v2/api/site/{site}/devices | POST | Adopt Device | 
|  | /v2/api/site/{site}/devices/{mac} | GET | Get Device Details | 
|  | /v2/api/site/{site}/devices/{mac} | PUT | Update Device | 
|  | /v2/api/site/{site}/devices/{mac} | DELETE | Forget/Remove Device | 
|  | /v2/api/site/{site}/devices/command | POST | Bulk Device Command | 
| Category | Endpoint | Method | Description | 
|---|---|---|---|
| Clients | /v2/api/site/{site}/clients | GET | List All Clients | 
|  | /v2/api/site/{site}/clients/active | GET | List Active Clients | 
|  | /v2/api/site/{site}/clients/history | GET | List Historical Clients | 
|  | /v2/api/site/{site}/clients/{mac} | GET | Get Client Details | 
|  | /v2/api/site/{site}/clients/{mac} | PUT | Update Client | 
|  | /v2/api/site/{site}/clients/{mac} | DELETE | Forget Client | 
| Category | Endpoint | Method | Description | 
|---|---|---|---|
| WiFi Stats | /v2/api/site/{site}/wifi-stats/radios | GET | Get Radio Statistics | 
|  | /v2/api/site/{site}/wifi-stats/performance | GET | Get WiFi Performance Summary | 
| Category | Endpoint | Method | Description | 
|---|---|---|---|
| System Log | /v2/api/site/{site}/system-log | GET | Get System Log Events | 
| Category | Endpoint | Method | Description | 
|---|---|---|---|
| Traffic | /v2/api/site/{site}/traffic/overview | GET | Get Traffic Overview | 
|  | /v2/api/site/{site}/traffic/flows | GET | Get Traffic Flows | 
|  | /v2/api/site/{site}/traffic/apps | GET | Get Application Traffic | 
| Category | Endpoint | Method | Description | 
|---|---|---|---|
| ACL | /v2/api/site/{site}/acl-rules | GET | List ACL Rules | 
|  | /v2/api/site/{site}/acl-rules | POST | Create ACL Rule | 
|  | /v2/api/site/{site}/acl-rules/{id} | GET | Get ACL Rule Details | 
|  | /v2/api/site/{site}/acl-rules/{id} | PUT | Update ACL Rule | 
|  | /v2/api/site/{site}/acl-rules/{id} | DELETE | Delete ACL Rule | 
| Category | Endpoint | Method | Description | 
|---|---|---|---|
| QoS | /v2/api/site/{site}/qos-rules | GET | List QoS Rules | 
|  | /v2/api/site/{site}/qos-rules | POST | Create QoS Rule | 
|  | /v2/api/site/{site}/qos-rules/{id} | GET | Get QoS Rule Details | 
|  | /v2/api/site/{site}/qos-rules/{id} | PUT | Update QoS Rule | 
|  | /v2/api/site/{site}/qos-rules/{id} | DELETE | Delete QoS Rule | 
| Category | Endpoint | Method | Description | 
|---|---|---|---|
| Firewall | /v2/api/site/{site}/firewall-rules | GET | List Firewall Rules | 
|  | /v2/api/site/{site}/firewall-rules | POST | Create Firewall Rule | 
|  | /v2/api/site/{site}/firewall-rules/{id} | GET | Get Firewall Rule Details | 
|  | /v2/api/site/{site}/firewall-rules/{id} | PUT | Update Firewall Rule | 
|  | /v2/api/site/{site}/firewall-rules/{id} | DELETE | Delete Firewall Rule | 
|  | /v2/api/site/{site}/firewall-groups | GET | List Firewall Groups | 
|  | /v2/api/site/{site}/firewall-groups | POST | Create Firewall Group | 
|  | /v2/api/site/{site}/firewall-groups/{id} | GET | Get Firewall Group Details | 
|  | /v2/api/site/{site}/firewall-groups/{id} | PUT | Update Firewall Group | 
|  | /v2/api/site/{site}/firewall-groups/{id} | DELETE | Delete Firewall Group | 
| Category | Endpoint | Method | Description | 
|---|---|---|---|
| Port Profiles | /v2/api/site/{site}/port-profiles | GET | List Port Profiles | 
|  | /v2/api/site/{site}/port-profiles | POST | Create Port Profile | 
|  | /v2/api/site/{site}/port-profiles/{id} | GET | Get Port Profile Details | 
|  | /v2/api/site/{site}/port-profiles/{id} | PUT | Update Port Profile | 
|  | /v2/api/site/{site}/port-profiles/{id} | DELETE | Delete Port Profile | 
|  | /v2/api/site/{site}/port-profiles/defaults | GET | Get Port Profile Defaults | 
| Category | Endpoint | Method | Description | 
|---|---|---|---|
| Port Management | /v2/api/site/{site}/devices/{mac}/ports/{portIndex} | GET | Get Port Configuration | 
|  | /v2/api/site/{site}/devices/{mac}/ports/{portIndex} | PUT | Update Port Configuration | 
| Category | Endpoint | Method | Description | 
|---|---|---|---|
| Routing | /v2/api/site/{site}/static-routes | GET | List Static Routes | 
|  | /v2/api/site/{site}/static-routes | POST | Create Static Route | 
|  | /v2/api/site/{site}/static-routes/{id} | GET | Get Static Route Details | 
|  | /v2/api/site/{site}/static-routes/{id} | PUT | Update Static Route | 
|  | /v2/api/site/{site}/static-routes/{id} | DELETE | Delete Static Route | 
| Category | Endpoint | Method | Description | 
|---|---|---|---|
| WAN | /v2/api/site/{site}/port-forward-advanced | GET | List Advanced Port Forwarding Rules | 
|  | /v2/api/site/{site}/port-forward-advanced | POST | Create Advanced Port Forwarding Rule | 
|  | /v2/api/site/{site}/port-forward-advanced/{id} | GET | Get Advanced Port Forwarding Rule Details | 
|  | /v2/api/site/{site}/port-forward-advanced/{id} | PUT | Update Advanced Port Forwarding Rule | 
|  | /v2/api/site/{site}/port-forward-advanced/{id} | DELETE | Delete Advanced Port Forwarding Rule | 
| Category | Endpoint | Method | Description | 
|---|---|---|---|
| Floorplan | /v2/api/site/{site}/maps | GET | List Site Maps | 
|  | /v2/api/site/{site}/maps | POST | Create Site Map | 
|  | /v2/api/site/{site}/maps/{id} | GET | Get Site Map Details | 
|  | /v2/api/site/{site}/maps/{id} | PUT | Update Site Map | 
|  | /v2/api/site/{site}/maps/{id} | DELETE | Delete Site Map | 
|  | /v2/api/site/{site}/walls | GET | List Walls | 
|  | /v2/api/site/{site}/walls | POST | Create Wall | 
|  | /v2/api/site/{site}/walls/{id} | GET | Get Wall Details | 
|  | /v2/api/site/{site}/walls/{id} | PUT | Update Wall | 
|  | /v2/api/site/{site}/walls/{id} | DELETE | Delete Wall | 
| Category | Endpoint | Method | Description | 
|---|---|---|---|
| Legacy Config | /api/s/{site}/stat/sites | GET | List Sites Statistics | 
|  | /api/s/{site}/stat/device | GET | List Device Statistics | 
|  | /api/s/{site}/stat/client | GET | List Client Statistics | 
|  | /api/s/{site}/stat/sessions | GET | Get Session Statistics | 
|  | /api/s/{site}/list/user | GET | List Users | 
|  | /api/s/{site}/list/usergroup | GET | List User Groups | 
|  | /api/s/{site}/list/wlanconf | GET | List WLAN Configurations | 
|  | /api/s/{site}/list/networkconf | GET | List Network Configurations | 
|  | /api/s/{site}/list/firewallrule | GET | List Firewall Rules | 
|  | /api/s/{site}/list/firewallgroup | GET | List Firewall Groups | 
|  | /api/s/{site}/list/portconf | GET | List Port Configurations | 
|  | /api/s/{site}/list/radiusprofile | GET | List RADIUS Profiles | 
|  | /api/s/{site}/list/account | GET | List Accounts | 
|  | /api/s/{site}/list/wlangroup | GET | List WLAN Groups | 
|  | /api/s/{site}/list/rogueap | GET | List Rogue Access Points | 
|  | /api/s/{site}/list/known_rogueap | GET | List Known Rogue Access Points | 
|  | /api/s/{site}/list/event | GET | List Events | 
|  | /api/s/{site}/list/alarm | GET | List Alarms | 
|  | /api/s/{site}/list/hotspotop | GET | List Hotspot Operators | 
|  | /api/s/{site}/list/hotspotpackage | GET | List Hotspot Packages | 
|  | /api/s/{site}/list/voucher | GET | List Vouchers | 
|  | /api/s/{site}/list/self | GET | Get Self Information | 
|  | /api/s/{site}/list/settings | GET | List Settings | 
|  | /api/s/{site}/get/setting/{section} | GET | Get Setting Section | 
|  | /api/s/{site}/list/health | GET | List Health Status | 
|  | /api/s/{site}/list/dashboard | GET | List Dashboard Data | 
|  | /api/s/{site}/list/portforward | GET | List Port Forwarding Rules | 
|  | /api/s/{site}/list/dynamicdns | GET | List Dynamic DNS Configurations | 
|  | /api/s/{site}/list/portprofile | GET | List Port Profiles | 
|  | /api/s/{site}/list/dpi_stats | GET | List DPI Statistics | 
|  | /api/s/{site}/list/dpi_stats_fields | GET | List DPI Statistics Fields | 
|  | /api/s/{site}/list/dpiapp | GET | List DPI Applications | 
|  | /api/s/{site}/list/dpigroup | GET | List DPI Groups | 
|  | /api/s/{site}/list/current_channels | GET | List Current Channels | 
|  | /api/s/{site}/list/country_codes | GET | List Country Codes | 
|  | /api/s/{site}/list/admins | GET | List Administrators | 
|  | /api/s/{site}/stat/admin | GET | Get Admin Statistics | 
|  | /api/s/{site}/cmd/evtmgr | POST | Event Manager Command | 
|  | /api/s/{site}/cmd/stamgr | POST | Station Manager Command | 
|  | /api/s/{site}/set/setting/{section} | POST | Update Setting Section | 
|  | /api/s/{site}/rest/user | POST | Create User | 
|  | /api/s/{site}/rest/usergroup | POST | Create User Group | 
|  | /api/s/{site}/rest/wlanconf | POST | Create WLAN Configuration | 
|  | /api/s/{site}/rest/networkconf | POST | Create Network Configuration | 
|  | /api/s/{site}/rest/firewallrule | POST | Create Firewall Rule | 
|  | /api/s/{site}/rest/firewallgroup | POST | Create Firewall Group | 
|  | /api/s/{site}/rest/portconf | POST | Create Port Configuration | 
|  | /api/s/{site}/rest/radiusprofile | POST | Create RADIUS Profile | 
|  | /api/s/{site}/rest/account | POST | Create Account | 
|  | /api/s/{site}/rest/wlangroup | POST | Create WLAN Group | 
|  | /api/s/{site}/rest/hotspotop | POST | Create Hotspot Operator | 
|  | /api/s/{site}/rest/hotspotpackage | POST | Create Hotspot Package | 
|  | /api/s/{site}/rest/voucher | POST | Create Voucher | 
|  | /api/s/{site}/rest/portforward | POST | Create Port Forwarding Rule | 
|  | /api/s/{site}/rest/dynamicdns | POST | Create Dynamic DNS Configuration | 
|  | /api/s/{site}/rest/portprofile | POST | Create Port Profile | 
|  | /api/s/{site}/rest/dpiapp | POST | Create DPI Application | 
|  | /api/s/{site}/rest/dpigroup | POST | Create DPI Group | 
|  | /api/s/{site}/upd/user/{id} | PUT | Update User | 
|  | /api/s/{site}/upd/usergroup/{id} | PUT | Update User Group | 
|  | /api/s/{site}/upd/wlanconf/{id} | PUT | Update WLAN Configuration | 
|  | /api/s/{site}/upd/networkconf/{id} | PUT | Update Network Configuration | 
|  | /api/s/{site}/upd/firewallrule/{id} | PUT | Update Firewall Rule | 
|  | /api/s/{site}/upd/firewallgroup/{id} | PUT | Update Firewall Group | 
|  | /api/s/{site}/upd/portconf/{id} | PUT | Update Port Configuration | 
|  | /api/s/{site}/upd/radiusprofile/{id} | PUT | Update RADIUS Profile | 
|  | /api/s/{site}/upd/account/{id} | PUT | Update Account | 
|  | /api/s/{site}/upd/wlangroup/{id} | PUT | Update WLAN Group | 
|  | /api/s/{site}/upd/hotspotop/{id} | PUT | Update Hotspot Operator | 
|  | /api/s/{site}/upd/hotspotpackage/{id} | PUT | Update Hotspot Package | 
|  | /api/s/{site}/upd/portforward/{id} | PUT | Update Port Forwarding Rule | 
|  | /api/s/{site}/upd/dynamicdns/{id} | PUT | Update Dynamic DNS Configuration | 
|  | /api/s/{site}/upd/portprofile/{id} | PUT | Update Port Profile | 
|  | /api/s/{site}/upd/dpiapp/{id} | PUT | Update DPI Application | 
|  | /api/s/{site}/upd/dpigroup/{id} | PUT | Update DPI Group | 
|  | /api/s/{site}/del/user/{id} | DELETE | Delete User | 
|  | /api/s/{site}/del/usergroup/{id} | DELETE | Delete User Group | 
|  | /api/s/{site}/del/wlanconf/{id} | DELETE | Delete WLAN Configuration | 
|  | /api/s/{site}/del/networkconf/{id} | DELETE | Delete Network Configuration | 
|  | /api/s/{site}/del/firewallrule/{id} | DELETE | Delete Firewall Rule | 
|  | /api/s/{site}/del/firewallgroup/{id} | DELETE | Delete Firewall Group | 
|  | /api/s/{site}/del/portconf/{id} | DELETE | Delete Port Configuration | 
|  | /api/s/{site}/del/radiusprofile/{id} | DELETE | Delete RADIUS Profile | 
|  | /api/s/{site}/del/account/{id} | DELETE | Delete Account | 
|  | /api/s/{site}/del/wlangroup/{id} | DELETE | Delete WLAN Group | 
|  | /api/s/{site}/del/hotspotop/{id} | DELETE | Delete Hotspot Operator | 
|  | /api/s/{site}/del/hotspotpackage/{id} | DELETE | Delete Hotspot Package | 
|  | /api/s/{site}/del/voucher/{id} | DELETE | Delete Voucher | 
|  | /api/s/{site}/del/portforward/{id} | DELETE | Delete Port Forwarding Rule | 
|  | /api/s/{site}/del/dynamicdns/{id} | DELETE | Delete Dynamic DNS Configuration | 
|  | /api/s/{site}/del/portprofile/{id} | DELETE | Delete Port Profile | 
|  | /api/s/{site}/del/dpiapp/{id} | DELETE | Delete DPI Application | 
|  | /api/s/{site}/del/dpigroup/{id} | DELETE | Delete DPI Group | 
| Category | Endpoint | Method | Description | 
|---|---|---|---|
| Content Filtering | /v2/api/site/{site}/content-filtering | GET | Get Content Filtering Configuration | 
|  | /v2/api/site/{site}/content-filtering | PUT | Update Content Filtering Configuration | 
| Category | Endpoint | Method | Description | 
|---|---|---|---|
| DHCP | /v2/api/site/{site}/excluded-ips | GET | List Excluded IPs | 
|  | /v2/api/site/{site}/excluded-ips | POST | Create Excluded IP | 
|  | /v2/api/site/{site}/excluded-ips/{id} | GET | Get Excluded IP Details | 
|  | /v2/api/site/{site}/excluded-ips/{id} | PUT | Update Excluded IP | 
|  | /v2/api/site/{site}/excluded-ips/{id} | DELETE | Delete Excluded IP | 
| Category | Endpoint | Method | Description | 
|---|---|---|---|
| Hotspot | /v2/api/site/{site}/hotspot2-conf | GET | List Hotspot 2.0 Configurations | 
|  | /v2/api/site/{site}/hotspot2-conf | POST | Create Hotspot 2.0 Configuration | 
|  | /v2/api/site/{site}/hotspot2-conf/{id} | GET | Get Hotspot 2.0 Configuration Details | 
|  | /v2/api/site/{site}/hotspot2-conf/{id} | PUT | Update Hotspot 2.0 Configuration | 
|  | /v2/api/site/{site}/hotspot2-conf/{id} | DELETE | Delete Hotspot 2.0 Configuration | 
|  | /v2/api/site/{site}/hotspot-operators | GET | List Hotspot Operators | 
|  | /v2/api/site/{site}/hotspot-operators | POST | Create Hotspot Operator | 
|  | /v2/api/site/{site}/hotspot-operators/{id} | GET | Get Hotspot Operator Details | 
|  | /v2/api/site/{site}/hotspot-operators/{id} | PUT | Update Hotspot Operator | 
|  | /v2/api/site/{site}/hotspot-operators/{id} | DELETE | Delete Hotspot Operator | 
|  | /v2/api/site/{site}/hotspot-packages | GET | List Hotspot Packages | 
|  | /v2/api/site/{site}/hotspot-packages | POST | Create Hotspot Package | 
|  | /v2/api/site/{site}/hotspot-packages/{id} | GET | Get Hotspot Package Details | 
|  | /v2/api/site/{site}/hotspot-packages/{id} | PUT | Update Hotspot Package | 
|  | /v2/api/site/{site}/hotspot-packages/{id} | DELETE | Delete Hotspot Package | 
| Category | Endpoint | Method | Description | 
|---|---|---|---|
| Static DNS | /v2/api/site/{site}/static-dns | GET | List Static DNS Records | 
|  | /v2/api/site/{site}/static-dns | POST | Create Static DNS Record | 
|  | /v2/api/site/{site}/static-dns/{id} | GET | Get Static DNS Record Details | 
|  | /v2/api/site/{site}/static-dns/{id} | PUT | Update Static DNS Record | 
|  | /v2/api/site/{site}/static-dns/{id} | DELETE | Delete Static DNS Record | 
| Category | Endpoint | Method | Description | 
|---|---|---|---|
| Teleport | /v2/api/site/{site}/teleport | GET | Get Teleport Configuration | 
|  | /v2/api/site/{site}/teleport | PUT | Update Teleport Configuration | 
| Category | Endpoint | Method | Description | 
|---|---|---|---|
| Topology | /v2/api/site/{site}/topology | GET | Get Site Topology | 
| Category | Endpoint | Method | Description | 
|---|---|---|---|
| Users | /v2/api/site/{site}/user-records | GET | List User Records | 
|  | /v2/api/site/{site}/user-records | POST | Create User Record | 
|  | /v2/api/site/{site}/user-records/{id} | GET | Get User Record Details | 
|  | /v2/api/site/{site}/user-records/{id} | PUT | Update User Record | 
|  | /v2/api/site/{site}/user-records/{id} | DELETE | Delete User Record | 
|  | /v2/api/site/{site}/user-groups | GET | List User Groups | 
|  | /v2/api/site/{site}/user-groups | POST | Create User Group | 
|  | /v2/api/site/{site}/user-groups/{id} | GET | Get User Group Details | 
|  | /v2/api/site/{site}/user-groups/{id} | PUT | Update User Group | 
|  | /v2/api/site/{site}/user-groups/{id} | DELETE | Delete User Group | 
| Category | Endpoint | Method | Description | 
|---|---|---|---|
| VPN | /v2/api/site/{site}/vpn | GET | Get VPN Configuration | 
|  | /v2/api/site/{site}/vpn | PUT | Update VPN Configuration | 
| Category | Endpoint | Method | Description | 
|---|---|---|---|
| WireGuard | /v2/api/site/{site}/wireguard-users | GET | List WireGuard Users | 
|  | /v2/api/site/{site}/wireguard-users | POST | Create WireGuard User | 
|  | /v2/api/site/{site}/wireguard-users/{id} | GET | Get WireGuard User Details | 
|  | /v2/api/site/{site}/wireguard-users/{id} | PUT | Update WireGuard User | 
|  | /v2/api/site/{site}/wireguard-users/{id} | DELETE | Delete WireGuard User | 
Use the OpenAPI specification to generate client SDKs:
# Generate Python client
openapi-generator-cli generate -i openapi/openapi.yaml -g python -o clients/python
# Generate JavaScript client
openapi-generator-cli generate -i openapi/openapi.yaml -g javascript -o clients/javascript
# Generate Go client
openapi-generator-cli generate -i openapi/openapi.yaml -g go -o clients/go
Use tools like Postman or Insomnia with the OpenAPI spec for interactive testing:
- Import openapi/openapi.yaml into your API testing tool
- Configure authentication (API key or session cookies)
- Test endpoints against your UniFi Controller
This project is licensed under the MIT License - see the LICENSE file for details.
Disclaimer: This is an unofficial, community-maintained documentation project. Ubiquiti does not officially support or endorse this API documentation. Use at your own risk and always test thoroughly in development environments before production deployment.