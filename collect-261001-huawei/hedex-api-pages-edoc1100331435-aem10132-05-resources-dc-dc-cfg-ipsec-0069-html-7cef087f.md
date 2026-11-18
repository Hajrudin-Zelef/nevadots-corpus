As shown in Figure 1, two gateways RouterA and RouterB are deployed in the headquarters to improve security. RouterC in the branch communicates with the headquarters through the public network.
The enterprise requires to protect traffic transmitted over the public network between the enterprise branch and headquarters.
IPSec tunnels can be set up between the branch gateways and headquarters gateway because they communicate over the Internet. The branch gateway attempts to establish an IPSec tunnel with the headquarters gateway RouterA. If the attempt fails, the branch gateway establishes an IPSec tunnel with the headquarters gateway RouterB.
Configure IP addresses for interfaces and configure static routes to ensure that there are reachable routes between two ends.
Configure an ACL to define the data flows to be protected by the IPSec tunnel.
Configure an IPSec proposal to define the traffic protection method.
Configure an IKE peer and define the attributes used for IKE negotiation.
Create an IPSec policy on RouterA, RouterB, and RouterC to determine protection methods used for protecting different types of data flows. On RouterA and RouterB, IPSec policies are created through IPSec policy templates.
Apply an IPSec policy group to an interface so that the interface can protect traffic.
# Assign an IP address to each interface on RouterA.
<Huawei> system-view
[Huawei] sysname RouterA
[RouterA] interface gigabitethernet 0/0/1
[RouterA-GigabitEthernet0/0/1] ip address 60.1.1.1 255.255.255.0
[RouterA-GigabitEthernet0/0/1] quit
[RouterA] interface gigabitethernet 0/0/2
[RouterA-GigabitEthernet0/0/2] ip address 192.168.1.2 255.255.255.0
[RouterA-GigabitEthernet0/0/2] quit
# Configure a static route to the peer on RouterA. This example assumes that the next-hop address in the route to the headquarters subnet is 60.1.1.2.
[RouterA] ip route-static 70.1.1.0 255.255.255.0 60.1.1.2
[RouterA] ip route-static 192.168.3.0 255.255.255.0 60.1.1.2
# Assign an IP address to each interface on RouterB.
<Huawei> system-view
[Huawei] sysname RouterB
[RouterB] interface gigabitethernet 0/0/1 
[RouterB-GigabitEthernet0/0/1] ip address 60.1.2.1 255.255.255.0
[RouterB-GigabitEthernet0/0/1] quit
[RouterB] interface gigabitethernet 0/0/2
[RouterB-GigabitEthernet0/0/2] ip address 192.168.1.3 255.255.255.0
[RouterB-GigabitEthernet0/0/2] quit
# Configure a static route to the peer on RouterB. This example assumes that the next-hop address in the route to the headquarters subnet is 60.1.2.2.
[RouterB] ip route-static 70.1.1.0 255.255.255.0 60.1.2.2
[RouterB] ip route-static 192.168.3.0 255.255.255.0 60.1.2.2
# Assign an IP address to each interface on RouterC.
<Huawei> system-view
[Huawei] sysname RouterC
[RouterC] interface gigabitethernet 0/0/1 
[RouterC-GigabitEthernet0/0/1] ip address 70.1.1.1 255.255.255.0
[RouterC-GigabitEthernet0/0/1] quit
[RouterC] interface gigabitethernet 0/0/2
[RouterC-GigabitEthernet0/0/2] ip address 192.168.3.2 255.255.255.0
[RouterC-GigabitEthernet0/0/2] quit
# Configure a static route to the peer on RouterC. This example assumes that the next-hop address in the route to the headquarters gateways RouterA and RouterB is 70.1.1.2.
[RouterC] ip route-static 0.0.0.0 0.0.0.0 70.1.1.2
IPSec policies are created on RouterA and RouterB through an IPSec policy template; therefore, this step is optional. If you configure an ACL on RouterA and RouterB, you must specify the destination address in the ACL rule.
# Configure an ACL on RouterC to define the data flows from subnet 192.168.3.0/24 to subnet 192.168.1.0/24.
[RouterC] acl number 3002
[RouterC-acl-adv-3002] rule permit ip source 192.168.3.0 0.0.0.255 destination 192.168.1.0 0.0.0.255
[RouterC-acl-adv-3002] quit
# Create an IPSec proposal on RouterA.
[RouterA] ipsec proposal tran1
[RouterA-ipsec-proposal-tran1] esp authentication-algorithm sha2-256
[RouterA-ipsec-proposal-tran1] esp encryption-algorithm aes-128
[RouterA-ipsec-proposal-tran1] quit
# Create an IPSec proposal on RouterB.
[RouterB] ipsec proposal tran1
[RouterB-ipsec-proposal-tran1] esp authentication-algorithm sha2-256
[RouterB-ipsec-proposal-tran1] esp encryption-algorithm aes-128
[RouterB-ipsec-proposal-tran1] quit
# Create an IPSec proposal on RouterC.
[RouterC] ipsec proposal tran1
[RouterC-ipsec-proposal-tran1] esp authentication-algorithm sha2-256
[RouterC-ipsec-proposal-tran1] esp encryption-algorithm aes-128
[RouterC-ipsec-proposal-tran1] quit
Run the display ipsec proposal command on RouterA, RouterB, and RouterC to check the configuration of the IPSec proposal. The command output on RouterA is used as an example.
[RouterA] display ipsec proposal name tran1
IPSec proposal name: tran1
 Encapsulation mode: Tunnel
 Transform         : esp-new
 ESP protocol      : Authentication SHA2-HMAC-256
                     Encryption     AES-128
