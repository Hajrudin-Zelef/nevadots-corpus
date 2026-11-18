# 
       Dual WAN Failover with same gateway issue 

    
    Hi everyone,

      I was wondering if it is possible to set up a dual WAN failover where both WANs have the same gateway.

This could be the case if you have two modems/routers and two connection lines from the same ISP (or they could be different for that matter), and you put an OPNsense device behind them to better manage and protect your network.
    

      Is it possible in OPnsense? Could there be some problems?

Thanks
    

If you have two modems with the same gateway from the same ISP it will not work since you would need two routing tables.

https://github.com/opnsense/core/issues/6576

Comment removed by moderator

...and? Could you tell me why please?