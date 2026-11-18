After configuring inter-AS seamless MPLS, you can check established LSPs and the connectivity of BGP LSPs between a CSG and an MASG.
Prerequisites
Inter-AS seamless MPLS has been configured.
Procedure
- Run the display ip routing-table command on a CSG or an MASG to check the routes to the peer end.
- Run the display mpls lsp command to check LSP information.
- Run the ping lsp [ -a source-ip | -c count | -exp exp-value | -h ttl-value | -m interval | -r reply-mode | -s packet-size | -t time-out | -v ] * bgp destination-iphost mask-length [ ip-address ] command on a CSG or MASG to check the BGP LSP connectivity.