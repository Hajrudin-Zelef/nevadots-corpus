Huawei VRP Syntax
The syntax for this plugin and an audit are as follows:
<custom_item>
description: "Huawei: Set super password"
info: "Set super password for management levels of 3-15."
solution: "In system view, run the following command to configure super
password super password level <level> encryption-type cipher
<password>"
reference: "SANS-CSC|10,PCI|2.2.4,COBIT5|BAI10.01,800-53|CM-2"
expect: "^super password level ([3-9]|1[0-5]) cipher"
</custom_item>