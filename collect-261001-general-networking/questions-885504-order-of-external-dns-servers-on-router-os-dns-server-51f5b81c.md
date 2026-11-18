We have DNS server on our Mikrotik (RouterOS 6.36).
[admin@xxx] /ip dns> print 
                servers: 10.0.10.3,8.8.8.8
        dynamic-servers: 
  allow-remote-requests: yes
    max-udp-packet-size: 4096
   query-server-timeout: 2s
    query-total-timeout: 10s
             cache-size: 10240KiB
          cache-max-ttl: 1w
             cache-used: 243KiB
[admin@xxx] /ip dns> 
There is list of DNS servers where it should ask. I would expect that it asks first 10.0.10.3 if the record is not found than ask 8.8.8.8?
The problem is, that 10.0.10.3 is our internal DNS server with some local addresses. These are not available on 8.8.8.8
When I have both server listed the nslookup for the address fails. When I remove the google one (8.8.8.8) and leave only our internal it works.
Why is the order of the servers not respected on RouterOS ?
I tried to make the order 10.0.10.3,8.8.8.8 or 8.8.8.8, 10.0.10.3 , it doesn't matter.
Any ideas? Thanks