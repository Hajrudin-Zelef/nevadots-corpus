---
id: collect-261121-veille-ia/copilot-microsoft/mikrotik-routeros-v7-dual-dhcp-wan-recursive-failover-w-pcc-load-balancing-and-recursive-ecmp
title: "MikroTik RouterOS v7 dual DHCP WAN recursive failover w/ PCC load-balancing; and recursive ECMP"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-05-07"]
keywords: ["recursive", "failover", "dhcp", "routeros", "mikrotik", "dual", "guide", "ecmp"]
source: https://gist.github.com/marfillaster/7a136ea826815ac22f2849e099a1c6a1?permalink_comment_id=4241644
source_anchor: ""
source_lines: [1, 714]
section: "Copilot_Microsoft"
---
# MikroTik RouterOS v7 — Dual DHCP WAN Recursive Failover with PCC Load Balancing

Last updated: **2026-05-07**

This is a production-minded RouterOS v7 dual-WAN guide. It covers:

- two DHCP WAN uplinks;
- recursive gateway checks;
- per-connection-classifier, or PCC, load balancing;
- automatic failover inside each policy routing table;
- recursive main-table defaults for router-originated traffic, with optional ECMP;
- NAT masquerade across both WANs.

It is written as a guide, not just a pastebin. Review every interface name, subnet, probe IP, firewall rule, and route comment before using it on a live router.

Companion lab validation report: Vultr CHR lab setup, methodology, and results

Want the short pasteable version first? Start with TL;DR default-config paste, then come back here for the rationale and variants.


| Aspect | This design | 
| WAN type | Two DHCP uplinks (PPPoE/static covered in §9) | 
| Load balancing | Per-connection (PCC), connection-sticky | 
| Health check | Recursive route + `check-gateway=ping` (link state alone is not enough) | 
| Failover | Per policy table; backup default at `distance=2` | 
| Failback | Automatic when the probe recovers | 
| NAT | Masquerade on both WANs | 
| IPv6 | Out of scope | 
| Tested on | RouterOS 7.21.4 on CHR, Vultr lab — see companion report | 

**This design is for**: a single router with two ISPs where LAN clients should use both uplinks concurrently and survive a single-WAN failure without manual intervention.

**This design is *not***: bandwidth bonding, session-preserving failover, BGP multihoming, or a substitute for a real firewall policy.

**If you take one thing away from this guide**: a recursive default route through a probe IP that itself resolves through the DHCP-learned gateway, paired with `check-gateway=ping`, is what makes failover trigger on *upstream* outages instead of only on link drops. Everything else in this document is plumbing around that idea. See §5 for the rationale and §6.6 for the routes themselves.


These are the issues most likely to bite a reader applying this guide. Each links to the section that explains how the design handles it.

| Gotcha | What goes wrong | Where to read | 
| Cloud CHR or multi-port routers may not have `ether1` as WAN | Pasting `ether1` /`ether2` examples blindly can turn a management NIC into a "WAN" and lock you out | §1 cloud CHR warning, §2 | 
| FastTrack bypasses mangle and routing marks | Traffic marked for `to_ISP1` /`to_ISP2` exits the wrong WAN, or via`main` | §6.1 | 
| DHCP route-update script can leave placeholders disabled on first apply | First script run during the unbound state disables placeholders; bootstrap once after first apply | §6.7 | 
| Public management default route in `main` masks main-table dual-WAN | Router-originated services (DNS, NTP, package downloads, DDNS) follow the public path instead of the recursive defaults | §6.6 cloud-CHR note, §7.7 | 
| PCC is connection-sticky and changes public IP per-flow | Single-stream speed tests show one WAN; existing sessions break on failover; some banking / anti-fraud flows misbehave | §6.8, §11.1 | 
| Probe set should be disjoint from DNS resolvers | If your only probes are also your only resolvers, route-health failure and DNS-service failure become indistinguishable in triage | §6.6, §10 | 
| `check-gateway=ping` reaction window is ~30–40 s | Link-layer-style HA, not BFD; do not expect sub-second failover | §10 | 
| Local destinations must bypass PCC explicitly | Without the `local` address-list bypass, LAN-to-VLAN/VPN traffic gets PCC-marked and may fail | §6.4, §6.8, §8 | 
| `passthrough=no` on`mark-routing` is required | `passthrough=yes` lets a later mangle rule overwrite the routing mark and silently misroute | §6.8 | 
| Static or PPPoE WANs need a different bootstrap | The DHCP script does not apply; gateway must be set manually or to the PPPoE interface | §9.3, §9.4 | 


1. Assumptions
2. Read this before pasting
3. Topology
4. Packet-flow overview
5. Why recursive routing is used here
6. Core config
7. Validation checklist
8. Troubleshooting
9. Optional variants
10. Operational notes
11. Design trade-offs and when not to use this
12. References
13. Changelog


This example assumes:

| Item | Example value | Change this? | 
| RouterOS | v7.x, lab-tested on 7.21.4 | Yes, test on your target version | 
| WAN 1 interface | `ether1` | Usually yes | 
| WAN 2 interface | `ether2` | Usually yes | 
| LAN bridge | `bridge` | Usually yes | 
| LAN subnet | `192.168.88.0/24` | Usually yes | 
| Router LAN IP | `192.168.88.1/24` | Usually yes | 
| WAN type | DHCP on both WANs | Yes, see static/PPPoE notes | 
| IPv4 NAT | masquerade on both WANs | Usually yes | 
| IPv6 | not covered | Add separately | 

The config does **not** include a complete firewall policy. Keep or build a real input/forward firewall around it.

### Cloud CHR management NIC warning

Cloud-hosted RouterOS instances often have an extra public management NIC before the private WAN/LAN NICs. In the Vultr CHR lab used to validate this guide, `ether1` was the public management interface, while the actual dual-WAN test interfaces were `ether2` and `ether3`.

Do not assume `ether1` is WAN1 on a cloud VM. First map interfaces with DHCP/static probes, then substitute the real WAN and LAN interface names throughout this guide. If you must keep public SSH/WinBox management during testing, leave the public default route in the `main` table and apply the dual-WAN policy routing to LAN traffic only.

Also check the cloud provider's private-network MTU. Vultr VPC 2.0 used MTU `1450` in the validation lab, so the WAN-side CHR interfaces were set to `1450`.


## 2. Read this before pasting

Do not paste this into production blind.

Recommended process:

```
/export file=before-dual-wan-change
/system backup save name=before-dual-wan-change
```

Use **Safe Mode** in WinBox/terminal while applying the routing and firewall changes.

Rollback path if something goes wrong: load the backup from a local console with `/system backup load name=before-dual-wan-change`, or restore selected sections by `/import file-name=before-dual-wan-change.rsc`. Keep at least one console-reachable management path that does not depend on the new policy tables.

Important preflight checks:

1. Replace `ether1` ,`ether2` ,`bridge` , and`192.168.88.0/24` with your real values.
2. Confirm whether the router has a separate public management NIC that should stay out of the WAN list.
3. Disable or bypass FastTrack for traffic that will use routing marks.
4. Add all local LAN, VLAN, and VPN subnets to the `local` address list.
5. Pick probe IPs that are stable and reachable through the intended ISP.
6. Test failover by breaking upstream reachability, not only by unplugging a cable.


```
                  ┌─────────────── ISP 1, DHCP ───────────────┐
                  │                                             │
LAN clients ── bridge ── MikroTik RouterOS v7 ── ether1 / WAN1  │
                  │                           └─ ether2 / WAN2 ─┤
                  │                                             │
                  └─────────────── ISP 2, DHCP ───────────────┘
```


For LAN-originated traffic:

```
LAN client packet
    ↓
/prerouting mangle
    ↓
PCC marks the connection as ISP1_conn or ISP2_conn
    ↓
Routing mark is set to to_ISP1 or to_ISP2
    ↓
Route lookup happens in the selected FIB table
    ↓
Default route points at a recursive probe IP
    ↓
Probe IP resolves through the current DHCP gateway
    ↓
Packet exits WAN1 or WAN2
```

For router replies to traffic that arrived from a WAN, the inbound packet is connection-marked by WAN interface, and the router's `output` mangle rule sends the reply back through the same WAN. This is useful for router services and for symmetric behavior around dst-nat/port-forwarded flows.

For router-originated **new** traffic, the router uses the `main` table unless you add separate output mangle or routing rules. This includes the router's own DNS resolver, NTP, package downloads, DDNS updates, pings, and other services that are not generated by LAN clients in `prerouting`. Recursive defaults in the `main` table give that router-originated traffic the same upstream-health failover mechanics used by the policy tables. Equal route distances make those defaults ECMP; different distances make them active-backup.


## 5. Why recursive routing is used here

A normal default route that points at a DHCP gateway only checks whether the gateway itself is reachable. That may still be true when the ISP has lost upstream internet.

Recursive routing adds one extra layer:

```
0.0.0.0/0 → probe IP, for example 64.6.64.6
probe IP → actual DHCP gateway learned from ISP
```

The router pings the probe IP with `check-gateway=ping`. If the probe cannot be reached through the intended WAN, the route becomes inactive and the backup route with the next distance can take over.

The key values are:

| Setting | Purpose | 
| `/routing/table add fib name=to_ISP1` | Creates a FIB routing table for traffic marked to ISP1 | 
| `/routing/table add fib name=to_ISP2` | Creates a FIB routing table for traffic marked to ISP2 | 
| `/ip route ... dst-address=<probe>/32 gateway=<actual WAN gateway>` | Host route that forces each probe through the intended ISP | 
| `/ip route ... dst-address=0.0.0.0/0 gateway=<probe>` | Recursive default route | 
| `target-scope=11` on default route | Lets the default route resolve through the probe route with `scope=10` | 
| `check-gateway=ping` | Deactivates a route when its recursive probe fails | 

