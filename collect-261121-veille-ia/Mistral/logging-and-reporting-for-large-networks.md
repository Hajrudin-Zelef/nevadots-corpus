---
id: collect-261121-veille-ia/fortinet/logging-and-reporting-for-large-networks
title: "Logging and reporting for large networks"
domain: fortinet
role: reference
task: article
actors: ["Mistral AI"]
dates: []
keywords: ["settings", "fortigate", "default", "generating", "enable", "logging", "message", "following"]
source: https://www.fortinetguru.com/2016/11/logging-and-reporting-for-large-networks/
source_anchor: ""
source_lines: [1, 346]
section: "Mistral"
---
**Logg****i****n****g and reporting for large networks**

This section explains how to configure the FortiGate unit for logging and reporting in a larger network, such as an enterprise network. To set up this type of network, you are modifying the default log settings, and you are also modifying the default report.

The following procedures are examples and can be used to help you when configuring your own network’s log topology.

Since some of these settings must be modified or enabled or disabled in the CLI, it is recommended to review the FortiGate CLI Reference for any additional information about the commands used herein, as well as any that you would need to use in your own newtork’s log topology.


**M****od****i****f****y****i****n****g default log device settings**

The default log device settings must be modified so that system performance is not compromised. The FortiGate unit, by default, has all logging of FortiGate features enabled and well as logging to either the FortiGate unit’s system memory or hard disk, depending on the model.


**M****od****i****f****y****i****n****g multiple FortiGate units’ system memory default settings**

When the FortiGate unit’s default log device is its system memory, you can modify it to fit your log network topology. In this topic, the following is an example of how you can modify these default settings.


**T****o modify the default system memory settings**

**1****.** Log in to the CLI.

**2****.** Enter the following command syntax to modify the logging settings:

config log memory setting set ips-archive disable set status enable

end

**3****.** Enter the following command syntax to modify the FortiGate features that are enabled for logging:

config log memory filter set attack enable

set forward-traffic enable set local-traffic enable set netscan enable

set email-log-imap enable

set multicast-traffic enable set scanerror enable

set app-ctrl enable end

**4****.** Repeat steps 2 and 3 for the other FortiGate units.

**5****.** Test the modified settings using the procedure below.


**M****od****i****f****y****i****n****g multiple FortiGate units’ hard disk default log settings**

You will have to modify each FortiGate unit’s hard disk default log settings. The following is an example of how to modify these default settings.


**T****o modify the default hard disk settings**

**1****.** Log in to the CLI.

**2****.** Enter the following command syntax to modify the logging settings:

config log disk setting

set ips-archive disable set status enable

set max-log-file-size 1000 set storage Internal

set log-quota 100

set report-quota 100 end

**3****.** In the CLI, enter the following to disable certain event log messages that you do not want logged:

config log disk filter

set sniffer-traffic disable set local-traffic enable

end

**4****.** Repeat the steps 2 to 4 for the other FortiGate units.

**5****.** Test the modified settings using the procedure below.


**T****es****t****i****n****g the modified log settings**

After modifying both the settings and the FortiGate features for logging, you can test that the modified settings are working properly. This test is done in the CLI.


**T****o test sending logs to the log device**

**1****.** In the CLI, enter the following command syntax:

diag log test

When you enter the command, the following appears:

generating a system event message with level – warning generating an infected virus message with level – warning generating a blocked virus message with level – warning generating a URL block message with level – warning generating a DLP message with level – warning

generating an IPS log message generating an anomaly log message

generating an application control IM message with level – information generating an IPv6 application control IM message with level – information generating deep application control logs with level – information generating an antispam message with level – notification

generating an allowed traffic message with level – notice generating a multicast traffic message with level – notice generating a ipv6 traffic message with level – notice

generating a wanopt traffic log message with level – notification

generating a HA event message with level – warning generating netscan log messages with level – notice generating a VOIP event message with level – information generating a DNS event message with level – information generating authentication event messages

generating a Forticlient message with level – information generating a NAC QUARANTINE message with level – notification generating a URL block message with level – warning

