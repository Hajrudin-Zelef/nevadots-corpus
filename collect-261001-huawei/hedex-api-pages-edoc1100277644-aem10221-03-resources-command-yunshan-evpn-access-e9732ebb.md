The evpn access vll convergence separate disable command enables the coupling flag in the EVPN-accessing-VLL direction.
The undo evpn access vll convergence separate disable command enables the decoupling flag in the EVPN-accessing-VLL direction.
By default, the decoupling flag is enabled in the EVPN-accessing-VLL direction.
evpn access vll convergence separate disable
undo evpn access vll convergence separate disable
Usage Scenario
To enable the coupling flag in the EVPN-accessing-VLL direction, run the evpn access vll convergence separate disable command. This configuration allows EVPN services to be forwarded only through an LDP, TE, or LDP over TE LSP as the public network tunnel.
Precautions
If the coupling flag is enabled in the EVPN-accessing-VLL direction, only LDP, TE, or LDP over TE LSPs are supported on the public network side of EVPN accessing VLL.