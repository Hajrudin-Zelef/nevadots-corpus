| name | meraki-automation | 
|---|---|
| description | This skill should be used when the user asks to write Python scripts, automation code, or any programmatic tasks involving Cisco Meraki networks, devices, or the Meraki Dashboard API. It provides a structured index of all 914 Meraki Dashboard API v1.69.0 endpoints and step-by-step guidance for matching APIs to user requirements and generating complete, executable Python scripts. | 
This skill enables accurate generation of Python scripts that call the Cisco Meraki Dashboard API. It covers 914 unique API operations in the supplied Meraki Dashboard API v1.69.0 collection, grouped into 458 categories across:
- Platform APIs: organizations, networks, devices, admins, config templates, firmware, etc.
- Products APIs: appliance, switch, wireless, camera, sensor, cellular gateway, insight, SM, etc.
The skill uses a two-level lookup strategy:
- scripts/search_index.py — fast keyword search overreferences/api_index.json ; always run this first to find matching APIs.
- scripts/parse_collection.py — retrieves full body schema and complete response examples from the original Postman collection for any specific API.
The original collection contains 7,021 request entries, including repeated shortcuts.
Both lookup layers deduplicate by API name, HTTP method and normalized URL, preferring
platform/ or products/ entries, then entries with response examples. Root-only APIs are retained.
Run the commands below from this skill's directory, or use absolute paths.
Identify the key action words and target resources. Map to Meraki concepts:
| User says | Meraki concept | Index category hint | 
|---|---|---|
| "list all networks" | getOrganizationNetworks | platform/configure | 
| "get/list devices" | getOrganizationDevices, getNetworkDevices | platform/configure/devices | 
| "configure SSID" | updateNetworkWirelessSsid | products/wireless/configure/ssids | 
| "firewall rules" | appliance MX L3/L7 firewall | products/appliance/configure/firewall | 
| "switch port" | updateDeviceSwitchPort | products/switch/configure/ports | 
| "VPN" | site-to-site or client VPN | products/appliance/configure/vpn | 
| "alerts / webhooks" | network alerts, HTTP servers | platform/configure/alerts orplatform/configure/webhooks | 
| "clients" | getNetworkClients | platform/monitor orgetNetworkClients | 
| "firmware upgrade" | firmware upgrades | platform/configure/firmwareUpgrades | 
| "camera" | camera APIs | products/camera | 
| "sensor" | sensor APIs | products/sensor | 
Run a keyword search to find matching APIs. The script searches across API name, URL, and description:
# Keyword search (returns up to 20 results by default)
python scripts/search_index.py --search "ssid"
python scripts/search_index.py --search "firewall" --method GET
python scripts/search_index.py --search "switch port" --limit 10
# Exact lookup by API function name
python scripts/search_index.py --name "getNetworkWirelessSsids"
# Browse all APIs under a category
python scripts/search_index.py --category "products/wireless/configure/ssids"
# List all 458 categories with API counts
python scripts/search_index.py --list-categories
Search output reports total_found before applying --limit, returned for the
number included, and truncated when more matches exist. Increase --limit or
narrow the search when truncated. Category lookup includes the selected category
and its descendants; a trailing /* is also accepted (not general glob syntax).
Each result entry contains:
{
  "category": "products/wireless/configure/ssids",
  "name": "getNetworkWirelessSsids",
  "method": "GET",
  "url": "/api/v1/networks/{networkId}/wireless/ssids",
  "desc": "List the MR SSIDs in a network",
  "path_params": ["networkId"],
  "body_fields": [],
  "resp": {"array_of": ["number", "name", "enabled", "authMode"]}
}
- resp — top-level keys for an object response, or{"array_of": [...]} for an array of objects (compact hint; the example above abbreviates the keys).
- query_params — present only when the API supports optional/required filters.
- Query parameter optional isfalse for an explicit(Required) marker,true for(Optional) , andnull when the collection does not state it.disabled only records whether Postman sends the parameter in its sample;
it does not establish requiredness. Fornull , check the parameter description
and applicable API documentation before constructing the request.
- body_fields — top-level fields accepted in the request body.
For POST/PUT/DELETE calls or when you need the exact nested body structure and complete response examples, run:
python scripts/parse_collection.py "references/Meraki Dashboard API - v1.69.0.postman_collection.json" --name "getNetworkWirelessSsids"
This returns:
- body_schema — parsed Postman request-body template; example values are not a formal schema or guaranteed defaults
- response_examples — complete JSON response example from the official docs
The Postman collection is located at:
references/Meraki Dashboard API - v1.69.0.postman_collection.json
Follow all code generation rules in the section below.
import requests
import time
import json
BASE_URL = "https://api.meraki.cn/api/v1"
API_KEY = "YOUR_API_KEY_HERE"  # Replace with actual key or os.environ.get("MERAKI_API_KEY")
HEADERS = {
    "X-Cisco-Meraki-API-Key": API_KEY,
    "Content-Type": "application/json",
    "Accept": "application/json",
}
Always use https://api.meraki.cn/api/v1 as BASE_URL (China region endpoint).
Replace path parameters using Python f-strings:
# Postman: /api/v1/networks/{networkId}/wireless/ssids/{number}
url = f"{BASE_URL}/networks/{network_id}/wireless/ssids/{ssid_number}"def make_request(method, url, params=None, payload=None, retries=3):
    for attempt in range(retries):
        try:
            response = requests.request(
                method,
                url,
                headers=HEADERS,
                params=params,
                json=payload,
                timeout=30,
            )
            if response.status_code == 429:
                retry_after = int(response.headers.get("Retry-After", 1))
                print(f"Rate limited. Waiting {retry_after}s...")
                time.sleep(retry_after)
                continue
            response.raise_for_status()
            if response.status_code == 204:
                return None
            return response.json()
        except requests.exceptions.HTTPError as e:
            print(f"HTTP Error: {e} | Response: {response.text}")
            raise
        except requests.exceptions.RequestException as e:
            print(f"Request failed (attempt {attempt+1}/{retries}): {e}")
            if attempt == retries - 1:
                raise
            time.sleep(2 ** attempt)
    return None
Many GET list APIs support cursor-based pagination via perPage + startingAfter:
def get_all_pages(url, params=None):
    """Fetch all pages for a paginated GET list endpoint."""
    all_results = []
    page_params = dict(params or {})
    page_params["perPage"] = 1000
    while True:
        response = requests.get(url, headers=HEADERS, params=page_params, timeout=30)
        if response.status_code == 429:
            time.sleep(int(response.headers.get("Retry-After", 1)))
            continue
        response.raise_for_status()
        data = response.json()
        if isinstance(data, list):
            all_results.extend(data)
            if len(data) < page_params["perPage"]:
                break
            # Use last item's id or serial for cursor
            link_header = response.headers.get("Link", "")
            if 'rel="next"' not in link_header:
                break
            # Extract startingAfter from Link header
            import re
            match = re.search(r'startingAfter=([^&>]+)', link_header)
            if match:
                page_params["startingAfter"] = match.group(1)
            else:
                break
        else:
            return data  # Non-list response, no pagination needed
    return all_results
Encapsulate each API call as an independent function. Use a main() entry point:
def get_organizations():
    """Return list of organizations accessible with this API key."""
    url = f"{BASE_URL}/organizations"
    return make_request("GET", url)
def get_organization_networks(org_id):
    """Return the list of networks in an organization."""
    url = f"{BASE_URL}/organizations/{org_id}/networks"
    return get_all_pages(url)
def update_network_wireless_ssid(network_id, ssid_number, payload):
    """Update the attributes of an MR SSID.
    
    payload example:
        {"name": "My SSID", "enabled": True, "authMode": "psk", "psk": "password"}
    """
    url = f"{BASE_URL}/networks/{network_id}/wireless/ssids/{ssid_number}"
    return make_request("PUT", url, payload=payload)
def main():
    org_id = "YOUR_ORG_ID"
    print("Fetching organizations...")
    orgs = get_organizations()
    for org in orgs:
        print(f"  {org['id']}: {org['name']}")
    print(f"\nFetching networks for org {org_id}...")
    networks = get_organization_networks(org_id)
    print(f"  Found {len(networks)} networks")
if __name__ == "__main__":
    main()
Use the resp field from api_index.json or the full response_examples from parse_collection.py --name to know the exact field names:
# Example: getOrganizationDevices returns list of device objects
# resp keys: ["serial", "name", "model", "mac", "networkId", "lanIp", "firmware", "status", ...]
devices = get_all_pages(f"{BASE_URL}/organizations/{org_id}/devices")
for device in devices:
    print(f"{device['serial']} | {device['name']} | {device['model']} | {device.get('lanIp', 'N/A')}")orgs = make_request("GET", f"{BASE_URL}/organizations")
for org in orgs:
    networks = get_all_pages(f"{BASE_URL}/organizations/{org['id']}/networks")
    for net in networks:
        # process each network
        passerrors = []
for item in items:
    try:
        result = make_request("PUT", url, payload=payload)
    except Exception as e:
        errors.append({"item": item, "error": str(e)})
if errors:
    print(f"Completed with {len(errors)} errors:")
    for err in errors:
        print(f"  {err}")import csv, json
# JSON export
with open("output.json", "w", encoding="utf-8") as f:
    json.dump(results, f, ensure_ascii=False, indent=2)
# CSV export
with open("output.csv", "w", newline="", encoding="utf-8") as f:
    if results:
        writer = csv.DictWriter(f, fieldnames=results[0].keys())
        writer.writeheader()
        writer.writerows(results)
Use these category paths to navigate api_index.json efficiently:
| Task | Category in api_index.json | 
|---|---|
| Organizations | getOrganizations ,platform/configure | 
| Networks | getOrganizationNetworks ,platform/configure/devices | 
| Devices / Inventory | getOrganizationDevices ,platform/configure/inventory | 
| Network Clients | getNetworkClients ,platform/configure/clients | 
| Admins | platform/configure/admins | 
| Config Templates | platform/configure/configTemplates | 
| Firmware | platform/configure/firmwareUpgrades | 
| Alerts & Webhooks | platform/configure/alerts ,platform/configure/webhooks | 
| Appliance (MX) | products/appliance/configure/* ,products/appliance/monitor/* | 
| Switch (MS) | products/switch/configure/* ,products/switch/monitor/* | 
| Wireless (MR) | products/wireless/configure/* ,products/wireless/monitor/* | 
| Camera (MV) | products/camera/configure/* ,products/camera/monitor/* | 
| Sensor (MT) | products/sensor/configure/* ,products/sensor/monitor/* | 
| Cellular Gateway | products/cellularGateway/configure/* | 
| Insight | products/insight/configure/* ,products/insight/monitor/* | 
| Systems Manager | products/sm/configure/* | 
| SASE sites / connectors | platform/configure/sase ,platform/monitor/sase | 
| Global policies | platform/configure/policies/global ,products/appliance/configure/policies/global | 
If the Postman Collection is updated, regenerate the index by running:
python scripts/generate_index.py "<path_to_collection.json>" --output references/api_index.json
Keep the collection unchanged, update its filename and version references, and use
the generated meta.total and meta.categories for documentation counts. Check
the new collection's categories before changing category hints. Validate offline:
python -m unittest discover -s scripts -p "test_*.py"
python scripts/search_index.py --name "getOrganizationSaseSites"
python scripts/parse_collection.py "references/Meraki Dashboard API - v1.69.0.postman_collection.json" --name "getOrganizationSaseSites"