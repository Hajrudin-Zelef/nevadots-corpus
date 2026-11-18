config firewall policy
config firewall policy
Configure IPv4/IPv6 policies.
config firewall policy
    Description: Configure IPv4/IPv6 policies.
    edit <policyid>
        set action [accept|deny|...]
        set anti-replay [enable|disable]
        set application-list {string}
        set auth-cert {string}
        set auth-path [enable|disable]
        set auth-redirect-addr {string}
        set auto-asic-offload [enable|disable]
        set av-profile {string}
        set block-notification [enable|disable]
        set captive-portal-exempt [enable|disable]
        set capture-packet [enable|disable]
        set casb-profile {string}
        set cifs-profile {string}
        set comments {var-string}
        set custom-log-fields <field-id1>, <field-id2>, ...
        set decrypted-traffic-mirror {string}
        set delay-tcp-npu-session [enable|disable]
        set diameter-filter-profile {string}
        set diffserv-copy [enable|disable]
        set diffserv-forward [enable|disable]
        set diffserv-reverse [enable|disable]
        set diffservcode-forward {user}
        set diffservcode-rev {user}
        set disclaimer [enable|disable]
        set dlp-profile {string}
        set dnsfilter-profile {string}
        set dsri [enable|disable]
        set dstaddr <name1>, <name2>, ...
        set dstaddr-negate [enable|disable]
        set dstaddr6 <name1>, <name2>, ...
        set dstaddr6-negate [enable|disable]
        set dstintf <name1>, <name2>, ...
        set dynamic-shaping [enable|disable]
        set email-collect [enable|disable]
        set emailfilter-profile {string}
        set fec [enable|disable]
        set file-filter-profile {string}
        set firewall-session-dirty [check-all|check-new]
        set fixedport [enable|disable]
        set fsso-agent-for-ntlm {string}
        set fsso-groups <name1>, <name2>, ...
        set geoip-anycast [enable|disable]
        set geoip-match [physical-location|registered-location]
        set groups <name1>, <name2>, ...
        set http-policy-redirect [enable|disable]
        set icap-profile {string}
        set identity-based-route {string}
        set inbound [enable|disable]
        set inspection-mode [proxy|flow]
        set internet-service [enable|disable]
        set internet-service-custom <name1>, <name2>, ...
        set internet-service-custom-group <name1>, <name2>, ...
        set internet-service-group <name1>, <name2>, ...
        set internet-service-name <name1>, <name2>, ...
        set internet-service-negate [enable|disable]
        set internet-service-src [enable|disable]
        set internet-service-src-custom <name1>, <name2>, ...
        set internet-service-src-custom-group <name1>, <name2>, ...
        set internet-service-src-group <name1>, <name2>, ...
        set internet-service-src-name <name1>, <name2>, ...
        set internet-service-src-negate [enable|disable]
        set internet-service6 [enable|disable]
        set internet-service6-custom <name1>, <name2>, ...
        set internet-service6-custom-group <name1>, <name2>, ...
        set internet-service6-group <name1>, <name2>, ...
        set internet-service6-name <name1>, <name2>, ...
        set internet-service6-negate [enable|disable]
        set internet-service6-src [enable|disable]
        set internet-service6-src-custom <name1>, <name2>, ...
        set internet-service6-src-custom-group <name1>, <name2>, ...
        set internet-service6-src-group <name1>, <name2>, ...
        set internet-service6-src-name <name1>, <name2>, ...
        set internet-service6-src-negate [enable|disable]
        set ippool [enable|disable]
        set ips-sensor {string}
        set ips-voip-filter {string}
        set logtraffic [all|utm|...]
        set logtraffic-start [enable|disable]
        set match-vip [enable|disable]
        set match-vip-only [enable|disable]
        set name {string}
        set nat [enable|disable]
        set nat46 [enable|disable]
        set nat64 [enable|disable]
        set natinbound [enable|disable]
        set natip {ipv4-classnet}
        set natoutbound [enable|disable]
        set network-service-dynamic <name1>, <name2>, ...
        set network-service-src-dynamic <name1>, <name2>, ...
        set np-acceleration [enable|disable]
        set ntlm [enable|disable]
        set ntlm-enabled-browsers <user-agent-string1>, <user-agent-string2>, ...
        set ntlm-guest [enable|disable]
        set outbound [enable|disable]
        set passive-wan-health-measurement [enable|disable]
        set pcp-inbound [enable|disable]
        set pcp-outbound [enable|disable]
        set pcp-poolname <name1>, <name2>, ...
        set per-ip-shaper {string}
        set permit-any-host [enable|disable]
        set permit-stun-host [enable|disable]
        set policy-expiry [enable|disable]
        set policy-expiry-date {datetime}
        set policy-expiry-date-utc {user}
        set poolname <name1>, <name2>, ...
        set poolname6 <name1>, <name2>, ...
        set port-preserve [enable|disable]
        set profile-group {string}
        set profile-protocol-options {string}
        set profile-type [single|group]
        set radius-mac-auth-bypass [enable|disable]
        set redirect-url {var-string}
        set replacemsg-override-group {string}
        set reputation-direction [source|destination]
        set reputation-direction6 [source|destination]
        set reputation-minimum {integer}
        set reputation-minimum6 {integer}
        set rtp-addr <name1>, <name2>, ...
        set rtp-nat [disable|enable]
        set schedule {string}
        set schedule-timeout [enable|disable]
        set sctp-filter-profile {string}
        set send-deny-packet [disable|enable]
        set service <name1>, <name2>, ...
        set service-negate [enable|disable]
        set session-ttl {user}
        set sgt <id1>, <id2>, ...
        set sgt-check [enable|disable]
        set src-vendor-mac <id1>, <id2>, ...
        set srcaddr <name1>, <name2>, ...
        set srcaddr-negate [enable|disable]
        set srcaddr6 <name1>, <name2>, ...
        set srcaddr6-negate [enable|disable]
        set srcintf <name1>, <name2>, ...
        set ssh-filter-profile {string}
        set ssh-policy-redirect [enable|disable]
        set ssl-ssh-profile {string}
        set status [enable|disable]
        set tcp-mss-receiver {integer}
        set tcp-mss-sender {integer}
        set tcp-session-without-syn [all|data-only|...]
        set timeout-send-rst [enable|disable]
        set tos {user}
        set tos-mask {user}
        set tos-negate [enable|disable]
        set traffic-shaper {string}
        set traffic-shaper-reverse {string}
        set users <name1>, <name2>, ...
        set utm-status [enable|disable]
        set uuid {uuid}
        set videofilter-profile {string}
        set virtual-patch-profile {string}
        set vlan-cos-fwd {integer}
        set vlan-cos-rev {integer}
        set vlan-filter {user}
        set voip-profile {string}
        set vpntunnel {string}
        set waf-profile {string}
        set wanopt [enable|disable]
        set wanopt-detection [active|passive|...]
        set wanopt-passive-opt [default|transparent|...]
        set wanopt-peer {string}
        set wanopt-profile {string}
        set wccp [enable|disable]
        set webcache [enable|disable]
        set webcache-https [disable|enable]
        set webfilter-profile {string}
        set webproxy-forward-server {string}
        set webproxy-profile {string}
        set ztna-device-ownership [enable|disable]
        set ztna-ems-tag <name1>, <name2>, ...
        set ztna-ems-tag-secondary <name1>, <name2>, ...
        set ztna-geo-tag <name1>, <name2>, ...
        set ztna-policy-redirect [enable|disable]
        set ztna-status [enable|disable]
        set ztna-tags-match-logic [or|and]
    next
end
                                            config firewall policy
| Parameter | Description | Type | Size | Default | 
|---|---|---|---|---|
| action | Policy action (accept/deny/ipsec). | option | - | deny | 
|  |  |  |  |  | 
| anti-replay | Enable/disable anti-replay check. | option | - | enable | 
|  |  |  |  |  | 
| application-list | Name of an existing Application list. | string | Maximum length: 35 |  | 
| auth-cert | HTTPS server certificate for policy authentication. | string | Maximum length: 35 |  | 
| auth-path | Enable/disable authentication-based routing. | option | - | disable | 
|  |  |  |  |  | 
| auth-redirect-addr | HTTP-to-HTTPS redirect address for firewall authentication. | string | Maximum length: 63 |  | 
| auto-asic-offload * | Enable/disable policy traffic ASIC offloading. | option | - | enable | 
|  |  |  |  |  | 
| av-profile | Name of an existing Antivirus profile. | string | Maximum length: 35 |  | 
| block-notification | Enable/disable block notification. | option | - | disable | 
|  |  |  |  |  | 
| captive-portal-exempt | Enable to exempt some users from the captive portal. | option | - | disable | 
|  |  |  |  |  | 
| capture-packet * | Enable/disable capture packets. | option | - | disable | 
|  |  |  |  |  | 
| casb-profile | Name of an existing CASB profile. | string | Maximum length: 35 |  | 
| cifs-profile | Name of an existing CIFS profile. | string | Maximum length: 35 |  | 
| comments | Comment. | var-string | Maximum length: 1023 |  | 
| custom-log-fields <field-id> | Custom fields to append to log messages for this policy. Custom log field. | string | Maximum length: 35 |  | 
| decrypted-traffic-mirror | Decrypted traffic mirror. | string | Maximum length: 35 |  | 
| delay-tcp-npu-session | Enable TCP NPU session delay to guarantee packet order of 3-way handshake. | option | - | disable | 
|  |  |  |  |  | 
| diameter-filter-profile | Name of an existing Diameter filter profile. | string | Maximum length: 35 |  | 
| diffserv-copy | Enable to copy packet's DiffServ values from session's original direction to its reply direction. | option | - | disable | 
|  |  |  |  |  | 
| diffserv-forward | Enable to change packet's DiffServ values to the specified diffservcode-forward value. | option | - | disable | 
|  |  |  |  |  | 
| diffserv-reverse | Enable to change packet's reverse (reply) DiffServ values to the specified diffservcode-rev value. | option | - | disable | 
|  |  |  |  |  | 
| diffservcode-forward | Change packet's DiffServ to this value. | user | Not Specified |  | 
| diffservcode-rev | Change packet's reverse (reply) DiffServ to this value. | user | Not Specified |  | 
| disclaimer | Enable/disable user authentication disclaimer. | option | - | disable | 
|  |  |  |  |  | 
| dlp-profile | Name of an existing DLP profile. | string | Maximum length: 35 |  | 
| dnsfilter-profile | Name of an existing DNS filter profile. | string | Maximum length: 35 |  | 
| dsri | Enable DSRI to ignore HTTP server responses. | option | - | disable | 
|  |  |  |  |  | 
| dstaddr <name> | Destination IPv4 address and address group names. Address name. | string | Maximum length: 79 |  | 
| dstaddr-negate | When enabled dstaddr specifies what the destination address must NOT be. | option | - | disable | 
|  |  |  |  |  | 
| dstaddr6 <name> | Destination IPv6 address name and address group names. Address name. | string | Maximum length: 79 |  | 
| dstaddr6-negate | When enabled dstaddr6 specifies what the destination address must NOT be. | option | - | disable | 
|  |  |  |  |  | 
| dstintf <name> | Outgoing (egress) interface. Interface name. | string | Maximum length: 79 |  | 
| dynamic-shaping | Enable/disable dynamic RADIUS defined traffic shaping. | option | - | disable | 
|  |  |  |  |  | 
| email-collect | Enable/disable email collection. | option | - | disable | 
|  |  |  |  |  | 
| emailfilter-profile | Name of an existing email filter profile. | string | Maximum length: 35 |  | 
| fec | Enable/disable Forward Error Correction on traffic matching this policy on a FEC device. | option | - | disable | 
|  |  |  |  |  | 
| file-filter-profile | Name of an existing file-filter profile. | string | Maximum length: 35 |  | 
| firewall-session-dirty | How to handle sessions if the configuration of this firewall policy changes. | option | - | check-all | 
|  |  |  |  |  | 
| fixedport | Enable to prevent source NAT from changing a session's source port. | option | - | disable | 
|  |  |  |  |  | 
| fsso-agent-for-ntlm | FSSO agent to use for NTLM authentication. | string | Maximum length: 35 |  | 
| fsso-groups <name> | Names of FSSO groups. Names of FSSO groups. | string | Maximum length: 511 |  | 
| geoip-anycast | Enable/disable recognition of anycast IP addresses using the geography IP database. | option | - | disable | 
|  |  |  |  |  | 
| geoip-match | Match geography address based either on its physical location or registered location. | option | - | physical-location | 
|  |  |  |  |  | 
| groups <name> | Names of user groups that can authenticate with this policy. Group name. | string | Maximum length: 79 |  | 
| http-policy-redirect | Redirect HTTP(S) traffic to matching transparent web proxy policy. | option | - | disable | 
|  |  |  |  |  | 
| icap-profile | Name of an existing ICAP profile. | string | Maximum length: 35 |  | 
| identity-based-route | Name of identity-based routing rule. | string | Maximum length: 35 |  | 
| inbound | Policy-based IPsec VPN: only traffic from the remote network can initiate a VPN. | option | - | disable | 
|  |  |  |  |  | 
| inspection-mode | Policy inspection mode (Flow/proxy). Default is Flow mode. | option | - | flow | 
|  |  |  |  |  | 
| internet-service | Enable/disable use of Internet Services for this policy. If enabled, destination address and service are not used. | option | - | disable | 
|  |  |  |  |  | 
| internet-service-custom <name> | Custom Internet Service name. Custom Internet Service name. | string | Maximum length: 79 |  | 
| internet-service-custom-group <name> | Custom Internet Service group name. Custom Internet Service group name. | string | Maximum length: 79 |  | 
| internet-service-group <name> | Internet Service group name. Internet Service group name. | string | Maximum length: 79 |  | 
| internet-service-name <name> | Internet Service name. Internet Service name. | string | Maximum length: 79 |  | 
| internet-service-negate | When enabled internet-service specifies what the service must NOT be. | option | - | disable | 
|  |  |  |  |  | 
| internet-service-src | Enable/disable use of Internet Services in source for this policy. If enabled, source address is not used. | option | - | disable | 
|  |  |  |  |  | 
| internet-service-src-custom <name> | Custom Internet Service source name. Custom Internet Service name. | string | Maximum length: 79 |  | 
| internet-service-src-custom-group <name> | Custom Internet Service source group name. Custom Internet Service group name. | string | Maximum length: 79 |  | 
| internet-service-src-group <name> | Internet Service source group name. Internet Service group name. | string | Maximum length: 79 |  | 
| internet-service-src-name <name> | Internet Service source name. Internet Service name. | string | Maximum length: 79 |  | 
| internet-service-src-negate | When enabled internet-service-src specifies what the service must NOT be. | option | - | disable | 
|  |  |  |  |  | 
| internet-service6 | Enable/disable use of IPv6 Internet Services for this policy. If enabled, destination address and service are not used. | option | - | disable | 
|  |  |  |  |  | 
| internet-service6-custom <name> | Custom IPv6 Internet Service name. Custom Internet Service name. | string | Maximum length: 79 |  | 
| internet-service6-custom-group <name> | Custom Internet Service6 group name. Custom Internet Service6 group name. | string | Maximum length: 79 |  | 
| internet-service6-group <name> | Internet Service group name. Internet Service group name. | string | Maximum length: 79 |  | 
| internet-service6-name <name> | IPv6 Internet Service name. IPv6 Internet Service name. | string | Maximum length: 79 |  | 
| internet-service6-negate | When enabled internet-service6 specifies what the service must NOT be. | option | - | disable | 
|  |  |  |  |  | 
| internet-service6-src | Enable/disable use of IPv6 Internet Services in source for this policy. If enabled, source address is not used. | option | - | disable | 
|  |  |  |  |  | 
| internet-service6-src-custom <name> | Custom IPv6 Internet Service source name. Custom Internet Service name. | string | Maximum length: 79 |  | 
| internet-service6-src-custom-group <name> | Custom Internet Service6 source group name. Custom Internet Service6 group name. | string | Maximum length: 79 |  | 
| internet-service6-src-group <name> | Internet Service6 source group name. Internet Service group name. | string | Maximum length: 79 |  | 
| internet-service6-src-name <name> | IPv6 Internet Service source name. Internet Service name. | string | Maximum length: 79 |  | 
| internet-service6-src-negate | When enabled internet-service6-src specifies what the service must NOT be. | option | - | disable | 
|  |  |  |  |  | 
| ippool | Enable to use IP Pools for source NAT. | option | - | disable | 
|  |  |  |  |  | 
| ips-sensor | Name of an existing IPS sensor. | string | Maximum length: 35 |  | 
| ips-voip-filter | Name of an existing VoIP (ips) profile. | string | Maximum length: 35 |  | 
| logtraffic | Enable or disable logging. Log all sessions or security profile sessions. | option | - | utm | 
|  |  |  |  |  | 
| logtraffic-start | Record logs when a session starts. | option | - | disable | 
|  |  |  |  |  | 
| match-vip | Enable to match packets that have had their destination addresses changed by a VIP. | option | - | enable | 
|  |  |  |  |  | 
| match-vip-only | Enable/disable matching of only those packets that have had their destination addresses changed by a VIP. | option | - | disable | 
|  |  |  |  |  | 
| name | Policy name. | string | Maximum length: 35 |  | 
| nat | Enable/disable source NAT. | option | - | disable | 
|  |  |  |  |  | 
| nat46 | Enable/disable NAT46. | option | - | disable | 
|  |  |  |  |  | 
| nat64 | Enable/disable NAT64. | option | - | disable | 
|  |  |  |  |  | 
| natinbound | Policy-based IPsec VPN: apply destination NAT to inbound traffic. | option | - | disable | 
|  |  |  |  |  | 
| natip | Policy-based IPsec VPN: source NAT IP address for outgoing traffic. | ipv4-classnet | Not Specified | 0.0.0.0 0.0.0.0 | 
| natoutbound | Policy-based IPsec VPN: apply source NAT to outbound traffic. | option | - | disable | 
|  |  |  |  |  | 
| network-service-dynamic <name> | Dynamic Network Service name. Dynamic Network Service name. | string | Maximum length: 79 |  | 
| network-service-src-dynamic <name> | Dynamic Network Service source name. Dynamic Network Service name. | string | Maximum length: 79 |  | 
| np-acceleration * | Enable/disable UTM Network Processor acceleration. | option | - | enable | 
|  |  |  |  |  | 
| ntlm | Enable/disable NTLM authentication. | option | - | disable | 
|  |  |  |  |  | 
| ntlm-enabled-browsers <user-agent-string> | HTTP-User-Agent value of supported browsers. User agent string. | string | Maximum length: 79 |  | 
| ntlm-guest | Enable/disable NTLM guest user access. | option | - | disable | 
|  |  |  |  |  | 
| outbound | Policy-based IPsec VPN: only traffic from the internal network can initiate a VPN. | option | - | enable | 
|  |  |  |  |  | 
| passive-wan-health-measurement | Enable/disable passive WAN health measurement. When enabled, auto-asic-offload is disabled. | option | - | disable | 
|  |  |  |  |  | 
| pcp-inbound | Enable/disable PCP inbound DNAT. | option | - | disable | 
|  |  |  |  |  | 
| pcp-outbound | Enable/disable PCP outbound SNAT. | option | - | disable | 
|  |  |  |  |  | 
| pcp-poolname <name> | PCP pool names. PCP pool name. | string | Maximum length: 79 |  | 
| per-ip-shaper | Per-IP traffic shaper. | string | Maximum length: 35 |  | 
| permit-any-host | Accept UDP packets from any host. | option | - | disable | 
|  |  |  |  |  | 
| permit-stun-host | Accept UDP packets from any Session Traversal Utilities for NAT (STUN) host. | option | - | disable | 
|  |  |  |  |  | 
| policy-expiry | Enable/disable policy expiry. | option | - | disable | 
|  |  |  |  |  | 
| policy-expiry-date | Policy expiry date (YYYY-MM-DD HH:MM:SS). | datetime | Not Specified | 0000-00-00 00:00:00 | 
| policy-expiry-date-utc | Policy expiry date and time, in epoch format. | user | Not Specified |  | 
| policyid | Policy ID (0 - 4294967294). | integer | Minimum value: 0 Maximum value: 4294967294 | 0 | 
| poolname <name> | IP Pool names. IP pool name. | string | Maximum length: 79 |  | 
| poolname6 <name> | IPv6 pool names. IPv6 pool name. | string | Maximum length: 79 |  | 
| port-preserve | Enable/disable preservation of the original source port from source NAT if it has not been used. | option | - | enable | 
|  |  |  |  |  | 
| profile-group | Name of profile group. | string | Maximum length: 35 |  | 
| profile-protocol-options | Name of an existing Protocol options profile. | string | Maximum length: 35 | default | 
| profile-type | Determine whether the firewall policy allows security profile groups or single profiles only. | option | - | single | 
|  |  |  |  |  | 
| radius-mac-auth-bypass | Enable MAC authentication bypass. The bypassed MAC address must be received from RADIUS server. | option | - | disable | 
|  |  |  |  |  | 
| redirect-url | URL users are directed to after seeing and accepting the disclaimer or authenticating. | var-string | Maximum length: 1023 |  | 
| replacemsg-override-group | Override the default replacement message group for this policy. | string | Maximum length: 35 |  | 
| reputation-direction | Direction of the initial traffic for reputation to take effect. | option | - | destination | 
|  |  |  |  |  | 
| reputation-direction6 | Direction of the initial traffic for IPv6 reputation to take effect. | option | - | destination | 
|  |  |  |  |  | 
| reputation-minimum | Minimum Reputation to take action. | integer | Minimum value: 0 Maximum value: 4294967295 | 0 | 
| reputation-minimum6 | IPv6 Minimum Reputation to take action. | integer | Minimum value: 0 Maximum value: 4294967295 | 0 | 
| rtp-addr <name> | Address names if this is an RTP NAT policy. Address name. | string | Maximum length: 79 |  | 
| rtp-nat | Enable Real Time Protocol (RTP) NAT. | option | - | disable | 
|  |  |  |  |  | 
| schedule | Schedule name. | string | Maximum length: 35 |  | 
| schedule-timeout | Enable to force current sessions to end when the schedule object times out. Disable allows them to end from inactivity. | option | - | disable | 
|  |  |  |  |  | 
| sctp-filter-profile | Name of an existing SCTP filter profile. | string | Maximum length: 35 |  | 
| send-deny-packet | Enable to send a reply when a session is denied or blocked by a firewall policy. | option | - | disable | 
|  |  |  |  |  | 
| service <name> | Service and service group names. Service and service group names. | string | Maximum length: 79 |  | 
| service-negate | When enabled service specifies what the service must NOT be. | option | - | disable | 
|  |  |  |  |  | 
| session-ttl | TTL in seconds for sessions accepted by this policy (0 means use the system default session TTL). | user | Not Specified |  | 
| sgt <id> | Security group tags. Security group tag (1 - 65535). | integer | Minimum value: 1 Maximum value: 65535 |  | 
| sgt-check | Enable/disable security group tags (SGT) check. | option | - | disable | 
|  |  |  |  |  | 
| src-vendor-mac <id> | Vendor MAC source ID. Vendor MAC ID. | integer | Minimum value: 0 Maximum value: 4294967295 |  | 
| srcaddr <name> | Source IPv4 address and address group names. Address name. | string | Maximum length: 79 |  | 
| srcaddr-negate | When enabled srcaddr specifies what the source address must NOT be. | option | - | disable | 
|  |  |  |  |  | 
| srcaddr6 <name> | Source IPv6 address name and address group names. Address name. | string | Maximum length: 79 |  | 
| srcaddr6-negate | When enabled srcaddr6 specifies what the source address must NOT be. | option | - | disable | 
|  |  |  |  |  | 
| srcintf <name> | Incoming (ingress) interface. Interface name. | string | Maximum length: 79 |  | 
| ssh-filter-profile | Name of an existing SSH filter profile. | string | Maximum length: 35 |  | 
| ssh-policy-redirect | Redirect SSH traffic to matching transparent proxy policy. | option | - | disable | 
|  |  |  |  |  | 
| ssl-ssh-profile | Name of an existing SSL SSH profile. | string | Maximum length: 35 | no-inspection | 
| status | Enable or disable this policy. | option | - | enable | 
|  |  |  |  |  | 
| tcp-mss-receiver | Receiver TCP maximum segment size (MSS). | integer | Minimum value: 0 Maximum value: 65535 | 0 | 
| tcp-mss-sender | Sender TCP maximum segment size (MSS). | integer | Minimum value: 0 Maximum value: 65535 | 0 | 
| tcp-session-without-syn | Enable/disable creation of TCP session without SYN flag. | option | - | disable | 
|  |  |  |  |  | 
| timeout-send-rst | Enable/disable sending RST packets when TCP sessions expire. | option | - | disable | 
|  |  |  |  |  | 
| tos | ToS (Type of Service) value used for comparison. | user | Not Specified |  | 
| tos-mask | Non-zero bit positions are used for comparison while zero bit positions are ignored. | user | Not Specified |  | 
| tos-negate | Enable negated TOS match. | option | - | disable | 
|  |  |  |  |  | 
| traffic-shaper | Traffic shaper. | string | Maximum length: 35 |  | 
| traffic-shaper-reverse | Reverse traffic shaper. | string | Maximum length: 35 |  | 
| users <name> | Names of individual users that can authenticate with this policy. Names of individual users that can authenticate with this policy. | string | Maximum length: 79 |  | 
| utm-status | Enable to add one or more security profiles (AV, IPS, etc.) to the firewall policy. | option | - | disable | 
|  |  |  |  |  | 
| uuid | Universally Unique Identifier (UUID; automatically assigned but can be manually reset). | uuid | Not Specified | 00000000-0000-0000-0000-000000000000 | 
| videofilter-profile | Name of an existing VideoFilter profile. | string | Maximum length: 35 |  | 
| virtual-patch-profile | Name of an existing virtual-patch profile. | string | Maximum length: 35 |  | 
| vlan-cos-fwd | VLAN forward direction user priority: 255 passthrough, 0 lowest, 7 highest. | integer | Minimum value: 0 Maximum value: 7 | 255 | 
| vlan-cos-rev | VLAN reverse direction user priority: 255 passthrough, 0 lowest, 7 highest. | integer | Minimum value: 0 Maximum value: 7 | 255 | 
| vlan-filter | VLAN ranges to allow | user | Not Specified |  | 
| voip-profile | Name of an existing VoIP (voipd) profile. | string | Maximum length: 35 |  | 
| vpntunnel | Policy-based IPsec VPN: name of the IPsec VPN Phase 1. | string | Maximum length: 35 |  | 
| waf-profile | Name of an existing Web application firewall profile. | string | Maximum length: 35 |  | 
| wanopt * | Enable/disable WAN optimization. | option | - | disable | 
|  |  |  |  |  | 
| wanopt-detection * | WAN optimization auto-detection mode. | option | - | active | 
|  |  |  |  |  | 
| wanopt-passive-opt * | WAN optimization passive mode options. This option decides what IP address will be used to connect server. | option | - | default | 
|  |  |  |  |  | 
| wanopt-peer * | WAN optimization peer. | string | Maximum length: 35 |  | 
| wanopt-profile * | WAN optimization profile. | string | Maximum length: 35 |  | 
| wccp | Enable/disable forwarding traffic matching this policy to a configured WCCP server. | option | - | disable | 
|  |  |  |  |  | 
| webcache * | Enable/disable web cache. | option | - | disable | 
|  |  |  |  |  | 
| webcache-https * | Enable/disable web cache for HTTPS. | option | - | disable | 
|  |  |  |  |  | 
| webfilter-profile | Name of an existing Web filter profile. | string | Maximum length: 35 |  | 
| webproxy-forward-server | Webproxy forward server name. | string | Maximum length: 63 |  | 
| webproxy-profile | Webproxy profile name. | string | Maximum length: 63 |  | 
| ztna-device-ownership | Enable/disable zero trust device ownership. | option | - | disable | 
|  |  |  |  |  | 
| ztna-ems-tag <name> | Source ztna-ems-tag names. Address name. | string | Maximum length: 79 |  | 
| ztna-ems-tag-secondary <name> | Source ztna-ems-tag-secondary names. Address name. | string | Maximum length: 79 |  | 
| ztna-geo-tag <name> | Source ztna-geo-tag names. Address name. | string | Maximum length: 79 |  | 
| ztna-policy-redirect | Redirect ZTNA traffic to matching Access-Proxy proxy-policy. | option | - | disable | 
|  |  |  |  |  | 
| ztna-status | Enable/disable zero trust access. | option | - | disable | 
|  |  |  |  |  | 
| ztna-tags-match-logic | ZTNA tag matching logic. | option | - | or | 
|  |  |  |  |  | 
| Option | Description | 
|---|---|
| accept | Allows session that match the firewall policy. | 
| deny | Blocks sessions that match the firewall policy. | 
| ipsec | Firewall policy becomes a policy-based IPsec VPN policy. | 
| Option | Description | 
|---|---|
| enable | Enable anti-replay check. | 
| disable | Disable anti-replay check. | 
| Option | Description | 
|---|---|
| enable | Enable authentication-based routing. | 
| disable | Disable authentication-based routing. | 
| Option | Description | 
|---|---|
| enable | Enable auto ASIC offloading. | 
| disable | Disable ASIC offloading. | 
| Option | Description | 
|---|---|
| enable | Enable setting. | 
| disable | Disable setting. | 
| Option | Description | 
|---|---|
| enable | Enable exemption of captive portal. | 
| disable | Disable exemption of captive portal. | 
| Option | Description | 
|---|---|
| enable | Enable capture packets. | 
| disable | Disable capture packets. | 
| Option | Description | 
|---|---|
| enable | Enable TCP NPU session delay in order to guarantee packet order of 3-way handshake. | 
| disable | Disable TCP NPU session delay in order to guarantee packet order of 3-way handshake. | 
| Option | Description | 
|---|---|
| enable | Enable DSCP copy. | 
| disable | Disable DSCP copy. | 
| Option | Description | 
|---|---|
| enable | Enable setting forward (original) traffic Diffserv. | 
| disable | Disable setting forward (original) traffic Diffserv. | 
| Option | Description | 
|---|---|
| enable | Enable setting reverse (reply) traffic DiffServ. | 
| disable | Disable setting reverse (reply) traffic DiffServ. | 
| Option | Description | 
|---|---|
| enable | Enable user authentication disclaimer. | 
| disable | Disable user authentication disclaimer. | 
| Option | Description | 
|---|---|
| enable | Enable DSRI. | 
| disable | Disable DSRI. | 
| Option | Description | 
|---|---|
| enable | Enable destination address negate. | 
| disable | Disable destination address negate. | 
| Option | Description | 
|---|---|
| enable | Enable IPv6 destination address negate. | 
| disable | Disable IPv6 destination address negate. | 
| Option | Description | 
|---|---|
| enable | Enable dynamic RADIUS defined traffic shaping. | 
| disable | Disable dynamic RADIUS defined traffic shaping. | 
| Option | Description | 
|---|---|
| enable | Enable email collection. | 
| disable | Disable email collection. | 
| Option | Description | 
|---|---|
| enable | Enable Forward Error Correction. | 
| disable | Disable Forward Error Correction. | 
| Option | Description | 
|---|---|
| check-all | Flush all current sessions accepted by this policy. These sessions must be started and re-matched with policies. | 
| check-new | Continue to allow sessions already accepted by this policy. | 
| Option | Description | 
|---|---|
| enable | Enable setting. | 
| disable | Disable setting. | 
| Option | Description | 
|---|---|
| enable | Enable recognition of anycast IP addresses using the geography IP database. | 
| disable | Disable recognition of anycast IP addresses using the geography IP database. | 
| Option | Description | 
|---|---|
| physical-location | Match geography address to its physical location using the geography IP database. | 
| registered-location | Match geography address to its registered location using the geography IP database. | 
| Option | Description | 
|---|---|
| enable | Enable HTTP(S) policy redirect. | 
| disable | Disable HTTP(S) policy redirect. | 
| Option | Description | 
|---|---|
| enable | Enable setting. | 
| disable | Disable setting. | 
| Option | Description | 
|---|---|
| proxy | Proxy based inspection. | 
| flow | Flow based inspection. | 
| Option | Description | 
|---|---|
| enable | Enable use of Internet Services in policy. | 
| disable | Disable use of Internet Services in policy. | 
| Option | Description | 
|---|---|
| enable | Enable negated Internet Service match. | 
| disable | Disable negated Internet Service match. | 
| Option | Description | 
|---|---|
| enable | Enable use of Internet Services source in policy. | 
| disable | Disable use of Internet Services source in policy. | 
| Option | Description | 
|---|---|
| enable | Enable negated Internet Service source match. | 
| disable | Disable negated Internet Service source match. | 
| Option | Description | 
|---|---|
| enable | Enable use of IPv6 Internet Services in policy. | 
| disable | Disable use of IPv6 Internet Services in policy. | 
| Option | Description | 
|---|---|
| enable | Enable negated IPv6 Internet Service match. | 
| disable | Disable negated IPv6 Internet Service match. | 
| Option | Description | 
|---|---|
| enable | Enable use of IPv6 Internet Services source in policy. | 
| disable | Disable use of IPv6 Internet Services source in policy. | 
| Option | Description | 
|---|---|
| enable | Enable negated IPv6 Internet Service source match. | 
| disable | Disable negated IPv6 Internet Service source match. | 
| Option | Description | 
|---|---|
| enable | Enable setting. | 
| disable | Disable setting. | 
| Option | Description | 
|---|---|
| all | Log all sessions accepted or denied by this policy. | 
| utm | Log traffic that has a security profile applied to it. | 
| disable | Disable all logging for this policy. | 
| Option | Description | 
|---|---|
| enable | Enable setting. | 
| disable | Disable setting. | 
| Option | Description | 
|---|---|
| enable | Match DNATed packet. | 
| disable | Do not match DNATed packet. | 
| Option | Description | 
|---|---|
| enable | Enable matching of only those packets that have had their destination addresses changed by a VIP. | 
| disable | Disable matching of only those packets that have had their destination addresses changed by a VIP. | 
| Option | Description | 
|---|---|
| enable | Enable setting. | 
| disable | Disable setting. | 
| Option | Description | 
|---|---|
| enable | Enable NAT46. | 
| disable | Disable NAT46. | 
| Option | Description | 
|---|---|
| enable | Enable NAT64. | 
| disable | Disable NAT64. | 
| Option | Description | 
|---|---|
| enable | Enable setting. | 
| disable | Disable setting. | 
| Option | Description | 
|---|---|
| enable | Enable UTM Network Processor acceleration. | 
| disable | Disable UTM Network Processor acceleration. | 
| Option | Description | 
|---|---|
| enable | Enable setting. | 
| disable | Disable setting. | 
| Option | Description | 
|---|---|
| enable | Enable Passive WAN health measurement. | 
| disable | Disable Passive WAN health measurement. | 
| Option | Description | 
|---|---|
| enable | Enable PCP inbound DNAT. | 
| disable | Disable PCP inbound DNAT. | 
| Option | Description | 
|---|---|
| enable | Enable PCP outbound SNAT. | 
| disable | Disable PCP outbound SNAT. | 
| Option | Description | 
|---|---|
| enable | Enable setting. | 
| disable | Disable setting. | 
| Option | Description | 
|---|---|
| enable | Enable policy expiry. | 
| disable | Disable polcy expiry. | 
| Option | Description | 
|---|---|
| enable | Use the original source port if it has not been used. | 
| disable | Source NAT always changes the source port. | 
| Option | Description | 
|---|---|
| single | Do not allow security profile groups. | 
| group | Allow security profile groups. | 
| Option | Description | 
|---|---|
| enable | Enable MAC authentication bypass. | 
| disable | Disable MAC authentication bypass. | 
| Option | Description | 
|---|---|
| source | Check reputation for source address. | 
| destination | Check reputation for destination address. | 
| Option | Description | 
|---|---|
| source | Check reputation for IPv6 source address. | 
| destination | Check reputation for IPv6 destination address. | 
| Option | Description | 
|---|---|
| disable | Disable setting. | 
| enable | Enable setting. | 
| Option | Description | 
|---|---|
| enable | Enable schedule timeout. | 
| disable | Disable schedule timeout. | 
| Option | Description | 
|---|---|
| disable | Disable deny-packet sending. | 
| enable | Enable deny-packet sending. | 
| Option | Description | 
|---|---|
| enable | Enable negated service match. | 
| disable | Disable negated service match. | 
| Option | Description | 
|---|---|
| enable | Enable SGT check. | 
| disable | Disable SGT check. | 
| Option | Description | 
|---|---|
| enable | Enable source address negate. | 
| disable | Disable source address negate. | 
| Option | Description | 
|---|---|
| enable | Enable IPv6 source address negate. | 
| disable | Disable IPv6 source address negate. | 
| Option | Description | 
|---|---|
| enable | Enable SSH policy redirect. | 
| disable | Disable SSH policy redirect. | 
| Option | Description | 
|---|---|
| enable | Enable setting. | 
| disable | Disable setting. | 
| Option | Description | 
|---|---|
| all | Enable TCP session without SYN. | 
| data-only | Enable TCP session data only. | 
| disable | Disable TCP session without SYN. | 
| Option | Description | 
|---|---|
| enable | Enable sending of RST packet upon TCP session expiration. | 
| disable | Disable sending of RST packet upon TCP session expiration. | 
| Option | Description | 
|---|---|
| enable | Enable TOS match negate. | 
| disable | Disable TOS match negate. | 
| Option | Description | 
|---|---|
| enable | Enable setting. | 
| disable | Disable setting. | 
| Option | Description | 
|---|---|
| active | Active WAN optimization peer auto-detection. | 
| passive | Passive WAN optimization peer auto-detection. | 
| off | Turn off WAN optimization peer auto-detection. | 
| Option | Description | 
|---|---|
| default | Allow client side WAN opt peer to decide. | 
| transparent | Use address of client to connect to server. | 
| non-transparent | Use local FortiGate address to connect to server. | 
| Option | Description | 
|---|---|
| enable | Enable WCCP setting. | 
| disable | Disable WCCP setting. | 
| Option | Description | 
|---|---|
| enable | Enable setting. | 
| disable | Disable setting. | 
| Option | Description | 
|---|---|
| disable | Disable web cache for HTTPS. | 
| enable | Enable web cache for HTTPS. | 
| Option | Description | 
|---|---|
| enable | Enable ZTNA device ownership check. | 
| disable | Disable ZTNA device ownership check. | 
| Option | Description | 
|---|---|
| enable | Enable ZTNA proxy-policy redirect. | 
| disable | Disable ZTNA proxy-policy redirect. | 
| Option | Description | 
|---|---|
| enable | Enable zero trust network access. | 
| disable | Disable zero trust network access. | 
| Option | Description | 
|---|---|
| or | Match ZTNA tags using a logical OR operator. | 
| and | Match ZTNA tags using a logical AND operator. | 
* This parameter may not exist in some models.