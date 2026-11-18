Welcome to the **EVE-NG Labs Catalog**! This repository is a comprehensive collection of network simulation labs designed for network engineers, students, and professionals who want to enhance their skills in routing, switching, and firewall technologies. These labs utilize EVE-NG (Emulated Virtual Environment - Next Generation) to provide a hands-on, practical learning experience.

- **Description:** Dive into dynamic and static routing. This section includes labs on OSPF, EIGRP, BGP, and more.
- **Devices:** Cisco Routers & Switches (various models)

- **Description:** Learn to secure networks with industry-leading firewall technologies. This section includes labs on:
  - **Cisco ASA & Firepower** 🛡️
  - **Palo Alto** 🔐
  - **Checkpoint** 🔲
  - **FortiGate** 🏰

Each lab comes with:

- **Topology Files:** Topology files for EVE-NG. (Compatible with both Community and PRO)✅
- **Configuration Files:** Initial and final configuration files. ❌
- **Problem Statements:** Detailed problem statements to guide you through each lab.✅

To get started, follow these steps:

1. **Import Labs into EVE-NG:**

By working through these labs, you will:

- Gain hands-on experience with routing and switching protocols.
- Understand the configuration and management of various firewalls.
- Develop troubleshooting skills in a simulated network environment.
- Exploring Multiple real-time Problem statement.

Objective: Configure IPv4 and IPv6 Addresses on Cisco Routers

- You are tasked with configuring both IPv4 and IPv6 addresses on a Cisco router.
- The router has two interfaces: GigabitEthernet 0/0 and GigabitEthernet 0/1.
- The IPv4 address for GigabitEthernet 0/0 should be 192.168.1.1/24 and for GigabitEthernet 0/1 should be 192.168.2.1/24.
- The IPv6 address for GigabitEthernet 0/0 should be 2001:192:168:1::1/64 and for GigabitEthernet 0/1 should be 2001:192:168:2::1/64

Objective: Cisco network setup consisting of both Layer 2 and Layer 3 switches

- Assign an IP address to the Layer 3 switch.
- Enable routing on the Layer 3 switch.
- Create VLANs and assign them to specific ports on both Layer 2 and Layer 3 switches.

Requirements:

- Assign IP Address to Layer 3 Switch:
- Choose an appropriate IP address and subnet mask for the management interface of the Layer 3 switch.
- Enable Routing on Layer 3 Switch:
- Enable IP routing to allow the Layer 3 switch to route traffic between different VLANs.

Create VLANs:

- Define multiple VLANs (e.g., VLAN 10 for the Sales department, VLAN 20 for the HR department).
- Assign these VLANs to specific ports on both Layer 2 and Layer 3 switches.
- Ensure inter-VLAN routing is configured to allow communication between VLANs on the Layer 3 switch.

Objective:

Design and understand a basic 2-tier network topology. This topology will include end devices (such as computers or workstations) connected to a switch, which in turn connects to a router providing access to external networks like the internet. This exercise aims to help you grasp the fundamental components and functions of a simple network architecture.

Components:

1. 
End Devices: 
  - 4 Computers (PC1, PC2, PC3, PC4)
2. 
Network Devices: 
  - 1 Switch (Switch1)
  - 1 Router (Router1)
3. 
Network Connections: 
  - Ethernet cables to connect PCs to the switch
  - An Ethernet cable to connect the switch to the router

Network Requirements:

1. 
IP Addressing: 
  - Use a private IP address range (e.g., 192.168.1.0/24).
  - Assign static IP addresses to each PC.
  - Configure the router with an appropriate IP address within the same subnet.
2. 
Switch Configuration: 
  - Basic configuration to ensure all ports are active and correctly connected.
3. 
Router Configuration: 
  - Configure the router with an IP address within the network subnet.
  - Set up the router to provide internet access or simulate an external network connection.

Steps to Complete the Task:

1. 
Design the Topology: 
  - Draw a simple diagram with 4 PCs connected to Switch1.
  - Show a connection from Switch1 to Router1.
2. 
Assign IP Addresses: 
  - Assign IP addresses to each PC. Example:
    - PC1: 192.168.1.2
    - PC2: 192.168.1.3
    - PC3: 192.168.1.4
    - PC4: 192.168.1.5
  - Assign the switch a management IP if needed (e.g., 192.168.1.1).
