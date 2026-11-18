Enterprise
In Figure 3-103, the enterprise requires that the administrator use AAA local authentication to log in to the device through STelnet. The specific requirements are as follows:
<HUAWEI> system-view 
[HUAWEI] sysname DeviceA 
[DeviceA] vlan batch 10
[DeviceA] interface 10ge 1/0/1
[DeviceA-10GE1/0/1] portswitch
[DeviceA-10GE1/0/1] port link-type trunk
[DeviceA-10GE1/0/1] port trunk allow-pass vlan 10
[DeviceA-10GE1/0/1] quit
[DeviceA] interface vlanif 10
[DeviceA-Vlanif10] ip address 192.168.10.1 24
[DeviceA-Vlanif10] quit
# Configure DeviceA to generate a local key pair.
[DeviceA] rsa local-key-pair create
The key name will be:Host
The range of public key size is (2048, 4096).
NOTE: Key pair generation will take a short while.
Please input the modulus [default = 3072]:3072 
# Set the authentication mode and protocol for accessing VTY user interfaces 0 to 4 to AAA and SSH, respectively.
[DeviceA] user-interface vty 0 4 
[DeviceA-ui-vty0-4] authentication-mode aaa 
[DeviceA-ui-vty0-4] protocol inbound ssh
[DeviceA-ui-vty0-4] quit
# Enable the SSH server function on DeviceA.
[DeviceA] stelnet server enable 
[DeviceA] ssh server-source -i vlanif 10
# Set the authentication mode of all SSH users to password authentication.
[DeviceA] ssh authentication-type default password
If only the authentication mode and service type of a few SSH users need to be set to password authentication and STelnet, you can specify the SSH user name to set the authentication mode and service type of a single SSH user. For example, set the authentication mode and service type of an SSH user named admin to password authentication and STelnet, respectively.
[DeviceA] ssh user admin authentication-type password
[DeviceA] ssh user admin service-type stelnet
[DeviceA] aaa
[DeviceA-aaa] local-user user1-huawei password irreversible-cipher YsHsjx_202206
[DeviceA-aaa] local-user user1-huawei service-type ssh
Warning: If the service type of local users is configured multiple times, the new configuration overwrites the previous one. The new configuration may cause local user authentication to fail. Continue? [Y/N]:y 
[DeviceA-aaa] local-user user1-huawei privilege level 3
Warning: This operation may affect online users and will change the user privilege level, Continue? [Y/N]:y
[DeviceA-aaa] quit
The administrator can log in to DeviceA through the STelnet client after entering the correct user name and password.
#
sysname DeviceA
#
aaa
 local-user user1-huawei password irreversible-cipher $1d$OwseVRh@LH}ZeTBm$1nH4$ab>d(N{-%0!ab48y=Ic*xEUR4pVhR2"9-~,$
 local-user user1-huawei privilege level 3
 local-user user1-huawei service-type ssh   
# 
vlan batch 10
#
interface Vlanif10
 ip address 192.168.10.1 255.255.255.0
#
interface 10GE1/0/1
 port link-type trunk
 port trunk allow-pass vlan 10
#
stelnet server enable 
ssh server-source -i Vlanif 10
#
user-interface vty 0 4           
 authentication-mode aaa           
 protocol inbound ssh
# 
return 
In Figure 3-104, a RADIUS server is deployed on an enterprise network. The enterprise requires that the administrator use RADIUS authentication to log in to DeviceA through STelnet. The specific requirements are as follows:
Ensure that the shared key in the RADIUS server template is the same as that configured on the RADIUS server.
If the RADIUS server does not support the user name containing a domain name, run the undo radius-server user-name domain-included command in the RADIUS server template view to configure the device to send packets that do not contain a domain name to the RADIUS server.
<HUAWEI> system-view
[HUAWEI] sysname DeviceA
[DeviceA] vlan batch 10 20
[DeviceA] interface 10ge 1/0/1 
[DeviceA-10GE1/0/1] portswitch
[DeviceA-10GE1/0/1] port link-type trunk
[DeviceA-10GE1/0/1] port trunk allow-pass vlan 10
[DeviceA-10GE1/0/1] quit
[DeviceA] interface 10ge 1/0/2 
[DeviceA-10GE1/0/2] portswitch
[DeviceA-10GE1/0/2] port link-type trunk
[DeviceA-10GE1/0/2] port trunk allow-pass vlan 20
[DeviceA-10GE1/0/2] quit
[DeviceA] interface vlanif 10
[DeviceA-Vlanif10] ip address 10.1.1.2 255.255.255.0
[DeviceA-Vlanif10] quit
[DeviceA] interface vlanif 20
[DeviceA-Vlanif20] ip address 10.1.6.2 255.255.255.0
[DeviceA-Vlanif20] quit
# Configure a RADIUS server template for communication between DeviceA and RADIUS server.
[DeviceA] radius-server template 1
[DeviceA-radius-1] radius-server authentication 10.1.6.6 1812
[DeviceA-radius-1] radius-server accounting 10.1.6.6 1813
[DeviceA-radius-1] radius-server shared-key cipher YsHsjx_202206139
[DeviceA-radius-1] quit
# Configure an AAA authentication scheme and set the authentication mode to RADIUS and local authentication.
[DeviceA] aaa
[DeviceA-aaa] authentication-scheme auth1
[DeviceA-aaa-authen-auth1] authentication-mode radius local
[DeviceA-aaa-authen-auth1] quit
# Configure an AAA accounting scheme named acc1 and set the accounting mode to RADIUS accounting.
[DeviceA-aaa] accounting-scheme acc1 
[DeviceA-aaa-accounting-acc1] accounting-mode radius 
[DeviceA-aaa-accounting-acc1] quit
# Apply the AAA schemes and RADIUS server template to a domain.
[DeviceA-aaa] domain huawei.com
[DeviceA-aaa-domain-huawei.com] authentication-scheme auth1
[DeviceA-aaa-domain-huawei.com] accounting-scheme acc1
[DeviceA-aaa-domain-huawei.com] radius-server 1
[DeviceA-aaa-domain-huawei.com] quit
[DeviceA-aaa] quit
[DeviceA] domain huawei.com admin
The configuration includes adding a device, adding a user, and setting the user privilege level to 3.
In the command output, the values of User access type, User Privilege, User authentication type, Current authentication method, Current authorization method, and Current accounting method indicate that the user login mode is SSH, the privilege level is 3, the authentication type is administrator authentication, and the authentication, authorization, as well as accounting modes are RADIUS.
<DeviceA> display access-user username user1-huawei detail
 ------------------------------------------------------------------------------
Basic:
  User ID                         : 16414
  User name                       : user1-huawei
  Domain-name                     : huawei.com
  User MAC                        : -
  User IP address                 : 10.1.1.10
  User IPv6 address               : -
  User access time                : 2024/07/01 10:49:22
  User accounting session ID      : example010000000000006d****010001e
  User access type                : SSH
  User Privilege                  : 3
  User Group                      : -
AAA:
  User authentication type        : Administrator authentication
  Current authentication method   : RADIUS
  Current authorization method    : RADIUS
  Current accounting method       : RADIUS
In the command output, the values of User access type, User Privilege, User authentication type, Current authentication method, Current authorization method, and Current accounting method indicate that the user login mode is SSH, the privilege level is 3, the authentication type is administrator authentication, the authentication mode is local, and the accounting mode is RADIUS.
<DeviceA> display access-user username user1-huawei detail
 ------------------------------------------------------------------------------
Basic:
  User ID                         : 16414
  User name                       : user1-huawei
  Domain-name                     : huawei.com
  User MAC                        : -
  User IP address                 : 10.1.1.10
  User IPv6 address               : -
  User access time                : 2024/07/01 10:49:22
  User accounting session ID      : example010000000000006d****010001e
  User access type                : SSH
  User Privilege                  : 3
  User Group                      : -
AAA:
  User authentication type        : Administrator authentication
  Current authentication method   : Local
  Current authorization method    : -
  Current accounting method       : RADIUS
DeviceA
#
sysname DeviceA
#
radius-server template 1                                                        
 radius-server shared-key cipher %+%##!!!!!!!!!"!!!!"!!!!*!!!!SKvr${[Fs.3t@/5k|BENhEu>W(3\~XG!!D;!!!!!2jp5!!!!!!A!!!!3"pK8qv!}9M#(4$jGWvQF/R[CNe/+:W^jk8HUe&W%+%#
 radius-server authentication 10.1.6.6 1812 weight 80                           
 radius-server accounting 10.1.6.6 1813 weight 80
#
aaa
 authentication-scheme auth1    
  authentication-mode radius local 
 accounting-scheme acc1    
  accounting-mode radius  
 domain huawei.com            
  authentication-scheme auth1     
  accounting-scheme acc1
  radius-server 1      
 local-user user1-huawei password irreversible-cipher $1d$OwseVRh@LH}ZeTBm$1nH4$ab>d(N{-%0!ab48y=Ic*xEUR4pVhR2"9-~,$
 local-user user1-huawei privilege level 3
 local-user user1-huawei service-type ssh   
#  
domain huawei.com admin 
#
vlan batch 10 20
#
interface Vlanif10
 ip address 10.1.1.2 255.255.255.0
#
interface Vlanif20
 ip address 10.1.6.2 255.255.255.0
#
interface 10GE1/0/1
 port link-type trunk
 port trunk allow-pass vlan 10
#
interface 10GE1/0/2
 port link-type trunk
 port trunk allow-pass vlan 20
#
stelnet server enable 
ssh server-source -i Vlanif10
#
user-interface vty 0 4           
 authentication-mode aaa           
 protocol inbound ssh
# 
return 
# Create an HWTACACS server template named template1 for communication between DeviceA and the HWTACACS server.
[DeviceA] hwtacacs-server template template1 
[DeviceA-hwtacacs-template1] hwtacacs-server authentication 10.1.6.6 49 
[DeviceA-hwtacacs-template1] hwtacacs-server authorization 10.1.6.6 49 
[DeviceA-hwtacacs-template1] hwtacacs-server accounting 10.1.6.6 49 
[DeviceA-hwtacacs-template1] hwtacacs-server shared-key cipher YsHsjx_202206139 
[DeviceA-hwtacacs-template1] quit
# Create an authentication scheme named sch1 and set the authentication mode to HWTACACS and local authentication.
[DeviceA] aaa 
[DeviceA-aaa] authentication-scheme sch1
[DeviceA-aaa-authen-sch1] authentication-mode hwtacacs local
[DeviceA-aaa-authen-sch1] quit
# Configure an authorization scheme named sch2 and set the authorization mode to HWTACACS and local authorization.
[DeviceA-aaa] authorization-scheme sch2 
[DeviceA-aaa-author-sch2] authorization-mode hwtacacs local
[DeviceA-aaa-author-sch2] quit
# Create an accounting scheme named sch3 and set the accounting mode to HWTACACS accounting.
[DeviceA-aaa] accounting-scheme sch3 
[DeviceA-aaa-accounting-sch3] accounting-mode hwtacacs 
[DeviceA-aaa-accounting-sch3] quit
# Apply the HWTACACS server template and AAA schemes to the domain huawei.com.
[DeviceA-aaa] domain huawei.com 
[DeviceA-aaa-domain-huawei.com] hwtacacs-server template1 
[DeviceA-aaa-domain-huawei.com] authentication-scheme sch1 
[DeviceA-aaa-domain-huawei.com] authorization-scheme sch2 
[DeviceA-aaa-domain-huawei.com] accounting-scheme sch3 
[DeviceA-aaa-domain-huawei.com] quit 
[DeviceA-aaa] quit
# Set the domain huawei.com as the global default administrative domain.
In the command output, the values of User access type, User Privilege, User authentication type, Current authentication method, Current authorization method, and Current accounting method indicate that the user login mode is SSH, the privilege level is 3, the authentication type is administrator authentication, and the authentication, authorization, as well as accounting modes are HWTACACS.
<DeviceA> display access-user username user1-huawei detail
 ------------------------------------------------------------------------------
Basic:
  User ID                         : 16414
  User name                       : user1-huawei
  Domain-name                     : huawei.com
  User MAC                        : -
  User IP address                 : 10.1.1.10
  User IPv6 address               : -
  User access time                : 2024/07/01 10:49:22
  User accounting session ID      : example010000000000006d****010001e
  User access type                : SSH
  User Privilege                  : 3
  User Group                      : -
AAA:
  User authentication type        : Administrator authentication
  Current authentication method   : HWTACACS
  Current authorization method    : HWTACACS
  Current accounting method       : HWTACACS
In the command output, the values of User access type, User Privilege, User authentication type, Current authentication method, Current authorization method, and Current accounting method indicate that the login mode is SSH, the privilege level is 3, the authentication type is administrator authentication, the authentication and authorization modes are local, and the accounting mode is HWTACACS.
<DeviceA> display access-user username user1-huawei detail
 ------------------------------------------------------------------------------
Basic:
  User ID                         : 16414
  User name                       : user1-huawei
  Domain-name                     : huawei.com
  User MAC                        : -
  User IP address                 : 10.1.1.10
  User IPv6 address               : -
  User access time                : 2024/07/01 10:49:22
  User accounting session ID      : example010000000000006d****010001e
  User access type                : SSH
  User Privilege                  : 3
  User Group                      : -
AAA:
  User authentication type        : Administrator authentication
  Current authentication method   : Local
  Current authorization method    : Local
  Current accounting method       : HWTACACS
# 
sysname DeviceA 
# 
hwtacacs-server template template1                                           
 hwtacacs-server authentication 10.1.6.6                                    
 hwtacacs-server authorization 10.1.6.6                                   
 hwtacacs-server accounting 10.1.6.6  
 hwtacacs-server shared-key cipher %+%##!!!!!!!!!"!!!!"!!!!*!!!!SKvr${[Fs."u,S-6a-X1'[X=L"cpF!5Oz`1!!!!!2jp5!!!!!!A!!!!Ix>cM8i{y6!);(8Dr9:dK`&BHfE(H2=.:SH{@pT%+%#  
# 
aaa 
 authentication-scheme sch1     
  authentication-mode hwtacacs local
 authorization-scheme sch2                                                       
  authorization-mode hwtacacs local
 accounting-scheme sch3                                                          
  accounting-mode hwtacacs  
 domain huawei.com                                                               
  authentication-scheme sch1                                                     
  accounting-scheme sch3                                                         
  authorization-scheme sch2                                                      
  hwtacacs-server template1  
 local-user user1-huawei password irreversible-cipher $1d$OwseVRh@LH}ZeTBm$1nH4$ab>d(N{-%0!ab48y=Ic*xEUR4pVhR2"9-~,$
 local-user user1-huawei privilege level 3
 local-user user1-huawei service-type ssh   
#  
domain huawei.com admin 
#
vlan batch 10 20
#
interface Vlanif10
 ip address 10.1.1.2 255.255.255.0
#
interface Vlanif20
 ip address 10.1.6.2 255.255.255.0
#
interface 10GE1/0/1
 port link-type trunk
 port trunk allow-pass vlan 10
#
interface 10GE1/0/2
 port link-type trunk
 port trunk allow-pass vlan 20
#
stelnet server enable 
ssh server-source -i Vlanif10
# 
user-interface vty 0 4           
 authentication-mode aaa           
 protocol inbound ssh
# 
return
# Configure an authentication scheme named sch1 and set the authentication mode to HWTACACS and local authentication.
# Create an authorization scheme named sch2, set the authorization mode to HWTACACS authorization, and enable command authorization for the administrator with the privilege level 3.
[DeviceA-aaa] authorization-scheme sch2 
[DeviceA-aaa-author-sch2] authorization-mode hwtacacs local
[DeviceA-aaa-author-sch2] authorization-cmd 3 hwtacacs local
[DeviceA-aaa-author-sch2] quit
# Create a recording scheme named sch0 to record the commands that the administrator has executed.
[DeviceA-aaa] recording-scheme sch0 
[DeviceA-aaa-recording-sch0] recording-mode hwtacacs template1 
[DeviceA-aaa-recording-sch0] quit 
[DeviceA-aaa] cmd recording-scheme sch0
The configuration includes the following: add a device, add a user, set the user privilege level to 3, and configure command authorization. Note that the reset hwtacacs-server statistics all command cannot be configured.
You can check logs recording command execution successes and failures of all users including non-HWTACACS authentication users under Reports and Activity > TACACS+ Administration.
[DeviceA] quit
<DeviceA> reset hwtacacs-server statistics all
 Error: Failed to pass the authorization.
# 
sysname DeviceA 
# 
hwtacacs-server template template1                                           
 hwtacacs-server authentication 10.1.6.6                                    
 hwtacacs-server authorization 10.1.6.6                                   
 hwtacacs-server accounting 10.1.6.6  
 hwtacacs-server shared-key cipher %+%##!!!!!!!!!"!!!!"!!!!*!!!!SKvr${[Fs."u,S-6a-X1'[X=L"cpF!5Oz`1!!!!!2jp5!!!!!!A!!!!Ix>cM8i{y6!);(8Dr9:dK`&BHfE(H2=.:SH{@pT%+%#  
# 
aaa 
 authentication-scheme sch1     
  authentication-mode hwtacacs local
 authorization-scheme sch2                                                       
  authorization-mode hwtacacs local
  authorization-cmd 3 hwtacacs local
 accounting-scheme sch3                                                          
  accounting-mode hwtacacs  
 recording-scheme sch0                                                           
  recording-mode hwtacacs template1                                                    
 cmd recording-scheme sch0 
 domain huawei.com                                                               
  authentication-scheme sch1                                                     
  accounting-scheme sch3                                                         
  authorization-scheme sch2                                                      
  hwtacacs-server template1  
 local-user user1-huawei password irreversible-cipher $1d$OwseVRh@LH}ZeTBm$1nH4$ab>d(N{-%0!ab48y=Ic*xEUR4pVhR2"9-~,$
 local-user user1-huawei privilege level 3
 local-user user1-huawei service-type ssh   
#  
domain huawei.com admin 
#
vlan batch 10 20
#
interface Vlanif10
 ip address 10.1.1.2 255.255.255.0
#
interface Vlanif20
 ip address 10.1.6.2 255.255.255.0
#
interface 10GE1/0/1
 port link-type trunk
 port trunk allow-pass vlan 10
#
interface 10GE1/0/2
 port link-type trunk
 port trunk allow-pass vlan 20
#
stelnet server enable 
ssh server-source -i Vlanif10
# 
user-interface vty 0 4           
 authentication-mode aaa           
 protocol inbound ssh
# 
return
In Figure 3-107, an LDAP server is deployed on the network, and user information as well as group information are stored on the LDAP server. The enterprise requires that the administrator can log in to the device through STelnet only after passing LDAP authentication.
# Configure an LDAP server template, and configure the IP address, port number, Base DN, administrator DN, and administrator password in the template.
[DeviceA] ldap-server template template1
[DeviceA-ldap-template1] ldap-server authentication 10.1.6.6 389 no-ssl
[DeviceA-ldap-template1] ldap-server authentication base-dn dc=esaptest,dc=com
[DeviceA-ldap-template1] ldap-server authentication manager cn=Administrator,cn=users Admin@123
[DeviceA-ldap-template1] quit
# Configure an authentication scheme named auth1 and set the authentication mode to LDAP authentication.
[DeviceA] aaa
[DeviceA-aaa] authentication-scheme auth1
[DeviceA-aaa-authen-auth1] authentication-mode ldap
[DeviceA-aaa-authen-auth1] quit
# Configure an authorization scheme named acc1 and set the authorization mode to LDAP authorization.
[DeviceA-aaa] authorization-scheme acc1
[DeviceA-aaa-author-acc1] authorization-mode ldap
[DeviceA-aaa-author-acc1] quit
# Configure the service scheme s1 and set the administrator privilege level to 3 in the service scheme.
[DeviceA-aaa] service-scheme s1
[DeviceA-aaa-service-s1] admin-user privilege level 3
[DeviceA-aaa-service-s1] quit
# Apply the created AAA schemes and LDAP server template to a specific domain.
[DeviceA-aaa] domain huawei.com
[DeviceA-aaa-domain-huawei.com] authentication-scheme auth1
[DeviceA-aaa-domain-huawei.com] authorization-scheme acc1
[DeviceA-aaa-domain-huawei.com] ldap-server template1
[DeviceA-aaa-domain-huawei.com] service-scheme s1
[DeviceA-aaa-domain-huawei.com] quit
[DeviceA-aaa] quit
#
sysname DeviceA
#
ldap-server template template1
 ldap-server authentication 10.1.6.6 389 no-ssl
 ldap-server authentication manager cn=Administrator,cn=users %+%##!!!!!!!!!"!!!!"!!!!*!!!!SKvr${[Fs.nt[%K)u50(_zvw*/=MzISB3'K!!!!!2jp5!!!!!!:!!!!h$%%+PY@9GDRjBP8bVj/vDf{Cr}&[Sc`Kf)!!!!!%+%#
 ldap-server authentication base-dn dc=esaptest,dc=com
#
aaa
 authentication-scheme auth1    
  authentication-mode ldap  
 authorization-scheme acc1    
  authorization-mode ldap  
 service-scheme s1
  admin-user privilege level 3 
 domain huawei.com            
  authentication-scheme auth1     
  authorization-scheme acc1
  service-scheme s1 
  ldap-server template1 
#  
domain huawei.com admin 
#
vlan batch 10 20
#
interface Vlanif10
 ip address 10.1.1.2 255.255.255.0
#
interface Vlanif20
 ip address 10.1.6.2 255.255.255.0
#
interface 10GE1/0/1
 port link-type trunk
 port trunk allow-pass vlan 10
#
interface 10GE1/0/2
 port link-type trunk
 port trunk allow-pass vlan 20
#
stelnet server enable 
ssh server-source -i Vlanif10
#
user-interface vty 0 4           
 authentication-mode aaa           
 protocol inbound ssh
# 
return 
In Figure 3-107, an AD server is deployed on the network, and user information as well as group information are stored on the AD server. The enterprise requires that the administrator can log in to the device through STelnet only after passing AD authentication.
In this example, interface 1 and interface 2 represent 10GE 1/0/1 and 10GE 1/0/2, respectively.
# Configure an AD server template, and configure the IP address, port number, Base DN, administrator DN, and administrator password in the template.
[DeviceA] ad-server template template1
[DeviceA-ad-template1] ad-server authentication 10.1.6.6 88
[DeviceA-ad-template1] ad-server authentication base-dn dc=test1,dc=com
[DeviceA-ad-template1] ad-server authentication manager cn=Administrator,cn=users Admin@123
[DeviceA-ad-template1] ad-server authentication host-name win.aa
[DeviceA-ad-template1] quit
# Configure an authentication scheme named auth1 and set the authentication mode to AD authentication.
[DeviceA] aaa
[DeviceA-aaa] authentication-scheme auth1
[DeviceA-aaa-authen-auth1] authentication-mode ad
[DeviceA-aaa-authen-auth1] quit
# Configure an authorization scheme named acc1 and set the authorization mode to local authorization.
[DeviceA-aaa] authorization-scheme acc1
[DeviceA-aaa-author-acc1] authorization-mode local
[DeviceA-aaa-author-acc1] quit
# Configure a local user and access type.
# Configure local authorization. Select one or more configuration methods as required.
[DeviceA-aaa] domain huawei.com
[DeviceA-aaa-domain-huawei.com] authentication-scheme auth1
[DeviceA-aaa-domain-huawei.com] authorization-scheme acc1
[DeviceA-aaa-domain-huawei.com] ad-server template1
[DeviceA-aaa-domain-huawei.com] service-scheme s1
[DeviceA-aaa-domain-huawei.com] quit
[DeviceA-aaa] quit
#
sysname DeviceA
#
ad-server template template1
 ad-server authentication 10.1.6.6 88 ldap-over-ssl
 ad-server authentication base-dn dc=test1,dc=com
 ad-server authentication manager cn=Administrator,cn=users %+%##!!!!!!!!!"!!!!"!!!!*!!!!SKvr${[Fs.|#*S#kB\=3@-+#@CYd5E^@|E*!!!!!2jp5!!!!!!:!!!!F4_]L0O\."9h_o8tIO/(`'>p";xS#/Fns\*!!!!!%+%#
 ad-server authentication host-name win.aa
#
aaa
 authentication-scheme auth1    
  authentication-mode ad  
 authorization-scheme acc1    
  authorization-mode local  
 service-scheme s1
  admin-user privilege level 3 
 domain huawei.com            
  authentication-scheme auth1     
  authorization-scheme acc1
  service-scheme s1
  ad-server template1 
 local-user user1-huawei privilege level 3
 local-user user1-huawei service-type ssh  
#  
domain huawei.com admin 
#
vlan batch 10 20
#
interface Vlanif10
 ip address 10.1.1.2 255.255.255.0
#
interface Vlanif20
 ip address 10.1.6.2 255.255.255.0
#
interface 10GE1/0/1
 port link-type trunk
 port trunk allow-pass vlan 10
#
interface 10GE1/0/2
 port link-type trunk
 port trunk allow-pass vlan 20
#
stelnet server enable 
ssh server-source -i Vlanif10
#
user-interface vty 0 4           
 authentication-mode aaa           
 protocol inbound ssh
# 
return 
In Figure 3-109, two RADIUS servers are deployed. The enterprise requires that the two RADIUS servers back up each other to ensure user authentication if one server is faulty.
The configuration roadmap is as follows:
<HUAWEI> system-view 
[HUAWEI] sysname DeviceA 
[DeviceA] vlan batch 10 20 30
[DeviceA] interface 10ge 1/0/1
[DeviceA-10GE1/0/1] portswitch
[DeviceA-10GE1/0/1] port link-type trunk
[DeviceA-10GE1/0/1] port trunk allow-pass vlan 10
[DeviceA-10GE1/0/1] quit
[DeviceA] interface 10ge 1/0/2
[DeviceA-10GE1/0/2] portswitch
[DeviceA-10GE1/0/2] port link-type trunk
[DeviceA-10GE1/0/2] port trunk allow-pass vlan 20
[DeviceA-10GE1/0/2] quit
[DeviceA] interface 10ge 1/0/3
[DeviceA-10GE1/0/3] portswitch
[DeviceA-10GE1/0/3] port link-type trunk
[DeviceA-10GE1/0/3] port trunk allow-pass vlan 30
[DeviceA-10GE1/0/3] quit
[DeviceA] interface vlanif 10
[DeviceA-Vlanif10] ip address 10.1.1.2 255.255.255.0
[DeviceA-Vlanif10] quit
[DeviceA] interface vlanif 20
[DeviceA-Vlanif20] ip address 10.1.6.2 255.255.255.0
[DeviceA-Vlanif20] quit
[DeviceA] interface vlanif 30
[DeviceA-Vlanif30] ip address 10.1.5.2 255.255.255.0
[DeviceA-Vlanif30] quit
# Configure a RADIUS template named shiva.
[DeviceA] radius-server template shiva
# Configure IP addresses and port numbers for the primary RADIUS authentication and accounting server.
[DeviceA-radius-shiva] radius-server authentication 10.7.66.66 1812 weight 80
[DeviceA-radius-shiva] radius-server accounting 10.7.66.66 1813 weight 80
# Configure IP addresses and port numbers for the secondary RADIUS authentication and accounting server.
[DeviceA-radius-shiva] radius-server authentication 10.7.66.67 1812 weight 40
[DeviceA-radius-shiva] radius-server accounting 10.7.66.67 1813 weight 40
# Set the shared key and maximum number of retransmissions for the RADIUS server, and configure the device not to encapsulate the domain name in the user name when sending RADIUS packets to the RADIUS server.
[DeviceA-radius-shiva] radius-server shared-key cipher YsHsjx_202206139
[DeviceA-radius-shiva] radius-server retransmit 2
[DeviceA-radius-shiva] undo radius-server user-name domain-included
[DeviceA-radius-shiva] quit
# Create an authentication scheme named auth and set the authentication mode to RADIUS and local authentication.
[DeviceA] aaa
[DeviceA-aaa] authentication-scheme auth
[DeviceA-aaa-authen-auth] authentication-mode radius local
[DeviceA-aaa-authen-auth] quit
# Create an accounting scheme named abc, and set the accounting mode to RADIUS accounting. Configure a policy for the device to keep users online upon accounting-start failures.
[DeviceA-aaa] accounting-scheme abc
[DeviceA-aaa-accounting-abc] accounting-mode radius
[DeviceA-aaa-accounting-abc] accounting start-fail online 
[DeviceA-aaa-accounting-abc] quit
[DeviceA-aaa] domain huawei
[DeviceA-aaa-domain-huawei] authentication-scheme auth
[DeviceA-aaa-domain-huawei] accounting-scheme abc
[DeviceA-aaa-domain-huawei] radius-server shiva
[DeviceA-aaa-domain-huawei] quit
[DeviceA-aaa] quit
[DeviceA] domain huawei admin
Run the display radius-server configuration template template-name command on DeviceA to check the RADIUS server template configuration.
[DeviceA] display radius-server configuration template shiva
  ------------------------------------------------------------------------------
  Server-template-name          :  shiva
  Server-template-index         :  14                                      
  Protocol-version              :  standard
  Traffic-unit                  :  B
  Shared-secret-key             :  ****************
  Group-filter                  :  class  
  Timeout-interval(in second)   :  5
  Retransmission                :  2
  EndPacketSendTime             :  0
  Dead time(in minute)          :  5
  Domain-included               :  NO
  NAS-IP-Address                :  0.0.0.0
  Calling-station-id MAC-format :  xxxx-xxxx-xxxx
  Called-station-id MAC-format  :  XX-XX-XX-XX-XX-XX
  NAS-Port-ID format            :  New 
  Service-type                  :  - 
  NAS-IPv6-Address              :  ::
  Server algorithm              :  master-backup           
  Detect-interval(in second)    :  60     
  Detect up-server(in second)   :  0              
  Detect timeout(in second)     :  3                               
  Chargeable-user-identity      :  Not Support             
  CUI Not reject                :  No               
  Force framed-ip-addr Attr     :  No 
  Detect-interval(in second)    :  60 
  Authentication Server 1       :  10.7.66.66     Port:1812  Weight:80  [up]
                                   Vrf:- 
                                   Source Interface:NULL                        
                                   Source IP: ::
                                   Shared-key: -
                                   Down time left: -
  Authentication Server 2       :  10.7.66.67     Port:1812  Weight:40  [up]
                                   Vrf:- 
                                   Source Interface:NULL                        
                                   Source IP: ::
                                   Shared-key: -
                                   Down time left: -
  Accounting Server     1       :  10.7.66.66     Port:1813  Weight:80  [up]
                                   Vrf:- 
                                   Source Interface:NULL                        
                                   Source IP: ::
                                   Shared-key: -
                                   Down time left: -
  Accounting Server     2       :  10.7.66.67     Port:1813  Weight:40  [up]
                                   Vrf:- 
                                   Source Interface:NULL                        
                                   Source IP: ::
                                   Shared-key: -
                                   Down time left: -
  ------------------------------------------------------------------------------ 
#
sysname DeviceA
#
radius-server template shiva
 radius-server shared-key cipher %+%##!!!!!!!!!"!!!!"!!!!*!!!!SKvr${[Fs.3t@/5k|BENhEu>W(3\~XG!!D;!!!!!2jp5!!!!!!A!!!!3"pK8qv!}9M#(4$jGWvQF/R[CNe/+:W^jk8HUe&W%+%#
 radius-server authentication 10.7.66.66 1812 weight 80
 radius-server authentication 10.7.66.67 1812 weight 40
 radius-server accounting 10.7.66.66 1813 weight 80
 radius-server accounting 10.7.66.67 1813 weight 40
 radius-server retransmit 2
 undo radius-server user-name domain-included 
#
aaa
 authentication-scheme auth
  authentication-mode radius local
 accounting-scheme abc
  accounting-mode radius
 domain huawei
  authentication-scheme auth
  accounting-scheme abc
  radius-server shiva
 local-user user1-huawei password irreversible-cipher $1d$OwseVRh@LH}ZeTBm$1nH4$ab>d(N{-%0!ab48y=Ic*xEUR4pVhR2"9-~,$
 local-user user1-huawei privilege level 3
 local-user user1-huawei service-type ssh   
#
domain huawei admin
# 
vlan batch 10 20 30
#
interface Vlanif10
 ip address 10.1.1.2 255.255.255.0
#
interface Vlanif20
 ip address 10.1.6.2 255.255.255.0
#
interface Vlanif30
 ip address 10.1.5.2 255.255.255.0
#
interface 10GE1/0/1 
 port link-type trunk
 port trunk allow-pass vlan 10
#
interface 10GE1/0/2 
 port link-type trunk
 port trunk allow-pass vlan 20
#
interface 10GE1/0/3 
 port link-type trunk
 port trunk allow-pass vlan 30
#
return
In Figure 3-110, two HWTACACS servers are deployed. The enterprise requires that the two HWTACACS servers back up each other to ensure user authentication if one server is faulty.
Ensure that the shared key in the HWTACACS server template is the same as that configured on the HWTACACS server.
If the HWTACACS server does not support the user name containing the domain name, run the undo hwtacacs-server user-name domain-included command in the HWTACACS server template view to configure the device to send packets that do not contain the domain name to the HWTACACS server.
# Create an HWTACACS server template named ht.
[DeviceA] hwtacacs-server template ht
# Configure IP addresses and port numbers for the primary HWTACACS authentication, authorization, and accounting servers.
[DeviceA-hwtacacs-ht] hwtacacs-server authentication 10.7.66.66 49
[DeviceA-hwtacacs-ht] hwtacacs-server authorization 10.7.66.66 49
[DeviceA-hwtacacs-ht] hwtacacs-server accounting 10.7.66.66 49
# Configure IP addresses and port numbers for the secondary HWTACACS authentication, authorization, and accounting servers.
[DeviceA-hwtacacs-ht] hwtacacs-server authentication 10.7.66.67 49 secondary
[DeviceA-hwtacacs-ht] hwtacacs-server authorization 10.7.66.67 49 secondary
[DeviceA-hwtacacs-ht] hwtacacs-server accounting 10.7.66.67 49 secondary
# Configure a shared key for the HWTACACS server.
[DeviceA-hwtacacs-ht] hwtacacs-server shared-key cipher YsHsjx_202206139
[DeviceA-hwtacacs-ht] quit
# Create an authentication scheme named l-h, and set the authentication mode to HWTACACS and local authentication.
[DeviceA] aaa
[DeviceA-aaa] authentication-scheme l-h
[DeviceA-aaa-authen-l-h] authentication-mode hwtacacs local
[DeviceA-aaa-authen-l-h] quit
# Create an authorization scheme named hwtacacs, and set the authorization mode to HWTACACS and local authorization.
[DeviceA-aaa] authorization-scheme hwtacacs
[DeviceA-aaa-author-hwtacacs] authorization-mode hwtacacs local
[DeviceA-aaa-author-hwtacacs] quit
# Create an accounting scheme named hwtacacs, and set the accounting mode to HWTACACS accounting. Configure a policy for the device to keep users online upon accounting-start failures.
[DeviceA-aaa] accounting-scheme hwtacacs
[DeviceA-aaa-accounting-hwtacacs] accounting-mode hwtacacs 
[DeviceA-aaa-accounting-hwtacacs] accounting start-fail online
# Set the real-time accounting interval to 3 minutes.
[DeviceA-aaa-accounting-hwtacacs] accounting realtime 3
[DeviceA-aaa-accounting-hwtacacs] quit
[DeviceA-aaa] domain huawei
[DeviceA-aaa-domain-huawei] authentication-scheme l-h
[DeviceA-aaa-domain-huawei] authorization-scheme hwtacacs
[DeviceA-aaa-domain-huawei] accounting-scheme hwtacacs
[DeviceA-aaa-domain-huawei] hwtacacs-server ht
[DeviceA-aaa-domain-huawei] quit
[DeviceA-aaa] quit
Run the display hwtacacs-server template command on DeviceA. The command output shows that the HWTACACS server template configuration meets the requirements.
[DeviceA] display hwtacacs-server template name ht
  ---------------------------------------------------------------------------
  HWTACACS-server template name        : ht
  HWTACACS-server template index       : 1
  Primary-authentication-server        : 10.7.66.66:49 Vrf:- Shared-key:- Status:UP
  Primary-authentication-ipv6-server   : -:0 Vrf:- Shared-key:- Status:-
  Primary-authorization-server         : 10.7.66.66:49 Vrf:- Shared-key:- Status:UP
  Primary-authorization-ipv6-server    : -:0 Vrf:- Shared-key:- Status:-
  Primary-accounting-server            : 10.7.66.66:49 Vrf:- Shared-key:- Status:UP
  Primary-accounting-ipv6-server       : -:0 Vrf:- Shared-key:- Status:-
  Secondary-authentication-server      : 10.7.66.66:49 Vrf:- Shared-key:- Status:-
  Secondary-authentication-ipv6-server : -:0 Vrf:- Shared-key:- Status:-
  Secondary-authorization-server       : 10.7.66.66:49 Vrf:- Shared-key:- Status:-
  Secondary-authorization-ipv6-server  : -:0 Vrf:- Shared-key:- Status:-
  Secondary-accounting-server          : 10.7.66.66:49 Vrf:- Shared-key:- Status:-
  Secondary-accounting-ipv6-server     : -:0 Vrf:- Shared-key:- Status:-
  Third-authentication-server          : -:0 Vrf:- Shared-key:- Status:-
  Third-authentication-ipv6-server     : -:0 Vrf:- Shared-key:- Status:-
  Third-authorization-server           : -:0 Vrf:- Shared-key:- Status:-
  Third-authorization-ipv6-server      : -:0 Vrf:- Shared-key:- Status:-
  Third-accounting-server              : -:0 Vrf:- Shared-key:- Status:-
  Third-accounting-ipv6-server         : -:0 Vrf:- Shared-key:- Status:-
  Fourth-authentication-server         : -:0 Vrf:- Shared-key:- Status:-
  Fourth-authentication-ipv6-server    : -:0 Vrf:- Shared-key:- Status:-
  Fourth-authorization-server          : -:0 Vrf:- Shared-key:- Status:-
  Fourth-authorization-ipv6-server     : -:0 Vrf:- Shared-key:- Status:-
  Fourth-accounting-server             : -:0 Vrf:- Shared-key:- Status:-
  Fourth-accounting-ipv6-server        : -:0 Vrf:- Shared-key:- Status:-
  Current-authentication-server        : 10.7.66.66:49 Vrf:- Shared-key:- Status:UP
  Current-authentication-ipv6-server   : -:0 Vrf:- Shared-key:- Status:-
  Current-authorization-server         : 10.7.66.66:49 Vrf:- Shared-key:- Status:UP
  Current-authorization-ipv6-server    : -:0 Vrf:- Shared-key:- Status:-
  Current-accounting-server            : 10.7.66.66:49 Vrf:- Shared-key:- Status:UP
  Current-accounting-ipv6-server       : -:0 Vrf:- Shared-key:- Status:-
  Source-IP-address                    : -
  Source-LoopBack                      : -
  Source-Vlanif                        : -
  Source-IPv6-address                  : -
  IPv6 Source-LoopBack                 : -
  IPv6 Source-Vlanif                   : -
  Shared-key                           : ****************
  Quiet-interval(min)                  : 5
  Response-timeout-Interval(sec)       : 5
  Domain-included                      : Original
  Traffic-unit                         : B
  User name in authen-start message    : No
  ---------------------------------------------------------------------------
#
sysname DeviceA
#
hwtacacs-server template ht
 hwtacacs-server authentication 10.7.66.66
 hwtacacs-server authentication 10.7.66.67 secondary
 hwtacacs-server authorization 10.7.66.66
 hwtacacs-server authorization 10.7.66.67 secondary
 hwtacacs-server accounting 10.7.66.66
 hwtacacs-server accounting 10.7.66.67 secondary
 hwtacacs-server shared-key cipher %+%##!!!!!!!!!"!!!!"!!!!*!!!!SKvr${[Fs."u,S-6a-X1'[X=L"cpF!5Oz`1!!!!!2jp5!!!!!!A!!!!Ix>cM8i{y6!);(8Dr9:dK`&BHfE(H2=.:SH{@pT%+%#
#
aaa
 authentication-scheme l-h
  authentication-mode hwtacacs local
 authorization-scheme hwtacacs
  authorization-mode hwtacacs local
 accounting-scheme hwtacacs
  accounting-mode hwtacacs
  accounting realtime 3
 domain huawei
  authentication-scheme l-h
  accounting-scheme hwtacacs
  authorization-scheme hwtacacs
  hwtacacs-server ht
 local-user user1-huawei password irreversible-cipher $1d$OwseVRh@LH}ZeTBm$1nH4$ab>d(N{-%0!ab48y=Ic*xEUR4pVhR2"9-~,$
 local-user user1-huawei privilege level 3
 local-user user1-huawei service-type ssh   
#
domain huawei admin
#
vlan batch 10 20 30
#
interface Vlanif10
 ip address 10.1.1.2 255.255.255.0
#
interface Vlanif20
 ip address 10.1.6.2 255.255.255.0
#
interface Vlanif30
 ip address 10.1.5.2 255.255.255.0
#
interface 10GE1/0/1 
 port link-type trunk
 port trunk allow-pass vlan 10
#
interface 10GE1/0/2 
 port link-type trunk
 port trunk allow-pass vlan 20
#
interface 10GE1/0/3 
 port link-type trunk
 port trunk allow-pass vlan 30
#
return 
As shown in Figure 3-111, a RADIUS server is deployed on the network. To ensure the confidentiality and integrity of data transmitted between the RADIUS server and client, the administrator configures DTLS for data transmission between the server and client.
If different users access the network after RADIUS over DTLS is configured, you are advised to adjust the CPCAR value of packets based on the following table to ensure the user access rate.
| RADIUS over DTLS Scenario | Possible Problem | Workaround | Impact | 
|---|---|---|---|
| Administrator | The access rate of administrators is low. | Do not adjust the CPCAR value. | There is no impact. | 
| MAC | Many users need to be concurrently authenticated, and RADIUS DTLS packets need to be retransmitted. | Run the access-user car-mode dtls mac-authen wired command to automatically adjust the CPCAR value of MAC protocol packets to 20% of the default CPCAR value. | The access rate of wired MAC address authentication users in the non-RADIUS over DTLS authentication scenario is affected. | 
| Portal 2.0 | Many users need to be concurrently authenticated, and RADIUS DTLS packets need to be retransmitted. | Run the access-user car-mode dtls portal wired command to automatically adjust the CPCAR value of Portal protocol packets to 20% of the default CPCAR value. | The access rate of wired or wireless Portal 2.0 authentication users in the non-RADIUS over DTLS authentication scenario is affected. | 
| HTTPS Portal | Many users need to be concurrently authenticated, and RADIUS DTLS packets need to be retransmitted. | Run the access-user car-mode dtls https-portal wired command to automatically adjust the CPCAR value of HTTPS Portal protocol packets to 20% of the default CPCAR value. | The access rate of wired or wireless HTTPS Portal authentication users in the non-RADIUS over DTLS authentication scenario is affected. | 
| 802.1X CHAP/PAP | Many users need to be concurrently authenticated, and RADIUS DTLS packets need to be retransmitted. | Run the access-user car-mode dtls dot1x wired command to automatically adjust the CPCAR value of 802.1X protocol packets to 20% of the default CPCAR value. | The access rate of wired 802.1X authentication users in the non-RADIUS over DTLS authentication scenario is affected. | 
| 802.1X EAP | Many users need to be concurrently authenticated, and RADIUS DTLS packets need to be retransmitted. |  | The access rate of wired 802.1X authentication users in the non-RADIUS over DTLS authentication scenario is affected. | 
| Wireless 802.1X | Many users need to be concurrently authenticated, and RADIUS DTLS packets need to be retransmitted. |  | The access rates of wireless open authentication, MAC address authentication, and Portal authentication users are affected. | 
| Wireless MAC | Many users need to be concurrently authenticated, and RADIUS DTLS packets need to be retransmitted. | Run the access-user car-mode dtls mac-authen wireless command to automatically adjust the CPCAR value in user association to 20% of the default CPCAR value. | The access rates of wireless open authentication, 802.1X authentication, and Portal authentication users are affected. | 
| Wireless Portal | Many users need to be concurrently authenticated, and RADIUS DTLS packets need to be retransmitted. | Run the access-user car-mode dtls portal wireless command to automatically adjust the CPCAR value in user association and that of Portal protocol packets to 20% of the default CPCAR value. | The access rates of wireless open authentication, 802.1X authentication, and MAC address authentication users are affected. | 
| Wireless MAC+802.1X | Many users need to be concurrently authenticated, and RADIUS DTLS packets need to be retransmitted. | Run the access-user car-mode dtls mac-authen dot1x wireless command to automatically adjust the CPCAR value in user association and that of wireless 802.1X packets to 20% of the default CPCAR value. | The access rates of wireless open authentication, 802.1X authentication, and Portal authentication users are affected. | 
| RADIUS over DTLS in active/standby or load balancing scenario | The CPCAR value is small in the RADIUS over DTLS scenario. As a result, packets may fail to be received in the multi-server scenario. |  | In the multi-server scenario, servers may fail to receive packets. | 
Ensure that the PKI realm realm-name bound to the DTLS policy has a certificate loaded. The certificate can be an initial device certificate or a digital certificate applied by a user. For details about how to load a certificate using PKI, see "PKI Configuration" in CLI Configuration Guide > Security Configuration.
[DeviceA] pki realm abcd 
[DeviceA-pki-abcd] quit 
[DeviceA] pki import-certificate ca realm abcd pem filename test.cer
[DeviceA] dtls policy huawei
[DeviceA-dtls-policy-huawei] pki-domain abcd
[DeviceA-dtls-policy-huawei] quit
# Configure a RADIUS server template named shiva.
# Configure an IP address and a port number for the RADIUS authentication and accounting server.
[DeviceA-radius-shiva] radius-server authentication 10.7.66.66 2083 weight 80 dtls-policy huawei
[DeviceA-radius-shiva] radius-server accounting 10.7.66.66 2083 weight 80 dtls-policy huawei
# Create an authentication scheme named auth and set the authentication mode to RADIUS authentication and local authentication.
Run the display radius-server configuration template template-name command on DeviceA to check the RADIUS server template configuration. The command output shows that the configuration meets the requirements.
[DeviceA] display radius-server configuration template shiva
  ------------------------------------------------------------------------------
  Server-template-name          :  shiva
  Server-template-index         :  14                                      
  Protocol-version              :  standard
  Traffic-unit                  :  B
  Shared-secret-key             :  ****************
  Group-filter                  :  class  
  Timeout-interval(in second)   :  5
  Retransmission                :  2
  EndPacketSendTime             :  0
  Dead time(in minute)          :  5
  Domain-included               :  NO
  NAS-IP-Address                :  0.0.0.0
  Calling-station-id MAC-format :  xxxx-xxxx-xxxx
  Called-station-id MAC-format  :  XX-XX-XX-XX-XX-XX
  NAS-Port-ID format            :  New 
  Service-type                  :  - 
  NAS-IPv6-Address              :  ::
  Server algorithm              :  master-backup           
  Detect-interval(in second)    :  60     
  Detect up-server(in second)   :  0              
  Detect timeout(in second)     :  3                               
  Chargeable-user-identity      :  Not Support             
  CUI Not reject                :  No               
  Force framed-ip-addr Attr     :  No 
  Detect-interval(in second)    :  60 
  Authentication Server 1       :  10.7.66.66     Port:2083  Weight:80  [up]
                                   Vrf:- 
                                   Source Interface:NULL                        
                                   Source IP: ::
                                   Shared-key: -
                                   Down time left: -
                                   Dtls-Policy name: huawei
  Accounting Server     1       :  10.7.66.66     Port:1813  Weight:80  [up]
                                   Vrf:- 
                                   Source Interface:NULL                        
                                   Source IP: ::
                                   Shared-key: -
                                   Down time left: -
                                   Dtls-Policy name: huawei
  ------------------------------------------------------------------------------ 
#
pki realm abcd  
#
sysname DeviceA
#
#
dtls policy huawei
 pki-domain abcd
#
radius-server template shiva
 radius-server authentication 10.7.66.66 2083 weight 80 dtls-policy huawei
 radius-server accounting 10.7.66.66 2083 weight 80 dtls-policy huawei
#
aaa
 authentication-scheme auth
  authentication-mode radius local
 accounting-scheme abc
  accounting-mode radius
 domain huawei
  authentication-scheme auth
  accounting-scheme abc
  radius-server shiva
 local-user user1-huawei password irreversible-cipher $1d$OwseVRh@LH}ZeTBm$1nH4$ab>d(N{-%0!ab48y=Ic*xEUR4pVhR2"9-~,$
 local-user user1-huawei privilege level 3
 local-user user1-huawei service-type ssh   
#
domain huawei admin
# 
vlan batch 10 20
#
interface Vlanif10
 ip address 10.1.1.2 255.255.255.0
#
interface Vlanif20
 ip address 10.1.6.2 255.255.255.0
#
interface 10GE1/0/1 
 port link-type trunk
 port trunk allow-pass vlan 10
#
interface 10GE1/0/2 
 port link-type trunk
 port trunk allow-pass vlan 20
#
return
In Figure 3-112, users connect to the network through DeviceA. The user names do not contain any domain name. It is required that common users connect to the network and obtain corresponding rights only after passing RADIUS authentication and that the administrator logs in to DeviceA only after passing local authentication.
Ensure that users have been configured on the RADIUS server. In this example, a user with the name test1 and password YsHsjx_202206 has been configured on the RADIUS server.
This example provides only the configuration of DeviceA. The RADIUS server configuration is not provided here.
# Create VLAN 11 on DeviceA.
<HUAWEI> system-view
[HUAWEI] sysname DeviceA
[DeviceA] vlan batch 11
# On DeviceA, configure 10GE 1/0/2 connecting to the RADIUS server as an access interface, and add 10GE 1/0/1 to VLAN 11.
[DeviceA] interface 10ge 1/0/2
[DeviceA-10GE1/0/2] port link-type access
[DeviceA-10GE1/0/2] port default vlan 11
[DeviceA-10GE1/0/2] quit
# Create VLANIF 11 and configure the IP address 192.168.2.29/24 for it.
[DeviceA] interface vlanif 11
[DeviceA-Vlanif11] ip address 192.168.2.29 24
[DeviceA-Vlanif11] quit
# Create and configure the RADIUS server template rd1.
[DeviceA] radius-server template rd1
[DeviceA-radius-rd1] radius-server authentication 192.168.2.30 1812
[DeviceA-radius-rd1] radius-server accounting 192.168.2.30 1813
[DeviceA-radius-rd1] radius-server shared-key cipher YsHsjx_202206139
[DeviceA-radius-rd1] radius-server retransmit 2
[DeviceA-radius-rd1] quit
# Create the AAA authentication scheme abc and accounting scheme abc, and set the authentication mode and accounting mode to RADIUS.
[DeviceA] aaa
[DeviceA-aaa] authentication-scheme abc
[DeviceA-aaa-authen-abc] authentication-mode radius
[DeviceA-aaa-authen-abc] quit
[DeviceA-aaa] accounting-scheme abc
[DeviceA-aaa-accounting-abc] accounting-mode radius
[DeviceA-aaa-accounting-abc] quit
# Test the connection between DeviceA and the RADIUS server. (The following assumes that the test user test1 and password YsHsjx_202206 have been configured on the RADIUS server.)
[DeviceA-aaa] test-aaa test1 YsHsjx_202206 radius-template rd1
# Bind the AAA authentication scheme abc, AAA accounting scheme abc, and RADIUS server template rd1 to the domain default.
[DeviceA-aaa] domain default
[DeviceA-aaa-domain-default] authentication-scheme abc
[DeviceA-aaa-domain-default] accounting-scheme abc
[DeviceA-aaa-domain-default] radius-server rd1
[DeviceA-aaa-domain-default] quit
[DeviceA-aaa] quit
# Enable 802.1X authentication on an interface.
[DeviceA] dot1x-access-profile name d1
[DeviceA-dot1x-access-profile-d1] quit
[DeviceA] authentication-profile name p1
[DeviceA-authen-profile-p1] dot1x-access-profile d1
[DeviceA-authen-profile-p1] authentication mode multi-authen max-user 100
[DeviceA-authen-profile-p1] quit
[DeviceA] vlan batch 10
[DeviceA] interface 10ge 1/0/1
[DeviceA-10GE1/0/1] port link-type access
[DeviceA-10GE1/0/1] port default vlan 10 
[DeviceA-10GE1/0/1] authentication-profile p1
[DeviceA-10GE1/0/1] quit
# Set the domain default as the global default common domain. After common users enter their user names in the format of user@default, the device performs AAA authentication for these users in the domain default. If a user name does not contain a domain name or contains a nonexistent domain name, the device authenticates the user in the default common domain.
[DeviceA] domain default
[DeviceA] rsa local-key-pair create
The key name will be:Host
The range of public key size is (2048, 4098).
NOTE: Key pair generation will take a short while.
Please input the modulus [default = 3072]:3072 
[DeviceA] stelnet server enable 
[DeviceA] ssh server-source all-interface
# Create a local user named test, set the password to YsHsjx_202206, access type to SSH, and privilege level to 3.
[DeviceA] aaa
[DeviceA-aaa] local-user user1-huawei password irreversible-cipher YsHsjx_202206
[DeviceA-aaa] local-user user1-huawei service-type ssh
[DeviceA-aaa] local-user user1-huawei privilege level 3
[DeviceA-aaa] quit
# Enable the local account locking function, and set the authentication retry interval to 5 minutes, maximum number of consecutive failed password attempts to 3, and account lockout duration to 5 minutes.
[DeviceA-aaa] local-aaa-user wrong-password retry-interval 5 retry-time 3 block-time 5
# Configure an authentication scheme named auth and set the authentication mode to local authentication.
[DeviceA-aaa] authentication-scheme auth
[DeviceA-aaa-authen-auth] authentication-mode local
[DeviceA-aaa-authen-auth] quit
# Configure an authorization scheme named autho and set the authorization mode to local authorization.
[DeviceA-aaa] authorization-scheme autho
[DeviceA-aaa-author-autho] authorization-mode local
[DeviceA-aaa-author-autho] quit
# Configure the domain default_admin and apply the authentication scheme auth and authorization scheme autho to the domain.
[DeviceA-aaa] domain default_admin
[DeviceA-aaa-domain-default_admin] authentication-scheme auth
[DeviceA-aaa-domain-default_admin] authorization-scheme autho
[DeviceA-aaa-domain-default_admin] quit
[DeviceA-aaa] quit
# Set the domain default_admin as the global default administrative domain. After the administrator enters the user name in the format of user@default_admin, the device performs AAA authentication for the administrator in the domain default_admin. If the user name does not contain a domain name or contains a nonexistent domain name, the device authenticates the administrator in the default administrative domain.
[DeviceA] domain default_admin admin
[DeviceA] quit
# Run the display dot1x interface command on DeviceA to check 802.1X authentication information.
# After a common user enters the user name test1 and password YsHsjx_202206 on the 802.1X client to log in to the device, you can run the display access-user domain and display access-user user-id commands on the device to check the domain and access type of the user.
<DeviceA> display access-user domain default
 ------------------------------------------------------------------------------
 UserID Username             IP address             MAC               Status
 ------------------------------------------------------------------------------
 16040  test1                -                      00e0-fc01-31f6    Success
 ------------------------------------------------------------------------------
 Total: 1, printed: 1
<DeviceA> display access-user user-id 16040
 Total: 1                                                               
 ------------------------------------------------------------------------------ 
Basic:
  User id                         : 16040
  User name                       : test1
  Domain-name                     : default
  User MAC                        : 00e0-4c97-31f6
  User IP address                 : -
  User IPv6 address               : -
  User access time                : 2024/02/15 19:10:52
  User accounting session ID      : example255255000000000f****2016040
  Option82 information            : -
  User access type                : 802.1x
AAA:
  User authentication type        : 802.1x authentication
  Current authentication method   : RADIUS
  Current authorization method    : -
  Current accounting method       : RADIUS
 ------------------------------------------------------------------------------
# After a user enters the user name test and password YsHsjx_202206 to log in to the device through Telnet, you can run the display access-user domain and display access-user user-id commands on the device to check the domain and access type of the user.
<DeviceA> display access-user domain default_admin
 ------------------------------------------------------------------------------
 UserID Username             IP address             MAC               Status
 ------------------------------------------------------------------------------
 16009  test                 10.135.18.217          -                 Success
 ------------------------------------------------------------------------------
 Total: 1, printed: 1
<DeviceA> display access-user user-id 16009
 Total: 1                                                               
 ------------------------------------------------------------------------------ 
Basic:
  User id                         : 16009
  User name                       : test
  Domain-name                     : default_admin
  User MAC                        : -
  User IP address                 : 10.135.18.217
  User IPv6 address               : -
  User access time                : 2024/02/15 05:10:52
  User accounting session ID      : example255255000000000f****2016009
  Option82 information            : -
  User access type                : SSH
AAA:
  User authentication type        : Administrator authentication
  Current authentication method   : Local
  Current authorization method    : Local
  Current accounting method       : None
 ------------------------------------------------------------------------------
#
sysname DeviceA
#
vlan batch 10 to 11
#
authentication-profile name p1
 dot1x-access-profile d1
 authentication mode multi-authen max-user 100
#
radius-server template rd1
 radius-server shared-key cipher %^%#Q75cNQ6IF(e#L4WMxP~%^7'u17,]D87GO{"[o]`D%^%#
 radius-server authentication 192.168.2.30 1812 weight 80
 radius-server accounting 192.168.2.30 1813 weight 80
 radius-server retransmit 2
#
aaa
 authentication-scheme abc
  authentication-mode radius
 authentication-scheme auth
 authorization-scheme autho
 accounting-scheme abc
  accounting-mode radius
 domain default
  authentication-scheme abc
  accounting-scheme abc
  radius-server rd1
 domain default_admin
  authentication-scheme auth
  authorization-scheme autho
 local-user user1-huawei password irreversible-cipher $1d$OwseVRh@LH}ZeTBm$1nH4$ab>d(N{-%0!ab48y=Ic*xEUR4pVhR2"9-~,$
 local-user user1-huawei privilege level 3
 local-user user1-huawei service-type ssh   
#
interface Vlanif11
 ip address 192.168.2.29 255.255.255.0
#
interface 10GE1/0/1
 port link-type access
 port default vlan 10
 authentication-profile p1
# 
interface 10GE1/0/2
 port link-type access
 port default vlan 11
#
stelnet server enable 
ssh server-source all-interface
#
user-interface vty 0 4           
 authentication-mode aaa           
 protocol inbound ssh
# 
dot1x-access-profile name d1
#  
return
In Figure 3-113, the gateway DeviceA functions as an authentication control device and Device_1 to Device_N function as authentication access devices. Smart access authentication needs to be performed for users on the authentication control device and their access policies need to be implemented on the authentication access devices. VLAN 10 is a user VLAN, and VLAN 20 is the management VLAN of CAPWAP tunnels. MAC address authentication is used for user access authentication.
In the policy association scenario, smart access needs to be configured on an authentication control device. For details about the authentication access device and authentication control device models, see Support for Policy Association.
# Create VLAN 10 and VLAN 20 on Device_1.
<HUAWEI> system-view
[HUAWEI] sysname Device_1
[Device_1] vlan batch 10 20
# On Device_1, configure 10GE 1/0/2 connected to users as an access interface and add it to VLAN 10.
[Device_1] interface 10ge 1/0/2
[Device_1-10GE1/0/2] port link-type access
[Device_1-10GE1/0/2] port default vlan 10 
[Device_1-10GE1/0/2] quit
# On Device_1, configure 10GE 1/0/1 connected to DeviceA as a trunk interface and configure the interface to permit packets from VLAN 10 and VLAN 20.
[Device_1] interface 10ge 1/0/1
[Device_1-10GE1/0/1] port link-type trunk
[Device_1-10GE1/0/1] port trunk allow-pass vlan 10 20
[Device_1-10GE1/0/1] quit
# Create VLAN 10 and VLAN 20 on DeviceA.
# On DeviceA, configure 10GE 1/0/1 connected to Device_1 as a trunk interface and configure the interface to permit packets from VLAN 10 and VLAN 20.
[DeviceA] interface 10ge 1/0/1
[DeviceA-10GE1/0/1] port link-type trunk
[DeviceA-10GE1/0/1] port trunk allow-pass vlan 10 20
[DeviceA-10GE1/0/1] quit
[DeviceA] dhcp enable
[DeviceA] interface vlanif 10
[DeviceA-Vlanif10] ip address 192.168.2.1 255.255.255.0
[DeviceA-Vlanif10] dhcp select interface
[DeviceA-Vlanif10] quit
[Device_1] as-mode enable
Warning: If the device switches to the AS mode, UC configurations will be deleted. Continue? [Y/N]:y 
# Create VLANIF 20 on Device_1, configure VLANIF 20 to obtain an IP address through DHCP, and specify VLANIF 20 as the source interface of a CAPWAP tunnel.
[Device_1] interface vlanif 20
[Device_1-Vlanif20] ip address dhcp-alloc
[Device_1-Vlanif20] quit
[Device_1] as access interface vlanif 20
# Create VLANIF 20 on DeviceA, configure an address pool on VLANIF 20, and specify VLANIF 20 as the source interface of a CAPWAP tunnel.
[DeviceA] interface vlanif 20
[DeviceA-Vlanif20] ip address 192.168.3.1 255.255.255.0
[DeviceA-Vlanif20] dhcp select interface
[DeviceA-Vlanif20] quit
[DeviceA] capwap source interface vlanif 20 
//When the source interface of a CAPWAP tunnel is configured for the first time, the system displays a message, asking you to check whether security-related configurations exist, including the PSK for DTLS encryption. If DTLS encryption is disabled, the message does not affect policy association configurations.
[DeviceA] as-auth
[DeviceA-as-auth] auth-mode none
Warning: None authentication is configured, which has security risks. Continue? [Y/N]: y
[DeviceA-as-auth] quit
# Configure the authentication scheme a1 and set the authentication mode to local authentication.
[DeviceA] aaa
[DeviceA-aaa] authentication-scheme a1
[DeviceA-aaa-authen-a1] authentication-mode local
[DeviceA-aaa-authen-a1] quit
# Create the authentication domain isp1 and bind the AAA authentication scheme a1 and authorization scheme b1 to the domain.
[DeviceA-aaa] domain isp1
[DeviceA-aaa-domain-isp1] authentication-scheme a1
[DeviceA-aaa-domain-isp1] quit
[DeviceA-aaa] quit
# Configure the global default domain isp1.
[DeviceA] domain isp1 access
When a user enters the user name in the format of user@isp1 for access authentication, the user is authenticated in the authentication domain isp1. If the user name does not contain a domain name or contains an invalid domain name, the user is authenticated in the default domain.
# Configure 10GE 1/0/2 on Device_1 as an access point.
[Device_1] interface 10ge 1/0/2
[Device_1-10GE1/0/2] authentication access-point
[Device_1-10GE1/0/2] quit
# Configure 10GE 1/0/1 on DeviceA as the control point.
[DeviceA] interface 10ge 1/0/1
[DeviceA-10GE1/0/1] authentication control-point
[DeviceA-10GE1/0/1] quit
[Device_1] mac-access-profile name m1
[Device_1-mac-access-profile-d1] quit
[Device_1] authentication-profile name p1
[Device_1-authen-profile-p1] mac-access-profile m1
[Device_1-authen-profile-p1] quit
[Device_1] interface 10ge 1/0/2
[Device_1-10GE1/0/2] authentication-profile p1
[Device_1-10GE1/0/2] quit
# Enable smart access globally.
[DeviceA] smart-access enable
# Configure a smart access account and authorize a service scheme. You can select one or more of the following methods:
[DeviceA] aaa
[DeviceA-aaa] service-scheme asd
[DeviceA-aaa-service-asd] quit
[DeviceA-aaa] smart-access-account mac-address 00e0-fcd4-8828 service-scheme asd description smart-access-account state active
[DeviceA-aaa] quit
[DeviceA] time-range t1 from 08:30 2025/10/1 to 18:00 2025/10/1
[DeviceA] aaa
[DeviceA-aaa] service-scheme asd
[DeviceA-aaa-service-asd] quit
[DeviceA-aaa] smart-access-policy name p1
[DeviceA-aaa-smart-access-policy-p1] rule 1 match time-range t1 apply service-scheme asd
[DeviceA-aaa-smart-access-policy-p1] quit
[DeviceA-aaa] quit
# Enable MAC address authentication on 10GE 1/0/1 of DeviceA and configure smart access authentication.
[DeviceA] mac-access-profile name d1
[DeviceA-mac-access-profile-d1] smart-access enable
[DeviceA-mac-access-profile-d1] quit
[DeviceA] authentication-profile name p1
[DeviceA-authen-profile-p1] mac-access-profile d1
[DeviceA-authen-profile-p1] quit
[DeviceA] interface 10ge 1/0/1
[DeviceA-10GE1/0/1] authentication-profile p1
[DeviceA-10GE1/0/1] quit
# Configure an authentication-free rule to allow packets from the CAPWAP tunnel management VLAN.
[DeviceA] free-rule-template name default_free_rule
[DeviceA-free-rule-default_free_rule] free-rule 1 source vlan 20
[DeviceA-free-rule-default_free_rule] quit
Administrators can run the display access-user access-type mac-authen smart-access command on the device to check information about online smart access authentication users.
Device_1
#
sysname Device_1
#
vlan batch 10 20
#
as-mode enable
#
authentication-profile name p1
 mac-access-profile m1
#
as access interface vlanif 20
#
interface Vlanif20
 ip address dhcp-alloc
#
interface 10GE1/0/1
 port link-type trunk
 port trunk allow-pass vlan 10 20
#
interface 10GE1/0/2
 port link-type access
 port default vlan 10
 authentication-profile p1
 authentication access-point
#
mac-access-profile name m1
# 
return  
#
sysname DeviceA
#
vlan batch 10 20
#
time-range t1 from 08:30 2025/10/1 to 18:00 2025/10/1
#
authentication-profile name p1
 mac-access-profile m1
#
dhcp enable
# 
free-rule-template name default_free_rule
 free-rule 1 source vlan 20
# 
aaa
 authentication-scheme a1
  authentication-mode local
 service-scheme asd
 domain isp1
  authentication-scheme a1
 smart-access-account mac-address 00e0-fcd4-8828 service-scheme asd description smart-access-account state active
 smart-access-policy name p1
  rule 1 match time-range t1 apply service-scheme asd
#
domain isp1 access
#
smart-access enable
#
interface Vlanif10
 ip address 192.168.2.1 255.255.255.0
 dhcp select interface
#
interface Vlanif20
 ip address 192.168.3.1 255.255.255.0
 dhcp select interface
#
interface 10GE1/0/1
 port link-type trunk
 port trunk allow-pass vlan 10 20
 authentication-profile p1
 authentication control-point
#
capwap source interface vlanif 20
#
as-auth
 auth-mode none
#
mac-access-profile name m1
 smart-access enable
#  
return  
In Figure 3-114, user terminals are connected to the company intranet through DeviceA. To ensure efficient and secure terminal access when no AAA server is deployed in the company, the administrator wants to configure smart access on DeviceA.
# Create VLANs, configure the allowed VLANs on interfaces, and configure IP addresses for interfaces.
<HUAWEI> system-view
[HUAWEI] sysname DeviceA
[DeviceA] vlan batch 10 20
[DeviceA] interface 10ge 1/0/1
[DeviceA-10GE1/0/1] portswitch
[DeviceA-10GE1/0/1] port link-type trunk
[DeviceA-10GE1/0/1] port trunk allow-pass vlan 10
[DeviceA-10GE1/0/1] quit
[DeviceA] interface vlanif 10
[DeviceA-Vlanif10] ip address 192.168.1.10 24
[DeviceA-Vlanif10] quit
[DeviceA] interface 10ge 1/0/2
[DeviceA-10GE1/0/2] portswitch
[DeviceA-10GE1/0/2] port link-type access
[DeviceA-10GE1/0/2] port default vlan 20
[DeviceA-10GE1/0/2] quit
[DeviceA] interface vlanif 20
[DeviceA-Vlanif20] ip address 192.168.2.10 24
[DeviceA-Vlanif20] quit
# Create the domain example.com and apply the authentication scheme a1 to the domain.
[DeviceA-aaa] domain example.com
[DeviceA-aaa-domain-example.com] authentication-scheme a1
[DeviceA-aaa-domain-example.com] quit
[DeviceA-aaa] quit
# Configure the MAC access profile m1 and configure smart access authentication.
[DeviceA] mac-access-profile name m1
[DeviceA-mac-access-profile-m1] smart-access enable
[DeviceA-mac-access-profile-m1] quit
# Configure the authentication profile p1, bind the MAC access profile m1 to the authentication profile, and specify the domain example.com as the forcible authentication domain in the authentication profile.
[DeviceA] authentication-profile name p1
[DeviceA-authen-profile-p1] mac-access-profile m1
[DeviceA-authen-profile-p1] access-domain example.com force
[DeviceA-authen-profile-p1] quit
# Bind the authentication profile p1 to 10GE 1/0/1 and enable MAC address authentication on 10GE 1/0/1.
#
sysname DeviceA
#
authentication-profile name p1
 mac-access-profile m1
 access-domain example.com force
#
vlan batch 10 20
#
time-range t1 from 08:30 2025/10/1 to 18:00 2025/10/1
#
aaa
 authentication-scheme a1    
  authentication-mode local
 service-scheme asd
 domain example.com            
  authentication-scheme a1
 smart-access-account mac-address 00e0-fcd4-8828 service-scheme asd description smart-access-account state active
 smart-access-policy name p1
  rule 1 match time-range t1 apply service-scheme asd
#
smart-access enable
#  
mac-access-profile name m1
 smart-access enable
#
interface Vlanif10
 ip address 192.168.1.10 255.255.255.0
#
interface Vlanif20
 ip address 192.168.2.10 255.255.255.0
#
interface 10GE1/0/1
 port link-type trunk
 port trunk allow-pass vlan 10
 authentication-profile p1
#
interface 10GE1/0/2
 port link-type access
 port default vlan 20
#
return 
In Figure 3-115, dumb terminals are connected to the company intranet through DeviceA. To ensure intranet security, the administrator requires that DeviceA control the network access rights of users.
Because dumb terminals cannot have the authentication client installed, MAC address authentication needs to be configured on DeviceA. As a result, MAC addresses of the dumb terminals are used as user information and sent to the RADIUS server for authentication.
[DeviceA] radius-server template rd1
[DeviceA-radius-rd1] radius-server authentication 192.168.2.30 1812
[DeviceA-radius-rd1] radius-server accounting 192.168.2.30 1813
[DeviceA-radius-rd1] radius-server shared-key cipher Huawei@123456789
[DeviceA-radius-rd1] quit
# Create the authentication scheme abc and set the authentication mode to RADIUS authentication.
# Create the accounting scheme scheme2 and set the accounting mode to RADIUS accounting.
[DeviceA-aaa] accounting-scheme scheme2
[DeviceA-aaa-accounting-scheme2] accounting-mode radius
[DeviceA-aaa-accounting-scheme2] accounting realtime 15
[DeviceA-aaa-accounting-scheme2] quit
# Create the authentication domain example.com, and bind the authentication scheme abc, accounting scheme scheme2, and RADIUS server template rd1 to the domain.
[DeviceA-aaa] domain example.com
[DeviceA-aaa-domain-example.com] authentication-scheme abc
[DeviceA-aaa-domain-example.com] accounting-scheme scheme2
[DeviceA-aaa-domain-example.com] radius-server rd1
[DeviceA-aaa-domain-example.com] quit
[DeviceA-aaa] quit
# Check whether a user can pass RADIUS authentication. (The test user test1 and password test1@123 have been configured on the RADIUS server.)
[DeviceA] test-aaa test1 test1@123 radius-template rd1
Info: Account test succeeded.
# Configure the MAC access profile m1.
In a MAC access profile, the user name and password for MAC address authentication are MAC addresses without hyphens (-) by default. Ensure that the user name and password used for MAC address authentication configured on the RADIUS server are the same as those configured on the access device.
[DeviceA] mac-access-profile name m1
[DeviceA-mac-access-profile-m1] quit
# Configure the authentication profile p1, bind the MAC access profile m1 to the authentication profile, specify the domain example.com as the forcible authentication domain in the authentication profile, set the user access mode to multi-authen, and set the maximum number of access users to 100.
[DeviceA] authentication-profile name p1
[DeviceA-authen-profile-p1] mac-access-profile m1
[DeviceA-authen-profile-p1] access-domain example.com force
[DeviceA-authen-profile-p1] authentication mode multi-authen max-user 100
[DeviceA-authen-profile-p1] quit
#
sysname DeviceA
#
authentication-profile name p1
 mac-access-profile m1
 access-domain example.com force
 authentication mode multi-authen max-user 100
#
vlan batch 10 20
#
aaa
 authentication-scheme abc    
  authentication-mode radius    
 accounting-scheme scheme2
  accounting-mode radius
  accounting realtime 15
 domain example.com            
  authentication-scheme abc     
  accounting-scheme scheme2
  radius-server rd1      
#  
radius-server template rd1 
 radius-server shared-key cipher %+%##!!!!!!!!!"!!!!"!!!!*!!!!SKvr${[Fs.3t@/5k|BENhEu>W(3\~XG!!D;!!!!!2jp5!!!!!!A!!!!3"pK8qv!}9M#(4$jGWvQF/R[CNe/+:W^jk8HUe&W%+%#
 radius-server authentication 192.168.2.30 1812 weight 80
 radius-server accounting 192.168.2.30 1813 weight 80
#
mac-access-profile name m1
#
interface Vlanif10
 ip address 192.168.1.10 255.255.255.0
#
interface Vlanif20
 ip address 192.168.2.10 255.255.255.0
#
interface 10GE 1/0/1
 port link-type trunk
 port trunk allow-pass vlan 10
 authentication-profile p1
#
interface 10GE 1/0/2
 port link-type access
 port default vlan 20
#
return 
In Figure 3-116, dumb terminals are connected to the company intranet through DeviceA. To ensure intranet security, the administrator requires that DeviceA control the network access rights of users.
Because dumb terminals cannot have the authentication client installed, MAC address authentication needs to be configured on DeviceA. In addition, local authentication is used to authenticate users.
# Configure the authorization scheme b1 and set the authorization mode to local authorization.
[DeviceA-aaa] authorization-scheme b1
[DeviceA-aaa-author-b1] authorization-mode local
[DeviceA-aaa-author-b1] quit
# Configure the user name, password, and access type of a local user.
Configure a terminal's MAC address as the local user name, set the password to Example@123, and set the access type to MAC address authentication (dot1x). The following assumes that the MAC address of printer 1 is 00e0-fcd4-8828.
[DeviceA-aaa] local-access-user 00e0-fcd4-8828
[DeviceA-aaa-access-user-00e0-fcd4-8828] password cipher Example@123
[DeviceA-aaa-access-user-00e0-fcd4-8828] service-type dot1x
Warning: If the service type of local access users is configured multiple times, the new configuration overwrites the previous one. The new configuration may cause local access users unable to go online. Continue? [Y/N]:y
[DeviceA-aaa-access-user-00e0-fcd4-8828] quit
# Configure the domain example.com, and apply the authentication scheme a1 and authorization scheme b1 to the domain.
[DeviceA-aaa] domain example.com
[DeviceA-aaa-domain-example.com] authentication-scheme a1
[DeviceA-aaa-domain-example.com] authorization-scheme b1
[DeviceA-aaa-domain-example.com] quit
[DeviceA-aaa] quit
When AAA local authentication and authorization are used, the user name and password used for MAC address authentication must be the same as those of the AAA local user. In this example, the user name of the local user is the terminal's MAC address containing hyphens (-), and the password is Example@123.
[DeviceA] mac-access-profile name m1
[DeviceA-mac-access-profile-m1] mac-authen username macaddress format with-hyphen password cipher Example@123
[DeviceA-mac-access-profile-m1] quit
[DeviceA] interface 10ge 1/0/1
[DeviceA-10GE 1/0/1] authentication-profile p1
[DeviceA-10GE 1/0/1] quit
#
sysname DeviceA
#
authentication-profile name p1
 mac-access-profile m1
 access-domain example.com force
#
vlan batch 10 20
#
aaa
 authentication-scheme a1    
  authentication-mode local
 authorization-scheme b1
  authorization-mode local
 domain example.com            
  authentication-scheme a1
  authorization-scheme b1
 local-access-user 00e0-fcd4-8828
  password cipher %+%##!!!!!!!!!"!!!!"!!!!*!!!!SKvr${[Fs.<FvBB,.w;M75IN5Z>'!L8G:n-!!!!!2jp5!!!!!!<!!!!k9&fPO<BSRW}jPT(,ewKyfIL"zVtM1~=>e.!!!!!%+%# 
  service-type dot1x 
#  
mac-access-profile name m1
 mac-authen username macaddress format with-hyphen password cipher %+%##!!!!!!!!!"!!!!"!!!!*!!!!SKvr${[Fs.vW-9,<\Dz<6:AiLq@Ls2I%l3/!!!!!2jp5!!!!!!<!!!!5v2C4u&Z-8<3C6-//E:GF\uP8a9Le$7xWEF!!!!!%+%#
#
interface Vlanif10
 ip address 192.168.1.10 255.255.255.0
#
interface Vlanif20
 ip address 192.168.2.10 255.255.255.0
#
interface 10GE1/0/1
 port link-type trunk
 port trunk allow-pass vlan 10
 authentication-profile p1
#
interface 10GE1/0/2
 port link-type access
 port default vlan 20
#
return 
As shown in Figure 3-117, DeviceA functioning as a WAC in an enterprise is directly connected to an AP. The enterprise deploys the WLAN named wlan-net to provide wireless network access for employees. The WAC functions as a DHCP server to assign IP addresses on the network segment 10.23.101.0/24 to STAs.
As the WLAN is open to users, there are potential security risks to enterprise information if no access control is configured for the WLAN. To meet the enterprise's security requirements, MAC address authentication is used for authenticating dumb terminals such as wireless printers and wireless phones that cannot have an authentication client installed. In this case, MAC addresses of terminals are used as user information and sent to the RADIUS server for authentication. When users connect to the WLAN, they do not need to enter their user names and passwords for authentication.
| Configuration Item | Data | 
|---|---|
| RADIUS authentication parameters |  | 
| MAC access profile |  | 
| Authentication domain | The authentication domain is named example.com and is bound to:  | 
| Authentication profile |  | 
| DHCP server | The WAC functions as a DHCP server to assign IP addresses to the AP and STAs. | 
| IP address pool for the AP | 10.23.100.2 to 10.23.100.254/24 | 
| IP address pool for STAs | 10.23.101.2 to 10.23.101.254/24 | 
| IP address of the WAC's source interface | VLANIF 100: 10.23.100.1/24 | 
| AP group |  | 
| Regulatory domain profile |  | 
| SSID profile |  | 
| Security profile |  | 
| VAP profile |  | 
# Configure the WAC and add 10GE 1/0/1 to VLAN 100 (management VLAN).
In this example, the service data forwarding mode is tunnel forwarding. In this mode, the management VLAN and service VLAN cannot be the same.
In direct forwarding mode, you are advised to configure port isolation on the WAC's interface 10GE 1/0/1 connected to the AP. If port isolation is not configured, unnecessary broadcast packets will be transmitted in VLANs or WLAN users on different APs will be able to communicate with each other at Layer 2.
<HUAWEI> system-view
[HUAWEI] sysname DeviceA
[DeviceA] vlan batch 100 101
[DeviceA] interface 10ge 1/0/1
[DeviceA-10GE1/0/1] portswitch
[DeviceA-10GE1/0/1] port link-type trunk
[DeviceA-10GE1/0/1] port trunk pvid vlan 100
[DeviceA-10GE1/0/1] port trunk allow-pass vlan 100
[DeviceA-10GE1/0/1] quit
# Add the WAC's uplink interface 10GE 1/0/2 to VLAN 101 (service VLAN).
[DeviceA] interface 10ge 1/0/2
[DeviceA-10GE1/0/2] portswitch
[DeviceA-10GE1/0/2] port link-type trunk
[DeviceA-10GE1/0/2] port trunk allow-pass vlan 101
[DeviceA-10GE1/0/2] quit
# Configure the WAC as a DHCP server to assign an IP address to the AP from the IP address pool on VLANIF 100, and assign IP addresses to STAs from the IP address pool on VLANIF 101.
[DeviceA] dhcp enable
[DeviceA] interface vlanif 100
[DeviceA-Vlanif100] ip address 10.23.100.1 24
[DeviceA-Vlanif100] dhcp select interface
[DeviceA-Vlanif100] quit
[DeviceA] interface vlanif 101
[DeviceA-Vlanif101] ip address 10.23.101.1 24
[DeviceA-Vlanif101] dhcp select interface
[DeviceA-Vlanif101] quit
[DeviceA] ip route-static 10.23.200.0 255.255.255.0 10.23.101.2
# Create an AP group to which the APs with the same configuration can be added.
[DeviceA] wlan
[DeviceA-wlan] ap-group name ap-group1
[DeviceA-wlan-ap-group-ap-group1] quit
# Create a regulatory domain profile, configure the WAC country code in the profile, and bind the profile to the AP group.
[DeviceA-wlan] regulatory-domain-profile name domain1
[DeviceA-wlan-regulate-domain-domain1] country-code cn
Warning: Modifying the country code will clear the channel and power configurations of radios, and requires the APs to be restarted if they run V200R019C10 or earlier. Continue? [Y/N]:y
[DeviceA-wlan-regulate-domain-domain1] quit
[DeviceA-wlan] ap-group name ap-group1
[DeviceA-wlan-ap-group-ap-group1] regulatory-domain-profile domain1
Warning: This configuration change will clear the channel and power configurations of radios, and may restart APs. Continue?[Y/N]:y
[DeviceA-wlan-ap-group-ap-group1] quit
[DeviceA-wlan] quit
# Configure the WAC's source interface.
[DeviceA] capwap dtls no-auth enable
Warning: This operation allows for device access in non-DTLS encryption mode even when DTLS is enabled and brings security risks. Af
ter the device goes online for the first time, disable this function to prevent security risks. Continue? [Y/N]:y
[DeviceA] capwap source interface vlanif 100
Set the DTLS PSK(contains 8-32 plain-text characters, or 128 or 148 cipher-text characters that must be a combination of at least tw
o of the following: lowercase letters a to z, uppercase letters A to Z, digits, and special characters):********
Confirm PSK:********
Set the user name for FIT APs(The value is a string of 4 to 31 characters, which can contain letters, underscores, and digits, and m
ust start with a letter):********
Set the password for FIT APs(plain-text password of 8-128 characters or cipher-text password of 128-268 characters that must be a co
mbination of at least three of the following: lowercase letters a to z, uppercase letters A to Z, digits, and special characters):******
Confirm password:********
Set the PSK of the global offline management VAP(plain-text password of 8-63 characters or cipher-text password of 128-188 character
s that must be a combination of at least two of the following: lowercase letters a to z, uppercase letters A to Z, digits, and speci
al characters):********
Confirm PSK:********
After the peer device goes online, run the undo capwap dtls no-auth enable command to disable the function of establishing CAPWAP DTLS sessions in none authentication mode, thereby preventing unauthorized devices from accessing the network.
The default AP authentication mode is MAC address authentication. If the default settings have not been changed, you do not need to run the ap auth-mode mac-auth command.
[DeviceA] wlan
[DeviceA-wlan] ap auth-mode mac-auth
[DeviceA-wlan] ap-id 0 ap-mac 00e0-fc12-3456
[DeviceA-wlan-ap-0] ap-name area_1
[DeviceA-wlan-ap-0] ap-group ap-group1
Warning: This operation may cause AP reset. If the country code changes, it will clear channel, power and antenna gain configuration s of the radio, Whether to continue? [Y/N]:y
[DeviceA-wlan-ap-0] quit
[DeviceA-wlan] quit
# After the AP goes online, disable the function of establishing CAPWAP DTLS sessions in non-authentication mode.
[DeviceA] undo capwap dtls no-auth enable
Ensure that the RADIUS server address, port number, and shared key are correctly configured and are the same as those on the RADIUS server.
# Configure a RADIUS server template.
[DeviceA] radius-server template radius_huawei
[DeviceA-radius-radius_huawei] radius-server authentication 10.23.200.1 1812
[DeviceA-radius-radius_huawei] radius-server accounting 10.23.200.1 1813
[DeviceA-radius-radius_huawei] radius-server shared-key cipher YsHsjx_202206mc@1
[DeviceA-radius-radius_huawei] quit
# Configure an authentication scheme that uses RADIUS authentication.
[DeviceA] aaa
[DeviceA-aaa] authentication-scheme scheme1
[DeviceA-aaa-authen-scheme1] authentication-mode radius
[DeviceA-aaa-authen-scheme1] quit
# Configure an accounting scheme that uses RADIUS accounting.
# Create the authentication domain example.com and bind an authentication scheme, an accounting scheme, and a RADIUS server template to the authentication domain.
[DeviceA-aaa] domain example.com
[DeviceA-aaa-domain-example.com] authentication-scheme scheme1
[DeviceA-aaa-domain-example.com] accounting-scheme scheme2
[DeviceA-aaa-domain-example.com] radius-server radius_huawei
[DeviceA-aaa-domain-example.com] quit
[DeviceA-aaa] quit
[DeviceA] authentication-profile name p1
[DeviceA-authentication-profile-p1] mac-access-profile m1
[DeviceA-authentication-profile-p1] access-domain example.com force
[DeviceA-authentication-profile-p1] quit
# Create the security profile wlan-security and configure a security policy in the profile.
[DeviceA] wlan
[DeviceA-wlan] security-profile name wlan-security
[DeviceA-wlan-sec-prof-wlan-security] security open
[DeviceA-wlan-sec-prof-wlan-security] quit
# Create the SSID profile wlan-ssid and set the SSID name to wlan-net.
[DeviceA-wlan] ssid-profile name wlan-ssid
[DeviceA-wlan-ssid-prof-wlan-ssid] ssid wlan-net
[DeviceA-wlan-ssid-prof-wlan-ssid] quit
# Create the VAP profile wlan-vap, configure the service data forwarding mode and service VLAN, and bind the security profile, SSID profile, and authentication profile to the VAP profile.
[DeviceA-wlan] vap-profile name wlan-vap
[DeviceA-wlan-vap-prof-wlan-vap] forward-mode tunnel
[DeviceA-wlan-vap-prof-wlan-vap] service-vlan vlan-id 101
[DeviceA-wlan-vap-prof-wlan-vap] security-profile wlan-security
[DeviceA-wlan-vap-prof-wlan-vap] ssid-profile wlan-ssid
[DeviceA-wlan-vap-prof-wlan-vap] authentication-profile p1
[DeviceA-wlan-vap-prof-wlan-vap] quit
# Bind the VAP profile wlan-vap to the AP group and apply the profile to radios 0 and 1 of the APs.
[DeviceA-wlan] ap-group name ap-group1
[DeviceA-wlan-ap-group-ap-group1] vap-profile wlan-vap wlan 1 radio 0
[DeviceA-wlan-ap-group-ap-group1] vap-profile wlan-vap wlan 1 radio 1
[DeviceA-wlan-ap-group-ap-group1] quit
The automatic channel and power calibration functions are enabled by default. The manual channel and power configurations take effect only when the two functions are disabled. The channel and power configurations for the AP radios in this example are for reference only. In practice, configure the channel and power of AP radios based on the actual country codes of APs and network planning.
[DeviceA-wlan] ap-id 0
[DeviceA-wlan-ap-0] radio 0
[DeviceA-wlan-ap-0-radio-0] calibrate auto-channel-select disable
[DeviceA-wlan-ap-0-radio-0] calibrate auto-txpower-select disable
[DeviceA-wlan-ap-0-radio-0] channel 20mhz 6
Warning: This action may cause service interruption. Continue?[Y/N]y 
[DeviceA-wlan-ap-0-radio-0] eirp 127
[DeviceA-wlan-ap-0-radio-0] quit
[DeviceA-wlan-ap-0] radio 1
[DeviceA-wlan-ap-0-radio-1] calibrate auto-channel-select disable
[DeviceA-wlan-ap-0-radio-1] calibrate auto-txpower-select disable
[DeviceA-wlan-ap-0-radio-1] channel 80mhz 149
Warning: This action may cause service interruption. Continue?[Y/N]y 
[DeviceA-wlan-ap-0-radio-1] eirp 127
[DeviceA-wlan-ap-0-radio-1] quit
[DeviceA-wlan-ap-0] quit
[DeviceA-wlan] quit
#
sysname DeviceA
#
vlan batch 100 to 101
#
authentication-profile name p1
 mac-access-profile m1
 access-domain example.com force
#
mac-access-profile name m1
#
dhcp enable
#
radius-server template radius_huawei
 radius-server shared-key cipher %+%##!!!!!!!!!"!!!!"!!!!*!!!!Cd/`W03KjAwAqn64E<\TxGC_SOri<2BP\A+!!!!!2jp5!!!!!!B!!!!Oe2HMc->XMa#TLDZUaJBFJtm#XVj*E:S*|(N7`J1B:3QY!!!!!!!!!!!!!!!%+%# 
 radius-server authentication 10.23.200.1 1812 weight 80
 radius-server accounting 10.23.200.1 1813 weight 80
#
aaa
 authentication-scheme scheme1
  authentication-mode radius
 accounting-scheme scheme2
  accounting-mode radius
  accounting realtime 15
 domain example.com
  authentication-scheme scheme1
  accounting-scheme scheme2
  radius-server radius_huawei
#
interface Vlanif100
 ip address 10.23.100.1 255.255.255.0
 dhcp select interface
#
interface Vlanif101
 ip address 10.23.101.1 255.255.255.0
 dhcp select interface
#
interface 10GE1/0/1
 port link-type trunk
 port trunk pvid vlan 100
 port trunk allow-pass vlan 100
#
interface 10GE1/0/2
 port link-type trunk
 port trunk allow-pass vlan 101
#
ip route-static 10.23.200.0 255.255.255.0 10.23.101.2 
#  
capwap source interface vlanif 100
capwap dtls psk %+%##!!!!!!!!!"!!!!"!!!!*!!!!a4O4$.&PxRTGtHR(VaB4*(KhWT"C@K+HZ%@!!!!!2jp5!!!!!!;!!!!..AjEF}u<=!%e=F%Dz-Y[R--3yrC-*MVXf<!!!!!%+%#
#
wlan
 security-profile name wlan-security
  security open
 ssid-profile name wlan-ssid
  ssid wlan-net
 vap-profile name wlan-vap
  forward-mode tunnel
  service-vlan vlan-id 101
  ssid-profile wlan-ssid
  security-profile wlan-security
  authentication-profile p1
 regulatory-domain-profile name domain1
 ap-group name ap-group1
  regulatory-domain-profile domain1
  radio 0
   vap-profile wlan-vap wlan 1
  radio 1
   vap-profile wlan-vap wlan 1
 ap-id 0 type-id 1 ap-mac 00e0-fc12-3456 ap-sn 210235554710CB000042
  ap-name area_1
  ap-group ap-group1
  radio 0
   channel 20mhz 6
   eirp 127
   calibrate auto-channel-select disable  
   calibrate auto-txpower-select disable
  radio 1
   channel 80mhz 149
   eirp 127
   calibrate auto-channel-select disable 
   calibrate auto-txpower-select disable
#
return
In Figure 3-118, DeviceA functioning as a WAC in an enterprise is directly connected to an AP. The enterprise deploys the WLAN named wlan-net to provide wireless network access for employees. The WAC functions as a DHCP server to assign IP addresses on the network segment 10.23.101.0/24 to STAs.
As the WLAN is open to users, there are potential security risks to enterprise information if no access control is configured for the WLAN. Dumb terminals (such as printers) in the physical access control department cannot have an authentication client installed. To meet the enterprise's security requirements, MAC address authentication can be configured on the WAC to authenticate dumb terminals in local authentication mode.
| Item | Data | 
|---|---|
| Local authentication parameters | Local authentication scheme name: a1 User name, password, and access type of the local user (STA1 is used as an example):  | 
| MAC access profile |  | 
| Authentication domain | The authentication domain is named example.com and is bound to:  | 
| Authentication profile |  | 
| DHCP server | The WAC functions as a DHCP server to assign IP addresses to the AP and STAs. | 
| IP address pool for the AP | 10.23.100.2/24 to 10.23.100.254/24 | 
| IP address pool for STAs | 10.23.101.2/24 to 10.23.101.254/24 | 
| IP address of the WAC's source interface | VLANIF 100: 10.23.100.1/24 | 
| AP group |  | 
| Regulatory domain profile |  | 
| SSID profile |  | 
| Security profile |  | 
| VAP profile |  | 
Configure a terminal's MAC address as the local user name, set the password to Example@123, and set the access type to MAC address authentication (dot1x). In this example, the MAC address of printer 1 is 00e0-fcd4-8828.
#
sysname DeviceA
#
vlan batch 100 to 101
#
authentication-profile name p1
 mac-access-profile m1
 access-domain example.com force
#
mac-access-profile name m1
 mac-authen username macaddress format with-hyphen password cipher %^%#PW~_5m;sAFFI.cEB"%^@6@4$96ds_5+O'28+d3:A%^%# 
#
dhcp enable
#
aaa
 authentication-scheme a1    
  authentication-mode local
 authorization-scheme b1
  authorization-mode local
 domain example.com            
  authentication-scheme a1
  authorization-scheme b1
 local-access-user 00e0-fcd4-8828
  password cipher %+%##!!!!!!!!!"!!!!"!!!!*!!!!SKvr${[Fs.<FvBB,.w;M75IN5Z>'!L8G:n-!!!!!2jp5!!!!!!<!!!!k9&fPO<BSRW}jPT(,ewKyfIL"zVtM1~=>e.!!!!!%+%# 
  service-type dot1x 
# 
interface Vlanif100
 ip address 10.23.100.1 255.255.255.0
 dhcp select interface
#
interface Vlanif101
 ip address 10.23.101.1 255.255.255.0
 dhcp select interface
#
interface 10GE1/0/1
 port link-type trunk
 port trunk pvid vlan 100
 port trunk allow-pass vlan 100
#
interface 10GE1/0/2
 port link-type trunk
 port trunk allow-pass vlan 101
#
capwap source interface vlanif 100
capwap dtls psk %+%##!!!!!!!!!"!!!!"!!!!*!!!!a4O4$.&PxRTGtHR(VaB4*(KhWT"C@K+HZ%@!!!!!2jp5!!!!!!;!!!!..AjEF}u<=!%e=F%Dz-Y[R--3yrC-*MVXf<!!!!!%+%#
#
wlan
 security-profile name wlan-security
  security open
 ssid-profile name wlan-ssid
  ssid wlan-net
 vap-profile name wlan-vap
  forward-mode tunnel
  service-vlan vlan-id 101
  ssid-profile wlan-ssid
  security-profile wlan-security
  authentication-profile p1
 regulatory-domain-profile name domain1
 ap-group name ap-group1
  regulatory-domain-profile domain1
  radio 0
   vap-profile wlan-vap wlan 1
  radio 1
   vap-profile wlan-vap wlan 1
 ap-id 0 type-id 1 ap-mac 00e0-fc12-3456 ap-sn 210235554710CB000042
  ap-name area_1
  ap-group ap-group1
  radio 0
   channel 20mhz 6
   eirp 127
   calibrate auto-channel-select disable  
   calibrate auto-txpower-select disable
  radio 1
   channel 80mhz 149
   eirp 127
   calibrate auto-channel-select disable 
   calibrate auto-txpower-select disable
#
return
In Figure 3-119, DeviceA functioning as a WAC in an enterprise is directly connected to an AP. The enterprise deploys the WLAN named wlan-net to provide wireless network access for employees. The WAC functions as a DHCP server to assign IP addresses on the network segment 10.23.101.0/24 to STAs.
As the WLAN is open to users, there are potential security risks to enterprise information if no access control is available to the WLAN. To meet the enterprise's high security requirements, 802.1X authentication is used for authenticating wireless users through a RADIUS server.
| Item | Data | 
|---|---|
| RADIUS authentication parameters | Authentication scheme name: scheme1 Accounting scheme name: scheme2 RADIUS server template name: radius_huawei  | 
| Authentication domain | The authentication domain is named example.com and is bound to:  | 
| 802.1X access profile |  | 
| Authentication profile |  | 
| DHCP server | The WAC functions as a DHCP server to assign IP addresses to the AP and STAs. | 
| IP address pool for the AP | 10.23.100.2 to 10.23.100.254/24 | 
| IP address pool for STAs | 10.23.101.2 to 10.23.101.254/24 | 
| IP address of the WAC's source interface | VLANIF 100: 10.23.100.1/24 | 
| AP group |  | 
| Regulatory domain profile |  | 
| SSID profile |  | 
| Security profile |  | 
| VAP profile |  | 
By default, an 802.1X access profile uses EAP authentication. Ensure that the RADIUS server supports the EAP protocol. Otherwise, the RADIUS server cannot process 802.1X authentication requests.
[DeviceA] authentication-profile name p1
[DeviceA-authentication-profile-p1] dot1x-access-profile d1
[DeviceA-authentication-profile-p1] access-domain example.com force
[DeviceA-authentication-profile-p1] quit
[DeviceA] wlan
[DeviceA-wlan] security-profile name wlan-security
[DeviceA-wlan-sec-prof-wlan-security] security wpa2 dot1x aes
[DeviceA-wlan-sec-prof-wlan-security] quit
Configuration in the Windows XP operating system:
Configuration in the Windows 7 operating system:
#
sysname DeviceA
#
vlan batch 100 to 101
#
authentication-profile name p1
 dot1x-access-profile d1
 access-domain example.com force
#
dot1x-access-profile name d1
#
dhcp enable
#
radius-server template radius_huawei
 radius-server shared-key cipher %+%##!!!!!!!!!"!!!!"!!!!*!!!!Cd/`W03KjAwAqn64E<\TxGC_SOri<2BP\A+!!!!!2jp5!!!!!!B!!!!Oe2HMc->XMa#TLDZUaJBFJtm#XVj*E:S*|(N7`J1B:3QY!!!!!!!!!!!!!!!%+%# 
 radius-server authentication 10.23.200.1 1812 weight 80
 radius-server accounting 10.23.200.1 1813 weight 80
#
aaa
 authentication-scheme scheme1
  authentication-mode radius
 accounting-scheme scheme2
  accounting-mode radius
  accounting realtime 15
 domain example.com
  authentication-scheme scheme1
  accounting-scheme scheme2
  radius-server radius_huawei
#
interface Vlanif100
 ip address 10.23.100.1 255.255.255.0
 dhcp select interface
#
interface Vlanif101
 ip address 10.23.101.1 255.255.255.0
 dhcp select interface
#
interface 10GE1/0/1
 port link-type trunk
 port trunk pvid vlan 100
 port trunk allow-pass vlan 100
#
interface 10GE1/0/2
 port link-type trunk
 port trunk allow-pass vlan 101
#
ip route-static 10.23.200.0 255.255.255.0 10.23.101.2 
#  
capwap source interface vlanif 100
capwap dtls psk %+%##!!!!!!!!!"!!!!"!!!!*!!!!a4O4$.&PxRTGtHR(VaB4*(KhWT"C@K+HZ%@!!!!!2jp5!!!!!!;!!!!..AjEF}u<=!%e=F%Dz-Y[R--3yrC-*MVXf<!!!!!%+%#
#
wlan
 security-profile name wlan-security
  security wpa2 dot1x aes
 ssid-profile name wlan-ssid
  ssid wlan-net
 vap-profile name wlan-vap
  forward-mode tunnel
  service-vlan vlan-id 101
  ssid-profile wlan-ssid
  security-profile wlan-security
  authentication-profile p1
 regulatory-domain-profile name domain1
 ap-group name ap-group1
  regulatory-domain-profile domain1
  radio 0
   vap-profile wlan-vap wlan 1
  radio 1
   vap-profile wlan-vap wlan 1
 ap-id 0 type-id 1 ap-mac 00e0-fc12-3456 ap-sn 210235554710CB000042
  ap-name area_1
  ap-group ap-group1
  radio 0
   channel 20mhz 6
   eirp 127
   calibrate auto-channel-select disable  
   calibrate auto-txpower-select disable
  radio 1
   channel 80mhz 149
   eirp 127
   calibrate auto-channel-select disable 
   calibrate auto-txpower-select disable
#
return
In Figure 3-120, terminals in a company's office area are connected to the company's intranet through DeviceB. DeviceB functioning as an access switch is connected to 10GE 1/0/2 on DeviceA functioning as an aggregation switch through 10GE 1/0/1.
To meet the enterprise's high security requirements, 802.1X authentication needs to be configured to authenticate terminals in the office area through the RADIUS server. Additionally, to facilitate maintenance and reduce the number of authentication points, an authentication point needs to be deployed on 10GE 1/0/2 of DeviceA.
In the 802.1X authentication scenario, if there is a Layer 2 switch between the 802.1X-enabled device and users, Layer 2 transparent transmission must be enabled for 802.1X authentication packets on the Layer 2 switch; otherwise, users cannot be successfully authenticated. This function needs to be configured on all interfaces through which 802.1X authentication packets pass.
<HUAWEI> system-view
[HUAWEI] sysname DeviceB
[DeviceB] l2protocol-tunnel user-defined-protocol 802.1X protocol-mac 0180-c200-0003 group-mac 0100-0000-0002
[DeviceB] interface 10ge 1/0/1
[DeviceB-10GE1/0/1] l2protocol-tunnel user-defined-protocol 802.1X enable
[DeviceB-10GE1/0/1] port link-type trunk
[DeviceB-10GE1/0/1] port trunk allow-pass vlan 20
[DeviceB-10GE1/0/1] quit
[DeviceB] interface 10ge 1/0/2
[DeviceB-10GE1/0/2] l2protocol-tunnel user-defined-protocol 802.1X enable
[DeviceB-10GE1/0/2] port link-type access
[DeviceB-10GE1/0/2] port default vlan 20
[DeviceB-10GE1/0/2] quit
<HUAWEI> system-view
[HUAWEI] sysname DeviceA
[DeviceA] vlan batch 10 20
[DeviceA] interface 10ge 1/0/1
[DeviceA-10GE1/0/1] port link-type trunk
[DeviceA-10GE1/0/1] port trunk allow-pass vlan 10
[DeviceA-10GE1/0/1] quit
[DeviceA] interface 10ge 1/0/2
[DeviceA-10GE1/0/2] port link-type trunk
[DeviceA-10GE1/0/2] port trunk allow-pass vlan 20
[DeviceA-10GE1/0/2] quit
[DeviceA] interface vlanif 10
[DeviceA-Vlanif10] ip address 192.168.1.10 24
[DeviceA-Vlanif10] quit
[DeviceA] interface vlanif 20  
[DeviceA-Vlanif20] ip address 192.168.2.10 24
[DeviceA-Vlanif20] quit
[DeviceA] radius-server template rd1
[DeviceA-radius-rd1] radius-server authentication 192.168.1.30 1812
[DeviceA-radius-rd1] radius-server accounting 192.168.1.30 1813
[DeviceA-radius-rd1] radius-server shared-key cipher YsHsjx_202206mc@1
[DeviceA-radius-rd1] quit
Check whether a user can pass RADIUS authentication. (The test user test and password YsHsjx_2022061 have been configured on the RADIUS server.)
[DeviceA] test-aaa test YsHsjx_2022061 radius-template rd1
Info: Account test succeeded.
[DeviceA] dot1x-access-profile name d1
[DeviceA-dot1x-access-profile-d1] dot1x authentication-method eap
[DeviceA-dot1x-access-profile-d1] dot1x timer client-timeout 30
[DeviceA-dot1x-access-profile-d1] quit
Configure the authentication profile p1, bind the 802.1X access profile d1 to the authentication profile, specify the domain example.com as the forcible authentication domain in the authentication profile, set the user access mode to multi-authen, and set the maximum number of access users to 100.
[DeviceA] authentication-profile name p1
[DeviceA-authen-profile-p1] dot1x-access-profile d1
[DeviceA-authen-profile-p1] access-domain example.com force
[DeviceA-authen-profile-p1] authentication mode multi-authen max-user 100
[DeviceA-authen-profile-p1] quit
# Bind the authentication profile p1 to 10GE 1/0/2 and enable 802.1X authentication on 10GE 1/0/2.
[DeviceA] interface 10ge 1/0/2
[DeviceA-10GE1/0/2] authentication-profile p1
[DeviceA-10GE1/0/2] quit
#
sysname DeviceB
#
vlan batch 20 
#
l2protocol-tunnel user-defined-protocol 802.1X protocol-mac 0180-c200-0003 group-mac 0100-0000-0002
#
interface 10GE 1/0/1
 l2protocol-tunnel user-defined-protocol 802.1X enable
 port link-type trunk
 port trunk allow-pass vlan 20
#
interface 10GE 1/0/2
 l2protocol-tunnel user-defined-protocol 802.1X enable
 port link-type access
 port default vlan 20
#
return
#
sysname DeviceA
#
vlan batch 10 20 
#
authentication-profile name p1
 dot1x-access-profile d1
 access-domain example.com force
 authentication mode multi-authen max-user 100
#
radius-server template rd1
 radius-server shared-key cipher %+%##!!!!!!!!!"!!!!"!!!!*!!!!Cd/`W03KjAwAqn64E<\TxGC_SOri<2BP\A+!!!!!2jp5!!!!!!B!!!!Oe2HMc->XMa#TLDZUaJBFJtm#XVj*E:S*|(N7`J1B:3QY!!!!!!!!!!!!!!!%+%# 
 radius-server authentication 192.168.1.30 1812 weight 80
 radius-server accounting 192.168.1.30 1813 weight 80
#
interface 10GE1/0/1
 port link-type trunk
 port trunk allow-pass vlan 10
#
interface 10GE1/0/2
 port link-type trunk
 port trunk allow-pass vlan 20
 authentication-profile p1
#
interface vlanif 10
 ip address 192.168.1.10 255.255.255.0
#
interface vlanif 20
 ip address 192.168.2.10 255.255.255.0
#
aaa
 authentication-scheme abc
  authentication-mode radius
 accounting-scheme scheme2
  accounting-mode radius
  accounting realtime 15
 domain example.com
  authentication-scheme abc
  accounting-scheme scheme2
  radius-server rd1
#
dot1x-access-profile name d1
 dot1x timer client-timeout 30
#
return
In Figure 3-121, terminals in a company's office area are connected to the company's intranet through DeviceA. The downlink interfaces (for example, 10GE 1/0/2) of DeviceA are directly connected to terminals in the office area, and the uplink interface 10GE 1/0/1 of DeviceA is connected to the RADIUS server through the intranet.
To meet the company's high security requirements, 802.1X authentication needs to be configured to authenticate terminals in the office area through the RADIUS server. Additionally, authentication points need to be deployed on DeviceA's interfaces (for example, 10GE 1/0/2) that are directly connected to the terminals.
[DeviceA] radius-server template rd1
[DeviceA-radius-rd1] radius-server authentication 192.168.1.30 1812
[DeviceA-radius-rd1] radius-server accounting 192.168.1.30 1813
[DeviceA-radius-rd1] radius-server shared-key cipher Huawei@123456789
[DeviceA-radius-rd1] quit
# Create the AAA authentication scheme abc and set the authentication mode to RADIUS authentication.
# Check whether a user can pass RADIUS authentication. (The test user test and password Example2012 have been configured on the RADIUS server.)
[DeviceA] test-aaa test Example2012 radius-template rd1
Info: Account test succeeded.
# Configure the authentication profile p1, bind the 802.1X access profile d1 to the authentication profile, and configure the forcible authentication domain example.com for users using the authentication profile.
[DeviceA] authentication-profile name p1
[DeviceA-authen-profile-p1] dot1x-access-profile d1
[DeviceA-authen-profile-p1] access-domain example.com force
[DeviceA-authen-profile-p1] quit
# Bind the authentication profile p1 to a downlink interface and enable 802.1X authentication on the interface. The following uses 10GE 1/0/2 as an example. Other interfaces have similar configurations.
#
sysname DeviceA
#
authentication-profile name p1
 dot1x-access-profile d1
 access-domain example.com force
#
vlan batch 10 20
#
aaa
 authentication-scheme abc    
  authentication-mode radius    
 accounting-scheme scheme2
  accounting-mode radius
  accounting realtime 15
 domain example.com            
  authentication-scheme abc     
  accounting-scheme scheme2
  radius-server rd1      
#  
radius-server template rd1
 radius-server shared-key cipher %+%##!!!!!!!!!"!!!!"!!!!*!!!!SKvr${[Fs.3t@/5k|BENhEu>W(3\~XG!!D;!!!!!2jp5!!!!!!A!!!!3"pK8qv!}9M#(4$jGWvQF/R[CNe/+:W^jk8HUe&W%+%#
 radius-server authentication 192.168.1.30 1812 weight 80
 radius-server accounting 192.168.1.30 1813 weight 80
#
dot1x-access-profile name d1
 dot1x timer client-timeout 30
#
interface Vlanif10
 ip address 192.168.1.10 255.255.255.0
#
interface Vlanif20
 ip address 192.168.2.10 255.255.255.0
#
interface 10GE1/0/1
 port link-type trunk
 port trunk allow-pass vlan 10
#
interface 10GE1/0/2
 port link-type access
 port default vlan 20
 authentication-profile p1
#
return 
In Figure 3-122, terminals in a company's office area are connected to the company's intranet through DeviceA. The downlink interface (for example 10GE 1/0/2) of DeviceA is directly connected to terminals in the office area.
To meet the company's high security requirements, 802.1X authentication and local authentication need to be configured to authenticate terminals in the office area. Additionally, authentication points need to be deployed on DeviceA's interfaces (for example, 10GE 1/0/2) that are directly connected to the terminals.
# Configure the service scheme s1. In the service scheme s1, set the maximum number of users who are allowed to access the network using the same user name to 15.
[DeviceA-aaa] service-scheme s1
[DeviceA-aaa-service-s1] access-limit user-name max-num 15
[DeviceA-aaa-service-s1] quit
# Configure the domain example.com, and apply the authentication scheme a1, authorization scheme b1, and service scheme s1 to the domain.
[DeviceA-aaa] domain example.com
[DeviceA-aaa-domain-example.com] authentication-scheme a1
[DeviceA-aaa-domain-example.com] authorization-scheme b1
[DeviceA-aaa-domain-example.com] service-scheme s1
[DeviceA-aaa-domain-example.com] quit
[DeviceA-aaa] quit
[DeviceA] dot1x-access-profile name d1
[DeviceA-dot1x-access-profile-d1] dot1x authentication-method pap
[DeviceA-dot1x-access-profile-d1] dot1x timer client-timeout 30
[DeviceA-dot1x-access-profile-d1] quit
#
sysname DeviceA
#
authentication-profile name p1
 dot1x-access-profile d1
 access-domain example.com force
#
vlan batch 10 20
#
aaa
 authentication-scheme a1    
  authentication-mode local
 authorization-scheme b1
  authorization-mode local
 service-scheme s1
  access-limit user-name max-num 15
 domain example.com            
  authentication-scheme a1
  authorization-scheme b1
  service-scheme s1
 local-access-user 00e0-fcd4-8828
  password cipher %+%##!!!!!!!!!"!!!!"!!!!*!!!!SKvr${[Fs.<FvBB,.w;M75IN5Z>'!L8G:n-!!!!!2jp5!!!!!!<!!!!k9&fPO<BSRW}jPT(,ewKyfIL"zVtM1~=>e.!!!!!%+%# 
  service-type dot1x 
#  
dot1x-access-profile name d1
 dot1x authentication-method pap
 dot1x timer client-timeout 30
#
interface Vlanif10
 ip address 192.168.1.10 255.255.255.0
#
interface Vlanif20
 ip address 192.168.2.10 255.255.255.0
#
interface 10GE1/0/1
 port link-type trunk
 port trunk allow-pass vlan 10
#
interface 10GE1/0/2
 port link-type access
 port default vlan 20
 authentication-profile p1
#
return 
On the enterprise network shown in Figure 3-123, DeviceA functions as the access device, and two RADIUS servers are deployed for 802.1X and RADIUS authentication of users on the enterprise network. Users can access the Internet only after being successfully authenticated. The administrator requires that users can enter the bypass state and access the Internet when the two RADIUS servers are faulty and that users can be re-authenticated and re-authorized by the RADIUS servers after the RADIUS servers recover.
The RADIUS authentication and accounting keys configured on the RADIUS server must be the same as the RADIUS server's shared key configured on the device.
[DeviceA] radius-server dead-interval 7
[DeviceA] radius-server dead-count 1
Run the test-aaa command four times. The RADIUS server then goes down.
<HUAWEI> system-view
[HUAWEI] sysname DeviceA
[DeviceA] vlan batch 10 20
[DeviceA] interface 10ge 1/0/1
[DeviceA-10GE1/0/1] portswitch
[DeviceA-10GE1/0/1] port link-type trunk
[DeviceA-10GE1/0/1] port trunk allow-pass vlan 10
[DeviceA-10GE1/0/1] quit
[DeviceA] interface vlanif 10
[DeviceA-Vlanif10] ip address 192.168.1.10 24
[DeviceA-Vlanif10] quit
[DeviceA] interface 10ge 1/0/2
[DeviceA-10GE1/0/2] portswitch
[DeviceA-10GE1/0/2] port link-type hybrid
[DeviceA-10GE1/0/2] port hybrid pvid vlan 20
[DeviceA-10GE1/0/2] port hybrid untagged vlan 20
[DeviceA-10GE1/0/2] quit
[DeviceA] interface vlanif 20
[DeviceA-Vlanif20] ip address 192.168.2.10 24
[DeviceA-Vlanif20] quit
# Configure a route destined for RADIUS servers. In this example, the next-hop IP address is 192.168.1.1.
[DeviceA] ip route-static 10.7.66.0 255.255.255.0 192.168.1.1
# Create the RADIUS server template controller.
[DeviceA] radius-server template controller
# Configure IP addresses and port numbers of the active and standby RADIUS authentication and accounting servers, set the algorithm for selecting RADIUS servers to master-backup, and set the RADIUS authentication key.
[DeviceA-radius-controller] radius-server authentication 10.7.66.66 1812 weight 80
[DeviceA-radius-controller] radius-server accounting 10.7.66.66 1813 weight 80
[DeviceA-radius-controller] radius-server authentication 10.7.66.67 1812 weight 40
[DeviceA-radius-controller] radius-server accounting 10.7.66.67 1813 weight 40
[DeviceA-radius-controller] radius-server algorithm master-backup
[DeviceA-radius-controller] radius-server shared-key cipher Huawei@123456789
# Configure automatic detection.
[DeviceA-radius-controller] radius-server testuser username test1 password cipher abc@123
# Configure the automatic detection interval and detection packet timeout period for RADIUS servers in down state. (This example uses the default values.)
[DeviceA-radius-controller] radius-server detect-server interval 60
[DeviceA-radius-controller] radius-server detect-server timeout 3
# Configure the number of retransmissions and timeout interval for RADIUS authentication requests. (This example uses the default values.)
[DeviceA-radius-controller] radius-server retransmit 3 timeout 5
[DeviceA-radius-controller] quit
# Configure the conditions for setting the RADIUS server to the down state. (This example uses the default values.)
[DeviceA] radius-server dead-interval 5
[DeviceA] radius-server dead-count 2
[DeviceA] radius-server detect-cycle 2
[DeviceA] radius-server max-unresponsive-interval 300
# Configure the authentication scheme auth and set the authentication mode to RADIUS authentication.
[DeviceA] aaa
[DeviceA-aaa] authentication-scheme auth
[DeviceA-aaa-authen-auth] authentication-mode radius
[DeviceA-aaa-authen-auth] quit
# Configure the accounting scheme acc and set the accounting mode to RADIUS accounting.
[DeviceA-aaa] accounting-scheme acc
[DeviceA-aaa-accounting-acc] accounting-mode radius
[DeviceA-aaa-accounting-acc] quit
# Configure the domain example, and apply the authentication scheme auth, accounting scheme acc, and RADIUS server template controller to the domain.
[DeviceA-aaa] domain example
[DeviceA-aaa-domain-example] authentication-scheme auth
[DeviceA-aaa-domain-example] accounting-scheme acc
[DeviceA-aaa-domain-example] radius-server controller
[DeviceA-aaa-domain-example] quit
[DeviceA-aaa] quit
# Bind the 802.1X access profile d1 to the authentication profile, and configure the forcible authentication domain example for users using the authentication profile.
After a forcible domain is configured in an authentication profile, users using this authentication profile are authenticated in the domain regardless of whether the user names carry domain names or carry what kind of domain names.
[DeviceA] authentication-profile name p1
[DeviceA-authen-profile-p1] dot1x-access-profile d1
[DeviceA-authen-profile-p1] access-domain example force
[DeviceA-authen-profile-p1] quit
# Configure the rights granted to users when RADIUS servers are faulty, and enable the re-authentication function when the RADIUS servers recover. In this example, service scheme-based authorization is used during user authentication bypass. For other authorization modes, see (Optional) Configuring the Bypass Function.
[DeviceA] acl 3001
[DeviceA-acl-adv-3001] rule 1 permit ip source 192.168.2.0 0.0.0.255
[DeviceA-acl-adv-3001] quit
[DeviceA] aaa
[DeviceA-aaa] service-scheme s1
[DeviceA-aaa-service-s1] acl-id 3001
[DeviceA-aaa-service-s1] quit
[DeviceA-aaa] quit
[DeviceA] authentication-profile name p1
[DeviceA-authen-profile-p1] authentication event authen-server-down action authorize service-scheme s1
[DeviceA-authen-profile-p1] authentication event authen-server-up action re-authen
[DeviceA-authen-profile-p1] quit
# Disable the pre-connection function.
[DeviceA] undo authentication pre-authen-access enable
# Bind the authentication profile p1 to 10GE 1/0/2 and enable 802.1X authentication on 10GE 1/0/2. The configurations of other interfaces connected to terminals are similar.
#
sysname DeviceA
#
undo authentication pre-authen-access enable 
authentication-profile name p1
 dot1x-access-profile d1
 access-domain example force
 authentication event authen-server-down action authorize service-scheme s1
 authentication event authen-server-up action re-authen
#
vlan batch 10 20
#
acl 3001 
 rule 1 permit ip source 192.168.2.0 0.0.0.255
#
aaa
 authentication-scheme auth    
  authentication-mode radius 
 accounting-scheme acc
  accounting-mode radius   
 service-scheme s1 
  acl-id 3001
 domain example            
  authentication-scheme auth
  accounting-scheme acc
  radius-server controller        
# 
radius-server template controller
 radius-server shared-key cipher %+%##!!!!!!!!!"!!!!"!!!!*!!!!SKvr${[Fs.3t@/5k|BENhEu>W(3\~XG!!D;!!!!!2jp5!!!!!!A!!!!3"pK8qv!}9M#(4$jGWvQF/R[CNe/+:W^jk8HUe&W%+%#
 radius-server accounting 10.7.66.66 1813 weight 80   
 radius-server accounting 10.7.66.67 1813 weight 40 
 radius-server authentication 10.7.66.66 1812 weight 80 
 radius-server authentication 10.7.66.67 1812 weight 40   
 radius-server testuser username test1 password cipher %+%##!!!!!!!!!"!!!!"!!!!*!!!!SKvr${[Fs.3t@/5k|BENhEu>W(3\~XG!!D;!!!!!2jp5!!!!!!A!!!!3"pK8qv!}9M#(4$jGWvQF/R[CNe/+:W^jk8HUe&W%+%#
#
dot1x-access-profile name d1
 dot1x timer client-timeout 30 
#
interface Vlanif10
 ip address 192.168.1.10 255.255.255.0
#
interface Vlanif20
 ip address 192.168.2.10 255.255.255.0
#
interface 10GE1/0/1
 port link-type trunk
 port trunk allow-pass vlan 10
#
interface 10GE1/0/2
 port link-type hybrid
 port hybrid pvid vlan 20
 port hybrid untagged vlan 20
 authentication-profile p1
#
ip route-static 10.7.66.0 255.255.255.0 192.168.1.1
#
return 
In Figure 3-124, DeviceA functioning as a WAC in an enterprise is directly connected to an AP. The enterprise needs to deploy the WLAN named wlan-net to provide wireless network access for employees. The WAC functions as a DHCP server to assign IP addresses on the network segment 10.23.101.0/24 to STAs.
To reduce network security risks, you can deploy Portal authentication on the WAC. The WAC works with the RADIUS server (integrated with the Portal server) to implement access control on STAs that attempt to access the enterprise network, meeting the enterprise's security requirements.
| Item | Data | 
|---|---|
| RADIUS authentication parameters |  | 
| Portal server template |  | 
| Portal access profile |  | 
| Authentication-free rule profile |  | 
| Authentication domain | The authentication domain is named example.com and bound to:  | 
| Authentication profile |  | 
| DHCP server | The WAC functions as a DHCP server to assign IP addresses to the AP and STAs. | 
| IP address pool for the AP | 10.23.100.2 to 10.23.100.254/24 | 
| IP address pool for STAs | 10.23.101.2 to 10.23.101.254/24 | 
| IP address of the WAC's source interface | VLANIF 100: 10.23.100.1/24 | 
| AP group |  | 
| Regulatory domain profile |  | 
| SSID profile |  | 
| Security profile |  | 
| VAP profile |  | 
# Configure a URL template, configure the redirect URL for the Portal server, and specify parameters carried in the URL as the user IP address and user MAC address.
[DeviceA] url-template name url1 
[DeviceA-url-template-url1] url http://10.23.200.1:19008/portal 
[DeviceA-url-template-url1] url-parameter set device-ip 10.23.100.1
[DeviceA-url-template-url1] url-parameter device-ip ac-ip user-ipaddress uaddress user-mac umac 
[DeviceA-url-template-url1] quit
# Configure a Portal server template and bind it to the URL template.
[DeviceA] web-auth-server server-source all-interface
[DeviceA] web-auth-server abc
[DeviceA-web-auth-server-abc] server-ip 10.23.200.1
[DeviceA-web-auth-server-abc] shared-key cipher YsHsjx_202206
[DeviceA-web-auth-server-abc] port 50200
[DeviceA-web-auth-server-abc] url-template url1
[DeviceA-web-auth-server-abc] quit
[DeviceA] portal-access-profile name portal1
[DeviceA-portal-access-profile-portal1] web-auth-server abc
[DeviceA-portal-access-profile-portal1] quit
[DeviceA] free-rule-template name default_free_rule
[DeviceA-free-rule-default_free_rule] free-rule 1 destination ip 10.23.200.2 mask 24
[DeviceA-free-rule-default_free_rule] quit
[DeviceA] authentication-profile name p1
[DeviceA-authentication-profile-p1] portal-access-profile portal1
[DeviceA-authentication-profile-p1] free-rule-template default_free_rule
[DeviceA-authentication-profile-p1] access-domain example.com force
[DeviceA-authentication-profile-p1] quit
#
sysname DeviceA
#
vlan batch 100 to 101
#
authentication-profile name p1
 portal-access-profile portal1
 free-rule-template default_free_rule
 access-domain example.com force
#
dhcp enable
#
radius-server template radius_huawei
 radius-server shared-key cipher %^%#Oc6_BMCw#9gZ2@SMVtk!PAC6>Ou*eLW/"qLp+f#$%^%#
 radius-server authentication 10.23.200.1 1812 weight 80
 radius-server accounting 10.23.200.1 1813 weight 80
#
free-rule-template name default_free_rule
 free-rule 1 destination ip 10.23.200.2 mask 255.255.255.0
# 
url-template name url1  
 url http://10.23.200.1:19008/portal  
 url-parameter set device-ip 10.23.100.1 
 url-parameter device-ip ac-ip user-ipaddress uaddress user-mac umac  
#
web-auth-server server-source all-interface
web-auth-server abc
 server-ip 10.23.200.1
 port 50200
 shared-key cipher %^%#4~ZXE3]6@BXu;2;aw}hA{rSb,@"L@T#e{%6G1AiD%^%#
 url-template url1
#
portal-access-profile name portal1
 web-auth-server abc
#
aaa
 authentication-scheme scheme1
  authentication-mode radius
 accounting-scheme scheme2
  accounting-mode radius
  accounting realtime 15
 domain example.com
  authentication-scheme scheme1
  accounting-scheme scheme2
  radius-server radius_huawei
#
interface Vlanif100
 ip address 10.23.100.1 255.255.255.0
 dhcp select interface
#
interface Vlanif101
 ip address 10.23.101.1 255.255.255.0
 dhcp select interface 
#
interface 10GE1/0/1
 port link-type trunk
 port trunk pvid vlan 100
 port trunk allow-pass vlan 100
#
interface 10GE1/0/2
 port link-type trunk
 port trunk allow-pass vlan 101
#
ip route-static 10.23.200.0 255.255.255.0 10.23.101.2
#  
capwap source interface vlanif 100
capwap dtls psk %+%##!!!!!!!!!"!!!!"!!!!*!!!!a4O4$.&PxRTGtHR(VaB4*(KhWT"C@K+HZ%@!!!!!2jp5!!!!!!;!!!!..AjEF}u<=!%e=F%Dz-Y[R--3yrC-*MVXf<!!!!!%+%#
#
wlan
 security-profile name wlan-security
  security open
 ssid-profile name wlan-ssid
  ssid wlan-net
 vap-profile name wlan-vap
  forward-mode tunnel
  service-vlan vlan-id 101
  ssid-profile wlan-ssid
  security-profile wlan-security
  authentication-profile p1
 regulatory-domain-profile name domain1
 ap-group name ap-group1
  regulatory-domain-profile domain1
  radio 0
   vap-profile wlan-vap wlan 1
  radio 1
   vap-profile wlan-vap wlan 1
 ap-id 0 type-id 1 ap-mac 00e0-fc12-3456 ap-sn 210235554710CB000042
  ap-name area_1
  ap-group ap-group1
  radio 0
   channel 20mhz 6
   eirp 127
   calibrate auto-channel-select disable  
   calibrate auto-txpower-select disable
  radio 1
   channel 80mhz 149
   eirp 127
   calibrate auto-channel-select disable 
   calibrate auto-txpower-select disable
#
return
In Figure 3-125, DeviceA functioning as a WAC in an enterprise is directly connected to an AP. The enterprise needs to deploy the WLAN named wlan-net to provide wireless network access for employees. The WAC functions as a DHCP server to assign IP addresses on the network segment 10.23.101.0/24 to STAs.
| Item | Data | 
|---|---|
| RADIUS authentication parameters |  | 
| SSL policy |  | 
| URL template |  | 
| Portal server template |  | 
| Portal access profile |  | 
| Authentication-free rule profile |  | 
| Authentication domain | The authentication domain is named example.com and is bound to:  | 
| Authentication profile |  | 
| DHCP server | The WAC functions as a DHCP server to assign IP addresses to the AP and STAs. | 
| IP address pool for the AP | 10.23.100.2 to 10.23.100.254/24 | 
| IP address pool for STAs | 10.23.101.2 to 10.23.101.254/24 | 
| IP address of the WAC's source interface | VLANIF 100: 10.23.100.1/24 | 
| AP group |  | 
| Regulatory domain profile |  | 
| SSID profile |  | 
| Security profile |  | 
| VAP profile |  | 
[DeviceA] url-template name url1
[DeviceA-url-template-url1] url https://10.23.200.1:8445/portal
[DeviceA-url-template-url1] url-parameter login-url wac_url https://10.23.100.1:8443
[DeviceA-url-template-url1] quit
The login-url parameter is used by terminals to send account information to the device. If a Portal server does not support the login URL setting, you need to run the url-parameter command with the login-url parameter specified. The IP address in the login-url parameter is the WAC's local IP address, which needs to be permitted in the authentication-free rule profile to ensure that there is a reachable route between the terminal and the IP address. The port number in the parameter is specified using the portal web-authen-server https command, and the default port number is 8443.
[DeviceA] pki realm abcd 
[DeviceA-pki-abcd] quit 
[DeviceA] pki import-certificate ca realm abcd pem filename test.cer
[DeviceA] ssl policy huawei
[DeviceA-ssl-policy-huawei] pki-domain abcd
[DeviceA-ssl-policy-huawei] quit
[DeviceA] portal web-authen-server https ssl-policy huawei
[DeviceA] portal web-authen-server server-source all-interface  
[DeviceA] web-auth-server abc
[DeviceA-web-auth-server-abc] protocol http
[DeviceA-web-auth-server-abc] quit
Ensure that the Portal server IP address, URL, and Portal authentication port number in the URL are configured correctly and are the same as those on the Portal server.
[DeviceA] web-auth-server abc
[DeviceA-web-auth-server-abc] server-ip 10.23.200.1
[DeviceA-web-auth-server-abc] url-template url1
[DeviceA-web-auth-server-abc] quit
[DeviceA] free-rule-template name default_free_rule
[DeviceA-free-rule-default_free_rule] free-rule 1 destination ip 10.23.200.2 mask 24
[DeviceA-free-rule-default_free_rule] free-rule 2 destination ip 10.23.100.1 mask 24
[DeviceA-free-rule-default_free_rule] quit
WAC
#
pki realm abcd 
#
 sysname AC
#    
vlan batch 100 to 101
#
authentication-profile name p1
 portal-access-profile portal1
 free-rule-template default_free_rule
 access-domain example.com force
#
portal web-authen-server https ssl-policy huawei 
portal web-authen-server server-source all-interface
#   
dhcp enable
#
radius-server template radius_huawei
 radius-server shared-key cipher %^%#Oc6_BMCw#9gZ2@SMVtk!PAC6>Ou*eLW/"qLp+f#$%^%#
 radius-server authentication 10.23.200.1 1812 weight 80
 radius-server accounting 10.23.200.1 1813 weight 80
#
ssl policy huawei
 pki-domain abcd
#
free-rule-template name default_free_rule
 free-rule 1 destination ip 10.23.200.2 mask 255.255.255.0 
 free-rule 2 destination ip 10.23.100.1 mask 255.255.255.0
# 
url-template name url1
 url https://10.23.200.1:8445/portal
 url-parameter login-url wac_url https://10.23.100.1:8443
# 
web-auth-server abc
 server-ip 10.23.200.1
 url-template url1
 protocol http
#
portal-access-profile name portal1
 web-auth-server abc
#
aaa
 authentication-scheme scheme1
  authentication-mode radius
 accounting-scheme scheme2
  accounting-mode radius
  accounting realtime 15
 domain example.com
  authentication-scheme scheme1
  accounting-scheme scheme2
  radius-server radius_huawei
#
interface Vlanif100
 ip address 10.23.100.1 255.255.255.0
 dhcp select interface
#
interface Vlanif101
 ip address 10.23.101.1 255.255.255.0
 dhcp select interface 
#
interface 10GE1/0/1
 port link-type trunk
 port trunk pvid vlan 100
 port trunk allow-pass vlan 100
#
interface 10GE1/0/2
 port link-type trunk
 port trunk allow-pass vlan 101
#
ip route-static 10.23.200.0 255.255.255.0 10.23.101.2
#  
capwap source interface vlanif 100
#
wlan
 security-profile name wlan-security
  security open
 ssid-profile name wlan-ssid
  ssid wlan-net
 vap-profile name wlan-vap
  forward-mode tunnel
  service-vlan vlan-id 101
  ssid-profile wlan-ssid
  security-profile wlan-security
  authentication-profile p1
 regulatory-domain-profile name domain1
 ap-group name ap-group1
  regulatory-domain-profile domain1
  radio 0
   vap-profile wlan-vap wlan 1
  radio 1
   vap-profile wlan-vap wlan 1
 ap-id 0 type-id 1 ap-mac 00e0-fc12-3456 ap-sn 210235554710CB000042
  ap-name area_1
  ap-group ap-group1
#
return
In Figure 3-126, DeviceA functioning as a WAC in an enterprise is directly connected to an AP. The enterprise needs to deploy the WLAN named wlan-net to provide wireless network access for employees. The WAC functions as a DHCP server to assign IP addresses on the network segment 10.23.101.0/24 to STAs.
HTTPS is recommended, as it is more secure than HTTP. For related examples, see Example for Configuring Wireless Portal Authentication (Using the HTTPS Protocol).
| Item | Data | 
|---|---|
| RADIUS authentication parameters |  | 
| URL template |  | 
| Portal server template |  | 
| Portal access profile |  | 
| Authentication-free rule profile |  | 
| Authentication domain | The authentication domain is named example.com and is bound to:  | 
| Authentication profile |  | 
| DHCP server | The WAC functions as a DHCP server to assign IP addresses to the AP and STAs. | 
| IP address pool for the AP | 10.23.100.2/24 to 10.23.100.254/24 | 
| IP address pool for STAs | 10.23.101.2/24 to 10.23.101.254/24 | 
| IP address of the WAC's source interface | VLANIF 100: 10.23.100.1/24 | 
| AP group |  | 
| Regulatory domain profile |  | 
| SSID profile |  | 
| Security profile |  | 
| VAP profile |  | 
[DeviceA] url-template name url1
[DeviceA-url-template-url1] url http://10.23.200.1:8080/portal
[DeviceA-url-template-url1] url-parameter login-url ac_url http://10.23.100.1:8000
[DeviceA-url-template-url1] quit
The login-url parameter is used by terminals to send account information to the device. Some Portal servers do not support the login-url parameter setting. Therefore, you need to configure the URL to carry the login-url parameter so that the URL can carry the login-url parameter to the Portal server when a terminal accesses the Portal page. The IP address in the login-url parameter is the WAC's local IP address, which needs to be permitted in the authentication-free rule profile to ensure that there is a reachable route between the terminal and the IP address. The port number in the parameter is specified using the portal web-authen-server http command, and the default port number is 8000.
[DeviceA] portal web-authen-server http  
[DeviceA] portal web-authen-server server-source all-interface
[DeviceA] web-auth-server abc
[DeviceA-web-auth-server-abc] protocol http
[DeviceA-web-auth-server-abc] quit
#
 sysname DeviceA
#    
vlan batch 100 to 101
#
authentication-profile name p1
 portal-access-profile portal1
 free-rule-template default_free_rule
 access-domain example.com force
#
portal web-authen-server http   
portal web-authen-server server-source all-interface                
#   
dhcp enable
#
radius-server template radius_huawei
 radius-server shared-key cipher %^%#Oc6_BMCw#9gZ2@SMVtk!PAC6>Ou*eLW/"qLp+f#$%^%#
 radius-server authentication 10.23.200.1 1812 weight 80
 radius-server accounting 10.23.200.1 1813 weight 80                                                              
#
free-rule-template name default_free_rule                                                                                           
 free-rule 1 destination ip 10.23.200.2 mask 255.255.255.0
 free-rule 2 destination ip 10.23.100.1 mask 255.255.255.0                                                               
#
url-template name url1
 url http://10.23.200.1:8080/portal
 url-parameter login-url ac_url http://10.23.100.1:8000
# 
web-auth-server abc
 server-ip 10.23.200.1
 url-template url1
 protocol http
#
portal-access-profile name portal1
 web-auth-server abc
#
aaa
 authentication-scheme scheme1
  authentication-mode radius
 accounting-scheme scheme2
  accounting-mode radius
  accounting realtime 15
 domain example.com
  authentication-scheme scheme1
  accounting-scheme scheme2
  radius-server radius_huawei
#
interface Vlanif100
 ip address 10.23.100.1 255.255.255.0
 dhcp select interface
#
interface Vlanif101
 ip address 10.23.101.1 255.255.255.0
 dhcp select interface 
#
interface 10GE1/0/1
 port link-type trunk
 port trunk pvid vlan 100
 port trunk allow-pass vlan 100
#
interface 10GE1/0/2
 port link-type trunk
 port trunk allow-pass vlan 101
#
ip route-static 10.23.200.0 255.255.255.0 10.23.101.2
#  
capwap source interface vlanif 100
#
wlan
 security-profile name wlan-security
  security open
 ssid-profile name wlan-ssid
  ssid wlan-net
 vap-profile name wlan-vap
  forward-mode tunnel
  service-vlan vlan-id 101
  ssid-profile wlan-ssid
  security-profile wlan-security
  authentication-profile p1
 regulatory-domain-profile name domain1
 ap-group name ap-group1
  regulatory-domain-profile domain1
  radio 0
   vap-profile wlan-vap wlan 1
  radio 1
   vap-profile wlan-vap wlan 1
 ap-id 0 type-id 1 ap-mac 00e0-fc12-3456 ap-sn 210235554710CB000042
  ap-name area_1
  ap-group ap-group1
#
return
In Figure 3-127, DeviceA of an enterprise functions as a WAC and is directly connected to an AP. The enterprise deploys the WLAN named wlan-net to provide wireless network access for employees. The WAC functions as a DHCP server to assign IP addresses on the network segment 10.23.101.0/24 to STAs.
To reduce network security risks, you can deploy Portal authentication on the WAC. The WAC works with the RADIUS server (integrated with the Portal server) to implement access control on STAs who attempt to access the enterprise network, meeting the enterprise's security requirements.
Portal authentication allows you to specify parameters carried in a redirect URL so that the Portal server can obtain user terminal information based on the parameters in the URL and provide different web authentication pages for different users based on the user terminal information.
[DeviceA] url-template name url1 
[DeviceA-url-template-url1] url http://10.23.200.1:19008/portal 
[DeviceA-url-template-url1] url-parameter set device-ip 10.23.101.1
[DeviceA-url-template-url1] url-parameter device-ip ac-ip user-ipaddress uaddress user-mac umac
[DeviceA-url-template-url1] quit
#
sysname DeviceA
#
vlan batch 100 to 101
#
authentication-profile name p1
 portal-access-profile portal1
 free-rule-template default_free_rule
 access-domain example.com force
#
dhcp enable
#
radius-server template radius_huawei
 radius-server shared-key cipher %^%#Oc6_BMCw#9gZ2@SMVtk!PAC6>Ou*eLW/"qLp+f#$%^%#
 radius-server authentication 10.23.200.1 1812 weight 80
 radius-server accounting 10.23.200.1 1813 weight 80
#
free-rule-template name default_free_rule
 free-rule 1 destination ip 10.23.200.2 mask 255.255.255.0
# 
url-template name url1
 url https://10.23.200.1:19008/portal
 url-parameter set device-ip 10.23.101.1
 url-parameter device-ip ac-ip user-ipaddress uaddress user-mac umac
#
web-auth-server server-source all-interface
web-auth-server abc
 server-ip 10.23.200.1
 port 50200
 shared-key cipher %^%#4~ZXE3]6@BXu;2;aw}hA{rSb,@"L@T#e{%6G1AiD%^%#
 url-template url1
#
portal-access-profile name portal1
 web-auth-server abc
#
aaa
 authentication-scheme scheme1
  authentication-mode radius
 accounting-scheme scheme2
  accounting-mode radius
  accounting realtime 15
 domain example.com
  authentication-scheme scheme1
  accounting-scheme scheme2
  radius-server radius_huawei
#
interface Vlanif100
 ip address 10.23.100.1 255.255.255.0
 dhcp select interface
#
interface Vlanif101
 ip address 10.23.101.1 255.255.255.0
 dhcp select interface 
#
interface 10GE1/0/1
 port link-type trunk
 port trunk pvid vlan 100
 port trunk allow-pass vlan 100
#
interface 10GE1/0/2
 port link-type trunk
 port trunk allow-pass vlan 101
#
ip route-static 10.23.200.0 255.255.255.0 10.23.101.2
#  
capwap source interface vlanif 100
capwap dtls psk %+%##!!!!!!!!!"!!!!"!!!!*!!!!a4O4$.&PxRTGtHR(VaB4*(KhWT"C@K+HZ%@!!!!!2jp5!!!!!!;!!!!..AjEF}u<=!%e=F%Dz-Y[R--3yrC-*MVXf<!!!!!%+%#
#
wlan
 security-profile name wlan-security
  security open
 ssid-profile name wlan-ssid
  ssid wlan-net
 vap-profile name wlan-vap
  forward-mode tunnel
  service-vlan vlan-id 101
  ssid-profile wlan-ssid
  security-profile wlan-security
  authentication-profile p1
 regulatory-domain-profile name domain1
 ap-group name ap-group1
  regulatory-domain-profile domain1
  radio 0
   vap-profile wlan-vap wlan 1
  radio 1
   vap-profile wlan-vap wlan 1
 ap-id 0 type-id 1 ap-mac 00e0-fc12-3456 ap-sn 210235554710CB000042
  ap-name area_1
  ap-group ap-group1
  radio 0
   channel 20mhz 6
   eirp 127
   calibrate auto-channel-select disable   
   calibrate auto-txpower-select disable
  radio 1
   channel 80mhz 149
   eirp 127
   calibrate auto-channel-select disable  
   calibrate auto-txpower-select disable
#
return
On the network shown in Figure 3-128, users in a company's guest area access the company's intranet through DeviceA. Unauthorized access to the intranet will damage the company's service system and cause leakage of key information. Therefore, the administrator requires that DeviceA should control users' network access rights to ensure intranet security. Because guest users move frequently, Portal authentication is used and the RADIUS server authenticates the users.
[DeviceA] web-auth-server server-source all-interface
[DeviceA] web-auth-server abc
[DeviceA-web-auth-server-abc] server-ip 192.168.2.30
[DeviceA-web-auth-server-abc] shared-key cipher YsHsjx_202206
[DeviceA-web-auth-server-abc] port 50200
[DeviceA-web-auth-server-abc] url http://192.168.2.30:8080/portal
[DeviceA-web-auth-server-abc] quit
[DeviceA] authentication-profile name p1
[DeviceA-authentication-profile-p1] portal-access-profile portal1
[DeviceA-authentication-profile-p1] access-domain example.com force
[DeviceA-authentication-profile-p1] quit
#
sysname DeviceA
#
vlan batch 10 20
#
aaa
 authentication-scheme abc    
  authentication-mode radius    
 accounting-scheme scheme2
  accounting-mode radius
  accounting realtime 15
 domain example.com            
  authentication-scheme abc     
  accounting-scheme scheme2
  radius-server rd1      
#  
radius-server template rd1 
 radius-server shared-key cipher %+%##!!!!!!!!!"!!!!"!!!!*!!!!SKvr${[Fs.3t@/5k|BENhEu>W(3\~XG!!D;!!!!!2jp5!!!!!!A!!!!3"pK8qv!}9M#(4$jGWvQF/R[CNe/+:W^jk8HUe&W%+%#
 radius-server authentication 192.168.2.30 1812 weight 80
 radius-server accounting 192.168.2.30 1813 weight 80
#
web-auth-server server-source all-interface
web-auth-server abc
 server-ip 192.168.2.30
 port 50200
 shared-key cipher %^%#4~ZXE3]6@BXu;2;aw}hA{rSb,@"L@T#e{%6G1AiD%^%#
 url http://192.168.2.30:8445/portal
#
portal-access-profile name portal1
 web-auth-server abc
#
authentication-profile name p1
 portal-access-profile portal1
 access-domain example.com force
#
interface Vlanif10
 ip address 192.168.1.10 255.255.255.0
#
interface Vlanif20
 ip address 192.168.2.10 255.255.255.0
#
interface 10GE1/0/1
 port link-type trunk
 port trunk allow-pass vlan 10
 authentication-profile p1
#
interface 10GE1/0/2
 port link-type access
 port default vlan 20
#
return 
[DeviceA] url-template name url1 
[DeviceA-url-template-url1] url http://192.168.2.30:19008/portal 
[DeviceA-url-template-url1] url-parameter set device-ip 192.168.2.10
[DeviceA-url-template-url1] url-parameter device-ip ac-ip user-ipaddress uaddress user-mac umac 
[DeviceA-url-template-url1] quit
[DeviceA] web-auth-server server-source all-interface
[DeviceA] web-auth-server abc
[DeviceA-web-auth-server-abc] server-ip 192.168.2.30
[DeviceA-web-auth-server-abc] shared-key cipher YsHsjx_202206
[DeviceA-web-auth-server-abc] port 50200
[DeviceA-web-auth-server-abc] url-template url1
[DeviceA-web-auth-server-abc] quit
#
sysname DeviceA
#
vlan batch 10 20
#
aaa
 authentication-scheme abc    
  authentication-mode radius    
 accounting-scheme scheme2
  accounting-mode radius
  accounting realtime 15
 domain example.com            
  authentication-scheme abc     
  accounting-scheme scheme2
  radius-server rd1      
#  
radius-server template rd1 
 radius-server shared-key cipher %+%##!!!!!!!!!"!!!!"!!!!*!!!!SKvr${[Fs.3t@/5k|BENhEu>W(3\~XG!!D;!!!!!2jp5!!!!!!A!!!!3"pK8qv!}9M#(4$jGWvQF/R[CNe/+:W^jk8HUe&W%+%#
 radius-server authentication 192.168.2.30 1812 weight 80
 radius-server accounting 192.168.2.30 1813 weight 80
#
url-template name url1 
 url http://192.168.2.30:19008/portal 
 url-parameter set device-ip 192.168.2.10
 url-parameter device-ip ac-ip user-ipaddress uaddress user-mac umac 
#
web-auth-server server-source all-interface
web-auth-server abc
 server-ip 192.168.2.30
 port 50200
 shared-key cipher %^%#4~ZXE3]6@BXu;2;aw}hA{rSb,@"L@T#e{%6G1AiD%^%#
 url-template url1
#
portal-access-profile name portal1
 web-auth-server abc
#
authentication-profile name p1
 portal-access-profile portal1
 access-domain example.com force
#
interface Vlanif10
 ip address 192.168.1.10 255.255.255.0
#
interface Vlanif20
 ip address 192.168.2.10 255.255.255.0
#
interface 10GE1/0/1
 port link-type trunk
 port trunk allow-pass vlan 10
 authentication-profile p1
#
interface 10GE1/0/2
 port link-type access
 port default vlan 20
#
return 
[DeviceA] url-template name url1 
[DeviceA-url-template-url1] url https://192.168.2.30:19008/portal 
[DeviceA-url-template-url1] url-parameter set device-ip 192.168.2.10
[DeviceA-url-template-url1] url-parameter device-ip ac-ip user-ipaddress uaddress user-mac umac
[DeviceA-url-template-url1] quit
[DeviceA] web-auth-server abc
[DeviceA-web-auth-server-abc] server-ip 192.168.2.30
[DeviceA-web-auth-server-abc] protocol http
[DeviceA-web-auth-server-abc] http-method post cmd-key cmd1
[DeviceA-web-auth-server-abc] url-template url1
[DeviceA-web-auth-server-abc] quit
[DeviceA] free-rule-template name default_free_rule
[DeviceA-free-rule-default_free_rule] free-rule 1 destination ip 192.168.1.10 mask 24
[DeviceA-free-rule-default_free_rule] quit
#
pki realm abcd 
#
sysname DeviceA
#
vlan batch 10 20
#
aaa
 authentication-scheme abc    
  authentication-mode radius    
 accounting-scheme scheme2
  accounting-mode radius
  accounting realtime 15
 domain example.com            
  authentication-scheme abc     
  accounting-scheme scheme2
  radius-server rd1      
#  
portal web-authen-server https ssl-policy huawei 
portal web-authen-server server-source all-interface
#   
radius-server template rd1 
 radius-server shared-key cipher %+%##!!!!!!!!!"!!!!"!!!!*!!!!SKvr${[Fs.3t@/5k|BENhEu>W(3\~XG!!D;!!!!!2jp5!!!!!!A!!!!3"pK8qv!}9M#(4$jGWvQF/R[CNe/+:W^jk8HUe&W%+%#
 radius-server authentication 192.168.2.30 1812 weight 80
 radius-server accounting 192.168.2.30 1813 weight 80
#
ssl policy huawei
 pki-domain abcd
#
free-rule-template name default_free_rule
 free-rule 1 destination ip 192.168.1.10 mask 255.255.255.0 
# 
url-template name url1 
 url https://192.168.2.30:19008/portal 
 url-parameter set device-ip 192.168.2.10
 url-parameter device-ip ac-ip user-ipaddress uaddress user-mac umac
#
web-auth-server abc
 server-ip 192.168.2.30
 protocol http  
 http-method post cmd-key cmd1
 url-template url1
#
portal-access-profile name portal1
 web-auth-server abc
#
authentication-profile name p1
 portal-access-profile portal1
 access-domain example.com force
#
interface Vlanif10
 ip address 192.168.1.10 255.255.255.0
#
interface Vlanif20
 ip address 192.168.2.10 255.255.255.0
#
interface 10GE1/0/1
 port link-type trunk
 port trunk allow-pass vlan 10
 authentication-profile p1
#
interface 10GE1/0/2
 port link-type access
 port default vlan 20
#
return 
HTTPS is recommended, as it is more secure than HTTP. For related examples, see Example for Configuring Wired Portal Authentication (Using the HTTPS Protocol).
[DeviceA] portal web-authen-server http
[DeviceA] portal web-authen-server server-source ip-address 192.168.2.10
#
sysname DeviceA
#
vlan batch 10 20
#
aaa
 authentication-scheme abc    
  authentication-mode radius    
 accounting-scheme scheme2
  accounting-mode radius
  accounting realtime 15
 domain example.com            
  authentication-scheme abc     
  accounting-scheme scheme2
  radius-server rd1      
#  
portal web-authen-server http 
portal web-authen-server server-source ip-address 192.168.2.10
#   
radius-server template rd1 
 radius-server shared-key cipher %+%##!!!!!!!!!"!!!!"!!!!*!!!!SKvr${[Fs.3t@/5k|BENhEu>W(3\~XG!!D;!!!!!2jp5!!!!!!A!!!!3"pK8qv!}9M#(4$jGWvQF/R[CNe/+:W^jk8HUe&W%+%#
 radius-server authentication 192.168.2.30 1812 weight 80
 radius-server accounting 192.168.2.30 1813 weight 80
#
free-rule-template name default_free_rule
 free-rule 1 destination ip 192.168.1.10 mask 255.255.255.0 
# 
url-template name url1 
 url http://192.168.2.30:19008/portal 
 url-parameter set device-ip 192.168.2.10
 url-parameter device-ip ac-ip user-ipaddress uaddress user-mac umac 
#
web-auth-server abc
 server-ip 192.168.2.30
 protocol http  
 http-method post cmd-key cmd1
 url-template url1
#
portal-access-profile name portal1
 web-auth-server abc
#
authentication-profile name p1
 portal-access-profile portal1
 access-domain example.com force
#
interface Vlanif10
 ip address 192.168.1.10 255.255.255.0
#
interface Vlanif20
 ip address 192.168.2.10 255.255.255.0
#
interface 10GE1/0/1
 port link-type trunk
 port trunk allow-pass vlan 10
 authentication-profile p1
#
interface 10GE1/0/2
 port link-type access
 port default vlan 20
#
return 
In Figure 3-132, users in a company's guest area access the company's intranet through DeviceA. Unauthorized access to the intranet will damage the company's service system and cause leakage of key information. Therefore, the administrator requires that DeviceA should control users' network access rights to ensure intranet security.
# Create VLANs 10 and 20.
# Configure 10GE 1/0/1 connecting DeviceA to users as an access interface and add the interface to VLAN 10.
[DeviceA] interface 10ge 1/0/1 
[DeviceA-10GE1/0/1] port link-type access
[DeviceA-10GE1/0/1] port default vlan 10 
[DeviceA-10GE1/0/1] quit
[DeviceA] interface vlanif 10
[DeviceA-Vlanif10] ipv6 enable
[DeviceA-Vlanif10] ipv6 address fc00:2::1 112
[DeviceA-Vlanif10] ipv6 nd ra halt disable
[DeviceA-Vlanif10] quit
# Configure 10GE 1/0/2 connecting DeviceA to the RADIUS server as an access interface and add the interface to VLAN 20.
[DeviceA] interface 10ge 1/0/2
[DeviceA-10GE1/0/2] port link-type access
[DeviceA-10GE1/0/2] port default vlan 20 
[DeviceA-10GE1/0/2] quit
[DeviceA] interface vlanif 20
[DeviceA-Vlanif20] ip address 192.168.2.10 24
[DeviceA-Vlanif20] ipv6 enable
[DeviceA-Vlanif20] ipv6 address fc00:3::1 112
[DeviceA-Vlanif20] ipv6 nd ra halt disable
[DeviceA-Vlanif20] quit
[DeviceA] ip route-static 192.168.3.0 255.255.255.0 192.168.2.1
[DeviceA] ipv6 route-static fc00:1:: 112 fc00:3::2
[DeviceA] radius-server template rd1
[DeviceA-radius-rd1] radius-server authentication fc00:1::1 1812
[DeviceA-radius-rd1] radius-server accounting fc00:1::1 1813
[DeviceA-radius-rd1] radius-server shared-key cipher YsHsjx_202206139
[DeviceA-radius-rd1] quit
[DeviceA-aaa] accounting-scheme scheme1 
[DeviceA-aaa-accounting-scheme1] accounting-mode radius 
[DeviceA-aaa-accounting-scheme1] accounting realtime 15 
[DeviceA-aaa-accounting-scheme1] quit 
# Create the authentication domain example.com and bind the AAA authentication scheme, AAA accounting scheme, and RADIUS server template to the authentication domain.
[DeviceA-aaa] domain example.com
[DeviceA-aaa-domain-example.com] authentication-scheme abc
[DeviceA-aaa-domain-example.com] accounting-scheme scheme1
[DeviceA-aaa-domain-example.com] radius-server rd1
[DeviceA-aaa-domain-example.com] quit
[DeviceA-aaa] quit
# Check whether a user can pass RADIUS authentication. (The test user test and password YsHsjx_2022061 have been configured on the RADIUS server.)
[DeviceA] web-auth-server abc
[DeviceA-web-auth-server-abc] server-source ip-address 192.168.2.10 
[DeviceA-web-auth-server-abc] server-ip 192.168.3.30
[DeviceA-web-auth-server-abc] server-ip ipv6 fc00:1::1
[DeviceA-web-auth-server-abc] port 50200
[DeviceA-web-auth-server-abc] url http://[192.168.3.30]:8445/portal
[DeviceA-web-auth-server-abc] url ipv6 http://[FC00:1::1]:8445/portal
[DeviceA-web-auth-server-abc] shared-key cipher YsHsjx_202206
[DeviceA-web-auth-server-abc] quit
Ensure that the port number configured on the device is the same as that used by the Portal server.
[DeviceA] portal-access-profile name web1
[DeviceA-portal-access-profile-web1] web-auth-server abc
[DeviceA-portal-access-profile-web1] quit
# Configure the authentication profile p1, bind the Portal access profile web1 to the authentication profile, specify the domain example.com as the forcible authentication domain in the authentication profile, set the user access mode to multi-authen, and set the maximum number of access users to 100.
[DeviceA] authentication-profile name p1
[DeviceA-authen-profile-p1] portal-access-profile web1
[DeviceA-authen-profile-p1] access-domain example.com force
[DeviceA-authen-profile-p1] authentication mode multi-authen max-user 100
# Bind the authentication profile p1 to 10GE 1/0/1 and enable Portal authentication on 10GE 1/0/1.
#
sysname DeviceA
#
vlan batch 10 20 
#
authentication-profile name p1
 portal-access-profile web1
 access-domain example.com force
 authentication mode multi-authen max-user 100
#
radius-server template rd1
 radius-server authentication fc00:1::1 1812
 radius-server accounting fc00:1::1 1813
 radius-server shared-key cipher %^%#b<4UC_J36%l@*;E]1\s6fJIY85mHu68SrhKtU%"B%^%#
#
web-auth-server abc
 server-source ip-address 192.168.2.10
 server-ip 192.168.3.30
 server-ip ipv6 fc00:1::1
 port 50200
 url http://[192.168.3.30]:8445/portal
 url ipv6 http://[FC00:1::1]:8445/portal
 shared-key cipher %^%#S/:6+:&#J0Sj|H:P2"MOcOh<!DZqX'kF.uSH'~Q'%^%#
#
portal-access-profile name web1
 web-auth-server abc
#
interface 10GE 1/0/1
 port link-type access
 port default vlan 10
 authentication-profile p1
#
interface 10GE 1/0/2
 port link-type access
 port default vlan 20
#
interface vlanif 10
 ipv6 enable
 ip address 192.168.1.10 255.255.255.0
 ipv6 address fc00:2::1 112
 ipv6 nd ra halt disable
#
interface vlanif 20
 ipv6 enable
 ip address 192.168.2.10 255.255.255.0
 ipv6 address fc00:3::1 112
 ipv6 nd ra halt disable
#
aaa
 authentication-scheme abc
  authentication-mode radius
 accounting-scheme scheme1 
  accounting-mode radius 
  accounting realtime 15
 domain example.com
  authentication-scheme abc
  accounting-scheme scheme1
  radius-server rd1
#
ip route-static 192.168.3.0 255.255.255.0 192.168.2.1
ipv6 route-static fc00:1:: 112 fc00:3::2
#
return
On the enterprise network shown in Figure 3-133, DeviceA functions as an access device, and two RADIUS/Portal servers are deployed for Portal and RADIUS authentication of users on the enterprise network. Users can access the Internet only after being successfully authenticated. The administrator has the following user authentication requirements: When the two RADIUS/Portal servers are faulty, users bypass authentication and are granted the same network access rights as they are successfully authenticated. After the RADIUS/Portal servers recover, users are re-authenticated and re-authorized by the RADIUS servers.
The Portal shared key and port number configured on the Portal server must be the same as the shared key configured on the device and the port number used by the device to listen to Portal protocol packets, respectively.
<HUAWEI> system-view
[HUAWEI] sysname DeviceA
[DeviceA] vlan batch 10 20
[DeviceA] interface 10ge 1/0/2
[DeviceA-10GE1/0/2] port link-type access
[DeviceA-10GE1/0/2] port default vlan 20
[DeviceA-10GE1/0/2] quit
[DeviceA] interface vlanif 20
[DeviceA-Vlanif20] ip address 192.168.2.10 24
[DeviceA-Vlanif20] quit
[DeviceA] interface 10ge 1/0/1
[DeviceA-10GE1/0/1] port link-type trunk
[DeviceA-10GE1/0/1] port trunk allow-pass vlan 10
[DeviceA-10GE1/0/1] quit
[DeviceA] interface vlanif 10
[DeviceA-Vlanif10] ip address 192.168.1.10 24
[DeviceA-Vlanif10] quit
# Create the RADIUS server template rad1.
[DeviceA] radius-server template rad1
[DeviceA-radius-rad1] radius-server authentication 10.7.66.66 1812 weight 80
[DeviceA-radius-rad1] radius-server accounting 10.7.66.66 1813 weight 80
[DeviceA-radius-rad1] radius-server authentication 10.7.66.67 1812 weight 40
[DeviceA-radius-rad1] radius-server accounting 10.7.66.67 1813 weight 40
[DeviceA-radius-rad1] radius-server algorithm master-backup
[DeviceA-radius-rad1] radius-server shared-key cipher YsHsjx_202206139
[DeviceA-radius-rad1] radius-server testuser username test1 password cipher YsHsjx_2022061
[DeviceA-radius-rad1] radius-server detect-server interval 60
[DeviceA-radius-rad1] radius-server detect-server timeout 3
[DeviceA-radius-rad1] radius-server retransmit 3 timeout 5
[DeviceA-radius-rad1] quit
# Configure the domain example, and apply the authentication scheme auth, accounting scheme acc, and RADIUS server template rad1 to the domain.
[DeviceA-aaa] domain example
[DeviceA-aaa-domain-example] authentication-scheme auth
[DeviceA-aaa-domain-example] accounting-scheme acc
[DeviceA-aaa-domain-example] radius-server rad1
[DeviceA-aaa-domain-example] quit
[DeviceA-aaa] quit
[DeviceA] web-auth-server server1
[DeviceA-web-auth-server-server1] server-source ip-address 192.168.1.10  
[DeviceA-web-auth-server-server1] server-ip 10.7.66.66
[DeviceA-web-auth-server-server1] port 50200
[DeviceA-web-auth-server-server1] url http://10.7.66.66:8080/portal
[DeviceA-web-auth-server-server1] shared-key cipher YsHsjx_202206
[DeviceA-web-auth-server-server1] server-detect max-times 3 interval 60
[DeviceA-web-auth-server-server1] quit
[DeviceA] web-auth-server server2
[DeviceA-web-auth-server-server1] server-source ip-address 192.168.1.10  
[DeviceA-web-auth-server-server2] server-ip 10.7.66.67
[DeviceA-web-auth-server-server2] port 50200
[DeviceA-web-auth-server-server2] url http://10.7.66.67:8080/portal
[DeviceA-web-auth-server-server2] shared-key cipher YsHsjx_202206
[DeviceA-web-auth-server-server2] server-detect max-times 3 interval 60
[DeviceA-web-auth-server-server2] quit
[DeviceA] portal-access-profile name web1
[DeviceA-portal-access-profile-web1] web-auth-server server1 server2
[DeviceA-portal-access-profile-web1] quit
[DeviceA] acl 3001
[DeviceA-acl-adv-3001] rule 0 permit ip
[DeviceA-acl-adv-3001] quit
[DeviceA] acl 3001
[DeviceA-acl-adv-3001] rule 1 permit ip destination 192.168.10.166 0
[DeviceA-acl-adv-3001] rule 2 permit ip destination 192.168.11.31 0
[DeviceA-acl-adv-3001] rule 3 permit ip destination 192.168.12.26 0
[DeviceA-acl-adv-3001] rule 4 deny ip     //This deny rule must be configured, indicating that it is not allowed to access all network segments except the network segments allowed access by rules 1, 2, and 3.
[DeviceA-acl-adv-3001] quit
# Configure the rights granted to users when RADIUS servers are faulty. In this example, service scheme-based authorization is used during user authentication bypass. For other authorization modes, see (Optional) Configuring the Bypass Function.
[DeviceA] aaa
[DeviceA-aaa] service-scheme s1
[DeviceA-aaa-service-s1] acl-id 3001
[DeviceA-aaa-service-s1] quit
[DeviceA-aaa] quit
[DeviceA] authentication-profile name p1
[DeviceA-authen-profile-p1] authentication event authen-server-down action authorize service-scheme s1
[DeviceA-authen-profile-p1] quit
# Configure the rights granted to users when Portal servers are faulty, and enable the re-authentication function when the Portal servers recover. In this example, service scheme-based authorization is used during user authentication bypass. For other authorization modes, see (Optional) Configuring the Portal Authentication Bypass Function.
[DeviceA] portal-access-profile name web1
[DeviceA-portal-access-profile-web1] authentication event portal-server-down action authorize service-scheme s1
[DeviceA-portal-access-profile-web1] authentication event portal-server-up action re-authen
[DeviceA-portal-access-profile-web1] quit
# Bind the Portal access profile web1 to the authentication profile, and configure the forcible authentication domain example.com for users using this authentication profile.
[DeviceA] authentication-profile name p1
[DeviceA-authen-profile-p1] portal-access-profile web1
[DeviceA-authen-profile-p1] access-domain example.com force
[DeviceA-authen-profile-p1] quit
# Bind the authentication profile p1 to 10GE 1/0/2 and enable Portal authentication on 10GE 1/0/2. The configurations of other downlink interfaces for user access are similar.
#
sysname DeviceA
#
vlan batch 10 20 
#
authentication-profile name p1
 authentication event authen-server-down action authorize service-scheme s1
 portal-access-profile web1
 access-domain example.com force
#
radius-server template rad1
 radius-server authentication 10.7.66.66 1812 weight 80
 radius-server accounting 10.7.66.66 1813 weight 80
 radius-server authentication 10.7.66.67 1812 weight 40
 radius-server accounting 10.7.66.67 1813 weight 40
 radius-server shared-key cipher %^%#b<4UC_J36%l@*;E]1\s6fJIY85mHu68SrhKtU%"B%^%#
 radius-server testuser username test1 password cipher %^%#O80v=d`~o>9:XXAS|o9'wrqD9hY+Q!El"EK:GT{G%^%#
#
web-auth-server server1
 server-source ip-address 192.168.1.10  
 server-ip 10.7.66.66
 port 50200
 url http://10.7.66.66:8080/portal
 shared-key cipher %^%#-;k-</gjVJ=ZK&Ea)<WB(j1FD8HJOGq^@$Ly=\0Y%^%#
 server-detect max-times 3 interval 60
web-auth-server server2
 server-source ip-address 192.168.1.10   
 server-ip 10.7.66.67
 port 50200
 url http://10.7.66.67:8080/portal
 shared-key cipher %^%#S/:6+:&#J0Sj|H:P2"MOcOh<!DZqX'kF.uSH'~Q'%^%#
 server-detect max-times 3 interval 60
#
portal-access-profile name web1
 web-auth-server server1 server2 
 authentication event portal-server-down action authorize service-scheme s1
 authentication event portal-server-up action re-authen
#
interface 10GE 1/0/1
 port link-type trunk
 port trunk allow-pass vlan 10
#
interface 10GE 1/0/2
 port link-type access
 port default vlan 20
 authentication-profile p1
#
interface vlanif 10
 ip address 192.168.1.10 255.255.255.0
#
interface vlanif 20
 ip address 192.168.2.10 255.255.255.0
#
acl 3001
 rule 0 permit ip
#
aaa
 authentication-scheme auth
  authentication-mode radius
 accounting-scheme acc
  accounting-mode radius
 service-scheme s1
  acl-id 3001
 domain example
  authentication-scheme auth
  accounting-scheme acc
  radius-server rad1
#
ip route-static 10.7.66.0 255.255.255.0 192.168.1.1
#
return
On the enterprise network shown in Figure 3-134, DeviceA provides the native AC function and is connected to APs through DeviceB (access switch). The enterprise plans to deploy a wireless network named wlan-net to provide wireless access for employees. DeviceA functions as a DHCP server to assign IP addresses on the network segment 10.23.101.0/24 to STAs.
MAC address-prioritized Portal authentication can be enabled to allow users to connect to the wireless network without entering their user names and passwords when they move in and out of the wireless coverage area repeatedly within a period of time (for example, 60 minutes).
| Item | Data | 
|---|---|
| RADIUS authentication parameters | RADIUS authentication scheme: radius_test RADIUS accounting scheme: scheme1 RADIUS server template: radius_test  | 
| Portal parameters |  | 
| Authentication profile |  | 
| DHCP server | DeviceA functions as a DHCP server to assign IP addresses to STAs and APs.  | 
| IP address of the AC's source interface | VLANIF 100: 10.23.100.1/24 | 
| AP group |  | 
| Regulatory domain profile |  | 
| SSID profile |  | 
| Security profile |  | 
| VAP profile |  | 
| Authentication-free rule profile |  | 
MAC address-prioritized Portal authentication:
<HUAWEI> system-view
[HUAWEI] sysname DeviceB
[DeviceB] vlan batch 100
[DeviceB] interface 10ge 1/0/1
[DeviceB-10GE1/0/1] port link-type trunk
[DeviceB-10GE1/0/1] port trunk pvid vlan 100
[DeviceB-10GE1/0/1] port trunk allow-pass vlan 100
[DeviceB-10GE1/0/1] quit
[DeviceB] 10ge 1/0/2
[DeviceB-10GE1/0/2] port link-type trunk
[DeviceB-10GE1/0/2] port trunk pvid vlan 100
[DeviceB-10GE1/0/2] port trunk allow-pass vlan 100
[DeviceB-10GE1/0/2] quit
[DeviceB] interface 10ge 1/0/3
[DeviceB-10GE1/0/3] port link-type trunk
[DeviceB-10GE1/0/3] port trunk allow-pass vlan 100
[DeviceB-10GE1/0/3] quit
# Configure DeviceA and add 10GE 1/0/1 to VLAN 100 (management VLAN).
In this example, the service data forwarding mode is tunnel forwarding. If direct forwarding is used, you are advised to configure port isolation on GE 0/0/1 that connects the AC to an AP. If port isolation is not configured, unnecessary broadcast packets will be transmitted in VLANs or WLAN users on different APs will be able to communicate with each other at Layer 2.
In tunnel forwarding mode, the management VLAN and service VLAN cannot be the same.
[DeviceA] vlan batch 100 101 200
[DeviceA] interface 10ge 1/0/1
[DeviceA-10GE1/0/1] port link-type trunk
[DeviceA-10GE1/0/1] port trunk allow-pass vlan 100
[DeviceA-10GE1/0/1] quit
# Add the uplink interface 10GE 1/0/2 of DeviceA to VLAN 200 (VLAN for communicating with the RADIUS server).
[DeviceA] interface 10ge 1/0/2
[DeviceA-10GE1/0/2] port link-type trunk
[DeviceA-10GE1/0/2] port trunk allow-pass vlan 200
[DeviceA-10GE1/0/2] quit
# Configure DeviceA as a DHCP server to assign IP addresses to APs from the IP address pool on VLANIF 100, and assign IP addresses to STAs from the IP address pool on VLANIF 101.
[DeviceA] interface vlanif 200
[DeviceA-Vlanif200] ip address 10.23.200.3 24
[DeviceA-Vlanif200] quit
[DeviceA] radius-server template radius_test
[DeviceA-radius-radius_test] radius-server authentication 10.23.200.1 1812
[DeviceA-radius-radius_test] radius-server accounting 10.23.200.1 1813
[DeviceA-radius-radius_test] radius-server shared-key cipher YsHsjx_202206mc@1
[DeviceA-radius-radius_test] quit
[DeviceA] aaa
[DeviceA-aaa] authentication-scheme radius_test
[DeviceA-aaa-authen-radius_test] authentication-mode radius
[DeviceA-aaa-authen-radius_test] quit
[DeviceA-aaa] accounting-scheme scheme1
[DeviceA-aaa-accounting-scheme1] accounting-mode radius
[DeviceA-aaa-accounting-scheme1] accounting realtime 15
[DeviceA-aaa-accounting-scheme1] quit
[DeviceA-aaa] quit
# Configure an authentication domain.
[DeviceA-aaa] domain test
[DeviceA-aaa-domain-test] authentication-scheme radius_test
[DeviceA-aaa-domain-test] accounting-scheme scheme1
[DeviceA-aaa-domain-test] radius-server radius_test
[DeviceA-aaa-domain-test] quit
[DeviceA] web-auth-server abc
[DeviceA-web-auth-server-abc] server-source ip-address 10.23.200.3 
[DeviceA-web-auth-server-abc] server-ip 10.23.200.1
[DeviceA-web-auth-server-abc] port 50200
[DeviceA-web-auth-server-abc] url http://10.23.200.1:8080/portal
[DeviceA-web-auth-server-abc] shared-key cipher YsHsjx_202206
[DeviceA-web-auth-server-abc] quit
[DeviceA] free-rule-template name default_free_rule 
[DeviceA-free-rule-default_free_rule] free-rule 1 destination ip 10.23.202.1 mask 32
[DeviceA-free-rule-default_free_rule] quit
[DeviceA] authentication-profile name p1
[DeviceA-authentication-profile-p1] portal-access-profile web1
[DeviceA-authentication-profile-p1] free-rule-template default_free_rule
[DeviceA-authentication-profile-p1] mac-access-profile m1
[DeviceA-authentication-profile-p1] access-domain test
[DeviceA-authentication-profile-p1] quit
#
sysname DeviceA
#
vlan batch 100 to 101 200
#
authentication-profile name p1
 mac-access-profile m1    
 free-rule-template default_free_rule
 portal-access-profile web1
 access-domain test
# 
dhcp enable
#
radius-server template radius_test
 radius-server shared-key cipher %+%##!!!!!!!!!"!!!!"!!!!*!!!!Cd/`W03KjAwAqn64E<\TxGC_SOri<2BP\A+!!!!!2jp5!!!!!!B!!!!Oe2HMc->XMa#TLDZUaJBFJtm#XVj*E:S*|(N7`J1B:3QY!!!!!!!!!!!!!!!%+%#
 radius-server authentication 10.23.200.1 1812 weight 80
 radius-server accounting 10.23.200.1 1813 weight 80
#
free-rule-template name default_free_rule 
 free-rule 1 destination ip 10.23.202.1 mask 255.255.255.255
#
web-auth-server abc
 server-source ip-address 10.23.200.3
 server-ip 10.23.200.1
 port 50200
 url http://10.23.200.1:8080/portal
 shared-key cipher %^%#-;k-</gjVJ=ZK&Ea)<WB(j1FD8HJOGq^@$Ly=\0Y%^%#
#
portal-access-profile name web1
 web-auth-server abc 
#
aaa
 authentication-scheme radius_test
  authentication-mode radius
 accounting-scheme scheme1
  accounting-mode radius
  accounting realtime 15
 domain test
  authentication-scheme radius_test
  accounting-scheme scheme1
  radius-server radius_test
#
interface Vlanif100
 ip address 10.23.100.1 255.255.255.0
 dhcp select interface
#
interface Vlanif101
 ip address 10.23.101.1 255.255.255.0
 dhcp select interface
#
interface Vlanif200
 ip address 10.23.200.3 255.255.255.0
#
interface 10GE 1/0/1
 port link-type trunk
 port trunk allow-pass vlan 100
#
interface 10GE 1/0/2
 port link-type trunk
 port trunk allow-pass vlan 200
#
capwap source interface vlanif 100
capwap dtls psk %+%##!!!!!!!!!"!!!!"!!!!*!!!!a4O4$.&PxRTGtHR(VaB4*(KhWT"C@K+HZ%@!!!!!2jp5!!!!!!;!!!!..AjEF}u<=!%e=F%Dz-Y[R--3yrC-*MVXf<!!!!!%+%#
#
wlan
 security-profile name wlan-security
  security open
 ssid-profile name wlan-ssid
  ssid wlan-net
 vap-profile name wlan-vap
  forward-mode tunnel
  service-vlan vlan-id 101
  ssid-profile wlan-ssid
  security-profile wlan-security
  authentication-profile p1
 regulatory-domain-profile name domain1
 ap-group name ap-group1
  regulatory-domain-profile domain1
  radio 0
   vap-profile wlan-vap wlan 1
  radio 1
   vap-profile wlan-vap wlan 1
 ap-id 0 ap-mac 00e0-fc76-e360
  ap-name area_1
  ap-group ap-group1
  radio 0
   channel 20mhz 6
   eirp 127
  radio 1
   channel 80mhz 149
   eirp 127
 ap-id 0 type-id 1 ap-mac 00e0-fc12-3456 ap-sn 210235554710CB000042
  ap-name area_2
  ap-group ap-group1
  radio 0
   channel 20mhz 6
   eirp 127
  radio 1
   channel 80mhz 149
   eirp 127
#
mac-access-profile name m1
#
return
#
sysname DeviceB
#
vlan batch 100 
#
interface 10GE 1/0/1
 port link-type trunk
 port trunk pvid vlan 100
 port trunk allow-pass vlan 100
#
interface 10GE 1/0/2
 port link-type trunk
 port trunk pvid vlan 100
 port trunk allow-pass vlan 100
#
interface 10GE 1/0/3
 port link-type trunk
 port trunk allow-pass vlan 100
#
return
On an enterprise network, DeviceA is connected to APs through DeviceB (access switch). The enterprise plans to deploy a wireless network named wlan-net to provide wireless access for employees. To enhance network security, mobile phone users are allowed access to the network only after successful authentication of user names and passwords.
In this example, interfaces 1, 2, and 3 represent 10GE 1/0/1, 10GE 1/0/2, and 10GE 1/0/3, respectively.
| Item | Data | 
|---|---|
| Portal page to be pushed | Default page for user name and password authentication provided by iMaster NCE-Campus. | 
| Portal authentication mode | User name and password authentication:  | 
| Portal authentication-free validity period | Retain the default setting, which is 2 hours. | 
| Item | Data | 
|---|---|
| SSID profile |  | 
| IP address of the AC's source interface | VLANIF 102: 192.168.102.1/24 | 
| Security profile |  | 
| HACA server template |  | 
| Portal server configuration |  | 
| Portal access profile |  | 
| Authentication profile |  | 
| VAP profile |  | 
<HUAWEI> system-view
[HUAWEI] sysname DeviceB
[DeviceB] vlan batch 102
[DeviceB] interface 10ge 1/0/1
[DeviceB-10GE1/0/1] description to_AP1
[DeviceB-10GE1/0/1] port link-type trunk
[DeviceB-10GE1/0/1] port trunk pvid vlan 102
[DeviceB-10GE1/0/1] undo port trunk allow-pass vlan 1
[DeviceB-10GE1/0/1] port trunk allow-pass vlan 102
[DeviceB-10GE1/0/1] quit
[DeviceB] interface 10ge 1/0/2
[DeviceB-10GE1/0/2] description to_AP2
[DeviceB-10GE1/0/2] port link-type trunk
[DeviceB-10GE1/0/2] port trunk pvid vlan 102
[DeviceB-10GE1/0/2] undo port trunk allow-pass vlan 1
[DeviceB-10GE1/0/2] port trunk allow-pass vlan 102
[DeviceB-10GE1/0/2] quit
[DeviceB] interface 10ge 1/0/3
[DeviceB-10GE1/0/3] description to_WAC
[DeviceB-10GE1/0/3] port link-type trunk
[DeviceB-10GE1/0/3] undo port trunk allow-pass vlan 1
[DeviceB-10GE1/0/3] port trunk allow-pass vlan 102
[DeviceB-10GE1/0/3] quit
<HUAWEI> system-view
[HUAWEI] sysname DeviceA 
[DeviceA] vlan batch 101 102 103
[DeviceA] interface 10ge 1/0/1
[DeviceA-10GE1/0/1] description to_DeviceB
[DeviceA-10GE1/0/1] port link-type trunk
[DeviceA-10GE1/0/1] undo port trunk allow-pass vlan 1
[DeviceA-10GE1/0/1] port trunk allow-pass vlan 102
[DeviceA-10GE1/0/1] quit
[DeviceA] interface 10ge 1/0/2
[DeviceA-10GE1/0/2] description to_HACA_Server
[DeviceA-10GE1/0/2] port link-type trunk
[DeviceA-10GE1/0/2] undo port trunk allow-pass vlan 1
[DeviceA-10GE1/0/2] port trunk allow-pass vlan 103
[DeviceA-10GE1/0/2] quit
# Configure DHCP on DeviceA. Configure VLANIF 102 to assign IP addresses to APs and VLANIF 101 to assign IP addresses to wireless users.
[DeviceA] dhcp enable
[DeviceA] interface vlanif 101
[DeviceA-Vlanif101] ip address 192.168.101.1 24
[DeviceA-Vlanif101] dhcp select interface
[DeviceA-Vlanif101] quit
[DeviceA] interface vlanif 102
[DeviceA-Vlanif102] ip address 192.168.102.1 24
[DeviceA-Vlanif102] dhcp select interface 
[DeviceA-Vlanif102] quit
[DeviceA] vlan 103
[DeviceA] interface vlanif 103
[DeviceA-Vlanif103] ip address 192.168.103.1 24
[DeviceA-Vlanif103] quit
[DeviceA] ip route-static 192.168.100.0 255.255.255.0 192.168.103.2
[DeviceA] pki realm abcd  
[DeviceA-pki-abcd] quit  
[DeviceA] ssl policy huawei 
[DeviceA-ssl-policy-huawei] pki-domain abcd
Warning: A preset certificate is loaded to the specified PKI domain. The current operation has security risks. Continue? [Y/N]: y
[DeviceA-ssl-policy-huawei] quit
[DeviceA] haca-server template HACA 
[DeviceA-haca-HACA] haca-server server-address 192.168.100.100 50301 ssl-policy huawei 
[DeviceA-haca-HACA] haca enable
[DeviceA-haca-HACA] quit
[DeviceA] url-template name urlTemplate_0
[DeviceA-url-template-urlTemplate_0] url https://192.168.100.100:19008/portal
[DeviceA-url-template-urlTemplate_0] url-parameter device-mac wac-mac redirect-url redirect-url ssid ssid user-ipaddress uaddress user-mac umac
[DeviceA-url-template-urlTemplate_0] quit
[DeviceA] web-auth-server Portal
[DeviceA-web-auth-server-Portal] server-ip 192.168.100.100 
[DeviceA-web-auth-server-Portal] port 50100 
[DeviceA-web-auth-server-Portal] url-template urlTemplate_0 
[DeviceA-web-auth-server-Portal] protocol haca
[DeviceA-web-auth-server-Portal] quit
[DeviceA] portal-access-profile name wlan-net-auth 
[DeviceA-portal-access-profile-wlan-net-auth] web-auth-server Portal
[DeviceA] aaa
[DeviceA-aaa] authentication-scheme wlan-net-auth
[DeviceA-aaa-authen-wlan-net-auth] authentication-mode haca
[DeviceA-aaa-authen-wlan-net-auth] quit
# Set the accounting mode to non-accounting.
[DeviceA-aaa] accounting-scheme wlan-net-auth 
[DeviceA-aaa-accounting-wlan-net-auth] accounting-mode none
[DeviceA-aaa-accounting-wlan-net-auth] quit
[DeviceA-aaa] domain wlan-net-auth
[DeviceA-aaa-domain-wlan-net-auth] authentication-scheme wlan-net-auth
[DeviceA-aaa-domain-wlan-net-auth] accounting-scheme wlan-net-auth
[DeviceA-aaa-domain-wlan-net-auth] haca-server HACA
[DeviceA-aaa-domain-wlan-net-auth] quit
[DeviceA-aaa] quit
[DeviceA]mac-access-profile name wlan-net-auth
[DeviceA-mac-access-profile-wlan-net-auth] quit
[DeviceA] authentication-profile name wlan-net-auth
[DeviceA-authen-profile-wlan-net-auth] mac-access-profile wlan-net-auth 
[DeviceA-authen-profile-wlan-net-auth] portal-access-profile wlan-net-auth 
[DeviceA-authen-profile-wlan-net-auth] free-rule-template default_free_rule 
[DeviceA-authen-profile-wlan-net-auth] authentication timer re-authen pre-authen 0 wlan-user 
[DeviceA-authen-profile-wlan-net-auth] access-domain wlan-net-auth
[DeviceA-authen-profile-wlan-net-auth] quit
# Create a regulatory domain profile, configure the WAC country code in the profile, and bind the profile to the created AP group.
[DeviceA] interface vlanif 100
[DeviceA-Vlanif100] ip address 10.23.100.1 24
[DeviceA-Vlanif100] quit
[DeviceA] capwap dtls no-auth enable
Warning: This operation allows for device access in non-DTLS encryption mode even when DTLS is enabled and brings security risks. Af
ter the device goes online for the first time, disable this function to prevent security risks. Continue? [Y/N]:y
[DeviceA] capwap source interface vlanif 100
Set the DTLS PSK(contains 8-32 plain-text characters, or 128 or 148 cipher-text characters that must be a combination of at least tw
o of the following: lowercase letters a to z, uppercase letters A to Z, digits, and special characters):********
Confirm PSK:********
Set the user name for FIT APs(The value is a string of 4 to 31 characters, which can contain letters, underscores, and digits, and m
ust start with a letter):********
Set the password for FIT APs(plain-text password of 8-128 characters or cipher-text password of 128-268 characters that must be a co
mbination of at least three of the following: lowercase letters a to z, uppercase letters A to Z, digits, and special characters):******
Confirm password:********
Set the PSK of the global offline management VAP(plain-text password of 8-63 characters or cipher-text password of 128-188 character
s that must be a combination of at least two of the following: lowercase letters a to z, uppercase letters A to Z, digits, and speci
al characters):********
Confirm PSK:********
[DeviceA] wlan
[DeviceA-wlan-view] security-profile name wlan-net-security
[DeviceA-wlan-sec-prof-wlan-net-security] security open
[DeviceA-wlan-sec-prof-wlan-net-security] quit
[DeviceA-wlan-view] ssid-profile name wlan-net-ssid
[DeviceA-wlan-ssid-prof-wlan-net-ssid] ssid wlan-net
[DeviceA-wlan-ssid-prof-wlan-net-ssid] quit
[DeviceA-wlan-view] vap-profile name wlan-net-vap
[DeviceA-wlan-vap-prof-wlan-net-vap] forward-mode tunnel
[DeviceA-wlan-vap-prof-wlan-net-vap] service-vlan vlan-id 101
[DeviceA-wlan-vap-prof-wlan-net-vap] security-profile wlan-net-security
[DeviceA-wlan-vap-prof-wlan-net-vap] ssid-profile wlan-net-ssid
[DeviceA-wlan-vap-prof-wlan-net-vap] authentication-profile wlan-net-auth
[DeviceA-wlan-vap-prof-wlan-net-vap] quit
[DeviceA-wlan-view] ap-group name ap-group1
[DeviceA-wlan-ap-group-ap-group1] vap-profile wlan-net-vap wlan 1 radio 0
[DeviceA-wlan-ap-group-ap-group1] vap-profile wlan-net-vap wlan 1 radio 1
[DeviceA-wlan-ap-group-ap-group1] quit
#
sysname DeviceB
#
vlan batch 102
#
interface 10GE1/0/1
 description to_AP1
 port link-type trunk
 port trunk pvid vlan 102
 undo port trunk allow-pass vlan 1
 port trunk allow-pass vlan 102
#
interface 10GE1/0/2
 description to_AP2
 port link-type trunk
 port trunk pvid vlan 102
 undo port trunk allow-pass vlan 1
 port trunk allow-pass vlan 102
#
interface 10GE1/0/3
 description to_WAC
 port link-type trunk
 undo port trunk allow-pass vlan 1
 port trunk allow-pass vlan 102
#
return
#
pki realm abcd
#
sysname DeviceA
#
ssl policy huawei
 pki-domain abcd
#
vlan batch 101 to 103
#
free-rule-template name default_free_rule 
 free-rule 1 destination ip 10.23.202.1 mask 255.255.255.255
#
authentication-profile name wlan-net-auth
 mac-access-profile wlan-net-auth 
 portal-access-profile wlan-net-auth 
 free-rule-template default_free_rule 
 authentication timer re-authen pre-authen 0 wlan-user 
 access-domain wlan-net-auth
#
mac-access-profile name wlan-net-auth
#
haca-server template HACA 
 haca-server server-address 192.168.100.100 50301 ssl-policy huawei
 haca enable
#
url-template name urlTemplate_0
 url https://192.168.100.100:19008/portal
 url-parameter device-mac wac-mac redirect-url redirect-url ssid ssid user-ipaddress uaddress user-mac umac
#
web-auth-server Portal
 server-ip 192.168.100.100 
 port 50100 
 url-template urlTemplate_0 
 protocol haca
#
portal-access-profile name wlan-net-auth 
 web-auth-server Portal
#
aaa
 authentication-scheme wlan-net-auth
  authentication-mode haca
 accounting-scheme wlan-net-auth 
  accounting-mode none
 domain wlan-net-auth
  authentication-scheme wlan-net-auth
  accounting-scheme wlan-net-auth
  haca-server HACA
#
dhcp enable
#
interface Vlanif101
 ip address 192.168.101.1 255.255.255.0
 dhcp select interface
#
interface Vlanif102
 ip address 192.168.102.1 255.255.255.0
 dhcp select interface
#
interface Vlanif103
 ip address 192.168.103.1 255.255.255.0
#
interface 10GE1/0/1
 description to_DeviceB
 port link-type trunk
 undo port trunk allow-pass vlan 1
 port trunk allow-pass vlan 102
#
interface 10GE1/0/2
 description to_HACA_Server
 port link-type trunk
 undo port trunk allow-pass vlan 1
 port trunk allow-pass vlan 103
#
capwap source interface vlanif 102
capwap dtls psk %+%##!!!!!!!!!"!!!!"!!!!*!!!!a4O4$.&PxRTGtHR(VaB4*(KhWT"C@K+HZ%@!!!!!2jp5!!!!!!;!!!!..AjEF}u<=!%e=F%Dz-Y[R--3yrC-*MVXf<!!!!!%+%#
#
wlan
 security-profile name wlan-net-security
  security open
 ssid-profile name wlan-net-ssid
  ssid wlan-net
 vap-profile name wlan-net-vap
  forward-mode tunnel
  service-vlan vlan-id 101
  ssid-profile wlan-net-ssid
  security-profile wlan-net-security
  authentication-profile wlan-net-auth
 regulatory-domain-profile name domain1
 ap-group name ap-group1
  regulatory-domain-profile domain1
  radio 0
   vap-profile wlan-net-vap wlan 1
  radio 1
   vap-profile wlan-net-vap wlan 1
 ap-id 0 type-id 1 ap-mac 00e0-fc12-3456 ap-sn 210235554710CB000042
  ap-name area_1
  ap-group ap-group1
  radio 0
   channel 20mhz 6
   eirp 127
   calibrate auto-channel-select disable  
   calibrate auto-txpower-select disable
  radio 1
   channel 80mhz 149
   eirp 127
   calibrate auto-channel-select disable 
   calibrate auto-txpower-select disable
#
ip route-static 192.168.100.0 255.255.255.0 192.168.103.2
#
On the network shown in Figure 3-136, the AC and AP are third-party WLAN access devices. Mobile STAs can access the Internet only after associating with a Wi-Fi network and passing Portal authentication. Specific requirements are as follows:
# Add 10GE 1/0/1 that connects DeviceA to DeviceB to VLAN 100 and VLAN 101. In this example, the AC uses the direct forwarding mode. If the AC uses the tunnel forwarding mode, you do not need to add this interface to VLAN 101.
<HUAWEI> system-view
[HUAWEI] sysname DeviceA
[DeviceA] vlan batch 20 100 101
[DeviceA] interface 10ge 1/0/1
[DeviceA-10GE1/0/1] port link-type trunk
[DeviceA-10GE1/0/1] port trunk allow-pass vlan 100 101
[DeviceA-10GE1/0/1] quit
# Add 10GE 1/0/2 that connects DeviceA to the AC to VLAN 100 and VLAN 101.
[DeviceA] interface 10ge 1/0/2
[DeviceA-10GE1/0/2] port link-type trunk
[DeviceA-10GE1/0/2] port trunk allow-pass vlan 100 101
[DeviceA-10GE1/0/2] quit
# Add 10GE 1/0/3 that connects DeviceA to the server to VLAN 20.
[DeviceA] interface 10ge 1/0/3
[DeviceA-10GE1/0/3] port link-type trunk
[DeviceA-10GE1/0/3] port trunk allow-pass vlan 20
[DeviceA-10GE1/0/3] quit
# Add interfaces on DeviceB to VLAN 100 and VLAN 101. In this example, the AC uses the direct forwarding mode. If the AC uses the tunnel forwarding mode, you do not need to add these interfaces to VLAN 101.
<HUAWEI> system-view
[HUAWEI] sysname DeviceB
[DeviceB] vlan batch 100 101
[DeviceB] interface 10ge 1/0/1
[DeviceB-10GE1/0/1] port link-type trunk
[DeviceB-10GE1/0/1] port trunk allow-pass vlan 100 101
[DeviceB-10GE1/0/1] quit
[DeviceB] interface 10ge 1/0/2
[DeviceB-10GE1/0/2] port link-type trunk
[DeviceB-10GE1/0/2] port trunk allow-pass vlan 100 101
[DeviceB-10GE1/0/2] port trunk pvid vlan 100
[DeviceB-10GE1/0/2] quit
# On DeviceA, configure an IP address for a VLANIF interface and configure a route destined for the server. In this example, the next-hop IP address is 10.4.1.2.
[DeviceA] interface vlanif 20
[DeviceA-Vlanif20] ip address 10.4.1.1 24
[DeviceA-Vlanif20] quit
[DeviceA] interface vlanif 101
[DeviceA-Vlanif101] ip address 10.3.1.2 24
[DeviceA-Vlanif101] quit
[DeviceA] ip route-static 10.5.1.0 255.255.255.0 10.4.1.2
[DeviceA] radius-server template rd1
[DeviceA-radius-rd1] radius-server authentication 10.5.1.3 1812
[DeviceA-radius-rd1] radius-server accounting 10.5.1.3 1813
[DeviceA-radius-rd1] radius-server shared-key cipher YsHsjx_202206139
[DeviceA-radius-rd1] quit
[DeviceA-aaa] accounting-scheme acco1
[DeviceA-aaa-accounting-acco1] accounting-mode radius
[DeviceA-aaa-accounting-acco1] accounting realtime 15  
[DeviceA-aaa-accounting-acco1] quit
[DeviceA-aaa] domain isp
[DeviceA-aaa-domain-isp] authentication-scheme abc
[DeviceA-aaa-domain-isp] accounting-scheme acco1
[DeviceA-aaa-domain-isp] radius-server rd1
[DeviceA-aaa-domain-isp] quit
[DeviceA-aaa] quit
[DeviceA] aaa
[DeviceA-aaa] authentication-scheme noauthen
[DeviceA-aaa-authen-noauthen] authentication-mode none
[DeviceA-aaa-authen-noauthen] quit
[DeviceA-aaa] domain ap_noauthen
[DeviceA-aaa-domain-ap_noauthen] authentication-scheme noauthen
[DeviceA-aaa-domain-ap_noauthen] quit
[DeviceA-aaa] quit
# Configure non-authentication for the AP using either of the following methods:
[DeviceA] domain ap_noauthen mac-authen force mac-address 00e0-fc74-9640 mask ffff-ffff-ff00
[DeviceA] access-context profile enable 
[DeviceA] access-context profile name ap_access   
[DeviceA-access-context-ap_access] if-match vlan-id 100   
[DeviceA-access-context-ap_access] quit
[DeviceA] access-author policy name ap_noauthen   
[DeviceA-access-author-ap_noauthen] match access-context-profile ap_access action access-domain ap_noauthen force
[DeviceA-access-author-ap_noauthen] quit
[DeviceA] access-author policy ap_noauthen global  
The priorities of the forcible domain, domain carried in the user name, and default domain in different views are as follows in descending order: forcible domain with a specified authentication mode in an authentication profile > forcible domain in an authentication profile > forcible domain with a specified authentication mode based on a user context profile > forcible domain based on a user context profile > domain carried in the user name > default domain with a specified authentication mode in an authentication profile > default domain in an authentication profile > default domain with a specified authentication mode based on a user context profile > default domain based on a user context profile > global default domain.
[DeviceA] web-auth-server abc
[DeviceA-web-auth-server-abc] server-source ip-address 10.4.1.1  
[DeviceA-web-auth-server-abc] server-ip 10.5.1.3
[DeviceA-web-auth-server-abc] port 50200
[DeviceA-web-auth-server-abc] url http://10.5.1.3:8445/portal
[DeviceA-web-auth-server-abc] shared-key cipher YsHsjx_202206
[DeviceA-web-auth-server-abc] quit
# Configure the Portal access profile web1.
[DeviceA] authentication-profile name p1
[DeviceA-authen-profile-p1] mac-access-profile m1    
[DeviceA-authen-profile-p1] portal-access-profile web1 
[DeviceA-authen-profile-p1] access-domain isp  
[DeviceA-authen-profile-p1] quit
#
sysname DeviceB
#
vlan batch 100 101
#
interface 10GE 1/0/1
 port link-type trunk
 port trunk allow-pass vlan 100 101
#
interface 10GE 1/0/2
 port link-type trunk
 port trunk allow-pass vlan 100 101
 port trunk pvid vlan 100
#
return
#
sysname DeviceA
#
vlan batch 20 100 101
#
authentication-profile name p1
 mac-access-profile m1    
 portal-access-profile web1 
 access-domain isp
#
radius-server template rd1
 radius-server authentication 10.5.1.3 1812
 radius-server accounting 10.5.1.3 1813
 radius-server shared-key cipher %^%#6A6YSa1*aLmY+*4|W|<Xqr9XDeO6l$IHV{49K(s3%^%#
#
web-auth-server abc
 server-source ip-address 10.4.1.1
 server-ip 10.5.1.3
 port 50200
 url http://10.5.1.3:8445/portal
 shared-key cipher %^%#-;k-</gjVJ=ZK&Ea)<WB(j1FD8HJOGq^@$Ly=\0Y%^%#
#
portal-access-profile name web1
 web-auth-server abc
#
interface 10GE 1/0/1
 port link-type trunk
 port trunk allow-pass vlan 100 101
 authentication-profile p1
#
interface 10GE 1/0/2
 port link-type trunk
 port trunk allow-pass vlan 100 101
#
interface 10GE 1/0/3
 port link-type trunk
 port trunk allow-pass vlan 20
#
interface vlanif 20
 ip address 10.4.1.1 255.255.255.0
#
interface vlanif 101
 ip address 10.3.1.2 255.255.255.0
#
aaa
 authentication-scheme abc
  authentication-mode radius
 authentication-scheme noauthen
  authentication-mode none
 accounting-scheme acco1
  accounting-mode radius
  accounting realtime 15  
 domain isp
  authentication-scheme abc
  accounting-scheme acco1
  radius-server rd1
 domain ap_noauthen
  authentication-scheme noauthen
#
domain ap_noauthen mac-authen force mac-address 00e0-fc74-9640 mask ffff-ffff-ff00
#
access-context profile name ap_access 
 if-match vlan-id 100   
access-context profile enable 
#
access-author policy name ap_noauthen   
 match access-context-profile ap_access action access-domain ap_noauthen force
access-author policy ap_noauthen global
#
mac-access-profile name m1
#
ip route-static 10.5.1.0 255.255.255.0 10.4.1.2
#
return
On the enterprise network shown in Figure 3-137, DeviceC, DeviceB, and DeviceA function as an access switch, an aggregation switch, and a core switch, respectively. DeviceA assigns IP addresses to users and APs. Wired users are connected to DeviceC and can access the enterprise network only after passing 802.1X or MAC address authentication. Wireless users are connected to the AP that is connected to DeviceC and can access the enterprise network only after passing MAC address-prioritized Portal authentication. Authentication is not required for the AP as the authentication server does not store AP information.
# On DeviceA, create VLANs and configure allowed VLANs on interfaces. (VLANIF 100 is the source interface of the AC, VLAN 200 is the wired service VLAN, VLAN 201 is the wireless service VLAN, and VLAN 300 is the uplink VLAN for communicating with servers.)
<HUAWEI> system-view
[HUAWEI] sysname DeviceA
[DeviceA] vlan batch 100 200 201 300
[DeviceA] interface eth-trunk 10
[DeviceA-Eth-Trunk10] port link-type trunk
[DeviceA-Eth-Trunk10] port trunk allow-pass vlan 100 200
[DeviceA-Eth-Trunk10] quit
[DeviceA] interface 10ge 1/0/1
[DeviceA-10GE1/0/1] eth-trunk 10
[DeviceA-10GE1/0/1] quit
[DeviceA] interface 10ge 1/0/2
[DeviceA-10GE1/0/2] eth-trunk 10
[DeviceA-10GE1/0/2] quit
[DeviceA] interface 10ge 1/0/3
[DeviceA-10GE1/0/3] port link-type trunk
[DeviceA-10GE1/0/3] port trunk allow-pass vlan 300
[DeviceA-10GE1/0/3] quit
[DeviceA] interface vlanif 100
[DeviceA-Vlanif100] ip address 10.1.1.1 24  
[DeviceA-Vlanif100] quit
[DeviceA] interface vlanif 200
[DeviceA-Vlanif200] ip address 10.2.1.1 24
[DeviceA-Vlanif200] quit
[DeviceA] interface vlanif 201
[DeviceA-Vlanif201] ip address 10.2.2.1 24
[DeviceA-Vlanif201] quit
[DeviceA] interface vlanif 300
[DeviceA-Vlanif300] ip address 10.3.1.1 24
[DeviceA-Vlanif300] quit
[DeviceA] ip route-static 10.5.1.0 255.255.255.0 10.3.1.2 
# On DeviceB, configure transparent transmission of 802.1X packets, create VLANs, and configure allowed VLANs on interfaces.
DeviceB is a Layer 2 switch. To ensure that 802.1X authentication can be performed for users, configure transparent transmission of 802.1X packets on DeviceB.
<HUAWEI> system-view
[HUAWEI] sysname DeviceB
[DeviceB] vlan batch 100 200
[DeviceB] l2protocol-tunnel user-defined-protocol 802.1X protocol-mac 0180-c200-0003 group-mac 0100-0000-0002
[DeviceB] interface eth-trunk 10
[DeviceB-Eth-Trunk10] l2protocol-tunnel user-defined-protocol 802.1X enable
[DeviceB-Eth-Trunk10] port link-type trunk
[DeviceB-Eth-Trunk10] port trunk allow-pass vlan 100 200
[DeviceB-Eth-Trunk10] quit
[DeviceB] interface 10ge 1/0/1
[DeviceB-10GE1/0/1] eth-trunk 10
[DeviceB-10GE1/0/1] quit
[DeviceB] interface 10ge 1/0/2
[DeviceB-10GE1/0/2] eth-trunk 10
[DeviceB-10GE1/0/2] quit
[DeviceB] interface 10ge 1/0/3
[DeviceB-10GE1/0/3] l2protocol-tunnel user-defined-protocol 802.1X enable
[DeviceB-10GE1/0/3] port link-type trunk
[DeviceB-10GE1/0/3] port trunk allow-pass vlan 100 200
[DeviceB-10GE1/0/3] quit
# On DeviceC, configure transparent transmission of 802.1X packets, create VLANs, and configure allowed VLANs on interfaces.
DeviceC is a Layer 2 switch. To ensure that 802.1X authentication can be performed for users, configure transparent transmission of 802.1X packets on DeviceC.
<HUAWEI> system-view
[HUAWEI] sysname DeviceC
[DeviceC] vlan batch 100 200
[DeviceC] l2protocol-tunnel user-defined-protocol 802.1X protocol-mac 0180-c200-0003 group-mac 0100-0000-0002
[DeviceC] interface 10ge 1/0/1
[DeviceC-10GE1/0/1] port link-type trunk
[DeviceC-10GE1/0/1] undo port trunk allow-pass vlan 1
[DeviceC-10GE1/0/1] port trunk pvid vlan 100
[DeviceC-10GE1/0/1] port trunk allow-pass vlan 100
[DeviceC-10GE1/0/1] quit
[DeviceC] interface 10ge 1/0/2
[DeviceC-10GE1/0/2] l2protocol-tunnel user-defined-protocol 802.1X enable
[DeviceC-10GE1/0/2] port link-type access
[DeviceC-10GE1/0/2] port default vlan 200
[DeviceC-10GE1/0/2] quit
[DeviceC] interface 10ge 1/0/3
[DeviceC-10GE1/0/3] l2protocol-tunnel user-defined-protocol 802.1X enable
[DeviceC-10GE1/0/3] port link-type trunk
[DeviceC-10GE1/0/3] port trunk allow-pass vlan 100 200
[DeviceC-10GE1/0/3] quit
Configure VLANIF 100, VLANIF 200, and VLANIF 201 to assign IP addresses to APs, wired users, and wireless users, respectively.
[DeviceA] dhcp enable
[DeviceA] interface vlanif 100
[DeviceA-Vlanif100] dhcp select interface
[DeviceA-Vlanif100] quit
[DeviceA] interface vlanif 200
[DeviceA-Vlanif200] dhcp select interface 
[DeviceA-Vlanif200] quit
[DeviceA] interface vlanif 201
[DeviceA-Vlanif201] dhcp select interface 
[DeviceA-Vlanif201] quit
[DeviceA] dot1x-access-profile name d1
[DeviceA-dot1x-access-profile-d1] dot1x authentication-method eap
[DeviceA-dot1x-access-profile-d1] quit
[DeviceA] web-auth-server abc
[DeviceA-web-auth-server-abc] server-source ip-address 10.3.1.1   
[DeviceA-web-auth-server-abc] server-ip 10.5.1.3
[DeviceA-web-auth-server-abc] port 50200
[DeviceA-web-auth-server-abc] url http://10.5.1.3:8080/portal
[DeviceA-web-auth-server-abc] shared-key cipher YsHsjx_202206
[DeviceA-web-auth-server-abc] quit
[DeviceA] authentication-profile name p1
[DeviceA-authen-profile-p1] mac-access-profile m1    
[DeviceA-authen-profile-p1] dot1x-access-profile d1
[DeviceA] authentication-profile name p2
[DeviceA-authen-profile-p2] mac-access-profile m1    
[DeviceA-authen-profile-p2] portal-access-profile web1
[DeviceA-authen-profile-p2] access-domain isp
# Enable MAC address bypass authentication and configure an authentication domain.
[DeviceA-authen-profile-p1] authentication dot1x-mac-bypass   
[DeviceA-authen-profile-p1] access-domain isp  
[DeviceA-authen-profile-p1] quit
[DeviceA] interface eth-trunk 10
[DeviceA-Eth-Trunk10] authentication-profile p1
[DeviceA-Eth-Trunk10] quit
[DeviceA] free-rule-template name default_free_rule
[DeviceA-free-rule-default_free_rule] free-rule 1 source vlan 100 
[DeviceA-free-rule-default_free_rule] quit
[DeviceA] authentication-profile name p1
[DeviceA-authen-profile-p1] free-rule-template default_free_rule
[DeviceA-authen-profile-p1] quit
[DeviceA] wlan
[DeviceA-wlan] security-profile name wlan-net-security
[DeviceA-wlan-sec-prof-wlan-net-security] security open
[DeviceA-wlan-sec-prof-wlan-net-security] quit
[DeviceA-wlan] ssid-profile name wlan-net-ssid
[DeviceA-wlan-ssid-prof-wlan-net-ssid] ssid wlan-net
[DeviceA-wlan-ssid-prof-wlan-net-ssid] quit
[DeviceA-wlan] vap-profile name wlan-net-vap
[DeviceA-wlan-vap-prof-wlan-net-vap] forward-mode tunnel
[DeviceA-wlan-vap-prof-wlan-net-vap] service-vlan vlan-id 201
[DeviceA-wlan-vap-prof-wlan-net-vap] security-profile wlan-net-security
[DeviceA-wlan-vap-prof-wlan-net-vap] ssid-profile wlan-net-ssid
[DeviceA-wlan-vap-prof-wlan-net-vap] authentication-profile p2
[DeviceA-wlan-vap-prof-wlan-net-vap] quit
[DeviceA-wlan] ap-group name ap-group1
[DeviceA-wlan-ap-group-ap-group1] vap-profile wlan-net-vap wlan 1 radio 0
[DeviceA-wlan-ap-group-ap-group1] vap-profile wlan-net-vap wlan 1 radio 1
[DeviceA-wlan-ap-group-ap-group1] vap-profile wlan-net-vap wlan 1 radio 2
[DeviceA-wlan-ap-group-ap-group1] quit
Users can access the network after being successfully authenticated.
After the users go online, you can run the display access-user command on the device to check online user information.
#
sysname DeviceA
#
vlan batch 100 200 201 300
#
dhcp enable
#
authentication-profile name p1
 mac-access-profile m1
 dot1x-access-profile d1
 authentication dot1x-mac-bypass
 access-domain isp
 free-rule-template default_free_rule
#
authentication-profile name p2
 mac-access-profile m1
 portal-access-profile web1
 access-domain isp
#
free-rule-template name default_free_rule
 free-rule 1 source vlan 100
#
web-auth-server abc
 server-source ip-address 10.3.1.1
 server-ip 10.5.1.3
 port 50200
 url http://10.5.1.3:8080/portal
 shared-key cipher %^%#-;k-</gjVJ=ZK&Ea)<WB(j1FD8HJOGq^@$Ly=\0Y%^%#
#
portal-access-profile name web1
 web-auth-server abc
#
radius-server template rd1
 radius-server authentication 10.5.1.3 1812
 radius-server accounting 10.5.1.3 1813
 radius-server shared-key cipher %^%#x$`LC*6I3H&~})~8O[$F,,o6FN!+35|H-E3Wi}Z:%^%#
#
interface eth-trunk 10
 port link-type trunk
 port trunk allow-pass vlan 100 200
 authentication-profile p1
#
interface 10GE 1/0/1
 eth-trunk 10
#
interface 10GE 1/0/2
 eth-trunk 10
#
interface 10GE 1/0/3
 port link-type trunk
 port trunk allow-pass vlan 300
#
interface vlanif 100
 ip address 10.1.1.1 255.255.255.0 
 dhcp select interface
#
interface vlanif 200
 ip address 10.2.1.1 255.255.255.0
 dhcp select interface
#
interface vlanif 201
 ip address 10.2.2.1 255.255.255.0
 dhcp select interface
#
interface vlanif 300
 ip address 10.3.1.1 255.255.255.0
#
aaa
 authentication-scheme abc
  authentication-mode radius
 authentication-scheme noauthen
  authentication-mode none
 accounting-scheme acco1
  accounting-mode radius
  accounting realtime 15
 domain isp
  authentication-scheme abc
  accounting-scheme acco1
  radius-server rd1
 domain ap_noauthen
  authentication-scheme noauthen
#
wlan
 security-profile name wlan-net-security
  security open
 ssid-profile name wlan-net-ssid
  ssid wlan-net
 vap-profile name wlan-net-vap
  forward-mode tunnel
  service-vlan vlan-id 201
  ssid-profile wlan-net-ssid
  security-profile wlan-net-security
  authentication-profile p2
 regulatory-domain-profile name domain1
  country-code cn
 ap-group name ap-group1
  regulatory-domain-profile domain1
  radio 0
   vap-profile wlan-net-vap wlan 1
  radio 1
   vap-profile wlan-net-vap wlan 1
  radio 2
   vap-profile wlan-net-vap wlan 1
 ap-id 0 type-id 1 ap-mac 00e0-fc12-3456 ap-sn 210235554710CB000042
  ap-name area_1
  ap-group ap-group1
#
domain ap_noauthen mac-authen force mac-address 00e0-fc76-e360 mask ffff-ffff-ff00  
#
access-context profile name ap_access
 if-match vlan-id 100
access-context profile enable
#
access-author policy name ap_noauthen
 match access-context-profile ap_access action access-domain ap_noauthen force
access-author policy ap_noauthen global
#
mac-access-profile name m1
#
dot1x-access-profile name d1
#
capwap source interface vlanif 100
#
ip route-static 10.5.1.0 255.255.255.0 10.3.1.2
#
return
#
sysname DeviceB
#
vlan batch 100 200
#
l2protocol-tunnel user-defined-protocol 802.1X protocol-mac 0180-c200-0003 group-mac 0100-0000-0002
#
interface eth-trunk 10
 l2protocol-tunnel user-defined-protocol 802.1X enable
 port link-type trunk
 port trunk allow-pass vlan 100 200
#
interface 10GE 1/0/1
 eth-trunk 10
#
interface 10GE 1/0/2
 eth-trunk 10
#
interface 10GE 1/0/3
 l2protocol-tunnel user-defined-protocol 802.1X enable
 port link-type trunk
 port trunk allow-pass vlan 100 200
#
return
#
sysname DeviceC
#
vlan batch 100 200
#
l2protocol-tunnel user-defined-protocol 802.1X protocol-mac 0180-c200-0003 group-mac 0100-0000-0002
#
interface 10GE 1/0/1
 port link-type trunk
 undo port trunk allow-pass vlan 1
 port trunk pvid vlan 100
 port trunk allow-pass vlan 100
#
interface 10GE 1/0/2
 l2protocol-tunnel user-defined-protocol 802.1X enable
 port link-type access
 port default vlan 200
#
interface 10GE 1/0/3
 l2protocol-tunnel user-defined-protocol 802.1X enable
 port link-type trunk
 port trunk allow-pass vlan 100 200
#
return
In Figure 3-138, to save investment costs, users require that IP phones and PCs connect to the network through VoIP. IP phones support LLDP and can obtain the voice VLAN through LLDP. The network needs to meet the following requirements:
The priority of voice packets sent by IP phones is low and needs to be increased to ensure communication quality.
Voice and data packets are transmitted in VLAN 200 and VLAN 100, respectively.
IP phones and PCs obtain IP addresses from a DHCP server.
IP phones connect to DeviceA without authentication and PCs connect to DeviceA through 802.1X authentication.
[HUAWEI] authentication mac-move enable vlan all
[HUAWEI] authentication mac-move detect enable
# Create VLANs and enable the LLDP function.
<HUAWEI> system-view
[HUAWEI] sysname DeviceA
[DeviceA] vlan batch 100 200 300
[DeviceA] lldp enable
# Add interfaces to the data VLAN.
[DeviceA] interface 10ge 1/0/1
[DeviceA-10GE1/0/1] portswitch
[DeviceA-10GE1/0/1] port link-type hybrid
[DeviceA-10GE1/0/1] port hybrid pvid vlan 100
[DeviceA-10GE1/0/1] port hybrid untagged vlan 100
[DeviceA-10GE1/0/1] quit
[DeviceA] interface 10ge 1/0/2
[DeviceA-10GE1/0/2] portswitch
[DeviceA-10GE1/0/2] port link-type hybrid
[DeviceA-10GE1/0/2] port hybrid pvid vlan 100
[DeviceA-10GE1/0/2] port hybrid untagged vlan 100
[DeviceA-10GE1/0/2] quit
[DeviceA] interface 10ge 1/0/3
[DeviceA-10GE1/0/3] portswitch
[DeviceA-10GE1/0/3] port link-type access
[DeviceA-10GE1/0/3] port default vlan 300
[DeviceA-10GE1/0/3] quit
[DeviceA] interface vlanif 100
[DeviceA-Vlanif100] ip address 10.1.1.1 24  
[DeviceA-Vlanif100] quit
[DeviceA] interface vlanif 300
[DeviceA-Vlanif300] ip address 10.3.1.1 24
[DeviceA-Vlanif300] quit
# Add interfaces to the voice VLAN.
[DeviceA] interface 10ge 1/0/1
[DeviceA-10GE1/0/1] port hybrid tagged vlan 200
[DeviceA-10GE1/0/1] quit
[DeviceA] interface 10ge 1/0/2
[DeviceA-10GE1/0/2] port hybrid tagged vlan 200
[DeviceA-10GE1/0/2] quit
[DeviceA] interface vlanif 200
[DeviceA-Vlanif200] ip address 10.2.1.1 24  
[DeviceA-Vlanif200] quit
# Enable the voice VLAN on interfaces and configure the interfaces to authorize the voice VLAN in TLV mode.
[DeviceA] interface 10ge 1/0/1
[DeviceA-10GE1/0/1] voice-vlan 200 enable   
[DeviceA-10GE1/0/1] stp edged-port enable   
[DeviceA-10GE1/0/1] lldp tlv-enable med-tlv network-policy voice-vlan vlan 200 cos 6 dscp 60   
[DeviceA-10GE1/0/1] lldp mdn enable txrx //For IP phones that use non-standard discovery protocols such as CDP, perform this step to ensure that the device can establish neighbor relationships with these IP phones.
[DeviceA-10GE1/0/1] quit
[DeviceA] interface 10ge 1/0/2
[DeviceA-10GE1/0/2] voice-vlan 200 enable
[DeviceA-10GE1/0/2] stp edged-port enable 
[DeviceA-10GE1/0/2] lldp tlv-enable med-tlv network-policy voice-vlan vlan 200 cos 6 dscp 60  
[DeviceA-10GE1/0/2] lldp mdn enable txrx //For IP phones that use non-standard discovery protocols such as CDP, perform this step to ensure that the device can establish neighbor relationships with these IP phones.
[DeviceA-10GE1/0/2] quit
# Configure a static route from DeviceA to the server zone. In this example, the next-hop IP address is 10.3.1.2.
[DeviceA] ip route-static 0.0.0.0 0.0.0.0 10.3.1.2 
[DeviceA] radius-server template rd1
[DeviceA-radius-rd1] radius-server authentication 10.5.1.3 1812
[DeviceA-radius-rd1] radius-server accounting 10.5.1.3 1813
[DeviceA-radius-rd1] radius-server shared-key cipher Huawei@123456789
[DeviceA-radius-rd1] quit
# Configure a forcible authentication domain. If user names carry domain names, you do not need to configure a forcible authentication domain for 802.1X users.
[DeviceA-authen-profile-p1] access-domain isp dot1x force  
# Enable IP phones to go online without authentication.
[DeviceA-authen-profile-p1] authentication device-type voice authorize  
[DeviceA-authen-profile-p1] quit
# Bind the authentication profile to interfaces.
[DeviceA] interface 10ge 1/0/1
[DeviceA-10GE1/0/1] authentication-profile p1
[DeviceA-10GE1/0/1] quit
[DeviceA] interface 10ge 1/0/2
[DeviceA-10GE1/0/2] authentication-profile p1
[DeviceA-10GE1/0/2] quit
After an IP phone starts, it automatically obtains an IP address and connects to the network without authentication.
A user starts a PC and enters the user name and password, triggering 802.1X authentication. After the authentication is successful, the user can access the network.
#
sysname DeviceA
#
vlan batch 100 200 300
#
authentication-profile name p1
 dot1x-access-profile d1 
 access-domain isp dot1x force
 authentication device-type voice authorize
#
radius-server template rd1
 radius-server shared-key cipher %+%##!!!!!!!!!"!!!!"!!!!*!!!!SKvr${[Fs.3t@/5k|BENhEu>W(3\~XG!!D;!!!!!2jp5!!!!!!A!!!!3"pK8qv!}9M#(4$jGWvQF/R[CNe/+:W^jk8HUe&W%+%#
 radius-server authentication 10.5.1.3 1812 weight 80
 radius-server accounting 10.5.1.3 1813 weight 80
#
aaa
 authentication-scheme abc
  authentication-mode radius
 accounting-scheme acco1
  accounting-mode radius
  accounting realtime 15 
 domain isp
  authentication-scheme abc
  accounting-scheme acco1
  radius-server rd1
#
dhcp enable
#
interface Vlanif100
 ip address 10.1.1.1 255.255.255.0
 dhcp select interface
#
interface Vlanif200
 ip address 10.2.1.1 255.255.255.0
 dhcp select interface
#
interface Vlanif300
 ip address 10.3.1.1 255.255.255.0
#
interface 10GE1/0/1
 port link-type hybrid
 port hybrid pvid vlan 100     
 port hybrid tagged vlan 200
 port hybrid untagged vlan 100    
 stp edged-port enable 
 lldp mdn enable txrx 
 lldp tlv-enable med-tlv network-policy voice-vlan vlan 200 cos 6 dscp 60 
 voice-vlan 200 enable
 authentication-profile p1
#
interface 10GE1/0/2
 port link-type hybrid
 port hybrid pvid vlan 100 
 port hybrid tagged vlan 200    
 port hybrid untagged vlan 100 
 stp edged-port enable 
 lldp mdn enable txrx 
 lldp tlv-enable med-tlv network-policy voice-vlan vlan 200 cos 6 dscp 60 
 voice-vlan 200 enable
 authentication-profile p1
#
interface 10GE1/0/3
 port link-type access
 port default vlan 300 
#
ip route-static 0.0.0.0 0.0.0.0 10.3.1.2
#
dot1x-access-profile name d1
 dot1x timer client-timeout 30
#
mac-access-profile name m1
#
return
In Figure 3-139, to save investment costs, users require that IP phones and PCs connect to the network through VoIP. IP phones support LLDP and can obtain the voice VLAN through LLDP. The network needs to meet the following requirements:
IP phones connect to DeviceA through MAC address authentication and PCs connect to DeviceA through 802.1X authentication.
# Enable MAC address bypass authentication.
[DeviceA-authen-profile-p1] access-domain isp dot1x force
[DeviceA-authen-profile-p1] access-domain isp mac-authen force   
[DeviceA-authen-profile-p1] quit
After a phone starts, 802.1X authentication is triggered. When 802.1X authentication fails, MAC address authentication is triggered, and MAC address authentication of the phone is successful.
#
sysname DeviceA
#
vlan batch 100 200 300
#
authentication-profile name p1
 mac-access-profile m1
 dot1x-access-profile d1 
 authentication dot1x-mac-bypass
 access-domain isp dot1x force
 access-domain isp mac-authen force
#
radius-server template rd1
 radius-server shared-key cipher %+%##!!!!!!!!!"!!!!"!!!!*!!!!SKvr${[Fs.3t@/5k|BENhEu>W(3\~XG!!D;!!!!!2jp5!!!!!!A!!!!3"pK8qv!}9M#(4$jGWvQF/R[CNe/+:W^jk8HUe&W%+%#
 radius-server authentication 10.5.1.3 1812 weight 80
 radius-server accounting 10.5.1.3 1813 weight 80
#
aaa
 authentication-scheme abc
  authentication-mode radius
 accounting-scheme acco1
  accounting-mode radius
  accounting realtime 15 
 domain isp
  authentication-scheme abc
  accounting-scheme acco1
  radius-server rd1
#
dhcp enable
#
interface Vlanif100
 ip address 10.1.1.1 255.255.255.0
 dhcp select interface
#
interface Vlanif200
 ip address 10.2.1.1 255.255.255.0
 dhcp select interface
#
interface Vlanif300
 ip address 10.3.1.1 255.255.255.0
#
interface 10GE1/0/1
 port link-type hybrid
 port hybrid pvid vlan 100     
 port hybrid tagged vlan 200
 port hybrid untagged vlan 100    
 stp edged-port enable 
 lldp mdn enable txrx
 lldp tlv-enable med-tlv network-policy voice-vlan vlan 200 cos 6 dscp 60 
 voice-vlan 200 enable
 authentication-profile p1
#
interface 10GE1/0/2
 port link-type hybrid
 port hybrid pvid vlan 100 
 port hybrid tagged vlan 200    
 port hybrid untagged vlan 100 
 stp edged-port enable 
 lldp mdn enable txrx
 lldp tlv-enable med-tlv network-policy voice-vlan vlan 200 cos 6 dscp 60 
 voice-vlan 200 enable
 authentication-profile p1
#
interface 10GE1/0/3
 port link-type access
 port default vlan 300 
#
ip route-static 0.0.0.0 0.0.0.0 10.3.1.2
#
dot1x-access-profile name d1
 dot1x timer client-timeout 30
#
mac-access-profile name m1
#
return
By default, the device can trigger MAC address authentication for users upon receiving DHCP, ARP, DHCPv6, or ND packets. If a static IPv4 address is configured for a client, no DHCP or ARP packet is exchanged between the client and device. In this case, the function of triggering MAC address authentication through LLDP or CDP packets needs to be configured. During access authentication of an IP phone, LLDP or CDP negotiation packets sent from the IP phone do not carry VLAN tags. After receiving the packets, an access device adds data VLAN tags to the packets. Since the function of triggering MAC address authentication through any packet is configured, the IP phone is authenticated and goes online in the data VLAN.
In Figure 3-140, DeviceA is connected to a voice service device (IP phone) and a data service device (PC), and uses VLAN 200 to transmit voice packets and VLAN 100 to transmit data packets. The PC is connected to the IP phone, and the IP phone is connected to DeviceA. Users require high quality of the voice service; therefore, voice data flows must be transmitted with a high priority to ensure the call quality.
<HUAWEI> sysname DeviceA
[DeviceA] vlan batch 100 200 300
[DeviceA] lldp enable   
# Configure the PVID of 10GE 1/0/1 and the allowed data VLAN on 10GE 1/0/1.
[DeviceA] interface 10ge 1/0/1
[DeviceA-10GE 1/0/1] portswitch
[DeviceA-10GE 1/0/1] port link-type hybrid 
[DeviceA-10GE 1/0/1] port hybrid pvid vlan 100
[DeviceA-10GE 1/0/1] port hybrid untagged vlan 100
[DeviceA-10GE 1/0/1] quit
[DeviceA] interface vlanif 100
[DeviceA-Vlanif100] ip address 192.168.100.10 24
[DeviceA-Vlanif100] quit
[DeviceA] interface vlanif 200
[DeviceA-Vlanif200] ip address 192.168.200.10 24
[DeviceA-Vlanif200] quit
# Configure 10GE 1/0/2 connecting DeviceA to the RADIUS server as an access interface and add the interface to VLAN 300.
[DeviceA] interface 10ge 1/0/2
[DeviceA-10GE1/0/2] portswitch
[DeviceA-10GE1/0/2] port link-type access
[DeviceA-10GE1/0/2] port default vlan 300 
[DeviceA-10GE1/0/2] quit
[DeviceA] interface vlanif 300
[DeviceA-Vlanif300] ip address 192.168.1.20 24
[DeviceA-Vlanif300] quit
[DeviceA] voice-vlan mac-address 00e0-fc12-3456 mask ffff-ff00-0000   
[DeviceA] interface 10ge 1/0/1
[DeviceA-10GE 1/0/1] voice-vlan 200 enable  
[DeviceA-10GE 1/0/1] stp edged-port enable   
[DeviceA-10GE 1/0/1] port hybrid tagged vlan 200
[DeviceA-10GE 1/0/1] voice-vlan remark-mode mac-address  
[DeviceA-10GE 1/0/1] lldp tlv-enable med-tlv network-policy voice-vlan vlan 200   
[DeviceA-10GE 1/0/1] lldp mdn enable txrx   //For IP phones that use non-standard discovery protocols such as CDP, perform this step to ensure that the device can establish neighbor relationships with these IP phones.
[DeviceA-10GE 1/0/1] quit
[DeviceA] ip route-static 192.168.2.0 255.255.255.0 192.168.1.2
# Configure the MAC access profile m1 and enable the function of triggering MAC address authentication through any packet.
[DeviceA] mac-access-profile name m1
[DeviceA-mac-access-profile-m1] authentication trigger-condition dhcp arp dhcpv6 nd any-l2-packet
[DeviceA-mac-access-profile-m1] quit
The IP phone can go online and deliver clear, high-quality voice communication. The PC can also go online normally.
#
sysname DeviceA
#
vlan batch 100 200 300
#
authentication-profile name p1
 mac-access-profile m1
 access-domain example.com force
#
radius-server template rd1
 radius-server shared-key cipher %+%##!!!!!!!!!"!!!!"!!!!*!!!!SKvr${[Fs.3t@/5k|BENhEu>W(3\~XG!!D;!!!!!2jp5!!!!!!A!!!!3"pK8qv!}9M#(4$jGWvQF/R[CNe/+:W^jk8HUe&W%+%#
 radius-server authentication 192.168.2.30 1812 weight 80
 radius-server accounting 192.168.2.30 1813 weight 80
#
aaa
 authentication-scheme abc
  authentication-mode radius
 accounting-scheme scheme2
  accounting-mode radius
  accounting realtime 15
 domain example.com
  authentication-scheme abc
  accounting-scheme scheme2
  radius-server rd1
#
interface Vlanif100
 ip address 192.168.100.10 255.255.255.0
#
interface Vlanif200
 ip address 192.168.200.10 255.255.255.0
#
interface Vlanif300
 ip address 192.168.1.20 255.255.255.0
#
interface 10GE1/0/1
 port link-type hybrid
 port hybrid pvid vlan 100     
 port hybrid tagged vlan 200
 port hybrid untagged vlan 100    
 stp edged-port enable 
 authentication-profile p1
 lldp tlv-enable med-tlv network-policy voice-vlan vlan 200
 lldp mdn enable txrx
 voice-vlan 200 enable
 voice-vlan remark-mode mac-address
#
interface 10GE1/0/2
 port link-type access
 port default vlan 300
#
ip route-static 192.168.2.0 255.255.255.0 192.168.1.2
#
mac-access-profile name m1
#
voice-vlan mac-address 00e0-fc12-3456 mask ffff-ff00-0000
#
return
In Figure 3-141, to save investment costs, the customer requires that IP phones access the network through VoIP. The IP phones cannot obtain voice VLAN IDs and can only send untagged voice packets. The network needs to meet the following requirements:
| Item | Value | 
|---|---|
| Voice VLAN | VLAN 100 | 
| MAC address | 00e0-fcc7-0001 00e0-fc8f-0002 | 
| Address segment | 10.20.20.1/24 | 
| Authentication mode | MAC address authentication | 
| Item | Value | 
|---|---|
| VLAN and IP address used by SwitchA to communicate with SwitchB | VLAN 200 and 10.10.20.1/24 | 
| VLAN and IP address used by SwitchB to communicate with SwitchA | VLAN 200 and 10.10.20.2/24 | 
| IP address of SwitchA | 192.168.100.200 | 
| MAC access profile name | ipphone | 
| IP address of the RADIUS authentication and accounting server | 192.168.100.182 | 
| Port number of the RADIUS authentication server | 1812 | 
| Port number of the RADIUS accounting server | 1813 | 
| RADIUS shared key | Huawei2012 | 
# Create voice VLAN 100.
<HUAWEI> system-view 
[HUAWEI] sysname SwitchA 
[SwitchA] vlan batch 100
# Add interfaces to VLAN 100 in untagged mode.
[SwitchA] interface 10ge 1/0/1 
[SwitchA-10GE 1/0/1] port link-type hybrid 
[SwitchA-10GE 1/0/1] port hybrid untagged vlan 100  //Add the interface to voice VLAN 100 in untagged mode because packets sent by IP phones do not carry tags.
[SwitchA-10GE 1/0/1] quit 
[SwitchA] interface 10ge 1/0/2
[SwitchA-10GE1/0/2] port link-type hybrid 
[SwitchA-10GE1/0/2] port hybrid untagged vlan 100 
[SwitchA-10GE1/0/2] quit
[SwitchA] interface 10ge 1/0/1  
[SwitchA-10GE 1/0/1] voice-vlan 100 enable  //Enable the voice VLAN function on the interface.
[SwitchA-10GE 1/0/1] voice-vlan remark-mode mac-address  //Enable the interface to identify voice packets based on MAC addresses.
[SwitchA-10GE 1/0/1] port hybrid pvid vlan 100  //Set the PVID of the interface.
[SwitchA-10GE 1/0/1] quit 
[SwitchA] interface 10ge 1/0/2
[SwitchA-10GE1/0/2] voice-vlan 100 enable 
[SwitchA-10GE1/0/2] voice-vlan remark-mode mac-address 
[SwitchA-10GE1/0/2] port hybrid pvid vlan 100 
[SwitchA-10GE1/0/2] quit 
[SwitchA] voice-vlan mac-address 00e0-fcc7-0000 mask ffff-ffff-0000   
[SwitchA] voice-vlan mac-address 00e0-fc8f-0000 mask ffff-ffff-0000
# Configure the DHCP relay function on an interface.
[SwitchA] dhcp enable  //Enable DHCP globally.
[SwitchA] interface Vlanif 100 
[SwitchA-Vlanif100] ip address 10.20.20.1 255.255.255.0  //Configure an IP address for VLANIF 100.
[SwitchA-Vlanif100] dhcp select relay  //Enable the DHCP relay function on VLANIF 100.
[SwitchA-Vlanif100] dhcp relay server-ip 10.10.20.2  //Configure the DHCP server address on the DHCP relay agent.
[SwitchA-Vlanif100] quit
# Create VLANIF 200.
[SwitchA] vlan batch 200 
[SwitchA] interface Vlanif 200 
[SwitchA-Vlanif200] ip address 10.10.20.1 255.255.255.0  //Configure an IP address for VLANIF 200 for communication with SwitchB.
[SwitchA-Vlanif200] quit
# Add the uplink interface to VLAN 200.
[SwitchA] interface 10ge 1/0/3 
[SwitchA-10GE1/0/3] port link-type access 
[SwitchA-10GE1/0/3] port default vlan 200 
[SwitchA-10GE1/0/3] quit
# Configure a default static route.
[SwitchA] ip route-static 0.0.0.0 0.0.0.0 10.10.20.2  //The next-hop address of the route is the IP address of VLANIF 200 on SwitchB.
# Configure an address pool.
<HUAWEI> system-view 
[HUAWEI] sysname SwitchB 
[SwitchB] ip pool ip-phone  //Create an address pool to allocate IP addresses to IP phones.
[SwitchB-ip-pool-ip-phone] gateway-list 10.20.20.1  //Configure the gateway address of IP phones.
[SwitchB-ip-pool-ip-phone] network 10.20.20.0 mask 255.255.255.0  //Configure the network segment where IP addresses can be allocated in the address pool.
[SwitchB-ip-pool-ip-phone] quit
# Configure the DHCP server function.
[SwitchB] dhcp enable  //Enable DHCP globally. By default, DHCP is disabled.
[SwitchB] vlan batch 200 
[SwitchB] interface Vlanif 200  //Create VLANIF 200.
[SwitchB-Vlanif200] ip address 10.10.20.2 255.255.255.0  //Configure an IP address for the VLANIF interface.
[SwitchB-Vlanif200] dhcp select global  //Configure the interface to allocate IP addresses to IP phones from the global address pool.
[SwitchB-Vlanif200] quit
# Add the downlink interface to VLAN 200.
[SwitchB] interface 10ge 1/0/3 
[SwitchB-10GE1/0/3] port link-type access 
[SwitchB-10GE1/0/3] port default vlan 200 
[SwitchB-10GE1/0/3] quit
# Configure a return route.
[SwitchB] ip route-static 10.20.20.0 255.255.255.0 10.10.20.1
# Create and configure a RADIUS server template.
[SwitchA] radius-server template ipphone  //Create a RADIUS server template named ipphone.
[SwitchA-radius-ipphone] radius-server authentication 192.168.100.182 1812  //Configure the IP address and port number of the RADIUS authentication server.
[SwitchA-radius-ipphone] radius-server accounting 192.168.100.182 1813  //Configure the IP address and port number of the RADIUS accounting server.
[SwitchA-radius-ipphone] radius-server shared-key cipher YsHsjx_202206  //Configure the RADIUS shared key.
[SwitchA-radius-ipphone] quit
# Configure an authentication scheme.
[SwitchA] aaa 
[SwitchA-aaa] authentication-scheme radius  //Create an AAA authentication scheme named radius.
[SwitchA-aaa-authen-radius] authentication-mode radius  //Set the authentication mode to RADIUS.
[SwitchA-aaa-authen-radius] quit
# Create an AAA domain, and configure the RADIUS server template and authentication scheme.
[SwitchA-aaa] domain default  //Configure a domain named default.
[SwitchA-aaa-domain-default] authentication-scheme radius  //Bind the authentication scheme radius to the domain.
[SwitchA-aaa-domain-default] radius-server ipphone  //Bind the RADIUS server template ipphone to the domain.
[SwitchA-aaa-domain-default] quit 
[SwitchA-aaa] quit
# Configure an access profile.
[SwitchA] mac-access-profile name ipphone  //Create a MAC access profile named ipphone.
[SwitchA-mac-access-profile-ipphone] quit
# Configure an authentication profile.
[SwitchA] authentication-profile name ipphone  //Configure an authentication profile.
[SwitchA-authen-profile-ipphone] mac-access-profile ipphone  //Bind the MAC access profile to the authentication profile.
[SwitchA-authen-profile-ipphone] quit
# Apply the authentication profile to interfaces.
[SwitchA] interface 10ge 1/0/1  
[SwitchA-10GE 1/0/1] authentication-profile ipphone  
[SwitchA-10GE 1/0/1] quit 
[SwitchA] interface 10ge 1/0/2
[SwitchA-10GE1/0/2] authentication-profile ipphone 
[SwitchA-10GE1/0/2] quit
In the authorization settings, set the VLAN to be authorized to VLAN 100, authorize the attribute HW-Voice-Vlan (33), and set the attribute value to 1.
If the authorized VLAN is used together with the attribute HW-Voice-Vlan (33), the authorized VLAN is a voice VLAN.
[SwitchA] display access-user 
 ------------------------------------------------------------------------------  
 UserID Username     IP address       MAC            Status           
 ------------------------------------------------------------------------------  
 564   001bd4c71fa9  10.20.20.198     00e0-fcc7-1fa9 Success         
 565   0021a08f2fa8  10.20.20.199     00e0-fc8f-2fa8 Success          
 ------------------------------------------------------------------------------  
 Total: 2, printed: 2  
# 
sysname SwitchA 
# 
voice-vlan mac-address 00e0-fcc7-0000 mask ffff-ffff-0000 
voice-vlan mac-address 00e0-fc8f-0000 mask ffff-ffff-0000 
# 
vlan batch 100 200 
# 
authentication-profile name ipphone 
 mac-access-profile ipphone 
# 
dhcp enable 
# 
radius-server template ipphone 
 radius-server shared-key cipher %+%##!!!!!!!!!"!!!!"!!!!*!!!!SKvr${[Fs.3t@/5k|BENhEu>W(3\~XG!!D;!!!!!2jp5!!!!!!A!!!!3"pK8qv!}9M#(4$jGWvQF/R[CNe/+:W^jk8HUe&W%+%# 
 radius-server authentication 192.168.100.182 1812 weight 80 
 radius-server accounting 192.168.100.182 1813 weight 80 
# 
aaa 
 authentication-scheme radius 
  authentication-mode radius 
 domain default 
  authentication-scheme radius 
  radius-server ipphone 
# 
interface Vlanif100 
 ip address 10.20.20.1 255.255.255.0 
 dhcp select relay 
 dhcp relay server-ip 10.10.20.2 
# 
interface Vlanif200 
 ip address 10.10.20.1 255.255.255.0 
# 
interface 10GE1/0/1        
 port link-type hybrid 
 voice-vlan 100 enable 
 voice-vlan remark-mode mac-address 
 port hybrid pvid vlan 100 
 port hybrid untagged vlan 100 
 authentication-profile ipphone 
# 
interface  10GE 1/0/2
 port link-type hybrid 
 voice-vlan 100 enable 
 voice-vlan remark-mode mac-address 
 port hybrid pvid vlan 100 
 port hybrid untagged vlan 100 
 authentication-profile ipphone 
# 
interface 10GE 1/0/3        
 port link-type access 
 port default vlan 200 
# 
ip route-static 0.0.0.0 0.0.0.0 10.10.20.2 
# 
mac-access-profile name ipphone 
# 
return     
# 
sysname SwitchB 
# 
vlan batch 200 
# 
dhcp enable 
# 
ip pool ip-phone 
 gateway-list 10.20.20.1  
 network 10.20.20.0 mask 255.255.255.0  
# 
interface Vlanif200 
 ip address 10.10.20.2 255.255.255.0 
 dhcp select global 
# 
interface 10GE 1/0/3 
 port link-type access 
 port default vlan 200 
# 
ip route-static 10.20.20.0 255.255.255.0 10.10.20.1 
# 
return     
In Figure 3-142, terminals in an enterprise's office area are connected to the enterprise's intranet through DeviceA. To meet high security requirements of the enterprise, the enterprise wants to deploy 802.1X authentication and use a RADIUS server to authenticate terminals.
Enterprise users often connect their terminals to the network from different locations; for example, they may move their laptops to other offices for working or presentation. By default, a user cannot immediately initiate authentication or access the network after connecting to a new interface. Instead, the user can only initiate authentication on the new interface after the user status detection period expires, or after user online entries are cleared by manually disabling and enabling the interface. To improve user experience, MAC address migration needs to be configured so that users can immediately initiate authentication and access the network after connecting to a new interface.
[DeviceA] radius-server template rd1
[DeviceA-radius-rd1] radius-server authentication 192.168.2.30 1812
[DeviceA-radius-rd1] radius-server accounting 192.168.2.30 1813
[DeviceA-radius-rd1] radius-server shared-key cipher YsHsjx_202206mc@1
[DeviceA-radius-rd1] quit
[DeviceA] aaa
[DeviceA-aaa] authentication-scheme abc
[DeviceA-aaa-authen-abc] authentication-mode radius
[DeviceA-aaa-authen-abc] quit
# Configure the authentication profile p1, bind the 802.1X access profile d1 to the authentication profile, specify the domain example.com as the forcible authentication domain in the authentication profile, set the user access mode to multi-authen, and set the maximum number of access users to 100.
# Bind the authentication profile p1 to 10GE 1/0/1 and enable 802.1X authentication on 10GE 1/0/1.
[DeviceA] interface 10ge 1/0/1
[DeviceA-10GE1/0/1] authentication-profile p1
[DeviceA-10GE1/0/1] quit
[DeviceA] authentication mac-move enable vlan all
[DeviceA] authentication mac-move detect enable
#
sysname DeviceA
#
vlan batch 10 20 
#
authentication-profile name p1
 dot1x-access-profile d1
 access-domain example.com force
 authentication mode multi-authen max-user 100
#
radius-server template rd1
 radius-server authentication 192.168.2.30 1812 weight 80
 radius-server accounting 192.168.2.30 1813 weight 80
 radius-server shared-key cipher  %+%##!!!!!!!!!"!!!!"!!!!*!!!!Cd/`W03KjAwAqn64E<\TxGC_SOri<2BP\A+!!!!!2jp5!!!!!!B!!!!Oe2HMc->XMa#TLDZUaJBFJtm#XVj*E:S*|(N7`J1B:3QY!!!!!!!!!!!!!!!%+%# 
#
authentication mac-move enable vlan all
authentication mac-move detect enable
#
interface 10GE 1/0/1
 port link-type trunk
 port trunk allow-pass vlan 10
 authentication-profile p1
#
interface 10GE 1/0/2
 port link-type access
 port default vlan 20
#
interface vlanif 10
 ip address 192.168.1.10 255.255.255.0
#
interface vlanif 20
 ip address 192.168.2.10 255.255.255.0
#
aaa
 authentication-scheme abc
  authentication-mode radius
 accounting-scheme scheme2
  accounting-mode radius
  accounting realtime 15
 domain example.com
  authentication-scheme abc
  accounting-scheme scheme2
  radius-server rd1
#
dot1x-access-profile name d1
 dot1x timer client-timeout 30
#
return
In Figure 3-143, the WAC of an enterprise is connected to an AP through Switch_A (access switch). The enterprise needs to deploy the WLAN wlan-net to provide wireless network access for employees. The WAC functions as a DHCP server to assign IP addresses on the network segment 10.23.101.0/24 to STAs.
As the WLAN is open to users, there are potential security risks to enterprise information if no access control is available to the WLAN. To meet high security requirements, the enterprise needs to configure the AP as an 802.1X client before 802.1X authentication is performed on STAs. The AP can go online only after being authenticated by Switch_A. Then, the WAC performs 802.1X authentication on STAs and the RADIUS server authenticates identities of STAs.
| Item | Data | 
|---|---|
| RADIUS authentication parameters | RADIUS authentication scheme name: radius_huawei RADIUS accounting scheme name: scheme1 RADIUS server template name: radius_huawei  | 
| 802.1X access profile |  | 
| Authentication domain |  | 
| Authentication profile |  | 
| Item | Data | 
|---|---|
| RADIUS authentication parameters | RADIUS authentication scheme name: scheme1 RADIUS accounting scheme name: scheme2 RADIUS server template name: radius_huawei  | 
| 802.1X access profile |  | 
| Authentication domain |  | 
| Authentication profile |  | 
| DHCP server | The WAC functions as a DHCP server to assign IP addresses to the AP and STAs.  | 
| IP address of the WAC's source interface | VLANIF 100: 10.23.100.1/24 | 
| AP group |  | 
| Regulatory domain profile |  | 
| SSID profile |  | 
| Security profile |  | 
| VAP profile |  | 
<HUAWEI> system-view
[HUAWEI] sysname Switch_A
[Switch_A] vlan batch 100
[Switch_A] interface 10ge 1/0/1
[Switch_A-10GE1/0/1] port link-type trunk
[Switch_A-10GE1/0/1] port trunk pvid vlan 100
[Switch_A-10GE1/0/1] port trunk allow-pass vlan 100
[Switch_A-10GE1/0/1] quit
[Switch_A] interface 10ge 1/0/2
[Switch_A-10GE1/0/2] port link-type trunk
[Switch_A-10GE1/0/2] port trunk allow-pass vlan 100
[Switch_A-10GE1/0/2] quit
[Switch_A] interface vlanif 100
[Switch_A-Vlanif100] ip address 10.23.100.2 24
[Switch_A-Vlanif100] quit
[Switch_A] ip route-static 10.23.200.0 255.255.255.0 10.23.100.1
# Create and configure the RADIUS server template radius_huawei.
[Switch_A] radius-server template radius_huawei
[Switch_A-radius-radius_huawei] radius-server authentication 10.23.200.1 1812 weight 80
[Switch_A-radius-radius_huawei] radius-server accounting 10.23.200.1 1813 weight 80
[Switch_A-radius-radius_huawei] radius-server shared-key cipher YsHsjx_202206
[Switch_A-radius-radius_huawei] quit
# Create the RADIUS authentication scheme radius_huawei and RADIUS accounting scheme scheme1, and set the authentication mode and accounting mode to RADIUS.
[Switch_A] aaa  
[Switch_A-aaa] authentication-scheme radius_huawei
[Switch_A-authen-radius_huawei] authentication-mode radius
[Switch_A-authen-radius_huawei] quit
[Switch_A-aaa] accounting-scheme scheme1
[Switch_A-aaa-accounting-scheme1] accounting-mode radius
[Switch_A-aaa-accounting-scheme1] accounting realtime 15
[Switch_A-aaa-accounting-scheme1] quit
In this example, the device is connected to Agile Controller-Campus. The accounting function is not provided for accounting purposes, and is only used to maintain terminal online information through accounting packets.
The accounting realtime command is used to set the real-time accounting interval. A shorter real-time accounting interval requires higher performance of the device and RADIUS server. The real-time accounting interval needs to be set based on the user quantity.
| User Quantity | Real-Time Accounting Interval | 
|---|---|
| 1 to 99 | 3 minutes | 
| 100 to 499 | 6 minutes | 
| 500 to 999 | 12 minutes | 
| ≥ 1000 | ≥ 15 minutes | 
# Create the authentication domain huawei and bind the RADIUS authentication scheme radius_huawei, accounting scheme scheme1, and RADIUS server template radius_huawei to the domain.
[Switch_A-aaa] domain huawei
[Switch_A-aaa-domain-huawei] authentication-scheme radius_huawei
[Switch_A-aaa-domain-huawei] accounting-scheme scheme1
[Switch_A-aaa-domain-huawei] radius-server radius_huawei
[Switch_A-aaa-domain-huawei] quit
[Switch_A-aaa] quit
# Configure the 802.1X access profile d1.
By default, an 802.1X access profile uses EAP authentication. Ensure that the RADIUS server supports the EAP protocol; otherwise, it cannot process 802.1X authentication requests.
[Switch_A] dot1x-access-profile name d1
[Switch_A-dot1x-access-profile-d1] quit
# Configure the authentication profile p1, bind the 802.1X access profile d1 to the authentication profile, and configure the forcible authentication domain huawei for users using the authentication profile.
[Switch_A] authentication-profile name p1
[Switch_A-authen-profile-p1] dot1x-access-profile d1
[Switch_A-authen-profile-p1] access-domain huawei force
[Switch_A-authen-profile-p1] quit
# Bind the authentication profile p1 to 10GE 1/0/1 and enable 802.1X authentication on the interface.
[Switch_A] interface 10ge 1/0/1
[Switch_A-10GE1/0/1] authentication-profile p1
[Switch_A-10GE1/0/1] quit
# Check whether a user can pass RADIUS authentication. (The test user test and password YsHsjx_202206 have been configured on the RADIUS server.)
[Switch_A] test-aaa test YsHsjx_202206 radius-template radius_huawei 
Info: Account test succeeded.
# Create the 802.1X client profile huawei, enter the 802.1X client profile view, and set the 802.1X authentication mode to EAP-PEAP.
<HUAWEI> system-view
[HUAWEI] sysname WAC
[WAC] dot1x-client-profile name huawei
[WAC-dot1x-client-profile-huawei] eap-method eap-peap username huawei password cipher YsHsjx_202206
[WAC-dot1x-client-profile-huawei] quit
# Bind the created 802.1X client profile to an AP wired port profile so that the 802.1X client profile can take effect. Create the wired port profile wired-port1 and bind the 802.1X client profile huawei to it.
[WAC] wlan
[WAC-wlan-view] wired-port-profile name wired-port1
[WAC-wlan-wired-port-wired-port1] dot1x-client-profile huawei
[WAC-wlan-wired-port-wired-port1] quit
# Create an AP group to which the APs with the same configuration can be added. Bind the AP wired port profile wired-port1 to MultiGE0 of the AP in the AP group ap-group1.
[WAC-wlan-view] ap-group name ap-group1
[WAC-wlan-ap-group-ap-group1] wired-port-profile wired-port1 MultiGE 0
[WAC-wlan-ap-group-ap-group1] quit
# Configure the 802.1X client function on the AP.
MDCLI> diagnose wlan-nsc set-dot1x-client interface MultiGE0/0/0 peap-user-name huawei peap-password
Enter password:
Confirm password:
# Check the 802.1X client configuration on the AP.
MDCLI> diagnose
MDCLI> dot1x-client
MDCLI> query-dot1x-client-configuration profile-name multige0
  Profile name                            : multige0
  EAP Method                              : PEAP
  EAP Username                            : huawei
  EAP Password                            : ******
  SSL Policy Name                         : multige0
In this example, tunnel forwarding is used to forward service data. If direct forwarding is used, you are advised to configure port isolation on 10GE 0/0/1 that connects the AC to APs. If port isolation is not configured, unnecessary broadcast packets will be transmitted in VLANs or WLAN users on different APs will be able to communicate with each other at Layer 2.
[WAC] vlan batch 100 101 200
[WAC] interface 10ge 1/0/1
[WAC-10GE1/0/1] port link-type trunk
[WAC-10GE1/0/1] port trunk allow-pass vlan 100
[WAC-10GE1/0/1] quit
# Add the WAC's uplink interface 10GE 1/0/2 to VLAN 200.
[WAC] interface 10ge 1/0/2
[WAC-10GE1/0/2] port link-type trunk
[WAC-10GE1/0/2] port trunk allow-pass vlan 200
[WAC-10GE1/0/2] quit
[AC] interface vlanif 200
[AC-Vlanif200] ip address 10.23.200.2 24
[AC-Vlanif200] quit
# Configure the WAC as a DHCP server to assign IP addresses to APs from the IP address pool on VLANIF 100, and to assign IP addresses to STAs from the IP address pool on VLANIF 101.
[WAC] dhcp enable
[WAC] interface vlanif 100
[WAC-Vlanif100] ip address 10.23.100.1 24
[WAC-Vlanif100] dhcp select interface
[WAC-Vlanif100] dhcp server excluded-ip-address 10.23.100.2
[WAC-Vlanif100] quit
[WAC] interface vlanif 101
[WAC-Vlanif101] ip address 10.23.101.1 24
[WAC-Vlanif101] dhcp select interface
[WAC-Vlanif101] quit
[WAC] wlan
[WAC-wlan] regulatory-domain-profile name domain1
[WAC-wlan-regulate-domain-domain1] country-code cn
Warning: Modifying the country code will clear the channel and power configurations of radios, and requires the APs to be restarted if they run V200R019C10 or earlier. Continue? [Y/N]:y
[WAC-wlan-regulate-domain-domain1] quit
[WAC-wlan] ap-group name ap-group1
[WAC-wlan-ap-group-ap-group1] regulatory-domain-profile domain1
Warning: This configuration change will clear the channel and power configurations of radios, and may restart APs. Continue?[Y/N]:y
[WAC-wlan-ap-group-ap-group1] quit
[WAC-wlan] quit
[WAC] capwap dtls no-auth enable
Warning: This operation allows for device access in non-DTLS encryption mode even when DTLS is enabled and brings security risks. Af
ter the device goes online for the first time, disable this function to prevent security risks. Continue? [Y/N]:y
[WAC] capwap source interface vlanif 100
Set the DTLS PSK(contains 8-32 plain-text characters, or 128 or 148 cipher-text characters that must be a combination of at least tw
o of the following: lowercase letters a to z, uppercase letters A to Z, digits, and special characters):********
Confirm PSK:********
Set the user name for FIT APs(The value is a string of 4 to 31 characters, which can contain letters, underscores, and digits, and m
ust start with a letter):********
Set the password for FIT APs(plain-text password of 8-128 characters or cipher-text password of 128-268 characters that must be a co
mbination of at least three of the following: lowercase letters a to z, uppercase letters A to Z, digits, and special characters):******
Confirm password:********
Set the PSK of the global offline management VAP(plain-text password of 8-63 characters or cipher-text password of 128-188 character
s that must be a combination of at least two of the following: lowercase letters a to z, uppercase letters A to Z, digits, and speci
al characters):********
Confirm PSK:********
[WAC] wlan
[WAC-wlan] ap auth-mode mac-auth
[WAC-wlan] ap-id 0 ap-mac 00e0-fc12-3456
[WAC-wlan-ap-0] ap-name area_1
[WAC-wlan-ap-0] ap-group ap-group1
Warning: This operation may cause AP reset. If the country code changes, it will clear channel, power and antenna gain configuration s of the radio, Whether to continue? [Y/N]:y
[WAC-wlan-ap-0] quit
[WAC-wlan] quit
[WAC] undo capwap dtls no-auth enable
[WAC] radius-server template radius_huawei
[WAC-radius-radius_huawei] radius-server authentication 10.23.200.1 1812
[WAC-radius-radius_huawei] radius-server accounting 10.23.200.1 1813
[WAC-radius-radius_huawei] radius-server shared-key cipher YsHsjx_202206mc@1
[WAC-radius-radius_huawei] quit
[WAC] aaa
[WAC-aaa] authentication-scheme scheme1
[WAC-aaa-authen-scheme1] authentication-mode radius
[WAC-aaa-authen-scheme1] quit
[WAC-aaa] accounting-scheme scheme2
[WAC-aaa-accounting-scheme2] accounting-mode radius
[WAC-aaa-accounting-scheme2] accounting realtime 15
[WAC-aaa-accounting-scheme2] quit
In this example, the device is connected to iMaster NCE-Campus. The accounting function is not provided for accounting purposes, and is only used to maintain terminal online information through accounting packets.
[WAC-aaa] domain example.com
[WAC-aaa-domain-example.com] authentication-scheme scheme1
[WAC-aaa-domain-example.com] accounting-scheme scheme2
[WAC-aaa-domain-example.com] radius-server radius_huawei
[WAC-aaa-domain-example.com] quit
[WAC-aaa] quit
[WAC] dot1x-access-profile name d1
[WAC-dot1x-access-profile-d1] quit
[WAC] authentication-profile name p1
[WAC-authentication-profile-p1] dot1x-access-profile d1
[WAC-authentication-profile-p1] access-domain example.com force
[WAC-authentication-profile-p1] quit
[WAC] wlan
[WAC-wlan] security-profile name wlan-security
[WAC-wlan-sec-prof-wlan-security] security wpa2 dot1x aes
[WAC-wlan-sec-prof-wlan-security] quit
[WAC-wlan] ssid-profile name wlan-ssid
[WAC-wlan-ssid-prof-wlan-ssid] ssid wlan-net
[WAC-wlan-ssid-prof-wlan-ssid] quit
[WAC-wlan] vap-profile name wlan-vap
[WAC-wlan-vap-prof-wlan-vap] forward-mode tunnel
[WAC-wlan-vap-prof-wlan-vap] service-vlan vlan-id 101
[WAC-wlan-vap-prof-wlan-vap] security-profile wlan-security
[WAC-wlan-vap-prof-wlan-vap] ssid-profile wlan-ssid
[WAC-wlan-vap-prof-wlan-vap] authentication-profile p1
[WAC-wlan-vap-prof-wlan-vap] quit
# Bind the VAP profile wlan-vap to the AP group and apply the profile to radios 0 and 1 of the AP.
[WAC-wlan] ap-group name ap-group1
[WAC-wlan-ap-group-ap-group1] vap-profile wlan-vap wlan 1 radio 0
[WAC-wlan-ap-group-ap-group1] vap-profile wlan-vap wlan 1 radio 1
[WAC-wlan-ap-group-ap-group1] quit
The automatic channel and power calibration functions of radios are enabled by default. The manual channel and power settings take effect only when the two functions are disabled. The channel and power settings for the AP radios in this example are for reference only. In practice, configure the channel and power of AP radios based on the actual country code of the AP and network planning.
[WAC-wlan] ap-id 0
[WAC-wlan-ap-0] radio 0
[WAC-wlan-ap-0-radio-0] calibrate auto-channel-select disable
[WAC-wlan-ap-0-radio-0] calibrate auto-txpower-select disable
[WAC-wlan-ap-0-radio-0] channel 20mhz 6
Warning: This action may cause service interruption. Continue?[Y/N]y 
[WAC-wlan-ap-0-radio-0] eirp 127
[WAC-wlan-ap-0-radio-0] quit
[WAC-wlan-ap-0] radio 1
[WAC-wlan-ap-0-radio-1] calibrate auto-channel-select disable
[WAC-wlan-ap-0-radio-1] calibrate auto-txpower-select disable
[WAC-wlan-ap-0-radio-1] channel 80mhz 149
Warning: This action may cause service interruption. Continue?[Y/N]y 
[WAC-wlan-ap-0-radio-1] eirp 127
[WAC-wlan-ap-0-radio-1] quit
[WAC-wlan-ap-0] quit
[WAC-wlan] quit
#
 sysname Switch_A    
#                                                                                                        
vlan batch 100                                                             
#
authentication-profile name p1
 dot1x-access-profile d1
 access-domain huawei force  
#
dot1x-access-profile name d1
#
radius-server template radius_huawei
 radius-server shared-key cipher %^%#ANM|Cb!>GNo=U@V~_{E1fQ>;I2#2l(3Q%1~Z.u|R%^%# 
 radius-server authentication 10.23.200.1 1812 weight 80
 radius-server accounting 10.23.200.1 1813 weight 80                    
#
aaa        
 authentication-scheme radius_huawei
  authentication-mode radius
 accounting-scheme scheme1                                                                   
  accounting-mode radius       
  accounting realtime 15                                                                             
 domain huawei                                                                            
  authentication-scheme radius_huawei                                                                          
  accounting-scheme scheme1
  radius-server radius_huawei
#
interface 10GE1/0/1
 port link-type trunk 
 port trunk pvid vlan 100
 port trunk allow-pass vlan 100
 authentication-profile p1
#
interface 10GE1/0/2
 port link-type trunk
 port trunk allow-pass vlan 100  
#
interface vlanif 100
 ip address 10.23.100.2 255.255.255.0
#
ip route-static 10.23.200.0 255.255.255.0 10.23.100.1
#
return
#
sysname DeviceA
#
vlan batch 100 to 101 200
#
authentication-profile name p1
 dot1x-access-profile d1
 access-domain example.com force
#
dot1x-access-profile name d1
#
dhcp enable
#
dot1x-client-profile name huawei
 eap-method eap-peap username huawei password cipher %^%#f,x[/WLW|B;vh/Nbaey$V4s17cL/R06x|d$G%!q'%^%#  
#
radius-server template radius_huawei
 radius-server shared-key cipher %+%##!!!!!!!!!"!!!!"!!!!*!!!!Cd/`W03KjAwAqn64E<\TxGC_SOri<2BP\A+!!!!!2jp5!!!!!!B!!!!Oe2HMc->XMa#TLDZUaJBFJtm#XVj*E:S*|(N7`J1B:3QY!!!!!!!!!!!!!!!%+%# 
 radius-server authentication 10.23.200.1 1812 weight 80
 radius-server accounting 10.23.200.1 1813 weight 80
#
aaa
 authentication-scheme scheme1
  authentication-mode radius
 accounting-scheme scheme2
  accounting-mode radius
  accounting realtime 15
 domain example.com
  authentication-scheme scheme1
  accounting-scheme scheme2
  radius-server radius_huawei
#
interface Vlanif100
 ip address 10.23.100.1 255.255.255.0
 dhcp select interface
 dhcp server excluded-ip-address 10.23.100.2
#
interface Vlanif101
 ip address 10.23.101.1 255.255.255.0
 dhcp select interface
#
interface Vlanif200 
 ip address 10.23.200.2 255.255.255.0 
#
interface 10GE1/0/1
 port link-type trunk
 port trunk pvid vlan 100
 port trunk allow-pass vlan 100
#
interface 10GE1/0/2
 port link-type trunk
 port trunk allow-pass vlan 200
#
ip route-static 10.23.200.0 255.255.255.0 10.23.101.2 
#  
capwap source interface vlanif 100
#
wlan
 security-profile name wlan-security
  security wpa2 dot1x aes
 ssid-profile name wlan-ssid
  ssid wlan-net
 vap-profile name wlan-vap
  forward-mode tunnel
  service-vlan vlan-id 101
  ssid-profile wlan-ssid
  security-profile wlan-security
  authentication-profile p1
 regulatory-domain-profile name domain1
 ap-group name ap-group1
  regulatory-domain-profile domain1
  radio 0
   vap-profile wlan-vap wlan 1
  radio 1
   vap-profile wlan-vap wlan 1
  wired-port-profile wired-port1 MultiGE 0
 ap-id 0 type-id 1 ap-mac 00e0-fc12-3456 ap-sn 210235554710CB000042
  ap-name area_1
  ap-group ap-group1
  radio 0
   channel 20mhz 6
   eirp 127
   calibrate auto-channel-select disable  
   calibrate auto-txpower-select disable
  radio 1
   channel 80mhz 149
   eirp 127
   calibrate auto-channel-select disable 
   calibrate auto-txpower-select disable
#
return
In Figure 3-144, DeviceA functioning as a WAC in an enterprise is directly connected to an AP. The enterprise needs to deploy the WLAN named wlan-net to provide wireless network access for employees. The WAC functions as a DHCP server to assign IP addresses on the network segment 10.23.101.0/24 to STAs. To meet the enterprise's security requirements, Portal authentication needs to be deployed on the WAC. The WAC then works with the RADIUS server (integrated with the Portal server) to implement access control on STAs that attempt to access the enterprise network.
Employees need to access the network using domain names and interact with third-party software or services (such as Weibo or Microsoft). In this context, access to the software or services must be permitted before employees pass the authentication. As the services are deployed in distributed mode, one domain name corresponds to multiple IP addresses that may change. As such, you need to permit access to domain names of these services on an authentication device.
| Item | Data | 
|---|---|
| RADIUS authentication parameters |  | 
| Portal server template |  | 
| Portal access profile |  | 
| Authentication-free rule profile |  | 
| Authentication domain | The authentication domain is named example.com and is bound to:  | 
| Authentication profile |  | 
| DHCP server | The WAC functions as a DHCP server to assign IP addresses to the AP and STAs. | 
| IP address pool for the AP | 10.23.100.2 to 10.23.100.254/24 | 
| IP address pool for STAs | 10.23.101.2 to 10.23.101.254/24 | 
| IP address of the WAC's source interface | VLANIF 100: 10.23.100.1/24 | 
| AP group |  | 
| Regulatory domain profile |  | 
| SSID profile |  | 
| Security profile |  | 
| VAP profile |  | 
[DeviceA] dns resolve
[DeviceA] dns server 10.23.200.2
[DeviceA] passthrough-domain name .microsoftonline.com id 1
[DeviceA] acl number 6001
[DeviceA-acl-ucl-6001] rule permit ip destination passthrough-domain .microsoftonline.com 
[DeviceA-acl-ucl-6001] rule permit ip destination ip-address 10.23.200.2 0.0.0.0 
[DeviceA-acl-ucl-6001] quit
[DeviceA] free-rule-template name default_free_rule
[DeviceA-free-rule-default_free_rule] free-rule acl 6001
[DeviceA-free-rule-default_free_rule] quit
#
sysname DeviceA
#
vlan batch 100 to 101
#
dhcp enable
#
dns server 10.23.200.2 
#
dns resolve
#
passthrough-domain name .microsoftonline.com id 1
#
acl number 6001
 rule 5 permit ip destination passthrough-domain .microsoftonline.com
 rule 15 permit ip destination ip-address 10.23.200.2 0
#
free-rule-template name default_free_rule
 free-rule acl 6001
# 
radius-server template radius_huawei
 radius-server shared-key cipher %^%#Oc6_BMCw#9gZ2@SMVtk!PAC6>Ou*eLW/"qLp+f#$%^%#
 radius-server authentication 10.23.200.1 1812 weight 80
 radius-server accounting 10.23.200.1 1813 weight 80
#
url-template name url1  
 url http://10.23.200.1:19008/portal  
 url-parameter set device-ip 10.23.100.1 
 url-parameter device-ip ac-ip user-ipaddress uaddress user-mac umac
#
web-auth-server server-source all-interface
web-auth-server abc
 server-ip 10.23.200.1
 port 50200
 shared-key cipher %^%#4~ZXE3]6@BXu;2;aw}hA{rSb,@"L@T#e{%6G1AiD%^%#
 url-template url1
#
portal-access-profile name portal1
 web-auth-server abc
#
aaa
 authentication-scheme scheme1
  authentication-mode radius
 accounting-scheme scheme2
  accounting-mode radius
  accounting realtime 15
 domain example.com
  authentication-scheme scheme1
  accounting-scheme scheme2
  radius-server radius_huawei
#
authentication-profile name p1
 portal-access-profile portal1
 free-rule-template default_free_rule
 access-domain example.com force
#
interface Vlanif100
 ip address 10.23.100.1 255.255.255.0
 dhcp select interface
#
interface Vlanif101
 ip address 10.23.101.1 255.255.255.0
 dhcp select interface 
#
interface 10GE1/0/1
 port link-type trunk
 port trunk pvid vlan 100
 port trunk allow-pass vlan 100
#
interface 10GE1/0/2
 port link-type trunk
 port trunk allow-pass vlan 101
#
ip route-static 10.23.200.0 255.255.255.0 10.23.101.2
#  
capwap source interface vlanif 100
capwap dtls psk %+%##!!!!!!!!!"!!!!"!!!!*!!!!a4O4$.&PxRTGtHR(VaB4*(KhWT"C@K+HZ%@!!!!!2jp5!!!!!!;!!!!..AjEF}u<=!%e=F%Dz-Y[R--3yrC-*MVXf<!!!!!%+%#
#
wlan
 security-profile name wlan-security
  security open
 ssid-profile name wlan-ssid
  ssid wlan-net
 vap-profile name wlan-vap
  forward-mode tunnel
  service-vlan vlan-id 101
  ssid-profile wlan-ssid
  security-profile wlan-security
  authentication-profile p1
 regulatory-domain-profile name domain1
 ap-group name ap-group1
  regulatory-domain-profile domain1
  radio 0
   vap-profile wlan-vap wlan 1
  radio 1
   vap-profile wlan-vap wlan 1
 ap-id 0 type-id 1 ap-mac 00e0-fc12-3456 ap-sn 210235554710CB000042
  ap-name area_1
  ap-group ap-group1
  radio 0
   channel 20mhz 6
   eirp 127
   calibrate auto-channel-select disable  
   calibrate auto-txpower-select disable
  radio 1
   channel 80mhz 149
   eirp 127
   calibrate auto-channel-select disable 
   calibrate auto-txpower-select disable
#
return
On the network shown in Figure 3-145, users in a company's guest area access the company's intranet through DeviceA. Unauthorized access to the intranet will damage the company's service system and cause leakage of key information. Therefore, the administrator requires that DeviceA should control users' network access rights to ensure intranet security. Because guests move frequently, Portal authentication is used and the RADIUS server authenticates them.
[DeviceA] dns resolve
[DeviceA] dns server 192.168.2.100
[DeviceA] ucl-group name group1
[DeviceA-ucl-group-group1] domain domain-name .microsoftonline.com //Configure a domain name in a static UCL group.
[DeviceA-ucl-group-group1] quit
[DeviceA] acl number 6001 
[DeviceA-acl-ucl-6001] rule 1 permit ip destination ucl-group name group1 //Configure an ACL rule to permit packets destined for the domain name in the static UCL group.
[DeviceA-acl-ucl-6001] rule permit ip destination ip-address 192.168.2.100 0.0.0.0 //Configure an ACL rule to permit traffic destined for the DNS server.
[DeviceA-acl-ucl-6001] quit
[DeviceA] free-rule-template name default_free_rule
[DeviceA-free-rule-default_free_rule] free-rule acl 6001 //Configure an authentication-free rule to permit access to domain names.
[DeviceA-free-rule-default_free_rule] quit
[DeviceA] dns snooping ttl delay-time 5760
[DeviceA] dns snooping server-ip-address 192.168.2.100
[DeviceA] interface 10ge 1/0/2
[DeviceA-10GE1/0/2] dns snooping enable //Enable DNS snooping on an uplink interface.
[DeviceA-10GE1/0/2] quit
#
sysname DeviceA
#
vlan batch 10 20
#
dns server 192.168.2.100 
#
dns resolve
#
dns snooping ttl delay-time 5760
dns snooping server-ip-address 192.168.2.100
#
ucl-group 1 name group1
 domain domain-name .microsoftonline.com
#
acl number 6001
 rule 5 permit ip destination ucl-group name group1
 rule 10 permit ip destination ip-address 192.168.2.100 0
#
free-rule-template name default_free_rule 
 free-rule acl 6001
#
aaa
 authentication-scheme abc    
  authentication-mode radius    
 accounting-scheme scheme2
  accounting-mode radius
  accounting realtime 15
 domain example.com            
  authentication-scheme abc     
  accounting-scheme scheme2
  radius-server rd1      
#  
radius-server template rd1 
 radius-server shared-key cipher %+%##!!!!!!!!!"!!!!"!!!!*!!!!SKvr${[Fs.3t@/5k|BENhEu>W(3\~XG!!D;!!!!!2jp5!!!!!!A!!!!3"pK8qv!}9M#(4$jGWvQF/R[CNe/+:W^jk8HUe&W%+%#
 radius-server authentication 192.168.2.30 1812 weight 80
 radius-server accounting 192.168.2.30 1813 weight 80
#
web-auth-server server-source all-interface
web-auth-server abc
 server-ip 192.168.2.30
 port 50200
 shared-key cipher %^%#4~ZXE3]6@BXu;2;aw}hA{rSb,@"L@T#e{%6G1AiD%^%#
 url http://192.168.2.30:8445/portal
#
portal-access-profile name portal1
 web-auth-server abc
#
authentication-profile name p1
 portal-access-profile portal1
 access-domain example.com force
#
interface Vlanif10
 ip address 192.168.1.10 255.255.255.0
#
interface Vlanif20
 ip address 192.168.2.10 255.255.255.0
#
interface 10GE1/0/1
 port link-type trunk
 port trunk allow-pass vlan 10
 authentication-profile p1
#
interface 10GE1/0/2
 port link-type access
 port default vlan 20
 dns snooping enable
#
return 
In Figure 3-146, user PCs access the network through DeviceE, and DeviceE connects to DeviceA and DeviceB using M-LAG. When both DeviceA and DeviceB work properly, traffic is load balanced to them. In addition, services will not be affected if any of the two devices fails, ensuring high service reliability.
In addition, to meet high security requirements, 802.1X authentication is used for access control of user PCs through the RADIUS server, and the authentication point is deployed on M-LAG member devices (DeviceA and DeviceB).
In this example, interfaces 1, 2, 3, 4, 5, 6, and 7 on DeviceA represent 10GE 1/0/1, 10GE 1/0/2, 10GE 1/0/3, 10GE 1/0/4, 10GE 1/0/5, 10GE 1/0/6, and MEth 0/0/0, respectively.
Interfaces 1, 2, 3, 4, 5, 6, and 7 on DeviceB represent 10GE 1/0/1, 10GE 1/0/2, 10GE 1/0/3, 10GE 1/0/4, 10GE 1/0/5, 10GE 1/0/6, and MEth 0/0/0, respectively.
Interface 1 and interface 2 on DeviceC represent 10GE 1/0/1 and 10GE 1/0/2, respectively.
Interface 1 and interface 2 on DeviceD represent 10GE 1/0/1 and 10GE 1/0/2, respectively.
On DeviceA and DeviceB, Eth-Trunk 0 serves as a peer-link interface, M-LAG interface Eth-Trunk 1 connects to downlink terminals, and Eth-Trunk 2 connects to the uplink server.
Configure DeviceA and DeviceB as root bridges with the same bridge MAC address so that both devices function as root bridges on the Layer 2 network.
Configure an IP address for the management interface on both DeviceA and DeviceB to ensure Layer 3 connectivity for heartbeat packet exchange between them.
Configure M-LAG on DeviceA and DeviceB so that terminals can be dual-homed to DeviceA and DeviceB.
Configure RADIUS authentication and 802.1X authentication on DeviceA and DeviceB, enable NAC for M-LAG globally, and enable NAC authentication on M-LAG interfaces.
When configuring 802.1X authentication in an M-LAG scenario:
If the downstream device dual-homed to the M-LAG member devices is a switching device, root protection must be configured.
# Configure DeviceA.
<HUAWEI> system-view
[HUAWEI] sysname DeviceA
[DeviceA] stp root primary
[DeviceA] stp bridge-address 00e0-fc12-3458  //Configure the bridge MAC address of the root bridge (MAC address of the M-LAG master device).
[DeviceA] interface eth-trunk 1
[DeviceA-Eth-Trunk1] trunkport 10ge 1/0/2
[DeviceA-Eth-Trunk1] trunkport 10ge 1/0/5
[DeviceA-Eth-Trunk1] stp edged-port enable
[DeviceA-Eth-Trunk1] quit
# Configure DeviceB.
<HUAWEI> system-view
[HUAWEI] sysname DeviceB
[DeviceB] stp root primary
[DeviceB] stp bridge-address 00e0-fc12-3458   //Configure the bridge MAC address of the root bridge.
[DeviceB] interface eth-trunk 1
[DeviceB-Eth-Trunk1] trunkport 10ge 1/0/2
[DeviceB-Eth-Trunk1] trunkport 10ge 1/0/5
[DeviceB-Eth-Trunk1] stp edged-port enable
[DeviceB-Eth-Trunk1] quit
Ensure that DeviceA and DeviceB can communicate at Layer 3 through their management interfaces.
[DeviceA] interface meth 0/0/0
[DeviceA-MEth0/0/0] ip address 10.1.1.1 24
[DeviceA-MEth0/0/0] quit
[DeviceB] interface meth 0/0/0
[DeviceB-MEth0/0/0] ip address 10.1.1.2 24
[DeviceB-MEth0/0/0] quit
[DeviceA] dfs-group 1
[DeviceA-dfs-group-1] dual-active detection source ip 10.1.1.1 peer 10.1.1.2
[DeviceA-dfs-group-1] priority 150
[DeviceA-dfs-group-1] authentication-mode hmac-sha256 password YsHsjx_202206
[DeviceA-dfs-group-1] dfs-group state switchover disable   //Disable the proactive master/backup status switchback function of a DFS group. This step is required when both M-LAG and NAC are configured.
[DeviceA-dfs-group-1] quit
[DeviceB] dfs-group 1
[DeviceB-dfs-group-1] dual-active detection source ip 10.1.1.2 peer 10.1.1.1
[DeviceB-dfs-group-1] priority 120
[DeviceB-dfs-group-1] authentication-mode hmac-sha256 password YsHsjx_202206
[DeviceB-dfs-group-1] dfs-group state switchover disable   //Disable the proactive master/backup status switchback function of a DFS group. This step is required when both M-LAG and NAC are configured.
[DeviceB-dfs-group-1] quit
[DeviceA] interface eth-trunk 0
[DeviceA-Eth-Trunk0] mode lacp-static
[DeviceA-Eth-Trunk0] trunkport 10ge 1/0/3
[DeviceA-Eth-Trunk0] trunkport 10ge 1/0/4
[DeviceA-Eth-Trunk0] undo stp enable
[DeviceA-Eth-Trunk0] peer-link 1
[DeviceA-Eth-Trunk0] quit
[DeviceB] interface eth-trunk 0
[DeviceB-Eth-Trunk0] mode lacp-static
[DeviceB-Eth-Trunk0] trunkport 10ge 1/0/3
[DeviceB-Eth-Trunk0] trunkport 10ge 1/0/4
[DeviceB-Eth-Trunk0] undo stp enable
[DeviceB-Eth-Trunk0] peer-link 1
[DeviceB-Eth-Trunk0] quit
The uplink interfaces that connect the terminals to DeviceA and DeviceB must be added to an Eth-Trunk interface, and the working mode of the Eth-Trunk interface must be the same as that of the Eth-Trunk interfaces on both devices. In this example, the Eth-Trunk interfaces on both devices are configured to work in static LACP mode.
[DeviceA] vlan batch 10 11
[DeviceA] interface eth-trunk 1
[DeviceA-Eth-Trunk1] mode lacp-static
[DeviceA-Eth-Trunk1] port link-type access
[DeviceA-Eth-Trunk1] port default vlan 11
[DeviceA-Eth-Trunk1] dfs-group 1 m-lag 1
[DeviceA-Eth-Trunk1] quit
[DeviceB] vlan batch 10 11
[DeviceB] interface eth-trunk 1
[DeviceB-Eth-Trunk1] mode lacp-static
[DeviceB-Eth-Trunk1] port link-type access
[DeviceB-Eth-Trunk1] port default vlan 11
[DeviceB-Eth-Trunk1] dfs-group 1 m-lag 1
[DeviceB-Eth-Trunk1] quit
[DeviceA] interface eth-trunk 2
[DeviceA-Eth-Trunk2] mode lacp-static
[DeviceA-Eth-Trunk2] port link-type trunk
[DeviceA-Eth-Trunk2] port trunk allow-pass vlan 10 11
[DeviceA-Eth-Trunk2] trunkport 10ge 1/0/1
[DeviceA-Eth-Trunk2] trunkport 10ge 1/0/6
[DeviceA-Eth-Trunk2] quit
[DeviceB] interface eth-trunk 2
[DeviceB-Eth-Trunk2] mode lacp-static
[DeviceB-Eth-Trunk2] port link-type trunk
[DeviceB-Eth-Trunk2] port trunk allow-pass vlan 10 11
[DeviceB-Eth-Trunk2] trunkport 10ge 1/0/1
[DeviceB-Eth-Trunk2] trunkport 10ge 1/0/6
[DeviceB-Eth-Trunk2] quit
# Configure DeviceC.
<HUAWEI> system-view
[HUAWEI] sysname DeviceC
[DeviceC] vlan batch 10 11
[DeviceC] interface eth-trunk 2
[DeviceC-Eth-Trunk2] mode lacp-static
[DeviceC-Eth-Trunk2] port link-type trunk
[DeviceC-Eth-Trunk2] port trunk allow-pass vlan 10 11
[DeviceC-Eth-Trunk2] trunkport 10ge 1/0/1
[DeviceC-Eth-Trunk2] trunkport 10ge 1/0/2
[DeviceC-Eth-Trunk2] quit
# Configure DeviceD.
<HUAWEI> system-view
[HUAWEI] sysname DeviceD
[DeviceD] vlan batch 10 11
[DeviceD] interface eth-trunk 2
[DeviceD-Eth-Trunk2] mode lacp-static
[DeviceD-Eth-Trunk2] port link-type trunk
[DeviceD-Eth-Trunk2] port trunk allow-pass vlan 10 11
[DeviceD-Eth-Trunk2] trunkport 10ge 1/0/1
[DeviceD-Eth-Trunk2] trunkport 10ge 1/0/2
[DeviceD-Eth-Trunk2] quit
<HUAWEI> system-view
[HUAWEI] sysname DeviceE
[DeviceE] l2protocol-tunnel user-defined-protocol 802.1X protocol-mac 0180-c200-0003 group-mac 0100-0000-0002
[DeviceE] interface eth-trunk 1
[DeviceE-Eth-Trunk1] l2protocol-tunnel user-defined-protocol 802.1X enable
[DeviceE-Eth-Trunk1] port link-type access
[DeviceE-Eth-Trunk1] port default vlan 11
[DeviceE-Eth-Trunk1] quit
[DeviceE] interface 10ge 1/0/2
[DeviceE-10GE1/0/2] l2protocol-tunnel user-defined-protocol 802.1X enable
[DeviceE-10GE1/0/2] port link-type access
[DeviceE-10GE1/0/2] port default vlan 11
[DeviceE-10GE1/0/2] quit
# Configure the source address for communicating with the RADIUS server. When both M-LAG and NAC are configured, DeviceA and DeviceB must have the same source address configured.
[DeviceA] interface vlanif 10
[DeviceA-Vlanif10] ip address 192.168.1.1 24
[DeviceA-Vlanif10] mac-address 0000-0000-0011
[DeviceB] interface vlanif 10
[DeviceB-Vlanif10]ip address 192.168.1.1 24
[DeviceB-Vlanif10] mac-address 0000-0000-0011
Create and configure the RADIUS server template rd1.
[DeviceA] radius-server template rd1
[DeviceA-radius-rd1] radius-server authentication 192.168.10.1 1812 source ip-address 192.168.1.1
[DeviceA-radius-rd1] radius-server accounting 192.168.10.1 1813 source ip-address 192.168.1.1
[DeviceA-radius-rd1] radius-server shared-key cipher YsHsjx_202206mc@1
[DeviceA-radius-rd1] quit
[DeviceA] access-user m-lag enable    //Enable NAC in an M-LAG scenario.
[DeviceA] interface eth-trunk 1
[DeviceA-Eth-Trunk1] authentication-profile p1
[DeviceA-Eth-Trunk1] quit
#
sysname DeviceA
#
dfs-group 1
 priority 150
 dual-active detection source ip 10.1.1.1 peer 10.1.1.2
 authentication-mode hmac-sha256 password %+%##!!!!!!!!!"!!!!"!!!!*!!!!C+tR0CW9x*eB&pWp`t),Azgw-h\o8#4LZPD!!!!!!!!!!!!!!!9!!!!>fwJ)I0E{=:%,*,XRhbH&t0MCy_8=7!!!!!!!!!!%+%#
 dfs-group state switchover disable
#
vlan batch 10 to 11
#
access-user m-lag enable
#
radius-server template rd1
 radius-server shared-key cipher %+%##!!!!!!!!!"!!!!"!!!!*!!!!Cd/`W03KjAwAqn64E<\TxGC_SOri<2BP\A+!!!!!2jp5!!!!!!B!!!!Oe2HMc->XMa#TLDZUaJBFJtm#XVj*E:S*|(N7`J1B:3QY!!!!!!!!!!!!!!!%+%# 
 radius-server authentication 192.168.10.1 1812 source ip-address 192.168.1.1 weight 80
 radius-server accounting 192.168.10.1 1813 source ip-address 192.168.1.1 weight 80
#
aaa
 authentication-scheme abc
  authentication-mode radius
 accounting-scheme scheme2
  accounting-mode radius
  accounting realtime 15
 domain example.com
  authentication-scheme abc
  accounting-scheme scheme2
  radius-server rd1
#
dot1x-access-profile name d1
 dot1x timer client-timeout 30
#
authentication-profile name p1
 dot1x-access-profile d1
 access-domain example.com force
 authentication mode multi-authen max-user 100
#
stp bridge-address 00e0-fc12-3458
stp instance 0 root primary
#
interface MEth0/0/0
 ip address 10.1.1.1 255.255.255.0
#
interface Vlanif10
 ip address 192.168.1.1 255.255.255.0
 mac-address 0000-0000-0011
#
interface Eth-Trunk0
 mode lacp-static
 stp disable
 peer-link 1
#
interface Eth-Trunk1
 port link-type access
 port default vlan 11
 stp edged-port enable
 mode lacp-static
 dfs-group 1 m-lag 1
 authentication-profile p1
#
interface Eth-Trunk2
 port link-type trunk
 port trunk allow-pass vlan 10 11
 mode lacp-static 
#
interface 10GE1/0/1
 eth-trunk 2
#
interface 10GE1/0/2
 eth-trunk 1
#
interface 10GE1/0/3
 eth-trunk 0
#
interface 10GE1/0/4
 eth-trunk 0
#
interface 10GE1/0/5
 eth-trunk 1
#
interface 10GE1/0/6
 eth-trunk 2
#
return
DeviceB
#
sysname DeviceB
#
dfs-group 1
 priority 120
 dual-active detection source ip 10.1.1.2 peer 10.1.1.1
 authentication-mode hmac-sha256 password %+%##!!!!!!!!!"!!!!"!!!!*!!!!=I9f8>C{!P_bhB31@7r-=jrS8c|_"(Bn~#=!!!!!!!!!!!!!!!9!!!!kx-6@.tGA(wAt/IQXl6>[g{6YlOi9$!!!!!!!!!!%+%#
 dfs-group state switchover disable
#
vlan batch 10 to 11
#
access-user m-lag enable
#
radius-server template rd1
 radius-server shared-key cipher %+%##!!!!!!!!!"!!!!"!!!!*!!!!Cd/`W03KjAwAqn64E<\TxGC_SOri<2BP\A+!!!!!2jp5!!!!!!B!!!!Oe2HMc->XMa#TLDZUaJBFJtm#XVj*E:S*|(N7`J1B:3QY!!!!!!!!!!!!!!!%+%# 
 radius-server authentication 192.168.10.1 1812 source ip-address 192.168.1.1 weight 80
 radius-server accounting 192.168.10.1 1813 source ip-address 192.168.1.1 weight 80
#
aaa
 authentication-scheme abc
  authentication-mode radius
 accounting-scheme scheme2
  accounting-mode radius
  accounting realtime 15
 domain example.com
  authentication-scheme abc
  accounting-scheme scheme2
  radius-server rd1
#
dot1x-access-profile name d1
 dot1x timer client-timeout 30
#
authentication-profile name p1
 dot1x-access-profile d1
 access-domain example.com force
 authentication mode multi-authen max-user 100
#
stp bridge-address 00e0-fc12-3458
stp instance 0 root primary
#
interface MEth0/0/0
 ip address 10.1.1.2 255.255.255.0
#
interface Vlanif10
 ip address 192.168.1.1.1 255.255.255.0
 mac-address 0000-0000-0011
#
interface Eth-Trunk0
 mode lacp-static
 stp disable
 peer-link 1
#
interface Eth-Trunk1
 port link-type access
 port default vlan 11
 stp edged-port enable
 mode lacp-static
 dfs-group 1 m-lag 1
 authentication-profile p1
#
interface Eth-Trunk2
 port link-type trunk
 port trunk allow-pass vlan 10 11
 mode lacp-static
#
interface 10GE1/0/1
 eth-trunk 2
#
interface 10GE1/0/2
 eth-trunk 1
#
interface 10GE1/0/3
 eth-trunk 0
#
interface 10GE1/0/4
 eth-trunk 0
#
interface 10GE1/0/5
 eth-trunk 1
#
interface 10GE1/0/6
 eth-trunk 2
#
return
DeviceC
#
sysname DeviceC
#
vlan batch 10 to 11
#
interface Eth-Trunk2
 port link-type trunk
 port trunk allow-pass vlan 10 11
 mode lacp-static
#
interface 10GE1/0/1
 eth-trunk 2
#
interface 10GE1/0/2
 eth-trunk 2
#
return
DeviceD
#
sysname DeviceD
#
vlan batch 10 to 11
#
interface Eth-Trunk2
 port link-type trunk
 port trunk allow-pass vlan 10 11
 mode lacp-static
#
interface 10GE1/0/1
 eth-trunk 2
#
interface 10GE1/0/2
 eth-trunk 2
#
return
#
sysname DeviceE
#
vlan batch 11 
#
l2protocol-tunnel user-defined-protocol 802.1X protocol-mac 0180-c200-0003 group-mac 0100-0000-0002
#
interface Eth-Trunk2
 l2protocol-tunnel user-defined-protocol 802.1X enable
 port link-type access
 port default vlan 11
#
interface 10GE 1/0/2
 l2protocol-tunnel user-defined-protocol 802.1X enable
 port link-type access
 port default vlan 11
#
return
In Figure 3-147, user PCs are connected to the network through DeviceD. DeviceA and DeviceB set up an M-LAG, are connected to DeviceC in the uplink direction, and are connected to DeviceD in the downlink direction.
In addition, to meet high security requirements, Portal authentication is used for access control of user PCs through the RADIUS server, and the authentication point is deployed on M-LAG member devices (DeviceA and DeviceB).
In this example, interfaces 1, 2, 3, 4, 5, and 6 on DeviceA represent 10GE 1/0/1, 10GE 1/0/2, 10GE 1/0/3, 10GE 1/0/4, 10GE 1/0/5, and 10GE 1/0/6, respectively.
Interfaces 1, 2, 3, 4, 5, and 6 on DeviceB represent 10GE 1/0/1, 10GE 1/0/2, 10GE 1/0/3, 10GE 1/0/4, 10GE 1/0/5, and 10GE 1/0/6, respectively.
On DeviceA and DeviceB, Eth-Trunk 0 serves as a peer-link interface, M-LAG interface Eth-Trunk 1 connects to downlink terminals and is added to VLAN 11, and M-LAG interface Eth-Trunk 2 connects to the uplink server and is added to VLAN 10.
When configuring Portal authentication in an M-LAG scenario:
<HUAWEI> system-view
[HUAWEI] sysname DeviceA
[DeviceA] stp mode rstp
[DeviceA] stp v-stp enable
<HUAWEI> system-view
[HUAWEI] sysname DeviceB
[DeviceB] stp mode rstp
[DeviceB] stp v-stp enable
[DeviceA] interface eth-trunk 0
[DeviceA-Eth-Trunk0] mode lacp-static
[DeviceA-Eth-Trunk0] trunkport 10ge 1/0/3
[DeviceA-Eth-Trunk0] trunkport 10ge 1/0/4
[DeviceA-Eth-Trunk0] peer-link 1
[DeviceA-Eth-Trunk0] quit
[DeviceB] interface eth-trunk 0
[DeviceB-Eth-Trunk0] mode lacp-static
[DeviceB-Eth-Trunk0] trunkport 10ge 1/0/3
[DeviceB-Eth-Trunk0] trunkport 10ge 1/0/4
[DeviceB-Eth-Trunk0] peer-link 1
[DeviceB-Eth-Trunk0] quit
[DeviceA] vlan batch 11
[DeviceA] interface eth-trunk 1
[DeviceA-Eth-Trunk1] trunkport 10ge 1/0/2
[DeviceA-Eth-Trunk1] trunkport 10ge 1/0/5
[DeviceA-Eth-Trunk1] mode lacp-static
[DeviceA-Eth-Trunk1] port link-type access
[DeviceA-Eth-Trunk1] port default vlan 11
[DeviceA-Eth-Trunk1] dfs-group 1 m-lag 1
[DeviceA-Eth-Trunk1] quit
[DeviceB] vlan batch 11
[DeviceB] interface eth-trunk 1
[DeviceB-Eth-Trunk1] trunkport 10ge 1/0/2
[DeviceB-Eth-Trunk1] trunkport 10ge 1/0/5
[DeviceB-Eth-Trunk1] mode lacp-static
[DeviceB-Eth-Trunk1] port link-type access
[DeviceB-Eth-Trunk1] port default vlan 11
[DeviceB-Eth-Trunk1] dfs-group 1 m-lag 1
[DeviceB-Eth-Trunk1] quit
DeviceA and DeviceB must be configured with the same virtual IP address and virtual MAC address.
[DeviceA] interface vlanif 11
[DeviceA-Vlanif11] ip address 10.2.1.1 24
[DeviceA-Vlanif11] mac-address 0000-5e00-0101
[DeviceA-Vlanif11] quit
[DeviceB] interface vlanif 11
[DeviceB-Vlanif11] ip address 10.2.1.1 24
[DeviceB-Vlanif11] mac-address 0000-5e00-0101
[DeviceB-Vlanif11] quit
[DeviceA] vlan batch 10
[DeviceA] interface eth-trunk 2
[DeviceA-Eth-Trunk2] mode lacp-static
[DeviceA-Eth-Trunk2] port link-type trunk
[DeviceA-Eth-Trunk2] port trunk allow-pass vlan 10
[DeviceA-Eth-Trunk2] trunkport 10ge 1/0/1
[DeviceA-Eth-Trunk2] trunkport 10ge 1/0/6
[DeviceA-Eth-Trunk2] dfs-group 1 m-lag 2
[DeviceA-Eth-Trunk2] quit
[DeviceB] vlan batch 10
[DeviceB] interface eth-trunk 2
[DeviceB-Eth-Trunk2] mode lacp-static
[DeviceB-Eth-Trunk2] port link-type trunk
[DeviceB-Eth-Trunk2] port trunk allow-pass vlan 10
[DeviceB-Eth-Trunk2] trunkport 10ge 1/0/1
[DeviceB-Eth-Trunk2] trunkport 10ge 1/0/6
[DeviceB-Eth-Trunk2] dfs-group 1 m-lag 2
[DeviceB-Eth-Trunk2] quit
# Configure the source address for communicating with the RADIUS or Portal server. When both M-LAG and NAC are configured, it is recommended that DeviceA and DeviceB have the same source address configured.
[DeviceA] url-template name url1 
[DeviceA-url-template-url1] url http://192.168.10.1:19008/portal 
[DeviceA-url-template-url1] url-parameter set device-ip 192.168.1.1
[DeviceA-url-template-url1] url-parameter device-ip ac-ip user-ipaddress uaddress user-mac umac 
[DeviceA-url-template-url1] quit
[DeviceA] web-auth-server server-source all-interface
[DeviceA] web-auth-server abc
[DeviceA-web-auth-server-abc] server-ip 192.168.10.1
[DeviceA-web-auth-server-abc] source-ip 192.168.1.1
[DeviceA-web-auth-server-abc] shared-key cipher YsHsjx_202206
[DeviceA-web-auth-server-abc] port 50200
[DeviceA-web-auth-server-abc] url-template url1
[DeviceA-web-auth-server-abc] quit
#
sysname DeviceA
#
dfs-group 1
 priority 150
 dual-active detection source ip 10.1.1.1 peer 10.1.1.2
 authentication-mode hmac-sha256 password %+%##!!!!!!!!!"!!!!"!!!!*!!!!C+tR0CW9x*eB&pWp`t),Azgw-h\o8#4LZPD!!!!!!!!!!!!!!!9!!!!>fwJ)I0E{=:%,*,XRhbH&t0MCy_8=7!!!!!!!!!!%+%#
 dfs-group state switchover disable
#
vlan batch 10 to 11
#
stp mode rstp 
stp v-stp enable
#
access-user m-lag enable
#
radius-server template rd1
 radius-server shared-key cipher %+%##!!!!!!!!!"!!!!"!!!!*!!!!Cd/`W03KjAwAqn64E<\TxGC_SOri<2BP\A+!!!!!2jp5!!!!!!B!!!!Oe2HMc->XMa#TLDZUaJBFJtm#XVj*E:S*|(N7`J1B:3QY!!!!!!!!!!!!!!!%+%# 
 radius-server authentication 192.168.10.1 1812 source ip-address 192.168.1.1 weight 80
 radius-server accounting 192.168.10.1 1813 source ip-address 192.168.1.1 weight 80
#
aaa
 authentication-scheme abc
  authentication-mode radius
 accounting-scheme scheme2
  accounting-mode radius
  accounting realtime 15
 domain example.com
  authentication-scheme abc
  accounting-scheme scheme2
  radius-server rd1
#
url-template name url1 
 url http://192.168.10.1:19008/portal 
 url-parameter set device-ip 192.168.1.1
 url-parameter device-ip ac-ip user-ipaddress uaddress user-mac umac 
#
web-auth-server server-source all-interface
web-auth-server abc
 server-ip 192.168.10.1
 source-ip 192.168.1.1
 port 50200
 shared-key cipher %^%#4~ZXE3]6@BXu;2;aw}hA{rSb,@"L@T#e{%6G1AiD%^%#
 url-template url1
#
portal-access-profile name portal1
 web-auth-server abc
#
authentication-profile name p1
 portal-access-profile portal1
 access-domain example.com force
#
interface MEth0/0/0
 ip address 10.1.1.1 255.255.255.0
#
interface Vlanif10
 ip address 192.168.1.1 255.255.255.0
 mac-address 0000-0000-0011
#
interface Vlanif11
 ip address 10.2.1.1 255.255.255.0 
 mac-address 0000-5e00-0101
#
interface Eth-Trunk0
 mode lacp-static
 peer-link 1
#
interface Eth-Trunk1
 port link-type access
 port default vlan 11
 mode lacp-static
 dfs-group 1 m-lag 1
 authentication-profile p1
#
interface Eth-Trunk2
 port link-type trunk
 port trunk allow-pass vlan 10 
 mode lacp-static 
 dfs-group 1 m-lag 2
#
interface 10GE1/0/1
 eth-trunk 2
#
interface 10GE1/0/2
 eth-trunk 1
#
interface 10GE1/0/3
 eth-trunk 0
#
interface 10GE1/0/4
 eth-trunk 0
#
interface 10GE1/0/5
 eth-trunk 1
#
interface 10GE1/0/6
 eth-trunk 2
#
return
#
sysname DeviceB
#
dfs-group 1
 priority 120
 dual-active detection source ip 10.1.1.2 peer 10.1.1.1
 authentication-mode hmac-sha256 password %+%##!!!!!!!!!"!!!!"!!!!*!!!!=I9f8>C{!P_bhB31@7r-=jrS8c|_"(Bn~#=!!!!!!!!!!!!!!!9!!!!kx-6@.tGA(wAt/IQXl6>[g{6YlOi9$!!!!!!!!!!%+%#
 dfs-group state switchover disable
#
vlan batch 10 to 11
#
stp mode rstp 
stp v-stp enable
#
access-user m-lag enable
#
radius-server template rd1
 radius-server shared-key cipher %+%##!!!!!!!!!"!!!!"!!!!*!!!!Cd/`W03KjAwAqn64E<\TxGC_SOri<2BP\A+!!!!!2jp5!!!!!!B!!!!Oe2HMc->XMa#TLDZUaJBFJtm#XVj*E:S*|(N7`J1B:3QY!!!!!!!!!!!!!!!%+%# 
 radius-server authentication 192.168.10.1 1812 source ip-address 192.168.1.1 weight 80
 radius-server accounting 192.168.10.1 1813 source ip-address 192.168.1.1 weight 80
#
aaa
 authentication-scheme abc
  authentication-mode radius
 accounting-scheme scheme2
  accounting-mode radius
  accounting realtime 15
 domain example.com
  authentication-scheme abc
  accounting-scheme scheme2
  radius-server rd1
#
url-template name url1 
 url http://192.168.10.1:19008/portal 
 url-parameter set device-ip 192.168.1.1
 url-parameter device-ip ac-ip user-ipaddress uaddress user-mac umac 
#
web-auth-server server-source all-interface
web-auth-server abc
 server-ip 192.168.10.1
 source-ip 192.168.1.1
 port 50200
 shared-key cipher %^%#4~ZXE3]6@BXu;2;aw}hA{rSb,@"L@T#e{%6G1AiD%^%#
 url-template url1
#
portal-access-profile name portal1
 web-auth-server abc
#
authentication-profile name p1
 portal-access-profile portal1
 access-domain example.com force
#
interface MEth0/0/0
 ip address 10.1.1.2 255.255.255.0
#
interface Vlanif10
 ip address 192.168.1.1.1 255.255.255.0
 mac-address 0000-0000-0011
#
interface Vlanif11 
 ip address 10.2.1.1 255.255.255.0 
 mac-address 0000-5e00-0101  
#
interface Eth-Trunk0
 mode lacp-static
 peer-link 1
#
interface Eth-Trunk1
 port link-type access
 port default vlan 11
 mode lacp-static
 dfs-group 1 m-lag 1
 authentication-profile p1
#
interface Eth-Trunk2
 port link-type trunk
 port trunk allow-pass vlan 10 
 mode lacp-static
 dfs-group 1 m-lag 2
#
interface 10GE1/0/1
 eth-trunk 2
#
interface 10GE1/0/2
 eth-trunk 1
#
interface 10GE1/0/3
 eth-trunk 0
#
interface 10GE1/0/4
 eth-trunk 0
#
interface 10GE1/0/5
 eth-trunk 1
#
interface 10GE1/0/6
 eth-trunk 2
#
return
Select the content with the mouse pointer to quickly report the problem.