# Create an IKE proposal on RouterA.
[RouterA] ike proposal 5
[RouterA-ike-proposal-5] encryption-algorithm aes-128
[RouterA-ike-proposal-5] authentication-algorithm sha2-256
[RouterA-ike-proposal-5] dh group14
[RouterA-ike-proposal-5] quit
# Create an IKE peer on RouterA.
[RouterA] ike peer rut1
[RouterA-ike-peer-rut1] version 1
[RouterA-ike-peer-rut1] undo version 2
[RouterA-ike-peer-rut1] pre-shared-key cipher YsHsjx_202206
[RouterA-ike-peer-rut1] ike-proposal 5
[RouterA-ike-peer-rut1] quit
# Create an IKE proposal on RouterB.
[RouterB] ike proposal 5
[RouterB-ike-proposal-5] encryption-algorithm aes-128
[RouterB-ike-proposal-5] authentication-algorithm sha2-256
[RouterB-ike-proposal-5] dh group14
[RouterB-ike-proposal-5] quit
# Create an IKE peer on RouterB.
[RouterB] ike peer rut1
[RouterB-ike-peer-rut1] version 1
[RouterB-ike-peer-rut1] undo version 2
[RouterB-ike-peer-rut1] pre-shared-key cipher YsHsjx_202206
[RouterB-ike-peer-rut1] ike-proposal 5
[RouterB-ike-peer-rut1] quit
RouterA and RouterB function as IKE responders, and IPSec policies are created on RouterA and RouterB through IPSec policy templates. You do not need to set remote-address.
# Create an IKE peer on RouterC.
[RouterC] ike proposal 5
[RouterC-ike-proposal-5] encryption-algorithm aes-128
[RouterC-ike-proposal-5] authentication-algorithm sha2-256
[RouterC-ike-proposal-5] dh group14
[RouterC-ike-proposal-5] quit
[RouterC] ike peer rut1
[RouterC-ike-peer-rut1] version 1
[RouterC-ike-peer-rut1] undo version 2
[RouterC-ike-peer-rut1] ike-proposal 5
[RouterC-ike-peer-rut1] pre-shared-key cipher YsHsjx_202206
[RouterC-ike-peer-rut1] remote-address 60.1.1.1
[RouterC-ike-peer-rut1] remote-address 60.1.2.1
[RouterC-ike-peer-rut1] quit
# Create an IPSec policy template on RouterA and apply the IPSec policy template to an IPSec policy.
[RouterA] ipsec policy-template use1 10
[RouterA-ipsec-policy-templet-use1-10] ike-peer rut1
[RouterA-ipsec-policy-templet-use1-10] proposal tran1
[RouterA-ipsec-policy-templet-use1-10] quit
[RouterA] ipsec policy policy1 10 isakmp template use1
# Create an IPSec policy template on RouterB and apply the IPSec policy template to an IPSec policy.
[RouterB] ipsec policy-template use1 10
[RouterB-ipsec-policy-templet-use1-10] ike-peer rut1
[RouterB-ipsec-policy-templet-use1-10] proposal tran1
[RouterB-ipsec-policy-templet-use1-10] quit
[RouterB] ipsec policy policy1 10 isakmp template use1
# Create an IPSec policy on RouterC.
[RouterC] ipsec policy policy1 10 isakmp
[RouterC-ipsec-policy-isakmp-policy1-10] ike-peer rut1
[RouterC-ipsec-policy-isakmp-policy1-10] proposal tran1
[RouterC-ipsec-policy-isakmp-policy1-10] security acl 3002
[RouterC-ipsec-policy-isakmp-policy1-10] quit
# Apply an IPSec policy group to an interface of RouterA.
[RouterA] interface gigabitethernet 0/0/1
[RouterA-GigabitEthernet0/0/1] ipsec policy policy1
[RouterA-GigabitEthernet0/0/1] quit
# Apply an IPSec policy group to an interface of RouterB.
[RouterB] interface gigabitethernet 0/0/1
[RouterB-GigabitEthernet0/0/1] ipsec policy policy1
[RouterB-GigabitEthernet0/0/1] quit
# Apply an IPSec policy group to an interface of RouterC.
[RouterC] interface gigabitethernet 0/0/1
[RouterC-GigabitEthernet0/0/1] ipsec policy policy1
[RouterC-GigabitEthernet0/0/1] quit
# After the configurations are complete, PC C can ping PC A successfully. The data transmitted between PC C and PC A is encrypted.
# Run the display ike sa command on RouterA to check information about the tunnel established with RouterC.
[RouterA] display ike sa
IKE SA information :
   Conn-ID  Peer          VPN   Flag(s)   Phase   RemoteType  RemoteID
  ---------------------------------------------------------------------------
  24366    70.1.1.1:500         RD        v1:2    IP          70.1.1.1
  24274    70.1.1.1:500         RD        v1:1    IP          70.1.1.1
                                   
  Number of IKE SA : 2
  ---------------------------------------------------------------------------
                                                           
  Flag Description:           
  RD--READY   ST--STAYALIVE   RL--REPLACED   FD--FADING   TO--TIMEOUT
  HRT--HEARTBEAT   LKG--LAST KNOWN GOOD SEQ NO.   BCK--BACKED UP
  M--ACTIVE   S--STANDBY   A--ALONE  NEG--NEGOTIATING   