3. Assign IP addresses to each PC. Example:
4. 
Connect Devices: 
  - Physically or virtually (in a network simulator) connect PCs to the switch using Ethernet cables.
  - Connect the switch to the router using an Ethernet cable.
5. 
Configure the Router: 
  - Set the router’s internal IP address to 192.168.1.1.
  - Configure the router to route traffic to an external network (e.g., simulate internet access).
6. 
Verify Connectivity: 
  - Ping from one PC to another to ensure local network connectivity.
  - Ping the router from each PC to verify they can reach the gateway.
  - If an external network is configured, ping an external IP address to ensure internet connectivity.

Expected Outcomes:

1. 
Local Connectivity: 
  - All PCs should be able to communicate with each other.
  - PCs should be able to reach the router.
2. 
External Connectivity: 
  - If configured correctly, PCs should be able to reach an external network (e.g., ping 8.8.8.8 for internet).

Deliverables:

1. 
Network Diagram: 
  - A visual representation of the 2-tier network topology.
2. 
IP Address Plan: 
  - A table listing each device and its assigned IP address.
3. 
Configuration Files: 
  - Any configuration settings applied to the router and switch.
4. 
Connectivity Test Results: 
  - Ping test results showing successful connectivity between devices.

Objective:

Three-tier network architecture is a design model for organizing network infrastructure into three distinct layers, each with specific functions and responsibilities. This segmentation helps improve scalability, performance, manageability, and security.

**Problem Statement: Network Design and Configuration**

**Objective:**

Design and configure a network infrastructure for a small-to-medium-sized organization that includes access, distribution, and core layers. The network should support connectivity for four PCs, provide redundancy and fault tolerance, and ensure reliable internet access.

**Requirements:**

1. 
**Access Layer:**
  - Connect four PCs (PC-1 to PC-4) to four individual access switches (A-SW-1 to A-SW-4).
  - Each access switch should be connected to two distribution layer switches for redundancy and load balancing.
2. 
**Distribution Layer:**
  - Implement two distribution switches (D-SW-1 and D-SW-2).
  - Configure EtherChannel with two links between the distribution switches to ensure high bandwidth and redundancy.
  - Each distribution switch should be connected to two access switches.
3. 
**Core Layer:**
  - Deploy two core switches (C-SW-1 and C-SW-2).
  - Ensure that both core switches are interconnected and each core switch is connected to both distribution switches.
  - Connect the core switches to the internet through a router.
4. 
**IP Addressing:**
  - Define an IP addressing scheme for each layer, including VLANs for different network segments and devices to ensure proper network segmentation and management.
5. 
**HSRP Configuration:**
  - Configure Hot Standby Router Protocol (HSRP) on the core switches to provide gateway redundancy.
  - Ensure that one core switch is designated as the active HSRP router and the other as the standby to maintain network availability in case of a failure.
6. 
**Internet Connectivity:**
  - Verify that the core switches have connectivity to a router that provides internet access.
  - Configure routing protocols as necessary to enable proper internet access from all network devices.

**Constraints:**

- Ensure that the design is scalable to accommodate potential future expansion.
- The network must provide redundancy and high availability to minimize downtime.
- IP addressing and routing configurations must be efficient and well-documented to facilitate troubleshooting and management.

**Deliverables:**

- A detailed network diagram illustrating the access, distribution, and core layers, including connections and configurations.
- An IP addressing scheme and VLAN assignments.
- HSRP configuration details for the core switches.
- Routing configuration for internet access.
- Documentation of all configurations and design decisions.

**Problem Statement: Simulating a Spine-Leaf Network Architecture**

**Objective:**

Design and simulate a spine-leaf network architecture for a high-performance data center to achieve scalability, low latency, and high throughput. This simulation will provide insights into traffic management and the interactions between spine switches, leaf switches, and end devices.

**Requirements:**

1. 
**Network Design:**
  - Implement a spine-leaf network topology consisting of multiple spine switches and leaf switches.
  - Ensure that each leaf switch is connected to every spine switch to create a non-blocking network with equal bandwidth paths.
  - Designate a sufficient number of spine and leaf switches to meet the performance and scalability goals.
