# Traffic Statistics per VLAN

Question asked by Shinomoto Asakura | Score: 1 | Tags: vlan, huawei, traffic, statistics

## Question

I'm using Huawei s6720 and I would like to monitor traffic per vlan, example: 

```
`# 
Interface GigabitEthernet0/0/6 
port link-type trunk 
port trunk allow-pass vlan 200 300 400 500 
# 
`

I can verify the traffic on interface, however I don't know the traffic on VLANs.

Thanks in advance