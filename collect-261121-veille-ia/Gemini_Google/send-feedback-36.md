---
id: collect-261121-veille-ia/gemini-google/send-feedback-36
title: "Send Feedback"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["wisp", "edit", "next", "server", "filter", "websense", "send", "profile"]
source: https://docs.Fortinet.com/document/fortigate/7.4.5/administration-guide/185370/websense-integrated-services-protocol
source_anchor: ""
source_lines: [1, 59]
section: "Gemini_Google"
---
# Websense Integrated Services Protocol

# Websense Integrated Services Protocol

Websense Integrated Services Protocol (WISP) is supported on the FortGate, which allows the firewall to send traffic to the third-party web filtering service for rating and approval checking.

When WISP is enabled, the FortiGate maintains a pool of TCP connections to the WISP server. The TCP connections are used to forward HTTP request information and log information to the WISP server and receive policy decisions.

When a WISP server is used in a web filter profile, in flow or proxy mode, the following web filter scanning priority sequence is used:

1. 
                                                    Local URL filter
2. 
                                                    Websense web filtering service
3. 
                                                    FortiGuard web filtering service

The following example uses a WISP server configured in a flow mode web filter profile.

###### To use a WISP server in flow mode:

1. 
                                                    Configure the WISP servers: ```
config web-proxy wisp
    edit "wisp1"
        set server-ip 10.2.3.4
    next
    edit "wisp2"
        set server-ip 10.2.3.5
    next
    edit "wisp3"
        set server-ip 192.168.1.2
    next
    edit "wisp4"
        set server-ip 192.168.3.4
    next
end
```
2. 
                                                    Configure the web filter profile: ```
config webfilter profile
    edit "webfilter_flowbase"
        set feature-set flow
        config ftgd-wf
            unset options
            config filters
                edit 64
                    set category 64
                    set action block
                next
            end
        end
        set wisp enable
        set wisp-servers "wisp1" "wisp2"
        set wisp-algorithm {primary-secondary | round-robin | auto-learning}
        set log-all-url enable
    next
end
```
