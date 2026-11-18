If no separate DHCP server is available the Huawei router/switch can also be configured as a DHCP server. There are two ways in which this can be done which are described here.
Central DHCP servers are often used which are not in the same network as the DHCP clients. In this case an IP-Helper/DHCP relay must be configured. However if the DHCP server should be present locally this can be configured directly on the Huawei device. It can be done in two ways: on the interface or using the a DHCP pool.
Option 1: Interface based
| 1. | Activate DHCP service [HUAWEI] dhcp enable | 
| 2. | Configure DHCP interface mode [HUAWEI-GigabitEthernet0/0/6] dhcp select interface | 
| 3. | Configure DNS server (e.g. 192.168.1.100) [HUAWEI-GigabitEthernet0/0/6] dhcp server dns-list 192.168.1.100 | 
| 4. | Configure DHCP exclusion (IP addresses that should not be allocated, eg. all addresses from 10.0.0.1 to 10.0.0.10) [HUAWEI-GigabitEthernet0/0/6] dhcp server excluded-ip-address 10.0.0.1 10.0.0.10 | 
| 5. | Configure DHCP lease time (default is one day if nothing is configured) [HUAWEI-GigabitEthernet0/0/6] dhcp server lease day 2 | 
Option 2: DHCP pool
| 1. | Activate DHCP service [HUAWEI] dhcp enable | 
| 2. | Create DHCP pool [HUAWEI] ip pool DHCP-POOLInfo: It is successful to create an IP address pool. | 
| 3. | Configure DHCP exclusion (IP addresses that should not be allocated, eg. all addresses from 10.0.0.1 to 10.0.0.10) [HUAWEI-ip-pool-DHCP-POOL] network 10.0.0.0 mask 24[HUAWEI-ip-pool-DHCP-POOL] gateway-list 10.0.0.1[HUAWEI-ip-pool-DHCP-POOL] dns-list 192.168.1.100[HUAWEI-ip-pool-DHCP-POOL] lease day 2 | 
| 4. | Configure interface to allocate IP addresses from DHCP pool [HUAWEI-GigabitEthernet0/0/6] dhcp select global | 
➡️ Both options lead to the same result - it's up to you which option you choose. Either you have the DHCP configuration at a glance on the interface or in a separate configuration in the DHCP pool.
Leave a Comment