**2****.** In the web-based interface, go to **Lo****g & Report > Event Log > User**, and view the logs to see some of the recently generated test log messages.

You will be able to tell the test log messages from real log messages because they do not have “real” information;

for example, the test log messages for the vulnerability scan contain the destination IP address of 1.1.1.1 or 2.2.2.2.


**C****on****f****i****gu****r****i****n****g the backup solution**

Even though you are logging to multiple FortiAnalyzer units, this is more of a redundancy solution rather than a complete backup solution in this example.

The multiple FortiAnalyzer units act similar to a HA cluster, since if one FortiAnalyzer unit fails, the others continue storing the logs they receive. In a backup solution, the logs are backed up to another secure location if something happens to the log device.

A good alternate or redundant option is the FortiCloud service, which can provide secure online logging and management for multiple devices.


**C****on****f****i****gu****r****i****n****g logging to multiple FortiAnalyzer units**

The following example shows how to configure logging to multiple FortiAnalyzer units. Configuring multiple FortiAnalyzer units is quick and easy; however, you can only configure up to three FortiAnalyzer units per FortiGate unit.


**T****o configure multiple FortiAnalyzer units**

**1****.** In the CLI, enter the following command syntax to configure the first FortiAnalyzer unit:

config log fortianalyzer setting set status enable

set server 172.20.120.22 set max-buffer-size 1000 set buffer-max-send 2000 set address-mode static set conn-timeout 100

set monitor-keepalive-period 120

set monitor-failure-retry-period 2000

end

**2****.** Disable the features that you do not want logged, using the following example command syntax. You can view the

CLI Reference to see what commands are available.

config log fortianalyzer filter set traffic (enable | disable)

… end

**3****.** Enter the following commands for the second FortiAnalyzer unit:

config log fortianalyzer2 setting set status enable

set server 172.20.120.23 set max-buffer-size 1000 set buffer-max-send 2000 set address-mode static set conn-timeout 100

set monitor-keepalive-period 120

set monitor-failure-retry-period 2000

end

**4****.** Disable the features that you do not want logged, using the following example command syntax.

config log fortianalyzer filter set web (enable | disable)

… end

**5****.** Enter the following commands for the last FortiAnalyzer unit:

config log fortianalyzer3 setting set status enable

set server 172.20.120.23 set max-buffer-size 1000 set buffer-max-send 2000 set address-mode static set conn-timeout 100

set monitor-keepalive-period 120

set monitor-failure-retry-period 2000

end

**6****.** Disable the features that you do not want logged, using the following example command syntax.

config log fortianalyzer filter

set web-filter (enable | disable)

… end

**7****.** Test the configuration by using the procedure, “Testing the modified log settings”.

**8****.** On the other FortiGate units, configure steps 1 through 6, ensuring that logs are being sent to the FortiAnalyzer units.


**C****on****f****i****gu****r****i****n****g logging to the FortiCloud server**

The FortiCloud server can be used as a redundant backup, or your primary logging solution. The following assumes that this service has already been registered, and a subscription has been purchased for expanded space. The following is an example of how to these settings are configured for a network’s log configuration. You need to have access to both the CLI and the web-based manager when configuring uploading of logs. The upload time and interval settings can be configured in the web-based interface.


**T****o configure logging to the FortiCloud server**

**1****.** Go to **S****ys****t****e****m > Dashboard > Status** and click **Log****i****n** next to **Fo****r****t****i****C****l****ou****d** in the License Information widget.

**2****.** Enter your username and password, and click **O****K**. (Or register, if you have not yet done so.)

**3****.** Logs will automatically be uploaded to FortiCloud as long as your FortiGate is linked to your FortiCloud account.

**4****.** To configure the upload time and interval, go to **Lo****g & Report > Log Config > Log Settings**.

**5****.** Under the Logging and Archiving header, you can select your desired upload time.

**6****.** With FortiCloud you can easily store and access FortiGate logs that can give you valuable insight into the health and security of your network.


**M****od****i****f****y****i****n****g the default FortiOS report**

