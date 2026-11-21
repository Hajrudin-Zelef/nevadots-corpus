---
id: collect-261121-veille-ia/gemini-google/on-this-page-3
title: "On this page"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["traffic", "shaping", "class", "bandwidth", "interface", "outgoing", "group", "application"]
source: https://docs.fortinet.com/document/fortigate/7.6.1/administration-guide/647914/interface-based-traffic-shaping-profile
source_anchor: ""
source_lines: [1, 275]
section: "Gemini_Google"
---
# Interface-based traffic shaping profile

# Interface-based traffic shaping profile

A traffic shaping policy can be used for interface-based traffic shaping by organizing traffic into 30 class IDs. The shaping profile defines the percentage of the interface bandwidth that is allocated to each class. Each traffic class ID is shaped to the assigned speed according to the outgoing bandwidth limit configured to the interface.

A shaping policy classifies traffic and organizes it into different class IDs, based on matching criteria. For traffic matching a criteria, you can choose to put it into 30 different shaping classes, identified by class ID 2 - 31.

|  | When NPU offloading is enabled on the NP6, SoC3, or SoC4 platforms, the class ID limit for egress traffic is 2 - 15. Setting the egress traffic class ID outside of these limits can result in unexpected behavior. If NPU offloading is disabled, or enabled on the NP7 platform, the class ID limit for egress traffic is 2 - 31. | 

You must select an outgoing interface for the traffic. The shaping policy is only applied when the traffic goes to one of the selected outgoing interfaces.

| Criterion | Description | 
|---|---|
| **Source** |  | 
| **Destination** |  | 
| **Schedule** | Match the current date and time to the selected schedule. You can select a one-time schedule, recurring schedule, or schedule group. This setting is optional. | 
| **Service** | Match the service of the traffic to the selected service or service group. | 
| **Application** | Match the application of the traffic to the selected application, application category, or application group. Application control must be enabled in the related firewall policy to know the application of the traffic. See Application control for more information. | 
| **URL category** | Match the URL of the traffic to the selected URL category. Web filter must be enabled in the related firewall policy to know the URL of the traffic. See Web filter for more information. | 

|  | When multiple items are selected in one criterion, it is considered a match when traffic matches any one of them. | 

Shaping profiles define how different shaping classes of traffic are prioritized. For each class, you can define three prioritization strategies: guaranteed bandwidth, maximum bandwidth, and priority.

For each shaping profile, a default shaping class must be defined. Traffic is prioritized based on the default shaping group in the following two circumstances:

- 
                                                    All traffic to the outgoing interface that does not match to any shaping policy
- 
                                                    Traffic with a shaping group that is not defined in a shaping profile

| Prioritization strategy | Description | 
|---|---|
| **Guaranteed bandwidth** | The percentage of the link speed that is reserved for the shaping group. The total guaranteed bandwidth for all shaping groups cannot exceed 100%. | 
| **Maximum bandwidth** | The maximum percentage of the link speed that the shaping group can use. | 
| **Priority** | The shaping class priority: top, critical, high, medium, or low. When groups are competing for bandwidth on the interface, the group with the higher priority wins. | 

Traffic shaping is accomplished by configuring the outgoing bandwidth and outgoing shaping profile on an interface. The shaping profile uses the outgoing bandwidth of the interface as the maximum link speed, and it only works when the outgoing bandwidth is configured.

This example shows how to apply interface-based traffic shaping to web and file accessing traffic according to a schedule:

- 
                                                    The link speed of the wan1 interface is 10 Mb/s.
- 
                                                    File access can use up to 2 Mb/s from 8:00 AM to 6:00 PM.
- 
                                                    Web access can use 8 Mb/s from 8:00 AM to 6:00 PM.


### Putting the traffic into shaping classes

###### To create a recurring schedule in the GUI:

1. 
                                                    Go to *Policy & Objects > Schedules* and navigate to the*Recurring Schedule* tab.
2. 
                                                    Click *Create New* .
3. 
                                                    Configure a recurring schedule called *Day_Hours* for everyday from 8:00 AM to 6:00 PM.
4. 
                                                    Click *OK* .

###### To create a traffic shaping policy and class ID for the web accessing traffic in the GUI:

1. 
                                                    Go to *Policy & Objects > Traffic Shaping* , select the*Traffic Shaping Policy* tab, and click*Create New* .
2. 
                                                    Enter a name for the policy, such as *web_access_day_hours* .
3. 
                                                    Set *Outgoing interface* to*wan1* .
4. 
                                                    Enable *Schedule* and select the schedule you just created.
5. 
                                                    Set *Service* to web accessing services, such as*HTTP* and*HTTPS* .
6. 
                                                    Enable *Assign shaping class ID* .
7. 
                                                    Click the *Traffic shaping class ID* drop down then click*Create* .
8. 
                                                    Enter an integer value for the *ID* (*3* ) and a description for the*Name* , such as*Web Access* .
9. 
                                                    Click *OK* .
10. 
                                                    Select the class ID you just created for *Traffic shaping class ID* .
11. 
                                                    Configure the remaining settings as required.
12. 
                                                    Click *OK* .

###### To create a traffic shaping policy and class ID for the file accessing traffic in the GUI:

1. 
                                                    Go to *Policy & Objects > Traffic Shaping* , select the*Traffic Shaping Policy* tab, and click*Create New* .
2. 
                                                    Enter a name for the policy, such as *file_access_day_hours* .
3. 
                                                    Set *Outgoing interface* to*wan1* .
4. 
                                                    Enable *Schedule* and select the schedule you just created.
5. 
                                                    Set *Service* to file accessing services, such as*ASF3* ,*FTP* and*SMB* .
6. 
                                                    Enable *Assign shaping class ID* .
7. 
                                                    Click the *Traffic shaping class ID* drop down then click*Create* .
8. 
                                                    Enter an integer value for the *ID* (*4* ) and a description for the*Name* , such as*File Access* .
9. 
                                                    Click *OK* .
10. 
                                                    Select the class ID you just created for *Traffic shaping class ID* .
11. 
                                                    Configure the remaining settings as required.
12. 
                                                    Click *OK* .

###### To put the traffic into shaping classes in the CLI:

1. 
                                                    Create a recurring schedule: ```
config firewall schedule recurring
    edit "Day_Hours"
        set start 08:00
        set end 18:00
        set day sunday monday tuesday wednesday thursday friday saturday
    next
end
```
2. 
                                                    Create the traffic class IDs: ```
config firewall traffic-class
    edit 3
        set class-name "Web Access"
    next
    edit 4
        set class-name "File Access"
    next
end
```
3. 
                                                    Create the web and file accessing traffic shaping policies: ```
config firewall shaping-policy
    edit 2
        set name "web_access_day_hours"
        set comment "Limit web accessing traffic to 8Mb/s in day time"
        set service "HTTP" "HTTPS"
        set schedule "Day_Hours"
        set dstintf "wan1"
        set class-id 3
        set srcaddr "all"
        set dstaddr "all"
    next
    edit 3
        set name "file_access_day_hours"
        set comment "Limit file accessing traffic to 2Mb/s during the day"
        set service "AFS3" "FTP" "FTP_GET" "FTP_PUT" "NFS" "SAMBA" "SMB" "TFTP"
        set schedule "Day_Hours"
        set dstintf "wan1"
        set class-id 4
        set srcaddr "all"
        set dstaddr "all"
    next
end
```

### Allocating bandwidth to the shaping classes

A traffic shaping profile defines the guaranteed and maximum bandwidths each class receives. In this example, file access can use up to 2 Mb/s and web access can use 8 Mb/s from 8:00 AM to 6:00 PM.

###### To create a traffic shaping profile using the GUI:

1. 
                                                    Go to *Policy & Objects > Traffic Shaping* , select the*Traffic Shaping Profile* tab, and click*Create New* .
2. 
                                                    Enter a name for the profile, such as *Day_Hours_Profile* .
3. 
                                                    Configure a default traffic shaping class: This class has a high priority, meaning that when the other classes have reached their guaranteed bandwidths, this default class will use the rest of the available bandwidth. 
  1. 
                                                            In the *Traffic Shaping Classes* table click*Create New* .
  2. 
                                                            Click the *Traffic shaping class ID* drop down then click*Create* .
  3. 
                                                            Enter a name for the class, such as *Default Access* .
  4. 
                                                            Click *OK* .
  5. 
                                                            Select the class ID you just created for *Traffic shaping class ID* .
  6. 
                                                            Configure the following settings, then click *OK* :Guaranteed bandwidth 30 Maximum bandwidth 100 Priority High
4. 
                                                            
5. 
                                                    Configure a web accessing traffic shaping class: When other types of traffic are competing for bandwidth, this class is guaranteed to 6 Mb/s, or 60% of the bandwidth. 
  1. 
                                                            In the *Traffic Shaping Classes* table click*Create New* .
  2. 
                                                            Configure the following settings, then click *OK* :Traffic shaping class ID Web Access Guaranteed bandwidth 60 Maximum bandwidth 80 Priority Medium
6. 
                                                            
7. 
                                                    Configure a file accessing traffic shaping class: When other types of traffic are competing for bandwidth, this group is guaranteed to 1 Mb/s, or 10% of the bandwidth. 
  1. 
                                                            In the *Traffic Shaping Classes* table click*Create New* .
  2. 
                                                            Configure the following settings, then click *OK* :Traffic shaping class ID File Access Guaranteed bandwidth 10 Maximum bandwidth 20 Priority Medium
8. 
                                                            
9. 
                                                    Click *OK* .

###### To create a traffic shaping profile using the CLI:

```
config firewall shaping-profile
    edit "Day_Hours_Profile"
        set default-class-id 2
        config shaping-entries
            edit 1
                set class-id 2
                set guaranteed-bandwidth-percentage 30
                set maximum-bandwidth-percentage 100
            next
            edit 2
                set class-id 3
                set priority medium
                set guaranteed-bandwidth-percentage 60
                set maximum-bandwidth-percentage 80
            next
            edit 3
                set class-id 4
                set priority medium
                set guaranteed-bandwidth-percentage 10
                set maximum-bandwidth-percentage 20
            next
        end
    next
end
```
                                            ### Defining the available bandwidth on an interface

In this example, the link speed of the wan1 interface is 10 Mb/s.

###### To set the bandwidth of the wan1 interface in the GUI:

1. 
                                                    Go to *Network > Interfaces* .
2. 
                                                    Edit the wan1 interface.
3. 
                                                    Under Traffic Shaping, enable *Outbound shaping profile* and select the profile that you just created,*Day_Hours_Profile* .
4. 
                                                    Enable *Outbound Bandwidth* and set it to*10000* Kbps.
5. 
                                                    Click *OK* .

###### To set the bandwidth of the wan1 interface in the CLI:

```
config system interface
    edit "wan1"
        set egress-shaping-profile "Day_Hours_Profile"
        set outbandwidth 10000
    next
end
```
                                            ### Diagnose commands

###### To check that the specific traffic is put into the correct shaping group or class ID:

# diagnose firewall iprope list 100015

###### To check the speed limit for each class ID on an interface:

# diagnose netlink interface list wan1
