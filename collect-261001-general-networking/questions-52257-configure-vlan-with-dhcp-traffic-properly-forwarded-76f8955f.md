I have a windows DHCP server in VLAN 10 and I have wireless clients in VLAN 288. The VLANs are defined in my Juniper EX-4600 and the VLANs are tagged/trunked all the way to the Meraki APs.
[DHCP]---[EX-4600]---[EX-4300]--[HP5130]---[Meraki APs]
There are VLANs that are working and VLANs that do not and they all seem to have the same configuration with the exception of the subnet.
The only thing that I have found in searching for a solution was on the juniper KB that said to configure IP helpers, but none of the other VLANs have that and they work, so I don't think that is the solution.
## Last commit: 2018-07-26 01:20:45 UTC by root
version 14.1X53-D40.8;
system {
    processes {
        dhcp-service {
            traceoptions {
                file dhcp_logfile size 10m;
                level all;
                flag all;
            }
        }
        app-engine-virtual-machine-management-service {
            traceoptions {
                level notice;
                flag all;
            }
        }
    }
}
chassis {
    redundancy {
        graceful-switchover;
    }
}
interfaces {
    interface-range VLAN4 {
        member-range ge-1/0/5 to ge-1/0/11;
        unit 0 {
            family ethernet-switching {
                vlan {
                    members 4;
                }
            }
        }
    }
    xe-0/0/23 {
        ether-options {
            flow-control;
        }
        unit 0 {
            family ethernet-switching {
                interface-mode trunk;
                vlan {
                    members [ 1 3 10 100 140-141 172 176 210-213 230-232 234 236 238 270 280 288 ];
                }
                storm-control default;
            }
        }
    }
    em1 {
        unit 0 {
            family inet;
        }
    }
    irb {
        unit 0 {
            family inet;
        }
        unit 1 {
            family inet {
                address 10.1.1.100/24;
            }
        }
        unit 2 {
            family inet {
                address 192.168.11.1/24;
            }
        }
        unit 3 {
            family inet {
                address 192.168.10.1/24;
            }
        }
        unit 4 {
            family inet {
                address 192.168.9.1/24;
            }
        }
        unit 5 {
            family inet {
                address 192.168.15.1/24;
            }
        }
        unit 6 {
            family inet {
                address 192.168.35.1/24;
            }
        }
        unit 8 {
            family inet {
                address 192.168.36.1/24;
            }
        }
        unit 9 {
            family inet {
                address 192.168.37.1/24;
            }
        }
        unit 10 {
            family inet {
                address 192.168.75.2/24;
            }
        }
        unit 11 {
            family inet {
                address 192.168.76.1/24;
            }
        }
        unit 15 {
            family inet {
                address 192.168.8.1/24;
            }
        }
        unit 20 {
            family inet {
                address 10.0.0.1/24;
            }
        }
        unit 100 {
            family inet {
                address 192.168.100.1/24;
            }
        }
        unit 111 {
            family inet {
                address 192.168.111.6/24;
            }
        }
        unit 130 {
            family inet {
                address 10.1.30.1/23;
            }
        }
        unit 132 {
            family inet {
                address 10.1.32.1/24;
            }
        }
        unit 133 {
            family inet {
                address 10.1.33.1/24;
            }
        }
        unit 136 {
            family inet {
                address 10.1.36.1/22;
            }
        }
        unit 138 {
            family inet {
                address 10.1.56.1/21;
            }
        }
        unit 140 {
            family inet {
                address 10.1.40.1/24;
            }
        }
        unit 141 {
            family inet {
                address 172.16.41.142/29;
            }
        }
        unit 170 {
            family inet {
                address 10.1.72.1/22;
            }
        }
        unit 171 {
            family inet {
                address 10.1.76.1/24;
            }
        }
        unit 172 {
            family inet {
                address 172.26.88.10/21;
            }
        }
        unit 176 {
            family inet {
                address 172.16.41.177/29;
            }
        }
        unit 180 {
            family inet {
                address 10.1.80.1/23;
            }
        }
        unit 188 {
            family inet {
                address 10.1.88.1/21;
            }
        }
        unit 210 {
            family inet {
                address 10.41.10.1/24;
            }
        }
        unit 211 {
            family inet {
                address 10.41.11.1/24;
            }
        }
        unit 212 {
            family inet {
                address 10.41.12.1/24;
            }
        }
        unit 213 {
            family inet {
                address 10.41.13.1/24;
            }
        }
        unit 230 {
            family inet {
                address 10.41.30.1/24;
            }
        }
        unit 231 {
            family inet {
                address 10.41.31.1/24;
            }
        }
        unit 232 {
            family inet {
                address 10.41.32.1/24;
            }
        }
        unit 234 {
            family inet {
                address 10.41.34.1/23;
            }
        }
        unit 236 {
            family inet {
                address 10.41.36.1/23;
            }
        }
        unit 238 {
            family inet {
                address 10.41.56.1/21;
            }
        }
        unit 270 {
            family inet {
                address 10.41.72.1/22;
            }
        }
        unit 271 {
            family inet {
                address 10.41.76.1/24;
            }
        }
        unit 280 {
            family inet {
                address 10.41.80.1/23;
            }
        }
        unit 288 {
            family inet {
                address 10.41.88.1/21;
            }
        }
        unit 311 {
            family inet {
                address 10.103.11.1/24;
            }
        }
        unit 331 {
            family inet {
                address 10.103.31.1/24;
            }
        }
        unit 332 {
            family inet {
                address 10.103.32.1/24;
            }
        }
        unit 334 {
            family inet {
                address 10.103.34.1/23;
            }
        }
        unit 336 {
            family inet {
                address 10.103.36.1/23;
            }
        }
        unit 338 {
            family inet {
                address 10.103.56.1/21;
            }
        }
        unit 370 {
            family inet {
                address 10.103.72.1/22;
            }
        }
        unit 371 {
            family inet {
                address 10.103.76.1/24;
            }
        }
        unit 380 {
            family inet {
                address 10.103.80.1/23;
            }
        }
        unit 388 {
            family inet {
                address 10.103.88.1/21;
            }
        }
    }
    vme {
        unit 0 {
            family inet;
        }
    }
}
snmp {
    name HSServerRoom-4600;
    description HSServerRoom-4600;
    location "HS Server Room";
    contact "jgaspard@kirbyvillecisd.org";
    client-list list0 {
        192.168.75.0/24;
    }
    community public {
        authorization read-only;
        client-list-name list0;
    }
    trap-group kcisd-traps {
        destination-port 162;
        targets {
            192.168.75.29;
        }
    }
}
forwarding-options {
    storm-control-profiles default {
        all;
    }
    analyzer {
        INTERNET_IN {
            input {
                ingress {
                    interface ge-1/0/0.0;
                }
                egress {
                    interface ge-1/0/0.0;
                }
            }
            output {
                interface ge-1/0/2.0;
            }
        }
    }
    dhcp-relay {
        server-group {
            dhcp_servers {
                192.168.75.12;
            }
        }
        group group1 {
            active-server-group dhcp_servers;
            interface irb.1;
            interface irb.2;
            interface irb.3;
            interface irb.4;
            interface irb.5;
            interface irb.6;
            interface irb.8;
            interface irb.9;
            interface irb.10;
            interface irb.11;
            interface irb.15;
            interface irb.100;
            interface irb.111;
            interface irb.130;
            interface irb.132;
            interface irb.133;
            interface irb.136;
            interface irb.138;
            interface irb.170;
            interface irb.180;
            interface irb.188;
            interface irb.210;
            interface irb.211;
            interface irb.212;
            interface irb.213;
            interface irb.230;
            interface irb.231;
            interface irb.232;
            interface irb.234;
            interface irb.236;
            interface irb.238;
            interface irb.270;
            interface irb.280;
            interface irb.288;
            interface irb.311;
            interface irb.331;
            interface irb.332;
            interface irb.334;
            interface irb.338;
            interface irb.370;
            interface irb.380;
            interface irb.388;
        }
    }
}
routing-options {
    nonstop-routing;
    static {
        route 0.0.0.0/0 next-hop 192.168.75.13;
        route 172.25.0.18/32 next-hop 172.16.41.182;
        route 172.25.0.19/32 next-hop 172.16.41.182;
    }
    router-id 192.168.100.1;
}
protocols {
    ospf {
        area 0.0.0.0 {
            interface irb.10;
            interface irb.1 {
                passive;
            }
            interface irb.4 {
                passive;
            }
            interface irb.2 {
                passive;
            }
            interface irb.5 {
                passive;
            }
            interface irb.6 {
                passive;
            }
            interface irb.8 {
                passive;
            }
            interface irb.9 {
                passive;
            }
            interface irb.15 {
                passive;
            }
            interface irb.111 {
                passive;
            }
            interface irb.100 {
                passive;
            }
            interface irb.172 {
                passive;
            }
            interface irb.141 {
                passive;
            }
            interface irb.11 {
                passive;
            }
            interface irb.130 {
                passive;
            }
            interface irb.311 {
                passive;
            }
            interface irb.140 {
                passive;
            }
            interface irb.133 {
                passive;
            }
            interface irb.132 {
                passive;
            }
            interface irb.176 {
                passive;
            }
            interface irb.331 {
                passive;
            }
            interface irb.332 {
                passive;
            }
            interface irb.334 {
                passive;
            }
            interface irb.136 {
                passive;
            }
            interface irb.20 {
                passive;
            }
            interface irb.212 {
                passive;
            }
            interface irb.3 {
                passive;
            }
            interface irb.211 {
                passive;
            }
            interface irb.213 {
                passive;
            }
            interface irb.230 {
                passive;
            }
            interface irb.231 {
                passive;
            }
            interface irb.232 {
                passive;
            }
            interface irb.234 {
                passive;
            }
            interface irb.236 {
                passive;
            }
            interface irb.210 {
                passive;
            }
            interface irb.138 {
                passive;
            }
            interface irb.238 {
                passive;
            }
            interface irb.338 {
                passive;
            }
            interface irb.170 {
                passive;
            }
            interface irb.180 {
                passive;
            }
            interface irb.188 {
                passive;
            }
            interface irb.288 {
                passive;
            }
            interface irb.388 {
                passive;
            }
            interface irb.380 {
                passive;
            }
            interface irb.370 {
                passive;
            }
            interface irb.270 {
                passive;
            }
            interface irb.280 {
                passive;
            }
        }
    }
    lldp {
        interface all;
    }
    lldp-med {
        interface all;
    }
    igmp-snooping {
        vlan default;
    }
    inactive: rstp {
        interface xe-0/0/0;
        interface xe-0/0/1;
        interface xe-0/0/2;
        interface xe-0/0/3;
        interface xe-0/0/4;
        interface xe-0/0/5;
        interface xe-0/0/6;
        interface xe-0/0/7;
        interface xe-0/0/8;
        interface xe-0/0/9;
        interface xe-0/0/10;
        interface xe-0/0/11;
        interface xe-0/0/12;
        interface xe-0/0/13;
        interface xe-0/0/14;
        interface xe-0/0/15;
        interface xe-0/0/16;
        interface xe-0/0/17;
        interface xe-0/0/18;
        interface xe-0/0/19;
        interface xe-0/0/20;
        interface xe-0/0/21;
        interface xe-0/0/22;
        interface xe-0/0/23;
        interface et-0/0/24;
        interface xe-0/0/24:0;
        interface xe-0/0/24:1;
        interface xe-0/0/24:2;
        interface xe-0/0/24:3;
        interface et-0/0/25;
        interface xe-0/0/25:0;
        interface xe-0/0/25:1;
        interface xe-0/0/25:2;
        interface xe-0/0/25:3;
        interface et-0/0/26;
        interface xe-0/0/26:0;
        interface xe-0/0/26:1;
        interface xe-0/0/26:2;
        interface xe-0/0/26:3;
        interface et-0/0/27;
        interface xe-0/0/27:0;
        interface xe-0/0/27:1;
        interface xe-0/0/27:2;
        interface xe-0/0/27:3;
    }
}
virtual-chassis {
    preprovisioned;
    member 0 {
        role routing-engine;
        serial-number TC3716430102;
    }
    member 1 {
        role routing-engine;
        serial-number PE3716350055;
    }
    member 2 {
        role line-card;
        serial-number PE3716350466;
    }
}
vlans {
    Backbone {
        vlan-id 10;
        l3-interface irb.10;
    }
    Central {
        vlan-id 5;
        l3-interface irb.5;
    }
    DEFAULT_VLAN {
        vlan-id 1;
        l3-interface irb.1;
    }
    DLVIDEO {
        vlan-id 141;
        l3-interface irb.141;
    }
    ES4thLab {
        vlan-id 311;
        l3-interface irb.311;
    }
    ESFac {
        vlan-id 380;
        l3-interface irb.380;
    }
    ESFacBYOD {
        vlan-id 388;
        l3-interface irb.388;
    }
    ESStu {
        vlan-id 370;
        l3-interface irb.370;
    }
    ES_BYOD {
        vlan-id 331;
        l3-interface irb.331;
    }
    ES_BYOD_Fac {
        vlan-id 332;
        l3-interface irb.332;
    }
    ES_Main_Wifi {
        vlan-id 334;
        l3-interface irb.334;
    }
    Fog {
        vlan-id 11;
        l3-interface irb.11;
    }
    HSFac {
        vlan-id 180;
        l3-interface irb.180;
    }
    HSFacBYOD {
        vlan-id 188;
        l3-interface irb.188;
    }
    HSPhone {
        vlan-id 140;
        l3-interface irb.140;
    }
    HSStu {
        vlan-id 170;
        l3-interface irb.170;
    }
    HS_BYOD_Fac {
        vlan-id 133;
        l3-interface irb.133;
    }
    HS_BYOD_Stu {
        vlan-id 132;
        l3-interface irb.132;
    }
    HighSchool {
        vlan-id 4;
        l3-interface irb.4;
    }
    JH {
        vlan-id 3;
        l3-interface irb.3;
    }
    JH107 {
        vlan-id 211;
        l3-interface irb.211;
    }
    JH116 {
        vlan-id 212;
        l3-interface irb.212;
    }
    JHFac {
        vlan-id 280;
        l3-interface irb.280;
    }
    JHFacBYOD {
        vlan-id 288;
        l3-interface irb.288;
    }
    JHOffice {
        vlan-id 213;
        l3-interface irb.213;
    }
    JHStu {
        vlan-id 270;
        l3-interface irb.270;
    }
    JH_BYOD_Fac {
        vlan-id 232;
        l3-interface irb.232;
    }
    JH_BYOD_Stu {
        vlan-id 231;
        l3-interface irb.231;
    }
    JH_Data {
        vlan-id 210;
        l3-interface irb.210;
    }
    JH_Faculty {
        vlan-id 234;
        l3-interface irb.234;
    }
    JH_Student {
        vlan-id 236;
        l3-interface irb.236;
    }
    JH_Wifi_APs {
        vlan-id 230;
        l3-interface irb.230;
    }
    KCISDFac_ES {
        vlan-id 338;
        l3-interface irb.338;
    }
    KCISDFac_HS {
        vlan-id 138;
        l3-interface irb.138;
    }
    KCISDFac_JH {
        vlan-id 238;
        l3-interface irb.238;
    }
    KHFac {
        vlan-id 130;
        l3-interface irb.130;
    }
    KHStu {
        vlan-id 136;
        l3-interface irb.136;
    }
    Scale {
        vlan-id 20;
        l3-interface irb.20;
    }
    SciAutoAGath {
        vlan-id 6;
        l3-interface irb.6;
    }
    VLAN172 {
        vlan-id 172;
        l3-interface irb.172;
    }
    VLAN176 {
        vlan-id 176;
        l3-interface irb.176;
    }
    VideoSur {
        vlan-id 100;
        l3-interface irb.100;
    }
    busbarn {
        vlan-id 9;
        l3-interface irb.9;
    }
    elem456 {
        vlan-id 15;
        l3-interface irb.15;
    }
    elementary {
        vlan-id 2;
        l3-interface irb.2;
    }
    hsmacser {
        vlan-id 8;
        l3-interface irb.8;
    }
    weather {
        vlan-id 111;
        l3-interface irb.111;
    }
}
The juniper at the junior high MDF is on port xe-0/0/23.
root@HSServerRoom-4600> show dhcp relay statistics
Packets dropped:
    Total                      127849
    Bootp packets              126744
    Interface not configured   588
    Bad UDP checksum           1
    No binding found           512
    dhcp-service total         4
Messages received:
    BOOTREQUEST                5571847
    DHCPDECLINE                142
    DHCPDISCOVER               1751818
    DHCPINFORM                 996327
    DHCPRELEASE                99599
    DHCPREQUEST                2723961
Messages sent:
    BOOTREPLY                  3999872
    DHCPOFFER                  968432
    DHCPACK                    3030802
    DHCPNAK                    638
    DHCPFORCERENEW             0
Packets forwarded:
    Total                      5737
    BOOTREQUEST                0
    BOOTREPLY                  5737