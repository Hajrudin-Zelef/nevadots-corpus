---
id: collect-261121-veille-ia/opnsense-pfsense/github-o-x-l-ansible-opnsense-ansible-collection-to-manage-opnsense-firewalls-using-their-api
title: "GitHub - O-X-L/ansible-opnsense: Ansible Collection to manage OPNsense firewalls using their API"
domain: opnsense-pfsense
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["opnsense", "oxlorg", "docs", "stable", "ipsec", "collection", "alias", "unbound"]
source: https://github.com/o-x-l/ansible-opnsense
source_anchor: ""
source_lines: [1, 211]
section: "Copilot_Microsoft"
---
**Functional Tests**: - (no resources)

The httpx python module is used for API communications!

`python3 -m pip install --upgrade httpx`
Then - install the collection itself:

```
# latest version:
ansible-galaxy collection install git+https://github.com/O-X-L/ansible-opnsense.git
# stable/tested version:
ansible-galaxy collection install git+https://github.com/O-X-L/ansible-opnsense.git,26.1.11
## OR
ansible-galaxy collection install oxlorg.opnsense
```
See: Docs

If you DO NOT want to use Ansible - this fork provides you with a raw Python3 interface.

Support the Open-Source projects that make these modules possible:

- Donate to OPNsense or Buy the Business-Edition
- Contact the ansible-collection maintainer for support

Feel free to contribute to this project using pull-requests, issues and discussions!

See also: Contributing

Currently, these modules have no multi-version support!

That means each release of the collection is only compatible with the same OPNsense version!

If you need to use these modules - ensure that there is a supported release BEFORE upgrading your OPNsense firewalls!

I currently try to create a stable release once or twice a year - as this takes 10-20h of work each time for implementing API-fixes.

As this project is unfunded we do not actively check for API-changes - if you find missing functionalities you need/want to have please report it! Not all features will be implemented!

**Development States**:

not implemented => development => testing => unstable (*practical testing*) => stable

| Function | Module | Usage | State | 
|---|---|---|---|
| **Base** | oxlorg.opnsense.list | Docs | stable | 
| **Base** | oxlorg.opnsense.reload | Docs | stable | 
| **Raw** | oxlorg.opnsense.raw | Docs | unstable | 
| **Services** | oxlorg.opnsense.service | Docs | stable | 
| **Alias** | oxlorg.opnsense.alias | Docs | stable | 
| **Alias** | oxlorg.opnsense.alias_multi | Docs | stable | 
| **Alias** | oxlorg.opnsense.alias_purge | Docs | unstable | 
| **Rules** | oxlorg.opnsense.rule | Docs | stable | 
| **Rules** | oxlorg.opnsense.rule_multi | Docs | stable | 
| **Rules** | oxlorg.opnsense.rule_purge | Docs | unstable | 
| **Rule Interface Groups** | oxlorg.opnsense.rule_interface_group | Docs | stable | 
| **Savepoints** | oxlorg.opnsense.savepoint | Docs | stable | 
| **Packages** | oxlorg.opnsense.package | Docs | stable | 
| **System** | oxlorg.opnsense.system | Docs | stable | 
| **Cron-Jobs** | oxlorg.opnsense.cron | Docs | stable | 
| **Routes** | oxlorg.opnsense.route | Docs | stable | 
| **Gateways** | oxlorg.opnsense.gateway | Docs | stable | 
| **DNS** | oxlorg.opnsense.unbound_general | Docs | stable | 
| **DNS** | oxlorg.opnsense.unbound_acl | Docs | stable | 
| **DNS** | oxlorg.opnsense.unbound_forward | Docs | stable | 
| **DNS** | oxlorg.opnsense.unbound_dot | Docs | stable | 
| **DNS** | oxlorg.opnsense.unbound_host | Docs | stable | 
| **DNS** | oxlorg.opnsense.unbound_host_alias | Docs | stable | 
| **DNS** | oxlorg.opnsense.unbound_dnsbl | Docs | stable | 
| **Syslog** | oxlorg.opnsense.syslog | Docs | stable | 
| **IPSec** | oxlorg.opnsense.ipsec_connection, oxlorg.opnsense.ipsec_tunnel | Docs | stable | 
| **IPSec** | oxlorg.opnsense.ipsec_pool, oxlorg.opnsense.ipsec_network | Docs | stable | 
| **IPSec** | oxlorg.opnsense.ipsec_auth_local | Docs | stable | 
| **IPSec** | oxlorg.opnsense.ipsec_auth_remote | Docs | stable | 
| **IPSec** | oxlorg.opnsense.ipsec_child | Docs | stable | 
| **IPSec** | oxlorg.opnsense.ipsec_vti | Docs | stable | 
| **IPSec** | oxlorg.opnsense.ipsec_cert | Docs | stable | 
| **IPSec** | oxlorg.opnsense.ipsec_psk | Docs | stable | 
| **IPSec** | oxlorg.opnsense.ipsec_manual_spd | Docs | stable | 
| **IPSec** | oxlorg.opnsense.general | Docs | unstable | 
| **Traffic Shaper** | oxlorg.opnsense.shaper_pipe | Docs | stable | 
| **Traffic Shaper** | oxlorg.opnsense.shaper_queue | Docs | stable | 
| **Traffic Shaper** | oxlorg.opnsense.shaper_rule | Docs | stable | 
| **Monit** | oxlorg.opnsense.monit_service | Docs | stable | 
| **Monit** | oxlorg.opnsense.monit_alert | Docs | stable | 
| **Monit** | oxlorg.opnsense.monit_test | Docs | stable | 
| **WireGuard** | oxlorg.opnsense.wireguard_server | Docs | stable | 
| **WireGuard** | oxlorg.opnsense.wireguard_peer | Docs | stable | 
| **WireGuard** | oxlorg.opnsense.wireguard_show | Docs | stable | 
| **WireGuard** | oxlorg.opnsense.wireguard_general | Docs | stable | 
| **Interfaces** | oxlorg.opnsense.interface_vlan | Docs | stable | 
| **Interfaces** | oxlorg.opnsense.interface_vxlan | Docs | stable | 
| **Interfaces** | oxlorg.opnsense.interface_vip | Docs | stable | 
| **Interfaces** | oxlorg.opnsense.interface_lagg | Docs | stable | 
| **Interfaces** | oxlorg.opnsense.interface_loopback | Docs | stable | 
| **Interfaces** | oxlorg.opnsense.interface_gre | Docs | stable | 
| **Interfaces** | oxlorg.opnsense.interface_bridge | Docs | unstable | 
| **Interfaces** | oxlorg.opnsense.interface_gif | Docs | unstable | 
| **NAT** | oxlorg.opnsense.nat_source | Docs | stable | 
| **NAT** | oxlorg.opnsense.nat_destination | Docs | unstable | 
| **NAT** | oxlorg.opnsense.nat_one_to_one | Docs | stable | 
| **Dynamic Routing** | oxlorg.opnsense.frr_diagnostic | Docs | stable | 
| **Dynamic Routing** | oxlorg.opnsense.frr_general | Docs | stable | 
| **Dynamic Routing** | oxlorg.opnsense.frr_bfd_general | Docs | stable | 
| **Dynamic Routing** | oxlorg.opnsense.frr_bfd_neighbor | Docs | stable | 
| **Dynamic Routing** | oxlorg.opnsense.frr_bgp_general | Docs | stable | 
| **Dynamic Routing** | oxlorg.opnsense.frr_bgp_neighbor | Docs | stable | 
| **Dynamic Routing** | oxlorg.opnsense.frr_bgp_prefix_list | Docs | stable | 
| **Dynamic Routing** | oxlorg.opnsense.frr_bgp_route_map | Docs | stable | 
| **Dynamic Routing** | oxlorg.opnsense.frr_bgp_community_list | Docs | stable | 
| **Dynamic Routing** | oxlorg.opnsense.frr_bgp_as_path | Docs | stable | 
| **Dynamic Routing** | oxlorg.opnsense.frr_bgp_redistribution | Docs | stable | 
| **Dynamic Routing** | oxlorg.opnsense.frr_bgp_peer_group | Docs | stable | 
| **Dynamic Routing** | oxlorg.opnsense.frr_ospf_general | Docs | stable | 
| **Dynamic Routing** | oxlorg.opnsense.frr_ospf_prefix_list | Docs | stable | 
| **Dynamic Routing** | oxlorg.opnsense.frr_ospf_route_map | Docs | stable | 
| **Dynamic Routing** | oxlorg.opnsense.frr_ospf_interface | Docs | stable | 
| **Dynamic Routing** | oxlorg.opnsense.frr_ospf_network | Docs | stable | 
| **Dynamic Routing** | oxlorg.opnsense.frr_ospf_redistribution | Docs | stable | 
| **Dynamic Routing** | oxlorg.opnsense.frr_ospf3_general | Docs | stable | 
| **Dynamic Routing** | oxlorg.opnsense.frr_ospf3_prefix_list | Docs | stable | 
| **Dynamic Routing** | oxlorg.opnsense.frr_ospf3_route_map | Docs | stable | 
| **Dynamic Routing** | oxlorg.opnsense.frr_ospf3_interface | Docs | stable | 
| **Dynamic Routing** | oxlorg.opnsense.frr_ospf3_network | Docs | stable | 
| **Dynamic Routing** | oxlorg.opnsense.frr_ospf3_redistribution | Docs | stable | 
| **Dynamic Routing** | oxlorg.opnsense.frr_rip | Docs | stable | 
| **DNS** | oxlorg.opnsense.bind_general | Docs | stable | 
| **DNS** | oxlorg.opnsense.bind_blocklist | Docs | stable | 
| **DNS** | oxlorg.opnsense.bind_acl | Docs | stable | 
| **DNS** | oxlorg.opnsense.bind_domain | Docs | stable | 
| **DNS** | oxlorg.opnsense.bind_record | Docs | stable | 
| **DNS** | oxlorg.opnsense.bind_record_multi | Docs | stable | 
| **Web Proxy** | oxlorg.opnsense.webproxy_general | Docs | stable | 
| **Web Proxy** | oxlorg.opnsense.webproxy_cache | Docs | stable | 
| **Web Proxy** | oxlorg.opnsense.webproxy_parent | Docs | stable | 
| **Web Proxy** | oxlorg.opnsense.webproxy_traffic | Docs | stable | 
| **Web Proxy** | oxlorg.opnsense.webproxy_forward | Docs | stable | 
| **Web Proxy** | oxlorg.opnsense.webproxy_acl | Docs | stable | 
| **Web Proxy** | oxlorg.opnsense.webproxy_icap | Docs | stable | 
| **Web Proxy** | oxlorg.opnsense.webproxy_auth | Docs | stable | 
| **Web Proxy** | oxlorg.opnsense.webproxy_remote_acl | Docs | stable | 
| **Web Proxy** | oxlorg.opnsense.webproxy_pac_proxy | Docs | stable | 
| **Web Proxy** | oxlorg.opnsense.webproxy_pac_match | Docs | stable | 
| **Web Proxy** | oxlorg.opnsense.webproxy_pac_rule | Docs | stable | 
| **IDS/IPS** | oxlorg.opnsense.ids_action | Docs | stable | 
| **IDS/IPS** | oxlorg.opnsense.ids_general | Docs | stable | 
| **IDS/IPS** | oxlorg.opnsense.ids_ruleset | Docs | stable | 
| **IDS/IPS** | oxlorg.opnsense.ids_rule | Docs | stable | 
| **IDS/IPS** | oxlorg.opnsense.ids_user_rule | Docs | stable | 
| **IDS/IPS** | oxlorg.opnsense.ids_policy | Docs | stable | 
| **IDS/IPS** | oxlorg.opnsense.ids_policy_rule | Docs | stable | 
| **OpenVPN** | oxlorg.opnsense.openvpn_client | Docs | stable | 
| **OpenVPN** | oxlorg.opnsense.openvpn_server | Docs | stable | 
| **OpenVPN** | oxlorg.opnsense.openvpn_static_key | Docs | stable | 
| **OpenVPN** | oxlorg.opnsense.openvpn_status | Docs | stable | 
| **OpenVPN** | oxlorg.opnsense.openvpn_client_override | Docs | stable | 
| **Nginx** | oxlorg.opnsense.nginx_general | Docs | stable | 
| **Nginx** | oxlorg.opnsense.nginx_upstream_server | Docs | stable | 
| **DHCP Relay** | oxlorg.opnsense.dhcrelay_relay | Docs | stable | 
| **DHCP Relay** | oxlorg.opnsense.dhcrelay_destination | Docs | stable | 
| **DHCP** | oxlorg.opnsense.dhcp_general | Docs | stable | 
| **DHCP Subnet** | oxlorg.opnsense.dhcp_subnet | Docs | stable | 
| **DHCP Reservation** | oxlorg.opnsense.dhcp_reservation | Docs | stable | 
| **DHCP Controlagent** | oxlorg.opnsense.dhcp_controlagent | Docs | stable | 
| **ACME (Certificates)** | oxlorg.opnsense.acme_account | Docs | stable | 
| **ACME (Certificates)** | oxlorg.opnsense.acme_action | Docs | stable | 
| **ACME (Certificates)** | oxlorg.opnsense.acme_general | Docs | stable | 
| **ACME (Certificates)** | oxlorg.opnsense.acme_validation | Docs | stable | 
| **ACME (Certificates)** | oxlorg.opnsense.acme_certificate | Docs | stable | 
| **Postfix** | oxlorg.opnsense.postfix_general | Docs | stable | 
| **Postfix** | oxlorg.opnsense.postfix_domain | Docs | stable | 
| **Postfix** | oxlorg.opnsense.postfix_recipient | Docs | stable | 
| **Postfix** | oxlorg.opnsense.postfix_recipientbcc | Docs | stable | 
| **Postfix** | oxlorg.opnsense.postfix_sender | Docs | stable | 
| **Postfix** | oxlorg.opnsense.postfix_senderbcc | Docs | stable | 
| **Postfix** | oxlorg.opnsense.postfix_sendercanonical | Docs | stable | 
| **Postfix** | oxlorg.opnsense.postfix_headercheck | Docs | stable | 
| **Postfix** | oxlorg.opnsense.postfix_address | Docs | stable | 
| **Snapshot** | oxlorg.opnsense.snapshot | Docs | stable | 
| **High Availability** | oxlorg.opnsense.hasync_general | Docs | stable | 
| **High Availability** | oxlorg.opnsense.hasync_service | Docs | stable | 
| **User Management** | oxlorg.opnsense.user | Docs | unstable | 
| **User Management** | oxlorg.opnsense.group | Docs | unstable | 
| **User Management** | oxlorg.opnsense.privilege | Docs | unstable | 
| **Neighbor** | oxlorg.opnsense.neighbor | Docs | unstable | 
| **Dnsmasq** | oxlorg.opnsense.dnsmasq_general | Docs | unstable | 
| **Dnsmasq** | oxlorg.opnsense.dnsmasq_domain | Docs | unstable | 
| **Dnsmasq** | oxlorg.opnsense.dnsmasq_host | Docs | unstable | 
| **Dnsmasq** | oxlorg.opnsense.dnsmasq_range | Docs | unstable | 
| **Dnsmasq** | oxlorg.opnsense.dnsmasq_option | Docs | unstable | 
| **Dnsmasq** | oxlorg.opnsense.dnsmasq_boot | Docs | unstable | 
| **Dnsmasq** | oxlorg.opnsense.dnsmasq_tag | Docs | unstable | 
| **HAProxy** | oxlorg.opnsense.haproxy_general_cache | Docs | unstable | 
| **HAProxy** | oxlorg.opnsense.haproxy_general_defaults | Docs | unstable | 
| **HAProxy** | oxlorg.opnsense.haproxy_general_logging | Docs | unstable | 
| **HAProxy** | oxlorg.opnsense.haproxy_general_peers | Docs | unstable | 
| **HAProxy** | oxlorg.opnsense.haproxy_general_settings | Docs | unstable | 
| **HAProxy** | oxlorg.opnsense.haproxy_general_stats | Docs | unstable | 
| **HAProxy** | oxlorg.opnsense.haproxy_general_tuning | Docs | unstable | 
| **HAProxy** | oxlorg.opnsense.haproxy_cpu | Docs | unstable | 
| **HAProxy** | oxlorg.opnsense.haproxy_user | Docs | unstable | 
| **HAProxy** | oxlorg.opnsense.haproxy_group | Docs | unstable | 
| **HAProxy** | oxlorg.opnsense.haproxy_maintenance | Docs | unstable | 
| **HAProxy** | oxlorg.opnsense.haproxy_acl | Docs | unstable | 
| **HAProxy** | oxlorg.opnsense.haproxy_lua | Docs | unstable | 
| **HAProxy** | oxlorg.opnsense.haproxy_action | Docs | unstable | 
| **HAProxy** | oxlorg.opnsense.haproxy_errorfile | Docs | unstable | 
| **HAProxy** | oxlorg.opnsense.haproxy_fcgi | Docs | unstable | 

New features can currently only be supplied as Pull Requests which have to follow the Contribution Guidelines & Developer Docs.

As there are very few maintainer-resources available for this project, it might take a long time for responses.