Do not reuse probe IPs blindly. Use targets that are unlikely to disappear and that your ISP does not block. Avoid using your router's only configured DNS resolvers as the only probes; DNS failure and route-health failure should be separable when possible. The specific probe IPs used by this guide are listed in §6.6, and the selection criteria are in §10.


### 6.1 Disable FastPath / FastTrack for this design

PCC and policy routing depend on mangle marks. FastTrack can bypass firewall/mangle processing and can misroute traffic that should use non-main routing tables.

`allow-fast-path=no` is a global setting and will lower maximum throughput on lower-end routers (hEX-class) because more packets stay on the slow path. Test under real traffic on small hardware. See §10 for more.

```
/ip settings set allow-fast-path=no
# If you started from a default MikroTik firewall, disable the default FastTrack rule.
# This command is safe if no such rule exists; it simply finds nothing.
/ip firewall filter disable [find where action=fasttrack-connection]
```

If you must keep FastTrack for selected hosts, add explicit `accept` exceptions before the FastTrack rule for all traffic that uses `to_ISP1` or `to_ISP2`. The simplest and safest version of this design disables FastTrack.


```
/interface list
add name=WAN comment="dual-WAN uplinks"
add name=LAN comment="trusted LAN-facing interfaces"
/interface list member
add interface=ether1 list=WAN comment="WAN1"
add interface=ether2 list=WAN comment="WAN2"
add interface=bridge list=LAN comment="LAN bridge"
```


### 6.3 Optional fresh-router LAN baseline

Skip this section if your bridge, LAN IP, DHCP server, VLANs, and firewall are already configured.

```
/interface bridge
add name=bridge comment="LAN bridge"
/interface bridge port
add bridge=bridge interface=ether3
add bridge=bridge interface=ether4
add bridge=bridge interface=ether5
/ip address
add address=192.168.88.1/24 interface=bridge network=192.168.88.0 comment="LAN gateway"
/ip pool
add name=pool-lan ranges=192.168.88.10-192.168.88.254
/ip dhcp-server
add name=dhcp-lan interface=bridge address-pool=pool-lan disabled=no
/ip dhcp-server network
add address=192.168.88.0/24 gateway=192.168.88.1 dns-server=192.168.88.1
/ip dns
set allow-remote-requests=yes servers=1.1.1.1,8.8.8.8
/ip dns static
add name=router.lan address=192.168.88.1
```

The DNS server IPs above are intentionally separate from the recursive probe IPs in §6.6. DNS resolution health and route-health probes should be separable when possible. Avoid making the router's only DNS resolvers identical to the only probe IPs used to decide whether a WAN is healthy.


Add every local subnet that should **not** be PCC load-balanced toward the internet.

```
/ip firewall address-list
add list=local address=192.168.88.0/24 comment="LAN subnet - add VLAN/VPN/local subnets here too"
```

Examples for additional internal networks:

```
/ip firewall address-list
add list=local address=10.10.10.0/24 comment="example VLAN"
add list=local address=172.16.0.0/16 comment="example VPN/internal range"
```


```
/routing table
add fib name=to_ISP1 comment="Policy table for traffic selected for ISP1"
add fib name=to_ISP2 comment="Policy table for traffic selected for ISP2"
```


### 6.6 Recursive probe routes and default routes

This section uses three host routes per WAN. They are initially disabled and then enabled by the DHCP client scripts after a lease is bound.

The route comments are intentional. The DHCP scripts update all routes with comment `WAN1_RECURSIVE` or `WAN2_RECURSIVE`. Do not reuse those exact comments for unrelated routes.

**Why does the placeholder use `gateway=ether1`?** It is a placeholder until the DHCP client binds. The script in §6.7 overwrites `gateway=` with the real next-hop IP from the lease and enables the route. On a long-running broadcast Ethernet WAN you should always have an actual next-hop IP set by the DHCP script — see §9.3 for the static-WAN case where you set the next-hop manually.


