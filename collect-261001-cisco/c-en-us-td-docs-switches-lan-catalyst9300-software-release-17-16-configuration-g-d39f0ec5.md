The documentation set for this product strives to use bias-free language. For the purposes of this documentation set, bias-free is defined as language that does not imply discrimination based on age, disability, gender, racial identity, ethnic identity, sexual orientation, socioeconomic status, and intersectionality. Exceptions may be present in the documentation due to language that is hardcoded in the user interfaces of the product software, language used based on RFP documentation, or language that is used by a referenced third-party product. Learn more about how Cisco is using Inclusive Language.
Multiprotocol Label Switching (MPLS) Virtual Private Network (VPN) services have been configured between the Provider Edge
(PE) routers and the customer edge (CE) routers at the customer sites.
Restrictions for EIGRP Prefix Limit Support
This feature is supported only under the IPv4 VRF address family and can be used only to limit the number of prefixes that
are accepted through a VRF.
The EIGRP Prefix Limiting Support feature is enabled only under the IPv4 VRF address-family. A peer that is configured to
send too many prefixes or a peer that rapidly advertises and then withdraws prefixes can cause instability in the network.
This feature can be configured to automatically reestablish a disabled peering session at the default or user-defined time
interval or when the maximum-prefix limit is not exceeded. However, the configuration of this feature alone cannot change
or correct a peer that is sending an excessive number of prefixes. If the maximum-prefix limit is exceeded, you will need
to reconfigure the maximum-prefix limit or reduce the number of prefixes that are sent from the peer.
Information About EIGRP Prefix Limit Support
The EIGRP Prefix Limit Support feature introduces the capability to limit the number of prefixes per VRF that are accepted
from a specific peer or to limit all prefixes that are accepted by an Enhanced Interior Gateway Routing Protocol (EIGRP) process
through peering and redistribution. This feature is designed to protect the local device from external misconfiguration that
can negatively impact local system resources; for example, a peer that is misconfigured to redistribute full Border Gateway
Protocol (BGP) routing tables into EIGRP. This feature is enabled under the IPv4 VRF address family and can be configured
to support the MPLS VPN Support for EIGRP Between Provider Edge and Customer Edge feature.
The EIGRP Prefix Limit Support feature provides the ability to configure a limit on the number of prefixes that are accepted
from EIGRP peers or learned through redistribution. This feature can be configured on per-peer or per-process basis and can
be configured for all peers and processes. This feature is designed to protect the local device from misconfigured external
peers by limiting the amount of system resources that can be consumed to process prefix updates.
Misconfigured VPN Peers
In MPLS VPNs, the number of routes that are permitted in the VPN routing and forwarding instance (VRF) is configured with
the maximumroutes VRF configuration command. However, limiting the number routes permitted in the VPN does not protect the local device from
a misconfigured peer that sends an excessive number of routes or prefixes. This type of external misconfiguration can have
a negative effect on the local device by consuming all available system resources (CPU and memory) in processing prefix updates.
This type of misconfiguration can occur on a peer that is not within the control of the local administrator.
Protecting the Device from External Peers
This feature can be configured to protect an individual peering session or protect all peering sessions. When this feature
is enabled and the maximum-prefix limit has been exceeded, the device will tear down the peering session, clear all routes
that were learned from the peer, and then place the peer in a penalty state for the default or user-defined time period. After
the penalty time period expires, normal peering will be reestablished.
Limiting the Number of Redistributed Prefixes
This feature can be configured to limit the number of prefixes that are accepted into the EIGRP topology table through redistribution
from the Routing Information Base (RIB). All sources of redistribution are processed cumulatively. When the maximum-prefix
limit is exceeded, all routes learned through redistribution are discarded and redistribution is suspended for the default
or user-defined time period. After the penalty time period expires, normal redistribution will occur.
Protecting the Device at the EIGRP Process Level
This feature can be configured to protect the device at the EIGRP process level. When this feature is configured at the EIGRP
process level, the maximum-prefix limit is applied to all peering sessions and to route redistribution. When the maximum-prefix
limit is exceeded, all sessions with the remote peers are torn down, all routes learned from remote peers are removed from
the topology and routing tables, all routes learned through redistribution are discarded, and redistribution and peering are
suspended for the default or user-defined time period.
Warning-Only Mode
The EIGRP Prefix Limit Support feature has two modes of operation. This feature can control peering and redistribution per
default and user-defined values or this feature can operate in warning-only mode. In warning-only mode the device will monitor
the number of prefixes learned through peering and/or redistribution but will not take any action when the maximum-prefix
limit is exceeded. Warning-only mode is activated only when the warning-only keyword is configured for any of the maximum-prefix limit commands. Only syslog messages are generated when this mode of
operation is enabled. Syslog messages can be sent to a syslog server or printed in the console. These messages can be buffered
or rate limited per standard Cisco IOS system logging configuration options.
Restart Reset and Dampening Timers and Counters
The EIGRP Prefix Limit Support feature provides two user-configurable timers, a restart counter, and a dampening mechanism.
When the maximum-prefix limit is exceeded, peering and/or redistribution is suspended for a default or user-defined time period.
If the maximum-prefix limit is exceeded too often, redistribution and/or peering will be suspended until manual intervention
is taken.
Restart Timer
The restart timer determines how long the router will wait to form an adjacency or accept redistributed routes from the RIB
after the maximum-prefix limit has been exceeded. The default restart-time period is 5 minutes.
Restart Counter
The restart counter determines the number of times a peering session can be automatically reestablished after the peering
session has been torn down or after the a redistributed routes have been cleared and relearned because the maximum-prefix
limit has been exceeded. The default restart-count limit is three.
Note
After the restart count limit has been crossed, you will need to enter the neighbor, or cleareigrpaddress-familyneighbor command to restore normal peering and redistribution.
Reset Timer
The reset timer is used to configure the device to reset the restart count to 0 after the default or configured reset-time
period has expired. This timer is designed to provide administrator with control over long-and medium-term accumulated penalties.
The default reset-time period is 15 minutes.
Dampening Mechanism
The dampening mechanism is used to apply an exponential decay penalty to the restart-time period each time the maximum-prefix
limit is exceeded. The half-life for the decay penalty is 150 percent of the default or user-defined restart-time value in
minutes. This mechanism is designed to identify and suppress unstable peers. It is disabled by default.
How to Configure the Maximum-Prefix Limit
Note
If the EIGRP process enters into a suspended (pending or down) state, the device will not establish neighborships with new
peers and thus cease to transmit and stop processing hello packets.
Configuring the Maximum Number of Prefix Accepted from Peering Sessions Autonomous System Configuration
The maximum-prefix limit can be configured for all peering sessions or individual peering sessions with the neighbormaximum-prefix(EIGRP) command. When the maximum-prefix limit is exceeded, the session with the remote peer is torn down and all routes learned
from the remote peer are removed from the topology and routing tables. The maximum-prefix limit that can be configured is
limited only by the available system resources on the device.
Note
In EIGRP,
neighbor commands have been used traditionally to configure static neighbors. In the context of this feature, however, the
neighbormaximum-prefix command can be used to configure the maximum-prefix limit for both statically configured and dynamically discovered neighbors.
Default or user-defined restart, restart-count, and reset-time values for the process-level configuration of this feature,
configured with the
maximum-prefix command, are inherited by the
redistributemaximum-prefix and
neighbormaximum-prefix command configurations by default. If a single peer is configured with the
neighbormaximum-prefix command, a process-level configuration or a configuration that is applied to all neighbors will be inherited.
Note
VRFs have been created and configured.
EIGRP peering is established through the MPLS VPN.
This task can be configured only in IPv4 VRF address family configuration mode.
When you configure the neighbormaximum-prefix command to protect a single peering session, only the maximum-prefix limit, the percentage threshold, the warning-only configuration
options can be configured. Session dampening, restart, and reset timers are configured on a global basis.
Procedure
Command or Action
Purpose
Step 1
enable
Example:
Device> enable
Enables privileged EXEC mode.
Enter your password if prompted.
Step 2
configureterminal
Example:
Device# configure terminal
Enters global configuration mode.
Step 3
routereigrpas-number
Example:
Device(config)# router eigrp 1
Enters router configuration mode and creates an EIGRP routing process.
A maximum of 30 EIGRP routing processes can be configured.
Limits the number of prefixes that are accepted from all EIGRP neighbors.
Step 8
end
Example:
Device(config-router-af)# end
Exits address family configuration mode and enters privileged EXEC mode.
Configuring the Maximum Number of Prefixes Accepted from Peering Sessions Named Configuration
The maximum-prefix limit can be configured for all peering sessions or individual peering sessions with the neighbormaximum-prefix(EIGRP) command. When the maximum-prefix limit is exceeded, the session with the remote peer is torn down and all routes learned
from the remote peer are removed from the topology and routing tables. The maximum-prefix limit that can be configured is
limited only by the available system resources on the device.
Note
In EIGRP, neighbor commands have been used traditionally to configure static neighbors. In the context of this feature, however, the neighbormaximum-prefix command can be used to configure the maximum-prefix limit for both statically configured and dynamically discovered neighbors.
Default or user-defined restart, restart-count, and reset-time values for the process-level configuration of this feature,
configured with the maximum-prefix command, are inherited by the redistributemaximum-prefix and neighbormaximum-prefix command configurations by default. If a single peer is configured with the neighbormaximum-prefix command, a process-level configuration or a configuration that is applied to all neighbors will be inherited.
Note
VRFs have been created and configured.
EIGRP peering is established through the MPLS VPN.
This task can be configured only in IPv4 VRF address family configuration mode.
When you configure the neighbormaximum-prefix command to protect a single peering session, only the maximum-prefix limit, the percentage threshold, the warning-only configuration
options can be configured. Session dampening, restart, and reset timers are configured on a global basis.
Procedure
Command or Action
Purpose
Step 1
enable
Example:
Device> enable
Enables privileged EXEC mode.
Enter your password if prompted.
Step 2
configureterminal
Example:
Device# configure terminal
Enters global configuration mode.
Step 3
routereigrpvirtual-instance-name
Example:
Device(config)# router eigrp virtual-name1
Enters router configuration mode and creates an EIGRP routing process.
A maximum of 30 EIGRP routing processes can be configured.
Limits the number of prefixes that are accepted from all EIGRP neighbors.
Step 8
exit-address-family
Example:
Device(config-router-af)# exit-address-family
Exits address family configuration mode.
Configuring the Maximum Number of Prefixes Learned Through Redistribution Autonomous System Configuration
The maximum-prefix limit can be configured for prefixes learned through redistribution with the redistributemaximum-prefix (EIGRP) command. When the maximum-prefix limit is exceeded, all routes learned from the RIB will be discarded and redistribution
will be suspended for the default or user-defined time period. The maximum-prefix limit that can be configured for redistributed
prefixes is limited only by the available system resources on the device.
Default or user-defined restart, restart-count, and reset-time values for the process-level configuration of this feature,
configured with the
maximum-prefix command, are inherited by the
redistributemaximum-prefix and
neighbormaximum-prefix command configurations by default. If a single peer is configured with the
neighbormaximum-prefix command, a process-level configuration or a configuration that is applied to all neighbors will be inherited.
Note
VRFs have been created and configured.
EIGRP peering is established through the MPLS VPN.
This task can be configured only in IPv4 VRF address family configuration mode.
Procedure
Command or Action
Purpose
Step 1
enable
Example:
Device> enable
Enables privileged EXEC mode.
Enter your password if prompted.
Step 2
configureterminal
Example:
Device# configure terminal
Enters global configuration mode.
Step 3
routereigrpas-number
Example:
Device(config)# router eigrp 1
Enters router configuration mode and creates an EIGRP routing process.
A maximum of 30 EIGRP routing processes can be configured.
Limits the number of prefixes redistributed into an EIGRP process.
Step 6
end
Example:
Device(config-router-af)# end
Exits address family configuration mode and enters privileged EXEC mode.
Configuring the Maximum Number of Prefixes Learned Through Redistribution Named Configuration
The maximum-prefix limit can be configured for prefixes learned through redistribution with the redistributemaximum-prefix(EIGRP) command. When the maximum-prefix limit is exceeded, all routes learned from the RIB will be discarded and redistribution
will be suspended for the default or user-defined time period. The maximum-prefix limit that can be configured for redistributed
prefixes is limited only by the available system resources on the device.
Default or user-defined restart, restart-count, and reset-time values for the process-level configuration of this feature,
configured with the
maximum-prefix command, are inherited by the
redistributemaximum-prefix and
neighbormaximum-prefix command configurations by default. If a single peer is configured with the
neighbormaximum-prefix command, a process-level configuration or a configuration that is applied to all neighbors will be inherited.
Note
VRFs have been created and configured.
EIGRP peering is established through the MPLS VPN.
Procedure
Command or Action
Purpose
Step 1
enable
Example:
Device> enable
Enables privileged EXEC mode.
Enter your password if prompted.
Step 2
configureterminal
Example:
Device# configure terminal
Enters global configuration mode.
Step 3
routereigrpvirtual-instance-name
Example:
Device(config)# router eigrp virtual-name1
Enters router configuration mode and creates an EIGRP routing process.
A maximum of 30 EIGRP routing processes can be configured.
Configuring the Maximum-Prefix Limit for an EIGRP Process Autonomous System Configuration
The maximum-prefix limit can be configured for an EIGRP process to limit the number prefixes that are accepted from all sources.
This task is configured with the
maximum-prefixcommand. When the maximum-prefix limit is exceeded, sessions with the remote peers are brought down and all routes learned
from remote peers are removed from the topology and routing tables. Also, all routes learned from the RIB are discarded and
redistribution is suspended for the default or user-defined time period.
Default or user-defined restart, restart-count, and reset-time values for the process-level configuration of this feature,
configured with the
maximum-prefix command, are inherited by the
redistributemaximum-prefix and
neighbormaximum-prefix command configurations by default. If a single peer is configured with the
neighbormaximum-prefix command, a process-level configuration or a configuration that is applied to all neighbors will be inherited.
Note
VRFs have been created and configured.
EIGRP peering is established through the MPLS VPN.
This task can be configured only in IPv4 VRF address family configuration mode.
Procedure
Command or Action
Purpose
Step 1
enable
Example:
Device> enable
Enables privileged EXEC mode.
Enter your password if prompted.
Step 2
configureterminal
Example:
Device# configure terminal
Enters global configuration mode.
Step 3
routereigrpas-number
Example:
Device(config)# router eigrp 1
Enters router configuration mode and creates an EIGRP routing process.
A maximum of 30 EIGRP routing processes can be configured.
Step 4
address-familyipv4 [unicast]
vrfvrf-name
Example:
Device(config-router)# address-family ipv4 vrf RED
Enters address family configuration mode and creates a session for the VRF.
Limits the number of prefixes that are accepted under an address family by an EIGRP process.
The example configures a maximum-prefix limit of 10,000 prefixes, a reset time period of 10 minutes, a warning message to
be displayed at 80 percent of the maximum-prefix limit, and a restart time period of 2 minutes.
Step 6
end
Example:
Device(config-router-af)# end
Exits address-family configuration mode and enters privileged EXEC mode.
Configuring the Maximum-Prefix Limit for an EIGRP Process Named Configuration
The maximum-prefix limit can be configured for an EIGRP process to limit the number prefixes that are accepted from all sources.
This task is configured with the
maximum-prefixcommand. When the maximum-prefix limit is exceeded, sessions with the remote peers are brought down and all routes learned
from remote peers are removed from the topology and routing tables. Also, all routes learned from the RIB are discarded and
redistribution is suspended for the default or user-defined time period.
Default or user-defined restart, restart-count, and reset-time values for the process-level configuration of this feature,
configured with the
maximum-prefix command, are inherited by the
redistributemaximum-prefix and
neighbormaximum-prefix command configurations by default. If a single peer is configured with the
neighbormaximum-prefix command, a process-level configuration or a configuration that is applied to all neighbors will be inherited.
Note
VRFs have been created and configured.
EIGRP peering is established through the MPLS VPN.
This task can be configured only in IPv4 VRF address family configuration mode.
Procedure
Command or Action
Purpose
Step 1
enable
Example:
Device> enable
Enables privileged EXEC mode.
Enter your password if prompted.
Step 2
configureterminal
Example:
Device# configure terminal
Enters global configuration mode.
Step 3
routereigrpvirtual-instance-name
Example:
Device(config)# router eigrp virtual-name1
Creates an EIGRP routing process and enters router configuration mode.
A maximum of 30 EIGRP routing processes can be configured.
Limits the number of prefixes that are accepted under an address family by an EIGRP process.
The example configures a maximum-prefix limit of 10,000 prefixes, a reset time period of 10 minutes, a warning message to
be displayed at 80 percent of the maximum-prefix limit, and a restart time period of 2 minutes.
Device# show eigrp address-family ipv4 22 accounting
(Optional) Displays prefix accounting information for EIGRP processes.
Note
Connected and summary routes are not listed individually in the output from this
show command but are counted in the total aggregate count per process.
Example
The following is sample output from the
showeigrpaddress-familyaccounting command:
Device# show eigrp address-family ipv4 22 accounting
EIGRP-IPv4 VR(saf) Accounting for AS(22)/ID(10.0.0.1)
Total Prefix Count: 3 States: A-Adjacency, P-Pending, D-Down
State Address/Source Interface Prefix Restart Restart/
Count Count Reset(s)
A 10.0.0.2 Et0/0 2 0 0
P 10.0.2.4 Se2/0 0 2 114
D 10.0.1.3 Et0/0 0 3 0
Configuration Examples for Configuring the Maximum-Prefix Limit
Example Configuring the Maximum-Prefix Limit for a Single Peer--Autonomous System Configuration
The following example, starting in global configuration mode, configures the maximum-prefix limit for a single peer. The maximum
limit is set to 1000 prefixes, and the warning threshold is set to 80 percent. When the maximum-prefix limit is exceeded,
the session with this peer will be torn down, all routes learned from this peer will be removed from the topology and routing
tables, and this peer will be placed in a penalty state for 5 minutes (default penalty value).
If the maximum prefix limit at process level and neighbor level is set together then the max prefix limit at process level
will take precedence. When the max prefix limit at neighbor level is set greater than the max prefix limit set at process
level, the device displays this message:
Max prefix limit at neighbor level is set to a value (%d) greater than max prefix limit at process level (%d)
Example Configuring the Maximum-Prefix Limit for a Single Peer--Named Configuration
The following example, starting in global configuration mode, configures the maximum-prefix limit for a single peer. The maximum
limit is set to 1000 prefixes, and the warning threshold is set to 80 percent. When the maximum-prefix limit is exceeded,
the session with this peer will be torn down, all routes learned from this peer will be removed from the topology and routing
tables, and this peer will be placed in a penalty state for 5 minutes (default penalty value).
If the maximum prefix limit at process level and neighbor level is set together then the max prefix limit at process level
will take precedence.When the max prefix limit at neighbor level is set greater than the max prefix limit set at process level,
the device displays this message:
Max prefix limit at neighbor level is set to a value (%d) greater than max prefix limit at process level (%d)
Example Configuring the Maximum-Prefix Limit for All Peers--Autonomous System Configuration
The following example, starting in global configuration mode, configures the maximum-prefix limit for all peers. The maximum
limit is set to 10,000 prefixes, the warning threshold is set to 90 percent, the restart timer is set to 4 minutes, a decay
penalty is configured for the restart timer with the dampenedkeyword, and all timers are configured to be reset to 0 every 60 minutes. When the maximum-prefix limit is exceeded, all peering
sessions will be torn down, all routes learned from all peers will be removed from the topology and routing tables, and all
peers will be placed in a penalty state for 4 minutes (user-defined penalty value). A dampening exponential decay penalty
will also be applied.
If the maximum prefix limit at process level and neighbor level is set together then the max prefix limit at process level
will take precedence. When the max prefix limit at neighbor level is set greater than the max prefix limit set at process
level, the device displays this message:
Max prefix limit at neighbor level is set to a value (%d) greater than max prefix limit at process level (%d)
Example Configuring the Maximum-Prefix Limit for All Peers--Named Configuration
The following example, starting in global configuration mode, configures the maximum-prefix limit for all peers. The maximum
limit is set to 10,000 prefixes, the warning threshold is set to 90 percent, the restart timer is set to 4 minutes, a decay
penalty is configured for the restart timer with the dampenedkeyword, and all timers are configured to be reset to 0 every 60 minutes. When the maximum-prefix limit is exceeded, all peering
sessions will be torn down, all routes learned from all peers will be removed from the topology and routing tables, and all
peers will be placed in a penalty state for 4 minutes (user-defined penalty value). A dampening exponential decay penalty
will also be applied.
If the maximum prefix limit at process level and neighbor level is set together then the max prefix limit at process level
will take precedence. When the max prefix limit at neighbor level is set greater than the max prefix limit set at process
level, the device displays this message:
Max prefix limit at neighbor level is set to a value (%d) greater than max prefix limit at process level (%d)
Example Configuring the Maximum-Prefix Limit for Redistributed Routes--Autonomous System Configuration
The following example, starting in global configuration mode, configures the maximum-prefix limit for routes learned through
redistribution. The maximum limit is set to 5000 prefixes and the warning threshold is set to 95 percent. When the number
of prefixes learned through redistribution reaches 4750 (95 percent of 5000), warning messages will be displayed in the console.
Because the warning-only keyword is configured, the topology and routing tables will not be cleared and route redistribution will not be placed in
a penalty state.
When the maximum prefix limit is configured at both the process level and redistribution level, the limit set at the process
level will take precedence. In cases where the max prefix limit at redistribution level is set greater than the max prefix
limit set at process level, the device displays this message:
Max prefix limit at redistribute level is set to a value (%d) greater than max prefix limit at process level (%d)
Example Configuring the Maximum-Prefix Limit for Redistributed Routes--Named Configuration
The following example, starting in global configuration mode, configures the maximum-prefix limit for routes learned through
redistribution. The maximum limit is set to 5000 prefixes and the warning threshold is set to 95 percent. When the number
of prefixes learned through redistribution reaches 4750 (95 percent of 5000), warning messages will be displayed in the console.
Because the warning-only keyword is configured, the topology and routing tables will not be cleared and route redistribution will not be placed in
a penalty state.
When the maximum prefix limit is configured at both the process level and redistribution level, the limit set at the process
level will take precedence. In cases where the max prefix limit at redistribution level is set greater than the max prefix
limit set at process level, the device displays this message:
Max prefix limit at redistribute level is set to a value (%d) greater than max prefix limit at process level (%d)
Example Configuring the Maximum-Prefix Limit for an EIGRP Process--Autonomous System Configuration
The following example, starting in global configuration mode, configures the maximum-prefix limit for an EIGRP process, which
includes routes learned through redistribution and routes learned through EIGRP peering sessions. The maximum limit is set
to 50,000 prefixes. When the number of prefixes learned through redistribution reaches 37,500 (75 percent of 50,000), warning
messages will be displayed in the console.
When the maximum-prefix limit is exceeded, all peering sessions will be reset, the topology and routing tables will be cleared,
and redistributed routes and all peering sessions will be placed in a penalty state.
Device(config)# router eigrp 100
Device(config-router)# address-family ipv4 vrf RED
Device(config-router-af)# maximum-prefix 50000
Device(config-router-af)# end
Note
When the max prefix limit at process level is set lower than the max prefix limit set at redistribute level, the device displays
this message:
Max prefix limit at process level is set to a value (%d) lower than max prefix limit at redistribute level (%d)
When the max prefix limit at process level is set lower than the max prefix limit set at neighbor level, the device displays
this message:
(%d) lower than max prefix limit at neighbor level (%d)
Example Configuring the Maximum-Prefix Limit for an EIGRP Process--Named Configuration
The following example, starting in global configuration mode, configures the maximum-prefix limit for an EIGRP process, which
includes routes learned through redistribution and routes learned through EIGRP peering sessions. The maximum limit is set
to 50,000 prefixes. When the number of prefixes learned through redistribution reaches 37,500 (75 percent of 50,000), warning
messages will be displayed in the console.
When the maximum-prefix limit is exceeded, all peering sessions will be reset, the topology and routing tables will be cleared,
and redistributed routes and all peering sessions will be placed in a penalty state.
When the max prefix limit at process level is set lower than the max prefix limit set at redistribute level, the device displays
this message:
Max prefix limit at process level is set to a value (%d) lower than max prefix limit at redistribute level (%d)
When the max prefix limit at process level is set lower than the max prefix limit set at neighbor level, the device displays
this message:
(%d) lower than max prefix limit at neighbor level (%d)
Feature History for Configuring EIGRP Prefix Limit Support
This table provides release and related information for the features explained in this module. These features are available
in all the releases subsequent to the one they were introduced in, unless noted otherwise.
Table 1. Feature History for Configuring EIGRP Prefix Limit Support
Release
Feature
Feature Information
Cisco IOS XE Everest 16.5.1a
Configuring EIGRP Prefix Limit Support
The EIGRP Prefix Limit Support feature introduces the capability to limit the number of prefixes per VRF that are accepted
from a specific peer or to limit all prefixes that are accepted by an Enhanced Interior Gateway Routing Protocol (EIGRP) process
through peering and redistribution.
Use the Cisco Feature Navigator to find information about platform and software image support. To access Cisco Feature Navigator,
go to https://cfnng.cisco.com/.