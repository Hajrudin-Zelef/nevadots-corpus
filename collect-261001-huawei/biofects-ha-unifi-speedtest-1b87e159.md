Requirement: This integration requires self-hosted UniFi OS Server software or a UniFi OS console such as a UDM Pro or UDM SE.
This Home Assistant custom integration provides real-time speed test monitoring for UniFi OS networks with full Multi-WAN support. It supports UniFi OS hardware consoles and self-hosted UniFi OS Server, allowing you to track download speed, upload speed, and ping directly within Home Assistant.
v4.0.0 breaking change: A local UniFi OS API key is required. Legacy standalone UniFi Network Application controllers using username/password are no longer supported.
If you find this plugin useful, please consider donating. Your support is greatly appreciated!
- 🔐 API Key Authentication: One local API-key flow for every supported UniFi OS system
- 🎯 One-Step Configuration: Enter the UniFi OS URL, API key, site, and integration options
- 🌐 Full Multi-WAN Support: Separate devices and sensors for each WAN interface (WAN, WAN2, WAN3, etc.)
- 📊 Dynamic Entity Creation: Entities appear automatically once speed test data exists for each WAN
- 🔧 UniFi OS Compatibility: Works with UDM Pro, UDM SE, UDM Base, Cloud Gateway, Cloud Key Gen2+, and self-hosted UniFi OS Server
- ⚡ Real-time Metrics: Monitor download speeds, upload speeds, and network latency (ping) for each WAN
- 🚀 Manual Speed Tests: Button entities for triggering tests on all WANs or specific interfaces
- ⏱️ Faster Updates: Automatic refresh scheduling after tests complete for immediate result visibility
- 🏠 Home Assistant Integration: Full integration with automations, scripts, and dashboards
- 🏷️ Clean Naming: Compact sensor names with Primary/Secondary WAN designation
- 👁️ Optional Inactive WAN Display: Choose whether to show disconnected/inactive WAN interfaces
- 🔒 Reliable API Usage: Uses official UniFi API endpoints with proper fallback handling
Configure multi-WAN support during setup to enable separate monitoring for each WAN interface:
See how the integration creates separate sensors for each WAN interface, providing individual speed metrics:
Monitor your primary WAN connection with dedicated sensors for download, upload, and ping:
Track your secondary WAN connection independently with its own set of performance metrics:
Notice how each WAN interface gets its own sensors, solving the issue where dual WAN setups previously showed identical speeds for both connections.
- Multi-WAN Support: ✅ Full dual WAN detection and monitoring
- Speed Test Monitoring: ✅ Automatic retrieval of speed test results
- Separate Sensors: ✅ Individual sensors for each WAN interface
- URL Format: https://udm-ip (port 443)
- Multi-WAN Support: ❌ Single WAN hardware limitation
- Speed Test Monitoring: ✅ Standard monitoring for single WAN
- Backward Compatible: ✅ Works exactly as before
- URL Format: https://udm-ip (port 443)
- Multi-WAN Support: ✅ Full dual WAN detection and monitoring
- Speed Test Monitoring: ✅ Full functionality
- API Support: ✅ Modern UniFi OS endpoints
- URL Format: https://cloudgateway-ip (port 443)
- ⚠️ Important: API key MUST be created via local access (see configuration below)
- Thanks to: @pterhaar for Cloud Gateway testing and documentation
- Multi-WAN Support: ✅ Depends on gateway model (USG Pro 4, UXG Pro)
- Speed Test Monitoring: ✅ Full functionality
- API Support: ✅ Modern UniFi OS endpoints
- URL Format: https://cloudkey-ip (port 443)
- Authentication: Local UniFi OS API key
- WAN Status: May be empty when no gateway hardware is available
- URL Format: Use the local UniFi OS Server URL
- Username/password authentication and unprefixed /api/... endpoints are not supported in v4.
- Migrate to UniFi OS Server or a UniFi OS hardware console before upgrading.
- Open HACS in your Home Assistant instance
- Click on "Integrations"
- Click the three dots in the top right corner
- Select "Custom repositories"
- Add this repository URL
- Select "Integration" as the category
- Click "Add"
- Find "HA Unifi Speedtest" in the integration list
- Click "Download"
- Restart Home Assistant
- Download the latest release
- Copy the custom_components/ha_unifi_speedtest directory to your Home Assistant'scustom_components directory
- Restart Home Assistant
For all UniFi OS systems:
- Open the console locally at https://192.168.1.1 (replace the address if your console uses a different local IP).
- Sign in and open the Network application.
- Go to Settings → Control Plane → Integrations.
- Click Create API Key, give the key a recognizable name, and copy it immediately.
- Use the generated Network Integration API key in Home Assistant.
Important: Create the key through the local Network application. Keys from unifi.ui.com, Site Manager, Protect, or another UniFi application do not authenticate with the local Network API used by this integration.
UniFi OS normally serves its local API over HTTPS. Disabling certificate verification does not change the URL to HTTP.
| Local console setup | URL | Verify SSL | 
|---|---|---|
| Local IP with the default self-signed certificate | https://192.168.1.1 | Off | 
| Local hostname with a trusted certificate matching that hostname | https://unifi.local | On | 
| Local IP with a trusted certificate that includes the IP address | https://192.168.1.1 | On | 
Do not use http://192.168.1.1. Keep https:// in the URL even when Verify SSL is disabled.
- Go to Settings → Devices & Services
- Click "+ Add Integration"
- Search for "HA Unifi Speedtest"
- Enter the UniFi OS connection details:
- URL: Your local console URL (for example, https://192.168.1.1 orhttps://unifi.local )
- API Key: The API key you generated
- Site (Optional): Site ID (default: "default")
- Verify SSL: Disable for the default self-signed IP certificate; enable for a trusted certificate matching the URL
- Enable Multi-WAN: Enable to detect and monitor multiple WAN interfaces
- Show Inactive WANs: Show entities for disconnected/inactive WANs
- Configure Options (can be changed later):
  - Enable Automatic Speed Tests: Schedule regular speed tests
  - Speed Test Interval: How often to run tests (15-1440 minutes, default: 90)
  - Polling Interval: How often to check for results (automatically calculated if not specified)
📋 See the Screenshots section above for visual examples of the configuration process and resulting sensors.
For multi-WAN configurations, separate devices are created for each WAN interface, each containing three sensors:
Primary WAN Device (e.g., "HA Unifi Speedtest Primary WAN [WAN - eth9]"):
- Download Speed WAN (Mbit/s)
- Upload Speed WAN (Mbit/s)
- Ping WAN (ms)
Secondary WAN Device (e.g., "HA Unifi Speedtest Secondary WAN [WAN2 - eth8]"):
- Download Speed WAN2 (Mbit/s)
- Upload Speed WAN2 (Mbit/s)
- Ping WAN2 (ms)
Additional WANs (WAN3, WAN4, etc.) follow the same pattern.
For single WAN setups or self-hosted UniFi OS Server, sensors use clean naming:
- UniFi Speed Test Download Speed (Mbit/s)
- UniFi Speed Test Upload Speed (Mbit/s)
- UniFi Speed Test Ping (ms)
- Speed Test Runs: Track total number of speed tests performed
- API Health: Monitor integration connection status and rate limiting
Each multi-WAN sensor includes detailed attributes:
- interface_name : Physical interface (e.g., "eth9", "eth8")
- wan_networkgroup : WAN group name (e.g., "WAN", "WAN2")
- wan_number : Sequential WAN number
- total_wan_interfaces : Total detected WAN interfaces
- is_primary_wan : Boolean indicating primary WAN (based on routing configuration)
- timestamp : Last speedtest timestamp
- status : Current interface status (up/down)
- ip_address : WAN IP address
- gateway : Gateway IP address
Note on is_primary_wan: Reflects the actual primary WAN as configured in your UniFi controller routing table, not just physical port order.
The integration creates button entities for triggering speed tests:
All WANs Button:
- HA Unifi Speedtest Run Speedtest (All WANs) - Triggers speed test on all WAN interfaces
Per-WAN Buttons (created dynamically for each active WAN):
- HA Unifi Speedtest Run Speedtest (WAN - eth9) - Trigger test on specific WAN
- HA Unifi Speedtest Run Speedtest (WAN2 - eth8) - Trigger test on specific WAN
- etc.
Initiates a speed test on your UniFi network.
Parameters:
- config_entry_id (optional): Specific integration instance
- interface_name (optional): Specific WAN interface to test
Manually refreshes speed test data from your UniFi controller.
Parameters:
- config_entry_id (optional): Specific integration instance
In Automations:
action:
  - service: ha_unifi_speedtest.start_speed_test
    data:
      interface_name: "eth9"  # Optional: test specific interface
In Scripts:
test_network_speed:
  sequence:
    - service: ha_unifi_speedtest.start_speed_test
    - delay: "00:02:00"  # Wait for test to complete
    - service: notify.mobile_app
      data:
        message: "Speed test completed. Download: {{ states('sensor.download_speed_wan') }} Mbps"
Lovelace Button Card:
type: button
name: Start Speed Test
icon: mdi:speedometer
tap_action:
  action: call-service
  service: ha_unifi_speedtest.start_speed_test
Multi-WAN Dashboard:
type: vertical-stack
cards:
  - type: entities
    title: Primary WAN (eth9)
    entities:
      - entity: sensor.download_speed_wan
        name: Download Speed
      - entity: sensor.upload_speed_wan
        name: Upload Speed
      - entity: sensor.ping_wan
        name: Ping
      - entity: button.ha_unifi_speedtest_run_speedtest_wan_eth9
        name: Run Speed Test
  
  - type: entities
    title: Secondary WAN (eth8)
    entities:
      - entity: sensor.download_speed_wan2
        name: Download Speed
      - entity: sensor.upload_speed_wan2
        name: Upload Speed
      - entity: sensor.ping_wan2
        name: Ping
      - entity: button.ha_unifi_speedtest_run_speedtest_wan2_eth8
        name: Run Speed Test
Single WAN Dashboard:
type: entities
title: Network Speed Test
entities:
  - entity: sensor.unifi_speed_test_download_speed
    name: Download Speed
  - entity: sensor.unifi_speed_test_upload_speed  
    name: Upload Speed
  - entity: sensor.unifi_speed_test_ping
    name: Ping
  - entity: button.ha_unifi_speedtest_run_speedtest_all_wans
    name: Run Speed Test
Fixes:
- Detects controller-initiated speed tests when UniFi reuses a result ID
- v4.1.0 - Device registry migration and stale device cleanup
- v4.0.0 - UniFi OS API-key-only integration
- v2.2.0 - User-controlled controller type selection
- v2.1.1 - UDM Pro 404 error fixes
- v2.1.0 - Intelligent primary WAN detection
- v2.0.1 - Initial multi-WAN support
Fixes:
- Preserves existing devices, area assignments, and user customizations when upgrading from v3
- Reconciles duplicate devices already created by v4.0.0
- Cleans up inactive WAN entities for interface names such as ppp0 andwan0
- Enables manual removal of stale integration devices
See CHANGELOG.md for complete details.
Invalid Authentication Error:
- Verify your local UniFi OS API key is correct and hasn't expired
  - Generate a new key if needed: Network → Settings → Control Plane → Integrations
- Confirm the key was created through the local Network application, not unifi.ui.com , Site Manager, or Protect
Cannot Connect:
- Check the URL format:
  - UniFi OS console: https://192.168.1.1
  - UniFi OS Server: use its locally configured URL and port
- UniFi OS console: 
- Verify the controller is accessible from Home Assistant
- Check firewall settings allow connections
- Disable "Verify SSL" when connecting by local IP with the default self-signed certificate
- Keep using https:// ; disabling verification does not enable plain HTTP
No Speed Test Data / No Sensors Appear:
- Ensure at least one speed test has been run on your controller
- For multi-WAN: Sensors appear dynamically after speed test data exists
- Use the "Run Speedtest (All WANs)" button to seed initial data
- Check that WANs are active (have link and IP address)
- Enable "Show Inactive WANs" in options if you want to see all WANs
Only One WAN Showing (Expected Multiple):
- Verify multi-WAN is enabled in integration options
- Check that multiple WANs are configured in UniFi controller
- Ensure WANs are active (connected with IP addresses)
- Run a manual speed test to trigger entity creation
- Check Developer Tools → States for all sensor.*wan* entities
API Key Not Working:
- Verify the target runs UniFi OS, not the legacy standalone Network Application
- Ensure the API key was copied completely (no spaces or truncation)
- Check the API key hasn't been revoked in the controller
- Try generating a new API key
Where to Find API Key:
- Local console → Network → Settings → Control Plane → Integrations → "Create API Key"
- Save the key immediately - you can't view it again after creation
Primary WAN Detection Methods (in order of priority):
- Routing Table Analysis: Checks the default route (0.0.0.0/0)
- WAN Group Priority: "WAN" group is preferred as primary
- Network Configuration: Looks for explicit primary WAN settings
- Speed Test Data: Uses most recent and complete data
- Fallback: First detected interface
If Primary WAN Detection is Incorrect:
- 
Check UniFi Network Settings: 
  - Verify routing configuration in UniFi Network application
  - Ensure the desired primary WAN has the default route
- 
Check Integration Logs (enable debug logging below): 
  - Look for: "Primary WAN determined from routing table: eth9_WAN"
  - Shows which detection method was used
- 
Check Sensor Attributes: 
  - Developer Tools → States → Find your speed test sensors
  - Verify is_primary_wan: true on correct interface
Expected Behavior:
- Interface configured as primary in UniFi shows is_primary_wan: true
- Devices named "Primary WAN [WAN - eth9]" and "Secondary WAN [WAN2 - eth8]"
- Each WAN gets separate device with its own sensors
Check for Errors:
# View Home Assistant logs
docker logs homeassistant | grep ha_unifi_speedtest
Common Causes:
- Missing required files (ensure all .py files are present)
- Python import errors (check file permissions)
- Configuration issues (try removing and re-adding integration)
To enable debug logging, add the following to your configuration.yaml:
logger:
  default: info
  logs:
    custom_components.ha_unifi_speedtest: debug
Feel free to contribute to this project. Please read the contributing guidelines before making a pull request.
This project is licensed under the MIT License - see the LICENSE file for details.
This integration is not affiliated with Ubiquiti Inc. or UI.com. All product names, logos, and brands are property of their respective owners.