```
/ip route
# -------------------------------------------------------------------
# Main-table recursive probes for router-originated traffic.
# These are updated by the DHCP scripts.
# Probes deliberately avoid Cloudflare and Google DNS so those can be
# used as normal DNS resolvers without also being route-health targets.
# -------------------------------------------------------------------
add disabled=yes dst-address=64.6.64.6/32 gateway=ether1 scope=10 target-scope=10 comment="WAN1_RECURSIVE"
add disabled=yes dst-address=149.112.112.112/32 gateway=ether2 scope=10 target-scope=10 comment="WAN2_RECURSIVE"
# Main-table recursive defaults for router-originated new traffic.
# Equal distances use ECMP. If you prefer WAN1 primary / WAN2 backup
# for main-table traffic, set MAIN_RECURSIVE_WAN2 distance=2.
add check-gateway=ping distance=1 dst-address=0.0.0.0/0 gateway=64.6.64.6 scope=10 target-scope=11 comment="MAIN_RECURSIVE_WAN1"
add check-gateway=ping distance=1 dst-address=0.0.0.0/0 gateway=149.112.112.112 scope=10 target-scope=11 comment="MAIN_RECURSIVE_WAN2"
# -------------------------------------------------------------------
# Recursive host routes for the policy routing tables.
# WAN1_RECURSIVE routes are forced through WAN1's current DHCP gateway.
# WAN2_RECURSIVE routes are forced through WAN2's current DHCP gateway.
#
# Each table's primary and backup intentionally use different DNS providers
# so a single provider outage does not deactivate both routes at once.
# -------------------------------------------------------------------
add disabled=yes dst-address=9.9.9.9/32 gateway=ether1 scope=10 target-scope=10 comment="WAN1_RECURSIVE"
add disabled=yes dst-address=208.67.222.222/32 gateway=ether1 scope=10 target-scope=10 comment="WAN1_RECURSIVE"
add disabled=yes dst-address=64.6.65.6/32 gateway=ether2 scope=10 target-scope=10 comment="WAN2_RECURSIVE"
add disabled=yes dst-address=208.67.220.220/32 gateway=ether2 scope=10 target-scope=10 comment="WAN2_RECURSIVE"
# -------------------------------------------------------------------
# Policy-table defaults.
# Each table prefers its own ISP, then falls back to the other ISP.
# Primary and backup deliberately use different probe providers.
# -------------------------------------------------------------------
add check-gateway=ping distance=1 dst-address=0.0.0.0/0 gateway=9.9.9.9 routing-table=to_ISP1 scope=10 target-scope=11 comment="to_ISP1_primary_via_WAN1"
add check-gateway=ping distance=2 dst-address=0.0.0.0/0 gateway=208.67.220.220 routing-table=to_ISP1 scope=10 target-scope=11 comment="to_ISP1_backup_via_WAN2"
add check-gateway=ping distance=1 dst-address=0.0.0.0/0 gateway=64.6.65.6 routing-table=to_ISP2 scope=10 target-scope=11 comment="to_ISP2_primary_via_WAN2"
add check-gateway=ping distance=2 dst-address=0.0.0.0/0 gateway=208.67.222.222 routing-table=to_ISP2 scope=10 target-scope=11 comment="to_ISP2_backup_via_WAN1"
```

To make the main table active-backup instead of ECMP:

```
/ip route set [find where comment="MAIN_RECURSIVE_WAN1"] distance=1
/ip route set [find where comment="MAIN_RECURSIVE_WAN2"] distance=2
```

If the router has a public management NIC and you need to keep SSH/WinBox reachable through that public interface, consider omitting the `MAIN_RECURSIVE_WAN1` and `MAIN_RECURSIVE_WAN2` defaults during the lab. The policy tables still exercise PCC and failover for LAN traffic, while the `main` table keeps the cloud provider's management default route.

The main-table recursive defaults exist for router-originated traffic — the router's own DNS resolver, NTP, package downloads, DDNS updates, pings — which uses `main` because it does not enter the LAN `prerouting` chain. Equal distances make these defaults ECMP; different distances make them active-backup. Static defaults, DHCP-added defaults, or deliberate output policy routing can fill the same role if ECMP is not the right shape for your design.


### 6.7 DHCP clients with route-update scripts

Add the DHCP clients after creating the route placeholders above.

```
/ip dhcp-client
add interface=ether1 add-default-route=no use-peer-dns=no use-peer-ntp=no comment="WAN1 DHCP - updates WAN1_RECURSIVE routes"
add interface=ether2 add-default-route=no use-peer-dns=no use-peer-ntp=no comment="WAN2 DHCP - updates WAN2_RECURSIVE routes"
```

Set the WAN1 DHCP script:

```
/ip dhcp-client
set [find where interface=ether1] script={
    :if ([:tobool $bound]) do={
        :local gw $"gateway-address";
        /ip/route/set [find where comment="WAN1_RECURSIVE"] gateway=$gw disabled=no;
        :log info ("WAN1 DHCP bound; recursive routes now use " . $gw);
    } else={
        /ip/route/set [find where comment="WAN1_RECURSIVE"] disabled=yes;
        :log warning "WAN1 DHCP unbound; WAN1 recursive routes disabled";
    }
    /ip/firewall/connection/remove [find where connection-mark="ISP1_conn"];
}
```

Set the WAN2 DHCP script:

```
/ip dhcp-client
set [find where interface=ether2] script={
    :if ([:tobool $bound]) do={
        :local gw $"gateway-address";
        /ip/route/set [find where comment="WAN2_RECURSIVE"] gateway=$gw disabled=no;
        :log info ("WAN2 DHCP bound; recursive routes now use " . $gw);
    } else={
        /ip/route/set [find where comment="WAN2_RECURSIVE"] disabled=yes;
        :log warning "WAN2 DHCP unbound; WAN2 recursive routes disabled";
    }
    /ip/firewall/connection/remove [find where connection-mark="ISP2_conn"];
}
```

