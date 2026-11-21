---
id: collect-261121-veille-ia/mikrotik/github-whilcayangyang-mikrotik-homelab-config-a-production-grade-routeros-7-configuration-for-a-segm
title: "GitHub - whilcayangyang/mikrotik-homelab-config: A production-grade RouterOS 7 configuration for a segmented homelab network: VLAN isolation, per-device ProtonVPN policy-based routing (kill-switched), hardened admin access with brute-force lockout, and QoS."
domain: mikrotik
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["interface", "list", "bridge1", "name", "vlan", "bridge", "pvid", "network"]
source: https://github.com/whilcayangyang/mikrotik-homelab-config
source_anchor: ""
source_lines: [1, 491]
section: "Copilot_Microsoft"
---
A production-grade RouterOS 7 configuration for a segmented homelab network: VLAN isolation, per-device ProtonVPN policy-based routing (kill-switched), hardened admin access with brute-force lockout, and QoS.

**Disclaimer:** All IP addresses, MAC addresses, hostnames, domains, keys, and credentials in this README are **placeholder examples**. Replace every `<...>` and example value with your own before deploying. Do not reuse the example WireGuard keys or AWS credentials shown here — they are illustrative only.


| Item | Example Value | Notes | 
|---|---|---|
| RouterOS version | `7.21.4` | Stable/long-term channel recommended for production | 
| Model | `C53UiG+5HPaxD2HPaxD` | e.g. MikroTik hAP ax³ or similar dual-band Wi-Fi 6 router | 
| Update channel | `long-term` | Avoid `testing` /`current` on a router you depend on | 

**Note:** Serial number and software ID are omitted intentionally — never publish these, they can be used for warranty/support impersonation.


| VLAN | Name | Subnet (example) | Purpose | 
|---|---|---|---|
| 32 | `vlan32-private` | `172.31.62.0/26` | Trusted personal devices (also doubles as MGMT-reachable) | 
| 33 | `vlan33-server` | `172.31.62.128/26` | Homelab / server tier (K8s, VMs, sandboxes) | 
| 34 | `vlan34-mgmt` | `172.31.62.192/26` | Out-of-band management network | 
| 35 | `vlan35-public` | `172.31.62.64/26` | Guest / untrusted devices, isolated from LAN | 

```
                 ┌─────────────┐
   WAN (ether2) ─┤   MikroTik  │
                 │   Router    │
                 └──────┬──────┘
                        │ bridge1 (VLAN-filtering)
        ┌───────┬───────┼───────┬────────┐
     VLAN32   VLAN33  VLAN34  VLAN35   WireGuard
    (private) (server) (mgmt) (public)  (wg1/wg2 -> ProtonVPN)
```
A single bridge (`bridge1`) with VLAN filtering enabled carries all internal traffic; physical/Wi-Fi ports are assigned a **PVID** to tag untagged traffic into the correct VLAN.

```
/interface bridge
add name=bridge1 vlan-filtering=yes
/interface vlan
add interface=bridge1 name=vlan32-private vlan-id=32
add interface=bridge1 name=vlan33-server  vlan-id=33
add interface=bridge1 name=vlan34-mgmt    vlan-id=34
add interface=bridge1 name=vlan35-public  vlan-id=35
/interface bridge port
add bridge=bridge1 interface=ether1 pvid=33 frame-types=admit-only-untagged-and-priority-tagged
add bridge=bridge1 interface=ether3 pvid=32 frame-types=admit-only-untagged-and-priority-tagged
add bridge=bridge1 interface=ether4 pvid=32 frame-types=admit-only-untagged-and-priority-tagged
add bridge=bridge1 interface=wifi1  pvid=32 frame-types=admit-only-untagged-and-priority-tagged
add bridge=bridge1 interface=wifi2  pvid=35 frame-types=admit-only-untagged-and-priority-tagged
```
**Caution:** `frame-types=admit-only-untagged-and-priority-tagged` silently drops any tagged frame arriving on that port that doesn't match the PVID. This is intentional here (prevents VLAN-hopping from a compromised endpoint) but will break trunking if you later connect a switch expecting tagged traffic on that port.


Interface lists group physical/logical interfaces for use in firewall rules instead of hardcoding interface names everywhere.