The default FortiOS report is provided to help you quickly and easily configure and generate a report. Below is a sample configuration with multiple examples of significant customizations that you can make to tailor reports for larger networks.


**C****r****ea****t****i****n****g datasets**

You need to create a new dataset for gathering information about HA, admin activity and configuration changes.

Creating datasets requires SQL knowledge.


**T****o create the datasets**

**1****.** Log in to the CLI.

**2****.** Enter the following command syntax:

config report dataset edit ha

set query “select subtype_ha count(*) as totalnum from event_log

where timestamp >= F_TIMESTAMP (‘now’, ‘hour’, ‘-23’) and group by subtype_ha order by totalnum desc”

next

**3****.** Create a dataset for the admin activity, that includes log ins and log outs from the three FortiGate administrators.

set query “select subtype_admin count(*) as totalnum from event_log

where timestamp >= F_TIMESTAMP (‘now’, ‘hour’, ‘-23’) and group by subtype_

admin order by totalnum desc”

next

**4****.** Create a dataset for the configuration changes that the administrators did for the past 24 hours.

set query “select subtype_config count(*) as totalnum from event_log

where timestamp >= F_TIMESTAMP (‘now’, ‘hour’, ‘-23’) and group by subtype_

config order by totalnum desc”

end

next


**C****r****ea****t****i****n****g charts for the datasets**

**1****.** Log in to the CLI.

**2****.** Enter the following to create a new chart:

config report chart edit ha.24h

set type table

set period last24h set dataset ha

set category event set favorite no

set style auto

set title “24 Hour HA Admin Activity”

end


**U****p****l****o****a****d****i****n****g the corporate images**

You need to upload the corporate images so that they appear on the report’s pages, as well as on the cover page. Uploading images is only available in the web-based manager.


**T****o upload corporate images**

**1****.** Go to **Lo****g & Report > Report > Local**.

**2****.** Select the Image icon and drag it to a place on the page.

**3****.** The Graphic Chooser window appears.

**4****.** Select Upload and then locate the image that you want to upload and upload the image.

The images are automatically uploaded and saved.

**5****.** Repeat step 4 until the other corporate images are uploaded.

**6****.** Select Cancel to close the Graphic Chooser window and return to the page.

The images can then be placed as you like by reopening the Graphic Chooser as in step 2.


**A****dd****i****n****g a new report cover and page**

You need to add a new cover for the report, as well as a new page that will display the HA activity, admin activity and configuration changes.


**T****o add and customize a new report cover**

**1****.** Go to **Lo****g & Report > Report > Local**.

**2****.** Select **C****u****s****t****o****m****iz****e**.

**3****.** In **S****ec****t****i****on****s**, select the current default report section, and enter Report Cover in the field that appears; then press Enter to save the change.

**4****.** Remove all content from the Report Cover section, and select the image icon and drag it into the main portion of the cover page; select a cover page image and then select **O****K**.

**5****.** Select the font size you want, and drag the text icon into the area beneath the image to add a title or explanation for the cover page.

**6****.** Select **S****av****e** to save the new report cover.


**T****o add and customize a new page**

**1****.** Go to **Lo****g & Report > Report > Local**.

**2****.** Select **C****u****s****t****o****m****iz****e**.

**3****.** Select **S****ec****t****i****on****s**, and select **C****r****ea****t****e New** to add a new section to the report. Name it Report Content, and press **E****n****t****e****r**, and **O****K** to close the menu.

**4****.** At the bottom of the editing window is the **S****ec****t****i****o****n** selection, where each **S****ec****t****i****o****n** is represented by a box. Select the second box.

**5****.** Edit the content for the report as you like.

For a simpler report structure, make use of the ‘FortiGate UTM Security Analysis Report’ charts, which automatically format themselves and fill in all necessary information.

For more complex reports, add headings, default and custom charts, and explanatory text.

**6****.** Select **S****av****e** to save the new report content.

The report will automatically combine all sections. You can use headers and text to more clearly separate parts of the report, and all properly configured charts have titles built-in.
