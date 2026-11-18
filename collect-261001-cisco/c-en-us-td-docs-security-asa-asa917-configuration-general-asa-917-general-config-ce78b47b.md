About SNMP
SNMP is an application-layer protocol that facilitates the exchange of management information between network devices and is part of the TCP/IP protocol suite. The ASA provides support for network monitoring using SNMP Versions 1, 2c, and 3, and support the use of all three versions simultaneously. The SNMP agent running on the ASA interface lets you monitor the network devices through network management systems (NMSes), such as HP OpenView. The ASA support SNMP read-only access through issuance of a GET request. SNMP write access is not allowed, so you cannot make changes with SNMP. In addition, the SNMP SET request is not supported.
You can configure the ASA to send traps, which are unsolicited messages from the managed device to the management station for certain events (event notifications) to an NMS, or you can use the NMS to browse the Management Information Bases (MIBs) on the security devices. MIBs are a collection of definitions, and the ASA maintain a database of values for each definition. Browsing a MIB means issuing a series of GET-NEXT or GET-BULK requests of the MIB tree from the NMS to determine values.
| Note | With intense workloads, deploying more than 10 NMS can impact the device's performance. To ensure device's stability and responsiveness, we recommend that you cautiously utilize NMS in conducting SNMP walk polling and in managing the trap traffic. | 
The ASA have an SNMP agent that notifies designated management stations if events occur that are predefined to require a notification, for example, when a link in the network goes up or down. The notification it sends includes an SNMP OID, which identifies itself to the management stations. The ASA agent also replies when a management station asks for information.
SNMP Terminology
The following table lists the terms that are commonly used when working with SNMP.
| Table 1. SNMP Terminology |  | 
|---|---|
| Term | Description | 
|---|---|
| Agent | The SNMP server running on the ASA. The SNMP agent has the following features:  | 
| Browsing | Monitoring the health of a device from the network management station by polling required information from the SNMP agent on the device. This activity may include issuing a series of GET-NEXT or GET-BULK requests of the MIB tree from the network management station to determine values. | 
| Management Information Bases (MIBs) | Standardized data structures for collecting information about packets, connections, buffers, failovers, and so on. MIBs are defined by the product, protocols, and hardware standards used by most network devices. SNMP network management stations can browse MIBs and request specific data or events be sent as they occur. | 
| Network management stations (NMSs) | The PCs or workstations set up to monitor SNMP events and manage devices, such as the ASA. | 
| Object identifier (OID) | The system that identifies a device to its NMS and indicates to users the source of information monitored and displayed. | 
| Trap | Predefined events that generate a message from the SNMP agent to the NMS. Events include alarm conditions such as linkup, linkdown, coldstart, warmstart, authentication, or syslog messages. | 
MIBs and Traps
MIBs are either standard or enterprise-specific. Standard MIBs are created by the IETF and documented in various RFCs. A trap reports significant events occurring on a network device, most often errors or failures. SNMP traps are defined in either standard or enterprise-specific MIBs. Standard traps are created by the IETF and documented in various RFCs. SNMP traps are compiled into the ASA software.
If needed, you can also download RFCs, standard MIBs, and standard traps from the following locations:
Browse the complete list of Cisco MIBs, traps, and OIDs from the following location:
ftp://ftp.cisco.com/pub/mibs/supportlists/asa/asa-supportlist.html
In addition, download Cisco OIDs by FTP from the following location:
ftp://ftp.cisco.com/pub/mibs/oid/oid.tar.gz
| Note | In software versions 7.2(1), 8.0(2), and later, the interface information accessed through SNMP refreshes about every 5 seconds. As a result, we recommend that you wait for at least 5 seconds between consecutive polls. | 
Not all OIDs in MIBs are supported. To obtain a list of the supported SNMP MIBs and OIDs for a specific ASA, enter the following command:
ciscoasa(config)# show snmp-server oidlist
| Note | Although the oidlist keyword does not appear in the options list for the show snmp-server command help, it is available. However, this command is for Cisco TAC use only. Contact the Cisco TAC before using this command. | 
This sample displays the output from the show snmp-server oidlist command:
ciscoasa(config)# show snmp-server oidlist
[0]     1.3.6.1.2.1.1.1.        sysDescr
[1]     1.3.6.1.2.1.1.2.        sysObjectID
[2]     1.3.6.1.2.1.1.3.        sysUpTime
[3]     1.3.6.1.2.1.1.4.        sysContact
[4]     1.3.6.1.2.1.1.5.        sysName
[5]     1.3.6.1.2.1.1.6.        sysLocation
[6]     1.3.6.1.2.1.1.7.        sysServices
[7]     1.3.6.1.2.1.2.1.        ifNumber
[8]     1.3.6.1.2.1.2.2.1.1.    ifIndex
[9]     1.3.6.1.2.1.2.2.1.2.    ifDescr
[10]    1.3.6.1.2.1.2.2.1.3.    ifType
[11]    1.3.6.1.2.1.2.2.1.4.    ifMtu
[12]    1.3.6.1.2.1.2.2.1.5.    ifSpeed
[13]    1.3.6.1.2.1.2.2.1.6.    ifPhysAddress
[14]    1.3.6.1.2.1.2.2.1.7.    ifAdminStatus
[15]    1.3.6.1.2.1.2.2.1.8.    ifOperStatus
[16]    1.3.6.1.2.1.2.2.1.9.    ifLastChange
[17]    1.3.6.1.2.1.2.2.1.10.   ifInOctets
[18]    1.3.6.1.2.1.2.2.1.11.   ifInUcastPkts
[19]    1.3.6.1.2.1.2.2.1.12.   ifInNUcastPkts
[20]    1.3.6.1.2.1.2.2.1.13.   ifInDiscards
[21]    1.3.6.1.2.1.2.2.1.14.   ifInErrors
[22]    1.3.6.1.2.1.2.2.1.16.   ifOutOctets
[23]    1.3.6.1.2.1.2.2.1.17.   ifOutUcastPkts
[24]    1.3.6.1.2.1.2.2.1.18.   ifOutNUcastPkts
[25]    1.3.6.1.2.1.2.2.1.19.   ifOutDiscards
[26]    1.3.6.1.2.1.2.2.1.20.   ifOutErrors
[27]    1.3.6.1.2.1.2.2.1.21.   ifOutQLen
[28]    1.3.6.1.2.1.2.2.1.22.   ifSpecific
[29]    1.3.6.1.2.1.4.1.        ipForwarding
[30]    1.3.6.1.2.1.4.20.1.1.   ipAdEntAddr
[31]    1.3.6.1.2.1.4.20.1.2.   ipAdEntIfIndex
[32]    1.3.6.1.2.1.4.20.1.3.   ipAdEntNetMask
[33]    1.3.6.1.2.1.4.20.1.4.   ipAdEntBcastAddr
[34]    1.3.6.1.2.1.4.20.1.5.   ipAdEntReasmMaxSize
[35]    1.3.6.1.2.1.11.1.       snmpInPkts
[36]    1.3.6.1.2.1.11.2.       snmpOutPkts
[37]    1.3.6.1.2.1.11.3.       snmpInBadVersions
[38]    1.3.6.1.2.1.11.4.       snmpInBadCommunityNames
[39]    1.3.6.1.2.1.11.5.       snmpInBadCommunityUses
[40]    1.3.6.1.2.1.11.6.       snmpInASNParseErrs
[41]    1.3.6.1.2.1.11.8.       snmpInTooBigs
[42]    1.3.6.1.2.1.11.9.       snmpInNoSuchNames
[43]    1.3.6.1.2.1.11.10.      snmpInBadValues
[44]    1.3.6.1.2.1.11.11.      snmpInReadOnlys
[45]    1.3.6.1.2.1.11.12.      snmpInGenErrs
[46]    1.3.6.1.2.1.11.13.      snmpInTotalReqVars
[47]    1.3.6.1.2.1.11.14.      snmpInTotalSetVars
[48]    1.3.6.1.2.1.11.15.      snmpInGetRequests
[49]    1.3.6.1.2.1.11.16.      snmpInGetNexts
[50]    1.3.6.1.2.1.11.17.      snmpInSetRequests
[51]    1.3.6.1.2.1.11.18.      snmpInGetResponses
[52]    1.3.6.1.2.1.11.19.      snmpInTraps
[53]    1.3.6.1.2.1.11.20.      snmpOutTooBigs
[54]    1.3.6.1.2.1.11.21.      snmpOutNoSuchNames
[55]    1.3.6.1.2.1.11.22.      snmpOutBadValues
[56]    1.3.6.1.2.1.11.24.      snmpOutGenErrs
[57]    1.3.6.1.2.1.11.25.      snmpOutGetRequests
[58]    1.3.6.1.2.1.11.26.      snmpOutGetNexts
[59]    1.3.6.1.2.1.11.27.      snmpOutSetRequests
[60]    1.3.6.1.2.1.11.28.      snmpOutGetResponses
[61]    1.3.6.1.2.1.11.29.      snmpOutTraps
[62]    1.3.6.1.2.1.11.30.      snmpEnableAuthenTraps
[63]    1.3.6.1.2.1.11.31.      snmpSilentDrops
[64]    1.3.6.1.2.1.11.32.      snmpProxyDrops
[65]    1.3.6.1.2.1.31.1.1.1.1. ifName
[66]    1.3.6.1.2.1.31.1.1.1.2. ifInMulticastPkts
[67]    1.3.6.1.2.1.31.1.1.1.3. ifInBroadcastPkts
[68]    1.3.6.1.2.1.31.1.1.1.4. ifOutMulticastPkts
[69]    1.3.6.1.2.1.31.1.1.1.5. ifOutBroadcastPkts
[70]    1.3.6.1.2.1.31.1.1.1.6. ifHCInOctets
--More--
SNMP Object Identifiers
Each Cisco system-level product has an SNMP object identifier (OID) for use as a MIB-II sysObjectID. The CISCO-PRODUCTS-MIB and the CISCO-ENTITY-VENDORTYPE-OID-MIB includes the OIDs that can be reported in the sysObjectID object in the SNMPv2-MIB, Entity Sensor MIB and Entity Sensor Threshold Ext MIB. You can use this value to identify the model type. The following table lists the sysObjectID OIDs for ASA and ISA models.
| Table 2. SNMP Object Identifiers |  |  | 
|---|---|---|
| Product Identifier | sysObjectID | Model Number | 
|---|---|---|
| ASAv | ciscoASAv (ciscoProducts 1902) | Cisco Adaptive Security Virtual Appliance (ASAv) | 
| ASAv System Context | ciscoASAvsy (ciscoProducts 1903) | Cisco Adaptive Security Virtual Appliance (ASAv) System Context | 
| ASAv Security Context | ciscoASAvsc (ciscoProducts 1904) | Cisco Adaptive Security Virtual Appliance (ASAv) Security Context. | 
| ISA 30004C Industrial Security Appliance | ciscoProducts 2268 | ciscoISA30004C | 
| CISCO ISA30004C with 4 GE Copper Security Context | ciscoProducts 2139 | ciscoISA30004Csc | 
| CISCO ISA30004C with 4 GE Copper System Context | ciscoProducts 2140 | ciscoISA30004Csy | 
| ISA 30002C2F Industrial Security Appliance | ciscoProducts 2267 | ciscoISA30002C2F | 
| CISCO ISA30002C2F with 2 GE Copper ports + 2 GE Fiber Security Context | ciscoProducts 2142 | ciscoISA30002C2Fsc | 
| CISCO ISA30002C2F with 2 GE Copper ports + 2 GE Fiber System Context | ciscoProducts 2143 | ciscoISA30002C2Fsy | 
| Cisco Industrial Security Appliance (ISA) 30004C Chassis | cevChassis 1677 | cevChassisISA30004C | 
| Cisco Industrial Security Appliance (ISA) 30002C2F Chassis | cevChassis 1678 | cevChassisISA30002C2F | 
| Central Processing Unit Temperature Sensor for ISA30004C Copper SKU | cevSensor 187 | cevSensorISA30004CCpuTempSensor | 
| Central Processing Unit Temperature Sensor for ISA30002C2F Fiber | cevSensor 189 | cevSensorISA30002C2FCpuTempSensor | 
| Processor Card Temperature Sensor for ISA30004C Copper SKU | cevSensor 192 | cevSensorISA30004CPTS | 
| Processor Card Temperature Sensor for ISA30002C2F Fiber SKU | cevSensor 193 | cevSensorISA30002C2FPTS | 
| Power Card Temperature Sensor for ISA30004C Copper SKU | cevSensor 197 | cevSensorISA30004CPowercardTS | 
| Power Card Temperature Sensor for ISA30002C2F Fiber SKU | cevSensor 198 | cevSensorISA30002C2FPowercardTS | 
| Port Card Temperature Sensor for ISA30004C | cevSensor 199 | cevSensorISA30004CPortcardTS | 
| Port Card Temperature Sensor for ISA30002C2F | cevSensor 200 | cevSensorISA30002C2FPortcardTS | 
| Central Processing Unit for ISA30004C Copper SKU | cevModuleCpuType 329 | cevCpuISA30004C | 
| Central Processing Unit for ISA30002C2F Fiber SKU | cevModuleCpuType 330 | cevCpuISA30002C2F | 
| Modules ISA30004C, ISA30002C2F | cevModule 111 | cevModuleISA3000Type | 
| 30004C Industrial Security Appliance Solid State Drive | cevModuleISA3000Type 1 | cevModuleISA30004CSSD64 | 
| 30002C2F Industrial Security Appliance Solid State Drive | cevModuleISA3000Type 2 | cevModuleISA30002C2FSSD64 | 
| Cisco ISA30004C/ISA30002C2F Hardware Bypass | cevModuleISA3000Type 5 | cevModuleISA3000HardwareBypass | 
| Cisco Secure Firewall 240P | ciscoCsf240P (ciscoProducts 3424) | ciscoCsf240P | 
| FirePOWER 4140 Security Appliance, 1U with embedded security module 36 | ciscoFpr4140K9 (ciscoProducts 2293) | FirePOWER 4140 | 
| FirePOWER 4120 Security Appliance, 1U with embedded security module 24 | ciscoFpr4120K9 (ciscoProducts 2294) | FirePOWER 4120 | 
| FirePOWER 4110 Security Appliance, 1U with embedded security module 12 | ciscoFpr4110K9 (ciscoProducts 2295) | FirePOWER 4110 | 
| FirePOWER 4110 Security Module 12 | ciscoFpr4110SM12 (ciscoProducts 2313) | FirePOWER 4110 Security Module 12 | 
| FirePOWER 4120 Security Module 24 | ciscoFpr4120SM24 (ciscoProducts 2314) | FirePOWER 4110 Security Module 24 | 
| FirePOWER 4140 Security Module 36 | ciscoFpr4140SM36 (ciscoProducts 2315) | FirePOWER 4110 Security Module 36 | 
| FirePOWER 4110 Chassis | cevChassis 1714 | cevChassisFPR4110 | 
| FirePOWER 4120 Chassis | cevChassis 1715 | cevChassisFPR4120 | 
| FirePOWER 4140 Chassis | cevChassis 1716 | cevChassisFPR4140 | 
| FirePOWER 4K Fan Bay | cevContainer 363 | cevContainerFPR4KFanBay | 
| FirePOWER 4K Power Supply Bay | cevContainer 364 | cevContainerFPR4KPowerSupplyBay | 
| FirePOWER 4120 Supervisor Module | cevModuleFPRType 4 | cevFPR4120SUPFixedModule | 
| FirePOWER 4140 Supervisor Module | cevModuleFPRType 5 | cevFPR4140SUPFixedModule | 
| FirePOWER 4110 Supervisor Module | cevModuleFPRType 7 | cevFPR4110SUPFixedModule | 
| Cisco FirePOWER 4110 Security Appliance, Threat Defense | cevChassis 1787 | cevChassisCiscoFpr4110td | 
| Cisco FirePOWER 4120 Security Appliance, Threat Defense | cevChassis 1788 | cevChassisCiscoFpr4120td | 
| Cisco FirePOWER 4140 Security Appliance, Threat Defense | cevChassis 1789 | cevChassisCiscoFpr4140td | 
| Cisco Firepower 9000 Security Module 24, Threat Defense | cevChassis 1791 | cevChassisCiscoFpr9000SM24td | 
| Cisco Firepower 9000 Security Module 24 NEBS, Threat Defense | cevChassis 1792 | cevChassisCiscoFpr9000SM24Ntd | 
| Cisco Firepower 9000 Security Module 36, Threat Defense | cevChassis 1793 | cevChassisCiscoFpr9000SM36td | 
| Cisco Secure Firewall Threat Defense Virtual, VMware | cevChassis 1795 | cevChassisCiscoFTDVVMW | 
| Cisco Firewall Threat Defense Virtual, AWS | cevChassis 1796 | cevChassisCiscoFTDVAWS | 
Physical Vendor Type Values
Each Cisco chassis or standalone system has a unique type number for SNMP use. The entPhysicalVendorType OIDs are defined in the CISCO-ENTITY-VENDORTYPE-OID-MIB. This value is returned in the entPhysicalVendorType object from the ASA, ASAv, or ASASM SNMP agent. You can use this value to identify the type of component (module, power supply, fan, sensors, CPU, and so on). The following table lists the physical vendor type values for the ASA models.
| Table 3. Physical Vendor Type Values |  | 
|---|---|
| Item | entPhysicalVendorType OID Description | 
|---|---|
| Gigabit Ethernet port | cevPortGe (cevPort 109) | 
| Cisco Adaptive Security Virtual Appliance | cevChassisASAv (cevChassis 1451) | 
Supported Tables and Objects in MIBs
The following table lists the supported tables and objects for the specified MIBs.
In multi-context mode, these tables and objects provide information for a single context. If you want data across contexts, you need to sum them. For example, to get overall memory usage, sum the cempMemPoolHCUsed values for each context.
| Table 4. Supported Tables and Objects in MIBs |  | 
|---|---|
| MIB Name and OID | Supported Tables and Objects | 
|---|---|
| CISCO-ENHANCED-MEMPOOL-MIB; OID:1.3.6.1.4.1.9.9.221 | cempMemPoolTable, cempMemPoolIndex, cempMemPoolType, cempMemPoolName, cempMemPoolAlternate, cempMemPoolValid. For a 32-bit memory system, poll using the 32-bit memory counters—cempMemPoolUsed, cempMemPoolFree,cempMemPoolUsedOvrflw, cempMemPoolFreeOvrflw, cempMemPoolLargestFree, cempMemPoolLowestFree, cempMemPoolUsedLowWaterMark, cempMemPoolAllocHit, cempMemPoolAllocMiss, cempMemPoolFreeHit, cempMemPoolFreeMiss, cempMemPoolLargestFreeOvrflw, cempMemPoolLowestFreeOvrflw, cempMemPoolUsedLowWaterMarkOvrflw, cempMemPoolSharedOvrflw. For a 64-bit memory system, poll using the 64-bit memory counters—cempMemPoolHCUsed, cempMemPoolHCFree, cempMemPoolHCLargestFree, cempMemPoolHCLowestFree, cempMemPoolHCUsedLowWaterMark, cempMemPoolHCShared | 
| CISCO-REMOTE-ACCESS-MONITOR-MIB; OID:1.3.6.1.4.1.9.9.392 | crasNumTotalFailures, crasNumSetupFailInsufResources, crasNumAbortedSessions | 
| CISCO-ENTITY-SENSOR-EXT-MIB; OID:1.3.6.1.4.1.9.9.745 | ceSensorExtThresholdTable | 
| CISCO-L4L7MODULE-RESOURCE-LIMIT-MIB; OID:1.3.6.1.4.1.9.9.480 | ciscoL4L7ResourceLimitTable | 
| CISCO-TRUSTSEC-SXP-MIB; OID:1.3.6.1.4.1.9.9.720 | ctsxSxpGlobalObjects, ctsxSxpConnectionObjects, ctsxSxpSgtObjects | 
| DISMAN-EVENT-MIB; OID:1.3.6.1.2.1.88 | mteTriggerTable, mteTriggerThresholdTable, mteObjectsTable, mteEventTable, mteEventNotificationTable | 
| DISMAN-EXPRESSION-MIB; OID:1.3.6.1.2.1.90 | expExpressionTable, expObjectTable, expValueTable | 
| ENTITY-SENSOR-MIB; OID: 1.3.6.1.2.1.99 | entPhySensorTable | 
| NAT-MIB; OID:1.3.6.1.2.1.123 | natAddrMapTable, natAddrMapIndex, natAddrMapName, natAddrMapGlobalAddrType, natAddrMapGlobalAddrFrom, natAddrMapGlobalAddrTo, natAddrMapGlobalPortFrom, natAddrMapGlobalPortTo, natAddrMapProtocol, natAddrMapAddrUsed, natAddrMapRowStatus | 
| CISCO-PTP-MIB; OID:1.3.6.1.4.1.9.9.760 | ciscoPtpMIBSystemInfo, cPtpClockDefaultDSTable, cPtpClockTransDefaultDSTable, cPtpClockPortTransDSTable | 
| CISCO-PROCESS-MIB; 1.3.6.1.4.1.9.9.109.1.1.1.1.7.1 1.3.6.1.4.1.9.9.109.1.1.1.1.7.2 to 1.3.6.1.4.1.9.9.109.1.1.1.1.7.(n+1) | cpmCPUTotal1minRev Associated parameters and values of cpmCPUTotal1minRev Examples:  | 
| Note | These three MIB OIDs can be used to track why remote access connections fail. | 
| Note | Not supported on the ASAv. | 
| Note | Provides information related to physical sensors, such as chassis temperature, fan RPM, power supply voltage, etc. Not supported on the ASAv platform. | 
| Note | Only MIBs corresponding to E2E Transparent Clock mode are supported. | 
Supported Traps (Notifications)
The following table lists the supported traps (notifications) and their associated MIBs.
| Table 5. Supported Traps (Notifications) |  |  | 
|---|---|---|
| Trap and MIB Name | Varbind List | Description | 
|---|---|---|
| authenticationFailure (SNMPv2-MIB) | — | For SNMP Version 1 or 2, the community string provided in the SNMP request is incorrect. For SNMP Version 3, a report PDU is generated instead of a trap if the auth or priv passwords or usernames are incorrect. The snmp-server enable traps snmp authentication command is used to enable and disable transmission of these traps. | 
| bgpBackwardTransition | bgpPeerLastError, bgpPeerState | The snmp-server enable traps peer-flap command is used to enable transmission of BGP peer-flap related trap. | 
| ccmCLIRunningConfigChanged (CISCO-CONFIG-MAN-MIB) | ccmHistoryRunningLastChanged, ccmHistoryEventTerminalType | The snmp-server enable traps config command is used to enable transmission of this trap. | 
| cefcFRUInserted (CISCO-ENTITY-FRU-CONTROL -MIB) | entPhysicalContainedIn | The snmp-server enable traps entity fru-insert command is used to enable this notification. | 
| cefcFRURemoved (CISCO-ENTITY-FRU-CONTROL -MIB) | entPhysicalContainedIn | The snmp-server enable traps entity fru-remove command is used to enable this notification. | 
| ceSensorExtThresholdNotification (CISCO-ENTITY-SENSOR-EXT -MIB) | entPhysicalName, entPhysicalDescr, entPhySensorValue, entPhySensorType, ceSensorExtThresholdValue | The snmp-server enable traps entity [power-supply-failure \| fan-failure \| cpu-temperature] command is used to enable transmission of the entity threshold notifications. This notification is sent for a power supply failure. The objects sent identify the fan and CPU temperature. The snmp-server enable traps entity fan-failure command is used to enable transmission of the fan failure trap.This trap does not apply to the Firepower 2100 series. The snmp-server enable traps entity power-supply-failure command is used to enable transmission of the power supply failure trap.This trap does not apply to the Firepower 2100 series. The snmp-server enable traps entity chassis-fan-failure command is used to enable transmission of the chassis fan failure trap. The snmp-server enable traps entity cpu-temperature command is used to enable transmission of the high CPU temperature trap. This trap does not apply to the Firepower 2100 series. The snmp-server enable traps entity power-supply-presence command is used to enable transmission of the power supply presence failure trap. The snmp-server enable traps entity power-supply-temperature command is used to enable transmission of the power supply temperature threshold trap. The snmp-server enable traps entity chassis-temperature command is used to enable transmission of the chassis ambient temperature trap. This trap does not apply to the Firepower 2100 series. The snmp-server enable traps entity accelerator-temperature command is used to enable transmission of the chassis accelerator temperature trap. | 
| cikeTunnelStart (CISCO-IPSEC-FLOW-MONITOR-MIB) | cikePeerLocalAddr, cikePeerRemoteAddr, cikeTunLifeTime | The snmp-server enable traps ikev2 start command is used to enable transmission of ikev2 start trap. | 
| cikeTunnelStop (CISCO-IPSEC-FLOW-MONITOR-MIB) | cikePeerLocalAddr, cikePeerRemoteAddr, cikeTunActiveTime | The snmp-server enable traps ikev2 stop command is used to enable transmission of ikev2 stop trap. | 
| cipSecTunnelStart (CISCO-IPSEC-FLOW-MONITOR -MIB) | cipSecTunLifeTime, cipSecTunLifeSize | The snmp-server enable traps ipsec start command is used to enable transmission of this trap. | 
| cipSecTunnelStop (CISCO-IPSEC-FLOW-MONITOR -MIB) | cipSecTunActiveTime | The snmp-server enable traps ipsec stop command is used to enable transmission of this trap. | 
| ciscoConfigManEvent (CISCO-CONFIG-MAN-MIB) | ccmHistoryEventCommandSource, ccmHistoryEventConfigSource, ccmHistoryEventConfigDestination | The snmp-server enable traps config command is used to enable transmission of this trap. | 
| ciscoRasTooManySessions (CISCO-REMOTE-ACCESS -MONITOR-MIB) | crasNumSessions, crasNumUsers, crasMaxSessionsSupportable, crasMaxUsersSupportable, crasThrMaxSessions | The snmp-server enable traps remote-access session-threshold-exceeded command is used to enable transmission of these traps. | 
| ciscoUFwFailoverStateChanged (CISCO-UNIFIED-FIREWALL-MIB) | gid, FOStatus | The snmp-server enable traps failover-state command is used to enable transmission of failover-state trap. | 
| clogMessageGenerated (CISCO-SYSLOG-MIB) | clogHistFacility, clogHistSeverity, clogHistMsgName, clogHistMsgText, clogHistTimestamp | Syslog messages are generated. The value of the clogMaxSeverity object is used to decide which syslog messages are sent as traps. The snmp-server enable traps syslog command is used to enable and disable transmission of these traps. | 
| clrResourceLimitReached (CISCO-L4L7MODULE-RESOURCE -LIMIT-MIB) | crlResourceLimitValueType, crlResourceLimitMax, clogOriginIDType, clogOriginID | The snmp-server enable traps connection-limit-reached command is used to enable transmission of the connection-limit-reached notification. The clogOriginID object includes the context name from which the trap originated. | 
| coldStart (SNMPv2-MIB) | — | The coldStart trap that occurs when the SNMP agent starts after the SNMP configuration. This trap also occurs when the agent starts after a system reboot. The snmp-server enable traps snmp coldstart command is used to enable and disable transmission of these traps. | 
| cpmCPURisingThreshold (CISCO-PROCESS-MIB) | cpmCPURisingThresholdValue, cpmCPUTotalMonIntervalValue, cpmCPUInterruptMonIntervalValue, cpmCPURisingThresholdPeriod, cpmProcessTimeCreated, cpmProcExtUtil5SecRev | The snmp-server enable traps cpu threshold rising command is used to enable transmission of the CPU threshold rising notification. The cpmCPURisingThresholdPeriod object is sent with the other objects. | 
| cufwClusterStateChanged (CISCO-UNIFIED-FIREWALL-MIB) | status | The snmp-server enable traps cluster-state command is used to enable transmission of cluster-state trap. | 
| entConfigChange (ENTITY-MIB) | — | The snmp-server enable traps entity config-change fru-insert fru-remove command is used to enable this notification. | 
| linkDown (IF-MIB) | ifIndex, ifAdminStatus, ifOperStatus | The linkdown trap for interfaces. The snmp-server enable traps snmp linkdown command is used to enable and disable transmission of these traps. | 
| linkUp (IF-MIB) | ifIndex, ifAdminStatus, ifOperStatus | The linkup trap for interfaces. The snmp-server enable traps snmp linkup command is used to enable and disable transmission of these traps. | 
| mteTriggerFired (DISMAN-EVENT-MIB) | mteHotTrigger, mteHotTargetName, mteHotContextName, mteHotOID, mteHotValue, cempMemPoolName, cempMemPoolHCUsed | The snmp-server enable traps memory-threshold command is used to enable the memory threshold notification. The mteHotOID is set to cempMemPoolHCUsed. The cempMemPoolName and cempMemPoolHCUsed objects are sent with the other objects. | 
| mteTriggerFired (DISMAN-EVENT-MIB) | mteHotTrigger, mteHotTargetName, mteHotContextName, mteHotOID, mteHotValue, ifHCInOctets, ifHCOutOctets, ifHighSpeed, entPhysicalName | The snmp-server enable traps interface-threshold command is used to enable the interface threshold notification. The entPhysicalName objects are sent with the other objects. | 
| natPacketDiscard (NAT-MIB) | ifIndex | The snmp-server enable traps nat packet-discard command is used to enable the NAT packet discard notification. This notification is rate limited for 5 minutes and is generated when IP packets are discarded by NAT because mapping space is not available. The ifIndex gives the ID of the mapped interface. | 
| ospfNbrStateChange | ospfRouterId, ospfNbrIpAddr, ospfNbrAddressLessIndex, ospfNbrRtrId, ospfNbrState | The snmp-server enable traps peer-flap command is used to enable transmission of OSPF peer-flap related trap. | 
| warmStart (SNMPv2-MIB) | — | The warmStart trap that occurs when the SNMP agent restarts for the first time. This trap also occurs when the agent restarts after a SNMP configuration change where, all the SNMP host configuration are removed and a fresh SNMP configuration is done. The snmp-server enable traps snmp warmstart command is used to enable and disable transmission of these traps. | 
| Note | For cluster and HA nodes, post a reload, if the interfaces reboot time exceeds 5 minutes (preset threshold), the trap is dropped. When the cluster and HA nodes have rebooted sucessfully, all other traps are sent as expected. | 
| Note | This notification is only sent in multimode when a security context is created or removed. | 
| Note | For ASA5585 models, the SNMP engine is changed to use the netsnmp version 5.8 library and the following OIDs are not available in the library:  | 
Interface Types and Examples
The interface types that produce SNMP traffic statistics include the following:
-  
                                 		  
                                 Logical—Statistics collected by the software driver, which are a subset of physical statistics.
-  
                                 		  
                                 Physical—Statistics collected by the hardware driver. Each physical named interface has a set of logical and physical statistics associated with it. Each physical interface may have more than one VLAN interface associated with it. VLAN interfaces only have logical statistics. Note 
 For a physical interface that has multiple VLAN interfaces associated with it, be aware that SNMP counters for ifInOctets and ifOutoctets OIDs match the aggregate traffic counters for that physical interface. 
-  
                                 		  
                                 VLAN-only—SNMP uses logical statistics for ifInOctets and ifOutOctets.
The examples in the following table show the differences in SNMP traffic statistics. Example 1 shows the difference in physical and logical output statistics for the show interface command and the show traffic command. Example 2 shows output statistics for a VLAN-only interface for the show interface command and the show traffic command. The example shows that the statistics are close to the output that appears for the show traffic command.
| Table 6. SNMP Traffic Statistics for Physical and VLAN                                     		Interfaces |  | 
|---|---|
| Example 1 | Example 2 | 
|---|---|
|  ciscoasa# show interface GigabitEthernet3/2 interface GigabitEthernet3/2 description fullt-mgmt nameif mgmt security-level 10 ip address 10.7.14.201 255.255.255.0 management-only  ciscoasa# show traffic (Condensed output)  Physical Statistics GigabitEthernet3/2: received (in 121.760 secs) 36 packets       3428 bytes 0 pkts/sec      28 bytes/sec  Logical Statistics mgmt: received (in 117.780 secs) 36 packets       2780 bytes 0 pkts/sec      23 bytes/sec  The following examples show the SNMP output statistics for the management interface and the physical interface. The ifInOctets value is close to the physical statistics output that appears in the show traffic command output but not to the logical statistics output. ifIndex of the mgmt interface:  IF_MIB::ifDescr.6 = Adaptive Security Appliance ‘mgmt’ interface  ifInOctets that corresponds to the physical interface statistics:  IF-MIB::ifInOctets.6 = Counter32:3246   |  ciscoasa# show interface GigabitEthernet0/0.100 interface GigabitEthernet0/0.100 vlan 100 nameif inside security-level 100 ip address 10.7.1.101 255.255.255.0 standby 10.7.1.102  ciscoasa# show traffic inside received (in 9921.450 secs) 1977 packets       126528 bytes 0 pkts/sec      12 bytes/sec transmitted (in 9921.450 secs) 1978 packets       126556 bytes 0 pkts/sec      12 bytes/sec  ifIndex of VLAN inside:  IF-MIB::ifDescr.9 = Adaptive Security Appliance ‘inside’ interface IF-MIB::ifInOctets.9 = Counter32: 126318   | 
SNMP Version 3 Overview
SNMP Version 3 provides security enhancements that are not available in SNMP Version 1 or Version 2c. SNMP Versions 1 and 2c transmit data between the SNMP server and SNMP agent in clear text. SNMP Version 3 adds authentication and privacy options to secure protocol operations. In addition, this version controls access to the SNMP agent and MIB objects through the User-based Security Model (USM) and View-based Access Control Model (VACM). The ASA also supports the creation of SNMP groups and users, as well as hosts, which is required to enable transport authentication and encryption for secure SNMP communications.
Security Models
For configuration purposes, the authentication and privacy options are grouped together into security models. Security models apply to users and groups, which are divided into the following three types:
- 
                                    NoAuthPriv—No Authentication and No Privacy, which means that no security is applied to messages.
