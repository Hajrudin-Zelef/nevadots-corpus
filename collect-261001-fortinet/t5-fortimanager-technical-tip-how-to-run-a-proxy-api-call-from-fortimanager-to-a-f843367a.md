Technical Tip: How to run a proxy API call from FortiManager to a managed FortiGate
Description
This article describes how to run a proxy API call from FortiManager to a managed FortiGate to collect data.
Scope
FortiManager and FortiGate.
Solution
- FortiManager will send proxy API calls via url:"sys/proxy/json" to managed FortiGate API "resource": "<FortiGate API Call>" via the FGFM tunnel that established between the FortiManager and the managed FortiGate.
Note:
For a complete list of FortiGate API calls, refer to Fortinet Development Network (FNDN):
{
     "id" : "1",
     "method": "exec",
     "params": [ 
         {
             "url": "sys/proxy/json",
             "data": {
                   "target": [ "device/<device-name>" ],
                   "action": "get",
                   "resource": "<FortiGate API Call>"
             } 
         } 
     ],
     "session" : "<session-id>"
}
- Below are two sample usages of proxy calls from FortiManager to a managed FortiGate:
- Get FortiGate Firewall Address:
{
     "id" : "1",
     "method": "exec",
     "params": [ 
         {
             "url": "sys/proxy/json",
             "data": {
                   "target": [ "device/lab-fgt2" ],
                   "action": "get",
                   "resource": "/api/v2/cmdb/firewall/address"
             } 
         } 
     ],
     "session" : "{{session-id}}"
}
- Get FortiGate License Info:
{
     "id" : "1",
     "method": "exec",
     "params": [ 
         {
             "url": "sys/proxy/json",
             "data": {
                   "target": [ "device/lab-fgt2" ],
                   "action": "get",
                   "resource": "/api/v2/monitor/license/status"
             } 
         } 
     ],
     "session" : "{{session-id}}"
}
Related articles: