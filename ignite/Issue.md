# IgniteIssue
1. 2个节点同时加入导致Merge异常问题
Answer: https://issues.apache.org/jira/browse/IGNITE-7366
Issue:  https://mail.google.com/mail/u/0/#inbox/160fcd2430d61d5f

2. Ignite IPV4 and IPV6
```
I see that you use both ipv4 and ipv6 stacks, I think it can somehow affect this behavior, so, I would recommend enabling -Djava.net.preferIPv4Stack=true 
```

3.