2. 
**Performance Metrics:**
  - Achieve low latency and high throughput across the network.
  - Optimize traffic management to prevent bottlenecks and ensure efficient data transfer between end devices.
3. 
**Scalability:**
  - Design the network to easily scale by adding additional spine or leaf switches without significant changes to the existing infrastructure.
  - Include mechanisms to handle increased traffic and device density effectively.
4. 
**Simulation Goals:**
  - Use network simulation tools to model and analyze the performance of the spine-leaf architecture.
  - Evaluate key performance indicators such as latency, throughput, and packet loss.
  - Simulate various traffic patterns and workloads to test the network’s efficiency and reliability.
5. 
**Traffic Management:**
  - Implement and test traffic distribution strategies to ensure balanced load across the spine and leaf switches.
  - Configure Quality of Service (QoS) policies as needed to prioritize critical traffic and manage network resources effectively.
6. 
**End Devices:**
  - Include a variety of end devices (nodes) in the simulation to represent typical data center workloads.
  - Assess the performance impact of different types of end devices and their interaction with the spine-leaf network.

**Constraints:**

- Ensure that the simulation accurately reflects real-world scenarios and network conditions.
- The design should be cost-effective and feasible with available hardware and software resources.
- Provide documentation of the simulation setup, configurations, and results to support analysis and decision-making.

**Deliverables:**

- A detailed network diagram of the spine-leaf architecture including spine and leaf switch configurations.
- Simulation results highlighting performance metrics such as latency, throughput, and traffic distribution.
- Analysis of the network's ability to scale and handle various traffic patterns.
- Recommendations for optimizing the spine-leaf network based on simulation findings.

Objective:

The goal is to ensure all devices are properly configured to communicate within their subnets and across the network, providing seamless connectivity to the Internet and internal resources.

You have been tasked with configuring a small office/home office (SOHO) network. The network diagram provided outlines the structure of the network, including various devices, subnets, and their interconnections. The goal is to ensure all devices are properly configured to communicate within their subnets and across the network, providing seamless connectivity to the Internet and internal resources.

1. **Router Configuration:**  - The router connects the internal network to the Internet.
  - The WAN interface (Gi0/1) should be configured to connect to the Internet using the subnet 192.168.1.0/24.
  - The LAN interface (Gi0/0) should be configured with the subnet 192.168.20.0/24.
2. **FortiGate Firewall Configuration:**  - The firewall serves as the primary security device between the internal network and the router.
  - Configure `port1` to connect to the switch using the subnet 192.168.20.0/24.
  - Configure `port2` to connect to the router using the subnet 192.168.20.0/24.
3. **Switch Configuration:**  - The switch connects multiple devices within the internal network.
  - Ensure all connected devices can communicate within the subnet 192.168.10.0/24.
  - Interfaces Gi0/0 to Gi0/3 should be configured to connect to PCs and workstations.
  - Interfaces Gi1/0 to Gi1/3 should be configured for other devices like laptops, IP phones, and tablets.
  - Interface Gi2/0 should be configured for the Wireless Access Point.
4. **Wireless Access Point Configuration:**  - The Wireless Access Point (WAP) should provide wireless connectivity for devices within the subnet 192.168.10.0/24.
5. **Device Configurations:**  - Each device on the network must be assigned an IP address within the appropriate subnet:
    - PCs, Workstations, IP Phones, and Tablets: 192.168.10.0/24
    - Wireless Laptops: 192.168.10.0/24 via WAP
6. Each device on the network must be assigned an IP address within the appropriate subnet:
7. **Additional Requirements:**  - Ensure proper VLAN configurations if necessary to separate different types of traffic.
  - Implement DHCP where applicable to automate IP address assignment for devices.
  - Enable appropriate firewall rules to allow necessary traffic while blocking unauthorized access.

VLAN Configuration and Testing - to check the basic knowledge of switching.

You are tasked with setting up a VLAN on two switches to ensure proper communication between devices on the same VLAN. The specific requirements are as follows:

1. Create VLAN 10:

- VLAN 10 needs to be created on both Switch 1 and Switch 2.

1. Configure Trunk Ports:

- The G0/0 interface on both Switch 1 and Switch 2 should be configured as trunk ports. These trunk ports will carry traffic for VLAN 10.

