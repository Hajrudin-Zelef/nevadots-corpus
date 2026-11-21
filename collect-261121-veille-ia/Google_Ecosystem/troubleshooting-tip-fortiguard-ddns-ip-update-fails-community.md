---
id: collect-261121-veille-ia/google-ecosystem/troubleshooting-tip-fortiguard-ddns-ip-update-fails-community
title: "Troubleshooting Tip: FortiGuard DDNS IP update fails | Community"
domain: google-ecosystem
role: reference
task: article
actors: ["Google"]
dates: ["2025-12-11"]
keywords: ["ddns", "fortiguard", "diagnose", "update", "domain", "debug", "fortigate", "interface"]
source: https://community.fortinet.com/t5/FortiGate/Troubleshooting-Tip-FortiGuard-DDNS-IP-update-fails/ta-p/198179
source_anchor: ""
source_lines: [1, 253]
section: "Google_Ecosystem"
---
**Description**

This article describes how to fix the 'IP not-updating' problem with FortiGuard DDNS.

When the public IP of the FortiGate has changed, FortiGuard DDNS updates are required over one specific ISP interface and sometimes FortiGuard DDNS does not update the IP.

One possible way to solve this, users need to configure static routing to allow the traffic from FortiGate to the FortiGuard IP addresses through that specific internet connection. This is shown below, and also other possible causes for this.**Related document:**

DDNS


**Scope**


FortiGate.

**Solution**


One common solution is to configure a static route to a known FortiGuard IP through a specific ISP Internet connection.

First of all, make sure that the FortiGate knows the new IP address. This is reflected in the Dashboard status widget, or **Network -> DNS**.

Make sure this is the correct public IP that the user wants to update in DDNS. If not correct, try the following commands to see if correctly updated:


```
diagnose sys waninfo
diagnose sys waninfo ipify
```

Run the following command from the CLI to find out the IP address used by FortiGate for the DDNS server:

- 'svr[0]= 208.91.113.230' is the FortiGuard IP used to create the static route.

```
diagnose test application ddnscd 3
FortiDDNS status:
ddns_ip=208.91.113.230 ddns_port=443 svr_num=1 domain_num=3
svr[0]= 208.91.113.230
domain[0]= fortiddns.com
domain[1]= fortidyndns.com
domain[2]= float-zone.com
```

If DDNS servers are not reachable:

`ddns_ip=0.0.0.0 ddns_port=443 svr_num=0 domain_num=0`

Creating the static route:

- Substitute <ip_gateway> with the ISP's remote IP.
- Substitute <interface_name> with the WAN interface (internet connection).

```
config router static
    edit 0
        set status enable
        set dst 208.91.113.230
        set gateway <ip_gateway>
        set device <interface_name>
    next
end
```

Before hardcoding ddns-server-ip, verify the currently learned FortiGuard DDNS server IP with diagnose test application ddnscd 3, as the available server addresses may change.

Another step is to manually set up the FortiGuard DDNS server IP address to a fixed one. This ensures the IP is not automatically changed, rendering the static route useless:

```
config system fortiguard
    set ddns-server-ip 208.91.113.230
end 
```

Or (another IP that can be used):

`set ddns-server-ip 173.243.138.225``set ddns-server-ip 173.243.138.226` 
**Note:** 173.243.138.225 is used when FortiGuard Anycast is enabled, and 173.243.138.226 is used when Anycast is disabled. Configure the ddns-server-ip value that matches the current Anycast setting. See Technical Tip: Unable to load FortiGuard DDNS server list for background on this IP pairing.


If the problem persists, make sure that the correct configuration is done on the unit, then run the debug commands below to show potential problems with DDNS:

- Wait 5-10 minutes until the timeout expires and some relevant output is produced after enabling the debug processes, then disable them.

```
diagnose debug application update -1
diagnose debug application ddnscd -1
diagnose debug console timestamp enable
diagnose debug enable
diagnose debug disable
```

The following errors may be seen:

`Failed on update FortiGuardDDNS (example_domain.fortiddns.com), due to internal/config/connect/io err`

This usually points to a routing error, or the domain has been registered with a different FortiGate serial number.


A possible fix if the ISP router and a local subnet are used on the WAN interface is to change the following in the DDNS setup:

- Make sure 'set use-public-ip' is enabled.
- Make sure 'set monitor-interface "port4"' is the WAN interface.

```
config system ddns
    show
        edit 1
            set use-public-ip enable
            set monitor-interface "port4"
        next
end
```

The following error shows a problem connecting to FortiGuard servers. Check the FortiGuard settings and change the relevant settings accordingly: Technical Tip: FortiGuard is not reachable via Anycast default method.

`failed to establish SSLconnection`

This, on the other hand, is not an error:

`next wait timeout 10 seconds.`

It is a repeating timer-message to show the DDNS is active. In some cases, it may be the only thing displayed (no other output even after 15 minutes).

In this case, the user may try to restart the DDNS connection:

```
execute update-now
diagnose test app ddnscd 2
diagnose test app ddnscd 4
```

