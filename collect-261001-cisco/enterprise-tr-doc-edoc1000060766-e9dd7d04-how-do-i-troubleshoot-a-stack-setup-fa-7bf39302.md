Enterprise
Check whether a stack can be set up between these member switches.
Run the display device command to view the member switch models and then determine whether a stack can be set up between these switches. If not, replace these switches.
You can refer to the product documentation to check whether these switches can set up a stack.
For example, if the switch model is CE5850-48T4S2Q-EI, you can confirm that this switch model can set up a stack with CE5850-48T4S2Q-EI or CE5850-24T4S2Q-EI.
<HUAWEI> display device
Device status:
-------------------------------------------------------------------------------------------
Slot  Card   Type                     Online   Power Register     Alarm     Primary
-------------------------------------------------------------------------------------------
1     -      CE5850-48T4S2Q-EI        Present  On    Registered   Normal    Master
      FAN2   FAN-40SA-B               Present  On    Registered   Normal    NA
      PWR2   PAC-150WA                Present  On    Registered   Normal    NA
-------------------------------------------------------------------------------------------
Check whether the stack configuration is correct.
Run the display stack configuration all command to check whether the stack configuration meets requirements.
The following uses the command output on a CE12800 switch as an example.
<HUAWEI> display stack configuration all
Oper : Operation
Conf : Configuration
*    : Offline configuration
Isolated Port: The port is in stack mode, but does not belong to any Stack-Port
Attribute Configuration:
---------------------------------------------------------------
 MemberID      Domain         Priority       Mode     Enable
Oper(Conf)   Oper(Conf)      Oper(Conf)   Oper(Conf)   Oper
---------------------------------------------------------------
1(1)         10(10)          150(150)     MB(MB)       Enable
---------------------------------------------------------------
Stack-Port Configuration:
--------------------------------------------------------------------------------
Stack-Port           Member Ports
--------------------------------------------------------------------------------
Stack-Port1/1        10GE1/1/0/1    10GE1/1/0/2
--------------------------------------------------------------------------------
Check whether all member switches use the same stack domain ID. All member switches must use the same stack domain ID. Otherwise, they cannot set up a stack.
If member switches use different stack domain IDs, run the stack member { member-id | all } domain domain-id command to change their stack domain IDs to the same.
(Applicable only to the CE12800 and CE12800E) Check whether all member switches use the same stack connection mode. All member switches must use the same stack connection mode. Otherwise, they cannot set up a stack. MB indicates the default MPU connection mode, and LC indicates the LPU connection mode.
If member switches use different stack connection modes, run the stack member { member-id | all } link-type { mainboard-direct | linecard-direct } command to change their stack connection modes to the same.
(Applicable only to the CE12800, CE12800E, and CE16800) Check whether the stacking function has been enabled on member switches. Member switches must have the stacking function enabled to set up a stack. Enable indicates that the stacking function is enabled, and Disable indicates that the stacking function is disabled.
If member switches have the stacking function disabled, run the stack enable command to enable this function.
Check whether member switches have offline configuration. The configuration marked with an asterisk (*) is offline configuration. If offline configuration exists, you must delete it. This is because offline configuration may case a stack configuration conflict, which will cause a failure to set up a stack.
Check whether stack connections are correct.
Check whether stack connections are consistent with the plan and configuration.
For details about how to connect switch members, see the corresponding product documentation.
CE12800 series switches support two stack connection modes: MPU connection and LPU connection. In MPU connection mode, both SIP ports on MPUs and service ports on LPUs need to be connected. In LPU connection mode, only service ports on LPUs need to be connected. The MPU connection mode is recommended because it separates management and forwarding links and ensures stack reliability.
Each CE8800, CE7800, CE6800, or CE5800 switch has two logical stack ports: Stack-Portn/1 and Stack-Portn/2, in which n indicates the stack member ID of a switch. One logical stack port can contain multiple physical member ports.
Stack member switches can be connected in a ring or chain topology. Logical stack ports on the member switches can be connected in any sequence.
Check whether ports used for stack connections are Up.
Run the display interface brief command to check whether the ports used for stack connections are physically Up, including stack member ports and SIP ports (applicable only to the CE12800, CE12800E, and CE16800). If these ports are physically Down, check whether optical modules and fibers are faulty.
<HUAWEI> display interface brief
......
Interface                  PHY      Protocol  InUti OutUti   inErrors  outErrors
...... 
10GE1/4/0/44               down     down         0%     0%          0          0 
10GE1/4/0/45               down     down         0%     0%          0          0 
MEth0/0/0/0                up       down      0.01%     0%          0          0 
NULL0                      up       up(s)        0%     0%          0          0 
Sip1/5/0/0                 down     down         0%     0%          0          0 
Sip1/5/0/1                 down     down         0%     0%          0          0 
Stack-Port1/1              down     down         0%     0%          0          0 
  10GE1/4/0/46             down     down         0%     0%          0          0 
  10GE1/4/0/47             down     down         0%     0%          0          
