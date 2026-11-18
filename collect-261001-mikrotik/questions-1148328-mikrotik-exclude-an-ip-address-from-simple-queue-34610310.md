I limited internet bandwidth of the range 192.168.100.0/24 using a simple queue in Mikrotik. Now I want to exclude some IP addresses (e.g 192.168.100.20) from this range to have unlimited internet bandwidth. 
How can I do that?
Create two simple queue: One for 192.168.100.0/24 which is limited and another for 192.168.100.20 which is unlimited. The simple queue for specific IP address should be upper than other queue. I could be done by Drag & Drop the queue in Winbox. Also you can create this queues and keep their priority using following commands:
/queue simple
add name=ExcludedIP target=192.168.100.20/32
add max-limit=2M/2M name=RangeLimit target=192.168.100.0/24
Packet Mark should be use to simplify managment.
                
                You can use packet mark or define multiple target address on queue.
192.168.100.0/24 and 192.168.100.20?