- 
                                    AuthNoPriv—Authentication but No Privacy, which means that messages are authenticated.
- 
                                    AuthPriv—Authentication and Privacy, which means that messages are authenticated and encrypted.
SNMP Groups
An SNMP group is an access control policy to which users can be added. Each SNMP group is configured with a security model, and is associated with an SNMP view. A user within an SNMP group must match the security model of the SNMP group. These parameters specify what type of authentication and privacy a user within an SNMP group uses. Each SNMP group name and security model pair must be unique.
SNMP Users
SNMP users have a specified username, a group to which the user belongs, authentication password, encryption password, and authentication and encryption algorithms to use. The authentication algorithm options are SHA-1, SHA-224, SHA-256 HMAC, and SHA-384. The encryption algorithm options are 3DES and AES (which is available in 128, 192, and 256 versions). When you create a user, you must associate it with an SNMP group. The user then inherits the security model of the group.
| Note | When configuring an SNMP v3 user account, ensure that the length of authentication algorithm is equal to or greater than the length of encryption algorithm. | 
SNMP Hosts
An SNMP host is an IP address to which SNMP notifications and traps are sent. To configure SNMP Version 3 hosts, along with the target IP address, you must configure a username, because traps are only sent to a configured user. SNMP target IP addresses and target parameter names must be unique on the ASA. Each SNMP host can have only one username associated with it. To receive SNMP traps, after you have added the snmp-server host command, make sure that you configure the user credentials on the NMS to match the credentials for the ASA.
| Note | You can add up to 4000 hosts. However, only 128 of this number can be for traps. | 
Implementation Differences Between the ASA and Cisco IOS Software
The SNMP Version 3 implementation in the ASA differs from the SNMP Version 3 implementation in the Cisco IOS software in the following ways:
- 
                                    		  
                                    The local-engine and remote-engine IDs are not configurable. The local engine ID is generated when the ASA starts or when a context is created.
- 
                                    		  
                                    No support exists for view-based access control, which results in unrestricted MIB browsing.
- 
                                    		  
                                    Support is restricted to the following MIBs: USM, VACM, FRAMEWORK, and TARGET.
- 
                                    		  
                                    You must create users and groups with the correct security model.
- 
                                    		  
                                    You must remove users, groups, and hosts in the correct sequence.
- 
                                    		  
                                    Use of the snmp-server host command creates an ASA rule to allow incoming SNMP traffic.
SNMP Syslog Messaging
SNMP generates detailed syslog messages that are numbered 212nnn. Syslog messages indicate the status of SNMP requests, SNMP traps, SNMP channels, and SNMP responses from the ASA or ASASM to a specified host on a specified interface.
For detailed information about syslog messages, see the syslog messages guide.
| Note | SNMP polling fails if SNMP syslog messages exceed a high rate (approximately 4000 per second). | 
Application Services and Third-Party Tools
For information about SNMP support, see the following URL:
http://www.cisco.com/en/US/tech/tk648/tk362/tk605/tsd_technology_support_sub-protocol_home.html
For information Refer to SNMP Chapter in Cisco ASA Series General Operations ASDM Configuration Guide, see the following URL: