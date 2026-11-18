**Description** 

This article describes the configuration to apply to the web-proxy to allow the web-proxy to follow the SD-WAN rules. The parameter 'interface-select-method' is supported on FortiOS 7.6 onward.**Scope**

FortiGate v7.6**Solution**

The default **vrf** value is -1, meaning it does not accept any SD-WAN interface selection. The VRF ID needs to be specified in the correct order to follow the SD-WAN rules:

```
config web-proxy explicit
    set interface-select-method sdwan
    set vrf -1
end
```

By default, FortiGate uses VRF 0. To obtain the VRF, run the following command:

`get router info routing-table details 8.8.8.8`

The output should look like 'VRF=X', where x is the ID of the VRF.

`Routing table for VRF=0`

Use the ID from this output ('0', in this case) for the configuration number in the web-proxy.**Related document:**