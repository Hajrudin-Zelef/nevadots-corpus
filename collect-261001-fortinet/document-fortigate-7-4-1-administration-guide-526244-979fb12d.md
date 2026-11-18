Interface policies
Interface policies
Interface policies are implemented before the security policies and are only flow-based. They are configured in the CLI.
This feature allows you to attach a set of IPS policies with the interface instead of the forwarding path, so packets can be delivered to IPS before entering the firewall. This feature is used for following IPS deployments:
- 
                                                    One-Arm: By defining interface policies with IPS and DoS anomaly checks and enabling sniff-mode on the interface, the interface can be used for one-arm IDS.
- 
                                                    IPv6 IPS: IPS inspection can be enabled through interface IPv6 policy.
- 
                                                    Scan traffic that is destined to the FortiGate.
- 
                                                    Scan and log traffic that are silently dropped or flooded by Firewall or Multicast traffic.
IPS sensors can be assigned to an interface policy. Both incoming and outgoing packets are inspected by IPS sensor (signature).
To configure an interface policy:
config firewall interface-policy
    edit 1
        set status enable
        set comments 'test interface policy #1'
        set logtraffic utm
        set interface "port2"
        set srcaddr all
        set dstaddr all
        set service "ALL"
        set application-list-status disable
        set ips-sensor-status disable
        set dsri disable
        set av-profile-status enable
        set av-profile default
        set webfilter-profile-status disable
    next
end