1. Assign Ports to VLAN 10:

- Both Switch 1 and Switch 2 should have specific ports assigned to VLAN 10. These ports will be used by devices that need to communicate with each other.

1. Test Communication:

- Ensure that devices connected to the ports assigned to VLAN 10 on Switch 1 and Switch 2 can communicate with each other.

Tasks Breakdown

1. VLAN Creation:

- On Switch 1, create VLAN 10.
- On Switch 2, create VLAN 10.

1. Trunk Port Configuration:

- On Switch 1, configure interface G0/0 as a trunk port.
- On Switch 2, configure interface G0/0 as a trunk port.

1. **Assign Ports to VLAN 10:

- On Switch 1, assign the desired ports to VLAN 10.
- On Switch 2, assign the desired ports to VLAN 10.

1. Testing:

- Connect two devices to the assigned ports on Switch 1 and Switch 2, respectively.
- Verify that the devices can communicate with each other, indicating that VLAN 10 is correctly configured and operational across both switches.

## 📝 Problem Statement: Problem Statement: Inter-VLAN Routing Solution with Router-on-a-Stick Configuration

**Objective:**

Design and implement an inter-VLAN routing solution using the router-on-a-stick configuration to enable communication between VLANs on a network. The goal is to configure and verify connectivity between VLANs, ensuring seamless communication while maintaining network segmentation and security.

**Topology:**

- **Router (Edge-R):** Equipped with two subinterfaces on its g0/0 interface:
  - Subinterface for VLAN 10 (IT) with IP address 10.1.1.100
  - Subinterface for VLAN 20 (HR) with IP address 20.1.1.100
- **Switch:** Configured with two VLANs:
  - VLAN 10 for the IT department
  - VLAN 20 for the HR department
- **PCs:**  - **PC-1 and PC-2:** Located in VLAN 10 (IT) with IP addresses in the 10.1.1.0/24 subnet
  - **PC-3 and PC-4:** Located in VLAN 20 (HR) with IP addresses in the 20.1.1.0/24 subnet

**Objectives:**

1. 
**VLAN Configuration:**
  - Configure VLAN 10 and VLAN 20 on the switch.
  - Assign IP subnet addresses to VLANs, ensuring VLAN 10 uses the 10.1.1.0/24 subnet and VLAN 20 uses the 20.1.1.0/24 subnet.
2. 
**Router Configuration:**
  - Establish a trunk link between the switch and the router.
  - Configure the router with subinterfaces for VLAN 10 and VLAN 20 to enable routing between the VLANs.
  - Assign the IP addresses 10.1.1.100 and 20.1.1.100 to the router’s subinterfaces.
3. 
**PC Configuration:**
  - Assign appropriate IP addresses and subnet masks to PCs within each VLAN:
    - PC-1 and PC-2 should be configured with IP addresses in the 10.1.1.0/24 subnet.
    - PC-3 and PC-4 should be configured with IP addresses in the 20.1.1.0/24 subnet.
4. Assign appropriate IP addresses and subnet masks to PCs within each VLAN:
5. 
**Connectivity Verification:**
  - Verify that PCs within the same VLAN can communicate with each other.
  - Test connectivity between PCs in different VLANs to ensure successful inter-VLAN routing.
  - Ensure that devices are able to communicate across VLANs through the router.
6. 
**Documentation:**
  - Document the configuration steps for the switch, router, and PCs.
  - Provide a comprehensive guide that includes network diagrams, configuration commands, and troubleshooting steps to support future maintenance and problem resolution.

**Deliverables:**

- A functional inter-VLAN routing setup demonstrating communication between devices in different VLANs.
- Detailed configuration documentation for switch VLANs, router subinterfaces, and PC IP settings.
- Verification results showing successful communication within and between VLANs.

**Constraints:**

- Ensure that the configuration maintains network segmentation and security while enabling required communication.
- The design should be scalable and easily adjustable for additional VLANs or changes in network topology.

**Objective:**

CDP is an essential protocol for Cisco network devices, simplifying network management and troubleshooting by providing a straightforward way to discover and display information about directly connected Cisco equipment. Proper configuration and usage of CDP can greatly enhance network visibility and operational efficiency.

