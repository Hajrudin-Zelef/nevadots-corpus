To enable packet capture capabilities for packet sniffing and network fault isolation, use the capture  command in privileged EXEC mode. To disable packet capture capabilities, use the no  form of this command.
                           
                           
                           
                              Capture network traffic:
                           
                           
                        
                        
                        
                           
                           
                           
                              
                              capture
                              capture_name
                              [ 
                              
                              type
                              { 
                              
                              asp-drop
                              [ 
                              all
                              | 
                              drop-code
                              ] 
                              
                              | 
                              tls-proxy
                              | 
                              raw-data
                              | 
                              
                              isakmp
                              [ 
                              ikev1
                              | 
                              ikev2
                              ] 
                              
                              | 
                              
                              inline-tag
                              [ 
                              tag
                              
                              ] 
                              | 
                              
                              webvpn
                              user
                              webvpn-user
                              
                              } 
                              ] 
                              
                              [ 
                              
                              access-list
                              access_list_name
                              
                              { 
                              
                              interface
                              { 
                              interface_name
                              | 
                              asa_dataplane
                              asa_mgmt_plane
                              | 
                              cplane
                              } 
                              } 
                              
                              [ 
                              
                              buffer
                              buf_size
                              
                              ] 
                              
                              [ 
                              ethernet-type
                              type
                              
                              ] 
                              [ 
                              reeinject-hide
                              ] 
                              [ 
                              
                              packet-length
                              bytes
                              ] 
                              
                              [ 
                              circular-buffer
                              
                              ] 
                              [ 
                              trace
                              
                              [ 
                              trace-count
                              number
                              
                              ] 
                              ] 
                              
                              [ 
                              
                              real-time
                              [ 
                              dump
                              ] 
                              [ 
                              detail
                              ] 
                              ] 
                              
                              [ 
                              
                              match
                              protocol
                              { 
                              
                              host
                              source-ip
                              | 
                              
                              
                              source-ip
                              mask
                              | 
                              
                              any
                              | 
                              any4
                              | 
                              any6
                              } 
                              
                              [ 
                              operator
                              src_port
                              ] 
                              
                              { 
                              
                              host
                              dest_ip
                              
                              | 
                              
                              dest_ip mask
                              | 
                              
                              | 
                              any
                              | 
                              any4
                              | 
                              any6
                              } 
                              
                              [ 
                              operator dest_port
                              ] 
                              ] 
                              
                              
                              [ 
                              switch
                              ] 
                              [ 
                              offload
                              ] 
                              
                              [ 
                              ivlan
                              number
                              ] 
                              
                              
                              [ 
                              ovlan
                              number
                              ] 
                              
                              
                               
                           
                           
                           
                            Capture cluster control-link traffic:
                           
                           
                           
                              
                              capture
                              capture_name
                              {  
                              type lacp
                              interface
                              interface_id
                              [ 
                              buffer
                              buf_size
                              ] 
                              [ 
                              packet-length
                              bytes
                              ] 
                              [ 
                              circular-buffer
                              ] 
                              [ 
                              real-time
                              [ 
                              dump
                              ] 
                              [ 
                              detail
                              ] 
                              ]  
                              
                               
                           
                           
                           
                              
                              capture
                              capture_name
                              interface cluster
                              [ 
                              buffer
                              buf_size
                              ] 
                              [ 
                              
                              ethernet-type
                              type
                              
                              ] 
                              
                              [ 
                              packet-length
                              bytes
                              
                              ] 
                              [ 
                              circular-buffer
                              ] 
                              [ 
                              cp-cluster
                              ] 
                              
                              [ 
                              trace
                              [ 
                              
                              trace-count
                              number
                              ] 
                              ] 
                              
                              
                              [ 
                              
                              real-time
                              [ 
                              dump
                              ] 
                              [ 
                              detail
                              ] 
                              ] 
                              
                              [ 
                              
                              match
                              protocol
                              { 
                              
                              host
                              source-ip
                              | 
                              
                              
                              source-ip
                              mask
                              | 
                              
                              any
                              | 
                              any4
                              | 
                              any6
                              } 
                              
                              [ 
                              operator
                              src_port
                              ] 
                              
                              { 
                              
                              host
                              dest_ip
                              
                              | 
                              
                              dest_ip mask
                              | 
                              
                              | 
                              any
                              | 
                              any4
                              | 
                              any6
                              } 
                              
                              [ 
                              operator dest_port
                              ] 
                              ] 
                              
                              
                              
                               
                           
                           
                           
                           Ingress switch capture packets  for Secure Firewall 3100 model devices:
                           
                           
                           
                              
                              capture
                              capture_name
                              switch
                              interface
                              interface_name
                              
                              [ 
                              drop
                              { 
                              disable
                              | 
                              mac-filter
                              }] 
                              
                              
                               
                           
                           
                           
                           Bi-directional switch packet capture for 1200/4200/6100 model devices:
                           
                           
                           
                           
                              
                              capture
                              capture_name
                              switch
                              interface
                              interface_name
                              [ 
                              direction
                              
                              
                              { { 
                              both
                              | 
                              egress
                              } 
                              [ 
                              drop disable
                              ] | 
                              ingress
                              [ 
                              drop
                              { 
                              disable
                              | 
                              mac-filter
                              }] }] 
                              
                              
                               
                           
                           
                           
                           
                              
                                 |  Note |  For Secure Firewall 4200 model devices, the mac-filter  option is supported only for the ingress  direction.                                          | 
                           
                              
                                 |  Note |  For Secure Firewall 6100 model devices, the drop  option is                                           supported only for the ingress direction.                                          | 
                           
 Capture packets cluster-wide:
                           
                           
                           
                              
                              cluster exec capture
                              capture_name
                              [ 
                              persist
                              ] 
                              [ 
                              include-decrypted
                              ] 
                              
                               
                           
                           
                           
                           Clear persistent packet traces cluster-wide:
                           
                           
                           
                              
                              cluster exec clear packet-trace
                              
                               
                           
                           
                           
                           Remove the packet capture:
                           
                           
                           
                              
                              no capture
                              capture_name
                              [ 
                              arguments
                              ] 
                              
                               
                           
                           
                           
                           Manually stop or start the packet capture:
                           
                           
                           
                              
                              capture
                              capture_name
                              stop
                              
                               
                           
                           
                           
                              
                              no capture
                              capture_name
                              stop
                              
                               
                           
                           
                           
                        
                        
                        
                           Syntax Description
                           
                              
                              
                                 
                                 
                              
                              
                                 
                                 
                                    
                                    | access-list access_list_name                                                |  (Optional) Captures traffic that matches an access list. In multiple context mode, this is only available within a context. | 
                                 
                                 
                                    
                                    | any  |  Specifies all IPv4 traffic. | 
                                 
                                 
                                    
                                    | any4  |  Specifies all IPv4 traffic. | 
                                 
                                 
                                    
                                    | any6  |  Specifies all IPv6 traffic. | 
                                 
                                 
                                    
                                    | all                                                                                     |  Captures all packets dropped by the accelerated security path. | 
                                 
                                 
                                    
                                    | asa_dataplane  |  Captures packets on the ASA backplane that pass between the ASA and a module that uses the backplane, such as the ASA FirePOWER                                           module.                                         | 
                                 
                                 
                                    
                                    | asp-drop                                               drop-code                                                                                    |  (Optional) Captures packets dropped by the accelerated security path. The drop-code specifies the type of traffic that is dropped by the accelerated security path. See the show                                              asp                                              drop                                              frame  command for a list of drop codes. You can enter this keyword with the packet-length , circular-buffer , and buffer                                               keywords, but not with the interface  or ethernet-type  keyword. In a cluster, dropped forwarded data packets from one unit to another are also captured. In multiple context mode,                                           when this option is issued in the system execution space, all dropped data packets are captured; when this option is issued                                           in a context, only dropped data packets that enter from interfaces belonging to the context are captured.                                         | 
                                 
                                 
                                    
                                    | buffer                                            buf_size  |  (Optional) Defines the buffer size used to store the packet in bytes. Once the byte buffer is full, packet capture stops.                                           When used in a cluster, this is the per-unit size, not the sum of all units.                                         | 
                                 
                                 
                                    
                                    | capture_name  |  Specifies the name of the packet capture. Use the same name on multiple capture  statements to capture multiple types of traffic. When you view the capture configuration using the show                                              capture  command, all options are combined on one line.                                         | 
                                 
                                 
                                    
                                    | circular-buffer  | (Optional) Overwrites the buffer, starting from the beginning, when the buffer is full. | 
                                 
                                 
                                    
                                    | cp-cluster  | (Optional) Capture control packets on cluster interface. | 
                                 
                                 
                                    
                                    | direction  | (Optional. Supported on: Secure Firewall 1220, Secure Firewall 4200, Secure Firewall 6100 devices.) Specifies the direction of the switch traffic to be captured. It can be                                           one of the following:                                                                                                                                                                                                                           both —To capture switch bi-directional traffic                                                                                                                                                                                            egress —To capture switch egressing traffic                                                                                                                                                                                            ingress —To capture switch ingressing traffic                                               | 
                                 
                                 
                                    
                                    | drop  | Specifies the packet capture configuration of the mac-filter drop:                                                                                          | Note |                                                                                                                                                                                                                                   For the Secure Firewall 3100, drop  is available when you                                                             select the interface.                                                                                                                                                                                For the Secure Firewall 4200, the drop  keyword is available                                                             only when you select the direction. However, the mac-filter                                                              option is supported only for the ingress packet capture direction.                                                                                                                                                                               For the Secure Firewall 6100, the drop  keyword is available                                                             only for the ingress packet capture direction.                                                           |  | 
                                 
                                 
                                    
                                    | ethernet-type                                               type  |  (Optional) Selects an Ethernet type to capture. Supported Ethernet types include 8021Q, ARP, IP, IP6, LACP, PPPOED, PPPOES,                                           RARP, and VLAN. An exception occurs with the 802.1Q or VLAN type. The 802.1Q tag is automatically skipped and the inner Ethernet                                           type is used for matching.                                         | 
                                 
                                 
                                    
                                    | host                                            ip                                                                                    |  Specifies the single IP address of the host to which the packet is being sent. | 
                                 
                                 
                                    
                                    | include-decrypted  | (Optional) Captures decrypted IPsec packets which contain both normal and decrypted traffic once they enter the firewall device.                                           It also captures packets of SSL decrypted traffic. However, the capture does not include the decrypted packets from VTI because                                           they are available only on the VTI interface and not on the outside interface.                                         | 
                                 
                                 
                                    
                                    | inline-tag                                            tag                                                                                    |  Specifies a tag for a particular SGT value or leaves it unspecified to capture a tagged packet with any SGT value. | 
                                 
                                 
                                    
                                    | interface                                               interface_name                                                                                    |  Sets the name of the interface on which to use packet capture. You must configure an interface for any packets to be captured                                           except for type                                              asp-drop . You can configure multiple interfaces using multiple capture  commands with the same name. To capture packets on the dataplane, management plane, or control plane of an ASA, you can use                                           the interface  keyword with asa_dataplane , asa_mgmt_plane , or cplane                                               as the interface name.You can specify cluster  as the interface name to capture the traffic on the cluster control link interface. If the type lacp  capture is configured, the interface name is the physical name.                                         | 
                                 
                                 
                                    
                                    | ikev1  or ikev2  |  Captures only IKEv1 or IKEv2 protocol information. | 
                                 
                                 
                                    
                                    | isakmp  |  (Optional) Captures ISAKMP traffic for VPN connections. The ISAKMP subsystem does not have access to the upper layer protocols.                                           The capture is a pseudo capture, with the physical, IP, and UDP layers combined together to satisfy a PCAP parser. The peer                                           addresses are obtained from the SA exchange and are stored in the IP layer.                                          | 
                                 
                                 
                                    
                                    | lacp  |  (Optional) Captures LACP traffic. If configured, the interface name is the physical interface name. | 
                                 
                                 
                                    
                                    | mask  |  The subnet mask for the IP address. When you specify a network mask, the method is different from the Cisco IOS software                                           access-list command. The ASA uses a network mask (for example, 255.255.255.0 for a Class C mask). The Cisco IOS mask uses                                           wildcard bits (for example, 0.0.0.255).                                         | 
                                 
                                 
                                    
                                    | match protocol  |  Specifies the packets that match the five-tuple to allow filtering of those packets to be captured. You can use this keyword                                           up to three times on one line.                                         | 
                                 
                                 
                                    
                                    |  operator                                                                                     |  (Optional) Matches the port numbers used by the source or destination. The permitted operators are as follows:                                                                                                                                                                                    lt—less than                                                                                              gt—greater than                                                                                              eq—equal to                                                                                             neq —not equal to                                                                                                                                           range —range                                               | 
                                 
                                 
                                    
                                    | packet-length                                            bytes  |  (Optional) Sets the maximum number of bytes of each packet to store in the capture buffer. | 
                                 
                                 
                                    
                                    | persit                                                                                     |  (Optional) Captures persistent packets on cluter units. | 
                                 
                                 
                                    
                                    | port  |  (Optional) If you set the protocol to tcp or udp, specifies the integer or name of a TCP or UDP port.  | 
                                 
                                 
                                    
                                    | raw-data  |  (Optional) Captures inbound and outbound packets on one or more interfaces.  | 
                                 
                                 
                                    
                                    | real-time  |  Displays the captured packets continuously in real-time. To terminate real-time packet capture, enter Ctrl                                              +                                              c.                                               To permanently remove the capture, use the no  form of this command. This option applies only to raw-data , switch ,  and asp-drop  captures. This option is not supported when you use the cluster                                              exec                                              capture  command.                                         | 
                                 
                                 
                                    
                                    | reinject-hide  |  (Optional) Specifies that no reinjected packets will be captured. Applies only in a clustering environment. | 
                                 
                                 
                                    
                                    | stop  | (Optional) Manually stops the capture without removing it. Use the no  form of this command to start the capture.                                         | 
                                 
                                 
                                    
                                    | tls-proxy  | (Optional) Captures decrypted inbound and outbound data from TLS proxy on one or more interfaces. | 
                                 
                                 
                                    
                                    |                                            trace trace_count  | (Optional) Captures packet trace information, and the number of packets to capture. This option is used with an access list                                           to insert trace packets into the data path to determine whether or not the packet has been processed as expected.                                         | 
                                 
                                 
                                    
                                    | type  |  (Optional) Specifies the type of data captured. | 
                                 
                                 
                                    
                                    | user                                            webvpn-user  |  (Optional) Specifies a username for a WebVPN capture. | 
                                 
                                 
                                    
                                    | webvpn  |  (Optional) Captures WebVPN data for a specific WebVPN connection. | 
                                 
                              
                           
Command Default
                           
                           
                              The defaults are as follows:
                           
                           
                           
                              
                              - 
                                 
                                    The default type  is raw-data 
- 
                                 
                                    The default 
                                    buffer size  is 
                                    512 KB
                                    .
                                    
                                 
- 
                                 
                                    The default Ethernet type is IP packets.
                                 
- 
                                 
                                    The default packet-length  
                                    is 
                                    1518
                                    bytes.
                                    
                                 
- 
                                 
                                 The default direction  is ingress.
                                 
Command Modes
                           
                           
The following table shows the modes in which you can enter the command:
                           
                           
                           
                              
                              
                                 
                                 
                                 
                                 
                                 
                                 
                              
                              
                                 
                                 
                                    
                                    | Command Mode | Firewall Mode | Security Context | 
                                 
                                 
                                    
                                    | Routed | Transparent | Single | Multiple | 
                                 
                                 
                                    
                                    | Context | System | 
                                 
                              
                              
                                 
                                 
                                    
                                    | Privileged EXEC |  |  |  |  |  | 
                                 
                              
                           
Command History
                           
                              
                              
                                 
                                 
                              
                              
                                 
                                 
                                    
                                    |  Release |  Modification | 
                                 
                              
                              
                                 
                                 
                                    
                                    |  6.2(1) |  This command was added. | 
                                 
                                 
                                    
                                    |  7.0(1) |  This command was modified to include the following keywords: type                                              asp-drop , type                                              isakmp , type                                              raw-data , and type                                              webvpn .                                         | 
                                 
                                 
                                    
                                    |  7.0(8) |  Added the all option to capture all packets that the ASA drops. | 
                                 
                                 
                                    
                                    |  7.2(1) |  This command was modified to include the following options: trace                                               trace_count, match                                            prot, real-time,                                                                                          host                                            ip, any ,                                               mask,  and operator.                                                                                    | 
                                 
                                 
                                    
                                    |  8.0(2) |  This command was modified to update the path to capture contents. | 
                                 
                                 
                                    
                                    |  8.4(1) |  The new type keywords ikev1 and ikev2 were added. | 
                                 
                                 
                                    
                                    |  8.4(2) |  Additional detail was added to the output for IDS. | 
                                 
                                 
                                    
                                    |  8.4(4.1) |  The asa_dataplane  option was added to support traffic over the backplane to the ASA CX module.                                         | 
                                 
                                 
                                    
                                    |  9.0(1) |  The cluster,                                              cluster                                              exec,                                               and reinject-hide  keywords were added. The new                                              type  option lacp  was added. Support for multiple-context mode was added for ISAKMP.                                         | 
                                 
                                 
                                    
                                    |  9.1(3) |  Supports filtering of packets captured on the ASA CX backplane with the asa_dataplane  option.                                         | 
                                 
                                 
                                    
                                    |  9.2(1) |  The asa_dataplane  option was extended to support the ASA FirePOWER module.                                         | 
                                 
                                 
                                    
                                    |  9.3(1) |  The inline-tag                                            tag keyword-argument pair was added to support the SGT plus Ethernet Tagging feature.                                         | 
                                 
                                 
                                    
                                    |  9.6(2) |  Packet capture of type                                              asp-drop                                               supports ACL and match filtering.                                         | 
                                 
                                 
                                    
                                    |  9.7(1) |  Added the stop  keyword to manually stop and start the packet capture.                                         | 
                                 
                                 
                                    
                                    |  9.8(1) |  This command was updated to store the contents of all the active captures to files on flash or disks at the time of box crash. | 
                                 
                                 
                                    
                                    |  9.9(1) |  Support for capturing clustering persistent tracing and decrypted packets. New options were added: persist  and include-decrypted .                                          In addition, the ethernet-type                                              ipx  was removed, because IPX corresponds to 3 separate ethernet-types. Instead, use the hexadecimal value of the IPX type you                                           want to capture.                                         | 
                                 
                                 
                                    
                                    |  9.10(1) |  Added the any4 and any6 keywords for the match  option to capture IPv4 and IPv6 network traffic respectively.                                         | 
                                 
                                 
                                    
                                    |  9.12(1) |  Added cp-cluster                                               to capture control packets on cluster interface.                                         | 
                                 
                                 
                                    
                                    | 9.18(1) |  Included real-time                                                keyword to enable real-time switch packet capture.                                         | 
                                 
                                 
                                    
                                    | 9.20(1) | The direction keyword was added to capture switch traffic that flows in egress, ingress, or both directions. This keyword is applicable only for Secure Firewall 4200 model devices.  | 
                                 
                                 
                              
                           
Usage Guidelines
                           
                           
                              Capturing packets is useful when troubleshooting connectivity problems or monitoring suspicious activity. You can create multiple
                              captures. The capture  command is not saved to the running configuration, and is not copied to the standby unit during failover.
                           
                           
                           
                              The ASA is capable of tracking all IP traffic that flows across it and of capturing all the IP traffic that is destined to
                              it, including all the management traffic (such as SSH and Telnet traffic). 
                           
                           
                           
                              The ASA architecture consists of three different sets of processors for packet processing; this architecture poses certain
                              restrictions on the capability of the capture feature. Typically most of the packet forwarding functionality in the ASA is
                              handled by the two front-end network processors, and packets are sent to the control-plane general-purpose processor only
                              if they need application inspection. The packets are sent to the session management path network processor only if there is
                              a session miss in the accelerated path processor. 
                           
                           
                           
                              Because all the packets that are forwarded or dropped by the ASA hits the two front-end network processors, the packet capture
                              feature is implemented in these network processors. So all the packets that hit the ASA can be captured by these front end
                              processors, if an appropriate capture is configured for those traffic interfaces. On the ingress side, the packets are captured
                              the moment the packet hits the ASA interfaces, and on the egress side the packets are captured just before they are sent out
                              on the wire. 
                           
                           
                           
                              
                                 |  Note |  Enabling WebVPN capture affects the performance of the ASA. Be sure to disable the capture after you generate the capture                                           files that you need for troubleshooting.                                          | 
                           
                                 Save the Capture
                           
                           
                              The contents of any active capture on ASA are saved when the box crashes. 
                           
                           
                           
                              When you activate captures as part of the troubleshooting process, you must note the following points:
                           
                           
                           
                              
                              - 
                                 
                                    The size of capture buffer to use and if there is enough space on flash/disk.
                                 
- 
                                 
                                    The capture buffer should be marked as circular for all the use cases, so that captured packets are the most recent before
                                    crash.
                                 
                              The name of the file for saving contents of an active capture is in the format of:
                           
                           
                           
                              [<context_name>.]<capture_name>.pcap
                              
                           
                           
                           
                              The context_name
                              indicates the name of the user context in which capture is activated in the multi-context mode. For the single context mode,
                              the context_name
                              is not applicable.
                           
                           
                           
                              The capture_name
                              indicates the name of the capture that is activated.
                           
                           
                           
                              The capture save happens before the console or crash dump. This increases the crash downtime by about 5 seconds for a 33 MB
                              capture buffer. The risk of a nested crash is minimal because copying the captured contents to a file is a simple process.
                           
                           
                           
                                 View the Capture
                           
                           
                              
                              - 
                                 
                                 To view the packet capture at the CLI, use the show
                                       capture 
                                    name command.
                                 
- 
                                 
                                 To save the capture to a file, use the copy
                                       capture  command. 
                                 
- 
                                 
                                 To see the packet capture information with a web browser, use the
                                    https://ASA-ip-address/admin/capture/capture_name[/pcap] 
                                    command. 
                                  You are prompted for a username and password. See the username 
                                    command to add a username to the local database.
                                  If you specify the pcap  keyword, then a libpcap-format file is
                                    downloaded to the web browser and can be saved using the web browser. (A libcap file can be
                                    viewed with TCPDUMP or Ethereal.)
                                 
If you copy the buffer contents to a TFTP server in ASCII format, you will see only the
                              headers, not the details and hexadecimal dump of the packets. To see the details and hexadecimal
                              dump, you need to transfer the buffer in PCAP format and read it with TCPDUMP or Ethereal.
                           
                           
                           
                                 Stop and Start the Capture
                           
                           
                              The packets can be stopped from being captured without removing them from the buffer. The stopped status of the capture is
                              displayed. The captured packet is retained in the buffer.
                           
                           
                           
                              Use the following command to manually stop packet capture:
                           
                           
                           capture  name
                              stop 
                           
                           
                              Use the following command to start capturing packets:
                           
                           
                           no capture  name
                              stop 
                           
                           
                                 Delete the Capture
                           
                           
                              Entering no capture  without any keywords deletes the capture. To preserve the capture, specify the access-list  or interface  keyword; the capture is detached from the specified ACL or interface and the capture is preserved.
                           
                           
                           
                                 Real Time Operations
                           
                           
                              You cannot perform any operations on a capture while the real-time display is in progress. Using the real-time  keyword with a slow console connection may result in an excessive number of non-displayed packets because of performance
                              considerations. The fixed limit of the buffer is 1000 packets. If the buffer fills up, a counter is maintained of the captured
                              packets. If you open another session, you can disable the real-time display be entering the no capture real-time  command.
                           
                           
                           
                                 Clustering
                           
                           
                              You can precede the capture  command with cluster exec  to issue the capture  command on one unit and run the command in all the other units at the same time. After you have performed cluster-wide capture,
                              to copy the same capture file from all units in the cluster at the same time to a TFTP server, enter the cluster exec copy  command on the master unit.
                           
                           
ciscoasa# cluster exec capture 
capture_name arguments
ciscoasa# cluster exec copy
 /pcap capture
: cap_name
 tftp
://location
/path
/filename
.pcap
                           
                              Multiple PCAP files, one from each unit, are copied to the TFTP server. The destination capture file name is automatically
                              attached with the unit name, such as filename_A.pcap, filename_B.pcap, and so on. In this example, A and B are cluster unit
                              names.
                           
                           
                           
                              When you capture traces on cluster units, they are persistent on each cluster node until you manually clear them from the
                              buffer. Decrypted IPsec packets are captured once they enter ASA. The captured packet includes both normal and decapsulated
                              traffic.
                           
                           
                           
                              
                                 |  Note |  A different destination name is generated if you add the unit name at the end of the filename.  | 
                           
                                 Limitations
                           
                           
                              The following are some of the limitations of the capture feature. Most of the limitations are caused by the distributed nature
                              of the ASA architecture and by the hardware accelerators that are being used in the ASA. 
                           
                           
                           
                              
                              - 
                                 
                                    You can configure captures on the cluster control link within a context; only the packet that is associated with the context
                                    sent in the cluster control link is captured.
                                 
- 
                                  For a shared VLAN, the following guidelines apply:  
                                    
                                    
  - 
                                       
                                        You can only configure one capture for the VLAN; if you configure a capture in multiple contexts on the shared VLAN, then
                                          only the last capture that was configured is used. 
                                       
  - 
                                       
                                        If you remove the last-configured (active) capture, no captures become active, even if you have previously configured a capture
                                          in another context; you must remove and readd the capture to make it active. 
                                       
  - 
                                       
                                        All traffic that enters the interface to which the capture is attached (and that matches the capture access list) is captured,
                                          including traffic to other contexts on the shared VLAN. 
                                       
  - 
                                       
                                        Therefore, if you enable a capture in Context A for a VLAN that is also used by Context B, both Context A and Context B ingress
                                          traffic are captured. 
                                       
- 
                                 
                                    For egress traffic, only the traffic of the context with the active capture is captured. The only exception is when you do
                                    not enable the ICMP inspection (therefore the ICMP traffic does not have a session in the accelerated path). In this case,
                                    both ingress and egress ICMP traffic for all contexts on the shared VLAN is captured. 
                                 
- 
                                 
                                    Configuring a capture typically involves configuring an access list that matches the traffic that needs to be captured. After
                                    an access list that matches the traffic pattern is configured, then you need to define a capture and associate this access
                                    list to the capture, along with the interface on which the capture needs to be configured. Note that a capture only works
                                    if an access list and an interface are associated with a capture for capturing IPv4 traffic. The access list is not required
                                    for IPv6 traffic.
                                 
- 
                                 
                                    For the ASA CX module traffic, captured packets contain an additional AFBP header that your PCAP viewer might not understand;
                                    be sure to use the appropriate plugin to view these packets.
                                 
- 
                                 
                                    For inline SGT tagged packets, captured packets contain an additional CMD header that your PCAP viewer might not understand.
                                 
- 
                                 
                                 When performing a switch packet capture on Secure Firewall 
                                    SGT-tagged packets are not captured if you apply a filter for SGT-tagged packets. However, SGT traffic can be captured on
                                    an interface if no filtering is applied.
                                 
- 
                                 
                                    If there is no ingress interface and therefore no global interface, packets sent on the backplane are treated as control packets
                                    in the system context. These packets bypass the access list check and are always captured. This behavior applies in both single
                                    mode and multiple context mode.
                                 
- 
                                 
                                    The show capture  command shows the correct reason when capturing a specific asp-drop. However, the show capture  command does not show the correct reason when capturing all asp-drops.
                                 
- 
                                 
                                 The switch packet capture feature for Secure Firewall 6100 series has these limitations:
- 
                                 
                                 The switch packet capture feature for Secure Firewall 4200 and Secure Firewall 6100 series has these limitations: 
                                    
                                    
  - 
                                       
                                       The Marvell 98CX8540 switch used in 4200 and 6100 Series device has a hardware limitation. Due to this limitation IPv6 packets
                                          with extension headers cannot be captured using egress switch packet capture when filters are based on IP protocol or TCP/UDP
                                          port ranges.
                                        This is because the egress processing is unaware of the Layer 4 protocol type.
Examples
                           
                           
                              To capture a packet, enter the following command:
                           
                           
ciscoasa# capture captest interface inside
ciscoasa# capture captest interface outside
                           
                              On a web browser, you can view the content of the capture  command that was issued, named “captest,” at the following location:
                           
                           
https://171.69.38.95/admin/capture/captest
                           
                              To download a libpcap file (that web browsers use) to a local machine, enter the following command:
                           
                           
https://171.69.38.95/capture/http/pcap
                           
                              The following example shows how to capture a packet in the single-mode when the ASA box crashes:
                           
                           
ciscoasa# capture 123 interface inside
                           
                              The contents of capture ‘123’ is saved as 123.pcap
                              file.
                           
                           
                           
                              The following example shows how to capture a packet in the multi-mode when the ASA box crashes:
                           
                           
ciscoasa# capture 456 interface inside
                           
                              The contents of capture ‘456’ in ‘admin’ context is saved as admin.456.pcap
                              file.
                           
                           
                           
                              The following example shows that the traffic is captured from an outside host at 171.71.69.234 to an inside HTTP server:
                           
                           
ciscoasa# access-list http permit tcp host 10.120.56.15 eq http host 171.71.69.234
ciscoasa# access-list http permit tcp host 171.71.69.234 host 10.120.56.15 eq http
ciscoasa# capture http access-list http packet-length 74 interface inside
                           
                              The following example shows how to capture ARP packets:
                           
                           
ciscoasa# capture arp ethernet-type arp interface outside
                           
                              The following example inserts five tracer packets into the data stream, where access-list 101  defines traffic that matches TCP protocol FTP:
                           
                           
hostname# capture ftptrace interface outside access-list 101 trace 5
                           
                              To view the traced packets and information about packet processing in an easily readable manner, use the show capture ftptrace  command.
                           
                           
                           
                              The following example shows how to display captured packets in real-time:
                           
                           
ciscoasa# capture test interface outside real-time
Warning: Using this option with a slow console connection may result in an excess amount of non-displayed packets due to performance limitations.
Use ctrl-c to terminate real-time capture.
10 packets displayed
12 packets not displayed due to performance limitations
                           
                              The following example shows how to configure an extended access list that matches the IPv4 traffic that needs to be captured:
                           
                           
ciscoasa (config)# access-list capture extended permit ip any any
                           
                              The following examples shows how to configure the capture:
                           
                           
ciscoasa (config)# capture name access-list acl_name interface interface_name
                           
                              By default, configuring a capture creates a linear capture buffer of size 512 KB. You can optionally configure a circular
                              buffer. By default, only 68 bytes of the packets are captured in the buffer. You can optionally change this value.
                           
                           
                           
                              The following example creates a capture called “ip-capture” using the capture access list previously configured that is applied
                              to the outside interface: 
                           
                           
ciscoasa (config)# capture ip-capture access-list capture interface outside
                            The following example creates a capture called “switch-capture” on outside interface for Secure Firewall 3100: 
                           
ciscoasa (config)# capture switch-capture switch interface outside drop ?
exec mode commands/options:
  disable Disable capturing dropped packets from switch
  mac-filter To capture switch mac-filter drop
ciscoasa(config)# capture switch-capture switch interface outside drop mac-filter
                           
                              The following example shows how to view the capture:
                           
                           
ciscoasa (config)# show capture name
                           
                              The following example shows how to end the capture, but retain the buffer:
                           
                           
ciscoasa (config)# no capture name access-list acl_name interface interface_name
                           
                              The following example shows how to end the capture and delete the buffer:
                           
                           
ciscoasa (config)# no capture name
                           
                              The following example shows how to filter traffic captured on the backplane in single mode:
                           
                           
ciscoasa# capture x interface asa_dataplane access-list any4
ciscoasa# capture y interface asa_dataplane match ip any any
                           
                              
                                 |  Note |  Control packets are captured in the single mode even though you have specified the access list.  | 
                           
                              The following examples show how to filter traffic captured on the backplane in multiple context mode:
                           
                           
                           
                              Usage in user context:
                           
                           
ciscoasa (contextA)# capture x interface asa_dataplane access-list any4
ciscoasa (contextA)# capture y interface asa_dataplane match ip any any
                           
                              Usage in system context:
                           
                           
ciscoasa# capture z interface asa_dataplane
                           
                              
                                 |  Note |  In multiple context mode, the access-list  and match  options are not available in the system context.                                          | 
                           
Examples
                           
                           
                           
                              Capture for Clustering
                           
                           
                           
                              To enable capture on all units in the cluster, you can add the cluster exec  keywords in front of each of these commands. 
                           
                           
                           
                              The following example shows how to create an LACP capture for the clustering environment:
                           
                           
ciscoasa (config)# capture lacp type lacp interface gigabitEthernet0/0
                           
                              The following example shows how to create a capture for control path packets in the clustering link:
                           
                           
ciscoasa (config)# cap cp interface cluster match udp any eq 49495 any 
ciscoasa (config)# cap cp interface cluster match udp any any eq 49495
                           
                              The following example shows how to create a capture for data path packets in the clustering link:
                           
                           
ciscoasa (config)# access-list cc1 extended permit udp any any eq 4193
ciscoasa (config)# access-list cc1 extended permit udp any eq 4193 any
ciscoasa (config)# capture dp interface cluster access-list ccl
                           
                              The following example shows how to capture data path traffic through the cluster:
                           
                           
ciscoasa (config)# capture abc interface inside match tcp host 1.1.1.1 host 2.2.2.2 eq www
ciscoasa (config)# capture abc interface inside match dup host 1.1.1.1 any
ciscoasa (config)# capture abc interface inside access-list xxx
                           
                              The following example shows how to capture logical update messages for flows that match the real source to the real destination,
                              and capture packets forwarded over CCL that match the real source to the real destination:
                           
                           
ciscoasa (config)# access-list dp permit
 real src real dst
                           
                              The following example shows how to capture a certain type of data plane message, such as icmp echo request/response, that
                              is forwarded from one ASA to another ASA using the match  keyword or the access list for the message type: 
                           
                           
ciscoasa (config)# capture capture_name interface cluster access-list match icmp any any 
                           
                              The following example shows how to create a capture by using access list 103 on a cluster control link in a clustering environment:
                           
                           
ciscoasa (config)# access-list 103 permit ip A B
ciscoasa (config)# capture example1 interface cluster 
                           
                              In the previous example, if A and B are IP addresses for the CCL interface, only the packets that are sent between these two
                              units are captured.
                           
                           
                           
                              If A and B are IP addresses for through-device traffic, then the following is true:
                           
                           
                           
                              
                              - 
                                 
                                    Forwarded packets are captured as usual, provided the source and destination IP addresses are matched with the access list.
                                 
- 
                                 
                                    The data path logic update message is captured provided it is for the flow between A and B or for an access list (for example,
                                    access-list 103). The capture matches the five-tuple of the embedded flow.
                                 
- 
                                 
                                    Although the source and destination addresses in the UDP packet are CCL addresses, if this packet is to update a flow that
                                    is associated with addresses A and B, it is also captured. That is, as long as addresses A and B that are embedded in the
                                    packet are matched, it is also captured.
                                 
                              The following example shows how to configure capture with persistent option:
                           
                           
cluster2-asa5585a(config)# cluster exec capture test interface outside trace persist 
  a(LOCAL):*************************************************************
  cluster2-asa5585a(config)#
                           
                              Now, you can send some traffic.
                           
                           
cluster2-asa5585a(config)# cluster exec show packet-tracer
 
  a(LOCAL):*************************************************************
  tracer 29/25 (allocate/freed), handle 29/25 (allocated/freed), error 0
  =======  Tracer origin-id a:23, hop 0 =======
  packet-id: Protocol: 0 src-port: 0 dst-port: 0
  Phase: 1
  Type: CAPTURE
  Subtype: 
  Result: ALLOW
  Config:
  Additional Information:
  MAC Access list
  Phase: 2
  Type: ACCESS-LIST
  Subtype: 
  Result: DROP
  Config:
  Implicit Rule
  Additional Information:
  MAC Access list
  Result:
  input-interface: outside
  input-status: up
  input-line-status: up
  Action: drop
  Drop-reason: (l2_acl) FP L2 rule drop
                           
                              The following example shows that, to free up some memory you must clear the captured persistent traces from the box.
                           
                           
ciscoasa# cluster exec clear packet-trace
                           
                              The following example displays how to configure the capture with include-decrypted option:
                           
                           
  cluster2-asa5585a(config)# cluster exec show capture
 
  a(LOCAL):*************************************************************
  capture in type raw-data trace interface outside include-decrypted [Capturing – 588 bytes] 
  capture out type raw-data trace interface outside include-decrypted [Capturing - 420 bytes] 
  cluster2-asa5585a(config)#
                           
                              Now, you can send some ICMP traffic through IPSec tunnel. The capture command obtains the decrypted ICMP packets as outlined:
                           
                           
  cluster2-asa5585a(config)# cluster exec show capture in | i icmp
  a(LOCAL):*************************************************************
  b:********************************************************************
  cluster2-asa5585a(config)# cluster exec show capture out | i icmp
  a(LOCAL):*************************************************************  
  b:********************************************************************
  cluster2-asa5585a(config)# cluster exec show capture in | i icmp
  a(LOCAL):*************************************************************
     8: 07:22:57.065014       802.1Q vlan#212 P0 211.1.1.1 > 213.1.1.2: icmp: echo request   
  b:********************************************************************
  cluster2-asa5585a(config)# cluster exec show capture out | i icmp
  a(LOCAL):*************************************************************
    10: 07:22:57.068004       802.1Q vlan#214 P0 213.1.1.2 > 211.1.1.1: icmp: echo reply
  b:********************************************************************
  cluster2-asa5585a(config)#
                           The following example shows how to create and start an egress traffic capture for a switch: 
                           
ciscoasa(config)# capture switch_cap switch interface gigabitEthernet0/0 direction ?
exec mode commands/options:
  both     To capture switch bi-directional traffic
  egress   To capture switch egressing traffic
  ingress  To capture switch ingressing traffic
 
ciscoasa(config)# capture switch_cap switch interface gigabitEthernet0/0 direction egress
ciscoasa(config)# no capture switch_cap switch stop
                           
                           If you want to capture packets where the accelerated security path (ASP) drop reason is dispatch-queue-limit, you need to
                              enable debug to delay the packet drops. Otherwise, the packets have been dropped and they cannot be captured. If you have
                              used show asp drop  command and see that dispatch queue tail drops (dispatch-queue-limit) are increasing, enable debug on asp 38 before performing
                              the capture. Do the following: 
                           
                           
(Enable delayed drop for 20 packets. You can specify 0-100. 
When the delayed drop count hits zero, the delayed drop 
behavior stops automatically. You can see how many drops 
are still available using debug menu asp 38 with no number)
ciscoasa(config)# debug menu asp 38 20
(Start the capture)
ciscoasa(config)# capture drop type asp-drop dispatch-queue-limit
(Generate the traffic that has been causing the drops, then view the capture.)
ciscoasa(config)# show capture drop