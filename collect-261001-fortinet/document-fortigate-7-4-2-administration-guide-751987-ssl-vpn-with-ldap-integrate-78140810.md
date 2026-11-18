SSL VPN with LDAP-integrated certificate authentication
                                                
                                            This is a sample configuration of SSL VPN that requires users to authenticate using a certificate with LDAP UserPrincipalName checking.
This sample uses Windows 2012R2 Active Directory acting as both the user certificate issuer, the certificate authority, and the LDAP server.
|  | When configuring an LDAP connection to an Active Directory server, an administrator must provide Active Directory user credentials.  | 
Sample topology
Sample configuration
WAN interface is the interface connected to ISP. This example shows static mode. You can also use DHCP or PPPoE mode. The SSL VPN connection is established over the WAN interface.
In this sample, the User Principal Name is included in the subject name of the issued certificate. This is the user field we use to search LDAP in the connection attempt.
To use the user certificate, you must first install it on the user's PC. When the user tries to authenticate, the user certificate is checked against the CA certificate to verify that they match.
Every user should have a unique user certificate. This allows you to distinguish each user and revoke a specific user's certificate, such as if a user no longer has VPN access.
To install the server certificate:
The server certificate is used for authentication and for encrypting SSL VPN traffic.
- Go to System > Feature Visibility and ensure Certificates is enabled.
- Go to System > Certificates and select Import > Local Certificate.
- Set Type to Certificate.
- Choose the Certificate file and the Key file for your certificate, and enter the Password.
- If required, change the Certificate Name.The server certificate now appears in the list of Certificates.
To install the CA certificate:
The CA certificate is the certificate that signed both the server certificate and the user certificate. In this example, it is used to authenticate SSL VPN users.
- Go to System > Certificates and select Import > CA Certificate.
- Select Local PC and then select the certificate file.The CA certificate now appears in the list of External CA Certificates. In this example, it is called CA_Cert_1.
To configure SSL VPN using the GUI:
- Configure the interface and firewall address. The port1 interface connects to the internal network.
  - Go to Network > Interfaces and edit the wan1 interface.
  - Set IP/Network Mask to 172.20.120.123/255.255.255.0.
  - Edit port1 interface and set IP/Network Mask to 192.168.1.99/255.255.255.0.
  - Click OK.
  - Go to Policy & Objects > Address and create an address for internet subnet 192.168.1.0.
- Configure the LDAP server:
  - Go to User & Authentication > LDAP Servers and click Create New.
  - Specify Name and Server IP/Name.
  - Set Distinguished Name to dc=fortinet-fsso,dc=com.
  - Set Bind Type to Regular.
  - Set Username to cn=admin,ou=testing,dc=fortinet-fsso,dc=com.
  - Set Password.
  - Click OK.
- Configure PKI users and a user group:To use certificate authentication, use the CLI to create PKI users. config user peer
    edit user1
        set ca CA_Cert_1
        set mfa-server "ldap-AD"
        set mfa-mode subject-identity
    next
endWhen you have create a PKI user, a new menu is added to the GUI: 
  - Go to User & Authentication > PKI to see the new user.
  - Go to User & Authentication > User > User Groups and create a group sslvpn-group.
  - Add the PKI peer object you created as a local member of the group.
  - Add a remote group on the LDAP server and select the group of interest.You need these users to be members using the LDAP browser window.
- Configure SSL VPN web portal:
  - Go to VPN > SSL-VPN Portals to edit the full-access portal.This portal supports both web and tunnel mode.
  - Disable Enable Split Tunneling so that all SSL VPN traffic goes through the FortiGate.
- Go to VPN > SSL-VPN Portals to edit the full-access portal.
- Configure SSL VPN settings:
  - Go to VPN > SSL-VPN Settings.
  - Select the Listen on Interface(s), in this example, wan1.
  - Set Listen on Port to 10443.
  - Set Server Certificate to the authentication certificate.
  - Under Authentication/Portal Mapping, set default Portal web-access for All Other Users/Groups.
  - Create new Authentication/Portal Mapping for group sslvpn-group mapping portal full-access.
- Configure SSL VPN firewall policy:
  - Go to Policy & Objects > Firewall Policy.
  - Fill in the firewall policy name. In this example, sslvpn certificate auth.
  - Incoming interface must be SSL-VPN tunnel interface(ssl.root).
  - Set the Source Address to all and Source User to sslvpn-group.
  - Set the Outgoing Interface to the local network interface so that the remote user can access the internal network. In this example, port1.
  - Set Destination Address to the internal protected subnet 192.168.1.0.
  - Set Schedule to always, Service to ALL, and Action to Accept.
  - Enable NAT.
  - Configure any remaining firewall and security options as desired.
  - Click OK.
To configure SSL VPN using the CLI:
- Configure the interface and firewall address:config system interface 
    edit "wan1"
        set vdom "root"
        set ip 172.20.120.123 255.255.255.0
    next
end
- Configure internal interface and protected subnet, then connect the port1 interface to the internal network:config system interface
    edit "port1"
        set vdom "root"
        set ip 192.168.1.99 255.255.255.0
    next