To set up a stack of CE12800 switches, ensure that SIP ports on MPUs are connected using network cables or using GE optical modules and LC fibers. Do not connect these SIP ports using 10GE optical modules.
You can also run the display stack link-state last-down-reason command to check the reason why the stack link protocol becomes Down. A possible cause is incorrect configuration or cable connection.
Check stack failure event information.
Run the display stack troubleshooting command to check whether stack failure events occur. This command can record some failures occurring during a stack setup, including configuration errors and connection errors. Troubleshoot these failures according to failure event description.
In V200R019C00 and earlier versions, run the display stack troubleshooting command to check faults in a stack.
In V200R005C20, V200R019C10, and later versions, run the display stack troubleshooting current command to check faults in a stack.
<HUAWEI> display stack troubleshooting current
Total :1      
----------------------------------------------------------------------------------------
Seq  Time                     Event Description
----------------------------------------------------------------------------------------
1    2012-11-23 19:28:23.889  The devices belong to different stack domains, 
                                    and stack cannot be established. (MemberID = 1, 
                                    DomainID = 10, PeerMemberID = 2, PeerDomainID = 20) 
----------------------------------------------------------------------------------------
| Fault | Description | Troubleshooting Procedure | 
|---|---|---|
| The devices belong to different stack domains. | The stack domain IDs of member switches are different. |  | 
| The type of devices is different. | The device types of member switches are different. | Set up a stack using switches of the same device type. | 
| Stack-Port link invalid. | Logical stack ports are incorrectly connected. |  | 
| Switches working in different forward modes cannot set up a CSS. | The card interoperability modes of member switches are different. |  | 
| The interface Stack-Port is down. | The protocol status of a logical stack port is Down. |  | 
| The physical status of the stack member port is up, but the protocol status is down. | The protocol status of a physical stack member port is Down, but its physical status is Up. |  | 
| Configuration conflict. | The configuration of a member switch conflicts with that of the master switch. | In most cases, this fault will occur if the master switch has member switch's stack port configurations that do not take effect. Run the display current-configuration all command on the master switch to check whether there are member switch's stack port configurations that do not take effect. If so, delete the configurations from the master switch. | 
| The port on CE-FWA board did not support configured as stack port. | Ports on the card cannot be used for stacking. | Use ports on stack-supporting cards to set up a stack. | 
Run the display diagnostic-information file-name command in the user view to collect diagnostic information and save the information to a file.
<HUAWEI> display diagnostic-information dia-info.txt 
Now saving the diagnostic information to the device 
 100% 
Info: The diagnostic information was saved to the device successfully.
The text file is stored in the flash:/ directory by default. You can run the dir command in the user view to check whether the file is generated.
After diagnostic information is saved to a file, you can export it from the device through FTP, SFTP, or SCP. For details, see Local File Management.
You can run the display diagnostic-information command to display diagnostic information and save terminal logs in a diagnostic file on a disk. For details, see Diagnostic File Obtaining Guide.
Run the save logfile command to save the log and trap information in the log buffer to files.
<HUAWEI> save logfile //Collect common user logs.
<HUAWEI> system-view 
[~HUAWEI] diagnose 
[~HUAWEI-diagnose] save logfile diagnose-log             //Collect diagnostic logs.
[~HUAWEI-diagnose] collect diagnostic information       //Collect diagnostic information of the operating system.
When the log files are generated, you can export the files from the device using FTP, SFTP, or SCP. For details, see Local File Management.
You can also run the display logbuffer and display trapbuffer commands to view the log and trap information on the device, and save terminal logs in a diagnostic file on a disk. For details, see Diagnostic File Obtaining Guide.
Contact us for technical support.
Technical support personnel will provide instructions for you to submit all the collected information and files, so that they can locate faults.
Select the content with the mouse pointer to quickly report the problem.