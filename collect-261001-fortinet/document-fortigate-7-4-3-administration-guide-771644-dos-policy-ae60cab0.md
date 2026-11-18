# DoS policy

# DoS policy

A Denial of Service (DoS) policy examines network traffic arriving at a FortiGate interface for anomalous patterns, which usually indicates an attack.

A denial of service occurs when an attacking system starts an abnormally large number of sessions with a target system. The large number of sessions slows down or disables the target system, preventing legitimate users from using it.

DoS policies are checked before security policies, preventing attacks from triggering more resource intensive security protection and slowing down the FortiGate.

## DoS anomalies

Predefined sensors are setup for specific anomalous traffic patterns. New DoS anomalies cannot be added by the user.

The predefined anomalies that can be used in DoS policies are:

| Anomaly | Description | Recommended Threshold | 
|---|---|---|
| tcp_syn_flood | If the SYN packet rate of new TCP connections, including retransmission, to one destination IP address exceeds the configured threshold value, the action is executed. | 2000 packets per second. | 
| tcp_port_scan | If the SYN packet rate of new TCP connections, including retransmission, from one source IP address exceeds the configured threshold value, the action is executed. | 1000 packets per second. | 
| tcp_src_session | If the number of concurrent TCP connections from one source IP address exceeds the configured threshold value, the action is executed. | 5000 concurrent sessions. | 
| tcp_dst_session | If the number of concurrent TCP connections to one destination IP address exceeds the configured threshold value, the action is executed. | 5000 concurrent sessions. | 
| udp_flood | If the UDP traffic to one destination IP address exceeds the configured threshold value, the action is executed. | 2000 packets per second. | 
| udp_scan | If the UDP sessions setup rate originating from one source IP address exceeds the configured threshold value, the action is executed. | 2000 sessions per second. | 
| udp_src_session | If the number of concurrent UDP connections from one source IP address exceeds the configured threshold value, the action is executed. | 5000 concurrent sessions. | 
| udp_dst_session | If the number of concurrent UDP connections to one destination IP address exceeds the configured threshold value, the action is executed. | 5000 concurrent sessions. | 
| icmp_flood | If the number of ICMP packets sent to one destination IP address exceeds the configured threshold value, the action is executed. | 250 packets per second. | 
| icmp_sweep | If the ICMP sessions setup rate originating from one source IP address exceeds the configured threshold value, the action is executed. | 100 sessions per second. | 
| icmp_src_session | If the number of concurrent ICMP connections from one source IP address exceeds the configured threshold value, the action is executed. | 300 concurrent sessions | 
| icmp_dst_session | If the number of concurrent ICMP connections to one destination IP address exceeds the configured threshold value, the action is executed. | 1000 concurrent sessions | 
| ip_src_session | If the number of concurrent IP connections from one source IP address exceeds the configured threshold value, the action is executed. | 5000 concurrent sessions. | 
| ip_dst_session | If the number of concurrent IP connections to one destination IP address exceeds the configured threshold value, the action is executed. | 5000 concurrent sessions. | 
| sctp_flood | If the number of SCTP packets sent to one destination IP address exceeds the configured threshold value, the action is executed. | 2000 packets per second | 
| sctp_scan | If the number of SCTP sessions originating from one source IP address exceeds the configured threshold value, the action is executed. | 1000 packets per second | 
| sctp_src_session | If the number of concurrent SCTP connections from one source IP address exceeds the configured threshold value, the action is executed. | 5000 concurrent sessions | 
| sctp_dst_session | If the number of concurrent SCTP connections to one destination IP address exceeds the configured threshold value, the action is executed. | 5000 concurrent sessions | 

For thresholds based on the number of concurrent sessions, blocking the anomaly will not allow more than the number of concurrent sessions to be set as the threshold.

For example, if the period for a particular anomaly is 60 seconds, such as those where the threshold is measured in concurrent sessions, after the 60 second timer has expired the number of allowed sessions that match the anomaly criteria is reset to zero. This means that, if you allow 10 sessions through before blocking, after the 60 seconds has elapsed, another 10 sessions will be allowed. The attrition of sessions from expiration should keep the allowed sessions from reaching the maximum.

For rate based thresholds, where the threshold is measured in packets per second, the *Block* action prevents anomalous traffic from overwhelming the firewall in two ways:

- 
                                                    continuous: Block packets once an anomaly is detected, and continue to block packets while the rate is above the threshold. This is the default setting.
- 
                                                    periodical: After an anomaly is detected, allow the configured number of packets per second.

For example, if a DoS policy is configured to block icmp_flood with a threshold of 10pps, and a continuous ping is started at a rate of 20pps for 1000 packets:

- 
                                                    In continuous mode, the first 10 packets are passed before the DoS sensor if triggered, and then the remaining 990 packets are blocked.
- 
                                                    In periodical mode, 10 packets are allowed to pass per second, so 500 packets are blocked in the 50 seconds during which the ping is occuring.

|  | The actual numbers of passed and blocked packets may not be exact, as fluctuations in the rates can occur, but the numbers should be close to the defined threshold. | 

###### To configure the block action for rate based anomaly sensors:

```
config ips global
    set anomaly-mode {continuous | periodical}
end
```
                                            ## DoS policies

A DoS policy can be configured to use one or more anomalies.

###### To configure a DoS policy in the GUI:

1. 
                                                    Go to *Policy & Objects > IPv4 DoS Policy* or*Policy & Objects > IPv6 DoS Policy* and click*Create New* .If the option is not visible, enable *DoS Policy* in*Feature Visibility* . See Feature visibility for details.
2. 
                                                    Configure the following: Name Enter a name for the policy. Incoming Interface Enter the interface that the policy applies to. Source Address Enter the source address. Destination Address Enter the destination address. This is the address that the traffic is addressed to. In this case, it must be an address that is associated with the firewall interface. For example, it could be an interface address, a secondary IP address, or the address assigned to a VIP address. Service Select the services or service groups. The ALL service can used or, to optimize the firewall resources, only the services that will be answered on an interface can be used. L3 Anomalies L4 Anomalies Configure the anomalies: 
  - 
                                                                            *Logging* : Enable/disable logging for specific anomalies or all of them. Anomalous traffic will be logged when the action is*Block* or*Monitor* .
  - 
                                                                            *Action* : Select the action to take when the threshold is reached:
    - 
                                                                                    *Disable* : Do not scan for the anomaly.
    - 
                                                                                    *Block* : Block the anomalous traffic.
    - 
                                                                                    *Monitor* : Allow the anomalous traffic, but record a log message if logging is enabled.
  - 
                                                                                    
  - 
                                                                            *Threshold* : The number of detected instances that triggers the anomaly action.
 Comments Optionally, enter a comment.
3. 
                                                                            
4. 
                                                    Enable the policy, then click *OK* .

|  | The quarantine option is only available in the CLI. See Quarantine for information. | 

###### To configure a DoS policy in the GUI:

```
config firewall DoS-policy
    edit 1
        set name "Flood"
        set interface "port1"
        set srcaddr "all"
        set dstaddr "all"
        set service "ALL"
        config anomaly
            edit "icmp_flood"
                set status enable
                set log enable
                set action block
                set quarantine attacker
                set quarantine-expiry 1d1h1m
                set quarantine-log enable
                set threshold 100
            next
        end
    next
end
```
                                            | name <string> | Enter a name for the policy. | 
| interface <string> | Enter the interface that the policy applies to. | 
| srcaddr <string> | Enter the source address. | 
| dstaddr <string> | Enter the destination address. This is the address that the traffic is addressed to. In this case, it must be an address that is associated with the firewall interface. For example, it could be an interface address, a secondary IP address, or the address assigned to a VIP address. | 
| service <string> | Enter the services or service groups. The `ALL` service can used or, to optimize the firewall resources, only the services that will be answered on an interface can be used. | 
| status {enable \| disable} | Enable/disable this anomaly. | 
| log {enable \| disable} | Enable/disable anomaly logging. When enabled, a log is generated whenever the anomaly action is triggered, regardless of which action is configured. | 
| action {pass \| block} | Set the action to take when the threshold is reached:  | 
| quarantine {none \| attacker} | Set the quarantine method (see Quarantine):  | 
| quarantine-expiry <###d##h##m> | Set the duration of the quarantine, in days, hours, and minutes (###d##h##m) (1m - 364d23h59m, default = 5m). This option is available  if `quarantine` is set`attacker` . | 
| quarantine-log {enable \| disable} | Enable/disable quarantine logging (default = disable).  This option is available  if `quarantine` is set`attacker` . | 
| threshold <integer> | The number of detected instances - packets per second or concurrent session number - that triggers the anomaly action. | 

Quarantine is used to block any further traffic from a source IP address that is considered a malicious actor or a source of traffic that is dangerous to the network. Traffic from the source IP address is blocked for the duration of the quarantine, and the source IP address is added to the banned user list.

The banned user list is kept in the kernel, and used by Antivirus, Data Loss Prevention (DLP), DoS, and Intrusion Prevention System (IPS). Any policies that use any of these features will block traffic from the attacker's IP address.

###### To view the quarantined user list:

# diagnose user banned-ip list
src-ip-addr       created                  expires                  cause            
192.168.2.205     Wed Nov 25 12:47:54 2020 Wed Nov 25 12:57:54 2020 DOS   

## Troubleshooting DoS attacks

The best way to troubleshoot DoS attacks is with Anomaly logs and IPS anomaly debug messages.


###### To test an icmp_flood attack:

1. 
                                                    From the Attacker, launch an icmp_flood with 50pps lasting for 3000 packets.
2. 
                                                    On the FortiGate, configure continuous mode and create a DoS policy with an icmp_flood threshold of 30pps: ```
config firewall DoS-policy
    edit 1
        set name icmpFlood
        set interface "port1"
        set srcaddr "all"
        set dstaddr "all"
        set service "ALL"
        config anomaly
            edit "icmp_flood"
                set status enable
                set log enable
                set action block
                set threshold 30
            next
        end
    next
end
```
3. 
                                                    Configure the debugging filter: # diagnose ips anomaly config DoS sensors in kernel vd 0: DoS id 1 proxy 0 0 tcp_syn_flood status 0 log 0 nac 0 action 0 threshold 2000 ... 7 udp_dst_session status 0 log 0 nac 0 action 0 threshold 5000 **8 icmp_flood status 1 log 1 nac 0 action 7 threshold 30** 9 icmp_sweep status 0 log 0 nac 0 action 0 threshold 100
  ...
total # DoS sensors: 1.# diagnose ips anomaly filter id 8
4. 
                                                    Launch the icmp_flood from a Linux machine. This example uses Nmap: $ sudo nping --icmp --rate 50 -c 3000 192.168.2.50 SENT (0.0522s) ICMP [192.168.2.205 > 192.168.2.50 Echo request (type=8/code=0) id=8597 seq=1] IP [ttl=64 id=47459 iplen=28 ] ... Max rtt: 11.096ms | Min rtt: 0.028ms | Avg rtt: 1.665ms Raw packets sent: 3000 (84.000KB) | Rcvd: 30 (840B) | Lost: 2970 (99.00%) Nping done: 1 IP address pinged in 60.35 seconds
5. 
                                                    During the attack, check the anomaly list on the FortiGate: # diagnose ips anomaly list list nids meter: **id=icmp_flood         ip=192.168.2.50 dos_id=1 exp=998 pps=46 freq=50** total # of nids meters: 1.id=icmp_flood The anomaly name. ip=192.168.2.50 The IP address of the host that triggered the anomaly. It can be either the client or the server. For icmp_flood, the IP address is the destination IP address. For icmp_sweep, it would be the source IP address. dos_id=1 The DoS policy ID. exp=998 The time to be expired, in jiffies (one jiffy = 0.01 seconds). pps=46 The number of packets that had been received when the diagnose command was executed. freq=50 For session based anomalies, freq is the number of sessions. For packet rate based anomalies (flood, scan): 
  - 
                                                                            In continuous mode: freq is the greater of pps, or the number of packets received in the last second.
  - 
                                                                            In periodic mode: freq is the pps.
6. 
                                                                            
7. 
                                                    Go to *Log & Report > Security Events* and download the*Anomaly* logs:date=2020-11-20 time=14:38:39 eventtime=1605911919824184594 tz="-0800" logid="0720018433" type="utm" subtype="anomaly" eventtype="anomaly" level="alert" vd="root" severity="critical" **srcip=192.168.2.205** srccountry="Reserved"**dstip=192.168.2.50** srcintf="port1" srcintfrole="undefined" sessionid=0**action="clear_session"** proto=1 service="PING" count=1307 attack="icmp_flood" icmpid="0x2195" icmptype="0x08" icmpcode="0x00" attackid=16777316 policyid=1 policytype="DoS-policy" ref="http://www.fortinet.com/ids/VID16777316"**msg="anomaly: icmp_flood, 31 > threshold 30, repeats 28 times"** crscore=50 craction=4096 crlevel="critical"date=2020-11-20 time=14:39:09 eventtime=1605911949826224056 tz="-0800" logid="0720018433" type="utm" subtype="anomaly" eventtype="anomaly" level="alert" vd="root" severity="critical" **srcip=192.168.2.205** srccountry="Reserved"**dstip=192.168.2.50** srcintf="port1" srcintfrole="undefined" sessionid=0**action="clear_session"** proto=1 service="PING" count=1497 attack="icmp_flood" icmpid="0x2195" icmptype="0x08" icmpcode="0x00" attackid=16777316 policyid=1 policytype="DoS-policy" ref="http://www.fortinet.com/ids/VID16777316"**msg="anomaly: icmp_flood, 50 > threshold 30, repeats 1497 times"** crscore=50 craction=4096 crlevel="critical"###### AnalysisIn the first log message: msg="anomaly: icmp_flood, 31 > threshold 30 At the beginning of the attack, a log is recorded when the threshold of 30pps is broken. repeats 28 times The number of packets that has exceeded the threshold since the last time a log was recorded. srcip=192.168.2.205 dstip=192.168.2.50 The source and destination IP addresses of the attack. action="clear_session" Equivalent to block. If `action` was set to`monitor` and logging was enabled, this would be`action="detected"` .In the second log message: 
  - 
                                                            Because it is an ongoing attack, the FortiGate generates one log message for multiple packets every 30 seconds..
  - 
                                                            It will not generate a log message if: 
    - 
                                                                    The same attack ID happened more than once in a five second period, or
    - 
                                                                    The same attack ID happened more than once in a 30 second period and the actions are the same and have the same source and destination IP addresses.
  - 
                                                                    
 msg="anomaly: icmp_flood, 50 > threshold 30 In the second before the log was recorded, 50 packets were detected, exceeding the configured threshold. repeats 1497 times The number of packets that has exceeded the threshold since the last time a log was recorded
8.