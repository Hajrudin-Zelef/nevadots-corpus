# Explicit proxy authentication

FortiGate supports multiple authentication methods. This topic explains using an external authentication server with Kerberos as the primary and NTLM as the fallback.

###### To configure Explicit Proxy with authentication:

###### To enable and configure explicit web proxy in the GUI:

1. 
                                                    Go to *Network > Explicit Proxy* .
2. 
                                                    Enable *Explicit Web Proxy* .
3. 
                                                    Select *port2* as the*Listen on Interfaces* and set the*HTTP Port* to*8080* .
4. 
                                                    Configure the remaining settings as needed.
5. 
                                                    Click *Apply* .

###### To enable and configure explicit web proxy in the CLI:

```
config web-proxy explicit
    set status enable
    set ftp-over-http enable
    set socks enable
    set http-incoming-port 8080
    set ipv6-status enable
    set unknown-http-version best-effort
end
config system interface
    edit "port2"
        set vdom "vdom1"
        set ip 10.1.100.1 255.255.255.0
        set allowaccess ping https ssh snmp http telnet
        set type physical
        set explicit-web-proxy enable
        set snmp-index 12
        end
    next
end
```
                                            
                                            Since we are using an external authentication server with Kerberos authentication as the primary and NTLM as the fallback, Kerberos authentication is configured first and then FSSO NTLM authentication is configured.

For successful authorization, the FortiGate checks if user belongs to one of the groups that is permitted in the security policy.

|  | When configuring an LDAP connection to an Active Directory server, an administrator must provide Active Directory user credentials.  | 

###### To configure an authentication server and create user groups in the GUI:

1. 
                                                    Configure Kerberos authentication: 
  1. 
                                                            Go to *User & Authentication > LDAP Servers* .
  2. 
                                                            Click *Create New* .
  3. 
                                                            Set the following: Name ldap-kerberos Server IP 172.18.62.220 Server Port 389 Common Name Identifier cn Distinguished Name dc=fortinetqa,dc=local
  4. 
                                                            Click *OK*
2. 
                                                            
3. 
                                                    Define Kerberos as an authentication service. This option is only available in the CLI. For information on generating a keytab, see Generating a keytab on a Windows server.
4. 
                                                    Configure FSSO NTLM authentication: FSSO NTLM authentication is supported in a Windows AD network. FSSO can also provide NTLM authentication service to the FortiGate unit. When a user makes a request that requires authentication, the FortiGate initiates NTLM negotiation with the client browser, but does not process the NTLM packets itself. Instead, it forwards all the NTLM packets to the FSSO service for processing. 
  1. 
                                                            Go to *Security Fabric > External Connectors* .
  2. 
                                                            Click *Create New* and select*FSSO Agent on Windows AD* from the*Endpoint/Identity* category.
  3. 
                                                            Set the *Name* to*FSSO* ,*Primary FSSO Agent* to*172.16.200.220* , and enter a password.
  4. 
                                                            Click *OK* .
5. 
                                                            
6. 
                                                    Create a user group for Kerberos authentication: 
  1. 
                                                            Go to *User & Authentication > User Groups* .
  2. 
                                                            Click *Create New* .
  3. 
                                                            Set the *Name* to*Ldap-Group* , and*Type* to*Firewall* .
  4. 
                                                            In the *Remote Groups* table, click*Add* , and set the*Remote Server* to the previously created*ldap-kerberos* server.
  5. 
                                                            Click *OK* .
7. 
                                                            
8. 
                                                    Create a user group for NTLM authentication: 
  1. 
                                                            Go to *User & Authentication > User Groups* .
  2. 
                                                            Click *Create New* .
  3. 
                                                            Set the *Name* to*NTLM-FSSO-Group* ,*Type* to*Fortinet Single Sign-On (FSSO)* , and add*FORTINETQA/FSSO* as a member.
  4. 
                                                            Click *OK* .
9. 
                                                            

###### To configure an authentication server and create user groups in the CLI:

1. 
                                                    Configure Kerberos authentication: ```
config user ldap
    edit "ldap-kerberos"
        set server "172.18.62.220"
        set cnid "cn"
        set dn "dc=fortinetqa,dc=local"
        set type regular
        set username "CN=root,CN=Users,DC=fortinetqa,DC=local"
        set password *********
    next
end
```
2. 
                                                    Define Kerberos as an authentication service: ```
config user krb-keytab
    edit "http_service"
        set pac-data disable
        set principal "HTTP/FGT.FORTINETQA.LOCAL@FORTINETQA.LOCAL"
        set ldap-server "ldap-kerberos"
        set keytab "BQIAAABFAAIAEEZPUlRJTkVUUUEuTE9DQUwABEhUVFAAFEZHVC5GT1JUSU5FVFFBLkxPQ0FMAAAAAQAAAAAEAAEACKLCMonpitnVAAAARQACABBGT1JUSU5FVFFBLkxPQ0FMAARIVFRQABRGR1QuRk9SVElORVRRQS5MT0NBTAAAAAEAAAAABAADAAiiwjKJ6YrZ1QAAAE0AAgAQRk9SVElORVRRQS5MT0NBTAAESFRUUAAURkdULkZPUlRJTkVUUUEuTE9DQUwAAAABAAAAAAQAFwAQUHo9uqR9cSkzyxdzKCEXdwAAAF0AAgAQRk9SVElORVRRQS5MT0NBTAAESFRUUAAURkdULkZPUlRJTkVUUUEuTE9DQUwAAAABAAAAAAQAEgAgzee854Aq1HhQiKJZvV4tL2Poy7hMIARQpK8MCB//BIAAAABNAAIAEEZPUlRJTkVUUUEuTE9DQUwABEhUVFAAFEZHVC5GT1JUSU5FVFFBLkxPQ0FMAAAAAQAAAAAEABEAEG49vHEiiBghr63Z/lnwYrU="
    next
end
```
For information on generating a keytab, see Generating a keytab on a Windows server.
3. 
                                                    Configure FSSO NTLM authentication: ```
config user fsso
    edit "1"
        set server "172.18.62.220"
        set password *********
    next
end
```
4. 
                                                    Create a user group for Kerberos authentication: ```
config user group
    edit "Ldap-Group"
        set member "ldap" "ldap-kerberos"
    next
end
```
5. 
                                                    Create a user group for NTLM authentication: ```
config user group
    edit "NTLM-FSSO-Group"
        set group-type fsso-service
        set member "FORTINETQA/FSSO"
    next
end
```

Explicit proxy authentication is managed by authentication schemes and rules. An authentication scheme must be created first, and then the authentication rule.

###### To create an authentication scheme and rules in the GUI:

1. 
                                                    Create an authentication scheme: 
  1. 
                                                            Go to *Policy & Objects > Authentication Rules* .
  2. 
                                                            Click *Create New > Authentication Schemes* .
  3. 
                                                            Set the *Name* to*Auth-scheme-Negotiate* and select*Negotiate* as the*Method* .
  4. 
                                                            Click *OK* .
2. 
                                                            
3. 
                                                    Create an authentication rule: 
  1. 
                                                            Go to *Policy & Objects > Authentication Rules* .
  2. 
                                                            Click *Create New > Authentication Rules* .
  3. 
                                                            Set the *Name* to*Auth-Rule* ,*Source Address* to*all* , and*Protocol* to*HTTP* .
  4. 
                                                            Enable *Authentication Scheme* , and select the just created*Auth-scheme-Negotiate* scheme.
  5. 
                                                            Click *OK* .
4. 
                                                            

###### To create an authentication scheme and rules in the CLI:

1. 
                                                    Create an authentication scheme: ```
config authentication scheme
    edit "Auth-scheme-Negotiate"
        set method negotiate       <<< Accepts both Kerberos and NTLM as fallback
    next
end
```
2. 
                                                    Create an authentication rule: ```
config authentication rule
    edit "Auth-Rule"
        set status enable
        set protocol http
        set srcaddr "all"
        set ip-based enable
        set active-auth-method "Auth-scheme-Negotiate"
        set comments "Testing"
    next
end
```

###### To create an explicit proxy policy and assign a user group to it in the GUI:

1. 
                                                    Go to *Policy & Objects > Proxy Policy* .
2. 
                                                    Click *Create New* .
3. 
                                                    Set *Proxy Type* to*Explicit Web* and*Outgoing Interface* to*port1* .
4. 
                                                    Set *Source* to*all* , and the just created user groups*NTLM-FSSO-Group* and*Ldap-Group* .
5. 
                                                    Also set *Destination* to*all* ,*Schedule* to*always* ,*Service* to*webproxy* , and*Action* to*ACCEPT* .
6. 
                                                    Click *OK* .

###### To create an explicit proxy policy and assign a user group to it in the CLI:

```
config firewall proxy-policy
    edit 1
        set proxy explicit-web
        set dstintf "port1"
        set srcaddr "all"
        set dstaddr "all"
        set service "web"
        set action accept
        set schedule "always"
        set logtraffic all
        set groups "NTLM-FSSO-Group" "Ldap-Group"
        set av-profile "av"
        set ssl-ssh-profile "deep-custom"
    next
end
```
                                            
                                            Log in using a domain and system that would be authenticated using the Kerberos server, then enter the `diagnose wad user list` CLI command to verify:

```
# diagnose wad user list
ID: 8, IP: 10.1.100.71, VDOM: vdom1
  user name   : test1@FORTINETQA.LOCAL
  duration    : 389
  auth_type   : IP
  auth_method : Negotiate
  pol_id      : 1
  g_id        : 1
  user_based  : 0
  expire      : no
  LAN:
    bytes_in=4862 bytes_out=11893
  WAN:
    bytes_in=7844 bytes_out=1023
```
                                            Log in using a system that is not part of the domain. The NTLM fallback server should be used:

```
# diagnose wad user list
ID: 2, IP: 10.1.100.202, VDOM: vdom1
  user name   : TEST31@FORTINETQA
  duration    : 7
  auth_type   : IP
  auth_method : NTLM
  pol_id      : 1
  g_id        : 5
  user_based  : 0
  expire      : no
  LAN:
    bytes_in=6156 bytes_out=16149
  WAN:
    bytes_in=7618 bytes_out=1917
```
                                            
                                            A keytab is used to allow services that are not running Windows to be configured with service instance accounts in the Active Directory Domain Service (AD DS). This allows Kerberos clients to authenticate to the service through Windows Key Distribution Centers (KDCs).

For an explanation of the process, see https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/ktpass.

###### To generate a keytab on a Windows server:

1. 
                                                    On the server, create a user for the FortiGate: 
  - 
                                                            The service name is the FQDN for the explicit proxy interface, such as the hostname in the client browser proxy configuration. In this example, the service name is *FGT* .
  - 
                                                            The account only requires *domain users* membership.
  - 
                                                            The password must be very strong.
  - 
                                                            The password is set to never expire.
2. 
                                                            
3. 
                                                    Add the FortiGate FQDN in to the Windows DNS domain, as well as in-addr.arpa.
4. 
                                                    Generate the Kerberos keytab using the `ktpass` command on Windows servers and many domain workstations:# ktpass -princ HTTP/<domain name of test fgt>@realm -mapuser <user> -pass <password> -crypto all -ptype KRB5_NT_PRINCIPAL -out fgt.keytab For example: ktpass -princ HTTP/FGT.FORTINETQA.LOCAL@FORTINETQA.LOCAL -mapuser FGT -pass *********** -crypto all -ptype KRB5_NT_PRINCIPAL -out fgt.keytab If the FortiGate is handling multiple keytabs in Kerberos authentication, use different passwords when generating each keytab.
5. 
                                                    Encode the keytab to base64 in a text file: 
  - 
                                                            On Windows: `certutil -encode fgt.keytab tmp.b64 && findstr /v /c:- tmp.b64 > fgt.txt`
  - 
                                                            On Linux: `base64 fgt.keytab > fgt.txt`
  - 
                                                            On MacOS: `base64 -i fgt.keytab -o fgt.txt`
6. 
                                                            
7. 
                                                    Use the code in `fgt.txt` as the keytab parameter when configuring the FortiGate.