If your terminal rejects the multiline script form, paste the script body into the DHCP client script field in WinBox/WebFig, or convert it to a quoted one-liner with escaped quotes.

After applying the scripts, renew the leases:

```
/ip dhcp-client renew [find where interface=ether1]
/ip dhcp-client renew [find where interface=ether2]
```

The DHCP route-update script disables placeholder routes during any unbound transition, including the brief unbound state that occurs during initial paste/import. Its first run can therefore leave the placeholders disabled even when the leases bind moments later. Bootstrap once, unconditionally, after first apply:

```
# Replace these gateways with the values shown by /ip dhcp-client print detail.
/ip route set [find where comment="WAN1_RECURSIVE"] gateway=<WAN1_DHCP_GATEWAY> disabled=no
/ip route set [find where comment="WAN2_RECURSIVE"] gateway=<WAN2_DHCP_GATEWAY> disabled=no
```

Verify:

```
/ip dhcp-client print detail
/ip route print detail where comment~"WAN1_RECURSIVE|WAN2_RECURSIVE"
```

Future DHCP bind/unbind events update the routes via the script; this manual bootstrap is needed only once.

Each script clears only its own ISP's connection marks. A WAN1 lease change (new public IP, recovery after failure, etc.) leaves stale `ISP1_conn` flows pinned to the old path or old NAT public IP, so they are flushed and re-classified. Healthy `ISP2_conn` flows on the still-up WAN2 are not disturbed.


### 6.8 Mangle rules for inbound symmetry, PCC, and routing marks

Order matters. Keep the local-traffic accept rule before the PCC rules. `add` appends, so paste this section into a router with no existing prerouting mangle rules — or reorder afterward with `move`.

The chain=output rules below intentionally do **not** filter on `connection-state=new`. They must mark every reply packet whose connection-mark matches; replies on already-established flows are not in state `new`, and adding that condition will break per-WAN reply symmetry.

```
/ip firewall mangle
# Do not PCC traffic whose destination is another local subnet.
add chain=prerouting action=accept in-interface-list=LAN dst-address-list=local comment="skip local destinations before PCC"
# Mark new inbound connections by the WAN where they arrived.
# This helps replies from router services and dst-nat flows return through the same ISP.
add chain=prerouting action=mark-connection connection-state=new connection-mark=no-mark in-interface=ether1 new-connection-mark=ISP1_conn passthrough=yes comment="new inbound via WAN1"
add chain=prerouting action=mark-connection connection-state=new connection-mark=no-mark in-interface=ether2 new-connection-mark=ISP2_conn passthrough=yes comment="new inbound via WAN2"
# PCC for new LAN-originated internet connections.
add chain=prerouting action=mark-connection connection-state=new connection-mark=no-mark in-interface-list=LAN dst-address-list=!local dst-address-type=!local new-connection-mark=ISP1_conn passthrough=yes per-connection-classifier=both-addresses-and-ports:2/0 comment="PCC bucket 0 -> ISP1"
add chain=prerouting action=mark-connection connection-state=new connection-mark=no-mark in-interface-list=LAN dst-address-list=!local dst-address-type=!local new-connection-mark=ISP2_conn passthrough=yes per-connection-classifier=both-addresses-and-ports:2/1 comment="PCC bucket 1 -> ISP2"
# Apply routing marks to LAN packets based on connection mark.
add chain=prerouting action=mark-routing connection-mark=ISP1_conn in-interface-list=LAN new-routing-mark=to_ISP1 passthrough=no comment="route ISP1_conn via to_ISP1"
add chain=prerouting action=mark-routing connection-mark=ISP2_conn in-interface-list=LAN new-routing-mark=to_ISP2 passthrough=no comment="route ISP2_conn via to_ISP2"
# Router-generated replies for marked inbound connections.
# Do NOT add connection-state=new here; replies on established flows must also be marked.
add chain=output action=mark-routing connection-mark=ISP1_conn dst-address-list=!local new-routing-mark=to_ISP1 passthrough=no comment="router output replies via ISP1"
add chain=output action=mark-routing connection-mark=ISP2_conn dst-address-list=!local new-routing-mark=to_ISP2 passthrough=no comment="router output replies via ISP2"
```

The `mark-routing` rules use `passthrough=no` on purpose. Once a packet has its routing mark, no later mangle rule should change it; `passthrough=yes` here would let a subsequent rule overwrite the mark and silently misroute traffic. The earlier `mark-connection` rules use `passthrough=yes` because the next stage (`mark-routing`) needs to read the connection mark.

Notes:

- PCC is per connection, not per packet.
- A single TCP session uses one WAN; it is not split across both links.
- Existing sessions usually break during failover. New sessions should use the active route.
- Some banking, gaming, streaming, and CDN flows may dislike changing public IPs across different connections.


```
/ip firewall nat
add chain=srcnat action=masquerade out-interface-list=WAN ipsec-policy=out,none comment="masquerade both WANs"
```

