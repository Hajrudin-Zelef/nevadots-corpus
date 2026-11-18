The evpn ext-format enable command enables the new SD-WAN tunnel encapsulation format.
The undo evpn ext-format enable command restores the default configuration.
By default, the new SD-WAN tunnel encapsulation format is not used.
Format
evpn ext-format enable
undo evpn ext-format enable
Parameters
None
Views
System view
Default Level
2: Configuration level
Usage Guidelines
In SD-WAN scenarios, you can run this command to enable the new SD-WAN tunnel encapsulation format, in which there is R=0 in the GRE encapsulation header.
Example
# Enable the new SD-WAN tunnel encapsulation format.