Rest api usage
Hi,
I am setting up a new 30E firewall for a small office and for once I had some time on my hands so I thought I would play around a little bit with the rest API for learning purposes since it would help out with managing other firewalls.
So I read through the reference guide http://docs.fortinet.com/d/fortiweb-5.5-restful-api-reference which seems pretty straight forward. However I get stuck right from the bat.
testing out the initial example: curl -H "Authorization: YWRtaW46" -k "https://172.22.10.74:90/api/v1.0/System/Network/StaticRoute"
But I get no response.
The firewall arrived with fortios v5.4.1 which I think has api v2 so I tried changing the url accordingly but with no difference
I cannot find anywhere to verify which api version my firewall is using, also I am lacking information in the reference guide on weather I have to manually enable the api or not.
I'm sure I'm just missing something, can someone here see any obvious signs?