# Run the display ike sa command on RouterC. The command output is displayed as follows:
[RouterC] display ike sa
IKE SA information :
  Conn-ID  Peer          VPN   Flag(s)   Phase   RemoteType  RemoteID
  --------------------------------------------------------------------------
   937    60.1.1.1:500         RD|ST     v1:2    IP          60.1.1.1
   936    60.1.1.1:500         RD|ST     v1:1    IP          60.1.1.1
                                   
  Number of IKE SA : 2
  --------------------------------------------------------------------------
                                                           
  Flag Description:           
  RD--READY   ST--STAYALIVE   RL--REPLACED   FD--FADING   TO--TIMEOUT
  HRT--HEARTBEAT   LKG--LAST KNOWN GOOD SEQ NO.   BCK--BACKED UP
  M--ACTIVE   S--STANDBY   A--ALONE  NEG--NEGOTIATING   
Configuration file of RouterA
#
 sysname RouterA
#
ipsec proposal tran1
 esp authentication-algorithm sha2-256   
 esp encryption-algorithm aes-128
#
ike proposal 5
 encryption-algorithm aes-128
 dh group14
 authentication-algorithm sha2-256
 authentication-method pre-share
 integrity-algorithm hmac-sha2-256
 prf hmac-sha2-256
#
ike peer rut1
 version 1
 pre-shared-key cipher %^%#JvZxR2g8c;a9~FPN~n'$7`DEV&=G(=Et02P/%\*!%^%#
 ike-proposal 5
#
ipsec policy-template use1 10
 ike-peer rut1
 proposal tran1
#
ipsec policy policy1 10 isakmp template use1
#
interface GigabitEthernet0/0/1
 ip address 60.1.1.1 255.255.255.0
 ipsec policy policy1
#
interface GigabitEthernet0/0/2
 ip address 192.168.1.2 255.255.255.0
#
ip route-static 70.1.1.0 255.255.255.0 60.1.1.2
ip route-static 192.168.3.0 255.255.255.0 60.1.1.2
#
return
Configuration file of RouterB
#
 sysname RouterB
#
ipsec proposal tran1
 esp authentication-algorithm sha2-256   
 esp encryption-algorithm aes-128
#
ike proposal 5
 encryption-algorithm aes-128
 dh group14
 authentication-algorithm sha2-256
 authentication-method pre-share
 integrity-algorithm hmac-sha2-256
 prf hmac-sha2-256
#
ike peer rut1
 version 1
 pre-shared-key cipher %^%#K{JG:rWVHPMnf;5\|,GW(Luq'qi8BT4nOj%5W5=)%^%#
 ike-proposal 5
#
ipsec policy-template use1 10
 ike-peer rut1
 proposal tran1
#
ipsec policy policy1 10 isakmp template use1
#
interface GigabitEthernet0/0/1
 ip address 60.1.2.1 255.255.255.0
 ipsec policy policy1
#
interface GigabitEthernet0/0/2
 ip address 192.168.1.3 255.255.255.0
#
ip route-static 70.1.1.0 255.255.255.0 60.1.2.2
ip route-static 192.168.3.0 255.255.255.0 60.1.2.2
#
return
Configuration file of RouterC
#
 sysname RouterC
#
acl number 3002
 rule 5 permit ip source 192.168.3.0 0.0.0.255 destination 192.168.1.0 0.0.0.255
#
ipsec proposal tran1
 esp authentication-algorithm sha2-256   
 esp encryption-algorithm aes-128
#
ike proposal 5
 encryption-algorithm aes-128
 dh group14
 authentication-algorithm sha2-256
 authentication-method pre-share
 integrity-algorithm hmac-sha2-256
 prf hmac-sha2-256
#
ike peer rut1
 version 1
 pre-shared-key cipher %^%#IRFGEiFPJ1$&a'Qy,L*XQL_+*Grq-=yMb}ULZdS6%^%#
 ike-proposal 5
 remote-address 60.1.1.1
 remote-address 60.1.2.1
#
ipsec policy policy1 10 isakmp
 security acl 3002
 ike-peer rut1
 proposal tran1
#
interface GigabitEthernet0/0/1
 ip address 70.1.1.1 255.255.255.0
 ipsec policy policy1
#
interface GigabitEthernet0/0/2
 ip address 192.168.3.2 255.255.255.0
#
ip route-static 0.0.0.0 0.0.0.0 70.1.1.2
#
return