---
id: collect-261121-veille-ia/claude-anthropic/send-feedback-33
title: "Migrating from SSL VPN to ZTNA"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["server", "ztna", "user", "access", "tunnel", "ldap", "configure", "migrating"]
source: https://docs.fortinet.com/document/fortigate/7.2.0/administration-guide/78050/migrating-from-ssl-vpn-to-ztna
source_anchor: ""
source_lines: [1, 288]
section: "Claude_Anthropic"
---
# Migrating from SSL VPN to ZTNA

# Migrating from SSL VPN to ZTNA

ZTNA can be used to replace VPN-based teleworking solutions to enhance the user experience and to increase security. A typical teleworking configuration may utilize SSL VPN tunnel or web portal mode with LDAP user authentication. Common objects defined for this setup can be reused when migrating to ZTNA, such as the remote LDAP server, user group, and address objects.

### SSL VPN tunnel mode access with LDAP user authentication

Remote users that are in the *ALLOWED-VPN* active directory group have access to a specific web server when they connect through the SSL VPN tunnel. The FortiGate enables split tunneling to the web server so that only traffic to that destination is routed through the tunnel. The web server hosts internal websites that are only accessible by employees.


### SSL VPN web mode access with LDAP user authentication

Remote users that are in the *ALLOWED-VPN* active directory group have access to a specific web server when they connect through the SSL VPN web portal. The web server hosts internal websites that are only accessible by employees. The predefined bookmark to the internal website is the only site that allows remote access.


## Common configurations

This section includes configurations for common objects used in the SSL VPN configuration that can be reused in the ZTNA deployment:

###### To configure an LDAP server:

```
config user ldap
    edit "WIN2K16-KLHOME-LDAPS"
        set server "192.168.20.6"
        set server-identity-check disable
        set cnid "sAMAccountName"
        set dn "dc=KLHOME,dc=local"
        set type regular
        set username "KLHOME\\Administrator"
        set password **********
        set secure ldaps
        set ca-cert "CA_Cert_1"
        set port 636
    next
end
```
                                            
                                            ###### To configure the user group:

```
config user group
    edit "KLHOME-ALLOWED-VPN"
        set member "WIN2K16-KLHOME-LDAPS"
        config match
            edit 1
                set server-name "WIN2K16-KLHOME-LDAPS"
                set group-name "CN=ALLOWED-VPN,DC=KLHOME,DC=local"
            next
        end
    next
end
```
                                            
                                            Firewall addresses can be reused in the server settings for TCP forwarding configurations.

###### To configure the firewall address:

```
config firewall address
    edit "winserver"
        set subnet 192.168.20.6 255.255.255.255
    next
end
```
                                            ## Migrating to ZTNA

The preceding simple SSL VPN tunnel and web mode teleworking solutions can be migrated to ZTNA configurations, providing device authentication using client certificates and additional security posture checks.


Instead of connecting to the SSL VPN tunnel or web portal, the remote user connects to the HTTPS access proxy that forwards traffic to the web server after authentication and security posture checks are completed. This provides granular control over who can access the web resource using role-based access control. It also gives the user transparent access to the website using only their browser.

Migrating to ZTNA includes the following steps:

The first step to configure ZTNA is to connect to and authorize a FortiClient EMS using the EMS connector. There are different ways to connect to an on-premise FortiClient EMS server and a FortiClient EMS Cloud. Refer to the first step of Configure a FortiClient EMS connector for instructions.

ZTNA tags and tagging rules define security posture checks that connecting devices must pass before they are allowed to access protected resources and applications. In the following example, a Zero Trust tagging rule is configured to detect if a virus file exists on an endpoint.

###### To configure a Zero Trust tagging rule on the FortiClient EMS:

1. 
                                                    Log in to the FortiClient EMS.
2. 
                                                    Go to *Zero Trust Tags > Zero Trust Tagging Rules* , and click*Add* .
3. 
                                                    In the *Name* field, enter*Malicious-File-Detected* .
4. 
                                                    In the *Tag Endpoint As* dropdown list, select*Malicious-File-Detected* .EMS uses this tag to dynamically group together endpoints that satisfy the rule, as well as any other rules that are configured to use this tag.