```
/interface list
add name=LAN
add name=WAN
add name=MGT
/interface list member
add interface=ether2           list=WAN
add interface=vlan32-private   list=LAN
add interface=vlan33-server    list=LAN
add interface=vlan34-mgmt      list=LAN
add interface=vlan35-public    list=LAN
add interface=wg1              list=WAN
add interface=wg2              list=WAN
add interface=lo               list=LAN
add interface=vlan32-private   list=MGT
add interface=vlan34-mgmt      list=MGT
```
**Note:** WireGuard interfaces (`wg1`, `wg2`) are added to the `WAN` list. This matters for firewall rules like "drop non-DSTNAT WAN input" — traffic arriving from the VPN tunnel is treated the same as internet-facing WAN traffic for forwarding purposes.


Two SSIDs are defined: a WPA3-only network for trusted devices, and a WPA2/WPA3-mixed guest network (for device compatibility) mapped to the isolated public VLAN.

```
/interface wifi security
add name="Home-Trusted" authentication-types=wpa3-psk encryption=ccmp,gcmp \
    management-protection=required management-encryption=gmac-256 wps=disable disabled=no
add name="Home-Guest"   authentication-types=wpa2-psk,wpa3-psk encryption=ccmp,gcmp \
    wps=disable disabled=no
/interface wifi configuration
add name=cfg1 mode=ap ssid="Home-Trusted" security="Home-Trusted" \
    datapath.client-isolation=yes \
    channel.band=5ghz-ax channel.frequency=5785 channel.width=20/40/80mhz \
    channel.skip-dfs-channels=disabled country=<your-country> max-clients=8 disabled=no
add name=cfg2 mode=ap ssid="Home-Guest" security="Home-Guest" \
    datapath.client-isolation=yes \
    channel.band=2ghz-ax channel.frequency=2437,2462 channel.width=20mhz \
    channel.skip-dfs-channels=10min-cac country=<your-country> max-clients=12 disabled=no
/interface wifi
set [ find default-name=wifi1 ] configuration=cfg1 configuration.mode=ap disabled=no
set [ find default-name=wifi2 ] configuration=cfg2 configuration.mode=ap disabled=no
```
**Warning:** Use WPA3-only (`wpa3-psk`) wherever every client supports it. Only fall back to mixed WPA2/WPA3 for a legacy-device SSID, and put that SSID on an isolated/untrusted VLAN (as done here with `vlan35-public`) so a downgrade attack on the weaker network doesn't expose the trusted tier.


**Note:** `management-protection=required` (PMF/802.11w, mandatory under WPA3) and `wps=disable` should be set on every SSID — WPS is a well-known brute-force vector regardless of the underlying authentication type. `datapath.client-isolation=yes` blocks client-to-client traffic within each SSID, which matters most on the guest network but costs nothing on the trusted one either. `cfg2`'s `channel.frequency=2437,2462` gives RouterOS a list of allowed 2.4GHz channels to auto-select from (channels 6 and 11) rather than pinning to one, avoiding manual retuning if a neighbor collides on the same channel.


| Pool | Range (example) | Bound to | 
|---|---|---|
| `vlan32-private-pool` | `172.31.62.2 – 172.31.62.62` | `vlan32-private` | 
| `vlan35-public-pool` | `172.31.62.66 – 172.31.62.126` | `vlan35-public` | 
| `vlan33-server-pool` | `172.31.62.130 – 172.31.62.190` | `vlan33-server` | 

```
/ip pool
add name=vlan32-private-pool ranges=172.31.62.2-172.31.62.62
add name=vlan35-public-pool  ranges=172.31.62.66-172.31.62.126
add name=vlan33-server-pool          ranges=172.31.62.130-172.31.62.190
/ip dhcp-server
add name=dhcp-vlan32-private address-pool=vlan32-private-pool interface=vlan32-private
add name=dhcp-vlan35-public  address-pool=vlan35-public-pool  interface=vlan35-public
add name=dhcp-vlan33-server  address-pool=vlan33-server-pool          interface=vlan33-server
/ip dhcp-server network
add address=172.31.62.0/26   gateway=172.31.62.1   dns-server=10.2.0.1
add address=172.31.62.64/26  gateway=172.31.62.65  dns-server=172.31.62.65
add address=172.31.62.128/26 gateway=172.31.62.129 dns-server=1.1.1.2,1.0.0.2
```
**Note:** Each VLAN's DHCP-issued DNS server differs on purpose:


`vlan32-private` clients are pointed at `10.2.0.1` — the DNS server *inside the WireGuard tunnel* (ProtonVPN-provided DNS), so trusted clients' DNS queries are also tunneled and don't leak.
`vlan35-public` clients resolve via the router itself (`172.31.62.65`), which is redirected to a local resolver (see DNS redirect NAT rule below).
`vlan33-server` clients use Cloudflare's malware-filtering resolver (`1.1.2.2`/`1.0.0.2`) directly, since server-tier traffic is not policy-routed through the VPN.

```
/ip dhcp-server lease
add address=172.31.62.2 mac-address=<AA:BB:CC:DD:EE:01> client-id=1:<...> server=dhcp-vlan32-private comment="desktop-1"
add address=172.31.62.3 mac-address=<AA:BB:CC:DD:EE:02> client-id=1:<...> server=dhcp-vlan32-private comment="laptop-1"
add address=172.31.62.5 mac-address=<AA:BB:CC:DD:EE:03> client-id=1:<...> server=dhcp-vlan32-private comment="phone-1"
add address=172.31.62.66 mac-address=<AA:BB:CC:DD:EE:04> client-id=1:<...> server=dhcp-vlan35-public comment="guest-phone"
add address=172.31.62.71 mac-address=<AA:BB:CC:DD:EE:05> client-id=1:<...> server=dhcp-vlan35-public comment="android-tv"
add address=172.31.62.130 mac-address=<AA:BB:CC:DD:EE:06> client-id=1:<...> server=dhcp-vlan33-server comment="k8s-node-1"
```
**Caution:** Static leases pin a device's IP, which is what makes the address-list-based firewall rules below (per-device VPN routing) possible. If you clone MAC addresses or use MAC randomization on a "trusted" device, its DHCP lease — and therefore its VPN routing/firewall treatment — will silently stop matching.


```
/ip dns
set servers=172.31.62.131 allow-remote-requests=yes verify-doh-cert=yes
```
**Warning:** `allow-remote-requests=yes` makes the router itself answer DNS queries from LAN clients. Only enable this if you intend the router to be a resolver/forwarder (as used by the public-VLAN DNS-redirect rule below) — otherwise this needlessly expands attack surface.


FQ-CoDel queues reduce bufferbloat on both the WAN-facing VLANs and the VPN tunnels.

```
/queue type
add name=fq-codel    kind=fq-codel
add name=fq-codel-wg kind=fq-codel fq-codel-flows=256 fq-codel-target=2ms
/queue simple
add name=protonvpn-wg1 target=wg1 max-limit=180M/100M queue=fq-codel-wg/fq-codel-wg
add name=protonvpn-wg2 target=wg2 max-limit=180M/100M queue=fq-codel-wg/fq-codel-wg
add name=fqcodel-vlan32 target=ether2 dst=vlan32-private max-limit=350M/200M queue=fq-codel/fq-codel
add name=fqcodel-vlan33 target=ether2 dst=vlan33-server  max-limit=350M/200M queue=fq-codel/fq-codel
add name=fqcodel-vlan35 target=ether2 dst=vlan35-public  max-limit=100M/100M queue=fq-codel/fq-codel
```
**Note:** `max-limit` values should reflect your actual WAN plan speed (up to ~90% of it) — set too high, shaping never engages; set too low, you throttle yourself. Values above are examples only.


Two independent WireGuard tunnels to different ProtonVPN server locations allow routing different device groups through different exit nodes (e.g., splitting traffic across two VPN endpoints for redundancy or geo-diversity).

```
/interface wireguard
add name=wg1 listen-port=<13746> mtu=1420 comment="protonvpn-<location-1>"
add name=wg2 listen-port=<8896>  mtu=1420 comment="protonvpn-<location-2>"
/interface wireguard peers
add interface=wg1 name=peer1 comment="protonvpn-<location-1>" \
    public-key="<PROTONVPN_SERVER_PUBLIC_KEY_1>" \
    endpoint-address=<203.0.113.10> endpoint-port=51820 \
    allowed-address=0.0.0.0/0 client-allowed-address=::/0 persistent-keepalive=25s
add interface=wg2 name=peer2 comment="protonvpn-<location-2>" \
    public-key="<PROTONVPN_SERVER_PUBLIC_KEY_2>" \
    endpoint-address=<203.0.113.20> endpoint-port=51820 \
    allowed-address=0.0.0.0/0 client-allowed-address=::/0 persistent-keepalive=25s
/ip address
add address=10.2.0.2 interface=wg1 network=10.2.0.0 comment=protonvpn-wg1
add address=10.2.0.2 interface=wg2 network=10.2.0.0 comment=protonvpn-wg2
```
**How to get these values:** Generate a WireGuard config from the ProtonVPN dashboard (Downloads → WireGuard configuration). It gives you your private key, the server's public key, the assigned tunnel address, and the endpoint IP/port — import those in place of the placeholders above.


**Warning:** Never commit your **private key** (`private-key=` under `/interface wireguard`) to any repository. It is omitted entirely from this example — RouterOS stores it in the interface config, not shown by default in `export` output unless you pass `show-sensitive`.


**Caution:** Both peers reuse the same tunnel address (`10.2.0.2`) here because each is on its own separate WireGuard interface (`wg1`/`wg2`) with its own routing table — this is correct for split tunnels, but would be a conflict if both were on the same interface.


This is the core mechanism that routes **specific devices** through **specific VPN tunnels**, while everything else (server tier, explicitly excluded devices) uses the normal WAN path — with a firewall-enforced kill switch (see §7) so VPN clients never leak traffic over plain WAN if the tunnel drops.

```
/ip firewall address-list
add list=protonvpn-wg1 address=<172.31.62.2>  comment="desktop-1"
add list=protonvpn-wg1 address=<172.31.62.3>  comment="laptop-1"
add list=protonvpn-wg1 address=<172.31.62.4>  comment="laptop-1-lan"
add list=protonvpn-wg2 address=<172.31.62.5>  comment="phone-1"
add list=non-vpn       address=<172.31.62.66> comment="guest-phone"
add list=non-vpn       address=<172.31.62.71> comment="android-tv"
add list=non-vpn       address=<172.31.62.73> comment="iot-device-1"
```
```
/routing table
add name=to-wg1 fib disabled=no
add name=to-wg2 fib disabled=no
```
```
/ip route
add routing-table=to-wg1 dst-address=0.0.0.0/1   gateway=wg1 distance=1 scope=30 target-scope=10
add routing-table=to-wg1 dst-address=128.0.0.0/1 gateway=wg1 distance=1 scope=30 target-scope=10
add routing-table=to-wg2 dst-address=0.0.0.0/1   gateway=wg2 distance=1 scope=30 target-scope=10
add routing-table=to-wg2 dst-address=128.0.0.0/1 gateway=wg2 distance=1 scope=30 target-scope=10
# Route to the VPN endpoint itself must go via the real WAN gateway, not the tunnel:
add routing-table=to-wg1 dst-address=<203.0.113.10>/32 gateway=<192.0.2.1> distance=1 scope=30 target-scope=10
add routing-table=to-wg2 dst-address=<203.0.113.20>/32 gateway=<192.0.2.1> distance=1 scope=30 target-scope=10
```
**Note:** The `0.0.0.0/1` + `128.0.0.0/1` split-default-route trick covers the full IPv4 space (`0.0.0.0/0`) across two routes. This is a common workaround so the "default route" in a custom routing table doesn't collide with/get overridden by the main table's real default route.


```
/ip firewall mangle
add chain=prerouting action=accept comment="bypass LAN-to-LAN" dst-address-list=lan-network
add chain=prerouting action=mark-connection new-connection-mark=wg1-client-conn \
    connection-state=new src-address-list=protonvpn-wg1
add chain=prerouting action=mark-connection new-connection-mark=wg2-client-conn \
    connection-state=new src-address-list=protonvpn-wg2
add chain=prerouting action=mark-packet new-packet-mark=wg1-client-pkt connection-mark=wg1-client-conn
add chain=prerouting action=mark-routing new-routing-mark=to-wg1 connection-mark=wg1-client-conn \
    src-address-list=protonvpn-wg1 passthrough=no
add chain=prerouting action=mark-packet new-packet-mark=wg2-client-pkt connection-mark=wg2-client-conn
add chain=prerouting action=mark-routing new-routing-mark=to-wg2 connection-mark=wg2-client-conn \
    src-address-list=protonvpn-wg2 passthrough=no
```
**Caution:** Mangle rule order matters — `mark-connection` must precede `mark-routing`/`mark-packet` referencing that connection mark, and the LAN-to-LAN bypass rule must come first so intra-LAN traffic (e.g., a VPN-routed laptop talking to a local NAS) isn't force-routed out through the tunnel.


```
/ip firewall nat
add chain=srcnat action=masquerade out-interface-list=WAN src-address-list=lan-network
```
**Note:** Because `wg1`/`wg2` are members of the `WAN` interface list (§1), this single masquerade rule also covers VPN-tunneled egress — no separate NAT rule is needed per tunnel.


The filter table is organized into four chains, executed in this order: `input` → `management` (jump target) → `forward` → `vlan-acl` / `port-restriction` (jump targets).

| # | Action | Match | Purpose | 
|---|---|---|---|
| 1.1 | accept | `connection-state=established,related,untracked` | Fast-path return traffic | 
| 1.2 | drop | `connection-state=invalid` | Reject malformed/invalid packets | 
| 1.3 | accept | `protocol=icmp` | Allow ping for diagnostics | 
| 1.4 | accept | `dst-address=127.0.0.1` | Loopback | 
| 1.5 | jump | → `management` chain | Route admin-access checks | 
| 1.6 | drop | `in-interface-list=!LAN` | Drop anything not from a defined LAN interface | 
| 1.7 | drop | (all) | Explicit final deny | 

```
/ip firewall filter
add chain=input action=accept connection-state=established,related,untracked comment="1.1 ACCEPT: Established/Related/Untracked"
add chain=input action=drop   connection-state=invalid                       comment="1.2 DROP: Invalid Packets"
add chain=input action=accept protocol=icmp                                  comment="1.3 ACCEPT: ICMP (Ping)"
add chain=input action=accept dst-address=127.0.0.1                          comment="1.4 ACCEPT: Loopback"
add chain=input action=jump jump-target=management                          comment="1.5 JUMP: Check Management Access"
add chain=input action=drop   in-interface-list=!LAN                         comment="1.6 DROP: Non-LAN Input"
add chain=input action=drop                                                  comment="1.7 DROP: Everything Else"
```
Implements a 3-stage progressive blacklist for SSH/HTTPS/Winbox login attempts, similar in spirit to fail2ban.

```
/ip firewall address-list
add list=allowed_to_router address=<192.168.88.0/30>   comment="mgmt-p2p-link"
add list=allowed_to_router address=<172.31.62.192/26>  comment="vlan34-mgmt"
add list=allowed_to_router address=<172.31.62.2-172.31.62.5> comment="trusted-devices"
add list=allowed_to_router address=<172.31.62.128/29>  comment="k8s-homelab"
/ip firewall filter
add chain=management action=accept src-address-list=allowed_to_router \
    comment="2.0 ALLOW: Whitelisted Admin IPs Skip Blacklist Check"
add chain=management action=drop src-address-list=admin-blacklist \
    comment="2.0b DROP: Blacklisted Brute-Force Source"
add chain=management action=add-src-to-address-list address-list=admin-blacklist address-list-timeout=1d \
    connection-state=new protocol=tcp dst-port=22,443,8291 src-address-list=admin-stage2 \
    comment="2.0c BLACKLIST: 3rd Failed Attempt in 5 Min"
add chain=management action=add-src-to-address-list address-list=admin-stage2 address-list-timeout=1m \
    connection-state=new protocol=tcp dst-port=22,443,8291 src-address-list=admin-stage1 \
    comment="2.0d STAGE2: 2nd Attempt in 1 Min"
add chain=management action=add-src-to-address-list address-list=admin-stage1 address-list-timeout=1m \
    connection-state=new protocol=tcp dst-port=22,443,8291 \
    comment="2.0e STAGE1: 1st Attempt"
add chain=management action=accept protocol=tcp dst-port=22,80,443,8291 src-address-list=allowed_to_router \
    comment="2.1 ALLOW: Admin Access (TCP)"
add chain=management action=accept protocol=udp dst-port=5678 src-address-list=allowed_to_router \
    comment="2.2 ALLOW: Admin Access (UDP, MNDP/neighbor discovery)"
add chain=management action=accept protocol=udp dst-port=53,67-68,123 src-address-list=lan-network \
    comment="2.3 ALLOW: DNS/DHCP/NTP (UDP)"
add chain=management action=accept protocol=tcp dst-port=53 src-address-list=lan-network \
    comment="2.4 ALLOW: DNS (TCP)"
add chain=management action=return comment="2.5 RETURN: To Input Chain (for Drop)"
```
**How the staged lockout works:** Any *new* connection attempt to an admin port (22/443/8291) from a source not already whitelisted gets added to `admin-stage1` (1 min TTL). A second attempt within that minute promotes the source into `admin-stage2` (also 1 min TTL). A third attempt while still in stage 2 promotes it to `admin-blacklist` for 1 day, at which point rule `2.0b` drops it outright. Legitimate admins should always be in `allowed_to_router` so they skip this entirely (rule `2.0`).


