---
id: collect-261121-veille-ia/gemini-google/send-feedback-24
title: "Send Feedback"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["certificate", "certificates", "user", "server", "authentication", "signed", "example", "client"]
source: https://docs.fortinet.com/document/fortigate/7.4.3/administration-guide/266506/ssl-vpn-with-certificate-authentication
source_anchor: ""
source_lines: [1, 263]
section: "Gemini_Google"
---
# SSL VPN with certificate authentication

# SSL VPN with certificate authentication

                                                

There are two ways to configure certificate authentication:

In this example, the server and client certificates are signed by the same Certificate Authority (CA).

|  | Self-signed certificates are provided by default to simplify initial installation and testing. It is **HIGHLY** recommended that you acquire a signed certificate for your installation. Continuing to use these certificates can result in your connection being compromised, allowing attackers to steal your information, such as credit card details. For more information, please review the Use a non-factory SSL certificate for the SSL VPN portal and learn how to Procuring and importing a signed SSL certificate. | 


When using PKI users, the FortiGate authenticates the user based on there identity in the subject or the common name on the certificate. The certificate must be signed by a CA that is known by the FortiGate, either through the default CA certificates or through importing a CA certificate.

The user can either match a static subject or common name defined in the PKI user settings, or match an LDAP user in the LDAP server defined in the PKI user settings. Multi-factor authentication can also be enabled with the password as the second factor.

Using this method, the user is authenticated based on their regular username and password, but SSL VPN will still require an additional certificate check. The client certificate only needs to be signed by a known CA in order to pass authentication.

This method can be configured by enabling *Require Client Certificate* (`reqclientcert`) in the SSL-VPN settings.

### Configuration

In the following example, SSL VPN users are authenticated using the first method. A PKI user is configured with multi-factor authentication

Pre-requisites:

- 
                                                    The CA has already issued a client certificate to the user.
- 
                                                    The CA has issued a server certificate for the FortiGate's SSL VPN portal.
- 
                                                    The CA certificate is available to be imported on the FortiGate.

###### To configure SSL VPN in the GUI:

1. 
                                                    Install the server certificate. The server certificate allows the clients to authenticate the server and to encrypt the SSL VPN traffic. 
  1. 
                                                            Go to *System > Feature Visibility* and ensure*Certificates* is enabled.
  2. 
                                                            Go to *System > Certificates* and select*Import > Local Certificate* .
    - 
                                                                    Set *Type* to*Certificate* .
    - 
                                                                    Choose the *Certificate file* and the*Key file* for your certificate, and enter the*Password* .
    - 
                                                                    If required, you can change the *Certificate Name* .
  3. 
                                                                    
 The server certificate now appears in the list of *Certificates* .
2. 
                                                            
3. 
                                                    Install the CA certificate. The CA certificate is the certificate that signed both the server certificate and the user certificate. In this example, it is used to authenticate SSL VPN users. 
  1. 
                                                            Go to *System > Certificates* and select*Import > CA Certificate* .
  2. 
                                                            Select *Local PC* and then select the certificate file.
 The CA certificate now appears in the list of *External CA Certificates* . In this example, it is called*CA_Cert_1* .
4. 
                                                            
5. 
                                                    Configure PKI users and a user group. To use certificate authentication, use the CLI to create PKI users. ```
config user peer
    edit pki01
        set ca CA_Cert_1
        set subject "CN=User01"
    next
end
```
Ensure that the subject matches the name of the user certificate. In this example, *User01* .
6. 
                                                    After you have create a PKI user, a new menu is added to the GUI: 
  1. 
                                                            Go to *User & Authentication > PKI* to see the new user.
  2. 
                                                            Edit the user account.
  3. 
                                                            Enable *Two-factor authentication* and set a password for the account.
  4. 
                                                            Go to *User & Authentication > User Groups* and create a group  called*sslvpngroup* .
  5. 
                                                            Add the PKI user *pki01* to the group.
7. 
                                                            
8. 
                                                    Configure SSL VPN web portal. 
  1. 
                                                            Go to *VPN > SSL-VPN Portals* to edit the*full-access* portal.This portal supports both web and tunnel mode.
  2. 
                                                            Disable *Enable Split Tunneling* so that all SSL VPN traffic goes through the FortiGate.
9. 
                                                            
10. 
                                                    Configure SSL VPN settings. 
  1. 
                                                            Go to *VPN > SSL-VPN Settings* and enable SSL-VPN.
  2. 
                                                            Set the *Listen on Interface(s)* to*wan1* .
  3. 
                                                            Set *Listen on Port* to*10443* .
  4. 
                                                            Set *Server Certificate* to the local certificate that was imported.
  5. 
                                                            Under *Authentication/Portal Mapping* , set default Portal*web-access* for*All Other Users/Groups* .
  6. 
                                                            Create new *Authentication/Portal Mapping* for group*sslvpngroup* mapping portal*full-access* .
11. 
                                                            
12. 
                                                    Configure SSL VPN firewall policy. 
  1. 
                                                            Go to *Policy & Objects > Firewall Policy* .
  2. 
                                                            Fill in the firewall policy name. In this example, *sslvpn certificate auth* .
  3. 
                                                            Incoming interface must be *SSL-VPN tunnel interface(ssl.root)* .
  4. 
                                                            Set the *Source Address* to*all* and*Source User* to*sslvpngroup* .
  5. 
                                                            Set the *Outgoing Interface* to the local network interface so that the remote user can access the internal network. In this example,*port1* .
  6. 
                                                            Set *Destination Address* to the internal protected subnet*192.168.1.0* .
  7. 
                                                            Set *Schedule* to*always* ,*Service* to*ALL* , and*Action* to*Accept* .
  8. 
                                                            Enable *NAT* .
  9. 
                                                            Configure any remaining firewall and security options as needed.
  10. 
                                                            Click *OK* .
