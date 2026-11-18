I got two RB951G-2HnD and two RBSXTG-5HPnD-HGr2.
  (clientsOfLan1) ))> RB1 <---> SXT1 ))> <(( SXT2 <---> RB2 <(( (clientsOfLan2)
I need to 'connect' two LAN's around 951.
I can't figure out how to 'bridge' those two networks that clientsOfLan1 could see clientsOfLan2 on L2 layer.
RB1/2 is in ap bridge
SXT1 is station pseudobridge
SXT2 is ap bridge
I am quite lost. Can anyone suggest how to achieve this? I need same L2 (ethernet) network on both sides best possible latency and bandwidth. I want use/setup SXT1 and SXT2 to simulate a 'wire' that I connect between those two routerboards.