endconfig firewall address
    edit "192.168.1.0"
        set subnet 192.168.1.0 255.255.255.0
    next
end
- Configure the LDAP server:config user ldap
    edit "ldap-AD"
        set server "172.18.60.206"
        set cnid "cn"
        set dn "dc=fortinet-fsso,dc=com"
        set type regular
        set username "cn=admin,ou=testing,dc=fortinet-fsso,dc=com"
        set password ldap-server-password
    next
end
- Configure PKI users and a user group:config user peer
    edit user1
        set ca CA_Cert_1
        set mfa-server "ldap-AD"
        set mfa-mode subject-identity
    next
endconfig user group
    edit "sslvpn-group"
        set member "ldap-AD" "user1"
        config match
            edit 1
                set server-name "ldap-AD"
                set group-name "CN=group3,OU=Testing,DC=Fortinet-FSSO,DC=COM"
            next
        end
    next
end
- Configure SSL VPN web portal:config vpn ssl web portal
    edit "full-access"
        set tunnel-mode enable
        set web-mode enable
        set ip-pools "SSLVPN_TUNNEL_ADDR1"
        set split-tunneling disable
    next
end
- Configure SSL VPN settings:config vpn ssl settings
    set servercert "server_certificate"
    set tunnel-ip-pools "SSLVPN_TUNNEL_ADDR1"
    set source-interface "wan1"
    set source-address "all"
    set default-portal "web-access"
    config authentication-rule
        edit 1
            set groups "sslvpn-group"
            set portal "full-access"
        next        
    end
end
- Configure one SSL VPN firewall policy to allow remote user to access the internal network:config firewall policy 
    edit 1
        set name "sslvpn web mode access"
        set srcintf "ssl.root"
        set dstintf "port1"
        set srcaddr "all"
        set dstaddr "192.168.1.0"
        set groups "sslvpn-group"
        set action accept
        set schedule "always"
        set service "ALL"
        set nat enable
    next
end
To see the results of tunnel connection:
- Download FortiClient from www.forticlient.com.
- Open the FortiClient Console and go to Remote Access > Configure VPN.
- Add a new connection.
  - Set the connection name.
  - Set Remote Gateway to the IP of the listening FortiGate interface, in this example, 172.20.120.123.
  - Select Customize Port and set it to 10443.
  - Enable Client Certificate and select the authentication certificate.
- Save your settings.Connecting to the VPN only requires the user's certificate. It does not require username or password.
To see the results of web portal:
- In a web browser, log into the portal http://172.20.120.123:10443. A message requests a certificate for authentication.
- Select the user certificate.You can connect to the SSL VPN web portal.
To check the SSL VPN connection using the GUI:
- Go to Dashboard > Network and expand the SSL-VPN widget to verify the user's connection.
- Go to Log & Report > VPN Events to view the details of the SSL VPN connection event log.
- Go to Log & Report > Forward Traffic to view the details of the SSL VPN traffic.
To check the SSL VPN connection using the CLI:
Below is a sample output of diagnose debug application fnbamd -1 while the user connects. This is a shortened output sample of a few locations to show the important parts. This sample shows lookups to find the group memberships (three groups total) of the user and that the correct group being found results in a match.
[1148] fnbamd_ldap_recv-Response len: 16, svr: 172.18.60.206
[829] fnbamd_ldap_parse_response-Got one MESSAGE. ID:4, type:search-result
[864] fnbamd_ldap_parse_response-ret=0
[1386] __fnbamd_ldap_primary_grp_next-Auth accepted
[910] __ldap_rxtx-Change state to 'Done'
[843] __ldap_rxtx-state 23(Done)
[925] fnbamd_ldap_send-sending 7 bytes to 172.18.60.206
[937] fnbamd_ldap_send-Request is sent. ID 5
[753] __ldap_stop-svr 'ldap-AD'
[53] ldap_dn_list_del_all-Del CN=test3,OU=Testing,DC=Fortinet-FSSO,DC=COM
[399] ldap_copy_grp_list-copied CN=group3,OU=Testing,DC=Fortinet-FSSO,DC=COM
[399] ldap_copy_grp_list-copied CN=Domain Users,CN=Users,DC=Fortinet-FSSO,DC=COM
[2088] fnbamd_auth_cert_check-Matching group 'sslvpn-group'
[2007] __match_ldap_group-Matching server 'ldap-AD' - 'ldap-AD'
[2015] __match_ldap_group-Matching group 'CN=group3,OU=Testing,DC=Fortinet-FSSO,DC=COM' - 'CN=group3,OU=Testing,DC=Fortinet-FSSO,DC=COM'
[2091] fnbamd_auth_cert_check-Group 'sslvpn-group' matched
[2120] fnbamd_auth_cert_result-Result for ldap svr[0] 'ldap-AD' is SUCCESS
[2126] fnbamd_auth_cert_result-matched user 'test3', matched group 'sslvpn-group'
You can also use diagnose firewall auth list to validate that a firewall user entry exists for the SSL VPN user and is part of the right groups.