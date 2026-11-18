Scope: Wired (MS), Wireless (MR), SD-WAN (MX), and Security best practices for a cloud-managed enterprise network. Source basis: Cisco Meraki Best Practice Design documentation (Dashboard Administration → Architectures and Best Practices). Audience: Network, security, and wireless engineers and dashboard administrators.
This guide consolidates Meraki's published design recommendations into a single reference. Every deployment is unique — validate sizing, addressing, and policy against your own requirements, and finalize designs with your Meraki SE or partner before production cutover.
Before configuring any individual product, get these organization-wide decisions right. They are the hardest things to change later.
Organization and network structure. Auto VPN, templates, and most reporting are scoped to a single dashboard organization. Devices in separate organizations generally represent separate SD-WAN domains and cannot form Auto VPN to one another. Plan your org boundary around the administrative and SD-WAN domain you actually want, not around geography alone.
Addressing and segmentation strategy (decide once, apply everywhere).
- Use multiple VLANs/subnets segmented by traffic role (employee, voice, guest, IoT, management). Segmentation is your first and cheapest security control and it reduces broadcast scope.
- Always isolate guest and untrusted traffic onto their own VLANs with no path to business VLANs by default.
- Standardize on /24 subnets for general user access (large enough for most sites, leaves room to grow and re-subnet). Use /23 where a single user segment genuinely needs more hosts.
- Never overlap subnets across the routed domain — overlaps cause inconsistent routing/forwarding.
- Reserve a dedicated management VLAN that is separate from any user data VLAN.
Firmware. Keep MX/MS/MR on the firmware trains your Meraki SE recommends, and standardize the same build across members of a stack or HA pair before provisioning them together.
| Setting | Recommendation | 
|---|---|
| RSTP | Enabled (default). Leave on; disable only after deliberate analysis. | 
| Root bridge | Force the root deterministically — set priority to "0 / likely root" on a stable core/aggregation switch (MS410/MS425 class). Choose a switch that rarely changes config or sees link flaps. | 
| STP diameter | Keep under 7 hops end to end. | 
| BPDU Guard | Enable on all access ports (user/server edge) to block rogue switches. | 
| Loop Guard | Enable on trunk links between switches. | 
| Root Guard | Enable on ports facing switches outside your administrative control. | 
| Catalyst/Nexus interop | Allow VLAN 1 on the Catalyst↔MS trunk (required for RSTP), and make the Catalyst the root when mixing PVST. | 
Trunks.
- Prune unnecessary VLANs from each trunk's allowed list to limit flooding scope.
- Ensure the native VLAN and allowed-VLAN list match on both ends — a native VLAN mismatch bridges traffic between VLANs.
- Use port tags (e.g., trunk ,wireless ,uplink ) for operational clarity.
Link aggregation. Only LACP is supported — confirm the far end runs LACP. Configure the aggregate in the dashboard before physically cabling the partner device.
Other L2 settings.
- MTU: leave at the default (9578) unless an intermediate device lacks jumbo-frame support; jumbo frames help server-to-server/app performance and avoid fragmentation.
- UDLD: enable on fiber trunks in Alert-Only mode.
- Link negotiation: auto-negotiate for Meraki-to-Meraki links; use forced mode only for a device that cannot auto-negotiate.
- Use L3 interfaces for data VLANs only. Do not put an L3 interface on the management VLAN — keep management traffic separate.
- In a switch stack, make sure the management IP subnet does not overlap any configured L3 interface subnet (overlap causes loss when pinging/SNMP-polling stack members). Exception: MS390 is not subject to this.
- L3 table changes on most MS models (MS210/225/250/350/355/410/425/450) flush and rebuild hardware tables, causing momentary disruption — make L3 changes only in a maintenance window.
ACLs.
- Summarize addresses wherever possible to conserve entries.
- Hard limit: 128 ACEs per network. Profile your real traffic first and always permit traffic to the Meraki dashboard (see Help → Firewall Info).
OSPF (Switching → Configure → OSPF Routing).
- Use broadcast mode for hellos; leave hello/dead timers at 10s/40s unless you've tested more aggressive values.
- All areas must attach directly to Area 0 (no virtual links).
- Set an explicit Router ID for manageability and enable MD5 authentication.
- To avoid OSPF forming needless adjacencies on every VLAN of a trunk, define a dedicated transit VLAN (typically core↔aggregation) with OSPF Passive = No, and set Passive = Yes on all other advertised subnets. Keep the management VLAN allowed on those trunks.
DHCP. Configure allowed DHCP servers to block rogue servers. Prefer stacking over warm spare for L3 DHCP, since warm-spare DHCP load balancing can hand clients to a member with no remaining leases.
- Switch stacking is the preferred HA/redundancy method for L3 switches — better redundancy and faster failover than warm spare.
  - Provision stacking carefully: add switches to dashboard without configuring the stack, bring them online, push the same latest firmware to all, then power-off and cable in a ring topology before provisioning the stack.
  - Use two uplink ports (one on the top, one on the bottom switch) and configure cross-stack link aggregation for uplink redundancy.
- Warm spare (VRRP) is available for L3 gateways where stacking isn't used. The pair shares a virtual MAC (00-00-5E-00-01-xx ) and virtual IP. Provide an alternate/direct path between primary and spare for VRRP exchange. L3-interface changes can briefly trigger VRRP transitions — make them in a change window.
- Classify traffic (commonly by VLAN — user/voice/network-control — and optionally refined by protocol/ports).
- MS acts on DSCP only (it does not honor 802.1p CoS in the dot1q header, and CoS markings are not preserved by default). If an endpoint can't mark DSCP itself, set it with a QoS rule.
- There are 6 weighted CoS queues (0–5) with weights 1/2/4/8/16/32. Bandwidth share under congestion = queue weight ÷ sum of configured queue weights. Plan capacity ahead — QoS only engages during congestion.
- Keep a dashboard network to ≤ 400 switches and < 8000 switch ports for reliable topology/port-page loading.
- For multi-gig links (2.5/5 Gbps), use Cat-6a for reliable operation; Cat-5e can work but is vulnerable to alien crosstalk/noise over longer runs and bundles.
- Where possible, keep source and receivers in the same VLAN and use IGMP snooping.
- Disable IGMP snooping if you have no L2 multicast need (it's CPU-bound).
- For multicast routing: place the PIM-SM rendezvous point close to the source (core/aggregation), configure an RP for every group, verify all multicast-routed switches can ping the RP, and ACL off noise like SSDP (239.255.255.250 ). Use the 239.0.0.0/8 range for internal apps. Always run an IGMP querier per multicast VLAN when there's no PIM-enabled device.
A coverage-only survey under-builds dense environments. Treat any area expecting >30 clients per AP as high density and size by capacity:
- Aggregate application throughput = per-user app throughput × concurrent users (compute per area: lobby, classroom, auditorium, etc.).
- Per-device usable throughput ≈ ~½ of the advertised data rate, then reduce a further ~30% for real-world overhead; size to the 20 MHz rate.
- APs by throughput = aggregate throughput ÷ per-device throughput.
- APs by client count = concurrent 5 GHz clients ÷ 25 (target ~25 clients/radio, ~50/AP). Assume roughly a 30/70 split of 2.4 GHz vs 5 GHz clients.
- Required APs = the larger of the two counts.
- Perform an active site survey + spectrum analysis (e.g., Ekahau/AirMagnet). Target ≥ 25 dB SNR across the coverage area, on both bands (don't survey 2.4 GHz only).
- Mounting heights:
  - Below ~8 ft (3 m): integrated/omni dipole antennas.
  - 8–25 ft (3–8 m): external downtilt omni antennas.
  - 25 ft+ or hard ceilings: wall-mount (10–15 ft, LED facing down) or use directional antennas.
- Use directional antennas above ~26 ft, outdoors, or where directional coverage is needed; point ceiling-mounted directional antennas straight down, tilt wall-mounted ones toward the floor.
- Keep SSIDs to ≤ 3 (a hard requirement in high density; ≤ 5 absolute max). Each extra SSID burns airtime on management frames — beyond 5 SSIDs you can lose 20%+ of capacity. Make one SSID per auth type (Splash / PSK / EAP) and consolidate same-auth SSIDs.
- Bridge mode is recommended for seamless L2 roaming (best for voice). All APs in the roaming domain (floor/RF profile) must share the same VLAN.
- Do not use NAT mode for VoIP/VPN/streaming — clients re-DHCP on every roam, breaking real-time sessions.
- For roaming across subnets at scale, the current Meraki solution is the Campus Gateway (VXLAN-tunneled SSID traffic; up to 5,000 APs / 50,000 clients per gateway). Distributed L3 roaming and "MX as concentrator" are no longer recommended for large-scale roaming.
| Setting | High-density recommendation | 
|---|---|
| Band selection | Dual-band with band steering if 2.4 GHz is needed; 5 GHz only if it isn't. | 
| Channel width | 20 MHz (VHT20) — preserves the number of non-overlapping channels and gives mixed-capability clients fair airtime. | 
| Minimum bitrate | Disable low legacy rates. Use ≥ 12 Mbps to exclude 802.11b and lift RF efficiency (Cisco SF uses 18 Mbps). Use 11 Mbps only if legacy 11b must be supported. | 
| DFS channels | Enable to reach up to 19 channels (US), improving channel reuse and lowering co-channel interference. | 
| Auto power | Auto RF tunes TX power every ~20 min, aiming for ≥3 neighbors heard. Constrain min/max TX power per RF profile in complex sites. | 
| RX-SOP | Tighten receive sensitivity to shrink cells in dense areas (e.g., 5 GHz −76/−78/−80 dBm high/med/low). Test first — too aggressive creates coverage holes. | 
| Client balancing | Enable in RF profiles for dense areas (off by default). | 
Fast roaming (mostly on by default): 802.11k (neighbor reports), 802.11i / PMK caching and OKC (faster 802.1X re-auth), and 802.11r / Fast BSS Transition — enable 802.11r explicitly under Access Control → Security for voice-heavy environments where clients support it.
- Add a shaping rule to give voice & video priority and set it to ignore the SSID per-client limit so real-time media isn't throttled; block/throttle P2P and recreational apps.
- MR APs automatically do multicast-to-unicast conversion (sends at negotiated rates — great for classroom video) and limit duplicate broadcasts to prevent storms and save client battery.
- Set each uplink's configured bandwidth limit to match the ISP's actual provisioned rate so the MX can shape correctly and avoid saturating the circuit.
- Configure multiple uplink-statistic test IPs (the default 8.8.8.8 , plus your ISP gateway and key remote VPN peers) for better monitoring and troubleshooting.
- Set security-list update frequency to hourly (AMP/IDS/URL lists) per uplink, including cellular.
- Load-balance across uplinks when you have similar-bandwidth redundant circuits.
- Use flow preferences to pin traffic to the right uplink — e.g., push guest traffic onto the cheaper secondary WAN to protect the primary.
- With multi-uplink Auto VPN, the MX builds tunnels on every reachable WAN interface and can dynamically move a flow to a healthier uplink when loss/latency/jitter degrade on the active path.
- Define performance/policy rules per application class so each flow takes the appropriate path; build these to match your traffic and circuit characteristics.
- Caveat: on MX 18.2 multi-WAN, SD-WAN and load-balancing policies do not apply to WAN3.
- In large client populations, set a global per-client bandwidth limit to prevent uplink saturation.
- Speedburst lets a client briefly exceed its cap (good for occasional large transfers) — enable it only where a few users need bursts, not where many need sustained high bandwidth simultaneously.
- Enable the default traffic-shaping rules — they already prioritize voice, software updates, and collaboration apps well for most deployments.
- Use Auto VPN for inter-site connectivity (Meraki-to-Meraki, same organization only).
- Use hub-and-spoke, not full mesh: a hub builds tunnels to all other hubs and to its configured spokes; a spoke builds tunnels only to its hubs. Making every MX a spoke (or everything a hub) degrades service at scale.
- Designate resource-rich sites (HQ, datacenter) as hubs. Auto VPN automatically builds redundant tunnels on all WAN interfaces that can reach the Meraki cloud, giving failover with no extra config.
- Deploy Client VPN with a Systems Manager (SM) policy pushed to endpoints — it removes manual user setup, improves the experience, and lets admins troubleshoot without walking users through config.
- Consider split tunneling to keep only corporate-bound traffic in the tunnel.
- Routed (NAT) mode when the MX terminates the ISP handoff directly — gives full L3 (NAT, routing, DHCP, multiple VLANs, the complete firewall/threat feature set).
- Passthrough / VPN-concentrator mode when an existing upstream L3 device handles routing — the MX behaves as an L2 device (typical for a datacenter Auto VPN concentrator). Add static routes on upstream L3 devices so VPN-destined traffic reaches the MX. Avoid running content filtering in this mode.
- Avoid stacking the MX behind another NAT device — it complicates cloud connectivity and can break Auto VPN.
- The MX is a stateful firewall: inbound is denied unless the flow originated inside or a forwarding rule exists.
- By default all VLANs can talk to each other — you must add L3 rules to stop it. Explicitly block guest VLANs from reaching business VLANs, and block specific destination IPs/URLs per VLAN as needed.
- L3 rules are processed top-down, first-match wins — order matters.
- L7 rules: be granular — broad category blocks (e.g., all "file sharing") can break legitimate apps like OneDrive. Block by country only when you're sure the traffic is malicious; over-blocking geographies breaks apps that pull resources from those regions.
- Scope inbound rules as narrowly as possible — only the required ports.
- Never use "Any" for allowed remote IPs except for a true public web server; even then, prefer an obscure public port rather than common web ports.
- Use 1:1 NAT to map a dedicated public IP to an internal host (keeps it off the MX WAN IP); use 1:Many NAT to multiplex services behind one public IP when addresses are scarce. For non-web services, restrict port and remote-IP ranges.
- AMP: enable — inspects HTTP downloads and blocks known-malicious files via the AMP cloud.
- IDS/IPS: enable — SNORT®-based detection/prevention on traffic through the MX; set to Prevention to actually block, not just alert.
- Keep security/signature lists updating hourly.
- Set IP Source Address Spoofing Protection to "Block" (not just "Log") so spoofed traffic is dropped at detection rather than merely recorded.
The most resilient topology combines redundancy at every layer:
| Layer | HA mechanism | 
|---|---|
| MX (edge) | Warm-spare HA pair (VRRP) + dual-ISP uplinks with automatic failover. Run HA management through a downstream L2 switch rather than a dedicated HA cable (the dedicated cable plus a shared L2 switch invites an STP loop). Allow all VLANs on the switch links so VRRP heartbeats reach across every VLAN. | 
| MS (switching) | Stacking (preferred) with cross-stack LACP uplinks on top+bottom switches; warm spare (VRRP) for L3 gateways where stacking isn't used. | 
| MR (wireless) | Bridge mode + common VLAN per roaming domain for seamless failover; Campus Gateway for large multi-subnet roaming. | 
| WAN/SD-WAN | Auto VPN auto-builds tunnels on all reachable uplinks for transparent path failover. | 
Foundation
- Org boundary chosen around the intended admin + Auto VPN domain
- VLAN plan: employee / voice / guest / IoT / management, no overlaps
- Standard /24 (or /23) user subnets; dedicated management VLAN
- Firmware standardized across stacks and HA pairs
Wired (MS)
- RSTP on; root forced (priority 0) on a stable core switch; diameter < 7
- BPDU Guard on access ports; Loop/Root Guard on appropriate trunks
- Trunks pruned; native/allowed VLANs matched both ends; LACP pre-configured in dashboard
- UDLD alert-only on fiber; auto-negotiation on Meraki links
- L3 interfaces on data VLANs only; mgmt subnet ≠ any L3 subnet (non-MS390 stacks)
- OSPF: Area 0 attached, MD5 auth, transit-VLAN design, sensible passive interfaces
- Allowed DHCP servers set; dashboard permitted in ACLs (≤128 ACEs)
- ≤400 switches / <8000 ports per network; Cat-6a for multi-gig
Wireless (MR)
- Sized by capacity (throughput and client count), ~25/radio, ~50/AP
- Active survey + spectrum analysis, ≥25 dB SNR both bands
- ≤3 SSIDs (≤5 max), one per auth type; bridge mode (not NAT) for voice
- RF profiles per area: 20 MHz width, min bitrate ≥12 Mbps, DFS on, client balancing on
- RX-SOP tuned and tested; 802.11k/i/OKC on; 802.11r on for voice
- Voice/video shaping rule set to ignore per-client limit
SD-WAN (MX)
- Uplink bandwidth limits match ISP rates; multiple test IPs; hourly list updates
- Load balancing / flow preferences configured (e.g., guest → secondary WAN)
- Performance policies per app class; default traffic-shaping rules enabled
- Auto VPN hub-and-spoke with HQ/DC as hubs
- Client VPN via Systems Manager policy (+ split tunnel where appropriate)
Security
- Correct MX mode (NAT vs concentrator) for the topology
- Inter-VLAN firewall rules (guest isolated); ordered top-down
- Granular L7 rules; country blocks only when justified
- Port-forwarding/NAT scoped narrowly, no "Any" remote IPs
- AMP + IDS/IPS (Prevention) enabled; spoofing protection set to Block
Reference: Cisco Meraki Best Practice Design — General MX Best Practices, General MS Best Practices, and High-Density Wi-Fi Deployments (documentation.meraki.com). Confirm model-specific behavior, licensing, and firmware caveats against current Meraki documentation and your Meraki SE.