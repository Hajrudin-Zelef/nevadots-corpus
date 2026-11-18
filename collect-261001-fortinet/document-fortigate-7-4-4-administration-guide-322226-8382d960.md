# Generate a new certificate

# Generate a new certificate

The FortiGate can generate a certificate using a pre-loaded, self-signed CA certificate: *Fortinet_CA_SSL*, instead of generating a CSR and providing it to a CA for signing. It is recommended that a server certificate from a well-known and trusted CA is used.

###### To generate a new certificate:

1. 
                                                    Go to *System > Certificates* and select*Create/Import > Certificate* .
2. 
                                                    Click *Generate Certificate* .
3. 
                                                    Set *Certificate name* to the name of the certificate. This is what is referenced when using the certificate in FortiGate configurations.
4. 
                                                    Set the *Common name* (CN) for the certificate. The common name should match the FQDN or IP of the primary SSL-VPN interface.
5. 
                                                    Optionally, set the *Subject alternative name* .
6. 
                                                    Click *Download CA Certificate* to download the CA certificate so that it can be installed or imported to all the machines that need to trust this certificate.
7. 
                                                    Click *Create* .
8. 
                                                    After the certificate is created, click *Download Certificate* to download the certificate. Click*View Details* to review the certificate details.
9. 
                                                    Click *OK* .