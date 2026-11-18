I am trying to debug some ssl-vpn connection stuff. If I run "diag debug application sslvpn -1" it generates a lot of debug lines. Downloading the output and filtering through it to find what I need is not fun.


Is there a way to filter this by the source IP of the remote VPN client? Or by some sort of VPN session ID? Or something so that I can focus on troubleshooting a single user without having to wade through all the other connection data?