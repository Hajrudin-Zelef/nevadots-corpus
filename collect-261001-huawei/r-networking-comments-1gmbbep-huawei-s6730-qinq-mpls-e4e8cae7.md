Huawei S6730 QinQ MPLS
Hello friends,
in company where i'm working we have customers using L1 Ethernet services via DWDM (think of it like fiber cable between two offices). My managers decides to migrate all L1 10G services (from DWDM) to L2 and they will run across our MPLS network. We dont't have idea what exactly customers use (like what protocols, and what services ). So with Huawei S6730-H and QinQ all links should be migrated. My initial questions :
- 
      In S6730-H configuration guide is written - Dot1q-tunnel interfaces do not support Layer 2 multicast (i'm pretty sure we've got customers in past using multicast and they were configured on port facing customer with port link-type dot1q-tunnel port default vlan 1234 and with interface vlan 1234 mpls l2vc 1.1.1.1 1234 So if dot1q-tunnel is not supporting multicast what are other options ?
- 
      If customer use STP, LACP is it possible to tunnel all bpdu's and transport them via MPLS ? Thank you in advance
- 
      https://imgur.com/a/tr8o4zx
Section des commentaires
I suppose that’s one way to lose all your business and get sued for breach of contract
A forma de transportar esses protocolos é sendo da forma Porta Based. Entrega uma interface pra ele e o tipo de do VC tem que ser ethernet tbm
Just use VSI on the Huawei
Or l2vc