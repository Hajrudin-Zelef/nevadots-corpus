DHCP options
DHCP options
When adding a DHCP server, you can include DHCP options. The DHCP options are BOOTP vendor information fields that provide additional vendor-independent configuration parameters to manage the DHCP server. For example, you might need to configure a FortiGate DHCP server that gives out a separate option as well as an IP address, such as an environment that needs to support PXE boot with Windows images. The Option code is specific to the application. The documentation for the application indicates the values to use. The Option code is a value between 1 and 255.
For detailed information about DHCP options, see RFC 2132, DHCP Options and BOOTP Vendor Extensions.
The FortiOS DHCP server supports up to a maximum of 30 options per DHCP server. These optional fields can be set in either the GUI or CLI.
|  | DHCP server options are not available in transparent mode. | 
The DHCP options include: