Security Fabric over IPsec VPN
                                                
                                            
                                            This is an example of configuring Security Fabric over IPsec VPN.
Sample topology
This sample topology shows a downstream FortiGate (HQ2) connected to the root FortiGate (HQ1) over IPsec VPN to join Security Fabric.
Sample configuration
To configure the root FortiGate (HQ1):
- 
                                                    Configure the interface: 
  - 
                                                            Go to Network > Interfaces.
  - 
                                                            Edit port2: 
    - 
                                                                    Set Role to WAN.
    - 
                                                                    For the interface connected to the internet, set the IP/Network Mask to 10.2.200.1/255.255.255.0
  - 
                                                                    
  - 
                                                            Edit port6: 
    - 
                                                                    Set Role to DMZ.
    - 
                                                                    For the interface connected to FortiAnalyzer, set the IP/Network Mask to 192.168.8.250/255.255.255.0
  - 
                                                                    
- 
                                                            
- 
                                                    Configure the static route to connect to the internet: 
  - 
                                                            Go to Network > Static Routes and click Create New or Create New > IPv4 Static Route. 
    - 
                                                                    Set Destination to 0.0.0.0/0.0.0.0.
    - 
                                                                    Set Interface to port2.
    - 
                                                                    Set Gateway Address to 10.2.200.2.
  - 
                                                                    
  - 
                                                            Click OK.
- 
                                                            
- 
                                                    Configure the IPsec VPN: 
  - 
                                                            Go to VPN > IPsec Wizard. 
    - 
                                                                    Set Name to To-HQ2.
    - 
                                                                    Set Template Type to Custom.
    - 
                                                                    Click Next.
    - 
                                                                    Set Authentication to Method.
    - 
                                                                    Set Pre-shared Key to 123456.
  - 
                                                                    
  - 
                                                            Leave all other fields in their default values and click OK.
- 
                                                            
- 
                                                    Configure the IPsec VPN interface IP address which will be used to form Security Fabric: 
  - 
                                                            Go to Network > Interfaces.
  - 
                                                            Edit To-HQ2: 
    - 
                                                                    Set Role to LAN.
    - 
                                                                    Set the IP/Network Mask to 10.10.10.1/255.255.255.255.
    - 
                                                                    Set Remote IP/Network Mask to 10.10.10.3/255.255.255.0.
  - 
                                                                    
- 
                                                            
- 
                                                    Configure the IPsec VPN local and remote subnets: 
  - 
                                                            Go to Policy & Objects > Addresses.
  - 
                                                            Click Create New 
    - 
                                                                    Set Name to To-HQ2_remote_subnet_2.
    - 
                                                                    Set Type to Subnet.
    - 
                                                                    Set IP/Network Mask to 10.10.10.3/32.
  - 
                                                                    
  - 
                                                            Click OK.
  - 
                                                            Click Create New 
    - 
                                                                    Set Name to To-HQ2_local_subnet_1.
    - 
                                                                    Set Type to Subnet.
    - 
                                                                    Set IP/Network Mask to 192.168.8.0/24.
  - 
                                                                    
  - 
                                                            Click OK.
  - 
                                                            Click Create New 
    - 
                                                                    Set Name to To-HQ2_remote_subnet_1.
    - 
                                                                    Set Type to Subnet.
    - 
                                                                    Set IP/Network Mask to 10.1.100.0/24.
  - 
                                                                    
  - 
                                                            Click OK.
- 
                                                            
- 
                                                    Configure the IPsec VPN static routes: 
  - 
                                                            Go to Network > Static Routes.
  - 
                                                            Click Create New or Create New > IPv4 Static Route. 
    - 
                                                                    For Named Address, select Type and select To-HQ2_remote_subnet_1.
    - 
                                                                    Set Interface to To-HQ2.
 Click OK.
  - 
                                                                    
  - 
                                                            Click Create New or Create New > IPv4 Static Route. 
    - 
                                                                    For Named Address, select Type and select To-HQ2_remote_subnet_1.
    - 
                                                                    Set Interface to Blackhole.
    - 
                                                                    Set Administrative Distance to 254.
  - 
                                                                    
  - 
                                                            Click OK.
- 
                                                            
- 
                                                    Configure the IPsec VPN policies: 
  - 
                                                            Go to Policy & Objects > Firewall Policy
  - 
                                                            Click Create New. 
    - 
                                                                    Set Name to vpn_To-HQ2_local.
    - 
                                                                    Set Incoming Interface to port6.
    - 
                                                                    Set Outgoing Interface to To-HQ2.
    - 
                                                                    Set Source to To-HQ2_local_subnet_1.
    - 
                                                                    Set Destination to To-HQ2_remote_subnet_1.
    - 
                                                                    Set Schedule to Always.
    - 
                                                                    Set Service to All.
    - 
                                                                    Disable NAT.
  - 
                                                                    
  - 
                                                            Click OK.
  - 
                                                            Click Create New. 
    - 
                                                                    Set Name to vpn_To-HQ2_remote.
    - 
                                                                    Set Incoming Interface to To-HQ2.
    - 
                                                                    Set Outgoing Interface to port6.
    - 
                                                                    Set Source to To-HQ2_remote_subnet_1, To-HQ2_remote_subnet_2.
    - 
                                                                    Set Destination to To-HQ2_local_subnet_1.
    - 
                                                                    Set Schedule to Always.
    - 
                                                                    Set Service to All.
    - 
                                                                    Enable NAT.
    - 
                                                                    Set IP Pool Configuration to Use Outgoing Interface Address.
  - 
                                                                    
  - 
                                                            Click OK.
- 
                                                            
- 
                                                    Configure the Security Fabric: 
  - 
                                                            Go to Security Fabric > Fabric Connectors and double-click the Security Fabric Setup card.
  - 
                                                            Select the Settings tab, and set the Security Fabric role to Serve as Fabric Root.
  - 
                                                            Enter a Fabric name, such as Office-Security-Fabric.
  - 
                                                            Ensure Allow other Security Fabric devices to join is enabled and add VPN interface To-HQ2.
  - 
                                                            Click OK.
- 
                                                            
- 
                                                    Configure the FortiAnalyzer logging settings: 
  - 
                                                            Go to Security Fabric > Fabric Connectors and double-click the Logging & Analytics card.
  - 
                                                            Select the Settings tab, select the FortiAnalyzer tab, and set the Status to Enabled.
  - 
                                                            Enter the FortiAnalyzer IP in the Server field (192.168.8.250). The Upload option is automatically set to Real Time.
  - 
                                                            Click Refresh. The FortiAnalyzer serial number is verified.
  - 
                                                            Click OK.
- 
                                                            
To configure the downstream FortiGate (HQ2):
- 
                                                    Configure the interface: 
  - 
                                                            Go to Network > Interfaces.
  - 
                                                            Edit interface wan1: 
    - 
                                                                    Set Role to WAN.
    - 
                                                                    For the interface connected to the internet, set the IP/Network Mask to 192.168.7.3/255.255.255.0.
  - 
                                                                    
  - 
                                                            Edit interface vlan20: 
    - 
                                                                    Set Role to LAN.
    - 
                                                                    For the interface connected to local endpoint clients, set the IP/Network Mask to 10.1.100.3/255.255.255.0.
  - 
                                                                    
- 
                                                            
- 
                                                    Configure the static route to connect to the internet: 
  - 
                                                            Go to Network > Static Routes and click Create New or Create New > IPv4 Static Route. 
    - 
                                                                    Set Destination to 0.0.0.0/0.0.0.0.
    - 
                                                                    Set Interface to wan1.
    - 
                                                                    Set Gateway Address to 192.168.7.2.
  - 
                                                                    
  - 
                                                            Click OK.
- 
                                                            
- 
                                                    Configure the IPsec VPN: 
  - 
                                                            Go to VPN > IPsec Wizard. 
    - 
                                                                    Set VPN Name to To-HQ1.
    - 
                                                                    Set Template Type to Custom.
    - 
                                                                    Click Next.
    - 
                                                                    In the Network IP Address, enter 10.2.200.1.
    - 
                                                                    Set Interface to wan1.
    - 
                                                                    Set Authentication to Method.
    - 
                                                                    Set Pre-shared Key to 123456.
  - 
                                                                    
  - 
                                                            Leave all other fields in their default values and click OK.
- 
                                                            
- 
                                                    Configure the IPsec VPN interface IP address which will be used to form Security Fabric: 
  - 
                                                            Go to Network > Interfaces.
  - 
                                                            Edit To-HQ1: 
    - 
                                                                    Set Role to WAN.
    - 
                                                                    Set the IP/Network Mask to 10.10.10.3/255.255.255.255.
    - 
                                                                    Set Remote IP/Network Mask to 10.10.10.1/255.255.255.0.0.
  - 
                                                                    
- 
                                                            
- 
                                                    Configure the IPsec VPN local and remote subnets: 
  - 
                                                            Go to Policy & Objects > Addresses.
  - 
                                                            Click Create New 
    - 
                                                                    Set Name to To-HQ1_local_subnet_1.
    - 
                                                                    Set Type to Subnet.
    - 
                                                                    Set IP/Network Mask to 10.1.100.0/24.
  - 
                                                                    
  - 
                                                            Click OK.
  - 
                                                            Click Create New 
    - 
                                                                    Set Name to To-HQ1_remote_subnet_1.
    - 
                                                                    Set Type to Subnet.
    - 
                                                                    Set IP/Network Mask to 192.168.8.0/24.
  - 
                                                                    
  - 
                                                            Click OK.
- 
                                                            
- 
                                                    Configure the IPsec VPN static routes: 
  - 
                                                            Go to Network > Static Routes and click Create New or Create New > IPv4 Static Route. 
    - 
                                                                    For Named Address, select Type and select To-HQ1_remote_subnet_1.
    - 
                                                                    Set Interface to To-HQ1.
  - 
                                                                    
  - 
                                                            Click OK.
  - 
                                                            Click Create New or Create New > IPv4 Static Route. 
    - 
                                                                    For Named Address, select Type and select To-HQ1_remote_subnet_1.
    - 
                                                                    Set Interface to Blackhole.
    - 
                                                                    Set Administrative Distance to 254.
  - 
                                                                    
  - 
                                                            Click OK.
- 
                                                            
- 
                                                    Configure the IPsec VPN policies: 
  - 
                                                            Go to Policy & Objects > Firewall Policy and click Create New. 
    - 
                                                                    Set Name to vpn_To-HQ1_local.
    - 
                                                                    Set Incoming Interface to vlan20.
    - 
                                                                    Set Outgoing Interface to To-HQ1.
    - 
                                                                    Set Source to To-HQ1_local_subnet_1.
    - 
                                                                    Set Destination to To-HQ1_remote_subnet_1.
    - 
                                                                    Set Schedule to Always.
    - 
                                                                    Set Service to All.
    - 
                                                                    Disable NAT.
  - 
                                                                    
  - 
                                                            Click OK.
  - 
                                                            Click Create New. 
    - 
                                                                    Set Name to vpn_To-HQ1_remote.
    - 
                                                                    Set Incoming Interface to To-HQ1.
    - 
                                                                    Set Outgoing Interface to vlan20.
    - 
                                                                    Set Source to To-HQ1_remote_subnet_1.
    - 
                                                                    Set Destination to -HQ1_local_subnet_1.
    - 
                                                                    Set Schedule to Always.
    - 
                                                                    Set Service to All.
    - 
                                                                    Disable NAT.
  - 
                                                                    
  - 
                                                            Click OK.
- 
                                                            
- 
                                                    Configure the Security Fabric: 
  - 
                                                            Go to Security Fabric > Fabric Connectors and double-click the Security Fabric Setup card.
  - 
                                                            In the Settings tab, set the Security Fabric role to Join Existing Fabric. FortiAnalyzer automatically enables logging. FortiAnalyzer settings will be retrieved when the downstream FortiGate connects to the root FortiGate.
  - 
                                                            Set the Upstream FortiGate IP to 10.10.10.1.
  - 
                                                            Click OK.
- 
                                                            
To authorize the downstream FortiGate (HQ2) on the root FortiGate (HQ1):
- 
                                                    In the root FortiGate (HQ1), go to System > Firmware & Registration. The table highlights the connected FortiGate with its serial numbers that is unauthorized.
- 
                                                    Select the unauthorized device and click Authorization > Authorize. After authorization, the downstream FortiGate (HQ2) appears in the Security Fabric widget. This means the downstream FortiGate (HQ2) has successfully joined the Security Fabric.
To check the Security Fabric over IPsec VPN:
- 
                                                    On the root FortiGate (HQ1), go to Security Fabric > Physical Topology. The root FortiGate (HQ1) is connected by the downstream FortiGate (HQ2) with VPN icon in the middle.
- 
                                                    On the root FortiGate (HQ1), go to Security Fabric > Logical Topology. The root FortiGate (HQ1) VPN interface To-HQ2 is connected by downstream FortiGate (HQ2) VPN interface To-HQ1 with VPN icon in the middle.
To run diagnostics:
- 
                                                    To view the downstream FortiGate pending authorization on root FortiGate (HQ1): HQ1 # diagnose sys csf authorization pending-list Serial IP Address HA-Members Path ------------------------------------------------------------------------------------ FG101ETK18002187 0.0.0.0 FG3H1E5818900718:FG101ETK18002187
- 
                                                    To view the downstream FortiGate (HQ2) after it joins the Security Fabric: HQ1 # diagnose sys csf downstream
 1:     FG101ETK18002187 (10.10.10.3) Management-IP: 0.0.0.0 Management-port:0 parent: FG3H1E5818900718
        path:FG3H1E5818900718:FG101ETK18002187
        data received: Y downstream intf:To-HQ1 upstream intf:To-HQ2 admin-port:443
        authorizer:FG3H1E5818900718
- 
                                                    To view the root FortiGate (HQ1) on the downstream FortiGate (HQ2) after joining the Security Fabric: HQ2 # diagnose sys csf upstream Upstream Information: Serial Number:FG3H1E5818900718 IP:10.10.10.1 Connecting interface:To-HQ1 Connection status:Authorized