---
id: collect-261121-veille-ia/langages-dev/header-and-logo-3-2
title: "Header And Logo"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["anchor", "rules", "ruleset", "main", "specified", "fragment", "name", "fragments"]
source: https://man.freebsd.org/cgi/man.cgi?query=pf.conf&sektion=5&manpath=freebsd-release-ports
source_anchor: ""
source_lines: [1501, 2006]
section: "Langages_Dev"
---
     never  applies  to a fragment, even if the fragment is part of a TCP packet
     with destination port 80, because without reassembly  this  information  is
     not  available  for  each	fragment.  This also means that fragments cannot
     create new or match existing state table entries, which makes stateful fil-
     tering and address translation (NAT, redirection) for fragments impossible.
     It's also possible to  reassemble	only  certain  fragments  by  specifying
     source or destination addresses or protocols as parameters in *scrub* rules.
     In  most  cases,  the benefits of reassembly outweigh the additional memory
     cost, and it's recommended to use *set* *reassemble* option or *scrub* rules with
     the *fragment* *reassemble* modifier to reassemble all fragments.
     The memory allocated for fragment caching can be  limited	using  *pfctl*(8).
     Once  this  limit	is  reached,  fragments that would have to be cached are
     dropped until other entries time out.  The timeout value can  also  be  ad-
     justed.
     When  forwarding  reassembled  IPv6  packets,  pf refragments them with the
     original maximum fragment size.  This allows the sender  to  determine  the
     optimal fragment size by path MTU discovery.
**ANCHORS**
     Besides the main ruleset, *pfctl*(8) can load rulesets into *anchor* attachment
     points.   An *anchor* is a container that can hold rules, address tables, and
     other anchors.
     An *anchor* has a name which specifies the path where *pfctl*(8) can be used to
     access the anchor to perform operations on it, such as attaching child  an-
     chors  to	it or loading rules into it.  Anchors may be nested, with compo-
     nents separated by `/' characters, similar to how file  system  hierarchies
     are  laid	out.  The main ruleset is actually the default anchor, so filter
     and translation rules, for example, may also be contained in any anchor.
     An anchor can reference another *anchor* attachment point using the following
     kinds of rules:
     *nat-anchor* <*name*>
	   Evaluates the *nat* rules in the specified *anchor*.
     *rdr-anchor* <*name*>
	   Evaluates the *rdr* rules in the specified *anchor*.
     *binat-anchor* <*name*>
	   Evaluates the *binat* rules in the specified *anchor*.
     *anchor* <*name*>
	   Evaluates the filter rules in the specified *anchor*.
     *load* *anchor* <*name*> *from* <*file*>
	   Loads the rules from the specified file into the anchor *name*.
     When evaluation of the main ruleset reaches an *anchor* rule, *pf*(4) will pro-
     ceed to evaluate all rules specified in that anchor.
     Matching filter and translation rules marked with the *quick* option are  fi-
     nal  and  abort  the  evaluation of the rules in other anchors and the main
     ruleset.  If the *anchor* itself is marked with  the  *quick*	option,  ruleset
     evaluation  will  terminate  when	the  anchor  is  exited if the packet is
     matched by any rule within the anchor.
     *anchor* rules are evaluated relative to the anchor in which  they  are  con-
     tained.   For  example, all *anchor* rules specified in the main ruleset will
     reference anchor attachment points underneath the main ruleset, and  *anchor*
     rules  specified  in a file loaded from a *load* *anchor* rule will be attached
     under that anchor point.
     Rules may be contained in *anchor* attachment points which do not contain any
     rules when the main ruleset is loaded, and later such anchors can be manip-
     ulated through *pfctl*(8) without reloading the main  ruleset  or  other  an-
     chors.  For example,
	   ext_if = "kue0"
	   block on $ext_if all
	   anchor spam
	   pass out on $ext_if all
	   pass in on $ext_if proto tcp from any \
		 to $ext_if port smtp
     blocks all packets on the external interface by default, then evaluates all
     rules  in	the *anchor* named "spam", and finally passes all outgoing connec-
     tions and incoming connections to port 25.
	   # echo "block in quick from 1.2.3.4 to any" | \
		 pfctl -a spam -f -
     This loads a single rule into the *anchor*, which blocks all packets  from  a
     specific address.
     The  anchor  can  also  be populated by adding a *load* *anchor* rule after the
     *anchor* rule:
	   anchor spam
	   load anchor spam from "/etc/pf-spam.conf"
     When *pfctl*(8) loads **pf.conf**, it will also load all the rules from the  file
     */etc/pf-spam.conf* into the anchor.
     Optionally,  *anchor* rules can specify packet filtering parameters using the
     same syntax as filter rules.  When parameters are used, the *anchor* rule  is
     only evaluated for matching packets.  This allows conditional evaluation of
     anchors, like:
	   block on $ext_if all
	   anchor spam proto tcp from any to any port smtp
	   pass out on $ext_if all
	   pass in on $ext_if proto tcp from any to $ext_if port smtp
     The rules inside *anchor* spam are only evaluated for *tcp* packets with desti-
     nation port 25.  Hence,
	   # echo "block in quick from 1.2.3.4 to any" | \
		 pfctl -a spam -f -
     will only block connections from 1.2.3.4 to port 25.
     Anchors may end with the asterisk (`*') character, which signifies that all
     anchors  attached at that point should be evaluated in the alphabetical or-
     dering of their anchor name.  For example,
	   anchor "spam/*"
     will evaluate each rule in each anchor attached to the **spam**  anchor.   Note
     that  it  will only evaluate anchors that are directly attached to the **spam**
     anchor, and will not descend to evaluate anchors recursively.
     Since anchors are evaluated relative to the anchor in which they  are  con-
     tained,  there is a mechanism for accessing the parent and ancestor anchors
     of a given anchor.  Similar to file system path name resolution, if the se-
     quence ".." appears as an anchor path component, the parent anchor  of  the
     current  anchor  in  the  path evaluation at that point will become the new
     current anchor.  As an example, consider the following:
	   # echo ' anchor "spam/allowed" ' | pfctl -f -
	   # echo -e ' anchor "../banned" \n pass' | \
		 pfctl -a spam/allowed -f -
     Evaluation of the main ruleset will  lead	into  the  **spam/allowed**  anchor,
     which will evaluate the rules in the **spam/banned** anchor, if any, before fi-
     nally evaluating the *pass* rule.
     An  *anchor*  rule  can  also  contain  a filter ruleset in a brace-delimited
     block.  In that case, no separate loading of rules into the anchor  is  re-
     quired.   Brace delimited blocks may contain rules or other brace-delimited
     blocks.  When an anchor is populated this way, the anchor name becomes  op-
     tional.
	   anchor "external" on $ext_if {
		   block
		   anchor out {
			   pass proto tcp from any to port { 25, 80, 443 }
		   }
		   pass in proto tcp to any port 22
	   }
     Since  the parser specification for anchor names is a string, any reference
     to an anchor name containing `/' characters will require double quote (`"')
     characters around the anchor name.
**SCTP CONSIDERATIONS**
     *pf*(4) supports *sctp*(4) connections.  It can match ports,  track  state  and
     NAT  SCTP	traffic.   However, it will not alter port numbers during nat or
     rdr translations.	Doing so would break SCTP multihoming.
