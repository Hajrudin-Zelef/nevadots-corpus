# What's the differences between sticky-config MAC & sticky MAC on Huawei SW？

Question asked by Gilbert Wong | Score: 4 | Tags: mac-address, port-security, huawei

## Question

In Port-security configuration，

```
`port-security mac-address sticky-config xxxx-xxxx-xxx vlan x
port-security mac-address sticky xxxx-xxxx-xxx vlan x
`

1.All of them do not disappear after SW restarting. What's differences between the two commands？

2.Are sticky-config MAC & sticky MAC called security static MAC？

## Answer 1 (ACCEPTED)

By Zac67 | Score: 1

From the support articles I found

- port-security mac-address sticky

- port-security mac-address sticky-config

my understanding is that `sticky` is a basically dynamic mode where you can add MAC addresses manually as well. `sticky-config` is a static mode where there's no learning and all addresses need to be configured.

Both commands need to survive a reboot to make sense.