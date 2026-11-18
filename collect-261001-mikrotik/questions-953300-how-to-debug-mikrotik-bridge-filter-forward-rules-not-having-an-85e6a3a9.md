As part of diagnosing a different problem were trying to add a bridge filter rule that will stop all traffic from forwarding between two interfaces on a bridge.
The router has two interfaces ether1 and ether2 on a bridge.
we then added a rule with this
/interface bridge filter
add action=drop chain=forward in-interface=ether1
i had expected this to stop all traffic that arrived on ether1 from being forwarded across the bridge and going out ether2. However traffic continues to flow and this rule has no effect.