You are tasked with configuring and verifying Cisco Discovery Protocol (CDP) on a network consisting of multiple Cisco devices. The network diagram shows the connectivity between various routers and switches. Your objective is to ensure that CDP is properly configured and operational across all devices to facilitate network discovery and troubleshooting.

1. 
**Enable CDP Globally on All Devices** :
  - Verify if CDP is enabled globally on each device.
  - If CDP is not enabled, enable it globally.
2. 
**Enable CDP on All Relevant Interfaces** :
  - Identify all the interfaces that connect to other Cisco devices.
  - Ensure CDP is enabled on these interfaces.
3. 
**Verify CDP Neighbor Information** :
  - Check the CDP neighbor table on each device.
  - Confirm that each device can see its directly connected neighbors.
4. 
**Collect and Document Neighbor Information** :
  - Document the details of each neighbor as shown in the CDP neighbor table.
  - Include information such as device name, local interface, neighbor interface, and capabilities.
5. 
**Troubleshoot CDP Issues** :
  - If any device does not display its neighbors correctly, verify the physical connectivity.
  - Ensure that CDP is not disabled on any necessary interfaces.
  - Check for any potential issues that might be blocking CDP packets (e.g., VLAN configuration, interface errors).
6. 
**Validate CDP Information Accuracy** :
  - Cross-check the discovered CDP information with the physical network diagram.
  - Ensure that all connections match the expected topology.
7. 
**Maintain CDP Configuration** :
  - Implement best practices for CDP configuration, such as setting the CDP timer and holdtime appropriately.
  - Regularly review and update the CDP neighbor information to reflect any network changes.
8. 
**Security Considerations** :
  - Evaluate the security implications of CDP in your network.
  - If necessary, disable CDP on interfaces connected to untrusted networks or devices.

**Objective:**

LLDP is a vendor-neutral protocol used by network devices to advertise their identity, capabilities, and neighbors on a local area network (LAN).

Use the Link Layer Discovery Protocol (LLDP) to discover and verify the network topology.

1. Enable LLDP on all network devices (routers, switches, IP phones, and VPC).
2. Verify connectivity and discover neighboring devices using LLDP.

- 
**Switch**
  - Connects to:
    - **VPC4** on port`eth0`
    - **IP_Phone-1** on port`eth0`
    - **Router-1** on port`Gi0/2`
- Connects to:
- 
**Router-1**
  - Connects to:
    - **Switch** on port`Gi0/2`
    - **IP_Phone-2** on port`eth0`
    - **Router-2** on port`Gi0/1`
- Connects to:
- 
**Router-2**
  - Connects to:
    - **Router-1** on port`Gi0/0`
- Connects to:

1. Enable LLDP globally.
2. Enable LLDP on interfaces `Gi0/0` ,`Gi0/1` , and`Gi0/2` .

1. Enable LLDP globally.
2. Enable LLDP on interface `Gi0/0` .

- Ensure LLDP is enabled if necessary through the phone's configuration interface (most IP phones support LLDP automatically).

- Since VPC typically does not support LLDP natively, verify the connection through the switch’s LLDP neighbor table.

1. Verify LLDP neighbors on the switch.
2. Verify LLDP neighbors on Router-1.
3. Verify LLDP neighbors on Router-2.
4. Verify LLDP neighbors on IP Phones through their settings or documentation.

By performing these steps, ensure all devices are properly discovered and that the topology is correctly identified according to the LLDP advertisements. This process assists in network troubleshooting, documentation, and ensuring proper connectivity across the network.

- Configure the VTP modes as per the provided diagram
- assign the modes as mentioned.
- Test the behavior of each mode, specifically checking the Configuration Revision (CR) number for transparent mode.
- Finally, manually configure all VLAN settings on the switch in transparent mode.

We welcome contributions! If you have a lab you’d like to add or improvements to suggest, please submit a pull request.

If you encounter any issues, please open an issue in this repository. We will address it as soon as possible.

This project is licensed under the MIT License. See the LICENSE file for more details.

```
Make sure to replace `https://github.com/yourusername/eve-ng-labs-catalog.git` with the actual URL of your repository and update the paths to your screenshots accordingly. This will make your repository more attractive and user-friendly on GitHub.
```