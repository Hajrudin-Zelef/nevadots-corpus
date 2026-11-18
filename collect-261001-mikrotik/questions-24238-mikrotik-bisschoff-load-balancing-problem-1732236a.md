I'm using bisschoff load-balancing .its really good and works fine.but it has a problem. The problem is when a adsl line don't have access to internet , it don't disable automatically and vice versa. For that i statically disable and enable lines. Is there any solution about this ? Thanks for your help.
1 Answer 1
You can use the routerOS netwatch tool to monitor each ADSL's gateway. When netwatch detects a failure (=no more ping), you can execute a script to disable some rules, and another to re-enable them when the ping gets back.
- 
        Thanks.Could you help me more and write script and netwatch ? Note : I have 4 PPPOE clients in mikrotik .mrb– mrb2015-11-10 13:04:43 +00:00Commented Nov 10, 2015 at 13:04