Executive Summary
This report synthesizes vendor guidance, practitioner best practices, and real-world tuning data to deliver a production-ready QoS framework for UniFi Network (9.x+) environments. The architecture addresses the critical decision points between Smart Queues (SQM), policy-based QoS rules, and DSCP-only designs, providing reusable profile templates optimized for multi-tenant MSP deployments. Key findings include: Smart Queues should be limited to WAN links under 300 Mbps at 80–95% of measured line rate; DSCP EF 46 marking for RTP voice must be preserved end-to-end; and over-prioritization across all traffic classes negates QoS benefits. The recommended profile library covers six core traffic classes with specific DSCP mappings, bandwidth allocations, and UniFi implementation paths.
UniFi Network provides three primary QoS mechanisms that operate at different layers of the stack. Understanding their interaction is essential for coherent policy design.
Smart Queues implement fq_codel (Fair Queuing with Controlled Delay) at the WAN edge to combat bufferbloat. The mechanism:123
- Shapes egress traffic to configured up/down rates
- Automatically prioritizes small, latency-sensitive packets (DNS, VoIP, gaming)
- Requires disabling hardware offload on legacy platforms (USG series) but runs efficiently on modern gateways (UDM/UXG series)4
- Recommended only for ISP connections <300 Mbps; counterproductive on high-bandwidth links due to unnecessary queuing delays51
Policy-based rules (Network 9.x) allow explicit Prioritize, Limit, or Prioritize and Limit actions on traffic matching source/destination criteria. Key capabilities:6
- Match by device, network (VLAN), application, IP address/port, domain, or region
- Enforce bandwidth caps per direction (Mbps)
- Higher granularity than Smart Queues but requires manual classification
- Performance impact scales with rule complexity; test throughput ceilings after activation7
DSCP values (0–63) mark packets for per-hop behavior across switches and access points:8
- EF (46): Expedited Forwarding for VoIP RTP (strict priority)
- AF41 (34): Assured Forwarding for interactive video
- CS0 (0): Default best-effort
- UniFi switches and APs automatically map DSCP to WMM access categories (AC_VO, AC_VI, AC_BE, AC_BK) when QoS is enabled on the port/profile910
Port-level QoS assignment complements gateway policies:
- Port Priority (High/Critical) elevates all traffic on a switch port
- Voice VLANs isolate phone traffic and enable LLDP-MED auto-configuration11
- Trust boundaries must be defined: either preserve endpoint DSCP or remark at switch ingress for untrusted devices8
| WAN Speed | Primary Use Case | Recommended Approach | Rationale | 
|---|---|---|---|
| <100 Mbps | Residential, small office, VoIP-heavy | Smart Queues at 85–90% of tested rate | Bufferbloat dominates; automatic fairness reduces latency spikes 1213 | 
| 100–300 Mbps | SMB, mixed-use environments | Smart Queues at 80–95% rate OR QoS rules for voice + Smart Queues for bulk | 权衡：Smart Queues add latency but simplify management; policy QoS preserves throughput for non-congested links 12 | 
| >300 Mbps | High-performance, MSP backbone | Policy QoS only (no Smart Queues) | SQM overhead unnecessary; shaping at non-bottleneck is ineffective 15 | 
| Variable (cellular, shared fiber) | Remote sites, pop-up offices | Adaptive Smart Queues at 80% of minimum observed rate + DSCP trust | Accommodates link variability; prevents queue overruns during troughs 142 | 
Table: WAN speed-based QoS strategy selection
Community consensus and fq_codel best practices recommend setting Smart Queue rates 10–20% below measured ISP throughput to account for variable overhead and upstream ISP buffering:12152
- Stable fiber/coax: 90–95% of sustained speedtest results
- Variable cable/DSL: 80–85% of off-peak measurements
- Cellular/Starlink: 70–80% of median observed rates
- Always verify with real-world tests (speedtest, bufferbloat.net) after enabling1612
Preserve end-to-end DSCP markings to leverage hardware queueing in switches and APs:178
- Trust endpoint markings for enterprise phones and video conferencing systems (most mark RTP correctly)
- Remark at switch for untrusted IoT or guest devices to prevent DSCP spoofing
- Avoid mid-path remarking unless converting between domains (e.g., ISP-facing EF remapping)
- Use VLANs as primary trust boundary: separate voice, video, guest, and bulk traffic into distinct broadcast domains with consistent DSCP policies1816
Disable SIP ALG on UniFi gateways for 95% of modern VoIP platforms (SIP ALG routinely breaks signaling). Restrict port forwards to required UDP/TCP ranges only; avoid blanket "DMZ" configurations. Use IP Group objects in QoS rules to target PBX/trunk IPs precisely.16
Apply bandwidth limits exclusively where congestion occurs—typically WAN upstream on asymmetric links. LAN-side shaping between VLANs is rarely necessary on gigabit UniFi switches and introduces unnecessary complexity.1916
Assigning Critical priority to multiple traffic classes creates contention and negates QoS benefits. Limit the number of high-priority classes to 2–3 per deployment (e.g., voice + gaming, or voice + video). Use Limit objectives for bulk traffic rather than deprioritizing into low queues.719
Enabling QoS rules disables hardware offload on legacy platforms, capping throughput to ~250–300 Mbps on USG/ER-X devices. Modern UXG/UDM series incur <1% CPU overhead. Always conduct post-implementation speed tests to validate gateway sizing.204
Test QoS efficacy under load:
- Bufferbloat: Use waveform.bufferbloat.net or DSL Reports speedtest (target A+ grade)
- VoIP: Concurrent calls + bulk upload (e.g., iCloud backup) → measure MOS score and packet loss
- Gaming: Saturate link with downloads while monitoring ping/jitter to game servers
- Video: 4K stream + software update download → verify playback continuity1216
Maintain a profile registry (YAML/JSON) with:
- Profile name, version, target environment
- WAN speed range, Smart Queue rates, QoS rule UUIDs
- DSCP mappings, VLAN IDs, port profiles
- Validation test results and known limitations
The following six profiles provide reusable archetypes for MCP server deployments. Each profile specifies intent, traffic class focus, UniFi implementation details, and recommended bandwidth allocations.
| Profile Name | Traffic Class Focus | Smart Queues | DSCP Tags | VLAN Strategy | Bandwidth Limits | Primary UniFi Knobs | 
|---|---|---|---|---|---|---|
| Voice-First | VoIP RTP, SIP signaling | Optional (<300 Mbps) | EF 46 (RTP), CS3 (SIP) | Dedicated voice VLAN | Reserve 30% upstream for voice | QoS rule (Prioritize voice VLAN), Smart Queues at 85% | 
| Video-Conferencing | Zoom, Teams, WebRTC | Optional (<300 Mbps) | AF41 34, EF 46 (if phone) | Trusted devices VLAN | Min 3 Mbps per concurrent HD stream | QoS rule (Prioritize AF41), DSCP trust on switch | 
| Cloud-Gaming | Stadia, GeForce Now, XCloud | Recommended (<300 Mbps) | EF 46 | Gaming device group | Min 15 Mbps down, 5 Mbps up | QoS rule (Prioritize gaming IPs), Smart Queues | 
| Streaming-Media | Netflix, YouTube 4K | Not recommended | AF41 34, AF31 26 | Media player VLAN | Limit bulk to 50% during peak | QoS rule (Limit Windows Update, etc.) | 
| Bulk-Backup | iCloud, OneDrive, Backblaze | Not recommended | CS1 8 (Scavenger) | Separate backup VLAN | Cap at 30% of WAN, de-prioritize | QoS rule (Limit backup VLAN to 30 Mbps) | 
| Guest-Best-Effort | Guest Wi-Fi, IoT | Not recommended | CS0 0 (default) | Guest VLAN only | Hard limit 5–10 Mbps per client | QoS rule (Limit guest network), port isolation | 
Table: Core QoS profile library for MCP deployments
- Intent: Ensure toll-quality VoIP regardless of background load
- Target: SMB offices, call centers, home offices with hosted PBX
- Implementation:
  - Create dedicated Voice VLAN (e.g., VLAN 110)
  - Enable QoS tagging on voice network: UniFi Network → Settings → Networks → [Voice VLAN] → Advanced → QoS Priority = High
  - Configure QoS rule: Objective = Prioritize, Source = Network (Voice VLAN), Destination = Any
  - If WAN <300 Mbps: Enable Smart Queues at 85% of tested rate to prevent bufferbloat from bulk uploads
  - On switches: Set port priority to Critical for phone ports; trust DSCP EF 46 from phones
- DSCP Handling: Preserve EF 46 from endpoints; remark untrusted devices to CS0
- Validation: 10 concurrent calls + 50 Mbps upload → MOS >4.0, zero packet loss
- Intent: Stable HD video for remote work; minimize freezes and "your connection is unstable"
- Target: Remote workers, executive home offices, hybrid meeting rooms
- Implementation:
  - Use Trusted Devices VLAN for work laptops and room systems
  - QoS rule: Objective = Prioritize, Source = Device Group (video endpoints), Destination = App (Zoom, Teams, Webex)
  - Match DSCP AF41 from conferencing apps; if missing, remark at switch ingress
  - Smart Queues optional at 90% rate for links <200 Mbps; omit for gigabit fiber
- Bandwidth: Reserve 3 Mbps per HD stream upstream; 1.5 Mbps for 720p
- Validation: 3 concurrent meetings + bulk download → <2% frame loss, latency <100 ms
- Intent: Low latency, jitter, and packet loss for cloud gaming platforms
- Target: Residential gaming, esports venues, gaming lounges
- Implementation:
  - Create Gaming Device Group (by MAC or static IP)
  - QoS rule: Objective = Prioritize, Source = Device Group, Destination = IP ranges (Stadia, GeForce Now, XCloud)
  - Enable Smart Queues at 85–90% of WAN rate; fq_codel automatically prioritizes small packets
  - On switches: Set gaming ports to High priority; disable EEE (Energy Efficient Ethernet) to reduce latency
- DSCP: Mark as EF 46 if platform supports it; trust endpoint markings
- Validation: Gaming latency <50 ms, jitter <10 ms while saturating link with downloads
- Intent: Ensure 4K streaming quality without buffering; deprioritize background bulk
- Target: Media rooms, residential entertainment networks
- Implementation:
  - No Smart Queues (streaming benefits from high throughput, not shaping)
  - QoS rule: Objective = Limit, Source = Any, Destination = App (Windows Update, iCloud Backup, OneDrive) → cap at 50% of downstream during peak hours
  - Use Schedule to allow unlimited bulk during off-peak (e.g., 2 AM–6 AM)
  - VLAN optional; prioritize by application signature rather than network segment
- DSCP: Use AF41 for streaming devices if switch supports remarking; otherwise rely on endpoint defaults
- Validation: 4K stream maintains 25 Mbps sustained; background updates do not cause buffering
- Intent: Contain backup/sync traffic to prevent WAN saturation and latency spikes
- Target: Remote offices with cloud backup, MSP-managed endpoints
- Implementation:
  - Create Backup VLAN (e.g., VLAN 900) or Device Group for backup agents
  - QoS rule: Objective = Limit, Source = Network (Backup VLAN), Destination = Any → 30 Mbps down, 10 Mbps up (adjust per link)
  - Mark traffic as DSCP CS1 (Scavenger) at switch ingress to ensure de-prioritization if it exits via alternate path
  - Disable Smart Queues; shaping already applied via rate limit
- Schedule: Allow higher limits during maintenance windows (e.g., weekends)
- Validation: Bulk transfer capped at defined rate; ping to 8.8.8.8 remains <20 ms during backup
- Intent: Isolate guest traffic; prevent guest devices from impacting business or primary traffic
- Target: Guest Wi-Fi, IoT networks, untrusted devices
- Implementation:
  - Guest VLAN with Hotspot or WPA2-Enterprise
  - QoS rule: Objective = Limit, Source = Network (Guest VLAN), Destination = Any → 5–10 Mbps per client, 50 Mbps aggregate
  - Enable Port Isolation on switches to prevent guest-to-guest communication
  - No DSCP trust; remark all guest traffic to CS0 at switch ingress
  - Disable inter-VLAN routing to guest VLAN (layer-2 only)
