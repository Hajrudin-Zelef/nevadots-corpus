# How to connect to use two internet connections on Mikrotik Router?

*Source : https://networkengineering.stackexchange.com/questions/26556/how-to-connect-to-use-two-internet-connections-on-mikrotik-router | Site : networkengineering | Score : 2*

I have two internet links coming to my Mikrotik router. I want to push the certain type of traffic from one internet line and rest from the other.
for e.g. I have the broadband line from which I wish to push the youtube and shopping website traffic whereas the rest of the traffic should go through the other internet leased line link.

What possible configuration should be done in the Mikrotik router to do this? (It is load balancing but of a different type)

---

## Reponse — score 1

You should act like this:

- Create a Mangle rules that mark all Youtube packets with a RoutingMark

- Create a static route for this traffic on the right gateway. (Be sure to select the right RoutingMark).

Check this link