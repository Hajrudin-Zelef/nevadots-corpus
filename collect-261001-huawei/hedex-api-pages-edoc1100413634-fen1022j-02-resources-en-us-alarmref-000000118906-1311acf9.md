The connection has entered the down state. (ConnectionID=[ConnectionID], DestSite=[DestSite], SrcSite=[SrcSite], DestTNP=[DestTNP], SrcTNP=[SrcTNP],DownReason=[DownReason])
The EVPN connection went Down.
| Alarm ID | OID | Alarm Severity | Alarm Type | 
|---|---|---|---|
| 15795185 | 1.3.6.1.4.1.2011.5.25.241.6.3.2 | Major | Quality of service alarm | 
| Name | Meaning | 
|---|---|
| ConnectionID | Connection ID. | 
| DestSite | Name of the destination site. | 
| SrcSite | Name of the source site. | 
| DestTNP | Destination TNP. | 
| SrcTNP | Source TNP. | 
| DownReasonID | Cause ID. | 
Run the display ipsec p2mp-policy command to check the DH groups used by the key exchange algorithm in the IPSec P2MP security policies of the local and peer devices. If the DH groups of the two devices are different and one device uses the strict mode, the DH groups cannot match. Configure the same DH group or disable the strict mode.