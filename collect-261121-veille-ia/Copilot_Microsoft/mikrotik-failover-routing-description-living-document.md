---
id: collect-261121-veille-ia/mikrotik/mikrotik-failover-routing-description-living-document
title: "MikroTik failover routing description (living document)"
domain: mikrotik
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["gateway", "route", "uplink", "primary", "mikrotik", "router", "failover", "secondary"]
source: https://gist.github.com/tdussa/95d036a73bb5ff79924b19b4316ea7ca
source_anchor: ""
source_lines: [1, 286]
section: "Copilot_Microsoft"
---
This is meant to be a somewhat-easier-to-digest recap of the discussion
that can be found on the MikroTik forum at this URL:
https://forum.mikrotik.com/viewtopic.php?f=23&t=157048&p=836497&hilit=failover#p836497
Note that the forum discussion not only addresses *failover*, but also
*load balancing* at the same time (without explicitly saying so in the
beginning).

For the sake of this document, we'll make some assumptions:

1. There are two uplinks, each bringing its own gateway: Gateway 1 and
gateway 2 .  (For the sake of this discussion, let the IP address of
gateway 1 be `1.2.3.4` and that of gateway 2 be`2.3.4.5` ).
2. The uplink via gateway 1 is to be the primary route to take, while
the uplink via gateway 2 is to be the backup route only.  In other
words, traffic should move via `1.2.3.4` unless this uplink breaks.
3. The primary uplink is attached via the `ether1` interface, while the
secondary uplink is attached via the`ether2` interface.

The obvious approach is to define two default routes, with the primary route having a smaller metric than the secondary route:

```
/ip route add gateway=1.2.3.4 distance=1 comment="Primary route"
/ip route add gateway=2.3.4.5 distance=2 comment="Secondary route"
```
This will have everything routed via `1.2.3.4` unless the `ether1`
interface goes down.  If `ether1` does go down, then the primary route
will become invalidated, and traffic will be routed through the secondary
route. The general picture is this:

```
+-----------+
| Internet  | <----------------------------------------------------------+
+-----------+                                                            |
  ^                                                                      |
  |                                                                      |
  |                                                                      |
+-----------+  Uplink 1 (primary)   +--------+  Uplink 2 (secondary)   +-----------+
| Gateway 1 | <-------------------- | Router | ----------------------> | Gateway 2 |
+-----------+                       +--------+                         +-----------+
```
However, this also means that if `ether1` stays up, then traffic will
happily be routed through the primary route.  In particular, this is the
case if there is, for instance, a DSL modem or an ONT connected to an
ethernet port of the MikroTik router and the DSL connection or the fiber
connection fails.  So, assuming that uplink 1 uses, say, a DSL modem, the
above network graph would be a little more accurate like this:

```
+-----------+
| Internet  | <------------------+
+-----------+                    |
  ^                              |
  |                              |
  |                              |
+-----------+                  +-----------+
| Gateway 1 |                  | Gateway 2 |
+-----------+                  +-----------+
  ^                              ^
  | Uplink 1                     | Uplink 2
  |                              |
+-----------+  Ethernet link   +-----------+
| DSL Modem | <--------------- |  Router   |
+-----------+                  +-----------+
```
This illustrates the basic problem in such a setup: If the DSL link
(labeled `Uplink 1` in the graph) fails, then the ethernet link from the
router to the DSL modem is still up.  This means that the router will not
detect the failed uplink, the primary route will still hold, and there will
not be a failover.  In other words, there will be no internet connectivity
for the router (or any networks it routes).

To counter this, MikroTik routers have the concept of gateway checking. To use gateway checking, the primary default route can be defined like this:

```
/ip route add gateway=1.2.3.4 distance=1 check-gateway=ping comment="Primary route"
```
This makes the router ping the gateway IP every 10 seconds, and if two
consecutive pings get lost, then the gateway is marked as `unreachable`.
This means that if the DSL link fails, then after a little bit of time, the
router will still notice that the gateway IP (which is located on the far
side of the DSL link) does not respond, and the primary route will not be
used any more.  Furthermore, as soon as the gateway IP is reachable again,
the primary default route will become active again.  So far, so good.