**Warning:** This throttles brute force but is not a substitute for strong authentication. Also enforce: disabled password-only root/admin login where possible, key-based SSH, and a non-default admin username.


```
/ip firewall filter
add chain=forward action=fasttrack-connection connection-mark=no-mark connection-state=established,related \
    comment="3.1 FASTTRACK: Standard Traffic (Excludes VPN)"
add chain=forward action=accept connection-state=established,related,untracked \
    comment="3.2 ACCEPT: Established/Related/Untracked"
add chain=forward action=drop connection-state=invalid \
    comment="3.3 DROP: Invalid Packets"
add chain=forward action=drop connection-nat-state=!dstnat connection-state=new in-interface-list=WAN \
    comment="3.4 DROP: WAN Garbage (Non-DSTNAT)"
add chain=forward action=drop src-address-list=no_forward_ipv4 \
    comment="3.5 DROP: Bad Public IPs (RFC6890/multicast/broadcast)"
add chain=forward action=jump jump-target=vlan-acl \
    comment="3.6 JUMP: Check VLAN ACLs"
add chain=forward action=jump jump-target=port-restriction \
    comment="3.7 JUMP: Check Port Restrictions"
add chain=forward action=drop \
    comment="3.8 DROP: Block Unknown Forwarding"
```
**Note:** Rule `3.1` deliberately excludes marked (VPN-policy-routed) connections from fasttrack — fasttracked packets bypass further firewall/mangle processing, which would break the kill-switch and PBR marking for VPN traffic. Only unmarked ("no-mark") traffic gets the fasttrack performance boost.


**Caution:** `3.5` (`no_forward_ipv4`) should reference an address-list containing RFC 6890 reserved ranges (`0.0.0.0/8`, `169.254.0.0/16`), multicast (`224.0.0.0/4`), and limited broadcast (`255.255.255.255`) — this is MikroTik's default `defconf` bogon list, not a full bogon feed. For internet-facing routers, consider adding a full bogon list.


| Rule | Allow | 
|---|---|
| 4.1 / 4.2 | `vlan32-private` ⇄`vlan33-server` | 
| 4.3 / 4.4 | `vlan33-server` ⇄`vlan34-mgmt` | 
| 4.5 / 4.6 | `vlan34-mgmt` ⇄`allowed_to_router` | 
| 4.7 / 4.7.1 | `vlan35-public` → homelab HTTP/S service only (specific host, ports 80/443) | 
| 4.8 / 4.8.1 | `non-vpn` list → homelab SSH/sandbox service only (specific host, port 22) | 
| 4.9 | Drop everything else between LAN networks | 
| 4.10 | Return to forward chain for internet-egress check | 