**TRANSLATION EXAMPLES**
     This example maps incoming requests on port 80 to port  8080,  on	which  a
     daemon  is running (because, for example, it is not run as root, and there-
     fore lacks permission to bind to port 80).
	   # use a macro for the interface name, so it can be changed easily
	   ext_if = "ne3"
	   # map daemon on 8080 to appear to be on 80
	   match in on $ext_if proto tcp from any to any port 80 \
		 rdr-to 127.0.0.1 port 8080
     If a *pass* rule is used with the *quick* modifier, packets matching the trans-
     lation rule are passed without inspecting subsequent filter rules:
	   pass in quick on $ext_if proto tcp from any to any port 80 \
		 rdr-to 127.0.0.1 port 8080
     In the example below, vlan12 is configured as  192.168.168.1;  the  machine
     translates  all  packets coming from 192.168.168.0/24 to 204.92.77.111 when
     they are going out any interface except vlan12.  This has the net effect of
     making traffic from the 192.168.168.0/24 network appear as though it is the
     Internet routable address 204.92.77.111 to nodes behind  any  interface  on
     the  router  except for the nodes on vlan12.  (Thus, 192.168.168.1 can talk
     to the 192.168.168.0/24 nodes.)
	   match out on ! vlan12 from 192.168.168.0/24 to any nat-to 204.92.77.111
     This longer example uses both a NAT and a redirection.  The external inter-
     face has the address 157.161.48.183.  On localhost,  we  are  running  *ftp-*
     *proxy*(8),	waiting  for  FTP  sessions  to  be redirected to it.  The three
     mandatory anchors for *ftp-proxy*(8) are omitted from this example;	see  the
     *ftp-proxy*(8) manpage.
	   # NAT
	   # Translate outgoing packets' source addresses (any protocol).
	   # In this case, any address but the gateway's external address is mapped.
	   pass out on $ext_if inet from ! ($ext_if) to any nat-to ($ext_if)
	   # NAT PROXYING
	   # Map outgoing packets' source port to an assigned proxy port instead of
	   # an arbitrary port.
	   # In this case, proxy outgoing isakmp with port 500 on the gateway.
	   pass out on $ext_if inet proto udp from any port = isakmp to any \
		 nat-to ($ext_if) port 500
	   # BINAT
	   # Translate outgoing packets' source address (any protocol).
	   # Translate incoming packets' destination address to an internal machine
	   # (bidirectional).
	   pass on $ext_if from 10.1.2.150 to any binat-to $ext_if
	   # Translate packets arriving on $peer_if addressed to 172.22.16.0/20
	   # to the corresponding address in 172.21.16.0/20 (bidirectional).
	   pass on $peer_if from 172.21.16.0/20 to any binat-to 172.22.16.0/20
	   # RDR
	   # Translate incoming packets' destination addresses.
	   # As an example, redirect a TCP and UDP port to an internal machine.
	   pass in on $ext_if inet proto tcp from any to ($ext_if) port 8080 \
		 rdr-to 10.1.2.151 port 22
	   pass in on $ext_if inet proto udp from any to ($ext_if) port 8080 \
		 rdr-to 10.1.2.151 port 53
	   # RDR
	   # Translate outgoing ftp control connections to send them to localhost
	   # for proxying with ftp-proxy(8) running on port 8021.
	   pass in on $int_if proto tcp from any to any port 21 \
		 rdr-to 127.0.0.1 port 8021
     In  this  example,  a NAT gateway is set up to translate internal addresses
     using a pool of public addresses (192.0.2.16/28) and to  redirect	incoming
     web server connections to a group of web servers on the internal network.
	   # NAT LOAD BALANCE
	   # Translate outgoing packets' source addresses using an address pool.
	   # A given source address is always translated to the same pool address by
	   # using the source-hash keyword.
	   pass out on $ext_if inet from any to any nat-to 192.0.2.16/28 source-hash
	   # RDR ROUND ROBIN
	   # Translate incoming web server connections to a group of web servers on
	   # the internal network.
	   pass in on $ext_if proto tcp from any to any port 80 \
		 rdr-to { 10.1.2.155, 10.1.2.160, 10.1.2.161 } round-robin
**COMPATIBILITY TRANSLATION EXAMPLES**
     In  the example below, the machine sits between a fake internal 144.19.74.*
     network, and a routable external IP of 204.92.77.100.  The *no* *nat* rule  ex-
     cludes protocol AH from being translated.
	   # NAT
	   no nat on $ext_if proto ah from 144.19.74.0/24 to any
	   nat on $ext_if from 144.19.74.0/24 to any -> 204.92.77.100
     In  the  example  below,  packets bound for one specific server, as well as
     those generated by the sysadmins are not  proxied;  all  other  connections
     are.
	   # RDR
	   no rdr on $int_if proto { tcp, udp } from any to $server port 80
	   no rdr on $int_if proto { tcp, udp } from $sysadmins to any port 80
	   rdr on $int_if proto { tcp, udp } from any to any port 80 \
		 -> 127.0.0.1 port 80
**FILTER EXAMPLES**
	   # The external interface is kue0
	   # (157.161.48.183, the only routable address)
	   # and the private network is 10.0.0.0/8, for which we are doing NAT.
	   # Reassemble incoming traffic
	   set reassemble yes
	   # use a macro for the interface name, so it can be changed easily
	   ext_if = "kue0"
	   # block and log everything by default
	   block return log on $ext_if all
	   # block anything coming from source we have no back routes for
	   block in from no-route to any
	   # block packets whose ingress interface does not match the one in
	   # the route back to their source address
	   block in from urpf-failed to any
	   # block and log outgoing packets that do not have our address as source,
	   # they are either spoofed or something is misconfigured (NAT disabled,
	   # for instance), we want to be nice and do not send out garbage.
	   block out log quick on $ext_if from ! 157.161.48.183 to any
	   # silently drop broadcasts (cable modem noise)
	   block in quick on $ext_if from any to 255.255.255.255
	   # block and log incoming packets from reserved address space and invalid
	   # addresses, they are either spoofed or misconfigured, we cannot reply to
	   # them anyway (hence, no return-rst).
	   block in log quick on $ext_if from { 10.0.0.0/8, 172.16.0.0/12, \
		 192.168.0.0/16, 255.255.255.255/32 } to any
	   # ICMP
	   # pass out/in certain ICMP queries and keep state (ping)
	   # state matching is done on host addresses and ICMP id (not type/code),
	   # so replies (like 0/0 for 8/0) will match queries
	   # ICMP error messages (which always refer to a TCP/UDP packet) are
	   # handled by the TCP/UDP states
	   pass on $ext_if inet proto icmp all icmp-type 8 code 0
	   # UDP
	   # pass out all UDP connections and keep state
	   pass out on $ext_if proto udp all
	   # pass in certain UDP connections and keep state (DNS)
	   pass in on $ext_if proto udp from any to any port domain
	   # TCP
	   # pass out all TCP connections and modulate state
	   pass out on $ext_if proto tcp all modulate state
	   # pass in certain TCP connections and keep state (SSH, SMTP, DNS, IDENT)
	   pass in on $ext_if proto tcp from any to any port { ssh, smtp, domain, \
		 auth }
	   # Do not allow Windows 9x SMTP connections since they are typically
	   # a viral worm. Alternately we could limit these OSes to 1 connection each.
	   block in on $ext_if proto tcp from any os {"Windows 95", "Windows 98"} \
		 to any port smtp
	   # IPv6
	   # pass in/out all IPv6 traffic: note that we have to enable this in two
	   # different ways, on both our physical interface and our tunnel
	   pass quick on gif0 inet6
	   pass quick on $ext_if proto ipv6
	   # Packet Tagging
	   # three interfaces: $int_if, $ext_if, and $wifi_if (wireless). NAT is
	   # being done on $ext_if for all outgoing packets. tag packets in on
	   # $int_if and pass those tagged packets out on $ext_if.  all other
	   # outgoing packets (i.e., packets from the wireless network) are only
	   # permitted to access port 80.
	   pass in on $int_if from any to any tag INTNET
	   pass in on $wifi_if from any to any
	   block out on $ext_if from any to any
	   pass out quick on $ext_if tagged INTNET
	   pass out on $ext_if proto tcp from any to any port 80
	   # tag incoming packets as they are redirected to spamd(8). use the tag
	   # to pass those packets through the packet filter.
	   rdr on $ext_if inet proto tcp from <spammers> to port smtp \
		   tag SPAMD -> 127.0.0.1 port spamd
	   block in on $ext_if
	   pass in on $ext_if inet proto tcp tagged SPAMD
     In the example below, a router handling both address families translates an
     internal IPv4 subnet to IPv6 using the well-known 64:ff9b::/96 prefix:
	 pass in on $v4_if inet af-to inet6 from ($v6_if) to 64:ff9b::/96
     Paired  with  the	example  above, the example below can be used on another
     router handling both address families to translate back to IPv4:
	 pass in on $v6_if inet6 to 64:ff9b::/96 af-to inet from ($v4_if)
**GRAMMAR**
     Syntax for **pf.conf** in BNF:
     line	    = ( option | ether-rule | pf-rule | nat-rule | binat-rule |
		      rdr-rule | antispoof-rule | altq-rule | queue-rule |
		      trans-anchors | anchor-rule | anchor-close | load-anchor |
		      table-rule | include )
     option	    = "set" ( [ "timeout" ( timeout | "{" timeout-list "}" ) ] |
		      [ "ruleset-optimization" [ "none" | "basic" | "profile" ]] |
		      [ "optimization" [ "default" | "normal" |
		      "high-latency" | "satellite" |
		      "aggressive" | "conservative" ] ]
		      [ "limit" ( limit-item | "{" limit-list "}" ) ] |
		      [ "loginterface" ( interface-name | "none" ) ] |
		      [ "block-policy" ( "drop" | "return" ) ] |
		      [ "state-policy" ( "if-bound" | "floating" ) ]
		      [ "state-defaults" state-opts ]
		      [ "require-order" ( "yes" | "no" ) ]
		      [ "fingerprints" filename ] |
		      [ "skip on" ifspec ] |
		      [ "debug" ( "none" | "urgent" | "misc" | "loud" ) ]
		      [ "keepcounters" ] )
     ether-rule     = "ether" etheraction [ ( "in" | "out" ) ]
		      [ "quick" ] [ "on" ifspec ] [ "bridge-to" interface-name ]
		      [ etherprotospec ] [ etherhosts ] [ "l3" hosts ]
		      [ etherfilteropt-list ]
     pf-rule	    = action [ ( "in" | "out" ) ]
		      [ "log" [ "(" logopts ")"] ] [ "quick" ]
		      [ "on" ifspec ] [ route ] [ af ] [ protospec ]
		      [ hosts ] [ filteropt-list ]
     logopts	    = logopt [ "," logopts ]
     logopt	    = "all" | "matches" | "user" | "to" interface-name
     etherfilteropt-list = etherfilteropt-list etherfilteropt | etherfilteropt
     etherfilteropt = "tag" string | "tagged" string | "queue" ( string ) |
		      "ridentifier" number | "label" string
     filteropt-list = filteropt-list filteropt | filteropt
     filteropt	    = user | group | flags | icmp-type | icmp6-type | "tos" tos |
		      "af-to" af "from" ( redirhost | "{" redirhost-list "}" )
		      [ "to" ( redirhost | "{" redirhost-list "}" ) ] |
		      ( "no" | "keep" | "modulate" | "synproxy" ) "state"
		      [ "(" state-opts ")" ] |
		      "fragment" | "no-df" | "min-ttl" number | "set-tos" tos |
		      "max-mss" number | "random-id" | "reassemble tcp" |
		      fragmentation | "allow-opts" |
		      "label" string | "tag" string | [ "!" ] "tagged" string |
		      "max-pkt-rate" number "/" seconds |
		      "set prio" ( number | "(" number [ [ "," ] number ] ")" ) |
		      "max-pkt-size" number |
		      "queue" ( string | "(" string [ [ "," ] string ] ")" ) |
		      "rtable" number | "probability" number"%" | "prio" number |
		      "dnpipe" ( number | "(" number "," number ")" ) |
		      "dnqueue" ( number | "(" number "," number ")" ) |
		      "ridentifier" number |
		      "binat-to" ( redirhost | "{" redirhost-list "}" )
		      [ portspec ] [ pooltype ] |
		      "rdr-to" ( redirhost | "{" redirhost-list "}" )
		      [ portspec ] [ pooltype ] |
		      "nat-to" ( redirhost | "{" redirhost-list "}" )
		      [ portspec ] [ pooltype ] [ "static-port" ] |
		      [ ! ] "received-on" ( interface-name | interface-group )
     nat-rule	    = [ "no" ] "nat" [ "pass" [ "log" [ "(" logopts ")" ] ] ]
		      [ "on" ifspec ] [ af ]
		      [ protospec ] hosts [ "tag" string ] [ "tagged" string ]
		      [ "->" ( redirhost | "{" redirhost-list "}" )
		      [ portspec ] [ pooltype ] [ "static-port" ]
		      [ "map-e-portset" number "/" number "/" number ] ]
     binat-rule     = [ "no" ] "binat" [ "pass" [ "log" [ "(" logopts ")" ] ] ]
		      [ "on" interface-name ] [ af ]
		      [ "proto" ( proto-name | proto-number ) ]
		      "from" address [ "/" mask-bits ] "to" ipspec
		      [ "tag" string ] [ "tagged" string ]
		      [ "->" address [ "/" mask-bits ] ]
     rdr-rule	    = [ "no" ] "rdr" [ "pass" [ "log" [ "(" logopts ")" ] ] ]
		      [ "on" ifspec ] [ af ]
		      [ protospec ] hosts [ "tag" string ] [ "tagged" string ]
		      [ "->" ( redirhost | "{" redirhost-list "}" )
		      [ portspec ] [ pooltype ] ]
     antispoof-rule = "antispoof" [ "log" ] [ "quick" ]
		      "for" ifspec [ af ] [ "label" string ]
		      [ "ridentifier" number ]
     table-rule     = "table" "<" string ">" [ tableopts-list ]
     tableopts-list = tableopts-list tableopts | tableopts
     tableopts	    = "persist" | "const" | "counters" | "file" string |
		      "{" [ tableaddr-list ] "}"
     tableaddr-list = tableaddr-list [ "," ] tableaddr-spec | tableaddr-spec
     tableaddr-spec = [ "!" ] tableaddr [ "/" mask-bits ]
     tableaddr	    = hostname | ifspec | "self" |
		      ipv4-dotted-quad | ipv6-coloned-hex
     altq-rule	    = "altq on" interface-name queueopts-list
		      "queue" subqueue
     queue-rule     = "queue" string [ "on" interface-name ] queueopts-list
		      subqueue
     anchor-rule    = "anchor" [ string ] [ ( "in" | "out" ) ] [ "on" ifspec ]
		      [ af ] [ protospec ] [ hosts ] [ filteropt-list ] [ "{" ]
     anchor-close   = "}"
     trans-anchors  = ( "nat-anchor" | "rdr-anchor" | "binat-anchor" ) string
		      [ "on" ifspec ] [ af ] [ "proto" ] [ protospec ] [ hosts ]
     load-anchor    = "load anchor" string "from" filename
     queueopts-list = queueopts-list queueopts | queueopts
     queueopts	    = [ "bandwidth" bandwidth-spec ] |
		      [ "qlimit" number ] | [ "tbrsize" number ] |
		      [ "priority" number ] | [ schedulers ]
     schedulers     = ( cbq-def | priq-def | hfsc-def )
     bandwidth-spec = "number" ( "b" | "Kb" | "Mb" | "Gb" | "%" )
     etheraction    = "pass" | "block"
     action	    = "pass" | "match" | "block" [ return ] | [ "no" ] "scrub"
     return	    = "drop" | "return" | "return-rst" [ "( ttl" number ")" ] |
		      "return-icmp" [ "(" icmpcode [ [ "," ] icmp6code ] ")" ] |
		      "return-icmp6" [ "(" icmp6code ")" ]
     icmpcode	    = ( icmp-code-name | icmp-code-number )
     icmp6code	    = ( icmp6-code-name | icmp6-code-number )
     ifspec	    = ( [ "!" ] ( interface-name | interface-group ) ) |
		      "{" interface-list "}"
     interface-list = [ "!" ] ( interface-name | interface-group )
		      [ [ "," ] interface-list ]
     route	    = ( "route-to" | "reply-to" | "dup-to" )
		      ( routehost | "{" routehost-list "}" )
		      [ pooltype ]
     af 	    = "inet" | "inet6"
     etherprotospec = "proto" ( proto-number | "{" etherproto-list "}" )
     etherproto-list = proto-number [ [ "," ] etherproto-list ]
     protospec	    = "proto" ( proto-name | proto-number |
		      "{" proto-list "}" )
     proto-list     = ( proto-name | proto-number ) [ [ "," ] proto-list ]
     etherhosts     = "from" macaddress "to" macaddress
     macaddress     = mac | mac "/" masklen | mac "&" mask
     hosts	    = "all" |
		      "from" ( "any" | "no-route" | "urpf-failed" | "self" | host |
		      "{" host-list "}" ) [ port ] [ os ]
		      "to"   ( "any" | "no-route" | "self" | host |
		      "{" host-list "}" ) [ port ]
     ipspec	    = "any" | host | "{" host-list "}"
     host	    = [ "!" ] ( address [ "/" mask-bits ] | "<" string ">" )
     redirhost	    = address [ "/" mask-bits ]
     routehost	    = "(" interface-name address [ "/" mask-bits ] ")"
     address	    = ( interface-name | interface-group |
		      "(" ( interface-name | interface-group ) ")" |
		      hostname | ipv4-dotted-quad | ipv6-coloned-hex )
     host-list	    = host [ [ "," ] host-list ]
     redirhost-list = redirhost [ [ "," ] redirhost-list ]
     routehost-list = routehost [ [ "," ] routehost-list ]
     port	    = "port" ( unary-op | binary-op | "{" op-list "}" )
     portspec	    = "port" ( number | name ) [ ":" ( "*" | number | name ) ]
     os 	    = "os"  ( os-name | "{" os-list "}" )
     user	    = "user" ( unary-op | binary-op | "{" op-list "}" )
     group	    = "group" ( unary-op | binary-op | "{" op-list "}" )
     unary-op	    = [ "=" | "!=" | "<" | "<=" | ">" | ">=" ]
		      ( name | number )
     binary-op	    = number ( "<>" | "><" | ":" ) number
     op-list	    = ( unary-op | binary-op ) [ [ "," ] op-list ]
     os-name	    = operating-system-name
     os-list	    = os-name [ [ "," ] os-list ]
     flags	    = "flags" ( [ flag-set ] "/"  flag-set | "any" )
     flag-set	    = [ "F" ] [ "S" ] [ "R" ] [ "P" ] [ "A" ] [ "U" ] [ "E" ]
		      [ "W" ]
     icmp-type	    = "icmp-type" ( icmp-type-code | "{" icmp-list "}" )
     icmp6-type     = "icmp6-type" ( icmp-type-code | "{" icmp-list "}" )
     icmp-type-code = ( icmp-type-name | icmp-type-number )
		      [ "code" ( icmp-code-name | icmp-code-number ) ]
     icmp-list	    = icmp-type-code [ [ "," ] icmp-list ]
     tos	    = ( "lowdelay" | "throughput" | "reliability" |
		      [ "0x" ] number )
     state-opts     = state-opt [ [ "," ] state-opts ]
     state-opt	    = ( "max" number | "no-sync" | timeout | "sloppy" |
		      "source-track" [ ( "rule" | "global" ) ] |
		      "max-src-nodes" number | "max-src-states" number |
		      "max-src-conn" number |
		      "max-src-conn-rate" number "/" number |
		      "overload" "<" string ">" [ "flush" ] |
		      "if-bound" | "floating" | "pflow" )
     fragmentation  = [ "fragment reassemble" ]
     timeout-list   = timeout [ [ "," ] timeout-list ]
     timeout	    = ( "tcp.first" | "tcp.opening" | "tcp.established" |
		      "tcp.closing" | "tcp.finwait" | "tcp.closed" |
		      "sctp.first" | "sctp.opening" | "sctp.established" |
		      "sctp.closing" | "sctp.closed" |
		      "udp.first" | "udp.single" | "udp.multiple" |
		      "icmp.first" | "icmp.error" |
		      "other.first" | "other.single" | "other.multiple" |
		      "frag" | "interval" | "src.track" |
		      "adaptive.start" | "adaptive.end" ) number
     limit-list     = limit-item [ [ "," ] limit-list ]
     limit-item     = ( "states" | "frags" | "src-nodes" ) number
     pooltype	    = ( "bitmask" | "random" |
		      "source-hash" [ ( hex-key | string-key ) ] |
		      "round-robin" ) [ sticky-address | prefer-ipv6-nexthop ]
     subqueue	    = string | "{" queue-list "}"
     queue-list     = string [ [ "," ] string ]
     cbq-def	    = "cbq" [ "(" cbq-opt [ [ "," ] cbq-opt ] ")" ]
     priq-def	    = "priq" [ "(" priq-opt [ [ "," ] priq-opt ] ")" ]
     hfsc-def	    = "hfsc" [ "(" hfsc-opt [ [ "," ] hfsc-opt ] ")" ]
     cbq-opt	    = ( "default" | "borrow" | "red" | "ecn" | "rio" )
     priq-opt	    = ( "default" | "red" | "ecn" | "rio" )
     hfsc-opt	    = ( "default" | "red" | "ecn" | "rio" |
		      linkshare-sc | realtime-sc | upperlimit-sc )
     linkshare-sc   = "linkshare" sc-spec
     realtime-sc    = "realtime" sc-spec
     upperlimit-sc  = "upperlimit" sc-spec
     sc-spec	    = ( bandwidth-spec |
		      "(" bandwidth-spec number bandwidth-spec ")" )
     include	    = "include" filename
**FILES**
     */etc/hosts*      Host name database.
     */etc/pf.conf*    Default location of the ruleset file.  The file has  to  be
		     created manually as it is not installed with a standard in-
		     stallation.
     */etc/pf.os*      Default location of OS fingerprints.
     */etc/protocols*  Protocol name database.
     */etc/services*   Service name database.
**SEE ALSO**
     *altq*(4),  *carp*(4),  *icmp*(4),  *icmp6*(4), *ip*(4), *ip6*(4), *pf*(4), *pflow*(4), *pf-*
     *sync*(4), *sctp*(4), *tcp*(4), *udp*(4), *hosts*(5),  *pf.os*(5),  *protocols*(5),  *ser-*
     *vices*(5), *ftp-proxy*(8), *pfctl*(8), *pflogd*(8)
**HISTORY**
     The **pf.conf** file format first appeared in OpenBSD 3.0.
FreeBSD 15.1			November 3, 2025		      *PF.CONF*(5)

NAME | DESCRIPTION | STATEMENT ORDER | MACROS | TABLES | OPTIONS | ETHERNET FILTERING | TRAFFIC NORMALIZATION | QUEUEING with ALTQ | QUEUEING with dummynet | TRANSLATION | PACKET FILTERING | ROUTING | POOL OPTIONS | STATE MODULATION | SYN PROXY | STATEFUL TRACKING OPTIONS | OPERATING SYSTEM FINGERPRINTING | BLOCKING SPOOFED TRAFFIC | FRAGMENT HANDLING | ANCHORS | SCTP CONSIDERATIONS | TRANSLATION EXAMPLES | COMPATIBILITY TRANSLATION EXAMPLES | FILTER EXAMPLES | GRAMMAR | FILES | SEE ALSO | HISTORY

Want to link to this manual page? Use this URL:

<https://man.freebsd.org/cgi/man.cgi?query=pf.conf&sektion=5&manpath=FreeBSD+15.1-RELEASE+and+Ports>
