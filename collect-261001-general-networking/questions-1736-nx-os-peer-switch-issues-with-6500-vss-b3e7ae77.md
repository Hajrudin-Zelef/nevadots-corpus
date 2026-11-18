I'm bringing up a new pair of Nexus 5500s running NX-OS 5.2 and I'd like to make use of the new vPC Peer Switch feature:
The Virtual Port Channel (vPC) peer switch feature addresses performance concerns around STP convergence. This feature allows a pair of Cisco Nexus 5000 Series devices to appear as a single STP root in the Layer 2 topology. This feature eliminates the need to pin the STP root to the vPC primary switch and improves vPC convergence if the vPC primary switch fails.
To avoid loops, the vPC peer link is excluded from the STP computation. In vPC peer switch mode, STP BPDUs are sent from both vPC peer devices to avoid issues related to STP BPDU timeout on the downstream switches, which can cause traffic disruption.
This feature can be used with the pure peer switch topology in which the devices all belong to the vPC.
The current topology is a pair Nexus 5500s with a peer-link between them and one connected to each side of a 6500 VSS. All connections are LACP port channels.
The VSS should represent a single logical device, but the 5500s don't seem to agree. When I try to bring up both connections to the VSS the 5500s give me this error:
%VPC-3-INTF_CONSISTENCY_FAILED: In domain 2, VPC 1 configuration (interface port-channel1) is not consistent (vpc port channel mis-config due to vpc links in the 2 switches connected to different partners)
%VPC-3-VPC_BRINGUP_FAILED: vPC 1 (interface port-channel1) bringup failed (Type-1 inconsistency exists)
The 6500 VSS does not log any errors.
Why is the vPC viewing each side of the VSS as a different partner? Am I mistaken in thinking this should be work?