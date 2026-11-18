Site-to-Site WireGuard Between Two UniFi Gateways: I Measured Where It Breaks
Two UCG Ultras on the same bench. Every claim below is a measurement, not a reading. Where I got something wrong, I say so and show what corrected it.
If you have two UniFi gateways in two locations and you want them to talk to each other, the internet gives you three answers:
- “Just use Site Magic.” It works. Nobody tells you the price.
- “You can do it with plain WireGuard.” You can get close. Where it stops is undocumented.
- “It’s impossible.” Nearly right, for the wrong reason.
I put two UCG Ultras on a bench, ran both paths end to end, and measured all three. Here is what actually happens.
The setup
- 2 × UniFi Cloud Gateway Ultra, UniFi Network 10.5.67
- Site A: 8 VLANs, zone-based firewall, dual WAN (PPPoE + DHCP-with-MAC-clone), both public, both on dynamic DNS
- Site B: same design, different second octet — deliberately non-overlapping subnets — single WAN, behind carrier-grade NAT
- A test host that exists only at Site B, so nothing could answer from the wrong side
That last point matters more than it sounds. Half the bad advice online comes from tests that accidentally succeeded locally.
INTERNET
                 |
   +-------------+--------------+
   |                            |
ISP 1 (PPPoE)            ISP 2 (DHCP + MAC clone)      ISP 3 (CGNAT)
public IP, dynamic       public IP, dynamic            NO public IP
ddns: vpn.<domain>       ddns: vpn2.<domain>           no inbound, ever
   |                            |                            |
   +------------+---------------+                            |
                |                                            |
        +-------+--------+                            +------+-------+
        |   SITE A       |                            |   SITE B     |
        |   UCG Ultra    | <-- the tunnel we want --> |   UCG Ultra  |
        |   dual WAN     |                            |  single WAN  |
        +-------+--------+                            +------+-------+
                |                                            |
     8 VLANs, 10.10.<vlan>.0/24                  8 VLANs, 10.20.<vlan>.0/24
     zone-based firewall                         same zones, same design