13. 
                                                            

###### To configure SSL VPN in the CLI:

1. 
                                                    Configure the protected subnet: ```
config firewall address
    edit "192.168.1.0"
        set subnet 192.168.1.0 255.255.255.0
    next
end
```
2. 
                                                    Install the server certificate: The server certificate allows the clients to authenticate the server and to encrypt the SSL VPN traffic. While it is easier to install the server certificate in the GUI, the CLI can be used to import a p12 certificate from a TFTP server. To import a p12 certificate, put the certificate *server_certificate.p12* on your TFTP server, then run following command on the FortiGate:execute vpn certificate local import tftp server_certificate.p12 <your tftp_server> p12 <your password for PKCS12 file> To check that the server certificate is installed: show vpn certificate local server_certificate
3. 
                                                    Install the CA certificate: The CA certificate is the certificate that signed both the server certificate and the user certificate. In this example, it is used to authenticate SSL VPN users. While it is easier to install the CA certificate from GUI, the CLI can be used to import a CA certificates from a TFTP server. To import a CA certificate, put the CA certificate on your TFTP server, then run following command on the FortiGate: execute vpn certificate ca import tftp <your CA certificate name> <your tftp server> To check that a new CA certificate is installed: show vpn certificate ca
4. 
                                                    Configure PKI users and a user group: ```
config user peer
    edit pki01
        set ca CA_Cert_1
        set subject "CN=User01"
        set two-factor enable
        set passwd **********
    next
end
```
```
config user group
    edit "sslvpngroup" 
        set member "pki01"
    next 
end
```
5. 
                                                    Configure SSL VPN web portal: ```
config vpn ssl web portal
    edit "full-access"
        set tunnel-mode enable
        set web-mode enable
        set ip-pools "SSLVPN_TUNNEL_ADDR1"
        set split-tunneling disable
     next
end
```
6. 
                                                    Configure SSL VPN settings: ```
config vpn ssl settings
    set servercert "server_certificate"
    set tunnel-ip-pools "SSLVPN_TUNNEL_ADDR1"
    set source-interface "wan1"
    set source-address "all"
    set default-portal "web-access"
    config authentication-rule
        edit 1
            set groups "sslvpngroup"
            set portal "full-access"
        next   
    end		
end
```
7. 
                                                    Configure one SSL VPN firewall policy to allow remote user to access the internal network: ```
config firewall policy 
    edit 1
        set name "sslvpn web mode access"
        set srcintf "ssl.root"
        set dstintf "port1"
        set srcaddr "all"
        set dstaddr "192.168.1.0"
        set groups "sslvpngroup"
        set action accept
        set schedule "always"
        set service "ALL"
        set nat enable
    next
end
```

### Installation

To use the user certificate, you must first install it on the user's PC. When the user tries to authenticate, the user certificate is checked against the CA certificate to verify that they match.

Every user should have a unique user certificate. This allows you to distinguish each user and revoke a specific user's certificate, such as if a user no longer has VPN access.

###### To install the user certificate on Windows 7, 8, and 10:

1. Double-click the certificate file to open the *Import Wizard* .
2. Use the *Import Wizard* to import the certificate into the*Personal store* of the current user.

###### To install the user certificate on Mac OS X:

1. Open the certificate file, to open *Keychain Access* .
2. Double-click the certificate.
3. Expand *Trust* and select*Always Trust* .

###### To see the results of tunnel connection:

1. Download FortiClient from www.forticlient.com.
2. Open the FortiClient Console and go to *Remote Access > Configure VPN* .
3. Add a new connection.
  - Set *VPN Type* to*SSL VPN* .
  - Set *Remote Gateway* to the IP of the listening FortiGate interface, in this example,*172.20.120.123* .
4. Set 
5. Select *Customize Port* and set it to*10443* .
6. Enable *Client Certificate* and select the authentication certificate.
7. Save your settings.
8. Use the credentials you've set up to connect to the SSL VPN tunnel.If the certificate is correct, you can connect.

###### To see the results of web portal:

1. In a web browser, log into the portal *http://172.20.120.123:10443* .A message requests a certificate for authentication.
2. Select the user certificate.
3. Enter your user credentials.If the certificate is correct, you can connect to the SSL VPN web portal.

###### To check the SSL VPN connection using the GUI:

1. Go to *VPN > Monitor > SSL-VPN Monitor* to verify the list of SSL users.
2. Go to *Log & Report > System Events* and select the*VPN Events* card to view the details for the SSL connection log.

###### To check the SSL VPN connection using the CLI:

get vpn ssl monitor
SSL VPN Login Users:
 Index   User    Auth Type      Timeout         From     HTTP in/out    HTTPS in/out
 0       pki01,cn=User01          1(1)            229     10.1.100.254   0/0     0/0
 1       pki01,cn=User01          1(1)            291     10.1.100.254   0/0     0/0
SSL VPN sessions:
 Index   User    Source IP      Duration        I/O Bytes       Tunnel/Dest IP 
 0       pki01,cn=User01          10.1.100.254    9       22099/43228    10.212.134.200