5. 
                                                    Click *Add Rule* then configure the rule:
  1. 
                                                            For *OS* , select*Windows* .
  2. 
                                                            From the *Rule Type* dropdown list, select*File* and click  the*+* button.
  3. 
                                                            Enter a file name, such as *C:\virus.txt* .
  4. 
                                                            Click *Save* .
6. 
                                                            
7. 
                                                    Click *Save* .

A ZTNA solution requires users to be registered and connected to the FortiClient EMS server. When an EMS server is behind the FortiGate, a VIP needs to be defined to allow remote users access to register to the FortiClient EMS. The only port required to be forwarded is TCP/8013. This VIP also needs to be applied in a firewall policy to allow this traffic.

###### To configure a VIP to allow traffic to the EMS server:

1. Go to *Policy & Objects > Virtual IPs* and click*Create New > Virtual IP* .
2. Set *Name* to*VIP-EMS* .
3. Configure the VIP settings:
  1. Set *Interface* to*port1* .
  2. Set *External IP address/range* to*192.168.2.5* .
  3. Set *Map to* to*192.168.20.10* .
  4. Enable *Port Forwarding* .
  5. Set *External service port* to*8013* .
  6. Set *Map to IPv4 port* to*8013* .
4. Set 
5. Click *OK* .

###### To configure the firewall policy:

1. Go to *Policy & Objects >Firewall Policy* and click*Create New* .
2. Set *Name* to*ZTNA-VIP* .
3. Configure the policy settings:
  1. Set *Incoming Interface* to*port1* .
  2. Set *Outgoing Interface* to*port3* .
  3. Set *Source* to*all* .
  4. Set *Destination* to*VIP-EMS* .
  5. For *Service* , select an option that is for TCP/8013.
  6. Disable *NAT* .
  7. Configure the remaining options as needed.
4. Set 
5. Click *OK* .

The ZTNA server defines the external IP and port used for the FortiGate access proxy. It also defines the protected resources that can be accessed through the HTTPS access proxy or TCP forwarding access proxy. The following configuration defines a HTTPS access proxy for accessing the web server on 192.168.20.6.

###### To configure a ZTNA server for HTTPS access proxy:

1. 
                                                    Go to *Policy & Objects > ZTNA* and select the*ZTNA Servers* tab.
2. 
                                                    Click *Create New* .
3. 
                                                    Set *Name* to*WIN2K16-P1* .
4. 
                                                    Configure the network settings: 
  1. 
                                                            Set *External interface* to*port1* .
  2. 
                                                            Set *External IP* to*192.168.2.86* .
  3. 
                                                            Set *External port* to*8443* .
5. 
                                                            
6. 
                                                    Select the *Default certificate* . Clients will be presented with this certificate when they connect to the access proxy VIP.
7. 
                                                    Add server mapping: 
  1. 
                                                            In the *Service/server mapping* table, click*Create New* .
  2. 
                                                            Set *Service* to*HTTPS* .
  3. 
                                                            Set *Virtual Host* to*Any Host* .
  4. 
                                                            Configure the path as needed. For example, to map to *winserver.fgdocs.com/fortigate* , enter*/fortigate* .
  5. 
                                                            Add a server: 
    1. 
                                                                    In the *Servers* table, click*Create New* .
    2. 
                                                                    Set *IP* to*192.168.20.6* .
    3. 
                                                                    Set *Port* to*443* .
    4. 
                                                                    Click *OK* .
  6. 
                                                                    
  7. 
                                                            Click *OK* .
8. 
                                                            
9. 
                                                    Click *OK* .

The authentication scheme defines the authentication method that is applied. In this example, basic HTTP authentication is used so that users are prompted for a username and password the first time that they connect to a website through the HTTPS access proxy. The LDAP server defined for the SSL VPN configurations can be reused here.

###### To configure an authentication scheme:

1. 
                                                    Go to *Policy & Objects > Authentication Rules* and click*Create New > Authentication Scheme* .
2. 
                                                    Set the name to *ZTNA-Auth-scheme* .
3. 
                                                    Set *Method* to*Basic* .
4. 
                                                    Set *User database* to*Other* and select*WIN2K16-KLHOME-LDAPS* as the LDAP server.
5. 
                                                    Click *OK* .