```
/ip firewall filter
add chain=vlan-acl action=accept src-address-list=vlan32-private dst-address-list=vlan33-server \
    comment="4.1 ALLOW: VLAN32 <-> VLAN33"
add chain=vlan-acl action=accept src-address-list=vlan33-server dst-address-list=vlan32-private \
    comment="4.2 ALLOW: VLAN33 <-> VLAN32"
add chain=vlan-acl action=accept src-address-list=vlan34-mgmt dst-address-list=vlan33-server \
    comment="4.3 ALLOW: VLAN34 <-> VLAN33"
add chain=vlan-acl action=accept src-address-list=vlan33-server dst-address-list=vlan34-mgmt \
    comment="4.4 ALLOW: VLAN33 <-> VLAN34"
add chain=vlan-acl action=accept src-address-list=vlan34-mgmt dst-address-list=allowed_to_router \
    comment="4.5 ALLOW: Mgmt <-> Allowed"
add chain=vlan-acl action=accept src-address-list=allowed_to_router dst-address-list=vlan34-mgmt \
    comment="4.6 ALLOW: Allowed <-> Mgmt"
add chain=vlan-acl action=accept src-address-list=vlan35-public dst-address=<172.31.62.130> protocol=tcp dst-port=80,443 \
    comment="4.7 ALLOW: Public -> Homelab (HTTP/S)"
add chain=vlan-acl action=accept src-address=<172.31.62.130> src-port=80,443 protocol=tcp dst-address-list=vlan35-public \
    comment="4.7.1 ALLOW: Homelab -> Public (Return)"
add chain=vlan-acl action=accept src-address-list=non-vpn dst-address=<172.31.62.132> protocol=tcp dst-port=22 \
    comment="4.8 ALLOW: Public -> Homelab (sandbox SSH)"
add chain=vlan-acl action=accept src-address=<172.31.62.132> src-port=22 protocol=tcp dst-address-list=non-vpn \
    comment="4.8.1 ALLOW: Homelab -> Public (Return)"
add chain=vlan-acl action=drop src-address-list=lan-network dst-address-list=lan-network \
    comment="4.9 DROP: Block Remaining Inter-VLAN"
add chain=vlan-acl action=return comment="4.10 RETURN: Proceed to Internet Check"
```
**Note:** This is default-deny between VLANs — only the explicit pairs above are permitted; rule `4.9` closes everything else before falling through to internet-egress handling.


```
/ip firewall filter
add chain=port-restriction action=accept src-address-list=vlan33-server \
    comment="5.1a ALLOW: VLAN33 Server Full Egress (trusted server tier)"
add chain=port-restriction action=accept src-address-list=protonvpn-wg1 \
    comment="5.1b ALLOW: VPN Clients Full Egress (WG1, tunneled+killswitched)"
add chain=port-restriction action=accept src-address-list=protonvpn-wg2 \
    comment="5.1c ALLOW: VPN Clients Full Egress (WG2, tunneled+killswitched)"
add chain=port-restriction action=accept protocol=icmp src-address-list=lan-network \
    comment="5.2 ALLOW: ICMP"
add chain=port-restriction action=accept protocol=tcp dst-port=53,80,443,465,587,993,995 src-address-list=lan-network \
    comment="5.3 ALLOW: Standard Web/Mail Ports (TCP)"
add chain=port-restriction action=accept protocol=udp dst-port=53,123 src-address-list=lan-network \
    comment="5.4 ALLOW: Standard DNS/NTP (UDP)"
add chain=port-restriction action=drop src-address-list=lan-network \
    comment="5.5 DROP: Strict Policy for Remaining LAN"
```
**Warning:** This is the actual **kill switch**. Devices in `protonvpn-wg1`/`protonvpn-wg2` get full egress (rules 5.1b/5.1c) — but that rule is only reachable *after* mangle has policy-routed their traffic into the `to-wg1`/`to-wg2` tables. If a WireGuard tunnel goes down, the policy route has no valid gateway and the packet is dropped rather than falling back to the main routing table/WAN — so a VPN client's traffic simply stops instead of leaking in plaintext. Any other LAN device is restricted to a narrow allow-list of web/mail/DNS/NTP ports (rules 5.2–5.4) and denied by default (5.5).


**Caution:** The narrow port allow-list in 5.3/5.4 means many non-web protocols (custom app ports, gaming, some VoIP) will be silently dropped for non-VPN, non-server-tier devices. Add explicit rules above `5.5` for any additional ports you need.


```
/ip firewall nat
add chain=srcnat action=masquerade out-interface-list=WAN src-address-list=lan-network \
    comment="masquerade: lan-network"
add chain=dstnat action=redirect protocol=udp dst-port=53 in-interface-list=LAN \
    src-address-list=vlan35-public to-ports=53 \
    comment="Force vlan35-public DNS to Router (UDP)"
add chain=dstnat action=redirect protocol=tcp dst-port=53 in-interface-list=LAN \
    src-address-list=vlan35-public to-ports=53 \
    comment="Force vlan35-public DNS to Router (TCP)"
```
**Note:** The DNS-redirect rules force the untrusted `vlan35-public` VLAN to use the router's resolver regardless of what DNS server a client manually configures — prevents DNS-based ad-block/filter bypass and gives you visibility into guest DNS queries.


