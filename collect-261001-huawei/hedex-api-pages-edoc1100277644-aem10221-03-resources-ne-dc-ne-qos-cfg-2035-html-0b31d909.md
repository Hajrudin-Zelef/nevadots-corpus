After QPPB is configured, you can view QPPB information.
Context
You can run the display commands in any view to check QPPB running information. 
Procedure
- Run the display qppb local-policy configuration policy-name command to check the configuration of a specific QPPB local policy.
- Run the display qppb local-policy statistics interface interface-type interface-number [ qos-local-id qos-local-id ] { inbound | outbound } command to check the statistics about a specific QPPB local policy.
- Run the display ip routing-table { ip-address [ [ mask ] | [ mask-length ] ] |  | host-name host-name } verbose command to check the QoS local ID carried in a specified route.