However, there is still a problem in practice these days.  A number of link
failures occur further upstream, somewhere between the gateway and the
internet.  This means that in the above example, both the ethernet and the
DSL links are up and running, and gateway 1 is pingable at `1.2.3.4`. This
means that, again, the router will not detect the broken internet
connection.

To address this issue, recursive routes can be used. (Alternatively, a script could be written that regularly pings an arbitrary host on the internet that is expected to be up, and if that host is not pingable, assumes that the primary uplink has failed.)

To use recursive routes, a watchdog host is needed that is located
somewhere on the internet and is expected to be up and, crucially,
pingable.  As an example, we will use CloudFlare's DNS server, found at
`1.1.1.1`, but really any IP address that can reliably be expected to be
pingable will do.

First, and counterintuitively, we define the primary default route to use the watchdog host as gateway:

```
/ip route add gateway=1.1.1.1 distance=1 check-gateway=ping comment="Primary virtual route"
```
Of course, `1.1.1.1` is not a valid gateway, and so this would never work:
Traffic going through the default route would be sent to `1.1.1.1`, which
is not directly attached to the router, so it would not be reachable except
through the default route, which would result in a loop.  I am not sure if
this loop would finally be detected by the router, but even if it did, the
packets would end up being routed through the secondary default route to
1.1.1.1, which would obviously drop them, so it would not work either way.

An additional route is necessary, telling the router to use `1.2.3.4` (the
actual gateway 1) as gateway for all traffic for `1.1.1.1`:

```
/ip route add dst-address=1.1.1.1/32 gateway=1.2.3.4 scope=10 comment="Primary route"
```
This way, traffic going to the primary default route will be shipped to
`1.1.1.1`, and since `1.1.1.1` is not directly attached, the routing table
is consulted again.  However, this time around, there is a specific route
for `1.1.1.1` which pushes traffic through to `1.2.3.4`, so there is no
loop.  Instead, traffic eventually goes through gateway 1 by default.

If, however, any part of uplink 1 breaks, then `1.1.1.1` is not reachable
any more, which is detected by the router, and the first default route will
be invalidated, making the secondary default route active.

Of course, there is still another problem with this setup: What if the
watchdog host, `1.1.1.1`, breaks or is not pingable for another reason, but
"our" internet uplink 1 still works fine?  In that case, the primary
default route would also be invalidated, which is not desired.  To counter
this, multiple watchdog hosts can be used.  As an example, we will use
`1.1.1.1`, `8.8.8.8` and `9.9.9.9`:

```
/ip route
add gateway=1.1.1.1 distance=1 check-gateway=ping comment="Primary virtual route A"
add gateway=8.8.8.8 distance=1 check-gateway=ping comment="Primary virtual route B"
add gateway=9.9.9.9 distance=1 check-gateway=ping comment="Primary virtual route C"
add dst-address=1.1.1.1/32 gateway=1.2.3.4 scope=10 comment="Primary route A"
add dst-address=8.8.8.8/32 gateway=1.2.3.4 scope=10 comment="Primary route B"
add dst-address=9.9.9.9/32 gateway=1.2.3.4 scope=10 comment="Primary route C"
```
This way, all three of the defined watchdog/virtual gateway hosts must fail at the same time in order for the primary uplink to be considered invalid.

With the above configuration, if the watchdog hosts cannot be reached, the
primary route is disabled, and the secondary route takes over.  However,
this is true for all hosts *except* for the watchdog hosts (1.1.1.1,
8.8.8.8, and 9.9.9.9) because for these hosts, the direct route to the
primary gateway overrides the virtual route that is disabled.

This means that those hosts are not reachable from connected clients in
this case, which clearly is undesirable.  To work around this, traffic from
the clients must be pushed to a different routing table that does *not*
contain the direct routes.  At the same time, since the router itself
should maintain its connectivity, the virtual routes must be present in the
main routing table as well, or the router itself will not have any default
route.  Also, the secondary route must be defined in *both* the HA and the
main routing tables.  Finally, a routing rule must be established that
pushes all traffic from the LAN interfaces (in this example, we use the
`bridge` interface) to the `HA` routing table:

```
/ip route
add gateway=1.1.1.1 distance=1 routing-mark=HA comment="Primary virtual route A (HA)"
add gateway=8.8.8.8 distance=1 routing-mark=HA comment="Primary virtual route B (HA)"
add gateway=9.9.9.9 distance=1 routing-mark=HA comment="Primary virtual route C (HA)"
add gateway=2.3.4.5 distance=2 routing-mark=HA comment="Secondary route (HA)"
add gateway=1.1.1.1 distance=1 check-gateway=ping comment="Primary virtual route A"
add gateway=8.8.8.8 distance=1 check-gateway=ping comment="Primary virtual route B"
add gateway=9.9.9.9 distance=1 check-gateway=ping comment="Primary virtual route C"
add gateway=2.3.4.5 distance=2 comment="Secondary route"
add dst-address=1.1.1.1/32 gateway=1.2.3.4 scope=10 comment="Primary route A"
add dst-address=8.8.8.8/32 gateway=1.2.3.4 scope=10 comment="Primary route B"
add dst-address=9.9.9.9/32 gateway=1.2.3.4 scope=10 comment="Primary route C"
/ip route rule
add interface=bridge table=HA
```
Note that it is sufficient to have *one* route that checks the gateway
availability with pings, so we do not need `check-gateway` statements for
the `HA` table.

However, there *still* is another problem: The routes are *statically*
defined.  This means that if in a particular situation, the default uplink
is dynamically defined (say, via DHCP, which is not uncommon behind a DSL
link), then there needs to be a DHCP script defined that sets the `gateway`
value correctly in `Primary route A`, `Primary route B`, `Primary route C`
and `Secondary route`.  (In order to make our life easier, we define an
additional virtual route for the secondary route, going through 127.1.1.1,
so that there is only *one* non-virtual secondary route to update.)
Furthermore, we need scripts that update the gateways in the actual
(non-virtual) routes upon changes.  We assume that the primary uplink is
served by DHCP client number 0 and the secondary uplink is served by DHCP
client number 1:

```
/ip route
add gateway=1.1.1.1 distance=1 routing-mark=HA comment="Primary virtual route A (HA)"
add gateway=8.8.8.8 distance=1 routing-mark=HA comment="Primary virtual route B (HA)"
add gateway=9.9.9.9 distance=1 routing-mark=HA comment="Primary virtual route C (HA)"
add gateway=127.1.1.1 distance=2 routing-mark=HA comment="Secondary virtual route (HA)"
add gateway=1.1.1.1 distance=1 check-gateway=ping comment="Primary virtual route A"
add gateway=8.8.8.8 distance=1 check-gateway=ping comment="Primary virtual route B"
add gateway=9.9.9.9 distance=1 check-gateway=ping comment="Primary virtual route C"
add gateway=127.1.1.1 distance=2 comment="Secondary virtual route"
add dst-address=1.1.1.1/32 gateway=1.2.3.4 scope=10 comment="Primary route A"
add dst-address=8.8.8.8/32 gateway=1.2.3.4 scope=10 comment="Primary route B"
add dst-address=9.9.9.9/32 gateway=1.2.3.4 scope=10 comment="Primary route C"
add dst-address=127.1.1.1/32 gateway=2.3.4.5 scope=10 comment="Secondary route"
/ip route rule
add interface=bridge table=HA
/ip dhcp-client
set 0 add-default-route=no script="# Update primary route\n:if (\$bound=1) do={\n  /ip route set [/ip route find where gateway!=\$\"gateway-address\" and comment~\"Primary route \"] gateway=\$\"gateway-address\"\n}"
set 1 add-default-route=no script="# Update secondary route\n:if (\$bound=1) do={\n  /ip route set [/ip route find where gateway!=\$\"gateway-address\" and comment~\"Secondary route\"] gateway=\$\"gateway-address\"\n}"
```
Some interfaces (such as `pppoe-client`) do not run a DHCP client, but
handle routes directly.  In particular, there is no `script` hook that
allows for push-based update of the routes.  This requires more work.
For the sake of this discussion, assume that the primary uplink is not a
direct link with DHCP enabled, but a PPPoE line.  This means that there is
no `dhcp-client` config for that uplink, but the IP address can still
change upon reconnect.  Fortunately, the underlying PPP connection can be
misused by setting the remote IP address in the PPP profile to a static
private IP.  In this context, this IP is a virtual IP and is never used
except for the routing decision.  For consistency reasons, we pick
`127.1.1.1` for the primary uplink, which means that the secondary uplink
needs another static virtual IP; we bump that to `127.1.1.2`.