The authentication rule defines the proxy sources and destination that require authentication, and what authentication scheme is applied. In this example, active authentication through the basic HTTP prompt is used and applied to all sources.

###### To configure an authentication rule:

1. 
                                                    Go to *Policy & Objects > Authentication Rules* and click*Create New > Authentication Rule* .
2. 
                                                    Set the name to *ZTNA-Auth-rule* .
3. 
                                                    Set *Source Address* to*all* .
4. 
                                                    Set *Protocol* to*HTTP* .
5. 
                                                    Enable *Authentication Scheme* and select*ZTNA-Auth-scheme* .
6. 
                                                    Click *OK* .

A user or user group must be applied to the ZTNA rule used to control user access. The authenticated user from the authentication scheme and rule must match the user or user group in the ZTNA rule. The user group, *KLHOME-ALLOWED-VPN*, defined  in the SSL VPN configurations is reused in this example. The ZTNA tag, *Malicious-File-Detected*, is used to define a rule to deny access when the connecting device has the malicious file detected.

###### To configure ZTNA rules to allow and deny traffic based on ZTNA tags:

1. 
                                                    Go to *Policy & Objects > ZTNA* and select the*ZTNA Rules* tab.
2. 
                                                    Create a rule to deny traffic: 
  1. 
                                                            Click *Create New* .
  2. 
                                                            Set *Name* to*ZTNA-Deny-malicious* .
  3. 
                                                            Set *Incoming Interface* to*port1* .
  4. 
                                                            Set *Source* to*all* , then click the*+* and from the*User* tab, select the*KLHOME-ALLOWED-VPN* group.
  5. 
                                                            Add the ZTNA tag *Malicious-File-Detected* .This tag is dynamically retrieved from EMS when the Zero Trust tagging rule is first created.
  6. 
                                                            Select the ZTNA server *WIN2K16-P1* .
  7. 
                                                            Set *Action* to*DENY* .
  8. 
                                                            Enable *Log Violation Traffic* .
  9. 
                                                            Click *OK* .
3. 
                                                            
4. 
                                                    Create a rule to allow traffic: 
  1. 
                                                            Click *Create New* .
  2. 
                                                            Set *Name* to*proxy-WIN2K16-P1* .
  3. 
                                                            Set *Incoming Interface* to*port1* .
  4. 
                                                            Set *Source* to*all* , then click the*+* and from the*User* tab, select the*KLHOME-ALLOWED-VPN* group. The*Source* can also be set to specific IP addresses to only allow those addresses to connect to this HTTPS access proxy.
  5. 
                                                            Add the ZTNA tag *Low* .
  6. 
                                                            Select the ZTNA server *WIN2K16-P1* .
  7. 
                                                            Set *Action* to*ACCEPT* .
  8. 
                                                            Configure the remaining options as needed.
  9. 
                                                            Click *OK* .
5. 
                                                            
6. 
                                                    In the *ZTNA Rules* list, make sure that the deny rule (*ZTNA-Deny-malicious* ) is above the allow rule (*proxy-WIN2K16-P1* ).

## Testing the connection

Once ZTNA is configured, connect to the FortiGate access proxy using an endpoint that is registered to EMS. The user should be prompted for their device certificate, username, and password the first time they connect. Once they have authenticated and they pass the security posture checks, they will be allowed to access the website.

See ZTNA HTTPS access proxy example and ZTNA HTTPS access proxy with basic authentication example for sample verifications and results.

## Disabling the SSL VPN

Once testing is complete and the ZTNA servers and policies are configured, the users can be migrated to using ZTNA. Use the following checklist to verify if the remote users are ready to migrate:

1. The users have installed a supported FortiClient version and have installed the ZTNA module.
2. The endpoints can register to FortiClient EMS.
3. If using a TCP forwarding access proxy, ensure that ZTNA rules are either pushed from FortiClient EMS, or the users know how to configure them manually.

Next, SSL VPN access can be disabled in a phased approach by disabling SSL VPN firewall policies that allow access to resources that are accessible using ZTNA.

Once all applications and resources have been migrated, the SSL VPN can be disabled entirely by going to *VPN > SSL-VPN Settings*, and deselecting the *Enable SSL-VPN* toggle.