```
/ip service
set ftp disabled=yes
set ssh disabled=yes
set telnet disabled=yes
set api disabled=yes
set api-ssl disabled=yes
set www address=<172.31.62.130/32>
set www-ssl certificate=<your-certificate-name>
/tool mac-server
set allowed-interface-list=none
/tool mac-server mac-winbox
set allowed-interface-list=MGT
/tool mac-server ping
set enabled=no
/tool bandwidth-server
set enabled=no
/radius incoming
set accept=yes
/ipv6 nd
set [ find default=yes ] advertise-dns=yes
/ip neighbor discovery-settings
set discover-interface-list=MGT protocol=mndp
/system clock
set time-zone-name=<Region/City>
/system ntp client
set enabled=yes
/system ntp client servers
add address=0.<region>.pool.ntp.org
add address=1.<region>.pool.ntp.org
add address=2.<region>.pool.ntp.org
/system package update
set channel=long-term
```
| Service | State | Why | 
|---|---|---|
| FTP / Telnet | disabled | Cleartext protocols — no legitimate use case | 
| SSH | disabled | Not needed if Winbox/HTTPS admin access suffices; re-enable with key-only auth if you need it | 
| API / API-SSL | disabled | Disable unless you have an external tool (Zabbix, Grafana, custom scripts) that needs RouterOS API access | 
| WWW / WWW-SSL | restricted to one address, cert required | Limits web UI reachability even from the LAN | 
| MAC-Winbox server | `MGT` interface list only | Prevents MAC-based discovery/login from any VLAN except management | 
| Ping (MAC server) | disabled | No operational need | 
| Bandwidth server | disabled | Only needed for `bandwidth-test` diagnostics; leaving it on is unnecessary exposure | 

**Warning:** `/radius incoming set accept=yes` allows the router to accept incoming RADIUS disconnect/CoA requests. Only enable this if you run a RADIUS server that needs to push disconnects to this router (e.g., hotspot/PPP setups) — otherwise leave disabled.


```
/tool e-mail
set server=<smtp.your-provider.com> port=587 tls=starttls \
    from=<no-reply@example.com> \
    user=<smtp-username> \
    certificate-verification=yes
```
**CRITICAL — Secrets management:** The SMTP password/credentials for `/tool e-mail` are stored **in cleartext in the router's configuration** and will appear in any `export` output unless you pass `hide-sensitive`. Never paste raw `/export` output (or `config.rsc`) into a public repository — treat it the same as a credentials file.


- Use a dedicated, least-privilege SMTP identity (send-only, single "from" address) rather than a general-purpose account.
- If using a cloud provider's SMTP relay (AWS SES, SendGrid, etc.), scope the IAM policy/API key to
`ses:SendRawEmail` (or equivalent) only.- Rotate credentials immediately if a config export is ever accidentally committed or shared.
`certificate-verification=yes` is recommended (shown above) — the original config had this disabled (`no`), which allows MITM on the SMTP session; only disable if your relay uses a self-signed cert you've separately pinned.

1. Back up the current configuration first: `/export file=backup-before-changes`
2. Replace every `<placeholder>` above with your real values (VLAN subnets, device MACs, ProtonVPN keys/endpoints, admin IPs, SMTP credentials, timezone, NTP pool).
3. Apply in RouterOS CLI/Winbox terminal, in the order presented (interfaces → addressing → firewall → services) — firewall rules depend on address-lists and interface-lists existing first.
4. Verify VPN kill-switch behavior: disable a WireGuard peer temporarily and confirm a device in its address-list **loses connectivity** rather than falling back to plain WAN.
5. Verify brute-force lockout: attempt 3 failed logins from a non-whitelisted IP within 5 minutes and confirm the source lands in `admin-blacklist` .

- **Never publish an unredacted `/export`** — it can include stored secrets (SMTP, RADIUS shared secrets, occasionally cached credentials) depending on RouterOS version and`hide-sensitive` flag.
- **WireGuard private keys** are never shown in this README and should never be committed anywhere, including private repos, without encryption at rest.
- **Static DHCP leases + MAC-based address lists** are the backbone of this VPN-routing/ACL scheme — keep that mapping current when devices are replaced.
- Review `admin-blacklist` /`admin-stage1` /`admin-stage2` address-lists periodically (`/ip firewall address-list print where list~"admin-"` ) to catch active brute-force attempts against your admin ports.

Licensed under the GNU General Public License v3.0.

```
Copyright (C) 2026  <your-name-or-handle>
This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.
This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
GNU General Public License for more details.
```