The trick is to create a PPP profile that defines the static remote IP:

```
/ppp profile add name=pppoe-static-profile remote-address=127.1.1.1
```
Then we need to tell the PPPoE client config to use that profile instead of
the `default` profile.  Also, no default route should be set by
`pppoe-client`:

```
/interface pppoe-client add comment="Primary uplink" disabled=no interface=uplink keepalive-timeout=disabled name=pppoe-uplink password=*password* profile=pppoe-static-profile user=*user*
```
The remainder of the settings are default settings.

Now we are all set for the complete setup.  All that is left to do is set
the primary routes to go via `127.1.1.1` and the secondary route via
`127.1.1.2`.  All in all, this is the result:

```
# Static routes
/ip route
add gateway=1.1.1.1 distance=1 routing-mark=HA comment="Primary virtual route A (HA)"
add gateway=8.8.8.8 distance=1 routing-mark=HA comment="Primary virtual route B (HA)"
add gateway=9.9.9.9 distance=1 routing-mark=HA comment="Primary virtual route C (HA)"
add gateway=127.1.1.2 distance=2 routing-mark=HA comment="Secondary virtual route (HA)"
add gateway=1.1.1.1 distance=1 check-gateway=ping comment="Primary virtual route A"
add gateway=8.8.8.8 distance=1 check-gateway=ping comment="Primary virtual route B"
add gateway=9.9.9.9 distance=1 check-gateway=ping comment="Primary virtual route C"
add gateway=127.1.1.2 distance=2 comment="Secondary virtual route"
add dst-address=1.1.1.1/32 gateway=127.1.1.1 scope=10 comment="Primary route A"
add dst-address=8.8.8.8/32 gateway=127.1.1.1 scope=10 comment="Primary route B"
add dst-address=9.9.9.9/32 gateway=127.1.1.1 scope=10 comment="Primary route C"
add dst-address=127.1.1.2/32 gateway=127.1.1.2 scope=10 comment="Secondary route"
# Clients attached via bridge0 get to use the HA routing table
/ip route rule
add interface=bridge0 table=HA
# Primary uplink via PPPoE
/interface pppoe-client
add comment="Primary uplink" interface=ether1 keepalive-timeout=disabled name=pppoe-primary password=*password* profile=pppoe-static-profile user=*user*
/ppp profile
add name=pppoe-static-profile remote-address=127.1.1.1
# Secondary uplink via DHCP-enabled interface
/ip dhcp-client
add add-default-route=no disabled=no interface=ether2 script=\
    "# Update secondary route\
    \n:if (\$bound=1) do={\
    \n  /ip route set [/ip route find where gateway!=\$\"gateway-address\" and comment~\"Secondary route\"] gateway=\$\"gateway-address\"\
    \n}" use-peer-dns=no use-peer-ntp=no`
```
Hopefully, this hack is also applicable to other dynamic, non-dhcp interfaces.

Issues that are left to tackle are:

- Notification/reset scripts in case of link failover.
- Routing for certain connections that will always go out via a given link, regardless of the failover state.

Great explanation, thank you. This works perfectly for me. My Primary is PPPOE to a WISP and backup via wlan station to my phone hotspot (DHCP). I implemented it at ROS 6.49 and was hoping it would all be translated when I upgraded to ROS 7.12. It wasn't. Is there an updated config that will work for Ros 7?