```
-->ResponseStatus=-2
fgt_unpack_fcpr()-578: Unpacked obj: Protocol=3.4|SerialNumber=DDNS-ANY-VM-0102|ResponseStatus=-2|Command=DDNSRemove|DomainName=xyxy.fortiddns.com
fgd_ddns_fcp_exchange()-935: Recvd FCPR=Protocol=3.4|SerialNumber=DDNS-ANY-VM-0102|ResponseStatus=-2|Command=DDNSRemove|DomainName=xyxy.fortiddns.com
```

'ResponseStatus=-2' indicates either that the domain name requested does not exist or is registered with a different device Serial Number.

The DDNS server list is obtained from FortiGuard, so FortiGuard settings may need to be reviewed. Sometimes disabling fortiguard-anycast and using the SDNS server IP manually can be helpful.

```
config system fortiguard
    set fortiguard-anycast disable
    set protocol udp
    set port 8888
    set sdns-server-ip 208.91.112.220 173.243.140.53 
end
```

In some cases, despite the previous configurations, the error continues:

`Failed on update FortiGuardDDNS (example_domain.fortiddns.com), due to internal/config/connect/io err`

Note the involvement of some SD-WAN configuration at this point. The routing problem may be because in the '**config system fortiguard**' configuration, the 'SDWAN' interface is being selected as the interface-select-method, and no SD-WAN Rule has been created for this condition. If this is the case, then change the method for choosing the SD-WAN interface to auto (FortiGate needs to know a valid default route to the DDNS server), or create an SD-WAN Rule so that the SD-WAN method is applied correctly:

- Make sure that FortiGate recognizes a valid route to the DDNS server with 'set interface-select-method auto'.

```
config system fortiguard
    set fortiguard-anycast disable
    set protocol udp
    set port 8888
    set sdns-server-ip 208.91.112.220 173.243.140.53 
    set ddns-server-ip 208.91.113.230
    set interface-select-method auto
end   
```

If the interface-select-method is SD-WAN instead of Auto, create an SD-WAN rule for the DDNS server, and the configuration will be applied correctly.

- Make sure to have created an SD-WAN rule for the DDNS server with 'set interface-select-method sdwan'.

```
config system fortiguard
    set fortiguard-anycast disable
    set protocol udp
    set port 8888
    set sdns-server-ip 208.91.112.220 173.243.140.53 
    set ddns-server-ip 208.91.113.230
    set interface-select-method sdwan 
end   
```

**Note:** There are some instances where, when the 'ddns-server-ip' is already configured, the '**internal/config/connect/io err**' error still appears. This can be fixed by unsetting the ddns-server-ip and ddns-server-port instead.

```
config system fortiguard
    unset ddns-server-ip
    unset ddns-server-port
end   
```

Another error that can be seen in the debug log is as follows:

```
2025-12-11 11:22:20 __ddns_ssl_connect()-669: ssl_res=1
2025-12-11 11:22:20 [1109] ssl_connect: SSL_connect failes: error:0A00042E:SSL routines::tlsv1 alert protocol version
2025-12-11 11:22:20 __ddns_ssl_connect()-669: ssl_res=-1
2025-12-11 11:22:20 ddns_sock_ssl_connect()-754: failed to establish SSL connection
```

This can be caused by a mismatch between the protocol version being used for negotiation and one that is not supported by the firewall. This needs to be verified further to determine why the unsupported version is being used.


As a workaround, the firewall can be configured to support the protocol being used for negotiation:

```
config system global
    set ssl-min-proto-version TLS1.0
end
```
**Note:** An MTU mismatch on the interface can also cause a TLS handshake failure like this, as an alternative to lowering ssl-min-proto-version. Verify the path MTU first with execute ping-options df-bit yes, followed by execute ping-options data-size <MTU size> and **execute ping 8.8.8.8**, then adjust the interface MTU if needed. See Technical Tip: How to adjust the Maximum Transmission Unit (MTU) value on a FortiGate interface for details on adjusting the MTU.


One more reason why the DDNS update might fail is if the WAN interface IP is assigned via DHCP and the 'override internal DNS' setting is enabled. The DNS server from the ISP may not be able to resolve the DDNS domain (globalddns.fortinet.net) and retrieve the IP for the FortiGuard DDNS servers.

In cases like this, the override has to be disabled:

In the GUI:


In the CLI:

```
config system interface
    edit "<wan interface>"
        set dns-server-override disable
    next
end
```

Restarting the FortiGate DDNS Client (ddnscd):

If FortiGuard **DDNS updates fail or become unresponsive**, restart the internal DDNS client daemon **(ddnscd)** to clear the session and force a fresh update.**Example use case:**

This can be useful when DDNS appears stuck, not updating despite correct configuration and connectivity.

Run the following command to terminate the running DDNS client daemon:

In the CLI:

`fnsysctl killall ddnscd`

This uses the FortiGate internal **fnsysctl** utility to kill all instances of the **ddnscd** process. Upon termination, the system will automatically restart the process as needed. 

If none of these steps allow the correct update of the IP, contact the Fortinet TAC team by creating a ticket for the issue and providing the above logs.**Updating the device with a new ISP link:**

If the ISP link is getting changed with a new public-ip and the DDNS resolving to that entry also needs to be changed, reach out to the TAC team to delete the old DDNS entry from the database.

After that, under DDNS settings via the CLI, delete the copy config for the previous one and delete that entry. Once done, paste the copied configuration and only change the attribute for set monitor-interface to the new WAN port.