If you use IPsec, WireGuard, GRE, EOIP, VLANs, or other tunnels, review NAT bypass rules before this masquerade rule.


```
/ip dhcp-client print detail
```

Confirm each WAN has:

- `status=bound` ;
- an address;
- a gateway value, shown as `gateway=` in print output and exposed to scripts as`$"gateway-address"` ;
- `add-default-route=no` .

### 7.2 Check recursive route state

```
/ip route print detail where comment~"WAN|MAIN|to_ISP"
```

Expected behavior:

- `WAN1_RECURSIVE` routes should use WAN1's current DHCP gateway.
- `WAN2_RECURSIVE` routes should use WAN2's current DHCP gateway.
- `to_ISP1_primary_via_WAN1` should be active when WAN1's probe is reachable.
- `to_ISP2_primary_via_WAN2` should be active when WAN2's probe is reachable.
- backup routes should become active when their primary route fails.

### 7.3 Test routing-table-specific pings

```
/tool ping address=9.9.9.9 routing-table=to_ISP1 count=5
/tool ping address=64.6.65.6 routing-table=to_ISP2 count=5
```

`routing-table=` on `/tool ping` requires RouterOS 7.1 or later, but syntax and CLI context can vary between versions and shells. If your build rejects that form, validate with route state plus source-address probes:

```
/ip route print detail where comment~"WAN|to_ISP"
# Replace the source addresses with the DHCP addresses on your WAN interfaces.
/ping address=9.9.9.9 src-address=<WAN1_DHCP_ADDRESS> count=5
/ping address=64.6.65.6 src-address=<WAN2_DHCP_ADDRESS> count=5
```

For PCC itself, prefer real client traffic from the LAN. Router-originated ping tests do not traverse the LAN `prerouting` PCC rules.

### 7.4 Check mangle hit counters

```
/ip firewall mangle print stats all
```

Generate client traffic, then confirm:

- PCC bucket counters increase;
- routing-mark rule counters increase;
- local-destination accept rule increases when clients talk to local subnets.

Example client-side flow generator:

for i in $(seq 1 30); do
  curl -4 -s --max-time 10 https://ifconfig.me
  echo
done | sort | uniq -c

If the client also has a separate public management NIC, make sure the test traffic is sourced through the LAN side of the MikroTik path, for example with `curl --interface <LAN_IP>`.

### 7.5 Check connection marks

```
/ip firewall connection print where connection-mark~"ISP"
```

You should see client flows marked as either `ISP1_conn` or `ISP2_conn`.

Test WAN1 failure:

```
/ip route print detail where routing-table=to_ISP1
```

Then break WAN1 upstream connectivity. Do not rely only on unplugging the Ethernet cable; a real ISP failure may leave the local link up while upstream internet is dead.

Expected result:

- WAN1 recursive probe becomes unreachable.
- `to_ISP1_primary_via_WAN1` becomes inactive.
- `to_ISP1_backup_via_WAN2` becomes the active route.
- New connections marked `ISP1_conn` use WAN2 until WAN1 recovers.

Repeat the test for WAN2.

### 7.7 Validate router-originated DNS if you depend on it

If LAN clients use the MikroTik as their DNS server, also test the router's own upstream DNS path. Client DNS queries enter the router locally, but the router's upstream resolver queries are generated by the router itself and normally use the `main` table.

Use resolver IPs that are not the same as your recursive probe IPs, flush the cache, resolve a new name, and check the DNS connections:

```
/ip dns set allow-remote-requests=yes servers=1.1.1.1,8.8.8.8
/ip dns cache flush
/ip firewall connection remove [find where dst-port=53]
:put [/resolve example.net]
:put [/resolve openai.com]
/ip firewall connection print detail where dst-port=53
/ip route print detail where comment~"MAIN_RECURSIVE"
```

Expected result:

- DNS resolution succeeds.
- DNS connections use the expected WAN source address or expected main-table path.
- If one main-table recursive default is failed, new resolver lookups use the surviving default.

Router-originated traffic uses `main`. If `main` contains a non-recursive default route — common on cloud VMs with a public management NIC, or on routers where DHCP-added WAN defaults still exist — that route can mask the recursive defaults during this test. To validate the real recursive path, manage the router through an out-of-band path (console, LAN-jump, noVNC) and temporarily disable the masking default before repeating the test.


| Symptom | Likely cause | What to check | 
| Recursive routes show unreachable | DHCP script did not update gateway, route comments do not match, or probe is blocked | `/ip route print detail where comment~"WAN"` ,`/log print` , DHCP client status | 
| Traffic marked for ISP1 exits ISP2 unexpectedly | FastTrack still active or main table is being used | Disable FastTrack; check mangle counters and route marks | 
| Router services reply through the wrong WAN | Inbound connection was not marked, or output mark rule missing | Check inbound mangle rules and `chain=output` mark-routing rules | 
| LAN clients cannot reach local VLANs/subnets | Local destinations are being PCC-marked | Add every internal subnet to `address-list=local` ; keep local accept rule before PCC | 
| Failover works, but existing downloads or SSH sessions die | Expected behavior; PCC is connection-based and NAT public IP changed | Start new sessions; optionally clear affected connection marks | 
| Failback does not happen after WAN recovery | Probe route still inactive, probe target does not answer, or DHCP route still disabled | Ping probe via intended WAN; check DHCP script logs and route state | 
| Port forwards work on only one WAN | Inbound connection marking or dst-nat rules are incomplete | Mark new inbound connections by WAN and ensure dst-nat rules exist for both WANs | 
| Traceroute looks odd | PCC, ECMP, ICMP behavior, and router-originated traffic can differ from TCP flows | Test with routing-table-specific pings/traceroutes and with real TCP traffic | 
| BGP/OSPF/tunnels behave oddly | Mangle rules can override route decisions for traffic that should use another policy | Add explicit accept/bypass rules before PCC for routing protocols, tunnels, or management subnets | 
| Load balancing seems uneven | PCC balances connection buckets, not bandwidth | Use weighted PCC buckets for asymmetric links; test with many flows | 

Manual cleanup commands:

```
/ip firewall connection remove [find where connection-mark="ISP1_conn"]
/ip firewall connection remove [find where connection-mark="ISP2_conn"]
```


### 9.1 Load-balance only selected clients

Use this when most clients should use WAN1 primary / WAN2 backup, while selected clients are PCC load-balanced.

First, make the main-table recursive defaults active-backup instead of ECMP:

```
/ip route set [find where comment="MAIN_RECURSIVE_WAN1"] distance=1
/ip route set [find where comment="MAIN_RECURSIVE_WAN2"] distance=2
```

Create an address list for clients allowed to use PCC:

```
/ip firewall address-list
add list=MultiWAN-Clients address=192.168.88.50 comment="example load-balanced client"
add list=MultiWAN-Clients address=192.168.88.60 comment="example load-balanced client"
```

Then add `src-address-list=MultiWAN-Clients` to the two PCC mark-connection rules:

```
/ip firewall mangle
set [find where comment="PCC bucket 0 -> ISP1"] src-address-list=MultiWAN-Clients
set [find where comment="PCC bucket 1 -> ISP2"] src-address-list=MultiWAN-Clients
```

Non-listed clients will not receive PCC routing marks and will use the main table.


### 9.2 Weighted PCC for asymmetric WAN speeds

PCC does not aggregate bandwidth, but you can bias new connection distribution.

Example: WAN1 is 250 Mbps, WAN2 is 500 Mbps. Use a 1:2 connection-bucket ratio.

Replace the two PCC rules with three buckets:

```
/ip firewall mangle
remove [find where comment="PCC bucket 0 -> ISP1"]
remove [find where comment="PCC bucket 1 -> ISP2"]
add chain=prerouting action=mark-connection connection-state=new connection-mark=no-mark in-interface-list=LAN dst-address-list=!local dst-address-type=!local new-connection-mark=ISP1_conn passthrough=yes per-connection-classifier=both-addresses-and-ports:3/0 comment="PCC weighted bucket 0 -> ISP1"
add chain=prerouting action=mark-connection connection-state=new connection-mark=no-mark in-interface-list=LAN dst-address-list=!local dst-address-type=!local new-connection-mark=ISP2_conn passthrough=yes per-connection-classifier=both-addresses-and-ports:3/1 comment="PCC weighted bucket 1 -> ISP2"
add chain=prerouting action=mark-connection connection-state=new connection-mark=no-mark in-interface-list=LAN dst-address-list=!local dst-address-type=!local new-connection-mark=ISP2_conn passthrough=yes per-connection-classifier=both-addresses-and-ports:3/2 comment="PCC weighted bucket 2 -> ISP2"
```

For different ratios, increase the denominator and allocate more buckets to the faster WAN.


### 9.3 Static WAN instead of DHCP

For a static Ethernet WAN, do not use the DHCP script for that interface. Set the recursive host routes manually to the ISP next-hop IP.

Example WAN1 static gateway `203.0.113.1`:

```
/ip route set [find where comment="WAN1_RECURSIVE"] gateway=203.0.113.1 disabled=no
```

Do not use `gateway=ether1` on a normal broadcast Ethernet WAN as a long-term substitute for a real next-hop IP. Use the actual gateway IP from the ISP.


### 9.4 PPPoE WAN instead of DHCP

For PPPoE, use the PPPoE interface as the gateway for recursive probe host routes because PPPoE is point-to-point.

Example WAN1 PPPoE interface `pppoe-out1`:

```
/interface list member add interface=pppoe-out1 list=WAN comment="WAN1 PPPoE"
/ip route set [find where comment="WAN1_RECURSIVE"] gateway=pppoe-out1 disabled=no
```

Then remove (not just disable) the WAN1 DHCP client, including its script. The script in §6.7 references `$"gateway-address"` from the DHCP lease; it has no meaning for PPPoE and must not run alongside this manual `set`. If you keep the DHCP client object disabled, also clear its script field to avoid confusion later.


PCC is not bandwidth bonding. It does not split one TCP flow across both WANs. It assigns each new connection to a bucket and keeps that connection sticky to the selected path as long as the connection is tracked.

Practical effects:

- speed tests with one connection may show only one WAN;
- multiple clients or multi-connection tests show balancing better;
- long-lived sessions may break when their selected WAN fails;
- sites that bind login/session state to public IP may behave poorly when different connections leave through different ISPs.

Use probe IPs that:

- are stable;
- answer ICMP consistently;
- are outside your ISP network if you want to detect wider internet reachability;
- are not all in the same provider or DNS service;
- are not the same IPs you plan to use as the router's DNS resolvers;
- are not blocked by the ISP.

If a provider blocks ICMP, choose another target or monitor by another method.

The example uses well-known anycast public DNS IPs from Quad9, OpenDNS, and Verisign/UltraDNS because they are globally reachable and tend to answer ICMP. It deliberately avoids Cloudflare and Google as route probes so those popular resolvers can be used for actual DNS service without also becoming route-health sensors.

The main-table probes (`64.6.64.6` / `149.112.112.112`) and the policy-table probes use different IPs on purpose. Reachability of one set does not guarantee reachability of the other, so it is normal during partial outages to see the router itself reach the internet via the main table while LAN clients on a policy table appear stuck — or the other way around. Compare both sets of probes when triaging.

Keep DNS resolvers and route probes separate where practical. If the router's only DNS servers are also the only recursive probe targets, DNS-service failure and route-health failure become harder to distinguish during troubleshooting.

### FastTrack and performance

Disabling FastTrack/FastPath can reduce maximum throughput on low-end routers because more packets stay on the slow path. On devices such as hEX-class routers, test CPU under real traffic. On more capable routers such as RB5009-class hardware, this design is usually much more comfortable, but throughput still depends on firewall complexity, queueing, packet size, and enabled services.

This guide only covers routing, mangle, and NAT. It does not replace a firewall policy. Keep normal protections such as:

- input chain drop for unsolicited WAN traffic;
- established/related accept rules;
- invalid drop rules where appropriate;
- explicit management access restrictions;
- dst-nat rules only for services you intend to expose.

Be cautious with generic invalid drops if your network has asymmetric routing, tunnels, or advanced policy routing.


## 11. Design trade-offs and when not to use this

### 11.1 What you give up by choosing PCC

PCC is connection-level, not packet-level. That means:

- A single TCP flow uses one WAN. Single-stream speed tests measure one link, not the sum of both.
- Sessions that bind to a public IP — banking, anti-fraud / Cloudflare Turnstile, some video conferencing, IMAP IDLE on certain providers — can misbehave when consecutive connections from the same client leave through different ISPs. If most users hit a small set of such services, route those destinations through one WAN via address-list + routing mark, alongside or instead of PCC.
- Failover is not session-preserving. Live SSH, VoIP, and streaming sessions on the failed WAN drop. New connections recover automatically.

### 11.2 When a simpler design is enough

If you only need **failover** (no load balancing), drop the PCC mangle rules and keep the recursive default routes in `main` at distances 1 and 2. One WAN is active at a time, and `check-gateway=ping` triggers failover the same way.

If you only need **per-client steering** (e.g. send the guest VLAN out WAN2), skip PCC and use `src-address-list` → `mark-routing` directly. Static assignment is easier to debug than connection-classifier hashing.

### 11.3 When PCC is the wrong tool

- Two ISPs with very different latency or jitter where you want application-aware steering. Use a real SD-WAN appliance.
- Public-facing services that must answer on a stable IP. PCC + masquerade gives a different public IP per WAN; you want BGP multihoming or an upstream front-door (DNS failover, anycast, cloud LB).
- True bandwidth aggregation. Look at MLPPP, L2TP+BCP, or commercial bonding services. None are RouterOS-native at the level you might expect.

### 11.4 Why recursive routes instead of `netwatch` scripts

Some guides drive failover with `/tool netwatch` scripts that enable and disable default routes when a probe goes down. That works, but it is an out-of-band controller racing with the routing table. Recursive routing keeps the health check inside the FIB: a route is active if and only if its probe is reachable through the intended next-hop. There is one source of truth, no script timing to tune, and `check-gateway=ping` is documented behavior rather than a custom script you have to maintain across upgrades.



- **2026-05-07** — Initial public version. Includes the TL;DR default-config paste, route probes deliberately disjoint from DNS resolvers, and validation against RouterOS 7.21.4 in a Vultr CHR lab. PCC split, per-policy-table failover and failback, recursive`main` -table ECMP for router-originated traffic, and the TL;DR paste applied to a default-like RouterBOARD baseline are all confirmed. See companion lab report.

@bwraith I removed the ICMP accept rule and updated the output to check for destination. It seems to work.