Why this exact shape is the whole problem
Read that diagram again with the WireGuard limitation in mind, because the two facts interlock and that’s the part nobody spells out:
- Site B is behind CGNAT, so it can never accept an inbound connection. It is forced to be the WireGuard client — the side that dials.
- UniFi NATs the WireGuard client interface. So the side that dials is the side that can’t be reached back.
The topology you’re forced into is exactly the topology that doesn’t work. There is no re-arrangement that helps, because the roles aren’t a choice: whichever end lacks a public IP must dial, and whichever end dials gets NATed. Site A having two public IPs doesn’t rescue it either — the constraint is at the far end, and the far end has none.
That’s why “just make the other one the server” — the most common suggestion in every thread — is not advice. It’s the configuration you already had.
Part 1 — Manual WireGuard, and exactly where it dies
The obvious build: WireGuard Server on one gateway, VPN Client on the other. The tunnel comes up. Handshake completes. The peer shows online. Then:
direction                     result
----------------------------  -----------------------------------
ICMP, client -> server        [ok] passes
ICMP, server -> client's LAN  [ok] passes (after the fixes below)
TCP, server -> client's LAN   [NO] connects, delivers nothing
The symptom is genuinely cruel. nc reports the port open. The TCP handshake completes. And curl times out with 0 bytes.
It looks exactly like an MTU problem. It is not an MTU problem.
The real cause, and other people who hit it
From the Ubiquiti feature request thread — 196 replies, three years, no official response — user rbrunka nails it:
UCG behind Starlink CGNAT as WireGuard client to a self-hosted VPS hub. The tunnel establishes fine and outbound policy-based routing works perfectly, but inbound traffic initiated by the hub to the UCG’s LAN is silently dropped — root cause is NAT applied by UniFi on the WG client interface.
And quasides on the Lawrence Systems forum, more bluntly: the WireGuard client is implemented behind NAT, so site-to-site is impossible; the server side can receive, but can’t initiate.
The client can reach the server. The server cannot reach back. That is the whole story, and it is not written anywhere in the product.
The workaround that gets further than any published attempt
UniFi does have a NAT rule type with an Exclude option, which turns off masquerading on a chosen interface. Finding it is its own small adventure:
Settings -> search NAT -> Policy Table -> NAT (/settings/policy-table?preset=nat-rules). /settings/nat does not exist. The Exclude checkbox sits below the fold of the form, so it’s easy to miss entirely.
For anyone automating, the API shape is:
{
  "type": "MASQUERADE",
  "exclude": true,
  "out_interface": "<network _id>",
  "source_filter":      {"filter_type": "NONE", "firewall_group_ids": [],
                         "invert_address": false, "invert_port": false},
  "destination_filter": {"filter_type": "NONE", "firewall_group_ids": [],
                         "invert_address": false, "invert_port": false},
  "protocol": "all", "ip_version": "IPV4", "rule_index": 0,
  "setting_preference": "manual", "pppoe_use_base_interface": false,
  "enabled": true, "logging": false
}
I found that shape by creating one rule in the UI and reading it back, because it isn’t documented either.
And it genuinely helps. Before the rule, the gateway answered !H (destination host unreachable) and nothing left the box. After it, ICMP crosses and TCP completes its handshake. That is further than any write-up I could find.
Get bruno roquette’s stories in your inbox
Join Medium for free to get updates from this writer.
It still doesn’t carry data. Source NAT was an obstacle, not the obstacle.
The test that settles it
Clean target — a machine that exists only at Site B, running python3 -m http.server:
from the local LAN (no tunnel)     200 · 1422 bytes ·  6 ms
across the tunnel, from Site A     000 ·    0 bytes ·  timeout, 3 of 3
⚠️ A methodology warning that cost me an hour. My first attempt aimed at macOS AirPlay Receiver on port 7000. It refuses connections whose source is outside the local subnet — so it produced a perfect false negative that looked like a network failure. Your test target must accept any source address.
Hypotheses I killed, and how
hypothesis                      how it died
------------------------------  ----------------------------------------------------------------------------
MTU                             ping succeeds up to 1408 B payload; the HTTP response was 1.4 kB
MSS clamping                    tried MTU 1380 and mss_clamp custom 1300 - no change
firewall at destination         the allow rule had 10,471 hits, and UniFi auto-created the (Return) policies
wrong key / AllowedIPs          compared byte for byte
service-side source filter      re-ran against http.server, which filters nothing
asymmetric routing on the host  test host had Wi-Fi off, wired only
Part 2 — Seven API traps, if you’re going to automate this anyway
- Static routes must be interface-route , pointing at the VPN network’s_id . As anexthop-route , traffic never leaves.static-route_interface will happily accept"wan" (wrong) and reject the interface name withIdInvalid .
- The VPN Client requires an Endpoint — without one you getapi.err.WireguardInvalidConfiguration . It dials; it never listens.
- ListenPort in the client’s.conf is accepted and persisted, but the port is never opened. Two clients dialing each other will never handshake. This one is especially expensive because nothing errors.
- A VPN client lands in the External zone and the API refuses to move it (NetworksCantBeReassignedToDifferentZone ). That’s not a bug — the tunnel is modeled as a virtual WAN.
- Don’t “fix” that by opening External -> LAN . That zone contains your actual WAN interfaces. Qualify by source with an address group instead.
- create_allow_respond is rejected onExternal -> Gateway (FirewallPolicyCreateRespondTrafficPolicyNotAllowed ).
- Creating a WireGuard peer isn’t automatable. I swept 21 REST collections and no peer’s public key appears in any of them. v2/.../vpn/users shows them, but it’s a status endpoint — read-only in practice.
And one reading trap that will silently waste your afternoon: a zone PUT signals success by returning the object (with _id), not by returning meta.rc. Code that checks meta sees “fine” on a call that changed nothing.
Part 3 — Site Magic / SD-WAN: it works, and here’s the bill
Four fields on one cloud page, and the exact test that failed above passes:
-> remote gateway   HTTPS   200 in 34 ms
-> remote host :8899        200 · 1422 bytes · 31 ms
That’s the honest headline: the official path works and it is fast. The cost is in four places nobody warns you about.
1. The UCG Ultra cannot be a Hub. The screen says “No sites are available to be set as Hubs” and offers no explanation. The documentation lists who can: EFG, UDM Pro Max, UDM SE, UDM Pro, UCG Fiber, UCG Industrial, UDW. So you use Mesh, which with two sites behaves identically — except it does not expose the VPN Tunnels field, which is what you’d use to pin a tunnel to a specific WAN on a dual-WAN site.
2. “Enable Subnet Overlap with SNAT” ships ON. It rewrites the remote site into 172.16.x. If your subnets don’t overlap — mine deliberately don’t — this throws away your entire addressing scheme for nothing. Uncheck it.
3. The cloud lags behind a WAN change. Until it catches up, the site shows greyed out and Mesh refuses to include it, without saying why.
4. It depends on Ubiquiti’s cloud. That’s the actual trade: the work disappears, the dependency appears. Whether that’s acceptable is a judgment call, not a technical one — but you should make it knowingly.
Where the firewall applies: both ends
The zone-based firewall documentation says the destination gateway evaluates tunnel traffic. That’s true, and it’s half the picture. The source gateway filters too.
⚠️ I concluded the opposite and I was wrong. I fired 25 pings, watched the destination counters stay at zero, and wrote down “the filter only happens at the source.” Then a real VNC session ran across the tunnel and the same counters moved immediately:
<zone-policy>-to-trusted     91,340 hits
<zone-policy>-to-gateway      4,159 hits
A flat counter under light traffic proves nothing about whether a rule is live. I’d been treating absence of evidence as evidence, on a sample far too small to say anything.
The clean way to isolate the source side — same machine, same cable, only the VLAN changes:
source VLAN        reaches other site?  internet?
-----------------  -------------------  -------------------------------
the permitted one  [ok] everything      [ok]
any other          [NO] nothing         [ok] (the experiment's control)
That control column matters. Without it, “nothing works” is indistinguishable from “I unplugged something.”
Part 4 — WAN failover: 78 seconds, unattended
I swapped the two ISPs between the sites — meaning both public IPs changed at the same moment, which is harsher than any real-world outage:
tunnel down     ~78 s, recovered on its own
back up with    200 in 33 ms, 0% loss
DDNS            followed
Dynamic WAN doesn’t break SD-WAN. The two ends re-find each other through the cloud, not by address — which is precisely the dependency you’re paying for. Note what this means both ways: you get resilience you didn’t configure, and you get it because a third party is brokering the rendezvous.
⚠️ Two things will stall the swap if you don’t plan for them: PPPoE must be reconfigured on the receiving gateway, and an ISP that requires MAC cloning will not hand out an address without it. Both cost me a bench outage before I learned to stage them first.
One last trap that isn’t a network problem
Once SD-WAN links your sites, a phone’s WireGuard profile actively hurts when you’re physically at one of them. Its AllowedIPs covers both networks, so local traffic hairpins out to the far site and back. The symptom is that the phone reaches nothing while everything else on the same Wi-Fi is fine. Turning the VPN off fixes it instantly, and it looks like a catastrophic failure for about five minutes first.
Verdict
                             manual WireGuard                             Site Magic / SD-WAN
---------------------------  -------------------------------------------  ---------------------
gateway<->gateway both ways  [NO] no - client NAT, unfixable from the UI  [ok] yes
independent of vendor cloud  [ok]                                         [NO] no
survives both WANs changing  -                                            [ok] 78 s, unattended
honest effort                days                                         minutes
If you need two UniFi gateways to route to each other in both directions, Site Magic is currently the only path, and the price is a cloud dependency. The manual route isn’t hard — it’s closed, at the NAT on the VPN client interface, and no amount of firewall or route work opens it.
That the workaround exists as an undocumented checkbox below the fold of a page you can only reach by search, and still isn’t sufficient, is the part I’d like Ubiquiti to fix. Three years and 196 replies on the feature request suggest I’m not the first to ask.
References
- Feature Request: UniFi WireGuard Site-to-Site — 196 replies over three years, no official response. Contains the SSH workaround (wg showconf /wg setconf ) and the warning that any UI change wipes it
- Add Feature: WireGuard AllowedIPs in the GUI for Client — the missing field, requested separately
- Unifi Site to Site WireGuard VPN (without Site Magic) — Lawrence Systems forum; ends without anyone demonstrating working bidirectional traffic
- Zone-Based Firewalls in UniFi — official
- Setting Up SD-WAN with UniFi Site Manager and Fabrics — official; this is where the Hub-capable hardware list lives
Discussion thread on r/Ubiquiti: https://www.reddit.com/r/Ubiquiti/s/jmYfUAqf04