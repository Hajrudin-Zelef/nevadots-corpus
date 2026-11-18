About the Secure Client VPN Client
The Secure Client provides secure SSL and IPsec/IKEv2 connections to the ASA for remote users. Without a previously-installed client, remote users enter the IP address in their browser of an interface configured to accept SSL or IPsec/IKEv2 VPN connections. Unless the ASA is configured to redirect http:// requests to https://, users must enter the URL in the form https://<address>.
After entering the URL, the browser connects to that interface and displays the login screen. If the user satisfies the login and authentication, and the ASA identifies the user as requiring the client, it downloads the client that matches the operating system of the remote computer. After downloading, the client installs and configures itself, establishes a secure SSL or IPsec/IKEv2 connection and either remains or uninstalls itself (depending on the configuration) when the connection terminates.
In the case of a previously installed client, when the user authenticates, the ASA examines the revision of the client, and upgrades the client as necessary.
When the client negotiates an SSL VPN connection with the ASA, it connects using Transport Layer Security (TLS), and optionally, Datagram Transport Layer Security (DTLS). DTLS avoids latency and bandwidth problems associated with some SSL connections and improves the performance of real-time applications that are sensitive to packet delays.
The Secure Client can be downloaded from the ASA, or it can be installed manually on the remote PC by the system administrator. For more information about installing the client manually, see the appropriate release of the Cisco AnyConnect Secure Mobility Configuration Guide .
The ASA downloads the client based on the group policy or username attributes of the user establishing the connection. You can configure the ASA to automatically download the client, or you can configure it to prompt the remote user about whether to download the client. In the latter case, if the user does not respond, you can configure the ASA to either download the client after a timeout period or present the login page.
Requirements for Secure Client
For the requirements of endpoint computers running the Secure Client, see the appropriate release of the Cisco AnyConnect Secure Mobility Release Notes.
Guidelines and Limitations for Secure Client
-  
                                 			 
                                 The ASA does not verify remote HTTPS certificates.
-  
                                 			 
                                 Supported in single or multiple context mode. AnyConnect Apex license is required for remote-access VPN in multi-context mode. Although ASA does not specifically recognize an AnyConnect Apex license, it enforces licenses characteristics of an Apex license such as AnyConnect Premium licensed to the platform limit, Secure Client for mobile, Secure Client for Cisco VPN phone, and advanced endpoint assessment. Shared licensing, AnyConnect Essentials, failover license aggregation, and flex/time-based licenses are not supported.
- 
                                 					
                                 					
                                 Issuing commands such as curl against the RA VPN headend is not directly supported, and might not have desirable results. For example, the headend does not respond to HTTP HEAD requests.
- 
                                 					
                                 When hardware VPN phones such as the Cisco 88xx series use Secure Client, they can experience a reconnection despite having DTLS up and Dead Peer Detection (DPD) configured.
- 
                                 					
                                 When a client connects to Secure Client, the IP address of the client before and after the connection changes. ASA supports this behavior.