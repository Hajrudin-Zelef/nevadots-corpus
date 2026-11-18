Path Schema
The Meraki API resources are organized by scope, and then by the product and its related service groups.
baseURI/scope/:id/product/serviceGroup/:id/service/:id
Base URI
In most parts of the world, every API operation will begin with the following URL to the Meraki cloud. 
https://api.meraki.com/api/v1
For organizations hosted in Canada, China, India and United Stated FedRAMP please refer to their respective API base URI mentioned here.
Scopes
The API mirrors the structure of the Meraki Dashboard.
 Organizations consist of Networks, which then contain Devices. 
Service Groups
Each Meraki product and scope will then have relative services which will be grouped together
/appliance/firewall/
Examples
Resource
/networks/L_646829496481100388/appliance/firewall/l3FirewallRules
URL
https://api.meraki.com/api/v1/networks/L_646829496481100388/appliance/firewall/l3FirewallRules
cURL