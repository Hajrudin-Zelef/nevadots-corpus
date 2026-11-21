---
id: collect-261121-veille-ia/gemini-google/arborescence-des-pages-36
title: "Arborescence des pages"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["hotspot", "address", "default", "server", "name", "network", "clients", "interface"]
source: https://help.mikrotik.com/docs/spaces/ROS/pages/56459266/HotSpot
source_anchor: ""
source_lines: [1, 315]
section: "Gemini_Google"
---
# Introduction

The MikroTik HotSpot Gateway provides authentication for clients before access to public networks.

**Hotspot (captive portal)** - uses web-proxy and it is capable of using only the default routing table, at the moment. Making the PCC(per connection-classifier) not a valid method, due to the, multiple routing tables used.

Important

## **HotSpot Gateway features:**

- different authentication methods of clients, using a local client database on the router, or remote RADIUS server;
- users accounting in a local database on the router, or on remote RADIUS server;
- a walled-garden system, access to some web pages without authorization;
- login page modification, where you can put information about the company;
- automatic and transparent change any IP address of a client to a valid address;
- HotSpot can inform DHCP clients that they are behind a captive portal (RFC7710);

A hotspot can work reliably only when IPv4 is used. Hotspot relies on Firewall NAT rules which currently are not supported for IPv6.

# Example

Verify HotSpot configuration:

## **Parameters asked during the setup process**

| Parameter | Description | 
|---|---|
| **hotspot interface** (*string* ; Default:**allow** ) | Interface name on which to run HotSpot. To run HotSpot on a bridge interface, make sure public interfaces are not included in the bridge ports. | 
| **local address of network** (*IP* ; Default:**10.5.50.1/24** ) | HotSpot gateway address | 
| **masquerade network** (*yes \| no* ; Default:**yes** ) | Whether to masquerade HotSpot network, when **yes** rule is added to*/ip firewall nat* with*action=masquerade* | 
| **address pool of network** (*string* ; Default:**yes** ) | Address pool for HotSpot network, which is used to change user IP address to a valid address. Useful if providing network access to mobile clients that are not willing to change their networking settings. | 
| **select certificate** (*none \| import-other-certificate* ; Default: ) | Choose SSL certificate, when HTTPS authorization method is required. | 
| **ip address of smtp server** (*IP* ; Default:**0.0.0.0** ) | The IP address of the SMTP server, where to redirect HotSpot's network SMTP requests (25 TCP port) | 
| **dns servers** (*IP* ; Default:**0.0.0.0** ) | DNS server addresses used for HotSpot clients, configuration taken from */ip dns* menu of the HotSpot gateway | 
| **dns name** (*string* ; Default:**""** ) | the domain name of the HotSpot server, a full qualified domain name is required, for example, www.example.com | 
| **name of local hotspot user** (*string* ; Default:**"admin"** ) | username of one automatically created HotSpot user, added to */ip hotspot user* | 
| **password for the user'** (*string* ; Default: ) | Password for automatically created HotSpot user | 

# HotSpot

The menu is designed to manage the HotSpot servers of the router. It is possible to run HotSpot on Ethernet, wireless, VLAN, and bridge interfaces. One HotSpot server is allowed per interface. When HotSpot is configured on the bridge interface, set HotSpot interface as bridge interface, not as bridge port, do not add public interfaces to bridge ports. You can add HotSpot servers manually to the */ip/hotspot* menu, but it is advised to run */ip/hotspot/setup*, which adds all necessary settings.

| Parameters | Description | 
|---|---|
| **name** (text) | HotSpot server's name or identifier | 
| **address-pool** (name/none; default:*none* ) | address space used to change HotSpot client *any* IP address to a valid address. Useful for providing public network access to mobile clients that are not willing to change their networking settings | 
| **idle-timeout** (time/none; default:*5m* ) | period of inactivity for unauthorized clients. When there is no traffic from this client (literally client computer should be switched off), once the timeout is reached, a user is dropped from the HotSpot host list, its used address becomes available | 
| **keepalive-timeout** (time/none; default:*none* ) | Value of how long host can stay out of reach to be removed from the HotSpot | 
| **login-timeout** (time/none; default:*none* ) | Period of time after which if a host hasn't been authorized itself with a system the host entry gets deleted from host table. Loop repeats until the host logs in the system. Enable if there are situations where a host cannot log in after being too long in the host table unauthorized. | 
| **interface** (name of an interface) | Interface to run HotSpot on | 
| **addresses-per-mac** (integer**/** unlimited; default: 2) | Number of IP addresses allowed to be bind with the MAC address, when multiple HotSpot clients connected with one MAC-address | 
| **profile** (name; default:***default*)** | HotSpot server default HotSpot profile, which is located in */ip/hotspot/profile* | 

Read-only

| Parameters | Description | 
|---|---|
| keepalive-timeout (read-only; time) | The exact value of the keepalive-timeout, that is applied to the user. Value shows how long the host can stay out of reach to be removed from the HotSpot | 

# HotSpot Profile

This submenu contains list of Hotspot server profiles. There may be various different HotSpot systems, defined as Server Profiles, on the same gateway machine. One or more interfaces can be grouped into one server profile. There are very few settings for the servers on particular interfaces - most of the configuration is set in the server profiles. For example, it is possible to make completely different set of servlet pages for each server profile, and define different RADIUS servers for authentication.

| Property | Description | 
|---|---|
| **dns-name** (*string* ; Default:**""** ) | DNS name of the HotSpot server. This is the DNS name used as the name of the HotSpot server (i.e., it appears as the location of the login page). This name will automatically be added as a static DNS entry in the DNS cache. | 
| **hotspot-address** (*IP* ; Default:**0.0.0.0** ) | IP address of HotSpot service. | 
| **html-directory** (*string* ; Default:**hotspot** ) | Directory name in which HotSpot HTML pages are stored (by default *hotspot* directory). It is possible to specify different directory with modified HTML pages. To change HotSpot login page, get HotSpot files from your router, change and upload them back to same location. Full path must be typed in html-directory field, including "/flash/(hotspot_dir)" | 
| **html-directory-override** (*string* ; Default:**none** ) | Alternative path for hotspot html files. It should be used only when customized hotspot html files are stored on external storage. | 
| **http-cookie-lifetime** (*time* ; Default:**3d** ) | HTTP cookie validity time, the option is related to *cookie* HotSpot login method | 
| **http-proxy** (*IP:Port* ; Default:**0.0.0.0:0** ) | Address and port of the proxy server for HotSpot service, when default value is used all request are resolved by the local /ip proxy | 
| **https-redirect** (*yes \| no* ; Default:**yes** ) | Whether to redirect unauthenticated user to hotspot login page, if user is visiting a https:// url. Since certificate domain name will mismatch, often this leads to errors, so you can set this parameter to "no" and all https requests will simply be rejected and user will have to visit a http page. | 
| **login-by** (*cookie\|http-chap\|http-pap\|https\|mac\|trial\|mac-cookie* ; Default:**http-chap, cookie** ) | Used HotSpot authentication method  | 
| **mac-auth-password** (*string* ; Default: ) | Used together with MAC authentication, field used to specify password for the users to be authenticated by their MAC addresses. The following option is required, when specific RADIUS server rejects authentication for the clients with blank password | 
| **name** (*string* ; Default: ) | Descriptive name of the profile | 
| **nas-port-type** (*string* ; Default:**wireless-802.11** ) | NAS-Port-Type value to be sent to RADIUS server, NAS-Port-Type values are described in the RADIUS RFC 2865. This optional value attribute indicates the type of the physical port of the HotSpot server. | 
| **radius-accounting** (*yes \| no* ; Default:**yes** ) | Send RADIUS server accounting information for each user, when yes is used | 
| **radius-default-domain** (*string* ; Default: ) | Default domain to use for RADIUS requests. Allows to use separate RADIUS server per */ip hotspot profile* . If used, same domain name should be specified under /radius domain value. | 
| **radius-interim-update** (*time \| received* ; Default:**received** ) | How often to send accounting updates . When *received* is set, interim-time is used from RADIUS server.**0s** is the same as*received* . | 
| **radius-location-name** (*string* ; Default: ) | RADIUS-Location-Id to be sent to RADIUS server. Used to identify location of the HotSpot server during the communication with RADIUS server. Value is optional and used together with RADIUS server. | 
| **radius-mac-format** (*"XX XX XX XX XX XX"\|XX:XX:XX:XX:XX:XX\|XXXXXX-XXXXXX\|XXXXXXXXXXXX\|XX-XX-XX-XX-XX-XX\|XXXX:XXXX:XXXX\|XXXXXX:XXXXXX* ; Default:**XX:XX:XX:XX:XX:XX** ) | Option to set format of user mac-address, that is sent to RADIUS server during AAA session. | 
| **rate-limit** (*string* ; Default:**""** ) | Rate limitation in form of **rx-rate[/tx-rate] [rx-burst-rate[/tx-burst-rate] [rx-burst-threshold[/tx-burst-threshold] [rx-burst-time[/tx-burst-time]]]] [priority] [rx-rate-min[/tx-rate-min]]** from the point of view of the router (so "rx" is client upload, and "tx" is client download). All rates should be numbers with optional 'k' (1,000s) or 'M' (1,000,000s). If tx-rate is not specified, rx-rate is as tx-rate too. Same goes for tx-burst-rate and tx-burst-threshold and tx-burst-time. If both rx-burst-threshold and tx-burst-threshold are not specified (but burst-rate is specified), rx-rate and tx-rate is used as burst thresholds. If both rx-burst-time and tx-burst-time are not specified, 1s is used as default. rx-rate-min and tx-rate min are the values of limit-at properties | 
| **smtp-server** (*IP* ; Default:**0.0.0.0** ) | SMTP server address to be used to redirect HotSpot users SMTP requests. | 
| **split-user-domain** (*yes \| no* ; Default:**no** ) | Split username from domain name when the username is given in "user@domain" or in "domain\user" format from RADIUS server | 
| **ssl-certificate** (*string \| none* ; Default:**none** ) | Name of the SSL certificate on the router to to use only for HTTPS authentication. | 
| **trial-uptime** (*time/time* ; Default:**30m/1d** ) | Used only with *trial* authentication method. First time value specifies, how long trial user identified by MAC address can use access to public networks without HotSpot authentication. Second time value specifies amount of time, that has to pass until user is allowed to use trial again. | 
| **trial-user-profile** (*string* ; Default:**default** ) | Specifies **hotspot user profile** for trial users. | 
| **use-radius** (*yes \| no* ; Default:**no** ) | Use RADIUS to authenticate HotSpot users. | 

# HotSpot User Profiles

User profile menu is used for common HotSpot client settings. Profiles are like User groups with the same set of settings, rate-limit, filter chain name, etc.

| Property | Description | 
|---|---|
| **add-mac-cookie** (*yes\|no* ; Default:**yes** ) | Allows to add mac cookie for users. | 
| **address-list** (*string* ; Default: ) | Name of the address list in which users IP address will be added. Useful to mark traffic per user groups for queue tree configurations. | 
| **address-pool** (*string \|none* ; Default:**none** ) | IP pool name from which the user will get IP. When user has improper network settings configuration on the computer, HotSpot server makes translation and assigns correct IP address from the pool instead of incorrect one | 
| **advertise** (*yes \| no* ; Default:**no** ) | Enable forced advertisement popups. After certain interval specific web-page is being displayed for HotSpot users. Advertisement page might be blocked by browsers popup blockers. | 
| **advertise-interval** (*time[,time[,..]]* ; Default:**30m,10m** ) | Set of intervals between advertisement popups. After the list is done, the last value is used for all further advertisements, 10 minutes | 
| **advertise-timeout** (*time \| immediately \| never* ; Default:**1m** ) | How long advertisement is shown, before blocking network access for HotSpot client. Connection to Internet is not allowed, when advertisement is not shown. | 
| **advertise-url** (*string[,string[,..]]* ; Default: ) | List of URLs that is show for advertisement popups. After the last URL is used, list starts from the begining. | 
| **idle-timeout** (*time \| none* ; Default:**none** ) | Maximal period of inactivity for authorized HotSpot clients. Timer is counting, when there is no traffic coming from that client and going through the router, for example computer is switched off. User is logged out, dropped of the host list, the address used by the user is freed, when timeout is reached. | 
| **incoming-filter** (*string* ; Default: ) | Name of the firewall chain applied to incoming packets from the users of this profile, jump rule is required from built-in chain (input, forward, output) to chain=hotspot | 
| **incoming-packet-mark** (*string* ; Default: ) | Packet mark put on incoming packets from every user of this profile | 
| **keepalive-timeout** (*time \| none* ; Default: ) | Keepalive timeout for authorized HotSpot clients. Used to detect, that the computer of the client is alive and reachable. User is logged out, when timeout value is reached | 
| **mac-cookie-timeout** (*time* ; Default:**3d** ) | Selects mac-cookie timeout from last login or logout. | 
| **name** (*string* ; Default: ) | Descriptive name of the profile | 
| **on-login** (*string* ; Default:**""** ) | Script name to be executed, when user logs in to the HotSpot from the particular profile. It is possible to get username from internal **user** and**interface** variable. For example,*:log info "User $user logged in!"* . If hotspot is set on bridge interface, then**interface** variable will show bridge as actual interface unless**use-ip-firewall'** is set in bridge settings. List of available variables:  | 
| **on-logout** (*string* ; Default:**""** ) | Script name to be executed, when user logs out from the HotSpot.It is possible to get username from internal **user** and**interface** variable. For example,*:log info "User $user logged in!"* . If hotspot is set on bridge interface, then**interface** variable will show bridge as actual interface unless**use-ip-firewall** is set in bridge settings. List of available variables:  Starting with v6.34rc11 some additional variables are available:  | 
| **open-status-page** (*always \| http-login* ; Default:**always** ) | Option to show status page for user authenticated with mac login method. For example to show advertisement on status page (alogin.html)  | 
| **outgoing-filter** (*string* ; Default: ) | Name of the firewall chain applied to outgoing packets from the users of this profile, jump rule is required from built-in chain (input, forward, output) to chain=hotspot | 
| **outgoing-packet-mark** (*string* ; Default: ) | Packet mark put on outgoing packets from every user of this profile | 
| **rate-limit** (*string* ; Default:**""** ) | Simple dynamic queue is created for user, once it logs in to the HotSpot. Rate-limitation is configured in the following form **[rx-rate[/tx-rate] [rx-burst-rate[/tx-burst-rate] [rx-burst-threshold[/tx-burst-threshold] [rx-burst-time[/tx-burst-time] [priority] [rx-rate-min[/tx-rate-min]]]]** . For example, to set 1M download, 512k upload for the client, rate-limit=512k/1M | 
| **session-timeout** (*time* ; Default:**0s** ) | Allowed session time for client. After this time, the user is logged out unconditionally | 
| **shared-users** (*integer* ; Default:**1** ) | Allowed number of simultaneously logged in users with the same HotSpot username | 
| **status-autorefresh** (*time \| none* ; Default:**none** ) | HotSpot status page autorefresh interval | 
| **transparent-proxy** (*yes \|* ; Default:**yes** ) | Use transparent HTTP proxy for the authorized users of this profile | 

# HotSpot Users

This is the menu, where client's user/password information is actually added, additional configuration options for HotSpot users are configured here as well.

| Property | Description | 
|---|---|
| **address** (*IP* ; Default:**0.0.0.0** ) | IP address, when specified client will get the address from the HotSpot one-to-one NAT translations. Address does not restrict HotSpot login only from this address | 
| **comment** (*string* ; Default: ) | descriptive information for HotSpot user, it might be used for scripts to change parameters for specific clients | 
| **email** (*string* ; Default: ) | HotSpot client's e-mail, informational value for the HotSpot user | 
| **limit-bytes-in** (*integer* ; Default:**0** ) | Maximal amount of bytes that can be received from the user. User is disconnected from HotSpot after the limit is reached. | 
| **limit-bytes-out** (*integer* ; Default:**0** ) | Maximal amount of bytes that can be transmitted from the user. User is disconnected from HotSpot after the limit is reached. | 
| **limit-bytes-total** (*integer* ; Default:**0** ) | (limit-bytes-in+limit-bytes-out). User is disconnected from HotSpot after the limit is reached. | 
| **limit-uptime** (*time* ; Default:**0** ) | Uptime limit for the HotSpot client, user is disconnected from HotSpot as soon as uptime is reached. | 
| **mac-address** (*MAC* ; Default:**00:00:00:00:00:00** ) | Client is allowed to login only from the specified MAC-address. If value is *00:00:00:00:00:00* , any mac address is allowed. | 
| **name** (*string* ; Default: ) | HotSpot login page username, when MAC-address authentication is used name is configured as client's MAC-address | 
| **password** (*string* ; Default: )*sensitive* | User password | 
| **profile** (*string* ; Default:**default** ) | User profile configured in */ip hotspot user profile* | 
| **routes** (*string* ; Default: ) | Routes added to HotSpot gateway when client is connected. The route format **dst-address gateway metric** (for example,*192.168.1.0/24 192.168.0.1 1* ) | 
| **server** (*string \| all* ; Default:**all** ) | HotSpot server's name to which user is allowed login | 
| **otp-secret** (*string* ; Default: )*sensitive* | A one-time password token that is used for HotSpot user authorization, it could be used as separate "password" for HotSpot user authentication. | 

**Read-only proterties**

| Property | Description | 
|---|---|
| **bytes-in** (*integer* ) |  | 
| **bytes-out** (*integer* ) |  | 
| **packets-in** (*integer* ) |  | 
| **packets-out** (*integer* ) |  | 
| **uptime** (*time* ) |  | 

# HotSpot Active

HotSpot active menu shows all clients authenticated in HotSpot, the menu is informational (read-only) it is not possible to change anything here, except user can be logged out with the remove command.

| Parameters | Description | 
|---|---|
| **server** (read-only; name) | HotSpot server name client is logged in | 
| **user** (read-only; name) | name of the HotSpot user | 
| **domain** (read-only; text) | the domain of the user (if split from the username), a parameter is used only with RADIUS authentication | 
| **address** (read-only; IP address) | The IP address of the HotSpot user | 
| **mac-address** (read-only; MAC-address) | MAC-address of the HotSpot user | 
| **login-by** (read-only; multiple-choice: cookie**/** http-chap**/** http-pap**/** https**/** mac**/** mac-cookie**/** trial) | the authentication method used by the HotSpot client | 
| **uptime** (read-only; time) | current session time of the user, it is showing how long the user has been logged in | 
| **idle-time** (read-only; time) | the amount of time the user has been idle | 
| **session-time-left** (read-only; time) | the exact value of session-time, that is applied for the user. Value shows how long user is allowed to be online to be logged off automatically by **uptime** reached | 
| **idle-timeout** (read-only; time) | the exact value of the user's idle-timeout | 
| **keepalive-timeout** (read-only; time) | the exact value of the keepalive-timeout, that is applied for the user. Value shows how long the host can stay out of reach to be removed from the HotSpot | 
| **limit-bytes-in** (read-only; integer) | value shows how many bytes received from the client, an option is active when the appropriate parameter is configured for HotSpot user | 
| **limit-bytes-out** (read-only; integer) | value shows how many bytes send to the client, an option is active when the appropriate parameter is configured for HotSpot user | 
| **limit-bytes-total** (read-only; integer) | value shows how many bytes total were send/received from the client, an option is active when the appropriate parameter is configured for HotSpot user | 

# HotSpot Host

The host table lists all computers connected to the HotSpot server. The host table is informational and it is not possible to change any value there:

| Parameters | Description | 
|---|---|
| **mac-address** (read-only; MAC-address) | HotSpot user MAC-address | 
| **address** (read-only; IP address) | HotSpot client original IP address | 
| **to-address** (read-only; IP address) | The new client address assigned by HotSpot might be the same as the original **address** | 
| **server** (read-only; name) | HotSpot server name client is connected to | 
| **bridge-port** (read-only; name) | *"/interface bridge port"* the client is connected to, value is unknown when HotSpot is not configured on the bridge | 
| **uptime** (read-only; time) | value shows how long the user is online (connected to the HotSpot) | 
| **idle-time** (read-only; time) | time user has been idle | 
| **idle-timeout** (read-only; time) | value of the client idle-timeout (unauthorized client) | 
| **keepalive-timeout** (read-only; time) | keepalive-timeout value of the unauthorized client | 
| **bytes-in** (read-only; integer) | amount of bytes received from an unauthorized client | 
| **packet-in** (read-only; integer) | amount of packets received from an unauthorized client | 
| **bytes-out** (read-only; integer) | amount of bytes sent to an unauthorized client | 
| **packet-out** (read-only; integer) | amount of packets sent to an unauthorized client | 

# HotSpot walled-garden

Walled garden is a system which allows unauthorized use of some resources, but requires authorization to access other resources. This is useful, for example, to give access to some general information about HotSpot service provider or billing options

The menu only manager Walled Garden for HTTP and HTTPs protocols. Other protocols can also be include in Walled Garden, but that is configured elsewhere (in /ip hotspot walled-garden ip).

| Property | Description | 
|---|---|
| **action** (*allow \| deny* ; Default:**allow** ) | Action to perform, when packet matches the rule  | 
| **server** (*string* ; Default: ) | Name of the HotSpot server, rule is applied to. | 
| **src-address** (*IP* ; Default: ) | Source address of the user, usually IP address of the HotSpot client | 
| **method** (*string* ; Default: ) | HTTP method of the request | 
| **dst-host** (*string* ; Default: ) | Domain name of the destination web-server | 
| **dst-port** (*integer* ; Default: ) | TCP port number, client sends request to | 
| **path** (*string* ; Default: ) | The path of the request, path comes after '''http://dst_host' | 

**Read-only properties**

| Property | Description | 
|---|---|
| **dst-address** (*IP* ) |  | 
| **hits** (*integer* ) |  | 

Wildcard properties (dst-host and path) match a complete string (i.e., they will not match "example.com" if they are set to "example"). Available wildcards are '*' (match any number of any characters) and '?' (match any one character). Regular expressions are also accepted here, but if the property should be treated as a regular expression, it should start with a colon (':'). To show that no symbols are allowed before the given pattern, we use ^ symbol at the beginning of the pattern. To specify that no symbols are allowed after the given pattern, we use $ symbol at the end of the pattern.

## Example

To only permit bypassed access in walled garden to "www.example.com/test" but not to "www.example.com/test/test.php" :

/ip hotspot walled-garden
add dst-host=:^www.example.com path=":/test\$"

# HotSpot walled-garden ip

To bypass HotSpot authentication for other protocols and different src/dst addresses (or address-lists). Used for different services (Winbox, SSH, Telnet, SIP, etc.)

| Property | Description | 
|---|---|
| **action** (*accept \|drop\|reject* ; Default:**allow** ) | Action to perform, when packet matches the rule  | 
| **server** (*string* ; Default: ) | Name of the HotSpot server, rule is applied to. | 
| **src-address** (*IP* ; Default: ) | Source address of the user, usually IP address of the HotSpot client | 
| **dst-address** (*IP* ; Default: ) | Destination IP address, IP address of the WEB-server. Ignored if **dst-host** is already specified. | 
| **src-address-list** (*string* ; Default: ) | Source address list name | 
| **dst-address-list** (*string* ; Default: ) | Destination address list. Ignored if **dst-host** is already specified. | 
| **dst-host** (*string* ; Default: ) | Domain name of the destination web-server. When this parameter is specified dynamic entry is added to Walled Garden | 
| **dst-port** (*integer* ; Default: ) | TCP port number, client sends request to | 
| **protocol** (*integer \| string* ; Default: ) | IP protocol | 

# IP Binding

IP-Binding HotSpot menu allows to the setup of static One-to-One NAT translations, allows to bypass specific HotSpot clients without any authentication, and also allows to block specific hosts and subnets from the HotSpot network

| Property | Description | 
|---|---|
| **address** (*IP Range* ; Default:**""** ) | The original IP address of the client | 
| **mac-address** (*MAC* ; Default:**""** ) | MAC address of the client | 
| **server** (*string \| all* ; Default:**"all"** ) | Name of the HotSpot server.  | 
| **to-address** (*IP* ; Default:**""** ) | New IP address of the client, translation occurs on the router (client does not know anything about the translation) | 
| **type** (*blocked \| bypassed \| regular* ; Default:**""** ) | Type of the IP-binding action  | 

# Cookies

The menu contains all cookies sent to the HotSpot clients, which are authorized by cookie method, all the entries are read-only.

| Property | Description | 
|---|---|
| **domain** (*string* ) | The domain name (if split from the username) | 
| **expires-in** (*time* ) | How long the cookie is valid | 
| **mac-address** (*MAC* ) | Client's MAC-address | 
| **user** (*string* ) | HotSpot username | 

# MAC Cookie

MAC cookie is a hotspot feature, designed to improve accessibility for smartphones, laptops and other mobile devices.

When MAC cookie feature is enabled (**login-by**=mac-cookie, **add-mac-cookie**=yes set in user profile), following actions are taken:

- **first successful login** . Mac cookie keeps record of username and password for the MAC address if there is only one host with such MAC. Cookie timeout is set to value equal to mac-cookie-timeout.
- **new host appears** . Hotspot checks if there is a mac cookie record for the MAC address and logs in host using recorded username and password. If there is more than one host with the same MAC address, user will not be logged in and MAC cookie record for this address will be deleted.

When **user logs out** mac cookie is removed in following cases:

- user-request - user clicked on logout button.
- admin-reset - disconnected from radius server or user is removed from hotspot active menu
- nas-request - traffic limit reached
- session-timeout

To debug problems with mac-cookies you will need to enable hotspot debug logs and look for reasons why mac-cookie login didn't work for certain host.

Reasons when mac cookie is removed by server:

- /ip hotspot cookie remove <x>
- /ip hotspot client remove <x>
- Radius server sends Disconnect-Request
- End-User has logged out him self via hotspot status page
- End user has reached his data cap ("traffic limit reached")
- Session-Timeout
- If mac-cookie login fails
- If server detects that in host table there is more than one entry with the same mac-address

# Using DHCP option to advertise HotSpot URL

Most devices, such as modern smartphones, do some kind of background checking to see if they are behind a captive portal. They do this by requesting a known webpage and comparing the contents of that page, to what they should be. If contents are different, the device assumes there is a login page and creates a popup with this login page.

This does not always happen, as this "known webpage" could be blocked, whitelisted, or not accessible in internal networks. To improve on this mechanism, RFC 7710 was created, allowing the HotSpot to inform all DHCP clients that they are behind a captive-portal device and that they will need to authenticate to get Internet access, regardless of what webpages they do or do not request.

This DHCP option field is enabled automatically, but only if the router has a DNS name configured and has a valid SSL certificate (so that the login page can be accessed over HTTPS). When these requirements are met, a special DHCP option will be sent, containing a link to `https://<dns-name-of-hotspot>/api`. This link contains information in JSON format, instructing the client device of the captive portal status, and the location of the login page.

Contents of `https://<dns-name-of-hotspot>/api` are as follows:

Some devices require venue-info URL as well, so you are free to modify the api.json file to your liking, just like any other hotspot files. It is located in the router files menu.

Important

If you have set up Hotspot before RouterOS v7.3 when RFC 7710 was implemented, you will have to use "Reset HTML" function, or manually add/edit the api.json file to have the above contents, for Hotspot detection to work.

# Load balancing with mangle on Hotspot server

In a multi-uplink Hotspot setup (for example, PCC), default routing often might fail to work as expected because the router prioritises mangle rules over the local destination check.

To fix this, ensure traffic destined for the Hotspot server is routed via the **local** table before the **mangle** chain is evaluated. You can achieve this by:

1. Adjusting the "`/routing settings"` (Routing table lookup).
2. Or, adding specific "`/ip route rule"` entries to force the Hotspot server's traffic to use the local table first (including HTTP 80 traffic which is used to detect the Hotspot server).