- Validation: Guest speedtest shows capped rate; business VLAN throughput unaffected
Smart Queues Throughput Ceiling: Legacy USG/ER-X platforms experience drastic throughput reduction (to ~280 Mbps) when Smart Queues enable software-forwarding paths. Modern UXG-Pro/UDM-Pro handle SQM with <1% CPU overhead, but rates >300 Mbps still render SQM unnecessary.14520
QoS Rules and Offload: Enabling any QoS rule disables hardware offload on pre-UDM platforms. Validate gateway CPU capacity under peak load; consider upgrade if sustained throughput drops below business requirements.217
Switch Model Limitations: Not all UniFi switches support DSCP remarking in current firmware. Verify feature support (Settings → Profiles → Ethernet Ports → Manual → Enable QoS) before designing trust-boundary policies.8
Smart Queues + QoS Rules: Concurrent use is supported but requires careful ordering. Smart Queues shape first; QoS rules apply within shaped bandwidth. Avoid double-shaping (e.g., Smart Queues at 90 Mbps + QoS rule limiting device to 80 Mbps) as it creates unpredictable behavior.2212
Per-SSID Bandwidth Profiles: Wi-Fi bandwidth limits (Settings → WiFi → [SSID] → Bandwidth Profile) operate independently of gateway QoS. Use these for airtime fairness, not WAN shaping. Combining SSID limits with Smart Queues can lead to unintended starvation.2312
DSCP Trust and WMM: UniFi APs automatically map DSCP to WMM queues. However, if QoS is disabled on the SSID or AP radio, DSCP is ignored and all traffic falls into Best Effort. Ensure QoS is enabled on all SSIDs carrying prioritized traffic.109
- Over-subscribing priority queues: Marking video, gaming, voice, and critical apps all as "Critical" results in none receiving true priority. Limit high-priority classes to ≤3.197
- Mismatched DSCP trust: Phones mark EF 46, but switch remarks to CS0; or switch trusts DSCP but gateway QoS rule matches on port only. Align trust boundaries end-to-end.178
- Smart Queues on gigabit fiber: Unnecessary latency injection; policy QoS alone suffices. Disable Smart Queues for >300 Mbps circuits.51
- Ignoring SIP ALG: SIP ALG on UniFi gateways breaks SIP signaling for most modern platforms. Disable under Settings → Internet → [WAN] → Advanced → SIP ALG.16
- Setting Smart Queues to 100% ISP rate: Fails to absorb ISP bufferbloat; always set 10–20% below tested rate.1312
Continuous Monitoring: Use UniFi Insights → Real-Time Traffic to verify QoS rule hits and bandwidth consumption. Graph Smart Queue drops and latency metrics if available.
Bufferbloat Testing: Monthly waveform.bufferbloat.net tests; target A+ grade with <20 ms latency increase under load. If grade drops, adjust Smart Queue rates downward by 5% increments.2412
VoIP MOS Scoring: Deploy synthetic call probes (e.g., ThousandEyes, PingPlotter) to measure MOS, packet loss, and jitter during peak hours. Target MOS >4.0, loss <0.5%, jitter <30 ms.16
Change Control: Version profiles in Git; apply via UniFi API or Settings → Advanced → Backup/Restore. Document WAN speed, Smart Queue rates, and test results per site.
For MCP server rollouts, standardize on the six core profiles defined in Section 4.2, selecting the appropriate profile based on site function (office, residential, gaming lounge). Always shape at the WAN edge using Smart Queues for links <300 Mbps or policy QoS for higher bandwidth. Preserve DSCP EF 46 for voice end-to-end; trust endpoint markings where possible. Disable SIP ALG and restrict port forwards to required ranges only. Validate each deployment with bufferbloat and synthetic VoIP testing, and maintain versioned profile documentation in infrastructure-as-code repositories.
The profile library provides a foundation for repeatable, supportable QoS policies across diverse UniFi deployments, balancing latency sensitivity for real-time applications with bandwidth availability for bulk transfers.